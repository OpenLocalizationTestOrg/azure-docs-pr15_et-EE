<properties
   pageTitle="Optimeerimine tehingud SQL-i andmebaas | Microsoft Azure'i"
   description="Parima tava juhised tehingu tõhusa värskendustega kirjalikult SQL Azure'i andmebaas"
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
   ms.date="07/31/2016"
   ms.author="jrj;barbkess"/>

# <a name="optimizing-transactions-for-sql-data-warehouse"></a>Optimeerimine tehingud SQL-andmebaas

Selles artiklis selgitatakse, kuidas optimeerida oma selgituseks kood jõudlusprobleeme minimeerimine risk pikk tagasipööramine.

## <a name="transactions-and-logging"></a>Tehingud ja logimine

Tehingud on oluline osa relatsiooniline andmebaasimootor. SQL-i andmebaas kasutab tehingud andmete muutmise ajal. Need tehingud võib olla otseselt või kaudselt. Ühe `INSERT`, `UPDATE` ja `DELETE` laused on peidetud tehingud kõik näited. Konkreetsete tehingute konkreetselt kirjutada, kasutades arendaja `BEGIN TRAN`, `COMMIT TRAN` või `ROLLBACK TRAN` ja kasutatakse tavaliselt kui vaja mitme muudatusega laused omavahel seotud ühe aatomi üksus. 

SQL Azure'i andmebaas paneb muudatused kasutades tehingute registrid andmebaasist. Iga jaotuse on eraldi tehingulogi. Tehingu log kirjutab on automaatne. On pole vaja konfigureerimine. Siiski samal ajal selle protsessi tagab kirjutada selle kehtestada on pea kohal süsteem. Saate minimeerida mõju, kirjutades ülekandega tõhusa kood. Ülekandega tõhusa kood jääb üldjoontes ühte kahest järgmisest kategooriast.

- Kasutada minimaalsete logimine importida võimaluse
- Andmete töötlemise vältida ainsuses pikaajalise tehingud jäävates pakettidena abil
- Üleminek mustri suurte muudatuste antud partition sektsiooni vastu

## <a name="minimal-vs-full-logging"></a>Minimaalne vs täielik logimine

Erinevalt täielikult logitud toimingud, mis tehingulogi abil silma peal iga rea muutmine, minimaalselt logitud toimingute ja silma peal hoida määral jaotuse ainult metaandmete muudatused. Seetõttu minimaalsete logimine hõlmab ainult tagasivõtmisel tehingu tõrke või selge taotluse korral nõutava teabe logimine (`ROLLBACK TRAN`). Kui palju vähem teavet jälitatakse tehingulogi, minimaalselt logitud toimingu teostab paremini sarnaselt suurusega täielikult logitud toimingu. Lisaks kuna vähem kirjutab siirdumiseks tehingulogi palju väiksema summa logiandmed luuakse ja nii on veel I/O tõhusa.

Tehingu turvalisuse piirangud kehtivad ainult täielikult logitud toiminguid.

>[AZURE.NOTE] Minimaalselt logitud toimingute saavad osaleda konkreetsete tehingud. Nagu kõik muutused eraldatud struktuurid jälgitakse, on võimalik minimaalselt logitud toimingute tagasi pöörata. See on oluline mõista muutmine logitakse "minimaalselt" pole un logitud.

## <a name="minimally-logged-operations"></a>Minimaalselt logitud toimingute

Järgmised toimingud on võimalik minimaalselt sisse loginud.

- Looge tabel AS valimine ([CTAS][])
- LISA. VALIGE
- REGISTRI LOOMINE
- LAUSED ALTER REGISTER TAASTADA
- RIPPLOENDI INDEX
- TABELI KÄRPIMINE
- TABELI KUSTUTAMINE
- LAUSED ALTER TABELI VAHETAMISE PARTITION

<!--
- MERGE
- UPDATE on LOB Types .WRITE
- SELECT..INTO
-->

