<properties
 pageTitle="Arendaja juhend - päringukeele | Microsoft Azure'i"
 description="Azure'i asjade jaoturi arendaja juhend - päringu keelt, mida kasutatakse teie asjade jaoturi kaksikud, meetodite ja töö kohta teabe toomiseks kirjeldus"
 services="iot-hub"
 documentationCenter=".net"
 authors="fsautomata"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="elioda"/>

# <a name="reference---query-language-for-twins-and-jobs"></a>Viide - päringukeele kaksikud ja-tööde haldamine

## <a name="overview"></a>Ülevaade

Asjade jaoturi pakub võimsaid SQL-nagu keele tuua [seadme kaksikud] puudutav teave[ lnk-twins] ja - [tööde haldamine][lnk-jobs]. Selles artiklis esitatakse:

* Sissejuhatus asjade jaoturi päringukeel, olulisemad funktsioonid ja
* Üksikasjalik kirjeldus keel.

## <a name="getting-started-with-twin-queries"></a>Alustamine: Double päringud

[Seadme kaksikud] [ lnk-twins] võib sisaldada suvalise JSON objektide nii sildid ja atribuudid. Asjade jaoturi abil saavad päringu seadme kaksikud kõiki Double andmeid sisaldav ühe JSON dokumendina.
Oletame näiteks, et teie asjade jaoturi Kaksikud on järgmine struktuur:

        {                                                                      
            "deviceId": "myDeviceId",                                            
            "etag": "AAAAAAAAAAc=",                                              
            "tags": {                                                            
                "location": {                                                      
                    "region": "US",                                                  
                    "plant": "Redmond43"                                             
                }                                                                  
            },                                                                   
            "properties": {                                                      
                "desired": {                                                       
                    "telemetryConfig": {                                             
                        "configId": "db00ebf5-eeeb-42be-86a1-458cccb69e57",            
                        "sendFrequencyInSecs": 300                                          
                    },                                                               
                    "$metadata": {                                                   
                    ...                                                     
                    },                                                               
                    "$version": 4                                                    
                },                                                                 
                "reported": {                                                      
                    "connectivity": {                                                
                        "type": "cellular"                            
                    },                                                               
                    "telemetryConfig": {                                             
                        "configId": "db00ebf5-eeeb-42be-86a1-458cccb69e57",            
                        "sendFrequencyInSecs": 300,                                         
                        "status": "Success"                                            
                    },                                                               
                    "$metadata": {                                                   
                    ...                                                
                    },                                                               
                    "$version": 7                                                    
                }                                                                  
            }                                                                    
        }

Asjade jaoturi seab seadme kaksikud nimetatakse **seadmete**dokumendi kogumina.
Nii toob järgmine päring seadme kaksikud terve kogumi alusel:

        SELECT * FROM devices

> [AZURE.NOTE] [Asjade jaoturi SDK-d] [ lnk-hub-sdks] toetavad Piipar suurte tulemuste.

Asjade jaoturi abil saavad tuua kaksikud suvalise tingimusega filtreerimine. Näiteks

        SELECT * FROM devices
        WHERE tags.location.region = 'US'

toob kaksikud **location.region** sildiga määratud **US**.
Toetatud brauserid ja aritmeetilised võrdlusi toetatakse ka, nt

        SELECT * FROM devices
        WHERE tags.location.region = 'US'
            AND properties.reported.telemetryConfig.sendFrequencyInSecs >= 60

toob kõik kaksikud asub USA-s telemeetria vähemalt iga minut saatmiseks konfigureeritud. Mugavuse huvides, on võimalik kasutada massiivikonstante koos **sisse** ja tehtemärgid, **NIN** (mitte tekstivormingus). Näiteks

        SELECT * FROM devices
        WHERE property.reported.connectivity IN ['wired', 'wifi']

toob kõik kaksikud, mis teatatud Wi-Fi- või juhtmega Ühenduvus. [WHERE-klausli] viidata[ lnk-query-where] täielik viide osas on filtreerimisvõimalused.

Toetatud on ka rühmitamise ja kogumeid. Näiteks

        SELECT properties.reported.telemetryConfig.status AS status,
            COUNT() AS numberOfDevices
        FROM devices
        GROUP BY properties.reported.telemetryConfig.status

