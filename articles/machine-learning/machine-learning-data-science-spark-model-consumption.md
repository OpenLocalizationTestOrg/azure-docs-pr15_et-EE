<properties
    pageTitle="Keskmine säde ehitatud masina õppe mudelid | Microsoft Azure'i"
    description="Kuidas Keskmine õppe mudelid, mis on salvestatud Azure'i bloobimälu salvestusruumi (WASB)."
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

# <a name="score-spark-built-machine-learning-models"></a>Keskmine säde ehitatud masina õppe mudelid 

[AZURE.INCLUDE [machine-learning-spark-modeling](../../includes/machine-learning-spark-modeling.md)]

Selles teemas kirjeldatakse, kuidas laadida seadme Õppekeskuse (ML) mudelid, mis on ehitatud säde MLlib ja talletatud Azure'i bloobimälu salvestusruumi (WASB), ja kuidas neid kogumid, mis on salvestatud ka WASB Keskmine. See näitab, kuidas eelnevalt töötlemine sisendandmete, muuta funktsioonide indekseerimise ja kodeering funktsioonide kasutamise MLlib tööriistakomplekt, ja kuidas luua siltidega punkti andmete objekti, mida saab kasutada sisendina ML mudelid hinded. Kasutatud hinded mudelid sisaldavad lineaarse regressioonisirge, logistilise regressiooni, juhusliku mets mudelite ja astmiku suurendamine puu mudelid.


## <a name="prerequisites"></a>Eeltingimused

1. Peate Azure'i konto ja mõne Hdinsightiga säde te vajate Hdinsightiga 3.4 säde 1,6 kobar juhendis lõpuleviimiseks. [Ülevaade andmete teadus kasutamise kohta Windows Azure Hdinsightiga säde](machine-learning-data-science-spark-overview.md) juhiste saamiseks vaadake Kuidas nendele nõuetele. Selle teema sisaldab ka siin kasutatud NYC 2013 takso andmed ja juhised, kuidas käivitada koodi Jupyter märkmiku säde klaster kirjeldus. **PySpark-machine-learning-data-science-spark-model-consumption.ipynb** märkmikku, mis sisaldab koodinäiteid selles teemas on saadaval [Github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/pySpark).

2. Peate looma ka masina Õppekeskuse mudelite reageerima siin töö kaudu [andmete uurimine ja modelleerimine säde](machine-learning-data-science-spark-data-exploration-modeling.md) teema järgi.   


[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]
 

## <a name="setup-storage-locations-libraries-and-the-preset-spark-context"></a>Install: salvestusruumi asukohad, teekide ja eelseadistatud säde kontekst

Säde on lugeda ja kirjutada, mis on Azure salvestusruumi bloobimälu (WASB). Nii, et oma olemasolevaid andmeid talletatud seal saab töödelda säde ning talletatud uuesti WASB tulemused.

WASB mudelite või failide salvestamiseks tee peab olema määratud õigesti. Vaike-ümbrisest, manustatud säde kobar viidatakse abil tee algavad sõnadega: *"wasb / /"*. Järgmine kood näidis saate määrata asukoha lugeda andmed ja mudel salvestusruumi kataloogi mudeli väljundi salvestatud tee. 


### <a name="set-directory-paths-for-storage-locations-in-wasb"></a>WASB directory teed salvestusruumi asukohtade määramine

Mudelid salvestatakse: "wasb: / / / kasutaja/remoteuser/NYCTaxi/mudelite". Kui see tee on määratud õigesti, ei laadita mudelite jaoks hinded.

Poolitusjoonega tulemused salvestatakse: "wasb: / / / kasutaja/remoteuser/NYCTaxi/ScoredResults". Kui kausta tee on vale, ei salvestata tulemused selles kaustas.   


>[AZURE.NOTE] Faili tee asukohad saab kopeerida ja kleepida kohatäited selle koodi väljundi **machine-learning-data-science-spark-data-exploration-modeling.ipynb** märkmiku viimane lahter.   


Siin on seadmiseks directory teed kood: 

    # LOCATION OF DATA TO BE SCORED (TEST DATA)
    taxi_test_file_loc = "wasb://mllibwalkthroughs@cdspsparksamples.blob.core.windows.net/Data/NYCTaxi/JoinedTaxiTripFare.Point1Pct.Test.tsv";
    
    # SET THE MODEL STORAGE DIRECTORY PATH 
    # NOTE THE LAST BACKSLASH IN THIS PATH IS NEEDED
    modelDir = "wasb:///user/remoteuser/NYCTaxi/Models/" 
    
    # SET SCORDED RESULT DIRECTORY PATH
    # NOTE THE LAST BACKSLASH IN THIS PATH IS NEEDED
    scoredResultDir = "wasb:///user/remoteuser/NYCTaxi/ScoredResults/"; 
    
    # FILE LOCATIONS FOR THE MODELS TO BE SCORED
    logisticRegFileLoc = modelDir + "LogisticRegressionWithLBFGS_2016-04-1817_40_35.796789"
    linearRegFileLoc = modelDir + "LinearRegressionWithSGD_2016-04-1817_44_00.993832"
    randomForestClassificationFileLoc = modelDir + "RandomForestClassification_2016-04-1817_42_58.899412"
    randomForestRegFileLoc = modelDir + "RandomForestRegression_2016-04-1817_44_27.204734"
    BoostedTreeClassificationFileLoc = modelDir + "GradientBoostingTreeClassification_2016-04-1817_43_16.354770"
    BoostedTreeRegressionFileLoc = modelDir + "GradientBoostingTreeRegression_2016-04-1817_44_46.206262"

    # RECORD START TIME
    import datetime
    datetime.datetime.now()

**VÄLJUND:**

datetime.datetime (2016, 4, 25, 23, 56, 19, 229403)


### <a name="import-libraries"></a>Teekide importimine

Määratud säde konteksti ja vajalikud teekide järgmine kood importimine

    #IMPORT LIBRARIES
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

PySpark tuumad, mis on varustatud Jupyter märkmikud on loodud kontekstis. Seega pole vaja määrata säde või taru kontekstides konkreetselt enne alustamist töötamine rakenduse arenevad. Need on vaikimisi teie jaoks saadaval. Nendes kontekstides on:

- SC - säde jaoks 
- sqlContext - mesilaspere

PySpark tuum pakub mõni eelmääratletud "maagia", mis on erikäsud, mille abil saate helistada %%. On kaks nende koodinäiteid kasutatud käske.

- **%% kohaliku** Määratud koodi read käivitatakse kohalikult. Kood peab olema lubatud Python kood.
- **%% sql -o<variable name>** 
- Käivitab taru päring on sqlContext. Kui -o parameeter on möödunud, päringu tulem on säilis soovitud %% kohaliku Python kontekstis Pandas dataframe nimega.
 

Jaoks Lisateavet tuumad Jupyter märkmike ja soovitud eelmääratletud "maagia" mida nad pakuvad, vt [tuumad Jupyter märkmikke Hdinsightiga säde Linux kogumite kohta Hdinsightiga jaoks saadaval](../hdinsight/hdinsight-apache-spark-jupyter-notebook-kernels.md).


## <a name="ingest-data-and-create-a-cleaned-data-frame"></a>Andmete neelata ja puhastada andmete raami loomine

See jaotis sisaldab sarja vajalikud andmed loetakse neelata ülesanded. Lugege ühendatud 0,1% valimi takso reisi ja hind faili (TSV failina salvestatud), vorming andmed ja seejärel loob selge andmete raami.

Takso reisi ja hind failid on liitunud sätestatud korras vastavalt selle: [meeskonnatöö andmete teadus protsessi tegelikkuses: Hdinsightiga Hadoopi kogumite abil](machine-learning-data-science-process-hive-walkthrough.md) teema.

    # INGEST DATA AND CREATE A CLEANED DATA FRAME

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # IMPORT FILE FROM PUBLIC BLOB
    taxi_test_file = sc.textFile(taxi_test_file_loc)
    
    # GET SCHEMA OF THE FILE FROM HEADER
    taxi_header = taxi_test_file.filter(lambda l: "medallion" in l)
    
    # PARSE FIELDS AND CONVERT DATA TYPE FOR SOME FIELDS
    taxi_temp = taxi_test_file.subtract(taxi_header).map(lambda k: k.split("\t"))\
            .map(lambda p: (p[0],p[1],p[2],p[3],p[4],p[5],p[6],int(p[7]),int(p[8]),int(p[9]),int(p[10]),
                            float(p[11]),float(p[12]),p[13],p[14],p[15],p[16],p[17],p[18],float(p[19]),
                            float(p[20]),float(p[21]),float(p[22]),float(p[23]),float(p[24]),int(p[25]),int(p[26])))
        
    # GET SCHEMA OF THE FILE FROM HEADER
    schema_string = taxi_test_file.first()
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
    
    # CREATE DATA FRAME
    taxi_df_test = sqlContext.createDataFrame(taxi_temp, taxi_schema)
    
    # CREATE A CLEANED DATA-FRAME BY DROPPING SOME UN-NECESSARY COLUMNS & FILTERING FOR UNDESIRED VALUES OR OUTLIERS
    taxi_df_test_cleaned = taxi_df_test.drop('medallion').drop('hack_license').drop('store_and_fwd_flag').drop('pickup_datetime')\
        .drop('dropoff_datetime').drop('pickup_longitude').drop('pickup_latitude').drop('dropoff_latitude')\
        .drop('dropoff_longitude').drop('tip_class').drop('total_amount').drop('tolls_amount').drop('mta_tax')\
        .drop('direct_distance').drop('surcharge')\
        .filter("passenger_count > 0 and passenger_count < 8 AND payment_type in ('CSH', 'CRD') AND tip_amount >= 0 AND tip_amount < 30 AND fare_amount >= 1 AND fare_amount < 150 AND trip_distance > 0 AND trip_distance < 100 AND trip_time_in_secs > 30 AND trip_time_in_secs < 7200" )
    
    # CACHE DATA-FRAME IN MEMORY & MATERIALIZE DF IN MEMORY
    taxi_df_test_cleaned.cache()
    taxi_df_test_cleaned.count()
    
    # REGISTER DATA-FRAME AS A TEMP-TABLE IN SQL-CONTEXT
    taxi_df_test_cleaned.registerTempTable("taxi_test")
    
    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**VÄLJUND:**

Kuluv lahtri kohal: 46,37 sekundit


## <a name="prepare-data-for-scoring-in-spark"></a>Hinded säde andmete ettevalmistamine 

Selles jaotises kirjeldatakse, kuidas indekseerida, kodeerida ja mastaapimiseks kategooriline funktsioonide kasutamise MLlib kontrollitud õ algoritmide liigitamine ja regressiooni ettevalmistamine.

### <a name="feature-transformation-index-and-encode-categorical-features-for-input-into-models-for-scoring"></a>Funktsioon teisendus: indekseerida ja kodeerida sisestatud mudelid hinded kategooriline funktsioonid 

Selles jaotises kirjeldatakse lepingute kategooriline andmed, kasutades mõnda `StringIndexer` ja kodeerida funktsioonide `OneHotEncoder` sisestatud mudelid.

[StringIndexer](http://spark.apache.org/docs/latest/ml-features.html#stringindexer) kodeeritakse stringi veergu sildid sildi indeksite veerule. Sildi sageduse järgi järjestatud indeksid. 

[OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) kaardid veeru silt indeksite veerule kahendarvu vektorite koos kõige ühe ühe-väärtuse. Kodeeringut võimaldab algoritmide kohta, mida oodata pidev väärtusega funktsioone, näiteks logistilise regressiooni, peaks rakenduma kategooriline funktsioone.
    
    #INDEX AND ONE-HOT ENCODE CATEGORICAL FEATURES

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.ml.feature import OneHotEncoder, StringIndexer, VectorAssembler, VectorIndexer
    
    # CREATE FOUR BUCKETS FOR TRAFFIC TIMES
    sqlStatement = """
        SELECT *,
        CASE
         WHEN (pickup_hour <= 6 OR pickup_hour >= 20) THEN "Night" 
         WHEN (pickup_hour >= 7 AND pickup_hour <= 10) THEN "AMRush" 
         WHEN (pickup_hour >= 11 AND pickup_hour <= 15) THEN "Afternoon"
         WHEN (pickup_hour >= 16 AND pickup_hour <= 19) THEN "PMRush"
        END as TrafficTimeBins
        FROM taxi_test 
    """
    taxi_df_test_with_newFeatures = sqlContext.sql(sqlStatement)
    
    # CACHE DATA-FRAME IN MEMORY & MATERIALIZE DF IN MEMORY
    taxi_df_test_with_newFeatures.cache()
    taxi_df_test_with_newFeatures.count()
    
    # INDEX AND ONE-HOT ENCODING
    stringIndexer = StringIndexer(inputCol="vendor_id", outputCol="vendorIndex")
    model = stringIndexer.fit(taxi_df_test_with_newFeatures) # Input data-frame is the cleaned one from above
    indexed = model.transform(taxi_df_test_with_newFeatures)
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
    
    # INDEX AND ENCODE TRAFFIC TIME BINS
    stringIndexer = StringIndexer(inputCol="TrafficTimeBins", outputCol="TrafficTimeBinsIndex")
    model = stringIndexer.fit(encoded3)
    indexed = model.transform(encoded3)
    encoder = OneHotEncoder(dropLast=False, inputCol="TrafficTimeBinsIndex", outputCol="TrafficTimeBinsVec")
    encodedFinal = encoder.transform(indexed)
    
    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**VÄLJUND:**

Kuluv lahtri kohal: 5.37 sekundit


### <a name="create-rdd-objects-with-feature-arrays-for-input-into-models"></a>Funktsioon massiivides sisendi mudelid RDD objektide loomine

See jaotis sisaldab koodi, mis näitab, kuidas indeks kategooriline tekstandmeid RDD objektina ja üks kuum kodeerida, saate seda kasutada koolitada ja testida MLlib logistilise regressiooni ja mudelite puu-põhine. Indekseeritud andmed salvestatakse [Olles jaotatud andmekomplekti (RDD)](http://spark.apache.org/docs/latest/api/java/org/apache/spark/rdd/RDD.html) objektid. Need on lihtne võtmiseks klõpsake säde. Objekti RDD tähistab elemendid, mida saate käitada paralleelselt säde püsiv, sektsioonitud kogum.

See sisaldab ka kood, mis näitab, kuidas andmeid mastaapimiseks soovitud `StandardScalar` esitatud MLlib lineaarse regressioonisirge koos Stohhastiline astmiku päritolu (SGD), populaarsed algoritmi jaoks mitmesuguseid masina õppe mudelid koolitus kasutamiseks. [StandardScaler](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.feature.StandardScaler) on kasutatud funktsioone ühiku dispersioon skaala. Funktsioon skaala tuntud ka kui andmete normaliseerimine, kindlustab, et suuresti väljastatud väärtustega funktsioonid on pole antud liigse kaaluda eesmärk funktsiooni. 


    # CREATE RDD OBJECTS WITH FEATURE ARRAYS FOR INPUT INTO MODELS

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # IMPORT LIBRARIES
    from pyspark.mllib.linalg import Vectors
    from pyspark.mllib.feature import StandardScaler, StandardScalerModel
    from pyspark.mllib.util import MLUtils
    from numpy import array
    
    # INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
    def parseRowIndexingBinary(line):
        features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.TrafficTimeBinsIndex,
                             line.pickup_hour, line.weekday, line.passenger_count, line.trip_time_in_secs, 
                             line.trip_distance, line.fare_amount])
        return  features
    
    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO LOGISTIC RERESSION MODELS
    def parseRowOneHotBinary(line):
        features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
                                            line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
                                            line.vendorVec.toArray(), line.rateVec.toArray(), 
                                            line.paymentVec.toArray(), line.TrafficTimeBinsVec.toArray()), axis=0)
        return  features
    
    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
    def parseRowIndexingRegression(line):
        features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.TrafficTimeBinsIndex, 
                             line.pickup_hour, line.weekday, line.passenger_count, line.trip_time_in_secs, 
                             line.trip_distance, line.fare_amount])
        return  features
    
    # INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO LINEAR REGRESSION MODELS
    def parseRowOneHotRegression(line):
        features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
                                            line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
                                            line.vendorVec.toArray(), line.rateVec.toArray(), 
                                            line.paymentVec.toArray(), line.TrafficTimeBinsVec.toArray()), axis=0)
        return  features

    # FOR BINARY CLASSIFICATION TRAINING AND TESTING
    indexedTESTbinary = encodedFinal.map(parseRowIndexingBinary)
    oneHotTESTbinary = encodedFinal.map(parseRowOneHotBinary)
    
    # FOR REGRESSION CLASSIFICATION TRAINING AND TESTING
    indexedTESTreg = encodedFinal.map(parseRowIndexingRegression)
    oneHotTESTreg = encodedFinal.map(parseRowOneHotRegression)
    
    # SCALING FEATURES FOR LINEARREGRESSIONWITHSGD MODEL
    scaler = StandardScaler(withMean=False, withStd=True).fit(oneHotTESTreg)
    oneHotTESTregScaled = scaler.transform(oneHotTESTreg)
    
    # CACHE RDDS IN MEMORY
    indexedTESTbinary.cache();
    oneHotTESTbinary.cache();
    indexedTESTreg.cache();
    oneHotTESTreg.cache();
    oneHotTESTregScaled.cache();
    
    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**VÄLJUND:**