>[AZURE.NOTE] Sisemise andmete liikumine toimingute (nt `BROADCAST` ja `SHUFFLE`) ei mõjuta turvalisuse limiit.

## <a name="minimal-logging-with-bulk-load"></a>Minimaalne hulgi koormusega logimine

`CTAS`ja `INSERT...SELECT` on mõlemad hulgiredigeerimiseks laadi toimingud. Siiski mõlemad mõjutavad target tabeli määratlus ja sõltuvad laadi stsenaariumi. Allpool on tabel, mis selgitab, kui teie hulgi toiming täielikult või minimaalselt logitakse:  

| Esmane Index               | Laadi stsenaarium                                            | Logimise režiim |
| --------------------------- | -------------------------------------------------------- | ------------ |
| Hunnik                        | Mis tahes                                                      | **Minimaalne**  |
| Rühmitatud indeks             | Tühja sihttabelisse                                       | **Minimaalne**  |
| Rühmitatud indeks             | Olemasolevate märkmikulehtede target ei kattu laaditud read | **Minimaalne**  |
| Rühmitatud indeks             | Olemasolevate märkmikulehtede target kattuvad laaditud read        | Täielik         |
| Rühmitatud Columnstore register | Partii suurus > = 102,400 partition joondatud jaotuse kohta. | **Minimaalne**  |
| Rühmitatud Columnstore register | Paketi suurus < 102,400 partition joondatud jaotuse kohta.  | Täielik         |

See märkimist, mis tahes kirjutab teisese või -rühmitatud registrite värskendamiseks alati on täielikult logitud toiminguid.

> [AZURE.IMPORTANT] SQL-i andmebaas on 60 jaotuse. Seetõttu, eeldades, et kõik read on ühtlaste ning kuvataks ühe sektsiooni, teie paketi peavad sisaldama 6,144,000 read või suurem logitakse minimaalselt kirjutamisel rühmitatud Columnstore register. Kui tabel on liigendatud ja read, mis hõlmavad partition piirmäärad, siis peate 6,144,000 ridade sektsiooni piiri eeldades isegi andmete jaotuse kohta. Iga sektsiooni iga jaotuse tohi sõltumatult 102,400 rea lävi olema minimaalselt logitud jaotuse sisestada.

Andmete laadimine on rühmitatud indeks tühi tabel võib sisaldada sageli segu täielikult logitud ja minimaalselt logitud read. Rühmitatud indeks on tasakaalustatud puu (b-puu) lehti. Kui lehe kirjutatakse juba sisaldab teise tehingu ridu, siis need kirjutab täielikult logitakse. Juhul, kui leht on tühi siis kirjutamine lehe minimaalselt logitakse.

## <a name="optimizing-deletes"></a>Kustutab optimeerimine

`DELETE`on täielikult logitud toimingu.  Kui teil on vaja suurt hulka andmeid tabeli või sektsiooni kustutada, on sageli mõttekas veel `SELECT` soovite alles jätta, andmed, mida saab käivitada nii minimaalselt logitud toimingu.  Selleks luua uue tabeli [CTAS][].  Kui loodud, abil [ümber NIMETADA][] vahetada välja vana tabeli vastloodud tabel.

```sql
-- Delete all sales transactions for Promotions except PromotionKey 2.

--Step 01. Create a new table select only the records we want to kep (PromotionKey 2)
CREATE TABLE [dbo].[FactInternetSales_d]
WITH
(   CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = HASH([ProductKey])
,   PARTITION   (   [OrderDateKey] RANGE RIGHT 
                                    FOR VALUES  (   20000101, 20010101, 20020101, 20030101, 20040101, 20050101
                                                ,   20060101, 20070101, 20080101, 20090101, 20100101, 20110101
                                                ,   20120101, 20130101, 20140101, 20150101, 20160101, 20170101
                                                ,   20180101, 20190101, 20200101, 20210101, 20220101, 20230101
                                                ,   20240101, 20250101, 20260101, 20270101, 20280101, 20290101
                                                )
)
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
WHERE   [PromotionKey] = 2
OPTION (LABEL = 'CTAS : Delete')
;

--Step 02. Rename the Tables to replace the 
RENAME OBJECT [dbo].[FactInternetSales]   TO [FactInternetSales_old];
RENAME OBJECT [dbo].[FactInternetSales_d] TO [FactInternetSales];
```

