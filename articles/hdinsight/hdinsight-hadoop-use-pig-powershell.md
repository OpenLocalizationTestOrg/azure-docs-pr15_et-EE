<properties
   pageTitle="Kasutage Hadoopi siga Hdinsightiga PowerShelli abil | Microsoft Azure'i"
   description="Saate teada, kuidas edastab siga tööd Hadoopi kobar Hdinsightiga Azure PowerShelli abil sisse."
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
   ms.date="10/11/2016"
   ms.author="larryfr"/>

#<a name="run-pig-jobs-using-powershell"></a>Käivitage PowerShelli kaudu siga tööde haldamine

[AZURE.INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

Selles dokumendis on toodud näide siga tööde Hadoopi Hdinsightiga kobar edastamiseks Azure PowerShelli kaudu. Siga võimaldab teil kirjutada MapReduce töö keele (siga Ladina), et andmed teisendused mudelid, abil, mitte kaarti ja vähendamiseks funktsioonid.

> [AZURE.NOTE] Selles dokumendis ei ole siga Ladina laused, näidetes kasutatud teha üksikasjalikku kirjeldust. Selles näites kasutatakse siga Ladina kohta leiate teavet teemast [Kasutamine siga Hadoopi Hdinsightiga](hdinsight-use-pig.md).

##<a id="prereq"></a>Eeltingimused

Selles artiklis toodud juhiseid tegemiseks on vaja järgmist.

- **An Azure'i tellimus**. Leiate [Azure'i saada tasuta prooviversioon](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- **Töökoha Azure PowerShelli abil**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]


##<a id="powershell"></a>Käivitage PowerShelli kaudu siga tööde haldamine

On toodud Azure PowerShelli *cmdlet-käsud* , mis võimaldavad teil kaugühenduse teel käivitada siga töö Hdinsightiga. Sees, seda saab teha ülejäänud kõned tööta Hdinsightiga klaster [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) (varasema nimega Templeton) abil.

Töötab siga töö remote Hdinsightiga kobar kasutatakse järgmised cmdlet-käsud:

* **Logi sisse – AzureRmAccount**: Azure'i PowerShelli autendib Azure tellimuse

* **Uus-AzureRmHDInsightPigJobDefinition**: loob uue *definitsiooni* määratud siga Ladina-lausete abil

* **Algus-AzureRmHDInsightJob**: saadab töö määratlus Hdinsightiga, käivitab töö ja tagastab *töö* objekti, mida saab kasutada töö oleku kontrollimine

* **Oodake-AzureRmHDInsightJob**: kasutab töö objekti töö oleku vaatamine. See oodake on töö lõpetanud, või ajal on ületatud.

* **Get-AzureRmHDInsightJobOutput**: töö väljundi toomiseks kasutada

Järgmised sammud kirjeldavad töö käitamist Hdinsightiga klaster nende cmdlettide kasutamise kohta.

1. Järgmine kood **pigjob.ps1**redaktoris, salvestada. **CLUSTERNAME** peate asendama klaster Hdinsightiga nime.

        #Login to your Azure subscription
        Login-AzureRmAccount
        #Get credentials for the admin/HTTPs account
        $creds = Get-Credential

        #Specify the cluster name
        $clusterName = "CLUSTERNAME"
        
        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName = $clusterInfo.DefaultStorageAccount.split('.')[0]
        $container = $clusterInfo.DefaultStorageContainer
        $storageAccountKey = (Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value

        #Store the Pig Latin into $QueryString
        $QueryString =  "LOGS = LOAD 'wasbs:///example/data/sample.log';" +
        "LEVELS = foreach LOGS generate REGEX_EXTRACT(`$0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;" +
        "FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;" +
        "GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;" +
        "FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;" +
        "RESULT = order FREQUENCIES by COUNT desc;" +
        "DUMP RESULT;"


        #Create a new HDInsight Pig Job definition
        $pigJobDefinition = New-AzureRmHDInsightPigJobDefinition `
            -Query $QueryString `
            -Arguments "-w"

        # Start the Pig job on the HDInsight cluster
        Write-Host "Start the Pig job ..." -ForegroundColor Green
        $pigJob = Start-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobDefinition $pigJobDefinition `
            -HttpCredential $creds

        # Wait for the Pig job to complete
        Write-Host "Wait for the Pig job to complete ..." -ForegroundColor Green
        Wait-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobId $pigJob.JobId `
            -HttpCredential $creds

        # Display the output of the Pig job.
        Write-Host "Display the standard output ..." -ForegroundColor Green
        Get-AzureRmHDInsightJobOutput `
            -ClusterName $clusterName `
            -JobId $pigJob.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds

2. Avage uus Windows PowerShelli Käsuviip. Muuta kataloogide **pigjob.ps1** faili asukoht ja seejärel käivitage skript järgmise käsu abil.

        .\pigjob.ps1
        
    Teil palutakse esmalt Azure tellimuse sisse logida. Seejärel saate küsida HTTPs/administraatori konto nimi ja parool Hdinsightiga kobar.

7. Pärast töö lõpulejõudmist, see peaks tagastama teavet umbes järgmine:

        Start the Pig job ...
        Wait for the Pig job to complete ...
        Display the standard output ...
        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)

##<a id="troubleshooting"></a>Tõrkeotsing

Kui pärast töö lõpulejõudmist, tagastatakse andmed, võib olla töötlemise ajal ilmnes tõrge. Selle töö tõrketeabe kuvamiseks lisada järgmine käsk **pigjob.ps1** faili lõppu, salvestage see ja käivitage see uuesti.

    # Print the output of the Pig job.
    Write-Host "Display the standard error output ..." -ForegroundColor Green
    Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $pigJob.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

Teavet, mis on kirjutatud STDERR serveris kui käivitasite töö taastatakse ja see võib aidata kindlaks teha, miks töö nurjub.

##<a id="summary"></a>Kokkuvõte

Nagu näete, Azure PowerShelli abil on lihtne siga töö käitamist on Hdinsightiga kobar, töö oleku jälgimine ja tuua väljund.

##<a id="nextsteps"></a>Järgmised sammud

Üldist teavet siga Hdinsightiga kohta:

* [Kasutage siga Hadoopi Hdinsightiga](hdinsight-use-pig.md)

Teavet teiste mooduste kohta saate töötada Hadoopi Hdinsightiga:

* [Hadoopi Hdinsightiga taru kasutamine](hdinsight-use-hive.md)

* [Hadoopi Hdinsightiga MapReduce kasutamine](hdinsight-use-mapreduce.md)
