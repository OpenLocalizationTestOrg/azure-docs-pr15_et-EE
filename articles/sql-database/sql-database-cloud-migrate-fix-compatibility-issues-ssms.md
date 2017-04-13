<properties
   pageTitle="SQL Serveri haldamine Studio abil enne migreerimise SQL-andmebaasiga SQL serveri andmebaasi ühilduvusega seotud probleemid lahendada | Microsoft Azure'i"
   description="Microsoft Azure'i SQL-andmebaasi, andmebaasi migreerimine, ühilduvust, SQL Azure'i migreerimise viisard"
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="sqldb-migrate"
   ms.date="08/24/2016"
   ms.author="carlrab"/>

# <a name="fix-sql-server-database-compatibility-issues-using-sql-server-management-studio-before-migration-to-sql-database"></a>SQL Server Management Studio abil enne migreerimise SQL-andmebaasiga SQL serveri andmebaasi ühilduvusega seotud probleemide lahendamine

> [AZURE.SELECTOR]
- [SQL Azure'i migreerimise viisardi](sql-database-cloud-migrate-fix-compatibility-issues.md) abil
- Kasutage [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
- Kasutage [SSMS](sql-database-cloud-migrate-fix-compatibility-issues-ssms.md)

SQL serveri andmebaasi ühilduvusega seotud probleemid enne migreerimise Azure SQL-i andmebaasi SQL Server Management Studio abil saate lahendada, kogenud kasutajad.


> [AZURE.IMPORTANT] On soovitatav alati kasutada uusimat versiooni Management Studio jäävad sünkroonitud värskendusega ning Microsoft Azure'i SQL-andmebaasi. [SQL Server Management Studio värskendada](https://msdn.microsoft.com/library/mt238290.aspx).


## <a name="using-sql-server-management-studio"></a>SQL Server Management Studio abil

SQL Server Management Studio abil saate mitmesuguste Transact-SQL-i käskude, näiteks **Muuda andmebaasi**kasutamine ühilduvusega seotud probleemid lahendada. Eelkõige mõeldud edasijõudnud kasutajatele, mis on mugav töötab seda meetodit Transact-SQL-i reaalajas andmebaasi. Muul juhul on soovitatav kasutada SSDT. 



## <a name="next-steps"></a>Järgmised sammud

- [SSDT uusim versioon](https://msdn.microsoft.com/library/mt204009.aspx)
- [SQL Server Management Studio uusim versioon](https://msdn.microsoft.com/library/mt238290.aspx)
- [Ühilduvad SQL serveri andmebaasi migreerimine SQL-andmebaasiga](sql-database-cloud-migrate.md#migrate-a-compatible-sql-server-database-to-sql-database)

## <a name="additional-resources"></a>Lisaressursid

- [SQL-i andmebaasi V12](sql-database-v12-whats-new.md)
- [Transact-SQL osaliselt või Toetuseta funktsioonid](sql-database-transact-sql-information.md)
- [SQL serveri migreerimise abimehe abil SQL serveri andmebaasi migreerimine](http://blogs.msdn.com/b/ssma/)
