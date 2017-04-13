<properties
   pageTitle="Andmetüübid tabelid SQL-i andmebaas | Microsoft Azure'i"
   description="Andmetüüpide SQL Azure'i andmebaas tabelitega töötamise alustamine."
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

# <a name="data-types-for-tables-in-sql-data-warehouse"></a>Andmetüübid tabelid SQL-andmebaas

> [AZURE.SELECTOR]
- [Ülevaade][]
- [Andmetüübid][]
- [Levitamine][]
- [Index][]
- [Partition][]
- [Statistika][]
- [Ajutiste][]

Kõige sagedamini kasutatavate SQL-i andmebaas toetab andmetüübid.  Allpool on SQL-i andmebaas ei toeta andmetüüpide loendi.  Täiendavad üksikasjad andmetüüp toe leiate teemast [tabeli loomine][].

|**Toetatud andmetüübid**|||
|---|---|---|
[suur täisarv][]|[Decimal][]|[smallint][]|
[binaarsed][]|[hõljumine][]|[smallmoney][]|
[bit][]|[int][]|[sysname][]|
[char][]|[raha][]|[aeg][]|
[kuupäev][]|[NChar][]|[tinyint][]|
[kuupäev ja kellaaeg][]|[nvarchar][]|[ainuidentifikaatori][]|
[datetime2][]|[reaal][]|[varbinaar][]|
[datetimeoffset][]|[smalldatetime][]|[varchar][]|


## <a name="data-type-best-practices"></a>Andmetüüp head tavad

 Teie veerutüübid määratlemisel väikseim andmetüüp, mis toetab andmete kasutamine parandab päringu jõudluse. See on eriti oluline CHAR ja VARCHAR veerud. Kui veerus pikima väärtus on 25 märki, siis määratleda oma veeru VARCHAR(25). Vältige määratlemine suurte vaikimisi pikkusega Märgi kõik veerud. Lisaks määratleda veergude VARCHAR kui see on kõik, mis on vaja asemel kasutada [NVARCHAR][].  NVARCHAR(MAX) või VARCHAR(MAX) asemel kasutada NVARCHAR(4000) või VARCHAR(8000) kui võimalik.

## <a name="polybase-limitation"></a>Polybase piirangud

Kui kasutate Polybase laadimiseks tabelite, määratleda tabelite maksimaalne võimalik rea suurus, sh täispikkuses muutuva pikkusega veerge, ei ületa 32 767 baiti.  Kuigi saate määratleda rea muutuv pikkus andmetega, mida saate selle laius ületada ja laadimine BCP ridu, ei saa Polybase abil andmete laadimine.  Polybase tugi lai read lisatakse peagi.

## <a name="unsupported-data-types"></a>Pole toetatud andmetüübid

Kui migreerite andmebaasi teise SQL-i platvormi nagu Azure'i SQL-andmebaasi, kui migreerite, võib ilmneda teatud andmetüüpe, mis ei toeta SQL-i andmebaas.  Allpool on toetamata andmetüübid kui ka mõned alternatiivid pole toetatud andmetüübid asemel saate kasutada.

|Andmetüüp|Lahendus|
|---|---|
|[geomeetria][]|[varbinaar][]|
|[geograafia][]|[varbinaar][]|
|[hierarchyid][]|[nvarchar][] (4000)|
|[pilt][ntext,text,image]|[varbinaar][]|
|[teksti][ntext,text,image]|[varchar][]|
|[ntext][ntext,text,image]|[nvarchar][]|
|[sql_variant][]|Veeru tükeldamine tugevalt tipitud mitmesse veergu.|
|[Tabel][]|Teisendage Ajutised tabelid.|
|[ajatempli][]|Töödelda koodi kasutada [datetime2][] ja `CURRENT_TIMESTAMP` funktsioon.  Toetatakse ainult konstante nimega vaikesätted, seega current_timestamp ei saa määratleda vaikimisi piirangu. Kui teil on vaja migreerida rea versioon väärtused veerust ajatempli tipitud seejärel Kasuta [KAHENDARVU][](8) või [VARBINAAR][KAHENDARVU](8) pole NULL või tühi rida väärtusi.|
|[XML-i][]|[varchar][]|
|[kasutaja määratletud tüübid][]|võimaluse korral tagasi kohalikke tüübiks teisendada|
|vaikeväärtused|vaikeväärtuste toetavad literaalide ja ainult konstante.  Mitte-tarkadeks avaldisi või funktsioone, näiteks `GETDATE()` või `CURRENT_TIMESTAMP`, ei toetata.|

