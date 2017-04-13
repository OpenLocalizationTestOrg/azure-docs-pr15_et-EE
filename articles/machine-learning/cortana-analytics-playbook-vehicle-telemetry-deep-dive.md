<properties 
    pageTitle="Sõiduki telemeetria analytics lahenduse playbook: sügav sukelduda lahendus | Microsoft Azure'i" 
    description="Cortana ärianalüüsi võimaluste kasutamiseks saada reaalajas ja ennustav ülevaateid sõiduki seisundi ja juhtimise tarbetu." 
    services="machine-learning" 
    documentationCenter="" 
    authors="bradsev" 
    manager="jhubbard" 
    editor="cgronlun" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/12/2016" 
    ms.author="bradsev" />


# <a name="vehicle-telemetry-analytics-solution-playbook-deep-dive-into-the-solution"></a>Sõiduki telemeetria analytics lahenduse playbook: sügav sukelduda lahendus

See **menüü** viited selle playbook osad: 

[AZURE.INCLUDE [cap-vehicle-telemetry-playbook-selector](../../includes/cap-vehicle-telemetry-playbook-selector.md)]

Selles jaotises Trelle alla igasse etapid kujutatud lahenduse arhitektuur juhiseid ja viitu kohandamine. 

## <a name="data-sources"></a>Andmeallikad

Lahendus kasutab kaks erinevate andmeallikatega:

- **jäljendatud sõiduki signaale ja diagnostika andmekomplekti** ja 
- **sõiduki kataloogi**

Sõiduki telemaatika simulaatorit on see lahendus kaasatud. See eraldab diagnostikateave ja signaale vastav sõiduki riigile ja juhtimise mustri antud hetkel aega. Klõpsake [Sõiduki telemaatika simulaatorit](http://go.microsoft.com/fwlink/?LinkId=717075) allalaadimiseks **Sõiduki telemaatika simulaatorit Visual Studio lahendus** kohanduste oma vajaduste järgi. Sõiduki kataloogi sisaldab viide andmekomplekti koos mõne VIN mudeli kaardistamine.

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig2-vehicle-telematics-simulator.png)

*Joonis 2 – sõiduki telemaatika simulaatorit*

See on JSON-vormingus andmekomplekti, mis sisaldab järgmist skeemi.

Veerg | Kirjeldus | Väärtused 
 ------- | ----------- | --------- 
VIN | Juhuslik valmistajatehase | See on saadud 10 000 juhuslik tehasetähised juhtslaidi loendit.
Väljaspool temperatuur | Kui sõiduki sunnib väljaspool temperatuur | Juhuslik arv vahemikus 0 – 100
Mootori temperatuur | Temperatuur sõiduki | Juhuslik arv vahemikus 0 – 500
Kiiruse | Kus sunnib sõiduki mootori kiirus | Juhuslik arv vahemikus 0 – 100
Kütuse | Kütusetase sõiduki | Juhuslik arv vahemikus 0 – 100 (näitab kütuse protsent)
EngineOil | Otsimootori oil tase sõiduki | Juhuslik arv vahemikus 0 – 100 (näitab engine oil protsent)
Rõhk | Sõiduki rehvirõhku | CEIP genereeritud numbri 0-50 (näitab rehvi rõhk protsent)
Läbisõidumõõdiku | Sõiduki läbisõidumõõdiku lugemine | Juhuslik arv vahemikus 0 – arv 200 000
Accelerator_pedal_position | Accelerator pedaali asukoha sõiduki | Juhuslik arv vahemikus 0 – 100 (näitab accelerator protsent)
Parking_brake_status | Näitab, kas sõiduk on pargitud või ei | Tõene või väär
Headlamp_status | Näitab, kus esilatern on või mitte | Tõene või väär
Brake_pedal_status | Näitab, kas piduripedaal vajutamisel või ei | Tõene või väär
Transmission_gear_position | Sõiduki asukoha edastamine hammasratas | Riigid: esimene, teine, kolmas, neljas, viies, kuuenda, seitsmenda, kaheksanda
Ignition_status | Näitab, kas on töötab või on peatatud | Tõene või väär
Windshield_wiper_status | Näitab, kas klaasipuhastajatega on sisse lülitatud või ei | Tõene või väär
ABS | Näitab, kas ABS osaleb või ei | Tõene või väär
Ajatempli | Ajatempli andmepunkti loomisel | Kuupäev
Linna | Auto asukoht | See lahendus 4 linnad: Bellevue Redmond, Sammamish, Seattle


Sõiduki mudeli viide andmekomplekti sisaldab VIN mudeli kaardistamine. 

