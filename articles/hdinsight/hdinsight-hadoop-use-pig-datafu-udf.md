<properties
pageTitle="Siga Hdinsightiga DataFu kasutamine"
description="DataFu on teekide jaoks Hadoopi kogum. Siit saate teada, kuidas saate kasutada DataFu koos siga Hdinsightiga klaster."
services="hdinsight"
documentationCenter=""
authors="Blackmist"
manager="jhubbard"
editor="cgronlun"/>

<tags
ms.service="hdinsight"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="big-data"
ms.date="08/23/2016"
ms.author="larryfr"/>

#<a name="use-datafu-with-pig-on-hdinsight"></a>Siga Hdinsightiga DataFu kasutamine

DataFu on avatud allika teekide jaoks Hadoopi kogum. Selles dokumendis saate teada klaster Hdinsightiga DataFu kasutamise kohta ja kuidas kasutada siga DataFu kasutaja määratletud funktsioonid (UDF).

##<a name="prerequisites"></a>Eeltingimused

* Azure'i tellimuse.

* Windows Azure Hdinsightiga kobar, (Linux või Windowsi põhine)

* Tavaline tuttava [siga Hdinsightiga](hdinsight-use-pig.md) abil

##<a name="install-datafu-on-linux-based-hdinsight"></a>Linux-põhine Hdinsightiga DataFu installimine

> [AZURE.NOTE] DataFu on installitud kogumite Linux-põhine versioon 3.3 ja suurema ja Windowsi-põhiste kogumite. See pole installitud Linux-põhine kogumite varem 3.3.
>
> Kui kasutate kobar Linux-põhine versioon 3.3 või uuem versioon või Windowsi-põhiste kobar, saate selle jaotise vahele jätta.

DataFu saate alla laadida ja installida Maven hoidla. Järgmiste juhiste abil saate lisada klaster Hdinsightiga DataFu:

1. Ühenduse Linux-põhine Hdinsightiga klaster SSH abil. Hdinsightiga SSH kasutamise kohta lisateabe saamiseks leiate järgmised dokumendid.

    * [Kasutada SSH Linux-põhine Hadoopi Hdinsightiga Linux, OS X ja Unix](hdinsight-hadoop-linux-use-ssh-unix.md)
    * [Kasutada SSH Linux-põhine Hadoopi Windows Hdinsightiga](hdinsight-hadoop-linux-use-ssh-unix.md)
    
2. Järgmise käsu abil saate wget kasuliku, kasutades DataFu jar faili alla laadida või kopeerige ja kleepige link brauseri allalaadimise käivitamiseks.

        wget http://central.maven.org/maven2/com/linkedin/datafu/datafu/1.2.0/datafu-1.2.0.jar

3. Järgmiseks faili üleslaadimiseks vaikimisi salvestusruum Hdinsightiga klaster. See muudab faili saadaval kõik sõlmed klaster ja isegi juhul, kui saate kustutada ja taastada klaster salvestusruumi jäävad faili.

        hdfs dfs -put datafu-1.2.0.jar /example/jars
    
    > [AZURE.NOTE] Eeltoodud näites talletab jar on `wasbs:///example/jars` , sest see kaust juba olemas kobar säilitamise kohta. Saate kasutada mis tahes asukohas soovite Hdinsightiga kobar säilitamise kohta.

##<a name="use-datafu-with-pig"></a>Siga DataFu kasutamine

Selle jaotise juhised endale tuttavaid siga kasutamine Hdinsightile, ja anda ainult siga Ladina laused, mitte kohta, kuidas neid kasutada koos klaster juhiseid. Siga Hdinsightiga kasutamise kohta leiate lisateavet teemast [Kasutamine siga Hdinsightiga](hdinsight-use-pig.md).

> [AZURE.IMPORTANT] DataFu sealt kasutamisel Linux-põhine Hdinsightiga kobar peate registreeruma jar-faili abil järgmine siga Ladina-lause:
>
> ```register wasbs:///example/jars/datafu-1.2.0.jar```
>
> Windowsi-põhiste Hdinsightiga kogumite vaikimisi on registreeritud DataFu.

Määratlege pseudonüümi DataFu funktsioonide tavaliselt. Näiteks:

    DEFINE SHA datafu.pig.hash.SHA();
    
See määratleb alias nimega `SHA` SHA, funktsioon räsi jaoks. Seejärel saate seda siga ladina kirjas räsi sisendandmete jaoks luua. Näiteks järgmine asendab nimed sisendandmete räsi väärtus:

    raw = LOAD '/data/raw/' USING PigStorage(',') AS  
        (name:chararray, 
        int1:int, 
        int2:int,
        int3:int); 
    mask = FOREACH raw GENERATE SHA(name), int1, int2, int3; 
    DUMP mask;

Kui seda kasutatakse Sisestuskeel järgmiste andmetega:

    Lana Zemljaric,5,9,1
    Qiong Zhong,9,3,6
    Sandor Harsanyi,0,7,3
    Roko Petkovic,2,6,2
    Tibor Rozsa,8,0,0
    Lea Hrastovsek,6,3,6
    Regina Toth,2,1,2
    Eva Makay,8,9,2
    Shi Liao,4,6,0
    Tjasa Zemljaric,0,2,5
    
See loob järgmine väljund:

    (c1a743b0f34d349cfc2ce00ef98369bdc3dba1565fec92b4159a9cd5de186347,5,9,1)
    (713d030d621ab69aa3737c8ea37a2c7c724a01cd0657a370e103d8cdecac6f99,9,3,6)
    (7a5f5abdd281f68168199319d98a1a662535f988d1443b3a3c497010937bac89,0,7,3)
    (a94818e93807e12079c4b35f8f3c8c8ef8e8acd1954e7f0476bc1a3a86fc96a9,2,6,2)
    (894ead4f48af91df7e088241218a23157bede7c52115272e417e95c046d48902,8,0,0)
    (6f99f163af3448fda672087db306f363e27a98a9e49c1f274a0860e303f8aec4,6,3,6)
    (a03de92a28be3c6a984c7a153fa9ed81c0413f76a9401955b5f7e04a5dd0ab9f,2,1,2)
    (6ceab977c8fb48d9ad0dc413e6bc646cabd89f22e7ab97a6b0133f3d225c6013,8,9,2)
    (fa9c436469096ff1bd297e182831f460501b826272ae97e921f5f6e3f54747e8,4,6,0)
    (bc22db7c238b86c37af79a62c78f61a304b35143f6087eb99c34040325865654,0,2,5)

##<a name="next-steps"></a>Järgmised sammud

Lisateavet DataFu või siga leiate järgmised dokumendid.

* [Apache DataFu siga juhend](http://datafu.incubator.apache.org/docs/datafu/guide.html).

* [Kasutage siga Hdinsightiga](hdinsight-use-pig.md)
