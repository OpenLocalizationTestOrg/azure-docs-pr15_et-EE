<properties
 pageTitle="Sõnumite lugemine säilis Azure Storage | Microsoft Azure'i"
 description="Nad on kirjutanud Azure'i tabelimälu oma seadme pilve sõnumite jälgida."
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

# <a name="33-read-messages-persisted-in-azure-storage"></a>3.3 loetud sõnumite säilis Azure Storage

## <a name="331-what-will-you-do"></a>3.3.1, mida te teete

Jälgida seadme pilve sõnumid, mis saadetakse teie asjade jaoturi nagu sõnumite kirjutada oma Azure'i tabelimälu oma Vaarika Pi 3. Kui vastate probleeme, otsida lahendusi [lehe tõrkeotsing](iot-hub-raspberry-pi-kit-node-troubleshooting.md).

## <a name="332-what-will-you-learn"></a>3.3.2 mis saate teada

- Azure'i tabelimälu oma säilis suutäis sõnumi lugemine tööülesande abil saate sõnumeid lugeda.

## <a name="333-what-do-you-need"></a>3.3.3 mida on vaja

- Peate on lõpule viidud [Azure vilkuv valimi rakenduse Vaarika Pi 3](iot-hub-raspberry-pi-kit-node-lesson3-run-azure-blink.md)eelmisest jaotisest.

## <a name="334-read-new-messages-from-your-storage-account"></a>3.3.4 lugeda uute sõnumite oma salvestusruumi konto

Eelmises jaotises käivitasite proovi taotluse oma pii. Valimi rakenduse saadetud sõnumid teie Azure'i asjade jaoturi. Oma asjade jaoturi saadetud sõnumid salvestatakse teie Azure'i tabelimälu Azure funktsioon rakenduse kaudu. Teil on vaja Azure Storage ühendusstringi Azure'i tabelimälu oma sõnumeid lugeda.

Azure'i tabelimälus talletatud sõnumite lugemiseks toimige järgmiselt.

1. Ühendusstringi hankimine käivitades järgmised käsud:

    ```bash
    az storage account list -g iot-sample --query [].name
    az storage account show-connection-string -g iot-sample -n {storage name}
    ```

    Esimese käsu toob selle `storage name` mis on kasutusel teine käsk saada ühendusstring. `iot-sample`Vaikeväärtus on `{resource group name}` muutmisel ei tund 2 väärtus.

2. Avage fail konfiguratsiooni `config-raspberrypi.json` faili Visual Studio kood, käivitage järgmine käsk:

    ```bash
    # For Windows command prompt
    code %USERPROFILE%\.iot-hub-getting-started\config-raspberrypi.json

    # For macOS or Ubuntu
    code ~/.iot-hub-getting-started/config-raspberrypi.json
    ```

3. Asendage `[Azure Storage connection string]` koos ühendusstringiga teil juhises 1.
4. Salvestage soovitud `config-raspberrypi.json` faili.
5. Sõnumite uuesti saata ja lugeda nende oma Azure'i tabelimälu, käivitage järgmine käsk:

    ```bash
    gulp run --read-storage
    ```

    Azure'i tabelimälu lugemine loogika on selle `azure-table.js` fail.

    ![Käivitage suutäis--lugemis-salvestusruumi](media/iot-hub-raspberry-pi-lessons/lesson3/gulp_read_message.png)

## <a name="335-summary"></a>3.3.5 Kokkuvõte

Olete edukalt ühendatud teie pii oma asjade jaoturi pilves ja vilkuv valimi rakenduse kasutatud seadme pilve sõnumite saatmiseks. Saate kasutada ka Azure funktsioon rakenduse salvestada oma Azure'i tabelimälu asjade jaoturi sõnumid. Te saate liikuda järgmisele õppetüki kohta, kuidas oma asjade jaoturi cloud-seadme sõnumeid saata oma pii.

## <a name="next-steps"></a>Järgmised sammud

[Õppetüki 4: Pilveteenuste-seadme sõnumite saatmine](iot-hub-raspberry-pi-kit-node-lesson4-send-cloud-to-device-messages.md)
