<properties
   pageTitle="Kasutage StorSimple halduri seadme armatuurlaua | Microsoft Azure'i"
   description="Kirjeldatakse StorSimple halduri teenuse seadme armatuurlaua ja kuidas seda kasutada talletusruumi mõõdikute ja ühendatud algataja vaatamine ja leida järjenumber ja IQN."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="09/21/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-device-dashboard"></a>Kasutage StorSimple halduri seadme armatuurlaud

## <a name="overview"></a>Ülevaade

Seadme StorSimple halduri armatuurlaual annab teile ülevaate teavet teatud StorSimple seadme vastupidiselt armatuurlaua teenus, mis annab teile teavet kõigis seadmetes, mis sisaldab teie Microsoft Azure'i StorSimple lahendus.

![Seadme Armatuurlaua leht](./media/storsimple-device-dashboard/StorSimple_DeviceDashbaord1M.png)

Armatuurlaua sisaldab järgmist teavet:

- **Diagrammiala** – saate vaadata oluline salvestusruumi mõõdikute diagrammi ala armatuurlaua ülaosas. See diagramm, saate kuvada mõõdikute esmane saadaoleva salvestus (suurt hulka andmeid, mis on kirjutanud hosts oma seadmesse) ja tarbitud seadme aja jooksul kokku salvestusruumi.

     Sellega *esmased* viitab andmete kirjutanud selle hosti kogusumma ja saab liigitada helitugevuse tüübi järgi: *esmane astmeline salvestusruumi* sisaldab nii kohalikult talletatav andmed ja andmete mitmetasandilise pilveteenusesse; *esmane kohalikult kinnitatud salvestusruumi* sisaldab ainult andmeid, mis on talletatud kohalik. *Salvestusruumi*, aga, on mõõtmine kogusumma pilves talletatud andmed. See hõlmab astmeline andmed ja varukoopiad. Võtke arvesse, et pilves talletatud andmed on deduplicated ja tihendatud, esmased näitab talletusmahtu kasutatud enne, kui andmed on deduplicated ja tihendatud. (Saate võrrelda nende kahe arvu saada aimu, milline tihendamise määr.) Nii esmase ja salvestusruumi, summad, mis põhineb jälgimise sagedust saate konfigureerida. Näiteks kui valite nädal sagedus, siis diagrammi kuvatakse andmeid eelmise nädala iga päeva.

     Saate konfigureerida diagrammi järgmiselt:

     - Aja jooksul tarbitud salvestusruumi summa kuvamiseks suvand **PILVETEENUSE salvestusruumi kasutada** . Saadaoleva salvestus, mis on kirjutanud selle hosti vaatamiseks valige **Esmane MITMETASANDILISE salvestusruumi kasutada** ja **esmane kohalikult kinnitatud salvestusruumi** suvandid. Näites on valitud mõlema variandi; Seetõttu kuvatakse diagrammi salvestusruumi summad esmased nii pilve. Pange tähele, et enne installimist Update 2 kasutada mis tahes esmane salvestusruumi tähistatud **Esmane MITMETASANDILISE salvestusruumi kasutatud** joon.
     - Diagrammi paremas ülanurgas olevat rippmenüüd abil määrata ajavahemik, nädala-1, 1 kuu, 3 kuud või 1 aasta. Pange tähele, et ülataseme diagramm on värskendatud ainult üks kord päevas seetõttu kajastuvad eelmisel päeval kogusummad.

     Lisateabe saamiseks vt [kasutamine StorSimple halduri teenuse StorSimple seadme jälgimiseks](storsimple-monitor-device.md).

- **Kasutus ülevaade** – **kasutus ülevaade** jaotises näete esmane talletusmahtu kasutatud, ettevalmistatud talletusmahtu ja seadme suurim lubatud mälumaht. Võrreldes need kasutus numbrid suurt hulka, mis on saadaval, saate vaadata ühe pilguga kui peate hankima täiendav salvestusruum. Pange tähele, et see ülevaade värskendatakse iga 15 minuti järel, tõttu erinevust Värskenda sagedus, võidakse kuvada erinevaid numbreid, kui need näha eeltoodud diagrammiala, mida värskendatakse iga päev. Lisateabe saamiseks vt [kasutamine StorSimple halduri teenuse StorSimple seadme jälgimiseks](storsimple-monitor-device.md).


