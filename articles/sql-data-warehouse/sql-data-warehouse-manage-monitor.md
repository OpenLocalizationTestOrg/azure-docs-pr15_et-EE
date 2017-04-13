<properties
   pageTitle="Jälgida oma töökoormus DMVs abil | Microsoft Azure'i"
   description="Saate teada, kuidas jälgida oma töökoormus DMVs abil."
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
   ms.date="10/08/2016"
   ms.author="sonyama;barbkess"/>

# <a name="monitor-your-workload-using-dmvs"></a>Jälgida oma töökoormus DMVs abil

Selles artiklis kirjeldatakse, kuidas dünaamilise haldusvaated () abil saate jälgida oma töökoormus ja uurida päringu täitmise SQL Azure'i andmebaas.

## <a name="permissions"></a>Õigused

Päringu DMVs selle artikli, tuleb teil vaadata andmebaasi riigi-või JUHTELEMENTI. Tavaliselt on andva vaadata andmebaasi riigi eelistatud õigus, et see on palju suuremad piirangud.

```sql
GRANT VIEW DATABASE STATE TO myuser;
```

## <a name="monitor-connections"></a>Kuvari ühendused

SQL-i andmebaas kõigi sisselogimist logitakse [sys.dm_pdw_exec_sessions][].  See DMV sisaldab viimase 10 000 sisselogimise.  Funktsiooni session_id on primaarvõti ja määratakse järjest iga uue sisselogimine.

```sql
-- Other Active Connections
SELECT * FROM sys.dm_pdw_exec_sessions where status <> 'Closed' and session_id <> session_id();
```

## <a name="monitor-query-execution"></a>Kuvari päringu käivitamine

SQL-i andmebaas täide kõik päringud logitakse [sys.dm_pdw_exec_requests][].  See DMV sisaldab viimase 10 000 päringuid, mis on täidetud.  Funktsiooni request_id kordumatult tuvastab iga päringu ja on see DMV primaarvõti.  Funktsiooni request_id on määratud järjest iga uue päringu ja on eesliide QID, mis tähistab päringu ID-ga.  Selle DMV jaoks antud session_id päringud kuvatakse kõik päringud antud sisselogimiseks.

>[AZURE.NOTE] Salvestatud toimingute kasutada mitut taotlemine ID-d.  Koosolekukutse ID-d on määratud järjekorras. 

Siin on uurida päringu täitmise režiimi ja kellaajad kindla päringu järgige juhiseid.

### <a name="step-1-identify-the-query-you-wish-to-investigate"></a>Samm 1: Tuvastada, mida soovite uurida päringu

```sql
-- Monitor active queries
SELECT * 
FROM sys.dm_pdw_exec_requests 
WHERE status not in ('Completed','Failed','Cancelled')
  AND session_id <> session_id()
ORDER BY submit_time DESC;

-- Find top 10 queries longest running queries
SELECT TOP 10 * 
FROM sys.dm_pdw_exec_requests 
ORDER BY total_elapsed_time DESC;

-- Find a query with the Label 'My Query'
-- Use brackets when querying the label column, as it it a key word
SELECT  *
FROM    sys.dm_pdw_exec_requests
WHERE   [label] = 'My Query';
```

Eelmise päringutulemite **Märkus paluda ID** , mida soovite uurida päringu kaudu.

Päringute **Suspended** olekus on järjekorda tõttu kokkulangevus piirangud. Need päringud kuvatakse ka sys.dm_pdw_waits ootab päringu UserConcurrencyResourceType tüüpi. Kokkulangevus limiitide kohta lisateabe saamiseks vt [kokkulangevus ja töökoormus][] . Päringute võite ka oodata kättesaamatuna objekti lukud.  Kui päring on ootel ressursi, lugege teemat [korraldama videokonverentsi päringute ressursside ootamine][] selles artiklis allpool.

Otsingu päringu sys.dm_pdw_exec_requests tabeli lihtsustamiseks kasutage [SILTI][] , et määrata oma päring, mis võib käsitleda sys.dm_pdw_exec_requests vaates kommentaari.

```sql
-- Query with Label
SELECT *
FROM sys.tables
OPTION (LABEL = 'My Query')
;
```

### <a name="step-2-investigate-the-query-plan"></a>Samm 2: Uurida päringu plaan

Taotleda ID abil saate tuua päringu jaotatud SQL-i (DSQL) lepingu [sys.dm_pdw_request_steps][].

```sql
-- Find the distributed query plan steps for a specific query.
-- Replace request_id with value from Step 1.

SELECT * FROM sys.dm_pdw_request_steps
WHERE request_id = 'QID####'
ORDER BY step_index;
```

Kui DSQL leping kulub oodatust kauem, võib põhjus keerukate leping paljude DSQL juhiseid või ainult ühe toiming võtab kaua aega.  Kui leping on palju juhiseid mitme Teisalda toimingutega, kaaluge optimeerimist oma tabeli jaotuse vähendada andmete liikumine. [Tabeli jaotuse][] artikkel selgitab, miks andmed teisaldatakse lahendada päringu ja selgitatakse mõningaid jaotuse strateegiad minimeerimiseks andmete liikumine.

Uurida üksiktoiming *operation_type* veeru pikaajalisi päringuetapi üksikasjad ja **Samm Index**Pange tähele, tehke järgmist.

