<properties
    pageTitle="Azure'i CDN serva jõudluse analüüs | Microsoft Azure'i"
    description="Microsoft Azure'i CDN serva sõlm jõudluse analüüsimine. Serva jõudluse Analytics pakub on CDN Varundustöö teavet liikluse ja läbilaskevõime kasutuse."
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
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="casoper"/>

# <a name="analyze-edge-node-performance-in-microsoft-azure-cdn"></a>Microsoft Azure'i CDN serva sõlm jõudluse analüüs

[AZURE.INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>Ülevaade
Serva jõudluse analytics pakub on CDN Varundustöö teavet liikluse ja läbilaskevõime kasutuse. Seda teavet saate kasutada siis trendid koostamiseks, mis võimaldavad teil saada teadmisi, kuidas oma varasid on on vahemällu talletatud ja teie klientideni. Omakorda see võimaldab teil vormi strateegia kohta, kuidas optimeerida oma sisu kohaletoimetamise ja probleemid tuleks käsitleda parem võimendada on CDN. Seetõttu, mitte ainult teil on võimalik andmete kohaletoimetamise jõudluse parandamiseks, kuid samuti saab CDN kulude vähendamise.

> [AZURE.NOTE] Kõigi aruannete kasutamine UTC/GMT esituses määramisel kuupäev ja kellaaeg.

## <a name="reports-and-log-collection"></a>Aruannete ja logide kogumist

Serva jõudluse Analytics mooduli koguda CDN tegevuse andmeid peab enne selle saate luua aruandeid selle. Selle saidikogumi protsess toimub üks kord päevas ja see hõlmab eelmisel päeval toimunud tegevuse. See tähendab, et statistika aruande tähistada päeva statistika valimi ajal see töödeldud ja ei pruugi sisaldada andmete komplekt tänasele päevale. Aruannete esmane ülesanne on hinnata jõudlust. Ta ei tohi kasutada arvelduse eesmärgil või täpse arvulise statistika.

> [AZURE.NOTE] Lähteandmed, millest luuakse serva jõudluse analüütiliste aruannete on saadaval 90 päeva jooksul.

## <a name="dashboard"></a>Armatuurlaua

Serva jõudluse Analytics armatuurlaua jälitab CDN praeguse ja ajalooliste liikluse läbi diagrammi ja statistika. Armatuurlaua abil saate tuvastada uuemaid ja pikaajalisi trende täitmise CDN-liikluse teie konto jaoks.

Armatuurlaua koosneb järgmistest elementidest:

* Interaktiivse diagrammi, mis võimaldab olulisemad mõõdikud ja trendide visualiseerimine.
* Ajaskaala, mis pakub pikaajaline mustrite tunnet olulisemad mõõdikud ja trendide.
* Võtme mõõdikute ja statistika teavet, kuidas parandab meie CDN võrgu liikluse mõõdetuna üldise jõudluse, kasutus ja tõhusust.

### <a name="accessing-the-edge-performance-dashboard"></a>Juurdepääs serva jõudluse armatuurlaud

1. Keelest CDN profiil nuppu **Halda** .

    ![CDN profiili blade haldamise nupp](./media/cdn-edge-performance/cdn-manage-btn.png)

    CDN haldusportaali avatakse.

2. Menüü **analüüsi** kursoriga ja seejärel kursoriga **Serva Perfomance Analytics** hüpik.  Klõpsake **armatuurlaual**.

    Serva sõlm analytics armatuurlaua kuvatakse.

### <a name="chart"></a>Diagrammi

Armatuurlaua sisaldab diagrammi, mis jälgib mõõdiku otse selle all kuvatavat ajaskaalal valitud ajavahemiku jooksul.  Ajaskaala graafikud üles viimase kahe aasta CDN tegevuse kuvatakse otse diagrammi all.

#### <a name="using-the-chart"></a>Diagrammi abil

* Vaikimisi on kaardistatud vahemälu tõhusust määr viimase 30 päeva.
* See diagramm luuakse andmete Eksemplarhaaval iga päev.
* Libistamisel üle päeva graafikul joon näitab kuupäeva ja mõõdiku väärtus kuupäeval.
* Klõpsake nuppu sisse-või väljalülitamiseks hele halli vertikaalne lindid, mis tähistavad nädalavahetustel hiirega diagrammile ülekatte nädalavahetustel esile tõsta. Seda tüüpi ülekatet on kasulik üle nädalavahetustel liikluse mustrite tuvastamiseks.
* Klõpsake vaate ühe aasta tagasi sisse-või väljalülitamiseks eelmise aasta tegevuse ülekatte sama aja jooksul kopeerida diagrammi. Seda tüüpi võrdlus annab ülevaate pikaajalise CDN mustreid. Diagrammi parempoolses ülanurgas sisaldab legend, mis näitab iga rea Graphi värvi kood.

#### <a name="updating-the-chart"></a>Diagrammi värskendamine

* Ajavahemiku: Tehke ühte järgmistest.
    * Valige soovitud piirkond ajaskaalat. Diagramm värskendatakse andmed, mis vastab valitud aja jooksul.
    * Topeltklõpsake soovitud diagrammi kuvada kõik saadaval ajaloolisi andmeid kuni kahe aasta jooksul.
* Meetermõõdustik: Klõpsake soovitud mõõdiku kõrval kuvatavat diagrammi ikooni. Koos vastavate meetermõõdustik andmeid värskendatakse diagrammi ja ajaskaala.


### <a name="key-metrics-and-statistics"></a>Võtme mõõdikute ja statistika

#### <a name="efficiency-metrics"></a>Tõhusust mõõdikud

Need mõõdikud eesmärk on näha, kas vahemälu tõhusust saab parandada. Peamine kasu tuletatud vahemälu tõhusust on:

* Mis võib tingida vähendatud koormus:
    * Web server töökindluse.
    * Vähendatud kohast.
* Täiustatud andmete kohaletoimetamise kiirendus Kuna rohkem taotlusi pakutakse otse soovitud CDN.

Väli | Kirjeldus
------|------------
Vahemälu tõhusust | Näitab, et andmete üle, mis kuvati vahemälust protsent. See argumendil mõõdud kui vahemällu talletatud versiooni nõutud sisu on taotlejad (nt veebibrauseri) otse CDN-ID (Edge'i serverid) kätte
Tulemus määr | Näitab taotlusi, mis olid serveeritud vahemälust protsent. See argumendil mõõdud kui on nõutud sisu vahemällu talletatud versiooni kätte oli otse CDN-ID (Edge'i serverid) taotlejad (nt veebibrauseri).
Remote baiti - pole vahemälu Config % | Näitab kätte oli origin serveritest CDN (Edge'i serverid), mis kuvatakse vahemälust tulemusena ära kasuta vahemälu funktsioon (HTTP reeglid mootor) liiklus protsent.
Remote baiti - aegunud vahemälu % | Aegunud sisu pikendamisega tulemusena näitab protsent liikluse, mis on kätte origin serveritest CDN-ID (Edge'i serverid).

#### <a name="usage-metrics"></a>Kasutus mõõdikud

Need mõõdikud eesmärk annab ülevaate järgmised kulude kärpimise Mõõdud:

* Minimeerimine kohast on CDN kaudu.
* Vähendamise CDN kulude vahemälu tõhusust ja tihendamise kaudu.

> [AZURE.NOTE] Arvude liikluse maht tähistavad liikluse oli kasutusel suhete ja protsentide arvutused, mis võidakse kuvada ainult osa kokku liikluse suuremahulise klientide.

Väli | Kirjeldus
------|------------
Ave baiti välja | Näitab Keskmine arv baiti üle iga taotlus kella (nt veebibrauseri) taotleja CDN-ID (Edge'i serverid).
Pole vahemälu Config bait määr | Näitab, et kella CDN-ID (Edge'i serverid) taotleja (nt veebibrauseri), mis kuvatakse vahemälust tõttu ära kasuta vahemälu funktsioon liikluse protsent.
Tihendatud bait määr | Näitab liikluse saadetud CDN-ID (Edge'i serverid) taotlejad (nt veebibrauseri) protsent tihendatud kujul.
Välja baiti | Näitab andmehulga baitides, mida taotleja (nt veebibrauseri) CDN-ID (Edge'i serverid) on kohale toimetatud.  
Baiti | Näitab andmehulga baitides, saadetakse taotlejad (nt veebibrauseri) CDN-ID (Edge'i serverid).
Baiti Remote | Näitab andmehulga baitides, saadetakse CDN-ID ja klientide origin serverid CDN-ID (Edge'i serverid).

#### <a name="performance-metrics"></a>Jõudluse mõõdikud

Need mõõdikud eesmärk on üldise CDN jõudluse oma liikluse jälgimiseks.

Väli | Kirjeldus
------|------------
Edasta määr | Näitab keskmiselt, mille sisu on üle soovitud CDN on taotleja.
Kestus | Näitab keskmiselt aeg millisekundites, mis kulus esitamisel vara on taotleja (nt veebibrauseri).
Tihendatud taotluse määr | Näitab tabamust, mis taotleja (nt veebibrauseri) CDN-ID (Edge'i serverid) on kohale toimetatud protsent tihendatud kujul.
4xx määr | Näitab tabamust, mis on loodud 4xx olekukoodi protsent.
5xx määr | Näitab tabamust loonud 5xx olekukoodi protsent.
Tabamust | Näitab, mitu taotlusi CDN sisu.

#### <a name="secure-traffic-metrics"></a>Turvaline liikluse mõõdikud

Need mõõdikud eesmärk on CDN jõudluse HTTPS-liikluse jälgimiseks.

Väli | Kirjeldus
------|------------
Turvaline vahemälu tõhusust | Näitab, et andmete üle HTTPS päringuid, mis olid serveeritud vahemälust protsent. See argumendil mõõdud kui on nõutud sisu vahemällu talletatud versiooni kätte oli otse CDN-ID (Edge'i serverid) taotlejad (nt veebibrauseri) https.
Turvaline edastamine määr | Näitab keskmiselt, mille sisu on üle CDN-ID (Edge'i serverid) taotlejad (nt web servers) https.
Turvaline kestus | Näitab keskmiselt aeg millisekundites, mis kulus esitamisel vara https on taotleja (nt veebibrauseri).
Turvaline tabamust | Näitab, mitu HTTPS päringuid CDN sisu jaoks.
Turvaline baiti välja | Näitab HTTPS-liikluse, baitides, mida taotleja (nt veebibrauseri) CDN-ID (Edge'i serverid) on kohale toimetatud.

## <a name="reports"></a>Aruanded

Iga aruande moodul sisaldab diagrammi ja statistikat erinevat tüüpi mõõdikute läbilaskevõime ja liikluse kasutamise kohta (nt HTTP olek koodide, vahemälu olek, taotleda URL-i jne). See teave võib kasutada selgitada põhjalikumalt kuidas sisu kätte oma klientidele ja täpsustada CDN käitumine andmete kohaletoimetamise jõudluse parandamiseks.

### <a name="accessing-the-edge-performance-reports"></a>Juurdepääs serva jõudlusearuannete

1. Keelest CDN profiil nuppu **Halda** .

    ![CDN profiili blade haldamise nupp](./media/cdn-edge-performance/cdn-manage-btn.png)

    CDN haldusportaali avatakse.

2. Menüü **analüüsi** kursoriga ja seejärel kursoriga **Serva Perfomance Analytics** hüpik.  Klõpsake **HTTP suur objekt**.

    Serva sõlm Kasutusanalüüsi aruannete ekraanil kuvatakse.

Aruanne | Kirjeldus
-------|------------
Igapäevane Kokkuvõte | Võimaldab vaadata iga päev liikluse trendide määratud aja jooksul. Klõpsake selle graafik iga riba tähistab kindla kuupäeva. Riba suurus näitab funktsiooni koguarv toimunud kuupäeval.
Kord tunnis Kokkuvõte | Võimaldab vaadata tunni liikluse trendide määratud aja jooksul. Klõpsake selle graafik iga riba tähistab ühe tunni teatud kuupäeval. Riba suurus näitab funktsiooni koguarv toimunud selle tunni jooksul.
Protokollid | Kuvab jaotus vahel HTTP ja HTTPS-protokolli. Rõngasdiagramm näitab iga tüüpi protokolli toimunud tabamust protsent.
HTTP meetodid | Võimaldab saamiseks, mis HTTP kasutatakse meetodite taotleda andmete. Tavaliselt on kõige levinumad HTTP taotluse meetodid toomine, pea ja POSTITA. Rõngasdiagramm näitab iga tüüpi HTTP päringu meetodit toimunud tabamust protsent.
URL-id | Sisaldab graafik, mis kuvab ülemine 10 nõutud URL. Riba kuvatakse iga URL-i. Riba pikkus näitab, mitu tabamust, mis ajavahemiku on loodud teatud URL-i hõlmatud aruanne. Ühe muutujaga statistika 100 ülemist nõutav URL kuvatakse see graafik all.
CNAMEs | Sisaldab graafik, mis kuvab 10 CNAMEs taotleda varad aja jooksul kasutatud funktsioone ja aruande ülaosas. Ühe muutujaga statistika 100 ülemist nõutav CNAMEs kuvatakse otse selle graafik.
Päritolu | Sisaldab graafik, mis kuvab ülemine 10 CDN või kliendi origin serverites, kust varad olid taotleda määratud aja jooksul. Ühe muutujaga statistika 100 ülemist nõutav CDN-ID või kliendi origin serverid kuvatakse otse selle graafik. Kliendi origin serverid tuvastatakse sellesse kausta nimi määratletud nimi.
Geo hüppab | Näitab, kui palju liiklust marsruuditakse kindla punkt-ja-kohalolek (POP) kaudu. Funktsiooni lühend tähistab POP meie CDN võrgus.
Kliendid | Sisaldab graafik, mis kuvab ülemine 10 klientide soovitud varad määratud aja jooksul. Selleks et aruandes, kõik kutsed, et sama IP-aadress on pärit loetakse sama kliendi. Ühe muutujaga statistika ülemise 100 klienti kuvatakse otse all see graafik. Aruande on kasulik määramiseks allalaadimine tegevuse mustrite ülemise kliendid.
Vahemälu olekud | Annab üksikasjalik jaotus vahemälu käitumine, mis võib esile lähenemisel parandamiseks üldine lõppkasutaja kogemus. Kuna parima jõudluse pärineb Vahemälutabamusi, saate optimeerida kohaletoimetamise andmeside minimeerimine vahemälu jätab ja aegunud Vahemälutabamusi.
ÜKSKI üksikasjad | Sisaldab graafik, mis kuvab ülemine 10 URL-ide vara, kus on märgitud vahemälu sisu värskus määratud aja jooksul. Top 100 URL-ide järgmist tüüpi varad statistika kuvatakse otse all see graafik.
CONFIG_NOCACHE üksikasjad | Sisaldab graafik, mis kuvab ülemine 10 URL-ide varad, mis olid vahemälust kliendi CDN konfiguratsiooni tõttu. Järgmist tüüpi varad olid serveeritud origin serverist. Top 100 URL-ide järgmist tüüpi varad statistika kuvatakse otse all see graafik.
UNCACHEABLE üksikasjad | Sisaldab graafik, mis kuvab ülemine 10 URL-ide varad, mis võivad vahemälust taotluse päise andmete tõttu. Top 100 URL-ide järgmist tüüpi varad statistika kuvatakse otse all see graafik.
TCP_HIT üksikasjad | Sisaldab graafik, mis kuvab ülemine 10 URL-ide varad, mis on saadaval kohe vahemälust. Top 100 URL-ide järgmist tüüpi varad statistika kuvatakse otse all see graafik.
TCP_MISS üksikasjad | Sisaldab graafik, mis kuvab ülemine 10 URL-ide vahemälu olek TCP_MISS varasid. Top 100 URL-ide järgmist tüüpi varad statistika kuvatakse otse all see graafik.
TCP_EXPIRED_HIT üksikasjad | Sisaldab graafik, mis kuvab ülemine 10 URL-ide aegunud varad, mis olid serveeritud otse POP. Top 100 URL-ide järgmist tüüpi varad statistika kuvatakse otse all see graafik.
TCP_EXPIRED_MISS üksikasjad | Sisaldab graafik, mis kuvab ülemine 10 URL-ide aegunud varad oli uus versioon origin serverist. Top 100 URL-ide järgmist tüüpi varad statistika kuvatakse otse all see graafik.
TCP_CLIENT_REFRESH_MISS üksikasjad | Sisaldab lintdiagrammi kuvava ülemine 10 URL-ide jaoks varad ei toodud origin serverist ei-vahemälu taotluse kliendi tõttu. Top 100 URL-ide järgmist tüüpi taotlusi statistika kuvatakse see diagramm all.
KLIENDITÜÜPIDE taotlus | Näitab tüüpi taotlusi tehtud HTTP kliendi (nt brauserid). See aruanne sisaldab rõngasdiagramm, mis pakub tunne selle kohta, kuidas taotlusi käsitletakse. Diagrammi all kuvatakse iga taotluse tüübi läbilaskevõime ja liikluse teavet.
Kasutajaagendi | Sisaldab kuvamise ülemine 10 kasutajaagentide taotleda oma sisu kaudu meie CDN tulpdiagramm. Tavaliselt on kasutajaagendi veebibrauseri, media Playeri või mobiiltelefoni brauseris. Ühe muutujaga statistika 100 ülemist kasutajaagentide kuvatakse otse selle diagrammi all.
Soovitajad | Sisaldab tulpdiagrammil kuvamise põhilised 10 soovitajad meie CDN kaudu sisule. Tavaliselt on referrer on URL-i veebileht või ressursi, mis ühendab sisu. Üksikasjalik teave on esitatud allpool graafik 100 ülemist soovitajad jaoks.
Tihendamise tüübid | Sisaldab rõngasdiagramm, mis ei tööta, kas need on tihendatud Edge'i serverid nõutud varad. Tihendatud varade protsent on kaupa tihendamise tüüp. Üksikasjalik teave on esitatud allpool graafik iga tihendamise tüüp ja olek.
Failitüübid | Tulpdiagramm, mis kuvab ülemine 10 failitüübid, kuni meie CDN teie konto jaoks soovitud sisaldab. Aruande eesmärgil faili tüüp on määratletud vara failinime laiend ja Internet meediumi tüüp (nt .html \[tekst/html\], .htm \[tekst/html\], .aspx \[tekst/html\]jne.). Üksikasjalik teave on esitatud allpool graafik ülemise 100 faili tüüpi.
Kordumatu failid | Sisaldab graafik, mis kujundab koguarvu kordumatu varad, mis on nõutav päeva määratud aja jooksul.
Turbeloa Auth Kokkuvõte | Sisaldab sektordiagrammi, mis annab kiirülevaate kas nõutud varad olid kaitstud luba põhinev autentimine. Kaitstud varad kuvatakse diagrammi vastavalt nende proovitud autentimist.
Turbeloa Auth keelamiseks üksikasjad | Sisaldab tulpdiagramm, mis võimaldab teil vaadata ülemisel 10 taotlusi, mis olid keelatud luba autentimise tõttu.
HTTP vastuse koodid | Pakub jaotus HTTP olek koode (nt 200 OK, 403 keelatud, 404 ei leitud, jne) mis toimetati kliendid HTTP Edge'i serverid. Sektordiagrammi abil saate kiiresti hinnata, kuidas oma varasid olid kätte. Üksikasjalik statistilisi andmeid on esitatud allpool graafik iga vastuse koodi.
404 vead | Sisaldab tulpdiagramm, mis võimaldab teil vaadata ülemine 10 nõuab, et tulemuseks 404 ei leitud vastuse koodi.
403 vead | Sisaldab tulpdiagramm, mis võimaldab teil vaadata ülemine 10 nõuab, et tulemuseks 403 keelatud vastuse koodi. 403 keelatud vastuse koodi juhul, kui taotlus on keelatud kliendi origin server või serva serveris meie POP.
4xx tõrked | Sisaldab tulpdiagramm, mis võimaldab teil vaadata ülemine 10 nõuab, et tulemuseks vastuse koodi 400 vahemikus. Selles aruandes ei 403 ei leitud ja 404 keelatud vastuse koode. Tavaliselt 4xx vastuse koodi juhul, kui taotlus on keelatud kliendi tõrke tõttu.
504 tõrked | Sisaldab tulpdiagramm, mis võimaldab teil vaadata ülemine 10 nõuab, et tulemuseks 504 lüüsi ajalõpp vastuse koodi. 504 lüüsi ajalõpp vastuse koodi ilmneb ajalõpp ilmnemisel HTTP-puhverserver püüdes teise serveriga. Meie CDN puhul 504 lüüsi ajalõpp vastuse koodi tavaliselt juhul, kui edge server ei saa luua kliendi origin serveriga.
502 tõrked | Sisaldab tulpdiagramm, mis võimaldab teil vaadata ülemine 10 nõuab, et tulemuseks 502 vale lüüs vastuse koodi. 502 vale lüüs vastuse koodi juhul, kui HTTP-protokoll tõrge ilmneb HTTP-puhverserver ja serveri vahel. Meie CDN puhul 502 vale lüüs vastuse koodi tavaliselt juhul, kui kliendi origin server tagastab Sobimatu vastuse serva-serveris. Vastuse ei sobi, kui seda ei saa sõeluda, või kui see on lõpetamata.
5xx tõrked | Sisaldab tulpdiagramm, mis võimaldab teil vaadata ülemine 10 nõuab, et tulemuseks vastuse koodi 500 vahemikus.  Selles aruandes ei 502 vale lüüs ja 504 lüüsi ajalõpp vastuse koode.

## <a name="see-also"></a>Vt ka
* [Azure'i CDN ülevaade](cdn-overview.md)
* [Reaalajas statistika rakenduses Microsoft Azure'i CDN-ID](cdn-real-time-stats.md)
* [Vaikekäitumist HTTP reeglid mootori abil alistamine](cdn-rules-engine.md)
* [Täiustatud HTTP aruanded](cdn-advanced-http-reports.md)
