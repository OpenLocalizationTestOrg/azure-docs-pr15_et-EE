<properties
    pageTitle="Cortana ärianalüüsi lahenduse malli sõnastikupõhise hooldustööd aerospace ja muude ettevõtete tehniline juhend | Microsoft Azure'i"
    description="Tehniline juhend lahenduse malli abil Microsoft Cortana ärianalüüsi sõnastikupõhise säilitamine aerospace, Utiliidid ja transport."
    services="cortana-analytics"
    documentationCenter=""
    authors="fboylu"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="cortana-analytics"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="fboylu" />

# <a name="technical-guide-to-the-cortana-intelligence-solution-template-for-predictive-maintenance-in-aerospace-and-other-businesses"></a>Cortana ärianalüüsi lahenduse malli sõnastikupõhise hooldustööd aerospace ja muude ettevõtete tehniline juhend

## <a name="acknowledgements"></a>**Kinnitused**
Selles artiklis on autoriks andmeteadlaste Yan Zhang, Gauher Shaheen, Fidan Boylu Uz ja Tammsalu Dan Grecoe Microsofti.

## <a name="overview"></a>**Ülevaade**

Mõne E2E demo peal Cortana ärianalüüsi komplekti koostamise protsessi kiirendamiseks lahenduse mallid. Juurutatud malli ettevalmistamise tellimuse vajalikud Cortana ärianalüüsi komponendid ja luua seoseid nende vahel. See ka seemnete andmete müügivõimaluste näidisandmetega genereeritud andmed generaator rakendus, mis saate alla laadida ja installida oma kohalikus arvutis pärast juurutamist lahenduse mall. Generaator genereeritud andmed kuvatakse hüdraat andmete müügivõimaluste ja genereerimine masina õ prognoose, mis saab siis tuleb visualiseeritakse Power BI armatuurlaua start. Juurutamise protsessi juhendab teid samm-sammult häälestamine lahenduse mandaat. Veenduge, et salvestate neid mandaate, nt lahenduse nimi, kasutajanimi ja parool esitate juurutamise käigus.  

Selle dokumendi eesmärk on selgitada viide arhitektuur ja eri osade ette valmistatud teie tellimus selle lahenduse malli osana. Dokumendi räägib ka tegelikke andmeid oma näeksid ülevaateid ja prognoose oma andmete põhjal Näidisandmete asendamiseks tehke järgmist. Lisaks dokumendi käsitletakse osad lahenduse malli, mida oleks vaja muuta, kui soovite kohandada oma andmetega lahendus. Lõpus on esitatud juhised, kuidas luua Power BI armatuurlaua selle lahenduse malli.

