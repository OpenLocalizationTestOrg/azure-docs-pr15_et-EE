<properties
    pageTitle="Kõigi teemade SQL-i andmebaas teenuse | Microsoft Azure'i"
    description="Tabeli kõigi teemade Azure'i teenuse nimega SQL-i andmebaas, mis on olemas http://azure.microsoft.com/documentation/articles/, pealkiri ja kirjeldus."
    services="sql-data-warehouse"
    documentationCenter=""
    authors="barbkess"
    manager="jhubbard"
    editor="MightyPen"/>

<tags
    ms.service="sql-data-warehouse"
    ms.workload="sql-data-warehouse"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/05/2016"
    ms.author="barbkess"/>


# <a name="all-topics-for-azure-sql-data-warehouse-service"></a>Kõigi teemade teenuse SQL Azure'i andmebaas

Selles teemas on loetletud iga teema, mis kehtib teenuse Azure **SQL-i andmebaas** . Saate otsida selle veebilehe märksõnade abil **Klahvikombinatsiooni Ctrl + F**, praegune huvi pakkuvate teemade otsimiseks.




## <a name="new"></a>Uue

| &nbsp; | Pealkiri | Kirjeldus |
| --: | :-- | :-- |
| 1 | [Varukoopiate SQL-andmebaas](sql-data-warehouse-backups.md) | Teavet SQL-i andmebaas sisseehitatud andmebaasi varundamine, mis võimaldavad teil taastamine punkti või muu geograafilise piirkonna Azure SQL-i andmebaas taastada. |


## <a name="updated-articles-sql-data-warehouse"></a>Värskendatud artikleid, SQL-i andmebaas

Selles jaotises on loetletud tooted, mis olid värskendanud, kui värskendus on suur või märgatavalt. Iga värskendatud artiklis töötlemata koodilõigu lisatud allahindlusest teksti kuvada. Artikleid värskendati **2016-10-05** **2016 – 08-22** vahemikus kuupäeva.

