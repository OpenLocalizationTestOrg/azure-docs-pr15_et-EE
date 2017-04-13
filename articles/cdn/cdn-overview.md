<properties
    pageTitle="Azure'i CDN ülevaade | Microsoft Azure'i"
    description="Siit saate teada, mis on Azure sisu kohaletoimetamise võrk (CDN) ja kuidas seda kasutada esitamisel läbilaskevõimega sisu, vahemällu plekid ja staatiliseks sisuks."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="09/30/2016"
    ms.author="casoper"/>

# <a name="overview-of-the-azure-content-delivery-network-cdn"></a>Azure'i Sisuedastusvõrgud (CDN) ülevaade

> [AZURE.NOTE] Dokumendis kirjeldatakse, mis on Azure sisu kohaletoimetamise võrk (CDN), kuidas see töötab ja Azure CDN iga toote funktsioone.  Kui soovite selle teabe vahele jätta ja liikuda kohe õpetus on CDN lõpp-punkti loomiseks, leiate [Azure'i CDN abil](cdn-create-new-endpoint.md).  Kui soovite näha praeguse CDN sõlm asukohtade loendit, lugege teemat [Azure CDN POP asukohad](cdn-pop-locations.md).

Azure'i sisu kohaletoimetamise võrk (CDN) vahemälu staatilise veebisisu strateegiliselt paigutatud kohtades videosisu kasutajatele maksimaalse läbilaskevõime ette.  Funktsiooni CDN pakub arendajate latentsusajaga videosisu, vahemällu füüsilise sõlmed sisu kogu maailmas globaalne lahendust. 

CDN vahemälu veebisaidi vara kasutamise eelised on järgmised.

- Parema jõudluse ja kasutajale kogemus lõppkasutajad, eriti siis, kui rakenduste abil, kus mitu vormimallikujundaja laadimiseks sisu on vaja.
- Suur skaleerimist paremini toime hetke suure koormuse, nt toote käivitamine sündmuse algust.
- Kasutaja taotlused levitamise ja serveeritakse sisu Edge'i serverid, saadetakse vähem liiklust päritolu.


## <a name="how-it-works"></a>Kuidas see toimib

![CDN ülevaade](./media/cdn-overview/cdn-overview.png)

1. Kasutaja (Alice) taotleb (nimetatakse ka vara) faili URL-i abil teisiti domeeni nimi, näiteks `<endpointname>.azureedge.net`.  DNS-i marsruudib taotluse parimate punkt-, võrgusolekuteabe (POP) asukohta.  See on tavaliselt POP, mis on geograafiliselt lähemal kasutaja.

2. Kui Edge'i serverid pop ei ole fail oma vahemälu, edge server nõuab faili päritolu.  Päritolu võib olla Azure Web App, Azure'i pilveteenuses, Azure Storage konto või mis tahes avalikult juurdepääsetava veebiserver.

3. Päritolu tagastab faili serva server, sh valikuline HTTP päised, mis kirjeldab faili aeg-to-Live (TTL).

4. Serva server salvestab faili ja tagastab faili algse planeerimisüksused (Alice).  Faili jääb vahemällu talletatud perimeeterserveris kuni TTL aegub.  Kui päritolu ei määra TTL-default TTL on seitse päeva.

5. Täiendavate kasutajate taotleda sama faili selle sama URL-i abil, siis ja selle sama POP suunata.

6. Kui TTL faili jaoks pole aegunud, tagastab Edge'i serveri faili vahemälu.  Tulemuseks kiirem, paremini kasutusvõimalused.


## <a name="azure-cdn-features"></a>Azure'i CDN-funktsioonid

On kolme Azure'i CDN tooted: **Azure'i CDN Standard Akamai kaudu**, **Azure'i CDN Standard Verizon**ja **Azure CDN Premium Verizon**.  Järgmises tabelis on loetletud funktsioonid, mis on saadaval iga toote.

|       | Standardse Akamai | Standardse Verizon | Premium Verizon |
|-------|-----------------|------------------|-----------------|
| Lihtne integreerimine Azure teenuste nt [salvestusruumi](cdn-create-a-storage-account-with-cdn.md), [Pilveteenustega](cdn-cloud-service-with-cdn.md), [Veebirakenduste](../app-service-web/cdn-websites-with-cdn.md)ja [Media Services](../media-services/media-services-portal-manage-streaming-endpoints.md) | **& #x2713;** | **& #x2713;** | **& #x2713;**|
| Juhtimise [REST API-ga](https://msdn.microsoft.com/library/mt634456.aspx), [.NET](./cdn-app-dev-net.md), [Node.js](./cdn-app-dev-node.md)või [PowerShelli](./cdn-manage-powershell.md)kaudu. | **& #x2713;** | **& #x2713;** | **& #x2713;** |
| HTTPS-i tugi | **& #x2713;** | **& #x2713;** | **& #x2713;** |
| Laadi tasakaalustamiseks | **& #x2713;** | **& #x2713;** | **& #x2713;** |
| [DDOS](https://www.us-cert.gov/ncas/tips/ST04-015) kaitse | **& #x2713;** | **& #x2713;** | **& #x2713;** |
| IPv4/IPv6 kahe-virnas | **& #x2713;** | **& #x2713;** | **& #x2713;** |
| [Kohandatud domeeni nime tugi](cdn-map-content-to-custom-domain.md) | **& #x2713;** | **& #x2713;** | **& #x2713;** |
| [Päringu stringi vahemällu talletamine](cdn-query-string.md) | **& #x2713;** | **& #x2713;** | **& #x2713;** |
| [Geo filtreerimine](cdn-restrict-access-by-country.md) |  | **& #x2713;** | **& #x2713;** |
| [Kiire likvideerite](cdn-purge-endpoint.md) | **& #x2713;** | **& #x2713;** | **& #x2713;** |
| [Varade eelse laadimine](cdn-preload-endpoint.md) |  | **& #x2713;** | **& #x2713;** |
| [Core analytics](cdn-analyze-usage-patterns.md) |  | **& #x2713;** | **& #x2713;** |
| [HTTP/2 tugi](https://msdn.microsoft.com/library/mt762901.aspx) | **& #x2713;**  |  |  |
| [Täiustatud HTTP aruanded](cdn-advanced-http-reports.md) | | | **& #x2713;** |
| [Reaalajas statistika](cdn-real-time-stats.md) | | | **& #x2713;** |
| [Reaalajas teatised](cdn-real-time-alerts.md) | | | **& #x2713;** |
| [Kohandatavad, reegli sisu kohaletoimetamise mootor](cdn-rules-engine.md) | | | **& #x2713;** |
| Vahemälu/päise sätted ( [reeglite mootori](cdn-rules-engine.md)abil)  | | | **& #x2713;** |
| URL-i ümbersuunamise/ümberkirjutamine ( [reeglite mootori](cdn-rules-engine.md)abil) | | | **& #x2713;** |
| Mobiilsideseadme reegleid ( [reeglite mootori](cdn-rules-engine.md)abil)  | | | **& #x2713;** |

>[AZURE.TIP] Kas soovite näha Azure'i CDN funktsioon?  [Andke meile tagasisidet](https://feedback.azure.com/forums/169397-cdn)! 

## <a name="next-steps"></a>Järgmised sammud

Alustamine CDN leiate [Azure'i CDN abil](./cdn-create-new-endpoint.md).

Kui olete olemasolevate CDN klient, saate hallata kohe CDN lõpp-punktide [Microsoft Azure portaali](https://portal.azure.com) kaudu või [PowerShelli abil](cdn-manage-powershell.md).

CDN tegelikkuses vaatamiseks vaadake selle [video meie seansi koostamine 2016](https://azure.microsoft.com/documentation/videos/build-2016-leveraging-the-new-azure-cdn-apis-to-build-wicked-fast-applications/).

Saate teada, kuidas Azure'i CDN [.net-i](./cdn-app-dev-net.md) või [Node.js](./cdn-app-dev-node.md)automatiseerimiseks.

Hindade teavet, lugege teemat [CDN hinnad](https://azure.microsoft.com/pricing/details/cdn/).
