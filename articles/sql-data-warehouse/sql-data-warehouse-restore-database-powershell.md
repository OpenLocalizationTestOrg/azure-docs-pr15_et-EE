<properties
   pageTitle="SQL Azure'i andmebaas (PowerShelli) taastamine | Microsoft Azure'i"
   description="PowerShelli tööülesannete taastamiseks Azure SQL-i andmebaas."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="Lakshmi1812"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/21/2016"
   ms.author="lakshmir;barbkess;sonyama"/>

# <a name="restore-an-azure-sql-data-warehouse-powershell"></a>SQL Azure'i andmebaas (PowerShelli) taastamine

> [AZURE.SELECTOR]
- [Ülevaade][]
- [Portaal][]
- [PowerShelli][]
- [ÜLEJÄÄNUD][]

Sellest artiklist saate teada, kuidas taastada Azure SQL-i andmebaas PowerShelli kaudu.

## <a name="before-you-begin"></a>Enne alustamist

**Kontrollige oma DTU võimsus.** SQL server (nt myserver.database.windows.net), mis on vaikimisi DTU kvoodi haldab iga SQL-andmebaas.  Enne taastamist SQL-i andmebaas, kontrollige, kas selle SQL serveris on piisavalt ülejäänud DTU piirmäära taastatakse andmebaas. Saate teada, kuidas arvutada DTU vaja või paluge rohkem DTU, lugege teemat [DTU kvoodi muudatuse taotlemine][].

### <a name="install-powershell"></a>PowerShelli installimine

Azure'i PowerShelli kasutamine SQL-i andmebaas, et peate installida Azure PowerShelli 1.0 või uuemat versiooni.  Saate vaadata oma versiooni, käivitades **Get-moodul - ListAvailable-nime AzureRM**.  Uusima versiooni saate installida [Microsoft Web platvormi Installer][].  Uusima versiooni installimise kohta lisateabe saamiseks vaadake, [Kuidas installida ja konfigureerida Azure PowerShelli][].

## <a name="restore-an-active-or-paused-database"></a>On aktiivne või peatatud andmebaasi taastamine

Taastamiseks kasutada hetktõmmise andmebaasi [Taastamine-AzureRmSqlDatabase][] PowerShelli cmdlet-käsk.

1. Avage Windows PowerShelli.
2. Azure'i kontoga ühenduse loomiseks ja kõik tellimused, mis on teie kontoga seotud loend.
3. Valige tellimus, mis sisaldab andmebaasi taastada.
4. Andmebaasi taastamine punktide loend.
5. Valige soovitud taastamine punkti abil soovitud RestorePointCreationDate.
6. Andmebaasi taastamine soovitud taastamise.
7. Veenduge, et taastatud andmebaasi on võrgus.

```Powershell

$SubscriptionName="<YourSubscriptionName>"
$ResourceGroupName="<YourResourceGroupName>"
$ServerName="<YourServerNameWithoutURLSuffixSeeNote>"  # Without database.windows.net
$DatabaseName="<YourDatabaseName>"
$NewDatabaseName="<YourDatabaseName>"

Login-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName $SubscriptionName

# List the last 10 database restore points
((Get-AzureRMSqlDatabaseRestorePoints -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName ($DatabaseName).RestorePointCreationDate)[-10 .. -1]

# Or list all restore points
Get-AzureRmSqlDatabaseRestorePoints -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Get the specific database to restore
$Database = Get-AzureRmSqlDatabase -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Pick desired restore point using RestorePointCreationDate
$PointInTime="<RestorePointCreationDate>"  

# Restore database from a restore point
$RestoredDatabase = Restore-AzureRmSqlDatabase –FromPointInTimeBackup –PointInTime $PointInTime -ResourceGroupName $Database.ResourceGroupName -ServerName $Database.$ServerName -TargetDatabaseName $NewDatabaseName –ResourceId $Database.ResourceID

# Verify the status of restored database
$RestoredDatabase.status

```

>[AZURE.NOTE] Kui taastamine on lõpule jõudnud, saate konfigureerida, järgmised [konfigureerimine andmebaasi taastamine pärast][]taastatud andmebaasi.


## <a name="restore-a-deleted-database"></a>Kustutatud andmebaasi taastamine

Kustutatud andmebaasi taastamine kasutage [Taastamine-AzureRmSqlDatabase][] cmdlet-käsk.

