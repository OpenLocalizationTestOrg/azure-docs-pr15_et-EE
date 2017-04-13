<properties
    pageTitle="SQL Azure'i andmebaasi ressursi piirangud"
    description="Sellel lehel kirjeldatakse mõnda levinud ressursi piirangud Azure SQL-i andmebaasi."
    services="sql-database"
    documentationCenter="na"
    authors="CarlRabeler"
    manager="jhubbard"
    editor="monicar" />


<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-management"
    ms.date="10/13/2016"
    ms.author="carlrab" />


# <a name="azure-sql-database-resource-limits"></a>Azure'i SQL-andmebaasi ressursi piirangud

## <a name="overview"></a>Ülevaade

Azure'i SQL-andmebaasi haldab abil kahe eri menetlustele andmebaasi ressurssidest: **Ressursside juhtimise** ja **Täitmise, piirangud**. See teema selgitab neid kaks peamist ressursside haldamine.

## <a name="resource-governance"></a>Ressursside juhtimine
Üks Basic, Standard, ja teenuse astme kujundus eesmärke on Azure'i SQL-andmebaasi käitub nagu siis, kui andmebaas töötab oma arvutis täiesti eraldatuna muid andmebaase. Ressursside juhtimise emuleerib seda käitumist. Kui liidetud ressursi kasutamine jõuab maksimaalne saadaval CPU, mälu, Log i/o ja määratud andmebaasi andmete i/o ressursside, ressursside juhtimise päringute täitmise järjekorda ja ressursse järjekorras olevad päringud, nagu nad vabastada.

Nimega sihtotstarbeline arvutisse, kasutades kõigi võimaluste tulemuseks enam täitmise täitva praegu päringud, mis võib põhjustada käsk ajalõpud klientarvutis. Rakenduste agressiivne uuesti loogika ja rakendused, mis käivitada andmebaasi kõrge sagedusega päringuid saate ilmneda tõrked sõnumeid käivitada uutele päringutele, kui samaaegseid taotlusi limiit jõudnud.

### <a name="recommendations"></a>Soovitused:
Jälgida ressursi kasutamine kui ka päringute Keskmine vastuse ajad lähenemas andmebaasi maksimaalset kasutamist. Kui tekib kõrgema päringu latentsused üldiselt on kolm võimalust.

1.  Lühendage sissetulevad taotlused andmebaasi ajalõpp ja miili üles taotluste vältimiseks.

2.  Andmebaasi jõudluse kõrgema taseme määramine.

3.  Optimeeri päringute vähendada mõlema päringu ressursi kasutamine. Lisateavet leiate Azure'i SQL-i andmebaasi jõudluse juhiseid artikli jaotisest päringu Tuning/Hinting.

## <a name="enforcement-of-limits"></a>Jõustamise piirangud
Ressursid peale CPU, mälu, Log i/o ja i andmete/o jõustamine keelates uutele päringutele, kui piirangud on teile helistada. [Tõrketeade](sql-database-develop-error-messages.md) , olenevalt piirang, mis on täis saavad kliendid.

Näiteks SQL-andmebaasiga ühenduste arv ning samaaegseid taotlusi, mida saab töödelda arv on piiratud. SQL-andmebaasi võimaldab andmebaas on suurem kui samaaegseid taotlusi toetama ühenduse ühendamise arvu ühenduste arv. Ajal ühendusi, mis on saadaval summa saab hõlpsasti kontrollida rakendus, paralleelselt taotlused on sageli korda raskem prognoosimiseks ja juhtimine. Eriti tippväärtus laadimise ajal kui piirab ühte saadab liiga palju taotlusi või andmebaasi jõuab selle ressursi ja käivitab tekkimise üles töötaja teemad tõttu enam töötava päringud tõrked võivad ette tulla.

## <a name="service-tiers-and-performance-levels"></a>Teenuse astme ja jõudluse tasemed

On teenuse astme ja tulemuslikkuse tasemeid nii autonoomse andmebaas ja elastne kaustu.

### <a name="standalone-databases"></a>Autonoomse andmebaasid

Autonoomse andmebaasi, andmebaasi piirangud on määratletud andmebaasi teenuse taseme- ja jõudluse tase. Järgmises tabelis kirjeldatakse omaduste Basic, Standard ja Premium andmebaasides jõudluse tase.

[AZURE.INCLUDE [SQL DB service tiers table](../../includes/sql-database-service-tiers-table.md)]

### <a name="elastic-pools"></a>Elastne kaustu

[Elastne kaustu](sql-database-elastic-pool.md) jagada õppematerjale kogu kogumi andmebaasid. Järgmises tabelis kirjeldatakse omaduste Basic, Standard ja Premium elastne andmebaasi kaustu.

[AZURE.INCLUDE [SQL DB service tiers table for elastic databases](../../includes/sql-database-service-tiers-table-elastic-db-pools.md)]

Laiendatud määratlus iga ressursi eelmises tabelis loetletud, vt kirjeldustest [teenuse taseme funktsioonid ja piirangud](sql-database-performance-guidance.md#service-tier-capabilities-and-limits). Teenuse astme ülevaate leiate teemast [Azure SQL-i andmebaasi teenuse astme ja jõudlust](sql-database-service-tiers.md).

## <a name="other-sql-database-limits"></a>Muud piirangud SQL-andmebaas

| Ala | Limiit | Kirjeldus |
|---|---|---|
| Andmebaaside kasutamine automatiseeritud ekspordi tellimuse kohta | 10 | Automatiseeritud ekspordi võimaldab teil luua kohandatud ajakava SQL-andmebaase varundada. Lisateavet leiate teemast [SQL andmebaase: tugi automatiseeritud SQL-i andmebaasi ekspordi](http://weblogs.asp.net/scottgu/windows-azure-july-updates-sql-database-traffic-manager-autoscale-virtual-machines).|
| Andmebaasi serveri kohta | Kuni 5000 | Serveri V12 serverites kohta on lubatud kuni 5000 andmebaasid. |  
| DTUs serveri kohta | 45000 | 45000 DTUs on saadaval serveri V12 serverites ettevalmistamise andmebaase, elastne kaustu ja andmete ladu kohta. |



## <a name="resources"></a>Ressursid

[Azure'i tellimus ja teenuste piirangud, kvoote ja piiranguid](../azure-subscription-service-limits.md)

[SQL Azure'i andmebaasi teenuse astme ja jõudluse tasemed](sql-database-service-tiers.md)

[SQL-andmebaasi Klientprogrammide tõrketeated](sql-database-develop-error-messages.md)
