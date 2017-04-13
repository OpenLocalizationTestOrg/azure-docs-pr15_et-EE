<properties
   pageTitle="Migreerimine SQL-andmebaasiga tiražeerimise abil | Microsoft Azure'i"
   description="Microsoft Azure'i SQL-andmebaasi, andmebaasi migreerimine, importimine andmebaasi tiražeerimise"
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
   ms.date="08/23/2016"
   ms.author="carlrab"/>

# <a name="migrate-sql-server-database-to-azure-sql-database-using-transactional-replication"></a>SQL serveri andmebaasi migreerimine Azure'i SQL-andmebaasi tiražeerimise abil

> [AZURE.SELECTOR]
- [SSMS migreerimise viisard](sql-database-cloud-migrate-compatible-using-ssms-migration-wizard.md)
- [BACPAC faili eksportimine](sql-database-cloud-migrate-compatible-export-bacpac-ssms.md)
- [BACPAC faili importimine](sql-database-cloud-migrate-compatible-import-bacpac-ssms.md)
- [Tiražeerimise](sql-database-cloud-migrate-compatible-using-transactional-replication.md)

Selles artiklis teada ühilduvad SQL serveri andmebaasi migreerimine Azure SQL-andmebaasiga minimaalne ajakulu SQL serveri tiražeerimise abil.

## <a name="understanding-the-transactional-replication-architecture"></a>Tiražeerimise ülesehituse mõistmine

Kui te ei saa eemaldada SQL Serveri andmebaasiga toodetud ajal migreerimise esineb, saate oma migreerimise lahenduseks SQL serveri tiražeerimise. See lahendus kasutamiseks saate seadistada Azure'i SQL-andmebaasi tellija, et asutusesisese SQL serveri eksemplar, mida soovite migreerida. Kohapealse tiražeerimise edasimüüja sünkroonib andmete sünkroonimist (publisher) ajal uue tehinguid tekkida kohapealse andmebaasist. 

Saate tiražeerimise migreerimine kohapealsesse andmebaasi alamhulga. Publikatsioon, kuhu te ise Azure SQL-andmebaasiga võib olla piiratud alamhulga kopeeritud andmebaasi tabelid. Iga tabeli kopeeritud, saate piirata andmete alamkogumit, read ja/või veerud alamhulga.

Tiražeerimise, kus SQL Azure'i andmebaasi kuvataks kõik muudatused skeemi teie andmetele juurde. Kui sünkroonimine on lõpule viidud ja olete migreerimiseks valmis, saate muuta rakenduste osutamiseks Azure'i SQL-andmebaasi ühendusstringi. Kui tiražeerimise kiirendab kohapealse andmebaasi ja kõik rakenduste osutage Azure'i DB vasakule muudatused, saate desinstallida tiražeerimise. Azure'i SQL-andmebaasi on nüüd teie tootmissüsteemi.

 ![SeedCloudTR skeem](./media/sql-database-cloud-migrate/SeedCloudTR.png)

## <a name="transactional-replication-requirements"></a>Selgituseks Dispersioonanalüüs nõuded

Tiražeerimise on valmis- ja SQL serveri integreeritud tehnoloogia alates SQL Server 6.5. See on küps ja tõestatud tehnoloogia, et enamik DBAs teate, kellega neil on kogemus. [SQL Server 2016](https://www.microsoft.com/en-us/cloud-platform/sql-server)on nüüd võimalik konfigureerida Azure'i SQL-andmebaasi [tiražeerimise abonendi](https://msdn.microsoft.com/library/mt589530.aspx) kohapealse publikatsiooni. Kogemus, et saada häälestamise Management Studio on sama, nagu siis, kui häälestate tiražeerimise abonendi kohapealse serveris. Selle stsenaariumi tugi on toetatud, kui avaldaja ja edasimüüja on vähemalt üks järgmistest SQL serveri versiooni:

 - SQL Server 2016 ja uuemad 
 - SQL serveri 2014 SP1 CU3 ja ülaltoodud
 - SQL serveri 2014 RTM 10 VÜ-d ja ülaltoodud
 - SQL Server 2012 SP2 CU8 ja ülaltoodud
 - SQL Server 2012 hoolduspaketi SP3 ja uuemad


> [AZURE.IMPORTANT] Uusima versiooni SQL Server Management Studio abil jäävad sünkroonitud värskendusega ning Microsoft Azure'i SQL-andmebaasi. Varasemate versioonide SQL Server Management Studio ei saa seadistada SQL-andmebaasi tellija. [SQL Server Management Studio värskendada](https://msdn.microsoft.com/library/mt238290.aspx).


## <a name="next-steps"></a>Järgmised sammud

- [SQL Server Management Studio uusim versioon](https://msdn.microsoft.com/library/mt238290.aspx)
- [SSDT uusim versioon](https://msdn.microsoft.com/library/mt204009.aspx)
- [SQL Server 2016](https://www.microsoft.com/en-us/cloud-platform/sql-server)

## <a name="additional-resources"></a>Lisaressursid

- [Tiražeerimise](https://msdn.microsoft.com/library/mt589530.aspx)
- [SQL-i andmebaasi V12](sql-database-v12-whats-new.md)
- [Transact-SQL osaliselt või Toetuseta funktsioonid](sql-database-transact-sql-information.md)
- [SQL serveri migreerimise abimehe abil SQL serveri andmebaasi migreerimine](http://blogs.msdn.com/b/ssma/)
