<properties
   pageTitle="SQL-andmebaasiga abil SqlPackage BACPAC faili importimine"
   description="Microsoft Azure'i SQL-andmebaasi, andmebaasi migreerimine, andmebaasi importida, sqlpackage BACPAC faili importimine"
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

# <a name="import-to-sql-database-from-a-bacpac-file-using-sqlpackage"></a>SQL-andmebaasiga abil SqlPackage BACPAC faili importimine

> [AZURE.SELECTOR]
- [SSMS](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [SqlPackage](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md)
- [Azure'i portaal](sql-database-import.md)
- [PowerShelli](sql-database-import-powershell.md)

Selles artiklis kirjeldatakse, kuidas importida SQL-andmebaasiga [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) faili kasutades [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) käsurea kasuliku. [SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) ja [SQL Server Data Tools for Visual Studio](https://msdn.microsoft.com/library/mt204009.aspx)uusimad versioonid koos teenusega selle kasuliku või [SqlPackage](https://www.microsoft.com/en-us/download/details.aspx?id=53876) uusima versiooni saate otse Microsofti allalaadimiskeskusest alla laadida.


> [AZURE.NOTE] Järgmiste juhiste korral eeldatakse on juba ette valmistatud SQL-andmebaasi server, käepärast ühenduse teavet ja on kinnitatud teie lähteandmebaasi ühildub.

## <a name="import-from-a-bacpac-file-into-azure-sql-database-using-sqlpackage"></a>Azure'i SQL-andmebaasi kasutamise SqlPackage BACPAC faili importimine

Kasutada järgmist [SqlPackage.exe](https://msdn.microsoft.com/library/hh550080.aspx) käsurea kasuliku ühilduvad SQL Serveri andmebaasiga (või SQL Azure'i andmebaasi) importimiseks BACPAC faili.

> [AZURE.NOTE] Järgmiste juhiste korral eeldatakse, et teil on juba ette valmistatud Azure SQL-i andmebaasiserveriga ja ühendusteabe käepärast.

1. Avage käsuviip ja muuta kataloogi sisaldava sqlpackage.exe käsurea kasuliku – see kasuliku teenusega nii Visual Studio ja SQL serveri.
2. Käsu järgmised sqlpackage.exe koos keskkonna järgmised argumendid:

    `sqlpackage.exe /Action:Import /tsn:< server_name > /tdn:< database_name > /tu:< user_name > /tp:< password > /sf:< source_file >`

  	| Argument  | Kirjeldus  |
  	|---|---|
  	| < serveri_nimi >  | Target (sihtkoht) serveri nimi  |
  	| < database_name >  | Target (sihtkoht) andmebaasi nimi  |
  	| < user_name >  | kasutaja nimi ja sihtkoht server |
  	| < parooli >  | kasutaja parooli  |
  	| < source_file >  | faili nimi ja asukoht imporditaval failil BACPAC  |

    ![Klõpsake soovitud rakendus eksportimine](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSQLPackage01c.png)

## <a name="next-steps"></a>Järgmised sammud

- [SSDT uusim versioon](https://msdn.microsoft.com/library/mt204009.aspx)
- [SQL Server Management Studio uusim versioon](https://msdn.microsoft.com/library/mt238290.aspx)

## <a name="additional-resources"></a>Lisaressursid

- [SQL-i andmebaasi V12](sql-database-v12-whats-new.md)
- [Transact-SQL osaliselt või Toetuseta funktsioonid](sql-database-transact-sql-information.md)
- [SQL serveri migreerimise abimehe abil SQL serveri andmebaasi migreerimine](http://blogs.msdn.com/b/ssma/)
