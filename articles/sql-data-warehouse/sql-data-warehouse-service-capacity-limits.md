<properties
   pageTitle="SQL-i andmebaas võimsuse piiranguid | Microsoft Azure'i"
   description="Suurim väärtus ühendused, andmebaase, tabeleid ja päringuid SQL-i andmebaas."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/01/2016"
   ms.author="sonyama;barbkess;jrj"/>

# <a name="sql-data-warehouse-capacity-limits"></a>SQL-i andmebaas võimsus piirangud

Järgmised tabelid sisaldavad eri osade SQL Azure'i andmebaas lubatud suurimad väärtused.


## <a name="workload-management"></a>Töökoormus haldus

| Kategooria            | Kirjeldus                                       | Maksimum            |
| :------------------ | :------------------------------------------------ | :----------------- |
| [Andmete ladu üksused (DWU)][]| Max DWU jaoks ühtse SQL-andmebaas | 6000               |
| [Andmete ladu üksused (DWU)][]| Max DWU ühe SQL Server         | Vaikimisi 6000<br/><br/> Vaikimisi on iga SQL serveri (nt myserver.database.windows.net) DTU kvoodi 45 000, mis võimaldab kuni 6000 DWU. Selle piirmäära on lihtsalt ohutus piirang. Saate suurendada, [luues tugi Piletite][] ja valides *kvoodi* taotluse tüübiks meiliteavitus.  Teie DTU arvutamiseks tuleb 7,5 korrutamine DWU vaja kogusumma. Saate vaadata oma praeguse DTU tarbimine keelest SQL serveri portaalis. Peatatud- ja un peatatud andmebaasi arvestata DTU kvoodi. |
| Andmebaasi ühendus | Samaaegseid avatud seansid                          | 1024<br/><br/>Me ei toeta kuni 1024 aktiivse ühendused, millest igaüks saab esitada taotlusi SQL-i andmebaas andmebaasi samal ajal. Pange tähele, et on olemas päringute tegelikult samaaegselt maksimumarvuga. Kui kokkulangevus limiidi ületamise korral taotluse läheb kus see ootab töödelda ka sisemise järjekorda.|
| Andmebaasi ühendus | Suurim lubatud mälu valmis laused            | 20 MB              |
| [Töökoormus haldus][] | Suurim lubatud samaaegseid päringud                    | 32<br/><br/> Vaikimisi käivitamiseks SQL-i andmebaas kuni 32 samaaegseid päringud ja järjekorrad allesjäänud päringud.<br/><br/>Kokkulangevus tase võib vähendada, kui kasutajad on määratud ressursi kõrgema klassi või SQL-i andmebaas on konfigureeritud madal DWU. Mõned päringud DMV päringud, nagu on alati käivitamine lubatud.|
| [Tempdb][]          | Max Tempdb suurus                                | 399 GB DW100 kohta. Seetõttu DWU1000 Tempdb juures on suuruses – 3,99 TB |


## <a name="database-objects"></a>Andmebaasiobjektide

| Kategooria          | Kirjeldus                                  | Maksimum            |
| :---------------- | :------------------------------------------- | :----------------- |
| Andmebaasi          | Max suurus                                     | 240 TB kettal tihendatud<br/><br/>Siia ei sõltu tempdb või Logi ruumi ja seetõttu siia eesmärk on jätkuvalt püsivate tabelid.  Rühmitatud columnstore tihendamise hinnatakse 5 X.  See tihendamise võimaldab andmebaasist amortisatsioonimäär kuni umbes 1 PB, kui kõik tabelid on rühmitatud columnstore (vaikimisi tabeli tüüp).|
| Tabel             | Max suurus                                     | 60 TB kettal tihendatud   |
| Tabel             | Tabeleid andmebaasi kohta                          | 2 miljonit          |
| Tabel             | Veergude tabeli kohta                            | 1024 veerud       |
| Tabel             | Baiti veeru kohta                             | Sõltub veeru [andmetüüpi][].  Lubatud 8000 char andmetüübid, 4000 nvarchar või MAX andmetüübid 2 GB.|
| Tabel             | Baiti määratletud suurus rea kohta                  | 8060 baiti<br/><br/>Rea kohta baitide arvu, arvutatakse samal viisil, et see on SQL serveri jaoks lehe pakkimine. SQL serveri, nagu SQL-i andmebaas toetab rea-ületäitumine salvestusruumi, mis võimaldab **eri pikkusega** lükata rida. Muutuv pikkus read on lükata rida, säilitatakse vaid 24-baidine root peamise kirje. Lisateavet leiate artiklist [rea-ületäitumine andmete üle 8 KB][] MSDN-i.|
| Tabel             | Sektsioonid tabeli kohta                    | 15000<br/><br/>Suure jõudlusega, soovitame vähendada mitmeid sektsioonide vaja ajal veel toetavad äritegevuse nõuetele. Sektsioonid arv kasvab, on pea kohal andmete andmekirjelduskeele (DDL) ja käsitlemise keel (piirmäära) kasvab ja põhjustab aeglasem.|
| Tabel             | Märkide partition piiri väärtus.| 4000 |
| Index             | Mitte-rühmitatud registrid tabeli kohta.        | 999<br/><br/>Kehtib ainult rowstore tabelid.|
| Index             | Tabeli kohta rühmitatud registrid.            | 1<br><br/>Kehtib rowstore nii columnstore tabelid.|
| Index             | Registri võtme suurus.                          | 900 baiti.<br/><br/>Kehtib ainult rowstore registrid.<br/><br/>Kui olemasolevad andmed veergudes ei ületa 900 baiti registri loomisel loomist registrid varchar veergude maksimaalse suurusega rohkem kui 900 baiti. Siiski hiljem lisada või veerud, mis põhjustavad kokku maht ületab lubatud 900 baiti UPDATE toimingud ei õnnestu.|
| Index             | Võtme veerud kohta indeks.                   | 16<br/><br/>Kehtib ainult rowstore registrid. Rühmitatud columnstore registrid kaasata kõik veerud.|
| Statistika        | Ühendatud veeruväärtuste suurus.      | 900 baiti.         |
| Statistika        | Veergude kohta statistika objekti.           | 32                 |
| Statistika        | Loodud tabeli veergude statistika. | 30 000            |
| Salvestatud toimingute | Suurim lubatud pesastustaset.               | 8                 |
| Vaade              | Veergude kuvamine kohta                         | 1024             |


