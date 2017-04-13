<properties 
    pageTitle="Täpsemad kodeeringus Media kodeerija Premium töövoo | Microsoft Azure'i" 
    description="Saate teada, kuidas kodeerida Media kodeerija Premium töövooga. Koodinäiteid C# kirjutada ja kasutada Media Services SDK .net-i jaoks." 
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

#<a name="advanced-encoding-with-media-encoder-premium-workflow"></a>Täpsem kodeerimine Media kodeerija Premium töövooga

>[AZURE.NOTE] Selles teemas kirjeldatud Media kodeerija Premium töövoo meediumi protsessor pole saadaval Hiinas.

Premium kodeerija küsimusi, e-posti mepd veebisaidil Microsoft.com.

##<a name="overview"></a>Ülevaade

Microsoft Azure Media Servicesi on **Media kodeerija Premium töövoo** meediumi protsessor tutvustus. See protsessor pakutakse advance kodeerimine võimalused oma premium nõudmisel töövood. 

Järgmistes teemades liigendamine **Media kodeerija Premium töövoo**seotud üksikasjad: 

- [Vormingute toetatud Media kodeerija Premium töövoo](media-services-premium-workflow-encoder-formats.md) – Discusses faili vormingud ja kodekite **Media kodeerija Premium töövoog**ei toeta.

