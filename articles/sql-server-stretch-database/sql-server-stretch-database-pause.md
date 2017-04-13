<properties
    pageTitle="Viige ja jätkake andmete migreerimise (venitamine andmebaas) | Microsoft Azure'i"
    description="Saate teada, kuidas peatada või seda jätkata andmete migreerimise Azure."
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
    ms.date="06/14/2016"
    ms.author="douglasl"/>

# <a name="pause-and-resume-data-migration-stretch-database"></a>Viige ja jätkake andmete migreerimise (venitamine andmebaas)

Peatamiseks või jätkamiseks andmete migreerimise Azure, valige **venitamine** tabeli SQL Server Management Studio ja **viige** andmete migreerimise peatamiseks või **jätkamiseks** andmete migreerimise jätkamiseks valige. Saate kasutada ka Transact\-SQL-i peatamiseks või jätkamiseks andmete migreerimise.

Paus andmete migreerimise üksikud tabelid, kui soovite tõrkeotsing kohalikus serveris või maksimeerimiseks võrgu läbilaskevõime.

## <a name="pause-data-migration"></a>Paus andmete migreerimine

### <a name="use-sql-server-management-studio-to-pause-data-migration"></a>SQL Server Management Studio abil saate peatada andmete migreerimine

1.  Valige SQL Server Management Studio Exploreris objekti venitada\-tabel, mille jaoks soovite peatada andmete migreerimise lubatud.

2.  Paremal\-klõpsake ja valige **venitada**, ja seejärel valige **Peata**.

### <a name="use-transact-sql-to-pause-data-migration"></a>Kasutage Transact\-SQL-i andmete migreerimise peatamiseks
Käivitage järgmine käsk.

```tsql
USE <Stretch-enabled database name>;
GO
ALTER TABLE <Stretch-enabled table name>  
    SET ( REMOTE_DATA_ARCHIVE ( MIGRATION_STATE = PAUSED ) ) ;  
GO
```

## <a name="resume-data-migration"></a>Elulookirjelduse andmete migreerimine

### <a name="use-sql-server-management-studio-to-resume-data-migration"></a>SQL Server Management Studio abil andmete migreerimise jätkamine

1.  Valige SQL Server Management Studio Exploreris objekti venitada\-tabeli jaoks, mille soovite naasta andmete migreerimise lubatud.

2.  Paremal\-klõpsake ja valige **venitada**ja seejärel valige **Jätka**.

### <a name="use-transact-sql-to-resume-data-migration"></a>Kasutage Transact\-SQL-i andmete migreerimise jätkamiseks
Käivitage järgmine käsk.

```tsql
USE <Stretch-enabled database name>;
GO
ALTER TABLE <Stretch-enabled table name>   
    SET ( REMOTE_DATA_ARCHIVE ( MIGRATION_STATE = OUTBOUND ) ) ;  
 GO
```

## <a name="check-whether-migration-is-active-or-paused"></a>Kontrollige, kas migreerimise on aktiivne või peatatud

### <a name="use-sql-server-management-studio-to-check-whether-migration-is-active-or-paused"></a>SQL Server Management Studio abil saate kontrollida, kas migreerimise on aktiivne või peatatud
SQL Server Management Studio, avage **Venitamine andmebaasi jälgimine** ja märkige ruut **Migreerimise olek** veeru väärtus. Lisateavet leiate teemast [jälgimine ja tõrkeotsing andmete migreerimise](sql-server-stretch-database-monitor.md).

### <a name="use-transact-sql-to-check-whether-migration-is-active-or-paused"></a>Kontrollige, kas migreerimise on aktiivne või peatatud Transact-SQL-i abil
Päringu kataloogi vaade **sys.remote_data_archive_tables** ja märkige **is_migration_paused** veeru väärtus. Lisateavet leiate teemast [sys.remote_data_archive_tables](https://msdn.microsoft.com/library/dn935003.aspx).

## <a name="see-also"></a>Vt ka

[ALTER TABLE (Transact-SQL-i)](https://msdn.microsoft.com/library/ms190273.aspx)
[jälgimine ja tõrkeotsing andmete migreerimise](sql-server-stretch-database-monitor.md)
