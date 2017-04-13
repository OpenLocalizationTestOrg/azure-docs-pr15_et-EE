<properties
   pageTitle="Looge tabel nagu valida (CTAS) SQL-i andmebaas | Microsoft Azure'i"
   description="Näpunäiteid kodeerimine Loo tabel (CTAS) lause select SQL Azure'i andmebaas nagu arendamise lahendusi."
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

# <a name="create-table-as-select-ctas-in-sql-data-warehouse"></a>Tabeli loomine nimega (CTAS) valige SQL-andmebaas
Valige tabeli loomine või `CTAS` on üks kõige olulisem T-SQL-i funktsioonid on saadaval. See on täielikult parallelized toiming, mis loob uue tabeli, mis põhineb väljund SELECT-lause. `CTAS`on lihtsaim ja kiireim võimalus luua tabeli koopia. Võite kaaluda seda ülelaadimisega versiooni `SELECT..INTO` kui soovite. Selles dokumendis on toodud nii näited ja head tavad `CTAS`.

## <a name="using-ctas-to-copy-a-table"></a>Tabeli kopeerimine CTAS abil

Võib-olla üks kõige levinum kasutab `CTAS` on tabeli eksemplari loomise, nii et saate muuta selle DDL. Kui näiteks olete loonud algse tabeli nimega `ROUND_ROBIN` ja nüüd soovite seda muuta jaotatud veeru, tabeli `CTAS` jaotuse veeru soovite muuta. `CTAS`saate kasutada ka eraldamine, indekseerimine või veeru muutmiseks.

Oletame, et see tabel vaikimisi jaotuse tüübi abil loodud `ROUND_ROBIN` jaotatud, kuna pole jaotuse veerg on määratud selle `CREATE TABLE`.

```sql
CREATE TABLE FactInternetSales
(
    ProductKey int NOT NULL,
    OrderDateKey int NOT NULL,
    DueDateKey int NOT NULL,
    ShipDateKey int NOT NULL,
    CustomerKey int NOT NULL,
    PromotionKey int NOT NULL,
    CurrencyKey int NOT NULL,
    SalesTerritoryKey int NOT NULL,
    SalesOrderNumber nvarchar(20) NOT NULL,
    SalesOrderLineNumber tinyint NOT NULL,
    RevisionNumber tinyint NOT NULL,
    OrderQuantity smallint NOT NULL,
    UnitPrice money NOT NULL,
    ExtendedAmount money NOT NULL,
    UnitPriceDiscountPct float NOT NULL,
    DiscountAmount float NOT NULL,
    ProductStandardCost money NOT NULL,
    TotalProductCost money NOT NULL,
    SalesAmount money NOT NULL,
    TaxAmt money NOT NULL,
    Freight money NOT NULL,
    CarrierTrackingNumber nvarchar(25),
    CustomerPONumber nvarchar(25)
);
```

Nüüd soovite luua uue eksemplari selle tabeli rühmitatud Columnstore indeks nii, et saate ära kasutada rühmitatud Columnstore tabelite jõudlus. Lisaks levitamisviisi selle tabeli ProductKey, kuna te näevad ühendused selles veerus ja soovite vältida andmete liikumise ajal ühenduste kohta ProductKey. Lõpuks ka lisatava eraldamine OrderDateKey kohta, et saate kiiresti kustutada vana andmete pukseerimine vana sektsioonid. Siin on CTAS lause, mille soovite oma vana tabeli kopeerimine uue tabeli.

```sql
CREATE TABLE FactInternetSales_new
WITH
(
    CLUSTERED COLUMNSTORE INDEX,
    DISTRIBUTION = HASH(ProductKey),
    PARTITION
    (
        OrderDateKey RANGE RIGHT FOR VALUES
        (
        20000101,20010101,20020101,20030101,20040101,20050101,20060101,20070101,20080101,20090101,
        20100101,20110101,20120101,20130101,20140101,20150101,20160101,20170101,20180101,20190101,
        20200101,20210101,20220101,20230101,20240101,20250101,20260101,20270101,20280101,20290101
        )
    )
)
AS SELECT * FROM FactInternetSales;
```

