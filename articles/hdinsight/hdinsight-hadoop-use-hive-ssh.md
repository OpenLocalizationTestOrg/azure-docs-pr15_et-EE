<properties
   pageTitle="Kasutage taru shell Hdinsightiga (Hadoopi) | Microsoft Azure'i"
   description="Saate teada, kuidas kasutada taru shell Linux-põhine Hdinsightiga kobar. Saate teada, kuidas ühenduse Hdinsightiga klaster SSh abil ja seejärel taru shelli abil saate käivitada interaktiivseks päringute."
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
   ms.date="10/04/2016"
   ms.author="larryfr"/>

# <a name="use-hive-with-hadoop-in-hdinsight-with-ssh"></a>Kasutada taru Hadoopi rakenduses Hdinsightiga SSH

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

Sellest artiklist saate teada, kuidas kasutada Secure Shell (SSH) ühenduse loomine Hadoopi Windows Azure Hdinsightiga kobar ja seejärel interaktiivseks esitada taru päringute abil taru käsurea liides (CLI).

> [AZURE.IMPORTANT] Taru käsk on saadaval Linuxi-põhiste Hdinsightiga kogumite, võiksite kasutada Linnulennutee. Linnulennutee on uuem kliendi töötamiseks taru ja on kaasas Hdinsightiga klaster. Selle kasutamise kohta leiate lisateavet teemast [Kasutamine taru koos Hadoopi Linnulennutee Hdinsightiga sisse](hdinsight-hadoop-use-hive-beeline.md).

##<a id="prereq"></a>Eeltingimused

Selles artiklis toodud juhiseid tegemiseks on vaja järgmist:

* Mõne Linuxi-põhiste Hadoopi Hdinsightiga kobar.

* SSH kliendi. Linux, Unix ja Mac OS-is peaks tulema SSH kliendi. Windowsi kasutajad peab klient, nt [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)alla laadida.

##<a id="ssh"></a>Kasutajaga SSH

Ühenduse täielik domeeninimi (FQDN), Hdinsightiga klaster SSH käsu abil. FQDN saab teie andsite kobar, siis **. azurehdinsight.net**. Näiteks järgmine soovite ühendada nimega **myhdinsight**klaster.

    ssh admin@myhdinsight-ssh.azurehdinsight.net

**Kui sertifikaadi võti SSH autentimise** Hdinsightiga kobar loomisel, võib-olla peate määrama privaatvõti asukoha kliendi arvutisse:

    ssh -i ~/mykey.key admin@myhdinsight-ssh.azurehdinsight.net

**Kui SSH autentimise parooli** loomisel Hdinsightiga kobar, peate sisestama parooli küsimise.

Hdinsightiga SSH kasutamise kohta leiate lisateavet teemast [Kasutamine SSH koos Linux-põhine Hadoopi Hdinsightiga Linux, OS X-ja Unix](hdinsight-hadoop-linux-use-ssh-unix.md).

###<a name="putty-windows-based-clients"></a>Kitt (Windowsi-põhiste kliendid)

