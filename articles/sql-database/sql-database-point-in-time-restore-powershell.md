<properties
    pageTitle="Azure'i SQL-andmebaasi taastamiseks eelmises punktis ajas (PowerShelli) | Microsoft Azure'i"
    description="Eelmises punktis seisu Azure'i SQL-andmebaasi taastamine"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="powershell"
    ms.workload="NA"
    ms.date="07/17/2016"
    ms.author="sstein"/>

# <a name="restore-an-azure-sql-database-to-a-previous-point-in-time-with-powershell"></a>Azure'i SQL-andmebaasi taastamiseks eelmises punktis ajas PowerShelli abil

> [AZURE.SELECTOR]
- [Ülevaade](sql-database-recovery-using-backups.md)
- [Punkti õigel ajal taastamine: Azure'i portaal](sql-database-point-in-time-restore-portal.md)

Selles artiklis kirjeldatakse, kuidas andmebaasi taastamine varasemasse ajas: [SQL-andmebaasi automaatse varundamise funktsiooni](sql-database-automated-backups.md). PowerShelli abil saate seda teha.

[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]

## <a name="restore-your-database-to-a-point-in-time-as-a-standalone-database"></a>Andmebaasi punkti seisu autonoomse andmebaasi taastamine

1. Saada andmebaas, mida soovite taastada, kasutades [Get-AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt603648(v=azure.300\).aspx) cmdlet-käsu.

        $Database = Get-AzureRmSqlDatabase -ResourceGroupName "resourcegroup01" -ServerName "server01" -DatabaseName "database01"

2. Andmebaasi taastamine punkti aega, kasutades [taastamine-AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt693390(v=azure.300\).aspx) cmdlet-käsu.

        Restore-AzureRmSqlDatabase –FromPointInTimeBackup –PointInTime UTCDateTime -ResourceGroupName $Database.ResourceGroupName -ServerName $Database.ServerName -TargetDatabaseName "RestoredDatabase" –ResourceId $Database.ResourceID -Edition "Standard" -ServiceObjectiveName "S2"


## <a name="restore-your-database-to-a-point-in-time-into-an-elastic-database-pool"></a>Punkti ajas kohapeal on elastne andmebaasi andmebaasi taastada

1. Saada andmebaas, mida soovite taastada, kasutades [Get-AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt603648(v=azure.300\).aspx) cmdlet-käsu.

        $Database = Get-AzureRmSqlDatabase -ResourceGroupName "resourcegroup01" -ServerName "server01" -DatabaseName "database01"

2. Andmebaasi taastamine punkti aega, kasutades [taastamine-AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt693390(v=azure.300\).aspx) cmdlet-käsu.

        Restore-AzureRmSqlDatabase –FromPointInTimeBackup –PointInTime UTCDateTime -ResourceGroupName $Database.ResourceGroupName -ServerName $Database.ServerName -TargetDatabaseName "RestoredDatabase" –ResourceId $Database.ResourceID –ElasticPoolName "elasticpool01"


## <a name="next-steps"></a>Järgmised sammud

- Äritegevuse järjepidevuse ülevaade ja stsenaariumid, vaadake teemat [ettevõtetele järjepidevuse ülevaade](sql-database-business-continuity.md)
- Azure'i SQL-andmebaasi automaatse varundamise funktsiooni kohta lisateabe saamiseks vt [SQL-andmebaasi automaatse varundamise funktsiooni](sql-database-automated-backups.md)
- Taastamine automaatse varundamise funktsiooni kasutamise kohta leiate teemast [andmebaasi teenuse algatatud varukoopiate põhjal taastamine](sql-database-recovery-using-backups.md)
- Kiirem taaste suvandite kohta leiate teemast [Aktiivne-Geo-Dispersioonanalüüs](sql-database-geo-replication-overview.md)  
- Arhiivimine automaatse varundamise funktsiooni kasutamise kohta lisateabe saamiseks lugege artiklit [andmebaasi kopeerimine](sql-database-copy.md)
