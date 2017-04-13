<properties
    pageTitle="Kohandada Hdinsightiga kogumite skripti toimingute kasutamine | Microsoft Azure'i"
    description="Saate teada, kuidas kohandada Hdinsightiga kogumite skripti toimingu abil."
    services="hdinsight"
    documentationCenter=""
    authors="nitinme"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="nitinme"/>

# <a name="customize-windows-based-hdinsight-clusters-using-script-action"></a>Windowsi-põhiste Hdinsightiga kogumite skripti toimingu abil kohandamine

**Skripti toimingu** saab kasutada [kohandatud skriptide](hdinsight-hadoop-script-actions.md) klaster täiendavat tarkvara installimise käigus kobar loomine.

Selle artikli teave on Windowsi-põhiste Hdinsightiga kogumite. Linux-põhine kogumite, leiate teemast [kohandamine Linux-põhine Hdinsightiga kogumite skripti toimingu abil](hdinsight-hadoop-customize-cluster-linux.md). 

Hdinsightiga kogumite saab kohandada mitmesuguseid muid võimalusi, nt sh Azure Storage täiendavaid kontosid, muutes Hadoopi selle konfiguratsiooni failid (core-site.xml, taru-site.xml jne) või lisada jagatud teekide (nt taru, Oozie) klaster levinud asukohtadesse. Azure'i PowerShelli, Azure Hdinsightiga .NET SDK või Azure portaali kaudu saab teha need kohandused. Lisateabe saamiseks lugege teemat [loomine Hadoopi kogumite sisse Hdinsightiga][hdinsight-provision-cluster].

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell-cli-and-dotnet-sdk.md)]

## <a name="script-action-in-the-cluster-creation-process"></a>Skripti toimingu loomisprotsessi kobar

Skripti toimingut kasutatakse ainult ajal on kogumite protsessi loomist. Järgmine diagramm näitab, kui skripti toimingu käigus loomine.

![Hdinsightiga kobar kohandamine ja etappide kobar loomise ajal][img-hdi-cluster-states]

Skripti töötamisel klaster sisestab **ClusterCustomization** esitusala. Selles etapis skript käivitatakse jaotises süsteemi administraatori konto määratud sõlme klaster, samal ajal ja pakub täielikku administraatoriõigused sõlmed.

> [AZURE.NOTE] Kuna teil on administraatoriõigused kobar sõlmed **ClusterCustomization** etapis, saate skripti nagu peatamine ja alates teenuste, sealhulgas Hadoopi seotud toiminguid. Nii, nagu osa skripti, peate veenduge, et selle Ambari ja muude teenuste Hadoopi seotud oleks tööks enne script on töö lõpetanud. Need teenused on nõutav tuvastamiseks edukalt seisundi ja olek klaster ajal luua. Kui muudate konfiguratsiooni klaster, mis mõjutab järgmisi teenuseid, peate kasutama helper funktsioone, mis on esitatud. Helper funktsioonide kohta leiate lisateavet teemast [arendamise skripti toimingu skriptid Hdinsightiga][hdinsight-write-script].

Väljund ja Tõrkelogide skripti on talletatud klaster määratud salvestusruumi vaikekonto. Tabeli nimi on talletatud logid **u < \cluster-name-fragment >< \time-stamp > setuplog**. Need on liitväärtuse logid käivitada sõlme (pea sõlme ja töötaja sõlmed) klaster käsikirjaga.

Iga kobar aktsepteerida mitut skripti toiminguid, mis on määratud järjestuses käivitas. Skripti saate parandusfunktsiooni pea sõlme, töötaja sõlmed või mõlemad.

Hdinsightiga pakub mitme skriptide Hdinsightiga kogumite installida järgmised komponendid.

