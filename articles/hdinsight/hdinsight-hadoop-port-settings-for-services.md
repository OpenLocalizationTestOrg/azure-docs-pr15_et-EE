<properties
pageTitle="Pordid kasutavad Hdinsightiga | Azure'i"
description="Hadoopi teenuseid Hdinsightiga kasutatavate portide loendit."
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
ms.date="10/03/2016"
ms.author="larryfr"/>

# <a name="ports-and-uris-used-by-hdinsight"></a>Pordid ja URI-d kasutavad Hdinsightiga

Selles dokumendis on toodud Linuxi-põhiste Hdinsightiga kogumite Hadoopi teenuseid kasutatavate portide loendit. See sisaldab ka teavet pordid ühendamiseks klaster SSH abil.

## <a name="public-ports-vs-non-public-ports"></a>Avaliku pordid vs – avaliku pordid

Linux-põhine Hdinsightiga kogumite seab ainult kolme pordid avalikult Internetis; 22, 23 ja 443. Neid kasutatakse turvaliseks juurdepääsuks klaster SSH ja teenuseid, mis on esitatud turvaline HTTPS-protokolli abil.

Sees, rakendatakse Hdinsightiga alusel mitu Azure'i Virtuaalmasinates (sõlmed kobar, sees) on Azure virtuaalse võrgu. Kaudu virtuaalse võrgustikus, pääsete Interneti kaudu avatud pordid. Näiteks kui loote ühenduse üks pea sõlmed SSH abil pea sõlme siis otse pääsete kobar sõlmed teenuseid.

