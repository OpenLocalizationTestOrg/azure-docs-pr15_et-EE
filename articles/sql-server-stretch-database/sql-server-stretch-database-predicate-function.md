<properties
    pageTitle="Valige read migreerimiseks funktsioon filter (venitamine andmebaas) abil | Microsoft Azure'i"
    description="Saate teada, kuidas valida ridade filtreerimine funktsiooni abil migreerida."
    services="sql-server-stretch-database"
    documentationCenter=""
    authors="douglaslMS"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-server-stretch-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/28/2016"
    ms.author="douglasl"/>

# <a name="select-rows-to-migrate-by-using-a-filter-function-stretch-database"></a>Valige read migreerida, kasutades funktsioon filter (venitamine andmebaas)

Kui salvestate külma andmete eraldi tabelisse, saate konfigureerida venitamine andmebaasi migreerimine tervet tabelit. Kui teie tabel sisaldab nii ja kuuma andmeid, aga, saate määrata migreerida ridade filtreerimine funktsiooni. Filtri predikaat on Tekstisisese tabel\-hinnatud funktsioon. Selles teemas kirjeldatakse, kuidas kirjutada on Tekstisisese tabel\-väärtuseks ridade migreerida valimiseks funktsioon.

>   [AZURE.NOTE] Kui annate funktsioon filter nõrk, andmete migreerimise ka teostab halvasti. Venitus andmebaasi kehtib funktsioon filter tabeli CROSS rakendamine tehtemärkide abil.

Kui te ei määra funktsioon filter, migreeritakse kogu tabel.

Venitamine viisardi lubamine andmebaasi käivitamisel saate migreerida terve tabeli või saate määrata funktsiooni Lihtfilter viisardi. Kui soovite kasutada mõnda muud tüüpi funktsioon filter valige read migreerida, tehke ühte järgmistest.

-   Sulgege viisard ja käivitage lause ALTER TABLE tabeli venitamine lubamine ja täpsustada funktsioon filter.

-   Funktsioon filter määramiseks pärast väljumist viisardi lause ALTER TABLE käivitada.

Selles teemas allpool on kirjeldatud ALTER TABLE süntaks lisamise funktsiooni.

## <a name="basic-requirements-for-the-filter-function"></a>Funktsioon filter nõuetele
Tekstisisene tabeli\-väärtusega funktsiooni venitamine andmebaasi filter predikaat näeb välja nagu järgmine näide.

```tsql
CREATE FUNCTION dbo.fn_stretchpredicate(@column1 datatype1, @column2 datatype2 [, ...n])
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN  SELECT 1 AS is_eligible
        WHERE <predicate>
```
Funktsiooni parameetrid peavad olema tabeli veergude identifikaatoreid.

Skeemi sidumine on vajalik vältida veerud, mida kasutatakse lähevad või muudetud funktsioon filter.

### <a name="return-value"></a>Tagastusväärtus
Kui mitte, tagastab funktsioon\-tühi tulemus, rida on õigus migreerida. Muul juhul \- , kui funktsioon ei tagasta tulemuseks \- rida ei migreerita.

### <a name="conditions"></a>Tingimused
Funktsiooni &lt; *predikaat* &gt; võib sisaldada ühe või mitme tingimuse liitunud ja loogika tehtemärkide abil.

```
<predicate> ::= <condition> [ AND <condition> ] [ ...n ]
```
Iga tingimuse omakorda võib sisaldada lihtsad ühe tingimuse või mitme lihtsad tingimuse liitunud loogilise tehtemärgi OR abil.

```
<condition> ::= <primitive_condition> [ OR <primitive_condition> ] [ ...n ]
```

### <a name="primitive-conditions"></a>Lihtsad tingimused
Lihtsad tingimus teha ühte järgmistest võrdlemine.

```
<primitive_condition> ::=
{
<function_parameter> <comparison_operator> constant
| <function_parameter> { IS NULL | IS NOT NULL }
| <function_parameter> IN ( constant [ ,...n ] )
}
```