Nimi | Skripti
----- | -----
**Installige säde** | https://hdiconfigactions.blob.Core.Windows.net/sparkconfigactionv03/Spark-Installer-V03.ps1. [Installimine ja kasutamine säde Hdinsightiga kogumite kohta]vt[hdinsight-install-spark].
**Installi R** | https://hdiconfigactions.blob.Core.Windows.net/rconfigactionv02/r-Installer-v02.ps1. Vt [installi ja kasutage R Hdinsightiga kogumite][hdinsight-install-r].
**Installige Solri** | https://hdiconfigactions.blob.Core.Windows.net/solrconfigactionv01/Solr-Installer-V01.ps1. Vt [installimine ja kasutamine kogumite Solri Hdinsightiga kohta](hdinsight-hadoop-solr-install.md).
- **Installige Giraph** | https://hdiconfigactions.blob.Core.Windows.net/giraphconfigactionv01/giraph-Installer-V01.ps1. Vt [installimine ja kasutamine kogumite Giraph Hdinsightiga kohta](hdinsight-hadoop-giraph-install.md).
| **Eel-laadida taru teegid** | https://hdiconfigactions.blob.Core.Windows.net/setupcustomhivelibsv01/setup-customhivelibs-V01.ps1. Vt [Hdinsightiga kogumite teekides taru lisamine](hdinsight-hadoop-add-hive-libraries.md) |


## <a name="call-scripts-using-the-azure-portal"></a>Kõne skriptide Azure'i portaalis

**Azure'i portaalis**

1. Käivitage luua klaster, nagu on kirjeldatud aadressil [loomine Hadoopi kogumite Hdinsightiga sisse](hdinsight-provision-clusters.md#portal).
2. Valikuline konfigureerimise **Skripti toimingud** Blade, klõpsake jaotises **skripti toimingu lisamine** üksikasjad skripti kohta, nagu allpool näidatud:

    ![Kasutage skripti toimingu kohandamiseks on kobar] (./media/hdinsight-hadoop-customize-cluster/HDI.CreateCluster.8.png "Kasutage skripti toimingu kohandamiseks on kobar")

    <table border='1'>
        <tr><th>Atribuut</th><th>Väärtus</th></tr>
        <tr><td>Nimi</td>
            <td>Määrake skripti toimingu nimi.</td></tr>
        <tr><td>Skripti URI</td>
            <td>Määrake URI skripti, mis on vaja järgida klaster kohandamiseks. s</td></tr>
        <tr><td>Juhi/töötaja</td>
            <td>Saate määrata kohandamine skripti käitatakse sõlmed (**juht** või **töötaja**). </b>.
        <tr><td>Parameetrid</td>
            <td>Määrake soovitud parameetrid skripti vajadusel.</td></tr>
    </table>

    Vajutage sisestusklahvi ENTER lisada rohkem kui ühe skripti toimingu klaster mitme komponentide installimine.

3. Klõpsake nuppu **Valige** skripti toimingu konfiguratsiooni salvestamiseks ja jätkake kobar loomine.

## <a name="call-scripts-using-azure-powershell"></a>Kõne skriptide Azure PowerShelli abil

Selle järgmist PowerShelli skripti näitab, kuidas installida Windows vastavalt Hdinsightiga kobar säde.  

    # Provide values for these variables
    $subscriptionID = "<Azure Suscription ID>" # After "Login-AzureRmAccount", use "Get-AzureRmSubscription" to list IDs.

    $nameToken = "<Enter A Name Token>"  # The token is use to create Azure service names.
    $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")
    
    $resourceGroupName = $namePrefix + "rg"
    $location = "EAST US 2" # used for creating resource group, storage account, and HDInsight cluster.
    
    $hdinsightClusterName = $namePrefix + "spark"
    $httpUserName = "admin"
    $httpPassword = "<Enter a Password>"
    
    $defaultStorageAccountName = "$namePrefix" + "store"
    $defaultBlobContainerName = $hdinsightClusterName
    
    #############################################################
    # Connect to Azure
    #############################################################
    
    Try{
        Get-AzureRmSubscription
    }
    Catch{
        Login-AzureRmAccount
    }
    Select-AzureRmSubscription -SubscriptionId $subscriptionID
    
    #############################################################
    # Prepare the dependent components
    #############################################################
    
    # Create resource group
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
    
    # Create storage account
    New-AzureRmStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $defaultStorageAccountName `
        -Location $location `
        -Type Standard_GRS
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey `
                                    -ResourceGroupName $resourceGroupName `
                                    -Name $defaultStorageAccountName)[0].Value
    $defaultStorageAccountContext = New-AzureStorageContext `
                                    -StorageAccountName $defaultStorageAccountName `
                                    -StorageAccountKey $storageAccountKey  
    New-AzureStorageContainer `
        -Name $defaultBlobContainerName `
        -Context $defaultStorageAccountContext
    
    #############################################################
    # Create cluster with ApacheSpark
    #############################################################
    
    # Specify the configuration options
    $config = New-AzureRmHDInsightClusterConfig `
                -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
                -DefaultStorageAccountKey $defaultStorageAccountKey 
                
    
    # Add a script action to the cluster configuration
    $config = Add-AzureRmHDInsightScriptAction `
                -Config $config `
                -Name "Install Spark" `
                -NodeType HeadNode `
                -Uri https://hdiconfigactions.blob.core.windows.net/sparkconfigactionv03/spark-installer-v03.ps1 `
    
    # Start creating a cluster with Spark installed
    New-AzureRmHDInsightCluster `
            -ResourceGroupName $resourceGroupName `
            -ClusterName $hdinsightClusterName `
            -Location $location `
            -ClusterSizeInNodes 2 `
            -ClusterType Hadoop `
            -OSType Windows `
            -DefaultStorageContainer $defaultBlobContainerName `
            -Config $config


