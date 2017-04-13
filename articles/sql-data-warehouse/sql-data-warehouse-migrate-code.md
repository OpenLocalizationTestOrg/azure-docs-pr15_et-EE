<properties
   pageTitle="SQL-koodi migreerimine SQL-i andmebaas | Microsoft Azure'i"
   description="Näpunäiteid migreerimine SQL-koodi SQL Azure'i andmebaas arendamise lahendusi."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/02/2016"
   ms.author="lodipalm;barbkess;sonyama;jrj"/>

# <a name="migrate-your-sql-code-to-sql-data-warehouse"></a>SQL-koodi migreerimine SQL-andmebaas

Kui teie kood migreerimine versioonist teise andmebaasi SQL-i andmebaas, peate tõenäoliselt muuta oma kood base. Mõned funktsioonid SQL-i andmebaas märkimisväärselt saate parandada jõudlust, nagu need on loodud töötama jaotatud viisil. Kuid jõudlus ja skaala säilitamiseks mõned funktsioonid pole ka saadaval.

## <a name="common-t-sql-limitations"></a>Levinud T-SQL-i piirangud

Järgmises loendis on toodud enamlevinud funktsioon, mis ei toeta SQL Azure'i andmebaas. Lingid viivad teid lahendused toetuseta funktsiooni:

