<properties 
    pageTitle="Kaitsta oma sisu Apple FairPlay ja/või Microsoft PlayReady HLS | Microsoft Azure'i" 
    description="Selles teemas antakse ülevaade ja näidatakse, kuidas kasutada Azure Media Servicesi krüptimiseks dünaamiliselt teie Apple'i FairPlay HTTP Live Streaming (HLS) sisu. Samuti kujutatakse läbiviimist Media Servicesi litsentsi teenus FairPlay litsentside klientidele." 
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
    ms.date="09/27/2016"
    ms.author="juliako"/>

# <a name="protect-your-hls-content-with-apple-fairplay-andor-microsoft-playready"></a>Oma sisu Apple FairPlay ja/või Microsoft PlayReady HLS kaitsmine

Azure Media Servicesi võimaldab teil dünaamiliselt krüptida HTTP Live Streaming (HLS) sisu abil järgmistes vormingutes:  

- **AES 128 ümbriku kustutusklahvi** 

    Terve kogumi on krüptitud **AES 128 CBC** režiimis. IOS-i ja OSX mängija dekrüptimine voo algupäraselt on toetatud. Lisateabe saamiseks lugege [artiklit](media-services-protect-with-aes128.md).

- **Apple'i FairPlay** 

    Üksikute video ja heli näidised on krüptitud **AES 128 CBC** režiimis. **FairPlay Streaming** (Sekundis) on integreeritud operatsioonisüsteemid seadme kohalikke toega iOS-i ja Apple TV. Safari OS x võimaldab sekundis Media laiendid (EME) liidest kasutades.
- **Microsoft PlayReady**

Järgmisel pildil on kujutatud **HLS + FairPlay ja/või PlayReady dünaamiline krüptimise** töövoog.

![Koos FairPlay kaitsmine](./media/media-services-content-protection-overview/media-services-content-protection-with-fairplay.png)

See teema näitab, kuidas kasutada Azure Media Servicesi HLS sisu koos Apple'i FairPlay dünaamiliselt krüptimiseks. Samuti kujutatakse läbiviimist Media Servicesi litsentsi teenus FairPlay litsentside klientidele.

>[AZURE.NOTE] Kui soovite ka teie HLS sisu PlayReady krüptimiseks, peate levinud sisu klahvi ja sidumiseks oma vara. Samuti peate konfigureerimine sisu klahvi autoriseerimine poliitika, [kasutades PlayReady dünaamiline levinud krüptimise](media-services-protect-with-drm.md) teemas kirjeldatud.

    
## <a name="requirements-and-considerations"></a>Nõuded ja kaalutlused

