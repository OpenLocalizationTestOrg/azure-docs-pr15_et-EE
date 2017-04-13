<properties
   pageTitle="Ülevaade tabelid SQL-i andmebaas | Microsoft Azure'i"
   description="Azure SQL-i ladu andmetabelite töötamise alustamine."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/04/2016"
   ms.author="sonyama;barbkess;jrj"/>

# <a name="overview-of-tables-in-sql-data-warehouse"></a>Ülevaade tabelid SQL-andmebaas

> [AZURE.SELECTOR]
- [Ülevaade][]
- [Andmetüübid][]
- [Levitamine][]
- [Index][]
- [Partition][]
- [Statistika][]
- [Ajutiste][]

Alustamine: SQL-i andmebaas tabelite loomine on lihtne.  [Tabeli loomine][] lihtsa süntaks järgib levinud süntaks olete tõenäoliselt juba tuttav töö muude andmebaasidega.  Tabeli loomiseks peate lihtsalt tabeli nime, Pange nimi oma veerud ja määratleda iga veeru andmetüübid.  Kui te olete tabelite loomine muid andmebaase, see peaks välja nägema väga tuttav.

```sql  
CREATE TABLE Customers (FirstName VARCHAR(25), LastName VARCHAR(25))
 ``` 

Eeltoodud näites loob tabeli nimega klientidele kaks veergu, eesnimi ja perekonnanimi.  Iga veeru on määratletud VARCHAR(25), mis piirab 25 märkide andmed andmete tüüpi.  Nende olulise atribuutide tabeli, kui ka teised, enamasti sama muid andmebaase.  Andmetüübid on määratletud iga veeru ja andmete terviklust tagada.  Registrite saab lisada vähendades I/O jõudluse parandamiseks.  Eraldamine saab lisada jõudluse parandamiseks, kui teil on vaja andmeid muuta.

[Ümbernimetamine] [ RENAME] SQL-i andmebaas tabeli, mis näeb välja umbes järgmine:

```sql  
RENAME OBJECT Customer TO CustomerOrig; 
 ```

## <a name="distributed-tables"></a>Jaotatud tabelid

Uue olulise atribuudi kasutusele jaotatud süsteemid, näiteks SQL-i andmebaas on **jaotuse veerg**.  Jaotuse veerg on väga palju, nagu see välja kõlab.  See on veerg, mis määrab, kuidas levitamine või jagamine, andmete taustal.  Tabeli loomisel ilma jaotuse veergu tabeli automaatselt levitatakse **round jaan**.  Kuigi round jaan tabelite võib olla piisavalt mõnel juhul, määratleda jaotuse veergude võib oluliselt vähendada andmete liikumine ajal päringud, seega optimeerida jõudlust.  Vaadake lisateavet selle kohta, kuidas jaotuse veeru valimiseks[hajuta] [levitamise tabeli].

## <a name="indexing-and-partitioning-tables"></a>Indekseerimise ja eraldamine tabelid

Nagu muutuvad täpsemaid kasutamisel SQL-i andmebaas ja soovite optimeerida jõudlust, soovite lisateavet tabeli kujundus.  Lisateabe saamiseks lugege artikleid [Tabeli andmetüübid][Andmetüübid], [levitamise tabeli][hajuta],[Index] [indekseerimine tabeli]ja[Partition] [eraldamine tabeli].

## <a name="table-statistics"></a>Tabeli statistika

Statistika on väga oluline parimaid tulemusi saada välja oma SQL-andmebaas.  Kuna SQL-i andmebaas pole veel automaatne loomine ja statistika värskendamise teile, nagu võib-olla olete jõudnud oodata, [statistika][] meie artikli lugemist Azure SQL andmebaasis võib olla üks kõige olulisem artikleid loete tagada parima jõudluse toomine oma päringuid.

## <a name="temporary-tables"></a>Ajutised tabelid

Ajutised tabelid on tabelid, mis ainult teie sisselogimise kestel olemas ja teised kasutajad ei näe.  Ajutised tabelid võib olla hea võimalus takistada teistel näha ajutine tulemused ja ka vähendamiseks Kettapuhastus.  Kuna Ajutised tabelid kasutada ka kohalikku, need võivad pakkuda oma tegevuse kiirema töö.  Lugege artikleid [Ajutise tabeli][ajutine] ajutiste tabelite kohta lisateabe saamiseks.

