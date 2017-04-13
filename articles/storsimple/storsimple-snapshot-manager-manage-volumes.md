<properties 
   pageTitle="StorSimple hetktõmmise juhataja ja mahud | Microsoft Azure'i"
   description="Kirjeldab, kuidas vaadata ja hallata mahud ja konfigureerimiseks varukoopiate StorSimple hetktõmmise halduri MMC lisandmooduli kasutamiseks."
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
   ms.date="04/18/2016"
   ms.author="v-sharos" />

# <a name="use-storsimple-snapshot-manager-to-view-and-manage-volumes"></a>StorSimple hetktõmmise Manageri abil saate vaadata ja hallata mahud

## <a name="overview"></a>Ülevaade

Saate StorSimple hetktõmmise halduri **mahud** sõlm (klõpsake paani **ulatus** ) mahud ja nende kohta käiva teabe vaatamiseks. Mahud on esitatud draivid teineteisele ühendada selle hosti mahtu. **Mahud** sõlm kuvatakse kohaliku mahud ja helitugevuse tüübid, mida toetavad StorSimple, sh mahud avastanud iSCSI ja seadme. 

Toetatud mahud kohta lisateabe saamiseks minge [mitme helitugevuse tuge](storsimple-what-is-snapshot-manager.md#support-for-multiple-volume-types).

![Helitugevuse loendis otsingutulemite paan](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Volume_node.png)

**Mahud** sõlm ka abil saate uuesti või kui StorSimple hetktõmmise haldur leiab need mahud kustutada. 

Selles õpetuses selgitatakse, kuidas saate mount, lähtestamine ja mahud vormindamine ja seejärel kasutage StorSimple hetktõmmise Manager:

- Saate vaadata teavet mahud 
- Kustutage mahud
- Uuesti mahud 
- Konfigureerige lihtsa helitugevust ja varundage see
- Dünaamiline Peegeldatud draiv konfigureerimine ja selle varundamine

>[AZURE.NOTE] Kõik **Helitugevuse** sõlm toimingud on saadaval paanil **toimingud** .
 
## <a name="mount-volumes"></a>Mount mahud

Järgmiste toimingute abil saate mount, lähtestamine ja vormindada StorSimple maht. Seda toimingut kasutatakse haldamise kõvaketta ja vastava mahud või sektsioonid tööriist ketta haldus. Ketas haldamise kohta lisateabe saamiseks minge Microsoft TechNeti veebisaidil [Ketta haldus](https://technet.microsoft.com/library/cc770943.aspx) .

#### <a name="to-mount-volumes"></a>Ühendada mahud

1. Host arvutis käivitamine Microsoft iSCSI algataja.

2. Esitama üks kasutajaliidese portaal target IP-aadresside või discovery IP-aadress ja ühendage seade. Kui seade on ühendatud, mahud on juurdepääs teie Windowsi süsteem. Microsoft iSCSI algataja kasutamise kohta lisateabe saamiseks minge jaotisse "Ühenduse iSCSI target seade" [installimine ja konfigureerimine Microsoft]iSCSI algataja[1].

3. Kasutage ühte järgmistest suvanditest käivitamiseks ketta haldus:

    - Tippige väljale **Käivita** Diskmgmt.msc.

    - Käivitage Server Manager, laiendage **salvestusruumi** ja valige **Ketas haldus**.

    - Käivitage **Haldustööriistad**, laiendage **Arvuti haldamine** ja valige **Ketas haldus**. 

    >[AZURE.NOTE] Ketas halduse käivitamiseks peate kasutama administraatori õigusi.
 
4. Tehke draivid võrgus.

   1. Paremklõpsake mis tahes Helitugevuse märgitud **ühenduseta**ketta haldus.

   2. Klõpsake **uuesti ketas**. Ketas märkida **Online'i** pärast ketas on uuesti aktiveerida.

5. Lähtestab draivid:

   1. Paremklõpsake avastatud maht.

   2. Valige menüü **Vorminda ketas**.

   3. Dialoogiboksi **Vorminda ketas** valige ketast, mida soovite lähtestada, ja seejärel klõpsake nuppu **OK**.

6. Lihtne mahud vormindamine:

   1. Paremklõpsake maht, mida soovite vormindada.

   2. Valige menüü **Lihtne uus draiv**.

   3. Lihtne uus draiv viisardi abil saate vormindada maht:

      - Määrake mahu suurus.
      - Lisage draivi nime.
      - Valige NTFS failisüsteemis.
      - Määrake 64 KB eraldatud ühiku suurus.
      - Kiirvormindus teha.

7. Vormindage mitme sektsiooni maht. Juhised leiate jaotisest "Sektsioonid ja mahud" [Kettapuhastusriista halduse rakendamiseks](https://msdn.microsoft.com/library/dd163556.aspx).

## <a name="view-information-about-your-volumes"></a>Teie mahus teabe vaatamine

Järgmiste toimingute abil saate vaadata teavet kohalik ja Azure StorSimple maht.

#### <a name="to-view-volume-information"></a>Helitugevuse teabe kuvamiseks

1. Töölaua ikooni alustamiseks StorSimple hetktõmmise haldur. 

2. Klõpsake paanil **ulatus** sõlme **maht** . Loendi kohaliku ja ühendatud mahu, sh kõik Azure StorSimple maht, kuvatakse **tulemuste** paanil. **Otsingutulemite** paan veerud on konfigureeritav. (Paremklõpsake sõlme **mahu** , valige **Kuva**ja valige siis **Lisa/Eemalda veerud**.)

    ![Veergude konfigureerimine](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_View_volumes.png)

    Tulemite veerg | Kirjeldus 
    :--------------|:-------------
    Nimi           | Veeru **nimi** sisaldab iga avastatud maht määratud ketas tähte.
    Seadme         | **Seadme** veerg sisaldab host arvutiga ühendatud seadme IP-aadress.
    Seadme maht nimi | **Seadme helitugevuse nime** veerg sisaldab nime, kuhu valitud helitugevuse kuulub seadme helitugevuse. See on määratletud teatud draiv Azure klassikaline portaalis helitugevuse nimi.
    Accessi teed   | **Accessi teed** veeru kuvab access tee maht. See on ketas tähis või ühenduspunkt punkt, mille maht on juurdepääsetav hosti arvutis.
 
## <a name="delete-a-volume"></a>Kustutage maht

Järgmiste toimingute abil saate kustutada maht StorSimple hetktõmmise haldur.

>[AZURE.NOTE] Maht ei saa kustutada, kui see on mis tahes Helitugevuse rühma osa. (Kustuta valik pole saadaval mahus, mis on helitugevuse rühma liikmetel.) Kustutage jaotise kogu draivi kustutamiseks.


#### <a name="to-delete-a-volume"></a>Kustutamiseks maht

1. Töölaua ikooni alustamiseks StorSimple hetktõmmise haldur.

2. Klõpsake paanil **ulatus** sõlme **maht** . 

3. **Otsingutulemite** paan, paremklõpsake draivi, mida soovite kustutada.

4. Menüü, klõpsake nuppu **Kustuta**. 

    ![Kustutage maht](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Delete_volume.png) 

5. Kuvatakse dialoogiboks **Helitugevuse kustutada** . Tippige tekst väljale **Kinnita** ja seejärel klõpsake nuppu **OK**.

6. Vaikimisi StorSimple hetktõmmise halduri varundab maht enne selle kustutamist. Kui kustutamine on tahtmatu, võib see ettevaatuse teid kaitsta andmekao. Kuigi see varundage maht, kuvatakse StorSimple hetktõmmise Manager meilisõnumi **Automaatse hetktõmmise** edenemist. 

    ![Automaatse hetktõmmise sõnum](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Automatic_snap.png) 

## <a name="rescan-volumes"></a>Uuesti mahud

Järgmiste toimingute abil uuesti mahud ühendatud StorSimple hetktõmmise haldur.

#### <a name="to-rescan-the-volumes"></a>Kui soovite uuesti mahud

1. Töölaua ikooni alustamiseks StorSimple hetktõmmise haldur.

2. Paanil **ulatus** **mahud**paremklõpsake ja klõpsake **uuesti maht**.

    ![Uuesti mahud](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Rescan_volumes.png)
 
    Selle toimingu sünkroonib helitugevuse loendi StorSimple hetktõmmise halduriga. Muudatused, nagu uue mahud või kustutatud mahu, kajastub tulemused.

## <a name="configure-and-back-up-a-basic-volume"></a>Tavaline helitugevuse varundamine ja konfigureerimiseks

Järgmiste toimingute abil konfigureerimine lihtsa helitugevust, varukoopia ja seejärel varukoopia kohe alustada või ajastatud varufailide poliitika loomine.

### <a name="prerequisites"></a>Eeltingimused

Enne alustamist:

- Veenduge, et StorSimple seadme ja hosti arvuti on õigesti konfigureeritud. Lisateabe saamiseks minge [Deploy kohapealse StorSimple seadme](storsimple-deployment-walkthrough-u2.md).

- Installima ja konfigureerima StorSimple hetktõmmise haldur. Lisateabe saamiseks minge [Juurutamine StorSimple hetktõmmise haldur](storsimple-snapshot-manager-deployment.md).

#### <a name="to-configure-backup-of-a-basic-volume"></a>Tavaline helitugevuse varukoopia konfigureerimine

1. Saate luua lihtsa maht StorSimple seadme.

2. Mount, lähtestamine ja draivi vormindada, nagu on kirjeldatud [Mount mahud](#mount-volumes). 

3. Teie töölaual ikooni StorSimple hetktõmmise haldur. StorSimple hetktõmmise Manager aken. 

4. **Ulatus** paanil Paremklõpsake sõlme **mahud** ja valige **uuesti maht**. Kui skannimine on lõpule jõudnud, kuvatakse loendi mahud **tulemuste** paanil. 

5. **Tulemuste** paanil Paremklõpsake helitugevust ja valige **Helitugevuse Loo rühm**. 

    ![Helitugevuse rühma loomine](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Create_volume_group.png) 

6. Dialoogiboksis **Loo helitugevuse rühm** helitugevuse rühma nimi Tippige, mahud määramiseks ja seejärel klõpsake nuppu **OK**.

7. Klõpsake paanil **ulatus** laiendage **Helitugevuse rühmad** . Jaotise uus helitugevuse peaks kuvatama jaotises sõlme **Helitugevuse rühmad** . 

8. Paremklõpsake rühma nime helitugevust.

    - Interaktiivsete (vajadusel) Varundustöö alustamiseks klõpsake **Teha varukoopia**. 

    - Klõpsake Automaatne varundamine plaanimiseks **Luua varukoopia poliitika**. Valige lehel **Üldine** loendist helitugevuse rühm. Sisestage lehel **ajakava** ajakava üksikasjad. Kui olete lõpetanud, klõpsake nuppu **OK**. 

9. Veendumaks, et Varundustöö käivitas, **töö** laiendage paanil **ulatuse** ja klõpsake sõlme **töötab** . Praegu töötavate tööde haldamine loendis kuvatakse **tulemuste** paanil. 

## <a name="configure-and-back-up-a-dynamic-mirrored-volume"></a>Konfigureerige ja dünaamiline Peegeldatud draiv varundamine

Järgmiste juhiste konfigureerida varukoopia dünaamiline Peegeldatud draiv:

- Samm 1: Kasutada kettapuhastusriista halduse luua dünaamiline Peegeldatud draiv. 

- Samm 2: Kasuta StorSimple hetktõmmise haldur konfigureerida varundamise.

### <a name="prerequisites"></a>Eeltingimused

Enne alustamist:

- Veenduge, et StorSimple seadme ja hosti arvuti on õigesti konfigureeritud. Lisateabe saamiseks minge [Deploy kohapealse StorSimple seadme](storsimple-deployment-walkthrough-u2.md).

- Installima ja konfigureerima StorSimple hetktõmmise haldur. Lisateabe saamiseks minge [Juurutamine StorSimple hetktõmmise haldur](storsimple-snapshot-manager-deployment.md).

- Saate konfigureerida StorSimple seadme kahest osast. (Näidetes saadaval maht on **ketas 1** ja **2 kettale**). 

### <a name="step-1-use-disk-management-to-create-a-dynamic-mirrored-volume"></a>Samm 1: Kasutada kettapuhastusriista halduse luua dünaamiline Peegeldatud draiv

See on ketta haldus haldamise kõvaketta ja mahud või sektsioonid, et need sisaldavad tööriist. Ketas haldamise kohta lisateabe saamiseks minge Microsoft TechNeti veebisaidil [Ketta haldus](https://technet.microsoft.com/library/cc770943.aspx) .

#### <a name="to-create-a-dynamic-mirrored-volume"></a>Dünaamiline Peegeldatud draiv loomiseks

1. Kasutage ühte järgmistest suvanditest käivitamiseks ketta haldus: 

   - Avage dialoogiboks **käivitamine** , tippige **Diskmgmt.msc**ja vajutage sisestusklahvi Enter.

   - Käivitage Server Manager, laiendage **salvestusruumi** ja valige **Ketas haldus**. 

   - Käivitage **Haldustööriistad**, laiendage **Arvuti haldamine** ja valige **Ketas haldus**. 

2. Veenduge, et seadmes StorSimple on kaks mahud saadaval. (Näites saadaval maht on **ketas 1** ja **2 kettale**). 

3. Aknas ketta halduse alumise paani paremas veerus Paremklõpsake **ketta 1** ja valige **Peegeldatud uus draiv**. 

    ![Peegeldatud uus draiv](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_New_mirrored_volume.png) 

4. **Peegeldatud uus draiv** Viisardilehe, klõpsake nuppu **edasi**.

5. Lehel **Valige ketast** , valige **kettale 2** paanil **valitud** , klõpsake nuppu **Lisa**ja seejärel klõpsake nuppu **edasi**. 

6. Lehel **määrata Drive Letter või tee** aktsepteerige vaikesätted ja klõpsake siis nuppu **edasi**. 

7. Valige lehel **Vorming helitugevuse** **Jaotuse ühiku maht** väljale **64 K**. Märkige ruut **teostada Kiirvormindus** , ja seejärel klõpsake nuppu **edasi**. 

8. Klõpsake lehel **Uus draiv peegeldatud lõpuleviimine** oma sätete ülevaatamine ja klõpsake siis nuppu **valmis**. 

9. Kuvatakse teade, mis näitab teisendatakse lihtsa ketas dünaamiline ketas. Klõpsake nuppu **Jah**.

    ![Dünaamiline ketas teisendamine sõnum](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Disk_management_msg.png) 

10. Kontrollige ketta haldus kettale 1 ning 2, kui teil on näidatud dünaamiline peegeldatud maht. (**Dünaamilise** peaks kuvatama veerus olek ja võimsus riba värv peaks punane, Peegeldatud draiv, mis näitab, et muuta.) 

    ![Ketas halduse peegeldatud dünaamiline ketast](./media/storsimple-snapshot-manager-manage-volumes/HCS_SSM_Verify_dynamic_disks_2.png) 
 
### <a name="step-2-use-storsimple-snapshot-manager-to-configure-backup"></a>Samm 2: Kasuta StorSimple hetktõmmise halduri varundamise konfigureerimine

Järgmiste toimingute abil konfigureerimine dünaamiline Peegeldatud draiv, ja seejärel käivitage varukoopia kohe või ajastatud varufailide poliitika loomine.

#### <a name="to-configure-backup-of-a-dynamic-mirrored-volume"></a>Dünaamiline Peegeldatud draiv varukoopia konfigureerimine

1. Teie töölaual ikooni StorSimple hetktõmmise haldur. StorSimple hetktõmmise Manager aken. 

2. **Ulatus** paanil Paremklõpsake sõlme **mahud** ja valige **uuesti maht**. Kui skannimine on lõpule jõudnud, kuvatakse loendi mahud **tulemuste** paanil. Dünaamiline Peegeldatud draiv ühte loendis. 

3. **Tulemuste** paanil Paremklõpsake dünaamiline Peegeldatud draiv ja seejärel klõpsake käsku **Loo helitugevuse rühm**. 

4. Dialoogiboksis **Loomine helitugevuse rühma** helitugevuse rühma nimi Tippige, dünaamiline peegeldatud helitugevuse määramiseks selle rühma ja seejärel klõpsake nuppu **OK**. 

5. Klõpsake paanil **ulatus** laiendage **Helitugevuse rühmad** . Jaotise uus helitugevuse peaks kuvatama jaotises sõlme **Helitugevuse rühmad** . 

6. Paremklõpsake rühma nime helitugevust. 

    - Interaktiivne (vajadusel) varukoopia töö alustamiseks klõpsake **Teha varukoopia**. 

    - Klõpsake Automaatne varundamine plaanimiseks **Luua varukoopia poliitika**. Valige lehel **Üldine** loendist helitugevuse rühm. Sisestage lehel **ajakava** ajakava üksikasjad. Kui olete lõpetanud, klõpsake nuppu **OK**. 

7. Saate jälgida Varundustöö töötab. Klõpsake paanil **ulatus** laiendage **töö** ja klõpsake nuppu **töötab**, töö üksikasjad kuvatakse **tulemuste** paanil. Varundustöö lõpetades kantakse **viimase 24** tunni töö loendi üksikasjad. 

## <a name="next-steps"></a>Järgmised sammud

- Siit saate teada, [StorSimple hetktõmmise halduri hallata oma StorSimple lahenduse kasutamise](storsimple-snapshot-manager-admin.md)kohta.
- Siit saate teada, [StorSimple hetktõmmise halduri abil saate luua ja hallata helitugevuse kasutamise](storsimple-snapshot-manager-manage-volume-groups.md)kohta.

<!--Reference links-->
[1]: https://msdn.microsoft.com/library/ee338480(v=ws.10).aspx
