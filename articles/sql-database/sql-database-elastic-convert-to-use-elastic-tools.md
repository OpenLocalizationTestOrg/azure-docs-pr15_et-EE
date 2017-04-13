<properties
   pageTitle="Migreerimine olemasolevaid andmebaase skaala-out | Microsoft Azure'i"
   description="Sharded kasutada elastne Andmebaasiriistad loomisega on Kildu kaardi halduri andmebaaside teisendamine"
   services="sql-database"
   documentationCenter=""
   authors="ddove"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-management"
   ms.date="10/24/2016"
   ms.author="ddove"/>

# <a name="migrate-existing-databases-to-scale-out"></a>Olemasolevaid andmebaase skaala-out migreerimine

Lihtne hallata olemasolevasse mastaabitud kontorist sharded andmebaasi kasutamise Azure'i SQL-andmebaasi Andmebaasiriistad (nt [elastne andmebaasi kliendi teek](sql-database-elastic-database-client-library.md)). Kõigepealt tuleb teil teisendada olemasoleva kogum, mida kasutada [Kildu vastendamine halduri](sql-database-elastic-scale-shard-map-management.md)andmebaaside. 

## <a name="overview"></a>Ülevaade
Olemasoleva sharded andmebaasi migreerimine: 

1. Ettevalmistused [Kildu kaardi halduri andmebaas](sql-database-elastic-scale-shard-map-management.md).
2. Kildu kaardi loomine
3. Üksikute shards ettevalmistamine.  
2. Lisage vastendused Kildu kaardi.

Nende meetodite abil saate rakendada [.NET Framework kliendi teek](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/)või [Azure SQL-i DB - elastne andmebaasi tööriistad skriptide](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db)PowerShelli skriptide abil. Näiteid siin kasutada PowerShelli skriptide.

Funktsiooni ShardMapManager kohta leiate lisateavet teemast [Kildu vastendamine haldus](sql-database-elastic-scale-shard-map-management.md). Elastne Andmebaasiriistad ülevaate leiate teemast [elastne andmebaasi funktsioonide ülevaate](sql-database-elastic-scale-introduction.md).

## <a name="prepare-the-shard-map-manager-database"></a>Ettevalmistused Kildu kaardi Manageri andmebaas

Kildu kaardi manager on teisiti andmebaasi, mis sisaldab andmeid, mastaabitud välja andmebaaside haldamine. Saate kasutada olemasolevat andmebaasi või uue andmebaasi loomiseks. Pange tähele, ei tohiks abistas Kildu kaardi halduri andmebaasi samas andmebaasis on Kildu. Samuti võtke arvesse, et PowerShelli skripti luua andmebaasi jaoks. 

## <a name="step-1-create-a-shard-map-manager"></a>Samm 1: Loo on Kildu kaardi haldur

    # Create a shard map manager. 
    New-ShardMapManager -UserName '<user_name>' 
    -Password '<password>' 
    -SqlServerName '<server_name>' 
    -SqlDatabaseName '<smm_db_name>' 
    #<server_name> and <smm_db_name> are the server name and database name 
    # for the new or existing database that should be used for storing 
    # tenant-database mapping information.

### <a name="to-retrieve-the-shard-map-manager"></a>Toomiseks Kildu kaardi haldur

Pärast loomist, saate tuua Kildu kaardi ülemus selle cmdlet. See toiming on vaja iga kord, kui peate kasutama ShardMapManager objekti.

    # Try to get a reference to the Shard Map Manager  
    $ShardMapManager = Get-ShardMapManager -UserName '<user_name>' 
    -Password '<password>' 
    -SqlServerName '<server_name>' 
    -SqlDatabaseName '<smm_db_name>' 

  
## <a name="step-2-create-the-shard-map"></a>Samm 2: looge Kildu kaardil

Valige tüüpi Kildu kaardi loomiseks. Valik sõltub andmebaasi arhitektuur: 

1. Ühe rentniku andmebaasi kohta (tingimustel, vt [sõnastik](sql-database-elastic-scale-glossary.md)). 
2. Mitme rentniku andmebaasi (kahte tüüpi) kohta:
    3. Loendi vastendamine
    4. Vahemiku kaardistamine
 

Ühe-rentniku mudeli **loendi vastenduse** Kildu kaardi loomine. Ühe-rentniku mudeli määrab ühe andmebaasi rentniku kohta. See on mudelit, mis on tegelik SaaS arendajatele, kuna see lihtsustab haldus.

![Loendi vastendamine][1]

Mitme rentniku mudeli määrab mitu rentnikud ühe andmebaasi (ja üle mitme andmebaasid saate levitada rentnikud rühmad). Selle mudeli kasutamine, kui eeldate, et iga Rentnik on väike andmete vajadustele. Selle mudeli anname mitmesuguseid rentnikud andmebaasi **vahemiku vastenduse**abil. 
 

![Vahemiku kaardistamine][2]

Või saate rakendada mitme rentniku andmebaasi mudeli *loendi vastenduse* abil saate määrata mitme rentniku ühe andmebaasi. Näiteks DEG1 kasutatakse rentniku id 1 ja 5 teabe talletamiseks ja DB2 salvestab andmed rentniku 7 ja 10 rentniku jaoks. 

![Klõpsake ühe DB Muliple rentnikega][3] 

**Vastavalt teie valikule, valige üks järgmistest suvanditest.**

