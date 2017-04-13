<properties
   pageTitle="SQL Azure'i andmebaas (REST API) taastamine | Microsoft Azure'i"
   description="REST API tööülesannete taastamiseks Azure SQL-i andmebaas."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="Lakshmi1812"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/21/2016"
   ms.author="lakshmir;barbkess;sonyama"/>

# <a name="restore-an-azure-sql-data-warehouse-rest-api"></a>SQL Azure'i andmebaas (REST API) taastamine

> [AZURE.SELECTOR]
- [Ülevaade][]
- [Portaal][]
- [PowerShelli][]
- [ÜLEJÄÄNUD][]

Sellest artiklist saate teada, kuidas taastada Azure SQL-i andmebaas REST API abil.

## <a name="before-you-begin"></a>Enne alustamist

**Kontrollige oma DTU võimsus.** SQL server (nt myserver.database.windows.net), mis on vaikimisi DTU kvoodi haldab iga SQL-andmebaas.  Enne taastamist SQL-i andmebaas, kontrollige, kas selle SQL serveris on piisavalt ülejäänud DTU piirmäära taastatakse andmebaas. Saate teada, kuidas arvutada DTU vaja või paluge rohkem DTU, lugege teemat [DTU kvoodi muudatuse taotlemine][].

## <a name="restore-an-active-or-paused-database"></a>On aktiivne või peatatud andmebaasi taastamine

Andmebaasi taastamine

1. Andmebaasi taastamine punktide toomine andmebaasi taastamine punktide selle toimingu abil soovitud loendi saamiseks.
2. Alustage oma taastamine [loomine andmebaasi taastamine taotluse][] selle toimingu abil.
3. Jälgida olekut oma taastamine [andmebaasi toimingu olek][] selle toimingu abil.

>[AZURE.NOTE] Kui taastamine on lõpule jõudnud, saate konfigureerida, järgmised [konfigureerimine andmebaasi taastamine pärast][]taastatud andmebaasi.

## <a name="restore-a-deleted-database"></a>Kustutatud andmebaasi taastamine

Kustutatud andmebaasi taastamine

1.  Loetle kõik teie kustutatud tagastatav andmebaaside [loendis tagastatav kaotatud andmebaaside][] selle toimingu abil.
2.  Soovite [saada tagastatav kaotatud andmebaasi][] toimingu abil taastada kustutatud andmebaasi üksikasjade vaatamiseks.
3.  Alustage oma taastamine [loomine andmebaasi taastamine taotluse][] selle toimingu abil.
4.  Jälgida olekut oma taastamine [andmebaasi toimingu olek][] selle toimingu abil.

>[AZURE.NOTE] Andmebaasi konfigureerimine, kui taastamine on lõpule jõudnud, leiate [oma andmebaasi taastamine pärast konfigureerimine][]. 


## <a name="next-steps"></a>Järgmised sammud
Lisateavet Azure'i SQL-andmebaasi väljaannete äri järjepidevus funktsioonid, lugege [Azure'i SQL-andmebaasi äri järjepidevuse ülevaade][].

<!--Image references-->

<!--Article references-->
[Azure'i SQL-andmebaasi äri järjepidevuse ülevaade]: ./sql-database-business-continuity.md
[DTU kvoodi muudatuse taotlemine]: ./sql-data-warehouse-get-started-create-support-ticket.md#request-quota-change
[Andmebaasi taastamine pärast konfigureerimine]: ./sql-database-disaster-recovery.md#configure-your-database-after-recovery
[How to install and configure Azure PowerShell]: ./powershell-install-configure.md
[Ülevaade]: ./sql-data-warehouse-restore-database-overview.md
[Portaal]: ./sql-data-warehouse-restore-database-portal.md
[PowerShelli]: ./sql-data-warehouse-restore-database-powershell.md
[ÜLEJÄÄNUD]: ./sql-data-warehouse-restore-database-rest-api.md

<!--MSDN references-->
[Andmebaasi taastamine koosolekukutse loomiseks]: https://msdn.microsoft.com/library/azure/dn509571.aspx
[Andmebaasi toimingu olek]: https://msdn.microsoft.com/library/azure/dn720371.aspx
[Tagastatav kaotatud andmebaasi hankimine]: https://msdn.microsoft.com/library/azure/dn509574.aspx
[Loendi tagastatav lähevad andmebaasid]: https://msdn.microsoft.com/library/azure/dn509562.aspx
[Restore-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt693390.aspx

<!--Other Web references-->
[Azure Portal]: https://portal.azure.com/
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps
