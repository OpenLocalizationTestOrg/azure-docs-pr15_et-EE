<properties
    pageTitle="Ja tuua atribuudid ja metaandmete Azure Storage objektide jaoks | Microsoft Azure'i"
    description="Kohandatud metaandmed salvestada Azure Storage, objektide ja seadmine ja tuua Süsteemiatribuudid."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>

# <a name="set-and-retrieve-properties-and-metadata"></a>Ja tuua atribuudid ja metaandmed #

## <a name="overview"></a>Ülevaade

Objektide Azure Storage tugi Süsteemiatribuudid ja kasutaja määratletud metaandmete Lisaks sisalduvate andmete:

*   **Süsteemiatribuudid.** Süsteemiatribuudid olemas iga salvestusruumi ressursi. Mõned neist saab lugeda ja määrata, samas kui teised on kirjutuskaitstud. All hõlmab, mõned Süsteemiatribuudid vastavad teatud standardse HTTP päised. Azure'i salvestusruumi kliendi teek säilitab need teie jaoks.  

*   **Kasutaja määratletud metaandmete.** Kasutaja määratletud metaandmete on antud ressursi, vormi nimi väärtus paari määratud metaandmete. Metaandmete abil saate talletada väärtuste salvestusruumi ressursiga; need väärtused ainult enda jaoks ja see ei mõjuta ressursi käitumise.  

Toomine salvestusruumi ressursi atribuudi ja metaandmete väärtused on kaheosaline toiming. Enne kui saate lugeda need väärtused, peab neid konkreetselt toomiseks, helistades **FetchAttributes** meetod.

> [AZURE.IMPORTANT] Atribuut ja metaandmete väärtused salvestusruumi ressursi täidetakse ei juhul, kui helistate meetodite **FetchAttributes** . 

## <a name="setting-and-retrieving-properties"></a>Seada ja allalaadimine atribuudid

Atribuudi väärtuste toomiseks kõne **FetchAttributes** meetodit bloobimälu või container asustamiseks atribuudid ja seejärel lugege väärtused.

Objekti atribuutide määramiseks määrake atribuudi väärtus ja seejärel **SetProperties** meetod.

Järgmine kood näide ümbris loob ja kirjutab osa selle atribuudi väärtuste konsooli aknas.

    //Parse the connection string for the storage account.
    const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);
    
    //Create the service client object for credentialed access to the Blob service.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve a reference to a container. 
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Create the container if it does not already exist.
    container.CreateIfNotExists();

    // Fetch container properties and write out their values.
    container.FetchAttributes();
    Console.WriteLine("Properties for container {0}", container.StorageUri.PrimaryUri.ToString());
    Console.WriteLine("LastModifiedUTC: {0}", container.Properties.LastModified.ToString());
    Console.WriteLine("ETag: {0}", container.Properties.ETag);
    Console.WriteLine();

## <a name="setting-and-retrieving-metadata"></a>Seadmine ja metaandmete allalaadimine

Saate määrata ühe või mitme nime ja väärtuse paarideks bloobimälu või container ressursi metaandmete. Metaandmete määramiseks nimi ja väärtuse paarideks lisada **metaandmete** saidikogumi ressurss ja seejärel **SetMetadata** meetod väärtused salvestada teenusesse.

> [AZURE.NOTE] Oma metaandmete nimi peab vastama nimereeglid C# ID-de jaoks.
 
Järgmine kood näide komplekti metaandmete ümbris. Ühe väärtuseks on seatud soovitud saidikogumi **Lisa** meetodi abil. Muud väärtuseks on seatud peidetud /-väärtuse süntaksi kasutamine. Mõlemad on lubatud.

    public static void AddContainerMetadata(CloudBlobContainer container)
    {
        //Add some metadata to the container.
        container.Metadata.Add("docType", "textDocuments");
        container.Metadata["category"] = "guidance";

        //Set the container's metadata.
        container.SetMetadata();
    }

Metaandmete toomiseks kõne **FetchAttributes** meetodit bloobimälu või container asustamiseks **metaandmete** saidikogumi, siis lugege väärtused, nagu on näidatud järgmises näites.

    public static void ListContainerMetadata(CloudBlobContainer container)
    {
        //Fetch container attributes in order to populate the container's properties and metadata.
        container.FetchAttributes();

        //Enumerate the container's metadata.
        Console.WriteLine("Container metadata:");
        foreach (var metadataItem in container.Metadata)
        {
            Console.WriteLine("\tKey: {0}", metadataItem.Key);
            Console.WriteLine("\tValue: {0}", metadataItem.Value);
        }
    }

## <a name="see-also"></a>Vt ka  

- [Azure'i salvestusruumi kliendi teek .net-i viide](http://msdn.microsoft.com/library/azure/wa_storage_30_reference_home.aspx)
- [Azure'i salvestusruumi kliendi teek .NET pakett](https://www.nuget.org/packages/WindowsAzure.Storage/) 
