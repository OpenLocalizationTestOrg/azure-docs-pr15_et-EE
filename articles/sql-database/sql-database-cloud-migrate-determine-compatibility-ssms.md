<properties
   pageTitle="SQL Server Management Studio abil saate määratleda SQL-andmebaasi ühilduvust enne migreerimise Azure SQL-andmebaasiga | Microsoft Azure'i"
   description="Microsoft Azure'i SQL-andmebaasi, andmebaasi migreerimine, SQL-andmebaasi ühilduvust, taseme rakenduse ekspordiviisardi andmeid"
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
   ms.date="08/29/2016"
   ms.author="carlrab"/>

# <a name="use-sql-server-management-studio-to-determine-sql-database-compatibility-before-migration-to-azure-sql-database"></a>SQL Server Management Studio abil saate määratleda SQL-andmebaasi ühilduvust enne migreerimise Azure SQL-andmebaasiga

> [AZURE.SELECTOR]
- [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
- [SqlPackage](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md)
- [SSMS](sql-database-cloud-migrate-determine-compatibility-ssms.md)
- [Advisor täiendamine](http://www.microsoft.com/download/details.aspx?id=48119)
- [SAMW](sql-database-cloud-migrate-fix-compatibility-issues.md)
 
Selle artikli teemad, mida saate teada, kuidas kindlaks teha, kui SQL serveri andmebaasi ühildub migreerida SQL-andmebaasiga, SQL Server Management Studio eksportida andmed taseme rakenduse viisardi abil.

## <a name="using-sql-server-management-studio"></a>SQL Server Management Studio abil

1. Veenduge, et teil on SQL Server Management Studio uusima versiooni. Uute versioonide Management Studio on värskendatud kuu jäävad sünkroonis Värskenduste Azure portaali.

     > [AZURE.IMPORTANT] On soovitatav alati kasutada uusimat versiooni Management Studio jäävad sünkroonitud värskendusega ning Microsoft Azure'i SQL-andmebaasi. [SQL Server Management Studio värskendada](https://msdn.microsoft.com/library/mt238290.aspx).

2. Avage Management Studio ja allika objekti Explorer andmebaasiga ühenduse.
3. Paremklõpsake objekti Exploreri lähteandmebaasi, **toimingud**ja klõpsake nuppu **Ekspordi rakendus...**

    ![Klõpsake soovitud rakendus eksportimine](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSSMS01.png)

4. Ekspordiviisard, klõpsake nuppu **edasi**ja konfigureerimist ekspordi kummagi kohalikule kettale asukohta või mõne Azure'i bloobimälu BACPAC faili salvestamiseks klõpsake menüü **sätted** . BACPAC fail on salvestatud, kui teil pole andmebaasi ühilduvusega seotud probleemid. Kui leidub ühilduvusega seotud probleemid, kuvatakse need konsooli.

    ![Ekspordi sätted](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSSMS02.png)

5. Andmete eksportimine, klõpsake **vahekaarti Täpsemalt** ja tühjendage ruut **Vali kõik** . Meie eesmärk on selles etapis ainult ühilduvuse kontrollimiseks.

    ![Ekspordi sätted](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSSMS03.png)

6. Klõpsake nuppu **edasi** ja seejärel klõpsake nuppu **valmis**. Andmebaasi ühilduvusega seotud probleemid, kuvatakse pärast viisardi kinnitatakse skeemiga.

    ![Ekspordi sätted](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSSMS04.png)

7. Kui tõrkeid, andmebaasi ühildub ja olete migreerimiseks valmis. Kui teil on tõrkeid, peate need lahendada. Tõrgete kuvamiseks klõpsake **Validating**skeemi **tõrge** . 
    ![Ekspordi sätted](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSSMS05.png)

8.  Kui soovitud *. BACPAC fail on edukalt loodud, siis andmebaasi ühildub SQL-andmebaasi ja olete migreerimiseks valmis.

## <a name="next-steps"></a>Järgmised sammud

- [SSDT uusim versioon](https://msdn.microsoft.com/library/mt204009.aspx)
- [SQL Server Management Studio uusim versioon](https://msdn.microsoft.com/library/mt238290.aspx)
- [Andmebaasi migreerimine ühilduvusega seotud probleemide lahendamine](sql-database-cloud-migrate.md#fix-database-migration-compatibility-issues)
- [Ühilduvad SQL serveri andmebaasi migreerimine SQL-andmebaasiga](sql-database-cloud-migrate.md#migrate-a-compatible-sql-server-database-to-sql-database)

## <a name="additional-resources"></a>Lisaressursid

- [SQL-i andmebaasi V12](sql-database-v12-whats-new.md)
- [Transact-SQL osaliselt või Toetuseta funktsioonid](sql-database-transact-sql-information.md)
- [SQL serveri migreerimise abimehe abil SQL serveri andmebaasi migreerimine](http://blogs.msdn.com/b/ssma/)
