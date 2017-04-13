<properties 
    pageTitle="Mitme Kildu päringute | Microsoft Azure'i" 
    description="Päringute sooritamine üle shards abil elastne andmebaasi kliendi teek." 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="torsteng" 
    editor=""/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="04/12/2016" 
    ms.author="torsteng"/>

# <a name="multi-shard-querying"></a>Mitme Kildu päringud

## <a name="overview"></a>Ülevaade

[Elastne Andmebaasiriistad](sql-database-elastic-scale-introduction.md), saate luua sharded andmebaasi lahendusi. **Mitme Kildu päringute** kasutatakse ülesandeid, nagu andmete kogumine ja aruandlus, mis nõuavad töötavad päringut, mis ulatub üle mitme shards. (Kontrast see [andmed sõltuvad marsruutimine](sql-database-elastic-scale-data-dependent-routing.md), mis töötab kõik ühe Kildu.) 

## <a name="overview"></a>Ülevaade

1. Hangi [**RangeShardMap**](https://msdn.microsoft.com/library/azure/dn807318.aspx) või [**ListShardMap**](https://msdn.microsoft.com/library/azure/dn807370.aspx) [**TryGetRangeShardMap**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetrangeshardmap.aspx), [**TryGetListShardMap**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetlistshardmap.aspx)või [**GetShardMap**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.getshardmap.aspx) meetodi abil. Lugege teemat [**ehitamine on ShardMapManager**](sql-database-elastic-scale-shard-map-management.md#constructing-a-shardmapmanager) ja [**RangeShardMap või ListShardMap**](sql-database-elastic-scale-shard-map-management.md#get-a-rangeshardmap-or-listshardmap).
2. **[MultiShardConnection](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardconnection.aspx)** objekti loomine.
2. Saate luua ka **[MultiShardCommand](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardcommand.aspx)**. 
3. Määrake **[atribuudi CommandText](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardcommand.commandtext.aspx#P:Microsoft.Azure.SqlDatabase.ElasticScale.Query.MultiShardCommand.CommandText)** T-SQL-i käsk.
3. Käsu, helistades **[ExecuteReader meetod](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardcommand.executereader.aspx)**.
4. Kasutades **[MultiShardDataReader klassi](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multisharddatareader.aspx)**tulemusi vaadata. 

## <a name="example"></a>Näide

Järgmine kood iseloomustab mitme Kildu päringute abil antud **ShardMap** nimega *myShardMap*kasutamist. 

    using (MultiShardConnection conn = new MultiShardConnection( 
                                        myShardMap.GetShards(), 
                                        myShardConnectionString) 
          ) 
    { 
    using (MultiShardCommand cmd = conn.CreateCommand())
           { 
            cmd.CommandText = "SELECT c1, c2, c3 FROM ShardedTable"; 
            cmd.CommandType = CommandType.Text; 
            cmd.ExecutionOptions = MultiShardExecutionOptions.IncludeShardNameColumn; 
            cmd.ExecutionPolicy = MultiShardExecutionPolicy.PartialResults; 

            using (MultiShardDataReader sdr = cmd.ExecuteReader()) 
                { 
                    while (sdr.Read())
                        { 
                            var c1Field = sdr.GetString(0); 
                            var c2Field = sdr.GetFieldValue<int>(1); 
                            var c3Field = sdr.GetFieldValue<Int64>(2);
                        } 
                } 
           } 
    } 

 
Põhiline erinevus on mitme Kildu ühendused ehitamine. Kui **SqlConnection** toimib ühe andmebaasi, suunab **MultiShardConnection** selle sisendina ***shards kogum*** . Asustada shards Kildu kaart kogum. Päring käivitatakse seejärel shards abil **UNION ALL** semantika otsustama ühe üldise tulemi kogumist. Soovi korral saab lisada Kildu, kui rida pärineb nime atribuudi **ExecutionOptions** kasutamine käsu väljund. 

Pange tähele **myShardMap.GetShards()**kõne. See meetod toob kõik shards Kildu kaardi ja käivitage päring üle kõik oluline andmebaasid on lihtne. Shards kogum mitme Kildu päringu saab rafineeritud täpsemaks täites üle selle saidikogumi LINQ päringu tagastatud **myShardMap.GetShards()**kõne. Kombinatsioonis osaline tulemuste poliitika on loodud praeguse võimalus mitme Kildu päringute töötab ka kümneid kuni sadu shards.

Mitme Kildu päringute piirang on praegu valideerimine shards ja shardlets, mis on esitama päringu puudumine. Ajal andmed sõltuvad marsruutimine kontrollib, kas antud Kildu on osa Kildu kaart päringute ajal, ärge tehke mitme Kildu päringute selle sisse. See võib põhjustada mitme Kildu päringute töötavate andmebaasidele, mis on eemaldatud Kildu kaarti.

## <a name="multi-shard-queries-and-split-merge-operations"></a>Mitme Kildu päringud ja tükeldatud ühendamine toimingud

Mitme Kildu päringute kontrollida, kas shardlets päringu andmebaasi osalevad poolelioleva tükeldatud ühendamine toimingud. (Vt [mastaapimine elastne tükeldatud ühendamine andmebaasiriista abil](sql-database-elastic-scale-overview-split-and-merge.md)). See võib kaasa tuua selliste vastuolude tekkimist, kus sama shardlet ridade kuvamine sama mitme Kildu päringu mitu andmebaasid. Arvestage need piirangud ja arvesse võtta äravool poolelioleva tükeldatud ühendamine toimingute ja muudatuste Kildu kaardi täites mitme Kildu päringud.

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

## <a name="see-also"></a>Vt ka
**[System.Data.SqlClient](http://msdn.microsoft.com/library/System.Data.SqlClient.aspx)** tunnid ja meetoditest.


Hallata shards [elastne andmebaasi kliendi teek](sql-database-elastic-database-client-library.md). Sisaldab nimega [Microsoft.Azure.SqlDatabase.ElasticScale.Query](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.aspx) , mis pakub võimalus päringu tulem ja ühe päringu abil mitme shards nimeruum. Päringu võtmiseks pakub üle shards kogum. See sisaldab ka alternatiivseid Andmetäite poliitikad, eriti osalist tulemused, tegeleda tõrkeid, kui palju shards üle päringute.  

 