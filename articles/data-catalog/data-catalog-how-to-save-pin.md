<properties
   pageTitle="Kuidas salvestada otsingute ja andmete varad | Microsoft Azure'i"
   description="Juhise esiletõstmine võimaluste Azure'i andmekataloogis salvestamise andmeallikate ja andmete varad hiljem korduskasutuseks."
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
   ms.date="10/10/2016"
   ms.author="maroche"/>

# <a name="how-to-save-searches-and-pin-data-assets"></a>Kuidas otsingute salvestamine ja kinnitamine andmete varad

## <a name="introduction"></a>Sissejuhatus

Microsoft Azure'i andmekataloogi võimaldab andmetuvastus allikas. Kasutajate kiiresti otsida ja filtreerida andmeallikate leidmiseks ja mõista nende otstarvet, kataloogi hõlpsam leida töö käepärast õigeid andmeid.

Aga kui kasutajad peavad regulaarselt samade andmetega töötada? Aga kui kasutajad aidata regulaarselt oma teadmisi kataloogi sama andmeallikatega? Sel juhul olles väljaandmisel korduvalt sama otsingud võib olla ebaefektiivne – see on koht, kus salvestatud otsingu ja kinnitatud varad aitab andmeid.

## <a name="saved-searches"></a>Salvestatud otsingud

Azure'i andmekataloogi salvestatud otsingu on korduvkasutatav, kasutaja otsing määratlus. Kui kasutaja on määratletud – sealhulgas otsinguterminid, sildid ja muud filtrid – otsingu ta salvestage see hilisemaks kasutamiseks. Salvestatud otsingu määratluse saab seejärel uuesti käivitada, hiljem, otsingukriteeriumidele oma andmeid vara tagastamiseks.

### <a name="creating-a-saved-search"></a>Salvestatud otsingu loomine

Salvestatud otsingu loomiseks sisestage esmalt otsingukriteeriumid uuesti kasutada. Klõpsake linki "Salvesta" otsinguväljale "praegune" Azure andmekataloogi portaalis.

 ![Valige "Salvesta praegune Otsingusätted](./media/data-catalog-how-to-save-pin/01-save-option.png)

Küsimise korral sisestage salvestatud otsingu nimi. Valige põnevamaks ja otsingu tagastatud andmeid varade kirjeldav nimi.

 ![Salvestatud otsingu nimi](./media/data-catalog-how-to-save-pin/02-name.png)

### <a name="managing-saved-searches"></a>Haldamine salvestatud otsingud

Kui kasutaja on üks või mitu otsingud salvestanud, kuvatakse "Salvestatud otsingud" suvand Azure'i andmekataloogi portaalis "praegune" otsinguvälja all. Kui laiendatud, kuvatakse salvestab otsingud täieliku loendi.

 ![Loendi salvestatud otsingud](./media/data-catalog-how-to-save-pin/03-list.png)

Valige loendist salvestatud otsingu põhjustab otsingu teostada.

Valige rippmenüüst menüü annab halduse suvandeid:

 ![Suvandite haldamine salvestatud otsingud](./media/data-catalog-how-to-save-pin/04-managing.png)

Valides "Ümbernimetamine" palutakse kasutajal sisestada salvestatud otsingu jaoks uus nimi. Otsingu määratlus ei saa muuta.

Valige "Kustuta" küsib kasutajalt kinnitust, ja seejärel eemaldage salvestatud otsingu kasutaja loendist.

Valige "Salvesta nimega vaikimisi" tähistab salvestatud otsingu kasutaja otsing vaikimisi valitud. Kui kasutaja sooritab avalehel Azure'i andmekataloogi otsing "tühi", käivitatakse kasutaja vaikimisi otsingu. Lisaks otsingu märgitud vaikimisi kuvatakse salvestatud otsingu loendi alguses.

### <a name="organizational-saved-searches"></a>Ettevõtte salvestatud otsingud

Iga kasutaja saab salvestada otsingud isiklikuks kasutamiseks. Andmete kataloogi administraatorid salvestada ka ettevõtte kõigi kasutajate otsib. Otsingu salvestamisel on esitatud administraatorite suvandit salvestatud otsingu ettevõttes ühiskasutusse anda. Kui see ruut on märgitud, kaasatakse salvestatud otsingu saadaval otsib kõigi kasutajate loendit.

 ![Ettevõtte salvestatud otsingud](./media/data-catalog-how-to-save-pin/08-organizational-saved-search.png)


## <a name="pinned-data-assets"></a>Kinnitatud andmete varad

Salvestatud otsingud kasutajatele salvestamiseks ja uuesti kasutamiseks otsing määratlused; otsingud tagastatud andmeid vara võivad muutuda aja jooksul Muuda kataloogi sisu. Andmete varasid kinnitamine võimaldab kasutajatel konkreetselt tuvastada konkreetsed andmed varasid kergemaks Accessi ilma otsingu kasutamine.

Andmete varade kinnitamine on lihtne – kasutajad saavad klõpsata lihtsalt "pin" ikooni andmed varade kinnitatud loendisse lisamiseks. See ikoon kuvatakse paani varade vaates paani ja vasakpoolse veeru loendivaates Azure'i andmekataloogi portaalis nurka.

![Andmete varade kinnitamine](./media/data-catalog-how-to-save-pin/05-pinning.png)

Vara avakuvalt eemaldamine on sama lihtne – kasutajad klõpsake lihtsalt "pin" ikooni, et valitud vara sätte sisse-ja väljalülitamine.

![Andmete varade avakuvalt eemaldamine](./media/data-catalog-how-to-save-pin/06-unpinning.png)

## <a name="my-assets"></a>"Minu varad"
Azure'i andmekataloogi portaali avalehe sisaldab "Minu varad" osa, mis kuvab varad huvi praeguse kasutaja. See jaotis sisaldab nii kinnitatud varasid ning salvestatud otsingud.

!["Minu vara" avalehel](./media/data-catalog-how-to-save-pin/07-my-assets.png)

## <a name="summary"></a>Kokkuvõte
Azure'i andmekataloogi pakub funktsioone, mis lihtsustavad kasutajad neile vajalikule andmeallikad avastada, et nad saaksid kulutada vähem aega andmed ja sellega töötamise rohkem aega. Salvestatud otsingud ja andmete varad luua neid core võimalusi nii, et kasutajad saavad autoriseerida andmeallikad, millega ta töötab seni kinnitatud.
