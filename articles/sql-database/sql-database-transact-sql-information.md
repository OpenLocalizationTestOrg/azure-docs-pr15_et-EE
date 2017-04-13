<properties
   pageTitle="SQL Azure'i andmebaasi T-SQL-i toetuseta | Microsoft Azure'i"
   description="Transact-SQL-laused, mis on väiksem kui täielikult toetatud Azure SQL-andmebaasis"
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
   ms.date="08/30/2016"
   ms.author="rick.byham@microsoft.com"/>

# <a name="azure-sql-database-transact-sql-differences"></a>Azure SQL-i andmebaasi Transact-SQL-i erinevused


Enamik Transact-SQL-i funktsioone, mis sõltuvad rakendused on toetatud Microsoft SQL Server ja Azure SQL-andmebaas. Osalise toetatud funktsioonid rakenduste loendit järgmiselt:

- Andmetüübid.
- Tehtemärke.
- String, aritmeetiline, loogiline ja kursor funktsioone.

Azure'i SQL-andmebaasi on mõeldud funktsioone, mis tahes sõltuvus **põhi** andmebaasi eristada. Seetõttu palju serveritaseme tegevuste puhul SQL-andmebaasi ja ei toetata. Funktsioonid, mis on aegunud SQL serveri SQL-andmebaasis üldiselt ei toetata.

> [AZURE.NOTE]
> Selles teemas käsitletakse funktsioone, mis on saadaval kõige uuemas SQL-andmebaasi uuendamisel praegune versioon; SQL-i andmebaasi V12. V12 kohta leiate lisateavet teemast [SQL-i andmebaasi V12 mis uus](sql-database-v12-whats-new.md).

Järgmistes jaotistes on loetletud funktsioone, mis on osaliselt toetatud ja funktsioone, mis pole täielikult toetatud.


## <a name="features-partially-supported-in-sql-database-v12"></a>SQL-i andmebaasi v12 osaliselt toetatud funktsioonid

SQL-i andmebaasi V12 toetab osa, kuid mitte kõik argumendid, mis on olemas vastav SQL Server 2016 Transact-SQL-lauseid. Näiteks luua toimingu lause on saadaval, ent toimingu loomine kõik suvandid pole saadaval. Leiate üksikasjalikumat teavet iga lause toetatud alade lingitud süntaks teemadest.

- Andmebaasid: [loomine](https://msdn.microsoft.com/library/dn268335.aspx )/[muuta](https://msdn.microsoft.com/library/ms174269.aspx)
- DMVs on üldiselt saadaval funktsioonid, mis on saadaval.
- Ülesanded: [loomine](https://msdn.microsoft.com/library/ms186755.aspx)/[muuta funktsioon](https://msdn.microsoft.com/library/ms186967.aspx)
- [TAPPA](https://msdn.microsoft.com/library/ms173730.aspx) 
- Logimine: [loomine](https://msdn.microsoft.com/library/ms189751.aspx)/[muuta sisselogimine](https://msdn.microsoft.com/library/ms189828.aspx)
- Salvestatud toimingute: [Loo](https://msdn.microsoft.com/library/ms187926.aspx)/[Muuta protseduur](https://msdn.microsoft.com/library/ms189762.aspx)
- Tabelid: [loomine](https://msdn.microsoft.com/library/dn305849.aspx)/[muuta](https://msdn.microsoft.com/library/ms190273.aspx)
- Tüüpi (kohandatud): [Tippige loomine](https://msdn.microsoft.com/library/ms175007.aspx)
- Kasutajad: [loomine](https://msdn.microsoft.com/library/ms173463.aspx)/[muuta kasutaja](https://msdn.microsoft.com/library/ms176060.aspx)
- Vaadete: [loomine](https://msdn.microsoft.com/library/ms187956.aspx)/[muuta vaade](https://msdn.microsoft.com/library/ms173846.aspx)

## <a name="features-not-supported-in-sql-database"></a>Funktsioonid, mis ei toeta SQL-andmebaas

- Süsteemi objektid võrdlemine
- Ühendusega seotud: lõpp-punkti laused, ORIGINAL_DB_NAME. SQL-andmebaasi ei toeta Windowsi autentimist, kuid ei toeta sarnase Azure Active Directory autentimine. Mõned autentimistüüpidest vaja SSMS uusim versioon. Lisateavet leiate teemast [ühenduse loomine SQL-andmebaasi või SQL-i andmete ladu, kasutades Azure Active Directory autentimise](sql-database-aad-authentication.md).
- Rist andmebaasipäringud, kasutades kolme või nelja osa nimesid. (Kirjutuskaitstud rist-andmebaasipäringud toetatakse [elastne andmebaasist päringu](sql-database-elastic-query-overview.md)abil).
- Rist andmebaasi omaniku Aheldamise, usaldusväärseid säte
- Andmekoguja
- Andmebaasi diagrammide
- E-posti andmebaasi
- DATABASEPROPERTY (Kasutage selle asemel DATABASEPROPERTYEX)
- AS Käivita logimine
- Krüptimine: laiendatav võtme haldus
- Kolmevõistlust: sündmusi, sündmuste teatised, päringu teatised
- Funktsioonid, mis on seotud andmebaasi paigutuse, suurus ja andmebaasi faile, mida haldab Microsoft Azure'i automaatselt.
- Funktsioonid, mis on seotud kõrge-saadavus, mida haldab Microsoft Azure'i konto kaudu: varundamine, taastamine AlwaysOn, andmebaasi peegeldus, Logi saatmine, taastamise režiimi. Lisateavet, lugege teemat Azure SQL-i andmebaasi varundamine ja taastamine.
- Funktsioonid, mis töötab SQL-andmebaasi Logi lugeja sõltuvad: Push Dispersioonanalüüs, Muuda andmete jäädvustada.
- Funktsioonid, mis sõltuvad SQL Server Agent või MSDB andmebaasi: töö, teatiste, tehtemärkide, poliitika juhtimise, e-posti andmebaasi, Kesk management serverid.
- FILESTREAM
- Ülesanded: fn_get_sql, fn_virtualfilestats, fn_virtualservernodes
- Globaalne Ajutised tabelid
- Riistvaraga seotud serveri sätted: mälu töötaja teemad, CPU osaleja, jälgi lipud, jne. Kasutage selle asemel teenuse tasemed.
- HAS_DBACCESS
- TAPPA STATISTIKA TÖÖ
- Lingitud serverid, OPENQUERY OPENROWSET, OPENDATASOURCE hulgi lisada ja nelja-osa nimed
- Juhtslaidi/target serverid
- .NET framework [CLR-i ja SQL serveri integreerimine](http://msdn.microsoft.com/library/ms254963.aspx)
- Ressursihaldur
- Semantilise otsing
- Serveri identimisteavet. Kasuta andmebaasi rakendatud hoopis mandaat.
- Katkestab taseme üksuste: serveri rollid, IS_SRVROLEMEMBER, sys.login_token. Serveri tasemel õiguste pole saadaval, kuigi mõned on asendatud andmebaasi taseme õigused. Mõned serveritaseme DMVs pole saadaval, kuigi mõned on asendatud andmebaasi taseme DMVs.
- Serverless express: localdb kasutaja eksemplari
- Teenuse ta
- REMOTE_PROC_TRANSACTIONS MÄÄRAMINE
- SULGEMINE
- sp_addmessage
- sp_configure suvandid ja RECONFIGURE. Mõned suvandid on saadaval abil [Muuta andmebaasi VEEBIRAKENDUST KONFIGUREERIMINE](https://msdn.microsoft.com/library/mt629158.aspx).
- sp_helpuser
- sp_migrate_user_to_contained
- SQL serveri audit. Kasutage selle asemel auditeerimine SQL-andmebaasi.
- SQL serveri Profiler
- SQL serveri jälgimine
- Jälgida lipud. Mõned Jälita lipp üksused on teisaldatud ühilduvuse režiimi.
- Transact-SQL silumine
- Päästikute: Serveri ulatusega või sisselogimise päästikute
- Kasutage lause: muuta andmebaasi raames mõni muu andmebaas, tuleb teil teha uue andmebaasi uus ühendus.


## <a name="full-transact-sql-reference"></a>Täielik Transact-SQL-i viide

Transact-SQL-i grammatika, kasutus ja näiteid, kohta leiate lisateavet teemast [Transact-SQL-i viide (andmebaasimootor)](https://msdn.microsoft.com/library/bb510741.aspx) SQL serveri raamatuid online. 

### <a name="about-the-applies-to-tags"></a>"Kehtib" Sildid

Transact-SQL-i viide sisaldab SQL Server 2008 versioonide kohal seotud teemade. Teema pealkiri seal all on ikooni riba, kus neli SQL serveri platvormid ja kohaldamine, mis näitab. Näiteks võeti kättesaadavus rühmad SQL Server 2012. [Loo AVAILABILTY rühm](https://msdn.microsoft.com/library/ff878399.aspx) teema näitab lause kehtib ** SQL serveri (alustades 2012). Lause ei kehti SQL Server 2008, SQL Server 2008 R2, Azure SQL-andmebaas, SQL Azure'i andmebaas või paralleelselt andmebaas.

Mõnel juhul teema üldine teema saab kasutada toote, kuid on mõnevõrra toodete erinevusi. Erinevused on märgitud keskpunktid teemas vastavalt vajadusele.

