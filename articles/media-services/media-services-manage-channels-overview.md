<properties 
    pageTitle="Ülevaade Live aurutatud Azure Media Servicesi kaudu | Microsoft Azure'i" 
    description="Selles teemas antakse ülevaade reaalajas aurutatud Azure Media Servicesi kaudu." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="ne" 
    ms.topic="article" 
    ms.date="10/17/2016"
    ms.author="juliako"/>

#<a name="overview-of-live-steaming-using-azure-media-services"></a>Ülevaade Live aurutatud Azure Media Servicesi kaudu

##<a name="overview"></a>Ülevaade

Reaalajas streaming sündmusega Azure Media Servicesi esitamisel järgmised komponendid on tavaliselt seotud:

- Kaamera, mida kasutatakse sündmuse leviedastus.
- Reaalajas video kodeerija, mis teisendab signaale kaamera reaalajas streaming teenuse saadetavatele voogu.

    Soovi korral saate reaalajas mitu korda sünkroonitud kodeerimisseadmetest. Teatud kriitiliste live sündmused nõudmisel väga kõrge-saadavus ja kogemusi, soovitatav tööle aktiivne aktiivne liigsete kodeerimisseadmetest aja sünkroonimise saavutamiseks sujuvalt Tõrkesiirde kaotamata andmeid.
- Reaalajas streaming teenus, mis võimaldab teil teha järgmist:
    
    - neelata reaalajas sisu abil erinevate reaalajas streaming protokollide (nt RTMP või sujuva voogesituse)
    - (valikuline) kodeerida oma voo kohandatava bitrate voo
    - teie reaalajas voo eelvaade
    - salvestamine ja hoida sissevõetava sisu tuleb voona hiljem (Video-on-Demand)
    - esitamisel kaudu levinud streaming protokollid (nt MPEG SIDEKRIIPSU, sujuv, HLS, HDS) sisu otse oma klientidele, või soovite sisu kohaletoimetamise võrk (CDN-ID) põhjalikumaks jaotuse.


**Microsoft Azure Media Servicesi** (AMS) võimaldab neelata, kodeerida, eelvaade, talletada ja esitada oma reaalajas streaming sisu.

Sisu esitamisel kliendid teie eesmärk on pakkuda kõrge kvaliteediga video erinevate seadmete erinevate võrgu tingimustel. Kasutage selleks kodeerida oma voos, et mitme bitikiirusega (kohandatava bitrate) videovoo reaalajas kodeerimisseadmetest.  Olge ettevaatlik streaming erinevates seadmetes, abil Media Servicesi [dünaamiline pakendit](media-services-dynamic-packaging-overview.md) dünaamiliselt paketti uuesti oma voo erinevate protokollidega. Media Services toetab kohaletoimetamise järgmised kohandatava bitrate voogesituse tehnoloogiaid: HTTP Live Streaming (HLS), sujuva voogesituse, MPEG MÕTTEKRIIPSU ja HDS (Adobe PrimeTime Access kes ainult) jaoks.

Azure Media Servicesi, **kanalid**, **programmide**ja **StreamingEndpoints** hakkama kõik reaalajas streaming funktsioonid, sh neelata, vorming, DVR, turvalisus, skaleeritavus ja koondamise.

**Kanali** tähistab müügivõimaluste töötlemiseks reaalajas streaming sisu. Kanali saab saata live voogu sisestada järgmiselt:

- Kohapealses live kanal, mis on konfigureeritud lubama **läbiv** kohaletoimetamise kodeerija saadab mitme bitikiirusega **RTMP** või **Sujuva voogesituse** (killustatud MP4). **Läbiv** kohaletoimetamise on kui sissevõetava voogu läbida **kanali**s ilma edasiseks töötlemiseks. Saate kasutada järgmisi reaalajas kodeerimisseadmetest, mis väljasta mitme bitikiirusega sujuva voogesituse: oksiidvormis Envivio, Cisco.  Järgmised reaalajas kodeerimisseadmetest väljund RTMP: Adobe Flash Media Live kodeerija (FMLE), Telestreami Wirecast ja Tricaster transcoders.  Reaalajas kodeerija Lisaks saate saata ühe bitrate voo kanal, mis pole lubatud reaalajas kodeerimine, kuid mis ei ole soovitatav. Nõudmisel pakub Media Servicesi voo klientidele.

    >[AZURE.NOTE] Läbiv meetodil on kõige ökonoomsem viis live streaming teete mitu sündmused pikka aega, kui teil on juba investeerida asutusesisese kodeerimisseadmetest. Lisateavet leiate teemast [hinnad](/pricing/details/media-services/) üksikasjad.
    
    
