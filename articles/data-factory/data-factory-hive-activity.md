<properties 
    pageTitle="Taru tegevus" 
    description="Siit saate teada, kuidas saate taru tegevuse on Azure andmete factory käivitamise kohta – nõudmisel/teie enda Hdinsightiga kobar taru päringud." 
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
    ms.date="10/11/2016" 
    ms.author="shlo"/>

# <a name="hive-activity"></a>Taru tegevus
> [AZURE.SELECTOR]
[Taru](data-factory-hive-activity.md)  
[Siga](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Hadoopi Streaming](data-factory-hadoop-streaming-activity.md)
[Masina õ](data-factory-azure-ml-batch-execution-activity.md) 
[Salvestatud protseduur](data-factory-stored-proc-activity.md)
[Andmete Lake Analytics U-SQL-i](data-factory-usql-activity.md)
[.NET kohandatud](data-factory-use-custom-activities.md)

Andmete Factory [müügivõimaluste](data-factory-create-pipelines.md) Hdinsightiga taru tegevuse käivitab Windowsi/Linuxi-põhiste Hdinsightile [oma](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) või [nõudmisel](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) kobar taru päringud. Selles artiklis põhineb [tegevusi andmete teisendamiseks](data-factory-data-transformation-activities.md) artikkel, mis tutvustab andmete teisendamiseks ja toetatud teisendus tegevuste ülevaade.

## <a name="syntax"></a>Süntaks

    {
        "name": "Hive Activity",
        "description": "description",
        "type": "HDInsightHive",
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
          "script": "Hive script",
          "scriptPath": "<pathtotheHivescriptfileinAzureblobstorage>",
          "defines": {
            "param1": "param1Value"
          }
        },
       "scheduler": {
          "frequency": "Day",
          "interval": 1
        }
    }
    
## <a name="syntax-details"></a>Süntaks: üksikasjad

Atribuut | Kirjeldus | Nõutav
-------- | ----------- | --------
Nimi | Tegevuse nimi | Jah
kirjeldus | Tekst, mis kirjeldab tegevuse kasutatud | Ei
tüüp | HDinsightHive | Jah
sisendina | Sisendina tarbitud taru tegevuse | Ei
väljundid | Toodetud taru tegevuse väljundid | Jah 
linkedServiceName | Lingitud andmete Factory teenust registreeritud viide Hdinsightiga kobar | Jah 
skripti | Määrake taru skripti teksti sees | Ei
skripti tee | Azure'i bloobimälu taru skripti talletada ja sisestage selle faili tee. Kasutage "script" või "scriptPath" atribuuti. Mõlemad ei saa koos kasutada. Faili nimi on tõstutundlikud. | Ei 
määratleb | Määrata parameetrid /-väärtuse paarideks jaoks taru skripti abil 'hiveconf' sees viitamine  | Ei

## <a name="example"></a>Näide

Vaatame mängu logid analytics, kuhu soovite tuvastada aega, mida kasutaja algatatud ettevõtte mängimine näide. 

Järgmised Logi on valimi mängu Logi, mis on komaga (`,`) eraldatud ja sisaldab järgmisi välju – ProfileID, alustamineJuba kavandatud, kestus, SrcIPAddress ja GameType.

    1809,2014-05-04 12:04:25.3470000,14,221.117.223.75,CaptureFlag
    1703,2014-05-04 06:05:06.0090000,16,12.49.178.247,KingHill
    1703,2014-05-04 10:21:57.3290000,10,199.118.18.179,CaptureFlag
    1809,2014-05-04 05:24:22.2100000,23,192.84.66.141,KingHill
    .....

Funktsiooni **taru skripti** töödelda andmed:

    DROP TABLE IF EXISTS HiveSampleIn; 
    CREATE EXTERNAL TABLE HiveSampleIn 
    (
        ProfileID       string, 
        SessionStart    string, 
        Duration        int, 
        SrcIPAddress    string, 
        GameType        string
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION 'wasb://adfwalkthrough@<storageaccount>.blob.core.windows.net/samplein/'; 
    
    DROP TABLE IF EXISTS HiveSampleOut; 
    CREATE EXTERNAL TABLE HiveSampleOut 
    (   
        ProfileID   string, 
        Duration    int
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION 'wasb://adfwalkthrough@<storageaccount>.blob.core.windows.net/sampleout/';
    
    INSERT OVERWRITE TABLE HiveSampleOut
    Select 
        ProfileID,
        SUM(Duration)
    FROM HiveSampleIn Group by ProfileID

Andmete Factory teel käivitada selle taru skripti, peate tehke järgmist.

1. Saate luua lingitud teenuse registreerida [oma Hdinsightiga arvutada kobar](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) või [nõudmisel Hdinsightiga arvutada kobar](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service)konfigureerimine. Vaatame helistage lingitud teenuse "HDInsightLinkedService".
2. Looge [lingitud teenuse](data-factory-azure-blob-connector.md) konfigureerida ühenduse Azure'i bloobimälu majutusteenuse andmed. Vaatame kõne lingitud teenuse "StorageLinkedService"
3. Looge [andmekomplektide](data-factory-create-datasets.md) osutab sisendi ja väljundi andmeid. Vaatame kõne Sisestuskeel andmekomplekti "HiveSampleIn" ja "HiveSampleOut" väljundi andmekomplekti
4. Kopeeri taru päringu Azure'i bloobimälu failina konfigureeritud juhises #2. Kui salvestusruumi hosting andmed on selle päringu faili hosting erineb, luua eraldi Azure Storage lingitud teenus ja viidata väljal tegevuse. Määrake taru päringu faili- ja **scriptLinkedService** määramiseks Azure talletamist, mis sisaldab skripti faili tee **scriptPath **abil. 

    > [AZURE.NOTE] Atribuudi **skripti** abil saate sisestada ka taru skripti Tekstisisese tegevuste määratlus. Me ei soovita seda moodust nagu kõik erimärgid skripti, tuleb seda vältida vajaduste JSON dokumendi sees ja silumine probleeme tekkida. Samm #4 jälgimiseks on parim.
5.  Müügivõimaluste loomine HDInsightHive tegevusega. Tegevuse protsesside/teisendusteta andmed.

        {
          "name": "HiveActivitySamplePipeline",
          "properties": {
            "activities": [
              {
                "name": "HiveActivitySample",
                "type": "HDInsightHive",
                "inputs": [
                  {
                    "name": "HiveSampleIn"
                  }
                ],
                "outputs": [
                  {
                    "name": "HiveSampleOut"
                  }
                ],
                "linkedServiceName": "HDInsightLinkedService",
                "typeproperties": {
                  "scriptPath": "adfwalkthrough\\scripts\\samplehive.hql",
                  "scriptLinkedService": "StorageLinkedService"
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                }
              }
            ]
          }
        }

6.  Tulemas juurutamine. Vt artiklit [loomine torujuhtmete](data-factory-create-pipelines.md) . 
7.  Tulemas on factory jälgimise ja halduse andmevaadete kaudu jälgida. Näha [jälgimine ja haldamine andmete Factory torujuhtmete](data-factory-monitor-manage-pipelines.md) artiklit. 


## <a name="specifying-parameters-for-a-hive-script"></a>Määrab taru skripti parameetrid  
Selles näites mängu logid on märkimisväärselt iga päev Azure'i bloobimälu sisse ja on salvestatud kausta liigendatud kuupäev ja kellaaeg. Soovite parameterize taru skripti ja edastama Sisestuskeel kausta asukoht dünaamiliselt ajal käitusaja ja toodavad ka väljund liigendatud kuupäev ja kellaaeg.

Kasutage parameetritega taru skripti, tehke järgmist.

- **Määratleb**määratleda parameetrid.

        {
            "name": "HiveActivitySamplePipeline",
            "properties": {
            "activities": [
                {
                    "name": "HiveActivitySample",
                    "type": "HDInsightHive",
                    "inputs": [
                        {
                            "name": "HiveSampleIn"
                          }
                    ],
                    "outputs": [
                        {
                            "name": "HiveSampleOut"
                        }
                    ],
                    "linkedServiceName": "HDInsightLinkedService",
                    "typeproperties": {
                        "scriptPath": "adfwalkthrough\\scripts\\samplehive.hql",
                        "scriptLinkedService": "StorageLinkedService",
                        "defines": {
                            "Input": "$$Text.Format('wasb://adfwalkthrough@<storageaccountname>.blob.core.windows.net/samplein/yearno={0:yyyy}/monthno={0:%M}/dayno={0:%d}/', SliceStart)",
                            "Output": "$$Text.Format('wasb://adfwalkthrough@<storageaccountname>.blob.core.windows.net/sampleout/yearno={0:yyyy}/monthno={0:%M}/dayno={0:%d}/', SliceStart)"
                        },
                        "scheduler": {
                            "frequency": "Hour",
                            "interval": 1
                        }
                    }
                }
            ]
          }
        }

- Skripti taru viidata parameetri abil **${hiveconf:parameterName}**. 

        DROP TABLE IF EXISTS HiveSampleIn; 
        CREATE EXTERNAL TABLE HiveSampleIn 
        (
            ProfileID   string, 
            SessionStart    string, 
            Duration    int, 
            SrcIPAddress    string, 
            GameType    string
        ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:Input}'; 
        
        DROP TABLE IF EXISTS HiveSampleOut; 
        CREATE EXTERNAL TABLE HiveSampleOut 
        (
            ProfileID   string, 
            Duration    int
        ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:Output}';
        
        INSERT OVERWRITE TABLE HiveSampleOut
        Select 
            ProfileID,
            SUM(Duration)
        FROM HiveSampleIn Group by ProfileID


## <a name="see-also"></a>Vt ka
- [Siga tegevus](data-factory-pig-activity.md)
- [MapReduce tegevus](data-factory-map-reduce.md)
- [Hadoopi Streaming tegevus](data-factory-hadoop-streaming-activity.md)
- [Autonoomsest säde programmid](data-factory-spark.md)
- [R skriptide autonoomsest](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample)









