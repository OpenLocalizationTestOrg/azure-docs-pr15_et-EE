<properties
    pageTitle="Alustamine kaksikud | Microsoft Azure'i"
    description="Selle õpetuse näidatakse, kuidas kasutada kaksikud"
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

Õppeteema lõpus on teil on .net-i ja Node.js konsooli rakendus:

* **AddTagsAndQuery.sln**, .net-i rakenduse, tagasi lõpust, mis lisab sildid ja päringute seadme kaksikud käivitamiseks mõeldud.
* **TwinSimulatedDevice.js**, Node.js rakendus, mis jäljendab seade, mis ühendab oma asjade jaoturi varem loodud seadme identiteediga ja aruandeid oma ühenduvuse tingimus.

> [AZURE.NOTE] Artikli [Asjade jaoturi SDK-d] [ lnk-hub-sdks] leiate teavet eri SDK-d, saate luua nii seadmest ja tagaandmebaas rakendusi.

Selle õpetuse lõpuleviimiseks vajate järgmist:

+ Microsoft Visual Studio 2015.

+ Node.js versioon 0.10.x või uuem versioon.

+ Aktiivne Azure'i konto. (Kui teil pole kontot, saate luua [tasuta konto] [ lnk-free-trial] vaid paar minutit.)

[AZURE.INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-the-service-app"></a>Teenuse rakenduse loomine

Selles jaotises saate luua Node.js konsooli rakendus, mis lisab asukoht metaandmete Double, mis on seotud **myDeviceId**. See siis päringud, mis on talletatud valides seadmete keskuses kaksikud asub USA-s ja seejärel need aruandluse mobiilsideühendust.

1. Visual Studio, lisada Visual C# Windowsi klassikaline töölaua projekti praegune lahendus **Konsooli rakendus** projekti malli abil. Projekti **AddTagsAndQuery**nimi.

    ![Visual C# Windowsi klassikaline töölaua uue projekti][img-createapp]

2. Solution Exploreris Paremklõpsake **AddTagsAndQuery** projekti ja seejärel klõpsake nuppu **Halda Nuget-paketid**.

3. **Nugeti Package Manager** aknas, veenduge, et märgitud on **kaasamine väljalaske-eelne** , **microsoft.azure.devices**otsida, valige installimiseks **installida** soovitud Office'i *väljalaske-eelse* versiooni **Microsoft.Azure.Devices** paketti ja nõustuge kasutustingimustega. Selle toimingu allalaadimist, installib ja lisab [Microsoft Azure'i asjade teenuse SDK] viide[ lnk-nuget-service-sdk] Nugeti pakett ja sõltuvustega.

    ![Nugeti Package Manager aken][img-servicenuget]

4. Lisage järgmine `using` laused **Program.cs** faili ülaosas:

        using Microsoft.Azure.Devices;

5. Saate lisada **programmi** klassi järgmised väljad. Asendage kohatäite väärtuse jaoks asjade jaoturi eelmises jaotises loodud ühendusstring.

        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";

6. Saate lisada **programmi** klassi järgmisel viisil:

        public static async Task AddTagsAndQuery()
        {
            var twin = await registryManager.GetTwinAsync("myDeviceId");
            var patch =
                @"{
                    tags: {
                        location: {
                            region: 'US',
                            plant: 'Redmond43'
                        }
                    }
                }";
            await registryManager.UpdateTwinAsync(twin.DeviceId, patch, twin.ETag);

            var query = registryManager.CreateQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43'", 100);
            var twinsInRedmond43 = await query.GetNextAsTwinAsync();
            Console.WriteLine("Devices in Redmond43: {0}", string.Join(", ", twinsInRedmond43.Select(t => t.DeviceId)));

            query = registryManager.CreateQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43' AND properties.reported.connectivity.type = 'cellular'", 100);
            var twinsInRedmond43UsingCellular = await query.GetNextAsTwinAsync();
            Console.WriteLine("Devices in Redmond43 using cellular network: {0}", string.Join(", ", twinsInRedmond43UsingCellular.Select(t => t.DeviceId)));
        }

    Klassi **RegistryManager** seab kõik meetodid, mis on nõutav seadme kaksikud teenuse suhelda. Eelmise koodi esmalt lähtestab **registryManager** objekti, seejärel toob Double jaoks **myDeviceId**ja lõpuks värskendab oma siltide teabega soovitud asukohta.

    Pärast värskendamist, see käivitab kaks päringute: esimese valib ainult seadmetes, mis asub **Redmond43** taimest seadme kaksikud ja teine täiustab päringu ainult ka mobiilsidevõrgu kaudu ühendatud seadmete valimiseks.

    Pange tähele, et eelmise koodi, kui **päring** objekti, loob see määrab tagastatud dokumentide arvu ülempiir. **Päringu** objekt sisaldab **HasMoreResults** kahendväärtus atribuut, mille abil saate kasutada **GetNextAsTwinAsync** meetodite kõik tulemeid tuua mitu korda. Meetodit nimetatakse **GetNextAsJson** on saadaval tulemused, mis pole seadme kaksikud näiteks koondamine päringute tulemid.

