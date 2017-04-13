<properties 
    pageTitle="Üksuse raames elastne andmebaasi kliendi teek abil | Microsoft Azure'i" 
    description="Elastne andmebaasi kliendi teek ja üksuse raames kodeerimise andmebaasi kasutamiseks" 
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
    ms.date="05/27/2016" 
    ms.author="torsteng"/>

# <a name="elastic-database-client-library-with-entity-framework"></a>Elastne andmebaasi kliendi teek koos üksuse raames 
 
Selle dokumendi, kuvatakse muudatused rakenduses üksuse raames, mis on vaja [elastne Andmebaasiriistad](sql-database-elastic-scale-introduction.md). Keskendutakse koostamise [Kildu kaardil haldus](sql-database-elastic-scale-shard-map-management.md) ja üksuse raames **Koodi esimene** lähenemine [andmed sõltuvad marsruutimist](sql-database-elastic-scale-data-dependent-routing.md) . [Esimese koodi uue andmebaasi](http://msdn.microsoft.com/data/jj193542.aspx) jaoks EF õpetuse toimib käivitatud näites selles dokumendis. Selle dokumendi lisatud proovi kood on elastne Andmebaasiriistad Visual Studio koodinäiteid proovide määramine.
  
## <a name="downloading-and-running-the-sample-code"></a>Alla laadida ja käivitada proovi kood
Selles artiklis kood allalaadimiseks tehke järgmist.

* Visual Studio 2012 või uuem versioon on nõutav. 
* Käivitage Visual Studio. 
* Visual Studio, valige pilt -> uus projekt. 
* Liikuge dialoogiboksis "Uus projekt" **Online näidised** **Visual C#** ja tippige "elastne db" paremas ülaservas otsinguväljale.
    
    ![Üksuse raames ja elastne andmebaasi valimi rakendus][1] 

    Valige valimi nimetatakse **Elastne DB tööriistad Azure SQL-juriidilise raamistiku integreerimine**. Pärast vastuvõtmist litsentsi, laadib valimi. 

Valimi käivitamiseks peate looma kolm tühja andmebaasi Azure SQL-andmebaasis:

* Kildu kaardi Manageri andmebaas
* Kildu 1 andmebaas
* Kildu 2 andmebaas

Kui olete loonud andmebaasidele, täitke koht omanike rakenduses **Program.cs** Azure SQL-i DB serverinime, andmebaasi nimed ja mandaadi ühenduse andmebaasid. Koostada lahenduse Visual Studios. Visual Studio alla nõutav Nugeti paketid elastne andmebaasi kliendi teek, üksuse raames ja siirdamiseks viga töötlemise koostamine käigus. Veenduge, et taastamine NuGet-paketid on lubatud teie lahendus. Selle sätte saate lubada paremklõpsates lahenduse Visual Studio Solution Exploreris fail. 

## <a name="entity-framework-workflows"></a>Üksuse Framework töövood 

Üksuse Framework arendajate sõltuvad järgmised neli töövoogude luua rakendusi ja tagada püsimine rakenduse objektide puhul: 

* **Esimese koodi (uus projekt)**: The EF arendaja loob mudel rakenduse koodi ja seejärel EF loob see andmebaas. 
* **Koodi esimene (olemasoleva andmebaasiga)**: arendaja võimaldab EF luua rakenduse koodi mudeli olemasolevast andmebaasist.
* **Mudeli esimene**: arendaja loob mudeli Designeris EF ja EF loob andmebaasi mudel.
* **Andmebaasi esimene**: arendaja kasutab EF instrumentaarium tuletamine mudeli olemasolevast andmebaasist. 

Kõik järgmistest viisidest toetuvad DbContext klassi andmebaasi ühendused ja andmebaasi skeemi rakenduse läbipaistev haldamiseks. Kui me arutada täpsemalt hiljem dokumendi erinevate ehitajatel DbContext base klassi võimaldavad juhtida ühenduse loomine, erinevate üksikasjatasemete andmebaasi eellaadimisel ja -skeemifailid loomine. Probleeme tekkida peamiselt asjaolu, et andmebaasi ühenduse haldamine esitatud EF ristub ühenduse halduse võimaluste andmed sõltuvad marsruutimise liideste esitatud elastne andmebaasi kliendi Raamatukogu. 

## <a name="elastic-database-tools-assumptions"></a>Elastne andmebaasi tööriistad oletused 

Termini määratlused, vt [elastne andmebaasi tööriistad sõnastik](sql-database-elastic-scale-glossary.md).

Elastne andmebaasi kliendi teegiga määratleda sektsioonid rakenduse andmete nimetatakse shardlets. Shardlets on tähistatud sharding klahvi ja teatud andmebaasid on vastendatud. Rakendus võib on nii palju andmebaaside vastavalt vajadusele ja levitamine shardlets esitada piisavalt või antud praeguse business nõuded. Kaardistamine sharding väärtused andmebaasid on talletatud API-de elastne andmebaasi kliendi poolt Kildu kaart. Me kutsume seda võimalust **Kildu kaart Management**või SMM lühikese. Kaardi Kildu toimib ka andmebaasi ühendusi taotlusi, mis viivad sharding klahvi ta. Me viidata seda võimalust nimega **andmed sõltuvad marsruutimist**. 
 
Kildu kaardi halduri kaitseb kasutajate vastuolulised vaated shardlet andmeid, mis võivad tekkida, mis juhtub samaaegseid shardlet haldamise toiminguid (nt andmete ümber üks Kildu teise). Selleks Kildu kaartide haldab kliendi teegi ta rakenduse andmebaasi ühendused. See võimaldab Kildu kaardi funktsioonid jääda automaatselt andmebaasi ühendus, kui Kildu toimingute võivad mõjutada shardlet, mis on loodud ühendus. Seda moodust peab integreerida osa EF's funktsioone, nagu uute ühenduste loomine olemasoleva andmebaasi olemasolu kontrollida. Üldiselt on meie jälgimine, et standard DbContext ehitajatel ainult töö usaldusväärselt jaoks suletud andmebaasi ühendusi, mis saab kloonitud turvaline EF jaoks töö. Selle asemel on elastne andmebaasi kujunduse põhimõte tagada ainult avatud ühendused. Üks võib mõelda sulgemist ühenduse vahendajaks kliendi teek enne selle üleandmine EF DbContext võib selle probleemi lahendada. Siiski sulgedes ühendust ja tuginedes EF selle uuesti avama, üks ülekandmises teegi valideerimise ja järjepidevuse kontrollid. Migratsioon funktsiooni EF, aga kasutab need ühendused aluseks oleva andmebaasi skeemi nii, et rakendus läbipaistev haldamine. Ideaalvariandis mitte soovime säilitada ja kombineerida kõiki neid võimalusi elastne andmebaasi kliendi teek ja EF sama rakenduse. Järgmises jaotises käsitletakse nende atribuutide ja nõuete üksikasjalikumalt. 


## <a name="requirements"></a>Nõuded 

Nii elastne andmebaasi kliendi teek ja üksuse Framework API-de töötamisel soovime säilitada järgmised atribuudid: 

* **Skaala-out**: lisamiseks või eemaldamiseks andmebaaside andmete taseme sharded taotluse vastavalt vajadusele võimsus nõudmistele rakenduse kaudu. See tähendab, et juhtimine üle soovitud loomine ja kustutamine ja andmebaaside kasutamine elastne andmebaasi Kildu vastendamine halduri API-de andmebaasid ja vastenduste shardlets, hallata. 

* **Vormindusühtsuse**: rakendus töötab sharding ja kasutab andmed sõltuvad marsruutimise võimaluste kliendi teek. Hoida andmebaasifailide või vale päringutulemite vältimiseks on ühendused vahendajaks Kildu kaardi halduri kaudu. See säilitab ka valideerimise ja järjepidevuse.
 
* **Koodi esimene**: säilitamise EF isiku koodi esimese paradigma mugavuse. Koodi esimene rakenduse klassid on vastendatud läbipaistev aluseks oleva andmebaasi struktuuri. Rakenduse koodi suhtleb DbSets, et peita enamik aspektid kaasatud aluseks oleva andmebaasi töötlemine.
 
* **Skeemi**: üksuse raames tegeleb algse andmebaasi skeemi loomine ja edaspidised skeemi uuendus migratsioon kaudu. Neid võimalusi säilitades rakenduse kohandamiseks on lihtne nagu andmete areneb. 

Järgmised juhised juhendab nendele nõuetele koodi esimene rakenduste elastne Andmebaasiriistad abil tehke järgmist. 

## <a name="data-dependent-routing-using-ef-dbcontext"></a>Andmed sõltuvad marsruutimine EF DbContext abil 

Andmebaasi ühendusi üksuse raames haldab tavaliselt alaliikide **DbContext**kaudu. Luua nende alaliikide tulenevad **DbContext**. See on teie **DbSets** andmebaasi varundada kogumeid CLR-i objektide rakenduse, et määratleda. Andmed sõltuvad marsruutimine kontekstis selgitame mitu kasulik atribuudid reguleerivad ei pruugi olla muude EF koodi esimese rakenduse stsenaariumid: 

* Andmebaas on juba olemas ja registreerimist elastne andmebaasi Kildu kaarti. 
* Skeemi rakendus on juba kasutusele võetud andmebaasi (allpool). 
* Andmete sõltuv marsruutimise ühendused andmebaasi on vahendajaks Kildu kaarti. 

**DbContexts** integreerida andmed sõltuvad marsruudi skaala välja:

1. Füüsilise andmebaasi ühenduste kaudu elastne andmebaasi kliendi kasutajaliideste juhataja Kildu kaardi loomine 
2. Murra **DbContext** alamklassi seos
3. Läbima ühenduse **DbContext** base klassi tagamaks, et kõik töötlemise EF küljel juhtub ka. 

Seda moodust iseloomustab järgmine kood näide. (See kood on ka kaasnevaid Visual Studio projekt)

    public class ElasticScaleContext<T> : DbContext
    {
    public DbSet<Blog> Blogs { get; set; }
    …

        // C'tor for data dependent routing. This call will open a validated connection 
        // routed to the proper shard by the shard map manager. 
        // Note that the base class c'tor call will fail for an open connection
        // if migrations need to be done and SQL credentials are used. This is the reason for the 
        // separation of c'tors into the data-dependent routing case (this c'tor) and the internal c'tor for new shards.
        public ElasticScaleContext(ShardMap shardMap, T shardingKey, string connectionStr)
            : base(CreateDDRConnection(shardMap, shardingKey, connectionStr), 
            true /* contextOwnsConnection */)
        {
        }

        // Only static methods are allowed in calls into base class c'tors.
        private static DbConnection CreateDDRConnection(
        ShardMap shardMap, 
        T shardingKey, 
        string connectionStr)
        {
            // No initialization
            Database.SetInitializer<ElasticScaleContext<T>>(null);

            // Ask shard map to broker a validated connection for the given key
            SqlConnection conn = shardMap.OpenConnectionForKey<T>
                                (shardingKey, connectionStr, ConnectionOptions.Validate);
            return conn;
        }    

## <a name="main-points"></a>Põhipunktid
* Uue ehitaja asendab Vaikekonstruktor rakenduses DbContext alamklassi 
* Uue ehitaja on argumenti, mis on vajalikud andmed sõltuvad marsruutimine elastne andmebaasi kliendi teegi kaudu:
    * Kildu kaardi juurde pääseda andmed sõltuvad marsruutimise liidesed
    * sharding võti shardlet, tuvastamine
    * ühendusstringi andmed sõltuvad marsruutimise ühendamise funktsiooni Kildu identimisteabega. 
 
* Base klassi konstruktori kõne võtab ümbersõit staatiline meetod, mis sooritavad kõigi vajalike toimingute tegemiseks vajalikud andmed sõltuvad marsruutimist. 
   * See kasutab OpenConnectionForKey kõne elastne andmebaasi kliendi liideste kaardil Kildu avatud ühendust luua.
   * Kildu kaarti loob Kildu, mis on antud sharding võti shardlet avatud ühendus.
   * Avatud sellega edastatakse tagasi base klassi konstruktori DbContext näitamaks, et see ühendus on EF asemel et EF automaatselt uue ühenduse loomiseks kasutada. Sellisel viisil ühendust on sildistatud elastne andmebaasi kliendi API nii, et saate tagada järjepidevuse Kildu kaarti haldamise toimingute all.
 
  
Uue ehitaja oma DbContext alamklassi Vaikekonstruktor koodi asemel kasutada. Siin on näide: 

    // Create and save a new blog.

    Console.Write("Enter a name for a new blog: "); 
    var name = Console.ReadLine(); 

    using (var db = new ElasticScaleContext<int>( 
                            sharding.ShardMap,  
                            tenantId1,  
                            connStrBldr.ConnectionString)) 
    { 
        var blog = new Blog { Name = name }; 
        db.Blogs.Add(blog); 
        db.SaveChanges(); 

        // Display all Blogs for tenant 1 
        var query = from b in db.Blogs 
                    orderby b.Name 
                    select b; 
     … 
    }

Uue ehitaja avatakse ühenduse Kildu, mis hoiab andmeid shardlet, mis on tähistatud **tenantid1**väärtus. **Kasutamise** blokeerimise kood jääb muutmata **DbSet** ajaveebide kohta on Kildu EF kasutamise **tenantid1**juurdepääsu. See muudab semantika jaoks koodi funktsiooni abil nii, et kõik andmebaasi toimingud on nüüd rakendatud üks Kildu, kus säilitatakse **tenantid1** blokeerida. Näiteks ainult tulemiks LINQ päringu üle ajaveebide **DbSet** ajaveebide salvestatud praeguse Kildu, kuid mitte muid shards salvestatud need.  

#### <a name="transient-faults-handling"></a>Siirdamiseks vead töötlemise
Microsoft Patterns & tavad meeskond avaldatud [Siirdamiseks viga töötlemise rakenduse Block](https://msdn.microsoft.com/library/dn440719.aspx). Teegi kasutatakse koos EF elastne skaala kliendi teegiga. Siiski tagada siirdamiseks erandite naasmine koht, kus saate tagada, uue ehitaja kasutatakse pärast siirdamiseks viga, et iga uue ühenduse loomise katse on tehtud, kasutades ehitajatel, meil on tweaked. Muul juhul ühenduse õige Kildu ei ole tagatud ja on pole kinnitused ühenduse peetakse muudatuste Kildu kaart. 

Järgmine kood näidis näitab, kuidas saab kasutada SQL-i uuesti poliitika uue **DbContext** alamklassi ehitajatel ümber. 

    SqlDatabaseUtils.SqlRetryPolicy.ExecuteAction(() => 
    { 
        using (var db = new ElasticScaleContext<int>( 
                                sharding.ShardMap,  
                                tenantId1,  
                                connStrBldr.ConnectionString)) 
            { 
                    var blog = new Blog { Name = name }; 
                    db.Blogs.Add(blog); 
                    db.SaveChanges(); 
            … 
            } 
        }); 

**SqlDatabaseUtils.SqlRetryPolicy** ülaltoodud kood on määratletud **SqlDatabaseTransientErrorDetectionStrategy** proovi uuesti arvu 10 ja 5 minutit ootama aega korduste vahel. See lähenemine on sarnane juhised EF ja kasutaja algatatud tehingud (vt [piirangud proovitakse uuesti täitmise strateegiad (alates EF6)](http://msdn.microsoft.com/data/dn307226). Mõlemal juhul nõua, et rakenduse programmi kontrollib ulatus, kus käib ajutine erand: uuesti tehingu või looge (nagu on näidatud) kaudu õige ehitaja konteksti, mis kasutab elastne andmebaasi kliendi teek.

Vajadus kontrollida, kus siirdamiseks erandid võtta meile tagasi ulatus välistab ka sisseehitatud **SqlAzureExecutionStrategy** EF kaasneva kasutamise. **SqlAzureExecutionStrategy** oleks uuesti ühenduse, kuid mitte kasutada **OpenConnectionForKey** ja seetõttu mööduda valideerimine, mis toimub osana **OpenConnectionForKey** kõne. Selle asemel proovi kood kasutab ka kaasas EF sisseehitatud **DefaultExecutionStrategy** . Erinevalt **SqlAzureExecutionStrategy**, see töötab õigesti koos uuesti poliitika kaudu siirdamiseks viga töötlemise. Täitmise poliitika on seatud **ElasticScaleDbConfiguration** klassi. Pange tähele, et otsustasime ei soovi kasutada **DefaultSqlExecutionStrategy** , kuna soovitatakse kasutada **SqlAzureExecutionStrategy** ilmnemisel siirdamiseks erandid – mis põhjustaks vale käitumine kasutajaprofiiliteabe. Lisateavet erinevate proovi uuesti ja EF [Ühenduse paindlikkust elementaarfaili](http://msdn.microsoft.com/data/dn456835.aspx).     

#### <a name="constructor-rewrites"></a>Ehitaja rewrites
Koodi näidetes illustreerimiseks on vaikimisi ehitaja uuesti kirjutab rakenduse andmed sõltuvad marsruutimine üksuse raames kasutamiseks peab. Järgmises tabelis üldistab seda lähenemisviisi muude ehitajatel. 


Praeguse ehitaja  | Transkribeeritakse ehitaja andmete jaoks | Base ehitaja | Märkmete
---------- | ----------- | ------------|----------
MyContext() |ElasticScaleContext (ShardMap, TKey) |DbContext (DbConnection bool) |Ühendus peab olema Kildu kaardi ja andmed sõltuvad marsruutimise võti. Peate kasutama by-pass automaatse ühenduse loomise teel EF, kuid selle asemel kasutada Kildu kaardi tagada ühendus. 
MyContext(string)|ElasticScaleContext (ShardMap, TKey) |DbContext (DbConnection bool) |Ühendus on selle funktsiooni Kildu kaardi ja andmed sõltuvad marsruutimise võti. Fikseeritud andmebaasi nimi või ühenduse stringi nagu nad ei tööta by-pass valideerimine Kildu kaart. 
MyContext(DbCompiledModel) |ElasticScaleContext (ShardMap, TKey, DbCompiledModel) |DbContext (DbConnection, DbCompiledModel, kahendmuutuja) |Ühendus on esitatud antud Kildu kaart ja sharding võti loonud. Kompileeritud mudeli edasi base c'tor.
MyContext (DbConnection bool) |ElasticScaleContext (ShardMap, TKey, kahendmuutuja) |DbContext (DbConnection bool) |Ühendus peab tuletada Kildu kaarti ja võti. Seda ei saa sisendina esitada (v.a juhul, kui selle sisendi oli juba kasutab Kildu kaardi ja klahvi). Funktsiooni kahendmuutuja edasi. 
MyContext (stringi, DbCompiledModel) |ElasticScaleContext (ShardMap, TKey, DbCompiledModel) |DbContext (DbConnection, DbCompiledModel, kahendmuutuja) |Ühendus peab tuletada Kildu kaarti ja võti. Seda ei saa sisendina esitada (v.a juhul, kui selle sisendi kasutas Kildu kaarti ja võti). Kompileeritud mudeli edasi. 
MyContext (ObjectContext bool) |ElasticScaleContext (ShardMap TKey ObjectContext, bool) |DbContext (ObjectContext bool) |Uue ehitaja peab veenduge, et kõik ühenduse ObjectContext, möödunud sisendina on ümber suunatud ühenduse haldab elastne skaala. Üksikasjalik arutelu ObjectContexts on selle dokumendi väljapoole.
MyContext (DbConnection, DbCompiledModel, kahendmuutuja) |ElasticScaleContext (ShardMap TKey DbCompiledModel, bool)| DbContext (DbConnection, DbCompiledModel, kahendmuutuja); |Ühendus peab tuletada Kildu kaarti ja võti. Ühenduse puudub sisendina (v.a juhul, kui selle sisendi oli juba kasutab Kildu kaardi ja klahvi). Mudeli ja kahendmuutuja edastatakse base klassi ehitaja. 

## <a name="shard-schema-deployment-through-ef-migrations"></a>Kildu skeemi juurutamise kaudu EF migratsioon 

Automaatse skeemi on esitatud üksuse raames mugavam. Rakendustes, mis kasutavad elastne Andmebaasiriistad kontekstis, soovime säilitada selles võimalus automaatselt ettevalmistamise skeemi, et vastloodud shards sharded teenuserakenduse andmebaaside lisamisel. Peamine kasutus on andmete taseme sharded rakenduste abil EF läbilaskevõime suurendamiseks. Tuginedes EF kasutaja võimalused skeemi vähendab andmebaasi Administreerimine vaeva tugineb EF sharded rakendusega. 

Skeemi juurutamise kaudu EF migratsioon toimib kõige paremini **avamata ühendused**. See on vastupidiselt stsenaariumi puhul andmed sõltuvad marsruutimine mis sõltub API elastne andmebaasi kliendi poolt avatud ühendus. Teine erinevus on järjepidevuse nõue: samal ajal tagada kõigi marsruutimise ühenduste andmed sõltuvad kaitsta samaaegseid Kildu kaardi stringimanipuleerimisfunktsioonide soovitav, ei ole muret koos algse skeemi juurutamise uue andmebaasiga, mis on veel registreeritud Kildu kaart ja pole veel pikalt shardlets eraldatud. Me saate seetõttu toetuvad tavaline andmebaasi ühendused selles stsenaariumi puhul, erinevalt andmed sõltuvad marsruutimist.  

See viib lähenemisviisi kui skeemi juurutamise kaudu EF migratsioon on kindlalt haagitud selle uue andmebaasi registreerimise on Kildu rakenduse Kildu mapis nimega. See sõltub järgmine kohustuslik tarkvara: 

* Andmebaas on juba loodud. 
* Andmebaas on tühi – tal pole kasutaja skeemi ja kasutaja andmeid.
* Andmebaasi veel puudub elastne andmebaasi kliendi API-de kaudu andmed sõltuvad marsruutimiseks. 

Eeltingimused kohas, saame luua tavalise un avatud **SqlConnection** abil võrgukoosolekuga EF migratsioon skeemi juurutamiseks. Järgmine kood näidis iseloomustab seda moodust. 

        // Enter a new shard - i.e. an empty database - to the shard map, allocate a first tenant to it  
        // and kick off EF intialization of the database to deploy schema 

        public void RegisterNewShard(string server, string database, string connStr, int key) 
        { 

            Shard shard = this.ShardMap.CreateShard(new ShardLocation(server, database)); 

            SqlConnectionStringBuilder connStrBldr = new SqlConnectionStringBuilder(connStr); 
            connStrBldr.DataSource = server; 
            connStrBldr.InitialCatalog = database; 

            // Go into a DbContext to trigger migrations and schema deployment for the new shard. 
            // This requires an un-opened connection. 
            using (var db = new ElasticScaleContext<int>(connStrBldr.ConnectionString)) 
            { 
                // Run a query to engage EF migrations 
                (from b in db.Blogs 
                    select b).Count(); 
            } 

            // Register the mapping of the tenant to the shard in the shard map. 
            // After this step, data-dependent routing on the shard map can be used 

            this.ShardMap.CreatePointMapping(key, shard); 
        } 
 

See näide kirjeldab meetodit **RegisterNewShard** registreerib funktsiooni Kildu Kildu kaardil, kasutab skeemi kaudu EF migratsioon ja talletab kaardistamine on Kildu sharding võti. See tugineb ehitaja **DbContext** alamklassi (**ElasticScaleContext** valimis), mis võtab sisendina ühendusstringi SQL-i. Seda konstruktorit kood on otse edasi, nagu järgmises näites: 


        // C'tor to deploy schema and migrations to a new shard 
        protected internal ElasticScaleContext(string connectionString) 
            : base(SetInitializerForConnection(connectionString)) 
        { 
        } 

        // Only static methods are allowed in calls into base class c'tors 
        private static string SetInitializerForConnection(string connnectionString) 
        { 
            // We want existence checks so that the schema can get deployed 
            Database.SetInitializer<ElasticScaleContext<T>>( 
        new CreateDatabaseIfNotExists<ElasticScaleContext<T>>()); 

            return connnectionString; 
        } 
 
Üks võib kasutada päritud base klassi ehitaja versiooni. Kuid koodi kasutamise jaoks EF vaikimisi initializer loomisel. Seega lühike ümbersõidust staatilise meetodi enne helistaja base klassi ehitaja ühendusstringi abil sisse. Pange tähele, et shards registreerimise peaks töötama erinevate rakenduse domeeni või protsessi, et tagada, et initializer sätete EF ei vastuollu. 


## <a name="limitations"></a>Piirangud 

Võimalused, mis on selles dokumendis toob kaasa mõned piirangud: 

* Kõigepealt tuleb EF rakendusi, mis kasutavad **LocalDb** enne kasutamist elastne andmebaasi kliendi teek tavalise SQL serveri andmebaasi migreerimine. Skaleerimist välja rakenduse kaudu sharding elastne skaala pole võimalik **LocalDb**. Pange tähele, et arengu saate siiski kasutada **LocalDb**. 

* Rakendusse muudatused, mille andmebaasi skeemi Muuda tuleb läbida EF migratsioon kõik shards kohta. Proovi kood selle dokumendi ei näidata, kuidas seda teha. Kaaluge Update andmebaasi ConnectionString parameetri kordamiseks üle kõik shards; või ekstraktida ootel migreerimise abil Update andmebaasi T-SQL-skripti – skripti valik ja rakendada oma shards T-SQL-skript.  

* Antud taotluse, eeldatakse, et kõik selle andmebaasi töötlemiseks sisalduvad ühe Kildu sharding võtme taotluse poolt määratletud. Siiski, selle eeldusel alati hoidke true. Näiteks, kui see pole võimalik sharding klahvi kättesaadavaks. Sellega tegelemiseks pakub kliendi teek **MultiShardQuery** klassi, mis rakendab päringute üle mitme shards ühenduse võtmiseks. Õppekeskuse kasutamine koos EF **MultiShardQuery** väljub selle dokumendi

## <a name="conclusion"></a>Kokkuvõte

Selles dokumendis kirjeldatud juhised, EF rakendusi saate kasutada elastne andmebaasi kliendi teek võimalus andmed sõltuvad marsruutimiseks, refaktooring ehitajatel, **DbContext** alamklasside EF rakenduses kasutatud. See piirab kohad, kus **DbContext** tunnid juba olemas vajalikud muudatused. Lisaks saate EF rakenduste jätkuvalt kasu automaatse skeemi juurutus, kombineerides toiminguid, mis on vajalikud EF migratsioon registreerimine uus shards ja vastenduste Kildu mapis autonoomsest. 


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-use-entity-framework-applications-visual-studio/sample.png
 