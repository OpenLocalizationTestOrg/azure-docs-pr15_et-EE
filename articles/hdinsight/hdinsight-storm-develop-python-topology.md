<properties
   pageTitle="Python komponentide kasutamine Storm topoloogia kohta Hdinsightiga | Microsoft Azure'i"
   description="Kuidas saate Python komponentide Apache torm Windows Azure Hdinsightiga kohta teada. Saate teada, kuidas kasutada nii Java vastavalt Python komponente ja Clojure vastavalt Storm topoloogia."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="python"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/27/2016"
   ms.author="larryfr"/>

#<a name="develop-apache-storm-topologies-using-python-on-hdinsight"></a>Arendamise Apache Storm topoloogiatest Python kasutamine Hdinsightiga

Apache Storm toetab mitut keelt, isegi võimaldab teil ühendada mitu keelt ühe topoloogia komponente. Selles dokumendis saate teada, kuidas Python komponentide kasutamine oma Java ja Clojure põhinev Storm topoloogiatest Hdinsightiga kohta.

##<a name="prerequisites"></a>Eeltingimused

* Python 2.7 või uuem versioon

* Java JDK 1.7 või uuem versioon

* [Leiningen](http://leiningen.org/)

##<a name="storm-multi-language-support"></a>Torm mitme keele tuge

Torm on mõeldud töötamiseks kirjutada, kasutades mis tahes programmeerimiskeel, kuid selleks, et komponendid aru saada, kuidas töötada [ökonoomsus määratlus Storm](https://github.com/apache/storm/blob/master/storm-core/src/storm.thrift)komponendid. Python, jaoks on esitatud mooduli Apache Storm projekti, mis võimaldab teil hõlpsasti kasutajaliides, kus torm. Leiate selle mooduli [https://github.com/apache/storm/blob/master/storm-multilang/python/src/main/resources/resources/storm.py](https://github.com/apache/storm/blob/master/storm-multilang/python/src/main/resources/resources/storm.py).

Kuna Apache Storm on Java protsess, mida käitatakse klõpsake Java virtuaalse masina (töötab), on teistes keeltes komponendid täidetakse subprocesses. Torm bittide, töötab soovitud töötab suhtleb nende subprocesses JSON saadetud üle stdin/stdout abil. Üksikasjalikumat teavet suhtlemine komponendid leiate dokumentatsiooni [mitme-lang Protocol (protokoll)](https://storm.apache.org/documentation/Multilang-protocol.html) .

###<a name="the-storm-module"></a>Torm mooduli

Torm mooduli (https://github.com/apache/storm/blob/master/storm-multilang/python/src/main/resources/resources/storm.py), pakub bittide, Python komponendid, mis töötavad Storm loomiseks vajalik.

See pakub näiteks `storm.emit` eraldavad kordseid, ja `storm.logInfo` kirjutamise logid. Kutsun üle selle faili lugeda ja mõista, mis pakub teile.

##<a name="challenges"></a>Probleemid

__Storm.py__ mooduli kasutamise, saate luua Python otsikuid, mis kasutavad andmete ja poldid andmeid, aga üldine Storm topoloogia mõiste, et juhtmed üles suhtlemine komponendid on endiselt kirjutatud Java või Clojure abil. Lisaks, kui kasutate Java, peate looma ka Java komponendid, mis Python komponentide pidama.

Ka, kuna torm kogumite käivitada jaotatud mood, peab veenduge, et kõik nõutud oma Python komponendid moodulid on saadaval kõigi töötaja sõlmed klaster. Torm ei paku mis tahes lihtne viis mitme-lang ressursid saavutamiseks – kas teil on kaasata kõik sõltuvused topoloogia jar-faili osana või käsitsi installimine sõltuvused iga töötaja sõlme klaster.

###<a name="java-vs-clojure-topology-definition"></a>Java vs Clojure topoloogia määratlus

Määratlemise topoloogia kaks meetodit Clojure on kõige on kõige lihtsam/puhtaim, kui saate otse referenc python komponendid topoloogia määratlus. Java-põhine topoloogia määratlused, tuleb teil määratleda Java komponendid, mis asju deklareerimise väljad soovitud kordseid tagastatud Python komponendid toime.

Allpool on mõlema meetodi on kirjeldatud selle dokumendi koos näide.

##<a name="python-components-with-a-java-topology"></a>Java topoloogia koos Python komponendid

> [AZURE.NOTE] Selles näites on saadaval veebisaidil [https://github.com/Azure-Samples/hdinsight-python-storm-wordcount](https://github.com/Azure-Samples/hdinsight-python-storm-wordcount) __JavaTopology__ kataloogis. See on Maven vastavalt projekti. Kui olete varem selliseid Maven, leiate lisateavet Maven projekti jaoks Storm topoloogia loomise kohta [arendamise Java-põhine topoloogiatest Apache torm Hdinsightiga kohta](hdinsight-storm-develop-java-topology.md) .

Java-põhine topoloogia, mis kasutab Python (või muud töötab keele komponendid) kuvatakse algselt kasutada Java komponendid; Aga kui vaadata iga Java otsikuid või poldid, kuvatakse teile järgmine kood:

    public SplitBolt() {
        super("python", "countbolt.py");
    }

See on kui Java viitab Python ja skript, mis sisaldab tegeliku polt loogika töötab. Java otsikuid/poldid (nt selle) deklareerida lihtsalt korteeži, mille aluseks Python komponent elektritootmisel väljad.

Python faile talletatakse selle `/multilang/resources` kataloog selles näites. Funktsiooni `/multilang` directory viidatakse __pom.xml__:

<resources>
    <resource>
        <!-- Where the Python bits are kept -->
        <directory>${basedir} / multilang</directory>
    </resource>
</resources>

See hõlmab kõik failid on `/multilang` jar, mis on selle projekti ehitatud kausta.

> [AZURE.IMPORTANT] Pange tähele, et seda määrab ainult selle `/multilang` directory ja ei `/multilang/resources`. Torm eeldab, et-töötab ressursside lisamine `resources` kaust, et seda on vaadanud ettevõttesiseselt juba. Pannes selle kausta komponendid võimaldab teil just juhend Java kood nime järgi. Näiteks `super("python", "countbolt.py");`. Teine võimalus, et on torm saavad vaadata selle `resources` kataloogi root (/) kasutamisel mitme-lang ressursid.
>
> Selles näites projekti, on `storm.py` mooduli sisaldub selle `/multilang/resources` directory.

###<a name="build-and-run-the-project"></a>Koostamine ja projekti käivitamine

Selle projekti kohalikult käivitamiseks kasutage luua ja käivitada kohalikus režiimis lihtsalt Maven järgmine käsk:

    mvn compile exec:java -Dstorm.topology=com.microsoft.example.WordCount

Protsessi jääda klahvikombinatsiooniga ctrl + c.

Juurutada Hdinsightiga kobar, mis töötab Apache torm projekt, siis tehke järgmist:

1. Koostamine on uber jar:

        mvn package

    See loob fail nimega __WordCount--1.0-SNAPSHOT.jar__ sisse selle `/target` selle projekti kataloog.

2. Jar faili üleslaadimine Hadoopi kobar, kasutades ühte järgmistest:

    * __Linux-põhine__ Hdinsightiga kogumite jaoks: kasutamine `scp WordCount-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:WordCount-1.0-SNAPSHOT.jar` jar faili kopeerimiseks kobar, asendades Kasutajanimi oma SSH kasutajanimi ja CLUSTERNAME Hdinsightiga kobar nimi.

        Kui fail on lõpule jõudnud, üles, kasutades SSH klaster ühenduse loomiseks ja topoloogia kasutamise alustamine`storm jar WordCount-1.0-SNAPSHOT.jar com.microsoft.example.WordCount wordcount`

    * __Windowsi-põhiste__ Hdinsightiga kogumite jaoks: ühenduse Storm armatuurlaua minnes HTTPS://CLUSTERNAME.azurehdinsight.net/ brauseris. Asendage CLUSTERNAME oma Hdinsightiga kobar nimi ja administraatori nimi ja parool, kui seda küsitakse.

        Vormi abil teha järgmisi toiminguid:

        * __Jar fail__: valige __Sirvi__ja seejärel valige __WordCount-1,0-SNAPSHOT.jar__ fail
        * __Klassi nimi__: sisestage`com.microsoft.example.WordCount`
        * __Täiendavad Paramters__: Sisestage sõbralik nimi, näiteks `wordcount` topoloogia tuvastamiseks

        Lõpetuseks, valige __Edasta__ topoloogia käivitamiseks.

> [AZURE.NOTE] Käivitatud Storm topoloogia töötab kuni peatamiseni (tapetud). Topoloogia peatamiseks kasutada ühte selle `storm kill TOPOLOGYNAME` käsk käsurea (SSH seansi Linux arvutikobaras näiteks) või torm Kasutajaliidese abil valige topoloogia, ja seejärel valige nupp __tappa__ .

##<a name="python-components-with-a-clojure-topology"></a>Python komponente Clojure topoloogia

> [AZURE.NOTE] Selles näites on saadaval veebisaidil [https://github.com/Azure-Samples/hdinsight-python-storm-wordcount](https://github.com/Azure-Samples/hdinsight-python-storm-wordcount) __ClojureTopology__ kataloogis.

See topoloogia on loodud, kasutades [Leiningen](http://leiningen.org) [Clojure uue projekti](https://github.com/technomancy/leiningen/blob/stable/doc/TUTORIAL.md#creating-a-project)loomiseks. Pärast seda, scaffolded projekti järgmised muudatused on tehtud:

* __Project.CLJ__: lisatud sõltuvused torm ja tühistamiste üksused, mis võib põhjustada probleem, kui Hdinsightiga serveris juurutatud.
* __ressursid/ressursid__: Leiningen luuakse vaikimisi `resources` kataloogi, aga siin salvestatud faile kuvada saada lisatud jar fail, mis on loodud selle projekti juurkaustas ja Storm eeldab, et kataloogi all faile `resources`. Et sub directory lisati ja Python failid talletatakse `resources/resources`. Käivita ajal see käsitletakse juursait (/) juurdepääsuks Python komponendid.
* __src/wordcount/Core.CLJ__: see fail sisaldab topoloogia määratluse ja __project.clj__ failist viidatud. Määratleda Storm topoloogia Clojure kasutamise kohta leiate lisateavet teemast [Clojure DSL](https://storm.apache.org/documentation/Clojure-DSL.html).

###<a name="build-and-run-the-project"></a>Koostamine ja projekti käivitamine

__Luua ja käivitada projekti kohalikult__, kasutage järgmist käsku:

    lein clean, run

Topoloogia peatamiseks kasutage __Klahvikombinatsiooni Ctrl + C__.

__Mõne uberjar luua ja juurutada Hdinsightiga__, siis tehke järgmist:

1. Mõne uberjar topoloogia ja sõltuvuste nõutud sisaldavad loomiseks tehke järgmist.

        lein uberjar

    See loob uue faili nimega `wordcount-1.0-SNAPSHOT.jar` klõpsake soovitud `target\uberjar+uberjar` kataloogi.
    
2. Kasutage üht järgmistest meetoditest juurutada ja käivitada topoloogia on Hdinsightiga arvutikobaras.

    * __Linuxi-põhiste Hdinsightiga__
    
        1. Faili kopeerimine Hdinsightiga kobar pea sõlme abil `scp`. Näiteks:
        
                scp wordcount-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:wordcount-1.0-SNAPSHOT.jar
                
            Asendage kasutajanimi SSH kasutaja kobar ja CLUSTERNAME Hdinsightiga kobar nimega.
            
        2. Kui fail on kopeeritud klaster, kasutage SSH ühenduse klaster ja esitage töö. Hdinsightiga SSH kasutamise kohta leiate artiklist ühte järgmistest:
        
            * [Kasutada SSH Linux-põhine Hdinsightiga Linux, Unix või OS X](hdinsight-hadoop-linux-use-ssh-unix.md)
            * [Kasutada SSH Windows Linux-põhine Hdinsightiga](hdinsight-hadoop-linux-use-ssh-windows.md)
            
        3. Kui ühendus on loodud, kasutage topoloogia alustamiseks järgmist:
        
                storm jar wordcount-1.0-SNAPSHOT.jar wordcount.core wordcount
    
    * __Windowsi-põhiste Hdinsightiga__
    
        1. Ühenduse loomine armatuurlaua Storm minnes HTTPS://CLUSTERNAME.azurehdinsight.net/ brauseris. Asendage CLUSTERNAME oma Hdinsightiga kobar nimi ja administraatori nimi ja parool, kui seda küsitakse.

        2. Vormi abil teha järgmisi toiminguid:

            * __Jar fail__: valige __Sirvi__ja seejärel valige __Wordcount-1,0-SNAPSHOT.jar__ fail
            * __Klassi nimi__: sisestage`wordcount.core`
            * __Täiendavad Paramters__: Sisestage sõbralik nimi, näiteks `wordcount` topoloogia tuvastamiseks

            Lõpetuseks, valige __Edasta__ topoloogia käivitamiseks.

> [AZURE.NOTE] Käivitatud Storm topoloogia töötab kuni peatamiseni (tapetud). Topoloogia peatamiseks kasutada ühte selle `storm kill TOPOLOGYNAME` käsk käsurea (SSH seansi Linux arvutikobaras,) või torm Kasutajaliidese abil valige topoloogia, ja seejärel valige nupp __tappa__ .

##<a name="next-steps"></a>Järgmised sammud

Selles dokumendis soovite õpitut kasutamine Python komponendid Storm topoloogia kaudu. Vaadake muid võimalusi, Python kasutamiseks Hdinsightiga järgmised dokumendid:

* [Kuidas kasutada Python streaming MapReduce tööde haldamine](hdinsight-hadoop-streaming-python.md)
* [Kuidas kasutada Python kasutaja määratletud funktsioonid (UDF) siga ja taru](hdinsight-python.md)
