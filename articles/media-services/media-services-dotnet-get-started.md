<properties
    pageTitle="Alustamine videosisu nõudmisel .net-i abil | Azure'i"
    description="Selles õpetuses juhendab teid rakendamiseks sees nõudmisel sisu kohaletoimetamise rakenduse abil Azure Media Servicesi kaudu .net-i juhiseid."
    services="media-services"
    documentationCenter=""
    authors="Juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/17/2016"
    ms.author="juliako"/>


# <a name="get-started-with-delivering-content-on-demand-using-net-sdk"></a>Kasutades .NET SDK nõudmisel videosisu alustamine

[AZURE.INCLUDE [media-services-selector-get-started](../../includes/media-services-selector-get-started.md)]

>[AZURE.NOTE]
> Selle õpetuse tegemiseks peate Azure'i konto. Lisateavet leiate teemast [Azure tasuta prooviversioon](/pricing/free-trial/?WT.mc_id=A261C142F). 
 
##<a name="overview"></a>Ülevaade 

Selles õpetuses juhendab teid rakendamiseks Video-on-Demand (VoD) sisu kohaletoimetamise rakenduse abil Azure Media Services (AMS) SDK .net-i juhiseid.


Õpetuse tutvustab lihtsa Media Servicesi töövoo ja kõige levinum programmeerimise objektid ja ülesandeid Media Servicesi arengu. Õpetuse lõpus saab voogesituseks või järk-järgult laadida valimi media faili, mis on üles laaditud, kodeeritud ja alla laaditud.

## <a name="what-youll-learn"></a>Käsitletavad

Õpetuse näitab, kuidas teha järgmisi toiminguid:

1.  Looge konto Media Services (kasutades Azure portaali).
2.  Konfigureerige streaming lõpp-punkti (kasutades Azure portaali).
3.  Loomine ja konfigureerimine Visual Studio projekti.
5.  Media Servicesi kontoga ühendust luua.
6.  Looge uus vara ja video faili üleslaadimine.
7.  Kodeerida lähtefail failikogumi kohandatava bitrate MP4.
8.  Vara avaldamine ja URL-ID hankimine videote voogesitamise ja järk-järgult allalaadimine.
9.  Testige oma sisu.

## <a name="prerequisites"></a>Eeltingimused

Õpetuse lõpule viimiseks on nõutav.

- Selle õpetuse tegemiseks peate Azure'i konto. 
    
    Kui teil pole kontot, saate luua tasuta prooviversiooni konto vaid paar minutit. Lisateavet leiate teemast [Azure tasuta prooviversioon](/pricing/free-trial/?WT.mc_id=A261C142F). Saate proovida makstud Azure'i teenuste kasutatavate krediidi summa liitmisel. Isegi juhul, kui tegu on kasutanud, saate konto hoida ja tasuta Azure teenused ja funktsioonid, näiteks veebirakenduste funktsioon teenuses Azure rakenduse kasutamine.
- Operatsioonisüsteemid: Windows 8 või uuem versioon, Windows 2008 R2, Windows 7.
- .NET framework 4.0 või uuem versioon
- Visual Studio 2010 SP1 (Professional, Premium, Ultimate või Express) või uuemad versioonid.


##<a name="download-sample"></a>Laadige alla näidis

