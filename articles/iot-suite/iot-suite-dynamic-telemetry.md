<properties
    pageTitle="Kasutage dünaamiline telemeetria | Microsoft Azure'i"
    description="Järgige selles õppetükis saate teada, kuidas kasutada dünaamilise telemeetria järelevalve Remote'i eelkonfigureeritud lahendus."
    services=""
    suite="iot-suite"
    documentationCenter=""
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-suite"
     ms.devlang="na"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/25/2016"
     ms.author="dobett"/>

# <a name="use-dynamic-telemetry-with-the-remote-monitoring-preconfigured-solution"></a>Dünaamiliste telemeetria kasutamine järelevalve Remote'i eelkonfigureeritud lahendus

## <a name="introduction"></a>Sissejuhatus

Dünaamiliste telemeetria võimaldab teil visualiseerida kõik telemeetria saadetud järelevalve Remote'i eelkonfigureeritud lahendus. Eelkonfigureeritud lahenduse juurutamine jäljendatud seadmete saada temperatuuri- ja telemeetria, saate visualiseerida, armatuurlaual. Kui olemasoleva jäljendatud seadmete kohandamine, luua uue jäljendatud seadmete või füüsilise seadmete ühenduse eelkonfigureeritud lahenduse saate saata muud telemeetria väärtused, nt temperatuur, p/min või tuulekiirus. Te saate visualiseerida siis selle täiendavad telemeetria armatuurlaual.

Selle õpetuse kasutab lihtsa Node.js jäljendatud seadme, mille saab hõlpsasti muuta dünaamiline telemeetria eksperimenteerida.

Selle õpetuse tegemiseks peate.

- Azure'i aktiivne tellimus. Kui teil pole kontot, saate luua tasuta prooviversiooni konto vaid paar minutit. Lisateavet leiate teemast [Azure tasuta prooviversioon][lnk_free_trial].
- [Node.js] [ lnk-node] versioon 0.12.x või uuem versioon.

Selle õpetuse mis tahes operatsioonisüsteem, nt Windowsi või Linuxi, kus saate installida Node.js lõpuleviimine

