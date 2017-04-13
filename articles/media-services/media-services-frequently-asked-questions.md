<properties 
    pageTitle="Korduma kippuvad küsimused | Microsoft Azure'i" 
    description="Korduma kippuvad küsimused (KKK)" 
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
    ms.date="09/19/2016" 
    ms.author="juliako"/>


#<a name="frequently-asked-questions"></a>Korduma kippuvad küsimused

##<a name="general-ams-faqs"></a>Üldine AMS KKK

Q: kuidas muudate indekseerimine?

V: reserveeritud üksused on sama kodeering ja indekseerimise tööülesanded. Järgige juhiseid [skaala kodeeringus reserveeritud üksuste](media-services-scale-media-processing-overview.md)kohta. **Teate** , et indekseerimise jõudlus ei mõjuta reserveeritud üksuse tüüp.

K: üles laaditud, kodeeritud ja avaldatud video. Mida oleks põhjus video esitamine taustal alla laadida ja selle katsel?

V: üks kõige levinumad põhjused on teil on vähemalt üks reserveeritud streaming ühiku eraldatud streaming lõpp-punkti, millest soovite taasesitus.  Järgige juhiseid [skaala Streaming reserveeritud üksuste](media-services-portal-scale-streaming-endpoints.md)kohta.

Q: saab teha kompostimise reaalajas voo kohta?

V: kompostimise kohta reaalajas voole on praegu pole saadaval Azure Media Servicesi, nii, et peate eelnevalt Koostage oma arvutis.

Q: kas kasutada Azure CDN Live Streaming?

V: Media Services toetab integreerimine Azure CDN-ID (Lisateavet leiate teemast [haldamine Streaming lõpp-punktid Media Services konto](media-services-portal-manage-streaming-endpoints.md)).  Saate kasutada Live streaming CDN. Azure Media Servicesi pakub tõrgeteta Streaming, HLS ja MPEG-KRIIPSJOONE väljundeid. Kõik nendes vormingutes kasutada HTTP andmeid ja saada vahemällu HTTP eelised. Klõpsake reaalajas streaming tegelik heli andmed on jaotatud fragmendid ja selle erinevate osade saada vahemällu CDN sisse. Ainult andmed tuleb sisestada värskendatud on manifest andmed. CDN värskendab perioodiliselt manifest andmeid.

Q: kas Azure Media Servicesi toetab salvestamist pildid?

V: kui otsite lihtsalt salvestada JPEG- või PNG-pilte, alles jätta neid Azure'i bloobimälu. Ei ole kasu, et suunamise Media Servicesi konto juhul, kui soovite hoida neid teie Video või heli varad seotud. Või kui peate võib-olla vaja kasutada video kodeerija ülekatete pildid. Media kodeerija Standard toetab overlaying piltide, videote peale, ja see on, mis loetleb JPEG ja PNG nagu toetatud sisestusmeetodi vormingud. Lisateabe saamiseks vt [Ülekatete loomine](media-services-custom-mes-presets-with-dotnet.md#overlay).

Q: kuidas saate kopeerida varad Media Servicesi ühelt kontolt teisele.

V: kopeerimiseks varad Media Servicesi ühelt kontolt teisele kasutades .net-i, kasutage [IAsset.Copy](https://github.com/Azure/azure-sdk-for-media-services-extensions/blob/dev/MediaServices.Client.Extensions/IAssetExtensions.cs#L354) laiendamine meetod, mis on saadaval [Azure Media Services .NET SDK laiendid](https://github.com/Azure/azure-sdk-for-media-services-extensions/) hoidla. Lisateavet leiate teemast [lõime Foorum](https://social.msdn.microsoft.com/Forums/azure/28912d5d-6733-41c1-b27d-5d5dff2695ca/migrate-media-services-across-subscription?forum=MediaServices) .

Q: mis on toetatud märgid failide nimetamiseks, kui te töötate AMS?

V: Media Services kasutab IAssetFile.Name atribuudi väärtus, kui hoone URL-ide streaming sisu (nt http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) Seetõttu protsentkodeering pole lubatud. **Nime** atribuudi väärtus ei tohi olla järgmised [protsendi-kodeeringus-reserveeritud märgid](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters): !*'();:@&=+$,/?%#[]". Samuti ei saa ühte "." jaoks failinimelaiend.


Q: kuidas ühenduse loomiseks kasutada ülejäänud?

V: pärast ühenduse loomist https://media.windows.net, saate määrata mõne muu Media Services URI 301 uuesti. Te peate järgmise kõned uue URI, nagu on kirjeldatud [Media Servicesi kaudu REST API -ga ühenduse loomine](media-services-rest-connect-programmatically.md). 


Q: kuidas saate video kodeerimise käigus pöörata.

V: [Media kodeerija Standard](media-services-dotnet-encode-with-media-encoder-standard.md) toetab nurgad pöördenurka 90/180/270. Vaikekäitumine on "Automaatne", kui püüab tuvasta pööre metaandmete sissetulevate MP4/MOV fail ja klõpsake seda. Järgmised järgmistest **allikatest** elemendi ühele json valmiskombinatsioonid määratletud [siin](http://msdn.microsoft.com/library/azure/mt269960.aspx).
    
    "Version": 1.0,
    "Sources": [
    {
      "Streams": [],
      "Filters": {
        "Rotation": "90"
      }
    }
    ],
    "Codecs": [
    
    ...




##<a name="media-services-learning-paths"></a>Media Servicesi Õppeteed

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Tagasiside saatmine

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
