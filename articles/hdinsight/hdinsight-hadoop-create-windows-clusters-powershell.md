<properties
   pageTitle="Windowsi-põhiste Hadoopi kogumite loomist Hdinsightiga Azure PowerShelli kaudu | Microsoft Azure'i"
    description="Saate teada, kuidas luua kogumite Windows Azure Hdinsightiga Azure PowerShelli abil."
   services="hdinsight"
   documentationCenter=""
   tags="azure-portal"
   authors="mumian"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="08/10/2016"
   ms.author="jgao"/>

# <a name="create-windows-based-hadoop-clusters-in-hdinsight-using-azure-powershell"></a>Windowsi-põhiste Hadoopi kogumite loomist Hdinsightiga Azure PowerShelli abil

[AZURE.INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

Saate teada, kuidas luua Hdinsightiga kogumite Azure PowerShelli kaudu. Azure'i PowerShelli on leiate Azure'i Windows PowerShelli cmdlet-käskude. Muud kobar loomise tööriistade ja funktsioonide klõpsake selle lehe ülaosas valige menüü või näha [kobar loomise võimalused](hdinsight-provision-clusters.md#cluster-creation-methods).


##<a name="prerequisites"></a>Eeltingimused

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Enne alustamist selles artiklis antud juhiseid, peab teil olema järgmised:

- Azure'i tellimus. Leiate [Azure'i saada tasuta prooviversioon](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Azure'i PowerShelli.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

### <a name="access-control-requirements"></a>Accessi kontrolli nõuded

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-clusters"></a>Kogumite loomine
Azure'i PowerShelli on võimas skriptimine keskkonnas, mille abil saate kontrollida ja juurutamise ja haldamise oma töökoormus Azure automatiseerida. Selles jaotises antakse juhiseid, kuidas luua mõne Hdinsightiga kobar Azure PowerShelli abil. Töökoha Hdinsightiga Windows PowerShelli cmdlet-käskude käivitamiseks konfigureerimise kohta leiate teemast [installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md). Hdinsightiga Azure PowerShelli kasutamise kohta lisateabe saamiseks lugege teemat [PowerShelli kaudu haldamise Hdinsightiga](hdinsight-administer-use-powershell.md). Hdinsightiga Windows PowerShelli cmdlet-käskude loendi leiate teemast [Hdinsightiga cmdleti viide](https://msdn.microsoft.com/library/azure/dn858087.aspx).


Järgmised toimingud on vaja luua ka Hdinsightiga kobar Azure PowerShelli abil:

    ####################################
    # Set these variables
    ####################################
    #region - used for creating Azure service names
    $nameToken = "<Enter an Alias>" 
    #endregion

    #region - cluster user accounts
    $httpUserName = "admin"  #HDInsight cluster username
    $httpPassword = "<Enter a Password>"
    #endregion

    ###########################################
    # Service names and varialbes
    ###########################################
    #region - service names
    $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")

    $resourceGroupName = $namePrefix + "rg"
    $hdinsightClusterName = $namePrefix + "hdi"
    $defaultStorageAccountName = $namePrefix + "store"
    $defaultBlobContainerName = $hdinsightClusterName

    $location = "East US 2"
    $clusterSizeInNodes = 1
    #endregion

    # Treat all errors as terminating
    $ErrorActionPreference = "Stop"

    ###########################################
    # Connect to Azure
    ###########################################
    #region - Connect to Azure subscription
    Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
    try{Get-AzureRmContext}
    catch{Login-AzureRmAccount}
    #endregion

    ###########################################
    # Create the resource group
    ###########################################
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location

    ###########################################
    # Preapre default storage account and container
    ###########################################
    New-AzureRmStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $defaultStorageAccountName `
        -Type Standard_GRS `
        -Location $location

    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey `
                                    -ResourceGroupName $resourceGroupName `
                                    -Name $defaultStorageAccountName)[0].Value
    $defaultStorageContext = New-AzureStorageContext `
                                    -StorageAccountName $defaultStorageAccountName `
                                    -StorageAccountKey $defaultStorageAccountKey
    New-AzureStorageContainer `
        -Name $hdinsightClusterName -Context $defaultStorageContext 

    ###########################################
    # Create the cluster
    ###########################################

    $httpPW = ConvertTo-SecureString -String $httpPassword -AsPlainText -Force
    $httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$httpPW)

    New-AzureRmHDInsightCluster `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -Location $location `
        -ClusterSizeInNodes $clusterSizeInNodes `
        -ClusterType Hadoop `
        -OSType Windows `
        -Version "3.2" `
        -HttpCredential $httpCredential `
        -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultStorageContainer $hdinsightClusterName 

    ####################################
    # Verify the cluster
    ####################################
    Get-AzureRmHDInsightCluster -ClusterName $hdinsightClusterName 

## <a name="create-clusters-using-arm-template"></a>Looge kogumite ARM malli abil

Azure'i PowerShelli abil saate juurutada ARM Mall, mis loob mõne Hdinsightiga kobar.  Vt [kõne Mallid Azure PowerShelli kaudu](hdinsight-hadoop-create-windows-clusters-arm-templates.md#call-templates-using-powershell).

##<a name="customize-clusters"></a>Kogumite kohandamine

- Lugege teemat [kohandamine Hdinsightiga kogumite alglaaduri abil](hdinsight-hadoop-customize-cluster-bootstrap.md#use-azure-powershell).
- Lugege teemat [Windows kohandada vastavalt Hdinsightiga kogumite skripti toimingu abil](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).


##<a name="next-steps"></a>Järgmised sammud
Selles artiklis on õppinud loomiseks Hdinsightiga kobar on mitu võimalust. Lisateabe saamiseks lugege järgmisi artikleid:

* [Alustamine Windows Azure Hdinsightiga](hdinsight-hadoop-linux-tutorial-get-started.md) – saate teada, kuidas alustada tööd oma Hdinsightiga kobar
* [Esitage Hadoopi töö programmiliselt](hdinsight-submit-hadoop-jobs-programmatically.md) – saate teada, kuidas programmiliselt edastab tööd Hdinsightiga
* [Hadoopi haldamine kogumite Hdinsightiga PowerShelli kaudu sisse](hdinsight-administer-use-powershell.md) – saate teada, kuidas töötada Hdinsightiga Azure PowerShelli abil
* [Azure Hdinsightiga SDK dokumentatsiooni]  [ hdinsight-sdk-documentation] -Hdinsightiga SDK avastamine




[hdinsight-sdk-documentation]: http://msdn.microsoft.com/library/dn479185.aspx
[azure-preview-portal]: https://manage.windowsazure.com
[connectionmanager]: http://msdn.microsoft.com/library/mt146773(v=sql.120).aspx
[ssispack]: http://msdn.microsoft.com/library/mt146770(v=sql.120).aspx
[ssisclustercreate]: http://msdn.microsoft.com/library/mt146774(v=sql.120).aspx
[ssisclusterdelete]: http://msdn.microsoft.com/library/mt146778(v=sql.120).aspx
