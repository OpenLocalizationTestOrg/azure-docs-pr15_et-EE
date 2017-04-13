<properties
    pageTitle="Azure'i bloobimälu (objekti salvestusruumi) kasutades .NET alustamine | Microsoft Azure'i"
    description="Azure'i bloobimälu (objekti salvestusruumi) koos pilveteenuses talletada struktureerimata andmed."
    services="storage"
    documentationCenter=".net"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="get-started-with-azure-blob-storage-using-net"></a>Azure'i bloobimälu .NET kasutamise alustamine

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Ülevaade

Azure'i bloobimälu on teenus, mis salvestab struktureerimata andmeid pilves objektide/plekid. Bloobimälu saab hoida mis tahes tüüpi teksti või kahendandmeid, näiteks dokumendi, media fail või rakenduse installer. Bloobimälu ka edaspidi objekti salvestusruumi.

### <a name="about-this-tutorial"></a>Selle õpetuse kohta

Selle õpetuse näitab, kuidas kirjutada mõne levinud stsenaariumi, kasutades Azure'i bloobimälu .net-i koodi. Stsenaariumid, mis hõlmab kaasata üles laadida, loetelu, allalaadimise ja kustutamise plekid.

**Hinnanguline aega:** 45 minutit

**Prerequisities:**

