<properties
   pageTitle="Jälgimise Azure SQL-andmebaasi dünaamilise halduse vaadete abil | Microsoft Azure'i"
   description="Saate teada, kuidas tuvastada ja diagnoosimine levinud jõudlusprobleemide dünaamilise halduse vaadete abil saate jälgida Microsoft Azure'i SQL-andmebaasi."
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""
   tags=""/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="09/20/2016"
   ms.author="carlrab"/>

# <a name="monitoring-azure-sql-database-using-dynamic-management-views"></a>Azure'i SQL-andmebaasi dünaamilise halduse vaadete abil jälgimine

Microsoft Azure'i SQL-andmebaasi võimaldab alamhulga dünaamilise halduse vaadete diagnoosida jõudlusprobleemide, mis võib olla põhjustatud blokeeritud või pikaajalisi päringuid, ressursside kitsaskohad, päringu kehv lepingud ja jne. Sellest teemast leiate teavet selle kohta, kuidas tuvastada levinud jõudlusprobleemide dünaamilise halduse vaadete abil.

SQL-andmebaasi toetab osaliselt kolmele dünaamilise halduse vaadete.

- Andmebaasi seotud dünaamilise halduse vaadete.
- Juhtimise täitmise seotud vaated.
- Tehingu seotud dünaamilise halduse vaadete.

