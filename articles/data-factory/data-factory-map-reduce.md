<properties 
    pageTitle="Autonoomsest MapReduce programmi kaudu Azure'i andmed Factory" 
    description="Saate teada, kuidas töötab MapReduce programmid klõpsake mõnda Windows Azure Hdinsightiga kobar on Azure andmete factory andmete töötlemiseks." 
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

# <a name="invoke-mapreduce-programs-from-data-factory"></a>Autonoomsest andmete Factory MapReduce programmides
> [AZURE.SELECTOR]
[Taru](data-factory-hive-activity.md)  
[Siga](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Hadoopi Streaming](data-factory-hadoop-streaming-activity.md)
[Masina õ](data-factory-azure-ml-batch-execution-activity.md) 
[Salvestatud protseduur](data-factory-stored-proc-activity.md)
[Andmete Lake Analytics U-SQL-i](data-factory-usql-activity.md)
[.NET kohandatud](data-factory-use-custom-activities.md)

Hdinsightiga MapReduce tegevuse andmete Factory [müügivõimaluste](data-factory-create-pipelines.md) aktiveeritakse MapReduce programmid [enda](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) või [nõudmisel](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Windowsi/Linuxi-põhiste Hdinsightiga kobar. Selles artiklis põhineb [tegevusi andmete teisendamiseks](data-factory-data-transformation-activities.md) artikkel, mis tutvustab andmete teisendamiseks ja toetatud teisendus tegevuste ülevaade.

## <a name="introduction"></a>Sissejuhatus 
Müügivõimaluste rakenduses on Azure andmete factory töötleb andmeid lingitud salvestusruumi teenuste lingitud Arvuta teenuste abil. See sisaldab tegevusi, kus iga tegevuse täidab konkreetse töötlemine jada. Selles artiklis kirjeldatakse Hdinsightiga MapReduce tegevuse abil.
 
Vaadake teemat [siga](data-factory-pig-activity.md) ja [taru](data-factory-hive-activity.md) skriptid siga/taru Windowsi/Linuxi-põhiste Hdinsightiga klaster müügivõimaluste kaudu, kasutades Hdinsightiga siga ja taru üksikasjad. 

## <a name="json-for-hdinsight-mapreduce-activity"></a>JSON Hdinsightiga MapReduce tegevuste 

JSON määratluse Hdinsightiga tegevuste jaoks: 
 
1. Seadke **Hdinsightiga** **tegevuse** **Tüüp** .
3. Määrake klassi **klassinimi** atribuudi nimi.
4. Määrake, sh faili nimi **jarFilePath** atribuudi JAR-faili tee.
5. Määrake lingitud teenus, mis viitab **jarLinkedService** atribuudi JAR faili sisaldava Azure'i bloobimälu.   
6. Määrake jaotises **argumendid** kõik argumendid MapReduce programmi. Käitusajal, näete mõne eest argumendid (nt: mapreduce.job.tags) kaudu MapReduce raames. Teie argumendid MapReduce argumendiga eristada, kaaluge suvand nii väärtus argumentidena nagu on näidatud järgmises näites (- s,--sisendit,--väljundi jne, on kohe järgneb nende väärtuste suvandid).

        {
            "name": "MahoutMapReduceSamplePipeline",
            "properties": {
                "description": "Sample Pipeline to Run a Mahout Custom Map Reduce Jar. This job calcuates an Item Similarity Matrix to determine the similarity between 2 items",
                "activities": [
                    {
                        "type": "HDInsightMapReduce",
                        "typeProperties": {
                            "className": "org.apache.mahout.cf.taste.hadoop.similarity.item.ItemSimilarityJob",
                            "jarFilePath": "adfsamples/Mahout/jars/mahout-examples-0.9.0.2.2.7.1-34.jar",
                            "jarLinkedService": "StorageLinkedService",
                            "arguments": [
                                "-s",
                                "SIMILARITY_LOGLIKELIHOOD",
                                "--input",
                                "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/input",
                                "--output",
                                "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/output/",
                                "--maxSimilaritiesPerItem",
                                "500",
                                "--tempDir",
                                "wasb://adfsamples@spestore.blob.core.windows.net/Mahout/temp/mahout"
                            ]
                        },
                        "inputs": [
                            {
                                "name": "MahoutInput"
                            }
                        ],
                        "outputs": [
                            {
                                "name": "MahoutOutput"
                            }
                        ],
                        "policy": {
                            "timeout": "01:00:00",
                            "concurrency": 1,
                            "retry": 3
                        },
                        "scheduler": {
                            "frequency": "Hour",
                            "interval": 1
                        },
                        "name": "MahoutActivity",
                        "description": "Custom Map Reduce to generate Mahout result",
                        "linkedServiceName": "HDInsightLinkedService"
                    }
                ],
                "start": "2014-01-03T00:00:00Z",
                "end": "2014-01-04T00:00:00Z",
                "isPaused": false,
                "hubName": "mrfactory_hub",
                "pipelineMode": "Scheduled"
            }
        }
    
    

Hdinsightiga MapReduce tegevuse abil saate käivitada mis tahes MapReduce jar faili on Hdinsightiga kobar. Järgmine näidis JSON määratluses müügivõimaluste, Hdinsightiga tegevuse on konfigureeritud Mahout JAR faili.

## <a name="sample-on-github"></a>Valimi github
Saate alla laadida valimi kasutamise Hdinsightiga MapReduce tegevuse kaudu: [Andmete Factory näidised github](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/JSON/MapReduce_Activity_Sample).  

## <a name="running-the-word-count-program"></a>Sõnaarvestuse programmi
Müügivõimaluste selles näites käivitatakse sõnade MAPI/vähenda programmi Windows Azure Hdinsightiga klaster.   

### <a name="linked-services"></a>Lingitud teenused
Esmalt, luua lingitud teenuse Azure Storage Windows Azure Hdinsightiga kobar, et Azure'i andmed factory kasutatavat linkimiseks. Kui saate kopeerige ja kleepige järgmine kood, ärge unustage Sisestage **konto nimi** ja **konto võti** nimi ja Azure salvestusruumi võti. 

#### <a name="azure-storage-linked-service"></a>Azure'i lingitud salvestusteenus

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
Seejärel looge lingitud teenus Windows Azure Hdinsightiga klaster linkimiseks factory Azure'i andmed. Kui saate kopeerige ja kleepige järgmine kood, Asendage **Hdinsightiga kobar nimi** nimi Hdinsightiga klaster ja kasutaja nimi ja parool väärtusi muuta.   

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
Müügivõimaluste selles näites ei võta sisendeid. Saate määrata mõne väljundi andmekomplekti Hdinsightiga MapReduce tegevuste jaoks. Tuvastatavate on lihtsalt näiva andmekomplekti nõutava müügivõimaluste ajakava juhtida.  

    {
        "name": "MROutput",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "fileName": "WordCountOutput1.txt",
                "folderPath": "example/data/",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            }
        }
    }

### <a name="pipeline"></a>Müügivõimaluste
Selles näites tulemas on ainult üks tegevus, mis on tippige: HDInsightMapReduce. Mõned olulised atribuudid on JSON on: 

Atribuut | Märkmete
:-------- | :-----
tüüp | **HDInsightMapReduce**peab olema seatud tüüp. 
klassinimi | Klassi nimi on: **wordcount**
jarFilePath | Sisaldavat ainekursust jar faili tee. Kui saate kopeerige ja kleepige järgmine kood, ärge unustage muuta klaster nime. 
jarLinkedService | Azure'i salvestusruumi lingitud teenus, mis sisaldab jar-faili. Lingitud teenus viitab talletamist, mis on seotud Hdinsightiga kobar. 
argumendid | Wordcount programm võtab kaks kohustuslikku argumenti, sisend ja väljund. Sisestuskeel fail on davinci.txt faili.
sagedus/intervall | Neid atribuute väärtused vastavad väljundi andmekomplekti. 
linkedServiceName | viitab Hdinsightiga lingitud teenuse oli varem loodud.   

    {
        "name": "MRSamplePipeline",
        "properties": {
            "description": "Sample Pipeline to Run the Word Count Program",
            "activities": [
                {
                    "type": "HDInsightMapReduce",
                    "typeProperties": {
                        "className": "wordcount",
                        "jarFilePath": "<HDInsight cluster name>/example/jars/hadoop-examples.jar",
                        "jarLinkedService": "StorageLinkedService",
                        "arguments": [
                            "/example/data/gutenberg/davinci.txt",
                            "/example/data/WordCountOutput1"
                        ]
                    },
                    "outputs": [
                        {
                            "name": "MROutput"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00",
                        "concurrency": 1,
                        "retry": 3
                    },
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1
                    },
                    "name": "MRActivity",
                    "linkedServiceName": "HDInsightLinkedService"
                }
            ],
            "start": "2014-01-03T00:00:00Z",
            "end": "2014-01-04T00:00:00Z"
        }
    }

## <a name="run-spark-programs"></a>Säde programmide käivitamine
Saate MapReduce tegevuste käivitamiseks säde programmide klaster Hdinsightiga säde. Üksikasjad leiate [Azure'i andmed Factory autonoomsest säde programmides](data-factory-spark.md) .  

[developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[cmdlet-reference]: http://go.microsoft.com/fwlink/?LinkId=517456


[adfgetstarted]: data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[adfgetstartedmonitoring]:data-factory-copy-data-from-azure-blob-storage-to-sql-database.md#monitor-pipelines 

[Developer Reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[Azure Portal]: http://portal.azure.com
 
## <a name="see-also"></a>Vt ka
- [Taru tegevus](data-factory-hive-activity.md)
- [Siga tegevus](data-factory-pig-activity.md)
- [Hadoopi Streaming tegevus](data-factory-hadoop-streaming-activity.md)
- [Autonoomsest säde programmid](data-factory-spark.md)
- [R skriptide autonoomsest](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)
