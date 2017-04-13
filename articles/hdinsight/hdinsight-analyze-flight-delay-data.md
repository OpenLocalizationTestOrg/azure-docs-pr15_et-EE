<properties
    pageTitle="Viivitus lennuandmetega Hadoopi rakenduses Hdinsightiga koos analüüsida | Microsoft Azure'i"
    description="Saate teada, kuidas luua mõne Hdinsightiga kobar ühe Windows PowerShelli skripti abil saate käivitada taru töö, Sqoop töö ja klaster kustutamine."
    services="hdinsight"
    documentationCenter=""
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="jgao"/>

#<a name="analyze-flight-delay-data-by-using-hive-in-hdinsight"></a>Abil rakenduses HDInsight taru lennureisi viivitus andmete analüüsiks

Taru pakub võimalust läbiv Hadoopi MapReduce töö SQL-like skriptimiskeele nimega * [HiveQL][hadoop-hiveql]*, mida saab rakendada kokkuvõtmine, päringu ja analüüsimine suuri andmemahtusid suunas.

> [AZURE.NOTE] Juhised selle dokumendi jaoks on vaja Windowsi-põhiste Hdinsightiga kobar. Kus Linux-põhine kobar juhised leiate teemast [viivitus lennuandmetega analüüsi taru sisse Hdinsightiga (Linux) abil](hdinsight-analyze-flight-delay-data-linux.md).

Üks peamisi eeliseid Windows Azure Hdinsightiga on andmete talletamine ja Arvuta. HDInsight kasutab Azure'i bloobimälu andmesalv. Tüüpilised töö hõlmab kolm osa:

1. **Azure'i bloobimälu andmete talletamiseks.**  Näiteks ilm andmed, andurite andmeid, web logid ja sel juhul viivitus lennuandmetega salvestatakse Azure'i bloobimälu.
2. **Käivitage tööde haldamine.** Kui see on andmete töötlemise aeg, luua mõne Hdinsightiga kobar Windows PowerShelli skripti (või kliendi rakendus) käivitate käivitada tööd ja kustutage klaster. Tööd salvestada Azure'i bloobimälu. Väljundi andmed säilivad isegi juhul, kui klaster kustutatakse. Sellisel viisil maksate ainult, mida teil on tarbitud.
3. **Azure'i bloobimälu väljund tuua**, või selles õpetuses eksportida andmed Azure SQL-andmebaasi.

Järgmine diagramm näitab seda stsenaariumi ja selle õpetuse ülesehituse:

![HDI. FlightDelays.flow][img-hdi-flightdelays-flow]

**Märkus**: diagrammi arvud vastavad jaotise pealkirjad. **M** tähistab puhul. **A** tähistab liites sisu.

See peamine õpetuse osa näitab, kuidas ühe Windows PowerShelli skripti abil saate teha järgmisi toiminguid:

- Saate luua ka Hdinsightiga kobar.
- Töö taru klaster keskmiste lendude Edasilükkamiste lennujaamade arvutamiseks. Azure'i bloobimälu salvestusruumi konto talletatakse lennuandmetega viivitust.
- Käivitage Sqoop töö eksportimiseks taru töö väljundi Azure SQL-andmebaasi.
- Kustutage Hdinsightiga kobar.

Lisad, leiate juhised lennuandmetega hilinemise üleslaadimise, luua ja üles taru päringustringi ja SQL Azure'i andmebaasi Sqoop tööks ettevalmistamine.

> [AZURE.NOTE] Windowsi-põhiste Hdinsightiga kogumite on selles dokumendis juhiseid. Kus Linux-põhine kobar juhised leiate teemast [analüüsi viivitus lennuandmetega taru sisse Hdinsightiga (Linux) abil](hdinsight-analyze-flight-delay-data-linux.md)

###<a name="prerequisites"></a>Eeltingimused

Enne alustamist selles õpetuses, peab teil olema järgmised üksused:

- **An Azure'i tellimus**. Leiate [Azure'i saada tasuta prooviversioon](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- **Töökoha Azure PowerShelli abil**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

**Selles õppetükis kasutatud failid**

Selle õpetuse kasutab täpsel jõudlus lennuliini lennuandmetega [uurimistöö ja uuendusliku tehnoloogiaga haldus, Bureau transport statistika]või RITA [rita-website]. Kopeerige andmed on üles laaditud Azure'i bloobimälu salvestusruumi nõus avaliku bloobimälu juurdepääsu õigus. Osa oma PowerShelli skripti kopeerib andmed avaliku bloobimälu container vaikimisi bloobimälu ümbris klaster. Sama bloobimälu container kopeeritakse ka HiveQL skript. Kui soovite siit saate teada, kuidas saada Laadi üles oma salvestusruumi konto andmed ja kuidas HiveQL skriptifail loomine või üleslaadimine, lugege teemat [lisa A](#appendix-a) ja [B](#appendix-b).

Järgmises tabelis on loetletud selles õpetuses kasutatud failid:

<table border="1">
<tr><th>Failide</th><th>Kirjeldus</th></tr>
<tr><td>wasbs://flightdelay@hditutorialdata.blob.core.windows.net/flightdelays.hql</td><td>HiveQL skriptifail kasutatavaid taru töö. See skript on üles laaditud Azure'i bloobimälu salvestusruumi konto avaliku juurdepääsuga. <a href="#appendix-b">Lisa B</a> on ettevalmistamine ja selle faili üleslaadimine oma Azure'i bloobimälu salvestusruumi konto juhised.</td></tr>
<tr><td>wasbs://flightdelay@hditutorialdata.blob.core.windows.net/2013Data</td><td>Sisendandmete taru töö. Andmed on laaditud Azure'i bloobimälu salvestusruumi konto avaliku juurdepääsuga. <a href="#appendix-a">Lisa A</a> on andmete toomine ja andmete üleslaadimine oma Azure'i bloobimälu salvestusruumi konto juhised.</td></tr>
<tr><td>\tutorials\flightdelays\output</td><td>Väljundi tee taru töö. Vaike-ümbrisest kasutatakse väljundi andmete talletamiseks.</td></tr>
<tr><td>\tutorials\flightdelays\jobstatus</td><td>Taru töö oleku kausta vaike-ümbrisest.</td></tr>
</table>


##<a name="create-cluster-and-run-hivesqoop-jobs"></a>Kobar loomine ja käivitamine taru/Sqoop tööde haldamine

Hadoopi MapReduce on Pakktöötlus. Optimaalsete taru töö käivitamiseks on klaster töö loomine ja kustutamine töö pärast töö on lõpule viidud. Järgmise skripti hõlmab kogu protsessi. Mõne Hdinsightiga kobar loomise ja taru tööde kohta leiate lisateavet teemast [loomine Hadoopi kogumite sisse Hdinsightiga] [ hdinsight-provision] ja [Kasutage taru koos Hdinsightiga][hdinsight-use-hive].

**Taru päringute käivitamiseks Azure PowerShelli abil**

1. Azure'i SQL-andmebaasiga ja Sqoop töö väljund tabeli loomine c [Liites](#appendix-c)juhiste järgi.
3. Avage Windows PowerShell ISE ja käivitage järgmine skript.

        $subscriptionID = "<Azure Subscription ID>"
        $nameToken = "<Enter an Alias>" 
        
        ###########################################
        # You must configure the follwing variables
        # for an existing Azure SQL Database
        ###########################################
        $existingSqlDatabaseServerName = "<Azure SQL Database Server>"
        $existingSqlDatabaseLogin = "<Azure SQL Database Server Login>"
        $existingSqlDatabasePassword = "<Azure SQL Database Server login password>"
        $existingSqlDatabaseName = "<Azure SQL Database name>"
        
        $localFolder = "E:\Tutorials\Downloads\" # A temp location for copying files. 
        $azcopyPath = "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy" # depends on the version, the folder can be different
        
        ###########################################
        # (Optional) configure the following variables
        ###########################################
        
        $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")

        $resourceGroupName = $namePrefix + "rg"
        $location = "EAST US 2"
        
        $HDInsightClusterName = $namePrefix + "hdi"
        $httpUserName = "admin"
        $httpPassword = "<Enter the Password>"
        
        $defaultStorageAccountName = $namePrefix + "store"
        $defaultBlobContainerName = $HDInsightClusterName # use the cluster name
        
        $existingSqlDatabaseTableName = "AvgDelays"
        $sqlDatabaseConnectionString = "jdbc:sqlserver://$existingSqlDatabaseServerName.database.windows.net;user=$existingSqlDatabaseLogin@$existingSqlDatabaseServerName;password=$existingSqlDatabaseLogin;database=$existingSqlDatabaseName"
        
        $hqlScriptFile = "/tutorials/flightdelays/flightdelays.hql" 
        
        $jobStatusFolder = "/tutorials/flightdelays/jobstatus"
        
        ###########################################
        # Login
        ###########################################
        try{
            $acct = Get-AzureRmSubscription
        }
        catch{
            Login-AzureRmAccount
        }
        Select-AzureRmSubscription -SubscriptionID $subscriptionID
        
        ###########################################
        # Create a new HDInsight cluster
        ###########################################
        
        # Create ARM group
        New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
        
        # Create the default storage account
        New-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName -Location $location -Type Standard_LRS
        
        # Create the default Blob container
        $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
        $defaultStorageAccountContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey 
        New-AzureStorageContainer -Name $defaultBlobContainerName -Context $defaultStorageAccountContext 
        
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
            -DefaultStorageContainer $existingDefaultBlobContainerName 
        
        
        ###########################################
        # Prepare the HiveQL script and source data
        ###########################################
        
        # Create the temp location  
        New-Item -Path $localFolder -ItemType Directory -Force 
        
        # Download the sample file from Azure Blob storage
        $context = New-AzureStorageContext -StorageAccountName "hditutorialdata" -Anonymous
        $blobs = Get-AzureStorageBlob -Container "flightdelay" -Context $context
        #$blobs | Get-AzureStorageBlobContent -Context $context -Destination $localFolder
        
        # Upload data to default container
        
        $azcopycmd = "cmd.exe /C '$azcopyPath\azcopy.exe' /S /Source:'$localFolder' /Dest:'https://$defaultStorageAccountName.blob.core.windows.net/$defaultBlobContainerName/tutorials/flightdelays' /DestKey:$defaultStorageAccountKey"
        
        Invoke-Expression -Command:$azcopycmd
        
        ###########################################
        # Submit the Hive job
        ###########################################
        Use-AzureRmHDInsightCluster -ClusterName $HDInsightClusterName -HttpCredential $httpCredential
        $response = Invoke-AzureRmHDInsightHiveJob `
                        -Files $hqlScriptFile `
                        -DefaultContainer $defaultBlobContainerName `
                        -DefaultStorageAccountName $defaultStorageAccountName `
                        -DefaultStorageAccountKey $defaultStorageAccountKey `
                        -StatusFolder $jobStatusFolder 
        
        write-Host $response
        
        
        ###########################################
        # Submit the Sqoop job
        ###########################################
        $exportDir = "wasbs://$defaultBlobContainerName@$defaultStorageAccountName.blob.core.windows.net/tutorials/flightdelays/output"
        
        $sqoopDef = New-AzureRmHDInsightSqoopJobDefinition `
                        -Command "export --connect $sqlDatabaseConnectionString --table $sqlDatabaseTableName --export-dir $exportDir --fields-terminated-by \001 "
        $sqoopJob = Start-AzureRmHDInsightJob `
                        -ResourceGroupName $resourceGroupName `
                        -ClusterName $hdinsightClusterName `
                        -HttpCredential $httpCredential `
                        -JobDefinition $sqoopDef #-Debug -Verbose
        
        Wait-AzureRmHDInsightJob `
            -ResourceGroupName $resourceGroupName `
            -ClusterName $HDInsightClusterName `
            -HttpCredential $httpCredential `
            -WaitTimeoutInSeconds 3600 `
            -Job $sqoopJob.JobId
        
        
        Get-AzureRmHDInsightJobOutput `
            -ResourceGroupName $resourceGroupName `
            -ClusterName $hdinsightClusterName `
            -HttpCredential $httpCredential `
            -DefaultContainer $existingDefaultBlobContainerName `
            -DefaultStorageAccountName $defaultStorageAccountName `
            -DefaultStorageAccountKey $defaultStorageAccountKey `
            -JobId $sqoopJob.JobId `
            -DisplayOutputType StandardError
        
        ###########################################
        # Delete the cluster
        ###########################################
        Remove-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName

6. Ühenduse loomine SQL-andmebaasi ja vaadake Keskmine lendude hilinemist AvgDelays tabeli linnade järgi:

    ![HDI. FlightDelays.AvgDelays.Dataset][image-hdi-flightdelays-avgdelays-dataset]



---
##<a id="appendix-a"></a>Lisa A – laadi viivitus lennuandmetega Azure'i bloobimälu
Üleslaadimise andmefaili ja HiveQL skripti faile (vt [B lisa](#appendix-b)) jaoks on vaja mõned kavandamine. Mõte on andmefailid ja enne mõne Hdinsightiga kobar loomise ja töö taru HiveQL faili salvestamiseks. Teil on kaks võimalust.

- **Kasutage sama Azure Storage kontoga, mida kasutatakse Hdinsightiga kobar failisüsteemi vaikimisi.** Kuna Hdinsightiga kobar on kiirklahv salvestusruumi konto, peate ei täiendavad muudatused.
- **Kasutage Azure Storage teise kontoga Hdinsightiga kobar failisüsteemi kaudu.** Kui see on nii, peate muutma Windows PowerShelli skripti leitud [kobar Hdinsightiga loomine ja käitamine taru/Sqoop töökohtade](#runjob) link nimega täiendava salvestusruumi konto salvestusruumi konto loomine osa. Juhised leiate teemast [loomine Hadoopi kogumite sisse Hdinsightiga][hdinsight-provision]. Hdinsightiga kobar siis teab kiirklahv salvestusruumi konto.

>[AZURE.NOTE] Andmefaili jaoks soovitud tee bloobimälu salvestusruumi on raske kodeeritud skriptifail HiveQL. Peate värskendama vastavalt sellele.

**Allalaadimiseks andmed lendude**

1. Liikuge sirvides [uurimistöö ja uuendusliku tehnoloogiaga Administreerimine Bureau transport statistika][rita-website].
2. Klõpsake lehel Valige järgmised väärtused:

    <table border="1">
    <tr><th>Nimi</th><th>Väärtus</th></tr>
    <tr><td>Filter aasta</td><td>2013 </td></tr>
    <tr><td>Filtreerimine perioodi kohta.</td><td>Jaanuar</td></tr>
    <tr><td>Väljad</td><td>*Aasta*, *FlightDate*, *UniqueCarrier*, *Carrier*, *FlightNum*, *OriginAirportID*, *Origin*, *OriginCityName*, *OriginState*, *DestAirportID*, *Dest*, *DestCityName*, *DestState*, *DepDelayMinutes*, *ArrDelay*, *ArrDelayMinutes*, *CarrierDelay*, *WeatherDelay*, *NASDelay*, *SecurityDelay*, *LateAircraftDelay* (Tühjenda kõik muud väljad)</td></tr>
    </table>

3. Klõpsake nuppu **Laadi alla**.
4. Pakkige lahti **C:\Tutorials\FlightDelay\2013Data** kausta fail. Iga fail on CSV-faili ja ligikaudu 60GB suuruseid.
5.  Nimetage fail ümber nimi, kuu, mis sisaldab andmeid. Näiteks oleks jaanuar andmeid sisaldava faili nimega *January.csv*.
6. Korrake juhiseid 2 – 5 iga 12 kuu 2013 faili allalaadimiseks. Peate vähemalt ühte faili õpetuse käivitamiseks.  

**Viivitus lennuandmetega üleslaadimiseks Azure'i bloobimälu**

1. Ettevalmistused parameetrid:

    <table border="1">
    <tr><th>Muutuja nimi</th><th>Märkmete</th></tr>
    <tr><td>$storageAccountName</td><td>Azure Storage konto, kuhu soovite andmed üles laadida.</td></tr>
    <tr><td>$blobContainerName</td><td>Bloobimälu ümbris, kuhu soovite andmed üles laadida.</td></tr>
    </table>
2. Avage Azure'i PowerShelli ISE.
3. Järgmised skripti kleepimine skripti paani:

        [CmdletBinding()]
        Param(

            [Parameter(Mandatory=$True,
                       HelpMessage="Enter the Azure storage account name for creating a new HDInsight cluster. If the account doesn't exist, the script will create one.")]
            [String]$storageAccountName,

            [Parameter(Mandatory=$True,
                       HelpMessage="Enter the Azure blob container name for creating a new HDInsight cluster. If not specified, the HDInsight cluster name will be used.")]
            [String]$blobContainerName
        )

        #Region - Variables
        $localFolder = "C:\Tutorials\FlightDelay\2013Data"  # The source folder
        $destFolder = "tutorials/flightdelay/2013data"     #The blob name prefix for the files to be uploaded
        #EndRegion
        
        #Region - Connect to Azure subscription
        Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
        try{Get-AzureRmContext}
        catch{Login-AzureRmAccount}
        #EndRegion
        
        #Region - Validate user input
        Write-Host "`nValidating the Azure Storage account and the Blob container..." -ForegroundColor Green
        # Validate the Storage account
        if (-not (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}))
        {
            Write-Host "The storage account, $storageAccountName, doesn't exist." -ForegroundColor Red
            exit
        }
        else{
            $resourceGroupName = (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}).ResourceGroupName
        }
        
        # Validate the container
        $storageAccountKey = (Get-AzureRmStorageAccountKey -StorageAccountName $storageAccountName -ResourceGroupName $resourceGroupName)[0].Value
        $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        
        if (-not (Get-AzureStorageContainer -Context $storageContext |Where-Object{$_.Name -eq $blobContainerName}))
        {
            Write-Host "The Blob container, $blobContainerName, doesn't exist" -ForegroundColor Red
            Exit
        }
        #EngRegion
        
        #Region - Copy the file from local workstation to Azure Blob storage  
        if (test-path -Path $localFolder)
        {
            foreach ($item in Get-ChildItem -Path $localFolder){
                $fileName = "$localFolder\$item"
                $blobName = "$destFolder/$item"
        
                Write-Host "Copying $fileName to $blobName" -ForegroundColor Green
        
                Set-AzureStorageBlobContent -File $fileName -Container $blobContainerName -Blob $blobName -Context $storageContext
            }
        }
        else
        {
            Write-Host "The source folder on the workstation doesn't exist" -ForegroundColor Red
        }
        
        # List the uploaded files on HDInsight
        Get-AzureStorageBlob -Container $blobContainerName  -Context $storageContext -Prefix $destFolder
        #EndRegion




