<properties 
    pageTitle="Teisaldage andmed Amazon lihtsa salvestusteenus abil andmete Factory | Microsoft Azure'i" 
    description="Lisateavet andmete Amazon lihtsa salvestusteenus (S3) abil Azure'i andmed Factory liikuda." 
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
    ms.date="10/24/2016" 
    ms.author="jingwang"/>

# <a name="move-data-from-amazon-simple-storage-service-using-azure-data-factory"></a>Andmete teisaldamine alla Amazon lihtsa salvestusteenus Azure'i andmed Factory abil

See artikkel kirjeldab, kuidas saate Kopeeri tegevuse on Azure andmete factory andmete kaudu Amazon Simple Storage Service (S3) teise andmete talletamiseks. Selles artiklis põhineb [andmete liikumine tegevuste](data-factory-data-movement-activities.md) artikkel, mis annab ülevaate andmete liikumine ja toetatud andmeallikate/valamu andmete salvestab loendi Kopeeri tegevuse.  

Andmete factory toetab praegu ainult jooksva andmete Amazon S3 muid andmeid talletab, kuid mitte muid andmeid salvestab andmete teisaldamine Amazon S3.

## <a name="required-permissions"></a>Vajalikke õigusi

Amazon S3 andmeid kopeerida, veenduge, et teile on antud allpool õigused.

- **S3:GetObject** ja **s3:GetObjectVersion** Amazon S3 objekti toimingute
- **S3:ListBucket** ja **s3:ListAllMyBuckets** (kasutatakse ainult Kopeeri viisardis) Amazon S3 Ämber toimingute 

Amazon S3 permisions detail täieliku loendi leiate [Poliitika õiguste määramine](http://docs.aws.amazon.com/AmazonS3/latest/dev/using-with-s3-actions.html).

## <a name="copy-data-wizard"></a>Kopeerige andmed viisard
Lihtsaim viis loomiseks mis kopeerib andmete Amazon S3 on Kopeerige andmed viisardi abil. Vt [õpetus: müügivõimaluste koopia viisardi abil luua](data-factory-copy-data-wizard-tutorial.md) loomiseks Kopeerige andmed viisardi abil müügivõimaluste kiirülevaate selgituse. 

Järgmises näites sisaldab valimi JSON määratlused kasutatavad loomiseks [Azure portaali](data-factory-copy-activity-tutorial-using-azure-portal.md) või [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) või [Azure PowerShelli](data-factory-copy-activity-tutorial-using-powershell.md)abil. See näitab, kuidas andmeid kopeerida Azure'i bloobimälu Amazon S3. Siiski saate andmeid kopeerida kõiki valamud märgitud [siin](data-factory-data-movement-activities.md#supported-data-stores).

## <a name="sample-copy-data-from-amazon-s3-to-azure-blob"></a>Näide: Andmete kopeerimine Amazon S3 Azure'i bloobimälu
See näide näitab, kuidas mõni Amazon S3 andmete kopeerimine mõnda Azure'i bloobimälu. Andmete saate siiski kopeeritud **otse** mis tahes valamud märgitud [siin](data-factory-data-movement-activities.md#supported-data-stores) Azure'i andmed Factory Kopeeri tegevuse abil.  
 
Valimi on andmete factory järgmised üksused:

- Lingitud teenuse tüüpi [AwsAccessKey](#linked-service-properties).
- Lingitud teenuse tüüpi [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
- Mõne Sisestuskeel [andmekomplekti](data-factory-create-datasets.md) tüüpi [AmazonS3](#dataset-type-properties).
- Mõne väljundi [andmekomplekti](data-factory-create-datasets.md) tüüpi [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
- [Müügivõimaluste](data-factory-create-pipelines.md) Kopeeri tegevust, mis kasutab [FileSystemSource](#copy-activity-type-properties) ja [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

Valimi kopeerib andmete Amazon S3 on Azure'i bloobimälu tunnis. Nende proovide kasutatakse atribuutide JSON on kirjeldatud jaotiste jälgimise näidised. 

**Amazon S3 lingitud teenus**

    {
        "name": "AmazonS3LinkedService",
        "properties": {
            "type": "AwsAccessKey",
            "typeProperties": {
                "accessKeyId": "<access key id>",
                "secretAccessKey": "<secret access key>"
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

**Amazon S3 Sisestuskeel andmekomplekti**

Säte **"välise": true** andmete Factory teenuse teatab, et andmekomplekti on väliste andmete factory ja ei esitata andmete factory toimingu. Selle atribuudi väärtuseks true Sisestuskeel andmekomplekti, mis ei esitata tulemas tegevuse kohta.

    {
        "name": "AmazonS3InputDataset",
        "properties": {
            "type": "AmazonS3",
            "linkedServiceName": "AmazonS3LinkedService",
            "typeProperties": {
                "key": "testFolder/test.orc",
                "bucketName": "testbucket",
                "format": {
                    "type": "OrcFormat"
                }
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
                "folderPath": "mycontainer/fromamazons3/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

Tulemas on Kopeeri tegevust, mis on konfigureeritud kasutama sisend- ja andmekomplektide ja on plaanitud käivituma tunnis. Tulemas JSON määratlus **Reaallika** tüüp väärtuseks **FileSystemSource** ja **valamu** tüüp väärtuseks **BlobSink**. 
    
    {
        "name": "CopyAmazonS3ToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "FileSystemSource",
                            "recursive": true
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "AmazonS3InputDataset"
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
                    "name": "AmazonS3ToBlob"
                }
            ],
            "start": "2014-08-08T18:00:00Z",
            "end": "2014-08-08T19:00:00Z"
        }
    }



## <a name="linked-service-properties"></a>Lingitud teenuse atribuudid

Järgmine tabel sisaldab kirjeldus JSON elementide teatud Amazon S3 (**AwsAccessKey**) lingitud teenus.

| Atribuut | Kirjeldus | Lubatud väärtus | Nõutav |
| -------- | ----------- | -------- | ------- |  
| accessKeyID | Salajane kiirklahv ID-d. | string | Jah |
| secretAccessKey | Salajane kiirklahv, ise. | Krüptitud salajane string | Jah | 


## <a name="dataset-type-properties"></a>Andmekomplekti atribuudid

Jaotised ja atribuute määratlemine andmekomplektide täieliku loendi leiate artiklist [loomine andmekomplektide](data-factory-create-datasets.md) . Jaotiste, nt struktuuri, kättesaadavus ja poliitika on sarnased kõigi andmekomplekti tüüpide (Azure SQL, Azure'i bloobimälu, Azure'i tabeli jne).

Jaotise **typeProperties** erineb iga tüüpi andmekomplekti ja andmesalve olevate andmete asukoha teave. Andmekomplekti typeProperties jaotises tüüp **AmazonS3** (mis sisaldab Amazon S3 andmekomplekti) on järgmised atribuudid

| Atribuut | Kirjeldus | Lubatud väärtus | Nõutav |
| -------- | ----------- | -------- | ------ | 
| bucketName | S3 Ämber nimi. | String | Jah |
| klahv | S3 objekti võti. | String | Ei | 
| eesliite | Eesliide S3 objekti võti. Objektid, mille klahvid alustage seda URL-i eesliide on valitud. Kehtib ainult siis, kui võti on tühi. | String | Ei | 
| versioon | Versiooni S3 objekti kui S3 Versioonimine on lubatud. | String | Ei |  
| vormindamine | Järgmist tüüpi vormingus toetatud: **tekstivorming**, **AvroFormat**, **JsonFormat**, **OrcFormat**, **ParquetFormat**. Need väärtused jaotises Vorming atribuudi **Tüüp** väärtuseks. Vt lisateavet [Täpsustades tekstivorming](#specifying-textformat), [Täpsustades AvroFormat](#specifying-avroformat), [Täpsustades JsonFormat](#specifying-jsonformat), [Täpsustades OrcFormat](#specifying-orcformat)ja [Täpsustades ParquetFormat](#specifying-parquetformat) lõigud. Kui soovite kopeerida faile-on vahel (kahendarvu koopia) salvestab failipõhiste saate vahele jätta vorming jaotisele nii sisend- ja andmekomplekti määratlusi.| Ei
| tihendamine | Määrake tüüp ja taseme tihendamise andmete jaoks. Toetatud tüübid on: **GZip**, **Deflate**, ja **BZip2** ja toetatud tasemed on: **optimaalne** ja **kiireim**. Praegu ei toetata tihendussätted andmete **AvroFormat** või **OrcFormat**. Lisateavet leiate teemast [tihendamise tugiteenuste](#compression-support) jaotises.  | Ei |

> [AZURE.NOTE] klahvi + bucketName määrab asukohta, kus kopp juurkausta container S3 objektide ja klahvi on S3 objekti täielik tee S3 objekti.

### <a name="sample-dataset-with-prefix"></a>Valimi andmekomplekti eesliitega

    {
        "name": "dataset-s3",
        "properties": {
            "type": "AmazonS3",
            "linkedServiceName": "link- testS3",
            "typeProperties": {
                "prefix": "testFolder/test",
                "bucketName": "testbucket",
                "format": {
                    "type": "OrcFormat"
                }
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true
        }
    }

### <a name="sample-data-set-with-version"></a>Valimi andmehulga (koos versioon)

    {
        "name": "dataset-s3",
        "properties": {
            "type": "AmazonS3",
            "linkedServiceName": "link- testS3",
            "typeProperties": {
                "key": "testFolder/test.orc",
                "bucketName": "testbucket",
                "version": "WBeMIxQkJczm0CJajYkHf0_k6LhBmkcL",
                "format": {
                    "type": "OrcFormat"
                }
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true
        }
    }


### <a name="dynamic-paths-for-s3"></a>Dünaamiliste teed S3

Valimis, kasutame fikseeritud väärtuste Amazon S3 andmekomplekti võti ja bucketName atribuudid. 

    "key": "testFolder/test.orc",
    "bucketName": "testbucket",

Teil võib olla andmete Factory võti ja bucketName dünaamiliselt käitusajal süsteemi muutujad nagu SliceStart abil arvutada.

    "key": "$$Text.Format('{0:MM}/{0:dd}/test.orc', SliceStart)"
    "bucketName": "$$Text.Format('{0:yyyy}', SliceStart)"

Mida saab teha sama eesliite atribuut, mis Amazon S3 andmekomplekti. Vaadake teemat [andmete Factory funktsioonid ja süsteemi muutujat](data-factory-functions-variables.md) toetatud funktsioonid ja muutujate loendi. 


[AZURE.INCLUDE [data-factory-file-format](../../includes/data-factory-file-format.md)]
[AZURE.INCLUDE [data-factory-compression](../../includes/data-factory-compression.md)]


## <a name="copy-activity-type-properties"></a>Tegevuse tüüp atribuutide kopeerimine

Jaotised ja atribuudid, mis on saadaval tegevuste määratlemiseks täieliku loendi leiate artiklist [Torujuhtmete loomine](data-factory-create-pipelines.md) . Näiteks nimi, kirjeldus, sisestus- ja väljundi tabelite ja poliitikate atribuudid on saadaval kõigi tegevuse tüüp. 

Tegevuse **typeProperties** jaotise saadaolevad atribuudid erinevad Teisalt iga tegevuse tüüp. Kopeeri tegevuste, need sõltuvad allikate ja neeldajate tüübid.

Kui allikas Kopeeri tegevuse tüüp **FileSystemSource** (mis sisaldab Amazon S3), järgmised atribuudid on saadaval jaotises typeProperties:

| Atribuut | Kirjeldus | Lubatud väärtus | Nõutav |
| -------- | ----------- | -------------- | -------- | 
| Rekursiivsed | Saate määrata, kas soovite rekursiivselt loendis S3 Directory objektid. | Tõene/Väär | Ei | 

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

## <a name="performance-and-tuning"></a>Jõudluse ja häälestamine  
Vaata [Kopeeri tegevuse jõudlus ja häälestamine juhend](data-factory-copy-activity-performance.md) Lisateavet peamised tegurid selle mõju jõudlust andmete liikumine (Kopeeri tegevus) Azure'i andmed Factory ja optimeerida seda mitmel viisil.

## <a name="next-steps"></a>Järgmised sammud
Lugege järgmisi artikleid: 
- Üksikasjalikud juhised loomine müügivõimaluste kopeerimine tegevuse [Kopeeri tegevuse õpetuse](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) . 
