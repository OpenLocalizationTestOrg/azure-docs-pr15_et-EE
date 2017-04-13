<properties 
   pageTitle="Asendamine raami StorSimple seadmes | Microsoft Azure'i"
   description="Kirjeldab, kuidas eemaldada ja asendada oma StorSimple esmane ruum või EBOD ruum jaoks."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="replace-the-chassis-on-your-storsimple-device"></a>Asendage raami StorSimple seadmes

## <a name="overview"></a>Ülevaade

Selle õpetuse selgitab, kuidas eemaldada ja asendada vastava raami StorSimple 8000 sarja seadmes. StorSimple 8100 mudel on ühe ruumi seadme (ühe raami), on 8600 on kaks ruumi seadme (kaks raami). 8600 mudeli, on potentsiaalselt kahe raami, mis võib nurjuda seadmesse: jaoks esmane ruum või EBOD ruum jaoks.

Mõlemal juhul asendamine raami, mis saadetakse Microsoft on tühi. Pole Power ja jahutus moodulid (PCMs) selle domeenikontrolleri moodulid, ühtlase olekus kettadraivide (SSD), kõvaketta draivid (HDD-d) või EBOD moodulid kaasatakse.

>[AZURE.IMPORTANT] Enne eemaldada ja asendada raami, vaadake turvalisuse [StorSimple riistvara komponent asendamine](storsimple-hardware-component-replacement.md).

## <a name="remove-the-chassis"></a>Eemaldage raam

Järgmiste toimingute raami StorSimple seadme eemaldada.

#### <a name="to-remove-a-chassis"></a>Vastava raami eemaldamine

1. Veenduge, et StorSimple seade on sulgeda ja lahti power allikad.

2. Eemaldage võrgu- ja SAS kaabel, kui on kohaldatav.

3. Üksuse eemaldamiseks raami.

4. Eemaldage iga draivid ja pange tähele, kust need eemaldatakse teenindusajad. Lisateavet leiate teemast [eemaldamine ketas, kuhu](storsimple-disk-drive-replacement.md#remove-the-disk-drive).

5. EBOD ruum (kui see on nurjunud raami), klõpsake EBOD kontrolleril moodulid eemaldada. Lisateabe saamiseks vt [mõne EBOD kontrolleril eemaldada](storsimple-ebod-controller-replacement.md#remove-an-ebod-controller). 

    Esmane ruum (kui see on nurjunud raami), klõpsake eemaldada kontrolöride ja pange tähele, kust need eemaldatakse teenindusajad. Lisateabe saamiseks vt [on selle domeenikontrolleri eemaldada](storsimple-controller-replacement.md#remove-a-controller).

## <a name="install-the-chassis"></a>Raami installimine

Järgmiste toimingute installimiseks raami StorSimple seadmes.

#### <a name="to-install-a-chassis"></a>Installimine on raami

1. Ühendage raami raami sisse. Lisateavet leiate teemast [sektsioon-mount StorSimple 8100 seadme](storsimple-8100-hardware-installation.md#rack-mount-your-storsimple-8100-device) või [sektsioon-kinnituse StorSimple 8600 seadme](storsimple-8600-hardware-installation.md#rack-mount-your-storsimple-8600-device).

2. Pärast raami on ühendatud raam, installige need olid varem installitud samasse positsioonidel kontrolleril moodulid.

3. Installige draivid sama positsioonide ja teenindusajad, et need olid varem installitud.

    >[AZURE.NOTE] Soovitame selle funktsiooni SSD esmalt installima ja seejärel installige soovitud HDD-d.

2. Seade paigaldatud raam ja installitud komponendid, ühendage seade vastav power allikad ja lülitage seade. Lisateavet leiate teemast [seadme StorSimple 8100](storsimple-8100-hardware-installation.md#cable-your-storsimple-8100-device) või [kaabel seadme StorSimple 8600](storsimple-8600-hardware-installation.md#cable-your-storsimple-8600-device).

## <a name="next-steps"></a>Järgmised sammud

Lugege lisateavet [StorSimple riistvara komponent asendamine](storsimple-hardware-component-replacement.md).

