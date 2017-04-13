<properties
    pageTitle="Azure'i ressursihaldur arengu tööriistad Hdinsightiga kogumite migreerimine | Microsoft Azure'i"
    description="Kuidas migreerida Hdinsightiga kogumite Azure'i ressursihaldur arengu tööriistad"
    services="hdinsight"
    editor="cgronlun"
    manager="jhubbard"
    authors="nitinme"
    documentationCenter=""/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="nitinme"/>

# <a name="migrating-to-azure-resource-manager-based-development-tools-for-hdinsight-clusters"></a>Azure'i ressursihaldur arengu tööriistad HDInsight kogumite migreerimine

Hdinsightiga on deprecating Azure Service Manager ASM-põhiste tööriistad Hdinsightiga jaoks. Kui te kasutate Azure PowerShelli, Azure'i CLI või Hdinsightiga .NET SDK töötamiseks Hdinsightiga kogumite, soovitatakse kasutada Azure ressursi halduri ARM-põhise versiooni PowerShelli, CLI ja .NET SDK edasi. Selles artiklis antakse kursoreid kohta, kuidas uue ARM-põhised moodust migreerida. Võimaluse korral selles artiklis märgib ka erinevusi ASM ja ARM lähenemisel HDInsight jaoks.

>[AZURE.IMPORTANT] ASM tugi vastavalt PowerShelli CLI, ja klõpsake **1 Jaanuar 2017**.NET SDK lõpetada.

##<a name="migrating-azure-cli-to-azure-resource-manager"></a>Migreerimine Azure CLI Azure ressursihaldur

Azure'i CLI nüüd on vaikimisi Azure'i ressursi Manager (ARM) režiimis, kui teostate varasem install; Sel juhul peate kasutama funktsiooni `azure config mode arm` käsk ARM lülitumine.

Põhilised käsud, mis Azure'i CLI töötamiseks HDInsight abil Azure'i teenus Management (ASM) on samad, kasutades ARM; kuid mõned parameetrid ja parameetrid võib olla uute nimede ja on palju uusi parameetreid saadaval ARM kasutamisel. Näiteks saate kohe kasutada `azure hdinsight cluster create` Azure virtuaalse võrk, mis on kobar tuleks luua või taru ja Oozie metastore teabe määramiseks.

Põhilised käsud kaudu ressursihaldur Azure Hdinsightiga töötamiseks on.

* `azure hdinsight cluster create`– loob uue Hdinsightiga kobar
* `azure hdinsight cluster delete`-kustutab mõne olemasoleva Hdinsightiga kobar
* `azure hdinsight cluster show`-kuvada teavet mõne olemasoleva kobar
* `azure hdinsight cluster list`-tellimuse Azure Hdinsightiga kogumite loendid

Kasutage funktsiooni `-h` kontrolli parameetrid iga käsu jaoks saadaolevate parameetrite ja aktiveerimine.

###<a name="new-commands"></a>Uue käsud

Uue käsud saadaval Azure ressursihaldur on:

* `azure hdinsight cluster resize`-dünaamiliselt muutub töötaja sõlmed klaster arv
* `azure hdinsight cluster enable-http-access`– lubab HTTPs juurdepääsu klaster (klõpsake vaikimisi)
* `azure hdinsight cluster disable-http-access`-klaster HTTPs access keelab
* `azure hdinsight-enable-rdp-access`– võimaldab kaugtöölaua protokolli Windowsi-põhiste Hdinsightiga klaster
* `azure hdinsight-disable-rdp-access`-keelab kaugtöölaua protokolli Windowsi-põhiste Hdinsightiga klaster
* `azure hdinsight script-action`– pakub loomise ja haldamise toiminguid skripti klaster käsud
* `azure hdinsight config`-annab käsud loomine konfigureerimise faili, mida saab kasutada funktsiooni `hdinsight cluster create` käsk konfiguratsiooni teavet.

###<a name="deprecated-commands"></a>Taunitud käsud

Kui kasutate funktsiooni `azure hdinsight job` käsud Hdinsightiga klaster tööde edastamiseks neid ei ole võimalik ARM käskude abil. Kui teil on vaja programmiliselt edastab tööd Hdinsightiga skriptide, peaksite kasutama REST API-de Hdinsightiga järgi. Esitamise abil REST API-de kohta leiate lisateavet teemast järgmised dokumendid.

