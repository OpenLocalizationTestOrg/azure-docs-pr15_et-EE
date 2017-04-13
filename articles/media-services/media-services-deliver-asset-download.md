<properties 
    pageTitle="Meediumivarad allalaadimine" 
    description="Vaadake, kuidas teave varad oma arvutisse alla laadida. Koodinäiteid C# kirjutada ja kasutada Media Services SDK .net-i jaoks." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="juliako"/>

#<a name="how-to-deliver-an-asset-by-download"></a>Kuidas: esitamisel vara alla

Selles teemas käsitletakse võimalusi pakkuda meediumivarad Media Servicesi üles laadida. Mitme rakenduse stsenaariumide suudab Media Servicesi sisu. Saate alla laadida meediumivarad või neile juurde pääseda, kasutades mõnda locator. Saate saata meediumisisu mõnda muusse rakendusse või muu sisu pakkuja. Paranenud jõudlus ja skaleeritavus, saate esitada sisu abil sisu kohaletoimetamise võrk (CDN).

Selles näites kujutatakse meediumivarad Media Servicesi kaudu kohalikku arvutisse allalaadimiseks. Koodi päringut töö ID Media Servicesi kontoga seotud töökohtade ja avab selle **OutputMediaAssets** saidikogumi (mis on ühe või mitme väljundi meediumivarad kogum, mis tuleneb projekti käitamise ajal). Selles näites kirjeldatakse, kuidas alla laadida väljundi meediumivarad töö, kuid saate rakendada sama lähenemisviisi muu vara alla laadida.

    
    // Download the output asset of the specified job to a local folder.
    static IAsset DownloadAssetToLocal( string jobId, string outputFolder)
    {
        // This method illustrates how to download a single asset. 
        // However, you can iterate through the OutputAssets
        // collection, and download all assets if there are many. 
    
        // Get a reference to the job. 
        IJob job = GetJob(jobId);
    
        // Get a reference to the first output asset. If there were multiple 
        // output media assets you could iterate and handle each one.
        IAsset outputAsset = job.OutputMediaAssets[0];
    
        // Create a SAS locator to download the asset
        IAccessPolicy accessPolicy = _context.AccessPolicies.Create("File Download Policy", TimeSpan.FromDays(30), AccessPermissions.Read);
        ILocator locator = _context.Locators.CreateLocator(LocatorType.Sas, outputAsset, accessPolicy);
    
        BlobTransferClient blobTransfer = new BlobTransferClient
        {
            NumberOfConcurrentTransfers = 20,
            ParallelTransferThreadCount = 20
        };
    
        var downloadTasks = new List<Task>();
        foreach (IAssetFile outputFile in outputAsset.AssetFiles)
        {
            // Use the following event handler to check download progress.
            outputFile.DownloadProgressChanged += DownloadProgress;
    
            string localDownloadPath = Path.Combine(outputFolder, outputFile.Name);
    
            Console.WriteLine("File download path:  " + localDownloadPath);
    
            downloadTasks.Add(outputFile.DownloadAsync(Path.GetFullPath(localDownloadPath), blobTransfer, locator, CancellationToken.None));
    
            outputFile.DownloadProgressChanged -= DownloadProgress;
        }
    
        Task.WaitAll(downloadTasks.ToArray());
    
        return outputAsset;
    }
    
    static void DownloadProgress(object sender, DownloadProgressChangedEventArgs e)
    {
        Console.WriteLine(string.Format("{0} % download progress. ", e.Progress));
    }



##<a name="media-services-learning-paths"></a>Media Servicesi Õppeteed

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Tagasiside saatmine

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

   
##<a name="see-also"></a>Vt ka 

[Streaming sisu esitamine](media-services-deliver-streaming-content.md)

