<properties 
    pageTitle="Teegis elastne andmebaasi kliendi identimisteabe haldamine | Microsoft Azure'i" 
    description="Kuidas määrata õige mandaat, ainult lugemiseks, elastne andmebaasi rakenduste administraator" 
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

# <a name="credentials-used-to-access-the-elastic-database-client-library"></a>Mandaadid elastne andmebaasi kliendi teek

[Elastne andmebaasi kliendi teek](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/) kasutab kolm erinevat tüüpi identimisteabe juurdepääsuks [Kildu kaardi haldur](sql-database-elastic-scale-shard-map-management.md). Sõltuvalt sellest, et vajaksite mandaadi abil madalaimate pääsutaseme võimalik.

* **Identimisteabe haldamine**: luua või käsitsemiseks Kildu kaardi haldur. (Vt [sõnastik](sql-database-elastic-scale-glossary.md).) 
* **Accessi identimisteabe**: juurdepääsu olemasoleva Kildu kaardi halduri shards kohta teabe saamiseks.
* **Ühenduse identimisteabe**: shards ühenduse. 

Vt ka [andmebaaside haldamine ja sisselogimise Azure SQL-andmebaasis](sql-database-manage-logins.md). 
 
## <a name="about-management-credentials"></a>Juhtimise mandaadi kohta

Halduse mandaat kasutatakse [**ShardMapManager**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx) objekti rakendusi, mis töödelda Kildu kaartide loomine. (Vt näiteks [lisamine on Kildu abil elastne Andmebaasiriistad](sql-database-elastic-scale-add-a-shard.md) ja [andmed sõltuvad marsruutimine](sql-database-elastic-scale-data-dependent-routing.md)) Elastne skaala kliendi teek kasutaja loob kasutajate SQL-i ja SQL-i sisselogimise ja tagab, et iga antakse lugemis-ja kirjutamisõigusega õiguste globaalne Kildu kaartide andmebaas ja ka kõik Kildu andmebaasid. Neid mandaate säilitada globaalne Kildu kaarti ja kohalik Kildu kaardid, kui Kildu kaardi muudatused on tehtud. Näiteks Kildu kaardi halduri objekti loomiseks kasutada identimisteabe haldamine (kasutades [**GetSqlShardMapManager**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager.aspx): 

    // Obtain a shard map manager. 
    ShardMapManager shardMapManager = ShardMapManagerFactory.GetSqlShardMapManager( 
            smmAdminConnectionString, 
            ShardMapManagerLoadPolicy.Lazy 
    ); 

Muutuv **smmAdminConnectionString** on ühenduse string, mis sisaldab halduse mandaat. Kasutaja ID ja parooliga pakub lugemis-ja kirjutamisõigusega pääsevad Kildu kaartide andmebaas ja üksikute shards. Ühendusstringi haldus hõlmab ka serveri nimi ja andmebaasi nimi globaalne Kildu kaartide andmebaas. Siin on tüüpilised ühendusstringi selleks:

     "Server=<yourserver>.database.windows.net;Database=<yourdatabase>;User ID=<yourmgmtusername>;Password=<yourmgmtpassword>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;” 

Ärge kasutage väärtused kujul "username@server"—instead kasutada lihtsalt "kasutajanimi" väärtus.  See on identimisteabe peab töötama Kildu kaardi halduri andmebaas ja üksikute shards, mis võivad olla erinevad serverites.

## <a name="access-credentials"></a>Accessi identimisteave
  
Loomisel on Kildu kaardi halduri haldamine Kildu kaartide rakendus, kasutage identimisteave, mis on kirjutuskaitstud õigused globaalne Kildu kaardil. Globaalne Kildu kaardi identimisteabe saadud teabele kasutatakse [andmed sõltuvad marsruudi](sql-database-elastic-scale-data-dependent-routing.md) ja kliendi Kildu kaarti vahemälu asustamiseks. Mandaadi pakutakse kaudu sama kõne muster **GetSqlShardMapManager** , nagu eespool näidatud: 

    // Obtain shard map manager. 
    ShardMapManager shardMapManager = ShardMapManagerFactory.GetSqlShardMapManager( 
            smmReadOnlyConnectionString, 
            ShardMapManagerLoadPolicy.Lazy
    );  

Pange tähele **smmReadOnlyConnectionString** kajastamiseks kasutamine eri mandaadi nimel **administraatoriõiguseta** kasutajatele juurdepääsu kasutamine: neid mandaate ei tohiks kirjutusõigusi anda globaalne Kildu kaart. 

## <a name="connection-credentials"></a>Ühenduse identimisteave 

Täiendavad identimisteave on vaja juurde pääseda Kildu, mis on seotud sharding klahvi [**OpenConnectionForKey**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkey.aspx) meetodi kasutamisel. Neid mandaate vaja kohaliku Kildu kaardi tabelite kohta on Kildu elavate kirjutuskaitstud juurdepääsu õigusi. See on vaja teha ühenduse Valideeri andmed sõltuvad marsruutimise kohta on Kildu. See koodilõigu võimaldab andmepääsu andmed sõltuvad marsruutimine raames: 
 
    using (SqlConnection conn = rangeMap.OpenConnectionForKey<int>( 
    targetWarehouse, smmUserConnectionString, ConnectionOptions.Validate)) 

Selles näites on **smmUserConnectionString** ühendusstringi kasutaja mandaati. Azure SQL-i dB, siit tüüpilised ühendusstringi jaoks kasutajatunnust. 

    "User ID=<yourusername>; Password=<youruserpassword>; Trusted_Connection=False; Encrypt=True; Connection Timeout=30;”  

Sarnaselt administraatori identimisteave väärtused kujul "username@server". Selle asemel kasutada lihtsalt "kasutajanimi".  Samuti võtke arvesse, et Ühendusstring ei sisalda serveri nimi ja andmebaasi nimi. On põhjus **OpenConnectionForKey** kõne suunab automaatselt ühendus on õige Kildu põhinevad. Seega ei esitata andmebaasi nimi ja serveri nimi. 

## <a name="see-also"></a>Vt ka
[Andmebaasid ja sisselogimise Azure'i SQL-andmebaasi haldamine](sql-database-manage-logins.md)

[SQL-andmebaasi turvamine](sql-database-security.md)

[Elastne andmebaasi töö alustamine](sql-database-elastic-jobs-getting-started.md)

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]
 