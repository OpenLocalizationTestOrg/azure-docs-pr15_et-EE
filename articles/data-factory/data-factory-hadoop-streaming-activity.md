<properties 
    pageTitle="Hadoopi Streaming tegevus" 
    description="Siit saate teada, kuidas saate Hadoopi Streaming tegevuse on Azure andmete factory käivitada Hadoopi Streaming programmide kohta – nõudmisel/teie enda Hdinsightiga kobar." 
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
    ms.date="09/20/2016" 
    ms.author="shlo"/>

# <a name="hadoop-streaming-activity"></a>Hadoopi Streaming tegevus
> [AZURE.SELECTOR]
[Taru](data-factory-hive-activity.md)  
[Siga](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Hadoopi Streaming](data-factory-hadoop-streaming-activity.md)
[Masina õ](data-factory-azure-ml-batch-execution-activity.md) 
[Salvestatud protseduur](data-factory-stored-proc-activity.md)
[Andmete Lake Analytics U-SQL-i](data-factory-usql-activity.md)
[.NET kohandatud](data-factory-use-custom-activities.md)

Saate kasutada HDInsightStreamingActivity tegevuse autonoomsest Hadoopi Streaming töö on Azure andmete Factory kohaletoimetamisel. Järgmised JSON koodilõigu näitab süntaks on HDInsightStreamingActivity kasutamine müügivõimaluste JSON faili. 

Hdinsightiga Streaming tegevuse andmete Factory [müügivõimaluste](data-factory-create-pipelines.md) aktiveeritakse Hadoopi Streaming programmid [enda](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) või [nõudmisel](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Windowsi/Linuxi-põhiste Hdinsightiga kobar. Selles artiklis põhineb [tegevusi andmete teisendamiseks](data-factory-data-transformation-activities.md) artikkel, mis tutvustab andmete teisendamiseks ja toetatud teisendus tegevuste ülevaade.

## <a name="json-sample"></a>JSON näidis
Hdinsightiga kobar lisatakse automaatselt näide programmid (wc.exe ja cat.exe) ja andmete (davinci.txt). Vaikimisi Hdinsightiga kobar kasutatavat ümbris nimi on klaster enda nime. Näiteks kui teie kobar nimi on myhdicluster, seotud bloobimälu ümbrisest nimi oleks myhdicluster. 

    {
        "name": "HadoopStreamingPipeline",
        "properties": {
            "description": "Hadoop Streaming Demo",
            "activities": [
                {
                    "type": "HDInsightStreaming",
                    "typeProperties": {
                        "mapper": "cat.exe",
                        "reducer": "wc.exe",
                        "input": "wasb://<nameofthecluster>@spestore.blob.core.windows.net/example/data/gutenberg/davinci.txt",
                        "output": "wasb://<nameofthecluster>@spestore.blob.core.windows.net/example/data/StreamingOutput/wc.txt",
                        "filePaths": [
                            "<nameofthecluster>/example/apps/wc.exe",
                            "<nameofthecluster>/example/apps/cat.exe"
                        ],
                        "fileLinkedService": "StorageLinkedService",
                        "getDebugInfo": "Failure"
                    },
                    "outputs": [
                        {
                            "name": "StreamingOutputDataset"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00",
                        "concurrency": 1,
                        "executionPriorityOrder": "NewestFirst",
                        "retry": 1
                    },
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1
                    },
                    "name": "RunHadoopStreamingJob",
                    "description": "Run a Hadoop streaming job",
                    "linkedServiceName": "HDInsightLinkedService"
                }
            ],
            "start": "2014-01-04T00:00:00Z",
            "end": "2014-01-05T00:00:00Z"
        }
    }

Võtke arvesse järgmist.

1. Seadke **linkedServiceName** lingitud teenus, mis viitab Hdinsightiga klaster, kus käivitatakse streaming mapreduce töö nime.
2. Seadke **HDInsightStreaming**tegevuse tüüp.
3. Määratlege **plaanuri** atribuudi nimi plaanuri käivitatava. Näites on cat.exe käivitatava plaanuri.
4. Määratlege **reduktor** atribuudi nimi reduktor käivitatava. Näites on wc.exe käivitatava reduktor.
5. Määratlege **Sisestuskeel** tüüp atribuudi plaanuri Sisestuskeel faili (sh asukoht). Näites: "wasb://adfsample@ <account name>.blob.core.windows.net/example/data/gutenberg/davinci.txt ": adfsample on bloobimälu ümbris, Gutenbergi/näide/andmed on kaust ja davinci.txt on selle bloobimälu.
6. Selle **väljundi** atribuudi, määratlege soovitud reduktor väljundfail (sh asukoht). Hadoopi Streaming töö väljund on kirjutatud selle atribuudi määratud asukohta.
7. Määrake jaotises **filePaths** plaanuri ja reduktor täitmisfailid liikumisteid. Näites: "adfsample/example/apps/wc.exe" adfsample on bloobimälu ümbris, nt/rakendused on kaust ja wc.exe on käivitatava.
8. Määrake atribuudi **fileLinkedService** Azure Storage lingitud teenus, mis tähistab Azure talletamist filePaths jaotises määratud faile sisaldava.
9. Määrake atribuudi **argumendid** streaming töö argumendid.
10. Atribuut **getDebugInfo** on fakultatiivsete elementide. Kui see on seatud tõrge, laaditakse logid ainult tõrke kohta. Kui see on seatud kõik, laaditakse logid alati sõltumata Andmetäite olek.

> [AZURE.NOTE] Nagu on näidatud, määrake atribuudi **väljundid** Hadoopi Streaming tegevuste mõne väljundi andmekomplekti. Tuvastatavate on lihtsalt näiva andmekomplekti nõutava müügivõimaluste ajakava juhtida. Pole vaja määrata mis tahes Sisestuskeel andmekomplekti **sisendina** atribuudi tegevuste jaoks.  

    
## <a name="example"></a>Näide
Müügivõimaluste ülevaate töötab sõnade streaming MAPI/vähenda programmi Windows Azure Hdinsightiga klaster. 

### <a name="linked-services"></a>Lingitud teenused

#### <a name="azure-storage-linked-service"></a>Azure'i lingitud salvestusteenus
Esmalt, luua lingitud teenuse Azure Storage Windows Azure Hdinsightiga kobar, et Azure'i andmed factory kasutatavat linkimiseks. Kui saate kopeerige ja kleepige järgmine kood, ärge unustage Sisestage konto nimi ja konto võti nimi ja Azure salvestusruumi võti. 

    {
        "name": "StorageLinkedService",
        "properties": {
            "type": "AzureStorage",
            "typeProperties": {
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>"
            }
        }
    }

#### <a name="azure-hdinsight-linked-service"></a>Azure Hdinsightiga lingitud teenus
Seejärel looge lingitud teenus Windows Azure Hdinsightiga klaster linkimiseks factory Azure'i andmed. Kui saate kopeerige ja kleepige järgmine kood, asendage Hdinsightiga kobar nimi nimi Hdinsightiga klaster ja kasutaja nimi ja parool väärtusi muuta. 
    
    {
        "name": "HDInsightLinkedService",
        "properties": {
            "type": "HDInsight",
            "typeProperties": {
                "clusterUri": "https://<HDInsight cluster name>.azurehdinsight.net",
                "userName": "admin",
                "password": "**********",
                "linkedServiceName": "StorageLinkedService"
            }
        }
    }

### <a name="datasets"></a>Andmekomplektide

#### <a name="output-dataset"></a>Väljundi andmekomplekti
Müügivõimaluste selles näites ei võta sisendeid. Saate määrata mõne väljundi andmekomplekti Hdinsightiga Streaming tegevuste jaoks. Tuvastatavate on lihtsalt fiktiivne andmekomplekti nõutava müügivõimaluste ajakava juhtida. 

    {
        "name": "StreamingOutputDataset",
        "properties": {
            "published": false,
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "folderPath": "adftutorial/streamingdata/",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                },
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            }
        }
    }

### <a name="pipeline"></a>Müügivõimaluste

Selles näites tulemas on ainult üks tegevus, mis on tippige: **HDInsightStreaming**. 

Hdinsightiga kobar lisatakse automaatselt näide programmid (wc.exe ja cat.exe) ja andmete (davinci.txt). Vaikimisi Hdinsightiga kobar kasutatavat ümbris nimi on klaster enda nime. Näiteks kui teie kobar nimi on myhdicluster, seotud bloobimälu ümbrisest nimi oleks myhdicluster.  

    {
        "name": "HadoopStreamingPipeline",
        "properties": {
            "description": "Hadoop Streaming Demo",
            "activities": [
                {
                    "type": "HDInsightStreaming",
                    "typeProperties": {
                        "mapper": "cat.exe",
                        "reducer": "wc.exe",
                        "input": "wasb://<blobcontainer>@spestore.blob.core.windows.net/example/data/gutenberg/davinci.txt",
                        "output": "wasb://<blobcontainer>@spestore.blob.core.windows.net/example/data/StreamingOutput/wc.txt",
                        "filePaths": [
                            "<blobcontainer>/example/apps/wc.exe",
                            "<blobcontainer>/example/apps/cat.exe"
                        ],
                        "fileLinkedService": "StorageLinkedService"
                    },
                    "outputs": [
                        {
                            "name": "StreamingOutputDataset"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00",
                        "concurrency": 1,
                        "executionPriorityOrder": "NewestFirst",
                        "retry": 1
                    },
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1
                    },
                    "name": "RunHadoopStreamingJob",
                    "description": "Run a Hadoop streaming job",
                    "linkedServiceName": "HDInsightLinkedService"
                }
            ],
            "start": "2014-01-04T00:00:00Z",
            "end": "2014-01-05T00:00:00Z"
        }
    }

## <a name="see-also"></a>Vt ka
- [Taru tegevus](data-factory-hive-activity.md)
- [Siga tegevus](data-factory-pig-activity.md)
- [MapReduce tegevus](data-factory-map-reduce.md)
- [Autonoomsest säde programmid](data-factory-spark.md)
- [R skriptide autonoomsest](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)

