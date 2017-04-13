<properties
    pageTitle="Andmete Factory õpetus: esimese andmete müügivõimaluste | Microsoft Azure'i"
    description="Õppeteema Azure'i andmed Factory näidatakse, kuidas luua ja ajastamine andmete factory, mis töötleb Hadoopi kobar taru skript kasutamine andmete."
    services="data-factory"
    keywords="Azure'i andmed factory kuueosalisest, hadoop kobar Hadoopi taru"
    documentationCenter=""
    authors="spelluru"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="data-factory"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article" 
    ms.date="09/26/2016"
    ms.author="spelluru"/>

# <a name="tutorial-build-your-first-pipeline-to-process-data-using-hadoop-cluster"></a>Õpetus: Koostage oma esimese müügivõimaluste andmete töötlemise abil Hadoopi kobar 
> [AZURE.SELECTOR]
- [Ülevaade ja eeltingimused](data-factory-build-your-first-pipeline.md)
- [Azure'i portaal](data-factory-build-your-first-pipeline-using-editor.md)
- [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
- [PowerShelli](data-factory-build-your-first-pipeline-using-powershell.md)
- [Ressursihaldur Mall](data-factory-build-your-first-pipeline-using-arm.md)
- [REST API-GA](data-factory-build-your-first-pipeline-using-rest-api.md)

Selles õpetuses loote oma esimese Azure'i andmed factory andmete kohaletoimetamisel, mis töötleb käivitades mõne Windows Azure Hdinsightiga (Hadoopi) kobar taru skripti abil. 

> [AZURE.NOTE] See artikkel ei paku kontseptuaalne ülevaade Azure'i andmed Factory. Teenuse kontseptuaalne ülevaate leiate teemast [Sissejuhatus Azure'i andmed Factory](data-factory-introduction.md). Lugege teemat [andmete Factory õppeteema](https://azure.microsoft.com/documentation/learning-paths/data-factory/) soovitatav viis liikumiseks meie sisu jaoks andmete hankimise kohta.

## <a name="whats-covered-in-this-tutorial"></a>Mis on selles õpetuses kaetud? 
**Azure'i andmed Factory** võimaldab teil koostada andmete **liikumine** ja andmete **töötlemiseks** tööülesannete nimega Andmepõhiste töövoogude (nimetatakse ka andmete torujuhtmete). Saate teada, kuidas luua oma andmete müügivõimaluste andmetöötlus (või andmete teisendus) esimese tegevusega. See tegevus kasutab Hdinsightiga Hadoopi kobar muuta ja analüüsida valimi web logid.  

Selles õppetükis saate tehke järgmist:

1.  Saate luua **andmete factory**. Andmete factory võib sisaldada ühte või rohkem andmeid välistrasside protsess ja teisaldamine andmeid. 
2.  Looge **lingitud teenused**. Saate luua lingitud teenuse andmete poe link või Arvuta teenuse andmete factory. Andmesalve, nt Azure Storage hoiab sisend andmete tegevuse teel. Arvuta teenuse Hdinsightiga Hadoopi kobar nt protsessid/teisendusteta andmed.    
3.  Sisestusmeetodi loomine ja **andmekomplektide**väljund. Mõne Sisestuskeel andmekomplekti tähistab sisend tegevuse teel ja mõne väljundi andmekomplekti tegevuse väljundi.
3.  **Müügivõimaluste**loomine Müügivõimaluste võib olla üks või mitu tegevuste (näited: Kopeeri tegevust, Hdinsightiga taru tegevuse). See näide kasutab Hdinsightiga taru tegevust, mis töötab taru skripti Hdinsightiga Hadoopi kobar. Skripti esmalt loob tabeli, mis viitab Azure'i bloobimälu talletatud töötlemata web log andmed ja seejärel piirded töötlemata andmete aasta ja kuu.

    On kahte tüüpi toetatavate Azure'i andmed Factory. Need on: [andmete liikumine](data-factory-data-movement-activities.md) ja [andmete teisendus tegevus](data-factory-data-transformation-activities.md). On ainult üks andmete liikumine tegevus, mis on tegevuse Kopeeri. Selles õpetuses te ei kasuta tegevuse Kopeeri. Kopeeri tegevuse kasutamise juhendi leiate [Õppeteema: andmete kopeerimine, on Azure SQL Azure'i Bloobivahemälu](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md). Hdinsightiga taru tegevuse kasutate selles õpetuses on üks andmete teisendus tegevuste andmete Factory ei toeta.  
 
Siin on valimi andmete factory koostate selles õpetuses **diagrammivaate** . 

![Diagrammivaate andmete Factory õpetuses](./media/data-factory-build-your-first-pipeline/data-factory-tutorial-diagram-view.png)

Selles õpetuses **adfgetstarted** Azure'i bloobimälu ümbrisest **inputdata** kaust sisaldab ühte faili nimega input.log. Selle faili on kolm kuud kirjed: jaanuar, veebruari ja märtsi, 2016. Siin on valimi read iga kuu Sisestuskeel failis. 

    2016-01-01,02:01:09,SAMPLEWEBSITE,GET,/blogposts/mvc4/step2.png,X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,53175,871 
    2016-02-01,02:01:10,SAMPLEWEBSITE,GET,/blogposts/mvc4/step7.png,X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,30184,871
    2016-03-01,02:01:10,SAMPLEWEBSITE,GET,/blogposts/mvc4/step7.png,X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,30184,871

Kui faili töötleb müügivõimaluste Hdinsightiga taru tegevust, tegevuse töötab taru skripti Hdinsightiga kobar, et sektsioonid sisestada andmeid, aasta ja kuu. Skripti loob kolme väljundi kaustad, mis sisaldavad fail, mille kirjed alates iga kuu.  

    adfgetstarted/partitioneddata/year=2016/month=1/000000_0
    adfgetstarted/partitioneddata/year=2016/month=2/000000_0
    adfgetstarted/partitioneddata/year=2016/month=3/000000_0

Valimi ridade eespool näidatud, esimene (koos 2016-01-01) on kirjutatud 000000_0 faili kuu = 1 kaust. Samuti teine kirjutatakse faili kuu = 2 kausta ja kolmas ühte kirjutatakse faili kuu = 3 kausta.  


## <a name="pre-requisites"></a>Eeltingimused
Enne alustamist selles õpetuses, peab teil olema järgmised eeltingimused:

1.  **Azure'i tellimuse** – kui teil pole Azure tellimuse, saate luua tasuta prooviversiooni konto vaid paar minutit. [Tasuta prooviversiooni](https://azure.microsoft.com/pricing/free-trial/) artiklist kohta, kuidas saate hankida tasuta prooviversiooni konto.

2.  **Azure Storage** – selles õpetuses andmete talletamiseks kasutada Azure storage kontot. Kui teil pole kontot Azure storage, leiate artiklist [Loo konto salvestusruumi](../storage/storage-create-storage-account.md#create-a-storage-account) . Pärast seda, kui olete loonud salvestusruumi konto, Pange kirja **konto nimi** ja **juurdepääsu võti**. [Vaade, kopeerimine ja uuesti luua salvestusruumi Pääsuklahvide](../storage/storage-create-storage-account.md#view-and-copy-storage-access-keys)kuvamine 

### <a name="upload-files-to-azure-storage-for-the-tutorial"></a>Õpetuse Azure Storage failide üleslaadimine
Enne õpetusega alustamist peate ettevalmistamine Azure Storage konto õpetuse valimi failidega.

1. Üles laadida **skript** kausta **adfgetstarted** bloobimälu ümbrisest taru päringu fail (HQL).
2. Sisestuskeel faili **adfgetstarted** bloobimälu ümbrisest **inputdata** kausta üles laadida. 

#### <a name="create-hql-script-file"></a>Looge HQL skriptifail 

1. Käivitage **Notepad** ja kleepige järgmine HQL skript. Taru skript loob kahe tabeli: **WebLogsRaw** ja **WebLogsPartitioned**. Klõpsake menüüd **fail** ja valige käsk **Salvesta nimega**. Aktiveerige **C:\adfgetstarted** kausta, kõvakettale. Valige * *kõik failid (*.*) **jaoks soovitud** Salvestage** tippimise ajal välja. Sisestage **partitionweblogs.hql** jaoks soovitud **faili nimi**. Veenduge, et selle **kodeerimine** dialoogiboksi allservas väärtuseks on seatud **ANSI**. Kui pole, määrake selleks **ANSI **.  

        set hive.exec.dynamic.partition.mode=nonstrict;
        
        DROP TABLE IF EXISTS WebLogsRaw; 
        CREATE TABLE WebLogsRaw (
          date  date,
          time  string,
          ssitename string,
          csmethod  string,
          csuristem  string,
          csuriquery string,
          sport int,
          susername string,
          cipcsUserAgent string,
          csCookie string,
          csReferer string,
          cshost  string,
          scstatus  int,
          scsubstatus  int,
          scwin32status  int,
          scbytes int,
          csbytes int,
          timetaken int
        )
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        LINES TERMINATED BY '\n' 
        tblproperties ("skip.header.line.count"="2");
        
        LOAD DATA INPATH '${hiveconf:inputtable}' OVERWRITE INTO TABLE WebLogsRaw;
        
        DROP TABLE IF EXISTS WebLogsPartitioned ; 
        create external table WebLogsPartitioned (  
          date  date,
          time  string,
          ssitename string,
          csmethod  string,
          csuristem  string,
          csuriquery string,
          sport int,
          susername string,
          cipcsUserAgent string,
          csCookie string,
          csReferer string,
          cshost  string,
          scstatus  int,
          scsubstatus  int,
          scwin32status  int,
          scbytes int,
          csbytes int,
          timetaken int
        )
        partitioned by ( year int, month int)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' 
        STORED AS TEXTFILE 
        LOCATION '${hiveconf:partitionedtable}';
        
        INSERT INTO TABLE WebLogsPartitioned  PARTITION( year , month) 
        SELECT
          date,
          time,
          ssitename,
          csmethod,
          csuristem,
          csuriquery,
          sport,
          susername,
          cipcsUserAgent,
          csCookie,
          csReferer,
          cshost,
          scstatus,
          scsubstatus,
          scwin32status,
          scbytes,
          csbytes,
          timetaken,
          year(date),
          month(date)
        FROM WebLogsRaw

Käitusajal taru tegevuse andmete Factory tulemas edastab väärtuste soovitud **inputtable** ja **partitionedtable** parameetrid, nagu on näidatud järgmises koodilõigu:  

        "inputtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/inputdata",
        "partitionedtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/partitioneddata"

**Storageaccountname** on Azure storage konto nimi.
 
#### <a name="create-a-sample-input-file"></a>Sisestuskeel näidisfaili loomine
Kasutades notepad, looge fail nimega **input.log** sisse **c:\adfgetstarted** sisu on järgmine: 

    #Software: Microsoft Internet Information Services 8.0
    #Fields: date time s-sitename cs-method cs-uri-stem cs-uri-query s-port cs-username c-ip cs(User-Agent) cs(Cookie) cs(Referer) cs-host sc-status sc-substatus sc-win32-status sc-bytes cs-bytes time-taken
    2016-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step2.png X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 53175 871 46
    2016-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step3.png X-ARR-LOG-ID=9eace870-2f49-4efd-b204-0d170da46b4a 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 51237 871 32
    2016-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step4.png X-ARR-LOG-ID=4bea5b3d-8ac9-46c9-9b8c-ec3e9500cbea 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 72177 871 47
    2016-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step5.png X-ARR-LOG-ID=9b0c14b1-434d-495a-9b0d-46775194257b 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 37931 871 32
    2016-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step6.png X-ARR-LOG-ID=f99cff81-ec38-4a13-b2fe-21b10adca996 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 27146 871 47
    2016-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step1.png X-ARR-LOG-ID=af94d3b4-8e05-4871-82c4-638f866d4e83 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 66259 871 140
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-03-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-03-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-03-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-03-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-03-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47

#### <a name="upload-input-file-and-hql-file-to-your-azure-blob-storage"></a>Azure'i bloobimälu oma Sisestuskeel ja HQL faili üleslaadimine

See jaotis sisaldab juhiseid input.log ja partitionweblogs.hql failide kopeerimine Azure'i bloobimälu **AzCopy** tööriista abil. Saate kasutada mis tahes teie valitud tööriista (nt: [Microsoft Azure salvestusruumi Exploreri](http://storageexplorer.com/), [CloudXPlorer ClumsyLeaf tarkvara abil](http://clumsyleaf.com/products/cloudxplorer) selleks.   
     
1. Alla laadida Office'i [uusima versiooni **AzCopy**](http://aka.ms/downloadazcopy)või [Eelvaade uusim versioon](http://aka.ms/downloadazcopypr). Vt [AzCopy kasutamise kohta](../storage/storage-use-azcopy.md) artikli juhised kasuliku abil.
2. Liikuge kausta, c:\adfgetstarted ja käivitage järgmine käsk: 

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\AzCopy" /Source:. /Dest:https://<storageaccountname>.blob.core.windows.net/adfgetstarted/inputdata /DestKey:<storageaccesskey>  /Pattern:input.log

    See käsk lisatud faili **input.log** salvestusruumi konto (**adfgetstarted** container ja **inputdata** kausta). Asendage nimi oma konto salvestusruumi ja **storageaccesskey** **storageaccountname** salvestusruumi Accessi võti.

    > [AZURE.NOTE] See käsk loob nimega **adfgetstarted** Azure'i bloobimälus ümbris ja kopeerib **input.log** faili kohalikult kettalt **inputdata** kausta ümbrises. 
    
3. Kui fail on nüüd üles laaditud, näete umbes järgmine: AzCopy väljundi.
    
        Finished 1 of total 1 file(s).
        [2015/12/16 23:07:33] Transfer summary:
        -----------------
        Total files transferred: 1
        Transfer successfully:   1
        Transfer skipped:        0
        Transfer failed:         0
        Elapsed time:            00.00:00:01
5. Käivitage järgmine käsk **skripti** kausta **adfgetstarted** -ümbrisest **partitionweblogs.hql** faili üles laadida. Siin on käsk: 
    
        AzCopy /Source:. /Dest:https://<storageaccountname>.blob.core.windows.net/adfgetstarted/script /DestKey:<storageaccesskey>  /Pattern:partitionweblogs.hql

Eeltingimused on lõpule viidud. Saate luua andmete factory, kasutades ühte järgmistest viisidest. Klõpsake menüüd ülemises või õpetuse sooritamiseks järgmisi linke. 

- [Azure'i portaal](data-factory-build-your-first-pipeline-using-editor.md)
- [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
- [PowerShelli](data-factory-build-your-first-pipeline-using-powershell.md)
- [Ressursihaldur Mall](data-factory-build-your-first-pipeline-using-arm.md)
- [REST API-GA](data-factory-build-your-first-pipeline-using-rest-api.md)