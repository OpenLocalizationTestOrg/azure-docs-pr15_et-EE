<properties 
   pageTitle="Varukoopia põhjal taastamine StorSimple helitugevuse | Microsoft Azure'i"
   description="Selgitab, kuidas kasutada StorSimple halduri teenuse varukoopia kataloogi lehe StorSimple helitugevuse taastamine varukoopia põhjal kogum."
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

# <a name="restore-a-storsimple-volume-from-a-backup-set-update-2"></a>StorSimple helitugevuse taastamine varukoopia põhjal määramine (Värskenda 2)

[AZURE.INCLUDE [storsimple-version-selector-restore-from-backup](../../includes/storsimple-version-selector-restore-from-backup.md)]

## <a name="overview"></a>Ülevaade

**Varukoopia kataloogi** kuvatakse kõik luuakse käsitsi või automaatse varundamise funktsiooni võtmisel varukoopia komplektid. Kasutage seda lehte varukoopiate varukoopia poliitika või maht, valige loendis või kustutada varukoopiaid või varukoopia abil saate taastada või klooni maht.

 ![Varukoopia kataloogi leht](./media/storsimple-restore-from-backup-set-u2/restore.png)

Selle õpetuse selgitatakse, kuidas kasutada **Varukoopia kataloogi** lehe seadme taastamine varukoopia põhjal kogum.

Saate taastada maht kohalikule või pilve varukoopia. Mõlemal juhul taastetoimingu toob võrgus helitugevuse kohe, kui andmed on alla laaditud taustal. 

Enne, kui annate taastetoimingu, arvestage järgmist:

- **Võtate helitugevuse ühenduseta** – võta helitugevuse ühenduseta nii selle hosti ja seadme enne algatada taastetoimingu. Kuigi taastetoimingu toob automaatselt võrgus helitugevuse seadmes, tuleb käsitsi tuua seade võrgus Host. Saate tuua võrgus helitugevuse hosti niipea, kui maht on võrguühendus seadmes. (Pole peate ootama kuni taastetoimingu on lõppenud.) Toimingute, minge [teha ühenduseta režiimis maht](storsimple-manage-volumes-u2.md#take-a-volume-offline).

- **Helitugevuse tüüp pärast taastamine** – kustutatud mahu taastatakse sõltuvalt hetktõmmis; mahud, mis on kinnitatud kohalikult taastatakse nimega kohalikult kinnitatud mahud ja mahud, mis olid mitmetasandilise taastatakse nimega astmeline maht.

    Olemasolevate väljaannete, alistab praeguse kasutamine tüüp helitugevuse tüüp, mis on talletatud hetktõmmis. Näiteks kui taastate maht hetktõmmise, mis võeti helitugevuse tüüp on mitmetasandilise ja draivi tüüp on nüüd kohalikult kinnitatud (tõttu teisendamine toiming, mille), siis maht taastatakse nimega kohalikult kinnitatud helitugevust. Samamoodi kui olemasoleva kohalikult kinnitatud draivi laiendatud ja hiljem taastada mõne vanema hetktõmmise võtta, kui maht on väiksem, jääb taastatud helitugevuse praeguse laiendatud suurus.

    Te ei saa teisendada maht, astmeline helitugevuse kohalikult kinnitatud-ni või kohalikult kinnitatud helitugevuse astmeline maht ajal helitugevuse taastatakse. Oodake, kuni taastetoimingu on lõpule jõudnud, ja seejärel saate teisendada helitugevuse mõnda muud tüüpi. Minge maht teisendamise kohta leiate teavet [Helitugevuse tüübi muutmine](storsimple-manage-volumes-u2.md#change-the-volume-type). 

- **Mahu suurus kajastub taastatud helitugevuse** – see on oluline, kui taastate kohalikult kinnitatud maht, mis on kustutatud (kuna kohalikult kinnitatud maht on täielikult ette valmistatud). Veenduge, et teil on piisavalt ruumi, enne kui proovite taastada kohalikult kinnitatud maht, mis on varem kustutatud. 

- **Te ei saa laiendada helitugevust, kui see on taastatud** – oodake, kuni taastetoimingu on lõpule jõudnud, enne kui proovite helitugevuse laiendamiseks. Lisateabe saamiseks laiendamine maht, minge [Muuda maht](storsimple-manage-volumes-u2.md#modify-a-volume).

- **Saate teha varukoopia ajal taastate kohaliku helitugevuse** – toimingute lugege [kasutamine StorSimple halduri teenuse varukoopia poliitikate haldamine](storsimple-manage-backup-policies.md).

- **Saate tühistada taastamine toimingu** – kui tühistate tellimuse taastamine töö ja seejärel helitugevus on olekusse, mis sel oli enne teie alustatud taastetoimingu tagasi pöörata. Toimingute, minge [katkestamine](storsimple-manage-jobs-u2.md#cancel-a-job).

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

Abil saate oma StorSimple helitugevuse teatud varukoopia põhjal taastamine **Varukoopia kataloogi** leht. Pidage meeles, taastamine maht taas helitugevuse kui varukoopia võeti riigile. Andmed, mis lisati pärast selle toimingu varukoopia lähevad kaotsi.

> [AZURE.WARNING] Varukoopia põhjal taastamine asendab olemasoleva mahud varukoopia. See võib põhjustada kaotsimineku pärast varukoopia tegemist koostatud andmeid.

### <a name="to-restore-your-volume"></a>Teie helitugevuse taastamine

1. Klõpsake lehel StorSimple halduri teenuse vahekaarti **varundamise kataloogi** .

    ![Varukoopia kataloogi](./media/storsimple-restore-from-backup-set-u2/restore.png)

2. Valige varukoopia on järgmine:
  1. Valige soovitud seade.
  2. Valige ripploendist, helitugevuse või varundamise poliitika varundamiseks, mida soovite valida.
  3. Määrake soovitud ajavahemiku.
  4. Klõpsake ikooni Otsi ![Märkige ruut ikoon](./media/storsimple-restore-from-backup-set-u2/HCS_CheckIcon.png) päringu.
 
    Valitud helitugevuse seostatud varukoopiaid või varukoopia poliitika peaks kuvatama varukoopia komplekti loendis.

3. Laiendage varunduse seotud mahud kuvamiseks. Need mahud tuleb host ja seadme ühenduseta, enne kui saate need taastada. Juurdepääs mahud **Helitugevuse ümbriste** lehel ja seejärel järgige [võtta maht ühenduseta](storsimple-manage-volumes-u2.md#take-a-volume-offline) võrguühenduseta neid juhiseid.

    > [AZURE.IMPORTANT] Veenduge, et on võtnud mahud ühenduseta hosti kõigepealt enne ühenduseta mahud seadmes. Kui võtate pole hosti mahud ühenduseta, see lubab ka potentsiaalselt viia andmete rikkumist.

4. Liikuge tagasi **Varukoopia kataloogi** menüü ja valige varukoopia kogum.

5. Klõpsake lehe allosas **taastada** .

6. Teil palutakse kinnitust. Taastamine teave üle ja seejärel märkige ruut kinnituse.

    ![Kinnituslehel](./media/storsimple-restore-from-backup-set-u2/ConfirmRestore.png)

7. Klõpsake ikooni Otsi ![kontrollida ikooni](./media/storsimple-restore-from-backup-set-u2/HCS_CheckIcon.png). Alustamiseks käsku Taasta tööd, mis **töö** lehe kaudu saate vaadata. 

8. Kui taastamine on lõpule jõudnud, saate kontrollida, et teie mahus sisu on asendatud mahud varukoopia.

![Video saadaval](./media/storsimple-restore-from-backup-set-u2/Video_icon.png) **Video saadaval**

Vaadake videot, mis näitab, kuidas saate kasutada klooni ja funktsioonide StorSimple taastada kustutatud faile taastada, klõpsake [siin](https://azure.microsoft.com/documentation/videos/storsimple-recover-deleted-files-with-storsimple/).

## <a name="if-the-restore-fails"></a>Kui taastamine nurjub

Saate teatise, kui taastetoimingu ei õnnestu mingil põhjusel. Sel juhul värskendage varukoopia loendit ja veenduge, et varundamine on kehtiv. Kui varundus on lubatud ja taastate pilvest, siis ühenduvusprobleemide võib probleeme põhjustada. 

Taastamine toimingu lõpuleviimiseks võtta helitugevuse ühenduseta hosti ja proovige uuesti taastamine. Pange tähele, et helitugevuse andmeid, mis on tehtud ajal taastamine muudatustest töötlemine lähevad kaotsi.

## <a name="next-steps"></a>Järgmised sammud

- Siit saate teada, kuidas [hallata StorSimple maht](storsimple-manage-volumes-u2.md).

- Siit saate teada, [hallata StorSimple seadme StorSimple halduri teenuse kasutamise](storsimple-manager-service-administration.md)kohta.
