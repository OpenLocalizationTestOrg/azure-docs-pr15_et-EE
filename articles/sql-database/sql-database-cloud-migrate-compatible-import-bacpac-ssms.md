<properties
   pageTitle="SQL serveri andmebaasi migreerimine Azure'i SQL-andmebaasi | Microsoft Azure'i"
   description="Microsoft Azure'i SQL-andmebaasi, andmebaasi juurutada, andmebaasi migreerimine andmebaasi import ekspordi andmebaasi migreerimine viisard"
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

# <a name="import-from-bacpac-to-sql-database-using-ssms"></a>Importimine BACPAC abil SSMS SQL-andmebaasiga

> [AZURE.SELECTOR]
- [SSMS](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [SqlPackage](sql-database-cloud-migrate-compatible-import-bacpac-sqlpackage.md)
- [Azure'i portaal](sql-database-import.md)
- [PowerShelli](sql-database-import-powershell.md)

Selles artiklis kirjeldatakse kuidas importida failist [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) SQL-andmebaasi SQL Server Management Studio eksportida andmed taseme rakenduse viisardi abil.

> [AZURE.NOTE] Järgmised toimingud endale on juba ette valmistatud oma Azure SQL-i loogilise eksemplar ja ühendusteabe käepärast.

1. Veenduge, et teil on SQL Server Management Studio uusima versiooni. Uute versioonide Management Studio on värskendatud kuu jäävad sünkroonis Värskenduste Azure portaali.

     > [AZURE.IMPORTANT] On soovitatav alati kasutada uusimat versiooni Management Studio jäävad sünkroonitud värskendusega ning Microsoft Azure'i SQL-andmebaasi. [SQL Server Management Studio värskendada](https://msdn.microsoft.com/library/mt238290.aspx).

2. Azure SQL-i andmebaasiserveriga ühenduse, paremklõpsake kausta **andmebaasid** ja klõpsake nuppu **impordi rakendus...**

    ![Andmete taseme rakenduse menüükäsk importimine](./media/sql-database-cloud-migrate/MigrateUsingBACPAC03.png)

3.  Andmebaasi loomiseks Azure SQL-andmebaasis BACPAC faili importimine oma kohalikule kettale või valige Azure storage konto ja container, millele saate üles laadida BACPAC faili.

    ![Impordisätete](./media/sql-database-cloud-migrate/MigrateUsingBACPAC04.png)

     > [AZURE.IMPORTANT] Kui importimine on BACPAC Azure'i bloobimälu, kasutada kindlad. Premium salvestusruumi lisamine BACPAC importimine ei toetata.

4.  **Uue andmebaasi nimi** andmebaasi Azure SQL-i DB ette, **Väljaande Microsoft Azure'i SQL-i andmebaasi** (teenuse kiht), **andmebaasi maksimummaht**ja **Teenuse eesmärk** (jõudlusega).

    ![Andmebaasi sätted](./media/sql-database-cloud-migrate/MigrateUsingBACPAC05.png)

5.  Klõpsake nuppu **edasi** ja seejärel nuppu **valmis** uue andmebaasi Azure'i SQL-andmebaasi serveri BACPAC faili importida.

6. Objekti Exploreri abil luua ühenduse oma Azure'i SQL-andmebaasi serveri migreeritud andmebaasi.

6.  Azure portaali kasutamisel saate vaadata oma andmebaasi ja selle atribuute.

## <a name="next-steps"></a>Järgmised sammud

- [SSDT uusim versioon](https://msdn.microsoft.com/library/mt204009.aspx)
- [SQL Server Management Studio uusim versioon](https://msdn.microsoft.com/library/mt238290.aspx)

## <a name="additional-resources"></a>Lisaressursid

- [SQL-i andmebaasi V12](sql-database-v12-whats-new.md)
- [Transact-SQL osaliselt või Toetuseta funktsioonid](sql-database-transact-sql-information.md)
- [SQL serveri migreerimise abimehe abil SQL serveri andmebaasi migreerimine](http://blogs.msdn.com/b/ssma/)
