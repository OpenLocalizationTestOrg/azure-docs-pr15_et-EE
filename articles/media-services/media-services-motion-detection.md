<properties
    pageTitle="Tuvasta liikumiste Azure Media Analytics | Microsoft Azure'i"
    description="Azure Media liikumisandur meediumi protsessor (MP) võimaldab teil tuvastada tõhus jaotiste huvi muidu pikk ja sündmustevaene video."
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
    ms.date="10/10/2016"  
    ms.author="milanga;juliako;"/>
 
# <a name="detect-motions-with-azure-media-analytics"></a>Tuvasta liikumiste Azure Media Analytics

##<a name="overview"></a>Ülevaade

**Azure Media liikumisandur** meediumi protsessor (MP) võimaldab teil tuvastada tõhus jaotiste huvi muidu pikk ja sündmustevaene video. Liikumistee tuvastamise saab kasutada staatilise kaamera video lõigud, kus ilmneb liikumistee tuvastamiseks. See loob JSON faili, mis sisaldab soovitud metaandmete ajatemplid ja piiritlusboksi piirkond, kus sündmus toimus.

See tehnoloogia suunatud turvalisus video kanalid, on võimalik liigitada liikumistee oluline sündmused ja false positiivsed, näiteks varje ja valgustus muutusi. See võimaldab teil luua Turbeteatiste kaamera kanalite ilma seda rämpsposti lõputu oluline sündmusi, samal ajal ekstraktida astunud huvi väga kaua järelevalve videod.

**Azure Media liikumisandur** MP praegu eelvaade.

See teema annab **Azure Media liikumisandur** üksikasjad ja näitab, kuidas seda kasutada koos Media Services SDK .net-i jaoks


##<a name="motion-detector-input-files"></a>Liikumistee detektor failid

Video faile. Praegu on toetatud failivormingud: MP4, MOV ja WMV.

##<a name="task-configuration-preset"></a>Ülesande konfigureerimine (algne)

Tööülesande koos **Azure Media liikumisandur**loomisel peate määrama konfiguratsiooni valmissäte. 

###<a name="parameters"></a>Parameetrid

Saate kasutada järgmisi.

Nimi|Suvandid|Kirjeldus|Vaikimisi
---|---|---|---
sensitivityLevel|Stringi: 'madal, "Keskmine", "kõrge"|Määrab tundlikkus taseme mis liikumiste on teatatud. Saate reguleerida seda reguleerida vale-positiivsed suurust.|"Keskmine"
frameSamplingValue|Positiivse täisarvu|Määrab sagedus mis algoritmi töötab. 1 võrdub iga raami, 2 tähendab, et iga paneeli 2 jne.|1
detectLightChange|Kahendmuutujaga: "true", "false"|Määrab, kas hele muudatused on teatatud tulemused|"False"
mergeTimeThreshold|XS-kellaaeg: Hh:mm:ss<br/>Näide: 00:00:03|Saate määrata aknas vahel liikumist sündmusi, kus 2 sündmuste kombineerida ja staatusega 1.|00:00:00
detectionZones|Massiivi tuvastamise tsoonid:<br/>-Tuvastamise Zone on massiiv 3 või mitme punkti<br/>-Punktist on x ja y koordinaatide vahemikus 0 kuni 1.|Kirjeldatakse hulknurga tuvastamise tsoonid kasutatav loend.<br/>Tulemite staatusega ID, kusjuures esimene "id" ja tsoonid: 0|Ühe tsooni, mis hõlmab kogu paneeli.

###<a name="json-example"></a>JSON näide

    
    {
      'version': '1.0',
      'options': {
        'sensitivityLevel': 'medium',
        'frameSamplingValue': 1,
        'detectLightChange': 'False',
        "mergeTimeThreshold":
        '00:00:02',
        'detectionZones': [
          [
            {'x': 0, 'y': 0},
            {'x': 0.5, 'y': 0},
            {'x': 0, 'y': 1}
           ],
          [
            {'x': 0.3, 'y': 0.3},
            {'x': 0.55, 'y': 0.3},
            {'x': 0.8, 'y': 0.3},
            {'x': 0.8, 'y': 0.55},
            {'x': 0.8, 'y': 0.8},
            {'x': 0.55, 'y': 0.8},
            {'x': 0.3, 'y': 0.8},
            {'x': 0.3, 'y': 0.55}
          ]
        ]
      }
    }


