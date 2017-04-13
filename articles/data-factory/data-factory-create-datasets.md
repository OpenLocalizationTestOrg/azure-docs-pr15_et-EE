<properties 
    pageTitle="Azure'i andmed Factory andmekomplektide loomine | Microsoft Azure'i" 
    description="Saate teada, kuidas luua andmekomplektide Azure'i andmed Factory näited, kus kasutatakse atribuutide, nt offset ja anchorDateTime."
    keywords="andmekomplekti andmekomplekti näide loomine, offset näide"
    services="data-factory" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/13/2016" 
    ms.author="shlo"/>

# <a name="datasets-in-azure-data-factory"></a>Azure'i andmed Factory andmekogumite
Selles artiklis kirjeldatakse andmekogumite Azure'i andmed Factory ja sisaldab näiteid, nt offset, anchorDateTime ja andmebaaside offset ja laadi.

Kui loote andmekomplekt, loote viit andmed, mida soovite töödelda. Andmeid töödeldakse (sisend/väljund) tegevuse ja tegevuse sisaldub müügivõimaluste. Mõne Sisestuskeel andmekomplekti tähistab sisend tegevuse teel ja mõne väljundi andmekomplekti tegevuse väljundi.

Andmekomplektide tuvastada andmete erinevat tüüpi andmete salvestab, nt tabeleid, faile, kaustu ja dokumendid. Kui olete loonud andmekomplekt, saate seda kasutada koos müügivõimaluste tegevusi. Näiteks andmekomplekt võib olla mõni sisend andmekomplekti, on kopeerimine või HDInsightHive tegevus. Azure'i portaal annab teile visuaalse paigutus torujuhtmed ja andmete sisendina ja väljundid. Lühidalt, näete seosed ja sõltuvused oma torujuhtmete üle kõik allikate nii teate alati, kus on pärit andmeid ja kus saab.

Azure'i andmed Factory, saad andmete andmekogumi müügivõimaluste Kopeeri tegevuse abil.

> [AZURE.NOTE] Kui olete kasutanud Azure'i andmed Factory, lugege [artikleid Sissejuhatus rakendusse Azure'i andmed Factory](data-factory-introduction.md) Azure'i andmed Factory teenuse ülevaate. Vt [koostamiseks oma andmeid esimese factory](data-factory-build-your-first-pipeline.md) õpetuse oma esimese andmete factory loomiseks. Nende kahe artiklites saate tausta vajalik teave, et selles artiklis paremini mõista. 

## <a name="define-datasets"></a>Andmekomplektide määratlemine
Azure'i andmed Factory rakenduses andmekomplekt on määratletud järgmiselt: 


    {
        "name": "<name of dataset>",
        "properties": {
            "type": "<type of dataset: AzureBlob, AzureSql etc...>",
            "external": <boolean flag to indicate external data. only for input datasets>,
            "linkedServiceName": "<Name of the linked service that refers to a data store.>",
            "structure": [
                {
                    "name": "<Name of the column>",
                    "type": "<Name of the type>"
                }
            ],
            "typeProperties": {
                "<type specific property>": "<value>",
                "<type specific property 2>": "<value 2>",
            },
            "availability": {
                "frequency": "<Specifies the time unit for data slice production. Supported frequency: Minute, Hour, Day, Week, Month>",
                "interval": "<Specifies the interval within the defined frequency. For example, frequency set to 'Hour' and interval set to 1 indicates that new data slices should be produced hourly>"
            },
           "policy": 
            {      
            }
        }
    }

Järgmises tabelis on kirjeldatud ülal JSON atribuudid:   

| Atribuut | Kirjeldus | Nõutav | Vaikimisi |
| -------- | ----------- | -------- | ------- |
| Nimi | Andmekomplekti nimi. Nimede reeglid leiate [Azure'i andmed Factory - nime andmise reeglid](data-factory-naming-rules.md) . | Jah | NA |
| tüüp | Andmekomplekti tüüp. Määrata ühe Azure'i andmed Factory ei toeta (nt: AzureBlob, AzureSqlTable). <br/><br/>Üksikasjad leiate [Andmekomplekti tüüp](#Type) . | Jah | NA |
| struktuur | Andmekomplekti skeem<br/><br/>Lisateavet leiate teemast [Andmekomplekti struktuuri](#Structure) osa. | Ei. | NA |
| typeProperties | Vastab valitud atribuudid. Vt [Andmekomplekti tüüp](#Type) jaotisest toetatud failitüübid ja nende atribuudid. | Jah | NA |
| väliste | Kahendmuutujaga lipp määrata, kas andmekomplekt konkreetselt toodab andmete factory kohaletoimetamisel või mitte.  | Ei | FALSE | 
| kättesaadavus | Määratleb töötlemine akna või viilutamine mudeli andmekomplekti koostamiseks. <br/><br/>Lisateavet leiate teemast [Andmekomplekti-saadavus](#Availability) jaotis. <br/><br/>Mudeli, leiate teemast [koosoleku plaanimine ja täitmise](data-factory-scheduling-and-execution.md) artiklis viilutamine andmekomplekti kohta. | Jah | NA
| poliitika | Määratleb kriteeriumid või tingimusel, et peate täitma andmekomplekti sektorid. <br/><br/>Lisateavet leiate teemast [Andmekomplekti poliitika](#Policy) jaotis. | Ei | NA |

## <a name="dataset-example"></a>Andmekomplekti näide
Järgmises näites tähistab andmekomplekti nimega **MyTable** **Azure SQL-andmebaasi**tabelisse. 

    {
        "name": "DatasetSample",
        "properties": {
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties": 
            {
                "tableName": "MyTable"
            },
            "availability": 
            {
                "frequency": "Day",
                "interval": 1
            }
        }
    }

Võtke arvesse järgmist. 

- tüüp on seatud AzureSqlTable.
- atribuut tableName tüüp (teatud AzureSqlTable tüüp) väärtuseks MyTable.
- linkedServiceName viitab lingitud teenuse tüüpi AzureSqlDatabase. Vaadake järgmisi lingitud teenuse määratlus. 
- kättesaadavus sagedus väärtuseks päev ja intervall väärtuseks 1, mis tähendab, et sektorit valmistatakse iga päev.  

AzureSqlLinkedService defineeritakse järgmiselt:

    {
        "name": "AzureSqlLinkedService",
        "properties": {
            "type": "AzureSqlDatabase",
            "description": "",
            "typeProperties": {
                "connectionString": "Data Source=tcp:<servername>.database.windows.net,1433;Initial Catalog=<databasename>;User ID=<username>@<servername>;Password=<password>;Integrated Security=False;Encrypt=True;Connect Timeout=30"
            }
        }
    }

Klõpsake ülaltoodud JSON: 

- tüüp väärtuseks AzureSqlDatabase
- atribuut connectionString tüüp määrab teabe Azure'i SQL-andmebaasiga ühenduse loomiseks.  


Nagu näete, määratleb lingitud teenuse Azure'i SQL-andmebaasiga ühenduse loomiseks tehke järgmist. Andmekomplekti määratleb, milliseid tabeli kasutatakse müügivõimaluste tegevuse sisendit/väljundina. Tegevuste jaotisele oma [müügivõimaluste](data-factory-create-pipelines.md) JSON saate määrata, kas mõni sisestuskeel või väljund andmekomplekti kasutatakse andmekomplekti.


> [AZURE.IMPORTANT] Kui abil Azure'i andmed Factory on koostatud andmekomplekt, see märkida **välise**. Tavaliselt on see säte rakendub sisendina müügivõimaluste esimese tegevusega.   

## <a name="Type"></a>Andmekomplekti tüüp
Toetatud andmeallikate ja andmekomplekti tüübid on joondatud. Teemadest viidatud [Andmete liikumine tegevuste](data-factory-data-movement-activities.md#supported-data-stores) artikli teavet tüübid ja andmekomplektide konfigureerimine. Näiteks kui kasutate Azure SQL andmebaasis asuvate andmete, klõpsake loendis toetatud andmete poed üksikasjalikuma teabe vaatamiseks Azure'i SQL-andmebaasi.  

## <a name="Structure"></a>Andmekomplekti struktuur
Jaotise **struktuuri** määratletakse andmekomplekti skeemiga. See sisaldab nimesid ja andmetüübid veergude kogum.  Järgmises näites on andmekomplekti kolme veergu slicetimestamp, projectname ja kuvamisega ja need on tüüpi: String, String ja kümnendkohtade vastavalt.

    structure:  
    [ 
        { "name": "slicetimestamp", "type": "String"},
        { "name": "projectname", "type": "String"},
        { "name": "pageviews", "type": "Decimal"}
    ]

## <a name="Availability"></a>Andmekomplekti kättesaadavus
**Kättesaadavus** jaotisele andmekomplekt määratleb akna töötlemine (tunni, kord päevas, kord nädalas jne) või viilutamine mudeli andmekogumi. Andmekomplekti viilutamine ja sõltuvus mudeli kohta lisateabe saamiseks vt [plaanimine ja täitmise](data-factory-scheduling-and-execution.md) artikkel. 

Järgmises jaotises saadavuse määrab, et väljundi andmekomplekti toodetud tunni (või) sisendit andmekomplekti on saadaval tunnis:

    "availability": 
    {   
        "frequency": "Hour",        
        "interval": 1   
    }

Järgmises tabelis kirjeldatakse saate kasutada kättesaadavus jaotises Atribuudid. 

| Atribuut | Kirjeldus | Nõutav | Vaikimisi |
| -------- | ----------- | -------- | ------- |
| Frequency | Saate määrata ajaühiku andmekomplekti sektorit tootmiseks.<br/><br/>**Toetatud sagedus**: minut, tund, päeva-, nädala, kuu | Jah | NA |
| intervall | Saate määrata mitmekordne, sagedus<br/><br/>"Sagedus x intervall" määratleb sageduse toodetud sektorit.<br/><br/>Kui teil on vaja andmekomplekti abil saab tükeldatud tunnis, määrake **sätte sagedus** ja **intervall** **1** **tund**.<br/><br/>**Märkus:** Kui määrate sagedus minutiga, soovitame intervalli määramine vähemalt 15 | Jah | NA |
| Laadi | Saate määrata, kas sektorit tuleks esitada intervalli algus ja lõpp.<ul><li>StartOfInterval</li><li>EndOfInterval</li></ul><br/><br/>Kui sagedus on määratud kuu ja laadikomplekti EndOfInterval, valmistatakse sektorit kuu viimase päeva. Kui soovitud laad on määratud StartOfInterval, saadakse sektorit kuu esimesel päeval.<br/><br/>Kui sagedus on määratud päeva ja laadikomplekti EndOfInterval, sektorit on toodetud viimase päeva tunni.<br/><br/>Kui sagedus on seatud tund ja laadikomplekti EndOfInterval, sektorit on toodetud tunni lõpus. Näiteks tükk pl 1 – 2 pl perioodi sektorit on toodetud kell 2. | Ei | EndOfInterval |
| anchorDateTime | Määratleb absoluutne positsioon kasutatavad ajasti andmekomplekti sektorit piirmäärad arvutada. <br/><br/>**Märkus:** Kui soovitud AnchorDateTime on kuupäev osad, mis on rohkem kui Varundustöö ignoreeritakse täpsema osade. <br/><br/>Näiteks kui **intervall** on **tunni** (sagedus: tund ja intervall: 1) ja **AnchorDateTime** sisaldab **minutid ja sekundid**ja seejärel soovitud AnchorDateTime **minutid ja sekundid** osad ignoreeritakse. | Ei | 01/01/0001 |
| OFFSET | Kuuline ajavahemik, mille algus ja lõpp andmekomplekti kõik sektorid on nihutatud. <br/><br/>**Märkus:** Kui määratud on nii anchorDateTime ja offset, on tulemiks kombineeritud shift. | Ei | NA |

### <a name="offset-example"></a>Näide offset

Iga päev viilud selle start asemel vaikimisi keskööst kell 6. 

    "availability":
    {
        "frequency": "Day",
        "interval": 1,
        "offset": "06:00:00"
    }

**Sagedus** väärtuseks **päev** ja **intervall** väärtuseks **1** (üks kord päevas): kui soovite kell 6 asemel vaikimisi ajal esitatavad sektorit: 00;. Pidage meeles, et see aeg on UTC-kellaaeg. 

## <a name="anchordatetime-example"></a>anchorDateTime näide

**Näide:** 23 tundi andmekomplekti sektorid käivitamine 2007-04-19T08:00:00

    "availability": 
    {   
        "frequency": "Hour",        
        "interval": 23, 
        "anchorDateTime":"2007-04-19T08:00:00"  
    }

## <a name="offsetstyle-example"></a>OFFSET/laadi näide

Kui teil on vaja andmekomplekti igakuiselt kindla kuupäeva ja kellaaja (Oletame 3 iga kuu kell 08:00), peaks töötama **offset** sildi kuupäeva-ja selle kasutamine. 

    {
      "name": "MyDataset",
      "properties": {
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties": {
          "tableName": "MyTable"
        },
        "availability": {
          "frequency": "Month",
          "interval": 1,
          "offset": "3.08:10:00",
          "style": "StartOfInterval"
        }
      }
    }


## <a name="Policy"></a>Andmekomplekti poliitika

Andmekomplekti määratluses jaotises **poliitika** määratleb kriteeriumid või tingimusel, et peate täitma andmekomplekti sektorid.

### <a name="validation-policies"></a>Valideerimine poliitika

| Poliitika nimi | Kirjeldus | Rakendatud | Nõutav | Vaikimisi |
| ----------- | ----------- | ---------- | -------- | ------- |
| minimumSizeMB | Kinnitab, et andmed on **Azure Bloobivahemälu** vastab miinimummaht (megabaitides). | Azure'i bloobimälu | Ei | NA |
|minimumRows | Kinnitab, et loomine **SQL Azure'i andmebaasi** või **Azure'i tabeli** andmed sisaldavad minimaalse ridade arvu. | <ul><li>Azure'i SQL-andmebaas</li><li>Azure'i tabeli</li></ul> | Ei | NA

#### <a name="examples"></a>Näited

**minimumSizeMB:**

    "policy":
    
    {
        "validation":
        {
            "minimumSizeMB": 10.0
        }
    }

**minimumRows**

    "policy":
    {
        "validation":
        {
            "minimumRows": 100
        }
    }

### <a name="external-datasets"></a>Välise andmekomplektide

Välise andmekomplektide on need, mis ei tooda töötava müügivõimaluste andmete factory sisse. Kui andmekomplekti märgitud **välise**, võib mõjutada toimimist andmekomplekti sektorit kättesaadavus määratleda **ExternalData** poliitika. 

Kui abil Azure'i andmed Factory on koostatud andmekomplekt, see märkida **välise**. See säte rakendub üldiselt esimese tegevusest müügivõimaluste sisendeid juhul, kui kasutatakse tegevuste või müügivõimaluste Aheldamise. 

| Nimi | Kirjeldus | Nõutav | Vaikeväärtus  |
| ---- | ----------- | -------- | -------------- |
| dataDelay | Aeg viivitamine sisse antud sektorit välisandmed olemasolust. Näiteks kui andmeid peaks olema saadaval tunni, Välisandmed on saadaval ja vastavate sektorit on valmis kuvamiseks märkige saab viivitada dataDelay abil.<br/><br/>Kehtib ainult kuni praeguseni.  Näiteks kui see on 1:00 PM kohe ja selle väärtus on 10 minuti valideerimine algab 1:10 PM.<br/><br/>See säte ei mõjuta sektorid varem (sektorid sektorit lõppkellaaeg + dataDelay < nüüd) töödeldakse ilma viivituseta.<br/><br/>Aeg, mis on suurem kui 23:59 tundi on vaja määratud day.hours:minutes:seconds vormingus. Näiteks kui soovite määrata 24 tundi, ei kasuta 24:00:00; selle asemel kasutada 1.00:00:00. Kui kasutate 24:00:00, käsitletakse seda 24 päeva (24.00:00:00). 1 päeva ja 4 tundi, määrake 1:04:00:00. | Ei | 0 |
| retryInterval | Tõrke ootamine ajavahemik järgmise proovi uuesti proovida. Kehtib praegu; kui eelmine proovige nurjunud, oota see kaua pärast viimast proovida. <br/><br/>Kui see on 1:00 pm kohe, alustame esimesel proovida. Kui esimene valideerimine sisse lõpuleviimiseks kestus on 15 minutit toiming nurjus, järgmise proovi uuesti, on 1:00 + 1 min (kestus) + 1min (uuesti intervalli) = 1:02 pm. <br/><br/>Enne sektorid, ei ole viivitus. Uuesti juhtub kohe. | Ei | 00:00:01 (1 minut) | 
| retryTimeout | Aeg, mille puhul iga proovi uuesti.<br/><br/>Kui see atribuut on seatud 10 minuti, peab valideerimise 10 minuti jooksul lõpule viia. Kui see võtab rohkem kui 10 minuti teha valideerimist, ilmneb ajalõpp ning proovige uuesti.<br/><br/>Kui kõik katsed valideerimise ajalõpp, sektorit on märgitud ajalõppe. | Ei | 00:00:10 (10 minutit) |
| maximumRetry | Mitu korda väliste andmete olemasolu kontrollida. Suurim lubatud väärtus on 10. | Ei | 3 | 

## <a name="scoped-datasets"></a>Jäävates andmekomplektide
Saate luua kogumid, mis on rakendatud müügivõimaluste **andmekomplektide** atribuudi. Nende andmekomplektide saab kasutada ainult selles müügivõimaluste tegevuse aga mitte muude torujuhtmetes tegevuste. Järgmises näites määratleb müügivõimaluste koos kahe andmekomplektide - InputDataset-rdc ja OutputDataset-rdc - tulemas kasutada:  

> [AZURE.IMPORTANT] Jäävates andmekomplektide on toetatud ainult ühekordse torujuhtmete (**pipelineMode** **OneTime**määramine). Üksikasjad leiate [Onetime kohaletoimetamisel](data-factory-scheduling-and-execution.md#onetime-pipeline) .

    {
        "name": "CopyPipeline-rdc",
        "properties": {
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "BlobSource",
                            "recursive": false
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "InputDataset-rdc"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "OutputDataset-rdc"
                        }
                    ],
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1,
                        "style": "StartOfInterval"
                    },
                    "name": "CopyActivity-0"
                }
            ],
            "start": "2016-02-28T00:00:00Z",
            "end": "2016-02-28T00:00:00Z",
            "isPaused": false,
            "pipelineMode": "OneTime",
            "expirationTime": "15.00:00:00",
            "datasets": [
                {
                    "name": "InputDataset-rdc",
                    "properties": {
                        "type": "AzureBlob",
                        "linkedServiceName": "InputLinkedService-rdc",
                        "typeProperties": {
                            "fileName": "emp.txt",
                            "folderPath": "adftutorial/input",
                            "format": {
                                "type": "TextFormat",
                                "rowDelimiter": "\n",
                                "columnDelimiter": ","
                            }
                        },
                        "availability": {
                            "frequency": "Day",
                            "interval": 1
                        },
                        "external": true,
                        "policy": {}
                    }
                },
                {
                    "name": "OutputDataset-rdc",
                    "properties": {
                        "type": "AzureBlob",
                        "linkedServiceName": "OutputLinkedService-rdc",
                        "typeProperties": {
                            "fileName": "emp.txt",
                            "folderPath": "adftutorial/output",
                            "format": {
                                "type": "TextFormat",
                                "rowDelimiter": "\n",
                                "columnDelimiter": ","
                            }
                        },
                        "availability": {
                            "frequency": "Day",
                            "interval": 1
                        },
                        "external": false,
                        "policy": {}
                    }
                }
            ]
        }
    }