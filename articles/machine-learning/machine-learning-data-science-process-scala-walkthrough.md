<properties
    pageTitle="Azure'i Scala ja säde kasutamine andmete teadus | Microsoft Azure'i"
    description="Kuidas kasutada Scala kuuluva masina õ ülesannete säde scalable MLlib ja säde ML paketid on Azure Hdinsightiga säde kobar kohta."  
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
    ms.date="08/01/2016"
    ms.author="bradsev;deguhath"/>


# <a name="data-science-using-scala-and-spark-on-azure"></a>Andmete teadus Azure'i Scala ja säde kasutamine

Selles artiklis kirjeldatakse, kuidas Scala kasutamiseks kuuluva masina õ ülesannete säde scalable MLlib ja säde ML paketid on Azure Hdinsightiga säde kobar kohta. See juhendab teid ülesanded, mida kujutavad endast [andmete teadus protsessi](http://aka.ms/datascienceprocess): andmete manustamisest ja avastamine, visualiseerimine, funktsioon matemaatika, modelleerimine ja mudeli tarbimine. Mudelite artiklist järgmised logistika ja lineaarse regressioonisirge, juhusliku metsade ja astmik võimendada puude (GBTs), lisaks kaks kuuluva masina õ põhitoimingud.

- Regressioonisirge probleem: prognoosida takso reisi näpunäite summa ($)
- Binaarsed liigitamine: prognoosida näpunäite või pole takso reisi Näpunäide (1/0)

Protsessi modelleerimine nõuab koolitus ja hindamise testi andmehulga ja oluline täpsuse mõõdikute. Selles artiklis saate teada, kuidas salvestada need mudelid Azure'i bloobimälu ja Keskmine ja hindate sõnastikupõhise töötamise kohta. See artikkel sisaldab ka täpsemate teemade, kuidas optimeerida mudelite rist – valideerimise ja hyper-parameeter pühkimine abil. Kasutatud andmed on valimi andmehulga 2013 NYC takso reisi ja hind github saadaval.

[Scala](http://www.scala-lang.org/), Java virtuaalse masina, põhineb keel ühendab objekti rakendusse ja funktsionaalne keele mõisted. See on scalable keel, mis sobib hästi jaotatud töötlemine pilves ja Azure säde kogumite töötab.

[Spark](http://spark.apache.org/) on avatud lähtekoodi samal ajal töötlemise raamistiku, mis toetab-mälu töötlemine big data analytics rakenduste jõudlust parandada. Säde töötlemine engine on loodud kiiruse hõlpsamaks kasutamine ja keeruka analüüsi jaoks. Säde tema mälu-jaotatud arvutus võimaluste oleks hea valik iteratiivne algoritmide masina õppimine ja Graphi arvutuste. [Spark.ml](http://spark.apache.org/docs/latest/ml-guide.html) pakett sisaldab ühtsed üksikasjalik API-de ehitatud andmete raamid, mille abil saate luua ja Õppekeskuse torujuhtmete praktiline seadme häälestamine. [MLlib](http://spark.apache.org/mllib/) on säde 's scalable masina õ teek, mis toob selle jaotatud keskkonna modelleerimise võimalusi.

[Hdinsightiga säde](../hdinsight/hdinsight-apache-spark-overview.md) on avatud lähtekoodi säde Azure'i majutatud pakkumine. Lisaks sisaldab tugi Jupyter Scala märkmike säde klaster ja käitamise säde SQL-i interaktiivne päringute muuta, filtreerimine ja visualiseerida andmeid, mis on talletatud Azure'i bloobimälu. Selle Scala Koodilõigud selle artikli mis pakuvad lahenduste ja kuvada nende andmete visualiseerimiseks oluline proovitükid käivitada Jupyter märkmike installitud säde rühmad. Modelleerimine juhised järgmiste teemade on kood, mis näitab, kuidas koolitada, hinnata, salvestamine ja tarbimine igat tüüpi mudel.

Häälestamise juhised ja selle artikli kood on Azure Hdinsightiga 3.4 säde 1,6. Siiski koodi käesolevas artiklis ja [Scala Jupyter märkmiku](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/Scala/Exploration%20Modeling%20and%20Scoring%20using%20Scala.ipynb) üldist ja peaks töötama mis tahes säde kobar. Kobar häälestamine ja haldamine juhiseid võib olla veidi teistsugune, mis on näidatud selles artiklis, kui te ei kasuta Hdinsightiga säde.

> [AZURE.NOTE] Teema, mis näitab, kuidas kasutada Python asemel Scala andmete teadus-lõpuni protsessi toimingute tegemiseks, leiate teemast [Andmete teadus kasutamise kohta Windows Azure Hdinsightiga säde](machine-learning-data-science-spark-overview.md).


## <a name="prerequisites"></a>Eeltingimused

-   Azure'i tellimuse peab teil olema. Kui te ei ole veel üks [on Azure tasuta prooviversiooni](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

-   Peate mõne Azure Hdinsightiga 3.4 säde 1,6 kobar lõpuleviimiseks järgmistest toimingutest. Klaster loomiseks vaadake teemat juhiseid [Alustamine: loomine Apache Spark Windows Azure Hdinsightiga](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md). Määrake menüüs **Kobar tüübi valimine** kobar tüüp ja versioon.

![Hdinsightiga kobar tüüp konfigureerimine](./media/machine-learning-data-science-process-scala-walkthrough/spark-cluster-on-portal.png)


>[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


NYC takso reisi andmed ja juhised, kuidas käivitada koodi Jupyter märkmiku säde klaster kirjeldust vt [Ülevaade andmete](machine-learning-data-science-spark-overview.md)Science kasutamise kohta Windows Azure Hdinsightiga säde jaotised.  


## <a name="execute-scala-code-from-a-jupyter-notebook-on-the-spark-cluster"></a>Käivitada Scala kood Jupyter märkmiku säde klaster

Saate käivitada Jupyter märkmiku Azure portaalist. Säde kobar armatuurlauale otsimine ja klõpsake selle lehe management sisestamiseks klaster. Järgmiseks klõpsake **Kobar armatuurlaudade**ja **Jupyter märkmiku** säde kobar-ga seostatud märkmiku avamiseks klõpsake.

![Kobar armatuurlaua ja Jupyter märkmikud](./media/machine-learning-data-science-process-scala-walkthrough/spark-jupyter-on-portal.png)

Lisaks pääsete juurde Jupyter märkmike aadressil https://&lt;clustername&gt;.azurehdinsight.net/jupyter. Asendage *clustername* klaster nime. Peate Jupyter märkmikele juurdepääsuks oma administraatori konto parool.

![Minge Jupyter märkmike kobar nime abil](./media/machine-learning-data-science-process-scala-walkthrough/spark-jupyter-notebook.png)

Valige **Scala** kataloogi, mis on esitatud mõned näited müügipakendis märkmikke, mis kasutavad PySpark API kuvamiseks. Avastamine modelleerimine ja Scoring Scala.ipynb märkmikku, mis sisaldab koodinäiteid selle komplekti säde teemade abil on saadaval [github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/Scala).


Märkmiku otse GitHub Jupyter märkmiku serveris saate üles laadida klaster säde kohta. Jupyter avalehel nuppu **üles laadida** . File Exploreris kleepige Scala märkmiku GitHub (töötlemata sisu) URL ja klõpsake siis nuppu **Ava**. Scala märkmik on saadaval järgmine URL:

[Exploration-Modeling-and-Scoring-using-Scala.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/Scala/Exploration-Modeling-and-Scoring-using-Scala.ipynb)

## <a name="setup-preset-spark-and-hive-contexts-spark-magics-and-spark-libraries"></a>Install: Valmissäte säde ja taru konteksti, säde magics ja säde teegid

### <a name="preset-spark-and-hive-contexts"></a>Algne säde ja taru kontekste

    # SET THE START TIME
    import java.util.Calendar
    val beginningTime = Calendar.getInstance().getTime()


Säde tuumad, mis on varustatud Jupyter märkmikud on algne kontekstides. Te ei pea konkreetselt määratud säde või taru kontekstides enne alustamist töötamine rakenduse arenevad. Algne kontekstides on:

- `sc`SparkContext jaoks
- `sqlContext`HiveContext jaoks


### <a name="spark-magics"></a>Säde magics

Säde tuum pakub mõni eelmääratletud "maagia", mis on erikäsud, mille abil saate helistada `%%`. Need käsud kaks kasutatakse koodi näidised.

- `%%local`Saate määrata, et read koodi käivitatakse kohalikult. Koodi peab olema lubatud Scala kood.
- `%%sql -o <variable name>`käivitab päringu taru vastu `sqlContext`. Kui soovitud `-o` parameeter on möödas, päringu tulem on säilis soovitud `%%local` säde andmete raami Scala kontekstis.

Jaoks Lisateavet tuumad Jupyter märkmike ja nende eelmääratletud "maagia" mida te helistada `%%` (näiteks `%%local`), vt [tuumad Jupyter märkmikke Hdinsightiga säde Linux kogumite kohta Hdinsightiga jaoks saadaval](../hdinsight/hdinsight-apache-spark-jupyter-notebook-kernels.md).


### <a name="import-libraries"></a>Teekide importimine

Importida säde, MLlib ja muud teegid, peate, kasutades järgmist.

    # IMPORT SPARK AND JAVA LIBRARIES
    import org.apache.spark.sql.SQLContext
    import org.apache.spark.sql.functions._
    import java.text.SimpleDateFormat
    import java.util.Calendar
    import sqlContext.implicits._
    import org.apache.spark.sql.Row

    # IMPORT SPARK SQL FUNCTIONS
    import org.apache.spark.sql.types.{StructType, StructField, StringType, IntegerType, FloatType, DoubleType}
    import org.apache.spark.sql.functions.rand

    # IMPORT SPARK ML FUNCTIONS
    import org.apache.spark.ml.Pipeline
    import org.apache.spark.ml.feature.{StringIndexer, VectorAssembler, OneHotEncoder, VectorIndexer, Binarizer}
    import org.apache.spark.ml.tuning.{ParamGridBuilder, TrainValidationSplit, CrossValidator}
    import org.apache.spark.ml.regression.{LinearRegression, LinearRegressionModel, RandomForestRegressor, RandomForestRegressionModel, GBTRegressor, GBTRegressionModel}
    import org.apache.spark.ml.classification.{LogisticRegression, LogisticRegressionModel, RandomForestClassifier, RandomForestClassificationModel, GBTClassifier, GBTClassificationModel}
    import org.apache.spark.ml.evaluation.{BinaryClassificationEvaluator, RegressionEvaluator, MulticlassClassificationEvaluator}

    # IMPORT SPARK MLLIB FUNCTIONS
    import org.apache.spark.mllib.linalg.{Vector, Vectors}
    import org.apache.spark.mllib.util.MLUtils
    import org.apache.spark.mllib.classification.{LogisticRegressionWithLBFGS, LogisticRegressionModel}
    import org.apache.spark.mllib.regression.{LabeledPoint, LinearRegressionWithSGD, LinearRegressionModel}
    import org.apache.spark.mllib.tree.{GradientBoostedTrees, RandomForest}
    import org.apache.spark.mllib.tree.configuration.BoostingStrategy
    import org.apache.spark.mllib.tree.model.{GradientBoostedTreesModel, RandomForestModel, Predict}
    import org.apache.spark.mllib.evaluation.{BinaryClassificationMetrics, MulticlassMetrics, RegressionMetrics}

    # SPECIFY SQLCONTEXT
    val sqlContext = new SQLContext(sc)


## <a name="data-ingestion"></a>Andmete manustamisest

Andmete teadus protsessi esimene samm on neelata andmed, mida soovite analüüsida. Saate tuua andmeid välistest allikatest ja süsteemide, kus see asub andmete uurimine ja modelleerimine keskkonnas. Selles artiklis on andmeid, mida neelata ühendatud 0,1% valim aia takso reisi ja hind faili (TSV failina salvestatud). Andmete uurimine ja modelleerimine keskkond on säde. See jaotis sisaldab järgmist sarja tööülesannete lõpuleviimiseks kood:

1. Määrake andmed ja mudel Storage directory teed.
2. Lugege Sisestuskeel andmete määramine (TSV failina salvestatud).
3. Andmete jaoks skeemi määratlemine ja andmete puhastamiseks.
4. Raami puhastada andmete loomine ja selle mälu vahemälu.
5. Registreerida andmed SQLContext ajutine tabelina.
6. Päringu tabeli ja importida andmed raami tulemused.


### <a name="set-directory-paths-for-storage-locations-in-azure-blob-storage"></a>Azure'i bloobimälu directory teed salvestusruumi asukohtade määramine

Säde saate lugeda ja kirjutada Azure'i bloobimälu. Saate kasutada mis tahes olemasolevate andmete töötlemiseks säde ja siis talletage uuesti bloobimälu.

Salvestamiseks bloobimälu mudelite või faile, peate määrama õigesti tee. Viide lisatud säde kobar abil tee, mis algab vaikimisi ümbris `wasb:///`. Viide muudesse asukohtadesse, kasutades `wasb://`.

Järgmine kood näidis saate määrata asukoha sisendandmete lugeda ja tee on seotud säde kobar bloobimälu kui mudel ei salvestata.

    # SET PATHS TO DATA AND MODEL FILE LOCATIONS
    # INGEST DATA AND SPECIFY HEADERS FOR COLUMNS
    val taxi_train_file = sc.textFile("wasb://mllibwalkthroughs@cdspsparksamples.blob.core.windows.net/Data/NYCTaxi/JoinedTaxiTripFare.Point1Pct.Train.tsv")
    val header = taxi_train_file.first;

    # SET THE MODEL STORAGE DIRECTORY PATH
    # NOTE THAT THE FINAL BACKSLASH IN THE PATH IS REQUIRED.
    val modelDir = "wasb:///user/remoteuser/NYCTaxi/Models/";


### <a name="import-data-create-an-rdd-and-define-a-data-frame-according-to-the-schema"></a>Andmete importimine, mõne RDD loomine ja andmete raami skeemi järgi määratlemine

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # DEFINE THE SCHEMA BASED ON THE HEADER OF THE FILE
    val sqlContext = new SQLContext(sc)
    val taxi_schema = StructType(
        Array(
            StructField("medallion", StringType, true),
            StructField("hack_license", StringType, true),
            StructField("vendor_id", StringType, true),
            StructField("rate_code", DoubleType, true),
            StructField("store_and_fwd_flag", StringType, true),
            StructField("pickup_datetime", StringType, true),
            StructField("dropoff_datetime", StringType, true),
            StructField("pickup_hour", DoubleType, true),
            StructField("pickup_week", DoubleType, true),
            StructField("weekday", DoubleType, true),
            StructField("passenger_count", DoubleType, true),
            StructField("trip_time_in_secs", DoubleType, true),
            StructField("trip_distance", DoubleType, true),
            StructField("pickup_longitude", DoubleType, true),
            StructField("pickup_latitude", DoubleType, true),
            StructField("dropoff_longitude", DoubleType, true),
            StructField("dropoff_latitude", DoubleType, true),
            StructField("direct_distance", StringType, true),
            StructField("payment_type", StringType, true),
            StructField("fare_amount", DoubleType, true),
            StructField("surcharge", DoubleType, true),
            StructField("mta_tax", DoubleType, true),
            StructField("tip_amount", DoubleType, true),
            StructField("tolls_amount", DoubleType, true),
            StructField("total_amount", DoubleType, true),
            StructField("tipped", DoubleType, true),
            StructField("tip_class", DoubleType, true)
            )
        )

    # CAST VARIABLES ACCORDING TO THE SCHEMA
    val taxi_temp = (taxi_train_file.map(_.split("\t"))
                            .filter((r) => r(0) != "medallion")
                            .map(p => Row(p(0), p(1), p(2),
                                p(3).toDouble, p(4), p(5), p(6), p(7).toDouble, p(8).toDouble, p(9).toDouble, p(10).toDouble,
                                p(11).toDouble, p(12).toDouble, p(13).toDouble, p(14).toDouble, p(15).toDouble, p(16).toDouble,
                                p(17), p(18), p(19).toDouble, p(20).toDouble, p(21).toDouble, p(22).toDouble,
                                p(23).toDouble, p(24).toDouble, p(25).toDouble, p(26).toDouble)))


    # CREATE AN INITIAL DATA FRAME AND DROP COLUMNS, AND THEN CREATE A CLEANED DATA FRAME BY FILTERING FOR UNWANTED VALUES OR OUTLIERS
    val taxi_train_df = sqlContext.createDataFrame(taxi_temp, taxi_schema)

    val taxi_df_train_cleaned = (taxi_train_df.drop(taxi_train_df.col("medallion"))
            .drop(taxi_train_df.col("hack_license")).drop(taxi_train_df.col("store_and_fwd_flag"))
            .drop(taxi_train_df.col("pickup_datetime")).drop(taxi_train_df.col("dropoff_datetime"))
            .drop(taxi_train_df.col("pickup_longitude")).drop(taxi_train_df.col("pickup_latitude"))
            .drop(taxi_train_df.col("dropoff_longitude")).drop(taxi_train_df.col("dropoff_latitude"))
            .drop(taxi_train_df.col("surcharge")).drop(taxi_train_df.col("mta_tax"))
            .drop(taxi_train_df.col("direct_distance")).drop(taxi_train_df.col("tolls_amount"))
            .drop(taxi_train_df.col("total_amount")).drop(taxi_train_df.col("tip_class"))
            .filter("passenger_count > 0 and passenger_count < 8 AND payment_type in ('CSH', 'CRD') AND tip_amount >= 0 AND tip_amount < 30 AND fare_amount >= 1 AND fare_amount < 150 AND trip_distance > 0 AND trip_distance < 100 AND trip_time_in_secs > 30 AND trip_time_in_secs < 7200"));

    # CACHE AND MATERIALIZE THE CLEANED DATA FRAME IN MEMORY
    taxi_df_train_cleaned.cache()
    taxi_df_train_cleaned.count()

    # REGISTER THE DATA FRAME AS A TEMPORARY TABLE IN SQLCONTEXT
    taxi_df_train_cleaned.registerTempTable("taxi_train")

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**Väljund:**

Lahtri aega: 8 sekundit.


### <a name="query-the-table-and-import-results-in-a-data-frame"></a>Päringu tabeli ja tulemuste raamis andmete importimine

Järgmiseks päringu tabeli hind, ja näpunäite andmeid; filtreerida vigane ja ühel andmeid; ja printida mitu rida.

    # QUERY THE DATA
    val sqlStatement = """
        SELECT fare_amount, passenger_count, tip_amount, tipped
        FROM taxi_train
        WHERE passenger_count > 0 AND passenger_count < 7
        AND fare_amount > 0 AND fare_amount < 200
        AND payment_type in ('CSH', 'CRD')
        AND tip_amount > 0 AND tip_amount < 25
    """
    val sqlResultsDF = sqlContext.sql(sqlStatement)

    # SHOW ONLY THE TOP THREE ROWS
    sqlResultsDF.show(3)

**Väljund:**

fare_amount|passenger_count|tip_amount|otsaga
-----------|---------------|----------|------
       13,5|            1.0|       2.9|   1.0
       16,0|            2.0|       3.4|   1.0
       10.5|            2.0|       1.0|   1.0


## <a name="data-exploration-and-visualization"></a>Andmete uurimine ja visualiseerimine

Pärast andmete toomiseks säde järgmiseks andmete teadus protsessi on põhjalikult tundma andmete uurimine ja visualiseerimine kaudu. Selles jaotises saate läbi takso andmed, kasutades SQL-päringud. Importige tulemused andmete raami diagrammile target muutujate ja tulevaste funktsioonid visuaalselt Jupyter automaatne visualiseerimine funktsiooniga.

### <a name="use-local-and-sql-magic-to-plot-data"></a>Andmete kandmine kohalik ja maagiline SQL-i abil

Vaikimisi, kui käivitate Jupyter märkmiku mis tahes koodilõigu väljund on seanss, mis on kestnud töötaja sõlmed raames saadaval. Kui soovite salvestada reisi arvutamiseks iga töötaja sõlmi, ja kui kõik andmed, et peate oma arvutamiseks on saadaval kohalikult sõlme Jupyter server (mis ei pea sõlme), saate selle `%%local` maagiline käivitamiseks koodilõigu Jupyter serveris.

- **SQL-i maagiline** (`%%sql`). Hdinsightiga säde tuum toetab lihtne Tekstisisese HiveQL päringute SQLContext vastu. Funktsiooni (`-o VARIABLE_NAME`) argument püsib SQL-päringu väljund Pandas andmete raami Jupyter serveris. See tähendab, et see on taas kättesaadav kohalikus režiimis.
- `%%local`**maagiline**. Funktsiooni `%%local` maagiline töötab koodi kohalikult Jupyter server, mis on Hdinsightiga kobar pea sõlme. Tavaliselt kasutatakse `%%local` maagiline koos selle `%%sql` maagiline, kus on `-o` parameeter. Funktsiooni `-o` parameetri püsiksid kohalikult, SQL-päringu väljund ja seejärel `%%local` maagiline oleks käivitamine täitmisega saate käivitada kohalikult vastu väljund SQL-päringud, mis on kestnud kohalikult koodilõigu.

### <a name="query-the-data-by-using-sql"></a>Päringu andmete SQL-i abil
Selle päringu toob takso reisi hind summa, reisijate arv ja näpunäite summa.

    # RUN THE SQL QUERY
    %%sql -q -o sqlResults
    SELECT fare_amount, passenger_count, tip_amount, tipped FROM taxi_train WHERE passenger_count > 0 AND passenger_count < 7 AND fare_amount > 0 AND fare_amount < 200 AND payment_type in ('CSH', 'CRD') AND tip_amount > 0 AND tip_amount < 25

Järgmine kood on `%%local` maagiline loob kohalikud andmed raami, sqlResults. SqlResults abil saate diagrammile matplotlib abil.

> [AZURE.TIP] Selles artiklis kasutatakse mitu korda kohaliku maagiline. Kui teie andmekogumis on suur, palun kuulake loomine kohaliku mälu andmete paneeli, mis ei sobi.

### <a name="plot-the-data"></a>Andmete joonestamiseks

Pärast andmete paneeli on kohalik kontekstis Pandas andmete raami Python koodi abil saate diagrammile kanda.

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER
    %%local

    # USE THE JUPYTER AUTO-PLOTTING FEATURE TO CREATE INTERACTIVE FIGURES.
    # CLICK THE TYPE OF PLOT TO GENERATE (LINE, AREA, BAR, ETC.)
    sqlResults


 Säde tuum automaatselt tänav visualiseeritakse väljund (HiveQL) SQL-päringud pärast koodi käivitamiseks. Saate valida mitut tüüpi visualiseeringute vahel.
 
- Tabel
- Sektordiagramm
- Rea
- Ala
- Lint

Siin on andmed diagrammile kood:

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    import matplotlib.pyplot as plt
    %matplotlib inline

    # PLOT TIP BY PAYMENT TYPE AND PASSENGER COUNT
    ax1 = sqlResults[['tip_amount']].plot(kind='hist', bins=25, facecolor='lightblue')
    ax1.set_title('Tip amount distribution')
    ax1.set_xlabel('Tip Amount ($)')
    ax1.set_ylabel('Counts')
    plt.suptitle('')
    plt.show()

    # PLOT TIP BY PASSENGER COUNT
    ax2 = sqlResults.boxplot(column=['tip_amount'], by=['passenger_count'])
    ax2.set_title('Tip amount by Passenger count')
    ax2.set_xlabel('Passenger count')
    ax2.set_ylabel('Tip Amount ($)')
    plt.suptitle('')
    plt.show()

    # PLOT TIP AMOUNT BY FARE AMOUNT; SCALE POINTS BY PASSENGER COUNT
    ax = sqlResults.plot(kind='scatter', x= 'fare_amount', y = 'tip_amount', c='blue', alpha = 0.10, s=5*(sqlResults.passenger_count))
    ax.set_title('Tip amount by Fare amount')
    ax.set_xlabel('Fare Amount ($)')
    ax.set_ylabel('Tip Amount ($)')
    plt.axis([-2, 80, -2, 20])
    plt.show()


**Väljund:**

![Näpunäide summa histogramm](./media/machine-learning-data-science-process-scala-walkthrough/plot-tip-amount-histogram.png)

![Näpunäide summa reisijate arvu järgi](./media/machine-learning-data-science-process-scala-walkthrough/plot-tip-amount-by-passenger-count.png)

![Näpunäide hind summa summa](./media/machine-learning-data-science-process-scala-walkthrough/plot-tip-amount-by-fare-amount.png)


## <a name="create-features-and-transform-features-and-then-prep-data-for-input-into-modeling-functions"></a>Funktsioonid luua ja muuta funktsioonide ja seejärel ettevalmistamine modelleerimine funktsioonide sisestatud andmed

Puu-põhiste modelleerimine funktsioonide säde ML ja MLlib, tuleb ettevalmistamine kasutades erinevaid tehnika, nt binning, indekseerimine, üks kuum kodeering ja vectorization target ja funktsioonid. Siin on toimingute jälgimiseks selles jaotises.

1. Luua uus funktsioon **binning** tundi liikluse aja ämbrid sisse.
2. **Indekseerimise ja üks kuum kodeerimine** rakendamine kategooriline funktsioone.
3. **Valimi ja Tükelda andmehulga** koolitus ja testi murdarvud.
4. **Määrake koolitus muutuja ja funktsioone**, ja seejärel luua indekseeritud või ühe kuum kodeeritud koolitus ja testimine Sisestuskeel siltidega punkti olles jaotatud andmekomplektide (RDDs) või andmete raamid.
5. Automaatselt **kategoriseerida ja vectorize funktsioonid ja sihtkohad** sisendina kasutada seadme õppe mudelid.


### <a name="create-a-new-feature-by-binning-hours-into-traffic-time-buckets"></a>Luua uus funktsioon binning tunni liikluse aja ämbrid

Järgmine kood näete, kuidas luua uus funktsioon binning tunni sisse liikluse aja ämbrid ja kuidas vahemälu tulemiks olevad andmed paneeli mälu. Kui RDDs ja andmete raamid kasutatakse korduvalt vahemällu viib täiustatud täitmise korda. Vastavalt sellele, kuvatakse vahemälu RDDs ja andmete raamid veebisaidil etappide mitu järgmistest toimingutest.

    # CREATE FOUR BUCKETS FOR TRAFFIC TIMES
    val sqlStatement = """
        SELECT *,
        CASE
         WHEN (pickup_hour <= 6 OR pickup_hour >= 20) THEN "Night"
         WHEN (pickup_hour >= 7 AND pickup_hour <= 10) THEN "AMRush"
         WHEN (pickup_hour >= 11 AND pickup_hour <= 15) THEN "Afternoon"
         WHEN (pickup_hour >= 16 AND pickup_hour <= 19) THEN "PMRush"
        END as TrafficTimeBins
        FROM taxi_train
    """
    val taxi_df_train_with_newFeatures = sqlContext.sql(sqlStatement)

    # CACHE THE DATA FRAME IN MEMORY AND MATERIALIZE THE DATA FRAME IN MEMORY
    taxi_df_train_with_newFeatures.cache()
    taxi_df_train_with_newFeatures.count()


### <a name="indexing-and-one-hot-encoding-of-categorical-features"></a>Indekseerimise ja üks kuum kodeerimine kategooriline funktsioonid

Modelleerimine ja prognoosida MLlib funktsioonid nõuavad funktsioonide kategooriline sisendandmete indekseeritud või kodeeritud enne kasutamist. Selles jaotises kirjeldatakse, kuidas indekseerida või kodeerida kategooriline funktsioonid sisestatud modelleerimine funktsioonid.

Soovite indekseerida või kodeerida oma mudelite mudelist erinevatel viisidel. Näiteks logistika ja lineaarse regressioonisirge mudelite jaoks on vaja ühe kuum kodeerimine. Näiteks saate funktsiooni, mille kolmele laiendada kolme funktsiooni veergu. Iga veerg sisaldaks 0 või 1 sõltuvalt on jälgimine. MLlib pakub ühe kuum kodeerimine [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) funktsiooni. See kodeerija kaardid veeru silt indeksite veerule kahendarvu vektorite koos kõige ühe ühe-väärtuse. Selles kodeeringus algoritmide kohta, mida oodata arvväärtuste väärtusega funktsioone, näiteks logistilise regressiooni, saab rakendada kategooriline funktsioone.

Siin saate muuta ainult neli muutujate näidised, mis on märgi stringide kuvamiseks. Te saate ka muud muutujad, näiteks weekday, tähistab arvväärtusi nimega kategooriline muutujate indekseerida.

Indekseerimise, kasutage `StringIndexer()`, ja üks kuum kodeeringus kasutamine `OneHotEncoder()` MLlib funktsioone. Siin on kood indeks ja kodeerida kategooriline funktsioonid:


    # CREATE INDEXES AND ONE-HOT ENCODED VECTORS FOR SEVERAL CATEGORICAL FEATURES

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # INDEX AND ENCODE VENDOR_ID
    val stringIndexer = new StringIndexer().setInputCol("vendor_id").setOutputCol("vendorIndex").fit(taxi_df_train_with_newFeatures)
    val indexed = stringIndexer.transform(taxi_df_train_with_newFeatures)
    val encoder = new OneHotEncoder().setInputCol("vendorIndex").setOutputCol("vendorVec")
    val encoded1 = encoder.transform(indexed)

    # INDEX AND ENCODE RATE_CODE
    val stringIndexer = new StringIndexer().setInputCol("rate_code").setOutputCol("rateIndex").fit(encoded1)
    val indexed = stringIndexer.transform(encoded1)
    val encoder = new OneHotEncoder().setInputCol("rateIndex").setOutputCol("rateVec")
    val encoded2 = encoder.transform(indexed)

    # INDEX AND ENCODE PAYMENT_TYPE
    val stringIndexer = new StringIndexer().setInputCol("payment_type").setOutputCol("paymentIndex").fit(encoded2)
    val indexed = stringIndexer.transform(encoded2)
    val encoder = new OneHotEncoder().setInputCol("paymentIndex").setOutputCol("paymentVec")
    val encoded3 = encoder.transform(indexed)

    # INDEX AND TRAFFIC TIME BINS
    val stringIndexer = new StringIndexer().setInputCol("TrafficTimeBins").setOutputCol("TrafficTimeBinsIndex").fit(encoded3)
    val indexed = stringIndexer.transform(encoded3)
    val encoder = new OneHotEncoder().setInputCol("TrafficTimeBinsIndex").setOutputCol("TrafficTimeBinsVec")
    val encodedFinal = encoder.transform(indexed)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**Väljund:**

Lahtri aega: 4 sekundit.



### <a name="sample-and-split-the-data-set-into-training-and-test-fractions"></a>Valimi ja tükeldatud andmehulga üheks koolitus ja testi murdude

Järgmine kood tekitab juhuslikku valimit andmeid (25% selles näites). Kuigi valimite ei ole vaja näites andmehulga mahu tõttu, artiklist näete, kuidas saate proovida, et teate, kuidas kasutada oma probleeme vastavalt vajadusele. Kui suur näiteid, see märgatavat aega säästa, samal ajal saate koolitada mudelid. Järgmiseks tükeldada valimi osa koolitus (75% selles näites) ja testimise osa (25% selles näites) kasutada liigitamine ja regressiooni modelleerimine.

Iga rea ("rand" veerus) valimiseks rist – valideerimise voldid ajal koolitus kasutatavate juhusliku arvu (vahemikus 0 kuni 1) lisada.


    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # SPECIFY SAMPLING AND SPLITTING FRACTIONS
    val samplingFraction = 0.25;
    val trainingFraction = 0.75;
    val testingFraction = (1-trainingFraction);
    val seed = 1234;
    val encodedFinalSampledTmp = encodedFinal.sample(withReplacement = false, fraction = samplingFraction, seed = seed)
    val sampledDFcount = encodedFinalSampledTmp.count().toInt

    val generateRandomDouble = udf(() => {
        scala.util.Random.nextDouble
    })

    # ADD A RANDOM NUMBER FOR CROSS-VALIDATION
    val encodedFinalSampled = encodedFinalSampledTmp.withColumn("rand", generateRandomDouble());

    # SPLIT THE SAMPLED DATA FRAME INTO TRAIN AND TEST, WITH A RANDOM COLUMN ADDED FOR DOING CROSS-VALIDATION (SHOWN LATER)
    # INCLUDE A RANDOM COLUMN FOR CREATING CROSS-VALIDATION FOLDS
    val splits = encodedFinalSampled.randomSplit(Array(trainingFraction, testingFraction), seed = seed)
    val trainData = splits(0)
    val testData = splits(1)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**Väljund:**

Lahtri aega: kaks sekundit all.


### <a name="specify-training-variable-and-features-and-then-create-indexed-or-one-hot-encoded-training-and-testing-input-labeled-point-rdds-or-data-frames"></a>Määrake koolitus muutuja ja funktsioonid ja seejärel looge indekseeritud või ühe kuum kodeeritud koolitus ja testimine sisestatud andmete või punkti RDDs raamid sildistatud

See jaotis sisaldab koodi, mis näitab, kuidas indekseerida kategooriline tekstandmeid siltidega punkti andmetüübina ja kodeerida selle abil saate koolitada ja testida MLlib logistilise regressiooni ja muud liigitamine mudelid. Siltidega punkti objektid on vormindatud nii, et on vaja sisendandmete enamik Õppekeskuse algoritmide kohta rakenduses MLlib masina RDDs. [Sildistatud punkti](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) on kohalik vektorkuju, kas tihe või vähe, seostatud sildi/vastus.

Järgmine kood, saate määrata target (sõltuvad) muutuja ja koolitada mudelite kasutada funktsioone. Seejärel saate luua indekseeritud või ühe kuum kodeeritud koolitus ja testimine sisestatud andmete või punkti RDDs raamid nimega.


    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # MAP NAMES OF FEATURES AND TARGETS FOR CLASSIFICATION AND REGRESSION PROBLEMS
    val featuresIndOneHot = List("paymentVec", "vendorVec", "rateVec", "TrafficTimeBinsVec", "pickup_hour", "weekday", "passenger_count", "trip_time_in_secs", "trip_distance", "fare_amount").map(encodedFinalSampled.columns.indexOf(_))
    val featuresIndIndex = List("paymentIndex", "vendorIndex", "rateIndex", "TrafficTimeBinsIndex", "pickup_hour", "weekday", "passenger_count", "trip_time_in_secs", "trip_distance", "fare_amount").map(encodedFinalSampled.columns.indexOf(_))

    # SPECIFY THE TARGET FOR CLASSIFICATION ('tipped') AND REGRESSION ('tip_amount') PROBLEMS
    val targetIndBinary = List("tipped").map(encodedFinalSampled.columns.indexOf(_))
    val targetIndRegression = List("tip_amount").map(encodedFinalSampled.columns.indexOf(_))

    # CREATE INDEXED LABELED POINT RDD OBJECTS
    val indexedTRAINbinary = trainData.rdd.map(r => LabeledPoint(r.getDouble(targetIndBinary(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)))
    val indexedTESTbinary = testData.rdd.map(r => LabeledPoint(r.getDouble(targetIndBinary(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)))
    val indexedTRAINreg = trainData.rdd.map(r => LabeledPoint(r.getDouble(targetIndRegression(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)))
    val indexedTESTreg = testData.rdd.map(r => LabeledPoint(r.getDouble(targetIndRegression(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)))

    # CREATE INDEXED DATA FRAMES THAT YOU CAN USE TO TRAIN BY USING SPARK ML FUNCTIONS
    val indexedTRAINbinaryDF = indexedTRAINbinary.toDF()
    val indexedTESTbinaryDF = indexedTESTbinary.toDF()
    val indexedTRAINregDF = indexedTRAINreg.toDF()
    val indexedTESTregDF = indexedTESTreg.toDF()

    # CREATE ONE-HOT ENCODED (VECTORIZED) DATA FRAMES THAT YOU CAN USE TO TRAIN BY USING SPARK ML FUNCTIONS
    val assemblerOneHot = new VectorAssembler().setInputCols(Array("paymentVec", "vendorVec", "rateVec", "TrafficTimeBinsVec", "pickup_hour", "weekday", "passenger_count", "trip_time_in_secs", "trip_distance", "fare_amount")).setOutputCol("features")
    val OneHotTRAIN = assemblerOneHot.transform(trainData)
    val OneHotTEST = assemblerOneHot.transform(testData)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**Väljund:**

Lahtri aega: 4 sekundit.


### <a name="automatically-categorize-and-vectorize-features-and-targets-to-use-as-inputs-for-machine-learning-models"></a>Automaatselt kategoriseerida ja vectorize funktsioonid ja sihtkohad sisendina kasutada seadme õppe mudelid

Kasutage säde ML liigitamiseks target ja funktsioonide puu-põhiste modelleerimine funktsioonide kasutamine. Kood on lõpule viidud kahe toimingu:

-   Loob kahendarvu target liigitamiseks, määrates väärtuse 0 või 1, kasutades lävi väärtuse 0,5 iga andmepunkti vahemikus 0 kuni 1.
- Automaatselt kategooria funktsioone. Kui mõni funktsioon distinct arvväärtused arv on väiksem kui 32, et see funktsioon on liigitatud.

Siin on nende kahe toimingu kood.

    # CATEGORIZE FEATURES AND BINARIZE THE TARGET FOR THE BINARY CLASSIFICATION PROBLEM

    # TRAIN DATA
    val indexer = new VectorIndexer().setInputCol("features").setOutputCol("featuresCat").setMaxCategories(32)
    val indexerModel = indexer.fit(indexedTRAINbinaryDF)
    val indexedTrainwithCatFeat = indexerModel.transform(indexedTRAINbinaryDF)
    val binarizer: Binarizer = new Binarizer().setInputCol("label").setOutputCol("labelBin").setThreshold(0.5)
    val indexedTRAINwithCatFeatBinTarget = binarizer.transform(indexedTrainwithCatFeat)

    # TEST DATA
    val indexerModel = indexer.fit(indexedTESTbinaryDF)
    val indexedTrainwithCatFeat = indexerModel.transform(indexedTESTbinaryDF)
    val binarizer: Binarizer = new Binarizer().setInputCol("label").setOutputCol("labelBin").setThreshold(0.5)
    val indexedTESTwithCatFeatBinTarget = binarizer.transform(indexedTrainwithCatFeat)

    # CATEGORIZE FEATURES FOR THE REGRESSION PROBLEM
    # CREATE PROPERLY INDEXED AND CATEGORIZED DATA FRAMES FOR TREE-BASED MODELS

    # TRAIN DATA
    val indexer = new VectorIndexer().setInputCol("features").setOutputCol("featuresCat").setMaxCategories(32)
    val indexerModel = indexer.fit(indexedTRAINregDF)
    val indexedTRAINwithCatFeat = indexerModel.transform(indexedTRAINregDF)

    # TEST DATA
    val indexerModel = indexer.fit(indexedTESTbinaryDF)
    val indexedTESTwithCatFeat = indexerModel.transform(indexedTESTregDF)



## <a name="binary-classification-model-predict-whether-a-tip-should-be-paid"></a>Binaarsed mudeli: prognoosida Näpunäide pöörata

Selles jaotises saate luua kahendarvu liigitamine mudelite prognoosida, kas Näpunäide tuleb tasuda kolme tüüpi:

- **Logistilise regressiooni mudeli** abil säde ML `LogisticRegression()` funktsioon
- **Juhusliku mets mudeli** abil säde ML `RandomForestClassifier()` funktsioon
- **Astmikvärvi suurendada puu mudeli** , kasutades funktsiooni MLlib `GradientBoostedTrees()` funktsioon

### <a name="create-a-logistic-regression-model"></a>Logistilise regressiooni andmemudeli loomine

Järgmiseks säde ML abil luua logistilise regressiooni mudeli `LogisticRegression()` funktsioon. Mudeli koostamise juhiseid koodi loomine

1. Määrake **rongi mudeli** andmete üks parameeter.
2. **Hinnake mudeli** koos mõõdikute testi andmehulgas.
3. **Salvestage mudel** bloobimälu tulevaste ettenähtud.
4. **Keskmine mudeli** testi andmetega.
5. Opsüsteem omadus (ROC) kõverjoonte vastuvõtja **joonistada tulemusi** .

Siin on neid toiminguid kood:

    # CREATE A LOGISTIC REGRESSION MODEL
    val lr = new LogisticRegression().setLabelCol("tipped").setFeaturesCol("features").setMaxIter(10).setRegParam(0.3).setElasticNetParam(0.8)
    val lrModel = lr.fit(OneHotTRAIN)

    # PREDICT ON THE TEST DATA SET
    val predictions = lrModel.transform(OneHotTEST)

    # SELECT `BinaryClassificationEvaluator()` TO COMPUTE THE TEST ERROR
    val evaluator = new BinaryClassificationEvaluator().setLabelCol("tipped").setRawPredictionCol("probability").setMetricName("areaUnderROC")
    val ROC = evaluator.evaluate(predictions)
    println("ROC on test data = " + ROC)

    # SAVE THE MODEL
    val datestamp = Calendar.getInstance().getTime().toString.replaceAll(" ", ".").replaceAll(":", "_");
    val modelName = "LogisticRegression__"
    val filename = modelDir.concat(modelName).concat(datestamp)
    lrModel.save(filename);

Laadi, Keskmine ja salvestada.

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # LOAD THE SAVED MODEL AND SCORE THE TEST DATA SET
    val savedModel = org.apache.spark.ml.classification.LogisticRegressionModel.load(filename)
    println(s"Coefficients: ${savedModel.coefficients} Intercept: ${savedModel.intercept}")

    # SCORE THE MODEL ON THE TEST DATA
    val predictions = savedModel.transform(OneHotTEST).select("tipped","probability","rawPrediction")
    predictions.registerTempTable("testResults")

    # SELECT `BinaryClassificationEvaluator()` TO COMPUTE THE TEST ERROR
    val evaluator = new BinaryClassificationEvaluator().setLabelCol("tipped").setRawPredictionCol("probability").setMetricName("areaUnderROC")
    val ROC = evaluator.evaluate(predictions)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    # PRINT THE ROC RESULTS
    println("ROC on test data = " + ROC)


**Väljund:**

ROC testi andmete põhjal = 0.9827381497557599


Kohaliku Pandas andmed raamid Python abil saate diagrammile ROC kõver.


    # QUERY THE RESULTS
    %%sql -q -o sqlResults
    SELECT tipped, probability from testResults


    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    %matplotlib inline
    from sklearn.metrics import roc_curve,auc

    sqlResults['probFloat'] = sqlResults.apply(lambda row: row['probability'].values()[0][1], axis=1)
    predictions_pddf = sqlResults[["tipped","probFloat"]]

    # PREDICT THE ROC CURVE
    # predictions_pddf = sqlResults.rename(columns={'_1': 'probability', 'tipped': 'label'})
    prob = predictions_pddf["probFloat"]
    fpr, tpr, thresholds = roc_curve(predictions_pddf['tipped'], prob, pos_label=1);
    roc_auc = auc(fpr, tpr)

    # PLOT THE ROC CURVE
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


**Väljund:**

![Näpunäide või pole näpunäite ROC kõvera](./media/machine-learning-data-science-process-scala-walkthrough/plot-roc-curve-tip-or-not.png)


### <a name="create-a-random-forest-classification-model"></a>Juhusliku mets liigitamine andmemudeli loomine

Järgmiseks juhusliku mets liigitamine andmemudeli loomine abil säde ML `RandomForestClassifier()` funktsioon ning seejärel proovige mudeli testi andmete põhjal.


    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # CREATE THE RANDOM FOREST CLASSIFIER MODEL
    val rf = new RandomForestClassifier().setLabelCol("labelBin").setFeaturesCol("featuresCat").setNumTrees(10).setSeed(1234)

    # FIT THE MODEL
    val rfModel = rf.fit(indexedTRAINwithCatFeatBinTarget)
    val predictions = rfModel.transform(indexedTESTwithCatFeatBinTarget)

    # EVALUATE THE MODEL
    val evaluator = new MulticlassClassificationEvaluator().setLabelCol("label").setPredictionCol("prediction").setMetricName("f1")
    val Test_f1Score = evaluator.evaluate(predictions)
    println("F1 score on test data: " + Test_f1Score);

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    # CALCULATE BINARY CLASSIFICATION EVALUATION METRICS
    val evaluator = new BinaryClassificationEvaluator().setLabelCol("label").setRawPredictionCol("probability").setMetricName("areaUnderROC")
    val ROC = evaluator.evaluate(predictions)
    println("ROC on test data = " + ROC)


**Väljund:**

ROC testi andmete põhjal = 0.9847103571552683


### <a name="create-a-gbt-classification-model"></a>GBT liigitamine andmemudeli loomine

Järgmiseks GBT liigitamine andmemudeli loomine abil MLlib's `GradientBoostedTrees()` funktsioon ning seejärel proovige mudeli testi andmete põhjal.


    # TRAIN A GBT CLASSIFICATION MODEL BY USING MLLIB AND A LABELED POINT

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # DEFINE THE GBT CLASSIFICATION MODEL
    val boostingStrategy = BoostingStrategy.defaultParams("Classification")
    boostingStrategy.numIterations = 20
    boostingStrategy.treeStrategy.numClasses = 2
    boostingStrategy.treeStrategy.maxDepth = 5
    boostingStrategy.treeStrategy.categoricalFeaturesInfo = Map[Int, Int]((0,2),(1,2),(2,6),(3,4))

    # TRAIN THE MODEL
    val gbtModel = GradientBoostedTrees.train(indexedTRAINbinary, boostingStrategy)

    # SAVE THE MODEL IN BLOB STORAGE
    val datestamp = Calendar.getInstance().getTime().toString.replaceAll(" ", ".").replaceAll(":", "_");
    val modelName = "GBT_Classification__"
    val filename = modelDir.concat(modelName).concat(datestamp)
    gbtModel.save(sc, filename);

    # EVALUATE THE MODEL ON TEST INSTANCES AND THE COMPUTE TEST ERROR
    val labelAndPreds = indexedTESTbinary.map { point =>
      val prediction = gbtModel.predict(point.features)
      (point.label, prediction)
    }
    val testErr = labelAndPreds.filter(r => r._1 != r._2).count.toDouble / indexedTRAINbinary.count()
    //println("Learned classification GBT model:\n" + gbtModel.toDebugString)
    println("Test Error = " + testErr)

    # USE BINARY AND MULTICLASS METRICS TO EVALUATE THE MODEL ON THE TEST DATA
    val metrics = new MulticlassMetrics(labelAndPreds)
    println(s"Precision: ${metrics.precision}")
    println(s"Recall: ${metrics.recall}")
    println(s"F1 Score: ${metrics.fMeasure}")

    val metrics = new BinaryClassificationMetrics(labelAndPreds)
    println(s"Area under PR curve: ${metrics.areaUnderPR}")
    println(s"Area under ROC curve: ${metrics.areaUnderROC}")

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    # PRINT THE ROC METRIC
    println(s"Area under ROC curve: ${metrics.areaUnderROC}")


**Väljund:**

ROC kõvera all: 0.9846895479241554


## <a name="regression-model-predict-tip-amount"></a>Regressiooni mudel: prognoosida näpunäite summa

Selles jaotises saate luua kahte tüüpi regressiooni mudeleid prognoosida näpunäite summa:

- Mõne **lahendada lineaarse regressioonisirge mudeli** abil säde ML `LinearRegression()` funktsioon. Saate salvestage mudel ja hinnata mudeli testi andmete põhjal.
- **Astmiku suurendavad puu regressiooni mudeli** abil säde ML `GBTRegressor()` funktsioon.


### <a name="create-a-regularized-linear-regression-model"></a>Lahendada lineaarse regressioonisirge andmemudeli loomine

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # CREATE A REGULARIZED LINEAR REGRESSION MODEL BY USING THE SPARK ML FUNCTION AND DATA FRAMES
    val lr = new LinearRegression().setLabelCol("tip_amount").setFeaturesCol("features").setMaxIter(10).setRegParam(0.3).setElasticNetParam(0.8)

    # FIT THE MODEL BY USING DATA FRAMES
    val lrModel = lr.fit(OneHotTRAIN)
    println(s"Coefficients: ${lrModel.coefficients} Intercept: ${lrModel.intercept}")

    # SUMMARIZE THE MODEL OVER THE TRAINING SET AND PRINT METRICS
    val trainingSummary = lrModel.summary
    println(s"numIterations: ${trainingSummary.totalIterations}")
    println(s"objectiveHistory: ${trainingSummary.objectiveHistory.toList}")
    trainingSummary.residuals.show()
    println(s"RMSE: ${trainingSummary.rootMeanSquaredError}")
    println(s"r2: ${trainingSummary.r2}")

    # SAVE THE MODEL IN AZURE BLOB STORAGE
    val datestamp = Calendar.getInstance().getTime().toString.replaceAll(" ", ".").replaceAll(":", "_");
    val modelName = "LinearRegression__"
    val filename = modelDir.concat(modelName).concat(datestamp)
    lrModel.save(filename);

    # PRINT THE COEFFICIENTS
    println(s"Coefficients: ${lrModel.coefficients} Intercept: ${lrModel.intercept}")

    # SCORE THE MODEL ON TEST DATA
    val predictions = lrModel.transform(OneHotTEST)

    # EVALUATE THE MODEL ON TEST DATA
    val evaluator = new RegressionEvaluator().setLabelCol("tip_amount").setPredictionCol("prediction").setMetricName("r2")
    val r2 = evaluator.evaluate(predictions)
    println("R-sqr on test data = " + r2)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**Väljund:**

Lahtri aega: 13 sekundit.


    # LOAD A SAVED LINEAR REGRESSION MODEL FROM BLOB STORAGE AND SCORE A TEST DATA SET

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # LOAD A SAVED LINEAR REGRESSION MODEL FROM AZURE BLOB STORAGE
    val savedModel = org.apache.spark.ml.regression.LinearRegressionModel.load(filename)
    println(s"Coefficients: ${savedModel.coefficients} Intercept: ${savedModel.intercept}")

    # SCORE THE MODEL ON TEST DATA
    val predictions = savedModel.transform(OneHotTEST).select("tip_amount","prediction")
    predictions.registerTempTable("testResults")

    # EVALUATE THE MODEL ON TEST DATA
    val evaluator = new RegressionEvaluator().setLabelCol("tip_amount").setPredictionCol("prediction").setMetricName("r2")
    val r2 = evaluator.evaluate(predictions)
    println("R-sqr on test data = " + r2)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    # PRINT THE RESULTS
    println("R-sqr on test data = " + r2)


**Väljund:**

R-sqr testi andmete põhjal = 0.5960320470835743


Järgmiseks päringu testi tulemused nimega andmed raami ja visualiseerida AutoVizWidget ja matplotlib abil.


    # RUN A SQL QUERY
    %%sql -q -o sqlResults
    select * from testResults

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER
    %%local

    # USE THE JUPYTER AUTO-PLOTTING FEATURE TO CREATE INTERACTIVE FIGURES
    # CLICK THE TYPE OF PLOT TO GENERATE (LINE, AREA, BAR, AND SO ON)
    sqlResults

Koodi loob kohalikud andmed raami päringu väljund ja kujundab andmed. Funktsiooni `%%local` maagiline loob kohalikud andmed raami, `sqlResults`, mille abil saate diagrammile koos matplotlib.

>[AZURE.NOTE] Selles artiklis kasutatakse mitu korda selle säde maagiline. Kui suurt hulka andmeid on suur, tuleks proovida, luua andmete paneeli, mis ei sobi kohaliku mälu.

Luua krundid Python matplotlib abil.

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    sqlResults
    %matplotlib inline
    import numpy as np

    # PLOT THE RESULTS
    ax = sqlResults.plot(kind='scatter', figsize = (6,6), x='tip_amount', y='prediction', color='blue', alpha = 0.25, label='Actual vs. predicted');
    fit = np.polyfit(sqlResults['tip_amount'], sqlResults['prediction'], deg=1)
    ax.set_title('Actual vs. Predicted Tip Amounts ($)')
    ax.set_xlabel("Actual")
    ax.set_ylabel("Predicted")
    #ax.plot(sqlResults['tip_amount'], fit[0] * sqlResults['prediction'] + fit[1], color='magenta')
    plt.axis([-1, 15, -1, 8])
    plt.show(ax)

**Väljund:**

![Näpunäide summa: tegelik / prognoositud](./media/machine-learning-data-science-process-scala-walkthrough/plot-actual-vs-predicted-tip-amount.png)


### <a name="create-a-gbt-regression-model"></a>GBT regressiooni andmemudeli loomine

GBT regressiooni andmemudeli loomine abil säde ML `GBTRegressor()` funktsioon ning seejärel proovige mudeli testi andmete põhjal.

[Astmiku võimendada puude](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) on otsust puude komplektid. GBTs koolitada otsust puude korduvalt kaotsimineku funktsiooni minimeerimiseks. Saate GBTs regressiooni ja liigitamine. Need võite hakkama kategooriline funktsioone, pole vaja funktsiooni skaleerimist ja saate jäädvustada nonlinearities ja funktsioon kasutusviisid. Samuti saate neid kasutada multiclass-liigitamine säte.


    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # TRAIN A GBT REGRESSION MODEL
    val gbt = new GBTRegressor().setLabelCol("label").setFeaturesCol("featuresCat").setMaxIter(10)
    val gbtModel = gbt.fit(indexedTRAINwithCatFeat)

    # MAKE PREDICTIONS
    val predictions = gbtModel.transform(indexedTESTwithCatFeat)

    # COMPUTE TEST SET R2
    val evaluator = new RegressionEvaluator().setLabelCol("label").setPredictionCol("prediction").setMetricName("r2")
    val Test_R2 = evaluator.evaluate(predictions)


    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    # PRINT THE RESULTS
    println("Test R-sqr is: " + Test_R2);


**Väljund:**

Testi R-sqr on: 0.7655383534596654



## <a name="advanced-modeling-utilities-for-optimization"></a>Täpsem modelleerimine Utiliidid optimeerimine

Selles jaotises saate kohapeal õ Utiliidid kasutavate arendajad sageli andmemudeli optimeerimine. Täpsemalt saab optimeerida parameeter pühkimine ja rist – valideerimise abil arvuti õ mudelite kolm võimalust:

-   Andmete tükeldamine raudtee- ja valideerimine komplekti, optimeerimine mudeli abil hyper-parameeter pühkimine komplekt ja hindate valideerimine komplekti (lineaarse regressioonisirge)
-   Optimeerimine mudeli rist – valideerimise ja hyper-parameetri pühkimine funktsiooniga säde ML CrossValidator (kahendarvu liigitus)
-   Mudeli optimeerimine, kasutades kohandatud rist – valideerimise ja parameeter-pühkimine koodi kasutada mis tahes seadme Õppekeskuse funktsioon ja parameetrite määramine (lineaarse regressioonisirge)


**Rist-valideerimine** on meetod, mis hindab, kuidas mudeli andmete teadaolevad kogumi väljaõppe on üldistada prognoosida andmekogumi, kuhu see on ei saanud funktsioone. Selle tehnika üldine mõte on mudeli õpetatakse teadaolevate andmete andmete kogumi, ja seejärel oma prognoose täpsuse testitakse sõltumatu andmehulgas.. Levinud rakendamine on andmehulga jagada *k*-voldid, ja seejärel koolitada round-jaan mood kõik peale ühe voldid mudel.

**Hyper-parameetri optimeerimine** on probleem, valides määrata hyper-parameetrid õ algoritmi, tavaliselt eesmärgiga optimeerida mõõtmine on algoritmi jõudlust sõltumatu andmehulgas.. Hyper-parameeter on väärtus, mille peate määrama väljaspool mudeli koolitus toimingut. Oletused kohta hyper parameetrite väärtused võivad mõjutada paindlikkust ja mudeli täpsuse. Otsus puude on hyper-parameetrid, näiteks, nt soovitud sügavus ja puu lehtede arv. Peate määrama klassifikatsioonivead karistus Termini tugi vektorkuju masina (SVM).

Levinud viisi hyper-parameeter optimeerimiseks on kasutada ruudustiku otsing, nimetatakse ka **parameetri Korrasta**. Koordinaatvõrgu otsingus on täielik otsitakse määratud alamhulga hyper-parameeter ruumi õ algoritmile väärtused kaudu. Rist – valideerimise saate pakkumise mõõdiku jõudluse sortimiseks välja koordinaatvõrgu otsingu algoritmi optimaalse tulemused. Kui kasutate rist – valideerimise hyper-parameeter pühkimine, aitab teil limiit probleeme nagu overfitting mudeli koolitus andmetega. Sellisel viisil mudeli säilitab rakendamiseks, millest koolitus andmed ei ekstraktitud üldine andmehulga võimsus.

### <a name="optimize-a-linear-regression-model-with-hyper-parameter-sweeping"></a>Lineaarse regressioonisirge mudel hyper-parameeter pühkimine optimeerimine

Järgmiseks tükeldada andmete raudtee- ja valideerimine komplekti, kasutage hyper-parameeter pühkimine mudeli optimeerida ning hinnata valideerimine komplekti (lineaarse regressioonisirge) komplekt.

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # RENAME `tip_amount` AS A LABEL
    val OneHotTRAINLabeled = OneHotTRAIN.select("tip_amount","features").withColumnRenamed(existingName="tip_amount",newName="label")
    val OneHotTESTLabeled = OneHotTEST.select("tip_amount","features").withColumnRenamed(existingName="tip_amount",newName="label")
    OneHotTRAINLabeled.cache()
    OneHotTESTLabeled.cache()

    # DEFINE THE ESTIMATOR FUNCTION: `THE LinearRegression()` FUNCTION
    val lr = new LinearRegression().setLabelCol("label").setFeaturesCol("features").setMaxIter(10)

    # DEFINE THE PARAMETER GRID
    val paramGrid = new ParamGridBuilder().addGrid(lr.regParam, Array(0.1, 0.01, 0.001)).addGrid(lr.fitIntercept).addGrid(lr.elasticNetParam, Array(0.1, 0.5, 0.9)).build()

    # DEFINE THE PIPELINE WITH A TRAIN/TEST VALIDATION SPLIT (75% IN THE TRAINING SET), AND THEN THE SPECIFY ESTIMATOR, EVALUATOR, AND PARAMETER GRID
    val trainPct = 0.75
    val trainValidationSplit = new TrainValidationSplit().setEstimator(lr).setEvaluator(new RegressionEvaluator).setEstimatorParamMaps(paramGrid).setTrainRatio(trainPct)

    # RUN THE TRAIN VALIDATION SPLIT AND CHOOSE THE BEST SET OF PARAMETERS
    val model = trainValidationSplit.fit(OneHotTRAINLabeled)

    # MAKE PREDICTIONS ON THE TEST DATA BY USING THE MODEL WITH THE COMBINATION OF PARAMETERS THAT PERFORMS THE BEST
    val testResults = model.transform(OneHotTESTLabeled).select("label", "prediction")

    # COMPUTE TEST SET R2
    val evaluator = new RegressionEvaluator().setLabelCol("label").setPredictionCol("prediction").setMetricName("r2")
    val Test_R2 = evaluator.evaluate(testResults)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");

    println("Test R-sqr is: " + Test_R2);


**Väljund:**

Testi R-sqr on: 0.6226484708501209


### <a name="optimize-the-binary-classification-model-by-using-cross-validation-and-hyper-parameter-sweeping"></a>Binaarsed mudeli abil rist – valideerimise ja hyper-parameeter pühkimine optimeerimine

Selles jaotises kirjeldatakse, kuidas optimeerida kahendarvu liigitamine mudeli rist – valideerimise ja hyper-parameeter pühkimine abil. See kasutab säde ML `CrossValidator` funktsioon.

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # CREATE DATA FRAMES WITH PROPERLY LABELED COLUMNS TO USE WITH THE TRAIN AND TEST SPLIT
    val indexedTRAINwithCatFeatBinTargetRF = indexedTRAINwithCatFeatBinTarget.select("labelBin","featuresCat").withColumnRenamed(existingName="labelBin",newName="label").withColumnRenamed(existingName="featuresCat",newName="features")
    val indexedTESTwithCatFeatBinTargetRF = indexedTESTwithCatFeatBinTarget.select("labelBin","featuresCat").withColumnRenamed(existingName="labelBin",newName="label").withColumnRenamed(existingName="featuresCat",newName="features")
    indexedTRAINwithCatFeatBinTargetRF.cache()
    indexedTESTwithCatFeatBinTargetRF.cache()

    # DEFINE THE ESTIMATOR FUNCTION
    val rf = new RandomForestClassifier().setLabelCol("label").setFeaturesCol("features").setImpurity("gini").setSeed(1234).setFeatureSubsetStrategy("auto").setMaxBins(32)

    # DEFINE THE PARAMETER GRID
    val paramGrid = new ParamGridBuilder().addGrid(rf.maxDepth, Array(4,8)).addGrid(rf.numTrees, Array(5,10)).addGrid(rf.minInstancesPerNode, Array(100,300)).build()

    # SPECIFY THE NUMBER OF FOLDS
    val numFolds = 3

    # DEFINE THE TRAIN/TEST VALIDATION SPLIT (75% IN THE TRAINING SET)
    val CrossValidator = new CrossValidator().setEstimator(rf).setEvaluator(new BinaryClassificationEvaluator).setEstimatorParamMaps(paramGrid).setNumFolds(numFolds)

    # RUN THE TRAIN VALIDATION SPLIT AND CHOOSE THE BEST SET OF PARAMETERS
    val model = CrossValidator.fit(indexedTRAINwithCatFeatBinTargetRF)

    # MAKE PREDICTIONS ON THE TEST DATA BY USING THE MODEL WITH THE COMBINATION OF PARAMETERS THAT PERFORMS THE BEST
    val testResults = model.transform(indexedTESTwithCatFeatBinTargetRF).select("label", "prediction")

    # COMPUTE THE TEST F1 SCORE
    val evaluator = new MulticlassClassificationEvaluator().setLabelCol("label").setPredictionCol("prediction").setMetricName("f1")
    val Test_f1Score = evaluator.evaluate(testResults)

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


**Väljund:**

Lahtri aega: 33 sekundit.


### <a name="optimize-the-linear-regression-model-by-using-custom-cross-validation-and-parameter-sweeping-code"></a>Kohandatud rist – valideerimise ja parameeter-pühkimine koodi lineaarse regressioonisirge mudeli optimeerimine

Järgmiseks optimeerimine mudeli kohandatud koodi ja tuvastamine parima mudeli parameetrid kõrgeim täpsuse kriteeriumi abil. Looge lõplik mudel, hinnata mudeli testi andmete põhjal ja salvestage mudel bloobimälu. Lõpuks laadimine mudeli, Keskmine testi andmed ja hindate täpsuse.

    # RECORD THE START TIME
    val starttime = Calendar.getInstance().getTime()

    # DEFINE THE PARAMETER GRID AND THE NUMBER OF FOLDS
    val paramGrid = new ParamGridBuilder().addGrid(rf.maxDepth, Array(5,10)).addGrid(rf.numTrees, Array(10,25,50)).build()

    val nFolds = 3
    val numModels = paramGrid.size
    val numParamsinGrid = 2

    # SPECIFY THE NUMBER OF CATEGORIES FOR CATEGORICAL VARIABLES
    val categoricalFeaturesInfo = Map[Int, Int]((0,2),(1,2),(2,6),(3,4))

    var maxDepth = -1
    var numTrees = -1
    var param = ""
    var paramval = -1
    var validateLB = -1.0
    var validateUB = -1.0
    val h = 1.0 / nFolds;
    val RMSE  = Array.fill(numModels)(0.0)

    # CREATE K-FOLDS
    val splits = MLUtils.kFold(indexedTRAINbinary, numFolds = nFolds, seed=1234)


    # LOOP THROUGH K-FOLDS AND THE PARAMETER GRID TO GET AND IDENTIFY THE BEST PARAMETER SET BY LEVEL OF ACCURACY
    for (i <- 0 to (nFolds-1)) {
        validateLB = i * h
        validateUB = (i + 1) * h
        val validationCV = trainData.filter($"rand" >= validateLB  && $"rand" < validateUB)
        val trainCV = trainData.filter($"rand" < validateLB  || $"rand" >= validateUB)
        val validationLabPt = validationCV.rdd.map(r => LabeledPoint(r.getDouble(targetIndRegression(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)));
        val trainCVLabPt = trainCV.rdd.map(r => LabeledPoint(r.getDouble(targetIndRegression(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)));
        validationLabPt.cache()
        trainCVLabPt.cache()

        for (nParamSets <- 0 to (numModels-1)) {
            for (nParams <- 0 to (numParamsinGrid-1)) {
                param = paramGrid(nParamSets).toSeq(nParams).param.toString.split("__")(1)
                paramval = paramGrid(nParamSets).toSeq(nParams).value.toString.toInt
                if (param == "maxDepth") {maxDepth = paramval}
                if (param == "numTrees") {numTrees = paramval}
            }
            val rfModel = RandomForest.trainRegressor(trainCVLabPt, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                                      numTrees=numTrees, maxDepth=maxDepth,
                                                      featureSubsetStrategy="auto",impurity="variance", maxBins=32)
            val labelAndPreds = validationLabPt.map { point =>
                                                     val prediction = rfModel.predict(point.features)
                                                     ( prediction, point.label )
                                                    }
            val validMetrics = new RegressionMetrics(labelAndPreds)
            val rmse = validMetrics.rootMeanSquaredError
            RMSE(nParamSets) += rmse
        }
        validationLabPt.unpersist();
        trainCVLabPt.unpersist();
    }
    val minRMSEindex = RMSE.indexOf(RMSE.min)

    # GET THE BEST PARAMETERS FROM A CROSS-VALIDATION AND PARAMETER SWEEP
    var best_maxDepth = -1
    var best_numTrees = -1
    for (nParams <- 0 to (numParamsinGrid-1)) {
        param = paramGrid(minRMSEindex).toSeq(nParams).param.toString.split("__")(1)
        paramval = paramGrid(minRMSEindex).toSeq(nParams).value.toString.toInt
        if (param == "maxDepth") {best_maxDepth = paramval}
        if (param == "numTrees") {best_numTrees = paramval}
    }

    # CREATE THE BEST MODEL WITH THE BEST PARAMETERS AND A FULL TRAINING DATA SET
    val best_rfModel = RandomForest.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                                      numTrees=best_numTrees, maxDepth=best_maxDepth,
                                                      featureSubsetStrategy="auto",impurity="variance", maxBins=32)

    # SAVE THE BEST RANDOM FOREST MODEL IN BLOB STORAGE
    val datestamp = Calendar.getInstance().getTime().toString.replaceAll(" ", ".").replaceAll(":", "_");
    val modelName = "BestCV_RF_Regression__"
    val filename = modelDir.concat(modelName).concat(datestamp)
    best_rfModel.save(sc, filename);

    # PREDICT ON THE TRAINING SET WITH THE BEST MODEL AND THEN EVALUATE
    val labelAndPreds = indexedTESTreg.map { point =>
                                            val prediction = best_rfModel.predict(point.features)
                                            ( prediction, point.label )
                                           }

    val test_rmse = new RegressionMetrics(labelAndPreds).rootMeanSquaredError
    val test_rsqr = new RegressionMetrics(labelAndPreds).r2

    # GET THE TIME TO RUN THE CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken to run the above cell: " + elapsedtime + " seconds.");


    # LOAD THE MODEL
    val savedRFModel = RandomForestModel.load(sc, filename)

    val labelAndPreds = indexedTESTreg.map { point =>
                                            val prediction = savedRFModel.predict(point.features)
                                            ( prediction, point.label )
                                           }
    # TEST THE MODEL
    val test_rmse = new RegressionMetrics(labelAndPreds).rootMeanSquaredError
    val test_rsqr = new RegressionMetrics(labelAndPreds).r2


**Väljund:**

Lahtri aega: 61 sekundit.

## <a name="consume-spark-built-machine-learning-models-automatically-with-scala"></a>Seadme säde ehitatud õ mudelite automaatselt Scala tarbimine

Teemad, mis hõlmavad andmete teadus protsessi Azure tööülesandeid sõelub ülevaate leiate teemast [Meeskonnatöö andmete teadus protsess](http://aka.ms/datascienceprocess).

[Meeskonnatöö andmete teadus protsessi juhendavad tutvustused](data-science-process-walkthroughs.md) kirjeldatakse muude lõpuni juhendavad tutvustused, mis illustreerivad teatud stsenaariumide meeskonnatöö andmete teadus protsessi etappe. Selle juhendavad tutvustused illustreerimine ka pilveteenuses ja kohapealsete tööriistad ja teenuste ühendamine töövoo või müügivõimaluste nutikad rakenduse loomiseks tehke järgmist.

[Keskmine säde ehitatud masina õppe mudelid](machine-learning-data-science-spark-model-consumption.md) näitab, kuidas kasutada Scala koodi laadimine ja uue andmehulga Keskmine arvuti õ mudelid sisseehitatud säde ja Azure'i bloobimälu salvestatakse automaatselt. Järgige seal esitatud juhiseid ja lihtsalt asendada Python koodi sellest artiklist leiate automatiseeritud tarbimine Scala kood.
