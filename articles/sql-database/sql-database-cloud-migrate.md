<properties
   pageTitle="SQL serveri andmebaasi migreerimine SQL-andmebaasiga | Microsoft Azure'i"
   description="Siit saate teada, kuidas on lood asutusesisese SQL serveri andmebaasi migreerimine Azure SQL-andmebaasiga pilveteenuses. Testida enne andmebaasi migreerimine ühilduvus Andmebaasiriistad migreerimise abil."
   keywords="andmebaasi migreerimine, sql serveri andmebaasi migreerimine, migreerimise Andmebaasiriistad, andmebaasi migreerimine, sql-andmebaasi migreerimine"
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

# <a name="sql-server-database-migration-to-sql-database-in-the-cloud"></a>SQL serveri andmebaasi migreerimine SQL-andmebaasiga pilveteenuses

Selles artiklis saate teada, kuidas mõni asutusesisese SQL Server 2005 või uuem versioon andmebaasi migreerimine Azure'i SQL-andmebaasi. Klõpsake selle andmebaasi migreerimisprotsessi migreerite oma skeemi ja andmete SQL serveri andmebaasi oma praeguse keskkonnas SQL-andmebaasi. Õnnestub, peate olemasoleva andmebaasi esmalt läbima ühilduvuse testi. [SQL-i andmebaasi V12](sql-database-v12-whats-new.md), mis on mõned ülejäänud ühilduvusprobleeme peale serveritaseme ja risti andmebaasi toimingutega seotud probleemi. Andmebaasid ja rakendused [osaliselt või Toetuseta funktsioonid](sql-database-transact-sql-information.md) on vaja mõned uuesti tehnika parandamiseks need vastuolude enne SQL serveri andmebaasi migreeritud.

Migreerimine, on järgmised juhised teil mugavalt:

- **Ühilduvuse test**: kinnitage andmebaasi ühilduvuse [SQL-i andmebaasi V12](sql-database-v12-whats-new.md). 
- **Lahendada ühilduvusega seotud probleemid, kui mõni**: kui valideerimine nurjub, peate valideerimine tõrgete parandamine.  
- **Käivita migreerimine** Kui teie andmebaas on ühilduv, saate ühe või mitme meetodite migreerimine. 

SQL serveri pakub mitmesuguseid viise tegemise neid toiminguid. Selles artiklis antakse ülevaade iga tööülesande saadaval meetoditest. Järgmine diagramm näitab juhiseid ja meetodid.

  ![VSSSDT migreerimise skeem](./media/sql-database-cloud-migrate/03VSSSDTDiagram.png)
  
 > [AZURE.NOTE] SQL serveri andmebaasi, sh Microsoft Accessi, Sybase, MySQL-i Oracle'i ja Azure SQL-andmebaasiga, DB2 migreerimine vt [SQL serveri migreerimise Plaanimisabimees](http://blogs.msdn.com/b/ssma/).

## <a name="database-migration-tools-test-sql-server-database-compatibility-with-sql-database"></a>Migreerimise Andmebaasiriistad testimine SQL serveri andmebaasi ühilduvuse SQL-andmebaas

SQL-andmebaasi ühilduvusega seotud probleemid katsetada enne alustamist andmebaasi migreerimisprotsessi, kasutage ühte järgmistest:

