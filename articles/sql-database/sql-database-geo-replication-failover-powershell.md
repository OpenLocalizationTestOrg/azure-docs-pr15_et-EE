<properties 
    pageTitle="Kontaktiga kavandatud või planeerimata Tõrkesiirde Azure SQL-i andmebaasi PowerShelliga | Microsoft Azure'i" 
    description="Kontaktiga kavandatud või planeerimata Tõrkesiirde Azure SQL-i andmebaasi PowerShelli abil" 
    services="sql-database" 
    documentationCenter="" 
    authors="stevestein" 
    manager="jhubbard" 
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="powershell"
    ms.workload="data-management" 
    ms.date="08/29/2016"
    ms.author="sstein"/>

# <a name="initiate-a-planned-or-unplanned-failover-for-azure-sql-database-with-powershell"></a>Plaanitud või planeerimata Tõrkesiirde kontaktiga Azure SQL-i andmebaasi PowerShelli abil



> [AZURE.SELECTOR]
- [Azure'i portaal](sql-database-geo-replication-failover-portal.md)
- [PowerShelli](sql-database-geo-replication-failover-powershell.md)
- [T-SQL-IS](sql-database-geo-replication-failover-transact-sql.md)


Selles artiklis kirjeldatakse, kuidas algatada kavandatud või planeerimata Tõrkesiirde SQL-i andmebaasi PowerShelli abil. Konfigureerimiseks Geo-Dispersioonanalüüs, lugege teemat [Konfigureerimine Geo-Dispersioonanalüüs Azure SQL-i andmebaasi](sql-database-geo-replication-powershell.md).



## <a name="initiate-a-planned-failover"></a>Kontaktiga kavandatud Tõrkesiirde

Kasutada cmdlet-käsu **Set-AzureRmSqlDatabaseSecondary** koos parameetriga **– Tõrkesiirde** edendada teise andmebaasi saada esmane uue andmebaasi, demoting olemasoleva esmase, teisese saada. See funktsioon on mõeldud kavandatud Tõrkesiirde, näiteks ajal katastroofi taastamine harjutused, ja nõuab esmane andmebaas kättesaadav.

Käsu sooritab järgmised töövoog.

1. Ajutiselt lülitumine dispersioonanalüüs sünkroonse. See võib põhjustada kõigi tasumata tehingute teisese tühjendatud.

2. Geo-Dispersioonanalüüs seos rollid kaks andmebaasi aktiveerimine  

Järjestusel tagab, et kaks andmebaasi sünkroonitakse aktiveerige rollid ja seetõttu tekib kaotamata andmeid. On lühikese aja jooksul nii andmebaasid on saadaval (0 – 25 sekundites) tellimusel ajal rollid on sisse lülitatud. Vähem kui minutiga lõpuleviimiseks tavaliselt tuleks kogu toiminguid teha. Lisateavet leiate teemast [Set-AzureRmSqlDatabaseSecondary](https://msdn.microsoft.com/library/mt619393.aspx).




Selle cmdlet-käsk tagastab kui, kui teise andmebaasi esmane lõpule.

Järgmine käsk aktiveerib nimega "mydb" klõpsake serveri "srv2" ressursi rühma "rg2" abil esmane jaotises andmebaasi rollid. Algse esmane, millele on ühendatud "db2" lülitub teisene pärast kaks andmebaasid on täielikult sünkroonitud.

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" –ResourceGroupName "rg2” –ServerName "srv2”
    $database | Set-AzureRmSqlDatabaseSecondary -Failover


> [AZURE.NOTE] Harva on võimalik, et toimingut ei saa lõpule viia, ja võidakse kuvada ei reageeri. Sel juhul saate kasutaja kõne Jõusta Tõrkesiirde käsk (planeerimata Tõrkesiirde) ja nõustuge andmekao.


## <a name="initiate-an-unplanned-failover-from-the-primary-database-to-the-secondary-database"></a>Kontaktiga planeerimata Tõrkesiirde esmane andmebaasist teise andmebaasi


Saate cmdlet **Set-AzureRmSqlDatabaseSecondary** **– Tõrkesiirde** ja **- AllowDataLoss** parameetritega edendada teise andmebaasi saada uue esmane andmebaas on planeerimata mood sunnib madaldamine olemasoleva esmase, teisese saada ajal, kui esmane andmebaas pole enam saadaval.

See funktsioon on mõeldud avariitaastet kui äärmiselt oluline on kättesaadavus andmebaasi taastamine ja mõned andmekao on aktsepteeritav. Jõustatud Tõrkesiirde kutsumisel, määratud teise andmebaasi kohe muutub esmane andmebaas ja algab vastu võtmist kirjutamine tehingud. Kui esmane algse andmebaasi saab taastada see peamine uus andmebaas pärast jõustatud Tõrkesiirde toimingut, on varundamist tehtud esmane algse andmebaasi ja vana esmane andmebaasi tehakse teisel andmebaasi esmane uue andmebaasi; seejärel on üksnes uue esmane koopia.

Kuid kuna punkti sisse aja taastamine ei toeta teisene andmebaaside, kui soovite andmete taastamist pühendunud vana esmane andmebaasi, mis oli pole kopeeritud esmane uue andmebaasi, mida peaks tegema CSS-i andmebaasi taastamine varukoopia teadaolevad log.

> [AZURE.NOTE] Kui käsk on välja, kui nii põhi- ja kas võrgus vana primaarne muutub uue teisese kohe, ilma andmete sünkroonimine. Kui esmane on toime tehingud käsk käivitati mõned Andmekao võivad ilmneda.


Kui esmane andmebaasil on mitu sekundaaride käsu osaliselt õnnestuda. Mis käsk oli täidetud teisese muutub esmane. Vana primaarne kuid jääb esmane, st kaks primaries lõpuks olekusse ja ühendatud peatatud dispersioonanalüüs link. Kasutaja peab käsitsi parandada selle konfiguratsiooni "Eemalda teisene" API kasutamine ühte neist esmane andmebaasid.


Järgmine käsk aktiveerib rollid andmebaas nimega "mydb", et esmane esmast pole saadaval. Algne esmane, millele on seotud "mydb" lülitub teisene pärast võrguühenduse taastamisel. Sel hetkel sünkroonimine võib kaasa tuua andmekao.

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb" –ResourceGroupName "rg2” –ServerName "srv2”
    $database | Set-AzureRmSqlDatabaseSecondary –Failover -AllowDataLoss




## <a name="next-steps"></a>Järgmised sammud   

- Pärast Tõrkesiirde, veenduge, et autentimise nõuded oma server ja andmebaas on konfigureeritud uue esmase. Lisateavet leiate teemast [SQL-andmebaasi turvalisuse pärast katastroofiabi](sql-database-geo-replication-security-config.md).
- Pärast katastroofi abil aktiivse Geo-kopeerimine, sh eelnevalt taastamisel ja postitada taastamise juhiseid ja läbimiseks katastroofi taastamine drill, uurige [Katastroofi taastamine Trelle](sql-database-disaster-recovery.md)
- Sasha Nosov ajaveebipostituse kohta aktiivse Geo-kopeerimine, lugege teemat [põhjalikult Geo-Dispersioonanalüüs uued võimalused](https://azure.microsoft.com/blog/spotlight-on-new-capabilities-of-azure-sql-database-geo-replication/)
- Pilv rakendusi kasutada aktiivse Geo-kopeerimine loomise kohta leiate teemast [kujundamisel pilve taotlused järjepidevuse Geo-kopeerimise abil](sql-database-designing-cloud-solutions-for-disaster-recovery.md)
- Elastne andmebaasi kaustu aktiivse Geo-kopeerimine kasutamise kohta leiate artiklist [elastne rakenduskausta katastroofi taastamine strateegiad](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).
- Business continurity ülevaate leiate teemast [Business järjepidevuse ülevaade](sql-database-business-continuity.md)
