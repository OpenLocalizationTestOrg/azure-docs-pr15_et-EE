<properties
   pageTitle="SQL serveri andmebaasi ühilduvusega seotud probleemid enne migreerimise SQL-andmebaasiga parandamine | Microsoft Azure'i"
   description="Microsoft Azure'i SQL-andmebaasi, andmebaasi migreerimine, ühilduvust, SQL Azure'i migreerimise viisardis SSDT"
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

# <a name="migrate-a-sql-server-database-to-azure-sql-database-using-sql-server-data-tools-for-visual-studio"></a>SQL serveri andmebaasi migreerimine SQL Server Data Tools for Visual Studio abil Azure SQL-andmebaasiga 

> [AZURE.SELECTOR]
- [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
- [SqlPackage](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md)
- [SSMS](sql-database-cloud-migrate-determine-compatibility-ssms.md)
- [Advisor täiendamine](http://www.microsoft.com/download/details.aspx?id=48119)
- [SAMW](sql-database-cloud-migrate-fix-compatibility-issues.md)

Selles artiklis saate teada, avastada ja parandada kasutades SQL Server Data Tools for Visual Studio enne migreerimise Azure SQL-andmebaasiga SQL serveri andmebaasi ühilduvusega seotud probleemid.

## <a name="using-sql-server-data-tools-for-visual-studio"></a>SQL Server Data Tools for Visual Studio abil

Kasutada SQL Server Data Tools for Visual Studio ("SSDT") importida Andmebaasiskeem Visual Studio andmebaasi projekti analüüsi jaoks. Analüüsiks, saate määrata suunatud platvormi projekti SQL-i andmebaasi V12 ja siis koostada projekt. Kui koostamine õnnestus, andmebaas on ühilduvad. Kui koostamine nurjub, saate lahendada tõrgete SSDT (või üks tööriistu, mida selles teemas käsitletakse). Kui projekt koostab edukalt, saate selle avaldada, uuesti lähteandmebaasi koopiana. Seejärel saate funktsiooni andmete võrdlus SSDT allika andmebaasi andmete kopeerimiseks Azure SQL-i V12 ühilduvad andmebaas. Seejärel saate seda värskendatud andmebaasi migreerida. Selle suvandi kasutamiseks alla [SSDT uusim versioon](https://msdn.microsoft.com/library/mt204009.aspx).

  ![VSSSDT migreerimise skeem](./media/sql-database-cloud-migrate/03VSSSDTDiagram.png)

  > [AZURE.NOTE] Kui on vaja teha vaid skeemi migreerimise, skeemiga saab avaldada otse Visual Studio otse Azure'i SQL-andmebaasi. Kasutage seda meetodit Andmebaasiskeem nõuab rohkem muudatusi, kui võib käsitleda eraldi migreerimise viisardiga.

## <a name="detecting-compatibility-issues-using-sql-server-data-tools-for-visual-studio"></a>SQL Server Data Tools for Visual Studio abil ühilduvusprobleemide tuvastamiseks
   
1.  Avage **SQL serveri objekti Exploreri** Visual Studios. Ühenduse loomine SQL serveri eksemplar, mis sisaldab andmebaasi migreerida **Lisamine SQL serveri** abil. Leidke andmebaas objekti Explorer, paremklõpsake andmebaasist ja valige **Loo uus projekt...**     
    
    ![Uue projekti](./media/sql-database-migrate-visualstudio-ssdt/02MigrateSSDT.png)    
   
2.  Importida **ainult rakenduse ulatusega objektide**importimine sätete konfigureerimine. Tühjendage ruudud importimiseks järgmist: viidatud sisselogimise, õiguste ja andmebaasi sätted.    

    ![Asetekst](./media/sql-database-migrate-visualstudio-ssdt/03MigrateSSDT.png)    

3.  Klõpsake andmebaasi importida ja luua sisaldav T-SQL-i skriptifail iga objekti andmebaasi projekt **käivitamine** . Skripti faile on pesastatud kaustad projekt.    

    ![Asetekst](./media/sql-database-migrate-visualstudio-ssdt/04MigrateSSDT.png)    

4.  Visual Studio lahenduste Explorer, paremklõpsake andmebaasi projekti ja valige Atribuudid. Lehel **Projekti sätete** konfigureerimine suunatud platvormi Microsoft Azure'i SQL-i andmebaasi v12.    
    
    ![Asetekst](./media/sql-database-migrate-visualstudio-ssdt/05MigrateSSDT.png)    
    
5.  Paremklõpsake projekti ja valige **koostada** projekti koostamiseks.    
    
    ![Asetekst](./media/sql-database-migrate-visualstudio-ssdt/06MigrateSSDT.png)    
    
6.  **Tõrge loendis** kuvatakse iga ühildamatus. Sel juhul kasutaja nimi, NT AUTHORITY\NETWORK teenus ei ühildu. Kuna see ei ühildu, saate selle välja kommentaaride või selle eemaldada (ja aadress mõju andmebaasi lahendus selle login ja rolli eemaldamine).     
    
    ![Asetekst](./media/sql-database-migrate-visualstudio-ssdt/07MigrateSSDT.png)    
    
## <a name="fixing-compatibility-issues-using-sql-server-data-tools-for-visual-studio"></a>Probleemide ühilduvusega seotud probleemid SQL Server Data Tools for Visual Studio abil

1.  Topeltklõpsake avamiseks skripti päringuaken ja kommentaari skripti välja esimese skripti ja seejärel käivitada skripti.     
    ![Asetekst](./media/sql-database-migrate-visualstudio-ssdt/08MigrateSSDT.png)

2.  Korrake seda toimingut iga skripti, mis sisaldavad vastuolude kuni tõrget ei jäävad.    
    ![Asetekst](./media/sql-database-migrate-visualstudio-ssdt/09MigrateSSDT.png)
    
3.  Kui andmebaas on tasuta tõrkeid, paremklõpsake projekti ja valige **Avalda**. Sisseehitatud ja avaldatud allika andmebaasi koopia (see on soovitatav kasutada koopia, vähemalt).     
 - Enne avaldamist, sõltuvalt allika SQL serveri versioon (varasem kui SQL Server 2014), peate projekti suunata platvorm lubamiseks juurutamise lähtestada.     
 - Kui migreerite mõne vanema SQL Serveri andmebaas, tutvustamine kõigi funktsioonide projekti, mis pole toetatud SQL serveri allikas kuni migreerimine uuemat versiooni SQL serveri andmebaasi.     

        ![alt text](./media/sql-database-migrate-visualstudio-ssdt/10MigrateSSDT.png)    
    
        ![alt text](./media/sql-database-migrate-visualstudio-ssdt/11MigrateSSDT.png)    
        
4.  SQL Server objekti Explorer, paremklõpsake andmebaasi allikas ja klõpsake käsku **Andmete võrdlus**. Projekti algset andmebaasi võrdlus aitab teil mõista, millised muudatused on tehtud viisard. Valige oma andmebaasi Azure SQL-i V12 versiooni ja seejärel klõpsake nuppu **valmis**.    
    
    ![Asetekst](./media/sql-database-migrate-visualstudio-ssdt/12MigrateSSDT.png)    
    
    ![Asetekst](./media/sql-database-migrate-visualstudio-ssdt/13MigrateSSDT.png)    

5.  Vaadake avastatud erinevusi ja seejärel nuppu **Update Target** andmete migreerimiseks allika andmebaasi Azure SQL-i V12 andmebaasi.     
    
    ![Asetekst](./media/sql-database-migrate-visualstudio-ssdt/14MigrateSSDT.png)    
    
6.  Valige juurutamise viis. Vt [ühilduvad SQL serveri andmebaasi migreerimine SQL-andmebaasiga.](sql-database-cloud-migrate.md)  

## <a name="next-steps"></a>Järgmised sammud

- [SSDT uusim versioon](https://msdn.microsoft.com/library/mt204009.aspx)
- [SQL Server Management Studio uusim versioon](https://msdn.microsoft.com/library/mt238290.aspx)

## <a name="additional-resources"></a>Lisaressursid

- [SQL-i andmebaasi V12](sql-database-v12-whats-new.md)
- [Transact-SQL osaliselt või Toetuseta funktsioonid](sql-database-transact-sql-information.md)
- [SQL serveri migreerimise abimehe abil SQL serveri andmebaasi migreerimine](http://blogs.msdn.com/b/ssma/)
