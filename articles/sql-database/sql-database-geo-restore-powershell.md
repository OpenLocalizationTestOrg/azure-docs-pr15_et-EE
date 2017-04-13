<properties
    pageTitle="Azure'i SQL-andmebaasi taastamine varukoopia geograafilise liigne (PowerShelli) | Microsoft Azure'i"
    description="Azure'i SQL-andmebaasi uus server geograafilise liigne varukoopia põhjal taastamine"
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

# <a name="restore-an-azure-sql-database-from-a-geo-redundant-backup-by-using-powershell"></a>Azure'i SQL-andmebaasi taastamine varukoopia geograafilise liigne PowerShelli abil


> [AZURE.SELECTOR]
- [Ülevaade](sql-database-recovery-using-backups.md)
- [Geo-taastamine: Azure'i portaal](sql-database-geo-restore-portal.md)

Selles artiklis kirjeldatakse, kuidas taastamiseks andmebaasi uus server, kasutades geo-taastamine. Seda saab teha PowerShelli kaudu.

[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]

## <a name="geo-restore-your-database-into-a-standalone-database"></a>Andmebaasi autonoomse andmebaasi Geo-taastamine

1. Saada geograafilise liigne varukoopia andmebaasi, mille soovite taastada, kasutades [Get-AzureRmSqlDatabaseGeoBackup] (https://msdn.microsoft.com/library/azure/mt693388(v=azure.300\).aspx) cmdlet-käsu.

        $GeoBackup = Get-AzureRmSqlDatabaseGeoBackup -ResourceGroupName "resourcegroup01" -ServerName "server01" -DatabaseName "database01"

2. Käivitage geograafilise liigne varukoopia põhjal taastamine [taastamine-AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt693390(v=azure.300\).aspx) cmdlet-käsu.

        Restore-AzureRmSqlDatabase –FromGeoBackup -ResourceGroupName "TargetResourceGroup" -ServerName "TargetServer" -TargetDatabaseName "RestoredDatabase" –ResourceId $GeoBackup.ResourceID -Edition "Standard" -RequestedServiceObjectiveName "S2"


## <a name="geo-restore-your-database-into-an-elastic-database-pool"></a>Teie andmebaasi kohapeal on elastne andmebaasi Geo-taastamine

1. Saada geograafilise liigne varukoopia andmebaasi, mille soovite taastada, kasutades [Get-AzureRmSqlDatabaseGeoBackup] (https://msdn.microsoft.com/library/azure/mt693388(v=azure.300\).aspx) cmdlet-käsu.

        $GeoBackup = Get-AzureRmSqlDatabaseGeoBackup -ResourceGroupName "resourcegroup01" -ServerName "server01" -DatabaseName "database01"

2. Käivitage geograafilise liigne varukoopia põhjal taastamine [taastamine-AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt693390(v=azure.300\).aspx) cmdlet-käsu. Määrake kausta nimi taastatava andmebaasi sisse.

        Restore-AzureRmSqlDatabase –FromGeoBackup -ResourceGroupName "TargetResourceGroup" -ServerName "TargetServer" -TargetDatabaseName "RestoredDatabase" –ResourceId $GeoBackup.ResourceID –ElasticPoolName "elasticpool01"  


## <a name="next-steps"></a>Järgmised sammud

- Äritegevuse järjepidevuse ülevaade ja stsenaariumid, vt [äritegevuse järjepidevuse ülevaade](sql-database-business-continuity.md).
- Azure'i SQL-andmebaasi automaatse varundamise funktsiooni kohta lisateabe saamiseks vt [SQL-andmebaasi automaatse varundamise funktsiooni](sql-database-automated-backups.md).
- Taastamine automaatse varundamise funktsiooni kasutamise kohta leiate teemast [andmebaasi teenuse algatatud varukoopiate põhjal taastamine](sql-database-recovery-using-backups.md).
- Kiirem taaste suvandite kohta leiate teemast [Aktiivne-Geo-Dispersioonanalüüs](sql-database-geo-replication-overview.md).  
- Arhiivimine automaatse varundamise funktsiooni kasutamise kohta lisateabe saamiseks lugege artiklit [andmebaasi kopeerimine](sql-database-copy.md).
