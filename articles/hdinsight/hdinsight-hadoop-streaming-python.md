<properties
   pageTitle="Arendamise Python MapReduce töökohtade Hdinsightiga | Microsoft Azure'i"
   description="Saate teada, kuidas luua ja käivitada Python MapReduce tööd Linuxi-põhiste Hdinsightiga kogumite."
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
   ms.date="10/11/2016"
   ms.author="larryfr"/>

#<a name="develop-python-streaming-programs-for-hdinsight"></a>Arendamise Python streaming programmide Hdinsightiga

Hadoopi pakub streaming API MapReduce, mis võimaldab kaardi kirjutamine ja vähendamiseks funktsioonide Java erinevas keeles. Sellest artiklist saate teada, kuidas kasutada Python MapReduce toimingute tegemiseks.

> [AZURE.NOTE] Ajal Python koodi selles dokumendis saab kasutada Windowsi-põhiste Hdinsightiga kobar, selle dokumendi juhised kehtivad teatud kogumite Linux-põhine.

Selles artiklis on alusel ja avaldatud Michael Noll [Programmis Hadoopi MapReduce Python](http://www.michael-noll.com/tutorials/writing-an-hadoop-mapreduce-program-in-python/)kirjutades näidetega.

##<a name="prerequisites"></a>Eeltingimused

Selles artiklis toodud juhiseid tegemiseks on vaja järgmist:

* Linux-põhine Hadoopi Hdinsightiga kobar

* Tekstiredaktoris

    > [AZURE.IMPORTANT] Tekstiredaktor tuleb kasutada LF rea lõpus. Kui seda kasutatakse CRLF, see põhjustada tõrkeid MapReduce projekti käivitamisel Linux-põhine Hdinsightiga kogumite kohta. Kui te pole kindel, valikuline etapp [Käivitada MapReduce](#run-mapreduce) jaotises teisendamiseks kasutada mis tahes CRLF LF.

* Windowsi kliendid kitt ja PSCP. Need Utiliidid on saadaval <a href="http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html" target="_blank">PuTTY alla laadida lehe</a>kaudu.

##<a name="word-count"></a>Sõnaarvestus

Selle näite puhul saate rakendada lihtsa sõnade plaanuri ja reduktor abil. Selle plaanuri jaotab lausete üksikuteks sõnadeks ja selle reduktor sõnad liitmise ja loendab andes väljund.

Järgmised vooskeem illustreerib, mis juhtub ajal kaardi ja vähendada etapist.

![kaardi joonise vähendamine](./media/hdinsight-hadoop-streaming-python/HDI.WordCountDiagram.png)

##<a name="why-python"></a>Miks Python?

Python on üldotstarbeline, üksikasjalik programmeerimiskeel, mis võimaldab teil kiire kontseptsioonid on vähem kui paljudes muudes keeltes koodiread. Seda viimati on sai populaarsed andmete Aserbaidžaan prototüüpimine keel, sest selle tõlgendada laad, dünaamiline tippimist ja elegantseid süntaks teeb sobib kiire rakenduste arendamise.

Kõik Hdinsightiga kogumite Python on installitud.

##<a name="streaming-mapreduce"></a>Streaming MapReduce

Hadoopi võimaldab teil määrata faili, mis sisaldab kaardi ja vähendamiseks loogika, mida kasutatakse töö. Kaardi teatud nõuded ja vähendamiseks loogika on:

* **Sisestuskeel**: kaart ja vähendamiseks komponendid lugeda sisendandmete stdin.

* **Väljundi**: kaart ja vähendamiseks komponendid tuleb kirjutada STDOUT väljundi andmeid.

* **Andmete vormindamine**: andmeid consumed ja toodeti peab olema paari /-väärtuse, eraldades need tabeldusmärgi.

Python saab hõlpsalt hakkama nendele nõuetele **sys** mooduli kasutamise stdin lugeda ja abil **Printimine** STDOUT printida. Ülejäänud tööülesanne on lihtsalt tabelduskoha abil andmete vormindamiseks (`\t`) märk võti ja väärtuse.

##<a name="create-the-mapper-and-reducer"></a>Plaanuri ja reduktor loomine

Plaanuri ja reduktor on tekstifailid, sel juhul **mapper.py** ja **reducer.py** (et oleks selge, mis teeb, mida). Saate luua need redaktoriga tehtud valikust.

###<a name="mapperpy"></a>Mapper.py

Looge uus fail nimega **mapper.py** ja kasutada sisu järgmine kood:

    #!/usr/bin/env python

    # Use the sys module
    import sys

    # 'file' in this case is STDIN
    def read_input(file):
        # Split each line into words
        for line in file:
            yield line.split()

    def main(separator='\t'):
        # Read the data using read_input
        data = read_input(sys.stdin)
        # Process each words returned from read_input
        for words in data:
            # Process each word
            for word in words:
                # Write to STDOUT
                print '%s%s%d' % (word, separator, 1)

    if __name__ == "__main__":
        main()

Mõne hetke koodi läbi lugeda, et saavad aru, mida see tähendab.

###<a name="reducerpy"></a>Reducer.py

Looge uus fail nimega **reducer.py** ja kasutada sisu järgmine kood:

    #!/usr/bin/env python

    # import modules
    from itertools import groupby
    from operator import itemgetter
    import sys

    # 'file' in this case is STDIN
    def read_mapper_output(file, separator='\t'):
        # Go through each line
        for line in file:
            # Strip out the separator character
            yield line.rstrip().split(separator, 1)

    def main(separator='\t'):
        # Read the data using read_mapper_output
        data = read_mapper_output(sys.stdin, separator=separator)
        # Group words and counts into 'group'
        #   Since MapReduce is a distributed process, each word
        #   may have multiple counts. 'group' will have all counts
        #   which can be retrieved using the word as the key.
        for current_word, group in groupby(data, itemgetter(0)):
            try:
                # For each word, pull the count(s) for the word
                #   from 'group' and create a total count
                total_count = sum(int(count) for current_word, count in group)
                # Write to stdout
                print "%s%s%d" % (current_word, separator, total_count)
            except ValueError:
                # Count was not a number, so do nothing
                pass

    if __name__ == "__main__":
        main()

##<a name="upload-the-files"></a>Laadige failid

Nii **mapper.py** ja **reducer.py** peavad olema klaster pea sõlme enne nende võtame. Lihtsaim viis üles laadida neid on kasutada **scp** (kui kasutate Windowsi kliendi**pscp** ).

Klient, samas kataloogis **mapper.py** ja **reducer.py**, kasutage järgmist käsku. Asendage **kasutajanimi** SSH kasutaja ja **clustername** klaster nimi.

    scp mapper.py reducer.py username@clustername-ssh.azurehdinsight.net:

Selle valiku puhul kopeeritakse failid kohalik süsteem pea sõlme.

> [AZURE.NOTE] Kui kasutasite parooli konto SSH, küsitakse teilt, kas soovite parooli. Kui olete kasutanud mõnda SSH võti, peate kasutama funktsiooni `-i` parameetri ja privaatvõti, näiteks tee `scp -i /path/to/private/key mapper.py reducer.py username@clustername-ssh.azurehdinsight.net:`.

##<a name="run-mapreduce"></a>Käivitage MapReduce

1. Ühendada klaster SSH abil.

        ssh username@clustername-ssh.azurehdinsight.net

    > [AZURE.NOTE] Kui kasutasite parooli konto SSH, küsitakse teilt, kas soovite parooli. Kui olete kasutanud mõnda SSH võti, peate kasutama funktsiooni `-i` parameetri ja privaatvõti, näiteks tee `ssh -i /path/to/private/key username@clustername-ssh.azurehdinsight.net`.

2. (Valikuline) Kui kasutasite tekstiredaktoris, mis kasutab CRLF loomisel mapper.py ja reducer.py failid, mille nimi lõpeb reana või ei tea, mis joon-lõpp oma redaktor kasutab, kasutada järgmisi käske esinemiskordade CRLF mapper.py ja reducer.py teisendamiseks LF.

        perl -pi -e 's/\r\n/\n/g' mappery.py
        perl -pi -e 's/\r\n/\n/g' reducer.py

2. Järgmise käsu abil saate alustada tööd MapReduce.

        yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar -files mapper.py,reducer.py -mapper mapper.py -reducer reducer.py -input wasbs:///example/data/gutenberg/davinci.txt -output wasbs:///example/wordcountout

    See käsk on järgmised osad:

    * **Hadoopi-streaming.jar**: kasutada, kui streaming MapReduce toimingute tegemiseks. Hadoopi liidesed väliste MapReduce koodi esitate.

    * **-failide**: ütleb Hadoopi, mis on määratud failid on vaja see MapReduce töö ja need kopeerida töötaja sõlmi.

    * **-plaanuri**: ütleb Hadoopi milline fail soovitud plaanuri kasutamiseks.

    * **-reduktor**: ütleb Hadoopi milline fail soovitud reduktor kasutamiseks.

    * **-Sisestuskeel**: Sisestuskeel fail, mida me peaks loendamine sõnu.

    * **-väljundi**: väljund kirjutatakse kataloogis.

        > [AZURE.NOTE] Töö on loodud see kaust.

Peaksite näha **teave** laused hulga töö käivitamise ja lõpuks näha kuvatakse protsentidena **kaarti** ja **vähendamiseks** toiming.

    15/02/05 19:01:04 INFO mapreduce.Job:  map 0% reduce 0%
    15/02/05 19:01:16 INFO mapreduce.Job:  map 100% reduce 0%
    15/02/05 19:01:27 INFO mapreduce.Job:  map 100% reduce 100%

Kui see on lõpule jõudnud, kuvatakse projekti olek teavet.

##<a name="view-the-output"></a>Vaate väljund

Kui töö on valmis, kasutage vaatamiseks väljund järgmine käsk:

    hdfs dfs -text /example/wordcountout/part-00000

See peaks loendi kuvamiseks sõnu ja mitu korda sõna ilmnenud. On valimi andmete väljund on järgmine:

    wrenching       1
    wretched        6
    wriggling       1
    wrinkled,       1
    wrinkles        2
    wrinkling       2

##<a name="next-steps"></a>Järgmised sammud

Nüüd, kui olete õppinud, kuidas kasutada streaming MapRedcue töö Hdinsightiga, järgmiste linkide abil muul viisil töötada Windows Azure Hdinsightiga uurimine.

* [Hdinsightiga taru kasutamine](hdinsight-use-hive.md)
* [Kasutage siga Hdinsightiga](hdinsight-use-pig.md)
* [Hdinsightiga MapReduce töö kasutamine](hdinsight-use-mapreduce.md)
