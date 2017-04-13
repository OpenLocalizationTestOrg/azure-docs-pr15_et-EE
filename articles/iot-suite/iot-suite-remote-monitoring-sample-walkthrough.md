<properties
 pageTitle="Järelevalve Remote'i eelkonfigureeritud lahenduse kiirtutvustus | Microsoft Azure'i"
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
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="08/17/2016"
 ms.author="dobett"/>

# <a name="remote-monitoring-preconfigured-solution-walkthrough"></a>Jälgida eelkonfigureeritud lahenduse kiirtutvustus

## <a name="introduction"></a>Sissejuhatus

Asjade komplekti serveri jälgimine [eelkonfigureeritud lahenduse] [ lnk-preconfigured-solutions] jälgib end-to-end rakendamise lahendus mitme masinad töötavad remote asukohad. Lahendus ühendab võtme Azure teenuseid teile üldine rakendamine äri stsenaariumi ja saate kasutada seda lähtepunktina oma rakendamiseks. Saate [kohandada] [ lnk-customize] lahendus oma ettevõtte konkreetseid nõuetele.

Selles artiklis tutvustatakse mõned peamised elemendid remote jälgimise lahendus, mis võimaldavad teil mõista, kuidas see toimib. Teadmiste abil saate:

- Lahendus probleemide tõrkeotsing.
- Kuidas kohandada oma konkreetsete vajaduste rahuldamiseks lahenduse plaanimine. 
- Kujundage oma asjade lahenduse, mis kasutab Azure teenused.

## <a name="logical-architecture"></a>Loogiline arhitektuur

Järgmine diagramm kirjeldab loogilise komponentide eelkonfigureeritud lahendus:

![Loogiline arhitektuur](media/iot-suite-remote-monitoring-sample-walkthrough/remote-monitoring-architecture.png)


## <a name="simulated-devices"></a>Jäljendatud seadmed

Eelkonfigureeritud lahenduse jäljendatud seadme tähistab jahutusseadise, (nt building Õhupuhastajad või poole käsitsemise üksuse). Eelkonfigureeritud lahenduse juurutamisel olete automaatselt ka ette neli jäljendatud seadmetes, mis on [Azure WebJob]käivitamiseks[lnk-webjobs]. Jäljendatud seadmete hõlbustavad uurimine lahenduse, mis tahes füüsilise seadmed juurutamiseks vajamata toimimist. Juurutada päris füüsilise seadmes, vt [ühenduse loomine Remote seadme jälgimise eelkonfigureeritud lahendus] [ lnk-connect-rm] õpetuse.

Iga jäljendatud seadme saate saata sõnum järgmist tüüpi asjade jaoturi:

| Sõnumi  | Kirjeldus |
|----------|-------------|
| Käivitus  | Kui seade, see saadab **seadme teavet** sõnumi tagasi lõpuks enda kohta teavet sisaldavate. Need andmed sisaldavad seadme ID-d, seadme metaandmeid, käskude loendi seadme toetab ja seadme konfiguratsioonile. |
| Kohaloleku | Seadme saadab perioodiliselt **olekuteade teatada, kas seade ei tunne sensori olemasolu** . |
| Telemeetria | Seadme saadab perioodiliselt **telemeetria** teade, et aruannete temperatuur ja niiskus kogutud seadme jäljendatud andurid jäljendatud väärtused. |


Jäljendatud seadmete saata meilisõnumi **seadme teavet** seadme järgmised atribuudid:

| Atribuut               |  Otstarve |
|------------------------|--------- |
| Seadme ID              | ID, mis on esitatud või määratud seadme loomisel lahendus. |
| Tootja           | Seadme tootjalt |
| Mudeli Number           | Seadme mudeli number |
| Järjenumbri          | Järjenumbri seade. |
| Püsivara               | Praeguse versiooni püsivara seadmes |
| Platvorm               | Seadme platvormi arhitektuur |
| Protsessor              | Protsessor töötab seade |
| Installitud RAM          | Seadmesse installitud muutmälu |
| Jaoturi lubatud olek      | Asjade jaoturi olekus atribuudi seade |
| Loodud aeg           | Lahendus on loodud seadme kellaaeg |
| Värskendamise kellaajaga           | Viimati värskendati atribuudid seade |
| Laiuskraad               | Seadme laiuskraad asukohta |
| Laiuskraadide              | Seadme laiuskraadide asukohta |

Funktsiooni simulaatorit seemned atribuutidest jäljendatud seadmete valimi väärtustega.  Iga kord, kui selle simulaatorit lähtestab jäljendatud seadme, seadme postituste eelmääratletud metaandmete asjade jaoturi. Pange tähele, kuidas kirjutatakse üle tehtud seadme portaalis metaandmete värskendusi.


Jäljendatud seadmed saavad hakkama armatuurlaualt lahenduse asjade keskuse kaudu saadetud järgmised käsud:

