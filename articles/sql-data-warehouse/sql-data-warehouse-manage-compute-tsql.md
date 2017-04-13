<properties
   pageTitle="Arvuta power rakenduses Azure SQL-i andmete ladu (REST) haldamine | Microsoft Azure'i"
   description="Skaala-out jõudluse, reguleerides DWUs tööülesandeid Transact-SQL-i (T-SQL-i). Kulude kokkuhoidmiseks skaleerimist tagasi-tippväärtus ajal."
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
   ms.date="08/08/2016"
   ms.author="barbkess;sonyama"/>

# <a name="manage-compute-power-in-azure-sql-data-warehouse-t-sql"></a>Arvuta power (T-SQL-i) SQL Azure'i andmebaas haldamine

> [AZURE.SELECTOR]
- [Ülevaade](sql-data-warehouse-manage-compute-overview.md)
- [Portaal](sql-data-warehouse-manage-compute-portal.md)
- [PowerShelli](sql-data-warehouse-manage-compute-powershell.md)
- [ÜLEJÄÄNUD](sql-data-warehouse-manage-compute-rest-api.md)
- [TSQL](sql-data-warehouse-manage-compute-tsql.md)


Skaala jõudluse skaala läbi arvutada materjale ja mälu muutmisega nõudmistele oma töökoormus. Salvestada kulud skaleerimise tagasi ressursid-tippväärtus korda või otsingurakenduse Arvuta ajal täielikult eemaldada. 

Selle saidikogumi tööülesannete kasutab T-SQL-i abil.

- Vaate praeguse DWU sätted
- Muuda arvutada, reguleerides DWUs ressursid

Peatamiseks või jätkamiseks andmebaasi, valige üks selles artiklis ülaosas platvormi muid võimalusi.

Teavet selle kohta leiate teemast [haldamine arvutada power ülevaade][].

<a name="current-dwu-bk"></a>

## <a name="view-current-dwu-settings"></a>Vaate praeguse DWU sätted

Andmebaaside DWU praeguste sätete vaatamiseks tehke järgmist.

1. Avage SQL serveri objekti Explorer Visual Studio 2015.
2. Andmebaasiga ühenduse loomiseks juhtslaidi seotud loogika SQL-andmebaasi server.
2. Valige sys.database_service_objectives dünaamilise halduse vaates. Siin on näide: 

```
SELECT
 db.name [Database],
 ds.edition [Edition],
 ds.service_objective [Service Objective]
FROM
 sys.database_service_objectives ds
 JOIN sys.databases db ON ds.database_id = db.database_id
```

<a name="scale-dwu-bk"></a>
<a name="scale-compute-bk"></a>

## <a name="scale-compute"></a>Arvuta skaala

[AZURE.INCLUDE [SQL Data Warehouse scale DWUs description](../../includes/sql-data-warehouse-scale-dwus-description.md)]

Funktsiooni DWUs muutmiseks tehke järgmist.


1. Andmebaasiga ühenduse loomiseks juhtslaidi seostatud teie loogika SQL-andmebaasi server.
2. Kasutage [Andmebaasi muutmine][] TSQL lause. Järgmises näites määrab selle teenuse DW1000 andmebaasi MySQLDW. 

```Sql
ALTER DATABASE MySQLDW
MODIFY (SERVICE_OBJECTIVE = 'DW1000')
;
```

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Järgmised sammud

Muude haldustoimingute teemast [halduse ülevaade][].

<!--Image references-->

<!--Article references-->
[Service capacity limits]: ./sql-data-warehouse-service-capacity-limits.md
[Juhtimise ülevaade]: ./sql-data-warehouse-overview-manage.md
[Arvuta power ülevaade haldamine]: ./sql-data-warehouse-manage-compute-overview.md

<!--MSDN references-->

[MUUDA ANDMEBAASI]: https://msdn.microsoft.com/library/mt204042.aspx


<!--Other Web references-->

[Azure portal]: http://portal.azure.com/
