<properties
   pageTitle="Azure'i andmekataloogi terminoloogia | Microsoft Azure'i"
   description="Selles artiklis on toodud põhimõtet ja Azure andmekataloogi dokumentides kasutatud tutvustus."
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

# <a name="azure-data-catalog-terminology"></a>Azure'i andmekataloogi terminoloogia

## <a name="catalog"></a>Kataloogi

Azure'i andmekataloogi on pilvepõhist metaandmete hoidla, milles saate registreeritud allikad ja andmete varasid. Kataloogi toimib strukturaalset metaandmete ekstraktimist allikate ja kirjeldava metaandmete lisatud kasutajate jaoks keskne talletuskoht.

## <a name="data-source"></a>Andmeallikas

Andmeallikas on süsteemi või ümbris, mida haldab andmete varad. Näiteks SQL serveri andmebaasi, Oracle'i andmebaaside, SQL Serveri analüüsiteenuste andmebaasid (tabeli või mitmedimensioonilise) ja SQL serveri aruandlusteenuste serverid.

## <a name="data-asset"></a>Andmete vara

Andmed on objekte, mis sisalduvad kataloogi registreeritakse andmeallikad. Näiteks SQL serveri tabelid ja vaated, Oracle'i tabelid ja vaated, SQL Server Analysis Services Mõõdud, mõõtmed ja KPI-d ja SQL serveri aruandlusteenuste aruandeid.

## <a name="data-asset-location"></a>Andmete vara asukoht

Kataloogi salvestab asukoha või andmeallika andmete varade, mida saab kasutada ühenduse loomiseks allika kliendi rakenduse abil. Andmeid asukoha ja vormingu sõltuvad andmeallika tüüp. Näiteks SQL serveri tabel saab tuvastada neli osa nime – serverinime, andmebaasi nimi, skeemi nime ja objekti nimi – ajal SQL serveri aruandlusteenuste aruandest saab kindlaks teha URL-i.

## <a name="structural-metadata"></a>Strukturaalset metaandmed

Strukturaalset metaandmete on andmeallikas, mis kirjeldab andmete varade struktuuri ekstraktimist metaandmeid. See hõlmab vara asukoht, selle objekti nimi ja tüüp ja täiendavad spetsiifilise omadused. Näiteks sisaldab strukturaalset metaandmeid tabelid ja vaated nimede ja objekti veergude andmetüübid.

## <a name="descriptive-metadata"></a>Kirjeldav metaandmed

