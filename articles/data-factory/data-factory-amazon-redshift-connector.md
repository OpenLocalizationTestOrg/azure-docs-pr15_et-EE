<properties 
    pageTitle="Teisaldage andmed Amazon punanihe abil andmete Factory | Microsoft Azure'i" 
    description="Lugege selle kohta, kuidas andmeid Amazon punanihe Azure'i andmed Factory abil teisaldada." 
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
    ms.date="08/23/2016" 
    ms.author="jingwang"/>

# <a name="move-data-from-amazon-redshift-using-azure-data-factory"></a>Andmete teisaldamine alla Amazon punanihe Azure'i andmed Factory abil

See artikkel kirjeldab, kuidas saate Kopeeri tegevuse on Azure andmete factory andmete talletamiseks Amazon punanihe teise andmete põhjal. Selles artiklis põhineb [andmete liikumine tegevuste](data-factory-data-movement-activities.md) artikkel, mis tutvustab andmete liikumine ülevaate Kopeeri tegevuste ja allika/valamu andmete salvestab loendi.  

Andmete factory toetab praegu ainult jooksva andmete Amazon punanihe muid andmeid talletab, kuid mitte muid andmeid salvestab andmete teisaldamine Amazon punanihe.

## <a name="prerequisites"></a>Eeltingimused

- Kui andmete teisaldamine mõnda kohapealse andmesalve anda Andmehalduslüüsi (Kasuta IP-aadressi) Amazon punanihe kobar juurdepääs. Vaadake [Autoriseerin juurdepääsu klaster](http://docs.aws.amazon.com/redshift/latest/gsg/rs-gsg-authorize-cluster-access.html) olevaid juhiseid. 
- Kui andmete teisaldamine mõnda Azure andmesalve teemast [Azure andmete keskmist IP-aadresside vahemikud](https://www.microsoft.com/download/details.aspx?id=41653) on arvutada IP-aadresside vahemikud (sh SQL-i vahemikke) kasutavad Microsoft Azure'i andmekeskuste. 

## <a name="copy-data-wizard"></a>Kopeerige andmed viisard
Lihtsaim viis loomiseks mis kopeerib andmete Amazon punanihe on Kopeerige andmed viisardi abil. Vt [õpetus: müügivõimaluste koopia viisardi abil luua](data-factory-copy-data-wizard-tutorial.md) loomiseks Kopeerige andmed viisardi abil müügivõimaluste kiirülevaate selgituse. 

Järgmises näites sisaldab valimi JSON määratlused kasutatavad loomiseks [Azure portaali](data-factory-copy-activity-tutorial-using-azure-portal.md) või [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) või [Azure PowerShelli](data-factory-copy-activity-tutorial-using-powershell.md)abil. See näitab, kuidas Amazon punanihe andmete kopeerimine Azure'i bloobimälu. Siiski saate andmeid kopeerida kõiki valamud märgitud [siin](data-factory-data-movement-activities.md#supported-data-stores).

## <a name="sample-copy-data-from-amazon-redshift-to-azure-blob"></a>Näide: Andmete kopeerimine Amazon punanihe Azure'i bloobimälu
See näide näitab, kuidas Amazon punanihe andmebaasis asuvate andmete kopeerimine mõnda Azure'i bloobimälu. Andmete saate siiski kopeeritud **otse** mis tahes valamud märgitud [siin](data-factory-data-movement-activities.md#supported-data-stores) Azure'i andmed Factory Kopeeri tegevuse abil.  
 
Valimi on andmete factory järgmised üksused:

- Lingitud teenuse tüüpi [AmazonRedshift](#linked-service-properties).
- Lingitud teenuse tüüpi [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
- Mõne Sisestuskeel [andmekomplekti](data-factory-create-datasets.md) tüüpi [RelationalTable](#dataset-type-properties).
- Mõne väljundi [andmekomplekti](data-factory-create-datasets.md) tüüpi [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
- [Müügivõimaluste](data-factory-create-pipelines.md) Kopeeri tegevust, mis kasutab [RelationalSource](#copy-activity-type-properties) ja [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

Valimi kopeerib andmete Amazon punanihe päringutulem on bloobimälu tunnis. Nende proovide kasutatakse atribuutide JSON on kirjeldatud jaotiste jälgimise näidised. 

**Amazon punanihe lingitud teenus**

    {
        "name": "AmazonRedshiftLinkedService",
        "properties":
        {
            "type": "AmazonRedshift",
            "typeProperties":
            {
                "server": "< The IP address or host name of the Amazon Redshift server >",
                "port": <The number of the TCP port that the Amazon Redshift server uses to listen for client connections.>,
                "database": "<The database name of the Amazon Redshift database>",
                "username": "<username>",
                "password": "<password>"
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

**Amazon punanihe Sisestuskeel andmekomplekti**

Säte **"välise": true** andmete Factory teenuse teatab, et andmekomplekti on väliste andmete factory ja ei esitata andmete factory toimingu. Selle atribuudi väärtuseks true Sisestuskeel andmekomplekti, mis ei esitata tulemas tegevuse kohta.

    {
        "name": "AmazonRedshiftInputDataset",
        "properties": {
            "type": "RelationalTable",
            "linkedServiceName": "AmazonRedshiftLinkedService",
            "typeProperties": {
                "tableName": "<Table name>"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true
        }
    }


**Azure'i bloobimälu väljund andmekomplekti**

Andmed on kirjutatud uue bloobimälu tunnis (sagedus: tund, intervall: 1). Kausta tee on bloobimälu hinnatakse dünaamiliselt algusaja sellest osast, mis töötlemise põhjal. Kausta tee kasutab aasta, kuu, päev ja tunni osad algusaeg.

    {
        "name": "AzureBlobOutputDataSet",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/fromamazonredshift/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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
        "name": "CopyAmazonRedshiftToBlob",
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
                            "name": "AmazonRedshiftInputDataset"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobOutputDataSet"
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
                    "name": "AmazonRedshiftToBlob"
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z"
        }
    }



## <a name="linked-service-properties"></a>Lingitud teenuse atribuudid

Järgmises tabelis on ära toodud JSON elementide teatud Amazon punanihe lingitud teenuse kirjeldus.

| Atribuut | Kirjeldus | Nõutav |
| -------- | ----------- | -------- | 
| tüüp | Atribuudi tüüp väärtuseks peab olema seatud: **AmazonRedshift**. | Jah |
| Server | IP-aadress või hosti Amazon punanihe serveri nimi. | Jah |
| Port | Amazon punanihe server kasutab kuulata kliendi ühenduste TCP-pordi number. | Ei, vaikimisi väärtus: 5439 |
| andmebaasi | Amazon punanihe andmebaasi nimi. | Jah |
| kasutajanimi | Kasutaja, kellel on juurdepääs andmebaasi nimi.| Jah |
| parooli | Kasutajakonto parooli.| Jah |


## <a name="dataset-type-properties"></a>Andmekomplekti atribuudid

Jaotised ja atribuute määratlemine andmekomplektide täieliku loendi leiate artiklist [loomine andmekomplektide](data-factory-create-datasets.md) . Jaotiste, nt struktuuri, kättesaadavus ja poliitika on sarnased kõigi andmekomplekti tüüpide (Azure SQL, Azure'i bloobimälu, Azure'i tabeli jne).

Jaotise **typeProperties** erineb iga tüüpi andmekomplekti ja andmesalve olevate andmete asukoha teave. Andmekomplekti typeProperties jaotises tüüp **RelationalTable** (mis sisaldab Amazon punanihe andmekomplekti) on järgmised atribuudid

| Atribuut | Kirjeldus | Nõutav |
| -------- | ----------- | -------- |
| tableName | Amazon punanihe andmebaas, mis viitab lingitud tabeli nimi. | Ei, (kui **RelationalSource** **päring** on määratud) | 

## <a name="copy-activity-type-properties"></a>Tegevuse tüüp atribuutide kopeerimine

Jaotised ja atribuudid, mis on saadaval tegevuste määratlemiseks täieliku loendi leiate artiklist [Torujuhtmete loomine](data-factory-create-pipelines.md) . Näiteks nimi, kirjeldus, sisestus- ja väljundi tabelite ja poliitikate atribuudid on saadaval kõigi tegevuse tüüp. 

Tegevuse **typeProperties** jaotise saadaolevad atribuudid erinevad Teisalt iga tegevuse tüüp. Kopeeri tegevuste, need sõltuvad allikate ja neeldajate tüübid.

Kui allika Kopeeri tegevuse tüüp **RelationalSource** (mis sisaldab Amazon punanihe) järgmised atribuudid on saadaval jaotises typeProperties:

| Atribuut | Kirjeldus | Lubatud väärtus | Nõutav |
| -------- | ----------- | -------------- | -------- |
| päringu | Kohandatud päringu abil saate andmeid lugeda. | SQL-i päringustringi. Näide: valige * MyTable kaudu. | Ei, (kui **tableName** **andmekogumi** on määratud) | 

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

### <a name="type-mapping-for-amazon-redshift"></a>Amazon punanihe vastendust tüüp

Kui [andmete liikumine tegevusi](data-factory-data-movement-activities.md) artiklis mainitud, sooritab Kopeeri tegevuse automaatne tüüpide dokumenditeisenduste allikatüüpide valamu tüüpi järgmised kaheastmelise lähenemisviisi kaudu.

1. .Net-i tüüp teisendada kohalikke andmeallika tüübiga.
2. Teisendada .NET kohalikke valamu tüüp

Kui andmete teisaldamine Amazon punanihe kasutatakse järgmist vastendused Amazon punanihe tüüpi .NET tüübid.

Amazon punanihe tüüp | .Net-i põhiste tüüp
-------------------- | ---------------
SMALLINT | Int16
TÄISARV | Int32
SUUR TÄISARV | Int64
DECIMAL | Decimal
REAAL | Ühe
KAHEKORDNE TÄPSUS | Kahekordne
KAHENDMUUTUJA | String
CHAR | String
VARCHAR | String
KUUPÄEV | Kuupäev ja kellaaeg
AJATEMPLI | Kuupäev ja kellaaeg
TEKSTI | String



[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

## <a name="performance-and-tuning"></a>Jõudluse ja häälestamine  
Vaata [Kopeeri tegevuse jõudlus ja häälestamine juhend](data-factory-copy-activity-performance.md) Lisateavet peamised tegurid selle mõju jõudlust andmete liikumine (Kopeeri tegevus) Azure'i andmed Factory ja optimeerida seda mitmel viisil.

## <a name="next-steps"></a>Järgmised sammud
Lugege järgmisi artikleid: 
- Üksikasjalikud juhised loomine müügivõimaluste kopeerimine tegevuse [Kopeeri tegevuse õpetuse](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) . 