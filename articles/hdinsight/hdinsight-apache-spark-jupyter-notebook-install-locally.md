<properties 
    pageTitle="Jupyter märkmik oma arvutisse installida ja luua ühenduse mõne Hdinsightiga säde kobar | Microsoft Azure'i" 
    description="Lisateave Jupyter märkmiku kohalikult oma arvutisse installida ja luua ühenduse mõne Apache Spark kobar Windows Azure Hdinsightiga kohta." 
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
    ms.date="09/26/2016" 
    ms.author="nitinme"/>


# <a name="install-jupyter-notebook-on-your-computer-and-connect-to-apache-spark-cluster-on-hdinsight-linux"></a>Jupyter märkmik oma arvutisse installida ja luua ühenduse Hdinsightiga Linux Apache Spark kobar

Sellest artiklist saate teada, kuidas installida Jupyter märkmikku, kohandatud PySpark (jaoks Python) ja (Scala) sütitaja tuumad koos säde maagiline ja märkmiku ühenduse loomine mõne Hdinsightiga kobar. Ei saa mitmel põhjusel Jupyter installimiseks oma kohalikus arvutis ja ei saa teatud probleemidega kui ka. Põhjused, miks ja probleemide loendi leiate jaotisest [miks peaksite installima Jupyter minu arvutis](#why-should-i-install-jupyter-on-my-computer) käesoleva artikli lõpus.

Kolmeks on seotud Jupyter ja säde maagiline installite oma arvutisse.

* Installige Jupyter märkmik
* Installige PySpark ja säde tuumad säde maagiline
* Säde maagiline säde kobar klõpsake Hdinsightiga juurdepääsu konfigureerimine

Kohandatud tuumad ja säde maagiline saadaval Jupyter märkmikke Hdinsightiga kobar kohta leiate lisateavet teemast [tuumad Jupyter märkmikke Apache Spark Linux kogumite kohta Hdinsightiga jaoks saadaval](hdinsight-apache-spark-jupyter-notebook-kernels.md).

##<a name="prerequisites"></a>Eeltingimused

Siin loetletud eeltingimused ei ole installimist Jupyter. Need on ühenduse Jupyter märkmik on Hdinsightiga kobar kui märkmik on installitud.

- Azure'i tellimuse. Leiate [Azure'i saada tasuta prooviversioon](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Apache Spark kobar Hdinsightiga Linux. Juhised leiate teemast [loomine Apache Spark kogumite Windows Azure Hdinsightiga sisse](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="install-jupyter-notebook-on-your-computer"></a>Jupyter märkmik oma arvutisse installida

Peate installima Python enne installimist Jupyter märkmikud. On saadaval nii Python ja Jupyter [Ananconda jaotuse](https://www.continuum.io/downloads)osana. Kui installite Anaconda, installimist tegelikult jaotumine Python. Kui Anaconda on installitud, saate lisada Jupyter installimise käsu. Sellest jaotisest leiate juhised, mida peaksite järgima.

1. Laadige alla oma platvormile [Anaconda installer](https://www.continuum.io/downloads) ja Käivita häälestamine. Ajal häälestamise viisardit, veenduge, et suvand Anaconda lisamiseks oma tee muutujana.

2. Käivitage järgmine käsk Jupyter installimiseks.

        conda install jupyter

    Installting Jupyter kohta leiate lisateavet teemast [Installimise Jupyter Anaconda abil](http://jupyter.readthedocs.io/en/latest/install.html).

## <a name="install-the-kernels-and-spark-magic"></a>Tuumad ja säde maagiline installimine

Juhised säde maagiline installimise kohta leiate teemast PySpark ja säde tuumad, [sparkmagic dokumentatsiooni](https://github.com/jupyter-incubator/sparkmagic#installation) github.

## <a name="configure-spark-magic-to-access-the-hdinsight-spark-cluster"></a>Säde maagiline Hdinsightiga säde kobar juurdepääsu konfigureerimine

Selles jaotises saate konfigureerida säde maagiline ühenduse Apache Spark kobar, peab olema juba loodud Windows Azure Hdinsightiga varem installitud.

1. Jupyter konfiguratsiooni teave talletatakse tavaliselt kasutajate home kataloog. Mis tahes OS platvormi home kataloogi leidmiseks tippige järgmised käsud.

    Käivitage Python kest. Käsuviiba aken, sisestage järgmine teave.

        python

    Python kest, sisestage järgmine käsk, et teada saada, home directory.

        import os
        print(os.path.expanduser('~'))

2. Navigeerige home kataloogi ja looge kaust nimega **.sparkmagic** , kui see pole juba olemas.

3. Kausta, faili nimega **config.json** loomine ja lisage järgmised JSON koodilõigu sees.

        {
          "kernel_python_credentials" : {
            "username": "{USERNAME}",
            "base64_password": "{BASE64ENCODEDPASSWORD}",
            "url": "https://{CLUSTERDNSNAME}.azurehdinsight.net/livy"
          },
          "kernel_scala_credentials" : {
            "username": "{USERNAME}",
            "base64_password": "{BASE64ENCODEDPASSWORD}",
            "url": "https://{CLUSTERDNSNAME}.azurehdinsight.net/livy"
          }
        }

4. Asendage väärtused kaust **{USERNAME}**, **{CLUSTERDNSNAME}**ja **{BASE64ENCODEDPASSWORD}** . Arvu Utiliidid oma lemmik programmeerimiskeel või võrgus abil saate Loo base64 kodeeritud parooli tegelikult parool. Oleks lihtne Python koodilõigu käivitamiseks oma Käsuviip:

        python -c "import base64; print(base64.b64encode('{YOURPASSWORD}'))"

5. Käivitage Jupyter. Kasutage Käsuviip järgmine käsk.

        jupyter notebook

6. Veenduge, et saate luua ühenduse abil Jupyter märkmiku klaster ja et saate säde maagiline saadaval tuumad. Tehke järgmist.

    1. Looge uus märkmik. Paremas nurgas nuppu **Uus**. Peaksite nägema vaikimisi tuum **Python2** ja kaks uut tuumade mis installimist **PySpark** ja **säde**.

        ![Jupyter uue märkmiku loomine] (./media/hdinsight-apache-spark-jupyter-notebook-install-locally/jupyter-kernels.png "Jupyter uue märkmiku loomine")

    
        Klõpsake **PySpark**.


    2. Käivitage järgmised koodilõigu.

            %%sql
            SELECT * FROM hivesampletable LIMIT 5

        Kui saate tuua edukalt väljund, testitakse Hdinsightiga klaster ühendust.

    >[AZURE.TIP] Kui soovite värskendada märkmiku konfiguratsiooni ühenduse loomiseks muu kobar, värskendage soovitud config.json uus komplekt väärtused, nagu on näidatud sammus 3 ülaltoodud. 

## <a name="why-should-i-install-jupyter-on-my-computer"></a>Miks peaks Jupyter oma arvutisse installida?

Ei saa arvu põhjust, miks võiksite Jupyter oma arvutisse installida ja ühendage see säde kobar Hdinsightiga kohta.

* Ehkki Jupyter märkmikud on juba olemas säde kobar rakenduses Windows Azure Hdinsightiga, Jupyter oma arvutisse installimise pakub teil võimalus luua märkmikke, testige rakendust esitada töötava kobar ja laadige klaster märkmikud. Märkmikud üleslaadimiseks klaster, saate üles laadida Jupyter märkmik, kus töötab või klaster abil või neid seotud klaster salvestusruumi konto /HdiNotebooks kausta salvestada. Klaster märkmike salvestamise kohta leiate lisateavet teemast [kus Jupyter märkmikke talletatakse](hdinsight-apache-spark-jupyter-notebook-kernels.md#where-are-the-notebooks-stored)?
* Saadaolevate märkmike abil, saate luua ühenduse erinevate säde kogumite vastavalt oma rakenduse nõue.
* Saate rakendada andmeallika juhtelemendi ja märkmikud versioon juhtelement on GitHub. Saate määrata ka koostööpõhise keskkonnas, kus mitu kasutajat saab töötada samas märkmikus.
* Saate töötada märkmike kohalikult klaster isegi ilma. Teil on vaja ainult klaster testimiseks märkmikke suhtes, mitte juhtida käsitsi märkmike või mõne arenduskeskkond.
* See võib olla lihtsam konfigureerimine oma kohaliku arenduskeskkond, kui see on klaster Jupyter installimist konfigureerida.  Saate ära kõik tarkvara on installitud kohalikult ühe või mitme remote kogumite konfigureerimata.

>[AZURE.WARNING] Koos Jupyter kohalikku arvutisse installitud, käivitada mitme kasutaja samas märkmikus sama säde kobar samal ajal. Sellisel juhul luuakse mitme Liviuse seansid. Kui teil tekib probleeme ja silumine, mida soovite, oleks keerukas toiming, mis Liviuse seansi jälgimiseks kuulub kasutaja.




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

* [Välise pakettide Jupyter märkmike kasutamine](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)

### <a name="manage-resources"></a>Ressursside haldamine

* [Ressursid Apache Spark kobar rakenduses Windows Azure Hdinsightiga haldamine](hdinsight-apache-spark-resource-manager.md)

* [Töötavate on Apache Spark kobar rakenduses Hdinsightiga jälitamine ja silumine tööde haldamine](hdinsight-apache-spark-job-debugging.md)
