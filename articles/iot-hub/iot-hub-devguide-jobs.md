<properties
 pageTitle="Arendaja juhend - töö | Microsoft Azure'i"
 description="Azure'i asjade jaoturi arendaja juhend - tööde käitamist mitmes seadmes planeerimisel ühendatud teie jaoturi"
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

# <a name="schedule-jobs-on-multiple-devices-preview"></a>Tööde mitmes seadmes (eelvaade)

## <a name="overview"></a>Ülevaade

Eelmise artikleid kirjeldatud Azure'i asjade jaoturi võimaldab arvu koosteüksuste ([seadme Double atribuudid ja siltide] [ lnk-twin-devguide] ja [pilveteenuse-seadme meetodite][lnk-dev-methods]).  Tavaliselt asjade tagasi end rakenduste lubada seadme administraatorid ja tehtemärgid, värskendada ja asjade seadmete hulgi ja ajastatud ajal suhelda.  Töö kapseldada täitmise seadme Double värskendusi ja C2D meetodite kogumi seadmete ajakava korraga.  Näiteks kasutaks tehtemärgi tagasi lõpuks rakendus, mis kontaktiga ja jälgida töö, mida soovite taaskäivitage seadmete 43 ja floor 3 koostamise ajal, mis ei ole segab hoone toimingute kogum.

### <a name="when-to-use"></a>Millal kasutada

Kaaluge abil töö: lahenduse tagasi lõpetamine vajadustele kavandada ja jälgimiseks, mis tahes seadme komplekti järgmist:

- Seadme Double soovitud atribuutide värskendamine
- Seadme Double siltide värskendamine
- Autonoomsest C2D meetodid

## <a name="job-lifecycle"></a>Töö elutsükkel

Töö on algataja lahenduse tagasi lõppu ja haldab asjade jaoturi.  Saate algatada töö kaudu teenuse suunatud URI (`{iot hub}/jobs/v2/{device id}/methods/<jobID>?api-version=2016-09-30-preview`) ja päring on täidesaatva töökohtade kaudu teenuse suunatud URI edenemist (`{iot hub}/jobs/v2/<jobId>?api-version=2016-09-30-preview`).  Töö on käivitatud, lubada päringute töökohta tagasi end rakenduse värskendada oleku tööde.