VIN | Mudel |
--------------|------------------
FHL3O1SA4IEHB4WU1 | Sedaan |
8J0U8XCPRGW4Z3NQE | Hübriid |
WORG68Z2PLTNZDBI7 | Pere Saloon |
JTHMYHQTEPP4WBMRN | Sedaan |
W9FTHG27LZN1YWO0Y | Hübriid |
MHTP9N792PHK08WJM | Pere Saloon |
EI4QXI2AXVQQING4I | Sedaan |
5KKR2VB4WHQH97PF8 | Hübriid |
W9NSZ423XZHAONYXB | Pere Saloon |
26WJSGHX4MA5ROHNL | Kabriolett |
GHLUB6ONKMOSI7E77 | Universaal |
9C2RHVRVLMEJDBXLP | Kompaktne auto |
BRNHVMZOUJ6EOCP32 | Väike maastur |
VCYVW0WUZNBTM594J | Sportauto |
HNVCE6YFZSA5M82NY | Keskmine maastur |
4R30FOR7NUOBL05GJ | Universaal |
WYNIIY42VKV6OQS1J | Suur maastur |
8Y5QKG27QET1RBK7I | Suur maastur |
DF6OX2WSRA6511BVG | Coupe |
Z2EOZWZBXAEW3E60T | Sedaan |
M4TV6IEALD5QDS3IR | Hübriid |
VHRA1Y2TGTA84F00H | Pere Saloon |
R0JAUHT1L1R3BIKI0 | Sedaan |
9230C202Z60XX84AU | Hübriid |
T8DNDN5UDCWL7M72H | Pere Saloon |
4WPYRUZII5YV7YA42 | Sedaan |
D1ZVY26UV2BFGHZNO | Hübriid |
XUF99EW9OIQOMV7Q7 | Pere Saloon
8OMCL3LGI7XNCC21U | Kabriolett |
…….  |   |


### <a name="to-generate-simulated-data"></a>Jäljendatud andmete loomiseks
1.  Andmete simulaatorit paketi allalaadimiseks klõpsake sõiduki telemaatika simulaatorit sõlm paremas ülaservas olevat noolt. Salvestage ja ekstrakti faile kohalikult teie arvutis. ![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig3-vehicle-telemetry-blueprint.png)*Joonis 3 – sõiduki telemeetria Analytics lahenduse näidis*

2.  Minge oma kohalikus arvutis kaust, kuhu ekstraktitud paketi sõiduki telemaatika simulaatorit. ![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig4-vehicle-telematics-simulator-folder.png)*Joonis 4 – sõiduki telemaatika simulaatorit kausta*

3.  Rakenduse **CarEventGenerator.exe**käivitada.

### <a name="references"></a>Viited

[Sõiduki telemaatika simulaatorit Visual Studio lahendus](http://go.microsoft.com/fwlink/?LinkId=717075) 

[Azure'i sündmuse jaoturi](https://azure.microsoft.com/services/event-hubs/)

[Azure'i andmed Factory](https://azure.microsoft.com/documentation/learning-paths/data-factory/)


## <a name="ingestion"></a>Manustamisest
Azure'i sündmuse jaoturi ja voo Analytics andmete Factory kombinatsioonid on võimendada, et neelata sõiduki signaale, diagnostika sündmused ja reaalajas ja analüüsi. Kõik need osad on loodud ja konfigureeritud lahenduse juurutamine osana. 

### <a name="real-time-analysis"></a>Reaalajas analüüs
Sündmuste loodud sõiduki telemaatika simulaatorit on avaldatud sündmuse jaoturi sündmuse jaoturi SDK abil. Voo Analytics töö saaks neid sündmusi sündmuse keskuse kaudu ja töötleb andmete reaalajas sõiduki seisundi analüüsimiseks. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig5-vehicle-telematics-event-hub-dashboard.png) 

*Joonis 5 – sündmuse jaoturi armatuurlaud*

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig6-vehicle-telematics-stream-analytics-job-processing-data.png) 

*Joonis 6 – voo analytics töö andmete töötlemiseks*

Voo analytics töö;

- saaks andmete sündmuse keskuse kaudu 
- sooritab ühenduse viide andmetega, et vastendada vastava mudeli auto VIN-kood 
- püsib need üheks Azure'i bloobimälu rikkaliku paketi Analyticsi. 

Jää püsima andmed üheks Azure'i bloobimälu kasutatakse voo analytics järgmine päring. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig7-vehicle-telematics-stream-analytics-job-query-for-data-ingestion.png) 

*Joonis 7 - voo analytics töö päringu andmete kohta*

### <a name="batch-analysis"></a>Paketi analüüs
Me loovad ka täiendavad hulga jäljendatud sõiduki signaale ja diagnostika andmekomplekti rikkalikumat paketi Analyticsi. See on nõutav hea andmeid mahu jaoks Pakktöötlus tagamiseks. Selleks kasutame nimega "PrepareSampleDataPipeline" müügivõimaluste Azure'i andmed Factory töövoo ühe aasta väärtuses jäljendatud sõiduki signaale ja diagnostika andmekomplekti loomiseks. Klõpsake [andmete Factory kohandatud tegevuse](http://go.microsoft.com/fwlink/?LinkId=717077) allalaadimiseks andmete Factory kohandatud DotNet tegevuse Visual Studio lahendus kohanduste oma vajaduste järgi. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig8-vehicle-telematics-prepare-sample-data-for-batch-processing.png) 

*Joonis 8 - paketi töötlemine töövoo jaoks Näidisandmete ettevalmistamine*

Tulemas koosneb kohandatud ADF .net tegevuse Kuva siin:

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig9-vehicle-telematics-prepare-sample-data-pipeline.png) 

