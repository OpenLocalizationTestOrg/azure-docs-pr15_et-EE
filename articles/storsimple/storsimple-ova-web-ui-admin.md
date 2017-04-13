<properties 
   pageTitle="StorSimple virtuaalse massiiv web UI Administreerimine | Microsoft Azure'i"
   description="Selles artiklis kirjeldatakse kaudu StorSimple virtuaalse massiiv web UI põhilised haldustoimingute tegemine."
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
   ms.date="04/07/2016"
   ms.author="alkohli" />

# <a name="use-the-web-ui-to-administer-your-storsimple-virtual-array"></a>Kasutajaliides Web abil saate hallata oma StorSimple virtuaalse massiiv

![häälestamise protsessi kulgemist](./media/storsimple-ova-web-ui-admin/manage4.png)

## <a name="overview"></a>Ülevaade

Selle artikli õpetused rakendada Microsoft Azure'i StorSimple virtuaalse massiiv (tuntud ka kui StorSimple kohapealse virtuaalse seadme) töötab märts 2016 üldiselt kättesaadav (GA) väljaanne. Selles artiklis kirjeldatakse mõningaid keerukate töövood ja haldamise toiminguid, mida saab teha StorSimple virtuaalse massiiv. StorSimple virtuaalse massiiv seadme StorSimple halduri teenuse Kasutajaliidese (nimetatakse portaali UI) ja kohalik web Kasutajaliidese abil saate hallata. See artikkel keskendub tööülesanded saate kasutajaliides web abil teha.

See artikkel sisaldab järgmist õpetused:

- Krüptovõtme teenuse andmete hankimine
- Web UI häälestamise tõrkeotsing
- Luua Logi pakett
- Sulgege ja taaskäivitage oma seade

## <a name="get-the-service-data-encryption-key"></a>Krüptovõtme teenuse andmete hankimine

Teenuse andmete krüptovõtme luuakse StorSimple halduri teenuse esimese seadme registreerimisel. See funktsioon on seejärel nõutav teenuse registreerimise võti StorSimple halduri teenuse täiendavad seadmed registreerida.

Kui teil on oma teenuse andmete krüptovõtme ja tuua seda vaja, tehke järgmised juhised kohaliku web UI seadme registreeritud teenust.

#### <a name="to-get-the-service-data-encryption-key"></a>Saada teenuse andmete krüptovõtme

1. Ühenduse loomine kohaliku web UI. Minge **konfiguratsiooni** > **Cloud sätted**.
  

2. Klõpsake lehe allosas nuppu **saada teenuse andmete krüptovõtme**. Kuvatakse klahvi. Kopeerige ja salvestage see võti.
    
    ![saada teenuse andmete krüptovõtme 1](./media/storsimple-ova-web-ui-admin/image27.png)
   


## <a name="troubleshoot-web-ui-setup-errors"></a>Web UI häälestamise tõrkeotsing

Mõnel juhul kui konfigureerite seadme kaudu kohaliku web UI, võivad tekkida tõrked. Diagnoosimine ja sellise tõrkeotsing, käivitada diagnostika testide.

#### <a name="to-run-the-diagnostic-tests"></a>Diagnostika testi käivitamine

1. Avage kohaliku web UI **tõrkeotsing** > **diagnostika kontrollib**.

    ![käivitage diagnostika 1](./media/storsimple-ova-web-ui-admin/image29.png)

2. Klõpsake lehe allosas nuppu **Käivita diagnostika kontrollib**. See alustab kontrollib diagnoosida võimalikke probleeme oma võrgu, seadme, veebiteenuse puhverserveri, kellaaja või pilveteenuse sätted. Kuvab teate, et seadmes töötaks kontrollib.

3. Kui katsete on lõpule jõudnud, kuvatakse tulemused. Järgmises näites on kujutatud diagnostika tulemused. Pange tähele, et web puhverserveri sätted pole konfigureeritud selle seadme seetõttu web puhverserveri test ei tööta. Kõik katsed võrgu sätteid, DNS-serveri ja kellaajasätete olid edukamad.

    ![käivitage diagnostika 2](./media/storsimple-ova-web-ui-admin/image30.png)

