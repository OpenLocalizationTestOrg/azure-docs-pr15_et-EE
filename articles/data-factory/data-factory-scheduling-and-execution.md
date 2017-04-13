<properties
    pageTitle="Plaanimis- ja täitmise abil andmete Factory | Microsoft Azure'i"
    description="Siit saate teada, plaanimis- ja täitmise aspektide Azure'i andmed Factory rakenduse mudel."
    services="data-factory"
    documentationCenter=""
    authors="spelluru"
    manager="jhubbard"
    editor="monicar"/>

<tags
    ms.service="data-factory"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="spelluru"/>

# <a name="data-factory-scheduling-and-execution"></a>Andmete Factory plaanimis- ja täitmise
Selles artiklis selgitatakse plaanimis- ja täitmise aspektide Azure'i andmed Factory rakenduse mudel. 

## <a name="prerequisites"></a>Eeltingimused
Selles artiklis eeldatakse, et te põhialuste andmete Factory rakenduse mudel põhimõtet, sh tegevuse, torustike, lingitud teenuste ja andmekomplektide. Azure'i andmed Factory põhimõtted, lugege järgmisi artikleid:

- [Andmete Factory tutvustus](data-factory-introduction.md)
- [Torujuhtmete](data-factory-create-pipelines.md)
- [Andmekomplektide](data-factory-create-datasets.md) 

## <a name="schedule-an-activity"></a>Ajakava tegevus

Saate määrata tegevuse JSON jaotises ajasti tegevuse korduva ajakava. Näiteks saate ajastada tegevus tunnis järgmiselt:

    "scheduler": {
        "frequency": "Hour",
        "interval": 1
    },  

![Ajasti näide](./media/data-factory-scheduling-and-execution/scheduler-example.png)

Nagu on näidatud joonisel, täpsustades tegevuse ajakava loob sarja langema Windowsi. Pyllähdys windows on fikseeritud suurus, kattuvad, sidusaid ajavahemike sarja. Need loogiline langema aknad tegevuse nimetatakse *tegevuse windows*.

Praegu täidesaatva tegevuse akna, pääsete tegevuse akna [WindowStart](data-factory-functions-variables.md#data-factory-system-variables) ja [WindowEnd](data-factory-functions-variables.md#data-factory-system-variables) süsteemi muutujad tegevuse JSON-ga seostatud ajavahemik. Saate kasutada nende muutujate mitmel otstarbel oma tegevuse JSON. Näiteks saate nende sisend- ja andmekomplektide tähistav kellaajaandmeid sarja andmete valimiseks.

Atribuudi **ajasti** toetab sama andma atribuudina **kättesaadavus** Andmekomplekt. Üksikasjad leiate [andmekomplekti kättesaadavus](data-factory-create-datasets.md#Availability) . Näited: veebisaidil kindla kellaaja offset ajastamine või režiimi joondamiseks töötlemine algusesse või lõppu tegevuse akna intervalli seadmine.

Saate määrata tegevuse **scheduler** atribuutide, kuid see atribuut pole **kohustuslik**. Kui määrate atribuudi, see peab ühtima sagedus, saate määrata väljundi andmekomplekti määratlus. Väljundi andmekomplekti on praegu, mis juhib ajakava, seega peate looma ka väljundi andmekomplekti isegi juhul, kui tegevuse pole mis tahes väljundi loomiseks. Kui tegevuse ei võta mis tahes sisendit, võite jätkata Sisestuskeel andmekomplekti loomine.

## <a name="time-series-datasets-and-data-slices"></a>Aja sarja andmekomplektide ja andmete sektorid

Sarja kellaajaandmeid on üksteise andmepunktide, mis koosneb tavaliselt järjestikuste mõõtmed, mis ajavahemiku jooksul. Levinud sarja kellaajaandmeid näiteks andur ja rakenduse telemeetria andmeid.

Andmete Factory, kus saate protsessi aja batched mood toimingute sarja andmete töötab. Tavaliselt on korduva sagedus, kus sisendandmete saabub ja väljasta andmeid tuleb esitada. See sagedus on kujundatud määramisega **kättesaadavus** andmekomplekti järgmiselt:

    "availability": {
      "frequency": "Hour",
      "interval": 1
    },

Iga üksuse andmete tarbitud ja toodetud tegevuse run nimetatakse andmete sektorit. Järgmisel joonisel on kujutatud üks sisendväärtuste andmekomplekti tegevuse ja üks väljund andmekomplekti. Nende andmekomplektide on **kättesaadavus** väärtuseks mõne tunni sagedus.

![Kättesaadavus ajasti](./media/data-factory-scheduling-and-execution/availability-scheduler.png)

Kord tunnis andmete sektorid sisend- ja andmekogumi eelmisel joonisel. Diagramm näitab kolme Sisestuskeel sektoritele, mis on valmis töötlemiseks. 10-11 EL tegevuse on pooleli, toodavad 10-11 EL väljundi sektorit.

Pääsete seotud praeguse sektorit, koostatud andmekogumis JSON muutujate [SliceStart](data-factory-functions-variables.md#data-factory-system-variables) ja [SliceEnd](data-factory-functions-variables.md#data-factory-system-variables)ajavahemik.

Praegu andmete Factory nõuab, et märgitud täpselt Ajasta vastab määratud väljundi andmekomplekti **kättesaadavus** ajakava. Seetõttu **WindowStart**, **WindowEnd**, **SliceStart**ja **SliceEnd** alati kaardil sama aja jooksul ja ühe väljundi sektorit.

Erinevate atribuudid saadaval jaotises saadavuse kohta leiate lisateavet teemast [loomine andmekomplektide](data-factory-create-datasets.md).

## <a name="move-data-from-sql-database-to-blob-storage"></a>Andmete teisaldamine SQL-i andmebaasist bloobimälu

Vaatame panna mõned asjad koos ja tegelikkuses loomisega müügivõimaluste kopeerib andmete Azure'i SQL-andmebaasi tabelist Azure'i bloobimälu tunnis.

**Sisend: Andmekomplekti Azure SQL-andmebaas**

    {
        "name": "AzureSqlInput",
        "properties": {
            "published": false,
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties": {
                "tableName": "MyTable"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true,
            "policy": {}
        }
    }


**Sagedus** on määratud **tunni** ja **intervall** väärtuseks **1** jaotises saadavuse.

**Väljund: Azure'i bloobimälu salvestusruumi andmekomplekti**

    {
        "name": "AzureBlobOutput",
        "properties": {
            "published": false,
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "folderPath": "mypath/{Year}/{Month}/{Day}/{Hour}",
                "format": {
                    "type": "TextFormat"
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
                            "format": "%M"
                        }
                    },
                    {
                        "name": "Day",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "%d"
                        }
                    },
                    {
                        "name": "Hour",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "%H"
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


**Sagedus** on määratud **tunni** ja **intervall** väärtuseks **1** jaotises saadavuse.



**Aktiivne: Kopeeri tegevus**

    {
        "name": "SamplePipeline",
        "properties": {
            "description": "copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "name": "AzureSQLtoBlob",
                    "description": "copy activity",
                    "typeProperties": {
                        "source": {
                            "type": "SqlSource",
                            "sqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 100000,
                            "writeBatchTimeout": "00:05:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "AzureSQLInput"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobOutput"
                        }
                    ],
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    }
                }
            ],
            "start": "2015-01-01T08:00:00Z",
            "end": "2015-01-01T11:00:00Z"
        }
    }


Valimi näitab tegevuse ajakava ja andmekomplekti kättesaadavus jaotiste määratud mõne tunni sagedus. Valimi näitab, kuidas saate kasutada **WindowStart** ja **WindowEnd** tegevuse asjakohaste andmete valimiseks käivitamine ja kopeerida see on bloobimälu koos vastav **folderPath**. **FolderPath** on parameetriga on eraldi kausta iga tund.

Kui kolme sektorite vahel 8 – 11 EL käivitada, Azure'i SQL-andmebaasi andmed on järgmine:

![Valimi sisendit](./media/data-factory-scheduling-and-execution/sample-input-data.png)

Pärast tulemas kasutajaks, sisustatakse Azure'i bloobimälu järgmiselt:

-   Faili mypath/2015/1/1/8/andmed. &lt;Guid&gt;.txt andmetega

            10002345,334,2,2015-01-01 08:24:00.3130000
            10002345,347,15,2015-01-01 08:24:00.6570000
            10991568,2,7,2015-01-01 08:56:34.5300000

    > [AZURE.NOTE] &lt;GUID&gt; on asendatud funktsiooniga on tegelik guid. Näide faili nimi: Data.bcde1348-7620-4f93-bb89-0eed3455890b.txt
-   Faili mypath/2015/1/1/9/andmeid. &lt;Guid&gt;.txt andmetega:

            10002345,334,1,2015-01-01 09:13:00.3900000
            24379245,569,23,2015-01-01 09:25:00.3130000
            16777799,21,115,2015-01-01 09:47:34.3130000
-   Faili mypath/2015/1/1/10/andmeid. &lt;Guid&gt;.txt, kus pole andmeid.


## <a name="active-period-for-pipeline"></a>Müügivõimaluste aktiivse perioodi

Aktiivse perioodi **algus** ja **lõpp** atribuutide seadmine määratud müügivõimaluste kontseptsiooni [loomise torujuhtmete](data-factory-create-pipelines.md) sisse.

Saate varem müügivõimaluste aktiivse perioodi alguskuupäev. Andmete Factory arvutab (tagasi täited) kõigi andmete sektorid minevikus ja algab töötlemine neid automaatselt.

## <a name="parallel-processing-of-data-slices"></a>Paralleelne andmete sektorid töötlemine
Saate konfigureerida tagasi sisestatud andmete sektorid käivitamiseks paralleelselt, seades **kokkulangevus** atribuuti tegevuse JSON poliitika osas. Selle atribuudi kohta leiate lisateavet teemast [loomine torujuhtmete](data-factory-create-pipelines.md).

## <a name="rerun-a-failed-data-slice"></a>Nurjunud andmete sektorit uuesti 
Saate jälgida täitmise sektorid rikkalike, visuaalne viisil. Üksikasjad leiate [jälgimine ja haldamine torujuhtmete abil Azure portaali labad](data-factory-monitor-manage-pipelines.md) või [rakenduse jälgimine ja haldamine](data-factory-monitor-manage-app.md) .

Kaaluge järgmises näites, kus on kuvatud kaks tegevust. Nυuded1 annab tulemiks kellaaja sarja andmekomplekti koos sektorid väljundina sisendina tarbitud Activity2 andes lõplik väljund aja sarja andmekomplekti.

![Nurjunud sektorit](./media/data-factory-scheduling-and-execution/failed-slice.png)

Diagramm näitab, et kokku kolme tehtud sektoritele, ilmnes tõrge toodavad 9-10 AM sektorit Dataset2. Andmete Factory saab automaatselt sõltuvus aja sarja andmekogumi. Seetõttu ei käivitu tegevuse käivitamine 9-10: 00 järgmise etapi sektorit.

Andmete Factory jälgimise ja halduse tööriistad võimaldavad üldiseks diagnostikalogid jaoks nurjunud sektorit jaoks probleemi põhjuseks leidmist ja paranda. Pärast on fikseeritud probleem, saate hõlpsalt käivitada, andes nurjunud sektorit tegevuse alustada. Käivitage uuesti ja mõista olekus üleminekud andmete sektorid kohta lisateabe saamiseks vt [jälgimine ja haldamine torujuhtmete abil Azure portaali labad](data-factory-monitor-manage-pipelines.md) või [rakenduse jälgimine ja haldamine](data-factory-monitor-manage-app.md).

Pärast seda, kui te käivitage uuesti 9-10: 00 sektorit **Dataset2**, algab andmete Factory Käivita jaoks 9-10 AM sõltuvad sektorit lõplik andmekomplekti.

![Käivita uuesti nurjunud sektorit](./media/data-factory-scheduling-and-execution/rerun-failed-slice.png)

## <a name="run-activities-in-a-sequence"></a>Järjestikuseks tegevuste käivitamine
Nimega Sisestuskeel andmekomplekti muu tegevuse väljundi andmekomplekti ühe tegevuse seadmisega saab keti kaks tegevuste (käivitamine ühe tegevuse teise järel). Tegevuste võib olla sama teel või muu torujuhtmetes. Teine tegevuse aktiveeritakse ainult siis, kui esimene lõpule viidud.

Näiteks kaaluda järgmist:

1.  Müügivõimaluste P1 on tegevuse A1 nõuab väline Sisestuskeel andmekomplekti D1, mis toodab väljundi andmekomplekti D2.
2.  Müügivõimaluste P2 on tegevuse A2 nõuab sisend andmekomplekti D2, mis toodab väljundi andmekomplekti D3.

Selle stsenaariumi korral tegevused A1 ja A2 on erinevate torujuhtmetes. Tegevuse A1 käivitub, kui välisandmed on saadaval ja ajastatud-saadavus sagedus on jõudnud. Tegevuse A2 käitatakse: D2 ajastatud sektorid muutuvad kättesaadavaks ja ajastatud-saadavus sagedus on jõudnud. Kui sektorite andmekogumis D2 ühte tõrget, A2 ei tööta jaoks sektorit, kuni see muutub kättesaadavaks.

Diagrammi vaate näeks välja nagu järgmisel joonisel:

![Kahe torujuhtmetes Aheldamise tegevuste](./media/data-factory-scheduling-and-execution/chaining-two-pipelines.png)

Nagu varem mainitud, võib tegevuste sama kohaletoimetamisel. Diagrammivaate nii tegevustega sama tulemas näeks välja nagu järgmisel joonisel:

![Aheldamise tegevuste sama teel](./media/data-factory-scheduling-and-execution/chaining-one-pipeline.png)

### <a name="copy-sequentially"></a>Kopeerige sirgjoontega
On võimalik käivitada mitme eksemplari toimingute üksteise järel järjestikku/tellitud viisil. Näiteks peate kaks Kopeeri tegevuste koos järgmised sisendandmete väljundi andmekomplektide müügivõimaluste (CopyActivity1 ja CopyActivity2):   

CopyActivity1

Sisestusmeetodi: andmekomplekti. Väljundi: Dataset2.

CopyActivity2

Sisestusmeetodi: Dataset2.  Väljundi: Dataset3.

CopyActivity2 läheks kui selle CopyActivity1 on edukalt käivitada ja Dataset2 on saadaval ainult.

Siin on näide müügivõimaluste JSON.

    {
        "name": "ChainActivities",
        "properties": {
            "description": "Run activities in sequence",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "BlobSource"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "copyBehavior": "PreserveHierarchy",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "Dataset1"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "Dataset2"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00"
                    },
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    },
                    "name": "CopyFromBlob1ToBlob2",
                    "description": "Copy data from a blob to another"
                },
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "BlobSource"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "Dataset2"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "Dataset3"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00"
                    },
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    },
                    "name": "CopyFromBlob2ToBlob3",
                    "description": "Copy data from a blob to another"
                }
            ],
            "start": "2016-08-25T01:00:00Z",
            "end": "2016-08-25T01:00:00Z",
            "isPaused": false
        }
    }

