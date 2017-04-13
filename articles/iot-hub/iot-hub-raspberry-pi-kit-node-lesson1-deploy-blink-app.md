<properties
 pageTitle="Luua ja juurutada vilkuv rakendus | Microsoft Azure'i"
 description="Valimi Node.js rakenduse Github klooni ja suutäis juurutada see rakendus oma Vaarika Pi 3 tahvlile. Selle rakenduse valimi vilgub LED ühendatud tahvli iga kaks sekundit."
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

# <a name="13-create-and-deploy-the-blink-application"></a>1.3 loomine ja vilkuv rakenduse juurutamine

## <a name="131-what-you-will-do"></a>1.3.1, mida te teete

Valimi Node.js rakenduse Github klooni ja valimi rakenduse oma Vaarika Pi 3 suutäis tööriista abil. Proovi taotluse vilgub LED ühendatud tahvli iga kaks sekundit. Kui vastate probleeme, otsida lahendusi [lehe tõrkeotsing](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="132-what-you-will-learn"></a>1.3.2, mida saate

- Kuidas kasutada soovitud `device-discover-cli` tööriista toomiseks võrguteabe kohta oma pii.
- Kuidas juurutada ja käivitage rakendus valimi oma pii.
- Kuidas juurutada ja silumine töötavad kaugühenduse teel oma Pi rakendused.

## <a name="133-what-you-need"></a>1.3.3 vajalik

Peate on lõpule viidud jälgi jaotiste õppetüki 1:

- [Seadme konfigureerimine](iot-hub-raspberry-pi-kit-node-lesson1-configure-your-device.md)
- [Tööriistad](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-win32.md)

## <a name="134-obtain-the-ip-address-and-host-name-of-your-pi"></a>1.3.4 saada IP aadress ja hosti nimi oma pii

Avage käsuviip Windowsi või macOS või Ubuntu terminal aknas ja seejärel käivitage järgmine käsk:

```bash
devdisco list --eth
```

Peaksite nägema ja väljund on järgmine:

![avastamine](media/iot-hub-raspberry-pi-lessons/lesson1/device_discovery.png)

Vaadake üle soovitud `IP address` ja `hostname` oma pii. Peate selle teabe allpool.

> [AZURE.NOTE] Veenduge, et teie pii on ühendatud arvuti samasse võrku. Näiteks kui teie arvuti on ühendatud traadita võrku oma pii on ühendatud juhtmega ühendatud võrku, te ei näe devdisco väljund IP-aadress.

## <a name="135-clone-the-sample-application"></a>1.3.5 klooni valimi rakendus

Proovi kood avamiseks tehke järgmist.

1. Klooni valimi hoidla kaudu Github, käivitage järgmine käsk:

    ```bash
    git clone https://github.com/Azure-Samples/iot-hub-node-raspberrypi-getting-started.git
    ```

2. Visual Studio kood valimi rakenduse avamiseks järgmised käsud:

    ```bash
    cd iot-hub-node-raspberrypi-getting-started
    cd Lesson1
    code .
    ```

![Repo struktuur](media/iot-hub-raspberry-pi-lessons/lesson1/vscode-blink-mac.png)

Funktsiooni `app.js` faili selle `app` alamkausta on klahv allika fail, mis sisaldab koodi juhtelemendile LED.

### <a name="136-install-application-dependencies"></a>1.3.6 installida rakendus sõltuvused

Installige teekide ja muude moodulid peate valimi rakenduse, käivitage järgmine käsk:

```bash
npm install
```

## <a name="137-configure-the-device-connection"></a>1.3.7 konfigureeriks seade

Seadme konfigureeriks, toimige järgmiselt.

1. Seadme konfiguratsiooni faili luua, käivitage järgmine käsk:

    ```bash
    gulp init
    ```

    Konfiguratsioonifail `config-raspberrypi.json` sisaldab kasutajatunnust, logige sisse oma Pi abil. Kasutajatunnust tegemist vältimiseks konfiguratsiooni fail on loodud alamkausta `.iot-hub-getting-started` home kausta teie arvutis.

2. Visual Studio kood seadme konfiguratsiooni faili avamiseks käivitage järgmine käsk:

    ```bash
    # For Windows command prompt
    code %USERPROFILE%\.iot-hub-getting-started\config-raspberrypi.json

    # For macOS or Ubuntu
    code ~/.iot-hub-getting-started/config-raspberrypi.json
    ```

3. Kohatäite asendamiseks `[device hostname or IP address]` IP-aadress või hosti nimi, mis kuvatakse jaotises 1.3.4.

    ![Config.JSON](media/iot-hub-raspberry-pi-lessons/lesson1/vscode-config-mac.png)

Palju õnne! Teie loodud edukalt esimene valimi taotlus oma pii.

## <a name="138-deploy-and-run-the-sample-application"></a>1.3.8 juurutada ja käivitage rakendus näidis

### <a name="1381-install-nodejs-and-npm-on-your-pi"></a>1.3.8.1 Node.js ja installida NPM oma Pi

Node.js ja installida NPM oma Pi, käivitage järgmine käsk:

```bash
gulp install-tools
```

Kümme minutit esimest korda, kui käivitate selle toimingu lõpuleviimiseks võib kuluda.

### <a name="1382-deploy-and-run-the-sample-app"></a>1.3.8.2 juurutada ja käivitage rakendus näidis

Juurutada ja käivitage rakendus valimi, käivitage järgmine käsk:

```bash
gulp deploy && gulp run
```

### <a name="1383-verify-the-app-works"></a>1.3.8.3 kinnitamine rakendus töötab

Nüüd peaks nähtaval LED oma piiga välkuvad iga kaks sekundit.  Kui te ei näe LED vilgub, vt [tõrkeotsingujuhendi](iot-hub-raspberry-pi-kit-node-troubleshooting.md) levinud probleemidele lahendusi.
![LED vilgub](media/iot-hub-raspberry-pi-lessons/lesson1/led_blinking.jpg)

> [AZURE.NOTE] Kasutage `Ctrl + C` rakenduse sulgemiseks.

## <a name="139-summary"></a>1.3.9 Kokkuvõte

Nõutavad tööriistad töötamiseks oma Pi installimist ja juurutada valimi taotluse abil oma Pi vilgub LED. Te saate nüüd liikuda järgmisele õppetüki loomine, juurutada ja teise valimi rakendus, mis ühendab oma Pi Azure'i asjade keskuse kaudu sõnumite saatmiseks ja vastuvõtmiseks.

## <a name="next-steps"></a>Järgmised sammud

Nüüd olete valmis tund 2, mis algab alustada [Azure'i tööriistad](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-win32.md)
