<properties
    pageTitle="Täiustatud andmete uurimine ja modelleerimine säde | Microsoft Azure'i"
    description="Kas andmete uurimine ja koolitada kahendarvu liigitamine ja regressiooni mudelite abil rist – valideerimise ja hyperparameter optimeerimine Hdinsightiga säde abil."
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="cgronlun"  />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/07/2016"
    ms.author="deguhath;bradsev;gokuma" />

# <a name="advanced-data-exploration-and-modeling-with-spark"></a>Täiustatud andmete uurimine ja modelleerimine säde 

[AZURE.INCLUDE [machine-learning-spark-modeling](../../includes/machine-learning-spark-modeling.md)]

Juhendis kasutab Hdinsightiga säde teha andmete uurimine ja koolitada kahendarvu liigitamine ja regressiooni mudelite rist – valideerimise abil ja hyperparameter optimeerimine valimi NYC takso reisi ja hind 2013 andmekomplekti. See juhendab teid juhiseid [Andmete teadus protsessi](http://aka.ms/datascienceprocess)end end, kasutades mõnda Hdinsightiga säde klaster, töötlemiseks ja Azure plekid andmed ja mudelid talletamiseks. Protsessi uurib tänav visualiseeritakse andmete põhjal on salvestusruumi Azure'i bloobimälu ja seejärel teeb andmed prognoosmudelite koostamiseks. Python on kasutatud kood soovitud lahenduse ja oluline proovitükid kuvamiseks. Need mudelid on koostamine abil säde MLlib tööriistakomplekt kahendarvu liigitamine ja regressiooni modelleerimine tööülesandeid teha. 

- **Binaarsed liigitamine** tööülesanne on prognoosida, kas Näpunäide makstakse reisil. 
- **Regressioonisirge** tööülesanne on prognoosida põhjal muude funktsioonide näpunäite näpunäidet summa. 

Modelleerimine juhiseid sisaldada ka kood, mis näitab, kuidas koolitada, hinnata ja salvestada erinevat tüüpi mudel. Teema hõlmab osa samal põhjusel [andmete uurimine ja modelleerimine säde ja](machine-learning-data-science-spark-data-exploration-modeling.md) teema. Kuid see on rohkem "advanced", et seda ka kasutab rist – valideerimise koos hyperparameter pühkimine optimaalselt täpne liigitamine ja regressiooni mudelite koolitada. 

**Rist-valideerimine (CV)** on meetod, mis hindab, kuidas mudeli andmete teadaolevad kogumi väljaõppe üldistab prognoosida andmekomplektide, kuhu see on ei saanud funktsioonide abil. Selle tehnika üldine mõte on, et mudeli õpetatakse andmekogumis teadaolevad andmed ja seejärel oma prognoose täpsuse testitakse mõne sõltumatu andmekomplekti. Levinud rakendamist, mis on siin kasutatud on K voldid andmekomplekt jagada ja seejärel koolitada round-jaan mood kõik peale ühe voldid mudel. 

**Hyperparameter optimeerimine** on probleem, valides määrata hyperparameters õ algoritmile, tavaliselt eesmärgiga optimeerida mõõtmine on algoritmi jõudlust sõltumatu andmehulgas.. **Hyperparameters** on väärtused, mis peab olema määratud väljaspool mudeli koolitus toimingut. Oletused nende väärtuste kohta võib mõjutada paindlikkuse ja täpsuse mudelid. Otsus puude on hyperparameters, näiteks, nt soovitud sügavus ja puu lehtede arv. Tugiteenuste vektorkuju masinad (SVMs) jaoks on vaja klassifikatsioonivead karistus Termini seadmine. 

Levinud viisi optimeerimiseks hyperparameter siin kasutatud on ruudustiku otsingu või **parameetri Korrasta**. See koosneb läbimiseks on täielik otsingu kaudu väärtused määratud alamhulga hyperparameter ruumi õ algoritmi. Rist valideerimise saate pakkumise mõõdiku jõudluse sortimiseks välja koordinaatvõrgu otsingu algoritmi optimaalse tulemused. CV kasutada hyperparameter laiad aitab limiit probleeme nagu overfitting mudeli koolitus andmetega, et mudeli säilitab rakendamiseks, millest koolitus andmed ei ekstraktitud üldine andmehulga võimsus.

Kasutame mudelid järgmised logistika ja lineaarse regressioonisirge, juhusliku metsade ja astmiku võimendatud puude.

- [Lineaarse regressioonisirge koos SGD](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.regression.LinearRegressionWithSGD) on lineaarse regressioonisirge mudelit, mis kasutab Stohhastiline astmiku päritolu (SGD) ja optimeerimine ja funktsioon skaleerimist prognoosida näpunäite summad makstud. 
- [Logistilise regressiooni koos LBFGS](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.classification.LogisticRegressionWithLBFGS) või "logit" regressiooni, on regressiooni mudelit, mida saab teha andmete liigitamine kategooriline sõltuv muutuja korral. LBFGS on arvestusliku Newton optimeerimine algoritmi, mis ühtlustab Broyden – Fletcher – Goldfarb – Shanno (BFGS) algoritmi piiratud hulgal arvuti mälu abil ja mis on levinud masina õ.
- [Juhuslik metsad](http://spark.apache.org/docs/latest/mllib-ensembles.html#Random-Forests) on otsust puude komplektid.  Need ühendada palju otsust puude overfitting vähendada. Juhusliku metsade kasutatakse regressiooni ja liigitamine võite hakkama kategooriline funktsioonid ja multiclass liigitamine, millega saate pikendada. Ei ole vaja funktsiooni skaleerimist ja suudavad jäädvustada mittelineaarsust ja funktsioonide populaarsed. Juhusliku metsad on üks kõige edukamad masina õppe liigitamiseks ja regressiooni mudelid.
- [Astmiku võimendada puude](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) on otsust puude komplektid. GBTs koolitada otsust puude korduvalt kaotsimineku funktsiooni minimeerimiseks. GBTs kasutatakse regressiooni ja liigitamine võite hakkama kategooriline funktsioone, pole vaja funktsiooni skaleerimist ja saavad jäädvustada mittelineaarsust ja funktsioonide kasutusviisid. Neid saab kasutada multiclass-liigitamine säte.

Modelleerimine CV- ja Hyperparameter kasutamise näited Korrasta kuvatakse kahendarvu liigitamine probleemi. Lihtsam (ilma parameetri vallutab) on esitatud peamised teemas regressiooni tööülesanded. Kuid lisa abil elastne net lineaarse regressioonisirge ja CV parameetri Korrasta abil juhusliku mets regressiooni on esitatud ka. **Elastne net** on lahendada regressiooni jaoks sobiv lineaarse regressioonisirge mudelite meetod, mis ühendab lineaarses L1 ja L2 mõõdikute sanktsioonide [lasso](https://en.wikipedia.org/wiki/Lasso%20%28statistics%29) ja [ridge](https://en.wikipedia.org/wiki/Tikhonov_regularization) nimega.   



>[AZURE.NOTE] Kuigi säde MLlib tööriistakomplekt on loodud töötama suurte andmekogumite, kasutatakse suhteliselt näidis (abil 170K read, umbes 0,1% algse NYC andmekomplekti ~ 30 Mb) siin mugavuse. Siin antud kasutamise töötab tõhus (umbes 10 minutit) on Hdinsightiga kobar 2 töötaja sõlmed abil. Sama koodi, minimaalsed muudatused, mille saab töödelda suuremat-andmekogumi, mälu andmete vahemällu talletamine ja kobar suuruse muutmise korral muudatustega.


## <a name="prerequisites"></a>Eeltingimused

Peate Azure'i konto ja mõne Hdinsightiga säde te vajate Hdinsightiga 3.4 säde 1,6 kobar juhendis lõpuleviimiseks. [Ülevaade andmete teadus kasutamise kohta Windows Azure Hdinsightiga säde](machine-learning-data-science-spark-overview.md) juhiste saamiseks vaadake Kuidas nendele nõuetele. Selle teema sisaldab ka siin kasutatud NYC 2013 takso andmed ja juhised, kuidas käivitada koodi Jupyter märkmiku säde klaster kirjeldus. **PySpark-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb** märkmikku, mis sisaldab koodinäiteid selles teemas on saadaval [Github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/pySpark).


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
    modelDir = "wasb:///user/remoteuser/NYCTaxi/Models/";

    # PRINT START TIME
    import datetime
    datetime.datetime.now()

**VÄLJUND**

datetime.datetime (2016, 4, 18, 17, 36, 27, 832799)


### <a name="import-libraries"></a>Teekide importimine

Importida vajalikud teekide järgmine kood:

    # LOAD PYSPARK LIBRARIES
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


## <a name="data-ingestion-from-public-blob"></a>Andmete manustamisest avaliku bloobimälu kaudu: 

Esimene samm andmete teadus protsess on neelata andmeid analüüsida allikatest, kus see asub teie andmete uurimine ja modelleerimine keskkonnas. See keskkond on säde ülevaate. See jaotis sisaldab mitmeid ülesandeid täita:

- andmete proovi modelleerida neelata
- lugeda Sisestuskeel andmekomplekti (TSV failina salvestatud)
- Vormindage ja andmete puhastamiseks
- loomine ja vahemälu mälu objektide (RDDs või andmed raamid)
- registreerida temp tabelina SQL-kontekstis.

Siin on kood andmete kohta.


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
    
    # CACHE & MATERIALIZE DATA-FRAME IN MEMORY. GOING THROUGH AND COUNTING NUMBER OF ROWS MATERIALIZES THE DATA-FRAME IN MEMORY
    taxi_df_train_cleaned.cache()
    taxi_df_train_cleaned.count()
    
    # REGISTER DATA-FRAME AS A TEMP-TABLE IN SQL-CONTEXT
    taxi_df_train_cleaned.registerTempTable("taxi_train")
    
    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**VÄLJUND**

Kuluv lahtri kohal: 276.62 sekundit


## <a name="data-exploration--visualization"></a>Andmete uurimine ja visualiseerimine 

Kui andmed on toodud säde, on järgmiseks andmete teadus protsessi mõistmiseks süvitsi andmete uurimine ja visualiseerimine kaudu. Selles jaotises läbi takso andmed, kasutades SQL-päringud ja joonistada target muutujate ja visuaalselt tulevane funktsioone. Täpsemalt me diagrammile sagedus reisija loendab takso reisi, sagedus näpunäite summade ja kuidas näpunäited erineb suuruse ja tüüp.

### <a name="plot-a-histogram-of-passenger-count-frequencies-in-the-sample-of-taxi-trips"></a>Histogrammi reisija count sagedusega takso reisi valimis diagrammile

See kood ja edaspidised Koodilõigud maagiline SQL-i abil päringu valimi ja kohalik maagiline andmete joonestamiseks.

- **SQL-i maagiline (`%%sql`)** Hdinsightiga PySpark tuum toetab lihtne Tekstisisese HiveQL päringute selle sqlContext vastu. Funktsiooni (-o muutuja_nimi) argument püsib SQL-päringu väljund Pandas DataFrame Jupyter serveris. See tähendab, et see on saadaval kohalikus režiimis.
- Funktsiooni ** `%%local` maagiline** kasutatakse Jupyter serveris, mis on headnode Hdinsightiga klaster kohalikult koodi käivitada. Tavaliselt kasutatakse `%%local` maagiline koos selle `%%sql` maagiline -o parameetriga. -O parameeter püsiksid kohalikult SQL-päringu väljund ja seejärel %% kohaliku maagiline oleks täitmisega saate käivitada kohalikult vastu väljund SQL-päringud, mis on kestnud kohalikult koodilõigu käivitamine

Väljund visualiseeritakse automaatselt pärast koodi käivitamiseks.

Selle päringu toob reisi reisijate arvu järgi. 

    # PLOT FREQUENCY OF PASSENGER COUNTS IN TAXI TRIPS

    # SQL QUERY
    %%sql -q -o sqlResults
    SELECT passenger_count, COUNT(*) as trip_counts FROM taxi_train WHERE passenger_count > 0 and passenger_count < 7 GROUP BY passenger_count


Järgmine kood loob kohalikud andmed paneeli päringu väljund ja kujundab andmed. Funktsiooni `%%local` maagiline loob kohalikud andmed raam, `sqlResults`, mida saab kasutada koos matplotlib joonestamist. 

>[AZURE.NOTE] See PySpark maagiline kasutatakse korduvalt juhendis. Kui suurt hulka andmeid on suur, tuleks proovida, luua andmete paneeli, mis ei sobi kohaliku mälu.


    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER
    %%local
    
    # USE THE JUPYTER AUTO-PLOTTING FEATURE TO CREATE INTERACTIVE FIGURES. 
    # CLICK ON THE TYPE OF PLOT TO BE GENERATED (E.G. LINE, AREA, BAR ETC.)
    sqlResults

Siin on kood diagrammile reisi reisijate arvu alusel

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    import matplotlib.pyplot as plt
    %matplotlib inline
    
    # PLOT PASSENGER NUMBER VS TRIP COUNTS
    x_labels = sqlResults['passenger_count'].values
    fig = sqlResults[['trip_counts']].plot(kind='bar', facecolor='lightblue')
    fig.set_xticklabels(x_labels)
    fig.set_title('Counts of trips by passenger count')
    fig.set_xlabel('Passenger count in trips')
    fig.set_ylabel('Trip counts')
    plt.show()

**VÄLJUND**

![Sagedus reisijate arvu järgi](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/frequency-of-trips-by-passenger-count.png)

Märkmiku **Tüüp** menüü nuppude abil saate valida mitut erinevat tüüpi visualiseeringud (tabel, sektordiagramm, rea, ala või riba). Diagrammi lint on näidatud siin.


### <a name="plot-a-histogram-of-tip-amounts-and-how-tip-amount-varies-by-passenger-count-and-fare-amounts"></a>Diagrammile histogrammi näpunäite ja kuidas näpunäite summa sõltub reisijate count ja hind summad.

Kasutage SQL-päringu Näidisandmete abil.
    
    # SQL SQUERY
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

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    %matplotlib inline
    
    # TIP BY PAYMENT TYPE AND PASSENGER COUNT
    ax1 = resultsPDDF[['tip_amount']].plot(kind='hist', bins=25, facecolor='lightblue')
    ax1.set_title('Tip amount distribution')
    ax1.set_xlabel('Tip Amount ($)')
    ax1.set_ylabel('Counts')
    plt.suptitle('')
    plt.show()
    
    # TIP BY PASSENGER COUNT
    ax2 = resultsPDDF.boxplot(column=['tip_amount'], by=['passenger_count'])
    ax2.set_title('Tip amount ($) by Passenger count')
    ax2.set_xlabel('Passenger count')
    ax2.set_ylabel('Tip Amount ($)')
    plt.suptitle('')
    plt.show()
    
    # TIP AMOUNT BY FARE AMOUNT, POINTS ARE SCALED BY PASSENGER COUNT
    ax = resultsPDDF.plot(kind='scatter', x= 'fare_amount', y = 'tip_amount', c='blue', alpha = 0.10, s=5*(resultsPDDF.passenger_count))
    ax.set_title('Tip amount by Fare amount ($)')
    ax.set_xlabel('Fare Amount')
    ax.set_ylabel('Tip Amount')
    plt.axis([-2, 120, -2, 30])
    plt.show()
    

**VÄLJUND:** 

![Näpunäide jaotamine](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/tip-amount-distribution.png)

![Näpunäide summa reisijate arvu järgi](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/tip-amount-by-passenger-count.png)

![Näpunäide summa alusel hind summa](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/tip-amount-by-fare-amount.png)


## <a name="feature-engineering-transformation-and-data-preparation-for-modeling"></a>Matemaatika erifunktsioonid, teisendamiseks ja andmete modelleerimine ettevalmistamine esiletõstmine

Selles jaotises kirjeldatakse ja antakse koodi kasutatakse andmete modelleerimine ML kasutamiseks ette valmistada. See näitab, kuidas teha järgmisi toiminguid:

- Luua uus funktsioon binning tunni liikluse aja ämbrid
- Indekseerimine ja klõpsake otsetee kodeerida kategooriline funktsioonid
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

**VÄLJUND**

126050


### <a name="index-and-one-hot-encode-categorical-features"></a>Indekseerimine ja üks kuum kodeerida kategooriline funktsioonid

Selles jaotises kirjeldatakse lepingute indekseerida või kodeerida kategooriline funktsioonid sisestatud modelleerimine funktsioonid. Modelleerimine ja prognoosida MLlib funktsioonid nõuavad funktsioonide kategooriline sisendandmete indekseeritud või kodeeritud enne kasutamist. 

Mudelist, peate indekseerida või kodeerida neid erineval viisil. Näiteks logistika ja lineaarse regressioonisirge mudelite nõuda ühe kuum kodeerimine, kus, näiteks saab funktsiooni, mille kolmele laiendada kolme funktsiooni veergu, iga sisaldavad 0 või 1 sõltuvalt on jälgimine. MLlib pakub [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) funktsioon teha üks kuum kodeeringus. See kodeerija kaardid veeru silt indeksite veerule kahendarvu vektorite koos kõige ühe ühe-väärtuse. Kodeeringut võimaldab algoritmide kohta, mida oodata arvväärtuste väärtusega funktsioone, näiteks logistilise regressiooni, peaks rakenduma kategooriline funktsioone.

Siin on kood indeks ja kodeerida kategooriline funktsioonid:


    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.ml.feature import OneHotEncoder, StringIndexer, VectorAssembler, OneHotEncoder, VectorIndexer
    
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


**VÄLJUND**

Kuluv lahtri kohal: 3,14 sekundit


### <a name="create-labeled-point-objects-for-input-into-ml-functions"></a>Siltidega punkti objektide jaoks sisestatud ML funktsioonide loomine

See jaotis sisaldab koodi, mis näitab, kuidas indekseerida kategooriline tekstandmeid siltidega punkti andmetüübina ja kodeerida see, et seda saab kasutada koolitada ja testida MLlib logistilise regressiooni ja muud liigitamine mudelid. Siltidega punkti objektid on olles jaotatud andmekomplektide (RDD) nii, et on vaja sisendandmete enamik ML algoritmide kohta rakenduses MLlib vormindatud. [Sildistatud punkti](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) on kohalik vektorkuju, kas tihe või vähe, seotud sildi/vastusega.

Siin on kood indeks ja kodeerida kahendarvu liigitamine teksti funktsioone.

    # FUNCTIONS FOR BINARY CLASSIFICATION

    # LOAD LIBRARIES
    from pyspark.mllib.regression import LabeledPoint
    from numpy import array

    # INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
    def parseRowIndexingBinary(line):
        features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.pickup_hour, line.weekday,
                             line.passenger_count, line.trip_time_in_secs, line.trip_distance, line.fare_amount])
        labPt = LabeledPoint(line.tipped, features)
        return  labPt
    
    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO LOGISTIC RERESSION MODELS
    def parseRowOneHotBinary(line):
        features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
                                            line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
                                   line.vendorVec.toArray(), line.rateVec.toArray(), line.paymentVec.toArray()), axis=0)
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
    
    # SPECIFY SAMPLING AND SPLITTING FRACTIONS
    from pyspark.sql.functions import rand
    
    samplingFraction = 0.25;
    trainingFraction = 0.75; testingFraction = (1-trainingFraction);
    seed = 1234;
    encodedFinalSampled = encodedFinal.sample(False, samplingFraction, seed=seed)
    
    # SPLIT SAMPLED DATA-FRAME INTO TRAIN/TEST, WITH A RANDOM COLUMN ADDED FOR DOING CV (SHOWN LATER)
    # INCLUDE RAND COLUMN FOR CREATING CROSS-VALIDATION FOLDS
    dfTmpRand = encodedFinalSampled.select("*", rand(0).alias("rand"));
    trainData, testData = dfTmpRand.randomSplit([trainingFraction, testingFraction], seed=seed);
    
    # CACHE TRAIN AND TEST DATA
    trainData.cache()
    testData.cache()
    
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

**VÄLJUND**

Kuluv lahtri kohal: 0,31 sekundit


### <a name="feature-scaling"></a>Funktsioon mastaapimine

Funktsioon skaala tuntud ka kui andmete normaliseerimine, kindlustab, et suuresti väljastatud väärtustega funktsioonid on pole antud liigse kaaluda eesmärk funktsiooni. Funktsioon skaleerimist kood kasutab [StandardScaler](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.feature.StandardScaler) mastaapimiseks ühiku dispersioon funktsioone. See on esitatud MLlib lineaarse regressioonisirge koos Stohhastiline astmiku päritolu (SGD), populaarsed algoritmi koolitus mitmesuguseid muid seadme õppe mudelid, nt lahendada regressioon või tugiteenuste vektorkuju masinad (SVM) jaoks kasutada.   

>[AZURE.TIP] Oleme leidnud LinearRegressionWithSGD algoritmi tundlik funktsioonide skaleerimist.   

Siin on skaala muutujate regularized lineaarse SGD algoritmi kasutamiseks koodi.

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

**VÄLJUND**

Kuluv lahtri kohal: 11.67 sekundit


### <a name="cache-objects-in-memory"></a>Vahemälu objektide mälu

Aja koolitus ja testimine ML algoritmide kohta saab vähendada vahemällu sisendandmete paneeli objektide liigitamiseks regressiooni kasutatud ja mastaabitud funktsioone.

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

**VÄLJUND** 

Kuluv lahtri kohal: 0,13 sekundit


## <a name="predict-whether-or-not-a-tip-is-paid-with-binary-classification-models"></a>Prognoosida, kas Näpunäide makstakse kahendarvu liigitamine mudelid

Selles jaotises kirjeldatakse, kuidas kasutada kolme mudelid kahendarvu liigitamine tööülesande prognoosida, kas Näpunäide makstakse takso reisi. Esitatud mudelid on:

- Logistilise regressiooni 
- Juhusliku mets
- Astmikvärvi suurendada puude

Iga mudeli koostamise koodi jaotises on jaotatud järgmisteks alljaotisteks juhiseid. 

1. Üks parameeter on seatud **mudeli koolitus** andmed
2. **Mudeli hindamise** testi andmehulgas mõõdikute abil
3. **Salvestamise mudeli** tulevaste ettenähtud bloobimälu

Kuidas teha rist – valideerimise (CV) näitame parameetriga pühkimine kahel viisil:

1. **Üldise** kohandatud koodi, mida saab rakendada mis tahes algoritmi MLlib ja iga parameetri seab algoritmi. 
1. **PySpark CrossValidator müügivõimaluste funktsiooni**abil. Pange tähele, et kuigi mugav, meie kogemuse CrossValidator säde 1.5.0 mõned piirangud: 

    - Müügivõimaluste mudelite ei saa salvestada/kestnud tulevaste tarbimine.
    - Mudeli iga parameetri puhul ei saa kasutada.
    - Ei saa kasutada iga MLlib algoritmi.


### <a name="generic-cross-validation-and-hyperparameter-sweeping-used-with-the-logistic-regression-algorithm-for-binary-classification"></a>Üldine valideerimise ja hyperparameter pühkimine logistilise regressiooni algoritmi kasutatakse kahendarvu liigitamine

Selle jaotise kood näitab, kuidas koolitada, hinnata ja salvestage logistilise regressiooni mudeli prognoosib kas Näpunäide makstakse reisi NYC takso reisi ja hind andmekogumis [LBFGS](https://en.wikipedia.org/wiki/Broyden%E2%80%93Fletcher%E2%80%93Goldfarb%E2%80%93Shanno_algorithm) . Kui seate mudeli õpetatakse, kasutades valideerimine (CV) ja hyperparameter pühkimine rakendatud kohandatud koodiga, mida saab kasutada mis tahes õ algoritmide MLlib sisse.   

>[AZURE.NOTE] Kohandatud CV koodita täitmise võib kuluda mitu minutit.

**Koolitada logistilise regressiooni mudeli CV- ja hyperparameter pühkimine abil**

    # LOGISTIC REGRESSION CLASSIFICATION WITH CV AND HYPERPARAMETER SWEEPING

    # GET ACCURACY FOR HYPERPARAMETERS BASED ON CROSS-VALIDATION IN TRAINING DATA-SET

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD LIBRARIES
    from pyspark.mllib.classification import LogisticRegressionWithLBFGS 
    from pyspark.mllib.evaluation import BinaryClassificationMetrics
    
    # CREATE PARAMETER GRID FOR LOGISTIC REGRESSION PARAMETER SWEEP
    from sklearn.grid_search import ParameterGrid
    grid = [{'regParam': [0.01, 0.1], 'iterations': [5, 10], 'regType': ["l1", "l2"], 'tolerance': [1e-3, 1e-4]}]
    paramGrid = list(ParameterGrid(grid))
    numModels = len(paramGrid)
    
    # SET NUM FOLDS AND NUM PARAMETER SETS TO SWEEP ON
    nFolds = 3;
    h = 1.0 / nFolds;
    metricSum = np.zeros(numModels);
    
    # BEGIN CV WITH PARAMETER SWEEP
    for i in range(nFolds):
        # Create training and x-validation sets
        validateLB = i * h
        validateUB = (i + 1) * h
        condition = (trainData["rand"] >= validateLB) & (trainData["rand"] < validateUB)
        validation = trainData.filter(condition)
        # Create LabeledPoints from data-frames
        if i > 0:
            trainCVLabPt.unpersist()
            validationLabPt.unpersist()
        trainCV = trainData.filter(~condition)
        trainCVLabPt = trainCV.map(parseRowOneHotBinary)
        trainCVLabPt.cache()
        validationLabPt = validation.map(parseRowOneHotBinary)
        validationLabPt.cache()
        # For parameter sets compute metrics from x-validation
        for j in range(numModels):
            regt = paramGrid[j]['regType']
            regp = paramGrid[j]['regParam']
            iters = paramGrid[j]['iterations']
            tol = paramGrid[j]['tolerance']
            # Train logistic regression model with hypermarameter set
            model = LogisticRegressionWithLBFGS.train(trainCVLabPt, regType=regt, iterations=iters,  
                                                      regParam=regp, tolerance = tol, intercept=True)
            predictionAndLabels = validationLabPt.map(lambda lp: (float(model.predict(lp.features)), lp.label))
            # Use ROC-AUC as accuracy metrics
            validMetrics = BinaryClassificationMetrics(predictionAndLabels)
            metric = validMetrics.areaUnderROC
            metricSum[j] += metric
    
    avgAcc = metricSum / nFolds;
    bestParam = paramGrid[np.argmax(avgAcc)];
    
    # UNPERSIST OBJECTS
    trainCVLabPt.unpersist()
    validationLabPt.unpersist()
        
    # TRAIN ON FULL TRAIING SET USING BEST PARAMETERS FROM CV/PARAMETER SWEEP
    logitBest = LogisticRegressionWithLBFGS.train(oneHotTRAINbinary, regType=bestParam['regType'], 
                                                  iterations=bestParam['iterations'], 
                                                  regParam=bestParam['regParam'], tolerance = bestParam['tolerance'], 
                                                  intercept=True)
    
    
    # PRINT COEFFICIENTS AND INTERCEPT OF THE MODEL
    # NOTE: There are 20 coefficient terms for the 10 features, 
    #       and the different categories for features: vendorVec (2), rateVec, paymentVec (6), TrafficTimeBinsVec (4)
    print("Coefficients: " + str(logitBest.weights))
    print("Intercept: " + str(logitBest.intercept))
    
    # PRINT ELAPSED TIME    
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**VÄLJUND**

Kordajad: [0.0082065285375,-0.0223675576104,-0.0183812028036, - 3.48124578069e-05-0.00247646947233,-0.00165897881503, 0.0675394837328,-0.111823113101,-0.324609912762,-0.204549780032,-1.36499216354, 0.591088507921,-0.664263411392,-1.00439726852, 3.46567827545,-3.51025855172,-0.0471341112232,-0.043521833294, 0.000243375810385, 0.054518719222]

Intercept:-0.0111216486893

Kuluv lahtri kohal: 14.43 sekundit


**Binaarsed mudeli koos standard mõõdikute hindamine**

Selle jaotise kood näitab, kuidas hinnata logistilise regressiooni mudeli suhtes testi-andmehulgas, sh graafiku ROC kõver.


    # RECORD START TIME
    timestart = datetime.datetime.now()

    #IMPORT LIBRARIES
    from sklearn.metrics import roc_curve,auc
    from pyspark.mllib.evaluation import BinaryClassificationMetrics
    from pyspark.mllib.evaluation import MulticlassMetrics
    
    # PREDICT ON TEST DATA WITH BEST/FINAL MODEL
    predictionAndLabels = oneHotTESTbinary.map(lambda lp: (float(logitBest.predict(lp.features)), lp.label))
    
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
    
    # OUTPUT PROBABILITIES AND REGISTER TEMP TABLE
    logitBest.clearThreshold(); # This clears threshold for classification (0.5) and outputs probabilities
    predictionAndLabelsDF = predictionAndLabels.toDF()
    predictionAndLabelsDF.registerTempTable("tmp_results");

    # PRINT ELAPSED TIME    
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**VÄLJUND**

Jaotises hind ala = 0.985336538462

Ala ROC = 0.983383274312

Kokkuvõte, statistika

Täpsuse = 0.984174341679

Tagasi kutsuda = 0.984174341679

F1 Keskmine = 0.984174341679

Kuluv lahtri kohal: 2,67 sekundit


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
    
    #PREDICTIONS
    predictions_pddf = sqlResults.rename(columns={'_1': 'probability', '_2': 'label'})
    prob = predictions_pddf["probability"] 
    fpr, tpr, thresholds = roc_curve(predictions_pddf['label'], prob, pos_label=1);
    roc_auc = auc(fpr, tpr)
    
    # PLOT ROC CURVES
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
    

**VÄLJUND**

![Logistilise regressiooni ROC kõvera üldise lähenemisviisi](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/logistic-regression-roc-curve.png)


**Mudeli bloobimälu, tulevaste ettenähtud jää püsima**

Selle jaotise kood näitab, kuidas salvestada logistilise regressiooni mudeli ettenähtud.

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.classification import LogisticRegressionModel
    
    # PERSIST MODEL
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    logisticregressionfilename = "LogisticRegressionWithLBFGS_" + datestamp;
    dirfilename = modelDir + logisticregressionfilename;
    
    logitBest.save(sc, dirfilename);
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds";


**VÄLJUND**

Kuluv lahtri kohal: 34.57 sekundit


### <a name="use-mllibs-crossvalidator-pipeline-function-with-logistic-regression-elastic-regression-model"></a>MLlib's CrossValidator müügivõimaluste funktsioon logistilise regressiooni (elastne regressiooni) mudeli kasutamine

Selle jaotise kood näitab, kuidas koolitada, hinnata ja salvestage logistilise regressiooni mudeli prognoosib kas Näpunäide makstakse reisi NYC takso reisi ja hind andmekogumis [LBFGS](https://en.wikipedia.org/wiki/Broyden%E2%80%93Fletcher%E2%80%93Goldfarb%E2%80%93Shanno_algorithm) . Kui seate mudeli õpetatakse valideerimine (CV) ja hyperparameter pühkimine rakendada funktsiooni MLlib CrossValidator müügivõimaluste CV parameetri Korrasta abil.   


>[AZURE.NOTE] Järgmine kood MLlib CV rahuldamine võib kuluda mitu minutit.


    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.ml.classification import LogisticRegression
    from pyspark.ml import Pipeline
    from pyspark.ml.evaluation import BinaryClassificationEvaluator
    from pyspark.ml.tuning import CrossValidator, ParamGridBuilder
    from sklearn.metrics import roc_curve,auc
    
    # DEFINE ALGORITHM / MODEL
    lr = LogisticRegression()
    
    # DEFINE GRID PARAMETERS
    paramGrid = ParamGridBuilder().addGrid(lr.regParam, (0.01, 0.1))\
                                  .addGrid(lr.maxIter, (5, 10))\
                                  .addGrid(lr.tol, (1e-4, 1e-5))\
                                  .addGrid(lr.elasticNetParam, (0.25,0.75))\
                                  .build()
    
    # DEFINE CV WITH PARAMETER SWEEP
    cv = CrossValidator(estimator= lr,
                        estimatorParamMaps=paramGrid,
                        evaluator=BinaryClassificationEvaluator(),
                        numFolds=3)
    
    # CONVERT TO DATA-FRAME: THIS DOES NOT RUN ON RDDs
    trainDataFrame = sqlContext.createDataFrame(oneHotTRAINbinary, ["features", "label"])
    
    # TRAIN WITH CROSS-VALIDATION
    cv_model = cv.fit(trainDataFrame)
    

    ## PREDICT AND EVALUATE ON TEST DATA-SET

    # USE TEST DATASET FOR PREDICTION
    testDataFrame = sqlContext.createDataFrame(oneHotTESTbinary, ["features", "label"])
    test_predictions = cv_model.transform(testDataFrame)
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds";

**VÄLJUND**

Kuluv lahtri kohal: 107.98 sekundit


**Diagrammile ROC kõver.**

Tabeli *tmp_results*, eelmise lahtri on registreeritud *predictionAndLabelsDF* . *tmp_results* saab teha päringuid ja väljundi tulemuste sqlResults andmete raami sisse joonestamist jaoks. Siin on kood.


    # QUERY RESULTS
    %%sql -q -o sqlResults
    SELECT label, prediction, probability from tmp_results

Siin on kood diagrammile ROC kõver.

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES 
    %%local
    from sklearn.metrics import roc_curve,auc
    
    # ROC CURVE
    prob = [x["values"][1] for x in sqlResults["probability"]]
    fpr, tpr, thresholds = roc_curve(sqlResults['label'], prob, pos_label=1);
    roc_auc = auc(fpr, tpr)
    
    #PLOT
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


**VÄLJUND**

![Logistilise regressiooni ROC kõvera MLlib's CrossValidator abil](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/mllib-crossvalidator-roc-curve.png)



### <a name="random-forest-classification"></a>Juhusliku mets liigitamine

Selle jaotise kood näitab, kuidas koolitada, hinnata ja salvestamine mets juhusliku regressiooni, mis prognoosib kas Näpunäide makstakse reisi NYC takso reisi ja hind andmekogumis.


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
    ## UN-COMMENT IF YOU WANT TO PRING TREES
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


**VÄLJUND**

Ala ROC = 0.985336538462

Kuluv lahtri kohal: 26.72 sekundit


### <a name="gradient-boosting-trees-classification"></a>Astmikvärvi suurendada puude liigitamine

Selle jaotise kood kujutatakse koolitada, hinnata, ja salvestage astmiku suurendada juhul mudel, mis prognoosib kas Näpunäide makstakse reisi NYC taksosõidu rakenduses ja hind andmekomplekti.


    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel
    
    # SPECIFY NUMBER OF CATEGORIES FOR CATEGORICAL FEATURES. FEATURE #0 HAS 2 CATEGORIES, FEATURE #2 HAS 2 CATEGORIES, AND SO ON
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    
    gbtModel = GradientBoostedTrees.trainClassifier(indexedTRAINbinary, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                                    numIterations=10)
    ## UNCOMMENT IF YOU WANT TO PRINT TREE DETAILS
    #print('Learned classification GBT model:')
    #print(bgtModel.toDebugString())
    
    # PREDICT ON TEST DATA AND EVALUATE
    predictions = gbtModel.predict(indexedTESTbinary.map(lambda x: x.features))
    predictionAndLabels = indexedTESTbinary.map(lambda lp: lp.label).zip(predictions)
    
    # Area under ROC curve
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

**VÄLJUND**

Ala ROC = 0.985336538462

Kuluv lahtri kohal: 28.13 sekundit


## <a name="predict-tip-amount-with-regression-models-not-using-cv"></a>Näpunäide summa prognoosida regressiooni mudelid (mis ei kasuta CV)

Selles jaotises kirjeldatakse, kuidas kasutada kolme mudelid regressiooni ülesanne prognoosida väljaostetud takso reisi näpunäidet summa alusel näpunäite muud funktsioonid. Esitatud mudelid on:

- Lahendada lineaarregressioon
- Juhusliku mets
- Astmikvärvi suurendada puude

Need mudelid on kirjeldatud Sissejuhatus. Iga mudeli koostamise koodi jaotises on jaotatud järgmisteks alljaotisteks juhiseid. 

1. Üks parameeter on seatud **mudeli koolitus** andmed
2. **Mudeli hindamise** testi andmehulgas mõõdikute abil
3. **Salvestamise mudeli** tulevaste ettenähtud bloobimälu   


>AZURE'I Märkus: Rist-valideerimine ei kasutata kolme regressiooni mudelid selles jaotises kuna see on kuvatud üksikasjalikult logistilise regressiooni mudelid. Selles teemas liites on esitatud näide, mis näitab, kuidas kasutada CV elastne Net lineaarse regressioonisirge.


>AZURE'I Märkus: Meie kogemusi, ei saa olla probleemid lähenemise LinearRegressionWithSGD mudeleid ja parameetrid peavad olema muudetud/optimeeritud hoolikalt saamiseks kehtiv mudel. Lähenemise skaleerimist märkimisväärselt muutujate abil. Elastne net regressiooni, liites teema, saate kasutada ka LinearRegressionWithSGD asemel.


### <a name="linear-regression-with-sgd"></a>Koos SGD lineaarregressioon

Selle jaotise kood kuvatakse lineaarse regressioonisirge, mis kasutab Stohhastiline astmiku päritolu (SGD) optimeerimine koolitada mastaabitud funktsioonide kasutamise kohta ja kuidas Keskmine, hinnata ja salvestage mudel Azure'i bloobimälu salvestusruumi (WASB).

>[AZURE.TIP] Meie kogemus, ei saa olla probleemid LinearRegressionWithSGD mudelite lähenemise ja parameetrid peavad olema muudetud/optimeeritud hoolikalt saamiseks kehtiv mudel. Lähenemise skaleerimist märkimisväärselt muutujate abil.


    # LINEAR REGRESSION WITH SGD 

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
    
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)
    
    # SAVE MODEL IN BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    linearregressionfilename = "LinearRegressionWithSGD_" + datestamp;
    dirfilename = modelDir + linearregressionfilename;
    
    linearModel.save(sc, dirfilename)
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

**VÄLJUND**

Kordajad: [0.0141707753435,-0.0252930927087,-0.0231442517137, 0.247070902996, 0.312544147152, 0.360296120645, 0.0122079566092,-0.00456498588241,-0.0898228505177, 0.0714046248793, 0.102171263868, 0.100022455632,-0.00289545676449,-0.00791124681938, 0.54396316518,-0.536293513569, 0.0119076553369,-0.0173039244582, 0.0119632796147, 0.00146764882502]

Intercept: 0.854507624459

RMSE = 1.23485131376

R-sqr = 0.597963951127

Kuluv lahtri kohal: 38.62 sekundit


### <a name="random-forest-regression"></a>Juhuslik mets regressiooni

Selle jaotise kood näitab, kuidas koolitada, hinnata ja juhusliku mets mudelit, mis prognoosib näpunäite summa NYC takso reisi andmed salvestada.   

>[AZURE.NOTE] Rist – valideerimise parameetriga pühkimine kohandatud koodi on esitatud liites.


    #PREDICT TIP AMOUNTS USING RANDOM FOREST

    # RECORD START TIME
    timestart= datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import RandomForest, RandomForestModel
    from pyspark.mllib.util import MLUtils
    from pyspark.mllib.evaluation import RegressionMetrics
    
    
    # TRAIN MODEL
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    rfModel = RandomForest.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                        numTrees=25, featureSubsetStrategy="auto",
                                        impurity='variance', maxDepth=10, maxBins=32)
    # UN-COMMENT IF YOU WANT TO PRING TREES
    #print('Learned classification forest model:')
    #print(rfModel.toDebugString())
    
    # PREDICT AND EVALUATE ON TEST DATA-SET
    predictions = rfModel.predict(indexedTESTreg.map(lambda x: x.features))
    predictionAndLabels = oneHotTESTreg.map(lambda lp: lp.label).zip(predictions)
    
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

**VÄLJUND**

RMSE = 0.931981967875

R-sqr = 0.733445485802

Kuluv lahtri kohal: 25.98 sekundit


### <a name="gradient-boosting-trees-regression"></a>Astmikvärvi suurendada puude regressiooni

Selle jaotise kood näitab, kuidas koolitada, hinnata ja astmiku suurendada puude mudelit, mis prognoosib näpunäite summa NYC takso reisi andmed salvestada.

**Koolitada ja hindamine**

    #PREDICT TIP AMOUNTS USING GRADIENT BOOSTING TREES

    # RECORD START TIME
    timestart= datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel
    from pyspark.mllib.util import MLUtils
    
    # TRAIN MODEL
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    gbtModel = GradientBoostedTrees.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo, 
                                                    numIterations=10, maxBins=32, maxDepth = 4, learningRate=0.1)
    
    # EVALUATE A TEST DATA-SET
    predictions = gbtModel.predict(indexedTESTreg.map(lambda x: x.features))
    predictionAndLabels = indexedTESTreg.map(lambda lp: lp.label).zip(predictions)
    
    testMetrics = RegressionMetrics(predictionAndLabels)
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)
    
    # PLOT SCATTER-PLOT BETWEEN ACTUAL AND PREDICTED TIP VALUES
    test_predictions= sqlContext.createDataFrame(predictionAndLabels)
    test_predictions_pddf = test_predictions.toPandas()
    
    # SAVE MODEL IN BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btregressionfilename = "GradientBoostingTreeRegression_" + datestamp;
    dirfilename = modelDir + btregressionfilename;
    gbtModel.save(sc, dirfilename)
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**VÄLJUND**

RMSE = 0.928172197114

R-sqr = 0.732680354389

Kuluv lahtri kohal: 20.9 sekundit


**Diagrammile**
    
*tmp_results* on registreeritud taru tabeli eelmisesse lahtrisse. Tulemuste tabelis on väljundi *sqlResults* andmete-raami jaoks joonestamist. Siin on kood

    # PLOT SCATTER-PLOT BETWEEN ACTUAL AND PREDICTED TIP VALUES

    # SELECT RESULTS
    %%sql -q -o sqlResults
    SELECT * from tmp_results


Siin on Jupyter serveri kasutamine andmete joonestamiseks kood.

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    import numpy as np
    
    # PLOT
    ax = sqlResults.plot(kind='scatter', figsize = (6,6), x='_1', y='_2', color='blue', alpha = 0.25, label='Actual vs. predicted');
    fit = np.polyfit(sqlResults['_1'], sqlResults['_2'], deg=1)
    ax.set_title('Actual vs. Predicted Tip Amounts ($)')
    ax.set_xlabel("Actual")
    ax.set_ylabel("Predicted")
    ax.plot(sqlResults['_1'], fit[0] * sqlResults['_1'] + fit[1], color='magenta')
    plt.axis([-1, 15, -1, 15])
    plt.show(ax)

![Tegelik-vs-ennustatud-Näpunäide-summad](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/actual-vs-predicted-tips.png)


## <a name="appendix-additional-regression-tasks-using-cross-validation-with-parameter-sweeps"></a>Lisa: Täiendavad regressiooni tööülesannete parameetri kontrolle rist valideerimise abil

See lisa sisaldab koodi, mis näitab, kuidas CV lineaarse regressioonisirge elastne net kasutamise ja kuidas seda teha CV kohandatud koodi kasutamise juhusliku mets regressiooni parameetri kaustadesse teisaldada.


### <a name="cross-validation-using-elastic-net-for-linear-regression"></a>Rist valideerimise abil elastne net lineaarregressioon

Selle jaotise kood näitab, kuidas ületada valideerimise abil elastne net lineaarse regressioonisirge ja kuidas hinnata mudeli testi andmetega.

    ###  CV USING ELASTIC NET FOR LINEAR REGRESSION

    # RECORD START TIME
    timestart = datetime.datetime.now()
    
    # LOAD PYSPARK LIBRARIES
    from pyspark.ml.regression import LinearRegression
    from pyspark.ml import Pipeline
    from pyspark.ml.evaluation import RegressionEvaluator
    from pyspark.ml.tuning import CrossValidator, ParamGridBuilder
    
    # DEFINE ALGORITHM/MODEL
    lr = LinearRegression()
    
    # DEFINE GRID PARAMETERS
    paramGrid = ParamGridBuilder().addGrid(lr.regParam, (0.01, 0.1))\
                                  .addGrid(lr.maxIter, (5, 10))\
                                  .addGrid(lr.tol, (1e-4, 1e-5))\
                                  .addGrid(lr.elasticNetParam, (0.25,0.75))\
                                  .build() 
    
    # DEFINE PIPELINE 
    # SIMPLY THE MODEL HERE, WITHOUT TRANSFORMATIONS
    pipeline = Pipeline(stages=[lr])
    
    # DEFINE CV WITH PARAMETER SWEEP
    cv = CrossValidator(estimator= lr,
                        estimatorParamMaps=paramGrid,
                        evaluator=RegressionEvaluator(),
                        numFolds=3)
    
    # CONVERT TO DATA FRAME, AS CROSSVALIDATOR WON'T RUN ON RDDS
    trainDataFrame = sqlContext.createDataFrame(oneHotTRAINreg, ["features", "label"])
    
    # TRAIN WITH CROSS-VALIDATION
    cv_model = cv.fit(trainDataFrame)
    

    # EVALUATE MODEL ON TEST SET
    testDataFrame = sqlContext.createDataFrame(oneHotTESTreg, ["features", "label"])
    
    # MAKE PREDICTIONS ON TEST DOCUMENTS
    # cvModel uses the best model found (lrModel).
    predictionAndLabels = cv_model.transform(testDataFrame)
    
    # CONVERT TO DF AND SAVE REGISER DF AS TABLE
    predictionAndLabels.registerTempTable("tmp_results");
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**VÄLJUND**

Kuluv lahtri kohal: 161.21 sekundit

**R-SQR mõõdiku koos hindamine**

*tmp_results* on registreeritud taru tabeli eelmisesse lahtrisse. Tulemuste tabelis on väljundi *sqlResults* andmete-raami jaoks joonestamist. Siin on kood

    # SELECT RESULTS
    %%sql -q -o sqlResults
    SELECT label,prediction from tmp_results


Siin on kood R-sqr arvutamiseks.

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    from scipy import stats
    
    #R-SQR TEST METRIC
    corstats = stats.linregress(sqlResults['label'],sqlResults['prediction'])
    r2 = (corstats[2]*corstats[2])
    print("R-sqr = %s" % r2)


**VÄLJUND**

R-sqr = 0.619184907088


### <a name="cross-validation-with-parameter-sweep-using-custom-code-for-random-forest-regression"></a>Risti valideerimine kohandatud koodi kasutamise juhusliku mets regressiooni parameetri korrastamine

Selle jaotise kood kuvatakse risti valideerimine parameetri Korrasta juhusliku mets regressiooni kohandatud koodi kasutamise kohta ja kuidas hinnata mudeli testi andmetega.


    # RECORD START TIME
    timestart= datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    # GET ACCURARY FOR HYPERPARAMETERS BASED ON CROSS-VALIDATION IN TRAINING DATA-SET
    from pyspark.mllib.tree import RandomForest, RandomForestModel
    from pyspark.mllib.util import MLUtils
    from pyspark.mllib.evaluation import RegressionMetrics
    from sklearn.grid_search import ParameterGrid
    
    ## CREATE PARAMETER GRID
    grid = [{'maxDepth': [5,10], 'numTrees': [25,50]}]
    paramGrid = list(ParameterGrid(grid))
    
    ## SPECIFY LEVELS OF CATEGORICAL VARIBLES
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    
    # SPECIFY NUMFOLDS AND ARRAY TO HOLD METRICS
    nFolds = 3;
    numModels = len(paramGrid)
    h = 1.0 / nFolds;
    metricSum = np.zeros(numModels);
    
    for i in range(nFolds):
        # Create training and x-validation sets
        validateLB = i * h
        validateUB = (i + 1) * h
        condition = (trainData["rand"] >= validateLB) & (trainData["rand"] < validateUB)
        validation = trainData.filter(condition)
        # Create labeled points from data-frames
        if i > 0:
            trainCVLabPt.unpersist()
            validationLabPt.unpersist()
        trainCV = trainData.filter(~condition)
        trainCVLabPt = trainCV.map(parseRowIndexingRegression)
        trainCVLabPt.cache()
        validationLabPt = validation.map(parseRowIndexingRegression)
        validationLabPt.cache()
        # For parameter sets compute metrics from x-validation
        for j in range(numModels):
            maxD = paramGrid[j]['maxDepth']
            numT = paramGrid[j]['numTrees']
            # Train logistic regression model with hypermarameter set
            rfModel = RandomForest.trainRegressor(trainCVLabPt, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                        numTrees=numT, featureSubsetStrategy="auto",
                                        impurity='variance', maxDepth=maxD, maxBins=32)
            predictions = rfModel.predict(validationLabPt.map(lambda x: x.features))
            predictionAndLabels = validationLabPt.map(lambda lp: lp.label).zip(predictions)
            # Use ROC-AUC as accuracy metrics
            validMetrics = RegressionMetrics(predictionAndLabels)
            metric = validMetrics.rootMeanSquaredError
            metricSum[j] += metric
    
    avgAcc = metricSum/nFolds;
    bestParam = paramGrid[np.argmin(avgAcc)];
    
    # UNPERSIST OBJECTS
    trainCVLabPt.unpersist()
    validationLabPt.unpersist()
            
    ## TRAIN FINAL MODL WIHT BEST PARAMETERS
    rfModel = RandomForest.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                        numTrees=bestParam['numTrees'], featureSubsetStrategy="auto",
                                        impurity='variance', maxDepth=bestParam['maxDepth'], maxBins=32)

    # EVALUATE MODEL ON TEST DATA
    predictions = rfModel.predict(indexedTESTreg.map(lambda x: x.features))
    predictionAndLabels = indexedTESTreg.map(lambda lp: lp.label).zip(predictions)
    
    #PRINT TEST METRICS
    testMetrics = RegressionMetrics(predictionAndLabels)
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)
    
    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


**VÄLJUND**

RMSE = 0.906972198262

R-sqr = 0.740751197012

Kuluv lahtri kohal: 69.17 sekundit


### <a name="clean-up-objects-from-memory-and-print-model-locations"></a>Objektide mälu ja Prindi mudeli asukohtadest puhastamiseks

Kasutage `unpersist()` vahemälus objektide kustutamine.

    # UNPERSIST OBJECTS CACHED IN MEMORY

    # REMOVE ORIGINAL DFs
    taxi_df_train_cleaned.unpersist()
    taxi_df_train_with_newFeatures.unpersist()
    trainData.unpersist()
    trainData.unpersist()
    
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


**VÄLJUND**

Veebisaidil RDD veebisaidil PythonRDD.scala PythonRDD [122]: 43


**Tarbimine märkmikus kasutatavat mudeli faili väljaprint tee.** Tarbimine ja sõltumatu andmehulga Keskmine, peate kopeerida ja kleepida need failinimedes märkmikus,"tarbimine".


    # PRINT MODEL FILE LOCATIONS FOR CONSUMPTION
    print "logisticRegFileLoc = modelDir + \"" + logisticregressionfilename + "\"";
    print "linearRegFileLoc = modelDir + \"" + linearregressionfilename + "\"";
    print "randomForestClassificationFileLoc = modelDir + \"" + rfclassificationfilename + "\"";
    print "randomForestRegFileLoc = modelDir + \"" + rfregressionfilename + "\"";
    print "BoostedTreeClassificationFileLoc = modelDir + \"" + btclassificationfilename + "\"";
    print "BoostedTreeRegressionFileLoc = modelDir + \"" + btregressionfilename + "\"";


**VÄLJUND**

logisticRegFileLoc = modelDir + "LogisticRegressionWithLBFGS_2016-05-0316_47_30.096528"

linearRegFileLoc = modelDir + "LinearRegressionWithSGD_2016-05-0316_51_28.433670"

randomForestClassificationFileLoc = modelDir + "RandomForestClassification_2016-05-0316_50_17.454440"

randomForestRegFileLoc = modelDir + "RandomForestRegression_2016-05-0316_51_57.331730"

BoostedTreeClassificationFileLoc = modelDir + "GradientBoostingTreeClassification_2016-05-0316_50_40.138809"

BoostedTreeRegressionFileLoc = modelDir + "GradientBoostingTreeRegression_2016-05-0316_52_18.827237"

## <a name="whats-next"></a>Mis saab edasi?

Nüüd, kui olete loonud regressiooni ja liigitamine mudelite säde MlLib, olete valmis, saate teada, kuidas Keskmine ja nende mudelite hinnata.

**Modelleerimise tarbimine:** Keskmine ja hindate liigitamine ja regressiooni mudelid luuakse selle teema kohta leiate teavet teemast [Keskmine ja hindate säde ehitatud masina õppe mudelid](machine-learning-data-science-spark-model-consumption.md).
