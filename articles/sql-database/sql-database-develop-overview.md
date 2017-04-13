<properties
    pageTitle="SQL-andmebaasi arendamise ülevaade | Microsoft Azure'i"
    description="Teavet saadaval connectivity teekide ja head tavad rakenduste SQL-andmebaasiga ühenduse loomine."
    services="sql-database"
    documentationCenter=""
    authors="annemill"
    manager="jhubbard"
    editor="genemi"/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/17/2016"
    ms.author="annemill"/>

# <a name="sql-database-development-overview"></a>SQL-i andmebaasi arengu ülevaade
Selles artiklis tutvustatakse arendaja peaks olema teadlik Azure SQL-i andmebaasiga ühenduse loomiseks koodi kirjutamiseks lihtsa asjaolu.

## <a name="language-and-platform"></a>Keele- ja platvormi
See on saadaval erinevad programmeerimise keeled ja platvormid koodinäiteid. Siit leiate linke koodinäiteid veebisaidil: 

* Lisateave: [SQL-andmebaas ja SQL serveri andmeühenduste teegid](sql-database-libraries.md)

## <a name="resource-limitations"></a>Ressurssidega
Azure'i SQL-andmebaasi haldab abil kahe eri menetlustele andmebaasi ressurssidest: ressursside juhtimise ja täitmise, piirangud.

* Lisateave: [SQL Azure'i andmebaasi ressursi piirangud](sql-database-resource-limits.md)

## <a name="security"></a>Turvalisus
Azure'i SQL-andmebaasi kokku ressursid juurdepääsu piiramise andmete kaitsmiseks ja SQL-andmebaasi tegevuse jälgimine.

* Lisateave: [SQL-andmebaasi turvamine](sql-database-security.md)

## <a name="authentication"></a>Autentimine
* Azure'i SQL-andmebaasi toetab SQL serveri autentimine kasutajate ja sisselogimise, kui ka [Azure Active Directory autentimine](sql-database-aad-authentication.md) kasutajate ja sisselogimise.
* Peate määrama kindla andmebaasi, mitte vaikeväärtus on *põhi* andmebaasi.
* Ei saa kasutada Transact-SQL-i **kasutamine myDatabaseName;** lause SQL-andmebaasi kasutusele mõne muu andmebaasi.
* Lisateavet: [SQL-andmebaasi turvalisuse: haldamine Accessi ja logige sisse andmebaasi turvalisuse](sql-database-manage-logins.md)

## <a name="resiliency"></a>Paindlikkust
Siirdamiseks tõrke ilmnemisel SQL-andmebaasiga ühenduse loomise ajal oma kood tuleks uuesti kõne.  Soovitame mis uuesti loogika kasutamine backoff loogika, nii, et see uputama SQL-andmebaasi mitme kliendi korraga proovitakse uuesti.

* Proovi kood: koodinäiteid, mis illustreerivad uuestiproovimise loogikale, kohta leiate veebisaidil teie valitud keele jaoks: [andmeühenduste teegid SQL-andmebaas ja SQL Server](sql-database-libraries.md)
* Lisateave: [SQL-andmebaasi Klientprogrammide tõrketeated](sql-database-develop-error-messages.md)

## <a name="managing-connections"></a>Ühenduste haldamine
* Oma kliendi ühenduse loogika alistada vaikimisi ajalõpp kuni 30 sekundit.  Vaikimisi 15 minutit liiga lühend ühendusi, mis sõltuvad Interneti-ühendus.
* Kui kasutate [ühenduse pool](http://msdn.microsoft.com/library/8xx3tyca.aspx), kindlasti katkestamiseks kohe oma programm on aktiivselt seda kasutama ja on valmistumine uuesti kasutada.

## <a name="network-considerations"></a>Võrgu peaksite arvesse võtma
* Arvutites, mis hostib teie klientprogrammi, veenduge, et tulemüüri võimaldab väljamineva TCP port 1433.  Lisateave: [mõne Azure'i SQL-andmebaasi tulemüüri konfigureerimine](sql-database-configure-firewall-settings.md)
* Kui teie klient programmi loob ühenduse SQL-i andmebaasi V12 ajal Azure virtuaalse masina (VM) oma kliendi töötab, peate avama teatud port vahemike VM. Lisateave: [pordid 1433 4.5 ADO.net-i ja SQL-i andmebaasi V12 Lisaks](sql-database-develop-direct-route-ports-adonet-v12.md)
* Kliendi ühendusi Azure SQL-i andmebaasi V12 mõnikord puhverserverist mööduda ja andmebaasi suhelda. Pordid 1433 peale muutuvad oluline. Lisateave: [pordid 1433 4.5 ADO.net-i ja SQL-i andmebaasi V12 Lisaks](sql-database-develop-direct-route-ports-adonet-v12.md)

## <a name="data-sharding-with-elastic-scale"></a>Andmete Sharding elastne skaala
Elastne skaala lihtsustab skaleerimist (ja). 

* [Kujundus mustrid mitme rentniku SaaS rakenduste Azure SQL-andmebaas](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* [Andmed sõltuvad marsruutimine](sql-database-elastic-scale-data-dependent-routing.md)
* [SQL Azure'i andmebaasi elastne skaala Preview kasutamise alustamine](sql-database-elastic-scale-get-started.md)

## <a name="next-steps"></a>Järgmised sammud

Uurige [võimalusi SQL-andmebaasi](https://azure.microsoft.com/services/sql-database/).
