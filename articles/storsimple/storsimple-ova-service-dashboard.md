<properties 
   pageTitle="StorSimple halduri teenuse armatuurlaua - virtuaalse massiiv | Microsoft Azure'i"
   description="Armatuurlaua StorSimple halduri teenuse kirjeldatakse ja selgitatakse, kuidas selle abil oma StorSimple virtuaalse massiiv seisundi jälgimine."
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
   ms.date="04/07/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-dashboard-for-the-storsimple-virtual-array"></a>Armatuurlaua StorSimple halduri teenuse kasutamiseks StorSimple virtuaalse massiiv

## <a name="overview"></a>Ülevaade

Armatuurlaualehe StorSimple halduri teenuse pakub koondvaate StorSimple virtuaalse massiivi (tuntud ka kui StorSimple kohapealse virtuaalse või virtuaalse seadmed) mis on ühendatud StorSimple halduri teenuse, esiletõstmine need, mis tuleb süsteemiadministraator on tähelepanu. Selle õpetuse tutvustab Armatuurlaua leht, selgitatakse armatuurlaua sisu ja funktsioon ja kirjeldatakse toiminguid, mida saate teha sellelt lehelt.

![Teenuste armatuurlaud](./media/storsimple-ova-service-dashboard/dashboard1.png)

Armatuurlaua StorSimple halduri teenuse kuvatakse järgmine teave:

- **Lehe ülaosas diagrammiala,** näete olulised mõõdikud oma virtuaalse seadmete jaoks. Saate vaadata kõigis seadmetes virtuaalse kasutatud esmane talletamist, samuti salvestusruumi, virtuaalse seadmed tarbitud aja jooksul. Juhtelementide kasutamine paremas ülanurgas diagrammi määramiseks suhteline või absoluutne kasutus ja 1-nädal, 1 kuu, 3 kuud või 1 aasta ajaskaala kuvamiseks. Kasutage värskendamise reguleerimine ![värskendamise reguleerimine](./media/storsimple-ova-service-dashboard/refresh-control.png) diagrammi värskendamiseks.

- **Kasutus ülevaade** kuvatakse esmane talletamist, mis on ette valmistatud ja tarbitud kõik virtuaalse seadmete suhtes saadaoleva salvestus, mis on saadaval kõigis seadmetes virtuaalse. **Provisioned** viitab talletusmahtu, mis on valmis ja eraldatud kasutamiseks, **kasutatakse** viitab ühtlasi või maht on vaadatud algataja, mis on ühendatud virtuaalse seadmete kasutamist ja **Max võimsus** kuvatakse kõik virtuaalse seadmete maksimaalne võimsus.

- **Teatiste** ala pakub kõik teatised aktiivsete hetktõmmise kõigis virtuaalse seadmetes, teatiste raskusaste Rühmitusalus. Raskusaste tase klõpsates avatakse veebirakendust nende teatiste kuvamiseks klõpsake lehe **teatiste** . Lehel **teatised** , võite klõpsata üksikute teatise selle teatise, sh võimalikke toiminguid täiendavate üksikasjade kuvamiseks. Kui probleem on lahendatud, saate ka teatise tühjendage.

- **Töö** ala üle virtuaalse kõigis seadmetes, mis on seotud teie teenuse pakub hetktõmmise tehtud töö. Seal on lingid, mille abil saate vaadata tööd, mis on praegu ja need, mis õnnestus või mitte viimase 24 tunni jooksul. 

- Lehe paremal alale **kiire ülevaade** pakub kasulikku teavet, nt teenuse olek, virtuaalse teenus, teenuse asukoht ja üksikasjad tellimuse, mis on seotud teenuse ühendatud seadmete koguarv. On ka link Logi toimingud. Klõpsake linki loendi kõigi lõpuleviidud StorSimple halduri toimingud. 

Armatuurlaualehe StorSimple halduri teenuse abil saate alustada järgmisi toiminguid:

- Saada teenuse registreerimise võti.
- Saate vaadata tegevuse logid.

## <a name="get-the-service-registration-key"></a>Saada teenuse registreerimise võti

Teenuse registreerimise võti kasutatakse teenusega StorSimple halduri StorSimple virtuaalse seadme registreerimiseks nii, et seade kuvatakse Azure klassikaline portaalis täpsemaks haldamise toimingute jaoks. Võti esimese virtuaalse seadmes on loodud ja ülejäänud virtuaalse seadmete ühiskasutusse. 

Klõpsates **Registreerimise võti** (lehekülje allosas) avab dialoogiboksi **Teenuse registreerimise võti** , kus saate praeguse teenuse registreerimise võti kopeerimine lõikelauale või taastada teenuse registreerimise võti.

![registreerimise võti](./media/storsimple-ova-service-dashboard/service-dashboard3.png)

Võti regenereeruvate ei mõjuta varem registreeritud virtuaalne seadmed: see mõjutab vaid pärast võtme uuesti on registreerunud teenuse virtuaalse seadmete.

Lisateavet selle kohta, kuidas teenuse registreerimise võti, valige [Hangi teenuse registreerimise võti](storsimple-ova-manage-service.md#get-the-service-registration-key).

## <a name="view-the-operations-logs"></a>Toimingute logid kuvamine

Saate vaadata toiming logid linki toiming logid **kiirpilgupaanil armatuurlaua** saadaval. See viib teid halduse teenuste lehe, kus saate filtreerida ja logid teatud StorSimple halduri teenust.

![Toimingute Logi](./media/storsimple-ova-service-dashboard/ops-log.png)

## <a name="next-steps"></a>Järgmised sammud

Siit saate teada, [kohaliku web UI hallata oma StorSimple virtuaalse massiiv kasutamise](storsimple-ova-web-ui-admin.md)kohta.