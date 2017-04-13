<properties
   pageTitle="CSV-faili andmete laadimine Azure SQL-i Databaase (bcp) | Microsoft Azure'i"
   description="Suurus: väike andmete jaoks kasutatakse andmete importimiseks Azure'i SQL-andmebaasi bcp."
   services="sql-database"
   documentationCenter="NA"
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/13/2016"
   ms.author="carlrab"/>


# <a name="load-data-from-csv-into-azure-sql-data-warehouse-flat-files"></a>CSV andmete laadimine (tasapinnalise failid) SQL Azure'i andmebaas

Saate importida andmed CSV-faili Azure'i SQL-andmebaasi bcp käsurea kasuliku.

## <a name="before-you-begin"></a>Enne alustamist

### <a name="prerequisites"></a>Eeltingimused

Samm selle õpetuse kaudu, peate.

- On loogiline Azure'i SQL-andmebaasi server ja andmebaas
- Bcp käsurea kasuliku installitud
- Sqlcmd käsurea kasuliku installitud

Bcp ja sqlcmd Utiliidid saate [Microsofti allalaadimiskeskusest][]alla laadida.

### <a name="data-in-ascii-or-utf-16-format"></a>Andmete ASCII- või UTF-16-vormingus

Kui üritate selles õpetuses oma andmetega, teie andmeid on vaja kasutada ASCII- või UTF-16 kodeering Kuna bcp ei toeta UTF-8. 

## <a name="1-create-a-destination-table"></a>1. sihtkoha tabeli loomine

Kui sihttabelis määratleda SQL-andmebaasi tabelisse. Tabeli veergude peab vastama teie andmefaili iga rea andmed.

Tabeli loomiseks avage käsuviip ja kasutage sqlcmd.exe käivitage järgmine käsk:


```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    CREATE TABLE DimDate2
    (
        DateId INT NOT NULL,
        CalendarQuarter TINYINT NOT NULL,
        FiscalQuarter TINYINT NOT NULL
    )
    ;
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


## <a name="next-steps"></a>Järgmised sammud

SQL serveri andmebaasi migreerimine vt [SQL serveri andmebaasi migreerimine](sql-database-cloud-migrate.md).

<!--MSDN references-->
[bcp]: https://msdn.microsoft.com/library/ms162802.aspx
[CREATE TABLE syntax]: https://msdn.microsoft.com/library/mt203953.aspx

<!--Other Web references-->
[Microsofti allalaadimiskeskus]: https://www.microsoft.com/download/details.aspx?id=36433
