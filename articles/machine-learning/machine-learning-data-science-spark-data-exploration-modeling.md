<properties
    pageTitle="Andmete uurimine ja modelleerimine säde | Microsoft Azure'i"
    description="Andmete uurimine ja modelleerimine võimaluste säde MLlib tööriistakomplekt tutvustatakse."
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
    ms.date="10/07/2016"
    ms.author="deguhath;bradsev;gokuma" />

# <a name="data-exploration-and-modeling-with-spark"></a>Andmete uurimine ja modelleerimine säde

[AZURE.INCLUDE [machine-learning-spark-modeling](../../includes/machine-learning-spark-modeling.md)]

Juhendis kasutab Hdinsightiga säde teha andmete uurimine ja kahendarvu liigitamine ja regressiooni modelleerimine tööülesannete NYC valimil takso reisi ja 2013 andmekomplekti hinda.  See juhendab teid juhiseid [Andmete teadus protsessi](http://aka.ms/datascienceprocess)end end, kasutades mõnda Hdinsightiga säde klaster, töötlemiseks ja Azure plekid andmed ja mudelid talletamiseks. Protsessi uurib tänav visualiseeritakse kaudu on salvestusruumi Azure'i bloobimälu andmete ja seejärel teeb andmed prognoosmudelite koostamiseks. Need mudelid on koostamine abil säde MLlib tööriistakomplekt kahendarvu liigitamine ja regressiooni modelleerimine tööülesandeid teha.

- **Binaarsed liigitamine** tööülesanne on prognoosida, kas Näpunäide makstakse reisil. 
- **Regressioonisirge** tööülesanne on prognoosida põhjal muude funktsioonide näpunäite näpunäidet summa. 

Kasutame mudelid järgmised logistika ja lineaarse regressioonisirge, juhusliku metsade ja astmiku võimendatud puude.

- [Lineaarse regressioonisirge koos SGD](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.regression.LinearRegressionWithSGD) on lineaarse regressioonisirge mudelit, mis kasutab Stohhastiline astmiku päritolu (SGD) ja optimeerimine ja funktsioon skaleerimist prognoosida näpunäite summad makstud. 
- [Logistilise regressiooni koos LBFGS](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.classification.LogisticRegressionWithLBFGS) või "logit" regressiooni, on regressiooni mudelit, mida saab teha andmete liigitamine kategooriline sõltuv muutuja korral. LBFGS on arvestusliku Newton optimeerimine algoritmi, mis ühtlustab Broyden – Fletcher – Goldfarb – Shanno (BFGS) algoritmi piiratud hulgal arvuti mälu abil ja mis on levinud masina õ.
- [Juhuslik metsad](http://spark.apache.org/docs/latest/mllib-ensembles.html#Random-Forests) on otsust puude komplektid.  Need ühendada palju otsust puude overfitting vähendada. Juhusliku metsade kasutatakse regressiooni ja liigitamine võite hakkama kategooriline funktsioonid ja multiclass liigitamine, millega saate pikendada. Ei ole vaja funktsiooni skaleerimist ja suudavad jäädvustada mittelineaarsust ja funktsioonide populaarsed. Juhusliku metsad on üks kõige edukamad masina õppe liigitamiseks ja regressiooni mudelid.
- [Astmiku võimendada puude](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) on otsust puude komplektid. GBTs koolitada otsust puude korduvalt kaotsimineku funktsiooni minimeerimiseks. GBTs kasutatakse regressiooni ja liigitamine võite hakkama kategooriline funktsioone, pole vaja funktsiooni skaleerimist ja saavad jäädvustada mittelineaarsust ja funktsioonide kasutusviisid. Neid saab kasutada multiclass-liigitamine säte.

Modelleerimine juhiseid sisaldada ka kood, mis näitab, kuidas koolitada, hinnata ja salvestada erinevat tüüpi mudel. Python on kasutatud kood soovitud lahenduse ja oluline proovitükid kuvamiseks.   


>[AZURE.NOTE] Kuigi säde MLlib tööriistakomplekt on loodud töötama suurte andmekogumite, kasutatakse suhteliselt näidis (abil 170K read, umbes 0,1% algse NYC andmekomplekti ~ 30 Mb) siin mugavuse. Siin antud kasutamise töötab tõhus (umbes 10 minutit) on Hdinsightiga kobar 2 töötaja sõlmed abil. Sama koodi, minimaalsed muudatused, mille saab töödelda suuremat-andmekogumi, mälu andmete vahemällu talletamine ja kobar suuruse muutmise korral muudatustega.

## <a name="prerequisites"></a>Eeltingimused

Peate Azure'i konto ja mõne Hdinsightiga säde te vajate Hdinsightiga 3.4 säde 1,6 kobar juhendis lõpuleviimiseks. [Ülevaade andmete teadus kasutamise kohta Windows Azure Hdinsightiga säde](machine-learning-data-science-spark-overview.md) juhiste saamiseks vaadake Kuidas nendele nõuetele. Selle teema sisaldab ka siin kasutatud NYC 2013 takso andmed ja juhised, kuidas käivitada koodi Jupyter märkmiku säde klaster kirjeldus. **PySpark-machine-learning-data-science-spark-data-exploration-modeling.ipynb** märkmikku, mis sisaldab koodinäiteid selles teemas on saadaval [Github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/pySpark). 


[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


## <a name="setup-storage-locations-libraries-and-the-preset-spark-context"></a>Install: salvestusruumi asukohad, teekide ja eelseadistatud säde kontekst

Säde on võimalik lugeda ja kirjutada Azure'i salvestusruumi bloobimälu (nimetatakse ka WASB). Nii, et oma olemasolevaid andmeid talletatud seal saab töödelda säde ning talletatud uuesti WASB tulemused.

WASB mudelite või failide salvestamiseks tee peab olema määratud õigesti. Vaike-ümbrisest, manustatud säde kobar viidatakse abil tee algavad sõnadega: "wasb: / / /". Muudesse asukohtadesse on viidatud, "wasb: / /".


### <a name="set-directory-paths-for-storage-locations-in-wasb"></a>WASB directory teed salvestusruumi asukohtade määramine

Järgmine kood näidis saate määrata asukoha lugeda andmed ja mudel salvestusruumi kataloogi mudeli väljundi salvestatud tee.


    # SET PATHS TO FILE LOCATIONS: DATA AND MODEL STORAGE

    # LOCATION OF TRAINING DATA
    taxi_train_file_loc = "wasb://mllibwalkthroughs@cdspsparksamples.blob.core.windows.net/Data/NYCTaxi/JoinedTaxiTripFare.Point1Pct.Train.tsv";

    # SET THE MODEL STORAGE DIRECTORY PATH 
    # NOTE THAT THE FINAL BACKSLASH IN THE PATH IS NEEDED.
    modelDir = "wasb:///user/remoteuser/NYCTaxi/Models/" 


### <a name="import-libraries"></a>Teekide importimine

Häälestamine nõuab ka importimise vajalikud teegid. Säde konteksti määramine ja importimine vajalikud teekide järgmine kood:


    # IMPORT LIBRARIES
    import pyspark
    from pyspark import SparkConf
    from pyspark import SparkContext
    from pyspark.sql import SQLContext
    import matplotlib
    import matplotlib.pyplot as plt
    from pyspark.sql import Row
    from pyspark.sql.functions import UserDefinedFunction
    from pyspark.sql.types import *
    import atexit
    from numpy import array
    import numpy as np
    import datetime


### <a name="preset-spark-context-and-pyspark-magics"></a>Algne säde konteksti ja PySpark magics

PySpark tuumad, mis on varustatud Jupyter märkmikud on loodud kontekstis. Seega pole vaja määrata säde või taru kontekstides konkreetselt enne alustamist töötamine rakenduse arenevad. Nendes kontekstides on vaikimisi teie jaoks saadaval. Nendes kontekstides on:

- SC - säde jaoks 
- sqlContext - mesilaspere

PySpark tuum pakub mõni eelmääratletud "maagia", mis on erikäsud, mille abil saate helistada %%. On kaks nende koodinäiteid kasutatud käske.

- **%% kohaliku** Määrab, et read koodi käivitada kohalikult. Kood peab olema lubatud Python kood.
- **%%sql -o <variable name>** Käivitab taru päring on sqlContext. Kui parameeter -o on möödunud, päringu tulem on säilis soovitud %% kohaliku Python kontekstis Pandas DataFrame nimega.
 

Jaoks Lisateavet tuumad Jupyter märkmike ja soovitud eelmääratletud "maagia" mida nad pakuvad, vt [tuumad Jupyter märkmikke Hdinsightiga säde Linux kogumite kohta Hdinsightiga jaoks saadaval](../hdinsight/hdinsight-apache-spark-jupyter-notebook-kernels.md).
 

## <a name="data-ingestion-from-public-blob"></a>Andmete manustamisest kaudu avaliku bloobimälu

Andmete teadus protsessi esimene samm on neelata analüüsitavat allikatest pärit andmete kus on oma andmete uurimine ja modelleerimine keskkond asub. Keskkond on säde ülevaate. See jaotis sisaldab mitmeid ülesandeid täita:

- andmete proovi modelleerida neelata
- lugeda Sisestuskeel andmekomplekti (TSV failina salvestatud)
- Vormindage ja andmete puhastamiseks
- loomine ja vahemälu mälu objektide (RDDs või andmed raamid)
- registreerida temp tabelina SQL-kontekstis.

Siin on kood andmete kohta.

    # INGEST DATA

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # IMPORT FILE FROM PUBLIC BLOB
    taxi_train_file = sc.textFile(taxi_train_file_loc)
    
    # GET SCHEMA OF THE FILE FROM HEADER
    schema_string = taxi_train_file.first()
    fields = [StructField(field_name, StringType(), True) for field_name in schema_string.split('\t')]
    fields[7].dataType = IntegerType() #Pickup hour
    fields[8].dataType = IntegerType() # Pickup week
    fields[9].dataType = IntegerType() # Weekday
    fields[10].dataType = IntegerType() # Passenger count
    fields[11].dataType = FloatType() # Trip time in secs
    fields[12].dataType = FloatType() # Trip distance
    fields[19].dataType = FloatType() # Fare amount
    fields[20].dataType = FloatType() # Surcharge
    fields[21].dataType = FloatType() # Mta_tax
    fields[22].dataType = FloatType() # Tip amount
    fields[23].dataType = FloatType() # Tolls amount
    fields[24].dataType = FloatType() # Total amount
    fields[25].dataType = IntegerType() # Tipped or not
    fields[26].dataType = IntegerType() # Tip class
    taxi_schema = StructType(fields)
    
    # PARSE FIELDS AND CONVERT DATA TYPE FOR SOME FIELDS
    taxi_header = taxi_train_file.filter(lambda l: "medallion" in l)
    taxi_temp = taxi_train_file.subtract(taxi_header).map(lambda k: k.split("\t"))\
            .map(lambda p: (p[0],p[1],p[2],p[3],p[4],p[5],p[6],int(p[7]),int(p[8]),int(p[9]),int(p[10]),
                            float(p[11]),float(p[12]),p[13],p[14],p[15],p[16],p[17],p[18],float(p[19]),
                            float(p[20]),float(p[21]),float(p[22]),float(p[23]),float(p[24]),int(p[25]),int(p[26])))
    
    
    # CREATE DATA FRAME
    taxi_train_df = sqlContext.createDataFrame(taxi_temp, taxi_schema)
    
    # CREATE A CLEANED DATA-FRAME BY DROPPING SOME UN-NECESSARY COLUMNS & FILTERING FOR UNDESIRED VALUES OR OUTLIERS
    taxi_df_train_cleaned = taxi_train_df.drop('medallion').drop('hack_license').drop('store_and_fwd_flag').drop('pickup_datetime')\
        .drop('dropoff_datetime').drop('pickup_longitude').drop('pickup_latitude').drop('dropoff_latitude')\
        .drop('dropoff_longitude').drop('tip_class').drop('total_amount').drop('tolls_amount').drop('mta_tax')\
        .drop('direct_distance').drop('surcharge')\
        .filter("passenger_count > 0 and passenger_count < 8 AND payment_type in ('CSH', 'CRD') AND tip_amount >= 0 AND tip_amount < 30 AND fare_amount >= 1 AND fare_amount < 150 AND trip_distance > 0 AND trip_distance < 100 AND trip_time_in_secs > 30 AND trip_time_in_secs < 7200" )

    
    # CACHE DATA-FRAME IN MEMORY & MATERIALIZE DF IN MEMORY
    taxi_df_train_cleaned.cache()
    taxi_df_train_cleaned.count()
    
    # REGISTER DATA-FRAME AS A TEMP-TABLE IN SQL-CONTEXT
    taxi_df_train_cleaned.registerTempTable("taxi_train")
    
    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds";

**VÄLJUND:**

Kuluv lahtri kohal: 51.72 sekundit


## <a name="data-exploration--visualization"></a>Andmete uurimine ja visualiseerimine

Kui andmed on toodud säde, on järgmiseks andmete teadus protsessi mõistmiseks süvitsi andmete uurimine ja visualiseerimine kaudu. Selles jaotises läbi takso andmed, kasutades SQL-päringud ja joonistada target muutujate ja visuaalselt tulevane funktsioone. Täpsemalt me diagrammile sagedus reisija loendab takso reisi, sagedus näpunäite summade ja kuidas näpunäited erineb suuruse ja tüüp.

### <a name="plot-a-histogram-of-passenger-count-frequencies-in-the-sample-of-taxi-trips"></a>Histogrammi reisija count sagedusega takso reisi valimis diagrammile

See kood ja edaspidised Koodilõigud maagiline SQL-i abil päringu valimi ja kohalik maagiline andmete joonestamiseks.

- **SQL-i maagiline (`%%sql`)** Hdinsightiga PySpark tuum toetab lihtne Tekstisisese HiveQL päringute selle sqlContext vastu. Funktsiooni (-o muutuja_nimi) argument püsib SQL-päringu väljund Pandas DataFrame Jupyter serveris. See tähendab, et see on saadaval kohalikus režiimis.
- Funktsiooni ** `%%local` maagiline** kasutatakse Jupyter serveris, mis on headnode Hdinsightiga klaster kohalikult koodi käivitada. Tavaliselt kasutatakse `%%local` maagiline koos selle `%%sql` maagiline -o parameetriga. -O parameeter püsiksid kohalikult SQL-päringu väljund ja seejärel %% kohaliku maagiline oleks täitmisega saate käivitada kohalikult vastu väljund SQL-päringud, mis on kestnud kohalikult koodilõigu käivitamine

Väljund visualiseeritakse automaatselt pärast koodi käivitamiseks.

Selle päringu toob reisi reisijate arvu järgi. 

    # PLOT FREQUENCY OF PASSENGER COUNTS IN TAXI TRIPS

    # HIVEQL QUERY AGAINST THE sqlContext
    %%sql -q -o sqlResults
    SELECT passenger_count, COUNT(*) as trip_counts 
    FROM taxi_train 
    WHERE passenger_count > 0 and passenger_count < 7 
    GROUP BY passenger_count 

Järgmine kood loob kohalikud andmed paneeli päringu väljund ja kujundab andmed. Funktsiooni `%%local` maagiline loob kohalikud andmed raam, `sqlResults`, mida saab kasutada koos matplotlib joonestamist. 

>[AZURE.NOTE] See PySpark maagiline kasutatakse korduvalt juhendis. Kui suurt hulka andmeid on suur, tuleks proovida, luua andmete paneeli, mis ei sobi kohaliku mälu.

    #CREATE LOCAL DATA-FRAME AND USE FOR MATPLOTLIB PLOTTING

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER
    %%local
    
    # USE THE JUPYTER AUTO-PLOTTING FEATURE TO CREATE INTERACTIVE FIGURES. 
    # CLICK ON THE TYPE OF PLOT TO BE GENERATED (E.G. LINE, AREA, BAR ETC.)
    sqlResults

Siin on kood diagrammile reisi reisijate arvu alusel

    # PLOT PASSENGER NUMBER VS. TRIP COUNTS
    %%local
    import matplotlib.pyplot as plt
    %matplotlib inline
    
    x_labels = sqlResults['passenger_count'].values
    fig = sqlResults[['trip_counts']].plot(kind='bar', facecolor='lightblue')
    fig.set_xticklabels(x_labels)
    fig.set_title('Counts of trips by passenger count')
    fig.set_xlabel('Passenger count in trips')
    fig.set_ylabel('Trip counts')
    plt.show()

**VÄLJUND:**

![Ärireisi sagedus reisijate arvu järgi](./media/machine-learning-data-science-spark-data-exploration-modeling/trip-freqency-by-passenger-count.png)

Märkmiku **Tüüp** menüü nuppude abil saate valida mitut erinevat tüüpi visualiseeringud (tabel, sektordiagramm, rea, ala või riba). Diagrammi lint on näidatud siin.
    
### <a name="plot-a-histogram-of-tip-amounts-and-how-tip-amount-varies-by-passenger-count-and-fare-amounts"></a>Diagrammile histogrammi näpunäite ja kuidas näpunäite summa sõltub reisijate count ja hind summad.

Kasutage SQL-päringu Näidisandmete abil.

    #PLOT HISTOGRAM OF TIP AMOUNTS AND VARIATION BY PASSENGER COUNT AND PAYMENT TYPE
    
    # HIVEQL QUERY AGAINST THE sqlContext
    %%sql -q -o sqlResults
    SELECT fare_amount, passenger_count, tip_amount, tipped 
    FROM taxi_train 
    WHERE passenger_count > 0 
    AND passenger_count < 7 
    AND fare_amount > 0 
    AND fare_amount < 200 
    AND payment_type in ('CSH', 'CRD') 
    AND tip_amount > 0 
    AND tip_amount < 25


See koodi lahter kasutab SQL-päringu andmed kolme loomiseks.

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER
    %%local
    
    # HISTOGRAM OF TIP AMOUNTS AND PASSENGER COUNT
    ax1 = sqlResults[['tip_amount']].plot(kind='hist', bins=25, facecolor='lightblue')
    ax1.set_title('Tip amount distribution')
    ax1.set_xlabel('Tip Amount ($)')
    ax1.set_ylabel('Counts')
    plt.suptitle('')
    plt.show()
    
    # TIP BY PASSENGER COUNT
    ax2 = sqlResults.boxplot(column=['tip_amount'], by=['passenger_count'])
    ax2.set_title('Tip amount by Passenger count')
    ax2.set_xlabel('Passenger count')
    ax2.set_ylabel('Tip Amount ($)')
    plt.suptitle('')
    plt.show()
    
    # TIP AMOUNT BY FARE AMOUNT, POINTS ARE SCALED BY PASSENGER COUNT
    ax = sqlResults.plot(kind='scatter', x= 'fare_amount', y = 'tip_amount', c='blue', alpha = 0.10, s=5*(sqlResults.passenger_count))
    ax.set_title('Tip amount by Fare amount')
    ax.set_xlabel('Fare Amount ($)')
    ax.set_ylabel('Tip Amount ($)')
    plt.axis([-2, 100, -2, 20])
    plt.show()


**VÄLJUND:** 

![Näpunäide jaotamine](./media/machine-learning-data-science-spark-data-exploration-modeling/tip-amount-distribution.png)

![Näpunäide summa reisijate arvu järgi](./media/machine-learning-data-science-spark-data-exploration-modeling/tip-amount-by-passenger-count.png)

![Näpunäide hind summa summa](./media/machine-learning-data-science-spark-data-exploration-modeling/tip-amount-by-fare-amount.png)


## <a name="feature-engineering-transformation-and-data-preparation-for-modeling"></a>Matemaatika erifunktsioonid, teisendamiseks ja andmete modelleerimine ettevalmistamine esiletõstmine
Selles jaotises kirjeldatakse ja antakse koodi kasutatakse andmete modelleerimine ML kasutamiseks ette valmistada. See näitab, kuidas teha järgmisi toiminguid:

- Luua uus funktsioon binning tunni liikluse aja ämbrid
- Indekseerimine ja kodeerida kategooriline funktsioonid
- Siltidega punkti objektide jaoks sisestatud ML funktsioonide loomine
- Juhusliku sub asutuste andmete loomine ja koolitus ja testimine komplekti tükeldamine
- Funktsioon mastaapimine
- Vahemälu objektide mälu


### <a name="create-a-new-feature-by-binning-hours-into-traffic-time-buckets"></a>Luua uus funktsioon binning tunni liikluse aja ämbrid

Järgmine kood näitab, kuidas luua uus funktsioon binning tunni liikluse aja ämbrid ja seejärel kuidas vahemälu tulemiks olevad andmed paneeli mälu. Kui olles jaotatud andmekomplektide (RDDs) ja andmed raamid kasutatakse korduvalt, vahemällu viib täiustatud täitmise korda. Seega me vahemälu RDDs ja andmed raamid mitmes etapis soovitud ülevaate veebisaidil. 

    # CREATE FOUR BUCKETS FOR TRAFFIC TIMES
    sqlStatement = """
        SELECT *,
        CASE
         WHEN (pickup_hour <= 6 OR pickup_hour >= 20) THEN "Night" 
         WHEN (pickup_hour >= 7 AND pickup_hour <= 10) THEN "AMRush" 
         WHEN (pickup_hour >= 11 AND pickup_hour <= 15) THEN "Afternoon"
         WHEN (pickup_hour >= 16 AND pickup_hour <= 19) THEN "PMRush"
        END as TrafficTimeBins
        FROM taxi_train 
    """
    taxi_df_train_with_newFeatures = sqlContext.sql(sqlStatement)
    
    # CACHE DATA-FRAME IN MEMORY & MATERIALIZE DF IN MEMORY
    # THE .COUNT() GOES THROUGH THE ENTIRE DATA-FRAME,
    # MATERIALIZES IT IN MEMORY, AND GIVES THE COUNT OF ROWS.
    taxi_df_train_with_newFeatures.cache()
    taxi_df_train_with_newFeatures.count()

**VÄLJUND:** 

126050

### <a name="index-and-encode-categorical-features-for-input-into-modeling-functions"></a>Indekseerimine ja kodeerida kategooriline funktsioonid sisestatud modelleerimine funktsioonid

Selles jaotises kirjeldatakse lepingute indekseerida või kodeerida kategooriline funktsioonid sisestatud modelleerimine funktsioonid. Modelleerimine ja prognoosida MLlib funktsioonid nõuavad funktsioonide kategooriline sisendandmete indekseeritud või kodeeritud enne kasutamist. Mudelist, peate indekseerida või kodeerida neid erineval viisil:  

- **Puu-põhiste modelleerimine** nõuab olema kodeeritud arvväärtustena kategooriate (nt funktsiooni, mille kolmele võib olla kodeeritud 0; 1; 2). See on esitatud MLlib's [StringIndexer](http://spark.apache.org/docs/latest/ml-features.html#stringindexer) funktsioon. See funktsioon kodeeritakse stringi veergu sildid sildi indeksite sildi sageduse tellitud veerule. Kuigi indekseeritud sisestus- ja andmetöötlus arvväärtused, saab määrata neid õigesti käsitlema kategooriad puu-põhiste algoritmide kohta. 

- **Logistika ja lineaarse regressioonisirge mudelite** nõuavad ühe kuum kodeerimine, kus, näiteks saab funktsiooni, mille kolmele laiendada kolme funktsiooni veergu, iga sisaldavad 0 või 1 sõltuvalt on jälgimine. MLlib pakub [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) funktsioon teha üks kuum kodeeringus. See kodeerija kaardid veeru silt indeksite veerule kahendarvu vektorite koos kõige ühe ühe-väärtuse. Kodeeringut võimaldab algoritmide kohta, mida oodata arvväärtuste väärtusega funktsioone, näiteks logistilise regressiooni, peaks rakenduma kategooriline funktsioone.

Siin on kood indeks ja kodeerida kategooriline funktsioonid:


    # INDEX AND ENCODE CATEGORICAL FEATURES

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES    
    from pyspark.ml.feature import OneHotEncoder, StringIndexer, VectorAssembler, VectorIndexer
    
    # INDEX AND ENCODE VENDOR_ID
    stringIndexer = StringIndexer(inputCol="vendor_id", outputCol="vendorIndex")
    model = stringIndexer.fit(taxi_df_train_with_newFeatures) # Input data-frame is the cleaned one from above
    indexed = model.transform(taxi_df_train_with_newFeatures)
    encoder = OneHotEncoder(dropLast=False, inputCol="vendorIndex", outputCol="vendorVec")
    encoded1 = encoder.transform(indexed)
    
    # INDEX AND ENCODE RATE_CODE
    stringIndexer = StringIndexer(inputCol="rate_code", outputCol="rateIndex")
    model = stringIndexer.fit(encoded1)
    indexed = model.transform(encoded1)
    encoder = OneHotEncoder(dropLast=False, inputCol="rateIndex", outputCol="rateVec")
    encoded2 = encoder.transform(indexed)
    
    # INDEX AND ENCODE PAYMENT_TYPE
    stringIndexer = StringIndexer(inputCol="payment_type", outputCol="paymentIndex")
    model = stringIndexer.fit(encoded2)
    indexed = model.transform(encoded2)
    encoder = OneHotEncoder(dropLast=False, inputCol="paymentIndex", outputCol="paymentVec")
    encoded3 = encoder.transform(indexed)
    
    # INDEX AND TRAFFIC TIME BINS
    stringIndexer = StringIndexer(inputCol="TrafficTimeBins", outputCol="TrafficTimeBinsIndex")
    model = stringIndexer.fit(encoded3)
    indexed = model.transform(encoded3)
    encoder = OneHotEncoder(dropLast=False, inputCol="TrafficTimeBinsIndex", outputCol="TrafficTimeBinsVec")
    encodedFinal = encoder.transform(indexed)
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**VÄLJUND:**

Kuluv lahtri kohal: 1.28 sekundit

### <a name="create-labeled-point-objects-for-input-into-ml-functions"></a>Siltidega punkti objektide jaoks sisestatud ML funktsioonide loomine

See jaotis sisaldab koodi, mis näitab, kuidas indekseerida kategooriline tekstandmeid siltidega punkti andmetüübina ja kodeerida see, et seda saab kasutada koolitada ja testida MLlib logistilise regressiooni ja muud liigitamine mudelid. Siltidega punkti objektid on olles jaotatud andmekomplektide (RDD) nii, et on vaja sisendandmete enamik ML algoritmide kohta rakenduses MLlib vormindatud. [Sildistatud punkti](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) on kohalik vektorkuju, kas tihe või vähe, seotud sildi/vastusega.  

See jaotis sisaldab koodi, mis näitab, kuidas indekseerida kategooriline tekstandmeid andmetüübina [sildistatud punkti](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) ja kodeerida see nii, et seda saab kasutada koolitada ja testida MLlib logistilise regressiooni ja muud liigitamine mudelid. Siltidega punkti objektid on olles jaotatud andmekomplektide (RDD) silt (target/vastuse muutuja) ja vector funktsiooni. Selles vormingus on vaja sisendina palju ML algoritmide kohta rakenduses MLlib.

Siin on kood indeks ja kodeerida kahendarvu liigitamine teksti funktsioone.

    # FUNCTIONS FOR BINARY CLASSIFICATION

    # LOAD LIBRARIES
    from pyspark.mllib.regression import LabeledPoint
    from numpy import array

    # INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
    def parseRowIndexingBinary(line):
        features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.TrafficTimeBinsIndex,
                             line.pickup_hour, line.weekday, line.passenger_count, line.trip_time_in_secs, 
                             line.trip_distance, line.fare_amount])
        labPt = LabeledPoint(line.tipped, features)
        return  labPt
    
    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO LOGISTIC RERESSION MODELS
    def parseRowOneHotBinary(line):
        features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
                                            line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
                                            line.vendorVec.toArray(), line.rateVec.toArray(), 
                                            line.paymentVec.toArray(), line.TrafficTimeBinsVec.toArray()), axis=0)
        labPt = LabeledPoint(line.tipped, features)
        return  labPt


