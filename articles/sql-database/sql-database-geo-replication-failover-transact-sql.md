<properties 
    pageTitle="Kontaktiga kavandatud või planeerimata Tõrkesiirde Transact-SQL Azure'i SQL-andmebaasi jaoks | Microsoft Azure'i" 
    description="Plaanitud või planeerimata Tõrkesiirde kontaktiga Azure SQL-i andmebaasi Transact-SQL-i abil" 
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
    ms.workload="data-management"
    ms.date="08/29/2016"
    ms.author="carlrab"/>

# <a name="initiate-a-planned-or-unplanned-failover-for-azure-sql-database-with-transact-sql"></a>Kontaktiga kavandatud või planeerimata Tõrkesiirde Transact-SQL Azure'i SQL-andmebaasi jaoks


> [AZURE.SELECTOR]
- [Azure'i portaal](sql-database-geo-replication-failover-portal.md)
- [PowerShelli](sql-database-geo-replication-failover-powershell.md)
- [T-SQL-IS](sql-database-geo-replication-failover-transact-sql.md)


Selles artiklis kirjeldatakse, kuidas algatada Tõrkesiirde teisene SQL-andmebaasiga Transact-SQL-i abil. Konfigureerimiseks Geo-Dispersioonanalüüs, lugege teemat [Konfigureerimine Geo-Dispersioonanalüüs Azure SQL-i andmebaasi](sql-database-geo-replication-transact-sql.md).



Tõrkesiirde käivitamiseks peate järgmist:

- Logi sisse, et on DBManager esmase, on kohalik andmebaas, et teile geo juhendist, db_ownership ja DBManager partneri server, millele tuleb konfigureerida Geo-Dispersioonanalüüs.
- SQL Server Management Studio (SSMS)


> [AZURE.IMPORTANT] On soovitatav alati kasutada uusimat versiooni Management Studio jäävad sünkroonitud värskendusega ning Microsoft Azure'i SQL-andmebaasi. [SQL Server Management Studio värskendada](https://msdn.microsoft.com/library/mt238290.aspx).




## <a name="initiate-a-planned-failover-promoting-a-secondary-database-to-become-the-new-primary"></a>Kontaktiga kavandatud Tõrkesiirde edendamine teise andmebaasi saada uut esmast

Lause **ALTER andmebaasi** abil saate edendada teise andmebaasi saada uue esmane andmebaasi mood planeeritud, demoting olemasoleva esmase, teisese saada. Selle avaldise juhtslaidi andmebaasi loogilise Azure'i SQL-andmebaasi server, kus asub geo kopeeritud teisene andmebaas, mida on reklaami. See funktsioon on mõeldud kavandatud Tõrkesiirde, näiteks ajal DR harjutused, ja nõuab esmane andmebaas kättesaadav.

Käsu sooritab järgmised töövoog.

1. Ajutiselt lülitub dispersioonanalüüs sünkroonse, põhjustab kõigi tasumata tehingute teisese tühjendatud ja blokeerivad kõigi uute tehingute;

2. Parameetrid Geo-Dispersioonanalüüs seos kahe andmebaasid rollid.  

Järjestusel tagab, et kaks andmebaasi sünkroonitakse aktiveerige rollid ja seetõttu tekib kaotamata andmeid. On lühikese aja jooksul nii andmebaasid on saadaval (0 – 25 sekundites) tellimusel ajal rollid on sisse lülitatud. Kui esmane andmebaasil on mitu teisene andmebaaside, kuvatakse käsu automaatselt taaskonfigureerimine muude sekundaaride uue esmase ühenduse.  Vähem kui minutiga lõpuleviimiseks tavaliselt tuleks kogu toiminguid teha. Lisateavet leiate teemast [Muuda andmebaasi (Transact-SQL-i)](https://msdn.microsoft.com/library/mt574871.aspx) ja [Teenuse astme](sql-database-service-tiers.md).


Järgmiste juhiste abil saate algatada kavandatud Tõrkesiirde.

1. Ühenduse, kus asub teisel geo tiražeeritud andmebaasi loogilise Azure'i SQL-andmebaasi server Management Studio.

2. Avage kaust, andmebaaside, laiendage kausta **Süsteemi andmebaasid** , paremklõpsake **juhtslaidi**ja seejärel klõpsake nuppu **Uus päring**.

3. Aktiveerige teise andmebaasi esmane roll järgmine **Muuda andmebaasi** lause abil.

        ALTER DATABASE <MyDB> FAILOVER;

4. Klõpsake päringu käivitamiseks **käivitamine** .

>[AZURE.NOTE] Harva, on võimalik, et toimingut ei saa lõpule viia, ja võidakse kuvada ummikus. Sel juhul saate kasutaja Jõusta Tõrkesiirde käsu täitmine ja nõustuge andmekao.


## <a name="initiate-an-unplanned-failover-from-the-primary-database-to-the-secondary-database"></a>Kontaktiga planeerimata Tõrkesiirde esmane andmebaasist teise andmebaasi

Lause **ALTER andmebaasi** abil saate edendada teise andmebaasi saada uue esmane andmebaas on planeerimata mood sunnib madaldamine olemasoleva esmase, teisese saada ajal, kui esmane databse pole enam saadaval. Selle avaldise juhtslaidi andmebaasi loogilise Azure'i SQL-andmebaasi server, kus asub geo kopeeritud teisene andmebaas, mida on reklaami.

See funktsioon on mõeldud avariitaastet kui äärmiselt oluline on kättesaadavus andmebaasi taastamine ja mõned andmekao on aktsepteeritav. Jõustatud Tõrkesiirde kutsumisel, määratud teise andmebaasi kohe muutub esmane andmebaas ja algab vastu võtmist kirjutamine tehingud. Kui esmane algse andmebaasi saab taastada see peamine uue andmebaasi, on varundamist tehtud esmane algse andmebaasi ja vana esmane andmebaasi tehakse teisel andmebaasi esmane uue andmebaasi; Järgnevalt on üksnes sünkroonimise koopia uue esmase.

Kuid kuna punkti sisse aja taastamine ei toeta teisene andmebaaside, kui kasutaja soovib pühendunud vana esmane andmebaas, mis oli enne jõustatud Tõrkesiirde ilmnenud esmane uue andmebaasi kopeeritud andmete taastamine, vajate kasutaja tegeleda tugi see katkes andmeid taastada.

Kui esmane andmebaasil on mitu teisene andmebaaside, kuvatakse käsu automaatselt taaskonfigureerimine muude sekundaaride uue esmase ühenduse.

Järgmiste juhiste abil saate algatada planeerimata Tõrkesiirde.

1. Ühenduse, kus asub teisel geo tiražeeritud andmebaasi loogilise Azure'i SQL-andmebaasi server Management Studio.

2. Avage kaust, andmebaaside, laiendage kausta **Süsteemi andmebaasid** , paremklõpsake **juhtslaidi**ja seejärel klõpsake nuppu **Uus päring**.

3. Aktiveerige teise andmebaasi esmane roll järgmine **Muuda andmebaasi** lause abil.

        ALTER DATABASE <MyDB>   FORCE_FAILOVER_ALLOW_DATA_LOSS;

4. Klõpsake päringu käivitamiseks **käivitamine** .

>[AZURE.NOTE] Kui käsk on välja, kui nii põhi- ja kas võrgus vana primaarne muutub uue teisese kohe, ilma andmete sünkroonimine. Kui esmane on toime tehingud käsk käivitati mõned Andmekao võivad ilmneda.



## <a name="next-steps"></a>Järgmised sammud   

- Pärast Tõrkesiirde, veenduge, et autentimise nõuded oma server ja andmebaas on konfigureeritud uue esmase. Lisateavet leiate teemast [SQL-andmebaasi turvalisuse pärast katastroofiabi](sql-database-geo-replication-security-config.md).
- Pärast katastroofi abil aktiivse Geo-kopeerimine, sh eelnevalt taastamisel ja postitada taastamise juhiseid ja läbimiseks katastroofi taastamine drill, uurige [Katastroofiabi](sql-database-disaster-recovery.md)
- Sasha Nosov ajaveebipostituse kohta aktiivse Geo-kopeerimine, lugege teemat [põhjalikult Geo-Dispersioonanalüüs uued võimalused](https://azure.microsoft.com/blog/spotlight-on-new-capabilities-of-azure-sql-database-geo-replication/)
- Pilv rakendusi kasutada aktiivse Geo-kopeerimine loomise kohta leiate teemast [kujundamisel pilve taotlused järjepidevuse Geo-kopeerimise abil](sql-database-designing-cloud-solutions-for-disaster-recovery.md)
- Elastne andmebaasi kaustu aktiivse Geo-kopeerimine kasutamise kohta leiate artiklist [elastne rakenduskausta katastroofi taastamine strateegiad](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).
- Äri continurity ülevaate leiate teemast [Äri järjepidevuse ülevaade](sql-database-business-continuity.md)
