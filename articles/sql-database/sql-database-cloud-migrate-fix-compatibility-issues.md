<properties
   pageTitle="SQL serveri andmebaasi ühilduvusega seotud probleemid enne migreerimise SQL-andmebaasiga parandamine | Microsoft Azure'i"
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

# <a name="use-sql-azure-migration-wizard-to-fix-sql-server-database-compatibility-issues-before-migration-to-azure-sql-database"></a>SQL Azure'i migreerimise viisardi abil saate lahendada SQL serveri andmebaasi ühilduvusega seotud probleemid enne migreerimise Azure SQL-andmebaasiga

> [AZURE.SELECTOR]
- [SQL Azure'i migreerimise viisardi](sql-database-cloud-migrate-fix-compatibility-issues.md) abil
- Kasutage [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
- Kasutage [SSMS](sql-database-cloud-migrate-fix-compatibility-issues-ssms.md)

Selles artiklis saate teada, avastada ja parandada SQL serveri andmebaasi ühilduvusega seotud probleemid enne migreerimise Azure SQL-andmebaasiga SQL Azure'i migreerimise viisardi abil.

## <a name="using-sql-azure-migration-wizard"></a>SQL Azure'i migreerimise viisardi abil

[SQL Azure'i migreerimise viisardi](http://sqlazuremw.codeplex.com/) CodePlex tööriista abil saate luua skripti T-SQL-i andmebaasist mitteühilduvad allikas. Seejärel muudetakse see skript teha seda SQL-andmebaasi ühildub viisardi. Seejärel ühendada Azure'i SQL-andmebaasi skripti käivitada. See tööriist analüüsib ka Jälgi faili määratlemiseks ühilduvusega seotud probleemid. Skripti saab luua ainult skeemi või võivad hõlmata andmeid BCP vormingus. Täiendavad dokumentatsiooni kohta, sh üksikasjalikud juhised on saadaval veebisaidil [SQL Azure'i migreerimise viisardi](http://sqlazuremw.codeplex.com/)codeplexis.  

 ![SAMW migreerimise skeem](./media/sql-database-cloud-migrate/02SAMWDiagram.png)

  > [AZURE.NOTE] Kõik mitteühilduvad skeemi tuvastatakse viisardi saate kindlaks selle sisseehitatud teisendused. Mitteühilduvad skripti, mida ei saa on teatatud tõrgete, kommentaaridega juhitakse loodud skript. Kui palju vigu, kasutage Visual Studio või SQL Server Management Studio läbi ja määrata iga vea, mis võib pole määratud SQL serveri migreerimise viisardi abil.

## <a name="next-steps"></a>Järgmised sammud

- [SSDT uusim versioon](https://msdn.microsoft.com/library/mt204009.aspx)
- [SQL Server Management Studio uusim versioon](https://msdn.microsoft.com/library/mt238290.aspx)
- [Ühilduvad SQL serveri andmebaasi migreerimine SQL-andmebaasiga](sql-database-cloud-migrate.md#migrate-a-compatible-sql-server-database-to-sql-database)

## <a name="additional-resources"></a>Lisaressursid

- [SQL-i andmebaasi V12](sql-database-v12-whats-new.md)
- [Transact-SQL osaliselt või Toetuseta funktsioonid](sql-database-transact-sql-information.md)
- [SQL serveri migreerimise abimehe abil SQL serveri andmebaasi migreerimine](http://blogs.msdn.com/b/ssma/)