4. Vajutage klahvi **F5** skripti käivitamiseks.

Kui valite failide üleslaadimise muu meetodi kasutamiseks, veenduge, et faili tee on õpetused/flightdelay/andmeid. Failidele juurdepääs süntaks on järgmine:

    wasbs://<ContainerName>@<StorageAccountName>.blob.core.windows.net/tutorials/flightdelay/data

Tee õpetused/flightdelay/andmed on virtuaalse kausta, mis on loodud, kui teil on üles laaditud faile. Veenduge, et 12 faile, üks iga kuu.

>[AZURE.NOTE] Peate värskendama taru päringu lugemise uude asukohta.

> Kas tuleb konfigureerida avalik või siduda salvestusruumi konto Hdinsightiga klaster container juurdepääsuõigus. Muul juhul ei saa taru päringustringi andmete failidele juurde pääseda.

---
##<a id="appendix-b"></a>Lisa B – luua ja üles laadida HiveQL skripti

Azure'i PowerShelli abil saate korraga töötada mitme HiveQL laused ühe või paketti HiveQL lause skripti faili. Selles jaotises kirjeldatakse, kuidas luua skripti HiveQL ja üles laadida skripti Azure'i bloobimälu Azure PowerShelli abil. Taru nõuab HiveQL skriptide Azure'i bloobimälu hoida.

