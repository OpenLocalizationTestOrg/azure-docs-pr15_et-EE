<properties
   pageTitle="Värskendage StorSimple seade | Microsoft Azure'i"
   description="Selgitab, kuidas kasutada funktsiooni StorSimple värskenduse installimiseks harilik ja hooldus režiimi värskendused ja käigultparandused."
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
   ms.date="06/28/2016"
   ms.author="v-sharos" />

# <a name="update-your-storsimple-8000-series-device"></a>StorSimple 8000 seeria seadme värskendamine

## <a name="overview"></a>Ülevaade

StorSimple värskenduste funktsioonid võimaldavad hõlpsalt StorSimple seadme kursis. Sõltuvalt värskenduse, saate rakendada värskenduste seadme Azure klassikaline portaali kaudu või Windows PowerShelli kasutajaliidese kaudu. Selles õppetükis kirjeldatakse värskenduste tüübid ja kuidas installida üks neist.

Saate rakendada kahte tüüpi seade värskendused: 

- Tavaline (või tavaline režiim) värskendused
- Hoolduse režiimi värskendused

Saate installida Windows PowerShelli; või Azure klassikaline portaali kaudu korrapäraselt Siiski peab hoolduse režiimi värskenduste installimine Windows PowerShelli abil. 

Iga värskenduse tüüp on eraldi allpool kirjeldatud.

### <a name="regular-updates"></a>Tavaline värskendused

Tavaline värskendused on-häiriva värskendused, mida saab installida, kui seade on tavaline režiimis. Järgmised värskendused on rakendatud iga seadme kontrolleril Microsoft Update'i veebisaidi kaudu. 

> [AZURE.IMPORTANT] Selle domeenikontrolleri Tõrkesiirde võib ilmneda värskendamise käigus. Kuid see ei mõjuta süsteemi-saadavus või toiming.

- Tavaline Värskenduste Azure klassikaline portaali kaudu installimise kohta leiate teemast [installi korrapäraselt Azure klassikaline portal(#install-regular-updates-via-the-azure-classic-portal) kaudu.

- Tavaline värskendusi Windows PowerShelli kaudu saate installida ka StorSimple. Lisateavet leiate teemast [Windows PowerShelli StorSimple kaudu regulaarselt värskenduste installimine](#install-regular-updates-via-windows-powershell-for-storsimple).

### <a name="maintenance-mode-updates"></a>Hoolduse režiimi värskendused

Hoolduse režiimi värskendused on häiriva värskendused nagu ketta püsivara uuendamine. Värskendused nõuavad seadme kasutusele võtta hooldus režiimis. Lisateavet leiate teemast [Samm 2: sisestage hooldus režiimis](#step2). Hoolduse režiimi värskenduste installimine ei saa kasutada Azure klassikaline portaali. Selle asemel, peate kasutama Windows PowerShelli StorSimple. 

Hoolduse režiimi värskenduste installimise kohta leiate teemast [hooldustööde installida režiimi värskenduste StorSimple Windows PowerShelli kaudu](#install-maintenance-mode-updates-via-windows-powershell-for-storsimple).

> [AZURE.IMPORTANT] Hoolduse režiimi värskenduste tuleb eraldi iga domeenikontrolleri. 

## <a name="install-regular-updates-via-the-azure-classic-portal"></a>Azure'i klassikaline portaali kaudu regulaarselt värskenduste installimine

Azure'i klassikaline portaali abil saate StorSimple seadme värskenduste rakendamine.

[AZURE.INCLUDE [storsimple-install-updates-manually](../../includes/storsimple-install-updates-manually.md)]

## <a name="install-regular-updates-via-windows-powershell-for-storsimple"></a>Tavaline värskendused Windows PowerShelli kaudu StorSimple

Teise võimalusena saate Windows PowerShelli StorSimple harilik (tavaline režiim) värskendused.

> [AZURE.IMPORTANT] Kuigi saate installida Windows PowerShelli kaudu StorSimple korrapäraselt, Soovitame installida Azure klassikaline portaali kaudu korrapäraselt. Alates Update 1, tehakse enne kontrolli enne värskenduste installimine portaalist. Need enne kontrolli ennetada tõrkeid ja tagada sujuvam kogemus. 

[AZURE.INCLUDE [storsimple-install-regular-updates-powershell](../../includes/storsimple-install-regular-updates-powershell.md)]

## <a name="install-maintenance-mode-updates-via-windows-powershell-for-storsimple"></a>Installige hooldustööd režiimi värskendusi Windows PowerShelli kaudu StorSimple

Windows PowerShelli StorSimple abil StorSimple seadme hooldus režiimis värskenduste rakendamine. Kõik I/O taotlusi on peatatud selles režiimis. Teenuseid, näiteks vajutanud muutmälu (NVRAM) või klastrite teenus ka peatada. Nii kontrollerid on rebooted, kui sisestate või väljuge selles režiimis. Selle režiimi rakendusest kõiki teenuseid jätkub ja oleks terve. (Selleks võib kuluda mõni minut.)

Kui teil on vaja hooldus režiimis värskendused, saate teatise Azure klassikaline portaali kaudu, et teil peab olema installitud värskendusi. See teade sisaldab juhiseid Windows PowerShelli StorSimple värskenduste installimiseks. Pärast värskendamist seadme, sama toimingut muutmiseks kasutada seadme tavalises režiimis. Üksikasjalikud juhised leiate teemast [Samm 4: välju hooldus režiimis](#step4).

> [AZURE.IMPORTANT] 
> 
> - Enne sisestamine hooldus režiimis, veenduge, et mõlema seadme kontrollerid on terve, märkides ruudu **Riistvara oleku** lehe **hooldustööd** Azure klassikaline portaalis. Kui selle domeenikontrolleri pole terve, pöörduge Microsoft Support järgmiste juhiste juurde. Minge lisateabe saamiseks pöörduge Microsofti tootetoe poole. 
> - Kui olete hooldus režiimis, peate rakendada värskenduse esmalt üks ja seejärel klõpsake soovitud kontrolleril.

### <a name="step-1-connect-to-the-serial-console-a-namestep1"></a>Samm 1: Ühendada järjestikune konsooli<a name="step1">

Esmalt kasutada näiteks PuTTY järjestikune konsooli. Järgnevalt selgitatakse, kuidas ühendada järjestikune konsooli PuTTY abil.

[AZURE.INCLUDE [storsimple-use-putty](../../includes/storsimple-use-putty.md)]

### <a name="step-2-enter-maintenance-mode-a-namestep2"></a>Samm 2: Sisestage hooldus režiimis<a name="step2">

Kui loote ühenduse konsooli, kindlaks, kas värskenduste installimine ja hooldus režiimis need installida.

[AZURE.INCLUDE [storsimple-enter-maintenance-mode](../../includes/storsimple-enter-maintenance-mode.md)]

### <a name="step-3-install-your-updates-a-namestep3"></a>Samm 3: Oma värskenduste installimine<a name="step3">

Järgmiseks oma värskenduste installimine.

[AZURE.INCLUDE [storsimple-install-maintenance-mode-updates](../../includes/storsimple-install-maintenance-mode-updates.md)]
 
### <a name="step-4-exit-maintenance-mode-a-namestep4"></a>Samm 4: Välju hooldus režiimis<a name="step4">

Lõpetuseks, sulgege hooldus režiimis.

[AZURE.INCLUDE [storsimple-exit-maintenance-mode](../../includes/storsimple-exit-maintenance-mode.md)]

## <a name="install-hotfixes-via-windows-powershell-for-storsimple"></a>Installige käigultparandused Windows PowerShelli kaudu StorSimple

Erinevalt Microsoft Azure'i StorSimple värskendused installitakse käigultparandused ühiskaustast. Sarnaselt värskendused on kahte tüüpi käigultparandused. 

- Tavaline käigultparandused 
- Hoolduse režiimi käigultparandused  

Järgmised toimingud selgitavad, kuidas kasutada Windows PowerShelli StorSimple regular ja hooldus režiimis draiverite installimiseks.

[AZURE.INCLUDE [storsimple-install-regular-hotfixes](../../includes/storsimple-install-regular-hotfixes.md)]

[AZURE.INCLUDE [storsimple-install-maintenance-mode-hotfixes](../../includes/storsimple-install-maintenance-mode-hotfixes.md)]

## <a name="what-happens-to-updates-if-you-perform-a-factory-reset-of-the-device"></a>Mis juhtub kui lähtestamist factory seadme värskendusi?

Kui seadme algsätete lähtestamist, lähevad kaotsi kõik värskendused. Pärast factory-reset seade on registreeritud ja konfigureeritud, peate käsitsi installida Azure klassikaline portaali ja/või Windows PowerShelli kaudu StorSimple. Lisateavet factory lähtestamine leiate teemast [algsätted seadme lähtestada](storsimple-manage-device-controller.md#reset-the-device-to-factory-default-settings).

## <a name="next-steps"></a>Järgmised sammud

- Lisateavet [Windows PowerShelli StorSimple hallata StorSimple seadme kaudu](storsimple-windows-powershell-administration.md).
- Lisateavet [hübriidjuurutuses StorSimple seadme StorSimple halduri teenuse kasutamise](storsimple-manager-service-administration.md)kohta.
