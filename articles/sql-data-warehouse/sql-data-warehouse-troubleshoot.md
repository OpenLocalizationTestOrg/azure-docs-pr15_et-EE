<properties
   pageTitle="SQL Azure'i andmebaas tõrkeotsing | Microsoft Azure'i"
   description="Tõrkeotsingu SQL Azure'i andmebaas."
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
   ms.date="08/30/2016"
   ms.author="sonyama;barbkess"/>

# <a name="troubleshooting-azure-sql-data-warehouse"></a>Tõrkeotsingu Azure SQL-andmebaas

Selles teemas on loetletud mõned sagedamini tõrkeotsingu küsimused Soovime kuulda klientidele.

## <a name="connecting"></a>Ühenduse loomine

| Probleem                              | Eraldusvõime                                      |
| :----------------------------------| :---------------------------------------------- |
| Kasutaja 'NT AUTHORITY\ANONYMOUS sisselogimine' sisselogimine nurjus. (Microsoft SQL Server, viga: 18456) | See tõrge ilmneb juhul, kui olete AAD kasutaja proovib juhtslaidi andmebaasiga ühenduse loomiseks, kuid ei sisalda kasutaja juhtslaidi.  Selle probleemi lahendamiseks kas määrata SQL-i andmebaas, mida soovite ühenduse luua ühenduse loomise ajal või põhi andmebaasi kasutaja lisamine.  Vt artikli üksikasjade [turvalisuse ülevaade][] .|
|Server põhisumma "MyUserName" ei saa andmebaasile juurde pääseda "juhtslaidi" jaotises Turve praeguses kontekstis. Kasutaja vaikimisi andmebaasi ei saa avada. Sisselogimine nurjus. Kasutaja 'MyUserName' sisselogimine nurjus. (Microsoft SQL Server, tõrge: 916) | See tõrge ilmneb juhul, kui olete AAD kasutaja proovib juhtslaidi andmebaasiga ühenduse loomiseks, kuid ei sisalda kasutaja juhtslaidi.  Selle probleemi lahendamiseks kas määrata SQL-i andmebaas, mida soovite ühenduse luua ühenduse loomise ajal või põhi andmebaasi kasutaja lisamine.  Vt artikli üksikasjade [turvalisuse ülevaade][] .|
| CTAIP tõrge                        | See tõrge võib ilmneda juhul, kui SQL serveri andmebaasi juhtslaidi, kuid mitte andmebaasi SQL-i andmebaas on loodud sisselogimist.  Kui tõrge ilmneb Heitke pilk artiklis [turvalisuse ülevaade][] .  Selles artiklis selgitatakse, kuidas luua luua kasutajanime ja kasutaja juhtslaid ja seejärel SQL-i andmebaas andmebaasi kasutaja loomise kohta.|
| Tulemüür blokeerib                |SQL Azure'i andmebaasid on kaitstud server ja andmebaas, et tagada taseme tulemüürid ainus teadaolev juurdepääs andmebaasi IP-aadressid. Tulemüürid on turvaline vaikimisi, mis tähendab, et peate selgesõnaliselt lubama ja IP-aadress või vahemik aadressid, enne kui saate luua ühenduse.  Konfigureerida tulemüüri juurdepääsu, järgige [Konfigureeri serveri tulemüüri juurdepääsu oma kliendi IP][] [Provisioning juhiseid][].|
| Tööriista või draiveri ei saa ühendust luua | SQL-i andmebaas soovitab päringu andmete [SSMS][], [SSDT Visual Studio 2015][]või [sqlcmd][] abil. Lugege lisateavet draiverid ja ühenduse SQL-i andmebaas [draiverid SQL Azure'i andmebaas][] ja [ühenduse loomine SQL Azure'i andmebaas][] artikleid.|


## <a name="tools"></a>Tööriistad

