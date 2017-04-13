<properties
    pageTitle="Java: failide talletamine kasutamine | Microsoft Azure'i"
    description="Saate teada, kuidas kasutada teenust Azure fail üles, alla, loendi, ja failide kustutamine. Näidised kirjutatud Java."
    services="storage"
    documentationCenter="java"
    authors="robinsh"
    manager="carmonm"
    editor="tysonn" />

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robinsh"/>

# <a name="how-to-use-file-storage-from-java"></a>Kuidas kasutada Java: failide talletamine

[AZURE.INCLUDE [storage-selector-file-include](../../includes/storage-selector-file-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-files.md)]

## <a name="overview"></a>Ülevaade

Sellest juhendist saate teada, kuidas Microsoft Azure'i faili salvestusteenus põhiliste toimingute. Näidised kirjutatud Java kaudu saate teada, kuidas luua osakud ja kataloogide, üles laadida, loendi ja failide kustutamine. Kui olete kasutanud Microsoft Azure'i faili salvestusteenus, läheb läbi järgnevate jaotiste mõisted on väga kasulik mõista näidised.

[AZURE.INCLUDE [storage-file-concepts-include](../../includes/storage-file-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-java-application"></a>Java rakenduse loomine

Koostamiseks näidised, peate selle Java Kit (JDK) ja [Azure'i salvestusruumi SDK Java] []. Saate ka olete loonud Azure storage konto.

## <a name="setup-your-application-to-use-file-storage"></a>Häälestamine rakenduse kasutamine failide talletamine

Azure storage API-de kasutamiseks lisada Java faili, kuhu kavatsete salvestusteenus kaudu juurdepääsu algusse järgmine tekst.

    // Include the following imports to use blob APIs.
    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.file.*;

## <a name="setup-an-azure-storage-connection-string"></a>Häälestus on Azure storage ühendusstring

Failide talletamine kasutamiseks peate Azure storage kontoga ühenduse loomiseks. Kõigepealt oleks konfigureerimine ühendusstring, mille kasutame salvestusruumi kontoga ühenduse loomiseks. Vaatame määratleda staatiline muutuja seda teha.

    // Configure the connection-string with your values
    public static final String storageConnectionString =
        "DefaultEndpointsProtocol=http;" +
        "AccountName=your_storage_account_name;" +
        "AccountKey=your_storage_account_key";

> [AZURE.NOTE] Asendage your_storage_account_name ja your_storage_account_key tegelike väärtuste salvestusruumi konto jaoks.

## <a name="connecting-to-an-azure-storage-account"></a>Azure'i salvestusruumi konto ühenduse loomine

Salvestusruumi kontoga ühendada, peate kasutama **CloudStorageAccount** objekti, läbides selle meetodiga **sõeluda** ühendusstringi.

    // Use the CloudStorageAccount object to connect to your storage account
    try {
        CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);
    } catch (InvalidKeyException invalidKey) {
        // Handle the exception
    }

**CloudStorageAccount.parse** põhjustab mõne InvalidKeyException nii, et see panna, proovige/saagi ploki peate.

## <a name="how-to-create-a-share"></a>Kuidas: luua aktsia

Kõik failid ja kataloogid failide talletamine asuda ümbris nimega **Anna ühiskasutusse**. Salvestusruumi konto võib olla nii palju ühtlasi teie konto võimsus võimaldab. Osa ja selle sisu juurdepääsu saamiseks peate kasutama faili salvestusruumi kliendi.

    // Create the file storage client.
    CloudFileClient fileClient = storageAccount.createCloudFileClient();

Kasutades faili salvestusruumi klient, saate hankida siis ühiskasutusega viide.

    // Get a reference to the file share
    CloudFileShare share = fileClient.getShareReference("sampleshare");

Ühiskasutus tegelikult loomiseks kasutage CloudFileShare objekti **createIfNotExists** meetodit.

    if (share.createIfNotExists()) {
        System.out.println("New share created");
    }

Selles etapis **ühiskasutus** hoiab viide **sampleshare**nime.

## <a name="how-to-upload-a-file"></a>Kuidas: faili üles laadimine

Azure'i salvestusruumi failikettal sisaldab vähemalt, root kataloogi, kus failid võivad asuda. Selles jaotises saate teada, kuidas üles laadida faili kohaliku salvestusruumi juurkaust aktsia peale.

Kõigepealt faili üleslaadimisel on saada viide kataloogi, kus see peaks asuvad. Tehke seda, helistades **getRootDirectoryReference** meetod ühiskasutus objekti.

    //Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.getRootDirectoryReference();

Nüüd, kui teil on viide root kataloogi ühiskasutus, saate laadida faili abil järgmine kood.

    // Define the path to a local file.
    final String filePath = "C:\\temp\\Readme.txt";

    CloudFile cloudFile = rootDir.getFileReference("Readme.txt");
    cloudFile.uploadFromFile(filePath);

## <a name="how-to-create-a-directory"></a>Kuidas: luua kataloog

Samuti saate salvestusruumi paigutades faile sub kataloogide asemel kõik need juurkaustas sees. Azure'i faili salvestusteenus võimaldab teil luua nii palju kataloogide vastavalt teie konto võimaldab. Alljärgnev kood loob **sampledir** jaotises juurkaustas sub kataloogi.

    //Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.getRootDirectoryReference();

    //Get a reference to the sampledir directory
    CloudFileDirectory sampleDir = rootDir.getDirectoryReference("sampledir");

    if (sampleDir.createIfNotExists()) {
        System.out.println("sampledir created");
    } else {
        System.out.println("sampledir already exists");
    }

## <a name="how-to-list-files-and-directories-in-a-share"></a>Kuidas: loendi failide ja kataloogide ühiskasutamine

Failide ja kataloogide sees osa loendi saamiseks on lihtne teha helistades **listFilesAndDirectories** CloudFileDirectory viide. Meetod tagastab loendi ListFileItem objektid, mida te saate saades. Näiteks järgmine kood nimekirja failide ja kataloogide juurkaustas sees.

    //Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.getRootDirectoryReference();

    for ( ListFileItem fileItem : rootDir.listFilesAndDirectories() ) {
        System.out.println(fileItem.getUri());
    }


## <a name="how-to-download-a-file"></a>Kuidas: faili allalaadimine

Üks sagedamini toimingute sooritamist vastu failide talletamine on faile alla laadida. Järgmises näites kood SampleFile.txt allalaaditavate failide ja kuvab selle sisu.

    //Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.getRootDirectoryReference();

    //Get a reference to the directory that contains the file
    CloudFileDirectory sampleDir = rootDir.getDirectoryReference("sampledir");

    //Get a reference to the file you want to download
    CloudFile file = sampleDir.getFileReference("SampleFile.txt");

    //Write the contents of the file to the console.
    System.out.println(file.downloadText());

## <a name="how-to-delete-a-file"></a>Kuidas: faili kustutamine

Muu faili levinud Salvestustoiming on faili kustutamine. Järgmine kood kustutab faili nimega SampleFile.txt talletatud **sampledir**kataloogi.


    // Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.getRootDirectoryReference();

    // Get a reference to the directory where the file to be deleted is in
    CloudFileDirectory containerDir = rootDir.getDirectoryReference("sampledir");

    String filename = "SampleFile.txt"
    CloudFile file;

    file = containerDir.getFileReference(filename)
    if ( file.deleteIfExists() ) {
        System.out.println(filename + " was deleted");
    }


## <a name="how-to-delete-a-directory"></a>Kuidas: kustutada kataloog

Kausta kustutamine on üsna lihtne ülesanne, kuigi tuleb märkida, et te ei saa kustutada faile endiselt sisaldav kaust või muud kataloogid.

    // Get a reference to the root directory for the share.
    CloudFileDirectory rootDir = share.getRootDirectoryReference();

    // Get a reference to the directory you want to delete
    CloudFileDirectory containerDir = rootDir.getDirectoryReference("sampledir");

    // Delete the directory
    if ( containerDir.deleteIfExists() ) {
        System.out.println("Directory deleted");
    }


## <a name="how-to-delete-a-share"></a>Kuidas: osa kustutamine

Ühiskasutusega kustutamine on töö lõpetanud, helistades **deleteIfExists** meetod CloudFileShare objekti. Siin on proovi kood, mis teeb seda.

    try
    {
        // Retrieve storage account from connection-string.
        CloudStorageAccount storageAccount = CloudStorageAccount.parse(storageConnectionString);

        // Create the file client.
       CloudFileClient fileClient = storageAccount.createCloudFileClient();

       // Get a reference to the file share
       CloudFileShare share = fileClient.getShareReference("sampleshare");

       if (share.deleteIfExists()) {
           System.out.println("sampleshare deleted");
       }
    } catch (Exception e) {
        e.printStackTrace();
    }

## <a name="next-steps"></a>Järgmised sammud

Kui soovite lisateavet muude Azure storage API-d, järgige neid linke.

- [Java Arenduskeskus](http://azure.microsoft.com/develop/java/)
- [Azure'i salvestusruumi SDK Java](https://github.com/azure/azure-storage-java)
- [Azure'i salvestusruumi SDK Androidi jaoks](https://github.com/azure/azure-storage-android)
- [Azure'i salvestusruumi kliendi SDK viide](http://dl.windowsazure.com/storage/javadoc/)
- [Azure'i salvestusruumi teenuste REST API-ga](https://msdn.microsoft.com/library/azure/dd179355.aspx)
- [Azure'i salvestusruumi meeskonna ajaveeb](http://blogs.msdn.com/b/windowsazurestorage/)
- [AzCopy käsurea kasuliku andmete edastamine](storage-use-azcopy.md)