HiveQL script on tehke järgmist.

1. **Tabeli delays_raw**, juhul tabeli juba olemas.
2. **Delays_raw välise taru tabeli loomine** osutab bloobimälu talletuskoht flight viivitus failidega. Selle päringu saate määrata, et väljad on piiratud "," ja et jooned lõpetatakse "\n". See probleemiks kui väljaväärtuste sisaldavad komad, kuna taru ei erista koma, mis on välja eraldaja ja ühe, mis on osa välja väärtust (mis on välja väärtuste ORIGIN\_linna\_nimi ja DEST\_linna\_nimi). Sellega tegelemiseks päringu loob TEMP veergu andmeid, mis on valesti jagatud veergude mahutamiseks.  
3. **Tabeli viivitused**, juhul tabeli juba olemas.
4. **Looge tabel viivitused**. On kasulik andmete puhastamiseks enne edasiseks töötlemiseks. Selle päringu loob uue tabeli, *viivitused*, delays_raw tabelist. Pange tähele, et TEMP veerud (nagu eelnevalt mainitud) ei kopeerita ja et **alamstringi** funktsiooni kasutatakse andmete eemaldamiseks ülakomade vahel.
5. **Arvutage Keskmine ilm viivitus ja rühmade tulemused linna nime järgi.** See on ka väljund bloobimälu tulemused. Pange tähele, et päring ülakomad eemaldamine andmed ei sisalda read, kus **weather_delay** väärtus on null. See on vajalik, kuna Sqoop, kasutatakse hiljem selles õpetuses töötleb neid väärtusi nõtkelt vaikimisi.

Käskude HiveQL täieliku loendi leiate teemast [Taru Andmekirjelduskeel][hadoop-hiveql]. Iga HiveQL käsu lõpetama semikooloniga.

**HiveQL skripti faili loomiseks**

1. Ettevalmistused parameetrid:

    <table border="1">
    <tr><th>Muutuja nimi</th><th>Märkmete</th></tr>
    <tr><td>$storageAccountName</td><td>Kui soovite üles laadida HiveQL skripti Azure Storage konto.</td></tr>
    <tr><td>$blobContainerName</td><td>Kui soovite üles laadida HiveQL skripti bloobimälu ümbris.</td></tr>
    </table>
2. Avage Azure'i PowerShelli ISE.

3. Kopeerige ja kleepige järgmine skript paani skripti:

        [CmdletBinding()]
        Param(

            # Azure Blob storage variables
            [Parameter(Mandatory=$True,
                       HelpMessage="Enter the Azure storage account name for creating a new HDInsight cluster. If the account doesn't exist, the script will create one.")]
            [String]$storageAccountName,

            [Parameter(Mandatory=$True,
                       HelpMessage="Enter the Azure blob container name for creating a new HDInsight cluster. If not specified, the HDInsight cluster name will be used.")]
            [String]$blobContainerName
        )

        #region - Define variables
        # Treat all errors as terminating
        $ErrorActionPreference = "Stop"
        
        # The HiveQL script file is exported as this file before it's uploaded to Blob storage
        $hqlLocalFileName = "e:\tutorials\flightdelay\flightdelays.hql"
        
        # The HiveQL script file will be uploaded to Blob storage as this blob name
        $hqlBlobName = "tutorials/flightdelay/flightdelays.hql"
        
        # These two constants are used by the HiveQL script file
        #$srcDataFolder = "tutorials/flightdelay/data"
        $dstDataFolder = "/tutorials/flightdelay/output"
        #endregion

        #Region - Connect to Azure subscription
        Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
        try{Get-AzureRmContext}
        catch{Login-AzureRmAccount}
        #EndRegion
        
        #Region - Validate user input
        Write-Host "`nValidating the Azure Storage account and the Blob container..." -ForegroundColor Green
        # Validate the Storage account
        if (-not (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}))
        {
            Write-Host "The storage account, $storageAccountName, doesn't exist." -ForegroundColor Red
            exit
        }
        else{
            $resourceGroupName = (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}).ResourceGroupName
        }
        
        # Validate the container
        $storageAccountKey = (Get-AzureRmStorageAccountKey -StorageAccountName $storageAccountName -ResourceGroupName $resourceGroupName)[0].Value
        $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        
        if (-not (Get-AzureStorageContainer -Context $storageContext |Where-Object{$_.Name -eq $blobContainerName}))
        {
            Write-Host "The Blob container, $blobContainerName, doesn't exist" -ForegroundColor Red
            Exit
        }
        #EngRegion
        
        #region - Validate the file and file path
        
        # Check if a file with the same file name already exists on the workstation
        Write-Host "`nvalidating the folder structure on the workstation for saving the HQL script file ..."  -ForegroundColor Green
        if (test-path $hqlLocalFileName){
        
            $isDelete = Read-Host 'The file, ' $hqlLocalFileName ', exists.  Do you want to overwirte it? (Y/N)'
        
            if ($isDelete.ToLower() -ne "y")
            {
                Exit
            }
        }
        
        # Create the folder if it doesn't exist
        $folder = split-path $hqlLocalFileName
        if (-not (test-path $folder))
        {
            Write-Host "`nCreating folder, $folder ..." -ForegroundColor Green
        
            new-item $folder -ItemType directory  
        }
        #end region
        
        #region - Write the Hive script into a local file
        Write-Host "`nWriting the Hive script into a file on your workstation ..." `
                    -ForegroundColor Green
        
        $hqlDropDelaysRaw = "DROP TABLE delays_raw;"
        
        $hqlCreateDelaysRaw = "CREATE EXTERNAL TABLE delays_raw (" +
                "YEAR string, " +
                "FL_DATE string, " +
                "UNIQUE_CARRIER string, " +
                "CARRIER string, " +
                "FL_NUM string, " +
                "ORIGIN_AIRPORT_ID string, " +
                "ORIGIN string, " +
                "ORIGIN_CITY_NAME string, " +
                "ORIGIN_CITY_NAME_TEMP string, " +
                "ORIGIN_STATE_ABR string, " +
                "DEST_AIRPORT_ID string, " +
                "DEST string, " +
                "DEST_CITY_NAME string, " +
                "DEST_CITY_NAME_TEMP string, " +
                "DEST_STATE_ABR string, " +
                "DEP_DELAY_NEW float, " +
                "ARR_DELAY_NEW float, " +
                "CARRIER_DELAY float, " +
                "WEATHER_DELAY float, " +
                "NAS_DELAY float, " +
                "SECURITY_DELAY float, " +
                "LATE_AIRCRAFT_DELAY float) " +
            "ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' " +
            "LINES TERMINATED BY '\n' " +
            "STORED AS TEXTFILE " +
            "LOCATION 'wasbs://flightdelay@hditutorialdata.blob.core.windows.net/2013Data';"
        
        $hqlDropDelays = "DROP TABLE delays;"
        
        $hqlCreateDelays = "CREATE TABLE delays AS " +
            "SELECT YEAR AS year, " +
                "FL_DATE AS flight_date, " +
                "substring(UNIQUE_CARRIER, 2, length(UNIQUE_CARRIER) -1) AS unique_carrier, " +
                "substring(CARRIER, 2, length(CARRIER) -1) AS carrier, " +
                "substring(FL_NUM, 2, length(FL_NUM) -1) AS flight_num, " +
                "ORIGIN_AIRPORT_ID AS origin_airport_id, " +
                "substring(ORIGIN, 2, length(ORIGIN) -1) AS origin_airport_code, " +
                "substring(ORIGIN_CITY_NAME, 2) AS origin_city_name, " +
                "substring(ORIGIN_STATE_ABR, 2, length(ORIGIN_STATE_ABR) -1)  AS origin_state_abr, " +
                "DEST_AIRPORT_ID AS dest_airport_id, " +
                "substring(DEST, 2, length(DEST) -1) AS dest_airport_code, " +
                "substring(DEST_CITY_NAME,2) AS dest_city_name, " +
                "substring(DEST_STATE_ABR, 2, length(DEST_STATE_ABR) -1) AS dest_state_abr, " +
                "DEP_DELAY_NEW AS dep_delay_new, " +
                "ARR_DELAY_NEW AS arr_delay_new, " +
                "CARRIER_DELAY AS carrier_delay, " +
                "WEATHER_DELAY AS weather_delay, " +
                "NAS_DELAY AS nas_delay, " +
                "SECURITY_DELAY AS security_delay, " +
                "LATE_AIRCRAFT_DELAY AS late_aircraft_delay " +
            "FROM delays_raw;"
        
        $hqlInsertLocal = "INSERT OVERWRITE DIRECTORY '$dstDataFolder' " +
            "SELECT regexp_replace(origin_city_name, '''', ''), " +
                "avg(weather_delay) " +
            "FROM delays " +
            "WHERE weather_delay IS NOT NULL " +
            "GROUP BY origin_city_name;"
        
        $hqlScript = $hqlDropDelaysRaw + $hqlCreateDelaysRaw + $hqlDropDelays + $hqlCreateDelays + $hqlInsertLocal
        
        $hqlScript | Out-File $hqlLocalFileName -Encoding ascii -Force
        #endregion
        
        #region - Upload the Hive script to the default Blob container
        Write-Host "`nUploading the Hive script to the default Blob container ..." -ForegroundColor Green
        
        # Create a storage context object
        $storageAccountKey = (Get-AzureRmStorageAccountKey -StorageAccountName $storageAccountName -ResourceGroupName $resourceGroupName)[0].Value
        $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        
        # Upload the file from local workstation to Blob storage
        Set-AzureStorageBlobContent -File $hqlLocalFileName -Container $blobContainerName -Blob $hqlBlobName -Context $destContext
        #endregion
        
        Write-host "`nEnd of the PowerShell script" -ForegroundColor Green

    Siin on kasutatud skripti muutujaid.

    - **$hqlLocalFileName** - skript salvestab HiveQL skriptifail kohalik enne üleslaadimine bloobimälu. See on faili nime. Vaikeväärtus on <u>C:\tutorials\flightdelay\flightdelays.hql</u>.
    - **$hqlBlobName** – see on HiveQL skripti bloobimälu failinime Azure'i bloobimälu kasutada. Vaikeväärtus on tutorials/flightdelay/flightdelays.hql. Kuna fail kirjutatakse otse Azure'i bloobimälu, pole "/" bloobimälu nime alguses. Kui soovite saada failile juurdepääsu bloobimälu, peate lisamiseks "/" faili nime alguses.
    - **$srcDataFolder** ja **$dstDataFolder** - = "andmete/õpetused/flightdelay" = "õpetused/flightdelay/väljund"


