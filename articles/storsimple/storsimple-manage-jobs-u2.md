<properties 
   pageTitle="Saate vaadata ja hallata StorSimple | Microsoft Azure'i"
   description="Kirjeldatakse StorSimple halduri teenuse töö lehe ja kuidas seda kasutada tehtud, praegune ja ajastatud varukoopia töö jälgimiseks."
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
   ms.workload="TBD"
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-view-and-manage-storsimple-jobs-update-2"></a>StorSimple halduri teenuse abil saate vaadata ja hallata StorSimple (Värskenda 2)

[AZURE.INCLUDE [storsimple-version-selector-manage-jobs](../../includes/storsimple-version-selector-manage-jobs.md)]

## <a name="overview"></a>Ülevaade

**Töö** lehe pakub ühe keskse portaali vaatamine ja haldamine tööd, mis on käivitatud seadmetes ühendatud StorSimple halduri teenust. Saate vaadata ajastatud, käivitatud, lõpule viidud, tühistatud ja nurjunud tööd mitmes seadmes. Tulemused on esitatud tabelina. 

![Lehe tööde haldamine](./media/storsimple-manage-jobs-u2/jobs.png)

Võite leida kiiresti töid olete huvitatud, filtreerides väljade näiteks:

- **Olek** – saate kasutama tööde haldamine, lõpule viidud, loobutakse, nurjunud, tühistamine või tõrgetega lõpule viidud.
- **Ja** -tööde haldamine filtreeritud vahemiku kuupäeva ja kellaaja alusel.
- **Tippige** – töö tüüp võib olla varukoopia, käsitsi varundamine, taastamine, klooni seadme Tõrkesiirde, luua kohalik kinnitatud maht, Helitugevuse muutmiseks, värskendada, toetavad paketi või virtuaalse seadme ettevalmistamise.

- **Seadmed** – töö algatatakse teatud teenust ühendatud seadmes.
Filtreeritud tööd on siis tabelis alusel järgmisi atribuute:

    - **Tippige** – varukoopia, käsitsi varundamine, taastamine, klooni seadme Tõrkesiirde, luua kohalik kinnitatud maht, Helitugevuse muutmiseks, värskendada, toetavad paketi või virtuaalse seadme ettevalmistamise.

    - **Olek** – töötab, lõpule viidud, loobutakse, nurjunud, tühistamine või tõrgetega lõpule viidud.

    - **Üksuse** – tööd, võib olla seotud maht, varukoopia poliitika või seade. Näiteks klooni töö on seostatud mõne helitugevuse ajastatud Varundustöö on seostatud varukoopia poliitika. Seadme töö luuakse avariitaastet (DR) või Taasta toiming.

    - **Seadme** -seadme nime kohta, kus töö alustamine.

    - **Klõpsake alustamine** – töö käivitamise aja.

    - **Edenemise** – esitatava töö protsendi lõpuleviimist. Valmis töö, see tuleb alati 100%.

Tööde loendi värskendatakse iga 30 sekundi järel.

Sellel lehel saate teha järgmisi tööga seotud toiminguid:

- Projekti üksikasjade kuvamine

- Katkestamine

## <a name="view-job-details"></a>Projekti üksikasjade kuvamine

Tehke iga töö üksikasju soovite vaadata.

#### <a name="to-view-job-details"></a>Projekti üksikasjade vaatamiseks

1. Klõpsake lehel **töö** kuvamine valitud, olete huvitatud käivitades päringu vastav filtritega. Saate otsida ka lõpule jõudnud, töötab või tühistada tööde haldamine.

2. Valige töö.

3. Klõpsake lehe allosas nuppu **üksikasjad**.

4. Saate vaadata dialoogiboksis **Varukoopia töö üksikasjad** oleku, üksikasjad, aeg statistika ja andmete statistika.
 
    ![Projekti üksikasjade lehe](./media/storsimple-manage-jobs-u2/JobDetails.png)

## <a name="cancel-a-job"></a>Katkestamine

Järgmiste toimingute töötava katkestamine.

>[AZURE.NOTE] Mõned tööd, nt muutmine maht tüübi Helitugevuse muutmiseks või laiendada käibe-, ei saa tühistada.

### <a name="to-cancel-a-job"></a>Katkestamine

1. Klõpsake lehel **töö** kuvamine töötava valitud, mida soovite tühistada, päringu käitamisel vastav filtritega.

1. Valige töö.

1. Klõpsake lehe allosas nuppu **Loobu**.

1. Kinnituse küsimisel nuppu **Jah**.

Selle töö on nüüd tühistatud.

## <a name="next-steps"></a>Järgmised sammud

- Siit saate teada, kuidas [oma StorSimple varukoopia poliitikate haldamine](storsimple-manage-backup-policies.md).

- Siit saate teada, [StorSimple halduri teenuse haldamiseks StorSimple seadme kasutamise](storsimple-manager-service-administration.md)kohta.
