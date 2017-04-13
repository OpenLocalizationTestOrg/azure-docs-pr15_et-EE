<properties
 pageTitle="Alustamine kommunikatsioon 3 | Microsoft Azure'i"
 description="Alustamine vaarikas pii 3, luua oma Azure'i asjade jaoturi ja ühenduse asjade jaoturi oma Pi"
 services="iot-hub"
 documentationCenter=""
 authors="shizn"
 manager="timlt"
 tags=""
 keywords=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/21/2016"
 ms.author="xshi"/>

# <a name="get-started-with-raspberry-pi-3"></a>Alustamine kommunikatsioon 3

Selles õpetuses alustamist töötamine Vaarika Pi 3 see töötab Raspbian põhitõdesid õppida. Saate siis teada, kuidas sujuvalt ühendada seadmetes [Azure'i asjade jaoturi](iot-hub-what-is-iot-hub.md)pilve. Windows 10 asjade Core näidised, külastage [windowsondevices.com](http://www.windowsondevices.com/).

## <a name="lesson-1-configure-your-device"></a>1 tund: Seadme konfigureerida

![Õppetüki 1 E2E skeem](media/iot-hub-raspberry-pi-lessons/e2e-lesson1.png)

Selle õppetüki saate konfigureerida Vaarika Pi 3 seadme operatsioonisüsteemi, häälestada oma arenduskeskkond ja funktsiooni Pi rakenduse juurutamine.

### <a name="configure-your-device"></a>Seadme konfigureerimine

Konfigureerimine oma Vaarika Pi 3 esimest korda kasutamiseks alla ja installige Raspbian OS, tasuta operatsioonisüsteem, mis on optimeeritud Vaarika Pi riistvara.

*Eeldatav aega: 30 minutiga* 

[Avage seadme konfigureerida](iot-hub-raspberry-pi-kit-node-lesson1-configure-your-device.md)

### <a name="get-the-tools"></a>Tööriistad
Tööriistad ja tarkvara luua ja juurutada Vaarika Pi 3 esimese rakenduse alla laadida.

*Eeldatav aega: 20 minutit* 

[Mine "Saada tööriistade"](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-win32.md)

### <a name="create-and-deploy-the-blink-application"></a>Luua ja juurutada vilkuv rakendus

Valimi Node.js rakenduse Github klooni ja suutäis juurutada see rakendus oma Vaarika Pi 3 tahvlile. Selle rakenduse valimi vilgub LED ühendatud tahvli iga kaks sekundit.

*Eeldatav aega: 5 minutit.* 

[Valige "loomine ja vilkuv rakenduse juurutamine"](iot-hub-raspberry-pi-kit-node-lesson1-deploy-blink-app.md)

## <a name="lesson-2-create-your-iot-hub"></a>Tund 2: Looge oma asjade jaoturi

![Tund 2 E2E skeem](media/iot-hub-raspberry-pi-lessons/e2e-lesson2.png)

Klõpsake selle õppetüki tasuta Azure'i konto loomine, ettevalmistamise oma Azure'i asjade jaoturi ja Azure asjade keskuses esimese seadme loomine.

Enne alustamist selle õppetüki, täitke 1 tund.

### <a name="get-the-azure-tools"></a>Azure'i tööriistad

Installige Azure'i käsurea liides (Azure'i CLI).

*Eeldatav aega: 10 minuti* 

["Saan Azure'i Tööriistad"](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-win32.md)

### <a name="create-your-iot-hub-and-register-your-raspberry-pi-3"></a>Luua oma asjade jaoturi ja registreerida oma Vaarika Pi 3

Luua oma ressursirühm, ettevalmistamise oma esimese Azure'i asjade jaoturi ja lisada esimese seadme Azure'i asjade jaoturi abil Azure'i CLI. 

*Eeldatav aega: 10 minuti* 

[Minge "Oma asjade keskuse loomine ja registreerida oma Vaarika Pi 3"](iot-hub-raspberry-pi-kit-node-lesson2-prepare-azure-iot-hub.md)


## <a name="lesson-3-send-device-to-cloud-messages"></a>Tund 3: Seadme pilve sõnumite saatmine

![Õppetüki 3 E2E skeem](media/iot-hub-raspberry-pi-lessons/e2e-lesson3.png)

Klõpsake selle õppetüki saadate sõnumeid oma Pi oma asjade jaoturi. Saate luua ka rakenduse Azure funktsioon, mis jätkab sissetulevate sõnumite oma asjade keskuse kaudu ja kirjutab need Azure'i tabelimälu.

Enne alustamist selle õppetüki täitke õppetükkide 1 ja 2.

### <a name="create-an-azure-function-app-and-azure-storage-account"></a>Azure'i funktsioon rakendus ja Azure Storage konto loomine

Azure'i ressursihaldur malli kasutamiseks Azure funktsioon rakendus ja Azure Storage konto loomiseks.

*Eeldatav aega: 10 minuti* 

[Minge "Azure funktsioon rakendus ja Azure Storage konto loomine"](iot-hub-raspberry-pi-kit-node-lesson3-deploy-resource-manager-template.md)

### <a name="run-sample-application-to-send-device-to-cloud-messages"></a>Valimi rakenduse seadme pilve sõnumite saatmiseks

Juurutada ja seadmesse vaarikas pii 3, mis saadab sõnumite asjade jaoturi valimi rakendus.

*Eeldatav aega: 10 minuti* 

[Mine "Valimi rakenduse käivitamiseks saatmine seadmesse pilve"](iot-hub-raspberry-pi-kit-node-lesson3-run-azure-blink.md)

### <a name="read-messages-persisted-in-azure-storage"></a>Sõnumite lugemine säilis Azure Storage
Jälgida seadme pilve sõnumite nad on kirjutanud Azure'i salvestusruumi.

*Eeldatav aega: 5 minutit.* 

[Minge "sõnumid loetuks säilis Azure Storage"](iot-hub-raspberry-pi-kit-node-lesson3-read-table-storage.md)


## <a name="lesson-4-send-cloud-to-device-messages"></a>Õppetüki 4: Pilveteenuste-seadme sõnumite saatmine

![Õppetüki 4 E2E skeem](media/iot-hub-raspberry-pi-lessons/e2e-lesson4.png)

Selle õppetüki demos Kuidas saata sõnumeid oma Azure'i asjade keskuse kaudu oma Vaarika Pi 3. Sõnumite kontrolli sisse- või väljalülitamine LED, mis on ühendatud teie pii toimimist. Valimi rakendus on valmis, saate selle ülesande saavutamiseks.

Täitke õppetükkide 1, 2 ja 3 enne alustamist selle õppetüki.

### <a name="run-the-sample-application-to-receive-cloud-to-device-messages"></a>Käivitage rakendus valimi sõnumeid cloud-seade

Õppetüki 4 valimi rakendus töötab teie pii ja jälgib sissetulevaid sõnumeid oma asjade keskuse kaudu. Uue tööülesande suutäis saadab sõnumite oma Pi oma asjade jaoturis vilgub LED.

*Eeldatav aega: 10 minuti* 

[Minge "Pilve-seadme sõnumeid valimi rakenduse käivitada"](iot-hub-raspberry-pi-kit-node-lesson4-send-cloud-to-device-messages.md)

### <a name="optional-section-change-the-on-and-off-behavior-of-the-led"></a>Valikuline jaotis: sisse- või väljalülitamine käitumine LED muutmine

Kohandada sõnumite muutmiseks LED's ja käitumine välja lülitada.

*Eeldatav aega: 10 minuti* 

[Valige "fakultatiivne jaotis: sisse- või väljalülitamine käitumine LED muutmine"](iot-hub-raspberry-pi-kit-node-lesson4-change-led-behavior.md)


## <a name="troubleshooting"></a>Tõrkeotsing

Kui vastate probleemid, mis tahes ajal tunnid, võite otsida lahendusi sellel lehel.

[Minge 'tõrkeotsing](iot-hub-raspberry-pi-kit-node-troubleshooting.md)
