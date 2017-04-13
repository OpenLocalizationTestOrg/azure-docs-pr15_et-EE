<properties
    pageTitle="Alustamine bloobimälu salvestusruumi ja Visual Studio ühendatud teenused (pilveteenustega) | Microsoft Azure'i"
    description="Kuidas alustada Azure'i bloobimälu kasutamine pilvepõhise teenuse projekti Visual Studio pärast salvestusruumi Visual Studio abil kontoga ühenduse ühendatud teenused"
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

# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-cloud-services-projects"></a>Azure'i bloobimälu ja Visual Studio kasutamise alustamine ühendatud teenused (cloud services projektid)

[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Ülevaade

Selles artiklis kirjeldatakse, kuidas alustada Azure'i bloobimälu pärast loodud või Visual Studio **Ühendatud teenuste lisamine** dialoogiboksi kaudu Projectis Visual Studio cloud services konto Azure Storage viidatud. Näitame teile, kuidas juurdepääsu ja nende loomine bloobimälu ümbriste ja kuidas teha levinud toiminguid nagu üles laadida, videod ja alla plekid. Näidiste kirjutada c\# ning [Microsoft Azure'i salvestusruumi kliendi teek .net-i jaoks](https://msdn.microsoft.com/library/azure/dn261237.aspx).

Azure'i bloobimälu on teenus salvestamiseks suure hulga struktureerimata andmed, millele pääseb juurde kõikjalt maailmas HTTP- või HTTPS-i kaudu. Ühe bloobimälu võib olla mis tahes suurusega. Plekid võib olla näiteks pildid, heli-ja videofailide, toorandmetega ja dokumendifailid.

Nagu failide live kaustadesse, salvestusruumi plekid elavad ümbriste. Kui olete loonud storage, saate luua ühe või mitme ümbriste talletamist. Näiteks laos nimega "Külalisteraamatusse"; ümbriste saate luua nimega "pildid" piltide talletamiseks talletamist ja teise nimega "heli" heli failid salvestada. Pärast soovitud ümbriste loomist saate üksikuid bloobimälu failide üleslaadimist need.

- Plekid programmiliselt käsitsemise kohta leiate lisateavet teemast [Alustamine Azure'i bloobimälu .net-i abil](storage-dotnet-how-to-use-blobs.md).
- Üldteavet Azure Storage [salvestusruumi](https://azure.microsoft.com/documentation/services/storage/)dokumentatsioonist.
- Üldteavet Azure'i pilveteenustega [Pilveteenustega](https://azure.microsoft.com/documentation/services/cloud-services/)dokumentatsioonist.
- ASP.net-i rakenduste programmeerimine kohta leiate lisateavet teemast [ASP.net-i](http://www.asp.net).

## <a name="access-blob-containers-in-code"></a>Accessi bloobimälu ümbriste kood

Programmiliselt juurde pääseda plekid pilvepõhise teenuse projektides, peate lisama järgmised üksused, kui nad ei ole juba Esita.

1. Järgmine kood nimeruum deklaratsiooni lisada mis tahes C# faili, kus soovite programmiliselt juurde Azure Storage algusse.

        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Blob;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Framework.Logging.LogLevel;

2. Saada **CloudStorageAccount** objekti, mis tähistab kontoteabe salvestusruumi. Järgmine kood abil saate selle oma salvestusruumi ühendusstringi ja Azure teenuse konfigureerimine salvestusruumi kontoteavet.

        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("<storage account name>_AzureStorageConnectionString"));

3. Saada **CloudBlobClient** objekti viide mõne olemasoleva container teie salvestusruumi konto.

        // Create a blob client.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

4. Saada **CloudBlobContainer** objekti viide teatud bloobimälu ümbrises.

        // Get a reference to a container named “mycontainer.”
        CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

> [AZURE.NOTE] Kasutada kõiki vastavalt eelmise jaotise järgnevalt näidatud koodi ette näidatud koodi.

## <a name="create-a-container-in-code"></a>Ümbris koodi loomine

> [AZURE.NOTE] ASP.net-i Azure Storage välja kõnesid teha mõned API-d on asünkroonne. Lisateabe saamiseks vaadake [Asynchronous programmeerimine asünkroonse ja ootame](http://msdn.microsoft.com/library/hh191443.aspx) . Järgmises näites kood eeldab, et kasutate asünkroonse programmeerimise meetodite.

Teie salvestusruumi konto loomiseks ümbris, peate tegema on lisada **CreateIfNotExistsAsync** kõne nagu järgmine kood:

    // If “mycontainer” doesn’t exist, create it.
    await container.CreateIfNotExistsAsync();


Kättesaadavaks ümbris faili kõigile, saate seada container olema avalik, kasutades järgmist.

    await container.SetPermissionsAsync(new BlobContainerPermissions
    {
        PublicAccess = BlobContainerPublicAccessType.Blob
    });


Kõik Interneti näevad plekid avaliku ümbrises, kuid saate muuta või kustutada ainult juhul, kui teil on asjakohane juurdepääsu võti.

## <a name="upload-a-blob-into-a-container"></a>Ümbris laadida soovitud bloobimälu

Azure'i salvestusruumi toetab Blokeeri plekid ja lehe plekid. Enamikul juhtudel, Blokeeri bloobimälu on soovitatav kasutada tüüp.

Faili üleslaadimine Blokeeri bloobimälu saada container viide ja selle abil saate blokeerida bloobimälu viide. Kui olete bloobimälu viide, saate mis tahes andmevoo see üles laadida, helistades **UploadFromStream** meetod. See toiming loob soovitud bloobimälu, kui see ei ole varem olemas või kirjutatakse üle, kui see on olemas. Järgmises näites kujutatakse laadida soovitud bloobimälu ümbris ja eeldab, et ümbris on juba loodud.

    // Retrieve a reference to a blob named "myblob".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

    // Create or overwrite the "myblob" blob with contents from a local file.
    using (var fileStream = System.IO.File.OpenRead(@"path\myfile"))
    {
        blockBlob.UploadFromStream(fileStream);
    }

## <a name="list-the-blobs-in-a-container"></a>Loendi a ümbrises plekid

Loendi plekid nõus, kõigepealt container viide. Seejärel saate tuua plekid ja/või kataloogide kogumahu kõigepealt ümbrist **ListBlobs** meetod. Juurdepääs suurel hulgal atribuudid ja meetodite jaoks tagastatud **IListBlobItem**, peab see **CloudBlockBlob**, **CloudPageBlob**või **CloudBlobDirectory** objekti enamus. Kui tüüp on tundmatu, saate määrata, milline see oma hääle tüüp sisse. Järgmine kood näitab, kuidas tuua ja iga üksuse **fotod** ümbrises URI väljund:

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

Nagu on näidatud eelmise proovi kood, on bloobimälu teenuse mõiste kataloogide ümbriste, sees. See on nii, et saaksite korraldada oma plekid kausta moodi struktuuris. Näiteks, võtke arvesse järgmist määramine Blokeeri plekid nimega **fotod**nõus.

    photo1.jpg
    2010/architecture/description.txt
    2010/architecture/photo3.jpg
    2010/architecture/photo4.jpg
    2011/architecture/photo5.jpg
    2011/architecture/photo6.jpg
    2011/architecture/description.txt
    2011/photo7.jpg

Kui helistate **ListBlobs** container (nt eelmine näidis), tagastatakse kogu sisaldab kataloogid ja plekid sisaldas ülatasemel objektide **CloudBlobDirectory** ja **CloudBlockBlob** . Siin on tulemuseks väljundi.

    Directory: https://<accountname>.blob.core.windows.net/photos/2010/
    Directory: https://<accountname>.blob.core.windows.net/photos/2011/
    Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg


Soovi korral saate määrata **ListBlobs** meetodi parameetri **UseFlatBlobListing** väärtuseks **true**. Selle tulemusena iga bloobimälu tagastatakse on **CloudBlockBlob**, olenemata kataloogi. Siin **ListBlobs**kõne.

    // Loop over items within the container and output the length and URI.
    foreach (IListBlobItem item in container.ListBlobs(null, true))
    {
       ...
    }

ja siin on tulemused.

    Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2010/architecture/description.txt
    Block blob of length 314618: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo3.jpg
    Block blob of length 522713: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo4.jpg
    Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2011/architecture/description.txt
    Block blob of length 419048: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo5.jpg
    Block blob of length 506388: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo6.jpg
    Block blob of length 399751: https://<accountname>.blob.core.windows.net/photos/2011/photo7.jpg
    Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg

Lisateavet leiate teemast [CloudBlobContainer.ListBlobs](https://msdn.microsoft.com/library/azure/dd135734.aspx).

## <a name="download-blobs"></a>Laadige alla plekid

Plekid allalaadimiseks esmalt tuua bloobimälu viide ja seejärel helistage **DownloadToStream** meetod. Järgmises näites kasutatakse **DownloadToStream** meetodit voo objekti, mida saate siis jää püsima kohaliku faili bloobimälu sisu edastada.

    // Get a reference to a blob named "photo1.jpg".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("photo1.jpg");

    // Save blob contents to a file.
    using (var fileStream = System.IO.File.OpenWrite(@"path\myfile"))
    {
        blockBlob.DownloadToStream(fileStream);
    }

**DownloadToStream** meetodi abil soovitud bloobimälu tekstistringi kujul sisu alla laadida.

    // Get a reference to a blob named "myblob.txt"
    CloudBlockBlob blockBlob2 = container.GetBlockBlobReference("myblob.txt");

    string text;
    using (var memoryStream = new MemoryStream())
    {
        blockBlob2.DownloadToStream(memoryStream);
        text = System.Text.Encoding.UTF8.GetString(memoryStream.ToArray());
    }

## <a name="delete-blobs"></a>Plekid kustutamine

Mõne bloobimälu kustutamiseks esmalt saada bloobimälu viide ja seejärel kõne **kustutamine** meetod.

    // Get a reference to a blob named "myblob.txt".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob.txt");

    // Delete the blob.
    blockBlob.Delete();


## <a name="list-blobs-in-pages-asynchronously"></a>Loendi plekid lehtedel asünkroonselt

Kui teil on suur hulk plekid loetletakse või soovite reguleerida naasete ühe kirjet toiminguga tulemite arv, saate loetleda plekid tulemuste lehtedel. Selles näites kujutatakse tulemeid lehtedel asünkroonselt, nii, et täitmise on blokeeritud ooteajal suure hulga tulemite tagastamiseks.

Selles näites tasapinnalise bloobimälu, kus on näha, kuid hierarhilise loendi, saate teha ka seadmisega **useFlatBlobListing** parameetri **ListBlobsSegmentedAsync** meetodi **väär (FALSE)**.

Kuna valimi meetodit helistab asünkroonne sisestusmeetod, peab sissejuhatava põhjendusega **asünkroonse** märksõna ja see peab andma **tööülesande** objekti. Ootame märksõna **ListBlobsSegmentedAsync** meetodi jaoks on määratud peatab täitmise valimi meetodi kuni kirjet tööülesanne on lõpule viidud.

    async public static Task ListBlobsSegmentedInFlatListing(CloudBlobContainer container)
    {
        // List blobs to the console window, with paging.
        Console.WriteLine("List blobs in pages:");

        int i = 0;
        BlobContinuationToken continuationToken = null;
        BlobResultSegment resultSegment = null;

        // Call ListBlobsSegmentedAsync and enumerate the result segment returned, while the continuation token is non-null.
        // When the continuation token is null, the last page has been returned and execution can exit the loop.
        do
        {
            // This overload allows control of the page size. You can return all remaining results by passing null for the maxResults parameter,
            // or by calling a different overload.
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

## <a name="next-steps"></a>Järgmised sammud

[AZURE.INCLUDE [vs-storage-dotnet-blobs-next-steps](../../includes/vs-storage-dotnet-blobs-next-steps.md)]
