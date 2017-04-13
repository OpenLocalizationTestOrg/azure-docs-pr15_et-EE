<properties 
    pageTitle="Kohandatud teekide abil on Hdinsightiga säde kobar analüüsida veebisaidi logid | Microsoft Azure'i" 
    description="Kasutage kohandatud teekide mõne Hdinsightiga säde kobar veebisaidi logid analüüsimiseks" 
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

# <a name="analyze-website-logs-using-a-custom-library-with-apache-spark-cluster-on-hdinsight-linux"></a>Analüüsi veebisaidi logid Apache Spark kobar Hdinsightiga Linux kohandatud teegi kasutamine

Selle märkmiku näitab, kuidas kohandatud teegi abil säde Hdinsightiga klõpsake logi andmete analüüsiks. Kasutame kohandatud teek on Python teegi nimega **iislogparser.py**.

> [AZURE.TIP] Selles õpetuses on saadaval Jupyter märkmiku säde (Linux) klaster, mis on loodud Hdinsightiga ka. Märkmiku teenuse abil saate käivitada Python Koodilõigud märkmikust ise. Õpetuse kaudu sees märkmiku täitmiseks luua säde kobar, käivitage Jupyter märkmiku (`https://CLUSTERNAME.azurehdinsight.net/jupyter`), ja seejärel käivitage **analüüsi logib koos säde, kasutades kohandatud library.ipynb** märkmiku **PySpark** kaustas.

**Eeltingimused**

Vajate järgmist:

- Azure'i tellimuse. Leiate [Azure'i saada tasuta prooviversioon](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Apache Spark kobar Hdinsightiga Linux. Juhised leiate teemast [loomine Apache Spark kogumite Windows Azure Hdinsightiga sisse](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="save-raw-data-as-an-rdd"></a>Mõne RDD toorandmetega salvestamine

Selles jaotises kasutame tööde töötlemata näidisandmed ja salvestage see taru tabelina seostatud mõne Apache Spark kobar sisse Hdinsightiga [Jupyter](https://jupyter.org) märkmik. Näidisandmete on CSV-faili (hvac.csv) saadaval kõik kogumite vaikimisi sisse.

Kui teie andmed on salvestatud taru tabelina, järgmises jaotises me ühendust taru tabel, nt Power BI ja sellele Ärianalüüsi tööriistade abil.

1. [Azure portaali](https://portal.azure.com/)kaudu startboard, klõpsake paani klaster säde (kui te kinnitatud selle startboard). Samuti saate liikuda klaster jaotises **Sirvi kõiki** > **Hdinsightiga kogumite**.   

2. Keelest säde kobar valige **Kobar armatuurlaud**, ja klõpsake nuppu **Jupyter märkmiku**. Kui kuvatakse vastav viip, sisestage administraatori identimisteave klaster.

    > [AZURE.NOTE] Võib-olla ka jõuate Jupyter märkmiku jaoks klaster, avades järgmine URL brauseri. Klaster nime __CLUSTERNAME__ asendamiseks tehke järgmist.
    >
    > `https://CLUSTERNAME.azurehdinsight.net/jupyter`

2. Looge uus märkmik. Klõpsake nuppu **Uus**ja seejärel klõpsake nuppu **PySpark**.

    ![Jupyter uue märkmiku loomine] (./media/hdinsight-apache-spark-custom-library-website-log-analysis/hdispark.note.jupyter.createnotebook.png "Jupyter uue märkmiku loomine")

3. Uus märkmik on loodud ja nimega Untitled.pynb avada. Klõpsake selle märkmiku nime kohal ja sisestage sõbralik nimi.

    ![Esita märkmiku nimi] (./media/hdinsight-apache-spark-custom-library-website-log-analysis/hdispark.note.jupyter.notebook.name.png "Esita märkmiku nimi")

4. Kuna te abil PySpark tuum märkmiku loonud, peate looma kõik kontekstides otseselt. Säde ja taru konteksti luuakse automaatselt teie eest, kui käivitate kood esimene lahter. Saate alustada vajalike seda tüüpi importimisega. Kleepige järgmine koodilõigu tühi lahter ja vajutage klahvikombinatsiooni **SHIFT + ENTER**.


        from pyspark.sql import Row
        from pyspark.sql.types import *


5. Saate luua mõne RDD kasutamine klaster log Näidisandmete juba saadaval. Pääsete salvestusruumi vaikekonto kobar veebisaidil **\HdiSamples\HdiSamples\WebsiteLogSampleData\SampleLog\909f2b.log**seotud andmed.


        logs = sc.textFile('wasbs:///HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/909f2b.log')


6. Saate tuua valimi Logi seadmine kinnitamise eelmist toimingut lõpule viidud.

        logs.take(5)

    Peaksite nägema järgmine väljund:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        [u'#Software: Microsoft Internet Information Services 8.0',
         u'#Fields: date time s-sitename cs-method cs-uri-stem cs-uri-query s-port cs-username c-ip cs(User-Agent) cs(Cookie) cs(Referer) cs-host sc-status sc-substatus sc-win32-status sc-bytes cs-bytes time-taken',
         u'2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step2.png X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 53175 871 46',
         u'2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step3.png X-ARR-LOG-ID=9eace870-2f49-4efd-b204-0d170da46b4a 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 51237 871 32',
         u'2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step4.png X-ARR-LOG-ID=4bea5b3d-8ac9-46c9-9b8c-ec3e9500cbea 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 72177 871 47']

## <a name="analyze-log-data-using-a-custom-python-library"></a>Kohandatud Python teegi kasutamine Logi andmete analüüsiks

7. Eespool nimetatud väljund mitmel esimese ridu lisada päise teabe ja ülejäänud ridadele vastab kirjeldatud selle päise skeemiga. Sellise logid sõelumine võib olla keeruline. Jah, kasutame kohandatud Python teegi (**iislogparser.py**), mis toodab sõelumine selliste logide märksa lihtsam. Vaikimisi on kaasatud säde klaster Hdinsightiga sisse veebisaidil **/HdiSamples/HdiSamples/WebsiteLogSampleData/iislogparser.py**see teek.

    See teek pole siiski tekstivormingus soovitud `PYTHONPATH` nii, et me ei saa kasutada näiteks lauset impordi abil `import iislogparser`. Selle teegi kasutamiseks me levitamine selle töötaja sõlmi. Käivitage järgmised koodilõigu.


        sc.addPyFile('wasbs:///HdiSamples/HdiSamples/WebsiteLogSampleData/iislogparser.py')


9. `iislogparser`pakub funktsiooni `parse_log_line` , mis tagastab `None` kui log rida on päiserida ja tagastab selle `LogLine` klassi log joone esinemisel. Kasutage funktsiooni `LogLine` klassi ainult log ridade eraldamiseks RDD-d:

        def parse_line(l):
            import iislogparser
            return iislogparser.parse_log_line(l)
        logLines = logs.map(parse_line).filter(lambda p: p is not None).cache()


10. Saate tuua paari ekstraktitud log rea kinnitamiseks etappi lõpule viidud.

        logLines.take(2)

    Väljund peaks olema umbes järgmine:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        [2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step2.png X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 53175 871 46,
        2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step3.png X-ARR-LOG-ID=9eace870-2f49-4efd-b204-0d170da46b4a 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 51237 871 32]


11. Funktsiooni `LogLine` klassi omakorda on mõned kasulikud meetodid, nt `is_error()`, mis tagastab kas logikirjet on tõrkekood. Selle abil saate arvutada vigade ekstraktitud log ridade arvu ja seejärel logige kõik tõrked erinevad faili.

        errors = logLines.filter(lambda p: p.is_error())
        numLines = logLines.count()
        numErrors = errors.count()
        print 'There are', numErrors, 'errors and', numLines, 'log entries'
        errors.map(lambda p: str(p)).saveAsTextFile('wasbs:///HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/909f2b-2.log')

    Peaksite nägema umbes selline väljund:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        There are 30 errors and 646 log entries

12. **Matplotlib** abil saate koostada andmete visualiseerimise. Näiteks kui soovite eristamiseks põhjus nõuab, et käivitada pikka aega, võite leida faile, mis ei teeni keskmiselt kõige rohkem aega võtta.
Allpool koodilõigu toob 25 ülemisel ressursse, mis leidsid enamik ajaks taotluse.

        def avgTimeTakenByKey(rdd):
            return rdd.combineByKey(lambda line: (line.time_taken, 1),
                                    lambda x, line: (x[0] + line.time_taken, x[1] + 1),
                                    lambda x, y: (x[0] + y[0], x[1] + y[1]))\
                      .map(lambda x: (x[0], float(x[1][0]) / float(x[1][1])))
            
        avgTimeTakenByKey(logLines.map(lambda p: (p.cs_uri_stem, p))).top(25, lambda x: x[1])

    Peaksite nägema umbes selline väljund:

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        [(u'/blogposts/mvc4/step13.png', 197.5),
         (u'/blogposts/mvc2/step10.jpg', 179.5),
         (u'/blogposts/extractusercontrol/step5.png', 170.0),
         (u'/blogposts/mvc4/step8.png', 159.0),
         (u'/blogposts/mvcrouting/step22.jpg', 155.0),
         (u'/blogposts/mvcrouting/step3.jpg', 152.0),
         (u'/blogposts/linqsproc1/step16.jpg', 138.75),
         (u'/blogposts/linqsproc1/step26.jpg', 137.33333333333334),
         (u'/blogposts/vs2008javascript/step10.jpg', 127.0),
         (u'/blogposts/nested/step2.jpg', 126.0),
         (u'/blogposts/adminpack/step1.png', 124.0),
         (u'/BlogPosts/datalistpaging/step2.png', 118.0),
         (u'/blogposts/mvc4/step35.png', 117.0),
         (u'/blogposts/mvcrouting/step2.jpg', 116.5),
         (u'/blogposts/aboutme/basketball.jpg', 109.0),
         (u'/blogposts/anonymoustypes/step11.jpg', 109.0),
         (u'/blogposts/mvc4/step12.png', 106.0),
         (u'/blogposts/linq8/step0.jpg', 105.5),
         (u'/blogposts/mvc2/step18.jpg', 104.0),
         (u'/blogposts/mvc2/step11.jpg', 104.0),
         (u'/blogposts/mvcrouting/step1.jpg', 104.0),
         (u'/blogposts/extractusercontrol/step1.png', 103.0),
         (u'/blogposts/sqlvideos/sqlvideos.jpg', 102.0),
         (u'/blogposts/mvcrouting/step21.jpg', 101.0),
         (u'/blogposts/mvc4/step1.png', 98.0)]


13. Samuti saate esitada selle teabe diagrammi kujul. Esimese sammuna lisamine diagrammi loomiseks, andke meile kõigepealt looma ajutise tabeli **AverageTime**. Tabeli rühmitatakse logid aeg, et näha, kui parajasti olid mis tahes ebatavalised latentsus diagrammi järgi.

        avgTimeTakenByMinute = avgTimeTakenByKey(logLines.map(lambda p: (p.datetime.minute, p))).sortByKey()
        schema = StructType([StructField('Minutes', IntegerType(), True),
                             StructField('Time', FloatType(), True)])
                             
        avgTimeTakenByMinuteDF = sqlContext.createDataFrame(avgTimeTakenByMinute, schema)
        avgTimeTakenByMinuteDF.registerTempTable('AverageTime')

14. Seejärel käivitada järgmine SQL-päringu saada kõik kirjed **AverageTime** tabelis.

        %%sql -o averagetime
        SELECT * FROM AverageTime

    Funktsiooni `%%sql` maagiline, millele järgneb `-o averagetime` tagab, et päringu väljund püsis kohalikult Jupyter server (tavaliselt headnode klaster). Väljund on jätkunud [Pandas](http://pandas.pydata.org/) dataframe koos määratud nimi **averagetime**nimega.

    Peaksite nägema umbes selline väljund:

    ![SQL-päringu väljund] (./media/hdinsight-apache-spark-custom-library-website-log-analysis/sql.output.png "SQL-päringu väljund")

    Lisateavet selle `%%sql` maagiline, samuti muud magics saadaval PySpark tuum, lugege teemat [tuumad Jupyter märkmikke säde Hdinsightiga kogumite saadaval](hdinsight-apache-spark-jupyter-notebook-kernels.md#why-should-i-use-the-new-kernels).

15. Nüüd saate Matplotlib kasutatud andmete visualiseerimine teegi lisamine diagrammi loomiseks. Kuna soovitud diagrammi tuleb luua kohalikult nõutud **averagetime** dataframe, koodilõigu peab algama selle `%%local` maagiline. See tagab kood käivitatakse kohalikult Jupyter serveris.

        %%local
        %matplotlib inline
        import matplotlib.pyplot as plt
        
        plt.plot(averagetime['Minutes'], averagetime['Time'], marker='o', linestyle='--')
        plt.xlabel('Time (min)')
        plt.ylabel('Average time taken for request (ms)')

    Peaksite nägema umbes selline väljund:

    ![Matplotlib väljund] (./media/hdinsight-apache-spark-custom-library-website-log-analysis/hdi-apache-spark-web-log-analysis-plot.png "Matplotlib väljund")

16. Kui olete lõpetanud, töötavad rakendused, peaksite sulgumist avatav märkmik Väljalaske ressursside. Selleks märkmikku menüü **fail** nuppu **Sule ja peatada**. See on Sule ja Sule märkmik.
    

## <a name="seealso"></a>Vt ka


* [Ülevaade: Apache Spark klõpsake Azure Hdinsightiga](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Stsenaariumid

* [Bi säde: andmeanalüüside interaktiivsed Hdinsightiga säde kasutamine koos Ärianalüüsi tööriistade kohta](hdinsight-apache-spark-use-bi-tools.md)

* [Seadme õppimisega säde: kasutamine säde rakenduses Hdinsightiga building temperatuur HVAC andmete analüüsimiseks](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

* [Seadme õppimisega säde: kasutamine säde Hdinsightiga prognoosida toiduga kontrollitulemuste rakenduses](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

* [Säde Streaming: Kasutamine säde rakenduses reaalajas streaming rakenduste Hdinsightiga](hdinsight-apache-spark-eventhub-streaming.md)

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
