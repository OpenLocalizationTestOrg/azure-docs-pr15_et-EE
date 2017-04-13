<properties 
    pageTitle="Azure SQL serveri virtuaalse masina andmete uurimine | Microsoft Azure'i" 
    description="Kuidas uurida andmeid, mis on talletatud Azure SQL serveri VM." 
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
    ms.date="09/13/2016" 
    ms.author="bradsev" /> 

#<a name="explore-data-in-sql-server-virtual-machine-on-azure"></a>Azure SQL serveri virtuaalse masina andmete uurimine


Selles dokumendis antakse ülevaade andmete, mis on talletatud Azure SQL serveri VM uurimine. Seda saab teha andmete mitmetunnist SQL-i abil või programmeerimiskeel, nt Python abil.

**Menüü** viivat teemadele, kus kirjeldatakse, kuidas erinevate salvestusruumi keskkonnas: andmete uurimine tööriistade abil. See toiming on samm sisse Cortana Analytics protsess (ots).

[AZURE.INCLUDE [cap-explore-data-selector](../../includes/cap-explore-data-selector.md)]


> [AZURE.NOTE] Valimi SQL-lauseid selles dokumendis Oletame, et andmed on SQL Server. Kui see pole, vaadake cloud teadus protsessi andmekaardi teavet andmete teisaldamine SQL serveriga.



## <a name="sql-dataexploration"></a>SQL-i skripte SQL-i andmete uurimine

Siin on mõni SQL-i näidisskriptid kasutatavate SQL serveri andmetega poed uurida.

1. Saada päevas vaatluste arv

    `SELECT CONVERT(date, <date_columnname>) as date, count(*) as c from <tablename> group by CONVERT(date, <date_columnname>)` 

2. Tasemete kategooriline veerus hankimine

    `select  distinct <column_name> from <databasename>`

3. Saada tasemete arvu kahe kategooriline veeru kombinatsioon 

    `select <column_a>, <column_b>,count(*) from <tablename> group by <column_a>, <column_b>`

4. Jaotuse arvväärtuste veergude hankimine

    `select <column_name>, count(*) from <tablename> group by <column_name>`

> [AZURE.NOTE] Praktiline näiteks saate kasutada [NYC takso andmekomplekti](http://www.andresmh.com/nyctaxitrips/) ja IPNB, pealkirjaga [NYC andmete mitmetunnist IPython märkmiku ja SQL serveri kasutamise](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) jaoks mõne lõpuni walk-through viidata.

##<a name="python"></a>SQL-i andmete Python uurimine

Andmete uurimine ja funktsioonide loomiseks, kui andmed on SQL serveri Python kasutamine sarnaneb Azure'i bloobimälu Python, kasutamine andmete töötlemiseks nagu dokumenteerida [protsessi Azure'i bloobimälu andmete keskkonna tundmine teadus](machine-learning-data-science-process-data-blob.md). Andmed tuleb andmebaasist laaditud on pandas DataFrame ja seejärel saab edasi töödelda. Me dokumendi ühenduse andmebaasi ja andmete laadimine DataFrame selles jaotises.

SQL Serveri andmebaasiga ühenduse Python pyodbc (Asenda serverinimi, dbname, kasutajanimi ja parool oma kindlate väärtustega) abil saab ühenduse stringi järgmises vormingus:

    #Set up the SQL Azure connection
    import pyodbc   
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

[Pandas teegi](http://pandas.pydata.org/) Python annab andmete käsitlemise Python Programmeerimine suurel hulgal erinevaid andmestruktuurid ja andmeanalüüsi tööriistad. Tulemused tagastatakse SQL serveri andmebaasi Pandas andmete raami järgmine kood on järgmine:

    # Query database and load the returned results in pandas data frame
    data_frame = pd.read_sql('''select <columnname1>, <cloumnname2>... from <tablename>''', conn)

Nüüd saate töötada Pandas DataFrame vastavalt teema [protsessi Azure'i bloobimälu andmed teie andmete teadus keskkonnas](machine-learning-data-science-process-data-blob.md).

## <a name="cortana-analytics-process-in-action-example"></a>Cortana Analytics protsessi toimingu näites

Cortana Analytics protsessi kasutades avaliku andmekomplekti-lõpuni kiirtutvustus näide leiate [meeskonnatöö andmete teadus protsessi tegelikkuses: SQL serveri kasutamine](machine-learning-data-science-process-sql-walkthrough.md).

 
