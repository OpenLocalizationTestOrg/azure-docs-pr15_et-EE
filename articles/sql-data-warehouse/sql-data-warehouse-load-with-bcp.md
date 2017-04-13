<properties
   pageTitle="Kasutage bcp andmete SQL-i andmebaas laadimiseks | Microsoft Azure'i"
   description="Siit saate teada, millised bcp on ja kuidas seda kasutada andmete hoidmise stsenaariumid."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="twounder"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="10/10/2016"
   ms.author="mausher;barbkess;sonyama"/>


# <a name="load-data-with-bcp"></a>Bcp andmete laadimine

> [AZURE.SELECTOR]
- [Redgate](sql-data-warehouse-load-with-redgate.md)  
- [Andmete Factory](sql-data-warehouse-get-started-load-with-azure-data-factory.md)  
- [PolyBase](sql-data-warehouse-get-started-load-with-polybase.md)  
- [BCP](sql-data-warehouse-load-with-bcp.md)


**[BCP][]** on käsurea hulgi laadi kasuliku, mis võimaldab kopeerida andmeid SQL serveri, andmefailidest ja SQL-i andmebaas. Kasutage bcp suure hulga ridade importida tabelid SQL-i andmebaas või andmete eksportimine SQL serveri tabelid andmefailid. Välja arvatud juhul, kui kasutatakse queryout suvandiga, bcp nõua Transact-SQL teadmisi.

BCP on kiire ja lihtne viis liikuda väiksema andmekogumi ja sealt andmebaasi SQL-i andmebaas. Täpse andmeid, mis on soovitatav laadi/ekstrakti kaudu bcp sõltub sisse teie võrguühendus Azure data Centeri kaudu.  Üldiselt mõõtme tabeleid saab laaditud ja ekstraktimist kergesti koos bcp, kuid bcp on soovitatav laadimine või ekstraktimiseks suuri andmemahtusid.  Polybase on soovitatav tööriista laadimine ja ekstraktimiseks suuri andmemahtusid kasutamine oluliselt paralleelne töötlemine arhitektuur SQL-i andmebaas paremini tagamisse.

Bcp saate teha järgmist.

- Lihtne käsurea kasuliku abil andmete laadimine SQL-i andmebaas.
- Lihtne käsurea kasuliku kasutamine andmete eraldamiseks SQL-i andmebaas.

Selle õpetuse näitab teile, kuidas soovite:

- Andmete importimine abil soovitud bcp käsk tabel
- Tabeli žestid bcp välja andmete eksportimine

>[AZURE.VIDEO loading-data-into-azure-sql-data-warehouse-with-bcp]

## <a name="prerequisites"></a>Eeltingimused

Samm selle õpetuse kaudu, peate.

- Andmebaasi SQL-i andmebaas
- Bcp käsurea kasuliku installitud
- SQLCMD käsurea kasuliku installitud

>[AZURE.NOTE] Bcp ja sqlcmd Utiliidid saate [Microsofti allalaadimiskeskusest][]alla laadida.

## <a name="import-data-into-sql-data-warehouse"></a>Andmete importimine SQL-andmebaas

Selles õpetuses tabeli loomine SQL Azure'i andmebaas ja tabel andmete importimine.

### <a name="step-1-create-a-table-in-azure-sql-data-warehouse"></a>Samm 1: Tabeli loomine SQL Azure'i andmebaas

Käsureale, kasutada sqlcmd päringut tabeli loomiseks oma eksemplari.

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

>[AZURE.NOTE] Lugege lisateavet tabeli loomise SQL-i andmebaas ja asendaja-klauslis saadaolevad suvandid [Tabeli ülevaade][] või [CREATE TABLE süntaks][] .

### <a name="step-2-create-a-source-data-file"></a>Samm 2: Allika andmefaili loomine.

Avage Notepad ja järgmised read andmete kopeerimine uue teksti faili ja seejärel salvestage see fail teie kohaliku temp kataloogi C:\Temp\DimDate2.txt.

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

> [AZURE.NOTE] See on oluline meeles pidada, et bcp.exe ei toeta kodeeringut UTF-8 fail. Kasutage ASCII või UTF-16 kodeeritud failide bcp.exe kasutamisel.

### <a name="step-3-connect-and-import-the-data"></a>Samm 3: Ühenduse loomiseks ja andmete importimine
Bcp abil saate ühendada ja importida andmed vastavalt vajadusele väärtuste asendamine järgmine käsk:

```sql
bcp DimDate2 in C:\Temp\DimDate2.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t  ','
```

Andmed on laaditud, käitades järgmine päring sqlcmd abil saate kontrollida.

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "SELECT * FROM DimDate2 ORDER BY 1;"
```

See peaks tagastavad järgmised tulemused.

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

### <a name="step-4-create-statistics-on-your-newly-loaded-data"></a>Samm 4: Luua oma äsja laaditud andmete statistika

SQL Azure'i andmebaas ei veel tugiteenuste automaatne loomine või statistika värskendamise automaatselt. Päringute parimaid tulemusi saada, on oluline, et kõik veerud kõigi tabelite luua statistika pärast esimest laadi või olulisi muudatuste andmeid. Üksikasjalik selgitus statistika töötada rühma teemade teemat [statistika][] . Allpool on lihtne näide, kuidas luua laaditud selles näites on esitatakse statistika

Käivita sqlcmd käsuviiba kaudu luua statistika järgmistest:

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    create statistics [DateId] on [DimDate2] ([DateId]);
    create statistics [CalendarQuarter] on [DimDate2] ([CalendarQuarter]);
    create statistics [FiscalQuarter] on [DimDate2] ([FiscalQuarter]);
"
```

## <a name="export-data-from-sql-data-warehouse"></a>Andmete eksportimine SQL-andmebaas
Selles õpetuses loote andmefaili SQL-i andmebaas tabelist. Me ei lõime üle andmed eksportida uue andmefaili nimega DimDate2_export.txt.

### <a name="step-1-export-the-data"></a>Samm 1: Eksportida andmed

Bcp kasuliku kasutamisel saate ühenduse loomiseks ja eksportida andmed, kasutades vastavalt vajadusele väärtuste asendamine järgmine käsk:

```sql
bcp DimDate2 out C:\Temp\DimDate2_export.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t ','
```
Saate kontrollida andmed on eksporditud õigesti, avades uue faili. Faili andmed peaksid olema samad allpool olevat teksti:

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

>[AZURE.NOTE] Hajutatud süsteemide laadist tellimuse andmed ei pruugi olla sama üle andmebaase SQL-i andmebaas. Teine võimalus on kasutada funktsiooni **queryout** BCP kirjutamine päringu ekstrakti asemel terve tabeli eksportida.

## <a name="next-steps"></a>Järgmised sammud
Laadimise ülevaate leiate teemast [Laadi andmeid SQL-i andmebaas][].
Veel arengu näpunäiteid teemast [SQL-i andmebaas arengu ülevaade][].

<!--Image references-->

<!--Article references-->

[Andmete laadimine SQL-andmebaas]: ./sql-data-warehouse-overview-load.md
[SQL-i andmebaas arengu ülevaade]: ./sql-data-warehouse-overview-develop.md
[Tabeli ülevaade]: ./sql-data-warehouse-tables-overview.md
[Statistika]: ./sql-data-warehouse-tables-statistics.md

<!--MSDN references-->
[BCP]: https://msdn.microsoft.com/library/ms162802.aspx
[TABELI loomine süntaks]: https://msdn.microsoft.com/library/mt203953.aspx

<!--Other Web references-->
[Microsofti allalaadimiskeskus]: https://www.microsoft.com/download/details.aspx?id=36433
