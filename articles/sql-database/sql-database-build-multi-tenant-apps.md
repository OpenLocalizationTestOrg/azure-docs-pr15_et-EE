<properties
   pageTitle="Azure'i SQL-andmebaasi koostab mitme rentniku rakenduste eraldamise ja tõhusalt"
   description="Siit saate teada, kuidas SQL-andmebaasi koostab mitme rentniku rakendused"
   keywords=""
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
   ms.date="10/13/2016"
   ms.author="carlrab"/>

# <a name="builds-multi-tenant-apps-with-azure-sql-database-with-isolation-and-efficiency"></a>Koostab Azure SQL-andmebaasi eraldamise ja tõhusalt koos mitme rentniku rakendused

## <a name="leverage-elastic-pools-and-build-more-efficient-multi-tenant-apps"></a>Võimendada elastne kaustu ja tõhusam mitme rentniku rakenduste koostamine

Kui olete SaaS arendaja mitme rentniku rakenduse kirjutamine ja töötlemise paljud kliendid, sageli teete kompromissidega kliendi jõudluse, haldamine ja turvalisus. Azure SQL-i andmebaasi elastne andmebaasi kaustu, mille peate enam veenduge, et kompromiss. Neid kaustu, mis aitavad teil hallata ja jälgida mitme rentniku rakendused ja saada eraldamise ühe kliendi andmebaasi eelised. Lugege teemat [mitme rentniku SaaS rakenduste Azure SQL-andmebaasi kujunduse mustrid](sql-database-design-patterns-multi-tenancy-saas-applications.md).

![Koosta-mitme rentniku apps](./media/sql-database-build-multi-tenant-apps/sql-database-build-multi-tenant-apps.png)

## <a name="auto-scaling-you-control"></a>Automaatne skaleerimist reguleerimine

Kaustu automaatselt mastaapida jõudlus ja mälumaht elastne andmebaaside pealt. Saate määrata jõudlus on määratud, lisada või eemaldada elastne andmebaaside nõudmisel ja määratlemine jõudluse elastne andmebaaside mõjutamata kogukulu pool. See tähendab, et te ei pea muretsema üksikute andmebaaside kasutamist haldamise kohta.

[Dokumentide lugemine](sql-database-elastic-pool.md)

## <a name="intelligent-management-of-your-environment"></a>Nutikas keskkonna haldamine

Sisseehitatud sizing soovitused tuvastada aktiivselt andmebaaside kasu kaustu. Soovituste luba "" mõjuanalüüsi kiirülevaate optimeerimine jõudluse eesmärgid täita. RTF-vormingus jõudluse jälgimise ja tõrkeotsingu armatuurlauad aitavad teil visualiseerida ajalooliste kasutamine.

[Dokumentide lugemine](sql-database-elastic-pool-guidance.md)

## <a name="performance-and-price-to-meet-your-needs"></a>Jõudluse ja hinna vastavalt vajadustele

Tavaline, Standard, ja Premium kaustu teile laia jõudlus, salvestusruumi ja hinnakirjad suvandid. Kaustu, võib sisaldada kuni 400 elastne andmebaasid. Elastne andmebaasid saate mastaabi automaatselt kuni 1000 elastne andmebaasi tehingu üksused (eDTU).

[Dokumentide lugemine](https://azure.microsoft.com/pricing/details/sql-database/?b=16.50)

## <a name="elastic-tools"></a>Elastne tööriistad

Lisaks elastne kaustu, on SQL-andmebaasi funktsioone, mis aitavad tegevuse üle mitme andmebaaside haldamine.

**Tehke rist andmebaasipäringud ja teatamine.**  
[Elastne andmebaasi päring](sql-database-elastic-query-overview.md) võimaldab teil töötavad päringut või aruannet kogu olevate elastne andmebaasid ja juurdepääs remote korraga palju andmebaaside oma puhvri talletatud andmed.

**Käivitage rist andmebaasitoiminguid.**  
[Elastne andmebaasitoiminguid](sql-database-elastic-transactions-overview.md) võimaldavad käivitada tehingud, mis hõlmavad mitme andmebaasidega SQL andmebaase ja toiminguid (st kui töötlemine tehingute üle andmebaasid või värskendamisel laoseisu ühe andmebaasi ja tellimused).

**Klõpsake andmebaasidest samu toiminguid käivitada.**  
Haldustoimingud, nt taastamine registrid või skeemide uuendamiseks üle iga andmebaasi olevate elastne [elastne andmebaasi töö](sql-database-elastic-jobs-overview.md) käivitamine

Mida peaksin veel SQL-andmebaasi on pakkuda kuvamiseks avalehele.
[Vaadake seda](https://azure.microsoft.com/services/sql-database/) 

## <a name="next-steps"></a>Järgmised sammud

Saada [tasuta Azure tellimuse](https://azure.microsoft.com/get-started/) ja [esimese Azure SQL-i andmebaasi loomine](sql-database-get-started.md).

## <a name="additional-resources"></a>Lisaressursid

Uurige [võimalusi SQL-andmebaasi](https://azure.microsoft.com/services/sql-database/).
 
Vaadake üle [tehniline ülevaade SQL-andmebaasi](sql-database-technical-overview.md).  
