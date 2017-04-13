<properties
    pageTitle="SQL-andmebaasi elastne rakenduskausta hind ja jõudlus"
    description="Teavet teatud elastne andmebaasi kaustu."
    services="sql-database"
    documentationCenter=""
    authors="srinia"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="05/27/2016"
    ms.author="srinia"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="elastic-database-pool-billing-and-pricing-information"></a>Elastne andmebaasi rakenduskausta arveldus- ja hinnad teave

Elastne andmebaasi kaustu eest tuleb tasuda kohta järgmised omadused:

- Kohapeal on elastne on arve isegi siis, kui ükski andmebaas on kogumi loomisest.
- Kohapeal on elastne on arve tunnis. See on sama mõõtmine sagedus nagu jõudluse tasemeid ühe andmebaasid.
- Kui kohapeal on elastne muutmisel uue hulgal eDTUs, siis pool on ei arve alusel uue eDTUS seni, kuni soovitud suuruse muutmise toiming on lõpule jõudnud. See järgib sama mudelit, mis jõudlusega autonoomse andmebaaside muutmine.


- Elastne kohapeal on hind on arv eDTUs kogumi alusel. Elastne kohapeal on hind on sõltumata arvu ja elastne andmebaasidele kasutamine.
- Hind on arvutatud (pool eDTUs arv) x (ühikuhinna eDTU).

Kohapeal on elastne eDTU ühikuhind on suurem kui sama teenuse kiht autonoomse andmebaasi DTU ühikuhind. Lisateavet leiate teemast [SQL-andmebaasi hinnad](https://azure.microsoft.com/pricing/details/sql-database/). 


Astme eDTUs ja teenuste kohta vt [SQL-andmebaasi suvandid ning jõudlust](sql-database-service-tiers.md).

## <a name="next-steps"></a>Järgmised sammud

- [Kohapeal on elastne andmebaasi loomine](sql-database-elastic-pool-create-portal.md)
- [Jälgida, hallata ja kohapeal on elastne andmebaasi suurus](sql-database-elastic-pool-manage-portal.md)
- [SQL-andmebaasi suvandid ja jõudluse: mõista, mis on saadaval iga teenuse kiht](sql-database-service-tiers.md)
- [PowerShelli skripti andmebaaside sobib kohapeal on elastne andmebaasi tuvastamine](sql-database-elastic-pool-database-assessment-powershell.md)