Muu tarkvara installimiseks peate asendamine skriptifail skripti:


Kui kuvatakse vastav viip, sisestage mandaat klaster. Võib kuluda mitu minutit enne klaster on loodud.

## <a name="call-scripts-using-net-sdk"></a>Kõne skriptide .NET SDK abil 

Järgmises näites näitab, kuidas installida Windows vastavalt Hdinsightiga kobar säde. Muu tarkvara installimiseks peate asendama skriptifail kood.

**Luua ka Hdinsightiga kobar säde** 

1. Luua Visual Studio C# konsooli rakendus.
2. Nugeti Package Manager konsool, käivitage järgmine käsk.

        Install-Package Microsoft.Rest.ClientRuntime.Azure.Authentication -Pre
        Install-Package Microsoft.Azure.Management.ResourceManager -Pre
        Install-Package Microsoft.Azure.Management.HDInsight

2. Kasutage järgmist laused Program.cs faili abil:

        using System;
        using System.Security;
        using Microsoft.Azure;
        using Microsoft.Azure.Management.HDInsight;
        using Microsoft.Azure.Management.HDInsight.Models;
        using Microsoft.Azure.Management.ResourceManager;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Rest;
        using Microsoft.Rest.Azure.Authentication;

3. Koodi paigutada tunni järgmiselt:

        private static HDInsightManagementClient _hdiManagementClient;

        // Replace with your AAD tenant ID if necessary
        private const string TenantId = UserTokenProvider.CommonTenantId; 
        private const string SubscriptionId = "<Your Azure Subscription ID>";
        // This is the GUID for the PowerShell client. Used for interactive logins in this example.
        private const string ClientId = "1950a258-227b-4e31-a9cf-717495945fc2";
        private const string ResourceGroupName = "<ExistingAzureResourceGroupName>";
        private const string NewClusterName = "<NewAzureHDInsightClusterName>";
        private const int NewClusterNumWorkerNodes = 2;
        private const string NewClusterLocation = "East US";
        private const string NewClusterVersion = "3.2";
        private const string ExistingStorageName = "<ExistingAzureStorageAccountName>";
        private const string ExistingStorageKey = "<ExistingAzureStorageAccountKey>";
        private const string ExistingContainer = "<ExistingAzureBlobStorageContainer>";
        private const string NewClusterType = "Hadoop";
        private const OSType NewClusterOSType = OSType.Windows;
        private const string NewClusterUsername = "<HttpUserName>";
        private const string NewClusterPassword = "<HttpUserPassword>";

        static void Main(string[] args)
        {
            System.Console.WriteLine("Running");

            // Authenticate and get a token
            var authToken = Authenticate(TenantId, ClientId, SubscriptionId);
            // Flag subscription for HDInsight, if it isn't already.
            EnableHDInsight(authToken);
            // Get an HDInsight management client
            _hdiManagementClient = new HDInsightManagementClient(authToken);

            CreateCluster();
        }

        private static void CreateCluster()
        {
            var parameters = new ClusterCreateParameters
            {
                ClusterSizeInNodes = NewClusterNumWorkerNodes,
                Location = NewClusterLocation,
                ClusterType = NewClusterType,
                OSType = NewClusterOSType,
                Version = NewClusterVersion,

                DefaultStorageAccountName = ExistingStorageName,
                DefaultStorageAccountKey = ExistingStorageKey,
                DefaultStorageContainer = ExistingContainer,

                UserName = NewClusterUsername,
                Password = NewClusterPassword,
            };

            ScriptAction sparkScriptAction = new ScriptAction("Install Spark",
                new Uri("https://hdiconfigactions.blob.core.windows.net/sparkconfigactionv03/spark-installer-v03.ps1"), "");

            parameters.ScriptActions.Add(ClusterNodeType.HeadNode, new System.Collections.Generic.List<ScriptAction> { sparkScriptAction });
            parameters.ScriptActions.Add(ClusterNodeType.WorkerNode, new System.Collections.Generic.List<ScriptAction> { sparkScriptAction });

            _hdiManagementClient.Clusters.Create(ResourceGroupName, NewClusterName, parameters);
        }

        /// <summary>
        /// Authenticate to an Azure subscription and retrieve an authentication token
        /// </summary>
        /// <param name="TenantId">The AAD tenant ID</param>
        /// <param name="ClientId">The AAD client ID</param>
        /// <param name="SubscriptionId">The Azure subscription ID</param>
        /// <returns></returns>
        static TokenCloudCredentials Authenticate(string TenantId, string ClientId, string SubscriptionId)
        {
            var authContext = new AuthenticationContext("https://login.microsoftonline.com/" + TenantId);
            var tokenAuthResult = authContext.AcquireToken("https://management.core.windows.net/", 
                ClientId, 
                new Uri("urn:ietf:wg:oauth:2.0:oob"), 
                PromptBehavior.Always, 
                UserIdentifier.AnyUser);
            return new TokenCloudCredentials(SubscriptionId, tokenAuthResult.AccessToken);
        }
        /// <summary>
        /// Marks your subscription as one that can use HDInsight, if it has not already been marked as such.
        /// </summary>
        /// <remarks>This is essentially a one-time action; if you have already done something with HDInsight
        /// on your subscription, then this isn't needed at all and will do nothing.</remarks>
        /// <param name="authToken">An authentication token for your Azure subscription</param>
        static void EnableHDInsight(TokenCloudCredentials authToken)
        {
            // Create a client for the Resource manager and set the subscription ID
            var resourceManagementClient = new ResourceManagementClient(new TokenCredentials(authToken.Token));
            resourceManagementClient.SubscriptionId = SubscriptionId;
            // Register the HDInsight provider
            var rpResult = resourceManagementClient.Providers.Register("Microsoft.HDInsight");
        }


