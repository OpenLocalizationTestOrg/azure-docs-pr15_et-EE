<properties 
    pageTitle="Toiminguid küsitlused | Microsoft Azure'i" 
    description="See teema kirjeldab toiminguid küsitlus." 
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


#<a name="delivering-live-streaming-with-azure-media-services"></a>Pakkuda reaalajas voogesitus Azure Media Servicesi

##<a name="overview"></a>Ülevaade

Microsoft Azure Media Servicesi pakub päringuid saata Media Servicesi tegevuse alustamiseks API-d (nt: loomiseks, käivitamiseks, peatamiseks või kanali kustutamine). Need toimingud on pikaajaline.

Media Services .NET SDK annab API-d, mille kutse saatmiseks ja oodake, kuni toiming lõpule jõuab (ettevõttesiseselt, on API-d on küsitlused tegevuse edenemine jaoks teatud intervalliga). Näiteks helistamisel kanal. Start(), meetod tagastab pärast kanali käivitatakse. Võite kasutada ka asünkroonne versioon: ootavad kanal. StartAsync() (toimingupõhise asünkroonne mustri kohta leiate teavet teemast [koputage](https://msdn.microsoft.com/library/hh873175(v=vs.110).aspx)). API-d saata taotluse toiming ja seejärel küsitlus oleku enne selle toimingu lõpuleviimist nimetatakse "küsitlused meetodid". Järgmised võimalused (eriti asünkroonse versioon) on soovitatav rikkaliku klientrakendustes ja/või stateful teenused.

On stsenaariumid, kus rakendus ei saa oodata kaua töötab http-päring ja soovib küsitlus tegevuse edenemine käsitsi. Tüüpiline näide oleks suheldes kodakondsuseta veebiteenuse brauseris: kui brauseri kanali loomiseks veebiteenuse käivitab toiming ja toimingu ID-d naaseb brauseris. Brauseri, siis võib Küsi veebiteenuse saada toiming oleku põhjal on ID. Media Services .NET SDK pakub API-d, mis on kasulik seda stsenaariumi. Nende API-de nimetatakse "-küsitlused meetodid".
"-Küsitlused meetodid" on järgmine nimede muster: saatmine*OperationName*toiming (nt SendCreateOperation). Saada*OperationName*toiming meetodid tagastavad **IOperation** objekti; tagastatud objekt sisaldab teavet, mida saate kasutada toimingu jälgimiseks. Saada*OperationName*OperationAsync meetodid tagastavad **tööülesande<IOperation>**.

Järgmiste toetus-küsitlused võimalust: **kanali**, **StreamingEndpoint**ja **programmi**.

Küsitlus toimingu olek, kasutage **OperationBaseCollection** klassi **GetOperation** meetodit. Järgmised intervallide kasutamine oleku toiming: **kanal** ja **StreamingEndpoint** toimingute puhul kasutada 30 sekundit; **programmi** toimingute puhul kasutada 10 sekundit.


##<a name="example"></a>Näide

Järgmises näites määratleb klassi nimetatakse **ChannelOperations**. Selle ainekursuse määratlus võib olla alguspunkti definitsiooni web teenuse tunni jaoks. Järgmistes näidetes kasutatakse lihtsa,-asünkroonse versioonid meetodid.

Käesolevas näites ka näitab, kuidas klient võib see tund.

###<a name="channeloperations-class-definition"></a>ChannelOperations klassi määratlus

    /// <summary> 
    /// The ChannelOperations class only implements 
    /// the Channel’s creation operation. 
    /// </summary> 
    public class ChannelOperations
    {
        // Read values from the App.config file.
        private static readonly string _mediaServicesAccountName =
            ConfigurationManager.AppSettings["MediaServicesAccountName"];
        private static readonly string _mediaServicesAccountKey =
            ConfigurationManager.AppSettings["MediaServicesAccountKey"];
    
        // Field for service context.
        private static CloudMediaContext _context = null;
        private static MediaServicesCredentials _cachedCredentials = null;
    
        public ChannelOperations()
        {
                _cachedCredentials = new MediaServicesCredentials(_mediaServicesAccountName,
                    _mediaServicesAccountKey);
    
                _context = new CloudMediaContext(_cachedCredentials);    }
    
        /// <summary>  
        /// Initiates the creation of a new channel.  
        /// </summary>  
        /// <param name="channelName">Name to be given to the new channel</param>  
        /// <returns>  
        /// Operation Id for the long running operation being executed by Media Services. 
        /// Use this operation Id to poll for the channel creation status. 
        /// </returns> 
        public string StartChannelCreation(string channelName)
        {
            var operation = _context.Channels.SendCreateOperation(
                new ChannelCreationOptions
                {
                    Name = channelName,
                    Input = CreateChannelInput(),
                    Preview = CreateChannelPreview(),
                    Output = CreateChannelOutput()
                });
    
            return operation.Id;
        }
    
        /// <summary> 
        /// Checks if the operation has been completed. 
        /// If the operation succeeded, the created channel Id is returned in the out parameter.
        /// </summary> 
        /// <param name="operationId">The operation Id.</param> 
        /// <param name="channel">
        /// If the operation succeeded, 
        /// the created channel Id is returned in the out parameter.</param>
        /// <returns>Returns false if the operation is still in progress; otherwise, true.</returns> 
        public bool IsCompleted(string operationId, out string channelId)
        {
            IOperation operation = _context.Operations.GetOperation(operationId);
            bool completed = false;
    
            channelId = null;
    
            switch (operation.State)
            {
                case OperationState.Failed:
                    // Handle the failure. 
                    // For example, throw an exception. 
                    // Use the following information in the exception: operationId, operation.ErrorMessage.
                    break;
                case OperationState.Succeeded:
                    completed = true;
                    channelId = operation.TargetEntityId;
                    break;
                case OperationState.InProgress:
                    completed = false;
                    break;
            }
            return completed;
        }
    
    
        private static ChannelInput CreateChannelInput()
        {
            return new ChannelInput
            {
                StreamingProtocol = StreamingProtocol.RTMP,
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
    
        private static ChannelOutput CreateChannelOutput()
        {
            return new ChannelOutput
            {
                Hls = new ChannelOutputHls { FragmentsPerSegment = 1 }
            };
        }
    }

###<a name="the-client-code"></a>Kliendi kood

    ChannelOperations channelOperations = new ChannelOperations();
    string opId = channelOperations.StartChannelCreation("MyChannel001");
    
    string channelId = null;
    bool isCompleted = false;
    
    while (isCompleted == false)
    {
        System.Threading.Thread.Sleep(TimeSpan.FromSeconds(30));
        isCompleted = channelOperations.IsCompleted(opId, out channelId);
    }
    
    // If we got here, we should have the newly created channel id.
    Console.WriteLine(channelId);
 


##<a name="media-services-learning-paths"></a>Media Servicesi Õppeteed

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Tagasiside saatmine

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