### <a name="option-1-create-a-shard-map-for-a-list-mapping"></a>Variant 1: Kildu kaardi jaoks loendi vastenduse loomine
ShardMapManager objekti abil Kildu kaardi loomine. 

    # $ShardMapManager is the shard map manager object. 
    $ShardMap = New-ListShardMap -KeyType $([int]) 
    -ListShardMapName 'ListShardMap' 
    -ShardMapManager $ShardMapManager 
 
 
### <a name="option-2-create-a-shard-map-for-a-range-mapping"></a>Variant 2: Kildu kaardi jaoks vahemiku vastenduse loomine

Pange tähele, et kasutada seda vastenduse mustri, rentniku id väärtused peab olema pidev vahemike ja võib vahe vahemikud on lihtsalt vahele vahemiku andmebaaside loomisel.

    # $ShardMapManager is the shard map manager object 
    # 'RangeShardMap' is the unique identifier for the range shard map.  
    $ShardMap = New-RangeShardMap 
    -KeyType $([int]) 
    -RangeShardMapName 'RangeShardMap' 
    -ShardMapManager $ShardMapManager 

### <a name="option-3-list-mappings-on-a-single-database"></a>Võimalus 3: Loendi vastendused ühe andmebaasi
See muster häälestamise ka nõuab kaardil loendi loomist, nagu on näidatud etappi 2, variant 1.

## <a name="step-3-prepare-individual-shards"></a>Samm 3: Ettevalmistamine üksikute shards

Kildu kaardi halduri iga Kildu (projekt) lisada. See loob üksikute andmebaaside vastenduse teabe talletamise. See meetod iga Kildu käivitada.
     
    Add-Shard 
    -ShardMap $ShardMap 
    -SqlServerName '<shard_server_name>' 
    -SqlDatabaseName '<shard_database_name>'
    # The $ShardMap is the shard map created in step 2.
 

## <a name="step-4-add-mappings"></a>Samm 4: Lisada vastendused

Vastenduste lisamine sõltub loodud Kildu kaart. Kui olete loonud kaardil loend, lisate loendisse vastendused. Kui olete loonud kaardil vahemik, lisate vahemiku vastendused.

### <a name="option-1-map-the-data-for-a-list-mapping"></a>Variant 1: kaardi loendi vastenduse andmeid

Vastendage andmed, lisades iga rentniku loendi vastenduse.  

    # Create the mappings and associate it with the new shards 
    Add-ListMapping 
    -KeyType $([int]) 
    -ListPoint '<tenant_id>' 
    -ListShardMap $ShardMap 
    -SqlServerName '<shard_server_name>' 
    -SqlDatabaseName '<shard_database_name>' 

### <a name="option-2-map-the-data-for-a-range-mapping"></a>Variant 2: kaardi vastenduse vahemiku andmed

Vahemiku vastendused kõigi rentniku id vahemiku – andmebaasi seosed lisamiseks tehke järgmist.

    # Create the mappings and associate it with the new shards 
    Add-RangeMapping 
    -KeyType $([int]) 
    -RangeHigh '5' 
    -RangeLow '1' 
    -RangeShardMap $ShardMap 
    -SqlServerName '<shard_server_name>' 
    -SqlDatabaseName '<shard_database_name>' 


### <a name="step-4-option-3-map-the-data-for-multiple-tenants-on-a-single-database"></a>Samm 4 suvand 3: andmete vastendama ühe andmebaasi mitme rentniku jaoks

Käivitage iga rentniku lisa-ListMapping (variant 1, kohal). 


## <a name="checking-the-mappings"></a>Vastendused kontrollimine

Teavet olemasoleva shards ja nendega seotud vastendused saate kasutataks, kasutades järgmisi käske:  

    # List the shards and mappings 
    Get-Shards -ShardMap $ShardMap 
    Get-Mappings -ShardMap $ShardMap 

## <a name="summary"></a>Kokkuvõte

Kui olete häälestamise lõpule viinud, saate hakata kasutama elastne andmebaasi kliendi teek. Samuti saate [andmed sõltuvad marsruutimine](sql-database-elastic-scale-data-dependent-routing.md) ja [mitme Kildu päringu](sql-database-elastic-scale-multishard-querying.md).

## <a name="next-steps"></a>Järgmised sammud


[Azure'i SQL-andmebaasi DB-elastne tööriistad sripts](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db)PowerShelli skriptide toomine.

Tööriistade on ka github: [Azure'i/elastne db tööriistad](https://github.com/Azure/elastic-db-tools).

Tükeldatud ühendamine tööriista abil andmete teisaldamine või mitme rentniku mudelist ühe rentniku mudel. Lugege teemat [tükeldatud Ühenda tööriista](sql-database-elastic-scale-get-started.md).

## <a name="additional-resources"></a>Lisaressursid

Ühiseid andmeid arhitektuur mudeleid mitme rentniku tarkvara-kui-a-service (SaaS) andmebaasi rakenduste kohta leiate teavet teemast [Kujundus mustrid mitme rentniku SaaS rakenduste Azure SQL-andmebaas](sql-database-design-patterns-multi-tenancy-saas-applications.md).

## <a name="questions-and-feature-requests"></a>Küsimused ja soovitada funktsioone

Küsimused, palun jõuda meile [SQL-andmebaasi Foorum](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted) ja soovitada funktsioone, lisage need [SQL-andmebaasi tagasiside Foorum](https://feedback.azure.com/forums/217321-sql-database/).

<!--Image references-->
[1]: ./media/sql-database-elastic-convert-to-use-elastic-tools/listmapping.png
[2]: ./media/sql-database-elastic-convert-to-use-elastic-tools/rangemapping.png
[3]: ./media/sql-database-elastic-convert-to-use-elastic-tools/multipleonsingledb.png
 
