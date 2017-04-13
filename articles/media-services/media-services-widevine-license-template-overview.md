<properties 
    pageTitle="Widevine litsentsi Mall ülevaade | Microsoft Azure'i" 
    description="Selles teemas antakse ülevaade Widevine litsentsi malli saab konfigureerida Widevine litsentsid." 
    authors="juliako" 
    manager="erikre" 
    editor="" 
    services="media-services" 
    documentationCenter=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"  
    ms.author="juliako"/>

#<a name="widevine-license-template-overview"></a>Widevine litsentsi Mall ülevaade

##<a name="overview"></a>Ülevaade

Azure Media Servicesi võimaldab nüüd Widevine litsentside taotlemine ja konfigureerimiseks. Kui lõppkasutaja mängija proovib Widevine kaitstud sisu esitada, saadetakse taotluse litsentsi teenus hankida litsentsi. Kui teenuse litsentsi kinnitab taotluse, probleemid selle litsentsi, mis on saadetud kliendi ja dekrüptida ja esitada määratud sisu saab kasutada.

Widevine litsentsi taotlus on vormindatud JSON-sõnumina.  

Pange tähele, et saate luua tühja meilisõnumi ei sisalda väärtusi lihtsalt "{}" litsentsi malli luuakse vaikimisi kõigi.  

    {  
       “payload”:“<license challenge>”,
       “content_id”: “<content id>” 
       “provider”: ”<provider>”
       “allowed_track_types”:“<types>”,
       “content_key_specs”:[  
          {  
             “track_type”:“<track type 1>”
          },
          {  
             “track_type”:“<track type 2>”
          },
          …
       ],
       “policy_overrides”:{  
          “can_play”:<can play>,
          “can persist”:<can persist>,
          “can_renew”:<can renew>,
          “rental_duration_seconds”:<rental duration>,
          “playback_duration_seconds”:<playback duration>,
          “license_duration_seconds”:<license duration>,
          “renewal_recovery_duration_seconds”:<renewal recovery duration>,
          “renewal_server_url”:”<renewal server url>”,
          “renewal_delay_seconds”:<renewal delay>,
          “renewal_retry_interval_seconds”:<renewal retry interval>,
          “renew_with_usage”:<renew with usage>
       }
    }

##<a name="json-message"></a>JSON sõnum

Nimi | Väärtus | Kirjeldus
---|---|---
last |Base64-kodeeringuga stringi. |Kliendi poolt saadetud litsentsi taotlus. 
content_id | Base64-kodeeringuga stringi.|Identifikaator tuletada KeyId(s) ja sisu klahvid iga content_key_specs.track_type jaoks kasutada.
pakkuja |string |Sisu võtmed ja -poliitikad otsimiseks kasutada. Nõutav.
policy_name | string |Varem registreeritud poliitika nimi. Valikuline.
allowed_track_types | loetelu  | SD_ONLY või SD_HD. Klahvid sisu, mis tuleks lisada litsentsi
content_key_specs | massiivi JSON struktuurid, lugege teemat **Sisu klahvi andmed** | Tabeliruudustikku tekstuuriga juhtelemendi sisu nooleklahvid tagastamiseks. Üksikasjad leiate sisu võti Spec all.  Ainult üks allowed_track_types ja content_key_specs saab määrata. 
use_policy_overrides_exclusively | kahendmuutuja. tõene või väär | Kasutage policy_overrides määratud rühmapoliitika atribuutide ja kõik varem salvestatud poliitika argument.
policy_overrides | JSON struktureerimine, **Alistab poliitika** allpool näha | Poliitikasätted selle litsentsi.  Juhul, kui see vara on eelmääratletud poliitika, kasutatakse neid määratud väärtused. 
session_init | JSON struktureerimine leiate teemast **Seansi käivitamine** | Valikuline andmete edasi litsentsi.
parse_only | kahendmuutuja. tõene või väär | Litsentsi taotluse sõeluda kuid see pole litsentsi. Siiski väärtuste vormi litsentsi taotluse tagastatud vastus.  

##<a name="content-key-specs"></a>Sisu võtme andmed 

Kui olemasoleva poliitika olemas, ei ole vaja määrata mis tahes väärtused sisu võti Spec.  Selle sisutüübiga seotud olemasoleva poliitika kasutatakse väljundi kaitse nagu HDCP ja CGMS määratlemiseks.  Kui olemasoleva poliitika Widevine litsentsi serveriga registreerimata sisu pakkuja saate annavad väärtused litsentsi taotluse.   


Iga content_key_specs peab olema määratud kõik rajad, olenemata suvand use_policy_overrides_exclusively. 


Nimi | Väärtus | Kirjeldus
---|---|---
content_key_specs. track_type | string | Jälita tippige nimi. Kui content_key_specs on määratud litsents, veenduge, et kõik jälgida tüüpi selgesõnaliselt määramiseks. Selleks tulemuseks tõrge taasesitus viimase 10 sekundit. 
content_key_specs  <br/> security_level | UInt32 | Määratleb kliendi töökindluse nõuded taasesitus. <br/> 1 - tarkvara-põhiste whitebox crypto on nõutav. <br/> 2 - tarkvara crypto ja on muudetud kodeerimise on nõutav. <br/> 3-võtit ja krüpto toimingud tuleb teha riistvara varundatud usaldusväärse täitmise keskkonnas. <br/> 4-crypto ja dekodeerimise sisu tuleb teha riistvara varundatud usaldusväärse täitmise keskkonnas.  <br/> 5-krüpto, dekodeerimise ja kõik töötlemine (tihendada või) meediumi tuleb käsitleda riistvara varundatud usaldusväärse täitmise keskkonnas.  
content_key_specs <br/> required_output_protection.HDC | tekstistringi – üks: HDCP_NONE, HDCP_V1, HDCP_V2 | Näitab, kas HDCP on vaja
content_key_specs <br/>klahv | Base64 <br/>encoded string|Sisu nuppu Jälita seda kasutada. Kui määratud, on vajalik track_type või key_id.  See suvand võimaldab sisu pakkuja sisestab sisu võti selle asemel et Widevine litsentsi serveri luua või otsingul klahvi jälitus.
content_key_specs.key_id| Base64 kodeeringuga stringi kahendarvu, 16 baiti | Võti ainuidentifikaator. 


##<a name="policy-overrides"></a>Poliitika alistab 

Nimi | Väärtus | Kirjeldus
---|---|---
policy_overrides. can_play | kahendmuutuja. tõene või väär | Näitab, et taasesitus sisu on lubatud. Vaikeväärtus on false.
policy_overrides. can_persist | kahendmuutuja. tõene või väär |Näitab, et litsentsi võib sunnita vajutanud mäluruumi ühenduseta kasutamiseks. Vaikeväärtus on false.
policy_overrides. can_renew | kahendmuutujaga tõene või väär |Näitab, et selle litsentsi pikendamise on lubatud. Kui väärtus on true, saate pikendada kestuse litsentsi südamelöögi ajal. Vaikeväärtus on false. 
policy_overrides. license_duration_seconds | Int64 | Näitab ajaakna selles teatud litsentsi. Väärtus 0 näitab, et on mingeid piiranguid kestus. Vaikimisi on 0. 
policy_overrides. rental_duration_seconds | Int64 | Näitab aknas ajal taasesitus on lubatud. Väärtus 0 näitab, et on mingeid piiranguid kestus. Vaikimisi on 0. 
policy_overrides. playback_duration_seconds | Int64 | Aja pärast taasesitus algab jooksul litsentsi aknast. Väärtus 0 näitab, et on mingeid piiranguid kestus. Vaikimisi on 0. 
policy_overrides. renewal_server_url |string | Kõik selle litsentsi südamelöögi ajal (uuendamine) taotlused suunatakse määratud URL. Väli kasutatakse ainult siis, kui can_renew on täidetud.
policy_overrides. renewal_delay_seconds |Int64 |Mitu sekundit pärast license_start_time, enne kui uuendamine on esimene proovitakse uuesti. Väli kasutatakse ainult siis, kui can_renew on täidetud. Vaikimisi on 0 
policy_overrides. renewal_retry_interval_seconds | Int64 | Saate määrata viivituse edaspidised litsentsi pikendamise taotluste ajalootabelis vahel sekundit. Väli kasutatakse ainult siis, kui can_renew on täidetud. 
policy_overrides. renewal_recovery_duration_seconds | Int64 | Aken, kus on lubatud taasesituse jätkamiseks uuendamise ajal aeg on proovitud, veel õnnestu tõttu kirjutamata probleeme serveri litsentsi. Väärtus 0 näitab, et on mingeid piiranguid kestus. Väli kasutatakse ainult siis, kui can_renew on täidetud.
policy_overrides. renew_with_usage | kahendmuutujaga tõene või väär |Näitab litsentsi saadetakse pikendada kasutamise alustamisel. Väli kasutatakse ainult siis, kui can_renew on täidetud. 

##<a name="session-initialization"></a>Seansi käivitamine

Nimi | Väärtus | Kirjeldus
---|---|---
provider_session_token | Base64-kodeeringuga stringi. |Selle seansi luba edastatakse tagasi litsentsi ja on olemas edaspidised pikendamine.  Seansi luba ei ole kestab kauem seansid. 
provider_client_token | Base64-kodeeringuga stringi. | Kliendi luba uuesti sisse litsentsi vastuse saata.  Kui litsentsi taotlus sisaldab kliendi luba, ignoreeritakse seda väärtust. Kliendi luba kuvatakse kestab kauem litsentsi seansid.
override_provider_client_token | kahendmuutuja. tõene või väär |Kui false ja litsentsi taotluse sisaldab kliendi luba, kasutada luba kaudu taotluse isegi juhul, kui määratud kliendi luba selle struktuuri.  Kui väärtus on true, kasutage alati luba määratud selle struktuuri.

##<a name="configure-your-widevine-licenses-using-net-types"></a>Konfigureerige oma Widevine litsentside kasutades .net-i andmetüübid

Media Services pakub .net-i API-d, mille abil saate konfigureerida oma Widevine litsentsid. 

###<a name="classes-as-defined-in-the-media-services-net-sdk"></a>Klassid on määratletud Media Services .NET SDK

Järgmist tüüpi määratlused on järgmised.

    public class WidevineMessage
    {
        public WidevineMessage();
    
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public AllowedTrackTypes? allowed_track_types { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public ContentKeySpecs[] content_key_specs { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public object policy_overrides { get; set; }
    }

    [JsonConverter(typeof(StringEnumConverter))]
    public enum AllowedTrackTypes
    {
        SD_ONLY = 0,
        SD_HD = 1
    }
    public class ContentKeySpecs
    {
        public ContentKeySpecs();

        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public string key_id { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public RequiredOutputProtection required_output_protection { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public int? security_level { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public string track_type { get; set; }
    }

    public class RequiredOutputProtection
    {
        public RequiredOutputProtection();

        public Hdcp hdcp { get; set; }
    }

    [JsonConverter(typeof(StringEnumConverter))]
    public enum Hdcp
    {
        HDCP_NONE = 0,
        HDCP_V1 = 1,
        HDCP_V2 = 2
    }

###<a name="example"></a>Näide

Järgmises näites kujutatakse .net-i API-de abil saate konfigureerida lihtsa Widevine litsentsi.

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


##<a name="media-services-learning-paths"></a>Media Servicesi Õppeteed

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Tagasiside saatmine

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


##<a name="see-also"></a>Vt ka

[PlayReady ja/või Widevine dünaamiline levinud krüptimise abil](media-services-protect-with-drm.md)
