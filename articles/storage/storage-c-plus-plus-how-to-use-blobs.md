<properties
    pageTitle="Kasutamise bloobimälu (objekti salvestusruumi) C++ | Microsoft Azure'i"
    description="Azure'i bloobimälu (objekti salvestusruumi) koos pilveteenuses talletada struktureerimata andmed."
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

# <a name="how-to-use-blob-storage-from-c"></a>Kuidas kasutada bloobimälu C++:  

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Ülevaade

Azure'i bloobimälu on teenus, mis salvestab struktureerimata andmeid pilves objektide/plekid. Bloobimälu saab hoida mis tahes tüüpi teksti või binaarandmete, nt dokumenti, media fail või rakenduse installer. Bloobimälu ka edaspidi objekti salvestusruumi.

Sellest juhendist näitab, kuidas teha levinud stsenaariumi salvestusteenus Azure'i bloobimälu abil. Näidised kirjutatakse C++ ja [Azure salvestusruumi kliendi teek C++ jaoks](http://github.com/Azure/azure-storage-cpp/blob/master/README.md)kasutada. Stsenaariumid, mis hõlmab kaasata **üles laadida**, **loetelu**, **allalaadimise**ja plekid **kustutamine** .  

>[AZURE.NOTE] Sellest juhendist eesmärgid Azure'i salvestusruumi kliendi teek c++ versioon 1.0.0 ja kohal. Soovitatav versioon on salvestusruumi kliendi teek 2.2.0, mis on saadaval [Nugeti](http://www.nuget.org/packages/wastorage) või [GitHub](https://github.com/Azure/azure-storage-cpp)kaudu.

