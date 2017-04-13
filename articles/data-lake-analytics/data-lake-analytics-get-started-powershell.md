<properties 
   pageTitle="Azure'i Lake andmeanalüüsi Azure PowerShelli kaudu alustamine | Azure'i" 
   description="Saate teada, kuidas Lake andmesalve konto loomine, looge Lake andmeanalüüsi töö abil U-SQL Azure'i PowerShelli kasutamine ja esitage töö. " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="edmacauley" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/21/2016"
   ms.author="edmaca"/>

# <a name="tutorial-get-started-with-azure-data-lake-analytics-using-azure-powershell"></a>Õpetus: alustamine Azure'i Lake andmeanalüüsi Azure PowerShelli abil

[AZURE.INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Saate teada, kuidas luua Azure'i andmed Lake Analyticsi kontod, määratlemine Lake andmeanalüüsi töö [U-SQL-i](data-lake-analytics-u-sql-get-started.md)ja edastab tööd andmete Lake analüütiliste kontode Azure PowerShelli abil. Andmeanalüüsi Lake kohta leiate lisateavet teemast [Azure andmeanalüüsi Lake ülevaade](data-lake-analytics-overview.md).

Selles õppetükis saate välja tööd, mis loeb vahekaardi eraldatud väärtuste (TSV) fail ja teisendab komaeraldusega väärtuste (CSV) faili. Läbida samas õpetuse muude toetatud tööriistade abil, klõpsake selle jaotise ülaosas vahekaarte.

##<a name="prerequisites"></a>Eeltingimused

Enne alustamist selles õpetuses, peab teil olema järgmised:

- **An Azure'i tellimus**. Leiate [Azure'i saada tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/).
- **Töökoha Azure PowerShelli abil**. Vaadake, [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md).
    
##<a name="create-data-lake-analytics-account"></a>Andmeanalüüsi Lake konto loomine

Saate kasutada mis tahes töö peab teil olema Lake andmeanalüüsi konto. Andmeanalüüsi Lake konto loomiseks peate määrama järgmist:

- **Azure'i ressursirühm**: A Lake andmeanalüüsi konto tuleb luua Azure'i ressursirühm sees. [Azure'i ressursihaldur](../azure-resource-manager/resource-group-overview.md) võimaldab teil töö ressurssidega oma rakenduse rühmana. Saate juurutada, värskendamine või kustutada kõik ressursse rakenduse ühe koordineeritud kasutusel.  

    Loendada ressursi rühmad teie tellimus:
    
        Get-AzureRmResourceGroup
    
    Uue ressursirühma loomiseks tehke järgmist.

        New-AzureRmResourceGroup `
            -Name "<Your resource group name>" `
            -Location "<Azure Data Center>" # For example, "East US 2"

- **Andmete Lake Analytics konto nimi**
- **Asukoht**: üks Azure andmekeskuste, mis toetab Lake andmeanalüüsi.
- **Vaikimisi andmete Lake konto**: iga Lake andmeanalüüsi konto on vaikimisi andmete Lake konto.

    Andmete Lake uue konto loomiseks tehke järgmist.

        New-AzureRmDataLakeStoreAccount `
            -ResourceGroupName "<Your Azure resource group name>" `
            -Name "<Your Data Lake account name>" `
            -Location "<Azure Data Center>"  # For example, "East US 2"

    > [AZURE.NOTE] Andmete Lake konto nimi peab sisaldama ainult väiketähti ja numbreid.



**Andmeanalüüsi Lake konto loomine**

1. Avage oma töökoha Windows PowerShell ISE.
2. Käivitage järgmine skript:

        $resourceGroupName = "<ResourceGroupName>"
        $dataLakeStoreName = "<DataLakeAccountName>"
        $dataLakeAnalyticsName = "<DataLakeAnalyticsAccountName>"
        $location = "East US 2"
        
        Write-Host "Create a resource group ..." -ForegroundColor Green
        New-AzureRmResourceGroup `
            -Name  $resourceGroupName `
            -Location $location
        
        Write-Host "Create a Data Lake account ..."  -ForegroundColor Green
        New-AzureRmDataLakeStoreAccount `
            -ResourceGroupName $resourceGroupName `
            -Name $dataLakeStoreName `
            -Location $location 
        
        Write-Host "Create a Data Lake Analytics account ..."  -ForegroundColor Green
        New-AzureRmDataLakeAnalyticsAccount `
            -Name $dataLakeAnalyticsName `
            -ResourceGroupName $resourceGroupName `
            -Location $location `
            -DefaultDataLake $dataLakeStoreName
        
        Write-Host "The newly created Data Lake Analytics account ..."  -ForegroundColor Green
        Get-AzureRmDataLakeAnalyticsAccount `
            -ResourceGroupName $resourceGroupName `
            -Name $dataLakeAnalyticsName  

##<a name="upload-data-to-data-lake"></a>Andmete Lake andmete üleslaadimine

Selles õpetuses töötlete mõne otsingu logid.  Otsingu log talletatud andmed Lake poe või Azure'i bloobimälu. 

Logifaili näidis otsing on kopeeritud avaliku Azure'i bloobimälu ümbrises. Kasutage järgmist PowerShelli skripti faili allalaadimiseks oma töökoha ja laadige fail vaikekonto Lake andmesalve Lake andmeanalüüsi kontole.

    $dataLakeStoreName = "<The default Data Lake Store account name>"
    
    $localFolder = "C:\Tutorials\Downloads\" # A temp location for the file. 
    $storageAccount = "adltutorials"  # Don't modify this value.
    $container = "adls-sample-data"  #Don't modify this value.

    # Create the temp location  
    New-Item -Path $localFolder -ItemType Directory -Force 

    # Download the sample file from Azure Blob storage
    $context = New-AzureStorageContext -StorageAccountName $storageAccount -Anonymous
    $blobs = Azure\Get-AzureStorageBlob -Container $container -Context $context
    $blobs | Get-AzureStorageBlobContent -Context $context -Destination $localFolder

    # Upload the file to the default Data Lake Store account    
    Import-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path $localFolder"SearchLog.tsv" -Destination "/Samples/Data/SearchLog.tsv"

Järgmist PowerShelli skripti kuvatakse hankimine Lake andmesalve vaikenime Lake andmeanalüüsi konto.


    $resourceGroupName = "<ResourceGroupName>"
    $dataLakeAnalyticsName = "<DataLakeAnalyticsAccountName>"
    $dataLakeStoreName = (Get-AzureRmDataLakeAnalyticsAccount -ResourceGroupName $resourceGroupName -Name $dataLakeAnalyticsName).Properties.DefaultDataLakeAccount
    echo $dataLakeStoreName

>[AZURE.NOTE] Azure'i portaalis on teie kasutajaliidese näidisfailide andmete kopeerimiseks Lake andmesalve vaikekonto. Juhised leiate teemast [Azure Lake andmeanalüüsi Azure'i portaalis alustamine](data-lake-analytics-get-started-portal.md#upload-data-to-the-default-data-lake-store-account).

Azure'i bloobimälu pääsete juurde ka Lake andmeanalüüsi.  Azure'i bloobimälu üles andmeid, vt [Azure PowerShelli kasutamine Azure Storage](../storage/storage-powershell-guide-full.md).

##<a name="submit-data-lake-analytics-jobs"></a>Esitage Lake andmeanalüüsi tööde haldamine

Andmeanalüüsi Lake töö kirjutada U-SQL-i keeles. A-SQL-i kohta leiate lisateavet teemast [Alustamine U-SQL-i keele](data-lake-analytics-u-sql-get-started.md) ja [U-SQL keele viide](http://go.microsoft.com/fwlink/?LinkId=691348).

**Luua skripti Lake andmeanalüüsi töö**

- Järgmise U-SQL skripti tekstifaili luua, ja salvestage tekstifail oma töökoha:

        @searchlog =
            EXTRACT UserId          int,
                    Start           DateTime,
                    Region          string,
                    Query           string,
                    Duration        int?,
                    Urls            string,
                    ClickedUrls     string
            FROM "/Samples/Data/SearchLog.tsv"
            USING Extractors.Tsv();
        
        OUTPUT @searchlog   
            TO "/Output/SearchLog-from-Data-Lake.csv"
        USING Outputters.Csv();

    A-SQL-skripti abil **Extractors.Tsv()**andmefailis loeb ja loob siis CSV-faili abil **Outputters.Csv()**. 
    
    Ärge muutke kaks teede kui lähtefail kopeerimine teise asukohta.  Kui see ei eksisteeri, loob Lake andmeanalüüsi väljund kausta.
    
    Lihtsam on kasutada suhtelised teed failid vaikimisi andmete Lake kontod. Samuti saate absoluutne teed.  Näide 
    
        adl://<Data LakeStorageAccountName>.azuredatalakestore.net:443/Samples/Data/SearchLog.tsv
        
    Failide lingitud salvestusruumi kontod juurdepääsuks peate kasutama absoluutne teed.  Lingitud Azure Storage konto talletatud failide süntaks on järgmine:
    
        wasb://<BlobContainerName>@<StorageAccountName>.blob.core.windows.net/Samples/Data/SearchLog.tsv

    >[AZURE.NOTE] Azure'i bloobimälu container avaliku plekid või avaliku ümbriste juurdepääsuõigused pole praegu toetatud.    
    
    
**Esitada töö**

1. Avage oma töökoha Windows PowerShell ISE.
2. Käivitage järgmine skript:

        $dataLakeAnalyticsName = "<DataLakeAnalyticsAccountName>"
        $usqlScript = "c:\tutorials\data-lake-analytics\copyFile.usql"
        
        $job = Submit-AzureRmDataLakeAnalyticsJob -Name "convertTSVtoCSV" -AccountName $dataLakeAnalyticsName –ScriptPath $usqlScript 

        Wait-AdlJob -Account $dataLakeAnalyticsName -JobId $job.JobId

        Get-AzureRmDataLakeAnalyticsJob -AccountName $dataLakeAnalyticsName -JobId $job.JobId

    Skript, talletatakse skriptifail U-SQL-i veebisaidil c:\tutorials\data-lake-analytics\copyFile.usql. Faili tee vastavalt uuendada.
 
Pärast töö on lõpule viidud, saate faili järgmised cmdlet-käskude abil ja faili alla laadida.
    
    $resourceGroupName = "<Resource Group Name>"
    $dataLakeAnalyticName = "<Data Lake Analytic Account Name>"
    $destFile = "C:\tutorials\data-lake-analytics\SearchLog-from-Data-Lake.csv"
    
    $dataLakeStoreName = (Get-AzureRmDataLakeAnalyticsAccount -ResourceGroupName $resourceGroupName -Name $dataLakeAnalyticName).Properties.DefaultDataLakeAccount
    
    Get-AzureRmDataLakeStoreChildItem -AccountName $dataLakeStoreName -path "/Output"
    
    Export-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path "/Output/SearchLog-from-Data-Lake.csv" -Destination $destFile

## <a name="see-also"></a>Vt ka

- Muude tööriistade abil sama õpetuse vaatamiseks klõpsake menüü lülitid lehe ülaosas.
- Keerukama päringu, leiate artiklist [analüüsi veebisaidi logid Azure'i Lake andmeanalüüsi abil](data-lake-analytics-analyze-weblogs.md).
- Alustamiseks U-SQL-i rakenduste arendamise, lugege teemat [arendada U-SQL-i skriptide abil andmete Lake Tools for Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).
- A-SQL-is leiate teemast [Azure andmete Lake Analytics U-SQL-i keele kasutamise alustamine](data-lake-analytics-u-sql-get-started.md).
- Vaadake haldamise toiminguid, [Hallata Azure Lake andmeanalüüsi Azure'i portaalis](data-lake-analytics-manage-use-portal.md).
- Andmeanalüüsi Lake ülevaate saamiseks vt [Azure'i andmeanalüüsi Lake ülevaade](data-lake-analytics-overview.md).