*Joonis 9 - PrepareSampleDataPipeline*

Kui tulemas täidab edukalt ja märgitud aasta väärt jäljendatud sõiduki signaale ja diagnostika "Valmis", "RawCarEventsTable" andmekomplekti toodeti andmed. Kuvatakse järgmised kaust ja teie salvestusruumi konto jaotises "connectedcar" container loodud fail.

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig10-vehicle-telematics-prepare-sample-data-pipeline-output.png) 

*Joonis 10 - PrepareSampleDataPipeline väljund*

### <a name="references"></a>Viited

[Azure'i sündmuse jaoturi SDK voo kohta](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)

[Azure'i andmed Factory andmete liikumine võimaluste](../data-factory/data-factory-data-movement-activities.md)
[Azure andmete Factory DotNet tegevus](../data-factory/data-factory-use-custom-activities.md)

[Azure'i andmed Factory DotNet tegevuse visual studio lahendus Näidisandmete ettevalmistamine](http://go.microsoft.com/fwlink/?LinkId=717077) 


## <a name="partition-the-dataset"></a>Andmekomplekti Partition

Töötlemata poolstruktuur sõiduki signaale ja diagnostika andmekomplekti on liigendatud andmete ettevalmistamine etapis sellises vormingus, aasta ja kuu. See eraldamine soodustab tõhusam päringu- ja scalable pikemaks, võimaldades viga üle ühe bloobimälu kontolt teisele, nagu on esimene konto täitub. 

>[AZURE.NOTE] Selles etapis tuleb lahendus on mõeldud ainult Pakktöötlus.

Sisend ja väljund andmehaldus andmeid:

- **Andmete** (nimega *PartitionedCarEventsTable*) on säilitatakse pikka aega põhilisi / "rawest" vorm andmete järves kliendi"andmed". 
- **Sisendandmete** selle müügivõimaluste tavaliselt soovite hüljata, nagu andmete väljund on täielik töökindluse, et sisend - lihtsalt talletatakse (liigendatud) paremini edaspidiseks kasutamiseks.

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig11-vehicle-telematics-partition-car-events-workflow.png)

*Sektsiooni auto sündmuste töövoo joonis 11*

Funktsiooni toorandmetega on liigendatud taru Hdinsightiga tegevuste kasutamine "PartitionCarEventsPipeline". Näidisandmete loodud samm 1 aasta on liigendatud aasta ja kuu. Sektsioonid kasutatakse sõiduki signaale ja Diagnostikaandmete loomiseks iga kuu (Kokku 12 sektsioonid) aasta kohta. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig12-vehicle-telematics-partition-car-events-pipeline.png)

*Joonis 12 - PartitionCarEventsPipeline*

Järgmise taru skripti, nimega "partitioncarevents.hql", kasutatakse jagamine ja allalaaditud zip kausta "\demo\src\connectedcar\scripts" asub. 


    SET hive.exec.dynamic.partition=true;
    SET hive.exec.dynamic.partition.mode = nonstrict;
    set hive.cli.print.header=true;

    DROP TABLE IF EXISTS RawCarEvents; 
    CREATE EXTERNAL TABLE RawCarEvents 
    (
                vin                             string,
                model                           string,
                timestamp                       string,
                outsidetemperature              string,
                enginetemperature               string,
                speed                           string,
                fuel                            string,
                engineoil                       string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position      string,
                parking_brake_status            string,
                headlamp_status                 string,
                brake_pedal_status              string,
                transmission_gear_position      string,
                ignition_status                 string,
                windshield_wiper_status         string,
                abs                             string,
                gendate                         string
                
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:RAWINPUT}'; 

    DROP TABLE IF EXISTS PartitionedCarEvents; 
    CREATE EXTERNAL TABLE PartitionedCarEvents 
    (
                vin                             string,
                model                           string,
                timestamp                       string,
                outsidetemperature              string,
                enginetemperature               string,
                speed                           string,
                fuel                            string,
                engineoil                       string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position      string,
                parking_brake_status            string,
                headlamp_status                 string,
                brake_pedal_status              string,
                transmission_gear_position      string,
                ignition_status                 string,
                windshield_wiper_status         string,
                abs                             string,
                gendate                         string
    ) partitioned by (YearNo int, MonthNo int) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:PARTITIONEDOUTPUT}';

    DROP TABLE IF EXISTS Stage_RawCarEvents; 
    CREATE TABLE IF NOT EXISTS Stage_RawCarEvents 
    (
                vin                             string,
                model                           string,
                timestamp                       string,
                outsidetemperature              string,
                enginetemperature               string,
                speed                           string,
                fuel                            string,
                engineoil                       string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position      string,
                parking_brake_status            string,
                headlamp_status                 string,
                brake_pedal_status              string,
                transmission_gear_position      string,
                ignition_status                 string,
                windshield_wiper_status         string,
                abs                             string,
                gendate                         string,
                YearNo                          int,
                MonthNo                         int) 
    ROW FORMAT delimited fields terminated by ',' LINES TERMINATED BY '10';

    INSERT OVERWRITE TABLE Stage_RawCarEvents
    SELECT
        vin,            
        model,
        timestamp,
        outsidetemperature,
        enginetemperature,
        speed,
        fuel,
        engineoil,
        tirepressure,
        odometer,
        city,
        accelerator_pedal_position,
        parking_brake_status,
        headlamp_status,
        brake_pedal_status,
        transmission_gear_position,
        ignition_status,
        windshield_wiper_status,
        abs,
        gendate,
        Year(gendate),
        Month(gendate)

    FROM RawCarEvents WHERE Year(gendate) = ${hiveconf:Year} AND Month(gendate) = ${hiveconf:Month}; 

    INSERT OVERWRITE TABLE PartitionedCarEvents PARTITION(YearNo, MonthNo) 
    SELECT
        vin,            
        model,
        timestamp,
        outsidetemperature,
        enginetemperature,
        speed,
        fuel,
        engineoil,
        tirepressure,
        odometer,
        city,
        accelerator_pedal_position,
        parking_brake_status,
        headlamp_status,
        brake_pedal_status,
        transmission_gear_position,
        ignition_status,
        windshield_wiper_status,
        abs,
        gendate,
        YearNo,
        MonthNo
    FROM Stage_RawCarEvents WHERE YearNo = ${hiveconf:Year} AND MonthNo = ${hiveconf:Month};

