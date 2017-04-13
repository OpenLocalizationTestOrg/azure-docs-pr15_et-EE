<properties
    pageTitle="Edastab säde tööd kaugkustutada Liviuse | Microsoft Azure'i"
    description="Saate teada, kuidas kasutada Liviuse Hdinsightiga kogumite esitada säde töid kaugühenduse teel."
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


# <a name="submit-spark-jobs-remotely-to-an-apache-spark-cluster-on-hdinsight-linux-using-livy"></a>Edastab säde tööd kaugühenduse teel on Apache Spark kobar Hdinsightiga Linux Liviuse abil

Klõpsake Windows Azure Hdinsightiga Apache Spark kobar sisaldab Liviuse ülejäänud kasutajaliidese esitamise kaugühenduse teel, et säde kobar. Üksikasjalikud dokumendid, vaadake [Liviuse](https://github.com/cloudera/hue/tree/master/apps/spark/java#welcome-to-livy-the-rest-spark-server).

Liviuse abil saate käivitada interaktiivsed säde kestad või esitage pakett-tööde käivitamiseks säde. Selles artiklis kirjeldatakse esitada pakett-tööde Liviuse kasutamise kohta. Allpool süntaks kasutab Curl ülejäänud helistada Liviuse lõpp-punkti.

**Eeltingimused**

Vajate järgmist:

- Azure'i tellimuse. Leiate [Azure'i saada tasuta prooviversioon](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Apache Spark kobar Hdinsightiga Linux. Juhised leiate teemast [loomine Apache Spark kogumite Windows Azure Hdinsightiga sisse](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="submit-a-batch-job-the-cluster"></a>Esitage pakett klaster

Enne pakett-töö, tuleb üles laadida rakenduse jar kobar talletamist seostatud klaster kohta. [**AzCopy**](../storage/storage-use-azcopy.md), käsurea kasuliku, saate seda teha. On palju teiste klientide abil saate üles laadida andmeid. Võite leida rohkem nende kohta [üles laadida andmete Hadoopi töökohta, Hdinsightiga](hdinsight-upload-data.md).

    curl -k --user "<hdinsight user>:<user password>" -v -H <content-type> -X POST -d '{ "file":"<path to application jar>", "className":"<classname in jar>" }' 'https://<spark_cluster_name>.azurehdinsight.net/livy/batches'

**Näited**:

* Kui jar fail on kobar salvestusruumi (WASB)

        curl -k --user "admin:mypassword1!" -v -H 'Content-Type: application/json' -X POST -d '{ "file":"wasbs://mycontainer@mystorageaccount.blob.core.windows.net/data/SparkSimpleTest.jar", "className":"com.microsoft.spark.test.SimpleFile" }' "https://mysparkcluster.azurehdinsight.net/livy/batches"

* Kui on, mida soovite edastada jar faili nimi ja selle klassinimi osana Sisestuskeel faili (selles näites input.txt)

        curl -k  --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\input.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

## <a name="get-information-on-batches-running-on-the-cluster"></a>Töötavate klaster pakettidena kohta teabe saamine

    curl -k --user "<hdinsight user>:<user password>" -v -X GET "https://<spark_cluster_name>.azurehdinsight.net/livy/batches"

**Näited**:

* Kui soovite kõik töölehed, töötavate klaster toomiseks.

        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches"

* Kui soovite alla laadida teatud paketi antud batchId abil

        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches/{batchId}"


## <a name="delete-a-batch-job"></a>Töö paketi kustutamine

    curl -k --user "<hdinsight user>:<user password>" -v -X DELETE "https://<spark_cluster_name>.azurehdinsight.net/livy/batches/{batchId}"

**Näide**:

    curl -k --user "admin:mypassword1!" -v -X DELETE "https://mysparkcluster.azurehdinsight.net/livy/batches/{batchId}"

## <a name="livy-and-high-availability"></a>Liviuse ja kõrge-saadavus

Liviuse pakub kõrge-saadavus säde klaster töid. Siin on mõned näited.

* Kui Liviuse teenuse läheb alla, kui olete saatnud töö eemalt säde kobar, töö jätkub taustal. Liviuse korral varundamine taastatakse olekut töö ja aruandeid seade uuesti.

* Jupyter märkmikud Hdinsightiga on tootja Liviuse klõpsake soovitud taustväärtus. Kui märkmik töötab säde töö ja Liviuse teenuse saab uuesti käivitada, edasi märkmiku lahtrid koodi käivitamiseks. 

## <a name="show-me-an-example"></a>Kuva mulle näide

Selles jaotises vaatame näited Liviuse säde rakenduse esitada, rakenduse edenemist jälgida ja seejärel kustutage töö kasutamise kohta. Selles näites kasutame rakendus on üks välja töötatud [Scala rakendusena ja käivitada Hdinsightiga säde kobar](hdinsight-apache-spark-create-standalone-application.md)loomine. Alltoodud juhiseid endale järgmist:

* Juba kopeeritud üle rakenduse jar salvestusruumi kontoga seostatud klaster.
* Teil on installitud arvutis, kus on nende juhiste järgimist CuRL.

Tehke järgmist.

1. Andke meile esmalt veenduge, et Liviuse töötab klaster. Oleme selleks saada installitud pakettidena loend. Kui teil on tööd kasutades Liviuse esimest korda, see peaks tagastama null.

        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches"

    Saate väljund sarnaneb järgmisega:

        < HTTP/1.1 200 OK
        < Content-Type: application/json; charset=UTF-8
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Fri, 20 Nov 2015 23:47:53 GMT
        < Content-Length: 34
        <
        {"from":0,"total":0,"sessions":[]}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact

    Pange tähele, kuidas ütleb väljund Viimane rida **kogusumma: 0**, mis viitab pole käivitatud töölehed.

2. Andke meile kohe esitada pakett-töö. Allpool koodilõigu kasutab Sisestuskeel faili (input.txt) edasi jar nimi ja selle tunni nime parameetrid. See on soovitatav, kui kasutate opsüsteemi Windows arvutis järgmist.

        curl -k --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\input.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

    Faili **input.txt** parameetrid on määratletud järgmiselt:

        { "file":"wasbs:///example/jars/SparkSimpleApp.jar", "className":"com.microsoft.spark.example.WasbIOTest" }

    Peaksite nägema järgmine väljund:

        < HTTP/1.1 201 Created
        < Content-Type: application/json; charset=UTF-8
        < Location: /0
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Fri, 20 Nov 2015 23:51:30 GMT
        < Content-Length: 36
        <
        {"id":0,"state":"starting","log":[]}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact

    Pöörake tähelepanu sellele, kuidas väljundisse Viimane rida ütleb **olek: alates**. Samuti ütleb **id: 0**. See on paketi ID-ga.

3. Nüüd saate alla laadida selle oleku selles teatud paketi abil paketi ID-ga.

        curl -k --user "admin:mypassword1!" -v -X GET "https://mysparkcluster.azurehdinsight.net/livy/batches/0"

    Peaksite nägema järgmine väljund:

        < HTTP/1.1 200 OK
        < Content-Type: application/json; charset=UTF-8
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Fri, 20 Nov 2015 23:54:42 GMT
        < Content-Length: 509
        <
        {"id":0,"state":"success","log":["\t diagnostics: N/A","\t ApplicationMaster host: 10.0.0.4","\t ApplicationMaster RPC port: 0","\t queue: default","\t start time: 1448063505350","\t final status: SUCCEEDED","\t tracking URL: http://hn0-myspar.lpel1gnnvxne3gwzqkfq5u5uzh.jx.internal.cloudapp.net:8088/proxy/application_1447984474852_0002/","\t user: root","15/11/20 23:52:47 INFO Utils: Shutdown hook called","15/11/20 23:52:47 INFO Utils: Deleting directory /tmp/spark-b72cd2bf-280b-4c57-8ceb-9e3e69ac7d0c"]}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact

    Väljundisse kuvatakse nüüd **olekus: edu**, mis näitab, et töö on edukalt lõpule viidud.

4. Kui soovite, saate nüüd paketi kustutada.

        curl -k --user "admin:mypassword1!" -v -X DELETE "https://mysparkcluster.azurehdinsight.net/livy/batches/0"

    Peaksite nägema järgmine väljund:

        < HTTP/1.1 200 OK
        < Content-Type: application/json; charset=UTF-8
        < Server: Microsoft-IIS/8.5
        < X-Powered-By: ARR/2.5
        < X-Powered-By: ASP.NET
        < Date: Sat, 21 Nov 2015 18:51:54 GMT
        < Content-Length: 17
        <
        {"msg":"deleted"}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact

    Viimane rida väljund näitab, et paketi kustutamine õnnestus. Kui kustutate töö töötamise ajal, siis tapavad põhiosas töö. Kui kustutate tööd, mis on lõpule viidud või muul viisil kustutab töö kohta täielikult.

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
