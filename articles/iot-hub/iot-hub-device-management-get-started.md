<properties
 pageTitle="Alustamine mobiilsideseadmete halduse | Microsoft Azure'i"
 description="Selle õpetuse näidatakse, kuidas alustada mobiilsideseadmete halduse Azure'i asjade keskuse kohta"
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

# <a name="tutorial-get-started-with-device-management-preview"></a>Õpetus: Alustamine mobiilsideseadmete halduse (eelvaade)

## <a name="introduction"></a>Sissejuhatus
Asjade pilv rakendusi abil saate primitiivid Azure'i asjade keskuses seadme Double ja otsese meetodid, eemalt käivitada ja jälgida seadme haldamise toiminguid seadmetes.  Sellest artiklist leiate juhised ja koodi kuidas asjade pilveteenuse rakenduse ja seadme töötavad koos, et alustada ja jälgida seadme taaskäivitamist asjade jaoturi abil.

Kaugühenduse teel käivitamine ja jälgida seadme haldamise toiminguid teie seadmetes pilvepõhist tagaandmebaas rakenduse kaudu, kasutage Azure'i asjade jaoturi primitiivid nt [seadme Double] [ lnk-devtwin] ja [pilveteenuse-seade (C2D) meetodite][lnk-c2dmethod]. Selle õpetuse näete, kuidas tagaandmebaas rakenduse ja seadme saate koos töötada selleks, et saaksite algatada ja jälgida seadme taaskäivitamist asjade keskuse kaudu.

C2D meetodit saate kasutada seadme haldamise toiminguid (nt taaskäivitage arvuti, factory lähtestamine ja püsivara värskendus) pilves tagaandmebaas rakenduse kaudu. Seadme vastutab:

- Töötlemise meetod taotluse saatnud asjade keskuse kaudu.
- Algatamine vastava seadme teatud toimingu seadmes.
- Asjade jaoturi esitada olekuvärskendusi Double seadme kaudu atribuudid teatada.

Pilveteenuses tagaandmebaas rakenduse abil saate käivitada seadme Double päringute teatada edenemise oma seadme haldamise toimingute kohta.

Selles õppetükis saate teada, kuidas soovite:

- Azure portaali abil saate luua ka asjade jaoturi ja luua oma asjade keskuses seadme identiteedi.
- Looge jäljendatud seade, millel on otsene meetod, mis võimaldab taaskäivitage arvuti, mille saab kutsuda pilve.
- Looge konsooli rakendus, mis nõuab taaskäivitamist otsene meetod jäljendatud seadmes oma asjade jaoturi kaudu.

Õppeteema lõpus on teil kaks Node.js konsooli rakendusi.

**dmpatterns_getstarted_device.js**, mis loob teie asjade jaoturi varem loodud seadme identiteediga, taaskäivitage arvuti otsene meetod saab, jäljendab füüsilise taaskäivitage arvuti ja aruannete kestuse viimase taaskäivitage arvuti.

**dmpatterns_getstarted_service.js**, mis nõuab otsene jäljendatud seadmes, kuvab vastus ja kuvab värskendatud seadme Double teatatud atribuudid.

Selle õpetuse tegemiseks vajate järgmist:

Node.js versioon 0.12.x või uuem versioon <br/>  [Ettevalmistused oma arenduskeskkond] [ lnk-dev-setup] kirjeldatakse, kuidas installida Node.js selles õpetuses Linux või Windows.

Aktiivne Azure'i konto. (Kui teil pole kontot, saate luua [tasuta konto] [ lnk-free-trial] vaid paar minutit.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-a-simulated-device-app"></a>Jäljendatud seadme rakenduse loomine

Selles jaotises Node.js konsooli rakendus, mis vastab otsese meetodi kutsunud pilves, mis käivitab jäljendatud seadme taaskäivitamist loomine ja kasutab seadme Double teatatud lubamiseks seadme Double päringute tuvastamiseks seadmed ja millal nad viimati rebooted atribuudid.

1. Looge uus tühi kaust nimega **manageddevice**.  Klõpsake kaustas **manageddevice** abil oma käsuviibas järgmine käsk package.json faili loomine.  Kõik vaikesätted aktsepteerida:

    ```
    npm init
    ```
    
2. Veebisaidil oma käsuviiba **manageddevice** kausta, käivitage järgmine käsk installimiseks on **azure-iot-device@dtpreview** seadme SDK paketi ja **azure-iot-device-mqtt@dtpreview** paketi:

    ```
    npm install azure-iot-device@dtpreview azure-iot-device-mqtt@dtpreview --save
    ```

