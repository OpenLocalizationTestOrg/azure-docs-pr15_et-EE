<properties 
    pageTitle="Andmete teisaldamine/Azure'i SQL-andmebaasi | Microsoft Azure'i" 
    description="Saate teada, kuidas andmeid Azure'i SQL-andmebaasi Azure'i andmed Factory abil teisaldada." 
    services="data-factory" 
    documentationCenter="" 
    authors="linda33wj" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/20/2016" 
    ms.author="jingwang"/>

# <a name="move-data-to-and-from-azure-sql-database-using-azure-data-factory"></a>Andmete teisaldamine ja sealt Azure'i SQL-andmebaasi Azure'i andmed Factory abil
See artikkel kirjeldab, kuidas saate Kopeeri tegevuse on Azure andmete factory teisaldamiseks teise andmesalve, et andmete Azure SQL-i andmebaasist. Selles artiklis põhineb [andmete liikumine tegevuste](data-factory-data-movement-activities.md) artikkel, mis annab ülevaate andmete liikumine Kopeeri aktiivsusest ja toetatud andmete poe kombinatsioone. 

## <a name="supported-sources-and-sinks"></a>Toetatud andmeallikate ja valamud
Andmete poed Toetatud andmeallikate või valamud Kopeeri tegevuse loendi vt [toetatud andmete poed](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tabelist. Saate liikuda andmeid mis tahes toetatud andmeallikate andmed poest Azure SQL-andmebaasiga või Azure'i SQL-andmebaasi mis tahes toetatud valamu andmesalve.

## <a name="create-pipeline"></a>Müügivõimaluste loomine
Saate luua müügivõimaluste Kopeeri tegevust, mis teisaldab erinevate tööriistad/API abil andmete Azure SQL-i andmebaasist.  

- Kopeerige viisard
- Azure'i portaal
- Visual Studio
- Azure'i PowerShelli
- .NET-I API-GA
- REST API-GA

Vaadake [Kopeeri tegevuse](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) üksikasjalike juhiste saamiseks loomine müügivõimaluste Kopeeri tegevuse erinevatel viisidel.   

## <a name="copy-data-wizard"></a>Kopeerige andmed viisard
Lihtsaim viis loomiseks kopeerib andmete Azure SQL-andmebaas on Kopeerige andmed viisardi abil. Vt [õpetus: müügivõimaluste koopia viisardi abil luua](data-factory-copy-data-wizard-tutorial.md) loomiseks Kopeerige andmed viisardi abil müügivõimaluste kiirülevaate selgituse. 

Järgmised näited annavad valimi JSON määratlused kasutatavad loomiseks [Azure portaali](data-factory-copy-activity-tutorial-using-azure-portal.md) või [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) või [Azure PowerShelli](data-factory-copy-activity-tutorial-using-powershell.md)abil. Andmete kopeerimine ja Azure SQL-andmebaas ja Azure'i bloobimälu kuvatakse. Siiski andmete võib olla kopeeritud **otse** mis tahes allikatest mis tahes valamud märgitud [siin](data-factory-data-movement-activities.md#supported-data-stores) Azure'i andmed Factory Kopeeri tegevuse abil.

## <a name="sample-copy-data-from-azure-sql-database-to-azure-blob"></a>Näide: Andmete kopeerimine Azure'i SQL-andmebaasi Azure'i bloobimälu

Sama määratleb andmete Factory järgmised üksused:

1. Lingitud teenuse tüüpi [AzureSqlDatabase](#azure-sql-linked-service-properties).
2. Lingitud teenuse tüüpi [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties). 
3. Mõne Sisestuskeel [andmekomplekti](data-factory-create-datasets.md) tüüpi [AzureSqlTable](#azure-sql-dataset-type-properties). 
4. Mõne väljundi [andmekomplekti](data-factory-create-datasets.md) tüüpi [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4. [Müügivõimaluste](data-factory-create-pipelines.md) Kopeeri tegevust, mis kasutab [SqlSource](#azure-sql-copy-activity-type-properties) ja [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

Valimi kopeerib aja andmesarja (iga päev, tund, jne) SQL Azure'i andmebaasi tabelisse lisamine bloobimälu iga tund. Nende proovide kasutatakse atribuutide JSON on kirjeldatud jaotiste jälgimise näidised.  

**Lingitud SQL Azure'i teenus**

    {
      "name": "AzureSqlLinkedService",
      "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
          "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
        }
      }
    }

Jaotisest [Azure SQL-i lingitud teenuse](#azure-sql-linked-service-properties) loendi atribuutide lingitud teenus ei toeta. 

**Azure'i bloobimälu lingitud salvestusteenus**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

[Azure'i bloobimälu](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties) saamiseks lugege artiklit loendi atribuutide lingitud teenus ei toeta. 

**Azure SQL-i Sisestuskeel andmekomplekti**

Valimi eeldab, et olete loonud tabeli "MyTable" Azure SQL-i ja see sisaldab veerg nimega "timestampcolumn" kellaajaandmete sarja. 

Säte "välise": "true" teatab Azure'i andmed Factory teenus, et andmekomplekti on väliste andmete factory ja ei esitata andmete factory toimingu.

    {
      "name": "AzureSqlInput",
      "properties": {
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties": {
          "tableName": "MyTable"
        },
        "external": true,
        "availability": {
          "frequency": "Hour",
          "interval": 1
        },
        "policy": {
          "externalData": {
            "retryInterval": "00:01:00",
            "retryTimeout": "00:10:00",
            "maximumRetry": 3
          }
        }
      }
    }

Jaotisest [Azure SQL-i andmekomplekti tüüp atribuutide](#azure-sql-dataset-type-properties) loendi atribuute, mis ei toeta seda tüüpi andmekomplekti.  

**Azure'i bloobimälu väljund andmekomplekti**

Andmed on kirjutatud uue bloobimälu tunnis (sagedus: tund, intervall: 1). Kausta tee on bloobimälu hinnatakse dünaamiliselt algusaja sellest osast, mis töötlemise põhjal. Kausta tee kasutab aasta, kuu, päev ja tunni osad algusaeg. 

    {
      "name": "AzureBlobOutput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}/",
          "partitionedBy": [
            {
              "name": "Year",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "yyyy"
              }
            },
            {
              "name": "Month",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "MM"
              }
            },
            {
              "name": "Day",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "dd"
              }
            },
            {
              "name": "Hour",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "HH"
              }
            }
          ],
          "format": {
            "type": "TextFormat",
            "columnDelimiter": "\t",
            "rowDelimiter": "\n"
          }
        },
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }

Jaotisest [Azure'i bloobimälu andmekomplekti tüüp atribuutide](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties) loendi atribuute, mis ei toeta seda tüüpi andmekomplekti.  

**Torujuhe Kopeeri tegevus**

Tulemas on Kopeeri tegevust, mis on konfigureeritud kasutama sisend- ja andmekomplektide ja on plaanitud käivituma tunnis. Tulemas JSON määratlus **Reaallika** tüüp väärtuseks **SqlSource** ja **valamu** tüüp väärtuseks **BlobSink**. SQL-päringu atribuudi **SqlReaderQuery** jaoks määratud valib andmete kopeerimiseks tunnis.

    {  
        "name":"SamplePipeline",
        "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline for copy activity",
        "activities":[  
          {
            "name": "AzureSQLtoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [
              {
                "name": "AzureSQLInput"
              }
            ],
            "outputs": [
              {
                "name": "AzureBlobOutput"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "SqlSource",
                "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
              },
              "sink": {
                "type": "BlobSink"
              }
            },
           "scheduler": {
              "frequency": "Hour",
              "interval": 1
            },
            "policy": {
              "concurrency": 1,
              "executionPriorityOrder": "OldestFirst",
              "retry": 0,
              "timeout": "01:00:00"
            }
          }
         ]
       }
    }