## <a name="loads"></a>Laadimise

| Kategooria          | Kirjeldus                                  | Maksimum            |
| :---------------- | :------------------------------------------- | :----------------- |
| Polybase laadimise    | Baiti rea kohta                                | 32,768<br/><br/>Polybase laadimise on piiratud laadimine read nii väiksem kui 32K ja ei saa laadida VARCHR(MAX), NVARCHAR(MAX) või VARBINARY(MAX).  Kuigi see piirang on olemas täna, eemaldatakse see üsna varsti.<br/><br/>


## <a name="queries"></a>Päringud

| Kategooria          | Kirjeldus                                  | Maksimum            |
| :---------------- | :------------------------------------------- | :----------------- |
| Päringu             | Kasutaja tabelite järjekorras olevad päringud.               | 1000               |
| Päringu             | Samaaegseid päringud vaadete süsteem.          | 100                |
| Päringu             | Ootel päringute süsteemi vaadetes               | 1000               |
| Päringu             | Suurim lubatud parameetrid                           | 2098               |
| Paketi             | Suurim lubatud maht                                 | 65, 536 * 4096        |
| Valige tulemused    | Veerge                              | 4096<br/><br/>Ei saa kunagi olla rohkem kui 4096 veerge valige tulemus. Ei ole kindel, et teil alati olemas 4096. Kui päringu plaan nõuab ajutise tabeli, võidakse rakendada tabeli kuni 1024 veerud.|
| VALIGE            | Pesastatud alampäringud                            | 32<br/><br/>Ei saa kunagi olla rohkem kui 32 pesastatud alampäringud SELECT-lause. Ei ole kindel, et teil alati olemas 32. Näiteks saate ühenduse Alampäringu lisada päringu plaan. Samuti saate alampäringud arv piiratud vaba mälumahuga.|
| VALIGE            | Veergude ühendamine kohta                             | 1024 veerud<br/><br/>Ei saa kunagi olla rohkem kui 1024 veergude ühendust. Ei ole kindel, et teil alati olemas 1024. Kui ühendus leping nõuab rohkem veerge kui Liitu tulemi ajutine tabel, 1024 piirang kehtib ajutine tabel. |
| VALIGE            | Baiti kohta GROUP BY veerud.                  | 8060<br/><br/>Klausel GROUP BY veerud võib olla kuni 8060 baiti.|
| VALIGE            | Baiti JÄRJESTUSALUS veergude kohta                   | 8060 baiti.<br/><br/>Klausel ORDER BY veerud võib olla kuni 8060 baiti.|
| Identifikaatorite ja konstandid lause kohta. | Arv viidatud identifikaatorite ja konstante. | 65 535<br/><br/>SQL-i andmebaas identifikaatorite ja ühe avaldises päringu sisalduvaid konstantide arv on piiratud. See piirang on 65 535. SQL Serveri tõrge 8632 see arv tulemused üle. Lisateavet leiate teemast [sisemine tõrge: piirarvu avaldis teenuste jõudnud][].|


## <a name="metadata"></a>Metaandmete

| Süsteemide vaade                        | Ridade maksimumarv |
| :--------------------------------- | :------------|
| sys.dm_pdw_component_health_alerts | 10 000       |
| sys.dm_pdw_dms_cores               | 100          |
| sys.dm_pdw_dms_workers             | Viimati tehtud 1000 DMS töötajate koguarv SQL taotleb. |
| sys.dm_pdw_errors                  | 10 000       |
| sys.dm_pdw_exec_requests           | 10 000       |
| sys.dm_pdw_exec_sessions           | 10 000       |
| sys.dm_pdw_request_steps           | Juhised viimase 1000 SQL-i taotlusi, mis on talletatud sys.dm_pdw_exec_requests koguarv. |
| sys.dm_pdw_os_event_logs           | 10 000       |
| sys.dm_pdw_sql_requests            | Viimati tehtud 1000 SQL-i taotlusi, mis on talletatud sys.dm_pdw_exec_requests. |


## <a name="next-steps"></a>Järgmised sammud
Viide lisateabe saamiseks vt [SQL-i andmebaas viide ülevaade][].

<!--Image references-->

<!--Article references-->
[Andmete ladu üksused (DWU)]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units
[SQL-i andmebaas viide ülevaade]: ./sql-data-warehouse-overview-reference.md
[Töökoormus haldus]: ./sql-data-warehouse-develop-concurrency.md
[Tempdb]: ./sql-data-warehouse-tables-temporary.md
[andmetüüp]: ./sql-data-warehouse-tables-data-types.md
[tugiteenuste Piletite loomine]: /sql-data-warehouse-get-started-create-support-ticket.md

<!--MSDN references-->
[Rea-ületäitumine andmete üle 8 KB]: https://msdn.microsoft.com/library/ms186981.aspx
[Sisemine tõrge: piirarvu avaldis teenuste jõudnud]: https://support.microsoft.com/kb/913050
