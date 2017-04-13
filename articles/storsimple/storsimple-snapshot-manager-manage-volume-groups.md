<properties 
   pageTitle="StorSimple hetktõmmise halduri helitugevuse rühmad | Microsoft Azure'i"
   description="Selles artiklis kirjeldatakse helitugevuse rühmi luua ja hallata StorSimple hetktõmmise halduri MMC lisandmooduli abil."
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

# <a name="use-storsimple-snapshot-manager-to-create-and-manage-volume-groups"></a>StorSimple hetktõmmise Manageri abil helitugevuse rühmade loomine ja haldamine

## <a name="overview"></a>Ülevaade

Saate **Helitugevuse rühmade** sõlm paanil **ulatus** mahud helitugevuse rühmadesse määrata, helitugevuse rühma kohta teabe vaatamiseks, varukoopiate plaanimine ja redigeerida helitugevuse rühmad. 

Helitugevuse rühmad on seotud mahud varukoopiaid rakenduse ühtsete tagamiseks kasutada kaustu. Lisateabe saamiseks vt [mahud ja helitugevuse rühmad](storsimple-what-is-snapshot-manager.md#volumes-and-volume-groups) ja [Windowsi draivi vari teenus integreerimine](storsimple-what-is-snapshot-manager.md#integration-with-windows-volume-shadow-copy-service).

>[AZURE.IMPORTANT] 
>
> * Helitugevuse rühma kõik maht peab pärinema ühe pilvepõhise teenuse pakkujalt.
> 
> * Kui konfigureerite helitugevuse rühmad, mitte segada kobar-jagatud mahud (CSV-de kaitse) ja mitte-CSV-de kaitse samasse helitugevust. StorSimple hetktõmmise haldur ei toeta sama hetktõmmise CSV-de kaitse ja mitte-CSV-de kaitse.
 
![Helitugevuse rühmade sõlm](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_Volume_groups.png)

**Joonis 1: StorSimple hetktõmmise halduri helitugevuse rühmade sõlm** 

Selles õpetuses selgitatakse, kuidas saate kasutada StorSimple hetktõmmise Manager:

- Saate vaadata teavet oma helitugevuse rühmad 
- Helitugevuse rühma loomine
- Helitugevuse rühma varundamine
- Helitugevuse rühma redigeerimine
- Helitugevuse rühma kustutamine

Kõik need toimingud on saadaval, klõpsake paanil **toimingud** .
 
## <a name="view-volume-groups"></a>Helitugevuse rühmade kuvamine

Kui klõpsate sõlme **Helitugevuse rühmad** , **tulemuste** paanil kuvatakse iga helitugevuse rühma, sõltuvalt veeru valikutele järgmine teave. ( **Tulemuste** paanil veerud on konfigureeritav. Paremklõpsake sõlme **mahu** , valige **Vaade**ja seejärel klõpsake nuppu **Lisa/Eemalda veerud**.)

Tulemite veerg | Kirjeldus 
:--------------|:------------ 
Nimi           | Veeru **nimi** sisaldab helitugevuse rühma nime.
Rakenduse    | **Rakenduste** veerus kuvatakse arvu VSS poolt praegu installitud ja Windows Server töötab.
Valitud       | **Valitud** veergu kuvatakse maht, mis sisalduvad arv jaotises maht. Null (0) näitab, et ükski rakendus on seostatud mahud jaotises maht.
Imporditud       | **Imporditud** veerus kuvatakse imporditud köidete arv. Kui seatud väärtusele **tõene**, see veerg näitab, et rühma helitugevuse Azure klassikaline portaalist imporditud ja on loodud StorSimple hetktõmmise halduris.
 
>[AZURE.NOTE] Azure'i klassikaline portaalis menüü **Varundamise poliitikate** kuvatakse StorSimple hetktõmmise halduri helitugevuse rühmad.
 
## <a name="create-a-volume-group"></a>Helitugevuse rühma loomine

Järgmiste toimingute abil saate luua helitugevuse rühma.

#### <a name="to-create-a-volume-group"></a>Helitugevuse rühma loomiseks

1. Töölaua ikooni alustamiseks StorSimple hetktõmmise haldur. 

2. **Ulatus** paanil Paremklõpsake **Helitugevuse rühmad**ja klõpsake **Helitugevuse Loo rühm**. 

    ![Helitugevuse rühma loomine](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_Create_volume_group.png)
 
    Kuvatakse dialoogiboks **Helitugevuse rühma loomine** . 

    ![Helitugevuse rühma dialoogiboksi loomine](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_CreateVolumeGroup_dialog.png) 

3.  Sisestage järgmine teave: 

    1. Tippige väljale **nimi** kordumatu nimi uue rühma helitugevust. 

    2. Valige väljal **rakenduste** mahud, mis siis tuleb lisada helitugevuse rühmaga seotud rakendusi. 

        **Rakenduste** loendite ruut ainult need rakendused, mis kasutada StorSimple mahud ja VSS poolt on lubatud neid. VSS writer on lubatud ainult juhul, kui maht, et kirjutaja on teadlik StorSimple maht. Kui rakenduste ruut on tühi, siis pole installitud rakendusi, mis kasutada Azure StorSimple mahud ja on toetatud VSS poolt on. (Praegu Azure StorSimple toetab Microsoft Exchange'i ja SQL Server) VSS poolt kohta leiate lisateavet teemast [Windows draivi vari teenus integreerimine](storsimple-what-is-snapshot-manager.md#integration-with-windows-volume-shadow-copy-service).

        Kui valite rakenduse, valitakse automaatselt kõik sellega seotud maht. Vastupidi, kui valite maht, mis on seotud konkreetse rakenduse, valitakse automaatselt rakendus, **rakenduste** väljale. 

    3. Valige väljal **mahud** StorSimple mahud helitugevuse rühma lisada. 

      - Saate lisada ühe või mitme sektsioonide mahud. (Mitu sektsiooni draivi saab dünaamilise ketast või ketta mitu sektsiooni.) Helitugevuse, mis sisaldab mitut sektsioonid käsitletakse ühe üksuse. Sellest, kui lisate ainult üks sektsioonid helitugevuse rühma, kõik partitsioonid lisatakse automaatselt helitugevuse rühma samal ajal. Kui olete lisanud mitu sektsiooni helitugevuse helitugevuse rühma, mitu sektsiooni maht jätkuvalt käsitleda ühe üksuse.

      - Saate luua tühja helitugevuse rühmad ei määrate neile mis tahes mahtu. 

      - Mitte segada kobar-jagatud mahud (CSV-de kaitse) ja mitte-CSV-de kaitse sama helitugevuse rühma. StorSimple hetktõmmise haldur ei toeta CSV mahud ja-CSV mahud sama hetktõmmis. 

4. Klõpsake jaotises maht salvestamiseks nuppu **OK** .

## <a name="back-up-a-volume-group"></a>Rühma helitugevuse varundamine

Järgmiste toimingute abil saate varundada helitugevuse rühma.

#### <a name="to-back-up-a-volume-group"></a>Helitugevuse rühma varundamine

1. Töölaua ikooni alustamiseks StorSimple hetktõmmise haldur.

2. **Ulatus** paanil laiendage **Helitugevuse rühmad** , paremklõpsake helitugevuse rühma nimi ja klõpsake **Varukoopia tegemine**. 

    ![Helitugevuse rühma kohe varundamine](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_Take_backup.png)

3. Dialoogiboksis **Varukoopia tegemine** valige **Kohalik hetktõmmise** või **Cloud hetktõmmise**ja seejärel klõpsake nuppu **Loo**. 

    ![Dialoogiboksi varukoopia tegemine](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_TakeBackup_dialog.png) 

4. Veenduge, et varundamine on, laiendage **tööde haldamine** ja seejärel nuppu **tööta**. Varukoopia tuleks loetleda.

5. Lõplikus hetktõmmise vaatamiseks laiendage **Varundamise kataloogi** , laiendamine helitugevuse rühma nime ja klõpsake **Kohaliku hetktõmmise** või **Cloud hetktõmmise**. Varundus on loetletud, kui selle lõpule viidud. 

## <a name="edit-a-volume-group"></a>Helitugevuse rühma redigeerimine

Järgmiste toimingute abil helitugevuse rühma redigeerimine.

#### <a name="to-edit-a-volume-group"></a>Kui soovite helitugevuse rühma redigeerimine

1. Töölaua ikooni alustamiseks StorSimple hetktõmmise haldur.

2. **Ulatus** paanil laiendage **Helitugevuse rühmad** , paremklõpsake helitugevuse rühma nimi ja klõpsake siis nuppu **Redigeeri**. 

3. Kuvatakse dialoogiboks **Helitugevuse rühma loomine **. Saate muuta **nime**, **rakenduste**ja **mahud** kirjeid. 

4. Klõpsake muudatuste salvestamiseks nuppu **OK** .

## <a name="delete-a-volume-group"></a>Helitugevuse rühma kustutamine

Helitugevuse rühma kustutamiseks toimige järgmiselt. 

>[AZURE.WARNING] See kustutatakse ka kõiki helitugevuse rühmaga seostatud varukoopiad.

#### <a name="to-delete-a-volume-group"></a>Helitugevuse rühma kustutamine

1. Töölaua ikooni alustamiseks StorSimple hetktõmmise haldur. 

2. **Ulatus** paanil laiendage **Helitugevuse rühmad** , paremklõpsake helitugevuse rühma nime ja seejärel klõpsake käsku **Kustuta**. 

3. Kuvatakse dialoogiboks **Helitugevuse rühma kustutamine** . Tippige tekst väljale **Kinnita** ja seejärel klõpsake nuppu **OK**. 

    Kustutatud mahu rühma kaob loendist **tulemuste** paanil ja kustutatakse kõik varukoopiaid, mis on seotud selle rühma helitugevuse varukoopia kataloogist.

## <a name="next-steps"></a>Järgmised sammud

- Siit saate teada, [StorSimple hetktõmmise halduri hallata oma StorSimple lahenduse kasutamise](storsimple-snapshot-manager-admin.md)kohta.
- Siit saate teada, [StorSimple hetktõmmise halduri luua ja hallata varukoopia poliitikate kasutamise](storsimple-snapshot-manager-manage-backup-policies.md)kohta.
