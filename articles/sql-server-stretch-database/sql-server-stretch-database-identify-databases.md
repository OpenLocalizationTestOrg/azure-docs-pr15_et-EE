<properties
    pageTitle="Andmebaasid ja tabelite tuvastamine venitamine andmebaasi Advisor käivitades venitamine andmebaasi | Microsoft Azure'i"
    description="Saate teada, kuidas tuvastada, andmebaasid ja tabelid, mis venitamine andmebaasi, saavad."
    services="sql-server-stretch-database"
    documentationCenter=""
    authors="douglaslMS"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-server-stretch-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/14/2016"
    ms.author="douglasl"/>

# <a name="identify-databases-and-tables-for-stretch-database-by-running-stretch-database-advisor"></a>Andmebaasid ja tabelite tuvastamine venitamine andmebaasi venitamine andmebaasi Advisor käivitades

Andmebaasid ja tabelid, mis venitamine andmebaasi, saavad tuvastamiseks SQL Server 2016 täiendamine Advisor alla laadida ja installida venitamine andmebaasi. Venitus andmebaasi Advisor tuvastab ka blokeerimise probleemid.

## <a name="download-and-install-upgrade-advisor"></a>Laadige alla ja installige
Laadige alla ja installige [siia](http://go.microsoft.com/fwlink/?LinkID=613421). See tööriist on olemas pole SQL serveri installikandjalt.

## <a name="run-the-stretch-database-advisor"></a>Venitus andmebaasi installida

1.  Käivitage versioonitäienduse Advisor.

2.  Valige **stsenaariumid**ja valige **Käivita VENITAMINE andmebaasi ADVISOR**.

3.  **Käivitage venitamine andmebaasi Advisor** enne, klõpsake nuppu **Valige andmebaaside analüüsimine**.

4.  **Valige andmebaaside** enne sisestage või valige serveri nimi ja autentimine teave. Klõpsake nuppu **Loo ühendus**.

5.  Kuvatakse loend andmebaaside valitud serveris. Valige andmebaase, mida soovite analüüsida. Klõpsake nuppu **Vali**.

6.  **Venitamine andmebaasi Advisor käivitada** enne, klõpsake nuppu **Käivita**.  Analüüsi töötab.

## <a name="review-the-results"></a>Vaadake tulemused üle

1.  Analüüsi lõpetades enne **Analyzed andmebaase** , valige üks andmebaase, mida te analüüsida tera **analüüsi tulemuste** kuvamiseks.

    **Analüüsi tulemuste** tera loetletud soovitatav vaikimisi soovitus kriteeriumidele vastavate valitud andmebaasi tabelid.

2.  **Analüüsi tulemuste** enne tabelite loendist, valige üks soovitatav tabelid tera **tabeli tulemite** kuvamiseks.

    Kui takistavad probleemid, on loetletud **tabeli tulemused** tera blokeerimise probleemide valitud tabeli. Tuvastatud venitamine andmebaasi Advisor blokeerimise probleemide kohta leiate artiklist [piirangud venitamine andmebaasi](sql-server-stretch-database-limitations.md).

3.  Blokeerimise probleemid enne **tabeli tulemuste** loendis Valige probleemide kohta leiate lisateavet valitud probleemi kuvamiseks ja teeb vähendamiseks juhiseid. Soovitatud vähendamiseks juhiseid rakendada, kui soovite valitud tabeli venitamine andmebaasi konfigureerimine.

## <a name="next-step"></a>Järgmise juhise juurde
Luba venitus andmebaasi.

-   **Andmebaasi**venitamine andmebaasi lubamise kohta lugege [Lubamine venitamine andmebaasi andmebaasi](sql-server-stretch-database-enable-database.md).

-   Venitamine andmebaasi lubamine teises **tabelis**, kui andmebaas on juba lubatud venitada, lugege teemat [Lubamine venitamine andmebaasi tabelisse](sql-server-stretch-database-enable-table.md).

## <a name="see-also"></a>Vt ka

[Venitus andmebaasi piirangud](sql-server-stretch-database-limitations.md)

[Andmebaasi venitamine andmebaasi lubamine](sql-server-stretch-database-enable-database.md)

[Tabeli venitamine andmebaasi lubamine](sql-server-stretch-database-enable-table.md)

[Kõigi teemade Azure SQL serveri venitamine andmebaasi teenuse jaoks](sql-server-stretch-database-index-all-articles.md)
