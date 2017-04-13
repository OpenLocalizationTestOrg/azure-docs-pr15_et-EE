<properties
   pageTitle="Ühendage seade Node.js abil | Microsoft Azure'i"
   description="Selles artiklis kirjeldatakse Azure'i asjade komplekti eelkonfigureeritud remote jälgimisega seotud lahenduse saate rakendust kirjutatud Node.js seadmega ühendada."
   services=""
   suite="iot-suite"
   documentationCenter="na"
   authors="dominicbetts"
   manager="timlt"
   editor=""/>

<tags
   ms.service="iot-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/05/2016"
   ms.author="dobett"/>


# <a name="connect-your-device-to-the-remote-monitoring-preconfigured-solution-nodejs"></a>Ühendage seade remote jälgimise eelkonfigureeritud lahendus (Node.js)

[AZURE.INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

## <a name="build-and-run-the-nodejs-sample-solution"></a>Luua ja käivitada node.js valimi lahendus

1. *Microsoft Azure'i asjade SDK-d* GitHub hoidla klooni ja installige *Microsoft Azure'i asjade seadme SDK Node.js* oma töölaua keskkonnas, järgige [oma arenduskeskkond ettevalmistamine] [ lnk-github-prepare] juhiseid.

2. Kohaliku koopia [azure-asjade Interneti-SDK-d] ,[ lnk-github-repo] hoidla järgmised kaks failide kopeerimine sõlm/seadme/näidised kaustast kausta seadme:

  - packages.JSON
  - remote_monitoring.js

3. Avage fail remote_monitoring.js ja vaadake, kas järgmine muutuja:

    ```
    var connectionString = "[IoT Hub device connection string]";
    ```

4. Asendage **[asjade jaoturi seadme ühendusstringi]** oma seadme ühendusstring. Leiate väärtused oma asjade jaoturi hostname, seadme id ja seadme võtit remote jälgimisega seotud lahenduse armatuurlaual. Ühendusstringi seade on järgmises vormingus:

    ```
    HostName={your IoT Hub hostname};DeviceId={your device id};SharedAccessKey={your device key}
    ```

    Kui teie asjade jaoturi hostname **contoso** ja seadme id on **mydevice**, näeb teie ühendusstring.

    ```
    var connectionString = "HostName=contoso.azure-devices.net;DeviceId=mydevice;SharedAccessKey=2s ... =="
    ```

5. Salvestage fail. Käivitage kaust, mis sisaldab need failid installida vajalikud pakettide ja seejärel käivitage rakendus valimi käsuviibale järgmised käsud:

    ```
    npm install --save
    node remote_monitoring.js
    ```

[AZURE.INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]

[lnk-github-repo]: https://github.com/azure/azure-iot-sdks
[lnk-github-prepare]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md