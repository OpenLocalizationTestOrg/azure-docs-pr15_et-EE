<properties
 pageTitle="Seadme teabe metaandmete remote jälgimise lahendus | Microsoft Azure'i"
 description="Azure'i asjade kirjeldus eelkonfigureeritud lahenduse jälgida ja selle arhitektuur."
 services=""
 suite="iot-suite"
 documentationCenter=""
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-suite"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/12/2016"
 ms.author="dobett"/>

# <a name="device-information-metadata-in-the-remote-monitoring-preconfigured-solution"></a>Seadme teabe metaandmete järelevalve Remote'i eelkonfigureeritud lahendus

Azure'i asjade komplekti remote jälgimise eelkonfigureeritud lahendus näitab lähenemisviisi jaoks seadme metaandmete haldamine. Selles artiklis kirjeldatakse seda lahendust suunab võimaldavad teil mõista lähenemine:

- Millist seadet metaandmete lahendus talletab.
- Kuidas lahendus haldab seadme metaandmete.

## <a name="context"></a>Kontekst

Järelevalve Remote'i eelkonfigureeritud lahendus kasutab [Azure asjade jaoturi] [ lnk-iot-hub] lubamiseks seadmetes andmeid saata pilve. Asjade jaoturi sisaldab [seadme identiteedi registri] [ lnk-identity-registry] asjade jaoturi juurdepääsu. Asjade jaoturi seadme identiteedi register on eraldi jälgimise lahendus konkreetse *seadme registri* seadme teabe metaandmete talletava serveri. Remote jälgimise lahendus kasutab mõnda [DocumentDB] [ lnk-docdb] andmebaasi rakendada oma seadme registri metaandmete seadme teabe talletamiseks. [Microsoft Azure'i asjade viide arhitektuur] [ lnk-ref-arch] kirjeldatakse tüüpilised asjade lahenduse seadme registri roll.

> [AZURE.NOTE] Järelevalve Remote'i eelkonfigureeritud lahendus hoiab seadme identiteedi registri sünkroonis seadme register. Sama seadme id abil nii kordumatult iga teie asjade jaoturiga ühendatud seadet.

[Asjade jaoturi seadme halduse preview] [ lnk-dm-preview] asjade jaoturi, mis sarnanevad selles artiklis kirjeldatud seadme teabe haldusfunktsioonid funktsioonide lisatakse. Praegu remote jälgimise lahendus kasutab ainult üldiselt kättesaadav (GA) funktsioonid asjade keskuses.

## <a name="device-information-metadata"></a>Seadme teabe metaandmed

Seadme teabe metaandmete JSON dokument salvestatakse seadme registri DocumentDB andmebaasi struktuur on järgmine:

```
{
  "DeviceProperties": {
    "DeviceID": "deviceid1",
    "HubEnabledState": null,
    "CreatedTime": "2016-04-25T23:54:01.313802Z",
    "DeviceState": "normal",
    "UpdatedTime": null
    },
  "SystemProperties": {
    "ICCID": null
  },
  "Commands": [],
  "CommandHistory": [],
  "IsSimulatedDevice": false,
  "id": "fe81a81c-bcbc-4970-81f4-7f12f2d8bda8"
}
```

