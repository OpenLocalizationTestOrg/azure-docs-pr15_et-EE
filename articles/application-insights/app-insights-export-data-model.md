<properties 
    pageTitle="Rakenduse ülevaateid andmemudel" 
    description="Kirjeldatakse atribuudid pidev ekspordi JSON eksporditud ja kasutada filtreid." 
    services="application-insights" 
    documentationCenter=""
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="03/21/2016" 
    ms.author="awills"/>

# <a name="application-insights-export-data-model"></a>Rakenduse ülevaateid ekspordi andmemudel

Selles tabelis on loetletud atribuutide telemeetria saadetud portaali [Rakenduse ülevaated](app-insights-overview.md) SDK-d. Näete atribuutidest andmete väljund [Pidev eksportida](app-insights-export-telemetry.md).
Lisaks kuvatakse need filtrid atribuudi [Väärtuseks meetermõõdustik Explorer](app-insights-metrics-explorer.md) ja [Diagnostika otsing](app-insights-diagnostic-search.md).

Pange tähele, et punkte.

* `[0]`nendes tabelites tähistab punkti tee, kus teil on lisada registri; kuid see pole alati 0.
* Aja kestused on kümnendikku microsecond, nii et 10000000 == 1 teise.
* Kuupäevad ja kellaajad on UTC ja on ära toodud ISO-vormingus`yyyy-MM-DDThh:mm:ss.sssZ`