Pange tähele, et näites väljundi andmekomplekti esimese Kopeeri tegevuse (Dataset2) määratud sisendina teise tegevuste jaoks. Seetõttu teise tegevuse töötab ainult siis, kui esimese tegevusega andmekomplekti väljund on valmis.  

Näites CopyActivity2 võib olla erinevad sisendit, nt Dataset3, kuid teie määratud Dataset2 CopyActivity2, et nii tegevuse ei tööta kuni CopyActivity1 lõpetab. Näiteks:

CopyActivity1

Sisestusmeetodi: Dataset1. Väljundi: Dataset2.

CopyActivity2

Sisendina: Dataset3, Dataset2. Väljundi: Dataset4.

    {
        "name": "ChainActivities",
        "properties": {
            "description": "Run activities in sequence",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "BlobSource"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "copyBehavior": "PreserveHierarchy",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "Dataset1"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "Dataset2"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00"
                    },
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    },
                    "name": "CopyFromBlobToBlob",
                    "description": "Copy data from a blob to another"
                },
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "BlobSource"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "Dataset3"
                        },
                        {
                            "name": "Dataset2"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "Dataset4"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00"
                    },
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    },
                    "name": "CopyFromBlob3ToBlob4",
                    "description": "Copy data from a blob to another"
                }
            ],
            "start": "2017-04-25T01:00:00Z",
            "end": "2017-04-25T01:00:00Z",
            "isPaused": false
        }
    }


Pange tähele, et näites on kaks Sisestuskeel andmekomplektide määratud teine koopia tegevuste. Kui mitme sisendeid on määratud, kasutatakse ainult esimese Sisestuskeel andmekomplekti andmete kopeerimine, kuid muud andmekomplektide kasutatakse sõltuvused. CopyActivity2 hakkaks alles pärast seda, kui järgmised tingimused on täidetud:

- CopyActivity1 on töö lõpetanud ja Dataset2 on saadaval. Tuvastatavate ei kasutata andmete kopeerimisel Dataset4. See toimib ainult ajastamise sõltuvus CopyActivity2 jaoks.   
- Dataset3 on saadaval. Tuvastatavate tähistab sihtkohta kopeerida andmeid.  



## <a name="model-datasets-with-different-frequencies"></a>Mudeli andmekomplektide erineva sagedusega

Näidised, sagedus sisend- ja andmekomplektide ja tegevuste ajakava aknas olid samad. Mõnel juhul vajavad võimalust andes väljundi erinevas sagedust ühe või mitme sisendeid sagedusega. Andmete Factory toetab modelleerimine järgmisi olukordi.

### <a name="sample-1-produce-a-daily-output-report-for-input-data-that-is-available-every-hour"></a>Näide 1: Esitama iga päev väljundi aruande sisendandmete, mis on saadaval iga tunni jaoks

Kaaluge võimalust, kus te sisestate andmeid anduritelt saadaval tunnis Azure'i bloobimälu stsenaarium. Soovite koostada igapäevane liitväärtuse aruande statistikaga, nt keskmise ja maksimaalse miinimum [andmete Factory taru tegevuste](data-factory-hive-activity.md)päev.

Siin on, kuidas saate selle stsenaariumi abil andmete Factory mudel.

**Sisestuskeel andmekomplekti**

Antud päeva kõrvaldatakse kord tunnis Sisestuskeel faile kausta. Kättesaadavus sisendi on seatud **Hour** (sagedus: tund, intervall: 1).

    {
      "name": "AzureBlobInput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
          "partitionedBy": [
            { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
            { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "%M"}},
            { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "%d"}}
          ],
          "format": {
            "type": "TextFormat"
          }
        },
        "external": true,
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }

**Väljundi andmekomplekti**

Ühe väljundfail luuakse iga päev soovitud päeva kausta. Väljundi kättesaadavus on määratud **päeval** (sagedus: päev ja intervall: 1).


    {
      "name": "AzureBlobOutput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
          "partitionedBy": [
            { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
            { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "%M"}},
            { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "%d"}}
          ],
          "format": {
            "type": "TextFormat"
          }
        },
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

**Aktiivne: taru tegevusest müügivõimaluste**

Taru skripti saab parameetrid, mis kasutavad **WindowStart** muutuja, nagu on näidatud järgmises koodilõigu *kuupäeva ja kellaaja* asjakohane teave. Taru skript kasutab muutuja päeva õige kaust andmeid laadida ja käivitada koondamine genereerida väljundi.

        {  
            "name":"SamplePipeline",
            "properties":{  
            "start":"2015-01-01T08:00:00",
            "end":"2015-01-01T11:00:00",
            "description":"hive activity",
            "activities": [
                {
                    "name": "SampleHiveActivity",
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
                    "linkedServiceName": "HDInsightLinkedService",
                    "type": "HDInsightHive",
                    "typeProperties": {
                        "scriptPath": "adftutorial\\hivequery.hql",
                        "scriptLinkedService": "StorageLinkedService",
                        "defines": {
                            "Year": "$$Text.Format('{0:yyyy}',WindowStart)",
                            "Month": "$$Text.Format('{0:%M}',WindowStart)",
                            "Day": "$$Text.Format('{0:%d}',WindowStart)"
                        }
                    },
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1
                    },          
                    "policy": {
                        "concurrency": 1,
                        "executionPriorityOrder": "OldestFirst",
                        "retry": 2,
                        "timeout": "01:00:00"
                    }
                 }
             ]
           }
        }

Järgmisel joonisel on esitatud andmed – sõltuvus seisukohast seda stsenaariumi.

![Andmete sõltuvus](./media/data-factory-scheduling-and-execution/data-dependency.png)

Väljundi sektorit iga päev sõltub 24 tunni sektoritele kaudu soovitud Sisestuskeel andmekomplekti. Andmete Factory arvutab neid järgnevusi uurides välja sisendandmete sektoritele, mis jäävad sama aja jooksul nimega väljundi sektorit tuleb luua automaatselt. Kui mõni 24 Sisestuskeel sektorite pole saadaval, andmete Factory ootab Sisestuskeel sektorit iga päev tegevuse käivitada enne olema valmis.


### <a name="sample-2-specify-dependency-with-expressions-and-data-factory-functions"></a>Näide 2: Määrake sõltuvus andmete Factory funktsioone ja avaldised

Vaatame teise stsenaariumi. Oletame, et teil on taru tegevust, mis töötleb kahte Sisestuskeel kogumid. Üks neist on uued andmed iga päev, kuid üks neist saab uued andmed, iga nädal. Oletame, et soovite teha ühenduse üle kahe sisendina ja aedvili väljund iga päev.

Milliseid andmeid Factory on lihtne lähenemisviisi automaatselt arvud välja õige sisendi sektorid töödelda viies väljund andmete sektorit aja jooksul ei tööta.

Määrake, et iga tegevuste käivitamine, andmete Factory tuleks kasutada eelmisel nädalal andmete sektorit nädala Sisestuskeel andmekomplekti. Kasutate Azure'i andmed Factory funktsioone nagu on näidatud järgmises koodilõigu rakendada seda käitumist.

**Input1: Azure'i bloobimälu**

Esimene on Azure'i bloobimälu, värskendamist iga päev.

    {
      "name": "AzureBlobInputDaily",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
          "partitionedBy": [
            { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
            { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "%M"}},
            { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "%d"}}
          ],
          "format": {
            "type": "TextFormat"
          }
        },
        "external": true,
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

**Input2: Azure'i bloobimälu**

Input2 on Azure'i bloobimälu, iga nädal värskendamist.

    {
      "name": "AzureBlobInputWeekly",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
          "partitionedBy": [
            { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
            { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "%M"}},
            { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "%d"}}
          ],
          "format": {
            "type": "TextFormat"
          }
        },
        "external": true,
        "availability": {
          "frequency": "Day",
          "interval": 7
        }
      }
    }

**Väljund: Azure'i bloobimälu**

Ühe väljundfail luuakse iga päev kausta päeva. Väljundi kättesaadavus väärtuseks **päev** (sagedus: päev, intervall: 1).

    {
      "name": "AzureBlobOutputDaily",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "mycontainer/myfolder/{Year}/{Month}/{Day}/",
          "partitionedBy": [
            { "name": "Year", "value": {"type": "DateTime","date": "SliceStart","format": "yyyy"}},
            { "name": "Month","value": {"type": "DateTime","date": "SliceStart","format": "%M"}},
            { "name": "Day","value": {"type": "DateTime","date": "SliceStart","format": "%d"}}
          ],
          "format": {
            "type": "TextFormat"
          }
        },
        "availability": {
          "frequency": "Day",
          "interval": 1
        }
      }
    }

**Aktiivne: taru tegevusest müügivõimaluste**

Taru tegevuse kulub kaks sisendina ja annab tulemiks on väljundi sektorit iga päev. Saate määrata iga päev väljundi sektorit sõltub eelmise nädala Sisestuskeel sektorit nädala sisendi järgmiselt.

    {  
        "name":"SamplePipeline",
        "properties":{  
        "start":"2015-01-01T08:00:00",
        "end":"2015-01-01T11:00:00",
        "description":"hive activity",
        "activities": [
          {
            "name": "SampleHiveActivity",
            "inputs": [
              {
                "name": "AzureBlobInputDaily"
              },
              {
                "name": "AzureBlobInputWeekly",
                "startTime": "Date.AddDays(SliceStart, - Date.DayOfWeek(SliceStart))",
                "endTime": "Date.AddDays(SliceEnd,  -Date.DayOfWeek(SliceEnd))"  
              }
            ],
            "outputs": [
              {
                "name": "AzureBlobOutputDaily"
              }
            ],
            "linkedServiceName": "HDInsightLinkedService",
            "type": "HDInsightHive",
            "typeProperties": {
              "scriptPath": "adftutorial\\hivequery.hql",
              "scriptLinkedService": "StorageLinkedService",
              "defines": {
                "Year": "$$Text.Format('{0:yyyy}',WindowStart)",
                "Month": "$$Text.Format('{0:%M}',WindowStart)",
                "Day": "$$Text.Format('{0:%d}',WindowStart)"
              }
            },
            "scheduler": {
              "frequency": "Day",
              "interval": 1
            },          
            "policy": {
              "concurrency": 1,
              "executionPriorityOrder": "OldestFirst",
              "retry": 2,  
              "timeout": "01:00:00"
            }
           }
         ]
       }
    }


## <a name="data-factory-functions-and-system-variables"></a>Factory andmefunktsioonid ja süsteemi muutujad   

Vaadake teemat [andmete Factory funktsioonid ja süsteemi muutujad](data-factory-functions-variables.md) loendi funktsioonid ja süsteemi muutujate, mis toetab andmete Factory.

## <a name="data-dependency-deep-dive"></a>Andmete sõltuvus deep dive

Luua tegevuse run andmekomplekti sektorit, andmete Factory kasutab järgmist *sõltuvus mudeli* määratlemiseks andmekomplektide tarbitud ja toodetud tegevuse vahelisi seoseid.

Ajavahemiku Sisestuskeel andmekogumite, mis on vaja luua väljundi andmekomplekti sektorit nimetatakse *sõltuvus perioodi kohta*.

Tegevuste run loob andmekomplekti sektorit alles pärast andmete sektorite Sisestuskeel andmekomplektide sõltuvus aja jooksul on saadaval. Teisisõnu, kõik Sisestuskeel sektoritele, mis sisaldab sõltuvus perioodi peab olema **valmis** olekus tegevuse käivitada, andes mõne väljundi andmekomplekti sektorit.

Andmekomplekti sektorit [**algus**, **lõpp**] loomiseks peab funktsiooni vastendama sõltuvus ajaks andmekomplekti sektorit. See funktsioon on põhiosas valemi, mis teisendab algus ja lõpp sõltuvus perioodi alguses ja lõpus andmekomplekti sektorit. Ametlikumalt:

    DatasetSlice = [start, end]
    DependecyPeriod = [f(start, end), g(start, end)]

**F** - ja **g** on vastendamise funktsioonid, mis arvutavad algus- ja iga toimingu sisestusmeetodi sõltuvus perioodi lõpu.

Nagu näha näidised, sõltuvus on sama andmete sektorit, mis on toodetud periood. Sellisel juhul arvutab andmete Factory automaatselt Sisestuskeel sektoritele, mis jäävad sõltuvus ajavahemikus.  

Näiteks koondamine valimi, kus väljund on iga päev toodeti ja sisendandmete on saadaval iga tunni, andmete sektorit aeg on 24 tundi. Andmete Factory leiab oluline tunni sisestusmeetodi viilud selle aja jooksul ja teeb väljundi sektorit sõltuvad Sisestuskeel sektorit.

Samuti saate sisestada oma vastenduse sõltuvus perioodi, nagu on näidatud valimi, kus üks sisendeid on nädala ja väljundi sektorit valmistatakse iga päev.

## <a name="data-dependency-and-validation"></a>Andmete sõltuvus ja valideerimine

Andmekomplekt võib olla valideerimine poliitika määratletud, mis täpsustab, kuidas andmeid, mida sektorit täitmise saate kinnitada enne, kui see on kasutusvalmis. Üksikasjad leiate [loomine andmekomplektide](data-factory-create-datasets.md) .

Sellisel juhul, kui sektorit on lõpule jõudnud täitmise, väljundi sektorit oleku muutmisel **ootavad** substatus **valideerimist**. Pärast sektorid on kinnitatud, sektorit olekuks **valmis**.

Kui andmete sektorit välja töötatud, kuid ei liigu valideerimist, tegevuse kestab järgmise etapi sektoritele, mis sõltuvad otseselt selle sektori ei töödelda.

[Jälgimine ja haldamine torujuhtmete](data-factory-monitor-manage-pipelines.md) antakse ülevaade eri riikide andmete Factory andmete sektorit.

## <a name="external-data"></a>Välisandmed

Väline (nagu on näidatud järgmises JSON koodilõigu), mis tähendab, et see ei loodud andmete Factory saate märgitakse Andmekomplekt. Sellisel juhul saate andmekomplekti poliitika täiendava kogumi parameetrid, mis kirjeldab valideerimine on ja proovige uuesti andmekomplekti poliitika. Vaadake [loomine torujuhtmete](data-factory-create-pipelines.md) kirjeldus kõik atribuudid.

Sarnaselt kogumid, mis on esitatud andmed Factory, andmete sektorid välisandmete peavad olema valmis enne sektorid sõltuvad saab töödelda.

    {
        "name": "AzureSqlInput",
        "properties":
        {
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties":
            {
                "tableName": "MyTable"
            },
            "availability":
            {
                "frequency": "Hour",
                "interval": 1     
            },
            "external": true,
            "policy":
            {
                "externalData":
                {
                    "retryInterval": "00:01:00",
                    "retryTimeout": "00:10:00",
                    "maximumRetry": 3
                }
            }  
        }
    }


## <a name="onetime-pipeline"></a>Müügivõimaluste ühekordselt
Saate luua ja ajastamine müügivõimaluste käivitamiseks perioodiliselt (nt: tund või iga päev) jooksul algus- ja lõpuaegade, saate määrata müügivõimaluste määratlus. Üksikasjad leiate [plaanimine tegevusi](#scheduling-and-execution) . Samuti saate luua müügivõimaluste, mis töötab ainult üks kord. Selleks saate atribuudi **pipelineMode** müügivõimaluste määratlus **ühekordselt** nagu on näidatud järgmises näites JSON. Selle atribuudi vaikeväärtus on **ajastatud**.

    {
        "name": "CopyPipeline",
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
                            "name": "InputDataset"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "OutputDataset"
                        }
                    ]
                    "name": "CopyActivity-0"
                }
            ]
            "pipelineMode": "OneTime"
        }
    }

Võtke arvesse järgmist.

- ** **Käivitamine** ja** lõpuaegade tulemas on määramata.
- Sisend- ja andmekomplektide **kättesaadavus** on määratud (**sagedus** ja **intervalli**), isegi juhul, kui andmete Factory ei kasuta väärtused.  
- Diagrammivaate ühekordse torujuhtmete ei kuvata. Selline käitumine on kujundus.
- Ühekordse torujuhtmete ei saa värskendada. Saate ühekordse müügivõimaluste klooni, ümber nimetada, atribuutide värskendamine ja juurutada seda, et luua muu poliitika.
