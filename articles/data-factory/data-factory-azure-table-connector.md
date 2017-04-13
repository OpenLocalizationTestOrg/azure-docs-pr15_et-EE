<properties 
    pageTitle="Andmete teisaldamine/Azure'i tabeli põhjal | Microsoft Azure'i" 
    description="Saate teada, kuidas andmeid Azure'i Tabelimälu Azure'i andmed Factory abil teisaldada." 
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
    ms.date="09/13/2016" 
    ms.author="jingwang"/>

# <a name="move-data-to-and-from-azure-table-using-azure-data-factory"></a>Andmete teisaldamine ja sealt Azure'i tabeli Azure'i andmed Factory abil

See artikkel kirjeldab, kuidas saate Kopeeri tegevuse on Azure andmete factory andmeid Azure'i tabelist teise andmesalve, et liikuda. Selles artiklis põhineb [andmete liikumine tegevuste](data-factory-data-movement-activities.md) artikkel, mis tutvustab andmete liikumine ja toetatud andmete poe kombinatsioonide ülevaade Kopeeri tegevuse.

## <a name="copy-data-wizard"></a>Kopeerige andmed viisard
Lihtsaim viis loomiseks kopeerib andmete Azure'i Tabelimälu on Kopeerige andmed viisardi abil. Vt [õpetus: müügivõimaluste koopia viisardi abil luua](data-factory-copy-data-wizard-tutorial.md) loomiseks Kopeerige andmed viisardi abil müügivõimaluste kiirülevaate selgituse. 

Järgmised näited annavad valimi JSON määratlused kasutatavad loomiseks [Azure portaali](data-factory-copy-activity-tutorial-using-azure-portal.md) või [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) või [Azure PowerShelli](data-factory-copy-activity-tutorial-using-powershell.md)abil. Sektordiagrammis kuvatakse andmete kopeerimine ja Azure Tabelimäluga ja Azure'i bloobimälu andmebaasi. Siiski andmete võib olla kopeeritud **otse** mis tahes allikatest mis tahes valamud märgitud [siin](data-factory-data-movement-activities.md#supported-data-stores) Azure'i andmed Factory Kopeeri tegevuse abil.

## <a name="sample-copy-data-from-azure-table-to-azure-blob"></a>Näide: Andmete kopeerimine Azure'i tabeli põhjal Azure'i bloobimälu

Järgmises näites on kuvatud.

