<properties 
    pageTitle="Teisaldage andmed DB2 | Azure'i andmed Factory" 
    description="Siit saate teada, kuidas Azure'i andmed Factory abil DB2 andmebaasist andmete teisaldamine" 
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
    ms.date="09/06/2016" 
    ms.author="jingwang"/>

# <a name="move-data-from-db2-using-azure-data-factory"></a>Andmete teisaldamine DB2 Azure'i andmed Factory abil
See artikkel kirjeldab, kuidas saate Kopeeri tegevuse on Azure andmete factory andmete DB2 teise andmete talletamiseks. Selles artiklis põhineb [andmete liikumine tegevuste](data-factory-data-movement-activities.md) artikkel, mis annab ülevaate andmete liikumine Kopeeri tegevuste ja toetatud andmete poe kombinatsioone.

Andmete factory toetab ühenduse asutusesisese DB2 allikatest Andmehalduslüüsi abil. Lisateavet Andmehalduslüüsi ja lüüsi häälestamise juhised leiate [jooksva andmete asukohad kohapealse ja pilveteenuse vahel](data-factory-move-data-between-onprem-and-cloud.md) artiklist. 

> [AZURE.NOTE]
Lüüsi abil ühenduse DB2 isegi juhul, kui see on majutatud Azure'i IaaS VM. Kui soovite ühenduse DB2 majutatud pilveteenuses eksemplari, saate installida ka lüüsi eksemplari IaaS VM.

Andmete factory toetab praegu ainult jooksva andmete DB2 muid andmeid talletab, mitte muude andmete poed DB2. 

## <a name="installation"></a>Installimine 

Andmehalduslüüsi pakub sisseehitatud DB2 draiver, mis toetab järgmist: 

- SQLAM 9 / 10 / 11
- DB2 LUW (Linux, Unix, Windows)
- DB2 z/OS ja DB2 i (ehk nimega / 400)

Seega peate enam käsitsi installida andmete kopeerimisel DB2.

> [AZURE.NOTE] Lugege teemat [tõrkeotsing lüüsi probleemide](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) näpunäiteid tõrkeotsingu ühenduse/lüüsi seotud probleemid. 

## <a name="copy-data-wizard"></a>Kopeerige andmed viisard
Lihtsaim viis loomiseks kopeerib andmete DB2 andmebaasist on Kopeerige andmed viisardi abil. Vt [õpetus: müügivõimaluste koopia viisardi abil luua](data-factory-copy-data-wizard-tutorial.md) loomiseks Kopeerige andmed viisardi abil müügivõimaluste kiirülevaate selgituse. 

Järgmised näited annavad valimi JSON määratlused kasutatavad loomiseks [Azure portaali](data-factory-copy-activity-tutorial-using-azure-portal.md) või [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) või [Azure PowerShelli](data-factory-copy-activity-tutorial-using-powershell.md)abil. Andmete kopeerimine DB2 andmebaas ja Azure'i bloobimälu kuvatakse. Siiski saate andmeid kopeerida kõiki valamud märgitud [siin](data-factory-data-movement-activities.md#supported-data-stores) Azure'i andmed Factory Kopeeri tegevuse abil.

## <a name="sample-copy-data-from-db2-to-azure-blob"></a>Näide: Andmete kopeerimine DB2 Azure'i bloobimälu

See näide näitab, kuidas kohapealse DB2 andmebaasist andmete kopeerimine mõnda Azure'i bloobimälu. Andmete saate siiski kopeeritud **otse** mis tahes valamud märgitud [siin](data-factory-data-movement-activities.md#supported-data-stores) Azure'i andmed Factory Kopeeri tegevuse abil.  
 
Valimi on andmete factory järgmised üksused:

1.  Lingitud teenuse tüüpi [OnPremisesDb2](data-factory-onprem-db2-connector.md#db2-linked-service-properties).
2.  Lingitud teenuse tüüpi [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties). 
3.  Mõne Sisestuskeel [andmekomplekti](data-factory-create-datasets.md) tüüpi [RelationalTable](data-factory-onprem-db2-connector.md#db2-dataset-type-properties).
4.  Mõne väljundi [andmekomplekti](data-factory-create-datasets.md) tüüpi [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties). 
5.  [Müügivõimaluste](data-factory-create-pipelines.md) Kopeeri tegevust, mis kasutab [RelationalSource](data-factory-onprem-db2-connector.md#db2-copy-activity-type-properties) ja [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties). 

Valimi kopeerib andmete päringu tulem DB2 andmebaasist mõne Azure'i bloobimälu tunnis. Nende proovide kasutatakse atribuutide JSON on kirjeldatud jaotiste jälgimise näidised. 

Esimese sammuna installima ja konfigureerima andmehalduslüüsi. [Jooksva andmete asukohad kohapealse ja pilveteenuse vahel](data-factory-move-data-between-onprem-and-cloud.md) artiklis on juhised.

**Lingitud DB2 teenus:**

    {
        "name": "OnPremDb2LinkedService",
        "properties": {
            "type": "OnPremisesDb2",
            "typeProperties": {
                "server": "<server>",
                "database": "<database>",
                "schema": "<schema>",
                "authenticationType": "<authentication type>",
                "username": "<username>",
                "password": "<password>",
                "gatewayName": "<gatewayName>"
            }
        }
    }


**Azure'i bloobimälu lingitud teenuse:**

    {
        "name": "AzureStorageLinkedService",
        "properties": {
            "type": "AzureStorageLinkedService",
            "typeProperties": {
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=<AccountName>;AccountKey=<AccountKey>"
            }
        }
    }

**DB2 Sisestuskeel andmekomplekti:**

Valimi eeldab, et olete loonud tabeli "MyTable" DB2 ja see sisaldab veerg nimega "ajatempli" kellaajaandmete sarja.

Säte "välise": true teatab andmete Factory teenuse, et see andmekomplekti on väliste andmete factory ja ei esitata andmete factory toimingu. Pange tähele, et **Tüüp** on määratud **RelationalTable**. 

    {
        "name": "Db2DataSet",
        "properties": {
            "type": "RelationalTable",
            "linkedServiceName": "OnPremDb2LinkedService",
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


**Azure'i bloobimälu väljund andmekomplekti:**

Andmed on kirjutatud uue bloobimälu tunnis (sagedus: tund, intervall: 1). Kausta tee on bloobimälu hinnatakse dünaamiliselt algusaja sellest osast, mis töötlemise põhjal. Kausta tee kasutab aasta, kuu, päev ja tunni osad algusaeg.

    {
        "name": "AzureBlobDb2DataSet",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/db2/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

**Torujuhe Kopeeri tegevuse abil:**

Tulemas on Kopeeri tegevust, mis on konfigureeritud kasutama sisend- ja andmekomplektide ja on plaanitud käivituma tunnis. Tulemas JSON määratlus **Reaallika** tüüp väärtuseks **RelationalSource** ja **valamu** tüüp väärtuseks **BlobSink**. SQL-päringu jaoks **päringu** atribuudi määratud klõpsab andmed tabelis Orders.

    {
        "name": "CopyDb2ToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "RelationalSource",
                            "query": "select * from \"Orders\""
                        },
                        "sink": {
                            "type": "BlobSink"
                        }
                    },
                    "inputs": [
                        {
                            "name": "Db2DataSet"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobDb2DataSet"
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
                    "name": "Db2ToBlob"
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z"
        }
    }


## <a name="db2-linked-service-properties"></a>Lingitud DB2 teenuse atribuudid

Järgmises tabelis on ära toodud JSON elementide teatud DB2 lingitud teenuse kirjeldus. 

| Atribuut | Kirjeldus | Nõutav |
| -------- | ----------- | -------- | 
| tüüp | Atribuudi tüüp väärtuseks peab olema seatud: **OnPremisesDB2** | Jah |
| Server | DB2 serveri nimi. | Jah |
| andmebaasi | DB2 andmebaasi nimi. | Jah |
| skeemi | Andmebaasi skeemi nime. Skeemi nimi on tõstutundlikud. | Ei |
| authenticationType | Tüüp, autentimine DB2 andmebaasiga ühenduse loomiseks kasutada. Võimalikud väärtused on: Anonüümne, Basic ja Windows. | Jah |
| kasutajanimi | Määrake kasutajanimi, kui kasutate Basic või Windowsi autentimist. | Ei |
| parooli | Määrake kasutajanimi jaoks määratud kasutajakonto parooli. | Ei |
| gatewayName | Lüüsi, mis kohapealse DB2 andmebaasiga ühenduse loomiseks kasutama andmete Factory teenuse nimi. | Jah |

Üksikasjad kohapealse DB2 andmeallika mandaati määramise kohta leiate [säte mandaadid ja turvalisus](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) . 


## <a name="db2-dataset-type-properties"></a>DB2 andmekomplekti tüüp atribuudid

Jaotised ja atribuute määratlemine andmekomplektide täieliku loendi leiate artiklist [loomine andmekomplektide](data-factory-create-datasets.md) . Jaotiste, nt struktuuri, kättesaadavus ja Andmekomplekt JSON poliitika on sarnased kõigi andmekomplekti tüüpide (Azure SQL, Azure'i bloobimälu, Azure'i tabeli jne).

Jaotise typeProperties erineb iga tüüpi andmekomplekti ja andmesalve olevate andmete asukoha teave. Andmekomplekti typeProperties jaotises Tüüp RelationalTable (mis sisaldab DB2 andmekomplekti) on järgmised atribuudid.

| Atribuut | Kirjeldus | Nõutav |
| -------- | ----------- | -------- | 
| tableName | DB2 andmebaasiga eksemplari, mis viitab lingitud tabeli nimi. Funktsiooni tableName on tõstutundlikud. | Ei, (kui **RelationalSource** **päring** on määratud) |

## <a name="db2-copy-activity-type-properties"></a>DB2 Kopeeri tegevuse tüüp atribuudid

Jaotised ja atribuudid, mis on saadaval tegevuste määratlemiseks täieliku loendi leiate artiklist [Torujuhtmete loomine](data-factory-create-pipelines.md) . Näiteks nimi, kirjeldus, sisestus- ja väljundi tabelite ja poliitikate atribuudid on saadaval kõigi tegevuse tüüp. 

Tegevuse typeProperties jaotise saadaolevad atribuudid erinevad teiselt iga tegevuse tüüp. Kopeeri tegevuste, need sõltuvad allikate ja neeldajate tüübid.

Kopeeri tegevuste, kui allika tüüp **RelationalSource** (mis sisaldab DB2) järgmised atribuudid on saadaval jaotises typeProperties:


| Atribuut | Kirjeldus | Lubatud väärtus | Nõutav |
| -------- | ----------- | -------- | -------------- |
| päringu | Kohandatud päringu abil saate andmeid lugeda. | SQL-i päringustringi. Näide: `"query": "select * from "MySchema"."MyTable""`. | Ei, (kui **tableName** **andmekogumi** on määratud)|

> [AZURE.NOTE] Skeemi ja tabelinimede on tõstutundlikud. Ümbritsege nimed "" (jutumärkide topeltklõpsake) päringu.  

**Näide:**

    "query": "select * from "DB2ADMIN"."Customers""


[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="type-mapping-for-db2"></a>Tippige vastenduse DB2
Nagu [andmete liikumine tegevusi](data-factory-data-movement-activities.md) artiklis mainitud, sooritab Kopeeri tegevuse automaatne tüüpide dokumenditeisenduste allikatüüpide valamu tüüpi järgmised 2-samm lähenemisviisi kaudu.

1. .Net-i tüüp teisendada kohalikke andmeallika tüübiga.
2. Teisendada .NET kohalikke valamu tüüp

Kui andmete teisaldamine DB2 järgmised vastendused kasutatakse .NET tüüp, DB2 tüüp.

Tippige DB2 andmebaasiga | .NET framework tüüp 
----------------- | ------------------- 
SmallInt | Int16
Täisarv | Int32
Suur täisarv | Int64
Reaal | Ühe
Kahekordne | Kahekordne
Ujuvaks | Kahekordne
Decimal | Decimal
DecimalFloat | Decimal
Arv | Decimal
Kuupäev | Kuupäev ja kellaaeg
Aeg | Kuuline ajavahemik
Ajatempli | Kuupäev ja kellaaeg
XML-i | Byte]
Char | String
VarChar | String
LongVarChar | String
DB2DynArray | String
Binaarsed | Byte]
Varbinaar | Byte]
LongVarBinary | Byte]
Pilt | String
VarGraphic | String
LongVarGraphic | String
CLOB | String
Bloobimälu | Byte]
DbClob | String
SmallInt | Int16
Täisarv | Int32
Suur täisarv | Int64
Reaal | Ühe
Kahekordne | Kahekordne
Ujuvaks | Kahekordne
Decimal | Decimal
DecimalFloat | Decimal
Arv | Decimal
Kuupäev | Kuupäev ja kellaaeg
Aeg | Kuuline ajavahemik
Ajatempli | Kuupäev ja kellaaeg
XML-i | Byte]
Char | String


[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]


[AZURE.INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

## <a name="performance-and-tuning"></a>Jõudluse ja häälestamine  
Vaata [Kopeeri tegevuse jõudlus ja häälestamine juhend](data-factory-copy-activity-performance.md) Lisateavet peamised tegurid selle mõju jõudlust andmete liikumine (Kopeeri tegevus) Azure'i andmed Factory ja optimeerida seda mitmel viisil.


