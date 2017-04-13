<properties 
    pageTitle="Teadaolevad probleemid, Apache Spark rakenduses Hdinsightile | Microsoft Azure'i" 
    description="Teadaolevad probleemid Apache Spark Hdinsightiga sisse." 
    services="hdinsight" 
    documentationCenter="" 
    authors="mumian" 
    manager="jhubbard" 
    editor="cgronlun"
    tags="azure-portal"/>

<tags 
    ms.service="hdinsight" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/25/2016" 
    ms.author="nitinme"/>

# <a name="known-issues-for-apache-spark-cluster-on-hdinsight-linux"></a>Teadaolevad probleemid Hdinsightiga Linux Apache Spark kobar

Selle dokumendi jälgib, kõik Hdinsightiga säde avaliku Preview teadaolevad probleemid.  

##<a name="livy-leaks-interactive-session"></a>Liviuse lekib interaktiivsed seanss
 
Liviuse taaskäivitamisel interaktiivsed seansiga (Ambari või tõttu headnode 0 virtual arvuti taaskäivitamist) elus on lekkinud on interaktiivne töö seanss. Seetõttu töökohta saate kinni aktsepteeritud olekus ja ei saa käivitada.

**Vähendamiseks:**

Kasutage lahendus probleemi järgmiselt:

1. SSH headnode sisse. 
2. Käivitage järgmine käsk otsimine interaktiivse tööd alustada Liviuse kaudu rakenduste ID-sid. 

        yarn application –list

    Vaikimisi töö nimesid saab Liviuse kui tööd on alustamine Liviuse interaktiivsed seansi konkreetsete ilma nimedeta määratud, seansi jaoks Liviuse algatatud Jupyter märkmik, töö nimi algab remotesparkmagics_ *. 

3. Käivitage järgmine käsk jääda nende tööde haldamine. 

        yarn application –kill <Application ID>

Uute töökohtade hakatakse. 

##<a name="spark-history-server-not-started"></a>Säde ajalugu Server pole alustatud 

Säde ajalugu Server on käivitatud automaatselt klaster loomise järel.  

**Vähendamiseks:** 

Käsitsi käivitamine ajalugu serveri Ambari.

## <a name="permission-issue-in-spark-log-directory"></a>Õiguste probleem säde log kataloog 

Kui hdiuser esitab spark-submit töökoha, on ka tõrge java.io.FileNotFoundException: /var/log/spark/sparkdriver_hdiuser.log (juurdepääs keelatud) ja draiver log on kirjutatud. 

**Vähendamiseks:**
 
1. Hadoopi rühma hdiuser lisada. 
2. Pärast kobar loomist pakuvad /var/log/spark 777 õigused. 
3. Värskendage säde Logi asukoha olema kataloog 777 õigustega Ambari abil.  
4. Käivita säde-esitada sudo.  

## <a name="issues-related-to-jupyter-notebooks"></a>Märkmike Jupyter seotud probleemid

Järgnevalt on mõned teadaolevad probleemid, mis on seotud Jupyter märkmikud.


### <a name="notebooks-with-non-ascii-characters-in-filenames"></a>Mitte-ASCII märkide failinimed märkmikke

Jupyter märkmike säde Hdinsightiga rühmades kasutatavate ei peaks olema failinimed-ASCII märke. Kui proovite üles laadida kuni Jupyter kasutajaliides, mis on-ASCII faili nimi faili, see ei õnnestu vaikselt (st Jupyter ei võimaldavad teil faili üles laadida, kuid see ei põhjustada nähtav tõrge kas). 

### <a name="error-while-loading-notebooks-of-larger-sizes"></a>Viga märkmike mahukamate.

Tõrge võib ilmneda **`Error loading notebook`** kui laadite märkmikke, mis on suurem suurus.  

**Vähendamiseks:**

Kui saate selle tõrketeate, ei tähenda teie andmed on vigane või kaotsi.  Teie märkmikud on endiselt kettal `/var/lib/jupyter`, ja saate SSH üheks kobar neile juurde pääseda. Oma kohalikus arvutis (kasutades SCP või WinSCP) saate varukoopiana märkmiku mis tahes oluliste andmete kaotsimineku vältimiseks saate kopeerida klaster märkmikke. Seejärel saate SSH tunneliga sisse oma headnode Port 8001 juurdepääsu Jupyter ilma lüüsi.  Sealt saate oma märkmiku väljund tühjendage ja uuesti salvestada selle märkmiku mahu minimeerimiseks.

Selle tõrke takistada kordumist tulevikus, peate järgima parimaid tavasid:

* On tähtis hoida märkmiku suurus väike. Mis tahes väljund oma säde töökohta, mis saadetakse tagasi Jupyter on jätkunud märkmikus.  See on hea tava koos Jupyter üldiselt vältimiseks töötab `.collect()` sisse suur RDD või dataframes; selle asemel, kui soovite mõne RDD sisu Piilumine, kaaluge töötab `.take()` või `.sample()` , et teie väljundi ei saa liiga suur.
* Ka märkmiku salvestamisel, tühjendage ruut Kõik väljund mahu vähendamiseks lahtritesse.

### <a name="notebook-initial-startup-takes-longer-than-expected"></a>Märkmiku algse käivitus kulub oodatust rohkem aega 

Esimene kood lause Jupyter märkmiku abil säde maagiline võib kuluda mitu minutit.  

**Selgitus:**
 
See juhtub siis, kui esimene lahter kood töötab. See käivitab taustal seansi konfiguratsiooni ja säde, SQL-i ja taru kontekstides määratakse. Pärast nende kontekstides määratakse, esimene lause käitatakse ja see mulje, et see lause võttis kaua aega.

### <a name="jupyter-notebook-timeout-in-creating-the-session"></a>Jupyter märkmiku ajalõpp loomisel seanss

Kui säde kobar on ressursid, kuvatakse säde ja Pyspark tuumade Jupyter märkmiku loomise seansi ajalõpp. 

**Kergendamise:** 

1. Vabastada mõne ressursside klaster säde alusel:

    - Menüü Sule ja peatada või klõpsates raadionuppu sulgumist märkmik Exploreri peatamine muude säde märkmikud.
    - Muude säde rakenduste lõngast peatamine.

2. Taaskäivitage üritasite alustada märkmik. Piisavalt ressursse peaks olema nüüd seansi loomiseks saadaval.

##<a name="see-also"></a>Vt ka

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

* [Jupyter oma arvutisse installida ja luua ühenduse mõne Hdinsightiga säde kobar](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Ressursside haldamine

* [Ressursid Apache Spark kobar rakenduses Windows Azure Hdinsightiga haldamine](hdinsight-apache-spark-resource-manager.md)

* [Töötavate on Apache Spark kobar rakenduses Hdinsightiga jälitamine ja silumine tööde haldamine](hdinsight-apache-spark-job-debugging.md)