Lõpuks saate vahetada uue tabeli ja seejärel pukseerige vana tabeli tabelite ümber nimetada.

```sql
RENAME OBJECT FactInternetSales TO FactInternetSales_old;
RENAME OBJECT FactInternetSales_new TO FactInternetSales;

DROP TABLE FactInternetSales_old;
```

> [AZURE.NOTE] SQL Azure'i andmebaas ei veel tugiteenuste automaatne loomine või statistika värskendamise automaatselt.  Päringute parimaid tulemusi saada, on oluline, et kõik veerud kõigi tabelite luua statistika pärast esimest laadi või olulisi muudatuste andmeid.  Üksikasjalik selgitus statistika töötada rühma teemade teemat [statistika][] .

## <a name="using-ctas-to-work-around-unsupported-features"></a>Toetuseta funktsioonid lahendamiseks CTAS abil

`CTAS`saate kasutada ka lahendamiseks arvu Mittetoetatavaid funktsioone, mis on loetletud allpool. See saab sageli osutuda võit/win olukord mitte üksnes teie kood on nõuetele, kuid see on sageli käivitada kiiremini SQL-i andmebaas. See on tulemusena selle täielikult parallelized kujundus. Stsenaariumid, mida saab töötas ümber CTAS järgmised.

- VALIGE. RAKENDUSSE
- ANSI ühenduste värskendused
- ANSI-ühenduste kohta kustutab
- Lause ühendamine

> [AZURE.NOTE] Proovige mõelda "CTAS esimese". Kui arvate, et saate lahendada probleemi kasutades `CTAS` siis see on üldiselt lähenemine – see on parim viis ka siis, kui selle tulemusena kirjutatava rohkem andmeid.
>

## <a name="selectinto"></a>VALIGE. RAKENDUSSE
Võite leida `SELECT..INTO` kuvatakse teie lahendus kohtade arv.

Allpool on kujutatud on `SELECT..INTO` lause:

```sql
SELECT *
INTO    #tmp_fct
FROM    [dbo].[FactInternetSales]
```

Teisendada eespool `CTAS` on üsna otse edasi:

```sql
CREATE TABLE #tmp_fct
WITH
(
    DISTRIBUTION = ROUND_ROBIN
)
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
;
```

> [AZURE.NOTE] Praegu nõuab CTAS jaotuse veerg olema määratud.  Kui pole tahtlikult soovite jaotuse veergu, muuta oma `CTAS` täidab kõige kiiremini, kui valite jaotuse veerg, mis on sama, mis on aluseks olevat tabelit, nagu see strateegia vältida andmete liikumine.  Kui loote väikest tabeli, kus jõudluse ei ole, siis saate määrata `ROUND_ROBIN` vältimiseks saate otsustada, missugused veeru jaotuse.

## <a name="ansi-join-replacement-for-update-statements"></a>ANSI Liitu asendust update laused

Leiate keerukate värskenduse, mis ühendab rohkem kui kahte tabelit koos kasutades ANSI liitumise süntaks värskendamine või Kustuta.

Oletage, et oleksite selle tabeli värskendamiseks:

```sql
CREATE TABLE [dbo].[AnnualCategorySales]
(   [EnglishProductCategoryName]    NVARCHAR(50)    NOT NULL
,   [CalendarYear]                  SMALLINT        NOT NULL
,   [TotalSalesAmount]              MONEY           NOT NULL
)
WITH
(
    DISTRIBUTION = ROUND_ROBIN
)
;
```

Algne päring võib uurinud umbes selline:

```sql
UPDATE  acs
SET     [TotalSalesAmount] = [fis].[TotalSalesAmount]
FROM    [dbo].[AnnualCategorySales]     AS acs
JOIN    (
        SELECT  [EnglishProductCategoryName]
        ,       [CalendarYear]
        ,       SUM([SalesAmount])              AS [TotalSalesAmount]
        FROM    [dbo].[FactInternetSales]       AS s
        JOIN    [dbo].[DimDate]                 AS d    ON s.[OrderDateKey]             = d.[DateKey]
        JOIN    [dbo].[DimProduct]              AS p    ON s.[ProductKey]               = p.[ProductKey]
        JOIN    [dbo].[DimProductSubCategory]   AS u    ON p.[ProductSubcategoryKey]    = u.[ProductSubcategoryKey]
        JOIN    [dbo].[DimProductCategory]      AS c    ON u.[ProductCategoryKey]       = c.[ProductCategoryKey]
        WHERE   [CalendarYear] = 2004
        GROUP BY
                [EnglishProductCategoryName]
        ,       [CalendarYear]
        ) AS fis
ON  [acs].[EnglishProductCategoryName]  = [fis].[EnglishProductCategoryName]
AND [acs].[CalendarYear]                = [fis].[CalendarYear]
;
```

Kuna SQL-i andmebaas ei toeta ANSI liitub selle `FROM` punkti on `UPDATE` lause, ei saa kopeerida järgmine kood üle veidi muutmata.

Saate kasutada kombinatsiooni on `CTAS` ja on peidetud Liitu asendamiseks järgmine kood:

```sql
-- Create an interim table
CREATE TABLE CTAS_acs
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT  ISNULL(CAST([EnglishProductCategoryName] AS NVARCHAR(50)),0)    AS [EnglishProductCategoryName]
,       ISNULL(CAST([CalendarYear] AS SMALLINT),0)                      AS [CalendarYear]
,       ISNULL(CAST(SUM([SalesAmount]) AS MONEY),0)                     AS [TotalSalesAmount]
FROM    [dbo].[FactInternetSales]       AS s
JOIN    [dbo].[DimDate]                 AS d    ON s.[OrderDateKey]             = d.[DateKey]
JOIN    [dbo].[DimProduct]              AS p    ON s.[ProductKey]               = p.[ProductKey]
JOIN    [dbo].[DimProductSubCategory]   AS u    ON p.[ProductSubcategoryKey]    = u.[ProductSubcategoryKey]
JOIN    [dbo].[DimProductCategory]      AS c    ON u.[ProductCategoryKey]       = c.[ProductCategoryKey]
WHERE   [CalendarYear] = 2004
GROUP BY
        [EnglishProductCategoryName]
,       [CalendarYear]
;

-- Use an implicit join to perform the update
UPDATE  AnnualCategorySales
SET     AnnualCategorySales.TotalSalesAmount = CTAS_ACS.TotalSalesAmount
FROM    CTAS_acs
WHERE   CTAS_acs.[EnglishProductCategoryName] = AnnualCategorySales.[EnglishProductCategoryName]
AND     CTAS_acs.[CalendarYear]               = AnnualCategorySales.[CalendarYear]
;

--Drop the interim table
DROP TABLE CTAS_acs
;
```

## <a name="ansi-join-replacement-for-delete-statements"></a>ANSI Liitu asendust kustutamine laused
Mõnikord on parim viis, et andmete kustutamist on kasutada `CTAS`. Selle asemel, et kustutada andmed lihtsalt valige andmed, mida soovite säilitada. See kehtib eriti `DELETE` laused, mis kasutada ansi liitub liitumist süntaks, kuna ei toeta ANSI SQL-i andmebaas on `FROM` punkti on `DELETE` lause.

Näide teisendatud Kustuta lause on saadaval allpool:

```sql
CREATE TABLE dbo.DimProduct_upsert
WITH
(   Distribution=HASH(ProductKey)
,   CLUSTERED INDEX (ProductKey)
)
AS -- Select Data you wish to keep
SELECT     p.ProductKey
,          p.EnglishProductName
,          p.Color
FROM       dbo.DimProduct p
RIGHT JOIN dbo.stg_DimProduct s
ON         p.ProductKey = s.ProductKey
;

RENAME OBJECT dbo.DimProduct        TO DimProduct_old;
RENAME OBJECT dbo.DimProduct_upsert TO DimProduct;
```

