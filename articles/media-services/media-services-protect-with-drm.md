<properties
    pageTitle="PlayReady ja/või Widevine dünaamiline levinud krüptimise abil | Microsoft Azure'i"
    description="Microsoft Azure Media Servicesi võimaldab teil esitada Microsoft PlayReady DRM kaitsega MPEG-KRIIPSJOONE, sujuva voogesituse ja Http Live Streaming (HLS) voogu. Selle abil saate tarne krüptitud Widevine DRM MÕTTEKRIIPSU. Selles teemas kirjeldatakse, kuidas krüptida dünaamiliselt PlayReady ja Widevine DRM."
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
    ms.topic="get-started-article" 
    ms.date="09/27/2016"
    ms.author="juliako"/>


#<a name="using-playready-andor-widevine-dynamic-common-encryption"></a>PlayReady ja/või Widevine dünaamiline levinud krüptimise abil

> [AZURE.SELECTOR]
- [.NET-I](media-services-protect-with-drm.md)
- [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
- [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)

Microsoft Azure Media Servicesi võimaldab teil esitada [Microsoft PlayReady DRM](https://www.microsoft.com/playready/overview/)kaitsega MPEG-kriips, sujuva voogesituse ja HTTP Live Streaming (HLS) voogu. Selle abil saate esitamisel krüptitud MÕTTEKRIIPSU voogu Widevine DRM litsentsid. PlayReady nii Widevine krüptitud määratlus levinud krüptimise (ISO/IEC 23001-7 CENC) kohta. Saate konfigureerida oma AssetDeliveryConfiguration kasutada Widevine [AMS.NET SDK](https://www.nuget.org/packages/windowsazure.mediaservices/) (alates versioonist 3.5.1) või REST API-ga.

Media Servicesi pakub teenust pakkuda PlayReady ja Widevine DRM litsentsid. Media Servicesi pakub API-d, mille abil saate konfigureerida õigused ja piirangud, mida soovite PlayReady või Widevine DRM käitusaja jõustamiseks, kui kasutaja esitatakse tagasi kaitstud sisu. Kui kasutaja taotleb kaitsega sisu, taotleda mängija rakenduse litsentsi teenusest AMS litsentsi. AMS litsentsi teenuse annab mängija litsentsi, kui see on lubatud. PlayReady või Widevine litsentsi sisaldab dekrüptimine klahvi, mida saab kasutada kliendi mängija dekrüptida ning voona sisu.


Saate kasutada ka järgmised AMS partnerite aitavad esitamisel Widevine litsentside: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [castLabs](http://castlabs.com/company/partners/azure/). Lisateavet leiate teemast: integreerimine [Axinom](media-services-axinom-integration.md) ja [castLabs](media-services-castlabs-integration.md).

Media Services toetab mitu võimalust lubada kasutajad, kes võtme taotlused. Sisu võtme autoriseerimine poliitika võib olla üks või mitu autoriseerimine piirangud: avamine või sümboolne piirang. Turbeloa piiratud poliitika peab olema lisatud märgiks väljaandja järgi on turvaline Turbeloa teenus (STS). Media Services toetab sõned [Lihtne Web sõned](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2) (SWT) vorming ja [JSON Web Turbeloa](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3) (JWT) vormingus. Lisateavet leiate teemast konfigureerimine sisu klahvi autoriseerimine poliitika.

Dünaamiline krüptimise ära, peate olema varade, mis sisaldab mitme bitikiirusega MP4 failide või mitme bitikiirusega sujuva voogesituse lähtefailid. Samuti peate vara (käesolevas teemas kirjeldatud) kohaletoimetamise poliitikate konfigureerimine. Seejärel streaming URL-is määratud vormingu põhjal, tellitavate Streaming server tagab, et olete valinud protokollis saadetaks voo. Seetõttu peate ainult talletada ja ühe salvestusruumi-vormingus failide eest maksta ja Media Servicesi koostamine ja teeni sobiv HTTP vastus iga kliendi taotluse alusel.

Selles teemas oleks kasulik arendajatele töötavate rakendusi, mis on kaitstud mitme digitaalõiguste, nt PlayReady ja Widevine meediumi esitamisel. Teema näitab, kuidas konfigureerida teenus PlayReady litsentsi autoriseerimine poliitikatega nii, et ainult volitatud kliendid saavad PlayReady või Widevine litsentsi. Samuti kujutatakse krüptimise dünaamiline krüptimise PlayReady või Widevine DRM üle MÕTTEKRIIPSU.

>[AZURE.NOTE]Dünaamiline krüptimise kasutamise alustamiseks tuleb teil esmalt hankida vähemalt üks skaala ühik (tuntud ka kui streaming üksus). Lisateavet leiate teemast [skaala meediumid teenus](media-services-portal-manage-streaming-endpoints.md).


##<a name="download-sample"></a>Laadige alla näidis

Selles artiklis [allpool](https://github.com/Azure-Samples/media-services-dotnet-dynamic-encryption-with-drm)kirjeldatud valimi saate alla laadida.

##<a name="configuring-dynamic-common-encryption-and-drm-license-delivery-services"></a>Dünaamiline levinud krüptimise ja DRM litsentsi kohaletoimetamise teenuste konfigureerimine

Järgnevalt on toodud üldised juhised, et teil oleks vaja teha, kui kaitsta oma varasid koos PlayReady Media Servicesi litsentsi kohaletoimetamise teenuse kasutamise ja ka dünaamiline krüptimise abil.

1. Looge vara ja failide üleslaadimine rakendusse vara.
1. Kodeerida kohandatava bitrate MP4 seadmine faili sisaldav vara.
1. Sisu klahvi ja sidumiseks encoded vara. Media Services sisu võti sisaldab vara krüptovõtme.
1. Konfigureerimine sisu klahvi autoriseerimine poliitika. Sisu võtme autoriseerimine poliitika peab teie konfigureeritud ja klient selleks, et sisu võti kohaletoimetamiseks kliendile.

Sisu võtme autoriseerimine poliitika loomisel peate määrama järgmist: kohaletoimetamise meetod (PlayReady või Widevine) piirangud (avatud või Turbeloa) ja peamised tüüp, mis määratleb, kuidas kliendile ([PlayReady](media-services-playready-license-template-overview.md) või [Widevine](media-services-widevine-license-template-overview.md) litsentsi malli) võti on esitatud teavet.
1. Vara poliitika kohaletoimetamise konfigureerimine. Kohaletoimetamise poliitika konfiguratsioon sisaldab: kohaletoimetamise protocol (nt MPEG KRIIPSJOONE, HLS, HDS, sujuva voogesituse või kõik), dünaamiline krüptimise (nt levinud krüptimine) PlayReady või Widevine litsentsi omandamise URL-i tüüp.

Muu poliitika võib rakendada sama vara iga protokolli. Näiteks võite rakendada PlayReady krüptimise sujuv/MÕTTEKRIIPSU ja AES ümbriku HLS. Mis tahes protokollid, mis on määratletud kohaletoimetamise poliitika (näiteks saate lisada ühe poliitika, mis määrab ainult HLS protokoll) streaming blokeeritud. Erandiks on, kui teil pole üldse määratud varade kohaletoimetamise poliitika. Klõpsake kõigi Protokollid lubatud selge.
1. Selleks, et saada streaming URL-i luua mõne nõudmisel locator.

Siit leiate täieliku .net-i näites teema lõpus.

Järgmisel pildil näitab eespool kirjeldatud töövoog. Siin on luba kasutada autentimiseks.

![Koos PlayReady kaitsmine](./media/media-services-content-protection-overview/media-services-content-protection-with-drm.png)

Selles teemas ülejäänud pakub selgitused, koodi näited ja lingid teemad, mis näitab, kuidas eespool kirjeldatud ülesannete täitmiseks.

##<a name="current-limitations"></a>Praeguse piirangud

Kui lisate või värskendada varade kohaletoimetamise poliitika, peate kustutama seotud locator (vajaduse korral) ja luua uus locator.

Piirangud krüptimisel Widevine koos Azure Media Servicesi: praegu ei toeta mitut sisu võtmed.

##<a name="create-an-asset-and-upload-files-into-the-asset"></a>Vara loomine ja failide üleslaadimine rakendusse vara

Hallata, kodeerida ja voona videoid, et peate esmalt laadige sisu rakendusse Microsoft Azure Media Servicesi. Üleslaaditud sisu talletatakse turvaliselt edasiseks töötlemiseks ja streaming pilveteenuses.

Üksikasjalikku teavet leiate teemast [Media Services kontole faile üles laadida](media-services-dotnet-upload-files.md).

##<a name="encode-the-asset-containing-the-file-to-the-adaptive-bitrate-mp4-set"></a>Kodeerida vara sisaldava faili kohandatava bitrate MP4 seadmine

Dünaamiline krüptimise abil piisab vara, mis sisaldab mitme bitikiirusega MP4 failide või mitme bitikiirusega sujuva voogesituse lähtefailid loomiseks. Seejärel manifesti ja fragment taotluse määratud vormingu põhjal, tellitavate Streaming server tagab kuvatakse voo valitud protokolli. Seetõttu peate ainult talletada ja ühe salvestusruumi-vormingus failide eest maksta ja Media Servicesi teenuse koostamine ja teeni sobiv vastus taotluste klientrakenduses. Lisateavet leiate teemast [Dünaamiline pakendit ülevaade](media-services-dynamic-packaging-overview.md) .

Kodeerida juhised leiate teemast [kodeerida abil Media kodeerija Standard vara](media-services-dotnet-encode-with-media-encoder-standard.md).


##<a id="create_contentkey"></a>Sisu klahvi ja sidumiseks encoded vara

Media Servicesi, sisaldab sisu võti võti, mida soovite krüptida vara.

Üksikasjalikku teavet leiate teemast [Loo sisu võti](media-services-dotnet-create-contentkey.md).


##<a id="configure_key_auth_policy"></a>Sisu klahvi autoriseerimine poliitika konfigureerimine

Media Services toetab kinnitamise kasutajad, kes võtme taotlused mitu võimalust. Sisu võtme autoriseerimine poliitika peab teie konfigureeritud ja klient (mängija) võti toimetada kliendi täidetud. Sisu võtme autoriseerimine poliitika võib olla üks või mitu autoriseerimine piirangud: avamine või sümboolne piirang.

Üksikasjalikku teavet leiate teemast [Konfigureerimine sisu klahvi autoriseerimine poliitika](media-services-dotnet-configure-content-key-auth-policy.md#playready-dynamic-encryption).

##<a id="configure_asset_delivery_policy"></a>Varade kohaletoimetamise poliitika konfigureerimine 

Konfigureerige kohaletoimetamise poliitika oma vara. Mõned asjad, mida varade kohaletoimetamise poliitika konfiguratsioon sisaldab:

- DRM litsentsi omandamise URL. 
- Varade kohaletoimetamise protocol (nt MÕTTEKRIIPSU MPEG, HLS, HDS, sujuva voogesituse või kõik). 
- Tüüp, dünaamiline krüptimise (sel juhul levinud krüptimise). 

Üksikasjalikku teavet leiate teemast [konfigureerimine varade kohaletoimetamise poliitika ](media-services-rest-configure-asset-delivery-policy.md).

##<a id="create_locator"></a>Kuvatakse nõudmisel streaming locator selleks, et saada streaming URL-i loomine

Peate anda oma kasutaja sujuv, MÕTTEKRIIPSU või HLS streaming URL-i.

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

##<a id="example"></a>Näide


Järgmises näites näitab funktsioonid, mis võeti kasutusele .net-i Azure Media Services SDK-versiooni 3.5.2 (täpsemalt määratleda Widevine litsentsi malli ja Azure Media Servicesi Widevine litsentsi taotleda võimalus). Installige pakett kasutati Nugeti pakett järgmine käsk:

    PM> Install-Package windowsazure.mediaservices -Version 3.5.2


1. Looge uus konsooli projekt.
1. Kasutage Nugeti installida ja Azure Media Services .NET SDK.
2. Täiendavate viidete lisamine: System.Configuration.
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

1. Vähemalt üks streaming ühiku saamiseks streaming lõpp-punkti, millest plaanite kohaletoimetamise sisu. Lisateavet leiate teemast: [konfigureerimine voogesituse lõpp-punktid](media-services-dotnet-get-started.md#configure-streaming-endpoint-using-the-portal).

1. Kirjuta kood Program.cs faili sellesse lahtrisse kood.
    
    Veenduge, et muutujate kaustad, kus asuvad teie failid osutamiseks värskendada.
        
        using System;
        using System.Collections.Generic;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using System.Threading;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization;
        using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;
        using Microsoft.WindowsAzure.MediaServices.Client.Widevine;
        using Newtonsoft.Json;
        
        namespace DynamicEncryptionWithDRM
        {
            class Program
            {
                // Read values from the App.config file.
                private static readonly string _mediaServicesAccountName =
                    ConfigurationManager.AppSettings["MediaServicesAccountName"];
                private static readonly string _mediaServicesAccountKey =
                    ConfigurationManager.AppSettings["MediaServicesAccountKey"];
        
                private static readonly Uri _sampleIssuer =
                    new Uri(ConfigurationManager.AppSettings["Issuer"]);
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
                    // Used the cached credentials to create CloudMediaContext.
                    _context = new CloudMediaContext(_cachedCredentials);
        
                    bool tokenRestriction = false;
                    string tokenTemplateString = null;
        
                    IAsset asset = UploadFileAndCreateAsset(_singleMP4File);
                    Console.WriteLine("Uploaded asset: {0}", asset.Id);
        
                    IAsset encodedAsset = EncodeToAdaptiveBitrateMP4Set(asset);
                    Console.WriteLine("Encoded asset: {0}", encodedAsset.Id);
        
                    IContentKey key = CreateCommonTypeContentKey(encodedAsset);
                    Console.WriteLine("Created key {0} for the asset {1} ", key.Id, encodedAsset.Id);
                    Console.WriteLine("PlayReady License Key delivery URL: {0}", key.GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense));
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
        
                        // Generate a test token based on the the data in the given TokenRestrictionTemplate.
                        // Note, you need to pass the key id Guid because we specified 
                        // TokenClaim.ContentKeyIdentifierClaim in during the creation of TokenRestrictionTemplate.
                        Guid rawkey = EncryptionUtils.GetKeyIdAsGuid(key.Id);
                        string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate, null, rawkey, 
                                                                                DateTime.UtcNow.AddDays(365));
                        Console.WriteLine("The authorization token is:\nBearer {0}", testToken);
                        Console.WriteLine();
                    }
        
                    // You can use the http://amsplayer.azurewebsites.net/azuremediaplayer.html player to test streams.
                    // Note that DASH works on IE 11 (via PlayReady), Edge (via PlayReady), Chrome (via Widevine).
                     
                    string url = GetStreamingOriginLocator(encodedAsset);
                    Console.WriteLine("Encrypted DASH URL: {0}/manifest(format=mpd-time-csf)", url);
        
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
                    IAsset inputAsset = _context.Assets.Create(assetName, AssetCreationOptions.None);
        
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
        
                static public IAsset EncodeToAdaptiveBitrateMP4Set(IAsset inputAsset)
                {
                    var encodingPreset = "H264 Multiple Bitrate 720p";
            
                    IJob job = _context.Jobs.Create(String.Format("Encoding into Mp4 {0} to {1}",
                                            inputAsset.Name,
                                            encodingPreset));
            
                    var mediaProcessors =
                        _context.MediaProcessors.Where(p => p.Name.Contains("Media Encoder Standard")).ToList();
            
                    var latestMediaProcessor =
                        mediaProcessors.OrderBy(mp => new Version(mp.Version)).LastOrDefault();
            
                    ITask encodeTask = job.Tasks.AddNew("Encoding", latestMediaProcessor, encodingPreset, TaskOptions.None);
                    encodeTask.InputAssets.Add(inputAsset);
                    encodeTask.OutputAssets.AddNew(String.Format("{0} as {1}", inputAsset.Name, encodingPreset),    AssetCreationOptions.StorageEncrypted);
            
                    job.StateChanged += new EventHandler<JobStateChangedEventArgs>(JobStateChanged);
                    job.Submit();
                    job.GetExecutionProgressTask(CancellationToken.None).Wait();
            
                    return job.OutputMediaAssets[0];
                }
        
        
                static public IContentKey CreateCommonTypeContentKey(IAsset asset)
                {
                    
                    Guid keyId = Guid.NewGuid();
                    byte[] contentKey = GetRandomBuffer(16);
        
                    IContentKey key = _context.ContentKeys.Create(
                                            keyId,
                                            contentKey,
                                            "ContentKey",
                                            ContentKeyType.CommonEncryption);
        
                    // Associate the key with the asset.
                    asset.ContentKeys.Add(key);
        
                    return key;
                }
        
                static public void AddOpenAuthorizationPolicy(IContentKey contentKey)
                {
        
                    // Create ContentKeyAuthorizationPolicy with Open restrictions 
                    // and create authorization policy          
        
                    List<ContentKeyAuthorizationPolicyRestriction> restrictions = new List<ContentKeyAuthorizationPolicyRestriction>
                    {
                        new ContentKeyAuthorizationPolicyRestriction
                        {
                            Name = "Open",
                            KeyRestrictionType = (int)ContentKeyRestrictionType.Open,
                            Requirements = null
                        }
                    };
        
                    // Configure PlayReady and Widevine license templates.
                    string PlayReadyLicenseTemplate = ConfigurePlayReadyLicenseTemplate();
        
                    string WidevineLicenseTemplate = ConfigureWidevineLicenseTemplate();
        
                    IContentKeyAuthorizationPolicyOption PlayReadyPolicy =
                        _context.ContentKeyAuthorizationPolicyOptions.Create("",
                            ContentKeyDeliveryType.PlayReadyLicense,
                                restrictions, PlayReadyLicenseTemplate);
        
                    IContentKeyAuthorizationPolicyOption WidevinePolicy =
                        _context.ContentKeyAuthorizationPolicyOptions.Create("", 
                            ContentKeyDeliveryType.Widevine, 
                            restrictions, WidevineLicenseTemplate);
        
                    IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                                ContentKeyAuthorizationPolicies.
                                CreateAsync("Deliver Common Content Key with no restrictions").
                                Result;
        
        
                    contentKeyAuthorizationPolicy.Options.Add(PlayReadyPolicy);
                    contentKeyAuthorizationPolicy.Options.Add(WidevinePolicy);
                    // Associate the content key authorization policy with the content key.
                    contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
                    contentKey = contentKey.UpdateAsync().Result;
                }
        
                public static string AddTokenRestrictedAuthorizationPolicy(IContentKey contentKey)
                {
                    string tokenTemplateString = GenerateTokenRequirements();
        
                    List<ContentKeyAuthorizationPolicyRestriction> restrictions = new List<ContentKeyAuthorizationPolicyRestriction>
                    {
                        new ContentKeyAuthorizationPolicyRestriction
                        {
                            Name = "Token Authorization Policy",
                            KeyRestrictionType = (int)ContentKeyRestrictionType.TokenRestricted,
                            Requirements = tokenTemplateString,
                        }
                    };
        
                    // Configure PlayReady and Widevine license templates.
                    string PlayReadyLicenseTemplate = ConfigurePlayReadyLicenseTemplate();
        
                    string WidevineLicenseTemplate = ConfigureWidevineLicenseTemplate();
        
                    IContentKeyAuthorizationPolicyOption PlayReadyPolicy =
                        _context.ContentKeyAuthorizationPolicyOptions.Create("Token option",
                            ContentKeyDeliveryType.PlayReadyLicense,
                                restrictions, PlayReadyLicenseTemplate);
        
                    IContentKeyAuthorizationPolicyOption WidevinePolicy =
                        _context.ContentKeyAuthorizationPolicyOptions.Create("Token option",
                            ContentKeyDeliveryType.Widevine,
                                restrictions, WidevineLicenseTemplate);
        
                    IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                                ContentKeyAuthorizationPolicies.
                                CreateAsync("Deliver Common Content Key with token restrictions").
                                Result;
        
                    contentKeyAuthorizationPolicy.Options.Add(PlayReadyPolicy);
                    contentKeyAuthorizationPolicy.Options.Add(WidevinePolicy);
        
                    // Associate the content key authorization policy with the content key
                    contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
                    contentKey = contentKey.UpdateAsync().Result;
        
                    return tokenTemplateString;
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
        
                static private string ConfigurePlayReadyLicenseTemplate()
                {
                    // The following code configures PlayReady License Template using .NET classes
                    // and returns the XML string.
        
                    //The PlayReadyLicenseResponseTemplate class represents the template for the response sent back to the end user. 
                    //It contains a field for a custom data string between the license server and the application 
                    //(may be useful for custom app logic) as well as a list of one or more license templates.
                    PlayReadyLicenseResponseTemplate responseTemplate = new PlayReadyLicenseResponseTemplate();
        
                    // The PlayReadyLicenseTemplate class represents a license template for creating PlayReady licenses
                    // to be returned to the end users. 
                    //It contains the data on the content key in the license and any rights or restrictions to be 
                    //enforced by the PlayReady DRM runtime when using the content key.
                    PlayReadyLicenseTemplate licenseTemplate = new PlayReadyLicenseTemplate();
                    //Configure whether the license is persistent (saved in persistent storage on the client) 
                    //or non-persistent (only held in memory while the player is using the license).  
                    licenseTemplate.LicenseType = PlayReadyLicenseType.Nonpersistent;
        
                    // AllowTestDevices controls whether test devices can use the license or not.  
                    // If true, the MinimumSecurityLevel property of the license
                    // is set to 150.  If false (the default), the MinimumSecurityLevel property of the license is set to 2000.
                    licenseTemplate.AllowTestDevices = true;
        
                    // You can also configure the Play Right in the PlayReady license by using the PlayReadyPlayRight class. 
                    // It grants the user the ability to playback the content subject to the zero or more restrictions 
                    // configured in the license and on the PlayRight itself (for playback specific policy). 
                    // Much of the policy on the PlayRight has to do with output restrictions 
                    // which control the types of outputs that the content can be played over and 
                    // any restrictions that must be put in place when using a given output.
                    // For example, if the DigitalVideoOnlyContentRestriction is enabled, 
                    //then the DRM runtime will only allow the video to be displayed over digital outputs 
                    //(analog video outputs won’t be allowed to pass the content).
        
                    //IMPORTANT: These types of restrictions can be very powerful but can also affect the consumer experience. 
                    // If the output protections are configured too restrictive, 
                    // the content might be unplayable on some clients. For more information, see the PlayReady Compliance Rules document.
        
                    // For example:
                    //licenseTemplate.PlayRight.AgcAndColorStripeRestriction = new AgcAndColorStripeRestriction(1);
        
                    responseTemplate.LicenseTemplates.Add(licenseTemplate);
        
                    return MediaServicesLicenseTemplateSerializer.Serialize(responseTemplate);
                }
        
        
                private static string ConfigureWidevineLicenseTemplate()
                {
                    var template = new WidevineMessage
                    {
                        allowed_track_types = AllowedTrackTypes.SD_HD,
                        content_key_specs = new[]
                        {
                            new ContentKeySpecs
                            {
                                required_output_protection = new RequiredOutputProtection { hdcp = Hdcp.HDCP_NONE},
                                security_level = 1,
                                track_type = "SD"
                            }
                        },
                        policy_overrides = new
                        {
                            can_play = true,
                            can_persist = true,
                            can_renew = false
                        }
                    };
        
                    string configuration = JsonConvert.SerializeObject(template);
                    return configuration;
                }
        
                static public void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
                {
                    // Get the PlayReady license service URL.
                    Uri acquisitionUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense);
                
                    // GetKeyDeliveryUrl for Widevine attaches the KID to the URL.
                    // For example: https://amsaccount1.keydelivery.mediaservices.windows.net/Widevine/?KID=268a6dcb-18c8-4648-8c95-f46429e4927c.  
                    // The WidevineBaseLicenseAcquisitionUrl (used below) also tells Dynamaic Encryption 
                    // to append /? KID =< keyId > to the end of the url when creating the manifest.
                    // As a result Widevine license acquisition URL will have KID appended twice, 
                    // so we need to remove the KID that in the URL when we call GetKeyDeliveryUrl.
            
                    Uri widevineUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.Widevine);
                    UriBuilder uriBuilder = new UriBuilder(widevineUrl);
                    uriBuilder.Query = String.Empty;
                    widevineUrl = uriBuilder.Uri;
        
                    Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
                        new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
                        {
                            {AssetDeliveryPolicyConfigurationKey.PlayReadyLicenseAcquisitionUrl, acquisitionUrl.ToString()},
                            {AssetDeliveryPolicyConfigurationKey.WidevineBaseLicenseAcquisitionUrl, widevineUrl.ToString()}
        
                        };
        
                    // In this case we only specify Dash streaming protocol in the delivery policy,
                    // All other protocols will be blocked from streaming.
                    var assetDeliveryPolicy = _context.AssetDeliveryPolicies.Create(
                            "AssetDeliveryPolicy",
                        AssetDeliveryPolicyType.DynamicCommonEncryption,
                        AssetDeliveryProtocol.Dash,
                        assetDeliveryPolicyConfiguration);
        
        
                    // Add AssetDelivery Policy to the asset
                    asset.DeliveryPolicies.Add(assetDeliveryPolicy);
        
                }
        
                /// <summary>
                /// Gets the streaming origin locator.
                /// </summary>
                /// <param name="assets"></param>
                /// <returns></returns>
                static public string GetStreamingOriginLocator(IAsset asset)
                {
        
                    // Get a reference to the streaming manifest file from the  
                    // collection of files in the asset. 
        
                    var assetFile = asset.AssetFiles.Where(f => f.Name.ToLower().
                                                 EndsWith(".ism")).
                                                 FirstOrDefault();
        
                    // Create a 30-day readonly access policy. 
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
        
                static private void JobStateChanged(object sender, JobStateChangedEventArgs e)
                {
                    Console.WriteLine(string.Format("{0}\n  State: {1}\n  Time: {2}\n\n",
                        ((IJob)sender).Name,
                        e.CurrentState,
                        DateTime.UtcNow.ToString(@"yyyy_M_d__hh_mm_ss")));
                }
        
                static private byte[] GetRandomBuffer(int length)
                {
                    var returnValue = new byte[length];
        
                    using (var rng =
                        new System.Security.Cryptography.RNGCryptoServiceProvider())
                    {
                        rng.GetBytes(returnValue);
                    }
        
                    return returnValue;
                }
            }
        }


## <a name="next-step"></a>Järgmise juhise juurde

Vaadake üle Media Servicesi õppeteemad.

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Tagasiside saatmine

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="see-also"></a>Vt ka

[CENC mitme-DRM ja juurdepääsu reguleerimine](media-services-cenc-with-multidrm-access-control.md)

[Widevine pakendit ja AMS konfigureerimine](http://mingfeiy.com/how-to-configure-widevine-packaging-with-azure-media-services)

[Google Widevine litsentsi kohaletoimetamise teenuses Azure Media Servicesi seoses](https://azure.microsoft.com/blog/announcing-general-availability-of-google-widevine-license-services/)
