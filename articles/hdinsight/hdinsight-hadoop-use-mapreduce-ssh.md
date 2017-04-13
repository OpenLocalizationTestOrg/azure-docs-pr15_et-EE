<properties
   pageTitle="Hadoopi sisse Hdinsightiga MapReduce ja SSH ühenduse | Microsoft Azure'i"
   description="Saate teada, kuidas kasutada SSH Hadoopi kasutamine Hdinsightiga MapReduce tööde haldamine."
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
   ms.date="08/23/2016"
   ms.author="larryfr"/>

# <a name="use-mapreduce-with-hadoop-on-hdinsight-with-ssh"></a>Kasutada MapReduce Hdinsightiga Hadoopi SSH

[AZURE.INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

Sellest artiklist saate teada, kuidas kasutada Secure Shell (SSH) ühenduse loomine Hadoopi Hdinsightiga kobar ja seejärel edastab MapReduce tööd Hadoopi käskude abil.

> [AZURE.NOTE] Kui teil on juba tuttavad Hadoopi Linuxi-põhiste serverite kasutamine, kuid teil on uus Hdinsightiga, teemast [Linux-põhine Hdinsightiga näpunäited](hdinsight-hadoop-linux-information.md).

##<a id="prereq"></a>Eeltingimused

Selles artiklis toodud juhiseid tegemiseks on vaja järgmist:

* Linux-põhine Hdinsightiga (Hadoopi Hdinsightiga) kobar

* SSH kliendi. Linux, Unix ja Mac operatsioonisüsteemid peaks tulema SSH kliendi. Windowsi kasutajad peab klient, nt [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)alla laadida.

##<a id="ssh"></a>Kasutajaga SSH

Ühenduse täielik domeeninimi (FQDN), Hdinsightiga klaster SSH käsu abil. FQDN saab teie andsite kobar, millele järgneb **. azurehdinsight.net**. Näiteks järgmine soovite ühendada nimega **myhdinsight**klaster.

    ssh admin@myhdinsight-ssh.azurehdinsight.net

**Kui sertifikaadi võti SSH autentimise** Hdinsightiga kobar loomisel, peate privaatvõti asukoha määramiseks oma kliendi süsteemi, näiteks:

    ssh -i ~/mykey.key admin@myhdinsight-ssh.azurehdinsight.net

**Kui SSH autentimise parooli** loomisel Hdinsightiga kobar, peate sisestama parooli küsimise.

Hdinsightiga SSH kasutamise kohta leiate lisateavet teemast [Kasutamine SSH koos Linux-põhine Hadoopi Hdinsightiga Linux, OS X-ja Unix](hdinsight-hadoop-linux-use-ssh-unix.md).

###<a name="putty-windows-clients"></a>Kitt (Windowsi kliendid)

Windows ei paku sisseehitatud SSH klient. Soovitame kasutada **kitt**, mille saate alla laadida [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

PuTTY kasutamise kohta leiate lisateavet teemast [Kasutamine SSH koos Linux-põhine Hadoopi Hdinsightiga Windows ](hdinsight-hadoop-linux-use-ssh-windows.md).

##<a id="hadoop"></a>Hadoopi käskude kasutamine

1. Pärast seda, kui olete loonud Hdinsightiga klaster, kasutage MapReduce töö alustamiseks **Hadoopi** järgmine käsk:

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount wasb:///example/data/gutenberg/davinci.txt wasb:///example/data/WordCountOutput

    Käivitub **wordcount** ainekursust, mis sisaldub **hadoop-mapreduce-examples.jar** faili. Sisendina, kasutab **wasbs://example/data/gutenberg/davinci.txt** dokumendi ja väljundi talletuskohaks oleval veebisaidil **wasbs: / / / näide/andmete/WordCountOutput**.

    > [AZURE.NOTE] See MapReduce töö ja näidisandmetega kohta leiate lisateavet teemast [Kasutamine MapReduce Hadoopi Hdinsightiga sisse](hdinsight-use-mapreduce.md).

2. Töö eraldab üksikasjad töötleb ning tagastab see teave järgmine pärast töö lõpulejõudmist:

        File Input Format Counters
        Bytes Read=1395666
        File Output Format Counters
        Bytes Written=337623

3. Pärast töö lõpulejõudmist, kasutage väljundi faili, mis on talletatud **wasbs://example/data/WordCountOutput**veebisaidil järgmine käsk:

        hdfs dfs -ls wasbs:///example/data/WordCountOutput

    See peaks olema kuvatud kaks faili, **_SUCCESS** ja **osa-r-00000**. **Osa-r-00000** fail sisaldab väljundi selle töö.

    > [AZURE.NOTE] Mõned MapReduce tööd võib eri **osa-r-##** faile jagatud tulemid. Kui jah, kasutage selle ## järelliite tähistamiseks järjestuse failid.

4. Vaatamiseks väljund, kasutage järgmist käsku:

        hdfs dfs -cat wasbs:///example/data/WordCountOutput/part-r-00000

    Kuvatakse sõnu, mis sisalduvad **wasbs://example/data/gutenberg/davinci.txt** faili- ja mitu korda iga sõna ilmnenud. Järgmises näites failis sisalduvate andmete:

        wreathed        3
        wreathing       1
        wreaths         1
        wrecked         3
        wrenching       1
        wretched        6
        wriggling       1

##<a id="summary"></a>Kokkuvõte

Nagu näete, Hadoop käsud pakkuda lihtne viis on Hdinsightiga kobar MapReduce tööde käitamine ja seejärel kuvamiseks töö väljund.

##<a id="nextsteps"></a>Järgmised sammud

Üldist teavet MapReduce töökohtade Hdinsightiga kohta:

* [Hdinsightiga Hadoopi MapReduce kasutamine](hdinsight-use-mapreduce.md)

Teavet teiste mooduste kohta saate töötada Hadoopi Hdinsightiga:

* [Hadoopi Hdinsightiga taru kasutamine](hdinsight-use-hive.md)

* [Kasutage siga Hadoopi Hdinsightiga](hdinsight-use-pig.md)
