<properties 
    pageTitle="Lisaks 1433 SQL-i andmebaasi Porte | Microsoft Azure'i"
    description="Kliendi ühenduste ADO.net-i Azure SQL-i andmebaasi v12 mõnikord puhverserverist mööduda ja andmebaasi suhelda. Pordid 1433 peale muutuvad oluline."
    services="sql-database"
    documentationCenter=""
    authors="MightyPen"
    manager="jhubbard"
    editor="" />


<tags 
    ms.service="sql-database" 
    ms.workload="drivers"
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/17/2016"
    ms.author="annemill"/>


# <a name="ports-beyond-1433-for-adonet-45-and-sql-database-v12"></a>Lisaks 1433 ADO.NET 4.5 ja SQL-i andmebaasi V12 pordid


Selles teemas kirjeldatakse muudatusi, mis toob Azure SQL-i andmebaasi V12 ühenduse toimimist klientides ADO.net-i 4.5 või uuem versioon.


## <a name="v11-of-sql-database-port-1433"></a>SQL-i andmebaasi v11: Port 1433


Kui teie klient programm kasutab ADO.net-i 4.5 ühenduse loomiseks ja päringu SQL-i andmebaasi V11 koos, sisemine järjestus on järgmine:


1. ADO.net-i püüab SQL-andmebaasiga ühenduse loomiseks.

2. ADO.net-i kasutab port 1433 helistada vahevara mooduli ja selle vahevara loob ühenduse SQL-andmebaasi.

3. SQL-andmebaasi saadab oma vastuse vahevara, ADO.net-i vastuse pordi 1433 andev.


**Terminoloogia:** Kirjeldame öeldes ADO.NET suhtleb SQL-andmebaasiga, kasutades *puhverserveri marsruutimine*eelnev jada. Kui pole vahevara on kaasatud, me öelda *otsese marsruutimiseks* kasutati.


## <a name="v12-of-sql-database-outside-vs-inside"></a>SQL-i andmebaasi v12: väljaspool vs sees


Ühenduste V12, peab palume, kas teie klientprogrammi töötab *väljas* või *sees* Azure pilveteenuse äärist. Lõigetes käsitletakse kahte levinud stsenaariumi.


#### <a name="outside-client-runs-on-your-desktop-computer"></a>*Väljaspool:* Klient töötab arvuti töölauale


Port 1433 on ainult port, mis tuleb avada oma lauaarvutis, mis hostib klientrakenduse SQL-andmebaasi.


#### <a name="inside-client-runs-on-azure"></a>*Sees:* Klient töötab Azure


Kui teie klient töötab Azure pilveteenuse Rühmaboks sisse, kasutab me helistamiseks *marsruutimiseks otse* suhelda SQL-andmebaasi server. Pärast seda, kui ühendus on loodud, edasiseks kliendi ja andmebaasi vaheline kaasamiseks pole vahevara puhverserveri.


Jadas on järgmine:


1. ADO.net-i 4,5 (või uuem versioon) alustab lühike suhtluse Azure pilvega ja saab dünaamiliselt tuvastatud pordinumber.
 - Dünaamiliselt tuvastatud pordi number on vahemikus 11000 – 11999 või 14000-14999.

2. ADO.net-i seejärel loob ühenduse SQL-andmebaasi server otse pole vahevara vahel.

3. Päringute on saadetud otse andmebaasi ja tulemused tagastatakse otse kliendile.


Veenduge, et pordi vahemike 11000-11999 ja 14000-14999 Azure kliendi arvutisse on jäänud ADO.net-i 4.5 kliendi suhtlust SQL-i andmebaasi V12 jaoks saadaval.

- Eelkõige Porte vahemikus peab olema tasuta väljaminev blokaatorid.

- Klõpsake oma Azure VM, **Täiustatud turvalisusega Windowsi tulemüür** juhtelemendid pordi sätteid.
 - [Tulemüüri kasutajaliidese](http://msdn.microsoft.com/library/cc646023.aspx) abil saate lisada reegli, mille **TCP** -protokolli koos port vahemikus umbes **11000-11999**süntaks.


## <a name="version-clarifications"></a>Versiooni selgitused


Selles jaotises selgitatakse hüüdnimede, mis viitavad versioonid. Samuti on loetletud mõned sidumiste versioonide toodete vahel.


#### <a name="adonet"></a>ADO.NET-I


- ADO.NET 4.0 toetab protokolli TDS 7.3, kuid mitte 7,4.
- ADO.net-i 4.5 ja uuemad versioonid toetab protokolli TDS 7,4.


#### <a name="sql-database-v11-and-v12"></a>SQL-i andmebaasi V11 ja V12


Selles teemas on esile tõstetud kliendi ühenduse SQL-i andmebaasi V11 ja V12 vahelised erinevused.


*Märkus:* Transact-SQL-lause `SELECT @@version;` annab vastuseks väärtuse, mis algavad numbriga, nt "11." või "12." ja need vastavad meie V11 ja V12 versiooni nimed SQL-andmebaasi.


## <a name="related-links"></a>Seotud lingid


- ADO.NET 4.6 ilmus 20 juuli 2015. Ajaveebi teadaanne .net-i meeskond on saadaval [siin](http://blogs.msdn.com/b/dotnet/archive/2015/07/20/announcing-net-framework-4-6.aspx).


- ADO.net-i 4.5 ilmus 15 augustis 2012. Ajaveebi teadaanne .net-i meeskond on saadaval [siin](http://blogs.msdn.com/b/dotnet/archive/2012/08/15/announcing-the-release-of-net-framework-4-5-rtm-product-and-source-code.aspx).
 - Ajaveebipostituse ADO.net-i 4.5.1 kohta on saadaval [siin](http://blogs.msdn.com/b/dotnet/archive/2013/06/26/announcing-the-net-framework-4-5-1-preview.aspx).


- [TDS protokolli versiooni loend](http://www.freetds.org/userguide/tdshistory.htm)


- [SQL-i andmebaasi arengu ülevaade](sql-database-develop-overview.md)


- [Azure'i SQL-andmebaasi tulemüüri](sql-database-firewall-configure.md)


- [Kuidas: SQL-andmebaasi tulemüüri sätete konfigureerimine](sql-database-configure-firewall-settings.md)

