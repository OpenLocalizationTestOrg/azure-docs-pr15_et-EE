<properties
   pageTitle="Päringu SQL Azure'i andmebaas (Visual Studio) | Microsoft Azure'i"
   description="Päringu SQL-i andmebaas Visual Studio."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/16/2016"
   ms.author="sonyama;barbkess"/>

# <a name="query-azure-sql-data-warehouse-visual-studio"></a>Päringu SQL Azure'i andmebaas (Visual Studio)

> [AZURE.SELECTOR]
- [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
- [Azure'i masina õpetused](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
- [Visual Studio](sql-data-warehouse-query-visual-studio.md)
- [sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 

Visual Studio abil päringu SQL Azure'i andmebaas vaid paari minutiga. Seda meetodit kasutab Visual Studio laiend SQL serveri andmete tööriistad (SSDT). 

## <a name="prerequisites"></a>Eeltingimused

Selle õpetuse kasutamiseks on vaja:

+ Olemasoleva SQL-i andmebaas. Ühenduse loomiseks leiate teemast [loomine SQL-i andmebaas][].
+ Visual Studio SSDT. Kui teil on Visual Studios, teil ilmselt juba on järgmine. Installimisjuhiseid ja suvandite kohta leiate teemast [Visual Studio installimine ja SSDT][].
+ Täielikult kvalifitseeritud SQL serveri nimi. Leidmiseks, vt [ühenduse loomine SQL-i andmebaas][].

## <a name="1-connect-to-your-sql-data-warehouse"></a>1. ühenduse loomiseks oma SQL-andmebaas

1. Avage Visual Studio 2013 või 2015.
2. Avage SQL serveri objekti Explorer. Selleks valige **Kuva** > **SQL serveri objekti Explorer**.

    ![SQL serveri objekti Exploreri][1]

3. Klõpsake ikooni **Lisa SQL Server** .

    ![SQL serveri lisamine][2]

4. Täitke väljad ühenduse Server aken.

    ![Serveriga][3]

    - **Serveri nimi**. Sisestage **Serveri nimi** ei tuvastatud.
    - **Autentimist**. Valige **SQL serveri autentimine** või **Active Directory integreeritud autentimist**.
    - **Kasutajanimi** ja **parool**. Sisestage kasutajanimi ja parool, kui SQL serveri autentimine on valitud kohal.
    - Klõpsake nuppu **Loo ühendus**.

5. Laiendage uurida, Azure SQL serveris. Saate vaadata seotud server andmebaasid. Näidisettevõtte andmebaasi tabelite nägemiseks AdventureWorksDW laiendamine

    ![AdventureWorksDW uurimine][4]

## <a name="2-run-a-sample-query"></a>2. valimi päringu käitamine

Nüüd, kui ühendus on loodud andmebaasi, kirjutage loome päringu.

1. Paremklõpsake oma andmebaasi SQL serveri objekti Explorer.

2. Valige **uus päring**. Avaneb aken uus päring.

    ![Uus päring][5]

3. Kopeerige see TSQL päringu päringuakna.

    ```sql
    SELECT COUNT(*) FROM dbo.FactInternetSales;
    ```

4. Käivitage päring. Selleks klõpsake rohelist noolt või kasutada järgmist otseteed: `CTRL` + `SHIFT` + `E`.

    ![Käivita päring][6]

5. Vaadake päringu tulemused. Selles näites FactInternetSales tabelis on 60398 read.

    ![Päringu tulemused][7]

## <a name="next-steps"></a>Järgmised sammud

Nüüd, kui saate ühendada ja päringu, proovige [PowerBI andmete visualiseerimiseks][].

Keskkonna Azure Active Directory autentimise konfigureerimine, lugege teemat [autentimise abil SQL-i andmebaas][].

<!--Arcticles-->
[Ühenduse loomine SQL-andmebaas]: sql-data-warehouse-connect-overview.md
[Looge SQL-i andmebaas]: sql-data-warehouse-get-started-provision.md
[Visual Studio ja SSDT installimisel]: sql-data-warehouse-install-visual-studio.md
[Autentida SQL-andmebaas]: sql-data-warehouse-authentication.md
[PowerBI andmete visualiseerimine]: sql-data-warehouse-get-started-visualize-with-power-bi.md  

<!--Other-->
[Azure portal]: https://portal.azure.com

<!--Image references-->

[1]: media/sql-data-warehouse-query-visual-studio/open-ssdt.png
[2]: media/sql-data-warehouse-query-visual-studio/add-server.png
[3]: media/sql-data-warehouse-query-visual-studio/connection-dialog.png
[4]: media/sql-data-warehouse-query-visual-studio/explore-sample.png
[5]: media/sql-data-warehouse-query-visual-studio/new-query2.png
[6]: media/sql-data-warehouse-query-visual-studio/run-query.png
[7]: media/sql-data-warehouse-query-visual-studio/query-results.png
