<properties
    pageTitle="Kuidas profiili andmete andmeallikad"
    description="Kuidas lisada tabeli ja veeru tasemel andmete profiilid registreerimisel andmeallikate Azure'i andmekataloogi ja kasutamine andmete profiilid andmeallikate mõistmiseks esiletõstmine juhise."
    services="data-catalog"
    documentationCenter=""
    authors="spelluru"
    manager="NA"
    editor=""
    tags=""/>
<tags
    ms.service="data-catalog"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="data-catalog"
    ms.date="09/13/2016"
    ms.author="spelluru"/>

# <a name="data-profile-data-sources"></a>Profiili andmed andmeallikad

## <a name="introduction"></a>Sissejuhatus

**Microsoft Azure'i andmekataloogi** on täielikult hallatud pilveteenuses, mis toimib registreerimise süsteemi ja ettevõtte andmeallikaid discovery süsteem. Teisisõnu on **Azure andmekataloogi** teave aitab tuvastada, mõista ja andmeallikate ja aidata ettevõtted abil saate oma olemasolevad andmed rohkem väärtust. Kui andmeallikas on registreeritud **Azure'i andmekataloogi**, metaandmete kopeeritakse ja teenuse indekseeritud, kuid lugu ei lõpe.

**Profiili andmed** **Azure'i andmekataloogi** funktsiooni uurib kataloogis Toetatud andmeallikate andmed ja kogub statistika ja teavet andmeid. See on lihtne kaasamiseks oma andmeid varasid profiili. Kui registreerute andmete varade, valige **Kaasata andmete profiil** andmete allikas registreerimise tööriista.

## <a name="what-is-data-profiling"></a>Mis on andmete profiili

Profiili andmed uurib registreerimise andmeallika andmete ja kogub statistika ja teavet andmeid. Allika andmetuvastus ajal statistika aitavad kindlaks teha sobivust andmed oma äri probleemi lahendada.

<!-- In [How to discover data sources](data-catalog-how-to-discover.md), you learn about **Azure Data Catalog's** extensive search capabilities including searching for data assets that have a profile. See [How to include a data profile when registering a data source](#howto). -->

Järgmiste andmeallikatega toeta profiilide andmeid.

- (Sh DB Azure SQL-i ja SQL Azure'i andmebaas) SQL serveri tabelid ja vaated
- Oracle'i tabelid ja vaated
- Teradata tabelid ja vaated
- Taru tabelid

Sh andmete profiilid, kui andmed varad registreerimisel aitab kasutajatel küsimustele andmeallikatega, sealhulgas:

-   Saate seda kasutada oma äri probleemi lahendada?
-   Andmed ei vasta kindla standarditele või mustriga?
-   Millised on mõned andmeallika kõrvalekaldeid?
-   Mis on võimalikud probleemid integreerida andmed minu rakenduste?

> [AZURE.NOTE] Saate lisada ka dokumendid, et vara kirjeldamiseks, kuidas andmeid võib integreerida rakenduse. Vaadake, [kuidas dokument andmeallikad](data-catalog-how-to-documentation.md).


<a name="howto"/>
## <a name="how-to-include-a-data-profile-when-registering-a-data-source"></a>Kuidas lisada andmed profiili andmeallika registreerimisel

On lihtne lisada andmeallika profiili. Kui registreerute andmeallika, andmete allikas registreerimise tööriist paanil **objektide registreerida** , valige **Kaasa andmed profiil**.

![](media\data-catalog-data-profile\data-catalog-register-profile.png)

Lisateavet selle kohta, kuidas registreerida andmeallikate leiate jaotisest [Kuidas registreeruda andmeallikate](data-catalog-how-to-register.md) ja [Azure andmekataloogi kasutamise alustamine](data-catalog-get-started.md).


## <a name="filtering-on-data-assets-that-include-data-profiles"></a>Andmete varad, mis sisaldavad andmeid profiilid filtreerimine
Leida andmete varad, mis sisaldavad andmeid profiil, võite lisada `has:tableDataProfiles` või `has:columnsDataProfiles` ühe otsingusõnade.

> [AZURE.NOTE] Valimisel **Kaasa andmed profiil** andmete allikas registreerimise tööriist hõlmab nii tabeli veeru taseme profiiliteavet. Kuid andmete kataloogi API võimaldab andmete varad ainult üks kogum sisaldab profiiliteavet registreerida.

## <a name="viewing-data-profile-information"></a>Andmete profiil teabe vaatamine

Kui olete leidnud sobiva andmeallika profiiliga, saate vaadata andmete profiilisätteid. Andmete profiili vaatamiseks valige andmete varade ja valige **Andmete profiili** andmekataloogi portaali aknas.

![](media\data-catalog-data-profile\data-catalog-view.png)

**Azure'i** andmekataloogis profiili andmed kuvatakse tabeli ja veeru profiili teave:

### <a name="object-data-profile"></a>Objekti andmete profiil

-   Ridade arv.
-   Tabeli suurus
-   Kui objekti viimati värskendatud

### <a name="column-data-profile"></a>Veeru andmete profiil

- Veeru andmetüüpi
- Erinevate väärtuste arv
- Väärtust ridade arv.
- Miinimum, maksimum, Keskmine ja standardhälve veeru väärtused

## <a name="summary"></a>Kokkuvõte
Profiili andmed pakub statistika ja registreeritud andmete varade aitab teil otsustada lahendamiseks andmete sobivust teavet. Koos marginaalid lisanud, ja andmeallikate dokumenteerimine, andmete profiilid anda kasutajatele süvitsi mõistmine oma andmeid.


## <a name="see-also"></a>Vt ka
-   [Kuidas registreeruda andmeallikad](data-catalog-how-to-register.md)
-   [Azure'i andmekataloogi kasutamise alustamine](data-catalog-get-started.md)