- Jaotise [võrdlus kodeerimisseadmetest](media-services-encode-asset.md#compare_encoders) võrdleb kodeering võimaluste **Media kodeerija Premium töövoog** ja **Media kodeerija standardne**.

See teema näitab, kuidas kodeerida **Media kodeerija Premium töövoo** .net-i abil.

Kodeering tööülesannete **Media kodeerija Premium töövoo** jaoks vaja eraldi konfiguratsioonifail, nimetatakse töövoo faili. Need failid on .workflow laiendamine ja luuakse [Töövoo disaineri](media-services-workflow-designer.md) tööriista abil.

##<a name="encode"></a>Kodeerida

Kodeering tööülesannete **Media kodeerija Premium töövoo** jaoks vaja eraldi konfiguratsioonifail, nimetatakse töövoo faili. Need failid on .workflow laiendamine ja luuakse [Töövoo disaineri](media-services-workflow-designer.md) tööriista abil.


Samuti saate vaikimisi töövoo failid [siia](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows). See kaust sisaldab ka need failid kirjeldust.

Töövoo failid on vaja aktiva konto Media Servicesi üles laadida ja selle vara tuleks edastada kodeering tööülesanne.

Järgmises näites näitab, kuidas kodeerida **Media kodeerija Premium**töövooga. 

Tehakse järgmist: 
 
1. Looge vara ja töövoo faili üles laadida. 
2. Vara loomine ja allika meediumi faili üles laadida.
3. Saada "Media kodeerija Premium töövoo" meediumi protsessor.
4. Töö ja tööülesande loomine. 

    Enamikul juhtudel ülesande konfigureerimine string on tühi (nagu järgmises näites). On mõned täpsemalt stsenaariumid (mis nõuavad juurde dünaamiliselt käitusaja atribuutide seadmine) Sel juhul saate annaks XML-string, kodeering tööülesannet. Näiteid sellised stsenaariumid: luua mõne ülekatet, paralleelne või järjestikune meediumi õmblemisega, subtiitrimine.
5. Lisage kaks Sisestuskeel varad tööülesande.
    
    lisamine. 1. – töövoo vara.

    b. 2. – video vara.
    
    **Märkus**: varade töövoog tuleb lisada meediumi varade enne tööülesande. Konfiguratsiooni stringi selle toimingu jaoks peab olema tühi. 

6. Esitage kodeering töö.

Järgnevalt on lõpule viidud näide. Media Services .NET arengu häälestamise kohta leiate teavet teemast [Media Services arendamiseni .net-i](media-services-dotnet-how-to-use.md).


    using System; 
    using System.Linq;
    using System.Configuration;
    using System.IO;
    using System.Text;
    using System.Threading;
    using System.Threading.Tasks;
    using System.Collections.Generic;
    using Microsoft.WindowsAzure;
    using Microsoft.WindowsAzure.MediaServices.Client;
    
    namespace MediaEncoderPremiumWorkflowSample
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
    
            private static readonly string _supportFiles =
                Path.GetFullPath(@"../..\Media");
    
            private static readonly string _workflowFilePath =
                Path.GetFullPath(_supportFiles + @"\H264 Progressive Download MP4.workflow");
            
            private static readonly string _singleMP4InputFilePath =
                Path.GetFullPath(_supportFiles + @"\BigBuckBunny.mp4");
    
    
            static void Main(string[] args)
            {
                // Create and cache the Media Services credentials in a static class variable.
                _cachedCredentials = new MediaServicesCredentials(
                                _mediaServicesAccountName,
                                _mediaServicesAccountKey);
    
                // Used the cached credentials to create CloudMediaContext.
                _context = new CloudMediaContext(_cachedCredentials);
    
                var workflowAsset = CreateAssetAndUploadSingleFile(_workflowFilePath);
                var videoAsset = CreateAssetAndUploadSingleFile(_singleMP4InputFilePath);
                IAsset outputAsset = CreateEncodingJob(workflowAsset, videoAsset); 
    
            }
    
            static public IAsset CreateAssetAndUploadSingleFile(string singleFilePath)
            {
                var assetName = "UploadSingleFile_" + DateTime.UtcNow.ToString();
                var asset = _context.Assets.Create(assetName, AssetCreationOptions.None);
    
                var fileName = Path.GetFileName(singleFilePath);
    
                var assetFile = asset.AssetFiles.Create(fileName);
    
                Console.WriteLine("Created assetFile {0}", assetFile.Name);
    
                var accessPolicy = _context.AccessPolicies.Create(assetName, TimeSpan.FromDays(30),
                                                                    AccessPermissions.Write | AccessPermissions.List);
    
                var locator = _context.Locators.CreateLocator(LocatorType.Sas, asset, accessPolicy);
    
                Console.WriteLine("Upload {0}", assetFile.Name);
    
                assetFile.Upload(singleFilePath);
                Console.WriteLine("Done uploading {0}", assetFile.Name);
    
                locator.Delete();
                accessPolicy.Delete();
    
                return asset;
            }
    
            static public IAsset CreateEncodingJob(IAsset workflow, IAsset video)
            {
                // Declare a new job.
                IJob job = _context.Jobs.Create("Premium Workflow encoding job");
                // Get a media processor reference, and pass to it the name of the 
                // processor to use for the specific task.
                IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Premium Workflow");
    
                // Create a task with the encoding details, using a string preset.
                ITask task = job.Tasks.AddNew("Premium Workflow encoding task",
                    processor,
                    "",
                    TaskOptions.None);
    
                // Specify the input asset to be encoded.
                task.InputAssets.Add(workflow);
                task.InputAssets.Add(video); // we add one asset
                // Add an output asset to contain the results of the job. 
                // This output is specified as AssetCreationOptions.None, which 
                // means the output asset is not encrypted. 
                task.OutputAssets.AddNew("Output asset",
                    AssetCreationOptions.None);
    
                // Use the following event handler to check job progress.  
                job.StateChanged += new
                        EventHandler<JobStateChangedEventArgs>(StateChanged);
    
                // Launch the job.
                job.Submit();
    
                // Check job execution and wait for job to finish. 
                Task progressJobTask = job.GetExecutionProgressTask(CancellationToken.None);
                progressJobTask.Wait();
    
                // If job state is Error the event handling 
                // method for job progress should log errors.  Here we check 
                // for error state and exit if needed.
                if (job.State == JobState.Error)
                {
                    throw new Exception("\nExiting method due to job error.");
                }
    
                return job.OutputMediaAssets[0];
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
                        LogJobStop(job.Id);
                        break;
                    default:
                        break;
                }
            }
    
            static private void LogJobStop(string jobId)
            {
                StringBuilder builder = new StringBuilder();
                IJob job = _context.Jobs.Where(j => j.Id == jobId).FirstOrDefault();
    
                builder.AppendLine("\nThe job stopped due to cancellation or an error.");
                builder.AppendLine("***************************");
                builder.AppendLine("Job ID: " + job.Id);
                builder.AppendLine("Job Name: " + job.Name);
                builder.AppendLine("Job State: " + job.State.ToString());
                builder.AppendLine("Job started (server UTC time): " + job.StartTime.ToString());
                builder.AppendLine("Media Services account name: " + _mediaServicesAccountName);
                // Log job errors if they exist.  
                if (job.State == JobState.Error)
                {
                    builder.Append("Error Details: \n");
                    foreach (ITask task in job.Tasks)
                    {
                        foreach (ErrorDetail detail in task.ErrorDetails)
                        {
                            builder.AppendLine("  Task Id: " + task.Id);
                            builder.AppendLine("    Error Code: " + detail.Code);
                            builder.AppendLine("    Error Message: " + detail.Message + "\n");
                        }
                    }
                }
                builder.AppendLine("***************************\n");
    
                Console.Write(builder.ToString());
            }
    
    
            static private IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
            {
                var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
                    ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();
    
                if (processor == null)
                    throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));
    
                return processor;
            }
        }
    }


Premium kodeerija küsimusi, e-posti mepd veebisaidil Microsoft.com.

##<a name="media-services-learning-paths"></a>Media Servicesi Õppeteed

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Tagasiside saatmine

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
