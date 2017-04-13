<properties
    pageTitle="Azure'i CDN mustreid analüüsida | Microsoft Azure'i"
    description="Saate vaadata oma CDN abil järgmised aruanded mustreid: läbilaskevõime, andmed üle, tabamust vahemälu olekud, vahemälu tabas suhe, IPV4/IPV6 andmed üle."
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

# <a name="analyze-azure-cdn-usage-patterns"></a>Azure'i CDN mustreid analüüs

[AZURE.INCLUDE [cdn-verizon-only](../../includes/cdn-verizon-only.md)]

Saate vaadata oma CDN abil järgmised aruanded mustreid:

- Läbilaskevõime
- Andmete
- Tabamust
- Vahemälu olekud
- Vahemälu tabas suhe
- Andmete IPv4 ja IPV6

## <a name="accessing-advanced-http-reports"></a>Juurdepääs täpsemalt HTTP aruanded

1. Keelest CDN profiil nuppu **Halda** .

    ![CDN profiili blade haldamise nupp](./media/cdn-reports/cdn-manage-btn.png)

    CDN haldusportaali avatakse.

2. Menüü **analüüsi** kursoriga ja seejärel kursoriga **Core aruannete** hüpik.  Klõpsake menüüs soovitud aruande.

    ![Haldusportaali CDN - menüü Core aruanded](./media/cdn-reports/cdn-core-reports.png)


## <a name="bandwidth"></a>Läbilaskevõime

Aruande läbilaskevõime koosneb graph ja andmete tabel näitab läbilaskevõime kasutuse HTTP ja HTTPS kindla ajavahemiku jooksul. Saate vaadata läbilaskevõime kasutuse üle kõik CDN kuvatakse või kindla POP. Võimaldab vaadata üle CDN kuvatakse rakenduses Mbps liikluse diagrammi ja jaotuse.

- Valige kõik serva sõlmed liiklus kõik sõlmed või valige ripploendist teatud piirkond/sõlm.
- Valige kuupäevavahemik, vaadata andmete täna/sel nädalal/sel kuul jne või sisestage kohandatud kuupäevad, siis klõpsake "Mine" veendumaks, et teie valiku värskendatakse.
- Saate eksportida ja laadige andmed Exceli leht ikooni kõrval "Mine".

Kui aruannet värskendatakse iga 5 minuti järel.

![Läbilaskevõime aruanne](./media/cdn-reports/cdn-bandwidth.png)

## <a name="data-transferred"></a>Andmete

Aruande koosneb graph ja andmete tabel näitab liiklus kasutamine HTTP ja HTTPS kindla ajavahemiku jooksul. Saate vaadata liiklus kasutamine üle kõik CDN kuvatakse või kindla POP. Võimaldab vaadata liikluse diagrammi ja jaotuse üle CDN kuvatakse GB.

- Valige kõik serva sõlmed liikluse aadressilt märkmed või valida kindla piirkonna/sõlm ripploendist.
- Valige kuupäevavahemik, vaadata andmete täna/sel nädalal/sel kuul jne või sisestage kohandatud kuupäevad, siis klõpsake "Mine" veendumaks, et teie valiku on värskendatud.
- Saate eksportida ja laadige andmed Exceli leht ikooni kõrval "Mine".

Kui aruannet värskendatakse iga 5 minuti järel.

![Andmed üle aruanne](./media/cdn-reports/cdn-data-transferred.png)

## <a name="hits-status-codes"></a>Tabamust (oleku koodid)

Aruandes kirjeldatakse taotluse oleku koodid sisu jaotuse. Iga taotluse sisu loob mõne HTTP olekukoodi. Oleku koodi kirjeldatakse, kuidas serva hüppab töödelda kutse. Näiteks 2xx olekukoodi näitavad, et taotluse edukalt kätte klient ajal 4xx olekukoodi näitab ilmnenud on viga. HTTP olekukoodi kohta leiate lisateavet teemast [olek koode](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes).

- Valige kuupäevavahemik, vaadata andmete täna/sel nädalal/sel kuul jne või sisestage kohandatud kuupäevad, siis klõpsake "Mine" veendumaks, et teie valiku on värskendatud.
- Saate eksportida ja laadige andmed, klõpsates Exceli leht "Mine" kõrval.

![Tabamust aruanne](./media/cdn-reports/cdn-hits.png)

## <a name="cache-statuses"></a>Vahemälu olekud

Aruandes kirjeldatakse Vahemälutabamusi- ja vahemälu jätab kliendi taotlus jaotuse. Kuna parima jõudluse pärineb Vahemälutabamusi, saate optimeerida kohaletoimetamise andmeside minimeerimine vahemälu jätab ja aegunud Vahemälutabamusi. Vahemälu jätab vähendada konfigureerimisega serverisse origin vältimiseks määramine "no vahemälu" vastuse päised, vältimine-päringustringi vahemällu välja arvatud juhul, kui see on vajalik ja vältida vahemällu vastuse koode. Aegunud vahemälu tabamust saab hoida, muutes vara kasutaja max-vanus nii kaua taotlusi origin server minimeerimiseks.

![Vahemälu oleku aruanne](./media/cdn-reports/cdn-cache-statuses.png)

### <a name="main-cache-statuses-include"></a>Peamised vahemälu olekud on järgmised.