Siin on kood kodeerida ja indekseerida kategooriline teksti funktsioonid lineaarse regressioonisirge analüüsi jaoks.

    # FUNCTIONS FOR REGRESSION WITH TIP AMOUNT AS TARGET VARIABLE

    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
    def parseRowIndexingRegression(line):
        features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.TrafficTimeBinsIndex, 
                             line.pickup_hour, line.weekday, line.passenger_count, line.trip_time_in_secs, 
                             line.trip_distance, line.fare_amount])

        labPt = LabeledPoint(line.tip_amount, features)
        return  labPt
    
    # INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO LINEAR REGRESSION MODELS
    def parseRowOneHotRegression(line):
        features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
                                            line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
                                            line.vendorVec.toArray(), line.rateVec.toArray(), 
                                            line.paymentVec.toArray(), line.TrafficTimeBinsVec.toArray()), axis=0)
        labPt = LabeledPoint(line.tip_amount, features)
        return  labPt


### <a name="create-a-random-sub-sampling-of-the-data-and-split-it-into-training-and-testing-sets"></a>Juhusliku sub asutuste andmete loomine ja koolitus ja testimine komplekti tükeldamine

Järgmine kood tekitab juhuslikku valimit (25% on siin kasutatud) andmed. Kuigi see ei ole vaja näites andmekomplekti suuruse tõttu, näitame, kuidas saate proovida siin nii, et teate, kuidas kasutada oma probleemi vastavalt vajadusele. Kui suur näiteid, saate selle märgatavat aja koolitus mudelite ajal salvestada. Järgmine tükeldada valimi kasutamine liigitamine ja regressiooni modelleerimine koolitus osa (siin 75%) ja testimise osa (25% siin).


    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.sql.functions import rand

    # SPECIFY SAMPLING AND SPLITTING FRACTIONS
    samplingFraction = 0.25;
    trainingFraction = 0.75; testingFraction = (1-trainingFraction);
    seed = 1234;
    encodedFinalSampled = encodedFinal.sample(False, samplingFraction, seed=seed)
    
    # SPLIT SAMPLED DATA-FRAME INTO TRAIN/TEST
    # INCLUDE RAND COLUMN FOR CREATING CROSS-VALIDATION FOLDS (FOR USE LATER IN AN ADVANCED TOPIC)
    dfTmpRand = encodedFinalSampled.select("*", rand(0).alias("rand"));
    trainData, testData = dfTmpRand.randomSplit([trainingFraction, testingFraction], seed=seed);
    
    # FOR BINARY CLASSIFICATION TRAINING AND TESTING
    indexedTRAINbinary = trainData.map(parseRowIndexingBinary)
    indexedTESTbinary = testData.map(parseRowIndexingBinary)
    oneHotTRAINbinary = trainData.map(parseRowOneHotBinary)
    oneHotTESTbinary = testData.map(parseRowOneHotBinary)
    
    # FOR REGRESSION TRAINING AND TESTING
    indexedTRAINreg = trainData.map(parseRowIndexingRegression)
    indexedTESTreg = testData.map(parseRowIndexingRegression)
    oneHotTRAINreg = trainData.map(parseRowOneHotRegression)
    oneHotTESTreg = testData.map(parseRowOneHotRegression)
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**VÄLJUND:**

