<properties
   pageTitle="Luua Hdinsightiga kogumite Azure'i Lake andmesalve PowerShelli kaudu | Azure'i"
   description="Loomine ja kasutamine andmete järvevaatega Azure Hdinsightiga kogumite Azure PowerShelli kasutamine"
   services="data-lake-store,hdinsight" 
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/21/2016"
   ms.author="nitinme"/>

# <a name="create-an-hdinsight-cluster-with-data-lake-store-using-azure-powershell"></a>Luua mõne Hdinsightiga kobar Lake andmesalve Azure PowerShelli abil

> [AZURE.SELECTOR]
- [Portaalis](data-lake-store-hdinsight-hadoop-use-portal.md)
- [PowerShelli kasutamine](data-lake-store-hdinsight-hadoop-use-powershell.md)
- [Ressursihaldur abil](data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)

Saate teada, kuidas Azure'i PowerShelli abil saate konfigureerida juurdepääsu Azure andmesalve Lake Hdinsightiga kobar, (Hadoopi, HBase või Storm). Mõned olulised kaalutlused selles versioonis:

* **Jaoks säde kogumite (Linux) ja Hadoopi/torm kogumite (Windows ja Linux)**, Lake andmesalve saab kasutada ainult konto täiendav salvestusruum. Sellise rühmad salvestusruumi vaikekonto ikkagi Azure'i salvestusruumi plekid (WASB).

* **Jaoks HBase kogumite (Windows ja Linux)**, Lake andmesalve saate kasutada vaikimisi salvestusruumi või täiendava salvestusruumi.

> [AZURE.NOTE] Pange tähele, et mõned olulised punktid. 
> 
> * Võimalus luua Hdinsightiga kogumite juurdepääsu Lake andmesalve on saadaval ainult Hdinsightiga versioonid 3,2 ja 3.4 (for Windows kui ka Linux Hadoopi, HBase ja torm kogumid). Säde kogumite Linux, see suvand on ainult Hdinsightiga 3.4 kogumite saadaval.
>
> * Eespool nimetatud Lake andmesalve on saadaval vaikimisi salvestusruumi teatava kobar (HBase) ja muude kobar (Hadoopi, säde, Storm) täiendav salvestusruum. Kasutades Lake andmesalve konto täiendav salvestusruum mõjutada jõudlust või kirjutuskaitse säilitamist kaudu klaster võimalus. Kui andmesalve Lake kasutatakse täiendav salvestusruum stsenaariumi kirjutada kobar seotud faile (nt logid jne) vaikimisi salvestusruumi (Azure'i plekid), ajal andmed, mida soovite töödelda saab salvestada Lake andmesalve konto.


Selles artiklis me ettevalmistamise Hadoopi kobar andmete Lake poe nimega täiendav salvestusruum.

Konfigureerimine Hdinsightiga töötamiseks Lake andmesalve PowerShelli kaudu hõlmab järgmist:

* Azure'i Lake andmesalve loomine
* Autentimise Lake andmesalve Rollipõhine juurdepääsu häälestamine
* Luua Hdinsightiga kobar Lake andmesalve autentimine
* Käivitage test töö klaster

## <a name="prerequisites"></a>Eeltingimused

Enne alustamist selles õpetuses, peab teil olema järgmised:

- **An Azure'i tellimus**. Leiate [Azure'i saada tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/).

- **Azure'i PowerShelli 1.0 või suurem**. Vaadake, [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md).