- Kohapealse reaalajas kodeerija saadab ühe bitikiirusega voo kanal, mis on lubatud teha reaalajas kodeerimine Media teenustega ühes järgmises vormingus: RTMP või sujuva voogesituse (killustatud MP4). RTP (MPEG-TS) toetatakse ka, kui teil on spetsiaalne ühenduse Azure'i andmekeskuse. Järgmised reaalajas kodeerimisseadmetest RTMP väljund on teada, et seda tüüpi kanalite töötamine: Telestreami Wirecast, FMLE. Kanali seejärel sooritab reaalajas kodeerimine sissetulevad ühe bitrate voo mitme bitrate (kohandatava) videovoo abil. Nõudmisel pakub Media Servicesi voo klientidele.


Alates väljaandesse Media Services 2.10 kanali loomisel, saate määrata sellest, kuidas soovite oma kanali Sisestuskeel voo vastu võtta ning kas soovite kanali reaalajas kodeerimine voo sooritamiseks. Teil on kaks võimalust.

- **Ükski** (läbiv) – määrata selle väärtuse, kui kavatsete kasutada kohapealse reaalajas kodeerija, mis väljund mitme bitikiirusega voo (läbiv voo). Sel juhul sissetuleva voo läbi väljund ilma mis tahes kodeeringus. See on enne 2.10 kanali toimimist.  

- **Standard** – valige see väärtus, kui kavatsete kasutada Media Servicesi kodeerida ühe bitrate reaalajas voo mitme bitikiirusega voog. See meetod on väiksema ülespoole kiiresti harv sündmuste jaoks. Pange tähele, et arvelduse mõju reaalajas kodeeringus ja tuleks meeles pidada, et jättes reaalajas kodeering kanali "Töötab" olekus tekivad arve kulude.  Saate kohe peatada oma töötava kanalite kui teie live streaming sündmus on lõpule jõudnud, lisatasu kord tunnis kulude vältimiseks on soovitatav. 

##<a name="comparison-of-channel-types"></a>Kanali tüüpi võrdlus

Järgmises tabelis on ära toodud võrdlus toetatud Media Servicesi kanali kahte tüüpi juhend

Funktsioon|Läbiv kanal|Standard kanal
---|---|---
Ühe bitrate sisendit kodeeritakse mitme bitikiirus pilveteenuses|Ei|Jah
Suurim lubatud eraldusvõime kihtide arv|1080p, 8 kihid, 60 + sekundis|720p, 6 kihid, 30 sekundis
Sisestuskeel Protokollid|RTMP sujuva voogesituse|RTMP, sujuva voogesituse ja RTP
Hind|[Lehe hinnad](/pricing/details/media-services/) ja klõpsake vahekaardil "Live Video"|Vaadake [lehel hinnad](/pricing/details/media-services/) 
Maksimum Käitusaeg|24 x 7|8 tundi
Tahvlid lisamise tugi|Ei|Jah
Ad-signaal tugi|Ei|Jah
Läbiv CEA 608/708 pealdised|Jah|Jah
Võimalus lühike kioskites osakaal kanali taastamine|Jah|Ei (kanali alustab slating 6 + w/o sisendandmete sekundi pärast)
Ühtne Sisestuskeel GOPs tugi|Jah|Ei – sisendi peab olema kinnitatud 2sec GOPs
Muutuv paneeli määr sisendi tugi|Jah|Ei – sisendi peab olema kinnitatud paneeli kiirus.<br/>Muudatusi on lubatud, näiteks kõrge liikumistee stseenide ajal. Kuid kodeerija ei saa kukutage 10 raamid sekundis.
Automaatse-sulgemiseni kanalite kui sisestusteade kanali on kadunud|Ei|Pärast 12 tundi, kui ei ole programmi, kus töötab 