## <a name="external-tables"></a>Välise tabeleid

Välise tabeleid, nimetatakse ka Polybase tabelid on tabelid, kus saate kasutataks SQL-i andmebaas, kuid punkt: SQL-i andmebaas väliste andmetega.  Näiteks saate luua ka välise tabeli mis osutab failide Azure'i bloobimälu.  Lisateavet selle kohta, kuidas luua ja päringu saab välise tabeli, lugege teemat [laadi andmete Polybase][].  

## <a name="unsupported-table-features"></a>Mittetoetatavad tabelifunktsioonid

Kuigi SQL-i andmebaas sisaldab palju muid andmebaase pakutud tabeli funktsioone, on mõned funktsioonid, mis pole veel toetatud.  Allpool on mõned tabeli funktsioonid, mis pole veel toetatud loendi.

| Toetuseta funktsioonid |
| --- |
|[Identiteedi atribuudi][] (vt [Määramine asendusemadus klahvi lahendus][])|
|Primaarvõtme, võõrvõtmed, kordumatu ja sisse [Tabeli piiranguid][]|
|[Kordumatud indeksid][]|
|[Arvutatud veerud][]|
|[Vähe veerud][]|
|[Kasutaja määratletud tüübid][]|
|[Järjestus][]|
|[Päästikute][]|
|[Indekseeritud vaated][]|
|[Sünonüümid][]|

## <a name="table-size-queries"></a>Tabeli suuruse päringud

Üks lihtne viis tuvastada ruumi ja tarbitud iga 60 jaotuse, tabeli read on [DBCC PDW_SHOWSPACEUSED][]kasutada.

```sql
DBCC PDW_SHOWSPACEUSED('dbo.FactInternetSales');
```

Siiski DBCC käskude abil saab üsna piirata.  Dünaamiliste haldusvaated () võimaldab teil näha palju üksikasjalikumalt samuti teile palju suuremat kontrolli päringu tulemused üle.  Alustuseks loomise suunatakse paljud meie näidetes kõnealuse ja ka muud artiklid selles vaates.

