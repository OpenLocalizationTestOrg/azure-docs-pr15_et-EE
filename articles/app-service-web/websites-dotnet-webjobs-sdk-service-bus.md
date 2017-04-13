<properties 
    pageTitle="Kuidas kasutada Azure teenuse siini WebJobs SDK" 
    description="Saate teada, kuidas kasutada Azure teenuse siini järjekorrad ja teemade WebJobs SDK." 
    services="app-service\web, service-bus" 
    documentationCenter=".net" 
    authors="tdykstra" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="06/01/2016" 
    ms.author="tdykstra"/>

# <a name="how-to-use-azure-service-bus-with-the-webjobs-sdk"></a>Kuidas kasutada Azure teenuse siini WebJobs SDK

## <a name="overview"></a>Ülevaade

Sellest juhendist leiate C# koodi näidised, mis näitab, kuidas käivitamiseks Azure'i teenus siini meilisõnumi saamisel. Proovi kood kasutada [WebJobs SDK](websites-dotnet-webjobs-sdk.md) versioon 1.x.

Juhend eeldab, et teate, [Kuidas luua WebJob projekti Visual Studios, mis käsk konto salvestusruumi ühendusstringi abil](websites-dotnet-webjobs-sdk-get-started.md).

Funktsiooni Koodilõigud kuvada ainult funktsioone, koodi, mis loob soovitud `JobHost` objekti, nagu järgmises näites:

```
public class Program
{
   public static void Main()
   {
      JobHostConfiguration config = new JobHostConfiguration();
      config.UseServiceBus();
      JobHost host = new JobHost(config);
      host.RunAndBlock();
   }
}
```

[Täieliku siini teenuse kood näide](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/ServiceBus/Program.cs) on azure webjobs-sdk näidised hoidlas GitHub.com kohta.

## <a id="prerequisites"></a>Eeltingimused

Teenuse siini töötamiseks peate installima [Microsoft.Azure.WebJobs.ServiceBus](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus/) Nugeti pakett lisaks muude WebJobs SDK paketid. 

Peate ka seadmine Lisaks salvestusruumi ühendusstringi AzureWebJobsServiceBus ühendusstring.  Saate teha selle `connectionStrings` osa App.config faili, nagu on näidatud järgmises näites:

        <connectionStrings>
            <add name="AzureWebJobsDashboard" connectionString="DefaultEndpointsProtocol=https;AccountName=[accountname];AccountKey=[accesskey]"/>
            <add name="AzureWebJobsStorage" connectionString="DefaultEndpointsProtocol=https;AccountName=[accountname];AccountKey=[accesskey]"/>
            <add name="AzureWebJobsServiceBus" connectionString="Endpoint=sb://[yourServiceNamespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[yourKey]"/>
        </connectionStrings>

