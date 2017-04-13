<properties
    pageTitle="Installimine ja kasutamine Giraph Linux-põhine Hdinsightiga (Hadoopi) | Microsoft Azure'i"
    description="Saate teada, kuidas installida Giraph Linux-põhine Hdinsightiga kogumite skripti toimingute kasutamine. Skripti toimingud võimaldavad kohandada klaster muutes kobar konfiguratsiooni või teenuste ja Utiliidid loomise ajal."
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/17/2016"
    ms.author="larryfr"/>

# <a name="install-giraph-on-hdinsight-hadoop-clusters-and-use-giraph-to-process-large-scale-graphs"></a>Installige Giraph Hdinsightiga Hadoopi kogumite ja Giraph abil töödelda suuremahuliste graafikud

Kohandada klaster **Skripti toimingu** abil saate Giraph installida kobar Hadoopi rakenduses Windows Azure Hdinsightiga klõpsake mis tahes tüüpi.

Selles teemas saate teada, kuidas installida Giraph skripti toimingu abil. Kui olete installinud Giraph, saate ka teada, kuidas kasutada Giraph kõige Tüüpilised kasutusalad, mis on suuremahuliste graafikud töötlemine.

> [AZURE.NOTE] Selle artikli teave on Linux-põhine Hdinsightiga kogumite. Windowsi-põhiste kogumite töötamise kohta leiate teemast [Installida Giraph klõpsake Hdinsightiga Hadoopi kogumite (Windows)](hdinsight-hadoop-giraph-install.md)

## <a name="whatis"></a>Mis on Giraph?

