<properties 
    pageTitle="Välise pakettide kasutamine Jupyter märkmike Apache Spark rühmades Hdinsightiga kohta | Azure'i"
    description="Üksikasjalike juhiste kohta, kuidas konfigureerida Jupyter märkmike saadaval Hdinsightiga säde kogumite kasutada välise säde paketid." 
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
    ms.date="10/28/2016" 
    ms.author="nitinme"/>


# <a name="use-external-packages-with-jupyter-notebooks-in-apache-spark-clusters-on-hdinsight-linux"></a>Välise pakettide kasutamine Jupyter märkmike Hdinsightiga Linux Apache Spark rühmades

>[AZURE.NOTE] Selles artiklis on kohaldatav säde 1.5.2 Hdinsightiga 3.3 sisse- ja säde 1.6.2 Hdinsightiga 3.4. 

Saate teada, kuidas konfigureerida Jupyter märkmiku Apache Spark kobar klõpsake Hdinsightiga (Linux) kasutada välise, ühenduse kaasa paketid, mis pole kaasatud out-of-box klaster. 

Saate otsida [Maven hoidla](http://search.maven.org/) jaoks saadaolevate pakettide täieliku loendi. Samuti saate loendit saadaval pakettide muudest allikatest. Näiteks ühenduse kaasa paketid täieliku loendi on saadaval veebisaidil [Säde paketid](http://spark-packages.org/).

Sellest artiklist saate teada, kuidas [säde-csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) -paketi kasutamiseks Jupyter märkmik.

##<a name="prerequisites"></a>Eeltingimused

Vajate järgmist:

- Azure'i tellimuse. Leiate [Azure'i saada tasuta prooviversioon](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Apache Spark kobar Hdinsightiga Linux. Juhised leiate teemast [loomine Apache Spark kogumite Windows Azure Hdinsightiga sisse](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="use-external-packages-with-jupyter-notebooks"></a>Välise pakettide Jupyter märkmike kasutamine 

1. [Azure portaali](https://portal.azure.com/)kaudu startboard, klõpsake paani klaster säde (kui te kinnitatud selle startboard). Samuti saate liikuda klaster jaotises **Sirvi kõiki** > **Hdinsightiga kogumite**.   

2. Keelest säde kobar **Kiirlingid**nuppu ja klõpsake keelest **Kobar armatuurlaua** **Jupyter märkmik**. Kui kuvatakse vastav viip, sisestage administraatori identimisteave klaster.

    > [AZURE.NOTE] Võib-olla ka jõuate Jupyter märkmiku jaoks klaster, avades järgmine URL brauseri. Klaster nime __CLUSTERNAME__ asendamiseks tehke järgmist.
    >
    > `https://CLUSTERNAME.azurehdinsight.net/jupyter`

2. Looge uus märkmik. Klõpsake nuppu **Uus**ja seejärel klõpsake nuppu **säde**.

    ![Jupyter uue märkmiku loomine] (./media/hdinsight-apache-spark-jupyter-notebook-use-external-packages/hdispark.note.jupyter.createnotebook.png "Jupyter uue märkmiku loomine")

3. Uus märkmik on loodud ja nimega Untitled.pynb avada. Klõpsake selle märkmiku nime kohal ja sisestage sõbralik nimi.

    ![Esita märkmiku nimi] (./media/hdinsight-apache-spark-jupyter-notebook-use-external-packages/hdispark.note.jupyter.notebook.name.png "Esita märkmiku nimi")

4. Kasutage funktsiooni `%%configure` maagiline konfigureerimiseks kasutada ka välise paketi märkmik. Kasutage välise pakettide märkmikud veenduge, et helistate soovitud `%%configure` maagiline kood esimesse lahtrisse. See tagab, et tuum on konfigureeritud kasutama paketi enne selle seanss.

        %%configure
        { "packages":["com.databricks:spark-csv_2.10:1.4.0"] }


    >[AZURE.IMPORTANT] Kui unustate konfigureerimiseks tuum esimest lahtrit, saate selle `%%configure` abil soovitud `-f` parameeter, kuid mis ei taaskäivitage seanss ja kõik edenemise lähevad kaotsi.

5. Klõpsake eeltoodud koodilõigu `packages` eeldab, et maven koordinaate Maven keskses hoidlas loendit. Klõpsake selle koodilõigu `com.databricks:spark-csv_2.10:1.4.0` on maven koordinaatide **säde-csv** -paketi. Siin on, kuidas te ehitada paketi koordinaadid.

    lisamine. Leidke paketi Maven hoidla. Selles õpetuses kasutame [säde-csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar).
    
    b. Hoidla, Koguge väärtused **GroupId**, **ArtifactId**ja **versioon**.

    ![Kasutage välise paketid Jupyter märkmik] (./media/hdinsight-apache-spark-jupyter-notebook-use-external-packages/use-external-packages-with-jupyter.png "Kasutage välise paketid Jupyter märkmik")

    c. CONCATENATE kolme väärtused, eraldades need koolon (****:).

        com.databricks:spark-csv_2.10:1.4.0

6. Käivitage koodi lahter, mille on `%%configure` maagiline. See tuleb konfigureerida aluseks Liviuse seansi andsite paketi kasutamiseks. Märkmikus edaspidised lahtrites nüüd saate paketti, nagu allpool näidatud.

        val df = sqlContext.read.format("com.databricks.spark.csv").
        option("header", "true").
        option("inferSchema", "true").
        load("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

7. Seejärel saate käivitada pikad, nagu allpool näidatud dataframe, eelmises juhises loodud andmete kuvamiseks.

        df.show()

        df.select("Time").count()


## <a name="seealso"></a>Vt ka


* [Ülevaade: Apache Spark klõpsake Azure Hdinsightiga](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Stsenaariumid

* [Bi säde: andmeanalüüside interaktiivsed Hdinsightiga säde kasutamine koos Ärianalüüsi tööriistade kohta](hdinsight-apache-spark-use-bi-tools.md)

* [Seadme õppimisega säde: kasutamine säde rakenduses Hdinsightiga building temperatuur HVAC andmete analüüsimiseks](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

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

* [Jupyter oma arvutisse installida ja luua ühenduse mõne Hdinsightiga säde kobar](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Ressursside haldamine

* [Ressursid Apache Spark kobar rakenduses Windows Azure Hdinsightiga haldamine](hdinsight-apache-spark-resource-manager.md)

* [Töötavate on Apache Spark kobar rakenduses Hdinsightiga jälitamine ja silumine tööde haldamine](hdinsight-apache-spark-job-debugging.md)
