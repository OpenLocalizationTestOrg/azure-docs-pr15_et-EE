<properties
    pageTitle="Exceli ühenduse SQL-andmebaasi | Microsoft Azure'i"
    description="Saate teada, kuidas Microsoft Excel pilveteenuses Azure SQL-i andmebaasiga ühenduse loomiseks. Andmete importimine rakendusse Excel aruandlus ja andmete uurimine."
    services="sql-database"
    keywords="ühenduse loomine SQL excel, Exceli andmete importimine"
    documentationCenter=""
    authors="joseidz"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/05/2016"
    ms.author="joseidz"/>


# <a name="sql-database-tutorial-connect-excel-to-an-azure-sql-database-and-create-a-report"></a>SQL-andmebaasi õpetus: Azure'i SQL-andmebaasiga ühenduse Exceli ja aruande koostamine

> [AZURE.SELECTOR]
- [Visual Studio](sql-database-connect-query.md)
- [SSMS](sql-database-connect-query-ssms.md)
- [Exceli](sql-database-connect-excel.md)

Siit saate teada, kuidas ühendada Exceli SQL-andmebaasiga pilves, et saate andmed importida ja tabeleid ja diagramme koostada andmebaasi väärtuste põhjal. Selles õpetuses häälestamist Exceli ja andmebaasi tabelisse vahelise ühenduse, salvestage fail, mis salvestab andmed ja ühenduse teavet Excel ja seejärel looge pivot diagrammi andmebaasi väärtuste põhjal.

Enne alustamist peate SQL Azure'i andmebaasi. Kui teil pole ühte, leiate teemast [esimese SQL-andmebaasi loomine](sql-database-get-started.md) andmebaasi Näidisandmete üles ja töötab paar minutit. Selles artiklis kuvatakse Näidisandmete artiklist Excelisse importida, kuid sarnaseid juhiseid järgides saate oma andmete põhjal.

Peate Exceli eksemplaris. Selles artiklis kasutab [Microsoft Excel 2016](https://products.office.com/en-US/).

## <a name="connect-excel-to-a-sql-database-and-create-an-odc-file"></a>Ühenduse loomine SQL-andmebaasiga Exceli ja odc-faili loomine

1.  Exceli SQL-andmebaasiga ühendust luua, avage Excel ja uue töövihiku loomine või avage olemasolev Exceli töövihik.

2.  Menüüriba **andmete**klõpsake lehe ülaosas nuppu **Muudest allikatest**ja seejärel klõpsake käsku **SQL Serverist**.

    ![Valige andmeallikas: Exceli SQL-andmebaasiga ühenduse loomiseks.](./media/sql-database-connect-excel/excel_data_source.png)

    Avatakse andmeühendusviisard.

3.  Tippige dialoogiboksis **ühenduse loomine andmebaasiserveriga** ühenduse vormil soovitud SQL-andmebaasi **Serveri nimi** <*serverinimi*>**. database.windows.net**. Näiteks **adworkserver.database.windows.net**.

4.  **Mandaadi sisse logida**, klõpsake nuppu **Kasuta järgmist kasutajanime ja parooli**, tippige **Kasutajanime** ja **parooli** saate seadistada SQL-andmebaasi server kui lõite, ja seejärel klõpsake nuppu **edasi**.

    ![Sisestage serveri nimi ja sisselogimise identimisteave](./media/sql-database-connect-excel/connect-to-server.png)

    > [AZURE.TIP] Olenevalt teie keskkonnast ei saa ühendust luua või ühendus võivad kaotada, kui SQL-andmebaasi server ei luba liiklus kliendi IP-aadress. Avage [Azure portaali](https://portal.azure.com/)klõpsake SQL-i serverite oma serveri, klõpsake jaotises sätted nuppu tulemüüri ja lisada oma kliendi IP-aadress. Üksikasjad leiate [tulemüüri sätete konfigureerimine](sql-database-configure-firewall-settings.md) .

5. Valige dialoogiboksis **Valige andmebaas ja tabel** , töötamiseks loendist soovitud andmebaas ja klõpsake tabelite või vaadete, mida soovite töötada (valisime **vGetAllCategories**), ja seejärel klõpsake nuppu **edasi**.

    ![Valige andmebaas ja tabel.](./media/sql-database-connect-excel/select-database-and-table.png)

    Avaneb dialoogiboks **Salvesta andmeühendusfail ja valmis** , kuhu saate sisestada teavet Office'i andmebaasiga ühenduse (odc) faili, mis kasutab Excel. Saate kohandada oma valikute või jätke vaikesätted.

6. Saate jätke vaikesätted, kuid Märkus eelkõige **Faili nimi** . **Kirjeldus**, **Sõbralik nimi**ja **Märksõnad** , mis aitavad teil ja teiste kasutajate meeles pidada, mida loote ühenduse ja otsige üles ühendus. Kui soovite ühenduseteavet talletatavad odc-faili, et seda saaks värskendada, kui ühendate selle, ja klõpsake siis nuppu **valmis**, klõpsake nuppu **alati proovida kasutada seda faili andmete värskendamine** .

    ![Odc-faili salvestamine](./media/sql-database-connect-excel/save-odc-file.png)

    Kuvatakse dialoogiboks **andmete importimine** .

## <a name="import-the-data-into-excel-and-create-a-pivot-chart"></a>Exceli andmete importimine ja pivot-diagrammi loomine
Nüüd, kui olete loonud ühenduse ja loodud faili andmed ja ühenduse teave, mida praegu loete andmed importida.

1. Dialoogiboksis **Andmete importimine** klõpsake töölehel andmete esitamiseks soovitud suvand ja seejärel klõpsake nuppu **OK**. Valisime **PivotChart-liigenddiagrammi**. Saate luua **uue töölehe** või lisa **need andmed andmemudelisse**. Andmemudelite kohta leiate lisateavet teemast [Exceli andmemudeli loomine](https://support.office.com/article/Create-a-Data-Model-in-Excel-87E7A54C-87DC-488E-9410-5C75DBCB0F7B). Klõpsake nuppu **Atribuudid** eelmises juhises loodud odc-faili teabe analüüsimiseks ja andmete värskendamise suvandite valimiseks.

    ![Exceli andmete vormingu valimine](./media/sql-database-connect-excel/import-data.png)

    Tööleht on nüüd tühja PivotTable-liigendtabeli ja diagrammi.

8. **PivotTable-liigendtabeli väljad**, valige kõik-ruudud, väljad, mida soovite vaadata.

    ![Andmebaasi aruande konfigureerimine.](./media/sql-database-connect-excel/power-pivot-results.png)

> [AZURE.TIP]Kui soovite luua ühenduse muude Exceli töövihikute ja töölehtede andmebaasiga, klõpsake menüü **andmed**nuppu **ühendused**, nuppu **Lisa**, valige loendist loodud ühendus, ning klõpsake nuppu **Ava**.
> ![Mõne teise töövihikuga ühenduse avamine](./media/sql-database-connect-excel/open-from-another-workbook.png)

## <a name="next-steps"></a>Järgmised sammud

- Siit saate teada, kuidas täpsema päringu ja analüüsi [ühenduse SQL-andmebaasi SQL Server Management Studio](sql-database-connect-query-ssms.md) .
- Lisateavet [elastne kaustu](sql-database-elastic-pool.md)eelised.
- Siit saate teada, kuidas [funktsiooni tagaandmebaas SQL-andmebaasiga, mis ühendab veebirakenduse loomine](../app-service-web/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md).
