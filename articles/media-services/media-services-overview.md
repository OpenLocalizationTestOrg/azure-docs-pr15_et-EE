<properties 
    pageTitle="Azure Media Servicesi ülevaade ja levinud stsenaariumi | Microsoft Azure'i" 
    description="Selles teemas antakse ülevaade Azure Media Servicesi" 
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
    ms.topic="hero-article" 
    ms.date="10/12/2016"
    ms.author="juliako;anilmur"/>

#<a name="azure-media-services-overview-and-common-scenarios"></a>Azure Media Servicesi ülevaade ja Levinumad stsenaariumid

Microsoft Azure Media Servicesi on laiendatav pilvepõhine platvorm, mis võimaldab arendajatel luua muutuv meedia haldus ja kohaletoimetamise rakendused. Media Servicesi põhineb REST API-d, mis võimaldavad teil turvaliselt üles laadida, salvestada, kodeerida ja paketti video või heli sisu kohaletoimetamiseks nõudmisel ja reaalajas streaming erinevate klientidega (nt TV, arvuti ja mobiilsideseadmete jaoks).

Saate koostada-lõpuni töövoogude abil täielikult Media Services. Samuti saate kasutada muude tootjate komponendid osa oma töövoo. Näiteks kodeerida kolmanda osapoole kodeerija abil. Seejärel üles laadida, kaitsta, paketti esitamisel Media Servicesi kaudu.