Kuluv lahtri kohal: 0,24 sekundit


### <a name="feature-scaling"></a>Funktsioon mastaapimine

Funktsioon skaala tuntud ka kui andmete normaliseerimine, kindlustab, et suuresti väljastatud väärtustega funktsioonid on pole antud liigse kaaluda eesmärk funktsiooni. Funktsioon skaleerimist kood kasutab [StandardScaler](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.feature.StandardScaler) mastaapimiseks ühiku dispersioon funktsioone. See on esitatud MLlib lineaarse regressioonisirge koos Stohhastiline astmiku päritolu (SGD), populaarsed algoritmi koolitus mitmesuguseid muid seadme õppe mudelid, nt lahendada regressioon või tugiteenuste vektorkuju masinad (SVM) jaoks kasutada.

>[AZURE.NOTE] Oleme leidnud LinearRegressionWithSGD algoritmi tundlik funktsioonide skaleerimist.

Siin on skaala muutujate regularized lineaarse SGD algoritmi kasutamiseks koodi.

    # FEATURE SCALING

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.regression import LabeledPoint
    from pyspark.mllib.linalg import Vectors
    from pyspark.mllib.feature import StandardScaler, StandardScalerModel
    from pyspark.mllib.util import MLUtils
    
    # SCALE VARIABLES FOR REGULARIZED LINEAR SGD ALGORITHM
    label = oneHotTRAINreg.map(lambda x: x.label)
    features = oneHotTRAINreg.map(lambda x: x.features)
    scaler = StandardScaler(withMean=False, withStd=True).fit(features)
    dataTMP = label.zip(scaler.transform(features.map(lambda x: Vectors.dense(x.toArray()))))
    oneHotTRAINregScaled = dataTMP.map(lambda x: LabeledPoint(x[0], x[1]))
    
    label = oneHotTESTreg.map(lambda x: x.label)
    features = oneHotTESTreg.map(lambda x: x.features)
    scaler = StandardScaler(withMean=False, withStd=True).fit(features)
    dataTMP = label.zip(scaler.transform(features.map(lambda x: Vectors.dense(x.toArray()))))
    oneHotTESTregScaled = dataTMP.map(lambda x: LabeledPoint(x[0], x[1]))
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**VÄLJUND:**

