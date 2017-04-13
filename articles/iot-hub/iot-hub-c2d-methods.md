<properties
 pageTitle="Kasutada otsest meetodit | Microsoft Azure'i"
 description="Selle õpetuse näitab, kuidas kasutada otsese meetodid"
 services="iot-hub"
 documentationCenter=""
 authors="nberdy"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/05/2016"
 ms.author="nberdy"/>

# <a name="tutorial-use-direct-methods"></a>Õpetus: Kasutada otsese meetodid

## <a name="introduction"></a>Sissejuhatus

Azure'i asjade jaoturi on täielikult hallatav teenus, mis võimaldab usaldusväärsest ja turvaline kahesuunalise side miljoneid asjade seadmed ja rakenduse uuesti end. Eelmise õpetused ([Alustamine asjade jaoturi] ja [saatke Cloud-seadme asjade jaoturi]) illustreerimine seadme pilve ja pilveteenuse-seadme sõnumside põhifunktsioone asjade jaoturi. Asjade jaoturi annab teile võimalus autonoomsest lühiealisi meetodite pilvest seadmetes. Meetodite tähistavad taotlus-vasta suhtluse seadmega, mis on sarnane HTTP-kõne, et neil õnnestub või lasta teada kõne olekut kasutaja kohe (pärast kasutaja määratud ajalõpp) ei. [Otsest meetodit seadmes] [ lnk-devguide-methods] kirjeldatakse lähemalt viise ja pakub juhised selle kohta, millal kasutada meetodite võrreldes cloud-seadme sõnumeid.

Selles õppetükis saate teada, kuidas soovite:

- Azure portaali abil saate luua ka asjade jaoturi ja luua oma asjade keskuses seadme identiteedi.
- Looge jäljendatud seade, millel on otsene meetod, mis saab kutsuda pilve.
- Looge konsooli rakendus, mis nõuab otsene jäljendatud seadmes oma asjade jaoturi kaudu.

> [AZURE.NOTE] Sel ajal, otsese meetodite pääseb juurde ainult asjade jaoturi MQTT protokolli abil ühendatud seadmete. Palun vaadake [MQTT toetavad] [ lnk-devguide-mqtt] artiklist leiate juhised, kuidas muuta olemasolevaid seadme rakenduse kasutamiseks MQTT.

Õppeteema lõpus on teil kaks Node.js konsooli rakendusi.

* **CallMethodOnDevice.js**, mis nõuab meetodi jäljendatud seadmes ja kuvab vastus.
* **SimulatedDevice.js**, mis ühendab oma asjade jaoturi varem loodud seadme identiteediga ja vastaks kutsunud pilveteenuses meetodit.

> [AZURE.NOTE] Artikli [Asjade jaoturi SDK-d] [ lnk-hub-sdks] teave on erinevate SDK-d, mille abil saate luua nii rakendusi käivitada seadmed ja teie lahendus tagasi end.

Selle õpetuse tegemiseks vajate järgmist:

+ Node.js versioon 0.10.x või uuem versioon.

+ Aktiivne Azure'i konto. (Kui teil pole kontot, saate luua [tasuta konto] [ lnk-free-trial] vaid paar minutit.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-a-simulated-device-app"></a>Jäljendatud seadme rakenduse loomine

Selles jaotises saate luua Node.js konsooli rakendus, mis vastab meetodi kutsunud pilve.

1. Looge uus tühi kaust nimega **simulateddevice**. Klõpsake kaustas **simulateddevice** abil oma käsuviibas järgmine käsk package.json faili loomine. Kõik vaikesätted aktsepteerida:

    ```
    npm init
    ```

2. Veebisaidil oma käsuviiba **simulateddevice** kausta, käivitage järgmine käsk installida **azure-asjade Interneti-seade** seadme SDK paketi ja **azure asjade Interneti-seadme mqtt** paketi:

    ```
    npm install azure-iot-device@dtpreview azure-iot-device-mqtt@dtpreview --save
    ```

3. Kaustas **simulateddevice** tekstiredaktoris kasutamisel **SimulatedDevice.js** uue faili loomine.

4. Lisage järgmine `require` laused **SimulatedDevice.js** faili alguses:

    ```
    'use strict';

    var Mqtt = require('azure-iot-device-mqtt').Mqtt;
    var DeviceClient = require('azure-iot-device').Client;
    ```

5. Lisage **connectionString** muutuja ja selle seadme kliendi loomiseks kasutada. Ühendusstringi *loomine seadme identiteedi* jaotises loodud **{seadme ühendusstringi}** asendamiseks tehke järgmist.

    ```
    var connectionString = '{device connection string}';
    var client = DeviceClient.fromConnectionString(connectionString, Mqtt);
    ```

6. Järgmised funktsiooni rakendada meetodit seadme lisamiseks tehke järgmist.

    ```
    function onWriteLine(request, response) {
        console.log(request.payload);

        response.send(200, 'Input was written to log.', function(err) {
            if(err) {
                console.error('An error ocurred when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.' );
            }
        });
    }
    ```