* [Käivitage MapReduce töö Hadoopi HDInsight cURL abil](hdinsight-hadoop-use-mapreduce-curl.md)
* [Käivitage Hadoopi taru päringute abil cURL Hdinsightiga](hdinsight-hadoop-use-hive-curl.md)
* [Käivitage siga töö Hadoopi Hdinsightiga cURL abil](hdinsight-hadoop-use-pig-curl.md)

Muul viisil käivitamiseks MapReduce, taru ja siga interaktiivseks kohta lisateabe saamiseks leiate [Kasutamine MapReduce koos Hadoopi Hdinsightiga](hdinsight-use-mapreduce.md), [Kasutamine taru koos Hdinsightiga Hadoopi](hdinsight-use-hive.md)ja [Kasutamine siga Hadoopi Hdinsightiga](hdinsight-use-pig.md).

###<a name="examples"></a>Näited

__Luua klaster__

* Vana käsk (ASM) –`azure hdinsight cluster create myhdicluster --location northeurope --osType linux --storageAccountName mystorage --storageAccountKey <storagekey> --storageContainer mycontainer --userName admin --password mypassword --sshUserName sshuser --sshPassword mypassword`
* Käsk Uus (ARM)`azure hdinsight cluster create myhdicluster -g myresourcegroup --location northeurope --osType linux --clusterType hadoop --defaultStorageAccountName mystorage --defaultStorageAccountKey <storagekey> --defaultStorageContainer mycontainer --userName admin -password mypassword --sshUserName sshuser --sshPassword mypassword`

__Klaster kustutamine__

* Vana käsk (ASM) –`azure hdinsight cluster delete myhdicluster`
* Käsk Uus (ARM)`azure hdinsight cluster delete mycluster -g myresourcegroup`

__Loendi kogumite__

* Vana käsk (ASM) –`azure hdinsight cluster list`
* Käsk Uus (ARM)`azure hdinsight cluster list`

> [AZURE.NOTE] Käsk loendi jaoks täpsustades ressursi rühma abil `-g` tagastab määratud ressursirühm ainult rühmad.

__Kobar teabe kuvamine__

* Vana käsk (ASM) –`azure hdinsight cluster show myhdicluster`
* Käsk Uus (ARM)`azure hdinsight cluster show myhdicluster -g myresourcegroup`


##<a name="migrating-azure-powershell-to-azure-resource-manager"></a>Migreerimine Azure PowerShelli Azure ressursihaldur

Üldteavet Azure PowerShelli Azure'i ressursi Manager (ARM) režiimis leiate [Azure'i ressursihaldur Azure PowerShelli kasutamine](../powershell-azure-resource-manager.md).

ARM Azure PowerShelli cmdlet-käskude võib olla installitud-kõrvuti ASM cmdlet-käskude abil. Cmdlet-käsud: kahe režiimi saab eristada oma nime.  ARM režiim on *AzureRmHDInsight* võrreldes *AzureHDInsight* ASM režiimis cmdlet nimed.  Näiteks *New-AzureRmHDInsightCluster* vs *New-AzureHDInsightCluster*. Parameetrite ja parameetrid võib-olla uudiste nimed ja on palju uusi parameetreid saadaval ARM kasutamisel.  Näiteks mitme cmdlet-käsud vaja *- ResourceGroupName*ehk uue lülitit. 

Enne Hdinsightiga cmdlet-käskude kasutamiseks peab Azure kontoga ühenduse loomiseks ja luua uue ressursirühma.