Kuluv lahtri kohal: 13.17 sekundit


### <a name="cache-objects-in-memory"></a>Vahemälu objektide mälu

Aja koolitus ja testimine ML algoritmide kohta saab vähendada vahemällu sisendandmete paneeli objektide liigitamiseks regressiooni, kasutada ja mastaabitud funktsioone.

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # FOR BINARY CLASSIFICATION TRAINING AND TESTING
    indexedTRAINbinary.cache()
    indexedTESTbinary.cache()
    oneHotTRAINbinary.cache()
    oneHotTESTbinary.cache()
    
    # FOR REGRESSION TRAINING AND TESTING
    indexedTRAINreg.cache()
    indexedTESTreg.cache()
    oneHotTRAINreg.cache()
    oneHotTESTreg.cache()
    
    # SCALED FEATURES
    oneHotTRAINregScaled.cache()
    oneHotTESTregScaled.cache()
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**VÄLJUND:** 

Kuluv lahtri kohal: 0,15 sekundit


## <a name="predict-whether-or-not-a-tip-is-paid-with-binary-classification-models"></a>Prognoosida, kas Näpunäide makstakse kahendarvu liigitamine mudelid

Selles jaotises kirjeldatakse, kuidas kasutada kolme mudelid kahendarvu liigitamine tööülesande prognoosida, kas Näpunäide makstakse takso reisi. Esitatud mudelid on:

