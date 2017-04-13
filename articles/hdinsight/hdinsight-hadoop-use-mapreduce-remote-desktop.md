<properties
   pageTitle="MapReduce ja Kaugtöölaud Hadoopi rakenduses Hdinsightiga | Microsoft Azure'i"
   description="Saate teada, kuidas kasutada kaugtöölaua ühenduse loomine Hadoopi Hdinsightiga ja käivitada MapReduce tööd."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/27/2016"
   ms.author="larryfr"/>

# <a name="use-mapreduce-in-hadoop-on-hdinsight-with-remote-desktop"></a>Hadoopi kaugtöölaua Hdinsightiga MapReduce kasutamine

[AZURE.INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

Sellest artiklist saate teada, kuidas ühenduse loomine Hadoopi Hdinsightiga kobar kaugtöölaua ja seejärel käivitage MapReduce töö Hadoopi käsu abil.

##<a id="prereq"></a>Eeltingimused

Selles artiklis toodud juhiseid tegemiseks on vaja järgmist:

* Windowsi-põhiste Hdinsightiga (Hadoopi Hdinsightiga) kobar

* Kliendi arvutisse, kus töötab Windows 10, Windows 8 või Windows 7

##<a id="connect"></a>Kaugtöölaua kasutajaga

Luba kaugtöölaud Hdinsightiga kobar, siis selle ühenduse juures [Hdinsightiga kogumite abil RDP ühenduse](hdinsight-administer-use-management-portal.md#rdp)juhiste järgi.

##<a id="hadoop"></a>Hadoopi käsuga

Kui olete loonud ühenduse töölaua jaoks Hdinsightiga kobar, järgmiste juhiste abil Hadoopi käsu abil saate käivitada MapReduce töö:

1. **Hadoopi käsurea**alustamiseks töölaualt Hdinsightiga. Avatakse uus käsuviiba soovitud **c:\apps\dist\hadoop-&lt;versiooninumber >** kataloogi.

    > [AZURE.NOTE] Versiooninumber muutub Hadoopi värskendatakse. **HADOOP_HOME** keskkonna muutuja saab leida tee. Näiteks `cd %HADOOP_HOME%` muutub kataloogide Hadoopi kataloogi, ilma et peaksite teadma versiooninumber.

2. **Hadoopi** käsu abil saate käivitada on näide MapReduce töö, kasutage järgmist käsku:

        hadoop jar hadoop-mapreduce-examples.jar wordcount wasbs:///example/data/gutenberg/davinci.txt wasbs:///example/data/WordCountOutput

    Käivitub **wordcount** ainekursust, mis sisaldub **hadoop-mapreduce-examples.jar** praegust kausta fail. Sisendina, kasutab **wasbs://example/data/gutenberg/davinci.txt** dokumendi ja väljundi talletuskohaks oleval veebisaidil: **wasbs: / / / näide/andmete/WordCountOutput**.

    > [AZURE.NOTE] See MapReduce töö ja näidisandmetega kohta leiate lisateavet teemast <a href="hdinsight-use-mapreduce.md">Kasutamine MapReduce Hdinsightiga Hadoopi sisse</a>.

2. Toimub töötlemine ning tagastab see teave järgmine kui töö on valmis, eraldab töö üksikasjad:

        File Input Format Counters
        Bytes Read=1395666
        File Output Format Counters
        Bytes Written=337623

3. Kui töö on valmis, kasutada väljundi faili juures **wasbs://example/data/WordCountOutput**järgmine käsk:

        hadoop fs -ls wasbs:///example/data/WordCountOutput

    See peaks olema kuvatud kaks faili, **_SUCCESS** ja **osa-r-00000**. **Osa-r-00000** fail sisaldab väljundi selle töö.

    > [AZURE.NOTE] Mõned MapReduce tööd võib eri **osa-r-##** faile jagatud tulemid. Kui jah, kasutage selle ### järelliite tähistamiseks järjestuse failid.

4. Vaatamiseks väljund, kasutage järgmist käsku:

        hadoop fs -cat wasbs:///example/data/WordCountOutput/part-r-00000

    Kuvatakse failis **wasbs://example/data/gutenberg/davinci.txt** koos mitu korda iga sõna ilmnenud sisalduvaid sõnu. Järgmises näites failis sisalduvate andmete:

        wreathed        3
        wreathing       1
        wreaths         1
        wrecked         3
        wrenching       1
        wretched        6
        wriggling       1

##<a id="summary"></a>Kokkuvõte

Nagu näete, Hadoop käsu abil on lihtne MapReduce töö käitamist on Hdinsightiga kobar ja seejärel kuvamiseks töö väljund.

##<a id="nextsteps"></a>Järgmised sammud

Üldist teavet MapReduce töökohtade Hdinsightiga kohta:

* [Hdinsightiga Hadoopi MapReduce kasutamine](hdinsight-use-mapreduce.md)

Teavet teiste mooduste kohta saate töötada Hadoopi Hdinsightiga:

* [Hadoopi Hdinsightiga taru kasutamine](hdinsight-use-hive.md)

* [Kasutage siga Hadoopi Hdinsightiga](hdinsight-use-pig.md)
