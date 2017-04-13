<properties 
    pageTitle="Säde kobar Hdinsightiga Linux Zeppelin märkmike abil | Microsoft Azure'i" 
    description="Üksikasjalikud juhised, kuidas Zeppelin märkmike kasutamiseks säde kogumite Hdinsightiga Linux." 
    services="hdinsight" 
    documentationCenter="" 
    authors="nitinme" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="hdinsight" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/05/2016" 
    ms.author="nitinme"/>


# <a name="use-zeppelin-notebooks-with-apache-spark-cluster-on-hdinsight-linux"></a>Apache Spark kobar Hdinsightiga Linux Zeppelin märkmike kasutamine

Hdinsightiga säde kogumite kaasata Zeppelin märkmikku, mille abil saate käivitada säde tööd. Selles artiklis saate teada, kuidas kasutada Zeppelin märkmik on Hdinsightiga kobar.


**Eeltingimused**

* Azure'i tellimuse. Leiate [Azure'i saada tasuta prooviversioon](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

* Mõne Apache Spark kobar. Juhised leiate teemast [loomine Apache Spark kogumite Windows Azure Hdinsightiga sisse](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="launch-a-zeppelin-notebook"></a>Käivitage Zeppelin märkmiku

1. Keelest säde kobar valige **Kobar armatuurlaud**, ja klõpsake nuppu **Zeppelin märkmiku**. Kui kuvatakse vastav viip, sisestage administraatori identimisteave klaster.

    > [AZURE.NOTE] Võib-olla ka jõuate Zeppelin märkmiku jaoks klaster, avades järgmine URL brauseri. Klaster nime __CLUSTERNAME__ asendamiseks tehke järgmist.
    >
    > `https://CLUSTERNAME.azurehdinsight.net/zeppelin`

2. Looge uus märkmik. Päise paanil klõpsake **märkmiku**ja **Luua uue märkme loomiseks**klõpsake.

    ![Zeppelin uue märkmiku loomine] (./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.createnewnote.png "Zeppelin uue märkmiku loomine")

    Sisestage märkmiku nimi ja seejärel klõpsake nuppu **Lisa loomine**.

3. Lisaks veenduge, et märkmiku päis kuvatakse ühendatud olek. See on tähistatud rohelise täpil paremas ülanurgas.

    ![Zeppelin märkmiku olek] (./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.newnote.connected.png "Zeppelin märkmiku olek")

4. Näidisandmete laadimine ajutine tabel. Hdinsightiga säde kobar loomisel kopeeritakse valimi andmefaili **hvac.csv**, jaotises **\HdiSamples\SensorSampleData\hvac**kontoga seostatud salvestusruumi.

    Tühi lõik, mis on loodud uue märkmiku vaikimisi, kleepige järgmine koodilõigu.

        %livy.spark
        //The above magic instructs Zeppelin to use the Livy Scala interpreter

        // Create an RDD using the default Spark context, sc
        val hvacText = sc.textFile("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
        
        // Define a schema
        case class Hvac(date: String, time: String, targettemp: Integer, actualtemp: Integer, buildingID: String)
        
        // Map the values in the .csv file to the schema
        val hvac = hvacText.map(s => s.split(",")).filter(s => s(0) != "Date").map(
            s => Hvac(s(0), 
                    s(1),
                    s(2).toInt,
                    s(3).toInt,
                    s(6)
            )
        ).toDF()
        
        // Register as a temporary table called "hvac"
        hvac.registerTempTable("hvac")
        
    Vajutage klahvikombinatsiooni **SHIFT + ENTER** või lõigu väljavõte käivitamiseks nuppu **Esita** . Klõpsake paremas ülanurgas lõigu olek peaks edu valmis ootel, töötab lõpetatud. Sama lõigu allservas kuvatakse väljund. Kuvatõmmise näeb välja umbes järgmine:

    ![Looge uus ajutine tabel lähteandmed] (./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.note.loaddDataintotable.png "Looge uus ajutine tabel lähteandmed")

    Saate sisestada ka iga lõigu tiitel. Parempoolses ülanurgas, klõpsake nuppu **sätted** ja seejärel käsku **Kuva tiitel**.

5. Nüüd saate säde SQL-lauseid **hvac** tabeli. Uue lõigu kleepige järgmine päring. Päringu toob building ID "ja" target ja tegeliku temperatuuride iga hoone antud kuupäeva vahe. Vajutage klahvikombinatsiooni **SHIFT + ENTER**.

        %sql
        select buildingID, (targettemp - actualtemp) as temp_diff, date from hvac where date = "6/1/13" 

    **% Sql** -lause alguses ütleb märkmikku kasutama Liviuse Scala Tõlgi.

    Järgmine pilt kuvatakse väljund.

    ![Käivitage säde SQL-lauses märkmiku abil] (./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.note.sparksqlquery1.png "Käivitage säde SQL-lauses märkmiku abil")

     Klõpsake sama väljundi jaoks eri esinduste vaheldumisi kuvamissuvandid (esile tõstetud ristkülikut). Klõpsake nuppu **sätted** , et valida milliseid consitutes võtme ja väärtused väljund. Ekraanipildi kohal kasutab **buildingID** võti ja **temp_diff** keskmise väärtusena.

    
6. Samuti saate käivitada säde SQL-lauseid muutujate kasutamine päringu. Järgmise koodilõigu näitab, kuidas määrata muutuja, **Temp**, päringu võimalikud väärtused soovite päringu abil. Päringu esmakordsel käivitamisel drop-down lisatakse automaatselt teie määratud muutuja väärtused.

        %sql
        select buildingID, date, targettemp, (targettemp - actualtemp) as temp_diff from hvac where targettemp > "${Temp = 65,65|75|85}" 

    Kleepige see koodilõigu uue lõigu ja vajutage klahvikombinatsiooni **SHIFT + ENTER**. Järgmine pilt kuvatakse väljund.

    ![Käivitage säde SQL-lauses märkmiku abil] (./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.note.sparksqlquery2.png "Käivitage säde SQL-lauses märkmiku abil")

    Edaspidised päringuid, saate valida rippmenüüst uus väärtus ja käivitage päring uuesti. Klõpsake nuppu **sätted** , et valida milliseid consitutes võtme ja väärtused väljund. Ülaltoodud ekraanipildi kasutab **buildingID** võti, keskmise väärtusena **temp_diff** ja **targettemp** rühma nimega.

7. Taaskäivitage rakendus väljumiseks Liviuse Tõlgi. Selleks Tõlgi sätete avamine, klõpsates sisseloginud paremas ülanurgas kasutaja nime ja seejärel käsku **Tõlgi**.

    ![Käivitage Tõlgi] (./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Taru väljund")

2. Liikuge kerides Liviuse Tõlgi sätted ja klõpsake **uuesti**.

    ![Taaskäivitage Liviuse intepreter] (./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.zeppelin.restart.interpreter.png "Taaskäivitage Zeppelin intepreter")

## <a name="how-do-i-use-external-packages-with-the-notebook"></a>Kuidas kasutamine väliste pakettide märkmiku?

Saate konfigureerida Zeppelin märkmiku Apache Spark kobar klõpsake Hdinsightiga (Linux) kasutada välise, ühenduse kaasa pakette, mis pole kaasatud välja-ja--box klaster. Saate otsida [Maven hoidla](http://search.maven.org/) jaoks saadaolevate pakettide täieliku loendi. Samuti saate loendit saadaval pakettide muudest allikatest. Näiteks ühenduse kaasa paketid täieliku loendi on saadaval veebisaidil [Säde paketid](http://spark-packages.org/).

Selles artiklis kuvatakse Jupyter märkmikuga [säde-csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) -paketi kasutamise kohta.

1. Avage sätted Tõlgi. Paremas ülanurgas, klõpsake sisseloginud kasutaja nimi ja seejärel klõpsake nuppu **Tõlgi**.

    ![Käivitage Tõlgi] (./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Taru väljund")

2. Liikuge Liviuse Tõlgi sätted ja seejärel klõpsake nuppu **Redigeeri**.

    ![Tõlgi sätete muutmine] (./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-use-external-package-1.png "Tõlgi sätete muutmine")

3. Lisage uue tootenumbri, nimetatakse **livy.spark.jars.packages** ja määrake selle väärtuseks vormingus `group:id:version`. Jah, kui soovite kasutada [säde-csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) , peate määrama võti väärtus `com.databricks:spark-csv_2.10:1.4.0`.

    ![Tõlgi sätete muutmine] (./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-use-external-package-2.png "Tõlgi sätete muutmine")

    Klõpsake nuppu **Salvesta** ja seejärel taaskäivitage Liviuse Tõlgi.

4. **Näpunäide**: kui soovite, et aru saada, kuidas jõuda võti väärtus sisestatud kohale, siin on kuidas.

    lisamine. Leidke paketi Maven hoidla. Selles õpetuses kasutasime [säde-csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar).
    
    b. Hoidla, Koguge väärtused **GroupId**, **ArtifactId**ja **versioon**.

    ![Kasutage välise paketid Jupyter märkmik] (./media/hdinsight-apache-spark-zeppelin-notebook/use-external-packages-with-jupyter.png "Kasutage välise paketid Jupyter märkmik")

    c. CONCATENATE kolme väärtused, eraldades need koolon (****:).

        com.databricks:spark-csv_2.10:1.4.0

## <a name="where-are-the-zeppelin-notebooks-saved"></a>Kuhu salvestatakse Zeppelin märkmikud?

Märkmike Zeppelin salvestatakse kobar headnodes. Nii, et klaster kustutamisel kustutatakse ka märkmikud. Kui te ei soovi säilitada märkmikke hilisemaks kasutamiseks klõpsake muude kogumite, tuleb need pärast lõpetamist tööde eksportida. Märkmiku eksportimiseks klõpsake nuppu **ekspordi** ikooni nagu on näidatud järgmisel pildil.

![Märkmiku allalaadimine] (./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-download-notebook.png "Märkmiku allalaadimine")

See salvestab märkmiku JSON-failina oma allalaadimiskoht.

## <a name="livy-session-management"></a>Liviuse seansi haldus

Märkmikus Zeppelin esimese koodi lõigu käivitamisel luuakse Liviuse uue seansi klaster Hdinsightiga säde. Selle seansi jagatakse kõigi edaspidi loodavate Zeppelin märkmikust. Kui mingil põhjusel tapsid Liviuse, mis on seanss (kobar taaskäivitage arvuti jne), ei saa käivitada töö Zeppelin märkmik.

Sellisel juhul peab tegema järgmised toimingud enne alustamist tööde käitamist Zeppelin märkmiku. 

1. Taaskäivitage Zeppelin märkmiku Liviuse Tõlgi. Selleks Tõlgi sätete avamine, klõpsates sisseloginud paremas ülanurgas kasutaja nime ja seejärel käsku **Tõlgi**.

    ![Käivitage Tõlgi] (./media/hdinsight-apache-spark-zeppelin-notebook/zeppelin-launch-interpreter.png "Taru väljund")

2. Liikuge kerides Liviuse Tõlgi sätted ja klõpsake **uuesti**.

    ![Taaskäivitage Liviuse intepreter] (./media/hdinsight-apache-spark-zeppelin-notebook/hdispark.zeppelin.restart.interpreter.png "Taaskäivitage Zeppelin intepreter")

3. Olemasoleva märkmiku Zeppelin koodi lahtri käivitada. See loob uue Liviuse seansiga Hdinsightiga kobar.

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

* [Tuumad Jupyter märkmiku säde kobar Hdinsightiga jaoks saadaval](hdinsight-apache-spark-jupyter-notebook-kernels.md)

* [Välise pakettide Jupyter märkmike kasutamine](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)

* [Jupyter oma arvutisse installida ja luua ühenduse mõne Hdinsightiga säde kobar](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Ressursside haldamine

* [Ressursid Apache Spark kobar rakenduses Windows Azure Hdinsightiga haldamine](hdinsight-apache-spark-resource-manager.md)

* [Töötavate on Apache Spark kobar rakenduses Hdinsightiga jälitamine ja silumine tööde haldamine](hdinsight-apache-spark-job-debugging.md)


[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-management-portal]: https://manage.windowsazure.com/
[azure-create-storageaccount]: storage-create-storage-account.md 







