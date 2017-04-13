<properties
    pageTitle="SQL-mälu, alustamine | Microsoft Azure'i"
    description="SQL-mälu tehnoloogiad oluliselt parandada jõudlust selgituseks ja analytics töökoormus. Saate teada, kuidas nende tehnoloogiate eeliseid."
    services="sql-database"
    documentationCenter=""
    authors="jodebrui"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="jodebrui"/>


# <a name="get-started-with-in-memory-preview-in-sql-database"></a>Alustamine-mälu (eelvaade) SQL-andmebaasis

-Mälu funktsioonid oluliselt parandada jõudlust selgituseks ja analytics töökoormus õige olukordades.

Selles teemas rõhutab kahe esitlused, üks-mälu OLTP ja üks-mälu Analytics. Iga demo tuleb koos juhiseid ja demo käivitamiseks peate kood. Saate teha järgmist.

- Muudatuste vaatamiseks erinevused jõudluse tulemuste; testimiseks kasutada kood või
- Lugege mõista seda stsenaariumi, ja vaadake, kuidas luua ja kasutada In Memory objektide koodi.

> [AZURE.VIDEO azure-sql-database-in-memory-technologies]

- [Kiire alustamine 1:-mälu OLTP tehnoloogiad kiiremini T-SQL-i jõudluse](http://msdn.microsoft.com/library/mt694156.aspx) -on teise artikkel aitab teil tööd alustada.

#### <a name="in-memory-oltp"></a>-Mälu OLTP

-Mälu [OLTP](#install_oltp_manuallink) (online tehingu töötlemine) funktsioonid on:

- Mälu optimeeritud tabelid.
- Algupäraselt koostatud salvestatud toimingute.


Mälu optimeeritud tabelis on üks kujutis ise aktiivne mälu, lisaks standard esituse kõvakettale. Tabeli tehingud kiiremini käitada, kuna need otse interaktiivselt kasutada ainult esituse, mis on aktiivne mälu.

Mis-mälu OLTP, võite saavutada kuni 30 korda saada läbilaskevõimet sõltuvalt töökoormus üksikasjad.


Üldjuhul kompileeritud salvestatud toimingute jaoks on vaja vähem arvutisse juhiseid Käivita ajal, kui traditsiooniline tõlgendada salvestatud protseduurid. Oleme näinud kohalikke koostamine tulemi kestus, mis on 1 100 tõlgendada kestus.


#### <a name="in-memory-analytics"></a>-Mälu Analytics 

Funktsioon-mälu [Analytics](#install_analytics_manuallink) on:

Columnstore registrite parandada analüüsi- ja päringute teatamine. 


#### <a name="real-time-analytics"></a>Reaalajas Analytics

[Reaalajas](http://msdn.microsoft.com/library/dn817827.aspx) Analyticsi tuleb teil omavahel kombineerida-mälu OLTP ja analüüsi, et saada:

- Reaalajas äri ülevaate funktsionaalseid andmete põhjal.


#### <a name="availability"></a>Kättesaadavus


GA, üldiselt kättesaadav:

- [Columnstore registrid](http://msdn.microsoft.com/library/dn817827.aspx) , mis on *kettal*.


Eelvaade:

- -Mälu OLTP
- Reaalajas funktsionaalseid Analytics


Kaalutlused ajal eelvaade on In Memory funktsioonid on kirjeldatud [seda teemat](#preview_considerations_for_in_memory).


> [AZURE.NOTE] Need eelvaade funktsioonid on saadaval ainult [*Premium*](sql-database-service-tiers.md) andmebaasidega pole elastne kaustu ja mis tahes Basic või Standard andmebaaside jaoks pole saadaval.  Premium andmebaaside elastne kaustadesse tugi on tulekul. 


<a id="install_oltp_manuallink" name="install_oltp_manuallink"></a>

&nbsp;

## <a name="a-install-the-in-memory-oltp-sample"></a>V-SSE. Installige In Memory OLTP näidis

[Azure'i portaalis](https://portal.azure.com/)mõne klõpsu abil saate luua näidisandmebaasi AdventureWorksLT [V12]. Seejärel selle jaotise juhised selgitavad, kuidas saate rikastamine andmebaasi AdventureWorksLT:

- Tabelite-mälu.
- Üldjuhul kompileeritud salvestatud protseduur.


#### <a name="installation-steps"></a>Installimise juhiseid.

1. [Azure'i portaalis](https://portal.azure.com/)V12 serveris Premium andmebaasi loomine. Määrake **Allikas** näidisandmebaasi AdventureWorksLT [V12].
 - Üksikasjalikud juhised, saate vaadata [esimese Azure SQL-i andmebaasi loomine](sql-database-get-started.md).

2. Andmebaasiga ühenduse loomiseks ja SQL Server Management Studio [(SSMS.exe)](http://msdn.microsoft.com/library/mt238290.aspx).

3. [-Mälu OLTP Transact-SQL-i skripti](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/sql_in-memory_oltp_sample.sql) kopeerimine lõikelauale.
 - T-SQL-skript loob vajalikud mälu objektide AdventureWorksLT näidisandmebaasi lõite juhises 1.

4. T-SQL-i skripti kleepimine SSMS ja seejärel käivitada skripti.
 - Oluline on selle `MEMORY_OPTIMIZED = ON` klausel tabeli loomine laused, nagu:


```
CREATE TABLE [SalesLT].[SalesOrderHeader_inmem](
    [SalesOrderID] int IDENTITY NOT NULL PRIMARY KEY NONCLUSTERED ...,
    ...
) WITH (MEMORY_OPTIMIZED = ON);
```


#### <a name="error-40536"></a>Tõrge 40536


Kui kuvatakse tõrketeade 40536 T-SQL-i skripti käivitamisel, käivitage järgmine T-SQL skript kontrollimaks, kas andmebaasi toetab-mälu:


```
SELECT DatabasePropertyEx(DB_Name(), 'IsXTPSupported');
```


Tulemus on **0** tähendab, et ei toetata-mälu ja 1 tähendab, et see on toetatud. Probleemi diagnoosida:

- Veenduge, et andmebaas on loodud pärast In Memory OLTP funktsioonid sai aktiivne eelvaade.
- Veenduge, et andmebaas on Premium teenuse kiht.


#### <a name="about-the-created-memory-optimized-items"></a>Loodud mälu optimeeritud üksuste kohta

**Tabelite**: valimi sisaldab mälu optimeeritud tabelid:

- SalesLT.Product_inmem
- SalesLT.SalesOrderHeader_inmem
- SalesLT.SalesOrderDetail_inmem
- Demo.DemoSalesOrderHeaderSeed
- Demo.DemoSalesOrderDetailSeed


Saate kontrollida mälu optimeeritud tabelid **Objekti Exploreri** SSMS, kaudu:

- Paremklõpsake **tabeli** > **Filter** > **Filtrisätteid** > **Mälu on optimeeritud** võrdub 1.


Või saab päringud vaadete kataloogi näiteks järgmist.


```
SELECT is_memory_optimized, name, type_desc, durability_desc
    FROM sys.tables
    WHERE is_memory_optimized = 1;
```


**Algupäraselt koostatud salvestatud protseduuri**: SalesLT.usp_InsertSalesOrder_inmem saab kontrollida kataloogi vaade päringu kaudu:


```
SELECT uses_native_compilation, OBJECT_NAME(object_id), definition
    FROM sys.sql_modules
    WHERE uses_native_compilation = 1;
```


&nbsp;

## <a name="run-the-sample-oltp-workload"></a>Valimi OLTP töökoormus käivitamine

Ainult vahe järgmised kaks *Salvestatud toimingute* on, et esimene kord kasutab mälu optimeeritud versiooni tabelid, samal ajal teine protseduur kasutab tavalise kettal tabelid:

- SalesLT**.** usp_InsertSalesOrder**_inmem**
- SalesLT**.** usp_InsertSalesOrder**_ondisk**


Selles jaotises kuvatakse mugav **ostress.exe** kasuliku abil saate käivitada kaks salvestatud toimingute stressirohke tasanditel. Saate võrrelda palju aega kulub kaks stress käivitatakse lõpuleviimiseks.


Kui käivitate ostress.exe, soovitame, et te kaotate parameetrite väärtused, mis on kujundatud nii:

- Käivitage suure hulga samaaegseid ühendusi, kasutades - n100.
- On iga ühendus tsükkel sadu korda, mõõt kasutamine – r500.


Siiski võib soovite alustada palju väiksemate väärtustega like - n10 ja - r50 tagamaks, et kõik töötab.


### <a name="script-for-ostressexe"></a>Skripti ostress.exe


Selles jaotises kuvatakse T-SQL-i skripti, mis on manustatud meie ostress.exe käsurea. Skript kasutab T-SQL-i skripti installisite varasemas versioonis loodud üksusi.


Järgmise skripti lisatakse järgmised mälu optimeeritud *tabelid*proovi müük, et viis kirjeid:

- SalesLT.SalesOrderHeader_inmem
- SalesLT.SalesOrderDetail_inmem


```
DECLARE
    @i int = 0,
    @od SalesLT.SalesOrderDetailType_inmem,
    @SalesOrderID int,
    @DueDate datetime2 = sysdatetime(),
    @CustomerID int = rand() * 8000,
    @BillToAddressID int = rand() * 10000,
    @ShipToAddressID int = rand() * 10000;
    
INSERT INTO @od
    SELECT OrderQty, ProductID
    FROM Demo.DemoSalesOrderDetailSeed
    WHERE OrderID= cast((rand()*60) as int);
    
WHILE (@i < 20)
begin;
    EXECUTE SalesLT.usp_InsertSalesOrder_inmem @SalesOrderID OUTPUT,
        @DueDate, @CustomerID, @BillToAddressID, @ShipToAddressID, @od;
    SET @i = @i + 1;
end
```


Eelmise T-SQL-i ostress.exe _ondisk versiooni teha soovite nii esinemiskordade *_inmem* alamstringi asendada lihtsalt *_ondisk*. Neid asendab mõjutavad salvestatud toimingute ja tabelite nimesid.


### <a name="install-rml-utilities-and-ostress"></a>RML Utiliidid ja ostress installimine


Soovite plaanite ideaalvariandis mitte ostress.exe käitamist on Azure VM. Luua ka [Azure virtuaalse masina](https://azure.microsoft.com/documentation/services/virtual-machines/) Azure geograafilised piirkonna AdventureWorksLT andmebaasi asukoht. Kuid saab käitada ostress.exe oma sülearvutis.


VM või mis tahes majutada, saate valida, installige utiliidid (RML kordus Markup Language), mis sisaldavad ostress.exe.

- Lugege teemat ostress.exe arutelu [näidis-mälu OLTP](http://msdn.microsoft.com/library/mt465764.aspx)andmebaasi.
 - Või vaadake teemat [Näidis-mälu OLTP andmebaasi](http://msdn.microsoft.com/library/mt465764.aspx).
 - Või vaadake teemat [installimist ostress.exe ajaveeb](http://blogs.msdn.com/b/psssql/archive/2013/10/29/cumulative-update-2-to-the-rml-utilities-for-microsoft-sql-server-released.aspx)



<!--
dn511655.aspx is for SQL 2014,
[Extensions to AdventureWorks to Demonstrate In-Memory OLTP]
(http://msdn.microsoft.com/library/dn511655&#x28;v=sql.120&#x29;.aspx)

whereas for SQL 2016+
[Sample Database for In-Memory OLTP]
(http://msdn.microsoft.com/library/mt465764.aspx)
-->



### <a name="run-the-inmem-stress-workload-first"></a>Käivitage _inmem stress töökoormus esmalt


Mõne *RML Cmd Käsuviip* akna abil saate käivitada meie ostress.exe käsurea. Käsurea parameetrid otsene ostress abil:

- Käivitada üheaegselt ühenduste 100 (-n100).
- On iga ühendus T-SQL-i skripti 50 korda käivitada (-r50).


```
ostress.exe -n100 -r50 -S<servername>.database.windows.net -U<login> -P<password> -d<database> -q -Q"DECLARE @i int = 0, @od SalesLT.SalesOrderDetailType_inmem, @SalesOrderID int, @DueDate datetime2 = sysdatetime(), @CustomerID int = rand() * 8000, @BillToAddressID int = rand() * 10000, @ShipToAddressID int = rand()* 10000; INSERT INTO @od SELECT OrderQty, ProductID FROM Demo.DemoSalesOrderDetailSeed WHERE OrderID= cast((rand()*60) as int); WHILE (@i < 20) begin; EXECUTE SalesLT.usp_InsertSalesOrder_inmem @SalesOrderID OUTPUT, @DueDate, @CustomerID, @BillToAddressID, @ShipToAddressID, @od; set @i += 1; end"
```


Eelmise ostress.exe käsurea käivitamiseks tehke järgmist.


1. Andmebaasi andmete sisu lähtestada SSMS, mis tahes eelmise käivitatakse sisestatud andmete kustutamiseks käivitage järgmine käsk:
```
EXECUTE Demo.usp_DemoReset;
```

2. Eelmise ostress.exe käsurea teksti kopeerida lõikelauale.

3. Asendada selle `<placeholders>` jaoks soovitud parameetrid -S - U -P -d ja õige tegeliku väärtuse.

4. Käivitage oma redigeeritud käsurea RML Cmd aknas.


#### <a name="result-is-a-duration"></a>Tulem on kestus


Kui ostress.exe on lõpule jõudnud, kirjutab Käivita kestus, kui selle väljundi viimase rea RML Cmd aknas. Näiteks lühemaks katse kestis umbes 1,5 minutit:

`11/12/15 00:35:00.873 [0x000030A8] OSTRESS exiting normally, elapsed time: 00:01:31.867`


#### <a name="reset-edit-for-ondisk-then-rerun"></a>Lähtestada, _ondisk redigeerida ja seejärel käivitage uuesti


Olete _inmem käitatav tulem, järgmiste toimingute jaoks _ondisk, käivitage:


1. Lähtestada andmebaasi SSMS kustutada kõik andmed, mis lisati eelmise Käivita käivitage järgmine käsk:
```
EXECUTE Demo.usp_DemoReset;
```

2. Asendage kõik *_inmem* *_ondisk*ostress.exe käsurea redigeerida.

3. Käivitage uuesti ostress.exe teist korda ja tulemuseks duration jäädvustada.

4. Uuesti lähtestada andmebaasi, mida saab suurt hulka andmeid testi vastutav kustutamiseks.


#### <a name="expected-comparison-results"></a>Oodatud võrdlustulemused

Meie-mälu katsed on näidanud **9 korda** jõudluse parandamiseks selle lihtsustatud töökoormus, kus töötab mõni Azure VM andmebaasi Azure sama piirkonna ostress.



<a id="install_analytics_manuallink" name="install_analytics_manuallink"></a>

&nbsp;


## <a name="b-install-the-in-memory-analytics-sample"></a>B. Installige In Memory Analytics näidis


Selles jaotises saate võrrelda tulemusi IO ja statistika columnstore register ja traditsiooniline b-puu index kasutamisel.


Reaalajas analytics on OLTP töökoormus, on sageli kõige paremini kasutada NONclustered columnstore indeks. Üksikasju vt [Columnstore registrite kirjeldatud](http://msdn.microsoft.com/library/gg492088.aspx).



### <a name="prepare-the-columnstore-analytics-test"></a>Ettevalmistused columnstore analytics test


1. Värske AdventureWorksLT andmebaasi loomine algusest valimi Azure portaali abil.
 - Kasutage seda täpne nimi.
 - Valige mis tahes Premium teenuse kiht.

2. [Sql_in-memory_analytics_sample](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/sql_in-memory_analytics_sample.sql) kopeerimine lõikelauale.
 - T-SQL-skript loob vajalikud mälu objektide AdventureWorksLT näidisandmebaasi lõite juhises 1.
 - Skripti loob tabelis dimensioonid ja kahe faktitabelite. Fact tabelid täidetakse 3,5 miljonit rida.
 - Skripti võib võtta 15 minutit.

3. T-SQL-i skripti kleepimine SSMS ja seejärel käivitada skripti.
 - Oluline on **COLUMNSTORE** märksõna kohta lause **CREATE INDEX** nimega:<br/>`CREATE NONCLUSTERED COLUMNSTORE INDEX ...;`

4. Määrata AdventureWorksLT ühilduvuse taseme 130.<br/>`ALTER DATABASE AdventureworksLT SET compatibility_level = 130;`
 - Taseme 130 on otseselt seotud-mälu funktsioonid. Kuid üldiselt pakub tase 130 kiirem päringu jõudluse kui 120.


#### <a name="crucial-tables-and-columnstore-indexes"></a>Oluline tabelite ja registrite columnstore


- dbo. FactResellerSalesXL_CCI on tabel, mis on rühmitatud **columnstore** indeks, mis on täiustatud tasemel *andmete* tihendamise.

- dbo. FactResellerSalesXL_PageCompressed on tabel, milles on samaväärne tavalise rühmitatud indeks, mis on tihendatud ainult *lehe* tasemel.


#### <a name="crucial-queries-to-compare-the-columnstore-index"></a>Oluline päringute võrdlemiseks columnstore register


[Siin](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/clustered_columnstore_sample_queries.sql) on mitu T-SQL-päringu käivitada jõudlustäiustusi kuvamiseks. Samm 2 T-SQL-i skripti, on paar päringud, mis on huvipunkti. Kahe päringu erinevad ainult ühele reale:


- `FROM FactResellerSalesXL_PageCompressed a`
- `FROM FactResellerSalesXL_CCI a`


Rühmitatud columnstore indeks on FactResellerSalesXL**_CCI** tabel.

Järgmised T-SQL-i skripti väljavõte prinditakse statistika IO ja kellaaeg iga tabeli päringu.


```
/*********************************************************************
Step 2 -- Overview
-- Page Compressed BTree table v/s Columnstore table performance differences
-- Enable actual Query Plan in order to see Plan differences when Executing
*/
-- Ensure Database is in 130 compatibility mode
ALTER DATABASE AdventureworksLT SET compatibility_level = 130
GO

-- Execute a typical query that joins the Fact Table with dimension tables
-- Note this query will run on the Page Compressed table, Note down the time
SET STATISTICS IO ON
SET STATISTICS TIME ON
GO

SELECT c.Year
    ,e.ProductCategoryKey
    ,FirstName + ' ' + LastName AS FullName
    ,count(SalesOrderNumber) AS NumSales
    ,sum(SalesAmount) AS TotalSalesAmt
    ,Avg(SalesAmount) AS AvgSalesAmt
    ,count(DISTINCT SalesOrderNumber) AS NumOrders
    ,count(DISTINCT a.CustomerKey) AS CountCustomers
FROM FactResellerSalesXL_PageCompressed a
INNER JOIN DimProduct b ON b.ProductKey = a.ProductKey
INNER JOIN DimCustomer d ON d.CustomerKey = a.CustomerKey
Inner JOIN DimProductSubCategory e on e.ProductSubcategoryKey = b.ProductSubcategoryKey
INNER JOIN DimDate c ON c.DateKey = a.OrderDateKey
WHERE e.ProductCategoryKey =2
    AND c.FullDateAlternateKey BETWEEN '1/1/2014' AND '1/1/2015'
GROUP BY e.ProductCategoryKey,c.Year,d.CustomerKey,d.FirstName,d.LastName
GO
SET STATISTICS IO OFF
SET STATISTICS TIME OFF
GO


-- This is the same Prior query on a table with a Clustered Columnstore index CCI 
-- The comparison numbers are even more dramatic the larger the table is, this is a 11 million row table only.
SET STATISTICS IO ON
SET STATISTICS TIME ON
GO
SELECT c.Year
    ,e.ProductCategoryKey
    ,FirstName + ' ' + LastName AS FullName
    ,count(SalesOrderNumber) AS NumSales
    ,sum(SalesAmount) AS TotalSalesAmt
    ,Avg(SalesAmount) AS AvgSalesAmt
    ,count(DISTINCT SalesOrderNumber) AS NumOrders
    ,count(DISTINCT a.CustomerKey) AS CountCustomers
FROM FactResellerSalesXL_CCI a
INNER JOIN DimProduct b ON b.ProductKey = a.ProductKey
INNER JOIN DimCustomer d ON d.CustomerKey = a.CustomerKey
Inner JOIN DimProductSubCategory e on e.ProductSubcategoryKey = b.ProductSubcategoryKey
INNER JOIN DimDate c ON c.DateKey = a.OrderDateKey
WHERE e.ProductCategoryKey =2
    AND c.FullDateAlternateKey BETWEEN '1/1/2014' AND '1/1/2015'
GROUP BY e.ProductCategoryKey,c.Year,d.CustomerKey,d.FirstName,d.LastName
GO

SET STATISTICS IO OFF
SET STATISTICS TIME OFF
GO
```



<a id="preview_considerations_for_in_memory" name="preview_considerations_for_in_memory"></a>


## <a name="preview-considerations-for-in-memory-oltp"></a>Eelvaate kaalutluste kohta-mälu OLTP


Azure'i SQL-andmebaasi In Memory OLTP funktsioonide sai [aktiivne 28 oktoober 2015 eelvaade](https://azure.microsoft.com/updates/public-preview-in-memory-oltp-and-real-time-operational-analytics-for-azure-sql-database/).


Praeguse eelvaates-mälu OLTP on toetatud ainult:

- Mis on *Premium* teenuse kiht andmebaasid.

- Andmebaaside, mis on loodud pärast In Memory OLTP funktsioonid sai aktiivne.
 - Uue andmebaasi ei toeta In Memory OLTP taastamisel andmebaasist, mis loodi enne In Memory OLTP funktsioonid sai aktiivne.


Kahtluse korral saate igal ajal käivitada järgmine T-SQL Valige kindlaks teha, kas teie andmebaasi toetab-mälu OLTP. Tulemus on **1** tähendab andmebaasi ei toeta-mälu OLTP.

```
SELECT DatabasePropertyEx(DB_NAME(), 'IsXTPSupported');
```


Kui päring tagastab väärtuse **1**,-mälu OLTP toetatakse selles andmebaasis ja mis tahes andmebaasi kopeerimine ja loodud andmebaasi taastamine põhjal see andmebaas.


#### <a name="objects-allowed-only-at-premium"></a>Objektid, mis on lubatud ainult Premium


Kui andmebaas sisaldab järgmist tüüpi-mälu OLTP objektide või tüübid, kasutuselevõttu teenuse kiht tavaline või Standard Premium andmebaasi ei toetata. Alandada andmebaasi, esmalt kukutage need objektid:

- Mälu optimeeritud tabelid
- Mälu optimeeritud tabeli tüübid
- Üldjuhul kompileeritud moodulid


#### <a name="other-relationships"></a>Muud seosed


- Eelvaate andmebaasidega elastne kaustadesse abil-mälu OLTP funktsioonid pole toetatud.
 - Andmebaasi, mis sisaldab või -mälu OLTP objektide pidi kohapeal on elastne teisaldamiseks tehke järgmist.
  - 1. Mis tahes mälu optimeeritud tabelid, tabeli tüübid ja üldjuhul kompileeritud T-SQL-i moodulid kukutage andmebaasis
  - 2. Teenuse kiht andmebaasi muutmine standardseks
  - 3. Andmebaasi viimine elastne pool

- Kasutades-mälu OLTP SQL-i andmebaas ei toetata.
 - Funktsiooni columnstore index-mälu Analytics on toetatud SQL-i andmebaas.

- Päringu pood ei jäädvustada sees algupäraselt koostatud moodulid päringud.

- Mõned Transact-SQL-i funktsioonid ei toetata-mälu OLTP abil. See kehtib nii Microsoft SQL Server ja Azure SQL-andmebaas. Lisateavet leiate teemast:
 - [Transact-SQL-mälu OLTP tugi](http://msdn.microsoft.com/library/dn133180.aspx)
 - [Transact-SQL-i importida-mälu OLTP ei toeta](http://msdn.microsoft.com/library/dn246937.aspx)


## <a name="next-steps"></a>Järgmised sammud


- Proovige [kasutamine-mälu OLTP olemasoleva SQL Azure'i rakenduses.](sql-database-in-memory-oltp-migration.md)


## <a name="additional-resources"></a>Lisaressursid

#### <a name="deeper-information"></a>Täpsemat teavet

- [Lisateavet-mälu OLTP, mis kehtib Microsoft SQL Server ja Azure SQL-andmebaas](http://msdn.microsoft.com/library/dn133186.aspx)

- [Lisateavet reaalajas funktsionaalseid Analytics MSDN-is](http://msdn.microsoft.com/library/dn817827.aspx)

- Valge raamat [levinud töökoormus mustrite ja migreerimine](http://msdn.microsoft.com/library/dn673538.aspx), kus kirjeldatakse töökoormus mustrite kui-mälu OLTP näeb tavaliselt olulise jõudluse tõstmiseks.

#### <a name="application-design"></a>Rakenduse kujundus

- [Mälu OLTP (-mälu optimeerimine)](http://msdn.microsoft.com/library/dn133186.aspx)

- [Kasutage-mälu OLTP olemasoleva SQL Azure'i rakenduses.](sql-database-in-memory-oltp-migration.md)

#### <a name="tools"></a>Tööriistad

- [SQL serveri andmete tööriistad eelvaade (SSDT)](http://msdn.microsoft.com/library/mt204009.aspx), iga kuu uusim versioon.

- [SQL serveri kordus Markup Language (RML) Utiliidid kirjeldus](http://support.microsoft.com/en-us/kb/944837)

- [Kuvari - mälu säilitamise](sql-database-in-memory-oltp-monitoring.md) mälu-OLTP.