Näites kuvatakse SqlSource jaoks määratud **sqlReaderQuery** . Kopeeri tegevuse töötab see päring Azure'i SQL-andmebaasi allikas andmete toomiseks vastu. Teise võimalusena saate määrata salvestatud protseduur, määrates **sqlReaderStoredProcedureName** ja **storedProcedureParameters** (kui see on salvestatud protseduur võtab parameetrid).

Kui teil määrata, kas sqlReaderQuery või sqlReaderStoredProcedureName, kasutatakse andmekomplekti JSON struktuuri osa määratletud veerud käivitamiseks Azure SQL-i andmebaasist päringu koostamine. Näide: `select column1, column2 from mytable`. Kui andmekomplekti määratlus ei ole struktuuri, kõik veerud on valitud tabel. 


Vaadake teemat [SQL-i allika](#sqlsource) jaotis ja [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) SqlSource ja BlobSink ei toeta atribuutide loendi. 


## <a name="sample-copy-data-from-azure-blob-to-azure-sql-database"></a>Näide: Andmete kopeerimine Azure'i bloobimälu Azure SQL-andmebaas

Valimi määratleb andmete Factory järgmised üksused:  

1.  Lingitud teenuse tüüpi [AzureSqlDatabase](data-factory-azure-sql-connector.md#azure-sql-linked-service-properties).
2.  Lingitud teenuse tüüpi [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.  Mõne Sisestuskeel [andmekomplekti](data-factory-create-datasets.md) tüüpi [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4.  Mõne väljundi [andmekomplekti](data-factory-create-datasets.md) tüüpi [AzureSqlTable](data-factory-azure-sql-connector.md#azure-sql-dataset-type-properties).
4.  [Müügivõimaluste](data-factory-create-pipelines.md) Kopeeri tegevust, mis kasutab [BlobSource](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) ja [SqlSink](data-factory-azure-sql-connector.md#azure-sql-copy-activity-type-properties).

Valimi eksemplarid aeg-sarja andmete (tunni, päeva, jne), Azure Bloobivahemälu tabelisse Azure SQL-i andmebaasi tunnis. Nende proovide kasutatakse atribuutide JSON on kirjeldatud jaotiste jälgimise näidised. 


**Lingitud SQL Azure'i teenus**
    
    {
      "name": "AzureSqlLinkedService",
      "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
          "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
        }
      }
    }

Jaotisest [Azure SQL-i lingitud teenuse](#azure-sql-linked-service-properties) loendi atribuutide lingitud teenus ei toeta. 

**Azure'i bloobimälu lingitud salvestusteenus**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

[Azure'i bloobimälu](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties) saamiseks lugege artiklit loendi atribuutide lingitud teenus ei toeta.

**Azure'i bloobimälu Sisestuskeel andmekomplekti**

Andmed on te soovite uue bloobimälu tunnis (sagedus: tund, intervall: 1). Kausta tee ja faili nimi soovitud bloobimälu dünaamiliselt hinnatakse sellest osast, mis on töödeldud algusaeg. Kausta tee kasutab aasta, kuu ja päeva osa algusaeg ja faili nimi kasutab tund osa algusaeg. "välise": "true" säte teatab andmete Factory teenus, et selles tabelis on väliste andmete factory ja ei esitata andmete factory toimingu.

    {
      "name": "AzureBlobInput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/",
          "fileName": "{Hour}.csv",
          "partitionedBy": [
            {
              "name": "Year",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "yyyy"
              }
            },
            {
              "name": "Month",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "MM"
              }
            },
            {
              "name": "Day",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "dd"
              }
            },
            {
              "name": "Hour",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "HH"
              }
            }
          ],
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ",",
            "rowDelimiter": "\n"
          }
        },
        "external": true,
        "availability": {
          "frequency": "Hour",
          "interval": 1
        },
        "policy": {
          "externalData": {
            "retryInterval": "00:01:00",
            "retryTimeout": "00:10:00",
            "maximumRetry": 3
          }
        }
      }
    }

