<properties
   pageTitle="Juhend PolyBase kasutamine SQL-i andmebaas | Microsoft Azure'i"
   description="Juhised ja soovitused PolyBase kasutamine SQL-i andmebaas stsenaariumid."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="ckarst"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/30/2016"
   ms.author="cakarst;barbkess;sonyama"/>


# <a name="guide-for-using-polybase-in-sql-data-warehouse"></a>Juhend PolyBase kasutamine SQL-andmebaas

Sellest juhendist annab praktiline teave PolyBase kasutamine SQL-i andmebaas.

Alustamiseks, lugege teemat [laadi andmete PolyBase][] õpetuse.


## <a name="rotating-storage-keys"></a>Salvestusruumi klahvid pööramine

Aeg-ajalt soovite muuta kiirklahv bloobimälus turvalisuse põhjustel.

Selle toimingu sooritamiseks kõige elegantne viis on protsessi nimetatakse "pööramine klahve" järgida. Olete ehk märganud, et teil on kaks salvestusruumi klahvid bloobimälu salvestusruumi konto jaoks. See on nii, et te saate üleminek

Pööramine oma Azure storage konto klahvid on lihtne kolmest etapist

1. Looge teine andmebaas, kus otsinguulatuseks on määratud mandaadi põhjal teisene salvestusruumi kiirklahv
2. Teine vastavalt selle uue mandaadi välja välise andmeallika loomine
3. Pukseerida ja välise tabeleid, mis osutab uue välise andmeallika loomine

Kui teil on migreeritud kõik teie väline tabelid, mida soovite uue välise andmeallikaga seejärel saate teha saab puhastada tööülesanded.

1. Ripploendi esimese välisest andmeallikast
2. Ripploendi esimese andmebaasi veebirakendust mandaadi põhjal esmased kiirklahv
3. Azure'i sisse logida ja taastada valmis järgmine kord, kui esmane kiirklahv

## <a name="query-azure-blob-storage-data"></a>Salvestusruumi andmepäringuid Azure'i bloobimälu
Päringute vastu välise tabeleid kasutada lihtsalt tabeli nime nii, nagu see oli relatsiooniline tabelisse.

```sql
-- Query Azure storage resident data via external table.
SELECT * FROM [ext].[CarSensor_Data]
;
```

> [AZURE.NOTE] Päringu mõne välise tabeli võib nurjuda tõrke *"Päringu katkestatud--maksimaalne kõnest lävi oli jõudis väline lugemise ajal"*. See näitab, et välisandmete sisaldab *määrdunud* kirjed. Andmete kirje käsitletakse "määrdunud", kui tegelike andmete tüüpi/veergude arv ei vasta välise tabeli veergude määratlusi või kui andmed ei vasta määratud välise failivorming. Vea parandamiseks veenduge, et teie Välistabel ja välise faili vorming määratlused on õiged ja nende määratlused vastab välisandmete. Juhuks, kui väliste andmete alamhulk on määrdunud, saate need kirjed oma päringuid Hülga suvandite abil saate luua välise tabeli DDL Hülga.


## <a name="load-data-from-azure-blob-storage"></a>Laadi andmete Azure'i bloobimälu
Selles näites laadib andmed Azure'i bloobimälu SQL-i andmebaas andmebaasi.

Andmete talletamine otse eemaldab andmete edastamine aega päringuid. Andmete columnstore indeks salvestamiseks parandab päringu jõudluse analüüs päringuid, kuni 10 x.

Selles näites kasutatakse andmete laadimiseks loomine tabeli AS SELECT-lause. Uue tabeli pärib päringu nimega veerud. Nende veergude andmetüübid pärib välise tabeli määratlus.

Looge tabel AS valige on väga toimiv Transact-SQL-lauset, mis laadib andmed paralleelselt Arvuta sõlmed oma SQL-i andmebaas.  See algselt töötas oluliselt paralleelne töötlemine (MPP) mootori Analytics platvormi süsteemi jaoks ja SQL-i andmebaas on nüüd.

