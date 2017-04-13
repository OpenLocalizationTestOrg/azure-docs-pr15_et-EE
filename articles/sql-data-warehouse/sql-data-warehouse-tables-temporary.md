<properties
   pageTitle="Ajutised tabelid SQL-i andmebaas | Microsoft Azure'i"
   description="Alustamine Ajutised tabelid SQL Azure'i andmebaas."
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
   ms.date="06/29/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="temporary-tables-in-sql-data-warehouse"></a>Ajutised tabelid SQL-andmebaas

> [AZURE.SELECTOR]
- [Ülevaade][]
- [Andmetüübid][]
- [Levitamine][]
- [Index][]
- [Partition][]
- [Statistika][]
- [Ajutiste][]

Ajutised tabelid on väga kasulik, kui andmete - töötlemise ajal eriti teisendus vahe tulemused on ajutine. SQL-i andmebaas olemas Ajutised tabelid seansi tasemel.  Need on ainult seansi, kus nad on loodud ja kõrvaldatakse automaatselt, kui selle seansi logib välja näevad.  Ajutised tabelid pakkuda jõudluse kasu, kuna nende tulemused kirjutada kohaliku asemel remote salvestusruumi.  Ajutised tabelid on veidi teistsugused SQL Azure'i andmebaas Azure'i SQL-andmebaasi kui ta pääseb juurde kõikjalt seansi, sh nii ettevõtte töötajatel ja ettevõttevälistel salvestatud protseduur sees.

Selles artiklis sisaldab olulisi juhiseid kasutamise Ajutised tabelid ja tõstetakse esile seansi taseme Ajutised tabelid põhimõtetest. Selles artiklis toodud teabe abil saate oma koodi parandada nii korduvkasutatavuse ja lihtne hooldus teie koodi modularize.

## <a name="create-a-temporary-table"></a>Ajutiste tabeli loomine

Ajutised tabelid on loodud lihtsalt eesliidete lisamine oma tabeli nimi, kus on `#`.  Näiteks:

```sql
CREATE TABLE #stats_ddl
(
    [schema_name]       NVARCHAR(128) NOT NULL
,   [table_name]            NVARCHAR(128) NOT NULL
,   [stats_name]            NVARCHAR(128) NOT NULL
,   [stats_is_filtered]     BIT           NOT NULL
,   [seq_nmbr]              BIGINT        NOT NULL
,   [two_part_name]         NVARCHAR(260) NOT NULL
,   [three_part_name]       NVARCHAR(400) NOT NULL
)
WITH
(
    DISTRIBUTION = HASH([seq_nmbr])
,   HEAP
)
```

Ajutiste tabeleid saab luua ka mõne `CTAS` täpselt sama lähenemisviisi abil:

```sql
CREATE TABLE #stats_ddl
WITH
(
    DISTRIBUTION = HASH([seq_nmbr])
,   HEAP
)
AS
(
SELECT
        sm.[name]                                                               AS [schema_name]
,       tb.[name]                                                               AS [table_name]
,       st.[name]                                                               AS [stats_name]
,       st.[has_filter]                                                         AS [stats_is_filtered]
,       ROW_NUMBER()
        OVER(ORDER BY (SELECT NULL))                                            AS [seq_nmbr]
,                                QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])  AS [two_part_name]
,       QUOTENAME(DB_NAME())+'.'+QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])  AS [three_part_name]
FROM    sys.objects         AS ob
JOIN    sys.stats           AS st   ON  ob.[object_id]      = st.[object_id]
JOIN    sys.stats_columns   AS sc   ON  st.[stats_id]       = sc.[stats_id]
                                    AND st.[object_id]      = sc.[object_id]
JOIN    sys.columns         AS co   ON  sc.[column_id]      = co.[column_id]
                                    AND sc.[object_id]      = co.[object_id]
JOIN    sys.tables          AS tb   ON  co.[object_id]      = tb.[object_id]
JOIN    sys.schemas         AS sm   ON  tb.[schema_id]      = sm.[schema_id]
WHERE   1=1
AND     st.[user_created]   = 1
GROUP BY
        sm.[name]
,       tb.[name]
,       st.[name]
,       st.[filter_definition]
,       st.[has_filter]
)
SELECT
    CASE @update_type
    WHEN 1
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+');'
    WHEN 2
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH FULLSCAN;'
    WHEN 3
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH SAMPLE '+CAST(@sample_pct AS VARCHAR(20))+' PERCENT;'
    WHEN 4
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH RESAMPLE;'
    END AS [update_stats_ddl]
,   [seq_nmbr]
FROM    t1
;
``` 

>[AZURE.NOTE] `CTAS`on väga võimas käsu ja eelis on väga tõhusa tehingu Logi ruumi kasutama. 


## <a name="dropping-temporary-tables"></a>Pukseerimine Ajutised tabelid

Uue seansi loomisel ajutise tabeleid peaks olemas.  Siiski kui helistate salvestatud sama toimingut, mis loob ajutine tagamaks, et sama nimega oma `CREATE TABLE` laused on lihtne enne olemasolu saate on `DROP` saab kasutada nii on näide:

```sql
IF OBJECT_ID('tempdb..#stats_ddl') IS NOT NULL
BEGIN
    DROP TABLE #stats_ddl
END
```

Koodis järjepidevuse, on hea tava nii ja ajutiste tabelite selle mustri kasutamiseks.  Samuti on soovitav kasutada `DROP TABLE` eemaldamiseks Ajutised tabelid, kui olete lõpetanud nendega koodi.  Salvestatud protseduur kustutatavate arengu on üsna tavaline, drop käsud kokku kogutud protseduuri tagamaks, et need objektid lõpus.

```sql
DROP TABLE #stats_ddl
```

## <a name="modularizing-code"></a>Modularizing kood

Kuna Ajutised tabelid näha suvalist kasutaja seansi, saate selle ära aitavad modularize rakenduse koodis.  Näiteks alltoodud salvestatud protseduur koondab tavadega ülevalt luua DDL, mis värskendab kõik statistika andmebaasi statistiline nime järgi.

```sql
CREATE PROCEDURE    [dbo].[prc_sqldw_update_stats]
(   @update_type    tinyint -- 1 default 2 fullscan 3 sample 4 resample
    ,@sample_pct     tinyint
)
AS

IF @update_type NOT IN (1,2,3,4)
BEGIN;
    THROW 151000,'Invalid value for @update_type parameter. Valid range 1 (default), 2 (fullscan), 3 (sample) or 4 (resample).',1;
END;

IF @sample_pct IS NULL
BEGIN;
    SET @sample_pct = 20;
END;

IF OBJECT_ID('tempdb..#stats_ddl') IS NOT NULL
BEGIN
    DROP TABLE #stats_ddl
END

CREATE TABLE #stats_ddl
WITH
(
    DISTRIBUTION = HASH([seq_nmbr])
)
AS
(
SELECT
        sm.[name]                                                               AS [schema_name]
,       tb.[name]                                                               AS [table_name]
,       st.[name]                                                               AS [stats_name]
,       st.[has_filter]                                                         AS [stats_is_filtered]
,       ROW_NUMBER()
        OVER(ORDER BY (SELECT NULL))                                            AS [seq_nmbr]
,                                QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])  AS [two_part_name]
,       QUOTENAME(DB_NAME())+'.'+QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])  AS [three_part_name]
FROM    sys.objects         AS ob
JOIN    sys.stats           AS st   ON  ob.[object_id]      = st.[object_id]
JOIN    sys.stats_columns   AS sc   ON  st.[stats_id]       = sc.[stats_id]
                                    AND st.[object_id]      = sc.[object_id]
JOIN    sys.columns         AS co   ON  sc.[column_id]      = co.[column_id]
                                    AND sc.[object_id]      = co.[object_id]
JOIN    sys.tables          AS tb   ON  co.[object_id]      = tb.[object_id]
JOIN    sys.schemas         AS sm   ON  tb.[schema_id]      = sm.[schema_id]
WHERE   1=1
AND     st.[user_created]   = 1
GROUP BY
        sm.[name]
,       tb.[name]
,       st.[name]
,       st.[filter_definition]
,       st.[has_filter]
)
SELECT
    CASE @update_type
    WHEN 1
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+');'
    WHEN 2
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH FULLSCAN;'
    WHEN 3
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH SAMPLE '+CAST(@sample_pct AS VARCHAR(20))+' PERCENT;'
    WHEN 4
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH RESAMPLE;'
    END AS [update_stats_ddl]
,   [seq_nmbr]
FROM    t1
;
GO
```

Selles etapis ainus toiming, mille on ilmnenud on salvestatud protseduur, mis on lihtsalt loomine loodud ajutine tabeli #stats_ddl koos DDL laused.  See salvestatud protseduur langeb #stats_ddl, kui see on juba olemas, tagamaks, et seda ei suuda kui seansi jooksul rohkem kui ühe korra käitanud.  Kuid kuna seal on pole `DROP TABLE` salvestatud protseduur lõpus kui salvestatud toiming on lõpule jõudnud, see jätab loodud tabel, et seda saab lugeda väljaspool salvestatud protseduur.  SQL-i andmebaas, erinevalt SQL serveri muid andmebaase, on võimalik ajutine tabeli väljaspool loodud selle toimingu kasutamiseks.  SQL-i andmebaas Ajutised tabelid saab kasutatud **suvalist** seansi sees. See võib kaasa tuua rohkem muutuv ja mõistliku koodi, nagu on allpool näites:

```sql
EXEC [dbo].[prc_sqldw_update_stats] @update_type = 1, @sample_pct = NULL;

DECLARE @i INT              = 1
,       @t INT              = (SELECT COUNT(*) FROM #stats_ddl)
,       @s NVARCHAR(4000)   = N''

WHILE @i <= @t
BEGIN
    SET @s=(SELECT update_stats_ddl FROM #stats_ddl WHERE seq_nmbr = @i);

    PRINT @s
    EXEC sp_executesql @s
    SET @i+=1;
END

DROP TABLE #stats_ddl;
```

## <a name="temporary-table-limitations"></a>Ajutise tabeli piirangud

SQL-i andmebaas kehtestada piirangud paar rakendamisel Ajutised tabelid.  Praegu ainult seansi jäävates Ajutised tabelid on toetatud.  Globaalne ajutine tabeleid ei toetata.  Lisaks vaateid ei saa luua Ajutised tabelid.

## <a name="next-steps"></a>Järgmised sammud

Lisateabe saamiseks vt artikleid [Tabeli ülevaade][Ülevaade], [Tabeli andmetüübid][Andmetüübid], [levitamise tabeli][hajuta],[Index] [indekseerimine tabeli],[Partition] [eraldamine tabeli]ja [Säilitada tabeli statistika][statistika].  Lisateavet heade tavade kohta leiate teemast [Andmete ladu SQL-i põhitõed][].

<!--Image references-->

<!--Article references-->
[Ülevaade]: ./sql-data-warehouse-tables-overview.md
[Andmetüübid]: ./sql-data-warehouse-tables-data-types.md
[Levitamine]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistika]: ./sql-data-warehouse-tables-statistics.md
[Ajutiste]: ./sql-data-warehouse-tables-temporary.md
[SQL-i andmete ladu head tavad]: ./sql-data-warehouse-best-practices.md

<!--MSDN references-->

<!--Other Web references-->
