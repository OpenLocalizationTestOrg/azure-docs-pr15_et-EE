<properties
    pageTitle="Ühenduse loomine SQL-andmebaasiga, kasutades PHP opsüsteemis Windows | Microsoft Azure'i"
    description="Esitab valimi PHP programmi, mis ühendub klientrakenduses Windows Azure'i SQL-andmebaasi ja lingid kliendile vajalikku vajalik tarkvara komponendid."
    services="sql-database"
    documentationCenter=""
    authors="meet-bhagdev"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="drivers"
    ms.tgt_pltfrm="na"
    ms.devlang="php"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="meetb"/>


# <a name="connect-to-sql-database-by-using-php-on-windows"></a>Ühenduse loomine SQL-andmebaasiga, kasutades PHP opsüsteemis Windows


[AZURE.INCLUDE [sql-database-develop-includes-selector-language-platform-depth](../../includes/sql-database-develop-includes-selector-language-platform-depth.md)] 


Selles teemas on näidatud, kuidas saate luua ühenduse Azure'i SQL-andmebaasi kliendi rakendus kirjutatud PHP, kus töötab Windows.

## <a name="step-1--configure-development-environment"></a>Samm 1: Konfigureerimine arenduskeskkond

[Arenduskeskkond PHP arengu konfigureerimine](https://msdn.microsoft.com/library/mt720663.aspx)

## <a name="step-2-create-a-sql-database"></a>Samm 2: SQL-i andmebaasi loomine

Vaadake selle [lehe Alustamine](sql-database-get-started.md) saate teada, kuidas luua Näidisettevõtte andmebaas.  Oluline, et järgite juhend on **AdventureWorks veebiandmebaasi malli**loomiseks. Allpool näidised töötavad ainult **AdventureWorks skeemi**.


## <a name="step-3-get-connection-details"></a>Samm 3: Ühenduse üksikasjade saamiseks

[AZURE.INCLUDE [sql-database-include-connection-string-details-20-portalshots](../../includes/sql-database-include-connection-string-details-20-portalshots.md)]


## <a name="step-4-run-sample-code"></a>Samm 4: Käivita proovi kood

* [Ühenduse loomine SQL-i kasutades PHP mõistet tõendada](https://msdn.microsoft.com/library/mt720665.aspx)
* [SQL-i php elastselt ühendamine](https://msdn.microsoft.com/library/mt720667.aspx)


## <a name="next-steps"></a>Järgmised sammud

* Vaadake üle [SQL-i andmebaasi arengu ülevaade](sql-database-develop-overview.md)
* Lisateavet [Microsoft SQL Server PHP draiver](https://msdn.microsoft.com/library/dn865013.aspx)
* PHP installimise ja kasutamise kohta lisateabe saamiseks vt [SQL serveri andmebaasi php juurdepääs](http://social.technet.microsoft.com/wiki/contents/articles/1258.accessing-sql-server-databases-from-php.aspx).

## <a name="additional-resources"></a>Lisaressursid 

* [Kujundus mustrid mitme rentniku SaaS rakenduste Azure SQL-andmebaas](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* [SQL-andmebaasi võimaluste](https://azure.microsoft.com/services/sql-database/) uurimine
