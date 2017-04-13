<properties
    pageTitle="Ühenduse loomine SQL-andmebaasiga, kasutades .NET (C#) | Microsoft Azure'i"
    description="Proovi kood see Kiire alustamine koostamiseks kaasaegne rakendus C# ja toetavad võimsaid relatsiooniandmebaasist pilveteenuses koos Azure'i SQL-andmebaasi kasutada."
    services="sql-database"
    documentationCenter=""
    authors="tobbox"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="drivers"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/16/2016"
    ms.author="tobiast"/>

# <a name="connect-to-sql-database-by-using-net-c"></a>Ühenduse loomine SQL-andmebaasiga, kasutades .NET (C#)

[AZURE.INCLUDE [sql-database-develop-includes-selector-language-platform-depth](../../includes/sql-database-develop-includes-selector-language-platform-depth.md)] 

## <a name="step-1--configure-development-environment"></a>Samm 1: Konfigureerimine arenduskeskkond

[ADO.net-i arengu arenduskeskkond konfigureerimine](https://msdn.microsoft.com/library/mt718321.aspx)

## <a name="step-2-create-a-sql-database"></a>Samm 2: SQL-i andmebaasi loomine

Vaadake selle [lehe Alustamine](sql-database-get-started.md) saate teada, kuidas luua Näidisettevõtte andmebaas.  Oluline, et järgite juhend on **AdventureWorks veebiandmebaasi malli**loomiseks. Allpool näidised töötavad ainult **AdventureWorks skeemi**.  

## <a name="step-3--get-connection-string"></a>Samm 3: Ühendusstringi toomiseks

[AZURE.INCLUDE [sql-database-include-connection-string-dotnet-20-portalshots](../../includes/sql-database-include-connection-string-dotnet-20-portalshots.md)]

## <a name="step-4-run-sample-code"></a>Samm 4: Käivita proovi kood

* [Tõendada mõistet ühenduse SQL-i abil](https://msdn.microsoft.com/library/mt718320.aspx)
* [Elastselt ühenduse SQL-i ADO.net-i](https://msdn.microsoft.com/library/mt703195.aspx)

## <a name="next-steps"></a>Järgmised sammud

* [ASP.net-i MVC rakenduse auth ja SQL-i DB loomine ja juurutamine Azure'i rakendust Service]( ../app-service-web/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md)
* Vaadake üle [SQL-i andmebaasi arengu ülevaade](sql-database-develop-overview.md)
* Lisateavet [Microsoft SQL serveri ADO.net-i draiver](https://msdn.microsoft.com/library/mt657768.aspx)

## <a name="additional-resources"></a>Lisaressursid 

* [Kujundus mustrite mitme rentniku SaaS rakenduste Azure SQL-i andmebaasiga](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* [SQL-andmebaasi võimaluste](https://azure.microsoft.com/services/sql-database/) uurimine





