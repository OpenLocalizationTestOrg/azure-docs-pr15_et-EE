<properties 
   pageTitle="Saate vaadata ja hallata StorSimple virtuaalse massiiv | Microsoft Azure'i"
   description="Kirjeldatakse StorSimple halduri teenuse töö lehe ja kuidas seda kasutada StorSimple virtuaalse massiivi uuemaid ja praeguse töö jälgimiseks."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor=""/>
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="na"
   ms.date="06/07/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-view-jobs-for-the-storsimple-virtual-array"></a>Saate vaadata töid StorSimple virtuaalse massiivi StorSimple halduri teenuse abil

## <a name="overview"></a>Ülevaade

**Töö** lehe pakub ühe keskse portaali vaatamiseks ja haldamiseks tööd, mis on käivitatud virtuaalse massiivi (nimetatakse ka kohapealse virtuaalne seadmed), mis on seotud teie haldur StorSimple teenuse kohta. Saate vaadata käivitatud, lõpule viidud ja nurjunud töö mitme virtuaalse seadmete jaoks. Tulemused on esitatud tabelina. 

![Lehe tööde haldamine](./media/storsimple-ova-manage-jobs/ovajobs1.png)

Võite leida kiiresti töid olete huvitatud, filtreerides väljade näiteks:

- **Olek** – saate otsida kõik, töötab, lõpule viidud või nurjunud tööd.
- **Ja** -tööde haldamine filtreeritud vahemiku kuupäeva ja kellaaja alusel.
- **Tippige** – töö tüüp on kõiki varundada, taastada, Tõrkesiirde, allalaadimise värskendused ja värskenduste installimine.
- **Seadmed** – töö algatatakse teatud seade ühendatud teenust. Filtreeritud tööd on siis tabelis alusel järgmisi atribuute:

    - **Tippige** – töö tüüp on kõiki varundada, taastada, Tõrkesiirde, allalaadimise värskendused ja värskenduste installimine.

    - **Olek** – töö saab olema kõik töötab, täita või ei ole.

    - **Üksuse** – tööd, võib olla seotud helitugevust, võrguketta või seade. 

    - **Seadme** -seadme nime kohta, kus töö alustamine.

    - **Klõpsake alustamine** – töö käivitamise aja.

    - **Edenemise** – esitatava töö protsendi lõpuleviimist. Valmis töö, see tuleb alati 100%.

Tööde loendi värskendatakse iga 30 sekundi järel.

## <a name="view-job-details"></a>Projekti üksikasjade kuvamine

Tehke iga töö üksikasju soovite vaadata.

#### <a name="to-view-job-details"></a>Projekti üksikasjade vaatamiseks

1. Klõpsake lehel **töö** kuvamine valitud, olete huvitatud käivitades päringu vastav filtritega. Saate otsida tööd lõpule viidud või käivitatud.

2. Valige tabeli loendist töökohtade töö.

3. Klõpsake lehe allosas nuppu **üksikasjad**.

4. Saate vaadata dialoogiboksis **üksikasjad** oleku, üksikasjad ja aja statistika. Järgmisel joonisel on kujutatud dialoogiboks **Varundamise töö üksikasjad** .
 
    ![Projekti üksikasjade lehe](./media/storsimple-ova-manage-jobs/ovajobs2.png)

#### <a name="job-failures-when-the-virtual-machine-is-paused-in-the-hypervisor"></a>Töö vead kui selle hypervisor virtuaalse masina on peatatud

Kui tööd on teie StorSimple virtuaalse massiiv ja seadme (virtuaalne arvuti ette valmistatud hypervisor) on peatatud rohkem kui 15 minutit, töö nurjub. See on StorSimple virtuaalse massiiv aja tõttu on sünkroonitud Microsoft Azure'i aeg. Näiteks taastamine töö tõrge kuvatakse järgmine pilt.

![Töö tõrge taastamine](./media/storsimple-ova-manage-jobs/restorejobfailure.png)

Nende tõrgete rakendatakse varundamine, taastamine, värskendamine ja Tõrkesiirde tööde haldamine. Kui arvuti virtuaalne on ette valmistatud Hyper-v, lõpuks sünkroonida seadme abil oma hypervisor aeg. Kui see juhtub, saate oma töö taaskäivitada. 

## <a name="next-steps"></a>Järgmised sammud

[Saate teada, kuidas kasutada kohaliku web UI hallata oma StorSimple virtuaalse massiiv](storsimple-ova-web-ui-admin.md).
