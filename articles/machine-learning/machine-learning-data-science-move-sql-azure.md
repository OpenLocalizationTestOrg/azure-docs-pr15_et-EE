<properties 
    pageTitle="Andmete teisaldamine Azure'i SQL-andmebaasi Azure seadme õppe | Azure'i" 
    description="SQL-i tabeli ja laadi andmed SQL tabeli loomine" 
    services="machine-learning" 
    documentationCenter="" 
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/14/2016"
    ms.author="bradsev" /> 

# <a name="move-data-to-an-azure-sql-database-for-azure-machine-learning"></a>Azure'i masina õppe Azure SQL-andmebaasi andmete teisaldamine

Selles teemas kirjeldatakse võimalusi andmete teisaldamine tasapinnalise failid (CSV- või TSV vormingud) või kohapealne SQL Azure'i SQL-andmebaasi serveri talletatud andmed. Järgmised toimingud andmete teisaldamiseks pilveteenuses on andmete meeskonna teadus käigus.

Teema, mis kirjeldab suvandid arvuti õ on kohapealne SQL serveri andmete teisaldamine, leiate teemast [andmete SQL Server Azure'i virtual arvutis](machine-learning-data-science-move-sql-server-virtual-machine.md).

**Menüü** viivat teemadele, kus kirjeldatakse, kuidas andmeid neelata suunata keskkonnas, kus saate andmeid talletada ja töödelda ajal meeskonnatöö andmete teadus protsess (TDSP).

[AZURE.INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]

Järgmises tabelis on andmete teisaldamine Azure'i SQL-andmebaasi suvandid.

<b>ALLIKAS</b> |<b>Sihtkoht: Azure'i SQL-andmebaas</b> |
-------------- |--------------------------------|
<b>Lamefailiga (CSV- või TSV vormindatud)</b> |<a href="#bulk-insert-sql-query">SQL-päringu lisa hulgi |
<b>Asutusesisese SQL serveriga</b> | 1. <a href="#export-flat-file">lamefaili eksportimine<br> 2 <a href="#insert-tables-bcp">viisardis SQL-i andmebaasi migreerimine<br> 3. <a href="#db-migration">andmebaasi tagasi üles ja taastamine<br> 4. <a href="#adf">Azure'i andmed Factory |


## <a name="prereqs"></a>Eeltingimused
Siin kirjeldatud toiminguid nõua, et teil on:

* On **Azure tellimust**. Kui olete tellinud, te saate registreeruda [tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/).
* **Azure storage konto**. Kasutage andmete talletamiseks selles õpetuses Azure storage konto. Kui teil pole kontot Azure storage, leiate artiklist [Loo konto salvestusruumi](storage-create-storage-account.md#create-a-storage-account) . Kui olete loonud salvestusruumi konto, peate hankima konto võti talletamist juurdepääsuks kasutada. [Vaade, kopeerimine ja uuesti luua salvestusruumi Pääsuklahvide](storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys)kuvamine
* Juurdepääs **Azure'i SQL-andmebaas**. Kui häälestate peab Azure'i SQL-andmebaasi, [Microsoft Azure'i SQL-andmebaasiga alustamine](../sql-database/sql-database-get-started.md) annab teavet ettevalmistamise Azure'i SQL-andmebaasi uue eksemplari.
* Installitud ja konfigureeritud **Azure PowerShelli** kohalikult. Juhiste saamiseks vaadake, [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md).

**Andmed**: migreerimise protsessid on näidata [NYC takso andmekomplekti](http://chriswhong.com/open-data/foil_nyc_taxi/). Andmekomplekti NYC takso sisaldab reisi andmed ja messidel ja on saadaval, kui märkida, et postitus Azure'i bloobimälu: [NYC takso andmed](http://www.andresmh.com/nyctaxitrips/). Valimi ja kirjeldust need failid on toodud [NYC takso reisi andmekomplekti kirjeldus](machine-learning-data-science-process-sql-walkthrough.md#dataset).
 
Saate kohandada oma andmete siin kirjeldatud toiminguid või NYC takso andmekomplekti abil kirjeldatud juhised. Üleslaadimiseks NYC takso andmekomplekti kohapealne SQL Serveri andmebaasiga, toimige kirjeldatud [Hulgi andmete importimine SQL serveri andmebaasi](machine-learning-data-science-process-sql-walkthrough.md#dbload). Need juhised on mõeldud SQL serveri kohta on Azure virtuaalse masina, kuid üleslaadimise asutusesisese SQL serveriga on samad.


## <a name="file-to-azure-sql-database"></a>Lamefaili allika andmete teisaldamine Azure'i SQL-andmebaasiga

Andmete tasapinnalise failid (CSV- või TSV vormindatud) saab teisaldada päringuga hulgi lisada SQL Azure'i SQL-andmebaasi.

### <a name="bulk-insert-sql-query"></a>SQL-päringu lisa hulgi

Hulgi lisada SQL-päringu abil toimingu juhised on sarnased hõlma liikumiseks andmete lamefaili allika SQL serveriga on Azure VM. Lisateavet leiate teemast [Hulgi lisada SQL-päringu](machine-learning-data-science-move-sql-server-virtual-machine.md#insert-tables-bulkquery).


##<a name="sql-on-prem-to-sazure-sql-database"></a>Kohapealne SQL serveri andmete teisaldamine Azure'i SQL-andmebaasiga

Kui need andmed on salvestatud kohapealne SQL serveri, on mitu võimalust andmete teisaldamiseks Azure'i SQL-andmebaasiga:

1. [Eksportimine Lamefailiga](#export-flat-file) 
2. [SQL-i andmebaasi migreerimine viisard](#insert-tables-bcp)
3. [Andmebaasi tagasi üles ja taastamine](#db-migration)
4. [Azure'i andmed Factory](#adf)

Esimesed kolm juhised on väga sarnased nende [andmete teisaldamine SQL Server Azure'i virtual arvutis](machine-learning-data-science-move-sql-server-virtual-machine.md) jaotised, mis hõlmavad sama menetluse. Selle teema vastav jaotiste lingid on toodud järgmised juhised.

###<a name="export-flat-file"></a>Eksportimine Lamefailiga

Juhised selle eksportimise lamefailiga sarnanevad kaetud [lamefaili eksportida](machine-learning-data-science-move-sql-server-virtual-machine.md#export-flat-file).

###<a name="insert-tables-bcp"></a>SQL-i andmebaasi migreerimine viisard

Juhiseid SQL-i andmebaasi migreerimine viisardi abil sarnanevad hõlmatud [SQL-i andmebaasi migreerimine](machine-learning-data-science-move-sql-server-virtual-machine.md#sql-migration)viisardi juhiseid.

###<a name="db-migration"></a>Andmebaasi tagasi üles ja taastamine

Juhised abil andmebaasi varundamine ja taastamine sarnanevad nimetatud [andmebaasi varundamine ja taastamine](machine-learning-data-science-move-sql-server-virtual-machine.md#sql-backup).

###<a name="adf"></a>Azure'i andmed Factory

Azure'i SQL-andmebaasi koos Azure'i andmed Factory Automaatsöötja andmete kord on esitatud teemas [kohapealne SQL serveri SQL Azure'i Azure andmete Factory koos andmete teisaldamine](machine-learning-data-science-move-sql-azure-adf.md). Selles teemas kirjeldatakse andmete teisaldamine kohapealne SQL serveri andmebaasi Azure SQL-andmebaasi kaudu Azure'i bloobimälu ADF abil. 

Kaaluge ADF andmeid on vaja migreerida pidevalt hübriidjuurutuse stsenaariumi, mis kasutab kohapealne-ja pilveteenuse ja kui andmed on tehingu või muuta või lisada, kui seda ei viida äriloogika on vaja. ADF võimaldab soovitud plaanimine ja jälgimine abil lihtsa JSON skripte, et hallata andmeid perioodiliselt liikumine. ADF on ka muid võimalusi, nagu keerukate toimingute tugi.