> [AZURE.SELECTOR]
- [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
- [SqlPackage](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md)
- [SSMS](sql-database-cloud-migrate-determine-compatibility-ssms.md)
- [Advisor täiendamine](http://www.microsoft.com/download/details.aspx?id=48119)
- [SAMW](sql-database-cloud-migrate-fix-compatibility-issues.md)

- [SQL Server Data Tools for Visual Studio ("SSDT")](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md): SSDT kasutab viimase ühilduvus reeglid SQL-i andmebaasi V12 vastuolude tuvastamiseks. Kui vastuolude, saate otse selles tööriistas tuvastatud probleemid lahendada. See on soovitatav meetod testimiseks ja SQL-i andmebaasi V12 ühilduvusega seotud probleemid lahendada. 
- [SqlPackage](sql-database-cloud-migrate-determine-compatibility-sqlpackage.md): SqlPackage on käsurea kasuliku, mis testib ühilduvuse probleemid ja loob aruande, mis sisaldab tuvastatud ühilduvusega seotud probleemid. Kui kasutate seda tööriista, veenduge, et uusima versiooni abil saate kasutada viimase ühilduvus reegleid. Kui leitakse vigu, kasutage mõnda muud tööriista tuvastati ühilduvusprobleemide - parandamiseks SSDT on soovitatav.  
- [Eksportida andmed taseme rakendus viisard SQL Server Management Studio](sql-database-cloud-migrate-determine-compatibility-ssms.md): See tuvastab ja ekraanil kuvatakse tõrketeateid. Kui leitakse vigu, saate jätkamiseks ja lõpetamiseks migreerimise SQL-andmebaasiga. Kui leitakse vigu, kasutage mõnda muud tööriista tuvastati ühilduvusprobleemide - parandamiseks SSDT on soovitatav.
- [The Microsoft SQL Server 2016 täiendamine Advisor eelvaade](http://www.microsoft.com/download/details.aspx?id=48119): seda tööriista, mis on praegu eelvaade, tuvastab ja koostab aruande SQL-i andmebaasi V12 vastuolude. See tööriist ei ole veel viimase ühilduvus reegleid. Kui leitakse vigu, saate jätkata ja lõpuleviimine migreerimine SQL-andmebaasiga. Kui leitakse vigu, kasutage mõnda muud tööriista tuvastati ühilduvusprobleemide - parandamiseks SSDT on soovitatav. 
- [SQL Azure'i migreerimise viisard ("SAMW")](sql-database-cloud-migrate-fix-compatibility-issues.md): SAMW on codeplex tööriista, mis kasutab Azure SQL-i andmebaasi V11 ühilduvus reeglid Azure SQL-i andmebaasi V12 vastuolude tuvastamiseks. Kui avastatakse vastuolude, mõned probleemid fikseeritud otse see tööriist. See tööriist leida vastuolude, mis ei pea olema kinnitatud. See on esimene Azure'i SQL-andmebaasi abi Migreerimistööriista saadaval ja aktiivselt toetab ühenduse SQL serveri. Lisaks selles tööriistas saavad teostada tööriist ise migratsioon kaudu. 

## <a name="fix-database-migration-compatibility-issues"></a>Andmebaasi migreerimine ühilduvusega seotud probleemide lahendamine

Kui ühilduvusega seotud probleemid, mida nende lahendamiseks enne SQL serveri andmebaasi migreerimine. On mitmesuguseid ühilduvusega seotud probleemid, mis võivad ilmneda, sõltuvalt SQL serveri versiooni lähteandmebaasi ja keerukuse tõttu andmebaasi migreerimist. SQL serveri vanemad versioonid on veel ühilduvusega seotud probleemid. Kasutage järgmisi ressursse, lisaks suunatud Interneti-otsingu abil oma otsingumootori valikuid:

- [SQL serveri andmebaasi funktsioonid ei toeta Azure SQL-andmebaas](sql-database-transact-sql-information.md)
- [Peatatud andmebaasi mootori funktsioone SQL Server 2016](https://msdn.microsoft.com/library/ms144262%28v=sql.130%29)
- [Peatatud andmebaasi mootori funktsioone SQL serveri 2014](https://msdn.microsoft.com/library/ms144262%28v=sql.120%29)
- [Peatatud andmebaasi mootori funktsioone SQL Server 2012](https://msdn.microsoft.com/library/ms144262%28v=sql.110%29)
- [Peatatud andmebaasi mootori funktsioone SQL Server 2008 R2](https://msdn.microsoft.com/library/ms144262%28v=sql.105%29)
- [Peatatud andmebaasi mootori funktsioone SQL Server 2005](https://msdn.microsoft.com/library/ms144262%28v=sql.90%29)

Lisaks Interneti otsimine ja kasutamine kasutada järgmiste ressursside [MSDN SQL serveri Veebifoorumid](https://social.msdn.microsoft.com/Forums/sqlserver/home?category=sqlserver) või [StackOverflow](http://stackoverflow.com/).

Kasutage ühte järgmistest migreerimise Andmebaasiriistad tuvastatud probleemide lahendamiseks.

> [AZURE.SELECTOR]
- [SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md)
- [SSMS](sql-database-cloud-migrate-fix-compatibility-issues-ssms.md)
- [SAMW](sql-database-cloud-migrate-fix-compatibility-issues.md)

- Kasutage [SQL Server Data Tools for Visual Studio ("SSDT")](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md): SSDT kasutamiseks importimist oma andmebaasi skeemi SQL Server Data Tools for Visual Studio "SSDT") ja koostada projekti SQL-i andmebaasi V12 juurutamiseks. Seejärel parandage SSDT kõik tuvastati ühilduvusega seotud probleemid. Kui olete lõpetanud, saate sünkroonida muutused tagasi allika andmebaasi (või allika andmebaasi koopia. SSDT on praegu soovitatav meetod testimiseks ja SQL-i andmebaasi V12 ühilduvusega seotud probleemid lahendada. Klõpsake linki [walk-through abil SSDT](sql-database-cloud-migrate-fix-compatibility-issues-ssdt.md).
- [SQL Server Management Studio ("SSMS")](sql-database-cloud-migrate-fix-compatibility-issues-ssms.md)kasutamine: SSMS kasutamiseks täidate Transact-SQL-i käskude abil mõnda muud tööriista tõrgete parandamiseks. See meetod on peamiselt kogenud kasutajad muuta andmebaasi skeemi andmeallika andmebaasis. 
- Kasutage [SQL Azure'i migreerimise viisard ("SAMW")](sql-database-cloud-migrate-fix-compatibility-issues.md): SAMW kasutamiseks luua skripti Transact-SQL-i andmebaasist allikas. Viisardi abil skripti, kui vähegi võimalik, et skeemiga vastavuses SQL-i andmebaasi V12. Kui olete lõpetanud, SAMW saate luua ühenduse SQL-i andmebaasi V12 skripti käivitada. See tööriist analüüsib ka Jälgi faili määratlemiseks ühilduvusega seotud probleemid. Skripti saab luua ainult skeemi või võivad hõlmata andmeid BCP vormingus.

## <a name="migrate-a-compatible-sql-server-database-to-sql-database"></a>Ühilduvad SQL serveri andmebaasi migreerimine SQL-andmebaasiga

Ühilduvad SQL serveri andmebaasi migreerimiseks Microsoft osutab mitu migreerimise meetodid erinevaid võimalusi. Teie valitud meetod sõltub teie hälbe tööseisakute, suurus ja keerukus SQL serveri andmebaasi ja oma ühenduvuse Microsoft Azure'i pilve.  

> [AZURE.SELECTOR]
- [SSMS migreerimise viisard](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md)
- [BACPAC faili eksportimine](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
- [BACPAC faili importimine](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [Tiražeerimise](sql-database-cloud-migrate-compatible-using-transactional-replication.md)

Valige migreerimise meetod, on esimese küsimuse küsida, kui saate endale võtta andmebaasi migreerimine jooksul välja. Andmebaasi migreerimine ajal aktiivne tehingud toimuvad võib põhjustada andmebaasi vastuolusid ja võimalike andmebaas on rikutud. Andmebaasi, blokeerimine klientrakenduse Ühenduvus [andmebaasi hetktõmmise](https://msdn.microsoft.com/library/ms175876.aspx)loomiseks on palju võimalusi jõudeolekusse viima.

Minimaalne ajakulu migreerimiseks kasutada [SQL serveri tehingu tiražeerimise](sql-database-cloud-migrate-compatible-using-transactional-replication.md) kui andmebaasi vastab tiražeerimise. Kui saate endale mõned tööseisakute või parasjagu katse migreerimise tootmise andmebaasi hiljem migreerimise, kaaluge üks järgmised kolm võimalust:

- [SSMS migreerimise viisard](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md): väikestele ja keskmise suurusega andmebaase, ühilduvad SQL Server 2005 ja uuemad andmebaasi migreerimine on sama lihtne kui töötab SQL Server Management Studio [Microsoft Azure'i andmebaasi viisard andmebaasi juurutamine](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md) .
- [Eksportida BACPAC faili](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md) ja seejärel [Impordi failist BACPAC](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md): kui ühenduvuse probleeme (pole Ühenduvus, väikese läbilaskevõimega või ajalõpp probleemid) ja suurte andmebaaside keskmise, kasutage [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) faili. Selle meetodiga saate eksportida SQL serveri skeemi ja andmete BACPAC faili. Seejärel impordite BACPAC faili eksportida andmed taseme rakenduse viisardi abil SQL Server Management Studio või [SqlPackage](https://msdn.microsoft.com/library/hh550080.aspx) käsuviiba kasuliku SQL-andmebaasi.
- BACPAC ja BCP koos kasutamine: kasutamine [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) faili ja [BCP](https://msdn.microsoft.com/library/ms162802.aspx) suurema andmebaaside saavutamiseks suurem parallelization jõudluse suurenemine, ehkki suurem keerukus. Selle meetodi migreerida skeemi ja andmeid eraldi.
 - [Skeemi ainult, et BACPAC faili eksportida](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md).
 - SQL-andmebaasi [skeemi ainult BACPAC failist importimine](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md) .
 - Kasutage [BCP](https://msdn.microsoft.com/library/ms162802.aspx) ekstrakti andmed tasapinnalise failid ja seejärel [paralleelselt laadi](https://technet.microsoft.com/library/dd425070.aspx) need failid Azure'i SQL-andmebaasi.

     ![SQL serveri andmebaasi migreerimine – SQL-andmebaasi migreerimine pilve.](./media/sql-database-cloud-migrate/01SSMSDiagram_new.png)

## <a name="next-steps"></a>Järgmised sammud

- [Microsoft SQL Server 2016 versioonitäienduse Advisor eelvaade](http://www.microsoft.com/download/details.aspx?id=48119)
- [SSDT uusim versioon](https://msdn.microsoft.com/library/mt204009.aspx)
- [SQL Server Management Studio uusim versioon](https://msdn.microsoft.com/library/mt238290.aspx)

##<a name="additional-resources"></a>Lisaressursid

- [SQL-i andmebaasi V12](sql-database-v12-whats-new.md)
[Transact-SQL-i osaliselt või Toetuseta funktsioonid](sql-database-transact-sql-information.md)
- [SQL serveri migreerimise abimehe abil SQL serveri andmebaasi migreerimine](http://blogs.msdn.com/b/ssma/)
