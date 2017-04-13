<properties
 pageTitle="Kuidas teha püsivara värskendus | Microsoft Azure'i"
 description="Selle õpetuse näidatakse, kuidas teha püsivara värskendus"
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

# <a name="tutorial-how-to-do-a-firmware-update-preview"></a>Õpetus: Kuidas püsivara värskendus (eelvaade)

## <a name="introduction"></a>Sissejuhatus
Rakenduses [Alustamine mobiilsideseadmete halduse] [ lnk-dm-getstarted] kuueosalisest kuvatud, kuidas kasutada [seadme Double] [ lnk-devtwin] ja [pilveteenuse-seade (C2D) meetodite] [ lnk-c2dmethod] primitiivid kaugühenduse teel seadme taaskäivitamist. Selle õpetuse kasutab sama asjade jaoturi primitiivid ja antakse juhiseid ja näidatakse, kuidas teha lõpuni jäljendatud püsivara värskendust.  See muster kasutatakse Inteli Edison seadme valimi püsivara värskenduse rakendamist.

Selles õppetükis saate teada, kuidas soovite:

- Looge konsooli rakendus, mis nõuab firmwareUpdate otsene meetod jäljendatud seadmes oma asjade jaoturi kaudu.
- Looge jäljendatud seade, mis rakendab firmwareUpdate otsene meetod, mis läheb läbi mitmeetapiline protsess, mis ootab püsivara pildi allalaadimise, allalaadimist püsivara pilt ja lõpuks kehtib nda püsivara pilt.  Kogu käivitamisel iga etapi seadme kasutab, kusjuures teatatud atribuutide värskendamine edu.

Õppeteema lõpus on teil kaks Node.js konsooli rakendusi.

**dmpatterns_fwupdate_service.js**, mis nõuab otsene jäljendatud seadmes, kuvab vastus ja perioodiliselt (iga 500 ms) kuvatakse värskendatud seadme Double teatatud atribuudid.

**dmpatterns_fwupdate_device.js**, mis loob teie asjade jaoturi varem loodud seadme identiteediga, saab firmwareUpdate otsene meetod, läbi mitme riigi protsessi simuleerida püsivara värskendus sh töötab: pildi allalaadimise ootel, uue pildi allalaadimine ja lõpuks rakendamise pilt.


Selle õpetuse tegemiseks vajate järgmist:

Node.js versioon 0.12.x või uuem versioon <br/>  [Ettevalmistused oma arenduskeskkond] [ lnk-dev-setup] kirjeldatakse, kuidas installida Node.js selles õpetuses Linux või Windows.

Aktiivne Azure'i konto. (Kui teil pole kontot, saate luua [tasuta konto] [ lnk-free-trial] vaid paar minutit.)

Järgige oma asjade keskuse loomiseks ja teie ühendusstringi toomiseks [Alustamine mobiilsideseadmete halduse](iot-hub-device-management-get-started.md) artiklis.

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-a-simulated-device-app"></a>Jäljendatud seadme rakenduse loomine

Selles jaotises Node.js konsooli rakendus, mis vastab otsese meetodi kutsunud pilves, mis käivitab jäljendatud seadme püsivara värskendus loomine ja kasutab seadme Double teatatud lubamiseks seadme Double päringute tuvastamiseks seadmed ja millal nad viimati rebooted atribuudid.

1. Looge uus tühi kaust nimega **manageddevice**.  Klõpsake kaustas **manageddevice** abil oma käsuviibas järgmine käsk package.json faili loomine.  Kõik vaikesätted aktsepteerida:

    ```
    npm init
    ```
    
2. Veebisaidil oma käsuviiba **manageddevice** kausta, käivitage järgmine käsk installimiseks on **azure-iot-device@dtpreview** seadme SDK paketi ja **azure-iot-device-mqtt@dtpreview** paketi:

    ```
    npm install azure-iot-device@dtpreview azure-iot-device-mqtt@dtpreview --save
    ```

3. Kaustas **manageddevice** tekstiredaktoris kasutamisel **dmpatterns_fwupdate_device.js** uue faili loomine.

4. Lisage järgmine "nõuda" laused **dmpatterns_fwupdate_device.js** faili alguses.

    ```
    'use strict';

    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```

