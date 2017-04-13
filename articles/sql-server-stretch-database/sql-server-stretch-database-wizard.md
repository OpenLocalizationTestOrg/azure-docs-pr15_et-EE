<properties
    pageTitle="Alustuseks lubamine andmebaasi opsüsteemi venitamine viisardi | Microsoft Azure'i"
    description="Saate teada, kuidas konfigureerida andmebaasi venitamine andmebaasi käivitades lubamine andmebaasi venitamine viisardit."
    services="sql-server-stretch-database"
    documentationCenter=""
    authors="douglaslMS"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-server-stretch-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="08/05/2016"
    ms.author="douglasl"/>

# <a name="get-started-by-running-the-enable-database-for-stretch-wizard"></a>Alustuseks opsüsteemi venitamine viisardis andmebaasi lubamine

Andmebaasi konfigureerimine venitamine andmebaasi, käivitage lubamine andmebaasi venitamine viisardit.  Selles teemas kirjeldatakse teave, mida peate sisestama ja Valikud, mida peate tegema viisardi.

Venitamine andmebaasi kohta leiate lisateavet teemast [Venitamine andmebaasi](sql-server-stretch-database-overview.md).

 >   [AZURE.NOTE] Hiljem, kui keelate venitamine andmebaasi, pidage meeles, et keelamine venitamine andmebaasi tabelisse või andmebaasi ei kustutata remote objekti. Kui soovite kustutada serveri tabeli või kaugandmebaas, peate kukutage see Azure haldusportaali abil. Serveri objekte jätkuvalt kanda Azure kulusid kuni kustutamiseni käsitsi. 

## <a name="launch-the-wizard"></a>Käivitage viisard

1.  Valige SQL Server Management Studio Exploreris objekti andmebaasi, kuhu soovite lubada venitamine.

2.  Paremal\-klõpsake ja valige **toimingud**, ja valige **venitada**ja valige **Luba** viisardi käivitamiseks.

## <a name="Intro"></a>Sissejuhatus
Vaadake üle viisard ja eeltingimused eesmärk.

Olulised eeltingimused on järgmised:

-   Peate olema administraator andmebaasi sätete muutmiseks.
-   Teil on Microsoft Azure'i tellimus.
-   SQL serveris peab olema Azure kaugserverisse suhelda.

![Sissejuhatus venitamine andmebaasi viisardi lehel][StretchWizardImage1]

## <a name="Tables"></a>Valige tabelid
Valige tabelid, mida soovite lubada venitada.

Paljude ridadega tabelid kuvatakse sorditud loendi alguses. Enne viisard kuvab tabelite loendist, seda analüüsib neid andmetüüpe, mis ei toeta praegu venitamine andmebaasi jaoks.

![Venitamine andmebaasi viisardi lehel Vali tabelid][StretchWizardImage2]

