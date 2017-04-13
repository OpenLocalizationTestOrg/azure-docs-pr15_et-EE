<properties
    pageTitle="Käivita Hadoopi MapReduce näidised Linux-põhine Hdinsightile | Microsoft Azure'i"
    description="MapReduce näidiseid ja Linux-põhine Hdinsightiga kasutamise alustamine. Ühenduse loomine klaster SSH abil, siis Hadoopi käsu abil saate käivitada valimi tööd."
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
    ms.date="09/27/2016"
    ms.author="larryfr"/>




#<a name="run-the-hadoop-samples-in-hdinsight"></a>Hadoopi näidised läbiviimisel Hdinsightiga

[AZURE.INCLUDE [samples-selector](../../includes/hdinsight-run-samples-selector.md)]

Linux-põhine Hdinsightiga kogumite pakuvad MapReduce näidised, tutvuge Hadoopi MapReduce tööde käitamist kasutatavate kogum. Selles dokumendis teave saadaval näidiseid ja üle mõned neist töötab.

##<a name="prerequisites"></a>Eeltingimused

- **An Azure'i tellimus**: leiate [Azure'i saada tasuta prooviversioon](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)

- **A Linux-põhine Hdinsightiga kobar**: vt [Hadoopi taru rakenduses Hdinsightiga Linux koos kasutamise alustamine](hdinsight-hadoop-linux-tutorial-get-started.md)

