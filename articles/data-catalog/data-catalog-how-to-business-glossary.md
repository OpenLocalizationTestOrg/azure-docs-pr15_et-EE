<properties
    pageTitle="Häälestamise Business sõnastik eest, sildistamine | Microsoft Azure'i"
    description="Esiletõstmine business sõnastik Azure'i andmekataloogis loomine ja kasutamine levinud business sõnavara sildi juhise registreeritud andmete varad."
    services="data-catalog"
    documentationCenter=""
    authors="steelanddata"
    manager="NA"
    editor=""
    tags=""/>
<tags
    ms.service="data-catalog"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="data-catalog"
    ms.date="09/21/2016"
    ms.author="maroche"/>

# <a name="how-to-set-up-the-business-glossary-for-governed-tagging"></a>Kuidas häälestada Business sõnastik reguleeritakse sildistamine

## <a name="introduction"></a>Sissejuhatus

Azure'i andmekataloogi võimaldab andmete allika leidmiseks, mis võimaldab kasutajatel on lihtne leida ja mõista tuleb teha analüüsi ja otsuste tegemiseks andmeallikad. Need dokumendituvastuse funktsioone muuta suurim mõju, kui kasutajad saavad otsida ja mõista laiemas saadaval andmeallikate vahemik.

Ühe andmekataloogi funktsioon, mis aitab paremini mõista andmete on sildistamine. Sildistamine võimaldab kasutajatel märksõnade seostada vara või veerg, mis omakorda on hõlpsam leida varade otsimine või sirvimise kaudu, ning võimaldab kasutajatel hõlpsam mõista konteksti ja vara soovidele.

Siiski sildistamine võib mõnikord põhjustada probleeme oma. Mõned probleemid, mis võib tuua sildistamise näited:

1.  Kasutajate lühendite mõnel varad ja laiendatud teksti teistele ajal sildistamine. See vastuolu takistab on discovery varade, isegi kui soovidele sildistamiseks vara sama sildiga.
2.  Sildid, mis tähendab, et erinevaid asju erinevas kontekstis. Näiteks sildi nimega "Tulu" andmehulgas klient võib tähendab tulu kliendi poolt, kuid sama silti kvartali müük andmekogumis võib kvartali müügitulu ettevõtte.  

Selleks, et neid ja muid sarnaseid küsimusi, sisaldab andmekataloogi Business sõnastik.

Andmete kataloogi Business sõnastik võimaldab asutustel dokumendi oluliste terminite ja nende määratlused levinud business sõnavara loomiseks. Selle juhtimine võimaldab järjepidevuse andmekasutuse terves asutuses. Kui määratletud äri sõnastik, ta saab määrata andmete varasid ning kataloogi, kasutades sama lähenemisviisi sildistamine, võimaldades _reguleerida sildistamine_.

> [AZURE.NOTE] Selles artiklis kirjeldatud funktsioonid on saadaval ainult Azure andmete kataloogi Standard Edition. Tasuta versioon ei paku reguleeritakse sildistamiseks või business sõnastik.

## <a name="glossary-availability-and-privileges"></a>Sõnastik-saadavus ja õigused

*Business sõnastik on saadaval Azure andmete kataloogi Standard Edition. Tasuta andmekataloogi väljaande ei sisalda sõnastik.*

Suvand "Sõnastik" andmekataloogi portaali navigeerimismenüü pääseb business sõnastik.  

![Juurdepääs business sõnastik](./media/data-catalog-how-to-business-glossary/01-portal-menu.png)


Andmete kataloogi administraatorid ja sõnastik administraatorid rolli liikmed saavad loomine, redigeerimine või kustutamine business sõnastik terminite sõnastik. Andmekataloogi kõik kasutajad saavad vaadata Termini määratlused, ja saate sildistada varad sõnastik tingimustega.

![Uue sõnastik Termini lisamine](./media/data-catalog-how-to-business-glossary/02-new-term.png)


## <a name="creating-glossary-terms"></a>Sõnastik terminite loomine

Andmete kataloogi administraatorid ja sõnastik administraatorid, saate luua uusi sõnastik termineid klõpsates uue terminikomplekti ' nupp sõnastik terminite loomiseks järgmised väljad:

* Termini business määratlus
* Kirjeldus, mis sisaldab kasutus- või business reeglid varade/veerg
* Loendi huvirühmade, kes kõige Termini kohta teadma
* Ema termin, mis määratleb termin on algselt korraldatud hierarhia


## <a name="glossary-term-hierarchies"></a>Sõnastik Termini hierarhiad

Andmekataloogi business sõnastik pakub võimalus kirjeldada oma äri sõnavara terminite hierarhia. See võimaldab asutustel luua oma äri taksonoomia paremini tähistava terminite liigitus.

Termini nimi peab olema kordumatu antud tasemel hierarhia - dubleeritud nimed on lubatud. Hierarhia tasemete arvu pole piiratud, kuid hierarhia sageli hõlpsam mõista, kui seal on kolm taset või vähem.

Hierarhiad business sõnastikus kasutamine pole kohustuslik. Jättes ema Termini väli tühjaks sõnastik terminite loomine loendit kindla (hierarhiline) soovitud sõnastik.  

## <a name="tagging-assets-with-glossary-terms"></a>Suhtlussiltide varad tingimuste sõnastik

Kui sõnastik termineid on määratletud kataloogi sees, kogemusi sildistamine varad on optimeeritud sõnastik otsida, kui kasutaja tipib oma sildi. Andmekataloogi portaalis kuvatakse sobitamine kasutaja valida sõnastik tingimused. Kui kasutaja valib sõnastik Termini, lisatakse see vara sildina (alias loendist sõnastik silt). Kasutaja saab valida ka luua uus silt, tippides termin, mis pole sõnastik (alias kasutaja sildi).

![Andmete varade sildistatud ühe kasutaja silt ja kaks sõnastik Sildid](./media/data-catalog-how-to-business-glossary/03-tagged-asset.png)

> [AZURE.NOTE] Kasutaja sildid on toetatud tasuta andmekataloogi väljaande sildi ainuke.

### <a name="hover-behavior-on-tags"></a>Viige kursor siltide käitumine
Andmekataloogi portaalis on kahte tüüpi silte visuaalselt erinevate erinevate hover käitumist. Kasutaja ülelibistamisel kasutaja sildi nad näevad sildi teksti ja kasutaja või kasutajad, kellel on lisatud silt. Ülelibistamisel Kasutaja sõnastik sildi, nad näevad ka sõnastik termin ja lingi avamiseks business sõnastik vaatamiseks täielik määratluse termini määratlus.

### <a name="search-filters-for-tags"></a>Siltide otsimine filtrid
Sõnastik sildid ja kasutajale sildid on leitavad ja otsingu filtritena saab rakendada.

## <a name="summary"></a>Kokkuvõte
Business sõnastik Azure'i andmekataloogi ja seda lubab, reguleerib sildistamine luba andmete varasid tuvastatud, hallata ja avastanud sobival viisil. Business sõnastik saab edendada õ business sõnavara seas organisatsiooni kasutajad ning toetab mõtestatud metaandmeid, et varade discovery tegemine ja Tuul mõistmine.

## <a name="see-also"></a>Vt ka

- [REST API dokumentatsioonist äritegevuse sõnastik](https://msdn.microsoft.com/library/mt708855.aspx)
