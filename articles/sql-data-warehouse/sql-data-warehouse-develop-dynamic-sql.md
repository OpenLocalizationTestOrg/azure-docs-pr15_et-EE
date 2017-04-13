<properties
   pageTitle="Dünaamiline SQL SQL-i andmebaas | Microsoft Azure'i"
   description="Näpunäiteid kasutamise dünaamiline SQL Azure'i SQL-i andmebaas arendamise lahendusi."
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

# <a name="dynamic-sql-in-sql-data-warehouse"></a>Dünaamiline SQL SQL-andmebaas
SQL-i andmebaas rakenduse koodi väljatöötamisel peate abil dünaamiline sql paindlik, üldise ja muutuv lahendusi. SQL-i andmebaas ei toeta praegu bloobimälu andmetüübid. See piirata oma stringide suuruse bloobimälu tüübid on nii varchar(max) ja nvarchar(max) tüübid. Kui olete kasutanud järgmist tüüpi rakenduse koodis kui hoone väga suurte stringid, pead murda koodi tükkideks ja kasutada EXEC lause asemel.

Lihtne näide:

```sql
DECLARE @sql_fragment1 VARCHAR(8000)=' SELECT name '
,       @sql_fragment2 VARCHAR(8000)=' FROM sys.system_views '
,       @sql_fragment3 VARCHAR(8000)=' WHERE name like ''%table%''';

EXEC( @sql_fragment1 + @sql_fragment2 + @sql_fragment3);
```

Kui string on lühike saate [sp_executesql Klõpsake][] nagu tavaliselt.

> [AZURE.NOTE] Dünaamiline SQL täidetakse laused ikkagi kehtivad kõigi TSQL valideerimisreegleid.

## <a name="next-steps"></a>Järgmised sammud
Veel arengu näpunäiteid teemast [arengu ülevaade][].

<!--Image references-->

<!--Article references-->
[arengu ülevaade]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[sp_executesql Klõpsake]: https://msdn.microsoft.com/library/ms188001.aspx

<!--Other Web references-->