- Järgmine on vaja teenuse AMS esitamisel HLS krüptitud FairPlay ja pakkuda FairPlay litsentsid.

    - Azure'i konto. Lisateavet leiate teemast [Azure tasuta prooviversioon](/pricing/free-trial/?WT.mc_id=A261C142F).
    - Media Servicesi konto. Media Servicesi konto loomiseks vaadake teemat [Loo konto](media-services-portal-create-account.md).
    - [Apple'i arengu programmi](https://developer.apple.com/)registreeru.
    - Apple'i jaoks on vaja sisu omanik saada [juurutamise pakett](https://developer.apple.com/contact/fps/). Märkige juba rakendatud KSM (võtme turvalisus moodul) koos Azure Media Servicesi ja et taotlemise lõplik sekundis paketi taotluse. Tekib juhiseid lõplik sekundis paketi luua sertimine ja küsige, mida kasutate FairPlay konfigureerimiseks saada. 

    - Azure Media Services .NET SDK versioon **3.6.0** või uuem versioon.

- AMS peamised küljel peab olema seatud järgmised üksused:
    - **Rakenduse Cert AC** - pfx-fail, mis sisaldab privaatvõti. See fail kliendi poolt loodud ja parooliga krüptitud samale kliendile. 
        
        Kui kliendi konfigureerib peamised poliitika, nad peate sisestama parooli ja pfx base64-vormingus.

        Järgmised sammud kirjeldavad Loo pfx sertifikaadi FairPlay tehke järgmist.
        
        1. Installige OpenSSL https://slproweb.com/products/Win32OpenSSL.html
        
            Avage kaust, kus asuvad FairPlay serti ja muid faile, mis on esitatud Apple.
        
        2. Käsurea teisendamiseks soovitud cer pem:
        
            "C:\OpenSSL-Win32\bin\openssl.exe" x509-teavitavad der-fairplay.cer sisse-välja fairplay-out.pem
        
        3. Käsurea teisendada pem pfx privaatvõti (pfx-fail parooli seejärel palub OpenSSL).
        
            "C:\OpenSSL-Win32\bin\openssl.exe" pkcs12-ekspordi - välja fairplay-out.pfx-inkey privatekey.pem-sisse fairplay-out.pem-passin file:privatekey-pem-pass.txt 
        
    - **Rakenduse Cert parooli** - kliendi parooli loomise pfx-fail.
    - **Rakenduse Cert parooli ID** - kliendi tuleb üles laadida sarnane kuidas need muud AMS klahvid ja **ContentKeyType.FairPlayPfxPassword** kellaajavälju väärtuse kasutamine üleslaadimine parool. Selle tulemusena nad saavad AMS id on need tuleb kasutada peamised poliitika suvand sees.
    - **iv** - 16 baiti juhusliku väärtuse, peab vastama iv varade kohaletoimetamise poliitika. Kliendi genereeritud on IV ja paigutab selle mõlemas kohas: varade kohaletoimetamise poliitika ja klahvi kohaletoimetamise poliitika suvand. 
    - **ASK** - ASK (rakenduse salajane võti) on saanud Apple'i arendaja portaalis kinnitamise loomisel. Iga arendusmeeskonnale saate kordumatu ASK. Esitage koopia salvestamine ja salvestada selle kindlas kohas. Peate konfigureerima ASK FairPlayAsk Azure Media Services hiljem. 
    -  **ASK ID** - saadakse, kui kliendi lisatud ASk AMS sisse. Kliendi tuleb üles laadida ASk **ContentKeyType.FairPlayASk** kellaajavälju väärtuse kasutamine. Selle tulemusena tagastatakse AMS ID-d ja see on, mida tuleb kasutada, kui poliitika suvandi võtmed.

- Kliendipoolse sekundis tuleb kindlaks määrata järgmised üksused:
    - **Rakenduse Cert AC** -.cer/.der fail, mis sisaldab avalik võti, mis OS kasutab mõnda last krüptimiseks. AMS on vaja teada, kuna see on vajalik mängija. Võtme teenus dekrüptitakse see vastav privaatvõti abil.

- Taasesitus FairPlay krüptitud voo, peate saada reaal ASK esimese ja seejärel looge reaal sert. Selle protsessi loob kõik 3 osa:

    -  .der, 
    -  pfx ja 
    -  funktsiooni pfx parool.
 
- Kliendid, mis toetavad HLS **AES 128 CBC** krüptimise abil: Safari OS X, Apple TV, iOS.

##<a name="steps-for-configuring-fairplay-dynamic-encryption-and-license-delivery-services"></a>FairPlay dünaamiline krüptimise ja litsentsi kohaletoimetamise services konfigureerimise juhised

Järgnevalt on toodud üldised juhised, et teil oleks vaja teha, kui kaitsta oma varasid koos FairPlay Media Servicesi litsentsi kohaletoimetamise teenuse kasutamise ja ka dünaamiline krüptimise abil.

1. Looge vara ja failide üleslaadimine rakendusse vara. 
1. Kodeerida kohandatava bitrate MP4 seadmine faili sisaldav vara.
1. Sisu klahvi ja sidumiseks encoded vara.  
1. Konfigureerimine sisu klahvi autoriseerimine poliitika. Sisu võtme autoriseerimine poliitika loomisel peate määrama järgmist: 
    
    - kohaletoimetusviis (sel juhul, FairPlay), 
    - FairPlay poliitika suvandite konfigureerimine. FairPlay konfigureerimise kohta leiate teavet artiklist allpool valimis ConfigureFairPlayPolicyOptions() meetod.
    
        >[AZURE.NOTE] Tavaliselt ei taha konfigureerimine FairPlay poliitika suvandid ainult üks kord, kuna teil on ainult üks kogum sertimine ja küsi.
-piirangute (Ava või Turbeloa) - ja peamised tüüp, mis määratleb, kuidas võti kliendile esitatud teavet. 
    
2. Varade kohaletoimetamise poliitika konfigureerimine. Kohaletoimetamise poliitika konfiguratsiooni sisaldab järgmist: 

    - kohaletoimetamise protocol (HLS) 
    - dünaamiline krüptimise (levinud CBC krüptimine) tüüp 
    - litsentsi omandamise URL. 
    
    >[AZURE.NOTE]Kui soovite pakkuda voo, on krüptitud FairPlay + teise DRM, peate eraldi kohaletoimetamise poliitikate konfigureerimine 
    >
    >- Ühe IAssetDeliveryPolicy CENC (PlayReady + WideVine) ja sujuvalt koos PlayReady MÕTTEKRIIPSU konfigureerimiseks. 
    >- Mõne muu IAssetDeliveryPolicy konfigureerida FairPlay HLS

1. Saate luua ka nõudmisel locator saada streaming URL.

##<a name="using-fairplay-key-delivery-by-playerclient-apps"></a>FairPlay võtme kohaletoimetamise mängija/kliendi rakenduste abil

Kliendid võivad tekkida mängija rakendused iOS SDK abil. FairPlay sisu esitada klientide olema litsentsi exchange protokolli rakendamiseks. Apple pole määratud litsentsi exchange protokolli. See on iga rakenduse kohta peamised päringuid. AMS FairPlay peamised teenuste loodab SPC tulla www-form-url encoded postituse sõnumina järgmisel kujul: 

    spc=<Base64 encoded SPC>

>[AZURE.NOTE] Azure Media Player ei toeta FairPlay välja kasti. Kliendid peavad saama valimi mängija Apple arendaja kontolt FairPlay taasesitus MAC OSX saada. 
 
##<a name="streaming-urls"></a>Streaming URL-id

Kui teie vara on rohkem kui üks DRM krüptitud, peaksite kasutama mõne krüptimise sildi streaming URL: (vormingus = 'm3u8 aapl' krüptimise = 'xxx').

Kehtivad järgmisega.

- Saate määrata ainult null või üks krüptimise tüüp.
- Krüptimise tüüp pole määratud URL-i, kui ainult ühe krüptimise rakendatud vara.
- Krüptimise tüüp on tõstutundlikud.
- Saate määrata krüptimise järgmist tüüpi:  
    - **Cenc**: levinud krüptimise (Playready või Widevine)
    - **cbcs-aapl**: Fairplay
    - **CBC**: AES ümbriku krüptimine.


##<a name="net-example"></a>.Net-i näide


Järgmises näites näitab funktsioonid, mis võeti kasutusele .net-i Azure Media Services SDK-versiooni 3.6.0 (võimalus läbiviimist Azure Media Servicesi FairPlay krüptitud sisu). Installige pakett kasutati Nugeti pakett järgmine käsk:

    PM> Install-Package windowsazure.mediaservices -Version 3.6.0


1. Konsooli projekti loomine.
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

1. Kirjutada kood Program.cs faili sellesse lahtrisse kood.
            
        
        using System;
        using System.Collections.Generic;
        using System.Configuration;
        using System.IO;
        using System.Linq;
        using System.Threading;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization;
        using Microsoft.WindowsAzure.MediaServices.Client.DynamicEncryption;
        using Microsoft.WindowsAzure.MediaServices.Client.FairPlay;
        using Newtonsoft.Json;
        using System.Security.Cryptography.X509Certificates;
        
        namespace DynamicEncryptionWithFairPlay
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
        
                    IContentKey key = CreateCommonCBCTypeContentKey(encodedAsset);
                    Console.WriteLine("Created key {0} for the asset {1} ", key.Id, encodedAsset.Id);
                    Console.WriteLine("FairPlay License Key delivery URL: {0}", key.GetKeyDeliveryUrl(ContentKeyDeliveryType.FairPlay));
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
        
                    string url = GetStreamingOriginLocator(encodedAsset);
                    Console.WriteLine("Encrypted HLS URL: {0}/manifest(format=m3u8-aapl)", url);
        
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
        
                static public IContentKey CreateCommonCBCTypeContentKey(IAsset asset)
                {
                    // Create HLS SAMPLE AES encryption content key
                    Guid keyId = Guid.NewGuid();
                    byte[] contentKey = GetRandomBuffer(16);
        
                    IContentKey key = _context.ContentKeys.Create(
                                            keyId,
                                            contentKey,
                                            "ContentKey",
                                            ContentKeyType.CommonEncryptionCbcs);
        
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
        
        
                    // Configure FairPlay policy option.
                    string FairPlayConfiguration = ConfigureFairPlayPolicyOptions();
        
                    IContentKeyAuthorizationPolicyOption FairPlayPolicy =
                        _context.ContentKeyAuthorizationPolicyOptions.Create("",
                        ContentKeyDeliveryType.FairPlay,
                        restrictions,
                        FairPlayConfiguration);
        
        
                    IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                                ContentKeyAuthorizationPolicies.
                                CreateAsync("Deliver Common CBC Content Key with no restrictions").
                                Result;
        
                    contentKeyAuthorizationPolicy.Options.Add(FairPlayPolicy);
        
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
        
                    // Configure FairPlay policy option.
                    string FairPlayConfiguration = ConfigureFairPlayPolicyOptions();
        
        
                    IContentKeyAuthorizationPolicyOption FairPlayPolicy =
                        _context.ContentKeyAuthorizationPolicyOptions.Create("Token option",
                               ContentKeyDeliveryType.FairPlay,
                               restrictions,
                               FairPlayConfiguration);
        
                    IContentKeyAuthorizationPolicy contentKeyAuthorizationPolicy = _context.
                                ContentKeyAuthorizationPolicies.
                                CreateAsync("Deliver Common CBC Content Key with token restrictions").
                                Result;
        
                    contentKeyAuthorizationPolicy.Options.Add(FairPlayPolicy);
        
                    // Associate the content key authorization policy with the content key
                    contentKey.AuthorizationPolicyId = contentKeyAuthorizationPolicy.Id;
                    contentKey = contentKey.UpdateAsync().Result;
        
                    return tokenTemplateString;
                }
        
                private static string ConfigureFairPlayPolicyOptions()
                {
                    // For testing you can provide all zeroes for ASK bytes together with the cert from Apple FPS SDK. 
                    // However, for production you must use a real ASK from Apple bound to a real prod certificate.
                    byte[] askBytes = Guid.NewGuid().ToByteArray();
                    var askId = Guid.NewGuid();
                    // Key delivery retrieves askKey by askId and uses this key to generate the response.
                    IContentKey askKey = _context.ContentKeys.Create(
                                            askId,
                                            askBytes,
                                            "askKey",
                                            ContentKeyType.FairPlayASk);
        
                    //Customer password for creating the .pfx file.
                    string pfxPassword = "<customer password for creating the .pfx file>";
                    // Key delivery retrieves pfxPasswordKey by pfxPasswordId and uses this key to generate the response.
                    var pfxPasswordId = Guid.NewGuid();
                    byte[] pfxPasswordBytes = System.Text.Encoding.UTF8.GetBytes(pfxPassword);
                    IContentKey pfxPasswordKey = _context.ContentKeys.Create(
                                            pfxPasswordId,
                                            pfxPasswordBytes,
                                            "pfxPasswordKey",
                                            ContentKeyType.FairPlayPfxPassword);
        
                    // iv - 16 bytes random value, must match the iv in the asset delivery policy.
                    byte[] iv = Guid.NewGuid().ToByteArray();
        
                    //Specify the .pfx file created by the customer.
                    var appCert = new X509Certificate2("path to the .pfx file created by the customer", pfxPassword, X509KeyStorageFlags.Exportable);
        
                    string FairPlayConfiguration =
                        Microsoft.WindowsAzure.MediaServices.Client.FairPlay.FairPlayConfiguration.CreateSerializedFairPlayOptionConfiguration(
                            appCert,
                            pfxPassword,
                            pfxPasswordId,
                            askId,
                            iv);
        
                    return FairPlayConfiguration;
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
        
                static public void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
                {
                    var kdPolicy = _context.ContentKeyAuthorizationPolicies.Where(p => p.Id == key.AuthorizationPolicyId).Single();
        
                    var kdOption = kdPolicy.Options.Single(o => o.KeyDeliveryType == ContentKeyDeliveryType.FairPlay);
        
                    FairPlayConfiguration configFP = JsonConvert.DeserializeObject<FairPlayConfiguration>(kdOption.KeyDeliveryConfiguration);
        
                    // Get the FairPlay license service URL.
                    Uri acquisitionUrl = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.FairPlay);
        
                    // The reason the below code replaces "https://" with "skd://" is because
                    // in the IOS player sample code which you obtained in Apple developer account, 
                    // the player only recognizes a Key URL that starts with skd://. 
                    // However, if you are using a customized player, 
                    // you can choose whatever protocol you want. 
                    // For example, "https". 

                    Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
                        new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
                        {
                            {AssetDeliveryPolicyConfigurationKey.FairPlayLicenseAcquisitionUrl, acquisitionUrl.ToString().Replace("https://", "skd://")},
                            {AssetDeliveryPolicyConfigurationKey.CommonEncryptionIVForCbcs, configFP.ContentEncryptionIV}
                        };
        
                    var assetDeliveryPolicy = _context.AssetDeliveryPolicies.Create(
                            "AssetDeliveryPolicy",
                        AssetDeliveryPolicyType.DynamicCommonEncryptionCbcs,
                        AssetDeliveryProtocol.HLS,
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


##<a name="next-steps-media-services-learning-paths"></a>Järgmised toimingud: Media Servicesi õppeteemad

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Tagasiside saatmine

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