[Apache Giraph](http://giraph.apache.org/) võimaldab teil teha, kasutades Hadoopi töötlemine graafik ja saab kasutada Windows Azure Hdinsightiga. Graafikud mudeli objektid, nt ühendused marsruuteri nagu Internet suure võrgus, või seoseid inimeste suhtlusvõrkudesse (mõnikord nimetatakse social graafik) vahel seoseid. Graafik töötlemine võimaldab teil põhjuse kohta seoseid objektide graafiku, näiteks:

- Tuvastamise võimalikud sõprade vastavalt oma praeguse seosed.
- Tuvastamise lühima marsruutimiseks võrgus kahe arvuti vahel.
- Arvutamise veebilehtede lehe asukoha andmehulgas.

> [AZURE.WARNING] Koos Hdinsightiga kobar komponendid on täielikult toetatud ja Microsoft Support aitab eristada ja nende komponentide seotud probleemide lahendamiseks.
>
> Kohandatud komponendid, nt Giraph, tugiteenuseid äriliselt mõistlik aidata teil selle probleemi tõrkeotsingu sooritamiseks. See võib põhjustada probleemi lahendamine või palub teil otsimist ja avatud allika tehnoloogiad, kui leitakse sügav teadmised selle tehnoloogia. Näiteks on palju kogukonnafoorumi saite, mida saab kasutada, nt: [MSDN-i Foorum Hdinsightiga](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Ka Apache projektide on projekti saitidel [http://apache.org](http://apache.org), näiteks: [Hadoopi](http://hadoop.apache.org/).

##<a name="what-the-script-does"></a>Mida tähendab skript

See skript teha järgmisi toiminguid:

* Installib Giraph abil`/usr/hdp/current/giraph`
* Eksemplarid on `giraph-examples.jar` klaster jaoks vaikimisi salvestusruumi (WASB) faili:`/example/jars/giraph-examples.jar`

## <a name="install"></a>Installige Giraph skripti toimingute kasutamine

Proovi skripti Giraph installimiseks on Hdinsightiga kobar on saadaval järgmises asukohas.

    https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh

Selles jaotises antakse juhiseid kasutamise näidis skripti loomisel klaster Azure portaali kaudu. 

> [AZURE.NOTE] Azure'i PowerShelli, Azure'i CLI, Hdinsightiga .NET SDK või Azure ressursihaldur malle saab kasutada toimingute skript. Saate rakendada ka skript toimingud juba töötab kogumite. Lisateabe saamiseks vt [kohandamine Hdinsightiga kogumite skripti toimingute](hdinsight-hadoop-customize-cluster-linux.md).

1. Alustada luua klaster juhiste abil [luua Linux-põhine Hdinsightiga](hdinsight-hadoop-create-linux-clusters-portal.md)rühmades, kuid täielik loomine.

2. **Valikuline konfigureerimine** enne, valige **Skripti toimingud**ja allolev teave:

    * __Nimi__: sisestage skripti toimingu sõbralik nimi.
    * __Skripti URI__: https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh
    * __Juht__: märkige see ruut
    * __Töötaja__: Jätke ruut märkimata
    * __ZOOKEEPER__: Jätke ruut märkimata
    * __Parameetrite__: jätke see väli tühjaks

3. **Skripti toimingud**allosas kasutada konfiguratsiooni salvestamiseks **Valige** nupp. Lõpuks nuppu **Valige** **Valikuline konfigureerimine** tera allosas salvestamiseks kasutage valikuline konfiguratsiooniteavet.

4. Jätkake loomise klaster, nagu on kirjeldatud rühmades [luua Linux-põhine Hdinsightiga](hdinsight-hadoop-create-linux-clusters-portal.md).

## <a name="usegiraph"></a>Kuidas kasutada Giraph Hdinsightiga?

Kui klaster on lõpule jõudnud, loomise, järgmiste juhiste abil SimpleShortestPathsComputation näide, mis on kaasas Giraph käivitada. See rakendatakse lihtsa <a href = "http://people.apache.org/~edwardyoon/documents/pregel.pdf">Pregel</a> rakendamist lühima tee vahel graafiku objektide leidmiseks.

1. Ühendada Hdinsightiga klaster SSH abil.

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    Hdinsightiga SSH kasutamise kohta lisateabe saamiseks vt järgmist:

    * [Kasutada SSH Linux-põhine Hadoopi Hdinsightiga Linux, Unix või OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [Kasutada SSH Linux-põhine Hadoopi Windows Hdinsightiga](hdinsight-hadoop-linux-use-ssh-windows.md)

1. Uue nimega __tiny_graph.txt__faili loomiseks kasutada järgmist:

        nano tiny_graph.txt

    Kasutage seda faili sisu järgmine:

        [0,0,[[1,1],[3,3]]]
        [1,0,[[0,1],[2,2],[3,1]]]
        [2,0,[[1,2],[4,4]]]
        [3,0,[[0,3],[1,1],[4,4]]]
        [4,0,[[3,4],[2,4]]]

    Andmed kirjeldatakse suunatud graph, vormingus objektide vahel seose [allikas\_id, allikas\_väärtus, [[dest\_id], [serva\_väärtus];...]]. Iga rida tähistab vahelise seose lisamine **allika\_id** objekti ja üks või mitu **dest\_id** objektid. Funktsiooni **serva\_väärtus** (või kaal) saab kirjeldada kui tugevus või **source_id** vahelise ühenduse kaugus ja **dest\_id**.

    Välja tõmmata ja objektide vahelise kauguse väärtus (või kaal) kasutades, ülaltoodud andmete võib välja nägema järgmine:

    ![joonistatud ringid koos tekstiridade vahelise kauguse erineva nimega tiny_graph.txt](./media/hdinsight-hadoop-giraph-install-linux/giraph-graph.png)

2. Faili salvestamiseks kasutage __Klahvikombinatsiooni Ctrl + X__, seejärel __Y__ja lõpuks __Enter__ faili nimi.

3. Andmete talletamiseks üheks esmane salvestusruum klaster Hdinsightiga kasutage järgmist:

        hdfs dfs -put tiny_graph.txt /example/data/tiny_graph.txt

4. Käivitage järgmine käsk SimpleShortestPathsComputation näide.

         yarn jar /usr/hdp/current/giraph/giraph-examples.jar org.apache.giraph.GiraphRunner org.apache.giraph.examples.SimpleShortestPathsComputation -ca mapred.job.tracker=headnodehost:9010 -vif org.apache.giraph.io.formats.JsonLongDoubleFloatDoubleVertexInputFormat -vip /example/data/tiny_graph.txt -vof org.apache.giraph.io.formats.IdWithValueTextOutputFormat -op /example/output/shortestpaths -w 2

    Järgmises tabelis on kirjeldatud selle käsu kasutamisel parameetrid.

  	| Parameetri | Mida see tähendab? |
  	| --------- | ------------ |
  	| `jar /usr/hdp/current/giraph/giraph-examples.jar` | Jar fail, mis sisaldab näiteid. |
  	| `org.apache.giraph.GiraphRunner` | Klassi käivitamiseks näidetes kasutatud. |
  	| `org.apache.giraph.examples.SimpleShortestPathsCoputation` | Näide, mis on parandusfunktsiooni. Sel juhul see arvutab lühima tee ID 1 ja kõik muud ID graafik vahel. |
  	| `-ca mapred.job.tracker=headnodehost:9010` | Headnode klaster jaoks. |
  	| `-vif org.apache.giraph.io.formats.JsonLongDoubleFloatDoubleVertexInputFromat` | Kasutada sisendandmete sisestamise vorming. |
  	| `-vip /example/data/tiny_graph.txt` | Sisendandmete fail. |
  	| `-vof org.apache.giraph.io.formats.IdWithValueTextOutputFormat` | Vorming. Sel juhul ID ja väärtuse lihttekstina. |
  	| `-op /example/output/shortestpaths` | Väljastuskoht. |
  	| `-w 2` | Töötajate kasutada arvu. Sel juhul 2. |

    Neid ja muid parameetreid kasutatakse Giraph näidised kohta leiate lisateavet teemast [Giraph Kiirjuhend](http://giraph.apache.org/quick_start.html).

5. Kui töö on lõpule jõudnud, salvestatakse tulemused kuvatakse __wasbs: / / / näide/välja/shotestpaths__ kataloogi. Alguses on loodud faile __osa – m -__ ja lõpus esimese, teise, jne faili näitava arvu. Kasutage järgmist vaatamiseks väljund.

        hdfs dfs -text /example/output/shortestpaths/*

    Väljund peaks kuvatama umbes järgmine:

        0   1.0
        4   5.0
        2   2.0
        1   0.0
        3   1.0

    SimpleShortestPathComputation, näiteks on raske kodeeritud alustada objekti ID-d 1 ja muude objektide lühima tee otsimine. Nii, et lugeda väljund `destination_id distance`, kus vahemaa on objekti ID 1 ja sihtrakenduse ID vahel tegi servade väärtuse (või kaal)

    Visualiseerimine seda, saate seda kontrollida tulemused reisil lühima teed ID 1 ja kõigi muude objektide vahel. Pange tähele, et lühima tee ID-d 1-ID 4 on 5. See on kokku vahemaa <span style="color:orange">ID 1 ja 3</span>, ja seejärel <span style="color:red">ID 3 ja 4</span>.

    ![Joonise objektid ringid koos lühikese teed vahel](./media/hdinsight-hadoop-giraph-install-linux/giraph-graph-out.png)


## <a name="next-steps"></a>Järgmised sammud

- [Installimine ja kasutamine kogumite tooni Hdinsightiga kohta](hdinsight-hadoop-hue-linux.md). Tooni on web kasutajaliides, mis hõlbustab loomine, käivitamine ja Salvesta siga ja taru töö, samuti vaikimisi salvestusruumi otsige oma Hdinsightiga klaster.

- [Hdinsightiga kogumite R installimine](hdinsight-hadoop-r-scripts-linux.md): juhised, kuidas kasutada klaster installida ja kasutada R Hdinsightiga Hadoopi kogumite kohandamine. R on avatud lähtekoodi keel ja statistika arvutuste jaoks. Pakub sadu sisseehitatud statistikafunktsioonid ja oma programmeerimiskeel, mis ühendab objekti rakendusse ja funktsionaalne programmeerimise aspekte. See sisaldab ka olulisel graafiline võimalusi.

- [Installige Solri Hdinsightiga kogumite kohta](hdinsight-hadoop-solr-install-linux.md). Kobar kohandamise abil saate installida Solri Hdinsightiga Hadoopi kogumite. Solri võimaldab võimas otsing toiminguid talletatud andmed.