7. Avatud ühendus oma asjade jaoturi ja start lähtestab meetod kuulajale:

    ```
    client.open(function(err) {
        if (err) {
            console.error('could not open IotHub client');
        }  else {
            console.log('client opened');
            client.onDeviceMethod('writeLine', onWriteLine);
        }
    });
    ```

8. Salvestage ja sulgege fail **SimulatedDevice.js** .

> [AZURE.NOTE] Selles õpetuses juuruta hoida asju lihtne, mis tahes uuesti poliitika. Kood, tuleks rakendada uuesti poliitikate (nt ühenduse uuesti), soovitatud MSDN-i artiklist [Siirdamiseks viga töötlemise][lnk-transient-faults].

## <a name="call-a-method-on-a-device"></a>Kõne meetodi seadmes

Selles jaotises saate luua Node.js konsooli rakendus, mis meetodi kutsub jäljendatud seade ja seejärel kuvab vastus.

1. Looge uus tühi kaust nimega **callmethodondevice**. Klõpsake kaustas **callmethodondevice** abil oma käsuviibas järgmine käsk package.json faili loomine. Kõik vaikesätted aktsepteerida:

    ```
    npm init
    ```

2. Veebisaidil oma käsuviiba **callmethodondevice** kausta, installige **Azure'i-iothub** pakett järgmine käsk:

    ```
    npm install azure-iothub@dtpreview --save
    ```

3. Kasutades tekstiredaktoris, luua **CallMethodOnDevice.js** faili **callmethodondevice** kausta.

4. Lisage järgmine `require` laused **CallMethodOnDevice.js** faili alguses:

    ```
    'use strict';

    var Client = require('azure-iothub').Client;
    ```

5. Lisada järgmised muutuv deklareerimise ja asendage kohatäite väärtuse jaoks oma asjade jaoturi ühendusstringi:

    ```
    var connectionString = '{iothub connection string}';
    var methodName = 'writeLine';
    var deviceId = 'myDeviceId';
    ```

6. Kliendi avamiseks oma asjade jaoturi ühendus luua.

    ```
    var client = Client.fromConnectionString(connectionString);
    ```
    
7. Järgmise funktsiooni seadme meetodi ja printida konsooli seadme vastuse lisamiseks tehke järgmist.

    ```
    var methodParams = {
        methodName: methodName,
        payload: 'a line to be written',
        timeoutInSeconds: 30
    };

    client.invokeDeviceMethod(deviceId, methodParams, function (err, result) {
        if (err) {
            console.error('Failed to invoke method \'' + methodName + '\': ' + err.message);
        } else {
            console.log(methodName + ' on ' + deviceId + ':');
            console.log(JSON.stringify(result, null, 2));
        }
    });
    ```

7. Salvestage ja sulgege fail **CallMethodOnDevice.js** .

## <a name="run-the-applications"></a>Rakenduste käivitamine

Nüüd olete valmis rakendusi käivitada.

1. Käsuviibale- **simulateddevice** kausta, käivitage järgmine käsk alustamiseks listening meetod kõnede oma asjade keskuse kaudu:

    ```
    node SimulatedDevice.js
    ```

    ![][7]
    
2. Käsuviibale- **callmethodondevice** kausta, käivitage järgmine käsk oma asjade jaoturi jälgimise alustamiseks:

    ```
    node CallMethodOnDevice.js 
    ```

    ![][8]
    
3. Kuvatakse seade reageerida meetodit sõnumi ja rakendust, mis nimega meetod kuvamisviisi vastus seadmest välja printida.

    ![][9]
    
## <a name="next-steps"></a>Järgmised sammud

Selles õpetuses konfigureeritud uue asjade jaoturi Azure portaali ja seejärel loodud seadme identiteedi jaoturi identiteedi registri. Selle seadme identiteedi saate kasutada jäljendatud seadmes rakendus reageerida meetodid, millele pilve. Saate ka loodud rakendus, mis käivitab meetodite seadmes ja kuvab seadme vastus. 

Alustamine asjade jaoturi jätkamiseks ja teiste stsenaariumide asjade avastamiseks näha:

- [Asjade jaoturi kasutamise alustamine]
- [Mitmes seadmes tööde plaanimine][lnk-devguide-jobs]

Saate teada, kuidas laiendada oma asjade lahenduse ja ajakava nõuab mitmes seadmes, leiate [ajakava ja leviedastuse töökohtade] [ lnk-tutorial-jobs] õpetuse.

<!-- Images. -->
[7]: ./media/iot-hub-c2d-methods/run-simulated-device.png
[8]: ./media/iot-hub-c2d-methods/run-callmethodondevice.png
[9]: ./media/iot-hub-c2d-methods/methods-output.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-tutorial-jobs]: iot-hub-schedule-jobs.md
[lnk-devguide-methods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[Saatke asjade jaoturi Cloud-seade]: iot-hub-csharp-csharp-c2d.md
[Process Device-to-Cloud messages]: iot-hub-csharp-csharp-process-d2c.md
[Asjade jaoturi kasutamise alustamine]: iot-hub-node-node-getstarted.md
