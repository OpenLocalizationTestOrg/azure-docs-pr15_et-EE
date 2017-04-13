<properties
    pageTitle="Alustamine kaksikud | Microsoft Azure'i"
    description="Selle õpetuse näitab, kuidas kasutada kaksikud"
    services="iot-hub"
    documentationCenter="node"
    authors="fsautomata"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="node"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/13/2016"
     ms.author="elioda"/>

# <a name="tutorial-get-started-with-device-twins-preview"></a>Õpetus: Alustamine seadme kaksikud (eelvaade)

[AZURE.INCLUDE [iot-hub-selector-twin-get-started](../../includes/iot-hub-selector-twin-get-started.md)]

Õppeteema lõpus on teil kaks Node.js konsooli rakendusi:

* **AddTagsAndQuery.js**, tagasi lõpust, mis lisab sildid ja päringute seadme kaksikud käivitamiseks mõeldud Node.js rakenduse loomiseks.
* **TwinSimulatedDevice.js**, Node.js rakendus, mis jäljendab seade, mis ühendab oma asjade jaoturi varem loodud seadme identiteediga ja aruandeid oma ühenduvuse tingimus.

> [AZURE.NOTE] Artikli [Asjade jaoturi SDK-d] [ lnk-hub-sdks] leiate teavet eri SDK-d, saate luua nii seadmest ja tagaandmebaas rakendusi.

Selle õpetuse lõpuleviimiseks vajate järgmist:

+ Node.js versioon 0.10.x või uuem versioon.

+ Aktiivne Azure'i konto. (Kui teil pole kontot, saate luua [tasuta konto] [ lnk-free-trial] vaid paar minutit.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-the-service-app"></a>Teenuse rakenduse loomine

Selles jaotises saate luua Node.js konsooli rakendus, mis lisab asukoht metaandmete Double, mis on seotud **myDeviceId**. See siis päringud, mis on talletatud valides seadmete keskuses kaksikud asub USA-s ja seejärel need aruandluse mobiilsideühendust.

1. Looge uus tühi kaust nimega **addtagsandqueryapp**. Klõpsake kaustas **addtagsandqueryapp** abil oma käsuviibas järgmine käsk uue package.json faili loomine. Kõik vaikesätted aktsepteerida:

    ```
    npm init
    ```

2. Veebisaidil oma käsuviiba **addtagsandqueryapp** kausta, installige **Azure'i-iothub** pakett järgmine käsk:

    ```
    npm install azure-iothub@dtpreview --save
    ```

3. Kaustas **addtagsandqueryapp** tekstiredaktoris kasutamisel **AddTagsAndQuery.js** uue faili loomine.

4. Lisada järgmine kood **AddTagsAndQuery.js** faili ja asendada **{teenuse ühendusstringi}** kohatäite koos kopeerisite oma jaoturi loomisel ühendusstring.

        'use strict';
        var iothub = require('azure-iothub');
        var connectionString = '{service hub connection string}';
        var registry = iothub.Registry.fromConnectionString(connectionString);

        registry.getTwin('myDeviceId', function(err, twin){
            if (err) {
                console.error(err.constructor.name + ': ' + err.message);
            } else {
                var patch = {
                    tags: {
                        location: {
                            region: 'US',
                            plant: 'Redmond43'
                      }
                    }
                };
             
                twin.update(patch, function(err) {
                  if (err) {
                    console.error('Could not update twin: ' + err.constructor.name + ': ' + err.message);
                  } else {
                    console.log(twin.deviceId + ' twin updated successfully');
                    queryTwins();
                  }
                });
            }
        });

    **Registri** objekti seab kõik meetodid, mis on nõutav seadme kaksikud teenuse suhelda. Eelmise koodi esmalt lähtestab **registri** objekti, seejärel toob Double jaoks **myDeviceId**ja lõpuks värskendab oma siltide teabega soovitud asukohta.

    Pärast värskendamist sildid see nõuab **queryTwins** funktsioon.

7. Lisage järgmine kood **AddTagsAndQuery.js** rakendada funktsiooni **queryTwins** lõpus.

        var queryTwins = function() {
            var query = registry.createQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43'", 100);
            query.nextAsTwin(function(err, results) {
                if (err) {
                    console.error('Failed to fetch the results: ' + err.message);
                } else {
                    console.log("Devices in Redmond43: " + results.map(function(twin) {return twin.deviceId}).join(','));
                }
            });
            
            query = registry.createQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43' AND properties.reported.connectivity.type = 'cellular'", 100);
            query.nextAsTwin(function(err, results) {
                if (err) {
                    console.error('Failed to fetch the results: ' + err.message);
                } else {
                    console.log("Devices in Redmond43 using cellular network: " + results.map(function(twin) {return twin.deviceId}).join(','));
                }
            });
        };

    Eelmine kood käivitab kaks päringute: esimese valib ainult seadmetes, mis asub **Redmond43** taimest seadme kaksikud ja teine täiustab päringu ainult ka mobiilsidevõrgu kaudu ühendatud seadmete valimiseks.

    Pange tähele, et eelmise koodi, kui **päring** objekti, loob see määrab tagastatud dokumentide arvu ülempiir. **Päringu** objekt sisaldab **hasMoreResults** kahendmuutujaga atribuut, mille abil saate kasutada **nextAsTwin** võimalused kõigi tulemeid tuua mitu korda. Nimega **järgmise** meetodi on saadaval tulemused, mis pole seadme kaksikud näiteks koondamine päringute tulemid.

