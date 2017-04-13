<properties 
   pageTitle="Juurutamine StorSimple halduri teenus StorSimple virtuaalse massiiv | Microsoft Azure'i"
   description="Selgitab, kuidas luua ja kustutada StorSimple halduri teenuse Azure klassikaline portaali ja kirjeldatakse, kuidas hallata teenuse registreerimise võti."
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
   ms.date="05/19/2016"
   ms.author="alkohli" />

# <a name="deploy-the-storsimple-manager-service-for-storsimple-virtual-array"></a>StorSimple halduri teenuse StorSimple virtuaalse array juurutamine

## <a name="overview"></a>Ülevaade

StorSimple halduri teenuse Microsoft Azure'i töötab ja ühendab mitu StorSimple seadmete. Pärast teenuse loomist saate hallata seadmete Microsoft Azure'i klassikaline portaalis töötavad brauseris. See võimaldab teil jälgida kõiki seadmeid, mis on seotud StorSimple halduri teenuse keskse asukoha, vähendades haldus koormust.

StorSimple halduri sihtleht loetleb kõik StorSimple halduri teenuste abil saate hallata oma StorSimple salvestusruumi seadmed. Iga StorSimple halduri teenuse esitatakse StorSimple halduri lehel järgmine teave:

- **Nimi** – nimi, mis on määratud StorSimple halduri teenust, kui see on loodud. Pärast teenuse loomist ei saa muuta teenuse nimi.

- **Olek** – teenus, mis võib olla **aktiivne**, **loomine**või **Online**olekut.

- **Asukoht** – StorSimple seadme juurutatakse geograafiline asukoht.

- **Tellimuse** – arvelduse tellimus, mis on seotud teenust.

Levinud toimingud, mida saab teha StorSimple halduri lehe kaudu on:

- Teenuse loomine
- Teenuse kustutamine
- Saada teenuse registreerimise võti
- Taastada teenuse registreerimise võti

Selles õppetükis kirjeldatakse, kuidas neid toiminguid teha. Selles artiklis sisalduv teave kehtib ainult StorSimple virtuaalse massiivi. StorSimple 8000 sarja kohta lisateabe saamiseks minge [StorSimple halduri teenuse juurutamine](storsimple-manage-service.md).

## <a name="create-a-service"></a>Teenuse loomine

**Kiire loomine** suvandi abil saate luua StorSimple halduri teenuse, kui soovite juurutada StorSimple seadme. Teenuse loomiseks peate olema:

- Lepingu Enterprise tellimusele
- Microsoft Azure active salvestusruumi konto
- Arveldusteabe, mida kasutatakse juurdepääsu juhtimine

Saate valida ka teenuse loomisel vaikimisi salvestusruumi konto loomiseks.

Ühe teenuse haldamiseks mitmes seadmes. Siiski ei saa seadme ühendada mitu teenused. Suurettevõte võib olla mitu teenuse eksemplari töötamiseks erinevate tellimused, ettevõtted või isegi juurutamise asukohad.  

> [AZURE.NOTE] Teil on vaja eraldi eksemplarid StorSimple halduri teenuse haldamiseks StorSimple 8000 seeria seadmed ja StorSimple virtuaalse massiivi.

Järgmiste toimingute teenuse.

[AZURE.INCLUDE [storsimple-ova-create-new-service](../../includes/storsimple-ova-create-new-service.md)]

## <a name="delete-a-service"></a>Teenuse kustutamine

Enne kustutamist teenus, veenduge, et ühtegi ühendatud seadet kasutavad seda. Kui teenus on kasutusel, inaktiveerige ühendatud seadmete. Desaktiveeri selle toimingu seade ja teenuse vahelise ühenduse katkestama, kuid säilitada seadme andmed pilveteenuses. 

> [AZURE.IMPORTANT] Kui teenus on kustutatud, toimingut ei saa tühistada. 

Järgmiste toimingute kustutamiseks teenus.

### <a name="to-delete-a-service"></a>Teenuse kustutamine

1. Valige lehel **StorSimple halduri teenuse** teenus, mida soovite kustutada.

1. Klõpsake lehe allosas **kustutada** .

1. Või klõpsake kinnitusteatises nuppu **Jah** . Võib kuluda mõni minut teenuse, et kustutada.

## <a name="get-the-service-registration-key"></a>Saada teenuse registreerimise võti

Kui olete loonud teenuse, peate teenuse StorSimple seadme registreerimiseks. Esimese StorSimple seadme registreerimiseks peate teenuse registreerimise võti. Olemasoleva StorSimple teenuse täiendavad seadmed registreerida, peate nii registreerimise võti ja teenuse andmete krüptovõtme, (mis on loodud esimese seadme registreerimise käigus). Teenuse andmete krüptovõtme kohta lisateabe saamiseks lugege teemat [teenuse andmete krüptovõtme UI kohaliku veebist](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key). 

Järgmiste toimingute saada teenuse registreerimise võti.

[AZURE.INCLUDE [storsimple-ova-get-service-registration-key](../../includes/storsimple-ova-get-service-registration-key.md)]

Võtke teenuse registreerimise võti turvaline asukoht. Peate selle klahvi, kui ka teenuse andmete krüptovõtme selle teenuse täiendavad seadmed registreerida. Pärast teenuse registreerimise võti, peate seadme Windows PowerShelli kaudu StorSimple kasutajaliidese konfigureerida.

## <a name="regenerate-the-service-registration-key"></a>Taastada teenuse registreerimise võti

Peate teenuse registreerimise võti taastada, kui teil on vaja teha võtme pööre või teenuse administraatorite loend on muutunud. Kui te taastada võti, kasutatakse ainult registreerusite edaspidised seadmete uue tootenumbri. Juba registreeritud seadmete ei mõjuta see protsess.

Järgmiste toimingute teenuse registreerimise võti uuesti looma.

### <a name="to-regenerate-the-service-registration-key"></a>Kui soovite taastada teenuse registreerimise võti

1. Klõpsake lehel **StorSimple halduri teenuse** **Registreerimise võti**.

1. Klõpsake dialoogiboksis **Teenuse registreerimise võti** **taastada**.

1. Kuvatakse kinnitusteade. Klõpsake nuppu **OK** , et jätkata regenereerimise.

1. Kuvatakse uus teenus registreerimise võti.

1. Kopeerige see võti ja salvestage see registreerimise mis tahes uue seadmed see teenus.

1. Klõpsake ikooni Otsi ![Märkige ruut ikoon](./media/storsimple-ova-manage-service/image7.png) selle dialoogiboksi sulgemiseks.


## <a name="next-steps"></a>Järgmised sammud

- Siit saate teada, kuidas [alustada](storsimple-ova-deploy1-portal-prep.md) StorSimple virtuaalse massiiv.
    
- Siit saate teada, kuidas [hallata StorSimple seadme](storsimple-ova-web-ui-admin.md).

 
