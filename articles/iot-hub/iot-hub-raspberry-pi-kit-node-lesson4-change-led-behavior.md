<properties
 pageTitle="Valikuline jaotise - muuta sisse- või väljalülitamine käitumine LED | Microsoft Azure'i"
 description="Kohandada sõnumite muutmiseks LED's ja käitumine välja lülitada."
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

# <a name="42-optional-section-change-the-on-and-off-behavior-of-the-led"></a>4.2 fakultatiivne jaotis: sisse- või väljalülitamine käitumine LED muutmine

## <a name="421-what-you-will-do"></a>4.2.1, mida te teete

Kohandada sõnumite muutmiseks LED's ja käitumine välja lülitada. Kui vastate probleeme, otsida lahendusi [lehe tõrkeotsing](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="422-what-you-will-learn"></a>4.2.2, mida saate

Täiendavad Node.js funktsioonide abil saate muuta LED's ja käitumine välja lülitada.

## <a name="423-what-you-need"></a>4.2.3 vajalik

Peate on lõpule viidud [4.1 käivitada valimi rakendus oma vaarikas piiga saada seadme sõnumitele pilve](iot-hub-raspberry-pi-kit-node-lesson4-send-cloud-to-device-messages.md).

## <a name="424-add-nodejs-functions"></a>4.2.4 lisamine Node.js funktsioonid

1. Avage rakendus valimi Visual Studio koodi käivitades järgmised käsud:

    ```bash
    cd Lesson4
    code .
    ```

2. Avage soovitud `app.js` fail ja seejärel lõppu lisatakse järgmised funktsioonid:

    ```javascript
    function turnOnLED() {
      wpi.digitalWrite(CONFIG_PIN, 1);
    }

    function turnOffLED() {
      wpi.digitalWrite(CONFIG_PIN, 0);
    }
    ```

    ![app.js värskendamine](media/iot-hub-raspberry-pi-lessons/lesson4/updated_app_js.png)

3. Lisage järgmised tingimused enne vaikimisi blokeerimise vahetamise puhul, on `receiveMessageCallback` funktsioon:

    ```javascript
    case 'on':
      turnOnLED();
      break;
    case 'off':
      turnOffLED();
      break;
    ```

    Nüüd olete konfigureerinud valimi rakenduse vastata rohkem juhiseid sõnumite kaudu. LED lülitab juhiseid "on" ja "väljas" käsustikuga lülitab LED.

4. Avage gulpfile.js fail ja seejärel lisage uus funktsioon enne funktsiooni `sendMessage`:

    ```javascript
    var buildCustomMessage = function (messageId) {
      if ((messageId & 1) && (messageId < MAX_MESSAGE_COUNT)) {
        return new Message(JSON.stringify({ command: 'on', messageId: messageId }));
      } else if (messageId < MAX_MESSAGE_COUNT) {
        return new Message(JSON.stringify({ command: 'off', messageId: messageId }));
      } else {
        return new Message(JSON.stringify({ command: 'stop', messageId: messageId }));
      }
    }
    ```

    ![gulpfile värskendamine](media/iot-hub-raspberry-pi-lessons/lesson4/updated_gulpfile.png)

5. Klõpsake soovitud `sendMessage` funktsioon, asendage rea `var message = buildMessage(sentMessageCount);` koos uue rea, mis on näidatud järgmises koodilõigu:

    ```javascript
    var message = buildCustomMessage(sentMessageCount);
    ```

6. Salvestage kõik muutused.

### <a name="425-deploy-and-run-the-sample-application"></a>4.2.5 juurutada ja käivitage rakendus näidis

Juurutada ja käivitage rakendus valimi oma Pi, käivitage järgmine käsk:

```bash
gulp
```

Peaksite nägema LED kaks sekundit all sisse lülitatud ja seejärel välja lülitatud teise kaks sekundit. Viimase "stop" sõnumi peatab valimi rakendus töötab.

![sisse- ja väljalülitamine](media/iot-hub-raspberry-pi-lessons/lesson4/gulp_on_and_off.png)

Palju õnne! Teie edukalt kohandatud sõnumeid, mis on pii saadetakse teie asjade keskuse kaudu.

### <a name="427-summary"></a>4.2.7 Kokkuvõte

See valikuline jaotis demos sõnumite kohandamine nii, et proovi taotluse saate määrata sisse- või väljalülitamine käitumine LED erineval viisil.