Saada ja käivitage valimi [siin](https://azure.microsoft.com/documentation/samples/media-services-dotnet-on-demand-encoding-with-media-encoder-standard/).

## <a name="create-an-azure-media-services-account-using-the-azure-portal"></a>Azure'i portaalis Azure Media Servicesi konto loomine

Selle jaotise juhised näitab, kuidas luua konto AMS.

1. Logige sisse veebisaidil [Azure portaali](https://portal.azure.com/).
2. Valige **+ Uus** > **Media + CDN** > **Media Services**.

    ![Media Servicesi loomine](./media/media-services-portal-vod-get-started/media-services-new1.png)

3. Sisestage **MEDIA SERVICES konto loomine** nõutav väärtused.

    ![Media Servicesi loomine](./media/media-services-portal-vod-get-started/media-services-new3.png)
    
    1. Sisestage väljale **Konto nimi**uue AMS konto nimi. Media Servicesi konto nimi on kõik väiketähtedeks või -tähed ilma tühikuteta ja on 3 kuni 24 märki.
    2. Valige tellimus, vahel eri Azure'i tellimused, mida teil on juurdepääs.
    
    2. **Ressursirühm**valige uue või olemasoleva ressurss.  Ressursirühma on ressursid, millest jagada elutsükli, õiguste ja poliitikate kogum. Lisateavet [siin](azure-resource-manager/resource-group-overview.md#resource-groups).
    3. **Asukoht**, valige geograafiliselt saab talletada meediumid ja metaandmete kirjeid Media Servicesi konto jaoks. See piirkond saab töödelda ja voogesitus. Ainult saadaval Media Servicesi piirkondade kuvatakse väljal ripploendist. 
    
    3. **Salvestusruumi konto**, valige salvestusruumi konto bloobimälu Media Servicesi kontolt meediumi sisu esitada. Saate valida olemasoleva salvestusruumi konto sama geograafilise piirkonna Media Servicesi konto või luua salvestusruumi konto. Piirkonna on loodud uue konto salvestusruumi. Salvestusruumi kontonimed reeglid on samad Media Servicesi kontod.

        Lisateavet mäluruumi [siin](storage-introduction.md).

    4. Valige **Kinnita armatuurlaua** konto juurutamise edenemise kuvamiseks.
    
7. Klõpsake nuppu **Loo** vormi allservas.

    Kui konto on loodud, olek muutub **töötab**. 

    ![Media Servicesi sätted](./media/media-services-portal-vod-get-started/media-services-settings.png)

    AMS konto haldamiseks (nt videote üleslaadimine, kodeerida varad, töö edenemist jälgida) aknas **sätted** .

## <a name="configure-streaming-endpoints-using-the-azure-portal"></a>Azure'i portaalis streaming lõpp-punktid konfigureerimine

Kui te töötate Azure Media Servicesi on üks kõige levinumad stsenaariumid video voogesitus kohandatava bitrate kaudu pakkuda oma klientidele. Media Services toetab järgmisi kohandatava bitrate voogesituse tehnoloogiaid: HTTP Live Streaming (HLS), sujuva voogesituse, MPEG KRIIPSJOONE ja HDS (Adobe PrimeTime Access kes ainult) jaoks.

Media Servicesi pakub dünaamiline pakendit, mis võimaldab teil esitada oma kohandatava bitrate MP4 kodeeritud sisu voogesitamine vormingud Media Services (MPEG MÕTTEKRIIPSU, HLS, sujuva voogesituse, HDS) lihtsalt õigel ajal, pole vaja salvestada iga streaming vormingud eelnevalt pakitud versioonid ei toeta.

Dünaamiliste pakendit ära, peate tehke järgmist:

- Kodeerida Standard (allikas) faili failikogumi kohandatava bitrate MP4 (kodeering juhiseid on näidatud allpool selle õpetuse).  
- Looge vähemalt üks streaming ühiku *streaming lõpp-punkti* , millest plaanite kohaletoimetamise sisu. Kuidas muuta streaming ühikute alltoodud juhiseid kuvamine

Dünaamiliste pakendit, peate ainult talletada ja ühe salvestusruumi-vormingus failide eest maksta ja Media Servicesi koostab ja pakutakse sobiv vastus taotluste klientrakenduses.

Luua ja muuta streaming reserveeritud üksuste arvu, tehke järgmist.


1. Klõpsake aknas **sätted** **Streaming lõpp-punktid**. 

2. Klõpsake nuppu Vaikesäte streaming lõpp-punkti. 

    Kuvatakse aken **Vaikimisi STREAMING lõpp-punkti üksikasjad** .

3. Streaming ühikute määramiseks libistage liugurit **Streaming üksused** .

    ![Streaming ühikud](./media/media-services-portal-vod-get-started/media-services-streaming-units.png)

4. Klõpsake muudatuste salvestamiseks nuppu **Salvesta** .

    >[AZURE.NOTE]Mis tahes uute üksuste eraldatud kuluda kuni 20 minutit.

##<a name="create-and-configure-a-visual-studio-project"></a>Loomine ja konfigureerimine Visual Studio projekti

1. Looge uus konsool rakendus C# Visual Studio 2013, Visual Studio 2012 või Visual Studio 2010 SP1. Sisestage **nimi**, **asukoht**ja **lahenduse nimi**ja seejärel klõpsake nuppu **OK**.

2. Kasutage [windowsazure.mediaservices.extensions](https://www.nuget.org/packages/windowsazure.mediaservices.extensions) Nugeti pakett installida **Azure Media Services .NET SDK laiendid**.  Media Services .NET SDK laiendid on laiend meetodite ja helper funktsioone, mis lihtsustab oma kood ja lihtsam töötada Media Services. Selle paketi installimisel installib **Media Services.NET SDK** ja lisab kõik nõutavad sõltuvused.

3. Lisage System.Configuration komplekti viide. Selle komplekti sisaldab **System.Configuration.ConfigurationManager** klassi kasutatakse konfiguratsiooni faile, näiteks App.config juurdepääsuks.

4. Avage fail App.config (lisamine projekti faili, kui see on lisatud, vaikimisi) ja mõne *appSettings* jaotise faili lisamine. Määrake oma Azure Media Servicesi konto nimi ja konto võti, väärtused, nagu on näidatud järgmises näites. Konto nimi ja olulise teabe saamiseks [Azure'i portaal](https://portal.azure.com/) ja valige konto AMS. Valige **sätted** > **võtmed**. Kuvab haldamine klahve windows konto nimi ja esmaseid ja teiseseid klahvid ei kuvata.

        <configuration>
        ...
          <appSettings>
            <add key="MediaServicesAccountName" value="Media-Services-Account-Name" />
            <add key="MediaServicesAccountKey" value="Media-Services-Account-Key" />
          </appSettings>
          
        </configuration>

5. Olemasoleva **abil** teksti alguses Program.cs faili kirjutada järgmine kood.

        using System;
        using System.Collections.Generic;
        using System.Linq;
        using System.Text;
        using System.Threading.Tasks;
        using System.Configuration;
        using System.Threading;
        using System.IO;
        using Microsoft.WindowsAzure.MediaServices.Client;
        

6. Saate luua uue kausta projektide Directory ja kopeerige .mp4 või .wmv faili, mida soovite kodeerida ja taustal alla laadida või järk-järgult alla laadida. Selles näites kasutatakse "C:\VideoFiles" tee.

##<a name="connect-to-the-media-services-account"></a>Media Servicesi kontoga ühenduse loomiseks

Media Servicesi koos .net-i kasutamisel tuleb kasutada **CloudMediaContext** klassi enamik Media Services kavandamise tööülesannete jaoks: ühenduse Media Servicesi konto; loomise, värskendamine, juurdepääs ja järgmiste objektide kustutamine: varad, varade faile, töö, juurdepääsupoliitikaid, lokaatorid jne.

Vaikimisi programmi klassi kirjutada järgmine kood. Kood näitab, kuidas lugeda ühenduse väärtused App.config faili ja kuidas luua **CloudMediaContext** objekti Media Servicesi ühenduse loomiseks. Media Servicesi ühenduse loomise kohta leiate lisateavet leiate teemast [Media Services Media Services SDK .net-i ühenduse loomine](http://msdn.microsoft.com/library/azure/jj129571.aspx).

Funktsiooni **Main** helistab meetodid, mis on määratletud edasise selles jaotises.

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
            try
            {
                // Create and cache the Media Services credentials in a static class variable.
                _cachedCredentials = new MediaServicesCredentials(
                                _mediaServicesAccountName,
                                _mediaServicesAccountKey);
                // Used the chached credentials to create CloudMediaContext.
                _context = new CloudMediaContext(_cachedCredentials);

                // Add calls to methods defined in this section.

                IAsset inputAsset =
                    UploadFile(@"C:\VideoFiles\BigBuckBunny.mp4", AssetCreationOptions.None);

                IAsset encodedAsset =
                    EncodeToAdaptiveBitrateMP4s(inputAsset, AssetCreationOptions.None);

                PublishAssetGetURLs(encodedAsset);
            }
            catch (Exception exception)
            {
                // Parse the XML error message in the Media Services response and create a new
                // exception with its content.
                exception = MediaServicesExceptionParser.Parse(exception);

                Console.Error.WriteLine(exception.Message);
            }
            finally
            {
                Console.ReadLine();
            }
        }

##<a name="create-a-new-asset-and-upload-a-video-file"></a>Looge uus vara ja video faili üleslaadimine

Media Services, saate üles laadida (või neelata) oma digitaalse vara failid. **Varade** üksus võib sisaldada video, heli, pilte, pisipiltide saidikogumid, teksti rajad, ja tiitrite failide (ja nende failide metaandmeid.)  Kui failid on üles laaditud, talletatakse teie sisu turvaliselt edasiseks töötlemiseks ja streaming pilveteenuses. Vara failid nimetatakse **Varade failid**.

Kõnede **CreateFromFile** (määratletud .NET SDK laiendid) määratletud **UploadFile** meetodit. **CreateFromFile** loob uue vara, kus määratud lähtefail on üles laaditud.

**CreateFromFile** meetod on **AssetCreationOptions** , mis võimaldab teil määrata ühe järgmistest suvanditest varade loomine:

- **Ükski** - krüptimist kasutatakse. See on vaikeväärtus. Pange tähele, et selle suvandi kasutamisel sisu pole kaitstud teel või ülejäänud salvestusruumi.
Kui plaanite esitamisel on MP4 abil järk-järgult allalaadimine, kasutage seda suvandit.
- **StorageEncrypted** – kasutage seda suvandit Eemalda sisu täiustatud Standard (AES)-256 bitise krüptimise, mis seejärel lisatud Azure Storage, kui see on salvestatud kohalikult abil krüptida krüptitakse ülejäänud. Varade salvestusruumi krüptimise abil kaitstud automaatselt krüptimata ja enne kodeerimine süsteemi krüptitud faili asukoht ja soovi uuesti krüptitud enne üleslaadimist tagasi nimega uue väljundi varade. Salvestusruumi krüptimise esmane kasutamine puhul on, kui soovite secure kvaliteetne Sisestuskeel media failide tugev krüptimist ja ülejäänud kettal.
- **CommonEncryptionProtected** – kasutage seda suvandit, mis on juba krüptitud ja levinud krüptimise või PlayReady DRM (nt sujuva voogesituse kaitstud PlayReady DRM) kaitsega sisu üleslaadimiseks.
- **EnvelopeEncryptionProtected** – kasutage seda suvandit HLS krüptitud AES üleslaadimiseks. Pange tähele, et failid on kodeeritud ja muuta halduri krüptitud.

**CreateFromFile** meetodit saate ka määrata pakkumist selleks, et aruande faili üleslaadimine edenemist.

Järgmises näites oleme Määrake **pole** varade suvandite kuvamiseks.

Saate lisada klassi programmi järgmisel viisil.

    static public IAsset UploadFile(string fileName, AssetCreationOptions options)
    {
        IAsset inputAsset = _context.Assets.CreateFromFile(
            fileName,
            options,
            (af, p) =>
            {
                Console.WriteLine("Uploading '{0}' - Progress: {1:0.##}%", af.Name, p.Progress);
            });

        Console.WriteLine("Asset {0} created.", inputAsset.Id);

        return inputAsset;
    }


##<a name="encode-the-source-file-into-a-set-of-adaptive-bitrate-mp4-files"></a>Kodeerida failikogumi kohandatava bitrate MP4 lähtefail

Pärast söömisega varad meediumi teenustesse, meedia võib olla kodeeritud transmuxed, vesimärkidega, ja nii edasi, enne kui see saadetakse kliendid. Need tegevused on ajastatud ja vastuolus mitmes eksemplaris tausta rolli suurt jõudlust ja kättesaadavuse tagamiseks. Neid tegevusi nimetatakse töö ja iga töö on koostatud atomic ülesanded, mida teha varade faili tegelik töö.

Nagu mainitud varasemas versioonis, kui te töötate Azure Media Servicesi, on üks kõige levinumad stsenaariumid pakkuda oma klientidele streaming kohandatava bitrate. Media Servicesi dünaamiliselt saate paketti failikogumi kohandatava bitrate MP4 ühte järgmistes vormingutes: HTTP Live Streaming (HLS), sujuva voogesituse, MPEG MÕTTEKRIIPSU ja HDS (Adobe PrimeTime Access kes ainult) jaoks.

Dünaamiliste pakendit ära, peate tehke järgmist:

- Kodeerida või transcode kohandatava bitrate MP4 või kohandatava bitrate sujuva voogesituse failide komplekt esitada oma Standard (allikas).  
- Vähemalt üks streaming ühiku saamiseks streaming lõpp-punkti, millest plaanite kohaletoimetamise sisu.

Järgmine kood näitab, kuidas on kodeering töökohtade edastada. Töö sisaldab ühe tööülesande, mis määrab Transcode Standard-faili abil **Media kodeerija Standard**kohandatava bitrate MP4s komplekt. Koodi esitab töö ja ootab, kuni see on lõpule viidud.

Kui töö on lõppenud, saate oleks võimalik voogesituseks teie vara või järk-järgult allalaadimine MP4 transkodeerida tulemusena loodud faile.
Pange tähele, et teil pole vaja on rohkem kui 0 streaming ühikut järk-järgult MP4 faile alla laadida.

Saate lisada klassi programmi järgmisel viisil.

    static public IAsset EncodeToAdaptiveBitrateMP4s(IAsset asset, AssetCreationOptions options)
    {
    
        // Prepare a job with a single task to transcode the specified asset
        // into a multi-bitrate asset.
    
        IJob job = _context.Jobs.CreateWithSingleTask(
            "Media Encoder Standard",
            "H264 Multiple Bitrate 720p",
            asset,
            "Adaptive Bitrate MP4",
            options);
    
        Console.WriteLine("Submitting transcoding job...");
    
    
        // Submit the job and wait until it is completed.
        job.Submit();
    
        job = job.StartExecutionProgressTask(
            j =>
            {
                Console.WriteLine("Job state: {0}", j.State);
                Console.WriteLine("Job progress: {0:0.##}%", j.GetOverallProgress());
            },
            CancellationToken.None).Result;
    
        Console.WriteLine("Transcoding job finished.");
    
        IAsset outputAsset = job.OutputMediaAssets[0];
    
        return outputAsset;
    }

##<a name="publish-the-asset-and-get-urls-for-streaming-and-progressive-download"></a>Vara avaldamine ja videote voogesitamise ja järk-järgult allalaadimine URL-ID hankimine

Voogesituseks või vara alla laadida, peate esmalt "avaldada" see on locator loomisega. Lokaatorid pakuvad juurdepääsu vara failid. Media Services toetab kahte tüüpi lokaatorid: OnDemandOrigin lokaatorid, kasutada meediumide (nt MÕTTEKRIIPSU MPEG, HLS või sujuva voogesituse) ja lokaatorid Accessi allkirja (SAS), kasutatakse meediumide faile alla laadida.

Pärast soovitud lokaatorid loomist saate koostada URL-id, mida kasutatakse voogesituseks või oma faile alla laadida.


Sujuva voogesituse streaming URL on järgmises vormingus:

     {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

Streaming URL HLS on järgmises vormingus:

     {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

MPEG MÕTTEKRIIPSU streaming URL on järgmises vormingus:

    {streaming endpoint name-media services account name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

SAS URL-i kasutatakse failide allalaadimiseks on järgmises vormingus:

    {blob container name}/{asset name}/{file name}/{SAS signature}

Media Services .NET SDK laiendid pakuvad mugav helper meetodid, mis tagastavad vormindatud URL-id on avaldatud vara.

Järgmine kood kasutab .NET SDK laiendid lokaatorid loomiseks ja saada streaming ja järk-järgult allalaadimine URL-id. Koodi ka näitab, kuidas faile alla laadida kohalikus kaustas.

Saate lisada klassi programmi järgmisel viisil.

    static public void PublishAssetGetURLs(IAsset asset)
    {
        // Publish the output asset by creating an Origin locator for adaptive streaming,
        // and a SAS locator for progressive download.

        _context.Locators.Create(
            LocatorType.OnDemandOrigin,
            asset,
            AccessPermissions.Read,
            TimeSpan.FromDays(30));

        _context.Locators.Create(
            LocatorType.Sas,
            asset,
            AccessPermissions.Read,
            TimeSpan.FromDays(30));


        IEnumerable<IAssetFile> mp4AssetFiles = asset
                .AssetFiles
                .ToList()
                .Where(af => af.Name.EndsWith(".mp4", StringComparison.OrdinalIgnoreCase));

        // Get the Smooth Streaming, HLS and MPEG-DASH URLs for adaptive streaming,
        // and the Progressive Download URL.
        Uri smoothStreamingUri = asset.GetSmoothStreamingUri();
        Uri hlsUri = asset.GetHlsUri();
        Uri mpegDashUri = asset.GetMpegDashUri();

        // Get the URls for progressive download for each MP4 file that was generated as a result
        // of encoding.
        List<Uri> mp4ProgressiveDownloadUris = mp4AssetFiles.Select(af => af.GetSasUri()).ToList();


        // Display  the streaming URLs.
        Console.WriteLine("Use the following URLs for adaptive streaming: ");
        Console.WriteLine(smoothStreamingUri);
        Console.WriteLine(hlsUri);
        Console.WriteLine(mpegDashUri);
        Console.WriteLine();

        // Display the URLs for progressive download.
        Console.WriteLine("Use the following URLs for progressive download.");
        mp4ProgressiveDownloadUris.ForEach(uri => Console.WriteLine(uri + "\n"));
        Console.WriteLine();

        // Download the output asset to a local folder.
        string outputFolder = "job-output";
        if (!Directory.Exists(outputFolder))
        {
            Directory.CreateDirectory(outputFolder);
        }

        Console.WriteLine();
        Console.WriteLine("Downloading output asset files to a local folder...");
        asset.DownloadToFolder(
            outputFolder,
            (af, p) =>
            {
                Console.WriteLine("Downloading '{0}' - Progress: {1:0.##}%", af.Name, p.Progress);
            });

        Console.WriteLine("Output asset files available at '{0}'.", Path.GetFullPath(outputFolder));
    }

##<a name="test-by-playing-your-content"></a>Katsetage, sisu esitamine  

Kui käivitate programmi, mis on määratletud eelmises jaotises, kuvatakse järgmine URL-ide konsooli aken.

Kohandatavad streaming URL-ID:

Sujuva voogesituse

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest

HLS

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=m3u8-aapl)

MPEG MÕTTEKRIIPSU

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=mpd-time-csf)

Järk-järgult allalaadimine URL-id (heli ja video).

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_3400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_2250kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1500kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1000kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_56kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z


Kasutage voogesituseks saate video [Azure Media Services Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).

Järk-järgult allalaadimine testimiseks URL-i kleepida brauseris (nt Internet Exploreri, Chrome'i või Safari).


##<a name="next-steps-media-services-learning-paths"></a>Järgmised toimingud: Media Servicesi õppeteemad

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Tagasiside saatmine

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


### <a name="looking-for-something-else"></a>Kas otsite midagi muud?

Kui selle teema ei sisalda, mida te ei oodanud, pole midagi või mõnel muul viisil ei vasta teie vajadustele, anna meile tagasisidet, kasutades Disqus teemas allpool.


<!-- Anchors. -->


<!-- URLs. -->
  [Web Platform Installer]: http://go.microsoft.com/fwlink/?linkid=255386
  [Portal]: http://manage.windowsazure.com/