## <a name="optimizing-updates"></a>Värskenduste optimeerimine

`UPDATE`on täielikult logitud toimingu.  Kui teil on vaja värskendada suure hulga ridade tabelisse või sektsiooni sageli võib olla palju tõhusam, nt [CTAS][] minimaalselt logitud toimingu abil saate teha.

Täielik tabeli järgmises näites värskendus on teisendatud on `CTAS` nii, et minimaalsete logimine on võimalik.

Sel juhul tagasiulatuvalt lisame allahindluse summa tabelis müük:

```sql
--Step 01. Create a new table containing the "Update". 
CREATE TABLE [dbo].[FactInternetSales_u]
WITH
(   CLUSTERED INDEX
,   DISTRIBUTION = HASH([ProductKey])
,   PARTITION   (   [OrderDateKey] RANGE RIGHT 
                                    FOR VALUES  (   20000101, 20010101, 20020101, 20030101, 20040101, 20050101
                                                ,   20060101, 20070101, 20080101, 20090101, 20100101, 20110101
                                                ,   20120101, 20130101, 20140101, 20150101, 20160101, 20170101
                                                ,   20180101, 20190101, 20200101, 20210101, 20220101, 20230101
                                                ,   20240101, 20250101, 20260101, 20270101, 20280101, 20290101
                                                )
                )
)
AS 
SELECT
    [ProductKey]  
,   [OrderDateKey] 
,   [DueDateKey]  
,   [ShipDateKey] 
,   [CustomerKey] 
,   [PromotionKey] 
,   [CurrencyKey] 
,   [SalesTerritoryKey]
,   [SalesOrderNumber]
,   [SalesOrderLineNumber]
,   [RevisionNumber]
,   [OrderQuantity]
,   [UnitPrice]
,   [ExtendedAmount]
,   [UnitPriceDiscountPct]
,   ISNULL(CAST(5 as float),0) AS [DiscountAmount]
,   [ProductStandardCost]
,   [TotalProductCost]
,   ISNULL(CAST(CASE WHEN [SalesAmount] <=5 THEN 0
         ELSE [SalesAmount] - 5
         END AS MONEY),0) AS [SalesAmount]
,   [TaxAmt]
,   [Freight]
,   [CarrierTrackingNumber] 
,   [CustomerPONumber]
FROM    [dbo].[FactInternetSales]
OPTION (LABEL = 'CTAS : Update')
;

--Step 02. Rename the tables
RENAME OBJECT [dbo].[FactInternetSales]   TO [FactInternetSales_old];
RENAME OBJECT [dbo].[FactInternetSales_u] TO [FactInternetSales];

--Step 03. Drop the old table
DROP TABLE [dbo].[FactInternetSales_old]
```

> [AZURE.NOTE] Suurte tabelite uuesti luua saavad SQL-i andmebaas töökoormus halduse funktsioonide abil. Lisateabe saamiseks vaadake [kokkulangevus][] artikli jaotis töökoormus haldus.

## <a name="optimizing-with-partition-switching"></a>Optimeerimine koos partition vahetamine

Suuremahuliste muudatused sees [Tabel sektsioon][]sattunud teeb üleminek mustri sektsiooni mõttes palju. Kui andmete muutmist on oluline ja kestab mitu sektsiooni, siis lihtsalt iterating üle sektsioonid saavutab sama tulemuse.

