<properties 
    pageTitle="Azure'i järjekorda salvestusruumi jälgimine Media Servicesi töö teatiste .net-i abil | Microsoft Azure'i" 
    description="Saate teada, kuidas kasutada Azure järjekorda salvestusruumi jälgida Media Servicesi töö teatised. Proovi kood on kirjutatud C# ja kasutab Media Services SDK .net-i jaoks." 
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
    ms.date="08/19/2016"   
    ms.author="juliako"/>

# <a name="use-azure-queue-storage-to-monitor-media-services-job-notifications-with-net"></a>Azure'i järjekorda salvestusruumi jälgimine Media Servicesi töö teatiste .net-i abil

Tööde käitamisel sageli vaja võimalus töö jälgimiseks. Saate kontrollida edenemist Azure'i järjekorda salvestusruumi abil saate jälgida Media Servicesi töö teatised (selles teemas kirjeldatud) või määratlemine on StateChanged sündmuseohjuri ( [selles](media-services-check-job-progress.md) teemas kirjeldatud.  

## <a name="use-azure-queue-storage-to-monitor-media-services-job-notifications"></a>Azure'i järjekorda salvestusruumi abil saate jälgida Media Servicesi töö teatised

Microsoft Azure Media Servicesi on võimalus pakkuda teatis sõnumeid [Azure'i järjekorda salvestusruumi](../storage-dotnet-how-to-use-queues.md#what-is) , kui meediumi töid. Selles teemas kirjeldatakse, kuidas saada teatis teadetest järjekorda salvestusruumi.

Sõnumite toimetatud järjekorda salvestusruumi pääseb juurde kõikjalt maailmas. Azure'i kuhjuda sõnumside arhitektuur on usaldusväärne ja väga paindlik. Küsitlused järjekorda salvestusruumi on soovitatav kasutada muid viise. 

Üks levinum stsenaarium kuulamiseks Media Servicesi teatised on kui teil on tekkinud sisuhaldus süsteem, mis peab täitma mõned täiendavad ülesanne on kodeering töökohtade pärast lõpetab (nt päästik järgmine juhis töövoo või sisu avaldamiseks). 

###<a name="considerations"></a>Kaalutlused

Media Servicesi rakendusi, mis kasutavad Azure storage järjekorda väljatöötamisel, võtke arvesse järgmist.

- Järjekorrad teenuse tagada ees-sisse-esimese välja (FIFO) tellitud kohaletoimetamise. Lisateavet leiate [Azure'i järjekorrad ja Azure teenuse siini järjekorrad võrreldes ja Contrasted](https://msdn.microsoft.com/library/azure/hh767287.aspx).
- Azure'i salvestusruumi järjekorrad pole lükketeatiste teenus; teil on küsitlus järjekorda. 
- Teil võib olla mõni muu arv järjekorda. Lisateavet leiate teemast [Järjekorda teenuse REST API](https://msdn.microsoft.com/library/azure/dd179363.aspx).
- Azure'i salvestusruumi järjekorrad on mõned piirangud ja üksikasjad, mida on kirjeldatud järgmises artiklis: [Azure'i järjekorrad ja Azure teenuse siini järjekorrad võrreldes ja Contrasted](https://msdn.microsoft.com/library/azure/hh767287.aspx).

###<a name="code-example"></a>Koodi näide

Selle jaotise kood teeb järgmist.

1. Määratleb **EncodingJobMessage** klassi, mida saab vastendada teate sõnumivormingut. Koodi deserializes **EncodingJobMessage** tüüpi objektid kuhjuda saadud sõnumeid.
1. Laadib Media Servicesi ja salvestusruumi kontoteabe app.config faili. Kasutab seda teavet **CloudMediaContext** ja **CloudQueue** objektide loomiseks.
1. Loob järjekord saab teatise sõnumeid kodeering töö kohta.
1. Loob teatise lõpp-punkt, mis on vastendatud järjekorda.
1. Manustab teatis lõpp-punkti töö ja esitab kodeering töö. Teil võib olla mitu teatis lõpp-punkti manustatud töö.
1. Selles näites oleme ainult huvitatud lõplik Ühendriikides töö töötlemise nii, et võtame **NotificationJobState.FinalStatesOnly** **addUus** meetodiga. 
        
        job.JobNotificationSubscriptions.AddNew(NotificationJobState.FinalStatesOnly, _notificationEndPoint);
1. Kui te kaotate NotificationJobState.All, peaksite kõik muutmine teatiste saamiseks: ootele -> ajastatud -> töötlemine -> valmis. Siiski, nagu varem, ei garanteeri Azure'i salvestusruumi järjekorrad teenuse järjestatud kohaletoimetamise. Saate kasutada ajatempli atribuut (määratletud EncodingJobMessage tippige järgmises näites) järjestuses sõnumitele. On võimalik, et saate dubleeritud teated. Atribuudi ETag (määratletud EncodingJobMessage tippige) abil duplikaatide osas kontrollida. Samuti on võimalik, et mõned muutmine teatiste jäetakse vahele. 
1. Ootab töö saada kasutusvalmis, märkides ruudu kuhjuda iga 10 sekundi järel. Kustutab sõnumeid, kui neid on töödeldud.
1. Kustutab kuhjuda ja teatis lõpp-punkti.

>[AZURE.NOTE]Soovitatav viis on töö oleku jälgimine on teatis kuulamine, nagu on näidatud järgmises näites.
>
>Teise võimalusena võib märkige kohta on töö atribuudi **IJob.State** abil.  Teavitussõnumi kohta on töö lõpetamist võib saabuvad enne riigi **IJob** on **lõpetatud**. Atribuudi **IJob.State** täpne aastalõpu veidi aega.

    
    using System;
    using System.Linq;
    using System.Configuration;
    using System.IO;
    using System.Text;
    using System.Threading;
    using System.Threading.Tasks;
    using System.Collections.Generic;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using System.Web;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;
    using Microsoft.WindowsAzure.Storage.Queue;
    using System.Runtime.Serialization.Json;
    
    namespace JobNotification
    {
        public class EncodingJobMessage
        {
            // MessageVersion is used for version control. 
            public String MessageVersion { get; set; }
        
            // Type of the event. Valid values are 
            // JobStateChange and NotificationEndpointRegistration.
            public String EventType { get; set; }
        
            // ETag is used to help the customer detect if 
            // the message is a duplicate of another message previously sent.
            public String ETag { get; set; }
        
            // Time of occurrence of the event.
            public String TimeStamp { get; set; }
    
            // Collection of values specific to the event.
    
            // For the JobStateChange event the values are:
            //     JobId - Id of the Job that triggered the notification.
            //     NewState- The new state of the Job. Valid values are:
            //          Scheduled, Processing, Canceling, Cancelled, Error, Finished
            //     OldState- The old state of the Job. Valid values are:
            //          Scheduled, Processing, Canceling, Cancelled, Error, Finished
    
            // For the NotificationEndpointRegistration event the values are:
            //     NotificationEndpointId- Id of the NotificationEndpoint 
            //          that triggered the notification.
            //     State- The state of the Endpoint. 
            //          Valid values are: Registered and Unregistered.
    
            public IDictionary<string, object> Properties { get; set; }
        }
    
        class Program
        {
            private static CloudMediaContext _context = null;
            private static CloudQueue _queue = null;
            private static INotificationEndPoint _notificationEndPoint = null;
    
            private static readonly string _singleInputMp4Path =
                Path.GetFullPath(@"C:\supportFiles\multifile\BigBuckBunny.mp4");
    
            static void Main(string[] args)
            {
                // Get the values from app.config file.
                string mediaServicesAccountName = ConfigurationManager.AppSettings["MediaServicesAccountName"];
                string mediaServicesAccountKey = ConfigurationManager.AppSettings["MediaServicesAccountKey"];
                string storageConnectionString = ConfigurationManager.AppSettings["StorageConnectionString"];
    
    
                string endPointAddress = Guid.NewGuid().ToString();
    
                // Create the context. 
                _context = new CloudMediaContext(mediaServicesAccountName, mediaServicesAccountKey);
    
                // Create the queue that will be receiving the notification messages.
                _queue = CreateQueue(storageConnectionString, endPointAddress);
    
                // Create the notification point that is mapped to the queue.
                _notificationEndPoint = 
                        _context.NotificationEndPoints.Create(
                        Guid.NewGuid().ToString(), NotificationEndPointType.AzureQueue, endPointAddress);
    
    
                if (_notificationEndPoint != null)
                {
                    IJob job = SubmitEncodingJobWithNotificationEndPoint(_singleInputMp4Path);
                    WaitForJobToReachedFinishedState(job.Id);
                }
    
                // Clean up.
                _queue.Delete();      
                _notificationEndPoint.Delete();
           }
    
    
            static public CloudQueue CreateQueue(string storageAccountConnectionString, string endPointAddress)
            {
                CloudStorageAccount storageAccount = CloudStorageAccount.Parse(storageAccountConnectionString);
    
                // Create the queue client
                CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
    
                // Retrieve a reference to a queue
                CloudQueue queue = queueClient.GetQueueReference(endPointAddress);
    
                // Create the queue if it doesn't already exist
                queue.CreateIfNotExists();
    
                return queue;
            }
     
    
            public static IJob SubmitEncodingJobWithNotificationEndPoint(string inputMediaFilePath)
            {
                // Declare a new job.
                IJob job = _context.Jobs.Create("My MP4 to Smooth Streaming encoding job");
    
                //Create an encrypted asset and upload the mp4. 
                IAsset asset = CreateAssetAndUploadSingleFile(AssetCreationOptions.StorageEncrypted, 
                    inputMediaFilePath);
    
                // Get a media processor reference, and pass to it the name of the 
                // processor to use for the specific task.
                IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");
    
                // Create a task with the conversion details, using a configuration file. 
                ITask task = job.Tasks.AddNew("My encoding Task",
                    processor,
                    "H264 Multiple Bitrate 720p",
                    Microsoft.WindowsAzure.MediaServices.Client.TaskOptions.None);
    
                // Specify the input asset to be encoded.
                task.InputAssets.Add(asset);
    
                // Add an output asset to contain the results of the job.
                task.OutputAssets.AddNew("Output asset",
                    AssetCreationOptions.None);
    
                // Add a notification point to the job. You can add multiple notification points.  
                job.JobNotificationSubscriptions.AddNew(NotificationJobState.FinalStatesOnly, 
                    _notificationEndPoint);
    
                job.Submit();
    
                return job;
            }
    
            public static void WaitForJobToReachedFinishedState(string jobId)
            {
                int expectedState = (int)JobState.Finished;
                int timeOutInSeconds = 600;
    
                bool jobReachedExpectedState = false;
                DateTime startTime = DateTime.Now;
                int jobState = -1;
    
                while (!jobReachedExpectedState)
                {
                    // Specify how often you want to get messages from the queue.
                    Thread.Sleep(TimeSpan.FromSeconds(10));
    
                    foreach (var message in _queue.GetMessages(10))
                    {
                        using (Stream stream = new MemoryStream(message.AsBytes))
                        {
                            DataContractJsonSerializerSettings settings = new DataContractJsonSerializerSettings();
                            settings.UseSimpleDictionaryFormat = true;
                            DataContractJsonSerializer ser = new DataContractJsonSerializer(typeof(EncodingJobMessage), settings);
                            EncodingJobMessage encodingJobMsg = (EncodingJobMessage)ser.ReadObject(stream);
    
                            Console.WriteLine();
    
                            // Display the message information.
                            Console.WriteLine("EventType: {0}", encodingJobMsg.EventType);
                            Console.WriteLine("MessageVersion: {0}", encodingJobMsg.MessageVersion);
                            Console.WriteLine("ETag: {0}", encodingJobMsg.ETag);
                            Console.WriteLine("TimeStamp: {0}", encodingJobMsg.TimeStamp);
                            foreach (var property in encodingJobMsg.Properties)
                            {
                                Console.WriteLine("    {0}: {1}", property.Key, property.Value);
                            }
    
                            // We are only interested in messages 
                            // where EventType is "JobStateChange".
                            if (encodingJobMsg.EventType == "JobStateChange")
                            {
                                string JobId = (String)encodingJobMsg.Properties.Where(j => j.Key == "JobId").FirstOrDefault().Value;
                                if (JobId == jobId)
                                {
                                    string oldJobStateStr = (String)encodingJobMsg.Properties.
                                                                Where(j => j.Key == "OldState").FirstOrDefault().Value;
                                    string newJobStateStr = (String)encodingJobMsg.Properties.
                                                                Where(j => j.Key == "NewState").FirstOrDefault().Value;
    
                                    JobState oldJobState = (JobState)Enum.Parse(typeof(JobState), oldJobStateStr);
                                    JobState newJobState = (JobState)Enum.Parse(typeof(JobState), newJobStateStr);
    
                                    if (newJobState == (JobState)expectedState)
                                    {
                                        Console.WriteLine("job with Id: {0} reached expected state: {1}", 
                                            jobId, newJobState);
                                        jobReachedExpectedState = true;
                                        break;
                                    }
                                }
                            }
                        }
                        // Delete the message after we've read it.
                        _queue.DeleteMessage(message);
                    }
    
                    // Wait until timeout
                    TimeSpan timeDiff = DateTime.Now - startTime;
                    bool timedOut = (timeDiff.TotalSeconds > timeOutInSeconds);
                    if (timedOut)
                    {
                        Console.WriteLine(@"Timeout for checking job notification messages, 
                                            latest found state ='{0}', wait time = {1} secs",
                            jobState,
                            timeDiff.TotalSeconds);
    
                        throw new TimeoutException();
                    }
                }
            }
       
            static private IAsset CreateAssetAndUploadSingleFile(AssetCreationOptions assetCreationOptions, string singleFilePath)
            {
                var asset = _context.Assets.Create("UploadSingleFile_" + DateTime.UtcNow.ToString(), 
                    assetCreationOptions);
    
                var fileName = Path.GetFileName(singleFilePath);
    
                var assetFile = asset.AssetFiles.Create(fileName);
    
                Console.WriteLine("Created assetFile {0}", assetFile.Name);
                Console.WriteLine("Upload {0}", assetFile.Name);
    
                assetFile.Upload(singleFilePath);
                Console.WriteLine("Done uploading of {0}", assetFile.Name);
    
                return asset;
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

Ülaltoodud näites toodeti järgmine väljund. Saate väärtused erinevad.
    
    Created assetFile BigBuckBunny.mp4
    Upload BigBuckBunny.mp4
    Done uploading of BigBuckBunny.mp4
    
    EventType: NotificationEndPointRegistration
    MessageVersion: 1.0
    ETag: e0238957a9b25bdf3351a88e57978d6a81a84527fad03bc23861dbe28ab293f6
    TimeStamp: 2013-05-14T20:22:37
        NotificationEndPointId: nb:nepid:UUID:d6af9412-2488-45b2-ba1f-6e0ade6dbc27
        State: Registered
        Name: dde957b2-006e-41f2-9869-a978870ac620
        Created: 2013-05-14T20:22:35
    
    EventType: JobStateChange
    MessageVersion: 1.0
    ETag: 4e381f37c2d844bde06ace650310284d6928b1e50101d82d1b56220cfcb6076c
    TimeStamp: 2013-05-14T20:24:40
        JobId: nb:jid:UUID:526291de-f166-be47-b62a-11ffe6d4be54
        JobName: My MP4 to Smooth Streaming encoding job
        NewState: Finished
        OldState: Processing
        AccountName: westeuropewamsaccount
    job with Id: nb:jid:UUID:526291de-f166-be47-b62a-11ffe6d4be54 reached expected 
    State: Finished
    

## <a name="next-step"></a>Järgmise juhise juurde

Vaadake üle Media Servicesi õppeteemad

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Tagasiside saatmine

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
