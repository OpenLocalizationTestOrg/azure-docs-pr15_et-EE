<properties
   pageTitle="Azure'i Lake andmeanalüüsi abil veebisaidi logid analüüsida | Azure'i"
   description="Saate teada, kuidas analüüsida veebisaidi logid Lake andmeanalüüsi abil. "
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

# <a name="tutorial-analyze-website-logs-using-azure-data-lake-analytics"></a>Õpetus: Analüüsida veebisaidi logid Azure'i Lake andmeanalüüsi abil

Saate teada, kuidas analüüsida veebisaidi logid kasutamine andmete Lake Analytics, eriti teada saada, mis soovitajad ilmnes tõrkeid, kui nad proovinud veebisaidilt.

>[AZURE.NOTE] Kui soovite lihtsalt näha rakendus töötab, see salvestab [kasutamine Azure andmeanalüüsi interaktiivsed juhendid](data-lake-analytics-use-interactive-tutorials.md)läbi aja. Selle õpetuse põhineb sama stsenaariumi ja sama koodi. Selle õpetuse eesmärk on anda arendajate kogemus loomine ja käitamine Lake andmeanalüüsi rakenduse kaudu lõpuni.

## <a name="prerequisites"></a>Eeltingimused

- **Visual Studio 2015, Visual Studio 2013 update 4, või Visual Studio 2012 ja Visual C++ installitud**.
- **Microsoft Azure'i SDK .net-i versiooni 2.5 või kohale**.  Installige see [Web platvormi Installeri](http://www.microsoft.com/web/downloads/platform.aspx)abil.
- **[Andmete Lake tööriistade Visual Studio](http://aka.ms/adltoolsvs)**.

    Kui andmete Lake Tools for Visual Studio on installitud, kuvatakse menüü **Andmed Lake** Visual Studios.

    ![U-SQL Visual Studio menüü](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-menu.png)

- **Põhiteadmisi Lake andmeanalüüsi ja Visual Studio Lake Andmeriistad**. Alustamiseks järgmistest teemadest.

    - [Azure'i Lake andmeanalüüsi Azure'i portaalis alustamine](data-lake-analytics-get-started-portal.md).
    - [Arendamise U-SQL-skripti abil andmete Lake Visual Studio tööriistad](data-lake-analytics-data-lake-tools-get-started.md).

- **Andmeanalüüsi Lake konto.**  Teemast [Azure andmeanalüüsi Lake konto loomine](data-lake-analytics-get-started-portal.md#create_adl_analytics_account).

    Andmete Lake tööriistad ei toeta loomise andmete Lake Analyticsi kontod.  Seega peate looma Azure portaali, Azure PowerShelli, .NET SDK või Azure CLI abil.
- **Laadige Näidisandmete Lake andmeanalüüsi konto.** Lugege teemat [Lake andmesalv vaikekonto SearchLog.tsv üles laadida](data-lake-analytics-get-started-portal.md#update-data-to-the-default-adl-storage-account).

    Andmeanalüüsi Lake töö käivitamiseks peate mõned andmed. Ehkki Lake Andmetööriistad toetab üleslaadimisel andmeid, kasutage portaali üles laadida Näidisandmete selles õpetuses hõlbustamiseks jälgimiseks.

## <a name="connect-to-azure"></a>Azure'i ühendamine

Enne kui saate koostada ja mis tahes U-SQL-i skriptide testimiseks, peate esmalt ühenduse Azure'i.

**Ühenduse loomiseks Lake andmeanalüüsi**

1. Avage Visual Studio.
2. Klõpsake menüü **Andmed Lake** **suvandite ja sätete**.
4. Klõpsake nuppu **Logi sisse**või **Muuda kasutaja** , kui keegi on sisse logitud ja järgige juhiseid.
5. Klõpsake nuppu **OK** , et sulgeda dialoogiboks suvandite ja sätete.

**Sirvida oma andmete Lake Analyticsi kontod**

1. Visual Studio, **Serveri Exploreri** avamiseks vajutage **Klahvikombinatsiooni CTRL + ALT + S**.
2. **Server Explorer**, laiendage **Azure**ja seejärel laiendage **Lake andmeanalüüsi**. Oma andmete Lake Analyticsi kontod loendi peab näete, kui on olemas. Ei saa luua andmete Lake Analyticsi kontod Studio. Konto loomiseks vaadake teemat [Alustamine Azure'i Lake andmeanalüüsi Azure portaali kaudu](data-lake-analytics-get-started-portal.md) või [Alustamine Azure'i Lake andmeanalüüsi Azure PowerShelli kaudu](data-lake-analytics-get-started-powershell.md).

## <a name="develop-u-sql-application"></a>Tekib U-SQL-i rakendus

A-SQL-i rakendus on enamasti U-SQL-i skripti. A-SQL-i kohta leiate lisateavet teemast [Alustamine U-SQL-i](data-lake-analytics-u-sql-get-started.md).

Saate lisada rakenduse lisamine kasutaja määratletud tehtemärke.  Lisateabe saamiseks vt [arendamise U-SQL-i kasutaja määratletud Lake andmeanalüüsi tööde tehtemärke](data-lake-analytics-u-sql-develop-user-defined-operators.md).

**Luua ja esitada Lake andmeanalüüsi töö**

1. Klõpsake menüü **fail** nuppu **Uus**ja seejärel klõpsake nuppu **projekti**.
2. Valige U-SQL-i projekti tüüp.

    ![uue U-SQL-i Visual Studio projekti](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-new-project.png)

3. Klõpsake nuppu **OK**. Visual studio loob lahenduse Script.usql faili abil.
4. Sisestage järgmine skript Script.usql faili.

        // Create a database for easy reuse, so you don't need to read from a file every time.
        CREATE DATABASE IF NOT EXISTS SampleDBTutorials;

        // Create a Table valued function. TVF ensures that your jobs fetch data from the weblog file with the correct schema.
        DROP FUNCTION IF EXISTS SampleDBTutorials.dbo.WeblogsView;
        CREATE FUNCTION SampleDBTutorials.dbo.WeblogsView()
        RETURNS @result TABLE
        (
            s_date DateTime,
            s_time string,
            s_sitename string,
            cs_method string,
            cs_uristem string,
            cs_uriquery string,
            s_port int,
            cs_username string,
            c_ip string,
            cs_useragent string,
            cs_cookie string,
            cs_referer string,
            cs_host string,
            sc_status int,
            sc_substatus int,
            sc_win32status int,
            sc_bytes int,
            cs_bytes int,
            s_timetaken int
        )
        AS
        BEGIN

            @result = EXTRACT
                s_date DateTime,
                s_time string,
                s_sitename string,
                cs_method string,
                cs_uristem string,
                cs_uriquery string,
                s_port int,
                cs_username string,
                c_ip string,
                cs_useragent string,
                cs_cookie string,
                cs_referer string,
                cs_host string,
                sc_status int,
                sc_substatus int,
                sc_win32status int,
                sc_bytes int,
                cs_bytes int,
                s_timetaken int
            FROM @"/Samples/Data/WebLog.log"
            USING Extractors.Text(delimiter:' ');
            RETURN;
        END;

        // Create a table for storing referrers and status
        DROP TABLE IF EXISTS SampleDBTutorials.dbo.ReferrersPerDay;
        @weblog = SampleDBTutorials.dbo.WeblogsView();
        CREATE TABLE SampleDBTutorials.dbo.ReferrersPerDay
        (
            INDEX idx1
            CLUSTERED(Year ASC)
            PARTITIONED BY HASH(Year)
        ) AS

        SELECT s_date.Year AS Year,
            s_date.Month AS Month,
            s_date.Day AS Day,
            cs_referer,
            sc_status,
            COUNT(DISTINCT c_ip) AS cnt
        FROM @weblog
        GROUP BY s_date,
                cs_referer,
                sc_status;

    U-SQL vt [andmete Lake Analytics U-SQL-i keele kasutamise alustamine](data-lake-analytics-u-sql-get-started.md).    

5. Uue U-SQL-i skripti lisada projekti ja sisestage järgmine:

        // Query the referrers that ran into errors
        @content =
            SELECT *
            FROM SampleDBTutorials.dbo.ReferrersPerDay
            WHERE sc_status >=400 AND sc_status < 500;

        OUTPUT @content
        TO @"/Samples/Outputs/UnsuccessfulResponses.log"
        USING Outputters.Tsv();

6. Esimese U-SQL-skript ja kõrval nupp **Edasta** aktiveerimine, määrata Analytics kontole.
7. **Lahenduste Explorer**, paremklõpsake **Script.usql**ja klõpsake **Skripti koostamine**. Veenduge paanil väljundi tulemused.
8. **Lahenduste Explorer**, paremklõpsake **Script.usql**ja klõpsake **Skripti esitada**.
9. Veenduge, et **Analüüsi konto** on see, kuhu soovite töö ja klõpsake nuppu **Edasta**. Tulemuste esitamise ja töö link on saadaval Visual Studios annab akna Lake Andmetööriistad kui esitamise on lõpule viidud.
10. Oodake, kuni töö on lõpule viidud.  Kui töö nurjus, pole see tõenäoliselt lähtefail.  Lugege selle õpetuse nõutav osa. Tõrkeotsingu kohta lisateabe saamiseks lugege teemat [jälgimine ja Azure andmeanalüüsi Lake töö tõrkeotsing](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md).

    Kui töö on valmis, peab kuvatakse järgmine Kuva:

    ![andmeanalüüsi lake analüüsida ajaveebide veebisaidi logid](./media/data-lake-analytics-analyze-weblogs/data-lake-analytics-analyze-weblogs-job-completed.png)

11. Nüüd korrake juhiseid 7 - 10 **Script1.usql**.

>[AZURE.NOTE]Ei saa lugeda või kirjutada U-SQL-i tabeli, mis on loodud või muudetud sama skripti.  Mis on põhjus, miks kasutada kasutage kahe selle näite puhul.

**Töö väljundi kuvamiseks**

1. **Server Explorer**, laiendamine **Azure**, laiendamine **Lake andmeanalüüsi**, laiendamine Lake andmeanalüüsi kontole, laiendage **Salvestusruumi kontod**, paremklõpsake Lake andmesalv vaikekonto, ja klõpsake **Explorer**.
2.  **Näidised** kausta avamiseks topeltklõpsake ja seejärel topeltklõpsake **väljundeid**.
3.  Topeltklõpsake **UnsuccessfulResponsees.log**.
4.  Võite ka topeltklõpsata väljundfail sees Graphi vaate töö selleks, et liikuda väljund.

## <a name="see-also"></a>Vt ka

Alustamine Lake andmeanalüüsi erineva tööriista abil järgmistest teemadest.

- [Azure'i portaalis Lake andmeanalüüsi kasutamise alustamine](data-lake-analytics-get-started-portal.md)
- [Alustamine Lake andmeanalüüsi Azure PowerShelli abil](data-lake-analytics-get-started-powershell.md)
- [Kasutades .NET SDK Lake andmeanalüüsi kasutamise alustamine](data-lake-analytics-get-started-net-sdk.md)

Lisateavet arengu teemade vaatamiseks tehke järgmist.

- [Arendamise U-SQL-i skriptide abil andmete Lake tööriistad Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
- [Azure'i andmed Lake Analytics U-SQL-i keele kasutamise alustamine](data-lake-analytics-u-sql-get-started.md)
- [Arendamise U-SQL-i kasutaja määratletud tehtemärgid Lake andmeanalüüsi projektide jaoks](data-lake-analytics-u-sql-develop-user-defined-operators.md)
