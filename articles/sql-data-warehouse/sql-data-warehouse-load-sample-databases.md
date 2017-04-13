<properties
   pageTitle="Näidisandmete laadimine SQL-i andmebaas | Microsoft Azure'i"
   description="Näidisandmete laadimine SQL-andmebaas"
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
   ms.date="08/16/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

#<a name="load-sample-data-into-sql-data-warehouse"></a>Näidisandmete laadimine SQL-andmebaas

Järgmiste juhiste lihtsa laadimise ja päringu Adventure Works näidisandmebaasi. Nende skriptide esmalt abil sqlcmd käivitada SQL-i, mis loob tabelid ja vaated. Kui tabelid on loodud, kuvatakse skriptide bcp abil andmete laadimine.  Kui teil pole veel sqlcmd ja bcp installitud, järgige neid linke [bcp][] installimiseks ja [sqlcmd][]installimiseks.

##<a name="load-sample-data"></a>Laadi näidisandmed

1. [Adventure Worksi näidisskriptid SQL-i andmebaas][] zip faili alla laadida.

2. Väljavõte faile alla zip kataloogi teie kohalikus arvutis.

3. Aw_create.bat ekstraktitud faili redigeerimine ja määrake järgmised muutujad leitud faili ülaosas.  Veenduge, et pole tühiku vahele jätta on "=" ja parameeter.  Allpool on näited teie muudatused võivad ilme.

    ```
    server=mylogicalserver.database.windows.net
    user=mydwuser
    password=Mydwpassw0rd
    database=mydwdatabase
    ```

4. Käivitage Windows cmd käsuviiba kaudu, redigeeritud aw_create.bat.  Olla kindel, kuhu salvestasite oma redigeeritud versiooni aw_create.bat kataloogis.
Kas see skript...
    * Mis tahes Adventure Worksi tabelite või vaadete, mis on juba olemas andmebaasi kukutage
    * Adventure Worksi tabelite ja vaadete loomine
    * Iga Adventure Worksi tabeli abil bcp laadimine
    * Adventure Worksi tabeli iga rea loendab valideerimiseks
    * Adventure Worksi iga tabeli iga veeru statistika kogumiseks


##<a name="query-sample-data"></a>Päringu näidisandmed

Kui olete Näidisandmete laaditud oma SQL-i andmebaas, saate mõne päringute kiiresti käivitada.  Päringu käivitamiseks ühenduse oma vastloodud Adventure Worksi andmebaasi Azure SQL-i DW Visual Studio ja SSDT, nagu on kirjeldatud [Visual Studio abil päringu][] dokumendi.

Näide: lihtsa select-lause töötajate kogu vajaliku teabe saamiseks

```sql
SELECT * FROM DimEmployee;
```

Keerukama päringu abil importida, nt GROUP BY vaadata müügi kogusumma iga päev, näiteks:

```sql
SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales
FROM FactInternetSales
GROUP BY OrderDateKey
ORDER BY OrderDateKey;
```

Näide valik koos filtreerimiseks tellimusi enne teatud kuupäeval WHERE-klausel on järgmine.

```
SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales
FROM FactInternetSales
WHERE OrderDateKey > '20020801'
GROUP BY OrderDateKey
ORDER BY OrderDateKey;
```

SQL-i andmebaas toetab peaaegu kõik T-SQL-i importida, mis toetab SQL serveri.  Meie [migreerimine kood][] dokumendid on avaldatud erinevusi.

## <a name="next-steps"></a>Järgmised sammud
Nüüd, kui olete olnud võimalus mõned näidisandmed päringuid proovida, lugege teemat Kuidas [töötada][], [Laadi][]või [migreerida][] SQL-i andmebaas.

<!--Image references-->

<!--Article references-->
[migreerimine]: sql-data-warehouse-overview-migrate.md
[töötada]: sql-data-warehouse-overview-develop.md
[Laadi]: sql-data-warehouse-overview-load.md
[päring koos Visual Studio]: sql-data-warehouse-query-visual-studio.md
[migreerimine kood]: sql-data-warehouse-migrate-code.md
[Installige bcp]: sql-data-warehouse-load-with-bcp.md
[Installige sqlcmd]: sql-data-warehouse-get-started-connect-sqlcmd.md

<!--Other Web references-->
[Adventure Works näidisskriptid jaoks SQL-andmebaas]: https://migrhoststorage.blob.core.windows.net/sqldwsample/AdventureWorksSQLDW2012.zip