7. Lõpetuseks, lisage järgmised read **Esilehele** meetod.

        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        AddTagsAndQuery().Wait();
        Console.WriteLine("Press Enter to exit.");
        Console.ReadLine();

8. Seda rakendust käivitama ja kuvatakse üks tulemused päringu küsiks kõigis seadmetes, mis asuvad **Redmond43** ja pole päringu, mis piirab tulemused seadmed, mis kasutavad mobiilsidevõrgu seade.

    ![Päringutulemite aknas][img-addtagapp]

Järgmises jaotises loote seadme rakendus, mis ühenduvuse andmed ja muudab eelmises jaotises päringu tulem.

## <a name="create-the-device-app"></a>Seadme rakenduse loomine

Selles jaotises saate luua soovitud Node.js konsooli rakendus, mis ühendab oma jaoturi **myDeviceId**nimega ning seejärel värskendab tema kahe teatatud atribuudid, mis sisaldavad andmeid, et see on ühendatud mobiilsidevõrgu kaudu.

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

6. Nüüd, kui seadme teatatud selle kohta teavet, kuvatakse see nii päringud. Käivitage.NET **AddTagsAndQuery** rakenduse päringute uuesti käivitada. See aeg **myDeviceId** kuvatakse mõlema päringu tulemused.

    ![][img-addtagapp2]

## <a name="next-steps"></a>Järgmised sammud
Selles õpetuses konfigureeritud uue asjade jaoturi Azure portaali ja seejärel loodud seadme identiteedi jaoturi identiteedi registri. Lisatud seadme metaandmete siltide tagaandmebaas rakendusest ja kirjutas jäljendatud seadme rakendus aruande seadme ühenduvuse teavet seadme Double. Soovite õpitut ka päringu seda teavet asjade jaoturi SQL-like päringukeele abil tehke järgmist.

Kasutage järgmisi ressursse saate teada, kuidas:

- saada telemeetria seadmest, mille [Alustamine asjade jaoturi] [ lnk-iothub-getstarted] õpetuses
- seadmete Double 's soovitud atribuudid abil saate [kasutada soovitud atribuudid konfigureerida seadmete] konfigureerimine[ lnk-twin-how-to-configure] õpetuses
- juhtida seadmete interaktiivseks (nt sisselülitamine fänn kasutaja kontrollitud rakenduse kaudu) [kasutamine otsese meetodite] [ lnk-methods-tutorial] õpetuse.

<!-- images -->
[img-servicenuget]: media/iot-hub-csharp-node-twin-getstarted/servicesdknuget.png
[img-createapp]: media/iot-hub-csharp-node-twin-getstarted/createnetapp.png
[img-addtagapp]: media/iot-hub-csharp-node-twin-getstarted/addtagapp.png
[img-addtagapp2]: media/iot-hub-csharp-node-twin-getstarted/addtagapp2.png

<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/1.1.0-preview-004

[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-identity]: iot-hub-devguide-identity-registry.md

[lnk-iothub-getstarted]: iot-hub-node-node-getstarted.md
[lnk-methods-tutorial]: iot-hub-c2d-methods.md
[lnk-twin-how-to-configure]: iot-hub-csharp-node-twin-how-to-configure.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md

