<properties
    pageTitle="Tuvasta näo ja Azure Media Analytics emotsioon | Microsoft Azure'i"
    description="See teema näitab, kuidas nägu ja Azure Media Analytics emotsioone tuvastamiseks."
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
    ms.date="09/26/2016"   
    ms.author="milanga;juliako;"/>
 
#<a name="detect-face-and-emotion-with-azure-media-analytics"></a>Tuvasta näo ja Azure Media Analytics emotsioon

##<a name="overview"></a>Ülevaade

**Azure Media näo detektor** meediumi protsessor (MP) võimaldab teil loendada, jälgida liikumist ja isegi hinnata sihtrühma osalemist ja reaktsioon näo avaldiste kaudu. See teenus sisaldab kahte funktsioone: 

- **Näo tuvastamise**

    Näo tuvastamise leiab ja rajad inimese nägu video sees. Mitme saab tuvastada ja hiljem jälitata, nagu nad liikuda, aeg ja asukoht metaandmed, tagastatakse JSON-failis. Ajal jälgimine, proovib see anda ühtsete ID sama näo ajal isik on liikumine kuval isegi juhul, kui need on ummistada või jätke korraks paneeli.

    >[AZURE.NOTE]See teenuste sooritada näo. Isik, kes lahkub paneeli või muutub ummistada jaoks liiga pikk antakse uue ID nad tagasi.

- **Emotsioon automaattuvastus**
    
    Emotsioon tuvastamise on valikuline osa näo tuvastamise meediumi protsessor, mis tagastab analüüsi mitme emotsionaalne atribuutide nägu tuvastatud, sh õnn, kurbus, hirm, viha ja. 

**Azure Media näo detektor** MP praegu eelvaade.

See teema annab **Azure Media näo detektor** üksikasjad ja näitab, kuidas seda kasutada koos Media Services SDK .net-i jaoks.

##<a name="face-detector-input-files"></a>Esinema detektor failid

Video faile. Praegu on toetatud failivormingud: MP4, MOV ja WMV.

##<a name="face-detector-output-files"></a>Esinema detektor väljund faili

Näo tuvastamise ja selle jälgimise API pakub suure täpsusega näo asukoha tuvastamise ja jälgimine, mis tuvastab kuni 64 inimese nägu video. Eesmise nägu anda parima tulemuse, külgtahk ajal ja väike nägu (väiksem või võrdne 24 x 24 pikslit) ei pruugi olla nii täpne.

Tuvastatud ja jälitatud nägu tagastatakse koordinaadid (vasakul, üleval, laius ja kõrgus), mis näitab, nägu pikslit, samuti näo ID arvu, mis näitab, et üksikuid jälgimine pildi asukoha. Näo ID arvud on reset juhul, kui eesmise näo on kadunud või kattuv raami, tulemuseks mõned inimesed saada määratud mitu ID-d.

###<a id="output_elements"></a>JSON väljundfail elemendid

Näo tuvastamise ja jälitamisel toiming, sisaldab väljundi tulemi metaandmete nägu antud JSON-vormingus faili kaudu.

Näo tuvastamise ja jälitamine JSON hõlmab järgmisi atribuute:

Elemendi|Kirjeldus
---|---
Versioon|See viitab Video API versiooni.
Ajaskaala|"Puugid" sekundis video.
OFFSET|See on selle aja erinevus ajatemplid. Video API-de versiooni 1.0 see alati olla 0. Tulevikus toetame stsenaariumid, see väärtus võib muutuda.
Sekundis|Paneelid sekundis video.
Fragmendid|Metaandmete on chunked üles ehk fragmendid erinevate lõikudeks. Iga fragment sisaldab start, kestus, intervalli number ja sündmus (ed).
Alustamine|Algusaja "puugid" esimese sündmus.
Kestus|Pikkus fragment, klõpsake "puugid".
Intervall|Iga sündmuse kande fragmendid, klõpsake "puugid" intervalli.
Sündmused|Iga sündmuse sisaldab nägu tuvastatud ja jälitatud sees selle kestus. See on massiiv sündmuste massiivi. Väline massiiv tähistab ühe ajavahemik. Sisemine massiiv koosneb 0 või rohkem sündmusi, mis juhtus sel hetkel aega. Mõnda tühja nurksulgudega [] tähendab, et pole näod ei tuvastatud.
ID| Näo, mis on jälitatud ID-d. See arv tahtmatult võivad muutuda, kui näo omavahel kokku muutub avastamata. Teatud üksikute peaks olema sama ID kogu üldine video, kuid seda ei saa tagada tõttu piirangud on teile tuvastusalgoritm (oklusioon jne).
X JA Y|Vasakus ülanurgas X ja Y-koordinaatide piira väljal normaliseeritud skaala 0,0-1,0 näo. <br/>-X ja Y koordinaadid on suhteline horisontaalseks alati, kui teil on vertikaalseks, video (või tagurpidi iOS-i puhul), peate transponeerima koordinaate vastavalt sellele.
Laius, kõrgus|Laius ja kõrgus piira väljal normaliseeritud skaala 0,0-1,0 näo.
facesDetected|See on leitud JSON tulemused lõpus ja Kokkuvõte näod, mis algoritmi avastatud video arv. Kuna ID-d saate lähtestada tahtmatult kui näo omavahel kokku muutub avastamata (nt näo läheb ekraanilt ilmed eemal), see arv alati võib-olla ei võrdu näod video true arv.