---
##<a id="appendix-c"></a>Lisa C - Sqoop töö väljundi Azure SQL-andmebaasi ettevalmistamine
**SQL-andmebaasi ettevalmistamiseks (ühendamine Sqoop skripti)**

1. Ettevalmistused parameetrid:

    <table border="1">
    <tr><th>Muutuja nimi</th><th>Märkmete</th></tr>
    <tr><td>$sqlDatabaseServerName</td><td>Azure SQL-i andmebaasiserveri nimi. Sisestage midagi luua uus server.</td></tr>
    <tr><td>$sqlDatabaseUsername</td><td>Azure'i SQL-andmebaasi serveri kasutajanimi. Olemasoleva serveri korral $sqlDatabaseServerName kasutatakse kasutajanime ja parooli kinnitamiseks serveriga. Muul juhul neid kasutatakse Loo uus server.</td></tr>
    <tr><td>$sqlDatabasePassword</td><td>Azure'i SQL-andmebaasi serveri sisselogimise parool.</td></tr>
    <tr><td>$sqlDatabaseLocation</td><td>Seda väärtust kasutatakse ainult siis, kui loote uue server Azure'i andmebaas.</td></tr>
    <tr><td>$sqlDatabaseName</td><td>SQL-andmebaasi Sqoop töö AvgDelays tabeli loomiseks kasutada. Nimega HDISqoop andmebaasi loomiseks tühjaks. Tabeli nimi Sqoop töö väljund on AvgDelays. </td></tr>
    </table>
