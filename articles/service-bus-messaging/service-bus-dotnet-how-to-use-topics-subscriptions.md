<properties
    pageTitle=".Net-i teenuse siini teemade abil | Microsoft Azure'i"
    description="Saate teada, kuidas kasutada .NET Azure teenuse siini teemade ja tellimused. .NET rakenduste koodinäiteid kirjutada."
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="get-started-article"
    ms.date="09/16/2016"
    ms.author="sethm"/>

# <a name="how-to-use-service-bus-topics-and-subscriptions"></a>Kuidas kasutada teenuse siini teemade ja tellimused

[AZURE.INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

Selles artiklis kirjeldatakse, kuidas kasutada teenuse siini teemade ja tellimused. Näidiste C# kirjutada ja kasutada .net-i API-d. Stsenaariumid, mis hõlmab kaasata teemade ja tellimused tellimust filtrite loomise, teema sõnumite saatmine, tellimusest sõnumeid ja kustutamine teemade ja tellimuste loomine. Teemade ja tellimuste kohta leiate lisateavet jaotisest [järgmised toimingud](#next-steps) .

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

[AZURE.INCLUDE [howto-service-bus-topics](../../includes/howto-service-bus-topics.md)]

## <a name="configure-the-application-to-use-service-bus"></a>Rakenduse kasutamiseks teenuse siini konfigureerimine

Rakendus kasutab teenuse siini loomisel saate lisada teenuse siini komplekti ja kaasata vastavate nimeruumid. Lihtsaim viis selleks on sobiv [Nugeti](https://www.nuget.org) laadige.

## <a name="get-the-service-bus-nuget-package"></a>Teenuse siini Nugeti paketi hankimine

[Teenuse siini Nugeti pakett](https://www.nuget.org/packages/WindowsAzure.ServiceBus) on kõige lihtsam viis teenuse siini API ja rakenduse konfigureerimiseks vajalikud teenuse siini sõltuvusi. Projekti teenuse siini Nugeti paketi installimiseks tehke järgmist.

1.  Solution Exploreris Paremklõpsake **Viited**ja seejärel klõpsake käsku **Halda NuGet-paketid**.
2.  Otsige "Teenuse siini" ja **Microsoft Azure'i teenus siini** üksuse valimine. **Installida** installimise lõpuleviimiseks nuppu ja seejärel sulgege dialoogiboks järgmist.

    ![][7]

Nüüd olete valmis teenuse siini koodi kirjutamiseks.

## <a name="create-a-service-bus-connection-string"></a>Teenuse siini ühendusstringi loomine

Teenuse siini kasutab ühendusstringi lõpp-punktid ja mandaadi talletamiseks. Saate panna oma ühendusstring konfiguratsioonifail, mitte suur-kodeerimine selle:

- Azure'i teenuste kasutamisel on soovitatav salvestada oma ühendusstring Azure teenuse konfiguratsiooni süsteemi (.csdef ja .cscfg failid) abil.
- Kui kasutate Azure veebisaitide või Azure'i Virtuaalmasinates, on soovitatav salvestada oma ühendusstring, kasutades .net-i konfigureerimine süsteemi (nt faili Web.config).

Mõlemal juhul saate tuua ühenduse stringi abil soovitud `CloudConfigurationManager.GetSetting` meetod, nagu on näidatud selle artikli.

### <a name="configure-your-connection-string"></a>Oma ühendusstring konfigureerimine

Teenuse konfiguratsiooni süsteem võimaldab ilma ümbersuunamine rakenduse [Azure portaali][] dünaamiliselt konfiguratsiooni sätete muutmine. Näiteks lisada mõne `Setting` sildi teenuse määratlus (**.csdef**) faili, nagu on näidatud järgmises näites.

```
<ServiceDefinition name="Azure1">
...
    <WebRole name="MyRole" vmsize="Small">
        <ConfigurationSettings>
            <Setting name="Microsoft.ServiceBus.ConnectionString" />
        </ConfigurationSettings>
    </WebRole>
...
</ServiceDefinition>
```

Seejärel määrake väärtused teenuse konfiguratsioonifail (.cscfg).

```
<ServiceConfiguration serviceName="Azure1">
...
    <Role name="MyRole">
        <ConfigurationSettings>
            <Setting name="Microsoft.ServiceBus.ConnectionString"
                     value="Endpoint=sb://yourServiceNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey" />
        </ConfigurationSettings>
    </Role>
...
</ServiceConfiguration>
```

Kasutage ühiskasutusse Accessi allkirja (SAS) võtme nimi ja võtme väärtuste portaali eelnevalt kirjeldatud.

### <a name="configure-your-connection-string-when-using-azure-websites-or-azure-virtual-machines"></a>Konfigureerige oma ühendusstring Azure veebisaitide või Azure'i Virtuaalmasinates kasutamisel

Kui kasutate veebisaitide või Virtuaalmasinates, on soovitatav kasutada .net-i süsteemi konfiguratsioon (nt Web.config). Saate talletada ühenduse stringi abil soovitud `<appSettings>` element.

```
<configuration>
    <appSettings>
        <add key="Microsoft.ServiceBus.ConnectionString"
             value="Endpoint=sb://yourServiceNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourKey" />
    </appSettings>
</configuration>
```

Kasutage SAS nimi ja väärtused, mille proovisite peidust välja tuua [Azure portaali][]varem kirjeldatud.

## <a name="create-a-topic"></a>Teema loomine

Saate teenuse siini teemade ja [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) klassi kaudu tellimuste haldamise toiminguid teha. See tund pakub võimalust luua, loetlemine ja teemade kustutamine.

Järgmises näites sisuosast on `NamespaceManager` objekti abil soovitud Azure'i `CloudConfigurationManager` klassi koosneb baasaadressi teenuse siini nimeruum ja vastav muude ühendusstringi identimisteabe õigustega haldamiseks. See ühendusstringi on järgmisel kujul:

```
Endpoint=sb://<yourNamespace>.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=<yourKey>
```

Kasutage järgmises näites antud eelmises jaotises sätete konfigureerimine.

```
// Create the topic if it does not exist already.
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

var namespaceManager =
    NamespaceManager.CreateFromConnectionString(connectionString);

if (!namespaceManager.TopicExists("TestTopic"))
{
    namespaceManager.CreateTopic("TestTopic");
}
```

On ülekoormuse [CreateTopic](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.createtopic.aspx) meetod, mis võimaldavad teil määrata atribuudid teema; Näiteks vaikimisi aja live (TTL) väärtus peaks rakenduma teema saadetud sõnumid. Nende sätete rakendatakse [TopicDescription](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicdescription.aspx) klassi abil. Järgmises näites kujutatakse luua nimega **TestTopic** maksimaalse suurusega 5 GB ja vaikimisi sõnum TTL 1 minuti jooksul teema.

```
// Configure Topic Settings.
TopicDescription td = new TopicDescription("TestTopic");
td.MaxSizeInMegabytes = 5120;
td.DefaultMessageTimeToLive = new TimeSpan(0, 1, 0);

// Create a new Topic with custom settings.
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

var namespaceManager =
    NamespaceManager.CreateFromConnectionString(connectionString);

if (!namespaceManager.TopicExists("TestTopic"))
{
    namespaceManager.CreateTopic(td);
}
```

> [AZURE.NOTE] [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) objektide [TopicExists](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.topicexists.aspx) meetodi abil saate kontrollida, kas määratud nimega teema on juba olemas nimeruumi sees.

## <a name="create-a-subscription"></a>Tellimuse loomine

Saate luua ka teema tellimuste [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.servicebus.namespacemanager.aspx) klassi abil. Tellimused on nimega ja võib-olla valikuline filter, mis piirab kogumi, sõnumid, mis on möödas see tellimus virtuaalse järjekorda.

> [AZURE.IMPORTANT] Selleks, et sõnumite tellimuse vastu võtta, peate looma tellimuse enne saatmine teema. Kui pole tellimuste teemale, teema hüljatakse sõnumeid.

### <a name="create-a-subscription-with-the-default-matchall-filter"></a>Luua tellimuse vaikefilter (MatchAll)

Kui filtrit pole määratud, kui luuakse uus tellimus, **MatchAll** filter on vaikefilter, mida kasutatakse. **MatchAll** filtri kasutamisel paigutatakse kõik sõnumid, mis on avaldatud teema see tellimus virtuaalse järjekorda. Järgmises näites nimega "AllMessages" tellimuse loob ja kasutab **MatchAll** vaikefilter.

```
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

var namespaceManager =
    NamespaceManager.CreateFromConnectionString(connectionString);

if (!namespaceManager.SubscriptionExists("TestTopic", "AllMessages"))
{
    namespaceManager.CreateSubscription("TestTopic", "AllMessages");
}
```

### <a name="create-subscriptions-with-filters"></a>Tellimuste filtritega loomine

Saate häälestada ka filtrid, mille abil saate määrata, millised sõnumid saadetakse teema peaks kuvatakse mõne kindla teemaga seotud tellimuse pärast.

Kõige paindlik tellimuste filter on [SqlFilter][] ainekursust, mis rakendab SQL92 alamhulga. SQL-i filtrid toimivad atribuutide sõnumid, mis on avaldatud teema. SQL-i filtriga kasutatavate avaldiste kohta leiate lisateavet teemast [SqlFilter.SqlExpression][] süntaks.

Järgmises näites luuakse nimega **HighMessages** [SqlFilter][] objektiga, mis valib ainult sõnumid, mis on suurem kui 3 kohandatud **MessageNumber** atribuut tellimus.

```
// Create a "HighMessages" filtered subscription.
SqlFilter highMessagesFilter =
   new SqlFilter("MessageNumber > 3");

namespaceManager.CreateSubscription("TestTopic",
   "HighMessages",
   highMessagesFilter);
```

Järgmises näites luuakse samuti nimega **LowMessages** koos [SqlFilter][] , mis valib ainult sõnumid, mis on **MessageNumber** atribuudi väiksem või võrdne 3 tellimust.

```
// Create a "LowMessages" filtered subscription.
SqlFilter lowMessagesFilter =
   new SqlFilter("MessageNumber <= 3");

namespaceManager.CreateSubscription("TestTopic",
   "LowMessages",
   lowMessagesFilter);
```

Nüüd kui sõnum saadetakse `TestTopic`, see saadetakse alati vastuvõtjad tellinud **AllMessages** teema tellimus ja valikuliselt toimetatud vastuvõtjad tellinud **HighMessages** ja **LowMessages** teema tellimused (olenevalt sõnumi sisu).

## <a name="send-messages-to-a-topic"></a>Sõnumite saatmiseks teema

Sõnumi saatmiseks teenuse siini teema rakenduse loob ühendusestringi [TopicClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx) objekti.

Järgmine kood näitab, kuidas luua [TopicClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx) objekti varasemas versioonis loodud **TestTopic** teema on [`CreateFromConnectionString`](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.createfromconnectionstring.aspx) API kõne.

```
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

TopicClient Client =
    TopicClient.CreateFromConnectionString(connectionString, "TestTopic");

Client.Send(new BrokeredMessage());
```

Teenuse siini teemade saadetud sõnumid on juhtumeid [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx) klassi. [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx) objektid on standardatribuudid (nt [silt](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.label.aspx) ja [TimeToLive](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.timetolive.aspx)) sõnastiku, hoidke kohandatud rakenduse kohased atribuudid kasutatava ja keha suvalise rakenduse andmeid. Rakenduse saate määrata sõnumi kehasse sarjadesse jaotatav objekti, saates [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx) objekti konstruktori ja **DataContractSerializer** korral kasutatakse vahemälukausta objekti. Teine võimalus on **System.IO.Stream** on saadaval.

Järgmises näites näitab, kuidas saata viis **TestTopic** [TopicClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.topicclient.aspx) objektile saadud kood eelmises näites. Pange tähele, et iga sõnumi [MessageNumber](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.properties.aspx) atribuudi väärtus erineb sõltuvalt kursis iteratsiooni (seda määrab, millised tellimused jõua adressaadini).

```
for (int i=0; i<5; i++)
{
  // Create message, passing a string message for the body.
  BrokeredMessage message = new BrokeredMessage("Test message " + i);

  // Set additional custom app-specific property.
  message.Properties["MessageNumber"] = i;

  // Send message to the topic.
  Client.Send(message);
}
```

Teenuse siini teemade toetamiseks [Premium taseme](service-bus-premium-messaging.md)sõnumi maksimaalse mahu 256 KB [Standard taseme](service-bus-premium-messaging.md) ja 1 MB. Päisest, mis sisaldab standard- ja kohandatud rakenduse atribuutide, võib olla maksimaalse mahu 64 KB. Teema sõnumite arv pole piiratud, kuid kokku suurus sõnumid, mille teema on ülempiir. Selles teemas maht on määratletud loomise ajal, 5 GB ülempiir. Kui eraldamine on lubatud, on suurem. Lisateavet leiate teemast [Partitioned sõnumside üksused](service-bus-partitioning.md).

## <a name="how-to-receive-messages-from-a-subscription"></a>Kuidas sõnumeid, mis tellimusest

Soovitatav viis sõnumeid vastu tellimus on kasutada [SubscriptionClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.aspx) objekti. [SubscriptionClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.aspx) objektide saate töötada kaks erinevat režiimi: [ *ReceiveAndDelete* ja *PeekLock*](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx).

Kui **ReceiveAndDelete** režiimi kasutades saate on ühe-shot toiming; See tähendab, kui teenus siini saab sõnumi loetuks taotluse tellimus, see märgib sõnumi nagu tarbitud ja tagastab selle rakenduse. **ReceiveAndDelete** režiim on lihtsaim mudel ja sobib kõige paremini stsenaariumid, kus saate rakenduse luba töötlemise sõnumi tõrke korral. Mõistmaks, kaaluge stsenaarium, kus tarbija probleemide vastuvõtu taotlus ja siis kaob enne selle töötlemiseks. Kuna teenuse siini on märgitud sõnumi tarbitud, kui rakenduse taaskäivitamist ja algab tarbimine sõnumite uuesti, vastamata see sõnum, mida on kasutatud enne krahhi.

Turvarežiimis **PeekLock** (see on vaikimisi režiim) vastuvõtu protsessi muutub kahe etapi toiming, mis võimaldab tugi rakenduste, mistõttu puuduvad sõnumeid. Kui teenus siini saab taotluse, leiab see järgmine sõnum olla consumed, lukustab vältimaks teiste tarbijate seda vastu, ja tagastab rakendus. Pärast rakenduse lõppemist sõnumi töötlemine (või talletab selle tingimata tulevaste töötlemiseks), lõpetab teise etapi vastuvõtu protsessi [lõpuleviimine](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) helistades vastuvõetud sõnumi. Kui teenus siini näeb **lõpuleviimine** helistada, märgib sõnumi nagu tarbitud ja eemaldab tellimus.

Järgmises näites näitab, kuidas sõnumeid vastu võtta ning kasutamist vaikerežiim **PeekLock** . Muu [ReceiveMode](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.receivemode.aspx) väärtuse määramiseks saate teise ülekoormuse [CreateFromConnectionString](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.createfromconnectionstring.aspx). Selles näites kasutatakse [OnMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.onmessage.aspx) tagasihelistamise protsessi sõnumeid nende saabumisel **HighMessages** tellimuseks.

```
string connectionString =
    CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");

SubscriptionClient Client =
    SubscriptionClient.CreateFromConnectionString
            (connectionString, "TestTopic", "HighMessages");

// Configure the callback options.
OnMessageOptions options = new OnMessageOptions();
options.AutoComplete = false;
options.AutoRenewTimeout = TimeSpan.FromMinutes(1);

Client.OnMessage((message) =>
{
    try
    {
        // Process message from subscription.
        Console.WriteLine("\n**High Messages**");
        Console.WriteLine("Body: " + message.GetBody<string>());
        Console.WriteLine("MessageID: " + message.MessageId);
        Console.WriteLine("Message Number: " +
            message.Properties["MessageNumber"]);

        // Remove message from subscription.
        message.Complete();
    }
    catch (Exception)
    {
        // Indicates a problem, unlock message in subscription.
        message.Abandon();
    }
}, options);
```

Selles näites konfigureerib [OnMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.subscriptionclient.onmessage.aspx) tagasihelistamise, kasutades [OnMessageOptions](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.onmessageoptions.aspx) objekti. [Automaatteksti](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.onmessageoptions.autocomplete.aspx) on seatud väärtusele **väär** lubada käsitsi kontrolli kui helistada [lõpuleviimine](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) vastuvõetud sõnumi. [AutoRenewTimeout](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.onmessageoptions.autorenewtimeout.aspx) väärtuseks 1 minut, mis põhjustab ootama kuni ühe minuti enne lõpetamist funktsiooni Automaatne uuendamine kliendi ja kliendi muudab kõne uusi sõnumeid kontrollima. Selle atribuudi väärtust vähendab mitu korda kliendi teeb maksustatav kõned, et tuua sõnumeid.

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Rakendus jookseb ja loetavad sõnumite reageerimine

Teenuse siini pakub funktsiooni abil saate taastada nõtkelt kaudu oma sõnumi töötlemine probleeme või tõrkeid. Kui vastuvõtmise rakendus ei saa sõnumit töödelda mingil põhjusel, siis seda saate helistada [loobuda](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.abandon.aspx) meetodit vastuvõetud sõnumi (mitte [lõpuleviimine](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) meetod). See põhjustab teenuse siini sees tellimuse sõnumi avamiseks ja kättesaadavaks saanud uuesti, kas sama nõudvate rakenduse või mõni muu nõudvate rakendus.

On ka seotud lukustatud jooksul tellimuse sõnumi ajalõpp ja kui taotlus ei töödelda enne sõnumi Lukusta ajalõpp aegumise (näiteks siis, kui rakendus jookseb), siis teenuse avab sõnumi automaatselt ja teeb selle kättesaadavaks uuesti vastu võtta.

Juhul kui rakendus jookseb pärast sõnumi töötluse, kuid enne [lõpuleviimine](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.complete.aspx) kutse, kuvatakse sõnumi taastarnimiseni rakendusse taaskäivitamisel. Seda nimetatakse sageli *vähemalt ühe korra töötlemine*; mis on iga sõnumi töötlemise vähemalt ühe korra, kuid teatud juhtudel võib taastarnimiseni sama sõnumi. Kui seda stsenaariumi ei luba dubleeritud töötlemine, siis rakenduste arendajatele peaks lisada täiendavad loogika taotlusega sõnumite kohaletoimetamise käsitlema. See on sageli saavutada, kasutades [MessageId](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx) atribuut, mis jääb samaks saatmiskatsete üle.

## <a name="delete-topics-and-subscriptions"></a>Teemade ja tellimuste kustutamine

Järgmises näites näitab, kuidas kustutada teema **TestTopic** **HowToSample** teenuse nimeruumi.

```
// Delete Topic.
namespaceManager.DeleteTopic("TestTopic");
```

Teema kustutamine kustutatakse ka kõik tellimused, mis on registreeritud teema. Samuti saate tellimuste sõltumatult kustutatud. Järgmine kood näitab, kuidas kustutada tellimuse nimega **HighMessages** **TestTopic** teema.

```
namespaceManager.DeleteSubscription("TestTopic", "HighMessages");
```

## <a name="next-steps"></a>Järgmised sammud

Nüüd, kui olete õppinud põhitõdesid teenuse siini teemade ja tellimused, järgige neid linke lisateabe.

-   [Järjekorrad, teemade, ja tellimused][].
-   [Teema: filtrite näidis][]
-   API viide [SqlFilter][]jaoks.
-   Koostada töötamise rakendus, mis saadab ja võtab ja sealt teenuse siini järjekorda: [teenuse siini vahendajaks sõnumside .net-i õpetus][].
-   Teenuse siini näidised: [Azure'i näidised][] alla laadida või [Ülevaade](service-bus-samples.md).

  [Azure'i portaal]: https://portal.azure.com

  [7]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/getting-started-multi-tier-13.png

  [Järjekorrad, teemasid ja tellimused]: service-bus-queues-topics-subscriptions.md
  [Teema: filtrite näidis]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples/tree/master/TopicFilters
  [SqlFilter]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.aspx
  [SqlFilter.SqlExpression]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
  [Teenuse siini vahendajaks sõnumside .net-i õpetus]: service-bus-brokered-tutorial-dotnet.md
  [Azure'i näidised]: https://code.msdn.microsoft.com/site/search?query=service%20bus&f%5B0%5D.Value=service%20bus&f%5B0%5D.Type=SearchText&ac=2
