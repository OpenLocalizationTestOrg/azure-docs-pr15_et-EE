<properties
    pageTitle="Meediumifailide Azure Media indekseerija 2 eelvaatega indekseerimine | Microsoft Azure'i"
    description="Azure Media indekseerija võimaldab teil teha meediumifailide sisu otsida ja täisteksti logifaili tiitrid ja märksõnade jaoks luua. Selles teemas kirjeldatakse, kuidas kasutada Media indekseerija 2 eelvaade."
    services="media-services"
    documentationCenter=""
    authors="Juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/26/2016" 
    ms.author="adsolank;juliako;"/>


# <a name="indexing-media-files-with-azure-media-indexer-2-preview"></a>Indekseerimise meediumifailide Azure Media indekseerija 2 eelvaatega

##<a name="overview"></a>Ülevaade

**Azure Media indekseerija 2 eelvaade** meediumi protsessor (MP) võimaldab teil teha meediumifailide ja sisu otsida, samuti luua suletud pealdise jälitab. Võrreldes eelmise versiooni [Azure Media indekseerija](media-services-index-content.md), **Azure Media indekseerija 2 eelvaade** sooritab kiirem indekseerimine ja pakub laiem keele tuge. Toetatud keeltes kaasata inglise, Hispaania, prantsuse, saksa, Itaalia, Hiina, Portugali ja Araabia.

**Azure Media indekseerija 2 eelvaade** MP praegu eelvaade.

Selles teemas kirjeldatakse, kuidas luua indekseerimise töö **Azure Media indekseerija 2 eelvaade**.

>[AZURE.NOTE]Kehtivad järgmisega.
>
>Azure'i Hiinas ja Azure Governmenti ei toeta indekseerijat 2.
>
>Eelvaate piirdub töötlemise ~ 10 minutit, kuid on tasuta Kõik kliendid.
>
>Kui indekseerimine sisu, veenduge, et kasutada meediumifailide, millel on väga selge kõne (ilma taustamuusika, müra, efektide ja mikrofoni sisisema). Asjakohase sisu mõned näited: Salvestatud koosolekuid, loenguid või esitlusi. Järgmine sisu ei pruugi olla sobiv indekseerimise: Filmid, telesaateid midagi kombineeritud heli- ja heliefekte, mille halvasti salvestatud sisu taustamüra (sisisema).


See teema annab **Azure Media indekseerija 2 eelvaade** üksikasjad ja näitab, kuidas seda kasutada koos Media Services SDK .net-i jaoks

##<a name="input-and-output-files"></a>Sisend- ja failid

###<a name="input-files"></a>Failid

Heli-või videofaile

###<a name="output-files"></a>Väljund faili

On indekseerimise töökohtade saate luua tiitrite failid järgmises vormingus:  

- **SAAMI**
- **TTML**
- **WebVTT**

Suletud pealdise (koopia) järgmistes vormingutes faile saab kasutada heli-ja videofailide kättesaadavaks inimestele kuulmispuudega inimestele.

##<a name="task-configuration-preset"></a>Ülesande konfigureerimine (algne)

Indekseerimise toimingu **Azure Media indekseerija 2**eelvaatega loomisel peate määrama konfiguratsiooni valmissäte.

Järgmised JSON määrab saadaolevad parameetrid.

    {
      "version":"1.0",
      "Features":
        [
           {
           "Options": {
                "Formats":["WebVtt","ttml"],
                "Language":"enUs",
                "Type":"RecoOptions"
           },
           "Type":"SpReco"
        }]
    }

##<a name="supported-languages"></a>Toetatud keeltes  

Azure Media indekseerija 2 eelvaade toetab kõne – teksti järgmistes keeltes (kui täpsustades keele nimi tööülesande konfiguratsiooni puhul kasutada 4-kohaline kood sulgudes, nagu allpool näidatud).

- Inglise [EnUs]
- Hispaania [EsEs]
- Hiina [ZhCn]
- Prantsuse [FrFr]
- Saksa [DeDe]
- Itaalia [ItIt]
- Portugali keele [PtBr]
- Araabia (Egiptuse) [ArEg]


## <a name="sample-code"></a>Proovi kood

Järgmine programm kuvatakse kohta:

1. Looge vara ja media faili üleslaadimine rakendusse vara.
1. Loob töö põhjal konfiguratsioonifail, mis sisaldab järgmist json valmissäte indekseerimise toimingu.
            
        {
          "version":"1.0",
          "Features":
            [
               {
               "Options": {
                    "Formats":["WebVtt","ttml"],
                    "Language":"enUs",
                    "Type":"RecoOptions"
               },
               "Type":"SpReco"
            }]
        }