- Logi sisse – AzureRmAccount või [Valige-AzureRmProfile](https://msdn.microsoft.com/library/mt619310.aspx). Vt [teenuse põhilise Azure'i ressursihaldur autentimine](../resource-group-authenticate-service-principal.md)
- [Uue AzureRmResourceGroup](https://msdn.microsoft.com/library/mt603739.aspx)

###<a name="renamed-cmdlets"></a>Ümbernimetatud cmdlet-käsud

Loendisse Windows PowerShelli konsooli Hdinsightiga ASM cmdlet-käsud:

    help *azurermhdinsight*

Järgmises tabelis on loetletud ASM cmdlet-käskude ja nende nimed ARM režiimis:

|ASM cmdlet-käsud|ARM cmdlet-käsud|
|------|------|
| Lisage AzureHDInsightConfigValues | [Lisage AzureRmHDInsightConfigValues](https://msdn.microsoft.com/library/mt603530.aspx)|
| Lisage AzureHDInsightMetastore |[Lisage AzureRmHDInsightMetastore](https://msdn.microsoft.com/library/mt603670.aspx)|
| Lisage AzureHDInsightScriptAction |[Lisage AzureRmHDInsightScriptAction](https://msdn.microsoft.com/library/mt603527.aspx)|
| Lisage AzureHDInsightStorage |[Lisage AzureRmHDInsightStorage](https://msdn.microsoft.com/library/mt619445.aspx)|
| Get-AzureHDInsightCluster |[Get-AzureRmHDInsightCluster](https://msdn.microsoft.com/library/mt619371.aspx)|
| Get-AzureHDInsightJob |[Get-AzureRmHDInsightJob](https://msdn.microsoft.com/library/mt603590.aspx)|
| Get-AzureHDInsightJobOutput |[Get-AzureRmHDInsightJobOutput](https://msdn.microsoft.com/library/mt603793.aspx)|
| Get-AzureHDInsightProperties |[Get-AzureRmHDInsightProperties](https://msdn.microsoft.com/library/mt603546.aspx) |
| Anda-AzureHDInsightHttpServicesAccess | [Anda-AzureRmHDInsightHttpServicesAccess](https://msdn.microsoft.com/library/mt619407.aspx)|
| Anda-AzureHdinsightRdpAccess |[Anda-AzureRmHDInsightRdpServicesAccess](https://msdn.microsoft.com/library/mt603717.aspx)|
| Autonoomsest AzureHDInsightHiveJob |[Autonoomsest AzureRmHDInsightHiveJob](https://msdn.microsoft.com/library/mt603593.aspx)|
| Uue AzureHDInsightCluster | [Uue AzureRmHDInsightCluster](https://msdn.microsoft.com/library/mt619331.aspx)|
| Uue AzureHDInsightClusterConfig |[Uue AzureRmHDInsightClusterConfig](https://msdn.microsoft.com/library/mt603700.aspx)|
| Uue AzureHDInsightHiveJobDefinition |[Uue AzureRmHDInsightHiveJobDefinition](https://msdn.microsoft.com/library/mt619448.aspx)|
| Uue AzureHDInsightMapReduceJobDefinition |[Uue AzureRmHDInsightMapReduceJobDefinition](https://msdn.microsoft.com/library/mt603626.aspx)|
| Uue AzureHDInsightPigJobDefinition |[Uue AzureRmHDInsightPigJobDefinition](https://msdn.microsoft.com/library/mt603671.aspx)|
| Uue AzureHDInsightSqoopJobDefinition |[Uue AzureRmHDInsightSqoopJobDefinition](https://msdn.microsoft.com/library/mt608551.aspx)|
| Uue AzureHDInsightStreamingMapReduceJobDefinition |[Uue AzureRmHDInsightStreamingMapReduceJobDefinition](https://msdn.microsoft.com/library/mt603626.aspx)|
| Eemalda – AzureHDInsightCluster |[Eemalda – AzureRmHDInsightCluster](https://msdn.microsoft.com/library/mt619431.aspx)|
| Tühista-AzureHDInsightHttpServicesAccess |[Tühista-AzureRmHDInsightHttpServicesAccess](https://msdn.microsoft.com/library/mt619375.aspx)|
| Tühista-AzureHdinsightRdpAccess |[Tühista-AzureRmHDInsightRdpServicesAccess](https://msdn.microsoft.com/library/mt603523.aspx)|
| Set-AzureHDInsightClusterSize |[Set-AzureRmHDInsightClusterSize](https://msdn.microsoft.com/library/mt603513.aspx)|
| Set-AzureHDInsightDefaultStorage |[Set-AzureRmHDInsightDefaultStorage](https://msdn.microsoft.com/library/mt603486.aspx)|
| Algus-AzureHDInsightJob |[Algus-AzureRmHDInsightJob](https://msdn.microsoft.com/library/mt603798.aspx)|
| Lõpeta – AzureHDInsightJob |[Lõpeta – AzureRmHDInsightJob](https://msdn.microsoft.com/library/mt619424.aspx)|
| Kasutage-AzureHDInsightCluster |[Kasutage-AzureRmHDInsightCluster](https://msdn.microsoft.com/library/mt619442.aspx)|
| Oodake-AzureHDInsightJob |[Oodake-AzureRmHDInsightJob](https://msdn.microsoft.com/library/mt603834.aspx)|

###<a name="new-cmdlets"></a>Uued cmdlet-käsud
Järgmised uued cmdlet-käsud, mis on ainult ARM režiimis saadaval. 

**Skripti toimingu seotud cmdlet-käsud:**
- **Get-AzureRmHDInsightPersistedScriptAction**: saab nõutud skripti toimingute jaoks klaster ja need on loetletud ajalises järjestuses või saab määratud nõutud skripti toimingu üksikasjad. 
- **Get-AzureRmHDInsightScriptActionHistory**: saab skripti toimingu ajaloo klaster ja see on loetletud pööratud ajalises järjestuses või saab skripti eelnevalt sooritatud toimingu üksikasju. 
- **Eemalda – AzureRmHDInsightPersistedScriptAction**: nõutud skripti toiming eemaldab mõne Hdinsightiga kobar.
- **Set-AzureRmHDInsightPersistedScriptAction**: määrab skripti eelnevalt sooritatud toimingu olema nõutud skripti toiming.
- **Edasta – AzureRmHDInsightScriptAction**: esitab uue skripti toimingu on Windows Azure Hdinsightiga arvutikobaras. 

Täiendavad kasutus leiate teemast [kohandamine Linux-põhine Hdinsightiga kogumite skripti toimingu abil](hdinsight-hadoop-customize-cluster-linux.md).

**Clsuter identiteedi seotud cmdlet-käsud:**

- **Lisa-AzureRmHDInsightClusterIdentity**: lisab kobar identiteedi kobar konfiguratsiooni objekti Hdinsightiga kobar pääseks juurde Azure'i andmed Lake poed. Vaadake [loomine mõne Hdinsightiga kobar andmete Lake poe Azure PowerShelli kaudu](../data-lake-store/data-lake-store-hdinsight-hadoop-use-powershell.md).

### <a name="examples"></a>Näited

**Looge kobar**

Vana käsk (ASM): 

    New-AzureHDInsightCluster `
        -Name $clusterName `
        -Location $location `
        -DefaultStorageAccountName "$storageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $storageAccountKey `
        -DefaultStorageContainerName $containerName `
        -ClusterSizeInNodes 2 `
        -ClusterType Hadoop `
        -OSType Linux `
        -Version "3.2" `
        -Credential $httpCredential `
        -SshCredential $sshCredential

Käsk Uus (ARM):

    New-AzureRmHDInsightCluster `
        -ClusterName $clusterName `
        -ResourceGroupName $resourceGroupName `
        -Location $location `
        -DefaultStorageAccountName "$storageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $storageAccountKey `
        -DefaultStorageContainer $containerName  `
        -ClusterSizeInNodes 2 `
        -ClusterType Hadoop `
        -OSType Linux `
        -Version "3.2" `
        -HttpCredential $httpcredentials `
        -SshCredential $sshCredentials

 
**Kustutage kobar**

Vana käsk (ASM):

    Remove-AzureHDInsightCluster -name $clusterName 

Käsk Uus (ARM):

    Remove-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $clusterName 
                
**Loendi kobar**

Vana käsk (ASM):

    Get-AzureHDInsightCluster
                
Käsk Uus (ARM):

    Get-AzureRmHDInsightCluster 

**Kuva kobar**

Vana käsk (ASM):

    Get-AzureHDInsightCluster -Name $clusterName
                
Käsk Uus (ARM):

    Get-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -clusterName $clusterName


####<a name="other-samples"></a>Muud näidised

- [Hdinsightiga kogumite loomine](hdinsight-hadoop-create-linux-clusters-azure-powershell.md)
- [Esitage taru tööde haldamine](hdinsight-hadoop-use-hive-powershell.md)
- [Esitage siga tööde haldamine](hdinsight-hadoop-use-pig-powershell.md)
- [Esitage Sqoop tööde haldamine](hdinsight-hadoop-use-sqoop-powershell.md)



##<a name="migrating-to-the-arm-based-hdinsight-net-sdk"></a>ARM-põhised Hdinsightiga .NET SDK migreerimine

Azure'i Teenusehaldus vastavalt [(ASM) Hdinsightiga .NET SDK](https://msdn.microsoft.com/library/azure/mt416619.aspx) on nüüd aegunud. Soovitatakse kasutada Azure ressursside haldamine vastavalt [(ARM) Hdinsightiga .NET SDK](https://msdn.microsoft.com/library/azure/mt271028.aspx). Järgmised ASM-põhise Hdinsightiga paketid on on aegunud.

* `Microsoft.WindowsAzure.Management.HDInsight`
* `Microsoft.Hadoop.Client`
 

Selles jaotises antakse lisateavet viitu kohta, kuidas teatud ülesandeid ARM-põhised SDK abil.

| Kuidas... ARM-põhised Hdinsightiga SDK abil | Lingid |
| ------------------- | --------------- |
| Kasutades .NET SDK Hdinsightiga kogumite loomine| Vt [loomine Hdinsightiga kogumite .NET SDK abil](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md)|
| Skripti toimingut kasutades .NET SDK klaster kohandamine | Vt [kohandamine Hdinsightiga Linux kogumite skripti toimingu abil](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md#use-script-action) |
| Rakenduste Azure Active Directory interaktiivseks kasutades .NET SDK autentida| Lugege teemat [kasutades .NET SDK käivitada taru päringuid](hdinsight-hadoop-use-hive-dotnet-sdk.md). Selles artiklis koodilõigu kasutab interaktiivsed autentimine lähenemine.|
| Rakenduste Azure Active Directory mitte interaktiivseks kasutades .NET SDK autentida | Vt [loomine – interaktiivsed taotlused Hdinsightiga](hdinsight-create-non-interactive-authentication-dotnet-applications.md) |
| Kasutades .NET SDK taru töö edastamine| [Esitage taru projektide](hdinsight-hadoop-use-hive-dotnet-sdk.md) kuvamiseks |
| Kasutades .NET SDK siga töö edastamine | [Esitage siga projektide](hdinsight-hadoop-use-pig-dotnet-sdk.md) kuvamiseks|
| Kasutades .NET SDK Sqoop töö edastamine | [Esitage Sqoop projektide](hdinsight-hadoop-use-sqoop-dotnet-sdk.md) kuvamiseks |
| Loendi Hdinsightiga kogumite, kasutades .NET SDK | Vaadake [loendi Hdinsightiga kogumite](hdinsight-administer-use-dotnet-sdk.md#list-clusters) |
| Skaala Hdinsightiga kogumite, kasutades .NET SDK | Vt [skaala Hdinsightiga kogumite](hdinsight-administer-use-dotnet-sdk.md#scale-clusters) |
| Anda/Tühista juurdepääs Hdinsightiga kogumite, kasutades .NET SDK | Vt [Hdinsightiga kogumite juurdepääsu andmine/Tühista](hdinsight-administer-use-dotnet-sdk.md#grantrevoke-access) |
| HTTP kasutaja mandaat Hdinsightiga kogumite, kasutades .NET SDK värskendamine | Lugege teemat [Update HTTP kasutaja mandaat Hdinsightiga kogumite](hdinsight-administer-use-dotnet-sdk.md#update-http-user-credentials) |
| Kasutades .NET SDK Hdinsightiga kogumite salvestusruumi vaikekonto otsimine | Vt [Hdinsightiga kogumite salvestusruumi vaikekonto otsimine](hdinsight-administer-use-dotnet-sdk.md#find-the-default-storage-account) |
| Kasutades .NET SDK Hdinsightiga kogumite kustutamine | Vt [kogumite Hdinsightiga kustutamine](hdinsight-administer-use-dotnet-sdk.md#delete-clusters) |

### <a name="examples"></a>Näited

Järgnevalt on mõned näited selle kohta, kuidas toiming on logiteavet ASM-põhiste SDK ja võrdväärse koodilõigu ARM-põhised SDK.

**Kobar CRUD kliendi loomine**

* Vana käsk (ASM)

        //Certificate auth
        //This logs the application in using a subscription administration certificate, which is not offered in Azure Resource Manager (ARM)
     
        const string subid = "454467d4-60ca-4dfd-a556-216eeeeeeee1";
        var cred = new HDInsightCertificateCredential(new Guid(subid), new X509Certificate2(@"path\to\certificate.cer"));
        var client = HDInsightClient.Connect(cred);

* Käsk Uus (ARM) (teenuse põhisumma autoriseerimine)

        //Service principal auth
        //This will log the application in as itself, rather than on behalf of a specific user.
        //For details, including how to set up the application, see:
        //   https://azure.microsoft.com/en-us/documentation/articles/hdinsight-create-non-interactive-authentication-dotnet-applications/
         
        var authFactory = new AuthenticationFactory();
         
        var account = new AzureAccount { Type = AzureAccount.AccountType.ServicePrincipal, Id = clientId };
         
        var env = AzureEnvironment.PublicEnvironments[EnvironmentName.AzureCloud];
         
        var accessToken = authFactory.Authenticate(account, env, tenantId, secretKey, ShowDialog.Never).AccessToken;
         
        var creds = new TokenCloudCredentials(subId.ToString(), accessToken);
         
        _hdiManagementClient = new HDInsightManagementClient(creds);

* Käsk Uus (ARM) (Kasutaja autoriseerimine)

        //User auth
        //This will log the application in on behalf of the user.
        //The end-user will see a login popup.
         
        var authFactory = new AuthenticationFactory();
         
        var account = new AzureAccount { Type = AzureAccount.AccountType.User, Id = username };
         
        var env = AzureEnvironment.PublicEnvironments[EnvironmentName.AzureCloud];
         
        var accessToken = authFactory.Authenticate(account, env, AuthenticationFactory.CommonAdTenant, password, ShowDialog.Auto).AccessToken;
         
        var creds = new TokenCloudCredentials(subId.ToString(), accessToken);
         
        _hdiManagementClient = new HDInsightManagementClient(creds);


**Luua klaster**

* Vana käsk (ASM)

        var clusterInfo = new ClusterCreateParameters
                    {
                        Name = dnsName,
                        DefaultStorageAccountKey = key,
                        DefaultStorageContainer = defaultStorageContainer,
                        DefaultStorageAccountName = storageAccountDnsName,
                        ClusterSizeInNodes = 1,
                        ClusterType = type,
                        Location = "West US",
                        UserName = "admin",
                        Password = "*******",
                        Version = version,
                        HeadNodeSize = NodeVMSize.Large,
                    };
        clusterInfo.CoreConfiguration.Add(new KeyValuePair<string, string>("config1", "value1"));
        client.CreateCluster(clusterInfo);


* Käsk Uus (ARM)

        var clusterCreateParameters = new ClusterCreateParameters
            {
                Location = "West US",
                ClusterType = "Hadoop",
                Version = "3.1",
                OSType = OSType.Windows,
                DefaultStorageAccountName = "mystorage.blob.core.windows.net",
                DefaultStorageAccountKey =
                    "O9EQvp3A3AjXq/W27rst1GQfLllhp0gUeiUUn2D8zX2lU3taiXSSfqkZlcPv+nQcYUxYw==",
                UserName = "hadoopuser",
                Password = "*******",
                HeadNodeSize = "ExtraLarge",
                RdpUsername = "hdirp",
                RdpPassword = ""*******",
                RdpAccessExpiry = new DateTime(2025, 3, 1),
                ClusterSizeInNodes = 5
            };
        var coreConfigs = new Dictionary<string, string> {{"config1", "value1"}};
        clusterCreateParameters.Configurations.Add(ConfigurationKey.CoreSite, coreConfigs);


**HTTP juurdepääsu lubamine**

* Vana käsk (ASM)

        client.EnableHttp(dnsName, "West US", "admin", "*******");

* Käsk Uus (ARM)

        var httpParams = new HttpSettingsParameters
        {
               HttpUserEnabled = true,
               HttpUsername = "admin",
               HttpPassword = "*******",
        };
        client.Clusters.ConfigureHttpSettings(resourceGroup, dnsname, httpParams);

**Klaster kustutamine**

* Vana käsk (ASM)

        client.DeleteCluster(dnsName);

* Käsk Uus (ARM)

        client.Clusters.Delete(resourceGroup, dnsname);




