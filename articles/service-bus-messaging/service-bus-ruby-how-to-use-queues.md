<properties
    pageTitle="Kuidas kasutada teenuse siini järjekorrad Ruby | Microsoft Azure'i"
    description="Saate teada, kuidas kasutada Azure teenuse siini järjekorrad. Koodi näidised Ruby kirjutada."
    services="service-bus"
    documentationCenter="ruby"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="ruby"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="sethm"/>

# <a name="how-to-use-service-bus-queues"></a>Kuidas kasutada teenuse siini järjekorrad

[AZURE.INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Sellest juhendist kirjeldatakse, kuidas kasutada teenuse siini järjekorrad. Näidiste Ruby kirjutada ja kasutada Azure gem. Stsenaariumid, mis hõlmab näiteks **loomise järjekorrad, sõnumite saatmine ja vastuvõtmine**ja **järjekorrad kustutamine**. Teenuse siini järjekorrad kohta leiate lisateavet jaotisest [Järgmised toimingud](#next-steps) .

## <a name="what-are-service-bus-queues"></a>Mis on teenuse siini järjekorrad?

Teenuse siini järjekorrad toetavad *vahendatud sõnumside* side mudel. Koos järjekordade komponendid jaotatud rakendus pole otse omavahel suhelda; selle asemel nad vahetada sõnumeid järjekorda, mis on vahendajaks kaudu. Sõnumi tootja (saatja) käed sõnumi järjekorda välja ja seejärel jätkub töötlemine.
Asünkroonselt, sõnumi tarbija (vastuvõtja) tõmbab sõnumi kuhjuda ja töötleb. Tootja ei pea ootama vastuse tarbija jätkamiseks töötlemine ja sõnumite saatmine. Järjekorrad pakkuda **esimese, esimese välja FIFO** kohaletoimetamise ühe või mitme konkureerivate tarbijatele. Seega sõnumid on tavaliselt vastu ja töödelda vastuvõtjad järjestuses, kus need olid varem lisatud kuhjuda ja iga sõnumi on saanud ja vaid ühe sõnumi tarbija töödelda.

![QueueConcepts](./media/service-bus-ruby-how-to-use-queues/sb-queues-08.png)

Teenuse siini järjekorrad on üldotstarbeline tehnoloogia, mida saab kasutada erinevaid stsenaariumeid lähemalt.

-   Suhtlemine on [mitmekihilise Azure rakenduse](service-bus-dotnet-multi-tier-app-using-service-bus-queues.md)web ja töötaja rollid.
-   Suhtlemine kohapealse rakendused ja Azure majutatud rakendused [hübriid lahendus](service-bus-dotnet-hybrid-app-using-service-bus-relay.md).
-   Suhtlemine jaotatud rakendus töötab kohapealse eri ettevõtted või ettevõttes osakondade komponendid.

Järjekorrad abil saate võimaldavad mastaapimiseks välja parem rakenduste ja lubada rohkem paindlikkust oma arhitektuur.

## <a name="create-a-namespace"></a>Nimeruumi loomine

Azure'i teenus siini järjekorrad kasutamine alustamiseks peate esmalt looma nimeruumi. Nimeruumi pakub ulatuse container teenuse siini ressursse rakenduse tegelemiseks. Kuna Azure portaali ei loo nimeruumi ACS ühendusega, peate looma nimeruumi käsurea kasutajaliidese kaudu.

Nimeruumi loomiseks tehke järgmist.

1. Avage mõni Azure PowerShelli konsooli.

2. Tippige järgmine käsk teenuse siini nimeruum loomiseks. Sisestage oma nimeruum väärtus ja määrata piirkonna rakenduse.

    ```
    New-AzureSBNamespace -Name 'yourexamplenamespace' -Location 'West US' -NamespaceType 'Messaging' -CreateACSNamespace $true

    ![Create Namespace](./media/service-bus-ruby-how-to-use-queues/showcmdcreate.png)
    ```

## <a name="obtain-management-credentials-for-the-namespace"></a>Nimeruumi halduse mandaat hankimine

Selleks, et haldamise toiminguid, näiteks järjekorda luua uue nimeruumi, peate hankima nimeruumi identimisteabe haldamine.

Käivitasite Azure'i teenus siini nimeruumi loomiseks PowerShelli cmdlet kuvab abil saate hallata nimeruumi võti. Kopeerige **DefaultKey** väärtus. Kasutage seda väärtust koodi hiljem sisse selles õpetuses.

![Kopeeri võti](./media/service-bus-ruby-how-to-use-queues/defaultkey.png)

> [AZURE.NOTE] See võti leiate ka siis, kui [Azure portaali](https://portal.azure.com/) sisse logida ja liikuge oma teenuse siini nimeruumi ühendusteabe.

## <a name="create-a-ruby-application"></a>Foneetiline rakenduse loomine

Foneetiline rakenduse loomine. Juhised leiate teemast [loomine Ruby rakenduse Azure](/develop/ruby/tutorials/web-app-with-linux-vm/).

## <a name="configure-your-application-to-use-service-bus"></a>Rakenduse kasutamiseks teenuse siini konfigureerimine

Azure'i teenus siini kasutamiseks alla laadida ja Ruby Azure'i pakett, mis sisaldab kogumi mugavuse teekide puhul, kus suhelda salvestusruumi ülejäänud teenuseid kasutada.

### <a name="use-rubygems-to-obtain-the-package"></a>Kasutage RubyGems saada pakett

1. Kasutage käsurea liides, nt **PowerShell** (Windows), **terminalis** (Mac) või **Bash** (Unix).

2. Tippige käsuviiba aken installida gem ja sõltuvused "gem installi azure".

### <a name="import-the-package"></a>Paketi importimine

Kui kavatsete kasutada salvestusruumi foneetiline faili ülaosas abil oma lemmik tekstiredaktoris, järgmine lisamiseks järgmist.

```
require "azure"
```

## <a name="set-up-an-azure-service-bus-connection"></a>Azure'i teenus siini-ühenduse häälestamine

Azure'i mooduli loeb keskkonna muutujate **AZURE'I\_SERVICEBUS\_NIMERUUM** ja **AZURE'I\_SERVICEBUS\_ACCESS_KEY** ühenduse loomiseks oma teenuse siini nimeruum vajalikku teavet. Kui need keskkonna muutujad on pole määratud, peate määrama nimeruum teabe enne **Azure::ServiceBusService** abil järgmine kood:

```
Azure.config.sb_namespace = "<your azure service bus namespace>"
Azure.config.sb_access_key = "<your azure service bus access key>"
```

Seadke nimeruum lõite, selle asemel, et kogu URL-i väärtust. Näiteks kasutada **"yourexamplenamespace"**, mitte "yourexamplenamespace.servicebus.windows.net".

## <a name="how-to-create-a-queue"></a>Kuidas luua järjekord

Objekti **Azure::ServiceBusService** võimaldab töötada järjekorrad. Järjekorra loomiseks kasutage **create_queue()** meetodit. Järgmises näites luuakse järjekorda või prindib välja vigu.

```
azure_service_bus_service = Azure::ServiceBusService.new
begin
  queue = azure_service_bus_service.create_queue("test-queue")
rescue
  puts $!
end
```

Samuti saab edastada **Azure::ServiceBus::Queue** objekti lisavõimalused, mis võimaldab teil järjekorda vaikesätted, nt sõnumi aega live või mahu ülempiir järjekorda. Järgmises näites kujutatakse 5GB ja aega kuni 1 minut live maksimaalne järjekorda suuruse määramine:

```
queue = Azure::ServiceBus::Queue.new("test-queue")
queue.max_size_in_megabytes = 5120
queue.default_message_time_to_live = "PT1M"

queue = azure_service_bus_service.create_queue(queue)
```

## <a name="how-to-send-messages-to-a-queue"></a>Kuidas sõnumeid saata järjekorda

Sõnumi saatmiseks teenuse siini järjekorda, rakenduse kõned on **saata\_järjekorda\_protsessi** **Azure::ServiceBusService** objekti meetod. Sõnumid saadetakse (ja vastuvõetud:) teenuse siini järjekorrad on **Azure::ServiceBus::BrokeredMessage** objektid ja on standardatribuudid kogum (nt **sildi** ja **aja\_abil\_live**), hoidke kohandatud rakenduse teatud atribuutide kasutatava sõnastiku ja keha suvalise rakenduse andmeid. Rakenduse saate seada sõnumi kehasse sooritab stringiväärtus sõnumile ja kõik nõutud standard atribuudid on täidetud vaikeväärtustega.

Järgmises näites näitab, kuidas test sõnumi saatmine nimega "testi järjekord" abil järjekorda **saata\_järjekorda\_protsessi**:

```
message = Azure::ServiceBus::BrokeredMessage.new("test queue message")
message.correlation_id = "test-correlation-id"
azure_service_bus_service.send_queue_message("test-queue", message)
```

Teenuse siini järjekorrad toetamiseks [Premium taseme](service-bus-premium-messaging.md)sõnumi maksimaalse mahu 256 KB [Standard taseme](service-bus-premium-messaging.md) ja 1 MB. Päisest, mis sisaldab standard- ja kohandatud rakenduse atribuutide, võib olla maksimaalse mahu 64 KB. Järjekorda sõnumite arv pole piiratud, kuid ei ülempiir suuruse järjekorda sõnumite kohta. Selles järjekorras maht on määratletud loomise ajal, 5 GB ülempiir.

## <a name="how-to-receive-messages-from-a-queue"></a>Kuidas sõnumeid vastu järjekord

Saadetud meilisõnumid järjekorda kasutades selle **vastu\_järjekorda\_protsessi** **Azure::ServiceBusService** objekti meetod. Vaikimisi sõnumeid lugeda ja lukustatud ilma mida kustutatakse järjekorda. Siiski saate kustutada sõnumid järjekorda nagu neid lugeda, seades selle **: peek_lock** suvand **väär (FALSE)**.

Vaikekäitumise teeb lugemis- ja kahe etapiga toiming, mis muudab toetamiseks rakendused, mistõttu puuduvad sõnumite kustutamine. Kui teenus siini saab taotluse, leiab see järgmine sõnum olla consumed, lukustab vältimaks teiste tarbijate seda vastu, ja tagastab rakendus. Pärast rakenduse lõppemist sõnumi töötlemine (või talletab selle tingimata tulevaste töötlemiseks), lõpetab vastuvõtu protsessi teise etapi helistades **kustutamine\_järjekorda\_protsessi** meetod ja esitada parameetrina soovitud sõnum. Funktsiooni **kustutamine\_järjekorda\_protsessi** meetod on sõnumi tarbitud ja eemaldamine järjekorda.

Kui soovitud **: peek\_Lukusta** parameeter on seatud väärtusele **väär**, lugemispaani ja sõnumi kustutamine muutub lihtsaim mudel ja sobib kõige paremini stsenaariumid, kus saate rakenduse luba töötlemise sõnumi tõrke korral. Mõistmaks, kaaluge stsenaarium, kus tarbija probleemide vastuvõtu taotlus ja siis kaob enne selle töötlemiseks. Kuna teenuse siini on märgitud sõnumid tarbitud, kui rakenduse taaskäivitamist ja algab tarbimine sõnumite uuesti, vastamata see sõnum, mida on kasutatud enne krahhi.

Järgmises näites näitab, kuidas saada ja töödelda sõnumeid, kasutades **vastu\_järjekorda\_protsessi**. Näide esmalt saab ja kustutab sõnumi, kasutades **: peek\_Lukusta** väärtuseks **false**, siis saab teise sõnumi ja seejärel kustutab sõnumi abil **kustutamine\_järjekorda\_protsessi**:

```
message = azure_service_bus_service.receive_queue_message("test-queue",
  { :peek_lock => false })
message = azure_service_bus_service.receive_queue_message("test-queue")
azure_service_bus_service.delete_queue_message(message)
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Rakendus jookseb ja loetavad sõnumite reageerimine

Teenuse siini pakub funktsiooni abil saate taastada nõtkelt kaudu oma sõnumi töötlemine probleeme või tõrkeid. Kui vastuvõtja rakendus ei saa sõnumit töödelda mingil põhjusel, siis seda saate helistada soovitud **avada\_järjekorda\_protsessi** **Azure::ServiceBusService** objekti meetod. See põhjustab teenuse siini sees kuhjuda sõnumi avamiseks ja kättesaadavaks saanud uuesti, kas sama nõudvate rakenduse või mõni muu nõudvate rakendus.

Olemas on ka seotud lukustatud jooksul kuhjuda sõnumi ajalõpp ja kui taotlus ei töödelda enne sõnumi lock ajalõpu aegumise (näiteks siis, kui rakendus jookseb), siis teenuse avab sõnumi automaatselt ja teeb selle kättesaadavaks uuesti vastu võtta.

Juhuks, kui rakendus jookseb pärast töötlemist sõnumi, kuid enne selle **kustutamine\_järjekorda\_protsessi** meetodit nimetatakse, siis sõnum on taastarnimiseni rakendusse taaskäivitamisel. Seda nimetatakse sageli **Vähemalt ühe korra töötlemine**; mis on iga sõnumi töötlemise vähemalt ühe korra, kuid teatud juhtudel võib taastarnimiseni sama sõnumi. Kui seda stsenaariumi ei luba dubleeritud töötlemine, siis rakenduste arendajatele peaks lisada täiendavad loogika taotlusega sõnumite kohaletoimetamise käsitlema. See on sageli saavutada, kasutades selle **sõnumi\_id** atribuut, mis jääb samaks saatmiskatsete üle.

## <a name="next-steps"></a>Järgmised sammud

Nüüd, kui olete õppinud põhitõdesid teenuse siini järjekorda, järgige neid linke lisateabe.

-   [Järjekorrad, teemasid ja tellimuste](service-bus-queues-topics-subscriptions.md)ülevaade.
-   Külastage [Azure'i Tarkvaraarenduskomplektist, mis](https://github.com/Azure/azure-sdk-for-ruby) hoidla github.

Selles artiklis ja Azure järjekorrad [kasutamise järjekorda salvestusruumi kaudu Ruby](../storage/storage-ruby-how-to-use-queue-storage.md) artiklis kirjeldatud Azure'i teenus siini järjekorrad võrdlus leiate [Azure'i järjekorrad ja Azure teenuse siini järjekorrad - võrreldes ja Contrasted](service-bus-azure-and-service-bus-queues-compared-contrasted.md)
 
