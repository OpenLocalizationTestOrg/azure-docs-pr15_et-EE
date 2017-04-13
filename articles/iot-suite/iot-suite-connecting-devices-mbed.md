<properties
   pageTitle="Ühendage seade C kasutamine mbed | Microsoft Azure'i"
   description="Selles artiklis kirjeldatakse Azure'i asjade komplekti eelkonfigureeritud remote jälgimisega seotud lahenduse saate rakendust kirjutatud C mbed seadmes töötab seadme ühendada."
   services=""
   suite="iot-suite"
   documentationCenter="na"
   authors="dominicbetts"
   manager="timlt"
   editor=""/>

<tags
   ms.service="iot-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/05/2016"
   ms.author="dobett"/>


# <a name="connect-your-device-to-the-remote-monitoring-preconfigured-solution-mbed"></a>Ühendage seade remote jälgimise eelkonfigureeritud lahendus (mbed)

[AZURE.INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

## <a name="build-and-run-the-c-sample-solution"></a>Koostamine ja C valimi lahenduse käivitamine

Järgmised juhised on kirjeldatud juhised ühenduse loomine mõne [Mbed lubatud Freescale FRDM-K64F] [ lnk-mbed-home] seadme remote jälgimise lahendus.

### <a name="connect-the-mbed-device-to-your-network-and-desktop-machine"></a>Ühendage seade mbed võrk ja töölaua arvuti

1. Ühendage seade mbed võrgu Ethernet-kaabli abil. See toiming on vaja, kuna valimi rakenduse jaoks on vaja.

2. Vt [Alustamine mbed] [ lnk-mbed-getstarted] oma Lauaarvuti mbed seadme ühendamiseks.

3. Kui teie töölaua Arvutis töötab Windows, lugege teemat [Arvuti konfiguratsioon] [ lnk-mbed-pcconnect] järjestikune port juurdepääsu seadme mbed konfigureerida.

### <a name="create-an-mbed-project-and-import-the-sample-code"></a>Luua mbed projekti ja importida proovi kood

1. Avage oma veebibrauseris mbed.org [arendaja sait](https://developer.mbed.org/). Kui te pole veel sisse loginud, näete suvandit Loo konto (see on tasuta). Vastasel korral logige sisse oma konto identimisteave. Klõpsake lehe paremas ülanurgas **koostaja** . See toiming viib teid *tööruumi* kasutajaliidese.

2. Veenduge, et kasutate riistvara platvormi kuvatakse akna paremas ülanurgas või valida oma riistvara platform parempoolses ülanurgas ikooni.

3. Menüü peamine nuppu **impordi** . Klõpsake importida kõrval mbed maakera logo URL-i linki **klõpsake siin** .

    ![][6]

4. Sisestage linki proovi kood https://developer.mbed.org/users/AzureIoTClient/code/remote_monitoring/ hüpikakna, seejärel nuppu **impordi**.

    ![][7]

5. Mbed koostaja aknas näete, et selle projekti importida impordi erinevate teekide. Mõned on esitatud ja säilitada Azure'i asjade meeskonna ([azureiot_common](https://developer.mbed.org/users/AzureIoTClient/code/azureiot_common/), [iothub_client](https://developer.mbed.org/users/AzureIoTClient/code/iothub_client/), [iothub_amqp_transport](https://developer.mbed.org/users/AzureIoTClient/code/iothub_amqp_transport/), [azure_uamqp](https://developer.mbed.org/users/AzureIoTClient/code/azure_uamqp/)), samas kui teised on kolmanda osapoole teeke mbed teekide kataloogi saadaval.

    ![][8]

6. Avage fail remote_monitoring\remote_monitoring.c ja otsige üles fail järgmine kood:

    ```
    static const char* deviceId = "[Device Id]";
    static const char* deviceKey = "[Device Key]";
    static const char* hubName = "[IoTHub Name]";
    static const char* hubSuffix = "[IoTHub Suffix, i.e. azure-devices.net]";
    ```

7. Asenda [seadme Id] ja [seadme klahv], teie seadme andmetega ühenduse loomiseks oma asjade jaoturi valimi programm. Kasutage Hostname jaoturi asjade asendamiseks IoTHub nimi ja kohatäidete [sufiks IoTHub, st Azure'i devices.net]. Näiteks kui teie asjade jaoturi Hostname on **contoso.azure-devices.net**, **contoso** on **hubName** ja kõik pärast **hubSuffix**on:

    ```
    static const char* deviceId = "mydevice";
    static const char* deviceKey = "mykey";
    static const char* hubName = "contoso";
    static const char* hubSuffix = "azure-devices.net";
    ```

    ![][9]

### <a name="walk-through-the-code"></a>Tutvustavad kood

Kui olete huvitatud programmi tööpõhimõte, selles jaotises kirjeldatakse olulisi osa proovi kood. Kui soovite lihtsalt koodi käivitamiseks, vahele jätta ees, [luua](#buildandrun)ja käivitada programmi.

#### <a name="defining-the-model"></a>Kui seate mudeli määratlemine

See näide kasutab [serializer] [ lnk-serializer] teegi määratleda mudelit, mis määrab sõnumite seadme saate asjade jaoturi saata ja vastu võtta keskuse asjade Interneti kaudu. Selles näites **Contoso** nimeruumi määratleb **termostaat** mudelit, mis määrab **temperatuuri**, **ExternalTemperature**ja **niiskuse** telemeetria andmed koos metaandmeid, nt seadme ID-d, seadme atribuutide ja käsud, mis vastab seadme:

```
BEGIN_NAMESPACE(Contoso);

DECLARE_STRUCT(SystemProperties,
    ascii_char_ptr, DeviceID,
    _Bool, Enabled
);

DECLARE_STRUCT(DeviceProperties,
ascii_char_ptr, DeviceID,
_Bool, HubEnabledState
);

DECLARE_MODEL(Thermostat,

    /* Event data (temperature, external temperature and humidity) */
    WITH_DATA(int, Temperature),
    WITH_DATA(int, ExternalTemperature),
    WITH_DATA(int, Humidity),
    WITH_DATA(ascii_char_ptr, DeviceId),

    /* Device Info - This is command metadata + some extra fields */
    WITH_DATA(ascii_char_ptr, ObjectType),
    WITH_DATA(_Bool, IsSimulatedDevice),
    WITH_DATA(ascii_char_ptr, Version),
    WITH_DATA(DeviceProperties, DeviceProperties),
    WITH_DATA(ascii_char_ptr_no_quotes, Commands),

    /* Commands implemented by the device */
    WITH_ACTION(SetTemperature, int, temperature),
    WITH_ACTION(SetHumidity, int, humidity)
);

END_NAMESPACE(Contoso);
```

Mudeli mõistet on **SetTemperature** ja **SetHumidity** käsud, mis vastab seadme määratlusi.

```
EXECUTE_COMMAND_RESULT SetTemperature(Thermostat* thermostat, int temperature)
{
    (void)printf("Received temperature %d\r\n", temperature);
    thermostat->Temperature = temperature;
    return EXECUTE_COMMAND_SUCCESS;
}

EXECUTE_COMMAND_RESULT SetHumidity(Thermostat* thermostat, int humidity)
{
    (void)printf("Received humidity %d\r\n", humidity);
    thermostat->Humidity = humidity;
    return EXECUTE_COMMAND_SUCCESS;
}
```

#### <a name="connecting-the-model-to-the-library"></a>Mudeli ühenduse loomine teegis

Funktsioonide **sendMessage** ja **IoTHubMessage** on trafaretset koodi saatmise telemeetria seadmest ja sõnumite asjade keskuse kaudu ühenduse käsk käitleja.

#### <a name="the-remotemonitoringrun-function"></a>Funktsiooni remote_monitoring_run

**Programmi põhifunktsiooni** käivitab funktsiooni **remote_monitoring_run** rakenduse käivitamisel seadme käitumine asjade jaoturi seadme klient käivitada. Funktsioon **remote_monitoring_run** enamasti koosneb Pesastatud funktsioonide paari.

- **platvormi\_käivitamise** ja **platvormi\_deinit** platvormi kohased käivitamine ja sulgemine toiminguid.
- **serializer\_käivitamise** ja **serializer\_deinit** lähtestamine ja tühistage lähtestada serializer teek.
- **IoTHubClient\_Loo** ja **IoTHubClient\_Destroy** kliendi pidet, **iotHubClientHandle**, seadme mandaadi abil oma asjade jaoturi ühenduse luua.

Funktsiooni **remote_monitoring_run** põhi jaotises programm sooritab järgmiste toimingute abil **iotHubClientHandle** pidet.

- Loob eksemplari Contoso termostaat mudeli ja häälestab sõnumi kontekstiatribuuti kaks käsud.
- Saadab selle seadme, sh käsud, mis toetab oma asjade jaoturi abil serializer teegi kohta. Kui jaoturi saab teade, selle vahetatakse seadme olek armatuurlaua **Ootel** **töötab**.
- Käivitab **ajal** tsükkel, mis saadab temperatuur, temperatuur ja niiskus väärtuste asjade jaoturi iga teine.

Viide, on valimi **DeviceInfo** sõnum saadetakse asjade jaoturi käivitamisel:

```
{
  "ObjectType":"DeviceInfo",
  "Version":"1.0",
  "IsSimulatedDevice":false,
  "DeviceProperties":
  {
    "DeviceID":"mydevice01", "HubEnabledState":true
  }, 
  "Commands":
  [
    {"Name":"SetHumidity", "Parameters":[{"Name":"humidity","Type":"double"}]},
    { "Name":"SetTemperature", "Parameters":[{"Name":"temperature","Type":"double"}]}
  ]
}
```

Viite siit saadetud asjade jaoturi valimi **telemeetria** sõnum.

```
{"DeviceId":"mydevice01", "Temperature":50, "Humidity":50, "ExternalTemperature":55}
```

Viide, siin on näide asjade jaoturi saadud **käsk** :

```
{
  "Name":"SetHumidity",
  "MessageId":"2f3d3c75-3b77-4832-80ed-a5bb3e233391",
  "CreatedTime":"2016-03-11T15:09:44.2231295Z",
  "Parameters":{"humidity":23}
}
```

<a id="buildandrun"/>
### <a name="build-and-run-the-program"></a>Koostamine ja programmi käivitamine

1. Klõpsake **kompileerida** programmi koostamiseks. Saate turvaline hoiatusi ignoreerida, kuid kui koostamine genereeritud tõrgete, lahendada need enne jätkamist.

2. Kui koostamine õnnestus, mbed koostaja veebisaidi loob .bin faili nimi oma projekti ja laadib teie kohalikus arvutis. Kopeerige fail .bin seade. Seadme .bin faili salvestamise põhjustab seadme taaskäivitamine ja .bin fail sisaldab programmi käivitamine. Programmi saate igal ajal taaskäivitage käsitsi, vajutades nuppu Lähtesta mbed seadmes.

3. Ühenduse seadme abil SSH kliendi rakendus, nt PuTTY. Saate määrata järjestikune pordi seadme kasutab, märkides ruudu Windows Seadmehaldur.

    ![][11]

4. Klõpsake pahtel, **järjestikune** ühendusetüübi. Seadme tavaliselt ühendab 9600 boodi, nii et sisestage 9600 **kiiruse** väljal. Klõpsake nuppu **Ava**.

5. Programm käivitub käivitamist. Võib-olla peate lähtestamine tahvli (vajutage klahvikombinatsiooni CTRL + tühik või vajutage selle tahvli nuppu Lähtesta) kui programm ei käivitu automaatselt ühenduse loomisel.

    ![][10]

[AZURE.INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]


[6]: ./media/iot-suite-connecting-devices-mbed/mbed1.png
[7]: ./media/iot-suite-connecting-devices-mbed/mbed2a.png
[8]: ./media/iot-suite-connecting-devices-mbed/mbed3a.png
[9]: ./media/iot-suite-connecting-devices-mbed/suite6.png
[10]: ./media/iot-suite-connecting-devices-mbed/putty.png
[11]: ./media/iot-suite-connecting-devices-mbed/mbed6.png

[lnk-mbed-home]: https://developer.mbed.org/platforms/FRDM-K64F/
[lnk-mbed-getstarted]: https://developer.mbed.org/platforms/FRDM-K64F/#getting-started-with-mbed
[lnk-mbed-pcconnect]: https://developer.mbed.org/platforms/FRDM-K64F/#pc-configuration
[lnk-serializer]: https://azure.microsoft.com/documentation/articles/iot-hub-device-sdk-c-intro/#serializer