##<a name="motion-detector-output-files"></a>Liikumistee detektor väljund faili

Liikumistee tuvastamise töö naaseb JSON faili väljund vara, kus kirjeldatakse liikumistee teatised ja nende rühmadest video. Fail sisaldab teavet aja ja liikumistee tuvastatud video kestus.

Liikumistee detektor API pakub näidikud, kui on objekte liikuma fikseeritud tausta video (nt järelevalve video). Liikumisandur õpetatakse vähendada valed kutsed, nt valgustus ja vari muutused. Praeguse piirangud algoritmide kaasata öösel nägemispuudega videod, poolläbipaistev objektid ja väikeste objektide.

###<a id="output_elements"></a>JSON väljundfail elemendid

>[AZURE.NOTE]Uusim versioon, väljundi JSON-vormingus on muutunud ja võib tähistada mõne muudatuse server.

Järgmises tabelis kirjeldatakse JSON väljundfail elemendid.

Elemendi|Kirjeldus
---|---
Versioon|See viitab Video API versiooni. Praegune versioon on 2.
Ajaskaala|"Puugid" sekundis video.
OFFSET|Ajatemplid rakenduses "puugid" aja erinevus. Video API-de versiooni 1.0 see alati olla 0. Tulevikus toetame stsenaariumid, see väärtus võib muutuda.
Sekundis|Paneelid sekundis video.
Laius, kõrgus|Viitab laius ja kõrgus pikslites video.
Alustamine|"Puugid" start ajatempliga.
Kestus|Sündmuse "puugid" pikkus.
Intervall|Iga kirje korral, "puugid" intervalli.
Sündmused|Iga sündmuse fragment sisaldab liikumist, mõõta selle kestus.
Tüüp|Praegune versioon on alati "2" üldine liikumist. See silt annab Video API-de paindlikkust liigitamiseks liikuma tulevikus versioonid.
RegionID|Nagu eespool kirjeldatud, see alati on selles versioonis 0. Sildi kaudu Video API otsimine liikumistee eri piirkondade tulevaste versioonides.
Piirkondade|Viitab video alal, kui teid huvitab liikumist. <br/><br/>-"id" tähistab piirkond ala – selles versioonis on ainult üks ID 0. <br/>-tähistab "tüüp" piirkond, mis teid huvitab jaoks liikumistee kuju. Praegu "ristküliku" ja "Hulknurk" on toetatud.<br/> Kui määrasite "ristküliku", piirkonnas on mõõtmed x Y, laius ja kõrgus. X ja Y-koordinaate tähistavad normaliseeritud skaala 0,0-1,0 piirkonna ülemises vasakus XY koordinaate. Laius ja kõrgus tähistavad normaliseeritud skaala 0,0-1,0 piirkonna suurust. Praegune versioon, X, Y, laius ja kõrgus on alati suuruseks 0, 0 ja 1, 1. <br/>Kui määrasite "Hulknurk", et piirkonnas on mõõtmed punktides. <br/>
Fragmendid|Metaandmete on chunked üles ehk fragmendid erinevate lõikudeks. Iga fragment sisaldab start, kestus, intervalli number ja sündmus (ed). Fragment pole sündmuste tähendab, et pole liikumistee tuvastati ajal, mis algusaeg ja kestus.
Sulgudes]|Iga jaoks tähistab ühe väärtuse korral. Tühi sulgudes selle intervall tähendab, et pole liikumistee tuvastati.
asukohad|Uue kirje sündmuste jaotises loendid liikumise toimumise asukoht. See on täpsem kui tuvastamise tsoonid.