- [Microsoft Visual Studio](https://www.visualstudio.com/en-us/visual-studio-homepage-vs.aspx)
- [Azure'i salvestusruumi kliendi teek .net-i jaoks](https://www.nuget.org/packages/WindowsAzure.Storage/)
- [Azure'i Configuration Manager .net-i jaoks](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/)
- [Azure storage konto](storage-create-storage-account.md#create-a-storage-account)


[AZURE.INCLUDE [storage-dotnet-client-library-version-include](../../includes/storage-dotnet-client-library-version-include.md)]

### <a name="more-samples"></a>Veel näidised

Täiendavad näiteid bloobimälu abil leiate [Azure'i bloobimälu .NET-is töötamise alustamine](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/). Saate alla laadida rakenduse valimi ja käivitage see või sirvida kood github.


[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

[AZURE.INCLUDE [storage-development-environment-include](../../includes/storage-development-environment-include.md)]

### <a name="add-namespace-declarations"></a>Nimeruumi deklaratsiooni lisamine

Lisage järgmine `using` laused ülaosas olevat `program.cs` faili:

    using Microsoft.Azure; // Namespace for CloudConfigurationManager
    using Microsoft.WindowsAzure.Storage; // Namespace for CloudStorageAccount
    using Microsoft.WindowsAzure.Storage.Blob; // Namespace for Blob storage types

### <a name="parse-the-connection-string"></a>Ühendusstringi sõeluda

[AZURE.INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]

### <a name="create-the-blob-service-client"></a>Kliendi teenuse bloobimälu loomine

**CloudBlobClient** klassi abil saate tuua ümbriste ja plekid talletatud bloobimälu. Siin on üks viis, kuidas luua teenuse kliendi:

    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

Nüüd olete valmis kirjutada koodi, mis loeb andmeid ja kirjutab bloobimälu andmeid.

## <a name="create-a-container"></a>Ümbris loomine

[AZURE.INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

Selles näites kirjeldatakse, kuidas luua ümbris, kui see pole juba olemas:

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve a reference to a container.
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Create the container if it doesn't already exist.
    container.CreateIfNotExists();

Vaikimisi on privaatne, tähendab, et peate määrama salvestusruumi kiirklahv plekid alla see ümbris uus ümbris. Kui soovite faili ümbris kõigile kättesaadavaks teha, saate häälestada container avalikuks abil järgmine kood:

    container.SetPermissions(
        new BlobContainerPermissions { PublicAccess = BlobContainerPublicAccessType.Blob });

Kõik Interneti näevad plekid avaliku ümbrises, kuid saate muuta või kustutada ainult siis, kui teil on sobiv konto kiirklahv või ühiskasutusega juurdepääsu allkirja.

## <a name="upload-a-blob-into-a-container"></a>Ümbris laadida soovitud bloobimälu

Azure'i bloobimälu toetab plokk plekid ja lehe plekid.  Enamikul juhtudel, Blokeeri bloobimälu on soovitatav kasutada tüüp.

Faili üleslaadimine Blokeeri bloobimälu saada container viide ja selle abil saate blokeerida bloobimälu viide. Kui olete bloobimälu viide, saate mis tahes andmevoo see üles laadida, helistades **UploadFromStream** meetod. Kui see pole varem olemas või üle kirjutada, kui see on olemas, loob see toiming on bloobimälu.

Järgmises näites kujutatakse laadida soovitud bloobimälu ümbris ja eeldab, et ümbris on juba loodud.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Retrieve reference to a blob named "myblob".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

    // Create or overwrite the "myblob" blob with contents from a local file.
    using (var fileStream = System.IO.File.OpenRead(@"path\myfile"))
    {
        blockBlob.UploadFromStream(fileStream);
    }

## <a name="list-the-blobs-in-a-container"></a>Loendi a ümbrises plekid

Loendi plekid nõus, kõigepealt container viide. Seejärel saate tuua plekid ja/või kataloogide kogumahu kõigepealt ümbrist **ListBlobs** meetod. Juurdepääs suurel hulgal atribuudid ja meetodite jaoks tagastatud **IListBlobItem**, peab see **CloudBlockBlob**, **CloudPageBlob**või **CloudBlobDirectory** objekti enamus.  Kui tüüp on tundmatu, saate määrata, milline see oma hääle tüüp sisse.  Järgmine kood näitab, kuidas tuua ja iga üksuse URI väljund on `photos` container:

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference("photos");

    // Loop over items within the container and output the length and URI.
    foreach (IListBlobItem item in container.ListBlobs(null, false))
    {
        if (item.GetType() == typeof(CloudBlockBlob))
        {
            CloudBlockBlob blob = (CloudBlockBlob)item;

            Console.WriteLine("Block blob of length {0}: {1}", blob.Properties.Length, blob.Uri);

        }
        else if (item.GetType() == typeof(CloudPageBlob))
        {
            CloudPageBlob pageBlob = (CloudPageBlob)item;

            Console.WriteLine("Page blob of length {0}: {1}", pageBlob.Properties.Length, pageBlob.Uri);

        }
        else if (item.GetType() == typeof(CloudBlobDirectory))
        {
            CloudBlobDirectory directory = (CloudBlobDirectory)item;

            Console.WriteLine("Directory: {0}", directory.Uri);
        }
    }

Nagu eespool näidatud, saate nime plekid tee teavet nende nimed. See loob virtuaalse kataloogi struktuuri, mis saate korraldada ja läbida nii, nagu teeksite traditsiooniline failisüsteemis. Pange tähele, et kataloogi struktuuri on virtuaalse ainult - bloobimälu saadaval ainult ressursid on ümbriste ja plekid. Kuid pakub salvestusruumi kliendi teek **CloudBlobDirectory** objekti virtuaalkaust ja lihtsustada töötamine plekid, mis on korraldatud sel viisil.

Näiteks, võtke arvesse järgmist määramine Blokeeri plekid ümbrises, nimega `photos`:

    photo1.jpg
    2010/architecture/description.txt
    2010/architecture/photo3.jpg
    2010/architecture/photo4.jpg
    2011/architecture/photo5.jpg
    2011/architecture/photo6.jpg
    2011/architecture/description.txt
    2011/photo7.jpg

Kui helistate **ListBlobs** 'fotod' ümbris (nagu ülaltoodud valimi), tagastatakse hierarhilise loendi. See sisaldab nii **CloudBlobDirectory** ja **CloudBlockBlob** objekte, mis tähistab kataloogid ja plekid ümbrises. Selle tulemusena tekkivad väljund näeb välja selline:

    Directory: https://<accountname>.blob.core.windows.net/photos/2010/
    Directory: https://<accountname>.blob.core.windows.net/photos/2011/
    Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg


Soovi korral saate määrata **ListBlobs** meetodi parameetri **UseFlatBlobListing** väärtuseks **true**. Sel juhul tagastatakse iga bloobimälu ümbrises **CloudBlockBlob** objektina. Kõne **ListBlobs** juurde naasmiseks tasapinnalise kirjet näeb välja umbes järgmine:

    // Loop over items within the container and output the length and URI.
    foreach (IListBlobItem item in container.ListBlobs(null, true))
    {
       ...
    }

ja tulemus näeb välja selline:

    Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2010/architecture/description.txt
    Block blob of length 314618: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo3.jpg
    Block blob of length 522713: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo4.jpg
    Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2011/architecture/description.txt
    Block blob of length 419048: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo5.jpg
    Block blob of length 506388: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo6.jpg
    Block blob of length 399751: https://<accountname>.blob.core.windows.net/photos/2011/photo7.jpg
    Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg


## <a name="download-blobs"></a>Laadige alla plekid

Plekid allalaadimiseks esmalt tuua bloobimälu viide ja seejärel helistage **DownloadToStream** meetod. Järgmises näites kasutatakse **DownloadToStream** meetodit voo objekti, mida saate siis jää püsima kohaliku faili bloobimälu sisu edastada.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Retrieve reference to a blob named "photo1.jpg".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("photo1.jpg");

    // Save blob contents to a file.
    using (var fileStream = System.IO.File.OpenWrite(@"path\myfile"))
    {
        blockBlob.DownloadToStream(fileStream);
    }

**DownloadToStream** meetodi abil soovitud bloobimälu tekstistringi kujul sisu alla laadida.

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Retrieve reference to a blob named "myblob.txt"
    CloudBlockBlob blockBlob2 = container.GetBlockBlobReference("myblob.txt");

    string text;
    using (var memoryStream = new MemoryStream())
    {
        blockBlob2.DownloadToStream(memoryStream);
        text = System.Text.Encoding.UTF8.GetString(memoryStream.ToArray());
    }

## <a name="delete-blobs"></a>Plekid kustutamine

Mõne bloobimälu kustutamiseks esmalt saada bloobimälu viide ja seejärel kõne seda meetodit **kustutamine** .

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Retrieve reference to a blob named "myblob.txt".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob.txt");

    // Delete the blob.
    blockBlob.Delete();


## <a name="list-blobs-in-pages-asynchronously"></a>Loendi plekid lehtedel asünkroonselt

Kui teil on suur hulk plekid loetletakse või soovite reguleerida naasete ühe kirjet toiminguga tulemite arv, saate loetleda plekid tulemuste lehtedel. Selles näites kujutatakse tulemeid lehtedel asünkroonselt, nii, et täitmise on blokeeritud ooteajal suure hulga tulemite tagastamiseks.

Selles näites on kujutatud tasapinnalise bloobimälu, milles on loetletud, kuid seadmisega saate teha ka hierarhilise loendi, kuvatakse `useFlatBlobListing` parameeter **ListBlobsSegmentedAsync** meetodi `false`.

Kuna valimi meetodit helistab asünkroonne sisestusmeetod, see tuleb koos sissejuhatava põhjendusega on `async` märksõna ja see peab andma **tööülesande** objekti. Ootame märksõna **ListBlobsSegmentedAsync** meetodi jaoks on määratud peatab täitmise valimi meetodi kuni kirjet tööülesanne on lõpule viidud.

    async public static Task ListBlobsSegmentedInFlatListing(CloudBlobContainer container)
    {
        //List blobs to the console window, with paging.
        Console.WriteLine("List blobs in pages:");

        int i = 0;
        BlobContinuationToken continuationToken = null;
        BlobResultSegment resultSegment = null;

        //Call ListBlobsSegmentedAsync and enumerate the result segment returned, while the continuation token is non-null.
        //When the continuation token is null, the last page has been returned and execution can exit the loop.
        do
        {
            //This overload allows control of the page size. You can return all remaining results by passing null for the maxResults parameter,
            //or by calling a different overload.
            resultSegment = await container.ListBlobsSegmentedAsync("", true, BlobListingDetails.All, 10, continuationToken, null, null);
            if (resultSegment.Results.Count<IListBlobItem>() > 0) { Console.WriteLine("Page {0}:", ++i); }
            foreach (var blobItem in resultSegment.Results)
            {
                Console.WriteLine("\t{0}", blobItem.StorageUri.PrimaryUri);
            }
            Console.WriteLine();

            //Get the continuation token.
            continuationToken = resultSegment.ContinuationToken;
        }
        while (continuationToken != null);
    }

## <a name="writing-to-an-append-blob"></a>Kui soovite mõne lisa bloobimälu kirjutamine

Mõne lisa bloobimälu on bloobimälu, versiooniga kasutusele uut tüüpi 5.x Azure storage kliendi teegi .net-i jaoks. Mõne lisa bloobimälu on optimeeritud lisa toiminguid, nagu logimine. Blokeeri bloobimälu, nt mõni lisa bloobimälu hulka kuuluvad plokid, kuid uue ploki lisamisel kuvatakse lisa bloobimälu on alati funktsiooni bloobimälu lõppu lisatud. Ei saa värskendada või kustutada mõne olemasoleva plokk on lisa bloobimälu. Blokeeri ID-de jaoks mõne lisa bloobimälu on kokku puutunud, samuti nagu Blokeeri bloobimälu.

Iga plokk on lisa bloobimälu võib olla erineva suuruse, kuni 4 MB, ja ka lisa bloobimälu võib sisaldada kuni 50 000 plokid. Seetõttu on maksimaalne maht on lisa bloobimälu veidi rohkem kui 195 GB (4 MB X 50,000 plokid).

Järgmises näites luuakse uus lisa bloobimälu ja lisab mõned andmed, simuleerida lihtne logimine toiming.

    //Parse the connection string for the storage account.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));

    //Create service client for credentialed access to the Blob service.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    //Get a reference to a container.
    CloudBlobContainer container = blobClient.GetContainerReference("my-append-blobs");

    //Create the container if it does not already exist.
    container.CreateIfNotExists();

    //Get a reference to an append blob.
    CloudAppendBlob appendBlob = container.GetAppendBlobReference("append-blob.log");

    //Create the append blob. Note that if the blob already exists, the CreateOrReplace() method will overwrite it.
    //You can check whether the blob exists to avoid overwriting it by using CloudAppendBlob.Exists().
    appendBlob.CreateOrReplace();

    int numBlocks = 10;

    //Generate an array of random bytes.
    Random rnd = new Random();
    byte[] bytes = new byte[numBlocks];
    rnd.NextBytes(bytes);

    //Simulate a logging operation by writing text data and byte data to the end of the append blob.
    for (int i = 0; i < numBlocks; i++)
    {
        appendBlob.AppendText(String.Format("Timestamp: {0:u} \tLog Entry: {1}{2}",
            DateTime.UtcNow, bytes[i], Environment.NewLine));
    }

    //Read the append blob to the console window.
    Console.WriteLine(appendBlob.DownloadText());

Vt [mõistmine Blokeeri plekid, lehe plekid, ja lisa plekid](https://msdn.microsoft.com/library/azure/ee691964.aspx) kolme tüüpi plekid erinevuste kohta lisateavet.

## <a name="managing-security-for-blobs"></a>Turvalisus plekid haldamine

Vaikimisi Azure Storage hoiab teie andmete turvaline piirata juurdepääsu konto omanikule, kes on konto kiirklahvide. Kui soovite konto salvestusruumi bloobimälu andmeid ühiselt kasutada, on oluline ilma kompromisse oma konto kiirklahvide turvalisus. Lisaks saate krüptida bloobimälu kohta, et tagada, et see on turvaline minna üle kaabel ja Azure Storage.

[AZURE.INCLUDE [storage-account-key-note-include](../../includes/storage-account-key-note-include.md)]

### <a name="controlling-access-to-blob-data"></a>Bloobimälu andmetele juurdepääsu juhtimine

Vaikimisi konto salvestusruumi bloobimälu andmed on juurdepääs ainult salvestusruumi konto omanik. Konto juurdepääsu võti autentimist taotlusi vastu bloobimälu nõuab vaikimisi. Aga soovite teatud bloobimälu andmete teistele kasutajatele kättesaadavaks teha. Teil on kaks võimalust.

- **Anonüümne juurdepääs:** Ümbris või selle plekid saate anonüümse juurdepääsu avalikult kättesaadavaks teha. Lisateabe saamiseks vaadake [haldamine anonüümse ümbriste ja plekid lugemisõigus](storage-manage-access-to-resources.md) .
- **Ühiskasutuses Accessi allkirjad:** Saate sisestada kliendid ühiskasutatava Accessi allkirjastatud (SAS), mis pakub delegeeritud juurdepääsu ressursile salvestusruumi konto teie määratud õigustega ja intervall, mis teie määratud aja. Lisateabe saamiseks vaadake [Kaudu ühiskasutusse antud juurdepääs allkirjade (SAS)](storage-dotnet-shared-access-signature-part-1.md) .

### <a name="encrypting-blob-data"></a>Krüptimist bloobimälu

Azure'i salvestusruumi toetab bloobimälu krüptimist kliendis nii serveris.

- **Kliendipoolne krüptimise:** .Net-i salvestusruumi kliendi teek toetab krüptimist klientrakendustes jooksul enne Azure Storage üleslaadimine ja dekrüptimine andmete kliendile allalaadimise ajal. Teegi toetab ka integreerimine Azure klahvi Vault mäluhaldus konto võti. Lisateabe saamiseks vaadake [Kliendipoolne krüptimise abil .net-i jaoks: Microsoft Azure Tabelimäluga](storage-client-side-encryption.md) . Vt ka [õpetus: krüptimiseks ja dekrüptida plekid Microsoft Azure Storage abil Azure'i klahvi Vault](storage-encrypt-decrypt-blobs-key-vault.md).
- **Serveripoolne krüptimise**: Azure'i salvestusruumi toetab nüüd serveripoolne krüptimine. Leiate [Azure'i salvestusruumi teenuse krüptimise ülejäänud (eelvaade) andmete](storage-service-encryption.md).

## <a name="next-steps"></a>Järgmised sammud

Nüüd, kui olete õppinud bloobimälu põhitõdesid, järgige neid linke lisateabe.

### <a name="microsoft-azure-storage-explorer"></a>Microsoft Azure'i salvestusruumi Explorer
- [Microsoft Azure'i salvestusruumi Explorer (MASE)](../vs-azure-tools-storage-manage-with-storage-explorer.md) on tasuta, autonoomne rakendus Microsoft, mis võimaldab töötada visuaalselt Azure'i andmed, mille Windows, OS X ja Linux.

### <a name="blob-storage-samples"></a>Bloobimälu salvestusruumi näidised

- [Azure'i bloobimälu .NET-is töötamise alustamine](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/)

### <a name="blob-storage-reference"></a>Bloobimälu salvestusruumi viide

- [Salvestusruumi kliendi teek .net-i viide](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)
- [REST API viide](http://msdn.microsoft.com/library/azure/dd179355)

### <a name="conceptual-guides"></a>Kontseptuaalne juhikud

- [Andmete AzCopy käsurea kasuliku](storage-use-azcopy.md)
- [Failide talletamine .NET kasutamise alustamine](storage-dotnet-how-to-use-files.md)
- [Kuidas kasutada WebJobs SDK Azure'i bloobimälu](../app-service-web/websites-dotnet-webjobs-sdk-storage-blobs-how-to.md)

  [Blob5]: ./media/storage-dotnet-how-to-use-blobs/blob5.png
  [Blob6]: ./media/storage-dotnet-how-to-use-blobs/blob6.png
  [Blob7]: ./media/storage-dotnet-how-to-use-blobs/blob7.png
  [Blob8]: ./media/storage-dotnet-how-to-use-blobs/blob8.png
  [Blob9]: ./media/storage-dotnet-how-to-use-blobs/blob9.png

  [Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/
  [Configuring Connection Strings]: http://msdn.microsoft.com/library/azure/ee758697.aspx
  [.NET client library reference]: http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409
  [REST API reference]: http://msdn.microsoft.com/library/azure/dd179355
