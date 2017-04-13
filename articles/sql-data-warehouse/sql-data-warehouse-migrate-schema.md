<properties
   pageTitle="SQL-i andmebaas oma skeemi migreerimine | Microsoft Azure'i"
   description="Näpunäiteid migreerimine oma skeemi SQL Azure'i andmebaas arendamise lahendusi."
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
   ms.date="08/25/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="migrate-your-schema-to-sql-data-warehouse"></a>Oma skeemi migreerimine SQL-andmebaas#

Järgmised kokkuvõtted aidata teil mõista SQL Server ja SQL-i andmebaas, mis aitavad teil oma andmebaasi migreerimine erinevused.

## <a name="table-migration"></a>Tabeli migreerimine

Kui migreerimine tabelid, mida soovite tutvuda tabeli funktsioone SQL-i andmebaas tabelid.  [Tabeli ülevaade][] on hea koht alustamiseks.  Selles artiklis tutvustab teile kõige olulisemad kaalutlused nagu tabeli statistika eraldamine ja indekseerimise jaotuse tabeli loomisel.  See hõlmab ka mõned [mittetoetatavad tabelifunktsioonid][] ja lahendusi.

SQL-i andmebaas toetab levinud business andmetüübid.  Loendi artiklist [andmetüübid][] toetatud ja [pole toetatud andmetüübid][].  [Andmetüüpide][] artikkel sisaldab ka päringu tuvastamiseks [pole toetatud andmetüübid][].  Kui teie andmetüübid, kindlasti pilk [andmetüüp head tavad][].

## <a name="next-steps"></a>Järgmised sammud
Kui teil on migreeritud oma andmebaasi skeemi abil SQL-i andmebaas, jätkake ühega järgmistest artiklitest:

- [Andmete migreerimine][]
- [Migreerimine oma kood][]

SQL-i andmebaas heade tavade kohta leiate lisateavet artiklist [parimaid tavasid][] .

<!--Image references-->

<!--Article references-->
[Migreerimine oma kood]: ./sql-data-warehouse-migrate-code.md
[Andmete migreerimine]: ./sql-data-warehouse-migrate-data.md
[head tavad]: ./sql-data-warehouse-best-practices.md
[tabeli ülevaade]: ./sql-data-warehouse-tables-overview.md
[mittetoetatavad tabelifunktsioonid]: ./sql-data-warehouse-tables-overview.md#unsupported-table-features
[andmetüübid]: ./sql-data-warehouse-tables-data-types.md
[pole toetatud andmetüübid]: ./sql-data-warehouse-tables-data-types.md#unsupported-data-types
[andmetüüp head tavad]: ./sql-data-warehouse-tables-data-types.md#data-type-best-practices

<!--MSDN references-->


<!--Other Web references-->
