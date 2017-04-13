<properties
    pageTitle="Liikuv stoppkaader Media failide jagamine Azure Media Liikuv stoppkaader | Microsoft Azure'i"
    description="Azure Media Liikuv stoppkaader loob sujuv aja aegunud videod esimese isiku või toimingu-kaamera sisu. Selles teemas kirjeldatakse, kuidas kasutada Media indekseerija."
    services="media-services"
    documentationCenter=""
    authors="asolanki"
    manager="johndeu"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/19/2016"  
    ms.author="adsolank"/>


# <a name="hyperlapse-media-files-with-azure-media-hyperlapse"></a>Liikuv stoppkaader Media failide jagamine Azure Media Liikuv stoppkaader

Azure Media Liikuv stoppkaader on meediumi protsessor (MP) loodud sujuv aja aegunud videod esimese isiku või toimingu-kaamera sisu.  Pilvepõhise vend [Microsoft Researchi töölaua Liikuv stoppkaader Pro](http://aka.ms/hyperlapse)ja telefoni teel Liikuv stoppkaader Mobile, Microsoft Hyperlapse Azure Media Servicesi kasutab suuri skaala Azure Media Services meediumi töötlemine horisontaalselt skaala ja parallelize platvormi hulgiredigeerimiseks Liikuv stoppkaader töötlemine.

>[AZURE.IMPORTANT]Microsoft Hyperlapse eesmärk on kõige paremini esimese isiku sisu jooksva kaamera.  Kuigi saate endiselt töötavad ikka-kaamera, jõudlus ja Azure Media Liikuv stoppkaader meediumi protsessor kvaliteeti ei saa tagada muud tüüpi sisu.  Lisateave Microsoft Hyperlapse Azure Media Servicesi ja leiate mõned videod näide, vaadake selle [sissejuhatav ajaveebipostituse](http://aka.ms/azurehyperlapseblog) avaliku eelvaade.

Azure Media Hyperlapse töö võtab sisestusmeetodi MP4, MOV ja WMV varade failina koos konfiguratsioonifail, mis määrab, millised raamid video peaks olema aega aegunud ja mida kiirus (nt esimese 10 000 raamid 2 x).  Väljund on stabiliseeritud ja kellaaja aegunud üleviimise video sisendit.

Uusimate värskenduste Azure Media Liikuv stoppkaader, leiate teemast [Media Services ajaveebide](https://azure.microsoft.com/blog/topics/media-services/).

## <a name="hyperlapse-an-asset"></a>Liikuv stoppkaader vara

Esmalt peate Azure Media Servicesi Sisestuskeel soovitud faili üles laadida.  Seotud üleslaadimine ja sisu haldamine põhimõtet kohta lisateabe saamiseks lugege [sisuhaldus artikkel](media-services-portal-vod-get-started.md).

###  <a id="configuration"></a>Liikuv stoppkaader eelseadistatud konfiguratsiooni

Kui teie sisu on teie konto Media Servicesi, peate ehitada oma konfiguratsioon valmissäte.  Järgmises tabelis selgitatakse kasutaja määratletud väljad.

 Väli | Kirjeldus
-------|-------------
StartFrame|Raami, millele Microsoft Hyperlapse töötlemist peaks algama.
NumFrames|Arvu paneelide töötlemiseks.
Kiiruse|Tegur, millega kiirendamiseks Sisestuskeel video.

Järgmises näites olemasolu konfiguratsioonifail XML-i ja JSON:

**XML-i valmissäte:**

    <?xml version="1.0" encoding="utf-16"?>
    <Preset xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">
        <Sources>
            <Source StartFrame="0" NumFrames="10000" />
        </Sources>
        <Options>
            <Speed>12</Speed>
        </Options>
    </Preset>

**JSON valmissäte:**

    {
        "Version":1.0,
        "Sources": [
            {
                "StartFrame":0,
                "NumFrames":2147483647
            }
        ],
        "Options": {
            "Speed":1,
            "Stabilize":false
        }
    }

###  <a id="sample_code"></a>Microsoft Liikuv stoppkaader AMS .NET SDK

Järgmisel viisil lisatud media fail aktiva ja loob töö Azure Media Liikuv stoppkaader meediumi protsessor.

> [AZURE.NOTE] Teil peaks juba mõne CloudMediaContext ulatus nimi "kontekstist" selle koodi töötamiseks.  Lugege lisateavet selle kohta, [sisuhaldus artikkel](media-services-dotnet-get-started.md).

> [AZURE.NOTE] Argument stringi "hyperConfig" on tõenäoliselt olemasolu konfiguratsiooni eelmääratud JSON-või XML-i eespool kirjeldatud.

staatilise bool RunHyperlapseJob (stringi sisendit, stringi väljundi, stringi hyperConfig) {/ / luua varade Sisestuskeel faili IAsset varade = raames. Varad. CreateAssetAndUploadSingleFile (input, "Minu Liikuv stoppkaader sisend", AssetCreationOptions.None);

Ostke eksemplarid Azure Media Liikuv stoppkaader MP IMediaProcessor liige = raames. MediaProcessors. GetLatestMediaProcessorByName ("Azure Media Hyperlapse");

Töö loomine Liikuv stoppkaader tööülesande IJob töö = raames. Tööd. Looge (String.Format (input "Hyperlapse {0}"));

Kui (String.IsNullOrEmpty(hyperConfig)) {/ / config ei saa olla tühi tagasi false;}

hyperConfig = File.ReadAllText(hyperConfig);

ITask hyperlapseTask = töö. Tasks.AddNew ("Liikuv stoppkaader ülesanne", mp, hyperConfig, TaskOptions.None); hyperlapseTask.InputAssets.Add(asset); hyperlapseTask.OutputAssets.AddNew ("Liikuv stoppkaader väljund", AssetCreationOptions.None);


töö. Submit();

Edenemise printimine ja tööülesanded tööülesande progressPrintTask päringute loomine = uus Task(() = > {}

IJob jobQuery = null; Tehke {var progressContext = kontekstis; jobQuery = progressContext.Jobs. Kus (j = > j.Id == töö. ID). First(); Console.WriteLine (stringi. Vorming ("\t {1} \t {0} {2}", DateTime.Now, jobQuery.State, jobQuery.Tasks[0]. Edenemise)); Thread.Sleep(10000); } ajal (jobQuery.State! = JobState.Finished & & jobQuery.State! = JobState.Error & & jobQuery.State! = JobState.Canceled); }); progressPrintTask.Start();

            Task progressJobTask = job.GetExecutionProgressTask(
                                                 CancellationToken.None);
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
                return false;                  
            }

        DownloadAsset(job.OutputMediaAssets.First(), output);
        return true;
    }

    static void DownloadAsset(IAsset asset, string outputDirectory)
    {
        foreach (IAssetFile file in asset.AssetFiles)
        {
            file.Download(Path.Combine(outputDirectory, file.Name));
        }
    }


    static IAsset CreateAssetAndUploadSingleFile(string filePath, string assetName, AssetCreationOptions options)
    {
        IAsset asset = context.Assets.Create(assetName, options);

        var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
        assetFile.Upload(filePath);

        return asset;
    }

    static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
    {
        var processor = context.MediaProcessors
        .Where(p => p.Name == mediaProcessorName)
        .ToList()
        .OrderBy(p => new Version(p.Version))
        .LastOrDefault();

        if (processor == null)
            throw new ArgumentException(string.Format("Unknown media processor",
                                                       mediaProcessorName));

        return processor;
    }

### <a id="file_types"></a>Toetatud failitüübid

- MP4
- MOV
- WMV



##<a name="media-services-learning-paths"></a>Media Servicesi Õppeteed

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Tagasiside saatmine

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="related-links"></a>Seotud lingid

[Azure Media Services Analytics ülevaade](media-services-analytics-overview.md)

[Azure Media Analytics demos](http://azuremedialabs.azurewebsites.net/demos/Analytics.html)
