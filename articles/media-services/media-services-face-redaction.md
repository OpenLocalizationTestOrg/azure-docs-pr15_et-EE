<properties
    pageTitle="Redaktsiooni Azure media Analytics esinema | Microsoft Azure'i"
    description="See teema näitab, kuidas nägu Azure media Analytics redigeerimine."
    services="media-services"
    documentationCenter=""
    authors="juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/12/2016"   
    ms.author="juliako;"/>
 
#<a name="face-redaction-with-azure-media-analytics"></a>Azure media Analytics näo muutmiseks

##<a name="overview"></a>Ülevaade

**Azure Media redaktor** on [Azure Media Analytics](media-services-analytics-overview.md) meediumi protsessor (MP), mis pakub scalable näo muutmiseks pilveteenuses. Näo muutmiseks võimaldab teil muuta video selleks, et blur on valitud inimesed. Kui soovite avaliku ohutus- ja meedia stsenaariumide näo muutmiseks teenust kasutada. Videomaterjali, mis sisaldab mitut nägu paar minutit võib kuluda tundi redigeerimine käsitsi, kuid see teenus nõuab näo muutmiseks protsessi vaid mõned lihtsad juhised. [Lisateabe saamiseks vt.](https://azure.microsoft.com/blog/azure-media-redactor/)

See teema annab **Azure Media redaktor** üksikasjad ja näitab, kuidas seda kasutada koos Media Services SDK .net-i jaoks.

**Azure Media redaktor** MP praegu eelvaates.

## <a name="face-redaction-modes"></a>Näo muutmiseks režiimi

Näo muutmiseks toimib avastada nägu iga video raam, ja jälgimise näo objekti nii edasi ja tagasi aeg, et sama isik saate hägune muu nurga alt. Automatiseeritud muutmiseks protsess on keerukaid ja ei mitte alati luua 100% soovitud väljund, seetõttu meediumi Analytics pakub paar võimalust muuta lõplik väljund.

Lisaks täielikult automaatne on kahe-pass töövoo, mis võimaldab selle valiku/värvisüsteemi-selection leitud nägu ID loendi kaudu. Samuti teha suvalise eest raam muudatused MP kasutab metaandmete JSON-vormingus faili. See töövoog on tükeldatud **analüüsi** ja **Redact** režiimi. Saate ühendada kaks režiimi ühe liigu, mis töötab nii tööülesannete tööd; selle režiimi nimetatakse **kombineeritud**.

###<a name="combined-mode"></a>Kombineeritud režiim

See loob redigeeritud mp4 automaatselt ilma mis tahes käsitsi sisestada.

Esitusala|Faili nimi|Märkmete
---|---|---
Sisestuskeel vara|foo.Bar|Video WMV, MOV või MP4-vormingus
Sisestuskeel config|Töö konfiguratsiooni eelmääratud|{"versiooni": "1.0', 'Valikud': {'viis':"kaupade"}}
Väljundi vara|foo_redacted.mp4|Video hägustamine rakendatud

####<a name="input-example"></a>Sisestuskeel näide:

[See video vaatamine](http://ampdemo.azureedge.net/?url=http%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fed99001d-72ee-4f91-9fc0-cd530d0adbbc%2FDancing.mp4)

####<a name="output-example"></a>Väljundi näide:

[See video vaatamine](http://ampdemo.azureedge.net/?url=http%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fc6608001-e5da-429b-9ec8-d69d8f3bfc79%2Fdance_redacted.mp4)

###<a name="analyze-mode"></a>Analüüsi režiim

**Analüüsida** töövoo kahepoolse-pass jõustamine võtab video sisend ja toodab JSON faili näo asukohtade ja iga tuvastati näo jpg pilte.

Esitusala|Faili nimi|Märkmete
---|---|----
Sisestuskeel vara|foo.Bar|Video WMV, Century või MP4-vormingus
Sisestuskeel config|Töö konfiguratsiooni eelmääratud|{"versiooni": "1.0', 'Valikud': {'viis': analüüsida"}}
Väljundi vara|foo_annotations.JSON|Marginaalide andmed näo kohtade JSON-vormingus. See saab redigeerida kasutaja muutmiseks hägustamine piira ruudud. Vaadake alltoodud valimi.
Väljundi vara|foo_thumb%06d.jpg [foo_thumb000001.jpg, foo_thumb000002.jpg]|Kärbitud jpg iga tuvastatud näo, kui arv on märgitud labelId näo

####<a name="output-example"></a>Väljundi näide:

    {
      "version": 1,
      "timescale": 50,
      "offset": 0,
      "framerate": 25.0,
      "width": 1280,
      "height": 720,
      "fragments": [
        {
          "start": 0,
          "duration": 2,
          "interval": 2,
          "events": [
            [  
              {
                "id": 1,
                "x": 0.306415737,
                "y": 0.03199235,
                "width": 0.15357475,
                "height": 0.322126418
              },
              {
                "id": 2,
                "x": 0.5625317,
                "y": 0.0868245438,
                "width": 0.149155334,
                "height": 0.355517566
              }
            ]
          ]
        },

… kärbitud


###<a name="redact-mode"></a>Redigeeri režiimis

Teine pass töövoo võtab sisendeid, mille saab ühendada ühe varade suurem arv.

See hõlmab ID blur, algse video ja marginaalid JSON loendit. Selle režiimi kasutab rakendamiseks hägustamine video sisendi marginaalid.

Analüüsi jõustamine väljund ei sisalda algse video. Video peab Sisestuskeel varade Redact režiimi tööülesande faile üles laadida ja valitud esmane failina.

Esitusala|Faili nimi|Märkmete
---|---|---
Sisestuskeel vara|foo.Bar|Video WMV, Century või MP4-vormingus. Sama, nagu samm 1 video.
Sisestuskeel vara|foo_annotations.JSON|marginaalide metaandmete faili etapp, valikuline muudatusi.
Sisestuskeel vara|foo_IDList.txt (valikuline)|Valikuline uue rea eraldatud loend eraldama ID-eurose. Kui tühjaks, see hδgustab kõik nägu.
Sisestuskeel config|Töö konfiguratsiooni eelmääratud|{"versiooni": "1.0', 'Valikud': {'viis': redigeerimine"}}
Väljundi vara|foo_redacted.mp4|Video hägustamine rakendatud põhjal marginaalid

####<a name="example-output"></a>Näide väljund

See on valitud üks ID-ga on IDList väljund.

[See video vaatamine](http://ampdemo.azureedge.net/?url=http%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fad6e24a2-4f9c-46ee-9fa7-bf05e20d19ac%2Fdance_redacted1.mp4)

##<a name="attribute-descriptions"></a>Atribuut kirjeldused

Redaktsiooni MP pakub suure täpsusega näo asukoha tuvastamise ja jälgimine, mis on tuvastanud kuni 64 inimese nägu videoraami. Eesmise nägu anda parima tulemuse, samal ajal külgtahk ja väike nägu (väiksem või võrdne 24 x 24 pikslit) on keeruline.

Tuvastatud ja jälitatud nägu tagastatakse koordinaadid, märkides nägu, samuti näo omavahel kokku asukoha ID-numbri, mis näitab, et üksikuid jälgimine. Näo ID arvud on reset juhul, kui eesmise näo on kadunud või kattuv raami, tulemuseks mõned inimesed saada määratud mitu ID-d.

Üksikasjalikud selgitused atribuute, teemat [tuvastada ja Azure Media Analytics emotsioon](media-services-face-and-emotion-detection.md) .

## <a name="sample-code"></a>Proovi kood

Järgmine programm kuvatakse kohta:

1. Looge vara ja media faili üleslaadimine rakendusse vara.
1. Looge töö näo muutmiseks ülesande konfiguratsioonifail, mis sisaldab järgmist json valmissäte põhjal. 
                    
        {'version':'1.0', 'options': {'mode':'combined'}}

1. Väljundi JSON failide allalaadimiseks. 
         
        using System;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using System.Threading;
        using System.Threading.Tasks;
        
        namespace FaceRedaction
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
        
                    // Run the FaceRedaction job.
                    var asset = RunFaceRedactionJob(@"C:\supportFiles\FaceRedaction\SomeFootage.mp4",
                                                @"C:\supportFiles\FaceRedaction\config.json");
        
                    // Download the job output asset.
                    DownloadAsset(asset, @"C:\supportFiles\FaceRedaction\Output");
                }
        
                static IAsset RunFaceRedactionJob(string inputMediaFilePath, string configurationFile)
                {
                    // Create an asset and upload the input media file to storage.
                    IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                        "My Face Redaction Input Asset",
                        AssetCreationOptions.None);
        
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("My Face Redaction Job");
        
                    // Get a reference to Azure Media Redactor.
                    string MediaProcessorName = "Azure Media Redactor";
        
                    var processor = GetLatestMediaProcessorByName(MediaProcessorName);
        
                    // Read configuration from the specified file.
                    string configuration = File.ReadAllText(configurationFile);
        
                    // Create a task with the encoding details, using a string preset.
                    ITask task = job.Tasks.AddNew("My Face Redaction Task",
                        processor,
                        configuration,
                        TaskOptions.None);
        
                    // Specify the input asset.
                    task.InputAssets.Add(asset);
        
                    // Add an output asset to contain the results of the job.
                    task.OutputAssets.AddNew("My Face Redaction Output Asset", AssetCreationOptions.None);
        
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


##<a name="next-step"></a>Järgmise juhise juurde

Vaadake üle Media Servicesi õppeteemad.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Tagasiside saatmine

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="related-links"></a>Seotud lingid

[Azure Media Services Analytics ülevaade](media-services-analytics-overview.md)

[Azure Media Analytics demos](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)
