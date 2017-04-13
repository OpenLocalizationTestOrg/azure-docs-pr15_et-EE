<properties
 pageTitle="Valimi rakenduse seadme pilve sõnumite saatmiseks | Microsoft Azure'i"
 description="Juurutada ja valimi rakendus oma Vaarika Pi 3, mis saadab sõnumid asjade jaoturi ja vilgub LED."
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

# <a name="32-run-sample-application-to-send-device-to-cloud-messages"></a>3,2 käivitada valimi seadme pilve sõnumite saatmiseks

## <a name="321-what-you-will-do"></a>3.2.1, mida te teete

Juurutada ja oma Vaarika Pi 3, mis saadab sõnumid teie asjade jaoturi valimi rakenduse käivitamiseks. Kui vastate probleeme, otsida lahendusi [lehe tõrkeotsing](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="322-what-you-will-learn"></a>3.2.2, mida saate

- Kuidas suutäis tööriista abil saate juurutada ja käivitada valimi Node.js rakenduse oma pii.

## <a name="323-what-you-need"></a>3.2.3 vajalik

- Peate on lõpule viidud eelmises jaotises klõpsake selle õppetüki: [Azure'i funktsioon rakendus ja töötlemiseks ja asjade jaoturi sõnumite talletamiseks Azure Storage konto loomine](iot-hub-raspberry-pi-kit-node-lesson3-deploy-resource-manager-template.md).

## <a name="324-get-your-iot-hub-and-device-connection-strings"></a>3.2.4 saada oma asjade jaoturi ja seadme ühendusstringi

Funktsiooni Pi ühenduse loomiseks oma asjade jaoturi kasutatakse seadme ühendusstring. Asjade jaoturi ühenduse stringi kasutatakse teie asjade jaoturi ühenduse seadme identiteedi, mis tähistab teie pii asjade keskuses.

- Hankida ühendusstringi asjade jaoturi Azure'i CLI järgmise käsu abil:

```bash
az iot hub show-connection-string --name {my hub name} --resource-group iot-sample
```

`{my hub name}`on nimi, mille määrasite tund 2. Kasutage `iot-sample` väärtus `{resource group name}` muutmisel ei tund 2 väärtus.

- Saada seadme ühendusstring, käivitage järgmine käsk:

```bash
az iot device show-connection-string --hub {my hub name} --device-id myraspberrypi --resource-group iot-sample
```

`{my hub name}`suunab sama väärtuse, mis eelmise käsu kasutada. Kasutage `iot-sample` väärtus `{resource group name}` ja kasutada `myraspberrypi` väärtus `{device id}` muutmisel ei tund 2 väärtus.

## <a name="325-configure-the-device-connection"></a>3.2.5 konfigureeriks seade

1. Lähtestab konfiguratsioonifail käivitades järgmised käsud:

    ```bash
    npm install
    gulp init
    ```

2. Avage seadme konfiguratsioonifail `config-raspberrypi.json` rakenduses Visual Studio kood, käivitage järgmine käsk:

    ```bash
    # For Windows command prompt
    code %USERPROFILE%\.iot-hub-getting-started\config-raspberrypi.json
  
    # For macOS or Ubuntu
    code ~/.iot-hub-getting-started/config-raspberrypi.json
    ```

    ![config.JSON](media/iot-hub-raspberry-pi-lessons/lesson3/config.png)

3. Veenduge, et järgmised asendused sisse selle `config-raspberrypi.json` fail:

  - Asendage **[seadme hostinimi või IP-aadress]** seadme IP-aadress või sisestada hostname `device-discovery-cli` või päritud konfigureeritud õppetüki 1 väärtus.
  - **[Asjade seadme ühendusstringi]** asendada selle `device connection string` saate saadud.
  - **[Asjade jaoturi ühendusstringi]** asendada selle `iot hub connection string` saate saadud.

Saate värskendada selle `config-raspberrypi.json` faili nii, et saate juurutada valimi rakendus oma arvutist.

## <a name="326-deploy-and-run-the-sample-application"></a>3.2.6 juurutada ja käivitage rakendus näidis

Juurutada ja käivitage rakendus valimi oma Pi, käivitage järgmine käsk:

```bash
gulp
```

> [AZURE.NOTE] Vaikimisi suutäis käivitub `install-tools`, `deploy`, ja `run` järjest toimingud. [Õppetüki 1](iot-hub-raspberry-pi-kit-node-lesson1-deploy-blink-app.md)käivitasite järgmised toimingud üksteise järel eraldi.

## <a name="327-verify-the-sample-application-works"></a>3.2.7 kinnitamine valimi rakendus töötab

Peaksite nägema LED, mis on ühendatud teie pii välkuvad iga kahe sekundi. Iga kord, kui LED vilgub, proovi taotluse saadab sõnumi oma asjade jaoturi ja kinnitab, et sõnum saadeti edukalt oma asjade jaoturi. Iga asjade jaoturi vastuvõetud sõnumid, prinditakse Lisaks sõnumi konsooli aken. Proovi taotluse lõpeb automaatselt pärast 20 sõnumite saatmiseks.

![](media/iot-hub-raspberry-pi-lessons/lesson3/gulp_run.png)

## <a name="328-summary"></a>3.2.8 Kokkuvõte

Olete juurutada ja käivitada uue vilkuv valimi rakenduse oma seadme pilve sõnumeid saata oma asjade jaoturi pii. Saate teisaldada järgmisse jaotisse sõnumite jälgimiseks, nagu need on kirjutatud Azure Storage kontole.

## <a name="next-steps"></a>Järgmised sammud

[3.3 loetud sõnumite säilis Azure Storage](iot-hub-raspberry-pi-kit-node-lesson3-read-table-storage.md)