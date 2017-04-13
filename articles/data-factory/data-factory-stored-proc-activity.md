<properties 
    pageTitle="SQL serveri salvestatud protseduur tegevus" 
    description="Siit saate teada, kuidas saate kasutada SQL serveri salvestatud protseduur tegevuse kasutada salvestatud protseduur Azure'i SQL-andmebaasi või SQL Azure'i andmebaas: andmete Factory müügivõimaluste." 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/30/2016" 
    ms.author="spelluru"/>

# <a name="sql-server-stored-procedure-activity"></a>SQL serveri salvestatud protseduur tegevus
> [AZURE.SELECTOR]
[Taru](data-factory-hive-activity.md)  
[Siga](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Hadoopi Streaming](data-factory-hadoop-streaming-activity.md)
[Masina õ](data-factory-azure-ml-batch-execution-activity.md) 
[Salvestatud protseduur](data-factory-stored-proc-activity.md)
[Andmete Lake Analytics U-SQL-i](data-factory-usql-activity.md)
[.NET kohandatud](data-factory-use-custom-activities.md)

SQL serveri salvestatud protseduur tegevuse andmete Factory [müügivõimaluste](data-factory-create-pipelines.md) abil saate kasutada salvestatud protseduuri järgmised andmed poed: 


- Azure'i SQL-andmebaas 
- SQL Azure'i andmebaas  
- SQL serveri andmebaasi teie ettevõtte või mõne Azure VM. Peate installima Andmehalduslüüsi samasse arvutisse, mis hostib andmebaasi või eraldi arvutisse omavahelise ressursid andmebaasi. Andmehalduslüüs on tarkvara, mis ühendab kohapealse andmete allikad/andmeallikate hosed Azure'i vms pilveteenustega turvaline ja hallatavate. Vt artikli [andmete kohapealse ja pilveteenuse vahel liikumine](data-factory-move-data-between-onprem-and-cloud.md) Andmehalduslüüsi üksikasjad. 

Selles artiklis põhineb [tegevusi andmete teisendamiseks](data-factory-data-transformation-activities.md) artikkel, mis tutvustab andmete teisendamiseks ja toetatud teisendus tegevuste ülevaade.

## <a name="walkthrough"></a>Kiirtutvustus

### <a name="sample-table-and-stored-procedure"></a>Näidistabeli ja salvestatud protseduur
1. Järgmises **tabelis** Azure SQL-i andmebaasi SQL Server Management Studio või mõnda muud tööriista, olete rahul abil luua. Datetimestamp veerg on kuupäev ja kellaaeg, millal vastav ID luuakse. 

        CREATE TABLE dbo.sampletable
        (
            Id uniqueidentifier,
            datetimestamp nvarchar(127)
        )
        GO

        CREATE CLUSTERED INDEX ClusteredID ON dbo.sampletable(Id);
        GO

    ID on kordumatu tuvastatud ja datetimestamp veerg on kuupäev ja kellaaeg, millal vastav ID luuakse.
    ![Näidisandmed](./media/data-factory-stored-proc-activity/sample-data.png)

    > [AZURE.NOTE] See näide kasutab Azure'i SQL-andmebaasi, kuid töötab samal viisil SQL Azure'i andmebaas ja SQL serveri andmebaasi. 
2. Saate luua, kuvatakse järgmine **salvestatud protseduur** andmete **sampletable**sisestatava.

        CREATE PROCEDURE sp_sample @DateTime nvarchar(127)
        AS
        
        BEGIN
            INSERT INTO [sampletable]
            VALUES (newid(), @DateTime)
        END

    > [AZURE.IMPORTANT] **Nime** ja **kest** parameetri (käesoleva näite kuupäev ja kellaaeg) peab vastama mis müügivõimaluste/tegevuse JSON määratud. Salvestatud protseduur määratlus, veenduge, et **@** parameetri eesliide kasutatakse.
    
