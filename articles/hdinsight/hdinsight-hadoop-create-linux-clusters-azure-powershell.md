<properties
    pageTitle="Luua Hadoopi, HBase, torm või säde kogumite Linux Hdinsightiga Azure PowerShelli kaudu | Microsoft Azure'i"
    description="Saate teada, kuidas luua Hadoopi, HBase, torm või säde kogumite Linux Hdinsightiga Azure PowerShelli abil."
    services="hdinsight"
    documentationCenter=""
    authors="nitinme"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="10/05/2016"
    ms.author="nitinme"/>

# <a name="create-linux-based-clusters-in-hdinsight-by-using-azure-powershell"></a>Luua Linux-põhine kogumite Hdinsightiga Azure PowerShelli abil

[AZURE.INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

Azure'i PowerShelli on võimas skriptimine keskkonnas, mille abil saate kontrollida ja juurutamise ja haldamise oma töökoormus Microsoft Azure automatiseerida. Selles dokumendis on toodud teavet loomiseks Linux-põhine Hdinsightiga kobar Azure PowerShelli abil. See hõlmab ka mõni näide skripti.

> [AZURE.NOTE] Azure'i PowerShelli kättesaadav ainult Windows kliente. Kui kasutate mõnda muud klienti Linux, Unix või Mac OS X, teemast Azure CLI luua klaster kasutamise kohta leiate [loomine Linux-põhine Hdinsightiga kobar, kasutades Azure CLI](hdinsight-hadoop-create-linux-clusters-azure-cli.md) .

## <a name="prerequisites"></a>Eeltingimused
Peate enne selle toimingu algust järgmist:

- Azure'i tellimuse. Leiate [Azure'i saada tasuta prooviversioon](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- Azure'i PowerShelli.
    PowerShelli Azure Hdinsightiga kasutamise kohta lisateabe saamiseks lugege teemat [PowerShelli kaudu haldamise Hdinsightiga](hdinsight-administer-use-powershell.md). Hdinsightiga Windows PowerShelli cmdlet-käskude loendi leiate teemast [Hdinsightiga cmdleti viide](https://msdn.microsoft.com/library/azure/dn858087.aspx).

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

### <a name="access-control-requirements"></a>Accessi kontrolli nõuded

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-clusters"></a>Kogumite loomine

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Azure PowerShelli abil saate luua ka Hdinsightiga kobar, peate tegema järgmist:

- Azure'i ressursi rühma loomine
- Azure Storage konto loomine
- Mõne Azure'i bloobimälu container loomine
- Saate luua ka Hdinsightiga kobar

Peate määrama loomiseks Linux kogumite kaks kõige olulisemad parameetrid on need, mis määrata OS tüüp ja SSH kasutaja üksikasjad:

- Veenduge, et määrate parameetri **- OSType** **Linux**.
- Klõpsake rühmad kaugseansi SSH kasutamiseks saate määrata SSH kasutaja parooli või SSH avalik võti. Kui määrate SSH avalik võti nii SSH kasutaja parooli, ignoreeritakse võti. Kui soovite kasutada SSH võti kaugseansi, peate määrama tühja SSH parooli küsimise ühe. Hdinsightiga SSH kasutamise kohta lisateabe saamiseks vt ühte järgmistest artiklitest:

    * [Kasutada SSH Linux-põhine Hadoopi Hdinsightiga Linux, Unix või OS X](hdinsight-hadoop-linux-use-ssh-unix.md)
    * [Kasutada SSH Linux-põhine Hadoopi Windows Hdinsightiga](hdinsight-hadoop-linux-use-ssh-windows.md)

Järgmise skripti näitab, kuidas luua uus klaster:

    $token ="<SpecifyAnUniqueString>"

    $resourceGroupName = $token + "rg"      # Provide a Resource Group name
    $clusterName = $token
    $defaultStorageAccountName = $token + "store"   # Provide a Storage account name
    $defaultStorageContainerName = $token + "container"
    $location = "East US 2"     # Change the location if needed
    $clusterNodes = 1           # The number of nodes in the HDInsight cluster

    # Sign in to Azure
    Login-AzureRmAccount

    # Select the subscription to use if you have multiple subscriptions
    #$subscriptionID = "<SubscriptionName>"        # Provide your Subscription Name
    #Select-AzureRmSubscription -SubscriptionId $subscriptionID

    # Create an Azure Resource Group
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location

    # Create an Azure Storage account and container used as the default storage
    New-AzureRmStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -StorageAccountName $defaultStorageAccountName `
        -Location $location `
        -Type Standard_LRS
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -Name $defaultStorageAccountName -ResourceGroupName $resourceGroupName)[0].Key1
    $destContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey
    New-AzureStorageContainer -Name $defaultStorageContainerName -Context $destContext

    # Create an HDInsight cluster
    $credentials = Get-Credential -Message "Enter Cluster user credentials" -UserName "admin"
    $sshCredentials = Get-Credential -Message "Enter SSH user credentials"

    # The location of the HDInsight cluster must be in the same data center as the Storage account.
    $location = Get-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -StorageAccountName $defaultStorageAccountName | %{$_.Location}

    New-AzureRmHDInsightCluster `
        -ClusterName $clusterName `
        -ResourceGroupName $resourceGroupName `
        -HttpCredential $credentials `
        -Location $location `
        -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultStorageContainer $defaultStorageContainerName  `
        -ClusterSizeInNodes $clusterNodes `
        -ClusterType Hadoop `
        -OSType Linux `
        -Version "3.4" `
        -SshCredential $sshCredentials

Hadoopi klaster jaoks kasutajakonto loomiseks kasutatakse **$clusterCredentials** jaoks määratud väärtused. Kasutada selle kontoga ühenduse loomiseks klaster.

**$sshCredentials** jaoks määratud väärtusi kasutatakse SSH kasutaja jaoks klaster loomiseks. Selle konto abil saate käivitada SSH kaugseansi klaster ja töö.

> [AZURE.IMPORTANT] See kirjas, peate määrama töötaja sõlmed, mis on alati klaster arv. Kui kavatsete kasutada rohkem kui 32 töötaja sõlmed (kas kobar loomisel või skaleerimist klaster pärast loomist), peate määrama vähemalt 8 ja -vormid 14 GB RAM-i abil ka pea sõlme suurus.
>
> Sõlm suurused ja seotud kulude kohta leiate lisateavet teemast [Hdinsightiga hinnad](https://azure.microsoft.com/pricing/details/hdinsight/).

Võib kuluda kuni 20 minutit klaster loomiseks.

Järgmises näites näitab, kuidas täiendavat salvestusruumi konto lisamine:

    # Create another storage account used as additional storage account
    $additionalStorageAccountName = $token + "store2"
    New-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -StorageAccountName $additionalStorageAccountName -Location $location -Type Standard_LRS
    $additionalStorageAccountKey = (Get-AzureRmStorageAccountKey -Name $additionalStorageAccountName -ResourceGroupName $resourceGroupName)[0].Value

    $config = New-AzureRmHDInsightClusterConfig
    Add-AzureRmHDInsightStorage -Config $config -StorageAccountName "$additionalStorageAccountName.blob.core.windows.net" -StorageAccountKey $additionalStorageAccountKey

    # Create a new HDInsight cluster
    New-AzureRmHDInsightCluster `
        -ClusterName $clusterName `
        -ResourceGroupName $resourceGroupName `
        -HttpCredential $credentials `
        -Location $location `
        -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultStorageContainer $defaultStorageContainerName  `
        -ClusterSizeInNodes $clusterNodes `
        -ClusterType Hadoop `
        -OSType Linux `
        -Version "3.4" `
        -SshCredential $sshCredentials `
        -Config $config

## <a name="customize-clusters"></a>Kogumite kohandamine

- Lugege teemat [kohandamine Hdinsightiga kogumite alglaaduri abil](hdinsight-hadoop-customize-cluster-bootstrap.md#use-azure-powershell).
- Lugege teemat [Windows kohandada vastavalt Hdinsightiga kogumite skripti toimingu abil](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).

## <a name="delete-the-cluster"></a>Klaster kustutamine

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a>Järgmised sammud

Nüüd, kui olete loonud mõne Hdinsightiga kobar, kasutage järgmisi ressursse saate teada, kuidas töötada klaster.

### <a name="hadoop-clusters"></a>Hadoopi kogumite

* [Hdinsightiga taru kasutamine](hdinsight-use-hive.md)
* [Kasutage siga Hdinsightiga](hdinsight-use-pig.md)
* [Hdinsightiga MapReduce kasutamine](hdinsight-use-mapreduce.md)

### <a name="hbase-clusters"></a>HBase kogumite

* [Klõpsake Hdinsightiga HBase kasutamise alustamine](hdinsight-hbase-tutorial-get-started-linux.md)
* [Arendamise kohta Hdinsightiga HBase taotlused](hdinsight-hbase-build-java-maven-linux.md)

### <a name="storm-clusters"></a>Torm kogumite

* [Java topoloogiatest arendamise Storm Hdinsightiga kohta](hdinsight-storm-develop-java-topology.md)
* [Kasutage Python komponendid Storm Hdinsightiga kohta](hdinsight-storm-develop-python-topology.md)
* [Juurutada ja kontrollida topoloogiatest torm Hdinsightiga kohta](hdinsight-storm-deploy-monitor-topology-linux.md)

### <a name="spark-clusters"></a>Kogumite säde

* [Kasutades Scala rakendusena loomine](hdinsight-apache-spark-create-standalone-application.md)
* [Käivitage töö eemalt säde klaster Liviuse abil](hdinsight-apache-spark-livy-rest-interface.md)
* [Bi säde: andmeanalüüside interaktiivsed Hdinsightiga säde kasutamine koos Ärianalüüsi tööriistade kohta](hdinsight-apache-spark-use-bi-tools.md)
* [Seadme õppimisega säde: kasutamine säde Hdinsightiga prognoosida toiduga kontrollitulemuste rakenduses](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Säde Streaming: Kasutamine säde rakenduses reaalajas streaming rakenduste Hdinsightiga](hdinsight-apache-spark-eventhub-streaming.md)
