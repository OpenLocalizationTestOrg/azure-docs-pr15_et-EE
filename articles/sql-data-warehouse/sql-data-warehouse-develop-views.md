<properties
   pageTitle="SQL-i andmebaas vaated | Microsoft Azure'i"
   description="Näpunäiteid Transact-SQL-i vaadete kasutamine SQL Azure'i andmebaas arendamise lahendusi."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="jrowlandjones"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="07/01/2016"
   ms.author="jrj;barbkess;sonyama"/>


# <a name="views-in-sql-data-warehouse"></a>Vaadete SQL-andmebaas

SQL-i andmebaas on eriti kasulik vaated. Neid saab kasutada mitmel erineval viisil teie lahendus kvaliteedi parandamiseks.  Selles artiklis tõstab esile rikastavad teie lahendus vaateid, samuti piirangud, mida tuleb arvesse mõned näited.

> [AZURE.NOTE] Süntaks `CREATE VIEW` pole selles artiklis kirjeldatud. Vaadake seda teavet MSDN-is artikkel [Loo vaade][] .

## <a name="architectural-abstraction"></a>Arhitektuuriline võtmiseks
Uuesti luua tabelite abil saate luua tabeli AS valida (CTAS), millele järgneb objekti ümbernimetamine mustri, samas kui andmete laadimine on väga levinud rakenduse mustri.

Järgmises näites lisab uue kuupäeva kirjete dimensiooni kuupäev. Pange tähele, kuidas uue lauad, DimDate_New, on loodud ja seejärel ümber asendamiseks tabeli algversiooni.

```sql
CREATE TABLE dbo.DimDate_New
WITH (DISTRIBUTION = ROUND_ROBIN
, CLUSTERED INDEX (DateKey ASC)
)
AS
SELECT *
FROM   dbo.DimDate  AS prod
UNION ALL
SELECT *
FROM   dbo.DimDate_stg AS stg
;

RENAME OBJECT DimDate TO DimDate_Old;
RENAME OBJECT DimDate_New TO DimDate;

```

Seda moodust võib põhjustada siiski tabelite ilmuvad ja kaovad kasutaja vaade, samuti tõrketeated "tabel pole olemas". Vaadete saab kasutajatele ühtsete esitluse kiht samas aluseks objektide ümber. Kasutajate juurdepääsu andmetele kaudu on pakkudes tähendab, et kasutajad ei pea olema nähtavus aluseks olevatest tabelitest. See pakub ühtsete kasutusvõimalused ning samal ajal tagada, et andmed ladu kujundajate saate arenevad andmemudeli abil CTAS ajal andmete laadimine protsessi jõudluse maksimeerimine.    

## <a name="performance-optimization"></a>Jõudluse optimeerimine
Vaadete võib kasutada ka jõustamiseks jõudluse optimeeritud ühenduste tabelite vahel. Näiteks vaate lisada liigsete jaotuse klahvi liitumist kriteeriumid andmete liikumine minimeerimiseks osana.  Vaate teine kasu võib olla jõustamine teatud päringu või liitumist vihje. Sel viisil vaadete kasutamine tagab ühenduste alati teostamise optimaalse viisil vältida kasutajad saavad ühenduste jaoks õige ehitada meeles pidada.

## <a name="limitations"></a>Piirangud
Vaadete SQL-i andmebaas on ainult metaandmete.  Seega pole saadaval järgmised suvandid:

-   Puudub võimalus skeemi sidumine
-   Base tabeleid ei saa värskendada vaadet
-   Vaadete ei saa luua üle Ajutised tabelid
-   Ei toeta jaoks Laienda / NOEXPAND näpunäiteid
-   SQL-i andmebaas on indekseeritud vaateid pole


## <a name="next-steps"></a>Järgmised sammud
Veel arengu näpunäiteid teemast [SQL-i andmebaas arengu ülevaade][].
Jaoks `CREATE VIEW` süntaks palun vaadake [Loo vaade][].

<!--Image references-->

<!--Article references-->
[SQL-i andmebaas arengu ülevaade]: ./sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[VAATE LOOMINE]: https://msdn.microsoft.com/en-us/library/ms187956.aspx

<!--Other Web references-->
