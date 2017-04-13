<properties
    pageTitle="Teenuse siini teemade (Ruby) kasutamise kohta | Microsoft Azure'i"
    description="Saate teada, kuidas kasutada Azure teenuse siini teemade ja tellimused. Foneetiline rakenduste koodinäiteid kirjutada."
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

# <a name="how-to-use-service-bus-topicssubscriptions"></a>Kuidas kasutada teenuse siini Teemad/tellimused

[AZURE.INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Selles artiklis kirjeldatakse, kuidas kasutada teenuse siini teemade ja tellimuste foneetiline rakendustest. Stsenaariumid, mis hõlmab kaasata **loomise teemade ja tellimuste loomise tellimuse filtrid, sõnumite saatmiseks** on teema, **sõnumeid tellimusest**ja **kustutamise teemade ja tellimused**. Teemade ja tellimuste kohta leiate lisateavet jaotisest [Järgmised toimingud](#next-steps) .

## <a name="service-bus-topics-and-subscriptions"></a>Teenuse siini teemade ja tellimused

Teenuse siini teemade ja tellimuste tugi on *avaldada/Telli* sõnumside side mudel. Teemade ja tellimuste kasutamisel komponendid jaotatud rakendus pole otse omavahel suhelda; need hoopis Exchange'i sõnumeid teema, mis on vahendajaks kaudu.

![TopicConcepts](./media/service-bus-ruby-how-to-use-topics-subscriptions/sb-topics-01.png)

Vastupidiselt teenuse siini järjekorrad, kus iga sõnumi töötleb ühe tarbija, teemade ja tellimuste sisestage vormile **üks-mitmele** teatise, kasutades avalda/tellimise mustri. On võimalik registreerida mitu tellimust teemale. Kui sõnum saadetakse teema, siis tehtud saadaval iga tellimuse töötlemine sõltumatult.

Teema: tellimuse sarnaneb virtuaalse järjekorda, mida saab teema saadetud sõnumite koopiad. Soovi korral saate registreerida filtri reeglid teema kohta tellimuse alusel, mis võimaldab teil filtri/piirata sõnumid, mida soovite teema on saanud, millised tellimused teema.

Teenuse siini teemade ja tellimuste võimaldavad mastaapimiseks töödelda suure hulga sõnumite üle suure hulga kasutajate ja rakendused.

## <a name="create-a-namespace"></a>Nimeruumi loomine

Azure'i teenus siini järjekorrad kasutamine alustamiseks peate esmalt looma nimeruumi. Nimeruumi pakub ulatuse container teenuse siini ressursse rakenduse tegelemiseks. Kuna [Azure portaali][] ei loo nimeruumi ACS ühendusega, peate looma nimeruumi käsurea kasutajaliidese kaudu.

Nimeruumi loomiseks tehke järgmist.

1. Azure'i PowerShelli konsooli akna avamine.

2. Tippige järgmine käsk nimeruumi loomiseks. Sisestage oma nimeruum väärtus ja määrata piirkonna rakenduse.

    ```
    New-AzureSBNamespace -Name 'yourexamplenamespace' -Location 'West US' -NamespaceType 'Messaging' -CreateACSNamespace $true
    ```

    ![Namespace loomine](./media/service-bus-ruby-how-to-use-topics-subscriptions/showcmdcreate.png)

## <a name="obtain-default-management-credentials-for-the-namespace"></a>Saada vaikimisi nimeruum halduse mandaat

Selleks, et haldamise toiminguid, näiteks järjekorda luua uue nimeruumi, peate hankima nimeruumi identimisteabe haldamine.

Käivitasite loomine teenuse siini nimeruumi PowerShelli cmdlet kuvab abil saate hallata nimeruumi võti. Kopeerige **DefaultKey** väärtus. Kasutage seda väärtust koodi hiljem sisse selles õpetuses.

![Kopeeri võti](./media/service-bus-ruby-how-to-use-topics-subscriptions/defaultkey.png)

> [AZURE.NOTE]
> See võti leiate ka siis, kui [Azure portaali][] sisse logida ja liikuge oma nimeruumi ühendusteabe.

## <a name="create-a-ruby-application"></a>Foneetiline rakenduse loomine

Juhised leiate teemast [loomine Ruby rakenduse Azure](../virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md).

## <a name="configure-your-application-to-use-service-bus"></a>Rakenduse kasutamine teenuse siini konfigureerimine

Teenuse siini kasutamiseks alla laadida ja Ruby Azure'i pakett, mis sisaldab kogumi mugavuse teekide puhul, kus suhelda salvestusruumi ülejäänud teenuseid kasutada.

### <a name="use-rubygems-to-obtain-the-package"></a>Kasutage RubyGems saada pakett

1. Kasutage käsurea liides, nt **PowerShell** (Windows), **terminalis** (Mac) või **Bash** (Unix).

2. Tippige käsuviiba aken installida gem ja sõltuvused "gem installi azure".

### <a name="import-the-package"></a>Paketi importimine

Kasutada oma lemmik tekstiredaktorit, lisage järgmine tekst, kus kavatsete kasutada salvestusruumi foneetiline faili ülaosas:

```
require "azure"
```

## <a name="set-up-a-service-bus-connection"></a>Teenuse siini-ühenduse häälestamine

Azure'i mooduli loeb keskkonna muutujate **AZURE'I\_SERVICEBUS\_NIMERUUM** ja **AZURE'I\_SERVICEBUS\_ACCESSI\_klahvi** oma nimeruum ühenduse loomiseks vajalikku teavet. Kui need keskkonna muutujad on pole määratud, peate määrama nimeruum teabe enne **Azure::ServiceBusService** abil järgmine kood:

```
Azure.config.sb_namespace = "<your azure service bus namespace>"
Azure.config.sb_access_key = "<your azure service bus access key>"
```

Seadke nimeruum loodud asemel kogu URL-i väärtust. Näiteks kasutada **"yourexamplenamespace"**, mitte "yourexamplenamespace.servicebus.windows.net".

## <a name="create-a-topic"></a>Teema loomine

Objekti **Azure::ServiceBusService** võimaldab töötada teemade. Järgmine kood tekitab **Azure::ServiceBusService** objekti. Teema loomiseks kasutage funktsiooni **loomine\_topic()** meetod. Järgmises näites luuakse teema või prindib tõrgete välja, kui on olemas.

```
azure_service_bus_service = Azure::ServiceBusService.new
begin
  topic = azure_service_bus_service.create_queue("test-topic")
rescue
  puts $!
end
```

Samuti saab edastada **Azure::ServiceBus::Topic** objekti lisasuvandid, mis võimaldavad teil alistada teema vaikesätteid näiteks sõnumi aja Live'i või mahu ülempiir järjekorda. Järgmises näites on kujutatud säte maksimaalne järjekorra suurus 5GB ja aega kuni 1 minut live:

```
topic = Azure::ServiceBus::Topic.new("test-topic")
topic.max_size_in_megabytes = 5120
topic.default_message_time_to_live = "PT1M"

topic = azure_service_bus_service.create_topic(topic)
```

## <a name="create-subscriptions"></a>Tellimuste loomine

Teema: tellimuste on loonud ka **Azure::ServiceBusService** objekti. Tellimused on nimega ja võib-olla valikuline filter, mis piirab kogumi sõnumid toimetada see tellimus virtuaalse järjekorda.

Tellimused on püsiv ja jätkub kuni ükskõik kumma nad või teema on seotud, kustutatakse. Kui rakenduse sisaldab loogika tellimuse loomiseks, tuleks esmalt kontrollida, kui tellimus juba olemas, getSubscription meetodi abil.

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Luua tellimuse vaikefilter (MatchAll)

**MatchAll** filter on vaikefilter, mida kasutatakse kui filtrit pole määratud, kui luuakse uus tellimus. **MatchAll** filtri kasutamisel paigutatakse kõik sõnumid, mis on avaldatud teema see tellimus virtuaalse järjekorda. Järgmises näites nimega "kõik sõnumid" tellimuse loob ja kasutab **MatchAll** vaikefilter.

```
subscription = azure_service_bus_service.create_subscription("test-topic", "all-messages")
```

### <a name="create-subscriptions-with-filters"></a>Tellimuste filtritega loomine

Samuti saate määratleda filtrid, mis võimaldavad teil määrata, milliseid teema saadetud sõnumid ilmub teatud tellimuse sees.

Kõige paindlik tellimuste filter on selle **Azure::ServiceBus::SqlFilter**, mis rakendab SQL92 alamhulga. SQL-i filtrid toimivad atribuutide sõnumid, mis on avaldatud teema. SQL-i filtriga kasutatavate avaldiste kohta lisateabe saamiseks vaadake [SqlFilter.SqlExpression](http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx) süntaks.

Saate lisada filtreid tellimust, kasutades funktsiooni **loomine\_rule()** **Azure::ServiceBusService** objekti meetodit. See meetod võimaldab uute filtrite lisamine olemasolevale tellimusele.

Kuna vaikefilter rakendatakse kõigi uute tellimused automaatselt, peate eemaldama vaikefilter või **MatchAll** alistab võite määrata filtreid. Saate eemaldada vaikimisi reegli abil soovitud **kustutamine\_rule()** meetod **Azure::ServiceBusService** objekti.

Järgmises näites luuakse nimega "kõrge messages" **Azure::ServiceBus::SqlFilter** , mis valib ainult sõnumid, mis on kohandatud tellimuse **sõnumi\_arvu** atribuut, mis on suurem kui 3:

```
subscription = azure_service_bus_service.create_subscription("test-topic", "high-messages")
azure_service_bus_service.delete_rule("test-topic", "high-messages", "$Default")

rule = Azure::ServiceBus::Rule.new("high-messages-rule")
rule.topic = "test-topic"
rule.subscription = "high-messages"
rule.filter = Azure::ServiceBus::SqlFilter.new({
  :sql_expression => "message_number > 3" })
rule = azure_service_bus_service.create_rule(rule)
```

Järgmises näites luuakse samuti tellimuse nimega "madal messages" **Azure::ServiceBus::SqlFilter** , mis valib ainult sõnumid, mis on **message_number** atribuudi väiksem või võrdne 3:

```
subscription = azure_service_bus_service.create_subscription("test-topic", "low-messages")
azure_service_bus_service.delete_rule("test-topic", "low-messages", "$Default")

rule = Azure::ServiceBus::Rule.new("low-messages-rule")
rule.topic = "test-topic"
rule.subscription = "low-messages"
rule.filter = Azure::ServiceBus::SqlFilter.new({
  :sql_expression => "message_number <= 3" })
rule = azure_service_bus_service.create_rule(rule)
```

Kui sõnum saadetakse kohe "test teema", see on alati toimetada vastuvõtjad tellinud "kõik sõnumid" teema tellimus ja valikuliselt toimetatud vastuvõtjad tellinud "kõrge-sõnumitele" ja "madal –" teema tellimused (sõltuvalt sõnumi sisu).

## <a name="send-messages-to-a-topic"></a>Sõnumite saatmiseks teema

Teenuse siini teema sõnumi saatmiseks peate kasutama rakenduse selle **saata\_teema\_protsessi** **Azure::ServiceBusService** objekti meetod. Teenuse siini teemade saadetud sõnumid on juhtumeid **Azure::ServiceBus::BrokeredMessage** objektid. **Azure::ServiceBus::BrokeredMessage** objektid on kogum standardatribuudid (nt **silt** ja **aja\_abil\_live**), hoidke kohandatud rakenduse teatud atribuutide kasutatava sõnastiku ja keha stringi andmed. Rakenduse seadistamiseks sõnumi kehasse stringiväärtuse, läbides selle **saata\_teema\_protsessi** meetod ja mõni nõutav standardatribuudid olema täidetud järgmised vaikimisi väärtused.

Järgmises näites näitab, kuidas saata viis testida sõnumeid "test teema". Pange tähele, et iga sõnumi **message_number** kohandatud atribuudi väärtus muutub kursis iteratsiooni (seda määrab, milline tellimus saab seda):

```
5.times do |i|
  message = Azure::ServiceBus::BrokeredMessage.new("test message " + i,
    { :message_number => i })
  azure_service_bus_service.send_topic_message("test-topic", message)
end
```

Teenuse siini teemade toetamiseks [Premium taseme](service-bus-premium-messaging.md)sõnumi maksimaalse mahu 256 KB [Standard taseme](service-bus-premium-messaging.md) ja 1 MB. Päisest, mis sisaldab standard- ja kohandatud rakenduse atribuutide, võib olla maksimaalse mahu 64 KB. Teema sõnumite arv pole piiratud, kuid kokku suurus sõnumid, mille teema on ülempiir. Selles teemas maht on määratletud loomise ajal, 5 GB ülempiir.

## <a name="receive-messages-from-a-subscription"></a>Sõnumeid, mis tellimusest

Saadetud meilisõnumid tellimuse kasutades soovitud **vastu\_tellimuse\_protsessi** **Azure::ServiceBusService** objekti meetod. Vaikimisi sõnumid on read(peak) ja ilma kustutamine tellimuse lukus. Saate lugeda ja sõnumi kustutamine tellimusest, seades selle **pisieelvaade\_Lukusta** suvand **väär (FALSE)**.

Vaikekäitumise teeb lugemis- ja kahe etapiga toiming, mis muudab toetamiseks rakendused, mistõttu puuduvad sõnumite kustutamine. Kui teenus siini saab taotluse, leiab see järgmine sõnum olla consumed, lukustab vältimaks teiste tarbijate seda vastu, ja tagastab rakendus. Pärast rakenduse lõppemist sõnumi töötlemine (või talletab selle tingimata tulevaste töötlemiseks), lõpetab vastuvõtu protsessi teise etapi helistades **kustutamine\_tellimuse\_protsessi** meetod ja esitada parameetrina soovitud sõnum. Funktsiooni **kustutamine\_tellimuse\_protsessi** meetod on sõnumi tarbitud ja eemaldamine tellimusest.

Kui soovitud **: peek\_Lukusta** parameeter on seatud väärtusele **väär**, lugemispaani ja sõnumi kustutamine muutub lihtsaim mudel ja sobib kõige paremini stsenaariumid, kus saate rakenduse luba töötlemise sõnumi tõrke korral. Mõistmaks, kaaluge stsenaarium, kus tarbija probleemide vastuvõtu taotlus ja siis kaob enne selle töötlemiseks. Kuna teenuse siini on märgitud sõnumid tarbitud, siis, kui rakenduse taaskäivitamist ja algab tarbimine sõnumite uuesti, see on vastamata sõnum, mis oli tarbitud enne krahhi.

Järgmises näites näitab, kuidas saate vastuvõetud sõnumid ja töödeldud abil **vastu\_tellimuse\_protsessi**. Näide esmalt saab ja kustutab sõnumi tellimusest "madal sõnumid", kasutades **: peek\_Lukusta** väärtuseks **false**, siis saab teise sõnumi "kõrge sõnumid" ja seejärel kustutab sõnumi abil **kustutamine\_tellimuse\_protsessi**:

```
message = azure_service_bus_service.receive_subscription_message(
  "test-topic", "low-messages", { :peek_lock => false })
message = azure_service_bus_service.receive_subscription_message(
  "test-topic", "high-messages")
azure_service_bus_service.delete_subscription_message(message)
```

## <a name="handle-application-crashes-and-unreadable-messages"></a>Rakendus jookseb ja loetavad sõnumid

Teenuse siini pakub funktsiooni abil saate taastada nõtkelt kaudu oma sõnumi töötlemine probleeme või tõrkeid. Kui vastuvõtja rakendus ei saa sõnumit töödelda mingil põhjusel, siis seda saate helistada soovitud **avada\_tellimuse\_protsessi** **Azure::ServiceBusService** objekti meetod. See põhjustab teenuse siini sees tellimuse sõnumi avamiseks ja kättesaadavaks saanud uuesti, kas sama nõudvate rakenduse või mõni muu nõudvate rakendus.

On ka seotud lukustatud jooksul tellimuse sõnumi ajalõpp ja kui taotlus ei töödelda enne sõnumi lock ajalõpu aegumise (näiteks siis, kui rakendus jookseb), siis teenuse siini kuvatakse sõnumi automaatselt avada ja kättesaadavaks uuesti vastu võtta.

Juhul kui rakendus jookseb pärast töötlemist sõnumi, kuid enne selle **kustutamine\_tellimuse\_protsessi** meetodit nimetatakse, siis sõnum on taastarnimiseni rakendusse taaskäivitamisel. Seda nimetatakse sageli **Vähemalt ühe korra töötlemine**; Seega iga sõnumi töödeldakse vähemalt ühe korra, kuid teatud juhtudel võib taastarnimiseni sama sõnumi. Kui seda stsenaariumi ei luba dubleeritud töötlemine, siis rakenduste arendajatele peaks lisada täiendavad loogika taotlusega sõnumite kohaletoimetamise käsitlema. See loogika on sageli saavutada, kasutades funktsiooni **sõnumi\_id** atribuut, mis samaks saatmiskatsete üle.

## <a name="delete-topics-and-subscriptions"></a>Teemade ja tellimuste kustutamine

Teemade ja tellimused on püsiv ja tuleb selgesõnaliselt [Azure portaali][] kaudu või programmiliselt kustutada. Järgmises näites näitab, kuidas kustutada teema nimega "test teema".

```
azure_service_bus_service.delete_topic("test-topic")
```

Teema kustutamine kustutatakse ka kõik tellimused, mis on registreeritud teema. Samuti saate tellimuste sõltumatult kustutatud. Järgmine kood näitab, kuidas kustutada tellimuse "kõrge-sõnumite" nimega "test teema" teema:

```
azure_service_bus_service.delete_subscription("test-topic", "high-messages")
```

## <a name="next-steps"></a>Järgmised sammud

Nüüd, kui olete teenuse siini teemade põhitõdesid õppinud, järgige neid linke lisateabe.

- Vt [järjekorrad, teemade, ja tellimused](service-bus-queues-topics-subscriptions.md).
- API viide [SqlFilter](http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.aspx)jaoks.
- Külastage [Azure'i Tarkvaraarenduskomplektist, mis](https://github.com/Azure/azure-sdk-for-ruby) hoidla github.
 
[Azure'i portaal]: https://portal.azure.com