### <a name="create-a-data-factory"></a>Andmete factory loomine  
4. [Azure'i portaali](https://portal.azure.com/)sisse logida. 
5. Klõpsake vasakul menüüs **Uus** , klõpsake **ärianalüüsi + Analytics**ja klõpsake **Andmete Factory**.
    
    ![Uute andmete factory](media/data-factory-stored-proc-activity/new-data-factory.png)   
4.  **Uute andmete factory** tera, sisestage **SProcDF** nimi. Azure'i andmed Factory nimed on **globaalselt kordumatu**. Peate nime factory andmeid koos teie nimega lubamiseks luua selle factory eesliide.

    ![Uute andmete factory](media/data-factory-stored-proc-activity/new-data-factory-blade.png)      
3.  Valige oma **Azure'i tellimus**. 
4.  **Ressursirühm**, tehke järgmist. 
    1.  Klõpsake nuppu **Loo uus** ja sisestage nimi ressursirühma.
    2.  Klõpsake käsku **Kasuta olemasolevat** ressursi olemasolevasse rühma valige.  
5.  Valige **asukoht** , andmete factory.
6.  Valige **Kinnita armatuurlaua** nii, et näete andmete factory armatuurlaual järgmine kord, kui logite sisse. 
6.  Klõpsake nuppu **Loo** **Uus andmete factory** enne.
6.  Näete andmete factory, luuakse **armatuurlaua** Azure portaali. Pärast andmete factory on loodud, kuvatakse andmete factory leht, mis kuvatakse andmete factory sisu.
    ![Andmete Factory Avaleht](media/data-factory-stored-proc-activity/data-factory-home-page.png)

### <a name="create-an-azure-sql-linked-service"></a>Lingitud Azure SQL-i teenuse loomine  
Pärast andmete factory loomist saate luua lingitud Azure SQL-i teenus, mis ühendab Azure'i SQL-andmebaasi andmete factory. See andmebaas sisaldab selle sampletable tabeli ja sp_sample salvestatud protseduur.

7.  Klõpsake **autor ja juurutamine** **Andmete Factory** enne **SProcDF** andmete Factory Editor käivitamiseks jaoks.
2.  Klõpsake käsku **uute andmete talletamiseks** ja valige **Azure SQL-andmebaas**. Peaksite nägema JSON script Editor lingitud Azure SQL-i teenuse loomiseks. 

    ![Uue andmesalve](media/data-factory-stored-proc-activity/new-data-store.png)
4. JSON skript, saate teha järgmisi muudatusi: 
    1. Asendage ** &lt;serverinimi&gt; ** Azure'i SQL-andmebaasi serveri nimi.
    2. Asendage ** &lt;databasename&gt; ** andmebaasi, mille lõite tabeli ja salvestatud protseduur.
    3. Asendage ** &lt; username@servername ** on juurdepääs andmebaasile kasutajakontoga.
    4. Asendage ** &lt;parooli&gt; ** kasutajakonto parooliga. 

    ![Uue andmesalve](media/data-factory-stored-proc-activity/azure-sql-linked-service.png)
5. Klõpsake **Deploy** käsuriba lingitud teenuse kasutuselevõtuks. Veenduge, et näete AzureSqlLinkedService puuvaates vasakul. 

    ![lingitud teenusega puuvaade](media/data-factory-stored-proc-activity/tree-view.png)

### <a name="create-an-output-dataset"></a>Mõne väljundi andmekomplekti loomine
6. Klõpsake **... Lisateavet** klõpsake tööriistaribal nuppu **Uus andmekomplekti**ja **Azure SQL-i**nuppu. **Uue andmehulga** käsuriba ja valige **Azure SQL-i**.

    ![lingitud teenusega puuvaade](media/data-factory-stored-proc-activity/new-dataset.png)
7. Kopeerige/kleepige järgmine JSON script Editor JSON.

        {               
            "name": "sprocsampleout",
            "properties": {
                "type": "AzureSqlTable",
                "linkedServiceName": "AzureSqlLinkedService",
                "typeProperties": {
                    "tableName": "sampletable"
                },
                "availability": {
                    "frequency": "Hour",
                    "interval": 1
                }
            }
        }
7. Klõpsake **Deploy** käsuriba andmekomplekti juurutamiseks. Veenduge, et näete andmekomplekti puuvaates. 

    ![puuvaade lingitud teenustega](media/data-factory-stored-proc-activity/tree-view-2.png)

### <a name="create-a-pipeline-with-sqlserverstoredprocedure-activity"></a>Müügivõimaluste SqlServerStoredProcedure tegevuse loomine
Nüüd, müügivõimaluste SqlServerStoredProcedure toimingute loomine.
 
9. Klõpsake **... Lisateavet** käsuriba ja klõpsake käsku **Uus kohaletoimetamisel**. 
9. Kopeerige/kleepige järgmine JSON koodilõigu. **StoredProcedureName** seatud **sp_sample**. Nime ja kest **DateTime** parameetri peab vastama nime ja kest parameetri salvestatud protseduur määratlus.  

        {
            "name": "SprocActivitySamplePipeline",
            "properties": {
                "activities": [
                    {
                        "type": "SqlServerStoredProcedure",
                        "typeProperties": {
                            "storedProcedureName": "sp_sample",
                            "storedProcedureParameters": {
                                "DateTime": "$$Text.Format('{0:yyyy-MM-dd HH:mm:ss}', SliceStart)"
                            }
                        },
                        "outputs": [
                            {
                                "name": "sprocsampleout"
                            }
                        ],
                        "scheduler": {
                            "frequency": "Hour",
                            "interval": 1
                        },
                        "name": "SprocActivitySample"
                    }
                ],
                "start": "2016-08-02T00:00:00Z",
                "end": "2016-08-02T05:00:00Z",
                "isPaused": false
            }
        }

    Kui teil on vaja parameetri null, kasutada järgmist süntaksit: "param1": null (kõik väiketähed). 
9. Klõpsake tööriistariba kasutada tulemas **Deploy** .  

### <a name="monitor-the-pipeline"></a>Funktsiooni müügivõimaluste jälgimine

6. Klõpsake nuppu **X** andmete Factory Editor labad sulgemiseks ja andmete Factory tera tagasiliikumiseks ja nuppu **skeem**.

    ![diagrammi paan](media/data-factory-stored-proc-activity/data-factory-diagram-tile.png)
7. **Diagrammivaate**näete torujuhtmed ja andmekomplektide kasutatud selles õpetuses ülevaade. 

    ![diagrammi paan](media/data-factory-stored-proc-activity/data-factory-diagram-view.png)
8. Topeltklõpsake vaates diagrammi andmekomplekti **sprocsampleout**. Näete sektorid valmis olekus. Peaks olema viis sektorit, kuna tükk on loodud iga tunni alguskellaaeg ja lõppkellaaeg selle JSON kaudu.

    ![diagrammi paan](media/data-factory-stored-proc-activity/data-factory-slices.png) 
10. Kui tükk on **valmis** olekus, käivitada, * *Valige* sampletable** päring SQL Azure'i andmebaasi kinnitamaks, et andmed sisestatud tabelisse, salvestatud protseduur.

    ![Väljundi andmed](./media/data-factory-stored-proc-activity/output.png)

    Azure'i andmed Factory torustike jälgimise kohta lisateabe saamiseks vt [kuvari tulemas](data-factory-monitor-manage-pipelines.md) .  

> [AZURE.NOTE] Selles näites on soovitud SprocActivitySample pole sisendeid. Kui soovite keti (ehk teisisõnu öeldes eelneva töötlemine) tegevusega enne selle tegevuse, varustava tegevuse väljundid saab kasutada sisendina selle tegevusega. Sellisel juhul see tegevus käivitada varustava tegevuse lõpulejõudmist ja varustava tegevuse tulemused on saadaval (on valmis olek). Sisendeid ei saa kasutada otse parameetrina tegevuse salvestatud protseduur

## <a name="json-format"></a>JSON-vormingus
    {
        "name": "SQLSPROCActivity",
        "description": "description", 
        "type": "SqlServerStoredProcedure",
        "inputs":  [ { "name": "inputtable"  } ],
        "outputs":  [ { "name": "outputtable" } ],
        "typeProperties":
        {
            "storedProcedureName": "<name of the stored procedure>",
            "storedProcedureParameters":  
            {
                "param1": "param1Value"
                …
            }
        }
    }

## <a name="json-properties"></a>JSON atribuudid

Atribuut | Kirjeldus | Nõutav
-------- | ----------- | --------
Nimi | Tegevuse nimi | Jah
kirjeldus | Tekst, mis kirjeldab tegevuse kasutatud | Ei
tüüp | SqlServerStoredProcedure | Jah
sisendina | Valikuline. Kui määrate mõne Sisestuskeel andmekomplekti, peab olema ('Valmis' olek) saadaval salvestatud protseduur tegevuste käivitamiseks. Sisestuskeel andmekomplekti ei saa parameetrina salvestatud protseduur tarbitud. Seda kasutatakse ainult kontrollida sõltuvus enne alustamist salvestatud protseduur tegevuse. | Ei
väljundid | Määrake soovitud väljundi andmekomplekti salvestatud protseduur tegevuste jaoks. Väljundi andmekomplekti määrab **ajakava** salvestatud protseduur tegevuste (tunni, iga nädal, iga kuu, jne). <br/><br/>Väljundi andmekomplekti tuleb kasutada **lingitud teenus** , mis viitab Azure'i SQL-andmebaasi või Azure SQL-i andmebaas või SQL Serveri andmebaas, milles soovite käivitada salvestatud protseduur. <br/><br/>Väljundi andmekomplekti võib olla nii edasi salvestatud protseduur tulemi edaspidised töötlemise muu tegevuse ([Aheldamise tegevuste](data-factory-scheduling-and-execution.md#chaining-activities)) teel. Siiski andmete Factory automaatselt kirjuta salvestatud toimingu väljund selle andmekomplekti. See on salvestatud protseduur, mis kirjutab SQL tabeli, mis osutab väljundi andmekomplekti. <br/><br/>Mõnel juhul võib väljundi andmekomplekti **Fiktiivne andmekomplekti**, mida kasutatakse ainult selleks, et määrata töötab salvestatud protseduur tegevuse ajakava. | Jah
storedProcedureName | Määrake SQL Azure'i andmebaasi või Azure SQL-i andmebaas, mis esindab lingitud teenus, mis kasutab väljund tabeli nimi salvestatud protseduur. | Jah
storedProcedureParameters | Määrake salvestatud protseduur parameetrite väärtused. Kui peate läbima parameetri null, kasutada järgmist süntaksit: "param1": null (kõik väiketähed). Järgmises näites selle atribuudi kasutamise kohta leiate teemast.| Ei

## <a name="passing-a-static-value"></a>Staatiline väärtus läbides 
Nüüd vaatame lisada teise veeru nimega "Stsenaarium" tabelis, mis sisaldab staatiline väärtus nimega "Dokument valimi".

![Näidisandmete 2](./media/data-factory-stored-proc-activity/sample-data-2.png)

    CREATE PROCEDURE sp_sample @DateTime nvarchar(127) , @Scenario nvarchar(127)
    
    AS
    
    BEGIN
        INSERT INTO [sampletable]
        VALUES (newid(), @DateTime, @Scenario)
    END

Nüüd, andke stsenaarium parameetri ja väärtuse salvestatud protseduur tegevuse. Valimis eelmise jaotise typeProperties näeb välja järgmine koodilõigu:

    "typeProperties":
    {
        "storedProcedureName": "sp_sample",
        "storedProcedureParameters": 
        {
            "DateTime": "$$Text.Format('{0:yyyy-MM-dd HH:mm:ss}', SliceStart)",
            "Scenario": "Document sample"
        }
    }