| Käsk                | Kirjeldus                                         |
|------------------------|-----------------------------------------------------|
| PingDevice             | Saadab _ping_ kontrollida see toimib seade   |
| StartTelemetry         | Käivitab saatmine telemeetria seade                 |
| StopTelemetry          | Lõpetab seade telemeetria saatmine             |
| ChangeSetPointTemp     | Muudab punkt väärtust, mille ümber juhusliku andmed luuakse |
| DiagnosticTelemetry    | Käivitab seadme simulaatorit saata täiendavad telemeetria väärtuse (externalTemp) |
| ChangeDeviceState      | Atribuudi väärtuseks seadme laiendatud olek muutub ja seadme teave sõnum saadetakse seadmest |

Seadme käsk kinnitus lahenduse tagasi lõppkuupäev on esitatud asjade keskuse kaudu.

## <a name="iot-hub"></a>Asjade jaoturi

[Asjade jaoturi] [ lnk-iothub] saaks saadetud seadmete pilve andmed ja teeb selle kättesaadavaks töö Azure'i voo Analytics (Ash). Asjade jaoturi saadab käsud seadmetes nimel seadme portaali. Iga voo ASA töö kasutab eraldi asjade jaoturi tarbija rühmale sõnumite vool lugeda teie seadmest.

## <a name="azure-stream-analytics"></a>Azure'i voo Analytics