1. Väljundi failide allalaadimist. 
    
        using System;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using System.Threading;
        using System.Threading.Tasks;
        
        namespace IndexContent
        {
            class Program
            {
                // Read values from the App.config file.
                private static readonly string _mediaServicesAccountName =
                    ConfigurationManager.AppSettings["MediaServicesAccountName"];
                private static readonly string _mediaServicesAccountKey =
                    ConfigurationManager.AppSettings["MediaServicesAccountKey"];
        
                // Field for service context.
                private static CloudMediaContext _context = null;
                private static MediaServicesCredentials _cachedCredentials = null;
        
                static void Main(string[] args)
                {
        
                    // Create and cache the Media Services credentials in a static class variable.
                    _cachedCredentials = new MediaServicesCredentials(
                                    _mediaServicesAccountName,
                                    _mediaServicesAccountKey);
                    // Used the cached credentials to create CloudMediaContext.
                    _context = new CloudMediaContext(_cachedCredentials);
        
                    // Run indexing job.
                    var asset = RunIndexingJob(@"C:\supportFiles\Indexer\BigBuckBunny.mp4",
                                                @"C:\supportFiles\Indexer\config.json");
        
                    // Download the job output asset.
                    DownloadAsset(asset, @"C:\supportFiles\Indexer\Output");
                }
        
                static IAsset RunIndexingJob(string inputMediaFilePath, string configurationFile)
                {
                    // Create an asset and upload the input media file to storage.
                    IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                        "My Indexing Input Asset",
                        AssetCreationOptions.None);
        
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("My Indexing Job");
        
                    // Get a reference to Azure Media Indexer 2 Preview.
                    string MediaProcessorName = "Azure Media Indexer 2 Preview";
        
                    var processor = GetLatestMediaProcessorByName(MediaProcessorName);
        
                    // Read configuration from the specified file.
                    string configuration = File.ReadAllText(configurationFile);
        
                    // Create a task with the encoding details, using a string preset.
                    ITask task = job.Tasks.AddNew("My Indexing Task",
                        processor,
                        configuration,
                        TaskOptions.None);
        
                    // Specify the input asset to be indexed.
                    task.InputAssets.Add(asset);
        
                    // Add an output asset to contain the results of the job.
                    task.OutputAssets.AddNew("My Indexing Output Asset", AssetCreationOptions.None);
        
                    // Use the following event handler to check job progress.  
                    job.StateChanged += new EventHandler<JobStateChangedEventArgs>(StateChanged);
        
                    // Launch the job.
                    job.Submit();
        
                    // Check job execution and wait for job to finish.
                    Task progressJobTask = job.GetExecutionProgressTask(CancellationToken.None);
        
                    progressJobTask.Wait();
        
                    // If job state is Error, the event handling
                    // method for job progress should log errors.  Here we check
                    // for error state and exit if needed.
                    if (job.State == JobState.Error)
                    {
                        ErrorDetail error = job.Tasks.First().ErrorDetails.First();
                        Console.WriteLine(string.Format("Error: {0}. {1}",
                                                        error.Code,
                                                        error.Message));
                        return null;
                    }
        
                    return job.OutputMediaAssets[0];
                }
        
                static IAsset CreateAssetAndUploadSingleFile(string filePath, string assetName, AssetCreationOptions options)
                {
                    IAsset asset = _context.Assets.Create(assetName, options);
        
                    var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
                    assetFile.Upload(filePath);
        
                    return asset;
                }
        
                static void DownloadAsset(IAsset asset, string outputDirectory)
                {
                    foreach (IAssetFile file in asset.AssetFiles)
                    {
                        file.Download(Path.Combine(outputDirectory, file.Name));
                    }
                }
        
                static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
                {
                    var processor = _context.MediaProcessors
                        .Where(p => p.Name == mediaProcessorName)
                        .ToList()
                        .OrderBy(p => new Version(p.Version))
                        .LastOrDefault();
        
                    if (processor == null)
                        throw new ArgumentException(string.Format("Unknown media processor",
                                                                   mediaProcessorName));
        
                    return processor;
                }
        
                static private void StateChanged(object sender, JobStateChangedEventArgs e)
                {
                    Console.WriteLine("Job state changed event:");
                    Console.WriteLine("  Previous state: " + e.PreviousState);
                    Console.WriteLine("  Current state: " + e.CurrentState);
        
                    switch (e.CurrentState)
                    {
                        case JobState.Finished:
                            Console.WriteLine();
                            Console.WriteLine("Job is finished.");
                            Console.WriteLine();
                            break;
                        case JobState.Canceling:
                        case JobState.Queued:
                        case JobState.Scheduled:
                        case JobState.Processing:
                            Console.WriteLine("Please wait...\n");
                            break;
                        case JobState.Canceled:
                        case JobState.Error:
                            // Cast sender as a job.
                            IJob job = (IJob)sender;
                            // Display or log error details as needed.
                            // LogJobStop(job.Id);
                            break;
                        default:
                            break;
                    }
                }
            }
        }


##<a name="media-services-learning-paths"></a>Media Servicesi Õppeteed

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Tagasiside saatmine

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


## <a name="related-links"></a>Seotud lingid

[Azure Media Services Analytics ülevaade](media-services-analytics-overview.md)

[Azure Media Analytics demos](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)