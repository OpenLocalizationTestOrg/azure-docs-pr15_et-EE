<properties 
    pageTitle="Kohapeal on elastne andmebaasi (PowerShelli) haldamine | Microsoft Azure'i" 
    description="Saate teada, kuidas kohapeal on elastne andmebaasi haldamine PowerShelli abil."  
    services="sql-database" 
    documentationCenter="" 
    authors="srinia" 
    manager="jhubbard" 
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="powershell"
    ms.workload="data-management" 
    ms.date="06/22/2016"
    ms.author="srinia"/>

# <a name="monitor-and-manage-an-elastic-database-pool-with-powershell"></a>Jälgimine ja haldamine on elastne andmebaasi rakenduskausta PowerShelli abil 

> [AZURE.SELECTOR]
- [Azure'i portaal](sql-database-elastic-pool-manage-portal.md)
- [PowerShelli](sql-database-elastic-pool-manage-powershell.md)
- [C#:](sql-database-elastic-pool-manage-csharp.md)
- [T-SQL-IS](sql-database-elastic-pool-manage-tsql.md)

Hallata on [elastne andmebaasi rakenduskausta](sql-database-elastic-pool.md) PowerShelli cmdlet-käskude abil. 

Levinud tõrkekoodid, lugege teemat [SQL-i tõrkekoodid SQL-andmebaasi klientrakendustes: andmebaasi ühenduse tõrge ja ka muude probleemide](sql-database-develop-error-messages.md).

Väärtuste kaustu leiate [eDTU ja salvestusruumi piirangud](sql-database-elastic-pool.md#eDTU-and-storage-limits-for-elastic-pools-and-elastic-databases). 

## <a name="prerequisites"></a>Eeltingimused

* Azure'i PowerShelli 1.0 või uuem versioon. Üksikasjalikku teavet, vaadake, [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md).
* Elastne andmebaasi kaustu saadaval ainult SQL-i andmebaasi V12 serveritega. Kui teil on SQL-i andmebaasi V11 server, sammhaaval [PowerShelliga versioonile V12 ja koostada andmebaas](sql-database-upgrade-server-portal.md) .


## <a name="move-a-database-into-an-elastic-pool"></a>Andmebaasi viimine kohapeal on elastne

Saate teisaldada andmebaasi või sealt välja lisamine rakenduskausta [Set-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt619433.aspx)abil. 

    Set-AzureRmSqlDatabase -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -ElasticPoolName "elasticpool1"

## <a name="change-performance-settings-of-a-pool"></a>Jõudluse, on sätete muutmine

Kui jõudluse kannatab, saate muuta sätteid pool kasvu. Kasutage cmdlet [Set-AzureRmSqlElasticPool](https://msdn.microsoft.com/library/azure/mt603511.aspx) . Määratud parameetri - Dtu eDTUs rakenduskausta kohta. Näha võimalikud väärtused [eDTU ja salvestusruumi piirangud](sql-database-elastic-pool.md#eDTU-and-storage-limits-for-elastic-pools-and-elastic-databases) .  

    Set-AzureRmSqlElasticPool –ResourceGroupName “resourcegroup1” –ServerName “server1” –ElasticPoolName “elasticpool1” –Dtu 1200 –DatabaseDtuMax 100 –DatabaseDtuMin 50 


## <a name="get-the-status-of-pool-operations"></a>Rakenduskausta toimingute oleku hankimine

On loomine võib võtta aega. Rakenduskausta toimingud, sh loomise ja värskenduste olekut, kasutage [Get-AzureRmSqlElasticPoolActivity](https://msdn.microsoft.com/library/azure/mt603812.aspx) cmdlet-käsk.

    Get-AzureRmSqlElasticPoolActivity –ResourceGroupName “resourcegroup1” –ServerName “server1” –ElasticPoolName “elasticpool1” 


## <a name="get-the-status-of-moving-an-elastic-database-into-and-out-of-a-pool"></a>Oleku ja elastne andmebaasi teisaldamine ja sealt on hankimine

Andmebaasi teisaldamine võib võtta aega. Teisalda olek, [Get-AzureRmSqlDatabaseActivity](https://msdn.microsoft.com/library/azure/mt603687.aspx) cmdlet-käsu abil saate jälitada.

    Get-AzureRmSqlDatabaseActivity -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -ElasticPoolName "elasticpool1"

## <a name="get-resource-usage-data-for-a-pool"></a>On ressursi kasutamine andmete hankimine

Saate tuua ressursside rakenduskausta limiit protsendina mõõdikud:   


| Argumendil nimi | Kirjeldus |
| :-- | :-- |
| CPU\_% | Keskmine Arvuta kasutamise limiit pool protsendina. |
| füüsilise\_andmete\_lugeda\_% | Keskmise I/O kasutamine protsentides limiit kogumi alusel. |
| log\_kirjutamine\_% | Keskmine kirjutage ressursi kasutamine limiit pool protsendina. | 
| DTU\_tarbimine\_% | Keskmine eDTU kasutamine protsendina pool eDTU limiit | 
| salvestusruumi\_% | Mäluruumi kasutamine protsendina talletuslimiidi kogumi keskmise. |  
| töötajate\_% | Suurim lubatud samaaegseid töötajate (taotluste) protsentides limiit kogumi alusel. |  
| seansid\_% | Suurim lubatud samaaegseid seansid protsent limiit kogumi alusel. | 
| eDTU_limit | Praeguse max elastne rakenduskausta DTU sätte see elastne pool selle ajavahemiku jooksul. |
| salvestusruumi\_limiit | Praeguse max elastne rakenduskausta talletuslimiidi seadmine see elastne pool megabaitides selles ajavahemikus. |
| eDTU\_kasutatud | Keskmine eDTUs kasutatavaid rakenduskausta sellesse vahemikku. |
| salvestusruumi\_kasutatud | Kasutatav rakenduskausta sellesse vahemikku baitides Keskmine salvestusruum |

**Mõõdikute granulaarsus/säilitus perioodi:**

* 5-minutilise granulaarsus tagastatakse andmed.  
* Andmete säilitamine on 35 päeva.  

Selle cmdlet-käsu ja API piirarvu ridu, mida saab tuua ühe kõne 1000 read (umbes 3 päeva 5 minuti granulaarsus andmete). See käsk saab kutsuda mitu korda koos muu algus ja lõpp ajavahemike rohkem andmeid tuua, kuid 

Toomiseks mõõdikud:

    $metrics = (Get-AzureRmMetric -ResourceId /subscriptions/<subscriptionId>/resourceGroups/FabrikamData01/providers/Microsoft.Sql/servers/fabrikamsqldb02/elasticPools/franchisepool -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime "4/18/2015" -EndTime "4/21/2015")  


## <a name="get-resource-usage-data-for-an-elastic-database"></a>Ressursi kasutamine andmete toomine elastne andmebaasi

Nende API-d on samad, mis praeguse (V12) API kasutatavad ressursi kasutamine autonoomse andmebaasi, välja arvatud järgmised semantilise erinevus. 

See API on protsendina väljendatud mõõdikute tuua funktsiooni max eDTUs (või samaväärne ots jaoks aluseks mõõdiku nagu CPU, IO jne) selle kausta jaoks. Näiteks 50% kasutamise mis tahes nende mõõdikute näitab, et teatud ressursi tarbimine on 50% selle andmebaasi ots limiit selle ressursi ema kogumi kohta. 

Toomiseks mõõdikud:

    $metrics = (Get-AzureRmMetric -ResourceId /subscriptions/<subscriptionId>/resourceGroups/FabrikamData01/providers/Microsoft.Sql/servers/fabrikamsqldb02/databases/myDB -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime "4/18/2015" -EndTime "4/21/2015") 

## <a name="add-an-alert-to-a-pool-resource"></a>Teatise rakenduskausta ressursi lisamine

Saate lisada ressursid saatmine meiliteatised või teatiste stringide [URL-i lõpp-punktid](https://msdn.microsoft.com/library/mt718036.aspx) kui ressursi tabab kasutamine lävi, mille seadistasite teatiste reeglid. Saate kasutada cmdlet-käsu Lisa-AzureRmMetricAlertRule. 

> [AZURE.IMPORTANT]Ressursi kasutamine elastne kaustu jälgimine on viivitusega vähemalt 20 minutit. Vähem kui 30 minuti teatiste seadmine elastne kaustu praegu ei toetata. Mis tahes teatiste seadmine elastne kaustu punktiga (parameeter nimega "-WindowSize" PowerShelli API) vähem kui 30 minuti võib olla käivitatud. Veenduge, et määratlete elastne kaustu teatisi kasutada või enama 30 minuti jooksul (WindowSize).

Selles näites liidetakse teatise saada teada, kui on pool eDTU tarbimine läheb üle teatud piiri.

    # Set up your resource ID configurations
    $subscriptionId = '<Azure subscription id>'      # Azure subscription ID
    $location =  '<location'                         # Azure region
    $resourceGroupName = '<resource group name>'     # Resource Group
    $serverName = '<server name>'                    # server name
    $poolName = '<elastic pool name>'                # pool name 

    #$Target Resource ID
    $ResourceID = '/subscriptions/' + $subscriptionId + '/resourceGroups/' +$resourceGroupName + '/providers/Microsoft.Sql/servers/' + $serverName + '/elasticpools/' + $poolName

    # Create an email action
    $actionEmail = New-AzureRmAlertRuleEmail -SendToServiceOwners -CustomEmail JohnDoe@contoso.com

    # create a unique rule name
    $alertName = $poolName + "- DTU consumption rule"

    # Create an alert rule for DTU_consumption_percent
    Add-AzureRMMetricAlertRule -Name $alertName -Location $location -ResourceGroup $resourceGroupName -TargetResourceId $ResourceID -MetricName "DTU_consumption_percent"  -Operator GreaterThan -Threshold 80 -TimeAggregationOperator Average -WindowSize 00:60:00 -Actions $actionEmail 

## <a name="add-alerts-to-all-databases-in-a-pool"></a>Kõik andmebaasid on teatiste lisamine

Teatiste reeglid saate lisada kõik andmebaasi elastne pargis meiliteatiste saatmiseks või [URL-i lõpp-punktid](https://msdn.microsoft.com/library/mt718036.aspx) kui ressursi teatiste stringide tabab teatise loodud kasutamine piirmäära. 

> [AZURE.IMPORTANT] Ressursi kasutamine elastne kaustu jälgimine on viivitusega vähemalt 20 minutit. Vähem kui 30 minuti teatiste seadmine elastne kaustu praegu ei toetata. Mis tahes teatiste seadmine elastne kaustu punktiga (parameeter nimega "-WindowSize" PowerShelli API) vähem kui 30 minuti võib olla käivitatud. Veenduge, et määratlete elastne kaustu teatisi kasutada või enama 30 minuti jooksul (WindowSize).

Selles näites liidetakse teatise iga andmebaaside jaoks saada teada, kui selle andmebaasi DTU tarbimine läheb üle teatud piiri pargis.

    # Set up your resource ID configurations
    $subscriptionId = '<Azure subscription id>'      # Azure subscription ID
    $location = '<location'                          # Azure region
    $resourceGroupName = '<resource group name>'     # Resource Group
    $serverName = '<server name>'                    # server name
    $poolName = '<elastic pool name>'                # pool name 

    # Get the list of databases in this pool.
    $dbList = Get-AzureRmSqlElasticPoolDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -ElasticPoolName $poolName

    # Create an email action
    $actionEmail = New-AzureRmAlertRuleEmail -SendToServiceOwners -CustomEmail JohnDoe@contoso.com

    # Get resource usage metrics for a database in an elastic database for the specified time interval.
    foreach ($db in $dbList)
    {
    $dbResourceId = '/subscriptions/' + $subscriptionId + '/resourceGroups/' + $resourceGroupName + '/providers/Microsoft.Sql/servers/' + $serverName + '/databases/' + $db.DatabaseName

    # create a unique rule name
    $alertName = $db.DatabaseName + "- DTU consumption rule"

    # Create an alert rule for DTU_consumption_percent
    Add-AzureRMMetricAlertRule -Name $alertName  -Location $location -ResourceGroup $resourceGroupName -TargetResourceId $dbResourceId -MetricName "dtu_consumption_percent"  -Operator GreaterThan -Threshold 80 -TimeAggregationOperator Average -WindowSize 00:60:00 -Actions $actionEmail

    # drop the alert rule
    #Remove-AzureRmAlertRule -ResourceGroup $resourceGroupName -Name $alertName
    } 



## <a name="collect-and-monitor-resource-usage-data-across-multiple-pools-in-a-subscription"></a>Koguda ja jälgida ressursside kasutusandmete üle mitme kaustu on tellimus

Kui teil on suur hulk andmebaaside tellimus, on tülikas kontrollida iga elastne kaust eraldi. Selle asemel SQL-i andmebaasi PowerShelli cmdlet-käskude ja T-SQL-päringud saate ühendada mitu kaustu ja nende andmebaaside jälgida ja analüüsida ressursikasutuse ressursi kasutamine andmete kogumine. [Küsitluse läbiviimine](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools) kogumi PowerShelli skriptide leiate GitHub SQL serveri näidised hoidla dokumente seda, mida ja kuidas seda kasutada koos.

Kasutage seda küsitluse läbiviimine tehke allpool.


1. Laadige alla [skripte ja dokumentatsiooni](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools):
2. Muutke skriptide keskkonnas. Saate määrata ühe või mitme serveri kohta, mis on majutatud elastne kaustu.
3. Määrake telemeetria andmebaasiga, kus asuvad kogutud mõõdikute talletamise. 
4. Kohandada script on skriptide täitmise kestuse määramiseks.

Kõrge, skriptide tehke järgmist.

*   Loetleb kõik serverid antud Azure tellimuse (või määratud nimekirja serverid).
*   Iga server töötab taustal töö. Töö töötab esitatavaks intervalliga ja kogub kõik kaustad server koguda andmeid. Seejärel laadib kogutud andmete määratud telemeetria andmebaasi.
*   Loetleb andmebaasidega igal pool andmebaasi ressursi kasutamine andmete kogumine loendit. Seejärel laadib kogutud andmete telemeetria andmebaasi.

Kogutud mõõdikute telemeetria andmebaasi saab jälgida elastne kaustu ja selle andmebaasid analüüsida. Skript installib ka eelnevalt määratletud tabeli väärtus funktsioon (TVF) telemeetria andmebaasis kokkuvõtte hõlbustamiseks mõõdikute jaoks määratud ajaakna. Näiteks saab kasutada funktsiooni TVF tulemuste kuvamiseks "ülemised N elastne kaustu ajahetkel aknas kasutamisega kuni eDTU." Võite kasutada analüütiline tööriistad, nagu Exceli või Power BI päringu ja kogutud andmete analüüsimiseks.

## <a name="example-retrieve-resource-consumption-metrics-for-a-pool-and-its-databases"></a>Näide: tuua ressursside tarbimine mõõdikute on ka selle andmebaasi jaoks

Selles näites toob tarbimine mõõdikute antud elastne kausta ja kõik selle andmebaasid. Kogutud andmed on vormindatud ja kirjutada vormindatud CSV-faili. Faili saate sirvida Exceliga.

    $subscriptionId = '<Azure subscription id>'       # Azure subscription ID
    $resourceGroupName = '<resource group name>'             # Resource Group
    $serverName = <server name>                              # server name
    $poolName = <elastic pool name>                          # pool name
        
    # Login to Azure account and select the subscription.
    Login-AzureRmAccount
    Set-AzureRmContext -SubscriptionId $subscriptionId
    
    # Get resource usage metrics for an elastic pool for the specified time interval.
    $startTime = '4/27/2016 00:00:00'  # start time in UTC
    $endTime = '4/27/2016 01:00:00'    # end time in UTC
    
    # Construct the pool resource ID and retrive pool metrics at 5 minute granularity.
    $poolResourceId = '/subscriptions/' + $subscriptionId + '/resourceGroups/' + $resourceGroupName + '/providers/Microsoft.Sql/servers/' + $serverName + '/elasticPools/' + $poolName
    $poolMetrics = (Get-AzureRmMetric -ResourceId $poolResourceId -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime $startTime -EndTime $endTime) 
    
    # Get the list of databases in this pool.
    $dbList = Get-AzureRmSqlElasticPoolDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -ElasticPoolName $poolName
    
    # Get resource usage metrics for a database in an elastic database for the specified time interval.
    $dbMetrics = @()
    foreach ($db in $dbList)
    {
        $dbResourceId = '/subscriptions/' + $subscriptionId + '/resourceGroups/' + $resourceGroupName + '/providers/Microsoft.Sql/servers/' + $serverName + '/databases/' + $db.DatabaseName
        $dbMetrics = $dbMetrics + (Get-AzureRmMetric -ResourceId $dbResourceId -TimeGrain ([TimeSpan]::FromMinutes(5)) -StartTime $startTime -EndTime $endTime)
    }
    
    #Optionally you can format the metrics and output as .csv file using the following script block.
    $command = {
    param($metricList, $outputFile)

    # Format metrics into a table.
    $table = @()
    foreach($metric in $metricList) { 
      foreach($metricValue in $metric.MetricValues) {
        $sx = New-Object PSObject -Property @{
            Timestamp = $metricValue.Timestamp.ToString()
            MetricName = $metric.Name; 
            Average = $metricValue.Average;
            ResourceID = $metric.ResourceId 
          }
          $table = $table += $sx
      }
    }
    
    # Output the metrics into a .csv file.
    write-output $table | Export-csv -Path $outputFile -Append -NoTypeInformation
    }
    
    # Format and output pool metrics
    Invoke-Command -ScriptBlock $command -ArgumentList $poolMetrics,c:\temp\poolmetrics.csv
    
    # Format and output database metrics
    Invoke-Command -ScriptBlock $command -ArgumentList $dbMetrics,c:\temp\dbmetrics.csv



## <a name="latency-of-elastic-pool-operations"></a>Latentsus elastne rakenduskausta toimingud

- Min eDTUs kohta andmebaasi või max eDTUs andmebaasi kohta tavaliselt muutmise lõpetab 5 minutit või vähem.
- Muutmise eDTUs kohta rakenduskausta sõltub sellest, kas kõik andmebaasid kogumi kasutatavat ruumi kogusumma. Muudatuste Keskmine 90 minutit iga 100 GB. Näiteks kui kasutatavat ruumi kokku kogumi kõik andmebaasid on 200 GB, siis oodatud latentsus muutmise pool eDTU pool kohta on 1 tund või vähem.

## <a name="migrate-from-v11-to-v12-servers"></a>Migreerimine V11 V12 serverid

PowerShelli cmdlet-käsud on saadaval käivitamiseks, peatamiseks või jälgida versiooniuuenduse Azure SQL-i andmebaasi v12 V11 või mõni muu eel-V12 versioon.

- [Versioonitäienduse SQL-i andmebaasi v12 PowerShelli abil](sql-database-upgrade-server-powershell.md)

Viide dokumendid nende PowerShelli cmdlet-käskude kohta vaadake:


- [Get-AzureRMSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt603582.aspx)
- [Algus-AzureRMSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt619403.aspx)
- [Lõpeta – AzureRMSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt603589.aspx)


Cmdlet Peata tähendab, et tühistada, ei saa peatada. Ei ole nii jätkamiseks versiooniuuenduse uuesti alates algusest peale. Cmdlet Peata migreerimispaketiga ja väljastab kõik asjakohased ressursid.

## <a name="next-steps"></a>Järgmised sammud

- [Loo elastne tööde haldamine](sql-database-elastic-jobs-overview.md) Elastne töö teile skripte T-SQL-i andmebaaside kogumi mõni muu arv peale.
- Vt [mastaapimine välja koos Azure'i SQL-andmebaasi](sql-database-elastic-scale-introduction.md): elastne Andmebaasiriistad abil skaala-out, andmete teisaldamine päringu või luua tehingud.
