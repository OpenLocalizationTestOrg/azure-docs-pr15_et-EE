<properties
 pageTitle="Valimi rakenduse sõnumeid cloud-seade | Microsoft Azure'i"
 description="Õppetüki 4 valimi rakendus töötab teie pii ja jälgib sissetulevaid sõnumeid oma asjade keskuse kaudu. Uue tööülesande suutäis saadab sõnumite oma Pi oma asjade jaoturis vilgub LED."
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

# <a name="41-run-the-sample-application-to-receive-cloud-to-device-messages"></a>4.1 cloud-seadme sõnumeid valimi rakenduse käivitada

Selles jaotises juurutamist proovi taotluse oma Vaarika Pi 3. Proovi taotluse jälgib sissetulevaid sõnumeid oma asjade keskuse kaudu. Suutäis toimingut käivitada ka arvutis oma asjade keskuse kaudu oma Pi sõnumeid saata. Sõnumite saamisel valimi rakenduse vilgub LED. Kui vastate probleeme, otsida lahendusi [lehe tõrkeotsing](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="411-what-you-will-do"></a>4.1.1, mida te teete

- Ühendada oma asjade jaoturi rakenduse valimi.
- Juurutada ja valimi rakenduse käivitada.
- Sõnumite saatmine oma asjade keskuse kaudu oma Pi vilgub LED abil.

## <a name="412-what-you-will-learn"></a>4.1.2 mis saate teada

- Kuidas oma asjade keskuse kaudu sissetulevate sõnumite jälgimine.
- Kuidas oma asjade jaoturi cloud-seadme sõnumeid saata oma pii. 

## <a name="413-what-do-you-need"></a>4.1.3 mida on vaja

- Vaarikas pii 3, mis on loodud kasutamiseks. Siit saate teada, kuidas häälestada oma Pi, lugege teemat [õppetüki 1: alustamine seadme Vaarika Pi 3](iot-hub-raspberry-pi-kit-node-get-started.md)
- Asjade jaoturi, mis on loodud Azure tellimuse. Oma Azure'i asjade jaoturi loomise kohta leiate teemast [tund 2: Looge oma Azure'i asjade jaoturi](iot-hub-raspberry-pi-kit-node-get-started.md)

## <a name="414-connect-the-sample-application-to-your-iot-hub"></a>4.1.4 ühendada oma asjade jaoturi rakenduse näidis

1. Veenduge, et olete repo kausta `iot-hub-node-raspberrypi-getting-started`. Visual Studio kood valimi rakenduse avamiseks järgmised käsud:

    ```bash
    cd Lesson4
    code .
    ```

    Teade kuvatakse `app.js` faili selle `app` alamkausta. Funktsiooni `app.js` fail on klahv allika fail, mis sisaldab koodi jälgida sissetulevate sõnumite asjade keskuse kaudu. Funktsiooni `blinkLED` funktsioon vilgub LED.

    ![Repo struktuur](media/iot-hub-raspberry-pi-lessons/lesson4/repo_structure.png)

2. Lähtestab konfiguratsioonifail järgmised käsud:

    ```bash
    npm install
    gulp init
    ```

    Kui olete lõpetanud õppetüki 3 selles arvutis, kuvatakse kõik päritakse nii, et võite jätkata juhisega 4.1.5. Kui õppetüki 3 mõnes teises arvutis, peate selle kohatäidete asendamine selle `config-raspberrypi.json` fail. Funktsiooni `config-raspberrypi.json` fail on home kaustas alamkausta.

    ![Config](media/iot-hub-raspberry-pi-lessons/lesson4/config_raspberrypi.png)

- Asendage oma Pi IP-aadress või hostname, mis kuvatakse käsu **[seadme hostinimi või IP-aadress]**`devdisco list --eth`
- Asendage **[asjade seadme ühendusstringi]** seadme ühendusstring, mida saate käsu `az iot hub show-connection-string --name {my hub name} --resource-group {resource group name}`.
- Asendage **[asjade jaoturi ühendusstringi]** asjade jaoturi ühendusstring, mida saate käsu `az iot device show-connection-string --hub {my hub name} --device-id {device id} --resource-group {resource group name}`.

## <a name="415-deploy-and-run-the-sample-application"></a>4.1.5 juurutada ja käivitage rakendus näidis

Juurutada ja käivitage rakendus valimi oma Pi käivitades järgmised käsud:
  
```
gulp
```

Installi-tööriistade tööülesande esmalt käivitatakse suutäis käsk. Seejärel kasutab seda oma Pi valimi rakenduse. Lõpuks see töötab rakendus oma pii ja eraldi tööülesande hosti arvutis 20 vilkuv sõnumeid saata oma Pi oma asjade keskuse kaudu.

Kui valimi rakendus töötab, see algab listening sõnumid teie asjade keskuse kaudu. Samal ajal suutäis tööülesande saadab mitut "vilkuv" sõnumid teie asjade keskuse kaudu oma pii. Iga vilkuv sõnumit saanud, valimi rakendus nõuab blinkLED funktsioon vilgub LED.

Peaksite nägema LED vilgub iga kahe sekundi järel, nagu suutäis tööülesanne on 20 sõnumeid saata oma asjade keskuse kaudu oma pii. Viimane on "stop" teade, mis ütleb rakendus töötab lõpetada.

![Suutäis](media/iot-hub-raspberry-pi-lessons/lesson4/gulp_blink.png)

## <a name="416-summary"></a>4.1.6 Kokkuvõte

Olete edukalt saadetud sõnumid teie asjade keskuse kaudu abil oma Pi vilgub LED abil. Järgmises jaotises on valikuline jaotis, mis näitab, kuidas muuta sisse- või väljalülitamine LED toimimist.

## <a name="next-steps"></a>Järgmised sammud

[Valikuline jaotis: sisse- või väljalülitamine käitumine LED muutmine](iot-hub-raspberry-pi-kit-node-lesson4-change-led-behavior.md)