
<properties
   pageTitle="SQL serveri andmebaasi eksportida BACPAC faili SQL Server Management Studio abil | Microsoft Azure'i"
   description="Microsoft Azure'i SQL-andmebaasi, andmebaasi migreerimine, andmebaasi eksportida, eksportida BACPAC faili eksportida andmed rakendus viisard"
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
   ms.workload="data-management"
   ms.date="08/16/2016"
   ms.author="carlrab"/>

# <a name="export-a-sql-server-database-to-a-bacpac-file-using-sql-server-management-studio"></a>SQL serveri andmebaasi eksportida BACPAC faili SQL Server Management Studio abil

> [AZURE.SELECTOR]
- [SSMS](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
- [SqlPackage](sql-database-cloud-migrate-compatible-export-bacpac-sqlpackage.md)

 
Selles artiklis kirjeldatakse, kuidas eksportida [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) faili eksportida andmed taseme rakenduse viisardi abil SQL Server Management Studio SQL serveri andmebaasi. 

1. Veenduge, et teil on SQL Server Management Studio uusima versiooni. Uute versioonide Management Studio on värskendatud kuu jäävad sünkroonis Värskenduste Azure portaali.

     > [AZURE.IMPORTANT] On soovitatav alati kasutada uusimat versiooni Management Studio jäävad sünkroonitud värskendusega ning Microsoft Azure'i SQL-andmebaasi. [SQL Server Management Studio värskendada](https://msdn.microsoft.com/library/mt238290.aspx).

2. Avage Management Studio ja allika objekti Explorer andmebaasiga ühenduse.

    ![Klõpsake soovitud rakendus eksportimine](./media/sql-database-cloud-migrate/MigrateUsingBACPAC01.png)

3. Paremklõpsake objekti Exploreri lähteandmebaasi, **toimingud**ja klõpsake nuppu **Ekspordi rakendus...**

    ![Klõpsake soovitud rakendus eksportimine](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSSMS01.png)

4. Ekspordi viisardis konfigureerimine BACPAC faili salvestada kohalikule kettale asukoht kas või mõne Azure Bloobivahemälu ekspordi. Eksporditud BACPAC sisaldab alati täieliku andmebaasi skeemi ja vaikimisi kõik tabeli andmed. Kui soovite mõne või kõigi tabelite andmed välja, kasutage vahekaarti Täpsemalt. Näiteks võite ainult andmeid, tabelid, mitte kõik tabeli eksportida.

***Oluliste*** Azure'i bloobimälu on BACPAC eksportimisel kasutada standard salvestusruumi. Premium salvestusruumi lisamine BACPAC importimine ei toetata.

    ![Export settings](./media/sql-database-cloud-migrate/MigrateUsingBACPAC02.png)


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
