<properties 
   pageTitle="StorSimple hetktõmmise halduri varukoopia kataloogi | Microsoft Azure'i"
   description="Kirjeldab, kuidas vaadata ja hallata varukoopia kataloogi StorSimple hetktõmmise halduri MMC lisandmooduli abil."
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

# <a name="use-storsimple-snapshot-manager-to-manage-the-backup-catalog"></a>Kasutage StorSimple hetktõmmise halduri varukoopia kataloogi haldamine

## <a name="overview"></a>Ülevaade

Funktsiooni esmane StorSimple hetktõmmise Manager on abil saate luua rakenduse ühtse varukoopiaid StorSimple mahud hetktõmmiseid kujul. Hetktõmmiseid on loetletud siis XML-faili nimi *varukoopia kataloogi*. Varukoopia kataloogi korraldab hetktõmmiseid helitugevuse rühma ja seejärel kohalik hetktõmmise või pilveteenuse hetktõmmise. 

Selles õppetükis kirjeldatakse, kuidas abil saate **Varukoopia kataloogi** sõlm teha järgmist:

- Taastamine maht 
- Klooni mahu või helitugevuse rühma 
- Varukoopia kustutamine 
- Faili taastamine
- Storsimple hetktõmmise Manageri andmebaasi taastamine

Saate vaadata varukoopia kataloogi **Varukoopia kataloogi** sõlm **ulatus** paani laiendada, ja seejärel helitugevuse rühma laiendamine.

- Kui klõpsate nuppu helitugevuse rühma nime, kuvatakse **otsingutulemite** paan arv kohaliku hetktõmmiseid ja pilveteenuse hetktõmmiseid helitugevuse rühma jaoks saadaval. 

- Kui klõpsate nuppu **Kohaliku hetktõmmise** või **Cloud hetktõmmise**, **tulemuste** paanil kuvatakse iga varukoopia hetktõmmise (olenevalt teie **vaate** sätted) järgmine teave: 

    - **Nimi** – hetktõmmis on kulunud aja. 

    - **Tippige** – kas see on kohalik hetktõmmise või hetktõmmise pilve. 

    - **Omaniku** -sisu omanik. 

    - **Saadaval** – kas hetktõmmis on praegu saadaval. **True** näitab, et hetktõmmis on saadaval ja neid saab taastada; **FALSE** tähendab hetktõmmis pole enam saadaval. 

    - **Imporditud** – kas imporditud varukoopia. **True** näitab, et varundamine on imporditud StorSimple halduri teenuse ajal seade on konfigureeritud StorSimple hetktõmmise halduri; **FALSE** tähendab, et see ei imporditud, kuid on loodud StorSimple hetktõmmise haldur. (Saate kerge vaevaga tuvastada ka imporditud helitugevuse rühma järelliide on lisatud, mis tuvastab imporditud helitugevuse jaotises seadme.)

    ![Varukoopia kataloogi](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Backup_catalog.png)

- Kui laiendada **Kohaliku hetktõmmise** või **Cloud hetktõmmise**ja seejärel klõpsake üksikute hetktõmmise nime, **tulemuste** paanil kuvatakse valitud hetktõmmise kohta järgmine teave:

    - **Nimi** – ketas tähega maht. 

    - **Kohaliku nimi** – kohaliku nime ketas (kui see on saadaval). 

    - **Seadme** -seadme nime kohta, mis asub maht. 

    - **Saadaval** – kas hetktõmmis on praegu saadaval. **True** näitab, et hetktõmmis on saadaval ja neid saab taastada; **FALSE** tähendab hetktõmmis pole enam saadaval. 


## <a name="restore-a-volume"></a>Taastamine maht

Järgmiste toimingute abil maht taastamine varukoopia põhjal.

#### <a name="prerequisites"></a>Eeltingimused

Kui te pole seda juba teinud, helitugevuse ja helitugevuse rühma loomine ja seejärel kustutage helitugevuse. Vaikimisi StorSimple hetktõmmise halduri varundab maht enne seda, et kustutada. Selle ettevaatuse saate vältida andmete kaotsimineku, kui maht on kustutatud tahtmatult või andmed peab olema taastatud mingil põhjusel. 

StorSimple hetktõmmise halduri kuvatakse järgmine teade ajal loob ettevaatusabinõud varukoopia.

![Automaatse hetktõmmise sõnum](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Automatic_snap.png) 

>[AZURE.IMPORTANT]Te ei saa kustutada draivi, mis on osa helitugevuse rühma. Suvand Kustuta pole saadaval. <br>

#### <a name="to-restore-a-volume"></a>Taastada maht

1. Töölaua ikooni alustamiseks StorSimple hetktõmmise haldur. 

