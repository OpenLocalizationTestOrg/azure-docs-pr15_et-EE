<properties 
    pageTitle="SQL Azure'i elastne skaala KKK | Microsoft Azure'i" 
    description="Korduma kippuvad küsimused SQL Azure'i andmebaasi elastne skaala." 
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
    ms.date="05/03/2016" 
    ms.author="ddove"/>

# <a name="elastic-database-tools-faq"></a>Elastne andmebaasi vahendid KKK 

#### <a name="if-i-have-a-single-tenant-per-shard-and-no-sharding-key-how-do-i-populate-the-sharding-key-for-the-schema-info"></a>Kui mul on ühe rentniku Shardi ja sharding võti kohta, kuidas ma asustada sharding võti skeemi teave

Skeemi teave objekti kasutatakse ainult tükeldamiseks Stsenaariumide ühendamine. Kui rakendus on potentsiaalselt ühe rentniku, ei ole vaja tööriista tükeldatud ühendamine ja seega ei ole vaja asustamiseks skeemi teave objekti.

#### <a name="ive-provisioned-a-database-and-i-already-have-a-shard-map-manager-how-do-i-register-this-new-database-as-a-shard"></a>Ma olen ette valmistatud andmebaasi ja mul on juba Kildu kaardi halduri, kuidas registreerida see uus andmebaas on Kildu?

Lugege, **[lisades rakenduse abil elastne andmebaasi kliendi teek on Kildu](sql-database-elastic-scale-add-a-shard.md)**. 

#### <a name="how-much-do-elastic-database-tools-cost"></a>Kui palju maksab elastne Andmebaasiriistad?

Kasutades elastne andmebaasi kliendi teek kanna mis tahes kulusid. Kulude tulla ainult Azure SQL-i andmebaasid shards ja Kildu kaardi halduri kasutatav, samuti saate ettevalmistamise tükeldatud ühendamine tööriista web/töötaja rollid.

#### <a name="why-are-my-credentials-not-working-when-i-add-a-shard-from-a-different-server"></a>Miks minu identimisteave ei tööta eri server on Kildu lisamisel?
Ärge kasutage identimisteabe kujul "kasutaja ID=username@servername”, asemel kasutada lihtsalt" kasutaja ID-d = kasutajanimi ".  Kindlasti ka "kasutajanimi" login on õigused on Kildu.

#### <a name="do-i-need-to-create-a-shard-map-manager-and-populate-shards-every-time-i-start-my-applications"></a>Vaja luua Kildu kaardi Manager ja asustamiseks shards iga kord, kui minu rakendusi käivitada?

Ei – ühekordne toiming on Kildu kaardi Manager (nt **[ShardMapManagerFactory.CreateSqlShardMapManager](http://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.createsqlshardmapmanager.aspx)**) loomine.  Rakenduse kasutada rakenduse käivitamise ajal kõne **[ShardMapManagerFactory.TryGetSqlShardMapManager()](http://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.trygetsqlshardmapmanager.aspx)** .  Ei peaks iga rakenduse domeeni ainult ühe sellise kõne.

#### <a name="i-have-questions-about-using-elastic-database-tools-how-do-i-get-them-answered"></a>Mul on küsimusi elastne Andmebaasiriistad kasutamise kohta, kuidas neile vastata? 

Palun jõuda meile [Azure'i SQL-andmebaasi Foorum](https://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted).

#### <a name="when-i-get-a-database-connection-using-a-sharding-key-i-can-still-query-data-for-other-sharding-keys-on-the-same-shard--is-this-by-design"></a>Kui saamiseks andmebaasi ühendus, kasutades klahvi sharding saan endiselt päringu andmete teiste sama Kildu sharding klahve.  See on kujundus?

Elastne skaala API-de teile õige andmebaasiga ühenduse sharding tootevõti, kuid ei paku sharding võtme filtreerimine.  Lisada päringu ulatuse piiramiseks esitatud sharding võti, **kus** klauslitest vajaduse korral.

#### <a name="can-i-use-a-different-azure-database-edition-for-each-shard-in-my-shard-set"></a>Kas ma saan kasutada Azure'i andmebaasi väljaannet iga Kildu minu Kildu komplekti?

Jah, on Kildu on üksikute andmebaasiga ja seega üks Kildu võiks Premium edition teise olla Standard edition. Lisaks saate soovitud Kildu väljaannet skaala üles või alla mitu korda on Kildu kehtivuse ajal.

#### <a name="does-the-split-merge-tool-provision-or-delete-a-database-during-a-split-or-merge-operation"></a>Tükeldatud Ühenda tööriista säte ei (või kustutada) andmebaasi tükeldamine või Ühenda töötamise ajal? 

Ei. Toiminguteks **tükeldamine** sihtandmebaas peab olemas olema vastav skeemi ja Kildu kaardi Manager registreerida.  **Ühenda** toimingute, peab kustutamine on Kildu Kildu kaardi haldur ja seejärel kustutage andmebaas.

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]
 