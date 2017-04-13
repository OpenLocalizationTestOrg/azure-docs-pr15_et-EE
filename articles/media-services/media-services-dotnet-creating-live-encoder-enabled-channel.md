<properties 
    pageTitle="Kuidas teha reaalajas streaming Azure Media Servicesi abil saate luua mitme bitikiirusega voogu .NET | Microsoft Azure'i" 
    description="Selles õppeteemas tutvustatakse juhiseid, mis saab ühe bitikiirusega reaalajas voo ja kodeeritakse see mitme bitikiirusega voo kasutades .NET SDK kanali loomine." 
    services="media-services" 
    documentationCenter="" 
    authors="anilmur" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article"
    ms.date="10/12/2016"
    ms.author="juliako;anilmur"/>


#<a name="how-to-perform-live-streaming-using-azure-media-services-to-create-multi-bitrate-streams-with-net"></a>Kuidas teha reaalajas streaming loomine mitme bitikiirusega voogu .NET Azure Media Servicesi kaudu

> [AZURE.SELECTOR]
- [Portaal](media-services-portal-creating-live-encoder-enabled-channel.md)
- [.NET-I](media-services-dotnet-creating-live-encoder-enabled-channel.md)
- [REST API-GA](https://msdn.microsoft.com/library/azure/dn783458.aspx)

>[AZURE.NOTE]
> Selle õpetuse tegemiseks peate Azure'i konto. Lisateavet leiate teemast [Azure tasuta prooviversioon](/pricing/free-trial/?WT.mc_id=A261C142F).

##<a name="overview"></a>Ülevaade

Selles õpetuses juhendab teid **kanali** , mida saab ühe bitikiirusega reaalajas voo ja kodeeritakse see mitme bitikiirusega voo loomise juhiseid.

Kontseptuaalne seotud kanalid, mis on lubatud reaalajas kodeerimine, Lisateavet [Live streaming loomine mitme bitikiirusega voogu Azure Media Servicesi kaudu](media-services-manage-live-encoder-enabled-channels.md).


##<a name="common-live-streaming-scenario"></a>Reaalajas Streaming stsenaarium

Järgmised sammud kirjeldavad seotud levinud reaalajas streaming rakenduse loomiseks.

>[AZURE.NOTE] Praegu on soovitatav max live sündmus kestab 8 tundi. Kui peate käivitama kanali jaoks pikemaks, võtke ühendust amslived veebisaidil Microsoft.com.

1. Videokaamera arvutiga. Käivitage ja konfigureerige kohapealse reaalajas kodeerija, mida saate väljund ühe bitrate voo ühel järgmistest protokollid: RTMP, sujuva voogesituse või RTP (MPEG-TS). Lisateabe saamiseks vt [Azure Media Services RTMP tugi ja Live kodeerimisseadmetest](http://go.microsoft.com/fwlink/?LinkId=532824).

Selles etapis tuleb võivad täita ka pärast oma kanali loomiseks.

1. Loomine ja käivitamine kanali.

1. Too kanali neelata URL-i.

Neelata URL-i kasutatakse reaalajas kodeerija voo saatmiseks kanal.

1. Saate tuua eelvaade kanali URL.

Seda URL-i abil veenduda, et teie kanal on õigesti saab reaalajas voo.

2. Saate luua vara.
3. Kui soovite varade esituse ajal dünaamiliselt krüptimist, tehke järgmist.
1. Saate luua sisu klahvi.
1. Konfigureerimine sisu klahvi autoriseerimine poliitika.
1. Konfigureerida varade kohaletoimetamise poliitika (kasutatakse dünaamilise pakendit ja dünaamiline krüptimise).
3. Programmi loomine ja määrake loodud vara kasutama.
1. Vara seostatud programm on nõudmisel locator loomisega avaldada.

Veenduge, et on vähemalt üks streaming reserveeritud üksus, millest soovite voogesituseks streaming lõpp-punkti.

1. Kui olete valmis alustama streaming ja arhiivimine, käivitage programm.
2. Soovi korral saate reaalajas kodeerija märku, reklaami käivitamiseks. Reklaami lisatakse väljundi voo.
1. Iga kord, kui soovite peatada streaming ja arhiivimine sündmus, sulgege see programm.
1. Kustutage programm (ja kustutada vara).

## <a name="what-youll-learn"></a>Käsitletavad

Selles teemas näidatakse, kuidas käivitada erinevaid toiminguid kanalid ja programmide Media Services .NET SDK abil. Kuna paljud toimingud on pikaajaline kasutatakse .net-i API-d pikaajalise toimingute tegemiseks.

Teema näitab, kuidas teha järgmist:

1. Loomine ja käivitamine kanali. Pikaajalisi API-d kasutatakse.
1. Saada kanalitega neelata (Sisestuskeel) lõpp-punkti. Kodeerija, mille saate saata ühe bitrate reaalajas voo tuleb see lõpp-punkti.
1. Saada eelvaade lõpp-punkti. Selle lõpp-punkti kasutatakse teie voo eelvaate.
1. Saate luua varade, mida kasutatakse teie sisu talletamiseks. Varade kohaletoimetamise poliitikate konfigureeritakse samuti, nagu on näidatud järgmises näites.
1. Programmi luua ja kasutada mõnes varasemas versioonis loodud varade määrata. Käivitage programm. Pikaajalisi API-d kasutatakse.
1. Looge locator, vara, et sisu saab avaldatud ja saate voona oma klientidele.
1. Kuvamine ja peitmine tahvlid. Käivitamine ja peatamine reklaamid. Pikaajalisi API-d kasutatakse.
1. Puhasta kanali ja seotud ressursid.


##<a name="prerequisites"></a>Eeltingimused

Õpetuse lõpule viimiseks on nõutav.

- Selle õpetuse tegemiseks peate Azure'i konto.

Kui teil pole kontot, saate luua tasuta prooviversiooni konto vaid paar minutit. Lisateavet leiate teemast [Azure tasuta prooviversioon](/pricing/free-trial/?WT.mc_id=A261C142F). Saate proovida makstud Azure'i teenuste kasutatavate krediidi summa liitmisel. Isegi juhul, kui tegu on kasutanud, saate konto hoida ja tasuta Azure teenused ja funktsioonid, näiteks veebirakenduste funktsioon teenuses Azure rakenduse kasutamine.
- Media Servicesi konto. Media Servicesi konto loomiseks vaadake teemat [Loo konto](media-services-portal-create-account.md).
- Visual Studio 2010 SP1 (Professional, Premium, Ultimate või Express) või uuemad versioonid.
- Kasutage Media Services .NET SDK versioon 3.2.0.0 või uuemat versiooni.
- Kodeerija, mille saate saata ühe bitrate ja veebikaamera live voo.

##<a name="considerations"></a>Kaalutlused

- Praegu on soovitatav max live sündmus kestab 8 tundi. Kui peate käivitama kanali jaoks pikemaks, võtke ühendust amslived veebisaidil Microsoft.com.
- Veenduge, et on vähemalt üks streaming reserveeritud üksus, millest soovite voogesituseks streaming lõpp-punkti.

##<a name="download-sample"></a>Laadige alla näidis

Saada ja käivitage valimi [siin](https://azure.microsoft.com/documentation/samples/media-services-dotnet-encode-live-stream-with-ams-clear/).


##<a name="set-up-for-development-with-media-services-sdk-for-net"></a>Arengu Media Services SDK .net-i häälestamine

1. Konsooli rakendus Visual Studio abil luua.
1. Lisage Media Services SDK .net-i konsooli rakendus Media Services Nugeti pakett.

##<a name="connect-to-media-services"></a>Ühenduse loomine Media Services
Hea tava, kasutage mõnda app.config faili salvestamiseks Media Servicesi nimi ja konto võti.

>[AZURE.NOTE]Nime ja klahvi asuvate väärtuste otsimine, Azure'i portaalis ja valige oma konto. Sätete aken paremal. Valige aknas sätted võtmed. Iga tekstivälja kõrval olevat ikooni klõpsamisel kopeerib väärtuse süsteemi lõikelauale.

Lisada appSettings jaotise app.config faili ja teie Media Servicesi konto nimi ja konto võti väärtused.


    <?xml version="1.0"?>
    <configuration>
      <appSettings>
          <add key="MediaServicesAccountName" value="YouMediaServicesAccountName" />
          <add key="MediaServicesAccountKey" value="YouMediaServicesAccountKey" />
      </appSettings>
    </configuration>
     
    
##<a name="code-example"></a>Koodi näide

    using System;
    using System.Collections.Generic;
    using System.Configuration;
    using System.IO;
    using System.Linq;
    using System.Net;
    using Microsoft.WindowsAzure.MediaServices.Client;
    using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;
    
    namespace EncodeLiveStreamWithAmsClear
    {
        class Program
        {
            private const string ChannelName = "channel001";
            private const string AssetlName = "asset001";
            private const string ProgramlName = "program001";
    
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
    
                IChannel channel = CreateAndStartChannel();
    
                // The channel's input endpoint:
                string ingestUrl = channel.Input.Endpoints.FirstOrDefault().Url.ToString();
    
                Console.WriteLine("Intest URL: {0}", ingestUrl);
    
    
                // Use the previewEndpoint to preview and verify 
                // that the input from the encoder is actually reaching the Channel. 
                string previewEndpoint = channel.Preview.Endpoints.FirstOrDefault().Url.ToString();
    
                Console.WriteLine("Preview URL: {0}", previewEndpoint);
    
                // When Live Encoding is enabled, you can now get a preview of the live feed as it reaches the Channel. 
                // This can be a valuable tool to check whether your live feed is actually reaching the Channel. 
                // The thumbnail is exposed via the same end-point as the Channel Preview URL.
                string thumbnailUri = new UriBuilder
                {
                    Scheme = Uri.UriSchemeHttps,
                    Host = channel.Preview.Endpoints.FirstOrDefault().Url.Host,
                    Path = "thumbnails/input.jpg"
                }.Uri.ToString();
    
                Console.WriteLine("Thumbain URL: {0}", thumbnailUri);
    
                // Once you previewed your stream and verified that it is flowing into your Channel, 
                // you can create an event by creating an Asset, Program, and Streaming Locator. 
                IAsset asset = CreateAndConfigureAsset();
    
                IProgram program = CreateAndStartProgram(channel, asset);
    
                ILocator locator = CreateLocatorForAsset(program.Asset, program.ArchiveWindowLength);
    
                // You can use slates and ads only if the channel type is Standard.  
                StartStopAdsSlates(channel);
    
                // Once you are done streaming, clean up your resources.
                Cleanup(channel);
    
            }
    
            public static IChannel CreateAndStartChannel()
            {
                var channelInput = CreateChannelInput();
                var channePreview = CreateChannelPreview();
                var channelEncoding = CreateChannelEncoding();
    
    
                ChannelCreationOptions options = new ChannelCreationOptions
                {
                    EncodingType = ChannelEncodingType.Standard,
                    Name = ChannelName,
                    Input = channelInput,
                    Preview = channePreview,
                    Encoding = channelEncoding
                };
    
                Log("Creating channel");
                IOperation channelCreateOperation = _context.Channels.SendCreateOperation(options);
                string channelId = TrackOperation(channelCreateOperation, "Channel create");
    
                IChannel channel = _context.Channels.Where(c => c.Id == channelId).FirstOrDefault();
    
                Log("Starting channel");
                var channelStartOperation = channel.SendStartOperation();
                TrackOperation(channelStartOperation, "Channel start");
    
                return channel;
            }
    
            /// <summary>
            /// Create channel input, used in channel creation options. 
            /// </summary>
            /// <returns></returns>
            private static ChannelInput CreateChannelInput()
            {
                return new ChannelInput
                {
                    StreamingProtocol = StreamingProtocol.RTPMPEG2TS,
                    AccessControl = new ChannelAccessControl
                    {
                        IPAllowList = new List<IPRange>
                        {
                            new IPRange
                            {
                                Name = "TestChannelInput001",
                                Address = IPAddress.Parse("0.0.0.0"),
                                SubnetPrefixLength = 0
                            }
                        }
                    }
                };
            }
    
            /// <summary>
            /// Create channel preview, used in channel creation options. 
            /// </summary>
            /// <returns></returns>
            private static ChannelPreview CreateChannelPreview()
            {
                return new ChannelPreview
                {
                    AccessControl = new ChannelAccessControl
                    {
                        IPAllowList = new List<IPRange>
                        {
                            new IPRange
                            {
                                Name = "TestChannelPreview001",
                                Address = IPAddress.Parse("0.0.0.0"),
                                SubnetPrefixLength = 0
                            }
                        }
                    }
                };
            }
    
            /// <summary>
            /// Create channel encoding, used in channel creation options. 
            /// </summary>
            /// <returns></returns>
            private static ChannelEncoding CreateChannelEncoding()
            {
                return new ChannelEncoding
                {
                    SystemPreset = "Default720p",
                    IgnoreCea708ClosedCaptions = false,
                    AdMarkerSource = AdMarkerSource.Api,
                    // You can only set audio if streaming protocol is set to StreamingProtocol.RTPMPEG2TS.
                    AudioStreams = new List<AudioStream> { new AudioStream { Index = 103, Language = "eng" } }.AsReadOnly()
                };
            }
    
            /// <summary>
            /// Create an asset and configure asset delivery policies.
            /// </summary>
            /// <returns></returns>
            public static IAsset CreateAndConfigureAsset()
            {
                IAsset asset = _context.Assets.Create(AssetlName, AssetCreationOptions.None);
    
                IAssetDeliveryPolicy policy =
                    _context.AssetDeliveryPolicies.Create("Clear Policy",
                    AssetDeliveryPolicyType.NoDynamicEncryption,
                    AssetDeliveryProtocol.HLS | AssetDeliveryProtocol.SmoothStreaming | AssetDeliveryProtocol.Dash, null);
    
                asset.DeliveryPolicies.Add(policy);
    
                return asset;
            }
    
            /// <summary>
            /// Create a Program on the Channel. You can have multiple Programs that overlap or are sequential;
            /// however each Program must have a unique name within your Media Services account.
            /// </summary>
            /// <param name="channel"></param>
            /// <param name="asset"></param>
            /// <returns></returns>
            public static IProgram CreateAndStartProgram(IChannel channel, IAsset asset)
            {
                IProgram program = channel.Programs.Create(ProgramlName, TimeSpan.FromHours(3), asset.Id);
                Log("Program created", program.Id);
    
                Log("Starting program");
                var programStartOperation = program.SendStartOperation();
                TrackOperation(programStartOperation, "Program start");
    
                return program;
            }
    
            /// <summary>
            /// Create locators in order to be able to publish and stream the video.
            /// </summary>
            /// <param name="asset"></param>
            /// <param name="ArchiveWindowLength"></param>
            /// <returns></returns>
            public static ILocator CreateLocatorForAsset(IAsset asset, TimeSpan ArchiveWindowLength)
            {
                // You cannot create a streaming locator using an AccessPolicy that includes write or delete permissions.            
                var locator = _context.Locators.CreateLocator
                    (
                        LocatorType.OnDemandOrigin,
                        asset,
                        _context.AccessPolicies.Create
                            (
                                "Live Stream Policy",
                                ArchiveWindowLength,
                                AccessPermissions.Read
                            )
                    );
    
                return locator;
            }
    
            /// <summary>
            /// Perform operations on slates.
            /// </summary>
            /// <param name="channel"></param>
            public static void StartStopAdsSlates(IChannel channel)
            {
                int cueId = new Random().Next(int.MaxValue);
                var path = Path.GetFullPath(Path.Combine(AppDomain.CurrentDomain.BaseDirectory, @"..\\..\\SlateJPG\\DefaultAzurePortalSlate.jpg"));
    
                Log("Creating asset");
                var slateAsset = _context.Assets.Create("Slate test asset " + DateTime.Now.ToString("yyyy-MM-dd HH-mm"), AssetCreationOptions.None);
                Log("Slate asset created", slateAsset.Id);
    
                Log("Uploading file");
                var assetFile = slateAsset.AssetFiles.Create("DefaultAzurePortalSlate.jpg");
                assetFile.Upload(path);
                assetFile.IsPrimary = true;
                assetFile.Update();
    
                Log("Showing slate");
                var showSlateOpeartion = channel.SendShowSlateOperation(TimeSpan.FromMinutes(1), slateAsset.Id);
                TrackOperation(showSlateOpeartion, "Show slate");
    
                Log("Hiding slate");
                var hideSlateOperation = channel.SendHideSlateOperation();
                TrackOperation(hideSlateOperation, "Hide slate");
    
                Log("Starting ad");
                var startAdOperation = channel.SendStartAdvertisementOperation(TimeSpan.FromMinutes(1), cueId, false);
                TrackOperation(startAdOperation, "Start ad");
    
                Log("Ending ad");
                var endAdOperation = channel.SendEndAdvertisementOperation(cueId);
                TrackOperation(endAdOperation, "End ad");
    
                Log("Deleting slate asset");
                slateAsset.Delete();
            }
    
            /// <summary>
            /// Clean up resources associated with the channel.
            /// </summary>
            /// <param name="channel"></param>
            public static void Cleanup(IChannel channel)
            {
                IAsset asset;
                if (channel != null)
                {
                    foreach (var program in channel.Programs)
                    {
                        asset = _context.Assets.Where(se => se.Id == program.AssetId)
                                                .FirstOrDefault();
    
                        Log("Stopping program");
                        var programStopOperation = program.SendStopOperation();
                        TrackOperation(programStopOperation, "Program stop");
    
                        program.Delete();
    
                        if (asset != null)
                        {
                            Log("Deleting locators");
                            foreach (var l in asset.Locators)
                                l.Delete();
    
                            Log("Deleting asset");
                            asset.Delete();
                        }
                    }
    
                    Log("Stopping channel");
                    var channelStopOperation = channel.SendStopOperation();
                    TrackOperation(channelStopOperation, "Channel stop");
    
                    Log("Deleting channel");
                    var channelDeleteOperation = channel.SendDeleteOperation();
                    TrackOperation(channelDeleteOperation, "Channel delete");
                }
            }
    
    
            /// <summary>
            /// Track long running operations.
            /// </summary>
            /// <param name="operation"></param>
            /// <param name="description"></param>
            /// <returns></returns>
            public static string TrackOperation(IOperation operation, string description)
            {
                string entityId = null;
                bool isCompleted = false;
    
                Log("starting to track ", null, operation.Id);
                while (isCompleted == false)
                {
                    operation = _context.Operations.GetOperation(operation.Id);
                    isCompleted = IsCompleted(operation, out entityId);
                    System.Threading.Thread.Sleep(TimeSpan.FromSeconds(30));
                }
                // If we got here, the operation succeeded.
                Log(description + " in completed", operation.TargetEntityId, operation.Id);
    
                return entityId;
            }
    
            /// <summary> 
            /// Checks if the operation has been completed. 
            /// If the operation succeeded, the created entity Id is returned in the out parameter.
            /// </summary> 
            /// <param name="operationId">The operation Id.</param> 
            /// <param name="channel">
            /// If the operation succeeded, 
            /// the entity Id associated with the sucessful operation is returned in the out parameter.</param>
            /// <returns>Returns false if the operation is still in progress; otherwise, true.</returns> 
            private static bool IsCompleted(IOperation operation, out string entityId)
            {
    
                bool completed = false;
    
                entityId = null;
    
                switch (operation.State)
                {
                    case OperationState.Failed:
                        // Handle the failure. 
                        // For example, throw an exception. 
                        // Use the following information in the exception: operationId, operation.ErrorMessage.
                        Log("operation failed", operation.TargetEntityId, operation.Id);
                        break;
                    case OperationState.Succeeded:
                        completed = true;
                        entityId = operation.TargetEntityId;
                        break;
                    case OperationState.InProgress:
                        completed = false;
                        Log("operation in progress", operation.TargetEntityId, operation.Id);
                        break;
                }
                return completed;
            }
    
    
            private static void Log(string action, string entityId = null, string operationId = null)
            {
                Console.WriteLine(
                    "{0,-21}{1,-51}{2,-51}{3,-51}",
                    DateTime.Now.ToString("yyyy'-'MM'-'dd HH':'mm':'ss"),
                    action,
                    entityId ?? string.Empty,
                    operationId ?? string.Empty);
            }
        }
    }   


##<a name="next-step"></a>Järgmise juhise juurde

Vaadake üle Media Servicesi õppeteemad.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Tagasiside saatmine

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

### <a name="looking-for-something-else"></a>Kas otsite midagi muud?

Kui selle teema ei sisalda, mida te ei oodanud, pole midagi või mõnel muul viisil ei vasta teie vajadustele, esitage meile teiega abil Disqus teemas allpool tagasisidet.
