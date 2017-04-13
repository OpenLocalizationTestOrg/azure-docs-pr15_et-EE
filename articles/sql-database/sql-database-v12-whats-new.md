<properties
    pageTitle="Mis on uut rakenduses SQL-i andmebaasi V12 | Microsoft Azure'i"
    description="Kirjeldatakse, miks äri süsteemid, mis kasutavad Azure'i SQL-andmebaasi pilveteenuses on kasulik Täiendamine versiooniks V12 kohe."
    services="sql-database"
    documentationCenter=""
    authors="MightyPen"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="genemi"/>


# <a name="whats-new-in-sql-database-v12"></a>Mis on uut rakenduses SQL-i andmebaasi V12


Selles teemas kirjeldatakse versiooni V11 Azure'i SQL-andmebaasi V12 uus versioon on mitmeid eeliseid.


Jätkame V12 funktsioonide lisamine. Seega soovitame tulemast teenusevärskendused Azure ja kasutada oma filtrid:


- [SQL-andmebaasi teenuse](https://azure.microsoft.com/updates/?service=sql-database)filtreeritud.
- SQL-andmebaasi funktsioone üldiselt kättesaadav [(GA) teadaannete](http://azure.microsoft.com/updates/?service=sql-database&update-type=general-availability) filtreeritud.


SQL-andmebaasi ressursi limiitide kohta värskeima teabe on dokumenteerida veebisaidil:<br/>[SQL Azure'i andmebaasi ressursi piirangud](sql-database-resource-limits.md).


## <a name="increased-application-compatibility-with-sql-server"></a>Suurema rakenduse ühilduvuse SQL Server


SQL-i andmebaasi V12 jaoks peamine eesmärk on Microsoft SQL Server 2014 ühilduvuse parandamiseks ja säilitamiseks ühilduvusest uue versioonide SQL serveri. Muude alade seas V12 saavutab tarkvarakomplektiga SQL serveri programmeeritavus ala tähtis. Näiteks:

- [JSON tugi](https://msdn.microsoft.com/library/dn921897.aspx)

- [Akna funktsioonid](http://msdn.microsoft.com/library/ms189798.aspx), [üle](http://msdn.microsoft.com/library/ms189461.aspx)

- [XML-i registrite](http://msdn.microsoft.com/library/bb934097.aspx) ja [valikulise XML-i registrite](http://msdn.microsoft.com/library/jj670104.aspx)

- [Muutuste jälitus](http://msdn.microsoft.com/library/bb933875.aspx)

- [VALIGE... RAKENDUSSE](http://msdn.microsoft.com/library/ms188029.aspx)

- [Otsingu](http://msdn.microsoft.com/library/ms142571.aspx)

- [Muuda andmebaasi JÄÄVATES KONFIGUREERIMINE (Transact-SQL)](http://msdn.microsoft.com/library/mt629158.aspx)

Leiate [siit](sql-database-transact-sql-information.md) väike hulk funktsioone, mis pole veel toetatud SQL-andmebaasis.


### <a name="compatibility-level-130"></a>Ühilduvuse taseme 130


> [AZURE.IMPORTANT] *Äsja* loodud andmebaaside Azure SQL-i andmebaasi V12 alates **juunist 2016**, on nende ühilduvuse taseme algavad 130, mis vastab Microsoft SQL Server 2016-ga
> 
> Saate kasutada `ALTER DATABASE YourDatabase SET COMPATIBILITY_LEVEL = 120` kui eelistate.
> 
> Enne juuni 2016 andmebaase pole nende ühilduvuse taseme selle muudatuse vaikimisi on muudetud. Samuti on muudetud täiendamine V11 V12 andmebaasi tase.



Selgitused, kuidas saate võrrelda kõige olulisemad päringute Viimane võrreldes varasemate ühilduvuse taseme vahel, leiate teemast:

- [Täiustatud päringu jõudluse ühilduvuse taseme 130 Azure'i SQL-andmebaas](sql-database-compatibility-level-query-performance-130.md)



## <a name="more-premium-performance-new-performance-levels"></a>Lisateavet premium jõudluse tagamiseks jõudluse uuele tasemele


V12, saame suurendada andmebaasi tehingu ühikute (DTUs) eraldatud kõiki Premium jõudluse tasemed täiendava tasuta 25%-ga. Veelgi jõudluse tõstmiseks saavutada uusi funktsioone, näiteks:


- Tugi-mälu [columnstore registrid](http://msdn.microsoft.com/library/gg492153.aspx).
- [Tabeli eraldamine ja rida](http://msdn.microsoft.com/library/ms187802.aspx) [KÄRPIGE](http://msdn.microsoft.com/library/ms177570.aspx)tabeliga seotud täiustused.
- Dünaamilise juhtimise kättesaadavus vaatamist [(DMVs)](http://msdn.microsoft.com/library/ms188754.aspx) abil saate jälgida ja jõudluse häälestamine.


### <a name="reliable-performance"></a>Usaldusväärne jõudlus


Kui teie klient programmi loob ühenduse SQL-i andmebaasi V12 ajal Azure virtuaalse masina (VM) oma kliendi töötab, peate avama järgmisi pordi vahemikke VM:

- 11000-11999
- 14000-14999


Klõpsake [siin](sql-database-develop-direct-route-ports-adonet-v12.md) V12 SQL-i andmebaasi Porte üksikasjad. Jõudlustäiustused SQL-i andmebaasi v12 vajavad pordid.


## <a name="better-support-for-cloud-saas-vendors"></a>Parem tugi cloud SaaS müüjad


Ainult V12, oleme välja jõudlust Standard uuele tasemele S3 ja [elastne andmebaasi kaustu](sql-database-elastic-pool.md)avaliku eelvaade. Elastne andmebaasi kaustu on lahenduse, mis on mõeldud pilveteenuste SaaS müüjad.  Elastne andmebaasi kaustu, saate teha järgmist.


- Ühiskasutus DTUs andmebaaside kulud andmebaaside suure hulga vähendamiseks.
- Käivitada [elastne andmebaasi töö](sql-database-elastic-jobs-overview.md) tasandil andmebaaside haldamine.


## <a name="security-enhancements"></a>Täiustatud turvalisus


Turvalisus on keegi, kes töötab oma ettevõtte pilveteenuses. Välja v12 uusima turbefunktsioonid on järgmised.


- [Rea taseme turvalisus](http://msdn.microsoft.com/library/dn765131.aspx) (RLS)
- [Dünaamiliste andmete peitmine](sql-database-dynamic-data-masking-get-started.md)
- [Suletud andmebaasid](http://msdn.microsoft.com/library/ff929188.aspx)
- [Rakenduse rollide](http://msdn.microsoft.com/library/ms190998.aspx) hallatud GRANT, Keela, tühistamine
- [Läbipaistva andmete krüptimine](http://msdn.microsoft.com/library/0bf7e8ff-1416-4923-9c4c-49341e208c62.aspx) (TDE)
- [Ühenduse loomine SQL-andmebaasi Azure Active Directory autentimist kasutades](sql-database-aad-authentication.md)
 - SQL-andmebaasi toetab nüüd Azure Active Directory autentimine, süsteemile ühenduse SQL-andmebaasiga, kasutades identiteedid Azure Active Directory (Azure AD). Azure Active Directory autentimise abil saate hallata ühes keskses kohas identiteedid andmebaasi kasutajate ja muude Microsofti teenuste ühes keskses kohas.
- [Alati krüptitud](https://msdn.microsoft.com/library/mt163865.aspx) (eelvaade) teeb krüptimise läbipaistvaid rakendustele ja võimaldab tundliku loomuga andmete klientrakendustes ilma krüptimise võtmed ühiskasutusse SQL-andmebaasi krüptimiseks kliendid.


## <a name="increased-business-continuity-when-recovery-is-needed"></a>Suurem kui taastamine on vajalik järjepidevuse tagamine


V12 pakub täiustatud taastamine punkti eesmärgid (RPOs) ja hinnangulise korda (ERTs).


| Funktsioon äri järjepidevus | Varasema versiooni | V12 |
| :-- | :-- | :-- |
| Geo-taastamine | • RPO < 24 tundi.<br/>• ERT < 12 tundi. | • RPO < 1 tund.<br/>• ERT < 12 tundi. |
| Aktiivse Geo-kopeerimine | • RPO < 5 minutit.<br/>• ERT < 1 tund. | • RPO < 5 minutit.<br/>• ERT < 30 sekundi järel. |


Lisateabe saamiseks vt [SQL-andmebaasi järjepidevuse](sql-database-business-continuity.md) .


## <a name="more-reasons-to-upgrade-now"></a>Veel põhjuseid uuendamine


On palju head põhjust, miks kliendid tuleks uuendada kohe Azure SQL-i andmebaasi v12 V11.


- SQL-i andmebaasi V12 on pikk loendi funktsioonid Lisaks V11 funktsioone.
- Lisada uusi funktsioone V12 jätkame, kuid V11 lisatakse uusi funktsioone.
- Enamik uusi funktsioone SQL-i andmebaasi V12 on välja enne, kui nad vabastatakse Microsoft SQL Server.


## <a name="are-you-using-v12-already"></a>Kas kasutate V12 juba?


Kas teil on andmebaasi või loogilise server töötab SQL-andmebaasi teenuse varasemas versioonis ühe lihtne viis on teha järgmist:


1. Avage [Azure'i portaalis](https://portal.azure.com/).
2. Klõpsake nuppu **Sirvi**.
3. Klõpsake nuppu **SQL-i serverid**.
4. Teie serveri või andmebaasi kõrval olevat ikooni räägib:
 - ![Ikoon v12 server](./media/sql-database-v12-whats-new/v12_icon.png) **V12 loogilise server**
 - ![Varasema versiooni server ikooni](./media/sql-database-v12-whats-new/earlier_icon.png) **varasema versiooni loogilise server**


Mõne muu tehnika veendumaks, et see versioon on käivitamiseks on `SELECT @@version;` lause oma andmebaasi ja vaadata sarnased tulemused:


- **12**.0.2000.10 &nbsp; *(V12 versioon)*
- **11**.0.9228.18 &nbsp; *(V11 versioon)*


Andmebaasi V12 võib olla majutatud ainult V12 loogilise server. Ja V12 serveri majutada ainult V12 andmebaasid.


Kui te pole veel töötab V12, täiendamist oma loogilise serveri [versioonile SQL-i andmebaasi V12 kohas](sql-database-v12-plan-prepare-upgrade.md)juhiseid järgides.


## <a name="V12AzureSqlDbPreviewGaTable"></a>Üldine kättesaadavus piirkondade


- 31 juuli 2015 kõigi piirkondade oli ülendatud abil General Availability (GA).
- V12 ilmus detsembrini 2014, kuid ainult eelvaade olekut.

[Microsoft Azure'i eelvaadete täiendavad kasutustingimusi](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
