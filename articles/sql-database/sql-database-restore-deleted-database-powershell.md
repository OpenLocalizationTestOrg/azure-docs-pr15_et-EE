<properties
    pageTitle="Taastada kustutatud Azure SQL-i andmebaasi (PowerShelli) | Microsoft Azure'i"
    description="Taastada kustutatud Azure SQL-i andmebaasi (PowerShelli)."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="10/12/2016"
    ms.author="sstein"
    ms.workload="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="restore-a-deleted-azure-sql-database-by-using-powershell"></a>PowerShelli abil kustutatud Azure SQL-i andmebaasi taastamine

> [AZURE.SELECTOR]
- [Ülevaade](sql-database-recovery-using-backups.md)
- [Taasta kustutatud DB: portaal](sql-database-restore-deleted-database-portal.md)
- [**Taasta kustutatud DB: PowerShelli**](sql-database-restore-deleted-database-powershell.md)

[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]


## <a name="get-a-list-of-deleted-databases"></a>Kustutatud andmebaaside loendi hankimine

```
$resourceGroupName = "resourcegroupname"
$sqlServerName = "servername"

$DeletedDatabases = Get-AzureRmSqlDeletedDatabaseBackup -ResourceGroupName $resourceGroupName -ServerName $sqlServerName
```

## <a name="restore-your-deleted-database-into-a-standalone-database"></a>Kustutatud andmebaasi autonoomse andmebaasi taastamine

Kustutatud andmebaasi varukoopia, mida soovite taastada, kasutades [Get-AzureRmSqlDeletedDatabaseBackup](https://msdn.microsoft.com/library/azure/mt693387(v=azure.300/).aspx) cmdlet-käsk saada. Alustage Taasta kustutatud andmebaasi varukoopia põhjal [Taastamine – AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt693390(v=azure.300/).aspx) cmdleti abil.

```
$resourceGroupName = "resourcegroupname"
$sqlServerName = "servername"
$databaseName = "deletedDbToRestore"

$DeletedDatabase = Get-AzureRmSqlDeletedDatabaseBackup -ResourceGroupName $resourceGroupName -ServerName $sqlServerName -DatabaseName $databaseName

Restore-AzureRmSqlDatabase –FromDeletedDatabaseBackup –DeletionDate $DeletedDatabase.DeletionDate -ResourceGroupName $DeletedDatabase.ResourceGroupName -ServerName $DeletedDatabase.ServerName -TargetDatabaseName "RestoredDatabase" –ResourceId $DeletedDatabase.ResourceID -Edition "Standard" -ServiceObjectiveName "S2"
```


## <a name="restore-your-deleted-database-into-an-elastic-database-pool"></a>Taastada kustutatud andmebaasi kohapeal on elastne andmebaas

Kustutatud andmebaasi varukoopia, mida soovite taastada, kasutades [Get-AzureRmSqlDeletedDatabaseBackup](https://msdn.microsoft.com/library/azure/mt693387(v=azure.300/).aspx) cmdlet-käsk saada. Alustage Taasta kustutatud andmebaasi varukoopia põhjal [Taastamine – AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt693390(v=azure.300/).aspx) cmdleti abil.

```
$resourceGroupName = "resourcegroupname"
$sqlServerName = "servername"
$databaseName = "deletedDbToRestore"

$DeletedDatabase = Get-AzureRmSqlDeletedDatabaseBackup -ResourceGroupName $resourceGroupName -ServerName $sqlServerName -DatabaseName $databaseName

Restore-AzureRmSqlDatabase –FromDeletedDatabaseBackup –DeletionDate $DeletedDatabase.DeletionDate -ResourceGroupName $DeletedDatabase.ResourceGroupName -ServerName $DeletedDatabase.ServerName -TargetDatabaseName "RestoredDatabase" –ResourceId $DeletedDatabase.ResourceID –ElasticPoolName "elasticpool01"
```


## <a name="next-steps"></a>Järgmised sammud

- Äritegevuse järjepidevuse ülevaade ja stsenaariumid, vaadake teemat [ettevõtetele järjepidevuse ülevaade](sql-database-business-continuity.md)
- Azure'i SQL-andmebaasi automaatse varundamise funktsiooni kohta lisateabe saamiseks vt [SQL-andmebaasi automaatse varundamise funktsiooni](sql-database-automated-backups.md)
- Taastamine automaatse varundamise funktsiooni kasutamise kohta leiate teemast [andmebaasi teenuse algatatud varukoopiate põhjal taastamine](sql-database-recovery-using-backups.md)
- Kiirem taaste suvandite kohta leiate teemast [Aktiivne-Geo-Dispersioonanalüüs](sql-database-geo-replication-overview.md)  
- Arhiivimine automaatse varundamise funktsiooni kasutamise kohta lisateabe saamiseks lugege artiklit [andmebaasi kopeerimine](sql-database-copy.md)
