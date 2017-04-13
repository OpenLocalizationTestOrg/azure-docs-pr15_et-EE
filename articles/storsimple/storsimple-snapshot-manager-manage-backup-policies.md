<properties 
   pageTitle="StorSimple hetktõmmise halduri varukoopia poliitikate | Microsoft Azure'i"
   description="Kirjeldab, kuidas luua ja hallata varukoopia poliitikad, mis juhib ajastatud varukoopiaid StorSimple hetktõmmise halduri MMC lisandmooduli abil."
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
   ms.date="05/12/2016"
   ms.author="v-sharos" />

# <a name="use-storsimple-snapshot-manager-to-create-and-manage-backup-policies"></a>StorSimple hetktõmmise halduri abil saate luua ja varukoopia poliitikate haldamine

## <a name="overview"></a>Ülevaade

Varukoopia poliitika loob ajakava kohalikult või pilves helitugevuse andmeid varundada. Kui loote varukoopia poliitika, saate määrata ka säilituspoliitika. (Saate säilitada kuni 64 pilte.) Varukoopia poliitika kohta leiate lisateavet teemast [varundamise tüüpi](storsimple-what-is-snapshot-manager.md#backup-type) [StorSimple 8000 sarja: hübriid pilve lahendus](storsimple-overview.md).

Selles õppeteemas selgitatakse, kuidas:

- Poliitika varukoopia loomine 
- Varukoopia poliitika redigeerimine 
- Varukoopia poliitika kustutamine 

## <a name="create-a-backup-policy"></a>Poliitika varukoopia loomine

Järgmiste toimingute abil saate luua uue poliitika varukoopia.

#### <a name="to-create-a-backup-policy"></a>Reeglite varukoopia loomiseks

1. Töölaua ikooni alustamiseks StorSimple hetktõmmise haldur.

2. **Ulatus** paanil Paremklõpsake **Varundamise poliitikate**ja klõpsake nuppu **Varundus poliitika loomine**.

    ![Poliitika varukoopia loomine](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_BU_policy.png)

    Kuvatakse dialoogiboks **Loo poliitika** . 

    ![Poliitika - vahekaardi Üldist loomine](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_policy_general.png)

3. Klõpsake vahekaardi **üldist** , sisestage järgmine teave:

   1. Tippige väljale **nimi** nimi poliitika.

   2. Tippige tekstiväljale **Helitugevuse rühma** seotud poliitika helitugevuse rühma nime.

   3. Valige **Kohalik hetktõmmise** või **Cloud hetktõmmis**.

   4. Valige hetktõmmiste säilitamise arvu. Kui olete valinud **Kõik**, on 64 hetktõmmiseid säilitatakse (maksimum). 

4. Klõpsake vahekaarti **ajakava** .

    ![Poliitika - ajakava menüü loomine](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_policy_schedule.png)

5. Klõpsake menüü **ajakava** sisestage järgmine teave: 

   1. Märkige ruut **Luba** plaanida järgmise varukoopia.

   2. Klõpsake jaotises **sätted**valige **üks kord**, **päeva**-, **nädala**või **kuu**. 

   3. **Käivitage** tekstivälja, klõpsake ikooni kalender ja valige alguskuupäev.

   4. Klõpsake jaotises **Täpsemad sätted**saate määrata valikuline kordamise ajakava ja lõppkuupäev.

   5. Klõpsake nuppu **OK**.

Kui olete loonud varukoopia poliitika, **tulemuste** paanil kuvatakse järgmine teave:

- **Nimi** – varukoopia poliitika nimi.

- **Tippige** – kohaliku hetktõmmise või pilveteenuse hetktõmmise.

- **Helitugevuse rühma** – helitugevuse rühma seotud poliitika.

- **Säilitus** -arvu hetktõmmiseid säilitada; maksimumväärtus on 64.

- **Loodud** – see poliitika loomise kuupäev.

- **Lubatud** – kas poliitika on praegu aktiivne: **True** näitab, et see on tegelikult; **FALSE** tähendab, et see ei ole efekti. 

## <a name="edit-a-backup-policy"></a>Varukoopia poliitika redigeerimine

Järgmiste toimingute abil saate olemasoleva varukoopia poliitika redigeerimine.

#### <a name="to-edit-a-backup-policy"></a>Kui soovite varukoopia poliitika redigeerimine

1. Töölaua ikooni alustamiseks StorSimple hetktõmmise haldur. 

2. Klõpsake paanil **ulatus** **Varundamise poliitika** sõlme. Varukoopia poliitikaid kuvatakse **tulemuste** paanil. 

3. Paremklõpsake poliitika, mida soovite redigeerida, ja seejärel klõpsake nuppu **Redigeeri**. 

    ![Varukoopia poliitika redigeerimine](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Edit_BU_policy.png) 

4. **Loo poliitika** akna kuvamisel sisestage muudatused ja seejärel klõpsake nuppu **OK**. 

## <a name="delete-a-backup-policy"></a>Varukoopia poliitika kustutamine

Järgmiste toimingute abil saate kustutada varukoopia poliitika.

#### <a name="to-delete-a-backup-policy"></a>Varukoopia poliitika kustutamine

1. Töölaua ikooni alustamiseks StorSimple hetktõmmise haldur. 

2. Klõpsake paanil **ulatus** **Varundamise poliitika** sõlme. Varukoopia poliitikaid kuvatakse **tulemuste** paanil. 

3. Paremklõpsake varukoopia poliitika, mille soovite kustutada, ja seejärel klõpsake käsku **Kustuta**.

4. Kinnitusteate kuvamisel klõpsake nuppu **Jah**.

    ![Kustutamise kinnitamine varukoopia poliitika](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Delete_BU_policy.png)

## <a name="next-steps"></a>Järgmised sammud

- Siit saate teada, [StorSimple hetktõmmise halduri hallata oma StorSimple lahenduse kasutamise](storsimple-snapshot-manager-admin.md)kohta.
- Siit saate teada, [StorSimple hetktõmmise halduri vaadata ja hallata varundamise kasutamise](storsimple-snapshot-manager-manage-backup-jobs.md)kohta.