[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]
[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-c-application"></a>C++ rakenduse loomine
Sellest juhendist kasutate salvestusruumi funktsioonid, mida saab käivitada C++ rakenduses.  

Selleks, peate installige Azure'i salvestusruumi kliendi teek C++ ja Azure tellimuse Azure storage konto loomiseks.   

Azure'i salvestusruumi kliendi teek c++ installimiseks saate järgmistest viisidest:

-   **Linux:** Järgige lehel [Azure'i salvestusruumi kliendi teegi jaoks C++ README](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) .  
-   **Windowsi:** Klõpsake Visual Studio **Tööriistad > Nugeti Package Manager > Package Manager konsooli**. [Nugeti Package Manager konsooli](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) tippige järgmine käsk ja vajutage sisestusklahvi **ENTER**.  

        Install-Package wastorage

## <a name="configure-your-application-to-access-blob-storage"></a>Rakenduse bloobimälu juurdepääsu konfigureerimine  
Lisage järgmine sisaldavad C++ plekid Azure storage API-de abil soovitud fail üles.  

    #include "was/storage_account.h"
    #include "was/blob.h"

## <a name="setup-an-azure-storage-connection-string"></a>Häälestus on Azure storage ühendusstring
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

Järgmiseks saada viide **cloud_blob_client** klassi see võimaldab teil laadida objekte, mis tähistavad ümbriste ja talletatud bloobimälu salvestusteenus plekid. Järgmine kood loob **cloud_blob_client** objekti me tuua kohal salvestusruumi konto objekti abil:  

    // Create the blob client.
    azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();  

## <a name="how-to-create-a-container"></a>Kuidas: ümbris loomine

[AZURE.INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

Selles näites kirjeldatakse, kuidas luua ümbris, kui see pole juba olemas:  

    try
    {
        // Retrieve storage account from connection string.
        azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

        // Create the blob client.
        azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

        // Retrieve a reference to a container.
        azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

        // Create the container if it doesn't already exist.
        container.create_if_not_exists();
    }
    catch (const std::exception& e)
    {
        std::wcout << U("Error: ") << e.what() << std::endl;
    }  

Vaikimisi uus ümbris on privaatne ja määrake salvestusruumi kiirklahv see ümbris plekid alla. Kui soovite teha failid (plekid) s.o ümbris, mille jooksul saadaval kõigile, saate määrata container avalikuks abil järgmine kood:  

    // Make the blob container publicly accessible.
    azure::storage::blob_container_permissions permissions;
    permissions.set_public_access(azure::storage::blob_container_public_access_type::blob);
    container.upload_permissions(permissions);  

Kõik Interneti näevad plekid avaliku ümbrises, kuid saate muuta või kustutada ainult juhul, kui teil on asjakohane juurdepääsu võti.  

## <a name="how-to-upload-a-blob-into-a-container"></a>Kuidas: üleslaadimine on bloobimälu üheks ümbris
Azure'i bloobimälu toetab Blokeeri plekid ja lehe plekid. Enamikul juhtudel, Blokeeri bloobimälu on soovitatav kasutada tüüp.  

Faili üleslaadimine Blokeeri bloobimälu saada container viide ja selle abil saate blokeerida bloobimälu viide. Kui olete bloobimälu viide, saate mis tahes andmevoo see üles laadida, helistades **upload_from_stream** meetod. Kui see pole varem olemas või üle kirjutada, kui see on olemas, loob see toiming on bloobimälu. Järgmises näites kujutatakse laadida soovitud bloobimälu ümbris ja eeldab, et ümbris on juba loodud.  

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the blob client.
    azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

    // Retrieve a reference to a previously created container.
    azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

    // Retrieve reference to a blob named "my-blob-1".
    azure::storage::cloud_block_blob blockBlob = container.get_block_blob_reference(U("my-blob-1"));

    // Create or overwrite the "my-blob-1" blob with contents from a local file.
    concurrency::streams::istream input_stream = concurrency::streams::file_stream<uint8_t>::open_istream(U("DataFile.txt")).get();
    blockBlob.upload_from_stream(input_stream);
    input_stream.close().wait();

    // Create or overwrite the "my-blob-2" and "my-blob-3" blobs with contents from text.
    // Retrieve a reference to a blob named "my-blob-2".
    azure::storage::cloud_block_blob blob2 = container.get_block_blob_reference(U("my-blob-2"));
    blob2.upload_text(U("more text"));

    // Retrieve a reference to a blob named "my-blob-3".
    azure::storage::cloud_block_blob blob3 = container.get_block_blob_reference(U("my-directory/my-sub-directory/my-blob-3"));
    blob3.upload_text(U("other text"));  

Teise võimalusena saate faili üleslaadimiseks Blokeeri bloobimälu **upload_from_file** meetod.

## <a name="how-to-list-the-blobs-in-a-container"></a>Kuidas: loendi a ümbrises plekid
Loendi plekid nõus, kõigepealt container viide. Seejärel saate tuua plekid ja/või kataloogide kogumahu kõigepealt ümbrist **list_blobs** meetod. Juurde pääseda suurel hulgal atribuudid ja tagastatud **list_blob_item**meetodid, peab kõne saada **cloud_blob** objekti **list_blob_item.as_blob** meetodit või saada cloud_blob_directory objekti **list_blob.as_directory** meetod. Järgmine kood näitab, kuidas tuua ja iga üksuse **minu valimi container** ümbrises URI väljund:

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the blob client.
    azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

    // Retrieve a reference to a previously created container.
    azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

    // Output URI of each item.
    azure::storage::list_blob_item_iterator end_of_results;
    for (auto it = container.list_blobs(); it != end_of_results; ++it)
    {
        if (it->is_blob())
        {
            std::wcout << U("Blob: ") << it->as_blob().uri().primary_uri().to_string() << std::endl;
        }
        else
        {
            std::wcout << U("Directory: ") << it->as_directory().uri().primary_uri().to_string() << std::endl;
        }
    }

Loetelu toimingute kohta leiate lisateavet teemast [Loendi Azure'i salvestusruumi ressursid c++](storage-c-plus-plus-enumeration.md).

## <a name="how-to-download-blobs"></a>Kuidas: plekid allalaadimine
Plekid allalaadimiseks esmalt tuua bloobimälu viide ja seejärel helistage **download_to_stream** meetod. Järgmises näites kasutatakse **download_to_stream** meetodit voo objekti, mida saate siis jää püsima kohaliku faili bloobimälu sisu edastada.  

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the blob client.
    azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

    // Retrieve a reference to a previously created container.
    azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

    // Retrieve reference to a blob named "my-blob-1".
    azure::storage::cloud_block_blob blockBlob = container.get_block_blob_reference(U("my-blob-1"));

    // Save blob contents to a file.
    concurrency::streams::container_buffer<std::vector<uint8_t>> buffer;
    concurrency::streams::ostream output_stream(buffer);
    blockBlob.download_to_stream(output_stream);

    std::ofstream outfile("DownloadBlobFile.txt", std::ofstream::binary);
    std::vector<unsigned char>& data = buffer.collection();

    outfile.write((char *)&data[0], buffer.size());
    outfile.close();  

Teise võimalusena saate alla laadida faili sisu lisamine bloobimälu **download_to_file** meetod.
Lisaks saate **download_text** meetod on bloobimälu tekstistringi kujul sisu alla laadida.  

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the blob client.
    azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

    // Retrieve a reference to a previously created container.
    azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

    // Retrieve reference to a blob named "my-blob-2".
    azure::storage::cloud_block_blob text_blob = container.get_block_blob_reference(U("my-blob-2"));

    // Download the contents of a blog as a text string.
    utility::string_t text = text_blob.download_text();

## <a name="how-to-delete-blobs"></a>Kuidas: plekid kustutamine
Mõne bloobimälu kustutamiseks esmalt saada bloobimälu viide ja seejärel helistage **delete_blob** meetod seda.  

    // Retrieve storage account from connection string.
    azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

    // Create the blob client.
    azure::storage::cloud_blob_client blob_client = storage_account.create_cloud_blob_client();

    // Retrieve a reference to a previously created container.
    azure::storage::cloud_blob_container container = blob_client.get_container_reference(U("my-sample-container"));

    // Retrieve reference to a blob named "my-blob-1".
    azure::storage::cloud_block_blob blockBlob = container.get_block_blob_reference(U("my-blob-1"));

    // Delete the blob.
    blockBlob.delete_blob();

## <a name="next-steps"></a>Järgmised sammud
Nüüd, kui olete õppinud bloobimälu põhitõdesid, järgige neid linke Lisateavet Azure Storage.  

-   [Kuidas kasutada järjekorda salvestusruumi C++:](storage-c-plus-plus-how-to-use-queues.md)
-   [Kuidas kasutada Tabelimälu C++:](storage-c-plus-plus-how-to-use-tables.md)
-   [Loendi Azure Storage ressursid c++](storage-c-plus-plus-enumeration.md)
-   [Salvestusruumi kliendi teek C++ teatmematerjalide](http://azure.github.io/azure-storage-cpp)
-   [Azure'i salvestusruumi dokumentatsioon](https://azure.microsoft.com/documentation/services/storage/)
- [Andmete AzCopy käsurea kasuliku](storage-use-azcopy.md)
