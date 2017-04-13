<properties 
    pageTitle="Teisaldage andmed Teradata | Azure'i andmed Factory" 
    description="Lisateavet Teradata konnektor jaoks andmete Factory teenus, mis võimaldab Teradata andmebaasist andmete teisaldamine" 
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

# <a name="move-data-from-teradata-using-azure-data-factory"></a>Teisaldage andmed Teradata Azure'i andmed Factory abil

See artikkel kirjeldab, kuidas saate Kopeeri tegevuse on Azure andmete factory andmete talletamiseks Teradata teise andmete põhjal. Selles artiklis põhineb [andmete liikumine tegevuste](data-factory-data-movement-activities.md) artikkel, mis annab ülevaate andmete liikumine Kopeeri tegevuste ja toetatud andmete poe kombinatsioone.

Andmete factory toetab ühenduse loomine Teradata asutusesisestest allikatest, Andmehalduslüüsi kaudu. Lisateavet Andmehalduslüüsi ja lüüsi häälestamise juhised leiate [jooksva andmete asukohad kohapealse ja pilveteenuse vahel](data-factory-move-data-between-onprem-and-cloud.md) artiklist. 

> [AZURE.NOTE]
Lüüs on vaja ka siis, kui selle Teradata on majutatud on Azure IaaS VM. Saate installida lüüsi, sama nimega andmesalve IaaS VM või muu VM kui lüüsi saate andmebaasiga ühenduse loomiseks. 

Andmete factory toetab ainult jooksva andmeid Teradata muid andmeid talletab, mitte muude andmete poed Teradata abil.

## <a name="installation"></a>Installimine 

Andmehalduslüüsi Teradata andmebaasiga ühenduse loomiseks, peate installida [.NET andmepakkuja Teradata jaoks](http://go.microsoft.com/fwlink/?LinkId=278886) sama süsteemi Andmehalduslüüsi.

> [AZURE.NOTE] Lugege teemat [tõrkeotsing lüüsi probleemide](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) näpunäiteid tõrkeotsingu ühenduse/lüüsi seotud probleemid. 

## <a name="copy-data-wizard"></a>Kopeerige andmed viisard
Lihtsaim viis loomiseks kopeerib andmete Teradata toetatud valamu andmete poe on Kopeerige andmed viisardi abil. Vt [õpetus: müügivõimaluste koopia viisardi abil luua](data-factory-copy-data-wizard-tutorial.md) loomiseks Kopeerige andmed viisardi abil müügivõimaluste kiirülevaate selgituse. 

Järgmises näites sisaldab valimi JSON määratlused kasutatavad loomiseks [Azure portaali](data-factory-copy-activity-tutorial-using-azure-portal.md) või [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) või [Azure PowerShelli](data-factory-copy-activity-tutorial-using-powershell.md)abil. Andmete kopeerimine Teradata Azure'i bloobimälu kuvatakse. Siiski saate andmeid kopeerida kõiki valamud märgitud [siin](data-factory-data-movement-activities.md#supported-data-stores) Azure'i andmed Factory Kopeeri tegevuse abil.   

### <a name="sample-copy-data-from-teradata-to-azure-blob"></a>Näide: Andmete kopeerimine Teradata Azure'i bloobimälu

See näide näitab, kuidas Teradata andmebaasist andmete kopeerimine mõnda Azure'i bloobimälu. Andmete saate siiski kopeeritud **otse** mis tahes valamud märgitud [siin](data-factory-data-movement-activities.md#supported-data-stores) Azure'i andmed Factory Kopeeri tegevuse abil.  
 
Valimi on andmete factory järgmised üksused:

1.  Lingitud teenuse tüüpi [OnPremisesTeradata](data-factory-onprem-teradata-connector.md#teradata-linked-service-properties).
2.  Lingitud teenuse tüüpi [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.  Mõne Sisestuskeel [andmekomplekti](data-factory-create-datasets.md) tüüpi [RelationalTable](data-factory-onprem-teradata-connector.md#teradata-dataset-type-properties).
4.  Mõne väljundi [andmekomplekti](data-factory-create-datasets.md) tüüpi [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties). 
4.  [Müügivõimaluste](data-factory-create-pipelines.md) Kopeeri tegevust, mis kasutab [RelationalSource](data-factory-onprem-teradata-connector.md#teradata-copy-activity-type-properties) ja [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

Valimi kopeerib andmete päringutulem Teradata andmebaas on bloobimälu tunnis. Nende proovide kasutatakse atribuutide JSON on kirjeldatud jaotiste jälgimise näidised. 

Esimese sammuna install andmehalduslüüsi. Juhised on artikli [jooksva andmete asukohad kohapealse ja pilveteenuse vahel](data-factory-move-data-between-onprem-and-cloud.md) .

**Lingitud Teradata teenus:**

    {
        "name": "OnPremTeradataLinkedService",
        "properties": {
            "type": "OnPremisesTeradata",
            "typeProperties": {
                "server": "<server>",
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


**Teradata Sisestuskeel andmekomplekti:**

Valimi eeldab, et olete loonud tabeli "MyTable" Teradata ja see sisaldab veerg nimega "ajatempli" kellaajaandmete sarja.

Säte "välise": true teatab andmete Factory teenus, et tabelis on väliste andmete factory ja ei esitata andmete factory toimingu.

    {
        "name": "TeradataDataSet",
        "properties": {
            "published": false,
            "type": "RelationalTable",
            "linkedServiceName": "OnPremTeradataLinkedService",
            "typeProperties": {
            },
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
        "name": "AzureBlobTeradataDataSet",
        "properties": {
            "published": false,
            "location": {
                "type": "AzureBlobLocation",
                "folderPath": "mycontainer/teradata/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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
                ],
                "linkedServiceName": "AzureStorageLinkedService"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }


**Torujuhe Kopeeri tegevuse abil:**

Tulemas on Kopeeri tegevust, mis on konfigureeritud kasutama sisend- ja andmekomplektide ja on plaanitud käivituma tunnis. Tulemas JSON määratlus **Reaallika** tüüp väärtuseks **RelationalSource** ja **valamu** tüüp väärtuseks **BlobSink**. SQL-päringu jaoks **päringu** atribuudi määratud valib andmete kopeerimiseks tunnis.

    {
        "name": "CopyTeradataToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "RelationalSource",
                            "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', SliceStart, SliceEnd)"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "TeradataDataSet"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobTeradataDataSet"
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
                    "name": "TeradataToBlob"
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z",
            "isPaused": false
        }
    }


## <a name="teradata-linked-service-properties"></a>Lingitud Teradata teenuse atribuudid

Järgmises tabelis on ära toodud JSON elementide teatud Teradata lingitud teenuse kirjeldus. 

Atribuut | Kirjeldus | Nõutav
-------- | ----------- | --------
tüüp | Atribuudi tüüp väärtuseks peab olema seatud: **OnPremisesTeradata** | Jah
Server | Teradata serveri nimi. | Jah
authenticationType | Teradata andmebaasiga ühenduse loomiseks kasutatud autentimise tüüp. Võimalikud väärtused on: Anonüümne, Basic ja Windows. | Jah
kasutajanimi | Määrake kasutajanimi, kui kasutate Basic või Windowsi autentimist. | Ei 
parooli | Määrake kasutajanimi jaoks määratud kasutajakonto parooli. | Ei 
gatewayName | Lüüsi, mis kohapealse Teradata andmebaasiga ühenduse loomiseks kasutama andmete Factory teenuse nimi. | Jah

Üksikasjad asutusesisese Teradata andmeallika mandaati määramise kohta leiate [säte mandaadid ja turvalisus](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) .

## <a name="teradata-dataset-type-properties"></a>Teradata andmekomplekti tüüp atribuudid

Jaotised ja atribuute määratlemine andmekomplektide täieliku loendi leiate artiklist [loomine andmekomplektide](data-factory-create-datasets.md) . Jaotiste, nt struktuuri, kättesaadavus ja Andmekomplekt JSON poliitika on sarnased kõigi andmekomplekti tüüpide (Azure SQL, Azure'i bloobimälu, Azure'i tabeli jne).

Jaotise **typeProperties** erineb iga tüüpi andmekomplekti ja andmesalve olevate andmete asukoha teave. Praegu ei ole tüüp atribuute, mis on toetatud Teradata andmekomplekti. 


## <a name="teradata-copy-activity-type-properties"></a>Teradata Kopeeri tegevuse tüüp atribuudid

Jaotised ja atribuudid, mis on saadaval tegevuste määratlemiseks täieliku loendi leiate artiklist [Torujuhtmete loomine](data-factory-create-pipelines.md) . Näiteks nimi, kirjeldus, sisestus- ja väljundi tabelite ja poliitikate atribuudid on saadaval kõigi tegevuse tüüp. 

Tegevuse typeProperties jaotise saadaolevad atribuudid erinevad teiselt iga tegevuse tüüp. Kopeeri tegevuste, need sõltuvad allikate ja neeldajate tüübid.

Kui allika tüüp **RelationalSource** (mis sisaldab Teradata) järgmised atribuudid on saadaval jaotises **typeProperties** :

Atribuut | Kirjeldus | Lubatud väärtus | Nõutav
-------- | ----------- | -------------- | --------
päringu | Kohandatud päringu abil saate andmeid lugeda. | SQL-i päringustringi. Näide: valige * MyTable kaudu. | Jah

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="type-mapping-for-teradata"></a>Tippige vastenduse Teradata jaoks

Nagu [andmete liikumine tegevusi](data-factory-data-movement-activities.md) artiklis mainitud, sooritab Kopeeri tegevuse automaatne tüüpide dokumenditeisenduste allikatüüpide valamu tüüpi järgmised 2-samm lähenemisviisi kaudu.

1. .Net-i tüüp teisendada kohalikke andmeallika tüübiga.
2. Teisendada .NET kohalikke valamu tüüp

Kui andmete teisaldamine Teradata kasutatakse järgmist vastendused Teradata tüüp, millest .NET tüüp.

Teradata andmebaas tüüp | .NET framework tüüp
----------------- | ---------------------------
Char | String
CLOB | String
Pilt | String
VarChar | String
VarGraphic | String
Bloobimälu | Byte]
Bait | Byte]
VarByte | Byte]
Suur täisarv | Int64
ByteInt | Int16
Decimal | Decimal
Kahekordne | Kahekordne
Täisarv | Int32
Arv | Kahekordne
SmallInt | Int16
Kuupäev | Kuupäev ja kellaaeg
Aeg | Kuuline ajavahemik
Ajavööndi aega | String
Ajatempli | Kuupäev ja kellaaeg
Ajatempli koos ajavöönd | DateTimeOffset
Intervall päeva | Kuuline ajavahemik
Intervall päeva tund | Kuuline ajavahemik
Minutid intervall päev | Kuuline ajavahemik
Teine intervall päeva | Kuuline ajavahemik
Intervall Hour | Kuuline ajavahemik
Intervall tund minut | Kuuline ajavahemik
Teine intervall Hour | Kuuline ajavahemik
Intervall minutid | Kuuline ajavahemik
Intervall minut teise | Kuuline ajavahemik
Intervall teise | Kuuline ajavahemik
Intervall aasta | String
Intervall aasta, kuu | String
Intervall kuu | String
Period(Date) | String
Period(Time) | String
Periood (aega ajavöönd) | String
Period(timestamp) | String
Periood (ajatempli koos ajavöönd) | String
XML-i | String

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

## <a name="performance-and-tuning"></a>Jõudluse ja häälestamine  
Vaata [Kopeeri tegevuse jõudlus ja häälestamine juhend](data-factory-copy-activity-performance.md) Lisateavet peamised tegurid selle mõju jõudlust andmete liikumine (Kopeeri tegevus) Azure'i andmed Factory ja optimeerida seda mitmel viisil.