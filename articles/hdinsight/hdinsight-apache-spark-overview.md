<properties 
    pageTitle="Ülevaate Apache Spark rakenduses Hdinsightiga | Microsoft Azure'i" 
    description="Sissejuhatus Apache säde Hdinsightiga sisse- ja stsenaariumid, kus säde kasutamine Hdinsightile oma rakendused." 
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
    ms.topic="get-started-article" 
    ms.date="08/25/2016" 
    ms.author="nitinme"/>

# <a name="overview-apache-spark-on-hdinsight-linux"></a>Ülevaade: Apache Spark Hdinsightiga Linux
 
<a href="http://spark.apache.org/" target="_blank">Apache Spark</a> on avatud lähtekoodi paralleelselt, mis töötlemine, mis toetab-mälu töötlemine andmete – suured analüütiline rakenduste jõudlust parandada. Säde töötlemine engine on loodud kiiruse hõlpsamaks kasutamine ja keeruka analüüsi jaoks. Säde tema mälu-arvutus võimaluste oleks hea valik iteratiivne algoritmide masina õppimine ja Graphi arvutuste. Säde ühildub ka Azure'i bloobimälu (WASB) nii, et teie olemasolevad andmed salvestatakse Azure saab töödelda hõlpsalt säde kaudu.

Hdinsightiga säde kobar loomisel saate luua Azure Arvuta ressursid säde installinud ja konfigureerinud. Luua säde kobar Hdinsightiga kulub vaid umbes kümme minutit. Azure'i bloobimälu talletatakse andmeid töödeldakse. Vt [teemasid Azure'i bloobimälu Hdinsightiga][hdinsight-storage].

![Apache Spark klõpsake Azure Hdinsightiga] (./media/hdinsight-apache-spark-overview/hdispark.architecture.png  "Apache Spark klõpsake Azure Hdinsightiga")


**Kas soovite alustada Apache Spark Windows Azure Hdinsightiga kohta?** Vt [Kiirjuhend: loomine säde kobar Hdinsightiga Linux ja käivitage valimi rakendustes, mis kasutavad Jupyter](hdinsight-apache-spark-jupyter-spark-sql.md).

>[AZURE.NOTE] Teadaolevate probleemide ja piirangutega praeguse väljaandesse loendi leiate teemast [teadaolevad probleemid, Apache Spark rakenduses Windows Azure Hdinsightiga (Linux)](hdinsight-apache-spark-jupyter-spark-sql.md).


## <a name="why-use-spark-on-azure-hdinsight"></a>Miks kasutada Windows Azure Hdinsightiga säde? 

Azure Hdinsightiga pakub täielikku hallatavate säde. Klõpsake Hdinsightiga säde kasutamise eelised on:

| Funktsioon                             | Kirjeldus       |
|-------------------------------------|-------------------|
| Lihtne kogumite loomine            | Saate luua uue säde kobar Hdinsightiga Azure'i haldusportaal, Azure PowerShelli või Hdinsightiga .NET SDK minutites. Vt [kasutamise alustamine rakenduses Hdinsightiga säde kobar](hdinsight-apache-spark-jupyter-spark-sql.md) |
| Kasutuslihtsus                     | Säde Hdinsightiga rühmades sisaldab Jupyter märkmike eelnevalt konfigureeritud. Saate neid interaktiivsete andmete töötlemiseks ja visualiseering. URL-ide soovitud on https://CLUSTERNAME.azurehdinsight.net/jupyter. Asendage __CLUSTERNAME__ klaster säde Hdinsightiga nime.|
| REST API-d                       | Säde rakenduses Hdinsightiga sisaldab [Liviuse](https://github.com/cloudera/hue/tree/master/apps/spark/java#welcome-to-livy-the-rest-spark-server), ülejäänud-API vastavalt säde töö server kaugühenduse teel esitada ja jälgida esitatava töö. |
| Azure'i andmesalve Lake tugi | Säde Hdinsightiga kohta saab konfigureerida Azure andmesalve Lake kasutamiseks on täiendav salvestusruum. Lake andmesalve kohta leiate lisateavet teemast [Ülevaade Azure'i andmed Lake pood](../data-lake-store/data-lake-store-overview.md).
| Azure'i teenuste integreerimine | Säde klõpsake Hdinsightiga kaasas konnektori Azure'i sündmuse jaoturi. Kliente saate koostada streaming rakendustes, mis kasutavad sündmuse jaoturiga, lisaks [Kafka](http://kafka.apache.org/), mis on juba olemas säde osana. |
| R Server tugi  | Saate häälestada R Server Hdinsightiga säde kobar jaotatud R arvutuste töötada koos säde kobar lubas kiiruse. Lisateabe saamiseks vt [R serveris Hdinsightiga kasutamise alustamine](hdinsight-hadoop-r-server-get-started.md).   |
| Huvitav idee integreerimine  | Saate luua ja esitada rakenduste abil on Hdinsightiga säde kobar Hdinsightiga lisandmooduli jaoks huvitav. Lisateabe saamiseks vt [Kasutamine Hdinsightiga tööriistade lisandmooduli huvitav idee luua säde Hdinsightiga säde Linux kobar rakendusi](hdinsight-apache-spark-intellij-tool-plugin.md). |
| Samaaegseid päringud              | Säde rakenduses Hdinsightiga toetab samaaegseid päringud. See võimaldab mitme päringu ühe kasutaja või mitme päringu erinevate kasutajate ja rakenduste sama kobar ressursside jagamiseks. |
| Vahemälu SSD                 | Kui soovite, et vahemälu andmete mälu või SSD manustatud kobar sõlmed. Mälu vahemällu annab päringu parimaid tulemusi, kuid võib olla kulukas; hea valik pole vaja luua kobar vastavalt kogu andmekomplekti mällu nõutava suurusega päringu jõudluse parandamiseks vahemällu SSD pakub.|
| Integreerimine Ärianalüüsi tööriistade kohta       | Hdinsightiga sütitaja pakub Ärianalüüsi tööriistade nagu [Power BI](http://www.powerbi.com/) ja [sellele](http://www.tableau.com/products/desktop) andmeanalüüsi konnektorid.|
| Eellaaditud Anaconda teegid        | Säde kogumite kohta Hdinsightiga on Anaconda teekide eelinstallitud. [Anaconda](http://docs.continuum.io/anaconda/) pakub lähedale 200 teekide masina õ, andmeanalüüs, visualiseeringu jne.|
| Skaleeritavus.                     | Kuigi saate määrata sõlmed arvu klaster loomise ajal, võite kasvata või Kahanda kobar, et need vastaksid töökoormus. Kõik Hdinsightiga kogumite võimaldavad sõlmed klaster arvu muutmine. Säde kogumite saate ka, lähevad andmete kaotamata, kuna kõik andmed on salvestatud Azure'i bloobimälu. |
| 24/7 tugi                    | Säde Hdinsightiga kohta on ettevõtte tasemel 24/7 tugi ja SLA 99,9% üles-kellaaeg.|



## <a name="what-are-the-use-cases-for-spark-on-hdinsight"></a>Mis on kasutamise juhtudel jaoks säde Hdinsightiga kohta?

Apache Spark rakenduses Hdinsightiga võimaldab loetletud tähtsaimad stsenaariumid.

### <a name="interactive-data-analysis-and-bi"></a>Interaktiivse andmeanalüüsi ja BI

[Vaadake õpetus](hdinsight-apache-spark-use-bi-tools.md)

Apache Spark rakenduses Hdinsightiga salvestab andmed Azure plekid. Ekspertide võtme tegijad analüüsida ja aruannete koostamine üle andmeid ja Microsoft Power BI abil analüüsitud andmete interaktiivsete aruannete koostamine. Ärianalüütikud saate alustada struktureerimata/pooleldi struktureeritud andmete põhjal Azure storage, määratleda skeemi abil märkmike andmete jaoks ja siis koostada andmemudelite Microsoft Power BI abil. Säde rakenduses Hdinsightiga toetab ka kolmanda osapoole Ärianalüüsi tööriistad, nt sellele, Qlikview ja SAP-i Lumira, mistõttu on optimaalne platvormi andmete ärianalüütikud, ekspertide ja võtme tegijad arvu.

### <a name="iterative-machine-learning"></a>Iteratiivsed masina õpetused

[Vaadake õpetus: prognoosida koostamise temperatuuride žestid HVAC andmed](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

[Vaadake õpetus: prognoosida toiduga kontrollitulemuste](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

Apache Spark kaasas [MLlib](http://spark.apache.org/mllib/), õppimine Raamatukogu ehitatud säde masina. Lisaks sellele säde klõpsake Hdinsightile sisaldab Anaconda Python jaotuse erinevaid arvuti õ paketid. Paar seda sisseehitatud tugi Jupyter märkmike ja teil on üla-ja-rea keskkonna masina õ rakenduste loomise kohta.  

### <a name="streaming-and-real-time-data-analysis"></a>Videote voogesitamise ja reaalajas Andmeanalüüs

[Vaadake õpetus](hdinsight-apache-spark-eventhub-streaming.md)

Reaalajas andmed analüüsiga stsenaariumid vahemikus aja vähendamiseks andmete ülevaate, nagu see maandub loomisse tõelise streaming lahendus andmete töötlemiseks. Säde rakenduses Hdinsightiga pakub rikkaliku tugi reaalajas analytics lahenduste. Kuigi säde on juba konnektorid neelata andmete paljudest allikatest, nt Kafka, Flume, Twitteri, ZeroMQ või TCP sockets, säde Hdinsightiga sisse lisab esimese klassi tugi söömisega Azure'i sündmuse jaoturi andmeid. Sündmuse jaoturi on enim kasutatud andmebaasitõrge teenuse Azure. Kellel on out-of-box tugi sündmuse jaoturi muudab säde rakenduses Hdinsightile optimaalne aluse koostamise reaalajas analytics müügivõimaluste.

##<a name="next-steps"></a>Millised komponendid on säde kobar kaasatud?

Säde rakenduses Hdinsightiga sisaldab järgmised komponendid, mis on saadaval vaikimisi rühmad.

- [Spark Core](https://spark.apache.org/docs/1.5.1/). Sisaldab Spark Core, säde SQL-i, säde streaming API-d, GraphX ja MLlib.
- [Anaconda](http://docs.continuum.io/anaconda/)
- [Liviuse](https://github.com/cloudera/hue/tree/master/apps/spark/java#welcome-to-livy-the-rest-spark-server)
- [Jupyter märkmik](https://jupyter.org)

Säde Hdinsightiga sisse ka pakub on [ODBC-draiver](http://go.microsoft.com/fwlink/?LinkId=616229) Ühenduvus säde kogumite Hdinsightiga sisse nagu Microsoft Power BI ja sellele Ärianalüüsi tööriistade kaudu.

## <a name="where-do-i-start"></a>Kust alustada?

Alustage luua säde klaster Hdinsightiga Linux. Vt [Kiirjuhend: loomine säde kobar Hdinsightiga Linux ja käivitage valimi rakendustes, mis kasutavad Jupyter](hdinsight-apache-spark-jupyter-spark-sql.md). 

## <a name="next-steps"></a>Järgmised sammud

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

* [Jupyter oma arvutisse installida ja luua ühenduse mõne Hdinsightiga säde kobar](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Ressursside haldamine

* [Ressursid Apache Spark kobar rakenduses Windows Azure Hdinsightiga haldamine](hdinsight-apache-spark-resource-manager.md)

* [Töötavate on Apache Spark kobar rakenduses Hdinsightiga jälitamine ja silumine tööde haldamine](hdinsight-apache-spark-job-debugging.md)


[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
