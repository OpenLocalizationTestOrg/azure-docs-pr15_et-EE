<properties
    pageTitle="SQL-andmebaasi jõudlus ja suvandid: teenuse astme | Microsoft Azure'i"
    description="Võrrelge SQL-andmebaasi jõudlus ja äri järjepidevus funktsioonid tasakaalustamiseks maksumuse ja võimalus, kui muudate teenuse astme."
    keywords="andmebaasi jõudluse andmebaasi suvandid"
    services="sql-database"
    documentationCenter=""
    authors="CarlRabeler"
    manager="jhubbard"
    editor="CarlRabeler"/>

<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="data-management"
    ms.date="08/10/2016"
    ms.author="carlrab"/>

# <a name="sql-database-options-and-performance-understand-whats-available-in-each-service-tier"></a>SQL-andmebaasi suvandid ja jõudluse: mõista, mis on saadaval iga teenuse kiht

[Azure'i SQL-andmebaasi](sql-database-technical-overview.md) pakub kolmest teenuse tasemest mitme tulemuslikkuse tasemeid käsitlema erinevate töökoormus. Iga taseme pakub suurenevad kogum, järjest suurem läbilaskevõime mõeldud ressursid. Saate hallata iga andmebaasi, koos eraldi jõudlusega [teenuse kiht](sql-database-service-tiers.md#standalone-database-service-tiers-and-performance-levels) . Saate hallata mitut andmebaasi [elastne rakenduskausta](sql-database-service-tiers.md#elastic-pool-service-tiers-and-performance-in-edtus) ressursid ühiskasutusega kogum. Autonoomse andmebaaside ressursid on väljendatud andmebaasi tehingu ühikute (DTUs) ja elastne DTUs või eDTUs elastne kaustu. DTUs ja eDTUs kohta leiate lisateavet teemast [mis on saanud DTU](sql-database-what-is-a-dtu.md). 

Mõlemal juhul kaasata teenuse astme **Basic**, **Standard**ja **Premium**. Andmebaasi suvandid nende astme sarnanevad autonoomse andmebaasid ja elastne kaustu, kuid on täiendavad kaalutluste kohta elastne kaustu. Selles artiklis antakse üksikasjad teenuse astme autonoomse andmebaasid ja elastne kaustu.

## <a name="service-tiers-and-database-options"></a>Teenuse astme ja andmebaasi suvandid
Tavaline, Standard, ja Premium teenuse astme kõik on mõni sees SLA 99,99% ja hõlpsalt prognoosida jõudluse, paindlik business järjepidevuse suvandid, turbefunktsioonid ja arveldamine kord tunnis. Järgmises tabelis antakse ülevaade parima astme sobib erinevate rakenduse töökoormus.

| Teenuse kiht | Sihtrakenduse töökoormus |
|---|---|
| **Tavaline** | Sobib kõige paremini väikese andmebaasi, tavaliselt ühe ühe aktiivse tegevuse kindlal ajahetkel toetavad. Näited arendamise ja katsetamiseks kasutatavad andmebaasid või väikesemahuliste harva kasutatavad rakendused. |
| **Standard** | Funktsiooni go to suvand enamik pilve rakendused, toetavad mitme samaaegseid päringu. Näiteks töörühm või veebirakenduste. |
| **Premium** | Mõeldud selgituseks suuremahulise, toetavad palju samaaegseid kasutajaid ja nõuavad järjepidevuse ärianalüüsivõimalused kõrgeima taseme. Näited andmebaaside toetavad kriitiline ülesanne rakendusi. |

>[AZURE.NOTE] Veebi ja väljaanded on kasutuselt kõrvaldatud. Kui kavatsete kasutada veebi ja väljaanded, lugege [Sunset KKK](https://azure.microsoft.com/pricing/details/sql-database/web-business/) .

## <a name="standalone-database-service-tiers-and-performance-levels"></a>Autonoomse andmebaasi teenuse astme ja jõudluse tasemed
Autonoomse andmebaaside jaoks, on mitu jõudluse taseme iga teenuse kiht. Teil on valida taset, mis vastab teie töökoormus nõuab paindlikkust. Kui teil on vaja mastaapimiseks üles või alla, saate hõlpsalt muuta oma andmebaasi tasemete seas. Üksikasjad leiate [muutmise andmebaasi teenuse astme ja jõudlust](sql-database-scale-up.md) .

Siin loetletud parameetritele rakendada andmebaasid on loodud [SQL-i andmebaasi V12](sql-database-v12-whats-new.md). Sõltumata sellest, mitu majutatud andmebaase, andmebaasi saab tagatud komplekti ressursid ja oodatud parameetritele andmebaasi ei mõjuta.

[AZURE.INCLUDE [SQL DB service tiers table](../../includes/sql-database-service-tiers-table.md)]

>[AZURE.NOTE] Kõik muud selle teenuse astme tabeli read üksikasjalik selgitus teemast [teenuse taseme funktsioonid ja piirangud](sql-database-performance-guidance.md#service-tier-capabilities-and-limits).

## <a name="elastic-pool-service-tiers-and-performance-in-edtus"></a>Elastne rakenduskausta teenuse astme ja jõudluse eDTUs
Lisaks loomise ja skaleerimist autonoomse andmebaasi, on teil ka mitu andmebaasid on [elastne rakenduskausta](sql-database-elastic-pool.md)haldamiseks. Kõik andmebaasid elastne kohapeal on ühised ressursid ühiskasutusse anda. Parameetritele on mõõdetud *Elastne andmebaasi tehingu üksused* (eDTUs). Kui autonoomse andmebaasidega, kaustu tulevad kolmest teenuse tasemest: **lihtne**, **Standard**ja **Premium**. Kaustu, määratleda need kolm teenuse astme endiselt üldise jõudluse piirangud ja mitmesuguseid funktsioone.

Kaustu lubage andmebaase ühiskasutusse andmine ja tarbimine DTU ressursid vajamata määrata konkreetset jõudlusega igal pool andmebaasi. Näiteks saate minna autonoomse andmebaasi Standard pargis 0 eDTUs, et maksimaalne andmebaasi eDTU, saate häälestada pool konfigureerimisel abil. Kaustu luba mitme erineva töökoormus tõhus kasutada eDTU ressursse, mis on saadaval terve pool andmebaasid. Üksikasjad leiate [hinna ja jõudluse kaalutluste kohta kohapeal on elastne](sql-database-elastic-pool-guidance.md) .

Järgmises tabelis kirjeldatakse omaduste rakenduskausta teenuse astme.

[AZURE.INCLUDE [SQL DB service tiers table for elastic pools](../../includes/sql-database-service-tiers-table-elastic-db-pools.md)]

Iga andmebaas on järgib ka eraldi andmebaasi omaduste teise astme. Näiteks lihtsa pool on piiratud max seansid 4800-28800 kogumi kohta, kuid üksikute andmebaasiga lihtsa rakenduskausta sees on andmebaasi piiri 300 seansid.

## <a name="choosing-a-service-tier"></a>Teenuse kiht valimine

Teenuse kiht otsustamiseks Alustuseks määratlemine, kas andmebaasi peaks olema eraldi andmebaasi või elastne kohapeal on osa. 

### <a name="choosing-a-service-tier-for-a-standalone-database"></a>Teenuse kiht autonoomse andmebaasi valimine

Teenuse kiht autonoomse andmebaasi otsustamiseks käivitamiseks andmebaasi funktsioonid, mida peate valima oma SQL-andmebaasi versiooni määratlemine.

- Andmebaasi suurus (2 GB kuni Basic, Standard kuni 250 GB ja 500 GB kuni 1 TB Premiumi - sõltuvalt jõudluse)
- Andmebaasi varukoopia säilitusperiood (7 päeva Basic, Standard 35 päeva ja Premiumi 35 päeva)

Kui olete otsustanud SQL-andmebaasi väljaande, olete valmis, jõudluse taseme andmebaasi (arv DTUs) määramiseks. Te võite ette kujutada ning seejärel [mastaapimiseks üles või alla dünaamiliselt](sql-database-scale-up.md) tegeliku kogemuse. Samuti saate [DTU kalkulaator](http://dtucalculator.azurewebsites.net/) ühtlustada DTUs vaja arv. 

### <a name="choosing-a-service-tier-for-an-elastic-database-pool"></a>Parim teenuse kiht kohapeal on elastne andmebaasi jaoks.

Teenuse kiht kohapeal on elastne andmebaasi jaoks otsustamiseks Alustuseks määramisel peate valima teenuse kiht mõeldud pool oma andmebaasi funktsioonid.

- Andmebaasi suurus (2 GB Basic, 250 GB Standard ja 500 GB Premium)
- Andmebaasi varukoopia säilitusperiood (7 päeva Basic, Standard 35 päeva ja Premiumi 35 päeva)
- Arvu andmebaaside kohta (Basicu 400, 400 Standard ja Premiumi 50).
- Suurim lubatud salvestusruumi rakenduskausta (117 GB Basic, Standard, 1200 ja 750 Premiumi)

Kui teil on määratud teenuse kiht oma pool, olete valmis jõudluse taseme mõeldud pool (eDTUs) määramiseks. Te võite ette kujutada ja seejärel [mastaapimiseks või dünaamiliselt skaala](sql-database-elastic-pool-manage-portal.md#change-performance-settings-of-a-pool) põhjal tegelik kogemus. Saate [DTU kalkulaator](http://dtucalculator.azurewebsites.net/) ühtlustada DTUs vaja igal pool andmebaasi tuvastada pool ülempiiri arv.

## <a name="next-steps"></a>Järgmised sammud
- Lugege lisateavet hinnad nende astme [SQL](https://azure.microsoft.com/pricing/details/sql-database/)-andmebaasi hinna jaoks.
- Teave [elastne kaustu](sql-database-elastic-pool-guidance.md) ja [hinna ja jõudluse kaalutluste kohta elastne kaustu](sql-database-elastic-pool-guidance.md).
- Siit saate teada, kuidas [jälgida, hallata, ja suuruse muutmine elastne kaustu](sql-database-elastic-pool-manage-portal.md) ja [kuvari autonoomse andmebaaside jõudlus](sql-database-single-database-monitor.md).
- Nüüd, kui te ei tea astme SQL-andmebaasi, proovige neid [tasuta konto](https://azure.microsoft.com/pricing/free-trial/) ja saate teada, [Kuidas luua esimese SQL-andmebaasi](sql-database-get-started.md).

## <a name="additional-resources"></a>Lisaressursid

- [Mitme rentniku SaaS rakenduste abil Azure'i SQL-andmebaasi mustrite kujundamine](sql-database-design-patterns-multi-tenancy-saas-applications.md)
- [Microsoft Virtual Academy video kursuse elastne andmebaasi võimaluste Azure SQL-andmebaasis](https://mva.microsoft.com/en-US/training-courses/elastic-database-capabilities-with-azure-sql-db-16554)
