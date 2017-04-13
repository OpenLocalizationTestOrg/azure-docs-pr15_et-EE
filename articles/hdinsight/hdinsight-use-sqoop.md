<properties
    pageTitle="Hadoopi Sqoop kasutamine HDInsight | Microsoft Azure'i"
    description="Saate teada, kuidas kasutada Azure PowerShelli töökoha käivitamiseks Sqoop importimine ja eksportimine mõnda Hadoopi kobar ja Azure SQL-andmebaasi vahel."
    editor="cgronlun"
    manager="jhubbard"
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/02/2016"
    ms.author="jgao"/>

#<a name="use-sqoop-with-hadoop-in-hdinsight"></a>Hadoopi sisse Hdinsightiga Sqoop kasutamine

[AZURE.INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

Saate teada, kuidas kasutada Sqoop Hdinsightiga importimiseks ja eksportimiseks Hdinsightiga kobar ja Azure SQL-andmebaasi või SQL serveri andmebaasi vahelise.

Kuigi Hadoopi on loomulikus valik töötlemiseks semistructured ja struktureerimata andmed, nt logid ja faile, samuti võib vaja struktureeritud andmeid, mis on talletatud relatsioonandmebaasidest.

[Sqoop] [ sqoop-user-guide-1.4.4] on mõeldud andmete edastamiseks Hadoopi kogumite ja relatsiooniandmebaasid tööriist. Andmete importimiseks relatsiooniandmebaasist haldamise süsteemi (RDBMS), nagu SQL Server, MySQL-i või Oracle süsteemi Hadoopi fail (HDFS) MapReduce või taru Hadoopi andmeid muuta, ja seejärel eksportige andmed tagasi mõne RDBMS saate seda kasutada. Selles õpetuses kasutate oma relatsiooniandmebaasist SQL serveri andmebaasi.

Sqoop versioonid, mis on toetatud Hdinsightiga kogumite, lugege teemat [mis on uut esitatud Hdinsightiga kobar versioonides?] [hdinsight-versions].

##<a name="understand-the-scenario"></a>Stsenaarium mõistmine

Hdinsightiga kobar sisaldab mõningaid näidisandmeid. Kasutage järgmised kaks näidet:

- Log4j logifaili, mis asub */example/data/sample.log*. Järgmised logid ekstraktitakse fail:

        2012-02-03 18:35:34 SampleClass6 [INFO] everything normal for id 577725851
        2012-02-03 18:35:34 SampleClass4 [FATAL] system problem at id 1991281254
        2012-02-03 18:35:34 SampleClass3 [DEBUG] detail for id 1304807656
        ...

- Taru tabelis nimega *hivesampletable*, mis viitab andmete failid, mis asuvad */hive/warehouse/hivesampletable*. Tabel sisaldab mobiilsideseadme andmeid. 

  	| Väli | Andmetüüp |
  	| ----- | --------- |
  	| clientid | string |
  	| querytime | string |
  	| Market | string |
  	| deviceplatform | string |
  	| devicemake | string |
  	| devicemodel | string |
  	| olek | string |
  	| riik | string |
  	| querydwelltime | kahekordne |
  	| SeansiId | suur täisarv |
  	| sessionpagevieworder | suur täisarv |

Saate eksportida *sample.log* ja *hivesampletable* SQL Azure'i andmebaasi või SQL serveri ja importige tabel, mis sisaldab mobiilsideseadme andmete HDInsight, kasutades järgmises asukohas:

    /tutorials/usesqoop/importeddata

## <a name="create-cluster-and-sql-database"></a>Kobar ja SQL-andmebaasi loomine

Selles jaotises kirjeldatakse, kuidas luua klaster ja SQL-i andmebaasi skeemid töötab õpetuse Azure portaali ja Azure ressursihaldur malli abil.  Kui eelistate kasutada Azure PowerShelli, vt [lisa A](#appendix-a---a-powershell-sample).

1. Järgmisel pildil Azure'i portaalis ressursi halduri malli avamiseks klõpsake.         

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Fusesqoop%2Fcreate-linux-based-hadoop-cluster-in-hdinsight-and-sql-database.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>
    
    Ressursi halduri Mall asub *https://hditutorialdata.blob.core.windows.net/usesqoop/create-linux-based-hadoop-cluster-in-hdinsight-and-sql-database.json*avaliku bloobimälu ümbrises. 
    
    Ressursi halduri malli kõned bacpac paketi juurutamiseks tabeli skeemid SQL-andmebaasiga.  Paketi bacpac asub ka https://hditutorialdata.blob.core.windows.net/usesqoop/SqoopTutorial-2016-2-23-11-2.bacpac avaliku bloobimälu ümbrises. Kui soovite kasutada privaatne container bacpac faile, kasutada malli järgmised väärtused:
    
        "storageKeyType": "Primary",
        "storageKey": "<TheAzureStorageAccountKey>",
    
2. Keelest parameetrid sisestage järgmine:

    - **ClusterName**: sisestage Hadoopi kobar loodava nimi.
    - **Kobar kasutajanime ja parooli**: login vaikenimi on haldus.
    - **SSH kasutajanimi ja parool**.
    - **SQL-andmebaasi serveri sisselogimisnimi ja parool**.

    Kõva muutujate jaotises järgmised väärtused:
    
  	|Vaikimisi salvestusruumikonto nimi|<CluterName>pood|
  	|----------------------------|-----------------|
  	|Azure SQL-i andmebaasiserveri nimi|<ClusterName>dbserver|
  	|Azure SQL-i andmebaasi nimi|<ClusterName>dB|
    
    Palun kirjutage need väärtused.  Te peate neid hiljem õpetuse.
    
3. Valige parameetrid salvestamiseks nuppu **OK** .

4. **kohandatud juurutamise** keelest, klõpsake **ressursirühm** rippmenüüst ja seejärel klõpsake nuppu **Uus** , et luua uue ressursirühma. Ressursirühma on ümbris, mis pakuvad klaster, sõltuvad salvestusruumi konto ja muud lingitud ressurssi.

5. klõpsake **õiguslikult**ja seejärel klõpsake nuppu **Loo**.

6. klõpsake nuppu **Loo**. Teile kuvatakse uue nimega esitamine juurutamise malli juurutamiseks paani. Kulub umbes 20 minutit kobar ja SQL-andmebaasi loomiseks.

Kui otsustate kasutada olemasoleva SQL Azure'i andmebaasi või Microsoft SQL Server

- **Azure'i SQL-andmebaasi**: konfigureerige tulemüür reegli Azure SQL-i andmebaasiserver, kui soovite lubada juurdepääsu oma töökoha. Juhised leiate Azure'i SQL-i loomise andmebaasi ja konfigureerida tulemüüri, leiate [Azure'i SQL-andmebaasi kasutamise alustamine][sqldatabase-get-started]. 

    > [AZURE.NOTE] Vaikimisi võimaldab Azure SQL-andmebaasi Azure teenuseid, näiteks Windows Azure Hdinsightiga ühendusi. Kui see säte tulemüür on keelatud, peab see lubatud Azure portaalist. Juhise Azure SQL-andmebaasi loomine ja konfigureerimine tulemüüri reeglid, vt [loomine ja konfigureerimine SQL-andmebaasi][sqldatabase-create-configue].

- **SQL serveri**: kui Hdinsightiga klaster on sama virtuaalse võrgus Azure SQL serveri, saate juhiseid selle artikli importida ja eksportida andmed SQL serveri andmebaasi.

    > [AZURE.NOTE] Hdinsightiga toetab ainult asukoht põhineb virtuaalse võrgu ja see ei tööta praegu osaleja rühma-põhiste virtuaalse võrgustikke.

    * Loomiseks ja konfigureerimiseks virtuaalse võrk, vt [virtuaalse võrgu Azure portaali loomine](../virtual-network/virtual-networks-create-vnet-arm-pportal.md).

        * Kui kasutate SQL serveri oma andmekeskuses, tuleb konfigureerida virtuaalse võrgu *- saidilt* või *saidi punkti*.

            > [AZURE.NOTE] **Punkti saidi** virtuaalne võrkude, SQL Server peab töötama VPN klient konfigureerimisrakenduse, mis on saadaval Azure virtuaalse võrgukonfiguratsioon **armatuurlaud** .

        * Kui kasutate SQL Server Azure'i virtual arvutis, mis tahes virtuaalse võrgu konfigureerimise saab kasutada kui virtuaalse masina hostiteenuse SQL Server kuulub HDInsight virtuaalse samasse võrku.

    * Mõne Hdinsightiga kobar virtuaalse võrgu loomiseks vaadake teemat [loomine Hadoopi kogumite rakenduses Hdinsightiga kohandatud suvandite abil](hdinsight-provision-clusters.md)

    > [AZURE.NOTE] SQL serveri ka peavad lubama autentimist. Selles artiklis toodud juhiseid lõpule viimiseks peate kasutama SQL serveri login.


## <a name="run-sqoop-jobs"></a>Käivitage Sqoop tööde haldamine

Hdinsightiga saab käitada Sqoop tööd, kasutades mitmesuguseid viise. Järgmise tabeli abil saate otsustada, milline viis teile sobib, siis järgige lühiülevaade linki.

| **Kasutage seda** , kui soovite...                                   | .. .an **interaktiivsed** shell | ... **Pakktöötlus** | .. ei ole see **kobar operatsioonisüsteem** | .. .minu **Kliendi operatsioonisüsteem** |
|:--------------------------------------------------------------|:---------------------------:|:-----------------------:|:------------------------------------------|:-----------------------------------------|
| [SSH](hdinsight-use-sqoop-mac-linux.md)                        |              ✔              |            ✔            | Linux                                     | Linux, Unix, Mac OS X või Windows        |
| [.NET SDK Hadoopi jaoks](hdinsight-hadoop-use-sqoop-dotnet-sdk.md) |           &nbsp;            |            ✔            | Linux või Windows                          | Windowsi (vähemalt praegu)                        |
| [Azure'i PowerShelli](hdinsight-hadoop-use-sqoop-powershell.md)  |           &nbsp;            |            ✔            | Linux või Windows                          | Windows                                  |

##<a name="limitations"></a>Piirangud

* Hulgi ekspordi - ja Linux-põhine HDInsight, kasutatakse Microsoft SQL Server või Azure'i SQL-andmebaasi andmete eksportimine Sqoop konnektor ei toeta praegu hulgi lisab.

* Partiide - ja Linux-põhine Hdinsightiga, kasutades funktsiooni `-batch` vahetada, kui lisatakse, Sqoop täidab mitme lisab asemel partiide lisa toimingud.

##<a name="next-steps"></a>Järgmised sammud

Nüüd olete õppinud, kuidas kasutada Sqoop. Lisateavet leiate järgmistest teemadest.

- [Hdinsightiga taru kasutamine](hdinsight-use-hive.md)
- [Kasutage siga Hdinsightiga](hdinsight-use-pig.md)
- [Oozie kasutamine Hdinsightiga][hdinsight-use-oozie]: kasutamine Sqoop toimingu Oozie töövoos.
- [Analüüsida viivitus lennuandmetega abil Hdinsightiga][hdinsight-analyze-flight-data]: flight analüüsimiseks kasutada taru viivitamine andmed ja Azure SQL-andmebaasi andmete eksportimine Sqoop abil.
- [Laadi andmed Hdinsightiga][hdinsight-upload-data]: Otsige muid viise andmete üleslaadimiseks Hdinsightiga/Azure'i bloobimälu.


## <a name="appendix-a---a-powershell-sample"></a>Lisa A - PowerShelli näidis

PowerShelli valimi teeb järgmist:

1. Ühenduse Azure'i.
2. Azure'i ressursi rühma loomine. Lisateavet leiate teemast [Azure ressursihaldur Azure PowerShelli kasutamine](../powershell-azure-resource-manager.md)
3. Azure SQL-i andmebaasiserver, Azure'i SQL-andmebaasiga ja kahe tabeli loomine. 

    Kui kasutate selle asemel SQL serveri, tabelite loomiseks kasutada järgmistest:
    
        CREATE TABLE [dbo].[log4jlogs](
         [t1] [nvarchar](50),
         [t2] [nvarchar](50),
         [t3] [nvarchar](50),
         [t4] [nvarchar](50),
         [t5] [nvarchar](50),
         [t6] [nvarchar](50),
         [t7] [nvarchar](50))

        CREATE TABLE [dbo].[mobiledata](
         [clientid] [nvarchar](50),
         [querytime] [nvarchar](50),
         [market] [nvarchar](50),
         [deviceplatform] [nvarchar](50),
         [devicemake] [nvarchar](50),
         [devicemodel] [nvarchar](50),
         [state] [nvarchar](50),
         [country] [nvarchar](50),
         [querydwelltime] [float],
         [sessionid] [bigint],
         [sessionpagevieworder][bigint])

    Lihtsaim viis tutvuda andmebaas ja tabelid on kasutada Visual Studio. Andmebaasiserveri ja andmebaasi saate uurida Azure'i portaalis.

4. Saate luua ka Hdinsightiga kobar.

    Klaster uurida, saate kasutada Azure portaali või Azure PowerShelli.

5. Eelnevalt protsessi andmefailis.

    Selles õpetuses on taru tabeli log4j logifaili (eraldajaga fail) ja Azure SQL-andmebaasi eksportida. Eraldajaga fail nimega */example/data/sample.log*. Õpetuses, teile kuvati mõned näited log4j logid. Logifaili, on mõned tühjad read ja mõned read sarnased nendele.
    
        java.lang.Exception: 2012-02-03 20:11:35 SampleClass2 [FATAL] unrecoverable system problem at id 609774657
            at com.osa.mocklogger.MockLogger$2.run(MockLogger.java:83)
    
    See on hea teisi näiteid, mida kasutada neid andmeid, kuid me peate eemaldama erandite enne me saate importida Azure SQL andmebaasis või SQL Server. Sqoop eksportimine ei õnnestu, kui seal on tühja stringi või rida on vähem kui määratletud SQL Azure'i andmebaasi tabeli väljade arv elementide arv. Log4jlogs tabelis on 7 stringi tüüpi väljad.

    See toiming loob uue faili klaster: tutorials/usesqoop/data/sample.log. Uurige faili muutmiskuupäev, saate kasutada Azure portaali, mis Azure Storage Exploreri tööriista või Azure PowerShelli. [Alustamine Hdinsightiga] [ hdinsight-get-started] on mõeldud Azure PowerShelli abil faili alla laadida ja faili sisu kuvamine proovi kood.

6. SQL Azure'i andmebaasi andmete faili eksportida.

    Lähtefail on tutorials/usesqoop/data/sample.log. Tabel, kus andmed on eksporditud nimetatakse log4jlogs.
    
    > [AZURE.NOTE] Ühendusestringi teave, v.a selle jaotise juhised peaks töötama Azure SQL-andmebaasi või SQL Server. Need juhised on testitud järgmist konfiguratsiooni:
    >
    > * **Azure virtuaalse punkti saidi võrgukonfiguratsioon**: virtuaalse võrku ühendatud HDInsight kobar SQL serveri privaatne andmekeskuses. Lisateabe saamiseks vaadake [konfigureerimine punkti saidi VPN haldusportaal](../vpn-gateway/vpn-gateway-point-to-site-create.md) .
    > * **Azure Hdinsightiga 3,1**: kohta leiate teavet [loomine Hadoopi kogumite rakenduses Hdinsightiga kohandatud suvandite abil](hdinsight-provision-clusters.md) luua klaster virtuaalse võrgus.
    > * **SQL serveri 2014**: konfigureeritud lubama autentimis- ja töötab VPN kliendi konfiguratsiooni pakett turvaliselt ühendada virtuaalse võrgu.

7. SQL Azure'i andmebaasi taru tabeli eksportida.

8. Importida tabeli mobiledata HDInsight klaster.

    Uurige faili muutmiskuupäev, saate kasutada Azure portaali, mis Azure Storage Exploreri tööriista või Azure PowerShelli.  [Alustamine Hdinsightiga] [ hdinsight-get-started] on kood valimi kohta Azure PowerShelli abil faili alla laadida ja faili sisu kuvamine.


### <a name="the-powershell-sample"></a>PowerShelli näidis

    # Prepare an Azure SQL database to be used by the Sqoop tutorial
    
    #region - provide the following values
    
    $subscriptionID = "<Enter your Azure Subscription ID>"
    
    $sqlDatabaseLogin = "<Enter a SQL Database Login name>" #SQL Database server login
    $sqlDatabasePassword = "<Enter a Password>"
    
    $httpUserName = "admin"  #HDInsight cluster username
    $httpPassword = "<Enter a Password>"
    
    # used for creating Azure service names
    $nameToken = "<Enter an alias>" 
    $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")
    #endregion
    
    #region - variables
    
    # Resource group variables
    $resourceGroupName = $namePrefix + "rg"
    $location = "East US 2" # used by all Azure services defined in this tutorial
    
    # SQL database varialbes
    $sqlDatabaseServerName = $namePrefix + "sqldbserver"
    $sqlDatabaseName = $namePrefix + "sqldb"
    $sqlDatabaseConnectionString = "Data Source=$sqlDatabaseServerName.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabasePassword;Encrypt=true;Trusted_Connection=false;"
    $sqlDatabaseMaxSizeGB = 10
    
    # Used for retrieving external IP address and creating firewall rules
    $ipAddressRestService = "http://bot.whatismyipaddress.com"
    $fireWallRuleName = "UseSqoop"
    
    # Used for creating tables and clustered indexes
    $cmdCreateLog4jTable = "CREATE TABLE [dbo].[log4jlogs](
        [t1] [nvarchar](50),
        [t2] [nvarchar](50),
        [t3] [nvarchar](50),
        [t4] [nvarchar](50),
        [t5] [nvarchar](50),
        [t6] [nvarchar](50),
        [t7] [nvarchar](50))"
    
    $cmdCreateLog4jClusteredIndex = "CREATE CLUSTERED INDEX log4jlogs_clustered_index on log4jlogs(t1)"
    
    $cmdCreateMobileTable = " CREATE TABLE [dbo].[mobiledata](
    [clientid] [nvarchar](50),
    [querytime] [nvarchar](50),
    [market] [nvarchar](50),
    [deviceplatform] [nvarchar](50),
    [devicemake] [nvarchar](50),
    [devicemodel] [nvarchar](50),
    [state] [nvarchar](50),
    [country] [nvarchar](50),
    [querydwelltime] [float],
    [sessionid] [bigint],
    [sessionpagevieworder][bigint])"
    
    $cmdCreateMobileDataClusteredIndex = "CREATE CLUSTERED INDEX mobiledata_clustered_index on mobiledata(clientid)"
    
    # HDInsight variables
    $hdinsightClusterName = $namePrefix + "hdi"
    $defaultStorageAccountName = $namePrefix + "store"
    $defaultBlobContainerName = $hdinsightClusterName
    #endregion
    
    # Treat all errors as terminating
    $ErrorActionPreference = "Stop"
    
    #region - Connect to Azure subscription
    Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
    try{Get-AzureRmContext}
    catch{Login-AzureRmAccount}
    #endregion
    
    #region - Create Azure resouce group
    Write-Host "`nCreating an Azure resource group ..." -ForegroundColor Green
    try{
        Get-AzureRmResourceGroup -Name $resourceGroupName
    }
    catch{
        New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
    }
    #endregion
    
    #region - Create Azure SQL database server
    Write-Host "`nCreating an Azure SQL Database server ..." -ForegroundColor Green
    try{
        Get-AzureRmSqlServer -ServerName $sqlDatabaseServerName -ResourceGroupName $resourceGroupName}
    catch{
        Write-Host "`nCreating SQL Database server ..."  -ForegroundColor Green
    
        $sqlDatabasePW = ConvertTo-SecureString -String $sqlDatabasePassword -AsPlainText -Force
        $credential = New-Object System.Management.Automation.PSCredential($sqlDatabaseLogin,$sqlDatabasePW)
    
        $sqlDatabaseServerName = (New-AzureRmSqlServer `
                                    -ResourceGroupName $resourceGroupName `
                                    -ServerName $sqlDatabaseServerName `
                                    -SqlAdministratorCredentials $credential `
                                    -Location $location).ServerName
        Write-Host "`tThe new SQL database server name is $sqlDatabaseServerName." -ForegroundColor Cyan
    
        Write-Host "`nCreating firewall rule, $fireWallRuleName ..." -ForegroundColor Green
        $workstationIPAddress = Invoke-RestMethod $ipAddressRestService
        New-AzureRmSqlServerFirewallRule `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -FirewallRuleName "$fireWallRuleName-workstation" `
            -StartIpAddress $workstationIPAddress `
            -EndIpAddress $workstationIPAddress
    
        #To allow other Azure services to access the server add a firewall rule and set both the StartIpAddress and EndIpAddress to 0.0.0.0. 
        #Note that this allows Azure traffic from any Azure subscription to access the server.
        New-AzureRmSqlServerFirewallRule `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -FirewallRuleName "$fireWallRuleName-Azureservices" `
            -StartIpAddress "0.0.0.0" `
            -EndIpAddress "0.0.0.0"
    }
    
    #endregion
    
    #region - Create and validate Azure SQL database
    Write-Host "`nCreating an Azure SQL database ..." -ForegroundColor Green
    
    try {
        Get-AzureRmSqlDatabase `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -DatabaseName $sqlDatabaseName
    }
    catch {
        Write-Host "`nCreating SQL Database, $sqlDatabaseName ..."  -ForegroundColor Green
        New-AzureRMSqlDatabase `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -DatabaseName $sqlDatabaseName `
            -Edition "Standard" `
            -RequestedServiceObjectiveName "S1"
    }
    
    #endregion
    
    #region - Create tables
    Write-Host "Creating the log4jlogs table and the mobiledata table ..." -ForegroundColor Green
    
    $conn = New-Object System.Data.SqlClient.SqlConnection
    $conn.ConnectionString = $sqlDatabaseConnectionString
    $conn.Open()
    
    # Create the log4jlogs table and index
    $cmd = New-Object System.Data.SqlClient.SqlCommand
    $cmd.Connection = $conn
    $cmd.CommandText = $cmdCreateLog4jTable
    $ret = $cmd.ExecuteNonQuery()
    $cmd.CommandText = $cmdCreateLog4jClusteredIndex
    $cmd.ExecuteNonQuery()
    
    # Create the mobiledata table and index
    $cmd.CommandText = $cmdCreateMobileTable
    $cmd.ExecuteNonQuery()
    $cmd.CommandText = $cmdCreateMobileDataClusteredIndex
    $cmd.ExecuteNonQuery()
    
    $conn.close()
    
    #endregion
    
    
    #region - Create HDInsight cluster
    
    Write-Host "Creating the HDInsight cluster and the dependent services ..." -ForegroundColor Green
    
    # Create the default storage account
    New-AzureRmStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $defaultStorageAccountName `
        -Location $location `
        -Type Standard_LRS
    
    # Create the default Blob container
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey `
                                    -ResourceGroupName $resourceGroupName `
                                    -Name $defaultStorageAccountName)[0].Value
    $defaultStorageAccountContext = New-AzureStorageContext `
                                        -StorageAccountName $defaultStorageAccountName `
                                        -StorageAccountKey $defaultStorageAccountKey 
    New-AzureStorageContainer `
        -Name $defaultBlobContainerName `
        -Context $defaultStorageAccountContext 
    
    # Create the HDInsight cluster
    $pw = ConvertTo-SecureString -String $httpPassword -AsPlainText -Force
    $httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$pw)
    
    New-AzureRmHDInsightCluster `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $HDInsightClusterName `
        -Location $location `
        -ClusterType Hadoop `
        -OSType Windows `
        -ClusterSizeInNodes 2 `
        -HttpCredential $httpCredential `
        -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultStorageContainer $defaultBlobContainerName 
    
    # Validate the cluster
    Get-AzureRmHDInsightCluster -ClusterName $hdinsightClusterName
    #endregion
    
    #region - pre-process the source file
    
    Write-Host "Preprocessing the source file ..." -ForegroundColor Green
    
    # This procedure creates a new file with $destBlobName
    $sourceBlobName = "example/data/sample.log"
    $destBlobName = "tutorials/usesqoop/data/sample.log"
    
    # Define the connection string
    $storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=$defaultStorageAccountName;AccountKey=$defaultStorageAccountKey"
    
    # Create block blob objects referencing the source and destination blob.
    $storageAccount = [Microsoft.WindowsAzure.Storage.CloudStorageAccount]::Parse($storageConnectionString)
    $storageClient = $storageAccount.CreateCloudBlobClient();
    $storageContainer = $storageClient.GetContainerReference($defaultBlobContainerName)
    $sourceBlob = $storageContainer.GetBlockBlobReference($sourceBlobName)
    $destBlob = $storageContainer.GetBlockBlobReference($destBlobName)
    
    # Define a MemoryStream and a StreamReader for reading from the source file
    $stream = New-Object System.IO.MemoryStream
    $stream = $sourceBlob.OpenRead()
    $sReader = New-Object System.IO.StreamReader($stream)
    
    # Define a MemoryStream and a StreamWriter for writing into the destination file
    $memStream = New-Object System.IO.MemoryStream
    $writeStream = New-Object System.IO.StreamWriter $memStream
    
    # Pre-process the source blob
    $exString = "java.lang.Exception:"
    while(-Not $sReader.EndOfStream){
        $line = $sReader.ReadLine()
        $split = $line.Split(" ")
    
        # remove the "java.lang.Exception" from the first element of the array
        # for example: java.lang.Exception: 2012-02-03 19:11:02 SampleClass8 [WARN] problem finding id 153454612
        if ($split[0] -eq $exString){
            #create a new ArrayList to remove $split[0]
            $newArray = [System.Collections.ArrayList] $split
            $newArray.Remove($exString)
    
            # update $split and $line
            $split = $newArray
            $line = $newArray -join(" ")
        }
    
        # remove the lines that has less than 7 elements
        if ($split.count -ge 7){
            write-host $line
            $writeStream.WriteLine($line)
        }
    }
    
    # Write to the destination blob
    $writeStream.Flush()
    $memStream.Seek(0, "Begin")
    $destBlob.UploadFromStream($memStream)
    
    #endregion
    
    #region - export a log file from the cluster to the SQL database
    
    Write-Host "Preprocessing the source file ..." -ForegroundColor Green
    
    $tableName_log4j = "log4jlogs"
    
    # Connection string for Azure SQL Database.
    # Comment if using SQL Server
    $connectionString = "jdbc:sqlserver://$sqlDatabaseServerName.database.windows.net;user=$sqlDatabaseLogin@$sqlDatabaseServerName;password=$sqlDatabasePassword;database=$sqlDatabaseName"
    # Connection string for SQL Server.
    # Uncomment if using SQL Server.
    #$connectionString = "jdbc:sqlserver://$sqlDatabaseServerName;user=$sqlDatabaseLogin;password=$sqlDatabasePassword;database=$sqlDatabaseName"
    
    $exportDir_log4j = "/tutorials/usesqoop/data"
    
    # Submit a Sqoop job
    $sqoopDef = New-AzureRmHDInsightSqoopJobDefinition `
        -Command "export --connect $connectionString --table $tableName_log4j --export-dir $exportDir_log4j --input-fields-terminated-by \0x20 -m 1"
    $sqoopJob = Start-AzureRmHDInsightJob `
                    -ClusterName $hdinsightClusterName `
                    -HttpCredential $httpCredential `
                    -JobDefinition $sqoopDef #-Debug -Verbose
    Wait-AzureRmHDInsightJob `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId
    
    Write-Host "Standard Error" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -DefaultContainer $defaultBlobContainerName -HttpCredential $httpCredential -JobId $sqoopJob.JobId -DisplayOutputType StandardError
    Write-Host "Standard Output" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -DefaultContainer $defaultBlobContainerName -HttpCredential $httpCredential -JobId $sqoopJob.JobId -DisplayOutputType StandardOutput
    
    #endregion
    
    #region - export a Hive table
    
    $tableName_mobile = "mobiledata"
    $exportDir_mobile = "/hive/warehouse/hivesampletable"
    
    $sqoopDef = New-AzureRmHDInsightSqoopJobDefinition `
        -Command "export --connect $connectionString --table $tableName_mobile --export-dir $exportDir_mobile --fields-terminated-by \t -m 1"
    $sqoopJob = Start-AzureRmHDInsightJob `
                    -ClusterName $hdinsightClusterName `
                    -HttpCredential $httpCredential `
                    -JobDefinition $sqoopDef #-Debug -Verbose
    
    Wait-AzureRmHDInsightJob `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId
    
    Write-Host "Standard Error" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -DefaultStorageAccountName $defaultStorageAccountName `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultContainer $defaultBlobContainerName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId `
        -DisplayOutputType StandardError
    
    Write-Host "Standard Output" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -DefaultStorageAccountName $defaultStorageAccountName `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultContainer $defaultBlobContainerName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId `
        -DisplayOutputType StandardOutput
    
    #endregion
    
    #region - import a database
    
    $targetDir_mobile = "/tutorials/usesqoop/importeddata/"
    
    $sqoopDef = New-AzureRmHDInsightSqoopJobDefinition `
        -Command "import --connect $connectionString --table $tableName_mobile --target-dir $targetDir_mobile --fields-terminated-by \t --lines-terminated-by \n -m 1"
    
    $sqoopJob = Start-AzureRmHDInsightJob `
                    -ClusterName $hdinsightClusterName `
                    -HttpCredential $httpCredential `
                    -JobDefinition $sqoopDef #-Debug -Verbose
    
    Wait-AzureRmHDInsightJob `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId
    
    Write-Host "Standard Error" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -DefaultStorageAccountName $defaultStorageAccountName `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultContainer $defaultBlobContainerName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId `
        -DisplayOutputType StandardError
    
    Write-Host "Standard Output" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -DefaultStorageAccountName $defaultStorageAccountName `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultContainer $defaultBlobContainerName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId `
        -DisplayOutputType StandardOutput
    
    #endregion



[azure-management-portal]: https://portal.azure.com/

[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md

[sqldatabase-get-started]: ../sql-database/sql-database-get-started.md
[sqldatabase-create-configue]: ../sql-database/sql-database-get-started.md

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-install]: powershell-install-configure.md
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[sqoop-user-guide-1.4.4]: https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html