*Joonis 13 - PartitionConnectedCarEvents taru skripti*

Kui tulemas on edukalt täidetud, kuvatakse järgmised sektsioonid loodud jaotises "connectedcar" container konto salvestusruumi.

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig14-vehicle-telematics-partitioned-output.png)

*Joonis 14 - sektsioonitud väljund*

Andmed on nüüd optimeeritud, on paremini hallatavaks ja valmis saada rikkaliku paketi ülevaateid edasiseks töötlemiseks. 

## <a name="data-analysis"></a>Andmeanalüüs

Selles jaotises näete, kuidas ühendada Azure'i voo analüüsi, Azure seadme õ, Azure'i andmed Factory ja Azure Hdinsighti rikas täpsemate analytics sõiduki seisundi ja juhtimise tarbetu. On kolme lõigetes siin.

1.  **Seadme õ**: käesoleva sisaldab teavet normaalne tuvastamise katse, et meil kasutada seda lahendust prognoosida nõudva teenindamine hooldus ja nõudva meenutab tõttu turvalisuse probleemid.
2.  **Reaalajas analüüsi**: käesoleva sisaldab reaalajas analytics voo Analytics Päringukeelt kasutavate ja operationalizing masina õ katse reaalajas kasutades kohandatud rakenduse puudutav teave.
3.  **Paketi analüüsi**: käesoleva sisaldab teavet ja kohta, muutes selle paketi andmete kasutamine Windows Azure Hdinsightiga ja Azure arvuti koolitus, Azure'i andmed Factory operationalized.

### <a name="machine-learning"></a>Seadme õpetused

Meie eesmärk siin on prognoosida hooldustööd või teatud heath statistika vastavalt tagasikutsumise vajavate sõidukite. Teeme järgmised oletused

- Kui üks järgmised kolm tingimust on täidetud, nõuavad sõidukite **teenindamine hooldus**:
    - Rõhk on väike
    - Otsimootori oil on madal
    - Mootori temperatuur on kõrge

- Kui üks järgmistest tingimustest on täidetud, sõidukite **turvalisuse probleemi** ja nõua **tagasivõtmise**.
    - Mootori temperatuur on kõrge, kuid väljaspool temperatuur on väike
    - Mootori temperatuur on madal, kuid väljaspool temperatuur kõrge

Eelmise vajaduste järgi, oleme loonud kaks eraldi mudelit kõrvalekaldeid, üks sõiduki hoolduse tuvastamise ja üks sõiduki tagasivõtmise tuvastamise tuvastamiseks. Sisseehitatud põhisumma komponent analüüsi (PKL) algoritmi kasutatakse mõlema mudeli normaalne tuvastamise. 

**Hooldus tuvastamise mudel**

Kui üks kolmest näitajad – rõhk, engine oil või mootori temperatuur – selle vastavad tingimusele, aruannete hooldus tuvastamise mudel normaalne. Seetõttu tuleb ainult need kolm muutujate koostamise mudeli uurida. Meie katse Azure seadme õ, esmalt kasutame mooduli **Andmekomplekti veergude valimine** nende kolme muutujate eraldamiseks. Järgmine kasutame PKL-põhiste normaalne tuvastamise mooduli luua normaalne tuvastamise mudel. 

