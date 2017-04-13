<properties
    pageTitle="AES 128 dünaamiline krüptimise ja peamised teenuse abil | Microsoft Azure'i"
    description="Microsoft Azure Media Servicesi võimaldab teil esitada oma sisu krüptitud AES 128-bitise krüptimise võtmed. Media Servicesi näeb klahvi kohaletoimetamise teenus, mis pakub krüptimise võtmed autoriseeritud kasutajad. Selles teemas kirjeldatakse, kuidas dünaamiliselt Krüpti parooliga AES 128 ja peamised teenust kasutada."
    services="media-services"
    documentationCenter=""
    authors="Juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article" 
    ms.date="10/24/2016"
    ms.author="juliako"/>

#<a name="using-aes-128-dynamic-encryption-and-key-delivery-service"></a>AES 128 dünaamiline krüptimise ja teenus tootenumbri abil

> [AZURE.SELECTOR]
- [.NET-I](media-services-protect-with-aes128.md)
- [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
- [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)

##<a name="overview"></a>Ülevaade

>[AZURE.NOTE] Vaadake [seda](https://channel9.msdn.com/Shows/Azure-Friday/Azure-Media-Services-Protecting-your-Media-Content-with-AES-Encryption) videot, leiate ülevaate sellest, kuidas kaitsta oma meediumi sisu AES krüptimise abil.

Microsoft Azure Media Servicesi võimaldab Http-Live-Streaming (HLS) ja sujuv voogu krüptitud ja täiustatud Standard (AES) (128-bitise krüptimise klahvide abil). Media Servicesi näeb klahvi kohaletoimetamise teenus, mis pakub krüptimise võtmed autoriseeritud kasutajad. Kui soovite Media Servicesi vara krüptimiseks, peate krüptimisvõtme seostada vara ja konfigureerida ka autoriseerimine poliitikate tootevõtit. Kui voo nõuab mängija, kasutab Media Servicesi määratud klahvi dünaamiliselt krüptimiseks AES krüptimise abil sisu. Dekrüptida voo, mängija taotleb võti teenusest võtmed. Otsustada, kas kasutajal on õigus saada võti, hindab teenuse autoriseerimine poliitikate võti määratud.

Media Services toetab kinnitamise kasutajad, kes võtme taotlused mitu võimalust. Sisu võtme autoriseerimine poliitika võib olla üks või mitu autoriseerimine piirangud: avamine või sümboolne piirang. Turbeloa piiratud poliitika peab olema lisatud märgiks väljaandja järgi on turvaline Turbeloa teenus (STS). Media Services toetab sõned [Lihtne Web sõned](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2) (SWT) vorming ja [JSON Web Turbeloa](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3) (JWT) vormingus. Lisateabe saamiseks lugege teemat [konfigureerimine sisu klahvi autoriseerimine poliitika](media-services-protect-with-aes128.md#configure_key_auth_policy).

Dünaamiline krüptimise ära, peate olema varade, mis sisaldab mitme bitikiirusega MP4 failide või mitme bitikiirusega sujuva voogesituse lähtefailid. Samuti peate konfigureerimine kohaletoimetamise poliitika vara (käesolevas teemas kirjeldatud). Seejärel streaming URL-is määratud vormingu põhjal, tellitavate Streaming server tagab, et olete valinud protokollis saadetaks voo. Seetõttu peate ainult talletada ja ühe salvestusruumi-vormingus failide eest maksta ja Media Servicesi teenuse koostamine ja teeni sobiv vastus taotluste klientrakenduses.

Selles teemas oleks kasulik arendajatele töötavate rakendusi, mis on kaitstud meediumifailide esitamisel. Teema näitab, kuidas konfigureerida peamised teenuse autoriseerimine poliitika nii, et ainult volitatud kliendid saavad krüptimise võtmed. Samuti kujutatakse dünaamiline krüptimise.

>[AZURE.NOTE]Dünaamiline krüptimise kasutamise alustamiseks tuleb teil esmalt hankida vähemalt üks skaala ühik (tuntud ka kui streaming üksus). Lisateavet leiate teemast [skaala meediumid teenus](media-services-portal-manage-streaming-endpoints.md).

##<a name="aes-128-dynamic-encryption-and-key-delivery-service-workflow"></a>AES 128 dünaamiline krüptimise ja peamised teenuse töövoog

Järgnevalt on toodud üldised juhised, et teil oleks vaja teha oma varasid koos AES Media Servicesi peamised teenuse kasutamise ja ka dünaamiline krüptimine krüptimisel.

1. [Vara loomine ja laadige failid vara](media-services-protect-with-aes128.md#create_asset).
1. [Encode kohandatava bitrate MP4 seadmine faili sisaldav vara](media-services-protect-with-aes128.md#encode_asset).
1. [Sisu klahvi loomine ja seosta seda encoded vara](media-services-protect-with-aes128.md#create_contentkey). Media Services sisu võti sisaldab vara krüptovõtme.
1. [Konfigureerimine sisu klahvi autoriseerimine poliitika](media-services-protect-with-aes128.md#configure_key_auth_policy). Sisu võtme autoriseerimine poliitika peab teie konfigureeritud ja selleks, et sisu võti klient kohaletoimetamiseks kliendile.
1. [Vara poliitika kohaletoimetamise konfigureerimine](media-services-protect-with-aes128.md#configure_asset_delivery_policy). Kohaletoimetamise poliitika konfiguratsioon sisaldab: võti omandamise URL-i ja lähtestamine vektorkuju IV (AES 128 nõuab sama IV teatamiseks krüptimine ja dekrüptimine), kohaletoimetamise protocol (nt MÕTTEKRIIPSU MPEG, HLS, HDS, sujuva voogesituse või kõik), dünaamiline krüptimise (nt ümbriku või dünaamiline krüptimist) tüüp.

Muu poliitika võib rakendada sama vara iga protokolli. Näiteks võite rakendada PlayReady krüptimise sujuv/MÕTTEKRIIPSU ja AES ümbriku HLS. Mis tahes protokollid, mis on määratletud kohaletoimetamise poliitika (näiteks saate lisada ühe poliitika, mis määrab ainult HLS protokoll) streaming blokeeritud. Erandiks on, kui teil pole üldse määratud varade kohaletoimetamise poliitika. Klõpsake kõigi Protokollid lubatud selge.

1. Selleks, et saada streaming URL-i [loomine kuvatakse nõudmisel locator](media-services-protect-with-aes128.md#create_locator) .

Teema näitab, [Kuidas kliendi rakendus saate taotleda peamised teenuse klahvi](media-services-protect-with-aes128.md#client_request).

Siit leiate täieliku .net-i [näites](media-services-protect-with-aes128.md#example) on teema lõpus.

Järgmisel pildil näitab eespool kirjeldatud töövoog. Siin on luba kasutada autentimiseks.

![Kaitsta AES 128](./media/media-services-content-protection-overview/media-services-content-protection-with-aes.png)

Selles teemas ülejäänud pakub selgitused, koodi näited ja lingid teemadele, mis näitab, kuidas eespool kirjeldatud ülesannete täitmiseks.

##<a name="current-limitations"></a>Praeguse piirangud

Kui lisate või värskendada oma varade kohaletoimetamise poliitika, peate kustutama mõne olemasoleva locator (vajaduse korral) ja luua uus locator.

##<a id="create_asset"></a>Vara loomine ja failide üleslaadimine rakendusse vara

Hallata, kodeerida ja voona videoid, et peate esmalt laadige sisu rakendusse Microsoft Azure Media Servicesi. Üleslaaditud sisu talletatakse turvaliselt edasiseks töötlemiseks ja streaming pilveteenuses. 

Üksikasjalikku teavet leiate teemast [Media Services kontole faile üles laadida](media-services-dotnet-upload-files.md).

##<a id="encode_asset"></a>Kodeerida vara sisaldava faili kohandatava bitrate MP4 seadmine

Dünaamiline krüptimise abil piisab varade, mis sisaldab mitme bitikiirusega MP4 failide või mitme bitikiirusega sujuva voogesituse lähtefailid loomiseks. Seejärel alusel määratud vormingu loetelu või fragment taotluse, tellitavate Streaming server tagab kuvatakse voo valitud protokolli. Seetõttu peate ainult talletada ja ühe salvestusruumi-vormingus failide eest maksta ja Media Servicesi teenuse koostamine ja teeni sobiv vastus taotluste klientrakenduses. Lisateavet leiate teemast [Dünaamiline pakendit ülevaade](media-services-dynamic-packaging-overview.md) .

Kodeerida juhised leiate teemast [kodeerida abil Media kodeerija Standard vara](media-services-dotnet-encode-with-media-encoder-standard.md).

##<a id="create_contentkey"></a>Sisu klahvi ja sidumiseks encoded vara

Media Services sisu võti sisaldab klahvi, mida soovite krüptida vara.

Üksikasjalikku teavet leiate teemast [Loo sisu võti](media-services-dotnet-create-contentkey.md).

##<a id="configure_key_auth_policy"></a>Sisu klahvi autoriseerimine poliitika konfigureerimine

Media Services toetab kinnitamise kasutajad, kes võtme taotlused mitu võimalust. Sisu võtme autoriseerimine poliitika peab teie konfigureeritud ja klient (mängija) võti toimetada kliendi täidetud. Sisu võtme autoriseerimine poliitika võib olla üks või mitu autoriseerimine piirangud: avatud, sümboolne piirang või IP piiranguteta.

Üksikasjalikku teavet leiate teemast [Konfigureerimine sisu klahvi autoriseerimine poliitika](media-services-dotnet-configure-content-key-auth-policy.md).

##<a id="configure_asset_delivery_policy"></a>Varade kohaletoimetamise poliitika konfigureerimine 

Konfigureerige kohaletoimetamise poliitika oma vara. Mõned asjad, mida varade kohaletoimetamise poliitika konfiguratsioon sisaldab:

- Klahv omandamise URL. 
- Funktsiooni lähtestamine vektorkuju IV jaoks ümbriku krüptimine. AES 128 nõuab sama IV teatamiseks krüptimine ja dekrüptimine. 
- Varade kohaletoimetamise protocol (nt MÕTTEKRIIPSU MPEG, HLS, HDS, sujuva voogesituse või kõik).
- Dünaamiline krüptimise (nt AES ümbriku) tüübi või dünaamiline krüptimist. 

Üksikasjalikku teavet leiate teemast [konfigureerimine varade kohaletoimetamise poliitika ](media-services-rest-configure-asset-delivery-policy.md).

##<a id="create_locator"></a>Kuvatakse nõudmisel streaming locator selleks, et saada streaming URL-i loomine

Peate anda oma kasutaja sujuv, KRIIPSJOONE või HLS streaming URL-i.

>[AZURE.NOTE]Kui lisate või värskendada oma varade kohaletoimetamise poliitika, peate kustutama mõne olemasoleva locator (vajaduse korral) ja luua uus locator.

Juhised selle kohta, kuidas avaldada vara ja koostada streaming URL-i, lugege teemat [koostada streaming URL](media-services-deliver-streaming-content.md).

##<a name="get-a-test-token"></a>Test Turbeloa hankimine

Saada test Turbeloa Turbeloa piirang, mida kasutati võtme autoriseerimine poliitika põhjal.

    // Deserializes a string containing an Xml representation of a TokenRestrictionTemplate
    // back into a TokenRestrictionTemplate class instance.
    TokenRestrictionTemplate tokenTemplate = 
        TokenRestrictionTemplateSerializer.Deserialize(tokenTemplateString);
    
    // Generate a test token based on the data in the given TokenRestrictionTemplate.
    //The GenerateTestToken method returns the token without the word “Bearer” in front
    //so you have to add it in front of the token string. 
    string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate);
    Console.WriteLine("The authorization token is:\nBearer {0}", testToken);

[AMS Playeri](http://amsplayer.azurewebsites.net/azuremediaplayer.html) abil saate oma voo testida.

##<a id="client_request"></a>Kuidas oma kliendi taotlemine klahvi peamised teenuse?

Eelmises etapis ehitatud saate URL-i, mis viitab manifestfailile. Oma kliendi tuleb ekstraktida streaming manifestifailidele vajaliku teabe taotluse selleks, et klahv kohaletoimetamise teenusega.

###<a name="manifest-files"></a>Failide näidata

Kliendi tuleb ekstraktida URL-i (mis sisaldab ka sisu võti Id (lapsele)) väärtust Avaldamisfail. Seejärel üritab kliendi krüptovõtme toomine võtme teenus. Kliendi peab IV väärtus ja kasutage seda dekrüptida voo ekstraktimiseks. Järgmised koodilõigu kuvatakse soovitud <Protection> sujuva voogesituse manifesti element.

    <Protection>
      <ProtectionHeader SystemID="B47B251A-2409-4B42-958E-08DBAE7B4EE9">
        <ContentProtection xmlns:sea="urn:mpeg:dash:schema:sea:2012" schemeIdUri="urn:mpeg:dash:sea:2012">
          <sea:SegmentEncryption schemeIdUri="urn:mpeg:dash:sea:aes128-cbc:2013"/>
          <sea:KeySystem keySystemUri="urn:mpeg:dash:sea:keysys:http:2013"/>
          <sea:CryptoPeriod IV="0xD7D7D7D7D7D7D7D7D7D7D7D7D7D7D7D7" 
                            keyUriTemplate="https://wamsbayclus001kd-hs.cloudapp.net/HlsHandler.ashx?
                                            kid=da3813af-55e6-48e7-aa9f-a4d6031f7b4d"/>
        </ContentProtection>
      </ProtectionHeader>
    </Protection>

HLS, puhul on root manifest jagatud lõigu failid. 

Juurkausta manifest on näiteks: http://test001.origin.mediaservices.windows.net/8bfe7d6f-34e3-4d1a-b289-3e48a8762490/BigBuckBunny.ism/manifest(format=m3u8-aapl) ja see sisaldab lõigu faili nimede loend.
    
    . . . 
    #EXT-X-STREAM-INF:PROGRAM-ID=1,BANDWIDTH=630133,RESOLUTION=424x240,CODECS="avc1.4d4015,mp4a.40.2",AUDIO="audio"
    QualityLevels(514369)/Manifest(video,format=m3u8-aapl)
    #EXT-X-STREAM-INF:PROGRAM-ID=1,BANDWIDTH=965441,RESOLUTION=636x356,CODECS="avc1.4d401e,mp4a.40.2",AUDIO="audio"
    QualityLevels(842459)/Manifest(video,format=m3u8-aapl)
    …

Kui avate ühe lõigu faili tekstiredaktoris (nt http://test001.origin.mediaservices.windows.net/8bfe7d6f-34e3-4d1a-b289-3e48a8762490/BigBuckBunny.ism/QualityLevels (514369)/Manifest(video,format=m3u8-aapl), see peaks sisaldama #EXT-X-klahv, mis näitab, et fail on krüptitud.
    
    #EXTM3U
    #EXT-X-VERSION:4
    #EXT-X-ALLOW-CACHE:NO
    #EXT-X-MEDIA-SEQUENCE:0
    #EXT-X-TARGETDURATION:9
    #EXT-X-KEY:METHOD=AES-128,
    URI="https://wamsbayclus001kd-hs.cloudapp.net/HlsHandler.ashx?
         kid=da3813af-55e6-48e7-aa9f-a4d6031f7b4d",
            IV=0XD7D7D7D7D7D7D7D7D7D7D7D7D7D7D7D7
    #EXT-X-PROGRAM-DATE-TIME:1970-01-01T00:00:00.000+00:00
    #EXTINF:8.425708,no-desc
    Fragments(video=0,format=m3u8-aapl)
    #EXT-X-ENDLIST
    
###<a name="request-the-key-from-the-key-delivery-service"></a>Peamised teenuse võti taotlemine

Järgmine kood näitab, kuidas saada taotlus Media Servicesi peamised teenuse abil võtme kohaletoimetamise Uri (mis on saadud manifest) ja märgiks (see teema ei räägi kohta, kuidas saada lihtsa Web sõned Secure Turbeloa Service).

    private byte[] GetDeliveryKey(Uri keyDeliveryUri, string token)
    {
        HttpWebRequest request = (HttpWebRequest)WebRequest.Create(keyDeliveryUri);
                
        request.Method = "POST";
        request.ContentType = "text/xml";
        if (!string.IsNullOrEmpty(token))
        {
            request.Headers[AuthorizationHeader] = token;
        }
        request.ContentLength = 0;
    
        var response = request.GetResponse();
     
        var stream = response.GetResponseStream();
        if (stream == null)
        {
            throw new NullReferenceException("Response stream is null");
        }
    
        var buffer = new byte[256];
        var length = 0;
        while (stream.CanRead && length <= buffer.Length)
        {
            var nexByte = stream.ReadByte();
            if (nexByte == -1)
            {
                break;
            }
            buffer[length] = (byte)nexByte;
            length++;
        }
        response.Close();
    
        // AES keys must be exactly 16 bytes (128 bits).
        var key = new byte[length];
        Array.Copy(buffer, key, length);
        return key;
    }
    
##<a id="example"></a>Näide

1. Looge uus konsooli projekt.
1. Kasutage Nugeti installida ja Azure Media Services .NET SDK laiendid. Selle paketi installimisel installib Media Services .NET SDK ja lisab kõik nõutavad sõltuvused.
2. Otsingukonfiguratsiooni fail, mis sisaldab konto nimi ja olulise teabe lisamiseks tehke järgmist.

    
        <?xml version="1.0" encoding="utf-8"?>
        <configuration>
            <startup> 
                <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5" />
            </startup>
              <appSettings>
            
                <add key="MediaServicesAccountName" value="AccountName"/>
                <add key="MediaServicesAccountKey" value="AccountKey"/>
            
                <add key="Issuer" value="http://testacs.com"/>
                <add key="Audience" value="urn:test"/>
              </appSettings>
        </configuration>

1. Kirjutada kood Program.cs faili sellesse lahtrisse kood.
    
    Veenduge, et muutujate kaustad, kus asuvad teie failid osutamiseks värskendada.
            
        
        using System;
        using System.Collections.Generic;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using System.Net;
        using System.Security.Cryptography;
        using System.Text;
        using System.Threading.Tasks;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using Newtonsoft.Json.Linq;
        using System.Threading;
        using Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization;
        using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;
        using System.Web;
        using System.Globalization;
        
        namespace AESDynamicEncryptionAndKeyDeliverySvc
        {
            class Program
            {
                // Read values from the App.config file.
                private static readonly string _mediaServicesAccountName =
                    ConfigurationManager.AppSettings["MediaServicesAccountName"];
                private static readonly string _mediaServicesAccountKey =
                    ConfigurationManager.AppSettings["MediaServicesAccountKey"];
        
                // A Uri describing the issuer of the token.  
                // Must match the value in the token for the token to be considered valid.
                private static readonly Uri _sampleIssuer =
                    new Uri(ConfigurationManager.AppSettings["Issuer"]);
                // The Audience or Scope of the token.  
                // Must match the value in the token for the token to be considered valid.
                private static readonly Uri _sampleAudience =
                    new Uri(ConfigurationManager.AppSettings["Audience"]);
        
                // Field for service context.
                private static CloudMediaContext _context = null;
                private static MediaServicesCredentials _cachedCredentials = null;
        
                private static readonly string _mediaFiles =
                    Path.GetFullPath(@"../..\Media");
        
                private static readonly string _singleMP4File =
                    Path.Combine(_mediaFiles, @"BigBuckBunny.mp4");
        
        
                static void Main(string[] args)
                {
                    // Create and cache the Media Services credentials in a static class variable.
                    _cachedCredentials = new MediaServicesCredentials(
                                    _mediaServicesAccountName,
                                    _mediaServicesAccountKey);
                    // Used the chached credentials to create CloudMediaContext.
                    _context = new CloudMediaContext(_cachedCredentials);
        
                    bool tokenRestriction = false;
                    string tokenTemplateString = null;
        
                    IAsset asset = UploadFileAndCreateAsset(_singleMP4File);
                    Console.WriteLine("Uploaded asset: {0}", asset.Id);
        
                    IAsset encodedAsset = EncodeToAdaptiveBitrateMP4Set(asset);
                    Console.WriteLine("Encoded asset: {0}", encodedAsset.Id);
        
                    IContentKey key = CreateEnvelopeTypeContentKey(encodedAsset);
                    Console.WriteLine("Created key {0} for the asset {1} ", key.Id, encodedAsset.Id);
                    Console.WriteLine();
        
                    if (tokenRestriction)
                        tokenTemplateString = AddTokenRestrictedAuthorizationPolicy(key);
                    else
                        AddOpenAuthorizationPolicy(key);
        
                    Console.WriteLine("Added authorization policy: {0}", key.AuthorizationPolicyId);
                    Console.WriteLine();
        
                    CreateAssetDeliveryPolicy(encodedAsset, key);
                    Console.WriteLine("Created asset delivery policy. \n");
                    Console.WriteLine();
        
                    if (tokenRestriction && !String.IsNullOrEmpty(tokenTemplateString))
                    {
                        // Deserializes a string containing an Xml representation of a TokenRestrictionTemplate
                        // back into a TokenRestrictionTemplate class instance.
                        TokenRestrictionTemplate tokenTemplate =
                            TokenRestrictionTemplateSerializer.Deserialize(tokenTemplateString);
        
                        // Generate a test token based on the data in the given TokenRestrictionTemplate.
                        // Note, you need to pass the key id Guid because we specified 
                        // TokenClaim.ContentKeyIdentifierClaim in during the creation of TokenRestrictionTemplate.
                        Guid rawkey = EncryptionUtils.GetKeyIdAsGuid(key.Id);
        
                        //The GenerateTestToken method returns the token without the word “Bearer” in front
                        //so you have to add it in front of the token string. 
                        string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate, null, rawkey);
                        Console.WriteLine("The authorization token is:\nBearer {0}", testToken);
                        Console.WriteLine();
                    }
        
                    // You can use the bit.ly/aesplayer Flash player to test the URL 
                    // (with open authorization policy). 
                    // Paste the URL and click the Update button to play the video. 
                    //
                    string URL = GetStreamingOriginLocator(encodedAsset);
                    Console.WriteLine("Smooth Streaming Url: {0}/manifest", URL);
                    Console.WriteLine();
                    Console.WriteLine("HLS Url: {0}/manifest(format=m3u8-aapl)", URL);
                    Console.WriteLine();
        
                    Console.ReadLine();
                }
        
                static public IAsset UploadFileAndCreateAsset(string singleFilePath)
                {
                    if (!File.Exists(singleFilePath))
                    {
                        Console.WriteLine("File does not exist.");
                        return null;
                    }
        
                    var assetName = Path.GetFileNameWithoutExtension(singleFilePath);
                    IAsset inputAsset = _context.Assets.Create(assetName, AssetCreationOptions.StorageEncrypted);
        
                    var assetFile = inputAsset.AssetFiles.Create(Path.GetFileName(singleFilePath));
        
                    Console.WriteLine("Created assetFile {0}", assetFile.Name);
        
                    var policy = _context.AccessPolicies.Create(
                                            assetName,
                                            TimeSpan.FromDays(30),
                                            AccessPermissions.Write | AccessPermissions.List);
        
                    var locator = _context.Locators.CreateLocator(LocatorType.Sas, inputAsset, policy);
        
                    Console.WriteLine("Upload {0}", assetFile.Name);
        
                    assetFile.Upload(singleFilePath);
                    Console.WriteLine("Done uploading {0}", assetFile.Name);
        
                    locator.Delete();
                    policy.Delete();
        
                    return inputAsset;
                }
        
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
                        AssetCreationOptions.StorageEncrypted);
        
                    job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
                    job.Submit();
                    job.GetExecutionProgressTask(CancellationToken.None).Wait();
        
                    return job.OutputMediaAssets[0];
                }
        
                private static IMediaProcessor GetLatestMediaProcessorByName(string mediaProcessorName)
                {
                    var processor = _context.MediaProcessors.Where(p => p.Name == mediaProcessorName).
                    ToList().OrderBy(p => new Version(p.Version)).LastOrDefault();
        
                    if (processor == null)
                        throw new ArgumentException(string.Format("Unknown media processor", mediaProcessorName));
        
                    return processor;
                }
        
                static public IContentKey CreateEnvelopeTypeContentKey(IAsset asset)
                {
                    // Create envelope encryption content key
                    Guid keyId = Guid.NewGuid();
                    byte[] contentKey = GetRandomBuffer(16);
        
                    IContentKey key = _context.ContentKeys.Create(
                                            keyId,
                                            contentKey,
                                            "ContentKey",
                                            ContentKeyType.EnvelopeEncryption);
        
                    // Associate the key with the asset.
                    asset.ContentKeys.Add(key);
        
                    return key;
                }
        
                static public void AddOpenAuthorizationPolicy(IContentKey contentKey)
                {
                    // Create ContentKeyAuthorizationPolicy with Open restrictions 
                    // and create authorization policy             
                    IContentKeyAuthorizationPolicy policy = _context.
                                            ContentKeyAuthorizationPolicies.
                                            CreateAsync("Open Authorization Policy").Result;
        
                    List<ContentKeyAuthorizationPolicyRestriction> restrictions =
                        new List<ContentKeyAuthorizationPolicyRestriction>();
        
                    ContentKeyAuthorizationPolicyRestriction restriction =
                        new ContentKeyAuthorizationPolicyRestriction
                        {
                            Name = "HLS Open Authorization Policy",
                            KeyRestrictionType = (int)ContentKeyRestrictionType.Open,
                            Requirements = null // no requirements needed for HLS
                                };
        
                    restrictions.Add(restriction);
        
                    IContentKeyAuthorizationPolicyOption policyOption =
                        _context.ContentKeyAuthorizationPolicyOptions.Create(
                        "policy",
                        ContentKeyDeliveryType.BaselineHttp,
                        restrictions,
                        "");
        
                    policy.Options.Add(policyOption);
        
                    // Add ContentKeyAutorizationPolicy to ContentKey
                    contentKey.AuthorizationPolicyId = policy.Id;
                    IContentKey updatedKey = contentKey.UpdateAsync().Result;
                    Console.WriteLine("Adding Key to Asset: Key ID is " + updatedKey.Id);
                }
        
                public static string AddTokenRestrictedAuthorizationPolicy(IContentKey contentKey)
                {
                    string tokenTemplateString = GenerateTokenRequirements();
        
                    IContentKeyAuthorizationPolicy policy = _context.
                                            ContentKeyAuthorizationPolicies.
                                            CreateAsync("HLS token restricted authorization policy").Result;
        
                    List<ContentKeyAuthorizationPolicyRestriction> restrictions =
                            new List<ContentKeyAuthorizationPolicyRestriction>();
        
                    ContentKeyAuthorizationPolicyRestriction restriction =
                            new ContentKeyAuthorizationPolicyRestriction
                            {
                                Name = "Token Authorization Policy",
                                KeyRestrictionType = (int)ContentKeyRestrictionType.TokenRestricted,
                                Requirements = tokenTemplateString
                            };
        
                    restrictions.Add(restriction);
        
                    //You could have multiple options 
                    IContentKeyAuthorizationPolicyOption policyOption =
                        _context.ContentKeyAuthorizationPolicyOptions.Create(
                            "Token option for HLS",
                            ContentKeyDeliveryType.BaselineHttp,
                            restrictions,
                            null  // no key delivery data is needed for HLS
                            );
        
                    policy.Options.Add(policyOption);
        
                    // Add ContentKeyAutorizationPolicy to ContentKey
                    contentKey.AuthorizationPolicyId = policy.Id;
                    IContentKey updatedKey = contentKey.UpdateAsync().Result;
                    Console.WriteLine("Adding Key to Asset: Key ID is " + updatedKey.Id);
        
                    return tokenTemplateString;
                }
        
                static public void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
                {
                    Uri keyAcquisitionUri = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.BaselineHttp);
        
                    string envelopeEncryptionIV = Convert.ToBase64String(GetRandomBuffer(16));
            
                    // When configuring delivery policy, you can choose to associate it
                    // with a key acquisition URL that has a KID appended or
                    // or a key acquisition URL that does not have a KID appended  
                    // in which case a content key can be reused. 

                    // EnvelopeKeyAcquisitionUrl:  contains a key ID in the key URL.
                    // EnvelopeBaseKeyAcquisitionUrl:  the URL does not contains a key ID

                    // The following policy configuration specifies: 
                    // key url that will have KID=<Guid> appended to the envelope and
                    // the Initialization Vector (IV) to use for the envelope encryption.
                    
                    Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
                        new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
                    {
                                {AssetDeliveryPolicyConfigurationKey.EnvelopeKeyAcquisitionUrl, keyAcquisitionUri.ToString()}
                    };
        
                    IAssetDeliveryPolicy assetDeliveryPolicy =
                        _context.AssetDeliveryPolicies.Create(
                                    "AssetDeliveryPolicy",
                                    AssetDeliveryPolicyType.DynamicEnvelopeEncryption,
                                    AssetDeliveryProtocol.SmoothStreaming | AssetDeliveryProtocol.HLS | AssetDeliveryProtocol.Dash,
                                    assetDeliveryPolicyConfiguration);
        
                    // Add AssetDelivery Policy to the asset
                    asset.DeliveryPolicies.Add(assetDeliveryPolicy);
                    Console.WriteLine();
                    Console.WriteLine("Adding Asset Delivery Policy: " +
                        assetDeliveryPolicy.AssetDeliveryPolicyType);
                }
        
                static public string GetStreamingOriginLocator(IAsset asset)
                {
        
                    // Get a reference to the streaming manifest file from the  
                    // collection of files in the asset. 
        
                    var assetFile = asset.AssetFiles.Where(f => f.Name.ToLower().
                                                EndsWith(".ism")).
                                                FirstOrDefault();
        
                    // Create a 30-day readonly access policy. 
                    // You cannot create a streaming locator using an AccessPolicy that includes write or delete permissions.            
                    IAccessPolicy policy = _context.AccessPolicies.Create("Streaming policy",
                        TimeSpan.FromDays(30),
                        AccessPermissions.Read);
        
                    // Create a locator to the streaming content on an origin. 
                    ILocator originLocator = _context.Locators.CreateLocator(LocatorType.OnDemandOrigin, asset,
                        policy,
                        DateTime.UtcNow.AddMinutes(-5));
        
                    // Create a URL to the manifest file. 
                    return originLocator.Path + assetFile.Name;
                }
        
                static private string GenerateTokenRequirements()
                {
                    TokenRestrictionTemplate template = new TokenRestrictionTemplate(TokenType.SWT);
        
                    template.PrimaryVerificationKey = new SymmetricVerificationKey();
                    template.AlternateVerificationKeys.Add(new SymmetricVerificationKey());
                    template.Audience = _sampleAudience.ToString();
                    template.Issuer = _sampleIssuer.ToString();
        
                    template.RequiredClaims.Add(TokenClaim.ContentKeyIdentifierClaim);
        
                    return TokenRestrictionTemplateSerializer.Serialize(template);
                }
        
                static private void JobStateChanged(object sender, JobStateChangedEventArgs e)
                {
                    Console.WriteLine(string.Format("{0}\n  State: {1}\n  Time: {2}\n\n",
                        ((IJob)sender).Name,
                        e.CurrentState,
                        DateTime.UtcNow.ToString(@"yyyy_M_d__hh_mm_ss")));
                }
        
                static private byte[] GetRandomBuffer(int size)
                {
                    byte[] randomBytes = new byte[size];
                    using (RNGCryptoServiceProvider rng = new RNGCryptoServiceProvider())
                    {
                        rng.GetBytes(randomBytes);
                    }
        
                    return randomBytes;
                }
            }
        }


##<a name="media-services-learning-paths"></a>Media Servicesi Õppeteed

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Tagasiside saatmine

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
