<properties
   pageTitle="Hadoopi siga kasutamine kaugtöölaua Hdinsightiga | Microsoft Azure'i"
   description="Saate teada, kuidas siga käsu abil saate käivitada siga Ladina laused kaugtöölaua ühendus Windowsi-põhiste Hadoopi arvutikobaras Hdinsightiga sisse."
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

#<a name="run-pig-jobs-from-a-remote-desktop-connection"></a>Kaugtöölaua ühendus pidamine siga tööde haldamine

[AZURE.INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

Selles dokumendis on toodud lühiülevaade käsk Sea käivitada siga Ladina laused kaugtöölaua ühendus Windowsi-põhiste Hdinsightiga arvutikobaras kasutamise kohta. Siga Ladina võimaldab teil luua MapReduce rakenduste andmete teisendused, mis kirjeldab, mitte kaarti ja vähendada funktsioonid.

Selles dokumendis saate teada, kuidas

##<a id="prereq"></a>Eeltingimused

Selles artiklis toodud juhiseid tegemiseks on vaja järgmist.

* Windowsi-põhiste Hdinsightiga (Hadoopi Hdinsightiga) kobar

* Kliendi arvutisse, kus töötab Windows 10, Windows 8 või Windows 7

##<a id="connect"></a>Kaugtöölaua kasutajaga

Luba kaugtöölaud Hdinsightiga kobar, siis selle ühenduse juures [Hdinsightiga kogumite abil RDP ühenduse](hdinsight-administer-use-management-portal.md#rdp)juhiste järgi.

##<a id="pig"></a>Käsuga siga

2. Kui teil on kaugtöölaua ühendus, käivitage **Hadoopi käsurea** ikooni töölaual.

2. Käsk Sea alustamiseks kasutage järgmist:

        %pig_home%\bin\pig

    Esitatakse koos mõne `grunt>` küsimus.

3. Sisestage järgmine tekst:

        LOGS = LOAD 'wasbs:///example/data/sample.log';

    See käsk laadib LOGID faili sample.log faili sisu. Järgmise käsu abil saate vaadata faili sisu:

        DUMP LOGS;

4. Andmeid muuta, rakendades regulaarne avaldis eraldada ainult Logimistase iga kirje:

        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;

    Pärast andmete kuvamiseks saate kasutada **DUMP** . Sel juhul `DUMP LEVELS;`.

5. Jätkake rakendamist teisendused abil järgmistest. Kasutage `DUMP` iga toimingu järel pöördteisenduse tulemi kuvamiseks.

    <table>
    <tr>
    <th>Lause</th><th>Mida see tähendab?</th>
    </tr>
    <tr>
    <td>FILTEREDLEVELS = FILTER taset LOGLEVEL pole null;</td><td>Eemaldab read, mis sisaldavad tühiväärtus log taseme ning see talletab tulemused FILTEREDLEVELS sisse.</td>
    </tr>
    <tr>
    <td>GROUPEDLEVELS = rühma FILTEREDLEVELS, LOGLEVEL;</td><td>Pakuvad log taseme read ja salvestab tulemused GROUPEDLEVELS sisse.</td>
    </tr>
    <tr>
    <td>SAGEDUS = foreach GROUPEDLEVELS luua rühma nimega LOGLEVEL COUNT (FILTEREDLEVELS. LOGLEVEL) nagu loendamine;</td><td>Loob uue andmeid, mis sisaldab iga kordumatu log ilmneb väärtusi ja mitu korda. See on talletatud sageduse muutmine</td>
    </tr>
    <tr>
    <td>TULEMUS = tellimuse sageduse järgi COUNT desc;</td><td>Tellimuste log tasemete arvu (laskuvas järjestuses) järgi ning see talletab tulemuse sisse</td>
    </tr>
    </table>

6. Abil saate salvestada ka on teisendus tulemused kuvatakse `STORE` lause. Näiteks järgmine käsk salvestab soovitud `RESULT` **/example/data/pigout** kataloogi vaikimisi salvestusruumi ümbrises klaster jaoks:

        STORE RESULT into 'wasbs:///example/data/pigout'

    > [AZURE.NOTE] Andmed on salvestatud failid nimega **osa-nnn**määratud kausta. Kui kaust on juba olemas, saate tõrketeate.

7. Väljuge grunt viip, sisestage järgmine tekst.

        QUIT;

###<a name="pig-latin-batch-files"></a>Siga Ladina paketi failid

Siga käsu abil saate käivitada siga ladina, mis on toodud faili.

3. Pärast väljumist grunt viip, **Notepadi** avamine ja nimega **pigbatch.pig** kataloogis **PIG_HOME %** uue faili loomine.

4. Tippige või kleepige järgmised read **pigbatch.pig** faili ja seejärel salvestage see:

        LOGS = LOAD 'wasbs:///example/data/sample.log';
        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
        FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
        GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
        FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
        RESULT = order FREQUENCIES by COUNT desc;
        DUMP RESULT;

5. Järgmine abil saate käivitada käsk Sea **pigbatch.pig** faili.

        pig %PIG_HOME%\pigbatch.pig

    Kui pakett-töö on lõpule jõudnud, kuvatakse järgmine väljund, mis peaks olema sama, kui kasutasite `DUMP RESULT;` eelmiste juhiste järgi:

        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)

##<a id="summary"></a>Kokkuvõte

Nagu näete, käsk Sea võimaldab teil interaktiivseks käitada MapReduce toimingud või siga Ladina tööd, mis on talletatud faili paketi käivitamine.

##<a id="nextsteps"></a>Järgmised sammud

Üldist teavet siga Hdinsightiga kohta:

* [Kasutage siga Hadoopi Hdinsightiga](hdinsight-use-pig.md)

Teavet teiste mooduste kohta saate töötada Hadoopi Hdinsightiga:

* [Hadoopi Hdinsightiga taru kasutamine](hdinsight-use-hive.md)

* [Hadoopi Hdinsightiga MapReduce kasutamine](hdinsight-use-mapreduce.md)
