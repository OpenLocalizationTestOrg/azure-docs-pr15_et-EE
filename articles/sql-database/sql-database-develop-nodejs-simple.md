<properties
    pageTitle="Ühenduse loomine SQL-andmebaasiga, kasutades Node.js | Microsoft Azure'i"
    description="Esitab Node.js kood valimi abil saate Azure SQL-i andmebaasiga ühenduse loomiseks."
    services="sql-database"
    documentationCenter=""
    authors="meet-bhagdev"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="drivers"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="meetb"/>

# <a name="connect-to-sql-database-by-using-nodejs"></a>Ühenduse loomine SQL-andmebaasi Node.js abil

[AZURE.INCLUDE [sql-database-develop-includes-selector-language-platform-depth](../../includes/sql-database-develop-includes-selector-language-platform-depth.md)] 

Selles teemas kirjeldatakse, kuidas päringu abil Node.js Azure SQL-andmebaasi ühendada. See näidis saate käivitada Windows, Ubuntu Linux-või Mac-arvutisse.

## <a name="step-1-configure-development-environment"></a>Samm 1: Konfigureerimine arenduskeskkond

[SQL serveri tüütu Node.js draiver kasutamise eeltingimused](https://msdn.microsoft.com/library/mt652094.aspx)

## <a name="step-2-create-a-sql-database"></a>Samm 2: SQL-i andmebaasi loomine

Vaadake selle [lehe Alustamine](sql-database-get-started.md) saate teada, kuidas luua Näidisettevõtte andmebaas.  Oluline, et järgite juhend on **AdventureWorks veebiandmebaasi malli**loomiseks. Allpool näidised töötavad ainult **AdventureWorks skeemi**.

## <a name="step-3-get-connection-details"></a>Samm 3: Ühenduse üksikasjade saamiseks

[AZURE.INCLUDE [sql-database-include-connection-string-details-20-portalshots](../../includes/sql-database-include-connection-string-details-20-portalshots.md)]

## <a name="step-4-run-sample-code"></a>Samm 4: Käivita proovi kood

[Ühenduse loomine SQL-i abil Node.js mõistet tõendada](https://msdn.microsoft.com/library/mt715784.aspx)

## <a name="next-steps"></a>Järgmised sammud

* Vaadake üle [SQL-i andmebaasi arengu ülevaade](sql-database-develop-overview.md)
* Lisateavet [Microsoft SQL serveri Node.js draiver](https://msdn.microsoft.com/library/mt652093.aspx)

## <a name="additional-resources"></a>Lisaressursid 

* [Kujundus mustrid mitme rentniku SaaS rakenduste Azure SQL-andmebaas](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* [SQL-andmebaasi võimaluste](https://azure.microsoft.com/services/sql-database/) uurimine