> [AZURE.NOTE] Kui annate tööd, atribuutide nimesid ja väärtuste võib sisaldada ainult US-ASCII prinditavad tähtedest ja numbritest koosnev, välja arvatud juhul, kui mõni järgmised määramine: ``{'$', '(', ')', '<', '>', '@', ',', ';', ':', '\', '"', '/', '[', ']', '?', '=', '{', '}', SP, HT}``.

## <a name="reference-topics"></a>Teemasid:

Järgmistes teemades viide teile töö kasutamise kohta leiate lisateavet.

## <a name="jobs-to-execute-direct-methods"></a>Töö, et käivitada otse meetodid

Järgmine on HTTP 1.1 taotluse üksikasjad on [otsene meetod] täitmine[ lnk-dev-methods] kogumi töö kasutavates seadmetes:

    ```
    PUT /jobs/v2/<jobId>?api-version=2016-09-30-preview
    
    Authorization: <config.sharedAccessSignature>
    Content-Type: application/json; charset=utf-8
    Request-Id: <guid>
    User-Agent: <sdk-name>/<sdk-version>

    {
        jobId: '<jobId>',
        type: 'scheduleDirectRequest', 
        cloudToDeviceMethod: {
            methodName: '<methodName>',
            payload: <payload>,                 
            timeoutInSeconds: methodTimeoutInSeconds 
        },
        queryCondition: '<queryOrDevices>', // if the queryOrDevices parameter is a string
        deviceIds: '<queryOrDevices>',      // if the queryOrDevices parameter is an array
        startTime: <jobStartTime>,          // as an ISO-8601 date string
        maxExecutionTimeInSeconds: <maxExecutionTimeInSeconds>        
    }
    ```
    
## <a name="jobs-to-update-device-twin-properties"></a>Töö, et seade Double atribuutide värskendamine

HTTP 1.1 taotluse üksikasjad töö abil seadme Double atribuutide värskendamine on järgmine:

    ```
    PUT /jobs/v2/<jobId>?api-version=2016-09-30-preview
    Authorization: <config.sharedAccessSignature>
    Content-Type: application/json; charset=utf-8
    Request-Id: <guid>
    User-Agent: <sdk-name>/<sdk-version>

    {
        jobId: '<jobId>',
        type: 'scheduleTwinUpdate', 
        updateTwin: <patch>                 // Valid JSON object
        queryCondition: '<queryOrDevices>', // if the queryOrDevices parameter is a string
        deviceIds: '<queryOrDevices>',      // if the queryOrDevices parameter is an array
        startTime: <jobStartTime>,          // as an ISO-8601 date string
        maxExecutionTimeInSeconds: <maxExecutionTimeInSeconds>        // format TBD
    }
    ```

## <a name="querying-for-progress-on-jobs"></a>Päringute edu tööde haldamine

Järgmine on HTTP 1.1 taotluse üksikasjad [päringute tööde][lnk-query]:

    ```
    GET /jobs/v2/query?api-version=2016-09-30-preview[&jobType=<jobType>][&jobStatus=<jobStatus>][&pageSize=<pageSize>][&continuationToken=<continuationToken>]
    
    Authorization: <config.sharedAccessSignature>
    Content-Type: application/json; charset=utf-8
    Request-Id: <guid>
    User-Agent: <sdk-name>/<sdk-version>
    ```
    
Funktsiooni continuationToken esitatakse vastus.  

## <a name="jobs-properties"></a>Töö atribuudid

Järgmises loendis atribuudid ja vastavad kirjeldused, mida saab kasutada kui päringu jaoks töö või töö tulemused.

| Atribuut | Kirjeldus |
| -------------- | -----------------|
| **jobId** | Rakenduse ID ette töö. |
| **startTime** | Kui töö algusaeg (ISO-8601). |
| **endTime** | Asjade jaoturi esitatud kuupäev (ISO-8601) kui tööks valmis. Lubatud alles pärast töö jõuab "lõpuleviidud". | 
| **tüüp** | Töö tüüpi: |
| | **scheduledUpdateTwin**: töö värskendada Double soovitud atribuudid või sildid. |
| | **scheduledDeviceMethod**: kasutada seadme meetodi kogumi Double tööd. |
| **olek** | Praeguse oleku töö. Olek võimalikud väärtused: |
| | **Ootel** : ajastatud ja töö teenus kätte ootamine. |
| | **ajastatud** : tulevikus ajale. |
| | **töötab** : aktiivne töö. |
| | **tühistatud** : töö on tühistatud. |
| | **nurjus** : töö nurjus. |
| | **lõpetatud** : töö on valmis. |
| **deviceJobStatistics** | Statistika projekti täitmise kohta. |

Eelvaate, deviceJobStatistics objekti on saadaval ainult siis, kui töö on lõpule jõudnud.

| Atribuut | Kirjeldus |
| -------------- | -----------------|
| **deviceJobStatistics.deviceCount** | Seadmete töö arv. |
| **deviceJobStatistics.failedCount** | Arv seadmetes, kus on töö nurjus. |
| **deviceJobStatistics.succeededCount** | Arv seadmetes, kus on töö on lõpule viidud. |
| **deviceJobStatistics.runningCount** | Praegu töötab töö seadmete arv. |
| **deviceJobStatistics.pendingCount** | Mitu seadet on ootel töö käivitamiseks. |


### <a name="additional-reference-material"></a>Täiendav viide materjali

Muud viited teemade arendaja juhend on järgmised.

- [Asjade jaoturi lõpp-punktid] [ lnk-endpoints] kirjeldatakse mitmesuguste lõpp-punktid, mis iga asjade jaoturi seab käitusaja ja haldus.
- [Pidurdamise ja kvootide] [ lnk-quotas] kirjeldatakse piirmäära, mis kehtivad teenuse asjade jaoturi ja ahendamise käitumine oodata, kui kasutate teenust.
- [Asjade jaoturi seade ja SDK-d] [ lnk-sdks] erinevate keele SDK-d on loetletud mõni kasutamine, kui teil tekib nii seade ja rakendused, mis nendega asjade jaoturi abil saate.
- [Päringu keel kaksikud, võimalused ja töö] [ lnk-query] kirjeldatakse päringukeele abil saate otsida teavet asjade keskuse kohta oma seadmes kaksikud, võimalused ja -tööde haldamine.
- [Asjade jaoturi MQTT tugi] [ lnk-devguide-mqtt] annab Lisateavet kohta asjade jaoturi tugi MQTT protokolli.

## <a name="next-steps"></a>Järgmised sammud

Kui soovite proovida mõnda selles artiklis kirjeldatud mõisted, võib olla huvi järgmised asjade jaoturi õpetus:

- [Ajakava ja leviedastus tööde haldamine][lnk-jobs-tutorial]

<!-- links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-jobs-tutorial]: iot-hub-schedule-jobs.md
[lnk-c2d-methods]: iot-hub-c2d-methods.md
[lnk-dev-methods]: iot-hub-devguide-direct-methods.md
[lnk-get-started-twin]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-devguide]: iot-hub-devguide-device-twins.md