Näo detektor kasutab võtteid killustatus (kus metaandmeid saab jagada ajapõhiste tükkideks ja saate alla laadida ainult, mida on vaja) ja jaotus (kus sündmused on murtud juhuks, kui nad saavad liiga suur). Mõned lihtsad arvutused abil saate muuta andmeid. Näiteks kui 6300 (puugid) juures alustamine sündmuse ajakava 2997 (puugid sekundis) ja sekundis, 29.97 (raamid sekundis), siis:

- Alusta/ajaskaala = 2.1 sekundi
- Sekundid (sekundis/ajaskaala) x = 63 paneelid

Allpool on lihtne näide ekstraktimiseks JSON üheks on paneeli vormingu näo tuvastamise ja selle jälgimise kohta:
    
    var faceDetectionResultJsonString = operationResult.ProcessingResult;
    var faceDetecionTracking = 
         JsonConvert.DeserializeObject<FaceDetectionResult>(faceDetectionResultJsonString, settings);


##<a name="face-detection-input-and-output-example"></a>Esinema tuvastamise sisend ja väljund näide

###<a name="input-video"></a>Sisestuskeel video

[Sisestage Video](http://ampdemo.azureedge.net/azuremediaplayer.html?url=https%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fc8834d9f-0b49-4b38-bcaf-ece2746f1972%2FMicrosoft%20Convergence%202015%20%20Keynote%20Highlights.ism%2Fmanifest&amp;autoplay=false)

###<a name="task-configuration-preset"></a>Ülesande konfigureerimine (algne)

Tööülesande koos **Azure Media näo detektoriga**loomisel peate määrama konfiguratsiooni valmissäte. Järgmised eelseadistatud konfiguratsiooni on ainult näo tuvastamise.

    {"version":"1.0"}

###<a name="json-output"></a>JSON väljund

JSON väljundi järgmises näites on kärbitud.

    {
    "version": 1,
    "timescale": 30000,
    "offset": 0,
    "framerate": 29.97,
    "width": 1280,
    "height": 720,
    "fragments": [
        {
        "start": 0,
        "duration": 60060
        },
        {
        "start": 60060,
        "duration": 60060,
        "interval": 1001,
        "events": [
            [
            {
                "id": 0,
                "x": 0.519531,
                "y": 0.180556,
                "width": 0.0867188,
                "height": 0.154167
            }
            ],
            [
            {
                "id": 0,
                "x": 0.517969,
                "y": 0.181944,
                "width": 0.0867188,
                "height": 0.154167
            }
            ],
            [
            {
                "id": 0,
                "x": 0.517187,
                "y": 0.183333,
                "width": 0.0851562,
                "height": 0.151389
            }
            ],

        . . . 

##<a name="emotion-detection-input-and-output-example"></a>Emotsioon tuvastamise sisend ja väljund näide


###<a name="input-video"></a>Sisestuskeel video

[Sisestage Video](http://ampdemo.azureedge.net/azuremediaplayer.html?url=https%3A%2F%2Freferencestream-samplestream.streaming.mediaservices.windows.net%2Fc8834d9f-0b49-4b38-bcaf-ece2746f1972%2FMicrosoft%20Convergence%202015%20%20Keynote%20Highlights.ism%2Fmanifest&amp;autoplay=false)

###<a name="task-configuration-preset"></a>Ülesande konfigureerimine (algne)

Tööülesande koos **Azure Media näo detektoriga**loomisel peate määrama konfiguratsiooni valmissäte. Järgmised eelseadistatud konfiguratsiooni määrab JSON emotsioon tuvastamise põhjal luua.
    
    {
      "version": "1.0",
      "options": {
        "aggregateEmotionWindowMs": "987",
        "mode": "aggregateEmotion",
        "aggregateEmotionIntervalMs": "342"
      }
    }


####<a name="attribute-descriptions"></a>Atribuut kirjeldused

Atribuudi nimi|Kirjeldus
---|---
Režiim|Nägu: Ainult esinema automaattuvastus <br/>AggregateEmotion: Saatja Keskmine emotsioon väärtuste kõik nägu raami sisse.
AggregateEmotionWindowMs|Kasutage, kui valitud AggregateEmotion. Saate määrata iga liitväärtuse tulemi millisekundites kasutada video pikkus.
AggregateEmotionIntervalMs|Kasutage, kui valitud AggregateEmotion. Saate määrata, milliseid sagedusega liitväärtuse tulemusi.

####<a name="aggregate-defaults"></a>Aggregate vaikesätted

Allpool on soovitatav väärtuste liitmine akna ja intervall sätted. AggregateEmotionWindowMs ei tohi olla pikem kui AggregateEmotionIntervalMs.

   |Vaikesätted (s)|Max(s)|Min(s)
---|---|---|---
AggregateEmotionWindowMs|0,5|2|0.25
AggregateEmotionIntervalMs|0,5|1|0,25

###<a name="json-output"></a>JSON väljund

JSON väljund liitväärtuse emotsioon (kärbitud):
 
    
    {
     "version": 1,
     "timescale": 30000,
     "offset": 0,
     "framerate": 29.97,
     "width": 1280,
     "height": 720,
     "fragments": [
       {
         "start": 0,
         "duration": 60060,
         "interval": 15015,
         "events": [
           [
             {
               "windowFaceDistribution": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               }
             }
           ],
           [
             {
               "windowFaceDistribution": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               }
             }
           ],
           [
             {
               "windowFaceDistribution": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               }
             }
           ],
           [
             {
               "windowFaceDistribution": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               }
             }
           ]
         ]
       },
       {
         "start": 60060,
         "duration": 60060,
         "interval": 15015,
         "events": [
           [
             {
               "windowFaceDistribution": {
                 "neutral": 1,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,
                 "contempt": 0
               },
               "windowMeanScores": {
                 "neutral": 0.688541,
                 "happiness": 0.0586323,
                 "surprise": 0.227184,
                 "sadness": 0.00945675,
                 "anger": 0.00592107,
                 "disgust": 0.00154993,
                 "fear": 0.00450447,
                 "contempt": 0.0042109
               }
             }
           ],
           [
             {
               "windowFaceDistribution": {
                 "neutral": 1,
                 "happiness": 0,
                 "surprise": 0,
                 "sadness": 0,
                 "anger": 0,
                 "disgust": 0,
                 "fear": 0,


##<a name="limitations"></a>Piirangud

- Sisestuskeel toetatud videovormingud kaasata MP4, MOV ja WMV.
- Määratav näo suurus vahemik on 24 x 24 2048 x 2048 pikslit. See vahemikust nägu ei tuvastata.
- Iga video näod tagastatud maksimumarv on 64.
- Mõned näod ei saa tuvastada tõttu tehnilised probleemid; nt väga suurte näo nurgad (juht-kujutavad) ja suure oklusioon. Eesmise ja eesmise lähedal on parimaid tulemusi.


## <a name="sample-code"></a>Proovi kood

Järgmine programm kuvatakse kohta:

1. Looge vara ja media faili üleslaadimine rakendusse vara.
1. Loob töö näo tuvastamise tööülesande konfiguratsioonifail, mis sisaldab järgmist json valmissäte põhjal. 
                    
        {
            "version": "1.0"
        }

1. Väljundi JSON failide allalaadimist. 
         
        using System;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using System.Threading;
        using System.Threading.Tasks;
        
        namespace FaceDetection
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
        
                    // Run the FaceDetection job.
                    var asset = RunFaceDetectionJob(@"C:\supportFiles\FaceDetection\BigBuckBunny.mp4",
                                                @"C:\supportFiles\FaceDetection\config.json");
        
                    // Download the job output asset.
                    DownloadAsset(asset, @"C:\supportFiles\FaceDetection\Output");
                }
        
                static IAsset RunFaceDetectionJob(string inputMediaFilePath, string configurationFile)
                {
                    // Create an asset and upload the input media file to storage.
                    IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                        "My Face Detection Input Asset",
                        AssetCreationOptions.None);
        
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("My Face Detection Job");
        
                    // Get a reference to Azure Media Face Detector.
                    string MediaProcessorName = "Azure Media Face Detector";
        
                    var processor = GetLatestMediaProcessorByName(MediaProcessorName);
        
                    // Read configuration from the specified file.
                    string configuration = File.ReadAllText(configurationFile);
        
                    // Create a task with the encoding details, using a string preset.
                    ITask task = job.Tasks.AddNew("My Face Detection Task",
                        processor,
                        configuration,
                        TaskOptions.None);
        
                    // Specify the input asset.
                    task.InputAssets.Add(asset);
        
                    // Add an output asset to contain the results of the job.
                    task.OutputAssets.AddNew("My Face Detectoion Output Asset", AssetCreationOptions.None);
        
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

[Azure Media Services Analytics ülevaade](media-services-analytics-overview.md)

[Azure Media Analytics demos](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)