Jaotisest [Azure'i bloobimälu andmekomplekti tüüp atribuutide](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties) loendi atribuute, mis ei toeta seda tüüpi andmekomplekti.

**Azure SQL-i väljundi andmekomplekti**

Valimi kopeerib nimega "MyTable" SQL Azure'i tabeli andmeid. Tabel koos sama veergude arv Azure SQL-i luua bloobimälu CSV-fail sisaldab ootuspäraselt. Uued read lisatakse tabelisse tunnis. 

    {
      "name": "AzureSqlOutput",
      "properties": {
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties": {
          "tableName": "MyOutputTable"
        },
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }

Jaotisest [Azure SQL-i andmekomplekti tüüp atribuutide](#azure-sql-dataset-type-properties) loendi atribuute, mis ei toeta seda tüüpi andmekomplekti.

**Torujuhe Kopeeri tegevus**

Tulemas on Kopeeri tegevust, mis on konfigureeritud kasutama sisend- ja andmekomplektide ja on plaanitud käivituma tunnis. Tulemas JSON määratlus **Reaallika** tüüp väärtuseks **BlobSource** ja **valamu** tüüp väärtuseks **SqlSink**.

    {  
        "name":"SamplePipeline",
        "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline with copy activity",
        "activities":[  
          {
            "name": "AzureBlobtoSQL",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [
              {
                "name": "AzureBlobInput"
              }
            ],
            "outputs": [
              {
                "name": "AzureSqlOutput"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "BlobSource",
                "blobColumnSeparators": ","
              },
              "sink": {
                "type": "SqlSink"
              }
            },
           "scheduler": {
              "frequency": "Hour",
              "interval": 1
            },
            "policy": {
              "concurrency": 1,
              "executionPriorityOrder": "OldestFirst",
              "retry": 0,
              "timeout": "01:00:00"
            }
          }
          ]
       }
    }

Vaadake teemat [SQL-i valamu](#sqlsink) jaotis ja [BlobSource](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) SqlSink ja BlobSource ei toeta atribuutide loendi. 


## <a name="azure-sql-linked-service-properties"></a>Azure SQL-i lingitud teenuse atribuudid
Näidised, on lingitud teenuse tüüpi **AzureSqlDatabase** kasutatud andmete factory Azure SQL-andmebaasi linkimiseks. Järgmises tabelis on ära toodud JSON elementide teatud lingitud Azure SQL-i teenuse kirjeldus.

| Atribuut | Kirjeldus | Nõutav |
| -------- | ----------- | -------- |
| tüüp | Atribuudi tüüp väärtuseks peab olema seatud: **AzureSqlDatabase** | Jah |
| connectionString | Määrake Azure'i SQL-andmebaasi eksemplari connectionString atribuudi ühenduse loomiseks vajalik teave. | Jah |

> [AZURE.NOTE] Konfigureerida [Azure SQL-i andmebaasi tulemüüri](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) andmebaasi server [Azure'i teenuste serveri juurdepääsu](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure)lubamiseks. Lisaks, kui kopeerite andmeid Azure SQL-andmebaasiga väljaspool Azure sh kohapealse andmeallikast andmete factory Gateway, konfigureerimine vastav IP-aadresside vahemiku Azure'i SQL-andmebaasi andmete saatva seadme jaoks. 

## <a name="azure-sql-dataset-type-properties"></a>Azure SQL-i andmekomplekti tüüp atribuudid
Näidised, on kasutatud andmekomplekti tüüpi **AzureSqlTable** tähistada Azure SQL-andmebaasi tabelisse. 

Jaotised ja atribuute määratlemine andmekomplektide täieliku loendi leiate artiklist [loomine andmekomplektide](data-factory-create-datasets.md) . Jaotiste, nt struktuuri, kättesaadavus ja Andmekomplekt JSON poliitika on sarnased kõigi andmekomplekti tüüpide (Azure SQL, Azure'i bloobimälu, Azure'i tabeli jne). 

Jaotise typeProperties erineb iga tüüpi andmekomplekti ja andmesalve olevate andmete asukoha teave. Andmekomplekti **typeProperties** jaotises tüüp **AzureSqlTable** on järgmised atribuudid.

| Atribuut | Kirjeldus | Nõutav |
| -------- | ----------- | -------- |
| tableName | Azure'i SQL-andmebaasi eksemplari, mis viitab lingitud tabeli nimi. | Jah |

## <a name="azure-sql-copy-activity-type-properties"></a>Azure SQL-i kopeerimine tegevuse tüüp atribuudid
Jaotised ja atribuute määratlemine tegevuste täieliku loendi leiate artiklist [Torujuhtmete loomine](data-factory-create-pipelines.md) . Näiteks nimi, kirjeldus, sisendi ja väljundi tabelite ja poliitika atribuudid on saadaval kõigi tegevuse tüüp.

> [AZURE.NOTE] Kopeeri tegevuse võtab ainult üks sisendit ja annab ainult ühe väljundi.

Tegevuse **typeProperties** jaotise saadaolevad atribuudid erinevad teiselt iga tegevuse tüüp. Kopeeri tegevuste, need sõltuvad allikate ja neeldajate tüübid. 

Kui andmete teisaldamise Azure'i SQL-andmebaasiga seate Kopeeri tegevuse **SqlSource**andmeallika tüüp. Samamoodi kui andmete teisaldamine Azure SQL-andmebaasi saate seadistada valamu tüüp **SqlSink**tegevuse Kopeeri. Sellest jaotisest leiate loendi atribuudid SqlSource ja SqlSink ei toeta. 

### <a name="sqlsource"></a>SqlSource

Kopeeri tegevust, kui allika tüüp **SqlSource**, järgmised atribuudid on saadaval jaotises **typeProperties** :

| Atribuut | Kirjeldus | Lubatud väärtus | Nõutav |
| -------- | ----------- | -------------- | -------- |
| sqlReaderQuery | Kohandatud päringu abil saate andmeid lugeda. | SQL-i päringustringi. Näide: `select * from MyTable`.  | Ei |
| sqlReaderStoredProcedureName | Salvestatud protseduur, mis loeb andmed lähtetabeli nimi. | Salvestatud toimingu nime. | Ei |
| storedProcedureParameters | Salvestatud protseduur parameetrid. | Nime ja väärtuse paarideks. Nimede ja parameetrite kest peab vastama nimed ja kest salvestatud protseduur parameetrid. | Ei |

Kui soovitud SqlSource määratud **sqlReaderQuery** , töötab Kopeeri tegevuse selle päringu Azure'i SQL-andmebaasi allikas andmete toomiseks vastu. Teise võimalusena saate määrata salvestatud protseduur, määrates **sqlReaderStoredProcedureName** ja **storedProcedureParameters** (kui see on salvestatud protseduur võtab parameetrid). 

Kui saate määrata, kas sqlReaderQuery või sqlReaderStoredProcedureName, kasutatakse andmekomplekti JSON struktuuri osa määratletud veerud päringu koostamine (`select column1, column2 from mytable`) käivitamiseks Azure SQL-i andmebaasist. Kui andmekomplekti määratlus ei ole struktuuri, kõik veerud on valitud tabel. 

> [AZURE.NOTE] **SqlReaderStoredProcedureName**kasutamisel peate endiselt määramiseks väärtuse **tableName** atribuudi andmekomplekti JSON. On pole valideerimised küll vastu selle tabeli läbi. 

### <a name="sqlsource-example"></a>SqlSource näide

    "source": {
        "type": "SqlSource",
        "sqlReaderStoredProcedureName": "CopyTestSrcStoredProcedureWithParameters",
        "storedProcedureParameters": {
            "stringData": { "value": "str3" },
            "id": { "value": "$$Text.Format('{0:yyyy}', SliceStart)", "type": "Int"}
        }
    }

**Salvestatud protseduur määratlus:** 

    CREATE PROCEDURE CopyTestSrcStoredProcedureWithParameters
    (
        @stringData varchar(20),
        @id int
    )
    AS
    SET NOCOUNT ON;
    BEGIN
         select *
         from dbo.UnitTestSrcTable
         where dbo.UnitTestSrcTable.stringData != stringData
        and dbo.UnitTestSrcTable.id != id
    END
    GO


### <a name="sqlsink"></a>SqlSink 

**SqlSink** toetab järgmised atribuudid.

| Atribuut | Kirjeldus | Lubatud väärtus | Nõutav |
| -------- | ----------- | -------------- | -------- |
| writeBatchTimeout | Oodake paketi lisa toimingu lõpuleviimiseks enne ilmneb ajalõpp aega. | kuuline ajavahemik<br/><br/> Näide: "00: 30:00" (30 minuti). | Ei | 
| writeBatchSize | Lisab andmed tabelisse SQL-i puhvri suurus writeBatchSize jõudmisel. | Täisarv (ridade arv)| Ei (vaikimisi: 10000)
| sqlWriterCleanupScript | Määrake päringu kopeerimine tegevuste käivitada nii, et teatud sektorit andmed on kustutatud. Lisateavet leiate jaotisest [korratavuse](#repeatability-during-copy). | Päringu märge.  | Ei |
| sliceIdentifierColumnName | Määrake Kopeeri tegevuste täitmiseks identifikaatoriga automaatselt loodud sektorit, mida kasutatakse puhastamiseks mõne kindla sektorit uuesti, kui andmed veeru nimi. Lisateavet leiate jaotisest [korratavuse](#repeatability-during-copy). | Veeru andmete tüüpi binary(32) veeru nimi. | Ei |
| sqlWriterStoredProcedureName | Salvestatud toimingu nime upserts (värskenduste/lisab) andmed sihttabelisse sisse. | Salvestatud toimingu nime. | Ei |
| storedProcedureParameters | Salvestatud protseduur parameetrid. | Nime ja väärtuse paarideks. Nimede ja parameetrite kest peab vastama nimed ja kest salvestatud protseduur parameetrid. | Ei | 
| sqlWriterTableType | Määrake kasutada salvestatud toimingu tüüp tabeli nime. Kopeeri tegevuse teeb andmed ei teisaldataks temp tabelis sellise tabeli saadaval. Salvestatud protseduur kood seejärel saate ühendada andmed kopeeritakse olemasolevate andmetega. | Tippige tabeli nimi. | Ei |

#### <a name="sqlsink-example"></a>SqlSink näide

    "sink": {
        "type": "SqlSink",
        "writeBatchSize": 1000000,
        "writeBatchTimeout": "00:05:00",
        "sqlWriterStoredProcedureName": "CopyTestStoredProcedureWithParameters",
        "sqlWriterTableType": "CopyTestTableType",
        "storedProcedureParameters": {
            "id": { "value": "1", "type": "Int" },
            "stringData": { "value": "str1" },
            "decimalData": { "value": "1", "type": "Decimal" }
        }
    }

## <a name="identity-columns-in-the-target-database"></a>Identiteedi sihtandmebaas veerud
Sellest jaotisest leiate näiteks lähtetabeliga ilma identiteedi veeru andmete kopeerimiseks sihtkoha tabel koos identiteedi veeru. 

**Lähtetabel:** 

    create table dbo.SourceTbl
    (
           name varchar(100),
           age int
    )

**Sihttabel:**

    create table dbo.TargetTbl
    (
           id int identity(1,1),
           name varchar(100),
           age int
    )


Pange tähele, et sihttabeli on identiteedi veeru. 

**Andmeallika andmekomplekti JSON määratlus**

    {
        "name": "SampleSource",
        "properties": {
            "type": " SqlServerTable",
            "linkedServiceName": "TestIdentitySQL",
            "typeProperties": {
                "tableName": "SourceTbl"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true,
            "policy": {}
        }
    }

**Sihtkoha andmekomplekti JSON määratlus**

    {
        "name": "SampleTarget",
        "properties": {
            "structure": [
                { "name": "name" },
                { "name": "age" }
            ],
            "type": "AzureSqlTable",
            "linkedServiceName": "TestIdentitySQLSource",
            "typeProperties": {
                "tableName": "TargetTbl"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": false,
            "policy": {}
        }   
    }


Teade, et teie lähte- ja tabelina on erinevate skeemi (sihtkoht on täiendav veerg identiteediga). Selle stsenaariumi korral peate määrata **struktuuri** atribuut target andmekomplekti määratlus, mis ei sisalda identiteedi veeru. 

Seejärel saate Vastendage veerud: allika andmekomplekti sihtkoha andmehulga veerud. Vt [veeru vastendamise näidised](#column-mapping-samples) sektsiooni näide. 

[AZURE.INCLUDE [data-factory-type-repeatability-for-sql-sources](../../includes/data-factory-type-repeatability-for-sql-sources.md)] 

[AZURE.INCLUDE [data-factory-sql-invoke-stored-procedure](../../includes/data-factory-sql-invoke-stored-procedure.md)]
 

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

### <a name="type-mapping-for-sql-server--azure-sql-database"></a>Tippige SQL serveri & Azure'i SQL-andmebaasi kaardistamine

Nimetatud [andmete liikumine tegevuste](data-factory-data-movement-activities.md) artiklis Kopeeri tegevuse sooritab automaatne tüüpide dokumenditeisenduste allikatüüpide valamu tüüpi järgmised 2-samm lähenemisviisi kaudu.

1. .Net-i tüüp teisendada kohalikke andmeallika tüübiga.
2. Teisendada .NET kohalikke valamu tüüp

Kui andmete teisaldamise ja Azure SQL-i, SQL serveri Sybase'i järgmised vastendused kasutatakse SQL-i tüüpi .net-i tüüp ja vastupidi.

Kaardistamine on sama SQL serveri andmetega tüüp vastenduse ADO.net-i.

| SQL serveri andmebaasi mootor tüüp | .NET framework tüüp |
| ------------------------------- | ------------------- |
| suur täisarv | Int64 |
| binaarsed | Byte] |
| bit | Kahendmuutuja |
| char | Stringi Char] |
| kuupäev | Kuupäev ja kellaaeg |
| Kuupäev ja kellaaeg | Kuupäev ja kellaaeg |
| datetime2 | Kuupäev ja kellaaeg |
| Datetimeoffset | DateTimeOffset |
| Decimal | Decimal |
| Atribuut FILESTREAM (varbinary(max)) | Byte] |
| Hõljumine | Kahekordne |
| pilt | Byte] | 
| int | Int32 | 
| raha | Decimal |
| NChar | Stringi Char] |
| ntext | Stringi Char] |
| arvuline | Decimal |
| nvarchar | Stringi Char] |
| reaal | Ühe |
| ROWVERSION | Byte] |
| smalldatetime | Kuupäev ja kellaaeg |
| smallint | Int16 |
| smallmoney | Decimal | 
| sql_variant | Objekti * |
| teksti | Stringi Char] |
| aeg | Kuuline ajavahemik |
| ajatempli | Byte] |
| tinyint | Bait |
| ainuidentifikaatori | GUID |
| varbinaar |  Byte] |
| varchar | Stringi Char] |
| XML-i | XML-i |


[AZURE.INCLUDE [data-factory-type-conversion-sample](../../includes/data-factory-type-conversion-sample.md)]

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

## <a name="performance-and-tuning"></a>Jõudluse ja häälestamine  
Vaata [Kopeeri tegevuse jõudlus ja häälestamine juhend](data-factory-copy-activity-performance.md) Lisateavet peamised tegurid selle mõju jõudlust andmete liikumine (Kopeeri tegevus) Azure'i andmed Factory ja optimeerida seda mitmel viisil.