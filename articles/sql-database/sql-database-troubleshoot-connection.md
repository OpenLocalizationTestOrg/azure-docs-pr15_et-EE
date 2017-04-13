<properties
    pageTitle="Andmebaasi server pole praegu saadaval, SQL-andmebaasiga ühenduse | Microsoft Azure'i"
    description="Tõrkeotsingut andmebaasi server pole praegu saadaval tõrge, kui rakendus loob SQL-andmebaasiga."
    services="sql-database"
    documentationCenter=""
    authors="dalechen"
    manager="felixwu"
    editor=""
    keywords="andmebaasi server pole praegu saadaval, sql-andmebaasiga ühenduse loomine"/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/21/2016"
    ms.author="daleche"/>

# <a name="error-database-on-server-is-not-currently-available-when-connecting-to-sql-database"></a>Tõrge "Andmebaasi server pole praegu saadaval" sql-andmebaasiga ühenduse loomisel
[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

Kui rakendus loob Azure SQL-andmebaasi, kuvatakse järgmine tõrketeade:

```
Error code 40613: "Database <x> on server <y> is not currently available. Please retry the connection later. If the problem persists, contact customer support, and provide them the session tracing ID of <z>"
```

> [AZURE.NOTE] See tõrketeade kuvatakse on tavaliselt siirdamiseks (lühiajaline).

See tõrge ilmneb juhul, kui Azure'i andmebaas on teisaldatud (või uuesti) ja rakenduse lähevad kaotsi selle ühenduse SQL-andmebaasiga. SQL-i andmebaasi ümberkonfigureerimine sündmuste põhjuseks kavandatud sündmuse (nt tarkvara versiooniuuenduse) või sündmuse planeerimata (nt protsessi krahh, või koormusetasakaalustuseks). Enamik ümberkonfigureerimine sündmused on tavaliselt lühiajaline ja peaks olema lõpule viidud alla 60 sekundi kõige. Kuid need sündmused aeg-ajalt võib võtta enam lõpetada, näiteks kui suure tehingu põhjustab pikaajalisi taastamine.

## <a name="steps-to-resolve-transient-connectivity-issues"></a>Siirdamiseks ühenduvuse probleemide lahendamiseks
1.  Märkige ruut [Microsoft Azure'i teenus armatuurlaua](https://azure.microsoft.com/status) teadaolevad katkestuste ilmnes ajal, mil esines rakenduse tõrked.
2. Rakendused, mis luua pilveteenusesse, näiteks Azure'i SQL-andmebaasi tuleks oodata perioodiliste ümberkonfigureerimine sündmused ja rakendada uuestiproovimise loogikale nende vigade asemel kaetud neid rakenduse tõrked kasutajatele. Vaadake üle jaotise [siirdamiseks tõrgete](sql-database-connectivity-issues.md) ja head tavad ja juhiseid [SQL-i andmebaasi arengu ülevaade](sql-database-develop-overview.md) lisateabe saamiseks ja üldine proovige strateegiad kujundus. Seejärel lugege teemat koodinäiteid veebisaidil [Andmeühenduste teegid SQL-andmebaas ja SQL serveri](sql-database-libraries.md) täpsemalt.
3.  Kui andmebaasi selle ressursi piirangud, saate tundub, et võrguühenduse ajutine probleem. Vaadake teemat [jõudlusega seotud probleemide tõrkeotsing](sql-database-troubleshoot-performance.md).
4.  Kui ühendusprobleemide jätkata, või kui tõrge tekkis rakenduse kestus ületab 60 sekundi järel või kuvatakse tõrge kordumist iga päev, faili, valides **Saada toetab** saidil [Azure tugiteenuste](https://azure.microsoft.com/support/options) Azure'i tugiteenuse taotluse.

## <a name="next-steps"></a>Järgmised sammud
- Kui kuvatakse tõrketeade eri, väärtustada [tõrketeade](sql-database-develop-error-messages.md) näpunäiteid põhjuse kohta.
- Kui probleem püsib, külastage [levinud ühenduse tõrkeotsing Azure SQL-andmebaasiga](sql-database-troubleshoot-common-connection-issues.md)juhiseid.
