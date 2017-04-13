<properties
    pageTitle="Alustamine Azure'i järjekorda salvestusruumi .net-i abil | Microsoft Azure'i"
    description="Azure'i järjekorrad pakuvad usaldusväärne, asünkroonne sõnumside rakenduse osade vahel. Pilvepõhine sõnumside lubab oma rakenduse komponendid mastaapimiseks sõltumatult."
    services="storage"
    documentationCenter=".net"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/12/2016"
    ms.author="robinsh"/>

# <a name="get-started-with-azure-queue-storage-using-net"></a>Alustamine Azure'i järjekorda salvestusruumi .net-i abil

[AZURE.INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Ülevaade

Azure'i järjekorda salvestusruumi pakub pilveteenuse vahel rakenduse komponendid sõnumside. Rakenduste skaala väljatöötamisel komponendid on sageli sidumata, nii, et nad saavad mastaapimiseks sõltumatult. Järjekorda salvestusruumi pakub asünkroonne sõnumside vahel rakenduse komponendid, kas need töötavad pilves, töölaual, kohapealse serveris või mobiilsideseadmes. Järjekorda salvestusruumi toetab ka asünkroonne ülesannete haldamiseks ja koostamise protsessi töövoogudes.

### <a name="about-this-tutorial"></a>Selle õpetuse kohta

Selle õpetuse näitab, kuidas kirjutada mõne levinud stsenaariumi, kasutades Azure järjekorda salvestusruumi .net-i koodi. Stsenaariumid, mis hõlmab kaasata loomine ja kustutamine järjekorrad lisamise, lugemise ja järjekorda sõnumite kustutamine.

**Hinnanguline aega:** 45 minutit

**Prerequisities:**

- [Microsoft Visual Studio](https://www.visualstudio.com/en-us/visual-studio-homepage-vs.aspx)
- [Azure'i salvestusruumi kliendi teek .net-i jaoks](https://www.nuget.org/packages/WindowsAzure.Storage/)
- [Azure'i Configuration Manager .net-i jaoks](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/)
- [Azure storage konto](storage-create-storage-account.md#create-a-storage-account)


[AZURE.INCLUDE [storage-dotnet-client-library-version-include](../../includes/storage-dotnet-client-library-version-include.md)]

[AZURE.INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

[AZURE.INCLUDE [storage-development-environment-include](../../includes/storage-development-environment-include.md)]

### <a name="add-namespace-declarations"></a>Nimeruumi deklaratsiooni lisamine

Lisage järgmine `using` laused ülaosas olevat `program.cs` faili:

    using Microsoft.Azure; // Namespace for CloudConfigurationManager
    using Microsoft.WindowsAzure.Storage; // Namespace for CloudStorageAccount
    using Microsoft.WindowsAzure.Storage.Queue; // Namespace for Queue storage types

### <a name="parse-the-connection-string"></a>Ühendusstringi sõeluda

[AZURE.INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]

### <a name="create-the-queue-service-client"></a>Kliendi teenuse järjekorda loomine

Klassi **CloudQueueClient** võimaldab teil tuua järjekorrad talletatud järjekorda salvestusruumi. Siin on üks viis, kuidas luua teenuse kliendi:

    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

Nüüd olete valmis kirjutada koodi, mis loeb andmeid ja kirjutab andmed järjekorda salvestusruumi.

## <a name="create-a-queue"></a>Järjekorra loomiseks

Selles näites kirjeldatakse, kuidas järjekorra loomiseks, kui see pole juba olemas.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a container.
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Create the queue if it doesn't already exist
    queue.CreateIfNotExists();

## <a name="insert-a-message-into-a-queue"></a>Sisestage sõnumi järjekord

Sõnumi lisamiseks mõne olemasoleva järjekorra esmalt looma uue **CloudQueueMessage**. Järgmiseks helistage **AddMessage** meetod. Mõne **CloudQueueMessage** saab luua string (vormingus UTF-8) või **baitide** massiivis. Siin on kood, mis loob järjekorda (kui see pole) ja lisab sõnumi "Tere, maailm":

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Create the queue if it doesn't already exist.
    queue.CreateIfNotExists();

    // Create a message and add it to the queue.
    CloudQueueMessage message = new CloudQueueMessage("Hello, World");
    queue.AddMessage(message);

## <a name="peek-at-the-next-message"></a>Järgmise sõnumi Piilumine

Saate sõnumi ees järjekorda Piilumine ilma eemaldamine järjekorda, helistades **PeekMessage** meetod.

    // Retrieve storage account from connection string
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Peek at the next message
    CloudQueueMessage peekedMessage = queue.PeekMessage();

    // Display message.
    Console.WriteLine(peekedMessage.AsString);

## <a name="change-the-contents-of-a-queued-message"></a>Ootel sõnumi sisu muutmine

Saate muuta sisu on sõnumi kohapealne järjekorda. Kui sõnumi tähistab ülesande tööd, võib selle funktsiooni abil värskendada tööülesande oleku. Järgmine kood värskendab järjekorda sõnumi uue sisu ja määrab nähtavuse ajalõpp pikendada mõne muu 60 sekundi järel. See salvestab sõnumi seotud töö oleku ja annab kliendi veel minut sõnum edasi töötada. Selle meetodi abil võib jälgida mitme etapi töövoogude järjekorda sõnumitele, ilma vajaduseta uuesti alustamiseks algusest kui töötlemine katkeb või riistvara tõrke tõttu. Tavaliselt hoiate uuesti count ka ja kui sõnumi uuesti proovida rohkem kui *n* korda, peaksite kustutama. See kaitseb sõnum, mis käivitab rakenduse viga iga kord, kui see on töödeldud.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Get the message from the queue and update the message contents.
    CloudQueueMessage message = queue.GetMessage();
    message.SetMessageContent("Updated contents.");
    queue.UpdateMessage(message,
        TimeSpan.FromSeconds(60.0),  // Make it visible for another 60 seconds.
        MessageUpdateFields.Content | MessageUpdateFields.Visibility);

## <a name="de-queue-the-next-message"></a>Järgmise sõnumi tühistage järjekord

Teie kood tühistage järjekorrad sõnumi järjekorda on kaks toimingut. Kui helistate **GetMessage**, saate järgmise sõnumi järjekorda. Tagastatud **GetMessage** sõnumi muutub nähtamatu mis tahes muu koodi lugemine sõnumeid selles järjekorras. Vaikimisi see sõnum jääb nähtamatu 30 sekundit. Sõnumi eemaldamine kuhjuda lõpetamiseks helistada ka **DeleteMessage**. Kontrollimiseks, eemaldades sõnumi tagab, et kui teie kood ei töödelda sõnumile või riistvara tõrke tõttu, muus eksemplaris oma koodi saate sama teade ja proovige uuesti. Teie kood kõned **DeleteMessage** paremale sõnum on töödeldud.

    // Retrieve storage account from connection string
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Get the next message
    CloudQueueMessage retrievedMessage = queue.GetMessage();

    //Process the message in less than 30 seconds, and then delete the message
    queue.DeleteMessage(retrievedMessage);

## <a name="use-async-await-pattern-with-common-queue-storage-apis"></a>Levinud järjekord salvestusruumi API-de asünkroonse ootavad mustri kasutamine

Selles näites kujutatakse kasutama asünkroonse ootavad mustri levinud järjekorda Storage API-d. Valimi kõned asünkroonne versiooni antud meetodi *asünkroonse* järelliite iga meetodi abil. Kui ka asünkroonse kasutatakse, on asünkroonse-ootavad mustri peatab kohaliku täitmise kuni kõne on lõpule viidud. Selline käitumine võimaldab teha muud tööd, mis aitab vältida jõudluse kitsaskohtade ja parandab rakenduse üldine reageerimine praeguse lõime. Asünkroonse-ootame mustri kasutamine .net-i kohta lisateabe saamiseks vt [asünkroonse ja ootame (C# ja Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx)

    // Create the queue if it doesn't already exist
    if(await queue.CreateIfNotExistsAsync())
    {
        Console.WriteLine("Queue '{0}' Created", queue.Name);
    }
    else
    {
        Console.WriteLine("Queue '{0}' Exists", queue.Name);
    }

    // Create a message to put in the queue
    CloudQueueMessage cloudQueueMessage = new CloudQueueMessage("My message");

    // Async enqueue the message
    await queue.AddMessageAsync(cloudQueueMessage);
    Console.WriteLine("Message added");

    // Async dequeue the message
    CloudQueueMessage retrievedMessage = await queue.GetMessageAsync();
    Console.WriteLine("Retrieved message with content '{0}'", retrievedMessage.AsString);

    // Async delete the message
    await queue.DeleteMessageAsync(retrievedMessage);
    Console.WriteLine("Deleted message");

## <a name="leverage-additional-options-for-de-queuing-messages"></a>Lisasuvandid tühistage andmebaasitõrge sõnumite jaoks kasutada.

On kaks võimalust toomise järjekorda kaudu saate kohandada.
Esmalt saate paketi sõnumite (kuni 32). Teiseks saate määrata rohkem või vähem nähtamatus ajalõpp, rohkem või vähem aega täielikult töödelda iga sõnumi, mis võimaldab teie kood. Järgmine kood näide kasutab **GetMessages** meetodit saada 20 sõnumite ühe kõne. Seejärel töötleb iga sõnumi **foreach** tsükkel abil. Seda määrab ka nähtamatus ajalõpp viis minutit iga sõnumi kohta. Pange tähele, et samal ajal kõiki sõnumeid algab 5 minutit pärast 5 minutit on möödunud **GetMessages**, mis tahes sõnumeid, mis ei kustutata kõne muutuvad nähtavaks uuesti.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    foreach (CloudQueueMessage message in queue.GetMessages(20, TimeSpan.FromMinutes(5)))
    {
        // Process all messages in less than 5 minutes, deleting each message after processing.
        queue.DeleteMessage(message);
    }

## <a name="get-the-queue-length"></a>Saada järjekorra pikkus

Saate sõnumite hinnanguline järjekorda. **FetchAttributes** meetodit küsib järjekorda teenuse järjekorda atribuute, sh sõnumi count toomiseks. Atribuudi **ApproximateMessageCount** tagastab viimase väärtuse toodavate **FetchAttributes** meetodit, ilma helistamiseks teenuse järjekorda.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Fetch the queue attributes.
    queue.FetchAttributes();

    // Retrieve the cached approximate message count.
    int? cachedMessageCount = queue.ApproximateMessageCount;

    // Display number of messages.
    Console.WriteLine("Number of messages in queue: " + cachedMessageCount);

## <a name="delete-a-queue"></a>Järjekorda kustutamine

Järjekorda ja kõik selles sisalduvad sõnumid kustutamiseks helistada järjekorda objekti **kustutamine** meetod.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the queue client.
    CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

    // Retrieve a reference to a queue.
    CloudQueue queue = queueClient.GetQueueReference("myqueue");

    // Delete the queue.
    queue.Delete();

## <a name="next-steps"></a>Järgmised sammud

Nüüd, kui olete õppinud järjekorda salvestusruumi põhitõdesid, järgige neid linke salvestusruumi keerukamate tööülesannete kohta.

- Järjekorda teenuse viide dokumentatsiooni kõik üksikasjad saadaval API-de vaatamiseks tehke järgmist.
    - [Salvestusruumi kliendi teek .net-i viide](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)
    - [REST API viide](http://msdn.microsoft.com/library/azure/dd179355)
- Saate teada, kuidas lihtsustada koodi kirjutamise töötamiseks Azure Storage [Azure'i WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk.md)abil.
- Saate vaadata funktsiooni juhikute lisasuvandid Azure andmete säilitamise kohta.
    - [Alustamine Azure'i tabelimälu .net-i abil](storage-dotnet-how-to-use-tables.md) Liigendatud andmete talletamiseks.
    - [Azure'i bloobimälu .NET kasutamise alustamine](storage-dotnet-how-to-use-blobs.md) struktureerimata andmed salvestada.
    - [Ühenduse loomine SQL-andmebaasiga, kasutades .NET (C#)](../sql-database/sql-database-develop-dotnet-simple.md) relatsiooniliste andmete talletamiseks.

  [Download and install the Azure SDK for .NET]: /develop/net/
  [.NET client library reference]: http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409
  [Creating a Azure Project in Visual Studio]: http://msdn.microsoft.com/library/azure/ee405487.aspx
  [Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/
  [OData]: http://nuget.org/packages/Microsoft.Data.OData/5.0.2
  [Edm]: http://nuget.org/packages/Microsoft.Data.Edm/5.0.2
  [Spatial]: http://nuget.org/packages/System.Spatial/5.0.2
