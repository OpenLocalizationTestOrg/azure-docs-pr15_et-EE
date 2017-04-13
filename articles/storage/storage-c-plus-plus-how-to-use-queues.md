<properties
    pageTitle="Kasutamise järjekorra salvestusruumi (C++) | Microsoft Azure'i"
    description="Saate teada, kuidas kasutada järjekorda salvestusteenus Azure. Näidet on kirjutatud C++."
    services="storage"
    documentationCenter=".net"
    authors="dineshmurthy"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="dineshm"/>

# <a name="how-to-use-queue-storage-from-c"></a>Kuidas kasutada järjekorda salvestusruumi C++:  

[AZURE.INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Ülevaade
Sellest juhendist näitab teile, kuidas teha levinud stsenaariumi, mis on Azure järjekorda salvestusteenus abil. Näidised kirjutatakse C++ ja [Azure salvestusruumi kliendi teek C++ jaoks](http://github.com/Azure/azure-storage-cpp/blob/master/README.md)kasutada. Stsenaariumid, mis hõlmab hulka **lisamise**, **piilumist**, **otsimine**ja **kustutamine** järjekorda sõnumeid, samuti **loomine ja kustutamine järjekorrad**.

>[AZURE.NOTE] Sellest juhendist eesmärgid Azure'i salvestusruumi kliendi teek c++ versioon 1.0.0 ja kohal. Soovitatav versioon on salvestusruumi kliendi teek 2.2.0, mis on saadaval [Nugeti](http://www.nuget.org/packages/wastorage) või [GitHub](http://github.com/Azure/azure-storage-cpp/)kaudu.

[AZURE.INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]
[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-c-application"></a>C++ rakenduse loomine  
Sellest juhendist kasutate salvestusruumi funktsioonid, mida saab käivitada C++ rakenduses.

Selleks, peate installige Azure'i salvestusruumi kliendi teek C++ ja Azure tellimuse Azure storage konto loomiseks.

Azure'i salvestusruumi kliendi teek c++ installimiseks saate järgmistest viisidest:

-   **Linux:** Järgige lehel [Azure'i salvestusruumi kliendi teegi jaoks C++ README](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) .
-   **Windowsi:** Klõpsake Visual Studio **Tööriistad > Nugeti Package Manager > Package Manager konsooli**. [Nugeti Package Manager konsooli](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) tippige järgmine käsk ja vajutage sisestusklahvi **ENTER**.

        Install-Package wastorage

## <a name="configure-your-application-to-access-queue-storage"></a>Rakenduse järjekorda salvestusruumi juurdepääsu konfigureerimine
Lisage järgmine sisaldavad C++ faili, kuhu soovite kasutada Azure storage API-de juurde pääseda järjekorrad ülaserva.  

    #include "was/storage_account.h"
    #include "was/queue.h"

## <a name="set-up-an-azure-storage-connection-string"></a>Mõne Azure storage ühendusstringi häälestamine

Kui Azure storage klient kasutab salvestusruumi ühendusstringi salvestada lõpp-punktid ja identimisteabe juurdepääsuks andmete halduse teenused. Kui kliendi rakendus töötab, peab võimaldama salvestusruumi ühendusstringi järgmises vormingus [Azure portaali](https://portal.azure.com) *account_name* ja *AccountKey* väärtuste loetletud salvestusruumi konto nimi konto salvestusruumi ja salvestusruumi kiirklahv kasutamise. Salvestusruumi kontod ja kiirklahvide kohta leiate teavet teemast [Azure salvestusruumi kontod](storage-create-storage-account.md). Selles näites on näha, kuidas saate deklareerida staatiline väli ühendusstring mahutamiseks:  

    // Define the connection-string with your values.
    const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));

Kontrollimiseks rakenduse Windowsi kohalikus arvutis, saate kasutada Microsoft Azure'i [salvestusruumi emulaator](storage-use-emulator.md) [Azure'i SDK](https://azure.microsoft.com/downloads/)installitud. Salvestusruumi emulaator on kasuliku, mis jäljendab bloobimälu, järjekord ja tabel teenuste saadaval Azure kohaliku arengu teie arvutis. Järgmises näites kirjeldatakse, kuidas saate deklareerida staatiline väli hoida oma kohalikku emulaator ühendusstring.  

    // Define the connection-string with Azure Storage Emulator.
    const utility::string_t storage_connection_string(U("UseDevelopmentStorage=true;"));  

Azure'i salvestusruumi emulaator käivitamiseks valige nupp **Start** või vajutage **Windowsi** klahvi. Hakake tippima **Azure salvestusruumi emulaator**ja valige **Microsoft Azure'i salvestusruumi emulaator** rakenduste loendist.

Järgmised näidet Oletame, et olete kasutanud mõnda viisil järgmistest salvestusruumi ühendusstringi toomiseks.

## <a name="retrieve-your-connection-string"></a>Teie ühendusstringi toomiseks
Saate tähistada salvestusruumi kontoteabe **cloud_storage_account** klassi. Ühendusstringi salvestusruumi salvestusruumi kontoteabe toomiseks saate **sõeluda** meetod.

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

## <a name="how-to-create-a-queue"></a>Kuidas: järjekorra loomiseks
**Cloud_queue_client** objekti võimaldab teil saada viide objektide järjekorrad. Järgmine kood tekitab **cloud_queue_client** objekti.

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create a queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

Kasutage **cloud_queue_client** objekti viide järjekorda, mida soovite kasutada. Kui see pole olemas, saate luua järjekorda.

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Create the queue if it doesn't already exist.
    queue.create_if_not_exists();  

## <a name="how-to-insert-a-message-into-a-queue"></a>Kuidas: lisada sõnumi järjekorda
Sõnumi lisamiseks mõne olemasoleva järjekorra esmalt looma uue **cloud_queue_message**. Järgmiseks helistage **add_message** meetod. Mõne **cloud_queue_message** saab luua stringi või **baitide** massiivis. Siin on kood, mis loob järjekorda (kui see pole) ja lisab sõnumi "Tere, maailm":

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Create the queue if it doesn't already exist.
    queue.create_if_not_exists();

    // Create a message and add it to the queue.
    azure::storage::cloud_queue_message message1(U("Hello, World"));
    queue.add_message(message1);  

## <a name="how-to-peek-at-the-next-message"></a>Kuidas: järgmise sõnumi Piilumine
Saate sõnumi ees järjekorda Piilumine ilma eemaldamine järjekorda, helistades **peek_message** meetod.

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Peek at the next message.
    azure::storage::cloud_queue_message peeked_message = queue.peek_message();

    // Output the message content.
    std::wcout << U("Peeked message content: ") << peeked_message.content_as_string() << std::endl;

## <a name="how-to-change-the-contents-of-a-queued-message"></a>Kuidas: ootel sõnumi sisu muutmine
Saate muuta sisu on sõnumi kohapealne järjekorda. Kui sõnumi tähistab ülesande tööd, võib selle funktsiooni abil värskendada tööülesande oleku. Järgmine kood värskendab järjekorda sõnumi uue sisu ja määrab nähtavuse ajalõpp pikendada mõne muu 60 sekundi järel. See salvestab sõnumi seotud töö oleku ja annab kliendi veel minut sõnum edasi töötada. Selle meetodi abil võib jälgida mitme etapi töövoogude järjekorda sõnumitele, ilma vajaduseta uuesti alustamiseks algusest kui töötlemine katkeb või riistvara tõrke tõttu. Tavaliselt hoiate uuesti count ka ja kui sõnumi uuesti proovida mitu korda, peaksite kustutama. See kaitseb sõnum, mis käivitab rakenduse viga iga kord, kui see on töödeldud.

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_conection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Get the message from the queue and update the message contents.
    // The visibility timeout "0" means make it visible immediately.
    // The visibility timeout "60" means the client can get another minute to continue
    // working on the message.
    azure::storage::cloud_queue_message changed_message = queue.get_message();

    changed_message.set_content(U("Changed message"));
    queue.update_message(changed_message, std::chrono::seconds(60), true);

    // Output the message content.
    std::wcout << U("Changed message content: ") << changed_message.content_as_string() << std::endl;  

## <a name="how-to-de-queue-the-next-message"></a>Kuidas: tühistage järjekorda järgmise sõnumi
Teie kood tühistage järjekorrad sõnumi järjekorda on kaks toimingut. Kui helistate **get_message**, saate järgmise sõnumi järjekorda. Tagastatud **get_message** sõnumi muutub nähtamatu mis tahes muu koodi lugemine sõnumeid selles järjekorras. Sõnumi eemaldamine kuhjuda lõpetamiseks helistada ka **delete_message**. Kontrollimiseks, eemaldades sõnumi tagab, et kui teie kood ei töödelda sõnumile või riistvara tõrke tõttu, muus eksemplaris oma koodi saate sama teade ja proovige uuesti. Teie kood kõned **delete_message** paremale sõnum on töödeldud.

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Get the next message.
    azure::storage::cloud_queue_message dequeued_message = queue.get_message();
    std::wcout << U("Dequeued message: ") << dequeued_message.content_as_string() << std::endl;

    // Delete the message.
    queue.delete_message(dequeued_message);

## <a name="how-to-leverage-additional-options-for-de-queuing-messages"></a>Kuidas: kasutada lisasuvandid tühistage andmebaasitõrge sõnumite jaoks
On kaks võimalust toomise järjekorda kaudu saate kohandada. Esmalt saate paketi sõnumite (kuni 32). Teiseks saate määrata rohkem või vähem nähtamatus ajalõpp, rohkem või vähem aega täielikult töödelda iga sõnumi, mis võimaldab teie kood. Järgmine kood näide kasutab **get_messages** meetodit saada 20 sõnumite ühe kõne. Seejärel töötleb iga sõnumi **puhul** tsükkel. Seda määrab ka nähtamatus ajalõpp viis minutit iga sõnumi kohta. Pange tähele, et samal ajal kõiki sõnumeid algab 5 minutit pärast 5 minutit on möödunud **get_messages**, mis tahes sõnumeid, mis ei kustutata kõne muutuvad nähtavaks uuesti.

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Dequeue some queue messages (maximum 32 at a time) and set their visibility timeout to
    // 5 minutes (300 seconds).
    azure::storage::queue_request_options options;
    azure::storage::operation_context context;

    // Retrieve 20 messages from the queue with a visibility timeout of 300 seconds.
    std::vector<azure::storage::cloud_queue_message> messages = queue.get_messages(20, std::chrono::seconds(300), options, context);

    for (auto it = messages.cbegin(); it != messages.cend(); ++it)
    {
        // Display the contents of the message.
        std::wcout << U("Get: ") << it->content_as_string() << std::endl;
    }

## <a name="how-to-get-the-queue-length"></a>Kuidas: saada järjekorra pikkus
Saate sõnumite hinnanguline järjekorda. **Download_attributes** meetodit küsib järjekorda teenuse järjekorda atribuute, sh sõnumi count toomiseks. **Approximate_message_count** meetodit saab sõnumite ligikaudse arvu järjekorras.

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // Fetch the queue attributes.
    queue.download_attributes();

    // Retrieve the cached approximate message count.
    int cachedMessageCount = queue.approximate_message_count();

    // Display number of messages.
    std::wcout << U("Number of messages in queue: ") << cachedMessageCount << std::endl;  

## <a name="how-to-delete-a-queue"></a>Kuidas: järjekorda kustutamine
Järjekorda ja kõik selles sisalduvad sõnumid kustutamiseks helistada järjekorda objekti **delete_queue_if_exists** meetod.

    // Retrieve storage account from connection-string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the queue client.
    azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

    // Retrieve a reference to a queue.
    azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

    // If the queue exists and delete it.
    queue.delete_queue_if_exists();  

## <a name="next-steps"></a>Järgmised sammud

Nüüd, kui olete õppinud järjekorda salvestusruumi põhitõdesid, järgige neid linke Lisateavet Azure Storage.

-   [Kuidas kasutada bloobimälu C++:](storage-c-plus-plus-how-to-use-blobs.md)
-   [Kuidas kasutada Tabelimälu C++:](storage-c-plus-plus-how-to-use-tables.md)
-   [Loendi Azure Storage ressursid c++](storage-c-plus-plus-enumeration.md)
-   [Salvestusruumi kliendi teek C++ teatmematerjalide](http://azure.github.io/azure-storage-cpp)
-   [Azure'i salvestusruumi dokumentatsioon](https://azure.microsoft.com/documentation/services/storage/)

