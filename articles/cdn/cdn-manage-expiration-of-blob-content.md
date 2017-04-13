<properties
 pageTitle="Halda salvestusruumi Azure'i bloobimälu sisu Azure'i CDN aegumise | Microsoft Azure'i"
 description="Siit saate teada, kontrollides aja live plekid Azure CDN vahemällu võimaluste kohta."
 services="cdn"
 documentationCenter=""
 authors="camsoper"
 manager="erikre"
 editor=""/>
<tags
 ms.service="cdn"
 ms.workload="media"
 ms.tgt_pltfrm="na"
 ms.devlang="multiple"
 ms.topic="article"
 ms.date="09/15/2016"
 ms.author="casoper"/>


# <a name="manage-expiration-of-azure-storage-blob-content-in-azure-cdn"></a>Aegumise Azure Storage CDN Azure'i bloobimälu sisu haldamine

> [AZURE.SELECTOR]
- [Rakenduste Azure Web pilveteenustega, ASP.net-i või IIS-i](cdn-manage-expiration-of-cloud-service-content.md)
- [Azure'i bloobimälu salvestusteenus](cdn-manage-expiration-of-blob-content.md)

[Azure](../storage/storage-introduction.md) Storage [bloobimälu teenus](../storage/storage-introduction.md#blob-storage) on üks integreeritud Azure CDN mitme Azure'i päritolu.  Mis tahes sisu, avalikult juurdepääsetava bloobimälu saab kopeerida Azure'i CDN kuni selle aja live (TTL) kaitseaega.  TTL määratakse HTTP vastuse Azure Storage [ *Olla* päis](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9) .

>[AZURE.TIP] Võite määrata pole TTL on bloobimälu.  Sel juhul kehtib Azure'i CDN automaatselt vaikimisi TTL seitse päeva.
>
>Azure'i CDN kiirendada Accessi plekid ja muude failide tööpõhimõtete kohta leiate lisateavet teemast [Azure CDN ülevaade](./cdn-overview.md).
>
>Azure'i bloobimälu salvestusteenus kohta leiate lisateavet teemast [Bloobimälu teenuse põhimõtet](https://msdn.microsoft.com/library/dd179376.aspx). 

Selle õpetuse näitab, mitu võimalust, mis määratavad TTL on bloobimälu Azure Storage.  

## <a name="azure-powershell"></a>Azure'i PowerShelli

[Azure'i PowerShelli](../powershell-install-configure.md) on üks kiireim, kõige tõhusamaid viise Azure'i teenuste haldamine.  Kasutage soovitud `Get-AzureStorageBlob` cmdlet-käsu saada bloobimälu, määrake soovitud `.ICloudBlob.Properties.CacheControl` atribuut. 

```powershell
# Create a storage context
$context = New-AzureStorageContext -StorageAccountName "<storage account name>" -StorageAccountKey "<storage account key>"

# Get a reference to the blob
$blob = Get-AzureStorageBlob -Context $context -Container "<container name>" -Blob "<blob name>"

# Set the CacheControl property to expire in 1 hour (3600 seconds)
$blob.ICloudBlob.Properties.CacheControl = "public, max-age=3600"

# Send the update to the cloud
$blob.ICloudBlob.SetProperties()
```

>[AZURE.TIP] PowerShelli abil saate [hallata oma CDN profiilid ja lõpp-punktid](./cdn-manage-powershell.md).

## <a name="azure-storage-client-library-for-net"></a>Azure'i salvestusruumi kliendi teek .net-i jaoks

Kasutades .net-i lisamine bloobimälu TTL, kasutage [CloudBlob.Properties.CacheControl](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.blobproperties.cachecontrol.aspx) atribuudi [Azure'i salvestusruumi kliendi teek .net-i jaoks](../storage/storage-dotnet-how-to-use-blobs.md) .

```csharp
class Program
{
    const string connectionString = "<storage connection string>";
    static void Main()
    {
        // Retrieve storage account information from connection string
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString);
        
        // Create a blob client for interacting with the blob service.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
        
        // Create a reference to the container
        CloudBlobContainer container = blobClient.GetContainerReference("<container name>");

        // Create a reference to the blob
        CloudBlob blob = container.GetBlobReference("<blob name>");

        // Set the CacheControl property to expire in 1 hour (3600 seconds)
        blob.Properties.CacheControl = "public, max-age=3600";

        // Update the blob's properties in the cloud
        blob.SetProperties();
    }
}
```

>[AZURE.TIP] On palju veel .NET koodinäiteid [Azure'i bloobimälu salvestusruumi näidised .net-i jaoks](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/)saadaval.

## <a name="other-methods"></a>Muud võimalused

- [Azure'i käsurea liides](../xplat-cli-install.md)

    Funktsiooni bloobimälu üleslaadimisel seadmine *cacheControl* atribuudi abil soovitud `-p` aktiveerimine.  Selles näites määrab TTL tund (3600 sekundit).

    ```text
    azure storage blob upload -c <connectionstring> -p cacheControl="public, max-age=3600" .\test.txt myContainer test.txt
    ```

- [Azure'i salvestusruumi teenuste REST API-ga](https://msdn.microsoft.com/library/azure/dd179355.aspx)

    Konkreetselt atribuudi *x-ms-bloobimälu-olla* [Sellele bloobimälu](https://msdn.microsoft.com/en-us/library/azure/dd179451.aspx), [Pange blokeeritute loendis](https://msdn.microsoft.com/en-us/library/azure/dd179467.aspx)või [Bloobimälu atribuutide seadmine](https://msdn.microsoft.com/library/azure/ee691966.aspx) taotluse.

- Muu Haldusriistad

    Mõne muu Azure Storage Haldusriistad võimaldavad *CacheControl* atribuudi plekid kohta. 

## <a name="testing-the-cache-control-header"></a>Testimine *Olla* päis

Saate hõlpsasti kontrollida oma plekid TTL.  Brauseri [arendaja tööriistade](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/)abil testida, et teie bloobimälu on sh *Olla* vastuse päis.  Samuti saate tööriista nagu **wget**, [postiljon](https://www.getpostman.com/)või [viiuldaja](http://www.telerik.com/fiddler) uurida vastus päised.

## <a name="next-steps"></a>Järgmised sammud

- [Lugege *Olla* päis](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
- [Saate teada, kuidas aegumise Azure'i CDN pilveteenusesse sisu haldamine](./cdn-manage-expiration-of-cloud-service-content.md)