Klõpsake remote jälgimise lahendus [Azure'i voo Analytics] [ lnk-asa] (Ash) saadab seadme sõnumeid asjade jaoturi lisakomponentide tagaandmebaas töötlemiseks või salvestusruumi. Erinevate ASA tööde teatud ülesandeid, sõnumite sisu põhjal.

**Töö 1: seadme teave** filtreerib seadme teavet sõnumite sissetuleva sõnumi voo ja saadab sündmuse jaoturi lõpp. Seadme saadab seadme teavet sõnumite käivitamisel ja **SendDeviceInfo** käsu. Selle töö kasutab järgmist päringu definitsiooni **seadme teavet** sõnumite tuvastamiseks:

```
SELECT * FROM DeviceDataStream Partition By PartitionId WHERE  ObjectType = 'DeviceInfo'
```

Selle töö saadab selle väljundi sündmuse jaoturiga edasiseks töötlemiseks.

**Töö 2: reeglite** hindab sissetulevaid temperatuuri- ja telemeetria väärtuste lävede seadme kohta. Reeglite redaktori saadaval lahenduse armatuurlauale seatakse läviväärtuse liugureid. Iga seadme/väärtuse paar on talletatud ajatempliga bloobimälu, mis voo Analytics loeb **Viide**andmetena. Töö võrdleb suvaline tühi väärtus piirmäärast seadme suhtes. Kui see ületab on ">" tingimusele, töö väljundid **alarm** sündmus, mis näitab, et piirmäär on ületatud ja pakub seade, väärtus ja ajatempli väärtused. Selle töö kasutab järgmist päringu definitsiooni telemeetria sõnumid, mis käivitab signaal tuvastamiseks:

```
WITH AlarmsData AS 
(
SELECT
     Stream.IoTHub.ConnectionDeviceId AS DeviceId,
     'Temperature' as ReadingType,
     Stream.Temperature as Reading,
     Ref.Temperature as Threshold,
     Ref.TemperatureRuleOutput as RuleOutput,
     Stream.EventEnqueuedUtcTime AS [Time]
FROM IoTTelemetryStream Stream
JOIN DeviceRulesBlob Ref ON Stream.IoTHub.ConnectionDeviceId = Ref.DeviceID
WHERE
     Ref.Temperature IS NOT null AND Stream.Temperature > Ref.Temperature

UNION ALL

SELECT
     Stream.IoTHub.ConnectionDeviceId AS DeviceId,
     'Humidity' as ReadingType,
     Stream.Humidity as Reading,
     Ref.Humidity as Threshold,
     Ref.HumidityRuleOutput as RuleOutput,
     Stream.EventEnqueuedUtcTime AS [Time]
FROM IoTTelemetryStream Stream
JOIN DeviceRulesBlob Ref ON Stream.IoTHub.ConnectionDeviceId = Ref.DeviceID
WHERE
     Ref.Humidity IS NOT null AND Stream.Humidity > Ref.Humidity
)

SELECT *
INTO DeviceRulesMonitoring
FROM AlarmsData

SELECT *
INTO DeviceRulesHub
FROM AlarmsData
```

Töö selle väljundi sündmuse jaoturiga edasiseks töötlemiseks ja salvestab iga teatise üksikasjad bloobimälu kui lahenduse armatuurlaual saate lugeda teatise teabele.

**Töö 3: telemeetria** töötab seadme telemeetria sissetuleva voo kahel viisil. Esimene saadab telemeetria kõigi sõnumite seadmete jaoks pikemaks püsivate bloobimälu. Teine avaldis arvutab väärtuste keskmine, miinimum- ja maksimumväärtused niiskus üle viie minuti libistades akna ja saadab andmed bloobimälu. Lahendus armatuurlaua loeb telemeetria andmed bloobimälu asustamiseks diagrammid. Selle töö kasutab järgmist päringu definitsiooni:

```
WITH 
    [StreamData]
AS (
    SELECT
        *
    FROM [IoTHubStream]
    WHERE
        [ObjectType] IS NULL -- Filter out device info and command responses
) 

SELECT
    IoTHub.ConnectionDeviceId AS DeviceId,
    Temperature,
    Humidity,
    ExternalTemperature,
    EventProcessedUtcTime,
    PartitionId,
    EventEnqueuedUtcTime,
    * 
INTO
    [Telemetry]
FROM
    [StreamData]

SELECT
    IoTHub.ConnectionDeviceId AS DeviceId,
    AVG (Humidity) AS [AverageHumidity],
    MIN(Humidity) AS [MinimumHumidity],
    MAX(Humidity) AS [MaxHumidity],
    5.0 AS TimeframeMinutes 
INTO
    [TelemetrySummary]
FROM [StreamData]
WHERE
    [Humidity] IS NOT NULL
GROUP BY
    IoTHub.ConnectionDeviceId,
    SlidingWindow (mi, 5)
```

## <a name="event-hubs"></a>Sündmuse jaoturi

**Seadme teave** ja **reeglid** ASA töökohtade väljund oma andmeid sündmuse jaoturi suunata usaldusväärselt **Sündmuse protsessor** on WebJob töötavad edasi.

## <a name="azure-storage"></a>Azure'i salvestusruum

Lahendus kasutab Azure'i bloobimälu püsima töötlemata ja summeeritud telemeetria andmete seadmetest lahendus. Armatuurlaua loeb telemeetria andmed bloobimälu asustamiseks diagrammid. Teatiste kuvamiseks armatuurlaua loeb andmed bloobimälu mis kirjeid kui telemeetria väärtused ületanud konfigureeritud läviväärtuse liugureid. Lahendus kasutab ka bloobimälu läviväärtuse liugureid, saate määrata armatuurlaua salvestada.

## <a name="webjobs"></a>WebJobs

Lisaks seadme simulaatorit, WebJobs lahendus majutada **Sündmuse protsessor** töötab Azure'i WebJob, mis tegeleb seadme teavet sõnumeid ja käsk vastuseid. See kasutab:

- Seadme teabe sõnumid uuendada seadme registri (talletatakse DocumentDB andmebaasis) praeguse seadme teabega.
- Käsk vastuse sõnumid uuendada seadme käsk ajalugu (salvestatud DocumentDB andmebaasi).

## <a name="documentdb"></a>DocumentDB

Lahendus kasutab DocumentDB andmebaasi lahendus ühendatud seadmete teabe talletamiseks. See teave sisaldab seadme metaandmete ja käsud, mis on saadetud seadmed armatuurlaualt ajalugu.

## <a name="web-apps"></a>Veebirakenduste

### <a name="remote-monitoring-dashboard"></a>Järelevalve Remote'i armatuurlaua
Selle lehe veebirakenduse kasutatakse PowerBI JavaScripti juhtelemente (vt [PowerBI-visuaale repo](https://www.github.com/Microsoft/PowerBI-visuals)) seadmete telemeetria andmete visualiseerimiseks. Lahendus kasutab ASA telemeetria töö Bloobivahemälu salvestusruumi telemeetria andmete kirjutamiseks.


### <a name="device-administration-portal"></a>Seadme haldamine portaalis

See web app võimaldab teil.

- Sätte uuele seadmele. See toiming määrab kordumatu seadme ID-d ja loob autentimise võti. See kirjutab teavet seadme asjade jaoturi identiteedi registri nii lahenduse kohased DocumentDB andmebaasi.
- Seadme atribuutide haldamine. See toiming sisaldab olemasolevate atribuutide kuvamine ja värskendamine uute atribuutidega.
- Käskude saada seadme.
- Saate seadme käsk ajalugu.
- Lubamine ja keelamine seadmed.

## <a name="next-steps"></a>Järgmised sammud

Järgmist TechNeti postitused pakuvad lisateavet järelevalve Remote'i eelkonfigureeritud lahendus:

- [Asjade komplekti - kaitstud - Remote jälgimine](http://social.technet.microsoft.com/wiki/contents/articles/32941.iot-suite-under-the-hood-remote-monitoring.aspx)
- [Asjade komplekti - jälgida - reaalajas ja jäljendatud seadmete lisamine](http://social.technet.microsoft.com/wiki/contents/articles/32975.iot-suite-remote-monitoring-adding-live-and-simulated-devices.aspx)

Saate jätkata alustamine asjade komplekti lugedes järgmistest artiklitest:

- [Ühendage seade järelevalve Remote'i eelkonfigureeritud lahendus][lnk-connect-rm]
- [Azureiotsuite.com saidiõigused][lnk-permissions]

[lnk-preconfigured-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-iothub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-asa]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-webjobs]: https://azure.microsoft.com/documentation/articles/websites-webjobs-resources/
[lnk-connect-rm]: iot-suite-connecting-devices.md
[lnk-permissions]: iot-suite-permissions.md