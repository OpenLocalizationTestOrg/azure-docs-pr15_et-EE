<properties 
    pageTitle="Vara kodeerida koos Media kodeerija Standard .net-i abil | Microsoft Azure'i" 
    description="Selles teemas kirjeldatakse, kuidas kasutada .net-i abil Media kodeerija Strandard vara kodeerida." 
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
    ms.date="09/19/2016"
    ms.author="juliako;anilmur"/>


# <a name="encode-an-asset-with-media-encoder-standard-using-net"></a>Vara kodeerida koos Media kodeerija Standard .net-i abil

Kodeering tööde haldamine on üks kõige levinum töötlemise Media Services. Saate luua kodeering töökohta kodeeringus teise meediumifailide teisendada. Kui te kodeerida, saate kasutada Media Servicesi sisseehitatud Media Encoderi. Saate kasutada ka esitatud partnerilt Media Services kodeerija kolmanda osapoole kodeerimisseadmetest on saadaval Azure'i turuplatsi kaudu. 

Selles teemas kirjeldatakse, kuidas kasutada .NET kodeerida oma varasid koos Media kodeerija Standardne (MES). Media kodeerija Standard on konfigureeritud, kasutades ühte kodeerija valmiskombinatsioonid kirjeldatud [allpool](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).

Soovitatav on alati kodeerida Standard failide on kohandatava bitrate MP4 seadmine ja seejärel teisendage määramine [Dünaamilise pakendit](media-services-dynamic-packaging-overview.md)abil soovitud vorming. Dünaamiliste pakendit ära, peate esmalt saada vähemalt üks tellitav streaming ühiku streaming lõpp-punkti, millest plaanite kohaletoimetamise sisu. Lisateavet leiate teemast [skaala Media Services](media-services-portal-manage-streaming-endpoints.md).

Kui teie väljundi vara on krüptitud salvestusruumi, tuleb konfigureerida varade kohaletoimetamise poliitika. Lisateabe saamiseks vt [seadistamine varade kohaletoimetamise poliitika](media-services-dotnet-configure-asset-delivery-policy.md).

>[AZURE.NOTE]MES toodab väljund faili nime, mis sisaldab esimene 32 märke Sisestuskeel faili nimi. Mis on loodud failis määratud põhineb nimi. Näiteks "failinimi": "{Basename} _ {Index} {laiend}". {Basename} asendatakse esimene 32 märkide Sisestuskeel faili nimi.

###<a name="mes-formats"></a>MES vormingud

[Vormingute ja kodekite](media-services-media-encoder-standard-formats.md)

###<a name="mes-presets"></a>MES valmiskombinatsioonid

Media kodeerija Standard on konfigureeritud, kasutades ühte kodeerija valmiskombinatsioonid kirjeldatud [allpool](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).

###<a name="input-and-output-metadata"></a>Sisend- ja metaandmed

Kui te kodeerida abil MES Sisestuskeel vara (või varad), saate väljundi vara eduka lõpetamise, et kodeerida tööülesande. Väljundi vara sisaldab video, heli, pisipildid, manifesti jne alusel kodeering valmissäte, mida kasutate.

Varade väljund sisaldab ka fail, mille metaandmete Sisestuskeel varade kohta. Metaandmete XML-faili nimi on järgmises vormingus: < asset_id > _metadata.xml (nt 41114ad3-eb5e - 4c 57-8d 92-5354e2b7d4a4_metadata.xml), kus on < asset_id > Sisestuskeel vara AssetId väärtust. See Sisestuskeel metaandmete XML-i skeemi on kirjeldatud [allpool](http://msdn.microsoft.com/library/azure/dn783120.aspx).

Varade väljund sisaldab ka fail, mille metaandmete väljundi varade kohta. Metaandmete XML-faili nimi on järgmises vormingus: < source_file_name > _manifest.xml (nt BigBuckBunny_manifest.xml). Skeemi selle väljundi metaandmete XML-i on kirjeldatud [allpool](http://msdn.microsoft.com/library/azure/dn783217.aspx).

Kui soovite kontrollida, kas kaks metaandmete faili, saate luua SAS locator ja faili oma arvutisse alla laadida. Ühe näite leiate SAS locator loomise ja kasutamise Media Services .NET SDK laiendid faili alla laadida.

##<a name="download-sample"></a>Laadige alla näidis

Saada ja käivitage valimi [siin](https://azure.microsoft.com/documentation/samples/media-services-dotnet-on-demand-encoding-with-media-encoder-standard/).

##<a name="example"></a>Näide

Järgmine kood näide kasutab Media Services .NET SDK teha järgmisi toiminguid:

- Luua mõne kodeering töö.
- Saada Media kodeerija Standard kodeerija viide.
- Määrake kasutada funktsiooni "H264 mitme Bitrate 720p" eelseadistatud. Saate vaadata kõiki valmiskombinatsioonid [siin](http://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409). Samuti saate uurida skeem, mis peab vastama valmissätteid [siin](https://msdn.microsoft.com/library/mt269962.aspx) teemas.
- Lisada ühe tööülesande kodeering töö. 
- Määrake Sisestuskeel varade olema kodeeritud.
- Looge encoded varade sisaldavate väljundi vara.
- Lisage sündmuseohjuri töö edenemise kontrollimiseks.
- Esitage töö.
        
        static public IAsset EncodeToAdaptiveBitrateMP4Set(IAsset asset)
        {
            // Declare a new job.
            IJob job = _context.Jobs.Create("Media Encoder Standard Job");
            // Get a media processor reference, and pass to it the name of the 
            // processor to use for the specific task.
            IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Standard");
        

            // Create a task with the encoding details, using a string preset.
            // In this case "H264 Multiple Bitrate 720p" preset is used.
            ITask task = job.Tasks.AddNew("My encoding task",
                processor,
                "H264 Multiple Bitrate 720p",
                TaskOptions.None);
        
            // Specify the input asset to be encoded.
            task.InputAssets.Add(asset);
            // Add an output asset to contain the results of the job. 
            // This output is specified as AssetCreationOptions.None, which 
            // means the output asset is not encrypted. 
            task.OutputAssets.AddNew("Output asset",
                AssetCreationOptions.None);
        
            job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
            job.Submit();
            job.GetExecutionProgressTask(CancellationToken.None).Wait();
        
            return job.OutputMediaAssets[0];
        }
        
        private static void JobStateChanged(object sender, JobStateChangedEventArgs e)
        {
            Console.WriteLine("Job state changed event:");
            Console.WriteLine("  Previous state: " + e.PreviousState);
            Console.WriteLine("  Current state: " + e.CurrentState);
            switch (e.CurrentState)
            {
                case JobState.Finished:
                    Console.WriteLine();
                    Console.WriteLine("Job is finished. Please wait while local tasks or downloads complete...");
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
                    break;
                default:
                    break;
            }
        }
        
        
        private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
        {
            var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
            ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();
        
            if (processor == null)
                throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));
        
            return processor;
        }


##<a name="media-services-learning-paths"></a>Media Servicesi Õppeteed

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Tagasiside saatmine

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="see-also"></a>Vt ka 

[Kuidas luua pisipilti, kasutades .NET Media kodeerija Standard](media-services-dotnet-generate-thumbnail-with-mes.md)
[Media Services kodeeringus ülevaade](media-services-encode-asset.md)