On mitu [näidised](app-insights-export-telemetry.md#code-samples) , mis illustreerivad, kuidas neid kasutada.



## <a name="example"></a>Näide

    // A server report about an HTTP request
    {
    "request": [ 
      {
        "urlData": { // derived from 'url'
          "host": "contoso.org",
          "base": "/",
          "hashTag": "" 
        },
        "responseCode": 200, // Sent to client
        "success": true, // Default == responseCode<400
        // Request id becomes the operation id of child events 
        "id": "fCOhCdCnZ9I=",  
        "name": "GET Home/Index",
        "count": 1, // 100% / sampling rate
        "durationMetric": {
          "value": 1046804.0, // 10000000 == 1 second
          // Currently the following fields are redundant:
          "count": 1.0,
          "min": 1046804.0,
          "max": 1046804.0,
          "stdDev": 0.0,
          "sampledValue": 1046804.0
        },
        "url": "/"
      }
    ],
    "internal": {
      "data": {
        "id": "7f156650-ef4c-11e5-8453-3f984b167d05",
        "documentVersion": "1.61"
      }
    },
    "context": {
      "device": { // client browser
        "type": "PC",
        "screenResolution": { },
        "roleInstance": "WFWEB14B.fabrikam.net"
      },
      "application": { },
      "location": { // derived from client ip
        "continent": "North America",
        "country": "United States",
        // last octagon is anonymized to 0 at portal:
        "clientip": "168.62.177.0", 
        "province": "",
        "city": ""
      },
      "data": {
        "isSynthetic": true, // we identified source as a bot
        // percentage of generated data sent to portal:
        "samplingRate": 100.0, 
        "eventTime": "2016-03-21T10:05:45.7334717Z" // UTC
      },
      "user": {
        "isAuthenticated": false,
        "anonId": "us-tx-sn1-azr", // bot agent id
        "anonAcquisitionDate": "0001-01-01T00:00:00Z",
        "authAcquisitionDate": "0001-01-01T00:00:00Z",
        "accountAcquisitionDate": "0001-01-01T00:00:00Z"
      },
      "operation": {
        "id": "fCOhCdCnZ9I=",
        "parentId": "fCOhCdCnZ9I=",
        "name": "GET Home/Index"
      },
      "cloud": { },
      "serverDevice": { },
      "custom": { // set by custom fields of track calls
        "dimensions": [ ],
        "metrics": [ ]
      },
      "session": {
        "id": "65504c10-44a6-489e-b9dc-94184eb00d86",
        "isFirst": true
      }
    }
  }




## <a name="context"></a>Kontekst

Igat tüüpi telemeetria on kaasas jaotises konteksti. Kõik need väljad edastatakse iga andmepunkti.



|Tee|Tüüp|Märkmete|
|---|---|---|
| Context.custom.Dimensions [0]  | objekti]  | Võti-väärtus stringi paari määratud parameetri kohandatud atribuudid. Võtme max pikkus 100 väärtused max pikkus 1024. Rohkem kui 100 kordumatuid väärtusi atribuudi saab otsida, kuid ei saa kasutada osadeks. Max 200 klahvide ikey kohta.  |
| Context.custom.Metrics [0]  | objekti]  | Võti ja väärtuse paarideks määrata kohandatud mõõtmed parameetrite ja TrackMetrics. Võtme max pikkus 100 väärtused võib arvväärtusega. |
| context.data.eventTime | string | UTC |
| context.data.isSynthetic | kahendmuutuja | Kutse kuvatakse pärit robot või web test. |
| context.data.samplingRate | arv | Telemeetria Tarkvaraarenduskomplektist, mis saadetakse portaali loodud protsent. Vahemik 0,0-100,0.|
| Context.Device | objekti | Kliendi seade |
| Context.Device.Browser | string | St, Chrome... |
| context.device.browserVersion | string | Chrome'i 48,0... |
| context.device.deviceModel | string | |
| context.device.deviceName | string | |
| Context.Device.ID | string | |
| Context.Device.locale | string | EN-GB, de-DE... |
| Context.Device.Network | string | |
| context.device.oemName | string | |
| context.device.osVersion | string | Host OS |
| context.device.roleInstance | string | Serveri hosti ID |
| context.device.roleName | string | |
| Context.Device.Type | string | Arvutis brauseri... |
| Context.location | objekti | Saadud clientip. |
| Context.location.City | string | Tuletatud clientip, kui see on teada  |
| Context.location.clientip | string | Viimase octagon on anonüümseks 0. |
| Context.location.Continent | string | |
| Context.location.Country | string | |
| Context.location.Province | string | Maakond või vald |
| Context.Operation.ID | string | Üksused, millel on sama toimingu ID-d on näidatud seostuvad üksused portaalis. Tavaliselt taotluse id. |
| Context.Operation.name | string | URL-i või päringu nimi |
| context.operation.parentId | string | Võimaldab pesastatud seostuvad üksused. |
| Context.Session.ID | string | Toimingute samast allikast rühma ID-d. Toimingu ilma 30 minuti jooksul signaale seansi lõppu. |
| context.session.isFirst | kahendmuutuja | |
| context.user.accountAcquisitionDate | string | |
| context.user.anonAcquisitionDate | string | |
| context.user.anonId | string | |
| context.user.authAcquisitionDate | string | [Autenditud kasutaja](app-insights-api-custom-events-metrics.md#authenticated-users) |
| context.user.isAuthenticated | kahendmuutuja | |
| internal.data.documentVersion | string | |
| Internal.Data.ID | string | |



## <a name="events"></a>Sündmused

Kohandatud sündmused, mis on loodud [TrackEvent()](app-insights-api-custom-events-metrics.md#track-event). 


|Tee|Tüüp|Märkmete|
|---|---|---|
| sündmuse [0] loendus | täisarv | 100 / ([valimite](app-insights-sampling.md) määr). Näide 4 =&gt; 25%. |
| sündmuse [0] nimi | string | Sündmuse nimi.  Max pikkus 250. |
| sündmuse [0] URL-i | string | |
| sündmuse [0] urlData.base | string | |
| sündmuse [0] urlData.host | string | |

## <a name="exceptions"></a>Erandid

Aruannete [erandid](app-insights-asp-net-exceptions.md) server ja brauseris. 


|Tee|Tüüp|Märkmete|
|---|---|---|
| basicException [0] komplekteerimine | string | |
| basicException [0] loendus | täisarv | 100 / ([valimite](app-insights-sampling.md) määr). Näide 4 =&gt; 25%. |
| exceptionGroup basicException [0] | string | |
| exceptionType basicException [0] | string | |string | |
| failedUserCodeMethod basicException [0] | string | |
| failedUserCodeAssembly basicException [0] | string | |
| handledAt basicException [0] | string | |
| hasFullStack basicException [0] | kahendmuutuja | |
| basicException [0] id | string | |
| basicException [0] meetod | string | |
| sõnumi basicException [0] | string | Erandi teade. Max pikkus 10k.|
| outerExceptionMessage basicException [0] | string | |
| outerExceptionThrownAtAssembly basicException [0] | string | |
| outerExceptionThrownAtMethod basicException [0] | string | |
| outerExceptionType basicException [0] | string | |
| outerId basicException [0] | string | |
| basicException [0] [0] parsedStack komplekteerimine | string | |
| basicException [0] [0] parsedStack faili nimi | string | |
| basicException [0] [0] parsedStack tase | täisarv | |
| basicException [0] [0] parsedStack joon | täisarv | |
| basicException [0] [0] parsedStack meetod | string | |
| basicException [0] virnas | string | Max pikkus 10k|
| basicException [0] typeName | string | |



## <a name="trace-messages"></a>Sõnumite jälgimine

Saadetud [TrackTrace](app-insights-api-custom-events-metrics.md#track-trace)- ja [logimine adapterit](app-insights-asp-net-trace-logs.md).


|Tee|Tüüp|Märkmete|
|---|---|---|
| sõnumi [0] loggerName | string ||
| sõnumi [0] parameetrid | string ||
| sõnumi [0] töötlemata | string | Log sõnumi max pikkus 10k. |
| sõnumi [0] severityLevel | string | |



## <a name="remote-dependency"></a>Remote sõltuvus

Saadetud TrackDependency. Kasutatakse aruande jõudlus ja server [nõuab sõltuvused](app-insights-asp-net-dependencies.md) kasutamist ja AJAXI helistab brauseris.

|Tee|Tüüp|Märkmete|
|---|---|---|
| asünkroonse remoteDependency [0] | kahendmuutuja | |
| remoteDependency [0] baseName | string |  |
| commandName remoteDependency [0] | string | Näiteks "Avaleht/index" |
| remoteDependency [0] loendus | täisarv | 100 / ([valimite](app-insights-sampling.md) määr). Näide 4 =&gt; 25%. |
| dependencyTypeName remoteDependency [0] | string | HTTP, SQL-I... |
| durationMetric.value remoteDependency [0] | arv | Kõne lõpetamist sõltuvus vastuse aeg. |
| remoteDependency [0] id | string | |
| remoteDependency [0] nimi | string | URL-i. Max pikkus 250.|
| resultCode remoteDependency [0] | string | sõltuvus HTTP kaudu |
| edu remoteDependency [0] | kahendmuutuja | |
| remoteDependency [0] tüüp | string | Http, SQL-i... |
| remoteDependency [0] URL-i | string |  Max pikkus 2000 |
| urlData.base remoteDependency [0] | string | Max pikkus 2000 |
| urlData.hashTag remoteDependency [0] | string | |
| urlData.host remoteDependency [0] | string | Max pikkus 200 |


## <a name="requests"></a>Taotlused

Saadetud [TrackRequest](app-insights-api-custom-events-metrics.md#track-request). Standard moodulid selle abil aruandeid serveri aega, mõõdetuna server. 


|Tee|Tüüp|Märkmete|
|---|---|---|
| taotluse [0] loendus | täisarv | 100 / ([valimite](app-insights-sampling.md) määr). Näide: 4 =&gt; 25%. |
| taotluse [0] durationMetric.value | arv | Taotluse saabuvad vastuse aeg. 1e7 == 1s |
| päringu [0]-id | string | Toimingu ID-d |
| taotluse [0] nimi | string | GET-/ POST + url base.  Max pikkus 250 |
| taotluse [0] responseCode | täisarv | HTTP vastuse saata kliendi |
| taotluse [0] edu | kahendmuutuja | Vaikimisi == (responseCode &lt; 400) |
| taotluse [0] URL-i | string | Välja arvatud host |
| taotluse [0] urlData.base | string | |
| taotluse [0] urlData.hashTag | string |  |
| taotluse [0] urlData.host | string | |


## <a name="page-view-performance"></a>Lehe vaate jõudlus

Brauseri edastatud. Meetmed aega, et töödelda lehel kasutaja alustamist taotluse täieliku (v.a asünkroonse AJAXI kõned) kuvamiseks.

Konteksti väärtused kuvada kliendi OS ja brauseri versiooni. 


|Tee|Tüüp|Märkmete|
|---|---|---|
| clientProcess.value clientPerformance [0] | täisarv | Lõppu vastuvõtmine HTML-i abil lehe kuvamise aeg. |
| clientPerformance [0] nimi | string | |
| networkConnection.value clientPerformance [0] | täisarv | Võrguga ühenduse loomine kuluv aeg. |
| receiveRequest.value clientPerformance [0] | täisarv | Kellaaeg saates taotluse vastu HTML-i vastamisel lõpust. |
| sendRequest.value clientPerformance [0] | täisarv | Aega võtta HTTP saatmistaotlus. |
| total.value clientPerformance [0] | täisarv | Käivitamise kutse saatmiseks lehe kuvamise aeg. |
| clientPerformance [0] URL-i | string | URL-i taotluse |
| urlData.base clientPerformance [0] | string | |
| urlData.hashTag clientPerformance [0] | string | |
| urlData.host clientPerformance [0] | string | |
| urlData.protocol clientPerformance [0] | string | |

## <a name="page-views"></a>Lehe vaated

Saadetud trackPageView() või [stopTrackPage](app-insights-api-custom-events-metrics.md#page-view)

|Tee|Tüüp|Märkmete|
|---|---|---|
| vaate [0] loendus | täisarv | 100 / ([valimite](app-insights-sampling.md) määr). Näide 4 =&gt; 25%. |
| vaate [0] durationMetric.value | täisarv | Soovi korral määrake trackPageView() või startTrackPage() - väärtuse stopTrackPage(). Pole sama, mis clientPerformance väärtused. |
| [0] vaate nimi | string | Lehe tiitel.  Max pikkus 250 |
| vaate [0] url | string | |
| vaate [0] urlData.base | string | |
| vaate [0] urlData.hashTag | string | |
| vaate [0] urlData.host | string | |



## <a name="availability"></a>Kättesaadavus

Aruannete [kättesaadavus web kontrollib](app-insights-monitor-web-app-availability.md).

|Tee|Tüüp|Märkmete|
|---|---|---|
| [0]-Saadavus availabilityMetric.name | string | kättesaadavus |
| [0]-Saadavus availabilityMetric.value | arv |1.0 või 0,0 |
| kättesaadavus [0] loendus | täisarv | 100 / ([valimite](app-insights-sampling.md) määr). Näide 4 =&gt; 25%. |
| [0]-Saadavus dataSizeMetric.name | string | |
| [0]-Saadavus dataSizeMetric.value | täisarv | |
| [0]-Saadavus durationMetric.name | string | |
| [0]-Saadavus durationMetric.value | arv | Testi kestus. 1e7 == 1s |
| [0]-Saadavus sõnum | string | Diagnostika tõrge |
| [0]-Saadavus tulem | string | Pass/Fail |
| [0]-Saadavus runLocation | string | Http req Geo allikas |
| [0]-Saadavus testName | string | |
| [0]-Saadavus testRunId | string | |
| [0]-Saadavus testTimestamp | string | |




## <a name="metrics"></a>Mõõdikud

Loodud TrackMetric().

Argumendil väärtus on leitud context.custom.metrics[0]

Näiteks:

    {
     "metric": [ ],
     "context": {
     ...
     "custom": {
        "dimensions": [
          { "ProcessId": "4068" }
        ],
        "metrics": [
          {
            "dispatchRate": {
              "value": 0.001295,
              "count": 1.0,
              "min": 0.001295,
              "max": 0.001295,
              "stdDev": 0.0,
              "sampledValue": 0.001295,
              "sum": 0.001295
            }
          }
         } ] }
    }

## <a name="about-metric-values"></a>Argumendil väärtuste kohta

Argumendil väärtused, nii argumendil aruandeid ja mujal, on kirjeldatud standardse objektistruktuuri. Näiteks:

      "durationMetric": {
        "name": "contoso.org",
        "type": "Aggregation",
        "value": 468.71603053650279,
        "count": 1.0,
        "min": 468.71603053650279,
        "max": 468.71603053650279,
        "stdDev": 0.0,
        "sampledValue": 468.71603053650279
      }

Praegu - kuigi see võib muutuda, tulevikus – kõik väärtused teatatud standard SDK moodulid, `count==1` ja ainult selle `name` ja `value` väljad on kasulik. Ainult juhul, kui nad oleks erinevate oleks, kui kirjutate TrackMetric kõnede rakenduses, kus saate määrata muid parameetrid. 

Muude väljade eesmärk luba mõõdikute koondatakse SDK, portaali liikluse vähendamiseks. Näiteks võib enne saatmist iga argumendil aruande Keskmine mitme järjestikuse lugemise käigus. Seejärel oleks min, max, standardhälve ja kogumaksumus (sum ja average) arvutamiseks ja lugemist tähistab aruande arvu loendamine määramine. 

Ülaltoodud tabelis on meil puudub harva kasutatavad väljad count, min, max, stdDev ja sampledValue.

Eelnevalt koondamisega mõõdikute, asemel saate [valimite](app-insights-sampling.md) , kui teil on vaja telemeetria mahu vähendamiseks.


### <a name="durations"></a>Kestus

Välja arvatud juhul, kui märgitud teisiti, kestused on esindatud kümnendikku on microsecond nii, et 10000000.0 tähendab 1 teise.



## <a name="see-also"></a>Vt ka

* [Rakenduse ülevaated](app-insights-overview.md) 
* [Pidev eksport](app-insights-export-telemetry.md)
* [Koodinäiteid](app-insights-export-telemetry.md#code-samples)


