<properties 
    pageTitle="Andmete teisaldamine/Oracle'i abil andmete Factory | Microsoft Azure'i" 
    description="Saate teada, kuidas andmeid Oracle'i andmebaasiga, mis on kohapealse Azure'i andmed Factory abil teisaldada." 
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

# <a name="move-data-tofrom-on-premises-oracle-using-azure-data-factory"></a>Andmete teisaldamine ja neid sealt kohapealse Oracle'i Azure'i andmed Factory abil 

See artikkel kirjeldab, kuidas saate kasutada andmete factory Kopeeri tegevus andmete teisaldamiseks Oracle'i teise andmeid, et salvestada ja sealt. Selles artiklis põhineb [andmete liikumine tegevuste](data-factory-data-movement-activities.md) artikkel, mis annab ülevaate andmete liikumine Kopeeri tegevuste ja toetatud andmete poe kombinatsioone.

## <a name="installation"></a>Installimine 
Teenuse Azure'i andmed Factory saaksid oma kohapealse Oracle'i andmebaasiga ühenduse loomiseks peate installima järgmised komponendid: 

- Andmehalduslüüsi samasse arvutisse, mis hostib andmebaasi või eraldi arvutisse omavahelise ressursid andmebaasi. Andmehalduslüüs on kliendi agent, mis ühendab kohapealse andmeallikate pilveteenused turvalise ja hallatavate. Vt artikli [andmete kohapealse ja pilveteenuse vahel liikumine](data-factory-move-data-between-onprem-and-cloud.md) Andmehalduslüüsi üksikasjad. 
- Oracle'i andmepakkuja .net-i jaoks. See komponent sisaldub [Oracle'i andmed Accessi komponendid for Windows](http://www.oracle.com/technetwork/topics/dotnet/downloads/). Installige host arvutisse, kuhu on installitud lüüsi sobiv versioon (32/64-bitine). [Oracle'i andmete pakkuja .NET 12.1](http://docs.oracle.com/database/121/ODPNT/InstallSystemRequirements.htm#ODPNT149) pääsevad Oracle'i andmebaasiga 10 g 2 või uuem väljaanne.

    Kui valite "XCopy installimise", järgige selle readme.htm. Soovitame teil valida Installeri abil UI (mitte-XCopy üks). 
 
    Pärast pakkuja installimist taaskäivitage andmete haldamise lüüsi hostiteenus teie arvutis teenuste applet (või) andmete Andmehalduslüüsi Konfigureerimishalduri abil.  

> [AZURE.NOTE] Lugege teemat [tõrkeotsing lüüsi probleemide](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) näpunäiteid tõrkeotsingu ühenduse/lüüsi seotud probleemid. 

## <a name="copy-data-wizard"></a>Kopeerige andmed viisard
Lihtsaim viis loomiseks kopeerib toetatud valamu andmete poe Oracle'i andmebaasiga, et andmed on kasutada Kopeerige andmed viisardi. Vt [õpetus: müügivõimaluste koopia viisardi abil luua](data-factory-copy-data-wizard-tutorial.md) loomiseks Kopeerige andmed viisardi abil müügivõimaluste kiirülevaate selgituse. 

Järgmises näites sisaldab valimi JSON määratlused kasutatavad loomiseks [Azure portaali](data-factory-copy-activity-tutorial-using-azure-portal.md) või [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) või [Azure PowerShelli](data-factory-copy-activity-tutorial-using-powershell.md)abil. Sektordiagrammis kuvatakse Oracle'i andmebaasiga Azure'i bloobimälu ja sealt, et andmete kopeerimine. Siiski saate andmeid kopeerida kõiki valamud märgitud [siin](data-factory-data-movement-activities.md#supported-data-stores) Azure'i andmed Factory Kopeeri tegevuse abil.   

## <a name="sample-copy-data-from-oracle-to-azure-blob"></a>Näide: Andmete kopeerimine Oracle'i Azure'i bloobimälu
See näide näitab, kuidas kohapealse Oracle'i andmebaasiga andmete kopeerimine mõnda Azure'i bloobimälu. Andmete saate siiski kopeeritud **otse** mis tahes valamud märgitud [siin](data-factory-data-movement-activities.md#supported-data-stores) Azure'i andmed Factory Kopeeri tegevuse abil.  
 
Valimi on andmete factory järgmised üksused:

1.  Lingitud teenuse tüüpi [OnPremisesOracle](data-factory-onprem-oracle-connector.md#oracle-linked-service-properties).
2.  Lingitud teenuse tüüpi [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.  Mõne Sisestuskeel [andmekomplekti](data-factory-create-datasets.md) tüüpi [OracleTable](data-factory-onprem-oracle-connector.md#oracle-dataset-type-properties). 
4.  Mõne väljundi [andmekomplekti](data-factory-create-datasets.md) tüüpi [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
5.  [Müügivõimaluste](data-factory-create-pipelines.md) Kopeeri tegevust, mis kasutab [OracleSource](data-factory-onprem-oracle-connector.md#oracle-copy-activity-type-properties) allikas ja [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) valamu.

Valimi kopeerib andmete kohapealse Oracle'i andmebaasiga tabelist soovitud bloobimälu tunnis. Lisateavet erinevate valimi kasutatavad atribuudid, dokumentatsioonist jälgimise näidised jaotised.

**Oracle'i lingitud teenus:**

    {
      "name": "OnPremisesOracleLinkedService",
      "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
          "ConnectionString": "data source=<data source>;User Id=<User Id>;Password=<Password>;",
          "gatewayName": "<gateway name>"
        }
      }
    }

**Azure'i bloobimälu lingitud teenuse:**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<Account key>"
        }
      }
    }

**Oracle'i Sisestuskeel andmekomplekti:**

Valimi eeldab, et olete loonud tabeli "MyTable" Oracle'i ja see sisaldab veerg nimega "timestampcolumn" kellaajaandmete sarja. 

Säte "välise": "true" teatab andmete Factory teenus, et andmekomplekti on väliste andmete factory ja ei esitata andmete factory toimingu.

    {
        "name": "OracleInput",
        "properties": {
            "type": "OracleTable",
            "linkedServiceName": "OnPremisesOracleLinkedService",
            "typeProperties": {
                "tableName": "MyTable"
            },
               "external": true,
            "availability": {
                "offset": "01:00:00",
                "interval": "1",
                "anchorDateTime": "2014-02-27T12:00:00",
                "frequency": "Hour"
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


**Azure'i bloobimälu väljund andmekomplekti:**

Andmed on kirjutatud uue bloobimälu tunnis (sagedus: tund, intervall: 1). Kausta tee ja faili nimi soovitud bloobimälu dünaamiliselt hinnatakse sellest osast, mis on töödeldud algusaeg. Kausta tee kasutab aasta, kuu, päev ja tunni osad algusaeg.
    
    {
      "name": "AzureBlobOutput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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


**Torujuhe Kopeeri tegevuse abil:**

Tulemas on Kopeeri tegevust, mis on konfigureeritud kasutama sisend- ja andmekomplektide ja on plaanitud käivituma tunnis. Tulemas JSON määratlus **Reaallika** tüüp väärtuseks **OracleSource** ja **valamu** tüüp väärtuseks **BlobSink**.  SQL-päringu määratud **oracleReaderQuery** atribuudiga valib andmete kopeerimiseks tunnis.

    
    {  
        "name":"SamplePipeline",
        "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline for copy activity",
        "activities":[  
          {
            "name": "OracletoBlob",
            "description": "copy activity",
            "type": "Copy",
            "inputs": [
              {
                "name": " OracleInput"
              }
            ],
            "outputs": [
              {
                "name": "AzureBlobOutput"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "OracleSource",
                "oracleReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
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


Soovite reguleerida päringustringi, kuidas kuupäevad on konfigureeritud Oracle'i andmebaasi põhjal. Kui kuvatakse järgmine tõrketeade: 

    Message=Operation failed in Oracle Database with the following error: 'ORA-01861: literal does not match format string'.,Source=,''Type=Oracle.DataAccess.Client.OracleException,Message=ORA-01861: literal does not match format string,Source=Oracle Data Provider for .NET,'.

võimalik, et peate päringu muutmine, nagu on näidatud järgmises näites (kasutades funktsiooni to_date):

    "oracleReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= to_date(\\'{0:MM-dd-yyyy HH:mm}\\',\\'MM/DD/YYYY HH24:MI\\')  AND timestampcolumn < to_date(\\'{1:MM-dd-yyyy HH:mm}\\',\\'MM/DD/YYYY HH24:MI\\') ', WindowStart, WindowEnd)"

## <a name="sample-copy-data-from-azure-blob-to-oracle"></a>Näide: Andmete kopeerimine Azure'i bloobimälu Oracle'i
See näide näitab, kuidas andmeid kopeerida mõne Azure'i bloobimälu kohapealse Oracle'i andmebaasiga. Andmeid saate siiski kopeeritud **otse** mis tahes allikatest märgitud [siin](data-factory-data-movement-activities.md#supported-data-stores) Azure'i andmed Factory Kopeeri tegevuse kasutamine.  
 
Valimi on andmete factory järgmised üksused:

1.  Lingitud teenuse tüüpi [OnPremisesOracle](data-factory-onprem-oracle-connector.md#oracle-linked-service-properties).
2.  Lingitud teenuse tüüpi [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.  Mõne Sisestuskeel [andmekomplekti](data-factory-create-datasets.md) tüüpi [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4.  Mõne väljundi [andmekomplekti](data-factory-create-datasets.md) tüüpi [OracleTable](data-factory-onprem-oracle-connector.md#oracle-dataset-type-properties). 
5.  [Müügivõimaluste](data-factory-create-pipelines.md) Kopeeri tegevust, mis kasutab [BlobSource](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) nagu andmeallika [OracleSink](data-factory-onprem-oracle-connector.md#oracle-copy-activity-type-properties) nimega valamu.

Valimi kopeerib andmete lisamine bloobimälu kohapealse Oracle'i andmebaasi tabelisse tunnis. Lisateavet erinevate valimi kasutatavad atribuudid, dokumentatsioonist jälgimise näidised jaotised.

**Oracle'i lingitud teenus:**

    {
      "name": "OnPremisesOracleLinkedService",
      "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
          "ConnectionString": "data source=<data source>;User Id=<User Id>;Password=<Password>;",
          "gatewayName": "<gateway name>"
        }
      }
    }

**Azure'i bloobimälu lingitud teenuse:**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<Account key>"
        }
      }
    }

**Azure'i bloobimälu Sisestuskeel andmekomplekti**

Andmed on te soovite uue bloobimälu tunnis (sagedus: tund, intervall: 1). Kausta tee ja faili nimi soovitud bloobimälu dünaamiliselt hinnatakse sellest osast, mis on töödeldud algusaeg. Kausta tee kasutab aasta, kuu ja päeva osa algusaeg ja faili nimi kasutab tund osa algusaeg. "välise": "true" säte teatab andmete Factory teenus, et selles tabelis on väliste andmete factory ja ei esitata andmete factory toimingu.

    {
      "name": "AzureBlobInput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}",
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

**Oracle'i väljundi andmekomplekti:**

Valimi eeldab, et olete loonud Oracle'i tabeli "MyTable". Tabel koos sama veergude arv Oracle'i luua bloobimälu CSV-fail sisaldab ootuspäraselt. Uued read lisatakse tabelisse tunnis.

    {
        "name": "OracleOutput",
        "properties": {
            "type": "OracleTable",
            "linkedServiceName": "OnPremisesOracleLinkedService",
            "typeProperties": {
                "tableName": "MyTable"
            },
            "availability": {
                "frequency": "Hour",
                "interval": "1"
            }
        }
    }


**Torujuhe Kopeeri tegevuse abil:**

Tulemas on Kopeeri tegevust, mis on konfigureeritud kasutama sisend- ja andmekomplektide ja on plaanitud käivituma tunnis. Tulemas JSON määratlus **Reaallika** tüüp väärtuseks **BlobSource** ja **valamu** tüüp väärtuseks **OracleSink**.  

    
    {  
        "name":"SamplePipeline",
        "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline with copy activity",
        "activities":[  
          {
            "name": "AzureBlobtoOracle",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [
              {
                "name": "AzureBlobInput"
              }
            ],
            "outputs": [
              {
                "name": "OracleOutput"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "BlobSource"
              },
              "sink": {
                "type": "OracleSink"
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


## <a name="oracle-linked-service-properties"></a>Oracle'i lingitud teenuse atribuudid

Järgmises tabelis on ära toodud JSON elementide teatud Oracle lingitud teenuse kirjeldus. 

Atribuut | Kirjeldus | Nõutav
-------- | ----------- | --------
tüüp | Atribuudi tüüp väärtuseks peab olema seatud: **OnPremisesOracle** | Jah
connectionString | Määrake atribuudi connectionString eksemplari Oracle'i andmebaasiga ühenduse loomiseks vajalik teave. | Jah 
gatewayName | Lüüsi nimi, mida kasutatakse ühenduse loomiseks kohapealse Oracle'i server | Jah

Üksikasjad kohapealse Oracle'i andmeallika mandaati määramise kohta leiate [säte mandaadid ja turvalisus](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) .
## <a name="oracle-dataset-type-properties"></a>Oracle'i andmekomplekti tüüp atribuudid

Jaotised ja atribuute määratlemine andmekomplektide täieliku loendi leiate artiklist [loomine andmekomplektide](data-factory-create-datasets.md) . Jaotiste, nt struktuuri, kättesaadavus ja Andmekomplekt JSON poliitika on sarnased kõigi andmekomplekti tüüpide (Oracle, Azure'i bloobimälu, Azure'i tabeli jne).
 
Jaotise typeProperties erineb iga tüüpi andmekomplekti ja andmesalve olevate andmete asukoha teave. Andmekomplekti typeProperties jaotises Tüüp OracleTable on järgmised atribuudid.

Atribuut | Kirjeldus | Nõutav
-------- | ----------- | --------
tableName | Oracle'i andmebaas, mis viitab teenuse lingitud tabeli nimi. | Ei, (kui on määratud **oracleReaderQuery** **OracleSource** .)

## <a name="oracle-copy-activity-type-properties"></a>Oracle'i Kopeeri tegevuse tüüp atribuudid

Jaotised ja atribuudid, mis on saadaval tegevuste määratlemiseks täieliku loendi leiate artiklist [Torujuhtmete loomine](data-factory-create-pipelines.md) . Näiteks nimi, kirjeldus, sisestus- ja väljundi tabelite ja poliitika atribuudid on saadaval kõigi tegevuse tüüp. 

> [AZURE.NOTE]
Kopeeri tegevuse võtab ainult üks sisendit ja annab ainult ühe väljundi.

Tegevuse typeProperties jaotise saadaolevad atribuudid erinevad teiselt iga tegevuse tüüp. Kopeeri tegevuste, need sõltuvad allikate ja neeldajate tüübid.

### <a name="oraclesource"></a>OracleSource
Kopeeri tegevust, kui allika tüüp **OracleSource** järgmised atribuudid on saadaval jaotises **typeProperties** :

Atribuut | Kirjeldus |Lubatud väärtus | Nõutav
-------- | ----------- | ------------- | --------
oracleReaderQuery | Kohandatud päringu abil saate andmeid lugeda. | SQL-i päringustringi. Näide: valige *kaudu MyTable <br/> <br/>kui pole määratud, käivitatakse SQL-lause: valige* MyTable kaudu | Ei, (kui **tableName** **andmekogumi** on määratud)

### <a name="oraclesink"></a>OracleSink
**OracleSink** toetab järgmised atribuudid.

Atribuut | Kirjeldus | Lubatud väärtus | Nõutav
-------- | ----------- | -------------- | --------
writeBatchTimeout | Oodake paketi lisa toimingu lõpuleviimiseks enne ilmneb ajalõpp aega. | kuuline ajavahemik<br/><br/> Näide: 00:30:00 (30 minuti). | Ei
writeBatchSize | Lisab andmed tabelisse SQL-i puhvri suurus writeBatchSize jõudmisel.   | Täisarv (ridade arv)| Ei (vaikimisi: 10000)  
sqlWriterCleanupScript | Määrake päringu kopeerimine tegevuste käivitada nii, et teatud sektorit andmed on kustutatud. | Päringu märge. | Ei
sliceIdentifierColumnName | Määrake veeru nimi Kopeeri tegevuste täitmiseks identifikaatoriga automaatselt loodud sektorit, mida kasutatakse puhastamiseks mõne kindla sektorit uuesti, kui andmed. | Veeru andmete tüüpi binary(32) veeru nimi. | Ei


[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

### <a name="type-mapping-for-oracle"></a>Tippige vastenduse Oracle

Nimetatud [andmete liikumine tegevuste](data-factory-data-movement-activities.md) artiklis Kopeeri tegevuse sooritab automaatne tüüpide dokumenditeisenduste allikatüüpide valamu tüüpi järgmised 2-samm lähenemisviisi kaudu.

1. .Net-i tüüp teisendada kohalikke andmeallika tüübiga.
2. Teisendada .NET kohalikke valamu tüüp

Kui andmete teisaldamise Oracle'i kasutatakse järgmised vastendused Oracle'i andmete tüübist .net-i tüüp ja vastupidi.

Oracle'i andmetüüp | .NET framework andmetüüp
---------------- | ------------------------
BFILE | Byte]
BLOOBIMÄLU | Byte]
CHAR | String
CLOB | String
KUUPÄEV | Kuupäev ja kellaaeg
HÕLJUMINE | Decimal
TÄISARV | Decimal
INTERVALL AASTA, KUU | Int32
TEINE INTERVALL PÄEVA | Kuuline ajavahemik
PIKK | String
PIKK TÖÖTLEMATA | Byte]
NCHAR | String
NCLOB | String
ARV | Decimal
NVARCHAR2 | String
TÖÖTLEMATA | Byte]
RÕHTREA-ID | String
AJATEMPLI | Kuupäev ja kellaaeg
AJATEMPLI KOOS AJAVÖÖND | Kuupäev ja kellaaeg
AJATEMPLI KOOS AJAVÖÖND | Kuupäev ja kellaaeg
ALLKIRJASTAMATA TÄISARV | Arv
VARCHAR2 | String
XML-I | String

## <a name="troubleshooting-tips"></a>Tõrkeotsingu näpunäited

**Probleem:** Kuvatakse järgmine **tõrketeade**: Kopeeri tegevuse täidetud Lubamatute parameetrite: UnknownParameterName, üksikasjalikud sõnumi: ei leia nõutav .net raamistiku andmepakkuja. See pole installitud".  

**Võimalikud põhjused:**

1. .NET raamistiku andmepakkuja Oracle'i ei installitud.
2. .NET raamistiku andmepakkuja Oracle'i .NET Framework 2.0 installiti ja .NET Framework 4.0 kaustadesse ei leita. 

**Eraldusvõime/lahendus:**

1. Kui teil pole installitud .net-i pakkuja Oracle, [installige see](http://www.oracle.com/technetwork/topics/dotnet/downloads/) ja proovige seda stsenaariumi. 
2. Kui kuvatakse tõrketeade kuvatakse ka pärast installimist pakkuja, tehke järgmist. 
    1. Avage arvuti config .NET 2.0 kaustast: <system disk>: \Windows\Microsoft.NET\Framework64\v2.0.50727\CONFIG\machine.config.
    2. Otsingu **Oracle'i andmepakkuja .net-i**ja teil peab olema leida kirje, nagu on näidatud Järgmine näidis unwn järgmistes Kuulake under **system.data** -> **DbProviderFactories**:
            “<add name="Oracle Data Provider for .NET" invariant="Oracle.DataAccess.Client" description="Oracle Data Provider for .NET" type="Oracle.DataAccess.Client.OracleClientFactory, Oracle.DataAccess, Version=2.112.3.0, Culture=neutral, PublicKeyToken=89b483f429c47342" />”
2.  Kirje kopeerimiseks machine.config fail v4.0 järgmises kaustas: <system disk>: \Windows\Microsoft.NET\Framework64\v4.0.30319\Config\machine.config ja muutke 4.xxx.x.x versioon.
3.  Installida "< ODP.NET installitud tee > \11.2.0\client_1\odp.net\bin\4\Oracle.DataAccess.dll" globaalne komplekti vahemälu (GAC) käivitate `gacutil /i [provider path]`.



[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]


## <a name="performance-and-tuning"></a>Jõudluse ja häälestamine  
Vaata [Kopeeri tegevuse jõudlus ja häälestamine juhend](data-factory-copy-activity-performance.md) Lisateavet peamised tegurid selle mõju jõudlust andmete liikumine (Kopeeri tegevus) Azure'i andmed Factory ja optimeerida seda mitmel viisil.
