<properties
   pageTitle="Java-põhine topoloogiatest arendamise Apache Storm | Microsoft Azure'i"
   description="Saate teada, kuidas luua Storm topoloogiatest Java lihtne sõna count topoloogia loomisega."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="java"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/14/2016"
   ms.author="larryfr"/>

#<a name="develop-java-based-topologies-for-a-basic-word-count-application-with-apache-storm-and-maven-on-hdinsight"></a>Arendamise Java-põhine topoloogiatest lihtsa sõnade rakenduse Apache torm ja Maven Hdinsightiga kohta

Saate teada, kuidas luua Java-põhine topoloogia Apache Storm klõpsake Hdinsightiga Maven abil. Saate luua lihtsa sõnade rakenduse Maven ja Java, kus topoloogia on määratletud Java abil loomise protsess juhendab. Seejärel saate teada, kuidas määratleda topoloogia, kasutades Flux raames.

> [AZURE.NOTE] Flux raames on saadaval Storm 0.10.0 või uuem versioon. Torm 0.10.0 on saadaval Hdinsightiga 3.3 ja 3.4.

Pärast selle dokumendi juhiseid, on teil tavaline topoloogia, juurutamist Apache torm Hdinsightiga kohta.

> [AZURE.NOTE] Selles dokumendis loodud topoloogiatest täidetud versioon on saadaval veebisaidil [https://github.com/Azure-Samples/hdinsight-java-storm-wordcount](https://github.com/Azure-Samples/hdinsight-java-storm-wordcount).

##<a name="prerequisites"></a>Eeltingimused

* <a href="https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html" target="_blank">Java arendaja Kit (JDK) versioon 7</a>

* <a href="https://maven.apache.org/download.cgi" target="_blank">Maven</a>: Maven on Java projektide projekti koostamine süsteem.

* Tekstiredaktoris, nt Notepadis <a href="http://www.gnu.org/software/emacs/" target="_blank">Emacs<a>, <a href="http://www.sublimetext.com/" target="_blank">Ülev teksti</a>, <a href="https://atom.io/" target="_blank">Atom.io</a>, <a href="http://brackets.io/" target="_blank">Brackets.io</a>. Või kasutage integreeritud arenduskeskkond (IDE) näiteks <a href="https://eclipse.org/" target="_blank">Eclipse</a> (versioon Luna või uuem versioon).

    > [AZURE.NOTE] Teie redigeerija või IDE võib-olla teatud funktsioonid töötavad Maven, mis on adresseeritud pole selles dokumendis. Redigeerimise keskkonna võimaluste kohta lisateabe saamiseks vt kasutatava toote dokumentatsioonist.

##<a name="configure-environment-variables"></a>Keskkonna muutujaid konfigureerimine

Järgmised keskkonna muutujad on seada Java ja selle JDK installimisel. Siiski peaksite, et need on olemas ja sisaldaksid teie süsteemi õigete väärtuste.

* Kuhu on installitud Java runtime keskkond (JRE) osutama **JAVA_HOME** . Näiteks Unix või Linux jaotus peaks olema umbes väärtus `/usr/lib/jvm/java-7-oracle`. Klõpsake Windowsi oleks väärtus, mis on sarnane`c:\Program Files (x86)\Java\jre1.7`

* **Tee** - peaks sisaldama järgmine tee:

    * **JAVA_HOME** (või samaväärne tee)

    * **JAVA_HOME\bin** (või samaväärne tee)

    * Kuhu on installitud Maven

##<a name="create-a-new-maven-project"></a>Maven uue projekti loomine

Käsurea, nimega **WordCount**Maven uue projekti loomiseks kasutada järgmine kood:

    mvn archetype:generate -DarchetypeArtifactId=maven-archetype-quickstart -DgroupId=com.microsoft.example -DartifactId=WordCount -DinteractiveMode=false

See loob uue kataloogi **WordCount** praegusesse asukohta, mis sisaldab Maven lihtprojektide.

**WordCount** register sisaldab järgmist:

* **POM.XML**: sisaldab Maven projekti sätted.

* **src\main\java\com\microsoft\example**: sisaldab rakenduse koodis.

* **src\test\java\com\microsoft\example**: sisaldab kontrollib rakenduse. Selle näite puhul ei loome kontrollib.

###<a name="remove-the-example-code"></a>Näide koodi eemaldamine

Kuna loome meie rakendus, kustutada loodud test ja rakenduse failid:

*  **src\test\java\com\microsoft\example\AppTest.Java**

*  **src\main\java\com\microsoft\example\App.Java**

##<a name="add-properties"></a>Atribuutide lisamine

Maven võimaldab määratleda projekti tasemel väärtused nimega atribuudid. Lisage järgmine pärast selle `<url>http://maven.apache.org</url>` joon:

    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <!--
        Storm 0.10.0 is for HDInsight 3.3 and 3.4.
        To find the version information for earlier HDInsight cluster
        versions, see https://azure.microsoft.com/en-us/documentation/articles/hdinsight-component-versioning/
        -->
        <storm.version>0.10.0</storm.version>
    </properties>

Nüüd saame kasutada neid väärtusi muudesse sektsioonidesse. Näiteks torm komponendid versiooni tingimusreeglite me kasutame `${storm.version}` asemel raske kodeerimine väärtus.

##<a name="add-dependencies"></a>Lisage sõltuvused

Kuna tegemist on torm topoloogia, peate lisama sõltuvus Storm komponendid. Avage fail **pom.xml** ja lisage järgmine kood on ** &lt;sõltuvused >** jaotis:

    <dependency>
      <groupId>org.apache.storm</groupId>
      <artifactId>storm-core</artifactId>
      <version>${storm.version}</version>
      <!-- keep storm out of the jar-with-dependencies -->
      <scope>provided</scope>
    </dependency>

Kompileerimise ajal Maven kasutab seda teavet Maven hoidla **torm tuumaga** otsimiseks. See esmalt välja hoidla kohalikus arvutis. Kui failid puuduvad, see laadida avaliku Maven hoidla ja salvestaks need kohaliku hoidla.

> [AZURE.NOTE] Teade kuvatakse `<scope>provided</scope>` lisasime jaotises joon. See ütleb Maven jätta **torm tuumaga** JAR faile saame luua, kuna see on esitatud süsteemi. See võimaldab paketid pisut väiksemaks loomine ja see tagab, et nad kasutavad **torm tuumaga** bittide kaasatud torm Hdinsightiga kobar kohta.

##<a name="build-configuration"></a>Koostage konfigureerimine

Maven lisandmoodulite võimaldavad kohandada koostamine etappide projekt, nt kuidas projekti on koostanud või kuidas see pakett JAR faili. Avage fail **pom.xml** ja lisage järgmine kood kohal on `</project>` joon.

    <build>
      <plugins>
      </plugins>
      <resources>
      </resources>
    </build>

Selles jaotises kasutatakse lisandmoodulite, materjale ja muid Koosta konfiguratsiooni võimalusi lisada. Vaadake __pom.xml__ faili täielik viite [http://maven.apache.org/pom.html](http://maven.apache.org/pom.html).

###<a name="add-plug-ins"></a>Lisandmoodulite lisamine

Torm topoloogiatest, <a href="http://mojo.codehaus.org/exec-maven-plugin/" target="_blank">Exec Maven lisandmoodul</a> on kasulik, sest see võimaldab teil hõlpsasti käivitamiseks topoloogia kohalikult oma arenduskeskkond. Lisatakse järgmised on `<plugins>` osa **pom.xml** faili lisada Exec Maven lisandmoodul:

    <plugin>
      <groupId>org.codehaus.mojo</groupId>
      <artifactId>exec-maven-plugin</artifactId>
      <version>1.4.0</version>
      <executions>
        <execution>
        <goals>
          <goal>exec</goal>
        </goals>
        </execution>
      </executions>
      <configuration>
        <executable>java</executable>
        <includeProjectDependencies>true</includeProjectDependencies>
        <includePluginDependencies>false</includePluginDependencies>
        <classpathScope>compile</classpathScope>
        <mainClass>${storm.topology}</mainClass>
      </configuration>
    </plugin>

> [AZURE.NOTE] Pange tähele, et selle `<mainClass>` kirje kasutab `${storm.topology}`. Me ei määratleda see varem jaotises Atribuudid (kuid me võiks olla). Selle asemel me seada selle väärtuse käsurea käivitamisel topoloogia toimub hiljem oma arenduskeskkond.

Teise kasulik lisandmoodul on <a href="http://maven.apache.org/plugins/maven-compiler-plugin/" target="_blank">Apache Maven koostaja lisandmoodul</a>, mis on kasutatud koostamine suvandite muutmine. See peamine põhjus on Java versioon, mis kasutab Maven lähte-kui ka rakenduse sihtrakenduse muuta. Soovime versiooni 1.7.

Lisada järgmist funktsiooni `<plugins>` **pom.xml** fail võib olla Apache Maven koostaja lisandmoodul ja lähte- ja versioonide 1,7 osas.

    <plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-compiler-plugin</artifactId>
      <version>3.3</version>
      <configuration>
        <source>1.7</source>
        <target>1.7</target>
      </configuration>
    </plugin>

###<a name="configure-resources"></a>Ressursside konfigureerimine

Jaotises ressursid võimaldab kaasata-koodi ressursside nagu nõutud topoloogias failid. Selles näites lisage järgmist funktsiooni `<resources>` **pom.xml** faili jaotises.

    <resource>
        <directory>${basedir}/resources</directory>
        <filtering>false</filtering>
        <includes>
          <include>log4j2.xml</include>
        </includes>
    </resource>

Sellega lisatakse ressursid kataloogi projekti juurkaustas (`${basedir}`) sisaldab ressursse, mis sisaldab nimega __log4j2.xml__faili asukoht. Millist teavet logib topoloogia konfigureerimine kasutatakse seda faili.

##<a name="create-the-topology"></a>Topoloogia loomine

Java-põhine Storm topoloogia koosneb kolmest osast, et teil tuleb autor (või viite) sõltuvus.

* **Spouts**: loeb andmed väliseid allikad ja eraldab andmete voogu topoloogia sisse.

* **Poldid**: sooritamine töötlemine voogu kiiratava otsikuid või muud poldid ja eraldab ühe või mitme voogu.

* **Topoloogia**: määratleb, kuidas otsikuid ja poldid on korraldatud ning pakub sisenemiskoha topoloogia.

###<a name="create-the-spout"></a>Tila loomine

Väliste andmeallikate seadmine nõuded vähendamiseks eraldab järgmised tila lihtsalt juhusliku laused. See on muudetud versiooni tila, mis on esitatud [Storm-Starter näited](https://github.com/apache/storm/blob/0.10.x-branch/examples/storm-starter/src/jvm/storm/starter).

> [AZURE.NOTE] Näiteks tila, mis loeb välisest andmeallikast, leiate järgmised näited:
>
> * [TwitterSampleSPout](https://github.com/apache/storm/blob/0.10.x-branch/examples/storm-starter/src/jvm/storm/starter/spout/TwitterSampleSpout.java): näide tila, mis loeb Twitteri
>
> * [Torm-Kafka](https://github.com/apache/storm/tree/0.10.x-branch/external/storm-kafka): tila, mis loeb Kafka

Tila, nimega **RandomSentenceSpout.java** **src\main\java\com\microsoft\example** directory uue faili loomine ja kasutage järgmist sisu.

    /**
     * Licensed to the Apache Software Foundation (ASF) under one
     * or more contributor license agreements.  See the NOTICE file
     * distributed with this work for additional information
     * regarding copyright ownership.  The ASF licenses this file
     * to you under the Apache License, Version 2.0 (the
     * "License"); you may not use this file except in compliance
     * with the License.  You may obtain a copy of the License at
     *
     * http://www.apache.org/licenses/LICENSE-2.0
     *
     * Unless required by applicable law or agreed to in writing, software
     * distributed under the License is distributed on an "AS IS" BASIS,
     * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
     * See the License for the specific language governing permissions and
     * limitations under the License.
     */

     /**
      * Original is available at https://github.com/apache/storm/blob/master/examples/storm-starter/src/jvm/storm/starter/spout/RandomSentenceSpout.java
      */

    package com.microsoft.example;

    import backtype.storm.spout.SpoutOutputCollector;
    import backtype.storm.task.TopologyContext;
    import backtype.storm.topology.OutputFieldsDeclarer;
    import backtype.storm.topology.base.BaseRichSpout;
    import backtype.storm.tuple.Fields;
    import backtype.storm.tuple.Values;
    import backtype.storm.utils.Utils;

    import java.util.Map;
    import java.util.Random;

    //This spout randomly emits sentences
    public class RandomSentenceSpout extends BaseRichSpout {
      //Collector used to emit output
      SpoutOutputCollector _collector;
      //Used to generate a random number
      Random _rand;

      //Open is called when an instance of the class is created
      @Override
      public void open(Map conf, TopologyContext context, SpoutOutputCollector collector) {
      //Set the instance collector to the one passed in
        _collector = collector;
        //For randomness
        _rand = new Random();
      }

      //Emit data to the stream
      @Override
      public void nextTuple() {
      //Sleep for a bit
        Utils.sleep(100);
        //The sentences that will be randomly emitted
        String[] sentences = new String[]{ "the cow jumped over the moon", "an apple a day keeps the doctor away",
            "four score and seven years ago", "snow white and the seven dwarfs", "i am at two with nature" };
        //Randomly pick a sentence
        String sentence = sentences[_rand.nextInt(sentences.length)];
        //Emit the sentence
        _collector.emit(new Values(sentence));
      }

      //Ack is not implemented since this is a basic example
      @Override
      public void ack(Object id) {
      }

      //Fail is not implemented since this is a basic example
      @Override
      public void fail(Object id) {
      }

      //Declare the output fields. In this case, an sentence
      @Override
      public void declareOutputFields(OutputFieldsDeclarer declarer) {
        declarer.declare(new Fields("sentence"));
      }
    }

Mõne hetke läbi mõista, kuidas see tila töötab koodi kommentaare lugeda.

> [AZURE.NOTE] Kuigi see topoloogia kasutab ainult ühte tila, teised on mitu, et erinevatest allikatest pärit andmete siseneda topoloogia.

###<a name="create-the-bolts"></a>Poldid loomine

Poldid toime andmete töötlemise. See topoloogia on meil kaks poldid:

* **SplitSentence**: jagab kiiratava **RandomSentenceSpout** üksikuteks sõnadeks laused.

* **WordCount**: loendab, mitu korda on ilmnenud iga sõna.

> [AZURE.NOTE] Poldid saab sõna-sõnalt midagi teha, näiteks arvutus, püsimine või välise komponendid rääkida.

Kahe **SplitSentence.java** ja **WordCount.Java** kataloogis **src\main\java\com\microsoft\example** uusi faile luua. Kasutage failide sisu järgmine:

**SplitSentence**

    package com.microsoft.example;

    import java.text.BreakIterator;

    import backtype.storm.topology.BasicOutputCollector;
    import backtype.storm.topology.OutputFieldsDeclarer;
    import backtype.storm.topology.base.BaseBasicBolt;
    import backtype.storm.tuple.Fields;
    import backtype.storm.tuple.Tuple;
    import backtype.storm.tuple.Values;

    //There are a variety of bolt types. In this case, we use BaseBasicBolt
    public class SplitSentence extends BaseBasicBolt {

      //Execute is called to process tuples
      @Override
      public void execute(Tuple tuple, BasicOutputCollector collector) {
        //Get the sentence content from the tuple
        String sentence = tuple.getString(0);
        //An iterator to get each word
        BreakIterator boundary=BreakIterator.getWordInstance();
        //Give the iterator the sentence
        boundary.setText(sentence);
        //Find the beginning first word
        int start=boundary.first();
        //Iterate over each word and emit it to the output stream
        for (int end=boundary.next(); end != BreakIterator.DONE; start=end, end=boundary.next()) {
          //get the word
          String word=sentence.substring(start,end);
          //If a word is whitespace characters, replace it with empty
          word=word.replaceAll("\\s+","");
          //if it's an actual word, emit it
          if (!word.equals("")) {
            collector.emit(new Values(word));
          }
        }
      }

      //Declare that emitted tuples will contain a word field
      @Override
      public void declareOutputFields(OutputFieldsDeclarer declarer) {
        declarer.declare(new Fields("word"));
      }
    }

**WordCount**

    package com.microsoft.example;

    import java.util.HashMap;
    import java.util.Map;
    import java.util.Iterator;

    import backtype.storm.Constants;
    import backtype.storm.topology.BasicOutputCollector;
    import backtype.storm.topology.OutputFieldsDeclarer;
    import backtype.storm.topology.base.BaseBasicBolt;
    import backtype.storm.tuple.Fields;
    import backtype.storm.tuple.Tuple;
    import backtype.storm.tuple.Values;
    import backtype.storm.Config;

    // For logging
    import org.apache.logging.log4j.Logger;
    import org.apache.logging.log4j.LogManager;

    //There are a variety of bolt types. In this case, we use BaseBasicBolt
    public class WordCount extends BaseBasicBolt {
        //Create logger for this class
        private static final Logger logger = LogManager.getLogger(WordCount.class);
        //For holding words and counts
        Map<String, Integer> counts = new HashMap<String, Integer>();
        //How often we emit a count of words
        private Integer emitFrequency;

        // Default constructor
        public WordCount() {
            emitFrequency=5; // Default to 60 seconds
        }

        // Constructor that sets emit frequency
        public WordCount(Integer frequency) {
            emitFrequency=frequency;
        }

        //Configure frequency of tick tuples for this bolt
        //This delivers a 'tick' tuple on a specific interval,
        //which is used to trigger certain actions
        @Override
        public Map<String, Object> getComponentConfiguration() {
            Config conf = new Config();
            conf.put(Config.TOPOLOGY_TICK_TUPLE_FREQ_SECS, emitFrequency);
            return conf;
        }

        //execute is called to process tuples
        @Override
        public void execute(Tuple tuple, BasicOutputCollector collector) {
            //If it's a tick tuple, emit all words and counts
            if(tuple.getSourceComponent().equals(Constants.SYSTEM_COMPONENT_ID)
                    && tuple.getSourceStreamId().equals(Constants.SYSTEM_TICK_STREAM_ID)) {
                for(String word : counts.keySet()) {
                    Integer count = counts.get(word);
                    collector.emit(new Values(word, count));
                    logger.info("Emitting a count of " + count + " for word " + word);
                }
            } else {
                //Get the word contents from the tuple
                String word = tuple.getString(0);
                //Have we counted any already?
                Integer count = counts.get(word);
                if (count == null)
                    count = 0;
                //Increment the count and store it
                count++;
                counts.put(word, count);
            }
        }

        //Declare that we will emit a tuple containing two fields; word and count
        @Override
        public void declareOutputFields(OutputFieldsDeclarer declarer) {
            declarer.declare(new Fields("word", "count"));
        }
    }

Mõne hetke kaudu mõista iga polt tööpõhimõte koodi kommentaare lugeda.

###<a name="define-the-topology"></a>Topoloogia määratlemine

Topoloogia seob selle otsikuid ja poldid kokku üheks graafiku, mis määratleb, kuidas andmevahetus komponendid. See sisaldab ka paralleelsus näpunäiteid, mis torm kasutab komponendid sees klaster eksemplari loomisel.

Järgmine skeem on järgmine põhilised komponendid selle topoloogia graafik.

![diagramm otsikuid ja poldid korraldamine](./media/hdinsight-storm-develop-java-topology/wordcount-topology.png)

Topoloogia kasutusele nimega **WordCountTopology.java** **src\main\java\com\microsoft\example** directory uue faili loomine. Kasutage järgmist faili sisu:

    package com.microsoft.example;

    import backtype.storm.Config;
    import backtype.storm.LocalCluster;
    import backtype.storm.StormSubmitter;
    import backtype.storm.topology.TopologyBuilder;
    import backtype.storm.tuple.Fields;

    import com.microsoft.example.RandomSentenceSpout;

    public class WordCountTopology {

      //Entry point for the topology
      public static void main(String[] args) throws Exception {
      //Used to build the topology
        TopologyBuilder builder = new TopologyBuilder();
        //Add the spout, with a name of 'spout'
        //and parallelism hint of 5 executors
        builder.setSpout("spout", new RandomSentenceSpout(), 5);
        //Add the SplitSentence bolt, with a name of 'split'
        //and parallelism hint of 8 executors
        //shufflegrouping subscribes to the spout, and equally distributes
        //tuples (sentences) across instances of the SplitSentence bolt
        builder.setBolt("split", new SplitSentence(), 8).shuffleGrouping("spout");
        //Add the counter, with a name of 'count'
        //and parallelism hint of 12 executors
        //fieldsgrouping subscribes to the split bolt, and
        //ensures that the same word is sent to the same instance (group by field 'word')
        builder.setBolt("count", new WordCount(), 12).fieldsGrouping("split", new Fields("word"));

        //new configuration
        Config conf = new Config();
        //Set to false to disable debug information
        // when running in production mode.
        conf.setDebug(false);

        //If there are arguments, we are running on a cluster
        if (args != null && args.length > 0) {
          //parallelism hint to set the number of workers
          conf.setNumWorkers(3);
          //submit the topology
          StormSubmitter.submitTopology(args[0], conf, builder.createTopology());
        }
        //Otherwise, we are running locally
        else {
          //Cap the maximum number of executors that can be spawned
          //for a component to 3
          conf.setMaxTaskParallelism(3);
          //LocalCluster is used to run locally
          LocalCluster cluster = new LocalCluster();
          //submit the topology
          cluster.submitTopology("word-count", conf, builder.createTopology());
          //sleep
          Thread.sleep(10000);
          //shut down the cluster
          cluster.shutdown();
        }
      }
    }

Mõne hetke läbi mõistmaks, kuidas topoloogia on määratletud ja seejärel esitatakse klaster koodi kommentaare lugeda.

###<a name="configure-logging"></a>Logisse kandmise konfigureerimine

Storm kasutab Apache Log4j Logiteave. Kui konfigureerite logimine väljastab topoloogia palju diagnostikateave, mis võib olla raske lugeda. Määrata, mis on sisse logitud, looge fail nimega __log4j2.xml__ kataloogis __ressursid__ . Kasutage järgmist faili sisu.

    <?xml version="1.0" encoding="UTF-8"?>
    <Configuration>
    <Appenders>
        <Console name="STDOUT" target="SYSTEM_OUT">
            <PatternLayout pattern="%d{HH:mm:ss} [%t] %-5level %logger{36} - %msg%n"/>
        </Console>
    </Appenders>
    <Loggers>
        <Logger name="com.microsoft.example" level="trace" additivity="false">
            <AppenderRef ref="STDOUT"/>
        </Logger>
        <Root level="error">
            <Appender-Ref ref="STDOUT"/>
        </Root>
    </Loggers>
    </Configuration>

See konfigureerib uue puuraidur __com.microsoft.example__ klassi, mis sisaldab see näide topoloogia komponendid. Taseme on seatud Jälita seda puuraidur, mis kuvatakse kõik selle topoloogias kiiratava logiteabe jäädvustada. Kui vaatate selle projekti koodi kaudu, märkate, et ainult WordCount.java faili rakendab logimine; See arv iga sõna sisse logida.

Funktsiooni `<Root level="error">` jaotis konfigureerib root tase (kõik ei __com.microsoft.example__,) logida sisse logida tõrketeabe.

> [AZURE.IMPORTANT] Kui see vähendab oluliselt sisse logitud, kui teie arenduskeskkond topoloogia testimise teave, ei eemalda silumine teavet toodeti klaster valmistamisel käivitamisel. Vähendamiseks seda teavet, peate seadma, silumine konfiguratsiooni, mis on esitatud klaster FALSE. Vaadake selles dokumendis näiteks WordCountTopology.java kood. 

Logimise Log4j konfigureerimise kohta leiate lisateavet teemast [http://logging.apache.org/log4j/2.x/manual/configuration.html](http://logging.apache.org/log4j/2.x/manual/configuration.html).

> [AZURE.NOTE] Torm versiooni 0.10.0 kasutab Log4j 2.x. Kasutatakse Log4j torm vanemaid versioone 1.x, mida kasutada mõnda muud vormingut log konfigureerimine. Vanema konfigureerimise kohta leiate teavet teemast [http://wiki.apache.org/logging-log4j/Log4jXmlFormat](http://wiki.apache.org/logging-log4j/Log4jXmlFormat).

##<a name="test-the-topology-locally"></a>Testige topoloogia kohalikult

Pärast faili salvestamist kasutada testimiseks topoloogia kohalikult järgmine käsk.

    mvn compile exec:java -Dstorm.topology=com.microsoft.example.WordCountTopology

Kui see töötab, kuvatakse topoloogia käivitus teavet. Seejärel algab laused on nõrgunud tila ja töödelda poldid read, mis sarnaneb järgmise kuvamiseks.

    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word snow
    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word white
    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 112 for word seven
    17:33:27 [Thread-16-count] INFO  com.microsoft.example.WordCount - Emitting a count of 195 for word the
    17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 113 for word and
    17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 57 for word dwarfs
    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 57 for word snow

Kiiratava WordCount polt logimine vaadates me näeme, et "ja" 113 korda emiteerimist. Kui topoloogia töötab, kuna tila pidevalt eraldab lausete tõusevad jätkavad arvu.

Olemas on ka 5 teine intervall sõnade emissiooni ja loendab vahel. Selle põhjuseks on __WordCount__ komponent on konfigureeritud ainult eraldavad teabe korteeži rist saabub, kui seda taotleb, et sellist kordseid toimetatakse ainult vaikimisi iga 5 sekundi järel.

## <a name="convert-the-topology-to-flux"></a>Flux topoloogia teisendamine

Ei uus raamistik torm 0.10.0, mis võimaldab teil eraldi konfigureerimist rakendamisel saadaval. Teie komponendid (poldid ja spouts) on endiselt määratletud Java, kuid topoloogia on määratletud YAML faili abil.

YAML fail määratleb komponentide kasutamiseks topoloogia, kuidas andmevahetus ja mis väärtused kasutada lähtestamisel komponendid. Saate lisada YAML faili käigus juurutamist või topoloogia käivitamisel saate kasutada välise YAML faili, mis sisaldab projekti jar faili.

1. Liikumine läbi projekti __WordCountTopology.java__ fail. Varem see määratletud topoloogia, kuid me ei kasuta seda Flux jaoks.

2. Kataloogis __ressursid__ __topology.yaml__nimega uue faili loomine. Kasutage seda faili sisu järgmist.

        # topology definition

        # name to be used when submitting. This is what shows up...
        # in the Storm UI/storm command-line tool as the topology name
        # when submitted to Storm
        name: "wordcount"

        # Topology configuration
        config:
        # Hint for the number of workers to create
        topology.workers: 1

        # Spout definitions
        spouts:
        - id: "sentence-spout"
            className: "com.microsoft.example.RandomSentenceSpout"
            # parallelism hint
            parallelism: 1

        # Bolt definitions
        bolts:
        - id: "splitter-bolt"
            className: "com.microsoft.example.SplitSentence"
            parallelism: 1

        - id: "counter-bolt"
            className: "com.microsoft.example.WordCount"
            constructorArgs:
            - 10
            parallelism: 1

        # Stream definitions
        streams:
        - name: "Spout --> Splitter" # name isn't used (placeholder for logging, UI, etc.)
            # The stream emitter
            from: "sentence-spout"
            # The stream consumer
            to: "splitter-bolt"
            # Grouping type
            grouping:
            type: SHUFFLE

        - name: "Splitter -> Counter"
            from: "splitter-bolt"
            to: "counter-bolt"
            grouping:
            type: FIELDS
            # field(s) to group on
            args: ["word"]

    Mõne hetke lugeda ja mõista iga jaotise mida ja kuidas see on seotud Java-põhine määratluse faili __WordCountTopology.java__ .

3. __Pom.xml__ faili teha järgmisi muudatusi.

    * Lisage järgmised uue sõltuvus on `<dependencies>` jaotis:

            <!-- Add a dependency on the Flux framework -->
            <dependency>
                <groupId>org.apache.storm</groupId>
                <artifactId>flux-core</artifactId>
                <version>${storm.version}</version>
            </dependency>

    * Järgmised lisandmooduli abil soovitud `<plugins>` jaotis. Selle lisandmooduli tegeleb projekti pakett (jar-faili) loomine ja kehtib mõne kindla teisendused Flux paketi loomisel.

            <!-- build an uber jar -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-shade-plugin</artifactId>
                <version>2.3</version>
                <configuration>
                    <transformers>
                        <!-- Keep us from getting a "can't overwrite file error" -->
                        <transformer implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer" />
                        <transformer implementation="org.apache.maven.plugins.shade.resource.ServicesResourceTransformer" />
                        <!-- We're using Flux, so refer to it as main -->
                        <transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
                            <mainClass>org.apache.storm.flux.Flux</mainClass>
                        </transformer>
                    </transformers>
                    <!-- Keep us from getting a bad signature error -->
                    <filters>
                        <filter>
                            <artifact>*:*</artifact>
                            <excludes>
                                <exclude>META-INF/*.SF</exclude>
                                <exclude>META-INF/*.DSA</exclude>
                                <exclude>META-INF/*.RSA</exclude>
                            </excludes>
                        </filter>
                    </filters>
                </configuration>
                <executions>
                    <execution>
                        <phase>package</phase>
                        <goals>
                            <goal>shade</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>

    * __Exec-maven-lisandmoodul__ `<configuration>` jaotise, muutke väärtus `<mainClass>` et `org.apache.storm.flux.Flux`. See võimaldab Flux käsitlema, käivitage see kohalikult arendatakse topoloogia käivitamise.

    * Klõpsake soovitud `<resources>` jaotises lisatakse järgmised on `<includes>`. See hõlmab YAML fail, mis määratleb topoloogia projekti osana.
    
            <include>topology.yaml</include>

## <a name="test-the-flux-topology-locally"></a>Kohalik flux topoloogia testimine

1. Järgmine abil saate koostada ja käivitada Flux topoloogia Maven abil.

        mvn compile exec:java -Dexec.args="--local -R /topology.yaml"
    
    Kui kasutate PowerShelli, kasutage järgmist:
    
        mvn compile exec:java "-Dexec.args=--local -R /topology.yaml"

    Kui on Linux-Unixi-OS X süsteemi ja on [installitud torm oma arenduskeskkond](http://storm.apache.org/releases/0.10.0/Setting-up-development-environment.html), saate järgmised käsud:

        mvn compile package
        storm jar target/WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local -R /topology.yaml

    Funktsiooni `--local` parameetri käivitatakse topoloogia kohalikus režiimis oma arenduskeskkond. Funktsiooni `-R /topology.yaml` parameetrit kasutab funktsiooni `topology.yaml` faili ressursside jar faili määratleda topoloogia.

    Kui see töötab, kuvatakse topoloogia käivitus teavet. Seejärel algab laused on nõrgunud tila ja töödelda poldid read, mis sarnaneb järgmise kuvamiseks.

        17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word snow
        17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word white
        17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 112 for word seven
        17:33:27 [Thread-16-count] INFO  com.microsoft.example.WordCount - Emitting a count of 195 for word the
        17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 113 for word and
        17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 57 for word dwarfs
    
    On 10 sekundilist viivitust vahel hulga andmeid, mis on `topology.yaml` faili edastab väärtus `10` WordCount komponent loomise. Selle sätte sekundilist viivitust intervalli rist korteeži.

2.  Tehke koopia on `topology.yaml` projekti faili. Kõne umbes `newtopology.yaml`. Otsige faili, järgmine jaotise ja väärtust `10` et `5`. See muudab intervalli kiirgavad hulga sõnaarvestusega 10 sekundi-5.

          - id: "counter-bolt"
            className: "com.microsoft.example.WordCount"
            constructorArgs:
            - 5
            parallelism: 1

3. Topoloogia käivitamiseks kasutage järgmine käsk:

        mvn exec:java -Dexec.args="--local /path/to/newtopology.yaml"

    Või kui teil on oma Linux/Unix/OS X arenduskeskkond torm:

        storm jar target/WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local /path/to/newtopology.yaml

    Muuda selle `/path/to/newtopology.yaml` eelmises etapis loodud newtopology.yaml faili tee. See käsk kasutab funktsiooni newtopology.yaml topoloogia määratlus. Kuna me ei sisalda soovitud `compile` parameeter, Maven taaskasutamine eelmiste juhiste järgi loodud projekti versiooni.

    Pärast topoloogia käivitub, tuleks märkate, et kajastamiseks newtopology.yaml väärtus on muutunud vahelise tekitatava pakettidena aja. Nii, et te näete, et saate muuta oma konfiguratsiooni YAML faili kaudu ilma Kompileeri topoloogia.

On mitu muud funktsioonid, et Flux pakub, mis ei käsitleta siin, näiteks muutuv asendus parameetrite põhjal YAML fail on möödas käitusajal või keskkonna muutujate. Nende ja muude funktsioonide Flux raamistiku kohta leiate lisateavet teemast [Flux (https://storm.apache.org/releases/0.10.0/flux.html)](https://storm.apache.org/releases/0.10.0/flux.html).

##<a name="trident"></a>Trident

Trident on üksikasjalik võtmiseks, mis on esitatud tormi. See toetab stateful töötlemine. Trident esmane eeliseid on see iga sõnumi, mis sisestab topoloogia on ainult üks kord töödeldud tagada. See on raske saavutada töötlemata Java topoloogia, millega tagatakse isiku sõnumite töödeldakse vähemalt ühe korra. On ka järgmisi erinevusi, nt sisseehitatud komponendid, mida saab kasutada poldid loomise asemel. Tegelikult asendatakse poldid täielikult vähem-üldine komponendid, nt filtrid, prognooside ja funktsioonid.

Trident rakenduste loomist Maven projektide abil. Selles artiklis esitatud kasutate samu juhiseid – ainult kood on erinev. Trident ei saa (praegu) kasutada ka koos Flux raames.

Trident kohta leiate lisateavet teemast <a href="http://storm.apache.org/documentation/Trident-API-Overview.html" target="_blank">Trident API ülevaade</a>.

Näide Trident rakendus, leiate teemast [Twitter trendid teemasid Apache torm Hdinsightiga kohta](hdinsight-storm-twitter-trending.md).

##<a name="next-steps"></a>Järgmised sammud

Te teada, kuidas luua Storm topoloogia Java abil. Nüüd saate teada, kuidas:

* [Juurutada ja hallata Apache Storm topoloogiatest HDInsight kohta](hdinsight-storm-deploy-monitor-topology.md)

* [C# topoloogiatest arendamise Apache Storm klõpsake Visual Studio abil Hdinsightiga](hdinsight-storm-develop-csharp-visual-studio-topology.md)

Võite leida rohkem näide Storm topoloogiatest külastades [näide topoloogiatest Storm Hdinsightiga kohta](hdinsight-storm-example-topology.md).
