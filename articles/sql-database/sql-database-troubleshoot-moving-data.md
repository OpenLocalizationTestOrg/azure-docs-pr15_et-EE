<properties
    pageTitle="Andmebaaside meiliserverite vahel tellimused ja sealt Azure'i vahel liikumine"
    description="Kiirtoimingud kopeerida, teisaldada ja andmete ja andmebaasidega Azure'i SQL-andmebaasi migreerimine."
    services="sql-database"
    documentationCenter=""
    authors="v-shysun"
    manager="felixwu"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="v-shysun"/>

# <a name="move-databases-between-servers-between-subscriptions-and-in-and-out-of-azure"></a>Tellimuste vahel ja sealt Azure serverites andmebaaside teisaldamine

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]
##<a name="to-move-a-database-to-a-different-server-in-the-same-subscription"></a>Andmebaasi liikumiseks sama tellimuse eri server
- [Azure portaali](https://portal.azure.com), käsku **SQL-i andmebaasid**, valige loendist andmebaas ja seejärel klõpsake käsku **Kopeeri**. Leiate [Azure'i SQL-andmebaasi kopeerimine](sql-database-copy.md) rohkem üksikasju.

## <a name="to-move-a-database-between-subscriptions"></a>Andmebaasi tellimuste vahel liikumiseks
- [Azure portaali](https://portal.azure.com), klõpsake käsku **SQL-i serverite** ja seejärel valige loendist oma andmebaasi majutavas serveris. Klõpsake nuppu **Teisalda**ja seejärel valige ressursside liikumiseks ja tellimuse liikumiseks.

## <a name="to-migrate-a-sql-database-into-azure"></a>Kui soovite Azure SQL-andmebaasi migreerimine
- Määratleda andmebaasi ühilduvuse ja valige siis õige migreerimismeetod, vastavalt oma vajadustele. Täitke juhised ja [SQL serveri andmebaasi migreerimine](sql-database-cloud-migrate.md)suvandid.

## <a name="to-create-a-copy-of-a-database-for-use-outside-of-azure"></a>Andmebaasi kasutamiseks väljaspool Azure koopia loomiseks
- [Eksportida BACPAC faili.](sql-database-export.md)