- Lahendada logistilise regressiooni 
- Juhusliku mets mudel
- Astmikvärvi suurendada puude

Iga mudeli koostamise koodi jaotises on jaotatud järgmisteks alljaotisteks juhiseid. 

1. Üks parameeter on seatud **mudeli koolitus** andmed
2. **Mudeli hindamise** testi andmehulgas mõõdikute abil
3. **Salvestamise mudeli** tulevaste ettenähtud bloobimälu

### <a name="classification-using-logistic-regression"></a>Kasutades logistilist regressiooni liigitamine

Selle jaotise kood näitab, kuidas koolitada, hinnata ja salvestage logistilise regressiooni mudeli prognoosib kas Näpunäide makstakse reisi NYC takso reisi ja hind andmekogumis [LBFGS](https://en.wikipedia.org/wiki/Broyden%E2%80%93Fletcher%E2%80%93Goldfarb%E2%80%93Shanno_algorithm) .

**Koolitada logistilise regressiooni mudeli CV- ja hyperparameter pühkimine abil**

    # LOGISTIC REGRESSION CLASSIFICATION WITH CV AND HYPERPARAMETER SWEEPING

    # GET ACCURACY FOR HYPERPARAMETERS BASED ON CROSS-VALIDATION IN TRAINING DATA-SET

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD LIBRARIES
    from pyspark.mllib.classification import LogisticRegressionWithLBFGS 
    from sklearn.metrics import roc_curve,auc
    from pyspark.mllib.evaluation import BinaryClassificationMetrics
    from pyspark.mllib.evaluation import MulticlassMetrics
    
    
    # CREATE MODEL WITH ONE SET OF PARAMETERS
    logitModel = LogisticRegressionWithLBFGS.train(oneHotTRAINbinary, iterations=20, initialWeights=None, 
                                                   regParam=0.01, regType='l2', intercept=True, corrections=10, 
                                                   tolerance=0.0001, validateData=True, numClasses=2)
    
    # PRINT COEFFICIENTS AND INTERCEPT OF THE MODEL
    # NOTE: There are 20 coefficient terms for the 10 features, 
    #       and the different categories for features: vendorVec (2), rateVec, paymentVec (6), TrafficTimeBinsVec (4)
    print("Coefficients: " + str(logitModel.weights))
    print("Intercept: " + str(logitModel.intercept))
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**VÄLJUND:** 

Kordajad: [0.0082065285375,-0.0223675576104,-0.0183812028036, - 3.48124578069e-05-0.00247646947233,-0.00165897881503, 0.0675394837328,-0.111823113101,-0.324609912762,-0.204549780032,-1.36499216354, 0.591088507921,-0.664263411392,-1.00439726852, 3.46567827545,-3.51025855172,-0.0471341112232,-0.043521833294, 0.000243375810385, 0.054518719222]

Intercept:-0.0111216486893

Kuluv lahtri kohal: 14.43 sekundit

**Binaarsed mudeli koos standard mõõdikute hindamine**

    #EVALUATE LOGISTIC REGRESSION MODEL WITH LBFGS

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # PREDICT ON TEST DATA WITH MODEL
    predictionAndLabels = oneHotTESTbinary.map(lambda lp: (float(logitModel.predict(lp.features)), lp.label))
    
    # INSTANTIATE METRICS OBJECT
    metrics = BinaryClassificationMetrics(predictionAndLabels)

    # AREA UNDER PRECISION-RECALL CURVE
    print("Area under PR = %s" % metrics.areaUnderPR)

    # AREA UNDER ROC CURVE
    print("Area under ROC = %s" % metrics.areaUnderROC)
    metrics = MulticlassMetrics(predictionAndLabels)

    # OVERALL STATISTICS
    precision = metrics.precision()
    recall = metrics.recall()
    f1Score = metrics.fMeasure()
    print("Summary Stats")
    print("Precision = %s" % precision)
    print("Recall = %s" % recall)
    print("F1 Score = %s" % f1Score)


    ## SAVE MODEL WITH DATE-STAMP
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    logisticregressionfilename = "LogisticRegressionWithLBFGS_" + datestamp;
    dirfilename = modelDir + logisticregressionfilename;
    logitModel.save(sc, dirfilename);
    
    # OUTPUT PROBABILITIES AND REGISTER TEMP TABLE
    logitModel.clearThreshold(); # This clears threshold for classification (0.5) and outputs probabilities
    predictionAndLabelsDF = predictionAndLabels.toDF()
    predictionAndLabelsDF.registerTempTable("tmp_results");
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds";

**VÄLJUND:** 

Jaotises hind ala = 0.985297691373

Ala ROC = 0.983714670256

Kokkuvõte, statistika

Arvutustäpsuse = 0.984304060189

Tagasi kutsuda = 0.984304060189

F1 Keskmine = 0.984304060189

Kuluv lahtri kohal: 57.61 sekundit

**Diagrammile ROC kõver.**

Tabeli *tmp_results*, eelmise lahtri on registreeritud *predictionAndLabelsDF* . *tmp_results* saab teha päringuid ja väljundi tulemuste sqlResults andmete raami sisse joonestamist jaoks. Siin on kood.


    # QUERY RESULTS                              
    %%sql -q -o sqlResults
    SELECT * from tmp_results


Siin on teha prognoose ja kanda ROC-kõvera kood.

    # MAKE PREDICTIONS AND PLOT ROC-CURVE

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    %matplotlib inline
    from sklearn.metrics import roc_curve,auc

    # MAKE PREDICTIONS
    predictions_pddf = test_predictions.rename(columns={'_1': 'probability', '_2': 'label'})
    prob = predictions_pddf["probability"] 
    fpr, tpr, thresholds = roc_curve(predictions_pddf['label'], prob, pos_label=1);
    roc_auc = auc(fpr, tpr)

    # PLOT ROC CURVE
    plt.figure(figsize=(5,5))
    plt.plot(fpr, tpr, label='ROC curve (area = %0.2f)' % roc_auc)
    plt.plot([0, 1], [0, 1], 'k--')
    plt.xlim([0.0, 1.0])
    plt.ylim([0.0, 1.05])
    plt.xlabel('False Positive Rate')
    plt.ylabel('True Positive Rate')
    plt.title('ROC Curve')
    plt.legend(loc="lower right")
    plt.show()
    

**VÄLJUND:**

![Logistilise regressiooni ROC curve.png](./media/machine-learning-data-science-spark-data-exploration-modeling/logistic-regression-roc-curve.png)


### <a name="random-forest-classification"></a>Juhusliku mets liigitamine

Selle jaotise kood näitab, kuidas koolitada, hinnata ja juhusliku mets mudelit, mis prognoosib kas Näpunäide makstakse reisi NYC takso reisi ja hind andmekogumis salvestamine.
    
    #PREDICT WHETHER A TIP IS PAID OR NOT USING RANDOM FOREST

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import RandomForest, RandomForestModel
    from pyspark.mllib.util import MLUtils
    from pyspark.mllib.evaluation import BinaryClassificationMetrics
    from pyspark.mllib.evaluation import MulticlassMetrics
    
    # SPECIFY NUMBER OF CATEGORIES FOR CATEGORICAL FEATURES. FEATURE #0 HAS 2 CATEGORIES, FEATURE #2 HAS 2 CATEGORIES, AND SO ON
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    
    # TRAIN RANDOMFOREST MODEL
    rfModel = RandomForest.trainClassifier(indexedTRAINbinary, numClasses=2, 
                                           categoricalFeaturesInfo=categoricalFeaturesInfo,
                                           numTrees=25, featureSubsetStrategy="auto",
                                           impurity='gini', maxDepth=5, maxBins=32)
    ## UN-COMMENT IF YOU WANT TO PRINT TREES
    #print('Learned classification forest model:')
    #print(rfModel.toDebugString())
    
    # PREDICT ON TEST DATA AND EVALUATE
    predictions = rfModel.predict(indexedTESTbinary.map(lambda x: x.features))
    predictionAndLabels = indexedTESTbinary.map(lambda lp: lp.label).zip(predictions)
    
    # AREA UNDER ROC CURVE
    metrics = BinaryClassificationMetrics(predictionAndLabels)
    print("Area under ROC = %s" % metrics.areaUnderROC)
    
    # PERSIST MODEL IN BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    rfclassificationfilename = "RandomForestClassification_" + datestamp;
    dirfilename = modelDir + rfclassificationfilename;
    
    rfModel.save(sc, dirfilename);
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**VÄLJUND:**

Ala ROC = 0.985297691373

Kuluv lahtri kohal: 31.09 sekundit


### <a name="gradient-boosting-trees-classification"></a>Astmikvärvi suurendada puude liigitamine

Selle jaotise kood kujutatakse koolitada, hinnata, ja salvestage astmiku suurendada juhul mudel, mis prognoosib kas Näpunäide makstakse reisi NYC taksosõidu rakenduses ja hind andmekomplekti.

    #PREDICT WHETHER A TIP IS PAID OR NOT USING GRADIENT BOOSTING TREES

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel
    
    # SPECIFY NUMBER OF CATEGORIES FOR CATEGORICAL FEATURES. FEATURE #0 HAS 2 CATEGORIES, FEATURE #2 HAS 2 CATEGORIES, AND SO ON
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    
    gbtModel = GradientBoostedTrees.trainClassifier(indexedTRAINbinary, categoricalFeaturesInfo=categoricalFeaturesInfo, numIterations=5)
    ## UNCOMMENT IF YOU WANT TO PRINT TREE DETAILS
    #print('Learned classification GBT model:')
    #print(bgtModel.toDebugString())
    
    # PREDICT ON TEST DATA AND EVALUATE
    predictions = gbtModel.predict(indexedTESTbinary.map(lambda x: x.features))
    predictionAndLabels = indexedTESTbinary.map(lambda lp: lp.label).zip(predictions)
    
    # AREA UNDER ROC CURVE
    metrics = BinaryClassificationMetrics(predictionAndLabels)
    print("Area under ROC = %s" % metrics.areaUnderROC)
    
    # PERSIST MODEL IN A BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btclassificationfilename = "GradientBoostingTreeClassification_" + datestamp;
    dirfilename = modelDir + btclassificationfilename;
    
    gbtModel.save(sc, dirfilename)
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**VÄLJUND:**

Ala ROC = 0.985297691373

Kuluv lahtri kohal: 19.76 sekundit


## <a name="predict-tip-amounts-for-taxi-trips-with-regression-models"></a>Näpunäide summad takso reisi regressiooni mudelid prognoosida

Selles jaotises kirjeldatakse, kuidas kasutada kolme mudelid regressiooni ülesanne prognoosida väljaostetud takso reisi näpunäidet summa alusel näpunäite muud funktsioonid. Esitatud mudelid on:

- Lahendada lineaarregressioon
- Juhusliku mets
- Astmikvärvi suurendada puude

Need mudelid on kirjeldatud Sissejuhatus. Iga mudeli koostamise koodi jaotises on jaotatud järgmisteks alljaotisteks juhiseid. 

1. Üks parameeter on seatud **mudeli koolitus** andmed
2. **Mudeli hindamise** testi andmehulgas mõõdikute abil
3. **Salvestamise mudeli** tulevaste ettenähtud bloobimälu

### <a name="linear-regression-with-sgd"></a>Koos SGD lineaarregressioon 

Selle jaotise kood kuvatakse lineaarse regressioonisirge, mis kasutab Stohhastiline astmiku päritolu (SGD) optimeerimine koolitada mastaabitud funktsioonide kasutamise kohta ja kuidas Keskmine, hinnata ja salvestage mudel Azure'i bloobimälu salvestusruumi (WASB).

>[AZURE.TIP] Meie kogemus, ei saa olla probleemid LinearRegressionWithSGD mudelite lähenemise ja parameetrid peavad olema muudetud/optimeeritud hoolikalt saamiseks kehtiv mudel. Lähenemise skaleerimist märkimisväärselt muutujate abil. 


    #PREDICT TIP AMOUNTS USING LINEAR REGRESSION WITH SGD

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD LIBRARIES
    from pyspark.mllib.regression import LabeledPoint, LinearRegressionWithSGD, LinearRegressionModel
    from pyspark.mllib.evaluation import RegressionMetrics
    from scipy import stats
    
    # USE SCALED FEATURES TO TRAIN MODEL
    linearModel = LinearRegressionWithSGD.train(oneHotTRAINregScaled, iterations=100, step = 0.1, regType='l2', regParam=0.1, intercept = True)

    # PRINT COEFFICIENTS AND INTERCEPT OF THE MODEL
    # NOTE: There are 20 coefficient terms for the 10 features, 
    #       and the different categories for features: vendorVec (2), rateVec, paymentVec (6), TrafficTimeBinsVec (4)
    print("Coefficients: " + str(linearModel.weights))
    print("Intercept: " + str(linearModel.intercept))
    
    # SCORE ON SCALED TEST DATA-SET & EVALUATE
    predictionAndLabels = oneHotTESTregScaled.map(lambda lp: (float(linearModel.predict(lp.features)), lp.label))
    testMetrics = RegressionMetrics(predictionAndLabels)
    
    # PRINT TEST METRICS
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)
    
    # SAVE MODEL WITH DATE-STAMP IN THE DEFAULT BLOB FOR THE CLUSTER
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    linearregressionfilename = "LinearRegressionWithSGD_" + datestamp;
    dirfilename = modelDir + linearregressionfilename;
    
    linearModel.save(sc, dirfilename)
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**VÄLJUND:**

