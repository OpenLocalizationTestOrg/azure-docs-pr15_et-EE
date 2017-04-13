<properties
 pageTitle="Asjade jaoturi tegevuse jälgimine"
 description="Azure'i asjade jaoturi toimingute jälgimine, mis võimaldab teil jälgida oma asjade jaoturi reaalajas toimingute ülevaade"
 services="iot-hub"
 documentationCenter=""
 authors="nberdy"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="08/11/2016"
 ms.author="nberdy"/>

# <a name="introduction-to-operations-monitoring"></a>Sissejuhatus toimingute jälgimine

Asjade jaoturi tegevuse jälgimine võimaldab teil jälgida oma asjade jaoturi reaalajas toiminguid. Asjade jaoturi jälgib sündmuste toimingute mitu kategooriate lõikes ja te saate valida ühe või mitme kategooria sündmuste saata oma asjade jaoturi töötlemiseks lõpp. Saate jälgida andmeid tõrgete või häälestamine keerukamaid töötlemine mustrite andmete põhjal.

Asjade jaoturi jälgib sündmuste viis kategooriate:

- Seadme identiteedi toimingud
- Seadme telemeetria
- Pilveteenuse-seadme käsud
- Ühendused
- Faili üleslaadimine

## <a name="how-to-enable-operations-monitoring"></a>Kuidas lubada toimingute jälgimine

1. Soovitud asjade keskuse loomine. Juhised leiate soovitud asjade jaoturi loomist saate [Alustada] [ lnk-get-started] juhend.

2. Avage oma asjade jaoturi tera. Seal, klõpsake nuppu **toimingud jälgimine**.

    ![][1]

3. Valige jälgimisega seotud kategooriad soovite saate jälgida, ja klõpsake siis nuppu **Salvesta**. Sündmused on saadaval lugemiseks **jälgimis sätted**loendis sündmuse jaoturi ühilduv lõpp-punkti. Asjade jaoturi lõpp-punkti nimetatakse `messages/operationsmonitoringevents`.

    ![][2]

## <a name="event-categories-and-how-to-use-them"></a>Sündmuse kategooriaid ja kuidas neid kasutada

Iga kontrolli kategooria lugusid toimingute mõnda muud tüüpi asjade jaoturi ja iga jälgimisega seotud kategooria on skeem, mis määratleb, kuidas sündmused kategooria on struktureeritud.

### <a name="device-identity-operations"></a>Seadme identiteedi toimingud

Seadme identiteedi toimingute kategooria jälitab tõrkeid ilmneda juhul, kui proovite luua, värskendada või asjade jaoturi identiteedi registris kirje kustutamine. Selle kategooria jälgimine on kasulik ettevalmistamise stsenaariumid.

    {
        "time": "UTC timestamp",
         "operationName": "create",
         "category": "DeviceIdentityOperations",
         "level": "Error",
         "statusCode": 4XX,
         "statusDescription": "MessageDescription",
         "deviceId": "device-ID",
         "durationMs": 1234,
         "userAgent": "userAgent",
         "sharedAccessPolicy": "accessPolicy"
    }

### <a name="device-telemetry"></a>Seadme telemeetria

Seadme telemeetria kategooria jälitab tõrkeid, mis esinevad asjade jaoturi ja on seotud telemeetria kohaletoimetamisel. Selle kategooria sisaldab tõrkeid ilmneda saatmine telemeetria sündmuste (nt pidurdamise) ja vastuvõtmine telemeetria sündmuste (nt volitamata lugemise). Pange tähele, et selle kategooria ei saa jaole juba koodi, mis töötab seadme ise põhjustatud vead.

    {
         "messageSizeInBytes": 1234,
         "batching": 0,
         "protocol": "Amqp",
         "authType": "{\"scope\":\"device\",\"type\":\"sas\",\"issuer\":\"iothub\"}",
         "time": "UTC timestamp",
         "operationName": "ingress",
         "category": "DeviceTelemetry",
         "level": "Error",
         "statusCode": 4XX,
         "statusType": 4XX001,
         "statusDescription": "MessageDescription",
         "deviceId": "device-ID",
         "EventProcessedUtcTime": "UTC timestamp",
         "PartitionId": 1,
         "EventEnqueuedUtcTime": "UTC timestamp"
    }

