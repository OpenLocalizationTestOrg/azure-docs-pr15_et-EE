<properties 
    pageTitle="SQL Azure'i andmete töötlemine | Microsoft Azure'i" 
    description="SQL Azure'i andmed protsess" 
    services="machine-learning" 
    documentationCenter="" 
    authors="garyericson" 
    manager="jhubbard" 
    editor="" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/16/2016" 
    ms.author="fashah;garye;bradsev" /> 

#<a name="heading"></a>SQL Server Azure'i virtuaalse masina andmete protsess

Selles dokumendis antakse ülevaade andmete uurimine ja funktsioone SQL serveri VM Azure talletatud andmete jaoks luua. Seda saab teha andmete mitmetunnist SQL-i abil või programmeerimiskeel, nt Python abil.


> [AZURE.NOTE] Valimi SQL-lauseid selles dokumendis Oletame, et andmed on SQL Server. Kui see pole, vaadake cloud teadus protsessi andmekaardi teavet andmete teisaldamine SQL serveriga.

##<a name="SQL"></a>SQL-i abil

Kirjeldame andmete wrangling järgmist selles jaotises SQL-i abil:

1. [Andmete uurimine](#sql-dataexploration)
2. [Funktsioon genereerimine](#sql-featuregen)

###<a name="sql-dataexploration"></a>Andmete uurimine
Siin on mõni SQL-i näidisskriptid kasutatavate SQL serveri andmetega poed uurida.


> [AZURE.NOTE] Praktiline näiteks saate kasutada [NYC takso andmekomplekti](http://www.andresmh.com/nyctaxitrips/) ja IPNB, pealkirjaga [NYC andmete mitmetunnist IPython märkmiku ja SQL serveri kasutamise](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) jaoks mõne lõpuni walk-through viidata.

1. Saada päevas vaatluste arv

    `SELECT CONVERT(date, <date_columnname>) as date, count(*) as c from <tablename> group by CONVERT(date, <date_columnname>)` 

2. Tasemete kategooriline veerus hankimine

    `select  distinct <column_name> from <databasename>`

3. Saada tasemete arvu kahe kategooriline veeru kombinatsioon 

    `select <column_a>, <column_b>,count(*) from <tablename> group by <column_a>, <column_b>`

4. Jaotuse arvväärtuste veergude hankimine

    `select <column_name>, count(*) from <tablename> group by <column_name>`


###<a name="sql-featuregen"></a>Funktsioon genereerimine

Selles jaotises on kirjeldatud viisil funktsioone SQL-i abil luua:  

1. [Count vastavalt funktsiooni genereerimine](#sql-countfeature)
2. [Funktsioon genereerimine binning](#sql-binningfeature)
3. [Jooksvalt läbi funktsioonide ühe veeru põhjal](#sql-featurerollout)


> [AZURE.NOTE] Kui teil luua uusi funktsioone, saate neid lisada olemasolevasse tabelisse veergude või lisavõimalusi ja primaarvõti, millega saate liitunud koos algse tabeli uue tabeli loomine. 

###<a name="sql-countfeature"></a>Count vastavalt funktsiooni genereerimine

Selle dokumendi näitab loomise loendamine funktsioonide kaks võimalust. Esimene meetod kasutab Tingimussumma ja teine meetod kasutab klausli "kui". Need saab siis liita koos algse tabeli (esmane võtme veergude abil) on loendamine funktsioonide kõrval algsed andmed.

    select <column_name1>,<column_name2>,<column_name3>, COUNT(*) as Count_Features from <tablename> group by <column_name1>,<column_name2>,<column_name3> 

    select <column_name1>,<column_name2> , sum(1) as Count_Features from <tablename> 
    where <column_name3> = '<some_value>' group by <column_name1>,<column_name2> 

###<a name="sql-binningfeature"></a>Funktsioon genereerimine binning

Järgmises näites näitab, kuidas luua binned funktsioonid binning (abil 5 alused) arvväärtuste veerg, mida saab kasutada funktsiooni hoopis:

    `SELECT <column_name>, NTILE(5) OVER (ORDER BY <column_name>) AS BinNumber from <tablename>`


###<a name="sql-featurerollout"></a>Jooksvalt läbi funktsioonide ühe veeru põhjal

Selles jaotises näitame välja ühe veeru tabeli lisafunktsioone loomiseks tehke järgmist. Näide eeldab, et tabel, kust soovite luua funktsioonid on veeru laius ja laiuskraadide.

Siin on lühike kaitsme laiuskraad/pikkuskraad asukohaandmete (varustatud vajalike ressurssidega kaudu stackoverflow [Kuidas mõõta täpsuse ja?](http://gis.stackexchange.com/questions/8650/how-to-measure-the-accuracy-of-latitude-and-longitude)). See on kasulik vaadata enne featurizing välja asukoht:

- Logige ütleb meile kas oleme Põhja või Lõuna, Ida või Lääne üle maailma.
- Nullist erinev sadu numbri ütleb meile kirjutit pikkuskraad, mitte laiuskraad!
- Kümneid numbri annab umbes 1000 km võimalik. See annab meile millist continent või ookeani oleme kasulikku teavet.
- Üksuste number (ühe komakoha kraadi) annab positsiooni kuni 111 km (60 meremiili, umbes 69 miili). See saab meile umbes mis suur maakond või riik oleme.
- Kümnendkohani tasub kuni 11.1 km: see võib eristada ühe suure linn naabruses suurte linnade asukoha.
- Koma tasub kuni 1.1 km: see üks küla eraldamiseks järgmisest argumendist.
- Kolmanda kümnendkohani pärast koma tasub 110 m: saate tuvastada, suure põllumajandusliku välja või institutsioonilise ülikooli.
- Neljanda kümnendkohani tasub kuni 11 m: saate tuvastada, paki maa. See on võrreldav tüüpiline täpsus korrigeerimata GPS üksuse ole.
- Viienda kümnendkohani pärast koma on kuni 1.1 m: väärt puude üksteisest eristada. See tase trükikojas GPS üksustega täpsuse võimalik üksnes siis vahe parandus.
- Kuuenda kümnendkohani pärast koma tasub kuni 0,11 m: abil saate seda küljendamise struktuurid üksikasjalikult kujundamiseks maastik, hoone teed. Peaks olema rohkem kui piisavalt hea liustikud ja jõed jälgimine. Seda saavutada taaskasutuse vaevarikas GPS, nt uimastialaste parandatud GPS.

Asukoha teave võib olla featurized järgmiselt eraldi välja piirkond, asukoha ja linn teavet. Pange tähele, et võite ka helistada ülejäänud lõpp-punkti nagu Bingi kaartide API saadaval [punkti asukoha leidmiseks](https://msdn.microsoft.com/library/ff701710.aspx) piirkond/piirkond teabe saamiseks.

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

Ülaltoodud asukoht põhineb funktsioone saab luua täiendavaid loendamine funktsioonide eespool kirjeldatud põhjalikumaks. 


> [AZURE.TIP] Saate lisada programmiliselt kirjed, kasutades teie valitud keeles. Võimalik, et peate lisada andmed tükkideks tõhustada kirjutamine (nt mõne teha, kasutades pyodbc leiate [A HelloWorld valimi SQLServer Python juurdepääsu](https://code.google.com/p/pypyodbc/wiki/A_HelloWorld_sample_to_access_mssql_with_python)). Teine võimalus on [BCP kasuliku](https://msdn.microsoft.com/library/ms162802.aspx)andmebaasi andmete lisamiseks.

###<a name="sql-aml"></a>Ühenduse loomine Azure seadme õpetused

Äsja loodud funktsiooni saate lisada olemasolevasse tabelisse veeru või talletatud uue tabeli ja liidetud algse tabeli masina õppimiseks. Funktsioonide loodud või juurde pääseda, kui juba loonud, [Andmete importimine] saab[ import-data] mooduli Azure seadme õppe nagu allpool näidatud:

![azureml lugejad][1] 

##<a name="python"></a>Nagu Python programmeerimiskeel abil

Andmete uurimine ja funktsioonide loomiseks, kui andmed on SQL serveri Python kasutamine sarnaneb Python kasutamise [protsess Azure'i bloobimälu andmete andmete teadus keskkonnas](machine-learning-data-science-process-data-blob.md)dokumenteerida Azure'i bloobimälu andmete töötlemiseks. Andmed tuleb andmebaasist laaditud pandas andmete raami ja seejärel saab edasi töödelda. Me dokumendi ühenduse andmebaasi ja andmete paneeli selles jaotises andmete laadimine.

SQL Serveri andmebaasiga ühenduse Python pyodbc (Asenda serverinimi, dbname, kasutajanimi ja parool oma kindlate väärtustega) abil saab ühenduse stringi järgmises vormingus:

    #Set up the SQL Azure connection
    import pyodbc   
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

[Pandas teegi](http://pandas.pydata.org/) Python annab andmete käsitlemise Python Programmeerimine suurel hulgal erinevaid andmestruktuurid ja andmeanalüüsi tööriistad. Tulemused tagastatakse SQL serveri andmebaasi Pandas andmete raami allpool kood on järgmine:

    # Query database and load the returned results in pandas data frame
    data_frame = pd.read_sql('''select <columnname1>, <cloumnname2>... from <tablename>''', conn)

Nüüd saate töötada Pandas andmete paneeli hõlmatud artikli [protsessi Azure'i bloobimälu andmete andmete teadus keskkonnas](machine-learning-data-science-process-data-blob.md).

## <a name="azure-data-science-in-action-example"></a>Azure'i andmed teadus toimingu näites

Azure'i andmed teadus protsessi kasutades avaliku andmekomplekti-lõpuni kiirtutvustus näide leiate [Azure'i andmed teadus protsessi toiming](machine-learning-data-science-process-sql-walkthrough.md).

[1]: ./media/machine-learning-data-science-process-sql-server-virtual-machine/reader_db_featurizedinput.png


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
 
