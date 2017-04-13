<properties
    pageTitle="Luua uue andmebaasi elastne kohapeal on PowerShelliga | Microsoft Azure'i"
    description="Saate teada, kuidas PowerShelli abil skaala-out Azure'i SQL-andmebaasi ressursse, luues scalable elastne andmebaasi rakenduskausta mitme andmebaaside haldamine."
    services="sql-database"
    documentationCenter=""
    authors="srinia"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="powershell"
    ms.workload="data-management"
    ms.date="05/27/2016"
    ms.author="srinia"/>

# <a name="create-a-new-elastic-database-pool-with-powershell"></a>Luua uue andmebaasi elastne rakenduskausta PowerShelli abil

> [AZURE.SELECTOR]
- [Azure'i portaal](sql-database-elastic-pool-create-portal.md)
- [PowerShelli](sql-database-elastic-pool-create-powershell.md)
- [C#:](sql-database-elastic-pool-create-csharp.md)


Saate teada, kuidas luua on [elastne andmebaasi rakenduskausta](sql-database-elastic-pool.md) PowerShelli cmdlet-käskude abil. 

Levinud tõrkekoodid, lugege teemat [SQL-i tõrkekoodid SQL-andmebaasi klientrakendustes: andmebaasi ühenduse tõrge ja ka muude probleemide](sql-database-develop-error-messages.md).

> [AZURE.NOTE] Elastne kaustu kõik Azure piirkondade peale Põhja keskse meile ja Lääne India, kui see on praegu eelvaade on üldiselt kättesaadav (GA).  GA järgmistes regioonides elastne kaustu, pakutakse nii kiiresti kui võimalik. Lisaks elastne kaustu ei toeta andmebaaside [- mälu OLTP või - mälu analytics](sql-database-in-memory.md)abil.


Peate kasutama Azure'i PowerShelli 1.0 või uuem versioon. Üksikasjalikku teavet, vaadake, [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md).

## <a name="create-a-new-pool"></a>Uue kausta loomine

Cmdlet-käsu [New-AzureRmSqlElasticPool](https://msdn.microsoft.com/library/azure/mt619378.aspx) loob uue kausta. Selle väärtused eDTU pool, min ja max Dtus piiravad teenuse taseme väärtus (basic, standard või premium). Vt [eDTU ja salvestusruumi piirab elastne kaustu ja elastne andmebaasid](sql-database-elastic-pool.md#eDTU-and-storage-limits-for-elastic-pools-and-elastic-databases).

    New-AzureRmSqlElasticPool -ResourceGroupName "resourcegroup1" -ServerName "server1" -ElasticPoolName "elasticpool1" -Edition "Standard" -Dtu 400 -DatabaseDtuMin 10 -DatabaseDtuMax 100


## <a name="create-a-new-elastic-database-in-a-pool"></a>Elastne uue andmebaasi loomiseks pargis

Kasutage cmdletti [New-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt619339.aspx) ja seadke **ElasticPoolName** parameetri target ühendada. Olemasoleva andmebaasi viimine on, vaadake [teisaldada andmebaasi elastne kohapeal on sisse](sql-database-elastic-pool-manage-powershell.md#Move-a-database-into-an-elastic-pool).

    New-AzureRmSqlDatabase -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -ElasticPoolName "elasticpool1"

## <a name="create-a-pool-and-populate-it-with-multiple-new-databases"></a>Koostada andmebaas ja asustada mitme uute andmebaaside 

Suure hulga pargis andmebaaside loomine võib võtta aega, kui lõpetanud portaali või korraga ainult ühe andmebaasi loodud PowerShelli cmdlet-käskude abil. Uue kausta sisse loomise automatiseerida, lugege teemat [CreateOrUpdateElasticPoolAndPopulate ](https://gist.github.com/billgib/d80c7687b17355d3c2ec8042323819ae).   

## <a name="example-create-a-pool-using-powershell"></a>Näide: luua kausta, mis on PowerShelli abil 

Skript loob uue ressursirühma Azure ja uus server. Vastava viiba kuvamisel pakkumise uue server (mitte Azure mandaat) on administraatori kasutajanime ja parooliga.

    $subscriptionId = '<your Azure subscription id>'
    $resourceGroupName = '<resource group name>'
    $location = '<datacenter location>'
    $serverName = '<server name>'
    $poolName = '<pool name>'
    $databaseName = '<database name>'

    Login-AzureRmAccount
    Set-AzureRmContext -SubscriptionId $subscriptionId

    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
    New-AzureRmSqlServer -ResourceGroupName $resourceGroupName -ServerName $serverName -Location $location -ServerVersion "12.0"
    New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName -ServerName $serverName -FirewallRuleName "rule1" -StartIpAddress "192.168.0.198" -EndIpAddress "192.168.0.199"

    New-AzureRmSqlElasticPool -ResourceGroupName $resourceGroupName -ServerName $serverName -ElasticPoolName $poolName -Edition "Standard" -Dtu 400 -DatabaseDtuMin 10 -DatabaseDtuMax 100

    New-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName -ElasticPoolName $poolName -MaxSizeBytes 10GB



## <a name="next-steps"></a>Järgmised sammud

- [Teie pool haldamine](sql-database-elastic-pool-manage-powershell.md)
- [Loo elastne tööde haldamine](sql-database-elastic-jobs-overview.md) Elastne töö teile skripte T-SQL-i andmebaaside kogumi mõni muu arv peale.
- [Azure'i SQL-andmebaasi skaala](sql-database-elastic-scale-introduction.md): elastne Andmebaasiriistad skaala-välja kasutamine.

