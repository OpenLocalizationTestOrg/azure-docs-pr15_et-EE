<properties
    pageTitle="Kasutamise järjekorra salvestusruumi Python kaudu | Microsoft Azure'i"
    description="Saate teada, kuidas Python Azure'i järjekorda teenuse abil saate luua ja kustutada järjekordade ja lisada, leida ja sõnumite kustutamine."
    services="storage"
    documentationCenter="python"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="robinsh"/>

# <a name="how-to-use-queue-storage-from-python"></a>Kuidas kasutada järjekorda salvestusruumi Python kaudu

[AZURE.INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Ülevaade

Sellest juhendist näitab, kuidas teha levinud stsenaariumi, mis on Azure järjekorda salvestusteenus abil. Näidiste Python kirjutada ja kasutada [Microsoft Azure'i salvestusruumi SDK Python]. Stsenaariumid, mis hõlmab hulka **lisamise**, **piilumist**, **otsimine**ja **kustutamine** järjekorda sõnumeid, samuti **loomine ja kustutamine järjekorrad**. Järjekorrad kohta lisateabe saamiseks vaadake jaotise [järgmised toimingud].

[AZURE.INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="how-to-create-a-queue"></a>Tehke järgmist: Järjekorra loomiseks

Objekti **QueueService** võimaldab töötada järjekorrad. Järgmine kood tekitab **QueueService** objekti. Lisage mis tahes Python faili, kus soovite programmiliselt juurde Azure Storage ülaosas järgmine tekst:

    from azure.storage.queue import QueueService

Järgmine kood loob **QueueService** objekti abil salvestusruumi konto nimi ja konto võti. Asendage "minu konto" ja 'mykey' oma konto nimi ja võti.

    queue_service = QueueService(account_name='myaccount', account_key='mykey')

    queue_service.create_queue('taskqueue')


## <a name="how-to-insert-a-message-into-a-queue"></a>Kuidas: Lisada sõnumi järjekorda

Sõnumi järjekorda, saate lisada soovitud **sellele\_sõnumi** meetodi abil uue sõnumi loomiseks ja järjekorda lisada.

    queue_service.put_message('taskqueue', u'Hello World')


## <a name="how-to-peek-at-the-next-message"></a>Juhised: Järgmise sõnumi Piilumine

Piilumine sõnumi ees järjekorda ilma eemaldamine kuhjuda helistades on **pisieelvaade\_sõnumite** meetod. Vaikimisi **peek\_sõnumite** peeks ühele sõnumile.

    messages = queue_service.peek_messages('taskqueue')
    for message in messages:
        print(message.content)


## <a name="how-to-dequeue-messages"></a>Kuidas: Dequeue sõnumid

Teie kood eemaldab sõnumi järjekorda on kaks toimingut. Kui helistate **saada\_sõnumite**, kuvatakse järgmine teade vaikimisi järjekorda. Tagastatud sõnumi **saada\_sõnumite** muutub nähtamatu mis tahes muu koodi lugemine sõnumeid selles järjekorras. Vaikimisi see sõnum jääb nähtamatu 30 sekundit. Sõnumi eemaldamine kuhjuda lõpetamiseks peate ka kõne **kustutamine\_sõnumi**. Kontrollimiseks, eemaldades sõnumi tagab, et kui teie kood ei töödelda sõnumile või riistvara tõrke tõttu, muus eksemplaris oma koodi saate sama teade ja proovige uuesti. Kõnede kood **kustutamine\_sõnumi** kohe pärast sõnumi töödeldud.

    messages = queue_service.get_messages('taskqueue')
    for message in messages:
        print(message.content)
        queue_service.delete_message('taskqueue', message.id, message.pop_receipt)

On kaks võimalust toomise järjekorda kaudu saate kohandada.
Esmalt saate paketi sõnumite (kuni 32). Teiseks saate määrata rohkem või vähem nähtamatus ajalõpp, rohkem või vähem aega täielikult töödelda iga sõnumi, mis võimaldab teie kood. Järgmine kood näide kasutab funktsiooni **hankimine\_sõnumite** meetod, et saada 16 sõnumite ühe kõne. Töötleb iga meilisõnumi lisavõimalusi ja seejärel soovitud tsükkel jaoks. Seda määrab ka nähtamatus ajalõpp viis minutit iga sõnumi kohta.

    messages = queue_service.get_messages('taskqueue', num_messages=16, visibility_timeout=5*60)
    for message in messages:
        print(message.content)
        queue_service.delete_message('taskqueue', message.id, message.pop_receipt)      


## <a name="how-to-change-the-contents-of-a-queued-message"></a>Tehke järgmist: Ootel sõnumi sisu muuta

Saate muuta sisu on sõnumi kohapealne järjekorda. Kui sõnumi tähistab ülesande tööd, võib selle funktsiooni abil värskendada tööülesande oleku. Koodi all kasutab funktsiooni **värskendada\_sõnumi** meetod sõnumi värskendamiseks. Nähtavuse ajalõpu väärtuseks on 0, tähendab teade kuvatakse kohe ja sisu värskendatakse.

    messages = queue_service.get_messages('taskqueue')
    for message in messages:
        queue_service.update_message('taskqueue', message.id, message.pop_receipt, 0, u'Hello World Again')

## <a name="how-to-get-the-queue-length"></a>Kuidas: Saada järjekorra pikkus

Saate sõnumite hinnanguline järjekorda. Funktsiooni **hankimine\_järjekorda\_metaandmete** meetod palub järjekorda teenuse metaandmeid kuhjuda ja **approximate_message_count**tagastamiseks. Tulem on ainult ligikaudne, kuna sõnumite saate lisada või eemaldada pärast järjekorda teenuse vastaks teie taotlus.

    metadata = queue_service.get_queue_metadata('taskqueue')
    count = metadata.approximate_message_count

## <a name="how-to-delete-a-queue"></a>Kuidas: Kustutamine järjekord

Järjekorda ja kõik selles sisalduvad sõnumid kustutamiseks helistada selle **kustutamine\_järjekorda** meetod.

    queue_service.delete_queue('taskqueue')

## <a name="next-steps"></a>Järgmised sammud

Nüüd, kui olete õppinud järjekorda salvestusruumi põhitõdesid, järgige neid linke lisateabe.

- [Python Arenduskeskus](/develop/python/)
- [Azure'i salvestusruumi teenuste REST API-ga](http://msdn.microsoft.com/library/azure/dd179355)
- [Azure'i salvestusruumi meeskonna ajaveeb]
- [Microsoft Azure'i salvestusruumi SDK Python]

[Azure'i salvestusruumi meeskonna ajaveeb]: http://blogs.msdn.com/b/windowsazurestorage/
[Microsoft Azure'i salvestusruumi SDK Python]: https://github.com/Azure/azure-storage-python
