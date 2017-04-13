<properties
    pageTitle="Kasutamise järjekorra salvestusruumi Java kaudu | Microsoft Azure'i"
    description="Saate teada, kuidas kasutada teenust Azure järjekorda loomine ja kustutamine järjekordade ja lisada, leida ja sõnumite kustutamine. Näidised kirjutatud Java."
    services="storage"
    documentationCenter="java"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robinsh"/>

# <a name="how-to-use-queue-storage-from-java"></a>Kuidas kasutada järjekorda salvestusruumi Java kaudu

[AZURE.INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Ülevaade

Sellest juhendist näitab teile, kuidas teha levinud stsenaariumi, mis on Azure järjekorda salvestusteenus abil. Näidiste Java kirjutada ja kasutada [Azure salvestusruumi SDK Java][]. Stsenaariumid, mis hõlmab hulka **lisamise**, **piilumist**, **otsimine**ja **kustutamine** järjekorda sõnumeid, samuti järjekorrad **loomine** ja **kustutamine** . Järjekorrad kohta leiate lisateavet jaotisest [järgmised toimingud](#Next-Steps) .

Märkus: Ka SDK on arendajatele, kes kasutavad rakendust Azure Storage Androidi seadmete jaoks saadaval. Lisateavet leiate [Azure'i salvestusruumi SDK Androidi jaoks][].

[AZURE.INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-java-application"></a>Java rakenduse loomine

Sellest juhendist kasutate salvestusruumi funktsioonid, mida saab käivitada Java rakenduses kohalikult või koodi, mis töötab web rolli või töötaja rolli Azure.

Selleks, peate installima soovitud Java Kit (JDK) ja Azure tellimuse Azure storage konto loomine. Kui te olete teinud, peate veenduge, et teie arendamise süsteemi vastab miinimumnõuded ja sõltuvused, mis on loetletud [Azure'i salvestusruumi SDK Java][] hoidla github. Kui teie süsteemi vastab nendele nõuetele, järgite juhiseid allalaadimist ja installimist Azure salvestusruumi teekide Java teie süsteemi selle hoidla. Kui olete lõpetanud need ülesanded, saab luua Java rakendus, mis kasutab selle artikli näited.

## <a name="configure-your-application-to-access-queue-storage"></a>Rakenduse järjekorda salvestusruumi juurdepääsu konfigureerimine

Java faili, kuhu soovite kasutada Azure storage API-de juurde pääseda järjekorrad ülaosas impordi järgmistest lisamiseks tehke järgmist.

    // Include the following imports to use queue APIs.
    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.queue.*;

## <a name="setup-an-azure-storage-connection-string"></a>Häälestus on Azure storage ühendusstring

Kui Azure storage klient kasutab salvestusruumi ühendusstringi salvestada lõpp-punktid ja identimisteabe juurdepääsuks andmete halduse teenused. Kui kliendi rakendus töötab, peab võimaldama salvestusruumi ühendusstringi järgmises vormingus [Azure portaali](https://portal.azure.com) *account_name* ja *AccountKey* väärtuste loetletud salvestusruumi konto nimi konto salvestusruumi ja esmane kiirklahv kasutamise. Selles näites on näha, kuidas saate deklareerida staatiline väli ühendusstring mahutamiseks:

    // Define the connection-string with your values.
    public static final String storageConnectionString =
        "DefaultEndpointsProtocol=http;" +
        "AccountName=your_storage_account;" +
        "AccountKey=your_storage_account_key";

Rakenduse Microsoft Azure rolli ettenähtud stringile teenuse konfiguratsioonifail *ServiceConfiguration.cscfg*, saab salvestada ja pääseb **RoleEnvironment.getConfigurationSettings** meetod kõne. Siin on näide **säte** element, nimega *StorageConnectionString* teenuse konfiguratsioonifailis ühendusstringi hankimine:

    // Retrieve storage account from connection-string.
    String storageConnectionString =
        RoleEnvironment.getConfigurationSettings().get("StorageConnectionString");

Järgmised näidet Oletame, et olete kasutanud mõnda viisil järgmistest salvestusruumi ühendusstringi toomiseks.

## <a name="how-to-create-a-queue"></a>Kuidas: järjekorra loomiseks

**CloudQueueClient** objekti võimaldab teil saada viide objektide järjekorrad. Järgmine kood tekitab **CloudQueueClient** objekti. (Märkus: on täiendavaid võimalusi luua **CloudStorageAccount** objekte; lisateabe saamiseks leiate [Azure'i salvestusruumi kliendi SDK viide] **CloudStorageAccount** .)

Kasutage **CloudQueueClient** objekti viide järjekorda, mida soovite kasutada. Kui see pole olemas, saate luua järjekorda.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

       // Create the queue client.
       CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

       // Retrieve a reference to a queue.
       CloudQueue queue = queueClient.getQueueReference("myqueue");

       // Create the queue if it doesn't already exist.
       queue.createIfNotExists();
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-add-a-message-to-a-queue"></a>Kuidas: saate lisada sõnumi järjekorda

Sõnumi lisamiseks mõne olemasoleva järjekorra esmalt looma uue **CloudQueueMessage**. Järgmiseks helistage **addMessage** meetod. Mõne **CloudQueueMessage** saab luua string (vormingus UTF-8) või baitide massiivis. Siin on kood, mis loob järjekorda (kui see pole) ja lisab sõnumi "Tere, maailm".

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // Create the queue if it doesn't already exist.
        queue.createIfNotExists();

        // Create a message and add it to the queue.
        CloudQueueMessage message = new CloudQueueMessage("Hello, World");
        queue.addMessage(message);
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-peek-at-the-next-message"></a>Kuidas: järgmise sõnumi Piilumine

Saate sõnumi ees järjekorda Piilumine ilma eemaldamine järjekorda, helistades **peekMessage**.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // Peek at the next message.
        CloudQueueMessage peekedMessage = queue.peekMessage();

        // Output the message value.
        if (peekedMessage != null)
        {
          System.out.println(peekedMessage.getMessageContentAsString());
       }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-change-the-contents-of-a-queued-message"></a>Kuidas: ootel sõnumi sisu muutmine

Saate muuta sisu on sõnumi kohapealne järjekorda. Kui sõnumi tähistab ülesande tööd, võib selle funktsiooni abil värskendada tööülesande oleku. Järgmine kood värskendab järjekorda sõnumi uue sisu ja määrab nähtavuse ajalõpp pikendada mõne muu 60 sekundi järel. See salvestab sõnumi seotud töö oleku ja annab kliendi veel minut sõnum edasi töötada. Selle meetodi abil võib jälgida mitme etapi töövoogude järjekorda sõnumitele, ilma vajaduseta uuesti alustamiseks algusest kui töötlemine katkeb või riistvara tõrke tõttu. Tavaliselt hoiate uuesti count ka ja kui sõnumi uuesti proovida rohkem kui *n* korda, peaksite kustutama. See kaitseb sõnum, mis käivitab rakenduse viga iga kord, kui see on töödeldud.

Järgmine kood näidis otsinguid kaudu sõnumite järjekord leiab esimese sõnumi, mis vastab "Tere, maailm" sisu, seejärel muudab sõnumi sisu ning väljub.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // The maximum number of messages that can be retrieved is 32.
        final int MAX_NUMBER_OF_MESSAGES_TO_PEEK = 32;

        // Loop through the messages in the queue.
        for (CloudQueueMessage message : queue.retrieveMessages(MAX_NUMBER_OF_MESSAGES_TO_PEEK,1,null,null))
        {
            // Check for a specific string.
            if (message.getMessageContentAsString().equals("Hello, World"))
            {
                // Modify the content of the first matching message.
                message.setMessageContent("Updated contents.");
                // Set it to be visible in 30 seconds.
                EnumSet<MessageUpdateFields> updateFields =
                    EnumSet.of(MessageUpdateFields.CONTENT,
                    MessageUpdateFields.VISIBILITY);
                // Update the message.
                queue.updateMessage(message, 30, updateFields, null, null);
                break;
            }
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

Teise võimalusena järgmine kood näidis värskendatakse ainult esimese nähtav sõnumi järjekorda.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // Retrieve the first visible message in the queue.
        CloudQueueMessage message = queue.retrieveMessage();

        if (message != null)
        {
            // Modify the message content.
            message.setMessageContent("Updated contents.");
            // Set it to be visible in 60 seconds.
            EnumSet<MessageUpdateFields> updateFields =
                EnumSet.of(MessageUpdateFields.CONTENT,
                MessageUpdateFields.VISIBILITY);
            // Update the message.
            queue.updateMessage(message, 60, updateFields, null, null);
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-get-the-queue-length"></a>Kuidas: saada järjekorra pikkus

Saate sõnumite hinnanguline järjekorda. **DownloadAttributes** meetodit küsib mitu praegusi väärtusi, sh mitu sõnumid on järjekorras kuhjuda teenus. Arvestus on ainult ligikaudne, kuna sõnumite saate lisada või eemaldada pärast järjekorda teenuse vastaks teie taotlus. **GetApproximateMessageCount** meetod tagastab viimase väärtuse toodavate **downloadAttributes**, kõne ilma helistamiseks teenuse järjekorda.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
           CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

       // Download the approximate message count from the server.
        queue.downloadAttributes();

        // Retrieve the newly cached approximate message count.
        long cachedMessageCount = queue.getApproximateMessageCount();

        // Display the queue length.
        System.out.println(String.format("Queue length: %d", cachedMessageCount));
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-dequeue-the-next-message"></a>Kuidas: Dequeue järgmise sõnumi

Teie kood dequeues sõnumi järjekorda on kaks toimingut. Kui helistate **retrieveMessage**, saate järgmise sõnumi järjekorda. Tagastatud **retrieveMessage** sõnumi muutub nähtamatu mis tahes muu koodi lugemine sõnumeid selles järjekorras. Vaikimisi see sõnum jääb nähtamatu 30 sekundit. Sõnumi eemaldamine kuhjuda lõpetamiseks helistada ka **deleteMessage**. Kontrollimiseks, eemaldades sõnumi tagab, et kui teie kood ei töödelda sõnumile või riistvara tõrke tõttu, muus eksemplaris oma koodi saate sama teade ja proovige uuesti. Teie kood kõned **deleteMessage** paremale sõnum on töödeldud.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // Retrieve the first visible message in the queue.
        CloudQueueMessage retrievedMessage = queue.retrieveMessage();

        if (retrievedMessage != null)
        {
            // Process the message in less than 30 seconds, and then delete the message.
            queue.deleteMessage(retrievedMessage);
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }


## <a name="additional-options-for-dequeuing-messages"></a>Täiendavate suvandite dequeuing sõnumite jaoks

On kaks võimalust toomise järjekorda kaudu saate kohandada. Esmalt saate paketi sõnumite (kuni 32). Teiseks saate määrata rohkem või vähem nähtamatus ajalõpp, rohkem või vähem aega täielikult töödelda iga sõnumi, mis võimaldab teie kood.

Järgmine kood näide kasutab **retrieveMessages** meetodit saada 20 sõnumite ühe kõne. Seejärel töötleb iga sõnumi **puhul** tsükkel. Seda määrab ka nähtamatus ajalõpp viie minuti (300 sekundi) iga sõnumi kohta. Pange tähele, et viis minutit algab kõigi sõnumite korraga, seega kui 5 minuti on möödunud **retrieveMessages**kõne, kõik sõnumid, mis ei kustutata muutuvad nähtavaks uuesti.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // Retrieve 20 messages from the queue with a visibility timeout of 300 seconds.
        for (CloudQueueMessage message : queue.retrieveMessages(20, 300, null, null)) {
            // Do processing for all messages in less than 5 minutes,
            // deleting each message after processing.
            queue.deleteMessage(message);
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-list-the-queues"></a>Kuidas: järjekorrad loend

Praeguse järjekorda loendi saamiseks helistage **CloudQueueClient.listQueues()** meetod, mis tagastavad **CloudQueue** objektide kogum.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient =
            storageAccount.createCloudQueueClient();

        // Loop through the collection of queues.
        for (CloudQueue queue : queueClient.listQueues())
        {
            // Output each queue name.
            System.out.println(queue.getName());
        }
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="how-to-delete-a-queue"></a>Kuidas: järjekorda kustutamine

Järjekorda ja kõik selles sisalduvad sõnumid kustutamiseks helistada **CloudQueue** objekti **deleteIfExists** meetod.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount =
            CloudStorageAccount.parse(storageConnectionString);

        // Create the queue client.
        CloudQueueClient queueClient = storageAccount.createCloudQueueClient();

        // Retrieve a reference to a queue.
        CloudQueue queue = queueClient.getQueueReference("myqueue");

        // Delete the queue if it exists.
        queue.deleteIfExists();
    }
    catch (Exception e)
    {
        // Output the stack trace.
        e.printStackTrace();
    }

## <a name="next-steps"></a>Järgmised sammud

Nüüd, kui olete õppinud järjekorda salvestusruumi põhitõdesid, järgige neid linke salvestusruumi keerukamate tööülesannete kohta.

- [Azure'i salvestusruumi SDK Java][]
- [Azure'i salvestusruumi kliendi SDK viide][]
- [Azure'i salvestusruumi teenuste REST API-ga][]
- [Azure'i salvestusruumi meeskonna ajaveeb][]

[Azure SDK for Java]: http://go.microsoft.com/fwlink/?LinkID=525671
[Azure'i salvestusruumi SDK Java]: https://github.com/azure/azure-storage-java
[Azure'i salvestusruumi SDK Androidi jaoks]: https://github.com/azure/azure-storage-android
[Azure'i salvestusruumi kliendi SDK viide]: http://dl.windowsazure.com/storage/javadoc/
[Azure'i salvestusruumi teenuste REST API-ga]: https://msdn.microsoft.com/library/azure/dd179355.aspx
[Azure'i salvestusruumi meeskonna ajaveeb]: http://blogs.msdn.com/b/windowsazurestorage/