### <a name="cloud-to-device-commands"></a>Pilveteenuse-seadme käsud

Pilveteenuse-seadme käskude kategooria jälitab tõrkeid, mis esinevad asjade jaoturi ja on seotud seadme käsk kohaletoimetamisel. Selle kategooria sisaldab tõrkeid, mis ilmnevad saatmise käskude (nt volitamata saatja) vastuvõtmine käske (nt kohaletoimetamise arvu ületamisega) ja vastuvõtmine käsk tagasiside (nt tagasiside aegunud). Selle kategooria ei tuvasta seadmest, mis tegeleb valesti käsu, kui käsu toimetati edukalt.

    {
         "messageSizeInBytes": 1234,
         "authType": "{\"scope\":\"hub\",\"type\":\"sas\",\"issuer\":\"iothub\"}",
         "deliveryAcknowledgement": 0,
         "protocol": "Amqp",
         "time": " UTC timestamp",
         "operationName": "ingress",
         "category": "C2DCommands",
         "level": "Error",
         "statusCode": 4XX,
         "statusType": 4XX001,
         "statusDescription": "MessageDescription",
         "deviceId": "device-ID",
         "EventProcessedUtcTime": "UTC timestamp",
         "PartitionId": 1,
         "EventEnqueuedUtcTime": “UTC timestamp"
    }

### <a name="connections"></a>Ühendused

Ühenduste kategooria jälitab tõrkeid ilmneda seadmete ühendamiseks või ühenduse mõne asjade keskuse kaudu. Jälgimine selle kategooria on kasulik volitamata ühenduse katsete tuvastamiseks ja jälgimiseks, kui ühendus katkeb alade kehva ühenduvuse seadmete jaoks.

    {
         "durationMs": 1234,
         "authType": "{\"scope\":\"hub\",\"type\":\"sas\",\"issuer\":\"iothub\"}",
         "protocol": "Amqp",
         "time": " UTC timestamp",
         "operationName": "deviceConnect",
         "category": "Connections",
         "level": "Error",
         "statusCode": 4XX,
         "statusType": 4XX001,
         "statusDescription": "MessageDescription",
         "deviceId": "device-ID"
    }

### <a name="file-uploads"></a>Faili üleslaadimine

Faili üleslaadimine kategooria jälitab tõrkeid, mis esinevad asjade jaoturi ja on seotud faili üleslaadimine funktsioonid. Selle kategooria sisaldab tõrkeid ilmneda SAS URI (nt aegumisel enne seadme teavitab lõplikus üles jaoturi), nurjunud lisatud teatatud seade ja kui faili ei leitud salvestusruumi asjade jaoturi teatise sõnumi loomisel. Pange tähele, et selle kategooria ei saa jaole juba tõrkeid ilmneda otse seade on faili üleslaadimine salvestusruumi.

    {
         "authType": "{\"scope\":\"hub\",\"type\":\"sas\",\"issuer\":\"iothub\"}",
         "protocol": "HTTP",
         "time": " UTC timestamp",
         "operationName": "ingress",
         "category": "fileUpload",
         "level": "Error",
         "statusCode": 4XX,
         "statusType": 4XX001,
         "statusDescription": "MessageDescription",
         "deviceId": "device-ID",
         "blobUri": "http//bloburi.com",
         "durationMs": 1234
    }

## <a name="next-steps"></a>Järgmised sammud

Asjade jaoturi võimaluste täpsemaks analüüsimiseks järgmistest teemadest.

- [Arendaja juhend][lnk-devguide]
- [Simuleerida seadme asjade lüüsi SDK][lnk-gateway]

<!-- Links and images -->
[1]: media/iot-hub-operations-monitoring/enable-OM-1.png
[2]: media/iot-hub-operations-monitoring/enable-OM-2.png

[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[lnk-diagnostic-metrics]: iot-hub-metrics.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-dr]: iot-hub-ha-dr.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md