Kordajad: [0.00457675809917,-0.0226314167349,-0.0191910355236, 0.246793409578, 0.312047890459, 0.359634405999, 0.00928692253981,-0.000987181489428,-0.0888306617845, 0.0569376211553, 0.115519551711, 0.149250164995,-0.00990211159703,-0.00637410344522, 0.545083566179,-0.536756072402, 0.0105762393099,-0.0130117577055, 0.0129304737772,-0.00171065945959]

Intercept: 0.853872718283

RMSE = 1.24190115863

R-sqr = 0.608017146081

Kuluv lahtri kohal: 58.42 sekundit


### <a name="random-forest-regression"></a>Juhuslik mets regressiooni

Selle jaotise kood näitab, kuidas koolitada, hinnata ja juhusliku mets regressiooni, mis prognoosib näpunäite summa NYC takso reisi andmed salvestada.


    #PREDICT TIP AMOUNTS USING RANDOM FOREST

    # RECORD START TIME
    timestart= datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import RandomForest, RandomForestModel
    from pyspark.mllib.util import MLUtils
    from pyspark.mllib.evaluation import RegressionMetrics
    
    
    ## TRAIN MODEL
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    rfModel = RandomForest.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                        numTrees=25, featureSubsetStrategy="auto",
                                        impurity='variance', maxDepth=10, maxBins=32)
    ## UN-COMMENT IF YOU WANT TO PRING TREES
    #print('Learned classification forest model:')
    #print(rfModel.toDebugString())
    
    ## PREDICT AND EVALUATE ON TEST DATA-SET
    predictions = rfModel.predict(indexedTESTreg.map(lambda x: x.features))
    predictionAndLabels = oneHotTESTreg.map(lambda lp: lp.label).zip(predictions)

    # TEST METRICS
    testMetrics = RegressionMetrics(predictionAndLabels)
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)
    
    # SAVE MODEL IN BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    rfregressionfilename = "RandomForestRegression_" + datestamp;
    dirfilename = modelDir + rfregressionfilename;
    
    rfModel.save(sc, dirfilename);
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**VÄLJUND:**

