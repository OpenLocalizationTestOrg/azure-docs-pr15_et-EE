<properties
   pageTitle="Dokumendi päringute siltide kasutamine SQL-i andmebaas | Microsoft Azure'i"
   description="Näpunäiteid dokumendi päringute siltide kasutamine SQL Azure'i andmebaas arendamise lahendusi."
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

# <a name="use-labels-to-instrument-queries-in-sql-data-warehouse"></a>Dokumendi päringute siltide kasutamine SQL-andmebaas
SQL-i andmebaas toetab mõiste nimega päring sildid. Enne dokumendi mis tahes sügavus Heitkem pilk üks näide:

```sql
SELECT *
FROM sys.tables
OPTION (LABEL = 'My Query Label')
;
```

Selle viimase rea sildid päringu stringi "Minu päringu märgistus". See on eriti kasulik, kui silt on päringu-võimalik on DMVs kaudu. See annab meile abil välja selgitada probleemi päringud ja tuvastada edenemise ETL run kaudu.

Hea nimetamistava tõesti aitab siin. Näiteks midagi ' projekti: TOIMINGUT: lause: KOMMENTAARI ' aitab tuvastada päringu seas kõik kood Juhtelemendi allikas.

Sildi otsimiseks saate kasutada järgmist päringut, mis kasutab dünaamilise halduse vaadete:

```sql
SELECT  *
FROM    sys.dm_pdw_exec_requests r
WHERE   r.[label] = 'My Query Label'
;
```

> [AZURE.NOTE] See on oluline, et te Murra nurksulgude või jutumärkidega sildi sõna ümber, kui päringu. Sildi on reserveeritud sõna ja põhjustada tõrke, kui see on eraldatud pole.


## <a name="next-steps"></a>Järgmised sammud
Veel arengu näpunäiteid teemast [arengu ülevaade][].

<!--Image references-->

<!--Article references-->
[arengu ülevaade]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->

<!--Other Web references-->
