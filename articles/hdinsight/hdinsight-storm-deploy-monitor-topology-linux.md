<properties
   pageTitle="Juurutada ja hallata Apache Storm topoloogiatest kohta Linux-põhine Hdinsightiga | Microsoft Azure'i"
   description="Saate teada, kuidas juurutada, jälgimine ja haldamine Apache Storm topoloogiatest Storm armatuurlaua kasutamine Linux-põhine Hdinsightiga. Hadoopi tööriistu saate kasutada Visual Studio."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="java"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/07/2016"
   ms.author="larryfr"/>

# <a name="deploy-and-manage-apache-storm-topologies-on-linux-based-hdinsight"></a>Juurutada ja hallata Apache Storm topoloogiatest Linux-põhine Hdinsightiga kohta

Selles dokumendis Õpi juhtimiseks ja Storm topoloogiatest töötab Linux-põhine Storm Hdinsightiga kogumite kohta.

> [AZURE.IMPORTANT] Selles artiklis toodud juhiseid vaja Linuxi-põhiste torm Hdinsightiga kobar kohta. Juurutamine ja jälgimise topoloogiatest Windowsi-põhiste Hdinsightiga kohta leiate teemast [Deploy ja hallata Apache Storm topoloogiatest sisse Windowsi-põhiste Hdinsightiga](hdinsight-storm-deploy-monitor-topology.md)

## <a name="prerequisites"></a>Eeltingimused

* **Klõpsake Hdinsightiga kobar Storm A Linux-põhine**: [Alustamine Apache Storm Hdinsightiga kohta](hdinsight-apache-storm-tutorial-get-started-linux.md) leiate juhised luua klaster

