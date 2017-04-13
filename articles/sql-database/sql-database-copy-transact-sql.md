<properties 
    pageTitle="Kopeerige abil Transact-SQL Azure'i SQL-andmebaasi | Microsoft Azure'i" 
    description="Eksemplari abil Transact-SQL Azure'i SQL-andmebaasi loomine" 
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="09/19/2016"
    ms.author="sstein"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="copy-an-azure-sql-database-using-transact-sql"></a>Kopeerige Transact-SQL-i abil SQL Azure'i andmebaas


> [AZURE.SELECTOR]
- [Ülevaade](sql-database-copy.md)
- [Azure'i portaal](sql-database-copy-portal.md)
- [PowerShelli](sql-database-copy-powershell.md)
- [T-SQL-IS](sql-database-copy-transact-sql.md)


Selle järgmist näitavad, kuidas kopeerida SQL-andmebaasi Transact-SQL-i sama serveri või muu server. Kopeeri andmebaasitoiming kasutab lause [Andmebaasi loomine](https://msdn.microsoft.com/library/ms176061.aspx) .

Selles artiklis toodud juhiseid lõpule viimiseks peate järgmist:

- Azure'i tellimuse. Kui teil on vaja Azure tellimuse lihtsalt **Tasuta PROOVIVERSIOON** selle lehe ülaosas nuppu ja siis naaske artiklit lõpuleviimiseks.
- Azure'i SQL-andmebaas. Kui te ei ole SQL-andmebaasi, luua ühe selles artiklis toodud juhiseid järgides: [esimese Azure SQL-i andmebaasi loomine](sql-database-get-started.md).
- [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/ms174173.aspx). Kui teil pole SSMS või kui selles artiklis kirjeldatud funktsioonid ei ole saadaval, [Laadige alla uusim versioon](https://msdn.microsoft.com/library/mt238290.aspx).


## <a name="copy-your-sql-database"></a>SQL-andmebaasi kopeerimine

Logige sisse põhi andmebaasi serveritaseme põhisumma login või Logi sisse, mis on loodud andmebaas, mida soovite kopeerida. Sisselogimise, mis pole serveritaseme põhisumma peab olema dbmanager rolli liikmed, et kopeerida andmebaasid. Sisselogimise ja serveriga ühenduse loomise kohta leiate lisateavet teemast [sisselogimise haldamine](sql-database-manage-logins.md).

Käivitage kopeerimise abil [Luua andmebaasi](https://msdn.microsoft.com/library/ms176061.aspx) lause allika andmebaasi. See lause alustab andmebaasi kopeerimise protsess. Kuna kopeerimine andmebaasi on asünkroonne protsess, tagastab andmebaasi loomine lause enne andmebaasi on lõpule jõudnud, kopeerimine.


### <a name="copy-a-sql-database-to-the-same-server"></a>SQL-andmebaasi kopeerimine sama server

Logige sisse põhi andmebaasi serveritaseme põhisumma login või Logi sisse, mis on loodud andmebaas, mida soovite kopeerida. Sisselogimise, mis pole serveritaseme põhisumma peab olema dbmanager rolli liikmed, et kopeerida andmebaasid.

See käsk koopiate Database1 edasi uue andmebaasi nime Database2 samas serveris. Olenevalt teie andmebaasi mahtu kopeerimine võib võtta aega.

    -- Execute on the master database.
    -- Start copying.
    CREATE DATABASE Database1_copy AS COPY OF Database1;

### <a name="copy-a-sql-database-to-a-different-server"></a>Kopeerige SQL-andmebaasi eri server

Logige sisse põhi andmebaasi sihtkoha server, Azure'i SQL-andmebaasi server, kus on uue andmebaasi loomiseks. Kasutage Logi, mis on sama nimi ja parool, kui andmebaasi omanik (DBO) lähteandmebaasi allikas Azure'i SQL-andmebaasi server. Sihtkoha server login tuleb ka dbmanager rolli liige või olla serveritaseme põhisumma Logi sisse.

See käsk kopeerib Database1 server1-uue andmebaasi nimega Database2 server2. Olenevalt teie andmebaasi mahtu kopeerimine võib võtta aega.


    -- Execute on the master database of the target server (server2)
    -- Start copying from Server1 to Server2
    CREATE DATABASE Database1_copy AS COPY OF server1.Database1;
    

## <a name="monitor-the-progress-of-the-copy-operation"></a>Kopeerimine edenemise jälgimine

Jälgida kopeerimise päringud vaadete sys.databases ja sys.dm_database_copies. Kopeerimine on pooleli, kuvatakse state_desc sys.databases vaate uue andmebaasi veerus kopeerimine.


- Kui kopeerimine nurjub, kuvatakse state_desc sys.databases vaate uue andmebaasi veerus kahtlane. Sel juhul käivitada lause DROP uue andmebaasi ja proovige uuesti.
- Kui kopeerimise õnnestub, seatakse state_desc veeru sys.databases vaate uue andmebaasi ONLINE. Sel juhul kopeerimise on lõpule viidud ja uus andmebaas on tavaline andmebaasi, võimalus muuta lähteandmebaasi sõltumatu.

> [AZURE.NOTE] – Kui otsustate kopeerimise ajal see tühistada, käivitada [Andmebaasi KUKUTAGE](https://msdn.microsoft.com/library/ms178613.aspx) lause uue andmebaasi. Teise võimalusena lause DROP andmebaasi allika andmebaasi käivitamisel tühistab kopeerimise.


## <a name="resolve-logins-after-the-copy-operation-completes"></a>Tõrke sisselogimise pärast toimingu Kopeeri

Pärast uue andmebaasi on sihtkoha server võrgus, [Laused ALTER USER](https://msdn.microsoft.com/library/ms176060.aspx) lause abil Kaardistage uuesti kasutajate uue andmebaasi sisselogimise sihtkoht serveris. Orbsisutüübi kasutajate lahendamiseks lugege teemat [Tõrkeotsing Orbsisutüübi kasutajad](https://msdn.microsoft.com/library/ms175475.aspx). Vt ka [SQL Azure'i andmebaasi turvasätete pärast katastroofiabi](sql-database-geo-replication-security-config.md).

Uue andmebaasi kõigi kasutajate haldamine õigused, et nad olid allika andmebaasi. Kasutaja algatatud andmebaasi koopia muutub uue andmebaasi andmebaasi omanik ja turvalisuse ID (SID) on määratud. Pärast kopeerimise õnnestub ja enne, kui teised kasutajad on aadressirida ainult algatatud kopeerimine, andmebaasi omanik (DBO) Logi sisse saate logida uue andmebaasi.


## <a name="next-steps"></a>Järgmised sammud

- Leiate [Azure'i SQL-andmebaasi kopeerimine](sql-database-copy.md) ülevaate kopeerimise Azure'i SQL-andmebaasi.
- Leiate Azure'i portaalis andmebaasi kopeerimiseks [kopeerige Azure'i portaalis Azure SQL-i andmebaas](sql-database-copy-portal.md) .
- Vaadake [Kopeeri Azure'i SQL-andmebaasiga PowerShelli kaudu](sql-database-copy-powershell.md) kopeerimiseks andmebaasi PowerShelli kaudu.
- [SQL Azure'i andmebaasi turvasätete avariitaastet pärast](sql-database-geo-replication-security-config.md) andmebaasi kopeerimisel eri loogilise server kasutajate ja sisselogimise haldamise kohta leiate teemast.



## <a name="additional-resources"></a>Lisaressursid

- [Sisselogimise haldamine](sql-database-manage-logins.md)
- [Ühenduse loomine SQL-andmebaasi SQL Server Management Studio ja valimi T-SQL-päringu tegemine](sql-database-connect-query-ssms.md)
- [Andmebaasi eksportimine mõnda BACPAC](sql-database-export.md)
- [Äritegevuse järjepidevuse ülevaade](sql-database-business-continuity.md)
- [SQL-andmebaasi dokumentatsioon](https://azure.microsoft.com/documentation/services/sql-database/)