[AZURE.INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

## <a name="configure-the-nodejs-simulated-device"></a>Node.js jäljendatud seadme konfigureerimine

1. Remote jälgimisega seotud armatuurlaual, klõpsake nuppu **+ Lisa seade** ja seejärel lisage kohandatud seade. Kirjutage asjade jaoturi hostname, seadme id ja seadme võtit. Vajate neid hiljem selles õpetuses ettevalmistamisel remote_monitoring.js seadme klientrakendusega.

2. Veenduge, et Node.js versioon 0.12.x või uuem versioon on installitud teie arvutisse arengu. Käivitage `node --version` käsuviibale või shell versiooni. Paketi manager installimiseks Node.js Linux kasutamise kohta leiate teemast [Installimist Node.js paketi halduri kaudu][node-linux].

3. Kui olete installinud Node.js, klooni uusim versioon [azure-asjade Interneti-SDK-d] [ lnk-github-repo] hoidla arengu teie arvutis. Kasutage **juhtslaidi** haru teekide ja näidised uusim versioon.

4. Kohaliku koopia [azure-asjade Interneti-SDK-d] ,[ lnk-github-repo] hoidla järgmised kaks faile kopeerida sõlm/seadme/näidised kausta tühja kausta arengu arvutis:

  - packages.JSON
  - remote_monitoring.js

5. Avage fail remote_monitoring.js ja otsige järgmine muutuv mõiste:

    ```
    var connectionString = "[IoT Hub device connection string]";
    ```

6. Asendage **[asjade jaoturi seadme ühendusstringi]** oma seadme ühendusstring. Kasutage väärtused asjade jaoturi hostname, seadme id ja seadme võtit seoses tehtud juhises 1. Ühendusstringi seade on järgmises vormingus:

    ```
    HostName={your IoT Hub hostname};DeviceId={your device id};SharedAccessKey={your device key}
    ```

    Kui teie asjade jaoturi hostname **contoso** ja seadme id on **mydevice**, teie ühendusstringi näeb välja umbes järgmine:

    ```
    var connectionString = "HostName=contoso.azure-devices.net;DeviceId=mydevice;SharedAccessKey=2s ... =="
    ```

7. Salvestage fail. Shell või kausta, mis sisaldab need failid installida vajalikud pakettide ja seejärel käivitage rakendus valimi Käsuviip käivitage järgmine käsk:

    ```
    npm install
    node remote_monitoring.js
    ```

## <a name="observe-dynamic-telemetry-in-action"></a>Dünaamiliste telemeetria tegelikkuses jälgida

Armatuurlaua näitab temperatuuri- ja telemeetria olemasoleva jäljendatud seadmetes:

![Vaikimisi armatuurlaual][image1]

Kui käivitasite eelmises jaotises Node.js jäljendatud seadme valimisel kuvatakse temperatuur, niiskus ja temperatuur telemeetria.

![Lisage välise temperatuur armatuurlaud][image2]

Remote jälgimise lahendus automaatselt tuvastab täiendavad temperatuur telemeetria tüüp ja lisab selle diagrammi armatuurlaual.

## <a name="add-a-telemetry-type"></a>Lisage telemeetria tüüp

Järgmiseks tuleb asendada uute väärtuste kogumi Node.js jäljendatud seadme loodud telemeetria:

1. Node.js jäljendatud seadet peatada, tippides Käsuviip või shell **Klahvikombinatsiooni Ctrl + C** .

2. Remote_monitoring.js faili, saate vaadata olemasolevaid temperatuur, niiskus ja temperatuur telemeetria põhiandmed väärtused. Lisage **pööret minutis** põhiandmed väärtus järgmiselt:

    ```
    // Sensors data
    var temperature = 50;
    var humidity = 50;
    var externalTemperature = 55;
    var rpm = 200;
    ```

3. Node.js jäljendatud seade kasutab funktsiooni **generateRandomIncrement** remote_monitoring.js faili lisada juhusliku lisandus põhiandmed väärtused. Juhuslikult **pööret minutis** väärtuse, lisades koodi joonele pärast olemasoleva randomizations järgmiselt:

    ```
    temperature += generateRandomIncrement();
    externalTemperature += generateRandomIncrement();
    humidity += generateRandomIncrement();
    rpm += generateRandomIncrement();
    ```

4. JSON last, seade saadab asjade jaoturi uue pööret minutis väärtuse lisamiseks tehke järgmist.

    ```
    var data = JSON.stringify({
      'DeviceID': deviceId,
      'Temperature': temperature,
      'Humidity': humidity,
      'ExternalTemperature': externalTemperature,
      'RPM': rpm
    });
    ```

5. Käivitage Node.js jäljendatud seadme abil järgmine käsk:

    ```
    node remote_monitoring.js
    ```

6. Uue pööret MINUTIS telemeetria tüüp, mis kuvab armatuurlaua diagrammil järgima:

![Lisage pööret MINUTIS armatuurlaud][image3]

> [AZURE.NOTE] Kui peate välja lülitada ja seejärel lubada Node.js seadme armatuurlaua muudatuste nägemiseks kohe lehel **seadmed** .

## <a name="customize-the-dashboard-display"></a>Armatuurlaua kuvamise kohandamine

**Seadme teavet** sõnumisse saab metaandmete telemeetria seadme saavad saata asjade keskuse kohta. Selle metaandmete saate määrata, et seade saadab telemeetria tüübid. Muutke **deviceMetaData** väärtus remote_monitoring.js faili lisada **käske** ülevaatamine **telemeetria** määratlus. Järgmised koodilõigu kuvatakse **käskude** määratlus (Veenduge, et lisada mõne `,` pärast **käskude** määratlus):

```
'Commands': [{
  'Name': 'SetTemperature',
  'Parameters': [{
    'Name': 'Temperature',
    'Type': 'double'
  }]
},
{
  'Name': 'SetHumidity',
  'Parameters': [{
    'Name': 'Humidity',
    'Type': 'double'
  }]
}],
'Telemetry': [{
  'Name': 'Temperature',
  'Type': 'double'
},
{
  'Name': 'Humidity',
  'Type': 'double'
},
{
  'Name': 'ExternalTemperature',
  'Type': 'double'
}]
```

> [AZURE.NOTE] Remote jälgimise lahendus kasutab väiketähed vaste võrdlemiseks metaandmete määratluse telemeetria voo andmetega.

Lisamise **telemeetria** määratlus, nagu on näidatud eelmise koodilõigu muuta armatuurlaua toimimist. Metaandmete võite ka **DisplayName** atribuudi Kuva armatuurlaua kohandamiseks. Värskendage **telemeetria** metaandmete määratlus, nagu on näidatud järgmises koodilõigu.

```
'Telemetry': [
{
  'Name': 'Temperature',
  'Type': 'double',
  'DisplayName': 'Temperature (C*)'
},
{
  'Name': 'Humidity',
  'Type': 'double',
  'DisplayName': 'Humidity (relative)'
},
{
  'Name': 'ExternalTemperature',
  'Type': 'double',
  'DisplayName': 'Outdoor Temperature (C*)'
}
]
```

Järgmine pilt näitab, kuidas see muudatus muudab diagrammi legendi armatuurlaual:

![Diagrammi legendi kohandamine][image4]

> [AZURE.NOTE] Kui peate välja lülitada ja seejärel lubada Node.js seadme armatuurlaua muudatuste nägemiseks kohe lehel **seadmed** .

## <a name="filter-the-telemetry-types"></a>Filtreerimine telemeetria tüübid

Vaikimisi kuvatakse diagrammi armatuurlaual telemeetria voo iga andmesarja. Metaandmete **Seadme teabe** abil saate telemeetria kindlat tüüpi diagrammi kuvamise. 

Veendumaks, et kuvada ainult temperatuuri- ja telemeetria diagrammi, argument **ExternalTemperature** **Seadme teavet** **telemeetria** metaandmete kaudu järgmiselt:

```
'Telemetry': [
{
  'Name': 'Temperature',
  'Type': 'double',
  'DisplayName': 'Temperature (C*)'
},
{
  'Name': 'Humidity',
  'Type': 'double',
  'DisplayName': 'Humidity (relative)'
},
//{
//  'Name': 'ExternalTemperature',
//  'Type': 'double',
//  'DisplayName': 'Outdoor Temperature (C*)'
//}
]
```

**Temperatuur** enam kuvatakse diagrammi.

![Telemeetria armatuurlaual filtreerimine][image5]

See muudatus mõjutab ainult diagrammi kuvamine. **ExternalTemperature** andmeväärtusi on endiselt talletatakse ja tehakse kättesaadavaks mis tahes taustväärtus töötlemiseks.

> [AZURE.NOTE] Kui peate välja lülitada ja seejärel lubada Node.js seadme armatuurlaua muudatuste nägemiseks kohe lehel **seadmed** .

## <a name="handle-errors"></a>Vigade

Jaoks andmevoos kuvamiseks klõpsake diagrammi, **Tüüp** metaandmete **Seadme teavet** peab vastama telemeetria väärtuste andmetüüpi. Näiteks kui metaandmete niiskus andmete **Tüüp** on **int** ja on **kahekordsete** on leitud telemeetria voo siis niiskus telemeetria ei kuvata diagrammil. Siiski **niiskus** väärtused on endiselt talletatakse ja tehakse kättesaadavaks mis tahes taustväärtus töötlemiseks.

## <a name="next-steps"></a>Järgmised sammud

Nüüd, kui olete näinud dünaamiline telemeetria kasutamise kohta, saate lisateavet kohta, kuidas kasutada eelkonfigureeritud lahenduste seadme teave: [seadme teabe metaandmete serveri jälgimise eelkonfigureeritud lahendus][lnk-devinfo].

[lnk-devinfo]: iot-suite-remote-monitoring-device-info.md

[image1]: media/iot-suite-dynamic-telemetry/image1.png
[image2]: media/iot-suite-dynamic-telemetry/image2.png
[image3]: media/iot-suite-dynamic-telemetry/image3.png
[image4]: media/iot-suite-dynamic-telemetry/image4.png
[image5]: media/iot-suite-dynamic-telemetry/image5.png

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-node]: http://nodejs.org
[node-linux]: https://github.com/nodejs/node-v0.x-archive/wiki/Installing-Node.js-via-package-manager
[lnk-github-repo]: https://github.com/Azure/azure-iot-sdks
