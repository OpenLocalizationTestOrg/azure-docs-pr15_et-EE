<properties
    pageTitle="Kuidas kasutada C++: failide talletamine | Microsoft Azure'i"
    description="Salvestada faili andmed koos Azure failide talletamine pilveteenuses."
    services="storage"
    documentationCenter=".net"
    authors="seguler"
    manager="jahogg"
    editor="tysonn" />

<tags ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="seguler" />

# <a name="how-to-use-file-storage-from-c"></a>Kuidas kasutada C++: failide talletamine

[AZURE.INCLUDE [storage-selector-file-include](../../includes/storage-selector-file-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-files](../../includes/storage-try-azure-tools-files.md)]
[AZURE.INCLUDE [storage-file-overview-include](../../includes/storage-file-overview-include.md)]

## <a name="about-this-tutorial"></a>Selle õpetuse kohta

Selles õppetükis saate teada, kuidas Microsoft Azure'i faili salvestusteenus põhiliste toimingute. Kaudu kirjutatud C++ näidised, saate teada, kuidas luua osakud ja kataloogide, üles laadida, loendi ja failide kustutamine. Kui olete kasutanud Microsoft Azure'i faili salvestusteenus, läheb läbi järgnevate jaotiste mõisted on väga kasulik mõista näidised.

