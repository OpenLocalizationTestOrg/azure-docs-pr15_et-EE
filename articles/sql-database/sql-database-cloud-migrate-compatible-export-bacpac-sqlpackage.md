<properties
   pageTitle="SQL serveri andmebaasi eksportida BACPAC faili SqlPackage abil | Microsoft Azure'i"
   description="Microsoft Azure'i SQL-andmebaasi, andmebaasi migreerimine, andmebaasi eksportida, eksportida BACPAC faili sqlpackage"
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

# <a name="export-a-sql-server-database-to-a-bacpac-file-using-sqlpackage"></a>SQL serveri andmebaasi eksportida BACPAC faili SqlPackage abil

> [AZURE.SELECTOR]
- [SSMS](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
- [SqlPackage](sql-database-cloud-migrate-compatible-export-bacpac-sqlpackage.md)

Selles artiklis kirjeldatakse SQL serveri andmebaasi eksportimisest [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) faili [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) käsurea kasuliku abil. [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) ja [SQL Server Data Tools for Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx)uusimad versioonid koos teenusega selle kasuliku või [SqlPackage](https://www.microsoft.com/en-us/download/details.aspx?id=53876) uusima versiooni saate otse Microsofti allalaadimiskeskusest alla laadida.

1. Avage käsuviip ja muuta kataloogi sisaldava sqlpackage.exe käsurea kasuliku – see kasuliku teenusega nii Visual Studio ja SQL serveri. Arvuti otsingufunktsiooni abil otsida tee teie keskkonnas.
2. Käsu järgmised sqlpackage.exe koos keskkonna järgmised argumendid:

    "sqlpackage.exe /Action:Export /ssn: < serveri_nimi > /sdn: < database_name > /tf: < target_file >

  	| Argument  | Kirjeldus  |
  	|---|---|
  	| < serveri_nimi >  | allika serveri nimi  |
  	| < database_name >  | allika andmebaasi nimi  |
  	| < target_file >  | faili nimi ja asukoht BACPAC faili  |

    ![Klõpsake soovitud rakendus eksportimine](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSQLPackage01b.png)

## <a name="next-steps"></a>Järgmised sammud

- [SSDT uusim versioon](https://msdn.microsoft.com/library/mt204009.aspx)
- [SQL Server Management Studio uusim versioon](https://msdn.microsoft.com/library/mt238290.aspx)
- [Importimine on BACPAC Azure'i SQL-andmebaasi SSMS abil](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [SQL Azure'i andmebaasi SqlPackage on BACPAC importimine](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md)
- [Importimine on BACPAC Azure SQL-i andmebaasi Azure'i portaal](sql-database-import.md)
- [SQL Azure'i andmebaasi PowerShelli on BACPAC importimine](sql-database-import-powershell.md)

## <a name="additional-resources"></a>Lisaressursid

- [SQL-i andmebaasi V12](sql-database-v12-whats-new.md)
- [Transact-SQL osaliselt või Toetuseta funktsioonid](sql-database-transact-sql-information.md)
- [SQL serveri migreerimise abimehe abil SQL serveri andmebaasi migreerimine](http://blogs.msdn.com/b/ssma/)