8. Käivitage rakendus abil:

        node AddTagsAndQuery.js

    Peaksite nägema üks tulemused päringu küsiks kõigis seadmetes, mis asuvad **Redmond43** ja pole päringu, mis piirab tulemused seadmed, mis kasutavad mobiilsidevõrgu seade.

    ![][1]

Järgmises jaotises loote seadme rakendus, mis ühenduvuse andmed ja muudab eelmises jaotises päringu tulem.

## <a name="create-the-device-app"></a>Seadme rakenduse loomine

Selles jaotises saate luua soovitud Node.js konsooli rakendus, mis ühendab oma jaoturi **myDeviceId**nimega ning seejärel värskendab tema kahe teatatud atribuudid, mis sisaldavad andmeid, et see on ühendatud mobiilsidevõrgu kaudu.

> [AZURE.NOTE] Sel ajal, seadme kaksikud pääseb juurde ainult asjade jaoturi MQTT protokolli abil ühendatud seadmete. Palun vaadake [MQTT toetavad] [ lnk-devguide-mqtt] artiklist leiate juhised, kuidas muuta olemasolevaid seadme rakenduse kasutamiseks MQTT.

1. Looge uus tühi kaust nimega **reportconnectivity**. Klõpsake kaustas **reportconnectivity** abil oma käsuviibas järgmine käsk uue package.json faili loomine. Kõik vaikesätted aktsepteerida:

    ```
    npm init
    ```

2. Veebisaidil oma käsuviiba **reportconnectivity** kausta, käivitage järgmine käsk installida **azure-asjade Interneti-seade**ja **azure asjade Interneti-seadme mqtt** paketi:

    ```
    npm install azure-iot-device@dtpreview azure-iot-device-mqtt@dtpreview --save
    ```

3. Kaustas **reportconnectivity** tekstiredaktoris kasutamisel **ReportConnectivity.js** uue faili loomine.

4. Lisada järgmine kood **ReportConnectivity.js** faili ja asendada **{seadme ühendusstringi}** kohatäite koos kopeeritud **myDeviceId** seadme identiteedi loomisel ühendusstring.

        'use strict';
        var Client = require('azure-iot-device').Client;
        var Protocol = require('azure-iot-device-mqtt').Mqtt;

        var connectionString = '{device connection string}';
        var client = Client.fromConnectionString(connectionString, Protocol);

        client.open(function(err) {
        if (err) {
            console.error('could not open IotHub client');
        }  else {
            console.log('client opened');

            client.getTwin(function(err, twin) {
            if (err) {
                console.error('could not get twin');
            } else {
                var patch = {
                    connectivity: {
                        type: 'cellular'
                    }
                };

                twin.properties.reported.update(patch, function(err) {
                    if (err) {
                        console.error('could not update twin');
                    } else {
                        console.log('twin state reported');
                        process.exit();
                    }
                });
            }
            });
        }
        });

    **Kliendi** objekti seab kõik meetodid, mida vajate suhtlemiseks seadme kaksikud seadmest. Eelmise koodi pärast selle lähtestab **Kliendi** objekti, toob Double jaoks **myDeviceId** ja värskendab teatatud atribuudi ühenduvuse teabega.

5. Käivitage rakendus seade

        node ReportConnectivity.js

    Kuvatakse teade `twin state reported`.

6. Nüüd, kui seadme teatatud selle kohta teavet, kuvatakse see nii päringud. Minge tagasi **addtagsandqueryapp** kausta ja päringute uuesti käivitada.

        node AddTagsAndQuery.js

    See aeg **myDeviceId** kuvatakse mõlema päringu tulemused.

    ![][3]

## <a name="next-steps"></a>Järgmised sammud
Selles õpetuses konfigureeritud uue asjade jaoturi Azure portaali ja seejärel loodud seadme identiteedi jaoturi identiteedi registri. Lisatud seadme metaandmete siltide tagaandmebaas rakendusest ja kirjutas jäljendatud seadme rakendus aruande seadme ühenduvuse teavet seadme Double. Soovite õpitut ka päringu seda teavet asjade jaoturi SQL-like päringukeele abil tehke järgmist.

Kasutage järgmisi ressursse saate teada, kuidas:

- saada telemeetria seadmest, mille [Alustamine asjade jaoturi] [ lnk-iothub-getstarted] õpetuses
- seadmete Double 's soovitud atribuudid abil saate [kasutada soovitud atribuudid konfigureerida seadmete] konfigureerimine[ lnk-twin-how-to-configure] õpetuses
- juhtida seadmete interaktiivseks (nt kasutaja kontrollitud rakendusest fänn sisselülitamine) [kasutamine otsese meetodite] [ lnk-methods-tutorial] õpetuse.

<!-- images -->
[1]: media/iot-hub-node-node-twin-getstarted/service1.png
[3]: media/iot-hub-node-node-twin-getstarted/service2.png

<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-identity]: iot-hub-devguide-identity-registry.md

[lnk-iothub-getstarted]: iot-hub-node-node-getstarted.md
[lnk-device-management]: iot-hub-device-management-get-started.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/

[lnk-twin-how-to-configure]: iot-hub-node-node-twin-how-to-configure.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md

[lnk-methods-tutorial]: iot-hub-c2d-methods.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md