3. Kaustas **manageddevice** tekstiredaktoris kasutamisel **dmpatterns_getstarted_device.js** uue faili loomine.

4. Lisage järgmine "nõuda" laused **dmpatterns_getstarted_device.js** faili alguses.

    ```
    'use strict';

    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```

5. Lisage **connectionString** muutuja ja selle seadme kliendi loomiseks kasutada.  Ühendusstringi asendada oma seadme ühendusstring.  

    ```
    var connectionString = 'HostName={youriothostname};DeviceId=myDeviceId;SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

6. Lisage järgmine funktsioon rakendada otsene meetod seadmes

    ```
    var onReboot = function(request, response) {
        
        // Respond the cloud app for the direct method
        response.send(200, 'Reboot started', function(err) {
            if (!err) {
                console.error('An error occured when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.');
            }
        });
        
        // Report the reboot before the physical restart
        var date = new Date();
        var patch = {
            iothubDM : {
                reboot : {
                    lastReboot : date.toISOString(),
                }
            }
        };

        // Get device Twin
        client.getTwin(function(err, twin) {
            if (err) {
                console.error('could not get twin');
            } else {
                console.log('twin acquired');
                twin.properties.reported.update(patch, function(err) {
                    if (err) throw err;
                    console.log('Device reboot twin state reported')
                });  
            }
        });
        
        // Add your device's reboot API for physical restart.
        console.log('Rebooting!');
    };
    ```

7. Avage oma asjade jaoturi ühenduse ja hakata kuulajale otsene meetod.

    ```
    client.open(function(err) {
        if (err) {
            console.error('Could not open IotHub client');
        }  else {
            console.log('Client opened.  Waiting for reboot method.');
            client.onDeviceMethod('reboot', onReboot);
        }
    });
    ```
    
8. Salvestage ja sulgege fail **dmpatterns_getstarted_device.js** .

 [AZURE.NOTE] Selles õpetuses ei rakenda hoida asju lihtne, mis tahes uuesti poliitika. Kood, tuleks rakendada uuesti poliitikate (nt eksponentsiaalse backoff) soovitatud MSDN-i artiklist [Siirdamiseks viga töötlemise][lnk-transient-faults].

## <a name="trigger-a-remote-reboot-on-the-device-using-a-direct-method"></a>Päästik remote uuesti otsene kasutamine seadmes 

Selles jaotises saate luua Node.js konsooli rakendus, mis käivitab remote taaskäivitage seade otse meetodil ja kasutab seadme Double päringute seadme taaskäivitamist viimast otsimiseks.

1. Looge uus tühi kaust nimega **triggerrebootondevice**.  Klõpsake kaustas **triggerrebootondevice** abil oma käsuviibas järgmine käsk package.json faili loomine.  Kõik vaikesätted aktsepteerida:

    ```
    npm init
    ```
    
2. Veebisaidil oma käsuviiba **triggerrebootondevice** kausta, käivitage järgmine käsk installimiseks on **azure-iothub@dtpreview** seadme SDK paketi ja **azure-iot-device-mqtt@dtpreview** paketi:

    ```
    npm install azure-iothub@dtpreview --save
    ```
    
3. Kaustas **triggerrebootondevice** tekstiredaktoris kasutamisel **dmpatterns_getstarted_service.js** uue faili loomine.

4. Lisage järgmine "nõuda" laused **dmpatterns_getstarted_service.js** faili alguses.

    ```
    'use strict';

    var Registry = require('azure-iothub').Registry;
    var Client = require('azure-iothub').Client;
    ```

5. Järgmised muutuv deklaratsiooni lisamine ja kohatäite väärtuste asendamine.

    ```
    var connectionString = '{iothubconnectionstring}';
    var registry = Registry.fromConnectionString(connectionString);
    var client = Client.fromConnectionString(connectionString);
    var deviceToReboot = 'myDeviceId';
    ```
    
6. Lisage järgmised funktsiooni, et seade meetodi target seadme taaskäivitamist.

    ```
    var startRebootDevice = function(twin) {

        var methodName = "reboot";
        
        var methodParams = {
            methodName: methodName,
            payload: null,
            timeoutInSeconds: 30
        };
        
        client.invokeDeviceMethod(deviceToReboot, methodParams, function(err, result) {
            if (err) { 
                console.error("Direct method error: "+err.message);
            } else {
                console.log("Successfully invoked the device to reboot.");  
            }
        });
    };
    ```

7. Päringu seadme ja taaskäivitage arvuti pärast viimast saada järgmise funktsiooni lisamiseks tehke järgmist.

    ```
    var queryTwinLastReboot = function() {

        registry.getTwin(deviceToReboot, function(err, twin){

            if (twin.properties.reported.iothubDM != null)
            {
                if (err) {
                    console.error('Could not query twins: ' + err.constructor.name + ': ' + err.message);
                } else {
                    var lastRebootTime = twin.properties.reported.iothubDM.reboot.lastReboot;
                    console.log('Last reboot time: ' + JSON.stringify(lastRebootTime, null, 2));
                }
            } else 
                console.log('Waiting for device to report last reboot time.');
        });
    };
    ```
    
8. Lisage järgmine kood helistamiseks funktsioonid, mis käivitab päringu ja taaskäivitage arvuti otsene meetod viimast taaskäivitage arvuti.

    ```
    startRebootDevice();
    setInterval(queryTwinLastReboot, 2000);
    ```
    
9. Salvestage ja sulgege fail **dmpatterns_getstarted_service.js** .

## <a name="run-the-applications"></a>Rakenduste käivitamine

Nüüd olete valmis rakendusi käivitada.

1. Käsuviibale- **manageddevice** kausta, käivitage järgmine käsk kuulama taaskäivitage arvuti otsene meetod.

    ```
    node dmpatterns_getstarted_device.js
    ```

2. Käsuviibale- **triggerrebootondevice** kausta, käivitage järgmine käsk mis käivitab remote taaskäivitage arvuti ja seadme kahe viimase leidmiseks päringu taaskäivitage aeg.

    ```
    node dmpatterns_getstarted_service.js
    ```

3. Kuvatakse sõnumi välja printida reageerida otsene meetod

## <a name="customize-and-extend-the-device-management-actions"></a>Kohandamine ja laiendamine seadme haldamise toimingud

Oma asjade lahenduste saate laiendada seadme halduse mustrite määratletud või lubada kohandatud mustrite seadme kahe- ja C2D meetod primitiivid abil. Muud seadme haldamise toiminguid näiteks factory reset, püsivara värskendamine, tarkvara värskendus, power haldus, võrgu ja ühenduvuse haldus ja andmete krüptimine.

## <a name="device-maintenance-windows"></a>Windowsi seadmes hooldustööd

Tavaliselt, saate konfigureerida seadmed korraga, mis vähendab segadusi ja tööseisakute toimingute teostamiseks.  Seadme hooldus windows on sageli kasutatud mustri määratlemiseks aega, kui seadme värskendama oma konfiguratsioon. Teie tagaandmebaas lahenduste saate kasutada seadme Double soovitud atribuutide määratlemine ja poliitika seadmes, mis võimaldab hooldustööd akna aktiveerimine. Kui seadme saab hooldustööd akna poliitika, see poliitika oleku teatatud atribuut, kusjuures seadme abil. Tagaandmebaas rakendust saate kasutada seadme Double päringute kinnitada seadmed ja iga poliitika.

## <a name="next-steps"></a>Järgmised sammud

Selles õpetuses kasutasite otsene seadmes kasutada remote uuesti käivitamiseks seadme Double teatatud teatada taaskäivitage arvuti viimast seadme atribuutide ja seadme kahe avastada pilvest seadme taaskäivitamist viimast esitatakse selle kohta päring.

Alustamine asjade jaoturi ja seadme halduse mustrid nagu serveri üle air püsivara värskendus jätkamiseks järgmistest teemadest.

[Õpetus: Kuidas püsivara värskendus][lnk-fwupdate]

Saate teada, kuidas laiendada oma asjade lahenduse ja ajakava nõuab mitmes seadmes, leiate [ajakava ja leviedastuse töökohtade] [ lnk-tutorial-jobs] õpetuse.

Jätkamiseks alustamine asjade jaoturi, vaadake teemat [Alustamine asjade lüüsi SDK][lnk-gateway-SDK].


<!-- images and links -->
[img-output]: media/iot-hub-get-started-with-dm/image6.png
[img-dm-ui]: media/iot-hub-get-started-with-dm/dmui.png

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md

[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-fwupdate]: iot-hub-firmware-update.md
[Azure portal]: https://portal.azure.com/
[Using resource groups to manage your Azure resources]: ../azure-portal/resource-group-portal.md
[lnk-dm-github]: https://github.com/Azure/azure-iot-device-management
[lnk-tutorial-jobs]: iot-hub-schedule-jobs.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx