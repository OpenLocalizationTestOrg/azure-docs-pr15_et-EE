<properties
   pageTitle="SQL serveri andmete laadimine (bcp) SQL Azure'i andmebaas | Microsoft Azure'i"
   description="Suurus: väike andmete jaoks kasutatakse bcp SQL serveri andmete eksportimine tasapinnalise failid ja importida andmed otse SQL Azure'i andmebaas."
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
   ms.date="06/30/2016"
   ms.author="lodipalm;barbkess;sonyama"/>


# <a name="load-data-from-sql-server-into-azure-sql-data-warehouse-flat-files"></a>SQL serveri andmete laadimine (tasapinnalise failid) SQL Azure'i andmebaas

> [AZURE.SELECTOR]
- [SSIS-I](sql-data-warehouse-load-from-sql-server-with-integration-services.md)
- [PolyBase](sql-data-warehouse-load-from-sql-server-with-polybase.md)
- [BCP](sql-data-warehouse-load-from-sql-server-with-bcp.md)

Väike andmekogumite, saate bcp käsurea kasuliku SQL serveri andmete eksportimine ja seejärel laadima otse SQL Azure'i andmebaas.

Selles õppetükis saate bcp abil.

- SQL serveri tabeli eksportimine bcp käsu abil (või luua lihtsa näidisfaili)
- Importida lamefailiga tabeli SQL-i andmebaas.
- Luua statistika laaditud andmed.

>[AZURE.VIDEO loading-data-into-azure-sql-data-warehouse-with-bcp]

## <a name="before-you-begin"></a>Enne alustamist

### <a name="prerequisites"></a>Eeltingimused

Samm selle õpetuse kaudu, peate.

- Andmebaasi SQL-i andmebaas
- Bcp käsurea kasuliku installitud
- Sqlcmd käsurea kasuliku installitud

Bcp ja sqlcmd Utiliidid saate [Microsofti allalaadimiskeskusest][]alla laadida.

### <a name="data-in-ascii-or-utf-16-format"></a>Andmete ASCII- või UTF-16-vormingus

Kui üritate selles õpetuses oma andmetega, teie andmeid on vaja kasutada ASCII- või UTF-16 kodeering Kuna bcp ei toeta UTF-8. 

PolyBase toetab UTF-8, kuid veel ei toeta UTF-16. Pange tähele, et kui soovite ühendada bcp PolyBase saate muuta andmete UTF-8 pärast seda, kui see on eksporditud SQL serverist. 


## <a name="1-create-a-destination-table"></a>1. sihtkoha tabeli loomine

Määratleda tabeli SQL-i andmebaas, millest saab sihttabel laadi jaoks. Tabeli veergude peab vastama teie andmefaili iga rea andmed.

Tabeli loomiseks avage käsuviip ja kasutage sqlcmd.exe käivitage järgmine käsk:


```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    CREATE TABLE DimDate2
    (
        DateId INT NOT NULL,
        CalendarQuarter TINYINT NOT NULL,
        FiscalQuarter TINYINT NOT NULL
    )
    WITH
    (
        CLUSTERED COLUMNSTORE INDEX,
        DISTRIBUTION = ROUND_ROBIN
    );
"
```


## <a name="2-create-a-source-data-file"></a>2. allika andmefaili loomine.

Avage Notepad ja järgmised read andmete kopeerimine uue teksti faili ja seejärel salvestage see fail teie kohaliku temp kataloogi C:\Temp\DimDate2.txt. Andmed on ASCII-vormingus.

```
20150301,1,3
20150501,2,4
20151001,4,2
20150201,1,3
20151201,4,2
20150801,3,1
20150601,2,4
20151101,4,2
20150401,2,4
20150701,3,1
20150901,3,1
20150101,1,3
```

(Valikuline) Oma andmete eksportimine SQL Serveri andmebaas, avage käsuviip ja käivitage järgmine käsk. Asendage TableName, serverinimi, DatabaseName, kasutajanimi ja parool oma andmetega.

```sql
bcp <TableName> out C:\Temp\DimDate2_export.txt -S <ServerName> -d <DatabaseName> -U <Username> -P <Password> -q -c -t ','
```



## <a name="3-load-the-data"></a>3. andmete laadimine
Andmete laadimine, avage käsuviip ja käivitage järgmine käsk, asendades väärtused serverinime, andmebaasi nimi, kasutajanimi ja parool oma andmetega.

```sql
bcp DimDate2 in C:\Temp\DimDate2.txt -S <ServerName> -d <DatabaseName> -U <Username> -P <password> -q -c -t  ','
```

Selle käsu abil saate kontrollida, kas andmed on laaditud õigesti

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "SELECT * FROM DimDate2 ORDER BY 1;"
```

Tulemused peaks välja nägema umbes järgmine:

DateId |CalendarQuarter |FiscalQuarter
----------- |--------------- |-------------
20150101 |1 |3
20150201 |1 |3
20150301 |1 |3
20150401 |2 |4
20150501 |2 |4
20150601 |2 |4
20150701 |3 |1
20150801 |3 |1
20150801 |3 |1
20151001 |4 |2
20151101 |4 |2
20151201 |4 |2

## <a name="4-create-statistics"></a>4. statistika loomine

SQL-i andmebaas ei veel tugiteenuste automaatne loomine või statistika automaatne uuendamine. Päringu parimaid tulemusi saada, on oluline luua statistika kõigi veergude kõigi tabelite pärast esimest laadi või olulisi muudatuste andmeid. Statistika üksikasjaliku selgituse leiate teemast [statistika][]. 

Käivitage järgmine käsk statistika äsja laaditud tabelit luua.

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    create statistics [DateId] on [DimDate2] ([DateId]);
    create statistics [CalendarQuarter] on [DimDate2] ([CalendarQuarter]);
    create statistics [FiscalQuarter] on [DimDate2] ([FiscalQuarter]);
"
```

## <a name="5-export-data-from-sql-data-warehouse"></a>5. andmete eksportimine SQL-andmebaas
Lõbus, saate eksportida andmed, mida saate just laaditud tagasi välja SQL-i andmebaas.  Käsk Ekspordi on täpselt sama, mis eksportimise SQL serverist.

On siiski tulemuste erinevus. Kuna andmete eksportimisel jaotatud asukohtadele SQL-i andmebaas, talletatakse andmeid iga MSDN kirjutab see andmed väljundfail. Andmete väljundfail järjestuse on tõenäoliselt erinevas Sisestuskeel faili andmete järjestust.

### <a name="export-a-table-and-compare-exported-results"></a>Eksportida tabeli ja võrrelda tulemusi eksporditud

Eksporditud andmete kuvamiseks avage käsuviip ja selle käsu abil oma parameetrid. Serveri nimi on loogiline Azure SQL serveri nimi.

```sql
bcp DimDate2 out C:\Temp\DimDate2_export.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t ','
```
Saate kontrollida andmed on eksporditud õigesti, avades uue faili. Faili andmed peaksid olema samad allpool olevat teksti, kuid tõenäoliselt sorditakse erinevas järjestuses.

```
20150301,1,3
20150501,2,4
20151001,4,2
20150201,1,3
20151201,4,2
20150801,3,1
20150601,2,4
20151101,4,2
20150401,2,4
20150701,3,1
20150901,3,1
20150101,1,3
```

### <a name="export-the-results-of-a-query"></a>Ekspordi tulemused päringu

BCP **queryout** funktsiooni abil saate eksportida asemel terve tabeli eksportida päringu tulemused. 

## <a name="next-steps"></a>Järgmised sammud
Laadimise ülevaate leiate teemast [Laadi andmeid SQL-i andmebaas][].
Veel arengu näpunäiteid teemast [SQL-i andmebaas arengu ülevaade][].
Lugege lisateavet tabeli loomise kohta SQL-i andmebaas [Tabeli ülevaade][] või [tabeli loomine süntaks][] .

<!--Image references-->

<!--Article references-->

[Andmete laadimine SQL-andmebaas]: ./sql-data-warehouse-overview-load.md
[SQL-i andmebaas arengu ülevaade]: ./sql-data-warehouse-overview-develop.md
[Tabeli ülevaade]: ./sql-data-warehouse-tables-overview.md
[Statistika]: ./sql-data-warehouse-tables-statistics.md

<!--MSDN references-->
[bcp]: https://msdn.microsoft.com/library/ms162802.aspx
[TABELI loomine süntaks]: https://msdn.microsoft.com/library/mt203953.aspx

<!--Other Web references-->
[Microsofti allalaadimiskeskus]: https://www.microsoft.com/download/details.aspx?id=36433