[AZURE.INCLUDE [storage-file-concepts-include](../../includes/storage-file-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-c-application"></a>C++ rakenduse loomine

Koostamiseks näidised, peate installige Azure'i salvestusruumi kliendi teek 2.4.0 C++. Saate ka olete loonud Azure storage konto.

Installida Azure salvestusruumi kliendi 2.4.0 C++, saate kasutada ühte järgmistest:

-   **Linux:** Järgige lehel [Azure'i salvestusruumi kliendi teegi jaoks C++ README](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) .

-   **Windowsi:** Klõpsake Visual Studio **Tööriistad &gt; Nugeti Package Manager &gt; Package Manager konsooli**. [Nugeti Package Manager konsooli](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) tippige järgmine käsk ja vajutage sisestusklahvi **ENTER**.

    Installi pakett wastorage

## <a name="set-up-your-application-to-use-file-storage"></a>Häälestada rakenduse kasutamine failide talletamine

Lisage järgmine sisaldavad ülaosas C++ faili, kuhu soovite kasutada Azure storage API-de failidele juurde pääseda.

    #include "was/storage_account.h"
    #include "was/file.h"

## <a name="set-up-an-azure-storage-connection-string"></a>Mõne Azure storage ühendusstringi häälestamine

Failide talletamine kasutamiseks peate Azure storage kontoga ühenduse loomiseks. Kõigepealt oleks konfigureerimine ühendusstring, mille kasutame salvestusruumi kontoga ühenduse loomiseks. Vaatame määratleda staatilise muutuja seda teha.

    // Define the connection-string with your values.
    const utility::string_t 
    storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));

## <a name="connecting-to-an-azure-storage-account"></a>Azure'i salvestusruumi konto ühenduse loomine

Saate tähistada salvestusruumi kontoteabe **cloud_storage_account** klassi. Ühendusstringi salvestusruumi salvestusruumi kontoteabe toomiseks saate **sõeluda** meetod.

    // Retrieve storage account from connection string. 
    azure::storage::cloud_storage_account storage_account = 
      azure::storage::cloud_storage_account::parse(storage_connection_string);

## <a name="how-to-create-a-share"></a>Kuidas: luua aktsia

Kõik failid ja kataloogid failide talletamine asuda ümbris nimega **Anna ühiskasutusse**. Salvestusruumi konto võib olla nii palju ühtlasi teie konto võimsus võimaldab. Osa ja selle sisu juurdepääsu saamiseks peate kasutama faili salvestusruumi kliendi.

    // Create the file storage client.
    azure::storage::cloud_file_client file_client = 
      storage_account.create_cloud_file_client();

Kasutades faili salvestusruumi klient, saate hankida siis ühiskasutusega viide.

    // Get a reference to the file share
    azure::storage::cloud_file_share share = 
      file_client.get_share_reference(_XPLATSTR("my-sample-share"));

Ühiskasutus loomiseks kasutage **cloud_file_share** objekti **create_if_not_exists** meetodit.

    if (share.create_if_not_exists()) { 
        std::wcout << U("New share created") << std::endl;  
    }

Selles etapis **ühiskasutus** hoiab viite nime **minu valimi ühiskasutus**.

## <a name="how-to-upload-a-file"></a>Kuidas: faili üles laadimine

Vähemalt, sisaldab Azure'i failikettal salvestusruumi lisamine juurkaust, kus failid võivad asuda. Selles jaotises saate teada, kuidas üles laadida faili kohaliku salvestusruumi juurkaust aktsia peale.

Kõigepealt faili üleslaadimisel on saada viide kataloogi, kus see peaks asuvad. Tehke seda, helistades **get_root_directory_reference** meetod ühiskasutus objekti.

    //Get a reference to the root directory for the share.
    azure::storage::cloud_file_directory root_dir = share.get_root_directory_reference();

Nüüd, kui teil on viide root kataloogi ühiskasutus, saate seda faili üles laadida. Selles näites on lisatud faili, teksti ja voo.

    // Upload a file from a stream.
    concurrency::streams::istream input_stream = 
      concurrency::streams::file_stream<uint8_t>::open_istream(_XPLATSTR("DataFile.txt")).get();

    azure::storage::cloud_file file1 = 
      root_dir.get_file_reference(_XPLATSTR("my-sample-file-1"));
    file1.upload_from_stream(input_stream);

    // Upload some files from text.
    azure::storage::cloud_file file2 = 
      root_dir.get_file_reference(_XPLATSTR("my-sample-file-2"));
    file2.upload_text(_XPLATSTR("more text"));

    // Upload a file from a file.
    azure::storage::cloud_file file4 = 
      root_dir.get_file_reference(_XPLATSTR("my-sample-file-3"));
    file4.upload_from_file(_XPLATSTR("DataFile.txt"));  

## <a name="how-to-create-a-directory"></a>Kuidas: luua kataloog

Samuti saate salvestusruumi paigutades failid alamkatalooge asemel kõik need juurkaustas sees. Azure'i faili salvestusteenus võimaldab teil luua nii palju kataloogide vastavalt teie konto võimaldab. Alljärgnev kood loob kataloogi **minu valimi directory** juurkaustas samuti mõnda alamkausta nimega **minu valimi alamkausta**.
    
    // Retrieve a reference to a directory
    azure::storage::cloud_file_directory directory = share.get_directory_reference(_XPLATSTR("my-sample-directory"));
    
    // Return value is true if the share did not exist and was successfully created.
    directory.create_if_not_exists();
    
    // Create a subdirectory.
    azure::storage::cloud_file_directory subdirectory = 
      directory.get_subdirectory_reference(_XPLATSTR("my-sample-subdirectory"));
    subdirectory.create_if_not_exists();

## <a name="how-to-list-files-and-directories-in-a-share"></a>Kuidas: loendi failide ja kataloogide ühiskasutamine

Failide ja kataloogide sees osa loendi saamiseks on lihtne teha helistades **list_files_and_directories** **cloud_file_directory** viide. Juurde pääseda suurel hulgal atribuudid ja tagastatud **list_file_and_directory_item**meetodid, peab kõne saada **cloud_file** objekti **list_file_and_directory_item.as_file** meetodit või saada **cloud_file_directory** objekti **list_file_and_directory_item.as_directory** meetod.

Järgmine kood näitab, kuidas tuua ja väljund URI iga üksuse juurkaustas osa.

    //Get a reference to the root directory for the share.
    azure::storage::cloud_file_directory root_dir = 
      share.get_root_directory_reference();
    
    // Output URI of each item.
    azure::storage::list_file_and_diretory_result_iterator end_of_results;
    
    for (auto it = directory.list_files_and_directories(); it != end_of_results; ++it)
    {
        if(it->is_directory())
        {
            ucout << "Directory: " << it->as_directory().uri().primary_uri().to_string() << std::endl;
        }
        else if (it->is_file())
        {
            ucout << "File: " << it->as_file().uri().primary_uri().to_string() << std::endl;
        }       
    }
    

## <a name="how-to-download-a-file"></a>Kuidas: faili allalaadimine

Failide allalaadimiseks esmalt tuua viite fail ja seejärel kõne **download_to_stream** meetodit voo objekti, mis saab siis ei kao kohaliku faili faili sisu edastada. Teise võimalusena saate alla laadida faili sisu kohaliku faili **download_to_file** meetod. **Download_text** meetodi abil saate tekstistringi kujul faili sisu alla laadida.

Järgmises näites kasutatakse **download_to_stream** ja **download_text** meetodite allalaadimine faile, mis loodi eelmiste jaotiste näitamiseks.

    // Download as text
    azure::storage::cloud_file text_file = 
      root_dir.get_file_reference(_XPLATSTR("my-sample-file-2"));
    utility::string_t text = text_file.download_text();
    ucout << "File Text: " << text << std::endl;
    
    // Download as a stream.
    azure::storage::cloud_file stream_file = 
      root_dir.get_file_reference(_XPLATSTR("my-sample-file-3"));
    
    concurrency::streams::container_buffer<std::vector<uint8_t>> buffer;
    concurrency::streams::ostream output_stream(buffer);
    stream_file.download_to_stream(output_stream);
    std::ofstream outfile("DownloadFile.txt", std::ofstream::binary);
    std::vector<unsigned char>& data = buffer.collection();
    outfile.write((char *)&data[0], buffer.size());
    outfile.close();

## <a name="how-to-delete-a-file"></a>Kuidas: faili kustutamine

Muu faili levinud Salvestustoiming on faili kustutamine. Järgmine kood kustutab fail nimega Minu-näidis-faili-3 salvestatud juurkaustas all.

    // Get a reference to the root directory for the share. 
    azure::storage::cloud_file_share share = 
      file_client.get_share_reference(_XPLATSTR("my-sample-share"));
    
    azure::storage::cloud_file_directory root_dir = 
      share.get_root_directory_reference();
    
    azure::storage::cloud_file file = 
      root_dir.get_file_reference(_XPLATSTR("my-sample-file-3"));
    
    file.delete_file_if_exists();

## <a name="how-to-delete-a-directory"></a>Kuidas: kustutada kataloog

Kausta kustutamine on lihtne ülesanne, kuigi tuleb märkida, et te ei saa kustutada faile endiselt sisaldav kaust või muud kataloogid.
    
    // Get a reference to the share.
    azure::storage::cloud_file_share share = 
      file_client.get_share_reference(_XPLATSTR("my-sample-share"));
    
    // Get a reference to the directory.
    azure::storage::cloud_file_directory directory = 
      share.get_directory_reference(_XPLATSTR("my-sample-directory"));
    
    // Get a reference to the subdirectory you want to delete.
    azure::storage::cloud_file_directory sub_directory =
      directory.get_subdirectory_reference(_XPLATSTR("my-sample-subdirectory"));
    
    // Delete the subdirectory and the sample directory.
    sub_directory.delete_directory_if_exists();
    
    directory.delete_directory_if_exists();

## <a name="how-to-delete-a-share"></a>Kuidas: osa kustutamine

Ühiskasutusega kustutamine on töö lõpetanud, helistades **delete_if_exists** meetod cloud_file_share objekti. Siin on proovi kood, mis teeb seda.
    
    // Get a reference to the share.
    azure::storage::cloud_file_share share = 
      file_client.get_share_reference(_XPLATSTR("my-sample-share"));
    
    // delete the share if exists
    share.delete_share_if_exists();

## <a name="set-the-maximum-size-for-a-file-share"></a>Määrake maksimaalne suurus faili ühiskasutus

Failide ühiskasutamine, gigabaiti määratavad kvoodi (või maksimaalne maht). Samuti saate vaadata, kui palju andmeid on praegu talletatud ühiskasutus.

Osa kvoodi seadmisega saate piirata suuruse talletatud ühiskasutusse andmise kohta. Kui failide ühiskasutus kogumaht ületab kvootide võrgukettal, siis kliendid ei saa olemasolevaid faile suurendamine või luua uusi faile, juhul, kui need failid on tühjad.

Järgmises näites on kuvatud, kuidas kontrollida praeguse kasutuse osa ja ühiskasutus piirmäära seadmise kohta.

    // Parse the connection string for the storage account.
    azure::storage::cloud_storage_account storage_account = 
      azure::storage::cloud_storage_account::parse(storage_connection_string);
    
    // Create the file client.
    azure::storage::cloud_file_client file_client = 
      storage_account.create_cloud_file_client();
    
    // Get a reference to the share.
    azure::storage::cloud_file_share share = 
      file_client.get_share_reference(_XPLATSTR("my-sample-share"));
    if (share.exists())
    {
        std::cout << "Current share usage: " << share.download_share_usage() << "/" << share.properties().quota();
    
        //This line sets the quota to 2560GB
        share.resize(2560);
    
        std::cout << "Quota increased: " << share.download_share_usage() << "/" << share.properties().quota();
    
    }

## <a name="generate-a-shared-access-signature-for-a-file-or-file-share"></a>Luua ühiskasutatava Accessi signatuuri, faili või faili ühiskasutusse andmine

Saate luua ühiskasutusega juurdepääsu signatuuri (SAS) failikettal või üksiku faili. Saate luua ühiskasutusega juurdepääsu poliitika failikettal haldamiseks ühiskasutusega juurdepääsu allkirjad. Ühiskasutusega juurdepääsu poliitika loomiseks on soovitatav, sest see pakub võimalust muude tühistamine, kui see peaks rikutud.

Järgmises näites ühiskasutatav ühiskasutusega juurdepääsu poliitika loob ja kasutab poliitika piiranguid SAS, klõpsake faili ühiskasutusse andmiseks.

    // Parse the connection string for the storage account.
    azure::storage::cloud_storage_account storage_account = 
      azure::storage::cloud_storage_account::parse(storage_connection_string);
    
    // Create the file client and get a reference to the share
    azure::storage::cloud_file_client file_client = 
      storage_account.create_cloud_file_client();
    
    azure::storage::cloud_file_share share = 
      file_client.get_share_reference(_XPLATSTR("my-sample-share"));
    
    if (share.exists())
    {
        // Create and assign a policy
        utility::string_t policy_name = _XPLATSTR("sampleSharePolicy");

        azure::storage::file_shared_access_policy sharedPolicy = 
          azure::storage::file_shared_access_policy();

        //set permissions to expire in 90 minutes
        sharedPolicy.set_expiry(utility::datetime::utc_now() + 
          utility::datetime::from_minutes(90));

        //give read and write permissions
        sharedPolicy.set_permissions(azure::storage::file_shared_access_policy::permissions::write | azure::storage::file_shared_access_policy::permissions::read);

        //set permissions for the share
        azure::storage::file_share_permissions permissions; 

        //retrieve the current list of shared access policies
        azure::storage::shared_access_policies<azure::storage::file_shared_access_policy> policies;
        
        //add the new shared policy
        policies.insert(std::make_pair(policy_name, sharedPolicy));

        //save the updated policy list
        permissions.set_policies(policies);
        share.upload_permissions(permissions);

        //Retrieve the root directory and file references
        azure::storage::cloud_file_directory root_dir = 
          share.get_root_directory_reference();
        azure::storage::cloud_file file = 
          root_dir.get_file_reference(_XPLATSTR("my-sample-file-1"));

        // Generate a SAS for a file in the share 
        //  and associate this access policy with it.       
        utility::string_t sas_token = file.get_shared_access_signature(sharedPolicy);
        
        // Create a new CloudFile object from the SAS, and write some text to the file.     
        azure::storage::cloud_file file_with_sas(azure::storage::storage_credentials(sas_token).transform_uri(file.uri().primary_uri()));
        utility::string_t text = _XPLATSTR("My sample content");        
        file_with_sas.upload_text(text);        
        
        //Download and print URL with SAS.
        utility::string_t downloaded_text = file_with_sas.download_text();      
        ucout << downloaded_text << std::endl;      
        ucout << azure::storage::storage_credentials(sas_token).transform_uri(file.uri().primary_uri()).to_string() << std::endl;
    
    }

Ühiskasutusega juurdepääsu signatuuride loomise ja kasutamise kohta leiate lisateavet teemast [Kaudu ühiskasutusse antud juurdepääs allkirjade (SAS)](storage-dotnet-shared-access-signature-part-1.md).

## <a name="next-steps"></a>Järgmised sammud

Lisateavet Azure Storage, saate nende ressursside:

-   [Salvestusruumi kliendi teek c++](https://github.com/Azure/azure-storage-cpp)

-   [Azure'i salvestusruumi Explorer](http://go.microsoft.com/fwlink/?LinkID=822673&clcid=0x409)

-   [Azure'i salvestusruumi dokumentatsioon](https://azure.microsoft.com/documentation/services/storage/)
