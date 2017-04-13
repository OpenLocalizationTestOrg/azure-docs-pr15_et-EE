<properties
    pageTitle="Päringu üle erinevate skeemi cloud andmebaasid | Microsoft Azure'i"
    description="Kuidas häälestada rist andmebaasipäringud vertikaalne sektsioonide üle"    
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

# <a name="query-across-cloud-databases-with-different-schemas-preview"></a>Päringu tegemine cloud andmebaasid eri skeemide (eelvaade)

![Päringu kõigis tabelites erinevad andmebaaside][1]

Andmebaaside vertikaalselt liigendatud kasutada erinevaid tabelite erinevate andmebaasid. See tähendab, et skeemi erineb eri andmebaasid. Näiteks kõigi tabelite jaoks varude on ühe andmebaasi samas kõik raamatupidamine seotud tabelid on teine andmebaas. 

## <a name="prerequisites"></a>Eeltingimused

* Kasutaja peab olema õigus muuta kõik välise ANDMEALLIKA. See õigus on kaasatud, kellel on õigus muuta andmebaasi.
* Muuda kõik välise ANDMEALLIKA õigusi on vaja andmevoo viidata.

## <a name="overview"></a>Ülevaade

**Märkus**: erinevalt koos horisontaalne eraldamine DDL-lausete ei sõltu määratlemine andmete taseme Kildu kaart kaudu elastne andmebaasi kliendi teek.

