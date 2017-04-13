<properties 
    pageTitle="Näidisandmed Azure SQL serveri | Microsoft Azure'i" 
    description="Näidisandmete Azure SQL serveris" 
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
    ms.date="09/19/2016" 
    ms.author="fashah;garye;bradsev" /> 

#<a name="heading"></a>Näidisandmete Azure SQL serveris


Selle dokumendi näitab, kuidas näidisandmed Azure SQL serveris talletatavate SQL-i või Python programmeerimiskeel. Samuti kujutatakse valimisse andmete viimine Azure seadme õ, salvestades selle faili, üleslaadimine on Azure'i bloobimälu ja seejärel lugemine see Azure seadme õ Studio.

Python valimite kasutab [pyodbc](https://code.google.com/p/pyodbc/) ODBC teegi ühenduse SQL Server Azure'i ja [Pandas](http://pandas.pydata.org/) teegi proovide teha.

>[AZURE.NOTE] Valimi SQL-koodi selles dokumendis eeldab, et andmed on SQL Server Azure'i. Kui see pole, vaadake leiate Azure'i SQL serveri andmete teisaldamiseks teemas [teisaldamine Azure SQL serveri andmetega](machine-learning-data-science-move-sql-server-virtual-machine.md) .

**Miks teie näidisandmed?**
Kui kavatsete analüüsida andmekomplekti on suur, on tavaliselt on hea mõte kasutada alla valimiga väiksem, kuid esindaja ja paremini hallataval mahu vähendamiseks andmed. See hõlbustab andmete mõistmine, avastamine ja funktsioon matemaatika erifunktsioonid. Selle rolli [Meeskonnatöö andmete teadus protsess (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) on kiire prototüüpimine töötlemise funktsioonidest ja seadme õppe mudelid lubamine.

**Menüü** allpool näidisandmed erinevate salvestusruumi keskkonnas, kuidas kirjeldavate teemade lingid. 

[AZURE.INCLUDE [cap-sample-data-selector](../../includes/cap-sample-data-selector.md)]

Selle toimingu valimite on samm [Meeskonnatöö andmete teadus protsess (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

##<a name="SQL"></a>SQL-i abil

Selles jaotises kirjeldatakse SQL-i abil saate teha lihtsa juhusliku valimi suhtes andmete andmebaasis mitu võimalust. Valige oma andmete mahtu ja selle jaotuse meetodit.

Alltoodud kahest näitab, kuidas kasutada SQL serveri newid proovide. Teie valitud meetod sõltub sellest, kuidas juhusliku soovite olema näidis (proovi kood allpool pk_id on eeldatakse, et automaatselt loodud esmane võti).

1. Vähem range juhuslik näidis

        select  * from <table_name> where <primary_key> in 
        (select top 10 percent <primary_key> from <table_name> order by newid())

2. Lisateavet juhuslik näidis 

        SELECT * FROM <table_name>
        WHERE 0.1 >= CAST(CHECKSUM(NEWID(), <primary_key>) & 0x7fffffff AS float)/ CAST (0x7fffffff AS int)

Tablesample saate olla valimite kasutatakse samuti allpool näidatud. See võib olla parem lähenemine, kui teie andmed on suur (eeldades, et sellel puudus erinevatel lehtedel olevate andmete) ja päringu lõpuleviimiseks mõistliku aja jooksul.

    SELECT *
    FROM <table_name> 
    TABLESAMPLE (10 PERCENT)

>[AZURE.NOTE] Saate uurida ja luua uue tabeli panete valimisse andmed funktsioonid


###<a name="sql-aml"></a>Ühenduse loomine Azure seadme õpetused

Saate otse valimi päringute kohal Azure seadme õ [Andmete importimine] [ import-data] mooduli alla valimiga pealt andmed ja Azure seadme õ katse viimiseks. Kuvatõmmis abil lugemise mooduli valimisse andmeid lugeda on allpool näidatud:
   
![lugeja SQL-i][1]

##<a name="python"></a>Pythoni abil 

Selles jaotises näitab, kasutades [pyodbc teek](https://code.google.com/p/pyodbc/) on ODBC Python SQL Serveri andmebaasiga ühenduse loomiseks. Andmebaasi ühendusstringi on järgmine: (asendage konfiguratsioonist serverinimi, dbname, kasutajanimi ja parool):

    #Set up the SQL Azure connection
    import pyodbc   
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

[Pandas](http://pandas.pydata.org/) teegi Python annab andmete käsitlemise Python Programmeerimine suurel hulgal erinevaid andmestruktuurid ja andmeanalüüsi tööriistad. Alljärgnev kood loeb 0,1% valimi andmete SQL Azure'i andmebaasi tabelisse Pandas andmeid:

    import pandas as pd

    # Query database and load the returned results in pandas data frame
    data_frame = pd.read_sql('''select column1, cloumn2... from <table_name> tablesample (0.1 percent)''', conn)

Nüüd saate töötada Pandas andmete paneeli valimisse andmed. 

###<a name="python-aml"></a>Ühenduse loomine Azure seadme õpetused

Saate faili alla valimisse andmed salvestada ja üleslaadimine on Azure'i bloobimälu järgmine proovi kood. Andmed on bloobimälu otse lugemiseks saab sisse Azure seadme õ katse kasutada [Andmete importimine] [ import-data] mooduli. Juhised on järgmised: 

1. Kirjutage pandas andmete paneeli kohalik fail.

        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)

2. Azure'i bloobimälu kohaliku faili üleslaadimine

        from azure.storage import BlobService
        import tables

        STORAGEACCOUNTNAME= <storage_account_name>
        LOCALFILENAME= <local_file_name>
        STORAGEACCOUNTKEY= <storage_account_key>
        CONTAINERNAME= <container_name>
        BLOBNAME= <blob_name>

        output_blob_service=BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)    
        localfileprocessed = os.path.join(os.getcwd(),LOCALFILENAME) #assuming file is in current working directory
        
        try:
       
        #perform upload
        output_blob_service.put_block_blob_from_path(CONTAINERNAME,BLOBNAME,localfileprocessed)
        
        except:         
            print ("Something went wrong with uploading blob:"+BLOBNAME)

3. Azure'i bloobimälu Azure seadme õ [Andmete importimine] kasutamine andmete lugemiseks[ import-data] moodul, nagu on näidatud ekraani rüütama alla:
 
![lugeja bloobimälu][2]

## <a name="the-team-data-science-process-in-action-example"></a>Meeskonnatöö andmete teadus protsessi toimingu näide

Näide-lõpuni kiirtutvustus andmete meeskonna teadus protsessi kasutamine avaliku andmekomplekti, leiate [meeskonnatöö andmete teadus protsessi tegelikkuses: abil SQL katkestab](machine-learning-data-science-process-sql-walkthrough.md).

[1]: ./media/machine-learning-data-science-sample-sql-server-virtual-machine/reader_database.png
[2]: ./media/machine-learning-data-science-sample-sql-server-virtual-machine/reader_blob.png

 [import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
