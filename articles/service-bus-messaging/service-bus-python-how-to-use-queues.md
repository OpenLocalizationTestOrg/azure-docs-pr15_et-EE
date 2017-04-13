<properties 
    pageTitle="Teenuse siini järjekorrad kasutamine Python | Microsoft Azure'i" 
    description="Saate teada, kuidas kasutada Azure teenuse siini järjekorrad Python kaudu." 
    services="service-bus" 
    documentationCenter="python" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="python" 
    ms.topic="article" 
    ms.date="09/21/2016" 
    ms.author="sethm;lmazuel"/>


# <a name="how-to-use-service-bus-queues"></a>Kuidas kasutada teenuse siini järjekorrad

[AZURE.INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Selles artiklis kirjeldatakse, kuidas kasutada teenuse siini järjekorrad. Näidiste Python kirjutada ja kasutada [Python Azure'i teenus siini paketi][]. Stsenaariumid, mis hõlmab näiteks **loomise järjekorrad, sõnumite saatmine ja vastuvõtmine**ja **järjekorrad kustutamine**.

[AZURE.INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

> [AZURE.NOTE] Python või [Python Azure'i teenus siini paketi][]installimiseks lugege teemat [Python installi juhend](../python-how-to-install.md).

## <a name="create-a-queue"></a>Järjekorra loomiseks

Objekti **ServiceBusService** võimaldab töötada järjekorrad. Lisage mis tahes Python faili, kus soovite programmiliselt juurde teenuse siini ülaservas järgmine kood:

```
from azure.servicebus import ServiceBusService, Message, Queue
```

Järgmine kood tekitab **ServiceBusService** objekti. Asendage `mynamespace`, `sharedaccesskeyname`, ja `sharedaccesskey` oma nimeruumi, ühiskasutusega juurdepääsu allkirja (SAS) klahv nime ja väärtuse.

```
bus_service = ServiceBusService(
    service_namespace='mynamespace',
    shared_access_key_name='sharedaccesskeyname',
    shared_access_key_value='sharedaccesskey')
```

Väärtused SAS võtme nimi ja väärtuse leiate [Azure'i klassikaline portaali][] ühenduseteavet või Visual Studio **Atribuudid** paanil valimisel teenuse siini nimeruumi Server Explorer (nagu on näidatud eelmises jaotises).

```
bus_service.create_queue('taskqueue')
```

**create_queue** toetab samuti veel suvandeid, mis lubavad teil Alista vaikesätted järjekorda näiteks sõnumi aega live (TTL) või mahu ülempiir järjekorda. Järgmises näites määrab maksimaalne järjekorda 5GB ja 15 minutit TTL väärtuse:

```
queue_options = Queue()
queue_options.max_size_in_megabytes = '5120'
queue_options.default_message_time_to_live = 'PT1M'

bus_service.create_queue('taskqueue', queue_options)
```

## <a name="send-messages-to-a-queue"></a>Sõnumite saatmiseks järjekord

Sõnumi saatmiseks teenuse siini järjekorda, kõnede rakendus on **saata\_järjekorda\_sõnumi** meetod **ServiceBusService** objekti.

Järgmises näites näitab, kuidas järjekorda nimega *taskqueue kasutamise* katse sõnumi saata **saata\_järjekorda\_sõnumi**:

```
msg = Message(b'Test Message')
bus_service.send_queue_message('taskqueue', msg)
```

Teenuse siini järjekorrad toetamiseks [Premium taseme](service-bus-premium-messaging.md)sõnumi maksimaalse mahu 256 KB [Standard taseme](service-bus-premium-messaging.md) ja 1 MB. Päisest, mis sisaldab standard- ja kohandatud rakenduse atribuutide, võib olla maksimaalse mahu 64 KB. Järjekorda sõnumite arv pole piiratud, kuid ei ülempiir suuruse järjekorda sõnumite kohta. Selles järjekorras maht on määratletud loomise ajal, 5 GB ülempiir. Kvootide kohta leiate lisateavet teemast [teenuse siini piirmäärasid][].

## <a name="receive-messages-from-a-queue"></a>Saadud sõnumite järjekord

Saadetud meilisõnumid järjekorda kasutades selle **vastu\_järjekorda\_sõnumi** meetod **ServiceBusService** objekti:

```
msg = bus_service.receive_queue_message('taskqueue', peek_lock=False)
print(msg.body)
```

Sõnumite kustutatakse järjekorda, nagu need on loetav, kui parameetri **peek\_Lukusta** väärtuseks **False**. Saate lugeda (peek) ja lukustamine sõnumi kustutamine kuhjuda parameetri ilma **pisieelvaade\_Lukusta** väärtuseks **True**.

Toimimist lugemise ja kustutage sõnum osana vastuvõtmise toiming on lihtsaim mudel ja sobib kõige paremini stsenaariumid, kus saate rakenduse luba töötlemise sõnumi tõrke korral. Mõistmaks, kaaluge stsenaarium, kus tarbija probleemide vastuvõtu taotlus ja siis kaob enne selle töötlemiseks. Kuna teenuse siini on märgitud sõnumid tarbitud, siis, kui rakenduse taaskäivitamist ja algab tarbimine sõnumite uuesti, see on vastamata sõnum, mis oli tarbitud enne krahhi.

Kui soovitud **pisieelvaade\_Lukusta** parameeter on seatud väärtusele **tõene**, on vastuvõtu muutub kahe etapi toiming, mis võimaldab tugi rakenduste, mistõttu puuduvad sõnumeid. Kui teenus siini saab taotluse, leiab see järgmine sõnum olla consumed, lukustab vältimaks teiste tarbijate seda vastu, ja tagastab rakendus. Pärast rakenduse lõppemist sõnumi töötlemine (või talletab selle tingimata tulevaste töötlemiseks), lõpetab vastuvõtu protsessi teise etapi helistades **sõnumi** objekti **kustutamine** meetod. **Kustutamine** meetod on nagu tarbitud sõnumi märkimine ja eemaldamine järjekorda.

```
msg = bus_service.receive_queue_message('taskqueue', peek_lock=True)
print(msg.body)

msg.delete()
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Rakendus jookseb ja loetavad sõnumite reageerimine

Teenuse siini pakub funktsiooni abil saate taastada nõtkelt kaudu oma sõnumi töötlemine probleeme või tõrkeid. Kui vastuvõtja rakenduse ei saa sõnumit töödelda mingil põhjusel, siis selle saate helistada **avada** meetod objektil **sõnum** . See põhjustab teenuse siini sees kuhjuda sõnumi avamiseks ja kättesaadavaks saanud uuesti, kas sama nõudvate rakenduse või mõni muu nõudvate rakendus.

Olemas on ka seotud lukustatud jooksul kuhjuda sõnumi ajalõpp ja kui taotlus ei töödelda enne sõnumi lock ajalõpu aegumise (nt juhul, kui rakendus jookseb), siis teenuse siini kuvatakse sõnumi automaatselt avada ja kättesaadavaks uuesti vastu võtta.

Juhuks, kui rakendus jookseb pärast sõnumi töötluse, kuid enne **kustutamine** meetodit nimetatakse, siis sõnum on olla taastarnimiseni rakendusse taaskäivitamisel. Seda nimetatakse sageli **Töötlemine on vähemalt üks kord**, st iga sõnumi töödeldakse vähemalt ühe korra, kuid teatud juhtudel võib taastarnimiseni sama sõnumit. Kui seda stsenaariumi ei luba dubleeritud töötlemine, siis rakenduste arendajatele peaks lisada täiendavad loogika taotlusega sõnumite kohaletoimetamise käsitlema. See on sageli saavutada, kasutades **MessageId** atribuut, mis samaks saatmiskatsete üle.

## <a name="next-steps"></a>Järgmised sammud

Nüüd, kui olete õppinud põhitõdesid teenuse siini järjekorda, järgige neid linke lisateabe.

-   Vt [järjekorrad, teemade, ja tellimused][].

[Azure'i klassikaline portaal]: https://manage.windowsazure.com
[Python Azure'i teenus siini pakett]: https://pypi.python.org/pypi/azure-servicebus  
[Järjekorrad, teemasid ja tellimused]: service-bus-queues-topics-subscriptions.md
[Teenuse siini kvoote]: service-bus-quotas.md
 
