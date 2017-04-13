<properties
    pageTitle="Jälgimine ja haldamine on elastne andmebaasi rakenduskausta C# | Microsoft Azure'i"
    description="C# meetodid andmebaasi abil saate hallata elastne andmebaasi kohapeal on Azure SQL-andmebaas."
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="csharp"
    ms.workload="data-management"
    ms.date="10/04/2016"
    ms.author="sstein"/>

# <a name="monitor-and-manage-an-elastic-database-pool-with-cx23"></a>Jälgimine ja haldamine on elastne andmebaasi rakenduskausta C & #x23; 

> [AZURE.SELECTOR]
- [Azure'i portaal](sql-database-elastic-pool-manage-portal.md)
- [PowerShelli](sql-database-elastic-pool-manage-powershell.md)
- [C#:](sql-database-elastic-pool-manage-csharp.md)
- [T-SQL-IS](sql-database-elastic-pool-manage-tsql.md)


Saate teada, kuidas hallata on [elastne andmebaasi rakenduskausta](sql-database-elastic-pool.md) C ja #x23; abil. 

>[AZURE.NOTE] Palju uusi funktsioone SQL-andmebaas on toetatud ainult siis, kui kasutate [Azure ressursihaldur juurutamise mudeli](../azure-resource-manager/resource-group-overview.md), seega peaksite kasutama alati Viimane **Azure SQL-i andmebaasi teegis .net-i ([dokumendid](https://msdn.microsoft.com/library/azure/mt349017.aspx) | [Nugeti pakett](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql))**. Vanema [klassikaline juurutamise mudeli teekide](https://www.nuget.org/packages/Microsoft.WindowsAzure.Management.Sql) on toetatud tagasiühilduvuse ainult, seega soovitame kasutada uuem vastavalt ressursihaldur teekide.

Selles artiklis toodud juhiseid lõpuleviimiseks vajate järgmist:

- Elastne rakenduskausta (pool, mida soovite hallata). Loomiseks on, vaadake teemat [loomine mõne elastne andmebaasi rakenduskausta C#](sql-database-elastic-pool-create-csharp.md).
- Visual Studio. Visual Studio tasuta koopia, vt [Visual Studio allalaadimine](https://www.visualstudio.com/downloads/download-visual-studio-vs) .


## <a name="move-a-database-into-an-elastic-pool"></a>Andmebaasi viimine kohapeal on elastne

Saate teisaldada eraldiseisev andmebaasi või on väljaspool.  

    // Retrieve current database properties.

    currentDatabase = sqlClient.Databases.Get("resourcegroup-name", "server-name", "Database1").Database;

    // Configure create or update parameters with existing property values, override those to be changed.
    DatabaseCreateOrUpdateParameters updatePooledDbParameters = new DatabaseCreateOrUpdateParameters()
    {
        Location = currentDatabase.Location,
        Properties = new DatabaseCreateOrUpdateProperties()
        {
            Edition = "Standard",
            RequestedServiceObjectiveName = "ElasticPool",
            ElasticPoolName = "ElasticPool1",
            MaxSizeBytes = currentDatabase.Properties.MaxSizeBytes,
            Collation = currentDatabase.Properties.Collation,
        }
    };

    // Update the database.
    var dbUpdateResponse = sqlClient.Databases.CreateOrUpdate("resourcegroup-name", "server-name", "Database1", updatePooledDbParameters);

## <a name="list-databases-in-an-elastic-pool"></a>Loendi andmebaasid on elastne pargis

Kõik andmebaasid on toomiseks helistage [ListDatabases](https://msdn.microsoft.com/library/microsoft.azure.management.sql.elasticpooloperationsextensions.listdatabases) meetod.

    //List databases in the elastic pool
    DatabaseListResponse dbListInPool = sqlClient.ElasticPools.ListDatabases("resourcegroup-name", "server-name", "ElasticPool1");
    Console.WriteLine("Databases in Elastic Pool {0}", "server-name.ElasticPool1");
    foreach (Database db in dbListInPool)
    {
        Console.WriteLine("  Database {0}", db.Name);
    }

## <a name="change-performance-settings-of-a-pool"></a>Jõudluse, on sätete muutmine

Saate tuua olemasoleva kausta atribuudid. Muutke väärtusi ja käivitada CreateOrUpdate meetod.

    var currentPool = sqlClient.ElasticPools.Get("resourcegroup-name", "server-name", "ElasticPool1").ElasticPool;

    // Configure create or update parameters with existing property values, override those to be changed.
    ElasticPoolCreateOrUpdateParameters updatePoolParameters = new ElasticPoolCreateOrUpdateParameters()
    {
        Location = currentPool.Location,
        Properties = new ElasticPoolCreateOrUpdateProperties()
        {
            Edition = currentPool.Properties.Edition,
            DatabaseDtuMax = 50, /* Setting DatabaseDtuMax to 50 limits the eDTUs that any one database can consume */
            DatabaseDtuMin = 10, /* Setting DatabaseDtuMin above 0 limits the number of databases that can be stored in the pool */
            Dtu = (int)currentPool.Properties.Dtu,
            StorageMB = currentPool.Properties.StorageMB,  /* For a Standard pool there is 1 GB of storage per eDTU. */
        }
    };

    newPoolResponse = sqlClient.ElasticPools.CreateOrUpdate("resourcegroup-name", "server-name", "ElasticPool1", newPoolParameters);


## <a name="latency-of-elastic-pool-operations"></a>Latentsus elastne rakenduskausta toimingud

- Min eDTUs kohta andmebaasi või max eDTUs andmebaasi kohta tavaliselt muutmise lõpetab viis minutit või vähem.
- Aeg (eDTUs) kausta suuruse muutmiseks sõltub kõik andmebaasid kogumi kogumaht. Muudatuste Keskmine 90 minutit iga 100 GB. Näiteks kui kokku ruumi pool kõik andmebaasid on 200 GB, siis oodatud latentsus muutmine pool eDTU pool kohta on 1 tund või vähem.




## <a name="additional-resources"></a>Lisaressursid

- [SQL-i tõrkekoodid SQL-andmebaasi klientrakendustes: andmebaasi ühenduse tõrge ja ka muude probleemide](sql-database-develop-error-messages.md).
- [SQL-andmebaas](https://azure.microsoft.com/documentation/services/sql-database/)
- [Azure'i ressursside halduse API-d](https://msdn.microsoft.com/library/azure/dn948464.aspx)
- [Luua uue andmebaasi elastne rakenduskausta C#](sql-database-elastic-pool-create-csharp.md)
- [Millal tuleks kasutada elastne andmebaasi kohapeal on?](sql-database-elastic-pool-guidance.md)
- Vt [mastaapimine välja koos Azure'i SQL-andmebaasi](sql-database-elastic-scale-introduction.md): elastne Andmebaasiriistad abil skaala-out, andmete teisaldamine päringu või luua tehingud.

