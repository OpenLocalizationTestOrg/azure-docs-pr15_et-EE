<properties
    pageTitle="Prognoosi kujutatud tehnilise juhendist nõuda | Microsoft Azure'i"
    description="Tehniline juhend lahenduse malli abil Microsoft Cortana ärianalüüsi nõudmisel kujutatud prognoosi koostamine."
    services="cortana-analytics"
    documentationCenter=""
    authors="yijichen"
    manager="ilanr9"
    editor="yijichen"/>

<tags
    ms.service="cortana-analytics"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/16/2016"
    ms.author="inqiu;yijichen;ilanr9"/>

# <a name="technical-guide-to-the-cortana-intelligence-solution-template-for-demand-forecast-in-energy"></a>Cortana ärianalüüsi lahenduse malli nõudmisel prognoosi koostamine kujutatud tehniline juhend

## <a name="overview"></a>**Ülevaade**

Mõne E2E demo peal Cortana ärianalüüsi komplekti koostamise protsessi kiirendamiseks lahenduse mallid. Juurutatud malli ettevalmistamise tellimuse vajalikud Cortana ärianalüüsi komponent ja koostada vahelisi seoseid. See ka seemnete andmete müügivõimaluste näidisandmetega saada loodud andmete simulatsioon rakendusest. Andmete simulaatorit linki allalaadimine ja installimine oma kohalikus arvutis, vaadake readme.txt faili juhised kasutate simulaatorit. Funktsiooni simulaatorit genereeritud andmed kuvatakse hüdraat andmete müügivõimaluste ja genereerimine masina õ ennustamine, mis saab siis tuleb visualiseeritakse Power BI armatuurlaua start.