| Probleem                              | Eraldusvõime                                      |
| :----------------------------------| :---------------------------------------------- |
| Visual Studio objekti Exploreri puudub AAD kasutajad | See on teadaolev probleem.  Lahendusena saate vaadata nende kasutajate [sys.database_principals][].  Vt lisateavet SQL-i andmebaas Azure Active Directory abil [autentimise SQL Azure'i andmebaas][] .|

## <a name="performance"></a>Jõudluse

|  Probleem                             | Eraldusvõime                                      |
| :----------------------------------| :---------------------------------------------- |
| Päringu jõudluse tõrkeotsingu otstarve  | Kui soovite teatud päringu tõrkeotsing, alustage [õppida, kuidas kontrollida oma päringuid][].|
| Päringu kehv jõudlus ja lepingute sageli on tulemus on puudu statistika   | Statistika tabelitel on jõudlus kõige levinum põhjus.  Vt [säilitamine tabeli statistika] [ Statistics] kuidas luua statistika ja miks need olulised jõudluse kohta.|
| Madal kokkulangevus / päringud ootele   | Mõistmine [töökoormus haldamise][] on oluline, et aru saada, kuidas koos kokkulangevus mälu jaotamine tasakaalustamiseks.|
| Kuidas rakendada head tavad    | Parim koht alustamiseks võimalust päringu jõudluse parandamiseks on [SQL-i andmebaas head tavad][] artiklis.|
| Kuidas parandada jõudlust mastaapimine  | Mõnikord lahendus jõudluse parandamine on lihtsalt lisada rohkem arvutada power päringutele skaala [Oma SQL-andmebaas][].|
| Päringu kehv jõudlus tulemusena kehva index kvaliteet | Mõnikord päringud saate aeglustumine [kehva columnstore index kvaliteedi][]tõttu.  Sellest artiklist leiate lisateavet teemast ja kuidas [taastada registrid lõigu kvaliteedi parandamiseks][].|

## <a name="system-management"></a>Süsteemi haldus

|  Probleem                             | Eraldusvõime                                      |
| :----------------------------------| :---------------------------------------------- |
| Sõnum 40847: Mitte toimingut teha, kuna server ületab lubatud andmebaasi tehingu ühiku piirmäära 45000. | Vähendage [DWU][] andmebaas, mida proovite luua või [taotleda kvoodi suurendamist][].|
| Kasutamise uurimine    | Lugege teemat mõista teie süsteemi ruumi kasutamine [tabeli suurust][] .|
| Abi tabelite haldamine          | [Tabeli ülevaade] [ Overview] artiklis abi tabelite haldamine.  See artikkel sisaldab ka linke sisse üksikasjalikuma teemasid [tabeli andmetüübid][Data types], [levitamise tabeli][Distribute], [indekseerimine tabeli][Index], [eraldamine tabeli][Partition], [säilitamine tabeli statistika] [ Statistics] ja [Ajutised tabelid][Temporary].|

## <a name="polybase"></a>Polybase

|  Probleem                             | Eraldusvõime                                      |
| :----------------------------------| :---------------------------------------------- |
| UTF-8 tõrge                        |  PolyBase ainult toetab praegu laadimine on UTF-8 kodeeringuga andmefaile.  Leiate juhised selle kohta, kuidas see piirang [töö ümber PolyBase UTF-8 nõue][] .|
| Laadimine nurjub suurte read   | Praegu ei ole suur rea tugi Polybase jaoks saadaval.  See tähendab, et kui tabel sisaldab NVARCHAR(MAX), VARCHAR(MAX) ja VARBINARY(MAX), välise tabeleid ei saa kasutada andmete laadimine.  Suure ridade laadimise pole praegu toetatud ainult Azure'i andmed Factory (koos BCP), Azure'i voo Analytics, SSIS, BCP või .NET SQLBulkCopy klassi kaudu. Suure ridade PolyBase tugi lisatakse tulevikus.|
| Tabel koos MAX andmetüüp BCP laadimine nurjub | Teadaolevate probleemide kohta, mis nõuab NVARCHAR(MAX), VARCHAR(MAX) ja VARBINARY(MAX) paigutada tabeli mõnel juhul lõpus on.  Proovige oma MAX veergude tabeli lõppu.|

## <a name="differences-from-sql-database"></a>SQL-andmebaasi erinevused

|  Probleem                             | Eraldusvõime                                      |
| :----------------------------------| :---------------------------------------------- |
| Mittetoetatavad SQL-andmebaasi funktsioonid  | [Mittetoetatavad tabelifunktsioonid][]näha.|
| SQL-andmebaasi pole toetatud andmetüübid  | Vt [pole toetatud andmetüübid][].|
| Kustuta ja VÄRSKENDA piirangud      | Vt [UPDATE lahendusi][], siis [kustutage lahendusi][] ja [Lahendamiseks toetamata värskendamine ja kustutamine süntaks CTAS abil][].  |
| Kirjakooste aruande ei toetata   | Vt [ühendamine lahendusi][].|
| Salvestatud protseduur piirangud       | Vaadake [salvestatud protseduur piirangud][] mõista salvestatud toimingute piirangud.|
| Siit leiate ülevaate SELECT-laused ei toeta | See on meie UDF praegune piirang.  Vaadake [Loomine funktsioon][] toetame süntaks.   |

## <a name="next-steps"></a>Järgmised sammud

Kui olete olid ei saa ülaltoodud probleemi lahendamiseks, siin on mõned muud ressursid, võite proovida.

- [Ajaveebide]
- [Soovitada funktsioone]
- [Videod]
- [KASS meeskonnatöö ajaveebide]
- [Tugiteenuste Piletite loomine]
- [MSDN-i Foorum]
- [Virnastada ületäitumine Foorum]
- [Twitteri]

<!--Image references-->

<!--Article references-->
[Turvalisuse ülevaade]: ./sql-data-warehouse-overview-manage-security.md
[SSMS]: https://msdn.microsoft.com/library/mt238290.aspx
[SSDT Visual Studio 2015]: ./sql-data-warehouse-install-visual-studio.md
[Draiverid SQL Azure'i andmebaas]: ./sql-data-warehouse-connection-strings.md
[Ühenduse loomine SQL Azure'i andmebaas]: ./sql-data-warehouse-connect-overview.md
[Tugiteenuste Piletite loomine]: ./sql-data-warehouse-get-started-create-support-ticket.md
[Skaleerimist oma SQL-andmebaas]: ./sql-data-warehouse-manage-compute-overview.md
[DWU]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units
[taotleda kvoodi suurendamine]: ./sql-data-warehouse-get-started-create-support-ticket.md#request-quota-change 
[Õppida, kuidas oma päringuid jälgimine]: ./sql-data-warehouse-manage-monitor.md
[Ettevalmistamise juhised]: ./sql-data-warehouse-get-started-provision.md
[Kliendi IP serveri tulemüüri juurdepääsu konfigureerimine]: ./sql-data-warehouse-get-started-provision.md#create-a-new-azure-sql-server-level-firewall
[SQL-i andmebaas head tavad]: ./sql-data-warehouse-best-practices.md
[Tabeli suurust]: ./sql-data-warehouse-tables-overview.md#table-size-queries
[Mittetoetatavad tabelifunktsioonid]: ./sql-data-warehouse-tables-overview.md#unsupported-table-features
[Pole toetatud andmetüübid]: ./sql-data-warehouse-tables-data-types.md#unsupported-data-types
[Overview]: ./sql-data-warehouse-tables-overview.md
[Data types]: ./sql-data-warehouse-tables-data-types.md
[Distribute]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[Temporary]: ./sql-data-warehouse-tables-temporary.md
[Kehva columnstore index kvaliteet]: ./sql-data-warehouse-tables-index.md#causes-of-poor-columnstore-index-quality
[Taastada indeksid lõigu kvaliteedi parandamiseks]: ./sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality
[Töökoormus haldus]: ./sql-data-warehouse-develop-concurrency.md
[Kasutades CTAS lahendamiseks toetamata värskendamine ja kustutamine süntaks]: ./sql-data-warehouse-develop-ctas.md#using-ctas-to-work-around-unsupported-features
[VÄRSKENDAGE lahendused]: ./sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-update-statements
[Kustutage lahendused]: ./sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-delete-statements
[Ühenda lahendused]: ./sql-data-warehouse-develop-ctas.md#replace-merge-statements
[Salvestatud protseduur piirangud]: ./sql-data-warehouse-develop-stored-procedures.md#limitations
[SQL Azure'i andmebaas autentimine]: ./sql-data-warehouse-authentication.md
[Töö ümber PolyBase UTF-8 nõue]: ./sql-data-warehouse-load-polybase-guide.md#working-around-the-polybase-utf-8-requirement

<!--MSDN references-->
[sys.database_principals]: https://msdn.microsoft.com/library/ms187328.aspx
[FUNKTSIOON LOOMINE]: https://msdn.microsoft.com/library/mt203952.aspx
[sqlcmd]: https://azure.microsoft.com/en-us/documentation/articles/sql-data-warehouse-get-started-connect-sqlcmd/

<!--Other Web references-->
[Ajaveebide]: https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/
[KASS meeskonnatöö ajaveebide]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/
[Soovitada funktsioone]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[MSDN-i Foorum]: https://social.msdn.microsoft.com/Forums/home?forum=AzureSQLDataWarehouse
[Virnastada ületäitumine Foorum]: http://stackoverflow.com/questions/tagged/azure-sqldw
[Twitteri]: https://twitter.com/hashtag/SQLDW
[Videod]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse

