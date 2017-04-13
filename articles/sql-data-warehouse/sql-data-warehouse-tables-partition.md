<properties
   pageTitle="Tabelid SQL-i andmebaas jagamine | Microsoft Azure'i"
   description="Alustamine tabeli eraldamine rakenduses SQL Azure'i andmebaas."
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
   ms.date="07/18/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="partitioning-tables-in-sql-data-warehouse"></a>Eraldamine tabelid SQL-andmebaas

> [AZURE.SELECTOR]
- [Ülevaade][]
- [Andmetüübid][]
- [Levitamine][]
- [Index][]
- [Partition][]
- [Statistika][]
- [Ajutiste][]

Eraldamine on toetatud SQL-i andmebaas tabeli kõigi; sh rühmitatud columnstore, rühmitatud indeks ja hunnik.  Eraldamine toetatakse ka kõik jaotuse tüübid, sh round jaan jaotatud ja tuli.  Eraldamine võimaldab teil oma andmeid jagada väiksemate rühmade andmete ja enamasti eraldamine tehakse veeru kuupäev.

## <a name="benefits-of-partitioning"></a>Eraldamine eelised

Eraldamine saavad andmete hooldus ja päringu jõudluse.  Kas see on kasulik nii või ainult ühe sõltub kuidas andmed on laaditud ja kas sama veeru saab kasutada nii eesmärgil, kuna eraldamine saab teha ainult ühe veeru.

### <a name="benefits-to-loads"></a>Laadimise kasu

Peamine kasu eraldamine rakenduses SQL-i andmebaas on parandada tõhusust ja laadimise andmeid kasutada partition kustutamise, üleminek ja ühendamine.  Enamikul juhtudel on liigendatud andmete veeru kuupäev, mis on lähedalt seotud järjestus, kus andmed on laaditud andmebaasi kohta.  Üks suurimaid eeliseid sektsioonid abil andmeid säilitada selle tehingu logimine vältimine.  Kuigi lihtsalt lisamine, värskendamine või andmete kustutamist võib olla kõige lihtsam lahendus, vähe mõtte ja vaeva, kasutades eraldamine käigus oma laadi oluliselt parandab jõudlust.

Sektsiooni üleminek saab kiiresti eemaldada või asendada tabeli osa.  Näiteks müügi faktitabeli võivad sisaldada ainult andmeid 36 kuud.  Iga kuu lõpu seisuga kustutatakse vanimast kuu müügiandmetega tabelist.  Kustuta lause abil andmete kustutamiseks vanimast kuu saanud andmed kustutada.  Siiski suurt hulka andmeid rea-,-rea kustutamine lause kustutamine saate võtab väga kaua aega, kui ka loomine suurte tehingud, mida võib võtta kaua aega tagasivõtmisel kui riski.  Optimaalne lähenemine on lihtsalt langev vanimast partition andmeid.  Kui üksikute ridade kustutamine võib kuluda tundi, on kogu sektsiooni kustutamine võib kuluda sekundit.

### <a name="benefits-to-queries"></a>Päringute kasu

Eraldamine saate kasutada ka päringu jõudluse parandamiseks.  Kui päringu rakendab filtri sektsioonitud veergu, seda piirata skannimise ainult nõuetele sektsioonid, mis võib olla väiksemate alamhulk, andmete vältimiseks täielik kontrollima.  Rühmitatud columnstore registrid kehtestamine põhiliste kõrvaldamiseks jõudluse kasu on vähem kasulik, kuid mõnel juhul võib olla kasu päringud.  Näiteks kui müügi asjaolu tabelis on liigendatud müügikuupäev välja 36 kuudeks, siis päringute filtri müük kuupäeval vahele jätta otsimine sektsioonid, mis ei vasta filter.

## <a name="partition-sizing-guidance"></a>Sektsiooni sizing juhised

Mõnel juhul jõudluse parandamiseks saate kasutada eraldamine, tabeli loomise **liiga palju** sektsioonide võib kahjustada jõudluse teatud tingimustel.  Need probleemid on eriti rühmitatud columnstore tabelite jaoks.  Jagamine abiks olla, on oluline mõista, millal kasutada eraldamine ja sektsioonide loomine arv.  Ei ole raske kiiresti reegel selle kohta, kui palju sektsioonid on liiga palju, see sõltub teie andmed ja kui palju partitions laadimise abil korraga.  Kuid üldine rusikareegel arvates lisamise 10s 100s sektsioonide, mitte 1000s.