Kirjeldav metaandmete on metaandmed, mis kirjeldab otstarve või soovidele vastavaks andmete vara. Tavaliselt lisatakse kirjeldav metaandmete kataloogi kasutajad Azure'i andmekataloogi portaalis, kuid seda saab ka eraldada andmeallika registreerimise käigus. Näiteks tööriista Azure'i andmekataloogi registreerimise väljavõte kirjeldused SQL Server Analysis Services ja SQL serveri aruandlusteenuste atribuudi kirjelduse ja [ms_description laiendatud atribuudi](https://technet.microsoft.com/library/ms190243.aspx) SQL serveri andmebaaside, kui neid atribuute väärtustega Delve'i sisestatud.

## <a name="request-access"></a>Juurdepääsu taotlemine

Andmete varade kirjeldav metaandmete võivad hõlmata andmeid vara või andmeallika juurdepääsu taotlemise kohta. See teave on esitatud andmed vara asukoht ning võib sisaldada ühte või mitut järgmistest suvanditest:

- Kasutaja või meeskonna andmeallikale juurdepääsu andmise e-posti aadress.
- URL-i dokumenteeritud, mida kasutajad järgima andmeallikale juurdepääsuks.
- Identiteedi ja juurdepääsu juhtimine tööriist (nt Microsofti Identity Manager) andmeallikale juurdepääsuks kasutatavate URL-i.
- Tasuta teksti kirje, mis kirjeldab, kuidas kasutajad pääsevad andmeallikale.

## <a name="preview"></a>Eelvaade

Azure'i andmekataloogis eelvaade on kuni 20 kirjeid, mida saate ekstraktimist andmeallika registreerimise käigus ja kataloogis olevad andmed varade metaandmete salvestatud hetktõmmise. Eelvaate aitab kasutajad, kes avastamine andmete varade paremini mõista selle funktsioon ja eesmärk. Teisisõnu, kuvatakse Näidisandmete võib olla kui kuvatakse ainult veergude nimed ja andmetüübid.
Eelvaated toetatakse ainult tabelid ja vaated ja peab konkreetselt valitud kasutaja registreerimise käigus.

## <a name="data-profile"></a>Profiili andmed

Azure'i andmekataloogis profiili andmed on tabeli- ja veeru taseme metaandmeid registreeritud andmete varade, saab ekstraktimist andmeallika registreerimise käigus ja kataloogis olevad andmed varade metaandmete salvestatud hetktõmmise. Aitab andmete profiil kasutajad, kes avastamine andmete varade paremini mõista selle funktsioon ja eesmärk. Sarnaselt eelvaated, andmete profiilid peab olema selgesõnaliselt märgitud kasutaja registreerimise käigus.

> [AZURE.NOTE] Profiili andmed ekstraktimiseks võib olla kulukas toiming suur tabelid ja vaated ja võib oluliselt suurendada andmeallika registreerimiseks vajalik aeg.

## <a name="user-perspective"></a>Perspektiivi kasutaja

Azure'i andmekataloogi esitada iga kasutaja kirjeldav metaandmed registreeritud andmete vara. Igal kasutajal on erinevate perspektiivi andmed ja selle kasutamise kohta. Näiteks vastutab serveri administraator võib üksikasjad teenuseleping (SLA) või varukoopia Windowsi; andmehaldur võivad pakkuda ettevõtte dokumentatsiooni töötleb andmete toetab; ja analüütik võib kirjeldama tingimusi, mis on kõige asjakohasemad muude ärianalüütikud ja mis võib olla väärtuslik need kasutajad, kellel on vaja leida ja mõista andmeid.

Kõik need perspektiivid on potentsiaalselt väärtuslik ja Azure andmekataloogi koos iga kasutaja saate sisestada teavet, mis on tähenduslik, kui kõik kasutajad saavad kasutada seda teavet mõista andmed ja selle eesmärk.

## <a name="expert"></a>Ekspert

Ekspert on kasutaja, kes on määratletud võttes kursis püsida "ekspert" perspektiivi, andmete vara. Iga kasutaja ise või lisada teise kasutaja ekspert vara. On loetletud ekspert väljendada mis tahes täiendavaid õigusi Azure andmekataloogis; See võimaldab kasutajatel hõlpsasti leida nende perspektiivid tõenäoliselt kõige enam kasu, vaadates vara kirjeldav metaandmed.

## <a name="owner"></a>Omanik

Omaniku on kasutaja, kes on täiendavate õiguste haldamise andmete varade Azure'i andmekataloogis. Kasutajad saavad võtta registreeritud andmete varade ja omanikud saab lisada teiste kasutajate kaasomanike. Lisateabe saamiseks vaadake teemat [andmete varade haldamine](data-catalog-how-to-manage.md)  
> [AZURE.NOTE] Omaniku- ja haldus on saadaval ainult Azure andmete kataloogi Standard Edition.

## <a name="registration"></a>Registreerimine

Registreerimise on andmeallikast pärinevate andmete varade metaandmete ja kopeerimine Azure'i andmekataloogi teenuse toiming. Seejärel saate andmeid varad registreeritud selgitustega ja avastanud.

## <a name="see-also"></a>Vt ka

- [Mis on Azure andmekataloogi?](data-catalog-what-is-data-catalog.md) – Selles artiklis antakse ülevaade andmekataloogi Azure'i teenus, pakub väärtus ja stsenaariumid, see toetab.

- [Azure'i andmekataloogi kasutamise alustamine](data-catalog-get-started.md) – selles artiklis antakse lõpuni õpetuse, mis näitab, kuidas kasutada Azure andmekataloogi andmetuvastus allikas.  