- **DeviceProperties**: seadme ise kirjutab atribuutidest ja seade on asutuse andmed. Muud näide seadme atribuutide kaasata tootja, mudel telefoninumber ja järjenumbri. 
- **DeviceID**: seadme kordumatu ID-ga. See väärtus on sama registris asjade jaoturi seadme identiteedi.
- **HubEnabledState**: asjade keskuses seadme olek. See algselt väärtuseks **null** kuni seadme esmalt ühendab. Lahendus portaalis esitatud **tühiväärtus** mobiilsideseadme "registreeritud, kuid ei esine."
- **CreatedTime**: aeg seade on loodud.
- **DeviceState**: olek teatatud seadme abil.
- **UpdatedTime**: aega seadme viimati värskendati lahenduse portaali kaudu.
- **SystemProperties**: lahenduse portaali kirjutab süsteemi atribuudid ja seade ei ole neid atribuute. Mõni näide süsteemi atribuut on **ICCID** kui lahendus on lubatud koos ja SIM lubatud seadmete haldamine teenusega ühenduse.
- **Käsud**: käskude loendi seade toetab. Seadme esitab seda teavet lahendus.
- **CommandHistory**: Käsuloend, mis on saadetud remote jälgimise lahendus seade ja need käsud olekut.
- **IsSimulatedDevice**: lipuga, mille abil tuvastatakse jäljendatud seade seadme.
- **ID**: selle seadme dokumendi DocumentDB ainuidentifikaator.

> [AZURE.NOTE] Seadme teave võib sisaldada ka metaandmete seade saadab asjade jaoturi telemeetria kirjeldamiseks. Remote jälgimise lahendus kasutab seda telemeetria metaandmete kohandada, kuidas armatuurlaual kuvatakse [dünaamiline telemeetria][lnk-dynamic-telemetry].

## <a name="lifecycle"></a>Elutsükkel

Kui loote esmalt seadme lahenduse portaalis, loob lahendus oma seadme registri, nagu on näidatud varem. Palju teavet on esialgu stubbed ja **HubEnabledState** väärtuseks **null**. Selles etapis lahendus loob ka kirje seade seadme identiteedi registri, mis loob seade kasutab autentimiseks asjade jaoturi võtmed.

Kui seade loob esmalt lahenduse, see saadab seadme sõnumile. Seadme teade sisaldab seadme atribuutide, nt seadme tootja, mudel arv ja järjenumbri. Seadme sõnumile ka Käsuloend, mis sisaldab ka teavet selle kohta, mis tahes käsu parameetrid toetab seade. Kui lahendus saab teade, selle värskendamist seadme teabe metaandmete registris seade.

### <a name="view-and-edit-device-information-in-the-solution-portal"></a>Saate vaadata ja muuta seadme lahenduse portaalis

Seadmete loend lahenduse portaalis kuvatakse järgmised seadme atribuudid veergudena: **olek** **DeviceId** **tootja**, **Mudel arv**, **järjenumbri**, **püsivara**, **platvormi**, **protsessor**ja **RAM installitud**. Seadme atribuutide **laiuskraad** ja **pikkuskraad** drive armatuurlaual Bingi kaardi kohta. 

![Seadmete loend][img-device-list]

Kui klõpsate nuppu **Redigeeri** lahenduse portaalis üksikasjapaanil **Seade** , saate redigeerida kõiki neid atribuute. Nende atribuutide redigeerimine värskendatakse seadme DocumentDB andmebaasi kirje. Juhul, kui seadme saadab sõnumi värskendatud seadme teave, kirjutab lahenduse portaalis tehtud muudatused. Kuna ainult seade on üle neid atribuute ei saa redigeerida **DeviceId**, **Hostname**, **HubEnabledState**, **CreatedTime**, **DeviceState**ja **UpdatedTime** atribuudid lahenduse portaalis.

![Seadme redigeerimine][img-device-edit]

Lahendus portaali abil saate seadme eemaldamiseks teie lahendus. Seadme eemaldamisel lahendus lahendus seadme registri seadme teabe metaandmete eemaldab ja eemaldab seadme kirje registris asjade jaoturi seadme identiteedi. Seadme eemaldamiseks, saate selle keelata.

![Seadme eemaldamine][img-device-remove]

## <a name="device-information-message-processing"></a>Seadme teabe sõnumi töötlemine