Selle all SQL-i saab kasutada oma praeguse tuvastamiseks veerge, mis on SQL-andmebaasi ei toeta SQL Azure'i andmebaas:

```sql
SELECT  t.[name], c.[name], c.[system_type_id], c.[user_type_id], y.[is_user_defined], y.[name]
FROM sys.tables  t
JOIN sys.columns c on t.[object_id]    = c.[object_id]
JOIN sys.types   y on c.[user_type_id] = y.[user_type_id]
WHERE y.[name] IN ('geography','geometry','hierarchyid','image','text','ntext','sql_variant','timestamp','xml')
 AND  y.[is_user_defined] = 1;
```

## <a name="next-steps"></a>Järgmised sammud

Lisateabe saamiseks vt artikleid [Tabeli ülevaade][Ülevaade], [levitamise tabeli][hajuta],[Index] [indekseerimine tabeli], [eraldamine tabeli][Partition], [Säilitada tabeli statistika][statistika] ja [Ajutised tabelid][ajutine].  Lisateavet heade tavade kohta leiate teemast [Andmete ladu SQL-i põhitõed][].

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
[tabeli loomine]: https://msdn.microsoft.com/library/mt203953.aspx
[suur täisarv]: https://msdn.microsoft.com/library/ms187745.aspx
[binaarsed]: https://msdn.microsoft.com/library/ms188362.aspx
[bit]: https://msdn.microsoft.com/library/ms177603.aspx
[char]: https://msdn.microsoft.com/library/ms176089.aspx
[kuupäev]: https://msdn.microsoft.com/library/bb630352.aspx
[kuupäev ja kellaaeg]: https://msdn.microsoft.com/library/ms187819.aspx
[datetime2]: https://msdn.microsoft.com/library/bb677335.aspx
[datetimeoffset]: https://msdn.microsoft.com/library/bb630289.aspx
[Decimal]: https://msdn.microsoft.com/library/ms187746.aspx
[hõljumine]: https://msdn.microsoft.com/library/ms173773.aspx
[geomeetria]: https://msdn.microsoft.com/library/cc280487.aspx
[geograafia]: https://msdn.microsoft.com/library/cc280766.aspx
[hierarchyid]: https://msdn.microsoft.com/library/bb677290.aspx
[int]: https://msdn.microsoft.com/library/ms187745.aspx
[raha]: https://msdn.microsoft.com/library/ms179882.aspx
[NChar]: https://msdn.microsoft.com/library/ms186939.aspx
[nvarchar]: https://msdn.microsoft.com/library/ms186939.aspx
[ntext,text,image]: https://msdn.microsoft.com/library/ms187993.aspx
[reaal]: https://msdn.microsoft.com/library/ms173773.aspx
[smalldatetime]: https://msdn.microsoft.com/library/ms182418.aspx
[smallint]: https://msdn.microsoft.com/library/ms187745.aspx
[smallmoney]: https://msdn.microsoft.com/library/ms179882.aspx
[sql_variant]: https://msdn.microsoft.com/library/ms173829.aspx
[sysname]: https://msdn.microsoft.com/library/ms186939.aspx
[Tabel]: https://msdn.microsoft.com/library/ms175010.aspx
[aeg]: https://msdn.microsoft.com/library/bb677243.aspx
[ajatempli]: https://msdn.microsoft.com/library/ms182776.aspx
[tinyint]: https://msdn.microsoft.com/library/ms187745.aspx
[ainuidentifikaatori]: https://msdn.microsoft.com/library/ms187942.aspx
[varbinaar]: https://msdn.microsoft.com/library/ms188362.aspx
[varchar]: https://msdn.microsoft.com/library/ms186939.aspx
[XML-i]: https://msdn.microsoft.com/library/ms187339.aspx
[kasutaja määratletud tüübid]: https://msdn.microsoft.com/library/ms131694.aspx
