<properties
   pageTitle="SQL Azure'i andmebaas PowerShelli cmdlet-käsud"
   description="Azure SQL-i andmebaas, sh kuidas peatada ja jätkata andmebaasi ülemise PowerShelli cmdlet-käskude otsimine."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/16/2016"
   ms.author="sonyama;barbkess;mausher"/>

# <a name="powershell-cmdlets-and-rest-apis-for-sql-data-warehouse"></a>PowerShelli cmdlet-käskude ja REST API-de jaoks SQL-andmebaas

Mitme SQL-i andmebaas haldustoimingute saab hallata Azure PowerShelli cmdlet-käskude või REST API-d.  Allpool on mõned näited PowerShelli käskude abil automatiseerida oma SQL-i andmebaas tavalised toimingud.  Hea ülejäänud näited leiate artiklist [ülejäänud haldamine skaleeritavus][].

> [AZURE.NOTE]  PowerShelli Azure SQL-i andmebaas kasutamiseks peate Azure PowerShelli versioon 1.0.3 või suurem.  Saate vaadata oma versiooni, käivitades **Get-moodul - ListAvailable-nime Azure'i**.  Uusima versiooni saate installida [Microsoft Web platvormi Installer][].  Uusima versiooni installimise kohta lisateabe saamiseks vaadake, [Kuidas installida ja konfigureerida Azure PowerShelli][].

## <a name="get-started-with-azure-powershell-cmdlets"></a>Azure'i PowerShelli cmdlet-käskude kasutamise alustamine

1. Avage Windows PowerShelli. 
2. Käivitage PowerShelli käsuviiba need käsud Azure'i ressursihaldur sisselogimine ja valige oma tellimuse.

    ```PowerShell
    Login-AzureRmAccount
    Get-AzureRmSubscription
    Select-AzureRmSubscription -SubscriptionName "MySubscription"
    ```

## <a name="pause-sql-data-warehouse-example"></a>Paus SQL-i andmete ladu näide

Viige andmebaasi nimega "Database02" majutatud serveris nimega "Server01."  Server on Azure ressursirühm, nimega "ResourceGroup1." 

```Powershell
Suspend-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
```
Mõne muudatuse selles näites torud [Peata-AzureRmSqlDatabase][]välja objekti.  Selle tulemusena andmebaas on peatatud. Lõplik käsk näitab tulemusi.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Suspend-AzureRmSqlDatabase
$resultDatabase
```

## <a name="start-sql-data-warehouse-example"></a>Käivitage SQL-i andmete ladu näide

Elulookirjelduse toimimise andmebaasi nimega "Database02" majutatud serveris nimega "Server01." Server on toodud ressursirühma nimega "ResourceGroup1."

```Powershell
Resume-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" -DatabaseName "Database02"
```

Mõne muudatuse selles näites toob andmebaasi nimega "Database02" nimega "Server01" nimega "ResourceGroup1." ressursirühma sisalduv serverist See torud [CV-AzureRmSqlDatabase][]välja objekti.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Resume-AzureRmSqlDatabase
```

> [AZURE.NOTE] Pange tähele, et kui teie server on foo.database.windows.net, kasutage PowerShelli cmdlet-käskude serverinimi-"foo".

## <a name="frequently-used-powershell-cmdlets"></a>Sageli kasutatavate PowerShelli cmdlet-käsud

SQL Azure'i andmebaas kasutatakse sageli nende PowerShelli cmdlet-käsud.

- [Get-AzureRmSqlDatabase][]
- [Get-AzureRmSqlDeletedDatabaseBackup][]
- [Get-AzureRmSqlDatabaseRestorePoints][]
- [Uue AzureRmSqlDatabase][]
- [Eemalda – AzureRmSqlDatabase][]
- [Taastamine-AzureRmSqlDatabase][] 
- [CV-AzureRmSqlDatabase][]
- [Valige-AzureRmSubscription][]
- [Set-AzureRmSqlDatabase][]
- [Peatada AzureRmSqlDatabase][]

## <a name="next-steps"></a>Järgmised sammud
Rohkem näiteid PowerShelli järgmistest teemadest.

- [Looge PowerShelli kaudu SQL-i andmebaas][]
- [Andmebaasi taastamine][]

Kõik toimingud, mis saab automaatselt PowerShelliga loendi leiate teemast [Azure SQL-i andmebaasi cmdlet-käsud][].  Tööülesannete loend, mis automatiseerida ülejäänud leiate teemast [Azure SQL-i andmebaaside jaoks toimingud][].

<!--Image references-->

<!--Article references-->
[Kuidas installida ja konfigureerida Azure PowerShell]: ./powershell-install-configure.md
[Looge PowerShelli kaudu SQL-i andmebaas]: ./sql-data-warehouse-get-started-provision-powershell.md
[Andmebaasi taastamine]: ./sql-data-warehouse-restore-database-powershell.md
[Ülejäänud skaleeritavus haldamine]: ./sql-data-warehouse-manage-compute-rest-api.md

<!--MSDN references-->
[SQL Azure'i andmebaasi cmdlet-käsud]: https://msdn.microsoft.com/library/mt574084.aspx
[Toimingute jaoks SQL Azure'i andmebaasid]: https://msdn.microsoft.com/library/azure/dn505719.aspx
[Get-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt603648.aspx
[Get-AzureRmSqlDeletedDatabaseBackup]: https://msdn.microsoft.com/library/mt693387.aspx
[Get-AzureRmSqlDatabaseRestorePoints]: https://msdn.microsoft.com/library/mt603642.aspx
[Uue AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619339.aspx
[Eemalda – AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619368.aspx
[Taastamine-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt693390.aspx
[CV-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619347.aspx
<!-- It appears that Select-AzureRmSubscription isn't documented, so this points to Select-AzureSubscription -->
[Valige-AzureRmSubscription]: https://msdn.microsoft.com/library/dn722499.aspx
[Set-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619433.aspx
[Peatada AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619337.aspx

<!--Other Web references-->
[Microsoft Web platvormi Installer]: https://aka.ms/webpi-azps
