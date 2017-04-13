<properties 
    pageTitle="Apache Spark abil saate koostada seadme õ rakendusi Hdinsightiga | Microsoft Azure'i" 
    description="Üksikasjalike juhiste kohta, kuidas luua seadme õ rakendusi Apache Spark märkmike kasutamine" 
    services="hdinsight" 
    documentationCenter="" 
    authors="nitinme" 
    manager="jhubbard" 
    editor="cgronlun"
    tags="azure-portal"/>

<tags 
    ms.service="hdinsight" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/05/2016" 
    ms.author="nitinme"/>


# <a name="build-machine-learning-applications-to-run-on-apache-spark-clusters-on-hdinsight-linux"></a>Apache Spark kogumite Hdinsightiga Linux masina õ rakenduste koostamine

Saate teada, kuidas koostada Õppekeskuse rakenduste abil on Apache Spark kobar Hdinsightiga masina. Selles artiklis kirjeldatakse, kuidas kasutada Jupyter märkmiku saadaval klaster luua ja testige meie rakendust. Rakendus kasutab HVAC.csv näidisandmed, mis on saadaval kõigi kogumite vaikimisi.

**Eeltingimused**

Vajate järgmist:

- Azure'i tellimuse. Leiate [Azure'i saada tasuta prooviversioon](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Apache Spark kobar Hdinsightiga Linux. Juhised leiate teemast [loomine Apache Spark kogumite Windows Azure Hdinsightiga sisse](hdinsight-apache-spark-jupyter-spark-sql.md). 

##<a name="data"></a>Kuva mulle andmed

Enne alustamist koostamise rakendus, andke meile mõista andmed ja teeme andmete analüüsi liiki struktuuri. 

Selles artiklis kasutame valimi **HVAC.csv** andmefaili, mis on saadaval Azure Storage konto, millega te seostatud Hdinsightiga kobar. Salvestusruumi konto, on faili **\HdiSamples\HdiSamples\SensorSampleData\hvac**. Laadige alla ja avage CSV-faili saate andmeid kiirülevaate.  

![HVAC andmete hetktõmmis] (./media/hdinsight-apache-spark-ipython-notebook-machine-learning/hdispark.ml.show.data.png "HVAC andmete hetktõmmist")

Andmed kuvatakse target temperatuuri ja tegeliku temperatuuri building, kuhu on installitud HVAC süsteemide. Oletame, et **süsteemi** veeru tähistab süsteemi ID-d ja **SystemAge** veerg on põhiprojekt on kohale building aastate arv.

Kasutame neid andmeid prognoosida, kas hoone on kuumem või alla vastavalt target temperatuuri, antud süsteemi ID-d ja süsteemi vanus.

##<a name="app"></a>Seadme õ rakenduse abil säde MLlib kirjutamine

Selle rakenduse kasutame säde ML müügivõimaluste dokumendi liigitamine sooritamiseks. Ettevalmistamisel me dokumendi jagatud sõnade, sõnade teisendamine arvväärtuste funktsiooni vektorkuju ja lõpuks koostada mudeli ennustamine funktsioon vektorite ja siltide abil. Järgmiste toimingute loomise rakendus.

1. [Azure portaali](https://portal.azure.com/)kaudu startboard, klõpsake paani klaster säde (kui te kinnitatud selle startboard). Samuti saate liikuda klaster jaotises **Sirvi kõiki** > **Hdinsightiga kogumite**.   

2. Keelest säde kobar valige **Kobar armatuurlaud**, ja klõpsake nuppu **Jupyter märkmiku**. Kui kuvatakse vastav viip, sisestage administraatori identimisteave klaster.

    > [AZURE.NOTE] Võib-olla ka jõuate Jupyter märkmiku jaoks klaster, avades järgmine URL brauseri. Klaster nime __CLUSTERNAME__ asendamiseks tehke järgmist.
    >
    > `https://CLUSTERNAME.azurehdinsight.net/jupyter`

2. Looge uus märkmik. Klõpsake nuppu **Uus**ja seejärel klõpsake nuppu **PySpark**.

    ![Jupyter uue märkmiku loomine] (./media/hdinsight-apache-spark-ipython-notebook-machine-learning/hdispark.note.jupyter.createnotebook.png "Jupyter uue märkmiku loomine")

3. Uus märkmik on loodud ja nimega Untitled.pynb avada. Klõpsake selle märkmiku nime kohal ja sisestage sõbralik nimi.

    ![Esita märkmiku nimi] (./media/hdinsight-apache-spark-ipython-notebook-machine-learning/hdispark.note.jupyter.notebook.name.png "Esita märkmiku nimi")

3. Kuna te abil PySpark tuum märkmiku loonud, peate looma kõik kontekstides otseselt. Säde ja taru konteksti luuakse automaatselt teie eest, kui käivitate kood esimest lahtrit. Saate alustada vajalike seda tüüpi importimisega. Kleepige järgmine koodilõigu tühi lahter ja vajutage klahvikombinatsiooni **SHIFT + ENTER**. 

        from pyspark.ml import Pipeline
        from pyspark.ml.classification import LogisticRegression
        from pyspark.ml.feature import HashingTF, Tokenizer
        from pyspark.sql import Row
        
        import os
        import sys
        from pyspark.sql.types import *
        
        from pyspark.mllib.classification import LogisticRegressionWithSGD
        from pyspark.mllib.regression import LabeledPoint
        from numpy import array
        
        
     
4. Nüüd tuleb laadite andmed (hvac.csv), sõeluda selle ja kasutage seda koolitada mudel. Selleks saate määratleda funktsioon, mis kontrollib, kas hoone tegelik temperatuur on suurem kui target temperatuur. Kui tegelik temperatuur on suurem, hoone on kuum, tähistatud väärtus **1.0**. Kui tegelik temperatuur on väiksem, hoone on külm, tähistatud väärtus **0,0**. 

    Kleepige järgmine koodilõigu tühi lahter ja vajutage klahvikombinatsiooni **SHIFT + ENTER**.

        
        # List the structure of data for better understanding. Becuase the data will be
        # loaded as an array, this structure makes it easy to understand what each element
        # in the array corresponds to

        # 0 Date
        # 1 Time
        # 2 TargetTemp
        # 3 ActualTemp
        # 4 System
        # 5 SystemAge
        # 6 BuildingID

        LabeledDocument = Row("BuildingID", "SystemInfo", "label")

        # Define a function that parses the raw CSV file and returns an object of type LabeledDocument
        
        def parseDocument(line):
            values = [str(x) for x in line.split(',')]
            if (values[3] > values[2]):
                hot = 1.0
            else:
                hot = 0.0        
    
            textValue = str(values[4]) + " " + str(values[5])
    
            return LabeledDocument((values[6]), textValue, hot)

        # Load the raw HVAC.csv file, parse it using the function
        data = sc.textFile("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

        documents = data.filter(lambda s: "Date" not in s).map(parseDocument)
        training = documents.toDF()


5. Konfigureerimine säde masina õ kohaletoimetamisel, mis koosneb kolmest etapist: tokenizer, hashingTF ja lr. Lisateavet selle kohta, mis on näha müügivõimaluste ja kuidas see toimib <a href="http://spark.apache.org/docs/latest/ml-guide.html#how-it-works" target="_blank">säde masina Õppekeskuse kohaletoimetamisel</a>.

    Kleepige järgmine koodilõigu tühi lahter ja vajutage klahvikombinatsiooni **SHIFT + ENTER**.

        tokenizer = Tokenizer(inputCol="SystemInfo", outputCol="words")
        hashingTF = HashingTF(inputCol=tokenizer.getOutputCol(), outputCol="features")
        lr = LogisticRegression(maxIter=10, regParam=0.01)
        pipeline = Pipeline(stages=[tokenizer, hashingTF, lr])

6. Müügivõimaluste koolitus dokumenti mahu. Kleepige järgmine koodilõigu tühi lahter ja vajutage klahvikombinatsiooni **SHIFT + ENTER**.

        model = pipeline.fit(training)

7. Kontrollige koolitus dokumendi kontrollpunkt edenemise rakenduse. Kleepige järgmine koodilõigu tühi lahter ja vajutage klahvikombinatsiooni **SHIFT + ENTER**.

        training.show()

    See peaks andma väljund sarnaneb järgmisega:

        +----------+----------+-----+
        |BuildingID|SystemInfo|label|
        +----------+----------+-----+
        |         4|     13 20|  0.0|
        |        17|      3 20|  0.0|
        |        18|     17 20|  1.0|
        |        15|      2 23|  0.0|
        |         3|      16 9|  1.0|
        |         4|     13 28|  0.0|
        |         2|     12 24|  0.0|
        |        16|     20 26|  1.0|
        |         9|      16 9|  1.0|
        |        12|       6 5|  0.0|
        |        15|     10 17|  1.0|
        |         7|      2 11|  0.0|
        |        15|      14 2|  1.0|
        |         6|       3 2|  0.0|
        |        20|     19 22|  0.0|
        |         8|     19 11|  0.0|
        |         6|      15 7|  0.0|
        |        13|      12 5|  0.0|
        |         4|      8 22|  0.0|
        |         7|      17 5|  0.0|
        +----------+----------+-----+


    Minge tagasi ja kontrollige väljundi suhtes töötlemata CSV-faili. Näiteks CSV-faili esimene rida on andmed.

    ![HVAC andmete hetktõmmis] (./media/hdinsight-apache-spark-ipython-notebook-machine-learning/hdispark.ml.show.data.first.row.png "HVAC andmete hetktõmmist")

    Pange tähele, kuidas tegelik temperatuur on väiksem kui target temperatuur kujutav building on külm. Seega väljund koolitus väärtus tabeli esimeses reas **silt** on **0,0**, mis tähendab, et hoone ei ole kuum.

8.  Ettevalmistused andmehulgas käivitamiseks koolitatud mudeli vastu. Selleks me ei liigu süsteemi ID-d ja süsteemi vanus (tähistatakse **SystemInfo** koolitus väljund) ja mudeli oleks prognoosida, kas selle süsteemi ID ja süsteemi vanus building oleks kuumem (tähistatud 1.0) või külmemaks (tähistatud 0,0).

    Kleepige järgmine koodilõigu tühi lahter ja vajutage klahvikombinatsiooni **SHIFT + ENTER**.
        
        # SystemInfo here is a combination of system ID followed by system age
        Document = Row("id", "SystemInfo")
        test = sc.parallelize([(1L, "20 25"),
                      (2L, "4 15"),
                      (3L, "16 9"),
                      (4L, "9 22"),
                      (5L, "17 10"),
                      (6L, "7 22")]) \
            .map(lambda x: Document(*x)).toDF() 

9. Lõpuks tehke prognoose testi andmete põhjal. Kleepige järgmine koodilõigu tühi lahter ja vajutage klahvikombinatsiooni **SHIFT + ENTER**.

        # Make predictions on test documents and print columns of interest
        prediction = model.transform(test)
        selected = prediction.select("SystemInfo", "prediction", "probability")
        for row in selected.collect():
            print row

10. Peaksite nägema järgmine väljund:

        Row(SystemInfo=u'20 25', prediction=1.0, probability=DenseVector([0.4999, 0.5001]))
        Row(SystemInfo=u'4 15', prediction=0.0, probability=DenseVector([0.5016, 0.4984]))
        Row(SystemInfo=u'16 9', prediction=1.0, probability=DenseVector([0.4785, 0.5215]))
        Row(SystemInfo=u'9 22', prediction=1.0, probability=DenseVector([0.4549, 0.5451]))
        Row(SystemInfo=u'17 10', prediction=1.0, probability=DenseVector([0.4925, 0.5075]))
        Row(SystemInfo=u'7 22', prediction=0.0, probability=DenseVector([0.5015, 0.4985]))

    Esimese rea prognoosida, näete, et ID 20 ja 25 aastat süsteemi vanus on põhiprojekt building puhul kuum (**ennustamine = 1.0**). Esimene väärtus DenseVector (0.49999) vastab ennustamine 0,0 ja teine väärtus (0.5001) vastab ennustamine 1.0. Väljund, kuigi teine väärtus on vaid vähesel määral suurem, mudel näitab **ennustamine = 1.0**.

11. Pärast lõpetamist töötab rakendus, peaksite sulgumist ressursside vabastamiseks märkmik. Selleks märkmikku menüü **fail** nuppu **Sule ja peatada**. See on Sule ja Sule märkmik.
           

##<a name="anaconda"></a>Kasutage Anaconda scikit – lugege teegi masina õppe

Apache Spark kogumite kohta Hdinsightiga kaasata Anaconda teekide. See hõlmab ka selle **scikit – siit saate teada,** teegi masina õppimiseks. Teek sisaldab ka erinevate andmekogumi, mille abil saate luua otse märkmiku Jupyter valimi rakendusi. Näiteid selle scikit kasutamise kohta – siit saate teada, teegi, vt [http://scikit-learn.org/stable/auto_examples/index.html](http://scikit-learn.org/stable/auto_examples/index.html).

##<a name="seealso"></a>Vt ka

* [Ülevaade: Apache Spark klõpsake Azure Hdinsightiga](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Stsenaariumid

* [Bi säde: andmeanalüüside interaktiivsed Hdinsightiga säde kasutamine koos Ärianalüüsi tööriistade kohta](hdinsight-apache-spark-use-bi-tools.md)

* [Seadme õppimisega säde: kasutamine säde Hdinsightiga prognoosida toiduga kontrollitulemuste rakenduses](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

* [Säde Streaming: Kasutamine säde rakenduses reaalajas streaming rakenduste Hdinsightiga](hdinsight-apache-spark-eventhub-streaming.md)

* [Veebisaidi logi analüüs Hdinsightiga säde kasutamine](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Luua ja kasutada rakendusi

* [Kasutades Scala rakendusena loomine](hdinsight-apache-spark-create-standalone-application.md)

* [Käivitage töö eemalt säde klaster Liviuse abil](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Tööriistad ja laiendid

* [Hdinsightiga tööriistade lisandmooduli huvitav idee abil saate luua ja esitage säde Scala rakendusi](hdinsight-apache-spark-intellij-tool-plugin.md)

* [Hdinsightiga tööriistade lisandmooduli huvitav idee abil silumine säde rakenduste kaugühenduse teel](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)

* [Säde kobar klõpsake Hdinsightiga Zeppelin märkmike kasutamine](hdinsight-apache-spark-use-zeppelin-notebook.md)

* [Tuumad Jupyter märkmiku säde kobar Hdinsightiga jaoks saadaval](hdinsight-apache-spark-jupyter-notebook-kernels.md)

* [Välise pakettide Jupyter märkmike kasutamine](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)

* [Jupyter oma arvutisse installida ja luua ühenduse mõne Hdinsightiga säde kobar](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Ressursside haldamine

* [Ressursid Apache Spark kobar rakenduses Windows Azure Hdinsightiga haldamine](hdinsight-apache-spark-resource-manager.md)

* [Töötavate on Apache Spark kobar rakenduses Hdinsightiga jälitamine ja silumine tööde haldamine](hdinsight-apache-spark-job-debugging.md)


[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-weblogs-sample]: hdinsight-hive-analyze-website-log.md
[hdinsight-sensor-data-sample]: hdinsight-hive-analyze-sensor-data.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-management-portal]: https://manage.windowsazure.com/
[azure-create-storageaccount]: storage-create-storage-account.md
