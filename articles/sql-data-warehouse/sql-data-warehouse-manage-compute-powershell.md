<properties
   pageTitle="Arvuta power (PowerShelli) SQL Azure'i andmebaas haldamine | Microsoft Azure'i"
   description="PowerShelli ülesannete haldamiseks Arvutage power. Skaala Arvuta ressursid, reguleerides DWUs. Või Peata ja Jätka arvutada ressursside kulude salvestamiseks."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/13/2016"
   ms.author="barbkess;sonyama"/>

# <a name="manage-compute-power-in-azure-sql-data-warehouse-powershell"></a>Arvuta power (PowerShelli) SQL Azure'i andmebaas haldamine

> [AZURE.SELECTOR]
- [Ülevaade](sql-data-warehouse-manage-compute-overview.md)
- [Portaal](sql-data-warehouse-manage-compute-portal.md)
- [PowerShelli](sql-data-warehouse-manage-compute-powershell.md)
- [ÜLEJÄÄNUD](sql-data-warehouse-manage-compute-rest-api.md)
- [TSQL](sql-data-warehouse-manage-compute-tsql.md)


Skaala jõudluse skaala läbi arvutada materjale ja mälu muutmisega nõudmistele oma töökoormus. Salvestada kulud skaleerimise tagasi ressursid-tippväärtus korda või otsingurakenduse Arvuta ajal täielikult eemaldada. 

Selle saidikogumi tööülesannete kasutab Azure'i portaalis.

- Arvuta skaala
- Paus Arvuta
- Elulookirjelduse Arvuta

Teavet selle kohta leiate teemast [haldamine arvutada ülevaade][].


## <a name="before-you-begin"></a>Enne alustamist

### <a name="install-the-latest-version-of-azure-powershell"></a>Installige uusim versioon Azure PowerShell

> [AZURE.NOTE]  PowerShelli Azure SQL-i andmebaas kasutamiseks on vaja Azure PowerShelli versioon 1.0.3 või suurem.  Oma praeguse versiooni, käivitage käsk kinnitamiseks **Get-moodul - ListAvailable-nime Azure'i**. Saate installida [Microsoft Web platvormi Installeri][]uusim versioon.  Lisateabe saamiseks vaadake, [Kuidas installida ja konfigureerida Azure PowerShelli][].

### <a name="get-started-with-azure-powershell-cmdlets"></a>Azure'i PowerShelli cmdlet-käskude kasutamise alustamine

Alustamiseks:

1. Avage Azure'i PowerShelli. 
2. Käivitage PowerShelli käsuviiba need käsud Azure'i ressursihaldur sisselogimine ja valige oma tellimuse.

    ```PowerShell
    Login-AzureRmAccount
    Get-AzureRmSubscription
    Select-AzureRmSubscription -SubscriptionName "MySubscription"
    ```

<a name="scale-performance-bk"></a>
<a name="scale-compute-bk"></a>

## <a name="scale-compute-power"></a>Skaala Arvuta power

[AZURE.INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

Saate muuta selle DWUs [Set-AzureRmSqlDatabase][] PowerShelli cmdlet-käsk. Järgmises näites määrab selle teenuse DW1000 andmebaasi MySQLDW, mis on majutatud serveris minuserver. 

```Powershell
Set-AzureRmSqlDatabase -DatabaseName "MySQLDW" -ServerName "MyServer" -RequestedServiceObjectiveName "DW1000"
```

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a>Paus Arvuta

[AZURE.INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

Kasutage andmebaasi peatamiseks [Peata-AzureRmSqlDatabase][] cmdlet-käsk. Järgmises näites peatab Database02 majutatud serveris nimega Server01 andmebaas. Server on Azure ressursirühm, nimega ResourceGroup1. 

> [AZURE.NOTE] Pange tähele, et kui teie server on foo.database.windows.net, kasutage PowerShelli cmdlet-käskude serverinimi-"foo".

```Powershell
Suspend-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" –DatabaseName "Database02"
```
Mõne muudatuse järgmise näites toob andmebaasi $database objekti. See siis torud [Peata-AzureRmSqlDatabase][]objekti. Objekti resultDatabase on talletatud tulemused. Lõplik käsk näitab tulemusi.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Suspend-AzureRmSqlDatabase
$resultDatabase
```

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a>Elulookirjelduse Arvuta

[AZURE.INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]

Andmebaasi alustamiseks cmdletiga [CV-AzureRmSqlDatabase][] . Järgmises näites alustab Database02 majutatud serveris nimega Server01 andmebaas. Server on Azure ressursirühm, nimega ResourceGroup1. 

```Powershell
Resume-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" -DatabaseName "Database02"
```

Mõne muudatuse järgmise näites toob andmebaasi $database objekti. Seejärel torud [CV-AzureRmSqlDatabase][] objekti ja talletab $resultDatabase tulemused. Lõplik käsk näitab tulemusi.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" `
–ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Resume-AzureRmSqlDatabase
$resultDatabase
```

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Järgmised sammud

Muude haldustoimingute teemast [halduse ülevaade][].

<!--Image references-->

<!--Article references-->
[Service capacity limits]: ./sql-data-warehouse-service-capacity-limits.md
[Juhtimise ülevaade]: ./sql-data-warehouse-overview-manage.md
[Kuidas installida ja konfigureerida Azure PowerShell]: ./powershell-install-configure.md
[Arvuta ülevaade haldamine]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->
[CV-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619347.aspx
[Peatada AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619337.aspx
[Set-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619433.aspx

<!--Other Web references-->
[Microsoft Web platvormi Installer]: https://aka.ms/webpi-azps
[Azure portal]: http://portal.azure.com/