```sql
CREATE VIEW dbo.vTableSizes
AS
WITH base
AS
(
SELECT 
 GETDATE()                                                             AS  [execution_time]
, DB_NAME()                                                            AS  [database_name]
, s.name                                                               AS  [schema_name]
, t.name                                                               AS  [table_name]
, QUOTENAME(s.name)+'.'+QUOTENAME(t.name)                              AS  [two_part_name]
, nt.[name]                                                            AS  [node_table_name]
, ROW_NUMBER() OVER(PARTITION BY nt.[name] ORDER BY (SELECT NULL))     AS  [node_table_name_seq]
, tp.[distribution_policy_desc]                                        AS  [distribution_policy_name]
, c.[name]                                                             AS  [distribution_column]
, nt.[distribution_id]                                                 AS  [distribution_id]
, i.[type]                                                             AS  [index_type]
, i.[type_desc]                                                        AS  [index_type_desc]
, nt.[pdw_node_id]                                                     AS  [pdw_node_id]
, pn.[type]                                                            AS  [pdw_node_type]
, pn.[name]                                                            AS  [pdw_node_name]
, di.name                                                              AS  [dist_name]
, di.position                                                          AS  [dist_position]
, nps.[partition_number]                                               AS  [partition_nmbr]
, nps.[reserved_page_count]                                            AS  [reserved_space_page_count]
, nps.[reserved_page_count] - nps.[used_page_count]                    AS  [unused_space_page_count]
, nps.[in_row_data_page_count] 
    + nps.[row_overflow_used_page_count] 
    + nps.[lob_used_page_count]                                        AS  [data_space_page_count]
, nps.[reserved_page_count] 
 - (nps.[reserved_page_count] - nps.[used_page_count]) 
 - ([in_row_data_page_count] 
         + [row_overflow_used_page_count]+[lob_used_page_count])       AS  [index_space_page_count]
, nps.[row_count]                                                      AS  [row_count]
from 
    sys.schemas s
INNER JOIN sys.tables t
    ON s.[schema_id] = t.[schema_id]
INNER JOIN sys.indexes i
    ON  t.[object_id] = i.[object_id]
    AND i.[index_id] <= 1
INNER JOIN sys.pdw_table_distribution_properties tp
    ON t.[object_id] = tp.[object_id]
INNER JOIN sys.pdw_table_mappings tm
    ON t.[object_id] = tm.[object_id]
INNER JOIN sys.pdw_nodes_tables nt
    ON tm.[physical_name] = nt.[name]
INNER JOIN sys.dm_pdw_nodes pn
    ON  nt.[pdw_node_id] = pn.[pdw_node_id]
INNER JOIN sys.pdw_distributions di
    ON  nt.[distribution_id] = di.[distribution_id]
INNER JOIN sys.dm_pdw_nodes_db_partition_stats nps
    ON nt.[object_id] = nps.[object_id]
    AND nt.[pdw_node_id] = nps.[pdw_node_id]
    AND nt.[distribution_id] = nps.[distribution_id]
LEFT OUTER JOIN (select * from sys.pdw_column_distribution_properties where distribution_ordinal = 1) cdp
    ON t.[object_id] = cdp.[object_id]
LEFT OUTER JOIN sys.columns c
    ON cdp.[object_id] = c.[object_id]
    AND cdp.[column_id] = c.[column_id]
)
, size
AS
(
SELECT
   [execution_time]
,  [database_name]
,  [schema_name]
,  [table_name]
,  [two_part_name]
,  [node_table_name]
,  [node_table_name_seq]
,  [distribution_policy_name]
,  [distribution_column]
,  [distribution_id]
,  [index_type]
,  [index_type_desc]
,  [pdw_node_id]
,  [pdw_node_type]
,  [pdw_node_name]
,  [dist_name]
,  [dist_position]
,  [partition_nmbr]
,  [reserved_space_page_count]
,  [unused_space_page_count]
,  [data_space_page_count]
,  [index_space_page_count]
,  [row_count]
,  ([reserved_space_page_count] * 8.0)                                 AS [reserved_space_KB]
,  ([reserved_space_page_count] * 8.0)/1000                            AS [reserved_space_MB]
,  ([reserved_space_page_count] * 8.0)/1000000                         AS [reserved_space_GB]
,  ([reserved_space_page_count] * 8.0)/1000000000                      AS [reserved_space_TB]
,  ([unused_space_page_count]   * 8.0)                                 AS [unused_space_KB]
,  ([unused_space_page_count]   * 8.0)/1000                            AS [unused_space_MB]
,  ([unused_space_page_count]   * 8.0)/1000000                         AS [unused_space_GB]
,  ([unused_space_page_count]   * 8.0)/1000000000                      AS [unused_space_TB]
,  ([data_space_page_count]     * 8.0)                                 AS [data_space_KB]
,  ([data_space_page_count]     * 8.0)/1000                            AS [data_space_MB]
,  ([data_space_page_count]     * 8.0)/1000000                         AS [data_space_GB]
,  ([data_space_page_count]     * 8.0)/1000000000                      AS [data_space_TB]
,  ([index_space_page_count]  * 8.0)                                   AS [index_space_KB]
,  ([index_space_page_count]  * 8.0)/1000                              AS [index_space_MB]
,  ([index_space_page_count]  * 8.0)/1000000                           AS [index_space_GB]
,  ([index_space_page_count]  * 8.0)/1000000000                        AS [index_space_TB]
FROM base
)
SELECT * 
FROM size
;
```

### <a name="table-space-summary"></a>Tabeli ruumi Kokkuvõte

See päring tagastab read ja ruumi, tabel.  See on suurepärane päringu näha, millised tabelid on teie suurim tabelid ja kas need on ringi jaan või jaotatud räsi.  Jaotatud räsi tabelite jaoks ka kuvatakse veeru jaotuse.  Enamikul juhtudel tuleks suurim tabelite räsi jaotatud rühmitatud columnstore indeks.

