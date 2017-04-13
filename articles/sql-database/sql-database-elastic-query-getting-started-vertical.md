<properties
    pageTitle="Alustamine (vertikaalne eraldamine) rist andmebaasipäringud | Microsoft Azure'i"   
    description="elastne andmebaasi päringu kasutamine vertikaalselt liigendatud andmebaasid"
    services="sql-database"
    documentationCenter=""  
    manager="jhubbard"
    authors="torsteng"/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/23/2016"
    ms.author="torsteng" />

# <a name="get-started-with-cross-database-queries-vertical-partitioning-preview"></a>Alustamine rist andmebaasipäringud (vertikaalne eraldamine) (eelvaade)

Azure'i SQL-andmebaasi elastne andmebaasi päringu (eelvaade) võimaldab T-SQL-päringud, mitut andmebaasi kasutamise ühe ühenduspunkt ulatuvad käivitada. See teema kehtib [vertikaalselt liigendatud andmebaasid](sql-database-elastic-query-vertical-partitioning.md).  

Kui valmis, siis: saate teada, kuidas konfigureerida ja Azure SQL-andmebaasi abil saate teha päringuid, mitme seotud andmebaase ulatuvad. 

Elastne andmebaasi päringu funktsiooni kohta lisateabe saamiseks leiate [Azure'i SQL-andmebaasi elastne andmebaasi päringu ülevaade](sql-database-elastic-query-overview.md). 

## <a name="create-the-sample-databases"></a>Näidisettevõtte andmebaasi loomine

Kõigepealt tuleb luua kaks andmebaase, **Kliendid** ja **tellimused**, kas samas või mõnes muus loogilise serverid.   

Käivitada **tellimused** andmebaasi **OrderInformation** tabeli loomine ja sisestusmeetodi Näidisandmete järgmistele küsimustele. 

    CREATE TABLE [dbo].[OrderInformation]( 
        [OrderID] [int] NOT NULL, 
        [CustomerID] [int] NOT NULL 
        ) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (123, 1) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (149, 2) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (857, 2) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (321, 1) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (564, 8) 

Nüüd käivitada pärast päringu **klientide** andmebaasi **CustomerInformation** tabeli loomine ja sisestada näidisandmeid. 

    CREATE TABLE [dbo].[CustomerInformation]( 
        [CustomerID] [int] NOT NULL, 
        [CustomerName] [varchar](50) NULL, 
        [Company] [varchar](50) NULL 
        CONSTRAINT [CustID] PRIMARY KEY CLUSTERED ([CustomerID] ASC) 
    ) 
    INSERT INTO [dbo].[CustomerInformation] ([CustomerID], [CustomerName], [Company]) VALUES (1, 'Jack', 'ABC') 
    INSERT INTO [dbo].[CustomerInformation] ([CustomerID], [CustomerName], [Company]) VALUES (2, 'Steve', 'XYZ') 
    INSERT INTO [dbo].[CustomerInformation] ([CustomerID], [CustomerName], [Company]) VALUES (3, 'Lylla', 'MNO') 

## <a name="create-database-objects"></a>Andmebaasiobjektide loomine
### <a name="database-scoped-master-key-and-credentials"></a>Andmebaasi veebirakendust juhtslaidi võti ja mandaadi

1. Avage SQL Server Management Studio või SQL Server Data Tools Visual Studios.
2. Tellimuste andmebaasiga ühenduse loomiseks ja käivitada T-SQL-i järgmised käsud:

        CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<password>'; 
        CREATE DATABASE SCOPED CREDENTIAL ElasticDBQueryCred 
        WITH IDENTITY = '<username>', 
        SECRET = '<password>';  

    "Kasutajanimi" ja "parooli" peaks olema kasutajanimi ja parool, mida kasutatakse klientide andmebaasi sisselogimiseks.
    Azure Active Directory abil elastne päringute autentimine on praegu ei toetata.

### <a name="external-data-sources"></a>Väliste andmeallikatega
Luua välise andmeallikaga, käivitada andmebaasi tellimused järgmine käsk: 

    CREATE EXTERNAL DATA SOURCE MyElasticDBQueryDataSrc WITH 
        (TYPE = RDBMS, 
        LOCATION = '<server_name>.database.windows.net', 
        DATABASE_NAME = 'Customers', 
        CREDENTIAL = ElasticDBQueryCred, 
    ) ;

### <a name="external-tables"></a>Välise tabeleid
Välise tabeli loomine tellimused andmebaasi, mis vastab CustomerInformation tabeli määratlus:

    CREATE EXTERNAL TABLE [dbo].[CustomerInformation] 
    ( [CustomerID] [int] NOT NULL, 
      [CustomerName] [varchar](50) NOT NULL, 
      [Company] [varchar](50) NOT NULL) 
    WITH 
    ( DATA_SOURCE = MyElasticDBQueryDataSrc) 

## <a name="execute-a-sample-elastic-database-t-sql-query"></a>Valimi elastne andmebaasi T-SQL-päringu käivitada

Kui olete määratlenud oma välise andmeallikaga ja oma välise tabeleid abil saate nüüd T-SQL-i päringu oma välise tabeleid. Päringut andmebaasi tellimused: 

    SELECT OrderInformation.CustomerID, OrderInformation.OrderId, CustomerInformation.CustomerName, CustomerInformation.Company 
    FROM OrderInformation 
    INNER JOIN CustomerInformation 
    ON CustomerInformation.CustomerID = OrderInformation.CustomerID 

## <a name="cost"></a>Maksumus

Praegu funktsiooni elastne andmebaasi päring on kaasatud kulu Azure'i SQL-andmebaasi.  

Hinnad teavet teemast [SQL-andmebaasi hinnad](/pricing/details/sql-database). 


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->

<!--anchors-->
