<properties
    pageTitle="Alustamine järjekorda salvestusruumi ja Visual Studio ühendatud teenused (ASP.net-i) | Microsoft Azure'i"
    description="Kuidas alustada, kasutades Azure järjekorda salvestusruumi ASP.net-i projekti Visual Studio pärast salvestusruumi Visual Studio abil kontoga ühenduse ühendatud teenused"
    services="storage"
    documentationCenter=""
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="storage"
    ms.workload="web"
    ms.tgt_pltfrm="vs-getting-started"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="tarcher"/>

# <a name="get-started-with-azure-queue-storage-and-visual-studio-connected-services"></a>Alustamine Azure'i järjekorda salvestusruumi ja Visual Studio ühendatud teenused

[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Ülevaade

Selles artiklis kirjeldatakse, kuidas kasutuselevõtt Azure'i järjekorda salvestusruumi Visual Studios, kui olete loonud või viidatud Azure storage konto ASP.net-i projekti Visual Studio **Ühendatud teenuste lisamine** dialoogiboksi kaudu.

Näitame teile, kuidas luua ja on Azure järjekord teie konto salvestusruumi juurde. Samuti näitame teile kuidas lihtsa järjekorda toiminguid, nagu lisamine, muutmine, lugemispaani ja järjekorda sõnumite eemaldamine. Näidised on kirjutatud C# koodi ja [Microsoft Azure'i salvestusruumi kliendi teek .net-i jaoks](https://msdn.microsoft.com/library/azure/dn261237.aspx)kasutada. ASP.net-i kohta leiate lisateavet teemast [ASP.net-i](http://www.asp.net).

Azure'i järjekorda salvestusruumi on teenus hoidke ühiskaustas palju sõnumeid, millele pääseb juurde kõikjalt maailmas autenditud telefonikõned HTTP- või HTTPS-i kaudu. Ühe järjekorda sõnumi võib olla kuni 64 KB suurust ja järjekorda võib sisaldada miljonite sõnumitega salvestusruumi konto koguvõimsus ulatuses.

## <a name="access-queues-in-code"></a>Accessi järjekorrad kood

ASP.net-i projektide järjekorrad juurdepääsuks peate kaasata järgmised üksused, mis tahes C# lähtefailiga Azure'i järjekorda salvestusruumi juurde.

1. Veenduge, et C# faili ülaosas nimeruumi andmed sisaldavad järgmisi **abil** .

        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Queue;

2. Saada **CloudStorageAccount** objekti, mis tähistab kontoteabe salvestusruumi. Järgmine kood abil saate selle oma salvestusruumi ühendusstringi ja Azure teenuse konfigureerimine salvestusruumi kontoteavet.

         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));

3. Saada **CloudQueueClient** objekti järjekorda objektide teie salvestusruumi konto viitena.  

        // Create the CloudQueueClient object for this storage account.
        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

4. Saada **CloudQueue** objekti kindla järjekorda viitena.

        // Get a reference to a queue named "messageQueue"
        CloudQueue messageQueue = queueClient.GetQueueReference("messageQueue");


**Märkus** Kasutage kõiki ülaltoodud kood ees tähis järgmist näidet.

## <a name="create-a-queue-in-code"></a>Järjekorra-koodi loomiseks

Koodis on Azure järjekorra loomiseks lisage lihtsalt **CreateIfNotExists** kõne ülaltoodud kood.

    // Create the messageQueue if it does not exist
    messageQueue.CreateIfNotExists();

## <a name="add-a-message-to-a-queue"></a>Sõnumi järjekorda lisamine

Mõne olemasoleva järjekorda lisada sõnumi, looge uus **CloudQueueMessage** objekt ja seejärel **AddMessage** meetod.

**CloudQueueMessage** objekti saab luua string (vormingus UTF-8) või baitide massiivis.

Siin on näide, mis lisab sõnumi "Tere, maailm".

    // Create a message and add it to the queue.
    CloudQueueMessage message = new CloudQueueMessage("Hello, World");
    messageQueue.AddMessage(message);

## <a name="read-a-message-in-a-queue"></a>Järjekorda sõnumi lugemine

Saate sõnumi ees järjekorda Piilumine ilma eemaldamine järjekorda, helistades PeekMessage() meetod.

    // Peek at the next message
    CloudQueueMessage peekedMessage = messageQueue.PeekMessage();

## <a name="read-and-remove-a-message-in-a-queue"></a>Lugeda ja sõnumi järjekorda eemaldamine