- [ANSI ühenduste värskendused][]
- [ANSI-ühenduste kohta kustutab][]
- [Ühenda lause][]
- rist-andmebaasi ühendused
- [kursorid][]
- [VALIGE. RAKENDUSSE][]
- [LISA. EXEC][]
- väljundi klausel
- Tekstisisene kasutaja määratletud funktsioonid
- mitme lause funktsioonid
- [levinud tabeli avaldiste](#Common-table-expressions)
- [Rekursiivsed levinud tabeli avaldiste (CTE)] (#Recursive-common-table-expressions-(CTE)
- CLR-i funktsioonide ja toimingute
- funktsioon $partition
- tabeli muutujate
- tabeli väärtus parameetrid
- jaotatud tehinguid
- Kinnita / tagasipööramine töö
- tehingu salvestamine
- täitmise kontekstides (EXECUTE AS)
- [klausel koos funktsiooniga rollup Rühmitusalus / kuup / komplekti rühmitamissuvandite][]
- [Pesastustasemete Lisaks 8][]
- [vaadete kaudu värskendamine][]
- [Valige muutuja kasutamine][]
- [dünaamiline SQL-i stringide puhul tippige MAX andmed][]

Enamik need piirangud saab Õnneks töötas ümber. Selgitused on toodud oluline arengu artiklite viidatud kohal.

## <a name="supported-cte-features"></a>Toetatud CTE funktsioonid

SQL-i andmebaas on osaliselt toetatud levinud tabeli avaldiste (CTEs).  CTE järgmised funktsioonid on praegu toetatud:

- SELECT-lauses saab määrata mõne CTE.
- Mõne CTE saab määrata lauses Loo vaade.
- Mõne CTE saab määrata lauses loomine tabeli AS valida (CTAS).
- Mõne CTE saab määrata lauses loomine REMOTE tabeli AS valimine (CRTAS).
- Mõne CTE saab määrata lauses luua välise tabeli AS valimine (CETAS).
- Mõne CTE saab viidata serveri tabel.
- Mõne CTE saab viidata ka välise tabeli.
- Mitme CTE päringu määratlused saate määratleda on CTE.

## <a name="cte-limitations"></a>CTE piirangud

Levinud tabeli avaldiste on mõned piirangud, sh SQL-i andmebaas järgmist.

- Mõne CTE peab järgneb ühe SELECT-lause. LISAMINE, värskendamine, kustutamine, ja Ühenda laused ei toetata.
- Levinud tabelavaldis, mis sisaldab viiteid ise (Rekursiivsed levinud tabeli avaldise) ei toetata (vt jaotist allpool).
- Mis määrab mitu KLAUSLIGA on CTE pole lubatud. Näiteks kui ka CTE_query_definition sisaldab Alampäringu, et Alampäringu ei tohi sisaldada pesastatud KLAUSLIGA, mis määratleb teise CTE.
- Klausel ORDER BY ei saa kasutada CTE_query_definition, välja arvatud juhul, kui ÜLEMISEL klausel on määratud.
- Lause, mis on osa partii kasutamisel on CTE tuleb järgida enne, kui see lause semikooloniga.
- Kui kasutatakse koostatud sp_prepare, CTEs käituvad sarnaselt muude SELECT-laused PDW. Siiski CTEs kasutamisel osana koostatud sp_prepare CETAS käitumise saab edasi lükata, SQL serveri ja muu PDW laused sellepärast, et sidumine rakendatud sp_prepare. Kui valige viited CTE kasutab vales veerus, mida pole CTE, on sp_prepare läheb ilma tuvastamiseks viga, et tõrge visatakse ajal sp_execute hoopis.

## <a name="recursive-ctes"></a>Rekursiivsed CTEs

Rekursiivsed CTEs ei toeta SQL-i andmebaas.  Migraion Rekursiivsed CTE, võib olla veidi lõpule viidud ja parim protsess on murda selle üheks mitme juhiseid. Tavaliselt saab kasutada esitatavaks ja asustada ajutise tabeli, nagu te itereerima Rekursiivsed ajutised päringute üle. Kui ajutine tabel on täidetud saate andmeid tagastada siis kogumina ühe tulemi. Sarnane lähenemine on kasutatud lahendada `GROUP BY WITH CUBE` [Rühmitusalus klausel koos funktsiooniga rollup / kuup / komplekti rühmitamissuvandite][] artiklis.

## <a name="unsupported-system-functions"></a>Mittetoetatavad süsteemi funktsioonid

Samuti on mõned süsteemi funktsioonid, mis pole toetatud. Mõned andmete hoidmise kasutatakse enamasti võite leida tähtsamad on:

- NEWSEQUENTIALID()
- @@NESTLEVEL()
- @@IDENTITY()
- @@ROWCOUNT()
- ROWCOUNT_BIG
- ERROR_LINE()

Mõned järgmiste probleemide korral saate töötas ümber.

## <a name="rowcount-workaround"></a>@@ROWCOUNTlahendus

Lahendamiseks puudumine @@ROWCOUNT, salvestatud protseduuri, et tuua sys.dm_pdw_request_steps viimase ridade arvu ja seejärel käivitada `EXEC LastRowCount` pärast piirmäära lause.

```sql
CREATE PROCEDURE LastRowCount AS
WITH LastRequest as 
(   SELECT TOP 1    request_id
    FROM            sys.dm_pdw_exec_requests
    WHERE           session_id = SESSION_ID()
    AND             resource_class IS NOT NULL
    ORDER BY end_time DESC
),
LastRequestRowCounts as
(
    SELECT  step_index, row_count
    FROM    sys.dm_pdw_request_steps
    WHERE   row_count >= 0
    AND     request_id IN (SELECT request_id from LastRequest)
)
SELECT TOP 1 row_count FROM LastRequestRowCounts ORDER BY step_index DESC
;
```

## <a name="next-steps"></a>Järgmised sammud
Kõigi toetatud T-SQL-lausete täieliku loendi leiate [Transact-SQL-i teemadest][].

<!--Image references-->

<!--Article references-->
[ANSI ühenduste värskendused]: ./sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-update-statements
[ANSI-ühenduste kohta kustutab]: ./sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-delete-statements
[Ühenda lause]: ./sql-data-warehouse-develop-ctas.md#replace-merge-statements
[LISA. EXEC]: ./sql-data-warehouse-tables-temporary.md#modularizing-code
[Transact-SQL-i Teemad]: ./sql-data-warehouse-reference-tsql-statements.md

[kursorid]: ./sql-data-warehouse-develop-loops.md
[VALIGE. RAKENDUSSE]: ./sql-data-warehouse-develop-ctas.md#selectinto
[klausel koos funktsiooniga rollup Rühmitusalus / kuup / komplekti rühmitamissuvandite]: ./sql-data-warehouse-develop-group-by-options.md
[Pesastustasemete Lisaks 8]: ./sql-data-warehouse-develop-transactions.md
[vaadete kaudu värskendamine]: ./sql-data-warehouse-develop-views.md
[Valige muutuja kasutamine]: ./sql-data-warehouse-develop-variable-assignment.md
[dünaamiline SQL-i stringide puhul tippige MAX andmed]: ./sql-data-warehouse-develop-dynamic-sql.md

<!--MSDN references-->

<!--Other Web references-->
