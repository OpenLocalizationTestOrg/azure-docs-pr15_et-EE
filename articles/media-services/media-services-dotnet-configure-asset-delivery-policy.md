<properties 
    pageTitle="Varade kohaletoimetamise poliitikate konfigureerimine .NET SDK | Microsoft Azure'i" 
    description="Selles teemas kirjeldatakse, kuidas konfigureerida erinevate varade kohaletoimetamise poliitikate Azure Media Services .NET SDK." 
    services="media-services" 
    documentationCenter="" 
    authors="Mingfeiy" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="09/19/2016"
    ms.author="juliako;mingfeiy"/>

#<a name="configure-asset-delivery-policies-with-net-sdk"></a>.NET SDK varade kohaletoimetamise poliitikate konfigureerimine
[AZURE.INCLUDE [media-services-selector-asset-delivery-policy](../../includes/media-services-selector-asset-delivery-policy.md)]

##<a name="overview"></a>Ülevaade

Kui plaanite kohaletoimetamise krüptitud varad, üks Media Servicesi sisu kohaletoimetamise töövoo juhised konfigureerib kohaletoimetamise poliitika varad. Varade kohaletoimetamise poliitika ütleb Media Servicesi, kuidas soovite oma vara kohaletoimetamiseks: üheks mis streaming protokoll peaks teie vara olema dünaamiliselt pakitud (näiteks MPEG MÕTTEKRIIPSU, HLS, sujuva voogesituse või kõik), kas soovite oma varade dünaamiliselt krüptimiseks ja kuidas (ümbriku või levinud krüptimise).

Selles teemas käsitletakse miks ja kuidas luua ja varade kohaletoimetamise poliitikate konfigureerimine.

>[AZURE.NOTE]Saama kasutada dünaamilise pakendit ja dünaamiline krüptimist, tuleb teil teha kindel, et on vähemalt üks skaala ühik (tuntud ka kui streaming üksus). Lisateavet leiate teemast [skaala meediumid teenus](media-services-portal-manage-streaming-endpoints.md).
>
>Lisaks teie vara peab sisaldama kohandatava bitrate MP4s või kohandatava bitrate sujuva voogesituse failide kogum.

Erinevate poliitikate võivad rakendada sama vara. Näiteks võib rakendada sujuva voogesituse ja AES ümbriku krüptimise MPEG MÕTTEKRIIPSU ja HLS PlayReady krüptimine. Mis tahes protokollid, mis on määratletud kohaletoimetamise poliitika (näiteks saate lisada ühe poliitika, mis määrab ainult HLS protokoll) streaming blokeeritud. Erandiks on, kui teil pole üldse määratud varade kohaletoimetamise poliitika. Klõpsake kõigi Protokollid lubatud selge.

Kui soovite pakkuda salvestusruumi krüptitud varade, tuleb konfigureerida vara kohaletoimetamise poliitika. Enne oma varade saate voona, streaming server eemaldab salvestusruumi krüptimise ja andmevoogu abil määratud kohaletoimetamise poliitika sisu. Näiteks teie vara krüptitud Advanced Standard (AES) ümbriku krüptovõtme osutamiseks seatud poliitika tüüp **DynamicEnvelopeEncryption**. Salvestusruumi krüptimise eemaldamine ja voona varade selge, tippige poliitika määramine **NoDynamicEncryption**. Näiteid, kuidas konfigureerida järgmist tüüpi poliitika jälgimine.

Sõltuvalt varade kohaletoimetamise poliitika konfigureerimise teil oleks võimalik dünaamiliselt paketti, dünaamiliselt krüptimiseks ja taustal alla laadida järgmistest streaming protokollid: sujuva voogesituse HLS, MPEG MÕTTEKRIIPSU ja HDS.

Järgmises loendis on näidatud vormingud abil sujuv, HLS, MÕTTEKRIIPSU ja HDS.

Sujuva voogesituse:

{streaming lõpp-punkti nimi-media Servicesi konto name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest

HLS:

{streaming lõpp-punkti nimi-media Servicesi konto name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

MPEG MÕTTEKRIIPSU

{streaming lõpp-punkti nimi-media Servicesi konto name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

HDS

{streaming lõpp-punkti nimi-media Servicesi konto name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=f4m-f4f)

Juhised selle kohta, kuidas avaldada vara ja koostada streaming URL-i, lugege teemat [koostada streaming URL](media-services-deliver-streaming-content.md).

##<a name="considerations"></a>Kaalutlused

- Te ei saa kustutada ka AssetDeliveryPolicy seotud vara ajal kuvatakse nõudmisel (streaming) locator olemas vara. Soovitus on enne kustutamist poliitika vara poliitika eemaldamine.
- Streaming locator ei saa luua salvestusruumi krüptitud varade kui vara kohaletoimetamise poliitika on seatud.  Kui vara pole krüptitud salvestusruumi, süsteem võimaldab teil luua mõne locator ja voona varade selge ilma varade kohaletoimetamise poliitika.
- Teil võib olla mitu varade kohaletoimetamise poliitika, mis on seotud ühe varade, kuid saate määrata ainult üks viis käsitlema antud AssetDeliveryProtocol.  See tähendab, kui proovite kahe kohaletoimetamise poliitikate määratud AssetDeliveryProtocol.SmoothStreaming protokolli, mille tulemuseks on tõrge, kuna süsteemi ei tea, kus üks, milles soovite neid rakendada, kui mõnda muud klienti taotleb sujuva voogesituse link.
- Kui teil on mõne olemasoleva streaming locator vara, linkida ei saa uue poliitika vara (saate olemasoleva poliitika kaudu vara linkimise tühistamine, või värskendada kohaletoimetamise poliitika vara).  Peate esmalt eemaldamine streaming locator, muuta poliitikaid ja seejärel uuesti luua streaming locator.  Kui streaming locator uuesti luua, kuid veenduge, mis ei põhjustada probleeme klientidele, kuna sisu saab kopeerida päritolu või järgmise etapi CDN-ID, saate kasutada sama locatorId.


##<a name="clear-asset-delivery-policy"></a>Eemalda varade kohaletoimetamise poliitika

Järgmised **ConfigureClearAssetDeliveryPolicy** meetodit saate määrata, ei kehti dünaamiline krüptimise ja pakkuda voo mis tahes järgmised protokollid: MPEG MÕTTEKRIIPSU, HLS ja sujuva voogesituse protokollid. Võite oma varasid salvestusruumi krüptitud rakendama.

Teavet Millised väärtused saate määrata ka AssetDeliveryPolicy loomisel, jaotisest [kasutada AssetDeliveryPolicy määratlemisel](#types) .

staatilise avaliku kehtetu ConfigureClearAssetDeliveryPolicy (IAsset vara) {IAssetDeliveryPolicy poliitika = _context. AssetDeliveryPolicies.Create ("Selge poliitika" AssetDeliveryPolicyType.NoDynamicEncryption, AssetDeliveryProtocol.HLS | AssetDeliveryProtocol.SmoothStreaming | AssetDeliveryProtocol.Dash, null);

varade. DeliveryPolicies.Add(policy); }

##<a name="dynamiccommonencryption-asset-delivery-policy"></a>DynamicCommonEncryption varade kohaletoimetamise poliitika


**CreateAssetDeliveryPolicy** järgmisel viisil loob **AssetDeliveryPolicy** , mis on konfigureeritud dünaamiline levinud krüptimise (**DynamicCommonEncryption**) rakendamiseks sujuva voogesituse protokolli (muud Protokollid blokeeritud streaming). Meetodi kulub kaks parameetrid: **varade** (varade, millele soovite rakendada poliitika kohaletoimetamise) ja **IContentKey** (sisu võti **CommonEncryption** tüüp, leiate lisateavet teemast: [loomise sisu klahvi](media-services-dotnet-create-contentkey.md#common_contentkey)).

Teavet Millised väärtused saate määrata ka AssetDeliveryPolicy loomisel, jaotisest [kasutada AssetDeliveryPolicy määratlemisel](#types) .


staatilise avaliku kehtetu CreateAssetDeliveryPolicy (IAsset vara, IContentKey key) {Uri acquisitionUrl = võti. GetKeyDeliveryUrl(ContentKeyDeliveryType.PlayReadyLicense);

Sõnastiku < AssetDeliveryPolicyConfigurationKey, stringi > assetDeliveryPolicyConfiguration uue sõnastiku < AssetDeliveryPolicyConfigurationKey stringi > = {{AssetDeliveryPolicyConfigurationKey.PlayReadyLicenseAcquisitionUrl, acquisitionUrl.ToString()}};

        var assetDeliveryPolicy = _context.AssetDeliveryPolicies.Create(
                "AssetDeliveryPolicy",
            AssetDeliveryPolicyType.DynamicCommonEncryption,
            AssetDeliveryProtocol.SmoothStreaming,
            assetDeliveryPolicyConfiguration);

        // Add AssetDelivery Policy to the asset
        asset.DeliveryPolicies.Add(assetDeliveryPolicy);

        Console.WriteLine();
        Console.WriteLine("Adding Asset Delivery Policy: " +
            assetDeliveryPolicy.AssetDeliveryPolicyType);
    }

Azure Media Servicesi võimaldab teil Widevine krüptimise lisamiseks. Järgmises näites näitab PlayReady nii Widevine varade kohaletoimetamise poliitika lisamist.


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
            {AssetDeliveryPolicyConfigurationKey.WidevineLicenseAcquisitionUrl, widevineUrl.ToString()}

        };

        var assetDeliveryPolicy = _context.AssetDeliveryPolicies.Create(
                "AssetDeliveryPolicy",
            AssetDeliveryPolicyType.DynamicCommonEncryption,
            AssetDeliveryProtocol.Dash,
            assetDeliveryPolicyConfiguration);


        // Add AssetDelivery Policy to the asset
        asset.DeliveryPolicies.Add(assetDeliveryPolicy);

    }

>[AZURE.NOTE]Krüptimisel Widevine, saate vaid saaks esitamisel KRIIPSJOONE abil. Veenduge, et määrata MÕTTEKRIIPSU varade kohaletoimetamise protokolli.


##<a name="dynamicenvelopeencryption-asset-delivery-policy"></a>DynamicEnvelopeEncryption varade kohaletoimetamise poliitika 

**CreateAssetDeliveryPolicy** järgmisel viisil loob **AssetDeliveryPolicy** , mis on konfigureeritud dünaamilise ümbriku krüptimise (**DynamicEnvelopeEncryption**) rakendamiseks sujuva voogesituse, HLS ja MÕTTEKRIIPSU Protokollid (kui te ei soovi teatud protokollid ei määra, nad blokeeritud streaming). Meetodi kulub kaks parameetrid: **varade** (varade, millele soovite rakendada poliitika kohaletoimetamise) ja **IContentKey** (sisu võti **EnvelopeEncryption** tüüp, leiate lisateavet teemast: [loomise sisu klahvi](media-services-dotnet-create-contentkey.md#envelope_contentkey)).


Teavet Millised väärtused saate määrata ka AssetDeliveryPolicy loomisel, jaotisest [kasutada AssetDeliveryPolicy määratlemisel](#types) .   

    private static void CreateAssetDeliveryPolicy(IAsset asset, IContentKey key)
    {
        
        //  Get the Key Delivery Base Url by removing the Query parameter.  The Dynamic Encryption service will
        //  automatically add the correct key identifier to the url when it generates the Envelope encrypted content
        //  manifest.  Omitting the IV will also cause the Dynamice Encryption service to generate a deterministic
        //  IV for the content automatically.  By using the EnvelopeBaseKeyAcquisitionUrl and omitting the IV, this
        //  allows the AssetDelivery policy to be reused by more than one asset.
        //
        Uri keyAcquisitionUri = key.GetKeyDeliveryUrl(ContentKeyDeliveryType.BaselineHttp);
        UriBuilder uriBuilder = new UriBuilder(keyAcquisitionUri);
        uriBuilder.Query = String.Empty;
        keyAcquisitionUri = uriBuilder.Uri;

        // The following policy configuration specifies: 
        //   key url that will have KID=<Guid> appended to the envelope and
        //   the Initialization Vector (IV) to use for the envelope encryption.
        Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
            new Dictionary<AssetDeliveryPolicyConfigurationKey, string> 
        {
            {AssetDeliveryPolicyConfigurationKey.EnvelopeBaseKeyAcquisitionUrl, keyAcquisitionUri.ToString()},
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
        Console.WriteLine("Adding Asset Delivery Policy: " + assetDeliveryPolicy.AssetDeliveryPolicyType);
    }


##<a id="types"></a>Kasutatud AssetDeliveryPolicy määratlemisel tüübid

###<a id="AssetDeliveryProtocol"></a>AssetDeliveryProtocol 

    /// <summary>
    /// Delivery protocol for an asset delivery policy.
    /// </summary>
    [Flags]
    public enum AssetDeliveryProtocol
    {
        /// <summary>
        /// No protocols.
        /// </summary>
        None = 0x0,

        /// <summary>
        /// Smooth streaming protocol.
        /// </summary>
        SmoothStreaming = 0x1,

        /// <summary>
        /// MPEG Dynamic Adaptive Streaming over HTTP (DASH)
        /// </summary>
        Dash = 0x2,

        /// <summary>
        /// Apple HTTP Live Streaming protocol.
        /// </summary>
        HLS = 0x4,

        /// <summary>
        /// Adobe HTTP Dynamic Streaming (HDS)
        /// </summary>
        Hds = 0x8,

        /// <summary>
        /// Include all protocols.
        /// </summary>
        All = 0xFFFF
    }

###<a id="AssetDeliveryPolicyType"></a>AssetDeliveryPolicyType

    /// <summary>
    /// Policy type for dynamic encryption of assets.
    /// </summary>
    public enum AssetDeliveryPolicyType
    {
        /// <summary>
        /// Delivery Policy Type not set.  An invalid value.
        /// </summary>
        None,

        /// <summary>
        /// The Asset should not be delivered via this AssetDeliveryProtocol. 
        /// </summary>
        Blocked, 

        /// <summary>
        /// Do not apply dynamic encryption to the asset.
        /// </summary>
        /// 
        NoDynamicEncryption,  

        /// <summary>
        /// Apply Dynamic Envelope encryption.
        /// </summary>
        DynamicEnvelopeEncryption,

        /// <summary>
        /// Apply Dynamic Common encryption.
        /// </summary>
        DynamicCommonEncryption
    }

###<a id="ContentKeyDeliveryType"></a>ContentKeyDeliveryType

    /// <summary>
    /// Delivery method of the content key to the client.
    /// </summary>
    public enum ContentKeyDeliveryType
    {
        /// <summary>
        /// None.
        /// </summary>
        None = 0,

        /// <summary>
        /// Use PlayReady License acquistion protocol
        /// </summary>
        PlayReadyLicense = 1,

        /// <summary>
        /// Use MPEG Baseline HTTP key protocol.
        /// </summary>
        BaselineHttp = 2,

        /// <summary>
        /// Use Widevine License acquistion protocol
        ///
        </summary>
        Widevine = 3

    }

###<a id="AssetDeliveryPolicyConfigurationKey"></a>AssetDeliveryPolicyConfigurationKey

    /// <summary>
    /// Keys used to get specific configuration for an asset delivery policy.
    /// </summary>
    public enum AssetDeliveryPolicyConfigurationKey
    {
        /// <summary>
        /// No policies.
        /// </summary>
        None,

        /// <summary>
        /// Exact Envelope key URL.
        /// </summary>
        EnvelopeKeyAcquisitionUrl,

        /// <summary>
        /// Base key url that will have KID=<Guid> appended for Envelope.
        /// </summary>
        EnvelopeBaseKeyAcquisitionUrl,
        
        /// <summary>
        /// The initialization vector to use for envelope encryption in Base64 format.
        /// </summary>
        EnvelopeEncryptionIVAsBase64,

        /// <summary>
        /// The PlayReady License Acquisition Url to use for common encryption.
        /// </summary>
        PlayReadyLicenseAcquisitionUrl,

        /// <summary>
        /// The PlayReady Custom Attributes to add to the PlayReady Content Header
        /// </summary>
        PlayReadyCustomAttributes,

        /// <summary>
        /// The initialization vector to use for envelope encryption.
        /// </summary>
        EnvelopeEncryptionIV,

        /// <summary>
        /// Widevine DRM acquisition url
        /// </summary>
        WidevineLicenseAcquisitionUrl
    }

##<a name="media-services-learning-paths"></a>Media Servicesi Õppeteed

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Tagasiside saatmine

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


