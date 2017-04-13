<properties 
    pageTitle="Kopeerige PowerShelli kaudu Azure SQL-andmebaasi | Microsoft Azure'i" 
    description="Eksemplari PowerShelli kaudu Azure SQL-andmebaasi loomine" 
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="09/08/2016"
    ms.author="sstein"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="copy-an-azure-sql-database-using-powershell"></a>Kopeerige PowerShelli kaudu Azure SQL-i andmebaas


> [AZURE.SELECTOR]
- [Ülevaade](sql-database-copy.md)
- [Azure'i portaal](sql-database-copy-portal.md)
- [PowerShelli](sql-database-copy-powershell.md)
- [T-SQL-IS](sql-database-copy-transact-sql.md)

Selles artiklis kirjeldatakse, kuidas kopeerida SQL-andmebaasi PowerShelliga sama server, eri server, või kopeerige andmebaasi on [elastne andmebaasi pool](sql-database-elastic-pool.md). Kopeeri andmebaasitoiming kasutab cmdlet-käsu [New-AzureRmSqlDatabaseCopy](https://msdn.microsoft.com/library/mt603644.aspx) . 


Selles artiklis tegemiseks vajate järgmist:

- Azure'i SQL-andmebaasi (andmebaas, et kopeerida). Kui te ei ole SQL-andmebaasi, luua ühe selles artiklis toodud juhiseid järgides: [esimese Azure SQL-i andmebaasi loomine](sql-database-get-started.md).
- Azure'i PowerShelli uusim versioon. Üksikasjalikku teavet, vaadake, [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md).


Palju uusi funktsioone SQL-andmebaasi on toetatud ainult siis, kui kasutate [Azure ressursihaldur juurutamise mudeli](../azure-resource-manager/resource-group-overview.md)nii näited [Azure SQL-i andmebaasi PowerShelli cmdlet-käskude](https://msdn.microsoft.com/library/azure/mt574084.aspx) kasutamine ressursihaldur jaoks. Olemasoleva klassikaline juurutamise mudeli [Azure'i SQL-andmebaasi (klassikaline) cmdlet-käsud](https://msdn.microsoft.com/library/azure/dn546723.aspx) on toetatud tagasiühilduvuse jaoks, kuid soovitame kasutada ressursihaldur cmdlet-käsud.


>[AZURE.NOTE] Olenevalt teie andmebaasi mahtu, kopeerimine võib võtta aega.


## <a name="copy-a-sql-database-to-the-same-server"></a>SQL-andmebaasi kopeerimine sama server

Koopia loomiseks samas serveris argument on `-CopyServerName` parameeter (või määrake selleks sama server).

    New-AzureRmSqlDatabaseCopy -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -CopyDatabaseName "database1_copy"

## <a name="copy-a-sql-database-to-a-different-server"></a>Kopeerige SQL-andmebaasi eri server

Mõnes muus serveris koopia loomiseks kaasata selle `-CopyServerName` parameeter ja määrake selle väärtuseks eri server. *Kopeeri* serveri peab juba olemas. Kui see on erinevate ressursirühm, siis peate lisama ka selle `-CopyResourceGroupName` parameeter.

    New-AzureRmSqlDatabaseCopy -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -CopyServerName "server2" -CopyDatabaseName "database1_copy"


## <a name="copy-a-sql-database-into-an-elastic-database-pool"></a>Kopeerige kohapeal on elastne andmebaasi SQL-andmebaasiga

SQL-andmebaasi koopia loomiseks pargis, määrake soovitud `-ElasticPoolName` mõne olemasoleva kausta-parameeter.

    New-AzureRmSqlDatabaseCopy -ResourceGroupName "resourcegoup1" -ServerName "server1" -DatabaseName "database1" -CopyResourceGroupName "poolResourceGroup" -CopyServerName "poolServer1" -CopyDatabaseName "database1_copy" -ElasticPoolName "poolName"


## <a name="resolve-logins"></a>Sisselogimise lahendamine

Lahendamiseks sisselogimise pärast koopia toiming on lõpule jõudnud, lugege teemat [sisselogimise lahendamine](sql-database-copy-transact-sql.md#resolve-logins-after-the-copy-operation-completes)


## <a name="example-powershell-script"></a>Näide PowerShelli skripti

Järgmise skripti eeldab kõik ressursi rühmad, serverites ja pool juba olemas (muutuvate väärtuste asendamine oma olemasoleva ressurssidega). Kõik peab olemas, välja arvatud andmebaasi koopia.

    # Sign in to Azure and set the subscription to work with
    # ------------------------------------------------------
    $SubscriptionId = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    Add-AzureRmAccount
    Set-AzureRmContext -SubscriptionId $SubscriptionId
    
    
    # SQL database source (the existing database to copy)
    # ---------------------------------------------------
    $sourceDbName = "db1"
    $sourceDbServerName = "server1"
    $sourceDbResourceGroupName = "rg1"
    
    # SQL database copy (the new db to be created)
    # --------------------------------------------
    $copyDbName = "db1_copy"
    $copyDbServerName = "server2"
    $copyDbResourceGroupName = "rg2"
    
    # Copy a database to the same server
    # ----------------------------------
    New-AzureRmSqlDatabaseCopy -ResourceGroupName $sourceDbResourceGroupName -ServerName $sourceDbServerName -DatabaseName $sourceDbName -CopyDatabaseName $copyDbName
    
    # Copy a database to a different server
    # -------------------------------------
    New-AzureRmSqlDatabaseCopy -ResourceGroupName $sourceDbResourceGroupName -ServerName $sourceDbServerName -DatabaseName $sourceDbName -CopyResourceGroupName $copyDbResourceGroupName -CopyServerName $copyDbServerName -CopyDatabaseName $copyDbName
    
    # Copy a database into an elastic database pool
    # ---------------------------------------------
    $poolName = "pool1"
    
    New-AzureRmSqlDatabaseCopy -ResourceGroupName $sourceDbResourceGroupName -ServerName $sourceDbServerName -DatabaseName $sourceDbName -CopyResourceGroupName $copyDbResourceGroupName -CopyServerName $copyDbServerName -ElasticPoolName $poolName -CopyDatabaseName $copyDbName



    

## <a name="next-steps"></a>Järgmised sammud

- Leiate [Azure'i SQL-andmebaasi kopeerimine](sql-database-copy.md) ülevaate kopeerimise Azure'i SQL-andmebaasi.
- Leiate Azure'i portaalis andmebaasi kopeerimiseks [kopeerige Azure'i portaalis Azure SQL-i andmebaas](sql-database-copy-portal.md) .
- Vaadake [Kopeeri Azure'i SQL-andmebaasiga abil T-SQL-i](sql-database-copy-transact-sql.md) abil Transact-SQL-i andmebaasi kopeerimiseks.
- [SQL Azure'i andmebaasi turvasätete avariitaastet pärast](sql-database-geo-replication-security-config.md) andmebaasi kopeerimisel eri loogilise server kasutajate ja sisselogimise haldamise kohta leiate teemast.


## <a name="additional-resources"></a>Lisaressursid

- [Uue AzureRmSqlDatabase](https://msdn.microsoft.com/library/mt603644.aspx)
- [Get-AzureRmSqlDatabaseActivity](https://msdn.microsoft.com/library/mt603687.aspx)
- [Sisselogimise haldamine](sql-database-manage-logins.md)
- [Ühenduse loomine SQL-andmebaasi SQL Server Management Studio ja valimi T-SQL-päringu tegemine](sql-database-connect-query-ssms.md)
- [Andmebaasi eksportimine mõnda BACPAC](sql-database-export.md)
- [Äritegevuse järjepidevuse ülevaade](sql-database-business-continuity.md)
- [SQL-andmebaasi dokumentatsioon](https://azure.microsoft.com/documentation/services/sql-database/)
- [SQL Azure'i andmebaasi PowerShelli cmdleti viide](https://msdn.microsoft.com/library/mt574084.aspx)