Valimi projekt, mis sisaldab teenuse siini ühenduse stringi säte App.config faili, vt [teenuse siini näide](https://github.com/Azure/azure-webjobs-sdk-samples/tree/master/BasicSamples/ServiceBus). 

Ühenduse stringe saab määrata ka Azure käitusaja keskkonnas, kus siis alistab App.config sätted soovitud WebJob käitamisel Azure; Lisateavet leiate teemast [Alustamine WebJobs SDK](websites-dotnet-webjobs-sdk-get-started.md#configure-the-web-app-to-use-your-azure-sql-database-and-storage-account).

## <a id="trigger"></a>Kuidas funktsiooni käivitamiseks teenuse siini järjekorda sõnumi saabumisel

Kirjutage WebJobs SDK kõned kui järjekorda sõnum on funktsiooni, kasutage funktsiooni `ServiceBusTrigger` atribuut. Atribuut ehitaja võtab parameeter, mis määrab nime järjekorra küsitlus.

### <a name="how-servicebustrigger-works"></a>ServiceBusTrigger tööpõhimõte

SDK saab sõnum `PeekLock` režiimi ja kõnede `Complete` sõnumit, kui funktsioon lõpule viidud või kõnede `Abandon` funktsiooni nurjumisel. Kui funktsioon töötab pikem kui selle `PeekLock` ajalõpp lukustus uuendatakse automaatselt.

Teenuse siini ei oma mürki järjekorda töötlemine, mida ei saa kontrollida või WebJobs SDK konfigureeritud. 

### <a name="string-queue-message"></a>Stringi järjekorda sõnum

Järgmine kood näidis loeb järjekorda teade, mis sisaldab stringi ja kirjutab stringi WebJobs SDK armatuurlaud.

        public static void ProcessQueueMessage([ServiceBusTrigger("inputqueue")] string message, 
            TextWriter logger)
        {
            logger.WriteLine(message);
        }

**Märkus:** Kui loote rakenduses, mis ei kasuta WebJobs SDK järjekorda sõnumeid, veenduge, et seada [BrokeredMessage.ContentType](http://msdn.microsoft.com/library/microsoft.servicebus.messaging.brokeredmessage.contenttype.aspx) "teksti/tavaliseks".

### <a name="poco-queue-message"></a>Sõnumi POCO järjekord

SDK kuvatakse automaatselt andmeatribuutide järjekorda sõnumi, mis sisaldab JSON jaoks soovitud POCO [(tavaline vana CLR-i objekti](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) tüüp. Järgmine kood näidis loeb järjekorda sõnum, mis sisaldab soovitud `BlobInformation` objekti, mis sisaldab soovitud `BlobName` atribuudi:

        public static void WriteLogPOCO([ServiceBusTrigger("inputqueue")] BlobInformation blobInfo,
            TextWriter logger)
        {
            logger.WriteLine("Queue message refers to blob: " + blobInfo.BlobName);
        }

Näitab, kuidas kasutada funktsiooni POCO atribuudid plekid ja tabelid sama funktsiooni töötamiseks koodinäiteid, vt [salvestusruumi järjekorrad versiooni selles artiklis](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#pocoblobs).

Kui teie sõnumi järjekorda vöötkood ei kasuta WebJobs SDK, kasutada koodi, mis sarnaneb järgmises näites:

        var client = QueueClient.CreateFromConnectionString(ConfigurationManager.ConnectionStrings["AzureWebJobsServiceBus"].ConnectionString, "blobadded");
        BlobInformation blobInformation = new BlobInformation () ;
        var message = new BrokeredMessage(blobInformation);
        client.Send(message);

### <a name="types-servicebustrigger-works-with"></a>Tüüpi ServiceBusTrigger töötab

Lisaks `string` ja POCO tüübid, saate kasutada funktsiooni `ServiceBusTrigger` baitide massiivis atribuuti või `BrokeredMessage` objekti.

## <a id="create"></a>Kuidas luua teenuse siini järjekorda sõnumid

Funktsioon, mis loob uue järjekorda sõnumi kasutamine kirjutamiseks on `ServiceBus` atribuut ja edastama atribuut ehitaja järjekorra nimi. 


### <a name="create-a-single-queue-message-in-a-non-async-function"></a>Funktsiooni asünkroonse ühe järjekorda sõnumi loomine

Järgmine kood näidis kasutab on väljund parameeter uue sõnumi loomiseks klõpsake nimega "outputqueue" nimega "inputqueue" järjekorda vastuvõetud sõnumid sama sisu koos järjekorras.

        public static void CreateQueueMessage(
            [ServiceBusTrigger("inputqueue")] string queueMessage,
            [ServiceBus("outputqueue")] out string outputQueueMessage)
        {
            outputQueueMessage = queueMessage;
        }

Väljundi parameeter ühe järjekorda sõnumi loomisel võib olla mis tahes järgmist tüüpi:

* `string`
* `byte[]`
* `BrokeredMessage`
* Sarjadesse jaotatav POCO tüüp, mida saate määratleda. Automaatselt seeriasertide JSON nimega.

POCO tüüp parameetrite järjekorda sõnumi alati loodud funktsiooni lõppemisel; Kui parameeter on tühi, loob SDK järjekorda teade, mis tagastavad null kui sõnum on saanud ja deserialized. Muud tüüpi, kui parameeter on null pole järjekorda sõnumi luuakse.

### <a name="create-multiple-queue-messages-or-in-async-functions"></a>Mitme järjekorda meilisõnumi loomine või asünkroonse funktsioonid

Mitme meilisõnumi loomiseks kasutage funktsiooni `ServiceBus` atribuut koos `ICollector<T>` või `IAsyncCollector<T>`, nagu on näidatud järgmises näites kood:

        public static void CreateQueueMessages(
            [ServiceBusTrigger("inputqueue")] string queueMessage,
            [ServiceBus("outputqueue")] ICollector<string> outputQueueMessage,
            TextWriter logger)
        {
            logger.WriteLine("Creating 2 messages in outputqueue");
            outputQueueMessage.Add(queueMessage + "1");
            outputQueueMessage.Add(queueMessage + "2");
        }

Iga sõnumi järjekorda luuakse kohe kui selle `Add` meetod on kutsutud.

## <a id="topics"></a>Kuidas töötada teenuse siini Teemad

Funktsioon, mis SDK nõuab siini teenuse teema sõnumi saabumisel kirjutamiseks saate kasutada funktsiooni `ServiceBusTrigger` atribuuti konstruktori, mis viib teema ja tellimuse nimi, nagu on näidatud järgmises näites kood:

        public static void WriteLog([ServiceBusTrigger("outputtopic","subscription1")] string message,
            TextWriter logger)
        {
            logger.WriteLine("Topic message: " + message);
        }

Sõnumi loomine mõnel kindlal huvipakkuval teemal, kasutage funktsiooni `ServiceBus` atribuuti teema nimega ühtemoodi, saate seda kasutada koos järjekorra nimi.

## <a name="features-added-in-release-11"></a>Funktsioonid, mis on lisatud versiooni 1.1

Järgmised funktsioonid on lisatud versiooni 1.1:

* Luba sügav kohandamise kaudu teate `ServiceBusConfiguration.MessagingProvider`.
* `MessagingProvider`toetab teenus kohandamine `MessagingFactory` ja `NamespaceManager`.
* A `MessageProcessor` strateegia mustri saate määrata protsessor järjekorda/teema kohta.
* Vaikimisi on toetatud sõnumi töötlemine kokkulangevus. 
* Lihtne kohandamine `OnMessageOptions` kaudu `ServiceBusConfiguration.MessageOptions`.
* Luba [AccessRights](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/ServiceBus/Functions.cs#L71) määratavaid `ServiceBusTriggerAttribute` / `ServiceBusAttribute` (jaoks stsenaariumid, kus teil ei ole õiguste haldamine). 

## <a id="queues"></a>Seotud teemad salvestusruumi järjekorrad juhise järgi

WebJobs SDK stsenaariumid, pole teatud teenuse siini kohta leiate teemast [Azure järjekorda salvestusruumi WebJobs SDK kasutamine](websites-dotnet-webjobs-sdk-storage-queues-how-to.md). 

Selle artikli teemad on järgmised:

* Asünkroonse funktsioonid
* Mitmes eksemplaris
* Graatsiline sulgemine
* Kasutage funktsiooni kehas WebJobs SDK atribuudid
* Ühendusstringi SDK-koodi määramine
* Määrata väärtused WebJobs SDK ehitaja parameetrite kood
* Funktsiooni käsitsi käivitamine
* Logide kirjutamine

## <a id="nextsteps"></a>Järgmised sammud

Sellest juhendist on andnud koodinäiteid, reageerimine tavastsenaariumid töötamine Azure'i teenus siini kuvavate. Azure'i WebJobs ja WebJobs SDK kasutamise kohta leiate lisateavet teemast [Azure WebJobs Soovitatavad ressursid](http://go.microsoft.com/fwlink/?linkid=390226).
 
