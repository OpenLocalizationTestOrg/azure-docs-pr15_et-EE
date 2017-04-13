<properties
   pageTitle="Luua andmebaas SQL Azure'i portaalis | Microsoft Azure'i"
   description="Saate teada, kuidas luua Azure'i portaalis Azure SQL-i andmebaas"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="jhubbard"
   editor=""
   tags="azure-sql-data-warehouse"/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/25/2016"
   ms.author="barbkess;lodipalm;sonyama"/>

# <a name="create-an-azure-sql-data-warehouse"></a>Saate luua ka SQL Azure'i andmebaas

> [AZURE.SELECTOR]
- [Azure'i portaal](sql-data-warehouse-get-started-provision.md)
- [TSQL](sql-data-warehouse-get-started-create-database-tsql.md)
- [PowerShelli](sql-data-warehouse-get-started-provision-powershell.md)

Selle õpetuse kasutab Azure portaali loomine SQL-i andmebaas, mis sisaldab ka AdventureWorksDW Näidisettevõtte andmebaas.


## <a name="prerequisites"></a>Eeltingimused

Enne alustamist peate.

- **Azure'i konto**: külastage [Azure'i tasuta prooviversiooni][] või [MSDN-i Azure'i][] konto loomiseks.
- **Azure SQL-i server**: üksikasjalikumat teavet leiate [Azure'i SQL-andmebaasi loogilise server Azure'i portaalis loomine][] .

> [AZURE.NOTE] Uue arveldatavate teenuse võib põhjustada loomine SQL-i andmebaas.  Lisateabe saamiseks vt [SQL-i andmebaas hinnad][] .

## <a name="create-a-sql-data-warehouse"></a>Looge SQL-i andmebaas

1. [Azure'i portaali](https://portal.azure.com)sisse logida.

2. Valige **+ Uus** > **andmete + salvestusruumi** > **SQL-i andmebaas**.

    ![Loomine](./media/sql-data-warehouse-get-started-provision/create-sample.gif)

3. **SQL-i andmebaas** labale täide teave vaja ja seejärel vajutage Loo loomine.

    ![Andmebaasi loomine](./media/sql-data-warehouse-get-started-provision/create-database.png)

    - **Server**: soovitame teil valida oma serveri esmalt.  

    - **Andmebaasi nimi**: nimi, mida kasutatakse viidata SQL-i andmebaas.  See peab olema kordumatu serveriga.
    
    - **Jõudluse**: soovitame, alustades 400 [DWUs][DWU]. Saate liigutage liugurit vasakule või paremale kohandada oma andmebaas jõudlus või mastaapimiseks pärast loomist üles või alla.  DWUs kohta lisateabe saamiseks lugege teemat oma dokumentatsiooni [skaleerimist](./sql-data-warehouse-manage-compute-overview.md) või meie [hinnad lehel][SQL-i andmebaas hinnad]. 

    - **Tellimus**: valige [tellimus] , mis see SQL-i andmebaas arve soovite.

    - **Ressursirühm**: [Ressursirühma] [ Resource group] on aitab teil haldamiseks Azure ressursse. Lisateavet [Ressursi rühmade](../azure-resource-manager/resource-group-overview.md)kohta.

    - **Valige allikas**: klõpsake raadionuppu **Vali tulemiallikas** > **valimi**. Azure'i kuvab automaatselt **valimi** koos AdventureWorksDW suvand.

> [AZURE.NOTE] Vaikimisi võrdlemine jaoks SQL-i andmebaas on SQL_Latin1_General_CP1_CI_AS. Erinevate võrdlemine vajadusel saab luua andmebaasi erinevate võrdlemine [T-SQL-i][] .

4. Klõpsake nuppu **Loo** luua oma SQL-andmebaas.

5. Oodake mõni minut. Kui teie andmebaas on valmis, tuleks tagastada [Azure portaali](https://portal.azure.com). Leiate oma SQL-i andmebaas armatuurlauale loetletud jaotises SQL-andmebaase või ressursirühm, mida kasutasite selle loomiseks. 

    ![portaali vaade](./media/sql-data-warehouse-get-started-provision/database-portal-view.png)

[AZURE.INCLUDE [SQL Database create server](../../includes/sql-database-create-new-server-firewall-portal.md)] 

## <a name="next-steps"></a>Järgmised sammud

Nüüd, kui SQL-i andmebaas on loodud, on valmis [ühendamine](./sql-data-warehouse-connect-overview.md) ja alustage päringud.

Andmete laadimine SQL-i andmebaas, lugege teemat [laadimine ülevaade](./sql-data-warehouse-overview-load.md).

Kui soovite olemasoleva andmebaasi migreerimine SQL-i andmebaas, [migreerimise ülevaade](./sql-data-warehouse-overview-migrate.md) või kasutage [Migreerimise kasuliku](./sql-data-warehouse-migrate-migration-utility.md).

Tulemüüri reeglid konfigureerida Transact-SQL-i abil. Lisateavet leiate teemast [sp_set_firewall_rule][] ja [sp_set_database_firewall_rule][].

Samuti on hea mõte vaadata [parimaid tavasid][].

<!--Article references-->
[Luua Azure'i SQL-andmebaasi loogilise server Azure'i portaalis]: ../sql-database/sql-database-get-started.md#create-an-azure-sql-database-logical-server
[Create an Azure SQL Database logical server with PowerShell]: ../sql-database/sql-database-get-started-powershell.md#database-setup-create-a-resource-group-server-and-firewall-rule
[resource groups]: ../resource-group-template-deploy-portal.md
[Head tavad]: sql-data-warehouse-best-practices.md
[DWU]: sql-data-warehouse-overview-what-is.md#data-warehouse-units
[tellimuse]: ../azure-glossary-cloud-terminology.md#subscription
[resource group]: ../azure-glossary-cloud-terminology.md#resource-group
[T-SQL-IS]: ./sql-data-warehouse-get-started-create-database-tsql.md
 
<!--MSDN references-->
[sp_set_firewall_rule]: https://msdn.microsoft.com/library/dn270017.aspx
[sp_set_database_firewall_rule]: https://msdn.microsoft.com/library/dn270010.aspx

<!--Other Web references-->
[SQL-i andmebaas hinnad]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
[Azure'i tasuta prooviversioon]: https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F
[MSDN-i Azure krediiti]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F

