<properties
 pageTitle="Kuidas plaanida töö | Microsoft Azure'i"
 description="Selle õpetuse näidatakse, kuidas plaanida tööde haldamine"
 services="iot-hub"
 documentationCenter=".net"
 authors="juanjperez"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="juanpere"/>

# <a name="tutorial-schedule-and-broadcast-jobs-preview"></a>Õpetus: Plaanimine ja leviedastus tööde haldamine (eelvaade)

## <a name="introduction"></a>Sissejuhatus
Azure'i asjade jaoturi on täielikult hallatav teenus, mis võimaldab rakenduse tagasi end luua ja jälgida plaanimine ja värskendada miljoneid seadmete tööd.  Töö saab kasutada järgmisi toiminguid:

- Seadme Double soovitud atribuutide värskendamine
- Seadme Double siltide värskendamine
- Autonoomsest cloud-seadme meetodid

Põhimõtteliselt töö murtakse ühte neid toiminguid ja rajad kogumi seadmetes, mis on määratletud Double päringu täitmise edenemist.  Näiteks töö abil rakenduse tagasi lõpetamist saate autonoomsest 10 000 seadmetes määratud Double päringu ja tulevaste ajastatud meetodi taaskäivitage arvuti.  Selle rakenduse saab siis jälgimiseks nagu kõik need vastu ja taaskäivitage arvuti meetodit käivitada.

Lugege lisateavet kõigi nende funktsioonidele artiklitest:

- Seadme kahe- ja atribuudid: [Alustamine Double] [ lnk-get-started-twin] ja [õpetus: kasutamine Double atribuudid][lnk-twin-props]
- Pilveteenuse-seadme meetodid: [arendaja juhend - otsese meetodid] [ lnk-dev-methods] ja [õpetus: C2D meetodite][lnk-c2d-methods]

Selles õppetükis saate teada, kuidas soovite:

- Looge jäljendatud seade, millel on otsene meetod, mis võimaldab lockDoor, mis saab kutsuda rakendus uuesti end.
- Looge konsooli rakendus, mis nõuab lockDoor otsene meetod jäljendatud seadme töö abil ja värskendab seadme töö abil Double soovitud atribuudid.

Õppeteema lõpus on teil kaks Node.js konsooli rakendusi.

**simDevice.js**, mis loob ühenduse oma asjade jaoturi seadme identiteedi ja võtab vastu lockDoor otsene meetod.

**scheduleJobService.js**, mis nõuab otsene jäljendatud seadmes ja töö abil soovitud Double disired atribuutide värskendamine.

Selle õpetuse tegemiseks vajate järgmist:

Node.js versioon 0.12.x või uuem versioon <br/>  [Ettevalmistused oma arenduskeskkond] [ lnk-dev-setup] kirjeldatakse, kuidas installida Node.js selles õpetuses Linux või Windows.

Aktiivne Azure'i konto. (Kui teil pole kontot, saate luua [tasuta konto] [ lnk-free-trial] vaid paar minutit.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-a-simulated-device-app"></a>Jäljendatud seadme rakenduse loomine

Selles jaotises Node.js konsooli rakendus, mis vastab otsese meetodi kutsunud pilves, mis käivitab jäljendatud seadme taaskäivitamist loomine ja kasutab seadme Double teatatud lubamiseks seadme Double päringute tuvastamiseks seadmed ja millal nad viimati rebooted atribuudid.

1. Looge uus tühi kaust nimega **simDevice**.  Klõpsake kaustas **simDevice** abil oma käsuviibas järgmine käsk package.json faili loomine.  Kõik vaikesätted aktsepteerida:

    ```
    npm init
    ```
    
2. Veebisaidil oma käsuviiba **simDevice** kausta, käivitage järgmine käsk installida **azure-asjade Interneti-seade** seadme SDK paketi ja **azure asjade Interneti-seadme mqtt** paketi:

    ```
    npm install azure-iot-device@dtpreview azure-iot-device-mqtt@dtpreview --save
    ```

3. Kaustas **simDevice** tekstiredaktoris kasutamisel **simDevice.js** uue faili loomine.

4. Lisage järgmine "nõuda" laused **simDevice.js** faili alguses.

    ```
    'use strict';

    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```

5. Lisage **connectionString** muutuja ja selle seadme kliendi loomiseks kasutada.  

    ```
    var connectionString = 'HostName={youriothostname};DeviceId={yourdeviceid};SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

6. Lisage järgmine funktsioon käsitlema lockDoor meetod.

    ```
    var onLockDoor = function(request, response) {
        
        // Respond the cloud app for the direct method
        response.send(200, function(err) {
            if (!err) {
                console.error('An error occured when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.');
            }
        });
        
        console.log('Locking Door!');
    };
    ```

7. Lisage järgmine kood sündmuseohjuri lockDoor meetodi registreerimiseks.

    ```
    client.open(function(err) {
        if (err) {
            console.error('Could not connect to IotHub client.');
        }  else {
            console.log('Client connected to IoT Hub.  Waiting for reboot direct method.');
            client.onDeviceMethod('lockDoor', onLockDoor);
        }
    });
    ```
    
8. Salvestage ja sulgege fail **simDevice.js** .

> [AZURE.NOTE] Selles õpetuses ei rakenda hoida asju lihtne, mis tahes uuesti poliitika. Kood, tuleks rakendada uuesti poliitikate (nt eksponentsiaalse backoff) soovitatud MSDN-i artiklist [Siirdamiseks viga töötlemise][lnk-transient-faults].

## <a name="schedule-jobs-for-calling-a-direct-method-and-updating-a-twins-properties"></a>Plaanimine tööd otsese meetodi ja uuendamiseks on Double atribuudid

Selles jaotises luua Node.js konsooli rakendus, mis käivitab remote lockDoor otsene kasutamine seadmes ja selle Double atribuutide värskendamine.

1. Looge uus tühi kaust nimega **scheduleJobService**.  Klõpsake kaustas **scheduleJobService** abil oma käsuviibas järgmine käsk package.json faili loomine.  Kõik vaikesätted aktsepteerida:

    ```
    npm init
    ```
    
2. Veebisaidil oma käsuviiba **scheduleJobService** kausta, käivitage järgmine käsk installida **azure-iothub** seadme SDK paketi ja **azure asjade Interneti-seadme mqtt** paketi:

    ```
    npm install azure-iothub@dtpreview uuid --save
    ```
    
3. Kaustas **scheduleJobService** tekstiredaktoris kasutamisel **scheduleJobService.js** uue faili loomine.

4. Lisage järgmine "nõuda" laused **dmpatterns_gscheduleJobServiceetstarted_service.js** faili alguses.

    ```
    'use strict';

    var uuid = require('uuid');
    var JobClient = require('azure-iothub').JobClient;
    ```

5. Järgmised muutuv deklaratsiooni lisamine ja kohatäite väärtuste asendamine.

    ```
    var connectionString = '{iothubconnectionstring}';
    var deviceArray = ['myDeviceId'];
    var startTime = new Date();
    var maxExecutionTimeInSeconds =  3600;
    var jobClient = JobClient.fromConnectionString(connectionString);
    ```
    
6. Lisage järgmised funktsiooni kasutatakse töö täitmise jälgimiseks.

    ```
    function monitorJob (jobId, callback) {
        var jobMonitorInterval = setInterval(function() {
            jobClient.getJob(jobId, function(err, result) {
            if (err) {
                console.error('Could not get job status: ' + err.message);
            } else {
                console.log('Job: ' + jobId + ' - status: ' + result.status);
                if (result.status === 'completed' || result.status === 'failed' || result.status === 'cancelled') {
                clearInterval(jobMonitorInterval);
                callback(null, result);
                }
            }
            });
        }, 5000);
    }
    ```

7. Lisage järgmine kood plaanida töö, mis nõuab seadme meetod.

    ```
    var methodParams = {
        methodName: 'lockDoor',
        payload: null,
        timeoutInSeconds: 45
    };

    var methodJobId = uuid.v4();
    console.log('scheduling Device Method job with id: ' + methodJobId);
    jobClient.scheduleDeviceMethod(methodJobId,
                                deviceArray,
                                methodParams,
                                startTime,
                                maxExecutionTimeInSeconds,
                                function(err) {
        if (err) {
            console.error('Could not schedule device method job: ' + err.message);
        } else {
            monitorJob(methodJobId, function(err, result) {
                if (err) {
                    console.error('Could not monitor device method job: ' + err.message);
                } else {
                    console.log(JSON.stringify(result, null, 2));
                }
            });
        }
    });
    ```
        
8. Järgmine kood plaanida töö, kusjuures värskendamiseks lisamiseks tehke järgmist.

    ```
    var twinPatch = {
        etag: '*',
        desired: {
            building: '43',
            floor: 3
        }
    };

    var twinJobId = uuid.v4();

    console.log('scheduling Twin Update job with id: ' + twinJobId);
    jobClient.scheduleTwinUpdate(twinJobId,
                                deviceArray,
                                twinPatch,
                                startTime,
                                maxExecutionTimeInSeconds,
                                function(err) {
        if (err) {
            console.error('Could not schedule twin update job: ' + err.message);
        } else {
            monitorJob(twinJobId, function(err, result) {
                if (err) {
                    console.error('Could not monitor twin update job: ' + err.message);
                } else {
                    console.log(JSON.stringify(result, null, 2));
                }
            });
        }
    });
    ```
    
9. Salvestage ja sulgege fail **scheduleJobService.js** .

## <a name="run-the-applications"></a>Rakenduste käivitamine

Nüüd olete valmis rakendusi käivitada.

1. Käsuviibale- **simDevice** kausta, käivitage järgmine käsk kuulama taaskäivitage arvuti otsene meetod.

    ```
    node simDevice.js
    ```

2. Käsuviibale- **scheduleJobService** kausta, käivitage järgmine käsk mis käivitab remote taaskäivitage arvuti ja seadme kahe viimase leidmiseks päringu taaskäivitage aeg.

    ```
    node scheduleJobService.js
    ```

3. Kuvatakse nii seadmest ja tagasi end rakenduste väljund.


## <a name="next-steps"></a>Järgmised sammud

Selles õpetuses kasutasite otsene meetod seadmega ja seadme Double atribuudid Värskenda ajakava töö.

Alustamine asjade jaoturi ja seadme halduse mustrid nagu serveri üle air püsivara värskendus jätkamiseks järgmistest teemadest.

[Õpetus: Kuidas püsivara värskendus][lnk-fwupdate]

Jätkamiseks alustamine asjade jaoturi, vaadake teemat [Alustamine asjade lüüsi SDK][lnk-gateway-SDK].

[lnk-get-started-twin]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-props]: iot-hub-node-node-twin-how-to-configure.md
[lnk-c2d-methods]: iot-hub-c2d-methods.md
[lnk-dev-methods]: iot-hub-devguide-direct-methods.md
[lnk-fwupdate]: iot-hub-firmware-update.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx