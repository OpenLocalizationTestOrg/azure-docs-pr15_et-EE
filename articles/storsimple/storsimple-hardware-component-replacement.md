<properties 
   pageTitle="StorSimple riistvara komponent asendamine | Microsoft Azure'i"
   description="Selles artiklis kirjeldatakse turvaline asendamine PCMs, domeenikontrolleri moodulid, EBOD kontrollerid, kettadraivide uuesti ja raami StorSimple seadme."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="10/11/2016"
   ms.author="alkohli" />

# <a name="storsimple-hardware-component-replacement"></a>StorSimple riistvara osa asendamine

## <a name="overview"></a>Ülevaade

Riistvara komponent asendamine õpetused kirjeldavad riistvara seadme Microsoft Azure StorSimple 8000 sari ja eemaldada ja asendada need tarvilikud toimingud. Selles artiklis kirjeldatakse ohutus ikoonid, pakub viitu üksikasjalikud õpetused, ja loendite asendatavate komponendid.

>[AZURE.IMPORTANT] Enne, kui proovite eemaldada või mis tahes StorSimple komponent asendada, veenduge, et vaadata [Turvalisus ikooni põhimõtted](#safety-icon-conventions) ja muud [ettevaatusabinõusid](storsimple-safety.md).
 
### <a name="safety-icon-conventions"></a>Turvalisus ikooni põhimõtted

Järgmises tabelis kirjeldatakse turvalisuse ikoonid kasutatud olümpiaandmed. Pöörake tähelepanu nende turvalisuse ikoonid, nagu läbite eemaldada ja asendada seadme komponendid.

| Ikoon | Teksti | Lisateave |
|:---- |:---- |:-----------|
|![Ikoon hoiatus](./media/storsimple-hardware-component-replacement/Warning.png)| **OHTU!** | Näitab ohtlikku olukorda, mille eiramise tulemuseks surma või märgatav kahju. See märksõna on piiratud kõige olukordades.|
|![Ikoon hoiatus](./media/storsimple-hardware-component-replacement/Warning.png)| **HOIATUS!** | Näitab ohtlikku olukorda, mis eiramise, võivad põhjustada surma või märgatav kahju.|
|![Ettevaatlik ikoon](./media/storsimple-hardware-component-replacement/Caution.png)| **ETTEVAATLIK!** |Näitab ohtlikku olukorda, mis eiramise, võivad põhjustada kerge või mõõdukas kahju.|
|![Teate ikoon](./media/storsimple-hardware-component-replacement/NoticeIcon.png)| **TEADE:** | Näitab, et olulised, kuid mitte ohtu seotud teavet.|
|![Elektrilöögi ikoon](./media/storsimple-hardware-component-replacement/Electric.png) | **Elektrisüsteemi elektrilöögiohu** | Näitab kõrgepinge.|
|![Raske ikoon](./media/storsimple-hardware-component-replacement/Weight.png)| **Raske**| |
|![Kasutaja kasutuskõlblikud osad ikoon](./media/storsimple-hardware-component-replacement/NoUserServiceableParts.png)| **Pole kasutaja kasutuskõlblikud osad** | Juurdepääs, välja arvatud väljaõppe.|
|![Lugege juhiseid ikoon](./media/storsimple-hardware-component-replacement/ReadInstructions.png)|**Esmalt lugege kõiki juhiseid**| |
|![Näpunäide ohtu ikoon](./media/storsimple-hardware-component-replacement/TipHazard.png)|**Näpunäide ohtu**| |

### <a name="before-you-begin"></a>Enne alustamist

Tutvumine ohutus teavet oma seadme ja ohutus ikoonid, kasutatakse selle õpetuse. Avage [turvaline installida](storsimple-safety.md) ja kasutada seadme StorSimple täielikku teavet. Vaadake [ettevaatusabinõusid](storsimple-safety.md#handling-precautions) , enne kui saate toime StorSimple seadme kindlasti. 

Enne, kui proovite asendada komponent, võtke arvesse järgmist.

![Ikoon hoiatus](./media/storsimple-hardware-component-replacement/Warning.png) ![elektrilöögi ikooni](./media/storsimple-hardware-component-replacement/Electric.png) **hoiatus!** 

- Maa ise õigesti, kasutades elektrostaatiliste või antistaatilise Matt moodulid ja komponentide StorSimple seadme käsitlemisel.

- Puudutage mis tahes lülitused. Kasutage kaasasolevat pidemete ja juhikud komponendid, mis võivad olla kokku puutunud lülitused töötlemise ajal.

![Ikoon hoiatus](./media/storsimple-hardware-component-replacement/Warning.png) ![Pange tähele ikooni](./media/storsimple-hardware-component-replacement/NoticeIcon.png) **teade:**

Kui asendate mooduli, **Ärge jätke mõnda tühja bay taga ruumi**. Enne eemaldamist probleem osa asendamine või tühi mooduli hankimiseks.

## <a name="hardware-component-replacement-procedures"></a>Riistvara komponent asendamine toimingute

Seadme StorSimple 8000 sarja koosneb mitu lisandmoodulit moodulid primaarne ja/või EBOD Kirjalisandid. Funktsiooni 8100 on ühe esmane ruumi, on 8600 on kaks ruumi seadme esmane ruumi ja EBOD ruumi.

Järgmises tabelis on kokku võetud peamised riistvara teie seadmes. Klõpsake linki veerus **protseduuri** seotud õpetuse avamiseks.

|Komponendid|# Esita|Lisandmooduli mooduli?|Protseduuri
|:---------|:--------|:--------------|:---------------------|
| Raami|1|Ei|[Asendage raami StorSimple seadmes](storsimple-chassis-replacement.md) |
|Esmane kontrollerid|2|Jah| [Selle domeenikontrolleri mooduli StorSimple seadme asendamine](storsimple-controller-replacement.md) |
|764W Power ja jahutus moodulid (PCMs)|2|Jah| [Asendage Power ja jahutus mooduli StorSimple seadmes](storsimple-power-cooling-module-replacement.md) |
|Varuaku|2|Jah| [Asendage varuaku mooduli StorSimple seadmes](storsimple-battery-replacement.md) |
|Kettadraivide|12|Jah| [Asendage kettadraivi StorSimple seadmes](storsimple-disk-drive-replacement.md) |

**Tabel 1** Jaotises esmane ruumi

Esmane ruumi ja EBOD ruumi erinevad nende I/O moodulid. Lisaks on PCMs on erinevate võimsus. PCMs rakenduses esmane ruumi on 764 W, neid EBOD ruumi on 580 W. PCMs rakenduses esmane ruumi sisaldada ka varuaku mooduli.

|Komponendid|# Esita|Lisandmooduli mooduli?| Protseduuri
|:---------|:--------|:--------------|:---------------------|
|Raami|1|Ei| [Asendage raami StorSimple seadmes](storsimple-chassis-replacement.md) |
|EBOD kontrollerid|2|Jah| [Asendage mõne EBOD kontrolleril StorSimple seadmes](storsimple-ebod-controller-replacement.md) |
|580W Power ja jahutus moodulid (PCMs)|2|Jah| [Asendage Power ja jahutus mooduli StorSimple seadmes](storsimple-power-cooling-module-replacement.md) |
|Kettadraivide|12|Jah| [Asendage kettadraivi StorSimple seadmes](storsimple-disk-drive-replacement.md) |

**Tabel 2** Jaotises EBOD ruumi

Lisandmooduli moodulid seadmes on esile tõstetud järgmised esi- ja tagumine diagrammid. Nende diagrammide abil saate määrata asukoha erinevate lisandmooduli moodulid, kui on vaja teha asendamine. Front diagramm näitab funktsiooni kettadraivide ja tagumine diagrammide EBOD ja esmane ruumi Kuva lisandmooduli moodulid.

![Frontplane kettadraivide seade.](./media/storsimple-hardware-component-replacement/IC741028.png)

**Joonis 1** Seadme

|Sildi|Kirjeldus|
|:----|:----------|
|0 - 11|Kettadraivide (Kokku 12)|

Nii esmane ruumi ja EBOD ruumi on ketas carrier moodulid. Raami maja kaksteist 3.5" kettadraivide korraldatud 3, 4-vormingus.

![Backplane seadme esmane ruum moodulid](./media/storsimple-hardware-component-replacement/IC740994.png)

**Joonis 2** Esmane ruumi tagakülg

|Sildi|Kirjeldus|
|:----|:----------|
|1|PCM 0|
|2|PCM 1|
|3|Selle domeenikontrolleri 0|
|4|Selle domeenikontrolleri 1|

![Backplane seadme EBOD ruumi lisandmooduli moodulid](./media/storsimple-hardware-component-replacement/IC769599.png)

**Joonis 3** EBOD ruumi tagakülg

|Sildi|Kirjeldus|
|:----|:----------|
|1|PCM 0|
|2|PCM 1|
|3|Selle domeenikontrolleri EBOD 0|
|4|Selle domeenikontrolleri EBOD 1|

## <a name="field-replaceable-units"></a>Väli asendatavate ühikud

Järgmine väli asendatavate ühikute (FRUs) on saadaval StorSimple seadme.

- Raami (sh integreeritud toimingute paneel)

- 764 W AC PCM

- 580 W AC PCM

- Arvuti kõvaketta ketas carrier moodul

- Selle domeenikontrolleri mooduli

- EBOD kontrolleril mooduli

- Varuaku mooduli

- Hooldus raudtee kit sektsioon

Palun [pöörduge Microsofti klienditoe poole](storsimple-contact-microsoft-support.md) järjestamiseks ühte nende üksuste asendamine.

## <a name="next-steps"></a>Järgmised sammud

Kõigi [turvalisuse teavet](storsimple-safety.md) vaadata, enne kui proovite asendada StorSimple riistvara osa.
