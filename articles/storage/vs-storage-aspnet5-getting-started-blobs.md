<properties
    pageTitle="Alustamine bloobimälu salvestusruumi ja Visual Studio ühendatud teenused (ASP.net-i 5) | Microsoft Azure'i"
    description="Kuidas alustada Azure'i bloobimälu kasutamine Visual Studio ASP.net-i 5 projekti pärast loomist salvestusruumi kontoga ühendatud Visual Studio teenused"
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
    ms.date="07/18/2016"
    ms.author="tarcher"/>

# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-aspnet-5"></a>Azure'i bloobimälu alustamine salvestusruumi ja Visual Studio ühendatud teenused (ASP.net-i 5)

[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

##<a name="overview"></a>Ülevaade

Selles artiklis kirjeldatakse, kuidas alustada, kasutades Azure'i bloobimälu Visual Studios, kui olete loonud või viidatud Azure storage konto 5 ASP.net-i projekti Visual Studio ühendatud teenuste lisamine dialoogiboksi kaudu.

Azure'i bloobimälu on teenus salvestamiseks suure hulga struktureerimata andmed, millele pääseb juurde kõikjalt maailmas HTTP- või HTTPS-i kaudu. Ühe bloobimälu võib olla mis tahes suurusega. Plekid võib olla näiteks pildid, heli-ja videofailide, toorandmetega ja dokumendifailid. Selles artiklis kirjeldatakse, kuidas alustada bloobimälu, kui olete loonud Azure storage konto 5 ASP.net-i projekti Visual Studio **Ühendatud teenuste lisamine** dialoogiboksi kaudu.

Nagu failid talletatakse kaustad, salvestusruumi plekid elavad ümbriste. Kui olete loonud storage, saate luua ühe või mitme ümbriste talletamist. Näiteks laos nimega "Külalisteraamatusse"; ümbriste saate luua nimega "pildid" piltide talletamiseks talletamist ja teise nimega "heli" heli failid salvestada. Pärast soovitud ümbriste loomist saate üksikuid bloobimälu failide üleslaadimist need. Plekid programmiliselt käsitsemise kohta lisateabe saamiseks leiate [Azure'i bloobimälu .NET kasutamise alustamine](storage-dotnet-how-to-use-blobs.md) .

##<a name="access-blob-containers-in-code"></a>Accessi bloobimälu ümbriste kood

Programmiliselt juurde pääseda plekid ASP.net-i 5 projektides, peate lisama järgmised üksused, kui nad ei ole juba Esita.

1. Järgmine kood nimeruum deklaratsiooni lisada mis tahes C# faili, kus soovite programmiliselt juurde Azure storage algusse.

        using Microsoft.Extensions.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Blob;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Extensions.Logging.LogLevel;

2. Saada **CloudStorageAccount** objekti, mis tähistab kontoteabe salvestusruumi. Järgmine kood abil saate selle oma salvestusruumi ühendusstringi ja Azure teenuse konfigureerimine salvestusruumi kontoteavet.

         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));

    **Märkus:** Järgmistes jaotistes kasutada kõiki ülaltoodud kood ees tähis.


3. **CloudBlobClient** objekti abil saate avada mõne olemasoleva container **CloudBlobContainer** viide konto salvestusruumi.

        // Create a blob client.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

        // Get a reference to a container named “mycontainer.”
        CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");



##<a name="create-a-container-in-code"></a>Ümbris koodi loomine

Saate **CloudBlobClient** ümbris teie salvestusruumi konto loomiseks. Kõik, mida on vaja teha on lisada **CreateIfNotExistsAsync** kõne nagu järgmine kood:

    // Create a blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Get a reference to a container named “my-new-container.”
    CloudBlobContainer container = blobClient.GetContainerReference("my-new-container");

    // If “mycontainer” doesn’t exist, create it.
    await container.CreateIfNotExistsAsync();


**Märkus:** ASP.net-i 5 kõned Azure storage toetava API-d on asünkroonne. Lisateabe saamiseks vaadake [Asynchronous programmeerimine asünkroonse ja ootame](http://msdn.microsoft.com/library/hh191443.aspx) . Alljärgnev kood eeldab asünkroonse programmeerimise meetodite kasutatakse.

Kättesaadavaks ümbris faili kõigile, saate seada container olema avalik, kasutades järgmist.

    await container.SetPermissionsAsync(new BlobContainerPermissions
    {
        PublicAccess = BlobContainerPublicAccessType.Blob
    });

##<a name="upload-a-blob-into-a-container"></a>Ümbris laadida soovitud bloobimälu

Üleslaadimiseks bloobimälu faili ümbris, saada container viide ja kasutage seda saada bloobimälu viide. Olete bloobimälu viide, saate mis tahes andmevoo seda üles laadida, helistades **UploadFromStreamAsync** meetod. See toiming loob soovitud bloobimälu, kui see pole juba olemas või kirjutatakse üle, kui see on olemas. Järgmises näites kujutatakse laadida soovitud bloobimälu ümbris ja eeldab, et ümbris on juba loodud.

    // Get a reference to a blob named "myblob".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

    // Create or overwrite the "myblob" blob with the contents of a local file
    // named “myfile”.
    using (var fileStream = System.IO.File.OpenRead(@"path\myfile"))
    {
        await blockBlob.UploadFromStreamAsync(fileStream);
    }

##<a name="list-the-blobs-in-a-container"></a>Loendi a ümbrises plekid
Loendi plekid nõus, kõigepealt container viide. Seejärel saate helistada kõigepealt ümbrist **ListBlobsSegmentedAsync** meetod plekid ja/või kataloogide kogumahu toomiseks. Juurdepääs suurel hulgal atribuudid ja meetodite jaoks tagastatud **IListBlobItem**, peab see **CloudBlockBlob**, **CloudPageBlob**või **CloudBlobDirectory** objekti enamus. Kui te ei tea bloobimälu tüüp, saate määrata, milline see oma hääle tüüp sisse. Järgmine kood näitab, kuidas tuua ja iga üksuse ümbris URI väljund.

    BlobContinuationToken token = null;
    do
    {
        BlobResultSegment resultSegment = await container.ListBlobsSegmentedAsync(token);
        token = resultSegment.ContinuationToken;

        foreach (IListBlobItem item in resultSegment.Results)
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
    } while (token != null);

On teisi viisid bloobimälu container sisu loendi. Lisateavet leiate [Azure'i bloobimälu .NET kasutamise alustamine](storage-dotnet-how-to-use-blobs.md#list-the-blobs-in-a-container) .

##<a name="download-a-blob"></a>Laadige alla mõne bloobimälu
Laadige alla mõne bloobimälu, kõigepealt soovitud bloobimälu viide ja seejärel kõne **DownloadToStreamAsync** meetod. Järgmises näites kasutatakse **DownloadToStreamAsync** meetodit bloobimälu sisu edastada voo objekti, mida saate siis kohaliku failina salvestada.

    // Get a reference to a blob named "photo1.jpg".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("photo1.jpg");

    // Save the blob contents to a file named “myfile”.
    using (var fileStream = System.IO.File.OpenWrite(@"path\myfile"))
    {
        await blockBlob.DownloadToStreamAsync(fileStream);
    }

On muul viisil plekid failidena salvestada. Lisateavet leiate [Azure'i bloobimälu .NET kasutamise alustamine](storage-dotnet-how-to-use-blobs.md#download-blobs) .

##<a name="delete-a-blob"></a>Kustutamine on bloobimälu
Mõne bloobimälu kustutamiseks kõigepealt viide on bloobimälu ja seejärel helistage **DeleteAsync** meetod seda.

    // Get a reference to a blob named "myblob.txt".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob.txt");

    // Delete the blob.
    await blockBlob.DeleteAsync();

## <a name="next-steps"></a>Järgmised sammud

[AZURE.INCLUDE [vs-storage-dotnet-blobs-next-steps](../../includes/vs-storage-dotnet-blobs-next-steps.md)]
