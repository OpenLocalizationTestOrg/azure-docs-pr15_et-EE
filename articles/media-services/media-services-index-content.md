<properties
    pageTitle="Azure Media indekseerija meediumifaile indekseerimine"
    description="Azure Media indekseerija võimaldab teil teha meediumifailide sisu otsida ja täisteksti logifaili tiitrid ja märksõnade jaoks luua. Selles teemas kirjeldatakse, kuidas kasutada Media indekseerija."
    services="media-services"
    documentationCenter=""
    authors="Asolanki"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/12/2016"   
    ms.author="adsolank;juliako;johndeu"/>


# <a name="indexing-media-files-with-azure-media-indexer"></a>Azure Media indekseerija meediumifaile indekseerimine


Azure Media indekseerija võimaldab teil teha meediumifailide sisu otsida ja täisteksti logifaili tiitrid ja märksõnade jaoks luua. Töötlete meediumifail ühe või mitme meediumifaile partii.  

>[AZURE.IMPORTANT] Kui indekseerimine sisu, veenduge, et kasutada meediumifailide, millel on väga selge kõne (ilma taustamuusika, müra, efektide ja mikrofoni sisisema). Asjakohase sisu mõned näited: Salvestatud koosolekuid, loenguid või esitlusi. Järgmine sisu ei pruugi olla sobiv indekseerimise: Filmid, telesaateid midagi kombineeritud heli- ja heliefekte, mille halvasti salvestatud sisu taustamüra (sisisema).


Mõne indekseerimise töö saate luua järgmised:

- Suletud pealdise failid järgmises vormingus: **saami**, **TTML**ja **WebVTT**.

    Tiitrite faile lisada sildi äratundmisrõõmu, mis on indekseerimise töökohtade vastavalt sellele, kuidas tundmatu allika video kõne on hinded.  Saate Kuva väljundi failide jaoks kasutatavuse äratundmisrõõmu väärtus. Madal Keskmine tähendab halva helikvaliteedi indekseerimise tulemused.
