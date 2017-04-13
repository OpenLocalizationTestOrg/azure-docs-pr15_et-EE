<properties
   pageTitle="Hadoopi MapReduce ja PowerShelli abil | Microsoft Azure'i"
   description="Saate teada, kuidas eemalt käivitage MapReduce töö Hadoopi Hdinsightiga PowerShelli abil."
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
   ms.date="08/29/2016"
   ms.author="larryfr"/>

# <a name="run-mapreduce-jobs-with-hadoop-on-hdinsight-using-powershell"></a>Käivitage PowerShelli kaudu Hdinsightiga Hadoopi MapReduce tööde haldamine

[AZURE.INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

Selles dokumendis on toodud näide MapReduce töö loomine Hadoopi sõitma Hdinsightiga kobar Azure PowerShelli kaudu.

##<a id="prereq"></a>Eeltingimused

Selles artiklis toodud juhiseid tegemiseks on vaja järgmist:

- **Windows Azure Hdinsightiga (Hadoopi Hdinsightiga) kobar, (Windowsi-põhiste või Linuxi-põhine)**

- **Töökoha Azure PowerShelli abil**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

##<a id="powershell"></a>MapReduce töö Azure PowerShelli abil

On toodud Azure PowerShelli *cmdlet-käsud* , mis võimaldavad teil kaugühenduse teel käivitada MapReduce töö Hdinsightiga. Sees, seda saab teha ülejäänud kõned tööta Hdinsightiga klaster [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) (varasema nimega Templeton) abil.

Järgmised cmdlet-käsud kasutatakse remote Hdinsightiga kobar MapReduce töö käivitamisel.

* **Logi sisse – AzureRmAccount**: Azure'i PowerShelli autendib Azure tellimuse

* **Uus-AzureRmHDInsightMapReduceJobDefinition**: loob uue *definitsiooni* määratud MapReduce teabe abil

* **Algus-AzureRmHDInsightJob**: saadab töö määratlus Hdinsightiga, käivitab töö ja tagastab *töö* objekti, mida saab kasutada töö oleku kontrollimine

* **Oodake-AzureRmHDInsightJob**: kasutab töö objekti töö oleku vaatamine. See ei pea ootama kuni töö lõpulejõudmist või ajal on ületatud.

* **Get-AzureRmHDInsightJobOutput**: töö väljundi toomiseks kasutada

Järgmised sammud kirjeldavad käivitamiseks töö klaster Hdinsightiga nende cmdlettide kasutamise kohta.

1. Järgmine kood **mapreducejob.ps1**redaktoris, salvestada. **CLUSTERNAME** peate asendama klaster Hdinsightiga nime.

        #Specify the values
        $clusterName = "CLUSTERNAME"
                
        # Login to your Azure subscription
        # Is there an active Azure subscription?
        $sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
        if(-not($sub))
        {
            Login-AzureRmAccount
        }

        #Get HTTPS/Admin credentials for submitting the job later
        $creds = Get-Credential
        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value

        #Create a storage content and upload the file
        $context = New-AzureStorageContext `
            -StorageAccountName $storageAccountName `
            -StorageAccountKey $storageAccountKey
            
        #Define the MapReduce job
        #NOTE: If using an HDInsight 2.0 cluster, use hadoop-examples.jar instead.
        # -JarFile = the JAR containing the MapReduce application
        # -ClassName = the class of the application
        # -Arguments = The input file, and the output directory
        $wordCountJobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
            -JarFile "wasbs:///example/jars/hadoop-mapreduce-examples.jar" `
            -ClassName "wordcount" `
            -Arguments `
                "wasbs:///example/data/gutenberg/davinci.txt", `
                "wasbs:///example/data/WordCountOutput"

        #Submit the job to the cluster
        Write-Host "Start the MapReduce job..." -ForegroundColor Green
        $wordCountJob = Start-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobDefinition $wordCountJobDefinition `
            -HttpCredential $creds

        #Wait for the job to complete
        Write-Host "Wait for the job to complete..." -ForegroundColor Green
        Wait-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobId $wordCountJob.JobId `
            -HttpCredential $creds
        # Download the output
        Get-AzureStorageBlobContent `
            -Blob 'example/data/WordCountOutput/part-r-00000' `
            -Container $container `
            -Destination output.txt `
            -Context $context
        # Print the output
        Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $wordCountJob.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds
            
2. Avage uus **Azure PowerShelli** Käsuviip. Muuta kataloogide **mapreducejob.ps1** faili asukoht ja seejärel käivitage skript järgmise käsu abil.

        .\mapreducejob.ps1
    
    Kui käivitate skript, võidakse teil paluda Azure tellimuse autentimiseks. Teil palutakse ette Hdinsightiga kobar HTTPS/administraatori konto nimi ja parool.

3. Kui töö on lõpule jõudnud, on teil õigus väljund sarnaneb järgmisega:

        Cluster         : CLUSTERNAME
        ExitCode        : 0
        Name            : wordcount
        PercentComplete : map 100% reduce 100%
        Query           :
        State           : Completed
        StatusDirectory : f1ed2028-afe8-402f-a24b-13cc17858097
        SubmissionTime  : 12/5/2014 8:34:09 PM
        JobId           : job_1415949758166_0071

    Selle väljundi näitab töö lõpule viidud.

    > [AZURE.NOTE] Kui väärtus pole 0 on **ExitCode** , lugege teemat [tõrkeotsing](#troubleshooting).

    Selles näites salvestab faili **output.txt** allalaaditud failid kataloogis, kui käivitate käsikirjaga.

###<a name="view-output"></a>Vaate väljund

Avage tekstiredaktoris kuvamiseks sõnade ja loendab toodetud töö **output.txt** fail.

> [AZURE.NOTE] Väljund faili MapReduce töö on püsiv. Seega, kui saate, käivitage see proovi uuesti, peate väljundfail nime muuta.

##<a id="troubleshooting"></a>Tõrkeotsing

Kui pärast töö lõpulejõudmist, tagastatakse andmed, võib olla töötlemise ajal ilmnes tõrge. Selle töö tõrketeabe kuvamiseks lisada järgmine käsk **mapreducejob.ps1** faili lõppu, salvestage see ja käivitage see uuesti.

    # Print the output of the WordCount job.
    Write-Host "Display the standard output ..." -ForegroundColor Green
    Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $wordCountJob.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

See annab teavet, mis on kirjutatud STDERR serveris kui käivitasite töö ja see võib aidata kindlaks teha, miks töö nurjub.

##<a id="summary"></a>Kokkuvõte

Nagu näete, Azure PowerShelli abil on lihtne MapReduce töö käitamist on Hdinsightiga kobar, töö oleku jälgimine ja tuua väljund.

##<a id="nextsteps"></a>Järgmised sammud

Üldist teavet MapReduce töökohtade Hdinsightiga kohta:

* [Hdinsightiga Hadoopi MapReduce kasutamine](hdinsight-use-mapreduce.md)

Teavet teiste mooduste kohta saate töötada Hadoopi Hdinsightiga:

* [Hadoopi Hdinsightiga taru kasutamine](hdinsight-use-hive.md)

* [Kasutage siga Hadoopi Hdinsightiga](hdinsight-use-pig.md)