Lahendus malli leiate [siit](https://gallery.cortanaintelligence.com/SolutionTemplate/Demand-Forecasting-for-Energy-1) 

Juurutamise protsessi juhendab teid samm-sammult häälestamine lahenduse mandaat. Veenduge, et salvestate neid mandaate, nt lahenduse nimi, kasutajanimi ja parool, mida pakuvad juurutamise käigus.

Selle dokumendi eesmärk on selgitada viide arhitektuur ja eri osade ette valmistatud teie tellimus selle lahenduse malli osana. Dokumendi räägib ka tegelikke andmeid oma näeksid teie andmete võitis ülevaateid/prognoose Näidisandmete asendamiseks tehke järgmist. Lisaks selle dokumendi räägib osad lahenduse malli, mida oleks vaja muuta, kui soovite kohandada oma andmetega lahendus. Lõpus on esitatud juhised, kuidas luua Power BI armatuurlaua selle lahenduse malli.

## <a name="big-picture"></a>**Suur pilt**

![](media\cortana-analytics-technical-guide-demand-forecast\ca-topologies-energy-forecasting.png)

### <a name="architecture-explained"></a>Arhitektuuri ülevaade
Lahenduse juurutamisel erinevate Azure'i teenuste Cortana Analytics komplekti on aktiveeritud (*st* keskuses sündmuse *voo Analytics, Hdinsightiga, andmete Factory, seadme õ jne*). Ülaltoodud arhitektuur skeem kuvatakse kõrge, kuidas vajadusel prognoosimise kujutatud lahenduse malli ehitatakse lõpuni. Mida saab uurida järgmisi teenuseid neile lahenduse malli skeemi loodud lahenduse juurutamise klõpsates. Järgmistes jaotistes kirjeldatakse iga tekstilõigu.

## <a name="data-source-and-ingestion"></a>**Andmeallika ja manustamisest**

### <a name="synthetic-data-source"></a>Sünteetiline andmeallikas

Selle malli kasutada andmeallika luuakse töölauarakendus, et te alla laadida ja käivitada kohalikult pärast juurutamise. Siit leiate juhised Laadige alla ja installige see rakendus atribuudid ribal esimese sõlm nimega kujutatud prognoosi koostamiseks andmete simulaatorit lahenduse malli diagrammi valimisel. Selle rakenduse kanalid [Azure'i sündmuse jaoturi](#azure-event-hub) teenuse andmepunkte või sündmusi, lahenduse voogu ülejäänud kasutatud.

Sündmuse loomine rakenduse asustada Azure'i sündmuse jaoturi ainult siis, kui see on teie arvutis käivitamist.

### <a name="azure-event-hub"></a>Azure'i sündmuse jaoturi

[Azure'i sündmuse jaoturi](https://azure.microsoft.com/services/event-hubs/) teenus on eespool kirjeldatud sünteetiliste andmeallika esitatud andmete adressaadile.

## <a name="data-preparation-and-analysis"></a>**Andmete ettevalmistamine ja analüüs**


### <a name="azure-stream-analytics"></a>Azure'i voo Analytics

[Azure'i voo Analytics](https://azure.microsoft.com/services/stream-analytics/) teenuse kasutatakse lähedal reaalajas analytics Sisestuskeel voogu [Azure'i sündmuse jaoturi](#azure-event-hub) teenuse ja avaldamise tulemuste [Power BI](https://powerbi.microsoft.com) armatuurlaua kui ka kõik töötlemata sissetulevate sündmusi [Azure'i](https://azure.microsoft.com/services/storage/) salvestusteenus hiljem töötlemiseks [Azure'i andmed Factory](https://azure.microsoft.com/documentation/services/data-factory/) teenus võttis arhiivimine.

### <a name="hd-insights-custom-aggregation"></a>HD ülevaateid kohandatud koondamine

Azure'i HD ülevaate teenuse kasutatakse skripte [taru](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) (juhitud Azure'i andmed Factory) esitada liitmised töötlemata sündmusi, mis olid arhiivitakse Azure'i voo Analytics teenuse kasutamise kohta.

### <a name="azure-machine-learning"></a>Azure'i masina õpetused

[Azure'i masina õ](https://azure.microsoft.com/services/machine-learning/) teenuse kasutamise (juhitud Azure'i andmed Factory) tulevaste energiatarbimist antud saadud sisendeid kindla piirkonna prognoosi koostamise teha.

## <a name="data-publishing"></a>**Andmete avaldamine**


### <a name="azure-sql-database-service"></a>SQL Azure'i andmebaasi teenus

[Azure'i SQL-andmebaasi](https://azure.microsoft.com/services/sql-database/) teenuse abil saab salvestada (haldab Azure'i andmed Factory) saadud Azure seadme õ teenus, mis on tarbitud [Power BI](https://powerbi.microsoft.com) armatuurlaua prognoose.

## <a name="data-consumption"></a>**Andmete tarbimine**

### <a name="power-bi"></a>Power BI

[Power BI](https://powerbi.microsoft.com) teenust kasutatakse armatuurlaud, mis sisaldab liitmised antud [Azure'i voo Kasutusanalüüsi](https://azure.microsoft.com/services/stream-analytics/) teenus ning vajadusel prognoosida tulemusi talletatud [Azure'i SQL-andmebaasi](https://azure.microsoft.com/services/sql-database/) , koostatud [Azure seadme õ](https://azure.microsoft.com/services/machine-learning/) teenuse abil. Juhised, kuidas luua Power BI armatuurlaua see lahendus Mall, vaadake allpool olevat jaotist.

## <a name="how-to-bring-in-your-own-data"></a>**Kuidas oma andmeid tuua**

Selles jaotises kirjeldatakse, kuidas oma andmeid tuua Azure ja mis valdkondi nõuaks muudatusi saate tuua see arhitektuur andmed.

See on tõenäoline, et kõik andmekomplekti tuua vastaks selle lahenduse malli kasutatakse andmekomplekti. Andmete ja nõuetele on oluline, kuidas saate muuta selle malli oma andmetega töötamiseks. Kui see on teie esimene Azure seadme õ teenus, saate teenuseettevõtte tutvustus, kasutades näiteks [oma esimese katse loomise kohta](machine-learning\machine-learning-create-experiment.md).

Järgmistes jaotistes arutada Mall, mida on vaja muuta, kui on kasutusele uue andmehulga jaotised.

### <a name="azure-event-hub"></a>Azure'i sündmuse jaoturi

[Azure'i sündmuse jaoturi](https://azure.microsoft.com/services/event-hubs/) teenus on väga üldine nii, et andmeid saab sisestada jaoturi CSV- või JSON-vormingus. Azure'i sündmuse keskuses ilmneb pole teisiti töötlemine, kuid on oluline mõistate juhitakse seda andmeid.

Selles dokumendis kirjeldatud, kuidas oma andmeid neelata, kuid ühte hõlpsalt saata sündmusi või andmete Azure'i sündmuse jaoturiga, kasutades [Sündmuse jaoturi API](event-hubs\event-hubs-programming-guide.md).

### <a name="azure-stream-analytics"></a>Azure'i voo Analytics

[Azure'i voo Analytics](https://azure.microsoft.com/services/stream-analytics/) teenuse kasutatakse lähedal reaalajas analytics andmevoogu lugemine ja kirjutamine suvalist arvu allikatest andmeid.

Nõudmisel prognoosimine kujutatud lahenduse malli, Azure'i voo Analytics päringu koosneb kahe sub päringu iga tarbimine sündmuste Azure'i sündmuse jaoturi teenuse sisendina ja väljundid on kaks erinevat asukohta. Need väljundid koosnevad ühe Power BI andmekomplekti ja Azure talletuskoht.

Leiate [Azure'i voo Analytics](https://azure.microsoft.com/services/stream-analytics/) päringu järgi:

-   [Azure'i haldusportaal](https://manage.windowsazure.com/) sisse logimine

-   Voo analytics töö asukoha ![](media\cortana-analytics-technical-guide-demand-forecast\icon-stream-analytics.png) mis loodi lahenduse juurutamisel. Üks on lükkamine andmete Bloobivahemälu salvestusruumi (nt mytest1streaming432822asablob) ja teine on mõeldud lükkamine andmed Power BI (nt mytest1streaming432822asapbi).


-   Valimine

    -   Päringu sisendi kuvamiseks ***SISENDINA***

    -   ***PÄRINGU*** ise päringu vaatamiseks

    -   ***Tulemused*** erinevad väljundid kuvamiseks

[Voo Analytics päringu viide](https://msdn.microsoft.com/library/azure/dn834998.aspx) MSDN-is leiate Azure'i voo Analytics päringu ehituse teavet.

See lahendus on Azure voo Analytics tööst, mis väljundid andmekomplekt on lähedal reaalajas analytics teavet Power BI armatuurlaud sissetulevate andmevoos selle lahenduse malli osana esitatud. Kuna seal on peidetud teadmisi sissetulevate andmete vormindamine, need päringud oleks vaja muuta oma andmete vormingu põhjal.

Muud Azure'i voo Analytics töö väljundid kõik [Sündmuse jaoturi](https://azure.microsoft.com/services/event-hubs/) sündmusi [Azure Storage](https://azure.microsoft.com/services/storage/) ja seega nõuab sõltumata teie andmete vormingu muutmise, nagu on täielik sündmuse teabe voona salvestusruumi.

### <a name="azure-data-factory"></a>Azure'i andmed Factory

[Azure'i andmed Factory](https://azure.microsoft.com/documentation/services/data-factory/) teenuse orchestrates liikumine ja andmete töötlemiseks. Rakenduses nõudmisel prognoosimise kujutatud lahenduse malli tehakse andmete factory kaksteist [torujuhtmete](data-factory\data-factory-create-pipelines.md) teisaldamine ja töödelda erinevate tehnoloogiate andmeid.

  Pääsete oma andmete factory, avades andmete Factory sõlm lahenduse malli skeemi loodud lahenduse juurutamise allosas. See viib teid andmete hankimise kohta oma Azure haldusportaali. Kui näete oma andmekomplektide jaotises tõrkeid, saate need, kui need on tingitud andmete factory kasutusele enne andmete generaator käivitati ignoreerida. Need vead ei takista oma andmete factory toimimist.

Selles jaotises käsitletakse vajalikud [torujuhtmete](data-factory\data-factory-create-pipelines.md) ja [tegevuste](data-factory\data-factory-create-pipelines.md) [Azure'i andmed Factory](https://azure.microsoft.com/documentation/services/data-factory/). Allpool on diagrammivaate lahenduse.

![](media\cortana-analytics-technical-guide-demand-forecast\ADF2.png)

Viis selle factory torujuhtmete sisaldavad [taru](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) skriptide kasutatakse partition ja andmeid koondada. Kui märkida skriptide asub [Azure Storage](https://azure.microsoft.com/services/storage/) konto häälestamise ajal loodud. Nende asukoht: demandforecasting\\\\skripti\\\\taru\\ \\ (või https://[Your lahenduse name].blob.core.windows.net/demandforecasting).

Sarnaselt [Azure'i voo Analytics](#azure-stream-analytics-1) päringud, [taru](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) skriptide on peidetud teadmisi sissetulevate andmete vormindamine, need päringud oleks vaja muuta andmete vormindamine ja [funktsioon matemaatika](machine-learning\machine-learning-feature-selection-and-engineering.md) nõuete alusel.

#### <a name="aggregatedemanddatato1hrpipeline"></a>*AggregateDemandDataTo1HrPipeline*

See [müügivõimaluste](data-factory\data-factory-create-pipelines.md) müügivõimaluste sisaldab ühe tegevuse - [HDInsightHive](data-factory\data-factory-hive-activity.md) tegevuse abil [HDInsightLinkedService](https://msdn.microsoft.com/library/azure/dn893526.aspx) [taru](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) skripti liita iga 10 sekundi käivituva voona nõudmisel andmete alajaama tasemes tunni piirkond tasemele ja sellele [Azure](https://azure.microsoft.com/services/storage/) Storage Azure'i voo Analytics töö kaudu.

Selle eraldamine ülesande jaoks [taru](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) script on ***AggregateDemandRegion1Hr.hql***


#### <a name="loadhistorydemanddatapipeline"></a>*LoadHistoryDemandDataPipeline*

Selle [müügivõimaluste](data-factory\data-factory-create-pipelines.md) sisaldab kaks tegevust.
- [HDInsightHive](data-factory\data-factory-hive-activity.md) tegevuse abil [HDInsightLinkedService](https://msdn.microsoft.com/library/azure/dn893526.aspx) käivituva taru skripti kord tunnis ajalugu nõudmisel andmeid alajaama taseme tunni piirkond tasemele ja Azure Storage Azure'i voo Analytics töö käigus

- [Kopeeri](https://msdn.microsoft.com/library/azure/dn835035.aspx) tegevus, mis viib kokkuvõtlike andmete salvestusruumi Azure'i bloobimälu Azure SQL-andmebaasiga, mis on ette valmistatud lahenduse malli installimise käigus.

Selle ülesande jaoks [taru](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) script on ***AggregateDemandHistoryRegion.hql***.


#### <a name="mlscoringregionxpipeline"></a>*MLScoringRegionXPipeline*

Nende [torujuhtmete](data-factory\data-factory-create-pipelines.md) sisaldavad mitut tegevust ja mille seetõttu on poolitusjoonega prognoose Azure seadme õ katse seostatud lahendus selle malli põhjal. Need on identne peale üks neist tegeleb ainult teises regioonis, mis on tehtud erinevate RegionID ADF müügivõimaluste ja taru script on möödas iga piirkonna jaoks.  
Selles sisalduvad tegevused on:
-   [HDInsightHive](data-factory\data-factory-hive-activity.md) tegevuse abil [HDInsightLinkedService](https://msdn.microsoft.com/library/azure/dn893526.aspx) käivituva taru skripti liitmised ja funktsioon matemaatika vajalik Azure seadme õ katse sooritamiseks. Taru skriptid selle ülesande jaoks on vastav ***PrepareMLInputRegionX.hql***.

-   [Kopeeri](https://msdn.microsoft.com/library/azure/dn835035.aspx) tegevus, mis viib tulemused [HDInsightHive](data-factory\data-factory-hive-activity.md) tegevuse üle ühe Azure Storage bloobimälu, mida saab Accessi [AzureMLBatchScoring](https://msdn.microsoft.com/library/azure/dn894009.aspx) tegevuse alusel.

-   [AzureMLBatchScoring](https://msdn.microsoft.com/library/azure/dn894009.aspx) tegevust, mis nõuab Azure seadme õ proovida, mille tulemuseks on tulemused pannakse ühe Azure Storage Bloobivahemälu.

#### <a name="copyscoredresultregionxpipeline"></a>*CopyScoredResultRegionXPipeline*
Nende [torujuhtmete](data-factory\data-factory-create-pipelines.md) sisaldavad ühe tegevuse - [Kopeeri](https://msdn.microsoft.com/library/azure/dn835035.aspx) tegevuse, mis viib Azure seadme õ katse tulemused vastav ***MLScoringRegionXPipeline*** Azure SQL-andmebaasiga, mis on ette valmistatud lahenduse malli installimise käigus.

#### <a name="copyaggdemandpipeline"></a>*CopyAggDemandPipeline*
See [torujuhtmete](data-factory\data-factory-create-pipelines.md) sisaldavad ühe tegevuse - [Kopeeri](https://msdn.microsoft.com/library/azure/dn835035.aspx) tegevust, mis teisaldab liidetud poolelioleva nõudmisel andmed ***LoadHistoryDemandDataPipeline*** Azure SQL-andmebaasiga, mis on ette valmistatud lahenduse malli installimise käigus.

#### <a name="copyregiondatapipeline-copysubstationdatapipeline-copytopologydatapipeline"></a>*CopyRegionDataPipeline, CopySubstationDataPipeline, CopyTopologyDataPipeline*
Nende [torujuhtmete](data-factory\data-factory-create-pipelines.md) sisaldavad ühe tegevuse - [Kopeeri](https://msdn.microsoft.com/library/azure/dn835035.aspx) tegevust, mis teisaldab Topologygeo/piirkond/alajaama viide andmed, mis on üles laaditud salvestusruumi Azure'i bloobimälu osana lahenduse malli installimise Azure SQL-andmebaasiga, mis on ette valmistatud lahenduse malli installimise käigus.

### <a name="azure-machine-learning"></a>Azure'i masina õpetused
Selle lahenduse malli kasutada [Azure seadme õ](https://azure.microsoft.com/services/machine-learning/) katse pakub vajadusel piirkonna ennustamiseks. Katse on seotud tarbitud andmehulga ja seetõttu on vaja muuta või asendada konkreetsed andmed, mida on toodud.

## <a name="monitor-progress"></a>**Edenemise jälgimine**
Pärast andmete generaator käivitatakse, tulemas hakkab saada hüdreeritud ja teie lahendus erinevate osade alustada lööd jagada käske välja andmete Factory toimingu järgmist. On kaks võimalust, et saate jälgida tulemas.

1. Märkige ruut Azure'i bloobimälu andmeid.

    Üks voo Analytics töö kirjutab sissetulevate toorandmetega bloobimälu. Kui klõpsate **Azure'i bloobimälu** osa teie lahendus kuval saate edukalt juurutatud lahendus ja klõpsake nuppu **Ava** paremal, see viib teid [Azure haldusportaali](https://portal.azure.com). Kui seal, klõpsake **plekid**. Järgmise paanil kuvatakse ümbriste loendit. Klõpsake **"energysadata"**. Järgmise paneeli, kuvatakse kaust **"demandongoing"** . Kaustas rawdata kuvatakse kaustad nimedega nagu kuupäev = 2016-01-28 jne. Kui te ei näe neid kaustu, see näitab, et töötlemata andmete on edukalt on teie arvutis loodud ja talletatud bloobimälu. Peaksite nägema faile, mis peaks olema piiratud suurused MB need kaustad.

2. Märkige ruut Azure'i SQL-andmebaasi andmeid.

    Viimases etapis tulemas on kirjutada andmeid (nt prognoose õ seadme kaudu) SQL-andmebaasi. Peate ootama kuni 2 tunni jaoks andmeid kuvada SQL-andmebaasis. Üks võimalus jälgida, kui palju andmeid on saadaval SQL-andmebaasis on kaudu [Azure'i haldusportaal](https://manage.windowsazure.com/). Leidke vasakul SQL ANDMEBAASE![](media\cortana-analytics-technical-guide-demand-forecast\SQLicon2.png) ja klõpsake seda. Seejärel leidke oma andmebaasi (st demo123456db) ja klõpsake seda. Klõpsake järgmisel lehel jaotises **"Andmebaasi ühenduse loomine"** , **"Käivita Transact-SQL-i päringute vastu SQL-andmebaasi"**.

    Siin, võite klõpsata uue päringu ja päringu jaoks (nt "select count(*) kaudu DemandRealHourly) ridade arvu" andmebaasi kasvab, tabeli ridade arvu, mis peaks suurendama.)

3. Märkige ruut Power BI Armatuurlaua andmed.

    Saate häälestada Power BI kuum tee armatuurlaua sissetulevate toorandmetega jälgimiseks. Järgige juhiseid jaotises "Power BI armatuurlaud".



## <a name="power-bi-dashboard"></a>**Power BI armatuurlaua**

### <a name="overview"></a>Ülevaade

Selles jaotises kirjeldatakse, kuidas luua Power BI armatuurlaua Azure voo Analytics (kuum tee) reaalajas andmete visualiseerimiseks, samuti prognoosida tulemusi õppeteema (külma) Azure arvutist.


### <a name="setup-hot-path-dashboard"></a>Saadaolevad tee armatuurlaua häälestamine

Järgmised toimingud juhendab teid visualiseerimine reaalajas andmed väljund voo Analytics tööd, mis loodi lahenduse juurutamine ajal tehke järgmist. [Power BI võrgus](http://www.powerbi.com/) konto on nõutav tehke järgmist. Kui teil pole kontot, saate [luua](https://powerbi.microsoft.com/pricing).

1.  Lisage Power BI väljundi Azure'i voo Analytics (Ash).

    -  Peate järgige juhiseid teemas [Azure voo analüüsimine ja Power BI: reaalajas nähtavuse streaming andmete reaalajas analytics armatuurlaua](stream-analytics-power-bi-dashboard.md) Power BI armatuurlaua Azure'i voo Analytics töö väljund häälestamiseks.

    - Otsige üles oma [Azure'i haldusportaal](https://manage.windowsazure.com)voo analytics töö. Töö nimi peaks olema: YourSolutionName + streamingjob"" + juhuslik arv + "asapbi" (st demostreamingjob123456asapbi).

    - Lisage PowerBI väljundi ASA töö. Määrata **väljundi pseudonüüm** **"PBIoutput"**. Saate häälestada **Andmekomplekti nimi** ja **Tabeli nimi** kujul **"EnergyStreamData"**. Kui olete lisanud väljund, **"Start"** lehe allosas voo Analytics töö alustamiseks klõpsake. Saate kinnitusteade (*nt*, "Alustades voo analytics töö myteststreamingjob12345asablob õnnestus").

2. Logige sisse [Power BI võrgus](http://www.powerbi.com)

    -   Vasakpoolsel paanil andmekomplektide jaotise minu tööruumis peaks olema näeksid uue andmekomplekti, kus on näha vasakpoolsel paanil Power BI. See on voogesituse andmete olete lükanud Azure'i voo Analytics eelmises etapis.

    -   Veenduge, et paani ***visualiseeringud*** on avatud ja kuvatakse ekraani paremas servas.

3. "Nõudmisel ajatempli" paani loomiseks tehke järgmist.
    -   Klõpsake vasakpoolsel paanil andmekomplektide jaotise andmekomplekti **'EnergyStreamData'** .

    -   Klõpsake ikooni **"joondiagramm"** ![](media\cortana-analytics-technical-guide-demand-forecast\PowerBIpic8.png).

    -   Klõpsake paanil **väljad** 'EnergyStreamData'.

    -   Klõpsake **"Ajatempli"** ja veenduge, et see kuvatakse jaotises "Telje". Klõpsake **"Koormus"** ja veenduge, et see kuvatakse jaotises "Väärtused".

    -   Klõpsake nuppu **Salvesta** ülaosas ja nime aruanne "EnergyStreamDataReport". Aruande nimega "EnergyStreamDataReport" kuvatakse vasakul paanil navigaator jaotist aruanded.

    -   Valige **"Kinnita visuaalse"** ![](media\cortana-analytics-technical-guide-demand-forecast\PowerBIpic6.png) see joondiagramm paremas ülanurgas ikooni, "PIN-koodi armatuurlaud" akna võib ilmu saab valida armatuurlaua. Palun valige "EnergyStreamDataReport", siis valige "Kinnita".

    -   Hõljutage kursorit üle armatuurlaual sellel paanil, klõpsake nuppu "Redigeeri" nimega "Nõudmisel ajatempli" selle pealkirja muutmine paremas ülanurgas ikooni

4.  Muud armatuurlaua paanid vastav andmekomplektide põhjal luua. Lõplik armatuurlaua vaade on näidatud allpool.
        ![](media\cortana-analytics-technical-guide-demand-forecast\PBIFullScreen.png)


### <a name="setup-cold-path-dashboard"></a>Külm tee armatuurlaua häälestamine
Külm tee andmete teel, on oluline eesmärk on saada nõudmisel prognoosi iga piirkonna. Power BI loob Azure SQL-andmebaasi selle ennustamine tulemite talletamiseks andmeallikana.

> [AZURE.NOTE] 1) kulub paari tunni koguda piisavalt prognoositud tulemuste armatuurlaud. Soovitame alustada selle protsessi 2-3 tundi pärast saate Lõuna andmete generaator. (2) selles juhises on nõutav alla ja installige tasuta tarkvara [Power BI Desktopi](https://powerbi.microsoft.com/desktop).



1.  Andmebaasi mandaadi saamiseks.

    Peate enne järgmist **Andmebaasiserveri nimi, andmebaasi nimi, kasutajanimi ja parool** . Järgnevalt kirjeldatakse, kuidas neid leida suunata.

    -   Kui teie lahendus malli skeemi **"Azure'i SQL-andmebaasi"** roheliseks, klõpsake seda ja seejärel nuppu **"Ava"**. Siis suunavad Azure haldusportaali ja oma andmebaasi teave lehe avaneb samuti.

    -   Klõpsake lehel leiate jaotist "Andmebaasi". See on loetletud teie loodud andmebaasi välja. Andmebaasi nimi peaks olema **"Lahenduse nimi + juhuslik arv + 'db'"** (nt "mytest12345db").

    -   Klõpsake oma andmebaasi uus pop välja pannel, leiate oma andmebaasiserveri nimi peal. Andmebaasi serveri nimi nimi shoud olema **"Lahenduse nimi + juhuslik arv + 'database.windows.net,1433'"** (nt "mytest12345.database.windows.net,1433").

    -   Teie andmebaasi **kasutajanimi** ja **parool** on sama kasutajanime ja parooli varem salvestatud ajal lahenduse juurutamise.

2.  Power BI külma tee faili andmeallika värskendamine
    -  Veenduge, et olete installinud [Power BI Desktopi](https://powerbi.microsoft.com/desktop)uusim versioon.

    -   **"DemandForecastingDataGeneratorv1.0"** kausta alla laadida, topeltklõpsake faili **"Power BI Template\DemandForecastPowerBI.pbix"** . Algne visualiseeringud on fiktiivne andmete põhjal. **Märkus:** Kui kuvatakse tõrge Massaaž, veenduge, et olete installinud Power BI Desktopi uusim versioon.

        Kui avate seda faili peal, klõpsake nuppu **Redigeeri päringud**. Pop välja aknas, topeltklõpsake **'Allika'** paremal.
    ![](media\cortana-analytics-technical-guide-demand-forecast\PowerBIpic1.png)

    -   Välja aknas pop, **"Server"** ja **"Andmebaasi"** asendamine oma server ja andmebaas nimed ja klõpsake nuppu **"OK"**. Serveri nimi, veenduge, et teie määratud pordi 1433 (**YourSolutionName.database.windows.net 1433**). Ignoreeri hoiatusteated, mis kuvatakse ekraanil.

    -   Järgmise pop välja aknas, kuvatakse teile kaks suvandid vasakpoolsel paanil (**Windowsi** ja **andmebaasi**). Klõpsake nuppu **"Andmebaasi"**, sisestage oma **"Kasutajanimi"** ja **"Parool"** (see on kasutajanimi ja parool sisestatud kui esmalt juurutatud lahendus ja Azure SQL-andmebaasi loodud). ***Nende sätete rakendamiseks valida, milliseid***, märkige taseme suvandit andmebaasi. Klõpsake nuppu **"Connect"**.

    -   Kui te kasutate juhendamisega tagasi eelmisele lehele, sulgege aken. Sõnumi pop välja – klõpsake nuppu **Rakenda**. Lõpuks klõpsake muudatuste salvestamiseks nuppu **Salvesta** . Power BI faili nüüd on loodud ühendus serveriga. Kui teie visualiseeringud on tühi, veenduge, et visualiseerida kõik andmed, klõpsates nuppu Kustutuskumm legendid paremas ülanurgas ikooni visualiseeringute Valikud tühjendamist. Nupp Värskenda kasutada uute andmete arutleda visualiseering. Algselt, siis kuvatakse ainult seemne andmete visualiseeringutes nagu andmete factory on ajastatud värskendamine iga 3 tunni. 3 tunni pärast, kuvatakse uus prognoose kajastuksid oma visualiseeringuid, kui andmete värskendamine.

3. (Valikuline) [Power BI võrgus](http://www.powerbi.com/)külma tee armatuurlaua avaldada. Pange tähele, et seda toimingut ka Power BI konto (või Office 365 konto).

    -   Klõpsake nuppu **"Avalda"** ja hiljem sekundi aknas kuvatakse "Avaldamine Power BI edu!" koos roheline märge. Klõpsake linki "Ava demoprediction.pbix Power BI" all. Üksikasjalikud juhised leiate teemast [avaldamine Power BI Desktopi kaudu](https://support.powerbi.com/knowledgebase/articles/461278-publish-from-power-bi-desktop).

    -   Uue armatuurlaua loomine: klõpsake soovitud **+** Logi vasakul paanil jaotises **armatuurlaudade** kõrval. Sisestage nimi "Nõudmisel prognoosi koostamiseks Demo" selle armatuurlaud.

    -   Kui avate aruande, klõpsake ![](media\cortana-analytics-technical-guide-demand-forecast\PowerBIpic6.png) kõikide visualiseeringute armatuurlauale kinnitamine. Üksikasjalikud juhised leiate teemast [PIN-koodi paan Power BI armatuurlaua aruandest](https://support.powerbi.com/knowledgebase/articles/430323-pin-a-tile-to-a-power-bi-dashboard-from-a-report).
        Minge lehele armatuurlaua ja suuruse ja asukoha oma visualiseeringuid ja nende redigeerimine. Kuidas muuta paanide üksikasjalikud juhised leiate teemast [Redigeeri paani--suurust, teisaldamine, ümbernimetamine ja PIN-koodi, kustutada, lisage hüperlink](https://powerbi.microsoft.com/documentation/powerbi-service-edit-a-tile-in-a-dashboard/#rename). Siin on mõni näide armatuurlaud, kus mõned külma tee visualiseeringute kinnitatud.

        ![](media\cortana-analytics-technical-guide-demand-forecast\PowerBIpic7.png)

4. (Valikuline) Andmeallika värskendamise ajastamine.
    -     Andmete värskendamise ajakava, et Hõljutage kursorit **EnergyBPI lõplik** andmekomplekti, klõpsake ![](media\cortana-analytics-technical-guide-demand-forecast\PowerBIpic3.png) ja klõpsake nuppu **Värskenda ajakava**.
    **Märkus:** Kui kuvatakse hoiatus Massaaž, klõpsake nuppu **Redigeeri identimisteavet** ja veenduge, et teie andmebaasi mandaat on samad, mis on kirjeldatud juhises 1.

    ![](media\cortana-analytics-technical-guide-demand-forecast\PowerBIpic4.png)

    -   Laiendage **Värskenda ajakava** . Lülitage sisse "hoida oma andmed ajakohane".

    -   Vastavalt oma vajadustele Värskenda ajakava. Lisateavet leiate teemast [andmete värskendamine Power BI](https://powerbi.microsoft.com/documentation/powerbi-refresh-data/).


## <a name="how-to-delete-your-solution"></a>**Kuidas kustutada teie lahendus**
Veenduge, et lõpetate andmete generaator kui aktiivselt ei kasutamine andmete generaator töötab lahendus on kulud suuremad. Kustutage lahenduse, kui te ei kasuta seda. Teie lahendus kustutamisel kustutatakse kõik komponendid ette valmistatud lahenduse juurutamisel teie tellimus. Kui soovite kustutada lahenduse klõpsake vasakul paanil lahenduse malli oma lahenduse nime ja klõpsake käsku Kustuta.

## <a name="cost-estimation-tools"></a>**Maksumuse hindamine tööriistad**

Järgmised kaks tööriistad on olemas, et aidata teil paremini mõista kulude töötab nõudmisel prognoosi koostamiseks kujutatud lahenduse malli teie tellimus:

-   [Microsoft Azure'i kulude prognoos tööriist (võrgus)](https://azure.microsoft.com/pricing/calculator/)

-   [Microsoft Azure'i kulude prognoos tööriist (töölauaversioon)](http://www.microsoft.com/download/details.aspx?id=43376)

## <a name="acknowledgements"></a>**Kinnitused**
Selles artiklis on autoriks andmete teadlane Yijing Chen ja Tammsalu Microsofti Qiu Min.