- Märksõna fail (XML).
- Heli indekseerimine bloobimälu faili (AIB) kasutamiseks SQL serveriga.

    Lisateabe saamiseks vaadake teemat [Azure Media indekseerija ja SQL serveri abil AIB failide](https://azure.microsoft.com/blog/2014/11/03/using-aib-files-with-azure-media-indexer-and-sql-server/).


Selles teemas kirjeldatakse, kuidas indekseerimise töökohtade **Index vara** ja **indekseerida mitu faili**loomiseks.

Azure Media indekseerija uusimad värskendused, leiate teemast [Media Services ajaveebide](#preset).

## <a name="using-configuration-and-manifest-files-for-indexing-tasks"></a>Tööülesannete indekseerimise konfigureerimine ja manifesti failide kasutamine

Saate määrata rohkem üksikasju indekseerimise tööülesanded tööülesande konfiguratsiooni abil. Näiteks saate määrata, millised metaandmete meediumi faili jaoks. Selle metaandmete on kasutada mootori keele sõnavara laiendamiseks ja parandab oluliselt Kõnetuvastuse täpsuse.  Teil on võimalus määrata failide soovitud väljund.

Mitme meediumifailide saab töödelda ka korraga manifest-faili abil.

Lisateavet leiate teemast [Tööülesande eelmääratud Azure Media indekseerija](https://msdn.microsoft.com/library/dn783454.aspx).

## <a name="index-an-asset"></a>Vara indekseerimine

Järgmisel viisil lisatud media fail aktiva ja loob töö, mida soovite indekseerida vara.

Pange tähele, et kui konfiguratsioonifail pole määratud, meediumifail indekseeritud vaikesätetega kõik.

    static bool RunIndexingJob(string inputMediaFilePath, string outputFolder, string configurationFile = "")
    {
        // Create an asset and upload the input media file to storage.
        IAsset asset = CreateAssetAndUploadSingleFile(inputMediaFilePath,
            "My Indexing Input Asset",
            AssetCreationOptions.None);

        // Declare a new job.
        IJob job = _context.Jobs.Create("My Indexing Job");

        // Get a reference to the Azure Media Indexer.
        string MediaProcessorName = "Azure Media Indexer";
        IMediaProcessor processor = GetLatestMediaProcessorByName(MediaProcessorName);

        // Read configuration from file if specified.
        string configuration = string.IsNullOrEmpty(configurationFile) ? "" : File.ReadAllText(configurationFile);

        // Create a task with the encoding details, using a string preset.
        ITask task = job.Tasks.AddNew("My Indexing Task",
            processor,
            configuration,
            TaskOptions.None);

        // Specify the input asset to be indexed.
        task.InputAssets.Add(asset);

        // Add an output asset to contain the results of the job.
        task.OutputAssets.AddNew("My Indexing Output Asset", AssetCreationOptions.None);

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
            Console.WriteLine("Exiting method due to job error.");
            return false;
        }

        // Download the job outputs.
        DownloadAsset(task.OutputAssets.First(), outputFolder);

        return true;
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
<!-- __ -->
### <a id="output_files"></a>Väljund faili

Vaikimisi on indekseerimise töö loob väljundi järgmised failid. Esimese väljundi varade salvestatakse failid.

Kui seal on rohkem kui üks sisestuskeel meediumi faili, loob indekseerija manifestifaili jaoks töö väljundid, nimega "JobResult.txt". Iga sisend meediumifail, tulemuseks AIB saami TTML, WebVTT ja märksõna-failid on järjestikku nummerdatud ja nimega "Alias" abil.

Faili nimi | Kirjeldus
----------|------------
__InputFileName.aib__ | Helifaili indekseerimise bloobimälu. <br /><br /> Helifaili indekseerimine bloobimälu (AIB) on kahendarvu faili Microsoft SQL server täisteksti otsingu abil saab otsida.  AIB fail on tugevam, kui lihtne pealdise failid, kuna see sisaldab iga sõna, mis võimaldab märksa otsingut alternatiive. <br/> <br/>See nõuab installi indekseerija SQL-i lisandmooduli kohta kohapeal töötab Microsoft SQL server 2008 või uuem versioon. Otsingu AIB, kasutades Microsoft SQL serveri täisteksti otsing pakub täpsema otsingu tulemusi, kui otsimine suletud pealdis failid, mis on loodud WAMI. See on selle AIB sisaldab sõna alternatiive sarnaselt mis heli tiitrite failid sisaldavad kõrgeim confidence word iga segmendi heli. Kui räägitakse sõnade otsimine on upmost oluline, siis on soovitatav kasutada koos AIB rakenduses Microsoft SQL Server.<br/><br/> Lisandmooduli allalaadimiseks klõpsake <a href="http://aka.ms/indexersql">Azure Media indekseerija SQL-i lisandmoodul</a>. <br/><br/>Samuti on võimalik kasutada muud otsingumootorid nagu Apache Lucene/Solri lihtsalt registrisse video tiitrite ja märksõna XML-faile, kuid selle tulemuseks vähem täpsed otsingutulemite.
__InputFileName.smi__<br />__InputFileName.ttml__<br />__InputFileName.vtt__ |Suletud pealdis (koopia) faile saami, TTML ja WebVTT vormingus.<br/><br/>Ta saab teha heli-ja videofailide inimestele kuulmispuudega inimestele.<br/><br/>Pealdise kuulub <b>äratundmisrõõmu</b> , mis on indekseerimise töökohtade põhjal kuidas tundmatu kõne video allikas hinded on sildi.  Saate Kuva väljundi failide jaoks kasutatavuse <b>äratundmisrõõmu</b> väärtus. Madal Keskmine tähendab halva helikvaliteedi indekseerimise tulemused.
__InputFileName.kw.xml<br />InputFileName.info__ |Märksõna ja teave failid. <br/><br/>Märksõna fail on XML-faili, mis sisaldab märksõnu ekstraktimist kõne sisu, sagedus ja offset teavet. <br/><br/>Teave fail on lihtteksti-faili, mis sisaldab Varundustöö teavet iga tuvastas termin. Esimene rida on eriline ja see sisaldab äratundmisrõõmu Keskmine. Iga järgmise rea on eraldatud väärtuste loend järgmised andmed: aeg, lõppaega, sõna/fraas, turvaline käivitamine. Kellaajad on esitatud sekundit ja usaldus on esitatud arvuna 0-1. <br/><br/>Näide joon: "1.20 1.45 sõna 0,67" <br/><br/>Need failid saate kasutada mitmel otstarbel, nagu näiteks, teha kõne analytics, või otsingumootorid nagu Bingi, Google või Microsoft SharePointi meediumifailid teha veel leitavad või isegi kasutatud pakkuda asjakohasemate reklaamide kokku puutuda.
__JobResult.txt__ |Väljundi manifesti, esita ainult siis, kui indekseerimine mitu faili, mis sisaldab järgmist:<br/><br/><table border="1"><tr><th>Sisendfail</th><th>Alias (pseudonüüm)</th><th>MediaLength</th><th>Tõrge</th></tr><tr><td>a.mp4</td><td>Media_1</td><td>300</td><td>0</td></tr><tr><td>b.mp4</td><td>Media_2</td><td>0</td><td>3000</td></tr><tr><td>c.mp4</td><td>Media_3</td><td>600</td><td>0</td></tr></table><br/>



Kui kõik Sisestuskeel meediumifailide indekseeritud, indekseerimise töö nurjub tõrkekood 4000. Lisateavet leiate teemast [tõrkekoodid](#error_codes).

## <a name="index-multiple-files"></a>Mitme faili indekseerimine

Järgmisel viisil lisatud mitu meediumifailide aktiva ja loob töö, mida soovite indekseerida kõik need failid partii.

Manifest faili laiendiga LST on loodud ja üleslaadimisel vara sisse. Avaldamisfail sisaldab kõiki varade failide loend. Lisateavet leiate teemast [Tööülesande eelmääratud Azure Media indekseerija](https://msdn.microsoft.com/library/dn783454.aspx).

    static bool RunBatchIndexingJob(string[] inputMediaFiles, string outputFolder)
    {
        // Create an asset and upload to storage.
        IAsset asset = CreateAssetAndUploadMultipleFiles(inputMediaFiles,
            "My Indexing Input Asset - Batch Mode",
            AssetCreationOptions.None);

        // Create a manifest file that contains all the asset file names and upload to storage.
        string manifestFile = "input.lst";            
        File.WriteAllLines(manifestFile, asset.AssetFiles.Select(f => f.Name).ToArray());
        var assetFile = asset.AssetFiles.Create(Path.GetFileName(manifestFile));
        assetFile.Upload(manifestFile);

        // Declare a new job.
        IJob job = _context.Jobs.Create("My Indexing Job - Batch Mode");

        // Get a reference to the Azure Media Indexer.
        string MediaProcessorName = "Azure Media Indexer";
        IMediaProcessor processor = GetLatestMediaProcessorByName(MediaProcessorName);

        // Read configuration.
        string configuration = File.ReadAllText("batch.config");

        // Create a task with the encoding details, using a string preset.
        ITask task = job.Tasks.AddNew("My Indexing Task - Batch Mode",
            processor,
            configuration,
            TaskOptions.None);

        // Specify the input asset to be indexed.
        task.InputAssets.Add(asset);

        // Add an output asset to contain the results of the job.
        task.OutputAssets.AddNew("My Indexing Output Asset - Batch Mode", AssetCreationOptions.None);

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
            Console.WriteLine("Exiting method due to job error.");
            return false;
        }

        // Download the job outputs.
        DownloadAsset(task.OutputAssets.First(), outputFolder);

        return true;
    }

    private static IAsset CreateAssetAndUploadMultipleFiles(string[] filePaths, string assetName, AssetCreationOptions options)
    {
        IAsset asset = _context.Assets.Create(assetName, options);

        foreach (string filePath in filePaths)
        {
            var assetFile = asset.AssetFiles.Create(Path.GetFileName(filePath));
            assetFile.Upload(filePath);
        }

        return asset;
    }

### <a name="partially-succeeded-job"></a>Osaliselt õnnestus töö

Kui kõik Sisestuskeel meediumifailide indekseeritud, indekseerimise töö nurjub tõrkekood 4000. Lisateavet leiate teemast [tõrkekoodid](#error_codes).


Luuakse samasse väljundid (nagu õnnestus tööde haldamine). Viidata saate väljundi Avaldamisfail teada Sisestuskeel faile, mis on veaväärtused veeru vastavalt nurjus. Nurjus, tulemuseks AIB, saami, TTML, WebVTT ja märksõna Sisestuskeel failide faile ei looda.

### <a id="preset"></a>Tööülesande eelseadistatud Azure Media indekseerija

Töötlemine: Azure Media indekseerija saab kohandada, pakkudes valikuline toimingu eelmääratud tööülesande kõrval.  Järgnevalt selle konfiguratsiooni XML-vormingus.

Nimi | Nõua | Kirjeldus
----|----|---
__sisestusmeetodi__ | FALSE | Varade failid, mida soovite indekseerida.</p><p>Azure Media indekseerija toetab meediumi järgmistes failivormingutes: MP4, WMV, MP3, M4A, WMA, AAC, WAV.</p><p>Saate määrata faili nimi (s) **Sisestuskeel** elemendi **nime** või **loendi** atribuut (nagu allpool näidatud). Kui määrate mis varade faili index, valitakse esmane fail. Kui on seatud esmane varade faili, Sisestuskeel varade esimest faili on indekseeritud.</p><p>Määrata varade faili nime, tehke.<br />`<input name="TestFile.wmv">`<br /><br />Samuti saate indekseerida mitme varade faile korraga (kuni 10-failid). Soovitud toiming<br /><br /><ol class="ordered"><li><p>Luua tekstifaili (manifestifaili) ja anda LST laiendamine. </p></li><li><p>Selle manifestifaili lisada oma Sisestuskeel vara kõigi varade nimede loend. </p></li><li><p>Vara (Laadi üles) thanifest faili lisada.  </p></li><li><p>Määrake Avaldamisfail nimi, siis sisendit loendi atribuut.<br />`<input list="input.lst">`</li></ol><br /><br />Märkus: Kui pikem kui 10 failide lisamine Avaldamisfail indekseerimise töö ei õnnestu tõrkekoodiga 2006.
__metaandmete__ | FALSE | Metaandmete jaoks kasutatud sõnavara kohandamise määratud varade failid.  Kasulik indekseerija mittestandardsed sõnavara sõnade, nagu on päris nimisõnad ettevalmistamine.<br />`<metadata key="..." value="..."/>` <br /><br />Eelmääratletud __võtmed__ __väärtused__ on võimalik pakkuda. Hetkel on toetatud järgmised võtmed:<br /><br />"pealkiri" ja "kirjeldus" - kasutatud sõnavara kohandamiseks saate keele modelleerimise oma töö ja parandada Kõnetuvastuse täpsuse.  Väärtused külvatakse Interneti otsingud kontekstipõhise asjakohaste teksti dokumendid, tõsta sisemise sõnastiku indekseerimise tööülesande kestuse sisu abil.<br />`<metadata key="title" value="[Title of the media file]" />`<br />`<metadata key="description" value="[Description of the media file] />"`
__funktsioonid__ <br /><br /> Lisatud versioon 1.2. Praegu on ainus toetatud funktsioon kõnetuvastus ("ASR").| FALSE | Funktsiooni Kõnetuvastus on sätted järgmised võtmed.<table><tr><th><p>Klahv</p></th>     <th><p>Kirjeldus</p></th><th><p>Näide väärtus</p></th></tr><tr><td><p>Keel</p></td><td><p>Loomulikus keeles MMS faili ära tunda.</p></td><td><p>Inglise, Hispaania</p></td></tr><tr><td><p>CaptionFormats</p></td><td><p>semikooloniga eraldatud loend soovitud pealdis väljundvormingus (vajaduse korral)</p></td><td><p>ttml; saami webvtt</p></td></tr><tr><td><p>GenerateAIB</p></td><td><p>Kahendmuutujaga lipu määrata, kas kasutada AIB faili kasutamiseks on vajalik (SQL serveri ja kliendi indekseerija IFilter).  Lisateabe saamiseks vaadake teemat <a href="http://azure.microsoft.com/blog/2014/11/03/using-aib-files-with-azure-media-indexer-and-sql-server/">Azure Media indekseerija ja SQL serveri abil AIB failide</a>.</p></td><td><p>True; FALSE</p></td></tr><tr><td><p>GenerateKeywords</p></td><td><p>Kahendmuutujaga lipu määrata, kas märksõna XML-failis on nõutav.</p></td><td><p>True; FALSE. </p></td></tr><tr><td><p>ForceFullCaption</p></td><td><p>Kahendmuutujaga lipu määrata, kas soovite jõustada täielik pealdiste (sõltumata tase).  </p><p>Vaikeväärtus on false, sellisel juhul sõnu ja väljendeid, mis on vähem kui 50% tase lõplik pealdise väljundeid välja jätta ja asendada kolmikpunkti ("...").  Kolmikpunkti on kasulikud pealdise kvaliteedikontroll ja auditeerimine.</p></td><td><p>True; FALSE. </p></td></tr></table>

### <a id="error_codes"></a>Tõrkekoodid

Tõrke, puhul peaks aruande Azure Media indekseerija tagasi ühte järgmistest tõrkekoodid:

Kood | Nimi | Võimalikud põhjused
-----|------|------------------
2000 | Sobimatu konfiguratsioon | Sobimatu konfiguratsioon
2001 | Vigaste Sisestuskeel varad | Puuduvad Sisestuskeel varad või tühja vara.
2002 | Vigaste manifesti | Manifest on tühi või manifesti sisaldab lubamatuid märke.
2003 | Meediumi faili alla laadida nurjus | Avaldamisfail URL ei sobi.
2004 | Mittetoetatavad Protocol (protokoll) | Meediumi URL-i protokoll pole toetatud.
2005 | Toetuseta failitüüp | Sisestuskeel media faili tüüp pole toetatud.
2006 | Sisestuskeel liiga palju faile | Sisestuskeel manifest on pikem kui 10 faile.
3000 | Nurjus dekodeerida media fail | Mittetoetatavad meediumi kodekiga <br/>või<br/> Rikutud media fail <br/>või<br/> Heli voo Sisestuskeel meedia.
4000 | Paketi indekseerimine osaliselt õnnestus | Mõned Sisestuskeel meediumifailid on indekseeritud nurjus. Lisateavet leiate teemast <a href="#output_files">väljund faili</a>.
muud | Sisemised tõrked | Võtke ühendust klienditoega. indexer@microsoft.com


## <a id="supported_languages"></a>Toetatud keeltes

Praegu toetatud inglise ja Hispaania keeles. Lisateabe saamiseks vt [v1.2 väljaanne ajaveebipostituse](https://azure.microsoft.com/blog/2015/04/13/azure-media-indexer-spanish-v1-2/).


##<a name="media-services-learning-paths"></a>Media Servicesi Õppeteed

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Tagasiside saatmine

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


## <a name="related-links"></a>Seotud lingid

[Azure Media Services Analytics ülevaade](media-services-analytics-overview.md)

[SQL serveri ja Azure Media indekseerija AIB failide kasutamine](https://azure.microsoft.com/blog/2014/11/03/using-aib-files-with-azure-media-indexer-and-sql-server/)

[Indekseerimise meediumifailide Azure Media indekseerija 2 eelvaatega](media-services-process-content-with-indexer2.md)