* **Tuttav SSH ja SCP**: SSH ja SCP Hdinsightiga kasutamise kohta lisateabe saamiseks vaadake järgmist:

    * **Linux, Unix või OS X kliendid**: vt [Kasutamine SSH koos Linux-põhine Hadoopi Linux, OS X või Unix Hdinsightiga](hdinsight-hadoop-linux-use-ssh-unix.md)

    * **Windowsi kliendid**: vt [Kasutamine SSH koos Linux-põhine Hadoopi Hdinsightiga Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

* **An SCP kliendi**: see on esitatud kõik Linux, Unix ja OS X süsteemidega. Windowsi kliendid, soovitame PSCP, mis on saadaval [PuTTY alla laadida lehe](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)kaudu.

## <a name="start-a-storm-topology"></a>Käivitage Storm topoloogia

### <a name="using-ssh-and-the-storm-command"></a>SSH ja Storm käsu abil

1. Kasutage SSH ühenduse Hdinsightiga kobar. Asendage **kasutajanimi** on teie SSH login nime. Asendage **CLUSTERNAME** oma Hdinsightiga kobar nimega.

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    Ühenduse loomiseks Hdinsightiga klaster SSH kasutamise kohta lisateabe saamiseks vt järgmised dokumendid
    
    * **Linux, Unix või OS X kliendid**: vt [Kasutamine SSH koos Linux-põhine Hadoopi Linux, OS X või Unix Hdinsightiga](hdinsight-hadoop-linux-use-ssh-unix.md)
    
    * **Windowsi kliendid**: vt [Kasutamine SSH koos Linux-põhine Hadoopi Hdinsightiga Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

2. Kasutage alustamiseks on näide topoloogia järgmine käsk:

        storm jar /usr/hdp/current/storm-client/contrib/storm-starter/storm-starter-topologies-0.9.3.2.2.4.9-1.jar storm.starter.WordCountTopology WordCount

    See käivitab näide WordCount topoloogia klaster. CEIP Loo laused ja loendamiseks esinemiskord iga sõna lauses.

    > [AZURE.NOTE] Topoloogia klaster esitamisel tuleb esmalt kopeerida jar fail, mis sisaldab klaster enne selle `storm` käsk. Seda saab teha, kasutades funktsiooni `scp` käsk klient, kus fail on olemas. Näiteks`scp FILENAME.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:FILENAME.jar`
    >
    > Näide WordCount ja muude torm starter näited, on juba lisatud klaster veebisaidil `/usr/hdp/current/storm-client/contrib/storm-starter/`.

### <a name="programmatically"></a>Programmiliselt

Saate juurutada programmiliselt topoloogia Storm klõpsake Hdinsightiga suheldes Nimbus teenuse majutatud klaster. [https://github.com/Azure-samples/HDInsight-Java-Deploy-Storm-topology](https://github.com/Azure-Samples/hdinsight-java-deploy-storm-topology) hõlmab näiteks Java rakendus, mis näitab, kuidas juurutada ja käivitage topoloogia Nimbus teenuse kaudu.

## <a name="monitor-and-manage-using-the-storm-command"></a>Jälgimine ja haldamine torm käsu kasutamine

Funktsiooni `storm` kasuliku saate töötamise topoloogiatest käsurea kaudu. Järgmises loendis sagedamini kasutatavad käsud. Kasutage `storm -h` käsud täieliku loendi.

### <a name="list-topologies"></a>Loendi topoloogiatest

Kasutage järgmine käsk, et kõik töötab topoloogiatest.

    storm list
    
See tagastab teabe umbes järgmine:

    Topology_name        Status     Num_tasks  Num_workers  Uptime_secs
    -------------------------------------------------------------------
    WordCount            ACTIVE     29         2            263

### <a name="deactivate-and-reactivate"></a>Desaktiveerige ja uuesti aktiveerimine

Desaktiveerides topoloogia peatab seda seni, kuni see tappis või uuesti aktiveerida. Desaktiveerige ja uuesti kasutada järgmist.

    storm Deactivate TOPOLOGYNAME
    
    storm Activate TOPOLOGYNAME

### <a name="kill-a-running-topology"></a>Tappa töötava topoloogia

Torm topoloogiatest, üks kord alustamine, kasutab ka edaspidi töötab kuni peatamiseni. Kasutage topoloogia peatamiseks järgmine käsk:

    storm stop TOPOLOGYNAME

### <a name="rebalance"></a>Hooldus

Ümberkorraldamine topoloogia võimaldab süsteemi topoloogia paralleelsust muuta. Näiteks kui saate lisada rohkem märkmeid kobar nende suurust muuta, ümberkorraldamine võimaldab töötava topoloogia veenduge, et kasutada uut sõlmed.

> [AZURE.WARNING] Ümberkorraldamine topoloogia esmalt Desaktiveeri topoloogia, siis jaotab töötajate ühtlaselt üle klaster, siis lõpuks annab topoloogia sel oli enne ümberkorraldamine ilmnenud olek. Nii kui topoloogia on aktiivne, muutub aktiivne uuesti. Kui see on välja lülitatud, jääb see välja.

    storm rebalance TOPOLOGYNAME

## <a name="monitor-and-manage-using-the-storm-ui"></a>Jälgimine ja haldamine Storm Kasutajaliidese abil

Torm UI pakub web interface töötamiseks topoloogiatest töötab, ja lisatud Hdinsightiga klaster. Torm UI kuvamiseks veebibrauseris avamiseks kasutada __https://CLUSTERNAME.azurehdinsight.net/stormui__, kus __CLUSTERNAME__ on klaster nimi.

> [AZURE.NOTE] Kui palutakse sisestada kasutajanimi ja parool, sisestage kobar administraator (admin) ja parool, mida olete kasutanud, millal loomise klaster.


### <a name="main-page"></a>Avaleht

Põhilehe Storm UI pakub järgmist teavet:

* **Kobar Kokkuvõte**: torm kobar põhiteavet.

* **Topoloogia Kokkuvõte**: installitud topoloogiatest loend. Selle jaotise linkide abil vaadata täpsemat teavet teatud topoloogiatest.

* **Inspektori Kokkuvõte**: torm juhendajaga teavet.

* **Nimbus konfigureerimine**: Nimbus klaster konfigureerimine.

### <a name="topology-summary"></a>Topoloogia Kokkuvõte

Lingi valimine jaotises **topoloogia Kokkuvõte** kuvab topoloogia kohta järgmine teave:

* **Topoloogia Kokkuvõte**: topoloogia põhiteavet.

* **Topoloogia toimingud**: haldamise toiminguid, mida saate teha topoloogia.

  * **Aktiveeri**: desaktiveeritakse topoloogia elulookirjeldused töötlemine.

  * **Desaktiveeri**: peatab töötava topoloogia.

  * **Taastub**: reguleerib topoloogia paralleelsust. Töötava topoloogiatest peaks taastub, kui olete muutnud sõlmed klaster arv. See võimaldab topoloogia kohandada paralleelsus hüvitamine sõlmed klaster suurenemine või vähenemine arv.

    Lisateabe saamiseks lugege teemat <a href="http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html" target="_blank">Storm topoloogia paralleelsust mõistmine</a>.

  * **Tappa**: lõpeb määratud ajalõpu Storm topoloogia.

* **Topoloogia statistika**: topoloogia statistikat. **Akna** veerus linkide abil määrata ajavahemiku ülejäänud kirjed lehel.

* **Spouts**: otsikuid, mis kasutavad topoloogia. Selle jaotise linkide abil vaadata täpsemat teavet teatud otsikuid.

* **Poldid**: kasutatavaid topoloogia poldid. Selle jaotise linkide abil vaadata täpsemat teavet teatud poldid.

* **Topoloogia konfigureerimine**: valitud topoloogia konfigureerimine.

### <a name="spout-and-bolt-summary"></a>Tila ja polt Kokkuvõte

Valides tila **Spouts** või **poldid** jaotiste kuvab valitud üksuse kohta järgmine teave:

* **Komponendi Kokkuvõte**: põhiteavet tila või polt.

* **Tila/polt statistika**: statistikat tila või polt. **Akna** veerus linkide abil määrata ajavahemiku ülejäänud kirjed lehel.

* **Sisestusmeetodi statistika** (ainult polt): teavet sisendi voole, tarbitud polt.

* **Väljundi statistika**: voole, kiiratava seda teavet tila või polt.

* **Haldavad isikud**: teavet tila või polt eksemplarid. Valige **Port** kirje teatud Executor toodeti töövooeksemplari diagnostikateave Logi kuvamiseks.

* **Tõrgete**: tõrgete teave selle tila või polt.

## <a name="rest-api"></a>REST API-GA

Torm UI on ehitatud peal REST API-ga, et sarnaseid haldamise ja järelevalve funktsiooni REST API abil saate teha. REST API abil saate luua kohandatud tööriistade juhtimiseks ja Storm topoloogiatest.

Lisateabe saamiseks vt [Storm UI REST API -ga](http://storm.apache.org/releases/0.9.6/STORM-UI-REST-API.html). Teatud REST API kasutamine Apache Storm Hdinsightiga kohta on järgmine teave.

> [AZURE.IMPORTANT] Torm REST API ei ole avalikult kättesaadav Interneti kaudu ja pääseb juurde ka SSH tunneliga Hdinsightiga kobar pea sõlme abil. Loomine ja kasutamine on SSH tunneliga kohta leiate teavet teemast [kasutamine SSH Tunneling Ambari web UI, ResourceManager, JobHistory, NameNode, Oozie, ja muud web UI's juurde pääseda](hdinsight-linux-ambari-ssh-tunnel.md).

### <a name="base-uri"></a>Base URI

Base URI REST API Linux-põhine Hdinsightiga kogumite kohta on saadaval veebisaidil pea sõlme **https://HEADNODEFQDN:8744/api/v1/**; siiski pea sõlme domeeninimi on loodud kobar loomise ajal ja ei ole staatiline.

Kobar pea sõlme täielik domeeninimi (FQDN) leiate mitmel viisil:

* __Kaudu an SSH seansi__: käsuga `headnode -f` klaster SSH seanss.

* __Ambari veebist__: __teenuste__ lehe ülaosas valige __torm__. Valige vahekaardil __Kokkuvõte__ __Torm UI Server__. Lehe ülaosas on FQDN sõlm, mis torm Kasutajaliidese ja REST API töötab.

* __Ambari REST API__: käsuga `curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/STORM/components/STORM_UI_SERVER"` hankida teavet sõlm, mis torm Kasutajaliidese ja REST API töötavad. Asendage klaster administraatori parool __parool__ . Asendage __CLUSTERNAME__ kobar nimi. Vastuse, "host_name" kirje sisaldab sõlme FQDN.

### <a name="authentication"></a>Autentimine

REST API taotluste peate kasutama **elementaarautentimine**nii, et kasutate Hdinsightiga kobar administraatori nimi ja parool.

> [AZURE.NOTE] Kuna elementaarautentimine saadetakse avateksti abil, tuleks **alati** HTTPS suhtlemine klaster turvamiseks kasutamine.

### <a name="return-values"></a>Väärtuste tagastamiseks

Kasutatav kobar või virtuaalmasinates sama Azure virtuaalse võrgus klaster võib ainult teavet, mis on tagastatud REST API-ga. Näiteks kuvatakse täielik domeeninimi (FQDN) tagastatud jaoks Zookeeper serverid ei pääse Interneti kaudu.

## <a name="next-steps"></a>Järgmised sammud

Nüüd, kui olete õppinud, kuidas juurutada ja jälgida topoloogiatest Storm armatuurlaua abil, saate teada, kuidas [töötada Java-põhine topoloogiatest Maven abil](hdinsight-storm-develop-java-topology.md).

Veel näide topoloogiatest loendi leiate teemast [näide topoloogiatest Storm Hdinsightiga kohta](hdinsight-storm-example-topology.md).
