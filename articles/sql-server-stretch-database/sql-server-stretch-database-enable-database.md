<properties
    pageTitle="Venitamine andmebaasi lubamine andmebaasi | Microsoft Azure'i"
    description="Saate teada, kuidas konfigureerida andmebaasi venitamine andmebaasi."
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
    ms.topic="article"
    ms.date="08/05/2016"
    ms.author="douglasl"/>

# <a name="enable-stretch-database-for-a-database"></a>Andmebaasi venitamine andmebaasi lubamine

Olemasoleva andmebaasi konfigureerimine venitamine andmebaasi, valige **tööülesannete | Venitamine | Luba** andmebaasi SQL Server Management Studio **Lubamine andmebaasi venitada** viisardi avamiseks. Saate kasutada ka Transact\-SQL-i andmebaasi venitamine andmebaasi lubamise kohta.

Kui valite **tööülesannete | Venitamine | Luba** saate üksikuid tabelis ja te pole veel lubanud andmebaasi venitamine andmebaasi, viisard konfigureerib andmebaasi venitamine andmebaasi ja abil saate valida tabelite protsessi osana. Järgige selles teemas asemel [Lubada venitamine andmebaasi tabeli](sql-server-stretch-database-enable-database.md)juhiseid.

Andmebaasi või tabeli venitamine andmebaasi lubamine nõuab db\_omaniku õigused. Andmebaasi venitamine andmebaasi lubamine nõuab ka andmebaasi õigused.

 >   [AZURE.NOTE] Hiljem, kui keelate venitamine andmebaasi, pidage meeles, et keelamine venitamine andmebaasi tabelisse või andmebaasi ei kustutata remote objekti. Kui soovite kustutada serveri tabeli või kaugandmebaas, peate kukutage see Azure haldusportaali abil. Serveri objekte jätkuvalt kanda Azure kulusid kuni kustutamiseni käsitsi.

## <a name="before-you-get-started"></a>Enne alustamist

-   Enne andmebaasi konfigureerimine venitada, soovitame käivitada venitamine andmebaasi Advisor andmebaasid ja tabelid, kellel on õigus venitada tuvastamiseks. Venitamine andmebaasi Advisor tuvastab ka blokeerimise probleemid. Lisateavet leiate teemast [tuvastamine andmebaasid ja venitamine andmebaasi tabelid](sql-server-stretch-database-identify-databases.md).

-   Vaadake üle [piirangud venitada andmebaasi](sql-server-stretch-database-limitations.md).

-   Venitus andmebaasi migreerib Azure'i andmed. Seega peate olema Azure'i konto ja arveldamine tellimuse. Saada Azure kontot, [klõpsake siin](http://azure.microsoft.com/pricing/free-trial/).

-   On vaja luua uue Azure server või olemasoleva Azure serveri valimiseks ühenduse ja login info.

## <a name="EnableTSQLServer"></a>Nõutav: Lubamine venitamine andmebaasi server
Enne andmebaasi venitamine andmebaasi või tabeli, peate selle kohalikus serveris lubada. Selle toimingu jaoks süsteemiadministraator või serveradmin õigused.

-   Kui teil on vaja administraatoriõigusi, **Venitamine andmebaasi lubamine** viisard konfigureerib serveri venitada.

-   Kui teil pole vajalikke õigusi, administraator on selle suvandi lubamiseks käsitsi käivitades **sp\_konfigureerimine** enne, kui käivitate viisardi või administraator on viisardi käivitamiseks.

Venitamine andmebaasi serveri käsitsi käivitage **sp\_konfigureerimine** ja **remote loendiandmete arhiivi** suvandi sisselülitamine. Järgmises näites võimaldab **remote loendiandmete arhiivi** suvand määrates selle väärtuseks 1.

```
EXEC sp_configure 'remote data archive' , '1';
GO

RECONFIGURE;
GO
```
Lisateavet leiate teemast [Konfigureeri serveri andmete arhiivimine serveri konfigureerimist](https://msdn.microsoft.com/library/mt143175.aspx) ja [sp_configure (Transact-SQL-i)](https://msdn.microsoft.com/library/ms188787.aspx).

## <a name="Wizard"></a>Viisardi abil saate andmebaasi venitamine andmebaasi lubamine
Sh venitamine viisardi lubamine andmebaasi kohta leiate teave, mida peate sisestama ja Valikud, mida tuleb teha, vaadata, [käivitades lubamine andmebaasi venitamine viisardi alustamine](sql-server-stretch-database-wizard.md).

## <a name="EnableTSQLDatabase"></a>Kasutage Transact\-SQL-i lubamiseks venitamine andmebaasi andmebaasi
Enne kui saate lubada venitamine andmebaasi üksikud tabelid, peate lubamine andmebaasi.

Andmebaasi või tabeli venitamine andmebaasi lubamine nõuab db\_omaniku õigused. Andmebaasi venitamine andmebaasi lubamine nõuab ka andmebaasi õigused.

1.  Enne alustamist, olemasoleva Azure serveri venitamine andmebaasi migreerib andmete jaoks valida või luua uue Azure server.

2.  Azure'i server, tulemüüri reegli loomine SQL serveri, mis võimaldab SQL Server suhtlemine serveri IP-aadresside vahemiku.

    Te saate kerge vaevaga väärtused peate, kuid tulemüüri reegli loomine üritades serveriga Azure objekti Explorer rakenduses SQL Server Management Studio (SSMS). SSMS abil saate luua reegli, avades järgmine dialoogiboks, mis juba sisaldab nõutud IP address väärtused.

    ![SSMS tulemüüri reegli loomine][FirewallRule]

3.  SQL serveri andmebaasi konfigureerimiseks venitamine andmebaasi andmebaasi peab olema andmebaasi lahti. Andmebaasi juhtslaidi võti tagab identimisteavet, mida venitamine andmebaasi kasutab serveri andmebaasiga ühenduse loomiseks. Siin on näide, mis loob uue andmebaasi lahti.

    ```tsql
    USE <database>;
    GO

    CREATE MASTER KEY ENCRYPTION BY PASSWORD ='<password>';
    GO
    ```

    Juhtslaidi võti andmebaasi kohta lisateabe saamiseks vt [loomine JUHTSLAIDI klahv (Transact-SQL-i)](https://msdn.microsoft.com/library/ms174382.aspx) ja [loomine andmebaasi lahti](https://msdn.microsoft.com/library/aa337551.aspx).

4.  Andmebaasi konfigureerimisel venitamine andmebaasi tuleb sisestada mandaadi venitamine andmebaasi kasutamiseks suhtlemine ja sees tööruumides SQL Server Azure'i kaugserverisse. Teil on kaks võimalust.

    -   Saate sisestada ka administraatori identimisteavet.

        -   Kui lubate venitamine andmebaasi viisardi töötab, saate luua mandaati sel ajal.

        -   Kui kavatsete lubada venitamine andmebaasi käivitades **Muuda andmebaasi**, peate looma mandaati käsitsi enne **Andmebaasi muutmine** lubamiseks venitamine andmebaasi käivitamist.

        Siin on näide, mis loob uue mandaati.

        ```tsql
        CREATE DATABASE SCOPED CREDENTIAL <db_scoped_credential_name>
            WITH IDENTITY = '<identity>' , SECRET = '<secret>';
        GO
        ```

        Mandaadi lisateavet, lugege teemat [Luua andmebaasi VEEBIRAKENDUST MANDAADI (Transact-SQL-i)](https://msdn.microsoft.com/library/mt270260.aspx). Mandaadi loomise jaoks on vaja muuta kõik MANDAATI õigused.

    -   Saate välise teenusekonto SQL Server Azure'i kaugserverisse suhelda, kui järgmised tingimused on täidetud kõik.

        -   Teenuse konto, mis töötab SQL serveri eksemplar on domeeni konto.

        -   Domeenikonto kuulub kelle Active Directory on ühendatud Azure Active Directory domeeni.

        -   Azure'i remote server on konfigureeritud toetama Azure Active Directory autentimine.

        -   Teenuse konto, mis töötab SQL serveri eksemplar peab olema konfigureeritud dbmanager või süsteemiadministraator konto Azure kaugserverisse.

5.  Andmebaasi konfigureerimine venitamine andmebaasi, käivitage käsk Muuda andmebaasi.

    1.  Pakuvad argumendi serveri nimi olemasoleva Azure serveri, sh selle `.database.windows.net` nime osa \- näiteks `MyStretchDatabaseServer.database.windows.net`.

    2.  Anda mõne olemasoleva administraatori identimisteavet argumendi MANDAATI või määrake Mikroneesia\_teenuse\_konto = sisse. Järgmises näites sisaldab mõne olemasoleva mandaati.

    ```tsql
    ALTER DATABASE <database name>
        SET REMOTE_DATA_ARCHIVE = ON
            (
                SERVER = '<server_name>',
                CREDENTIAL = <db_scoped_credential_name>
            ) ;
    GO
    ```

## <a name="next-steps"></a>Järgmised sammud
-   [Luba venitamine andmebaasi tabeli](sql-server-stretch-database-enable-table.md) lubamiseks täiendavaid tabeleid.

-   [Kuvari venitamine andmebaasi](sql-server-stretch-database-monitor.md) andmete migreerimise oleku vaatamiseks.

-   [Viige ja venitamine andmebaasi jätkamine](sql-server-stretch-database-pause.md)

-   [Haldamine ja venitamine andmebaasi tõrkeotsing](sql-server-stretch-database-manage.md)

-   [Varukoopia venitamine lubatud andmebaasid](sql-server-stretch-database-backup.md)

## <a name="see-also"></a>Vt ka

[Andmebaasid ja tabelite venitamine andmebaasi tuvastamine](sql-server-stretch-database-identify-databases.md)

[Muuda andmebaasi suvandite seadmine (Transact-SQL)](https://msdn.microsoft.com/library/bb522682.aspx)

[FirewallRule]: ./media/sql-server-stretch-database-enable-database/firewall.png
