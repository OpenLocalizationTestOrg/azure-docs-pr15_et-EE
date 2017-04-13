<properties
   pageTitle="Kuidas andmeallikate | Microsoft Azure'i"
   description="Juhise, kuidas leida registreeritud andmete vara Azure'i andmekataloogi, sh otsimine, filtreerimine ja esiletõstmine võimaluste Azure'i andmekataloogi portaali tabas abil esile tõsta."
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
   ms.date="10/04/2016"
   ms.author="maroche"/>

# <a name="how-to-discover-data-sources"></a>Kuidas leida andmeallikad

## <a name="introduction"></a>Sissejuhatus
**Microsoft Azure'i andmekataloogi** on täielikult hallatud pilveteenuses, mis toimib registreerimise süsteemi ja ettevõtte andmeallikaid discovery süsteem. Teisisõnu on **Azure andmekataloogi** teave aitab tuvastada, mõista ja andmeallikate ja aidata ettevõtted abil saate oma olemasolevad andmed rohkem väärtust. Pärast andmeallika on registreeritud **Azure'i andmekataloogi**, metaandmete on indekseeritud, millal teenus, et kasutajad saavad hõlpsalt avastada, et need on vaja andmeid otsida.

## <a name="searching-and-filtering"></a>Otsingu ja filtreerimise

**Azure'i** andmekataloogis Discovery kasutab kaks esmane menetlustele: otsingu ja filtreerimise.

Otsingu eesmärk on võimas – ja intuitiivne vaikimisi, otsinguterminid sobitatakse suhtes, mis tahes atribuudi kataloogi, sh kasutaja esitatud marginaalid.

Filtreerimine on mõeldud täiendavad otsimine. Kasutajad saavad valida kindla omadused, nt asjatundjate, andmeallika tüüp, objekti tüüp ja sildid, ainult vastavaid andmeid varad ja sobitamine varad ka otsingutulemite piiramiseks.

Otsingu ja filtreerimise kombinatsiooni abil kasutajad saavad kiiresti liikuda andmeallikad, mis on registreeritud **Azure'i andmekataloogi** avastada neile vajalikule andmeallikad.

## <a name="search-syntax"></a>Otsingu süntaks

Kuigi vaikimisi otsingu tasuta tekst on lihtne ja intuitiivne, kasutajad saavad kasutada ka **Azure andmekataloogi**otsingu süntaks on otsingutulemite üle suuremat kontrolli. **Azure'i andmekataloogi** otsingu toetab järgnevalt:

| Tehnika                 | Kasutamine                                                                                                                                     | Näide                                                   |
|---------------------------|-----------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------|
| Lihtotsinguavaldis              | Lihtsa otsingu abil üks või mitu otsinguterminit. Tulemus on mis tahes varasid, mis tahes atribuudi ühe või mitme määratud tingimustele vastavad. | müügiandmed                                                |
| Atribuut ulatuse määramine          | Ainult tagastada andmeallikad, kus Otsitav termin sobitada määratud atribuut                                                   | nimi: finance                                              |
| Toetatud brauserid         | Laiendada või kahendmuutuja toimingute abil otsingu piiritlemine                                                                                     | rahandus ei ole ettevõtte                                     |
| Rühmituse ümarsulg | Sulgude rühma osade päringu saavutamiseks loogilise eraldi, eriti koos toetatud brauserid              | nimi: rahandus ja (Sildid: Kv1 OR Sildid: K2) |
| Võrdluse tehtemärgid      | Kasutada võrdlusi peale võrdse atribuudid, mis on andmetüüpide arv ja kuupäeva                                                | modifiedTime > "11/05/2014"                                 |

**Azure'i andmekataloogi** otsingu kohta leiate lisateavet teemast [https://msdn.microsoft.com/library/azure/mt267594.aspx](https://msdn.microsoft.com/library/azure/mt267594.aspx).

## <a name="hit-highlighting"></a>Tulemus esiletõstmine
Otsingutulemite kuvamisel kuvatud atribuudid, mis vastavad määratud otsinguterminid – näiteks andmete vara nimi, kirjeldus ja sildid – tõstetakse esile oleks lihtsam kindlaks teha, miks antud andmete vara tagastati antud otsing.

> [AZURE.NOTE] Kasutajad saavad lülitada tabas esiletõstmine välja, kui soovitud, kasutades "Tõsta" **Azure andmekataloogi** portaalis.

Otsingutulemite kuvamisel see ei pruugi alati olla selge miks andmete varade on lisatud, isegi kui tasute tabas esiletõstmine lubatud. Kuna kõik atribuudid otsitakse vaikimisi, tuleb andmete vara tagastatakse tõttu vaste veeru taseme atribuudi. Ja kuna mitme kasutaja marginaalide registreeritud andmete varad oma siltide ja kirjeldustega, mitte kõik metaandmete võidakse kuvada otsingutulemuste loendis.

Vaikimisi klõpsake paani vaate, iga kuvatakse otsingutulemite paan sisaldab "vaade otsingutermin vastab" ikooni, mis võimaldab kasutajal kiiresti vaadata arv vasteid ja nende asukoht ja soovi korral neile liikuda.

 ![Tulemus, esiletõstmine ja otsing vastab Azure'i andmekataloogi portaalis](./media/data-catalog-how-to-discover/search-matches.png)

## <a name="summary"></a>Kokkuvõte
Andmeallika registreerimise andmeallika **Azure'i andmekataloogi** on lihtsam üles leida ja mõista, kopeerides struktuuri ja kirjeldava metaandmete andmeallikast kataloogi teenuse. Pärast andmeallika on registreeritud, kasutajad saavad avastada abil filtreerimise ja **Azure andmekataloogi** portaali kaudu otsing.

## <a name="see-also"></a>Vt ka
- [Alustamine Azure'i andmekataloogi](data-catalog-get-started.md) õpetus andmeallikate üksikasjalike üksikasju.