4. Vajutage klahvi **F5** käivitage rakendus.


## <a name="support-for-open-source-software-used-on-hdinsight-clusters"></a>Avatud lähtekoodi tarkvara kasutada Hdinsightiga kogumite tugi
Microsoft Azure Hdinsightiga teenus on paindlik platvorm, mis võimaldab teil luua andmete – suured rakendusi pilves, kasutades avatud lähtekoodi tehnoloogiad loodi Hadoopi ökosüsteemi. Microsoft Azure'i annab üldine toetustase avatud lähtekoodi tehnoloogiale, nagu <a href="http://azure.microsoft.com/support/faq/" target="_blank">Azure'i toe KKK veebisaidi</a>jaotises **Ulatus toetavad** . Hdinsightiga teenus pakub toetavad mõned komponendid, nagu allpool kirjeldatud.

On kahte tüüpi avatud lähtekoodi komponendid, mis on saadaval teenuses Hdinsightiga.

- **Sisseehitatud komponendid** - järgmised komponendid on eelinstallitud opsüsteemi Hdinsightiga kogumite ja pakuvad klaster põhifunktsioone. Näiteks LÕNG ResourceManager, taru päringukeele (HiveQL) ja Mahout teegi kuuluvad kategooriasse. Täieliku loendi kobar komponendid on saadaval [mis on uut rakenduses Hadoopi kobar versioonide pakutavast Hdinsightiga?](hdinsight-component-versioning.md) </a>.
- **Kohandatud komponendid** -, kui kasutaja kobar, saate installida või kasutada mis tahes osa ühenduse või teie poolt loodud oma töökoormus.