**Rühmitatud columnstore** tabelites eraldamine loomisel on oluline silmas pidada, kui palju maa on iga partition.  Optimaalse tihendamise ja rühmitatud columnstore tabelid, on vaja vähemalt 1 miljoni rea jaotuse ja sektsioon.  Enne sektsioonid on loodud, SQL-i andmebaas juba jagab iga tabeli 60 jaotatud andmebaasid.  Tabel lisatakse eraldamine on lisaks jaotuse, mis on loodud taustal.  Selles näites kasutamine, kui sisalduvad tabeli müük fact 36 kuu sektsioonid ja et SQL-i andmebaas 60 jaotuse, siis müügi asjaolu tabelis peaks sisaldama 60 miljonit ridade kuus või 2,1 miljardit read kui kõik kuud täidetakse.  Kui tabel sisaldab märkimisväärselt vähem kui soovitatav minimaalne partition ridade arv ridu, kaaluge vähem sektsioonid selleks, et suurendada partition ridade arvu.  Ka artiklist [indekseerimise][Index] , mis sisaldab päringuid, mis töötab SQL-i andmebaas kobar columnstore registrite kvaliteedi hindamiseks.

## <a name="syntax-difference-from-sql-server"></a>Süntaksi erinevus SQL serverist

SQL-i andmebaas tutvustab lihtsustatud määratluse sektsioonid, mis on veidi teistsugused SQL serverist.  Eraldamine funktsioonid ja skeemid ei kasutata SQL-i andmebaas nagu need on SQL Server.  Selle asemel tegemiseks on vaja sektsioonitud veerg ja piiri punktide tuvastamine.  Kuigi süntaks eraldamine võib olla veidi teistsugune SQL serveri, põhialused on samad.  SQL Server ja SQL-i andmebaas toetavad ühe sektsiooni veeru tabeli, mis võib olla jäi sektsiooni kohta.  Eraldamine kohta leiate lisateavet teemast [liigendatud tabelite ja registrite][].

Selle all SQL-i andmebaas liigendatud [tabeli loomine][] lause näiteks piirded FactInternetSales tabeli OrderDateKey veerus:

```sql
CREATE TABLE [dbo].[FactInternetSales]
(
    [ProductKey]            int          NOT NULL
,   [OrderDateKey]          int          NOT NULL
,   [CustomerKey]           int          NOT NULL
,   [PromotionKey]          int          NOT NULL
,   [SalesOrderNumber]      nvarchar(20) NOT NULL
,   [OrderQuantity]         smallint     NOT NULL
,   [UnitPrice]             money        NOT NULL
,   [SalesAmount]           money        NOT NULL
)
WITH
(   CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = HASH([ProductKey])
,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                    (20000101,20010101,20020101
                    ,20030101,20040101,20050101
                    )
                )
)
;
```

## <a name="migrating-partitioning-from-sql-server"></a>Migreerimine eraldamine SQL serverist

Migreerimine SQL serveri partition määratlused lihtsalt SQL-i andmebaas:

- SQL serveri [sektsiooni kava][]kõrvaldamiseks.
- [Funktsioon partition][] määratluse lisada tabeli loomine.

Kui migreerimise sektsioonitud tabeli SQL serveri eksemplar on SQL-i all aitavad teil arv ridu, mis on iga partition kuulama.  Pidage meeles, et sama eraldamine granulaarsus kasutamisel SQL-i andmebaas kohta sektsiooni ridade arv väheneb 60 tegurit.  

```sql
-- Partition information for a SQL Server Database
SELECT      s.[name]                        AS      [schema_name]
,           t.[name]                        AS      [table_name]
,           i.[name]                        AS      [index_name]
,           p.[partition_number]            AS      [partition_number]
,           SUM(a.[used_pages]*8.0)         AS      [partition_size_kb]
,           SUM(a.[used_pages]*8.0)/1024    AS      [partition_size_mb]
,           SUM(a.[used_pages]*8.0)/1048576 AS      [partition_size_gb]
,           p.[rows]                        AS      [partition_row_count]
,           rv.[value]                      AS      [partition_boundary_value]
,           p.[data_compression_desc]       AS      [partition_compression_desc]
FROM        sys.schemas s
JOIN        sys.tables t                    ON      t.[schema_id]         = s.[schema_id]
JOIN        sys.partitions p                ON      p.[object_id]         = t.[object_id]
JOIN        sys.allocation_units a          ON      a.[container_id]      = p.[partition_id]
JOIN        sys.indexes i                   ON      i.[object_id]         = p.[object_id]
                                            AND     i.[index_id]          = p.[index_id]
JOIN        sys.data_spaces ds              ON      ds.[data_space_id]    = i.[data_space_id]
LEFT JOIN   sys.partition_schemes ps        ON      ps.[data_space_id]    = ds.[data_space_id]
LEFT JOIN   sys.partition_functions pf      ON      pf.[function_id]      = ps.[function_id]
LEFT JOIN   sys.partition_range_values rv   ON      rv.[function_id]      = pf.[function_id]
                                            AND     rv.[boundary_id]      = p.[partition_number]
WHERE       p.[index_id] <=1
GROUP BY    s.[name]
,           t.[name]
,           i.[name]
,           p.[partition_number]
,           p.[rows]
,           rv.[value]
,           p.[data_compression_desc]
;
```

## <a name="workload-management"></a>Töökoormus haldus

Ühe tabeli partition otsust tegur lõplik tükk tähelepanu on [töökoormus haldus][].  Töökoormus haldus SQL-i andmebaas on peamiselt mälu ja kokkulangevus.  SQL-i andmebaas on eraldatud iga jaotuse päringu käitamise ajal suurim lubatud mälu reguleeritakse ressursi tunnid.  Ideaalvariandis mitte on teie sektsioonid suurusega, võttes arvesse ka muud tegurid nagu mälu vajadustele koostamise rühmitatud columnstore registrid.  Rühmitatud columnstore registrid kasu oluliselt, kui nad on eraldatud rohkem mälu.  Seetõttu, mida soovite tagada, et partition register taastada on näljane mälu. Päringu mälu suurendades saavutamiseks üleminek vaikimisi roll, smallrc, üks nagu largerc rollid.

Teave on eraldatud mälu jaotuse kohta on saadaval päringute ressursi kuberner dünaamilise halduse vaadete. Tegelikult teie mälu andmine olema väiksem kui arvud. Siiski see pakub juhised, mille abil saate oma sektsioonid andmete haldamise toimingute sortimisel tasemele.  Proovige vältida oma sektsioonid Lisaks mälu andmine, mis on esitatud suurtele ressursside klassi suurus. Kui teie sektsioonid kasvada üle sellel joonisel käivitate mälu, mis omakorda vähem optimaalse tihendamise riski.

```sql
SELECT  rp.[name]                               AS [pool_name]
,       rp.[max_memory_kb]                      AS [max_memory_kb]
,       rp.[max_memory_kb]/1024                 AS [max_memory_mb]
,       rp.[max_memory_kb]/1048576              AS [mex_memory_gb]
,       rp.[max_memory_percent]                 AS [max_memory_percent]
,       wg.[name]                               AS [group_name]
,       wg.[importance]                         AS [group_importance]
,       wg.[request_max_memory_grant_percent]   AS [request_max_memory_grant_percent]
FROM    sys.dm_pdw_nodes_resource_governor_workload_groups  wg
JOIN    sys.dm_pdw_nodes_resource_governor_resource_pools   rp ON wg.[pool_id] = rp.[pool_id]
WHERE   wg.[name] like 'SloDWGroup%'
AND     rp.[name]    = 'SloDWPool'
;
```

## <a name="partition-switching"></a>Sektsiooni vahetamine

SQL-i andmebaas toetab partition tükeldamise, ühendamise ja vahetamine. Kõik need funktsioonid on excuted lause [ALTER TABLE][] abil.

Sektsioonid kahe tabeli vahel liikumiseks peate veenduma, et sektsioonid ühtiks nende vastavate piirmäärad ja, et tabel määratlused vastavad. Nagu sisse piiranguid pole saadaval jõustamiseks tabelis olevaid väärtusi vahemikus lähtetabel peab sisaldama sama nimega sihttabelisse sektsiooni piirid. Kui see ei ole, siis partition aktiveerimine nurjub nagu partition metaandmete ei sünkroonita.

### <a name="how-to-split-a-partition-that-contains-data"></a>Kuidas sektsioon, mis sisaldab andmeid, jagada

Kõige tõhusam meetod tükeldamiseks partition, mis sisaldab juba andmeid saab kasutada mõnda `CTAS` lause. Kui sektsioonitud tabel on rühmitatud columnstore seejärel tabel sektsioon peavad olema tühjad saab tükeldada.

Allpool on valimi sektsioonitud columnstore tabel, mis sisaldab ühe rea iga sektsiooni:

```sql
CREATE TABLE [dbo].[FactInternetSales]
(
        [ProductKey]            int          NOT NULL
    ,   [OrderDateKey]          int          NOT NULL
    ,   [CustomerKey]           int          NOT NULL
    ,   [PromotionKey]          int          NOT NULL
    ,   [SalesOrderNumber]      nvarchar(20) NOT NULL
    ,   [OrderQuantity]         smallint     NOT NULL
    ,   [UnitPrice]             money        NOT NULL
    ,   [SalesAmount]           money        NOT NULL
)
WITH
(   CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = HASH([ProductKey])
,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                    (20000101
                    )
                )
)
;

INSERT INTO dbo.FactInternetSales
VALUES (1,19990101,1,1,1,1,1,1);
INSERT INTO dbo.FactInternetSales
VALUES (1,20000101,1,1,1,1,1,1);


CREATE STATISTICS Stat_dbo_FactInternetSales_OrderDateKey ON dbo.FactInternetSales(OrderDateKey);
```

> [AZURE.NOTE] Statistiline objekti loomisega me veenduge, et selle tabeli metaandmete on täpsem. Kui me välja loomise statistika, siis SQL-i andmebaas kasutatakse vaikeväärtused. Lisateavet statistika palun vaadake üle [statistika][].

Oleme seejärel saate päringu rida count kasutamise soovitud `sys.partitions` kataloogi vaade:

```sql
SELECT  QUOTENAME(s.[name])+'.'+QUOTENAME(t.[name]) as Table_name
,       i.[name] as Index_name
,       p.partition_number as Partition_nmbr
,       p.[rows] as Row_count
,       p.[data_compression_desc] as Data_Compression_desc
FROM    sys.partitions p
JOIN    sys.tables     t    ON    p.[object_id]   = t.[object_id]
JOIN    sys.schemas    s    ON    t.[schema_id]   = s.[schema_id]
JOIN    sys.indexes    i    ON    p.[object_id]   = i.[object_Id]
                            AND   p.[index_Id]    = i.[index_Id]
WHERE t.[name] = 'FactInternetSales'
;
```

Kui me selles tabelis tükeldamine, saame tõrge:

```sql
ALTER TABLE FactInternetSales SPLIT RANGE (20010101);
```

Sõnum 35346, tase 15, riigi 1 rea 44 TÜKELDAMINE klausel lause ALTER PARTITION nurjus, kuna sektsiooni pole tühi.  Ainult tühja sektsioonid saate tükeldada sisse, kui tabel on olemas columnstore register. Kaaluge keelamine columnstore register enne lause ALTER PARTITION emissiooni ja seejärel columnstore registri taastamine, kui muuta PARTITION on lõpule jõudnud.

Siiski saate kasutame `CTAS` meie andmete mahutamiseks uue tabeli loomine.

```sql
CREATE TABLE dbo.FactInternetSales_20000101
    WITH    (   DISTRIBUTION = HASH(ProductKey)
            ,   CLUSTERED COLUMNSTORE INDEX
            ,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                                (20000101
                                )
                            )
            )
AS
SELECT *
FROM    FactInternetSales
WHERE   1=2
;
```

Nagu partition piirmäärad joondatud lülitit on lubatud. See jätab lähtetabel tühjade sektsiooni, mida me saame hiljem jagada.

```sql
ALTER TABLE FactInternetSales SWITCH PARTITION 2 TO  FactInternetSales_20000101 PARTITION 2;

ALTER TABLE FactInternetSales SPLIT RANGE (20010101);
```

Kõik, mis on jäänud teha on meie uued sektsiooni piirid abil andmete joondamiseks `CTAS` ja meie andmed uuesti põhitabeli vahetamine

```sql
CREATE TABLE [dbo].[FactInternetSales_20000101_20010101]
    WITH    (   DISTRIBUTION = HASH([ProductKey])
            ,   CLUSTERED COLUMNSTORE INDEX
            ,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                                (20000101,20010101
                                )
                            )
            )
AS
SELECT  *
FROM    [dbo].[FactInternetSales_20000101]
WHERE   [OrderDateKey] >= 20000101
AND     [OrderDateKey] <  20010101
;

ALTER TABLE dbo.FactInternetSales_20000101_20010101 SWITCH PARTITION 2 TO dbo.FactInternetSales PARTITION 2;
```

Kui olete lõpetanud andmete liikumine on mõistlik tagada need kajastavad täpselt uue jaotuse nende vastavate sektsioonid andmed sihttabelisse statistika värskendamine:

```sql
UPDATE STATISTICS [dbo].[FactInternetSales];
```

### <a name="table-partitioning-source-control"></a>Tabeli eraldamine Juhtelemendi allikas

Tabeli definitsiooni **roostetamise** vältimiseks andmeallika juhtelemendi süsteemis soovite võtke arvesse järgmist lähenemisviisi:

1. Tabeli loomine sektsioonitud tabelina, kuid ei sisalda väärtusi partition

```sql
CREATE TABLE [dbo].[FactInternetSales]
(
    [ProductKey]            int          NOT NULL
,   [OrderDateKey]          int          NOT NULL
,   [CustomerKey]           int          NOT NULL
,   [PromotionKey]          int          NOT NULL
,   [SalesOrderNumber]      nvarchar(20) NOT NULL
,   [OrderQuantity]         smallint     NOT NULL
,   [UnitPrice]             money        NOT NULL
,   [SalesAmount]           money        NOT NULL
)
WITH
(   CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = HASH([ProductKey])
,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                    ()
                )
)
;
```

2. `SPLIT`tabeli juurutamise käigus:

```sql
-- Create a table containing the partition boundaries

CREATE TABLE #partitions
WITH
(
    LOCATION = USER_DB
,   DISTRIBUTION = HASH(ptn_no)
)
AS
SELECT  ptn_no
,       ROW_NUMBER() OVER (ORDER BY (ptn_no)) as seq_no
FROM    (
        SELECT CAST(20000101 AS INT) ptn_no
        UNION ALL
        SELECT CAST(20010101 AS INT)
        UNION ALL
        SELECT CAST(20020101 AS INT)
        UNION ALL
        SELECT CAST(20030101 AS INT)
        UNION ALL
        SELECT CAST(20040101 AS INT)
        ) a
;

-- Iterate over the partition boundaries and split the table

DECLARE @c INT = (SELECT COUNT(*) FROM #partitions)
,       @i INT = 1                                 --iterator for while loop
,       @q NVARCHAR(4000)                          --query
,       @p NVARCHAR(20)     = N''                  --partition_number
,       @s NVARCHAR(128)    = N'dbo'               --schema
,       @t NVARCHAR(128)    = N'FactInternetSales' --table
;

WHILE @i <= @c
BEGIN
    SET @p = (SELECT ptn_no FROM #partitions WHERE seq_no = @i);
    SET @q = (SELECT N'ALTER TABLE '+@s+N'.'+@t+N' SPLIT RANGE ('+@p+N');');

    -- PRINT @q;
    EXECUTE sp_executesql @q;

    SET @i+=1;
END

-- Code clean-up

DROP TABLE #partitions;
```

See lähenemine andmeallika juhtelemendi kood jääb staatilise eraldamine piiri väärtused on lubada dünaamiline; ja arenevad ladu aja jooksul.

## <a name="next-steps"></a>Järgmised sammud

Lisateabe saamiseks lugege artikleid [Tabeli ülevaade][Ülevaade], [Tabeli andmetüübid][Andmetüübid], [levitamise tabeli][hajuta],[Index] [indekseerimine tabeli], [Säilitada tabeli statistika][statistika] ja [Ajutised tabelid][ajutine].  Lisateavet heade tavade kohta leiate teemast [Andmete ladu SQL-i põhitõed][].

<!--Image references-->

<!--Article references-->
[Ülevaade]: ./sql-data-warehouse-tables-overview.md
[Andmetüübid]: ./sql-data-warehouse-tables-data-types.md
[Levitamine]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistika]: ./sql-data-warehouse-tables-statistics.md
[Ajutiste]: ./sql-data-warehouse-tables-temporary.md
[töökoormus haldus]: ./sql-data-warehouse-develop-concurrency.md
[SQL-i andmete ladu head tavad]: ./sql-data-warehouse-best-practices.md

<!-- MSDN Articles -->
[Sektsioonitud tabelite ja registrite]: https://msdn.microsoft.com/library/ms190787.aspx
[MUUDA TABELI]: https://msdn.microsoft.com/en-us/library/ms190273.aspx
[TABELI LOOMINE]: https://msdn.microsoft.com/library/mt203953.aspx
[funktsioon Partition]: https://msdn.microsoft.com/library/ms187802.aspx
[sektsiooni kava]: https://msdn.microsoft.com/library/ms179854.aspx


<!-- Other web references -->
