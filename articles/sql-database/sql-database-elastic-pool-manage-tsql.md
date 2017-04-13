<properties 
    pageTitle="Luua või elastne kohapeal on abil T-SQL Azure'i SQL-andmebaasiga viimine | Microsoft Azure'i" 
    description="T-SQL-i abil saate luua Azure'i SQL-andmebaasiga on elastne kogumi. Või T-SQL-i abil teisaldada selle datbase ja sealt kaustu." 
    services="sql-database" 
    documentationCenter="" 
    authors="srinia" 
    manager="jhubbard" 
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="data-management" 
    ms.date="05/27/2016"
    ms.author="srinia"/>

# <a name="monitor-and-manage-an-elastic-database-pool-with-transact-sql"></a>Jälgimine ja haldamine on elastne andmebaasi rakenduskausta Transact-SQL-i abil  

> [AZURE.SELECTOR]
- [Azure'i portaal](sql-database-elastic-pool-manage-portal.md)
- [PowerShelli](sql-database-elastic-pool-manage-powershell.md)
- [C#:](sql-database-elastic-pool-manage-csharp.md)
- [T-SQL-IS](sql-database-elastic-pool-manage-tsql.md)

Luua ja teisaldada andmebaasid ja sealt elastne kaustu [(Azure'i SQL-andmebaasi) andmebaasi luua](https://msdn.microsoft.com/library/dn268335.aspx) ja [Muuta Database(Azure SQL Database)](https://msdn.microsoft.com/library/mt574871.aspx) käskude abil. Elastne rakenduskausta kuuluma saate kasutada järgmisi käske. Need käsud mõjutavad ainult andmebaasid. Uue kaustu luua ja kausta atribuudid (nt min ja max eDTUs) säte ei saa muuta T-SQL-i käskude abil.

## <a name="create-a-new-database-in-an-elastic-pool"></a>Uue andmebaasi loomiseks on elastne pargis
ANDMEBAASI loomine käsu SERVICE_OBJECTIVE suvandiga.   

    CREATE DATABASE db1 ( SERVICE_OBJECTIVE = ELASTIC_POOL (name = [S3M100] ));
    -- Create a database named db1 in a pool named S3M100.

Kõik andmebaasid on elastne rakenduskausta pärivad teenuse kiht elastne pool (Basic, Standard, Premium). 


## <a name="move-a-database-between-elastic-pools"></a>Andmebaasi teisaldamine elastne kaustu
Käsu Muuda andmebaasi kasutamine Muuda ja määrata teenuse\_eesmärk suvand nimega ELASTNE\_POOL; Määrake nimi sihtrakenduse pool nime.

    ALTER DATABASE db1 MODIFY ( SERVICE_OBJECTIVE = ELASTIC_POOL (name = [PM125] ));
    -- Move the database named db1 to a pool named P1M125  

## <a name="move-a-database-into-an-elastic-pool"></a>Andmebaasi viimine kohapeal on elastne 
Käsu Muuda andmebaasi kasutamine Muuda ja määrata teenuse\_eesmärk suvand nimega ELASTIC_POOL; Määrake nimi sihtrakenduse pool nime.

    ALTER DATABASE db1 MODIFY ( SERVICE_OBJECTIVE = ELASTIC_POOL (name = [S3100] ));
    -- Move the database named db1 to a pool named S3100.

## <a name="move-a-database-out-of-an-elastic-pool"></a>Andmebaasi kolima kohapeal on elastne
Käsuga Muuda andmebaasi ja määrake soovitud SERVICE_OBJECTIVE üks jõudluse tasemeid (S0, S1 jne).

    ALTER DATABASE db1 MODIFY ( SERVICE_OBJECTIVE = 'S1');
    -- Changes the database into a stand-alone database with the service objective S1.

## <a name="list-databases-in-an-elastic-pool"></a>Loendi andmebaasid on elastne pargis
Kasutage soovitud [sys.database\_teenuse \_eesmärkide vaade](https://msdn.microsoft.com/library/mt712619) loetleda kõik andmebaasid elastne kohapeal on. Logige sisse juhtslaid andmebaasi päringu vaate.

    SELECT d.name, slo.*  
    FROM sys.databases d 
    JOIN sys.database_service_objectives slo  
    ON d.database_id = slo.database_id
    WHERE elastic_pool_name = 'MyElasticPool'; 

## <a name="get-resource-usage-data-for-a-pool"></a>On ressursi kasutamine andmete hankimine

Kasutamine on [sys.elastic\_pool \_ressurss \_statistika vaade](https://msdn.microsoft.com/library/mt280062.aspx) uurida ressursi kasutuse statistika on elastne puhvri loogilise serveris. Logige sisse juhtslaid andmebaasi päringu vaate.

    SELECT * FROM sys.elastic_pool_resource_stats 
    WHERE elastic_pool_name = 'MyElasticPool'
    ORDER BY end_time DESC;

## <a name="get-resource-usage-for-an-elastic-database"></a>Ressursikasutus elastne andmebaasi hankimine

Kasutamine on [sys.dm\_ db\_ ressurss\_statistika vaade](https://msdn.microsoft.com/library/dn800981.aspx) või [sys.resource \_statistika vaade](https://msdn.microsoft.com/library/dn269979.aspx) uurida ressursi kasutuse statistika on elastne rakenduskausta andmebaasi. See protsess on sarnane päringute ressursikasutus iga ühe andmebaasi.

## <a name="next-steps"></a>Järgmised sammud

Pärast kohapeal on elastne andmebaasi loomist saate hallata elastne andmebaaside kogumi elastne töökohtade loomine. Elastne tööd hõlbustada töötava T-SQL-i skriptide andmebaaside kogumi mõni muu arv peale. Lisateavet leiate teemast [elastne andmebaasi töö ülevaade](sql-database-elastic-jobs-overview.md). 

Vt [mastaapimine välja koos Azure'i SQL-andmebaasi](sql-database-elastic-scale-introduction.md): elastne Andmebaasiriistad abil skaala-out, andmete teisaldamine päringu või luua tehingud.
