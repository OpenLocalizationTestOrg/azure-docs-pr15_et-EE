<properties
    pageTitle="Ühenduse loomine SQL-andmebaasi kasutades Java JDBC Windows | Microsoft Azure'i"
    description="Esitab Java koodi valimi abil saate Azure SQL-i andmebaasiga ühenduse loomiseks. Valimi kasutab JDBC ja see töötab Windows klientarvutis."
    services="sql-database"
    documentationCenter=""
    authors="LuisBosquez"
    manager="jhubbard"
    editor="genemi"/>


<tags
    ms.service="sql-database"
    ms.workload="drivers"
    ms.tgt_pltfrm="na"
    ms.devlang="java"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="lbosq"/>


# <a name="connect-to-sql-database-by-using-java-with-jdbc-on-windows"></a>Kasutades Java JDBC Windows SQL-andmebaasiga ühenduse loomine


[AZURE.INCLUDE [sql-database-develop-includes-selector-language-platform-depth](../../includes/sql-database-develop-includes-selector-language-platform-depth.md)] 


Sellest teemast leiate Java koodi valimi Azure SQL-i andmebaasiga ühenduse loomiseks saate. Java valimi tugineb klõpsake Java arengu Kit (JDK) versioon 1.8. Valimi loob Azure SQL-andmebaasi JDBC draiveri abil.

## <a name="step-1--configure-development-environment"></a>Samm 1: Konfigureerimine arenduskeskkond

[Java arengu arenduskeskkond konfigureerimine](https://msdn.microsoft.com/library/mt720658.aspx)

## <a name="step-2-create-a-sql-database"></a>Samm 2: SQL-i andmebaasi loomine

Vaadake selle [lehe Alustamine](sql-database-get-started.md) saate teada, kuidas luua andmebaasi.  

## <a name="step-3-get-connection-string"></a>Samm 3: Ühendusstringi toomiseks

[AZURE.INCLUDE [sql-database-include-connection-string-jdbc-20-portalshots](../../includes/sql-database-include-connection-string-jdbc-20-portalshots.md)]

> [AZURE.NOTE] Kui kasutate JTDS JDBC draiveri, siis peate lisama "SSL-i = nõua" URL-i ühenduse string ja teil vaja selle töötab järgmised suvandi seadmine "-Djsse.enableCBCProtection=false". See töötab suvand keelab tarkvarakomplektist parandust veenduge, et saate aru, milliseid risk on seotud enne, kui see suvand.

## <a name="step-4-run-sample-code"></a>Samm 4: Käivita proovi kood

* [Ühenduse loomine SQL-i abil Java mõistet tõendada](https://msdn.microsoft.com/library/mt720656.aspx)

## <a name="next-steps"></a>Järgmised sammud

* Külastage [Java Arenduskeskus](/develop/java/).
* Vaadake üle [SQL-i andmebaasi arengu ülevaade](sql-database-develop-overview.md)
* Lisateavet [Microsoft JDBC draiveri SQL Server](https://msdn.microsoft.com/library/mt484311.aspx)

## <a name="additional-resources"></a>Lisaressursid 

* [Kujundus mustrid mitme rentniku SaaS rakenduste Azure SQL-andmebaas](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* [SQL-andmebaasi võimaluste](https://azure.microsoft.com/services/sql-database/) uurimine
