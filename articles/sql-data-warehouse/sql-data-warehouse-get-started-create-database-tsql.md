<properties
   pageTitle="Luua SQL-i andmebaas TSQL | Microsoft Azure'i"
   description="Siit saate teada, kuidas luua TSQL Azure SQL-i andmebaas"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""
   tags="azure-sql-data-warehouse"/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/24/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# <a name="create-a-sql-data-warehouse-database-by-using-transact-sql-tsql"></a>SQL-i andmebaas andmebaasi loomine Transact-SQL-i (TSQL) abil

> [AZURE.SELECTOR]
- [Azure'i portaal](sql-data-warehouse-get-started-provision.md)
- [TSQL](sql-data-warehouse-get-started-create-database-tsql.md)
- [PowerShelli](sql-data-warehouse-get-started-provision-powershell.md)

Selles artiklis kirjeldatakse, kuidas luua SQL-i andmebaas T-SQL-i abil.

## <a name="prerequisites"></a>Eeltingimused

Enne alustamist peate. 

- **Azure'i konto**: külastage [Azure'i tasuta prooviversiooni][] või [MSDN-i Azure'i][] konto loomiseks.
- **Azure SQL-i server**: vt [loomine Azure'i SQL-andmebaasi loogilise server Azure'i portaalis][] või [Loo Azure'i SQL-andmebaasi loogilise serveri PowerShelliga][] üksikasju.
- **Ressursirühm**: kasutada ühte ressursirühma Azure SQL Server või vaadake, [Kuidas luua ressursirühma][].
- **Keskkonna T-SQL-i käivitada**: saate [Visual Studio][Installing Visual Studio and SSDT], [sqlcmd][]või [SSMS][] T-SQL-i käivitada.

> [AZURE.NOTE] Loomine SQL-i andmebaas võib kaasa tuua uus arveldatavate teenus.  Hinnad kohta lisateabe saamiseks vt [SQL-i andmebaas hinnad][] .

## <a name="create-a-database-with-visual-studio"></a>Visual Studio andmebaasi loomine

Kui olete kasutanud Visual Studios, leiate artiklist [Päringu Azure SQL-i andmebaas (Visual Studio)][].  Alustamiseks avage SQL serveri objekti Exploreri Visual Studio ja SQL-i andmebaas andmebaasi majutava serveriga ühendust luua.  Kui ühendus on loodud, saate luua SQL-i andmebaas, käivitades järgmise SQL-käsu **juhtslaidi** andmebaasist.  See käsk loob andmebaasi MySqlDwDb teenuse eesmärk DW400 ja luba andmebaasi kasvada veeruga 10 TB.

```sql
CREATE DATABASE MySqlDwDb COLLATE SQL_Latin1_General_CP1_CI_AS (EDITION='datawarehouse', SERVICE_OBJECTIVE = 'DW400', MAXSIZE= 10240 GB);
```

## <a name="create-a-database-with-sqlcmd"></a>Koos sqlcmd andmebaasi loomine

Teise võimalusena võite käivitada sama käsu koos sqlcmd käitades järgmine käsk.

```sql
sqlcmd -S <Server Name>.database.windows.net -I -U <User> -P <Password> -Q "CREATE DATABASE MySqlDwDb COLLATE SQL_Latin1_General_CP1_CI_AS (EDITION='datawarehouse', SERVICE_OBJECTIVE = 'DW400', MAXSIZE= 10240 GB)"
```

Vaikimisi võrdlemine kui määramata on EKSEMPLARHAAVAL SQL_Latin1_General_CP1_CI_AS.  Funktsiooni `MAXSIZE` võib olla kuni 250 GB 240 TB.  Funktsiooni `SERVICE_OBJECTIVE` DW100 ja DW2000 vahel võib olla [DWU][].  Kõik lubatud väärtuste loendi leiate dokumentatsioonist MSDN-i andmebaasi [Loomine][].  T-SQL-käsu [Muuda andmebaasi][] abil saate muuta nii MAXSIZE ja SERVICE_OBJECTIVE.  Andmebaasi võrdlemine ei saa pärast loomist muuta.   Peaksite olema ettevaatlik muutmine SERVICE_OBJECTIVE mis DWU muutmine põhjustab uuesti teenuste, mis tühistab kõik päringute lennureisi.  Muutmise MAXSIZE taaskäivitage teenuste, kui see on ainult metaandmete lihtsa toimingu.

## <a name="next-steps"></a>Järgmised sammud

Kui teie SQL-i andmebaas on lõpule jõudnud, saate ettevalmistamise saate [laadimine Näidisandmete][] või kontrollida, kuidas [töötada][], [Laadi][]või [migreerida][].

<!--Article references-->
[DWU]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units
[how to create a SQL Data Warehouse from the Azure portal]: sql-data-warehouse-get-started-provision.md
[Päringu SQL Azure'i andmebaas (Visual Studio)]: sql-data-warehouse-query-visual-studio.md
[migreerimine]: sql-data-warehouse-overview-migrate.md
[töötada]: sql-data-warehouse-overview-develop.md
[Laadi]: sql-data-warehouse-overview-load.md
[Laadi näidisandmed]: sql-data-warehouse-load-sample-databases.md
[Luua Azure'i SQL-andmebaasi loogilise server Azure'i portaalis]: ../sql-database/sql-database-get-started.md#create-an-azure-sql-database-logical-server
[Luua Azure'i SQL-andmebaasi loogilise server PowerShelli abil]: ../sql-database/sql-database-get-started-powershell.md#database-setup-create-a-resource-group-server-and-firewall-rule
[Kuidas luua ressursirühma]: ../resource-group-template-deploy-portal.md#create-resource-group
[Installing Visual Studio and SSDT]: sql-data-warehouse-install-visual-studio.md
[sqlcmd]: sql-data-warehouse-get-started-connect-sqlcmd.md

<!--MSDN references--> 
[ANDMEBAASI LOOMINE]: https://msdn.microsoft.com/library/mt204021.aspx
[MUUDA ANDMEBAASI]: https://msdn.microsoft.com/library/mt204042.aspx
[SSMS]: https://msdn.microsoft.com/library/mt238290.aspx

<!--Other Web references-->
[SQL-i andmebaas hinnad]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
[Azure'i tasuta prooviversioon]: https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F
[MSDN-i Azure krediiti]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F
