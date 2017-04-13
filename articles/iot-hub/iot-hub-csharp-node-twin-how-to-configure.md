<properties
    pageTitle="Double omaduste kasutamine | Microsoft Azure'i"
    description="Selle õpetuse näitab, kuidas kasutada standard atribuudid"
    services="iot-hub"
    documentationCenter=".net"
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

# <a name="tutorial-use-desired-properties-to-configure-devices-preview"></a>Õpetus: Abil soovitud atribuudid konfigureerida seadmed (eelvaade)

[AZURE.INCLUDE [iot-hub-selector-twin-how-to-configure](../../includes/iot-hub-selector-twin-how-to-configure.md)]

Õppeteema lõpus on teil kaks Node.js konsooli rakendusi:

* **SimulateDeviceConfiguration.js**, jäljendatud seadme rakendus, mis ootab soovitud konfiguratsiooni värskendamise ja aruannete jäljendatud Otsingukonfiguratsiooni värskenduse toimingu olekut.
* **SetDesiredConfigurationAndQuery**, .net-i konsooli rakendus mõeldud tagasi lõpuks, mis määrab soovitud konfiguratsiooni seadme ja päringute konfiguratsiooni värskenduse toimingu käivitada.

> [AZURE.NOTE] Artikli [Asjade jaoturi SDK-d] [ lnk-hub-sdks] leiate teavet eri SDK-d, saate luua nii seadmest ja tagaandmebaas rakendusi.

Selle õpetuse lõpuleviimiseks vajate järgmist:

+ Microsoft Visual Studio 2015.

+ Node.js versioon 0.10.x või uuem versioon.

+ Aktiivne Azure'i konto. (Kui teil pole kontot, saate luua [tasuta konto] [ lnk-free-trial] vaid paar minutit.)

Kui olete täitnud [Alustamine seadme kaksikud] [ lnk-twin-tutorial] õpetuses olete juba seadme halduse lubatud jaoturi ja seadme identiteedi nimetatakse **myDeviceId**; ja [jäljendatud seadme rakenduse loomine] jäta[ lnk-how-to-configure-createapp] jaotis.

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-the-simulated-device-app"></a>Jäljendatud seadme rakenduse loomine

Selles jaotises saate luua Node.js konsooli rakendus, mis ühendab oma jaoturi nimega **myDeviceId**, ootab soovitud konfiguratsiooni update ning seejärel aruanded jäljendatud konfigureerimine värskenduse toimingu värskendusi.

1. Looge uus tühi kaust nimega **simulatedeviceconfiguration**. Klõpsake kaustas **simulatedeviceconfiguration** abil oma käsuviibas järgmine käsk uue package.json faili loomine. Kõik vaikesätted aktsepteerida:

    ```
    npm init
    ```

2. Veebisaidil oma käsuviiba **simulatedeviceconfiguration** kausta, käivitage järgmine käsk installida **azure-asjade Interneti-seade**ja **azure asjade Interneti-seadme mqtt** paketi:

    ```
    npm install azure-iot-device@dtpreview azure-iot-device-mqtt@dtpreview --save
    ```

3. Kaustas **simulatedeviceconfiguration** tekstiredaktoris kasutamisel **SimulateDeviceConfiguration.js** uue faili loomine.

4. Lisada järgmine kood **SimulateDeviceConfiguration.js** faili ja asendada **{seadme ühendusstringi}** kohatäite koos kopeeritud **myDeviceId** seadme identiteedi loomisel ühendusstring.

        'use strict';
        var Client = require('azure-iot-device').Client;
        var Protocol = require('azure-iot-device-mqtt').Mqtt;

        var connectionString = '{device connection string}';
        var client = Client.fromConnectionString(connectionString, Protocol);

        client.open(function(err) {
            if (err) {
                console.error('could not open IotHub client');
            } else {
                client.getTwin(function(err, twin) {
                    if (err) {
                        console.error('could not get twin');
                    } else {
                        console.log('retrieved device twin');
                        twin.properties.reported.telemetryConfig = {
                            configId: "0",
                            sendFrequency: "24h"
                        }
                        twin.on('properties.desired', function(desiredChange) {
                            console.log("received change: "+JSON.stringify(desiredChange));
                            var currentTelemetryConfig = twin.properties.reported.telemetryConfig;
                            if (desiredChange.telemetryConfig &&desiredChange.telemetryConfig.configId !== currentTelemetryConfig.configId) {
                                initConfigChange(twin);
                            }
                        });
                    }
                });
            }
        });

    **Kliendi** objekti seab kõik meetodid, mis on nõutav seadme kaksikud seadme suhelda. Eelmine kood pärast seda lähtestab **Kliendi** objekti, toob Double jaoks **myDeviceId**ja manustab sündmuseohjuri, värskendus, klõpsake soovitud atribuudid. Funktsiooni sündmuseohjuri kinnitab, et seal on taotluse tegeliku konfiguratsiooni muutmine võrreldes soovitud configIds ja seejärel viitab meetod, mis käivitab konfiguratsiooni muuta.

    Pange tähele, et huvides lihtne, kasutab eelmise koodi raske koodiga vaikimisi algse konfigureerimise. Otse rakenduse oleks laadi tõenäoliselt selle konfiguratsiooni kohaliku salvestusruumi.
    
    > [AZURE.IMPORTANT] Soovitud atribuudi muutmine sündmuste alati väljumise kord seadme ühendus, veenduge, et kontrollida, on tegelik muutmine soovitud atribuudid, mis tahes toimingu sooritamiseks.

5. Lisage järgmistest meetoditest enne selle `client.open()` kutsumise:

        var initConfigChange = function(twin) {
            var currentTelemetryConfig = twin.properties.reported.telemetryConfig;
            currentTelemetryConfig.pendingConfig = twin.properties.desired.telemetryConfig;
            currentTelemetryConfig.status = "Pending";

            var patch = {
            telemetryConfig: currentTelemetryConfig
            };
            twin.properties.reported.update(patch, function(err) {
                if (err) {
                    console.log('Could not report properties');
                } else {
                    console.log('Reported pending config change: ' + JSON.stringify(patch));
                    setTimeout(function() {completeConfigChange(twin);}, 60000);
                }
            });
        }

        var completeConfigChange =  function(twin) {
            var currentTelemetryConfig = twin.properties.reported.telemetryConfig;
            currentTelemetryConfig.configId = currentTelemetryConfig.pendingConfig.configId;
            currentTelemetryConfig.sendFrequency = currentTelemetryConfig.pendingConfig.sendFrequency;
            currentTelemetryConfig.status = "Success";
            delete currentTelemetryConfig.pendingConfig;
            
            var patch = {
                telemetryConfig: currentTelemetryConfig
            };
            patch.telemetryConfig.pendingConfig = null;

            twin.properties.reported.update(patch, function(err) {
                if (err) {
                    console.error('Error reporting properties: ' + err);
                } else {
                    console.log('Reported completed config change: ' + JSON.stringify(patch));
                }
            });
        };

    **InitConfigChange** meetod värskendab teatatud kohaliku Double objekti atribuudid config update taotluse ja määrab selle oleku **Ootel**, siis värskendatakse seadme Double teenuse. Pärast värskendamist edukalt kusjuures, see jäljendab pikaajalise protsessi, mis lõpeb **completeConfigChange**täitmiseks. See meetod uuendab kohaliku Double teatatud atribuutide seadmine oleku **edu** ja **pendingConfig** objekti. See siis värskendatakse Double teenuse.

    Pange tähele, et salvestada läbilaskevõime, teatatud atribuudid on värskendatud, määrates vaid atribuudid muuta (nimega **paik** ülaltoodud kood), asendades kogu dokumendis asemel.

    > [AZURE.NOTE] Selles õpetuses pole simuleerida mis tahes käitumine samaaegseid konfiguratsiooni värskendusi. Mõned konfiguratsiooni värskendamine protsessid võib kohandada target konfiguratsiooni muudatusi, kui värskenduse töötab, teised võib-olla järjekorda need ja teised võivad tagasi lükata tõrkeolukorras. Veenduge, et teie konkreetse konfiguratsiooni protsessi soovitud käitumist silmas pidada ja lisage sobiv loogika konfiguratsiooni muutmise alustamist.

6. Käivitage rakendus seade.

        node SimulateDeviceConfiguration.js

    Kuvatakse teade `retrieved device twin`. Säilita rakendus töötab.

## <a name="create-the-service-app"></a>Teenuse rakenduse loomine

Selles jaotises loote .net-i konsooli rakendus, mis värskendab Double, mis on seotud **myDeviceId** uue telemeetria konfiguratsiooni objekti *soovitud atribuudid* . Seejärel seade kaksikud keskuses salvestatud päringute ja kuvatakse soovitud ja teatatud konfiguratsioone seadme vahe.

1. Visual Studio, lisada Visual C# Windowsi klassikaline töölaua projekti praegune lahendus **Konsooli rakendus** projekti malli abil. Projekti **SetDesiredConfigurationAndQuery**nimi.

    ![Visual C# Windowsi klassikaline töölaua uue projekti][img-createapp]

2. Solution Exploreris Paremklõpsake **SetDesiredConfigurationAndQuery** projekti ja seejärel klõpsake nuppu **Halda Nuget-paketid**.

3. **Nugeti Package Manager** aknas, veenduge, et märgitud on **kaasamine väljalaske-eelne** , **microsoft.azure.devices**otsida, valige installimiseks **installida** soovitud Office'i *väljalaske-eelse* versiooni **Microsoft.Azure.Devices** paketti ja nõustuge kasutustingimustega. Selle toimingu allalaadimist, installib ja lisab [Microsoft Azure'i asjade teenuse SDK] viide[ lnk-nuget-service-sdk] Nugeti pakett ja sõltuvustega.

    ![Nugeti Package Manager aken][img-servicenuget]

4. Lisage järgmine `using` laused **Program.cs** faili ülaosas:

        using Microsoft.Azure.Devices;
        using System.Threading;
        using Newtonsoft.Json;

5. Saate lisada **programmi** klassi järgmised väljad. Asendage kohatäite väärtuse jaoks asjade jaoturi eelmises jaotises loodud ühendusstring.

        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";

6. Saate lisada **programmi** klassi järgmisel viisil:

        static private async Task SetDesiredConfigurationAndQuery()
        {
            var twin = await registryManager.GetTwinAsync("myDeviceId");
            var patch = new {
                    properties = new {
                        desired = new {
                            telemetryConfig = new {
                                configId = Guid.NewGuid().ToString(),
                                sendFrequency = "5m"
                            }
                        }
                    }
                };
            
            await registryManager.UpdateTwinAsync(twin.DeviceId, JsonConvert.SerializeObject(patch), twin.ETag);
            Console.WriteLine("Updated desired state");

            while (true)
            {
                var query = registryManager.CreateQuery("SELECT * FROM devices WHERE deviceId = 'myDeviceId'");
                var results = await query.GetNextAsTwinAsync();
                foreach (var result in results)
                {
                    Console.WriteLine("Config report for: {0}", result.DeviceId);
                    Console.WriteLine("Desired telemetryConfig: {0}", JsonConvert.SerializeObject(result.Properties.Desired["telemetryConfig"], Formatting.Indented));
                    Console.WriteLine("Reported telemetryConfig: {0}", JsonConvert.SerializeObject(result.Properties.Reported["telemetryConfig"], Formatting.Indented));
                    Console.WriteLine();
                }
                Thread.Sleep(10000);
            }
        }

    **Registri** objekti seab kõik meetodid, mis on nõutav seadme kaksikud teenuse suhelda. Eelmise koodi pärast selle lähtestab **registri** objekti, toob Double jaoks **myDeviceId**ja värskendab uue telemeetria konfiguratsiooni objekti soovitud atribuudid.
    Pärast seda, iga 10 sekundi seda päringut talletatud keskuses kaksikud ja prindib soovitud ja teatatud telemeetria konfiguratsioone. [Asjade jaoturi päringukeele] viidata[ lnk-query] saate teada, kuidas rikkaliku aruannete loomiseks kõigis oma seadmetes.

    > [AZURE.IMPORTANT] See rakendus küsib asjade jaoturi iga 10 sekundi järel toodud. Päringute kasutamine kasutaja suunatud aruannete loomiseks palju seadmetes ja ei tuvasta muudatused. Kui teie lahendus on vaja reaalajas teatised seadme sündmuste kasutada [seadme pilve sõnumite][lnk-d2c].

7. Lõpetuseks, lisage järgmised read **Esilehele** meetod.

        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        SetDesiredConfigurationAndQuery();
        Console.WriteLine("Press any key to quit.");
        Console.ReadLine();

8. **SimulateDeviceConfiguration.js** töötab, kasutades **klahvi F5** ja te Visual Studio .net-i rakenduse, lugege artiklit teatatud konfiguratsiooni muuta **edu** **Ootel** **edu** uuesti koos uut Käivita saada sagedus 24 tunni asemel viis minutit.

    > [AZURE.IMPORTANT] On viivitus, kuni minuti seadme aruande ja päringu tulem vahel. See on lubamiseks päringu taristu tööl väga kõrge skaala. Toomiseks kasutada ühtsete vaadete ühe Double **getDeviceTwin** meetod **registri** tunni.

## <a name="next-steps"></a>Järgmised sammud

Selles õpetuses tagasi lõpuks seada soovitud konfiguratsiooni *soovitud atribuudid* ja kirjutas seadme rakendus selle muudatuse tuvastamiseks ja simuleerida mitme etapi update protsessi aruandlus selle olek on kasu *teatatud* atribuutidena.

Kasutage järgmisi ressursse saate teada, kuidas:

- saada telemeetria seadmest, mille [Alustamine asjade jaoturi] [ lnk-iothub-getstarted] õpetuses
- või toiminguid suurte andmekomplektide seadmed vt [kasutamine töö kavandada ja leviedastus seadme toimingute] ajastamine[ lnk-schedule-jobs] õpetuse.
- juhtida seadmete interaktiivseks (nt kasutaja kontrollitud rakendusest fänn sisselülitamine) [kasutamine otsese meetodite] [ lnk-methods-tutorial] õpetuse.

<!-- images -->
[img-servicenuget]: media/iot-hub-csharp-node-twin-getstarted/servicesdknuget.png
[img-createapp]: media/iot-hub-csharp-node-twin-getstarted/createnetapp.png

<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/1.1.0-preview-004

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-dm-overview]: iot-hub-device-management-overview.md
[lnk-twin-tutorial]: iot-hub-node-node-twin-getstarted.md
[lnk-schedule-jobs]: iot-hub-schedule-jobs.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
[lnk-device-management]: iot-hub-device-management-get-started.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-iothub-getstarted]: iot-hub-node-node-getstarted.md
[lnk-methods-tutorial]: iot-hub-c2d-methods.md

[lnk-guid]: https://en.wikipedia.org/wiki/Globally_unique_identifier

[lnk-how-to-configure-createapp]: iot-hub-node-node-twin-how-to-configure.md#create-the-simulated-device-app
