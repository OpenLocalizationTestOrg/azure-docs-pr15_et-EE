<properties
    pageTitle="Saatke asjade jaoturi cloud-seade | Microsoft Azure'i"
    description="Järgige selles õppetükis saate teada, kuidas Azure'i asjade jaoturi abil Java cloud-seadme sõnumite saatmiseks."
    services="iot-hub"
    documentationCenter="nodejs"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="javascript"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/23/2016"
     ms.author="dobett"/>

# <a name="tutorial-how-to-send-cloud-to-device-messages-with-iot-hub-and-nodejs"></a>Õpetus: Kuidas asjade jaoturi ja Node.js cloud-seadme saatmine

[AZURE.INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

## <a name="introduction"></a>Sissejuhatus

Azure'i asjade jaoturi on täielikult hallatav teenus, mis aitab lubada vahelise miljoneid asjade seadmete turvalist kahesuunalise suhtluse ja rakenduse uuesti lõpetada. Õpetusega [Alustamine asjade jaoturi] näitab, kuidas luua soovitud asjade jaoturi, seadme identiteedi see säte ja kood jäljendatud seade, mis saadab seadme pilve sõnumite.

Selle õpetuse põhineb [asjade jaoturi alustamine]. See näitab teile, kuidas soovite:

- Oma rakenduse pilve tagasi lõppu, cloud-seadme sõnumeid saata ühe seadme asjade keskuse kaudu.
- Pilveteenuse-seadme sõnumeid seadmes.
- Oma rakenduse pilve lõpetamise uuesti, taotleda seadme asjade keskuse kaudu saadetud sõnumite kohaletoimetamise kinnituse (*tagasiside*).

Cloud-seadme sõnumite kohta lisateavet leiate [Asjade jaoturi arendaja juhend][IoT Hub Developer Guide - C2D].

Õppeteema lõpus käivitate kaks Node.js konsooli rakendusi:

* **SimulatedDevice**, muudetud versiooni rakenduse loodud [asjade jaoturi alustamine], mis loob teie asjade jaoturi ja saab pilve-seadme sõnumeid.
* **SendCloudToDeviceMessage**, mis saadab sõnumi cloud-seadme jäljendatud seadme asjade jaoturi abil ja seejärel saab selle kohaletoimetamise kinnituse.

> [AZURE.NOTE] Asjade jaoturi SDK toetab mitme seadme platvormide ja keelte jaoks (sh C, Java ja Javascript) kaudu Azure'i asjade seadme SDK-d. Ühendage seade selles õpetuses kood ning üldiselt Azure'i asjade jaoturi üksikasjalikud juhised leiate teemast [Azure asjade Arenduskeskus].

Selle õpetuse tegemiseks vajate järgmist:

+ Node.js versioon 0.10.x või uuem versioon.

+ Aktiivne Azure'i konto. (Kui teil pole kontot, saate luua [tasuta konto] [ lnk-free-trial] vaid paar minutit.)

## <a name="receive-messages-on-the-simulated-device"></a>Meilisõnumite jäljendatud seadmes

Selles jaotises saate muuta jäljendatud seadme rakenduse loodud [Alustamine asjade jaoturi] cloud-seadme sõnumeid asjade keskuse kaudu.

1. Avage tekstiredaktoris abil SimulatedDevice.js faili.

2. Muutke funktsiooni **connectCallback** reageerimine asjade keskuse kaudu saadetud sõnumid. Selles näites seade alati viitab **täieliku** funktsiooni teatada asjade jaoturi sõnumi töötlemist. Funktsiooni **connectCallback** uue versiooni näeb välja umbes järgmine:

    ```
    var connectCallback = function (err) {
      if (err) {
        console.log('Could not connect: ' + err);
      } else {
        console.log('Client connected');
        client.on('message', function (msg) {
          console.log('Id: ' + msg.messageId + ' Body: ' + msg.data);
          client.complete(msg, printResultFor('completed'));
        });
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

    > [AZURE.NOTE] Kui te kasutate HTTP asemel MQTT või AMQP transport, kontrollib **DeviceClient** eksemplari meilisõnumeid asjade jaoturi harva (vähem kui iga 25 minuti järel). MQTT, AMQP ja HTTP ja asjade jaoturi pidurdamise erinevuste kohta leiate lisateavet teemast [Asjade jaoturi arendaja juhend][IoT Hub Developer Guide - C2D].

## <a name="send-a-cloud-to-device-message"></a>Pilveteenuse-seadme sõnumi saatmine

Selles jaotises saate luua Node.js konsooli rakendus, mis saadab cloud-seadme sõnumite jäljendatud seadmes rakendus. Peate seadme Id seadme [Alustamine asjade jaoturi] õpetuses lisasite. Samuti vajate ühendusstringi oma asjade jaoturi, mille leiate [Azure'i portaalis].

1. Looge tühi kaust nimega **sendcloudtodevicemessage**. Klõpsake kaustas **sendcloudtodevicemessage** abil oma käsuviibas järgmine käsk package.json faili loomine. Kõik vaikesätted aktsepteerida:

    ```
    npm init
    ```

2. Veebisaidil oma käsuviiba **sendcloudtodevicemessage** kausta, installige **Azure'i-iothub** pakett järgmine käsk:

    ```
    npm install azure-iothub --save
    ```

3. Kaustas **sendcloudtodevicemessage** tekstiredaktoris kasutamisel **SendCloudToDeviceMessage.js** uue faili loomine.

4. Lisage järgmine `require` laused **SendCloudToDeviceMessage.js** faili alguses:

    ```
    'use strict';
    
    var Client = require('azure-iothub').Client;
    var Message = require('azure-iot-common').Message;
    ```

5. Järgmine kood **SendCloudToDeviceMessage.js** faili lisada. Asendage ühenduse stringiväärtus kohatäite jaoks loodud õpetusega [Alustamine asjade jaoturi] asjade jaoturi ühendusstring. [Alustamine asjade jaoturi] õpetuses lisasite seade seadme ID-d target seadme kohatäite asendamiseks tehke järgmist.

    ```
    var connectionString = '{iot hub connection string}';
    var targetDevice = '{device id}';

    var serviceClient = Client.fromConnectionString(connectionString);
    ```

6. Järgmise toimingu tulemused konsooli printimiseks funktsiooni lisamiseks tehke järgmist.

    ```
    function printResultFor(op) {
      return function printResult(err, res) {
        if (err) console.log(op + ' error: ' + err.toString());
        if (res) console.log(op + ' status: ' + res.constructor.name);
      };
    }
    ```

7. Järgmise funktsiooni printimiseks tagasiside sõnumite kohaletoimetamise konsooli lisamiseks tehke järgmist.

    ```
    function receiveFeedback(err, receiver){
      receiver.on('message', function (msg) {
        console.log('Feedback message:')
        console.log(msg.getData().toString('utf-8'));
      });
    }
    ```

8. Sõnumi saatmine seadmesse ja käsitlemiseks tagasiside sõnumi, kui seadme tunnustab cloud-seadme sõnumi järgmine kood lisamiseks tehke järgmist.

    ```
    serviceClient.open(function (err) {
      if (err) {
        console.error('Could not connect: ' + err.message);
      } else {
        console.log('Service client connected');
        serviceClient.getFeedbackReceiver(receiveFeedback);
        var message = new Message('Cloud to device message.');
        message.ack = 'full';
        message.messageId = "My Message ID";
        console.log('Sending message: ' + message.getData());
        serviceClient.send(targetDevice, message, printResultFor('send'));
      }
    });
    ```

7. Salvestage ja sulgege **SendCloudToDeviceMessage.js** fail.

## <a name="run-the-applications"></a>Rakenduste käivitamine

Nüüd olete valmis rakendusi käivitada.

1. Käsuviibale- **simulateddevice** kausta, käivitage järgmine käsk asjade jaoturi telemeetria saatmiseks ja kuulata cloud-seadme sõnumeid:

    ```
    node SimulatedDevice.js 
    ```

    ![Käivitage rakendus jäljendatud seade][img-simulated-device]

2. Käsuviibale- **sendcloudtodevicemessage** kausta, käivitage järgmine käsk cloud-seadme sõnumi saatmine ja oodake, kuni kinnitus tagasiside:

    ```
    node SendCloudToDeviceMessage.js 
    ```

    ![Käivitage rakendus, et c2d käsk Saada][img-send-command]

    > [AZURE.NOTE] Selles õpetuses ei rakenda lihtsuse huvides, mis tahes uuesti poliitika. Kood, tuleks rakendate uuesti poliitikate (nt eksponentsiaalse backoff) soovitatud [Siirdamiseks viga töötlemise]MSDN-i artiklist.

## <a name="next-steps"></a>Järgmised sammud

Selles õppetükis saate õppinud, kuidas pilve-seadme sõnumite saatmiseks ja vastuvõtmiseks. 

Täieliku lõpuni lahendusi, mis kasutavad asjade jaoturi näiteid, leiate teemast [Azure asjade komplekti].

Asjade jaoturi lahendusi arendamise kohta leiate lisateavet teemast [Asjade jaoturi arendaja juhend].

<!-- Images -->
[img-simulated-device]: media/iot-hub-node-node-c2d/receivec2d.png
[img-send-command]:  media/iot-hub-node-node-c2d/sendc2d.png

<!-- Links -->

[Asjade jaoturi kasutamise alustamine]: iot-hub-node-node-getstarted.md
[IoT Hub Developer Guide - C2D]: iot-hub-devguide-messaging.md
[Asjade jaoturi arendaja juhend]: iot-hub-devguide.md
[Azure'i asjade Arenduskeskus]: http://www.azure.com/develop/iot
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md
[Siirdamiseks viga töötlemine]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Azure'i portaal]: https://portal.azure.com
[Azure'i asjade komplekti]: https://azure.microsoft.com/documentation/suites/iot-suite/