> [AZURE.IMPORTANT] Kui loote mõnda Hdinsightiga kobar, kui te ei määra on Azure virtuaalse võrgu konfiguratsiooni valik, luuakse üks; Siiski ei saa seda automaatselt loodud virtuaalse võrgu liituda muud seadmed (nt muude Azure'i Virtuaalmasinates või kliendi arengu seadme). 

Liituda arvutite virtuaalse võrku, virtuaalse võrgu esmalt looma ja selle Hdinsightiga klaster loomisel määrata. Lisateabe saamiseks lugege teemat [laiendamine Hdinsightiga võimaluste on Azure virtuaalse võrgu kaudu](hdinsight-extend-hadoop-virtual-network.md)

## <a name="public-ports"></a>Avaliku pordid

Mõne Hdinsightiga kobar sõlme asuvad Azure virtuaalse võrgu ja ei pääse Interneti kaudu otse juurde. Avaliku lüüsi pakub järgmised pordid, mis on levinud üle kõik Hdinsightiga kobar tüüpi Interneti-ühendus.

| Teenus | Port | Protocol (protokoll) | Kirjeldus |
| ---- | ---------- | -------- | ----------- | ----------- |
| SSHD | 22 | SSH | Ühendab kliendid sshd esmane headnode kohta. Vt [kasutada SSH Linuxi-põhiste Hdinsightiga](hdinsight-hadoop-linux-use-ssh-windows.md) |
| SSHD | 22 | SSH | Ühendab kliendid sshd serva sõlme (ainult Hdinsightiga Premium). Vt [R serveris Hdinsightiga kasutamise alustamine](hdinsight-hadoop-r-server-get-started.md) |
| SSHD | 23 | SSH | Ühendab kliendid sshd teisene headnode kohta. Vt [kasutada SSH Linuxi-põhiste Hdinsightiga](hdinsight-hadoop-linux-use-ssh-windows.md) |
| Ambari | 443 | HTTPS | Ambari web UI. Vaadake [Hdinsightiga haldamine kasutamine Ambari Web UI](hdinsight-hadoop-manage-ambari.md) |
| Ambari | 443 | HTTPS | Ambari REST API-ga. Vaadake [Hdinsightiga haldamine kasutamine Ambari REST API -ga](hdinsight-hadoop-manage-ambari-rest-api.md) |
| WebHCat | 443 | HTTPS | HCatalog REST API-ga. Vaadake teemat [kasutada taru koos Curl](hdinsight-hadoop-use-pig-curl.md), [kasutada Curl siga](hdinsight-hadoop-use-pig-curl.md), [MapReduce Curl koos kasutamine](hdinsight-hadoop-use-mapreduce-curl.md) |
| HiveServer2 | 443 | ODBC | Loob ühenduse taru ODBC abil. Vt [Ühenduse loomine Exceli Hdinsightiga Microsoft ODBC-draiver](hdinsight-connect-excel-hive-odbc-driver.md). |
| HiveServer2 | 443 | JDBC | Loob ühenduse taru JDBC abil. Vt [ühenduse loomine taru kohta Hdinsightiga taru JDBC draiveri abil](hdinsight-connect-hive-jdbc-driver.md) |

Järgnevalt on saadaval kobar teatud tüüpi.

| Teenus | Port | Protocol (protokoll) |Kobar tüüp | Kirjeldus |
| ------------ | ---- |  ----------- | --- | ----------- |
| Tähevärav | 443 | HTTPS | HBase | HBase REST API-ga. Vt [HBase kasutamise alustamine](hdinsight-hbase-tutorial-get-started-linux.md) |
| Liviuse | 443 | HTTPS |  Säde | Säde REST API-ga. [Kaugkustutada Liviuse esitada säde projektide](hdinsight-apache-spark-livy-rest-interface.md) kuvamiseks |
| Torm | 443 | HTTPS | Torm | Torm web UI. Vt [Deploy ja hallata Storm topoloogiatest kohta Hdinsightiga](hdinsight-storm-deploy-monitor-topology-linux.md)

### <a name="authentication"></a>Autentimine

Kõigi teenuste internetis avalikult esitatud peavad olema kinnitatud:

| Port | Identimisteave |
| ---- | ----------- |
| 22 või 23 | SSH kasutajatunnust määratud kobar loomise ajal |
| 443 | Kasutajanime (vaikimisi: admin) ja parool, mida määratud kobar loomise ajal |

## <a name="non-public-ports"></a>Mitte-avalik pordid

> [AZURE.NOTE] Teatud teenuste saadaval ainult teatud kobar tüübid. Näiteks HBase on ainult saadaval HBase kobar puhul.

### <a name="hdfs-ports"></a>HDFS-pordid

| Teenus | Node(s) | Port | Protocol (protokoll) | Kirjeldus |
| ------- | ------- | ---- | -------- | ----------- | 
| NameNode web UI | Juhi sõlmed | 30070 | HTTPS | Praeguse oleku kuvamiseks kasutajaliides Web |
| NameNode metaandmete teenus | pea sõlmed | 8020 | IPC | Failisüsteemi metaandmete 
| DataNode | Kõik töötaja sõlmed | 30075 | HTTPS | Kasutajaliides Web oleku kuvamine, logid jne. |
| DataNode | Kõik töötaja sõlmed | 30010 | &nbsp; | Andmete edastamine |
| DataNode | Kõik töötaja sõlmed | 30020 | IPC | Metaandmete toimingud |
| Teisene NameNode | Juhi sõlmed | 50090 | HTTP KAUDU | Kontrollpunkt NameNode metaandmete jaoks |

### <a name="yarn-ports"></a>LÕNG pordid

| Teenus | Node(s) | Port | Protocol (protokoll) | Kirjeldus |
| ------- | ------- | ---- | -------- | ----------- |
| Ressursihaldur web UI | Juhi sõlmed | 8088 | HTTP KAUDU | Web Kasutajaliidese jaoks ressursihaldur |
| Ressursihaldur web Kasutajaliidese | Juhi sõlmed | 8090 | HTTPS | Web UI ressursihaldur jaoks |
| Ressursihaldur administraator kasutajaliidese | pea sõlmed | 8141 | IPC | Rakenduse kohta (taru taru server, siga, jne.) |
| Ressursihaldur ajasti | pea sõlmed | 8030 | HTTP KAUDU | Administreerimine |
| Ressursihaldur rakendustes | pea sõlmed | 8050 | HTTP KAUDU |Rakenduste haldur kasutajaliidese aadress |
| NodeManager | Kõik töötaja sõlmed | 30050 | &nbsp; | Container halduri aadress |
| NodeManager web Kasutajaliidese | Kõik töötaja sõlmed | 30060 | HTTP KAUDU | Ressursi halduri kasutajaliides |
| Ajaskaala aadress | Juhi sõlmed | 10200 | RPC | Ajaskaala teenus RPC. |
| Ajaskaala web UI | Juhi sõlmed | 8181 | HTTP KAUDU | Ajaskaala teenuse web UI |

### <a name="hive-ports"></a>Taru pordid

| Teenus | Node(s) | Port | Protocol (protokoll) | Kirjeldus |
| ------- | ------- | ---- | -------- | ----------- |
| HiveServer2 | Juhi sõlmed | 10001 | Ökonoomsus | Teenuse programmiliselt ühendamiseks taru (ökonoomsus/JDBC) |
| HiveServer | Juhi sõlmed | 10000 | Ökonoomsus | Teenuse programmiliselt ühendamiseks taru (ökonoomsus/JDBC) |
| Taru Metastore | Juhi sõlmed | 9083 | Ökonoomsus | Teenuse programmiliselt ühenduse taru metaandmete (ökonoomsus/JDBC) |

### <a name="webhcat-ports"></a>WebHCat pordid

| Teenus | Node(s) | Port | Protocol (protokoll) | Kirjeldus |
| ------- | ------- | ---- | -------- | ----------- |
| WebHCat server | Juhi sõlmed | 30111 | HTTP KAUDU | Veebi-API HCatalog ja muude teenuste Hadoopi peal |

### <a name="mapreduce-ports"></a>MapReduce pordid

| Teenus | Node(s) | Port | Protocol (protokoll) | Kirjeldus |
| ------- | ------- | ---- | -------- | ----------- |
| JobHistory | Juhi sõlmed | 19888 | HTTP KAUDU | MapReduce JobHistory web UI |
| JobHistory | Juhi sõlmed | 10020 | &nbsp; | MapReduce JobHistory server |
| ShuffleHandler | &nbsp; | 13562 | &nbsp; | Edastamine vahe kaardi väljundid taotlemise pärast abil |

### <a name="oozie"></a>Oozie

| Teenus | Node(s) | Port | Protocol (protokoll) | Kirjeldus |
| ------- | ------- | ---- | -------- | ----------- |
| Oozie server | Juhi sõlmed | 11000 | HTTP KAUDU | Oozie teenuse URL-i |
| Oozie server | Juhi sõlmed | 11001 | HTTP KAUDU | Pordi Oozie administraator |

### <a name="ambari-metrics"></a>Ambari mõõdikud

| Teenus | Node(s) | Port | Protocol (protokoll) | Kirjeldus |
| ------- | ------- | ---- | -------- | ----------- |
| Ajaskaala (ajalugu) | Juhi sõlmed | 6188 | HTTP KAUDU | Ajaskaala teenuse web UI |
| Ajaskaala (ajalugu) | Juhi sõlmed | 30200 | RPC | Ajaskaala teenuse web UI |

### <a name="hbase-ports"></a>HBase pordid

| Teenus | Node(s) | Port | Protocol (protokoll) | Kirjeldus |
| ------- | ------- | ---- | -------- | ----------- |
| HMaster | Juhi sõlmed | 16000 | &nbsp; | &nbsp; |
| HMaster teave Web UI | Juhi sõlmed | 16010 | HTTP KAUDU | Pordi HBase juhtslaidi Web UI |
| Piirkonna server | Kõik töötaja sõlmed | 16020 | &nbsp; | &nbsp; |
| &nbsp; | &nbsp; | 2181 | &nbsp; | Pordi ühenduse ZooKeeper kasutavate klientide |

### <a name="kafka-ports"></a>Kafka pordid

| Teenus | Node(s) | Port | Protocol (protokoll) | Kirjeldus |
| ------- | ------- | ---- | -------- | ----------- |
| Ta  | Töötaja sõlmed | 9092 | [Kafka kaabel Protocol (protokoll)](http://kafka.apache.org/protocol.html) | Kasutatakse kliendi suhtlemine |
| &nbsp; | Zookeeper sõlmed | 2181 | &nbsp; | Pordi ühenduse Zookeeper kasutavate klientide |