| &nbsp; | Artikkel | Värskendatud teksti, koodilõigu | Millal värskendatud |
| --: | :-- | :-- | :-- |
| 2 | [Azure'i bloobimälu andmete laadimine SQL-i andmebaas (PolyBase)](sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md) | /-Failide baiti ja valige r.command, s.request_id, r.status (erinevate input_name), arvestatakse nbr_files, jälgida sum (s.bytes_processed) / 1024/1024 nimega gb_processed sys.dm_pdw_exec_requests r sisemine ühendus sys.dm_pdw_dms_external_work s r.request_id = s.request_id kus r. sildi = ' CTAS: Laadi cso. DimProduct "OR r. sildi = ' CTAS: Laadi cso. FactOnlineSales' GROUP BY r.command, s.request_id, r.status ORDER BY nbr_files laskuv gb_processed desc;  | 2016-09-07 |
| 3 | [SQL-i andmebaas taastamine](sql-data-warehouse-restore-database-overview.md) | **Peatatud andmebaas saab taastada?** Andmebaas, mis on peatatud taastamiseks peate esmalt tuua tagasi võrgus. Kui andmebaas on tagasi online, peate taastamine punkte valida seitse päeva. **Geograafilise liigne alale taastamine** Kui kasutate geograafilise liigne salvestusruumi, saate taastada oma andmepunktipaaride andmekeskuse teises regioonis asuva geograafiliste andmebaas. Andmebaas on taastatud viimase päeva varukoopia põhjal. **Ajaskaala taastamine** Andmebaasi saab taastada mis tahes taastamine punkti seitsme viimase päeva jooksul. Hetktõmmiseid käivitage iga neli kaheksa tundi ja on saadaval seitse päeva. Kui hetktõmmise on üle seitsme päeva, aegumist ja selle taastamine punkti pole enam saadaval. **Kulude taastamine** Salvestusruumi tasu taastatud andmebaas on arve Azure Premium Storage määra. Kui andmebaas on taastatud peatamiseks ostmisega Azure Premium Storage määr salvestusruum. On see eelis, pausi te ei tasu | 2016-09-29 |





## <a name="get-started"></a>Alustamine

| &nbsp; | Pealkiri | Kirjeldus |
| --: | :-- | :-- |
| 4 | [SQL Azure'i andmebaas autentimine](sql-data-warehouse-authentication.md) | Azure Active Directory (AAD) ja SQL serveri autentimine SQL Azure'i andmebaas. |
| 5 | [Head tavad SQL Azure'i andmebaas](sql-data-warehouse-best-practices.md) | Soovitused ja põhitõed, mida peaksite teadma, kui välja lahenduste jaoks SQL Azure'i andmebaas. Need aitavad teil edukaks. |
| 6 | [Draiverid SQL Azure'i andmebaas](sql-data-warehouse-connection-strings.md) | Ühendusstringi ja draiverid SQL-andmebaas |
| 7 | [Ühenduse loomine SQL Azure'i andmebaas](sql-data-warehouse-connect-overview.md) | Kuidas leida serveri nimi ja ühenduse stringi teie soovite SQL Azure'i andmebaas |
| 8 | [Azure'i masina õppimisega andmete analüüsiks](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md) | Azure'i masina Õppekeskuse abil saate sõnastikupõhise masina Õppekeskuse mudel SQL Azure'i andmebaas talletatud andmete põhjal koostada. |
| 9 | [Päringu SQL Azure'i andmebaas (sqlcmd)](sql-data-warehouse-get-started-connect-sqlcmd.md) | Päringud käsurea kasuliku sqlcmd SQL Azure'i andmebaas. |
| 10 | [SQL-i andmebaas andmebaasi loomine Transact-SQL-i (TSQL) abil](sql-data-warehouse-get-started-create-database-tsql.md) | Siit saate teada, kuidas luua TSQL Azure SQL-i andmebaas |
| 11 | [Kuidas luua tugi Piletite SQL-andmebaas](sql-data-warehouse-get-started-create-support-ticket.md) | Kuidas luua tugi Piletite SQL Azure'i andmebaas. |
| 12 | [Azure'i andmed Factory andmete laadimine](sql-data-warehouse-get-started-load-with-azure-data-factory.md) | Saate teada, kuidas Azure'i andmed Factory andmete laadimine |
| 13 | [Laadi andmete PolyBase rakenduses SQL-andmebaas](sql-data-warehouse-get-started-load-with-polybase.md) | Siit saate teada, mis PolyBase on ja kuidas seda kasutada andmete hoidmise stsenaariumid. |
| 14 | [Saate luua ka SQL Azure'i andmebaas](sql-data-warehouse-get-started-provision.md) | Saate teada, kuidas luua Azure'i portaalis Azure SQL-i andmebaas |
| 15 | [Looge PowerShelli kaudu SQL-i andmebaas](sql-data-warehouse-get-started-provision-powershell.md) | PowerShelli abil luua SQL-andmebaas |
| 16 | [Power BI andmete visualiseerimine](sql-data-warehouse-get-started-visualize-with-power-bi.md) | Power BI SQL-i andmebaas andmete visualiseerimine |
| 17 | [Päringu SQL Azure'i andmebaas (Visual Studio)](sql-data-warehouse-query-visual-studio.md) | Päringu SQL-i andmebaas Visual Studio. |



## <a name="develop"></a>Töötada

| &nbsp; | Pealkiri | Kirjeldus |
| --: | :-- | :-- |
| 18 | [Optimeerimine tehingud SQL-andmebaas](sql-data-warehouse-develop-best-practices-transactions.md) | Parima tava juhised tehingu tõhusa värskendustega kirjalikult SQL Azure'i andmebaas |
| 19 | [SQL-i andmebaas kokkulangevus ja töökoormus haldus](sql-data-warehouse-develop-concurrency.md) | Lahenduste arendamise mõista kokkulangevus ja töökoormus juhtimise SQL Azure'i andmebaas. |
| 20 | [Tabeli loomine nimega (CTAS) valige SQL-andmebaas](sql-data-warehouse-develop-ctas.md) | Näpunäiteid kodeerimine Loo tabel (CTAS) lause select SQL Azure'i andmebaas nagu arendamise lahendusi. |
| 21 | [Dünaamiline SQL SQL-andmebaas](sql-data-warehouse-develop-dynamic-sql.md) | Näpunäiteid kasutamise dünaamiline SQL Azure'i SQL-i andmebaas arendamise lahendusi. |
| 22 | [Rühmitusalus suvandid SQL-andmebaas](sql-data-warehouse-develop-group-by-options.md) | Näpunäiteid rakendamise jaotises Suvandid SQL Azure'i andmebaas arendamise lahendusi. |
| 23 | [Dokumendi päringute siltide kasutamine SQL-andmebaas](sql-data-warehouse-develop-label.md) | Näpunäiteid dokumendi päringute siltide kasutamine SQL Azure'i andmebaas arendamise lahendusi. |
| 24 | [Silmuseid SQL-andmebaas](sql-data-warehouse-develop-loops.md) | Näpunäiteid Transact-SQL-i silmuseid ja asendades kursorid rakenduses SQL Azure'i andmebaas arendamise lahendusi. |
| 25 | [Salvestatud toimingute SQL-andmebaas](sql-data-warehouse-develop-stored-procedures.md) | Näpunäiteid salvestatud toimingute arendamise lahenduste rakendamisel SQL Azure'i andmebaas. |
| 26 | [Tehingute SQL-andmebaas](sql-data-warehouse-develop-transactions.md) | Näpunäiteid tehingud arendamise lahenduste rakendamisel SQL Azure'i andmebaas. |
| 27 | [Kasutaja määratletud skeemid SQL-andmebaas](sql-data-warehouse-develop-user-defined-schemas.md) | Näpunäiteid rakenduse SQL Azure'i andmebaas Transact-SQL-i skeemid arendamise lahendusi. |
| 28 | [Määrata muutujate SQL-andmebaas](sql-data-warehouse-develop-variable-assignment.md) | Näpunäiteid Transact-SQL-i muutujate SQL Azure'i andmebaas määramine lahendusi. |
| 29 | [Vaadete SQL-andmebaas](sql-data-warehouse-develop-views.md) | Näpunäiteid Transact-SQL-i vaadete kasutamine SQL Azure'i andmebaas arendamise lahendusi. |
| 30 | [Kujundus otsuseid ja kodeerimise võtteid SQL-andmebaas](sql-data-warehouse-overview-develop.md) | Arengu põhimõtet, kujundus otsuste, soovituste ja kodeerimise võtteid SQL-i andmebaas. |



## <a name="manage"></a>Haldamine

| &nbsp; | Pealkiri | Kirjeldus |
| --: | :-- | :-- |
| 31 | [Arvuta power (ülevaade) SQL Azure'i andmebaas haldamine](sql-data-warehouse-manage-compute-overview.md) | Jõudluse skaala läbi funktsioonidele SQL Azure'i andmebaas. Välja, reguleerides DWUs mastaapimiseks või viige ja jätkake Arvuta ressursside kulude salvestamiseks. |
| 32 | [Arvuta power (Azure'i portaal) SQL Azure'i andmebaas haldamine](sql-data-warehouse-manage-compute-portal.md) | Azure'i portaali ülesannete haldamiseks Arvutage power. Skaala Arvuta ressursse, reguleerides DWUs. Või Peata ja Jätka arvutada ressursside kulude salvestamiseks. |
| 33 | [Arvuta power (PowerShelli) SQL Azure'i andmebaas haldamine](sql-data-warehouse-manage-compute-powershell.md) | PowerShelli ülesannete haldamiseks Arvutage power. Skaala Arvuta ressursid, reguleerides DWUs. Või Peata ja Jätka arvutada ressursside kulude salvestamiseks. |
| 34 | [Arvuta power rakenduses Azure SQL-i andmete ladu (REST) haldamine](sql-data-warehouse-manage-compute-rest-api.md) | PowerShelli ülesannete haldamiseks Arvutage power. Skaala Arvuta ressursid, reguleerides DWUs. Või Peata ja Jätka arvutada ressursside kulude salvestamiseks. |
| 35 | [Arvuta power (T-SQL-i) SQL Azure'i andmebaas haldamine](sql-data-warehouse-manage-compute-tsql.md) | Skaala-out jõudluse, reguleerides DWUs tööülesandeid Transact-SQL-i (T-SQL-i). Kulude kokkuhoidmiseks skaleerimist tagasi-tippväärtus ajal. |
| 36 | [Jälgida oma töökoormus DMVs abil](sql-data-warehouse-manage-monitor.md) | Saate teada, kuidas jälgida oma töökoormus DMVs abil. |
| 37 | [SQL Azure'i andmebaas andmebaaside haldamine](sql-data-warehouse-overview-manage.md) | Ülevaade SQL-i andmebaas andmebaaside haldamine. Sisaldab Haldusriistad, DWUs ja tõrkeotsingu päringu jõudluse, millega hea turbepoliitikate ja andmebaasi taastamine andmete rikkumist või piirkondliku katkestuste skaala-out jõudlust. |
| 38 | [Kasutaja päringuid SQL Azure'i andmebaas jälgimine](sql-data-warehouse-overview-manage-user-queries.md) | Peaksite arvesse võtma, head tavad, ja tööülesannete jälgimiseks kasutaja päringute SQL Azure'i andmebaas ülevaade |
| 39 | [SQL-i andmebaas taastamine](sql-data-warehouse-restore-database-overview.md) | Andmebaasi taastamine suvandid SQL Azure'i andmebaas andmebaasi taastamisel ülevaade. |
| 40 | [SQL Azure'i andmebaas (portaal) taastamine](sql-data-warehouse-restore-database-portal.md) | Azure'i portaali tööülesannete taastamiseks Azure SQL-i andmebaas. |
| 41 | [SQL Azure'i andmebaas (PowerShelli) taastamine](sql-data-warehouse-restore-database-powershell.md) | PowerShelli tööülesannete taastamiseks Azure SQL-i andmebaas. |
| 42 | [SQL Azure'i andmebaas (REST API) taastamine](sql-data-warehouse-restore-database-rest-api.md) | REST API tööülesannete taastamiseks Azure SQL-i andmebaas. |



## <a name="tables-and-indexes"></a>Tabelite ja registrite

| &nbsp; | Pealkiri | Kirjeldus |
| --: | :-- | :-- |
| 43 | [Andmetüübid tabelid SQL-andmebaas](sql-data-warehouse-tables-data-types.md) | Andmetüüpide SQL Azure'i andmebaas tabelitega töötamise alustamine. |
| 44 | [Levitamise tabelid SQL-andmebaas](sql-data-warehouse-tables-distribute.md) | Alustamine levitamise tabelid SQL Azure'i andmebaas. |
| 45 | [Indekseerimise tabelid SQL-andmebaas](sql-data-warehouse-tables-index.md) | Alustamine rakenduses SQL Azure'i andmebaas indekseerimine tabel. |
| 46 | [Ülevaade tabelid SQL-andmebaas](sql-data-warehouse-tables-overview.md) | Azure SQL-i ladu andmetabelite töötamise alustamine. |
| 47 | [Eraldamine tabelid SQL-andmebaas](sql-data-warehouse-tables-partition.md) | Alustamine tabeli eraldamine rakenduses SQL Azure'i andmebaas. |
| 48 | [Haldamise statistika tabelid SQL-andmebaas](sql-data-warehouse-tables-statistics.md) | Alustamine statistika tabelid SQL Azure'i andmebaas. |
| 49 | [Ajutised tabelid SQL-andmebaas](sql-data-warehouse-tables-temporary.md) | Alustamine Ajutised tabelid SQL Azure'i andmebaas. |



## <a name="integrate"></a>Sujuv

| &nbsp; | Pealkiri | Kirjeldus |
| --: | :-- | :-- |
| 50 | [Azure'i andmed Factory kasutamine SQL-andmebaas](sql-data-warehouse-integrate-azure-data-factory.md) | Näpunäiteid kasutades Azure Factory (ADF) SQL Azure'i andmebaas lahendusi. |
| 51 | [Azure'i masina Õppekeskuse kasutamine SQL-andmebaas](sql-data-warehouse-integrate-azure-machine-learning.md) | Õpetus Azure seadme õ abil SQL Azure'i andmebaas lahendusi. |
| 52 | [Azure'i voo Analytics kasutamine SQL-andmebaas](sql-data-warehouse-integrate-azure-stream-analytics.md) | Näpunäiteid kasutades Azure voo Analytics SQL Azure'i andmebaas lahendusi. |
| 53 | [Power BI kasutamine SQL-andmebaas](sql-data-warehouse-integrate-power-bi.md) | Näpunäiteid Power BI abil SQL Azure'i andmebaas lahendusi. |
| 54 | [SQL-i andmebaas koos muude teenustega kasutada.](sql-data-warehouse-overview-integrate.md) | Tööriistad ja lahendusi, mis SQL-i andmebaas integreerida partnerite.  |



## <a name="load"></a>Laadi

| &nbsp; | Pealkiri | Kirjeldus |
| --: | :-- | :-- |
| 55 | [Azure'i bloobimälu andmete laadimine (Azure'i andmed Factory) SQL Azure'i andmebaas](sql-data-warehouse-load-from-azure-blob-storage-with-data-factory.md) | Saate teada, kuidas Azure'i andmed Factory andmete laadimine |
| 56 | [Azure'i bloobimälu andmete laadimine SQL-i andmebaas (PolyBase)](sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md) | Saate teada, kuidas kasutada PolyBase andmete Azure'i bloobimälu laadimiseks SQL-i andmebaas. Asetage mõned tabelid avalikest andmetest skeemi Contoso jaemüügi andmebaas. |
| 57 | [SQL serveri andmete laadimine (AZCopy) SQL Azure'i andmebaas](sql-data-warehouse-load-from-sql-server-with-azcopy.md) | Kasutab bcp SQL serveri andmete eksportimine tasapinnalise failid, saate importida andmed Azure'i bloobimälu AZCopy ja PolyBase neelata andmed üheks SQL Azure'i andmebaas. |
| 58 | [SQL serveri andmete laadimine (tasapinnalise failid) SQL Azure'i andmebaas](sql-data-warehouse-load-from-sql-server-with-bcp.md) | Suurus: väike andmete jaoks kasutatakse bcp SQL serveri andmete eksportimine tasapinnalise failid ja importida andmed otse SQL Azure'i andmebaas. |
| 59 | [Andmete laadimine SQL Serverist rakendusse Azure SQL-i andmete ladu (SSIS)](sql-data-warehouse-load-from-sql-server-with-integration-services.md) | Näitab, kuidas luua SQL serveri Integration Services (SSIS) paketi andmeallikate mitmesuguseid andmete teisaldamiseks SQL-i andmebaas. |
| 60 | [Laadi andmete PolyBase rakenduses SQL-andmebaas](sql-data-warehouse-load-from-sql-server-with-polybase.md) | Kasutab bcp SQL serveri andmete eksportimine tasapinnalise failid, saate importida andmed Azure'i bloobimälu AZCopy ja PolyBase neelata andmed üheks SQL Azure'i andmebaas. |
| 61 | [Juhend PolyBase kasutamine SQL-andmebaas](sql-data-warehouse-load-polybase-guide.md) | Juhised ja soovitused PolyBase kasutamine SQL-i andmebaas stsenaariumid. |
| 62 | [Näidisandmete laadimine SQL-andmebaas](sql-data-warehouse-load-sample-databases.md) | Näidisandmete laadimine SQL-andmebaas |
| 63 | [Bcp andmete laadimine](sql-data-warehouse-load-with-bcp.md) | Siit saate teada, millised bcp on ja kuidas seda kasutada andmete hoidmise stsenaariumid. |
| 64 | [Andmete laadimine SQL Azure'i andmebaas](sql-data-warehouse-overview-load.md) | Siit saate teada, tavastsenaariumid andmete laadimine SQL-i andmebaas. Nendeks PolyBase, Azure'i bloobimälu, tasapinnalise failide ja ketta saatmine. Saate kasutada ka kolmanda osapoole tööriistu. |



## <a name="migrate"></a>Migreerimine

| &nbsp; | Pealkiri | Kirjeldus |
| --: | :-- | :-- |
| 65 | [SQL-koodi migreerimine SQL-andmebaas](sql-data-warehouse-migrate-code.md) | Näpunäiteid migreerimine SQL-koodi SQL Azure'i andmebaas arendamise lahendusi. |
| 66 | [Andmete migreerimine](sql-data-warehouse-migrate-data.md) | Näpunäiteid andmete migreerimist SQL Azure'i andmebaas arendamise lahendusi. |
| 67 | [Andmete ladu migreerimise kasuliku (eelvaade)](sql-data-warehouse-migrate-migration-utility.md) | SQL-i andmebaas migreerida. |
| 68 | [Oma skeemi migreerimine SQL-andmebaas](sql-data-warehouse-migrate-schema.md) | Näpunäiteid migreerimine oma skeemi SQL Azure'i andmebaas arendamise lahendusi. |
| 69 | [Teie lahendus migreerimine SQL-andmebaas](sql-data-warehouse-overview-migrate.md) | Migreerimise juhiseid SQL Azure'i andmebaas platvormi teie lahendus toomiseks. |



## <a name="partners"></a>Partnerite

| &nbsp; | Pealkiri | Kirjeldus |
| --: | :-- | :-- |
| 70 | [SQL-i andmebaas business intelligence partnerite](sql-data-warehouse-partner-business-intelligence.md) | Loendite kolmanda osapoole äripartneritega ärianalüüsi lahendusi, mis toetab SQL-i andmebaas. |
| 71 | [SQL-i andmebaas andmete integreerimise partnerite](sql-data-warehouse-partner-data-integration.md) | Loendite kolmanda osapoole partnerite andmete integreerimise lahendusi, mis toetab SQL Azure'i andmebaas. |
| 72 | [SQL-i andmebaas andmete haldamise partnerite](sql-data-warehouse-partner-data-management.md) | Loendite kolmanda osapoole andmete haldamise partnerite lahendusi, mis toetab SQL-i andmebaas. |



## <a name="reference"></a>Viide

| &nbsp; | Pealkiri | Kirjeldus |
| --: | :-- | :-- |
| 73 | [Viide teemade jaoks SQL-andmebaas](sql-data-warehouse-overview-reference.md) | Viide sisu lingid SQL-i andmebaas. |
| 74 | [PowerShelli cmdlet-käskude ja REST API-de jaoks SQL-andmebaas](sql-data-warehouse-reference-powershell-cmdlets.md) | Azure SQL-i andmebaas, sh kuidas peatada ja jätkata andmebaasi ülemise PowerShelli cmdlet-käskude otsimine. |
| 75 | [Keele elemendid](sql-data-warehouse-reference-tsql-language-elements.md) | Viide sisu Transact-SQL-i keele elemendid, mida kasutatakse SQL-i andmebaas linkide loend. |
| 76 | [Transact-SQL-i Teemad](sql-data-warehouse-reference-tsql-statements.md) | Viide sisu Transact-SQL-i teemad, mis kasutavad SQL-i andmebaas sisaldab linke. |
| 77 | [Süsteemi vaated](sql-data-warehouse-reference-tsql-system-views.md) | Linkide süsteemi vaated sisu SQL-i andmebaas. |



## <a name="security"></a>Turvalisus

| &nbsp; | Pealkiri | Kirjeldus |
| --: | :-- | :-- |
| 78 | [SQL-i andmebaas - alltaseme kliendid toetavad Masking Auditeerimispoliitika ja dünaamiline andmete jaoks](sql-data-warehouse-auditing-downlevel-clients.md) | Lisateavet SQL-i andmebaas alltaseme kliendid tugi andmete kontrollimine |
| 79 | [Auditi rakenduses SQL Azure'i andmebaas](sql-data-warehouse-auditing-overview.md) | Alustamine rakenduses SQL Azure'i andmebaas kontrollimine |
| 80 | [Töö alustamine koos läbipaistev krüptimise (TDE) rakenduses SQL-andmebaas](sql-data-warehouse-encryption-tde.md) | SQL-i andmebaas läbipaistvaid andmete krüptimine (TDE) |
| 81 | [Töö alustamine läbipaistev andmete krüptimise (TDE)](sql-data-warehouse-encryption-tde-tsql.md) | Läbipaistva andmete krüptimine (TDE) SQL-i andmebaas (T-SQL) |
| 82 | [Turvaline andmebaasi SQL-i andmebaas](sql-data-warehouse-overview-manage-security.md) | Näpunäiteid turvaliseks SQL Azure'i andmebaas andmebaasi arendamise lahendusi. |



## <a name="miscellaneous"></a>Mitmesugused

| &nbsp; | Pealkiri | Kirjeldus |
| --: | :-- | :-- |
| 83 | [Visual Studio 2015 ja SSDT installida SQL-andmebaas](sql-data-warehouse-install-visual-studio.md) | Installige Visual Studio ja SQL Server arengu Tools (SSDT) SQL Azure'i andmebaas |
| 84 | [Migreerimise Premium salvestusruumi üksikasjad](sql-data-warehouse-migrate-to-premium-storage.md) | Juhised migreerimine olemasoleva SQL-i andmebaas premium salvestusruum |
| 85 | [Ohtude tuvastamise kasutamise alustamine](sql-data-warehouse-security-threat-detection.md) | Kuidas alustada ohtude tuvastamise |
| 86 | [SQL-i andmebaas võimsus piirangud](sql-data-warehouse-service-capacity-limits.md) | Suurim väärtus ühendused, andmebaase, tabeleid ja päringuid SQL-i andmebaas. |
| 87 | [Tõrkeotsingu Azure SQL-andmebaas](sql-data-warehouse-troubleshoot.md) | Tõrkeotsingu SQL Azure'i andmebaas. |