```sql
-- Load data from Azure blob storage to SQL Data Warehouse

CREATE TABLE [dbo].[Customer_Speed]
WITH
(   
    CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = HASH([CarSensor_Data].[CustomerKey])
)
AS
SELECT *
FROM   [ext].[CarSensor_Data]
;
```

Vaadake [luua tabeli AS valimine (Transact-SQL)][].

## <a name="create-statistics-on-newly-loaded-data"></a>Luua statistika äsja laaditud andmete

SQL Azure'i andmebaas ei veel tugiteenuste automaatne loomine või statistika värskendamise automaatselt.  Päringute parimaid tulemusi saada, on oluline, et kõik veerud kõigi tabelite luua statistika pärast esimest laadi või olulisi muudatuste andmeid.  Üksikasjalik selgitus statistika töötada rühma teemade teemat [statistika][] .  Allpool on lihtne näide, kuidas luua statistika esitatakse on laaditud selles näites.

```sql
create statistics [SensorKey] on [Customer_Speed] ([SensorKey]);
create statistics [CustomerKey] on [Customer_Speed] ([CustomerKey]);
create statistics [GeographyKey] on [Customer_Speed] ([GeographyKey]);
create statistics [Speed] on [Customer_Speed] ([Speed]);
create statistics [YearMeasured] on [Customer_Speed] ([YearMeasured]);
```

## <a name="export-data-to-azure-blob-storage"></a>Andmete eksportimine Azure'i bloobimälu
Selles jaotises kirjeldatakse lepingute andmete eksportimine andmebaas SQL Azure'i bloobimälu. Selles näites kasutatakse luua välise tabeli AS valige mis on väga toimiv Transact-SQL-lause paralleelselt andmete eksportimine Arvuta sõlmed.

Järgmises näites luuakse välise tabeli Weblogs2014 veergude määratlusi ja andmete dbo abil. Ajaveebide tabel. Kas välise tabeli määratlus on talletatud SQL-i andmebaas ja SELECT-lause tulemused eksporditakse "/ arhiivimine/log2014 /" kataloogi jaotises bloobimälu ümbris määratud andmeallikas. Andmed on eksporditud määratud teksti vormingus.

```sql
CREATE EXTERNAL TABLE Weblogs2014 WITH
(
    LOCATION='/archive/log2014/',
    DATA_SOURCE=azure_storage,
    FILE_FORMAT=text_file_format
)
AS
SELECT
    Uri,
    DateRequested
FROM
    dbo.Weblogs
WHERE
    1=1
    AND DateRequested > '12/31/2013'
    AND DateRequested < '01/01/2015';
```


## <a name="working-around-the-polybase-utf-8-requirement"></a>Töö ümber PolyBase UTF-8 nõue
Esita PolyBase toetab laadimine on UTF-8 kodeeringuga andmefaile. Kui UTF-8 kasutab sama märkide kodeering nimega ASCII PolyBase on ka tugi laadimise andmeid, mis on ASCII kodeeritud. Siiski PolyBase ei toeta teiste märkide kodeering näiteks UTF-16 / Unicode'i või laiendatud ASCII märgid. Laiendatud ASCII sisaldab diakriitikutega tähed nagu mis on levinud saksa keele.

Selle nõude lahendamiseks on parim vastus kirjutamiseks uuesti UTF-8 kodeeringus.

On selleks mitu võimalust. Allpool on PowerShelli kaudu kahel viisil:

### <a name="simple-example-for-small-files"></a>Lihtne näide small failide jaoks

Allpool on lihtne ühe rea PowerShelli skripti, mis loob faili.

```PowerShell
Get-Content <input_file_name> -Encoding Unicode | Set-Content <output_file_name> -Encoding utf8
```

