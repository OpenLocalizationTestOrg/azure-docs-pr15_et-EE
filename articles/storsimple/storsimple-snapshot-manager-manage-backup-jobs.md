<properties 
   pageTitle="StorSimple hetktõmmise halduri varukoopia töö | Microsoft Azure'i"
   description="Kirjeldab, kuidas vaadata ja hallata ajastatud, praegu töötab ja lõplikus varukoopia StorSimple hetktõmmise halduri MMC lisandmooduli abil."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="04/26/2016"
   ms.author="v-sharos" />


# <a name="use-storsimple-snapshot-manager-to-view-and-manage-backup-jobs"></a>Saate vaadata ja hallata varundamise StorSimple hetktõmmise halduri abil

## <a name="overview"></a>Ülevaade

**Töö** sõlm **ulatus** paanil kuvatakse **ajastatud**, **viimase 24 tundi**ja **töötab** varukoopia tööülesandeid, mida teie alustatud interaktiivseks või konfigureeritud poliitika. 

Selle õpetuse selgitatakse, kuidas saate kasutada **töö** sõlm ajastatud, tehtud ja praegu töötab varukoopia töö kohta teabe kuvamiseks. (Loendi töökohtade ja vastav teave kuvatakse **tulemuste** paanil.) Lisaks võite paremklõpsata loetletud töö ja leiate kontekstimenüü, kus on loetletud saadaolevad toimingud.

## <a name="view-scheduled-jobs"></a>Kuva ajastatud tööde haldamine

Järgmiste toimingute abil saate vaadata plaanitud.

#### <a name="to-view-scheduled-jobs"></a>Ajastatud tööd kuvamiseks

1. Töölaua ikooni alustamiseks StorSimple hetktõmmise haldur. 

2. **Ulatus** paanil laiendage **tööde haldamine** ja klõpsake **ajastatud**. **Tulemuste** paanil kuvatakse järgmine teave:

    - **Nimi** – ajastatud hetktõmmise nimi

    - **Järgmise** – kuupäev ja kellaaeg Järgmine ajastatud hetktõmmise

    - **Viimase käivitamine** – kuupäev ja kellaaeg Viimati tehtud ajastatud hetktõmmise

    >[AZURE.NOTE] Ühekordse ainult hetktõmmiseid **Järgmise käivitamine** ja **Käivitage viimase** saab sama. 
 
    ![Plaanitud](./media/storsimple-snapshot-manager-manage-backup-jobs/HCS_SSM_Jobs_scheduled.png) 
 
3. Konkreetse töö täiendavaid toiminguid, paremklõpsake **tulemuste** paanil töö nime ja valige menüü Suvandid.

## <a name="view-recent-jobs"></a>Tehtud töö kuvamine

Varunduse ja taaste tööd, mis olid täidetud viimase 24 tunni jooksul järgmiste toimingute abil.

#### <a name="to-view-recent-jobs"></a>Tehtud töö kuvamiseks

1. Töölaua ikooni alustamiseks StorSimple hetktõmmise haldur.

2. **Ulatus** paanil laiendage **tööde haldamine** ja klõpsake **viimase 24 tundi**. **Otsingutulemite** paan näitab varundamise viimase 24 tunni jooksul (kuni 64 tööde haldamine). Sõltuvalt valitud **Vaade** suvandid **tulemuste** paanil kuvatakse järgmine teave:

    - **Nimi** – ajastatud hetktõmmise nime.
 
    - **Alustamine** – kuupäev ja kellaaeg, millal algas hetktõmmis.

    - **Peatatud** – kuupäev ja kellaaeg, millal hetktõmmis lõpule jõudnud või on peatatud.

    - **Möödunud** – **Alustamine** ja **peatamiseni** vahelise aja korda.

    - **Olek** – viimati lõpetatud projekti olek. **Edu** näitab, et varundamine on loodud. **Nurjunud** näitab töö ei käivitu edukalt.

    - **Teavet** – selle põhjuseks on tõrge.

    - **Baiti töödeldud (MB)** – andmehulga helitugevuse rühmast, mis on töödeldud (MB). 

    ![Tööd, mis oli viimase 24 tunni jooksul](./media/storsimple-snapshot-manager-manage-backup-jobs/HCS_SSM_Jobs_Last_24_hours.png) 

3. Konkreetse töö täiendavaid toiminguid, paremklõpsake **tulemuste** paanil töö nime ja valige menüü Suvandid.

    ![Töö kustutamine](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Delete_backup.png) 
     
## <a name="view-currently-running-jobs"></a>Kuva praegu töötavate tööde haldamine

Vaate projektidele töötavad järgmise toiminguga.

#### <a name="to-view-currently-running-jobs"></a>Vaadata praegu töötab tööde haldamine

1. Töölaua ikooni alustamiseks StorSimple hetktõmmise haldur.

2. **Ulatus** paanil laiendage **tööde haldamine** ja klõpsake **töötab**. **Kuva** suvandid sõltuvalt teie määratud **tulemuste** paanil kuvatakse järgmine teave: 

    - **Nimi** – ajastatud hetktõmmise nime.

    - **Alustamine** – kuupäev ja kellaaeg, millal algas hetktõmmis.

    - **Kontrollpunkt** – praeguse toimingu varukoopia.

    - **Olek** – täitmise protsent.
    
    - **Möödunud** – aeg, mis on möödunud algas varukoopia. 

    - **Keskmine läbilaskevõime (MB)** -suhte kokku baiti andmeid töödeldakse mis kokku kuluv aeg töötlemiseks (MB).

    - **Baiti töödeldud (MB)** – kokku baiti töödeldavate andmete (MB).

    - **Baiti kirjutada (MB)** – kokku baiti andmeid kirjutatud (MB). See sisaldab andmeid, samuti metaandmete ja seega on tavaliselt suuremad kui baiti töödeldud.

    ![Töö praegu töötavate](./media/storsimple-snapshot-manager-manage-backup-jobs/HCS_SSM_Jobs_running.png)

3. Konkreetse töö täiendavaid toiminguid, paremklõpsake **tulemuste** paanil töö nime ja valige menüü Suvandid.

## <a name="next-steps"></a>Järgmised sammud

- Siit saate teada, [StorSimple hetktõmmise halduri hallata oma StorSimple lahenduse kasutamise](storsimple-snapshot-manager-admin.md)kohta.
- Siit saate teada, [StorSimple hetktõmmise Manager hallata varukoopia kataloogi kasutamise](storsimple-snapshot-manager-manage-backup-catalog.md)kohta.















            


 

