<properties
   pageTitle="Otsuste ja kodeerimise võtteid SQL-i andmebaas arengu kujundamiseks | Microsoft Azure'i"
   description="Arengu põhimõtet, kujundus otsuste, soovituste ja kodeerimise võtteid SQL-i andmebaas."
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
   ms.date="08/16/2016"
   ms.author="jrj;barbkess;sonyama"/>

# <a name="design-decisions-and-coding-techniques-for-sql-data-warehouse"></a>Kujundus otsuseid ja kodeerimise võtteid SQL-andmebaas

Vaadake läbi arengu artiklitest paremini mõistmiseks olulisemad otsuste, soovituste ja kodeerimise tehnika jaoks SQL-i andmebaas.

## <a name="key-design-decisions"></a>Olulisemad otsuseid.
Järgmised artiklid esiletõstmiseks mõned peamised mõisted ja kujundus otsuseid on vaja mõista teie jaotatud andmebaas, kasutades SQL-i andmebaas arendamiseks tehke järgmist.

- [ühendused][]
- [kokkulangevus][]
- [tehinguid][]
- [kasutaja määratletud skeemid][]
- [tabeli jaotuse.][]
- [tabeli registrite][]
- [tabeli sektsioonid][]
- [CTAS][]
- [statistika][]

## <a name="development-recommendations-and-coding-techniques"></a>Arengu soovitusi ja kodeerimise meetodid
Artiklitest esiletõstmiseks kindla kodeerimine tehnika, näpunäited ja soovitused arendada oma SQL-i andmebaas tehke järgmist.

- [Salvestatud toimingute][]
- [Sildid][]
- [vaated][]
- [Ajutised tabelid][]
- [dünaamiline SQL-i][]
- [Hoidke][]
- [Rühmita suvandid][]
- [muutuv ülesanne][]

## <a name="next-steps"></a>Järgmised sammud
Kui teil on läbi arengu artiklite Heitke pilk [Transact-SQL-i viide][] lehel Lisateavet toetatud süntaksi SQL-i andmebaas.

<!--Image references-->

<!--Article references-->
[kokkulangevus]: ./sql-data-warehouse-develop-concurrency.md
[ühendused]: ./sql-data-warehouse-connect-overview.md
[CTAS]: ./sql-data-warehouse-develop-ctas.md
[dünaamiline SQL-i]: ./sql-data-warehouse-develop-dynamic-sql.md
[Rühmita suvandid]: ./sql-data-warehouse-develop-group-by-options.md
[Sildid]: ./sql-data-warehouse-develop-label.md
[Hoidke]: ./sql-data-warehouse-develop-loops.md
[statistika]: ./sql-data-warehouse-tables-statistics.md
[Salvestatud toimingute]: ./sql-data-warehouse-develop-stored-procedures.md
[tabeli jaotuse.]: ./sql-data-warehouse-tables-distribute.md
[tabeli registrite]: ./sql-data-warehouse-tables-index.md
[tabeli sektsioonid]: ./sql-data-warehouse-tables-partition.md
[Ajutised tabelid]: ./sql-data-warehouse-tables-temporary.md
[tehinguid]: ./sql-data-warehouse-develop-transactions.md
[kasutaja määratletud skeemid]: ./sql-data-warehouse-develop-user-defined-schemas.md
[muutuv ülesanne]: ./sql-data-warehouse-develop-variable-assignment.md
[vaated]: ./sql-data-warehouse-develop-views.md
[Transact-SQL-i viide]: ./sql-data-warehouse-overview-reference.md

<!--MSDN references-->
[renaming objects]: https://msdn.microsoft.com/library/mt631611.aspx

<!--Other Web references-->