Põhisumma komponent analüüsi (PKL) on loodud tehnika masina õppe, mida saab rakendada normaalne tuvastamise funktsioonide valik ja liigitamine. PKL teisendab juhul võib-olla seotud muutujat sisaldava nimega põhikomponentide väärtuste kogumi kogum. Võtme idee modelleerimine PKL-põhine on projekti andmete kirjutamiseks alumises mitmedimensioonilised ruumi nii, et funktsioonid ja kõrvalekaldeid saate hõlpsamini eristada.
 
Iga uue sisendi tuvastamise mudeli normaalne detektor esmalt arvutab selle projektsiooni klõpsake soovitud eigenvectors, ja seejärel arvutab normaliseeritud ülesehitus tõrge. Normaliseeritud tõrke on normaalne Keskmine. Mida suurem on tõrge, rohkem anomaalsete on eksemplari. 

Hoolduse tuvastamise probleem, saate iga kirje pidada määratletud punkti 3 mitmedimensioonilised ruumi rõhk, engine oil ja mootori temperatuur koordinaatide alusel. Jäädvustada neid kõrvalekaldeid, me peale 2 mitmedimensioonilised ruumi, kasutades PKL projekti algsed andmed 3 mitmedimensioonilised alale. Seega me seada parameetri komponentide PKL abil 2. See parameeter on oluline roll rakendamise PKL-põhiste normaalne tuvastamise. Pärast väljaulatuvate andmete PKL, saate selgitame neid kõrvalekaldeid hõlpsam.

**Tagasi kutsuda normaalne tuvastamise mudel** Tagasivõtmise normaalne tuvastamise mudelis kasutame andmekomplekti ja PKL-põhiste normaalne veergude valimine tuvastamise moodulid sarnaselt. Täpsemalt me ekstrakti esmalt kolm muutujat – mootori temperatuur, väljaspool temperatuur ja kiirus - **Veergude valimine andmekomplekti** mooduli kasutamise. Meil on ka kiiruse muutuja Kuna temperatuur on tavaliselt seotud kiirust. Järgmine kasutame PKL-põhiste normaalne tuvastamise mooduli projekti 3 mitmedimensioonilised ruumi andmeid peale 2 mitmedimensioonilised ruumi. Tagasivõtmise kriteeriumid on ja seda nõuab sõiduki tagasivõtmise kui mootori temperatuur ja väljaspool temperatuur on negatiivselt tugevalt seotud. PKL-põhiste normaalne tuvastusalgoritm kasutamisel me saate hõivata kõrvalekaldeid pärast PKL. 

Kui kumbki mudel koolitus, läheb vaja kasutada tavaline andmeid, mis ei nõua hooldustööd või tagasivõtmise kui sisendandmete koolitada PKL-põhiste normaalne tuvastamise mudel. Hinded katse kasutame tuvastada, kas sõiduki nõuab hooldustööd või tagasivõtmise koolitatud normaalne tuvastamise mudel. 


### <a name="real-time-analysis"></a>Reaalajas analüüs

Järgmised voo Analytics SQL-päringu kasutatakse keskmise olulise parameetrite nagu kiiruse, kütusetase, mootori temperatuur, läbisõidumõõdiku lugemise, rõhk, mootor taset ja teistele saada. Funktsiooni keskmiste kasutatakse leida kõrvalekaldeid, probleemi teatised, ja kindlakstegemiseks üldine seisund sıidukipargi kindla ala ja seejärel oleksid selle demograafilistest. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig15-vehicle-telematics-stream-analytics-query-for-real-time-processing.png)

Joonis 15 – voo analytics päringu reaalajas töötlemiseks

Kõik keskmiste arvutatakse 3 sekundi TumblingWindow üle. Me ei kasuta TubmlingWindow sel juhul, kuna me kattuvad ja järjestikuste ajavahemike. 

