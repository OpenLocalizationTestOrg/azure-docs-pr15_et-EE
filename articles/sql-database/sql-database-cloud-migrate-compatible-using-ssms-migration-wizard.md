<properties
   pageTitle="SQL serveri andmebaasi migreerimine SQL-andmebaasiga, kasutades Microsoft Azure'i andmebaasi viisard andmebaasi juurutamine | Microsoft Azure'i"
   description="Microsoft Azure'i SQL-andmebaasi, andmebaasi migreerimine, Microsoft Azure'i andmebaasi viisard"
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

# <a name="migrate-sql-server-database-to-sql-database-using-deploy-database-to-microsoft-azure-database-wizard"></a>SQL serveri andmebaasi migreerimine SQL-andmebaasiga, kasutades Microsoft Azure'i andmebaasi viisard andmebaasi juurutamine


> [AZURE.SELECTOR]
- [SSMS migreerimise viisard](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md)
- [BACPAC faili eksportimine](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
- [BACPAC faili importimine](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [Tiražeerimise](sql-database-cloud-migrate-compatible-using-transactional-replication.md)

Microsoft Azure'i andmebaasi Viisardi SQL Server Management Studio andmebaasi migreerib [ühilduvad SQL serveri andmebaasi](sql-database-cloud-migrate.md) otse oma Azure SQL-andmebaasi server.

## <a name="use-the-deploy-database-to-microsoft-azure-database-wizard"></a>Deploy andmebaasi Microsoft Azure'i andmebaasi viisardi kasutamine

> [AZURE.NOTE] Järgmiste juhiste korral eeldatakse, et teil on [ette valmistatud SQL-andmebaasi server](https://azure.microsoft.com/documentation/learning-paths/sql-database-training-learn-sql-database/).

1. Veenduge, et teil on SQL Server Management Studio uusima versiooni. Uute versioonide Management Studio on värskendatud kuu jäävad sünkroonis Värskenduste Azure portaali.

    > [AZURE.IMPORTANT] On soovitatav alati kasutada uusimat versiooni Management Studio jäävad sünkroonitud värskendusega ning Microsoft Azure'i SQL-andmebaasi. [SQL Server Management Studio värskendada](https://msdn.microsoft.com/library/mt238290.aspx).

2. Avage Management Studio ja migreerida objekti Explorer SQL Serveri andmebaasiga ühenduse loomiseks.
3. Paremklõpsake objekti Exploreri andmebaasis, **toimingud**ja klõpsake käsku **Microsoft Azure'i SQL-andmebaasi... andmebaas juurutamine**

    ![Tööülesannete menüüst Azure juurutamine](./media/sql-database-cloud-migrate/MigrateUsingDeploymentWizard01.png)

4.  Juurutamise viisardis klõpsake nuppu **edasi**ja seejärel klõpsake nuppu **Loo ühendus** konfigureerida ühenduse oma SQL-andmebaasi server.

    ![Tööülesannete menüüst Azure juurutamine](./media/sql-database-cloud-migrate/MigrateUsingDeploymentWizard002.png)

5. Sisestage dialoogiboksis ühenduse oma ühenduse teavet oma SQL-i andmebaasiserveriga ühenduse loomiseks.

    ![Tööülesannete menüüst Azure juurutamine](./media/sql-database-cloud-migrate/MigrateUsingDeploymentWizard00.png)

5.  Sisestage järgmine [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) faili, mis selle viisardi migreerimise käigus.

 - **Uue andmebaasi nimi** 
 - **Väljaande Microsoft Azure'i SQL-i andmebaasi** ([teenuse kiht](sql-database-service-tiers.md))
 - **Andmebaasi maksimummaht**
 - **Teenuse eesmärk** (jõudlusega)
 - **Ajutise faili nimi**  

    ![Ekspordi sätted](./media/sql-database-cloud-migrate/MigrateUsingDeploymentWizard02.png)

6.  Viisardi lõpuleviimine. Sõltuvalt suurus ja keerukus andmebaasi, juurutamise võib kuluda paar minutit mitu tundi. Kui viisard tuvastab ühilduvusega seotud probleemid, kuvatakse Kuva tõrked ja migreerimise ei saa jätkata. Juhised, kuidas andmebaasi ühilduvusprobleemi lahendamiseks, minge [andmebaasi ühilduvusega seotud probleemid lahendada](sql-database-cloud-migrate-fix-compatibility-issues.md).

7.  Objekti Exploreri abil luua ühenduse oma Azure'i SQL-andmebaasi serveri migreeritud andmebaasi.
8.  Azure portaali kasutamisel saate vaadata oma andmebaasi ja selle atribuute.

## <a name="next-steps"></a>Järgmised sammud

- [SSDT uusim versioon](https://msdn.microsoft.com/library/mt204009.aspx)
- [SQL Server Management Studio uusim versioon](https://msdn.microsoft.com/library/mt238290.aspx)

## <a name="additional-resources"></a>Lisaressursid

- [SQL-i andmebaasi V12](sql-database-v12-whats-new.md)
- [Transact-SQL osaliselt või Toetuseta funktsioonid](sql-database-transact-sql-information.md)
- [SQL serveri migreerimise abimehe abil SQL serveri andmebaasi migreerimine](http://blogs.msdn.com/b/ssma/)