- **An SSH kliendi**: SSH abil Hdinsightiga kohta lisateabe saamiseks lugege järgmisi artikleid:

    - [Kasutada SSH Linux-põhine Hadoopi Hdinsightiga Linux, Unix või OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

    - [Kasutada SSH Linux-põhine Hadoopi Windows Hdinsightiga](hdinsight-hadoop-linux-use-ssh-windows.md)

## <a name="the-samples"></a>Näidised ##

**Asukoht**: näidised asuvad Hdinsightiga klaster veebisaidil **/usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar**

**Sisu**: järgmised näidet sisalduvad arhiivi:

- **aggregatewordcount**: An liitväärtuse vastavalt kaardi või vähendada programmi, mis loeb sõnu Sisestuskeel failid
- **aggregatewordhist**: An liitväärtuse vastavalt kaardi või vähendada programmi, mis arvutab histogrammi sõnade Sisestuskeel failid
- **neljal**: kaardi või vähendada programmi, mis kasutab Bailey-Borwein-Plouffe arvutada täpse numbrit Pi
- **dbcount**: näide töö, mis loendab kuvamise logid talletatud lisamine andmebaasi
- **distbbp**: kaart või vähendada programmi, mis kasutab NELJAL tüüpi valemiga arvutada täpse bittide pii
- **grep**: kaardi või vähendada programmi, mis loendab on regex panus vasteid
- **Liitu**: töö, et ühenduse mõju üle sorditud, võrdselt liigendatud andmekomplektide
- **multifilewc**: tööd, mis loendab, mitu faili sõnu
- **pentomino**: kaardi või vähendada paani, millega programmi pentomino probleemide lahenduste leidmiseks
- **pii**: kaardi või vähendada programmi, mis hindab Pi abil arvestusliku Monte Carlo meetod
- **randomtextwriter**: kaart või vähendada programmi, mis kirjutab 10 GB sõlm euro kohta juhusliku tekstandmeid
- **randomwriter**: kaart või vähendada programmi, mis kirjutab 10 GB sõlm euro kohta juhusliku andmete
- **secondarysort**: näide on vähendada teisene sortimise määratlemine
- **sortimine**: kaardi või vähendada programmi, mis sordib kirjutanud juhusliku kirjutaja andmed
- **Sudoku**: sudoku solver
- **teragen**: andmete jaoks soovitud terasort loomiseks
- **terasort**: selle terasort käivitamine
- **teravalidate**: terasort tulemuste kontrollimine
- **wordcount**: kaart või vähendada programmi, mis loeb sõnu Sisestuskeel failid
- **wordmean**: kaart või vähendada programmi, mis loendab keskmine pikkus, sõnad Sisestuskeel failid
- **wordmedian**: kaart või vähendada programmi, mis loendab median pikkus sõnad Sisestuskeel failid
- **wordstandarddeviation**: kaart või vähendada programmi, mis loeb sõnad Sisestuskeel failid pikkus standardhälve

**Lähtekoodi**: need näidised lähtekoodi lisatud Hdinsightiga kobar veebisaidil **/usr/hdp/2.2.4.9-1/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples**

> [AZURE.NOTE] Funktsiooni `2.2.4.9-1` tee on Hortonworks andmete platvormi Hdinsightiga kobar versiooni ja võib muutuda Hdinsightiga värskendatakse.

## <a name="how-to-run-the-samples"></a>Kuidas käivitada näidised ##

1. Ühenduse loomine Hdinsightiga SSH abil, nagu on kirjeldatud järgmistes artiklites:

    - [Kasutada SSH Linux-põhine Hadoopi Hdinsightiga Linux, Unix või OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

    - [Kasutada SSH Linux-põhine Hadoopi Windows Hdinsightiga](hdinsight-hadoop-linux-use-ssh-windows.md)

2. Kaudu soovitud `username@#######:~$` Küsi, loendi näidised järgmise käsu abil:

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar

    See loob valimi loendi eelmise jaotise selle dokumendi.

3. Järgmise käsu abil saate abi konkreetse valimi. Sel juhul **wordcount** näidis:

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount

    Peaksite nägema järgmine teade:

        Usage: wordcount <in> [<in>...] <out>

    See näitab, et saate sisestada mitu Sisestuskeel teed allika dokumentide. Lõplik tee on koht, kus talletatakse väljundi (dokumentides allika sõnade arv).

4. Kõigi märkmike Leonardo Da Vinci, mis on toodud Näidisandmete klaster sõnade loendamine kasutada järgmist:

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount /example/data/gutenberg/davinci.txt /example/data/davinciwordcount

    Selle töö sisendit ei loe **wasbs:///example/data/gutenberg/davinci.txt**.

    Selles näites on talletatud väljund **wasbs: / / / näide/andmete/davinciwordcount**.

    > [AZURE.NOTE] Nagu wordcount valimi abi, saate määrata ka mitu failid. Näiteks `hadoop jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount /example/data/gutenberg/davinci.txt /example/data/gutenberg/ulysses.txt /example/data/twowordcount` oleks davinci.txt nii ulysses.txt sõnade loendamine.

5. Pärast töö lõpulejõudmist järgmise käsu abil saate vaadata väljund:

        hdfs dfs -cat /example/data/davinciwordcount/*

    See ühendab väljund faili toodetud töö ja neid kuvada. See lihtne näide on ainult üks fail, aga kui oleks rohkem selle käsu oleks itereerima üle kõik need.

    Väljund on järgmine:

        zum     1
        zur     1
        zwanzig 1
        zweite  1

    Iga rida tähistab sõna ja kui palju kordi ilmnenud sisendandmete.

## <a name="sudoku"></a>Sudoku

Sudoku näide on mõnevõrra kasutu kasutusjuhendid; "Hulka mõistatuse käsureal."

[Sudoku](https://en.wikipedia.org/wiki/Sudoku) on loogika mõistatuse üheksa 3 x 3 võrgud koosneb. Mõne lahtri ruudustiku on numbrid, samas kui teised on tühi ja eesmärk on lahendada tühjade lahtrite. Linki sisaldab rohkem teavet puzzle, kuid see valimi eesmärk on lahendada tühjade lahtrite. Nii tuleks meie sisendit faili, mis on järgmises vormingus:

- Üheksa üheksa veergude read

- Iga veeru võib sisaldada kas number või `?` (mis näitab tühi lahter)

- Lahtrid on eraldatud ruumi

On võimalus ehitada Sudoku mõistatused; ei saa korrake rea või veeru number. On näiteks Hdinsightiga kobar, et valem oleks õigesti koostatud. See asub **/usr/hdp/2.2.4.9-1/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples/src/main/java/org/apache/hadoop/examples/dancing/puzzle1.dta** ja sisaldab järgmist:

    8 5 ? 3 9 ? ? ? ?
    ? ? 2 ? ? ? ? ? ?
    ? ? 6 ? 1 ? ? ? 2
    ? ? 4 ? ? 3 ? 5 9
    ? ? 8 9 ? 1 4 ? ?
    3 2 ? 4 ? ? 8 ? ?
    9 ? ? ? 8 ? 5 ? ?
    ? ? ? ? ? ? 2 ? ?
    ? ? ? ? 4 5 ? 7 8

> [AZURE.NOTE] Funktsiooni `2.2.4.9-1` tee osa võib muutuda Hdinsightiga kobar tehtud värskendused.

Käivitamiseks Sudoku näide kaudu, kasutage järgmist käsku:

    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar sudoku /usr/hdp/2.2.9.1-1/hadoop/src/hadoop-mapreduce-project/hadoop-mapreduce-examples/src/main/java/org/apache/hadoop/examples/dancing/puzzle1.dta

Tulemused kuvatakse järgmine:

    8 5 1 3 9 2 6 4 7
    4 3 2 6 7 8 1 9 5
    7 9 6 5 1 4 3 8 2
    6 1 4 8 2 3 7 5 9
    5 7 8 9 6 1 4 2 3
    3 2 9 4 5 7 8 1 6
    9 4 7 2 8 6 5 3 1
    1 8 5 7 3 9 2 6 4
    2 6 3 1 4 5 9 7 8

## <a name="pi-"></a>Pii (π)

Pii näide kasutab statistilise (arvestusliku Monte Carlo) meetod pii väärtuse. Punktide paigutada juhusliku sees üksuse kuuluvad ka ruudu Lisaja ruutu koos tõenäosus on võrdne soovitud ringi pindala ringi pii/4. Pii väärtuse saab hinnata 4R, kus R on punkte, mis on kokku arvu punkte, mis on ruudu ringi sees arvu faktoriaali suhte väärtus. Suurem valimi kasutatud punktide, seda paremini hinnata on.

Selle valimi plaanuri genereeritud juhusliku sees üksuse ruut punktide arv ja seejärel loendab nendes punktides, mis on ringi sees.

Selle reduktor siis liidetakse punktide loendatud soovitud mappers ja hindab: valemi 4R, kus R on ringi kokku arvu punkte, mis on ruudu sees arvestatud punktide arvu faktoriaali suhte pii väärtuse.

Järgmise käsu abil saate käivitada selle valimi. See kasutab 16 kaartide 10000000 proovi prognoosimiseks pii väärtuse:

    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar pi 16 10000000

See poolt tagastatud väärtus peab olema **3.14159155000000000000**. Viited, on pii esimese 10 kümnendkohtade 3.1415926535.

##<a name="10gb-greysort"></a>10GB Greysort

GraySort on võrdlusalus sortimine, mille väärtuseks meetermõõdustik on sortimine määr (TB minutis) on väga suurte andmehulkade, tavaliselt 100TB minimaalne sortimise saavutada.

See näide kasutab mõõdukas 10GB andmete nii, et seda saab käivitada suhteliselt kiiresti. Kasutab MapReduce rakenduste Owen O'Malley ja Arun Murthy võidetud aastase üldotstarbeline ("daytona") Teratavu sortimise aluseks 2009 määra 0.578 TB/min (100 TB 173 minutites). Selle ja muude sortimist kriteeriumid kohta leiate lisateavet teemast [Sortbenchmark](http://sortbenchmark.org/) saidile.

See näide kasutab MapReduce programmide kolme rühma:

- **TeraGen**: MapReduce programmi, mis loob ridade andmete sortimiseks

- **TeraSort**: näidised sisendandmete ja kasutab MapReduce andmete sortimiseks kokku tellimusse

    TeraSort on standardne MapReduce ülesanded, välja arvatud kohandatud partitioner, mis kasutab sorditud loendi n-1 valimisse tutvustatakse, mida võtme vahemiku jaoks iga vähendamine. Eelkõige kõik võtmed sellise proovi [i-1] < = võti < valimi, [k] saadetakse ma vähendada. See tagab, et väljundid vähendada mul on väiksem kui väljund vähendada i + 1.

- **TeraValidate**: MapReduce programmi, mis kinnitab, et väljund on globaalselt sorditud

    See loob ühe kaardi faili kohta kataloogis väljundi ja iga kaart tagab, et iga võti on väiksem või võrdne eelmine. Funktsiooni kaardi loob ka kirjeid iga faili esimese ja viimase klahvide ja funktsiooni vähendamine tagab, et faili esimene võti mul on suurem kui viimase võti faili i-1. Probleemideta on staatusega vähendada väljund abil, mis on vales järjestuses.

Kasutage järgmisi juhiseid andmeid, luua sortida, ja seejärel kinnitage väljund:

1. 10GB andmeid, mis talletab Hdinsightiga kobar vaikimisi salvestusruum luua **wasbs: / / / Näide / / 10GB – sortimine-andmesisestuse**:

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teragen -Dmapred.map.tasks=50 100000000 /example/data/10GB-sort-input

    Funktsiooni `-Dmapred.map.tasks` ütleb Hadoopi mitu kaarti tööülesannete kasutamine selle töö. Lõplik kaks parameetrid paluge töö 10GB väärt andmeid luua ja talletada veebisaidil **wasbs: / / / Näide / / 10GB – sortimine-andmesisestuse**.

2. Kasutage andmete sortimiseks järgmine käsk:

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar terasort -Dmapred.map.tasks=50 -Dmapred.reduce.tasks=25 /example/data/10GB-sort-input /example/data/10GB-sort-output

    Funktsiooni `-Dmapred.reduce.tasks` ütleb Hadoopi mitu vähendamiseks kasutada projekti tööülesanded. Lõplik kaks parameetrid on lihtsalt andmete sisend- ja asukohad.

3. Järgmine abil loodud sortimine andmete valideerimine.

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teravalidate -Dmapred.map.tasks=50 -Dmapred.reduce.tasks=25 /example/data/10GB-sort-output /example/data/10GB-sort-validate

##<a name="next-steps"></a>Järgmised sammud ##

Käesolevast artiklist soovite õpitut käivitamise valimites koos Linux-põhine Hdinsightiga rühmad. Õpetused Hdinsightiga siga, taru ja MapReduce kasutamise kohta, leiate järgmistest teemadest:

* [Kasutage siga Hadoopi Hdinsightiga][hdinsight-use-pig]
* [Hadoopi Hdinsightiga taru kasutamine][hdinsight-use-hive]
* [Hadoopi Hdinsightiga MapReduce kasutamine] [hdinsight-use-mapreduce]



[hdinsight-errors]: hdinsight-debug-jobs.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-sdk-documentation]: https://msdn.microsoft.com/library/azure/dn479185.aspx

[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-introduction]: hdinsight-hadoop-introduction.md



[hdinsight-samples]: hdinsight-run-samples.md

[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