Sisseehitatud komponendid on täielikult toetatud ja Microsoft Support aitab eristada ja nende komponentide seotud probleemide lahendamiseks.

> [AZURE.WARNING] Koos Hdinsightiga kobar komponendid on täielikult toetatud ja Microsoft Support aitab eristada ja nende komponentide seotud probleemide lahendamiseks.
>
> Kohandatud komponendid tugiteenuseid äriliselt mõistlik aidata teil selle probleemi tõrkeotsingu sooritamiseks. See võib põhjustada probleemi lahendamine või palub teil otsimist ja avatud allika tehnoloogiaid, kui leitakse sügav teadmised selle tehnoloogia. Näiteks on palju kogukonnafoorumi saite, mida saab kasutada, nt: [MSDN-i Foorum Hdinsightiga](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Ka Apache projektide on projekti saitidel [http://apache.org](http://apache.org), näiteks: [Hadoopi](http://hadoop.apache.org/), [säde](http://spark.apache.org/).

Hdinsightiga teenus on mitu võimalust kasutada kohandatud komponendid. Sõltumata sellest, kuidas komponent on kasutatud või klaster installitud, kehtib tugi samal tasemel. Allpool on kõige levinum viise, et kohandatud komponendid saab kasutada Hdinsightiga kogumite loend.

1. Töö esitamise - Hadoopi või muud tüüpi tööd, käivitada või kasutada kohandatud komponendid saab esitada klaster.
2. Kobar kohandamine - kobar loomise ajal saate määrata täiendavaid sätteid ja kohandatud komponendid, mis installitakse kobar sõlmed.
3. Proovi - populaarsed kohandatud komponendid, Microsoft ja teistele anda näidised, kuidas need komponendid saab kasutada Hdinsightiga rühmad. Need näited on saadaval ilma tugi.

## <a name="develop-script-action-scripts"></a>Skripti toimingu skriptide töötada

Vt [arendamise skripti toimingu skriptid Hdinsightiga][hdinsight-write-script].


## <a name="see-also"></a>Vt ka

- [Loomine Hadoopi kogumite Hdinsightiga] [ hdinsight-provision-cluster] pakub juhiseid, kuidas luua mõne Hdinsightiga kobar muude kohandatud suvandite abil.
- [Töötada skripti toimingu skriptide Hdinsightiga][hdinsight-write-script]
- [Installimine ja kasutamine säde Hdinsightiga kogumite][hdinsight-install-spark]
- [Installimine ja kasutamine R Hdinsightiga kogumite][hdinsight-install-r]
- [Installimine ja kasutamine kogumite Solri Hdinsightiga kohta](hdinsight-hadoop-solr-install.md).
- [Installimine ja kasutamine kogumite Giraph Hdinsightiga kohta](hdinsight-hadoop-giraph-install.md).

[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-install-r]: hdinsight-hadoop-r-scripts.md
[hdinsight-write-script]: hdinsight-hadoop-script-actions.md
[hdinsight-provision-cluster]: hdinsight-provision-clusters.md
[powershell-install-configure]: powershell-install-configure.md


[img-hdi-cluster-states]: ./media/hdinsight-hadoop-customize-cluster/HDI-Cluster-state.png "Etappide kobar loomise ajal"