##<a name="working-with-channels-that-receive-multi-bitrate-live-stream-from-on-premises-encoders-pass-through"></a>Töötamine kanalit, mille mitme bitikiirusega reaalajas voo saadud asutusesisese kodeerimisseadmetest (läbiv)

Järgmisel joonisel on esitatud põhi osad AMS platvormi, mis on seotud **läbiv** töövoos.

![Reaalajas töövoog](./media/media-services-live-streaming-workflow/media-services-live-streaming-current.png)

Lisateabe saamiseks lugege teemat [töötamine kanalite selle vastuvõtu mitme bitikiirusega Live voo kohapealse kodeerimisseadmetest kaudu](media-services-live-streaming-with-onprem-encoders.md).

##<a name="working-with-channels-that-are-enabled-to-perform-live-encoding-with-azure-media-services"></a>Kanalid, mis on lubatud teha reaalajas kodeerimine Azure Media Servicesi koos töötamine

Järgmisel joonisel on esitatud põhi osad AMS platvormi, mis on seotud Live Streaming töövoo, kus kanali on lubatud teha reaalajas kodeerimine meediumi teenustega.

![Reaalajas töövoog](./media/media-services-live-streaming-workflow/media-services-live-streaming-new.png)

Lisateavet leiate teemast [kanalid, mis on lubatud teha Live kodeeringus Azure Media Servicesi koos töötamine](media-services-manage-live-encoder-enabled-channels.md).

##<a name="description-of-a-channel-and-its-related-components"></a>Kanali loomine ja selle seotud osade kirjeldus

###<a name="channel"></a>Kanali