5. Lisage **connectionString** muutuja ja selle seadme kliendi loomiseks kasutada.  

    ```
    var connectionString = 'HostName={youriothostname};DeviceId=myDeviceId;SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

6. Lisage järgmine funktsioon, mida kasutatakse Double värskendamiseks teatatud atribuudid

    ```
    var reportFWUpdateThroughTwin = function(twin, firmwareUpdateValue) {
      var patch = {
          iothubDM : {
            firmwareUpdate : firmwareUpdateValue
          }
      };

      twin.properties.reported.update(patch, function(err) {
        if (err) throw err;
        console.log('twin state reported')
      });
    };
    ```
    
7. Järgmised funktsioonid, mis simuleerida allalaadimine ja rakendada püsivara pildi lisada.

    ```
    var simulateDownloadImage = function(imageUrl, callback) {
      var error = null;
      var image = "[fake image data]";
      
      console.log("Downloading image from " + imageUrl);
      
      callback(error, image);
    }

    var simulateApplyImage = function(imageData, callback) {
      var error = null;
      
      if (!imageData) {
        error = {message: 'Apply image failed because of missing image data.'};
      }
      
      callback(error);
    }
    ```

8. Lisage järgmine funktsioon, mis värskendab püsivara värskendamise olek – kusjuures teatada atribuudid allalaadimise ootel.  Tavaliselt seadmete teavitatakse värskendus saadaval ja administraatori määratletud poliitika põhjused seadme allalaadimise ja rakendamise Värskenda.  See on, kui poliitika lubamiseks loogika läheks.  Lihtsa, me edasi lükata 4 sekundit ja sellistesse püsivara pildi alla laadida. 

    ```
    var waitToDownload = function(twin, fwPackageUriVal, callback) {
      var now = new Date();
      
      reportFWUpdateThroughTwin(twin, {
        fwPackageUri: fwPackageUriVal,
        status: 'waiting',
        error : null,
        startedWaitingTime : now.toISOString()
      });
      setTimeout(callback, 4000);
    };
    ```
    
9. Lisage järgmine funktsioon, mis värskendab püsivara värskendamise olek – kusjuures teatada atribuudid püsivara pildi allalaadimine.  See järgneb jäljendamine püsivara allalaadimine ja lõpuks värskendab püsivara värskendamise olek teavitada allalaadimine edu või tõrge.

    ```
    var downloadImage = function(twin, fwPackageUriVal, callback) {
      var now = new Date();   
      
      reportFWUpdateThroughTwin(twin, {
        status: 'downloading',
      });
      
      setTimeout(function() {
        // Simulate download
        simulateDownloadImage(fwPackageUriVal, function(err, image) {
          
          if (err)
          {
            reportFWUpdateThroughTwin(twin, {
              status: 'downloadfailed',
              error: {
                code: error_code,
                message: error_message,
              }
            });
          }
          else {        
            reportFWUpdateThroughTwin(twin, {
              status: 'downloadComplete',
              downloadCompleteTime: now.toISOString(),
            });
            
            setTimeout(function() { callback(image); }, 4000);   
          }
        });
        
      }, 4000);
    }
    ```
    
10. Lisage järgmine funktsioon, mis värskendab püsivara värskendamise olek – kusjuures teatada atribuudid, rakendades püsivara pilt.  See järgib jäljendamine rakendamine püsivara pilt ja lõpuks värskendab püsivara värskendamise olek Rakenda õnnestumise või nurjumise teavitada.

    ```
    var applyImage = function(twin, imageData, callback) {
      var now = new Date();   
      
      reportFWUpdateThroughTwin(twin, {
        status: 'applying',
        startedApplyingImage : now.toISOString()
      });
      
      setTimeout(function() {
        
        // Simulate apply firmware image
        simulateApplyImage(imageData, function(err) {
          if (err) {
            reportFWUpdateThroughTwin(twin, {
              status: 'applyFailed',
              error: {
                code: err.error_code,
                message: err.error_message,
              }
            });
          } else { 
            reportFWUpdateThroughTwin(twin, {
              status: 'applyComplete',
              lastFirmwareUpdate: now.toISOString()
            });    
            
          }
        });
        
        setTimeout(callback, 4000);
        
      }, 4000);
    }
    ```

11. Lisage järgmised functoin, mis toime firmwareUpdate meetodit ja mitmeetapiline püsivara värskendamise protsessi algatada.

    ```
    var onFirmwareUpdate = function(request, response) {
            
      // Respond the cloud app for the direct method
      response.send(200, 'FirmwareUpdate started', function(err) {
        if (!err) {
          console.error('An error occured when sending a method response:\n' + err.toString());
        } else {
          console.log('Response to method \'' + request.methodName + '\' sent successfully.');
        }
      });

      // Get the parameter from the body of the method request
      var fwPackageUri = JSON.parse(request.payload).fwPackageUri;

      // Obtain the device twin
      client.getTwin(function(err, twin) {
        if (err) {
          console.error('Could not get device twin.');
        } else {
          console.log('Device twin acquired.');
          
          // Start the multi-stage firmware update
          waitToDownload(twin, fwPackageUri, function() {
            downloadImage(twin, fwPackageUri, function(imageData) {
              applyImage(twin, imageData, function() {});    
            });  
          });

        }
      });
    }
    ```
    
12. Lõpetuseks, lisage järgmine kood, mis loob ühenduse asjade jaoturi seade, 

    ```
    client.open(function(err) {
      if (err) {
        console.error('Could not connect to IotHub client');
      }  else {
        console.log('Client connected to IoT Hub.  Waiting for firmwareUpdate direct method.');
      }
      
      client.onDeviceMethod('firmwareUpdate', onFirmwareUpdate(request, response));
    });
    ```

> [AZURE.NOTE] Selles õpetuses ei rakenda hoida asju lihtne, mis tahes uuesti poliitika. Kood, tuleks rakendada uuesti poliitikate (nt eksponentsiaalse backoff) soovitatud MSDN-i artiklist [Siirdamiseks viga töötlemise][lnk-transient-faults].

## <a name="trigger-a-remote-firmware-update-on-the-device-using-a-direct-method"></a>Käivitab remote püsivara värskenduse otsene kasutamine seadmes 

Selles jaotises saate luua Node.js konsooli rakendus, mis käivitab remote püsivara värskendus otsene kasutamine seadmes ja kasutab seadme Double päringute perioodiliselt saada aktiivse püsivara värskendus olekut seadmes.


1. Looge uus tühi kaust nimega **triggerfwupdateondevice**.  Klõpsake kaustas **triggerfwupdateondevice** abil oma käsuviibas järgmine käsk package.json faili loomine.  Kõik vaikesätted aktsepteerida:

    ```
    npm init
    ```
    
2. Veebisaidil oma käsuviiba **triggerfwupdateondevice** kausta, käivitage järgmine käsk installida **azure-iothub** seadme SDK paketi ja **azure asjade Interneti-seadme mqtt** paketi:

    ```
    npm install azure-iot-hub@dtpreview --save
    ```
    
3. Kaustas **triggerfwupdateondevice** tekstiredaktoris kasutamisel **dmpatterns_getstarted_service.js** uue faili loomine.

4. Lisage järgmine "nõuda" laused **dmpatterns_getstarted_service.js** faili alguses.

    ```
    'use strict';

    var Registry = require('azure-iothub').Registry;
    var Client = require('azure-iothub').Client;
    ```

5. Järgmised muutuv deklaratsiooni lisamine ja kohatäite väärtuste asendamine.

    ```
    var connectionString = '{device_connectionstring}';
    var registry = Registry.fromConnectionString(connectionString);
    var client = Client.fromConnectionString(connectionString);
    var deviceToUpdate = 'myDeviceId';
    ```
    
6. Lisage järgmine funktsioon leidmiseks ja Kuva väärtus on firmwareUpdate teatatud atribuudi.

    ```
    var queryTwinFWUpdateReported = function() {
        registry.getTwin(deviceToUpdate, function(err, twin){
            if (err) {
              console.error('Could not query twins: ' + err.constructor.name + ': ' + err.message);
            } else {
              console.log((JSON.stringify(twin.properties.reported.iothubDM.firmwareUpdate)) + "\n");
            }
        });
    };
    ```

7. Taaskäivitage seade target firmwareUpdate meetodit kasutada järgmist funktsiooni lisamiseks tehke järgmist.

    ```
    var startFirmwareUpdateDevice = function() {
      var params = {
          fwPackageUri: 'https://secureurl'
      };
      
      var methodName = "firmwareUpdate";
      var payloadData =  JSON.stringify(params);
      
      var methodParams = {
        methodName: methodName,
        payload: payloadData,
        timeoutInSeconds: 30
      };
      
      client.invokeDeviceMethod(deviceToUpdate, methodParams, function(err, result) {
        if (err) {
          console.error('Could not start the firmware update on the device: ' + err.message)
        } 
      });
    };
    ```

8. Lõpuks lisa järgmise funktsiooni koodi käivitamiseks püsivara jada värskendada, ja alustage perioodiliselt näitab, kusjuures teatatud atribuudid:

    ```
    startFirmwareUpdateDevice();
    setInterval(queryTwinFWUpdateReported, 500);
    ```
    
9. Salvestage ja sulgege fail **dmpatterns_fwupdate_service.js** .

## <a name="run-the-applications"></a>Rakenduste käivitamine

Nüüd olete valmis rakendusi käivitada.

1. Käsuviibale- **manageddevice** kausta, käivitage järgmine käsk kuulama taaskäivitage arvuti otsene meetod.

    ```
    node dmpatterns_fwupdate_device.js
    ```

2. Käsuviibale- **triggerfwupdateondevice** kausta, käivitage järgmine käsk mis käivitab remote taaskäivitage arvuti ja seadme kahe viimase leidmiseks päringu taaskäivitage aeg.

    ```
    node dmpatterns_fwupdate_service.js
    ```

3. Kuvatakse sõnumi välja printida reageerida otsene meetod

## <a name="next-steps"></a>Järgmised sammud

Selles õpetuses kasutasite otsene remote püsivara värskendus seadmes käivitamiseks ja perioodiliselt kasutatud kusjuures teatatud atribuudid mõista edenemise püsivara värskendada protsess.  

Saate teada, kuidas laiendada oma asjade lahenduse ja ajakava nõuab mitmes seadmes, leiate [ajakava ja leviedastuse töökohtade] [ lnk-tutorial-jobs] õpetuse.

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-dm-getstarted]: iot-hub-device-management-get-started.md
[lnk-tutorial-jobs]: iot-hub-schedule-jobs.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
