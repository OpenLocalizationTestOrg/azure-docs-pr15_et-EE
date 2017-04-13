<properties 
    pageTitle="Media Servicesi vabastage märkmete | Microsoft Azure'i" 
    description="Media Services väljalaskemärkmed" 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="media" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="09/19/2016"
    ms.author="juliako"/>

# <a name="azure-media-services-release-notes"></a>Azure Media Servicesi vabastage märkmed

Nende väljalaskemärkmed summarize muudatused varasemates versioonides ja teadaolevad probleemid.

>[AZURE.NOTE] Soovime kuulda meie klientide ja probleemide lahendamine, mis mõjutavad saate keskenduda. Probleemist või küsimusi, saatke [Azure Media Services MSDN-i Foorum].


##<a id="issues"></a>Praegu teadaolevad probleemid

### <a id="general_issues"></a>Media Services üldised küsimused

Probleem|Kirjeldus
---|---
Mitme levinud HTTP päised ei esitata REST API.|Kui teil tekib Media Servicesi rakenduste abil REST API-ga, leiate, et mõned levinud HTTP päise välju (klient-taotlus-ID, sh taotlus-ID ja tagasi kliendi taotluse ID) ei toetata. Päiste lisatakse tulevikus värskendust.
Protsentkodeering pole lubatud.|Media Services kasutab IAssetFile.Name atribuudi väärtus, kui hoone URL-ide streaming sisu (nt http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) Seetõttu protsentkodeering pole lubatud. **Nime** atribuudi väärtus ei tohi olla järgmised [protsendi-kodeeringus-reserveeritud märgid](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters): !*'();:@&=+$,/?%#[]". Samuti ei saa ühte "." jaoks failinimelaiend.
ListBlobs meetod, mis on osa Azure'i salvestusruumi SDK versioon 3.x nurjub.|Media Servicesi loob SAS URL-ide [2012-02-12](http://msdn.microsoft.com/library/azure/dn592123.aspx) versiooni. Kui soovite kasutada Azure salvestusruumi SDK loendi plekid bloobimälu ümbrises, kasutage [CloudBlobContainer.ListBlobs](http://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.cloudblobcontainer.listblobs.aspx) meetod, mis on osa Azure'i salvestusruumi SDK versioon 2.x. ListBlobs meetod, mis on osa Azure'i salvestusruumi SDK versioon 3.x nurjub.
Media Servicesi pidurdamise süsteem piirab ressursikasutuse rakenduste teenust liigne taotlus tehtud. Teenuse võib tagastada teenus pole saadaval (503) HTTP olekukoodi.|Lisateabe saamiseks vt 503 HTTP olekukoodi [Azure Media Services tõrkekoodid](http://msdn.microsoft.com/library/azure/dn168949.aspx) teemas kirjeldust.
Kui päringu üksused, on piirang 1000 üksuste tagasi korraga, sest avaliku ülejäänud v2 piirab päringutulemite 1000 tulemusi. | Peate kasutama **Jäta** ja **võtta** (.NET) / [.net-i näites](media-services-dotnet-manage-entities.md#enumerating-through-large-collections-of-entities) ja [REST API näites](media-services-rest-manage-entities.md#enumerating-through-large-collections-of-entities)kirjeldatud **top** (REST) nimega. 
Korrake sildi probleemi sujuva voogesituse manifestis kohata osa kliente.|[Lisateavet leiate jaotisest.](media-services-deliver-content-overview.md#known-issues)
Azure Media Services .NET SDK objekte ei saa ja seetõttu ei tööta Azure vahemällu.|Kui proovite serialiseerida SDK AssetCollection objekti lisamine Azure'i vahemällu, erandi Expression.error.
Kodeering tööde haldamine nurjuda sõnumi stringiga "etapp: DownloadFile. Kood: System.NullReferenceException ".|Tüüpilised kodeering töövoog on Sisestuskeel vara Sisestuskeel video failid üles laadida ja esitage ühe või mitu kodeering tööd Sisestuskeel vara, muutmata täpsemaks et vara. Siiski, kui muudate Sisestuskeel varade (nt, kustutamine, lisades/ümbernimetamine failidele juurde vara), siis edasiste tööde haldamine ei pruugi DownloadFile tõrge. Lahendus on Sisestuskeel varade kustutamine ja uuesti üles laadida uut varade Sisestuskeel fail(ID). 

##<a id="rest_version_history"></a>Versiooniajalugu REST API-ga

Media Services REST API versiooniajaloo kohta leiate teemast [Azure Media Services REST API viide].

##<a id="july_changes16"></a>Juuli 2016 väljaanne

###<a name="updates-to-manifest-file-ism-generated-by-encoding-tasks"></a>Värskenduste näidata fail (*. ISM) loodud kodeerimine tööülesanded

Toimingu kodeering esitamisel Media kodeerija Standard-või Azure Media Encoderi kodeering tööülesande loob [streaming manifestifaili](media-services-deliver-content-overview.md) (* .ism) faili väljund vara. Teenuse uusim versioon, kus see streaming manifestifaili süntaks on värskendatud.

>[AZURE.NOTE]Videote voogesitamise faili manifesti (.ism) süntaks on reserveeritud kasutamiseks ja tulevaste versioonidega muutuda. Palun muuta või selle faili sisu käsitsemiseks.

###<a name="a-new-client-manifest-ismc-file-is-generated-in-the-output-asset-when-an-encoding-task-outputs-one-or-more-mp4-files"></a>Uue klientrakenduse manifesti (*. ISMC) fail on loodud väljund varade kui kodeering toimingu väljund MP4 üks või mitu faili

Teenuse uusim versioon, alustades rohkem MP4 faile, väljund pärast lõpetamist kodeering tööülesande, mis loob ühte varade sisaldab streaming kliendi manifesti (*.ismc) faili. Faili .ismc aitab parandada dünaamiline streaming. 

>[AZURE.NOTE]Kliendi manifesti (.ismc) faili süntaks on reserveeritud kasutamiseks ja tulevaste versioonidega muutuda. Palun muuta või selle faili sisu käsitsemiseks.

[Lisateabe saamiseks vt.](https://blogs.msdn.microsoft.com/randomnumber/2016/07/08/encoder-changes-within-azure-media-services-now-create-ismc-file/)

### <a name="known-issues"></a>Teadaolevad probleemid

Osa kliente võib pärineda aross kordamise sildi probleemi sujuva voogesituse manifestis. [Lisateavet leiate jaotisest.](media-services-deliver-content-overview.md#known-issues)

##<a id="apr_changes16"></a>Aprill 2016 väljaanne

### <a name="azure-media-analytics"></a>Azure Media Analytics

Azure Media Servces kasutusele Azure Media Kasutusanalüüsi võimas video intelligence. Üksikasjalikku teavet leiate teemast [Azure Media Services Analytics ülevaade](media-services-analytics-overview.md).

### <a name="apple-fairplay-preview"></a>Apple'i FairPlay (eelvaade)

Azure Media Servicesi võimaldab nüüd dünaamiliselt krüptida teie Apple'i FairPlay HTTP Live Streaming (HLS) sisu. Saate AMS litsentsi kohaletoimetamise teenuse osutamiseks FairPlay litsentside klientidele. Üksikasjalikumat teavet leiate teemast [kasutamine Azure Media Servicesi voogesituseks oma HLS sisu Apple FairPlay kaitstud ](media-services-protect-hls-with-fairplay.md).
  
##<a id="feb_changes16"></a>Veebruar 2016 väljaanne

Azure Media Services SDK .net-i (3.5.3) uusim versioon sisaldab Widevine seotud vigu parandada. Probleem: AssetDeliveryPolicy ei saanud taaskasutada mitme Varad Widevine krüptitud. See parandus viga osana järgmised atribuut lisatakse SDK: **WidevineBaseLicenseAcquisitionUrl**.
    
    Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
        new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
    {
        {AssetDeliveryPolicyConfigurationKey.WidevineBaseLicenseAcquisitionUrl,"http://testurl"},
        
    };

##<a id="jan_changes_16"></a>Jaanuar 2016 väljaanne

Reserveeritud üksuste kodeerimine ümber vähendada segadust kodeerija nimedega.

Basic, Standard, ja kodeering reserveeritud üksused on S1, S2, ümber nimetada ja S3 reserveeritud üksused, vastavalt.  Klientidele, kes kasutavad põhilised kodeerimine RUs täna näed S1 sildina Azure portaali (ja arve) ajal Standard ja Premium kuvatakse vastavalt siltide S2 ja S3. 

##<a id="dec_changes_15"></a>Detsember 2015 väljaanne

Azure'i SDK meeskonnatöö avaldatud uue versiooni [Azure'i SDK php](http://github.com/Azure/azure-sdk-for-php) paketi, mis sisaldab Microsoft Azure Media Servicesi värskendusi ja uued funktsioonid. Eelkõige Azure Media Services SDK PHP toetab nüüd uusimate funktsioonide [sisu kaitse](media-services-content-protection-overview.md) : dünaamiline krüptimise AES ja DRM (PlayReady ja Widevine) koos ja luba piiranguteta. Samuti toetab mastaapimist [Kodeerimine ühikud](media-services-dotnet-encoding-units.md).

Lisateavet leiate teemast:

- [Microsoft Azure Media Services SDK PHP](http://southworks.com/blog/2015/12/09/new-microsoft-azure-media-services-sdk-for-php-release-available-with-new-features-and-samples/) ajaveebi.
- Järgmised [koodinäiteid](http://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices) , mis aitavad teil kiireks alustamiseks tehke järgmist.
    - **vodworkflow_aes.php**: see on PHP faili, mis näitab, kuidas kasutada AES 128 dünaamiline krüptimise ja klahvi teenus. See on [selles](media-services-protect-with-aes128.md) artiklis selgitatakse .NET valimi alusel.
    - **vodworkflow_aes.php**: see on PHP faili, mis näitab, kuidas kasutada PlayReady dünaamiline krüptimise ja litsentsi teenus. See on [selles](media-services-protect-with-drm.md) artiklis selgitatakse .NET valimi alusel.
    - **scale_encoding_units.php**: see on PHP faili, mis näitab, kuidas mastaapimiseks kodeering reserveeritud üksus.


##<a id="nov_changes_15"></a>November 2015 väljaanne

Azure Media Servicesi pakub nüüd Google Widevine litsentsi teenus pilveteenuses. Rohkem üksikasju, vaadake [seda teadet ajaveebi](https://azure.microsoft.com/blog/announcing-google-widevine-license-delivery-services-public-preview-in-azure-media-services/). Vaadake [selle õpetuse](media-services-protect-with-drm.md) ja [GitHub hoidla](http://github.com/Azure-Samples/media-services-dotnet-dynamic-encryption-with-drm). 

Pöörake tähelepanu sellele Widevine litsentsi kohaletoimetamise teenused Azure Media ripplae eelvaates. [Lisateabe saamiseks vt.](https://azure.microsoft.com/blog/announcing-google-widevine-license-delivery-services-public-preview-in-azure-media-services/)

##<a id="oct_changes_15"></a>Oktoober 2015 väljaanne

Azure Media Services (AMS) on nüüd järgmine andmekeskuste reaalajas: Brasiilia Lõuna, India Lääne, India, Lõuna ja India keskse. Nüüd saate kasutada Azure klassikaline portaali luua [meediumi](media-services-portal-create-account.md) ja teha erinevaid toiminguid, mis on kirjeldatud [allpool](https://azure.microsoft.com/documentation/services/media-services/). Siiski Live kodeerimine pole lubatud järgmised andmekeskuste. Lisaks kõik tüüpi kodeerimine reserveeritud üksused on saadaval nende andmekeskuste.

- Brasiilia Lõuna: Standard- ja põhilised kodeerimine reserveeritud üksused on saadaval ainult
- India Lääne, India, Lõuna ja India keskse: ainult lihtsa kodeerimine reserveeritud üksused on saadaval


##<a id="september_changes_15"></a>Mai 2015 väljaanne 

- AMS nüüd pakub võimalust kaitsta nii Video-On-Demand (VOD) Live voogu Widevine muutuv DRM tehnoloogiaga. Järgmised partnerite kohaletoimetamise teenuste abil saate aitavad teil esitada Widevine litsentside: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [castLabs](http://castlabs.com/company/partners/azure/). [Lisateabe saamiseks vt.](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/)

    Saate konfigureerida oma AssetDeliveryConfiguration kasutada Widevine [AMS.NET SDK](https://www.nuget.org/packages/windowsazure.mediaservices/) (alates versioonist 3.5.1) või REST API-ga.  

- AMS lisatud tugi Apple ProRes videod. Nüüd saate üles laadida QuickTime allikas videod failide Apple ProRes või muude kodekite. [Lisateabe saamiseks vt.](https://azure.microsoft.com/blog/announcing-support-for-apple-prores-videos-in-azure-media-services/)

- Nüüd saate Media kodeerija Standard sub lõikamine ja reaalajas arhiivi eraldamine teha. [Lisateabe saamiseks vt.](https://azure.microsoft.com/blog/sub-clipping-and-live-archive-extraction-with-media-encoder-standard/)

- On tehtud filtreerimise järgmised värskendused: 

    - Nüüd saate Apple'i HTTP Live Streaming (HLS) vormingus ainult filtriga. Selle värskenduse, mis võimaldab eemaldada ainult jälitus määramisega (ainult = false) URL-i.
    - Filtrite oma varasid määratlemisel nüüd on teil võimalus mitme ühendamiseks ühe URL (kuni 3) filtrid.

    [Lisateabe saamiseks vt.](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/)

- AMS toetab nüüd I-Frame HLS v4. Ma paneeli tugi optimeerib edasi-ja tagasikerimine. Vaikimisi kõik HLS v4 väljundeid sisaldavad ma paneeli loend (EXT-X-I-FRAME-STREAM-INF).
 
    [Lisateabe saamiseks vt.](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/)

##<a id="august_changes_15"></a>August 2015 väljaanne

- Azure Media Services SDK Java V0.8.0 väljaanne ja uue näidised on nüüd saadaval. Lisateavet leiate teemast:

    - [Ajaveebipostitus](http://southworks.com/blog/2015/08/25/microsoft-azure-media-services-sdk-for-java-v0-8-0-released-and-new-samples-available/)
    - [Java näidised hoidla](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
- Azure Media Playeri värskendus mitme heli voo toega. Lisateavet leiate teemast:
    - [Ajaveebipostitus](https://azure.microsoft.com/blog/2015/08/13/azure-media-player-update-with-multi-audio-stream-support/)

##<a id="july_changes_15"></a>Juuli 2015 väljaanne

- Üldiselt kättesaadav Media kodeerija Standard teada. Lisateavet leiate teemast [sellest ajaveebipostitusest](https://azure.microsoft.com/blog/2015/07/16/announcing-the-general-availability-of-media-encoder-standard/).

    Media kodeerija Standard kasutab valmiskombinatsioonid [selles](http://go.microsoft.com/fwlink/?LinkId=618336) jaotises kirjeldatud. Pange tähele, et kui valmissäte kasutamise 4k kodeeritakse peaks saama **Premium** reserveeritud üksuse tüüp. Lisateavet leiate teemast [skaala kodeerimine](media-services-scale-media-processing-overview.md).
- Reaalajas reaalajas pealdiste Azure Media Servicesi ja -pleier. Lisateabe saamiseks vt [ajaveebipostitust](https://azure.microsoft.com/blog/2015/07/08/live-real-time-captions-with-azure-media-services-and-player/)

###<a name="media-services-net-sdk-updates"></a>Media Services .NET SDK värskendused

Nüüd on Azure Media Services .NET SDK versioon 3.4.0.0. Järgmised funktsioonid on selles versioonis lisatud:  

- Rakendatud tugi reaalajas arhiivi. Pange tähele, ei saa alla laadida varade, mis sisaldab reaalajas arhiivi.
- Dünaamilisi filtreid rakendada tugi.
- Rakendatud funktsionaalsus, mis võimaldab kasutajatel hoida salvestusruumi container varade kustutamisel.
- Seotud poliitikad kanalit, proovige veaparandused.
- **Media kodeerija Premium töövoo**lubatud.

##<a id="june_changes_15"></a>Juuni 2015 väljaanne

###<a name="media-services-net-sdk-updates"></a>Media Services .NET SDK värskendused

Nüüd on Azure Media Services .NET SDK versioon 3.3.0.0. Järgmised funktsioonid on selles versioonis lisatud:  

- OpenId ühenduse Discovery spec, tugi
- tugi töötlemise klahvid edastamiseks identiteedi pakkuja poolel. 

Kui kasutate identiteedipakkuja juures, mis seab OpenID Connect discovery dokumendi (nagu järgmised pakkujad: Azure Active Directory, Google, Salesforce), saate juhendada Azure Media Servicesi hankimine allkirjastamiseks klahvid jaoks valideerimine JWT luba OpenID kaudu ühenduse discovery spec. 

Lisateabe saamiseks vt [Abil Json Web klahvid OpenID Connect discovery spec töötamiseks JWT Turbeloa autentimine Azure Media Servicesi kaudu](http://gtrifonov.com/2015/06/07/using-json-web-keys-from-openid-connect-discovery-spec-to-work-with-jwt-token-authentication-in-azure-media-services/).


##<a id="may_changes_15"></a>Mai 2015 väljaanne

Väljakuulutamist järgmised uued funktsioonid:

- [Reaalajas kodeerimine Media teenustega eelvaade](media-services-manage-live-encoder-enabled-channels.md)
- [Dünaamiliste manifesti](media-services-dynamic-manifest-overview.md)
- [Azure Media Liikuv stoppkaader meediumi protsessor eelvaade](https://azure.microsoft.com/blog/?p=286281&preview=1&_ppp=61e1a0b3db)

##<a id="april_changes_15"></a>Aprill 2015 väljaanne

 ###<a name="general-media-services-updates"></a>Üldine Media Services värskendused

- [Väljakuulutamist Azure Media Player](https://azure.microsoft.com/blog/2015/04/15/announcing-azure-media-player/).
- Media Services ülejäänud 2.10, kanalid, mis on konfigureeritud neelata on RTMP protokolli, alustades on loodud esmase ja teisese neelata URL-id. Lisateavet leiate teemast [kanali neelata konfiguratsioone](media-services-live-streaming-with-onprem-encoders.md#channel_input)
- Azure Media indekseerija värskendused
- Tugi: hispaania keeles
- Uue konfiguratsiooni XML-vormingus

[Lisateabe saamiseks vt.](https://azure.microsoft.com/blog/2015/04/13/azure-media-indexer-spanish-v1-2/)
###<a name="media-services-net-sdk-updates"></a>Media Services .NET SDK värskendused

Nüüd on Azure Media Services .NET SDK versioon 3.2.0.0.

Järgnevalt mõned kliendi vastastikuste värskendused:

- **Katkestamine muutmine**: muudetud **TokenRestrictionTemplate.Issuer** ja **TokenRestrictionTemplate.Audience** stringi tüüpi.
- Luua kohandatud seotud värskendusi uuesti poliitika.
- Failide allalaadimine ja üleslaadimine seotud veaparandused.
- Nüüd aktsepteerib **MediaServicesCredentials** klassi esmaseid ja teiseseid juurdepääsu juhtimine lõpp-punkti autentimiseks.



##<a id="march_changes_15"></a>Märts 2015 väljaanne

### <a name="general-media-services-updates"></a>Üldine Media Services värskendused

- Nüüd pakub Media Servicesi Azure'i CDN integreerimine. Integreerimine toetamiseks lisati **CdnEnabled** atribuudi **StreamingEndpoint**.  **CdnEnabled** saab kasutada REST API-de alustades 2.9 versioon (Lisateavet leiate teemast [StreamingEndpoint](https://msdn.microsoft.com/library/azure/dn783468.aspx)).  **CdnEnabled** saab kasutada .NET SDK alustades 3.1.0.2 versioon (Lisateavet leiate teemast [StreamingEndpoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.istreamingendpoint(v=azure.10).aspx)).
- Teadaanne **Media kodeerija Premium**töövoo. Lisateavet leiate teemast [Tutvustus Premium kodeerimine Azure Media Servicesi](https://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services/).
 


##<a id="february_changes_15"></a>Väljaanne veebruar 2015

### <a name="general-media-services-updates"></a>Üldine Media Services värskendused

Media Services REST API on nüüd versioon 2.9. Alustades selle versiooni, saate lubada integreerimine Azure CDN streaming lõpp-punktid. Lisateavet leiate teemast [StreamingEndpoint](https://msdn.microsoft.com/library/dn783468.aspx).

##<a id="january_changes_15"></a>Jaanuar 2015 väljaanne

### <a name="general-media-services-updates"></a>Üldine Media Services värskendused

Teadet, General Availability (GA) sisu kaitse dünaamiline krüptimise abil. Lisateavet leiate teemast [Azure Media Servicesi täiustab streaming turvalisus üldine kättesaadavus DRM tehnoloogia](https://azure.microsoft.com/blog/2015/01/29/azure-media-services-enhances-streaming-security-with-general-availability-of-drm-technology/).

###<a name="media-services-net-sdk-updates"></a>Media Services .NET SDK värskendused

Nüüd on Azure Media Services .NET SDK versioon 3.1.0.1.

See väljaanne märgitud Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization.TokenRestrictionTemplate Vaikekonstruktor aegunuks. Uue ehitaja võtab TokenType argumendina.

    TokenRestrictionTemplate template = new TokenRestrictionTemplate(TokenType.SWT);


##<a id="december_changes_14"></a>Detsembrini 2014 väljaanne

###<a name="general-media-services-updates"></a>Üldine Media Services värskendused

- Azure'i indekseerija meediumi protsessor lisati mõned värskendused ja uued funktsioonid. Lisateavet leiate teemast [Azure Media indekseerija versioon 1.1.6.7 väljalaskemärkmed](https://azure.microsoft.com/blog/2014/12/03/azure-media-indexer-version-1-1-6-7-release-notes/).
- Lisatud uus REST API, mis võimaldab teil värskendada kodeerimine reserveeritud ühikud: [EncodingReservedUnitType ülejäänud](http://msdn.microsoft.com/library/azure/dn859236.aspx).
- Lisatud CORS kasutajatugi võtme teenus.
- Päringute autoriseerimine poliitika valikud jõudlustäiustused on valmis.
- Hiina andmekeskuse, [URL-i võti kohaletoimetamise](http://msdn.microsoft.com/library/azure/ef4dfeeb-48ae-4596-ab28-44d6b36d8769#get_delivery_service_url) on nüüd kliendi kohta (nagu muude andmekeskuste).
- Lisatud HLS automaatne target kestus. Reaalajas streaming tehes HLS alati pakitud dünaamiliselt. Vaikimisi arvutab Media Services automaatselt HLS lõigu pakendit suhe (FragmentsPerSegment) ka edaspidi Piltide rühmitamine – GOP, mis on saadud reaalajas kodeerija keyframe intervall (KeyFrameInterval) alusel. Lisateavet leiate teemast [töötamine Azure Media Services Live Streaming].
 
###<a name="media-services-net-sdk-updates"></a>Media Services .NET SDK värskendused

- Nüüd on [Azure Media Services.NET SDK](http://www.nuget.org/packages/windowsazure.mediaservices/) versioon 3.1.0.0.
- Täiendatud sõltuvus .net SDK Frameworki 4.5.
- Lisatud uus API, mis võimaldab teil värskendada kodeering reserveeritud üksused. Lisateabe saamiseks lugege teemat [värskendamine reserveeritud üksuse tüüp ja suurendab kodeerimine RUs kasutades .net-i](http://msdn.microsoft.com/library/azure/jj129582.aspx).
- Lisatud JWT (JSON Web Turbeloa) kasutajatugi Turbeloa autentimist. Lisateabe saamiseks lugege teemat [Azure Media Servicesi ja dünaamiline krüptimise JWT Turbeloa autentimine](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/).
- Lisatud suhteline nihetega BeginDate ja ExpirationDate PlayReady litsentsi malli.


##<a id="november_changes_14"></a>November 2014 väljaanne

- Media Services võimaldab nüüd neelata reaalajas sujuva voogesituse (FMP4) sisu SSL-ühenduse kaudu. Neelata SSL, veenduge, et värskendada neelata URL-i HTTPS-i.  Lisateavet live streaming leiate teemast [Azure Media Services Live Streaming töötamine].
- Pange tähele, et praegu, te ei saa neelata RTMP reaalajas voogu SSL-ühenduse.
- Voogesituseks sisu SSL-ühenduse kaudu. Selleks, veenduge, et HTTPS Alusta voogesituse URL-ide.
- Pange tähele, et ainult voogesituseks SSL kui streaming lõpp-punkti, kus saate esitada oma sisu on loodud pärast 10 mai 2014. Kui streaming URL-ide on loodud pärast mai 10 streaming lõpp-punktid, URL sisaldab "streaming.mediaservices.windows.net" (uus vorming). SSL-i ei toeta streaming URL-id, mis sisaldavad "origin.mediaservices.windows.net" (vana vorming). Kui teie URL on vana vormingus ja soovite üle SSL-i, [saate luua uue streaming lõpp-punkti](media-services-portal-manage-streaming-endpoints.md). URL-id, mis on loodud uue streaming lõpp-punkti põhineb abil sisu voona SSL.

##<a id="october_changes_14"></a>Oktoober 2014 väljaanne

### <a id="new_encoder_release"></a>Media Services kodeerija väljaanne

Teada Media Services Azure Media Encoderi uus versioon. Tarkvara ja Azure Media Encoderi ainult ostmisega väljund GBs, kuid muidu uue kodeerija funktsioon ühildu eelmise kodeerija. Lisateavet [Media Services hinnad üksikasjad]).

### <a id="oct_sdk"></a>Media Servicesi .NET SDK 

Nüüd on versiooni 2.0.0.3 Media Services SDK .NET laiendid.

Nüüd on Media Services SDK .net-i versioon 3.0.0.8.

On tehtud järgmisi muudatusi:

- Refaktooring uuesti poliitika tunnid.
- Kasutajaagendi stringi lisamine http taotluse päised.
- Lisades Nugeti taastamine Koosta toimingut.
- Probleemide stsenaariumi katsed kasutada x509 cert hoidlast.
- Sätete kontrollimine kanali värskendamisel ja streaming end.
 

### <a name="new-github-repository-to-host-media-services-samples"></a>Uue GitHub hoidla host Media Servicesi proovide

Näidised asuvad [Azure Media Servicesi näidised GitHub hoidla](https://github.com/Azure/Azure-Media-Services-Samples).


##<a id="september_changes_14"></a>August 2014 väljaanne

Nüüd on Media Services ülejäänud metaandmete versioon 2.7. ÜLEJÄÄNUD uusimate värskenduste kohta leiate lisateavet teemast [Azure Media Services REST API viide].

Media Services SDK .net-i jaoks on nüüd versioon 3.0.0.7
 
### <a id="sept_14_breaking_changes"></a>Suurte muudatuste

* **Origin** nimetati ümber [StreamingEndpoint].
* Muudatuste vaikekäitumine kodeerida ja seejärel avaldada MP4 faile **Azure klassikaline portaali** kasutamisel.

Varem, kui avaldamine ühe faili MP4 video varade SAS URL-i Azure klassikaline portaali kaudu ei looda (SAS URL-id võimaldavad teil laadida video lisamine bloobimälu). Praegu kodeerida ja seejärel avaldada ühe faili MP4 video varade Azure klassikaline portaali kasutamisel loodud URL-i osutab Azure Media Servicesi streaming lõpp-punkti.  See muudatus ei mõjuta MP4 videod, mis on otse Media Servicesi üles laadida ja ilma on kodeeritud Azure Media Servicesi avaldatud.

Praegu on teil järgmised kaks võimalust probleemi lahendada.

* Luba streaming ühikud ja voona .mp4 varade sujuva voogesituse esitlusena dünaamiline pakendit abil.

* Looge SAS URL-i allalaadimine (või järk-järgult esitamiseks) soovitud .mp4. SAS locator loomise kohta leiate lisateavet teemast [Pakkuda sisu].


### <a id="sept_14_GA_changes"></a>Uute funktsioonide/stsenaariumid, mis on osa GA versioon

* **Indekseerija meediumi protsessor**. Lisateabe saamiseks vaadake teemat [Azure Media indekseerija indekseerimine meediumifaile].

* [StreamingEndpoint] ettevõte võimaldab nüüd lisada kohandatud domeeninimesid (host).

    Kohandatud domeeninime kasutada Media Servicesi streaming lõpp-punkti nimi, peate kohandatud hostinimed lisamiseks oma streaming lõpp-punkti. Media Services REST API-d või .NET SDK abil saate lisada kohandatud hostinimed.
    
    Kehtivad järgmisega.
    
    * Kohandatud domeeni nime omandiõigust peab teil olema.
    
    * Azure Media Servicesi nime domeeni omandiõiguse kinnitamiseks. Domeeni kinnitamiseks loomine CName, mis kaartide <MediaServicesAccountId>.<parent domain> verifydns. < mediaservices DNS-i tsooni >. 
    
    * Peate looma teise CName, mida saab vastendada teie Media Services StreamingEndpont hosti nimi (nt amstest.streaming.mediaservices.windows.net) kohandatud hosti nimi (nt sports.contoso.com).


    Lisateabe saamiseks vt **CustomHostNames** atribuut [StreamingEndpoint] teema.

### <a id="sept_14_preview_changes"></a>Uute funktsioonide/stsenaariumi, mis on osa avaliku eelvaateversiooniga

* Live Streaming eelvaade. Lisateavet leiate teemast [töötamine Azure Media Services Live Streaming].

* Võtme teenus. Lisateabe saamiseks vt [abil AES 128 dünaamiline krüptimise ja klahvi teenus].

* AES dünaamiline krüptimise. Lisateavet leiate teemadest [abil AES 128 dünaamiline krüptimise ja klahvi teenus].

* PlayReady litsentsi teenus. Lisateavet leiate teemadest [abil PlayReady dünaamiline krüptimise ja litsentsi teenus].

* PlayReady dünaamiline krüptimine. Lisateavet leiate teemadest [abil PlayReady dünaamiline krüptimise ja litsentsi teenus].

* Media Services PlayReady litsentsi malli. Kohta leiate teemast [Media Services PlayReady litsentsi Mall ülevaade].

* Salvestusruumi streaming krüptitud varad. Lisateavet leiate teemast [Streaming salvestusruumi krüptitud sisu].

##<a id="august_changes_14"></a>August 2014 väljaanne

Kui te kodeerida vara, saadakse väljundi vara lõpetamisel kodeering töö. Kuni see väljaanne toodeti Azure Media Services kodeerija metaandmeid väljundi varad. See väljaanne alustades kodeerija toodab ka metaandmeid Sisestuskeel varad. Lisateavet leiate teemadest [Sisendit metaandmete] ja [Väljundi metaandmed] .


##<a id="july_changes_14"></a>Juuli 2014 väljaanne

Järgmised veaparandused on tehtud Azure Media Services pakendaja ja krüptija:

* Ainult heli esitatakse tagasi, kui transmuxing reaalajas arhiivi varade abil HTTP Live Streaming – see on fikseeritud ja nüüd kõlava heli- ja video.

* Kui HTTP Live Streaming ja AES 128-bitine ümbriku krüptimise vara pakendamine, pakkimisel voogu ei esitata Android-seadmetes – see viga on fikseeritud ja pakkimisel voo esitatakse tagasi Androidi seadmetes, mis toetavad HTTP Live Streaming.

##<a id="may_changes_14"></a>Mail 2014 väljaanne

### <a id="may_14_changes"></a>Üldine Media Services värskendused

Nüüd saate [Dünaamiline pakendit] voogesituseks v3 HTTP Live Streaming (HLS). Voona HLS v3, lisage origin locator tee järgmises vormingus: * .ism/manifest(format=m3u8-aapl-v3). Lisateavet leiate teemast [hüüdnimi Drouin ajaveebi].

Sujuva voogesituse staatiliselt krüptitud PlayReady põhjal dünaamiliste pakendit nüüd ka PlayReady krüptitud toetab pakkuda HLS (v3 ja v4). Sujuva voogesituse PlayReady krüptimiseks kohta teabe saamiseks vt [Kaitsmine sujuv voo koos PlayReady].

### <a name="may_14_donnet_changes"></a>Media Services .NET SDK värskendused

Järgmised täiustused on kaasatud Media Services .NET SDK 3.0.0.5 versioonis:

* Parem kiirus ja paindlikkus meediumivarad üles/alla.

* Proovi uuesti loogika ja mööduvad erandi käsitsemise täiustused: 

    * Siirdamiseks tõrge tuvastamise ja proovige uuesti loogika on täiustatud erandid, mis põhjustavad päringud, muudatuste salvestamist, üleslaadimine või failide allalaadimise. 
    
    * Kui saada Web erandid (nt ajal ACS Turbeloa taotlus), märkate, et pöördumatud tõrked ei suuda kiiremini kohe.

Lisateabe saamiseks [Lisamise]kohta leiate teemast proovige Media Services SDK .net-i jaoks.

##<a id="april_changes_14"></a>Aprill 2014 kodeerija väljaanne

### <a name="april_14_enocer_changes"></a>Media Services kodeerija värskendused

* Lisatud tugi söömisega AVI faile autoriks redaktoris Grass Valley EDIUS nonlinear, kui video on kergelt tihendatud Grass Valley HQ/HQX kodekiga abil. Lisateabe saamiseks vt [Grass Valley teatab EDIUS 7 Streaming kaudu Cloud].

* Lisatud tugiteenuste määratlemiseks toodetud Media Encoderi failide nimereeglistik. Lisateabe saamiseks vt [Juhtimine meediumi teenuse kodeerija väljundi failinimed].

* Video-ja/või heli ülekatete lisatud tugi. Lisateabe saamiseks vt [Ülekatete loomine].

* Lisatud tugi selliste mitme video segmente. Lisateavet leiate teemast [Õmblemisega Video segmente].

* Fikseeritud vea, kui heli on kodeeritud MPEG-1 Audio layer 3 (ehk MP3) MP4s transkodeerida seotud.


##<a id="jan_feb_changes_14"></a>Väljalasete jaanuaris/veebruaris 2014

### <a name="jan_fab_14_donnet_changes"></a>Azure Media Services .NET SDK 3.0.0.1, 3.0.0.2 ja 3.0.0.3

Järgmised muudatused 3.0.0.1 ja 3.0.0.2.

* Fikseeritud probleemid, mis on seotud LINQ päringute OrderBy andmete kasutamist.

* Tükeldatud testi lahenduste [GitHub] põhineva kontrollib ja stsenaariumipõhiseid kontrollib.

Muudatuste kohta leiate lisateavet teemast: [Azure Media Services .NET SDK 3.0.0.1 ja 3.0.0.2 välja].

3.0.0.3 olid teha järgmisi muudatusi:

* Uuele versioonile üle viidud Azure storage sõltuvused kasutada versiooni 3.0.3.0. 

* Fikseeritud tagasiühilduvuse probleemi 3.0. *.* välja. 


##<a id="december_changes_13"></a>Detsember 2013 väljaanne

### <a name="dec_13_donnet_changes"></a>Azure Media Services .NET SDK 3.0.0.0

>[AZURE.NOTE] 3.0.x.x väljalasete pole tagasiühilduvad 2.4.x.x versioonidega.

Nüüd on Media Services SDK uusima versiooni 3.0.0.0. Saate Nugeti uusim alla laadida või selle bittide toomine [GitHub].

Alustades Media Services SDK versiooni 3.0.0.0, saate kasutada [Azure Active Directory Access Control teenus (ACS)] märgid. Lisateabe saamiseks [ühenduse loomine Media Servicesi Media Services SDK .net-i] teema jaotisest "Diagrammide korduvkasutamine juurdepääsu juhtimine teenuse sõned".

### <a name="dec_13_donnet_ext_changes"></a>Azure Media Services .NET SDK laiendid 2.0.0.0

Azure Media Services .NET SDK laiendid on laiend meetodite ja helper funktsioone, mis lihtsustab oma kood ja lihtsam töötada Azure Media Servicesi. Saad uusima bittide [Azure Media Services .NET SDK laiendid].

##<a id="november_changes_13"></a>November 2013 väljaanne

### <a name="nov_13_donnet_changes"></a>Azure Media Services .NET SDK muudatused

Alates selle versiooniga, Media Services SDK .net-i tegeleb siirdamiseks viga tõrkeid, mis võivad ilmneda helistamist Media Services REST API kihti.

##<a id="august_changes_13"></a>August 2013 väljaanne

### <a name="aug_13_powershell_changes"></a>Media Services PowerShelli cmdlet-käskude kaasatud Azure'i Sdk tööriistad

Järgmised Media Services PowerShelli cmdlet-käsud on nüüd kaasatud [azure-sdk-tööriistad].

* Get-AzureMediaServices 

    Näiteks `Get-AzureMediaServicesAccount`.

* Uue AzureMediaServicesAccount 

    Näiteks `New-AzureMediaServicesAccount -Name “MediaAccountName” -Location “Region” -StorageAccountName “StorageAccountName”`.

* Uue AzureMediaServicesKey 

    Näiteks `New-AzureMediaServicesKey -Name “MediaAccountName” -KeyType Secondary -Force`.

* Eemalda – AzureMediaServicesAccount 

    Näiteks `Remove-AzureMediaServicesAccount -Name “MediaAccountName” -Force`.

##<a id="june_changes_13"></a>Juuni 2013 väljaanne

### <a name="june_13_general_changes"></a>Azure Media Servicesi muudatused

Selles jaotises muudatused on kaasatud juuni 2013 Media Servicesi väljalasete värskendused.

* Võimalus mitme salvestusruumi konto linkimine meediumi teenusekonto. 

    StorageAccount
    
    Asset.StorageAccountName ja Asset.StorageAccount

* Võimalus värskendada Job.Priority. 

* Teatis seotud üksuste ja atribuudid: 

    JobNotificationSubscription
    
    NotificationEndPoint
    
    Töö

* Asset.Uri 

* Locator.Name 

### <a name="june_13_dotnet_changes"></a>Azure Media Services .NET SDK muudatused

Järgmisi muudatusi kaasatakse juuni 2013 Media Services SDK välja. Viimane Media Services SDK on saadaval github.

* Alates versioonist 2.3.0.0, Media Services SDK toetab mitut salvestusruumi linkimise kontod Media Servicesi konto. Seda funktsiooni ei toeta järgmisi API:
    
    IStorageAccount tüüp.
    
    Atribuut Microsoft.WindowsAzure.MediaServices.Client.CloudMediaContext.StorageAccounts.
    
    Atribuut StorageAccount.
    
    Atribuut StorageAccountName.
    
    Lisateabe saamiseks vt [Haldamise teenuste Meediumivarad üle mitme salvestusruumi kontod].

* Teatis seotud API-d. Alates versioonist 2.2.0.0 teil on võimalus kuulata Azure'i järjekorda salvestusruumi teatised. Lisateavet leiate teemast [Töötlemise meediumi teenuste töö teatised].
    
    Atribuut Microsoft.WindowsAzure.MediaServices.Client.IJob.JobNotificationSubscriptions.
    
    Microsoft.WindowsAzure.MediaServices.Client.INotificationEndPoint tüüp.
    
    Microsoft.WindowsAzure.MediaServices.Client.IJobNotificationSubscription tüüp.
    
    Microsoft.WindowsAzure.MediaServices.Client.NotificationEndPointCollection tüüp.
    
    Microsoft.WindowsAzure.MediaServices.Client.NotificationEndPointType tüüp.
    
    Microsoft.WindowsAzure.MediaServices.Client.NotificationJobState tüüp.

* Sõltuvus Azure Storage kliendi SDK 2.0 (Microsoft.WindowsAzure.StorageClient.dll).

* Sõltuvus OData 5,5 (Microsoft.Data.OData.dll).


##<a id="december_changes_12"></a>Väljaanne detsember 2012

### <a name="dec_12_dotnet_changes"></a>Azure Media Services .NET SDK muudatused

* IntelliSense'i: Lisatud puuduvad mitut tüüpi IntelliSense'i dokumentatsioonist.

* Microsoft.Practices.TransientFaultHandling.Core: Fikseeritud küsimus, kus SDK oli veel sõltuvus selle komplekti vanemat versiooni. SDK viitab nüüd selle komplekti 5.1.1209.1 versiooni.

Parandused November 2012 SDK leitud probleemide kohta:

* IAsset.Locators.Count: See arv on nüüd õigesti teatatud uue IAsset liideste pärast kõigi lokaatorid on kustutatud.

* IAssetFile.ContentFileSize: See väärtus on nüüd õigesti seatud pärast üleslaadimine IAssetFile.Upload(filepath) järgi.

* IAssetFile.ContentFileSize: Selle atribuudi saab nüüd seada kui vara faili loomisel. See on varem kirjutuskaitstud.

* IAssetFile.Upload(filepath): Fikseeritud küsimus, kus see sünkroonse üles meetod on korrutamine järgmine tõrketeade vara mitu faili üleslaadimisel. Tõrge "Server ei saa autentida kutse. Veenduge, et väärtus autoriseerimine päise moodustavad õigesti, sh allkirja."

* IAssetFile.UploadAsync: Fikseeritud küsimus, kus rohkem kui 5 failide üleslaadimist korraga.

* IAssetFile.UploadProgressChanged: Sel juhul on antud SDK.

* IAssetFile.DownloadAsync (stringi, BlobTransferClient, ILocator, CancellationToken): see meetod ülekoormuse on antud.

* IAssetFile.DownloadAsync: Fikseeritud küsimus, kus saanud korraga alla laadida kuni 5 failid.

* IAssetFile.Delete(): Fikseeritud küsimus, kus helistaja kustutamine võib põhjustada erandi, kui faili on üles laaditud jaoks soovitud IAssetFile.

* Töö: Fikseeritud küsimus, kus on "MP4 sujuv voogu tööülesande" töö malli abil "PlayReady kaitse ülesanne" Aheldamise ei looks toiminguteks üldse.

* EncryptionUtils.GetCertificateFromStore(): Seda meetodit enam põhjustab null viide erandi tõttu tõrkeid, mis on serdi konfiguratsiooniprobleemide põhjal serdi leidmine.

##<a id="november_changes_12"></a>November 2012 väljaanne

Selles jaotises muudatused on kaasatud November 2012 (versioon 2.0.0.0) värskendused SDK. Need muudatused võivad nõuda kood kirjutatakse juuni 2012 Preview SDK vabastage muuta või ümber.

* Varad
    
    IAsset.Create(assetName) on ainult varade loomise funktsioon. IAsset.Create lisatud failide enam meetod kõne osana. Kasutage IAssetFile üles.
    
    Teenuste SDK on eemaldatud IAsset.Publish meetodit ja AssetState.Publish loendamine väärtuse. Mis tahes tähis, mis sõltub selle väärtuseks peab olema ümber kirjutatud.

* FileInfo

    See tund on eemaldada ja asendada IAssetFile.

    IAssetFiles

    IAssetFile asendab FileInfo ja on erinevate käitumist. Seda kasutada IAssetFiles objekti, kasutades Media Services SDK või Azure salvestusruumi SDK faili üleslaadimine, millele järgneb väärtustada. Saate kasutada järgmisi IAssetFile.Upload ülekoormuse:

    * IAssetFile.Upload(filePath): Sünkroonse meetod, mis blokeerib teema ja on soovitatav ainult ühe faili üleslaadimisel.
    
    * IAssetFile.UploadAsync (filePath blobTransferClient locator, cancellationToken): asünkroonne sisestusmeetod. See on eelistatud üles süsteem. 

    Teadaolev viga: abil soovitud cancellationToken tõepoolest tühistamine üles; siiski tühistamise olek tööülesanded võib olla mis tahes arv Ühendriikides. Peate õigesti jaole juba ja käsitlemiseks erandid.

* Lokaatorid
    
    Origin kohased versioonid on eemaldatud. SAS kohased kontekstis. Locators.CreateSasLocator (varade, accessPolicy) on märgitud taunitud või ga poolt eemaldatud Vt lokaatorid jaotises uusi funktsioone värskendatud käitumist.


##<a id="june_changes_12"></a>Juuni 2012 eelvaateversiooniga

Järgmised funktsioonid on uus SDK November versioonis.

* Üksuste kustutamine

    IAsset, IAssetFile, ILocator IAccessPolicy, IContentKey objektide on nüüd kustutatud objekti tasemel, st IObject.Delete() asemel kogumise, mis on cloudMediaContext.ObjCollection.Delete(objInstance) kustutada.

* Lokaatorid

    Lokaatorid nüüd tuleb luua CreateLocator meetodil ja kasutage LocatorType.SAS või LocatorType.OnDemandOrigin kellaajavälju väärtusi argumendina locator kindlat tüüpi soovite luua.

    Lokaatorid lihtsam saada oma sisu jaoks kasutatav URI-d on lisatud uusi atribuute. See kujundada lokaatorid pidi taotlusi tulevaste kolmanda osapoole laiendatavuse ja hõlpsamaks kasutamise jaoks klientrakendused meediumid suurendada.

* Asünkroonne meetod tugi

    Asünkroonne tugi on lisatud kõik meetodid.


##<a name="media-services-learning-paths"></a>Media Servicesi Õppeteed

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Tagasiside saatmine

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]


<!-- Anchors. -->

<!-- Images. -->

<!--- URLs. --->
[Azure Media Services MSDN-i Foorum]: http://social.msdn.microsoft.com/forums/azure/home?forum=MediaServices
[Azure Media Services REST API viide]: http://msdn.microsoft.com/library/azure/hh973617.aspx 
[Teenuste hinnakirjad üksikasjad]: http://azure.microsoft.com/pricing/details/media-services/
[Sisestuskeel metaandmed]: http://msdn.microsoft.com/library/azure/dn783120.aspx
[Väljundi metaandmed]: http://msdn.microsoft.com/library/azure/dn783217.aspx
[Videosisu]: http://msdn.microsoft.com/library/azure/hh973618.aspx
[Azure Media indekseerija meediumifaile indekseerimine]: http://msdn.microsoft.com/library/azure/dn783455.aspx
[StreamingEndpoint]: http://msdn.microsoft.com/library/azure/dn783468.aspx
[Azure Media Servicesi töötamise Live Streaming]: http://msdn.microsoft.com/library/azure/dn783466.aspx
[AES 128 dünaamiline krüptimise ja teenus tootenumbri abil]: http://msdn.microsoft.com/library/azure/dn783457.aspx
[PlayReady dünaamiline krüptimise ja litsentsi teenus abil]: http://msdn.microsoft.com/library/azure/dn783467.aspx
[Preview features]: http://azure.microsoft.com/services/preview/
[Media Services PlayReady litsentsi Mall ülevaade]: http://msdn.microsoft.com/library/azure/dn783459.aspx
[Salvestusruumi streaming krüptitud sisu]: http://msdn.microsoft.com/library/azure/dn783451.aspx
[Azure Classic Portal]: https://manage.windowsazure.com
[Dünaamiliste pakendit]: http://msdn.microsoft.com/library/azure/jj889436.aspx
[Hüüdnimi Drouin ajaveeb]: http://blog-ndrouin.azurewebsites.net/hls-v3-new-old-thing/
[Sujuv voo koos PlayReady kaitsmine]: http://msdn.microsoft.com/library/azure/dn189154.aspx
[Uuestiproovimise loogikale Media teenuste SDK .net-i jaoks]: http://msdn.microsoft.com/library/azure/dn745650.aspx
[Hillarys teatab EDIUS 7 voogesitamine pilveteenuses]: http://www.streamingmedia.com/Producer/Articles/ReadArticle.aspx?ArticleID=96351&utm_source=dlvr.it&utm_medium=twitter
[Kontrolli meediumi teenuse kodeerija väljundi failinimed]: http://msdn.microsoft.com/library/azure/dn303341.aspx
[Ülekatete loomine]: http://msdn.microsoft.com/library/azure/dn640496.aspx
[Video segmente õmblemisega]: http://msdn.microsoft.com/library/azure/dn640504.aspx
[Azure Media Services .NET SDK 3.0.0.1 3.0.0.2 väljalasete]: http://www.gtrifonov.com/2014/02/07/windows-azure-media-services-.net-sdk-3.0.0.2-release/
[Azure Active Directory Access Control teenus (ACS)]: http://msdn.microsoft.com/library/hh147631.aspx
[Ühenduse Media Servicesi Media teenustega SDK .net-i jaoks]: http://msdn.microsoft.com/library/azure/jj129571.aspx
[Azure Media Services .NET SDK laiendid]: https://github.com/Azure/azure-sdk-for-media-services-extensions/tree/dev
[Azure'i-sdk-tööriistad]: https://github.com/Azure/azure-sdk-tools
[GitHub]: https://github.com/Azure/azure-sdk-for-media-services
[Haldamise Media Services varad üle mitme salvestusruumi kontod]: http://msdn.microsoft.com/library/azure/dn271889.aspx
[Töötlemise Media Services töö teatised]: http://msdn.microsoft.com/library/azure/dn261241.aspx
 
