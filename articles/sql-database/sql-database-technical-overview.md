<properties
    pageTitle="Mis on SQL-andmebaasi? Sissejuhatus SQL-andmebaasi | Microsoft Azure'i"
    description="Saada SQL-andmebaasi Sissejuhatus: tehniline üksikasjade ja võimaluste kohta Microsofti relatsiooniline andmebaasi pilveteenuses süsteemi (RDBMS)."
    keywords="SQL-i, Sissejuhatus: SQL-i, mis on sql-andmebaasi tutvustus"
    services="sql-database"
    documentationCenter=""
    authors="shontnew"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="08/16/2016"
   ms.author="shkurhek"/>

# <a name="what-is-sql-database-introduction-to-sql-database"></a>Mis on SQL-andmebaasi? SQL-andmebaasi tutvustus

SQL-andmebaasi on turu-juhtiva Microsoft SQL Server engine olulise võimaluste põhjal pilves relatsiooniandmebaasist teenus. SQL-andmebaasi pakub prognoositavad jõudluse tagamiseks skaleeritavus pole tööseisakute, järjepidevuse ja andmekaitse – kõik koos nulli lähedal haldamine. Saate keskenduda kiire rakenduste arendamiseni ja kiirendamine aja Marketist, mitte virtuaalmasinates ja taristu haldamise. Kuna see põhineb [SQL Server](https://msdn.microsoft.com/library/bb545450.aspx) engine, SQL-andmebaasi toetab olemasoleva SQL serveri tööriistad, teekide ja API-d, mis hõlbustab teil liikuda ja laiendamine pilveteenusesse.

Selles artiklis on Sissejuhatus SQL-andmebaasi keskendudes ja jõudluse, skaleeritavus ja hallatavus koos linkidega uurimine üksikasjad seotud funktsioonid. Kui olete valmis hüpata, saate [esimese SQL-andmebaasi loomine](sql-database-get-started.md) või [Create kohapeal on elastne andmebaasi](sql-database-elastic-pool-create-portal.md) minutit. Kui soovite põhjalikult sukelduda, vaadake seda videot ja 30 minuti.

> [AZURE.VIDEO azurecon-2015-get-started-with-azure-sql-database]

## <a name="adjust-performance-and-scale-without-downtime"></a>Jõudluse ja skaala ilma tööseisakute reguleerimine

SQL-i andmebaasid on saadaval Basic, Standard, ja *teenuse astme*. Iga teenuse kiht pakub [erineva jõudlus ja võimaluste](sql-database-service-tiers.md) toetamiseks kerge, et raskekaalu andmebaasi töökoormus. Saate koostada oma esimese rakenduse small andmebaasi paar dollarit kuus, siis [Muuda teenuse kiht](sql-database-scale-up.md) käsitsi või programmiliselt igal ajal kui rakenduse läheb viiruse kogu maailmas, ilma tööseisakute rakenduse või oma klientidele.

Paljude ettevõtete ja rakendused, on võimalik luua andmebaasid ja valida ühe andmebaasi jõudluse üles või alla nõudmisel on piisavalt, eriti siis, kui mustreid on üsna hõlpsalt prognoosida. Kuid kui teil on ettearvamatuid mustreid, seda saab teha raske kulud ja oma äri mudeli haldamine.

SQL-andmebaasi [elastne kaustu](sql-database-elastic-pool.md) probleemi lahendamiseks. Põhimõte on lihtne. Jõudluse on eraldada ja küsitlustulemuste jõudlus kogumi, mitte ühe andmebaasi jõudluse eest maksta. Te ei pea sissehelistamist andmebaasi jõudluse üles või alla. Andmebaaside sisse nimega *elastne andmebaaside*automaatselt mastaapida üles ja alla vastavad nõudmisel. Elastne andmebaaside peab olema, kuid ei ületa kogumi, nii, et teie maksumus jääb prognoositavad isegi siis, kui andmebaasi kasutus ei. Lisaks saate teha [lisamine ja eemaldamine pool andmebaase](sql-database-elastic-pool-manage-portal.md), skaleerimist rakenduse andmebaaside üksikud tuhandeliste, kõik, mida saate määrata eelarve piires. Kujundus mustrite abil elastne kaustu SaaS rakenduste kohta leiate lisateavet teemast [Mitme rentniku SaaS rakenduste Azure'i SQL-andmebaasi kujunduse mustrid](sql-database-design-patterns-multi-tenancy-saas-applications.md).

Mõlemal juhul, kui lähete – ühe või elastne – te olete ei lukustatud. Saate ühe andmebaasid elastne andmebaasi kaustu segu ja teenuse tasemete seas ühe andmebaasid ja kaustu luua uuendusliku kujunduse muutmine. Lisaks power ja Azure REACHi, saate mix ja match Azure SQL-andmebaasi teie vajadustele kordumatu tänapäevane rakenduse kujundus, drive maksumuse ja ressursside tõhusust ja avada uusi teenustega.

Aga kuidas teil võrrelda andmebaaside ja andmebaasi suhteline jõudlus? Kuidas teate paremklõpsake käsku Lõpeta, kui valite üles? Vastus on andmebaasi tehingu üksus (DTU) ühe andmebaasid ja elastne DTU (eDTU) elastne andmebaasid ja andmebaasi kaustu. Vt [SQL-andmebaasi suvandid ja jõudluse: mõista, mis on saadaval iga teenuse kiht](sql-database-service-tiers.md) üksikasju.

## <a name="keep-your-app-and-business-running"></a>Hoidke oma rakenduse ja ettevõtte igapäevategevuses

Azure valdkonna ees 99,99% kättesaadavus teenusetaseme lepinguga [(SLA)](http://azure.microsoft.com/support/legal/sla/), tootja globaalse võrgu Microsoft haldusega andmekeskuste, hoiab teie rakendus töötab 24/7. Iga SQL-andmebaasiga, saate ära kasutada sisseehitatud andmekaitse, tõrketaluvust ja andmekaitse, et teil oleks muidu kujundamine, ostmine, luua ja hallata. Isegi nii, olenevalt teie ettevõttest nõuetele, võivad nõuda täiendavate kihtide kaitse tagada teie rakendus ja teie ettevõtte saate taastada kiiresti katastroof, tõrge või midagi muud. SQL-andmebaasiga, kus iga teenuse kiht pakub eri menüüd töötab ja funktsioonide abil saate leida ja hinnast nii. Punkti õigel ajal taastamine abil saate andmebaasi varasema oleku, juba 35 päeva tagasi. Lisaks, kui andmekeskuse majutada oma andmebaasid on katkestuste, saate Tõrkesiirde andmebaasi koopiad teises regioonis. Või kasutage uuendamine või et erinevate piirkondade koopiad.

![SQL-i andmebaasi Geo-tiražeerimine.](./media/sql-database-technical-overview/azure_sqldb_map.png)


Vt lisateavet erinevate äri järjepidevus funktsioonid saadaval eri teenuse astme [Järjepidevuse](sql-database-business-continuity.md) .

## <a name="secure-your-data"></a>Kaitsta teie andmeid
SQL serveri on ühtlase andmete turvalisus, et SQL-andmebaasi toetab funktsioone, mis piirata juurdepääsu, andmete kaitsmine ja aitavad teil jälgimiseks aega. Teil on SQL-andmebaasis turbesuvandid kiirülevaade leiate [turvaliseks SQL-andmebaasi](sql-database-security.md) . Vaadake teemat [Turbekeskus andmebaasimootor SQL Server ja SQL-andmebaasi](https://msdn.microsoft.com/library/bb510589) põhjalikuma ülevaate turbefunktsioonid. Ja [Azure'i usalduskeskuses](https://azure.microsoft.com/support/trust-center/security/) Azure platvormi saamiseks külastage.

## <a name="next-steps"></a>Järgmised sammud
Nüüd, kui olete lugeda SQL-andmebaasi tutvustus ja vastas küsimusele "Mis on SQL-andmebaasi?", olete valmis:

- Vt [hinnad lehe](https://azure.microsoft.com/pricing/details/sql-database/) ühe andmebaasi ja elastne andmebaasi maksumus võrdlusi ja kalkulaatorid.
- Lisateavet [elastne kaustu](sql-database-elastic-pool.md).
- Alustuseks [esimese andmebaasi loomine](sql-database-get-started.md).
- [Ühenduse loomine ja päringu SSMS abil](sql-database-connect-query-ssms.md)
- Oma esimese rakenduse C#, Java, Node.js, PHP, Python või Ruby: [andmeühenduste teegid SQL-andmebaas ja SQL Server](sql-database-libraries.md)
- Registri tiitlite ja [kõigi](sql-database-index-all-articles.md)teemade Azure sql-andmebaasi teenuse kirjeldused leiate.
