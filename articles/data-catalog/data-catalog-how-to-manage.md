<properties
   pageTitle="Kuidas hallata andmeid varad | Microsoft Azure'i"
   description="Kuidas määrata nähtavuse ja varade andmete esiletõstmine juhise registreeritud Azure'i andmekataloogi."
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


# <a name="how-to-manage-data-assets"></a>Kuidas hallata andmeid varad

## <a name="introduction"></a>Sissejuhatus

**Azure'i andmekataloogi** võimaldab andmete allika leidmiseks, mis võimaldab kasutajatel on lihtne leida ja mõista tuleb teha analüüsi ja otsuste tegemiseks andmeallikad. Need dokumendituvastuse funktsioone muuta suurim mõju, kui kõik kasutajad saate otsida ja mõista laiemas saadaval andmeallikate vahemik. Seda silmas pidades andmekataloogi vaikekäitumine on nähtav – ja leitavaks, kõik registreeritud andmeallikate – kõik kataloogi kasutajad.

Andmekataloogi pole kasutajatele juurdepääsu andmine andmed ise. Juurdepääs andmetele reguleerib andmeallika omanik. Andmekataloogi võimaldab kasutajatel andmeallikate ja vaadata registreeritud kataloogi allikad seotud metaandmed.

Võib tekkida olukordi, kus andmeallikaid tuleks nähtavad ainult kindlatele kasutajatele või rühmadele liikmetele. Stsenaariumi puhul, andmekataloogi võimaldab kasutajatel omandiõigust registreeritud andmete varasid kataloogi ja seejärel juhtelemendile nähtavuse varade nad oma.

> [AZURE.NOTE] Selles artiklis kirjeldatud funktsioonid on saadaval ainult Azure andmete kataloogi Standard Edition. Tasuta versioon ei paku võimaluste omandiõiguse ja piiravad andmete varade nähtavuse.

## <a name="managing-ownership-of-data-assets"></a>Andmete varade haldamine
Vaikimisi on registreeritud andmekataloogis andmetega varasid unowned; Kõik kasutajad, kellel on õigus kataloogile saate leida ja neid varasid marginaalide lisamine. Kasutajad saavad võtta unowned andmete varasid ja seejärel saate piirata varasid nad oma nähtavust.

Kui andmete varade andmekataloogis kuulub, ainult kasutajad, kes on volitatud omanikud saate tuvastada vara ja vaadata selle metaandmete ja ainult omanikud saate kustutada vara kataloogist.

> [AZURE.NOTE] Omaniku andmekataloogis mõjutab ainult talletatud kataloogi metaandmed. See ei anna mis tahes õiguste aluseks oleva andmeallika.

### <a name="taking-ownership"></a>Ülevõtmist
Valides suvandi "Võtta omandi" andmekataloogi portaalis saavad kasutajad andmeid vara. Teisiti õigusi pole vajalikud omistada unowned andmete vara; igale kasutajale, võib selleks kuluda unowned andmete vara omandiõiguse.

### <a name="adding-owners-and-co-owners"></a>Omanikud ja kaasomaniku lisamine
Kui andmete vara kuulub juba kasutajad ei saa lihtsalt omandiõigust – need tuleb lisada kaasomanike olemasoleva omaniku. Mis tahes omanik saate lisada kaasomanike täiendavad kasutajad või turberühmad.

> [AZURE.NOTE] See on parim olema vähemalt kaks üksikisikute mis tahes olevad andmed vara omanikud.

### <a name="removing-owners"></a>Omanikud eemaldamine
Nii nagu mis tahes vara omanik saab lisada kaasomanike, mis tahes vara omanik iga kaasomaniku eemaldada.

Kui vara omaniku eemaldab ise omanikuna, ta enam vara haldamiseks. Kui vara omaniku eemaldab ise omanikuna on teiste kaasomanike, naaseb vara unowned olekusse.

## <a name="visibility"></a>Nähtavuse
Andmete varade omanikud saate määrata andmete varasid, mida nad oma nähtavust. Nähtavuse piiramiseks vaikimisi – kui kõik andmekataloogi kasutajad leida ja vaadata andmete vara – vara omanik saab tavalise nähtavuse säte "Kõik", "Omanikud ja need kasutajad" vara atribuutide. Seejärel saate omanikud lisada teatud kasutajate või turberühmad.

> [AZURE.NOTE] Võimaluse korral määratakse turberühmad, mitte üksikute kasutajate vara omaniku- ja nähtavus õigused.

## <a name="catalog-administrators"></a>Kataloogi administraatorid
Andmete kataloogi administraatorid on peidetult kaasomanike kõik varade kataloogi. Varade omanikud ei saa eemaldada nähtavuse kataloogi administraatorid ja administraatorid saavad hallata omaniku- ja nähtavus kõikide andmete varade kataloogi.

## <a name="summary"></a>Kokkuvõte
Äriandmete kataloogi crowdsourcing andmemudel varade metaandmete ja andmete tuvastamine võimaldab kõik kataloogi kasutajad ja leida. Standardse andmekataloogi väljaande pakub piiritlemiseks kindlate andmete varade kasutamise ja nähtavuse omandiõiguse ja haldamise võimalusi.