1. [LOOGE JUHTSLAIDI VÕTI](https://msdn.microsoft.com/library/ms174382.aspx)
2. [LUUA ANDMEBAASI, KUS OTSINGUULATUSEKS ON MÄÄRATUD MANDAATI](https://msdn.microsoft.com/library/mt270260.aspx)
3. [VÄLISE ANDMEALLIKA LOOMINE](https://msdn.microsoft.com/library/dn935022.aspx)
4. [VÄLISE TABELI LOOMINE](https://msdn.microsoft.com/library/dn935021.aspx) 


## <a name="create-database-scoped-master-key-and-credentials"></a>Andmebaasi, kus otsinguulatuseks on määratud juhtslaidi võti ja mandaadi loomine 

Mandaadi kasutatakse elastne päringu oma serveri andmebaasiga ühendust.  

    CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'password';
    CREATE DATABASE SCOPED CREDENTIAL <credential_name>  WITH IDENTITY = '<username>',  
    SECRET = '<password>'
    [;]
 
**Märkus**    Tagada, et selle *<username>* ei sisalda *"@servername"* järelliide. 

## <a name="create-external-data-sources"></a>Väliste andmeallikate loomine

Süntaks:

    <External_Data_Source> ::=
    CREATE EXTERNAL DATA SOURCE <data_source_name> WITH 
               (TYPE = RDBMS,
                LOCATION = ’<fully_qualified_server_name>’,
                DATABASE_NAME = ‘<remote_database_name>’,  
                CREDENTIAL = <credential_name> 
                ) [;] 

**Oluliste**   Parameetri tüüp peab olema seatud **RDBMS**. 

### <a name="example"></a>Näide 

Järgmine näide illustreerib kasutamine lause loomine väliste andmeallikatega. 

    CREATE EXTERNAL DATA SOURCE RemoteReferenceData 
    WITH 
    ( 
        TYPE=RDBMS, 
        LOCATION='myserver.database.windows.net', 
        DATABASE_NAME='ReferenceData', 
        CREDENTIAL= SqlUser 
    ); 
 
Praeguse väliste andmeallikate loendi toomiseks: 

    select * from sys.external_data_sources; 

### <a name="external-tables"></a>Välise tabeleid 

Süntaks:

    CREATE EXTERNAL TABLE [ database_name . [ schema_name ] . | schema_name . ] table_name  
    ( { <column_definition> } [ ,...n ])     
    { WITH ( <rdbms_external_table_options> ) } 
    )[;] 
    
    <rdbms_external_table_options> ::= 
      DATA_SOURCE = <External_Data_Source>, 
      [ SCHEMA_NAME = N'nonescaped_schema_name',] 
      [ OBJECT_NAME = N'nonescaped_object_name',] 

### <a name="example"></a>Näide  

    CREATE EXTERNAL TABLE [dbo].[customer]( 
        [c_id] int NOT NULL, 
        [c_firstname] nvarchar(256) NULL, 
        [c_lastname] nvarchar(256) NOT NULL, 
        [street] nvarchar(256) NOT NULL, 
        [city] nvarchar(256) NOT NULL, 
        [state] nvarchar(20) NULL, 
        [country] nvarchar(50) NOT NULL, 
    ) 
    WITH 
    ( 
           DATA_SOURCE = RemoteReferenceData 
    ); 

Järgmises näites kirjeldatakse, kuidas välise tabeleid loendi toomiseks praegune andmebaas. 

    select * from sys.external_tables; 

### <a name="remarks"></a>Märkused

Elastne query laiendab olemasoleva välise tabeli süntaks määratleda välise tabeleid, mis kasutavad tüüpi RDBMS väliste andmeallikatega. Vertikaalne jagamine mõne välise tabeli määratlus hõlmab järgmisi aspekte: 

* **Skeemi**: Välistabel DDL määratleb skeemi, mille abil saate oma päringuid. Lähtutud definitsiooni Välistabel skeemiga peab vastama skeemiga tabelite serveri andmebaasi tegelike andmete talletuskoht. 

* **Kaugandmebaas viide**: välise tabeli DDL viitab välisest andmeallikast. Välise andmeallika määrab loogiline serveri nimi ja andmebaasi nimi tegelik tabeli andmete talletuskoht kaugandmebaas. 

Välise andmeallika kasutamine eelmises jaotises kirjeldatud, luua välise tabeleid süntaks on järgmine: 

Klausel DATA_SOURCE määratleb välise tabeli jaoks kasutatava välise andmeallikaga (nt kaugandmebaas vertikaalne eraldamine korral).  

SCHEMA_NAME ja objekti Objekti_nimi klauslitest pakuvad vastendamine välise tabeli määratlus eri skeemi serveri andmebaasis tabeli või mõne muu nimega tabeli vastavalt võimalust. See on kasulik, kui soovite mõne Välistabel määratleda kataloogi vaate või DMV andmebaasi Kaug- või muud kui remote tabeli nimi on juba kasutusel kohalikult.  

Järgmine DDL lause langeb mõne olemasoleva välise tabeli määratlus kohaliku kataloogist. See ei mõjuta kaugandmebaas. 

    DROP EXTERNAL TABLE [ [ schema_name ] . | schema_name. ] table_name[;]  

**Õiguste loomine/DROP VÄLISTABEL**: Välistabel DDL, mida on vaja ka viidata aluseks oleva andmeallika jaoks on vaja muuta kõik välise ANDMEALLIKA õigused.  

## <a name="security-considerations"></a>Turvalisuse alused
Kasutajate juurdepääsu välise tabeli automaatselt juurde pääseda remote aluseks olevatest tabelitest jaotises toodud välise andmeallika määratluse mandaat. Peaks saate hallata hoolikalt välise tabeli juurdepääsu vältimiseks soovimatute õiguste läbi välise andmeallika mandaati. Tavalise SQL-i õigusi saab anda või tühistada juurdepääsu välise tabelisse ainult nii, nagu oleks regulaarne tabeli.  


## <a name="example-querying-vertically-partitioned-databases"></a>Näide: päringute vertikaalselt liigendatud andmebaasid 

Järgmine päring teostab kahe kohaliku tabeli Tellimused ja ridade ja remote tabeli vahel ühenduse kolm viis klientide. See on viide andmete kasutamine puhul elastne päringu näide: 

    SELECT      
     c_id as customer,
     c_lastname as customer_name,
     count(*) as cnt_orderline, 
     max(ol_quantity) as max_quantity,
     avg(ol_amount) as avg_amount,
     min(ol_delivery_d) as min_deliv_date
    FROM customer 
    JOIN orders 
    ON c_id = o_c_id
    JOIN  order_line 
    ON o_id = ol_o_id and o_c_id = ol_c_id
    WHERE c_id = 100


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

Tavaline SQL serveri ühendusstringi abil saate BI ja andmete kataloogiintegreerimise tööriistad ühenduse andmebaaside SQL DB server, mis on lubatud elastne päringu ja määratletud välise tabeleid. Veenduge, et teie tööriista andmeallikana on toetatud SQL serveri. Seejärel viidata elastne päringu andmebaasi ja oma välise tabeleid nii nagu mis tahes SQL Serveri andmebaas, mida soovite ühendada oma tööriista. 

## <a name="best-practices"></a>Head tavad 
 
* Veenduge, et elastne päringu lõpp-punkti andmebaasi on antud juurdepääs kaugandmebaas, võimaldades juurdepääsu oma SQL DB tulemüüri konfiguratsiooni Azure'i teenuste jaoks. Ka veenduge, et välise andmeallika määratluse lähtutud mandaadi edukalt saate sisse logida kaugandmebaas ja õigustega juurdepääsu serveri tabel.  

* Elastne päringu sobib kõige paremini päringute kus saab teha enamik arvutamisel kaugjuhtimise andmebaasid. Tavaliselt saate päringu parimaid tulemusi koos valikulise filter predicates, mida saab hinnata remote andmebaasid või ühendused, mis võivad täita täielikult kaugandmebaas. Muud päringu laadimiseks suurte andmehulkade kaugandmebaas vajada ja võib täita halvasti. 


## <a name="next-steps"></a>Järgmised sammud

Päringu horisontaalselt liigendatud andmebaasid (nimetatakse ka sharded andmebaasid) leiate teemast [päringute üle sharded cloud andmebaasid ((horisontaalselt liigendatud))](sql-database-elastic-query-horizontal-partitioning.md).

[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]


<!--Image references-->
[1]: ./media/sql-database-elastic-query-vertical-partitioning/verticalpartitioning.png


<!--anchors-->
