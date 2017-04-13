<properties 
    pageTitle="Azure'i SQL-andmebaasi jaoks planeeritud või planeerimata Tõrkesiirde alustamine Azure portaali | Microsoft Azure'i" 
    description="Kontaktiga kavandatud või planeerimata Tõrkesiirde Azure SQL-i andmebaasi Azure'i portaalis" 
    services="sql-database" 
    documentationCenter="" 
    authors="stevestein" 
    manager="jhubbard" 
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="NA"
    ms.workload="data-management" 
    ms.date="08/29/2016"
    ms.author="sstein"/>

# <a name="initiate-a-planned-or-unplanned-failover-for-azure-sql-database-with-the-azure-portal"></a>Kontaktiga kavandatud või planeerimata Tõrkesiirde Azure SQL-i andmebaasi Azure'i portaalis


> [AZURE.SELECTOR]
- [Azure'i portaal](sql-database-geo-replication-failover-portal.md)
- [PowerShelli](sql-database-geo-replication-failover-powershell.md)
- [T-SQL-IS](sql-database-geo-replication-failover-transact-sql.md)


Selles artiklis kirjeldatakse, kuidas algatada Tõrkesiirde [Azure portaali](http://portal.azure.com)teisene SQL-andmebaasiga. Konfigureerimiseks Geo-Dispersioonanalüüs, lugege teemat [Konfigureerimine Geo-Dispersioonanalüüs Azure SQL-i andmebaasi](sql-database-geo-replication-portal.md).


## <a name="initiate-a-failover"></a>Kontaktiga Tõrkesiirde

Teise andmebaasi, saab vahetada esmast saada.  

1. Esmane andmebaasi Geo-Dispersioonanalüüs koostöös [Azure portaali](http://portal.azure.com) Sirvi.
2. Enne SQL-andmebaasiga, valige **Kõik sätted** > **Geo-Dispersioonanalüüs**.
3. Valige loendis **SEKUNDAARIDE** soovite saada uut esmast ja klõpsake nuppu **Tõrkesiirde**andmebaas.

    ![Tõrkesiirde][2]

4. Klõpsake nuppu **Jah** alustamiseks on tõrkesiire.

Käsu lülitub teise andmebaasi üheks esmane roll. 

On lühikese aja jooksul nii andmebaasid on saadaval (0 – 25 sekundites) tellimusel ajal rollid on sisse lülitatud. Kui esmane andmebaasil on mitu teisene andmebaaside, kuvatakse käsu automaatselt taaskonfigureerimine muude sekundaaride uue esmase ühenduse. Vähem kui minutiga lõpuleviimiseks tavaliselt tuleks kogu toiminguid teha. 

>[AZURE.NOTE] Kui esmane on võrgus ja toime tehingud käsu väljaandmisel mõned andmekao võib ilmneda.


## <a name="next-steps"></a>Järgmised sammud   

- Pärast Tõrkesiirde, veenduge, et autentimise nõuded oma server ja andmebaas on konfigureeritud uue esmase. Lisateavet leiate teemast [SQL-andmebaasi turvalisuse pärast katastroofiabi](sql-database-geo-replication-security-config.md).
- Pärast katastroofi abil aktiivse Geo-kopeerimine, sh eelnevalt taastamisel ja postitada taastamise juhiseid ja läbimiseks katastroofi taastamine drill, uurige [Katastroofi taastamine Trelle](sql-database-disaster-recovery.md)
- Sasha Nosov ajaveebipostituse kohta aktiivse Geo-kopeerimine, lugege teemat [põhjalikult Geo-Dispersioonanalüüs uued võimalused](https://azure.microsoft.com/blog/spotlight-on-new-capabilities-of-azure-sql-database-geo-replication/)
- Pilv rakendusi kasutada aktiivse Geo-kopeerimine loomise kohta leiate teemast [kujundamisel pilve taotlused järjepidevuse Geo-kopeerimise abil](sql-database-designing-cloud-solutions-for-disaster-recovery.md)
- Elastne andmebaasi kaustu aktiivse Geo-kopeerimine kasutamise kohta leiate artiklist [elastne rakenduskausta katastroofi taastamine strateegiad](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md).
- Business continurity ülevaate leiate teemast [Business järjepidevuse ülevaade](sql-database-business-continuity.md)




<!--Image references-->
[1]: ./media/sql-database-geo-replication-failover-portal/failover.png
[2]: ./media/sql-database-geo-replication-failover-portal/secondaries.png
