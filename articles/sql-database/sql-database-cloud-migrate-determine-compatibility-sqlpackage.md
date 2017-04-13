<properties
   pageTitle="SQL-andmebaasi ühilduvuse SqlPackage.exe abil määrata | Microsoft Azure'i"
   description="Microsoft Azure'i SQL-andmebaasi, andmebaasi migreerimine, SQL-andmebaasi ühilduvust, SqlPackage"
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

# <a name="determine-sql-database-compatibility-using-sqlpackageexe"></a>SQL-andmebaasi ühilduvuse abil SqlPackage.exe määratlemine

> [AZURE.SELECTOR]
- [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
- [SqlPackage](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md)
- [SSMS](sql-database-cloud-migrate-determine-compatibility-ssms.md)
- [Advisor täiendamine](http://www.microsoft.com/download/details.aspx?id=48119)
- [SAMW](sql-database-cloud-migrate-fix-compatibility-issues.md)

Sellest artiklist saate teada SQL serveri andmebaasi migreerimine SQL-andmebaasiga, kasutades [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) käsuviiba kasuliku ühilduvuse kindlaksmääramine.

## <a name="using-sqlpackageexe"></a>SqlPackage.exe abil

1. Avage käsuviip ja muuta kataloogi sisaldava sqlpackage.exe uusim versioon. [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) ja [SQL Server Data Tools for Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx)uusimad versioonid koos teenusega selle kasuliku või [SqlPackage](https://www.microsoft.com/en-us/download/details.aspx?id=53876) uusima versiooni saate otse Microsofti allalaadimiskeskusest alla laadida.
2. Käsu järgmised SqlPackage koos keskkonna järgmised argumendid:

    "sqlpackage.exe /Action:Export /ssn: < serveri_nimi > /sdn: < database_name > /tf: < target_file > /p:TableData = < schema_name.table_name >>< output_file > 2 > & 1

  	| Argument  | Kirjeldus  |
  	|---|---|
  	| < serveri_nimi >  | allika serveri nimi  |
  	| < database_name >  | allika andmebaasi nimi  |
  	| < target_file >  | faili nimi ja asukoht BACPAC faili  |
  	| < schema_name.table_name >  | tabelid, mille andmed on sihtfail väljund  |
  	| < output_file >  | faili nimi ja asukoht vigadega, kui mõni väljundfail  |

    /P:TableName argument põhjus on selles, et soovime ainult testimiseks andmebaasi ühilduvuse eksportimiseks Azure SQL-i DB V12 asemel andmete eksportimine kõik tabelid. Kahjuks ei toeta sqlpackage.exe ekspordi argument, ekstraktimiseks null tabelid. Peate määrama vähemalt üks tabel, nt väike üheks tabeliks. < Output_file > sisaldab vigu aruanne. Funktsiooni "> 2 > & 1" stringi torud väljundit nii väljundfail määratud käsu täitmine tulenevad standardviga.

    ![Klõpsake soovitud rakendus eksportimine](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSQLPackage01.png)

3. Avage väljundfail ja ühilduvus tõrked, kui mõni üle vaadata. 

    ![Klõpsake soovitud rakendus eksportimine](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSQLPackage02.png)

## <a name="next-steps"></a>Järgmised sammud

- [Uusim versioon SSDT](https://msdn.microsoft.com/library/mt204009.aspx)
[SQL Server Management Studio uusim versioon](https://msdn.microsoft.com/library/mt238290.aspx)
- [Andmebaasi migreerimine ühilduvusega seotud probleemide lahendamine](sql-database-cloud-migrate.md#fix-database-migration-compatibility-issues)
- [Ühilduvad SQL serveri andmebaasi migreerimine SQL-andmebaasiga](sql-database-cloud-migrate.md#migrate-a-compatible-sql-server-database-to-sql-database)

## <a name="additional-resources"></a>Lisaressursid

- [SQL-i andmebaasi V12](sql-database-v12-whats-new.md)
- [Transact-SQL osaliselt või Toetuseta funktsioonid](sql-database-transact-sql-information.md)
- [SQL serveri migreerimise abimehe abil SQL serveri andmebaasi migreerimine](http://blogs.msdn.com/b/ssma/)
