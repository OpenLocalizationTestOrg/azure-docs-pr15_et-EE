<properties
    pageTitle="Linux-põhine Hdinsightiga installida R | Microsoft Azure'i"
    description="Saate teada, kuidas installida ja kasutada R Linux-põhine Hadoopi kogumite kohandada."
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="larryfr"/>

# <a name="install-and-use-r-on-hdinsight-hadoop-clusters"></a>Installimine ja kasutamine R Hdinsightiga Hadoopi kogumite

Saate installida R mis tahes tüüpi kobar rakenduses Hadoopi Hdinsightiga kohta, kasutades **Skripti toimingu** kobar kohandamine. See võimaldab andmeteadlaste ja ärianalüütikud R abil saate juurutada võimsaid MapReduce NIITIDE programmeerimise raames töötlemiseks suurte andmehulkade Hadoopi kogumite Hdinsightiga juurutatud kohta.

> [AZURE.IMPORTANT] [Premium taseme](https://azure.microsoft.com/pricing/details/hdinsight/) Hdinsightiga pakkumine sisaldab R Server klaster Hdinsightiga osana. See võimaldab kasutada MapReduce ja säde jaotatud arvutuste R skriptide. Lisateabe saamiseks vt [R serveris Hdinsightiga kasutamise alustamine](hdinsight-hadoop-r-server-get-started.md). 


## <a name="what-is-r"></a>Mis on R?

<a href="http://www.r-project.org/" target="_blank">R projekti statistika arvutuste jaoks</a> on avatud allikas keel ja statistika arvutuste jaoks. R leiate sadu sisseehitatud statistikafunktsioonid ja oma programmeerimiskeel, mis ühendab objekti rakendusse ja funktsionaalne programmeerimine aspekte. See sisaldab ka olulisel graafiline võimalusi. R on eelistatud programmeerimise keskkonna kõige professionaalne statistikute ja teadlased erinevaid väljad.

R skriptide saab käivitada Hadoopi kogumite Hdinsightiga, mis on kohandatud skript toimingu loomisel abil installida R keskkonnas. R on ühilduvad koos Azure'i bloobimälu salvestusruumi (WASB) nii, et seal talletatakse andmeid saab töödelda R kasutamine Hdinsightiga.

## <a name="what-the-script-does"></a>Mida tähendab skript

Skripti toimingu kasutatud installida R Hdinsightiga klaster installitakse järgmised Ubuntu paketid, mis pakuvad R installimise põhilised:

* [r-base](http://packages.ubuntu.com/precise/r-base): Base GNU R pakett
* [r-base-arendaja](http://packages.ubuntu.com/precise/r-base-dev): Auxilliary GNU R pakettide

Järgmised RHadoop paketid installitud on ka, mis annab integreerimine MapReduce ja HDFS:

* [rmr2](https://github.com/RevolutionAnalytics/rmr2): võimaldab arendajatel R Hadoopi MapReduce kasutamine
* [rhdfs](https://github.com/RevolutionAnalytics/rhdfs): võimaldab arendajatel R kasutada Hadoopi HDFS (WASB Hdinsightiga jaoks)

Lisaks on installitud järgmised R paketid:

| R pakett | Mida see annab |
| --------- | ---------------- |
| [rJava](https://cran.r-project.org/web/packages/rJava/index.html) | Madal R Java liides. |
| [Rcpp](https://cran.r-project.org/web/packages/Rcpp/index.html) | R- ja C++ integreerimine. |
| [RJSONIO](https://cran.r-project.org/web/packages/RJSONIO/index.html) | JSON objektide serialiseerida/andmeatribuutide R |
| [bitops](https://cran.r-project.org/web/packages/bitops/index.html) | Funktsioonid täisarv vektorite bititaseme toiminguid. |
| [Digest](https://cran.r-project.org/web/packages/digest/index.html) | Looge Cryptographic räsi Ülevaateväljaanded R objektid. |
| [otstarbekas](https://cran.r-project.org/web/packages/functional/index.html) | Karri, koostamine ja muud funktsioonid-järjekorras |
| [reshape2](https://cran.r-project.org/web/packages/reshape2/index.html) | Paindlikult ümber korraldada ja andmeid. |
| [stringr](https://cran.r-project.org/web/packages/stringr/index.html) | Lihtne, ühtsete ümbriste levinud Stringitoimingutest. |
| [plyr](https://cran.r-project.org/web/packages/plyr/index.html) | Tükeldamise, rakendamise ja andmeid kombineerides tööriistad. |
| [caTools](https://cran.r-project.org/web/packages/caTools/index.html) | Tööriistad teisaldamise akna statistika, GIF-, Base64, ROC AUC, jne. |
| [stringdist](https://cran.r-project.org/web/packages/stringdist/index.html) | Ühtlustada stringi sobitamine ja tekstistringi vahemaa funktsioone. |

## <a name="install-r-using-script-actions"></a>Installige R skripti toimingute kasutamine

Skripti järgmist toimingut kasutatakse installida R on Hdinsightiga kobar. https://hdiconfigactions.blob.Core.Windows.net/linuxrconfigactionv01/r-Installer-V01.sh
    
Sellest jaotisest leiate juhised selle kohta, kuidas kasutada skripti Azure portaalis uus klaster loomisel. 

> [AZURE.NOTE] Azure'i PowerShelli, Azure'i CLI, Hdinsightiga .NET SDK või Azure ressursihaldur malle saab kasutada skripti toimingute. Saate rakendada ka skripti toimingud juba töötab kogumite. Lisateabe saamiseks vt [kohandamine Hdinsightiga kogumite skripti toimingute](hdinsight-hadoop-customize-cluster-linux.md).

1. Alustage ettevalmistamise klaster juhiste abil [säte Linux-põhine Hdinsightiga](hdinsight-hadoop-provision-linux-clusters.md#portal)rühmades, kuid täitke ettevalmistamise.

2. **Valikuline konfigureerimine** enne, valige **Skripti toimingud**ja allolev teave:

    * __Nimi__: sisestage skripti toimingu sõbralik nimi.
    * __Skripti URI__: https://hdiconfigactions.blob.core.windows.net/linuxrconfigactionv01/r-installer-v01.sh
    * __Juht__: märkige see ruut
    * __Töötaja__: märkige see ruut
    * __ZOOKEEPER__: märkige see suvand, et installida Zookeeper sõlme.
    * __Parameetrite__: jätke see väli tühjaks

3. **Skripti toimingud**allosas kasutada konfiguratsiooni salvestamiseks **Valige** nupp. Lõpuks nuppu **Valige** **Valikuline konfigureerimine** tera allosas salvestamiseks kasutage valikuline konfiguratsiooniteavet.

4. Jätkake ettevalmistamise klaster, nagu on kirjeldatud [sätte Linux-põhine Hdinsightiga](hdinsight-hadoop-provision-linux-clusters.md#portal)rühmades.

## <a name="run-r-scripts"></a>R skriptide käitamiseks

Kui klaster on lõpule jõudnud, ettevalmistamise, kasutada järgmist R klaster MapReduce toimingu sooritamiseks.

1. Ühendada Hdinsightiga klaster SSH abil.

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    Hdinsightiga SSH kasutamise kohta lisateabe saamiseks vt järgmist:

    * [Kasutada SSH Linux-põhine Hadoopi Hdinsightiga Linux, Unix või OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [Kasutada SSH Linux-põhine Hadoopi Windows Hdinsightiga](hdinsight-hadoop-linux-use-ssh-windows.md)

2. Kaudu soovitud `username@hn0-CLUSTERNAME:~$` Küsi, sisestage järgmine käsk interaktiivsed R-seansi alustamine:

        R

3. Sisestage järgmine R programm. See loob arvude 1 kuni 100 ning seejärel korrutab need arvuga 2.

        library(rmr2)
        ints = to.dfs(1:100)
        calc = mapreduce(input = ints, map = function(k, v) cbind(v, 2*v))

    Esirea kõned RHadoop teegi rmr2, mida kasutatakse MapReduce toimingud.

    Teisel real genereeritud väärtused 1-100, siis salvestab need Hadoopi fail süsteemi abil `to.dfs`.

    Kolmanda rea loob MapReduce protsessi abil rmr2 funktsioonid ja algab töötlemine. Peaksite nägema kerige viimase töötlemine algab mitu rida.

4. Järgmiseks ajutine tee, mis on talletatud MapReduce väljundi kuvamiseks kasutada järgmist:

        print(calc())

    See peaks olema midagi sarnast `/tmp/file5f615d870ad2`. Tegelik väljund kuvamiseks tehke järgmist:

        print(from.dfs(calc))

    Väljund peaks välja nägema umbes järgmine:

        [1,]  1 2
        [2,]  2 4
        .
        .
        .
        [98,]  98 196
        [99,]  99 198
        [100,] 100 200

5. R välju, sisestage järgmine:

        q()


## <a name="next-steps"></a>Järgmised sammud

- [Installimine ja kasutamine kogumite tooni Hdinsightiga kohta](hdinsight-hadoop-hue-linux.md). Tooni on web kasutajaliides, mis hõlbustab loomine, käivitamine ja Salvesta siga ja taru töö, samuti vaikimisi salvestusruumi otsige oma Hdinsightiga klaster.

- [Installige Giraph Hdinsightiga kogumite kohta](hdinsight-hadoop-giraph-install.md). Kobar kohandamise abil saate installida Giraph Hdinsightiga Hadoopi kogumite. Giraph võimaldab teil teha Graphi töötlemine Hadoopi abil ja seda saab kasutada Windows Azure Hdinsightiga.

- [Installige Solri Hdinsightiga kogumite kohta](hdinsight-hadoop-solr-install.md). Kobar kohandamise abil saate installida Solri Hdinsightiga Hadoopi kogumite. Solri võimaldab salvestatud andmeid võimas otsing toiminguid.

- [Installige tooni Hdinsightiga kogumite kohta](hdinsight-hadoop-hue-linux.md). Kobar kohandamise abil saate installida tooni Hdinsightiga Hadoopi kogumite. Värvitoon on suhelda Hadoopi kobar veebirakenduste komplekt.

[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
