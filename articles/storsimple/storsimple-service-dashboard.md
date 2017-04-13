<properties
   pageTitle="StorSimple halduri teenuse armatuurlaua | Microsoft Azure'i"
   description="Armatuurlaua StorSimple halduri teenuse kirjeldatakse ja selgitatakse, kuidas selle abil saate jälgida tervis lahendusse StorSimple."
   services="storsimple"
   documentationCenter=""
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags
   ms.service="storsimple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="05/24/2016"
   ms.author="v-sharos" />

# <a name="use-the-storsimple-manager-service-dashboard"></a>Armatuurlaua StorSimple halduri teenuse kasutamine

## <a name="overview"></a>Ülevaade

Armatuurlaualehe StorSimple halduri teenuse pakub kõigis seadmetes, mis on ühendatud StorSimple halduri teenuse, esiletõstmine need, mis tuleb süsteemiadministraator on tähelepanu Kokkuvõte vaadet. Selle õpetuse tutvustab Armatuurlaua leht, selgitab armatuurlaua sisu ja funktsioon ja kirjeldatakse toiminguid, mida saate teha sellelt lehelt.

![Teenuste armatuurlaud](./media/storsimple-service-dashboard/HCS_ServiceDashboard.png)

Armatuurlaua StorSimple halduri teenuse kuvatakse järgmine teave:

- **Diagrammiala,** näete olulised mõõdikud diagrammi oma seadmete jaoks. Esmane salvestusruumi (kohalik kinnitatud ja mitmetasandilise) saate vaadata kõiki seadmeid, samuti salvestusruumi, tarbitud seadmed aja jooksul kasutanud. Diagrammi paremas ülanurgas juhtelementide abil määrata, nädala-1, 1 kuu, 3 kuud või 1 aasta ajaskaala.

- **Kasutus ülevaade** kuvatakse esmane talletamist, mis on ette valmistatud ja tarbitud kõigis seadmetes suhtes saadaoleva salvestus, mis on saadaval kõigis seadmetes. **Provisioned** viitab talletusmahtu, mis on valmis ja nähtud kasutamiseks, kui **kasutatakse** viitab kasutamist maht on vaadatud algataja, mis on ühendatud seadmete.

- **Teatiste** ala pakub kõik teatised aktiivsete hetktõmmise kõigist seadmetest, teatiste raskusaste Rühmitusalus. Raskusaste tase klõpsates avatakse veebirakendust nende teatiste kuvamiseks klõpsake lehe **teatiste** . Lehel **teatised** , võite klõpsata üksikute teatise selle teatise, sh võimalikke toiminguid täiendavate üksikasjade kuvamiseks. Kui probleem on lahendatud, saate ka teatise tühjendage.

- **Töö** ala pakub tehtud töö hetktõmmise kõikides seadmetes, mis on ühendatud teenust. Seal on lingid, mille abil saate vaadata tööd, mis on hetkel pooleli, need, mida ei saanud viimase 24 tunni jooksul või need, mis on ajastatud järgmise 24 tunni jooksul.

- **Kiire ülevaade** ala leiate kasulikku teavet teenuse olek, nt ühendatud seadmete arv teenus, teenuse asukoht ja see tellimus, mis on seotud teenuse üksikasjad. On ka link Logi toimingud. Klõpsake linki loendi kõigi lõpuleviidud StorSimple halduri toimingud.

Armatuurlaualehe StorSimple halduri teenuse abil saate alustada järgmisi toiminguid:

- Saate vaadata või taastada teenuse registreerimise võti.
- Saate muuta teenuse andmete krüptovõtme.
- Saate vaadata tegevuse logid.

## <a name="view-or-regenerate-the-service-registration-key"></a>Saate vaadata või taastada teenuse registreerimise võti

Teenuse registreerimise võti kasutatakse StorSimple halduri teenusega Microsoft Azure StorSimple seadme registreerimiseks nii, et seade kuvatakse Azure klassikaline portaalis täpsemaks haldamise toimingute jaoks. Võti esimese seadmes on loodud ja ülejäänud seadmetest ühiskasutusse.

Klõpsates **Registreerimise võti** (lehekülje allosas) avab dialoogiboksi **Teenuse registreerimise võti** , kus saate praeguse teenuse registreerimise võti kopeerimine lõikelauale või taastada teenuse registreerimise võti.

Võti regenereeruvate ei mõjuta varem registreeritud seadmed: see mõjutab vaid pärast võtme uuesti on registreerunud teenuse seadmed.

Vaatamine ja teenuse registreerimise võti loomise kohta lisateabe saamiseks valige [Hangi teenuse registreerimise võti](storsimple-manage-service.md#get-the-service-registration-key).

## <a name="change-the-service-data-encryption-key"></a>Krüptovõtme teenuse andmete muutmine

Teenuse andmete krüptimise võtmed kasutatakse konfidentsiaalse kliendiandmete, nt salvestusruumi konto identimisteave, mis saadetakse teie haldur StorSimple teenusest StorSimple seadme krüptimine. Peate need võtmed regulaarselt muuta, kui teie IT organisatsioonil on võtme pööre poliitika salvestusruumi seadmetes. Võtme muutmine protsess võib olla veidi teistsugune sõltuvalt sellest, kas on ühe seadme või mitme seadmete StorSimple halduri teenust hallata.

3-sammult teenuse andmete krüptovõtme muutmine on:

1. Azure'i klassikaline portaali kasutamisel lubada seadme krüptovõtme teenuse andmete muutmiseks.
2. Windows PowerShelli kaudu StorSimple, algatada teenuse andmete krüptimise võtme muutmine.
3. Kui teil on rohkem kui üks StorSimple seade, värskendage teenuse andmete krüptovõtme muudes seadmetes.

Järgmised sammud kirjeldavad krüptovõtme teenuse andmete edastamiseks protsess.

[AZURE.INCLUDE [storsimple-change-data-encryption-key](../../includes/storsimple-change-data-encryption-key.md)]


## <a name="view-the-operations-logs"></a>Toimingute logid kuvamine

Saate vaadata toiming logid linki toiming logid **kiirpilgupaanil armatuurlaua** saadaval. See viib teid halduse teenuste lehe, kus saate filtreerida ja logid teatud StorSimple halduri teenust.

## <a name="next-steps"></a>Järgmised sammud

- Siit saate teada, [StorSimple seadme](storsimple-troubleshoot-operational-device.md)tõrkeotsing.

- Lugege lisateavet selle kohta, kuidas [hallata StorSimple seadme StorSimple halduri teenust](storsimple-manager-service-administration.md)kasutada.
