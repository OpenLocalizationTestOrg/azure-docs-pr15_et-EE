<properties 
    pageTitle="Torujuhtmete loomine või ajakava, keti tegevuste andmete Factory | Microsoft Azure'i" 
    description="Siit saate teada, Azure'i andmed Factory teisaldada ja muuta andmete andmete müügivõimaluste loomiseks. Luua Andmepõhiste töövoo andes teabe kasutamiseks valmis." 
    keywords="andmete müügivõimaluste Andmepõhiste töövoo"
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
    ms.date="09/12/2016" 
    ms.author="shlo"/>

# <a name="pipelines-and-activities-in-azure-data-factory"></a>Torujuhtmed ja tegevusi Azure'i andmed Factory
See artikkel aitab teil mõista torujuhtmed ja Azure andmete Factory tegevuste ja kasutada neid koostada – lõpuni Andmepõhiste töövoogude andmete liikumine ja andmete töötlemise stsenaariumid.  

> [AZURE.NOTE] Selles artiklis eeldatakse, et olete läinud läbi [Sissejuhatus Azure'i andmed Factory](data-factory-introduction.md). Kui teil on hands-kohta – kogemus loomine andmete tehased, läheb läbi [koostamiseks oma andmeid esimese factory](data-factory-build-your-first-pipeline.md) õpetuse aitaks mõistate artiklit parem.  

## <a name="what-is-a-data-pipeline"></a>Mis on andmete müügivõimaluste?
**Müügivõimaluste** on loogiliselt seotud **tegevuste**rühm. Seda kasutatakse rühma tegevused, mis sooritavad tööülesande ühikuks. Torujuhtmete paremini mõista, peate mõista esimese tegevusega. 

## <a name="what-is-an-activity"></a>Mis on tegevust?
Tegevuste määratleda toiminguid sooritada oma andmete põhjal. Iga tegevuse võtab null või rohkem [andmekomplektide](data-factory-create-datasets.md) sisendina ja annab tulemiks ühe või mitme andmekomplektide väljundina. 

Näiteks võib kasutada Kopeeri tegevuse korraldab kopeerimine teise andmesalve ühe andmesalve andmetega. Samuti võib kasutada HDInsight taru tegevuse taru päringut, klõpsake mõnda Windows Azure Hdinsightiga kobar muuta andmete. Azure'i andmed Factory pakub mitmesuguseid [andmete teisendamiseks](data-factory-data-transformation-activities.md)ja [andmete liikumine](data-factory-data-movement-activities.md) tegevusi. Samuti võite luua kohandatud .NET tegevuse oma koodi käivitada. 

## <a name="sample-copy-pipeline"></a>Müügivõimaluste valimi kopeerimine
Järgmine näidis ettevalmistamisel on üks tegevuse tüüp **tegevuste** **Kopeeri** . Selles näites [kopeerimine tegevuse](data-factory-data-movement-activities.md) kopeerib andmed on Azure'i bloobimälu Azure SQL-andmebaasi. 

    {
      "name": "CopyPipeline",
      "properties": {
        "description": "Copy data from a blob to Azure SQL table",
        "activities": [
          {
            "name": "CopyFromBlobToSQL",
            "type": "Copy",
            "inputs": [
              {
                "name": "InputDataset"
              }
            ],
            "outputs": [
              {
                "name": "OutputDataset"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "BlobSource"
              },
              "sink": {
                "type": "SqlSink",
                "writeBatchSize": 10000,
                "writeBatchTimeout": "60:00:00"
              }
            },
            "Policy": {
              "concurrency": 1,
              "executionPriorityOrder": "NewestFirst",
              "retry": 0,
              "timeout": "01:00:00"
            }
          }
        ],
        "start": "2016-07-12T00:00:00Z",
        "end": "2016-07-13T00:00:00Z"
      }
    } 

Võtke arvesse järgmist.

- Jaotises tegevused on ainult üks tegevust, mille **Tüüp** on seatud **Kopeeri**.
- Sisestusmeetodi tegevuse jaoks on määratud **InputDataset** ja väljundi tegevuse jaoks on määratud **OutputDataset**.
- Jaotises **typeProperties** **BlobSource** on määratud Reaallika tüüp ja **SqlSink** on määratud valamu tüüp.

Täieliku lühiülevaade selle müügivõimaluste loomise kohta leiate teemast [õpetus: bloobimälu andmete kopeerimine SQL-andmebaasi](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md). 

## <a name="sample-transformation-pipeline"></a>Müügivõimaluste valimi teisendus
Järgmine näidis ettevalmistamisel on üks tegevuse tüüp **tegevuste** **HDInsightHive** . Selles näites [Hdinsightiga taru tegevuse](data-factory-hive-activity.md) abil andmeid mõne Azure'i bloobimälu käivitades mõne Azure Hdinsightiga Hadoopi kobar taru skripti faili. 

    {
        "name": "TransformPipeline",
        "properties": {
            "description": "My first Azure Data Factory pipeline",
            "activities": [
                {
                    "type": "HDInsightHive",
                    "typeProperties": {
                        "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                        "scriptLinkedService": "AzureStorageLinkedService",
                        "defines": {
                            "inputtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/inputdata",
                            "partitionedtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/partitioneddata"
                        }
                    },
                    "inputs": [
                        {
                            "name": "AzureBlobInput"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobOutput"
                        }
                    ],
                    "policy": {
                        "concurrency": 1,
                        "retry": 3
                    },
                    "scheduler": {
                        "frequency": "Month",
                        "interval": 1
                    },
                    "name": "RunSampleHiveActivity",
                    "linkedServiceName": "HDInsightOnDemandLinkedService"
                }
            ],
            "start": "2016-04-01T00:00:00Z",
            "end": "2016-04-02T00:00:00Z",
            "isPaused": false
        }
    }

Võtke arvesse järgmist. 

- Jaotises tegevused on ainult üks tegevust, mille **Tüüp** on seatud **HDInsightHive**.
- Taru skriptifail **partitionweblogs.hql**, talletatakse Azure storage konto (määratud scriptLinkedService, mida nimetatakse **AzureStorageLinkedService**) ja container **adfgetstarted** **skripti** kausta.
- **Määratleb** jaotise abil saate määrata edastatakse taru skripti taru konfiguratsiooni väärtustena käitusaja sätted (nt ${hiveconf: inputtable}, {hiveconf:partitionedtable} $).

Täieliku lühiülevaade selle müügivõimaluste loomise kohta leiate teemast [õpetus: koostada oma esimese müügivõimaluste Hadoopi kobar abil andmete töötlemise](data-factory-build-your-first-pipeline.md). 

## <a name="chaining-activities"></a>Aheldamise tegevuste
Kui teil on mitu tegevuste müügivõimaluste ja toimingu väljund pole mõne muu tegevuse sisendi, käivitada tegevuste paralleelselt kui sisendandmete sektorid tegevuste jaoks on valmis. 

Ühe tegevusega kui Sisestuskeel andmekomplekti muu tegevuse väljundi andmekomplekti, millel saab keti kaks tegevust. Tegevuste võib olla sama teel või muu torujuhtmetes. Teine tegevuse aktiveeritakse ainult siis, kui esimene lõpulejõudmist edukalt. 

Näiteks kaaluda järgmist:
 
1.  Müügivõimaluste P1 on tegevuse A1, mis nõuab väline Sisestuskeel andmekomplekti D1 ja aedvili **väljundi** andmekomplekti **D2**.
2.  Müügivõimaluste P2 on tegevuse A2 nõuab **Sisestuskeel** andmekomplekti **D2**, mis toodab väljundi andmekomplekti D3.
 
Selle stsenaariumi korral tegevuse A1 käivitab välisandmed on saadaval ja ajastatud-saadavus sagedus on jõudnud.  Tegevuse A2 käitatakse: D2 ajastatud sektorid muutuvad kättesaadavaks ja ajastatud-saadavus sagedus on jõudnud. Kui sektorite andmekogumis D2 ühte tõrget, A2 ei tööta jaoks sektorit, kuni see muutub kättesaadavaks.

Diagrammivaate:

![Kahe torujuhtmetes Aheldamise tegevuste](./media/data-factory-create-pipelines/chaining-two-pipelines.png)

Diagrammivaade pealt sama tulemas: 

![Aheldamise tegevuste sama teel](./media/data-factory-create-pipelines/chaining-one-pipeline.png)

Lisateavet leiate teemast [plaanimis- ja täitmise](#chaining-activities). 

## <a name="scheduling-and-execution"></a>Plaanimis- ja täitmise
Siiani on aru saanud torujuhtmed ja tegevused on. Saate ka uurinud kuidas need on määratletud ja üksikasjalik Azure'i andmed Factory tegevuste kuvamine. Nüüd vaatame kuidas neid saada täidetud. 

Müügivõimaluste on sisse lülitatud ainult vahel oma alguskellaaeg ja lõppkellaaeg. See ei käivitata algusaja enne või pärast lõppaega. Kui tulemas on peatatud, seda ei saada täidetakse sõltumata aeg algus ja lõpp. Müügivõimaluste, käivitamiseks, jaoks see peaks ei saa peatada. Tegelikult ei ole kohaletoimetamisel, mida saab teostada. See on tulemas tegevused, mida saada. Aga seda tulemas kontekstis. 

Vaadata [koosoleku plaanimine ja täitmise](data-factory-scheduling-and-execution.md) mõista, kuidas plaanimis- ja täitmise töötab Azure'i andmed Factory.

## <a name="create-pipelines"></a>Torujuhtmete loomine
Azure'i andmed Factory pakub erinevate võimaldada koostamine ja juurutamine torujuhtmete (mis sisaldavad omakorda ühe või mitme tegevuste seda). 

### <a name="using-azure-portal"></a>Azure'i portaalis
Saate andmeid Factory Editori Azure portaali loomiseks. Leiate [Azure'i andmed Factory (andmete Factory Editor) alustamine](data-factory-build-your-first-pipeline-using-editor.md) on lõpuni – ülevaade. 

### <a name="using-visual-studio"></a>Visual Studio abil 
Visual Studio abil saate koostamine ja juurutada torujuhtmete Azure'i andmed Factory. Leiate [Azure'i andmed Factory (Visual Studio) alustamine](data-factory-build-your-first-pipeline-using-vs.md) on lõpuni – ülevaade. 

### <a name="using-azure-powershell"></a>Azure'i PowerShelli abil
Azure PowerShelli abil saate luua torujuhtmete Azure'i andmed Factory. Öelge olete määratlenud müügivõimaluste JSON c:\DPWikisample.json faili. Saate üles laadida see oma Azure'i andmed Factory eksemplari nagu on näidatud järgmises näites:

    New-AzureRmDataFactoryPipeline -ResourceGroupName ADF -Name DPWikisample -DataFactoryName wikiADF -File c:\DPWikisample.json

On-lõpuni kiirtutvustus loomine andmete factory müügivõimaluste leiate [Azure'i andmed Factory (Azure'i PowerShelli) alustamine](data-factory-build-your-first-pipeline-using-powershell.md) . 

### <a name="using-net-sdk"></a>Kasutades .NET SDK
Saate luua ja juurutada liiga .NET SDK kaudu. See saab luua torustike programmiliselt. Lisateavet leiate teemast [loomine, haldamine, ja jälgida andmeid tehased programmiliselt](data-factory-create-data-factories-programmatically.md). 


### <a name="using-azure-resource-manager-template"></a>Azure'i ressursihaldur malli abil
Saate luua ja juurutada kohaletoimetamisel on Azure ressursihaldur malli abil. Lisateabe saamiseks lugege teemat [Alustamine Azure'i andmed Factory (Azure'i ressursihaldur)](data-factory-build-your-first-pipeline-using-arm.md). 

### <a name="using-rest-api"></a>REST API abil
Saate luua ja juurutada kasutades ka REST API-d müügivõimaluste. See saab luua torustike programmiliselt. Lisateavet leiate teemast [loomine või värskendamine müügivõimaluste](https://msdn.microsoft.com/library/azure/dn906741.aspx). 


## <a name="monitor-and-manage-pipelines"></a>Jälgimine ja haldamine torujuhtmete  
Kui müügivõimaluste juurutatakse, saate hallata ja jälgida oma torujuhtmete, sektorid ja töötab. Lisateavet leiate siit: [jälgimine ja haldamine torujuhtmete](data-factory-monitor-manage-pipelines.md).


## <a name="pipeline-json"></a>Müügivõimaluste JSON   
Andke meile lähemalt kohta, kuidas müügivõimaluste on määratletud JSON-vormingus. Müügivõimaluste üldise struktuuri näeb välja järgmine:

    {
        "name": "PipelineName",
        "properties": 
        {
            "description" : "pipeline description",
            "activities":
            [
    
            ],
            "start": "<start date-time>",
            "end": "<end date-time>"
        }
    }

**Tegevuste** võib olla üks või mitu selle määratletud tegevusi. Iga tegevuse on ülataseme järgmine struktuur:

    {
        "name": "ActivityName",
        "description": "description", 
        "type": "<ActivityType>",
        "inputs":  "[]",
        "outputs":  "[]",
        "linkedServiceName": "MyLinkedService",
        "typeProperties":
        {
    
        },
        "policy":
        {
        }
        "scheduler":
        {
        }
    }

Alljärgnevas tabelis on kirjeldatud atribuutide sees tegevuste ja müügivõimaluste JSON määratlusi.

Sildi | Kirjeldus | Nõutav
--- | ----------- | --------
Nimi | Tegevuse või tulemas nimi. Määrake nimi, mis tähistab tegevuste või müügivõimaluste konfigureeritud teha<br/><ul><li>Märkide arvu ülempiir: 260</li><li>Peab algama tähe arv või allkriipsu (_)</li><li>Jälgima märgid ei ole lubatud: ".", "+", "?", "/", "<",">", "*", "%", "ja", ":","\\"</li></ul> | Jah
kirjeldus | Tekst, mis kirjeldab tegevuste või müügivõimaluste kasutatakse | Jah
tüüp | Saate määrata tegevuse tüüp. Lugege artikleid [andmete liikumine](data-factory-data-movement-activities.md) ja [Andmete teisendus tegevus](data-factory-data-transformation-activities.md) eri tüüpi tegevuste jaoks. | Jah
sisendina | Kasutatavad tegevuse Sisestuskeel tabelid<br/><br/>ühe sisendtabel<br/>"sisendeid": [{"nimi": "inputtable1"}],<br/><br/>kahe Sisestuskeel tabeli <br/>"sisendeid": [{"nimi": "inputtable1"}, {"nimi": "inputtable2"}], | Jah
väljundid | Väljundi tabelite activity.// ühe väljundi tabelis kasutatud<br/>"väljundid": [{"nimi": "outputtable1"}],<br/><br/>kahe väljundi tabeli<br/>"väljundid": [{"nimi": "outputtable1"}, {"nimi": "outputtable2"}], | Jah
linkedServiceName | Lingitud teenust kasutada tegevuse nime. <br/><br/>Tegevus võib olla vaja, määrake lingitud teenus, mis ühendab nõutav Arvuta keskkonnas. | Jah HDInsight tegevuste ja Azure seadme Õppekeskuse paketi hinded tegevuse <br/><br/>Ei, kõik teised
typeProperties | TypeProperties jaotises Atribuudid sõltuvad tegevuse tüüp. | Ei
poliitika | Poliitika, mis mõjutavad tegevuse käitusaja toimimist. Kui see pole määratud, kasutatakse vaikepoliitikaid. | Ei
Alustamine | Kuupäev – alguskellaaeg tulemas. Peab olema [ISO](http://en.wikipedia.org/wiki/ISO_8601)-vormingus. Näide: 2014-10-14T16:32:41Z. <br/><br/>On võimalik kohalikku aega, näiteks mõne EST aja määrata. Siin on näide: "2016-02-27T06:00:00**-05:00**", mis on 6 AM eeldatav<br/><br/>Algus ja lõpp atribuutide määramine koos aktiivse perioodi tulemas. Väljundi sektorid on ainult toodetud selle aktiivse aja jooksul. | Ei<br/><br/>Kui määrate atribuudi end väärtus, määrake atribuudi start väärtus.<br/><br/>Algus- ja lõpuaegade nii saab tühja loomiseks. Määrake mõlemad väärtused müügivõimaluste käivitamiseks aktiivse perioodi määramiseks. Kui te ei määra algus- ja lõpuaegade müügivõimaluste loomisel saate neid hiljem Set-AzureRmDataFactoryPipelineActivePeriod cmdlet-käsu abil.
Lõpeta | Kuupäeva / kellaaja tulemas lõpetamiseks. Kui määratud peab olema ISO-vormingus. Näide: 2014-10-14T17:32:41Z <br/><br/>On võimalik kohalikku aega, näiteks mõne EST aja määrata. Siin on näide: "2016-02-27T06:00:00**-05:00**", mis on 6 AM eeldatav<br/><br/>Tulemas lõputult käivitamiseks määrata 9999-09-09 end atribuudi väärtust. | Ei <br/><br/>Kui määrate atribuudi start väärtus, määrake atribuudi end väärtus.<br/><br/>Vt märkuste atribuudi **käivitamine** .
isPaused | Kui see on seatud tõene tulemas ei tööta. Vaikimisi väärtus = false. Selle atribuudi abil saate lubada või keelata. | Ei 
ajasti | "scheduler" atribuuti kasutatakse määratlemiseks soovitud tegevuse plaanimine. Selle alamatribuutidega on samad, mis [kättesaadavus atribuudi andmekomplekt](data-factory-create-datasets.md#Availability). | Ei |   
| pipelineMode | Tulemas töötab plaanimiseks meetodit. Väärtused on lubatud: ajastatud (vaikeväärtus) ühekordselt.<br/><br/>"Ajastatud näitab tulemas töötab teatud ajavahemiku vastavalt oma aktiivse perioodi (algus ja lõpp aeg). "Ühekordselt" näitab, et tulemas töötab ainult üks kord. Kui loonud ühekordselt torujuhtmete ei saa muuta/uuendatud praegu. Üksikasjad ühekordselt sätte kohta leiate [Onetime kohaletoimetamisel](data-factory-scheduling-and-execution.md#onetime-pipeline) . | Ei | 
| expirationTime | Pärast loomist tulemas on kehtiv ja peaks jääma ettevalmistatud kestus. Kui see ei ole aktiivsisu, nurjus, või ootel käivitamist tulemas kustutatakse automaatselt, kui see jõuab aegumise aeg. | Ei | 
| andmekomplektide | Loendi andmekomplektide kasutavad tegevuste määratletud teel. Selle atribuudi saab määratleda kogumid, mis on omased selle müügivõimaluste ja andmete factory pole määratletud. Määratletud selle müügivõimaluste andmekomplektide saab kasutada ainult selles müügivõimaluste ja ei saa ühiskasutusse anda. Üksikasjad leiate [rakendatud andmekomplektide](data-factory-create-datasets.md#scoped-datasets) .| Ei |  
 

### <a name="policies"></a>Poliitika
Poliitika mõjutab käitusaja käitumine tegevuse, eriti siis, kui tabeli sektori töötlemise. Järgmine tabel sisaldab üksikasju.

Atribuut | Lubatud väärtused | Vaikeväärtus | Kirjeldus
-------- | ----------- | -------------- | ---------------
kokkulangevus | Täisarv <br/><br/>Max väärtus: 10 | 1 | Samaaegseid täitmised tegevuse arv.<br/><br/>See määratleb tegevuste paralleelselt täitmised, mis võib juhtuda, erinevate sektorite arvu. Näiteks kui tegevus tuleb läbida suure hulga olemasolevate andmete kokkulangevus suuremat väärtust kiirendab andmete töötlemise. 
executionPriorityOrder | NewestFirst<br/><br/>OldestFirst | OldestFirst | Määratleb andmete sektoritele, mis töödeldakse tellimine.<br/><br/>Näiteks, kui teil on 2 viilud (üks juhtub kell 4, ja teine kell 5) ja mõlemad on ootel täitmise. Kui seate executionPriorityOrder olema NewestFirst, töödeldakse esmalt sektorit kell 5. Samamoodi kui seate executionPriorityORder olevat OldestFIrst, siis sektorit kell 4 töötlemise. 
Proovige | Täisarv<br/><br/>Max väärtus võib olla 10 | 3 | Korduskatsed enne sektorit töötlemise on märgitud tõrge arv. Tegevuste täitmise jaoks andmete sektorit uuesti ülespoole arvu määratud proovi uuesti proovida. Pärast selle tehakse ning proovige uuesti.
ajalõpp | Kuuline ajavahemik | 00:00:00 | Ajalõpp tegevuse jaoks. Näide: 00:10:00 (tähendab ajalõpp 10 min)<br/><br/>Kui väärtus on määramata või on 0, aeg, mille on lõpmatu.<br/><br/>Kui andmete töötlemise ajal tükk ületab ajalõpu väärtust, see tühistatakse ja süsteem proovib uuesti töötlemine. Korduste arv sõltub atribuudi uuesti. Kui ilmneb ajalõpp, olekuks ajalõppe.
viivitus | Kuuline ajavahemik | 00:00:00 | Saate määrata viivituse, enne kui andmete töötlemise sektorit käivitamise.<br/><br/>Tegevuste jaoks andmete sektorit täitmise käivitatakse pärast viivituse on viimase oodatud täitmisaeg.<br/><br/>Näide: 00:10:00 (tähendab 10 minutit viivitust)
longRetry | Täisarv<br/><br/>Max väärtus: 10 | 1 | Pikk proovi uuesti üritab enne sektorit käivitamine on nurjus arv.<br/><br/>longRetry katsete on vahedega longRetryInterval. Et kui teil on vaja määrata ajavahemik proovi uuesti üritab kasutada longRetry. Kui määratud on nii proovi uuesti ja longRetry, iga longRetry katse sisaldab proovi uuesti üritab ja max katsete arv on uuesti * longRetry.<br/><br/>Kui näiteks järgmised sätted oleme tegevuse poliitika:<br/>Proovige: 3<br/>longRetry: 2<br/>longRetryInterval: 01:00:00<br/><br/>Oletame, on ainult üks sektorit käivitada (ootavad olek) ja tegevuste käivitamine nurjub iga kord. Esialgu oleks 3 järjestikust täitmise katsed. Pärast iga katse sektorit olek oleks uuesti. Pärast esimese 3 katsete ületavad, sektorit olek oleks LongRetry.<br/><br/>Pärast tund (st longRetryInteval's väärtus), oleks veel mõni 3 järjestikust täitmise katsed. Pärast seda, sektorit olek oleks nurjus ja pole veel korduskatsed oleks proovitakse. Seega 6 katsete tehtud üldist.<br/><br/>Kui mõni täitmise õnnestub, sektorit olek oleks valmis ja pole veel korduskatsed on proovitakse uuesti.<br/><br/>longRetry võib kasutada olukordades, kus sõltuvad andmete saabub-tarkadeks ajal või üldine keskkond on helbeline jaotises andmete töötlemine toimub. Sellisel juhul teha korduskatsed üksteise järel võib aidata ja tehes möödudes aeg tulemid soovitud väljund.<br/><br/>Sõna ettevaatlik: seadmine longRetry või longRetryInterval kõrge väärtused. Tavaliselt suuremad väärtused tähenda süsteemne küsimused. 
longRetryInterval | Kuuline ajavahemik | 00:00:00 | Pikk proovi uuesti üritab viivitus 


## <a name="next-steps"></a>Järgmised sammud

- Mõista [plaanimis- ja Azure andmete Factory täitmist](data-factory-scheduling-and-execution.md).  
- Lugege artiklit [andmete liikumine](data-factory-data-movement-activities.md) ja Azure andmete Factory [andmete teisendus võimalused](data-factory-data-transformation-activities.md)
- Aru [haldus ja Azure andmete Factory jälgimine](data-factory-monitor-manage-pipelines.md).
- [Luua ja juurutada oma esmalt kohaletoimetamisel](data-factory-build-your-first-pipeline.md). 
