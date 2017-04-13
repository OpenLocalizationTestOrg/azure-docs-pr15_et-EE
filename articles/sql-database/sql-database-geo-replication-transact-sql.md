<properties
    pageTitle="Geo-Dispersioonanalüüs konfigureerimine Transact-SQL Azure'i SQL-andmebaasi | Microsoft Azure'i"
    description="Geo-kopeerimise abil Transact-SQL Azure'i SQL-andmebaasi konfigureerimine"
    services="sql-database"
    documentationCenter=""
    authors="CarlRabeler"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="NA"
    ms.date="10/13/2016"
    ms.author="carlrab"/>

# <a name="configure-geo-replication-for-azure-sql-database-with-transact-sql"></a>Geo-Dispersioonanalüüs konfigureerimine Transact-SQL Azure'i SQL-andmebaas

> [AZURE.SELECTOR]
- [Ülevaade](sql-database-geo-replication-overview.md)
- [Azure'i portaal](sql-database-geo-replication-portal.md)
- [PowerShelli](sql-database-geo-replication-powershell.md)
- [T-SQL-IS](sql-database-geo-replication-transact-sql.md)

Selles artiklis kirjeldatakse, kuidas konfigureerida aktiivse Geo-kopeerimine Transact-SQL Azure'i SQL-andmebaasi.

Kontaktiga Tõrkesiirde Transact-SQL-i abil, lugege teemat [algatada kavandatud või planeerimata Tõrkesiirde Transact - SQL Azure'i SQL-andmebaasi jaoks](sql-database-geo-replication-failover-transact-sql.md).

>[AZURE.NOTE] Aktiivse Geo kopeerimine (loetavad sekundaaride) on nüüd saadaval kogu teenuse astme kõik andmebaasid. Aprillis 2017 pensionile-loetav teisese tüüp ja -loetav olemasolevaid andmebaase uuendatakse automaatselt loetav sekundaaride.

Aktiivse Geo-kopeerimine konfigureerimiseks Transact-SQL-i abil, peate järgmist:

- Azure'i tellimuse.
- Loogika Azure'i SQL-andmebaasi serveri <MyLocalServer> ja SQL-andmebaasi <MyDB> -esmane andmebaas, mida soovite korrata.
- Ühe või mitme loogilise Azure'i SQL-andmebaasi serveri < MySecondaryServer(n) > - loogilise serverid, mis on partneri servereid, mille loote teisene andmebaaside.
- Logi sisse, et on DBManager esmase, on kohalik andmebaas, et teile geo juhendist, db_ownership ja DBManager partneri server, millele tuleb konfigureerida Geo-Dispersioonanalüüs.
- SQL Server Management Studio (SSMS)

> [AZURE.IMPORTANT] On soovitatav alati kasutada uusimat versiooni Management Studio jäävad sünkroonitud värskendusega ning Microsoft Azure'i SQL-andmebaasi. [SQL Server Management Studio värskendada](https://msdn.microsoft.com/library/mt238290.aspx).


## <a name="add-secondary-database"></a>Teisene andmebaasi lisamine

Lause **ALTER andmebaasi** abil saate luua geo kopeeritud teise andmebaasi partneri serveris. See lause execute juhtslaidi järgitav sisaldava andmebaasi serveri andmebaasi. Geo tiražeeritud andmebaasi ("esmane andmebaasi") on sama kopeeritud andmebaasi nimi ja on vaikimisi esmane andmebaas teenuse samal tasemel. Teise andmebaasi saab lugeda või loetav ja saab ühe andmebaasi või mõne elastne databbase. Lisateavet leiate teemast [Muuda andmebaasi (Transact-SQL-i)](https://msdn.microsoft.com/library/mt574871.aspx) ja [Teenuse astme](sql-database-service-tiers.md).
Kui teise andmebaasi loonud ja seemnetega, hakkavad andmete imitatsiooniga asünkroonselt esmane andmebaasist. Alltoodud juhiseid kirjeldatakse, kuidas konfigureerida Geo-Dispersioonanalüüs Management Studio abil. Juhised-loetav ja on loetav sekundaaride ühe andmebaasi või mõne elastne andmebaasi loomiseks on olemas.

> [AZURE.NOTE] Kui sama nimega esmane andmebaas serveris määratud partneri pole andmebaasi käsu ei õnnestu.


### <a name="add-non-readable-secondary-single-database"></a>Lisage-loetav teisese (ühe andmebaas)

Järgmiste juhiste abil saate luua-loetav teisese ühe andmebaasi.

1. Kasutades versiooni 13.0.600.65 või uuem versioon ja SQL Server Management Studio.

     > [AZURE.IMPORTANT] Laadige SQL Server Management Studio [uusima](https://msdn.microsoft.com/library/mt238290.aspx) versiooni. On soovitatav alati kasutada uusimat versiooni Management Studio jäävad sünkroonis Värskenduste Azure portaali.


2. Avage kaust, andmebaaside, laiendage kausta **Süsteemi andmebaasid** , paremklõpsake **juhtslaidi**ja seejärel klõpsake nuppu **Uus päring**.

3. Järgmine lause **Muuda andmebaasi** abil saate teha kohaliku andmebaasi Geo-kopeerimise abil MySecondaryServer1, kus MySecondaryServer1 on teie sõbralik serveri nimi – loetav teisese andmebaasi esmane.

        ALTER DATABASE <MyDB>
           ADD SECONDARY ON SERVER <MySecondaryServer1> WITH (ALLOW_CONNECTIONS = NO);

4. Klõpsake päringu käivitamiseks **käivitamine** .


### <a name="add-readable-secondary-single-database"></a>Lisage loetav teisese (ühe andmebaas)
Järgmiste juhiste abil saate luua loetav teisese ühe andmebaasi.

1. Management Studio ühenduse Azure'i SQL-andmebaasi loogilise serverisse.

2. Avage kaust, andmebaaside, laiendage kausta **Süsteemi andmebaasid** , paremklõpsake **juhtslaidi**ja seejärel klõpsake nuppu **Uus päring**.

3. Järgmine **Muuda andmebaasi** lause abil saate muuta kohaliku andmebaasi Geo-Dispersioonanalüüs esmane loetav teisese andmebaasi teisese serveris.

        ALTER DATABASE <MyDB>
           ADD SECONDARY ON SERVER <MySecondaryServer2> WITH (ALLOW_CONNECTIONS = ALL);

4. Klõpsake päringu käivitamiseks **käivitamine** .



### <a name="add-non-readable-secondary-elastic-database"></a>Lisage-loetav teisese (elastne andmebaas)

Järgmiste juhiste abil saate luua-loetav teisese ka elastne andmebaas nimega.

1. Management Studio ühenduse Azure'i SQL-andmebaasi loogilise serverisse.

2. Avage kaust, andmebaaside, laiendage kausta **Süsteemi andmebaasid** , paremklõpsake **juhtslaidi**ja seejärel klõpsake nuppu **Uus päring**.

3. Järgmine **Muuda andmebaasi** lause abil saate muuta kohaliku andmebaasi koopia Geo esmane-loetav teisese andmebaasi elastne kohapeal on sekundaarne serveri abil.

        ALTER DATABASE <MyDB>
           ADD SECONDARY ON SERVER <MySecondaryServer3> WITH (ALLOW_CONNECTIONS = NO
           , SERVICE_OBJECTIVE = ELASTIC_POOL (name = MyElasticPool1));

4. Klõpsake päringu käivitamiseks **käivitamine** .



### <a name="add-readable-secondary-elastic-database"></a>Lisage loetav teisese (elastne andmebaas)
Järgmiste juhiste abil saate luua loetav teisese ka elastne andmebaas nimega.

1. Management Studio ühenduse Azure'i SQL-andmebaasi loogilise serverisse.

2. Avage kaust, andmebaaside, laiendage kausta **Süsteemi andmebaasid** , paremklõpsake **juhtslaidi**ja seejärel klõpsake nuppu **Uus päring**.

3. Järgmine **Muuda andmebaasi** lause abil saate muuta kohaliku andmebaasi koopia Geo esmane loetav teisese andmebaasi elastne kohapeal on sekundaarne serveri abil.

        ALTER DATABASE <MyDB>
           ADD SECONDARY ON SERVER <MySecondaryServer4> WITH (ALLOW_CONNECTIONS = ALL
           , SERVICE_OBJECTIVE = ELASTIC_POOL (name = MyElasticPool2));

4. Klõpsake päringu käivitamiseks **käivitamine** .



## <a name="remove-secondary-database"></a>Teisene andmebaasi eemaldamine

Lause **ALTER andmebaasi** abil saate kopeerimine teise andmebaasi ja oma esmase partnerlus jäädavalt lõpetada. Selle avaldise juhtslaidi andmebaasidega, kus asub esmane andmebaas. Pärast seose lõpetamine, saab teise andmebaasi tavalises lugemis-ja kirjutamisõigusega andmebaasis. Kui teisel andmebaasiga ühendust on katkenud käsu õnnestub, kuid teisese muutuvad lugemis-ja kirjutamisõigusega, kui ühendus on taastatud. Lisateavet leiate teemast [Muuda andmebaasi (Transact-SQL-i)](https://msdn.microsoft.com/library/mt574871.aspx) ja [Teenuse astme](sql-database-service-tiers.md).

Järgmiste juhiste abil saate geo kopeeritud teisese eemaldamine Geo-Dispersioonanalüüs seos.

1. Management Studio ühenduse Azure'i SQL-andmebaasi loogilise serverisse.

2. Avage kaust, andmebaaside, laiendage kausta **Süsteemi andmebaasid** , paremklõpsake **juhtslaidi**ja seejärel klõpsake nuppu **Uus päring**.

3. Eemaldage geo kopeeritud teisese järgmine **Muuda andmebaasi** lause abil.

        ALTER DATABASE <MyDB>
           REMOVE SECONDARY ON SERVER <MySecondaryServer1>;

4. Klõpsake päringu käivitamiseks **käivitamine** .

## <a name="monitor-geo-replication-configuration-and-health"></a>Geo-Dispersioonanalüüs konfiguratsiooni ja seisundi jälgimine

Jälgimisega seotud ülesannete hulka Geo-Dispersioonanalüüs konfiguratsiooni ja andmete kopeerimine seisund.  Juhtslaidi andmebaasi **sys.dm_geo_replication_links** dünaamilise juhtimise view abil saate tagastada kõik väljumisel dispersioonanalüüs lingid iga andmebaasi teave loogilise Azure'i SQL-andmebaasi server. See vaade sisaldab üht rida iga esmaseid ja teiseseid andmebaaside tiražeerimise seost. **Sys.dm_replication_link_status** dünaamilise juhtimise view abil saate tagastada rea jaoks iga Azure'i SQL-andmebaasi, mille lingi kopeerimine kopeerimine on praegu. See hõlmab nii põhi- ja andmebaasid. Kui rohkem kui üks pidev dispersioonanalüüs link on olemas andmebaasi esmane, see tabel sisaldab rida iga seoseid. Kõik andmebaasid, sh loogilise juhtslaidi luuakse vaade. Siiski tagastab päringute loogilise juhtslaidi see vaade on tühi hulk. **Sys.dm_operation_status** dünaamilise juhtimise view abil saate kuvada kõik andmebaasi toimingud, sh dispersioonanalüüs linkide oleku olek. Lisateabe saamiseks vt [sys.geo_replication_links (Azure'i SQL-andmebaasi)](https://msdn.microsoft.com/library/mt575501.aspx), [sys.dm_geo_replication_link_status (Azure'i SQL-andmebaasi)](https://msdn.microsoft.com/library/mt575504.aspx)ja [sys.dm_operation_status (Azure'i SQL-andmebaasi)](https://msdn.microsoft.com/library/dn270022.aspx).

Järgmiste juhiste abil saate jälgimine Geo-Dispersioonanalüüs seos.

1. Management Studio ühenduse Azure'i SQL-andmebaasi loogilise serverisse.

2. Avage kaust, andmebaaside, laiendage kausta **Süsteemi andmebaasid** , paremklõpsake **juhtslaidi**ja seejärel klõpsake nuppu **Uus päring**.

3. Järgmine lause abil saate kuvada kõik andmebaasid Geo-Dispersioonanalüüs lingid.

        SELECT database_id, start_date, modify_date, partner_server, partner_database, replication_state_desc, role, secondary_allow_connections_desc FROM [sys].geo_replication_links;

4. Klõpsake päringu käivitamiseks **käivitamine** .
5. Avage kaust, andmebaaside, laiendage kausta **Süsteemi andmebaasid** , paremklõpsake **MyDB**ja seejärel klõpsake nuppu **Uus päring**.
6. Järgmine lause abil saate kuvada dispersioonanalüüs jääb ja viimase dispersioonanalüüs minu sekundaarne andmebaaside MyDB aeg.

        SELECT link_guid, partner_server, last_replication, replication_lag_sec FROM sys.dm_geo_replication_link_status

7. Klõpsake päringu käivitamiseks **käivitamine** .
8. Kasutage järgmise geo-dispersioonanalüüs tehtavad toimingud, mis seotud andmebaasi MyDB kuvamiseks.

        SELECT * FROM sys.dm_operation_status where major_resource_id = 'MyDB'
        ORDER BY start_time DESC

9. Klõpsake päringu käivitamiseks **käivitamine** .

## <a name="upgrade-a-non-readable-secondary-to-readable"></a>Loetav teisese versioonile loetav

Aprillis 2017 pensionile-loetav teisese tüüp ja -loetav olemasolevaid andmebaase uuendatakse automaatselt loetav sekundaaride. Kui kasutate juba täna-loetav sekundaaride ja soovite neid loetav uuendada, saate iga teise järgmised lihtsad toimingud.

> [AZURE.IMPORTANT] Ei ole iseteenindusliku kohapealse uuendamise ning et loetav-loetav teisese meetodit. Kui te panete ainult teisene, siis esmane andmebaas jääb kaitsmata kuni uue teisese on täielikult sünkroonitud. Kui teie taotlus SLA nõuab, et esmane on alati kaitstud, kaaluge loomise paralleelselt teisese serveris enne rakendamist versioonitäienduse ülaltoodud juhiseid. Pange tähele, et iga primaarne võib olla kuni 4 teisene andmebaaside.


1. Esmalt *teisene* serveriga ühenduse loomise ja -loetav teisese andmebaasist.  
        
        DROP DATABASE <MyNonReadableSecondaryDB>;

2. Nüüd serveriga *esmane* ja lisage uus loetav teisese

        ALTER DATABASE <MyDB>
            ADD SECONDARY ON SERVER <MySecondaryServer> WITH (ALLOW_CONNECTIONS = ALL);




## <a name="next-steps"></a>Järgmised sammud

- Lisateavet kohta aktiivse Geo-kopeerimine, lugege teemat - [Aktiivse Geo-kopeerimine](sql-database-geo-replication-overview.md)
- Äritegevuse järjepidevuse ülevaade ja stsenaariumid, vaadake teemat [ettevõtetele järjepidevuse ülevaade](sql-database-business-continuity.md)