Windows ei paku sisseehitatud SSH klient. Soovitame kasutada **kitt**, mille saate alla laadida [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

PuTTY kasutamise kohta leiate lisateavet teemast [Kasutamine SSH koos Linux-põhine Hadoopi Hdinsightiga Windows ](hdinsight-hadoop-linux-use-ssh-windows.md).

##<a id="hive"></a>Käsuga taru

2. Kui ühendus on loodud, käivitage taru CLI järgmine käsk:

        hive

3. Sisestage CLI kasutades nimega **log4jLogs** Näidisandmete abil uue tabeli loomiseks järgmistest:

        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    Need teatised järgmisi toiminguid:

    * **Tabeli DROP** - kustutab tabeli ja selle andmefaili juhuks, kui tabel on juba olemas.
    * **Välise tabeli loomine** – loob uue tabeli "väline" taru. Välise tabeleid talletada ainult taru tabeli määratlus. Andmed on jäänud algsesse asukohta.
    * **Rea vorming** – taru saate teada, kuidas andmed on vormindatud. Sel juhul iga log väljad on eraldatud ruumi.
    * **Salvestatud AS TEKSTIFAILIDE asukoht** – ütleb taru, kus andmed on salvestatud (nt/andmete kataloogi) ja see, et tekstina.
    * **Valige** - valib kõik read, kus veerus **t4** sisaldab väärtust **[ERROR]**arv. See peaks tagastama väärtus **3** , kui palju on kolm rida, mis sisaldavad seda väärtust.
    * **INPUT__FILE__NAME nagu "%.log"** - kirjeldatakse, mida me ainult tagasi andmete failidest, mille lõpus on taru. log. See piirab otsingu sample.log faili, mis sisaldab andmeid, ja tagastab andmed muu näiteks andmefailid, mis ei vasta meil määratletud skeemiga hoiab.

    > [AZURE.NOTE] Kui eeldate alusandmete värskendada välise andmeallikaga, nagu on automaatne andmete üleslaadimine või mõne muu MapReduce toiming, kuid tahate alati taru päringute uusimaid andmeid kasutada tuleks kasutada välise tabeleid.
    >
    > Kas pukseerimine on Välistabel **ei** Kustuta andmed tabeli määratlus.

4. Uue nimega **errorLogs**"sisemine" tabeli loomiseks kasutada järgmistest:

        CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
        INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';

    Need teatised järgmisi toiminguid:

    * **Looge tabel kui pole olemas** - loob tabeli, kui see pole juba olemas. Kuna **väline** märksõna ei kasutata, see on sisemine tabel, mis on talletatud taru andmebaas ja hallatakse täielikult taru.
    * **Salvestatud AS ORC** - salvestab andmed optimeeritud rea piklikku (ORC) vormingus. See on väga optimeeritud ja tõhusa vorming taru andmete talletamiseks.
    * **Lisa kirjutada... Valige** - klõpsab ridade **log4jLogs** tabeli, mis sisaldavad **[ERROR]**, seejärel lisab andmed tabelisse **errorLogs** .

    Veenduge, et ainult read, mis sisaldab veergu t4 **[ERROR]** talletatakse **errorLogs** tabelisse, abil järgmine lause tulu **errorLogs**kõiki ridu.

        SELECT * from errorLogs;

    Kolm rida andmete tuleks tagastada, kõik sisaldavad **[ERROR]** veeru t4.

    > [AZURE.NOTE] Erinevalt välise tabeleid, pukseerimine on sisemine tabel kustutatakse ka aluseks olevad andmed.

##<a id="summary"></a>Kokkuvõte

Nagu näete, taru käsu abil on lihtne interaktiivseks taru päringute käitamist on Hdinsightiga kobar, töö oleku jälgimine ja tuua väljund.

##<a id="nextsteps"></a>Järgmised sammud

Üldteavet taru sisse Hdinsightiga:

* [Hadoopi Hdinsightiga taru kasutamine](hdinsight-use-hive.md)

Teavet teiste mooduste kohta, saate töötada Hadoopi Hdinsightiga:

* [Kasutage siga Hadoopi Hdinsightiga](hdinsight-use-pig.md)

* [Hadoopi Hdinsightiga MapReduce kasutamine](hdinsight-use-mapreduce.md)

Kui kasutate Tez taru, leiate teavet silumine järgmised dokumendid.

* [Windowsi-põhiste Hdinsightiga Tez UI kasutamine](hdinsight-debug-tez-ui.md)

* [Kasutage Linux-põhine Hdinsightiga Ambari Tez vaade](hdinsight-debug-ambari-tez-view.md)

[hdinsight-sdk-documentation]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: http://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/


[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md

[putty]: http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html

[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md



[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx


[img-hdi-hive-powershell-output]: ./media/hdinsight-use-hive/HDI.Hive.PowerShell.Output.png