- **Teatiste** – **teatiste** ala sisaldab seadme teatisi. Teatiste rühmitatud raskusaste ja tulemuseks on esitatud teatiste iga raskusastme tasandil arvu. Teatiste raskusaste klõpsates avatakse jäävates vaate kuvamiseks saate teatisi ainult selle raskusaste tasemel selle seadme vahekaarti teatised.

- **Töö** - **tööde haldamine** ala näete tehtud töö tegevuse tulemusi. See teile kinnitada, et operatsioonisüsteem ootuspäraselt või see saab teile teada, et peate rakendada. Hiljuti lõpetatud projektide kohta lisateabe vaatamiseks klõpsake **töö õnnestus viimase 24 tunni jooksul**.

- Armatuurlaua paremal alale **kiire ülevaade** annab kasulikku teavet, nt seadme mudelist, järjenumbri, olek, kirjeldus ja mahud arv.

Saate konfigureerida Tõrkesiirde ja vaadata armatuurlaualt seade ühendatud algataja.

Levinud toimingud, mida saab teha sellel lehel on:

- Ühendatud algataja kuvamine

- Seadme järjenumbri otsimine

- Seadme target IQN otsimine

## <a name="view-connected-initiators"></a>Ühendatud algataja kuvamine

Saate vaadata iSCSI algataja oma seadmesse oma seadme armatuurlaua alal **kiire ülevaade** **Kuva ühendatud algataja** linki klõpsates ühendatud. Sellelt lehelt leiate algataja, mis on ühendatud seadme loetelu tabeli kujul. Iga algataja puhul saate vaadata:

- ISCSI kvalifitseeritud ühendatud algataja nimi (IQN).

- Accessi juhtelement kirje (ACR), mis võimaldab seda ühendatud algataja nimi.

- Ühendatud algataja IP-aadress.

- Võrgu liidesed algataja ühendatud seadme salvestusruumi. Saate vahemiku andmetest 0 andmete 5.

- Kõik mahud, mis on ühendatud algataja on lubatud juurdepääs vastavalt ACR konfiguratsioonile.

Näete ootamatud algataja selles loendis või oodatud need ei kuvata, lugege läbi oma ACR konfigureerimine. 512 algataja kuni saate oma seadmega ühendada.

## <a name="find-the-device-serial-number"></a>Seadme järjenumbri otsimine

Seadme järjenumber võib juhtuda, kui konfigureerite Microsoft silma i/o (MPIO) seadmes. Järgmiste toimingute leidmiseks seadme järjenumbri.

#### <a name="to-find-the-device-serial-number"></a>Seadme järjenumbri leidmiseks

1. Liikuge **seadmete** > **armatuurlaud**.

2. Leidke parempoolsel paanil armatuurlaua alale **kiire ülevaade** .

3. Liikuge kerides allapoole ja otsige üles järjenumbri.

## <a name="find-the-device-target-iqn"></a>Seadme target IQN otsimine

Peate seadme target IQN ülesanne käepigistus autentimise Protocol (CHAP) konfigureerimisel StorSimple seadmes. Järgmiste toimingute leida seadme seatud IQN.

### <a name="to-find-the-device-target-iqn"></a>Seadme target IQN leidmiseks

1. Liikuge **seadmete** > **armatuurlaud**.

1. Leidke parempoolsel paanil armatuurlaua alale **kiire ülevaade** .

1. Liikuge kerides allapoole ja otsige üles target IQN.

## <a name="next-steps"></a>Järgmised sammud

- Lugege lisateavet [StorSimple halduri teenuse armatuurlaud](storsimple-service-dashboard.md).
- Lisateavet [hübriidjuurutuses StorSimple seadme StorSimple halduri teenuse kasutamise](storsimple-manager-service-administration.md)kohta.