"Akendesüsteemide" võimaluste Azure'i voo Analytics kohta lisateabe saamiseks klõpsake [akendesüsteemide (Azure'i voo Analytics)](https://msdn.microsoft.com/library/azure/dn835019.aspx).

**Reaalajas ennustamine**

Rakendus on lahendus käivitama arvuti õ mudeli reaalajas kaasatud. Selle rakenduse nimega "RealTimeDashboardApp" on loodud ja konfigureeritud lahenduse juurutamine osana. Rakenduse teeb järgmist:

1.  Sündmuse jaoturi eksemplari, kus voo Analytics on avaldamise sündmuste mustri pidevalt on kuulata. ![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig16-vehicle-telematics-stream-analytics-query-for-publishing.png)*Joonis 16 – voo analytics päringu väljund andmete avaldamine sündmuse jaoturi eksemplari* 

2.  Iga sündmuse selle rakenduse saab: 

    - Töötleb andmeid, kasutades arvuti õ päringu vastuse hinded (RRS) lõpp-punkti. Juurutamise käigus automaatselt avaldatakse RRS lõpp-punkti.
    - RRS väljund on avaldatud PowerBI andmekomplekt tõuketeatised API-de abil.

See muster kehtib ka stsenaariumid, mida soovite integreerida reaalajas analytics voogu, nt teatisi, teatiste ja sõnumside stsenaariumid ärivaldkonna (LoB) rakendus.

Klõpsake nuppu [RealtimeDashboardApp alla laadida](http://go.microsoft.com/fwlink/?LinkId=717078) alla laadida kohanduste RealtimeDashboardApp Visual Studio lahendus. 

**Reaalajas armatuurlaua rakenduse käivitada**

1.  Klõpsake diagrammi vaate sõlme PowerBI paanil atribuudid "Alla laadida reaalajas armatuurlaua rakendus" linki. ![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig17-vehicle-telematics-powerbi-dashboard-setup.png)*Joonis 17 – PowerBI armatuurlaua häälestusjuhised*
2.  Ekstraktida ja kohalikult salvestada ![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig18-vehicle-telematics-realtimedashboardapp-folder.png) *joonis 18 – RealtimeDashboardApp kaust*
3.  Rakenduse RealtimeDashboardApp.exe käivitada
4.  Kehtiv Power BI mandaat, logige sisse ja klõpsake nuppu Aktsepteeri ![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig19a-vehicle-telematics-realtimedashboardapp-sign-in-to-powerbi.png)![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig19b-vehicle-telematics-realtimedashboardapp-sign-in-to-powerbi.png) 

*Joonis 19 – RealtimeDashboardApp: Logige sisse PowerBI*

>[AZURE.NOTE] Kui soovite tühjendage Alustuseks PowerBI andmekomplekti, käivitada RealtimeDashboardApp parameetriga "flushdata": 

    RealtimeDashboardApp.exe -flushdata

### <a name="batch-analysis"></a>Paketi analüüs

Eesmärk siin on näidata, kuidas Contoso mootorite kasutab Azure Arvuta võimaluste rakendada suur andmed saada rikkaliku teadmisi mustri, kasutus käitumine ja sõiduki seisundi kohta. See on võimalik, et:

- Klientide programmikasutuskogemuse parandada ja muuta odavamaks pakkudes ülevaateid tarbetu ja kütuse tõhusa juhtimise käitumist
- Lisateavet aktiivselt kliendid ja nende juhtimise patters haldavad äriotsuseid ja esitage parim klassi tooted ja teenused

See lahendus me suunatud järgmised mõõdikud:

1.  **Juhtimise agressiivne käitumine**: tuvastab mudelid, asukohad, juhtimise tingimused ja aja jooksul saada teadmisi, agressiivne juhtimise mustrite trendi. Contoso mootorite saate kasutada järgmisi ülevaateid turunduskampaaniate, isikupärastatud uued funktsioonid ja kasutus-põhiste kindlustus.
2.  **Kütuse tõhusa juhtimise käitumine**: tuvastab mudelid, asukohad, juhtimise tingimused ja aja jooksul saada teadmisi, kütuse tõhusa juhtimise mustrite trendi. Contoso mootorite saate kasutada järgmisi ülevaateid turunduskampaaniate, auto juhtimise ajal uusi funktsioone ja aktiivne aruandlus draiverid maksumus tõhus ja keskkonna sõbralik juhtimise tarbetu. 
3.  **Tagasi kutsuda mudelite**: tuvastab mudelite vajavaid meenutab operationalizing normaalne tuvastamise seadme Õppekeskuse katse

Vaatame iga nende mõõdikute üksikasjad


**Agressiivne juhtimise muster**

Sektsioonitud sõiduki signaale ja Diagnostikaandmete töödeldakse nimega "AggresiveDrivingPatternPipeline" taru määratlemiseks mudelid, asukoht, sõiduki, juhtimise tingimuste ja muude parameetrite abil, mis eksponeerib agressiivne juhtimise mustri teel.

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig20-vehicle-telematics-aggressive-driving-pattern.png) 
*Joonis 20 – Aggressive mustri töövoo juhtimine*

"\Demo\src\connectedcar\scripts" kausta alla ZIP asub taru skripti nimega "aggresivedriving.hql" agressiivne juhtimise tingimus mustri analüüsimiseks kasutada. 

    DROP TABLE IF EXISTS PartitionedCarEvents; 
    CREATE EXTERNAL TABLE PartitionedCarEvents
    (
                vin                             string,
                model                           string,
                timestamp                       string,
                outsidetemperature              string,
                enginetemperature               string,
                speed                           string,
                fuel                            string,
                engineoil                       string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position      string,
                parking_brake_status            string,
                headlamp_status                 string,
                brake_pedal_status              string,
                transmission_gear_position      string,
                ignition_status                 string,
                windshield_wiper_status         string,
                abs                             string,
                gendate                         string
                                
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:PARTITIONEDINPUT}';

    DROP TABLE IF EXISTS CarEventsAggresive; 
    CREATE EXTERNAL TABLE CarEventsAggresive
    (
                vin                         string, 
                model                       string,
                timestamp                   string,
                city                        string,
                speed                       string,
                transmission_gear_position  string,
                brake_pedal_status          string,
                Year                        string,
                Month                       string
                                
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:AGGRESIVEOUTPUT}';



    INSERT OVERWRITE TABLE CarEventsAggresive
    select
    vin,
    model,
    timestamp,
    city,
    speed,
    transmission_gear_position,
    brake_pedal_status,
    "${hiveconf:Year}" as Year,
    "${hiveconf:Month}" as Month
    from PartitionedCarEvents
    where transmission_gear_position IN ('fourth', 'fifth', 'sixth', 'seventh', 'eight') AND brake_pedal_status = '1' AND speed >= '50'

*Joonis 21 – Aggressive mustri taru päringu juhtimine*

Hoolimatu/agressiivne juhtimise käitumise põhjal pidurdamise mustri kiire tuvastamiseks kasutab sõiduki edastamise hammasratta asukohta, piduri pedaali olek ja kiiruse kombinatsioon. 

Kui tulemas on edukalt täidetud, kuvatakse järgmised sektsioonid loodud jaotises "connectedcar" container konto salvestusruumi.

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig22-vehicle-telematics-aggressive-driving-pattern-output.png) 

*Joonis 22 – AggressiveDrivingPatternPipeline väljund*


**Kütuse tõhusa juhtimise muster**

Sektsioonitud sõiduki signaale ja Diagnostikaandmete töödeldakse nimega "FuelEfficientDrivingPatternPipeline" teel. Mudelid, asukoht, sõiduki, juhtimise tingimuste ja muid atribuute, et panna kütuse tõhusa juhtimise mustri määratlemiseks kasutatakse taru.

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig23-vehicle-telematics-fuel-efficient-driving-pattern.png) 

*Joonis 23 – kütuse tõhusa juhtimise mustri töövoo*

"\Demo\src\connectedcar\scripts" kausta alla ZIP asub taru skripti nimega "fuelefficientdriving.hql" agressiivne juhtimise tingimus mustri analüüsimiseks kasutada. 

    DROP TABLE IF EXISTS PartitionedCarEvents; 
    CREATE EXTERNAL TABLE PartitionedCarEvents
    (
                vin                             string,
                model                           string,
                timestamp                       string,
                outsidetemperature              string,
                enginetemperature               string,
                speed                           string,
                fuel                            string,
                engineoil                       string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position      string,
                parking_brake_status            string,
                headlamp_status                 string,
                brake_pedal_status              string,
                transmission_gear_position      string,
                ignition_status                 string,
                windshield_wiper_status         string,
                abs                             string,
                gendate                         string
                                
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:PARTITIONEDINPUT}';

    DROP TABLE IF EXISTS FuelEfficientDriving; 
    CREATE EXTERNAL TABLE FuelEfficientDriving
    (
                vin                         string, 
                model                       string,
                city                        string,
                speed                       string,
                transmission_gear_position  string,                
                brake_pedal_status          string,            
                accelerator_pedal_position  string,                             
                Year                        string,
                Month                       string
                                
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:FUELEFFICIENTOUTPUT}';



    INSERT OVERWRITE TABLE FuelEfficientDriving
    select
    vin,
    model,
    city,
    speed,
    transmission_gear_position,
    brake_pedal_status,
    accelerator_pedal_position,
    "${hiveconf:Year}" as Year,
    "${hiveconf:Month}" as Month
    from PartitionedCarEvents
    where transmission_gear_position IN ('fourth', 'fifth', 'sixth', 'seventh', 'eight') AND parking_brake_status = '0' AND brake_pedal_status = '0' AND speed <= '60' AND accelerator_pedal_position >= '50'


*Joonis 24 – kütuse tõhusa juhtimise mustri taru päring*

Kasutab 's edastamise hammasratta asukoha kombinatsiooni, piduri pedaali olek, kiiruse ja accelerator pedaali asukoha tuvastamiseks kütuse tõhusa juhtimise käitumise põhjal kiirenduse, pidurdamise ja mustrite kiirus. 

Kui tulemas on edukalt täidetud, kuvatakse järgmised sektsioonid loodud jaotises "connectedcar" container konto salvestusruumi.

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig25-vehicle-telematics-fuel-efficient-driving-pattern-output.png) 

*Joonis 25 – FuelEfficientDrivingPatternPipeline väljund*


**Ennustatud väärtuste tagasivõtmine**

Õppekeskuse katse masina on ette valmistatud ja avaldatud veebiteenuse osana lahenduse juurutamine. Lõpp-punkti hinded pakett on kasutada selle töövoo registreeritud andmete factory lingitud teenust ja operationalized andmete factory paketi hinded tegevuse abil.

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig26-vehicle-telematics-machine-learning-endpoint.png) 

*Lingitud andmete factory teenust registreeritud Joonis 26 – seadme Õppekeskuse lõpp-punkti*

Registreeritud lingitud teenuse kasutatakse funktsiooni DetectAnomalyPipeline Keskmine normaalne tuvastamise mudeli andmed. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig27-vehicle-telematics-aml-batch-scoring.png) 

