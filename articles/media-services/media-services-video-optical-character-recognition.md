<properties
    pageTitle="Digitaalse tekstiks teisendada teksti sisu videofailide Azure Media Analytics abil | Microsoft Azure'i"
    description="Azure Media Analytics OCR (OCR) võimaldab teil teksti sisu video faile redigeerida, otsida digitaalse tekstiks teisendada.  See võimaldab teil mõtestatud metaandmete ekstraktimine video signaal meediumite automatiseerimiseks."
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
    ms.author="juliako"/>
 
#<a name="use-azure-media-analytics-to-convert-text-content-in-video-files-into-digital-text"></a>Digitaalse tekstiks teisendada teksti sisu videofailide Azure Media Analytics abil 

##<a name="overview"></a>Ülevaade

Kui soovite teksti sisu ekstraktida video faile ja luua, redigeerida, otsida digitaalse teksti, kasutage Azure Media Analytics OCR (OCR). See Azure Media Protsessor tuvastab teie video faile teksti sisu ja loob tekstifailid kasutamiseks. OCR võimaldab mõtestatud metaandmete ekstraktimine video signaal meediumite automatiseerida.

Kui kasutada koos otsimootor, saate hõlpsasti indekseerida meediumifailide teksti alusel, ja parandada oma sisu leitavust. See on väga kasulik tugevalt teksti video, nt videosalvestise või ekraanipildi slaidiseansi esitluse. Azure'i OCR-i meediumi protsessor on optimeeritud digitaalse teksti.

**Azure Media OCR** meediumi protsessor on praegu eelvaade.

