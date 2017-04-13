<properties 
    pageTitle="SQL-i andmebaasi SSMS haldamine | Microsoft Azure'i" 
    description="Saate teada, kuidas kasutada SQL Server Management Studio SQL-andmebaasi serverid ja andmebaaside haldamine." 
    services="sql-database" 
    documentationCenter=".net" 
    authors="stevestein" 
    manager="jhubbard" 
    editor="tysonn"/>

<tags 
    ms.service="sql-database" 
    ms.workload="data-management" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/29/2016" 
    ms.author="sstein"/>


# <a name="managing-azure-sql-database-using-sql-server-management-studio"></a>Azure'i SQL-andmebaasi SQL Server Management Studio abil haldamine 


> [AZURE.SELECTOR]
- [Azure'i portaal](sql-database-manage-portal.md)
- [SSMS](sql-database-manage-azure-ssms.md)
- [PowerShelli](sql-database-manage-powershell.md)

SQL Server Management Studio (SSMS) abil saate Azure'i SQL-andmebaasi serverid ja andmebaaside haldamine. Selles teemas juhendab teid koos SSMS tavalised toimingud. Juba peaks teil olema server ja andmebaas loodud Azure'i SQL-andmebaasi enne alustamist. Lisateabe saamiseks vaadake [esimese Azure SQL-i andmebaasi loomine](sql-database-get-started.md) ja [ühendamine ja päringu SSMS abil](sql-database-connect-query-ssms.md) .

Soovitatav on kasutada SSMS uusim versioon iga kord, kui töötate Azure'i SQL-andmebaasi. 

> [AZURE.IMPORTANT] Kasuta alati SSMS uusim versioon, kuna pidevalt on parem töötamine Azure ja SQL-andmebaasi uusimad värskendused. Uusima versiooni kohta leiate teavet [SQL Server Management Studio alla laadida](https://msdn.microsoft.com/library/mt238290.aspx).



## <a name="create-and-manage-azure-sql-databases"></a>Saate luua ja hallata Azure SQL-i andmebaasid

Kui loote ühenduse **juhtslaidi** andmebaasi, andmebaaside loomine serveris ja muuta või langev olemasolevaid andmebaase. Järgnevalt kirjeldatakse, kuidas täita mitme levinud andmebaasi haldamise toiminguid Management Studio kaudu. Nende toimingute tegemiseks veenduge, et olete loonud ühenduse serveritaseme põhisumma login, kui häälestate oma serveri loodud **juhtslaidi** andmebaas.

Management Studio päringu akna avamiseks avage kaust, andmebaaside, laiendage kausta **Süsteemi andmebaasid** , paremklõpsake **juhtslaidi**ja seejärel klõpsake nuppu **Uus päring**.

-   **Andmebaasi loomine** lause abil saate luua andmebaasi. Lisateavet leiate teemast [Luua andmebaasi (SQL-i andmebaasi)](https://msdn.microsoft.com/library/dn268335.aspx). Järgmine lause loob **myTestDB** andmebaas ja määrab, et see on standardne S0 väljaande andmebaasi 250 GB maksimaalne vaikesuurust.

        CREATE DATABASE myTestDB
        (EDITION='Standard',
         SERVICE_OBJECTIVE='S0');

Klõpsake päringu käivitamiseks **käivitamine** .

-   Lause **ALTER andmebaasi** abil saate muuta olemasoleva andmebaasiga, näiteks, kui soovite muuta nimi ja andmebaasi väljaannet. Lisateavet leiate teemast [Muuda andmebaasi (SQL-andmebaasi)](https://msdn.microsoft.com/library/ms174269.aspx). Järgmine lause muudab muutmiseks eelmises juhises loodud andmebaasi Standard S1 edition.

        ALTER DATABASE myTestDB
        MODIFY
        (SERVICE_OBJECTIVE='S1');

-   Lause **DROP andmebaasi** abil saate kustutada olemasoleva andmebaasiga. Lisateavet leiate teemast [KATKESTA andmebaas (SQL-andmebaasi)](https://msdn.microsoft.com/library/ms178613.aspx). Järgmine lause kustutab **myTestDB** andmebaasi, kuid ei kukutage see kohe, kas kasutate luua sisselogimise järgmise juhise juurde.

        DROP DATABASE myTestBase;

-   Juhtslaidi andmebaasis on **sys.databases** vaade, mille abil saate kuvada kõik andmebaasid üksikasjad. Saate vaadata kõiki olemasolevaid andmebaase, käivitada järgmine tekst:

        SELECT * FROM sys.databases;

-   SQL-andmebaasis, ei toetata **kasutamine** lause andmebaaside vahetamise. Selle asemel, tuleb teil luua ühendus otse Sihtandmebaas.

>[AZURE.NOTE] Paljud Transact-SQL-i laused, mis loomine või muutmine andmebaasi tuleb käitada sees oma ja muud Transact-SQL-lausete abil ei saa rühmitada. Lisateavet leiate teemast lause teavet.

## <a name="create-and-manage-logins"></a>Saate luua ja hallata logimine

**Juhtslaidi** andmebaas sisaldab sisselogimise ja millised sisselogimise õigust andmebaaside või muude sisselogimise loomine. Sisselogimise haldamiseks serveritaseme põhisumma login, kui häälestate oma serveri loodud **juhtslaidi** andmebaasiga ühenduse loomisel. Saate **Luua LOGIN**, **Muuta LOGIN**või **LANGEV LOGIN** laused päringute vastu juhtslaidi andmebaas, mida haldab sisselogimise üle kogu server käivitada. Lisateavet leiate teemast [andmebaaside haldamine ja SQL-andmebaasis sisselogimise](http://msdn.microsoft.com/library/azure/ee336235.aspx). 


-   **Luua LOGIN** avalduse abil saate luua serveritaseme sisselogimist. Lisateavet leiate teemast [Luua LOGIN (SQL-andmebaasi)](https://msdn.microsoft.com/library/ms189751.aspx). Järgmine lause loob nimetatakse **login1**sisselogimist. Asendage **parool1** teie valitud parool.

        CREATE LOGIN login1 WITH password='password1';

-   **Luua kasutaja** aruande abil saate andmebaasi taseme õiguste andmine. Kõik sisselogimise tuleb luua **põhi** andmebaasi. Sisselogimise muu andmebaasiga ühenduse loomiseks, peab andma selle andmebaasi **Luua kasutaja** aruande kasutamine andmebaasi taseme õigused. Lisateabe saamiseks vt [Kasutajate loomine (SQL-andmebaasi)](https://msdn.microsoft.com/library/ms173463.aspx). 

-   Andmebaasi, mida nimetatakse **myTestDB**login1 õiguste andmiseks tehke järgmist:

 1.  Objekti Exploreri vaatamiseks **myTestDB** andmebaasi, mille lõite värskendamiseks Paremklõpsake objekti Explorer serveri nimi ja seejärel klõpsake **värskendamine**.  

     Kui ühendus sulgenud, saate uuesti ühendada, valides **Ühenduse objekti Exploreri** menüü fail.

 2. Paremklõpsake **myTestDB** andmebaas ja valige **Uus päring**.

    3.  Järgmine lause myTestDB andmebaasi loomiseks andmebaasi kasutaja **login1User** , mis vastab serveritaseme sisselogimise **login1**käivitada.

            CREATE USER login1User FROM LOGIN login1;

-   Kasutage funktsiooni **sp\_addrolemember** salvestatud protseduur soovitud taseme õiguste andmebaasi kasutajakonto anda. Lisateavet leiate teemast [sp_addrolemember (Transact-SQL-i)](http://msdn.microsoft.com/library/ms187750.aspx). Järgmine lause annab **login1User** kirjutuskaitstud õigused andmebaasi, lisades **login1User** abil soovitud **db\_datareader** roll.

        exec sp_addrolemember 'db_datareader', 'login1User';    

-   Lause **ALTER LOGIN** abil saate muuta olemasolevaid login, näiteks, kui soovite muuta parooli login. Lisateavet leiate teemast [Muuta LOGIN (SQL-andmebaasi)](https://msdn.microsoft.com/library/ms189828.aspx). Lause **ALTER LOGIN** peaks töötama **juhtslaidi** andmebaasist. Aktiveerige uuesti päringuakna, mis on ühendatud selle andmebaasi. Järgmine lause muudab **login1** sisselogimise parool lähtestada. Asendage **Uusparool** oma valiku ja **oldPassword** login praegune parool ja parooli.

        ALTER LOGIN login1
        WITH PASSWORD = 'newPassword'
        OLD_PASSWORD = 'oldPassword';

-   Lause **DROP LOGIN** abil saate kustutada juba olemasoleva sisselogimine. Kustutamise sisselogimist server tasandil kustutatakse ka kõik seotud andmebaasi Kasutajakontod. Lisateavet leiate teemast [KATKESTA andmebaas (SQL-andmebaasi)](https://msdn.microsoft.com/library/ms178613.aspx). Lause **DROP LOGIN** peaks töötama **juhtslaidi** andmebaasist. Lause kustutab **login1** Logi sisse.

        DROP LOGIN login1;

-   Juhtslaidi andmebaas on selle **sys.sql\_sisselogimise** vaadata, et saate vaadata sisselogimise. Käivitada kogu olemasoleva sisselogimise vaatamiseks järgmine tekst:

        SELECT * FROM sys.sql_logins;

## <a name="monitor-sql-database-using-dynamic-management-views"></a>Dünaamilise halduse vaadete kasutamine SQL-andmebaasi jälgimine

SQL-andmebaasi toetab mitut dünaamilise juhtimise vaadet, mille abil saate jälgida üksikute andmebaasiga. Mõned näited kuvari andmetüüp saate kuulata läbi need vaated on järgmine. Kõigi üksikasjade ja rohkem kasutamise näited leiate teemast [dünaamilise halduse vaadete kasutamine SQL-andmebaasi jälgimine](https://msdn.microsoft.com/library/azure/ff394114.aspx).

-   Päringute juhtimise vaate jaoks on vaja **Vaadata andmebaasi riigi** õigused. Kindla andmebaasi kasutaja **Vaadata andmebaasi riigi** õiguste andmiseks andmebaasiga ühenduse loomiseks ja käivitada andmebaasi järgmine tekst:

        GRANT VIEW DATABASE STATE TO login1User;

-   Andmebaasi suurus abil arvutada soovitud **sys.dm\_db\_sektsiooni\_statistika** vaade. Funktsiooni **sys.dm\_db\_sektsiooni\_statistika** vaade tagastab lehe ja ridade arvu teavet iga sektsiooni andmebaasi, mille abil saate arvutada andmebaasi mahtu. Järgmine päring tagastab andmebaasi maht megabaitides:

        SELECT SUM(reserved_page_count)*8.0/1024
        FROM sys.dm_db_partition_stats;   

-   Kasutage soovitud **sys.dm\_exec\_ühendused** ja **sys.dm\_exec\_seansid** vaadete praeguse kasutaja ühendused ja seostatud andmebaasi sisemise ülesannete kohta teabe toomiseks. Järgmine päring tagastab praeguse ühenduse teave:

        SELECT
            e.connection_id,
            s.session_id,
            s.login_name,
            s.last_request_end_time,
            s.cpu_time
        FROM
            sys.dm_exec_sessions s
            INNER JOIN sys.dm_exec_connections e
              ON s.session_id = e.session_id;

-   Kasutage soovitud **sys.dm\_exec\_päringu\_statistika** vaate jaoks liitväärtuse tootlusstatistika toomiseks vahemälus talletatud päringu lepingute. Järgmine päring tagastab keskmise CPU kellaaja järjestab viis põhipäringud teavet.

        SELECT TOP 5 query_stats.query_hash AS "Query Hash",
            SUM(query_stats.total_worker_time), SUM(query_stats.execution_count) AS "Avg CPU Time",
            MIN(query_stats.statement_text) AS "Statement Text"
        FROM
            (SELECT QS.*,
            SUBSTRING(ST.text, (QS.statement_start_offset/2) + 1,
            ((CASE statement_end_offset
                WHEN -1 THEN DATALENGTH(ST.text)
                ELSE QS.statement_end_offset END
                    - QS.statement_start_offset)/2) + 1) AS statement_text
             FROM sys.dm_exec_query_stats AS QS
             CROSS APPLY sys.dm_exec_sql_text(QS.sql_handle) as ST) as query_stats
        GROUP BY query_stats.query_hash
        ORDER BY 2 DESC;
 
 