2. Avage Azure'i PowerShelli ISE.
3. Kopeerige ja kleepige järgmine skript paani skripti:

        [CmdletBinding()]
        Param(
        
            # Azure Resource group variables
            [Parameter(Mandatory=$True,
                    HelpMessage="Enter the Azure resource group name. It will be created if it doesn't exist.")]
            [String]$resourceGroupName,
        
            # SQL database server variables
            [Parameter(Mandatory=$True,
                    HelpMessage="Enter the Azure SQL Database Server Name. It will be created if it doesn't exist.")]
            [String]$sqlDatabaseServer, 
        
            [Parameter(Mandatory=$True,
                    HelpMessage="Enter the Azure SQL Database admin user.")]
            [String]$sqlDatabaseLogin,
        
            [Parameter(Mandatory=$True,
                    HelpMessage="Enter the Azure SQL Database admin user password.")]
            [String]$sqlDatabasePassword,
        
            [Parameter(Mandatory=$True,
                    HelpMessage="Enter the region to create the Database in.")]
            [String]$sqlDatabaseLocation,   #For example, West US.
        
            # SQL database variables
            [Parameter(Mandatory=$True,
                    HelpMessage="Enter the database name. It will be created if it doesn't exist.")]
            [String]$sqlDatabaseName # specify the database name if you have one created. Otherwise use "" to have the script create one for you.
        )
        
        # Treat all errors as terminating
        $ErrorActionPreference = "Stop"
        
        #region - Constants and variables
        
        # IP address REST service used for retrieving external IP address and creating firewall rules
        [String]$ipAddressRestService = "http://bot.whatismyipaddress.com"
        [String]$fireWallRuleName = "FlightDelay"
        
        # SQL database variables
        [String]$sqlDatabaseMaxSizeGB = 10
        
        #SQL query string for creating AvgDelays table
        [String]$sqlDatabaseTableName = "AvgDelays"
        [String]$sqlCreateAvgDelaysTable = " CREATE TABLE [dbo].[$sqlDatabaseTableName](
                    [origin_city_name] [nvarchar](50) NOT NULL,
                    [weather_delay] float,
                CONSTRAINT [PK_$sqlDatabaseTableName] PRIMARY KEY CLUSTERED
                (
                    [origin_city_name] ASC
                )
                )"
        #endregion
        
        #Region - Connect to Azure subscription
        Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
        try{Get-AzureRmContext}
        catch{Login-AzureRmAccount}
        #EndRegion
        
        #region - Create and validate Azure resouce group
        try{
            Get-AzureRmResourceGroup -Name $resourceGroupName
        }
        catch{
            New-AzureRmResourceGroup -Name $resourceGroupName -Location $sqlDatabaseLocation
        }
        
        #EndRegion
        
        #region - Create and validate Azure SQL database server
        try{
            Get-AzureRmSqlServer -ServerName $sqlDatabaseServer -ResourceGroupName $resourceGroupName}
        catch{
            Write-Host "`nCreating SQL Database server ..."  -ForegroundColor Green
        
            $sqlDatabasePW = ConvertTo-SecureString -String $sqlDatabasePassword -AsPlainText -Force
            $credential = New-Object System.Management.Automation.PSCredential($sqlDatabaseLogin,$sqlDatabasePW)
        
            $sqlDatabaseServer = (New-AzureRmSqlServer -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -SqlAdministratorCredentials $credential -Location $sqlDatabaseLocation).ServerName
            Write-Host "`tThe new SQL database server name is $sqlDatabaseServer." -ForegroundColor Cyan
        
            Write-Host "`nCreating firewall rule, $fireWallRuleName ..." -ForegroundColor Green
            $workstationIPAddress = Invoke-RestMethod $ipAddressRestService
            New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -FirewallRuleName "$fireWallRuleName-workstation" -StartIpAddress $workstationIPAddress -EndIpAddress $workstationIPAddress
        
            #To allow other Azure services to access the server add a firewall rule and set both the StartIpAddress and EndIpAddress to 0.0.0.0. Note that this allows Azure traffic from any Azure subscription to access the server.
            New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -FirewallRuleName "$fireWallRuleName-Azureservices" -StartIpAddress "0.0.0.0" -EndIpAddress "0.0.0.0"
        }
        
        #endregion
        
        #region - Create and validate Azure SQL database
        
        try {
            Get-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -DatabaseName $sqlDatabaseName
        }
        catch {
            Write-Host "`nCreating SQL Database, $sqlDatabaseName ..."  -ForegroundColor Green
            New-AzureRMSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -DatabaseName $sqlDatabaseName -Edition "Standard" -RequestedServiceObjectiveName "S1"
        }
        
        #endregion
        
        #region -  Execute an SQL command to create the AvgDelays table
        
        Write-Host "`nCreating SQL Database table ..."  -ForegroundColor Green
        $conn = New-Object System.Data.SqlClient.SqlConnection
        $conn.ConnectionString = "Data Source=$sqlDatabaseServer.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabasePassword;Encrypt=true;Trusted_Connection=false;"
        $conn.open()
        $cmd = New-Object System.Data.SqlClient.SqlCommand
        $cmd.connection = $conn
        $cmd.commandtext = $sqlCreateAvgDelaysTable
        $cmd.executenonquery()
        
        $conn.close()
        
        Write-host "`nEnd of the PowerShell script" -ForegroundColor Green

    >[AZURE.NOTE] Skript kasutab esinduslik riigile üleandmise (ülejäänud) teenuse http://bot.whatismyipaddress.com, tuua välise IP-aadressi. IP-aadressi kasutatakse teie SQL-andmebaasi serveri tulemüüri reegli loomiseks.  

    Siin on mõned muutujad skripti kasutada.

    - **$ipAddressRestService** - vaikeväärtus on http://bot.whatismyipaddress.com. See on avaliku IP-aadressi ülejäänud teenuse saada välise IP-aadressi. Kui soovite, saate kasutada muid teenuseid. Teenuse kaudu välise IP-aadressi kasutatakse teie Azure SQL-andmebaasi server, tulemüüri reegli loomiseks nii, et pääsete andmebaasi oma töökoha (Windows PowerShelli skripti abil).
    - **$fireWallRuleName** – see on Azure SQL-andmebaasi server tulemüüri reegli nimi. Vaikenimi on <u>FlightDelay</u>. Te saate ümbernimetamine, kui soovite.
    - **$sqlDatabaseMaxSizeGB** – seda väärtust kasutatakse ainult siis, kui loote uue Azure SQL-andmebaasi server. Vaikeväärtus on 10GB. 10GB piisab selles õpetuses.
    - **$sqlDatabaseName** – seda väärtust kasutatakse ainult siis, kui loote uue SQL Azure'i andmebaasi. Vaikeväärtus on HDISqoop. Kui nimetate, peate värskendama Sqoop Windows PowerShelli skripti vastavalt sellele.

