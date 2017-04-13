<properties
   pageTitle="Arendamise U-SQL-i skriptide abil andmete Lake tööriistad Visual Studio | Azure'i"
   description="Saate teada, kuidas installida Visual Studios, kuidas luua ja testi U-SQL-i skriptide Lake Andmeriistad. "
   services="data-lake-analytics"
   documentationCenter=""
   authors="edmacauley"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="05/16/2016"
   ms.author="edmaca"/>

# <a name="tutorial-get-started-with-azure-data-lake-analytics-u-sql-language"></a>Õpetus: Alustamine Azure andmete Lake Analytics U-SQL-i keel

A-SQL-is on keel, mis ühendab SQL-i eelised oma kood väljendusvõimalustega õigus kõik andmeid mis tahes skaala. U SQL scalable jaotatud päringu võimalus võimaldab tõhus analüüside poes ja relatsiooniline salvestab, nt Azure'i SQL-andmebaasi üle.  See võimaldab protsessi struktureerimata andmed järgi skeemi lugeda, lisada kohandatud loogika ja UDF's ja sisaldab laiendatavuse peene tekstuuriga juhtida, kuidas täita tasandil. Kujundus filosoofia U-SQL-i kohta lisateabe saamiseks vaadake selle [Visual Studio ajaveebi postitamine](https://blogs.msdn.microsoft.com/visualstudio/2015/09/28/introducing-u-sql-a-language-that-makes-big-data-processing-easy/).

Mõned erinevused ANSI SQL-i või T-SQL-is on. Näiteks oma märksõnad, nagu valige peavad olema suurtähtedega.

See on tippige süsteem ja avaldis keele sees select-klauslid, kus C# on predicates jne.
See tähendab, et andmetüübid C# tüübid ja andmetüüpide C# NULL semantika ning järgige võrdlus toimingute sees predikaat C# süntaks (nt lisamine == "foo").  See tähendab ka, et väärtused on täielik .NET objekte, mis võimaldab teil hõlpsasti kasutada mis tahes meetodit sõitmiseks objekti (nt "f o o o". Split(' ')).

Lisateavet leiate teemast [U-SQL-i juhend](http://go.microsoft.com/fwlink/p/?LinkId=691348).

###<a name="prerequisites"></a>Eeltingimused

Peate teostama [õpetus: arendamise U-SQL-i skriptide abil andmete Lake tööriistad Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).

Selle õpetuse käivitasite Lake andmeanalüüsi töö järgmise U-SQL skripti:

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    OUTPUT @searchlog   
        TO "/output/SearchLog-first-u-sql.csv"
    USING Outputters.Csv();

See skript ei sisalda mis tahes teisendus juhiseid. Nimega **SearchLog.tsv**lähtefail loeb, schematizes selle ja väljundid on Reahulk uuesti sisse fail nimega **SearchLog ees-u sql.csv**.

Pange tähele küsimärki kõrval kestuse välja andmetüübist. Mida tähendab, et välja kestus võib olla tühi.

Mõned mõisted ja märksõnad leitud skripti:

- **Reahulk muutujate**: iga päringu avaldis, mis annab tulemiks on Reahulk saab määrata muutujana. U-SQL järgib T-SQL-i muutuvate nimede muster, näiteks **@searchlog** skripti.
    Pange tähele, et ülesande ei jõustada täitmise. See üksnes avaldise nimed ja võimaldab teil luua veel keerulistes avaldistes.
- **ERALDAMISEKS** võimaldab määratleda skeemi lugemine. Skeemi on määratud veeru nimi ja C# tüüp nimi paari veeru kohta. Kasutab on nn **Extractor**, näiteks **Extractors.Tsv()** eraldamiseks tsv failid. Saate arendada kohandatud väljatõmbajad.
- **Väljundi** võtab on Reahulk ja selle serializes. Funktsiooni Outputters.Csv() väljund CSV-vormingus faili määratud asukohta. Samuti saab luua kohandatud Outputters.
- Pange tähele, kaks teed on suhteline teed. Samuti saate absoluutne teed.  Näide

        adl://<ADLStorageAccountName>.azuredatalakestore.net:443/Samples/Data/SearchLog.tsv

    Lingitud salvestusruumi kontod failid juurdepääsuks peate kasutama absoluutne tee.  Lingitud Azure Storage konto talletatud failide süntaks on järgmine:

        wasb://<BlobContainerName>@<StorageAccountName>.blob.core.windows.net/Samples/Data/SearchLog.tsv

    >[AZURE.NOTE] Azure'i bloobimälu container avaliku plekid või avaliku ümbriste juurdepääsuõigused pole praegu toetatud.

## <a name="use-scalar-variables"></a>Skalaarse muutujate kasutamine

Skalaarse muutujat saate kasutada ka oma skripti haldamise hõlbustamiseks. Eelmise U-SQL-i skripti võib ka kirjutada järgmist:

    DECLARE @in  string = "/Samples/Data/SearchLog.tsv";
    DECLARE @out string = "/output/SearchLog-scalar-variables.csv";

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM @in
        USING Extractors.Tsv();

    OUTPUT @searchlog   
        TO @out
        USING Outputters.Csv();

## <a name="transform-rowsets"></a>Muuta rowsets

**Valige** abil saate muuta rowsets.

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    @rs1 =
        SELECT Start, Region, Duration
        FROM @searchlog
    WHERE Region == "en-gb";

    OUTPUT @rs1   
        TO "/output/SearchLog-transform-rowsets.csv"
        USING Outputters.Csv();

WHERE-klausli kasutab [C# loogikaavaldis](https://msdn.microsoft.com/library/6a71f45d.aspx). Saate teha oma avaldiste ja funktsioonide C# avaldis keel. Saate teha ka keerukamaid filtreerimine, kombineerides neid loogilise sidesõnade (ANDs) ja disjunctions (ORs).

Järgmine skript kasutab DateTime.Parse() meetodit ja lisamine koos.

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    @rs1 =
        SELECT Start, Region, Duration
        FROM @searchlog
    WHERE Region == "en-gb";

    @rs1 =
        SELECT Start, Region, Duration
        FROM @rs1
        WHERE Start >= DateTime.Parse("2012/02/16") AND Start <= DateTime.Parse("2012/02/17");

    OUTPUT @rs1   
        TO "/output/SearchLog-transform-datatime.csv"
        USING Outputters.Csv();

Märkate, et teine päring töötab esimest Reahulk tulem ja seega tulemus on kaks filtrid. Saate uuesti kasutada muutuja nimi ja nimed on rakendatud haito keel.

## <a name="aggregate-rowsets"></a>Aggregate rowsets

U-SQL pakub teile tuttavad **ORDER BY**, **GROUP BY** ja liitmised.

Järgmine päring kestab kokku müüginäitajaid piirkonna kohta leiab, ja seejärel väljundi kohal 5 kestused järjestuses.

U-SQL rowsets Säilita oma tellimuse järgmise päringu. Seega järjestamiseks väljund, peate lisama JÄRJESTUSALUS väljundi lause, nagu allpool näidatud:

    DECLARE @outpref string = "/output/Searchlog-aggregation";
    DECLARE @out1    string = @outpref+"_agg.csv";
    DECLARE @out2    string = @outpref+"_top5agg.csv";

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    @rs1 =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM @searchlog
    GROUP BY Region;

    @res =
    SELECT *
    FROM @rs1
    ORDER BY TotalDuration DESC
    FETCH 5 ROWS;

    OUTPUT @rs1
        TO @out1
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();
    OUTPUT @res
        TO @out2
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

U-SQL JÄRJESTUSALUS klausel on kombineerida toomise klauslis SELECT avaldises.

Klausel U-SQL-is on saab piirata väljund rühmad, mis vastavad tingimusele HAVING:

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
        USING Extractors.Tsv();

    @res =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM @searchlog
    GROUP BY Region
    HAVING SUM(Duration) > 200;

    OUTPUT @res
        TO "/output/Searchlog-having.csv"
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

## <a name="join-data"></a>Andmete liitumine

U-SQL-is toodud levinud Liitu tehtemärgid, näiteks sisemine ühendus, vasakule/paremale/täielik väline ühendus, POOLELDI JOIN, liitumine tabelid, kuid mis tahes rowsets (ka need, mis on saadud failid).

Järgmine skript ühendab searchlog mõne reklaami mulje Logi ja annab meile päringu stringi reklaamide antud kuupäeval.

    @adlog =
        EXTRACT UserId int,
                Ad string,
                Clicked int
        FROM "/Samples/Data/AdsLog.tsv"
        USING Extractors.Tsv();

    @join =
        SELECT a.Ad, s.Query, s.Start AS Date
        FROM @adlog AS a JOIN <insert your DB name>.dbo.SearchLog1 AS s
                        ON a.UserId == s.UserId
        WHERE a.Clicked == 1;

    OUTPUT @join   
        TO "/output/Searchlog-join.csv"
        USING Outputters.Csv();


U-SQL toetab ainult ANSI nõuetele vastavuse ühenduse süntaksi: Rowset1 Liitu Rowset2 sees predikaat. Vana süntaks: Rowset1, kus Rowset2 predikaat ei toetata.
Predikaat ühendamise peab olema on võrdne Liitu ja pole avaldis. Kui soovite avaldise, lisage see eelmise Reahulk select-klauslis. Kui soovite teha erinevate võrdluse, saate selle viimine WHERE-klausli.


## <a name="create-databases-table-valued-functions-views-and-tables"></a>Andmebaaside, tabelväljundiga funktsioonide, vaadete ja tabelite loomine

U-SQL võimaldab kasutada andmete andmebaasi ja -skeemifailid kontekstis. Seega ei pea alati lugeda või kirjutada faile.

Iga U-SQL-i skripti töötab vaikimisi andmebaasi (juhtslaidi) ja vaikimisi skeemi (DBO) vaikimisi konteksti. Saate luua oma andmebaasi ja/või skeemi. Konteksti muutmiseks kasutage **kasutamine** lause konteksti muutmiseks.


### <a name="create-a-table-valued-function-tvf"></a>Looge tabelväärtusega funktsiooni (TVF)

Eelmise U-SQL-i skripti, saate korrata, kasutades sama lähtefaili lugemise ekstrakti. U-SQL tabelväärtusega funktsiooni abil saate Kapselda andmed edaspidiseks kasutamiseks.   

Järgmine skript loob TVF, mis on vaikimisi andmebaasi ja -skeemifailid *Searchlog()* nimega:

    DROP FUNCTION IF EXISTS Searchlog;

    CREATE FUNCTION Searchlog()
    RETURNS @searchlog TABLE
    (
                UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
    )
    AS BEGIN
    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
    USING Extractors.Tsv();
    RETURN;
    END;

Järgmise skripti näitab, kuidas kasutada TVF, mis on määratletud eelmise skripti:

    @res =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM Searchlog() AS S
    GROUP BY Region
    HAVING SUM(Duration) > 200;

    OUTPUT @res
        TO "/output/SerachLog-use-tvf.csv"
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

### <a name="create-views"></a>Vaadete loomine

Kui teil on ainult üks päringu avaldis, mille soovite abstraktne ja soovite selle parameterize, saate luua vaate asemel tabelväärtusega funktsiooni.

Järgmine skript loob vaate, nimega *SearchlogView* vaikimisi andmebaasi ja -skeemifailid:

    DROP VIEW IF EXISTS SearchlogView;

    CREATE VIEW SearchlogView AS  
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM "/Samples/Data/SearchLog.tsv"
    USING Extractors.Tsv();

Järgmise skripti näitab määratletud vaate kasutamine:

    @res =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM SearchlogView
    GROUP BY Region
    HAVING SUM(Duration) > 200;

    OUTPUT @res
        TO "/output/Searchlog-use-view.csv"
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

### <a name="create-tables"></a>Tabelite loomine

Sarnane relatsiooniandmebaasist tabeli A-SQL-i võimaldab teil Looge tabel, milles eelmääratletud skeemi või tabeli loomine ja tuletada skeemi päring, mis kuvab tabeli (tuntud ka kui loomine tabeli AS valimine või CTAS) põhjal.

Järgmise skripti andmebaasi ja kahe tabeli loomiseks tehke järgmist.

    DROP DATABASE IF EXISTS SearchLogDb;
    CREATE DATABASE SeachLogDb
    USE DATABASE SearchLogDb;

    DROP TABLE IF EXISTS SearchLog1;
    DROP TABLE IF EXISTS SearchLog2;

    CREATE TABLE SearchLog1 (
                UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string,

                INDEX sl_idx CLUSTERED (UserId ASC)
                    PARTITIONED BY HASH (UserId)
    );

    INSERT INTO SearchLog1 SELECT * FROM master.dbo.Searchlog() AS s;

    CREATE TABLE SearchLog2(
        INDEX sl_idx CLUSTERED (UserId ASC)
                PARTITIONED BY HASH (UserId)
    ) AS SELECT * FROM master.dbo.Searchlog() AS S; // You can use EXTRACT or SELECT here


### <a name="query-tables"></a>Päringu tabelite

Saate teha päringuid (loodud eelmise skripti) tabelid samamoodi nagu saate päringu üle andmefailid. Loomise asemel mõne Reahulk abil ekstrakti, nüüd lihtsalt viidata saate tabeli nimi.

Varem kasutatud transform script on muudetud tabelite lugeda.

    @rs1 =
        SELECT
            Region,
            SUM(Duration) AS TotalDuration
        FROM SearchLogDb.dbo.SearchLog2
    GROUP BY Region;

    @res =
        SELECT *
        FROM @rs1
        ORDER BY TotalDuration DESC
        FETCH 5 ROWS;

    OUTPUT @res
        TO "/output/Searchlog-query-table.csv"
        ORDER BY TotalDuration DESC
        USING Outputters.Csv();

Pange tähele, et praegu ei saa käivitada valik sama skript skripti tabeli kus saate luua tabeli.


##<a name="conclusion"></a>Kokkuvõte

Mida hõlmab õpetuses on üksnes väike osa U-SQL-i. Selles õpetuses ulatuse, kuna see ei kuulu kõike, järgmised:

- Kasuta CROSS rakendada lahti ridadesse stringid, massiivid ja kaardid.
- Töövool sektsioonitud andmekogumite (faili komplekti ja piiretega tabelid).
- Kasutaja määratletud tehtemärkide tõmburid, outputters, protsessorite ja kasutaja määratletud lugeja C# nt töötada.
- U-SQL akendesüsteemide funktsioonidega.
- Halda vaateid, tabelväljundiga funktsioonide ja salvestatud toimingute U-SQL-koodi.
- Käivitage oma töötlemine sõlmed suvalise kohandatud koodi.
- Ühenduse Azure SQL-i andmebaasid ja federate päringute neile ja U-SQL-i ja Azure andmete Lake andmete üle.

## <a name="see-also"></a>Vt ka

- [Microsoft Azure'i andmed Lake Analytics ülevaade](data-lake-analytics-overview.md)
- [Arendamise U-SQL-i skriptide abil andmete Lake tööriistad Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
- [Azure'i andmeanalüüsi Lake tööde U-SQL-i akna funktsioonide abil](data-lake-analytics-use-window-functions.md)
- [Jälgimine ja Azure andmeanalüüsi Lake töö Azure'i portaalis tõrkeotsing](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)

## <a name="let-us-know-what-you-think"></a>Andke meile teada, mida te arvate

- [Funktsioon taotluse esitamine](http://aka.ms/adlafeedback)
- [Abi saate tugiteenuste Foorumid](http://aka.ms/adlaforums)
- [Tagasiside andmine: klõpsake U-SQL-i](http://aka.ms/usqldiscuss)