1. Avage Windows PowerShelli.
2. Azure'i kontoga ühenduse loomiseks ja kõik tellimused, mis on teie kontoga seotud loend.
3. Valige tellimus, mis sisaldab kustutatud andmebaasi taastada.
4. Saada teatud kustutatud andmebaasi.
5. Kustutatud andmebaasi taastada.
6. Veenduge, et taastatud andmebaasi on võrgus.

```Powershell
$SubscriptionName="<YourSubscriptionName>"
$ResourceGroupName="<YourResourceGroupName>"
$ServerName="<YourServerNameWithoutURLSuffixSeeNote>"  # Without database.windows.net
$DatabaseName="<YourDatabaseName>"
$NewDatabaseName="<YourDatabaseName>"

Login-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName $SubscriptionName

# Get the deleted database to restore
$DeletedDatabase = Get-AzureRmSqlDeletedDatabaseBackup -ResourceGroupName $ResourceGroupNam -ServerName $ServerName -DatabaseName $DatabaseName

# Restore deleted database
$RestoredDatabase = Restore-AzureRmSqlDatabase –FromDeletedDatabaseBackup –DeletionDate $DeletedDatabase.DeletionDate -ResourceGroupName $DeletedDatabase.ResourceGroupName -ServerName $DeletedDatabase.ServerName -TargetDatabaseName $NewDatabaseName –ResourceId $DeletedDatabase.ResourceID

# Verify the status of restored database
$RestoredDatabase.status
```

>[AZURE.NOTE] Kui taastamine on lõpule jõudnud, saate konfigureerida, järgmised [konfigureerimine andmebaasi taastamine pärast][]taastatud andmebaasi.


## <a name="restore-from-an-azure-geographical-region"></a>Azure'i geograafiliste piirkonnast taastamine

Andmebaasi taastamiseks kasutada cmdlet [Taastamine – AzureRmSqlDatabase][] .

1. Avage Windows PowerShelli.
2. Azure'i kontoga ühenduse loomiseks ja kõik tellimused, mis on teie kontoga seotud loend.
3. Valige tellimus, mis sisaldab andmebaasi taastada.
4. Pöörduge andmebaasi, mille soovite taastada.
5. Andmebaasi taastamine taotluse loomine
6. Kontrollige geo taastada andmebaasi olekut.

```Powershell
Login-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName "<Subscription_name>"

# Get the database you want to recover
$GeoBackup = Get-AzureRmSqlDatabaseGeoBackup -ResourceGroupName "<YourResourceGroupName>" -ServerName "<YourServerName>" -DatabaseName "<YourDatabaseName>"

# Recover database
$GeoRestoredDatabase = Restore-AzureRmSqlDatabase –FromGeoBackup -ResourceGroupName "<YourResourceGroupName>" -ServerName "<YourTargetServer>" -TargetDatabaseName "<NewDatabaseName>" –ResourceId $GeoBackup.ResourceID

# Verify that the geo-restored database is online
$GeoRestoredDatabase.status
```

>[AZURE.NOTE] Andmebaasi konfigureerimine, kui taastamine on lõpule jõudnud, leiate [oma andmebaasi taastamine pärast konfigureerimine][]. 


Taastatud andmebaasiga, oleks TDE lubatud lähteandmebaasi on TDE lubatud.


## <a name="next-steps"></a>Järgmised sammud
Lisateavet Azure'i SQL-andmebaasi väljaannete äri järjepidevus funktsioonid, lugege [Azure'i SQL-andmebaasi äri järjepidevuse ülevaade][].

<!--Image references-->

<!--Article references-->
[Azure'i SQL-andmebaasi äri järjepidevuse ülevaade]: sql-database-business-continuity.md
[DTU kvoodi muudatuse taotlemine]: ./sql-data-warehouse-get-started-create-support-ticket.md#request-quota-change
[Andmebaasi taastamine pärast konfigureerimine]: ./sql-database-disaster-recovery.md#configure-your-database-after-recovery
[Kuidas installida ja konfigureerida Azure PowerShell]: powershell-install-configure.md
[Ülevaade]: ./sql-data-warehouse-restore-database-overview.md
[Portaal]: ./sql-data-warehouse-restore-database-portal.md
[PowerShelli]: ./sql-data-warehouse-restore-database-powershell.md
[ÜLEJÄÄNUD]: ./sql-data-warehouse-restore-database-rest-api.md
[Andmebaasi taastamine pärast konfigureerimine]: ./sql-database-disaster-recovery.md#configure-your-database-after-recovery

<!--MSDN references-->
[Taastamine-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt693390.aspx

<!--Other Web references-->
[Azure Portal]: https://portal.azure.com/
[Microsoft Web platvormi Installer]: https://aka.ms/webpi-azps
