<properties
    pageTitle="Bloobimälu (objekti salvestusruumi) PHP kasutamise kohta | Microsoft Azure'i"
    description="Azure'i bloobimälu (objekti salvestusruumi) koos pilveteenuses talletada struktureerimata andmed."
    documentationCenter="php"
    services="storage"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="PHP"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>

# <a name="how-to-use-blob-storage-from-php"></a>Kuidas kasutada alates PHP bloobimälu

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Ülevaade

Azure'i bloobimälu on teenus, mis salvestab struktureerimata andmeid pilves objektide/plekid. Bloobimälu saab hoida mis tahes tüüpi teksti või kahendandmeid, näiteks dokumendi, media fail või rakenduse installer. Bloobimälu ka edaspidi objekti salvestusruumi.

Sellest juhendist näitab, kuidas teha levinud stsenaariumi Azure'i bloobimälu teenuse abil. Näidiste php kirjutada ja kasutada [Azure SDK php] [download]. Stsenaariumid, mis hõlmab kaasata **üles laadida**, **loetelu**, **allalaadimise**ja plekid **kustutamine** . Plekid kohta leiate lisateavet jaotisest [järgmised toimingud](#next-steps) .

[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-php-application"></a>PHP rakenduse loomine

Ainult PHP rakendus, mis kasutab teenust Azure'i bloobimälu loomise kohta on selle viitamine tunde Azure'i SDK php kaudu oma koodi. Mis tahes arengu tööriistade abil saate luua rakenduse, sh Notepadi.

Sellest juhendist kasutate teenuse funktsioone, mis saab kutsuda PHP rakenduses kohalikult või koodi, mis töötab Azure web roll, töötaja rolli või veebisaiti.

## <a name="get-the-azure-client-libraries"></a>Azure'i kliendi teegid hankimine

[AZURE.INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-to-access-the-blob-service"></a>Rakenduse bloobimälu teenuse konfigureerimine

Azure'i bloobimälu API-teenuse kasutamiseks peate:

1. Viide autoloader faili, kasutades [require_once] lause ja
2. Viide mis tahes tunnid, võite kasutada.

Järgmises näites on kujutatud, kuidas lisada autoloader fail ja **ServicesBuilder** klassi viidata.

> [AZURE.NOTE] Selles näites (ja muud selle artikli näited) Oletame, et olete installinud PHP kliendi teegid Azure helilooja kaudu. Kui installisite teekide käsitsi, peate viide on `WindowsAzure.php` autoloader fail.

    require_once 'vendor/autoload.php';
    use WindowsAzure\Common\ServicesBuilder;


Allpool toodud näidetes on `require_once` lause kuvatakse alati, kuid ainult vaja, nt käivitada klassid on viidatud.

## <a name="set-up-an-azure-storage-connection"></a>Azure'i salvestusruumi-ühenduse häälestamine

Väärtustada e Azure'i bloobimälu teenuse klient, peate esmalt sobiva ühendusstringi. Ühendusstringi bloobimälu teenuse vorming on:

Reaalajas teenuse kasutamiseks:

    DefaultEndpointsProtocol=[http|https];AccountName=[yourAccount];AccountKey=[yourKey]

Juurdepääsuks salvestusruumi emulaator:

    UseDevelopmentStorage=true


Luua mis tahes Azure'i teenus klient, peate kasutama **ServicesBuilder** klassi. Sa saad:

* Edastama ühendusstringi otse selle või
* Kontrollige mitme välised allikad ühendusstringi **CloudConfigurationManager (CCM)** abil:
    * Vaikimisi kaasas tugi ühe välise andmeallikaga - keskkonna muutujaid.
    * Saate lisada uue allikad pikendades **ConnectionStringSource** klassi.

Siin kirjeldatud näited kantakse otse ühendusstring.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;

    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);

## <a name="create-a-container"></a>Ümbris loomine

[AZURE.INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

**BlobRestProxy** objekti saate luua bloobimälu container **createContainer** meetod. Ümbris loomisel saate seada suvandid ümbris, kuid see on ei vaja. (Järgmises näites kujutatakse seadmine container metaandmete ja container pääsuloendi (ACL).)

    require_once 'vendor\autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Blob\Models\CreateContainerOptions;
    use MicrosoftAzure\Storage\Blob\Models\PublicAccessType;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create blob REST proxy.
    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


    // OPTIONAL: Set public access policy and metadata.
    // Create container options object.
    $createContainerOptions = new CreateContainerOptions();

    // Set public access policy. Possible values are
    // PublicAccessType::CONTAINER_AND_BLOBS and PublicAccessType::BLOBS_ONLY.
    // CONTAINER_AND_BLOBS:
    // Specifies full public read access for container and blob data.
    // proxys can enumerate blobs within the container via anonymous
    // request, but cannot enumerate containers within the storage account.
    //
    // BLOBS_ONLY:
    // Specifies public read access for blobs. Blob data within this
    // container can be read via anonymous request, but container data is not
    // available. proxys cannot enumerate blobs within the container via
    // anonymous request.
    // If this value is not specified in the request, container data is
    // private to the account owner.
    $createContainerOptions->setPublicAccess(PublicAccessType::CONTAINER_AND_BLOBS);

    // Set container metadata.
    $createContainerOptions->addMetaData("key1", "value1");
    $createContainerOptions->addMetaData("key2", "value2");

    try {
        // Create container.
        $blobRestProxy->createContainer("mycontainer", $createContainerOptions);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179439.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

Helistamiseks **setPublicAccess (PublicAccessType::CONTAINER\_ja\_PLEKID)** teeb andmete container ja bloobimälu anonüümse taotlusi kaudu. Helistamiseks **setPublicAccess(PublicAccessType::BLOBS_ONLY)** teeb ainult bloobimälu andmed anonüümse taotlusi kaudu. Container ACL-ide kohta leiate lisateavet teemast [määramine container ACL (REST API)][container-acl].

Tõrkekoodid bloobimälu teenuse kohta leiate lisateavet teemast [Bloobimälu teenuse tõrkekoodid][error-codes].

## <a name="upload-a-blob-into-a-container"></a>Ümbris laadida soovitud bloobimälu

Kasutage mõnda bloobimälu faili üleslaadimiseks **BlobRestProxy -> createBlockBlob** meetodit. See toiming loob soovitud bloobimälu, kui see pole olemas või kirjutatakse üle, kui see on. Järgmises näites kood eeldatakse, et ümbris on juba loodud ja kasutab [fopen] [ fopen] voo faili avamiseks.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create blob REST proxy.
    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


    $content = fopen("c:\myfile.txt", "r");
    $blob_name = "myblob";

    try {
        //Upload blob
        $blobRestProxy->createBlockBlob("mycontainer", $blob_name, $content);
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179439.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

Pange tähele, et eelmine näidis lisatud on bloobimälu nimega voo. Siiski on bloobimälu saab ka üles laadida stringi abil, nagu näiteks selle [faili\_saada\_sisu] [ file_get_contents] funktsioon. Seda teha, kasutades eelmises valimi, muuta `$content = fopen("c:\myfile.txt", "r");` et `$content = file_get_contents("c:\myfile.txt");`.

## <a name="list-the-blobs-in-a-container"></a>Loendi a ümbrises plekid

Loendi plekid nõus, kasutamine **foreach** tsükkel tsükkel tulemi kaudu **BlobRestProxy -> listBlobs** meetodit. Järgmine kood kuvatakse iga bloobimälu nimi on ümbrises väljundina ja kuvab selle URI brauseris.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create blob REST proxy.
    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


    try {
        // List blobs.
        $blob_list = $blobRestProxy->listBlobs("mycontainer");
        $blobs = $blob_list->getBlobs();

        foreach($blobs as $blob)
        {
            echo $blob->getName().": ".$blob->getUrl()."<br />";
        }
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179439.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }


## <a name="download-a-blob"></a>Laadige alla mõne bloobimälu

**BlobRestProxy -> getBlob** meetod on bloobimälu allalaadimiseks ja seejärel kõne **getContentStream** meetodit **GetBlobResult** tulemiks oleva objekti.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create blob REST proxy.
    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


    try {
        // Get blob.
        $blob = $blobRestProxy->getBlob("mycontainer", "myblob");
        fpassthru($blob->getContentStream());
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179439.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

Pange tähele, et Ülaltoodud näites saab on bloobimälu voo ressurssi (vaikekäitumine). Siiski saate kasutada funktsiooni [voo\_saada\_sisu] [ stream-get-contents] funktsioon tagastatud voo teisendamiseks string.

## <a name="delete-a-blob"></a>Kustutamine on bloobimälu

Mõne bloobimälu kustutamiseks edastama container nime ja bloobimälu nime **BlobRestProxy -> deleteBlob**.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create blob REST proxy.
    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


    try {
        // Delete blob.
        $blobRestProxy->deleteBlob("mycontainer", "myblob");
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179439.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

## <a name="delete-a-blob-container"></a>Bloobimälu container kustutamine

Lõpuks bloobimälu container kustutamiseks edastama container nime **BlobRestProxy -> deleteContainer**.

    require_once 'vendor/autoload.php';

    use WindowsAzure\Common\ServicesBuilder;
    use MicrosoftAzure\Storage\Common\ServiceException;

    // Create blob REST proxy.
    $blobRestProxy = ServicesBuilder::getInstance()->createBlobService($connectionString);


    try {
        // Delete container.
        $blobRestProxy->deleteContainer("mycontainer");
    }
    catch(ServiceException $e){
        // Handle exception based on error codes and messages.
        // Error codes and messages are here:
        // http://msdn.microsoft.com/library/azure/dd179439.aspx
        $code = $e->getCode();
        $error_message = $e->getMessage();
        echo $code.": ".$error_message."<br />";
    }

## <a name="next-steps"></a>Järgmised sammud

Nüüd, kui olete õppinud põhitõdesid Azure'i bloobimälu teenuse, järgige neid linke salvestusruumi keerukamate tööülesannete kohta.

- Külastage [Azure Storage meeskonna ajaveeb](http://blogs.msdn.com/b/windowsazurestorage/)
- Vt [PHP Blokeeri bloobimälu näide](https://github.com/WindowsAzure/azure-sdk-for-php-samples/blob/master/storage/BlockBlobExample.php).
- Vt [PHP lehe bloobimälu näide](https://github.com/WindowsAzure/azure-sdk-for-php-samples/blob/master/storage/PageBlobExample.php).
- [AzCopy käsurea kasuliku andmete edastamine](storage-use-azcopy.md)
 
Lisateabe saamiseks vt ka [PHP Arenduskeskus](/develop/php/).


[download]: http://go.microsoft.com/fwlink/?LinkID=252473
[container-acl]: http://msdn.microsoft.com/library/azure/dd179391.aspx
[error-codes]: http://msdn.microsoft.com/library/azure/dd179439.aspx
[file_get_contents]: http://php.net/file_get_contents
[require_once]: http://php.net/require_once
[fopen]: http://www.php.net/fopen
[stream-get-contents]: http://www.php.net/stream_get_contents
