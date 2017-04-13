<properties 
   pageTitle="Varukoopia põhjal taastamine StorSimple helitugevuse | Microsoft Azure'i"
   description="Selgitab, kuidas kasutada StorSimple halduri teenuse varukoopia kataloogi lehe StorSimple helitugevuse taastamine varukoopia põhjal kogum."
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
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="restore-a-storsimple-volume-from-a-backup-set"></a>StorSimple helitugevuse taastamine varukoopia põhjal määramine

[AZURE.INCLUDE [storsimple-version-selector-restore-from-backup](../../includes/storsimple-version-selector-restore-from-backup.md)]

## <a name="overview"></a>Ülevaade

**Varukoopia kataloogi** kuvatakse kõik luuakse käsitsi või automaatse varundamise funktsiooni võtmisel varukoopia komplektid. Kasutage seda lehte varukoopiate varukoopia poliitika või maht, valige loendis või kustutada varukoopiaid või varukoopia abil saate taastada või klooni maht.

 ![Varukoopia kataloogi leht](./media/storsimple-restore-from-backup-set/HCS_BackupCatalog.png)

Selle õpetuse selgitatakse, kuidas kasutada **Varukoopia kataloogi** lehe seadmel heli taastamine varukoopia põhjal kogum.

## <a name="how-to-use-the-backup-catalog"></a>Varukoopia kataloogi kasutamine 

**Varukoopia kataloogi** lehelt leiate päring, mis aitab teil varukoopia määramine valiku piiritlemine. Saate filtreerida varukoopia komplekti, mis laaditakse järgmiste parameetrite põhjal:

- **Seadme** -seade, mis loodi varukoopia määramine.
- **Varundus poliitika** või **Helitugevuse** – varukoopia poliitika või helitugevuse seostatud see varukoopia komplekt.
- **Saatja** ja **et** – kuupäev ja kellaaeg vahemiku varukoopia määramine loomisel.

Filtreeritud varukoopia komplektid on siis tabelis põhjal järgmisi atribuute:

- **Nimi** – nimi varukoopia poliitika või helitugevuse seostatud varukoopia määramine.
- **Suurus** – varukoopiad määrata tegeliku suurusega.
- **Klõpsake loodud** – kuupäev ja kellaaeg, millal on loodud varukoopiaid. 
- **Tippige** – varundamise komplekti saab kohaliku pilte või pilvepõhises hetktõmmiseid. Kohaliku hetktõmmise on kõik oma seadmes talletatud kohalik helitugevuse andmete varukoopia pilve hetktõmmise viitab elavate pilveteenuses andmete varundamine. Kohaliku hetktõmmiseid juurdepääsu kiirem cloud hetktõmmiseid on valida andmete paindlikkust.
- **Algatanud** – varukoopiaid saab algatada automaatselt skeemi järgi või käsitsi kasutaja. (Saate varukoopia poliitika kavandada varukoopiaid. Teise võimalusena saate **teha varukoopia** suvand on interaktiivne varukoopia tegemiseks.)

## <a name="how-to-restore-your-storsimple-volume-from-a-backup"></a>Kuidas oma StorSimple helitugevuse varukoopia põhjal taastamine

Abil saate oma StorSimple helitugevuse teatud varukoopia põhjal taastamine **Varukoopia kataloogi** leht. 

> [AZURE.WARNING] Varukoopia põhjal taastamine asendab olemasoleva mahud varukoopia. See võib põhjustada kaotsimineku pärast varukoopia tegemist koostatud andmeid.

Enne, kui annate taastamise maht, veenduge, et helitugevus on ühenduseta. Peate tegema helitugevuse ühenduseta hosti esimese ja seejärel soovitud seade. Järgige [teha ühenduseta režiimis maht](storsimple-manage-volumes.md#take-a-volume-offline). Järgmiste toimingute maht taastamine varukoopia põhjal kogum.

### <a name="to-restore-from-a-backup-set"></a>Taastamine varukoopia põhjal määramine

1. Klõpsake lehel StorSimple halduri teenuse vahekaarti **varundamise kataloogi** .

    ![Varukoopia kataloogi](./media/storsimple-restore-from-backup-set/HCS_Restore.png)

2. Valige varukoopia on järgmine:
  1. Valige soovitud seade.
  2. Valige ripploendist, helitugevuse või varundamise poliitika varundamiseks, mida soovite valida.
  3. Määrake soovitud ajavahemiku.
  4. Klõpsake ikooni Otsi ![Märkige ruut ikoon](./media/storsimple-restore-from-backup-set/HCS_CheckIcon.png) päringu.
 
    Valitud helitugevuse seostatud varukoopiaid või varukoopia poliitika peaks kuvatama varukoopia komplekti loendis.

3. Laiendage varunduse seotud mahud kuvamiseks. Need mahud tuleb host ja seadme ühenduseta, enne kui saate need taastada. Järgige [teha ühenduseta režiimis maht](storsimple-manage-volumes.md#take-a-volume-offline).

    >  [AZURE.IMPORTANT] Veenduge, et on võtnud mahud ühenduseta hosti kõigepealt enne ühenduseta mahud seadmes. Kui võtate pole hosti mahud ühenduseta, see lubab ka potentsiaalselt viia andmete rikkumist.

4. Valige varukoopia kogum. Klõpsake lehe allosas **taastada** .

6. Teil palutakse kinnitust. 

    ![Kinnituslehel](./media/storsimple-restore-from-backup-set/HCS_ConfirmRestore.png)

7. Taastamine teave üle ja klõpsake ikooni Otsi ![kontrollida ikooni](./media/storsimple-restore-from-backup-set/HCS_CheckIcon.png). Alustamiseks käsku Taasta tööd, mis **töö** lehe kaudu saate vaadata. 

8. Kui taastamine on lõpule jõudnud, saate kontrollida, et teie mahus sisu on asendatud mahud varukoopia.

![Video saadaval](./media/storsimple-restore-from-backup-set/Video_icon.png) **Video saadaval**

Vaadake videot, mis näitab, kuidas saate kasutada klooni ja funktsioonide StorSimple taastada kustutatud faile taastada, klõpsake [siin](https://azure.microsoft.com/documentation/videos/storsimple-recover-deleted-files-with-storsimple/).

## <a name="next-steps"></a>Järgmised sammud

- Siit saate teada, kuidas [hallata StorSimple maht](storsimple-manage-volumes.md).

- Siit saate teada, [hallata StorSimple seadme StorSimple halduri teenuse kasutamise](storsimple-manager-service-administration.md)kohta.
