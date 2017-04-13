## <a name="receive-messages-with-apache-storm"></a>Meilisõnumite Apache torm

[**Apache Storm**](https://storm.incubator.apache.org) on jaotatud reaalajas arvutus süsteemi, mis lihtsustab usaldusväärne ääretu voole andmete töötlemise. Selles jaotises kirjeldatakse lepingute abil soovitud sündmus jaoturi Storm tila sündmuste saadud sündmuse jaoturi. Apache Storm kasutamisel saate tükeldada sündmuste üle mitme eri sõlmed majutatud protsessid. Sündmuse jaoturi integreerimine Storm lihtsustab sündmuse tarbimine läbipaistev checkpointing edenemisega kasutades Sunda Zookeeper installi, püsivate postkastid haldamine ja samal ajal saab sündmuse jaoturi kaudu.

Lisateavet sündmuse jaoturi mustrite vastu võtta, [sündmuse jaoturi ülevaade][].

Selle õpetuse kasutab [Hdinsightiga torm][] installi, mis on kaasas sündmuse jaoturi tila juba saadaval.

1. Saate luua uue Hdinsightiga kobar ja kaudu kaugtöölaua ühenduse loomiseks toimige [Hdinsightiga Storm - alustamine](../articles/hdinsight/hdinsight-storm-overview.md) .

2. Kopeerige soovitud `%STORM_HOME%\examples\eventhubspout\eventhubs-storm-spout-0.9-jar-with-dependencies.jar` faili teie kohaliku arenduskeskkond. See sisaldab sündmuste torm tila.

3. Järgmise käsu abil saate installida paketi Maven kohalikku salve. See võimaldab teil selle abimaterjalina Storm projektis hiljem lisada.

        mvn install:install-file -Dfile=target\eventhubs-storm-spout-0.9-jar-with-dependencies.jar -DgroupId=com.microsoft.eventhubs -DartifactId=eventhubs-storm-spout -Dversion=0.9 -Dpackaging=jar

4. Eclipse, luua uue projekti Maven (klõpsake **faili**, siis **Uus**, siis **projekti**).

    ![][12]

5. Valige **Kasuta vaikeasukoha tööruumi**ja klõpsake nuppu **edasi**

6. Valige **maven-arhetüüp-Kiirjuhend** arhetüüp ja seejärel klõpsake nuppu **Järgmine**

7. Lisage **GroupId** ja **ArtifactId**ja seejärel klõpsake nuppu **valmis**

8. **Pom.xml**, lisage järgmised sõltuvused on `<dependency>` sõlm.

        <dependency>
            <groupId>org.apache.storm</groupId>
            <artifactId>storm-core</artifactId>
            <version>0.9.2-incubating</version>
            <scope>provided</scope>
        </dependency>
        <dependency>
            <groupId>com.microsoft.eventhubs</groupId>
            <artifactId>eventhubs-storm-spout</artifactId>
            <version>0.9</version>
        </dependency>
        <dependency>
            <groupId>com.netflix.curator</groupId>
            <artifactId>curator-framework</artifactId>
            <version>1.3.3</version>
            <exclusions>
                <exclusion>
                    <groupId>log4j</groupId>
                    <artifactId>log4j</artifactId>
                </exclusion>
                <exclusion>
                    <groupId>org.slf4j</groupId>
                    <artifactId>slf4j-log4j12</artifactId>
                </exclusion>
            </exclusions>
            <scope>provided</scope>
        </dependency>

9. **Src** kausta, looge fail nimega **Config.properties** ja kopeerige järgmine sisu, asendades järgmised väärtused:

        eventhubspout.username = ReceiveRule

        eventhubspout.password = {receive rule key}

        eventhubspout.namespace = ioteventhub-ns

        eventhubspout.entitypath = {event hub name}

        eventhubspout.partitions.count = 16

        # if not provided, will use storm's zookeeper settings
        # zookeeper.connectionstring=localhost:2181

        eventhubspout.checkpoint.interval = 10

        eventhub.receiver.credits = 10

    **Eventhub.receiver.credits** väärtus määratleb mitmel võistlusalal on batched enne nende Storm müügivõimaluste avaldamist. Selles näites määrab huvides lihtne, seda väärtust 10. Valmistamisel, see tavaliselt seadma suuremad väärtused; näiteks 1024.

10. Saate luua uue klassi nimega **LoggerBolt** järgmine kood:

        import java.util.Map;
        import org.slf4j.Logger;
        import org.slf4j.LoggerFactory;
        import backtype.storm.task.OutputCollector;
        import backtype.storm.task.TopologyContext;
        import backtype.storm.topology.OutputFieldsDeclarer;
        import backtype.storm.topology.base.BaseRichBolt;
        import backtype.storm.tuple.Tuple;

        public class LoggerBolt extends BaseRichBolt {
            private OutputCollector collector;
            private static final Logger logger = LoggerFactory
                      .getLogger(LoggerBolt.class);

            @Override
            public void execute(Tuple tuple) {
                String value = tuple.getString(0);
                logger.info("Tuple value: " + value);

                collector.ack(tuple);
            }

            @Override
            public void prepare(Map map, TopologyContext context, OutputCollector collector) {
                this.collector = collector;
                this.count = 0;
            }

            @Override
            public void declareOutputFields(OutputFieldsDeclarer declarer) {
                // no output fields
            }

        }

    See torm polt logib sündmused received sisu. See saab hõlpsasti laiendada kordseid talletada salvestusruum teenuses. [Hdinsightiga andur analüüsi õpetuse] kasutab sama viisi andmete talletamiseks HBase sisse.

11. Klassi nimetatakse **LogTopology** järgmine kood loomiseks tehke järgmist.

        import java.io.FileReader;
        import java.util.Properties;
        import backtype.storm.Config;
        import backtype.storm.LocalCluster;
        import backtype.storm.StormSubmitter;
        import backtype.storm.generated.StormTopology;
        import backtype.storm.topology.TopologyBuilder;
        import com.microsoft.eventhubs.samples.EventCount;
        import com.microsoft.eventhubs.spout.EventHubSpout;
        import com.microsoft.eventhubs.spout.EventHubSpoutConfig;

        public class LogTopology {
            protected EventHubSpoutConfig spoutConfig;
            protected int numWorkers;

            protected void readEHConfig(String[] args) throws Exception {
                Properties properties = new Properties();
                if (args.length > 1) {
                    properties.load(new FileReader(args[1]));
                } else {
                    properties.load(EventCount.class.getClassLoader()
                            .getResourceAsStream("Config.properties"));
                }

                String username = properties.getProperty("eventhubspout.username");
                String password = properties.getProperty("eventhubspout.password");
                String namespaceName = properties
                        .getProperty("eventhubspout.namespace");
                String entityPath = properties.getProperty("eventhubspout.entitypath");
                String zkEndpointAddress = properties
                        .getProperty("zookeeper.connectionstring"); // opt
                int partitionCount = Integer.parseInt(properties
                        .getProperty("eventhubspout.partitions.count"));
                int checkpointIntervalInSeconds = Integer.parseInt(properties
                        .getProperty("eventhubspout.checkpoint.interval"));
                int receiverCredits = Integer.parseInt(properties
                        .getProperty("eventhub.receiver.credits")); // prefetch count
                                                                    // (opt)
                System.out.println("Eventhub spout config: ");
                System.out.println("  partition count: " + partitionCount);
                System.out.println("  checkpoint interval: "
                        + checkpointIntervalInSeconds);
                System.out.println("  receiver credits: " + receiverCredits);

                spoutConfig = new EventHubSpoutConfig(username, password,
                        namespaceName, entityPath, partitionCount, zkEndpointAddress,
                        checkpointIntervalInSeconds, receiverCredits);

                // set the number of workers to be the same as partition number.
                // the idea is to have a spout and a logger bolt co-exist in one
                // worker to avoid shuffling messages across workers in storm cluster.
                numWorkers = spoutConfig.getPartitionCount();

                if (args.length > 0) {
                    // set topology name so that sample Trident topology can use it as
                    // stream name.
                    spoutConfig.setTopologyName(args[0]);
                }
            }

            protected StormTopology buildTopology() {
                TopologyBuilder topologyBuilder = new TopologyBuilder();

                EventHubSpout eventHubSpout = new EventHubSpout(spoutConfig);
                topologyBuilder.setSpout("EventHubsSpout", eventHubSpout,
                        spoutConfig.getPartitionCount()).setNumTasks(
                        spoutConfig.getPartitionCount());
                topologyBuilder
                        .setBolt("LoggerBolt", new LoggerBolt(),
                                spoutConfig.getPartitionCount())
                        .localOrShuffleGrouping("EventHubsSpout")
                        .setNumTasks(spoutConfig.getPartitionCount());
                return topologyBuilder.createTopology();
            }

            protected void runScenario(String[] args) throws Exception {
                boolean runLocal = true;
                readEHConfig(args);
                StormTopology topology = buildTopology();
                Config config = new Config();
                config.setDebug(false);

                if (runLocal) {
                    config.setMaxTaskParallelism(2);
                    LocalCluster localCluster = new LocalCluster();
                    localCluster.submitTopology("test", config, topology);
                    Thread.sleep(5000000);
                    localCluster.shutdown();
                } else {
                    config.setNumWorkers(numWorkers);
                    StormSubmitter.submitTopology(args[0], config, topology);
                }
            }

            public static void main(String[] args) throws Exception {
                LogTopology topology = new LogTopology();
                topology.runScenario(args);
            }
        }


    See tund loob uue sündmuse jaoturi tila, kasutades atribuutide konfiguratsiooni fail instatiate see. See on oluline, et selles näites loob nii palju otsikuid tööülesannete sektsioonid sündmuse keskuses arvuna selleks, et kasutada suurima paralleelsus, sündmuse jaoturi lubatud.

<!-- Links -->
[Sündmuse jaoturi ülevaade]: ../articles/event-hubs/event-hubs-overview.md
[Hdinsightiga torm]: ../articles/hdinsight/hdinsight-storm-overview.md
[Hdinsightiga andur analüüsi õpetus]: ../articles/hdinsight/hdinsight-storm-sensor-data-analysis.md

<!-- Images -->

[12]: ./media/service-bus-event-hubs-get-started-receive-storm/create-storm1.png