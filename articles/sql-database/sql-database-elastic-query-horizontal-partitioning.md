<properties
    pageTitle="Aruande kõikide mastaabitud kontorist pilve andmebaasid | Microsoft Azure'i"
    description="Kuidas häälestada elastne päringute üle horisontaalne sektsioonid"    
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
    ms.author="torsteng" />

# <a name="reporting-across-scaled-out-cloud-databases-preview"></a>Aruande kõikide mastaabitud kontorist pilve andmebaasid (eelvaade)

![Päringu tegemine shards][1]

Sharded andmebaaside jaotada ridade kogu mastaabitud välja andmete taseme. Skeemi on identne kõigi osalevate andmebaase, tuntud ka kui horisontaalne eraldamine. Elastne värskenduspäringu, saate luua aruandeid, ulatuvad sharded andmebaasis kõik andmebaasid.

Lühijuhend, leiate teemast [aruandlusteenuste üle mastaabitud kontorist pilve andmebaasid](sql-database-elastic-query-getting-started.md).

Sharded andmebaase, leiate teemast [päringu üle eri skeemide cloud andmebaasid](sql-database-elastic-query-vertical-partitioning.md). 

 
## <a name="prerequisites"></a>Eeltingimused

* Kasutades elastne andmebaasi kliendi teek Kildu kaardi loomine. lugege teemat [Kildu vastendamine haldus](sql-database-elastic-scale-shard-map-management.md). Või kasutage valimi rakenduse [kasutamise alustamine elastne Andmebaasiriistad](sql-database-elastic-scale-get-started.md).
* Teise võimalusena teemast [migreerimine olemasolevaid andmebaase mastaabitud kontorist andmebaasidele](sql-database-elastic-convert-to-use-elastic-tools.md).
* Kasutaja peab olema õigus muuta kõik välise ANDMEALLIKA. See õigus on kaasatud, kellel on õigus muuta andmebaasi.
* Muuda kõik välise ANDMEALLIKA õigusi on vaja andmevoo viidata.

## <a name="overview"></a>Ülevaade

Need teatised luua oma sharded andmete taseme metaandmete kujutis elastne päringu andmebaasi. 


1. [LOOGE JUHTSLAIDI VÕTI](https://msdn.microsoft.com/library/ms174382.aspx)
2. [LUUA ANDMEBAASI, KUS OTSINGUULATUSEKS ON MÄÄRATUD MANDAATI](https://msdn.microsoft.com/library/mt270260.aspx)
3. [VÄLISE ANDMEALLIKA LOOMINE](https://msdn.microsoft.com/library/dn935022.aspx)
4. [VÄLISE TABELI LOOMINE](https://msdn.microsoft.com/library/dn935021.aspx) 

## <a name="11-create-database-scoped-master-key-and-credentials"></a>1.1 andmebaasi, kus otsinguulatuseks on määratud juhtslaidi võti ja mandaadi loomine 

Mandaadi kasutatakse elastne päringu oma serveri andmebaasiga ühendust.  

    CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'password';
    CREATE DATABASE SCOPED CREDENTIAL <credential_name>  WITH IDENTITY = '<username>',  
    SECRET = '<password>'
    [;]
 
>[AZURE.NOTE] Veenduge, et selle *"\<kasutajanimi\>"* ei sisalda *"@servername"* järelliide. 

## <a name="12-create-external-data-sources"></a>1.2 loomine väliste andmeallikatega

Süntaks:

    <External_Data_Source> ::=    
    CREATE EXTERNAL DATA SOURCE <data_source_name> WITH                                            
            (TYPE = SHARD_MAP_MANAGER,
                    LOCATION = '<fully_qualified_server_name>',
            DATABASE_NAME = ‘<shardmap_database_name>',
            CREDENTIAL = <credential_name>, 
            SHARD_MAP_NAME = ‘<shardmapname>’ 
                   ) [;] 

### <a name="example"></a>Näide 

    CREATE EXTERNAL DATA SOURCE MyExtSrc 
    WITH 
    ( 
        TYPE=SHARD_MAP_MANAGER,
        LOCATION='myserver.database.windows.net', 
        DATABASE_NAME='ShardMapDatabase', 
        CREDENTIAL= SMMUser, 
        SHARD_MAP_NAME='ShardMap' 
    );
 
Praeguse väliste andmeallikate loendi allalaadimine: 

    select * from sys.external_data_sources; 

Välise andmeallika viiteid Kildu kaarti. Elastne päring seejärel kasutab välise andmeallikaga ja aluseks Kildu kaardi loetlemine osalemine andmete taseme andmebaasides. Sama identimisteavet kasutatakse lugeda Kildu kaarti ja juurde pääseda shards andmeid elastne päringu töötlemise ajal. 

## <a name="13-create-external-tables"></a>1.3 välise tabelite loomine 
 
Süntaks:  

    CREATE EXTERNAL TABLE [ database_name . [ schema_name ] . | schema_name. ] table_name  
        ( { <column_definition> } [ ,...n ])     
        { WITH ( <sharded_external_table_options> ) }
    ) [;]  
    
    <sharded_external_table_options> ::= 
      DATA_SOURCE = <External_Data_Source>,       
      [ SCHEMA_NAME = N'nonescaped_schema_name',] 
      [ OBJECT_NAME = N'nonescaped_object_name',] 
      DISTRIBUTION = SHARDED(<sharding_column_name>) | REPLICATED |ROUND_ROBIN

**Näide**

    CREATE EXTERNAL TABLE [dbo].[order_line]( 
         [ol_o_id] int NOT NULL, 
         [ol_d_id] tinyint NOT NULL,
         [ol_w_id] int NOT NULL, 
         [ol_number] tinyint NOT NULL, 
         [ol_i_id] int NOT NULL, 
         [ol_delivery_d] datetime NOT NULL, 
         [ol_amount] smallmoney NOT NULL, 
         [ol_supply_w_id] int NOT NULL, 
         [ol_quantity] smallint NOT NULL, 
         [ol_dist_info] char(24) NOT NULL 
    ) 
    
    WITH 
    ( 
        DATA_SOURCE = MyExtSrc, 
        SCHEMA_NAME = 'orders', 
        OBJECT_NAME = 'order_details', 
        DISTRIBUTION=SHARDED(ol_w_id)
    ); 

Praeguse andmebaasi tuua välise tabeleid loendit: 

    SELECT * from sys.external_tables; 

Loobuda välise tabeleid:

    DROP EXTERNAL TABLE [ database_name . [ schema_name ] . | schema_name. ] table_name[;]

### <a name="remarks"></a>Märkused

ANDMETE\_allika klausel määratleb välise tabeli jaoks kasutatava välise andmeallikaga (Kildu kaarti).  

SKEEMI\_nime ja objekti\_nimi klauslitest välise tabeli määratlus vastendada erinevate skeemi tabelisse. Kui välja jäetud, remote objekti skeemiga on eeldatakse, et "dbo" ja selle nimi on eeldatakse, et olema sama, mis on määratletud välise tabeli nimi. See on kasulik, kui teie serveri tabeli nimi on juba kasutusel andmebaas, kuhu soovite välise tabeli loomine. Näiteks soovite määrata mõne Välistabel kataloogi vaadete liitväärtuse ülevaate saamiseks või DMVs mastaabitud välja andmete taseme. Kuna kataloogi vaadete ja DMVs juba olemas kohalik, ei saa kasutada nende nimed välise tabeli määratlus. Selle asemel kasutada mõni muu nimi ja kataloogi vaate või funktsiooni DMV skeemi nime\_nime ja/või objekti\_nimi klauslitest. (Vt alumist näidet). 

JAOTUSE klausel saate määrata, kasutatakse selle tabeli andmete jaotuse. Päringu protsessor kasutab jaotuse klauslis kõige tõhusam lepingud päringu koostamiseks teave.  

1. **SHARDED** tähendab, et andmed on horisontaalselt liigendatud üle andmebaasid. Andmete jaotuse eraldamine võti on **< sharding_column_name >** parameeter.
2. **Paljundatud** tähendab, et identse koopia tabeli iga andmebaas olemas. See on Teie vastutate selle koopiad on täpselt ühesugused üle andmebaasid.
3. **ROUND\_jaan** tähendab, et tabelis on horisontaalselt liigendatud rakenduse sõltuv jaotuse meetodi abil. 

**Andmete taseme viide**: välise tabeli DDL viitab välisest andmeallikast. Välise andmeallika määrab Kildu kaart, mis annab välise tabeli teavet, mis on vajalik, et leida oma andmete astme kõik andmebaasid. 


### <a name="security-considerations"></a>Turvalisuse alused 

Kasutajate juurdepääsu välise tabeli automaatselt juurde pääseda remote aluseks olevatest tabelitest jaotises toodud välise andmeallika määratluse mandaat. Vältige soovimatute õiguste läbi välise andmeallika mandaati. Kasutada andmine või Tühista mõne Välistabel just nagu oleks tavaline tabel.  

Kui olete määratlenud oma välise andmeallikaga ja oma välise tabeleid, saate kasutada täielikku T-SQL-i nüüd üle oma välise tabeleid.

## <a name="example-querying-horizontal-partitioned-databases"></a>Näide: päringute horisontaalne sektsioonitud andmebaasid 

Järgmine päring sooritab kolm viis Liitu ladu, tellimused ja ridade vahel ja kasutab mitu agregaadid ja valikulise filter. Eeldatakse, et (1) horisontaalne eraldamine (sharding) (2), et ladu, tellimused ja ridade on sharded ladu id veeru alusel ja et elastne päringu saate asukohti ühenduste kohta shards ja töötlemine kallim osa shards paralleelselt päring. 

    select  
         w_id as warehouse,
         o_c_id as customer,
         count(*) as cnt_orderline,
         max(ol_quantity) as max_quantity,
         avg(ol_amount) as avg_amount, 
         min(ol_delivery_d) as min_deliv_date
    from warehouse 
    join orders 
    on w_id = o_w_id
    join order_line 
    on o_id = ol_o_id and o_w_id = ol_w_id 
    where w_id > 100 and w_id < 200 
    group by w_id, o_c_id 
 
## <a name="stored-procedure-for-remote-t-sql-execution-spexecuteremote"></a>Salvestatud protseduur T-SQL serveri täitmiseks: sp\_execute_remote

Elastne päring sisaldab ka salvestatud protseduur, mis pakub otsest juurdepääsu shards. Salvestatud protseduur nimetatakse [sp\_käivitada \_remote](https://msdn.microsoft.com/library/mt703714) ja seda saab kasutada remote andmebaasid remote salvestatud toimingute või T-SQL-koodi käivitada. Kulub järgmisi: 

* Andmeallika nimi (nvarchar): välise andmeallika tüübi RDBMS nime. 
* Päringu (nvarchar): T-SQL päringu täitmise kohta iga Kildu. 
* Parameetri deklareerimise (nvarchar) – valikuline: String andmete tüüpi määratlused parameetreid kasutatakse Päringuparameetri (nt sp_executesql Klõpsake). 
* Parameetri väärtus loendi - valikuline: komaga eraldatud loend parameetrite väärtused (nt sp_executesql Klõpsake).

Sp\_käivitada\_remote kasutab välise andmeallika esitatud kutsumise parameetrid käivitada antud T-SQL-lause remote andmebaasid. Kasutab shardmap halduri andmebaas ja remote andmebaaside ühenduse välise andmeallika mandaati.  

Näide: 

    EXEC sp_execute_remote
        N'MyExtSrc',
        N'select count(w_id) as foo from warehouse' 

## <a name="connectivity-for-tools"></a>Tööriistade Ühenduvus  

Ühenduse rakenduse, tavaline SQL serveri ühendusstringi abil oma BI ja andmete kataloogiintegreerimise tööriistad koos oma Välistabel määratlused andmebaasi. Veenduge, et teie tööriista andmeallikana on toetatud SQL serveri. Seejärel viide elastne päringu andmebaas nagu SQL Serveri andmebaasiga ühendatud tööriista ja kasutage välise tabeleid tööriista või rakenduse nagu oleks tegemist kohaliku tabeleid. 

## <a name="best-practices"></a>Head tavad 

* Veenduge, et elastne päringu lõpp-punkti andmebaas on antud juurdepääs, shardmap andmebaasi ja kõik shards SQL DB tulemüürid kaudu.  

* Kinnitage või Jõusta määratletud välise tabeli andmete jaotuse. Kui teie tegelike andmete jaotuse erineb määratud tabeli definitsiooni jaotuse, võib päringute yield ootamatuid tulemusi. 

* Elastne päringu praegu teha Kildu kõrvaldamine, kui predicates üle sharding võti võimaldab seda turvaliselt jätta teatud shards töötlemine.

* Elastne päring töötab kõige paremini päringuid, kus enamik arvutamisel saab teha shards. Tavaliselt saate päringu parimaid tulemusi koos valikulise filter predicates, mida saab hinnata shards või ühenduste eraldamine klahvid, mida saab teha kõik shards partition joondatud moodus üle. Muud päringu mustrite vajalikud suurte andmehulkade shards laadimiseks pea sõlme ja võib täita halvasti

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-query-horizontal-partitioning/horizontalpartitioning.png
<!--anchors-->
