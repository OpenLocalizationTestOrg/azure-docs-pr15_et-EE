<properties
    pageTitle="CDN vahemällu poliitika Media Services laiendamine"
    description="Selles teemas antakse ülevaade CDN-ID, vahemällu poliitika Media Services laiendamine."
    services="media-services,cdn"
    documentationCenter=".NET"
    authors="juliako"
    manager="erikre"
    editor=""/>

<tags
    ms.service="media-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="juliako"/>
 
#<a name="cdn-caching-policy-in-media-services-extension"></a>CDN vahemällu poliitika Media Services laiendamine

Azure Media Servicesi näevad HTTP kohandatav voogesitus ja järk-järgult alla laadida. HTTP vastavalt streaming on väga paindlik kasu vahemällu puhverserveri CDN kihid ning kliendi küljel vahemällu. Lõpp-punktid streaming pakub üldist streaming võimaluste ja ka konfiguratsioon HTTP vahemälu päised. Lõpp-punktid streaming määrab HTTP olla: max-vanus ja aegub päised. Saate lisateavet HTTP vahemälu päiste [W3.org](http://www.w3.org/Protocols/rfc2616/rfc2616-sec13.html)kaudu.

##<a name="default-caching-headers"></a>Vaikimisi vahemälu päised

Vaikimisi rakendada streaming lõpp 3 päeva vahemälu päised tellitavate voogesituse andmete (tegelik meediumi fragmendid/tükkideks) ja manifest(playlist). Reaalajas streaming streaming lõpp-punktid rakendamine 3 päeva vahemälu päised andmete (tegelik meediumi fragmendid/tükkideks) ja kaks sekundit all vahemälu manifest(playlist) taotlusi päis. Kui reaalajas programmi muutub nõudmisel (reaalajas Arhiiv), rakendage nõudmisel streaming vahemälu päised.

##<a name="azure-cdn-integration"></a>Azure'i CDN integreerimine

Azure Media Servicesi pakub [integreeritud CDN](https://azure.microsoft.com/updates/azure-media-services-now-fully-integrated-with-azure-cdn/) streaming-lõpp-punktid. Olla päised kehtib samamoodi nagu streaming lõpp-punktid, et CDN lubatud streaming lõpp-punktid. Azure'i CDN kasutab streaming lõpp-punkti konfigureeritud vahemälu väärtused määratleda elu jooksul ettevõttesiseselt vahemällu talletatud objektide ning kasutab ka selle väärtuse määramiseks kohaletoimetamisega vahemälu päised. Kui kasutades CDN lubatud streaming lõpp-punktid ei ole soovitatav määrata small vahemälu väärtused. Säte väiksed väärtused vähendamiseks jõudlus ja kasu CDN vähendage. See pole lubatud seadmiseks vahemälu päised väiksemad kui 600 sekundit CDN lubatud streaming lõpp-punktid.

>[AZURE.IMPORTANT] Azure Media Servicesi integreerimine Azure CDN rakendatakse **Azure'i CDN Verizon**.  Kui soovite kasutada Azure Media Servicesi **Kaudu Akamai Azure'i CDN-ID** , peate [lõpp-punkti käsitsi konfigureerimine](cdn-create-new-endpoint.md).  Azure'i CDN funktsioonide kohta leiate lisateavet teemast [CDN ülevaade](cdn-overview.md).

##<a name="configuring-cache-headers-with-azure-media-services"></a>Vahemälu päised ja Azure Media Servicesi konfigureerimine

Saate konfigureerida vahemälu päise väärtused Azure'i haldusportaal või Azure Media Services API-d.

1. Konfigureerida vahemälu päised haldusega portaali palun vaadake jaotist [Kuidas hallata Streaming lõpp-punktid](../media-services/media-services-portal-manage-streaming-endpoints.md) konfigureerimise Streaming lõpp-punkti.
2. Azure Media Services REST API [StreamingEndpoint](https://msdn.microsoft.com/library/azure/dn783468.aspx#StreamingEndpointCacheControl).
3. Azure Media Services .NET SDK [StreamingEndpointCacheControl atribuudid](http://go.microsoft.com/fwlink/?LinkId=615302).

##<a name="cache-configuration-precedence-order"></a>Vahemälu konfiguratsiooni tähtsuse järjekord

1. Azure Media Servicesi konfigureeritud vahemälu väärtus alistab vaikeväärtus.
2. Kui pole käsitsi konfigureerimine, kehtib vaikeväärtused.
3. Poolt vaikimisi kaks sekundit all vahemälu päised kehtib live streaming manifest(playlist) sõltumata Azure Media või Azure Storage konfiguratsiooni ja selle väärtuse alistamine pole saadaval.
