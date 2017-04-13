<properties
    pageTitle="Välja mälu tõrke (OOM) - taru sätted | Microsoft Azure'i"
    description="Parandamine Hadoopi rakenduses Hdinsightiga mälu tõrge (OOM) taru päringu kontorist väljas. Kliendi stsenaarium on palju suurte tabelite päringu."
    keywords="välja mälu viga, OOM, taru sätted"
    services="hdinsight"
    documentationCenter=""
    authors="rashimg"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="09/02/2016"
    ms.author="rashimg;jgao"/>

# <a name="fix-an-out-of-memory-oom-error-with-hive-memory-settings-in-hadoop-in-azure-hdinsight"></a>Tõrke välja mälu (OOM) taru mälu sätetega Hadoopi rakenduses Windows Azure Hdinsightiga

Üks levinumad probleemid meie klientide näo on kuvatakse tõrketeade välja mälu (OOM) abil taru. Selles artiklis kirjeldatakse kliendi stsenaarium ja taru sätted, soovitame selle probleemi lahendamiseks.

## <a name="scenario-hive-query-across-large-tables"></a>Stsenaarium: Taru päringu üle suurtele tabelitele

Kliendi parandusfunktsiooni päringu abil taru all.

    SELECT
        COUNT (T1.COLUMN1) as DisplayColumn1,
        …
        …
        ….
    FROM
        TABLE1 T1,
        TABLE2 T2,
        TABLE3 T3,
        TABLE5 T4,
        TABLE6 T5,
        TABLE7 T6
    where (T1.KEY1 = T2.KEY1….
        …
        …

Mõned nüansid päringu:

* T1 on suur tabelisse Tabel1, mis on palju STRINGI veerutüübid pseudonüümi.
* Muudes tabelites ei ole nii suur, kuid on suur veergude arv.
* Kõigi tabelite ühinevad, mõnel juhul Tabel1 jt mitme veeruga.

Kui kliendi parandusfunktsiooni taru kasutamine MapReduce 24 sõlme A3 kobar päringu, päringu parandusfunktsiooni 26 minutites. Kliendi märganud järgmised hoiatusteated, kui päring on MapReduce taru kasutamine:

    Warning: Map Join MAPJOIN[428][bigTable=?] in task 'Stage-21:MAPRED' is a cross product
    Warning: Shuffle Join JOIN[8][tables = [t1933775, t1932766]] in Stage 'Stage-4:MAPRED' is a cross product

Kuna päring on lõpule jõudnud, 26 minutites käivitamisel, kliendi ignoreeritakse neid hoiatused ja selle asemel alustada keskenduda kuidas parandada see päringu jõudluse täpsemaks.

Kliendi nõu [Hadoopi rakenduses Hdinsightiga optimeerimine taru päringuid](hdinsight-hadoop-optimize-hive-query.md)ja otsustanud kasutada mootori Tez täitmise. Kui sama päringu käivitamine koos selle Tez säte lubatud päringu käivitasite 15 minutit ja siis viskas järgmine tõrketeade:

    Status: Failed
    Vertex failed, vertexName=Map 5, vertexId=vertex_1443634917922_0008_1_05, diagnostics=[Task failed, taskId=task_1443634917922_0008_1_05_000006, diagnostics=[TaskAttempt 0 failed, info=[Error: Failure while running task:java.lang.RuntimeException: java.lang.OutOfMemoryError: Java heap space
        at
    org.apache.hadoop.hive.ql.exec.tez.TezProcessor.initializeAndRunProcessor(TezProcessor.java:172)
        at org.apache.hadoop.hive.ql.exec.tez.TezProcessor.run(TezProcessor.java:138)
        at
    org.apache.tez.runtime.LogicalIOProcessorRuntimeTask.run(LogicalIOProcessorRuntimeTask.java:324)
        at
    org.apache.tez.runtime.task.TezTaskRunner$TaskRunnerCallable$1.run(TezTaskRunner.java:176)
        at
    org.apache.tez.runtime.task.TezTaskRunner$TaskRunnerCallable$1.run(TezTaskRunner.java:168)
        at java.security.AccessController.doPrivileged(Native Method)
        at javax.security.auth.Subject.doAs(Subject.java:415)
        at org.apache.hadoop.security.UserGroupInformation.doAs(UserGroupInformation.java:1628)
        at
    org.apache.tez.runtime.task.TezTaskRunner$TaskRunnerCallable.call(TezTaskRunner.java:168)
        at
    org.apache.tez.runtime.task.TezTaskRunner$TaskRunnerCallable.call(TezTaskRunner.java:163)
        at java.util.concurrent.FutureTask.run(FutureTask.java:262)
        at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1145)
        at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:615)
        at java.lang.Thread.run(Thread.java:745)
    Caused by: java.lang.OutOfMemoryError: Java heap space

Kliendi seejärel otsustanud kasutada suurem VM (st D12) suurem VM mõtlete oleks hunnik rohkem ruumi. Kliendi jätkuvalt isegi siis, kuvatakse tõrketeade. Kliendi jõudnud Hdinsightiga meeskonna abi silumine probleemi.

## <a name="debug-the-out-of-memory-oom-error"></a>Mälu (OOM) välja tõrke silumine

Meie tugiteenuste ja tehnika meeskonnad koos on leidnud mõne välja mälu (OOM) tõrget probleeme oli [teadaolev probleem Apache JIRA kirjeldatud](https://issues.apache.org/jira/browse/HIVE-8306). Klõpsake soovitud JIRA kirjeldus:

    When hive.auto.convert.join.noconditionaltask = true we check noconditionaltask.size and if the sum  of tables sizes in the map join is less than noconditionaltask.size the plan would generate a Map join, the issue with this is that the calculation doesnt take into account the overhead introduced by different HashTable implementation as results if the sum of input sizes is smaller than the noconditionaltask size by a small margin queries will hit OOM.

Oleme kinnitanud, et selle **hive.auto.convert.join.noconditionaltask** tõepoolest oli määratud väärtusele **tõene** vaadates jaotises taru-site.xml faili:

    <property>
        <name>hive.auto.convert.join.noconditionaltask</name>
        <value>true</value>
        <description>
            Whether Hive enables the optimization about converting common join into mapjoin based on the input file size.
            If this parameter is on, and the sum of size for n-1 of the tables/partitions for a n-way join is smaller than the
            specified size, the join is directly converted to a mapjoin (there is no conditional task).
        </description>
    </property>

Hoiatus ja selle JIRA põhineva meie juhul oli kaardi liitumine Java hunnik ruumi OOM tõrke põhjuse. Nii, et me kaevas põhjalikult probleemi.

Ajaveebipostituse [Hadoopi lõng mälu sätete Hdinsightiga](http://blogs.msdn.com/b/shanyu/archive/2014/07/31/hadoop-yarn-memory-settings-in-hdinsigh.aspx)kirjeldatud Tez täitmise engine on kasutatud hunnik ruumi kasutamisel tegelikult kuulub Tez ümbrises. Vaadake allolevat kirjeldav Tez container mälu.

![Tez container mälu skeemi: välja mälu tõrke OOM taru](./media/hdinsight-hadoop-hive-out-of-memory-error-oom/hive-out-of-memory-error-oom-tez-container-memory.png)


Nagu ajaveebipostituse järgmised kaks mälu sätted määrata container hunnik: **hive.tez.container.size** ja **hive.tez.java.opts**. Meie kogemus OOM erand ei tähenda container maht on liiga väike. See tähendab, et Java kuhja maht (hive.tez.java.opts) on liiga väike. Nii, et iga kord, kui te ei näe OOM, proovige **hive.tez.java.opts**suurendamiseks. Vajaduse korral võib olla vaja **hive.tez.container.size**suurendada. Sätte **java.opts** peaks olema umbes 80% **container.size**.

> [AZURE.NOTE]  Sätte **hive.tez.java.opts** peab alati olema väiksem kui **hive.tez.container.size**.

Kuna D12 masina 28GB mälu, otsustasime kasutada container maht 10GB (10240MB) ja määrata java.opts 80%. Seda tehti taru konsooli alltoodud sätte abil:

    SET hive.tez.container.size=10240
    SET hive.tez.java.opts=-Xmx8192m

Nende sätete põhjal, päringu edukalt parandusfunktsiooni all kümme minutit.

## <a name="conclusion-oom-errors-and-container-size"></a>Kokkuvõte: OOM tõrgete ja container suurus

Kuvatakse tõrketeade OOM ei tähenda tingimata container maht on liiga väike. Selle asemel peaks mälu sätteid konfigureerida nii, et kuhja maht on suurem ja vähemalt 80% container mälu suurus.
