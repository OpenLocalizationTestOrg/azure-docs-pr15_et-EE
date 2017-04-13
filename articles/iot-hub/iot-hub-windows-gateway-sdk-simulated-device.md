<properties
    pageTitle="Simuleerida asjade lüüsi SDK seade | Microsoft Azure'i"
    description="Kasutamise illustreerimiseks saatmise telemeetria jäljendatud seadmest, kasutades Azure asjade lüüsi SDK Windows Azure'i asjade lüüsi SDK kiirtutvustus."
    services="iot-hub"
    documentationCenter=""
    authors="chipalost"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="cpp"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/29/2016"
     ms.author="andbuc"/>


# <a name="azure-iot-gateway-sdk-beta--send-device-to-cloud-messages-with-a-simulated-device-using-windows"></a>Azure'i asjade lüüsi SDK (beeta) – seadme pilve sõnumite saatmine Windows kasutamine jäljendatud seadmega

[AZURE.INCLUDE [iot-hub-gateway-sdk-simulated-selector](../../includes/iot-hub-gateway-sdk-simulated-selector.md)]

## <a name="build-and-run-the-sample"></a>Koostamine ja valimi käivitamine

Enne alustamist peate tegema järgmist:

- [Häälestada oma arenduskeskkond] [ lnk-setupdevbox] Windows SDK töötamiseks.
- [Soovitud asjade keskuse loomine] [ lnk-create-hub] Azure tellimuse, peate oma jaoturi juhendis lõpuleviimiseks nime. Kui teil pole kontot, saate luua [tasuta konto] [ lnk-free-trial] vaid paar minutit.
- Kahe seadme lisada oma asjade jaoturi ja kirjutage oma ID-d ja seadme võtmed. Saate selle [seadme Exploreri või iothub-Exploreri] [ lnk-explorer-tools] tööriista lisamine asjade jaoturi eelmises juhises loodud seadmete ja tuua oma võtmed.

Valimi koostamiseks:

1. Avage käsuviip **arendaja Käsuviip VS2015 jaoks** .
2. Liikuge oma kohaliku eksemplari **Azure'i asjade Interneti-lüüsi sdk** hoidla juurkausta.
3. Käivitage selle **Tööriistad\\build.cmd** skripti. See skript loob faili lahenduse Visual Studios, koostab lahendus ja töötab testide. **Koostada** kausta lahenduse Visual Studios leiate **Azure'i asjade Interneti-lüüsi sdk** hoidla kohaliku eksemplari.

Valimi käivitamiseks tehke järgmist.

Avage tekstiredaktoris, fail **näidised\\simulated_device_cloud_upload\\src\\simulated_device_cloud_upload_win.json** **azure asjade Interneti-lüüsi sdk** hoidla kohaliku eksemplari. Selle faili konfigureerib moodulid valimi Gateway:

- **IoTHub** mooduli loob teie asjade jaoturi. Tuleb konfigureerida oma asjade jaoturi andmeid saata. Täpsemalt **IoTHubName** väärtuseks asjade jaoturi oma nime ja määrake väärtuseks **IoTHubSuffix** **azure-devices.net**. Määrake väärtuseks **Transport** üks: "HTTP", "AMQP" või "MQTT". Pange tähele, et praegu ainult "HTTP" jagada ühe TCP ühendus seadme kõigis sõnumites. Kui seate "AMQP" või "MQTT", lüüsi säilitab eraldi TCP-ühendus asjade jaoturi iga seadmega.
- **Vastenduse** mooduli kaardid jäljendatud seadmetes MAC-aadressid asjade jaoturi seadme ID-d. Veenduge, et **deviceId** väärtused kattuvad lisasite oma asjade jaoturi kahe seadme ID-d, ning et **deviceKey** väärtused sisaldavad kahe seadme klahvid.
- **BLE1** ja **BLE2** moodulid on jäljendatud seadmed. Pange tähele, kuidas oma MAC-aadressid liikumis- **vastenduse** mooduli vaste.
- **Puuraidur** mooduli logib oma lüüsi tegevuse faili.
- **Mooduli tee** väärtused allpool Oletame, et te kloonitud asjade lüüsi SDK hoidla juurkausta **c** -ketas. Kui laadisite teise asukohta, peate **tee mooduli** väärtused kohandada.
- JSON-faili allosas **linke** massiiv loob **BLE1** ja **BLE2** moodulid **vastenduse** mooduli ja selle **vastenduse** mooduli **IoTHub** mooduli. Samuti tagatakse, et kõik sõnumid on sisse logitud **puuraidur** mooduli.

