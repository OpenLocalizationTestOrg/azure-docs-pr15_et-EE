<properties 
   pageTitle="Muuta paroole StorSimple | Microsoft Azure'i" 
   description="Kirjeldab, kuidas kasutada StorSimple halduri teenuse StorSimple hetktõmmise juhataja ja seadme administraatori parooli muuta." 
   services="storsimple" 
   documentationCenter="NA" 
   authors="alkohli" 
   manager="carmonm" 
   editor=""/>

<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD" 
   ms.date="08/17/2016"
   ms.author="alkohli"/>

# <a name="use-the-storsimple-manager-service-to-change-your-storsimple-passwords"></a>StorSimple halduri teenuse abil saate muuta paroole StorSimple

## <a name="overview"></a>Ülevaade 

Azure'i klassikaline portaali lehe **konfigureerimine** sisaldab kõiki seadme parameetreid saate konfigureerida, StorSimple seadmes, mida haldab StorSimple halduri teenuse. Selles õppeteemas selgitatakse, kuidas saate kasutada lehe **konfigureerimine** oma seadme administraator või StorSimple hetktõmmise halduri parooli muutmiseks.

## <a name="change-the-device-administrator-password"></a>Seadme administraatori parooli muutmine

Windows PowerShelli kasutajaliidese kasutamisel StorSimple seadme juurdepääsu teil palutakse sisestada seadme administraatori parooli. Kui esimene StorSimple seade on registreeritud teenus, on vaikimisi parool selle kasutajaliidese *parool1*. Andmete turvalisuse huvides on nõutav lõpus registreerimise käigus selle parooli muuta. See parool muutmata ei saa Väljumise registreerimise käigus. Lisateavet leiate teemast [Samm 3: konfigureerimine ja registreerida seadme Windows PowerShelli kaudu StorSimple](storsimple-deployment-walkthrough-u2.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple).

Azure'i klassikaline portaali kaudu saab muuta seejärel parool, mis loodi esmalt Windows PowerShelli kasutajaliidese registreerimise käigus. Järgmiste toimingute seadme administraatori parooli muuta.

#### <a name="to-change-the-device-administrator-password"></a>Seadme administraatori parooli muutmine

1. Klassikaline portaalis **seadmete**nuppu > seadme**konfigureerimine** .

2. Liikuge kerides jaotiseni **Seadme administraatori parooli** . Sisestage administraatori parool, mis sisaldab 8-15 märki. Parooli peab olema 3 või enama suurtähtedeks, väiketähtedeks, arvulised ja teisiti märkide kombinatsioon.

3. Kinnitage parool.

4. Klõpsake lehe allosas **salvestada** .

Nüüd peaks uuendada seadme administraatori parooli. Saate juurdepääsu Windows PowerShelli liidese see muudetud parool.

## <a name="change-the-storsimple-snapshot-manager-password"></a>StorSimple hetktõmmise halduri parooli muutmine

StorSimple hetktõmmise Manager tarkvara asub Windows Server ja saavad administraatorid varukoopiaid kohalik kujul StorSimple seadme haldamiseks ja Pilveteenusest hetktõmmiseid luua.

Seadme konfigureerimisel StorSimple hetktõmmise halduris, palutakse teil seadme IP-aadress ja parool oma mäluseadmesse autentida. See parool on konfigureeritud esmalt Windows PowerShelli kasutajaliidese kaudu. Lisateavet leiate teemast [Samm 3: konfigureerimine ja registreerida seadme Windows PowerShelli kaudu StorSimple](storsimple-deployment-walkthrough-u2.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple).

Klassikaline portaali kaudu saab muuta seejärel parool, mis loodi esmalt Windows PowerShelli kasutajaliidese registreerimise käigus. Järgmiste toimingute StorSimple hetktõmmise halduri parooli muuta.

#### <a name="to-change-the-storsimple-snapshot-manager-password"></a>StorSimple hetktõmmise halduri parooli muutmine

1. Klassikaline portaalis **seadmete**nuppu > seadme**konfigureerimine** .

2. Liikuge kerides jaotiseni **StorSimple hetktõmmise haldur** . Sisestage parool, mis on 14 või 15 märki. Veenduge, et parool sisaldab 3 või enama suurtähtedeks, väiketähtedeks, arvulised ja teisiti märkide kombinatsioon.

3. Kinnitage parool.

4. Klõpsake lehe allosas **salvestada** .

Nüüd peaks StorSimple hetktõmmise halduri parooli uuendada.
 

## <a name="next-steps"></a>Järgmised sammud

- Lugege lisateavet [StorSimple turvalisus](storsimple-security.md).

- Lugege lisateavet [oma seadme konfiguratsiooni](storsimple-modify-device-config.md).

- Lisateavet [hübriidjuurutuses StorSimple seadme StorSimple halduri teenuse kasutamise](storsimple-manager-service-administration.md)kohta.
