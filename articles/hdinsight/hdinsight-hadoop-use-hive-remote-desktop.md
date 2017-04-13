<properties
   pageTitle="Hadoopi taru ja kaugtöölaua kasutamine Hdinsightiga | Microsoft Azure'i"
   description="Saate teada, kuidas ühenduse loomine Hadoopi kobar rakenduses Hdinsightiga kaugtöölaua ja seejärel käivitage taru päringute abil taru käsurea liides."
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
   ms.date="09/06/2016"
   ms.author="larryfr"/>

# <a name="use-hive-with-hadoop-on-hdinsight-with-remote-desktop"></a>Kaugtöölaua Hdinsightiga Hadoopi taru kasutamine

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

Selles artiklis on siit saate teada, kuidas luua ühenduse mõne Hdinsightiga kobar kaugtöölaua ja käivitage taru päringute abil taru käsurea liides (CLI).

> [AZURE.NOTE] Selle dokumendi ei paku HiveQL laused, mis on näidetes kasutatud teha üksikasjalikku kirjeldust. HiveQL, mida selles näites kasutatakse kohta leiate teavet teemast [Kasutamine taru Hadoopi Hdinsightiga abil](hdinsight-use-hive.md).

##<a id="prereq"></a>Eeltingimused

Selles artiklis toodud juhiseid tegemiseks on vaja järgmist:

* Windowsi-põhiste Hdinsightiga (Hadoopi Hdinsightiga) kobar

* Kliendi arvutis, kus töötab Windows 10, akna 8 või Windows 7

##<a id="connect"></a>Kaugtöölaua kasutajaga

Luba kaugtöölaud Hdinsightiga kobar, siis selle ühenduse juures [Hdinsightiga kogumite abil RDP ühenduse](hdinsight-administer-use-management-portal.md#rdp)juhiste järgi.

##<a id="hive"></a>Käsuga taru

Kui teil on ühendatud lauaarvuti jaoks Hdinsightiga kobar, siis tehke järgmist taru töötamiseks:

1. **Hadoopi käsurea**alustamiseks töölaualt Hdinsightiga.

2. Sisestage taru CLI käivitage järgmine käsk:

        %hive_home%\bin\hive

    Kui CLI käivitas, kuvatakse viip taru CLI: `hive>`.

3. Sisestage CLI kasutades nimega **log4jLogs** Näidisandmete abil uue tabeli loomiseks järgmistest:

        set hive.execution.engine=tez;
        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasbs:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    Need teatised järgmisi toiminguid:

    * **Tabeli LANGEV**: kustutab tabeli ja andmefaili, kui tabel on juba olemas.

    * **Välise tabeli loomine**: loob uue tabeli "väline" taru. Välise tabeleid säilitada tabeli määratlus taru (andmed on jäänud algsesse asukohta).

        > [AZURE.NOTE] Kui eeldate alusandmete tuleb värskendada välise andmeallikaga (nt automaatne andmete üleslaadimine) või mõni muu MapReduce toiming tuleb kasutada välise tabeleid, kuid tahate alati taru päringute kasutada uusimaid andmeid.
        >
        > Kas pukseerimine on Välistabel **ei** Kustuta andmed tabeli määratlus.

    * **Rea vorming**: taru saate teada, kuidas andmed on vormindatud. Sel juhul iga log väljad on eraldatud ruumi.

    * **Salvestatud AS TEKSTIFAILIDE asukoht**: ütleb taru, kus andmed on salvestatud (nt/andmete kataloogi) ja tekstina talletatud.

    * **Valige**: valib kõik read, kus veerus **t4** sisaldab väärtust **[ERROR]**arv. See peaks tagastama väärtus **3** , sest seal on kolm rida, mis sisaldavad seda väärtust.

    * **INPUT__FILE__NAME nagu "%.log"** - kirjeldatakse, mida me ainult tagasi andmete failidest, mille lõpus on taru. log. See piirab otsingu sample.log faili, mis sisaldab andmeid, ja tagastab andmed muu näiteks andmefailid, mis ei vasta meil määratletud skeemiga hoiab.


4. Uue nimega **errorLogs**"sisemine" tabeli loomiseks kasutada järgmistest:

        CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
        INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';

    Need teatised järgmisi toiminguid:

    * **Looge tabel kui pole olemas**: loob tabeli, kui see pole juba olemas. Kuna **väline** märksõna ei kasutata, see on sisemine tabel, mis on talletatud taru andmebaas ja hallatakse täielikult taru.

        > [AZURE.NOTE] Erinevalt **välise** tabeleid, pukseerimine on sisemine tabel kustutatakse ka aluseks olevad andmed.

    * **Salvestatud AS ORC**: talletatakse andmeid optimeeritud rea veeruformaadis (ORC). See on väga optimeeritud ja tõhusa vorming taru andmete talletamiseks.

    * **Lisa kirjutada... Valige**: valitakse tabelist **log4jLogs** sisaldavate ridade **[ERROR]**, seejärel lisab andmed tabelisse **errorLogs** .

    Veenduge, et ainult veeru t4 **[ERROR]** sisaldavate ridade talletatakse **errorLogs** tabelisse, abil järgmine lause tulu **errorLogs**kõiki ridu.

        SELECT * from errorLogs;

    Kolm rida andmete tuleks tagastada, kõik sisaldavad **[ERROR]** veeru t4.

##<a id="summary"></a>Kokkuvõte

Nagu näete, et taru käsu abil on lihtne interaktiivseks taru päringute käitamist on Hdinsightiga kobar, töö oleku jälgimine ja tuua väljund.

##<a id="nextsteps"></a>Järgmised sammud

Üldist teavet rakenduses Hdinsightiga taru kohta:

* [Hadoopi Hdinsightiga taru kasutamine](hdinsight-use-hive.md)

Teavet teiste mooduste kohta saate töötada Hadoopi Hdinsightiga:

* [Kasutage siga Hadoopi Hdinsightiga](hdinsight-use-pig.md)

* [Hadoopi Hdinsightiga MapReduce kasutamine](hdinsight-use-mapreduce.md)

Kui kasutate Tez taru, leiate teavet silumine järgmised dokumendid.

* [Windowsi-põhiste Hdinsightiga Tez UI kasutamine](hdinsight-debug-tez-ui.md)

* [Kasutage Linux-põhine Hdinsightiga Ambari Tez vaade](hdinsight-debug-ambari-tez-view.md)

[1]: ../HDInsight/hdinsight-hadoop-visual-studio-tools-get-started.md

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





[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md


[Powershell-install-configure]: ../powershell-install-configure.md
[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx

