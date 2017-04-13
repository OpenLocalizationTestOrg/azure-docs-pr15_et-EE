<properties
   pageTitle="Kasutage Hadoopi taru Hdinsightiga PowerShelli abil | Microsoft Azure'i"
   description="PowerShelli abil saate käivitada taru päringute Hadoopi Hdinsightiga."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/07/2016"
   ms.author="larryfr"/>

#<a name="run-hive-queries-using-powershell"></a>Käivitage PowerShelli kaudu taru päringud

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

Selles dokumendis on toodud näide taru päringute loomine Hadoopi sõitma Hdinsightiga kobar Azure'i ressursirühm režiimis Azure PowerShelli kaudu.

> [AZURE.NOTE] Selle dokumendi ei paku HiveQL laused, mis on näidetes kasutatud teha üksikasjalikku kirjeldust. HiveQL, mida selles näites kasutatakse kohta leiate teavet teemast [Kasutamine taru Hadoopi Hdinsightiga abil](hdinsight-use-hive.md).


**Eeltingimused**

Selles artiklis toodud juhiseid tegemiseks on vaja järgmist.

- **Windows Azure Hdinsightiga (Hadoopi Hdinsightiga) kobar, (Windowsi-põhiste või Linuxi-põhine)**
- **Töökoha Azure PowerShelli abil**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

##<a name="run-hive-queries-using-azure-powershell"></a>Azure'i PowerShelli kaudu taru päringute sooritamine

On toodud Azure PowerShelli *cmdlet-käsud* , mis võimaldavad teil kaugühenduse teel käivitada taru päringute Hdinsightiga. Sees, seda saab teha ülejäänud kõned tööta Hdinsightiga klaster [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) (varasema nimega Templeton) abil.

Remote Hdinsightiga kobar taru päringu käivitamisel kasutatakse järgmised cmdlet-käsud:

* **Lisa-AzureRmAccount**: Azure'i PowerShelli autendib Azure tellimuse

* **Uus-AzureRmHDInsightHiveJobDefinition**: loob uue *definitsiooni* määratud HiveQL lausete abil

* **Algus-AzureRmHDInsightJob**: saadab töö määratlus Hdinsightiga, käivitab töö ja tagastab *töö* objekti, mida saab kasutada töö oleku kontrollimine

* **Oodake-AzureRmHDInsightJob**: kasutab töö objekti töö oleku vaatamine. See oodake töö lõpulejõudmist või ajal on ületatud.

* **Get-AzureRmHDInsightJobOutput**: töö väljundi toomiseks kasutada

* **Invoke-AzureRmHDInsightHiveJob**: kasutada HiveQL laused käitamiseks. See blokeerib päring on lõpule jõudnud, siis tagastab tulemused

* **Kasutage-AzureRmHDInsightCluster**: määrab praeguse kobar **Invoke-AzureRmHDInsightHiveJob** käsu kasutamine

Järgmised sammud kirjeldavad käivitamiseks töö klaster Hdinsightiga nende cmdlettide kasutamise kohta:

1. Järgmine kood **hivejob.ps1**redaktoris, salvestada. **CLUSTERNAME** peate asendama klaster Hdinsightiga nime.

        #Specify the values
        $clusterName = "CLUSTERNAME"
        $creds=Get-Credential

        # Login to your Azure subscription
        # Is there an active Azure subscription?
        $sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
        if(-not($sub))
        {
            Add-AzureRmAccount
        }

        #HiveQL
        #Note: set hive.execution.engine=tez; is not required for
        #      Linux-based HDInsight
        $queryString = "set hive.execution.engine=tez;" +
                    "DROP TABLE log4jLogs;" +
                    "CREATE EXTERNAL TABLE log4jLogs(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) ROW FORMAT DELIMITED FIELDS TERMINATED BY ' ' STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';" +
                    "SELECT * FROM log4jLogs WHERE t4 = '[ERROR]';"

        #Create an HDInsight Hive job definition
        $hiveJobDefinition = New-AzureRmHDInsightHiveJobDefinition -Query $queryString 

        #Submit the job to the cluster
        Write-Host "Start the Hive job..." -ForegroundColor Green

        $hiveJob = Start-AzureRmHDInsightJob -ClusterName $clusterName -JobDefinition $hiveJobDefinition -ClusterCredential $creds

        #Wait for the Hive job to complete
        Write-Host "Wait for the job to complete..." -ForegroundColor Green
        Wait-AzureRmHDInsightJob -ClusterName $clusterName -JobId $hiveJob.JobId -ClusterCredential $creds

        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
        # Print the output
        Write-Host "Display the standard output..." -ForegroundColor Green
        Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $hiveJob.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds
            
2. Avage uus **Azure PowerShelli** Käsuviip. Muuta kataloogide **hivejob.ps1** faili asukoht ja seejärel järgmise käsu abil saate käivitada skripti.

        .\hivejob.ps1

    Kui skript käivitub, palutakse teil sisestada HTTPS/administraatori konto identimisteave klaster. Ka palutakse teil sisse logida Azure tellimuse.
    
7. Pärast töö lõpulejõudmist, see peaks tagastama teavet umbes järgmine:

        Display the standard output...
        2012-02-03      18:35:34        SampleClass0    [ERROR] incorrect       id
        2012-02-03      18:55:54        SampleClass1    [ERROR] incorrect       id
        2012-02-03      19:25:27        SampleClass4    [ERROR] incorrect       id

4. Nagu varem mainitud, saab **Invoke-taru** päringu ja vastuse ootamine. Saate kasutada järgmisi käske ja Asendage **CLUSTERNAME** klaster nime.

        Use-AzureRmHDInsightCluster -ClusterName $clusterName -HttpCredential $creds
        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
        $queryString = "set hive.execution.engine=tez;" +
                    "DROP TABLE log4jLogs;" +
                    "CREATE EXTERNAL TABLE log4jLogs(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) ROW FORMAT DELIMITED FIELDS TERMINATED BY ' ' STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';" +
                    "SELECT * FROM log4jLogs WHERE t4 = '[ERROR]';"
        Invoke-AzureRmHDInsightHiveJob `
            -StatusFolder "statusout" `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -Query $queryString

    Väljund näeb välja järgmine:

        2012-02-03  18:35:34    SampleClass0    [ERROR] incorrect   id
        2012-02-03  18:55:54    SampleClass1    [ERROR] incorrect   id
        2012-02-03  19:25:27    SampleClass4    [ERROR] incorrect   id

    > [AZURE.NOTE] Rohkem HiveQL päringuid, saate kasutada Azure PowerShelli **Siin-stringide** cmdlet või HiveQL skripti faile. Järgmised koodilõigu näitab, kuidas kasutada cmdleti **Invoke-taru** HiveQL skripti faili. HiveQL skriptifail tuleb üles laadida wasbs: / /.
    >
    > `Invoke-AzureRmHDInsightHiveJob -File "wasbs://<ContainerName>@<StorageAccountName>/<Path>/query.hql"`
    >
    > **Siin-stringide**kohta leiate lisateavet teemast <a href="http://technet.microsoft.com/library/ee692792.aspx" target="_blank">Abil Windows PowerShelli siin-stringide</a>.

##<a name="troubleshooting"></a>Tõrkeotsing

Kui pärast töö lõpulejõudmist, tagastatakse andmed, võib olla töötlemise ajal ilmnes tõrge. Selle töö tõrketeabe kuvamiseks lisage järgmine **hivejob.ps1** faili lõppu salvestada, ja seejärel käivitage see uuesti.

    # Print the output of the Hive job.
    Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $job.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

See annab teavet, mis on kirjutatud STDERR serveris kui käivitasite töö ja see võib aidata kindlaks teha, miks töö nurjub.

##<a name="summary"></a>Kokkuvõte

Nagu näete, Azure PowerShelli abil on lihtne taru päringute käivitamiseks on Hdinsightiga kobar, töö oleku jälgimine ja tuua väljund.

##<a name="next-steps"></a>Järgmised sammud

Üldist teavet rakenduses Hdinsightiga taru kohta:

* [Hadoopi Hdinsightiga taru kasutamine](hdinsight-use-hive.md)

Teavet teiste mooduste kohta saate töötada Hadoopi Hdinsightiga:

* [Kasutage siga Hadoopi Hdinsightiga](hdinsight-use-pig.md)

* [Hadoopi Hdinsightiga MapReduce kasutamine](hdinsight-use-mapreduce.md)
