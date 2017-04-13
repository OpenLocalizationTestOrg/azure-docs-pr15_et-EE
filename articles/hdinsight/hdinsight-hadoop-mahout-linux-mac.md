<properties
    pageTitle="Soovitused, kasutades Mahout ja Linux-põhine Hdinsighti luua | Microsoft Azure'i"
    description="Saate teada, kuidas Apache Mahout masina Õppekeskuse teegi abil saate luua filmi soovitused koos Linux-põhine Hdinsightiga (Hadoopi)."
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
    ms.date="08/30/2016"
    ms.author="larryfr"/>

#<a name="generate-movie-recommendations-by-using-apache-mahout-with-linux-based-hadoop-in-hdinsight"></a>Luua filmi soovitused kasutades Apache Mahout Linux-põhine Hadoopi sisse Hdinsightiga

[AZURE.INCLUDE [mahout-selector](../../includes/hdinsight-selector-mahout.md)]

Saate teada, kuidas õppimine raamatukogu koos Windows Azure Hdinsightiga [Apache Mahout](http://mahout.apache.org) seadme abil saate luua filmi soovitused.

Mahout on [masina Õppekeskuse] [ ml] Apache Hadoop teek. Mahout sisaldab algoritmide kohta andmete filtreerimine, liigitamine, nt rühmitamise töötlemiseks. Selles artiklis on soovitus Mootor abil luua filmi soovitused, mis põhinevad Filmid sõpradele näinud.

> [AZURE.NOTE] Juhised selle dokumendi jaoks on vaja Linuxi-põhiste Hadoopi Hdinsightiga kobar. Windowsi-põhiste kobar Mahout kasutamise kohta leiate teemast [Genereeri filmi soovitused, kasutades Apache Mahout koos Windowsi-põhiste Hadoopi Hdinsightiga sisse](hdinsight-mahout.md)

##<a name="prerequisites"></a>Eeltingimused

* Mõne Linuxi-põhiste Hadoopi Hdinsightiga kobar. Üks loomise kohta leiate teemast [Linux-põhine Hadoopi rakenduses Hdinsightiga kasutamise alustamine][getstarted]

##<a name="mahout-versioning"></a>Mahout Versioonimine

Kaasas Hdinsightiga klaster Mahout versiooni kohta lisateabe saamiseks vt [Hdinsightiga versioonid ja Hadoopi komponendid](hdinsight-component-versioning.md).

> [AZURE.WARNING] See on võimalik üles laadida muu versioon, Mahout Hdinsightiga kobar, ainult koos Hdinsightiga kobar komponendid on täielikult toetatud ja Microsoft Support aitab eristada ja nende komponentide seotud probleemide lahendamiseks.
>
> Kohandatud komponendid tugiteenuseid äriliselt mõistlik aidata teil selle probleemi tõrkeotsingu sooritamiseks. See võib põhjustada probleemi lahendamine või palub teil otsimist ja avatud allika tehnoloogiaid, kui leitakse sügav teadmised selle tehnoloogia. Näiteks on palju kogukonnafoorumi saite, mida saab kasutada, nt: [MSDN-i Foorum Hdinsightiga](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Ka Apache projektide on projekti saitidel [http://apache.org](http://apache.org), näiteks: [Hadoopi](http://hadoop.apache.org/), [säde](http://spark.apache.org/).

##<a name="recommendations"></a>Soovitused mõistmine

Üks funktsioonid, mis on esitatud Mahout on soovitus mootor. See mootor aktsepteerib andmete vorming `userID`, `itemId`, ja `prefValue` (kasutajad eelistus üksus). Mahout saate teha koostööd sündmust analüüsi määratlemiseks: _kasutajad, kes on üksuse eelistust on ka muud nendeks eelistust_. Mahout seejärel määrab kasutajatele meeldib üksusega eelistused, mida saate kasutada soovitusi.

Järgnev on väga lihtne näide, mis kasutab filmid:

* __Kaasautorluse sündmust__: Joe, Alice ja Bob kõik meeldinud _tähesõdade_, _Empire Strikes Back_ja _tagastada Jedi_. Mahout määratleb, et kasutajad, kes meeldib mõni järgmised Filmid ka nagu teised kaks.

* __Kaasautorluse sündmust__: Bob ja Alice meeldinud _Phantom Menace_, _rünnaku kloonide_ja _kätte maksta Sith_. Mahout määratleb, et kasutajad, kes meeldis eelmise kolm filmi samuti nagu need kolm.

* __Väga sarnased soovitus__: Kuna Joe meeldinud esimesed kolm Filmid Mahout suunatud filmid, et teised sarnased eelistused meeldinud, kuid Joe ei leidnud (meeldinud/hinnatud). Sel juhul soovitab Mahout _Phantom Menace_, _rünnaku kloonide_ja _kätte maksta Sith_.

###<a name="understanding-the-data"></a>Andmete mõistmine

Mugavalt, [GroupLens uurimistöö] [ movielens] annab Filmid vormingus, mis ühildub Mahout reiting andmeid. Andmed on saadaval teie kobar vaikimisi salvestusruum `/HdiSamples/HdiSamples/MahoutMovieData`.

On kaks faili, `moviedb.txt` (filmid, teavet) ja `user-ratings.txt`. Kasutaja-ratings.txt faili kasutatakse analüüsimisel, samal ajal moviedb.txt kasutatakse kasutajasõbralik teksti juubelipidustuste analüüsi tulemuste kuvamisel.

Kasutaja-ratings.txt sisalduvate andmete on struktuur `userID`, `movieID`, `userRating`, ja `timestamp`, mis ütleb, kuidas iga kasutaja saanud filmi. Siin on näide andmeid:


    196 242 3   881250949
    186 302 3   891717742
    22  377 1   878887116
    244 51  2   880606923
    166 346 1   886397596

##<a name="run-the-analysis"></a>Analüüsi käivitamine

Järgmise käsu abil saate käivitada soovitus töö:

    mahout recommenditembased -s SIMILARITY_COOCCURRENCE -i /HdiSamples/HdiSamples/MahoutMovieData/user-ratings.txt -o /example/data/mahoutout --tempDir /temp/mahouttemp

> [AZURE.NOTE] Töö võib kuluda mitu minutit, ning mitme MapReduce töö.

##<a name="view-the-output"></a>Vaate väljund

1. Pärast töö lõpulejõudmist järgmise käsu abil saate vaadata loodud väljund:

        hdfs dfs -text /example/data/mahoutout/part-r-00000

    Väljund kuvatakse järgmiselt:

        1   [234:5.0,347:5.0,237:5.0,47:5.0,282:5.0,275:5.0,88:5.0,515:5.0,514:5.0,121:5.0]
        2   [282:5.0,210:5.0,237:5.0,234:5.0,347:5.0,121:5.0,258:5.0,515:5.0,462:5.0,79:5.0]
        3   [284:5.0,285:4.828125,508:4.7543354,845:4.75,319:4.705128,124:4.7045455,150:4.6938777,311:4.6769233,248:4.65625,272:4.649266]
        4   [690:5.0,12:5.0,234:5.0,275:5.0,121:5.0,255:5.0,237:5.0,895:5.0,282:5.0,117:5.0]

    Esimene veerg on soovitud `userID`. Olevaid väärtusi "[" ja "]" on `movieId`:`recommendationScore`.

2. Saate selle väljundi koos moviedb.txt, kasutajasõbralik lisateabe kuvamiseks. Kõigepealt, on vaja kopeerida faile kohalikult abil järgmised käsud:

        hdfs dfs -get /example/data/mahoutout/part-r-00000 recommendations.txt
        hdfs dfs -get /HdiSamples/HdiSamples/MahoutMovieData/* .

    See fail nimega **recommendations.txt** praegust kausta, koos filmi andmefailid kopeerib väljundi andmeid.

3. Järgmise käsu abil saate luua uue Python skripti, mis otsib üles filmi nimede soovitused väljund andmete:

        nano show_recommendations.py

    Redaktori avamisel kasutada järgmist faili sisu.

        #!/usr/bin/env python

        import sys

        if len(sys.argv) != 5:
                print "Arguments: userId userDataFilename movieFilename recommendationFilename"
                sys.exit(1)

        userId, userDataFilename, movieFilename, recommendationFilename = sys.argv[1:]

        print "Reading Movies Descriptions"
        movieFile = open(movieFilename)
        movieById = {}
        for line in movieFile:
                tokens = line.split("|")
                movieById[tokens[0]] = tokens[1:]
        movieFile.close()

        print "Reading Rated Movies"
        userDataFile = open(userDataFilename)
        ratedMovieIds = []
        for line in userDataFile:
                tokens = line.split("\t")
                if tokens[0] == userId:
                        ratedMovieIds.append((tokens[1],tokens[2]))
        userDataFile.close()

        print "Reading Recommendations"
        recommendationFile = open(recommendationFilename)
        recommendations = []
        for line in recommendationFile:
                tokens = line.split("\t")
                if tokens[0] == userId:
                        movieIdAndScores = tokens[1].strip("[]\n").split(",")
                        recommendations = [ movieIdAndScore.split(":") for movieIdAndScore in movieIdAndScores ]
                        break
        recommendationFile.close()

        print "Rated Movies"
        print "------------------------"
        for movieId, rating in ratedMovieIds:
                print "%s, rating=%s" % (movieById[movieId][0], rating)
        print "------------------------"

        print "Recommended Movies"
        print "------------------------"
        for movieId, score in recommendations:
                print "%s, score=%s" % (movieById[movieId][0], score)
        print "------------------------"

    Vajutage klahvikombinatsiooni **Ctrl + X**, **Y**ja lõpuks **Enter** andmed salvestada.

3. Kasutage teha faili käivitatava järgmine käsk:

        chmod +x show_recommendations.py

4. Käivitage Python skript. Järgmine eeldab, olete kataloogis, kus kõik failid on alla laaditud:

        ./show_recommendations.py 4 user-ratings.txt moviedb.txt recommendations.txt

    See pilk loodud kasutaja ID 4 soovitusi.

    * **Kasutaja-ratings.txt** faili abil tuua filmid, mida kasutaja on hinnatud
    * Faili **moviedb.txt** kasutatakse Filmid nimed
    * **Recommendations.txt** kasutatakse filmi soovitused selle kasutaja jaoks

    See käsk väljund on järgmine:

        Reading Movies Descriptions
        Reading Rated Movies
        Reading Recommendations
        Rated Movies
        ------------------------
        Mimic (1997), rating=3
        Ulee's Gold (1997), rating=5
        Incognito (1997), rating=5
        One Flew Over the Cuckoo's Nest (1975), rating=4
        Event Horizon (1997), rating=4
        Client, The (1994), rating=3
        Liar Liar (1997), rating=5
        Scream (1996), rating=4
        Star Wars (1977), rating=5
        Wedding Singer, The (1998), rating=5
        Starship Troopers (1997), rating=4
        Air Force One (1997), rating=5
        Conspiracy Theory (1997), rating=3
        Contact (1997), rating=5
        Indiana Jones and the Last Crusade (1989), rating=3
        Desperate Measures (1998), rating=5
        Seven (Se7en) (1995), rating=4
        Cop Land (1997), rating=5
        Lost Highway (1997), rating=5
        Assignment, The (1997), rating=5
        Blues Brothers 2000 (1998), rating=5
        Spawn (1997), rating=2
        Wonderland (1997), rating=5
        In & Out (1997), rating=5
        ------------------------
        Recommended Movies
        ------------------------
        Seven Years in Tibet (1997), score=5.0
        Indiana Jones and the Last Crusade (1989), score=5.0
        Jaws (1975), score=5.0
        Sense and Sensibility (1995), score=5.0
        Independence Day (ID4) (1996), score=5.0
        My Best Friend's Wedding (1997), score=5.0
        Jerry Maguire (1996), score=5.0
        Scream 2 (1997), score=5.0
        Time to Kill, A (1996), score=5.0
        Rock, The (1996), score=5.0
        ------------------------

##<a name="delete-temporary-data"></a>Kustutage ajutised andmed

Mahout töö Eemalda ajutised andmed, mis on loodud ajal töö. Funktsiooni `--tempDir` parameeter on määratud näide töö eristamiseks ajutiste failide sisse teatud tee lihtne kustutamiseks. Ajutiste failide eemaldamiseks kasutage järgmine käsk:

    hdfs dfs -rm -f -r /temp/mahouttemp

> [AZURE.WARNING] Kui soovite käsu uuesti käivitada, siis kustutage väljundi kataloogi. Järgmine abil saate selle kausta kustutada.
>
> ```hdfs dfs -rm -f -r /example/data/mahoutout```

## <a name="next-steps"></a>Järgmised sammud

Nüüd, kui olete õppinud, kuidas kasutada Mahout, vaadake muid võimalusi Hdinsightiga andmetega töötamine:

* [Taru koos Hdinsightiga](hdinsight-use-hive.md)
* [Siga Hdinsightiga](hdinsight-use-pig.md)
* [MapReduce koos Hdinsightiga](hdinsight-use-mapreduce.md)

[build]: http://mahout.apache.org/developers/buildingmahout.html
[movielens]: http://grouplens.org/datasets/movielens/
[100k]: http://files.grouplens.org/datasets/movielens/ml-100k.zip
[getstarted]: hdinsight-hadoop-linux-tutorial-get-started.md
[upload]: hdinsight-upload-data.md
[ml]: http://en.wikipedia.org/wiki/Machine_learning
[forest]: http://en.wikipedia.org/wiki/Random_forest
[management]: https://manage.windowsazure.com/
[enableremote]: ./media/hdinsight-mahout/enableremote.png
[connect]: ./media/hdinsight-mahout/connect.png
[hadoopcli]: ./media/hdinsight-mahout/hadoopcli.png
[tools]: https://github.com/Blackmist/hdinsight-tools
 
