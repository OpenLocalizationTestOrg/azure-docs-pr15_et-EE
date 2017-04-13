<properties
   pageTitle="Twitter trendid teemasid Apache torm kohta Hdinsightiga | Microsoft Azure'i"
   description="Saate teada, kuidas luua Apache Storm topoloogia, mis määratleb trendid teemasid Twitter põhjal hashtags Trident abil."
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
   ms.date="09/27/2016"
   ms.author="larryfr"/>

#<a name="determine-twitter-trending-topics-with-apache-storm-on-hdinsight"></a>Määratleda Twitter trendid teemasid Apache torm Hdinsightiga kohta

Saate teada, kuidas luua Storm topoloogia, mis määratleb trendid teemasid (räsi sildid) Twitter Trident abil.

Trident on üksikasjalik võtmiseks, mis annab teie käsutusse tööriistad, nt ühendused, liitmised, rühmitamise, funktsioonid ja filtreid. Lisaks lisab Trident primitiivid tehes stateful, suureneva töötlemine. Selles näites näitab, kuidas saate luua kohandatud tila, funktsioon ja mitme sisseehitatud funktsioonid, mis on esitatud Trident topoloogia.

> [AZURE.NOTE] Selles näites on tugevalt alusel [Trident Storm](https://github.com/jalonsoramos/trident-storm) näide Juan Alonso.

##<a name="requirements"></a>Nõuded

* <a href="http://www.oracle.com/technetwork/java/javase/downloads/index.html" target="_blank">Java ja JDK 1.7</a>

* <a href="http://maven.apache.org/what-is-maven.html" target="_blank">Maven</a>

* <a href="http://git-scm.com/" target="_blank">Git</a>

* Twitter arendaja konto

##<a name="download-the-project"></a>Projekti allalaadimine

Kasutage järgmine kood klooni projekti kohalikult.

    git clone https://github.com/Blackmist/TwitterTrending

##<a name="topology"></a>Topoloogia

Selle näite puhul topoloogia on järgmine:

![topoloogia](./media/hdinsight-storm-twitter-trending/trident.png)

> [AZURE.NOTE] See on topoloogia lihtsustatud vaade. Mitmes eksemplaris komponendid jagatakse klaster sõlmed üle.

Rakendab topoloogia Trident koodis on järgmine:

    topology.newStream("spout", spout)
        .each(new Fields("tweet"), new HashtagExtractor(), new Fields("hashtag"))
        .groupBy(new Fields("hashtag"))
        .persistentAggregate(new MemoryMapState.Factory(), new Count(), new Fields("count"))
        .newValuesStream()
        .applyAssembly(new FirstN(10, "count"))
        .each(new Fields("hashtag", "count"), new Debug());

Järgmine kood teeb järgmist.

1. Loob uue voo tila. Tila toob tweets Twitter ja filtreerib need teatud märksõnade (armastust, muusikat ja kohvi selles näites).

2. HashtagExtractor, kohandatud funktsiooni kasutatakse iga säutsu räsi siltide eraldamiseks. Need eraldub voogu.

3. Voog on räsi sildi alusel rühmitatud ja lugeja edasi. See lugeja loob arv räsi igal sildil on ilmnenud mitu korda. Andmed on säilis mälu. Lõpuks uue voo on tekitatava sisaldava räsi tag ja arvu.

4. Kuna oleme huvitatud kõige populaarsemate räsi siltide antud paketi tweets jaoks, rakendatakse ainult ülemine 10 väärtuste tagastamiseks, põhineb väli count **FirstN** komplekti.

> [AZURE.NOTE] Peale selle tila ja HashtagExtractor kasutame Trident sisseehitatud funktsioonid.
>
> Sisseehitatud toimingute kohta leiate teavet teemast <a href="https://storm.apache.org/apidocs/storm/trident/operation/builtin/package-summary.html" target="_blank">paketi storm.trident.operation.builtin</a>.
>
> Trident olekuga rakendusi peale MemoryMapState, vaadake järgmist:
>
> * <a href="https://github.com/fhussonnois/storm-trident-elasticsearch" target="_blank">Torm Trident elastne otsing</a>
>
> * <a href="https://github.com/kstyrc/trident-redis" target="_blank">Trident-redis</a>

###<a name="the-spout"></a>Tila

Tila, **TwitterSpout**, kasutab <a href="http://twitter4j.org/en/" target="_blank">Twitter4j</a> Twitteri tweets toomiseks. Filter on loodud (armastust, muusikat ja kohvi selles näites) ja filtrile sissetulevate tweets (olek) on talletatud lingitud blokeerimise järjekorda. (Lisateavet leiate teemast <a href="http://docs.oracle.com/javase/7/docs/api/java/util/concurrent/LinkedBlockingQueue.html" target="_blank">Klassi LinkedBlockingQueue</a>.) Lõpuks üksuste tõmmatakse välja kuhjuda ja toote koostisse topoloogia.

###<a name="the-hashtagextractor"></a>Funktsiooni HashtagExtractor

Eraldamiseks räsi sildid, kasutatakse <a href="http://twitter4j.org/javadoc/twitter4j/EntitySupport.html#getHashtagEntities--" target="_blank">getHashtagEntities</a> räsi kõik sildid, mis sisalduvad selle säutsu toomiseks. Need siis eraldub voo.

##<a name="enable-twitter"></a>Twitteri lubamine

Uus Twitter rakendus registreerida ja vajaliku lugemise Twitter tarbija ja juurdepääs Turbeloa teabe saamiseks tehke järgmist:

1. Minge <a href="https://apps.twitter.com" target="_blank">Twitteri rakendused</a> ja klõpsake nuppu **Loo uus rakendus** . Kui vormi täitmise **Tagasihelistamise URL-i** välja tühjaks jätta.

2. Kui rakendus on loodud, klõpsake vahekaarti **Kiirklahvid ja juurdepääs märgid** .

3. Teabe **Tarbija võti** ja **Tarbija salajane** kopeerida.

4. Valige lehe allosas **Loo minu juurdepääsu luba** , kui sõned pole olemas. Kui märkide on loodud, kopeerige **Accessi Turbeloa** ja **Accessi Turbeloa salajane** teave.

5. Teil on varem kloonitud **TwitterSpoutTopology** projekti **resources/twitter4j.properties** faili avada, eelmiste juhiste järgi saate kogutud teabe lisamine ja seejärel salvestage fail.

##<a name="build-the-topology"></a>Topoloogia koostamine

Projekti koostamiseks kasutada järgmine kood:

        cd [directoryname]
        mvn compile

##<a name="test-the-topology"></a>Testige topoloogia

Topoloogia kohalikult testimiseks kasutada järgmine käsk:

    mvn compile exec:java -Dstorm.topology=com.microsoft.example.TwitterTrendingTopology

Pärast topoloogia käivitub, peaksite nägema silumine teavet, mis sisaldab räsi sildid ja loendab tekitatava topoloogia järgi. Väljund peaks kuvatama umbes järgmine:

    DEBUG: [Quicktellervalentine, 7]
    DEBUG: [GRAMMYs, 7]
    DEBUG: [AskSam, 7]
    DEBUG: [poppunk, 1]
    DEBUG: [rock, 1]
    DEBUG: [punkrock, 1]
    DEBUG: [band, 1]
    DEBUG: [punk, 1]
    DEBUG: [indonesiapunkrock, 1]

##<a name="next-steps"></a>Järgmised sammud

Nüüd, kui olete testinud topoloogia kohalikult, vaadake juurutamise topoloogia: [Deploy ja hallata Apache Storm topoloogiatest kohta Hdinsightiga](hdinsight-storm-deploy-monitor-topology.md).

Teil võib olla huvitatud Storm järgmisi teemasid:

* [Java topoloogiatest arendamise Storm Hdinsightiga Maven kasutamise kohta](hdinsight-storm-develop-java-topology.md)

* [C# topoloogiatest arendamise Storm klõpsake Visual Studio abil Hdinsightiga](hdinsight-storm-develop-csharp-visual-studio-topology.md)

Rohkem Storm näiteid Hdinsightiga jaoks:

* [Näide topoloogiatest Storm Hdinsightiga kohta](hdinsight-storm-example-topology.md)