Toiminguid, mida tuleb teha sektsiooni lüliti on järgmised:
1. Tühja välja sektsiooni loomine
2. 'Update' tegutsemine on CTAS
3. Olemasolevad andmed tabelisse välja aktiveerimine
4. Uute andmete vahetamine
5. Andmete puhastamiseks

Siiski, et tuvastada sektsioonid üle minna peame esmalt koostada helper protseduuri nagu allpool. 

```sql
CREATE PROCEDURE dbo.partition_data_get
    @schema_name           NVARCHAR(128)
,   @table_name            NVARCHAR(128)
,   @boundary_value        INT
AS
IF OBJECT_ID('tempdb..#ptn_data') IS NOT NULL
BEGIN
    DROP TABLE #ptn_data
END
CREATE TABLE #ptn_data
WITH    (   DISTRIBUTION = ROUND_ROBIN
        ,   HEAP
        )
AS
WITH CTE
AS
(
SELECT  s.name                          AS [schema_name]
,       t.name                          AS [table_name]
,       p.partition_number              AS [ptn_nmbr]
,       p.[rows]                        AS [ptn_rows]
,       CAST(r.[value] AS INT)          AS [boundary_value]
FROM        sys.schemas                 AS s
JOIN        sys.tables                  AS t    ON  s.[schema_id]       = t.[schema_id]
JOIN        sys.indexes                 AS i    ON  t.[object_id]       = i.[object_id]
JOIN        sys.partitions              AS p    ON  i.[object_id]       = p.[object_id] 
                                                AND i.[index_id]        = p.[index_id] 
JOIN        sys.partition_schemes       AS h    ON  i.[data_space_id]   = h.[data_space_id]
JOIN        sys.partition_functions     AS f    ON  h.[function_id]     = f.[function_id]
LEFT JOIN   sys.partition_range_values  AS r    ON  f.[function_id]     = r.[function_id] 
                                                AND r.[boundary_id]     = p.[partition_number]
WHERE i.[index_id] <= 1
)
SELECT  *
FROM    CTE
WHERE   [schema_name]       = @schema_name
AND     [table_name]        = @table_name
AND     [boundary_value]    = @boundary_value
OPTION (LABEL = 'dbo.partition_data_get : CTAS : #ptn_data')
;
GO
```

Selle toimingu maksimeerib koodi uuesti kasutada ja hoiab üleminek näide kompaktsema sektsiooni.

Alljärgnev kood näitab saavutamiseks üleminek tavapärase täielik sektsiooni ülalnimetatud viis juhiseid.