Media Servicesi, [kanali](https://msdn.microsoft.com/library/azure/dn783458.aspx)s vastutate töötlemine reaalajas streaming sisu. Kanali pakub Sisestuskeel lõpp (neelata URL-i), et siis anda reaalajas transcoder. Kanali saab reaalajas Sisestuskeel voogu reaalajas transcoder ja teeb selle kättesaadavaks streaming ühe või mitme StreamingEndpoints kaudu. Kanalite pakuvad ka eelvaade lõpp (preview URL-i), mis abil saate eelvaade ja kinnitage oma voo enne edasiseks töötlemiseks ja kohaletoimetamisega.

Kanali loomisel võite saada neelata URL-i ja URL-i eelvaade. Idest saamiseks kanalit ei ole alustatud olekus olema. Kui olete valmis alustama, lükates andmete reaalajas transcoder kaudu sisse kanalit, tuleb kanalit luua. Kui reaalajas transcoder hakkab söömisega andmeid, saate vaadata oma voo.

Iga Media Servicesi konto võib sisaldada mitut kanalid, mitme programmi ja mitme StreamingEndpoints. Sõltuvalt läbilaskevõime ja turvalisuse erivajadustega, saate ühe või mitme pühendunud StreamingEndpoint teenused. Mis tahes StreamingEndpoint tõmmata mis tahes kanali kaudu.


###<a name="program"></a>Programm 

[Programm](https://msdn.microsoft.com/library/azure/dn783463.aspx) võimaldab teil määrata avaldamise ja lõikude reaalajas voo salvestusruumi. Kanalite haldamine programmid. Kanali ja programmi seos on väga sarnane traditsioonilise meedia, kus kanali on pidev vool sisu ja programmi on rakendatud mõni selle kanali Ajastatud sündmus.
Saate määrata tundide, mida soovite säilitada, seades atribuuti **ArchiveWindowLength** programmi salvestatud sisu. Seda väärtust saab määrata vähemalt 5 minutit kuni 25 tundi. 

ArchiveWindowLength näitab ka suurt hulka aja kliendid saavad taotleda ajas tagasi reaalajas praegust paigutust. Programmide käitamise üle määratud aja jooksul, kuid sisu, mis langeb pikkus akna taga pidevalt hüljatakse. See selle atribuudi väärtust määratleb ka kaua kliendi manifestid võivad kasvada.

Iga programm on seostatud vara. Programmi avaldada, peate looma locator, on seotud vara. Millel see locator võimaldab teil koostada streaming URL, mida saate sisestada oma klientidele.

Kanali toetab kuni kolm samaaegselt töötavad programmid nii, et saate luua mitu sama sissetuleva voo arhiivi. See võimaldab teil avaldada ja arhiivi erinevate osade sündmuse vastavalt vajadusele. Näiteks oma ettevõtte nõue on arhiivida 6 tunni programmi, kuid ainult viimase 10 minuti levisaadet edastada. Selle saavutamiseks, peate looma kaks samaaegselt töötavad programmid. Ühes programmis on seatud arhiivida sündmuse 6 tunni, kuid programm ei ole avaldatud. Muu programmi on seatud arhiivida 10 minutit ja see programm on avaldatud.


##<a name="billing-implications"></a>Arveldamine mõju

Kanali algab arveldus niipea, kui see on oleku üleminekuid, et "Töötab" API kaudu.  

Järgmine tabel näitab, kuidas kanali olekus vastendamine arvelduse olekus API ja Azure portaalis. Pange tähele, et riikide pisut erinev API ja portaali UX. Kui on kanali kaudu API olekus "Töötab" või "Valmis" või "Streaming" olekus Azure'i portaalis arveldamine on aktiivne.

Kanali kaudu saate põhjalikumaks arveldus peatamiseks peate lõpetama kanali kaudu API või Azure'i portaalis.
Teie vastutate teie kanalid peatamine, kui olete teinud kanali. Tõrge peatamiseks kanali tulemuseks jätkumise arveldamine.

>[AZURE.NOTE]Standardse kanalite töötamisel AMS automaatselt sulgemiseni mis tahes kanal, mis on endiselt olekus "Töötab" 12 tundi pärast Sisestuskeel kanali läheb kaotsi ja puuduvad töötavad programmid. Siiski saate endiselt, saadetakse teile arve kanali on kujul "Töötab" korda.

###<a id="states"></a>Kanali Ühendriike ja kuidas need kaart arvelduse režiim 

Kanali praegune olek. Võimalikud väärtused on järgmised.

- **Peatatud**. See on algse oleku kanali pärast selle loomist (välja arvatud automaatselt käivitada ka valitud portaalis.) Pole arveldamine toimub olekus. Selles olekus kanali atribuutide saab värskendada, kuid streaming pole lubatud.
- **Alates**. Kanali käivitamist. Pole arveldamine toimub olekus. Pole värskendusi või streaming on lubatud ajal olekus. Kui ilmneb tõrge, tagastab kanali peatatud olekus.
- **Töötab**. Kanali suudab töötlemine reaalajas voogu. See on nüüd arveldus kasutus. Peatage kanali vältimiseks täpsemaks arveldus. 
- **Peatamine**. Kanali peatatakse. Pole arveldamine toimub siirdamiseks olekus. Pole värskendusi või streaming on lubatud ajal olekus.
- **Kustutamine**. Kanali kustutatakse. Pole arveldamine toimub siirdamiseks olekus. Pole värskendusi või streaming on lubatud ajal olekus.

Järgmine tabel näitab, kuidas kanali olekus kaart arvelduse režiim. 
 
Kanali olek|Portaali Kasutajaliidese näidikud|See on arveldamine?
---|---|---
Käivitamine|Käivitamine|(Siirdamiseks riik)
Töötab|Olete valmis (töötavad programmid)<br/>või<br/>Streaming (vähemalt üks töötava programmi)|Jah
Peatamine|Peatamine|(Siirdamiseks riik)
Peatatud|Peatatud|Ei


##<a name="media-services-learning-paths"></a>Media Servicesi Õppeteed

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Tagasiside saatmine

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]



##<a name="related-topics"></a>Seotud teemad

[Azure Media Servicesi killustatud MP4 Live neelata määratlus](media-services-fmp4-live-ingest-overview.md)

[Kanalid, mis on lubatud teha Live kodeerimine Azure Media Servicesi koos töötamine](media-services-manage-live-encoder-enabled-channels.md)

[Kanalite selle vastuvõtu mitme bitikiirusega Live voo kaudu asutusesisese kodeerimisseadmetest töötamine](media-services-live-streaming-with-onprem-encoders.md)

[Kui ka piirangud](media-services-quotas-and-limitations.md).  

[Media Services mõisted](media-services-concepts.md)