RMSE = 0.891209218139

R-sqr = 0.759661334921

Kuluv lahtri kohal: 49.21 sekundit


### <a name="gradient-boosting-trees-regression"></a>Astmikvärvi suurendada puude regressiooni

Selle jaotise kood näitab, kuidas koolitada, hinnata ja astmiku suurendada puude mudelit, mis prognoosib näpunäite summa NYC takso reisi andmed salvestada.

**Koolitada ja hindamine**

    #PREDICT TIP AMOUNTS USING GRADIENT BOOSTING TREES

    # RECORD START TIME
    timestart= datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel
    from pyspark.mllib.util import MLUtils
    
    ## TRAIN MODEL
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    gbtModel = GradientBoostedTrees.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo, 
                                                    numIterations=10, maxBins=32, maxDepth = 4, learningRate=0.1)
    
    ## EVALUATE A TEST DATA-SET
    predictions = gbtModel.predict(indexedTESTreg.map(lambda x: x.features))
    predictionAndLabels = indexedTESTreg.map(lambda lp: lp.label).zip(predictions)

    # TEST METRICS
    testMetrics = RegressionMetrics(predictionAndLabels)
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)

    # SAVE MODEL IN BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btregressionfilename = "GradientBoostingTreeRegression_" + datestamp;
    dirfilename = modelDir + btregressionfilename;
    gbtModel.save(sc, dirfilename)
    
    # CONVER RESULTS TO DF AND REGISER TEMP TABLE
    test_predictions = sqlContext.createDataFrame(predictionAndLabels)
    test_predictions.registerTempTable("tmp_results");
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**VÄLJUND:**

