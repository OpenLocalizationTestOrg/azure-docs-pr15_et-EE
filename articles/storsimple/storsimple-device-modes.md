<properties 
   pageTitle="Muuta StorSimple seadme režiimi | Microsoft Azure'i"
   description="Kirjeldab StorSimple seadme režiimi ja selgitatakse, kuidas kasutada Windows PowerShelli StorSimple seadme režiimi muuta."
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
   ms.date="06/17/2016"
   ms.author="alkohli" />

# <a name="change-the-device-mode-on-your-storsimple-device"></a>Seadme režiimi StorSimple seadme muutmine

Selles artiklis kirjeldatakse lühidalt erinevaid viise, kus saate töötada StorSimple seadme. Seadme StorSimple võib toimida, kolme režiimi: tavaline ja hooldus taastamine. 

Pärast selle artikli lugemist te teate:

- Millist StorSimple seadme režiimid on
- Kuidas aru saada, millist režiimi StorSimple seade on
- Kuidas muuta tavaline hooldustööde režiimi ja *vastupidi*


Ülaltoodud haldamise toiminguid saab ainult Windows PowerShelli liidese StorSimple seadme kaudu.

## <a name="about-storsimple-device-modes"></a>StorSimple seadme režiimide kohta

Seadme StorSimple kasutab tavaline, hoolduse või taastamine. Iga režiimidest on põgusalt kirjeldatud allpool.

### <a name="normal-mode"></a>Tavaline režiim

See on määratletud normaalne funktsionaalseid režiim täielikult konfigureeritud StorSimple seadme. Vaikimisi seadme peaks olema tavaline režiim.

### <a name="maintenance-mode"></a>Hoolduse režiim

Vahel võib tekkida vajadus StorSimple seadme asetada hooldus režiimis. Selle režiimi võimaldab teil teha hooldust seade ja installige häiriva värskendused, nagu need, mis on seotud ketta püsivara.

Saate panna süsteemi hoolduse režiimi ainult Windows PowerShelli kaudu StorSimple. Kõik I/O taotlusi on peatatud selles režiimis. Teenuseid, näiteks vajutanud muutmälu (NVRAM) või klastrite teenus ka peatada. Kui sisestate või väljuge selles režiimis nii kontrolörid taaskäivitada. Hoolduse režiimi rakendusest kõiki teenuseid jätkub ja oleks terve. See võib kuluda mõni minut.

>[AZURE.NOTE]Õigesti toimimise seadmes on toetatud ainult **hooldus režiimis. Ei toetata, kus üks või mõlemad hulka kuuluvad selle ei tööta seadmes.**
</br>

### <a name="recovery-mode"></a>Taastamise režiim

Taastamise režiimi saab kirjeldada kui "Windowsi turvarežiimide võrgu tugi". Taastamise režiimi rakendub Microsoft Support meeskonnatöö ja võimaldab sooritada diagnostika süsteem. Taastamise režiimi peamine eesmärk on tuua süsteemi logid.

Kui teie süsteemi läheb taastamise režiimi, peate pöörduma Microsoft Support järgmised toimingud. Minge lisateabe saamiseks [Pöörduge Microsofti tootetoe poole](storsimple-contact-microsoft-support.md).

>[AZURE.NOTE] **Kasutatav seade ei saa paigutamiseks taastamise režiimi. Kui seade pole töökorras, taastamise režiimi proovib saada seadme, kus saate seda uurida Microsoft Support töötajatele olekusse.**

## <a name="determine-storsimple-device-mode"></a>Määratleda StorSimple seadme režiim

#### <a name="to-determine-the-current-device-mode"></a>Praeguse seadme režiimi määratlemiseks

1. Logige sisse seadme järjestikune konsooli [kasutamine](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console)Putty seadme järjestikune konsooli ühenduse juhiste järgi.

2. Vaadake plakat sõnumi järjestikune konsooli menüüs seade. See teade näitab konkreetselt, kas seade on hooldustööd või taastamine. Kui sõnum ei sisalda seotud süsteemi režiimi kindla teabe, seade on tavaline režiimis.

## <a name="change-the-storsimple-device-mode"></a>StorSimple seadme viisi muutmine 

Saate lisada StorSimple seadme hooldustööde režiimi (tavaline režiim): täita hooldust või hooldustöid režiimi värskenduste installimine. Teha järgmisi toiminguid sisestada või sellest väljumine hooldus režiimis.

> [AZURE.IMPORTANT] Enne sisestamine hooldus režiimis, veenduge, et mõlema seadme kontrollerid on terve kasutades **Riistvara oleku** lehe **hooldustööd** Azure klassikaline portaalis. Kui üks või mõlemad kontrollerid ei ole terve, pöörduge Microsoft Support järgmiste juhiste juurde. Minge lisateabe saamiseks [Pöörduge Microsofti tootetoe poole](storsimple-contact-microsoft-support.md).

#### <a name="to-enter-maintenance-mode"></a>Kui soovite hooldus režiimis

1. Logige sisse seadme järjestikune konsooli [kasutamine](storsimple-deployment-walkthrough.md#use-putty-to-connect-to-the-device-serial-console)Putty seadme järjestikune konsooli ühenduse juhiste järgi.

2. Järjestikune konsooli menüüs valik, 1, **logige sisse täielik juurdepääs**. Küsimise korral sisestage **seadme administraatori parooli**. Vaikimisi parool on: `Password1`.

3. Tippige käsuviibale, 

    `Enter-HcsMaintenanceMode`

4. Kuvatakse hoiatusteade, mis ütleb, et hooldus režiimis kuvatakse kõik I/O taotlusi häirida ja katkestab ühenduse Azure'i klassikaline portaali ja teil palutakse kinnitust. Tippige **Y** hooldus režiimis.

5. Nii kontrollerid uuesti. Kui taaskäivitamine on lõpule jõudnud, kuvatakse teise sõnumi, mis näitab, et seade on hooldustööde režiimis.


#### <a name="to-exit-maintenance-mode"></a>Väljumiseks hooldus režiimis

1. Logige sisse seadme järjestikune konsooli. Kontrollige kaudu banner teade, et teie seade on hooldus režiimis.

2. Käsuviip, tippige:

    `Exit-HcsMaintenanceMode`

3. Kuvatakse hoiatusteade ja kinnitusteade. Tippige **Y** väljumiseks hooldus režiimis.

4. Mõlemad kontrollerid uuesti. Kui taaskäivitamine on lõpule jõudnud, kuvatakse teise sõnumi, mis näitab, et seade on tavaline režiimis.


## <a name="next-steps"></a>Järgmised sammud

Siit saate teada, [tavaline ja hooldus režiimi värskenduste rakendamine](storsimple-update-device.md) StorSimple seadme kohta.

