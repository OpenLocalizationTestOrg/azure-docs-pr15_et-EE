<properties 
    pageTitle="Lisada mõne Kildu elastne Andmebaasiriistad abil | Microsoft Azure'i" 
    description="Kuidas elastne skaala API abil saate lisada uue shards on Kildu määrata." 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="ddove" 
    editor=""/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="ddove"/>

# <a name="adding-a-shard-using-elastic-database-tools"></a>Lisada mõne Kildu elastne Andmebaasiriistad abil

## <a name="to-add-a-shard-for-a-new-range-or-key"></a>Lisamiseks mõne uue vahemiku või klahvi Kildu  

Rakenduste sageli vaja lihtsalt lisada uusi shards käsitlema andmeid, mida oodata on uus võtmed või võtme vahemikud, Kildu kaart, mis on juba olemas. Näiteks võib-olla ettevalmistamise uute Kildu uue rentniku jaoks rakenduse sharded rentniku ID või vajalikud andmed sharded kord kuus uue Kildu, ette valmistatud enne iga uue kuu alguse. 

Kui uus lahtrivahemik väärtused ei ole juba mõne olemasoleva vastenduse osa, on väga lihtne lisada uue Kildu ja uue tootenumbri või vahemiku selle Kildu seostada. 

### <a name="example--adding-a-shard-and-its-range-to-an-existing-shard-map"></a>Näide: lisamise olemasoleva Kildu vastenduse lisamine Shardi ja selle vahemiku
See näide kasutab [TryGetShard](https://msdn.microsoft.com/library/azure/dn823929.aspx) [CreateShard](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.createshard.aspx) [CreateRangeMapping](https://msdn.microsoft.com/library/azure/dn807221.aspx#M:Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.RangeShardMap`1.CreateRangeMapping(Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.RangeMappingCreationInfo{`0})) meetodid, ja loob [ShardLocation](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardlocation.shardlocation.aspx#M:Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.ShardLocation.) klassi eksemplar. Allpool valimis **sample_shard_2** ja kõik vajalikud skeemi objektide sees andmebaas on loodud hoida vahemiku [300, 400).  

    // sm is a RangeShardMap object.
    // Add a new shard to hold the range being added. 
    Shard shard2 = null; 

    if (!sm.TryGetShard(new ShardLocation(shardServer, "sample_shard_2"),out shard2)) 
    { 
        shard2 = sm.CreateShard(new ShardLocation(shardServer, "sample_shard_2"));  
    } 

    // Create the mapping and associate it with the new shard 
    sm.CreateRangeMapping(new RangeMappingCreationInfo<long> 
                            (new Range<long>(300, 400), shard2, MappingStatus.Online)); 


Teise võimalusena saate luua uue Kildu kaardi Manager PowerShelli. Näide on saadaval [siin](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).
## <a name="to-add-a-shard-for-an-empty-part-of-an-existing-range"></a>Lisada Kildu, tühja ala olemasoleva vahemiku jaoks  

Teatud juhtudel võib teil on juba vastendatud vahemik on Kildu ja osaliselt täidetud andmeid, kuid soovite nüüd eelseisvad andmed erinevate Kildu suunata. Näiteks saate Kildu päeva vahemiku ja on juba eraldatud 50 päeva on Kildu, kuid päeval 24 soovite tulevaste andmed maa eri Kildu. Elastne andmebaasi [tükeldamine ühendamine tööriista](sql-database-elastic-scale-overview-split-and-merge.md) saab seda toimingut, kuid kui andmete liikumine ei ole vaja (nt päevade [25, 50 vahemiku andmed), st, päeva 25-50 välistavad, kaasa arvatud pole veel olemas) saate teha seda täielikult abil Kildu kaardi halduse API-de otse.

### <a name="example-splitting-a-range-and-assigning-the-empty-portion-to-a-newly-added-shard"></a>Näide: tükeldamise vahemiku ja äsja lisatud Kildu tühja osa määramine

"Sample_shard_2" ja kõik vajalikud skeemi objektide sees andmebaas on loodud.  

 
    // sm is a RangeShardMap object.
    // Add a new shard to hold the range we will move 
    Shard shard2 = null; 

    if (!sm.TryGetShard(new ShardLocation(shardServer, "sample_shard_2"),out shard2)) 
    { 
    
        shard2 = sm.CreateShard(new ShardLocation(shardServer, "sample_shard_2"));  
    } 

    // Split the Range holding Key 25 

    sm.SplitMapping(sm.GetMappingForKey(25), 25); 

    // Map new range holding [25-50) to different shard: 
    // first take existing mapping offline 
    sm.MarkMappingOffline(sm.GetMappingForKey(25)); 
    // now map while offline to a different shard and take online 
    RangeMappingUpdate upd = new RangeMappingUpdate(); 
    upd.Shard = shard2; 
    sm.MarkMappingOnline(sm.UpdateMapping(sm.GetMappingForKey(25), upd)); 

**Tähtis**: Kasutage seda meetodit ainult juhul, kui olete kindel, et vahemiku jaoks värskendatud vastenduse on tühi.  Meetoditega kontrolli andmed vahemiku teisaldatud, seega on kõige sisaldada kontrolli koodi.  Kui read on olemas ei teisaldataks vahemikus, tegelike andmete jaotuse värskendatud Kildu kaarti ei sobi. [Tükeldatud ühendamine tööriista](sql-database-elastic-scale-overview-split-and-merge.md) kasutamine hoopis sellisel juhul toimingu sooritamiseks.  


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]
 
