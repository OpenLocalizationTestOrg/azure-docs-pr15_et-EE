<properties 
    pageTitle="Teisaldage andmed kohapealse HDFS | Azure'i andmed Factory" 
    description="Lisateavet asutusesisese HDFS Azure'i andmed Factory abil andmeid teisaldada." 
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

# <a name="move-data-from-on-premises-hdfs-using-azure-data-factory"></a>Teisaldage andmed kohapealse HDFS Azure'i andmed Factory abil
Selles artiklis kirjeldatakse, kuidas saate Kopeeri tegevuse on Azure andmete factory on kohapealse HDFS andmete teisaldamiseks teise andmesalve. Selles artiklis põhineb [andmete liikumine tegevuste](data-factory-data-movement-activities.md) artikkel, mis annab ülevaate andmete liikumine Kopeeri tegevuste ja toetatud andmete poe kombinatsioonide.

Andmete factory toetab praegu ainult jooksva andmed on kohapealse HDFS muid andmeid talletab, kuid mitte muid andmeid salvestab andmete teisaldamine mõnda kohapealse HDFS.


## <a name="enabling-connectivity"></a>Ühenduvuse lubamine
Andmete Factory teenuse toetab ühenduse kohapealse HDFS Andmehalduslüüsi abil. Lisateavet Andmehalduslüüsi ja lüüsi häälestamise juhised leiate [jooksva andmete asukohad kohapealse ja pilveteenuse vahel](data-factory-move-data-between-onprem-and-cloud.md) artiklist. Isegi juhul, kui see on majutatud on Azure IaaS VM HDFS ühenduse lüüs abil. 

Lüüsi installimise ajal saate kohapealse samasse arvutisse või Azure VM nimega soovitud HDFS, soovitame lüüsi installimine eraldi seadme/Azure'i IaaS VM. Lüüsi eraldi arvutisse vähendab ressursside väide ja parandab jõudlust. Lüüsi eraldi arvutisse installimisel tuleks seade seadme abil soovitud HDFS juurde pääseda. 


## <a name="copy-data-wizard"></a>Kopeerige andmed viisard
Lihtsaim viis loomiseks mis kopeerib andmete kohapealse HDFS on Kopeerige andmed viisardi abil. Vt [õpetus: müügivõimaluste koopia viisardi abil luua](data-factory-copy-data-wizard-tutorial.md) loomiseks Kopeerige andmed viisardi abil müügivõimaluste kiirülevaate selgituse. 

Järgmised näited annavad valimi JSON määratlused kasutatavad loomiseks [Azure portaali](data-factory-copy-activity-tutorial-using-azure-portal.md) või [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) või [Azure PowerShelli](data-factory-copy-activity-tutorial-using-powershell.md)abil. Andmete kopeerimine mõnda Azure'i bloobimälu on kohapealse HDFS kuvatakse. Siiski saate andmeid kopeerida kõiki valamud märgitud [siin](data-factory-data-movement-activities.md#supported-data-stores) Azure'i andmed Factory Kopeeri tegevuse abil.

## <a name="sample-copy-data-from-on-premises-hdfs-to-azure-blob"></a>Näide: Andmete kopeerimine kohapealse HDFS Azure'i bloobimälu

See näide näitab, kuidas andmeid kopeerida mõne kohapealse HDFS Azure'i bloobimälu. Andmete saate siiski kopeeritud **otse** mis tahes valamud märgitud [siin](data-factory-data-movement-activities.md#supported-data-stores) Azure'i andmed Factory Kopeeri tegevuse abil.  
 
Valimi on andmete factory järgmised üksused:

1.  Lingitud teenuse tüüpi [OnPremisesHdfs](#hdfs-linked-service-properties).
2.  Lingitud teenuse tüüpi [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.  Mõne Sisestuskeel [andmekomplekti](data-factory-create-datasets.md) tüüpi [FileShare](#hdfs-dataset-type-properties).
4.  Mõne väljundi [andmekomplekti](data-factory-create-datasets.md) tüüpi [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4.  [Müügivõimaluste](data-factory-create-pipelines.md) Kopeeri tegevust, mis kasutab [FileSystemSource](#hdfs-copy-activity-type-properties) ja [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

Valimi kopeerib andmed on kohapealse HDFS mõne Azure'i bloobimälu tunnis. Nende proovide kasutatakse atribuutide JSON on kirjeldatud jaotiste jälgimise näidised. 

Esimese sammuna häälestada andmehalduslüüsi. [Jooksva andmete asukohad kohapealse ja pilveteenuse vahel](data-factory-move-data-between-onprem-and-cloud.md) artiklis antud juhiseid. 

**HDFS lingitud teenus** Selles näites kasutatakse Windowsi autentimist. Vaadake [HDFS lingitud teenuse](#hdfs-linked-service-properties) osa autentimine, saate kasutada erinevat tüüpi. 

    {
        "name": "HDFSLinkedService",
        "properties":
        {
            "type": "Hdfs",
            "typeProperties":
            {
                "authenticationType": "Windows",
                "userName": "Administrator",
                "password": "password",
                "url" : "http://<machine>:50070/webhdfs/v1/",
                "gatewayName": "mygateway"
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

**HDFS sisend andmekomplekti** Tuvastatavate viitab HDFS kausta DataTransfer/UnitTest /. Tulemas kopeeritakse kõik failid selles kaustas sihtkohta. 

Säte "välise": "true" teatab andmete Factory teenus, et andmekomplekti on väliste andmete factory ja ei esitata andmete factory toimingu.
    
    {
        "name": "InputDataset",
        "properties": {
            "type": "FileShare",
            "linkedServiceName": "HDFSLinkedService",
            "typeProperties": {
                "folderPath": "DataTransfer/UnitTest/"
            },
            "external": true,
            "availability": {
                "frequency": "Hour",
                "interval":  1
            }
        }
    }




**Azure'i bloobimälu väljund andmekomplekti**

Andmed on kirjutatud uue bloobimälu tunnis (sagedus: tund, intervall: 1). Kausta tee on bloobimälu hinnatakse dünaamiliselt algusaja sellest osast, mis töötlemise põhjal. Kausta tee kasutab aasta, kuu, päev ja tunni osad algusaeg.

    {
        "name": "OutputDataset",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/hdfs/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

Tulemas on Kopeeri tegevust, mis on konfigureeritud kasutama neid sisend- ja andmekomplektide ja on plaanitud käivituma tunnis. Tulemas JSON määratlus **Reaallika** tüüp väärtuseks **FileSystemSource** ja **valamu** tüüp väärtuseks **BlobSink**. SQL-päringu jaoks **päringu** atribuudi määratud valib andmete kopeerimiseks tunnis.
    
    {
        "name": "pipeline",
        "properties":
        {
            "activities":
            [
                {
                    "name": "HdfsToBlobCopy",
                    "inputs": [ {"name": "InputDataset"} ],
                    "outputs": [ {"name": "OutputDataset"} ],
                    "type": "Copy",
                    "typeProperties":
                    {
                        "source":
                        {
                            "type": "FileSystemSource"
                        },
                        "sink":
                        {
                            "type": "BlobSink"
                        }
                    },
                    "policy":
                    {
                        "concurrency": 1,
                        "executionPriorityOrder": "NewestFirst",
                        "retry": 1,
                        "timeout": "00:05:00"
                    }
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z"
        }
    }



## <a name="hdfs-linked-service-properties"></a>HDFS lingitud teenuse atribuudid

Järgmises tabelis on ära toodud kirjeldus JSON elementide HDFS-i kohaste lingitud teenuse.

| Atribuut | Kirjeldus | Nõutav |
| -------- | ----------- | -------- | 
| tüüp | Atribuudi tüüp väärtuseks peab olema seatud: **Hdfs** | Jah | 
| URL-i | URL-i funktsiooni HDFS | Jah |
| encryptedCredential | [Uus-AzureRMDataFactoryEncryptValue](https://msdn.microsoft.com/library/mt603802.aspx) väljundit Accessi mandaati. | Ei |
| Kasutajanimi | Kasutajanimi Windowsi autentimist. | Jah (Windowsi autentimine)
| parooli | Parooli Windowsi autentimist. | Jah (Windowsi autentimine)
| authenticationType | Windowsi või Anonüümsetelt. | Jah |
| gatewayName | Lüüsi, mis ühenduse loomiseks soovitud HDFS tuleks kasutada andmete Factory teenuse nimi. | Jah |   

Üksikasjad kohapealse HDFS mandaadi seadmise kohta leiate [säte mandaadid ja turvalisus](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) .

### <a name="using-anonymous-authentication"></a>Anonüümne autentimine abil

    {
        "name": "hdfs",
        "properties":
        {
            "type": "Hdfs",
            "typeProperties":
            {
                "authenticationType": "Anonymous",
                "userName": "hadoop",
                "url" : "http://<machine>:50070/webhdfs/v1/",
                "gatewayName": "mygateway"
            }
        }
    }


### <a name="using-windows-authentication"></a>Windowsi autentimist kasutades
    
    {
        "name": "hdfs",
        "properties":
        {
            "type": "Hdfs",
            "typeProperties":
            {
                "authenticationType": "Windows",
                "userName": "Administrator",
                "password": "password",
                "url" : "http://<machine>:50070/webhdfs/v1/",
                "gatewayName": "mygateway"
            }
        }
    }
 


## <a name="hdfs-dataset-type-properties"></a>HDFS andmekomplekti atribuudid

Jaotised ja atribuute määratlemine andmekomplektide täieliku loendi leiate artiklist [loomine andmekomplektide](data-factory-create-datasets.md) . Jaotiste, nt struktuuri, kättesaadavus ja Andmekomplekt JSON poliitika on sarnased kõigi andmekomplekti tüüpide (Azure SQL, Azure'i bloobimälu, Azure'i tabeli jne).

Jaotise **typeProperties** erineb iga tüüpi andmekomplekti ja andmesalve olevate andmete asukoha teave. Andmekomplekti typeProperties jaotises tüüp **FileShare** (mis sisaldab HDFS andmekomplekti) on järgmised atribuudid

Atribuut | Kirjeldus | Nõutav
-------- | ----------- | --------
folderPath | Kausta tee. Näide:`myfolder`<br/><br/>Kasutada paomärki "\" stringi erimärkide jaoks. Näide: folder\subfolder, määratlege kausta\\\\alamkaust ja d:\samplefolder, määratlege d:\\\\samplefolder.<br/><br/>Saate selle atribuudi kombineerida **partitionBy** on sektori alusel kausta tee algus ja lõpp kuupäeva korda. | Jah
faili nimi | Kui soovite kindlat tüüpi failiga kausta viidata tabeli **folderPath** Määrake faili nimi. Kui te ei määra mis tahes väärtus selle atribuudi tabeli osutab kõik failid kaustast.<br/><br/>Kui faili nimi soovitud väljundi andmekogumi pole määratud, loodud faili nimi oleks järgmine järgmises vormingus: <br/><br/>Andmed. <Guid>txt (nt:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt | Ei
partitionedBy | partitionedBy saab määrata dünaamiline folderPath, filename kellaajaandmete sarja. Näide: folderPath parameetriga andmete iga tund. | Ei
fileFilter | Määrake filtri, et valida alamhulk on folderPath failide asemel kõik failid kasutada. <br/><br/>Väärtused on lubatud: `*` (mitut märki) ja `?` (üksikmärki).<br/><br/>Näited 1:`"fileFilter": "*.log"`<br/>Näide 2:`"fileFilter": 2014-1-?.txt"`<br/><br/>**Märkus**: fileFilter kehtib ka Sisestuskeel FileShare andmekomplekti | Ei
| vormindamine | Järgmist tüüpi vormingus toetatud: **tekstivorming**, **AvroFormat**, **JsonFormat**, **OrcFormat**ja **ParquetFormat**. Need väärtused jaotises Vorming atribuudi **Tüüp** väärtuseks. Vt lisateavet [Täpsustades tekstivorming](#specifying-textformat), [Täpsustades AvroFormat](#specifying-avroformat), [Täpsustades JsonFormat](#specifying-jsonformat), [Täpsustades OrcFormat](#specifying-orcformat)ja [Täpsustades ParquetFormat](#specifying-parquetformat) lõigud. Kui soovite kopeerida faile-on vahel (kahendarvu koopia) salvestab failipõhiste saate vahele jätta vorming jaotisele nii sisend- ja andmekomplekti määratlusi. | Ei 
| tihendamine | Määrake tüüp ja taseme tihendamise andmete jaoks. Toetatud tüübid on: **GZip**, **Deflate**, ja **BZip2** ja toetatud tasemed on: **optimaalne** ja **kiireim**. Praegu ei toetata tihendussätted andmete **AvroFormat** või **OrcFormat**. Lisateavet leiate teemast [tihendamise tugiteenuste](#compression-support) jaotises.  | Ei |



> [AZURE.NOTE] faili nimi ja fileFilter ei saa korraga kasutada.


### <a name="using-partionedby-property"></a>PartionedBy atribuudi abil

Nagu eelmises jaotises, saate määrata dünaamiline folderPath, aeg sarja andmete partitionedBy failinimi. Saate seda teha andmete Factory makrod ja süsteemi muutuja SliceStart, SliceEnd, mis näitavad loogilise tähtaeg antud andmete sektorit. 

Aja sarja andmekomplektide kohta lisateabe saamiseks plaanimine ja sektoritele, leiate artiklitest [Andmekomplektide loomise](data-factory-create-datasets.md), [plaanimine ja täitmise](data-factory-scheduling-and-execution.md)ja [Torujuhtmete loomine](data-factory-create-pipelines.md) . 

#### <a name="sample-1"></a>Näide 1:

    "folderPath": "wikidatagateway/wikisampledataout/{Slice}",
    "partitionedBy": 
    [
        { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
    ],

Selles näites {sektorit} on asendatud funktsiooniga määratud andmete Factory süsteemi muutuja SliceStart vormingus (YYYYMMDDHH) väärtuse. Funktsiooni SliceStart viitab alguskellaaeg sektorit. Funktsiooni folderPath on erinev iga sektorit. Näide: wikidatagateway/wikisampledataout/2014100103 või wikidatagateway/wikisampledataout/2014100104.

#### <a name="sample-2"></a>Näide 2:

    "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
    "fileName": "{Hour}.csv",
    "partitionedBy": 
     [
        { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
        { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } }, 
        { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }, 
        { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } } 
    ],

Selles näites ekstraktitakse aasta, kuu, päev ja kellaaeg SliceStart üheks eraldi muutujate, mida kasutatakse atribuutide folderPath ja faili nimi.

[AZURE.INCLUDE [data-factory-file-format](../../includes/data-factory-file-format.md)]   
[AZURE.INCLUDE [data-factory-compression](../../includes/data-factory-compression.md)]

## <a name="hdfs-copy-activity-type-properties"></a>HDFS Kopeeri tegevuse tüüp atribuudid

Jaotised ja atribuudid, mis on saadaval tegevuste määratlemiseks täieliku loendi leiate artiklist [Torujuhtmete loomine](data-factory-create-pipelines.md) . Näiteks nimi, kirjeldus, sisestus- ja väljundi tabelite ja poliitikate atribuudid on saadaval kõigi tegevuse tüüp. 

Tegevuse typeProperties jaotise saadaolevad atribuudid erinevad teiselt iga tegevuse tüüp. Kopeeri tegevuste, need sõltuvad allikate ja neeldajate tüübid.

Kopeeri tegevuste, kui allika tüüp **FileSystemSource** järgmised atribuudid on saadaval jaotises typeProperties:

**FileSystemSource** toetab järgmised atribuudid.

| Atribuut | Kirjeldus | Lubatud väärtus | Nõutav |
| -------- | ----------- | -------------- | -------- |
| Rekursiivsed | Näitab, kas andmed on kirjutuskaitstud rekursiivselt sub kaustad või ainult määratud kausta. | True, False (vaikeväärtus)| Ei |

[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="performance-and-tuning"></a>Jõudluse ja häälestamine  
Vaata [Kopeeri tegevuse jõudlus ja häälestamine juhend](data-factory-copy-activity-performance.md) Lisateavet peamised tegurid selle mõju jõudlust andmete liikumine (Kopeeri tegevus) Azure'i andmed Factory ja optimeerida seda mitmel viisil.

