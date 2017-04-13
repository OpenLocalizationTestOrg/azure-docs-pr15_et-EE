<properties
   pageTitle="SQL-i andmebaas PowerShelli abil luua | Microsoft Azure'i"
   description="PowerShelli abil luua SQL-andmebaas"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/25/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# <a name="create-sql-data-warehouse-using-powershell"></a>Looge PowerShelli kaudu SQL-i andmebaas

> [AZURE.SELECTOR]
- [Azure'i portaal](sql-data-warehouse-get-started-provision.md)
- [TSQL](sql-data-warehouse-get-started-create-database-tsql.md)
- [PowerShelli](sql-data-warehouse-get-started-provision-powershell.md)

Selles artiklis kirjeldatakse, kuidas luua PowerShelli kaudu SQL-i andmebaas.

## <a name="prerequisites"></a>Eeltingimused

Enne alustamist peate.

- **Azure'i konto**: külastage [Azure'i tasuta prooviversiooni][] või [MSDN-i Azure'i][] konto loomiseks.
- **Azure SQL-i server**: vt [loomine Azure'i SQL-andmebaasi loogilise server Azure'i portaalis][] või [Loo Azure'i SQL-andmebaasi loogilise serveri PowerShelliga][] üksikasju.
- **Ressursirühm**: kasutada ühte ressursirühma Azure SQL Server või vaadake, [Kuidas luua ressursirühma][].
- **PowerShelli versioon 1.0.3 või suurem**: saate vaadata oma versiooni, käivitades **Get-moodul - ListAvailable-nime Azure'i**.  Uusima versiooni saate installida [Microsoft Web platvormi Installer][].  Uusima versiooni installimise kohta lisateabe saamiseks vaadake, [Kuidas installida ja konfigureerida Azure PowerShelli][].

> [AZURE.NOTE] Loomine SQL-i andmebaas võib kaasa tuua uus arveldatavate teenus.  Hinnad kohta lisateabe saamiseks vt [SQL-i andmebaas hinnad][] .

## <a name="create-a-sql-data-warehouse"></a>Looge SQL-i andmebaas

1. Avage Windows PowerShelli.
2. Login Azure'i ressursihaldur käivitada.

    ```Powershell
    Login-AzureRmAccount
    ```
    
3. Valige tellimus, mida soovite kasutada oma praeguse seansi jaoks.

    ```Powershell
    Get-AzureRmSubscription -SubscriptionName "MySubscription" | Select-AzureRmSubscription
    ```

4.  Andmebaasi loomine. Selles näites luuakse andmebaasi nimega "mynewsqldw" teenuse eesmärk taseme "DW400" nimega "sqldwserver1", mis on ressursi rühma nimega "mywesteuroperesgp1" serveriga.

    ```Powershell
    New-AzureRmSqlDatabase -RequestedServiceObjectiveName "DW400" -DatabaseName "mynewsqldw" -ServerName "sqldwserver1" -ResourceGroupName "mywesteuroperesgp1" -Edition "DataWarehouse" -CollationName "SQL_Latin1_General_CP1_CI_AS" -MaxSizeBytes 10995116277760
    ```

Nõutav parameetrid on:

- **RequestedServiceObjectiveName**: [DWU][] taotlemise summa.  Toetatud väärtused: DW100, DW200, DW300, DW400, DW500, DW600, DW1000, DW1200, DW1500, DW2000, DW3000 ja DW6000.
- **DatabaseName**: SQL-i andmebaas, mida loote nime.
- **Serveri nimi**: loomiseks kasutatava serveri nimi (peab olema V12).
- **ResourceGroupName**: kasutate ressursirühma.  Saadaval ressursi leidmiseks kasutada rühmade tellimuse Get-AzureResource.
- **Versioon**: peab olema "Andmelao" loomine SQL-i andmebaas.

Valikulised parameetrid on:

- **CollationName**: vaikimisi võrdlemine kui määramata on SQL_Latin1_General_CP1_CI_AS.  Andmebaasi ei saa muuta võrdlemine.
- **MaxSizeBytes**: andmebaasi vaikimisi maksimaalne maht on 10 GB.


Parameetri valikute kohta leiate lisateavet teemast [New-AzureRmSqlDatabase][] ja [Andmebaasi loomine (Azure SQL-i andmebaas)][].

## <a name="next-steps"></a>Järgmised sammud

Kui teie SQL-i andmebaas on lõpule jõudnud, ettevalmistamise võite proovida [valimi andmete laadimine][] või kontrollida, kuidas [töötada][], [Laadi][]või [migreerimine][].

Kui olete huvitatud rohkem kohta, kuidas hallata SQL-i andmebaas programmiliselt, vaadake meie artikkel [PowerShelli cmdlet-käskude ja REST API-de][]kasutamine.

<!--Image references-->

<!--Article references-->
[DWU]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units
[migreerimine]: ./sql-data-warehouse-overview-migrate.md
[töötada]: ./sql-data-warehouse-overview-develop.md
[Laadi]: ./sql-data-warehouse-load-with-bcp.md
[Näidisandmete laadimine]: ./sql-data-warehouse-load-sample-databases.md
[PowerShelli cmdlet-käskude ja REST API-d]: ./sql-data-warehouse-reference-powershell-cmdlets.md
[firewall rules]: ../sql-database-configure-firewall-settings.md

[Kuidas installida ja konfigureerida Azure PowerShell]: ../powershell/powershell-install-configure.md
[how to create a SQL Data Warehouse from the Azure Portal]: ./sql-data-warehouse-get-started-provision.md
[Luua Azure'i SQL-andmebaasi loogilise server Azure'i portaalis]: ../sql-database/sql-database-get-started.md#create-an-azure-sql-database-logical-server
[Luua Azure'i SQL-andmebaasi loogilise server PowerShelli abil]: ../sql-database/sql-database-get-started-powershell.md#database-setup-create-a-resource-group-server-and-firewall-rule
[Kuidas luua ressursirühma]: ../resource-group-template-deploy-portal.md#create-resource-group

<!--MSDN references--> 
[MSDN]: https://msdn.microsoft.com/library/azure/dn546722.aspx
[Uue AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619339.aspx
[Luua andmebaasi (SQL Azure'i andmebaas)]: https://msdn.microsoft.com/library/mt204021.aspx

<!--Other Web references-->
[Microsoft Web platvormi Installer]: https://aka.ms/webpi-azps
[SQL-i andmebaas hinnad]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
[Azure'i tasuta prooviversioon]: https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F
[MSDN-i Azure krediiti]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F
