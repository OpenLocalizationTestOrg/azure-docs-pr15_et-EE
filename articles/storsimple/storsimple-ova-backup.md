<properties 
   pageTitle="StorSimple virtuaalse massiiv varukoopia õpetuse | Microsoft Azure'i"
   description="Selles artiklis kirjeldatakse StorSimple virtuaalse massiiv osakud ja mahud varundada."
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
   ms.date="06/07/2016"
   ms.author="alkohli" />

# <a name="back-up-your-storsimple-virtual-array"></a>Varundage oma StorSimple virtuaalse massiiv

## <a name="overview"></a>Ülevaade 

Selle õpetuse kehtib Microsoft Azure'i StorSimple virtuaalse massiiv (tuntud ka kui StorSimple kohapealse virtuaalse seadme või StorSimple virtuaalse seadme) töötava märts 2016 üldiselt kättesaadav (GA) Väljalaske- või uuemad versioonid.

StorSimple virtuaalse massiiv on hübriid pilve kohapealse virtuaalse mäluseade, mis saab konfigureerida failiserverisse või iSCSI serverisse. See varundamiseks, varukoopiate põhjal taastamine ja teha seadme Tõrkesiirde avariitaastet vajadusel. Kui fail server on konfigureeritud, võimaldab see ka Üksusetaseme taastamine. Selles õpetuses kirjeldatakse, kuidas luua oma StorSimple virtuaalse massiiv käsitsi ja ajastatud varukoopiaid Azure klassikaline portaali või StorSimple web UI abil.


## <a name="back-up-shares-and-volumes"></a>Varundamine osakud ja mahud

Varukoopiate punkti /-kellaaja kaitse parandada taastatavus ja taastamine korda osakud ja mahtu vähendada. Saate varundada võrguketta või helitugevuse StorSimple seadmes on kaks võimalust: **ajastatud** või **käsitsi**. Järgmistes lõikudes käsitletakse meetodi.

> [AZURE.NOTE] Selles väljaandes ajastatud varukoopiaid on loodud vaikepoliitika, mida iga päev määratud ajal käivitub ja varundatakse aktsiate või mahud seade. Ei ole võimalik luua kohandatud poliitikate ajastatud varukoopiaid sel ajal.

## <a name="change-the-backup-schedule"></a>Varunduse ajakava muutmine

Seadme StorSimple virtuaalse on varukoopia vaikepoliitika, mis ajavahemiku päeva (22:30) algab ja varundab aktsiate või mahud seadme kord päevas. Saate muuta aeg, mille varukoopia käivitub, kuid sagedus ja säilitamise (mis määrab varukoopiate säilitamise arvu) ei saa muuta. Nende varundamise ajal kogu virtuaalse seadme varundatakse; Soovitame, et ajastada need varukoopiate aegsasti.

[Azure'i klassikaline portaali](https://manage.windowsazure.com/) vaikimisi varukoopia algusaja muutmiseks tehke järgmist.

#### <a name="to-change-the-start-time-for-the-default-backup-policy"></a>Algusaja seadmine varukoopia vaikepoliitika muutmiseks

1. Liikuge vahekaardile seadme **konfigureerimine** .

2. Määrake jaotises **varukoopia** algusaja seadmine päev varukoopia.

3. Klõpsake nuppu **Salvesta**.

### <a name="take-a-manual-backup"></a>Käsitsi varukoopia tegemine

Lisaks ajastatud varukoopiaid, saate teha käsitsivormingu (vajadusel) varukoopia igal ajal.

#### <a name="to-create-a-manual-on-demand-backup"></a>Käsitsi (vajadusel) varukoopia loomine

1. Liikuge vahekaardil **ühtlasi** või **maht** .

2. Klõpsake lehe allosas nuppu **varundus kõik**. Teil palutakse kinnitamaks, et soovite nüüd varukoopia tegemiseks. Klõpsake ikooni Otsi ![kontrollida ikooni](./media/storsimple-ova-backup/image3.png) jätkata varukoopia.

    ![varukoopia kinnituse](./media/storsimple-ova-backup/image4.png)

    Kuvab teate, et Varundustöö hakkab.

    ![varundus käivitamine](./media/storsimple-ova-backup/image5.png)

    Kuvab teate, et töö on loodud.

    ![Varundustöö loodud](./media/storsimple-ova-backup/image7.png)

3. Töö edenemist jälgida, klõpsake nuppu **Kuva töö**.

4. Kui Varundustöö on lõpule jõudnud, avage vahekaart **varundamise kataloogi** . Peaksite nägema lõplikus varukoopia.

    ![Lõplikus varundamine](./media/storsimple-ova-backup/image8.png)

5. Määratud filtri Valikud soovitud seade, varukoopia poliitika ja ajavahemiku, ja seejärel klõpsake ikooni Otsi ![Märkige ruut ikoon](./media/storsimple-ova-backup/image3.png).

    Varukoopia tuleks kuvatakse varukoopia komplekti loend, mis kuvatakse kataloogi.

## <a name="view-existing-backups"></a>Saate vaadata olemasolevaid varukoopiate

Azure'i klassikaline portaalis olemasoleva varukoopiate vaatamiseks tehke järgmist.

#### <a name="to-view-existing-backups"></a>Olemasoleva varukoopiate kuvamiseks

1. Klõpsake lehel StorSimple halduri teenuse vahekaarti **varundamise kataloogi** .

2. Valige varukoopia on järgmine:

    1. Valige soovitud seade.

    2. Valige ripploendist, ühiskasutus või helitugevust varundamise, mida soovite valida.

    3. Määrake soovitud ajavahemiku.

    4. Klõpsake ikooni Otsi ![](./media/storsimple-ova-backup/image3.png) selle päringu käivitamiseks.

    Varukoopiaid seostatud valitud osa või märkida varukoopia komplekti loendis maht.

![video_icon](./media/storsimple-ova-backup/video_icon.png) **Video saadaval**

Vaadake videot, et näha, kuidas saate luua ühtlasi, ühtlasi varundamine ja taastamine andmete StorSimple virtuaalse massiivi.

> [AZURE.VIDEO use-the-storsimple-virtual-array]

## <a name="next-steps"></a>Järgmised sammud

Lisateave [teie StorSimple virtuaalse massiiv haldamise](storsimple-ova-web-ui-admin.md)kohta.
