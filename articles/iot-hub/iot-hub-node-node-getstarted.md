<properties
    pageTitle="Azure'i asjade jaotis Node.js alustamine | Microsoft Azure'i"
    description="Azure'i asjade jaoturi Node.js saada alustamine õpetuse. Azure'i asjade jaoturi ja Node.js asjade Interneti lahenduse kasutusele Microsoft Azure'i asjade SDK-d kasutada."
    services="iot-hub"
    documentationCenter="nodejs"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="javascript"
     ms.topic="hero-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/12/2016"
     ms.author="dobett"/>

# <a name="get-started-with-azure-iot-hub-for-nodejs"></a>Azure'i asjade jaoturi Node.js kasutamise alustamine

[AZURE.INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

Õppeteema lõpus on teil kolm Node.js konsooli rakendusi.

* **CreateDeviceIdentity.js**, mis loob seadme identiteedi ja nendega seotud võti jäljendatud videoseadme ühendamiseks.
* **ReadDeviceToCloudMessages.js**, mis kuvab telemeetria jäljendatud seadmest.
* **SimulatedDevice.js**, mis ühendab oma asjade jaoturi varem loodud seadme identiteediga ja saadab sõnumi telemeetria iga teise abil AMQP Protocol (protokoll).

> [AZURE.NOTE] Artikli [Asjade jaoturi SDK-d] [ lnk-hub-sdks] teave on erinevate SDK-d, mille abil saate luua nii rakendusi käivitada seadmed ja teie lahendus tagasi end.

Selle õpetuse tegemiseks vajate järgmist:

+ Node.js versioon 0.10.x või uuem versioon.

+ Aktiivne Azure'i konto. (Kui teil pole kontot, saate luua [tasuta konto] [ lnk-free-trial] vaid paar minutit.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

Nüüd olete loonud oma asjade jaoturi. Teil on asjade jaoturi hostname ja selle õpetuse ülejäänud soovitud asjade jaoturi ühendusstring.

## <a name="create-a-device-identity"></a>Seadme identiteedi loomine

Selles jaotises saate luua Node.js konsooli rakendus, mis loob teie asjade keskuses registris identiteedi seadme identiteedi. Seade ei saa ühendust asjade jaoturi juhul, kui kirje on registris seadme identiteedi. **Seadme identiteedi registri** jaotisest [Asjade jaoturi arendaja juhend] [ lnk-devguide-identity] lisateabe saamiseks. Kui käivitate selle konsooli rakendus, see tekitab kordumatu seadme ID ja seadme abil saate ise kindlaks teha, kui seadme pilve sõnumid saadetakse asjade jaoturi võti.

1. Looge uus tühi kaust nimega **createdeviceidentity**. Klõpsake kaustas **createdeviceidentity** abil oma käsuviibas järgmine käsk package.json faili loomine. Kõik vaikesätted aktsepteerida:

    ```
    npm init
    ```

2. Veebisaidil oma käsuviiba **createdeviceidentity** kausta, käivitage järgmine käsk installida **azure-iothub** teenuse SDK pakett:

    ```
    npm install azure-iothub --save
    ```

3. Kasutades tekstiredaktoris, luua **CreateDeviceIdentity.js** faili **createdeviceidentity** kausta.

4. Lisage järgmine `require` lause alguses **CreateDeviceIdentity.js** faili:

    ```
    'use strict';
    
    var iothub = require('azure-iothub');
    ```

5. Lisada järgmine kood **CreateDeviceIdentity.js** faili ja asendage kohatäite väärtuse jaoks loodud eelmises jaotises asjade jaoturi ühendusstring. 

    ```
    var connectionString = '{iothub connection string}';
    
    var registry = iothub.Registry.fromConnectionString(connectionString);
    ```

6. Lisage järgmine kood oma asjade keskuses registris seadme identiteedi seadme määratlus loomiseks. Järgmine kood loob seade, kui seadme ID-d ei ole registris, muul juhul annab vastuseks olemasoleva seadme võti:

    ```
    var device = new iothub.Device(null);
    device.deviceId = 'myFirstNodeDevice';
    registry.create(device, function(err, deviceInfo, res) {
      if (err) {
        registry.get(device.deviceId, printDeviceInfo);
      }
      if (deviceInfo) {
        printDeviceInfo(err, deviceInfo, res)
      }
    });

    function printDeviceInfo(err, deviceInfo, res) {
      if (deviceInfo) {
        console.log('Device id: ' + deviceInfo.deviceId);
        console.log('Device key: ' + deviceInfo.authentication.SymmetricKey.primaryKey);
      }
    }
    ```

7. Salvestage ja sulgege **CreateDeviceIdentity.js** fail.

8. **Createdeviceidentity** rakenduse käivitamiseks käivitada createdeviceidentity kausta-käsuviibale järgmine käsk:

    ```
    node CreateDeviceIdentity.js 
    ```

9. Kirjutage **seadme id** ja **seadme võtit**. Peate need väärtused hiljem kui loote rakendus, mis loob ühenduse asjade jaoturi seade.

> [AZURE.NOTE] Asjade jaoturi identiteedi registri salvestab ainult seadme identiteedid lubamiseks jaoturi turvaline juurdepääs. See talletab seadme ID-d ja klahvid, mida kasutada turvalisus mandaadid ja lubatud või keelatud lipp, mille abil saate üksikuid seadmes juurdepääsu keelamiseks. Kui rakenduse peab muu seadme metaandmete talletada, kasutada selle poest rakenduse kohased. [Asjade jaoturi arendaja juhend] viidata[ lnk-devguide-identity] lisateabe saamiseks.

## <a name="receive-device-to-cloud-messages"></a>Seadme pilve sõnumeid

Selles jaotises saate luua Node.js konsooli rakendus, mis loeb seadme pilve sõnumite asjade keskuse kaudu. Soovitud asjade jaoturi seab mõne [Sündmuse jaoturi][lnk-event-hubs-overview]-ühilduv lõpp-punkti selleks, et saaksite seadme pilve sõnumeid lugeda. Selles õpetuses hoida asju on lihtne, loob lihtsa lugeja, mis ei sobi kõrge läbilaskevõime juurutamine. [Protsessi seadme pilve sõnumite] [ lnk-process-d2c-tutorial] õpetuse näitab, kuidas seadme pilve sõnumite tasandil. [Alustamine sündmuse jaoturi] [ lnk-eventhubs-tutorial] õppetükk annab täiendava teabe kohta, kuidas protsessi sõnumeid sündmuse jaoturi kaudu ja on mõeldud asjade jaoturi sündmuse jaoturi ühilduv lõpp-punktid.

> [AZURE.NOTE] Sündmuse jaoturi ühilduv lõpp-punkti jaoks seadme pilve sõnumite lugemine. alati kasutab AMQP Protocol (protokoll).

1. Looge uus tühi kaust nimega **readdevicetocloudmessages**. Klõpsake kaustas **readdevicetocloudmessages** abil oma käsuviibas järgmine käsk package.json faili loomine. Kõik vaikesätted aktsepteerida:

    ```
    npm init
    ```

2. Veebisaidil oma käsuviiba **readdevicetocloudmessages** kausta, käivitage järgmine käsk installida **azure-sündmuse-jaoturi** paketi:

    ```
    npm install azure-event-hubs --save
    ```

3. Kasutades tekstiredaktoris, luua **ReadDeviceToCloudMessages.js** faili **readdevicetocloudmessages** kausta.

4. Lisage järgmine `require` laused **ReadDeviceToCloudMessages.js** faili alguses:

    ```
    'use strict';

    var EventHubClient = require('azure-event-hubs').Client;
    ```

5. Lisada järgmised muutuv deklareerimise ja asendage kohatäite väärtuse jaoks oma asjade jaoturi ühendusstringi:

    ```
    var connectionString = '{iothub connection string}';
    ```

6. Järgmised kaks funktsioonid, mida printida väljundi konsooli lisamiseks tehke järgmist.

    ```
    var printError = function (err) {
      console.log(err.message);
    };

    var printMessage = function (message) {
      console.log('Message received: ');
      console.log(JSON.stringify(message.body));
      console.log('');
    };
    ```

7. Lisage järgmine kood loomine **EventHubClient**, avage oma asjade jaoturi ühendus ja vastuvõtja iga sektsiooni jaoks luua. See rakendus kasutab filtri loob vastuvõtja nii, et vastuvõtja ainult loeb pärast vastuvõtja käivitub asjade jaoturi saadetud sõnumid. See filter on kasulik, testimiskeskkonnas, et kuvatakse ainult praegusi sõnumeid. Tootmiskeskkonnas, teie kood tuleks veenduge, et kõik sõnumid - töötleb saate teada, [Kuidas asjade jaoturi seadme pilve sõnumite] [ lnk-process-d2c-tutorial] Lisateavet õpetus:

    ```
    var client = EventHubClient.fromConnectionString(connectionString);
    client.open()
        .then(client.getPartitionIds.bind(client))
        .then(function (partitionIds) {
            return partitionIds.map(function (partitionId) {
                return client.createReceiver('$Default', partitionId, { 'startAfterTime' : Date.now()}).then(function(receiver) {
                    console.log('Created partition receiver: ' + partitionId)
                    receiver.on('errorReceived', printError);
                    receiver.on('message', printMessage);
                });
            });
        })
        .catch(printError);
    ```

8. Salvestage ja sulgege fail **ReadDeviceToCloudMessages.js** .

## <a name="create-a-simulated-device-app"></a>Jäljendatud seadme rakenduse loomine

Selles jaotises saate luua Node.js konsooli rakendus, mis jäljendab seade, mis saadab seadme pilve sõnumite asjade jaoturiga.

1. Looge uus tühi kaust nimega **simulateddevice**. Klõpsake kaustas **simulateddevice** abil oma käsuviibas järgmine käsk package.json faili loomine. Kõik vaikesätted aktsepteerida:

    ```
    npm init
    ```

2. Veebisaidil oma käsuviiba **simulateddevice** kausta, käivitage järgmine käsk installida **azure-asjade Interneti-seade** seadme SDK paketi ja **azure asjade Interneti-seadme amqp** paketi:

    ```
    npm install azure-iot-device azure-iot-device-amqp --save
    ```

3. Kaustas **simulateddevice** tekstiredaktoris kasutamisel **SimulatedDevice.js** uue faili loomine.

4. Lisage järgmine `require` laused **SimulatedDevice.js** faili alguses:

    ```
    'use strict';

    var clientFromConnectionString = require('azure-iot-device-amqp').clientFromConnectionString;
    var Message = require('azure-iot-device').Message;
    ```

5. Lisage **connectionString** muutuja ja selle seadme kliendi loomiseks kasutada. **{Youriothostname}** asendamine asjade jaoturi nime loodud *loomine soovitud asjade jaoturi* jaotis. Seadme võtmeväärtuse, mis on loodud, klõpsake jaotises *Loo seadme identiteedi* **{yourdevicekey}** asendamiseks tehke järgmist.

    ```
    var connectionString = 'HostName={youriothostname};DeviceId=myFirstNodeDevice;SharedAccessKey={yourdevicekey}';
    
    var client = clientFromConnectionString(connectionString);
    ```

6. Järgmise funktsiooni kuvamiseks väljund rakenduse lisamiseks tehke järgmist.

    ```
    function printResultFor(op) {
      return function printResult(err, res) {
        if (err) console.log(op + ' error: ' + err.toString());
        if (res) console.log(op + ' status: ' + res.constructor.name);
      };
    }
    ```

7. Pakkumist luua ja kasutada funktsiooni **setInterval** uue sõnumi saatmiseks teie asjade jaoturi iga teine:

    ```
    var connectCallback = function (err) {
      if (err) {
        console.log('Could not connect: ' + err);
      } else {
        console.log('Client connected');

        // Create a message and send it to the IoT Hub every second
        setInterval(function(){
            var windSpeed = 10 + (Math.random() * 4);
            var data = JSON.stringify({ deviceId: 'myFirstNodeDevice', windSpeed: windSpeed });
            var message = new Message(data);
            console.log("Sending message: " + message.getData());
            client.sendEvent(message, printResultFor('send'));
        }, 1000);
      }
    };
    ```

8. Avage oma asjade jaoturi ühendus ja hakata sõnumite saatmiseks.

    ```
    client.open(connectCallback);
    ```

9. Salvestage ja sulgege fail **SimulatedDevice.js** .

> [AZURE.NOTE] Selles õpetuses ei rakenda hoida asju lihtne, mis tahes uuesti poliitika. Kood, tuleks rakendada uuesti poliitikate (nt eksponentsiaalse backoff) soovitatud MSDN-i artiklist [Siirdamiseks viga töötlemise][lnk-transient-faults].


## <a name="run-the-applications"></a>Rakenduste käivitamine

Nüüd olete valmis rakendusi käivitada.

1. Käsuviibale- **readdevicetocloudmessages** kausta, käivitage järgmine käsk oma asjade jaoturi jälgimise alustamiseks:

    ```
    node ReadDeviceToCloudMessages.js 
    ```

    ![Node.js asjade jaoturi teenuse klientrakenduse seadme pilve sõnumite jälgimine][7]

2. Käsuviibale- **simulateddevice** kausta, käivitage järgmine käsk alustada oma asjade jaoturi telemeetria andmete saatmine:

    ```
    node SimulatedDevice.js
    ```

    ![Node.js asjade jaoturi seadme klientrakenduse seadme pilve sõnumite saatmiseks][8]

3. [Azure'i portaalis] paani **kasutus** [ lnk-portal] kuvatakse jaoturi saadetud sõnumite arv:

    ![Azure'i portaali kasutamine paani arv asjade jaoturi saadetud sõnumid][43]

## <a name="next-steps"></a>Järgmised sammud

Selles õpetuses konfigureeritud uue asjade jaoturi Azure portaali ja seejärel loodud seadme identiteedi jaoturi identiteedi registri. Selle seadme identiteedi saate kasutada jäljendatud seadmes rakendus jaoturi seadme pilve sõnumeid saata. Saate ka loodud rakendus, mis kuvab jaoturi vastuvõetud sõnumid. 

Alustamine asjade jaoturi jätkamiseks ja teiste stsenaariumide asjade avastamiseks näha:

- [Teie seadme ühendamine][lnk-connect-device]
- [Mobiilsideseadmete halduse töötamise alustamine][lnk-device-management]
- [Asjade lüüsi SDK töötamise alustamine][lnk-gateway-SDK]

Saate teada, kuidas laiendada oma asjade lahenduse ja seadme pilve sõnumite alal, lugege teemat [protsessi seadme pilve sõnumite] [ lnk-process-d2c-tutorial] õpetuse.

<!-- Images. -->
[7]: ./media/iot-hub-node-node-getstarted/runapp1.png
[8]: ./media/iot-hub-node-node-getstarted/runapp2.png
[43]: ./media/iot-hub-csharp-csharp-getstarted/usage.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-devguide-identity]: iot-hub-devguide-identity-registry.md
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-device-management]: iot-hub-device-management-get-started.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