Dünaamilise halduse vaadete kohta leiate üksikasjalikumat teavet teemast [dünaamilise halduse vaadete ja funktsioonide (Transact-SQL-i)](https://msdn.microsoft.com/library/ms188754.aspx) SQL serveri raamatuid online.

## <a name="permissions"></a>Õigused

SQL-andmebaasis, dünaamilise juhtimise vaadata päringu jaoks on vaja **Vaadata andmebaasi riigi** õigused. **Vaadata andmebaasi riigi** õigust tagastab teabe praeguse andmebaasi kõiki objekte.
Kindla andmebaasi kasutaja **Vaadata andmebaasi riigi** õiguste andmiseks käivitage järgmine päring.

```GRANT VIEW DATABASE STATE TO database_user; ```

Asutusesisese SQL serveri eksemplar, dünaamilise halduse vaadete tagastada serveri teave. SQL-andmebaasis, nad tagasi praeguse loogilise andmebaasi ainult puudutav teave.

## <a name="calculating-database-size"></a>Arvutamise andmebaasi suurus

Järgmine päring tagastab (megabaitides) oma andmebaasi suurus:

```
-- Calculates the size of the database.
SELECT SUM(reserved_page_count)*8.0/1024
FROM sys.dm_db_partition_stats;
GO
```

Järgmine päring tagastab (MB) üksikute objektide suurust andmebaasi:

```
-- Calculates the size of individual database objects.
SELECT sys.objects.name, SUM(reserved_page_count) * 8.0 / 1024
FROM sys.dm_db_partition_stats, sys.objects
WHERE sys.dm_db_partition_stats.object_id = sys.objects.object_id
GROUP BY sys.objects.name;
GO
```

## <a name="monitoring-connections"></a>Ühenduste jälgimine

Saate hankida teavet ühendused, mis on loodud teatud Azure'i SQL-andmebaasi server ja iga ühenduse üksikasju [sys.dm_exec_connections](https://msdn.microsoft.com/library/ms181509.aspx) vaade. Lisaks [sys.dm_exec_sessions](https://msdn.microsoft.com/library/ms176013.aspx) vaade on kasulik, kui toomine teavet kõik aktiivsele kasutajale ühendused ja sisemise tööülesanded.
Järgmine päring tagastab praeguse ühenduse teave:

```
SELECT
    c.session_id, c.net_transport, c.encrypt_option,
    c.auth_scheme, s.host_name, s.program_name,
    s.client_interface_name, s.login_name, s.nt_domain,
    s.nt_user_name, s.original_login_name, c.connect_time,
    s.login_time
FROM sys.dm_exec_connections AS c
JOIN sys.dm_exec_sessions AS s
    ON c.session_id = s.session_id
WHERE c.session_id = @@SPID;
```

> [AZURE.NOTE] Täitmisel **sys.dm_exec_requests** ja **sys.dm_exec_sessions vaateid**, kui teil on õigus **Vaadata andmebaasi riigi** andmebaasi, kuvatakse kõik täidesaatva seansid andmebaasi; muul juhul näete ainult praeguse seansi.

## <a name="monitoring-query-performance"></a>Päringu jõudluse jälgimine

Aeglane või pikk päringud saate tühjendada märgatavat süsteemi ressursse. Selles jaotises näitab, kuidas kasutada dünaamilise halduse vaadete mõned levinud päring jõudlusprobleemide tuvastamiseks. Mõne vanema, kuid ikka kasulik viide tõrkeotsing, on [SQL Server 2008 tõrkeotsingu jõudlusprobleemide](http://download.microsoft.com/download/D/B/D/DBDE7972-1EB9-470A-BA18-58849DB3EB3B/TShootPerfProbs2008.docx) Microsoft TechNeti artiklist.

### <a name="finding-top-n-queries"></a>Ülemised N päringute otsimise

Järgmine näide tagastab keskmise CPU kellaaja järjestab viis põhipäringud teavet. Selles näites liitmise päringute vastavalt oma päringu räsi, et loogiliselt samaväärsed päringud on rühmitatud nende kumulatiivse ressursside tarbimine.

```
SELECT TOP 5 query_stats.query_hash AS "Query Hash",
    SUM(query_stats.total_worker_time) / SUM(query_stats.execution_count) AS "Avg CPU Time",
    MIN(query_stats.statement_text) AS "Statement Text"
FROM
    (SELECT QS.*,
    SUBSTRING(ST.text, (QS.statement_start_offset/2) + 1,
    ((CASE statement_end_offset
        WHEN -1 THEN DATALENGTH(ST.text)
        ELSE QS.statement_end_offset END
            - QS.statement_start_offset)/2) + 1) AS statement_text
     FROM sys.dm_exec_query_stats AS QS
     CROSS APPLY sys.dm_exec_sql_text(QS.sql_handle) as ST) as query_stats
GROUP BY query_stats.query_hash
ORDER BY 2 DESC;
```

### <a name="monitoring-blocked-queries"></a>Blokeeritud päringute jälgimine

Aeglane või pikaajalisi päringuid saate kaasa liigse ressursside tarbimine ja olla blokeeritud päringud. Põhjus blokeerimine võib olla kehva rakenduse kujundus, halb päringu lepingud, puudumine kasulik registrid jne. Saate praeguse locking tegevuse kohta teabe saamiseks SQL Azure'i andmebaasi sys.dm_tran_locks vaade. Näiteks koodi, vt [sys.dm_tran_locks (Transact-SQL-i)](https://msdn.microsoft.com/library/ms190345.aspx) SQL serveri raamatuid online.

### <a name="monitoring-query-plans"></a>Päringu lepingute jälgimine

Ebaefektiivne päringu plaan võib suurendada CPU tarbimine. Järgmises näites kasutatakse [sys.dm_exec_query_stats](https://msdn.microsoft.com/library/ms189741.aspx) vaate päring, mis kasutab kõige kumulatiivse CPU.

```
SELECT
    highest_cpu_queries.plan_handle,
    highest_cpu_queries.total_worker_time,
    q.dbid,
    q.objectid,
    q.number,
    q.encrypted,
    q.[text]
FROM
    (SELECT TOP 50
        qs.plan_handle,
        qs.total_worker_time
    FROM
        sys.dm_exec_query_stats qs
    ORDER BY qs.total_worker_time desc) AS highest_cpu_queries
    CROSS APPLY sys.dm_exec_sql_text(plan_handle) AS q
ORDER BY highest_cpu_queries.total_worker_time DESC;
```

## <a name="see-also"></a>Vt ka

[SQL-andmebaasi tutvustus](sql-database-technical-overview.md)
