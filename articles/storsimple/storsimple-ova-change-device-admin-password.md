<properties 
   pageTitle="StorSimple virtuaalse seadme administraatori parooli muutmine | Microsoft Azure'i"
   description="Kirjeldab, kuidas kasutada Azure klassikaline portaali või StorSimple virtuaalse massiiv web UI seadme administraatori parooli muuta."
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
   ms.date="06/17/2016"
   ms.author="alkohli" />

# <a name="change-the-storsimple-virtual-array-device-administrator-password"></a>StorSimple virtuaalse massiiv seadme administraatori parooli muutmine

## <a name="overview"></a>Ülevaade

Windows PowerShelli kasutajaliidese kasutamisel StorSimple virtuaalse seadme juurdepääsuks on nõutav, seadme administraatori parooli. Vaikimisi parool on kui StorSimple seadme esmalt on ette valmistatud ja alustada, *parool1*. Teie andmete turvalisuse huvides vaikimisi parool aegub esmakordsel sisselogimisel ja teil palutakse seda parooli muutmine.

Saate muuta igal ajal pärast seadet juurutamist tootmiskeskkonnas seadme administraatori parooli ka kohaliku web UI või Azure klassikaline portaali. Kõiki neid toiminguid kirjeldatakse käesolevas artiklis.

## <a name="use-the-azure-classic-portal-to-change-the-password"></a>Azure'i klassikaline portaali abil parooli muutmine

Järgmiste toimingute Azure klassikaline portaali kaudu seadme administraatori parooli muuta.

#### <a name="to-change-the-device-administrator-password-via-the-azure-classic-portal"></a>Seadme administraatori parooli Azure klassikaline portaali kaudu

1. Portaalis **seadmete**nuppu > seadme**konfigureerimine** .

2. Liikuge kerides jaotiseni **Seadme administraatori parooli** . Sisestage administraatori parool, mis sisaldab 8-15 märki. Parooli peab olema suurtähtedeks, väiketähtedeks, arvulised ja teisiti märkide kombinatsioon.

3. Kinnitage parool.

4. Klõpsake lehe allosas **salvestada** .

Nüüd peaks uuendada seadme administraatori parooli. Saate selle muudetud parooli juurdepääsu kohalikult seade.

## <a name="use-the-storsimple-virtual-array-web-ui-to-change-the-password"></a>Kasutage StorSimple virtuaalse massiiv web UI parooli muutmine

Järgmiste toimingute kaudu kohaliku kasutajaliides web seadme administraatori parooli muuta.

#### <a name="to-change-the-device-administrator-password-via-the-local-web-ui"></a>Seadme administraatori parooli UI kohaliku veebi kaudu

1. Klõpsake kohaliku web UI **hooldustööd** > seadme**parooli muutmine** .

    ![parool1 muutmine](./media/storsimple-ova-change-device-admin-password/image40.png)

2. Sisestage **praegune parool**.

3. Sisestage **Uus parool**. Parool peab olema vähemalt 8 tähemärki. Peab sisaldama 3 / 4 järgmiselt: suurtähti, väiketähti, arvulised ja teisiti.

    Pange tähele, et teie parool ei saa sama viimased 24 paroolid.

3. Sisestage parool selle kinnitamiseks uuesti.

    ![password2 muutmine](./media/storsimple-ova-change-device-admin-password/image41.png)

4. Klõpsake lehe allosas nuppu **Rakenda**. Uus parool, seejärel rakendatakse. Kui parooli muutmine õnnestub, kuvatakse järgmine tõrketeade.

    ![parooli tõrge](./media/storsimple-ova-change-device-admin-password/image42.png)

    Pärast seda, kui parool on värskendatud, teavitatakse teid. Seejärel saate seda muudetud parooli seadme kohalikult juurdepääsu.

## <a name="next-steps"></a>Järgmised sammud

Lisateave [teie StorSimple virtuaalse massiiv haldamise](storsimple-ova-web-ui-admin.md)kohta.
