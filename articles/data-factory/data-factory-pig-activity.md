<properties 
    pageTitle="Siga tegevus" 
    description="Siit saate teada, kuidas saate siga tegevuse on Azure andmete factory siga skriptide käitamiseks kohta – nõudmisel/teie enda Hdinsightiga kobar kohta." 
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
    ms.date="08/31/2016" 
    ms.author="shlo"/>

# <a name="pig-activity"></a>Siga tegevus
> [AZURE.SELECTOR]
[Taru](data-factory-hive-activity.md)  
[Siga](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Hadoopi Streaming](data-factory-hadoop-streaming-activity.md)
[Masina õ](data-factory-azure-ml-batch-execution-activity.md) 
[Salvestatud protseduur](data-factory-stored-proc-activity.md)
[Andmete Lake Analytics U-SQL-i](data-factory-usql-activity.md)
[.NET kohandatud](data-factory-use-custom-activities.md)

Andmete Factory [müügivõimaluste](data-factory-create-pipelines.md) Hdinsightiga siga tegevuse käivitab siga päringute [oma](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) või [nõudmisel](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) Windowsi/Linuxi-põhiste Hdinsightiga kobar. Selles artiklis põhineb [tegevusi andmete teisendamiseks](data-factory-data-transformation-activities.md) artikkel, mis tutvustab andmete teisendamiseks ja toetatud teisendus tegevuste ülevaade.

## <a name="syntax"></a>Süntaks

    {
        "name": "HiveActivitySamplePipeline",
        "properties": {
        "activities": [
            {
                "name": "Pig Activity",
                "description": "description",
                "type": "HDInsightPig",
                "inputs": [
                    {
                        "name": "input tables"
                    }
                ],
                "outputs": [
                    {
                        "name": "output tables"
                    }
                ],
                "linkedServiceName": "MyHDInsightLinkedService",
                "typeProperties": {
                    "script": "Pig script",
                    "scriptPath": "<pathtothePigscriptfileinAzureblobstorage>",
                    "defines": {
                        "param1": "param1Value"
                    }
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                }
            }
        ]
      }
    }

## <a name="syntax-details"></a>Süntaks: üksikasjad

Atribuut | Kirjeldus | Nõutav
-------- | ----------- | --------
Nimi | Tegevuse nimi | Jah
kirjeldus | Tekst, mis kirjeldab tegevuse kasutatud | Ei
tüüp | HDinsightPig | Jah
sisendina | Ühe või mitme sisendina tarbitud siga tegevuse | Ei
väljundid | Ühe või mitme väljundeid toodetud siga tegevuse | Jah
linkedServiceName | Lingitud andmete Factory teenust registreeritud viide Hdinsightiga kobar | Jah
skripti | Määrake siga skripti teksti sees | Ei
skripti tee | Azure'i bloobimälu siga skripti talletada ja sisestage selle faili tee. Kasutage "script" või "scriptPath" atribuuti. Mõlemad ei saa koos kasutada. Faili nimi on tõstutundlikud. | Ei
määratleb | Määrata parameetrid /-väärtuse paarideks jaoks sees siga skripti viitamine | Ei

## <a name="example"></a>Näide

Vaatame mängu logid analytics, kuhu soovite tuvastada aega, mida mängijad mängimine algatatud ettevõtte näide.
 
Järgmine näidis mängu samamoodi on semikoolon (;) eraldatud fail. See sisaldab järgmised väljad – ProfileID, alustamineJuba kavandatud, kestus, SrcIPAddress ja GameType.

    1809,2014-05-04 12:04:25.3470000,14,221.117.223.75,CaptureFlag
    1703,2014-05-04 06:05:06.0090000,16,12.49.178.247,KingHill
    1703,2014-05-04 10:21:57.3290000,10,199.118.18.179,CaptureFlag
    1809,2014-05-04 05:24:22.2100000,23,192.84.66.141,KingHill
    .....

**Siga skripti** töötlemine:

    PigSampleIn = LOAD 'wasb://adfwalkthrough@anandsub14.blob.core.windows.net/samplein/' USING PigStorage(',') AS (ProfileID:chararray, SessionStart:chararray, Duration:int, SrcIPAddress:chararray, GameType:chararray);
    
    GroupProfile = Group PigSampleIn all;
    
    PigSampleOut = Foreach GroupProfile Generate PigSampleIn.ProfileID, SUM(PigSampleIn.Duration);
    
    Store PigSampleOut into 'wasb://adfwalkthrough@anandsub14.blob.core.windows.net/sampleoutpig/' USING PigStorage (',');

See siga skript käivitada andmete Factory teel, tehke järgmist.

1. Saate luua lingitud teenuse registreerida [oma Hdinsightiga arvutada kobar](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) või [nõudmisel Hdinsightiga arvutada kobar](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service)konfigureerimine. Vaatame helistage lingitud teenuse **HDInsightLinkedService**.
2.  Looge [lingitud teenuse](data-factory-azure-blob-connector.md) konfigureerida ühenduse Azure'i bloobimälu majutusteenuse andmed. Vaatame helistage lingitud teenuse **StorageLinkedService**.
3.  Looge [andmekomplektide](data-factory-create-datasets.md) osutab sisendi ja väljundi andmeid. Vaatame kõne Sisestuskeel andmekomplekti **PigSampleIn** ja väljundi andmekomplekti **PigSampleOut**.
4.  Kopeerige siga päringu faili Azure'i bloobimälu konfigureeritud etapis #2. Kui Azure talletamist, mis hostib andmed erineb ühte, mis hostib päringu faili, luua eraldi Azure Storage lingitud teenus. Vaadake lingitud teenuse tegevuse konfiguratsiooni. Määrake tee siga skriptifail ning **scriptLinkedService** **scriptPath **abil. 
    
    > [AZURE.NOTE] Atribuudi **skripti** abil saate sisestada ka siga skript Tekstisisese tegevuste määratlus. Siiski ei soovita seda moodust nagu kõik erimärgid script vajaduste tuleb seda vältida ja silumine probleeme tekkida. Samm #4 jälgimiseks on parim.
5. Looge tulemas HDInsightPig tegevusega. See tegevus töötleb sisendandmete käivitades siga skripti Hdinsightiga kobar.

        {
          "name": "PigActivitySamplePipeline",
          "properties": {
            "activities": [
              {
                "name": "PigActivitySample",
                "type": "HDInsightPig",
                "inputs": [
                  {
                    "name": "PigSampleIn"
                  }
                ],
                "outputs": [
                  {
                    "name": "PigSampleOut"
                  }
                ],
                "linkedServiceName": "HDInsightLinkedService",
                "typeproperties": {
                  "scriptPath": "adfwalkthrough\\scripts\\enrichlogs.pig",
                  "scriptLinkedService": "StorageLinkedService"
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                }
              }
            ]
          }
        } 
6. Tulemas juurutamine. Vt artiklit [loomine torujuhtmete](data-factory-create-pipelines.md) . 
7. Tulemas on factory jälgimise ja halduse andmevaadete kaudu jälgida. Näha [jälgimine ja haldamine andmete Factory torujuhtmete](data-factory-monitor-manage-pipelines.md) artiklit.

## <a name="specifying-parameters-for-a-pig-script"></a>Määrab siga skripti parameetrid 

Näide: mängu logid on märkimisväärselt iga päev Azure'i bloobimälu sisse ja talletatud kaustas sektsioonitud põhjal kuupäev ja kellaaeg. Soovite parameterize siga skripti ja edastama Sisestuskeel kausta asukoht dünaamiliselt ajal käitusaja ja toodavad ka väljund liigendatud kuupäev ja kellaaeg.
 
Parameetritega siga skripti kasutamiseks tehke järgmist.

- **Määratleb**määratleda parameetrid.

        {
            "name": "PigActivitySamplePipeline",
            "properties": {
            "activities": [
                {
                    "name": "PigActivitySample",
                    "type": "HDInsightPig",
                    "inputs": [
                        {
                            "name": "PigSampleIn"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "PigSampleOut"
                        }
                    ],
                    "linkedServiceName": "HDInsightLinkedService",
                    "typeproperties": {
                        "scriptPath": "adfwalkthrough\\scripts\\samplepig.hql",
                        "scriptLinkedService": "StorageLinkedService",
                        "defines": {
                            "Input": "$$Text.Format('wasb: //adfwalkthrough@<storageaccountname>.blob.core.windows.net/samplein/yearno={0: yyyy}/monthno={0: %M}/dayno={0: %d}/',SliceStart)",
                            "Output": "$$Text.Format('wasb://adfwalkthrough@<storageaccountname>.blob.core.windows.net/sampleout/yearno={0:yyyy}/monthno={0:%M}/dayno={0:%d}/', SliceStart)"
                        }
                    },
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1
                    }
                }
            ]
          }
        }  
      
- Siga skript, viidata parameetrid '**$parameterName**' abil, nagu on näidatud järgmises näites:

        PigSampleIn = LOAD '$Input' USING PigStorage(',') AS (ProfileID:chararray, SessionStart:chararray, Duration:int, SrcIPAddress:chararray, GameType:chararray);   
        GroupProfile = Group PigSampleIn all;       
        PigSampleOut = Foreach GroupProfile Generate PigSampleIn.ProfileID, SUM(PigSampleIn.Duration);      
        Store PigSampleOut into '$Output' USING PigStorage (','); 


## <a name="see-also"></a>Vt ka
- [Taru tegevus](data-factory-hive-activity.md)
- [MapReduce tegevus](data-factory-map-reduce.md)
- [Hadoopi Streaming tegevus](data-factory-hadoop-streaming-activity.md)
- [Autonoomsest säde programmid](data-factory-spark.md)
- [R skriptide autonoomsest](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)