Järgmine on näide JSON väljund

    {
      "version": 2,
      "timescale": 23976,
      "offset": 0,
      "framerate": 24,
      "width": 1280,
      "height": 720,
      "regions": [
        {
          "id": 0,
          "type": "polygon",
          "points": [{'x': 0, 'y': 0},
            {'x': 0.5, 'y': 0},
            {'x': 0, 'y': 1}]
        }
      ],
      "fragments": [
        {
          "start": 0,
          "duration": 226765
        },
        {
          "start": 226765,
          "duration": 47952,
          "interval": 999,
          "events": [
            [
              {
                "type": 2,
                "typeName": "motion",
                "locations": [
                  {
                    "x": 0.004184,
                    "y": 0.007463,
                    "width": 0.991667,
                    "height": 0.985185
                  }
                ],
                "regionId": 0
              }
            ],
    
    …
##<a name="limitations"></a>Piirangud

- Sisestuskeel toetatud videovormingud kaasata MP4, MOV ja WMV.
- Liikumistee tuvastamise on optimeeritud paigal tausta videod. Algoritmi keskendub valed kutsed, nt valgustus muutusi ja varjud vähendamine.
- Mõned liikumistee ei saa tuvastada tõttu tehnilised probleemid; nt öösel nägemispuudega videod, poolläbipaistev objektid ja väikeste objektide.


## <a name="sample-code"></a>Proovi kood

Järgmine programm kuvatakse kohta:

1. Looge vara ja media faili üleslaadimine rakendusse vara.
1. Loob video liikumistee tuvastamise tööülesande põhjal konfiguratsioonifail, mis sisaldab järgmist json valmissäte tööd. 
                    
        {
          'Version': '1.0',
          'Options': {
            'SensitivityLevel': 'medium',
            'FrameSamplingValue': 1,
            'DetectLightChange': 'False',
            "MergeTimeThreshold":
            '00:00:02',
            'DetectionZones': [
              [
                {'x': 0, 'y': 0},
                {'x': 0.5, 'y': 0},
                {'x': 0, 'y': 1}
               ],
              [
                {'x': 0.3, 'y': 0.3},
                {'x': 0.55, 'y': 0.3},
                {'x': 0.8, 'y': 0.3},
                {'x': 0.8, 'y': 0.55},
                {'x': 0.8, 'y': 0.8},
                {'x': 0.55, 'y': 0.8},
                {'x': 0.3, 'y': 0.8},
                {'x': 0.3, 'y': 0.55}
              ]
            ]
          }
        }

1. Väljundi JSON failide allalaadimist. 
         
        using System;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using System.Threading;
        using System.Threading.Tasks;
        
        namespace VideoMotionDetection
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
        
                    // Run the VideoMotionDetection job.
                    var asset = RunVideoMotionDetectionJob(@"C:\supportFiles\VideoMotionDetection\BigBuckBunny.mp4",
                                                @"C:\supportFiles\VideoMotionDetection\config.json");
        
                    // Download the job output asset.
                    DownloadAsset(asset, @"C:\supportFiles\VideoMotionDetection\Output");
                }
        
                static IAsset RunVideoMotionDetectionJob(string inputMediaFilePath, string configurationFile)
                {
                    // Create an asset and upload the input media file to storage.
                    IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                        "My Video Motion Detection Input Asset",
                        AssetCreationOptions.None);
        
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("My Video Motion Detection Job");
        
                    // Get a reference to Azure Media Motion Detector.
                    string MediaProcessorName = "Azure Media Motion Detector";
        
                    var processor = GetLatestMediaProcessorByName(MediaProcessorName);
        
                    // Read configuration from the specified file.
                    string configuration = File.ReadAllText(configurationFile);
        
                    // Create a task with the encoding details, using a string preset.
                    ITask task = job.Tasks.AddNew("My Video Motion Detection Task",
                        processor,
                        configuration,
                        TaskOptions.None);
        
                    // Specify the input asset.
                    task.InputAssets.Add(asset);
        
                    // Add an output asset to contain the results of the job.
                    task.OutputAssets.AddNew("My Video Motion Detectoion Output Asset", AssetCreationOptions.None);
        
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

##<a name="related-links"></a>Seotud lingid
[Azure Media Services liikumisandur ajaveeb](https://azure.microsoft.com/blog/motion-detector-update/)

[Azure Media Services Analytics ülevaade](media-services-analytics-overview.md)

[Azure Media Analytics demos](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)
