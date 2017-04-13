<properties
   pageTitle="SQL-i andmebaas muutujate määramine | Microsoft Azure'i"
   description="Näpunäiteid Transact-SQL-i muutujate SQL Azure'i andmebaas määramine lahendusi."
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

# <a name="assign-variables-in-sql-data-warehouse"></a>Määrata muutujate SQL-andmebaas
SQL-i andmebaas muutujate määratakse, kasutades funktsiooni `DECLARE` lause või `SET` lause.

Kõik järgmised on täiesti kehtiv võimalust seada muutuja väärtuse.

## <a name="setting-variables-with-declare"></a>Sätte muutujate DEKLAREERIMISLAUSET

Muutujate DEKLAREERIMISLAUSET käivitamine on üks kõige paindlik võimalust seada muutuja väärtuse SQL-i andmebaas.

```sql
DECLARE @v  int = 0
;
```

DEKLAREERIMISLAUSET abil saate määrata rohkem kui üks muutuja korraga. Te ei saa kasutada `SELECT` või `UPDATE` toiming:

```sql
DECLARE @v  INT = (SELECT TOP 1 c_customer_sk FROM Customer where c_last_name = 'Smith')
,       @v1 INT = (SELECT TOP 1 c_customer_sk FROM Customer where c_last_name = 'Jones')
;
```

Ei saa vooluvõrguga ja kasutada sama DEKLAREERIMISLAUSET lauses muutujana. **Illustreerivad punkt järgmises näites on lubatud nimega** @p1 sama DEKLAREERIMISLAUSET lauses kasutatud on nii lähtestada. See põhjustada tõrke.

```sql
DECLARE @p1 int = 0
,       @p2 int = (SELECT COUNT (*) FROM sys.types where is_user_defined = @p1 )
;
```

## <a name="setting-values-with-set"></a>Väärtused on seatud
On väga levinud meetod ühte seadmine.

Kõik alltoodud näiteid on säte on seatud muutujana lubatud võimalust.

```sql
SET     @v = (Select max(database_id) from sys.databases);
SET     @v = 1;
SET     @v = @v+1;
SET     @v +=1;
```

Üks muutuja saate määrata ainult korraga on seatud. Nagu näha kohal on liitindeksi tehtemärkide lubatud.

## <a name="limitations"></a>Piirangud
Te ei saa kasutada valiku- või muutuja.


## <a name="next-steps"></a>Järgmised sammud
Veel arengu näpunäiteid teemast [arengu ülevaade][].

<!--Image references-->

<!--Article references-->
[arengu ülevaade]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->

<!--Other Web references-->