>[AZURE.TIP] Te saate alla laadida ning printida [selle dokumendi PDF-i versioon](http://download.microsoft.com/download/F/4/D/F4D7D208-D080-42ED-8813-6030D23329E9/cortana-analytics-technical-guide-predictive-maintenance.pdf).

## <a name="big-picture"></a>**Suur pilt**

![](media/cortana-analytics-technical-guide-predictive-maintenance\predictive-maintenance-architecture.png)

Lahendus juurutamisel erinevate Azure'i teenuste Cortana Analytics komplekti on aktiveeritud (*st* keskuses sündmuse *voo Analytics, Hdinsightiga, andmete Factory, seadme õ jne*). Ülaltoodud arhitektuur skeemi kuvatakse kõrge, kuidas sõnastikupõhise hooldustööd Aerospace lahenduse malli valmistatud lõpuni. Mida saab uurida nende teenuste Azure'i portaalis neile lahenduse malli skeemi loodud lahenduse, välja arvatud Hdinsightiga juurutamise ettevalmistamist see teenus on nõudmisel kui seotud müügivõimaluste tegevused on vajalik Käivita ja kustutatud hiljem klõpsates.
[Diagrammi täissuuruses versiooni](http://download.microsoft.com/download/1/9/B/19B815F0-D1B0-4F67-AED3-A40544225FD1/ca-topologies-maintenance-prediction.png)saate alla laadida.

Järgmistes jaotistes kirjeldatakse iga tekstilõigu.

## <a name="data-source-and-ingestion"></a>**Andmeallika ja manustamisest**

### <a name="synthetic-data-source"></a>Sünteetiline andmeallikas

Selle malli kasutada andmeallika luuakse töölauarakendus, et te alla laadida ja käivitada kohalikult pärast juurutamise. Siit leiate juhised Laadige alla ja installige see rakendus atribuudid ribal esimese sõlm nimega sõnastikupõhise hooldustööd andmete generaator lahenduse malli skeemi valimisel. Selle rakenduse kanalid [Azure'i sündmuse jaoturi](#azure-event-hub) teenuse andmepunkte või sündmusi, lahenduse voogu ülejäänud kasutatud. Selle andmeallika asuvaid või saadud avalikult kättesaadavaks andmete [NASA andmete talletuskoht](http://ti.arc.nasa.gov/tech/dash/pcoe/prognostic-data-repository/) abil [turboventilaatormootorite Engine degradeerumine simulatsioon andmehulgas](http://ti.arc.nasa.gov/tech/dash/pcoe/prognostic-data-repository/#turbofan).

Sündmuse loomine rakenduse asustada Azure'i sündmuse jaoturi ainult siis, kui see on teie arvutis käivitamist.

### <a name="azure-event-hub"></a>Azure'i sündmuse jaoturi

[Azure'i sündmuse jaoturi](https://azure.microsoft.com/services/event-hubs/) teenus on eespool kirjeldatud sünteetiliste andmeallika esitatud andmete adressaadile.

## <a name="data-preparation-and-analysis"></a>**Andmete ettevalmistamine ja analüüs**


### <a name="azure-stream-analytics"></a>Azure'i voo Analytics

[Azure'i voo Analytics](https://azure.microsoft.com/services/stream-analytics/) teenuse kasutatakse lähedal reaalajas analytics Sisestuskeel voogu [Azure'i sündmuse jaoturi](#azure-event-hub) teenuse ja avaldamise tulemuste [Power BI](https://powerbi.microsoft.com) armatuurlaua kui ka kõik töötlemata sissetulevate sündmusi [Azure'i](https://azure.microsoft.com/services/storage/) salvestusteenus hiljem töötlemiseks [Azure'i andmed Factory](https://azure.microsoft.com/documentation/services/data-factory/) teenus võttis arhiivimine.

### <a name="hd-insights-custom-aggregation"></a>HD ülevaateid kohandatud koondamine

Azure'i HD ülevaate teenuse kasutatakse skripte [taru](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) (juhitud Azure'i andmed Factory) esitada liitmised töötlemata sündmusi, mis olid arhiivitakse Azure'i voo Analytics teenuse kasutamise kohta.

### <a name="azure-machine-learning"></a>Azure'i masina õpetused

[Azure'i masina õ](https://azure.microsoft.com/services/machine-learning/) teenuse kasutamise (juhitud Azure'i andmed Factory) teha prognoose kindla neljasilindriline, mis on antud saadud sisendeid ülejäänud kasutusaja (RUL).

## <a name="data-publishing"></a>**Andmete avaldamine**


### <a name="azure-sql-database-service"></a>SQL Azure'i andmebaasi teenus

[Azure'i SQL-andmebaasi](https://azure.microsoft.com/services/sql-database/) teenuse abil saab salvestada (haldab Azure'i andmed Factory) saadud Azure seadme õ teenus, mis on tarbitud [Power BI](https://powerbi.microsoft.com) armatuurlaua prognoose.

## <a name="data-consumption"></a>**Andmete tarbimine**

### <a name="power-bi"></a>Power BI

[Power BI](https://powerbi.microsoft.com) teenust kasutatakse armatuurlaud, mis sisaldab liitmised ja [Azure voo Analytics](https://azure.microsoft.com/services/stream-analytics/) teenuse teatiste samuti RUL prognoose talletatud [Azure'i SQL-andmebaasi](https://azure.microsoft.com/services/sql-database/) koostatud [Azure seadme õ](https://azure.microsoft.com/services/machine-learning/) teenuse abil. Juhised, kuidas luua Power BI armatuurlaua see lahendus Mall, vaadake allpool olevat jaotist.

## <a name="how-to-bring-in-your-own-data"></a>**Kuidas oma andmeid tuua**

Selles jaotises kirjeldatakse, kuidas oma andmeid tuua Azure ja mis valdkondi nõuaks muudatusi saate tuua see arhitektuur andmed.

See on tõenäoline, et mis tahes andmekomplekti tuua vastaks andmekomplekti [turboventilaatormootorite Engine degradeerumine simulatsioon andmehulga](http://ti.arc.nasa.gov/tech/dash/pcoe/prognostic-data-repository/#turbofan) kasutatakse selle lahenduse malli kasutada. Andmete ja nõuetele on oluline, kuidas saate muuta selle malli oma andmetega töötamiseks. Kui see on teie esimene Azure seadme õ teenus, saate teenuseettevõtte tutvustus, kasutades näiteks [oma esimese katse loomise kohta](machine-learning-create-experiment.md).

Järgmistes jaotistes arutada Mall, mida on vaja muuta, kui on kasutusele uue andmehulga jaotised.

### <a name="azure-event-hub"></a>Azure'i sündmuse jaoturi

Azure'i sündmuse jaoturi teenus on väga üldine nii, et andmeid saab sisestada jaoturi CSV- või JSON-vormingus. Azure'i sündmuse keskuses ilmneb pole teisiti töötlemine, kuid on oluline, et mõistate juhitakse seda andmeid.

Selles dokumendis kirjeldatud, kuidas oma andmeid neelata, kuid saate hõlpsalt saata sündmusi või andmete Azure'i sündmuse jaoturiga API sündmuse jaoturi abil.

### <a name="azure-stream-analytics"></a>Azure'i voo Analytics

Azure'i voo Analytics teenuse kasutatakse lähedal reaalajas analytics andmevoogu lugemine ja kirjutamine suvalist arvu allikatest andmeid.

Ennustav hooldus Aerospace lahenduse malli, Azure'i voo Analytics päringu koosneb nelja alamtäpi päringud, iga nõudvate sündmuste Azure'i sündmuse jaoturi teenuse ja väljundid vajaduseta nelja erinevate asukohad. Need väljundid koosneb kolmest Power BI andmekomplektide ja Azure talletuskoht.

Leiate Azure'i voo Analytics päringu järgi:

-   Azure'i portaali sisse logimine

-   Voo analytics töö asukoha ![](media\cortana-analytics-technical-guide-predictive-maintenance\icon-stream-analytics.png) mis loodi lahenduse juurutamisel (*nt*, **maintenancesa02asapbi** ja **maintenancesa02asablob** sõnastikupõhise hooldustööd lahenduse)

-   Valimine

    -   Päringu sisendi kuvamiseks ***SISENDINA***

    -   ***PÄRINGU*** ise päringu vaatamiseks

    -   ***Tulemused*** erinevad väljundid kuvamiseks

[Voo Analytics päringu viide](https://msdn.microsoft.com/library/azure/dn834998.aspx) MSDN-is leiate Azure'i voo Analytics päringu ehituse teavet.

See lahendus päringute väljund kolme andmekomplektide koos lähedal reaalajas analytics sissetulevate andmevoos Power BI armatuurlaud, mis on selle lahenduse malli osana esitatud teavet. Kuna seal on peidetud teadmisi sissetulevate andmete vormindamine, need päringud oleks vaja muuta oma andmete vormingu põhjal.

Teine voo analytics töö **maintenancesa02asablob** päringu lihtsalt väljundid kõik [Sündmuse jaoturi](https://azure.microsoft.com/services/event-hubs/) sündmusi [Azure Storage](https://azure.microsoft.com/services/storage/) ja seega nõuab sõltumata teie andmete vormingu muutmise, nagu on täielik sündmuse teabe voona salvestusruumi.

### <a name="azure-data-factory"></a>Azure'i andmed Factory

[Azure'i andmed Factory](https://azure.microsoft.com/documentation/services/data-factory/) teenuse orchestrates liikumine ja andmete töötlemiseks. Ennustav hooldus Aerospace lahenduse malli tehakse andmete factory kolme [torujuhtmete](../data-factory/data-factory-create-pipelines.md) teisaldamine ja töödelda erinevate tehnoloogiate andmeid.  Pääsete oma andmete factory, avades selle andmete Factory sõlm lahenduse malli skeemi allosas loodud lahenduse juurutamise. See viib teid andmete factory oma Azure'i portaalis. Kui näete oma andmekomplektide jaotises tõrkeid, saate need, kui need on tingitud andmete factory kasutusele enne andmete generaator käivitati ignoreerida. Need vead ei takista oma andmete factory toimimist.

![](media/cortana-analytics-technical-guide-predictive-maintenance/data-factory-dataset-error.png)

Selles jaotises käsitletakse vajalikud [torujuhtmete](../data-factory/data-factory-create-pipelines.md) ja [tegevuste](../data-factory/data-factory-create-pipelines.md) [Azure'i andmed Factory](https://azure.microsoft.com/documentation/services/data-factory/). Allpool on diagrammivaate lahenduse.

![](media/cortana-analytics-technical-guide-predictive-maintenance/azure-data-factory.png)

Kaks selle factory torujuhtmete sisaldavad [taru](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) skriptide kasutatakse partition ja andmeid koondada. Kui märkida skriptide asub [Azure Storage](https://azure.microsoft.com/services/storage/) konto häälestamise ajal loodud. Nende asukoht: maintenancesascript\\\\skripti\\\\taru\\ \\ (või https://[Your lahenduse name].blob.core.windows.net/maintenancesascript).

Sarnaselt [Azure'i voo Analytics](#azure-stream-analytics-1) päringud, [taru](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) skriptide on peidetud teadmisi sissetulevate andmete vormindamine, need päringud oleks vaja muuta andmete vormindamine ja [funktsioon matemaatika](machine-learning-feature-selection-and-engineering.md) nõuete alusel.

#### <a name="aggregateflightinfopipeline"></a>*AggregateFlightInfoPipeline*

Selle [müügivõimaluste](../data-factory/data-factory-create-pipelines.md) sisaldab ühe tegevuse - [HDInsightHive](../data-factory/data-factory-hive-activity.md) tegevuse abil [HDInsightLinkedService](https://msdn.microsoft.com/library/azure/dn893526.aspx) käivituva [taru](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) skripti partition [Azure](https://azure.microsoft.com/services/storage/) Storage andmete [Azure'i voo Analytics](https://azure.microsoft.com/services/stream-analytics/) töö käigus.

Selle eraldamine ülesande jaoks [taru](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) script on ***AggregateFlightInfo.hql***

#### <a name="mlscoringpipeline"></a>*MLScoringPipeline*

Selle [müügivõimaluste](../data-factory/data-factory-create-pipelines.md) sisaldab mitut tegevust ja mille tulemus on poolitusjoonega prognoose [Azure seadme õ](https://azure.microsoft.com/services/machine-learning/) katsetamiseks seotud lahendus selle malli abil.

Selles sisalduvad tegevused on:

-   [HDInsightHive](../data-factory/data-factory-hive-activity.md) tegevuse abil on [HDInsightLinkedService](https://msdn.microsoft.com/library/azure/dn893526.aspx) käivituva [taru](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) skripti liitmised ja funktsioon matemaatika vajalik [Azure seadme õ](https://azure.microsoft.com/services/machine-learning/) katse sooritamiseks.
    Selle eraldamine ülesande jaoks [taru](http://blogs.msdn.com/b/bigdatasupport/archive/2013/11/11/get-started-with-hive-on-hdinsight.aspx) script on ***PrepareMLInput.hql***.

-   [Kopeeri](https://msdn.microsoft.com/library/azure/dn835035.aspx) tegevus, mis viib tulemused [HDInsightHive](../data-factory/data-factory-hive-activity.md) tegevuse üle ühe [Azure Storage](https://azure.microsoft.com/services/storage/) bloobimälu, mida saab Accessi [AzureMLBatchScoring](https://msdn.microsoft.com/library/azure/dn894009.aspx) tegevuse alusel.

-   [AzureMLBatchScoring](https://msdn.microsoft.com/library/azure/dn894009.aspx) tegevust, mis nõuab [Azure seadme õ](https://azure.microsoft.com/services/machine-learning/) proovida, mille tulemuseks ühe [Salvestusruumi Azure'i](https://azure.microsoft.com/services/storage/) bloobimälu pannakse tulemused.

#### <a name="copyscoredresultpipeline"></a>*CopyScoredResultPipeline*

[Müügivõimaluste](../data-factory/data-factory-create-pipelines.md) see sisaldab ühe tegevuse - [Kopeeri](https://msdn.microsoft.com/library/azure/dn835035.aspx) tegevuse, mis viib [Azure seadme õ](#azure-machine-learning) tulemuste katsetamiseks ***MLScoringPipeline*** [Azure SQL-andmebaasiga](https://azure.microsoft.com/services/sql-database/) , kus on ette valmistatud lahenduse malli installimise käigus.

### <a name="azure-machine-learning"></a>Azure'i masina õpetused

Selle lahenduse malli kasutada [Azure seadme õ](https://azure.microsoft.com/services/machine-learning/) katse pakub selle ülejäänud kasulik kestus (RUL) õhusõiduki mootori. Katse on seotud tarbitud andmehulga ja seetõttu on vaja muuta või asendada konkreetsed andmed, mida on toodud.

Azure'i masina õ katse loomise viisist kohta leiate teavet teemast [ennustav hooldus: samm 1 / 3, andmete ettevalmistamine ja funktsioon matemaatika](http://gallery.cortanaanalytics.com/Experiment/Predictive-Maintenance-Step-1-of-3-data-preparation-and-feature-engineering-2).

## <a name="monitor-progress"></a>**Edenemise jälgimine**
 Pärast andmete generaator käivitatakse, tulemas hakkab saada hüdreeritud ja teie lahendus erinevate osade alustada lööd jagada käske välja andmete Factory toimingu järgmist. On kaks võimalust, et saate jälgida tulemas.

1. Üks voo Analytics töö kirjutab sissetulevate toorandmetega bloobimälu. Kui klõpsate bloobimälu osa teie lahendus kuval saate edukalt juurutatud lahendus ja seejärel klõpsake nuppu Ava Excelis paremal, kulub teil [haldusportaali](https://portal.azure.com/). Kui seal, klõpsake plekid. Järgmise paanil kuvatakse ümbriste loendit. Klõpsake **maintenancesadata**. Järgmise paneeli, kuvatakse **rawdata** kausta. Kaustas rawdata kuvatakse kaustad nimedega nagu tund = 17, tund = 18 jne. Kui te ei näe neid kaustu, see näitab, et töötlemata andmete on edukalt on teie arvutis loodud ja talletatud bloobimälu. Peaksite nägema csv-failid, mis peaks olema piiratud suurused MB need kaustad.

2. Viimases etapis tulemas on kirjutada andmeid (nt prognoose õ seadme kaudu) SQL-andmebaasi. Peate ootama kuni kolm tunni jaoks andmeid kuvada SQL-andmebaasis. Üks võimalus jälgida, kui palju andmeid on saadaval SQL-andmebaasis on [azure portaali](https://manage.windowsazure.com/)kaudu. Leidke vasakul SQL ANDMEBAASE ![](media\cortana-analytics-technical-guide-predictive-maintenance\icon-SQL-databases.png) ja klõpsake seda. Seejärel leidke oma andmebaasi **pmaintenancedb** ja klõpsake seda. Järgmisel lehel allosas nuppu haldamine

    ![](media\cortana-analytics-technical-guide-predictive-maintenance\icon-manage.png).

    Siin saate klõpsata uue päringu ja päringu jaoks (nt select count(*) PMResult) ridade arv. Andmebaasi kasvab, tabeli ridade arvu, mis peaks suurendama.


## <a name="power-bi-dashboard"></a>**Power BI armatuurlaua**

### <a name="overview"></a>Ülevaade

Selles jaotises kirjeldatakse paketi ennustamine tulemusi Azure seadme õppeteema (külma) ning kuidas luua Power BI armatuurlaua Azure voo Analytics (kuum tee), reaalajas andmete visualiseerimiseks.

### <a name="setup-cold-path-dashboard"></a>Häälestamise külma tee armatuurlaud

Ettevalmistamisel külma tee andmete oluline eesmärk on saada iga neljasilindriline sõnastikupõhise RUL (ülejäänud eluiga), kui ta lõpetab on lendude (tsükkel). Ennustamine tulemi värskendatakse iga 3 tunni selle mis lõpetamist on lendude viimase 3 tunni jooksul mootorite prognoosimiseks.

Power BI loob Azure SQL-andmebaasi selle ennustamine tulemite talletamiseks andmeallikana. Märkus: 1) korral oma lahenduse juurutamine reaal ennustamine kuvatakse andmebaasis 3 tunni jooksul.
Generaator allalaadimine kaasas pbix fail sisaldab seemne andmeid nii, et võite luua Power BI armatuurlaua kohe. (2) selles juhises on nõutav alla ja installige tasuta tarkvara [Power BI Desktopi](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/).

Järgmised juhised aitavad teil ühendamise pbix faili SQL-andmebaasiga, mis on kedratud üles ajal lahenduse juurutamine, mis sisaldab andmeid (*nt*. ennustamine tulemused) visualiseerimiseks.

1.  Andmebaasi mandaadi saamiseks.

    Peate enne järgmist **Andmebaasiserveri nimi, andmebaasi nimi, kasutajanimi ja parool** . Järgnevalt kirjeldatakse, kuidas neid leida suunata.

    -   Kui teie lahendus malli skeemi **"Azure'i SQL-andmebaasi"** roheliseks, klõpsake seda ja seejärel nuppu **"Ava"**.

    -   Kuvatakse uus menüü/brauseriaken, mis kuvab Azure'i portaal. Klõpsake vasakpoolsel paanil **"Ressursi rühmad"** .

    -   Valige tellimus, mida kasutate lahenduse juurutamine ja seejärel valige **' YourSolutionName\_ResourceGroup'**.

    -   Uus pop välja paani, klõpsake selle ![](media\cortana-analytics-technical-guide-predictive-maintenance\icon-sql.png) ikooni teie andmebaasile juurde pääsemiseks. Teie andmebaasi nimi on see kõrval ikooni (*nt*, **"pmaintenancedb"**) ja **Andmebaasiserveri nimi** on loetletud atribuudi serveri nimi ja peaks sarnanema **YourSoutionName.database.windows.net**.

    -   Teie andmebaasi **kasutajanimi** ja **parool** on sama kasutajanime ja parooli varem salvestatud ajal lahenduse juurutamise.

2.  Power BI Desktopi andmeallika külma tee fail värskendada.

    -   Topeltklõpsake oma arvutis, kus alla laadida ja lahti pakitud generaator faili kaustas soovitud **PowerBI\\PredictiveMaintenanceAerospace.pbix** faili. Kui faili avamisel kuvatakse kõik hoiatusteated, jätke need. Klõpsake faili ülaosas **Redigeerimine päringute**.

        ![](media\cortana-analytics-technical-guide-predictive-maintenance\edit-queries.png)

    -   Kuvatakse kaks tabelit, **RemainingUsefulLife** ja **PMResult**. Valige esimene tabel ja klõpsake nuppu ![](media\cortana-analytics-technical-guide-predictive-maintenance\icon-query-settings.png) kõrval **'Allika'** jaotises **Rakendatud etapid** **"päringu sätted"** armatuurlaua paremal. Ignoreerida kõiki hoiatus sõnumeid, mis kuvatakse.

    -   Välja aknas pop, **"Server"** ja **"Andmebaas"** asendamine oma server ja andmebaas nimed ja klõpsake nuppu **"OK"**. Serveri nimi, veenduge, et teie määratud pordi 1433 (**YourSoutionName.database.windows.net 1433**). Jätke väli Database **pmaintenancedb**. Ignoreeri hoiatusteated, mis kuvatakse ekraanil.

    -   Järgmise pop välja aknas, kuvatakse teile kaks suvandid vasakpoolsel paanil (**Windowsi** ja **andmebaasi**). Klõpsake nuppu **"Andmebaas"**, sisestage oma **"Kasutajanimi"** ja **"Parooli"** (see on kasutajanimi ja parool sisestatud kui esmalt juurutatud lahendus ja Azure SQL-andmebaasi loodud). ***Nende sätete rakendamiseks valida, milliseid***, märkige taseme suvandit andmebaasi. Klõpsake nuppu **"Loo"**.

    -   Teise tabeli **PMResult** klõpsake käsku ![](media\cortana-analytics-technical-guide-predictive-maintenance\icon-navigation.png) kõrval **'Allika'** jaotises **Rakendatud etapid** õige **' päringu sätted '** paani ja update server ja andmebaas nimed nagu ülaltoodud juhiseid ja klõpsake siis nuppu OK.

    -   Kui te kasutate juhendamisega tagasi eelmisele lehele, sulgege aken. Sõnumi pop välja – klõpsake nuppu **Rakenda**. Lõpuks klõpsake muudatuste salvestamiseks nuppu **Salvesta** . Power BI faili nüüd on loodud ühendus serveriga. Kui teie visualiseeringud on tühi, veenduge, et tühjendate Valikud visualiseeringute visualiseerida kõik andmed, klõpsates nuppu Kustutuskumm legendid paremas ülanurgas ikooni. Nupp Värskenda kasutada uute andmete arutleda visualiseering. Esialgu, siis kuvatakse ainult seemne andmete visualiseeringutes nagu andmete factory on ajastatud värskendamine iga 3 tunni. 3 tunni pärast, kuvatakse uus prognoose kajastuksid oma visualiseeringuid, kui andmete värskendamine.

3.  (Valikuline) [Power BI võrgus](http://www.powerbi.com/)külma tee armatuurlaua avaldada. Pange tähele, et seda toimingut ka Power BI konto (või Office 365 konto).

    -   Klõpsake nuppu **"Avalda"** ja mõne sekundi hiljem aknas kuvatakse "Avaldamine Power BI edu!" koos roheline märge. Klõpsake linki "Ava PredictiveMaintenanceAerospace.pbix rakenduses Power BI" all. Üksikasjalikud juhised leiate teemast [avaldamine Power BI Desktopi kaudu](https://support.powerbi.com/knowledgebase/articles/461278-publish-from-power-bi-desktop).

    -   Uue armatuurlaua loomine: klõpsake soovitud **+** Logi vasakul paanil jaotises **armatuurlaudade** kõrval. Sisestage nimi "Ennustav hooldus Demo" selle armatuurlaud.

    -   Kui avate aruande, klõpsake ![](media\cortana-analytics-technical-guide-predictive-maintenance\icon-pin.png) kõikide visualiseeringute armatuurlauale kinnitamine. Üksikasjalikud juhised leiate teemast [PIN-koodi paan Power BI armatuurlaua aruandest](https://support.powerbi.com/knowledgebase/articles/430323-pin-a-tile-to-a-power-bi-dashboard-from-a-report).
    Minge lehele armatuurlaua ja suuruse ja asukoha oma visualiseeringuid ja nende redigeerimine. Kuidas muuta paanide üksikasjalikud juhised leiate teemast [Redigeeri paani--suurust, teisaldamine, ümbernimetamine ja PIN-koodi, kustutada, lisage hüperlink](https://powerbi.microsoft.com/documentation/powerbi-service-edit-a-tile-in-a-dashboard/#rename). Siin on mõni näide armatuurlaud, kus mõned külma tee visualiseeringute kinnitatud.  Sõltuvalt sellest, kui kaua käivitate oma andmete generaator, võivad olla erinevad oma arvude visualiseering.
    <br/>
    ![](media\cortana-analytics-technical-guide-predictive-maintenance\final-view.png)
<br/>
    -   Andmete värskendamise ajakava, et Hõljutage kursorit **PredictiveMaintenanceAerospace** andmekomplekti, klõpsake ![](media\cortana-analytics-technical-guide-predictive-maintenance\icon-elipsis.png) ja klõpsake nuppu **Värskenda ajakava**.
<br/>
        **Märkus:** Kui kuvatakse hoiatus Massaaž, klõpsake nuppu **Redigeeri identimisteavet** ja veenduge, et teie andmebaasi mandaat on samad, mis on kirjeldatud juhises 1.
<br/>
    ![](media\cortana-analytics-technical-guide-predictive-maintenance\schedule-refresh.png)
<br/>
    -   Laiendage **Värskenda ajakava** . Lülitage sisse "hoida oma andmed ajakohane".
    <br/>
    -   Vastavalt oma vajadustele Värskenda ajakava. Lisateavet leiate teemast [andmete värskendamine Power BI](https://support.powerbi.com/knowledgebase/articles/474669-data-refresh-in-power-bi).

### <a name="setup-hot-path-dashboard"></a>Häälestamise kuum tee armatuurlaud

Järgmised toimingud juhendab teid visualiseerimine reaalajas andmed väljund voo Analytics tööd, mis loodi lahenduse juurutamine ajal tehke järgmist. [Power BI võrgus](http://www.powerbi.com/) konto on nõutav tehke järgmist. Kui teil pole kontot, saate [luua](https://powerbi.microsoft.com/pricing).

1.  Lisage Power BI väljundi Azure'i voo Analytics (Ash).

    -  Peate järgige juhiseid teemas [Azure voo analüüsimine ja Power BI: reaalajas nähtavuse streaming andmete reaalajas analytics armatuurlaua](stream-analytics-power-bi-dashboard.md) Power BI armatuurlaua Azure'i voo Analytics töö väljund häälestamiseks.
    - ASA päringus on kolm väljundeid, mis on **aircraftmonitor**, **aircraftalert**ja **flightsbyhour**. Saate vaadata, klõpsates menüü päring nuppu päring. Vastab iga need tabelid, peate ASA väljund lisamiseks. Kui lisate esimene väljund (*nt* **aircraftmonitor**) veenduge, et **Väljund pseudonüümi**, **Andmekomplekti nimi** ja **Tabeli nimi** on sama (**aircraftmonitor**). Korrake juhiseid väljundid **aircraftalert**ja **flightsbyhour**lisamiseks. Kui olete lisanud kõik kolm väljund tabelid ja ASA töö alustamine, saate kinnitusteade (*nt*, "Alustades voo analytics töö maintenancesa02asapbi õnnestus").

2. Logige sisse [Power BI võrgus](http://www.powerbi.com)

    -   Klõpsake vasakul paanil andmekomplektide jaotise minu tööruumis ***ANDMEKOMPLEKTI*** nimed **aircraftmonitor**, **aircraftalert**ja **flightsbyhour** peaks kuvatama. See on voogesituse andmete olete lükanud Azure'i voo Analytics eelmises etapis. Andmekomplekti **flightsbyhour** võib ei ilmu on kaks andmekomplektide SQL-päringu taga laadist samaaegselt. Siiski peaks näitama tunni pärast.
    -   Veenduge, et paani ***visualiseeringud*** on avatud ja kuvatakse ekraani paremas servas.

3. Kui olete soovitud andmed, mis on Power BI liiguvad, saate alustada voogesituse andmete visualiseerimiseks. Allpool on mõned kuum tee visualiseeringutega näidisarmatuurlaual kinnitatakse selle. Saate luua muud armatuurlaua paanid vastav andmekomplektide põhjal. Sõltuvalt sellest, kui kaua käivitate oma andmete generaator, võivad olla erinevad oma arvude visualiseering.


    ![](media\cortana-analytics-technical-guide-predictive-maintenance\dashboard-view.png)

4. Siin on mõned juhised loomiseks tehke ülaltoodud paanid – selle "laevastiku vaade, Sensor 11 vs lävi 48.26" paani:

    -   Klõpsake vasakpoolsel paanil andmekomplektide jaotise andmekomplekti **aircraftmonitor** .

    -   Klõpsake ikooni **Joondiagramm** .

    -   Klõpsake paanil **väljad** **töödeldav** nii, et see kuvatakse jaotises "Telje" Paani **visualiseeringud** .

    -   Klõpsake nuppu "s11" ja "s11\_teade" nii, et nad mõlemad kuvatakse jaotises "Väärtused". Klõpsake **s11** kõrval asuvat väikest noolt ja **s11\_teatise**, muuta "Summa" "Keskmine".

    -   Klõpsake nuppu **Salvesta** ülaosas ja nime aruanne "aircraftmonitor". Aruande nimega "aircraftmonitor" kuvatakse vasakul paanil **navigaator** jaotist **aruanded** .

    -   Klõpsake ikooni **Kinnita Visual** see joondiagramm paremas ülanurgas. "PIN-koodi armatuurlaud" akna võib ilmu saab valida armatuurlaua. Valige "Ennustav hooldus Demo", seejärel valige "Kinnita".

    -   Hõljutage kursorit üle selle paani armatuurlaual, muuta selle pealkirja "Laevastiku vaade, Sensor 11 vs lävi 48.26" ja "Keskmine kogu pargi aja jooksul" alapealkiri paremas ülanurgas ikooni "Muuda".

## <a name="how-to-delete-your-solution"></a>**Kuidas kustutada teie lahendus**
Veenduge, et lõpetate andmete generaator kui aktiivselt ei kasutamine andmete generaator töötab lahendus on kulud suuremad. Kustutage lahenduse, kui te ei kasuta seda. Teie lahendus kustutamisel kustutatakse kõik komponendid ette valmistatud lahenduse juurutamisel teie tellimus. Kui soovite kustutada lahenduse klõpsake vasakul paanil lahenduse malli oma lahenduse nime ja klõpsake käsku Kustuta.

## <a name="cost-estimation-tools"></a>**Maksumuse hindamine tööriistad**

Järgmised kaks tööriistad on saadaval, et aidata teil paremini mõista kulude töötab sõnastikupõhise hooldustööd Aerospace lahenduse malli teie tellimus.

-   [Microsoft Azure'i kulude prognoos tööriist (võrgus)](https://azure.microsoft.com/pricing/calculator/)

-   [Microsoft Azure'i kulude prognoos tööriist (töölauaversioon)](http://www.microsoft.com/download/details.aspx?id=43376)