-   Võrrelge funktsioon parameetri konstandi avaldis. Näiteks `@column1 < 1000`.

    Siin on näide, mis kontrollib, kas väärtus veeru *kuupäev* on &lt; 1/1/2016.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate(@column1 datetime)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 < CONVERT(datetime, '1/1/2016', 101)
    GO

    ALTER TABLE stretch_table_name SET ( REMOTE_DATA_ARCHIVE = ON (
        FILTER_PREDICATE = dbo.fn_stretchpredicate(date),
        MIGRATION_STATE = OUTBOUND
    ) )
    ```

-   Funktsioon parameeter on tühi või pole NULL tehtemärk rakendada.

-   Tehtemärk IN abil saate võrrelda funktsioon parameetrit konstandi väärtuste loendi.

    Siin on näide, mis kontrollib, kas väärtus on *saatmise\_olek* veerg on `IN (N'Completed', N'Returned', N'Cancelled')`.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate(@column1 nvarchar(15))
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 IN (N'Completed', N'Returned', N'Cancelled')
    GO

    ALTER TABLE table1 SET ( REMOTE_DATA_ARCHIVE = ON (
        FILTER_PREDICATE = dbo.fn_stretchpredicate(shipment_status),
        MIGRATION_STATE = OUTBOUND
    ) )
    ```

### <a name="comparison-operators"></a>Võrdluse tehtemärgid
Toetatakse järgmisi tehtemärke võrdlus.

`<, <=, >, >=, =, <>, !=, !<, !>`

```
<comparison_operator> ::= { < | <= | > | >= | = | <> | != | !< | !> }
```

### <a name="constant-expressions"></a>Konstandi avaldised
Funktsioon filter kasutatav konstantide võib olla mis tahes tarkadeks avaldis, mida saab hinnata, kui olete määratlenud funktsiooni. Konstandi avaldiste võib sisaldada järgmisi asju.

-   Literaalide. Näiteks `N’abc’, 123`.

-   Algebravõrrandeid. Näiteks `123 + 456`.

-   Tarkadeks funktsioonid. Näiteks `SQRT(900)`.

-   Kasutage CAST või Teisenda tarkadeks tulemusi. Näiteks `CONVERT(datetime, '1/1/2016', 101)`.

### <a name="other-expressions"></a>Teisi avaldisi
Kui tulemiks oleva funktsiooni vastab reegleid siin kirjeldatud pärast asendate BETWEEN ja vahel pole samaväärsed AND ja OR avaldiste abil saate kasutada BETWEEN ja pole BETWEEN tehtemärke.

Te ei saa kasutada alampäringud või mitte\-näiteks RAND() või GETDATE() tarkadeks funktsioonid.

## <a name="add-a-filter-function-to-a-table"></a>Funktsioon filter lisamine tabelisse
Funktsioon filter lisamine tabelisse töötab lause **ALTER TABLE** ja määrates Tekstisisese olemasolevasse tabelisse\-hinnatud funktsiooni väärtus on **FILTER\_põhiliste** parameeter. Näiteks:

```tsql
ALTER TABLE stretch_table_name SET ( REMOTE_DATA_ARCHIVE = ON (
    FILTER_PREDICATE = dbo.fn_stretchpredicate(column1, column2),
    MIGRATION_STATE = <desired_migration_state>
) )
```
Pärast funktsiooni sidumiseks tabel on predikaat järgmisi asju tingimused on täidetud.

-   Järgmise aja andmete migreerimise esineb ainult ridade puhul, mis tagastab funktsioon mitte\-migreeritakse tühja väärtuse.

-   Kasutatud funktsiooniga veerud on seotud skeemi. Nendes veergudes ei saa muuta, kui tabeli kasutab funktsiooni selle filtri predikaat.

Te ei saa tabeli Tekstisisese\-hinnatud funktsioon kui tabeli kasutab funktsiooni selle filtri predikaat.

>   [AZURE.NOTE] Funktsioon filter, jõudluse parandamiseks registri loomine veergude kasutavad funktsiooni.

### <a name="passing-column-names-to-the-filter-function"></a>Funktsioon filter läbimise veergude nimed
Kui määrate funktsioon filter tabeli, saate määrata veergude nimed edasi ühe osa nimega funktsioon filter. Kui määrate kolmeosaline nimi, kui te kaotate veeru nimed, päringutes vastu venitada\-lubatud tabeli nurjub.

Näiteks kui määrate kolmeosaline veeru nimi, nagu on näidatud järgmises näites, lause töötab edukalt, kuid tabeli päringutes nurjub.

```tsql
ALTER TABLE SensorTelemetry
  SET ( REMOTE_DATA_ARCHIVE = ON (
    FILTER_PREDICATE=dbo.fn_stretchpredicate(dbo.SensorTelemetry.ScanDate),
    MIGRATION_STATE = OUTBOUND )
  )
```