2. **Ulatus** paanil laiendage **Varundamise kataloogi** , helitugevuse rühma laiendamine ja valige **Kohalik pilte** või **Cloud hetktõmmiseid**. **Tulemuste** paanil kuvatakse loend varukoopia pilte. 

3. Otsimine varundamise, mille soovite taastada, paremklõpsake seda ja seejärel klõpsake käsku **Taasta**. 

    ![Kataloogi varukoopia põhjal taastamine](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Restore_BU_catalog.png) 

4. Klõpsake lehel confirmation üksikasju, tippige **Kinnita**ja seejärel klõpsake nuppu **OK**. StorSimple hetktõmmise halduri kasutab taastamine varukoopia. 

    ![Kinnitusteate taastamine](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Restore_volume_msg.png) 

5. Saate jälgida taastetoimingu see töötab. Klõpsake paanil **ulatus** laiendage **tööde haldamine** , ja klõpsake **töötab**. Töö üksikasjad kuvatakse **tulemuste** paanil. Kui taastamine töö on valmis, on töö üksikasjad üle loendi **viimase 24 tundi** .

## <a name="clone-a-volume-or-volume-group"></a>Klooni mahu või helitugevuse rühma

Järgmiste toimingute abil saate luua maht või helitugevuse rühma duplikaat (klooni).

#### <a name="to-clone-a-volume-or-volume-group"></a>Klooni mahu või helitugevuse rühma

1. Töölaua ikooni alustamiseks StorSimple hetktõmmise haldur.

2. **Ulatus** paanil laiendage **Varundamise kataloogi** , helitugevuse rühma laiendamine ja klõpsake **Cloud pilte**. Varukoopiate loend kuvatakse **tulemuste** paanil.

3. Leidke helitugevuse või helitugevuse rühm, mida soovite klooni, paremklõpsake helitugevuse või helitugevuse rühma nimi ja klõpsake nuppu **klooni**. Kuvatakse dialoogiboks **Klooni Cloud hetktõmmis** .

    ![Klooni hetktõmmise pilveteenuses](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Clone.png) 

4. Täitke dialoogiboksi **Klooni Cloud hetktõmmise** järgmiselt. 

    1. Tippige väljale **nimi** nimi kloonitud maht. See nimi kuvatakse **mahud** sõlme. 

    2. (Valikuline) valige **ketas**ja valige seejärel rippmenüü loendist draivi nime. 

    3. (Valikuline) valige **Kaust (NTFS)**, ja tippige selle kausta tee või klõpsake nuppu Sirvi ja valige kausta asukoht. 

    4. Klõpsake nuppu **Loo**.

