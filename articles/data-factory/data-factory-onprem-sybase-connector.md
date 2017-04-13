<properties 
    pageTitle="Teisaldage andmed Sybase'i | Azure'i andmed Factory" 
    description="Lisateavet andmete teisaldamiseks abil Azure'i andmed Factory Sybase'i andmebaasist." 
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

# <a name="move-data-from-sybase-using-azure-data-factory"></a>Teisaldage andmed Sybase'i Azure'i andmed Factory abil 

Selles artiklis kirjeldatakse, kuidas saate Kopeeri tegevuse on Azure andmete factory Sybase'i andmete teisaldamiseks teise andmesalve. Selles artiklis põhineb [andmete liikumine tegevuste](data-factory-data-movement-activities.md) artikkel, mis annab ülevaate andmete liikumine Kopeeri aktiivsusest ja toetatud andmete poe kombinatsioone.

Andmete Factory teenuse toetab ühenduse loomine Sybase asutusesisestest allikatest, kasutades Andmehalduslüüsi. Lisateavet Andmehalduslüüsi ja lüüsi häälestamise juhised leiate [jooksva andmete asukohad kohapealse ja pilveteenuse vahel](data-factory-move-data-between-onprem-and-cloud.md) artiklist. 

> [AZURE.NOTE]
> Lüüs on vaja ka siis, kui Sybase'i andmebaas on majutatud on Azure IaaS VM. Saate installida lüüsi, sama nimega andmesalve IaaS VM või muu VM kui lüüsi saate andmebaasiga ühenduse loomiseks. 

Andmete factory toetab praegu ainult jooksva andmete Sybase'i muid andmeid talletab, mitte muude andmete poed Sybase'i abil.

## <a name="installation"></a>Installimine

Andmehalduslüüsi Sybase'i andmebaasiga ühenduse loomiseks, peate installida [Sybase'i andmepakkuja](http://go.microsoft.com/fwlink/?linkid=324846) sama süsteemi Andmehalduslüüsi.

> [AZURE.NOTE] Lugege teemat [tõrkeotsing lüüsi probleemide](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) näpunäiteid tõrkeotsingu ühenduse/lüüsi seotud probleemid. 

## <a name="copy-data-wizard"></a>Kopeerige andmed viisard
Lihtsaim viis loomiseks kopeerib andmete Sybase'i andmebaasist toetatud valamu andmete poe on Kopeerige andmed viisardi abil. Vt [õpetus: müügivõimaluste koopia viisardi abil luua](data-factory-copy-data-wizard-tutorial.md) loomiseks Kopeerige andmed viisardi abil müügivõimaluste kiirülevaate selgituse. 

Järgmises näites sisaldab valimi JSON määratlused kasutatavad loomiseks [Azure portaali](data-factory-copy-activity-tutorial-using-azure-portal.md) või [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) või [Azure PowerShelli](data-factory-copy-activity-tutorial-using-powershell.md)abil. Sektordiagrammis kuvatakse andmete kopeerimine Azure'i bloobimälu Sybase'i andmebaas. Siiski saate andmeid kopeerida kõiki valamud märgitud [siin](data-factory-data-movement-activities.md#supported-data-stores) Azure'i andmed Factory Kopeeri tegevuse abil.   

## <a name="sample-copy-data-from-sybase-to-azure-blob"></a>Näide: Andmete kopeerimine Sybase'i Azure'i bloobimälu
See näide näitab, kuidas Sybase'i andmebaasist andmete kopeerimine mõnda Azure'i bloobimälu. Andmete saate siiski kopeeritud **otse** mis tahes valamud märgitud [siin](data-factory-data-movement-activities.md#supported-data-stores) Azure'i andmed Factory Kopeeri tegevuse abil.  
 
Valimi on andmete factory järgmised üksused:

1.  Lingitud teenuse tüüpi [OnPremisesSybase](data-factory-onprem-sybase-connector.md#sybase-linked-service-properties).
2.  Meeldis teenuse tüüpi [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.  Mõne Sisestuskeel [andmekomplekti](data-factory-create-datasets.md) tüüpi [RelationalTable](data-factory-onprem-sybase-connector.md#sybase-dataset-type-properties).
4.  Mõne väljundi [andmekomplekti](data-factory-create-datasets.md) tüüpi [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4.  [Müügivõimaluste](data-factory-create-pipelines.md) Kopeeri tegevust, mis kasutab [RelationalSource](data-factory-onprem-sybase-connector.md#sybase-copy-activity-type-properties) ja [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

Valimi kopeerib andmete päringutulem Sybase'i andmebaas on bloobimälu tunnis. Nende proovide kasutatakse atribuutide JSON on kirjeldatud jaotiste jälgimise näidised. 

Esimese sammuna install andmehalduslüüsi. Juhised on artikli [jooksva andmete asukohad kohapealse ja pilveteenuse vahel](data-factory-move-data-between-onprem-and-cloud.md) .

**Sybase'i lingitud teenus:**

    {
        "name": "OnPremSybaseLinkedService",
        "properties": {
            "type": "OnPremisesSybase",
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


**Sybase'i Sisestuskeel andmekomplekti:**

Valimi eeldab, et olete loonud tabeli "MyTable" Sybase'i ja see sisaldab veerg nimega "ajatempli" kellaajaandmete sarja.

Säte "välise": true teatab andmete Factory teenuse, et see andmekomplekti on väliste andmete factory ja ei esitata andmete factory toimingu. Teade, et lingitud teenuse **Tüüp** väärtuseks: **RelationalTable**. 
    
    {
        "name": "SybaseDataSet",
        "properties": {
            "type": "RelationalTable",
            "linkedServiceName": "OnPremSybaseLinkedService",
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
        "name": "AzureBlobSybaseDataSet",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/sybase/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

Tulemas on Kopeeri tegevust, mis on konfigureeritud kasutama sisend- ja andmekomplektide ja on plaanitud käivituma tunnis. Tulemas JSON määratlus **Reaallika** tüüp väärtuseks **RelationalSource** ja **valamu** tüüp väärtuseks **BlobSink**. SQL-päringu jaoks **päringu** atribuudi määratud klõpsab DBA andmed. Andmebaasi tabelisse orders.


    {
        "name": "CopySybaseToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "RelationalSource",
                            "query": "select * from DBA.Orders"
                        },
                        "sink": {
                            "type": "BlobSink"
                        }
                    },
                    "inputs": [
                        {
                            "name": "SybaseDataSet"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobSybaseDataSet"
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
                    "name": "SybaseToBlob"
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z"
        }
    }


## <a name="sybase-linked-service-properties"></a>Sybase'i lingitud teenuse atribuudid

Järgmises tabelis on ära toodud JSON elementide teatud Sybase'i lingitud teenuse kirjeldus.

Atribuut | Kirjeldus | Nõutav
-------- | ----------- | --------
tüüp | Atribuudi tüüp väärtuseks peab olema seatud: **OnPremisesSybase** | Jah
Server | Sybase'i serveri nimi. | Jah
andmebaasi | Sybase'i andmebaasi nimi. | Jah 
skeemi  | Andmebaasi skeemi nime. | Ei
authenticationType | Sybase'i andmebaasiga ühenduse loomiseks kasutatud autentimise tüüp. Võimalikud väärtused on: Anonüümne, Basic ja Windows. | Jah
kasutajanimi | Määrake kasutajanimi, kui kasutate Basic või Windowsi autentimist. | Ei
parooli | Määrake kasutajanimi jaoks määratud kasutajakonto parooli. |  Ei
gatewayName | Lüüsi, mis andmete Factory teenuse tuleks kasutada ühenduse loomiseks kohapealse Sybase'i andmebaas nimi. | Jah 

Üksikasjad kohapealse Sybase'i andmeallika mandaati määramise kohta leiate [säte mandaadid ja turvalisus](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) .

## <a name="sybase-dataset-type-properties"></a>Sybase'i andmekomplekti tüüp atribuudid

Jaotiste ja atribuute määratlemine andmekomplektide täieliku loendi leiate artiklist [loomine andmekomplektide](data-factory-create-datasets.md) . Jaotiste, nt struktuuri, kättesaadavus ja Andmekomplekt JSON poliitika on sarnased kõigi andmekomplekti tüüpide (Azure SQL, Azure'i bloobimälu, Azure'i tabeli jne).

Jaotise typeProperties erineb iga tüüpi andmekomplekti ja andmesalve olevate andmete asukoha teave. Andmekomplekti **typeProperties** jaotises tüüp **RelationalTable** (mis sisaldab Sybase'i andmekomplekti) on järgmised atribuudid.

Atribuut | Kirjeldus | Nõutav
-------- | ----------- | --------
tableName | Sybase'i andmebaas eksemplari, mis viitab lingitud tabeli nimi. | Ei, (kui **RelationalSource** **päring** on määratud)

## <a name="sybase-copy-activity-type-properties"></a>Sybase'i Kopeeri tegevuse tüüp atribuudid 

Jaotised ja atribuute määratlemine tegevused, vt [loomine välistrasside](data-factory-create-pipelines.md) artikkel täieliku loendi. Näiteks nimi, kirjeldus, sisendi ja väljundi tabelite ja poliitika atribuudid on saadaval kõigi tegevuse tüüp. 

Tegevuse typeProperties jaotise saadaolevad atribuudid erinevad teiselt iga tegevuse tüüp. Kopeeri tegevuste, need sõltuvad allikate ja neeldajate tüübid.

Kui allika tüüp **RelationalSource** (mis sisaldab Sybase'i) järgmised atribuudid on saadaval jaotises **typeProperties** :

Atribuut | Kirjeldus | Lubatud väärtus | Nõutav
-------- | ----------- | -------------- | --------
päringu | Kohandatud päringu abil saate andmeid lugeda. | SQL-i päringustringi. Näide: valige * MyTable kaudu. | Ei, (kui **tableName** **andmekogumi** on määratud)

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="type-mapping-for-sybase"></a>Sybase'i vastendust tüüp

Nagu [Andmete liikumine tegevuste](data-factory-data-movement-activities.md) artiklis mainitud, sooritab Kopeeri tegevuse automaatne tüüpide dokumenditeisenduste allikatüüpide valamu tüüpi järgmised 2-samm lähenemisviisi kaudu.

1. .Net-i tüüp teisendada kohalikke andmeallika tüübiga.
2. Teisendada .NET kohalikke valamu tüüp

Sybase'i toetab T-SQL-i ja T-SQL-i andmetüübid. Vastenduse tabeli SQL-i andmetüübid .NET tüüp, leiate [Azure'i SQL-i konnektor](data-factory-azure-sql-connector.md) artikkel.

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

## <a name="performance-and-tuning"></a>Jõudluse ja häälestamine  
Vaata [Kopeeri tegevuse jõudlus ja häälestamine juhend](data-factory-copy-activity-performance.md) Lisateavet peamised tegurid selle mõju jõudlust andmete liikumine (Kopeeri tegevus) Azure'i andmed Factory ja optimeerida seda mitmel viisil.