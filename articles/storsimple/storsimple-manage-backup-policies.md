<properties 
   pageTitle="Teie StorSimple varukoopia poliitikate haldamine | Microsoft Azure'i"
   description="Selgitatakse, kuidas saate kasutada StorSimple halduri teenuse luua ja hallata käsitsi varukoopiate, varukoopia ajakavade ja varukoopia säilitus."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor=""/>
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="05/10/2016"
   ms.author="v-sharos"/>

# <a name="use-the-storsimple-manager-service-to-manage-backup-policies"></a>StorSimple halduri teenuse abil varukoopia poliitikate haldamine

[AZURE.INCLUDE [storsimple-version-selector-manage-backup-policies](../../includes/storsimple-version-selector-manage-backup-policies.md)]

## <a name="overview"></a>Ülevaade

Selle õpetuse selgitatakse, kuidas teie mahus StorSimple StorSimple halduri teenuse **Varukoopia poliitikate** lehe varukoopia protsesside ja varukoopia säilituspoliitika kasutamiseks. Samuti kirjeldatakse lõpuleviimiseks käsitsi varundamise.

**Varukoopia poliitikate** lehel saate varukoopia poliitikate haldamine ja kavandada kohaliku ja cloud hetktõmmiseid luua. (Varukoopia poliitikate kasutatakse konfigureerida varunduse ajakava ja mahud kogumi varukoopia säilituspoliitikate). Varukoopia poliitikad võimaldavad korraga mitu draivi hetktõmmise tegemiseks. See tähendab, et varukoopiaid, mis on loodud varukoopia poliitika krahh ühtsete koopiad. Sellel lehel loetletakse varukoopia poliitikad, nende tüüpi, seotud mahud, säilitatakse varukoopiate arvu ja võimalus neid poliitikaid.

**Varukoopia poliitikate** lehel saate ka filtreerimine olemasoleva varukoopia poliitikate ühe või mitme järgmised väljad:

- **Poliitika nimi** – seotud poliitika nimi. Erinevat tüüpi poliitika on järgmised.

   - Ajastatud poliitikate, mis on loodud konkreetselt kasutaja.
   - Automaatse poliitikad, mis luuakse siis, kui varunduse helitugevuse see suvand on lubatud maht loomise ajal. Need poliitikad on nimeks VolumeName_Default, kui maht nimi viitab StorSimple helitugevuse konfigureeritud Azure klassikaline portaalis kasutaja nimi. Automaatse poliitikate tulemuseks igapäevane cloud hetktõmmiseid alguses 22:30 seadme kellaaeg.
   - Imporditud poliitikad, mis olid algselt loodud StorSimple hetktõmmise haldur. Need on StorSimple hetktõmmise haldur host poliitikaid imporditud kaudu kirjeldav silt.

- **Maht** -mahud seotud poliitika. Kõik mahud seostatud varukoopia poliitika on rühmitatud varukoopiate loomisel.

- **Viimane õnnestunud varundus** – kuupäev ja kellaaeg viimase eduka varunduse, et see poliitikasäte.

- **Järgmise varundamise** – kuupäev ja kellaaeg alustab selle poliitika järgmine ajastatud varukoopia.

- **Ajakava** – ajakava varukoopia poliitika seostatud arvu.

On sageli kasutatud toimingud, mida saate teha sellelt lehelt.

- Lisada varukoopia poliitika 
- Lisage või muutke ajakava 
- Varukoopia poliitika kustutamine 
- Käsitsi varukoopia tegemine 
- Kohandatud varukoopia poliitika mitme mahud ja ajakava loomine 

## <a name="add-a-backup-policy"></a>Lisada varukoopia poliitika

Lisage varukoopia poliitika automaatselt ajastada varukoopiate salvestamiseks. Azure'i klassikaline portaalis varukoopia poliitika StorSimple seadme lisamiseks tehke järgmist. Pärast lisamist poliitika, saate määratleda ajakava (vt [lisamine või muutmine ajakava](#add-or-modify-a-schedule)).

[AZURE.INCLUDE [storsimple-add-backup-policy](../../includes/storsimple-add-backup-policy.md)]

![Video saadaval](./media/storsimple-manage-backup-policies/Video_icon.png) **Video saadaval**

Vaadake videot, mis näitab, kuidas luua kohalikule või pilvepõhises varukoopia poliitika, klõpsake [siin](https://azure.microsoft.com/documentation/videos/create-storsimple-backup-policies/).


## <a name="add-or-modify-a-schedule"></a>Lisage või muutke ajakava

Saate lisada või muuta ajakava, mis on seotud olemasoleva varukoopia poliitika StorSimple seadmes. Azure'i klassikaline portaalis lisada või muuta ajakava tehke järgmist.

[AZURE.INCLUDE [storsimple-add-modify-backup-schedule](../../includes/storsimple-add-modify-backup-schedule.md)]

## <a name="delete-a-backup-policy"></a>Varukoopia poliitika kustutamine

Azure'i klassikaline portaalis varukoopia poliitika StorSimple seadme kustutamiseks tehke järgmist.

[AZURE.INCLUDE [storsimple-delete-backup-policy](../../includes/storsimple-delete-backup-policy.md)]


## <a name="take-a-manual-backup"></a>Käsitsi varukoopia tegemine

Azure'i klassikaline portaalis nõudmisel (käsitsi) jaoks ühte varukoopia loomiseks tehke järgmist.

[AZURE.INCLUDE [storsimple-create-manual-backup](../../includes/storsimple-create-manual-backup.md)]

## <a name="create-a-custom-backup-policy-with-multiple-volumes-and-schedules"></a>Kohandatud varukoopia poliitika mitme mahud ja ajakava loomine

Azure'i klassikaline portaalis kohandatud varukoopia poliitika, mis sisaldab mitme mahud ja ajakava loomiseks tehke järgmist.

[AZURE.INCLUDE [storsimple-create-custom-backup-policy](../../includes/storsimple-create-custom-backup-policy.md)]


## <a name="next-steps"></a>Järgmised sammud

Lisateavet [hübriidjuurutuses StorSimple seadme StorSimple halduri teenuse kasutamise](storsimple-manager-service-administration.md)kohta.
