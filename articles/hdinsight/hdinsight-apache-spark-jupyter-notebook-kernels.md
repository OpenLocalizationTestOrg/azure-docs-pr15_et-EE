<properties 
    pageTitle="Saadaval Jupyter märkmikud Hdinsightiga säde tuumad kogumite Linux | Microsoft Azure'i" 
    description="Lisateavet täiendavate Jupyter märkmiku tuumad säde kobar Hdinsightiga Linux saadaval." 
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


# <a name="kernels-available-for-jupyter-notebooks-with-apache-spark-clusters-on-hdinsight-linux"></a>Tuumad Jupyter märkmikke Apache Spark kogumite Hdinsightiga Linuxi jaoks saadaval

Apache Spark kobar klõpsake Hdinsightiga (Linux) sisaldab rakenduste testimiseks kasutatavad Jupyter märkmikud. On tuum on programm, mis käivitub ja tõlgendab oma kood. Hdinsightiga säde kogumite annavad kahe tuumad, mida saate kasutada Jupyter märkmikuga. Need on:

1. **PySpark** (rakenduste kirjutatud Python)
2. **Säde** (rakenduste kirjutatud Scala)

Selles artiklis saate teada, kuidas kasutada nende tuumad ja mis on teile kuvatakse nende kasutamise eeliste kohta.

**Eeltingimused**

Vajate järgmist:

- Azure'i tellimuse. Leiate [Azure'i saada tasuta prooviversioon](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- Apache Spark kobar Hdinsightiga Linux. Juhised leiate teemast [loomine Apache Spark kogumite Windows Azure Hdinsightiga sisse](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="how-do-i-use-the-kernels"></a>Kuidas kasutada tuumade? 

1. [Azure portaali](https://portal.azure.com/)kaudu startboard, klõpsake paani klaster säde (kui te kinnitatud selle startboard). Samuti saate liikuda klaster jaotises **Sirvi kõiki** > **Hdinsightiga kogumite**.   

2. Keelest säde kobar valige **Kobar armatuurlaud**, ja klõpsake nuppu **Jupyter märkmiku**. Kui kuvatakse vastav viip, sisestage administraatori identimisteave klaster.

    > [AZURE.NOTE] Võib-olla ka jõuate Jupyter märkmiku jaoks klaster, avades järgmine URL brauseri. Klaster nime __CLUSTERNAME__ asendamiseks tehke järgmist.
    >
    > `https://CLUSTERNAME.azurehdinsight.net/jupyter`

2. Uue märkmiku luua uue tuumad. Klõpsake nuppu **Uus**ja seejärel klõpsake käsku **Pyspark** või **säde**. Kasutage säde tuum Scala rakenduste ja PySpark tuum Python rakendusi.

    ![Jupyter uue märkmiku loomine] (./media/hdinsight-apache-spark-jupyter-notebook-kernels/jupyter-kernels.png "Jupyter uue märkmiku loomine") 

3. See avatakse uus märkmik valitud tuum.

## <a name="why-should-i-use-the-pyspark-or-spark-kernels"></a>Miks kasutada PySpark või säde tuumad?

Siin on mõned uue tuumad kasutamise eelised.

1. **Algne kontekstides**. **PySpark** või **säde** tuumad, mis on varustatud Jupyter märkmikke, pole vaja määrata säde või taru kontekstides konkreetselt enne alustamist töötamine rakenduse arendate; need on vaikimisi teie jaoks saadaval. Nendes kontekstides on:

    * **sc** - säde kontekstis
    * **sqlContext** - taru kontekstis


    Jah, pole teil vaja käivitada laused, nt kontekstides määramiseks järgmist:

        ###################################################
        # YOU DO NOT NEED TO RUN THIS WITH THE NEW KERNELS
        ###################################################
        sc = SparkContext('yarn-client')
        sqlContext = HiveContext(sc)

    Selle asemel saate eelseadistatud kontekstides rakenduse otse kasutada.
    
2. **Lahtri magics**. PySpark tuum pakub mõne eelmääratletud "maagia", mis on erikäsud, mille abil saate helistada `%%` (nt `%%MAGIC` <args>). Maagiline käsk peab olema lahtris koodi esimest sõna ja luba mitu tekstirida sisu. Maagiline sõna peaks olema esimest sõna lahtrisse. Midagi enne maagiline, paarislehekülgedele kommentaaride lisamine põhjustab vea.   Magics kohta leiate lisateavet teemast [siin](http://ipython.readthedocs.org/en/stable/interactive/magics.html).

    Järgmises tabelis on loetletud erinevate magics, mis on saadaval tuumade kaudu.

  	| Maagiline     | Näide                         | Kirjeldus  |
  	|-----------|---------------------------------|--------------|
  	| Abi      | `%%help`                            | Loob tabeli kõik saadaolevad Magics näide ja kirjeldus |
  	| teave      | `%%info`                          | Väljundeid seansi teabe praeguse Liviuse lõpp-punkti |
  	| konfigureerimine | `%%configure -f`<br>`{"executorMemory": "1000M"`,<br>`"executorCores": 4`} | Konfigureerib parameetrid seansi loomiseks. Jõusta lipp (-f) on kohustuslik, kui seanss on juba loodud ja seansi lähevad ja uuesti luua. Vaadake loendi kehtiv parameetrite [Liviuse 's postituse /sessions koosolekukutse kehasse](https://github.com/cloudera/livy#request-body) . Parameetrite peab olema möödunud JSON stringina ja tuleb pärast maagiline, nagu on näidatud veerus näide järgmisele reale. |
  	| SQL-i       |  `%%sql -o <variable name>`<br> `SHOW TABLES`    | Käivitab taru päring on sqlContext. Kui soovitud `-o` parameeter on möödas, päringu tulem on säilis soovitud %% kohaliku Python kontekstis [Pandas](http://pandas.pydata.org/) dataframe nimega.   |
  	| kohaliku     |     `%%local`<br>`a=1`              | Kõik read koodi täidetakse kohalikult. Kood peab olema lubatud Python kood. |
  	| logide      | `%%logs`                        | Väljundeid logid Liviuse praeguse seansi kohta.  |
  	| kustutamine    | `%%delete -f -s <session number>` | Kindla seansi praeguse Liviuse lõpp-punkti kustutamine Pange tähele, et te ei saa kustutada enda tuum käivitatakse seanss. |
  	| puhastus   | `%%cleanup -f`                    | Kustutab kõik eksemplarid, praegune Liviuse lõpp-punkti, sh selle märkmiku seansi. Jõusta lipp – f on kohustuslikud.  |

    >[AZURE.NOTE] Lisaks magics, PySpark salvestab lisanud, saate kasutada ka [sisseehitatud IPython magics](https://ipython.org/ipython-doc/3/interactive/magics.html#cell-magics), sh `%%sh`. Saate kasutada funktsiooni `%%sh` maagiline skripte ja plokk-koodi käitamist kobar headnode. 

3. **Automaatne visualiseering**. **Pyspark** tuum tänav visualiseeritakse automaatselt väljundi taru ja SQL-i päringud. Teil on võimalik valida mitmesuguseid eri tüüpi visualiseeringuid, sh tabeli sektordiagramm, rea, ala, riba.

## <a name="parameters-supported-with-the-sql-magic"></a>Parameetrite toetatud soovitud %% SQL-i maagiline

Funktsiooni %% SQL-i maagiline toetab eri parameetrid, mille abil saate määrata väljundi päringute käivitamisel kuvatakse selline. Järgmises tabelis on loetletud väljund.

| Parameetri     | Näide                         | Kirjeldus  |
|-----------|---------------------------------|--------------|
| -o      | `-o <VARIABLE NAME>`                          | Selle parameetri abil säilivad päringu tulem on %% kohaliku Python konteksti, [Pandas](http://pandas.pydata.org/) dataframe. Dataframe muutuja nimi on teie määratud muutuja nimi. |
| – q      | `-q`                          | Kasutage seda visualiseeringute lahtri jaoks välja lülitada. Kui te ei soovi lahtri sisu automaatne visualiseerimine ja tahan, et see on dataframe jäädvustada ja seejärel kasutage `-q -o <VARIABLE>`. Kui soovite välja lülitada visualiseeringute ilma tulemite talletamiseks (näiteks töötab SQL-päringu küljel efekte, nt mõne `CREATE TABLE` lause), kasutada lihtsalt `-q` ilma mõne `-o` argument. |
| – m       |  `-m <METHOD>`    | Kui **meetod** on **võtta** või **näidis** (vaikimisi on **teha**). Kui meetod on **võtta**, tuum huvitavat elementide ülaosas tulemi andmehulga määratud MAXROWS (kirjeldatud selle tabeli). Kui meetod on **valimi**, tuum on CEIP Kuulake elemendid vastavalt andmehulga `-r` parameeter, edasi kirjeldatud järgmises tabelis.   |
| -r     |     `-r <FRACTION>`            | Siin on **MURDARV** Ujukomaarv, vahemikus 0,0 ja 1.0. Kui SQL-päringu valimi meetod on `sample`, siis tuum CEIP näidised määratud murdosa elementide tulemi jaoks; Näiteks kui käivitate SQL-päringu argumentide `-m sample -r 0.01`, siis tulemus read 1% proove juhusliku ID-ga. |
| -n      | `-n <MAXROWS>`                        | **MAXROWS** on täisarv. Tuum piirab **MAXROWS**väljundi ridade arv. Kui **MAXROWS** on negatiivne arv, nt **-1**, siis on tulemihulga ridade arv ei ole piiratud. |

**Näide:**

    %%sql -q -m sample -r 0.1 -n 500 -o query2 
    SELECT * FROM hivesampletable

Ülaltoodud lause teeb järgmist.

* Valib kõik kirjed alates **hivesampletable**.
* Kuna me kasutame – q, see lülitab automaatne-visualiseering.
* Kuna me kasutame `-m sample -r 0.1 -n 500` see CEIP näidised on hivesampletable ridades 10% ja piirab tulemuste komplekti 500 ridade suurust.
* Lõpetuseks, kuna kasutasime `-o query2` ka salvestab väljund on dataframe nimega **query2**sisse.
    

## <a name="considerations-while-using-the-new-kernels"></a>Uue tuumad kasutamisel peaksite arvesse võtma

Sellest, millist tuum kasutate (PySpark või säde), jättes töötab märkmikud tarbib oma kobar ressursse.  Koos nende tuumad, kuna kontekstides algne lihtsalt väljumist märkmikud Tapa konteksti ja seega on kobar ressursid on jätkuvalt kasutusel. Hea tava koos PySpark ja säde tuumad oleks selle märkmiku menüü **fail** käsk **Sule ja peatada** suvandi. See tapab konteksti ja seejärel väljub märkmiku.    


## <a name="show-me-some-examples"></a>Kuva mulle näited

Märkmiku Jupyter avamisel kuvatakse juurtasemel kaks kausta.

* **PySpark** kaustal on valimi märkmikke, mis kasutavad uut **Python** tuum.
* **Scala** kaustal on valimi märkmikke, mis kasutavad uut **säde** tuum.

Saate avada märkmiku **00 – [lugeda ESMALT mind] säde maagiline tuum funktsioonide** kohta on saadaval erinevad magics **PySpark** või **säde** kaustast. Saate teada, kuidas saavutada erinevaid stsenaariumeid lähemalt Hdinsightiga säde kogumite Jupyter märkmike abil saate kasutada ka jaotises kahte kausta muude saadaolevate valimi märkmikud.

## <a name="where-are-the-notebooks-stored"></a>Kuhu on talletatud märkmikud?

Salvestusruumi kontoga seostatud kobar kaustas **/HdiNotebooks** salvestatakse Jupyter märkmikud.  Märkmike, teksti faile ja kaustu, mis on loodud sees Jupyter pääseb juurde WASB.  Näiteks kui Jupyter abil saate luua kausta **myfolder** ja märkmiku **myfolder/mynotebook.ipynb**, pääsete selle märkmiku juures `wasbs:///HdiNotebooks/myfolder/mynotebook.ipynb`.  Vastupidi kehtib ka, st kui laadite märkmiku otse oma konto salvestusruumi `/HdiNotebooks/mynotebook1.ipynb`, märkmiku saab Jupyter nähtav ka.  Märkmike jääb salvestusruumi konto isegi juhul, kui klaster kustutatakse.

Salvestusruumi kontol salvestatud märkmikud nii ühildub HDFS. Juhul, kui te SSH kobar, saate kasutada rakendusse faili haldamise käske umbes selline:

    hdfs dfs -ls /HdiNotebooks                            # List everything at the root directory – everything in this directory is visible to Jupyter from the home page
    hdfs dfs –copyToLocal /HdiNotebooks                 # Download the contents of the HdiNotebooks folder
    hdfs dfs –copyFromLocal example.ipynb /HdiNotebooks   # Upload a notebook example.ipynb to the root folder so it’s visible from Jupyter


Juhuks, kui on juurdepääs salvestusruumi konto jaoks klaster küsimusi, märkmikud salvestatakse ka selle headnode `/var/lib/jupyter`.

## <a name="supported-browser"></a>Toetatud brauserit
Google Chrome'is on toetatud Jupyter märkmike tööta Hdinsightiga säde kogumite suhtes.

## <a name="feedback"></a>Tagasiside

Kuvatakse uus on arenevad etapi ja on vanemad aja jooksul. See võib ka, et API-de võib muutuda, nagu need tuumad vanemad. Oleme teile tänulikud tagasisidet, et teil on need uue tuumad kasutamise ajal. See olla väga kasulik kujundamisel järgmisi tuumad lõppversiooni välja. Selles artiklis allosas jaotises **kommentaarid** saate kommentaarid ja tagasiside jätta.


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

* [Välise pakettide Jupyter märkmike kasutamine](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)

* [Jupyter oma arvutisse installida ja luua ühenduse mõne Hdinsightiga säde kobar](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Ressursside haldamine

* [Ressursid Apache Spark kobar rakenduses Windows Azure Hdinsightiga haldamine](hdinsight-apache-spark-resource-manager.md)

* [Töötavate on Apache Spark kobar rakenduses Hdinsightiga jälitamine ja silumine tööde haldamine](hdinsight-apache-spark-job-debugging.md)
