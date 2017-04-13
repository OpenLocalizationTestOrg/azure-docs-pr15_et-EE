<properties
 pageTitle="Näide Apache Storm topoloogiatest kohta Hdinsightiga | Microsoft Azure'i"
 description="Loetelu näide Storm topoloogiatest ja katsetada Apache Storm Hdinsightiga, sh põhilised C# ja Java topoloogiatest ja sündmuse jaoturi töötamise kohta."
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

# <a name="example-storm-toplogies-and-components-for-apache-storm-on-hdinsight"></a>Näide Storm toplogies ja komponentide Apache Storm Hdinsightiga kohta

Järgnev on loodud ja haldab Microsoft kasutamiseks klõpsake Hdinsightiga Apache torm näited. Nendes näidetes katta erinevaid teemasid, luua lihtsa C# ja Java topoloogiatest töötamise Azure teenuseid, näiteks sündmuse jaoturi DocumentDB, Power BI, SQL-andmebaasi, HBase Hdinsightiga ja Azure Storage. Mõned näited demonstreerivad seda ka-Azure'i või isegi Microsoft tehnoloogiaid, nagu SignalR ja Socket.IO töötamine

| Kirjeldus                                                                                             | Näitab                                         | Keele/raames         |
|:--------------------------------------------------------------------------------------------------------|:-----------------------------------------------------|:---------------------------|
| [Azure'i andmesalve Lake kirjutamise Apache torm](hdinsight-storm-write-data-lake-store.md) | Azure'i andmesalve Lake kirjutamine | Java |
| [Sündmuse jaoturi tila ja poldi allikas](https://github.com/apache/storm/tree/master/external/storm-eventhubs) | Sündmuse jaoturi tila ja polt allikas | Java |
| [Java-põhine topoloogiatest arendamise Apache Storm Hdinsightiga kohta][5797064f]                                 | Maven                                                | Java                       |
| [C# topoloogiatest arendamise Apache Storm klõpsake Visual Studio abil Hdinsightiga][16fce2d1]                     | Hdinsightiga Tools for Visual Studio                    | C#, Java                   |
| [Mitme andmevoogu C# Storm topoloogia loomine][ec5a4064]                                         | Mitme voogu                                     | C#                         |
| [Määratleda Twitter trendid teemasid torm Hdinsightiga kohta][3c86c7c8]                                   | Trident                                              | Java, Trident              |
| [Protsessi sündmuste Azure'i sündmuse jaoturi torm klõpsake Hdinsightiga (C#)][844d1d81]                                | Sündmuse jaoturi                                           | C# ja Java                |
| [Protsessi sündmuste Azure'i sündmuse jaoturi torm klõpsake Hdinsightiga (Java)](hdinsight-storm-develop-java-event-hub-topology.md) | Sündmuse jaoturi | Java |
| [Torm topoloogia andmete visualiseerimine Power Bi abil][94d15238]                              | Power BI                                             | C#                         |
| [Torm ja HBase sisse Hdinsightiga andur andmete analüüsimine][ab894747]                                     | Sündmuse jaoturi HBase, Socket.IO, Web armatuurlaud          | C#, Java, JavaScript, HTML-vormingus |
| [Protsess sõiduki anduri andmeid sündmuse jaoturi Hdinsightiga torm kasutamine][246ee964]                        | Sündmuse jaoturi DocumentDb, salvestusruumi Azure'i bloobimälu (WASB)    | C#, Java                   |
| [Ekstrakti, transformatsioon ja laadi (ETL): Azure'i sündmuse jaoturi abil HBase, Hdinsightiga torm kasutamine][b4b68194] | Sündmuse jaoturi HBase                                    | C#                         |
| [Vormimalli C# Storm topoloogia projekti töötamiseks Azure teenused Storm Hdinsightiga kohta][ce0c02a2]  | Sündmuse jaoturi DocumentDb, SQL-i andmebaasi HBase, SignalR | C#, Java                   |
| [Skaleeritavus kriteeriumid Azure'i sündmuse jaoturi abil Storm Hdinsightile lugemine][d6c540e3]           | Sõnumi läbilaskevõime, sündmuse jaoturi, SQL-andmebaas         | C#, Java                   |
| [Seostada sündmuste Hdinsightiga torm ja HBase kasutamine](hdinsight-storm-correlation-topology.md) | HBase | C# |
| [Kasutage Python torm Hdinsightiga kohta](hdinsight-storm-develop-python-topology.md) | Java ja Clojure Python komponendid vastavalt Storm topoloogiatest | Python |

## <a name="next-steps"></a>Järgmised sammud

* [Alustamine Apache Storm Hdinsightiga kohta][2b8c3488]

* [Saate teada, kuidas juurutada ja hallata Storm topoloogiatest torm Hdinsightiga kohta][6eb0d3b8]

  [2b8c3488]: hdinsight-apache-storm-tutorial-get-started-linux.md "Saate teada, kuidas luua torm Hdinsightiga kobar ja Storm armatuurlaua juurutamine näide topoloogiatest abil."
  [6eb0d3b8]: hdinsight-storm-deploy-monitor-topology.md "Saate teada, kuidas juurutada ja hallata topoloogiatest Visual Studio veebipõhise Storm armatuurlaua ja Storm UI või Hdinsightiga tööriistade abil."
  [16fce2d1]: hdinsight-storm-develop-csharp-visual-studio-topology.md "Saate teada, kuidas luua C# Storm topoloogiatest Visual Studio Hdinsightiga tööriistade abil."
  [5797064f]: hdinsight-storm-develop-java-topology.md "Saate teada, kuidas luua Storm topoloogiatest Java, kasutades Maven, luues lihtsa wordcount topoloogia."
  [94d15238]: hdinsight-storm-power-bi-topology.md "Näitab, kuidas kirjutamise andmete Power BI C# topoloogia ja seejärel luua diagrammi ja Armatuurlaua andmed."
  [ec5a4064]: https://github.com/Blackmist/csharp-storm-example "Näitab lihtsa Storm topoloogia, mis sooritavad wordcount, C# rakendada. See näitab ka mitu andmevoogu sees C# topoloogia loomise kohta."
  [844d1d81]: hdinsight-storm-develop-csharp-event-hub-topology.md "Saate teada, kuidas andmete lugemine ja kirjutamine: Azure'i sündmuse jaoturi torm Hdinsightiga kohta."
  [ab894747]: hdinsight-storm-sensor-data-analysis.md "Saate teada, kuidas kasutada Apache Storm Hdinsightiga töötlemine Azure'i sündmuse jaoturi anduri andmeid, kasutades D3.js visualiseerimine ja salvestada selle (soovi korral) HBase."
  [3c86c7c8]: hdinsight-storm-twitter-trending.md "Saate teada, kuidas luua Storm topoloogia, mis määratleb trendid teemasid (vastavalt hashtags,) Trident abil Twitter."
  [246ee964]: hdinsight-storm-iot-eventhub-documentdb.md "Saate teada, kuidas kasutada Storm topoloogia lugeda sõnumeid Azure'i sündmuse jaoturi, milles andmed: Azure'i DocumentDB dokumentide lugemine ja Azure Storage andmete salvestamine."
  [d6c540e3]: https://github.com/hdinsight/hdinsight-storm-examples/blob/master/EventCountExample "Mitme topoloogiatest läbilaskevõime näidata, kui Azure'i sündmuse jaoturi kaudu lugemine ja SQL-andmebaasiga, kasutades Apache Storm Hdinsightile salvestamist."
  [b4b68194]: https://github.com/hdinsight/hdinsight-storm-examples/blob/master/RealTimeETLExample "Saate teada, kuidas Azure'i sündmuse jaoturi liitväärtuse andmeid lugeda ja muuta andmete ja seejärel salvestada selle HBase Hdinsightiga kohta."
  [ce0c02a2]: https://github.com/hdinsight/hdinsight-storm-examples/tree/master/templates/HDInsightStormExamples "See projekt sisaldab malle otsikuid, poldid ja topoloogiatest näiteks sündmuse jaoturi, DocumentDB ja SQL-andmebaasi Azure mitmesuguste suhelda."
 