Tagastab arvu seadmete iga telemeetria konfiguratsiooni olekut.

        [
            {
                "numberOfDevices": 3,
                "status": "Success"
            },
            {
                "numberOfDevices": 2,
                "status": "Pending"
            },
            {
                "numberOfDevices": 1,
                "status": "Error"
            }
        ]

Ülaltoodud näites illustreerib olukorda, kus kolm seadmete teatatud eduka konfiguratsiooni kaks on endiselt rakendamine konfiguratsiooni ja üks teatatud tõrke.

### <a name="c-example"></a>C# näide

Päringu funktsioon on avatud, [C# teenuse SDK] [ lnk-hub-sdks] klõpsake soovitud **RegistryManager** klassi.
Siin on lihtne päringu näide:

        var query = registryManager.CreateQuery("SELECT * FROM devices", 100);
        while (query.HasMoreResults)
        {
            var page = await query.GetNextAsTwinAsync();
            foreach (var twin in page) 
            {
                // do work on twin object
            }
        }

Pange tähele, kuidas **päringu** objekt on käivitatud lehe suurus (kuni 1000) ja seejärel mitu korda **GetNextAsTwinAsync** meetodite helistades saab tuua mitu lehekülge.
See on oluline märkida, et päringu objekti seab mitme **järgmise\***, vahemäluasukohaga suvandi päringu nõutud, st twin või töö objektide või lihtsat Json kasutatava prognooside kasutamisel.

### <a name="node-example"></a>Sõlm näide

Päringu funktsioonid on esitatud [sõlm teenuse SDK] [ lnk-hub-sdks] klõpsake soovitud **registri** objekti.
Siin on lihtne päringu näide:

        var query = registry.createQuery('SELECT * FROM devices', 100);
        var onResults = function(err, results) {
            if (err) {
                console.error('Failed to fetch the results: ' + err.message);
            } else {
                // Do something with the results
                results.forEach(function(twin) {
                    console.log(twin.deviceId);
                });

                if (query.hasMoreResults) {
                    query.nextAsTwin(onResults);
                }
            }
        };
        query.nextAsTwin(onResults);

Pange tähele, kuidas **päringu** objekt on käivitatud lehe suurus (kuni 1000) ja seejärel mitu korda **nextAsTwin** meetodite helistades saab tuua mitu lehekülge.
On oluline märkida, et päringu objekti seab mitme **järgmise\***, vahemäluasukohaga suvandi päringu nõutud, st twin või töö objektide või lihtsat Json kasutatava prognooside kasutamisel.

### <a name="limitations"></a>Piirangud

Praegu prognooside on toetatud ainult siis, kui kasutate liitmised, st mitteliidetud päringud saate kasutada ainult `SELECT *`. Lisaks koondamine toetatakse ainult koos rühmitamine.

## <a name="getting-started-with-jobs-queries"></a>Päringute töö alustamine

[Töö] [ lnk-jobs] võimalda käivitada toiminguid komplekti seadmete. Iga seadme Double sisaldab teavet tööd, mis on osa nimega **tööde haldamine**.
Loogiliselt,

        {                                                                      
            "deviceId": "myDeviceId",                                            
            "etag": "AAAAAAAAAAc=",                                              
            "tags": {                                                            
                ...                                                              
            },                                                                   
            "properties": {                                                      
                ...                                                                 
            },
            "jobs": [
                { 
                    "deviceId": "myDeviceId",
                    "jobId": "myJobId",    
                    "jobType": "scheduleTwinUpdate",            
                    "status": "completed",                    
                    "startTimeUtc": "2016-09-29T18:18:52.7418462",
                    "endTimeUtc": "2016-09-29T18:20:52.7418462",
                    "createdDateTimeUtc": "2016-09-29T18:18:56.7787107Z",
                    "lastUpdatedDateTimeUtc": "2016-09-29T18:18:56.8894408Z",
                    "outcome": {
                        "deviceMethodResponse": null   
                    }                                         
                },
                ...
            ]                                                             
        }

Praegu on selle saidikogumi queriable nimega **devices.jobs** asjade jaoturi päringu keeles.

Näiteks kõik tööd (viimase ja ajastatud), mis mõjutavad ühe seadme saamiseks võite kasutada järgmist päringut:

        SELECT * FROM devices.jobs
        WHERE devices.jobs.deviceId = 'myDeviceId'

Pange tähele, kuidas see päring pakub seadme olek (ja võib-olla vastus otsene meetod) iga töö tagastatud.
Samuti on võimalik filtreerimiseks klõpsake kõigi objektide **devices.jobs** saidikogumi atribuutide suvalise kahendmuutujaga tingimusega.
Näiteks järgmine päring:

        SELECT * FROM devices.jobs
        WHERE devices.jobs.deviceId = 'myDeviceId'
            AND devices.jobs.jobType = 'scheduleTwinUpdate'
            AND devices.jobs.status = 'completed'
            AND devices.jobs.createdTimeUtc > '2016-09-01'

toob kõik lõpuleviidud Double update tööd seadme **myDeviceId** pärast mai 2016 loodud.

Samuti on võimalik tuua ühe töö tulemusi seadme kohta.

        SELECT * FROM devices.jobs
        WHERE devices.jobs.jobId = 'myJobId'

### <a name="limitations"></a>Piirangud
Praegu ei toeta **devices.jobs** päringud:

* Prognooside, st ainult `SELECT *` on võimalik;
* Olukorrad, mis viitavad seadme Double Lisaks töö atribuudid, nagu on näidatud
* Peforming liitmised, nt count, avg, rühmitada.

## <a name="basics-of-an-iot-hub-query"></a>Asjade jaoturi päringu põhialused

Iga asjade jaoturi päringu koosneb valik ja osalaused kaudu ja valikuline WHERE ja GROUP BY klauslitest. Iga päring käivitatakse JSON dokumendikogumi, nt seadme kaksikud. FROM-klausli näitab dokumendikogumi abil saab näidata (**seadmes** või **devices.jobs**). Seejärel WHERE-klausli filter on rakendatud. Liitmised, puhul selle etapi tulemused rühmitatakse rida luuakse määratud klausel GROUP BY ja iga rühma, mis on määratletud SELECT-klauslis.

        SELECT <select_list>
        FROM <from_specification>
        [WHERE <filter_condition>]
        [GROUP BY <group_specification>]

## <a name="from-clause"></a>FROM-klausel

Klausel **< from_specification > kaudu** saate endale ainult kahest väärtusest: **saatja seadmed**, päringu seadme kaksikud või **saatja devices.jobs**, päringu töö seadme kohta üksikasjad.

## <a name="where-clause"></a>WHERE-klausel

Funktsiooni **kus < filter_condition >** klausel pole kohustuslik. Seda määrab tingimusi, mis saatja saidikogumi JSON dokumentidele peavad vastama, et kaasata tulemuse. Mis tahes JSON dokumendi peavad olema väärtustatavad "TRUE" tulemi kaasatavate määratud tingimustele.

Lubatud tingimused on kirjeldatud jaotises [avaldiste ja tingimuste][lnk-query-expressions].

## <a name="select-clause"></a>SELECT-klauslis

SELECT-klauslis (**Valige < select_list >**) on kohustuslikud ja määrab väärtused on toodud päringust. Seda määrab JSON väärtused abil saate luua uue JSON objektide jaoks iga element saatja saidikogumi, filtreeritud (ja soovi korral rühmitatud) alamhulk projektsiooni etapp loob uue JSON objekti, ehitatud SELECT-klauslis määratud väärtustega.

See on SELECT-klauslis grammatika:

        SELECT [TOP <max number>] <projection list>

        <projection_list> ::=
            '*'
            | <projection_element> AS alias [, <projection_element> AS alias]+

        <projection_element> :==
            attribute_name
            | <projection_element> '.' attribute_name
            | <aggregate>

        <aggregate> :==
            count(<projection_element>) | count()
            | avg(<projection_element>) | avg()
            | sum(<projection_element>) | sum()
            | min(<projection_element>) | min()
            | max(<projection_element>) | max()

Kui **attribute_name** viitab vara JSON dokumendi saatja saidikogumi. SELECT-klauslid näited leiate [Alustamine Double päringute] [ lnk-query-getstarted] jaotis.

Praegu valitud klauslitest erinevas **Valige \* ** toetatakse ainult kaksikud liitväärtuse päringud.

## <a name="group-by-clause"></a>Klausel GROUP BY

Klausel **GROUP BY < group_specification >** on valikuline etapp, mis on saab teostada pärast WHERE-klausli ja valige määratud projektsiooni enne määratud filter. Atribuudi väärtus põhinevates dokumentides rühmitab. Nende rühmade kasutatakse kokkuvõtlike väärtuste kuvamiskuju SELECT-klauslis määratud loomiseks.

Päringu abil GROUP BY näide on järgmine:

        SELECT properties.reported.telemetryConfig.status AS status,
            COUNT() AS numberOfDevices
        FROM devices
        GROUP BY properties.reported.telemetryConfig.status

Ametlik süntaks GROUP BY on:

        GROUP BY <group_by_element>
        <group_by_element> :==
            attribute_name
            | < group_by_element > '.' attribute_name

Kui **attribute_name** viitab vara JSON dokumendi saatja saidikogumi. 

Praegu toetatud klausel GROUP BY ainult siis, kui päringu kaksikud.

## <a name="expressions-and-conditions"></a>Avaldiste ja tingimused

Kõrge, *avaldis*:

* Hindab JSON tüüp (st kahendväärtus, arv, stringi, massiiv või objekti), näiteks ja
* On määratletud seadme JSON dokument ja konstandid funktsioonide ja sisseehitatud tehtemärkide kasutamine andmete käsitsemiseks.

*Tingimused* on avaldisi, mille tulemiks on kahendväärtus, st mis tahes konstandi teistsugune kui kahendmuutujaga **true** peetakse **false** (sh **null**, **määratlemata**, mis tahes objekti või massiivist eksemplari, mis tahes stringi ja selgelt kahendmuutujaga **false**).

Avaldiste süntaks on järgmine:

        <expression> ::=
            <constant> |
            attribute_name |
            unary_operator <expression> |
            <expression> binary_operator <expression> |
            <create_array_expression> |
            '(' <expression> ')'

        <constant> ::=
            <undefined_constant>
            | <null_constant> 
            | <number_constant> 
            | <string_constant> 
            | <array_constant> 

        <undefined_constant> ::= undefined
        <null_constant> ::= null
        <number_constant> ::= decimal_literal | hexadecimal_literal
        <string_constant> ::= string_literal
        <array_constant> ::= '[' <constant> [, <constant>]+ ']'

kui:

| Sümbol | Määratlus |
| -------------- | -----------------|
| attribute_name | Mis tahes atribuudi JSON dokumendi saatja saidikogumi. |
| unary_operator | Mis tahes unaarne tehtemärk tehtemärkide punkti. |
| binary_operator | Binaarsed tehtemärk tehtemärgid punkti. |
| decimal_literal | Hõljumine, mis on väljendatud koma märke. |
| hexadecimal_literal | String, millele järgneb string kuueteistkümnendarvu kohtade arvu 0 x väljendatud arv. |
| string_literal | Stringi literaalide on Unicode'i stringid tähistab null või rohkem Unicode'i märkide jada või paoklahvi järjestuse. Stringi literaalide on ülakomade (ülakoma: ") või topeltklõpsake hinnapakkumised (jutumärk:"). Lubatud põgenemine: `\'`, `\"`, `\\`, `\uXXXX` Unicode'i märkide määratletud 4 kuueteistkümnendarvu numbrit. |

### <a name="operators"></a>Tehtemärgid

Toetatakse järgmisi tehtemärke:

| Pere | Tehtemärgid |
| -------------- | -----------------|
| Aritmeetiline | +,-,*,/,% |
| Loogiline | JA, VÕI EI |
| Võrdlus |  =,! =, <>;, < =, > =, <> |

## <a name="next-steps"></a>Järgmised sammud

Saate teada, kuidas käivitada päringute abil [Asjade jaoturi SDK-d]rakendused[lnk-hub-sdks].

[lnk-query-where]: iot-hub-devguide-query-language.md#where-clause
[lnk-query-expressions]: iot-hub-devguide-query-language.md#expressions-and-conditions
[lnk-query-getstarted]: iot-hub-devguide-query-language.md#getting-started-with-twin-queries

[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-jobs]: iot-hub-devguide-jobs.md
[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md