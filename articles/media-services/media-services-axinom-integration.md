<properties 
    pageTitle="Axinom esitamisel Widevine litsentside Azure Media Servicesi kaudu | Microsoft Azure'i" 
    description="Selles artiklis kirjeldatakse, kuidas saate kasutada Azure Media Services (AMS) voo, mis on dünaamiliselt krüptitud AMS nii PlayReady ja Widevine digitaalõiguste esitamisel. PlayReady litsentsi pärineb Media Services PlayReady litsentsi serveri ja Widevine litsentsi on esitatud Axinom litsentsi serveri." 
    services="media-services" 
    documentationCenter="" 
    authors="willzhan" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"   
    ms.author="willzhan;Mingfeiy;rajputam;Juliako"/>

#<a name="using-axinom-to-deliver-widevine-licenses-to-azure-media-services"></a>Axinom esitamisel Widevine litsentside Azure Media Servicesi kaudu  

> [AZURE.SELECTOR]
- [castLabs](media-services-castlabs-integration.md)
- [Axinom](media-services-axinom-integration.md)

##<a name="overview"></a>Ülevaade

Azure Media Services (AMS) on lisatud Google Widevine dünaamilise kaitse (vt [Mingfei's ajaveebi](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/) üksikasjad). Lisaks Azure Media Playeri (AMP) on lisatud ka Widevine tugi (vt [AMP dokumendi](http://amp.azure.net/libs/amp/latest/docs/) üksikasjad). See on tänapäevane brauseritega MSE ja EME suuremaid saavutusi rakenduses streaming MÕTTEKRIIPSU sisu kaitstud CENC mitme-native-DRM (PlayReady ja Widevine).

Alustades Media Services .NET SDK versioon 3.5.2, Media Servicesi võimaldab teil konfigureerida Widevine litsentsi Mall ja saada Widevine litsentsid. Saate kasutada ka järgmised AMS partnerite aitavad esitamisel Widevine litsentside: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [castLabs](http://castlabs.com/company/partners/azure/).

Selles artiklis kirjeldatakse, kuidas integreerida ja testida Widevine litsentsi serveri haldab Axinom. Täpsemalt katab:  

- Dünaamiline levinud krüptimise seadistamine mitme-DRM (PlayReady ja Widevine) koos vastavate litsentsi omandamise URL-id;
- Genereerimine märgiks JWT täitmiseks litsentsi serveri nõuded;
- Arendamise Azure Media Playeri rakendus, mis tegeleb litsentsi omandamise JWT Turbeloa autentimise;

Täielik süsteem ja voo sisu klahv, võti ID, võtme seemne, JTW luba ja oma nõuete kõige paremini kirjeldada järgmisel joonisel.

![MÕTTEKRIIPSU ja CENC](./media/media-services-axinom-integration/media-services-axinom1.png)

##<a name="content-protection"></a>Sisu kaitsmine

Konfigureerida dünaamilise kaitse ja peamised poliitika, leiate Mingfei's Ajaveeb: [Kuidas konfigureerida Azure Media Servicesi Widevine pakendis](http://mingfeiy.com/how-to-configure-widevine-packaging-with-azure-media-services).

Saate konfigureerida dünaamilise CENC kaitse mitme-DRM KRIIPSJOONE streaming, millel on mõlemad järgmistest:

1. PlayReady kaitse MS Edge ja IE11, mis võiks olla Turbeloa autoriseerimine piirangud. Turbeloa piiratud poliitika tuleb lisada märgiks väljaandja järgi on turvaline Turbeloa teenus (STS), nt Azure Active Directory;
1. Widevine kaitse Chrome'i, selle välja teise STS luba koos nõuavad Turbeloa autentimist. 

[JWT Turbeloa genereerimine](media-services-axinom-integration.md#jwt-token-generation) leiate jaotisest miks ei saa Azure Active Directory kasutada ka STS Axinom's Widevine litsentsi server.

###<a name="considerations"></a>Kaalutlused

1. Kasutage funktsiooni Axinom määratud võtme seemne (8888000000000000000000000000000000000000) ja teie loodud või valitud võti ID, et luua sisu võti konfigureerida võtme teenus. Axinom litsentsi serveri välja kõik litsentsid sisaldava sisu klahvid põhjal on sama võtme puhul testimine ja tootmise jaoks.
1. Testimiseks Widevine litsentsi omandamise URL: [https://drm-widevine-licensing.axtest.net/AcquireLicense](https://drm-widevine-licensing.axtest.net/AcquireLicense). HTTP- ja HTTS on lubatud.

##<a name="azure-media-player-preparation"></a>Azure Media Playeri ettevalmistamine

AMP v1.4.0 toetab AMS sisu, mis on kaasas dünaamiliselt PlayReady nii Widevine DRM taasesitus.
Kui Widevine litsentsi server ei nõua Turbeloa autentimine, ei ole midagi täiendavad, peate tegema selleks, et testida KRIIPSJOONE Widevine kaitstud sisu. Näiteks AMP meeskond pakub lihtne [näide](http://amp.azure.net/libs/amp/latest/samples/dynamic_multiDRM_PlayReadyWidevine_notoken.html), kus näete seda serva ja IE11 koos PlayReady ja Chrome'is Widevine koos töötamine.
Esitatud Axinom Widevine litsentsi server nõuab JWT Turbeloa autentimist. Luba JWT tuleb esitada litsentsi taotlusega kaudu HTTP-päise "X-AxDRM-sõnum". Selleks tuleb kõigepealt lisada järgmised JavaScripti majutusteenuse AMP enne allika veebilehele:

    <script>AzureHtml5JS.KeySystem.WidevineCustomAuthorizationHeader = "X-AxDRM-Message"</script>

Ülejäänud AMP kood on standardne AMP API AMP dokument nagu [siin](http://amp.azure.net/libs/amp/latest/docs/).

Pange tähele, on endiselt ülaltoodud JavaScripti sätte Luba kohandatud päise lühiajaliselt lähenemine enne ametlik amp pikaajaline lähenemine on välja.

##<a name="jwt-token-generation"></a>JWT Turbeloa loomine

Axinom Widevine litsentsi server testimiseks nõuab JWT Turbeloa autentimist. Lisaks üks JWT luba nõuded on keerukas objekti tüübi asemel lihtsad andmetüüp.

Kahjuks Azure AD saate anda ainult JWT sõned lihtsad tüübid. Samuti .NET Framework API (System.IdentityModel.Tokens.SecurityTokenHandler ja JwtPayload) ainult võimaldab teil sisestada taotluste keerukate objekti tüüp. Siiski nõuded on endiselt seeriasertide stringina. Seetõttu ei saa kasutada mis tahes kahe JWT sõnet Widevine litsentsi koosolekukutse loomiseks.


John Sheehan [JWT Nugeti pakett](https://www.nuget.org/packages/JWT) vastab vajadustele nii, et me kasutada Nugeti pakett.

Allpool on vaja taotluste Axinom Widevine litsentsi serveri nõutud loomisel JWT luba kood testimiseks:

    
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using System.IdentityModel.Tokens;
    using System.IdentityModel.Protocols.WSTrust;
    using System.Security.Claims;
    
    namespace OpenIdConnectWeb.Utils
    {
        public class JwtUtils
        {
            //using John Sheehan's NuGet JWT library: https://www.nuget.org/packages/JWT/
            public static string CreateJwtSheehan(string symmetricKeyHex, string key_id)
            {
                byte[] symmetricKey = ConvertHexStringToByteArray(symmetricKeyHex);  //hex string to byte[] Note: Note that the key is a hex string, however it must be treated as a series of bytes not a string when encoding.
    
                var payload = new Dictionary<string, object>()
                             {
                                 { "version", 1 },
                                 { "com_key_id", System.Configuration.ConfigurationManager.AppSettings["ax:com_key_id"] },
                                 { "message", new { type = "entitlement_message", key_ids = new string[] { key_id } }  }
                             };
    
                string token = JWT.JsonWebToken.Encode(payload, symmetricKey, JWT.JwtHashAlgorithm.HS256);
    
                return token;
            }
    
            //convert hex string to byte[]
            public static byte[] ConvertHexStringToByteArray(string hexString)
            {
                if (hexString.Length % 2 != 0)
                {
                    throw new ArgumentException(String.Format(System.Globalization.CultureInfo.InvariantCulture, "The binary key cannot have an odd number of digits: {0}", hexString));
                }
    
                byte[] HexAsBytes = new byte[hexString.Length / 2];
                for (int index = 0; index < HexAsBytes.Length; index++)
                {
                    string byteValue = hexString.Substring(index * 2, 2);
                    HexAsBytes[index] = byte.Parse(byteValue, System.Globalization.NumberStyles.HexNumber, System.Globalization.CultureInfo.InvariantCulture);
                }
    
                return HexAsBytes;
            }
    
        }  
    
    }  

Axinom Widevine litsentsi server

    <add key="ax:laurl" value="http://drm-widevine-licensing.axtest.net/AcquireLicense" />
    <add key="ax:com_key_id" value="69e54088-e9e0-4530-8c1a-1eb6dcd0d14e" />
    <add key="ax:com_key" value="4861292d027e269791093327e62ceefdbea489a4c7e5a4974cc904b840fd7c0f" />
    <add key="ax:keyseed" value="8888000000000000000000000000000000000000" />

###<a name="considerations"></a>Kaalutlused

1.  Ehkki AMS PlayReady litsentsi teenus nõuab "esitaja =" eelneva mõne autentimise luba, Axinom Widevine litsentsi server ei kasuta seda.
2.  Axinom side võtit kasutatakse sisselogimise võti. Pöörake tähelepanu sellele võti hex stringi, kuid see käsitleda sarja baiti ei stringi kui kodeering. Selle meetodi ConvertHexStringToByteArray saavutamiseks.

##<a name="retrieving-key-id"></a>Võtme ID toomine

Olete ehk märganud, et loomisel on JWT kood Turbeloa, võti ID on nõutav. JWT Turbeloa tuleb enne laadimise AMP mängija, valmis võti vajab ID tuuakse loomiseks JWT luba.

Muidugi on mitu võimalust kätte võtme ID-d. Näiteks üks võivad talletada koos sisu metaandmete andmebaasi ID võti. Või saate tuua võti ID MÕTTEKRIIPSU MPD (Media esitluse kirjeldus) failist. Alljärgnev kood on viimasel.

    //get key_id from DASH MPD
    public static string GetKeyID(string dashUrl)
    {
        if (!dashUrl.EndsWith("(format=mpd-time-csf)"))
        {
            dashUrl += "(format=mpd-time-csf)";
        }
    
        XPathDocument objXPathDocument = new XPathDocument(dashUrl);
        XPathNavigator objXPathNavigator = objXPathDocument.CreateNavigator();
        XmlNamespaceManager objXmlNamespaceManager = new XmlNamespaceManager(objXPathNavigator.NameTable);
        objXmlNamespaceManager.AddNamespace("",     "urn:mpeg:dash:schema:mpd:2011");
        objXmlNamespaceManager.AddNamespace("ns1",  "urn:mpeg:dash:schema:mpd:2011");
        objXmlNamespaceManager.AddNamespace("cenc", "urn:mpeg:cenc:2013");
        objXmlNamespaceManager.AddNamespace("ms",   "urn:microsoft");
        objXmlNamespaceManager.AddNamespace("mspr", "urn:microsoft:playready");
        objXmlNamespaceManager.AddNamespace("xsi",  "http://www.w3.org/2001/XMLSchema-instance");
        objXmlNamespaceManager.PushScope();
    
        XPathNodeIterator objXPathNodeIterator;
        objXPathNodeIterator = objXPathNavigator.Select("//ns1:MPD/ns1:Period/ns1:AdaptationSet/ns1:ContentProtection[@value='cenc']", objXmlNamespaceManager);
    
        string key_id = string.Empty;
        if (objXPathNodeIterator.MoveNext())
        {
            key_id = objXPathNodeIterator.Current.GetAttribute("default_KID", "urn:mpeg:cenc:2013");
        }
    
        return key_id;
    }

##<a name="summary"></a>Kokkuvõte

Widevine toetavad nii Azure Media Services sisu kaitse ja Azure Media Playeri uusim, oleme rakendamiseks streaming MÕTTEKRIIPSU + mitme-native-DRM (PlayReady + Widevine) nii PlayReady litsentsi teenuse AMS ja Widevine litsentsi serveri kaudu Axinom tänapäevane järgmiste veebibrauserite jaoks:

- Chrome'i
- Microsoft Edge opsüsteemis Windows 10
- IE 11 opsüsteemis Windows 8.1 ja Windows 10
- Nii Firefox (töölauaversioon) ja Safari Macis (mitte iOS) on samuti toetatud Silverlighti ja Azure Media Player sama URL-i kaudu

Mini-lahenduse ärakasutamine Axinom Widevine litsentsi server on vaja järgmisi. Välja arvatud klahv ID ülejäänud parameetrid on esitatud Axinom vastavalt nende Widevine Serveri install.


Parameetri|Kuidas seda kasutatakse
---|---
Suhtlus võti ID|Peab olema lisatud taotluste "com_key_id" JWT luba väärtus (vt [jaotist](media-services-axinom-integration.md#jwt-token-generation) ).
Suhtlus võti|Tuleb kasutada JWT luba allkirjastamiseks võti (vt [jaotist](media-services-axinom-integration.md#jwt-token-generation) ).
Võtme seemne|Tuleb kasutada luua mis tahes antud sisuga sisu võti võti ID-d (vt [jaotist](media-services-axinom-integration.md#content-protection) ).
URL-i Widevine litsentsi saamine|Tuleb kasutada konfigureerida varade kohaletoimetamise poliitika KRIIPSJOONE streaming (vt [selles](media-services-axinom-integration.md#content-protection) jaotises).
Sisu võtme ID|Peab olema õigus sõnumi nõude JWT luba väärtus kaasatud (vt [jaotist](media-services-axinom-integration.md#jwt-token-generation) ). 


##<a name="media-services-learning-paths"></a>Media Servicesi Õppeteed

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Tagasiside saatmine

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

###<a name="acknowledgments"></a>Litsents 

Soovime kinnitage järgmised inimestele, kes selle dokumendi loomiseks kasutatud: Kristjan Jõgi, Axinom, Mingfei Yan ja Amit Rajput.