## <a name="replace-merge-statements"></a>Ühenda laused asendamine
Ühenda laused saate asendada, vähemalt osa, kasutades `CTAS`. Saate konsolideerida soovitud `INSERT` ja `UPDATE` ühte aruandesse. Kõik kustutatud kirjed oleks vaja välja teise lauses sulgeda.

Näide on `UPSERT` on esitatud allpool:

```sql
CREATE TABLE dbo.[DimProduct_upsert]
WITH
(   DISTRIBUTION = HASH([ProductKey])
,   CLUSTERED INDEX ([ProductKey])
)
AS
-- New rows and new versions of rows
SELECT      s.[ProductKey]
,           s.[EnglishProductName]
,           s.[Color]
FROM      dbo.[stg_DimProduct] AS s
UNION ALL  
-- Keep rows that are not being touched
SELECT      p.[ProductKey]
,           p.[EnglishProductName]
,           p.[Color]
FROM      dbo.[DimProduct] AS p
WHERE NOT EXISTS
(   SELECT  *
    FROM    [dbo].[stg_DimProduct] s
    WHERE   s.[ProductKey] = p.[ProductKey]
)
;

RENAME OBJECT dbo.[DimProduct]          TO [DimProduct_old];
RENAME OBJECT dbo.[DimpProduct_upsert]  TO [DimProduct];

```

## <a name="ctas-recommendation-explicitly-state-data-type-and-nullability-of-output"></a>CTAS soovitus: selgesõnaliselt andmetüüp ja väljundi nullability

Kui migreerimine koodi võite leida käivitate üle seda tüüpi kodeerimine mustri:

```sql
DECLARE @d decimal(7,2) = 85.455
,       @f float(24)    = 85.455

CREATE TABLE result
(result DECIMAL(7,2) NOT NULL
)
WITH (DISTRIBUTION = ROUND_ROBIN)

INSERT INTO result
SELECT @d*@f
;
```

Instinktiivselt võib arvate järgmine kood tuleks migreerimine on CTAS ja oleks õige. Kuid on peidetud küsimus.

Järgmine kood ei anna sama tulemuse:

```sql
DECLARE @d decimal(7,2) = 85.455
,       @f float(24)    = 85.455
;

CREATE TABLE ctas_r
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT @d*@f as result
;
```

Pange tähele, et veerus "tulemus" kannab andmete tüübist ja nullability väärtused avaldise. See võib põhjustada väike dispersioonide väärtused, kui te ei ole ettevaatlik.

Proovige näiteks järgmine:

```sql
SELECT result,result*@d
from result
;

SELECT result,result*@d
from ctas_r
;
```

Väärtus, mis on talletatud tulemi jaoks on erinev. Nõutud väärtuse veerus tulemus on kasutatud teisi avaldisi muutub veelgi viga.

![][1]

See on eriti oluline andmete migratsioon. Kuigi teine päring on vaieldamatult Täpsem ilmneb probleeme. Andmete oleks erinevate võrreldes allika süsteem ja ja migreerimise küsimustele, mis viib. See on üks nende harva, kus "vale" vastus on tegelikult õige!

Põhjus, miks näeme, et see kahe tulemuse vaheline erinevus on peidetud tüüp valuahjud allapoole. Esimese näites määratleb tabeli veerus määratlus. Kuvatakse peidetud tüübi teisendamise ilmneb lisatakse rida. Teine näide on peidetud tüübi teisendamine pole nagu avaldise määratleb veeru andmetüüpi. Pange tähele ka, et teise näite veeru on määratletud NULLable veeru esimene näide ei ole. Tabeli loomise esimene näide veeru nullability on eraldi määratletud. Teine lihtsalt jäeti avaldis ja vaikimisi see näide tulemuseks NULL määratlus.  

Nende probleemide lahendamiseks tuleb seada otseselt tüübi teisendamine ja nullability klõpsake soovitud `SELECT` osa on `CTAS` lause. Neid atribuute ei saa seada tabeli loomine osa.

Järgmises näites näitab, kuidas lahendada kood:

```sql
DECLARE @d decimal(7,2) = 85.455
,       @f float(24)    = 85.455

CREATE TABLE ctas_r
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT ISNULL(CAST(@d*@f AS DECIMAL(7,2)),0) as result
```

Võtke arvesse järgmist.
- CAST või Teisenda oleks kasutatud
- ISNULL kasutatakse NULLability pole LIITUMISEGA jõustamine
- ISNULL on äärepoolseimate funktsioon
- Funktsiooni ISNULL teine osa on konstant ehk 0

> [AZURE.NOTE] Nullability olema õigesti seatud see on oluline kasutada `ISNULL` ja mitte `COALESCE`. `COALESCE`pole tarkadeks funktsioon ja seega Avaldise tulemus on alati NULLable. `ISNULL`erineb. See on tarkadeks. Seetõttu kui teine osa on `ISNULL` funktsioon on konstandi või mõne literaalmärgid siis on tulemiks oleva väärtuse pole NULL.

Näpunäide. see pole lihtsalt kasulikuks, et tagada teie arvutuste tagamine. See on oluline tabeli partition vahetada. Kujutage ette, peate selle tabeli, mis on määratletud teie fact:

```sql
CREATE TABLE [dbo].[Sales]
(
    [date]      INT     NOT NULL
,   [product]   INT     NOT NULL
,   [store]     INT     NOT NULL
,   [quantity]  INT     NOT NULL
,   [price]     MONEY   NOT NULL
,   [amount]    MONEY   NOT NULL
)
WITH
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101,20020101
                    ,20030101,20040101,20050101
                    )
                )
)
;
```

Välja väärtus on siiski arvutatud avaldis, mis pole osa lähteandmete.

Teie sektsioonitud andmekomplekti, võite selleks loomiseks tehke järgmist.

```sql
CREATE TABLE [dbo].[Sales_in]
WITH    
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101
                    )
                )
)
AS
SELECT
    [date]    
,   [product]
,   [store]
,   [quantity]
,   [price]   
,   [quantity]*[price]  AS [amount]
FROM [stg].[source]
OPTION (LABEL = 'CTAS : Partition IN table : Create')
;
```

Päringu läheks täiesti peene. Probleemi tuleb sooritada partition aktiveerimine. Tabeli määratlused ei vasta. Muuta selle CTAS vastavad tabeli määratlused on vaja muuta.

```sql
CREATE TABLE [dbo].[Sales_in]
WITH    
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101
                    )
                )
)
AS
SELECT
    [date]    
,   [product]
,   [store]
,   [quantity]
,   [price]   
,   ISNULL(CAST([quantity]*[price] AS MONEY),0) AS [amount]
FROM [stg].[source]
OPTION (LABEL = 'CTAS : Partition IN table : Create');
```

Seega saate vaadata hea engineering parima tava on tippige järjepidevuse ja nullability atribuutide lisamine CTAS haldamine. See aitab säilitada oma arvutusi ja ja ka tagab, et partition vahetamine on võimalik.

Palun [CTAS][]kasutamise kohta lisateabe saamiseks vaadake MSDN-i. See on üks kõige olulisemad laused rakenduses SQL Azure'i andmebaas. Veenduge, et te põhjalikult aru.

## <a name="next-steps"></a>Järgmised sammud
Veel arengu näpunäiteid teemast [arengu ülevaade][].

<!--Image references-->
[1]: media/sql-data-warehouse-develop-ctas/ctas-results.png

<!--Article references-->
[arengu ülevaade]: sql-data-warehouse-overview-develop.md
[Statistika]: ./sql-data-warehouse-tables-statistics.md

<!--MSDN references-->
[CTAS]: https://msdn.microsoft.com/library/mt204041.aspx

<!--Other Web references-->