1.  Lingitud teenuse tüüpi [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties) (kasutatakse bloobimälu & tabeli puhul).
2.  Mõne Sisestuskeel [andmekomplekti](data-factory-create-datasets.md) tüüpi [Azure'i](#azure-table-dataset-type-properties).
3.  Mõne väljundi [andmekomplekti](data-factory-create-datasets.md) tüüpi [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties). 
3.  [Müügivõimaluste](data-factory-create-pipelines.md) Kopeeri tegevust, mis kasutab [AzureTableSource](#azure-table-copy-activity-type-properties) ja [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties). 

Valimi kopeerib andmete kuuluvad vaikimisi sektsiooni Azure'i tabeli lisamine bloobimälu abil tunnis. Nende proovide kasutatakse atribuutide JSON on kirjeldatud jaotiste jälgimise näidised.

**Azure'i lingitud salvestusteenus:**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

Azure'i andmed Factory toetab kahte tüüpi lingitud Azure Storage teenused: **AzureStorage** ja **AzureStorageSas**. Esimene, saate määrata ühendusstring, mis sisaldab konto võti ja hiljem üks määrate Uri ühiskasutusse Accessi allkirja (SAS). Üksikasju vt [Lingitud teenuste](#linked-services) jaotises.  

**Azure'i tabeli Sisestuskeel andmekomplekti:**

Valimi eeldab, et olete loonud tabeli "MyTable" Azure'i tabeli.
 
Säte "välise": "true" teatab andmete Factory teenus, et andmekomplekti on väliste andmete factory ja ei esitata andmete factory toimingu.

    {
      "name": "AzureTableInput",
      "properties": {
        "type": "AzureTable",
        "linkedServiceName": "StorageLinkedService",
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

**Azure'i bloobimälu väljund andmekomplekti:**

Andmed on kirjutatud uue bloobimälu tunnis (sagedus: tund, intervall: 1). Kausta tee on bloobimälu hinnatakse dünaamiliselt algusaja sellest osast, mis töötlemise põhjal. Kausta tee kasutab aasta, kuu, päev ja tunni osad algusaeg. 

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

**Torujuhe Kopeeri tegevusega:**

Tulemas on Kopeeri tegevust, mis on konfigureeritud kasutama sisend- ja andmekomplektide ja on plaanitud käivituma tunnis. Tulemas JSON määratlus **Reaallika** tüüp väärtuseks **AzureTableSource** ja **valamu** tüüp väärtuseks **BlobSink**. SQL-päringu määratud **AzureTableSourceQuery** atribuudiga klõpsab andmed vaikimisi sektsiooni tunnis kopeerida.

    {  
        "name":"SamplePipeline",
        "properties":{  
            "start":"2014-06-01T18:00:00",
            "end":"2014-06-01T19:00:00",
            "description":"pipeline for copy activity",
            "activities":[  
                {
                    "name": "AzureTabletoBlob",
                    "description": "copy activity",
                    "type": "Copy",
                    "inputs": [
                        {
                            "name": "AzureTableInput"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobOutput"
                        }
                    ],
                    "typeProperties": {
                        "source": {
                            "type": "AzureTableSource",
                            "AzureTableSourceQuery": "PartitionKey eq 'DefaultPartitionKey'"
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

## <a name="sample-copy-data-from-azure-blob-to-azure-table"></a>Näide: Andmete kopeerimine Azure'i bloobimälu Azure'i tabeli

Järgmises näites on kuvatud.

1.  Lingitud teenuse tüüpi [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties) (kasutatakse bloobimälu & tabeli puhul)
3.  Mõne Sisestuskeel [andmekomplekti](data-factory-create-datasets.md) tüüpi [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4.  Mõne väljundi [andmekomplekti](data-factory-create-datasets.md) tüüpi [Azure'i](#azure-table-dataset-type-properties). 
4.  [Müügivõimaluste](data-factory-create-pipelines.md) Kopeeri tegevust, mis kasutab [BlobSource](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties) ja [AzureTableSink](#azure-table-copy-activity-type-properties). 


Valimi koopiad, kellaaja-sarja andmete on Azure Bloobivahemälu on Azure tabeli tund. Nende proovide kasutatakse atribuutide JSON on kirjeldatud jaotiste jälgimise näidised.

**Azure'i salvestusruum (Azure'i tabeli ja bloobimälu) lingitud teenus:**

    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

Azure'i andmed Factory toetab kahte tüüpi lingitud Azure Storage teenused: **AzureStorage** ja **AzureStorageSas**. Esimene, saate määrata ühendusstring, mis sisaldab konto võti ja hiljem üks määrate Uri ühiskasutusse Accessi allkirja (SAS). Üksikasju vt [Lingitud teenuste](#linked-services) jaotises. 

**Azure'i bloobimälu Sisestuskeel andmekomplekti:**

Andmed on te soovite uue bloobimälu tunnis (sagedus: tund, intervall: 1). Kausta tee ja faili nimi soovitud bloobimälu dünaamiliselt hinnatakse sellest osast, mis on töödeldud algusaeg. Kausta tee kasutab aasta, kuu ja päeva osa algusaeg ja faili nimi kasutab tund osa algusaeg. "välise": "true" säte teatab andmete Factory teenus, et andmekomplekti on väliste andmete factory ja ei esitata andmete factory toimingu.
    
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

**Azure'i tabeli väljund andmekomplekti:**

Valimi kopeerib tabelis nimega "MyTable" Azure'i tabeli andmeid. Azure'i tabeli loomine koos sama veergude arv bloobimälu CSV-fail sisaldab ootuspäraselt. Uued read lisatakse tabelisse tunnis. 

    {
      "name": "AzureTableOutput",
      "properties": {
        "type": "AzureTable",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "tableName": "MyOutputTable"
        },
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }

**Torujuhe Kopeeri tegevusega:**

Tulemas on Kopeeri tegevust, mis on konfigureeritud kasutama sisend- ja andmekomplektide ja on plaanitud käivituma tunnis. Tulemas JSON määratlus **Reaallika** tüüp väärtuseks **BlobSource** ja **valamu** tüüp väärtuseks **AzureTableSink**. 


    {  
        "name":"SamplePipeline",
        "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline with copy activity",
        "activities":[  
          {
            "name": "AzureBlobtoTable",
            "description": "Copy Activity",
            "type": "Copy",
            "inputs": [
              {
                "name": "AzureBlobInput"
              }
            ],
            "outputs": [
              {
                "name": "AzureTableOutput"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "BlobSource"
              },
              "sink": {
                "type": "AzureTableSink",
                "writeBatchSize": 100,
                "writeBatchTimeout": "01:00:00"
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

## <a name="linked-services"></a>Lingitud teenused
On kahte tüüpi lingitud teenuste abil saate linkida mõne Azure'i andmed factory Azure'i bloobimälu. Need on: **AzureStorage** lingitud ja **AzureStorageSas** lingitud teenuste. Azure Storage lingitud teenus pakub andmete factory üld-juurdepääsu Azure Storage. Samas Azure'i salvestusruumi SAS (ühiskasutusse Accessi allkiri) lingitud teenus pakub andmete factory piiratud/ajalised juurdepääsu Azure Storage. Ei ole muud erinevusi nende kahe lingitud teenuste vahel. Valige lingitud teenus, mis sobib teie vajadustele. Järgmised jaotised sisaldavad Lisateavet nende kahe lingitud teenuste kohta.

[AZURE.INCLUDE [data-factory-azure-storage-linked-services](../../includes/data-factory-azure-storage-linked-services.md)]

## <a name="azure-table-dataset-type-properties"></a>Azure'i tabeli andmekomplekti tüüp atribuudid

Jaotised ja atribuute määratlemine andmekomplektide täieliku loendi leiate artiklist [loomine andmekomplektide](data-factory-create-datasets.md) . Jaotiste, nt struktuuri, kättesaadavus ja Andmekomplekt JSON poliitika on sarnased kõigi andmekomplekti tüüpide (Azure SQL, Azure'i bloobimälu, Azure'i tabeli jne).

Jaotise typeProperties erineb iga tüüpi andmekomplekti ja andmesalve olevate andmete asukoha teave. Andmekomplekti **typeProperties** jaotises tüüp **Azure'i** on järgmised atribuudid.

| Atribuut | Kirjeldus | Nõutav |
| -------- | ----------- | -------- |
| tableName | Azure'i tabeli andmebaasi eksemplari, mis viitab lingitud tabeli nimi. | Jah. Kui soovitud tableName määratud ilma mõne azureTableSourceQuery, kopeeritakse kõik kirjed tabelist sihtkohta. Kui mõni azureTableSourceQuery on märgitud, on tabeli, mis vastab päringu kirjed sihtkohta kopeerida. |

### <a name="schema-by-data-factory"></a>Skeemi järgi andmete Factory
Skeemi tasuta andmed poe nagu Azure'i tabeli puhul andmete Factory teenuse tuletab skeemiga ühel järgmistest viisidest:

1.  Kui määrate atribuudi **struktuuri** andmekomplekti määratluse abil andmete struktuuri, andmete Factory teenuse kiitusega selle struktuuri skeem. Sel juhul, kui ei sisalda rea väärtuse veeru, tühiväärtus on esitatud seda.
2. Kui te ei määra andmete struktuuri, kasutades atribuudi **struktuuri** andmekomplekti määratluse, andmete Factory tuletab skeemi abil esimese rea andmeid. Sel juhul, kui esimene rida ei sisalda täielik skeemiga, veerge on vastamata kopeerimisel tulemus.

Seetõttu skeemi tasuta andmeallikate jaoks parim on andmete **struktuuri** atribuudi struktuuri määrata.

## <a name="azure-table-copy-activity-type-properties"></a>Azure'i tabeli kopeerimine tegevuse tüüp atribuudid

Jaotised ja atribuudid, mis on saadaval tegevuste määratlemiseks täieliku loendi leiate artiklist [Torujuhtmete loomine](data-factory-create-pipelines.md) . Näiteks nimi, kirjeldus, sisestus- ja väljundi andmekomplektide ja poliitikate atribuudid on saadaval kõigi tegevuse tüüp. 

Tegevuse typeProperties jaotise saadaolevad atribuudid erinevad teiselt iga tegevuse tüüp. Kopeeri tegevuste, need sõltuvad allikate ja neeldajate tüübid.

**AzureTableSource** toetab typeProperties jaotises järgmised atribuudid.

Atribuut | Kirjeldus | Lubatud väärtus | Nõutav
-------- | ----------- | -------------- | -------- 
azureTableSourceQuery | Kohandatud päringu abil saate andmeid lugeda. | Azure'i tabeli päringustringi. Vt näited järgmises jaotises. | Ei. Kui soovitud tableName määratud ilma mõne azureTableSourceQuery, kopeeritakse kõik kirjed tabelist sihtkohta. Kui mõni azureTableSourceQuery on märgitud, on tabeli, mis vastab päringu kirjed sihtkohta kopeerida.
azureTableSourceIgnoreTableNotFound | Näitab, kas neelama tabeli erand pole olemas. | TRUE<br/>FALSE | Ei |

### <a name="azuretablesourcequery-examples"></a>azureTableSourceQuery näited

Kui stringi tüüpi on Azure tabeli veerus. 

    azureTableSourceQuery": "$$Text.Format('PartitionKey ge \\'{0:yyyyMMddHH00_0000}\\' and PartitionKey le \\'{0:yyyyMMddHH00_9999}\\'', SliceStart)"

Kui Azure'i tabeli veerus on kuupäeva ja kellaaja tüüp: 

    "azureTableSourceQuery": "$$Text.Format('DeploymentEndTime gt datetime\\'{0:yyyy-MM-ddTHH:mm:ssZ}\\' and DeploymentEndTime le datetime\\'{1:yyyy-MM-ddTHH:mm:ssZ}\\'', SliceStart, SliceEnd)"


**AzureTableSink** toetab typeProperties jaotises järgmised atribuudid.


Atribuut | Kirjeldus | Lubatud väärtus | Nõutav  
-------- | ----------- | -------------- | -------- 
azureTableDefaultPartitionKeyValue | Sektsiooni võtme vaikeväärtus sink kasutatavate. | Stringiväärtus. | Ei 
azureTablePartitionKeyName | Määrake veergu, mille väärtused on kasutatud partition võtmed. Kui pole määratud, kasutatakse AzureTableDefaultPartitionKeyValue sektsiooni võti. | Veeru nimi. | Ei |
azureTableRowKeyName | Määrake veergu, mille veeru väärtused on kasutatud rea klahvi. Kui pole määratud, kasutage GUID iga rea kohta. | Veeru nimi. | Ei  
azureTableInsertType | Azure'i tabeli andmete lisamiseks režiimi.<br/><br/>Atribuut määrab, kas olemasolevate ridade väljundi tabeli sobitamine sektsiooni ja rida on nende väärtuste asendatakse või ühendatud. <br/><br/>Nende sätete (Ühenda ja asenda) kasutamise kohta leiate teemadest [Lisa või Ühenda üksus](https://msdn.microsoft.com/library/azure/hh452241.aspx) ja [lisamine või asendamine üksus](https://msdn.microsoft.com/library/azure/hh452242.aspx) . <br/><br> See säte rakendub rea tasemel, mitte tabeli tasemel ja ei valik kustutab väljund tabeli ridu, siis sisendit ei ole. | Ühenda (vaikeväärtus)<br/>asendamine | Ei 
writeBatchSize | Lisab andmed tabelisse Azure, kui writeBatchSize või writeBatchTimeout on tulemus. | Täisarv (ridade arv)| Ei (vaikimisi: 10000) 
writeBatchTimeout | Lisab andmed tabelisse Azure kui writeBatchSize või writeBatchTimeout on tulemus | kuuline ajavahemik<br/><br/>Näide: "00: 20:00" (20 minutit) | Ei (vaikimisi salvestusruumi kliendi vaikimisi ajalõpu väärtus 90 SEK)

### <a name="azuretablepartitionkeyname"></a>azureTablePartitionKeyName
Vastendage lähteveerg sihtkoha veeru abil translator JSON atribuudi enne sihtkoha veeru saate kasutada funktsiooni azureTablePartitionKeyName.

Järgmises näites on vastendatud lähteveeru DivisionID sihtkoha veerule: DivisionID.  

    "translator": {
        "type": "TabularTranslator",
        "columnMappings": "DivisionID: DivisionID, FirstName: FirstName, LastName: LastName"
    } 

Funktsiooni DivisionID on määratud sektsioon võti. 

    "sink": {
        "type": "AzureTableSink",
        "azureTablePartitionKeyName": "DivisionID",
        "writeBatchSize": 100,
        "writeBatchTimeout": "01:00:00"
    }


[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

### <a name="type-mapping-for-azure-table"></a>Tippige vastenduse Azure'i tabeli jaoks

Nagu [andmete liikumine tegevuste](data-factory-data-movement-activities.md) artiklis mainitud, teeb koopia tegevuse automaatne tüüpide dokumenditeisenduste allikatüüpide valamu tüüpi järgmised kaheastmelise lähenemisviisi kaudu.

1. .Net-i tüüp teisendada kohalikke andmeallika tüübiga.
2. Teisendada .NET kohalikke valamu tüüp

Liikudes andmed ja Azure'i tabeli põhjal, järgmised [vastendused määratletud Azure'i tabeli teenuse](https://msdn.microsoft.com/library/azure/dd179338.aspx) kasutatakse Azure'i tabeli OData tüüpi .net-i tüüp ja vastupidi. 

| OData-andmetüüp | .Net-i tüüp | Üksikasjad |
| --------------- | --------- | ------- |
| Edm.Binary | byte] | Massiivi baiti kuni 64 KB. |
| Edm.Boolean | bool | Loogikaväärtus. |
| Edm.DateTime | Kuupäev ja kellaaeg | 64-bitine väärtus väljendatud koordineeritud maailmaaega (UTC). Toetatud kuupäeva ja kellaaja vahemikus algab 12:00 keskööst jaanuar 1, 1601 ad (CE) UTC. Vahemiku lõpeb 31 Detsember 9999. |
| Edm.Double | kahekordne | 64-bitine ujuv punkti väärtus. |
| Edm.Guid | GUID | 128-bitise globaalselt kordumatut tunnust. |
| Edm.Int32 | Int32 või int | 32-bitise täisarvulise. |
| Edm.Int64 | Int64 ja vana | 64-bitise täisarvulise. |
| Edm.String | String | UTF-16-kodeeringuga väärtus. Stringi väärtuse võib olla kuni 64 KB. |

### <a name="type-conversion-sample"></a>Tüübi teisendamine näidis

Järgmises näites on kopeerimise andmed on Azure'i bloobimälu Azure'i tabeli tüüp teisendused. 

Oletame, et bloobimälu andmekomplekti on CSV-vormingus ja see sisaldab kolme veergu. Üks neist on kuupäeva ja kellaaja veerg kohandatud kuupäeva ja kellaaja vormingu abil nädalapäeva lühendatud Prantsuse nimede abil. 

Määratleda bloobimälu allika andmekomplekti järgmiselt koos tüüp veergude määratlusi.
    
    {
        "name": " AzureBlobInput",
        "properties":
        {
             "structure": 
              [
                    { "name": "userid", "type": "Int64"},
                    { "name": "name", "type": "String"},
                    { "name": "lastlogindate", "type": "Datetime", "culture": "fr-fr", "format": "ddd-MM-YYYY"}
              ],
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/myfolder",
                "fileName":"myfile.csv",
                "format":
                {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "external": true,
            "availability":
            {
                "frequency": "Hour",
                "interval": 1,
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

Azure'i tabeli OData tüüp tüüp vastenduse pöörata .NET tüüp oleks määratleda tabeli Azure'i tabeli järgmist skeemi. 

**Azure'i tabeli skeemi:**

Veeru nimi | Tüüp
----------- | --------
kasutajanimi | Edm.Int64
Nimi | Edm.String 
lastlogindate | Edm.DateTime

Järgmisena Määratlege Azure'i tabeli andmekomplekti. Teil pole vaja määrata "struktuur" jaotis teabega tüüp, kuna tüüpi teavet on juba määratud päringu aluseks andmesalve.

    {
      "name": "AzureTableOutput",
      "properties": {
        "type": "AzureTable",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "tableName": "MyOutputTable"
        },
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }

Sel juhul andmete Factory automaatselt andmetüüpide teisendamise, sh abil "fr-fr" Kultuur tabelisse Azure'i bloobimälu andmete teisaldamisel kohandatud kuupäeva ja kellaaja vormingus Datetime välja.



[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

## <a name="performance-and-tuning"></a>Jõudluse ja häälestamine  
Selle mõju jõudlust andmete liikumine (Kopeeri tegevus) Azure'i andmed Factory ja optimeerida seda mitmel viisil võtme tegurite kohta lisateabe saamiseks vt [Kopeeri tegevuse jõudlus ja juhend häälestamine](data-factory-copy-activity-performance.md).