|Veerg|Kirjeldus|
|----------|---------------|
|(tiitlit pole)|Märkige ruut selle veeru venitada valitud tabeli lubamiseks.|
|**Nimi**|Saate määrata tabelis veeru nimi.|
|(tiitlit pole)|Selles veerus sümbol võib tähistada hoiatus millel\'t takistada saate lubada valitud tabeli venitada. Blokeerimine probleem, mis takistab lubada valitud tabeli venitada ka kujutada \- näiteks, kuna tabeli kasutab toetuseta andmetüübi. Viige kursor sümboli kuvada rohkem teavet kohtspikker. Lisateavet leiate teemast [piirangud venitamine andmebaasi](sql-server-stretch-database-limitations.md).|
|**Tõmmatud**|Näitab, kas tabel on juba lubatud venitamine.|
|**Migreerimine**|Terve tabeli (**Kogu tabel**) saate migreerida või filtri saate määrata tabelis veeru. Kui soovite teise filtri funktsiooni abil saate valida ridade migreerida, käivitage funktsioon filter määramiseks pärast väljumist viisardi lause ALTER TABLE. Funktsioon filter kohta lisateabe saamiseks vt [Valige read migreerimiseks filtri funktsiooni abil](sql-server-stretch-database-predicate-function.md). Lisateabe saamiseks selle kohta, kuidas kasutada funktsiooni, lugege [Lubamine venitamine andmebaasi tabelisse](sql-server-stretch-database-enable-table.md) või [ALTER TABLE (Transact-SQL-i)](https://msdn.microsoft.com/library/ms190273.aspx).|
|**Read**|Saate määrata tabeli ridade arvu.|
|**Suurus (KB)**|Saate määrata KB tabeli suurust.|

## <a name="Filter"></a>Soovi korral sisestage rea filter

Kui soovite esitada filtrifunktsioonist migreerimiseks ridade valimiseks, tehke lehel **Vali tabelid** järgmisi asju.

1.  Klõpsake loendis **Valige tabelid, mida soovite venitada** tabeli rida **Kogu tabel** . Avaneb dialoogiboks **Valige read venitada** .

    ![Funktsioon filter määratlemine][StretchWizardImage2a]

2.  Valige dialoogiboksis **Valige read venitada** **Valige read**.

3.  Sisestage **väljale nimi**nimi, funktsioon filter.

4.  **Kus** klauslit, valige veerg tabelis, valige tehtemärgi ja sisestage väärtus.

5. Klõpsake funktsiooni testimiseks **märkige ruut** . Kui tabeli -, tagastab funktsioon tulemusi, kui migreerimine ridu, mis vastavad tingimusele - test aruannete **edu**.

    >   [AZURE.NOTE] Tekstivälja, mis kuvab päringu filter on kirjutuskaitstud. Ei saa muuta päringu tekstiväljale.

6.  Klõpsake nuppu valmis, **Valige tabelite** lehele naasmiseks.

Funktsioon filter on loodud SQL serveri ainult siis, kui olete lõpetanud viisardi. Seni, saate seda muuta või ümber nimetada funktsioon filter **Valige tabelid** lehele naasmiseks.

![Valige tabelid lehe pärast määratlemine funktsioon filter][StretchWizardImage2b]

Kui soovite kasutada mõnda muud tüüpi funktsioon filter valige read migreerida, tehke ühte järgmistest.  

-   Sulgege viisard ja käivitage lause ALTER TABLE tabeli venitamine lubamine ja täpsustada funktsioon filter. Lisateavet leiate teemast [Lubamine venitamine andmebaasi tabelisse](sql-server-stretch-database-enable-table.md).  

-   Funktsioon filter määramiseks pärast väljumist viisardi lause ALTER TABLE käivitada. Nõutav juhised leiate teemast [filtreerimine funktsiooni pärast viisardi käivitamist](sql-server-stretch-database-predicate-function.md#addafterwiz).

## <a name="Configure"></a>Azure'i juurutuse konfigureerimine

1.  Logige sisse Microsoft Azure'i Microsofti kontoga.

    ![Logige sisse Azure - venitamine andmebaasi viisard][StretchWizardImage3]

2.  Valige olemasolev Azure'i tellimus, venitamine andmebaasi kasutama.

3.  Valige Azure piirkond.
    -   Kui loote uue serveri, luuakse server selle piirkonna.  
    -   Kui teil on olemasolevaid serverid valitud ala, viisardi loendis nende **olemasolev serveri**valimisel.

    Valige latentsus minimeerimiseks Azure piirkond, kus asub SQL serveris. Lisateavet piirkondade leiate [Azure'i piirkondade](https://azure.microsoft.com/regions/).

4.  Määrake, kas soovite kasutada olemasoleva serveri või luua uue Azure server.

    Kui Active Directory SQL serveris on ühendatud Azure Active Directory, soovi korral saate ühendatud teenusekonto SQL Server Azure'i kaugserverisse suhelda. See suvand nõuete kohta lisateabe saamiseks, vt [Suvandid muuta andmebaasi määramine (Transact-SQL-i)](https://msdn.microsoft.com/library/bb522682.aspx).

    -   **Saate luua uue serveri**

        1.  Luua serveri administraatori kasutajanime ja parooli.

        2.  Võite kasutada ühendatud teenusekonto SQL Server Azure'i kaugserverisse suhelda.

        ![Saate luua uue Azure serveri - venitamine andmebaasi viisard][StretchWizardImage4]

    -   **Olemasoleva server**

        1.  Valige olemasolev Azure server.

        2.  Valige autentimise meetodit.

            -   Kui valite **SQL serveri autentimine**, sisestage administraatori kasutajanime ja parooli.

            -   Valige ühendatud teenusekonto SQL serveri abil saate suhelda kaugserverisse Azure **Active Directory integreeritud autentimise** . Kui valitud server on integreeritud Azure Active Directory, siis seda suvandit ei kuvata.

        ![Valige olemasolev Azure server - venitamine andmebaasi viisard][StretchWizardImage5]

## <a name="Credentials"></a>Turvaline identimisteave
Teil on lahti andmebaasi turvamiseks identimisteavet, mida venitamine andmebaasi kasutab serveri andmebaasiga ühenduse loomiseks.  

Kui andmebaas lahti juba olemas, sisestage parool selle.  

![Turvaline identimisteabe venitamine andmebaasi viisardi lehel][StretchWizardImage6b]

Kui andmebaas ei ole olemasoleva juhtslaidi võti, sisestage keeruka parooli loomine andmebaasi lahti.  

![Turvaline identimisteabe venitamine andmebaasi viisardi lehel][StretchWizardImage6]

Juhtslaidi võti andmebaasi kohta lisateabe saamiseks vt [loomine JUHTSLAIDI klahv (Transact-SQL-i)](https://msdn.microsoft.com/library/ms174382.aspx) ja [loomine andmebaasi lahti](https://msdn.microsoft.com/library/aa337551.aspx). Viisard loob mandaadi lisateavet, lugege teemat [Luua andmebaasi VEEBIRAKENDUST MANDAADI (Transact-SQL-i)](https://msdn.microsoft.com/library/mt270260.aspx).

## <a name="Network"></a>Valige IP-aadress
Azure, mis võimaldab SQL Server Azure'i kaugserverisse suhelda tulemüüri reegli loomiseks kasutada alamvõrgu IP address vahemikus (soovitatav) või SQL serveris avaliku IP-aadress.

IP-aadress või aadressid, mis pakub sellel lehel räägi Azure server sissetulevad andmed, päringuid ja SQL Server Azure'i tulemüüri läbida algataja toimingute lubamiseks. Viisard ei muuda midagi SQL serveri tulemüüri sätted.

![Valige IP address venitamine andmebaasi viisardi lehel][StretchWizardImage7]

## <a name="Summary"></a>Kokkuvõte
Vaadake üle sisestatud väärtuste ja valitud viisard ja Azure arvestuslike kulude soovitud suvandid. Seejärel valige **valmis** venitamine lubamine.

![Kokkuvõte venitamine andmebaasi viisardi lehel][StretchWizardImage8]

## <a name="Results"></a>Tulemused
Vaadake tulemused üle.

Andmete migreerimise oleku jälgimine, lugege teemat [jälgimine ja tõrkeotsing andmete migreerimise (venitamine andmebaas)](sql-server-stretch-database-monitor.md).

![Venitamine andmebaasi viisardi lehel tulemused][StretchWizardImage9]

## <a name="KnownIssues"></a>Viisardi tõrkeotsing
**Venitamine andmebaasi viisard nurjus.**
Venitamine andmebaasi pole veel serveri tasemel lubatud, kui käivitate viisardi ilma süsteemi lubamiseks administraatoriõigused, viisardi nurjub. Süsteemiadministraatori lubamiseks venitamine andmebaasist kohaliku serveri eksemplar ja seejärel käivitage viisard uuesti. Lisateavet leiate teemast [eelnevalt nõutud: õigus lubada venitamine andmebaasi serveris](sql-server-stretch-database-enable-database.md#EnableTSQLServer).

## <a name="next-steps"></a>Järgmised sammud
Täiendavate tabelite lubamine venitamine andmebaasi. Andmete migreerimise jälgimine ja haldamine venitamine\-lubatud andmebaase ja tabeleid.

-   [Luba venitamine andmebaasi tabeli](sql-server-stretch-database-enable-table.md) lubamiseks täiendavaid tabeleid.

-   [Jälgimine ja tõrkeotsing andmete migreerimise](sql-server-stretch-database-monitor.md) andmete migreerimise oleku vaatamiseks.

-   [Viige ja venitamine andmebaasi jätkamine](sql-server-stretch-database-pause.md)

-   [Haldamine ja venitamine andmebaasi tõrkeotsing](sql-server-stretch-database-manage.md)

-   [Varukoopia venitamine lubatud andmebaasid](sql-server-stretch-database-backup.md)

## <a name="see-also"></a>Vt ka

[Andmebaasi venitamine andmebaasi lubamine](sql-server-stretch-database-enable-database.md)

[Tabeli venitamine andmebaasi lubamine](sql-server-stretch-database-enable-table.md)

[StretchWizardImage1]: ./media/sql-server-stretch-database-wizard/stretchwiz1.png
[StretchWizardImage2]: ./media/sql-server-stretch-database-wizard/stretchwiz2.png
[StretchWizardImage2a]: ./media/sql-server-stretch-database-wizard/stretchwiz2a.png
[StretchWizardImage2b]: ./media/sql-server-stretch-database-wizard/stretchwiz2b.png
[StretchWizardImage3]: ./media/sql-server-stretch-database-wizard/stretchwiz3.png
[StretchWizardImage4]: ./media/sql-server-stretch-database-wizard/stretchwiz4.png
[StretchWizardImage5]: ./media/sql-server-stretch-database-wizard/stretchwiz5.png
[StretchWizardImage6]: ./media/sql-server-stretch-database-wizard/stretchwiz6.png
[StretchWizardImage6b]: ./media/sql-server-stretch-database-wizard/stretchwiz6b.png
[StretchWizardImage7]: ./media/sql-server-stretch-database-wizard/stretchwiz7.png
[StretchWizardImage8]: ./media/sql-server-stretch-database-wizard/stretchwiz8.png
[StretchWizardImage9]: ./media/sql-server-stretch-database-wizard/stretchwiz9.png
