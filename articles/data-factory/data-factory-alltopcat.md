<properties
    pageTitle="Kõigi teemade andmete Factory teenuse | Microsoft Azure'i"
    description="Tabeli kõigi teemade Azure'i teenuse nimega andmed Factory, mis on olemas http://azure.microsoft.com/documentation/articles/, pealkiri ja kirjeldus."
    services="data-factory"
    documentationCenter=""
    authors="spelluru"
    manager="jhubbard"
    editor="MightyPen"/>

<tags
    ms.service="data-factory"
    ms.workload="data-factory"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="spelluru"/>


# <a name="all-topics-for-azure-data-factory-service"></a>Kõigi teemade Azure'i andmed Factory teenuse jaoks

Selles teemas on loetletud iga teema, mis kehtib **Andmete Factory** teenuse Azure. Saate otsida selle veebilehe märksõnade abil **Klahvikombinatsiooni Ctrl + F**, praegune huvi pakkuvate teemade otsimiseks.




## <a name="new"></a>Uue

| &nbsp; | Pealkiri | Kirjeldus |
| --: | :-- | :-- |
| 1 | [Andmete teisaldamine alla Amazon punanihe Azure'i andmed Factory abil](data-factory-amazon-redshift-connector.md) | Lugege selle kohta, kuidas andmeid Amazon punanihe Azure'i andmed Factory abil teisaldada. |
| 2 | [Andmete teisaldamine alla Amazon lihtsa salvestusteenus Azure'i andmed Factory abil](data-factory-amazon-simple-storage-service-connector.md) | Lisateavet andmete Amazon Simple Storage Service (S3) abil Azure'i andmed Factory liikuda. |
| 3 | [Azure'i andmed Factory - kopeerimise viisard](data-factory-azure-copy-wizard.md) | Lisateavet toetatud andmeallikate andmete kopeerimine valamud andmete Factory Azure koopia viisardi abil. |
| 4 | [Õpetus: Koostada oma esimese Azure'i andmed factory andmete Factory REST API abil](data-factory-build-your-first-pipeline-using-rest-api.md) | Selles õpetuses loote valimi Azure'i andmed Factory müügivõimaluste abil andmete Factory REST API-ga. |
| 5 | [Õpetus: Luua müügivõimaluste Kopeeri tegevuse API .net-i abil](data-factory-copy-activity-tutorial-using-dotnet-api.md) | Selles õpetuses loote mõnda Azure'i andmed Factory müügivõimaluste Kopeeri tegevuse .NET API abil. |
| 6 | [Õpetus: Luua müügivõimaluste Kopeeri tegevuse REST API abil](data-factory-copy-activity-tutorial-using-rest-api.md) | Selles õpetuses loote mõnda Azure'i andmed Factory müügivõimaluste Kopeeri tegevuse REST API abil. |
| 7 | [Andmete Factory kopeerimise viisard](data-factory-copy-wizard.md) | Lisateavet andmete Factory koopia viisardi abil saate andmeid kopeerida valamud toetatud andmeallikad. |
| 8 | [Andmehalduslüüs](data-factory-data-management-gateway.md) | Häälestada andmete lüüsi andmete kohapealse ja pilveteenuse vahel liikumiseks. Azure'i andmed Factory Andmehalduslüüsi abil andmete teisaldamine. |
| 9 | [Andmete teisaldamine kohapealse Cassandra andmebaasist Azure'i andmed Factory abil](data-factory-onprem-cassandra-connector.md) | Lisateavet asutusesisese Cassandra andmebaasist Azure'i andmed Factory abil andmete teisaldamiseks. |
| 10 | [Andmete teisaldamine kaudu MongoDB Azure'i andmed Factory abil](data-factory-on-premises-mongodb-connector.md) | Lisateavet andmete teisaldamiseks abil Azure'i andmed Factory MongoDB andmebaasist. |
| 11 | [Teisaldage andmed Salesforce'i Azure'i andmed Factory abil](data-factory-salesforce-connector.md) | Lisateavet andmete teisaldamiseks Salesforce'i Azure'i andmed Factory abil. |


## <a name="updated-articles-data-factory"></a>Värskendatud artikleid, andmete Factory

Selles jaotises on loetletud tooted, mis olid värskendanud, kui värskendus on suur või märgatavalt. Iga värskendatud artiklis töötlemata koodilõigu lisatud allahindlusest teksti kuvada. Artikleid värskendati **2016-10-05** **2016 – 08-22** vahemikus kuupäeva.

| &nbsp; | Artikkel | Värskendatud teksti, koodilõigu | Millal värskendatud |
| --: | :-- | :-- | :-- |
| 12 | [Azure'i andmed Factory - .NET API muutmine Logi](data-factory-api-change-log.md) | Sellest artiklist leiate teavet muudatuste kohta Azure'i andmed Factory SDK abil konkreetsete versiooni. Viimane Nugeti pakett leiate Azure'i andmed Factory siin (https://www.nuget.org/packages/Microsoft.Azure.Management.DataFactories) **versiooni 4.11.0** funktsiooni täiendusi: / järgmist tüüpi lingitud teenus on lisatud: AwsAccessKeyLinkedService (https://msdn.microsoft.com/library/mt765144.aspx) – OnPremisesMongoDbLinkedService (https://msdn.microsoft.com/library/mt765129.aspx) – AmazonRedshiftLinkedService (https://msdn.microsoft.com/library/mt765121.aspx) - / andmekomplekti järgmist tüüpi on lisatud: - MongoDbCollectionDataset (https://msdn.microsoft.com/library/mt765145.aspx) – AmazonS3Dataset (https://msdn.microsoft.com/library/mt765112.aspx) / Kopeeri allikas järgmist tüüpi on lisatud :-MongoDbSource (https://msdn.microsoft.com/en-US/library/mt765123.aspx) **versioon 4.10.0** / valikuline järgmised atribuudid on lisatud tekstivorming:-Ski | 2016-09-22 |
| 13 | [Andmete teisaldamine ja sealt abil Azure'i andmed Factory Azure'i bloobimälu](data-factory-azure-blob-connector.md) |  / copyBehavior / määratleb Kopeeri käitumine, kui allikas on BlobSource või failisüsteemi.  / **PreserveHierarchy:** säilitab faili hierarhia sihtkaust. Suhteline lähtefail allikas kausta tee on identne suhteline tee sihtkaust sihtfail. br /. br /. **FlattenHierarchy:** kõik failid kaustast allikas on esimese taseme sihtkaust. Target (sihtkoht) failid on automaatselt loodud nimi. .br /. br /. **MergeFiles: (vaikeväärtus)** liidab kõik failid kaustast andmeallika ühe faili. Kui faili/bloobimälu nimi pole määratud, ühendatud faili nimi oleks määratud nimi; muul juhul oleks automaatselt loodud faili nimi.  / Ei / **BlobSource** ka toetab kahte atribuutidest tagasiühilduvuse tagamiseks. / **treatEmptyAsNull**: määrab, kas null või tühi käsitleda tühiväärtus. / **skipHeaderLineCount** – saate määrata, kui palju ridu vaja vahele. See on rakendatav ainult kui Sisestuskeel andmekomplekti kasutab tekstivorming. Samuti **BlobSink** toetab Nes | 2016-09-28 |
| 14 | [Looge sõnastikupõhise torujuhtmete Azure seadme õppimise abil](data-factory-azure-ml-batch-execution-activity.md) | **Veebiteenuse jaoks on vaja mitut sisendit** Kui veebiteenuse kulub mitme sisendina, kasutada atribuuti **webServiceInputs** **webServiceInput**kasutamise asemel. Andmekomplektide, millele viidatakse **webServiceInputs** peab ka tegevuse **sisendina**. Oma Azure'i ML katse, on web teenuse sisendi ja väljundi pordid ja globaalse parameetrite vaikenimed ("input1", "input2"), mida saate kohandada. Nimed webServiceInputs, webServiceOutputs ja globalParameters sätete kasutamiseks peab täpselt vastama katsete nimed. Valimi taotluse last saate vaadata oma Azure'i ML lõpp-punkti kinnitamiseks oodatud vastenduse lehel paketi täitmise spikker.    {"nimi": "PredictivePipeline", "atribuudid": {"kirjeldus": "AzureML mudeli kasutamine", "tegevused": {"nimi": "MLActivity", "tüüp": "AzureMLBatchExecution", "kirjeldus": "ennustamine analüüs sisestada töölehe", "sisendit": {"nimi": "inputDataset1"}, {"nimi": "inputDatase | 2016-09-13 |
| 15 | [Kopeerige tegevuse jõudlus ja häälestamise juhend](data-factory-copy-activity-performance.md) | 1. **looma võrdlusalus**. Ajal, Testige oma müügivõimaluste Kopeeri tegevuse abil andmeid valimi suhtes. Saate piirata andmehulga töötate andmete Factory, viilutamine mudeli (andmete-factory-plaanimine-ja-execution.md time-series-datasets-and-data-slices).  Täitmisaeg ja parameetritele, kasutades **jälgimine ja haldamine App**koguda. Valige avalehel andmete Factory **jälgimine ja haldamine** . Valige puuvaates **väljundi andmekomplekti**. Valige loendis **Tegevuse Windows** Kopeeri tegevuse käivitada. **Tegevuste Windows** on loetletud Kopeeri tegevuse kestuse ja kopeeritud andmete suurust. Funktsiooni läbilaskevõime on loetletud **tegevuste aknas**Exploreris. Rakenduse kohta leiate lisateavet teemast jälgimine ja haldamine Azure'i andmed Factory torujuhtmete jälgimine ja haldamine App (andmete-factory-kuvari-haldamine – app.md) abil.  ! Tegevuste käivitamine (./media/data-factory-copy-activity-performance/mmapp-activity-run-details.pn üksikasjad | 2016-09-27 |
| 16 | [Õpetus: Luua müügivõimaluste Kopeeri tegevuse Visual Studio abil](data-factory-copy-activity-tutorial-using-visual-studio.md) |   Pange tähele järgmist:-andmekomplekti **Tüüp** väärtuseks **AzureBlob**.     - **linkedServiceName** on seatud **AzureStorageLinkedService**. Samm 2 loodud lingitud teenus.     - **folderPath** on seatud **adftutorial** ümbrises. Saate määrata ka mõne bloobimälu abil atribuudi **nimi** kausta nimi. Kuna siis määrate selle bloobimälu nime, peetakse mõne sisendandmete kõik plekid ümbrises andmeid.    -vormingus **Tüüp** väärtuseks **tekstivorming** – on kaks väljad soovitud teksti faili ΓÇô **eesnimi** ja **perekonnanimi** ΓÇô eraldatud koma (**columnDelimiter**) – **kättesaadavus** väärtuseks **tunni** (**sagedus** väärtuseks **tund** ja **intervall** väärtuseks **1**). Seega andmete Factory otsitakse sisendandmete iga tund bloobimälu container (**adftutorial**) määratud juurkaustas.  kui te ei määra **faili nimi** soovitud **Sisestuskeel** andmekomplekti, kõik failid plekid Sisestuskeel kaustast (**folderPath**) on consid | 2016-09-29 |
| 17 | [Luua, jälgimine ja haldamine Azure'i andmed tehased andmete Factory .NET SDK abil](data-factory-create-data-factories-programmatically.md) | Pange kirja rakenduse ID ja parooliga (klient salajane) ja kasutada seda funktsiooni ülevaate. **Saada Azure'i tellimust ja rentniku ID-d** Kui teil pole teie arvutisse installitud PowerShelli uusim versioon, järgige juhiseid, kuidas installida ja konfigureerida Azure PowerShell (… / PowerShelli-installi-configure.md) artiklis selle installima. 1. Käivitage PowerShelli Azure ja käivitage järgmine käsk 2. Käivitage järgmine käsk ja sisestage kasutajanimi ja parool, mida kasutate Azure portaali sisse logida.         Logi sisse – AzureRmAccount, kui teil on ainult üks Azure'i tellimus, mis on seotud konto, peate tegema järgmised kaks toimingud. 3. Käivitage järgmine käsk, et kuvada kõik tellimused selle konto jaoks.       Get-AzureRmSubscription 4. Käivitage järgmine käsk valige tellimus, mida soovite töötada. Asendage **NameOfAzureSubscription** Azure tellimuse nime.       Get-AzureRmSubscription - SubscriptionName NameOfAzureSubscription / Set-AzureRmCo | 2016-09-14 |
| 18 | [Torujuhtmed ja tegevusi Azure'i andmed Factory](data-factory-create-pipelines.md) |       "start": "2016-07-12T00:00:00Z", "end": "2016-07-13T00:00:00Z"}} meeles pidada järgmist: / jaotises tegevused on ainult üks tegevust, mille **Tüüp** on seatud **Kopeeri**. / Sisendit tegevuse jaoks on määratud **InputDataset** ja väljundi tegevuse jaoks on määratud **OutputDataset**. / Jaotises **typeProperties** **BlobSource** on määratud Reaallika tüüp ja **SqlSink** on määratud valamu tüüp. Vaadake täieliku lühiülevaade selle müügivõimaluste loomise õpetus: bloobimälu andmete kopeerimine SQL-andmebaasi (data-factory-copy-data-from-azure-blob-storage-to-sql-database.md). **Müügivõimaluste valimi teisendus** Järgmine näidis ettevalmistamisel on üks tegevuse tüüp **tegevuste** **HDInsightHive** . Selles näites on Hdinsightiga taru tegevuse (andmete-factory-taru-activity.md) muudab andmed on Azure'i bloobimälu käivitades mõne Azure Hdinsightiga Hadoopi kobar taru skripti faili.  {"nimi": "TransformPipeline", "p | 2016-09-27 |
| 19 | [Muuta andmete Azure'i andmed Factory](data-factory-data-transformation-activities.md) | Andmete Factory toetab järgmisi andmete teisendus tegevusi, mida saab lisada välistrasside (andmete-factory loomine – pipelines.md) eraldi või muu tegevuse abil ühendatud. .  AZURE'I. Märkus lühiülevaade üksikasjalikud juhised leiate jaotisest müügivõimaluste taru transformatsioon (data-factory-build-your-first-pipeline.md) artikli. **Hdinsightiga taru tegevus** Andmete Factory teel Hdinsightiga taru tegevuse aktiveeritakse taru päringute ise või nõudmisel Windowsi/Linuxi-põhiste Hdinsightiga kobar. Artiklist taru tegevuse (andmete-factory-taru-activity.md) selle tegevuse üksikasjad. **HDInsight siga tegevus** Andmete Factory teel Hdinsightiga siga tegevuse aktiveeritakse siga päringute ise või nõudmisel Windowsi/Linuxi-põhiste Hdinsightiga kobar. Artiklist siga tegevuse (andmete-factory-siga-activity.md) selle tegevuse üksikasjad. **Hdinsightiga MapReduce tegevus** Andmete Factory teel Hdinsightiga MapReduce tegevuse käivitab MapReduce programmide y | 2016-09-26 |
| 20 | [Andmete Factory plaanimis- ja täitmise](data-factory-scheduling-and-execution.md) | CopyActivity2 läheks kui selle CopyActivity1 on edukalt käivitada ja Dataset2 on saadaval ainult. Siin on näide müügivõimaluste JSON: {"nimi": "ChainActivities", "atribuudid": {"kirjeldus": "Käivita tegevuste järjestus", "tegevused": {"tüüp": "Kopeeri", "typeProperties": {"allikas": {"tüüp": "BlobSource"}, "valamu": {"tüüp": "BlobSink", "copyBehavior": "PreserveHierarchy", "writeBatchSize": 0; "writeBatchTimeout": "00: 00:00"}}, "sisendeid": {"nimi": "Dataset1"}, "väljundid": {"nimi": "Dataset2"}, "poliitika": {"ajalõpp": "01: 00:00"}, "scheduler": {"sagedus": "Tund", "intervall": 1}, "nimi": "CopyFromBlob1ToBlob2", "kirjeldus": "Andmed on bloobimälu teisele kopeerimiseks"}, {"tüüp": "Kopeeri", "typeProperties": {"allikas": {"tüüp": "BlobSource"}, "valamu" : {"tüüp": "BlobSink", "writeBatchSize": 0; "writeBatchTimeout": "00: 00:00"}}, "on | 2016-09-28 |





## <a name="tutorials"></a>Õpetused

| &nbsp; | Pealkiri | Kirjeldus |
| --: | :-- | :-- |
| 21 | [Õpetus: Koostada oma esimese müügivõimaluste andmete töötlemise abil Hadoopi kobar](data-factory-build-your-first-pipeline.md) | Õppeteema Azure'i andmed Factory näidatakse, kuidas luua ja ajastamine andmete factory, mis töötleb taru skript kasutamine Hadoopi kobar. |
| 22 | [Õpetus: Koostada oma esimese Azure'i andmed factory Azure'i ressursihaldur malli abil](data-factory-build-your-first-pipeline-using-arm.md) | Selles õpetuses loote valimi Azure'i andmed Factory kohaletoimetamisel on Azure ressursihaldur malli abil. |
| 23 | [Õpetus: Koostada oma esimese Azure'i andmed factory Azure'i portaalis](data-factory-build-your-first-pipeline-using-editor.md) | Selles õpetuses loote valimi Azure'i andmed Factory müügivõimaluste Azure portaali andmete Factory redaktori kasutamine. |
| 24 | [Õpetus: Koostada oma esimese Azure'i andmed factory Azure PowerShelli abil](data-factory-build-your-first-pipeline-using-powershell.md) | Selles õpetuses loote valimi Azure'i andmed Factory müügivõimaluste Azure PowerShelli kaudu. |
| 25 | [Õpetus: Koosta oma Azure esimese andmete factory Microsoft Visual Studio abil](data-factory-build-your-first-pipeline-using-vs.md) | Selles õpetuses loote valimi Azure'i andmed Factory müügivõimaluste Visual Studio abil. |
| 26 | [Õpetus: Luua müügivõimaluste Kopeeri tegevuse Azure'i portaalis](data-factory-copy-activity-tutorial-using-azure-portal.md) | Selles õpetuses loote mõnda Azure'i andmed Factory müügivõimaluste Kopeeri tegevuse Azure'i portaalis andmete Factory redaktori abil. |
| 27 | [Õpetus: Luua müügivõimaluste Kopeeri tegevuse Azure PowerShelli abil](data-factory-copy-activity-tutorial-using-powershell.md) | Selles õpetuses loote mõnda Azure'i andmed Factory müügivõimaluste Kopeeri tegevuse Azure PowerShelli abil. |
| 28 | [Õpetus: Luua müügivõimaluste Kopeeri tegevuse Visual Studio abil](data-factory-copy-activity-tutorial-using-visual-studio.md) | Selles õpetuses loote mõnda Azure'i andmed Factory müügivõimaluste Kopeeri tegevuse Visual Studio abil. |
| 29 | [Kopeerige andmed bloobimälu abil andmete Factory SQL-andmebaasiga](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) | Selle õpetuse näidatakse, kuidas kasutada Kopeeri tegevuse on Azure andmete Factory teel bloobimälu andmete kopeerimine SQL-andmebaasi. |
| 30 | [Õpetus: Luua müügivõimaluste Kopeeri tegevuse andmete Factory koopia viisardi abil](data-factory-copy-data-wizard-tutorial.md) | Selles õpetuses loote mõnda Azure'i andmed Factory müügivõimaluste Kopeeri tegevuse andmete Factory ei toeta Kopeeri viisardi abil |



## <a name="data-movement"></a>Andmete liikumine

| &nbsp; | Pealkiri | Kirjeldus |
| --: | :-- | :-- |
| 31 | [Andmete teisaldamine ja sealt abil Azure'i andmed Factory Azure'i bloobimälu](data-factory-azure-blob-connector.md) | Saate teada, kuidas Azure'i andmed Factory bloobimälu andmeid kopeerida. Kasutage meie: andmete kopeerimine ja Azure'i bloobimälu ja Azure SQL-andmebaas. |
| 32 | [Andmete teisaldamine ja sealt Azure'i Lake andmesalve Azure'i andmed Factory abil](data-factory-azure-datalake-connector.md) | Saate teada, kuidas Azure'i Lake andmesalve abil Azure'i andmed Factory ja sealt andmete teisaldamine |
| 33 | [Andmete teisaldamine ja sealt DocumentDB Azure'i andmed Factory abil](data-factory-azure-documentdb-connector.md) | Siit saate teada, kuidas andmete teisaldamine/Azure'i DocumentDB saidikogumi Azure'i andmed Factory abil |
| 34 | [Andmete teisaldamine ja sealt Azure'i SQL-andmebaasi Azure'i andmed Factory abil](data-factory-azure-sql-connector.md) | Saate teada, kuidas andmeid Azure'i SQL-andmebaasi Azure'i andmed Factory abil teisaldada. |
| 35 | [Andmete teisaldamine ja sealt abil Azure'i andmed Factory Azure SQL-i andmebaas](data-factory-azure-sql-data-warehouse-connector.md) | Saate teada, kuidas Azure SQL-i andmebaas abil Azure'i andmed Factory ja sealt andmete teisaldamine |
| 36 | [Andmete teisaldamine ja sealt Azure'i tabeli Azure'i andmed Factory abil](data-factory-azure-table-connector.md) | Saate teada, kuidas andmeid Azure'i Tabelimälu Azure'i andmed Factory abil teisaldada. |
| 37 | [Kopeerige tegevuse jõudlus ja häälestamise juhend](data-factory-copy-activity-performance.md) | Lisateavet olulisi tegureid, mis andmete liikumine Azure'i andmed Factory jõudlust mõjutavad Kopeeri tegevuste kasutamisel. |
| 38 | [Andmete teisaldamine, kopeerimine tegevuse abil](data-factory-data-movement-activities.md) | Lisateavet andmete liikumine andmete Factory torujuhtmetes: andmete migreerimine pilve poed ja poest kohapealse ja pilveteenuse poe vahel. Kopeeri toiminguga. |
| 39 | [Andmehalduslüüsi väljalaskemärkmed](data-factory-gateway-release-notes.md) | Data Management Gateway tory väljalaskemärkmed |
| 40 | [Teisaldage andmed kohapealse HDFS Azure'i andmed Factory abil](data-factory-hdfs-connector.md) | Lisateavet asutusesisese HDFS Azure'i andmed Factory abil andmeid teisaldada. |
| 41 | [Jälgimine ja haldamine Azure'i andmed Factory torujuhtmete uue jälgimine ja halduse rakenduse abil](data-factory-monitor-manage-app.md) | Saate teada, kuidas jälgimine ja haldamine Azure'i andmed tehased ja torujuhtmed jälgimine ja haldamine rakenduse abil. |
| 42 | [Andmete teisaldamine asutusesisestest allikatest ja pilveteenuse vahel koos Andmehalduslüüs](data-factory-move-data-between-onprem-and-cloud.md) | Häälestada andmete lüüsi andmete kohapealse ja pilveteenuse vahel liikumiseks. Azure'i andmed Factory Andmehalduslüüsi abil andmete teisaldamine. |
| 43 | [Vaid OData allika Azure'i andmed Factory abil andmete teisaldamine](data-factory-odata-connector.md) | Lisateavet andmete teisaldamiseks OData allikatest Azure andmete Factory abil. |
| 44 | [Andmete kaudu ODBC andmete poed Azure'i andmed Factory abil liikumine](data-factory-odbc-connector.md) | Lisateavet ODBC andmete poed Azure'i andmed Factory abil andmeid teisaldada. |
| 45 | [Andmete teisaldamine DB2 Azure'i andmed Factory abil](data-factory-onprem-db2-connector.md) | Siit saate teada, kuidas Azure'i andmed Factory abil DB2 andmebaasist andmete teisaldamine |
| 46 | [Andmete teisaldamine ja sealt kohapealse failisüsteemis Azure'i andmed Factory abil](data-factory-onprem-file-system-connector.md) | Saate teada, kuidas andmete teisaldamine ja sealt kohapealse failisüsteemis Azure'i andmed Factory abil. |
| 47 | [Andmete teisaldamine kaudu MySQL-i abil Azure'i andmed Factory](data-factory-onprem-mysql-connector.md) | Lisateavet andmete teisaldamiseks abil Azure'i andmed Factory MySQL-i andmebaasist. |
| 48 | [Andmete teisaldamine ja neid sealt kohapealse Oracle'i Azure'i andmed Factory abil](data-factory-onprem-oracle-connector.md) | Saate teada, kuidas andmeid Oracle'i andmebaasiga, mis on kohapealse Azure'i andmed Factory abil teisaldada. |
| 49 | [Teisaldage andmed PostgreSQL-i abil Azure'i andmed Factory](data-factory-onprem-postgresql-connector.md) | Lisateavet andmete teisaldamiseks abil Azure'i andmed Factory PostgreSQL-i andmebaasist. |
| 50 | [Teisaldage andmed Sybase'i Azure'i andmed Factory abil](data-factory-onprem-sybase-connector.md) | Lisateavet andmete teisaldamiseks abil Azure'i andmed Factory Sybase'i andmebaasist. |
| 51 | [Teisaldage andmed Teradata Azure'i andmed Factory abil](data-factory-onprem-teradata-connector.md) | Lisateavet Teradata konnektor jaoks andmete Factory teenus, mis võimaldab Teradata andmebaasist andmete teisaldamine |
| 52 | [Azure'i andmed Factory abil andmete ja asutusesisesest SQL serveri või mis IaaS (Azure'i VM)](data-factory-sqlserver-connector.md) | Lisateavet SQL Serveri andmebaasiga, mis on kohapealse ja sealt või Azure VM, mis Azure'i andmed Factory kasutamine andmete teisaldamiseks. |
| 53 | [Teisaldage andmed tabeli veebiallika Azure'i andmed Factory abil](data-factory-web-table-connector.md) | Teada, kuidas andmete teisaldamiseks asutusesisesest tabeli abil Azure'i andmed Factory veebilehele. |



## <a name="data-transformation"></a>Andmete teisendus

| &nbsp; | Pealkiri | Kirjeldus |
| --: | :-- | :-- |
| 54 | [Looge sõnastikupõhise torujuhtmete Azure seadme õppimise abil](data-factory-azure-ml-batch-execution-activity.md) | Kirjeldab, kuidas luua loomine sõnastikupõhise torujuhtmete abil Azure'i andmed Factory ja Azure arvuti koolitus |
| 55 | [Arvutage lingitud teenused](data-factory-compute-linked-services.md) | Teave Arvuta enviornments, et abil saate Azure'i andmed Factory torujuhtmetes transformatsioon/protsessi andmeid. |
| 56 | [Protsess suurte andmekomplektide andmete Factory ja paketi abil](data-factory-data-processing-using-batch.md) | Selles artiklis kirjeldatakse töötlemine andmed on Azure andmete Factory teel paralleelne töötlemine videovõimaluste kontrollimine Azure'i paketi abil. |
| 57 | [Muuta andmete Azure'i andmed Factory](data-factory-data-transformation-activities.md) | Saate teada, kuidas andmeid või Azure'i andmed Factory Hadoopi, seadme õ või Azure Lake andmeanalüüsi protsessi andmeid muuta. |
| 58 | [Hadoopi Streaming tegevus](data-factory-hadoop-streaming-activity.md) | Siit saate teada, kuidas saate Hadoopi Streaming tegevuse on Azure andmete factory käivitada Hadoopi Streaming programmide kohta – nõudmisel/teie enda Hdinsightiga kobar. |
| 59 | [Taru tegevus](data-factory-hive-activity.md) | Siit saate teada, kuidas saate taru tegevuse on Azure andmete factory taru päringute käivitamise kohta – nõudmisel/teie enda Hdinsightiga kobar. |
| 60 | [Autonoomsest andmete Factory MapReduce programmides](data-factory-map-reduce.md) | Saate teada, kuidas töötab MapReduce programmid klõpsake mõnda Windows Azure Hdinsightiga kobar on Azure andmete factory andmete töötlemiseks. |
| 61 | [Siga tegevus](data-factory-pig-activity.md) | Siit saate teada, kuidas saate siga tegevuse on Azure andmete factory siga skriptide käitamiseks kohta – nõudmisel/teie enda Hdinsightiga kobar kohta. |
| 62 | [Autonoomsest andmete Factory säde programmides](data-factory-spark.md) | Saate teada, kuidas kasutada säde programmides Azure'i andmed factory, mis MapReduce tegevuse abil. |
| 63 | [SQL serveri salvestatud protseduur tegevus](data-factory-stored-proc-activity.md) | Siit saate teada, kuidas saate kasutada SQL serveri salvestatud protseduur tegevuse kasutada salvestatud protseduur Azure'i SQL-andmebaasi või SQL Azure'i andmebaas: andmete Factory müügivõimaluste. |
| 64 | [Kasutage kohandatud tegevuste on Azure andmete Factory müügivõimaluste](data-factory-use-custom-activities.md) | Saate teada, kuidas luua kohandatud tegevuste ja kasutada neid on Azure andmete Factory teel. |
| 65 | [Käivitage U-SQL skripti Azure andmeanalüüsi Lake Azure'i andmed Factory](data-factory-usql-activity.md) | Saate teada, kuidas käivitades skriptide U-SQL Azure'i andmeanalüüsi Lake Arvuta teenuse andmete töötlemiseks. |



## <a name="samples"></a>Näidised

| &nbsp; | Pealkiri | Kirjeldus |
| --: | :-- | :-- |
| 66 | [Azure'i andmed Factory - näidised](data-factory-samples.md) | Annab üksikasjade näidised, mis on Azure andmete Factory teenus. |



## <a name="use-cases"></a>Kasutage juhul

| &nbsp; | Pealkiri | Kirjeldus |
| --: | :-- | :-- |
| 67 | [Kliendi juhtumianalüüsid](data-factory-customer-case-studies.md) | Lugege teemat Kuidas mõned meie kliendid on antud Azure'i andmed Factory. |
| 68 | [Kasutusmall - kliendi profiili](data-factory-customer-profiling-usecase.md) | Siit saate teada, kuidas saab luua Andmepõhiste töövoo (müügivõimaluste) profiili mängimine kliendid Azure'i andmed Factory. |
| 69 | [Kasutusmall - toote soovitused](data-factory-product-reco-usecase.md) | Lisateavet mõne kasutamine juhul rakendada, kasutades Azure'i andmed Factory koos muude teenustega. |



## <a name="monitor-and-manage"></a>Jälgimine ja haldamine

| &nbsp; | Pealkiri | Kirjeldus |
| --: | :-- | :-- |
| 70 | [Jälgimine ja haldamine Azure'i andmed Factory torujuhtmete](data-factory-monitor-manage-pipelines.md) | Saate teada, kuidas Azure'i portaal ja Azure PowerShelli abil jälgimine ja haldamine Azure'i andmed tehased ja torujuhtmed loomist. |



## <a name="sdk"></a>SDK

| &nbsp; | Pealkiri | Kirjeldus |
| --: | :-- | :-- |
| 71 | [Azure'i andmed Factory - .NET API muutmine Logi](data-factory-api-change-log.md) | Kirjeldab server muudatused, funktsiooni täiendusi ja veaparandused jne... .net-i API Azure'i andmed Factory teatud versioonis. |
| 72 | [Luua, jälgimine ja haldamine Azure'i andmed tehased andmete Factory .NET SDK abil](data-factory-create-data-factories-programmatically.md) | Saate teada, kuidas programmiliselt loomine, jälgimine ja haldamine Azure'i andmed tehased andmete Factory SDK abil. |
| 73 | [Azure'i andmed Factory Tootearendusmaterjal](data-factory-sdks.md) | Lisateavet võimalust luua, jälgimine ja haldamine Azure'i andmed tehased |



## <a name="miscellaneous"></a>Mitmesugused

| &nbsp; | Pealkiri | Kirjeldus |
| --: | :-- | :-- |
| 74 | [Azure'i andmed Factory - korduma kippuvad küsimused](data-factory-faq.md) | Korduma kippuvad küsimused Azure'i andmete hankimise kohta. |
| 75 | [Azure'i andmed Factory - funktsioonide ja süsteemi muutujad](data-factory-functions-variables.md) | Azure'i andmed Factory funktsioonid ja süsteemi muutujate loend |
| 76 | [Azure'i andmed Factory - nime andmise reeglid](data-factory-naming-rules.md) | Kirjeldatakse andmete Factory üksuste nimede reeglid. |
| 77 | [Andmete Factory probleemide tõrkeotsing](data-factory-troubleshoot.md) | Saate teada, kuidas Azure'i andmed Factory seotud probleemide tõrkeotsing. |