Saate eemaldada oma koodi (tühistage järjekorda) sõnumi järjekorda on kaks toimingut.
1. Helistage GetMessage() saada järgmine sõnum järjekorda. Tagastatud GetMessage() sõnumi muutub nähtamatu mis tahes muu koodi lugemine sõnumeid selles järjekorras. Vaikimisi see sõnum jääb nähtamatu 30 sekundit.
2.  Sõnumi eemaldamine kuhjuda lõpetamiseks helistage **DeleteMessage**.

Kontrollimiseks, eemaldades sõnumi tagab, et kui teie kood ei töödelda sõnumile või riistvara tõrke tõttu, muus eksemplaris oma koodi saate sama teade ja proovige uuesti. Järgmine kood kõned **DeleteMessage** paremale sõnum on töödeldud.

    // Get the next message in the queue.
    CloudQueueMessage retrievedMessage = messageQueue.GetMessage();

    // Process the message in less than 30 seconds

    // Then delete the message.
    await messageQueue.DeleteMessage(retrievedMessage);


## <a name="use-additional-options-for-de-queuing-messages"></a>Täiendavate suvandite kasutamine tühistage andmebaasitõrge sõnumite jaoks

On kaks võimalust toomise järjekorda kaudu saate kohandada.
Esmalt saate paketi sõnumite (kuni 32). Teiseks saate määrata rohkem või vähem nähtamatus ajalõpp, rohkem või vähem aega täielikult töödelda iga sõnumi, mis võimaldab teie kood. Järgmine kood näide kasutab **GetMessages** meetodit saada 20 sõnumite ühe kõne. Seejärel töötleb iga sõnumi **foreach** tsükkel abil. Seda määrab ka nähtamatus ajalõpp viis minutit iga sõnumi kohta. Pange tähele, et samal ajal kõiki sõnumeid algab 5 minutit pärast 5 minutit on möödunud **GetMessages**, mis tahes sõnumeid, mis ei kustutata kõne muutuvad nähtavaks uuesti.

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

Saate sõnumite hinnanguline järjekorda. **FetchAttributes** meetodit küsib queueservice toomiseks järjekorra atribuutide, sh sõnumi arvu. Atribuudi **ApproximateMethodCount** tagastab viimase väärtuse ilma helistamiseks on queueservice meetodiga **FetchAttributes** tuua.

    // Fetch the queue attributes.
    messageQueue.FetchAttributes();

    // Retrieve the cached approximate message count.
    int? cachedMessageCount = messageQueue.ApproximateMessageCount;

    // Display number of messages.
    Console.WriteLine("Number of messages in queue: " + cachedMessageCount);

## <a name="use-async-await-pattern-with-common-queueapis"></a>Levinud queueAPIs asünkroonse ootavad mustri kasutamine

Selles näites kujutatakse levinud queueAPIs asünkroonse ootavad mustri kasutamiseks. Valimi helistab asünkroonse versiooni antud meetodi, saate märkida asünkroonse pärast lahendus iga meetodi abil. Kui sisestusmeetod asünkroonse kasutatakse selle asünkroonse-ootavad mustri peatab kohaliku täitmise kuni kõne on lõpule viidud. Selline käitumine võimaldab teha muud tööd, mis aitab vältida jõudluse kitsaskohtade ja parandab rakenduse üldine reageerimine praeguse lõime. Lisateavet, asünkroonse-ootame mustri kasutamine .net-i kohta leiate [asünkroonse ja ootame (C# ja Visual Basic)] (https://msdn.microsoft.com/library/hh191443.aspx)

    // Create a message to put in the queue
    CloudQueueMessage cloudQueueMessage = new CloudQueueMessage("My message");

    // Async enqueue the message
    await messageQueue.AddMessageAsync(cloudQueueMessage);
    Console.WriteLine("Message added");

    // Async dequeue the message
    CloudQueueMessage retrievedMessage = await messageQueue.GetMessageAsync();
    Console.WriteLine("Retrieved message with content '{0}'", retrievedMessage.AsString);

    // Async delete the message
    await messageQueue.DeleteMessageAsync(retrievedMessage);
    Console.WriteLine("Deleted message");

## <a name="delete-a-queue"></a>Järjekorda kustutamine

Järjekorda ja kõik selles sisalduvad sõnumid kustutamiseks helistada järjekorda objekti **kustutamine** meetod.

    // Delete the queue.
    messageQueue.Delete();

## <a name="next-steps"></a>Järgmised sammud

[AZURE.INCLUDE [vs-storage-dotnet-queues-next-steps](../../includes/vs-storage-dotnet-queues-next-steps.md)]