```sql
--Create a partitioned aligned empty table to switch out the data 
IF OBJECT_ID('[dbo].[FactInternetSales_out]') IS NOT NULL
BEGIN
    DROP TABLE [dbo].[FactInternetSales_out]
END

CREATE TABLE [dbo].[FactInternetSales_out]
WITH
(   DISTRIBUTION = HASH([ProductKey])
,   CLUSTERED COLUMNSTORE INDEX
,   PARTITION   (   [OrderDateKey] RANGE RIGHT 
                                    FOR VALUES  (   20020101, 20030101
                                                )
                )
)
AS
SELECT *
FROM    [dbo].[FactInternetSales]
WHERE 1=2
OPTION (LABEL = 'CTAS : Partition Switch IN : UPDATE')
;

--Create a partitioned aligned table and update the data in the select portion of the CTAS
IF OBJECT_ID('[dbo].[FactInternetSales_in]') IS NOT NULL
BEGIN
    DROP TABLE [dbo].[FactInternetSales_in]
END

CREATE TABLE [dbo].[FactInternetSales_in]
WITH
(   DISTRIBUTION = HASH([ProductKey])
,   CLUSTERED COLUMNSTORE INDEX
,   PARTITION   (   [OrderDateKey] RANGE RIGHT 
                                    FOR VALUES  (   20020101, 20030101
                                                )
                )
)
AS 
SELECT
    [ProductKey]  
,   [OrderDateKey] 
,   [DueDateKey]  
,   [ShipDateKey] 
,   [CustomerKey] 
,   [PromotionKey] 
,   [CurrencyKey] 
,   [SalesTerritoryKey]
,   [SalesOrderNumber]
,   [SalesOrderLineNumber]
,   [RevisionNumber]
,   [OrderQuantity]
,   [UnitPrice]
,   [ExtendedAmount]
,   [UnitPriceDiscountPct]
,   ISNULL(CAST(5 as float),0) AS [DiscountAmount]
,   [ProductStandardCost]
,   [TotalProductCost]
,   ISNULL(CAST(CASE WHEN [SalesAmount] <=5 THEN 0
         ELSE [SalesAmount] - 5
         END AS MONEY),0) AS [SalesAmount]
,   [TaxAmt]
,   [Freight]
,   [CarrierTrackingNumber] 
,   [CustomerPONumber]
FROM    [dbo].[FactInternetSales]
WHERE   OrderDateKey BETWEEN 20020101 AND 20021231
OPTION (LABEL = 'CTAS : Partition Switch IN : UPDATE')
;

--Use the helper procedure to identify the partitions
--The source table
EXEC dbo.partition_data_get 'dbo','FactInternetSales',20030101
DECLARE @ptn_nmbr_src INT = (SELECT ptn_nmbr FROM #ptn_data)
SELECT @ptn_nmbr_src

--The "in" table
EXEC dbo.partition_data_get 'dbo','FactInternetSales_in',20030101
DECLARE @ptn_nmbr_in INT = (SELECT ptn_nmbr FROM #ptn_data)
SELECT @ptn_nmbr_in

--The "out" table
EXEC dbo.partition_data_get 'dbo','FactInternetSales_out',20030101
DECLARE @ptn_nmbr_out INT = (SELECT ptn_nmbr FROM #ptn_data)
SELECT @ptn_nmbr_out

--Switch the partitions over
DECLARE @SQL NVARCHAR(4000) = '
ALTER TABLE [dbo].[FactInternetSales]   SWITCH PARTITION '+CAST(@ptn_nmbr_src AS VARCHAR(20))   +' TO [dbo].[FactInternetSales_out] PARTITION ' +CAST(@ptn_nmbr_out AS VARCHAR(20))+';
ALTER TABLE [dbo].[FactInternetSales_in] SWITCH PARTITION '+CAST(@ptn_nmbr_in AS VARCHAR(20))   +' TO [dbo].[FactInternetSales] PARTITION '     +CAST(@ptn_nmbr_src AS VARCHAR(20))+';'
EXEC sp_executesql @SQL

--Perform the clean-up
TRUNCATE TABLE dbo.FactInternetSales_out;
TRUNCATE TABLE dbo.FactInternetSales_in;

DROP TABLE dbo.FactInternetSales_out
DROP TABLE dbo.FactInternetSales_in
DROP TABLE #ptn_data
```

## <a name="minimize-logging-with-small-batches"></a>Logimise väikeste pakettidena minimeerimine

Suure andmete muutmist toimingute, siis oleks tükkideks või pakettidena piiritlemiseks üksuse töö toimingu jagada.

Allpool on esitatud heita pilgu järgmisele näitele. Paketi maht on seatud triviaalne arvu esiletõstmiseks tehnika. Tegelikult oleks märkimisväärselt suurem partii suurust. 

```sql
SET NO_COUNT ON;
IF OBJECT_ID('tempdb..#t') IS NOT NULL
BEGIN
    DROP TABLE #t;
    PRINT '#t dropped';
END

CREATE TABLE #t
WITH    (   DISTRIBUTION = ROUND_ROBIN
        ,   HEAP
        )
AS
SELECT  ROW_NUMBER() OVER(ORDER BY (SELECT NULL)) AS seq_nmbr
,       SalesOrderNumber
,       SalesOrderLineNumber
FROM    dbo.FactInternetSales
WHERE   [OrderDateKey] BETWEEN 20010101 and 20011231
;

DECLARE @seq_start      INT = 1
,       @batch_iterator INT = 1
,       @batch_size     INT = 50
,       @max_seq_nmbr   INT = (SELECT MAX(seq_nmbr) FROM dbo.#t)
;

DECLARE @batch_count    INT = (SELECT CEILING((@max_seq_nmbr*1.0)/@batch_size))
,       @seq_end        INT = @batch_size
;

SELECT COUNT(*)
FROM    dbo.FactInternetSales f

PRINT 'MAX_seq_nmbr '+CAST(@max_seq_nmbr AS VARCHAR(20))
PRINT 'MAX_Batch_count '+CAST(@batch_count AS VARCHAR(20))

WHILE   @batch_iterator <= @batch_count
BEGIN
    DELETE
    FROM    dbo.FactInternetSales
    WHERE EXISTS
    (
            SELECT  1
            FROM    #t t
            WHERE   seq_nmbr BETWEEN  @seq_start AND @seq_end
            AND     FactInternetSales.SalesOrderNumber      = t.SalesOrderNumber
            AND     FactInternetSales.SalesOrderLineNumber  = t.SalesOrderLineNumber
    )
    ;

    SET @seq_start = @seq_end
    SET @seq_end = (@seq_start+@batch_size);
    SET @batch_iterator +=1;
END
```

## <a name="pause-and-scaling-guidance"></a>Paus ja mastaapimist juhised

SQL Azure'i andmebaas võimaldab peatamiseks, jätkamiseks ja mastaapimiseks nõudmisel oma andmebaas. Kui peatamine või mastaapimiseks oma SQL-i andmebaas on oluline, et aru saada, mis tahes lennu tehingud lõpetatakse kohe; mis tahes avatud kande tagasi pööratud põhjustada. Kui teie töökoormus oli välja kaua töötama ning lõpetamata andmete muutmist enne selle toimingu paus või mastaabi, siis selle töö tuleb tegemata. Aega kulub peatamiseks või mastaapimiseks andmebaasi SQL Azure'i andmebaas võib mõjutada. 

> [AZURE.IMPORTANT] Mõlemad `UPDATE` ja `DELETE` on täielikult logitud toimingute ja nii, et need toimingu tagasivõtmine/uuestitegemine toiminguid saate teha märkimisväärselt oodatust kestvus minimaalselt logitud toiminguid. 

Parimal on lasta flight andmete muutmist tehingute lõpetamine enne pausi või skaleerimist SQL-i andmebaas. Kuid see ei pruugi alati olla praktiline. Pikk tagasipööramine vähendada, kaaluge üks järgmistest suvanditest:

- Uuesti kirjutada pikaajalise toimingute abil [CTAS][]
- Selle toimingu murda tükkideks; ridade alamhulga opsüsteem

## <a name="next-steps"></a>Järgmised sammud

Vaadake lisateavet eraldamise tasemed ja selgituseks piirangud [tehingute SQL-i andmebaas][] .  Muud head tavad ülevaate leiate teemast [SQL-i andmete ladu head tavad][].

<!--Image references-->

<!--Article references-->
[Tehingute SQL-andmebaas]: ./sql-data-warehouse-develop-transactions.md
[Tabel sektsioon]: ./sql-data-warehouse-tables-partition.md
[Kokkulangevus]: ./sql-data-warehouse-develop-concurrency.md
[CTAS]: ./sql-data-warehouse-develop-ctas.md
[SQL-i andmete ladu head tavad]: ./sql-data-warehouse-best-practices.md

<!--MSDN references-->
[alter index]:https://msdn.microsoft.com/library/ms188388.aspx
[ÜMBERNIMETAMINE]: https://msdn.microsoft.com/library/mt631611.aspx

<!-- Other web references -->

