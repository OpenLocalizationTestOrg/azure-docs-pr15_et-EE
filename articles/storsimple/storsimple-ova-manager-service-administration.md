<properties 
   pageTitle="StorSimple halduri Virtual massiiv Administreerimine | Microsoft Azure'i"
   description="Siit saate teada, kuidas hallata oma StorSimple kohapealse virtuaalse massiiv Azure klassikaline portaalis StorSimple halduri teenuse abil."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/11/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-administer-your-storsimple-virtual-array"></a>StorSimple halduri teenuse abil saate hallata oma StorSimple virtuaalse massiiv

![häälestamise protsessi kulgemist](./media/storsimple-ova-manager-service-administration/manage4.png)

## <a name="overview"></a>Ülevaade

Selles artiklis kirjeldatakse StorSimple halduri teenuse kasutajaliides, kuidas ühendada see ja erinevaid võimalusi, sealhulgas ja lingid teatud töövood, mida saab teha seda Kasutajaliidese kaudu. 

Pärast selle artikli lugemist te teate, kuidas:

- StorSimple halduri teenusega ühenduse loomiseks
- Liikuge StorSimple halduri kasutajaliides
- StorSimple virtuaalse massiiv StorSimple halduri teenuse kaudu hallata

> [AZURE.NOTE] Haldamise võimalusi StorSimple 8000 sarja seadme vaatamiseks avage [kasutamine StorSimple halduri teenuse haldamiseks StorSimple seadme](storsimple-manager-service-administration.md).

## <a name="connect-to-the-storsimple-manager-service"></a>StorSimple halduri teenusega ühenduse loomiseks

StorSimple halduri teenuse Microsoft Azure'i töötab ja ühendab mitu StorSimple virtuaalse massiivi. Keskse Microsoft Azure'i klassikaline portaali töötavad brauseris abil saate hallata järgmistesse seadmetesse. StorSimple halduri teenusega ühenduse loomiseks tehke järgmist.

#### <a name="to-connect-to-the-service"></a>Teenusega ühenduse

1. Avage [https://manage.windowsazure.com/](https://manage.windowsazure.com/).

2. Microsoft Azure'i klassikaline portaali (asub paani paremas ülanurgas) logige oma Microsofti konto identimisteave abil.

3. Liikuge kerides allapoole StorSimple halduri teenuse Vasakpoolsel navigeerimispaanil.

    ![liikuge kerides teenus](./media/storsimple-ova-manager-service-administration/admin-scroll.png)

## <a name="navigate-the-storsimple-manager-service-ui"></a>Liikuge StorSimple halduri teenuse kasutajaliides

Navigeerimise hierarhia StorSimple halduri teenuse UI on näidatud järgmises tabelis.

- **StorSimple halduri** sihtleht suunab teid kõik virtuaalse massiivi teenuse raames lehtede UI teenusetaseme kuvamine.

- **Seadmete** lehe suunab teid – seadmetasemel UI lehtede kehtivad teatud virtuaalse massiiv.

#### <a name="storsimple-manager-service-navigational-hierarchy"></a>StorSimple halduri teenuse navigeerimise hierarhia

|Sihtleht|Teenuse taseme lehed|Seadme taseme lehed|
|---|---|---|
|StorSimple halduri teenuse|Armatuurlaua (teenus)|Armatuurlaua (seadme)|
||Seadmed →|Jälgimine|
||Varukoopia kataloogi|Ühtlasi (faili server) või </br>Maht (iSCSI server)|
||Konfigureerimine (teenus)|Konfigureerimine (seadme)|
||Tööde haldamine|Hooldustööd|
||Teatised|

## <a name="use-the-storsimple-manager-service-to-perform-management-tasks"></a>Haldustoimingute sooritamiseks StorSimple halduri teenuse kasutamine

Järgmises tabelis on kokkuvõte tavalisi ja keerukate töövooge, mis tuleks teha StorSimple halduri teenuse kasutajaliides. Järgmised toimingud on korraldatud vastavalt nende algatatakse UI lehtedel.

Iga töövoo kohta lisateabe saamiseks klõpsake tabeli protseduuri.

#### <a name="storsimple-manager-workflows"></a>StorSimple halduri töövood

|Kui soovite selle tegemiseks...|Minge selle Kasutajaliidese lehe...|Selle toiminguga|
|---|---|---|
|Teenuse loomine</br>Teenuse kustutamine</br>Saada teenuse registreerimise võti</br>Taastada teenuse registreerimise võti|StorSimple halduri teenuse|[StorSimple halduri teenuse juurutamine](storsimple-ova-manage-service.md)|
|Krüptovõtme teenuse andmete muutmine</br>Toimingute logid kuvamine|StorSimple halduri teenuse → armatuurlaud|[Kasutage StorSimple teenuste armatuurlaud](storsimple-ova-service-dashboard.md)|
|Virtuaalne massiiv desaktiveerimine</br>Virtuaalne massiiv kustutamine|StorSimple halduri teenuse → seadmed|[Desaktiveerimine ja kustutamine virtuaalse massiiv](storsimple-ova-deactivate-and-delete-device.md)|
|Katastroofi taastamine ja seadme Tõrkesiirde</br>Tõrkesiirde eeltingimused</br>Virtuaalne seadme Tõrkesiirde</br>Järjepidevuse avariitaastet (BCDR)</br>Tõrgete katastroofiabi ajal|StorSimple halduri teenuse → seadmed|[Katastroofi taastamine ja seadme Tõrkesiirde oma StorSimple virtuaalse massiiv](storsimple-ova-failover-dr.md)|
|Varundamine osakud ja mahud</br>Käsitsi varukoopia tegemine</br>Varunduse ajakava muutmine</br>Saate vaadata olemasolevaid varukoopiate|StorSimple halduri → varundamise kataloog|[Varundage oma StorSimple virtuaalse massiiv](storsimple-ova-backup.md)|
|Ühtlasi taastamine varukoopia põhjal määramine</br>Mahud taastamine varukoopia põhjal määramine</br>Üksusetaseme taastamine (ainult faili server)|StorSimple halduri teenuse → varukoopia kataloogi|[Teie StorSimple virtuaalse massiiv varukoopia põhjal taastamine](storsimple-ova-restore.md)|
|Salvestusruumi kontode kohta</br>Salvestusruumi konto lisamine</br>Salvestusruumi konto redigeerimine</br>Salvestusruumi konto kustutamine|StorSimple halduri teenuse → konfigureerimine|[Salvestusruumi kontod StorSimple virtuaalse massiivi haldamine](storsimple-ova-manage-storage-accounts.md)|
|Juurdepääsu juhtimine kirjete kohta</br>Lisage või muutke kirje juurdepääsu juhtimine </br>Accessi juhtelement kirje kustutamine|StorSimple halduri teenuse → konfigureerimine|[StorSimple virtuaalse massiivi juurdepääsu juhtimine kirjeid hallata](storsimple-ova-manage-acrs.md)|
|Projekti üksikasjade kuvamine|StorSimple halduri teenuse → tööde haldamine| [Hallata StorSimple virtuaalse massiiv](storsimple-ova-manage-jobs.md)|
|Teatiste sätete konfigureerimine</br>Teatiste vastuvõtmine</br>Teatiste haldamine</br>Vaadake üle teatised|StorSimple halduri teenuse → teatiste|[Saate vaadata ja StorSimple virtuaalse massiivi teatiste haldamine](storsimple-ova-manage-alerts.md)|
|Seadme administraatori parooli muutmine|Seadmete StorSimple halduri teenuse → → konfigureerimine|[StorSimple virtuaalse massiiv seadme administraatori parooli muutmine](storsimple-ova-change-device-admin-password.md)|
|Tarkvara värskenduste installimine|Seadmete StorSimple halduri teenuse → → hooldustööd|[Värskendage oma virtuaalse massiiv](storsimple-ova-install-update-01.md)|

>[AZURE.NOTE] Peate kasutama [kohalikku web UI](storsimple-ova-web-ui-admin.md) järgmisi toiminguid:
>
>- [Krüptovõtme teenuse andmete toomiseks](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key)
>- [Tugiteenuste paketi loomine](storsimple-ova-web-ui-admin.md#generate-a-log-package)
>- [Peatage ja taaskäivitage virtuaalse massiiv](storsimple-ova-web-ui-admin.md#shut-down-and-restart-your-device)

##<a name="next-steps"></a>Järgmised sammud
Teavet kasutajaliides web ja kuidas seda kasutada, avage [StorSimple veebi UI hallata oma StorSimple virtuaalse massiiv](storsimple-ova-web-ui-admin.md).