Siiski kuigi see on lihtne viis uuesti kodeerida andmed on mingil kõige tõhusam. Io streaming järgmises näites on palju, kiiremini ja saavutab sama tulemuse.

### <a name="io-streaming-example-for-larger-files"></a>Suuremate failide IO Streaming näide

Proovi kood allpool on keerulisem, kuid see andmevoogu suunata selle andmeallika read on palju tõhusamaks. Seda moodust suuremat failide jaoks kasutada.

```PowerShell
#Static variables
$ascii = [System.Text.Encoding]::ASCII
$utf16le = [System.Text.Encoding]::Unicode
$utf8 = [System.Text.Encoding]::UTF8
$ansi = [System.Text.Encoding]::Default
$append = $False

#Set source file path and file name
$src = [System.IO.Path]::Combine("C:\input_file_path\","input_file_name.txt")

#Set source file encoding (using list above)
$src_enc = $ansi

#Set target file path and file name
$tgt = [System.IO.Path]::Combine("C:\output_file_path\","output_file_name.txt")

#Set target file encoding (using list above)
$tgt_enc = $utf8

$read = New-Object System.IO.StreamReader($src,$src_enc)
$write = New-Object System.IO.StreamWriter($tgt,$append,$tgt_enc)

while ($read.Peek() -ne -1)
{
    $line = $read.ReadLine();
    $write.WriteLine($line);
}
$read.Close()
$read.Dispose()
$write.Close()
$write.Dispose()
```

## <a name="next-steps"></a>Järgmised sammud
Andmete teisaldamine SQL-i andmebaas kohta leiate lisateavet teemast [andmete migreerimise ülevaade][].

<!--Image references-->

<!--Article references-->
[Load data with bcp]: ./sql-data-warehouse-load-with-bcp.md
[PolyBase andmete laadimine]: ./sql-data-warehouse-get-started-load-with-polybase.md
[Statistika]: ./sql-data-warehouse-tables-statistics.md
[andmete migreerimise ülevaade]: ./sql-data-warehouse-overview-migrate.md

<!--MSDN references-->
[supported source/sink]: https://msdn.microsoft.com/library/dn894007.aspx
[copy activity]: https://msdn.microsoft.com/library/dn835035.aspx
[SQL Server destination adapter]: https://msdn.microsoft.com/library/ms141095.aspx
[SSIS]: https://msdn.microsoft.com/library/ms141026.aspx

[CREATE EXTERNAL DATA SOURCE (Transact-SQL)]: https://msdn.microsoft.com/library/dn935022.aspx
[Luua välise FAILIVORMING (Transact-SQL-i)]: https://msdn.microsoft.com/library/dn935026) .aspx [välise tabeli loomine (Transact-SQL-i)]: https://msdn.microsoft.com/library/dn935021.aspxx

[DROP EXTERNAL DATA SOURCE (Transact-SQL)]: https://msdn.microsoft.com/library/mt146367.aspx
[DROP EXTERNAL FILE FORMAT (Transact-SQL)]: https://msdn.microsoft.com/library/mt146379.aspx
[DROP EXTERNAL TABLE (Transact-SQL)]: https://msdn.microsoft.com/library/mt130698.aspx

[Looge tabel AS valimine (Transact-SQL)]: https://msdn.microsoft.com/library/mt204041.aspx
[INSERT...SELECT (Transact-SQL)]: https://msdn.microsoft.com/library/ms174335.aspx
[CREATE MASTER KEY (Transact-SQL)]: https://msdn.microsoft.com/library/ms174382.aspx
[CREATE CREDENTIAL (Transact-SQL)]: https://msdn.microsoft.com/library/ms189522.aspx
[CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)]: https://msdn.microsoft.com/library/mt270260.aspx
[DROP CREDENTIAL (Transact-SQL)]: https://msdn.microsoft.com/library/ms189450.aspx

<!-- External Links -->
