<properties
    pageTitle="Asjade lüüsi SDK reaal seadme abil | Microsoft Azure'i"
    description="Azure'i asjade lüüsi SDK kiirtutvustus Texases vahendite SensorTag seadme abil andmeid saata asjade jaoturi töötab ka Inteli Edison arvutada mooduli lüüsi kaudu"
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


# <a name="azure-iot-gateway-sdk-beta--send-device-to-cloud-messages-with-a-real-device-using-linux"></a>Azure'i asjade lüüsi SDK (beeta) – Linuxi reaal seade seadme pilve sõnumite saatmine

See samm-sammult juhendi [Bluetooth madal kujutatud valimi] [ lnk-ble-samplecode] näitab, kuidas kasutada [Azure asjade lüüsi SDK] [ lnk-sdk] edasi seadme pilve telemeetria asjade jaoturi füüsilise seadmega ja kuidas marsruutimine füüsilise seadmega asjade keskuse kaudu käskude abil.

Juhendis antakse ülevaade:

* **Arhitektuur**: Bluetooth madal kujutatud valimi arhitektuuri oluline teave.

* **Koostamine ja Käivita**: koostamine ja käivitage valimi etapid.

## <a name="architecture"></a>Arhitektuur

Funktsiooni kiirtutvustus näitab, kuidas luua ja käivitada soovitud asjade lüüsi on Inteli Edison arvutada moodul, mis töötab Linux. Lüüs on loodud asjade lüüsi SDK abil. Valimi kasutab temperatuur andmete kogumise seadme Texases vahendite SensorTag Bluetooth madal kujutatud (tabel).

Kui käivitate lüüsi seda:

- Loob ühenduse SensorTag seadme abil protokoll Bluetooth madal kujutatud (tabel).
- Loob ühenduse asjade jaoturi abil HTTP-protokolli.
- Asjade jaoturi edastab telemeetria SensorTag seadme kaudu.
- Marsruudib käsud asjade jaoturi SensorTag seade.

Lüüsi sisaldab järgmisi mooduleid.

- *Tabel moodul* , mis liidesed tabel seadmega temperatuur andmete saamiseks seadmele käske seade ja saada.
- *Tabel Cloud seadme moodulisse* mis tõlgib JSON sõnumite pilvest tulevad tabel *Tabel mooduli*juhised.
- *Puuraidur moodul* , mis logib lüüsi kõigile sõnumitele.
- Mõne *identiteedi vastendust mooduli* vaste tabel seadme MAC aadressid ja Azure asjade jaoturi seadme vahel.
- Soovitud *asjade jaoturi mooduli* lisatud telemeetria andmeid, asjade jaoturiga ja võtab vastu seadme käsud on asjade keskuse kaudu.
- *Tabel printeri moodul* , mis tõlgendab telemeetria tabel seadmest ja prinditakse vormindatud andmete lubamiseks tõrkeotsing ja silumine konsooli.

### <a name="how-data-flows-through-the-gateway"></a>Kuidas andmevahetus lüüsi kaudu

Järgnev Blokeeri diagramm näitab telemeetria üles andmete meilivoo kohaletoimetamisel.

![](media/iot-hub-gateway-sdk-physical-device/gateway_ble_upload_data_flow.png)

Toimingud, mis telemeetria üksuse võtab reisil asjade jaoturi seadmest, tabel on.

1. Tabel seadme temperatuur näidis loob ja saadab Bluetoothi kaudu soovitud tabel mooduli lüüsi.
2. Tabel mooduli saab valimi ja avaldab selle ta koos seadme MAC-aadress.
3. Identiteedi vastenduse mooduli jätkab teade ja kasutab on sisemine tabel seadme MAC-aadressi tõlkida asjade jaoturi seadme identiteedi (seadme ID ja seadme võtit). See siis avaldab uue sõnumi, mis sisaldab temperatuur näidisandmete, seadme, seadme ID ja seadme võtit MAC-aadress.
4. Asjade jaoturi mooduli saab selle uue sõnumi (loodud identiteedi vastenduse moodul) ja selle avaldab asjade jaoturi.
5. Puuraidur mooduli logib kõigi sõnumite ta ketta faili.

Järgnev Blokeeri diagramm näitab seadme käsk andmete meilivoo kohaletoimetamisel.

![](media/iot-hub-gateway-sdk-physical-device/gateway_ble_command_data_flow.png)

