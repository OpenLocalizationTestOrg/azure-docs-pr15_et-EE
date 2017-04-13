<properties 
    pageTitle="Kasutamise järjekorra salvestusruumi Ruby kaudu | Microsoft Azure'i" 
    description="Saate teada, kuidas kasutada teenust Azure järjekorda loomine ja kustutamine järjekordade ja lisada, leida ja sõnumite kustutamine. Näidised Ruby kirjutada." 
    services="storage" 
    documentationCenter="ruby" 
    authors="robinsh" 
    manager="carmonm" 
    editor=""/>

<tags 
    ms.service="storage" 
    ms.workload="storage" 
    ms.tgt_pltfrm="na" 
    ms.devlang="ruby" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="robinsh"/>


# <a name="how-to-use-queue-storage-from-ruby"></a>Kuidas kasutada järjekorda salvestusruumi Ruby kaudu

[AZURE.INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Ülevaade

Sellest juhendist näitab, kuidas teha levinud stsenaariumi, kasutades Microsoft Azure'i järjekorda salvestusteenus. Näidiste kirjutada Ruby Azure'i API-ga.
Stsenaariumid, mis hõlmab hulka **lisamise**, **piilumist**, **otsimine**ja **kustutamine** järjekorda sõnumeid, samuti **loomine ja kustutamine järjekorrad**.

[AZURE.INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-ruby-application"></a>Foneetiline rakenduse loomine

Foneetiline rakenduse loomine. Juhised leiate teemast [Ruby on Rails veebirakenduse on Azure VM](../virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md).

## <a name="configure-your-application-to-access-storage"></a>Rakenduse Access mäluruumi konfigureerimine

Azure'i salvestusruumi kasutamiseks peate alla laadida ja foneetiline Azure'i pakett, mis sisaldab kogumi mugavuse teekide puhul, kus suhelda salvestusruumi ülejäänud teenuseid kasutada.

### <a name="use-rubygems-to-obtain-the-package"></a>Kasutage RubyGems saada pakett

1. Kasutage käsurea liides, nt **PowerShell** (Windows), **terminalis** (Mac) või **Bash** (Unix).

2. Tippige käsuviiba aken installida gem ja sõltuvused "gem installi azure".

### <a name="import-the-package"></a>Paketi importimine

Kasutage oma lemmik tekstiredaktorit, lisage järgmine foneetiline fail, kuhu kavatsete kasutada salvestusruumi ülaosas:

    require "azure"

## <a name="setup-an-azure-storage-connection"></a>Mõne Azure Storage ühenduse häälestamine

Azure'i mooduli ei loe keskkonna muutujate **AZURE'I\_salvestusruumi\_konto** ja **AZURE'I\_salvestusruumi\_ACCESS_KEY** Azure salvestusruumi oma kontoga ühenduse loomiseks vajalikku teavet. Kui need keskkonna muutujad on pole määratud, peate määrama kontoteabe enne **Azure::QueueService** abil järgmine kood:

    Azure.config.storage_account_name = "<your azure storage account>"
    Azure.config.storage_access_key = "<your Azure storage access key>"

 
Klassikaline või ressursihaldur salvestusruumi konto Azure portaali saada need väärtused:

1. [Azure'i portaali](https://portal.azure.com)sisse logida.
2. Liikuge salvestusruumi konto, mida soovite kasutada.
3. Klõpsake paremal labale sätted **Kiirklahvide**.
4. Accessi klahvid tera, mis kuvatakse, näete kiirklahv 1 ja 2 võti. Saate kasutada ühte järgmistest. 
5. Klõpsake ikooni koopia võti lõikelauale kopeerida. 

Klassikaline salvestusruumi konto klassikaline Azure'i portaalis saada need väärtused:

1. [Klassikaline Azure portaali](https://manage.windowsazure.com)sisse logida.
2. Liikuge salvestusruumi konto, mida soovite kasutada.
3. Klõpsake nuppu **Halda KIIRKLAHVIDE** navigeerimispaani allosas.
4. Dialoogiboksi üles pop, kuvatakse salvestusruumikonto nimi, esmane võti ja teisene kiirklahv. Kiirklahv, saate kasutada esmase või teisese ühe. 
5. Klõpsake ikooni koopia võti lõikelauale kopeerida.

## <a name="how-to-create-a-queue"></a>Tehke järgmist: Järjekorra loomiseks

Järgmine kood tekitab **Azure::QueueService** objekti, mis võimaldab töötada järjekorrad.

    azure_queue_service = Azure::QueueService.new

**Create_queue()** meetodi abil saate luua järjekorda määratud nimega.

    begin
      azure_queue_service.create_queue("test-queue")
    rescue
      puts $!
    end

## <a name="how-to-insert-a-message-into-a-queue"></a>Kuidas: Lisada sõnumi järjekorda

Sõnumi järjekorda, saate lisada **create_message()** meetod uue sõnumi loomine ja lisamine järjekorda.

    azure_queue_service.create_message("test-queue", "test message")

## <a name="how-to-peek-at-the-next-message"></a>Juhised: Järgmise sõnumi Piilumine

Piilumine sõnumi ees järjekorda ilma eemaldamine kuhjuda helistades on **pisieelvaade\_messages()** meetod. Vaikimisi **pisieelvaade\_messages()** peeks ühele sõnumile. Saate määrata, kui palju sõnumid, mida soovite peek.

    result = azure_queue_service.peek_messages("test-queue",
      {:number_of_messages => 10})

## <a name="how-to-dequeue-the-next-message"></a>Kuidas: Dequeue järgmise sõnumi

Sõnumi eemaldamiseks järjekorda on kaks toimingut.

1. Kui helistate **loendi\_messages()**, kuvatakse järgmine teade vaikimisi järjekorda. Saate määrata, kui palju sõnumeid, mille soovite saada. Saadud sõnumite **loendi\_messages()** muutub nähtamatu mis tahes muu koodi lugemine sõnumeid selles järjekorras. Saate edastada nähtavuse ajalõpp sekundites parameetrina.

2. Sõnumi eemaldamine kuhjuda lõpetamiseks helistada ka **delete_message()**.

Kontrollimiseks, eemaldades sõnumi tagab, et kui teie kood ei töödelda sõnumile või riistvara tõrke tõttu, muus eksemplaris oma koodi saate sama teade ja proovige uuesti. Kõnede kood **kustutamine\_protsessi** kohe pärast sõnumi töödeldud.

    messages = azure_queue_service.list_messages("test-queue", 30)
    azure_queue_service.delete_message("test-queue", 
      messages[0].id, messages[0].pop_receipt)

## <a name="how-to-change-the-contents-of-a-queued-message"></a>Tehke järgmist: Ootel sõnumi sisu muuta

Saate muuta sisu on sõnumi kohapealne järjekorda. Alljärgnev kood kasutab **update_message()** meetodit sõnumi värskendada. Meetodi tulemuseks korteeži, mis sisaldab sõnumi kuhjuda ja UTC kuupäev aja väärtus, mis tähistab kui sõnum on nähtav järjekorda pop saamist.

    message = azure_queue_service.list_messages("test-queue", 30)
    pop_receipt, time_next_visible = azure_queue_service.update_message(
      "test-queue", message.id, message.pop_receipt, "updated test message", 
      30)

## <a name="how-to-additional-options-for-dequeuing-messages"></a>Kuidas: Sõnumite Dequeuing lisasuvanditele

On kaks võimalust toomise järjekorda kaudu saate kohandada.

1. Saate sõnumi partii.

2. Saate määrata rohkem või vähem nähtamatus ajalõpp, rohkem või vähem aega täielikult töödelda iga sõnumi, mis võimaldab teie kood.

Järgmine kood näites kasutatakse funktsiooni **loendi\_messages()** meetod, et saada 15 sõnumite ühe kõne. Seejärel prinditakse ja iga sõnumi kustutamine. Seda määrab ka nähtamatus ajalõpp viis minutit iga sõnumi kohta.

    azure_queue_service.list_messages("test-queue", 300
      {:number_of_messages => 15}).each do |m|
      puts m.message_text
      azure_queue_service.delete_message("test-queue", m.id, m.pop_receipt)
    end

## <a name="how-to-get-the-queue-length"></a>Kuidas: Saada järjekorra pikkus

Saate sõnumite hinnang järjekorda. Funktsiooni **hankimine\_järjekorda\_metadata()** meetod palub järjekorda teenuse ligikaudne sõnumi count ja metaandmeid kuhjuda tagastamiseks.

    message_count, metadata = azure_queue_service.get_queue_metadata(
      "test-queue")

## <a name="how-to-delete-a-queue"></a>Kuidas: Kustutamine järjekord

Järjekorda ja kõik selles sisalduvad sõnumid kustutamiseks helistada selle **kustutamine\_queue()** meetod objektil järjekorda.

    azure_queue_service.delete_queue("test-queue")

## <a name="next-steps"></a>Järgmised sammud

Nüüd, kui olete õppinud järjekorda salvestusruumi põhitõdesid, järgige neid linke salvestusruumi keerukamate tööülesannete kohta.

- Külastage [Azure Storage meeskonna ajaveeb](http://blogs.msdn.com/b/windowsazurestorage/)
- Külastage [Azure'i Tarkvaraarenduskomplektist, mis](https://github.com/WindowsAzure/azure-sdk-for-ruby) hoidla github

Käesolevas artiklis ja Azure teenuse siini järjekorrad [kasutamise teenuse siini järjekorrad](/develop/ruby/how-to-guides/service-bus-queues/) artiklis kirjeldatud arutada Azure'i järjekorda teenuse võrdlus leiate [Azure'i järjekorrad ja teenuse siini järjekorrad - võrreldes ja Contrasted](../service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted.md)
 
