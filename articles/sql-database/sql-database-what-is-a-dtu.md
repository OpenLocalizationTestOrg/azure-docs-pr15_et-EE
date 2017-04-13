<properties
    pageTitle="SQL-andmebaasi: Mis on saanud DTU? | Microsoft Azure'i"
    description="Millist Azure SQL-andmebaasi mõistmine tehingu üksus on."
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
    ms.workload="NA"
    ms.date="09/06/2016"
    ms.author="carlrab"/>

# <a name="explaining-database-transaction-units-dtus-and-elastic-database-transaction-units-edtus"></a>Selgitab andmebaasi tehingu (DTUs) elastne andmebaasi tehingu ühikute ja (eDTUs)

Selles artiklis selgitatakse andmebaasi tehingu üksused (DTUs) ja elastne andmebaasi tehingu üksused (eDTUs) ja mis juhtub, kui vajutate maksimaalne DTUs või eDTUs.  

## <a name="what-are-database-transaction-units-dtus"></a>Mis on andmebaasi tehingu üksused (DTUs)

Mõne DTU on ressursse, mis on saadaval autonoomse Azure SQL-andmebaasiga sees on [eraldi andmebaasi teenuse kiht](sql-database-service-tiers.md#standalone-database-service-tiers-and-performance-levels)konkreetsete tulemuslikkuse tasemel tagatud mõõtühik. Mõne DTU on segatud CPU, mälu ja andmete SV- ja tehingu Logi I/O suhe määratud OLTP võrdlusalus töökoormus eesmärk olema tegelike OLTP töökoormus. Kahekordne soovitud DTUs andmebaasi jõudluse suurendades võrdub kahekordne olemasoleva andmebaasi ressursi määramine. Näiteks pakub Premium P11 andmebaasi 1750 DTUs 350 x rohkem DTU arvutada power kui 5 DTUs lihtsa andmebaasi. Taha OLTP võrdlusalus töökoormus DTU segu määramiseks kasutatav meetod, vt [SQL-andmebaasi võrdlusalus ülevaade](sql-database-benchmark-overview.md).

![SQL-andmebaasi Sissejuhatus: ühe andmebaasi DTUs taseme ja tase](./media/sql-database-what-is-a-dtu/single_db_dtus.png)

Saate [muuta teenuse astme](sql-database-scale-up.md) igal ajal minimaalne ajakulu rakenduse (üldjuhul keskmistamine neli-sekundit). Paljude ettevõtete ja rakendused, on võimalik luua andmebaasid ja valige üks andmebaasi jõudluse üles või alla nõudmisel on piisavalt, eriti siis, kui mustreid on üsna hõlpsalt prognoosida. Kuid kui teil on ettearvamatuid mustreid, seda saab teha raske kulud ja oma äri mudeli haldamine. Selle stsenaariumi puhul kasutage elastne kohapeal on eDTUs arvu.

## <a name="what-are-elastic-database-transaction-units-edtus"></a>Mis on elastne andmebaasi tehingu üksused (eDTUs)

Mõne eDTU on ressursid (DTUs), mida saab jagada vahel andmebaase, Azure SQL serveri - ehk on [elastne rakenduskausta](sql-database-elastic-pool.png)kogumi mõõtühik. Elastne kaustu pakuvad lihtsa lahenduseks hallata mitut andmebaasi, mis on väga erinevaid ja ettearvamatuid mustreid jõudluse eesmärke. [Elastne kaustu ja teenuse astme](sql-database-service-tiers.md#elastic-pool-service-tiers-and-performance-in-edtus) lisateabe saamiseks vt.

![SQL-andmebaasi Sissejuhatus: eDTUs taseme ja tase](./media/sql-database-what-is-a-dtu/sqldb_elastic_pools.png)

On antakse teatud eDTUs hinna arv. Maksimaalselt pool, üksikud andmebaasid on antud paindlikkust määramine parameetrite raames mastaabi automaatselt. Suur koormus, andmebaasi saab kasutada rohkem eDTUs rahuldamiseks. Andmebaaside hele laadimise peab olema väiksem ja andmebaaside koormamata tarbimine pole eDTUs. Ettevalmistamise ressursse kogu rakenduskausta mitte ühe andmebaaside lihtsustab haldamisega seotud toiminguid. Lisaks on teil hõlpsalt prognoosida eelarve mõeldud pool.

Täiendavate eDTUs saab lisada mõne olemasoleva kausta pole andmebaasi tööseisakute või andmebaaside elastne kogumi ei mõjuta. Samamoodi kui täiendavat eDTUs ei ole enam vaja need saab eemaldada olemasoleva kaustast igal ajal. Saate lisada või lahutamine andmebaaside pool. Kui andmebaas on ootuspäraselt jaotises-kasutades ressursse, kolima.

## <a name="how-can-i-determine-the-number-of-dtus-needed-by-my-workload"></a>Kuidas kindlaks teha DTUs töökoormus nõutud arv?

Kui otsite migreerimine olemasoleva kohapealse või SQL serveri virtuaalse masina töökoormus Azure SQL-andmebaasiga, saate kasutada [DTU kalkulaator](http://dtucalculator.azurewebsites.net/) ühtlustada DTUs vaja arv. Olemasoleva Azure'i SQL-andmebaasi töökoormus, saate [SQL-i andmebaasi päringu jõudluse ülevaate](sql-database-query-performance.md) mõista teie andmebaasi ressursside tarbimine (DTUs), et saada täpsemat ülevaade sellest, kuidas optimeerida oma töökoormus. Samuti saate [sys.dm_db_ resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) DMV ressursside tarbimine teavet viimase üks tund. Teise võimalusena kataloogi vaade [sys.resource_stats](http://msdn.microsoft.com/library/dn269979.aspx) saate ka kasutataks samade andmete saamiseks viimase 14 päeva jooksul, kuigi veebisaidil alumise töökindluse, viie minuti.

## <a name="how-do-i-know-if-i-could-benefit-from-an-elastic-pool-of-resources"></a>Kuidas teada saada, kui võib kasu ressursside elastne kaustast?

Suure hulga andmebaaside konkreetse kasutuse mustriga sobivad kaustu. Andmebaasi, seda mudelit iseloomustab madal keskmise kasutamine koos suhteliselt harv kasutamine diagrammi. SQL-andmebaasi automaatselt hindab olemasoleva SQL-andmebaasi serveri andmebaasidega ajalooliste ressursikasutuse ja soovitab vastav rakenduskausta konfiguratsiooni Azure'i portaalis. Lisateavet leiate teemast [Millal kohapeal on elastne andmebaasi kasutada?](sql-database-elastic-pool-guidance.md)

## <a name="what-happens-when-i-hit-my-maximum-dtus"></a>Mis juhtub minu maksimaalne DTUs klõpsasin

Jõudluse tasemed on kalibreeritud ja esitada vajalikud ressursid oma andmebaasi töökoormus max piires lubatud teie valitud taseme/jõudlusega käivitamiseks reguleerida. Kui teie töökoormus on pihta piirangud ühes CPU/andmete IO/Log IO piirangud, saate edaspidi ressursid maksimaalselt lubatud taseme, kuid te näete tõenäoliselt suurema latentsused teie päringuid. Need piirangud tulemuseks vigu, kuid pigem aeglustumine töökoormus, v.a juhul, kui aeglustumine muutub nii tugev, et päringute käivitage ajastus. Kui teil on pihta piirangud suurim lubatud samaaegseid kasutaja seansid/taotluste (töötaja teemad), näete konkreetsete tõrkeid. Leiate [Azure'i SQL-andmebaasi ressursi piirangud](sql-database-resource-limits.md) piirang peale CPU, mälu, andmete I/O ressursside kohta teavet, ja tehingu log i/o.

## <a name="next-steps"></a>Järgmised sammud

- Leiate teavet [teenuse kiht](sql-database-service-tiers.md) DTUs ja eDTUs saadaval autonoomse andmebaasid ja elastne kaustu.
- Leiate [Azure'i SQL-andmebaasi ressursi piirangud](sql-database-resource-limits.md) piirang peale CPU, mälu, andmete I/O ressursside kohta teavet, ja tehingu log i/o.
- Vt [SQL-i andmebaasi päringu jõudluse ülevaate](sql-database-query-performance.md) mõista teie tarbimine (DTUs).
- Vt [SQL-andmebaasi võrdlusalus ülevaade](sql-database-benchmark-overview.md) mõista taha OLTP võrdlusalus töökoormus DTU segu määramiseks kasutatav meetod.