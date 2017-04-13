<properties
    pageTitle="SQL-andmebaasi õpetus: SQL-i andmebaasi loomine | Microsoft Azure'i"
    description="Saate teada, kuidas häälestada loogilise SQL-andmebaasi server server tulemüüri reegel SQL-andmebaasi ja näidisandmed. Samuti saate teada, kuidas ühendada kliendi tööriistad, konfigureerimine kasutajatele ja andmebaasi tulemüüri reegli häälestamine."
    keywords="SQL-i andmebaasi õpetuses SQL-i andmebaasi loomine"
    services="sql-database"
    documentationCenter=""
    authors="CarlRabeler"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="09/07/2016"
    ms.author="carlrab"/>


# <a name="sql-database-tutorial-create-a-sql-database-in-minutes-by-using-the-azure-portal"></a>SQL-andmebaasi õpetus: Azure'i portaali kaudu luua SQL-andmebaasi minutit

> [AZURE.SELECTOR]
- [Azure'i portaal](sql-database-get-started.md)
- [C#:](sql-database-get-started-csharp.md)
- [PowerShelli](sql-database-get-started-powershell.md)

Selles õppetükis saate teada, kuidas kasutada Azure portaali, et:

- SQL Azure'i andmebaasi loomine näidisandmetega.
- Ühe IP-aadress või vahemik IP-aadresside serveritaseme tulemüüri reegli loomine.

Kas [C#](sql-database-get-started-csharp.md) või [PowerShelli](sql-database-get-started-powershell.md)abil saate teha neid samu toiminguid.

[AZURE.INCLUDE [Login](../../includes/azure-getting-started-portal-login.md)]

<a name="create-logical-server-bk"></a>

## <a name="create-your-first-azure-sql-database"></a>Esimese Azure SQL-i andmebaasi loomine 

1. Kui te pole praegu ühendatud, ühenduse [Azure'i portaalis](http://portal.azure.com).
2. Klõpsake nuppu **Uus**, klõpsake **andmete + salvestusruumi**ja otsige **SQL-andmebaasi**.

    ![Uue sql-andmebaasi 1](./media/sql-database-get-started/sql-database-new-database-1.png)

3. Klõpsake käsku **SQL-andmebaasi** SQL-andmebaasi tera avamiseks. See blade sisu erineb sõltuvalt arvu tellimuste ja teie olemasoleva objektide (nt olemasoleva serverid).

    ![Uue sql-andmebaasi 2](./media/sql-database-get-started/sql-database-new-database-2.png)

4. Sisestage väljale **andmebaasi nimi** nimi, esimese andmebaasi – näiteks "minu andmebaasi". Roheline märge näitab, et olete andnud sobiv nimi.

    ![Uue sql-andmebaasi 3](./media/sql-database-get-started/sql-database-new-database-3.png)

5. Kui teil on mitu tellimust, valige tellimus.
6. **Ressursirühm**, all nuppu **Loo uus** ja sisestage esimese ressursirühma – näiteks "minu--ressursirühm" nimi. Roheline märge näitab, et olete andnud sobiv nimi.

    ![Uue sql-andmebaasi 4](./media/sql-database-get-started/sql-database-new-database-4.png)

7. Jaotises **Vali tulemiallikas**, klõpsake nuppu **proovi** ja valige jaotises **valimi** **AdventureWorksLT [V12]**.

    ![Uue sql-andmebaasi 5](./media/sql-database-get-started/sql-database-new-database-5.png)

8. Klõpsake jaotises **serveri** **konfigureerimine vajalik sätted**.

    ![Uue sql-andmebaasi 6](./media/sql-database-get-started/sql-database-new-database-6.png)

9. Enne Server, klõpsake nuppu **Loo uus server**. Azure'i SQL-andmebaasi server objekti, mis võib olla uus server või olemasoleva serveri luuakse.

    ![Uue sql-andmebaasi 7](./media/sql-database-get-started/sql-database-new-database-7.png)

10. Vaadake üle **uue serveri** tera mõistmiseks peate sisestama oma uue serveri teave.

    ![Uue sql-andmebaasi 8](./media/sql-database-get-started/sql-database-new-database-8.png)

11. Sisestage väljale **Serveri nimi** nimi, esimese serverisse - nagu "minu-uue-server-objekt". Roheline märge näitab, et olete andnud sobiv nimi.

    ![Uue sql-andmebaasi 9](./media/sql-database-get-started/sql-database-new-database-9.png)
 
12. Jaotises **serveri administraator sisselogimine**, sisestage administraatori sisselogimise kasutajanimi selles serveris – näiteks "minu-administraatori-konto". See sisselogimine tuntakse serveri põhisumma Logi sisse. Roheline märge näitab, et olete andnud sobiv nimi.

    ![Uue sql-andmebaasi 10](./media/sql-database-get-started/sql-database-new-database-10.png)

13. Jaotises **parool** ja **parooli kinnitus**, sisestada parool serveri põhisumma login konto – näiteks "p@ssw0rd1". Roheline märge näitab, et olete andnud kehtiv parool.

    ![Uue sql-andmebaasi 11](./media/sql-database-get-started/sql-database-new-database-11.png)
 
14. Valige jaotises **asukoht**data Centeri kaudu, mis on asjakohane teie asukohale – näiteks "Austraalia Ida".

    ![Uue sql-andmebaasi 12](./media/sql-database-get-started/sql-database-new-database-12.png)

15. Klõpsake jaotises ** loomine V12 server (värskenduses), Pange tähele, on ainult võimalus luua Azure SQL serveri praeguse versiooni.

    ![Uue sql-andmebaasi 13](./media/sql-database-get-started/sql-database-new-database-13.png)

16. Märkate, et, vaikimisi märkeruut **Luba Azure'i** teenuste juurde pääseda server on valitud ja ei saa muuta. See on täpsemalt suvandit. Saate muuta selle sätte serveri tulemüüri sätteid selle serveri objekti, kuigi enamik stsenaariumid see ei ole vaja.

    ![Uue sql-andmebaasi 14](./media/sql-database-get-started/sql-database-new-database-14.png)

17. Enne uue server, vaadake oma valikut ja klõpsake selle uue andmebaasi jaoks uue serveri valimiseks **Valige** .

    ![Uue sql-andmebaasi 15](./media/sql-database-get-started/sql-database-new-database-15.png)

18. Enne SQL-andmebaasiga, **hinnakirjad taseme**, klõpsake **S2 Standard** ja seejärel nuppu **tavaline** kallim hinnakirjad taseme esimese andmebaasi valimiseks. Saate alati hinnakirjad taseme hiljem muuta.

    ![Uue sql-andmebaasi 16](./media/sql-database-get-started/sql-database-new-database-16.png)

19. Enne SQL-andmebaasiga, vaadake oma valikut ja seejärel käsku **Loo** luua oma esimene server ja andmebaas. Teie esitatud väärtused on kinnitatud ja juurutamise käivitatakse.

    ![Uue sql-andmebaasi 17](./media/sql-database-get-started/sql-database-new-database-17.png)

20. Klõpsake tööriistaribal portaali juurutamise oleku **teated** üksused.

    ![Uue sql-andmebaasi 18](./media/sql-database-get-started/sql-database-new-database-18.png)

>[AZURE.IMPORTANT]Kui juurutamise lõpule jõudnud, luuakse Azure'i oma uue Azure SQL server ja andmebaas. Te ei saa oma uue serveri või SQL serveri tööriistade abil, kuni olete loonud avamiseks tulemüür ühenduste väljaspool Azure SQL-andmebaasi server tulemüüri reegli andmebaasi ühenduse.

[AZURE.INCLUDE [Create server firewall rule](../../includes/sql-database-create-new-server-firewall-portal.md)]

## <a name="next-steps"></a>Järgmised sammud
Nüüd, kui olete selle õpetuse SQL-andmebaasi ning loodud andmebaasi koos näidisandmetega, olete valmis oma lemmik tööriistade abil uurimine.

- Kui olete tuttav Transact-SQL-i ja SQL Server Management Studio (SSMS), saate teada, kuidas [ühenduse loomine ja päringu SQL-i andmebaasi SSMS](sql-database-connect-query-ssms.md).

- Kui teate, Excel, saate teada, kuidas [Exceli Azure SQL-andmebaasiga ühenduse loomine](sql-database-connect-excel.md).

- Kui olete valmis alustama kodeerimine, valige oma programmeerimiskeel veebisaidil [andmeühenduste teegid SQL-andmebaas ja SQL Server](sql-database-libraries.md).

- Kui soovite oma asutusesisese SQL serveri andmebaasi liikumiseks Azure, vt lisateavet [SQL-andmebaasiga andmebaasi migreerimine](sql-database-cloud-migrate.md) .

- Kui soovite laadida mõned andmed uude tabelisse CSV-failist BCP käsurea tööriista abil, vt [CSV-faili abil BCP SQL-andmebaasi andmete laadimine](sql-database-load-from-csv-with-bcp.md).

- Kui soovite häälestamisega Azure'i SQL-andmebaasi turvalisus, lugege teemat [Turvalisus töötamise alustamine](sql-database-get-started-security.md)


## <a name="additional-resources"></a>Lisaressursid

[Mis on SQL-andmebaasi?](sql-database-technical-overview.md)
