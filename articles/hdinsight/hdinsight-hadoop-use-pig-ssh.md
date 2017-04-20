<properties
   pageTitle="Kasutage SSH Hadoop siga konkreetne klastri | Microsoft Azure"
   description="Õppida, kuidas ühendada Hadoop Linuxi põhises klastri SSH ja kasutage siga käsu käivitada Pig Ladina avaldused interaktiivselt või pakett-tööna."
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

#<a name="run-pig-jobs-on-a-linux-based-cluster-with-the-pig-command-ssh"></a>Linuxi baasil klastri siga käsk (SSH) siga tööde käitamine

[AZURE.INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

Selles dokumendis käid läbi protsessi ühendamine Linux-põhine Azure konkreetne klastri Secure Shell (SSH), käivitada Pig Ladina avaldused interaktiivselt, siga käsu abil või pakett-tööna.

Searasv Ladina programmeerimiskeel võimaldab kirjeldada muutusi, mida kantakse andmed soovitud väljundi tootmiseks.

> [AZURE.NOTE] Kui oled juba tuttav Hadoop Linuxi-põhiste serverite kasutamine, kuid ei ole konkreetne, vt [Linuxi-põhiste konkreetne näpunäiteid](hdinsight-hadoop-linux-information.md).

##<a id="prereq"></a>Eeltingimused

Käesoleva artikli juhiste vajate järgmist.

* Linuxi-põhiste konkreetne (konkreetne Hadoopi) klastri.

* SSH kliendi. Linux, Unix ja Mac OS peaks tulema SSH kliendi. Windowsi kasutajad alla laadida klient, nagu [kitt](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

##<a id="ssh"></a>Kasutajaga SSH

Täielik domeeninimi (FQDN) konkreetne klastri abil ühendust SSH käsk. FQDN on nimi, mille panite klaster, siis **. azurehdinsight.net**. Näiteks järgmine ühenduvad klastri nimega **myhdinsight**.

    ssh admin@myhdinsight-ssh.azurehdinsight.net

**Kui teie esitatud sertifikaadi võti SSH autentimiseks** konkreetne klastri loomisel, peate isikliku võtme asukoha määramiseks kliendi arvutisse.

    ssh admin@myhdinsight-ssh.azurehdinsight.net -i ~/mykey.key

**Kui andsite SSH paroolina** konkreetne klastri loomisel, pead sisestama parooli, kui küsitakse.

Konkreetne SSH kasutamise kohta lisateabe saamiseks vt [Kasutamine SSH koos Linux-põhine Hadoopi konkreetne Linux, OS X ja Unix](hdinsight-hadoop-linux-use-ssh-unix.md).

###<a name="putty-windows-based-clients"></a>Kitt (Windowsi-põhiste kliendid)

Windows pakub sisseehitatud SSH klient. Soovitame kasutada **kitt**, mille saab alla laadida [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

Kitt kasutamise kohta lisateabe saamiseks vt [Kasutamine SSH koos Linux-põhine Hadoopi konkreetne Windowsist ](hdinsight-hadoop-linux-use-ssh-windows.md).

##<a id="pig"></a>Käsu siga

2. Pärast ühenduse loomist käivitage siga käsurea liides (CLI) järgmine käsk.

        pig

    Hetke pärast, et sa näed on `grunt>` kiire.

3. Sisestage järgmine avaldus.

        LOGS = LOAD 'wasbs:///example/data/sample.log';

    See käsk laadib faili sample.log sisu kõrguses. Kuvatakse faili sisu, kasutades järgmist.

        DUMP LOGS;

4. Järgmine, muuta andmeid kasutades regulaaravaldise logimise taseme ekstraktimiseks iga kirje on toodud.

        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;

    Pärast andmete kuvamiseks saate kasutada **DUMP** . Sel juhul kasutada `DUMP LEVELS;`.

5. Jätkata teisenduste abil järgmistest. Kasutamine `DUMP` transformatsiooni pärast iga sammu tulemuse kuvamiseks.

    <table>
    <tr>
    <th>Avaldus</th><th>Tegevusvaldkonnad</th>
    </tr>
    <tr>
    <td>FILTEREDLEVELS = FILTER taset LOGLEVEL on null;</td><td>Eemaldab read, mis sisaldavad Logi tase tühiväärtust ja salvestab tulemused arvesse FILTEREDLEVELS.</td>
    </tr>
    <tr>
    <td>GROUPEDLEVELS = rühma FILTEREDLEVELS poolt LOGLEVEL;</td><td>Rühmitab read Logi tase ja salvestab tulemused arvesse GROUPEDLEVELS.</td>
    </tr>
    <tr>
    <td>SAGEDUSTE = foreach GROUPEDLEVELS luua rühma nagu LOGLEVEL, KRAHV (FILTEREDLEVELS. LOGLEVEL) nagu LOOTA;</td><td>Loob uued mis sisaldab iga unikaalne palk väärtusi ja kui mitu korda ta ilmneb. See on salvestatud sagedustesse.</td>
    </tr>
    <tr>
    <td>TULEMUS = tellimuse sageduste poolt arv desc;</td><td>Jätta Logi tasemete arvu (kahanevalt) järgi ja tulemus arvesse kauplustes.</td>
    </tr>
    </table>

6. Samuti ümberkujundamise tulemuste abil on `STORE` avaldus. Näiteks järgmine salvestab selle `RESULT` **/example/data/pigout** kataloog vaikimisi Hoiumahuti oma klastri kohta.

        STORE RESULT into 'wasbs:///example/data/pigout';

    > [AZURE.NOTE] Andmed salvestatakse määratud kataloogi **osa-nnnnn**nimetatud failides. Kui kaust on juba olemas, kuvatakse tõrge.

7. Väljumiseks grunt kui küsitakse, sisestage järgmise.

        QUIT;

###<a name="pig-latin-batch-files"></a>Searasv Ladina pakkfailide

Saate käsu siga Pig Ladina sisalduvat faili käivitada.

3. Pärast väljumist grunt viip, kasutage järgmist käsku toru STDIN faili, **pigbatch.pig**. See fail luuakse konto, logite sisse et SSH sessiooni kodukataloog.

        cat > ~/pigbatch.pig

4. Tippige või kleepige järgmised koodiread ja seejärel kasutage klahvikombinatsiooni Ctrl + D lõppedes.

        LOGS = LOAD 'wasbs:///example/data/sample.log';
        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
        FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
        GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
        FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
        RESULT = order FREQUENCIES by COUNT desc;
        DUMP RESULT;

5. Kasuta järgmisi **pigbatch.pig** faili käivitamiseks käsku siga.

        pig ~/pigbatch.pig

    Kui pakett-töö on lõppenud, peaks nägema järgmist väljundit, mis peaks olema sama kui `DUMP RESULT;` eelmiste toimingute käigus.

        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)

##<a id="summary"></a>Kokkuvõte

Nagu näete, siga käsu abil saate interaktiivselt MapReduce operatsioonide abil Pig ladina, samuti käivitada maksejuhiste salvestatud avaldused.

##<a id="nextsteps"></a>Järgmised sammud

Üldist teavet siga, konkreetne.

* [Kasutada siga Hadoopi konkreetne](hdinsight-use-pig.md)

Teavet teiste mooduste kohta saate töötada konkreetne Hadoopi.

* [Kasutada taru Hadoopi konkreetne](hdinsight-use-hive.md)

* [Kasutada MapReduce Hadoopi konkreetne](hdinsight-use-mapreduce.md)