Kuluv lahtri kohal: 11,72 sekundit


## <a name="score-with-the-logistic-regression-model-and-save-output-to-blob"></a>Keskmine logistilist regressiooni mudeliga ja väljundi Bloobivahemälu salvestamine

Selle jaotise kood näitab, kuidas logistilise regressiooni mudelit, mis on salvestatud Azure'i bloobimälu laadimine ja selle abil prognoosida, kas Näpunäide makstakse takso reisi Keskmine see standard liigitamine mõõdikute, ja seejärel salvestage ning joonistada bloobimälu tulemused. Poolitusjoonega tulemused salvestatakse RDD objektid. 


    # SCORE AND EVALUATE LOGISTIC REGRESSION MODEL

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # IMPORT LIBRARIES
    from pyspark.mllib.classification import LogisticRegressionModel
    
    ## LOAD SAVED MODEL
    savedModel = LogisticRegressionModel.load(sc, logisticRegFileLoc)
    predictions = oneHotTESTbinary.map(lambda features: (float(savedModel.predict(features))))
    
    ## SAVE SCORED RESULTS (RDD) TO BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    logisticregressionfilename = "LogisticRegressionWithLBFGS_" + datestamp + ".txt";
    dirfilename = scoredResultDir + logisticregressionfilename;
    predictions.saveAsTextFile(dirfilename)
    
    
    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds";

**VÄLJUND:**

Kuluv lahtri kohal: 19.22 sekundit


## <a name="score-a-linear-regression-model"></a>Keskmine mudeli lineaarregressioon

Kasutasime [LinearRegressionWithSGD](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.regression.LinearRegressionWithSGD) koolitada lineaarse regressioonisirge mudeli kasutamine Stohhastiline astmiku päritolu (SGD) optimeerimine prognoosida näpunäite makstud summa. 

Selle jaotise kood näitab, kuidas alla laadida lineaarse regressioonisirge mudeli Azure'i bloobimälu, Keskmine mastaabitud muutujate abil ja seejärel salvestage tulemused tagasi soovitud bloobimälu.

    #SCORE LINEAR REGRESSION MODEL

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    #LOAD LIBRARIES
    from pyspark.mllib.regression import LinearRegressionWithSGD, LinearRegressionModel
    
    # LOAD MODEL AND SCORE USING ** SCALED VARIABLES **
    savedModel = LinearRegressionModel.load(sc, linearRegFileLoc)
    predictions = oneHotTESTregScaled.map(lambda features: (float(savedModel.predict(features))))
    
    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    linearregressionfilename = "LinearRegressionWithSGD_" + datestamp;
    dirfilename = scoredResultDir + linearregressionfilename;
    predictions.saveAsTextFile(dirfilename)
    
    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**VÄLJUND:**

Kuluv lahtri kohal: 16.63 sekundit


## <a name="score-classification-and-regression-random-forest-models"></a>Keskmine liigitamine ja regressiooni juhusliku mets mudelite

Selle jaotise kood näitab, kuidas salvestatud liigitamine laadimine ja regressiooni juhusliku mets mudelite salvestatud Azure'i bloobimälu, Keskmine nende jõudlust standard liigitaja ja regressiooni mõõdud ja salvestage tulemused tagasi bloobimälu.

[Juhuslik metsad](http://spark.apache.org/docs/latest/mllib-ensembles.html#Random-Forests) on otsust puude komplektid.  Need ühendada palju otsust puude overfitting vähendada. Juhusliku metsade saab hakkama kategooriline funktsioone, multiclass liigitamine, millega laiendada, pole vaja funktsiooni skaleerimist ja saavad jäädvustada mittelineaarsust ja funktsioonide populaarsed. Juhusliku metsad on üks kõige edukamad masina õppe liigitamiseks ja regressiooni mudelid.

[Spark.mllib](http://spark.apache.org/mllib/) toetab juhusliku metsade kahendarvu ja multiclass liigitamine ja regressiooni, pidev- ja kategooriline funktsioonide abil. 

    # SCORE RANDOM FOREST MODELS FOR CLASSIFICATION AND REGRESSION

    # RECORD START TIME
    timestart = datetime.datetime.now()

    #IMPORT MLLIB LIBRARIES 
    from pyspark.mllib.tree import RandomForest, RandomForestModel
    
    
    # CLASSIFICATION: LOAD SAVED MODEL, SCORE AND SAVE RESULTS BACK TO BLOB
    savedModel = RandomForestModel.load(sc, randomForestClassificationFileLoc)
    predictions = savedModel.predict(indexedTESTbinary)
    
    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    rfclassificationfilename = "RandomForestClassification_" + datestamp + ".txt";
    dirfilename = scoredResultDir + rfclassificationfilename;
    predictions.saveAsTextFile(dirfilename)
    

    # REGRESSION: LOAD SAVED MODEL, SCORE AND SAVE RESULTS BACK TO BLOB
    savedModel = RandomForestModel.load(sc, randomForestRegFileLoc)
    predictions = savedModel.predict(indexedTESTreg)
    
    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    rfregressionfilename = "RandomForestRegression_" + datestamp + ".txt";
    dirfilename = scoredResultDir + rfregressionfilename;
    predictions.saveAsTextFile(dirfilename)

    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds";

**VÄLJUND:**

Kuluv lahtri kohal: 31.07 sekundit


## <a name="score-classification-and-regression-gradient-boosting-tree-models"></a>Keskmine liigitamine ja regressiooni astmiku suurendamine puu mudelite

Selle jaotise kood näitab, kuidas alla laadida liigitamine ja regressiooni astmiku suurendamine puu mudelite Azure'i bloobimälu, Keskmine nende jõudlust standard liigitaja ja regressiooni mõõdud ja seejärel salvestage tulemused tagasi bloobimälu. 

**Spark.mllib** toetab GBTs kahendarvu liigitamine ja regressiooni, pidev- ja kategooriline funktsioonide abil. 

[Astmikvärvi suurendada puude](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) on otsust puude komplektid. GBTs koolitada otsust puude korduvalt kaotsimineku funktsiooni minimeerimiseks. GBTs võite hakkama kategooriline funktsioone, pole vaja funktsiooni skaleerimist ja saavad jäädvustada mittelineaarsust ja funktsioonide kasutusviisid. Neid saab kasutada multiclass-liigitamine säte.


    # SCORE GRADIENT BOOSTING TREE MODELS FOR CLASSIFICATION AND REGRESSION

    # RECORD START TIME
    timestart = datetime.datetime.now()

    #IMPORT MLLIB LIBRARIES
    from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel
    
    # CLASSIFICATION: LOAD SAVED MODEL, SCORE AND SAVE RESULTS BACK TO BLOB

    #LOAD AND SCORE THE MODEL
    savedModel = GradientBoostedTreesModel.load(sc, BoostedTreeClassificationFileLoc)
    predictions = savedModel.predict(indexedTESTbinary)
    
    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btclassificationfilename = "GradientBoostingTreeClassification_" + datestamp + ".txt";
    dirfilename = scoredResultDir + btclassificationfilename;
    predictions.saveAsTextFile(dirfilename)
    

    # REGRESSION: LOAD SAVED MODEL, SCORE AND SAVE RESULTS BACK TO BLOB

    # LOAD AND SCORE MODEL 
    savedModel = GradientBoostedTreesModel.load(sc, BoostedTreeRegressionFileLoc)
    predictions = savedModel.predict(indexedTESTreg)
    
    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btregressionfilename = "GradientBoostingTreeRegression_" + datestamp + ".txt";
    dirfilename = scoredResultDir + btregressionfilename;
    predictions.saveAsTextFile(dirfilename)


    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 
    
**VÄLJUND:**

Kuluv lahtri kohal: 14.6 sekundi


## <a name="clean-up-objects-from-memory-and-print-scored-file-locations"></a>Objektide mälust puhastamiseks ja printida poolitusjoonega failide asukohad

    # UNPERSIST OBJECTS CACHED IN MEMORY
    taxi_df_test_cleaned.unpersist()
    indexedTESTbinary.unpersist();
    oneHotTESTbinary.unpersist();
    indexedTESTreg.unpersist();
    oneHotTESTreg.unpersist();
    oneHotTESTregScaled.unpersist();


    # PRINT OUT PATH TO SCORED OUTPUT FILES
    print "logisticRegFileLoc: " + logisticregressionfilename;
    print "linearRegFileLoc: " + linearregressionfilename;
    print "randomForestClassificationFileLoc: " + rfclassificationfilename;
    print "randomForestRegFileLoc: " + rfregressionfilename;
    print "BoostedTreeClassificationFileLoc: " + btclassificationfilename;
    print "BoostedTreeRegressionFileLoc: " + btregressionfilename;


**VÄLJUND:**

logisticRegFileLoc: LogisticRegressionWithLBFGS_2016-05-0317_22_38.953814.txt

linearRegFileLoc: LinearRegressionWithSGD_2016-05-0317_22_58.878949

randomForestClassificationFileLoc: RandomForestClassification_2016-05-0317_23_15.939247.txt

randomForestRegFileLoc: RandomForestRegression_2016-05-0317_23_31.459140.txt

BoostedTreeClassificationFileLoc: GradientBoostingTreeClassification_2016-05-0317_23_49.648334.txt

BoostedTreeRegressionFileLoc: GradientBoostingTreeRegression_2016-05-0317_23_56.860740.txt



## <a name="consume-spark-models-through-a-web-interface"></a>Tarbimine säde mudelite web kasutajaliidese kaudu

Säde võimaldab esitada kaugühenduse teel komponenti Liviuse pakett-tööde või interaktiivse päringute ülejäänud kasutajaliidese kaudu. Liviuse on vaikimisi sisse Hdinsightiga säde klaster. Liviuse kohta lisateabe saamiseks lugege teemat: [kaugkustutada Liviuse esitada säde tööde haldamine](../hdinsight/hdinsight-apache-spark-livy-rest-interface.md). 

Saate Liviuse kaugühenduse teel esitada tööd, mis hinded partii faili, mis on talletatud mõne Azure'i bloobimälu ja seejärel kirjutab teise bloobimälu tulemused. Selleks saate üles laadida Python skripti kaudu  
[Github](https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/Spark/Python/ConsumeGBNYCReg.py) bloobimälu säde klaster. Saate kopeerida skripti kobar bloobimälu tööriista nagu **Microsoft Azure'i salvestusruumi Explorer** või **AzCopy** . Meie puhul me üles laadida skripti ***wasb:///example/python/ConsumeGBNYCReg.py***.   


>[AZURE.NOTE] Kiirklahvide, mida teil on vaja leiate portaali konto salvestusruumi seostatud säde kobar. 


Kui laadisite selle kohta, see skript käivitatakse sees säde kobar jaotatud kontekstis. See laadib mudeli ja käivitage prognoose vormi failid.  

Saate selle skripti kaugühenduse teel autonoomsest teha lihtsa HTTPS/ülejäänud taotluse Liviuse.  Siin on curl käsk ehitada HTTP-päringu autonoomsest Python skript kaugühenduse teel. Asendage väärtused jaoks klaster säde CLUSTERLOGIN, CLUSTERPASSWORD, CLUSTERNAME.


    # CURL COMMAND TO INVOKE PYTHON SCRIPT WITH HTTP REQUEST

    curl -k --user "CLUSTERLOGIN:CLUSTERPASSWORD" -X POST --data "{\"file\": \"wasb:///example/python/ConsumeGBNYCReg.py\"}" -H "Content-Type: application/json" https://CLUSTERNAME.azurehdinsight.net/livy/batches

Saate kasutada mis tahes keeleks kaugsüsteem viidata säde töö Liviuse kaudu, tehes lihtsa HTTPS kõne Lihttekstautentimine.   


>[AZURE.NOTE] On mugav kasutada Python taotlusi teegi selle HTTP võtke ühendust, kuid see pole praegu installitud vaikimisi Azure'i funktsioonid. Nii, et vanemate HTTP-teekide, kasutatakse selle asemel   


Siin on HTTP kõne Python kood:

    #MAKE AN HTTPS CALL ON LIVY. 

    import os

    # OLDER HTTP LIBRARIES USED HERE INSTEAD OF THE REQUEST LIBRARY AS THEY ARE AVAILBLE BY DEFAULT
    import httplib, urllib, base64
    
    # REPLACE VALUE WITH ONES FOR YOUR SPARK CLUSTER
    host = '<spark cluster name>.azurehdinsight.net:443'
    username='<username>'
    password='<password>'
    
    #AUTHORIZATION
    conn = httplib.HTTPSConnection(host)
    auth = base64.encodestring('%s:%s' % (username, password)).replace('\n', '')
    headers = {'Content-Type': 'application/json', 'Authorization': 'Basic %s' % auth}
    
    # SPECIFY THE PYTHON SCRIPT TO RUN ON THE SPARK CLUSTER
    # IN THE FILE PARAMETER OF THE JSON POST REQUEST BODY
    r=conn.request("POST", '/livy/batches', '{"file": "wasb:///example/python/ConsumeGBNYCReg.py"}', headers )
    response = conn.getresponse().read()
    print(response)
    conn.close()


Võite sisestada ka järgmine kood Python [Azure'i funktsioonide](https://azure.microsoft.com/documentation/services/functions/) säde töö esitamise, et tulemus on bloobimälu põhjal ürituste timer, loomine või lisamine bloobimälu värskendamise käivitamiseks. 

Kui eelistate kood tasuta klientide programmikasutuskogemuse, [Azure'i loogika rakendused](https://azure.microsoft.com/documentation/services/app-service/logic/) abil autonoomsest säde paketi hinded määratleb toimingu HTTP **Loogika rakenduste Designer** ja seades selle parameetrid. 

- Azure'i portaalist loomiseks uue loogika rakenduse valige **+ Uus** -> **Web + Mobile** -> **Loogika rakendus**. 
- Avab **Loogika rakenduste Designer**, sisestage nimi loogika App ja rakenduse teenuse kavandamine.
- Valige toiming HTTP ja sisestage soovitud parameetrid, mis on näidatud järgmisel joonisel:

![](./media/machine-learning-data-science-spark-model-consumption/spark-logica-app-client.png)


## <a name="whats-next"></a>Mis saab edasi? 

**Rist – valideerimise ja hyperparameter pühkimine**: kuidas mudeleid saab [Täpsemad andmete uurimine ja modelleerimine säde](machine-learning-data-science-spark-advanced-data-exploration-modeling.md) näidata väljaõppe rist – valideerimise ja hyper-parameeter pühkimine abil.
