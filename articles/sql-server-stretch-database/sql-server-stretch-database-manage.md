<properties
    pageTitle="Haldamine ja venitamine andmebaasi tõrkeotsing | Microsoft Azure'i"
    description="Saate teada, kuidas hallata ja venitamine andmebaasi tõrkeotsing."
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
    ms.date="06/27/2016"
    ms.author="douglasl"/>

# <a name="manage-and-troubleshoot-stretch-database"></a>Haldamine ja venitamine andmebaasi tõrkeotsing

Haldamine ja tõrkeotsing venitamine andmebaasi, kasutada tööriistu ja selles teemas kirjeldatud.

## <a name="manage-local-data"></a>Kohalike andmete haldamine

### <a name="LocalInfo"></a>Kohaliku andmebaasi ja tabelite lubatud venitamine andmebaasi kohta teabe hankimine
Avage kataloogi vaadete **sys.databases** ja **sys.tables** venitamine kohta teabe kuvamiseks\-lubatud SQL serveri andmebaasi ja tabeleid. Lisateavet leiate teemast [sys.databases (Transact-SQL-i)](https://msdn.microsoft.com/library/ms178534.aspx) ja [sys.tables (Transact-SQL-i)](https://msdn.microsoft.com/library/ms187406.aspx).

Kui soovite vaadata, kui palju ruumi venitada\-lubatud tabeli kasutab SQL Server, käivitage järgmine tekst.

 ```tsql
USE <Stretch-enabled database name>;
GO
EXEC sp_spaceused '<Stretch-enabled table name>', 'true', 'LOCAL_ONLY';
GO
 ```
## <a name="manage-data-migration"></a>Andmete migreerimise haldamine

### <a name="check-the-filter-function-applied-to-a-table"></a>Funktsioon filter rakendatud tabeli kontrollimine
Kataloogi vaatamiseks avage **sys.remote\_andmete\_arhiivi\_tabelite** ja märkige ruut väärtus on **filter\_predikaat** veeru tuvastamise funktsiooni kasutav venitamine andmebaasi migreerimine ridade valimiseks. Kui väärtus on null, kogu tabel on õigus migreerida. Lisateavet leiate teemast [sys.remote_data_archive_tables (Transact-SQL-i)](https://msdn.microsoft.com/library/dn935003.aspx) ja [Valige read migreerimiseks filtri funktsiooni abil](sql-server-stretch-database-predicate-function.md).

### <a name="Migration"></a>Andmete migreerimise oleku kontrollimine
Valige **ülesanded | Venitamine | Kuvari** andmebaasi SQL Server Management Studio andmete migreerimise venitamine andmebaasi monitoris jälgimiseks. Lisateavet leiate teemast [jälgimine ja tõrkeotsing andmete migreerimise (venitamine andmebaas)](sql-server-stretch-database-monitor.md).

Või dünaamilise juhtimise vaatamiseks avage **sys.dm\_db\_rda\_migreerimise\_olek** näha mitu pakettidena ja andmeridade migreerimist.

### <a name="Firewall"></a>Andmete migreerimise tõrkeotsing
Tõrkeotsing soovitusi, vt [jälgimine ja tõrkeotsing andmete migreerimise (venitamine andmebaas)](sql-server-stretch-database-monitor.md).

## <a name="manage-remote-data"></a>Andmete haldamine

### <a name="RemoteInfo"></a>Remote andmebaasid ja tabelid, mis kasutavad venitamine andmebaasi kohta teabe hankimine
Kataloogi vaadete **sys.remote\_andmete\_arhiivi\_andmebaaside** ja **sys.remote\_andmete\_arhiivi\_tabelite** kuvamiseks remote andmebaasid ja tabelid, kus on salvestatud migreeritud andmete kohta. Lisateavet leiate teemast [sys.remote_data_archive_databases (Transact-SQL-i)](https://msdn.microsoft.com/library/dn934995.aspx) ja [sys.remote_data_archive_tables (Transact-SQL-i)](https://msdn.microsoft.com/library/dn935003.aspx).

Palju ruumi vaatamiseks kasutab tabeli venitamine lubatud Azure, käivitage järgmine tekst.

 ```tsql
USE <Stretch-enabled database name>;
GO
EXEC sp_spaceused '<Stretch-enabled table name>', 'true', 'REMOTE_ONLY';
GO
 ```

### <a name="delete-migrated-data"></a>Migreeritud andmete kustutamine  
Kui soovite kustutada, mis on juba migreeritud Azure, järgige [sys.sp_rda_reconcile_batch](https://msdn.microsoft.com/library/mt707768.aspx)kirjeldatud juhiseid.

## <a name="manage-table-schema"></a>Tabeli skeemi haldamine

### <a name="dont-change-the-schema-of-the-remote-table"></a>Ei muuda skeemi serveri tabel
Remote Azure'i tabeli, mis on konfigureeritud venitamine andmebaasi SQL Serveri tabeliga seotud skeemi ei muuda. Ärge muutke nime või veeru andmetüüpi. Venitamine andmebaasi funktsiooni abil erinevate oletused skeemi kohta remote tabeli SQL serveri tabel skeemi suhtes. Kui muudate remote skeemiga, venitamine andmebaasi lakkab töötamast muudetud tabeli.

### <a name="reconcile-table-columns"></a>Tabeli veergude sobitada  
Kui olete kogemata kustutanud remote tabelis veerud, käivitage **sp_rda_reconcile_columns** veergude lisamiseks serveri tabel, mis on olemas venitada\-lubatud SQL serveri tabel, kuid mitte serveri tabel. Lisateavet leiate teemast [sys.sp_rda_reconcile_columns](https://msdn.microsoft.com/library/mt707765.aspx).  

  > [!IMPORTANT] Kui **sp_rda_reconcile_columns** taasloob kogemata kustutanud serveri tabeli veergude, seda ei saa taastada andmeid, mis oli varem kustutatud veerud.

**sp_rda_reconcile_columns** ei kustuta veerud serveri tabel, mis on olemas serveri tabel, kuid mitte venitada\-lubatud SQL serveri tabel. Kui leidub serveri Azure'i tabeli veergu, mis pole enam venitada\-lubatud SQL serveri tabel, nende eest veergude ei takista venitamine andmebaasi tavapäraselt. Soovi korral saate eest veergude käsitsi eemaldada.  

## <a name="manage-performance-and-costs"></a>Jõudluse ja kulud haldamine  

### <a name="troubleshoot-query-performance"></a>Päringu jõudluse tõrkeotsing
Päringud, mis sisaldavad venitamine\-lubatud tabelite eeldatakse, et teha aeglasemalt nagu enne venitamine tabelid on lubatud. Kui päringu jõudluse laguneb oluliselt, vaadake järgmisi võimalikke probleeme.

-   On erinevate geograafiliste ala, kui teie SQL Server Azure'i serverisse? Konfigureerige Azure olema sama geograafilise piirkonna SQL Server võrgu latentsuse vähendada.

-   Oma võrgu läbilaskevõime võib olla halvenenud. Pöörduda võrguadministraatori poole tehtud probleemid või katkestuste kohta.

### <a name="increase-azure-performance-level-for-resource-intensive-operations-such-as-indexing"></a>Azure'i jõudluse tihendada ressursi\-intensiivne toimingute, nt indekseerimine
Kui koostada, taastada või ümber korraldada registri suur tabel, kus on konfigureeritud lubama andmebaasi venitada ja eeldate, et migreeritud andmete Azure raske päringute sel ajal, kaaluge vastavate Azure kaugandmebaasiga jõudluse suurendades toimingu kestel. Jõudluse tasemed ja hinnakirjad kohta lisateabe saamiseks vt [SQL serveri venitamine andmebaasi hinnad](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/).

### <a name="you-cant-pause-the-sql-server-stretch-database-service-on-azure"></a>Te ei saa peatada Azure SQL serveri venitamine andmebaasi teenus  
 Veenduge, et vastav jõudlus ja hinnad taseme valimine. Kui saate parandada jõudlust ajutiselt jaoks ressursi\-toiming, taastada endise pärast selle toimingu lõpuleviimist. Jõudluse tasemed ja hinnakirjad kohta lisateabe saamiseks vt [SQL serveri venitamine andmebaasi hinnad](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/).  

## <a name="change-the-scope-of-queries"></a>Muuda päringu ulatust  
 Päringute vastu venitamine\-lubatud tabelite tagastada kohalike ning kaugarvutite andmete vaikimisi. Saate muuta kõigi päringute kõigi kasutajate jaoks või ainult ühe päringu administraator ulatuse päringud.  

### <a name="change-the-scope-of-queries-for-all-queries-by-all-users"></a>Muuda ulatust päringute kõik päringud kõigi kasutajate jaoks  
 Kõigi kasutajate kõik päringud ulatuse muutmiseks käivitage salvestatud protseduur **sys.sp_rda_set_query_mode**. Saate vähendada ulatuse päringu kohalikud andmed ainult, Keela kõik päringud või taastada vaikesäte. Lisateavet leiate teemast [sys.sp_rda_set_query_mode](https://msdn.microsoft.com/library/mt703715.aspx).  

### <a name="queryHints"></a>Muuda päringu ulatust ühe päringu administraator  
 Ühe päringu db_owner rolli liige ulatuse muutmiseks lisage soovitud * *asendaja \( REMOTE_DATA_ARCHIVE_OVERRIDE = *väärtus* \)** päringu vihje, et SELECT-lause. REMOTE_DATA_ARCHIVE_OVERRIDE päringu vihje võib olla järgmised väärtused.  
 -   **LOCAL_ONLY**. Päringu ainult kohalikud andmed.  

 -   **REMOTE_ONLY**. Päringu andmete ainult.  

 -   **STAGE_ONLY**. Päringu ainult andmed tabeli, kus venitamine andmebaasi etappide ridade õigus migreerimise ja säilitab migreeritud read määratud perioodi pärast migreerimist. Selle päringu vihje on ainus võimalus päringu vahekausta tabelis.  

Näiteks järgmine päring tagastab ainult kohaliku tulemid.  

 ```tsql  
 USE <Stretch-enabled database name>;
 GO
 SELECT * FROM <Stretch_enabled table name> WITH (REMOTE_DATA_ARCHIVE_OVERRIDE = LOCAL_ONLY) WHERE ... ;
 GO
```  

## <a name="adminHints"></a>Veenduge, et haldus värskendused ja kustutab  
 Vaikimisi ei saa värskendada või Kustuta read, mis vastavad tingimustele migreerimise või read, mis on juba migreeritud, venitada\-lubatud tabel. Kui teil on probleemi lahendada, db_owner rolli liige käitamise värskendamine või kustutamine toimingut, lisades selle * *asendaja \( REMOTE_DATA_ARCHIVE_OVERRIDE = *väärtus* \)** päringu vihje-lausesse. REMOTE_DATA_ARCHIVE_OVERRIDE päringu vihje võib olla järgmised väärtused.  
 -   **LOCAL_ONLY**. Värskendada või kustutada ainult kohalikud andmed.  

 -   **REMOTE_ONLY**. Värskendada või kustutada ainult remote andmed.  

 -   **STAGE_ONLY**. Värskendada või kustutada ainult andmed tabeli, kus venitamine andmebaasi etappide ridade õigus migreerimise ja säilitab migreeritud read määratud perioodi pärast migreerimist.  

## <a name="see-also"></a>Vt ka

[Kuvari venitus andmebaas](sql-server-stretch-database-monitor.md)

[Varukoopia venitamine lubatud andmebaasid](sql-server-stretch-database-backup.md)

[Venitamine lubatud andmebaasi taastamine](sql-server-stretch-database-restore.md)