4. Vajutage klahvi **F5** skripti käivitamiseks.
5. Kinnitage skripti väljundi. Veenduge, et skripti käivitus edukalt.

##<a id="nextsteps"></a>Järgmised sammud
Nüüd aru Azure'i bloobimälu faili üleslaadimine, kuidas asustamiseks taru tabeli andmed Azure'i bloobimälu abil, käivitamise taru päringute ja andmete eksportimine HDFS Azure SQL-andmebaasi Sqoop kasutamise kohta. Lisateabe saamiseks lugege järgmisi artikleid:

* [Hdinsightiga töötamise alustamine][hdinsight-get-started]
* [Hdinsightiga taru kasutamine][hdinsight-use-hive]
* [Hdinsightiga Oozie kasutamine][hdinsight-use-oozie]
* [Hdinsightiga Sqoop kasutamine][hdinsight-use-sqoop]
* [Kasutage siga Hdinsightiga][hdinsight-use-pig]
* [Töötada Java MapReduce programmide Hdinsightiga][hdinsight-develop-mapreduce]



[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/


[rita-website]: http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time
[powershell-install-configure]: powershell-install-configure.md

[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-develop-mapreduce]: hdinsight-develop-deploy-java-mapreduce-linux.md

[hadoop-hiveql]: https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL
[hadoop-shell-commands]: http://hadoop.apache.org/docs/r0.18.3/hdfs_shell.html

[technetwiki-hive-error]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx

[image-hdi-flightdelays-avgdelays-dataset]: ./media/hdinsight-analyze-flight-delay-data/HDI.FlightDelays.AvgDelays.DataSet.png
[img-hdi-flightdelays-run-hive-job-output]: ./media/hdinsight-analyze-flight-delay-data/HDI.FlightDelays.RunHiveJob.Output.png
[img-hdi-flightdelays-flow]: ./media/hdinsight-analyze-flight-delay-data/HDI.FlightDelays.Flow.png