- **Windows SDK**. Saate selle installida [siia](https://dev.windows.com/en-us/downloads). Kasutage seda turbeserti loomiseks.

- **Azure Active Directory teenuse põhilise**. Selle õpetuse juhised leiate juhised, kuidas luua teenuse põhisumma Azure AD. Siiski peate olema administraator Azure AD saama loomine teenuse põhisumma. Kui olete Azure AD administraator, saate selle nõutav vahele jätta ja jätkata õpetuse.
    
    **Kui te ei ole administraator Azure AD**, ei saa luua teenuse põhilise nõutav toimingute. Sellisel juhul Azure AD administraator peate esmalt looma teenuse põhisumma enne saate luua ka Hdinsightiga kobar Lake andmesalve. Lisaks teenuse põhilise tuleb luua serdiga, nagu on kirjeldatud aadressil [põhisumma serdiga teenuse](../resource-group-authenticate-service-principal.md#create-service-principal-with-certificate). 


## <a name="create-an-azure-data-lake-store"></a>Azure'i Lake andmesalve loomine

Järgmiste juhiste abil luua andmete Lake pood.

1. Oma töölaualt avamine uues aknas Azure PowerShelli ja sisestage järgmine koodilõigu. Kui teil palutakse sisse logida, veenduge, et logite ühe tellimuse admininistrators või omanik.

        # Log in to your Azure account
        Login-AzureRmAccount

        # List all the subscriptions associated to your account
        Get-AzureRmSubscription

        # Select a subscription
        Set-AzureRmContext -SubscriptionId <subscription ID>

        # Register for Data Lake Store
        Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"

    >[AZURE.NOTE] Kui saate tõrketeate, mis on sarnane `Register-AzureRmResourceProvider : InvalidResourceNamespace: The resource namespace 'Microsoft.DataLakeStore' is invalid` Lake andmesalve ressursi pakkuja registreerimisel on võimalik, et teie subsrcription on whitelisted Azure'i andmed Lake poe. Veenduge, et lubate Azure tellimuse Lake andmesalve avaliku eelvaate neid [juhiseid](data-lake-store-get-started-portal.md#signup)järgides.

3. Azure'i andmesalve Lake konto on seostatud mõne Azure'i ressursirühma. Alustuseks on Azure ressursirühm loomine.

        $resourceGroupName = "<your new resource group name>"
        New-AzureRmResourceGroup -Name $resourceGroupName -Location "East US 2"

    ![Azure'i ressursi rühma loomine] (./media/data-lake-store-hdinsight-hadoop-use-powershell/ADL.PS.CreateResourceGroup.png "Azure'i ressursi rühma loomine")

2. Looge konto Azure andmesalve Lake. Konto nimi peab sisaldama ainult väiketähti ja numbreid.

        $dataLakeStoreName = "<your new Data Lake Store name>"
        New-AzureRmDataLakeStoreAccount -ResourceGroupName $resourceGroupName -Name $dataLakeStoreName -Location "East US 2"

    ![Azure'i andmed Lake konto loomine] (./media/data-lake-store-hdinsight-hadoop-use-powershell/ADL.PS.CreateADLAcc.png "Azure'i andmed Lake konto loomine")

3. Veenduge, et konto loonud.

        Test-AzureRmDataLakeStoreAccount -Name $dataLakeStoreName

    Väljundi see peaks olema **täidetud**.

4. Näidisandmete Azure'i andmed Lake üles laadida. Kasutame see selle artikli kinnitamaks, et andmed on juurdepääsetav ka Hdinsightiga kobar. Kui otsite Näidisandmete üles laadida, saate kausta **Kiirabi andmed** [Azure'i andmed Lake Git hoidla](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData).


        $myrootdir = "/"
        Import-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path "C:\<path to data>\vehicle1_09142014.csv" -Destination $myrootdir\vehicle1_09142014.csv


## <a name="set-up-authentication-for-role-based-access-to-data-lake-store"></a>Autentimise Lake andmesalve Rollipõhine juurdepääsu häälestamine

Iga Azure'i tellimus on seostatud Azure Active Directory. Kasutajate ja teenuseid, mida kasutada ressursse tellimuse Azure klassikaline portaali või Azure ressursihaldur API abil peate esmalt autentida selle Azure Active Directory. Juurdepääs on antud Azure'i tellimused ja teenuste määrates neile on Azure ressursi sobiv roll.  Teenuste, tuvastab teenuse põhilise teenuse Azure Active Directory (AAD). Selles jaotises illustreerib, kuidas anda teenuse rakendus, nagu Hdinsightiga juurdepääs on Azure ressurss (varem loodud Azure'i andmesalve Lake konto) teenuse põhilise rakenduse loomine ja rollide määramine mis Azure PowerShelli kaudu.

Active Directory autentimise Azure'i andmed Lake häälestada, peate tegema järgmist.

* Iseallkirjastatud serdi loomine
* Azure Active Directory ja teenuse põhilise rakenduse loomine

### <a name="create-a-self-signed-certificate"></a>Iseallkirjastatud serdi loomine

Veenduge, et teil on [Windows SDK](https://dev.windows.com/en-us/downloads) installitud enne selle jaotise juhiseid. Te peate olema ka loonud kataloogi, nt **C:\mycertdir**, kus luuakse serdi.

1. PowerShelli aknas, liikuge asukohta, kuhu installisite Windows SDK (tavaliselt `C:\Program Files (x86)\Windows Kits\10\bin\x86` ja kasutada [MakeCert] [ makecert] kasuliku luua iseallkirjastatud serti ja lisamine. Saate kasutada järgmisi käske.

        $certificateFileDir = "<my certificate directory>"
        cd $certificateFileDir
        $startDate = (Get-Date).ToString('MM/dd/yyyy')
        $endDate = (Get-Date).AddDays(365).ToString('MM/dd/yyyy')

        makecert -sv mykey.pvk -n "cn=HDI-ADL-SP" CertFile.cer -b $startDate -e $endDate -r -len 2048

    Teil palutakse privaatse võtme parooli sisestada. Pärast käsu täidab edukalt, peaksite nägema **CertFile.cer** ja **mykey.pvk** määratud serdi kataloogis.

4. Kasutage [Pvk2Pfx] [ pvk2pfx] kasuliku MakeCert loodud faile .pvk ja CER teisendamiseks pfx-fail. Käivitage järgmine käsk.

        pvk2pfx -pvk mykey.pvk -spc CertFile.cer -pfx CertFile.pfx -po <password>

    Küsimisel sisestage teie määratud varem privaatse võtme parool. Teie määratud **-po** parameetri väärtus parool, mida on seostatud pfx-fail. Kui käsk on edukalt lõpule jõudnud, peaksite nägema on määratud serdi kataloogis CertFile.pfx.

###  <a name="create-an-azure-active-directory-and-a-service-principal"></a>Azure Active Directory ja teenuse põhilise loomine

Selle jaotise juhiste loomine teenuse põhisumma rakenduse Azure Active Directory, rolli määramine põhisumma teenuse ja teenuse peamise autentida esitada sert. Käivitage järgmised käsud Azure Active Directory rakenduse loomiseks.

1. Kleepige aknas PowerShelli konsooli järgmised cmdlet-käsud. Veenduge, et teie määratud **- DisplayName** atribuudi väärtus on kordumatu. Lisaks **- Koduleht** ja **- IdentiferUris** väärtused on kohatäite väärtused ja on kinnitatud.

        $certificateFilePath = "$certificateFileDir\CertFile.pfx"

        $password = Read-Host –Prompt "Enter the password" # This is the password you specified for the .pfx file

        $certificatePFX = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2($certificateFilePath, $password)

        $rawCertificateData = $certificatePFX.GetRawCertData()

        $credential = [System.Convert]::ToBase64String($rawCertificateData)

        $application = New-AzureRmADApplication `
                    -DisplayName "HDIADL" `
                    -HomePage "https://contoso.com" `
                    -IdentifierUris "https://mycontoso.com" `
                    -KeyValue $credential  `
                    -KeyType "AsymmetricX509Cert"  `
                    -KeyUsage "Verify"  `
                    -StartDate $startDate  `
                    -EndDate $endDate

        $applicationId = $application.ApplicationId

2. Looge teenuse subjekt abil rakenduste ID-ga.

        $servicePrincipal = New-AzureRmADServicePrincipal -ApplicationId $applicationId

        $objectId = $servicePrincipal.Id

3. Andke teenuse põhisumma juurdepääs Lake andmesalve faili/kaust, kus pääsete Hdinsightiga kobar kaudu. Allpool koodilõigu pakub juurdepääsu juurkaustas Lake andmesalve konto.

        Set-AzureRmDataLakeStoreItemAclEntry -AccountName $dataLakeStoreName -Path / -AceType User -Id $objectId -Permissions All

    Kuvatakse vastav viip, sisestage **Y** kinnitamiseks.

## <a name="create-an-hdinsight-cluster-with-authentication-to-data-lake-store"></a>Luua ka Hdinsightiga kobar Lake andmesalve autentimine

Selles jaotises saame luua mõne Hdinsightiga Hadoopi kobar. See väljaanne Hdinsightiga kobar ja Lake andmesalve peab olema paigal (Ida-USA 2).

1. Alustage toomine rentniku Tellimuse ID-ga. Mis on hiljem vaja.

        $tenantID = (Get-AzureRmContext).Tenant.TenantId

2. Selles versioonis jaoks Hadoopi kobar, Lake andmesalve saab kasutada ainult mõne täiendav salvestusruum nimega klaster jaoks. Vaikimisi salvestusruumi ikkagi Azure storage plekid (WASB). Tuleb esmalt loome salvestusruumi konto ja salvestusruumi ümbriste klaster jaoks nõutavad.

        # Create an Azure storage account
        $location = "East US 2"
        $storageAccountName = "<StorageAcccountName>"   # Provide a Storage account name

        New-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -StorageAccountName $storageAccountName -Location $location -Type Standard_GRS

        # Create an Azure Blob Storage container
        $containerName = "<ContainerName>"              # Provide a container name
        $storageAccountKey = Get-AzureRmStorageAccountKey -Name $storageAccountName -ResourceGroupName $resourceGroupName | %{ $_.Key1 }
        $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        New-AzureStorageContainer -Name $containerName -Context $destContext

3. Looge Hdinsightiga kobar. Kasutage järgmised cmdlet-käsud.

        # Set these variables
        $clusterName = $containerName                   # As a best practice, have the same name for the cluster and container
        $clusterNodes = <ClusterSizeInNodes>            # The number of nodes in the HDInsight cluster
        $httpCredentials = Get-Credential
        $rdpCredentials = Get-Credential

        New-AzureRmHDInsightCluster -ClusterName $clusterName -ResourceGroupName $resourceGroupName -HttpCredential $httpCredentials -Location $location -DefaultStorageAccountName "$storageAccountName.blob.core.windows.net" -DefaultStorageAccountKey $storageAccountKey -DefaultStorageContainer $containerName  -ClusterSizeInNodes $clusterNodes -ClusterType Hadoop -Version "3.2" -RdpCredential $rdpCredentials -RdpAccessExpiry (Get-Date).AddDays(14) -ObjectID $objectId -AadTenantId $tenantID -CertificateFilePath $certificateFilePath -CertificatePassword $password

    Pärast cmdlet edukalt lõpule viidud, peaksite nägema väljund umbes järgmine:

        Name                      : hdiadlcluster
        Id                        : /subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/hdiadlgroup/providers/Mi
                                    crosoft.HDInsight/clusters/hdiadlcluster
        Location                  : East US 2
        ClusterVersion            : 3.2.7.707
        OperatingSystemType       : Windows
        ClusterState              : Running
        ClusterType               : Hadoop
        CoresUsed                 : 16
        HttpEndpoint              : hdiadlcluster.azurehdinsight.net
        Error                     :
        DefaultStorageAccount     :
        DefaultStorageContainer   :
        ResourceGroup             : hdiadlgroup
        AdditionalStorageAccounts :

## <a name="run-test-jobs-on-the-hdinsight-cluster-to-use-the-data-lake-store"></a>Hdinsightiga klaster kasutada Lake andmesalve testi tööde käitamine

Pärast seda, kui olete konfigureerinud mõne Hdinsightiga kobar, käivitada kobar testida, et Hdinsightiga kobar on juurdepääs Lake andmesalve testi tööde haldamine. Selleks võtame valimi taru tööd, mis loob teie andmesalve Lake varem üleslaaditud Näidisandmete abil tabeli.

### <a name="for-a-linux-cluster"></a>Jaoks Linux kobar

Selles jaotises saate küll SSH kobar ja Käivita on valimi taru päringu. Windows ei paku sisseehitatud SSH klient. Soovitame kasutada **kitt**, mille saate alla laadida [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

PuTTY kasutamise kohta leiate lisateavet teemast [Kasutamine SSH koos Linux-põhine Hadoopi Hdinsightiga Windows ](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).

1. Kui ühendus on loodud, käivitage taru CLI järgmine käsk:

        hive

2. Sisestage CLI kasutades nimega **sõidukite** Lake andmesalve Näidisandmete abil uue tabeli loomiseks järgmistest:

        DROP TABLE vehicles;
        CREATE EXTERNAL TABLE vehicles (str string) LOCATION 'adl://<mydatalakestore>.azuredatalakestore.net:443/';
        SELECT * FROM vehicles LIMIT 10;

    Peaksite nägema järgmine väljund:

        1,1,2014-09-14 00:00:03,46.81006,-92.08174,51,S,1
        1,2,2014-09-14 00:00:06,46.81006,-92.08174,13,NE,1
        1,3,2014-09-14 00:00:09,46.81006,-92.08174,48,NE,1
        1,4,2014-09-14 00:00:12,46.81006,-92.08174,30,W,1
        1,5,2014-09-14 00:00:15,46.81006,-92.08174,47,S,1
        1,6,2014-09-14 00:00:18,46.81006,-92.08174,9,S,1
        1,7,2014-09-14 00:00:21,46.81006,-92.08174,53,N,1
        1,8,2014-09-14 00:00:24,46.81006,-92.08174,63,SW,1
        1,9,2014-09-14 00:00:27,46.81006,-92.08174,4,NE,1
        1,10,2014-09-14 00:00:30,46.81006,-92.08174,31,N,1


### <a name="for-a-windows-cluster"></a>For Windows kobar

Kasutage taru päringu käivitamiseks järgmised cmdlet-käsud. Selles päringus Lake andmesalve andmete põhjal tabeli loomiseks ja käivitage päring loodud tabel.

    $queryString = "DROP TABLE vehicles;" + "CREATE EXTERNAL TABLE vehicles (str string) LOCATION 'adl://$dataLakeStoreName.azuredatalakestore.net:443/';" + "SELECT * FROM vehicles LIMIT 10;"

    $hiveJobDefinition = New-AzureRmHDInsightHiveJobDefinition -Query $queryString

    $hiveJob = Start-AzureRmHDInsightJob -ResourceGroupName $resourceGroupName -ClusterName $clusterName -JobDefinition $hiveJobDefinition -ClusterCredential $httpCredentials

    Wait-AzureRmHDInsightJob -ResourceGroupName $resourceGroupName -ClusterName $clusterName -JobId $hiveJob.JobId -ClusterCredential $httpCredentials

See on järgmine väljund. **ExitValue** 0 väljund soovitab töö lõpule viidud.

    Cluster         : hdiadlcluster.
    HttpEndpoint    : hdiadlcluster.azurehdinsight.net
    State           : SUCCEEDED
    JobId           : job_1445386885331_0012
    ParentId        :
    PercentComplete :
    ExitValue       : 0
    User            : admin
    Callback        :
    Completed       : done

Tuua väljund töö abil järgmine cmdlet-käsk:

    Get-AzureRmHDInsightJobOutput -ClusterName $clusterName -JobId $hiveJob.JobId -DefaultContainer $containerName -DefaultStorageAccountName $storageAccountName -DefaultStorageAccountKey $storageAccountKey -ClusterCredential $httpCredentials

Töö väljund sarnaneb järgmisega:

    1,1,2014-09-14 00:00:03,46.81006,-92.08174,51,S,1
    1,2,2014-09-14 00:00:06,46.81006,-92.08174,13,NE,1
    1,3,2014-09-14 00:00:09,46.81006,-92.08174,48,NE,1
    1,4,2014-09-14 00:00:12,46.81006,-92.08174,30,W,1
    1,5,2014-09-14 00:00:15,46.81006,-92.08174,47,S,1
    1,6,2014-09-14 00:00:18,46.81006,-92.08174,9,S,1
    1,7,2014-09-14 00:00:21,46.81006,-92.08174,53,N,1
    1,8,2014-09-14 00:00:24,46.81006,-92.08174,63,SW,1
    1,9,2014-09-14 00:00:27,46.81006,-92.08174,4,NE,1
    1,10,2014-09-14 00:00:30,46.81006,-92.08174,31,N,1


## <a name="access-data-lake-store-using-hdfs-commands"></a>Accessi andmete Lake poe HDFS käskude kasutamine

Kui olete konfigureerinud Hdinsightiga kobar Lake andmesalve kasutada, saate HDFS shell käsud juurdepääs store.

### <a name="for-a-linux-cluster"></a>Jaoks Linux kobar

Selle jaotise kuvatakse SSH klaster sisse ja käivitage HDFS käsud. Windows ei paku sisseehitatud SSH klient. Soovitame kasutada **kitt**, mille saate alla laadida [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

PuTTY kasutamise kohta leiate lisateavet teemast [Kasutamine SSH koos Linux-põhine Hadoopi Hdinsightiga Windows ](../hdinsight/hdinsight-hadoop-linux-use-ssh-windows.md).

Kui ühendus on loodud, kasutage järgmist HDFS failisüsteemi käsku Lake andmesalve faili.

    hdfs dfs -ls adl://<Data Lake Store account name>.azuredatalakestore.net:443/

See peaks loendis andmesalve Lake varem üleslaaditud faili.

    15/09/17 21:41:15 INFO web.CaboWebHdfsFileSystem: Replacing original urlConnectionFactory with org.apache.hadoop.hdfs.web.URLConnectionFactory@21a728d6
    Found 1 items
    -rwxrwxrwx   0 NotSupportYet NotSupportYet     671388 2015-09-16 22:16 adl://mydatalakestore.azuredatalakestore.net:443/mynewfolder

Saate kasutada ka funktsiooni `hdfs dfs -put` Lake andmesalve mõned failide üleslaadimine ja seejärel kasutage käsku `hdfs dfs -ls` kontrollida, kas failid on edukalt üles.


### <a name="for-a-windows-cluster"></a>For Windows kobar

1. Uue [Azure portaali](https://portal.azure.com)sisse logida.

2. Klõpsake nuppu **Sirvi**, klõpsake **Hdinsightiga kogumite**ja klõpsake Hdinsightiga kobar loodud.

3. Klõpsake **Kaugtöölaua**labale kobar ja seejärel **Kaugtöölaua** tera, klõpsake käsku **Ühenda**.

    ![Remote üheks HDI kobar] (./media/data-lake-store-hdinsight-hadoop-use-powershell/ADL.HDI.PS.Remote.Desktop.png "Azure'i ressursi rühma loomine")

    Küsimise korral sisestage esitatud serveri töölaua kasutaja mandaat.

4. Kaugseanss, käivitage Windows PowerShelli ja Azure Lake andmesalve faili HDFS failisüsteemi käskude abil.

        hdfs dfs -ls adl://<Data Lake Store account name>.azuredatalakestore.net:443/

    See peaks loendis andmesalve Lake varem üleslaaditud faili.

        15/09/17 21:41:15 INFO web.CaboWebHdfsFileSystem: Replacing original urlConnectionFactory with org.apache.hadoop.hdfs.web.URLConnectionFactory@21a728d6
        Found 1 items
        -rwxrwxrwx   0 NotSupportYet NotSupportYet     671388 2015-09-16 22:16 adl://mydatalakestore.azuredatalakestore.net:443/vehicle1_09142014.csv

    Saate kasutada ka funktsiooni `hdfs dfs -put` Lake andmesalve mõned failide üleslaadimine ja seejärel kasutage käsku `hdfs dfs -ls` kontrollida, kas failid on edukalt üles.

## <a name="see-also"></a>Vt ka

* [Portaal: Loomine Lake andmesalve kasutada mõne Hdinsightiga kobar](data-lake-store-hdinsight-hadoop-use-portal.md)

[makecert]: https://msdn.microsoft.com/library/windows/desktop/ff548309(v=vs.85).aspx
[pvk2pfx]: https://msdn.microsoft.com/library/windows/desktop/ff550672(v=vs.85).aspx
