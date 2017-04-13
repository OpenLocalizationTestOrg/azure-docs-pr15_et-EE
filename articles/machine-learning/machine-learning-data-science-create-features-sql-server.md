<properties
    pageTitle="Funktsioonid andmete loomine SQL serveri SQL-i ja Python abil | Microsoft Azure'i"
    description="SQL Azure'i andmed protsess"
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="bradsev;fashah;garye" />


# <a name="create-features-for-data-in-sql-server-using-sql-and-python"></a>Funktsioonid andmete loomine SQL serveri SQL-i ja Python abil


Selle dokumendi näitab, kuidas luua funktsioone SQL serveri VM Azure talletatavad andmed, mis aitavad algoritmide kohta lugege andmete tõhusamaks. Seda saab teha SQL-i abil või kasutades programmeerimiskeel, nt Python, mis on näidanud siin.

[AZURE.INCLUDE [cap-create-features-data-selector](../../includes/cap-create-features-selector.md)]See **menüü** erinevaid funktsioone andmete loomist kirjeldavate teemade lingid. See toiming on juhises [Meeskonnatöö andmete teadus protsess (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

> [AZURE.NOTE] Praktiline näiteks saate tutvuda [NYC takso andmekomplekti](http://www.andresmh.com/nyctaxitrips/) ja viidata IPNB, pealkirjaga [NYC andmete mitmetunnist IPython märkmiku ja SQL serveri kasutamise](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) jaoks mõne lõpuni walk-through.


## <a name="prerequisites"></a>Eeltingimused
Selles artiklis eeldatakse, et teil on:

* Loodud Azure storage konto. Kui vajate juhiseid, lugege teemat [Azure Storage konto loomine](../storage/storage-create-storage-account.md#create-a-storage-account)
* SQL serveris talletatavate andmete. Kui te ei ole, leiate juhised selle kohta, kuidas andmeid sinna [Azure seadme õppe Azure SQL-andmebaasi andmete teisaldamine](machine-learning-data-science-move-sql-azure.md) .


## <a name="sql-featuregen"></a>Funktsioon genereerimine SQL-i abil

Selles jaotises on kirjeldatud viisil funktsioone SQL-i abil luua:  

1. [Count vastavalt funktsiooni genereerimine](#sql-countfeature)
2. [Funktsioon genereerimine binning](#sql-binningfeature)
3. [Jooksvalt läbi funktsioonide ühe veeru põhjal](#sql-featurerollout)


> [AZURE.NOTE] Kui teil luua uusi funktsioone, saate neid lisada olemasolevasse tabelisse veergude või lisavõimalusi ja primaarvõti, millega saate liitunud koos algse tabeli uue tabeli loomine.

### <a name="sql-countfeature"></a>Count vastavalt funktsiooni genereerimine

Selle dokumendi näitab loomise loendamine funktsioonide kaks võimalust. Esimene meetod kasutab Tingimussumma ja teine meetod kasutab klausli "kui". Need saab siis liita koos algse tabeli (esmane võtme veergude abil) on loendamine funktsioonide kõrval algsed andmed.

    select <column_name1>,<column_name2>,<column_name3>, COUNT(*) as Count_Features from <tablename> group by <column_name1>,<column_name2>,<column_name3>

    select <column_name1>,<column_name2> , sum(1) as Count_Features from <tablename>
    where <column_name3> = '<some_value>' group by <column_name1>,<column_name2>

### <a name="sql-binningfeature"></a>Funktsioon genereerimine binning

Järgmises näites näitab, kuidas luua binned funktsioonid binning (abil 5 alused) arvväärtuste veerg, mida saab kasutada funktsiooni hoopis:

    `SELECT <column_name>, NTILE(5) OVER (ORDER BY <column_name>) AS BinNumber from <tablename>`


### <a name="sql-featurerollout"></a>Jooksvalt läbi funktsioonide ühe veeru põhjal

Selles jaotises näitame välja ühe veeru tabeli lisafunktsioone loomiseks tehke järgmist. Näide eeldab, et tabel, kust soovite luua funktsioonid on veeru laius ja laiuskraadide.

Siin on lühike kaitsme laiuskraad/pikkuskraad asukohaandmete (varustatud vajalike ressurssidega kaudu stackoverflow `http://gis.stackexchange.com/questions/8650/how-to-measure-the-accuracy-of-latitude-and-longitude`). See on kasulik vaadata enne featurizing välja asukoht:

- Logige ütleb meile kas oleme Põhja või Lõuna, Ida või Lääne üle maailma.
- Nullist erinev sadu numbri ütleb meile kirjutit pikkuskraad, mitte laiuskraad!
- Kümneid numbri annab umbes 1000 km võimalik. See annab meile millist continent või ookeani oleme kasulikku teavet.
- Üksuste number (ühe komakoha kraadi) annab positsiooni kuni 111 km (60 meremiili, umbes 69 miili). See saab meile umbes mis suur maakond või riik oleme.
- Kümnendkohani tasub kuni 11.1 km: see võib eristada ühe suure linn naabruses suurte linnade asukoha.
- Koma tasub kuni 1.1 km: see üks küla eraldamiseks järgmisest argumendist.
- Kolmanda kümnendkohani pärast koma tasub 110 m: saate tuvastada, suure põllumajandusliku välja või institutsioonilise ülikooli.
- Neljanda kümnendkohani tasub kuni 11 m: saate tuvastada, paki maa. See on võrreldav tüüpiline täpsuse korrigeerimata GPS üksuse ole.
- Viienda kümnendkohani pärast koma on kuni 1.1 m: väärt puude üksteisest eristada. See tase trükikojas GPS üksustega täpsuse võimalik üksnes siis vahe parandus.
- Kuuenda kümnendkohani pärast koma tasub kuni 0,11 m: abil saate seda küljendamise struktuurid üksikasjalikult kujundamiseks maastik, hoone teed. Peaks olema rohkem kui piisavalt hea liustikud ja jõed jälgimine. Seda saavutada taaskasutuse vaevarikas GPS, nt uimastialaste parandatud GPS.

Asukohateabe saate võib olla featurized järgmiselt eraldi välja piirkond, asukoha ja linn teavet. Pange tähele, et kui ka kõne ülejäänud lõpp-punkti nagu Bingi kaartide API saadaval `https://msdn.microsoft.com/library/ff701710.aspx` piirkond/piirkond teabe saamiseks.

    select
        <location_columnname>
        ,round(<location_columnname>,0) as l1       
        ,l2=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 1 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),1,1) else '0' end     
        ,l3=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 2 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),2,1) else '0' end     
        ,l4=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 3 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),3,1) else '0' end     
        ,l5=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 4 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),4,1) else '0' end     
        ,l6=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 5 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),5,1) else '0' end     
        ,l7=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 6 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),6,1) else '0' end     
    from <tablename>