```sql
SELECT 
     database_name
,    schema_name
,    table_name
,    distribution_policy_name
,     distribution_column
,    index_type_desc
,    COUNT(distinct partition_nmbr) as nbr_partitions
,    SUM(row_count)                 as table_row_count
,    SUM(reserved_space_GB)         as table_reserved_space_GB
,    SUM(data_space_GB)             as table_data_space_GB
,    SUM(index_space_GB)            as table_index_space_GB
,    SUM(unused_space_GB)           as table_unused_space_GB
FROM 
    dbo.vTableSizes
GROUP BY 
     database_name
,    schema_name
,    table_name
,    distribution_policy_name
,     distribution_column
,    index_type_desc
ORDER BY
    table_reserved_space_GB desc
;
```

### <a name="table-space-by-distribution-type"></a>Tabeli ruumi jaotuse tüübi järgi

```sql
SELECT 
     distribution_policy_name
,    SUM(row_count)                as table_type_row_count
,    SUM(reserved_space_GB)        as table_type_reserved_space_GB
,    SUM(data_space_GB)            as table_type_data_space_GB
,    SUM(index_space_GB)           as table_type_index_space_GB
,    SUM(unused_space_GB)          as table_type_unused_space_GB
FROM dbo.vTableSizes
GROUP BY distribution_policy_name
;
```

### <a name="table-space-by-index-type"></a>Tabeli ruumi index tüübi järgi

```sql
SELECT 
     index_type_desc
,    SUM(row_count)                as table_type_row_count
,    SUM(reserved_space_GB)        as table_type_reserved_space_GB
,    SUM(data_space_GB)            as table_type_data_space_GB
,    SUM(index_space_GB)           as table_type_index_space_GB
,    SUM(unused_space_GB)          as table_type_unused_space_GB
FROM dbo.vTableSizes
GROUP BY index_type_desc
;
```

### <a name="distribution-space-summary"></a>Jaotuse ruumi Kokkuvõte

```sql
SELECT 
    distribution_id
,    SUM(row_count)                as total_node_distribution_row_count
,    SUM(reserved_space_MB)        as total_node_distribution_reserved_space_MB
,    SUM(data_space_MB)            as total_node_distribution_data_space_MB
,    SUM(index_space_MB)           as total_node_distribution_index_space_MB
,    SUM(unused_space_MB)          as total_node_distribution_unused_space_MB
FROM dbo.vTableSizes
GROUP BY     distribution_id
ORDER BY    distribution_id
;
```

## <a name="next-steps"></a>Järgmised sammud

Lisateabe saamiseks lugege artikleid [Tabeli andmetüübid][Andmetüübid], [levitamise tabeli][hajuta],[Index] [indekseerimine tabeli], [eraldamine tabeli][Partition], [Säilitada tabeli statistika][statistika] ja [Ajutised tabelid][ajutine].  Lisateavet heade tavade kohta leiate teemast [Andmete ladu SQL-i põhitõed][].

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
[Polybase andmete laadimine]: ./sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md

<!--MSDN references-->
[TABELI LOOMINE]: https://msdn.microsoft.com/library/mt203953.aspx
[RENAME]: https://msdn.microsoft.com/library/mt631611.aspx
[DBCC PDW_SHOWSPACEUSED]: https://msdn.microsoft.com/library/mt204028.aspx
[Identiteedi atribuudi]: https://msdn.microsoft.com/library/ms186775.aspx
[Asendusemadus võtme lahendus määramine]: https://blogs.msdn.microsoft.com/sqlcat/2016/02/18/assigning-surrogate-key-to-dimension-tables-in-sql-dw-and-aps/
[Tabeli piiranguid]: https://msdn.microsoft.com/library/ms188066.aspx
[Arvutatud veerud]: https://msdn.microsoft.com/library/ms186241.aspx
[Vähe veerud]: https://msdn.microsoft.com/library/cc280604.aspx
[Kasutaja määratletud tüübid]: https://msdn.microsoft.com/library/ms131694.aspx
[Järjestus]: https://msdn.microsoft.com/library/ff878091.aspx
[Päästikute]: https://msdn.microsoft.com/library/ms189799.aspx
[Indekseeritud vaated]: https://msdn.microsoft.com/library/ms191432.aspx
[Sünonüümid]: https://msdn.microsoft.com/library/ms177544.aspx
[Kordumatud indeksid]: https://msdn.microsoft.com/library/ms188783.aspx

<!--Other Web references-->
