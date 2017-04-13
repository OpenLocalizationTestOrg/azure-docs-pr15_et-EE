<properties 
    pageTitle="Elastne andmebaasi kliendi teek funktsiooniga Dapper | Microsoft Azure'i" 
    description="Rakendust Dapper elastne andmebaasi kliendi teek." 
    services="sql-database" 
    documentationCenter="" 
    manager="jhubbard" 
    authors="torsteng"/>

<tags 
    ms.service="sql-database" 
    ms.workload="sql-database" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/27/2016" 
    ms.author="torsteng"/>

# <a name="using-elastic-database-client-library-with-dapper"></a>Kasutades Dapper elastne andmebaasi kliendi teek 

See dokument on arendajatele, mis sõltuvad Dapper luua rakendusi, kuid ka taha [instrumentaarium elastne andmebaasi](sql-database-elastic-scale-introduction.md) loomiseks rakendada sharding skaala-out nende andmete taseme rakendused.  Selle dokumendi iseloomustab Dapper põhiseid rakendusi, mis on vajalik elastne Andmebaasiriistad muudatused. Meie liikumine on sisse elastne andmebaasi Kildu haldamine ja andmed sõltuvad marsruutimine Dapper. 

**Proovi kood**: [elastne Andmebaasiriistad Azure SQL-i andmebaasi - Dapper integreerimine](https://code.msdn.microsoft.com/Elastic-Scale-with-Azure-e19fc77f).
 
Azure SQL-i andmebaasi elastne andmebaasi kliendi teek **Dapper** ja **DapperExtensions** integreerimine on lihtne. Rakenduste saab kasutada andmete sõltuvad, muutes loomine ja uue [SqlConnection](http://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) objektide avamise marsruutimine [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) kõne [kliendi teek](http://msdn.microsoft.com/library/azure/dn765902.aspx). See piirab ainult rakenduse muudatuste, kui luuakse uus ühendused ja avada. 

## <a name="dapper-overview"></a>Dapper ülevaade
**Dapper** on soovitud objekti relatsiooniline plaanuri. See kaardid .NET objektide rakendusest relatsiooniandmebaasist (ja vastupidi). Proovi kood esimene osa näitab, kuidas saate integreerida elastne andmebaasi kliendi teek Dapper-põhiste rakendustega. Teine osa proovi kood on näidatud, kuidas integreerida nii Dapper ja DapperExtensions kasutamise korral.  

Plaanuri funktsiooni Dapper annab pikendamise võimalused andmebaasi ühendusi, mis lihtsustada esitamise T-SQL-lausete täitmiseks või andmebaasi päringute kohta. Näiteks Dapper hõlbustab vastendamiseks oma .net-i objektide ja SQL-lauseid **Execute** kõnede parameetrid vahel või kasutamine SQL-päringud tulemuste abil **päringu** kõnede Dapper .NET objektid. 

DapperExtensions kasutamisel te enam ei vaja SQL-lauseid esitada. Laiendid meetodid **GetList** või **lisada** nagu üle andmebaasi ühendus luua SQL-lauseid taustal.
 
Dapper ja ka DapperExtensions on kas rakenduse juhtelemendid andmebaasiga ühenduse loomine. See aitab suhelda elastne andmebaasi kliendi teek, mis maaklerid andmebaasi ühendusi, mis põhineb shardlets andmebaasidele vastendust.

Dapper assemblereid kohta leiate teavet [Dapper dot net](http://www.nuget.org/packages/Dapper/). Dapper laiendid, vt [DapperExtensions](http://www.nuget.org/packages/DapperExtensions).

## <a name="a-quick-look-at-the-elastic-database-client-library"></a>Pilgu elastne andmebaasi kliendi teek

Teegiga elastne andmebaasi kliendi saate määratleda sektsioonid rakenduse andmete nimetatakse *shardlets* , vastendamine andmebaasid ja tuvastada *sharding klahvide*abil. Teil võib olla nii palju andmebaaside kui vaja ning jagada oma shardlets üle andmebaasidele. Kaardistamine sharding väärtused andmebaasid on talletatud teegi API-de esitatud Kildu kaart. Selle funktsiooni nimetatakse **Kildu vastendamine haldus**. Kildu kaardi toimib ka andmebaasi ühendusi taotlusi, mis viivad sharding klahvi ta. See funktsioon on edaspidi **andmed sõltuvad marsruutimist**.

![Kildu kaartide ja andmed sõltuvad marsruutimine][1]

Kildu kaardi halduri kaitseb kasutajate vastuolulised vaated shardlet andmeid, mis võivad tekkida samaaegseid shardlet toimingute juhtub andmebaasid. Selleks on Kildu kaardid ta andmebaasi ühendused rakenduse ehitatud teek. Kui Kildu toimingute võivad mõjutada selle shardlet, see võimaldab Kildu kaardi funktsioonid jääda automaatselt andmebaasi ühendus. 

Traditsiooniline võimalus luua ühendusi Dapper asemel peame [OpenConnectionForKey meetodit](http://msdn.microsoft.com/library/azure/dn824099.aspx)kasutada. See tagab kõigi toimub ja ühendused hallatakse õigesti, kui mis tahes andmed teisaldatakse shards vahel.

### <a name="requirements-for-dapper-integration"></a>Nõuded Dapper integreerimine

Nii elastne andmebaasi kliendi teek ja Dapper API-de töötamisel soovime säilitada järgmised atribuudid:

* **Scaleout**: Soovime lisamiseks või eemaldamiseks andmebaaside andmete taseme sharded taotluse vastavalt vajadusele võimsus nõudmistele rakenduse kaudu. 

-    **Vormindusühtsuse**: Kuna meie rakendus on mastaabitud kasutades sharding, tuleb teha andmed sõltuvad marsruutimist. Soovime andmed sõltuvad marsruutimise võimaluste teegi abil saate teha. Eelkõige soovime säilitada valideerimise ja järjepidevuse tagab esitatud ühendusi, mis on vahendajaks Kildu kaardi halduri kaudu hoida andmebaasifailide või vale päringutulemite vältimiseks. See tagab, et ühenduste antud shardlet on tagasi lükatud või kui selle shardlet praegu teisaldatakse (näiteks) erinevate Kildu, tükeldatud/ühendamine API-de kasutamine on peatatud.

-    **Objekti vastendamise**: Soovime säilitada mugavuse vastendused esitatud Dapper tõlkimiseks rakenduse tunnid ja aluseks oleva andmebaasi struktuuri vahel. 

Järgmises jaotises antakse nendele nõuetele rakendusi **Dapper** ja **DapperExtensions**juhised.

## <a name="technical-guidance"></a>Tehnilised juhendid
### <a name="data-dependent-routing-with-dapper"></a>Andmed sõltuvad Dapper marsruutimine 

Dapper, taotlus on tavaliselt eest loomine ja ühendused aluseks oleva andmebaasi avamisel. Sellist tüüpi T manustab rakendus, Dapper tagastab Päringutulemid nagu .NET saidikogumid tüüpi T. Dapper sooritab vastendust ridadest T-SQL-is tulemi objektide tüüpi T. Samuti Dapper kaardid .NET objektide SQL-i väärtuste või andmete stringimanipuleerimisfunktsioonide keele (piirmäära) laused parameetrid. Dapper pakub seda funktsiooni laiend meetodite tavalise [SqlConnection](http://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) objekti ADO .net-i SQL-i kliendile teekide kaudu. Tagastatud elastne skaala API-de jaoks DDR SQL-i ühendus on ka tavaline [SqlConnection](http://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) objektid. See võimaldab meil otse kasutada Dapper laiendid tagastatud kliendi teek DDR API, tüüp, et see on ka lihtne ühenduse SQL-i kliendile.

Märkused oleks lihtne ühendused vahendajaks elastne andmebaasi kliendi teek Dapper jaoks kasutada.

See koodi (kaudu lisatud valimi) on näide sharding võti osutamise taotlusega teeki tagada õige Kildu ühenduse lähenemisviisi.   

    using (SqlConnection sqlconn = shardingLayer.ShardMap.OpenConnectionForKey(
                     key: tenantId1, 
                     connectionString: connStrBldr.ConnectionString, 
                     options: ConnectionOptions.Validate))
    {
        var blog = new Blog { Name = name };
        sqlconn.Execute(@"
                      INSERT INTO
                            Blog (Name)
                            VALUES (@name)", new { name = blog.Name }
                        );
    }

Kõne suunamiseks [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) API asendab vaikimisi loomine ja avamine ühenduse SQL-i kliendile. [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) kõne on argumenti, mis on vajalikud andmed sõltuvad marsruutimine: 

-    Kildu kaardi juurde pääseda andmed sõltuvad marsruutimise liidesed
-    Sharding võti on shardlet tuvastamine
-    Mandaadi (kasutajanime ja parooli) ühenduse loomiseks on Kildu

Kildu kaardi objekti loob ühenduse Kildu, mis on antud sharding võtme shardlet. Elastne andmebaasi kliendi API-de sildistamine ka rakendada selle järjepidevuse garantiid ühendus. Kuna [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) kõne tagastab tavalise SQL-i kliendile ühenduse objekti, edaspidised kõne **täita** laiend meetod Dapper järgib Dapper tavaks.

Päringute töötavad väga sarnasel viisil – avate esmalt kliendi API [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) kaudu ühendus. Seejärel kasutage tavalise Dapper pikendamise võimalused vastendamiseks SQL-päringu tulemused .NET objektid:

    using (SqlConnection sqlconn = shardingLayer.ShardMap.OpenConnectionForKey(
                    key: tenantId1, 
                    connectionString: connStrBldr.ConnectionString, 
                    options: ConnectionOptions.Validate ))
    {    
           // Display all Blogs for tenant 1
           IEnumerable<Blog> result = sqlconn.Query<Blog>(@"
                                SELECT * 
                                FROM Blog
                                ORDER BY Name");

           Console.WriteLine("All blogs for tenant id {0}:", tenantId1);
           foreach (var item in result)
           {
                Console.WriteLine(item.Name);
            }
    }

Pange tähele, et **abil** blokeerida ühendusega DDR scopes kõik andmebaasi toimingud plokk üks Kildu, kus tenantId1 hoitakse piires. Päring tagastab ainult ajaveebide salvestatud praeguse Kildu, kuid mitte need, mis tahes muud shards salvestatud. 

## <a name="data-dependent-routing-with-dapper-and-dapperextensions"></a>Andmed sõltuvad Dapper ja DapperExtensions marsruutimine

Dapper kaasas täiendavaid laiendid, mis pakub veel mugavuse ja võtmiseks andmebaasist andmebaasi rakenduste ökosüsteemi. DapperExtensions on näide. 

DapperExtensions abil oma rakenduse ei muutu, kuidas andmebaasi ühendused luuakse ja hallatakse. See on ikka rakenduse kohustusi ühendused avamiseks ja tavalise SQL-i kliendile ühenduse objektide oodatakse laiend viisil. Me saate toetuvad [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) eespool kirjeldatud. Järgmine kood nimega näidet kujutavad ainult muudatus on see, et enam meil kirjutada T-SQL-lauseid:

    using (SqlConnection sqlconn = shardingLayer.ShardMap.OpenConnectionForKey(
                    key: tenantId2, 
                    connectionString: connStrBldr.ConnectionString, 
                    options: ConnectionOptions.Validate))
    {
           var blog = new Blog { Name = name2 };
           sqlconn.Insert(blog);
    }

Ja siin on päringu proovi kood: 

    using (SqlConnection sqlconn = shardingLayer.ShardMap.OpenConnectionForKey(
                    key: tenantId2, 
                    connectionString: connStrBldr.ConnectionString, 
                    options: ConnectionOptions.Validate))
    {
           // Display all Blogs for tenant 2
           IEnumerable<Blog> result = sqlconn.GetList<Blog>();
           Console.WriteLine("All blogs for tenant id {0}:", tenantId2);
           foreach (var item in result)
           {
               Console.WriteLine(item.Name);
           }
    }

### <a name="handling-transient-faults"></a>Käsitsemise siirdamiseks vead

Microsoft Patterns & tavad meeskond avaldatud [Siirdamiseks viga töötlemise rakenduse blokeerimine](http://msdn.microsoft.com/library/hh680934.aspx) aidata leevendada levinud siirdamiseks viga tingimused, kui töötab pilveteenuses rakenduste arendajatele. Lisateavet leiate teemast [visadust, kõik Triumphs salajane: abil siirdamiseks viga töötlemise rakenduse blokeerimine](http://msdn.microsoft.com/library/dn440719.aspx).

Proovi kood tugineb siirdamiseks viga teegi siirdamiseks vead kaitsta. 

    SqlDatabaseUtils.SqlRetryPolicy.ExecuteAction(() =>
    {
       using (SqlConnection sqlconn = 
          shardingLayer.ShardMap.OpenConnectionForKey(tenantId2, connStrBldr.ConnectionString, ConnectionOptions.Validate))
          {
              var blog = new Blog { Name = name2 };
              sqlconn.Insert(blog);
          }
    });

**SqlDatabaseUtils.SqlRetryPolicy** ülaltoodud kood on määratletud **SqlDatabaseTransientErrorDetectionStrategy** uuesti arvu 10 ja 5 minutit ootama aega korduste vahel. Kui kasutate tehingud, veenduge, et teie proovi uuesti ulatus ulatub tehingu puhul siirdamiseks viga alguseni.

## <a name="limitations"></a>Piirangud

Võimalused, mis on selles dokumendis toob kaasa mõned piirangud:

* Proovi kood selle dokumendi näidata, kuidas hallata skeemi üle shards.
* Antud taotluse, saame endale ühe Kildu määratud esitatud taotluse sharding võti sisaldub selle andmebaasi töötlemiseks. Siiski, selle eeldusel alati, näiteks kui hoidke ei ole võimalik sharding klahvi kättesaadavaks teha. Sellega tegelemiseks elastne andmebaasi kliendi teek sisaldab [MultiShardQuery klassi](http://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardexception.aspx). Klassi rakendab päringute üle mitme shards ühenduse võtmiseks. Kasutades MultiShardQuery koos Dapper on selle dokumendi väljapoole.

## <a name="conclusion"></a>Kokkuvõte

Rakenduste Dapper ja DapperExtensions abil saate hõlpsalt kasu elastne Andmebaasiriistad Azure'i SQL-andmebaasi. Selles dokumendis kirjeldatud juhised, nende rakenduste kasutada tööriista võimalus andmete sõltuvad, muutes loomine ja uue [SqlConnection](http://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) objektide avamise marsruutimine elastne andmebaasi kliendi teegi [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) kõne. See piirab vajalikud kohad, kus uued ühendused on loodud ja avada rakenduse muudatused. 

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-working-with-dapper/dapperimage1.png
 