```
{
    "modules" :
    [ 
        {
            "module name" : "IoTHub",
            "module path" : "C:\\azure-iot-gateway-sdk\\build\\modules\\iothub\\Debug\\iothub_hl.dll",
            "args" : 
            {
                "IoTHubName" : "{Your IoT hub name}",
                "IoTHubSuffix" : "azure-devices.net",
                "Transport": "HTTP"
            }
        },
        {
            "module name" : "mapping",
            "module path" : "C:\\azure-iot-gateway-sdk\\build\\modules\\identitymap\\Debug\\identity_map_hl.dll",
            "args" : 
            [
                {
                    "macAddress" : "01-01-01-01-01-01",
                    "deviceId"   : "{Device ID 1}",
                    "deviceKey"  : "{Device key 1}"
                },
                {
                    "macAddress" : "02-02-02-02-02-02",
                    "deviceId"   : "{Device ID 2}",
                    "deviceKey"  : "{Device key 2}"
                }
            ]
        },
        {
            "module name":"BLE1",
            "module path" : "C:\\azure-iot-gateway-sdk\\build\\modules\\simulated_device\\Debug\\simulated_device_hl.dll",
            "args":
            {
                "macAddress" : "01-01-01-01-01-01"
            }
        },
        {
            "module name":"BLE2",
            "module path" : "C:\\azure-iot-gateway-sdk\\build\\modules\\simulated_device\\Debug\\simulated_device_hl.dll",
            "args":
            {
                "macAddress" : "02-02-02-02-02-02"
            }
        },
        {
            "module name":"Logger",
            "module path" : "C:\\azure-iot-gateway-sdk\\build\\modules\\logger\\Debug\\logger_hl.dll",
            "args":
            {
                "filename":"C:\\azure-iot-gateway-sdk\\deviceCloudUploadGatewaylog.log"
            }
        }
    ],
    "links" : [
        { "source" : "*", "sink" : "Logger" },
        { "source" : "BLE1", "sink" : "mapping" },
        { "source" : "BLE2", "sink" : "mapping" },
        { "source" : "mapping", "sink" : "IoTHub" }
    ]
}
```

Konfiguratsioonifail tehtud muudatused salvestada.

Valimi käivitamiseks tehke järgmist.

1. Käsureale, liikuge oma kohaliku eksemplari **Azure'i asjade Interneti-lüüsi sdk** hoidla juurkausta.
2. Käivitage järgmine käsk:
  
    ```
    build\samples\simulated_device_cloud_upload\Debug\simulated_device_cloud_upload_sample.exe samples\simulated_device_cloud_upload\src\simulated_device_cloud_upload_win.json
    ```

3. Saate selle [seadme Exploreri või iothub-Exploreri] [ lnk-explorer-tools] tööriista sõnumid, mis asjade jaoturi saab lüüsi jälgimiseks.


## <a name="next-steps"></a>Järgmised sammud

Kui soovite asjade lüüsi SDK täpsemate mõistmiseks ja koodi näited katsetada, külastage järgmist arendaja õpetused ja ressursse:

- [Reaal seadmest, mille asjade lüüsi SDK seadme pilve sõnumite saatmine][lnk-physical-device]
- [Azure'i asjade lüüsi SDK][lnk-gateway-sdk]

Asjade jaoturi võimaluste täpsemaks analüüsimiseks järgmistest teemadest.

- [Arendaja juhend][lnk-devguide]

<!-- Links -->
[lnk-setupdevbox]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/doc/devbox_setup.md
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-explorer-tools]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/manage_iot_hub.md
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk/

[lnk-physical-device]: iot-hub-gateway-sdk-physical-device.md

[lnk-devguide]: ./iot-hub-devguide.md
[lnk-create-hub]: iot-hub-create-through-portal.md 