5. Kui kloonimise on lõppenud, peab kloonitud helitugevuse lähtestamine nurjus. Käivitage Server Manager, ja alustage ketta haldus. Üksikasjalikud juhised leiate teemast [Mount mahud](storsimple-snapshot-manager-manage-volumes.md#mount-volumes). Kui see on lähtestatud, maht jaotises **ulatus** paanil sõlme **maht** . Kui te ei näe loendis maht, värskendage loendit ja mahud (Paremklõpsake sõlme **maht** , ja seejärel klõpsake nuppu **Värskenda**).

## <a name="delete-a-backup"></a>Varukoopia kustutamine

Järgmiste toimingute abil saate kustutada hetktõmmise varukoopia kataloogist. 

>[AZURE.NOTE]Varundatud andmete hetktõmmis seostatud hetktõmmise kustutamine kustutab. Andmete pilvest puhastamine võib siiski veidi aega võtta.<br>
 
#### <a name="to-delete-a-backup"></a>Varukoopia kustutamine

1. Töölaua ikooni alustamiseks StorSimple hetktõmmise haldur.

2. **Ulatus** paanil laiendage **Varundamise kataloogi** , helitugevuse rühma laiendamine ja valige **Kohalik pilte** või **Cloud hetktõmmiseid**. Kuvatakse loend pilte **tulemuste** paanil. 

3. Paremklõpsake hetktõmmise, mille soovite kustutada, ja seejärel klõpsake käsku **Kustuta**.

4. Kinnitusteate kuvamisel klõpsake nuppu **OK**. 

## <a name="recover-a-file"></a>Faili taastamine

Kui fail on kogemata kustutanud maht, saate taastada faili toomine hetktõmmise, mis eelnevalt kuupäevad, mis on kustutamise, kasutades hetktõmmis klooni mahu loomiseks ja seejärel kopeerige fail kloonitud helitugevuse algse köite.

#### <a name="prerequisites"></a>Eeltingimused

Enne alustamist, veenduge, et teil on praeguse jaotise helitugevuse varukoopia. Kustutage ühes rühmas helitugevuse mahud salvestatud faili. Lõpuks kustutatud faili taastamine varukoopia järgmiste juhiste abil. 

#### <a name="to-recover-a-deleted-file"></a>Taastada kustutatud faili

1. Teie töölaual ikooni StorSimple hetktõmmise haldur. StorSimple hetktõmmise halduri konsooli aken. 

2. Klõpsake paanil **ulatus** laiendage **Varundamise kataloogi** ja liikuge hetktõmmise, mis sisaldab kustutatud fail. Tavaliselt, peaksite valima hetktõmmise, mis on loodud just enne kustutamist. 

3. Otsige maht, mida soovite klooni, paremklõpsake seda ja klõpsake nuppu **klooni**. Kuvatakse dialoogiboks **Klooni Cloud hetktõmmis** .

    ![Klooni hetktõmmise pilveteenuses](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Clone.png) 

4. Täitke dialoogiboksi **Klooni Cloud hetktõmmise** järgmiselt. 

   1. Tippige väljale **nimi** nimi kloonitud maht. See nimi kuvatakse **mahud** sõlme. 

   2. (Valikuline) Valige **ketas**ja valige seejärel rippmenüü loendist draivi nime. 

   3. (Valikuline) Valige **Kaust (NTFS)**, ja tippige selle kausta tee või klõpsake nuppu **Sirvi** ja valige kausta asukoht. 

   4. Klõpsake nuppu **Loo**. 

5. Kui kloonimise on lõppenud, peab kloonitud helitugevuse lähtestamine nurjus. Käivitage Server Manager, ja alustage ketta haldus. Üksikasjalikud juhised leiate teemast [Mount mahud](storsimple-snapshot-manager-manage-volumes.md#mount-volumes). Kui see on lähtestatud, maht jaotises **ulatus** paanil sõlme **maht** . 

    Kui te ei näe loendis helitugevuse, värskendage loendit ja mahud (Paremklõpsake sõlme **maht** , ja seejärel klõpsake nuppu **Värskenda**).

6. Avage NTFS kausta, mis sisaldab kloonitud maht, laiendage **mahud** ja avage kloonitud maht. Otsige üles fail, mille soovite taastada, ja kopeerige see peamine maht.

7. Pärast faili taastamiseks saate kustutada NTFS kausta, mis sisaldab kloonitud maht.

## <a name="restore-the-storsimple-snapshot-manager-database"></a>StorSimple hetktõmmise Manageri andmebaasi taastamine

Regulaarselt varundada tuleks StorSimple hetktõmmise halduri andmebaas arvutis host. Kui katastroof ilmneb või hosti arvuti mingil põhjusel nurjub, saate selle taastada seejärel varukoopia. Andmebaasi varukoopia loomisel on käsitsi.

#### <a name="to-back-up-and-restore-the-database"></a>Varundamine ja taastamine andmebaas

1. Microsoft StorSimple halduse teenuse lõpetamiseks tehke järgmist.

    1. Käivitage Server Manager.

    2. Valige serveri halduri armatuurlaual, klõpsake menüü **Tööriistad** **teenuseid**.

    3. Valige **Microsoft StorSimple halduse teenuse**aken **teenused** .

    4. Klõpsake parempoolsel paanil **Microsoft StorSimple halduse teenuse**, klõpsake jaotises **teenuse peatamine**.

2. Sirvige arvutis host C:\ProgramData\Microsoft\StorSimple\BACatalog. 

    >[AZURE.NOTE] ProgramData on peidetud kaust.
 
3. XML-i kataloogi faili üles leidnud, kopeerige fail ja turvaliste asukohas või pilves koopia salvestada. Kui selle hosti nurjub, saate selle varufaili taastamine varukoopia põhjal loodud StorSimple hetktõmmise halduris poliitika abil.

    ![Azure'i StorSimple varukoopia kataloogi fail](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_bacatalog.png)

4. Taaskäivitage Microsoft StorSimple halduse teenust. 

    1. Valige serveri halduri armatuurlaual, klõpsake menüü **Tööriistad** **teenuseid**.
    
    2. Valige **Microsoft StorSimple halduse teenuse**aken **teenused** .

    3. Parempoolsel paanil jaotises **Microsoft StorSimple halduse teenuse**nuppu **teenust uuesti**.

5. Sirvige arvutis host C:\ProgramData\Microsoft\StorSimple\BACatalog. 

6. Kataloogi XML-faili kustutamine ja asendage see varukoopia versiooniga loodud. 

7. Klõpsake töölaua StorSimple hetktõmmise halduri ikooni alustamiseks StorSimple hetktõmmise haldur. 

## <a name="next-steps"></a>Järgmised sammud

- Lisateave [halduris StorSimple hetktõmmise hallata oma StorSimple lahendus](storsimple-snapshot-manager-admin.md).
- Lisateavet [StorSimple hetktõmmise halduri tööülesanded ja töövood](storsimple-snapshot-manager-admin.md#storsimple-snapshot-manager-tasks-and-workflows).
