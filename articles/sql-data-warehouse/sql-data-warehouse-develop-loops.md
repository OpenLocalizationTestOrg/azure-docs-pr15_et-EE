<properties
   pageTitle="SQL-i andmebaas silmuseid | Microsoft Azure'i"
   description="Näpunäiteid Transact-SQL-i silmuseid ja asendades kursorid rakenduses SQL Azure'i andmebaas arendamise lahendusi."
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

# <a name="loops-in-sql-data-warehouse"></a>Silmuseid SQL-andmebaas
SQL-i andmebaas toetab [ajal][] tsükkel lause plokid korduvalt teostamiseks. See jätkub jaoks, kui määratud tingimused on tõesed või kuni koodi spetsiaalselt lõpeb tsükkel kasutamine selle `BREAK` märksõna. Silmuseid on eriti kasulik asendamine kursorid määratletud SQL-koodi. Ainult erinevate Õnneks edasi, on enamiku kursorid, mis on kirjutatud SQL-koodi lugeda. Seetõttu [ajal] silmuseid on suurepärane alternatiiv, kui teid huvitab võttes asendada ühe.

## <a name="leveraging-loops-and-replacing-cursors-in-sql-data-warehouse"></a>Kasutamine silmuseid ja asendamisel kursorid rakenduses SQL-andmebaas
Siiski enne sukeldumise pea esmalt peate peaks endale järgmise: "võiks olla selle kursor uuesti kirjutatud kirjeldatud põhine toimingute?". Paljudel juhtudel vastus jah ja on sageli parim lahendus. Seadmine vastavalt toiming sageli teostab märkimisväärselt kiiremini iteratiivne, ridahaaval lähenemisviisi.

Edasi kirjutuskaitstud kursorid saate hõlpsalt asendada Silmukoiminen ehitada. Allpool on lihtne näide. Selles näites kood värskendab statistika iga andmebaasi tabelisse. Tabelite kursis üle iterating oleme võimalus käsu järjest iga.

Esmalt looge ajutine tabel, mis sisaldab kordumatuid reanumbri, mis tähistavad üksikud laused:

```
CREATE TABLE #tbl
WITH
( DISTRIBUTION = ROUND_ROBIN
)
AS
SELECT  ROW_NUMBER() OVER(ORDER BY (SELECT NULL)) AS Sequence
,       [name]
,       'UPDATE STATISTICS '+QUOTENAME([name]) AS sql_code
FROM    sys.tables
;
```

Teiseks lähtestab täitma kursis muutujaid:

```
DECLARE @nbr_statements INT = (SELECT COUNT(*) FROM #tbl)
,       @i INT = 1
;
```

Nüüd pidev üle laused täitmine ühe korraga:

```
WHILE   @i <= @nbr_statements
BEGIN
    DECLARE @sql_code NVARCHAR(4000) = (SELECT sql_code FROM #tbl WHERE Sequence = @i);
    EXEC    sp_executesql @sql_code;
    SET     @i +=1;
END
```

Lõpuks tabeli ajutine loodud esimene samm

```
DROP TABLE #tbl;
```


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->

## <a name="next-steps"></a>Järgmised sammud
Veel arengu näpunäiteid teemast [arengu ülevaade][].

<!--Image references-->

<!--Article references-->
[arengu ülevaade]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[AEGA]: https://msdn.microsoft.com/library/ms178642.aspx


<!--Other Web references-->