Selle asemel määrata funktsioon filter ühe osa veeru nimi, nagu on näidatud järgmises näites.

```tsql
ALTER TABLE SensorTelemetry
  SET ( REMOTE_DATA_ARCHIVE = ON  (
    FILTER_PREDICATE=dbo.fn_stretchpredicate(ScanDate),
    MIGRATION_STATE = OUTBOUND )
  )
```

## <a name="addafterwiz"></a>Funktsioon filter lisamine pärast viisardi käivitamist  

Kui soovite funktsiooni **Lubamiseks andmebaasi venitada** viisard ei saa luua, saate määrata funktsiooni pärast väljumist viisardi lause ALTER TABLE käivitada. Enne kui saate rakendada funktsiooni, siiski tuleb peatada andmete migreerimise, mis on pooleli ja tuua tagasi migreeritud andmeid. (Lisateabe saamiseks selle kohta, miks see on vajalik, vt [Asenda olemasolev funktsioon filter](#replacePredicate).  

1. Migreerimise suunda tühistada ja tuua tagasi andmed juba viiakse. Seda toimingut ei saa tühistada pärast käivitamist. Saate ka kanda kulusid Azure väljaminev liiklus andmete edastamisel \(sealt\). Lisateavet leiate teemast [Kuidas Azure'i hinnad töötab](https://azure.microsoft.com/pricing/details/data-transfers/).  

    ```tsql  
    ALTER TABLE <table name>  
         SET ( REMOTE_DATA_ARCHIVE ( MIGRATION_STATE = INBOUND ) ) ;   
    ```  

2. Oodake, kuni migreerimise lõpuleviimiseks. Saate oleku **Venitamine andmebaasi kuvari** SQL Server Management Studio või saate teha päringuid **sys.dm_db_rda_migration_status** vaade. Lisateavet leiate teemast [jälgimine ja tõrkeotsing andmete migreerimise](sql-server-stretch-database-monitor.md) või [sys.dm_db_rda_migration_status](https://msdn.microsoft.com/library/dn935017.aspx).  

3. Funktsioon filter, mida soovite rakendada tabeli loomine  

4. Lisage tabelisse funktsiooni ja taaskäivitage andmete migreerimise Azure.  

    ```tsql  
    ALTER TABLE <table name>  
        SET ( REMOTE_DATA_ARCHIVE  
            (           
                FILTER_PREDICATE = <predicate>,  
                MIGRATION_STATE = OUTBOUND  
            )  
        );   
    ```  

## <a name="filter-rows-by-date"></a>Ridade filtreerimine kuupäeva järgi
Järgmises näites migreerib read, kus veeru **kuupäev** sisaldab väärtust, mis varem 1 Jaanuar 2016.

```tsql
-- Filter by date
--
CREATE FUNCTION dbo.fn_stretch_by_date(@date datetime2)
RETURNS TABLE
WITH SCHEMABINDING
AS
       RETURN SELECT 1 AS is_eligible WHERE @date < CONVERT(datetime2, '1/1/2016', 101)
GO
```

## <a name="filter-rows-by-the-value-in-a-status-column"></a>Ridade filtreerimine väärtus veerus Olek
Järgmises näites migreerib read, mille veerus **olek** sisaldab ühte määratud väärtusest.

```tsql
-- Filter by status column
--
CREATE FUNCTION dbo.fn_stretch_by_status(@status nvarchar(128))
RETURNS TABLE
WITH SCHEMABINDING
AS
       RETURN SELECT 1 AS is_eligible WHERE @status IN (N'Completed', N'Returned', N'Cancelled')
GO
```

## <a name="filter-rows-by-using-a-sliding-window"></a>Ridade filtreerimine libistades jälgimisakna abil
Ridade filtreerimiseks libistades akna abil, võtke arvesse järgmisi nõudeid funktsioon filter.

-   Funktsioon peab olema tarkadeks. Seetõttu ei saa luua funktsiooni, mis arvutab automaatselt ümber libistades akna aeg möödub.

-   Funktsioon kasutab skeemi sidumine. Seetõttu ei saa lihtsalt värskendada funktsiooni "asukoht" iga päev **Muuta funktsioon** libistades aknasse.

Alustage filtrifunktsioonist, nagu järgmises näites migreerib read, kus **systemEndTime** veerg sisaldab väärtust, mis varem 1 Jaanuar 2016.

```tsql
CREATE FUNCTION dbo.fn_StretchBySystemEndTime20160101(@systemEndTime datetime2)
RETURNS TABLE
WITH SCHEMABINDING  
AS  
RETURN SELECT 1 AS is_eligible
  WHERE @systemEndTime < CONVERT(datetime2, '2016-01-01T00:00:00', 101) ;
```

Funktsioon filter tabeli rakendada.

```tsql
ALTER TABLE <table name>
SET (
        REMOTE_DATA_ARCHIVE = ON
                (
                        FILTER_PREDICATE = dbo.fn_StretchBySystemEndTime20160101 (SysEndTime)
                                , MIGRATION_STATE = OUTBOUND
                )
        )
;
```

Kui soovite värskendada libistades akna, teha järgmisi toiminguid.

1.  Looge uus funktsioon, mis määrab libistades uues aknas. Järgmises näites valib varasem kui 2 Jaanuar 2016 asemel 1 Jaanuar 2016.

2.  Eelmise funktsioon filter asendada uue helistaja **ALTER TABLE**, nagu on näidatud järgmises näites.

3. Soovi korral võite kukutage eelmise funktsioon filter, mis pole enam kasutate helistaja **KUKUTAGE funktsioon**. (Näiteks ei kuvata seda toimingut.)

```tsql
BEGIN TRAN
GO
        /*(1) Create new predicate function definition */
        CREATE FUNCTION dbo.fn_StretchBySystemEndTime20160102(@systemEndTime datetime2)
        RETURNS TABLE
        WITH SCHEMABINDING
        AS
        RETURN SELECT 1 AS is_eligible
               WHERE @systemEndTime < CONVERT(datetime2,'2016-01-02T00:00:00', 101)
        GO

        /*(2) Set the new function as the filter predicate */
        ALTER TABLE <table name>
        SET
        (
               REMOTE_DATA_ARCHIVE = ON
               (
                       FILTER_PREDICATE = dbo.fn_StretchBySystemEndTime20160102(SysEndTime),
                       MIGRATION_STATE = OUTBOUND
               )
        )
COMMIT ;
```

## <a name="more-examples-of-valid-filter-functions"></a>Veel näiteid sobiv filter funktsioonid

-   Järgmises näites ühendab kaks lihtsad tingimused ja loogika tehtemärkide abil.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate((@column1 datetime, @column2 nvarchar(15))
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
      WHERE @column1 < N'20150101' AND @column2 IN (N'Completed', N'Returned', N'Cancelled')
    GO

    ALTER TABLE table1 SET ( REMOTE_DATA_ARCHIVE = ON (
        FILTER_PREDICATE = dbo.fn_stretchpredicate(date, shipment_status),
        MIGRATION_STATE = OUTBOUND
    ) )
    ```

-   Järgmine näide kasutab mitu tingimust ja tarkadeks teisendamine Teisenda.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate_example1(@column1 datetime, @column2 int, @column3 nvarchar)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
        WHERE @column1 < CONVERT(datetime, '1/1/2015', 101)AND (@column2 < -100 OR @column2 > 100 OR @column2 IS NULL)AND @column3 IN (N'Completed', N'Returned', N'Cancelled')
    GO
    ```

-   Järgmises näites kasutatakse matemaatilisi tehtemärke ja funktsioone.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate_example2(@column1 float)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 < SQRT(400) + 10
    GO
    ```

-   Järgmises näites kasutatakse BETWEEN ja pole BETWEEN tehtemärke. Selle kasutamine on lubatud, kuna tulemiks oleva funktsiooni vastab reegleid siin kirjeldatud pärast asendate BETWEEN ja vahel pole samaväärsed AND ja OR avaldiste abil.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate_example3(@column1 int, @column2 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 BETWEEN 0 AND 100
                AND (@column2 NOT BETWEEN 200 AND 300 OR @column1 = 50)
    GO
    ```
    Eelmise funktsiooni võrdub järgmise funktsiooni pärast asendate võrdväärse AND ja OR avaldiste BETWEEN ja pole BETWEEN tehtemärke.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate_example4(@column1 int, @column2 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 >= 0 AND @column1 <= 100AND (@column2 < 200 OR @column2 > 300 OR @column1 = 50)
    GO
    ```

## <a name="examples-of-filter-functions-that-arent-valid"></a>Filtri funktsioonid, mis ei sobi näited

-   Järgmise funktsiooni pole kehtiv, kuna see sisaldab mitte\-tarkadeks teisendamine.

    ```tsql
    CREATE FUNCTION dbo.fn_example5(@column1 datetime)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 < CONVERT(datetime, '1/1/2016')
    GO
    ```

-   Järgmise funktsiooni pole kehtiv, kuna see sisaldab mitte\-tarkadeks funktsioon kõne.

    ```tsql
    CREATE FUNCTION dbo.fn_example6(@column1 datetime)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 < DATEADD(day, -60, GETDATE())
    GO
    ```

-   Järgmise funktsiooni pole lubatud, kuna see sisaldab Alampäringu.

    ```tsql
    CREATE FUNCTION dbo.fn_example7(@column1 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 IN (SELECT SupplierID FROM Supplier WHERE Status = 'Defunct'))
    GO
    ```

-   Järgmised funktsioonid pole kehtiv Kuna avaldised kasutage algebraline tehtemärke või sisseehitatud\-funktsioonide peavad olema väärtustatavad konstandi kui olete määratlenud funktsiooni. Te ei tohi sisaldada veeruviited algebravõrrandeid või funktsiooni kõned.

    ```tsql
    CREATE FUNCTION dbo.fn_example8(@column1 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE @column1 % 2 =  0
    GO

    CREATE FUNCTION dbo.fn_example9(@column1 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE SQRT(@column1) = 30
    GO
    ```

-   Järgmise funktsiooni pole kehtiv, kuna see rikub siinkirjeldatud pärast tehtemärk BETWEEN asendamine sünonüüm ja reegleid.

    ```tsql
    CREATE FUNCTION dbo.fn_example10(@column1 int, @column2 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE (@column1 BETWEEN 1 AND 200 OR @column1 = 300) AND @column2 > 1000
    GO
    ```
    Eelmise funktsiooni vastab järgmise funktsiooni pärast tehtemärk BETWEEN asendamine ja sünonüüm. See funktsioon pole lubatud, kuna lihtsad tingimused saate kasutada ainult loogilise tehtemärgi OR.

    ```tsql
    CREATE FUNCTION dbo.fn_example11(@column1 int, @column2 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN  SELECT 1 AS is_eligible
            WHERE (@column1 >= 1 AND @column1 <= 200 OR @column1 = 300) AND @column2 > 1000
    GO
    ```

## <a name="how-stretch-database-applies-the-filter-function"></a>Kuidas venitada andmebaasi kehtib funktsioon filter
Venitus andmebaasi kehtib funktsioon filter tabeli ja määrab õigus ridade CROSS rakendamine tehtemärkide abil. Näiteks:

```tsql
SELECT * FROM stretch_table_name CROSS APPLY fn_stretchpredicate(column1, column2)
```
Kui mitte, tagastab funktsioon\-tühi tulemus rea, rida on õigus migreerida.

## <a name="replacePredicate"></a>Asendage olemasolevad funktsioon filter
Saate asendada eelnevalt määratud filtrifunktsioonist opsüsteemi uuesti lause **ALTER TABLE** ja uut väärtust määrates selle **FILTER\_põhiliste** parameeter. Näiteks:

```tsql
ALTER TABLE stretch_table_name SET ( REMOTE_DATA_ARCHIVE = ON (
    FILTER_PREDICATE = dbo.fn_stretchpredicate2(column1, column2),
    MIGRATION_STATE = <desired_migration_state>
```
Uue tabeli Tekstisisese\-väärtusega funktsioon on järgmistele nõuetele.

-   Uue funktsiooni peab olema paindlikumad eelmise funktsiooni.

-   Uue funktsiooni kuuluma tehtemärgid, et vana funktsiooni.

-   Uue funktsiooni ei tohi sisaldada tehtemärgid, mida pole olemas vana funktsiooni.

-   Tehtemärk argumentide järjestus ei saa muuta.

-   Ainult konstandi väärtused, mis on osa on `<, <=, >, >=` võrdlus saab muuta nii, et funktsioon vähem piiravad.

### <a name="example-of-a-valid-replacement"></a>Näide: lubatud asendamine
Eeldavad, et järgmise funktsiooni on praeguse filtri predikaat.

```tsql
CREATE FUNCTION dbo.fn_stretchpredicate_old (@column1 datetime, @column2 int)
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN  SELECT 1 AS is_eligible
        WHERE @column1 < CONVERT(datetime, '1/1/2016', 101)
            AND (@column2 < -100 OR @column2 > 100)
GO
```
Järgmise funktsiooni on lubatud asendamine, kuna (mis määrab hiljem äralõikekuupäeva) konstandi uue kuupäeva teeb funktsiooni vähem piiravad.

```tsql
CREATE FUNCTION dbo.fn_stretchpredicate_new (@column1 datetime, @column2 int)
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN  SELECT 1 AS is_eligible
        WHERE @column1 < CONVERT(datetime, '2/1/2016', 101)
            AND (@column2 < -50 OR @column2 > 50)
GO
```

### <a name="examples-of-replacements-that-arent-valid"></a>Näiteid asendused, mis pole lubatud
Järgmise funktsiooni pole kehtiv lugemisprogrammi, kuna uuele kuupäevale konstandi (mis määrab varem äralõikekuupäeva) ei tee funktsiooni vähem piiravad.

```tsql
CREATE FUNCTION dbo.fn_notvalidreplacement_1 (@column1 datetime, @column2 int)
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN  SELECT 1 AS is_eligible
        WHERE @column1 < CONVERT(datetime, '1/1/2015', 101)
            AND (@column2 < -100 OR @column2 > 100)
GO
```
Järgmise funktsiooni pole kehtiv lugemisprogrammi, kuna üks võrdluse tehtemärgid on eemaldatud.

```tsql
CREATE FUNCTION dbo.fn_notvalidreplacement_2 (@column1 datetime, @column2 int)
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN  SELECT 1 AS is_eligible
        WHERE @column1 < CONVERT(datetime, '1/1/2016', 101)
            AND (@column2 < -50)
GO
```
Järgmise funktsiooni pole kehtiv lugemisprogrammi, kuna uus tingimus on lisatud ja loogika tehtemärkide abil.

```tsql
CREATE FUNCTION dbo.fn_notvalidreplacement_3 (@column1 datetime, @column2 int)
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN  SELECT 1 AS is_eligible
        WHERE @column1 < CONVERT(datetime, '1/1/2016', 101)
            AND (@column2 < -100 OR @column2 > 100)
            AND (@column2 <> 0)
GO
```

## <a name="remove-a-filter-function-from-a-table"></a>Funktsioon filter tabeli eemaldamine
Valitud ridade asemel terve tabeli migreerimiseks eemaldada olemasoleva funktsiooni seadmisega **FILTER\_põhiliste** null. Näiteks:

```tsql
ALTER TABLE stretch_table_name SET ( REMOTE_DATA_ARCHIVE = ON (
    FILTER_PREDICATE = NULL,
    MIGRATION_STATE = <desired_migration_state>
) )
```
Pärast funktsiooni filtri eemaldamiseks tabeli kõigi ridade laiene migreerimise jaoks. Seetõttu ei saa määrata filtri funktsioon sama tabeli hiljem juhul, kui saate tuua tagasi remote andmed tabeli Azure'i esmalt. See piirang on olemas vältida olukorda, kus read, mis ei laiene migreerimise kui esitate uus funktsioon filter on juba migreeritud Azure.

## <a name="check-the-filter-function-applied-to-a-table"></a>Funktsioon filter rakendatud tabeli kontrollimine
Funktsioon tabelile rakendatud filter vaatamiseks avage Kuva kataloogi **sys.remote\_andmete\_arhiivi\_tabelite** ja märkige ruut väärtus on **filter\_predikaat** veerg. Kui väärtus on null, on õigus saada arhiivimine kogu tabel. Lisateavet leiate teemast [sys.remote_data_archive_tables (Transact-SQL-i)](https://msdn.microsoft.com/library/dn935003.aspx).

## <a name="security-notes-for-filter-functions"></a>Filtrifunktsioonid turvalisusest  
Rikutud konto db_owner privileegidega saate teha järgmisi toiminguid.  

-   Loomine ja rakendamine tabelväärtusega funktsiooni, mis tarbib suurel hulgal serveri ressursid või ootab pikema perioodi tulemuseks on teenuse keelamise.  

-   Loomine ja rakendamine tabelväärtusega funktsiooni, mis võimaldab tuletamine, mille kasutaja on selgesõnaliselt keelatud lugemisõigus tabeli sisu.  

## <a name="see-also"></a>Vt ka

[Muuda tabeli (Transact-SQL)](https://msdn.microsoft.com/library/ms190273.aspx)
