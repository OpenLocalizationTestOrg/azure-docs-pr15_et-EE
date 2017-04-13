<properties
   pageTitle="Power BI Microsoft Azure SQL-i andmebaas andmete visualiseerimine"
   description="Power BI SQL-i andmebaas andmete visualiseerimine"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor="" />

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/16/2016"
   ms.author="lodipalm;barbkess;sonyama" />

# <a name="visualize-data-with-power-bi"></a>Power BI andmete visualiseerimine

> [AZURE.SELECTOR]
- [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
- [Azure'i masina õpetused](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
- [Visual Studio](sql-data-warehouse-query-visual-studio.md)
- [sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 

Selle õpetuse näidatakse, kuidas kasutada Power BI ühenduse SQL-i andmebaas ja luua mõni lihtsam visualiseering.

> [AZURE.VIDEO azure-sql-data-warehouse-sample-data-and-powerbi]

## <a name="prerequisites"></a>Eeltingimused

Samm selle õpetuse kaudu, peate.

- SQL-i andmebaas eelnevalt laaditud AdventureWorksDW andmebaas. See säte, lugege teemat [loomine SQL-i andmebaas][] ja valige Näidisandmete laadimiseks. Kui teil juba on andmebaas, kuid ei saa näidisandmed, saate [laadida Näidisandmete käsitsi][].


## <a name="1-connect-to-your-database"></a>1. andmebaasiga ühenduse

Avage Power BI ja oma AdventureWorksDW andmebaasiga ühenduse loomiseks tehke järgmist.

1. [Azure'i portaali][]sisse logida.
2. Klõpsake **SQL andmebaase** ja valige andmebaasi AdventureWorks SQL-i andmebaas.

    ![Otsige üles oma andmebaasi][1]

3. Klõpsake nuppu Ava Power BI.

    ![Power BI nupp][2]

4. Nüüd näete oma andmebaasi veebiaadress kuvamine lehel SQL-i andmebaas ühenduse. Klõpsake nuppu edasi.

    ![Power BI ühendus][3]

6. Sisestage oma Azure SQL serveri kasutajanimi ja parooli ja täielikult ühendatakse andmebaasi SQL-i andmebaas.

    ![Power BI sisselogimine][4]

7. Kui te Power BI sisse logitud, klõpsake vasakul enne AdventureWorksDW andmekomplekti. See avab andmebaasi.

    ![Power BI avada AdventureWorksDW][5]



## <a name="2-create-a-report"></a>2 aruande loomine

Nüüd olete valmis AdventureWorksDW valimi andmete analüüsimine Power BI abil. Analüüsi sooritamiseks on AdventureWorksDW vaade, mida nimetatakse AggregateSales. See vaade sisaldab mõned peamised mõõdikute analüüsimiseks ettevõtte müügi.

1. Parempoolse väljade paanil Müügisumma vastavalt sihtnumber, kaardil loomiseks klõpsake nuppu AggregateSales vaate laiendamiseks. Klõpsake valimiseks nende veergude PostalCode ja SalesAmount.

    ![Power BI valige AggregateSales][6]

    Power BI automaatselt tuvastab see on geograafilisi andmeid ja luua kaardi jaoks.

    ![Power BI MAPI][7]

2. Selles etapis tuleb loob tulpdiagramm, mis kuvatakse müügist kliendi tulu summa. Loomiseks avage see laiendatud AggregateSales vaade. Klõpsake välja SalesAmount. Lohistage väli kliendi tulu vasakule ja kukutage see telg.

    ![Power BI valige telg][8]

    Oleme liikunud lintdiagrammi üle vasakule.

    ![Power BI ribal][9]

3. Selles etapis tuleb loob rea diagramm, mis kuvatakse Müügisumma kohta tellimuse kuupäev. Loomiseks avage see laiendatud AggregateSales vaade. Klõpsake SalesAmount ja TellimuseKuupäev. Klõpsake veeru visualiseering joondiagramm ikooni; See on teises reas jaotises visualiseeringute esimest ikooni.

    ![Power BI valige joondiagramm][10]

    Nüüd saate aruande, mis näitab kolme erinevate visualiseeringute andmete.

    ![Power BI joon][11]

Edenemise saate igal ajal salvestada, klõpsates menüüd **fail** ja valides **salvestada**.

## <a name="next-steps"></a>Järgmised sammud
Nüüd kus oleme olete andnud teile soe näidisandmetega aega, lugege teemat Kuidas [töötada][], [Laadi][]või [migreerida][]. Või Heitke pilk [veebisaidil Power BI][].

<!--Image references-->
[1]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-find-database.png
[2]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-button.png
[3]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-connect-to-azure.png
[4]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-sign-in.png
[5]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-open-adventureworks.png
[6]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-aggregatesales.png
[7]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-map.png
[8]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-chooseaxis.png
[9]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-bar.png
[10]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-prepare-line.png
[11]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-line.png
[12]: media/sql-data-warehouse-get-started-visualize-with-power-bi/pbi-save.png

<!--Article references-->
[migreerimine]: sql-data-warehouse-overview-migrate.md
[töötada]: sql-data-warehouse-overview-develop.md
[Laadi]: sql-data-warehouse-overview-load.md
[Näidisandmete käsitsi laadimine]: sql-data-warehouse-load-sample-databases.md
[connecting to SQL Data Warehouse]: sql-data-warehouse-integrate-power-bi.md
[Looge SQL-i andmebaas]: sql-data-warehouse-get-started-provision.md

<!--Other-->
[Azure'i portaal]: https://portal.azure.com/
[Power BI veebisait]: http://www.powerbi.com/