RMSE = 0.908473148639

R-sqr = 0.753835096681

Kuluv lahtri kohal: 34.52 sekundit

**Diagrammile**

*tmp_results* on registreeritud taru tabeli eelmisesse lahtrisse. Tulemuste tabelis on väljundi *sqlResults* andmete-raami jaoks joonestamist. Siin on kood

    # PLOT SCATTER-PLOT BETWEEN ACTUAL AND PREDICTED TIP VALUES

    # SELECT RESULTS
    %%sql -q -o sqlResults
    SELECT * from tmp_results

Siin on Jupyter serveri kasutamine andmete joonestamiseks kood.

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    %matplotlib inline
    import numpy as np

    # PLOT 
    ax = test_predictions_pddf.plot(kind='scatter', figsize = (6,6), x='_1', y='_2', color='blue', alpha = 0.25, label='Actual vs. predicted');
    fit = np.polyfit(test_predictions_pddf['_1'], test_predictions_pddf['_2'], deg=1)
    ax.set_title('Actual vs. Predicted Tip Amounts ($)')
    ax.set_xlabel("Actual")
    ax.set_ylabel("Predicted")
    ax.plot(test_predictions_pddf['_1'], fit[0] * test_predictions_pddf['_1'] + fit[1], color='magenta')
    plt.axis([-1, 20, -1, 20])
    plt.show(ax)
    

**VÄLJUND:**

![Tegelik-vs-ennustatud-Näpunäide-summad](./media/machine-learning-data-science-spark-data-exploration-modeling/actual-vs-predicted-tips.png)

    
## <a name="clean-up-objects-from-memory"></a>Objektide mälust puhastamiseks

Kasutage `unpersist()` vahemälus objektide kustutamine.
        
    # REMOVE ORIGINAL DFs
    taxi_df_train_cleaned.unpersist()
    taxi_df_train_with_newFeatures.unpersist()
    
    # FOR BINARY CLASSIFICATION TRAINING AND TESTING
    indexedTRAINbinary.unpersist()
    indexedTESTbinary.unpersist()
    oneHotTRAINbinary.unpersist()
    oneHotTESTbinary.unpersist()
    
    # FOR REGRESSION TRAINING AND TESTING
    indexedTRAINreg.unpersist()
    indexedTESTreg.unpersist()
    oneHotTRAINreg.unpersist()
    oneHotTESTreg.unpersist()
    
    # SCALED FEATURES
    oneHotTRAINregScaled.unpersist()
    oneHotTESTregScaled.unpersist()


## <a name="record-storage-locations-of-the-models-for-consumption-and-scoring"></a>Kirje salvestusruumi asukohad mudelite tarbimine ja hinded

Tarbimine ja Keskmine on kirjeldatud sõltumatu andmekomplekti on [Keskmine ja hindate säde ehitatud masina õppe mudelid](machine-learning-data-science-spark-model-consumption.md) teema, peate kopeerida ja kleepida need faili nimesid, mis sisaldavad salvestatud mudelid loodud siin tarbimine Jupyter märkmikku. Siin on kood peate seal mudelifaile teed välja printida.

    # MODEL FILE LOCATIONS FOR CONSUMPTION
    print "logisticRegFileLoc = modelDir + \"" + logisticregressionfilename + "\"";
    print "linearRegFileLoc = modelDir + \"" + linearregressionfilename + "\"";
    print "randomForestClassificationFileLoc = modelDir + \"" + rfclassificationfilename + "\"";
    print "randomForestRegFileLoc = modelDir + \"" + rfregressionfilename + "\"";
    print "BoostedTreeClassificationFileLoc = modelDir + \"" + btclassificationfilename + "\"";
    print "BoostedTreeRegressionFileLoc = modelDir + \"" + btregressionfilename + "\"";


**VÄLJUND**

logisticRegFileLoc = modelDir + "LogisticRegressionWithLBFGS_2016-05-0317_03_23.516568"

linearRegFileLoc = modelDir + "LinearRegressionWithSGD_2016-05-0317_05_21.577773"

randomForestClassificationFileLoc = modelDir + "RandomForestClassification_2016-05-0317_04_11.950206"

randomForestRegFileLoc = modelDir + "RandomForestRegression_2016-05-0317_06_08.723736"

BoostedTreeClassificationFileLoc = modelDir + "GradientBoostingTreeClassification_2016-05-0317_04_36.346583"

BoostedTreeRegressionFileLoc = modelDir + "GradientBoostingTreeRegression_2016-05-0317_06_51.737282"


## <a name="whats-next"></a>Mis saab edasi?

Nüüd, kui olete loonud regressiooni ja liigitamine mudelite säde MlLib, olete valmis, saate teada, kuidas Keskmine ja nende mudelite hinnata. Täiustatud andmete uurimine ja modelleerimine märkmiku sukeldub süvitsi rist – valideerimise, hyper-parameeter pühkimine, sh ja mudeli hindamiseks. 

**Modelleerimise tarbimine:** Keskmine ja hindate liigitamine ja regressiooni mudelid luuakse selle teema kohta leiate teavet teemast [Keskmine ja hindate säde ehitatud masina õppe mudelid](machine-learning-data-science-spark-model-consumption.md).

**Rist – valideerimise ja hyperparameter pühkimine**: kuidas mudeleid saab [Täpsemad andmete uurimine ja modelleerimine säde](machine-learning-data-science-spark-advanced-data-exploration-modeling.md) näidata väljaõppe rist – valideerimise ja hyper-parameeter pühkimine abil