Kui soovite, et voo sisu live või nõudmisel sisu esitamisel. Selles teemas kirjeldatakse Levinumad stsenaariumid pakkuda oma sisu [live](media-services-overview.md#live_scenarios) või [nõudmisel](media-services-overview.md#vod_scenarios). Teema on lingid ka muud asjakohased teemad.

## <a name="sdks-and-tools"></a>SDK-d ja tööriistad

Media Services lahenduste loomiseks saate kasutada:

- [Media Servicesi REST API-ga](https://msdn.microsoft.com/library/azure/hh973617.aspx)
- Üks saadaval kliendi SDK-d.
- [Azure Media Servicesi SDK .net-i jaoks](https://github.com/Azure/azure-sdk-for-media-services)
- [Azure'i SDK Java](https://github.com/Azure/azure-sdk-for-java),
- [Azure'i PHP SDK](https://github.com/Azure/azure-sdk-for-php),
- [Azure Media Servicesi Node.js jaoks](https://github.com/michelle-becker/node-ams-sdk/blob/master/lib/request.js) (See on Node.js SDK-Microsofti versiooni. See haldab ühenduse ja praegu on 100% ulatus AMS API-d).
- Olemasoleva Tööriistad:
- [Azure'i portaal](https://portal.azure.com/)
- [Azure Media Services Explorer](https://github.com/Azure/Azure-Media-Services-Explorer) (Azure Media Services Explorer (AMSE) on soovitud Winforms / C# rakendus Windows)

##<a name="media-services-learning-paths"></a>Media Servicesi Õppeteed

Saate vaadata AMS õppeteemad siit:

- [AMS Live Streaming töövoo](https://azure.microsoft.com/documentation/learning-paths/media-services-streaming-live/)
- [AMS Streaming töövoo nõudmisel](https://azure.microsoft.com/documentation/learning-paths/media-services-streaming-on-demand/)

##<a name="prerequisites"></a>Eeltingimused

Azure Media Servicesi kasutamise alustamiseks peaksid olema järgmised:

3. Azure'i konto. Kui teil pole kontot, saate luua tasuta prooviversiooni konto vaid paar minutit. Lisateavet leiate teemast [Azure tasuta prooviversioon](https://azure.microsoft.com).
2. Azure Media Servicesi konto. Azure'i portaalis, .net-i või REST API abil saate luua Azure Media Servicesi konto. Lisateavet leiate teemast [Loo konto](media-services-portal-create-account.md).
3. (Valikuline) Saate häälestada arenduskeskkond. Valige oma arenduskeskkond .net-i või REST API-ga. Lisateavet leiate teemast [keskkonna häälestamine](media-services-dotnet-how-to-use.md).

Samuti saate teada, kuidas ühendada programmiliselt [ühenduse loomine](media-services-dotnet-connect-programmatically.md).
4. (Soovitatav) Eraldada skaala üks või mitu üksust. Soovitatav on eraldada rakenduste tootmiskeskkonnas skaala üks või mitu üksust.   Lisateavet leiate teemast [haldamine streaming lõpp-punktid](media-services-portal-manage-streaming-endpoints.md).

##<a name="concepts-and-overview"></a>Põhimõtet ja ülevaade

Teemast Azure Media Servicesi põhimõtet, [põhimõtet](media-services-concepts.md).

Kuidas sari, mis tutvustab teile kõik põhilised Azure Media Servicesi, leiate teemast [Azure Media Services samm-sammult õpetused](https://docs.com/fukushima-shigeyuki/3439/english-azure-media-services-step-by-step-series). See sari on hea ülevaade põhimõtet ja AMSE tööriist kasutab näidata AMS tööülesanded. Pange tähele, et AMSE tööriist on Windowsi tööriist. See tööriist toetab enamik võite saavutada programmiliselt [AMS SDK .net-i](https://github.com/Azure/azure-sdk-for-media-services), [Azure'i SDK Java](https://github.com/Azure/azure-sdk-for-java)või [Azure PHP SDK](https://github.com/Azure/azure-sdk-for-php)tööülesanded.

##<a id="vod_scenarios"></a>Pakkuda meediumi tellitav Azure Media Servicesi koos: Levinumad stsenaariumid ja tööülesanded

Selles jaotises kirjeldatakse Levinumad stsenaariumid ja asjakohaste teemade lingid. Järgmisel joonisel on esitatud põhi osad Media Servicesi platvormi, mis on seotud saavutamisel nõudmisel sisu. 

![VoD töövoo](./media/media-services-video-on-demand-workflow/media-services-video-on-demand.png)


###<a name="protect-content-in-storage-and-deliver-streaming-media-in-the-clear-non-encrypted"></a>Salvestusruumi sisu kaitsta ja pakkuda meediumi selge (krüptitud)

1. Kõrge kvaliteediga Standard faili üleslaadimine arvesse vara.
    
    Soovitatav on rakendada storage krüptimise võimalus oma vara kaitsmiseks sisu üles ja samal ajal ülejäänud salvestusruumi.
 
1. Kodeerida kohandatava bitrate MP4 failide kogum. 

    Soovitatav on rakendada salvestusruumi krüptimise suvand väljundi vara kaitsmiseks ülejäänud oma sisu.
    
1. Konfigureerida varade kohaletoimetamise poliitika (kasutatakse dünaamilise pakendit). 
    
    Kui teie vara on krüptitud salvestusruumi, **tuleb** teil konfigureerida varade kohaletoimetamise poliitika. 

1. Avaldamine on nõudmisel locator loomisega vara.

    Veenduge, et on vähemalt üks streaming reserveeritud üksus, millest soovite voogesituseks streaming lõpp-punkti.

1. Voo avaldatud sisu.

###<a name="protect-content-in-storage-deliver-dynamically-encrypted-streaming-media"></a>Salvestusruumi sisu kaitsta, dünaamiliselt krüptitud meediumi esitamine  

Dünaamiline krüptimise saama, peate esmalt saada vähemalt üks streaming reserveeritud ühiku streaming lõpp-punkti, millest soovite voona krüptitud sisu.

1. Kõrge kvaliteediga Standard faili üleslaadimine arvesse vara. Vara storage krüptimise võimalus rakendada.
1. Kodeerida kohandatava bitrate MP4 failide kogum. Varade väljundi storage krüptimise võimalus rakendada.
1. Vara, mida soovite krüptida dünaamiliselt esituse ajal sisu krüptovõtme loomine.
2. Konfigureerimine sisu võtme autoriseerimine poliitika.
1. Konfigureerida varade kohaletoimetamise poliitika (kasutatakse dünaamilise pakendit ja dünaamiline krüptimise).
1. Avaldamine on nõudmisel locator loomisega vara.
1. Voo avaldatud sisu. 

###<a name="use-media-analytics-to-derive-actionable-insights-from-your-videos"></a>Kasutada meediumi Analytics vaidlustatavad ülevaateid tulenevad videoid 

Meediumi Analytics on kogumi, kõne- ja nägemispuudega komponendid, mis lihtsustavad ettevõtted ja ettevõtetele tuletada vaidlustatavad ülevaateid oma videofaile. Lisateavet leiate teemast [Azure Media Services Analytics ülevaade](media-services-analytics-overview.md).

1. Kõrge kvaliteediga Standard faili üleslaadimine arvesse vara.
2. Kasutage ühte meediumi analüüsiteenuste töödelda videoid.
    
    - **Indekseerija** – [Azure Media indekseerija 2 protsessi videod](media-services-process-content-with-indexer2.md)
    - **Liikuv stoppkaader** – [Liikuv stoppkaader meediumifailide Azure Media Liikuv stoppkaader](media-services-hyperlapse-content.md)
    - **Liikumistee tuvastamise** - [Liikumise tuvastamine Azure Media Analyticsi](media-services-motion-detection.md).
    - **Näo tuvastamise ja näo emotsioone** – [näo ja Azure Media Analyticsi emotsioon tuvastamise](media-services-face-and-emotion-detection.md).
    - **Video kokkuvõtet** – [Kasutage Azure Media Video pisipildid Loo Video kokkuvõtet](media-services-video-summarization.md)
3. Meediumi Analytics meediumi protsessorite aedvili MP4 või JSON failide. Kui meediumi protsessor toodeti MP4 faili, saate faili järk-järgult alla. Kui meediumi protsessor toodeti JSON faili, saate laadida alla faili Azure'i bloobimälu. 


###<a name="deliver-progressive-download"></a>Esitamisel järk-järgult allalaadimine 

1. Kõrge kvaliteediga Standard faili üleslaadimine arvesse vara.
1. Kodeerida ühe MP4 faili.
1. Üldine ressursiaadress nõudmisel või SAS loomisega avaldada vara.

    Kui Kasuta nõudmisel locator, veenduge, et olema vähemalt üks streaming reserveeritud ühiku streaming lõpp-punkti, millest plaanite järk-järgult sisu alla laadida.

    Kui SAS locator, sisu on alla laaditud Azure'i bloobimälu. Sel juhul pole vaja on voogesituse reserveeritud üksused.
  
1. Järk-järgult sisu alla laadida.

##<a id="live_scenarios"></a>Pakkuda reaalajas Streaming sündmuste Azure Media Servicesi

Kui te töötate Live Streaming järgmised komponendid on tavaliselt seotud:

- Kaamera, mida kasutatakse sündmuse leviedastus.
- Reaalajas video kodeerija, mis teisendab signaale kaamera reaalajas streaming teenuse saadetavatele voogu.

Soovi korral saate reaalajas mitu korda sünkroonitud kodeerimisseadmetest. Teatud kriitiliste live sündmusi, et nõudmisel väga kõrge-saadavus ja kogemusi, on soovitatav tööle aktiivne aktiivne liigsete kodeerimisseadmetest koos kellaajaga synchronizationto saavutada sujuvalt Tõrkesiirde kaotamata andmeid.
- Reaalajas streaming teenus, mis võimaldab teil teha järgmist:

- neelata reaalajas sisu abil erinevate reaalajas streaming protokollide (nt RTMP või sujuva voogesituse)
- (valikuline) kodeerida oma voo kohandatava bitrate voo
- teie reaalajas voo eelvaade
- salvestamine ja hoida sissevõetava sisu tuleb voona hiljem (Video-on-Demand)
- esitamisel kaudu levinud streaming protokollid (nt MPEG MÕTTEKRIIPSU, sujuv, HLS, HDS) sisu otse oma klientidele, või soovite sisu kohaletoimetamise võrk (CDN-ID) põhjalikumaks jaotuse.


**Microsoft Azure Media Servicesi** (AMS) võimaldab neelata, kodeerida, eelvaade, talletada ja esitada oma reaalajas streaming sisu.

Sisu esitamisel kliendid teie eesmärk on pakkuda kõrge kvaliteediga video erinevate seadmete erinevate võrgu tingimustel. Olge ettevaatlik kvaliteedi-ja võrgu, kasutage reaalajas kodeerimisseadmetest kodeerida oma voos, et mitme bitikiirusega (kohandatava bitrate) videovoo.  Olge ettevaatlik streaming erinevates seadmetes, abil Media Servicesi [dünaamiline pakendit](media-services-dynamic-packaging-overview.md) dünaamiliselt paketti uuesti oma voo erinevate protokollidega. Media Services toetab kohaletoimetamise järgmised kohandatava bitrate voogesituse tehnoloogiaid: HTTP Live Streaming (HLS), sujuva voogesituse, MPEG MÕTTEKRIIPSU ja HDS (Adobe PrimeTime Access kes ainult) jaoks.

Azure Media Servicesi, **kanalid**, **programmide**ja **StreamingEndpoints** hakkama kõik reaalajas streaming funktsioonid, sh neelata, vorming, DVR, turvalisus, skaleeritavus ja koondamise.

**Kanali** tähistab müügivõimaluste töötlemiseks reaalajas streaming sisu. Kanali saab saata live voogu sisestada järgmiselt:

- Kohapealses live kanal, mis on konfigureeritud lubama **läbiv** kohaletoimetamise kodeerija saadab mitme bitikiirusega **RTMP** või **Sujuva voogesituse** (killustatud MP4). **Läbiv** kohaletoimetamise on kui sissevõetava voogu läbida **kanali**s ilma edasiseks töötlemiseks. Saate kasutada järgmisi reaalajas kodeerimisseadmetest, mis väljasta mitme bitikiirusega sujuva voogesituse: oksiidvormis Envivio, Cisco.  Järgmised reaalajas kodeerimisseadmetest väljund RTMP: Adobe Flash Live, Telestreami Wirecast ja Tricaster transcoders.  Reaalajas kodeerija Lisaks saate saata ühe bitrate voo kanal, mis pole lubatud reaalajas kodeerimine, kuid mis ei ole soovitatav. Nõudmisel pakub Media Servicesi voo klientidele.

>[AZURE.NOTE] Läbiv meetodil on kõige ökonoomsem viis live streaming teete mitu sündmused pikka aega, kui teil on juba investeerida kohapealse kodeerimisseadmetest. Lisateavet leiate teemast [hinnad](/pricing/details/media-services/) üksikasjad.

- Kohapealse reaalajas kodeerija saadab ühe bitikiirusega voo kanal, mis on lubatud teha reaalajas kodeerimine Media teenustega ühes järgmises vormingus: RTP (MPEG-TS), RTMP või sujuva voogesituse (killustatud MP4). Kanali seejärel sooritab reaalajas kodeerimine sissetulevad ühe bitrate voo mitme bitrate (kohandatava) videovoo abil. Nõudmisel pakub Media Servicesi voo klientidele.


###<a name="working-with-channels-that-receive-multi-bitrate-live-stream-from-on-premises-encoders-pass-through"></a>Töötamine kanalit, mille mitme bitikiirusega reaalajas voo saadud asutusesisese kodeerimisseadmetest (läbiv)

Järgmisel joonisel on esitatud põhi osad AMS platvormi, mis on seotud **läbiv** töövoos.

![Reaalajas töövoog][live-overview2]

Lisateabe saamiseks lugege teemat [töötamine kanalite selle vastuvõtu mitme bitikiirusega Live voo kohapealse kodeerimisseadmetest kaudu](media-services-live-streaming-with-onprem-encoders.md).

###<a name="working-with-channels-that-are-enabled-to-perform-live-encoding-with-azure-media-services"></a>Kanalid, mis on lubatud teha reaalajas kodeerimine Azure Media Servicesi koos töötamine

Järgmisel joonisel on esitatud põhi osad AMS platvormi, mis on seotud Live Streaming töövoo, kus kanali on lubatud teha reaalajas kodeerimine meediumi teenustega.

![Reaalajas töövoog][live-overview1]

Lisateavet leiate teemast [kanalid, mis on lubatud teha Live kodeeringus Azure Media Servicesi koos töötamine](media-services-manage-live-encoder-enabled-channels.md).


##<a name="consuming-content"></a>Sisu tarbimine

Azure Media Servicesi tööriistade, peate looma rikkalike, dünaamiliste Playeri klientrakendustes enamik platvormid, sh: iOS-seadmetes, Android-seadmetes, Windowsi, Windows Phone, Xbox ja Set-top ruudud. Teema sisaldab linke SDK-d ja Playeri raamistiku, mille abil saate arendada oma kliendi rakendused, mida saab kasutada Media Servicesi kaudu videote voogesitamise meediumi.

[Videopleieri rakenduste arendamise](media-services-develop-video-players.md)

##<a name="enabling-azure-cdn"></a>Azure'i CDN lubamine

Media Services toetab integreerimine Azure CDN. Azure'i CDN lubamise kohta leiate teavet teemast [haldamine Streaming lõpp-punktid Media Services konto](media-services-portal-manage-streaming-endpoints.md).

##<a name="scaling-a-media-services-account"></a>Skaleerimist Media Servicesi konto

Muudate **Media Servicesi** , määrates **Streaming reserveeritud** ja **Kodeering reserveeritud üksused** , mille soovite oma kontot olema ette valmistatud ja arvu.

Saate ka skaala Media Servicesi kontole, salvestusruumi kontod lisada. Piiratud 500 TB salvestusruumi iga konto. Lisaks vaikimisi piirangud salvestusruumi laiendamiseks saate valida mitme salvestusruumi konto lisamiseks Media Servicesi ühe konto.

[See](media-services-portal-scale-streaming-endpoints.md) teema oluline teemade lingid.

##<a name="support"></a>Tugi

[Azure'i tugi](https://azure.microsoft.com/support/options/) pakub Azure, sh Media Servicesi suvandid.

##<a name="provide-feedback"></a>Tagasiside saatmine

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="service-level-agreement-sla"></a>Teenuseleping (SLA)

- Media Services kodeeringus, me tagada 99,9%-saadavus REST API tehingute.
- Jaoks Streaming, meil on edukalt hooldustaotlused olemasoleva meedia sisu kättesaadavus 99,9% garantii vähemalt üks Streaming ühiku ostmisel.
- Live kanalid me tagada, et töötab kanalite on välise ühenduvuse vähemalt 99,9% ajast.
- Sisu kaitse, on tagada, et saaksime edukalt täita olulisi taotlusi vähemalt 99,9% ajast.
- Jaoks indekseerija, saame edukalt teenuse indekseerija tööülesannete määramised koos mõne kodeering reserveeritud ühiku 99,9% ajast.

Lisateavet leiate teemast [Microsoft Azure'i SLA](https://azure.microsoft.com/support/legal/sla/).

<!-- Images -->
[overview]: ./media/media-services-overview/media-services-overview.png
[vod-overview]: ./media/media-services-video-on-demand-workflow/media-services-video-on-demand.png
[live-overview1]: ./media/media-services-live-streaming-workflow/media-services-live-streaming-new.png
[live-overview2]: ./media/media-services-live-streaming-workflow/media-services-live-streaming-current.png
 
