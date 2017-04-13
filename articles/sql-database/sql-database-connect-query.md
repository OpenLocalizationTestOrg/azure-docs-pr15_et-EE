<properties
    pageTitle="Ühenduse loomine SQL-andmebaasi C# päringu abil | Microsoft Azure'i"
    description="Kirjutage programmi C# päringu ja SQL-andmebaasiga ühenduse loomiseks. Teave IP aadressid, ühendusstringi, turvalist sisselogimine ja tasuta Visual Studio kohta."
    services="sql-database"
    keywords="c# andmebaasi päringu, c# query andmebaasiga ühenduse loomiseks, SQL-i C#"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="get-started-article"
    ms.date="08/17/2016"
    ms.author="stevestein"/>



# <a name="connect-to-a-sql-database-with-visual-studio"></a>Ühenduse loomine SQL-andmebaasi Visual Studio abil

> [AZURE.SELECTOR]
- [Visual Studio](sql-database-connect-query.md)
- [SSMS](sql-database-connect-query-ssms.md)
- [Exceli](sql-database-connect-excel.md)

Saate teada, kuidas Visual Studio Azure SQL-andmebaasiga ühendust luua. 

## <a name="prerequisites"></a>Eeltingimused


Ühenduse loomine SQL-andmebaasiga Visual Studio abil, peate järgmist: 


- Ühenduse loomine SQL-andmebaasiga. Selles artiklis kasutab näidisandmebaasi **AdventureWorks** . Näidisandmebaasi AdventureWorks saamiseks vaadake [demo andmebaasi loomine](sql-database-get-started.md).


- Visual Studio 2013 värskenduse 4 (või uuem versioon). Nüüd pakub Microsoft Visual Studio ühenduse jaoks *tasuta*.
 - [Visual Studio ühenduse allalaadimine](http://www.visualstudio.com/products/visual-studio-community-vs)
 - [Veel suvandeid tasuta Visual Studio](http://www.visualstudio.com/products/free-developer-offers-vs.aspx)




## <a name="open-visual-studio-from-the-azure-portal"></a>Avatud Visual Studio Azure'i portaalis


1. [Azure'i portaali](https://portal.azure.com/)sisse logida.

2. Klõpsake nuppu **Rohkem teenuseid** > **SQL andmebaase**
3. Avage **AdventureWorks** andmebaasi tera asukoha ja klõpsates *AdventureWorks* andmebaas.

6. Andmebaasi tera ülaosas nuppu **Tööriistad** :

    ![Uus päring. Ühenduse loomine SQL-andmebaasi server: SQL Server Management Studio](./media/sql-database-connect-query/tools.png)

7. Klõpsake nuppu **Ava Visual Studios** (kui teil on vaja Visual Studio, klõpsake linki allalaadimine):

    ![Uus päring. Ühenduse loomine SQL-andmebaasi server: SQL Server Management Studio](./media/sql-database-connect-query/open-in-vs.png)


8. Visual Studio avaneb aken **ühenduse Server** juba määratud ühenduse server ja andmebaas valitud portaalis.  (Klõpsake nuppu **Suvandid** , kinnitamaks, et ühendus on määratud õige andmebaasi.) Tippige serveri administraatori parool ja klõpsake nuppu **Ühenda**.


    ![Uus päring. Ühenduse loomine SQL-andmebaasi server: SQL Server Management Studio](./media/sql-database-connect-query/connect.png)


8. Kui teil on tulemüüri reegli seada üles oma arvuti IP-aadressi, kuvatakse teade *ei saa ühendust luua* siin. Tulemüüri reegli loomiseks vaadake teemat [reegli Azure'i SQL-andmebaasi server taseme tulemüüri konfigureerimine](sql-database-configure-firewall-settings.md).


9. Pärast ühenduse loomist, avaneb **SQL serveri objekti Exploreri** aknas andmebaasi ühendus.

    ![Uus päring. Ühenduse loomine SQL-andmebaasi server: SQL Server Management Studio](./media/sql-database-connect-query/sql-server-object-explorer.png)


## <a name="run-a-sample-query"></a>Valimi päringu käitamine

Nüüd kus oleme ühendatud andmebaasi, Kuva järgmist käivitamise lihtsa päringu:

2. Paremklõpsake andmebaas ja seejärel valige **Uus päring**.

    ![Uus päring. Ühenduse loomine SQL-andmebaasi server: SQL Server Management Studio](./media/sql-database-connect-query/new-query.png)

3. Päringuakna, kopeerige ja kleepige järgmine kood.

        SELECT
        CustomerId
        ,Title
        ,FirstName
        ,LastName
        ,CompanyName
        FROM SalesLT.Customer;

4. Klõpsake päringu käivitamiseks nuppu **Käivita** .

    ![Edu. Ühenduse loomine SQL-andmebaasi server: SVisual Studio](./media/sql-database-connect-query/run-query.png)

## <a name="next-steps"></a>Järgmised sammud

- SQL Server Data Tools avada SQL andmebaase Visual Studio kasutab. Lisateavet leiate teemast [SQL Server Data Tools](https://msdn.microsoft.com/library/hh272686.aspx).
- Ühenduse loomine SQL-andmebaasiga, kasutades koodi, vt [ühenduse SQL-andmebaasiga, kasutades .NET (C#)](sql-database-develop-dotnet-simple.md).