Seadme teabe sõnumid saadetakse seadme erinevad telemeetria sõnumeid. Seadme teavet sõnumite hulka kuuluvad seadme atribuutide, seadme saavad vastata käsud ja mis tahes käsu ajalugu näiteks järgmist teavet. Asjade jaoturi ise ei ole seadme sõnumis sisalduvate metaandmete ja töötleb sõnumi mis tahes seadme pilve sõnumi töötleb samal viisil. Klõpsake remote jälgimise lahendus on [Azure voo Analytics] [ lnk-stream-analytics] (Ash) töö loeb sõnumite asjade keskuse kaudu. **DeviceInfo** voo analytics töö filtrite sõnumeid, mis sisaldavad **"Objektitüüp": "DeviceInfo"** ja edastab need **EventProcessorHost** hosti eksemplari, mis töötab web töö. Loogika **EventProcessorHost** eksemplari kasutab seadme id konkreetse seadme DocumentDB kirje otsimine ja värskendamine kirje. Seadme registri kirje sisaldab nüüd teavet seadme atribuutide, käsud ja käsku ajalugu.

> [AZURE.NOTE] Seadme sõnumile on standardne seadme pilve sõnum. Lahendus eristatakse seadme teabe sõnumitele ja telemeetria ASA päringute abil.

## <a name="example-device-information-records"></a>Näide seadme teabe kirjed

Järelevalve Remote'i eelkonfigureeritud lahendus kasutab kahte tüüpi seade teavet kirjeid: kirjete juurutatud lahenduse ja kohandatud seadmete lahendus ühenduse loomisel kirjeid jäljendatud seadmete jaoks.

### <a name="simulated-device"></a>Jäljendatud seade

Järgmises näites on kujutatud JSON seadme teabe kirje jäljendatud seade. Kirje on väärtus **UpdatedTime**, mis näitab, kas seade on saatnud sõnumi **DeviceInfo** asjade jaoturi seadmine. Kirje sisaldab mõnda levinud seadme atribuutide, määratleb kuus käske jäljendatud seadmed toetavad ja on **IsSimulatedDevice** lipu seadmine **1**.

```
{
  "DeviceProperties": {
    "DeviceID": "SampleDevice001_455",
    "HubEnabledState": true,
    "CreatedTime": "2016-01-26T19:02:01.4550695Z",
    "DeviceState": "normal",
    "UpdatedTime": "2016-06-01T15:28:41.8105157Z",
    "Manufacturer": "Contoso Inc.",
    "ModelNumber": "MD-369",
    "SerialNumber": "SER9009",
    "FirmwareVersion": "1.39",
    "Platform": "Plat-34",
    "Processor": "i3-2191",
    "InstalledRAM": "3 MB",
    "Latitude": 47.583582,
    "Longitude": -122.130622
  },
  "Commands": [
    {
      "Name": "PingDevice",
      "Parameters": null
    },
    {
      "Name": "StartTelemetry",
      "Parameters": null
    },
    {
      "Name": "StopTelemetry",
      "Parameters": null
    },
    {
      "Name": "ChangeSetPointTemp",
      "Parameters": [
        {
          "Name": "SetPointTemp",
          "Type": "double"
        }
      ]
    },
    {
      "Name": "DiagnosticTelemetry",
      "Parameters": [
        {
          "Name": "Active",
          "Type": "boolean"
        }
      ]
    },
    {
      "Name": "ChangeDeviceState",
      "Parameters": [
        {
          "Name": "DeviceState",
          "Type": "string"
        }
      ]
    }
  ],
  "CommandHistory": [],
  "IsSimulatedDevice": 1,
  "Version": "1.0",
  "ObjectType": "DeviceInfo",
  "IoTHub": {
    "MessageId": null,
    "CorrelationId": null,
    "ConnectionDeviceId": "SampleDevice001_455",
    "ConnectionDeviceGenerationId": "635894317219942540",
    "EnqueuedTime": "0001-01-01T00:00:00",
    "StreamId": null
  },
  "SystemProperties": {
    "ICCID": null
  },
  "id": "7101c002-085f-4954-b9aa-7466980a2aaf"
}
```

### <a name="custom-device"></a>Kohandatud seade

Järgmises näites kuvatakse JSON seadme teabe kirje kohandatud seade ja on **IsSimulatedDevice** lipp väärtuseks **0**. Näete, et kohandatud seade toetab kahte käsud ja lahenduse portaali saatnud **SetTemperature** käsu seadmele:

```
{
  "DeviceProperties": {
    "DeviceID": "mydevice01",
    "HubEnabledState": true,
    "CreatedTime": "2016-03-28T21:05:06.6061104Z",
    "DeviceState": "normal",
    "UpdatedTime": "2016-06-07T22:05:34.2802549Z"
  },
  "SystemProperties": {
    "ICCID": null
  },
  "Commands": [
    {
      "Name": "SetHumidity",
      "Parameters": [
        {
          "Name": "humidity",
          "Type": "int"
        }
      ]
    },
    {
      "Name": "SetTemperature",
      "Parameters": [
        {
          "Name": "temperature",
          "Type": "int"
        }
      ]
    }
  ],
  "CommandHistory": [
    {
      "Name": "SetTemperature",
      "MessageId": "2a0cec61-5eca-4de7-92dc-9c0bc4211c46",
      "CreatedTime": "2016-06-07T21:05:18.140796Z",
      "Parameters": {
        "temperature": 20
      },
      "UpdatedTime": "2016-06-07T21:05:18.716076Z",
      "Result": "Expired"
    }
  ],
  "IsSimulatedDevice": 0,
  "id": "6184ae0f-2d94-4fbd-91cd-4b193aecc9d1",
  "ObjectType": "DeviceInfo",
  "Version": "1.0",
  "IoTHub": {
    "MessageId": null,
    "CorrelationId": null,
    "ConnectionDeviceId": "SampleCustom",
    "ConnectionDeviceGenerationId": "635947959068246845",
    "EnqueuedTime": "0001-01-01T00:00:00",
    "StreamId": null
  }
}
```

Järgnevalt kuvatakse JSON **DeviceInfo** sõnum saadetakse seadme teabe metaandmete värskendamine seade.

```
{ "ObjectType":"DeviceInfo",
  "Version":"1.0",
  "IsSimulatedDevice":false,
  "DeviceProperties": { "DeviceID":"mydevice01", "HubEnabledState":true },
  "Commands": [
    {"Name":"SetHumidity", "Parameters":[{"Name":"humidity","Type":"double"}]},
    {"Name":"SetTemperature", "Parameters":[{"Name":"temperature","Type":"double"}]}
  ]
}
```

## <a name="next-steps"></a>Järgmised sammud

Nüüd, kui olete lõpetanud, saate kohandada eelkonfigureeritud lahenduste Õppekeskuse, saate uurida muude funktsioonide ja võimaluste kohta asjade komplekti eelkonfigureeritud lahendused:

- [Eelkonfigureeritud sõnastikupõhise hooldustööd lahenduse ülevaade][lnk-predictive-overview]
- [Korduma kippuvad küsimused asjade komplekti][lnk-faq]
- [Asjade turvalisuse kohalt kaudu][lnk-security-groundup]



<!-- Images and links -->
[img-device-list]: media/iot-suite-remote-monitoring-device-info/image1.png
[img-device-edit]: media/iot-suite-remote-monitoring-device-info/image2.png
[img-device-remove]: media/iot-suite-remote-monitoring-device-info/image3.png

[lnk-iot-hub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-identity-registry]: ../iot-hub/iot-hub-devguide-identity-registry.md
[lnk-docdb]: https://azure.microsoft.com/documentation/services/documentdb/
[lnk-ref-arch]: http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf
[lnk-stream-analytics]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-dm-preview]: ../iot-hub/iot-hub-device-management-overview.md
[lnk-dynamic-telemetry]: iot-suite-dynamic-telemetry.md

[lnk-predictive-overview]: iot-suite-predictive-overview.md
[lnk-faq]: iot-suite-faq.md
[lnk-security-groundup]: securing-iot-ground-up.md
