<properties
   pageTitle="SQL-i andmebaas suvandid Rühmita | Microsoft Azure'i"
   description="Näpunäiteid rakendamise jaotises Suvandid SQL Azure'i andmebaas arendamise lahendusi."
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
   ms.date="06/14/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="group-by-options-in-sql-data-warehouse"></a>Rühmitusalus suvandid SQL-andmebaas

Klausel [GROUP BY][] kasutatakse andmete liitmine Kokkuvõte kogumisse read. Samuti on mitu võimalust, mis see on funktsioone, mida on vaja töötas ümber, nagu nad ei toeta otseselt SQL Azure'i andmebaas laiendamine.

Need suvandid on
- GROUP BY koos funktsiooniga ROLLUP
- RÜHMITAMISE KOMPLEKTID
- GROUP BY Cube

## <a name="rollup-and-grouping-sets-options"></a>Rollup ja rühmitamine määrab suvandid
Lihtsaim võimalus siin on kasutada `UNION ALL` selle asemel funktsiooni rollup täitmiseks mitte konkreetsete süntaks tuginedes. Tulemus on täpselt sama

Allpool on kujutatud rühma lause abil soovitud `ROLLUP` suvand:

```sql
SELECT [SalesTerritoryCountry]
,      [SalesTerritoryRegion]
,      SUM(SalesAmount)             AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t       ON s.SalesTerritoryKey       = t.SalesTerritoryKey
GROUP BY ROLLUP (
                        [SalesTerritoryCountry]
                ,       [SalesTerritoryRegion]
                )
;
```

ROLLUP abil oleme taotlenud järgmised liitmised:
- Riigi ja regiooni
- Riik
- Kogusumma

Seda peate kasutama asendamiseks `UNION ALL`; määrata selle liitmised nõutav just teiega sama tulemuse:

```sql
SELECT [SalesTerritoryCountry]
,      [SalesTerritoryRegion]
,      SUM(SalesAmount) AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t     ON s.SalesTerritoryKey       = t.SalesTerritoryKey
GROUP BY
       [SalesTerritoryCountry]
,      [SalesTerritoryRegion]
UNION ALL
SELECT [SalesTerritoryCountry]
,      NULL
,      SUM(SalesAmount) AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t     ON s.SalesTerritoryKey       = t.SalesTerritoryKey
GROUP BY
       [SalesTerritoryCountry]
UNION ALL
SELECT NULL
,      NULL
,      SUM(SalesAmount) AS TotalSalesAmount
FROM  dbo.factInternetSales s
JOIN  dbo.DimSalesTerritory t     ON s.SalesTerritoryKey       = t.SalesTerritoryKey;
```

RÜHMITUS SEAB kõik läheb vaja teha on sama peamine vastu, aga Liidu kõik jaotised koondamine tasemete soovime näha ainult loomine

## <a name="cube-options"></a>Kuubi suvandid
On võimalik luua rühma, kus kuubi UNION ALL lähenemine. Probleem on koodi saate kiiresti muutuvad tülikas ja kohmakas. Selle abil saate seda lähenemisviisi veel täpsemalt.

Ülaltoodud näites kasutame.

Kõigepealt tuleb määratleda 'kuubi, mis määratleb kõikide tasemete puhul liitmiseks, mille soovime luua. See on oluline arvestama CROSS liituda kaks tuletatud tabelite. See loob meile kõikide tasemete puhul. Ülejäänud kood on tõesti seal vormingut.

```sql
CREATE TABLE #Cube
WITH
(   DISTRIBUTION = ROUND_ROBIN
,   LOCATION = USER_DB
)
AS
WITH GrpCube AS
(SELECT    CAST(ISNULL(Country,'NULL')+','+ISNULL(Region,'NULL') AS NVARCHAR(50)) as 'Cols'
,          CAST(ISNULL(Country+',','')+ISNULL(Region,'') AS NVARCHAR(50))  as 'GroupBy'
,          ROW_NUMBER() OVER (ORDER BY Country) as 'Seq'
FROM       ( SELECT 'SalesTerritoryCountry' as Country
             UNION ALL
             SELECT NULL
           ) c
CROSS JOIN ( SELECT 'SalesTerritoryRegion' as Region
             UNION ALL
             SELECT NULL
           ) r
)
SELECT Cols
,      CASE WHEN SUBSTRING(GroupBy,LEN(GroupBy),1) = ','
            THEN SUBSTRING(GroupBy,1,LEN(GroupBy)-1)
            ELSE GroupBy
       END AS GroupBy  --Remove Trailing Comma
,Seq
FROM GrpCube;
```

Tulemused kuvatakse CTAS võib näha allpool:

![][1]

Teise etapi on määratud sihttabelisse ajutised tulemite talletamiseks.

```sql
DECLARE
 @SQL NVARCHAR(4000)
,@Columns NVARCHAR(4000)
,@GroupBy NVARCHAR(4000)
,@i INT = 1
,@nbr INT = 0
;
CREATE TABLE #Results
(
 [SalesTerritoryCountry] NVARCHAR(50)
,[SalesTerritoryRegion]  NVARCHAR(50)
,[TotalSalesAmount]      MONEY
)
WITH
(   DISTRIBUTION = ROUND_ROBIN
,   LOCATION = USER_DB
)
;
```

Pidev üle meie kuubi veergude liitmise läbimiseks on kolmas toiming. Päring kestab üks kord #Cube ajutise tabeli iga rea jaoks ja talletage #Results temp tabelis

```sql
SET @nbr =(SELECT MAX(Seq) FROM #Cube);

WHILE @i<=@nbr
BEGIN
    SET @Columns = (SELECT Cols    FROM #Cube where seq = @i);
    SET @GroupBy = (SELECT GroupBy FROM #Cube where seq = @i);

    SET @SQL ='INSERT INTO #Results
              SELECT '+@Columns+'
              ,      SUM(SalesAmount) AS TotalSalesAmount
              FROM  dbo.factInternetSales s
              JOIN  dbo.DimSalesTerritory t  
              ON s.SalesTerritoryKey = t.SalesTerritoryKey
              '+CASE WHEN @GroupBy <>''
                     THEN 'GROUP BY '+@GroupBy ELSE '' END

    EXEC sp_executesql @SQL;
    SET @i +=1;
END
```

Lõpuks me võib tagastada tulemused lihtsalt lugemise #Results ajutine tabelist

```sql
SELECT *
FROM #Results
ORDER BY 1,2,3
;
```

Koodi osadeks jaotistesse ja luues Silmukoiminen ehitada koodi muutub mõistliku ja hooldatav.


## <a name="next-steps"></a>Järgmised sammud
Veel arengu näpunäiteid teemast [arengu ülevaade][].

<!--Image references-->
[1]: media/sql-data-warehouse-develop-group-by-options/sql-data-warehouse-develop-group-by-cube.png

<!--Article references-->
[arengu ülevaade]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[RÜHMITUSALUS]: https://msdn.microsoft.com/library/ms177673.aspx


<!--Other Web references-->