1. Asjade jaoturi mooduli küsitlused perioodiliselt asjade jaoturi käsk uute sõnumite jaoks.
2. Kui asjade jaoturi mooduli saab käsk Uus sõnum, see avaldab see ta.
3. Identiteedi vastenduse mooduli jätkab käsk sõnumi ja kasutab on sisemine tabel tõlkimine asjade jaoturi seadme ID seadme MAC-aadress. See siis avaldab atribuudid kaardi sõnumi sihtrakenduse seadme MAC-aadressi sisaldava uue sõnumi.
4. Tabel pilveteenuses seadme moodulisse jätkab teade ja tõlgib proper tabel juhiseid mooduli tabel. See siis avaldab uue sõnumi.
5. Tabel mooduli jätkab teade ja käivitab I/O käsustikuga suheldes tabel seade.
6. Puuraidur mooduli logib kõigi sõnumite ta ketta faili.

## <a name="prepare-your-hardware"></a>Riistvara ettevalmistamine

Selle õpetuse eeldab, et kasutate [Texases vahendite SensorTag](http://www.ti.com/ww/en/wireless_connectivity/sensortag2015/index.html) seade on Inteli Edison tahvlile.

### <a name="set-up-the-edison-board"></a>Tahvli Edison häälestamine

Enne alustamist veenduge, et saate luua ühenduse seadme Edison traadita võrgu. Edison seadme häälestamine, peate ühenduse hosti. Inteli pakub alustusjuhendid järgmised operatsioonisüsteemid.

- [Inteli Edison arengu tahvli Windowsi 64-bitise rakendusega töö alustamine][lnk-setup-win64].
- [Inteli Edison arengu tahvli Windowsi 32-bitise rakendusega töö alustamine][lnk-setup-win32].
- [Alustamine Inteli Edison arengu tahvli operatsioonisüsteemis Mac OS X][lnk-setup-osx].
- [Alustamine: Intel® Edison tahvli Linux][lnk-setup-linux].

Edison seadme häälestamine ja tutvuge sellega, et teil tuleb täita kõiki neid juhiseid "Get started" artiklite viimases sammus klõpsake "Valige IDE", välja arvatud, mis on mittevajalike praeguse õpetuse. Edison häälestustoimingud lõpus on:

- Vilksatas oma Edison uusim püsivara.
- Funktsiooni Edison hosti järjestikune ühendus luua.
- Käivitage **configure_edison** skript parooli ja teie Edison WiFi lubamine.

### <a name="enable-connectivity-to-the-sensortag-device-from-your-edison-board"></a>Luba Ühenduvus SensorTag seadme kaudu oma Edison tahvlile

Operatsioonisüsteemi valimi, peate esmalt veenduge, et teie Edison tahvli saate luua ühenduse SensorTag seade.

Esmalt peate kontrollima, et teie Edison saate luua ühenduse SensorTag seade.

1. Blokeeringu bluetooth funktsiooni Edison ja kontrollige, kas versiooninumber on **5.37**.
    
    ```
    rfkill unblock bluetooth
    bluetoothctl --version
    ```

2. Käsu **bluetoothctl** . Olete nüüd on interaktiivne bluetooth shell. 

3. Sisestage käsk **power** power bluetooth kontrolleril üles. Peaksite nägema väljundi sarnane:
    
    ```
    [NEW] Controller 98:4F:EE:04:1F:DF edison [default]
    ```

4. Samas on interaktiivne bluetooth shell, sisestage soovitud käsk **kontrolli** Bluetooth-seadme otsimiseks. Peaksite nägema umbes väljundit:
    
    ```
    Discovery started
    [CHG] Controller 98:4F:EE:04:1F:DF Discovering: yes
    ```

5. Hõlpsamaks SensorTag seadme small nuppu (roheline LED peaks flash). Funktsiooni Edison tuleks teada SensorTag seade:
    
    ```
    [NEW] Device A0:E6:F8:B5:F6:00 CC2650 SensorTag
    [CHG] Device A0:E6:F8:B5:F6:00 TxPower: 0
    [CHG] Device A0:E6:F8:B5:F6:00 RSSI: -43
    ```
    
    Selles näites näete, et seade SensorTag MAC-aadress on **A0:E6:F8:B5:F6:00**.

6. Lülitage välja skannimine sisestades käsk **välja skannida** .
    
    ```
    [CHG] Controller 98:4F:EE:04:1F:DF Discovering: no
    Discovery stopped
    ```

7. Seadme SensorTag abil oma MAC-aadressi sisestamise teel ühenduse **ühenduse \<MAC aadress >**. Võtke arvesse, et väljundi näidis on lühendatud.
    
    ```
    Attempting to connect to A0:E6:F8:B5:F6:00
    [CHG] Device A0:E6:F8:B5:F6:00 Connected: yes
    Connection successful
    [CHG] Device A0:E6:F8:B5:F6:00 UUIDs: 00001800-0000-1000-8000-00805f9b34fb
    ...
    [NEW] Primary Service
            /org/bluez/hci0/dev_A0_E6_F8_B5_F6_00/service000c
            Device Information
    ...
    [CHG] Device A0:E6:F8:B5:F6:00 GattServices: /org/bluez/hci0/dev_A0_E6_F8_B5_F6_00/service000c
    ...
    [CHG] Device A0:E6:F8:B5:F6:00 Name: SensorTag 2.0
    [CHG] Device A0:E6:F8:B5:F6:00 Alias: SensorTag 2.0
    [CHG] Device A0:E6:F8:B5:F6:00 Modalias: bluetooth:v000Dp0000d0110
    ```
    
    Märkus: Saate loetleda GATT omadused seade uuesti käsk **loendi-atribuute** .

8. Saate nüüd eraldage käsuga **katkestada** seadmest ja seejärel väljuge bluetooth kest, kasutades käsku **Lõpeta** .
    
    ```
    Attempting to disconnect from A0:E6:F8:B5:F6:00
    Successful disconnected
    [CHG] Device A0:E6:F8:B5:F6:00 Connected: no
    ```

Nüüd olete valmis käivitamiseks tabel lüüsi valimi Edison seadmes.

## <a name="run-the-ble-gateway-sample"></a>Käivitage tabel lüüsi näidis

Teie Edison tabel valimi käitamiseks peate täitma kolme toimingut:

- Kahe valimi seadme konfigureerimine oma asjade keskuses.
- Koostada asjade lüüsi SDK Edison seadmes.
- Konfigureerida ja käivitada tabel valimi seadme Edison.

Kirjutamise ajal asjade lüüsi SDK toetab ainult lüüsid, mis kasutavad tabel moodulid Linux.

### <a name="configure-two-sample-devices-in-your-iot-hub"></a>Oma asjade keskuses kahe valimi seadme konfigureerimine

- [Soovitud asjade keskuse loomine] [ lnk-create-hub] Azure tellimuse, peate oma jaoturi juhendis lõpuleviimiseks nime. Kui teil pole kontot, saate luua [tasuta konto] [ lnk-free-trial] vaid paar minutit.
- Lisage üks seade nimega **SensorTag_01** oma asjade jaoturi ja kirjutage oma id ja seadme peamiste. Saate selle [seadme Exploreri või iothub-Exploreri] [ lnk-explorer-tools] tööriistad asjade jaoturi eelmises juhises loodud selle seadme lisamiseks ja tuua võtmed. Kasutatav seade on vastendamine SensorTag seadme lüüsi konfigureerimisel.

### <a name="build-the-iot-gateway-sdk-on-your-edison-device"></a>Koostage asjade lüüsi SDK Edison seadmes

Klõpsake soovitud Edsion **git** versiooni ei toeta alamoodulite puhul. Täielik allika allalaadimiseks asjade lüüsi SDK abil soovitud Edison on teil kaks võimalust:

- Variant #1: Klooni [Azure'i asjade lüüsi SDK] [ lnk-sdk] hoidla oma Edison kohta ja seejärel käsitsi klooni on iga submodule hoidla.
- Variant #2: Klooni [Azure'i asjade lüüsi SDK] [ lnk-sdk] hoidla töölaua seadmes, kuhu **git** toetab alamoodulite puhul ja seejärel kopeerige täieliku hoidla koos alamoodulite peale teie Edison puhul.

Kui valite suvandi #2, kasutage klooni asjade lüüsi SDK ja selle alamoodulite puhul **git** järgmised käsud:

```
git clone --recursive https://github.com/Azure/azure-iot-gateway-sdk.git 
git submodule update --init --recursive
```

Kogu kohaliku hoidla peaks ühe arhiivi faili zip siis enne selle Edison kopeerimist. Saate kasutada näiteks **pscp** , mis on kaasas **Putty** arhiivifaili kopeerimiseks on Edison kasuliku. Näiteks:

```
pscp .\gatewaysdk.zip root@192.168.0.45:/home/root
```

Kui teil on oma Edison asjade lüüsi SDK hoidla täielik koopia, saate koostada selle kausta, mis sisaldab SDK kaudu järgmine käsk:

```
./tools/build.sh
```

### <a name="configure-and-run-the-ble-sample-on-your-edison-device"></a>Konfigureerida ja käivitada tabel valimi Edison seadmes

Bootstrap ja käivitage valimi, peate iga moodul, mis osaleb lüüsi konfigureerimine. Selle konfiguratsiooni on esitatud JSON-failis ja teil on vaja konfigureerida kõik viis osalevate moodulid. On esitatud hoidlas, nimetatakse **gateway_sample.json** , mille abil saate oma konfiguratsioonifail hoone alguspunktiks JSON näidisfaili. See fail on kaustas **näidised/ble_gateway_hl/src** kohaliku eksemplari asjade lüüsi SDK hoidla.

Järgmistes jaotistes kirjeldatakse, kuidas muuta see tabel valimi konfiguratsioonifail ja Oletame, et asjade lüüsi SDK hoidla on selle **/home/root/azure-iot-gateway-sdk /** seadme Edison kausta. Kui hoidla on mujal, teed vastavalt kohandada:

#### <a name="logger-configuration"></a>Puuraidur konfigureerimine

Eeldades, et lüüsi hoidla asub kaustas **/home/root/azure-iot-gateway-sdk /**, konfigureerida moodulit puuraidur järgmiselt:

```json
{
    "module name": "logger",
    "module path": "/home/root/azure-iot-gateway-sdk/build/modules/logger/liblogger_hl.so",
    "args":
    {
        "filename":"/home/root/gw_logger.log"
    }
}
```

#### <a name="ble-module-configuration"></a>Tabel mooduli seadistamine

Proovi konfiguratsiooni tabel seadme eeldab Texases vahendite SensorTag seade. Standard tabel seadet, mis kasutab GATT, mis on perifeerne peaks töötamise ajal, kuid te peate GATT iseloomustav ID-d ja (juhised kirjutamine) andmete värskendamine. Seadme SensorTag MAC-aadressi lisamiseks tehke järgmist. 

```json
{
  "module name": "SensorTag",
  "module path": "/home/root/azure-iot-gateway-sdk/build/modules/ble/libble_hl.so",
  "args": {
    "controller_index": 0,
    "device_mac_address": "<<AA:BB:CC:DD:EE:FF>>",
    "instructions": [
      {
        "type": "read_once",
        "characteristic_uuid": "00002A24-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A25-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A26-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A27-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A28-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "read_once",
        "characteristic_uuid": "00002A29-0000-1000-8000-00805F9B34FB"
      },
      {
        "type": "write_at_init",
        "characteristic_uuid": "F000AA02-0451-4000-B000-000000000000",
        "data": "AQ=="
      },
      {
        "type": "read_periodic",
        "characteristic_uuid": "F000AA01-0451-4000-B000-000000000000",
        "interval_in_ms": 1000
      },
      {
        "type": "write_at_exit",
        "characteristic_uuid": "F000AA02-0451-4000-B000-000000000000",
        "data": "AA=="
      }
    ]
  }
}
```

#### <a name="iot-hub-module"></a>Asjade jaoturi mooduli

Lisage oma asjade jaoturi nimi. Järelliite väärtus on tavaliselt **azure-devices.net**:

```json
{
  "module name": "IoTHub",
  "module path": "/home/root/azure-iot-gateway-sdk/build/modules/iothub/libiothub_hl.so",
  "args": {
    "IoTHubName": "<<Azure IoT Hub Name>>",
    "IoTHubSuffix": "<<Azure IoT Hub Suffix>>",
    "Transport": "HTTP"
  }
}
```

#### <a name="identity-mapping-module-configuration"></a>Identiteedi vastenduse mooduli seadistamine

MAC-aadress teie SensorTag ja seadme ID-d ja klahvi lisasite oma asjade jaoturi **SensorTag_01** seadme lisamiseks tehke järgmist.

```json
{
  "module name": "mapping",
  "module path": "/home/root/azure-iot-gateway-sdk/build/modules/identitymap/libidentity_map_hl.so",
  "args": [
    {
      "macAddress": "<<AA:BB:CC:DD:EE:FF>>",
      "deviceId": "<<Azure IoT Hub Device ID>>",
      "deviceKey": "<<Azure IoT Hub Device Key>>"
    }
  ]
}
```

#### <a name="ble-printer-module-configuration"></a>Tabel printeri mooduli seadistamine

```json
{
    "module name": "BLE Printer",
    "module path": "/home/root/azure-iot-gateway-sdk/build/samples/ble_gateway_hl/ble_printer/libble_printer.so",
    "args": null
}
```

#### <a name="routing-configuration"></a>Marsruutimise konfigureerimine

Pärast konfiguratsioon tagab järgmist:
- **Puuraidur** mooduli saab ja logige kõigile sõnumitele.
- **SensorTag** moodul saadab sõnumid **vastenduse** ja **Tabel printeri** moodulid.
- **Vastenduse** mooduli saadab sõnumite **IoTHub** mooduli saadetakse teie asjade jaoturi.
- **IoTHub** moodul saadab sõnumeid tagasi **vastenduse** mooduli.
- **Vastenduse** mooduli saadab sõnumeid tagasi **SensorTag** mooduli.

```json
"links" : [
    {"source" : "*", "sink" : "Logger" },
    {"source" : "SensorTag", "sink" : "mapping" },
    {"source" : "SensorTag", "sink" : "BLE Printer" },
    {"source" : "mapping", "sink" : "IoTHub" },
    {"source" : "IoTHub", "sink" : "mapping" },
    {"source" : "mapping", "sink" : "SensorTag" }
  ]
```

Valimi käivitamiseks käivitate **ble_gateway_hl** kahendarvu, läbides JSON konfiguratsiooni faili tee. Kui kasutasite **gateway_sample.json** fail, käsk käivitada näeb välja umbes järgmine:

```
./build/samples/ble_gateway_hl/ble_gateway_hl ./samples/ble_gateway_hl/src/gateway_sample.json
```

Võimalik, et peate SensorTag seadme oleks leitavaks enne valimi väike nuppu.

Valimi käivitamisel, saate selle [seadme Exploreri või iothub-Exploreri] [ lnk-explorer-tools] tööriista lüüsi edastab SensorTag seadmest sõnumite jälgimine.

## <a name="send-cloud-to-device-messages"></a>Pilveteenuse-seadme sõnumite saatmine

Tabel mooduli toetab samuti saatmise Azure'i asjade jaoturi seadmele juhiseid. Saate [Azure'i asjade jaoturi seade Explorer](https://github.com/Azure/azure-iot-sdks/blob/master/tools/DeviceExplorer/doc/how_to_use_device_explorer.md) või [Asjade jaoturi Exploreri](https://github.com/Azure/azure-iot-sdks/tree/master/tools/iothub-explorer) tabel lüüsi mooduli edastab tabel seadme JSON sõnumite saatmiseks. Näiteks kui kasutate Texases vahendite SensorTag seade seejärel saate saata JSON järgmistest teadetest seadmele asjade keskuse kaudu.

- Lähtestada selle jäävat aega ja kõik LED (need välja lülitada)

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "AA=="
    }
    ```

- Nimega "remote" I/O konfigureerimine

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA66-0451-4000-B000-000000000000",
      "data": "AQ=="
    }
    ```

- Punane LED sisselülitamine

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "AQ=="
    }
    ```

- Rohelise LED sisselülitamine

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "Ag=="
    }
    ```

- Funktsiooni summeri sisselülitamine

    ```json
    {
      "type": "write_once",
      "characteristic_uuid": "F000AA65-0451-4000-B000-000000000000",
      "data": "BA=="
    }
    ```

HTTP-protokolli ühendamine asjade jaoturi abil seadme vaikekäitumine on kontrollida iga 25 minutit käsk Uus. Seetõttu, kui saadate mitu eraldi käsku peate ootama minutit seadme iga käsk saada.

> [AZURE.NOTE] Lüüsi kontrollib uute käskude iga kord, kui see algab nii, et saate jõustada seda protsessi lõpetamine ja alustades lüüsi käsu.

## <a name="next-steps"></a>Järgmised sammud

Kui soovite asjade lüüsi SDK täpsemate mõistmiseks ja koodi näited katsetada, külastage järgmist arendaja õpetused ja ressursse:

- [Azure'i asjade lüüsi SDK][lnk-sdk]

Asjade jaoturi võimaluste täpsemaks analüüsimiseks järgmistest teemadest.

- [Arendaja juhend][lnk-devguide]

<!-- Links -->
[lnk-ble-samplecode]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/samples/ble_gateway_hl
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-explorer-tools]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/manage_iot_hub.md
[lnk-setup-win64]: https://software.intel.com/get-started-edison-windows
[lnk-setup-win32]: https://software.intel.com/get-started-edison-windows-32
[lnk-setup-osx]: https://software.intel.com/get-started-edison-osx
[lnk-setup-linux]: https://software.intel.com/get-started-edison-linux
[lnk-sdk]: https://github.com/Azure/azure-iot-gateway-sdk/


[lnk-devguide]: iot-hub-devguide.md
[lnk-create-hub]: iot-hub-create-through-portal.md 