## <a name="generate-a-log-package"></a>Luua Logi pakett

Log paketi hulka kuuluvad asjakohaste logid, mis aitab Microsoft Support mis tahes seadme probleemide tõrkeotsing. See väljaanne saab luua Logi paketi kohaliku veebi Kasutajaliidese kaudu.

#### <a name="to-generate-the-log-package"></a>Log paketi loomiseks

1. Avage kohaliku web UI **tõrkeotsing** > **süsteemi logid**.

    ![luua Logi pakett 1](./media/storsimple-ova-web-ui-admin/image31.png)

2. Klõpsake lehe allosas nuppu **Loo log paketi**. Paketi süsteemi logid luuakse. See viib paar minutit.

    ![luua Logi pakett 2](./media/storsimple-ova-web-ui-admin/image32.png)

    Pärast paketi on loodud ja lehe värskendatakse tähistamiseks paketi loomise kuupäeva ja kellaaja, teavitatakse teid sellest.

    ![luua Logi pakett 3](./media/storsimple-ova-web-ui-admin/image33.png)

3. Klõpsake nuppu **log laadige**. ZIP paketi laaditakse teie arvutisse.

    ![luua Logi paketi 4](./media/storsimple-ova-web-ui-admin/image34.png)

4. Saate pakkige see lahti allalaaditud log paketi ja vaadata süsteemi logifailid.

## <a name="shut-down-and-restart-your-device"></a>Sulgege ja taaskäivitage oma seade

Saate välja lülitada või kohalik web Kasutajaliidese abil virtuaalse seadme taaskäivitamine. Me soovitame enne taaskäivitamist, võtke mahud või ühtlasi ühenduseta selle hosti ja seejärel soovitud seade. See minimeerida võimalust andmete rikkumist. 

#### <a name="to-shut-down-your-virtual-device"></a>Sulgeda virtuaalse seadme

1. Kohaliku web UI, minge **hooldustööd** > **Power sätted**.

2. Klõpsake lehe allosas nuppu **sulgumist**.

    ![seadme sulgumist 1](./media/storsimple-ova-web-ui-admin/image36.png)

3. Hoiatus kuvatakse selle kohta, et seadme suletakse katkestab mis tahes IO, mis olid pooleli, tulemuseks on tööseisakute. Klõpsake ikooni Otsi ![Märkige ruut ikoon](./media/storsimple-ova-web-ui-admin/image3.png).

    ![seadme sulgumist hoiatus](./media/storsimple-ova-web-ui-admin/image37.png)

    Kuvab teate, et selle värskendust.

    ![seadme sulgumist alustamine](./media/storsimple-ova-web-ui-admin/image38.png)

    Seadme sulgub kohe. Kui soovite alustada oma seadmesse, peate Hyper-V halduri kaudu.

#### <a name="to-restart-your-virtual-device"></a>Kui soovite virtuaalse seadme taaskäivitamine

1. Kohaliku web UI, minge **hooldustööd** > **Power sätted**.

2. Klõpsake lehe allosas nuppu **uuesti**.

    ![seadme taaskäivitamine](./media/storsimple-ova-web-ui-admin/image36.png)

3. Hoiatus kuvatakse selle kohta, et seadme taaskäivitamine katkestada mis tahes IOs, mis olid pooleli, tulemuseks on tööseisakute. Klõpsake ikooni Otsi ![Märkige ruut ikoon](./media/storsimple-ova-web-ui-admin/image3.png).

    ![taaskäivitage hoiatus](./media/storsimple-ova-web-ui-admin/image37.png)

    Kuvab teate, et uuesti algatada.

    ![uuesti alustada](./media/storsimple-ova-web-ui-admin/image39.png)

    Uuesti ajal kaotate ühenduse UI. Saate jälgida, värskendades UI perioodiliselt uuesti. Teise võimalusena saate jälgida seadme taaskäivitamist olek Hyper-V halduri kaudu.

## <a name="next-steps"></a>Järgmised sammud

Siit saate teada, [hallata oma seadme StorSimple halduri teenuse kasutamise](storsimple-manager-service-administration.md)kohta.