- TCP_HIT: Kätte servast. Objekti vahemälu oli ja max vanuse ei ole ületatud.
- TCP_MISS: Kella Origin. Objekti ei vahemälu ja vastus oli tagasi origin.
- TCP_EXPIRED _MISS: kätte päritolu pärast pikendamisega koos. Objekti vahemälu oli, kuid oli ületanud max vanuse. Koos on pikendamisega tulemuseks on asendatud uue vastust origin objekti vahemälu.
- TCP_EXPIRED _HIT: kätte servast pärast pikendamisega koos. Objekti vahemälu oli, kuid oli ületanud max vanuse. Origin serveriga on pikendamisega tulemuseks vahemälu objekti, mille on pole muudetud.

- Valige kuupäevavahemik, vaadata andmete täna/sel nädalal/sel kuul jne või sisestage kohandatud kuupäevad, siis klõpsake "Mine" veendumaks, et teie valiku värskendatakse.
- Saate eksportida ja laadige andmed Exceli leht ikooni kõrval "Mine".

### <a name="full-list-of-cache-statuses"></a>Täieliku loendi vahemälu olek

- TCP_HIT – see olek on teatatud kui taotluse serveeritakse otse POP-kliendi. Vara serveeritakse kohe, kui see on kõige lähemal kliendi POP vahemällu ja see on sobiv aeg live POP- või TTL. TTL määratletakse järgmised vastus päised.

    - Olla: s-maxage
    - Olla: max vanus
    - Lõppemist

- TCP_MISS – see olek näitab, et nõutud varade vahemällu talletatud versiooni ei leitud kõige lähemal kliendi POP. Vara küsitakse ettevõtte origin serveris või ettevõtte origin kilp serveris. Kui origin server või origin kilp server tagastab vara, siis selle kätte kliendi ja nii kliendi ja edge server vahemällu. Muul juhul-200 olekukoodi (nt 403 keelatud, 404 ei leitud, jne) tagastatakse.

- TCP_EXPIRED _HIT – selle oleku esitatakse, kui koosolekukutse, mis suunatud aegunud TTL, kui vara max-vanus on aegunud, nt vara kätte otse POP kliendile.

    Aegunud taotluse tulemuseks tavaliselt pikendamisega taotluse origin serveriga. Selleks, et TCP_EXPIRED _HIT tekkida, origin serveri peavad osutama uuemat versiooni vara pole. Seda tüüpi olukord värskendatakse tavaliselt selle vara olla ja aegub päised.

- TCP_EXPIRED _MISS – see olek on teatatud kui uuem versioon aegunud vahemällu talletatud vara serveeritakse POP-kliendi. See juhtub, kui vahemällu talletatud vara TTL on aegunud (nt aegunud max-aastane) ja origin server tagastab vara uuem versioon. Vara uus versioon pakutakse kliendile vahemällu talletatud versiooni asemel kasutada. Lisaks on vahemällu serva serveris ja kliendi.

- CONFIG_NOCACHE – see olek näitab, et kliendikohased konfiguratsiooni meie servast POP takistada vara sattumist vahemällu.

- PUUDUB – see olek näitab vahemälu sisu värskus sisse ei tehtud.

- TCP_ CLIENT_REFRESH _MISS – see olek on teatatud kui HTTP kliendi (nt brauseri) jõustab serva VII uue versiooni aegunud varade origin serverist alla laadida.

    Vaikimisi meie serverites sunnib meie Edge'i serverid uue versiooni vara toomiseks origin serverist HTTP kliendi takistada.

- TCP_ PARTIAL_HIT – see olek on teatatud kui bait vahemiku taotluse, on tulemuseks on pihta osaliselt vahemällu talletatud vara. Nõutud byte vahemiku serveeritakse kohe POP-kliendi.

- UNCACHEABLE – see olek on teatatud kui vara olla ja aegub päised näitab, et see peaks olema vahemälust POP- või HTTP kliendi poolt. Järgmist tüüpi taotlusi serveeritakse origin serverist

## <a name="cache-hit-ratio"></a>Vahemälu tabas suhe

See aruanne näitab vahemällu talletatud taotlusi, mis olid serveeritud otse vahemälu protsent.

Aruanne sisaldab järgmisi üksikasju:

- Kõige lähemal taotleja POP on nõutud sisu vahemällu.
- Taotluse kätte otse meie võrgu servast.
- Kutse ei nõua pikendamisega origin serveriga.

Aruanne ei sisalda:

- Taotlusi, mille tõttu riik filtreerimise suvandid on keelatud.
- Varad, mille päised näitab, et nad peaksid pole vahemällu taotlused. Näiteks olla: era, olla: ei-vahemälu või Pragma: päised ei-vahemälu aitab vältida vara sattumist vahemällu.
- Bait vahemiku taotlused osaliselt vahemällu talletatud sisu.

Valem on: (TCP_ tulemus / (TCP_ tulemus + TCP_MISS)) * 100

- Valige kuupäevavahemik, vaadata andmete täna/sel nädalal/sel kuul jne või sisestage kohandatud kuupäevad, siis klõpsake "Mine" veendumaks, et teie valiku värskendatakse.
- Saate eksportida ja laadige andmed Exceli leht ikooni kõrval "Mine".


![Vahemälu tabas aruanne](./media/cdn-reports/cdn-cache-hit-ratio.png)

## <a name="ipv4ipv6-data-transferred"></a>IPv4/IPV6 andmed üle

Aruandes kuvatakse IPV4 vs IPV6 liikluse kasutus jaotuse.

![IPv4/IPV6 andmed üle](./media/cdn-reports/cdn-ipv4-ipv6.png)

- Valige kuupäevavahemik, vaadata andmete täna/sel nädalal/sel kuul jne või sisestada kohandatud kuupäevad.
- Klõpsake "Mine" veendumaks, et teie valiku värskendatakse.


## <a name="considerations"></a>Kaalutlused

Ainult aruandeid saab viimase 18 aasta jooksul.