*Joonis 27 – andmete factory tegevusest Azure seadme õ paketi hinded* 

On mõned toimingud tehtud selle müügivõimaluste andmete koostamiseks nii, et see saab operationalized paketi hinded veebiteenuse abil. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig28-vehicle-telematics-pipeline-predicting-recalls.png) 

*Joonis 28 – DetectAnomalyPipeline sõidukid meenutab prognoosimiseks* 

Kui loendamine on lõppenud, kasutatakse Hdinsightiga tegevuse töötlemine ja andmeid, mis on liigitatud kõrvalekaldeid tõenäosuse Keskmine, 0,60 mudeli abil või uuem versioon.

    DROP TABLE IF EXISTS CarEventsAnomaly; 
    CREATE EXTERNAL TABLE CarEventsAnomaly 
    (
                vin                         string,
                model                       string,
                gendate                     string,
                outsidetemperature          string,
                enginetemperature           string,
                speed                       string,
                fuel                        string,
                engineoil                   string,
                tirepressure                string,
                odometer                    string,
                city                        string,
                accelerator_pedal_position  string,
                parking_brake_status        string,
                headlamp_status             string,
                brake_pedal_status          string,
                transmission_gear_position  string,
                ignition_status             string,
                windshield_wiper_status     string,
                abs                         string,
                maintenanceLabel            string,
                maintenanceProbability      string,
                RecallLabel                 string,
                RecallProbability           string
                                
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:ANOMALYOUTPUT}';

    DROP TABLE IF EXISTS RecallModel; 
    CREATE EXTERNAL TABLE RecallModel 
    (

                vin                         string,
                model                       string,
                city                        string,
                outsidetemperature          string,
                enginetemperature           string,
                speed                       string,
                Year                        string,
                Month                       string              
                                
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:RECALLMODELOUTPUT}';

    INSERT OVERWRITE TABLE RecallModel
    select
    vin,
    model,
    city,
    outsidetemperature,
    enginetemperature,
    speed,
    "${hiveconf:Year}" as Year,
    "${hiveconf:Month}" as Month
    from CarEventsAnomaly
    where RecallLabel = '1' AND RecallProbability >= '0.60'


Kui tulemas on edukalt täidetud, kuvatakse järgmised sektsioonid loodud jaotises "connectedcar" container konto salvestusruumi.

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig30-vehicle-telematics-detect-anamoly-pipeline-output.png) 

*Joonis 30 – joonis 30 – DetectAnomalyPipeline väljund*


## <a name="publish"></a>Avaldamine

### <a name="real-time-analysis"></a>Reaalajas analüüs

Üks töösse voo analytics päringud avaldab sündmuste väljund sündmuse jaoturi eksemplari. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig31-vehicle-telematics-stream-analytics-job-publishes-output-event-hub.png)

*Joonis 31 – voo analytics töö avaldab väljund sündmuse jaoturi eksemplari*

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig32-vehicle-telematics-stream-analytics-query-publish-output-event-hub.png)

*Joonis 32 – voo analytics päringu väljundisse avaldamiseks sündmuse jaoturi eksemplari*

Selle voo sündmuste on tarbitud RealTimeDashboardApp, mis sisaldab lahendus. Selle rakenduse mõjutab selle seadme õ päringu vastuse veebiteenuse reaalajas hinded ja avaldab saadavate andmete PowerBI andmekomplekti ettenähtud. 

### <a name="batch-analysis"></a>Paketi analüüs

Tulemuste paketi ja reaalajas töötlemine on avaldatud tarbimine Azure'i SQL-andmebaasi tabelid. Azure SQL Serveri andmebaas ja tabelid luuakse automaatselt skripti häälestamise käigus. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig33-vehicle-telematics-batch-processing-results-copy-to-data-mart.png)

*Joonis 33 – Batch processing andmete mart töövoo tulemuste koopia*

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig34-vehicle-telematics-stream-analytics-job-publishes-to-data-mart.png)

*Joonis 34 – voo analytics töö avaldab andmevaka*

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig35-vehicle-telematics-data-mart-setting-in-stream-analytics-job.png)

*Joonis 35 – andmete mart voo analytics töö alustamisega*


## <a name="consume"></a>Tarbimine

Power BI annab see lahendus rikkaliku armatuurlaua reaalajas andmed ja ennustav visualiseeringuid. 

Klõpsake siin PowerBI aruandeid ja armatuurlaud häälestamise juhised. Lõplik armatuurlaua näeb välja umbes järgmine:

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig36-vehicle-telematics-powerbi-dashboard.png)

*Joonis 36 - PowerBI armatuurlaua*

## <a name="summary"></a>Kokkuvõte

See dokument sisaldab üksikasjalikku süvitsimineku sõiduki telemeetria Analytics lahendus. See tutvustatakse lambda arhitektuur mustri reaalajas ja analüüsi prognoose ja toimingud. See muster kehtib mitmesuguseid kasutamise juhtudel, mis nõuavad kuum tee (reaalajas) ja analytics külma tee (paketi). 
