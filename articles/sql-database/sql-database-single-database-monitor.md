<properties
    pageTitle="Andmebaasi jõudlust Azure SQL-andmebaasis | Microsoft Azure'i"
    description="Siit saate teada, jälgida oma andmebaasi Azure tööriistad ja dünaamilise halduse vaadete võimaluste kohta."
    keywords="pilveteenuse andmebaasi jõudluse kontrollimise andmebaas"
    services="sql-database"
    documentationCenter=""
    authors="CarlRabeler"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="data-management"
    ms.date="09/27/2016"
    ms.author="carlrab"/>

# <a name="monitoring-database-performance-in-azure-sql-database"></a>Andmebaasi jõudluse Azure'i SQL-andmebaasi jälgimine
SQL Azure'i andmebaasi jõudluse kontrollimise algab ressursi kasutamine võrreldes valite andmebaasi tasemel jälgimise. Jälgimine aitab teil otsustada, kas andmebaasi on liigse või on probleeme kuna ressursid on maxed läbi ja seejärel Otsustage, kas on aeg reguleerimiseks jõudlusega ja oma andmebaasi [teenuse kiht](sql-database-service-tiers.md) . Saate jälgida oma andmebaasi graafiline tööriistade kasutamine [Azure portaali](https://portal.azure.com) või SQL-i [dünaamilise halduse vaadete](https://msdn.microsoft.com/library/ms188754.aspx)kasutamine.

## <a name="monitor-databases-using-the-azure-portal"></a>Azure'i portaalis andmebaaside jälgimine

[Azure'i portaalis](https://portal.azure.com/)saate jälgida ühe andmebaasi kasutamise klõpsate **jälgimis** diagrammi ja oma andmebaasi. Kuvatakse tabeliveergude **meetermõõdustik** aken, kus saate muuta, klõpsates nuppu **Redigeeri diagrammi** . Järgmised mõõdikute lisamiseks tehke järgmist.

- CPU protsent
- DTU protsent
- Andmete IO protsent
- Andmebaasi suurus protsent

Kui olete need mõõdikud lisanud, saate jätkata **jälgimis** diagrammi rohkem üksikasju **meetermõõdustik** akna kuvamine. Kõigi nelja mõõdikute Kuva keskmise kasutamine protsent andmebaasi **DTU** suhtes. Üksikasjalikumat teavet DTUs artiklist [teenuse astme](sql-database-service-tiers.md) .

![Teenuse taseme jälgimine andmebaasi jõudlust.](./media/sql-database-service-tiers/sqldb_service_tier_monitoring.png)

Samuti saate konfigureerida teatiste tulemuslikkuse näitajad. Klõpsake nuppu **Lisa teatise** **meetermõõdustik** aknas. Järgige viisardi abil häälestada oma teatise. Teil on võimalus teatise, kui mõõdikud ületab teatud ülemmäära või kui mõõdiku teatud piirmäärast.

Näiteks kui plaanite oma andmebaasi kasvada töökoormus, saate konfigureerida meiliteatise iga kord, kui andmebaasi jõuab 80% mis tahes jõudluse mõõdikute. Saate kasutada seda varajase hoiatuse aru saada, kui peate aktiveerimiseks järgmise kõrgema taseme jõudlus.

Jõudluse mõõdikute aitab teil otsustada, kui teil on võimalik kasutada madalama taseme jõudlust. Oletame, kasutatakse normaliseeritud S2 andmebaasi ja kõik jõudluse mõõdikute Kuva andmebaasi keskmiselt kasutada rohkem kui 10% igal ajal. On tõenäoline, et andmebaas töötab ka standardse S1. Siiski olema teadlik töökoormus, mis kühvel või enne liikumiseks jõudluse madalama taseme otsustamine müügitehingu muutuda.

## <a name="monitor-databases-using-dmvs"></a>Kuvari andmebaaside DMVs abil

Sama mõõdikute, mis on esitatud portaalis on ka süsteemi vaated läbi: [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx) oma serveri ja kasutajale andmebaasi [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) loogilise **juhtslaidi** andmebaasis. Kasutage **sys.resource_stats** , kui teil on vaja vähem Varundustöö andmete jälgimine üle pikema aja jooksul. Kasutage **sys.dm_db_resource_stats** , kui teil on vaja täpsema andmete väiksema aja jooksul. Lisateavet leiate teemast [Azure SQL-i andmebaasi jõudluse juhiseid](sql-database-performance-guidance.md#monitoring-resource-use-with-sysresourcestats).

>[AZURE.NOTE] **sys.dm_db_resource_stats** tagastab tühja tulemi määramine, kui seda kasutatakse veebi ja väljaande andmebaasidele, mis on kasutuselt kõrvaldatud.

Jaoks elastne andmebaasi kaustu, saate jälgida üksikute andmebaaside kogumi selles jaotises kirjeldatud tehnoloogiaga. Kuid samuti saate jälgida kogu kausta. Lisateavet leiate teemast [jälgimine ja haldamine kohapeal on elastne andmebaasi](sql-database-elastic-pool-manage-portal.md).
