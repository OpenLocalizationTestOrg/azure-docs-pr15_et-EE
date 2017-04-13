<properties 
    pageTitle="Teisaldage andmed MySQL-i | Azure'i andmed Factory" 
    description="Lisateavet andmete teisaldamiseks abil Azure'i andmed Factory MySQL-i andmebaasist." 
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

# <a name="move-data-from-mysql-using-azure-data-factory"></a>Andmete teisaldamine kaudu MySQL-i abil Azure'i andmed Factory

See artikkel kirjeldab, kuidas saate Kopeeri tegevuse on Azure andmete factory andmete MySQL teise andmete talletamiseks. Selles artiklis põhineb [andmete liikumine tegevuste](data-factory-data-movement-activities.md) artikkel, mis annab ülevaate andmete liikumine Kopeeri aktiivsusest ja toetatud andmete poe kombinatsioone.

Andmete Factory teenuse toetab ühenduse loomine MySQL-i asutusesisestest allikatest, kasutades Andmehalduslüüsi. Lisateavet Andmehalduslüüsi ja lüüsi häälestamise juhised leiate [jooksva andmete asukohad kohapealse ja pilveteenuse vahel](data-factory-move-data-between-onprem-and-cloud.md) artiklist. 

> [AZURE.NOTE] Lüüs on vaja ka siis, kui MySQL-i andmebaas on majutatud Azure'i IaaS virtuaalse masina (VM). Saate installida lüüsi, sama nimega andmesalve VM või muu VM kui lüüsi saate andmebaasiga ühenduse loomiseks.  

Andmete factory toetab praegu ainult jooksva andmete MySQL-i muid andmeid talletab, kuid mitte muid andmeid salvestab andmete teisaldamine MySQL-i.

## <a name="installation"></a>Installimine 
Andmehalduslüüsi MySQL-i andmebaasiga ühenduse loomiseks, peate installida [MySQL-i konnektori/Net 6.6.5 Microsoft Windowsi](http://go.microsoft.com/fwlink/?LinkId=278885) sama süsteemi Andmehalduslüüsi.

> [AZURE.NOTE] Lugege teemat [tõrkeotsing lüüsi probleemide](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) näpunäiteid tõrkeotsingu ühenduse/lüüsi seotud probleemid. 

## <a name="copy-data-wizard"></a>Kopeerige andmed viisard
Lihtsaim viis loomiseks kopeerib andmete MySQL-i andmebaasist toetatud valamu andmete poe on Kopeerige andmed viisardi abil. Vt [õpetus: müügivõimaluste koopia viisardi abil luua](data-factory-copy-data-wizard-tutorial.md) loomiseks Kopeerige andmed viisardi abil müügivõimaluste kiirülevaate selgituse. 

Järgmises näites toodud valimi JSON määratlused, mille abil saate loomiseks. Üksikasjalike juhiste täieliku lühiülevaade [jooksva andmete asukohad kohapealse ja pilveteenuse vahel](data-factory-move-data-between-onprem-and-cloud.md) artiklist. Andmeid saab kopeerida mõnda valamud märgitud [siin](data-factory-data-movement-activities.md#supported-data-stores) Azure'i andmed Factory Kopeeri tegevuse abil.   

## <a name="sample-copy-data-from-mysql-to-azure-blob"></a>Näide: Andmete kopeerimine MySQL-i Azure'i bloobimälu
See näide näitab, kuidas andmeid kopeerida mõne Azure'i bloobimälu asutusesisese MySQL-andmebaasiga. Andmete saate siiski kopeeritud **otse** mis tahes valamud märgitud [siin](data-factory-data-movement-activities.md#supported-data-stores) Azure'i andmed Factory Kopeeri tegevuse abil.  

> [AZURE.IMPORTANT] See näide pakub JSON Koodilõigud. Ei sisalda üksikasjalikke juhiseid andmete factory loomise kohta. Üksikasjalikud juhised [jooksva andmete asukohad kohapealse ja pilveteenuse vahel](data-factory-move-data-between-onprem-and-cloud.md) artiklist. 
 
Valimi on andmete factory järgmised üksused:

1.  Lingitud teenuse tüüpi [OnPremisesMySql](data-factory-onprem-mysql-connector.md#mysql-linked-service-properties).
2.  Lingitud teenuse tüüpi [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.  Mõne Sisestuskeel [andmekomplekti](data-factory-create-datasets.md) tüüpi [RelationalTable](data-factory-onprem-mysql-connector.md#mysql-dataset-type-properties).
4.  Mõne väljundi [andmekomplekti](data-factory-create-datasets.md) tüüpi [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4.  [Müügivõimaluste](data-factory-create-pipelines.md) Kopeeri tegevust, mis kasutab [RelationalSource](data-factory-onprem-mysql-connector.md#mysql-copy-activity-type-properties) ja [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

Valimi kopeerib andmete päringu tulem MySQL-i andmebaasis on bloobimälu tunnis. Nende proovide kasutatakse atribuutide JSON on kirjeldatud jaotiste jälgimise näidised. 

Esimese sammuna install andmehalduslüüsi. Juhised on artikli [jooksva andmete asukohad kohapealse ja pilveteenuse vahel](data-factory-move-data-between-onprem-and-cloud.md) . 

**Lingitud MySQL-i teenus**

    {
      "name": "OnPremMySqlLinkedService",
      "properties": {
        "type": "OnPremisesMySql",
        "typeProperties": {
          "server": "<server name>",
          "database": "<database name>",
          "schema": "<schema name>",
          "authenticationType": "<authentication type>",
          "userName": "<user name>",
          "password": "<password>",
          "gatewayName": "<gateway>"
        }
      }
    }

**Azure'i lingitud salvestusteenus**

    {
      "name": "AzureStorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

**MySQL-i Sisestuskeel andmekomplekti**

Valimi eeldab, et olete loonud tabeli "MyTable" MySQL-i ja see sisaldab veerg nimega "timestampcolumn" kellaajaandmete sarja.

Säte "välise": "true" teatab andmete Factory teenus, et tabelis on väliste andmete factory ja ei esitata andmete factory toimingu.
    
    {
        "name": "MySqlDataSet",
        "properties": {
            "published": false,
            "type": "RelationalTable",
            "linkedServiceName": "OnPremMySqlLinkedService",
            "typeProperties": {},
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true,
            "policy": {
                "externalData": {
                    "retryInterval": "00:01:00",
                    "retryTimeout": "00:10:00",
                    "maximumRetry": 3
                }
            }
        }
    }



**Azure'i bloobimälu väljund andmekomplekti**

Andmed on kirjutatud uue bloobimälu tunnis (sagedus: tund, intervall: 1). Kausta tee on bloobimälu hinnatakse dünaamiliselt algusaja sellest osast, mis töötlemise põhjal. Kausta tee kasutab aasta, kuu, päev ja tunni osad algusaeg.

    {
        "name": "AzureBlobMySqlDataSet",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/mysql/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
                "format": {
                    "type": "TextFormat",
                    "rowDelimiter": "\n",
                    "columnDelimiter": "\t"
                },
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
                ]
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }



**Torujuhe Kopeeri tegevus**

Tulemas on Kopeeri tegevust, mis on konfigureeritud kasutama sisend- ja andmekomplektide ja on plaanitud käivituma tunnis. Tulemas JSON määratlus **Reaallika** tüüp väärtuseks **RelationalSource** ja **valamu** tüüp väärtuseks **BlobSink**. SQL-päringu jaoks **päringu** atribuudi määratud valib andmete kopeerimiseks tunnis.
    
    {
        "name": "CopyMySqlToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "RelationalSource",
                            "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', WindowStart, WindowEnd)"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "MySqlDataSet"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobMySqlDataSet"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00",
                        "concurrency": 1
                    },
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    },
                    "name": "MySqlToBlob"
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z"
        }
    }



## <a name="mysql-linked-service-properties"></a>Lingitud MySQL-i teenuse atribuudid

Järgmises tabelis on ära toodud JSON elementide teatud lingitud MySQL-i teenuse kirjeldus.

| Atribuut | Kirjeldus | Nõutav |
| -------- | ----------- | -------- | 
| tüüp | Atribuudi tüüp väärtuseks peab olema seatud: **OnPremisesMySql** | Jah |
| Server | MySQL-i serveri nimi. | Jah |
| andmebaasi | MySQL-i andmebaasi nimi. | Jah | 
| skeemi  | Andmebaasi skeemi nime. | Ei | 
| authenticationType | MySQL-i andmebaasiga ühenduse loomiseks kasutatud autentimise tüüp. Võimalikud väärtused on: Anonüümne, Basic ja Windows. | Jah | 
| kasutajanimi | Määrake kasutajanimi, kui kasutate Basic või Windowsi autentimist. | Ei | 
| parooli | Määrake kasutajanimi jaoks määratud kasutajakonto parooli. | Ei | 
| gatewayName | Lüüsi, mis kohapealse MySQL-i andmebaasiga ühenduse loomiseks kasutama andmete Factory teenuse nimi. | Jah |

Üksikasjad kohapealse MySQL-i andmeallika mandaati määramise kohta leiate [säte mandaadid ja turvalisus](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) .

## <a name="mysql-dataset-type-properties"></a>MySQL-i andmekomplekti tüüp atribuudid

Jaotiste ja atribuute määratlemine andmekomplektide täieliku loendi leiate artiklist [loomine andmekomplektide](data-factory-create-datasets.md) . Jaotiste, nt struktuuri, kättesaadavus ja Andmekomplekt JSON poliitika on sarnased kõigi andmekomplekti tüüpide (Azure SQL, Azure'i bloobimälu, Azure'i tabeli jne).

Jaotise **typeProperties** erineb iga tüüpi andmekomplekti ja andmesalve olevate andmete asukoha teave. Andmekomplekti typeProperties jaotises tüüp **RelationalTable** (mis sisaldab MySQL-i andmekomplekti) on järgmised atribuudid

| Atribuut | Kirjeldus | Nõutav |
| -------- | ----------- | -------- |
| tableName | MySQL-andmebaasiga eksemplari, mis viitab lingitud tabeli nimi. | Ei, (kui **RelationalSource** **päring** on määratud) | 

## <a name="mysql-copy-activity-type-properties"></a>MySQL-i kopeerimine tegevuse tüüp atribuudid

Jaotised ja atribuudid, mis on saadaval tegevuste määratlemiseks täieliku loendi leiate artiklist [Torujuhtmete loomine](data-factory-create-pipelines.md) . Nimi, kirjeldus, sisend- ja tabelid, nt atribuudid on poliitikad on saadaval kõigi tegevuste jaoks. 

Tegevuse **typeProperties** jaotise saadaolevad atribuudid erinevad teiselt iga tegevuse tüüp. Kopeeri tegevuste, need sõltuvad allikate ja neeldajate tüübid.

Kui allikas Kopeeri tegevuse tüüp **RelationalSource** (mis sisaldab MySQL-i) järgmised atribuudid on saadaval jaotises typeProperties:

| Atribuut | Kirjeldus | Lubatud väärtus | Nõutav |
| -------- | ----------- | -------------- | -------- |
| päringu | Kohandatud päringu abil saate andmeid lugeda. | SQL-i päringustringi. Näide: valige * MyTable kaudu. | Ei, (kui **tableName** **andmekogumi** on määratud) | 

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

### <a name="type-mapping-for-mysql"></a>Tippige vastenduse MySQL-i jaoks

Kui [andmete liikumine tegevuste](data-factory-data-movement-activities.md) artiklis mainitud, sooritab Kopeeri tegevuse automaatne tüüpide teisendused allikatüüpide valamu tüüpi järgmised kaheastmelise lähenemisviisi kaudu.

1. .Net-i tüüp teisendada kohalikke andmeallika tüübiga.
2. Teisendada .NET kohalikke valamu tüüp

Kui andmete teisaldamine MySQL-i järgmised vastendused kasutatakse .NET tüübid, MySQL-i andmetüübid.

| MySQL-andmebaasiga tüüp | .NET framework tüüp |
| ------------------- | ------------------- | 
| suur täisarv allkirjastamata | Decimal |
| suur täisarv | Int64 |
| bit | Decimal |
| bloobimälu | Byte] |
| bool |  Kahendmuutuja | 
| char | String |
| kuupäev | Kuupäev ja kellaaeg |
| kuupäev ja kellaaeg | Kuupäev ja kellaaeg |
| Decimal | Decimal |
| kahekordne täpsus | Kahekordne |
| kahekordne | Kahekordne |
| loetelu | String |
| hõljumine | Ühe |
| allkirjastamata int | Int64 |
| int | Int32 |
| allkirjastamata täisarv | Int64 |
| täisarv | Int32 | 
| pikk varbinaar | Byte] |
| pikk varchar | String |
| longblob | Byte] |
| LONGTEXT | String | 
| mediumblob |  Byte] | 
| allkirjastamata mediumint | Int64 |
| mediumint | Int32 | 
| mediumtext | String |
| arvuline | Decimal |
| reaal |  Kahekordne |
| seadmine | String |
| allkirjastamata smallint | Int32 |
| smallint | Int16 |
| teksti | String |
| aeg | Kuuline ajavahemik |
| ajatempli | Kuupäev ja kellaaeg |
| tinyblob | Byte] |
| allkirjastamata tinyint |  Int16 | 
| tinyint | Int16 |
| tinytext | String | 
| varchar | String | 
| aasta | Int | 


[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

## <a name="performance-and-tuning"></a>Jõudluse ja häälestamine  
Vaata [Kopeeri tegevuse jõudlus ja häälestamine juhend](data-factory-copy-activity-performance.md) Lisateavet peamised tegurid selle mõju jõudlust andmete liikumine (Kopeeri tegevus) Azure'i andmed Factory ja optimeerida seda mitmel viisil.

