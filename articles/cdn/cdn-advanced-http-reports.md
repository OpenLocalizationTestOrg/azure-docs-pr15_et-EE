<properties
    pageTitle="Azure'i CDN Täpsemad HTTP aruannete | Microsoft Azure'i"
    description="Microsoft Azure'i CDN täpsemalt HTTP aruanded. Need aruanded teavet üksikasjalik CDN tegevuse."
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

# <a name="advanced-http-reports-in-microsoft-azure-cdn"></a>Microsoft Azure'i CDN täpsemalt HTTP aruanded

## <a name="overview"></a>Ülevaade

Selles dokumendis selgitatakse täpsemalt HTTP aruandlus Microsoft Azure'i CDN. Need aruanded teavet üksikasjalik CDN tegevuse.

[AZURE.INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="accessing-advanced-http-reports"></a>Juurdepääs täpsemalt HTTP aruanded

1. Keelest CDN profiil nuppu **Halda** .

    ![CDN profiili blade haldamise nupp](./media/cdn-advanced-http-reports/cdn-manage-btn.png)

    CDN haldusportaali avatakse.

2. Menüü **analüüsi** kursoriga ja seejärel **Täpsemalt HTTP aruannete** hüpik kursoriga.  Klõpsake **HTTP suure platvormi**.

    ![Haldusportaali CDN - menüü täpsemalt aruanded](./media/cdn-advanced-http-reports/cdn-advanced-reports.png)

    Aruande, kuvatakse suvandid.

## <a name="geography-reports-map-based"></a>Geograafia aruanded (kaardipõhiste)

On viis aruannete ära kaardil regioonid, kus taotletakse sisu näitamiseks. Need aruanded on maailma kaart, USA kaarti, Kanada kaart, Euroopa kaart ja Aasia Vaikse ookeani piirkonna kaarti.

Iga kaardipõhiste aruande järjestab geograafilised üksused (nt riikides, olekus või piirkondades) vastavalt tabamust, mis pärineb protsent antud piirkonnast. Lisaks on saadaval kaardil aitavad teil visualiseerida asukohta, kust teie sisu taotletakse. On võimalik värvikoodid iga piirkonna nõudmisel kogenud selle piirkonna hulgale palunud. Kergem varjustatud piirkondade näitavad mõjutatud oma sisu jaoks ajal tumedamaks piirkondade näitab kõrgem nõudmisel sisu jaoks.

Liikluse ja läbilaskevõime üksikasjalikku teavet iga piirkonna jaoks on esitatud otse kaardi all. See võimaldab vaadata tabamust, tabamust protsenti, andmete kogusumma koguarv üle (GB) ja andmete protsent üle iga piirkonna jaoks. Saate vaadata iga nende mõõdikute kirjeldus. Lõpuks kui liigute hiirega üle piirkonnas (st riik, maakond), nimi ja piirkonna toimunud tabamust protsent kuvatakse kohtspikrina.

Allpool on esitatud lühikirjeldus iga tüüpi kaardipõhiste geograafia aruanne.

Aruande nimi | Kirjeldus
------------|------------
Maailma kaart | Aruande võimaldab teil vaadata kogu maailmas nõudmisel CDN sisu jaoks. Iga riigi on värvkodeeritud kaardile tabamust, mis on pärit piirkonnas protsendi näitamiseks.
Ameerika Ühendriigid kaart | Aruande võimaldab vaadata CDN sisu järele Ameerika Ühendriikides. Iga maakonna on värvkodeeritud sellel kaardil tabamust, mis on pärit piirkonnas protsendi näitamiseks.
Kanada kaart | See aruanne võimaldab teil Kanada järele CDN sisu kuvamiseks. Iga maakond on värvkodeeritud sellel kaardil tabamust, mis on pärit piirkonnas protsendi näitamiseks.
Euroopa kaart | Aruande võimaldab teil vaadata Euroopa nõudmisel CDN sisu jaoks. Iga riigi on värvkodeeritud sellel kaardil tabamust, mis on pärit piirkonnas protsendi näitamiseks.
Aasia Vaikse ookeani kaardil | See aruanne võimaldab teil Aasia järele CDN sisu kuvamiseks. Iga riigi on värvkodeeritud sellel kaardil tabamust, mis on pärit piirkonnas protsendi näitamiseks.

## <a name="geography-reports-bar-charts"></a>Geograafia aruanded (Lintdiagrammid)

On kaks täiendavaid aruandeid, mis statistilise teabe vastavalt geograafia, üleval linna ja Top riigid. Aruannete järjestada linnad ja riigid, vastavalt tabamust, mis on pärit nende piirkondade arvu järgi. Pärast seda tüüpi aruande loomisel, näitab lintdiagrammi ülemine 10 linnad või riigid, kus on nõutavad teatud platvormi sisu. See lintdiagramm saate kiiresti hinnata regioonid, mille suurim arv taotlusi sisu jaoks luua.

Vasakus servas graafik (y-telje) näitab, mitu tabamust toimunud määratud piirkond. Otse all graafik (x-telje), leiate sildi ülemise 10 piirkondade jaoks.

### <a name="using-the-bar-charts"></a>Abil soovitud Lintdiagrammid

* Kui viite kursori lint, nimi ja selle koguarv toimunud piirkonna kuvatakse kohtspikrina.
* Kohtspikker jaoks parimaks linnade aruande tuvastab linna nime, state/province ja riigi lühend.
* Kui linna või piirkonna (st osariik/provints) taotluse pärineb ei saa määrata, siis see näitab, et need on teada. Kui riik on pole teada ja seejärel kaks küsimust märgid (nt?), kuvatakse.
* Aruande võib sisaldada mõõdikute "Europe" või "Aasia ja Vaikse ookeani piirkonna." Need üksused pole mõeldud statistilise teabe kõik IP-aadresside piirkondades. Pigem need rakenduvad ainult taotlusi, mis pärinevad IP-aadressid, mis on üle Euroopa või Aasia/Vaikse ookeani piirkonna asemel kindla linna või riik.

Riba diagrammi loomiseks kasutatud andmed saab vaadata selle all. Seal leiate tabamust, tabamust protsenti, suurt hulka andmeid koguarv üle (GB) ja andmete protsent üle 250 ülemine piirkondade. Saate vaadata iga nende mõõdikute kirjeldus.

Mõlemat tüüpi aruandeid jaoks on esitatud lühikirjeldus.

Aruande nimi | Kirjeldus
------------|------------
Top linnad | Aruande järjestab antud piirkonnast linnade tabamust, mis pärineb arvu järgi.
Ülemise riigid | Aruande järjestab selle piirkonna riikides tabamust, mis pärineb arvu järgi.

## <a name="daily-summary"></a>Igapäevane Kokkuvõte

Igapäevane Kokkuvõte võimaldab teil vaadata koguarvu tabamust ja andmete üle konkreetse platvormi üle iga päev. Seda teavet saab kasutada CDN tegevuse mustrite kiiresti leida. Näiteks aruande aitavad teil tuvastada, mis päevad kogenud suurem või väiksem kui oodatud liikluse.

Pärast seda tüüpi aruande loomisel, lintdiagramm annab visuaalse märgete platvormi kohased nõudmisel kogenud summa iga päev aruandes aja jooksul. Ei, kuvades riba iga päeva aruandes. Näiteks valimise ajal, mil nimega "Eelmine nädal" loob lintdiagrammil koos seitse lindid. Iga riba näitab funktsiooni koguarv kogenud sel päeval.

Vasakus servas graafik (y-telje) näitab, mitu tabamust ilmnenud määratud kuupäeva. Otse all graafik (x-telje), leiate silti, mis näitab, et kuupäev (vormingus: YYYY-MM-DD) iga päeva aruandesse.

> [AZURE.TIP] Kui viite kursori lint, kuvatakse selle koguarv kuupäeva tehtud kohtspikrina.

Riba diagrammi loomiseks kasutatud andmed saab vaadata selle all. Seal leiate selle koguarv ja andmete üle (GB) hulk iga päeva aruandes.

## <a name="by-hour"></a>Tunni võrra

Aruande tund võimaldab teil vaadata koguarvu tabamust ja andmete üle üle konkreetse platvormi klõpsake üks kord tunnis. Seda teavet saab kasutada CDN tegevuse mustrite kiiresti leida. Näiteks aruande aitavad teil tuvastada ajaperioodide päeva jooksul, mis on suurem või väiksem oodatud liikluse kogemus.

Pärast seda tüüpi aruande loomisel, annab lintdiagrammi visuaalse märgete summa platvormi kohased nõudmisel aruandes aja jooksul kogenud üks kord tunnis. Ei, kuvades riba iga tunni aruandes. Näiteks, valides 24-tunnise aja jooksul loob koos 24 lindid lintdiagrammi. Iga riba näitab funktsiooni koguarv kogenud selle tunni jooksul.

Vasakus servas graafik (y-telje) näitab, mitu tabamust ilmnenud täpsustatud tunnis. Otse all graafik (x-telje), leiate silti, mis näitab, kuupäev/kellaaeg (vorming: YYYY-MM-DD hh: mm) iga tunni aruandesse. Aeg teatatakse, kasutades 24-tunnises vormingus ja see on määratud UTC/GMT ajavööndile.

> [AZURE.TIP] Kui viite kursori lint, kuvatakse selle koguarv selle tunni jooksul tehtud kohtspikrina.

Riba diagrammi loomiseks kasutatud andmed saab vaadata selle all. Seal leiate selle koguarv ja andmete üle (GB) hulk iga tunni aruandes.

## <a name="by-file"></a>Faili

Aruande faili, võimaldab teil vaadata nõudmisel ja liiklus konkreetse platvormi kõige nõutud varade üle kantud summa. Pärast seda tüüpi aruande loomisel, luuakse lintdiagrammi ülemine 10 kõige nõutud varad määratud aja jooksul.

> [AZURE.NOTE] Aruande eesmärgil teisendatakse serva CNAME URL-id oma võrdväärse CDN URL. See lubab ka täpne ühtivad jaoks soovitud koguarv seotud vara sõltumata CDN- või CNAME URL, mida kasutatakse selle serva.

Vasakus servas graafik (y-telje) näitab iga varade taotlusi arvu määratud aja jooksul. Otse all graafik (x-telje), leiate silti, mis näitab, et iga ülemine 10 nõutud varasid faili nime.

Riba diagrammi loomiseks kasutatud andmed saab vaadata selle all. Seal leiate järgmist teavet iga ülemise 250 nõutud vara: suhteline tee, tabamust, tabamust protsenti, suurt hulka andmeid koguarv üle (GB) ja andmete protsent üle kanda.

## <a name="by-file-detail"></a>Faili üksikasjad

Faili üksikasjad aruande võimaldab teil vaadata nõudmisel ja konkreetse platvormi jaoks teatud varade üle kantud liiklus. Aruande tipus on suvand faili üksikasjad jaoks. See suvand pakub teie kõige nõutud varade loendi valitud platvormil. Selleks, et koostada aruande faili üksikasjad, peate soovitud vara valimine suvand faili üksikasjad jaoks. Pärast, mis lintdiagrammi näitab iga päev nõudmisel määratud aja jooksul genereerinud summa.

Vasakus servas graafik (y-telje) näitab taotlusi vara kogenud kindla arvu. Otse all graafik (x-telje), leiate silti, mis näitab, et kuupäev (vormingus: YYYY-MM-DD) jaoks millist CDN nõudmisel vara teatati.

Riba diagrammi loomiseks kasutatud andmed saab vaadata selle all. Seal leiate selle koguarv ja andmete üle (GB) hulk iga päeva aruandes.

## <a name="by-file-type"></a>Faili tüübi järgi

Faili tüüp aruanne võimaldab teil vaadata nõudmisel ja liiklus tekkinud faili tüüp. Pärast seda tüüpi aruande loomisel, näitab rõngasdiagramm tabamust loodud ülemine 10 failitüüpide protsent.

> [AZURE.TIP] Kui viite hiirekursori selle rõngasdiagramm sektorit, Interneti meediumi tüüpi failitüüp kuvatakse kohtspikrina.

Rõngas diagrammi loomiseks kasutatud andmed saab vaadata selle all. Leiate faili nime laiend Interneti meediumi tüüp, on koguarv, protsent hitid suurt hulka andmeid üle (GB) ja andmete protsent üle iga algusse 250 failitüübid.

## <a name="by-directory"></a>Directory

Directory, aruande võimaldab teil vajadusel ja liiklus tekkinud konkreetse platvormi teatud kataloogi sisu kuvamiseks. Pärast seda tüüpi aruande loomisel, lintdiagramm näitab funktsiooni koguarv loodud ülemine 10 kataloogide sisu.

### <a name="using-the-bar-chart"></a>Lintdiagrammi abil

* Libistage hiirekursoriga üle vaadata suhteline tee vastav kataloog.
* Salvestatud alamkaust kataloogi sisu ei Loenda arvutamisel nõudmisel Directory. Arvutuses tugineb ainult genereeritud sisu talletatakse tegelik arv.
* Aruande eesmärgil teisendatakse serva CNAME URL-id oma võrdväärse CDN URL. See lubab ka täpne ühtivad kõik statistika seotud vara sõltumata CDN- või CNAME URL, mida kasutatakse selle serva.

Vasakus servas graafik (y-telje) näitab taotluste sisu, mis on talletatud teie ülemine 10 kataloogide arv. Klõpsake diagrammi iga riba tähistab kataloog. Kasutage color-coding värviskeemi sobivad kuni lindi jaotises Top 250 täielik kataloogide kataloogiga.

Riba diagrammi loomiseks kasutatud andmed saab vaadata selle all. Seal leiate järgmist teavet iga algusse 250 kataloogid: suhteline tee, tabamust, tabamust protsenti, suurt hulka andmeid koguarv üle (GB) ja andmete protsent üle kanda.

## <a name="by-browser"></a>Brauser

Aruande, brauseri võimaldab teil vaadata, millised brauserid kasutati taotleda sisu. Pärast seda tüüpi aruande loomisel, näitab sektordiagrammi taotlusi käsitleja oli ülemine 10 brauserid protsent.

### <a name="using-the-pie-chart"></a>Sektordiagrammi abil

* Libistage hiirekursoriga üle vaadata brauseri nimi ja versioon sektordiagrammi sektorit.
* Selleks et aruandes, käsitletakse iga kordumatu brauseriversiooni kombinatsiooni mõnda muud brauserit.
* Nimega "Muu" sektorit näitab taotlusi käsitleja oli muude brauseri ja versiooni protsent.

Sektor-diagrammi loomiseks kasutatud andmed saab vaadata selle all. Seal leiate brauseri tüüp/versiooninumber, on koguarv ja tabamust protsenti iga algusse 250 brauserites.

## <a name="by-referrer"></a>Referrer järgi

Aruande järgi Referrer võimaldab vaadata põhilised soovitajad sisu valitud platvormi. Mõne referrer näitab loodud taotluse hostname. Pärast seda tüüpi aruande loomisel, näitab lintdiagrammi nõudmisel (st kogus) loodud põhilised 10 soovitajad summa.

Vasakus servas graafik (y-telje) näitab taotlusi vara kogenud iga referrer koguarv. Klõpsake diagrammi iga riba tähistab on referrer. Kasutage color-coding värviskeemi sobivad kuni referrer, mis on loetletud jaotises Top 250 Referrer riba.

Riba diagrammi loomiseks kasutatud andmed saab vaadata selle all. Seal leiate URL-i, on koguarv ja protsenti iga põhilised 250 soovitajad loodud tabamust.

## <a name="by-download"></a>Allalaadimine

Aruande, laadige alla võimaldab teil allalaadimine mustrid kõige nõutud sisu analüüsimiseks. Aruande ülaosas sisaldab võrdleb proovitakse kohale toimetada allalaadimiste lõplikus allalaadimise ülemine 10 nõutud varad lintdiagrammi. Iga riba on värvkodeeritud vastavalt sellele, kas see on proovitud laadida (sinine) või lõpuleviidud allalaadimine (roheline).

> [AZURE.NOTE] Aruande eesmärgil teisendatakse serva CNAME URL-id oma samaväärne CDN URL. See lubab ka täpne ühtivad kõik statistika seotud vara sõltumata CDN- või CNAME URL, mida kasutatakse selle serva.

Vasakus servas graafik (y-telje) näitab iga ülemine 10 nõutud vara faili nimi. Leiate otse all graafik (x-telje) sildid, mis näitavad arvu proovitakse lõpule viia allalaaditavad failid.

Otse all lintdiagrammil järgmine teave on loetletud ülemisel 250 nõutud varad: suhteline tee (sh faili nimi), mitu korda, et täitmise allalaaditud, mitu korda, mis see on nõutav ja nõuab, et tulemuseks täielik laadida protsent.

> [AZURE.TIP] Meie CDN ei teavitata HTTP kliendi (nt brauseri), kui vara on täielikult alla laaditud. Seetõttu on meil arvutamiseks, kas vara on täielikult alla laaditud olekukoodi ja bait-vahemiku taotlused. Vaatame arvutuses tegemisel esmalt on kas taotluse tulemusel 200 OK olek kood. Kui jah, siis vaatame bait-vahemiku taotlusi tagamaks, et need kataks kogu vara. Lõpuks me võrrelda andmehulga mahu nõutud varade üle. Kui andmete on võrdne või suurem kui failimahtu ja bait-vahemiku taotleb vara sobivad, siis on pihta loendatakse täieliku allalaadimine.
>
>Aruande tõlgendav laadist peaks pidage meeles, millega saab muuta järjepidevuse ja aruande täpsuse järgmist.
>
>* Liikluse mustrite ei saa täpselt prognoosida, kui kasutaja-agentide käituvad teisiti. See võib põhjustada lõplikus laadi tulemused, mis on suurem kui 100%.
>* Varade ära HTTP järkjärguline allalaadimine võib-olla ei täpselt tähistab aruandes. Selle põhjuseks kasutajad soovivad eri asukohtades video.

## <a name="by-404-errors"></a>404 vead järgi

404 tõrgete aruanne võimaldab teil tuvastada tüüpi sisu, mis loob suurim arv 404 ei leitud olekukoodi. Aruande ülaosas sisaldab ülemine 10 varade, mille 404 ei leitud olekukoodi tagastati lintdiagrammi. See lintdiagrammi võrdleb põhjustanud 404 ei leitud olek kood varade taotlusi taotluste arv. Iga riba on värvkodeeritud. Näitab, et taotluse tulemuseks 404 ei leitud olekukoodi kasutatakse kollast riba. Punane riba kasutatakse taotlusi vara koguarv.

> [AZURE.NOTE] Selleks et aruandes, võtke arvesse järgmist.
>
>* Tulemus tähistab Taotle vara olekukoodi sõltumata.
>* Serva CNAME URL-ide teisendatakse oma võrdväärse CDN URL. See lubab ka täpne ühtivad kõik statistika seotud vara sõltumata CDN- või CNAME URL, mida kasutatakse selle serva.

Vasakus servas graafik (y-telje) näitab iga ülemine 10 nõutud vara 404 ei leitud olekukoodi põhjustanud faili nimi. Leiate otse all graafik (x-telje) sildid, mis näitavad taotluste koguarvu ja tulemuseks 404 ei leitud olekukoodi taotluste arvu.

Otse all lintdiagrammil järgmine teave on loetletud ülemisel 250 nõutud varad: suhteline tee (sh faili nimi), taotluste 404 ei leitud olekukoodi tulemuseks arvu, on mitu korda, vara soovitud ja taotlusi, et tulemuseks 404 ei leitud olekukoodi protsent.

## <a name="see-also"></a>Vt ka
* [Azure'i CDN ülevaade](cdn-overview.md)
* [Reaalajas statistika rakenduses Microsoft Azure'i CDN-ID](cdn-real-time-stats.md)
* [Vaikekäitumist HTTP reeglid mootori abil alistamine](cdn-rules-engine.md)
* [Serva jõudluse analüüs](cdn-edge-performance.md)
