<properties
   pageTitle="SQL Azure'i andmebaasi turvalisuse juhised ja piirangud | Microsoft Azure'i"
   description="Lisateavet Microsoft Azure'i SQL-andmebaasi juhised ja seotud piirangud."
   services="sql-database"
   documentationCenter=""
   authors="BYHAM"
   manager="jhubbard"
   editor=""
   tags=""/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="10/18/2016"
   ms.author="rickbyh"/>

# <a name="azure-sql-database-security-guidelines-and-limitations"></a>Azure'i SQL-andmebaasi turvalisuse juhised ja piirangud

Selles artiklis kirjeldatakse Microsoft Azure'i SQL-andmebaasi juhised ja seotud piirangud. Kui SQL Azure'i andmebaasi hallata, võtke arvesse järgmist.

## <a name="access-to-the-virtual-master-database"></a>Virtuaalne juhtslaidi andmebaasile pääsevad juurde

Tavaliselt on vaja vaid administraatorid juhtslaidi andmebaasile pääsevad juurde. Tavalised andmebaasile pääsevad juurde iga kasutaja peaks olema administraator keskkonnas andmebaasi kasutajate loodud iga andmebaasi kaudu. Suletud andmebaasi kasutajate kasutamisel peate looma sisselogimise põhi andmebaasi. Lisateabe saamiseks vt [Sisalduvad andmebaasi kasutajate - teha oma andmebaasi Portable](https://msdn.microsoft.com/library/ff929188.aspx).


## <a name="firewall"></a>Tulemüür

SQL serveri tulemüür, mis on rakendatud kogu Azure SQL Server on konfigureeritud tavaliselt portaali kaudu ja peaks ainult ooteruumis administraatorite kasutatavad IP-aadressid. Andmebaasi ulatusega tulemüüri reegel, mis avab vajalikud vahemiku IP-aadresside iga andmebaasi loomiseks kasutaja andmebaasiga ühenduse loomiseks ja seejärel kasutage [sp_set_database_firewall_rule](https://msdn.microsoft.com/library/dn270010.aspx) Transact-SQL-lause.

Azure'i SQL-andmebaasi teenus kättesaadav ainult TCP pordi 1433 kaudu. SQL-i andmebaasile juurde pääseda oma arvutist, veenduge, et kliendi arvuti tulemüür võimaldab väljamineva TCP side TCP port 1433. Kui muude rakenduste ei vaja, blokeerida TCP port 1433 sissetulevaid ühendusi. 

Protsessi ühenduse osana ühendused: Azure'i virtuaalmasinates suunatakse teid erinevate IP-aadress ja port kordumatu iga töötaja roll. Pordi number on vahemikus 11000-11999. TCP-pordid kohta leiate lisateavet teemast [pordid 1433 4.5 ADO.net-i ja SQL-i andmebaasi V12 Lisaks](sql-database-develop-direct-route-ports-adonet-v12.md).


## <a name="authentication"></a>Autentimine

Kasutage Active Directory autentimine (integreeritud turvalisus) iga kord, kui võimalik. AD autentimise konfigureerimise kohta leiate teavet teemast [ühenduse loomine SQL-i andmebaasi, kasutades Azure Active Directory autentimise](sql-database-aad-authentication.md)ja [valides soovitud autentimisrežiim](https://msdn.microsoft.com/library/ms144284.aspx) SQL serveri raamatuid online. 

Esitatud andmebaasi kasutajatele täiustamiseks skaleeritavus. Lisateabe saamiseks leiate [Sisalduvad andmebaasi kasutajate - teha oma andmebaasi Portable](https://msdn.microsoft.com/library/ff929188.aspx), [(Transact-SQL-i) kasutaja loomiseks](https://technet.microsoft.com/library/ms173463.aspx)ja [Sisaldas andmebaasid](https://technet.microsoft.com/library/ff929071.aspx).

Andmebaasimootor suleb ühendusi, mis jäävad jõude rohkem kui 30 minutit. Ühendus peab login uuesti enne kasutamist.

Pidevalt tuleb aktiivne ühendusi SQL-andmebaasiga (läbi andmebaasimootor) reauthorization vähemalt iga 10 tundi. Andmebaasimootor avaldab reauthorization algselt esitatud parooliga ja pole kasutaja sisendist on nõutav. Jõudluse huvides parooli lähtestamisel SQL-andmebaasi ühendus pole reauthenticated isegi juhul, kui ühendus on lähtestatud tõttu ühenduse ühiskasutus. See erineb asutusesisese SQL serveri käitumist. Kui parool on muutunud, kuna algselt lubati ühenduse, tuleb ühendus lõpetada ja uue ühenduse tehtud abil uus parool. TAPPA ANDMEBAASIÜHENDUSE õigustega kasutaja saab lõpetada konkreetselt SQL-andmebaasiga ühenduse [TAPPA](https://msdn.microsoft.com/library/ms173730.aspx) käsu abil.

## <a name="logins-and-users"></a>Sisselogimise ja kasutajate

Kui haldamine sisselogimise ja kasutajate SQL-andmebaasis, on piiratud.


- Teil peab olema ühendatud **põhi** andmebaasi täitmisel on ``CREATE/ALTER/DROP DATABASE`` laused. -Vastav serveritaseme põhisumma login juhtslaidi andmebaasi andmebaasi kasutaja ei saa muuta ega lähevad. 
- USA-inglise keeles on serveritaseme põhisumma login vaikekeele.
- Ainult administraatorid (serveritaseme põhisumma login või Azure AD administraator) ja liikmete **dbmanager** andmebaasi rolli **juhtslaidi** andmebaasis on õigus käivitada soovitud ``CREATE DATABASE`` ja ``DROP DATABASE`` laused.
- Teil peab olema ühendatud põhi andmebaasi täitmisel on ``CREATE/ALTER/DROP LOGIN`` laused. Samas on takistada sisselogimise abil. Kasutage selle asemel keskkonnas andmebaasi kasutajad.
- Kasutaja andmebaasiga ühenduse, peate sisestama andmebaasi ühendusstringi nime.
- Ainult serveritaseme põhisumma login ja liikmete **loginmanager** andmebaasi rolli **juhtslaidi** andmebaasis on õigus käivitada soovitud ``CREATE LOGIN``, ``ALTER LOGIN``, ja ``DROP LOGIN`` laused.
- Kui soovitud ``CREATE/ALTER/DROP LOGIN`` ja ``CREATE/ALTER/DROP DATABASE`` parameetritega käskude kasutamine rakenduses ADO.NET laused, pole lubatud. Lisateavet leiate teemast [käskude ja parameetrite](https://msdn.microsoft.com/library/ms254953.aspx).
- Kui soovitud ``CREATE/ALTER/DROP DATABASE`` ja ``CREATE/ALTER/DROP LOGIN`` laused, iga aruande peab olema ainult partii Transact-SQL-lause. Muul juhul ilmneb tõrge. Näiteks järgmine Transact-SQL-i kontrollib, kas on olemas andmebaasi. Kui see on olemas, on ``DROP DATABASE`` lause nimetatakse eemaldamiseks andmebaasi. Kuna selle ``DROP DATABASE`` lause pole ainult lause paketi, käivitamisel järgmine Transact-SQL-lause tulemuseks viga.

```
IF EXISTS (SELECT [name]
           FROM   [sys].[databases]
           WHERE  [name] = N'database_name')
     DROP DATABASE [database_name];
GO
```

- Täitmisel on ``CREATE USER`` koos selle ``FOR/FROM LOGIN`` suvand peab olema ainult partii Transact-SQL-lause.
- Täitmisel on ``ALTER USER`` koos selle ``WITH LOGIN`` suvand peab olema ainult partii Transact-SQL-lause.
- Kui soovite ``CREATE/ALTER/DROP`` kasutamiseks on vaja selle ``ALTER ANY USER`` andmebaasi õigus.
- Andmebaasi rolli omanik püüab lisada või eemaldada mõne muu andmebaasi kasutaja või sealt andmebaasi rolli, võib ilmneda järgmine tõrketeade: **kasutajate ja rollide "Nimi" pole selles andmebaasis.** See tõrge ilmneb kasutaja ei näe omanik. Selle probleemi lahendamiseks anda rolli omanik on ``VIEW DEFINITION`` kasutaja õigus. 

Sisselogimise ja kasutajate kohta leiate lisateavet teemast [andmebaaside haldamine ja sisselogimise Azure SQL-andmebaasis](sql-database-manage-logins.md).


## <a name="see-also"></a>Vt ka

[SQL Azure'i andmebaasi tulemüüri](sql-database-firewall-configure.md)

[Kuidas: konfigureerimine tulemüüri sätted (SQL Azure'i andmebaas)](sql-database-configure-firewall-settings.md)

[Andmebaasid ja sisselogimise Azure'i SQL-andmebaasi haldamine](sql-database-manage-logins.md)

[Turbekeskus andmebaasimootor SQL Server ja Azure SQL-andmebaas](https://msdn.microsoft.com/library/bb510589)