Ülaltoodud asukoha funktsioone saab luua täiendavaid loendamine funktsioonide eespool kirjeldatud põhjalikumaks.


> [AZURE.TIP] Saate lisada programmiliselt kirjed, kasutades teie valitud keeles. Võimalik, et peate lisada andmed tükkideks kirjutamine tõhustada [väljamöllimise näide sellest, kuidas seda teha, kasutades pyodbc siin](https://code.google.com/p/pypyodbc/wiki/A_HelloWorld_sample_to_access_mssql_with_python).
Teine võimalus on kasutades [BCP kasuliku](https://msdn.microsoft.com/library/ms162802.aspx) andmebaasi andmete lisamiseks

### <a name="sql-aml"></a>Ühenduse loomine Azure seadme õpetused

Äsja loodud funktsiooni saate lisada olemasolevasse tabelisse veeru või talletatud uue tabeli ja liidetud algse tabeli masina õppimiseks. Funktsioone saab loodud või juurde siis, kui juba loonud, [Andmete importimine](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) mooduli kasutamise Azure'i ml, nagu allpool näidatud:

![azureml lugejad](./media/machine-learning-data-science-process-sql-server-virtual-machine/reader_db_featurizedinput.png)

## <a name="python"></a>Nagu Python programmeerimiskeel abil

Kasutades Python funktsioonid luua, kui andmed on SQL Server on sarnane Azure'i bloobimälu kasutamise [protsess Azure'i bloobimälu andmete andmete teadus keskkonnas](machine-learning-data-science-process-data-blob.md)dokumenteerida Python andmete töötlemiseks. Andmed tuleb andmebaasist laaditud pandas andmete raami ja seejärel saab edasi töödelda. Me dokumendi ühenduse andmebaasi ja andmete paneeli selles jaotises andmete laadimine.

SQL Serveri andmebaasiga ühenduse Python pyodbc (Asenda serverinimi, dbname, kasutajanimi ja parool oma kindlate väärtustega) abil saab ühenduse stringi järgmises vormingus:

    #Set up the SQL Azure connection
    import pyodbc
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

[Pandas teegi](http://pandas.pydata.org/) Python annab andmete käsitlemise Python Programmeerimine suurel hulgal erinevaid andmestruktuurid ja andmeanalüüsi tööriistad. Tulemused tagastatakse SQL serveri andmebaasi Pandas andmete raami allpool kood on järgmine:

    # Query database and load the returned results in pandas data frame
    data_frame = pd.read_sql('''select <columnname1>, <cloumnname2>... from <tablename>''', conn)

Nüüd saate töötada Pandas andmete raami vastavalt teemade [loomine Azure'i bloobimälu andmed, mille abil Panda funktsioone](machine-learning-data-science-create-features-blob.md).