- Jätkake **SQL**-analüüsitoiminguid samm 3: OnOperation, RemoteOperation, ReturnOperation.
- Jätkata juhisega 3b **Andmete liikumine**toimingute: ShuffleMoveOperation, BroadcastMoveOperation, TrimMoveOperation, PartitionMoveOperation, MoveOperation, CopyOperation.

### <a name="step-3a-investigate-sql-on-the-distributed-databases"></a>Samm 3: jaotatud andmebaasid SQL-i uurimine

Üksikasjade toomine [sys.dm_pdw_sql_requests][], mis sisaldab täitmise teavet päringu etapp kõik jaotatud andmebaaside taotlemine ID ja samm registri abil.

```sql
-- Find the distribution run times for a SQL step.
-- Replace request_id and step_index with values from Step 1 and 3.

SELECT * FROM sys.dm_pdw_sql_requests
WHERE request_id = 'QID####' AND step_index = 2;
```

Päringu etapp töötamisel saab [DBCC PDW_SHOWEXECUTIONPLAN][] SQL serveri plaani vahemälu samm kindla leviloendi töötab SQL serveri hinnanguline leping toomiseks.

```sql
-- Find the SQL Server execution plan for a query running on a specific SQL Data Warehouse Compute or Control node.
-- Replace distribution_id and spid with values from previous query.

DBCC PDW_SHOWEXECUTIONPLAN(1, 78);
```

### <a name="step-3b-investigate-data-movement-on-the-distributed-databases"></a>SAMM 3b: andmete liikumine jaotatud andmebaasid uurimine

Taotleda ID ja samm registri abil saate tuua teavet andmete liikumine samm töötab iga jaotuse [sys.dm_pdw_dms_workers][]kaudu.

```sql
-- Find the information about all the workers completing a Data Movement Step.
-- Replace request_id and step_index with values from Step 1 and 3.

SELECT * FROM sys.dm_pdw_dms_workers
WHERE request_id = 'QID####' AND step_index = 2;
```

- Märkige *total_elapsed_time* veeru kuvamiseks, kui kindla leviloendi võtab oluliselt pikem kui teised andmete liikumine.
- Kontrollige levitamiseks pikaajalisi *rows_processed* veeru kas selle jaotuse viiakse ridade arvu, mis on märkimisväärselt suurem kui teised. Kui jah, see võib viidata skew aluseks olevate andmete.

Kui päring töötab, saab [DBCC PDW_SHOWEXECUTIONPLAN][] SQL serveri hinnanguline leping toomiseks SQL serveri plaani vahemälu praegu töötab SQL samm kindla jaotuse sees.

```sql
-- Find the SQL Server estimated plan for a query running on a specific SQL Data Warehouse Compute or Control node.
-- Replace distribution_id and spid with values from previous query.

DBCC PDW_SHOWEXECUTIONPLAN(55, 238);
```

<a name="waiting"></a>
## <a name="monitor-waiting-queries"></a>Ootel päringute jälgimine

Kui avastate, et päring on tegemist edenemist, kuna ressursi ootama, siis siin on päring, mis kuvab kõik ressursid päringu ootab.

```sql
-- Find queries 
-- Replace request_id with value from Step 1.

SELECT waits.session_id,
      waits.request_id,  
      requests.command,
      requests.status,
      requests.start_time,  
      waits.type,
      waits.state,
      waits.object_type,
      waits.object_name
FROM   sys.dm_pdw_waits waits
   JOIN  sys.dm_pdw_exec_requests requests
   ON waits.request_id=requests.request_id
WHERE waits.request_id = 'QID####'
ORDER BY waits.object_name, waits.object_type, waits.state;
```

Kui päring on ootel aktiivselt ressursid teise päringu, siis olek on **AcquireResources**.  Kui päring on vajalikud ressursid, siis olek on **antakse**.

## <a name="next-steps"></a>Järgmised sammud
DMVs kohta lisateabe saamiseks lugege teemat [süsteemi vaated][] .
Lisateavet [SQL-i andmebaas head tavad][] heade tavade kohta

<!--Image references-->

<!--Article references-->
[Manage overview]: ./sql-data-warehouse-overview-manage.md
[SQL-i andmebaas head tavad]: ./sql-data-warehouse-best-practices.md
[Süsteemi vaated]: ./sql-data-warehouse-reference-tsql-system-views.md
[Tabeli jaotuse.]: ./sql-data-warehouse-tables-distribute.md
[Kokkulangevus ja töökoormus haldus]: ./sql-data-warehouse-develop-concurrency.md
[Päringute ootamine ressursside uurimisel]: ./sql-data-warehouse-manage-monitor.md#waiting

<!--MSDN references-->
[sys.dm_pdw_dms_workers]: http://msdn.microsoft.com/library/mt203878.aspx
[sys.dm_pdw_exec_requests]: http://msdn.microsoft.com/library/mt203887.aspx
[sys.dm_pdw_exec_sessions]: http://msdn.microsoft.com/library/mt203883.aspx
[sys.dm_pdw_request_steps]: http://msdn.microsoft.com/library/mt203913.aspx
[sys.dm_pdw_sql_requests]: http://msdn.microsoft.com/library/mt203889.aspx
[DBCC PDW_SHOWEXECUTIONPLAN]: http://msdn.microsoft.com/library/mt204017.aspx
[DBCC PDW_SHOWSPACEUSED]: http://msdn.microsoft.com/library/mt204028.aspx
[SILDI]: https://msdn.microsoft.com/library/ms190322.aspx