See teema annab **Azure Media OCR** üksikasjad ja näitab, kuidas seda kasutada koos Media Services SDK .net-i jaoks. [Lisateabe saamiseks ja näiteid, vt.](https://azure.microsoft.com/blog/announcing-video-ocr-public-preview-new-config/)

##<a name="ocr-input-files"></a>OCR failid

Video faile. Praegu on toetatud failivormingud: MP4, MOV ja WMV.

##<a name="task-configuration"></a>Ülesande konfigureerimine 

Tööülesande konfiguratsioon (algne). Tööülesande **Azure Media**OCR loomisel peate määrama konfiguratsiooni eelmääratud JSON- või XML-i abil. 

###<a name="attribute-descriptions"></a>Atribuut kirjeldused

Atribuudi nimi|Kirjeldus
---|---
Keel|(valikuline) kirjeldatakse keele tekst, mille kohta soovite vaadata. Ühte järgmistest: automaatne tuvastamine (vaikeväärtus), Araabia, ChineseSimplified, ChineseTraditional, Tšehhi, Taani, hollandi, inglise, Soome, prantsuse, saksa, Kreeka, Ungari, Itaalia, Jaapani, Korea, Norra, Poola, Portugali, Rumeenia, vene, Serbia (kirillitsa), SerbianLatin, slovaki, Hispaania, Rootsi, Türgi.
TextOrientation|(valikuline) kirjeldatakse jaoks, kust leiate teksti suuna.  "Vasak" tähendab, et kõik tähed ülaosas on märgitud vasakule poole.  Vaikimisi teksti (nt seda, mis võib leida raamatu) saab kutsuda "Kuni" saamine.  Ühte järgmistest: automaatne tuvastamine (vaikeväärtus), üles, paremale, alla, vasakule.
TimeInterval|(valikuline) kirjeldatakse proovide arv.  Vaikimisi on iga teine 1/2.<br/>JSON-vormingus – HH:mm:ss. SSS (vaikimisi 00:00:00.500)<br/>XML-vorming – W3C XSD kestus primitiivne (vaikimisi PT0.5)
DetectRegions|(valikuline) Massiivi DetectRegion objektide määramine piirkondade videot, kus teksti tuvastamiseks.<br/>DetectRegion objekt on valmistatud neli täisarvu järgmised väärtused:<br/>Vasak – pikslit vasakule veerisepuudust kaudu<br/>Top – pikslit ülaveeris – kaudu<br/>Laius – laius pikslites piirkonna<br/>Kõrgus – kõrgus pikslites piirkonna

####<a name="json-preset-example"></a>JSON eelmääratud näide
        
    {
        'Version':'1.0', 
        'Options': 
        {
        'Language':'English', 
            'TimeInterval':'00:00:01.5',
            'DetectRegions': 
             [
                {'Left':'1','Top':'1','Width':'1','Height':'1'},
                {'Left':'2','Top':'2','Width':'2','Height':'2'}
             ],
            'TextOrientation':'Up'
        }
    }

####<a name="xml-preset-example"></a>XML-i eelmääratud näide

    <?xml version=""1.0"" encoding=""utf-16""?>
    <VideoOcrPreset xmlns:xsi=""http://www.w3.org/2001/XMLSchema-instance"" xmlns:xsd=""http://www.w3.org/2001/XMLSchema"" Version=""1.0"" xmlns=""http://www.windowsazure.com/media/encoding/Preset/2014/03"">
      <Options>
         <Language>English</Language>
         <TimeInterval>PT1.5S</TimeInterval>
         <DetectRegions>
             <DetectRegion>
                   <Left>1</Left>
                   <Top>1</Top>
                   <Width>1</Width>
                   <Height>1</Height>
            </DetectRegion>
       </DetectRegions>
       <TextOrientation>Up</TextOrientation>
      </Options>
    </VideoOcrPreset>

##<a name="ocr-output-files"></a>OCR väljund faili

OCR meediumi protsessor väljund on JSON faili.

###<a name="elements-of-the-output-json-file"></a>JSON väljundfail elemendid

Video OCR-i väljundi pakub andmete aja segmenditud video leitud märkide.  Saate atribuudid, nagu keel või suuna hone-in täpselt sõnad, et olete huvitatud analüüsimine. 

Väljund sisaldab järgmisi atribuute:

Elemendi|Kirjeldus
---|---
Ajaskaala|"puugid" video sekundis
OFFSET|ajavahe ajatemplid aeg. Video API-de versiooni 1.0 see alati olla 0.
Sekundis|Paneelid video sekundis
laius|laius pikslites video
kõrgus|kõrgus pikslites video
Fragmendid|massiivi Ajapõhiste osa video, kuhu on chunked metaandmeid.
Alustamine|alguskellaaeg fragment "puugid"
Kestus|pikkus fragment "puugid"
intervall|intervall iga sündmuse antud fragment sees.
sündmused|massiivi sisaldava piirkondade
piirkond|objekti tähistav tuvastatud sõnad või fraasid
keel|piirkonna tuvastatud teksti keelele
suund|piirkonna tuvastatud teksti suuna
read|massiivi tuvastatud piirkonna tekstirida.
teksti|tegelik tekst

###<a name="json-output-example"></a>JSON väljundi näide

Järgmises näites väljundi sisaldab video Üldteave ja mitme video fragmendid. Iga video fragment see sisaldab iga piirkonna, mida ei tuvastanud OCR-i MP keeles ja selle teksti suund. Piirkonna sisaldab ka iga sõna joon selle piirkonna rea teksti, rea asukoht ja iga sõna teave (word sisu, asukoha ja confidence) real. Järgmises näites ja mõned tekstisiseste kommentaaride panna.

    {
        "version": 1, 
        "timescale": 90000, 
        "offset": 0, 
        "framerate": 30, 
        "width": 640, 
        "height": 480,  // general video information
        "fragments": [
            {
                "start": 0, 
                "duration": 180000, 
                "interval": 90000,  // the time information about this fragment
                "events": [
                    [
                       { 
                            "region": { // the detected region array in this fragment 
                                "language": "English",  // region language
                                "orientation": "Up",  // text orientation
                                "lines": [  // line information array in this region, including the text and the position
                                    {
                                        "text": "One Two", 
                                        "left": 10, 
                                        "top": 10, 
                                        "right": 210, 
                                        "bottom": 110, 
                                        "word": [  // word information array in this line
                                            {
                                                "text": "One", 
                                                "left": 10, 
                                                "top": 10, 
                                                "right": 110, 
                                                "bottom": 110, 
                                                "confidence": 900
                                            }, 
                                            {
                                                "text": "Two", 
                                                "left": 110, 
                                                "top": 10, 
                                                "right": 210, 
                                                "bottom": 110, 
                                                "confidence": 910
                                            }
                                        ]
                                    }
                                ]
                            }
                        }
                    ]
                ]
            }
        ]
    }

## <a name="sample-code"></a>Proovi kood

Järgmine programm kuvatakse kohta:

1. Looge vara ja media faili üleslaadimine rakendusse vara.
1. Loob töö OCR konfiguratsiooni/valmissäte faili.
1. Väljundi JSON failide allalaadimist. 
         
        using System;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using System.Threading;
        using System.Threading.Tasks;
        
        namespace OCR
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
        
                    // Run the OCR job.
                    var asset = RunOCRJob(@"C:\supportFiles\OCR\presentation.mp4",
                                                @"C:\supportFiles\OCR\config.json");
        
                    // Download the job output asset.
                    DownloadAsset(asset, @"C:\supportFiles\OCR\Output");
                }
        
                static IAsset RunOCRJob(string inputMediaFilePath, string configurationFile)
                {
                    // Create an asset and upload the input media file to storage.
                    IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
                        "My OCR Input Asset",
                        AssetCreationOptions.None);
        
                    // Declare a new job.
                    IJob job = _context.Jobs.Create("My OCR Job");
        
                    // Get a reference to Azure Media OCR.
                    string MediaProcessorName = "Azure Media OCR";
        
                    var processor = GetLatestMediaProcessorByName(MediaProcessorName);
        
                    // Read configuration from the specified file.
                    string configuration = File.ReadAllText(configurationFile);
        
                    // Create a task with the encoding details, using a string preset.
                    ITask task = job.Tasks.AddNew("My OCR Task",
                        processor,
                        configuration,
                        TaskOptions.None);
        
                    // Specify the input asset.
                    task.InputAssets.Add(asset);
        
                    // Add an output asset to contain the results of the job.
                    task.OutputAssets.AddNew("My OCR Output Asset", AssetCreationOptions.None);
        
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
