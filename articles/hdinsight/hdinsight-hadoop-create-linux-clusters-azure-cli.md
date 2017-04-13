<properties
    pageTitle="Hadoopi, HBase või torm kogumite loomine rakenduses abil CLI platvormidel Azure Hdinsightiga Linux | Microsoft Azure'i"
    description="Saate teada, kuidas luua Linux-põhine Hdinsightiga kogumite platvormidel Azure'i CLI, Azure'i ressursihaldur Mallid ja Azure REST API kasutamine. Saate määrata kobar tüüp (Hadoopi, HBase või Storm) või skriptide abil saate installida kohandatud komponendid."
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
    ms.date="09/20/2016"
    ms.author="larryfr"/>

#<a name="create-linux-based-clusters-in-hdinsight-using-the-azure-cli"></a>Luua Linux-põhine kogumite abil Azure CLI Hdinsightiga

[AZURE.INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

Azure'i CLI on mitu platvormi käsurea kasuliku, mis võimaldab teil hallata Azure teenused. Seda saab kasutada, koos Azure ressursside haldamise malle, luua Hdinsightiga kobar, koos seotud salvestusruumi kontod ja muud teenused.

Azure'i ressursihalduse Mallid on JSON dokumendid, mis kirjeldavad __Ressursirühma__ ja seda kõik ressursid (nt Hdinsightiga.) Selle malli lähenemine võimaldab määratleda ressursid, et peate Hdinsightiga ühe malli. Samuti saate __juurutuste__, mille muudatused rakendada kogu rühmale kogu muutuste rühma haldamine.

Juhised selle dokumendi tutvustavad uus Hdinsightiga klaster Azure'i CLI ja malli abil loomise protsess.

> [AZURE.IMPORTANT] Juhised selle dokumendi kasutamiseks on Hdinsightiga kobar vaikearvu töötaja sõlmed (4). Kui plaanite rohkem kui 32 töötaja sõlmed (kobar loomise ajal või skaala klaster), siis peate valima pea sõlme suurus koos vähemalt 8 14 GB RAM-i ja -vormid.
>
> Sõlm suurused ja seotud kulude kohta leiate lisateavet teemast [Hdinsightiga hinnad](https://azure.microsoft.com/pricing/details/hdinsight/).

##<a name="prerequisites"></a>Eeltingimused

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

- **An Azure'i tellimus**. Leiate [Azure'i saada tasuta prooviversioon](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- __Azure'i CLI__. Juhised selle dokumendi olid viimati testitud Azure'i CLI 0.10.1 versioon.

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)] 


### <a name="access-control-requirements"></a>Accessi kontrolli nõuded

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

##<a name="log-in-to-your-azure-subscription"></a>Azure'i tellimuse sisse logida

Järgige dokumenteerida [ühenduse Azure tellimuse: Azure'i käsurea liides (Azure'i CLI)](../xplat-cli-connect.md) ja ühendage tellimuse meetodil __Logi sisse__ .

##<a name="create-a-cluster"></a>Klaster loomine

Käsuviiba, shell või terminal seansi tuleks teha järgmised toimingud pärast installimist ja konfigureerimist Azure'i CLI.

1. Kasutage autentimiseks Azure tellimuse järgmine käsk:

        azure login

    Teil palutakse sisestada oma nimi ja parool. Kui teil on mitu Azure tellimust, kasutage `azure account set <subscriptionname>` seadmiseks tellimus, mida kasutada Azure CLI käsud.

3. Lülitumine Azure'i ressursihaldur abil järgmine käsk:

        azure config mode arm

4. Ressursi rühma loomine. Selle ressursirühma sisaldab Hdinsightiga kobar ja seotud salvestusruumi konto.

        azure group create groupname location
        
    * Asendage __groupname__ rühma kordumatu nimi. 
    * Asendage __asukoha__ geograafilised piirkond, mida soovite rühma luua. 
    
        Loendi kehtiv asukohad, kasutage funktsiooni `azure location list` käsk ja seejärel kasutage ühte asukohad veeru __nimi__ .

5. Looge konto salvestusruumi. Selle konto salvestusruumi kasutatakse vaikimisi salvestamise Hdinsightiga kobar.

        azure storage account create -g groupname --sku-name RAGRS -l location --kind Storage storagename
        
     * Asendage __groupname__ eelmises etapis loodud rühma nime.
     * Asendage __asukoht__ eelmises etapis kasutatud samasse asukohta. 
     * Asendage __storagename__ salvestusruumi konto kordumatu nimi.
     
     > [AZURE.NOTE] Lisateavet parameetrite abil, kasutatakse selle käsu kasutamine `azure storage account create -h` vaadata selle käsu spikker.

5. Saate tuua kasutatud salvestusruumi kontole juurde pääseda võti.

        azure storage account keys list -g groupname storagename
        
    * Asendage __groupname__ ressursi rühma nime.
    * Asendage __storagename__ salvestusruumi konto nimi.
    
    Tagastatavad andmed salvestada __võti1__ __võti__ väärtus.

6. Saate luua ka Hdinsightiga kobar.

        azure hdinsight cluster create -g groupname -l location -y Linux --clusterType Hadoop --defaultStorageAccountName storagename.blob.core.windows.net --defaultStorageAccountKey storagekey --defaultStorageContainer clustername --workerNodeCount 2 --userName admin --password httppassword --sshUserName sshuser --sshPassword sshuserpassword clustername

    * Asendage __groupname__ ressursi rühma nime.

    * Asendage __Hadoopi__ kobar tüüp, mida soovite luua. Näiteks `Hadoop`, `HBase`, `Storm` või `Spark`.

        > [AZURE.IMPORTANT] Hdinsightiga kogumite tulevad erinevaid töökoormus või klaster on häälestatud, mis vastavad tüüpi. Ei ole toetatud meetodit klaster, mis ühendab mitut tüüpi, näiteks torm ja HBase sisse ühe kobar loomiseks. 

    * Asendage __asukoht__ eelmisi toiminguid kasutatakse samasse asukohta.

    * Asendage __storagename__ salvestusruumikonto nimi.

    * Asendage __storagekey__ eelmises etapis saadud võti. 

    * Jaoks soovitud `--defaultStorageContainer` parameeter, kasutage sama nimi, mida kasutate klaster.

    * Asendage __administraator__ ja __httppassword__ kasutajanimi ja parool, mida soovite kasutada, kui klaster HTTPS-i kaudu.

    * Asendage __sshuser__ ja __sshuserpassword__ kasutajanimi ja parool, mida soovite kasutada, kui klaster SSH abil

    Võib kuluda mitu minutit kobar loomise protsessi lõpuleviimiseks. Tavaliselt umbes 15.

##<a name="next-steps"></a>Järgmised sammud

Nüüd, kui olete loonud Hdinsightiga kobar, mis Azure'i CLI abil, kasutage järgmist saate teada, kuidas töötada klaster:

###<a name="hadoop-clusters"></a>Hadoopi kogumite

* [Hdinsightiga taru kasutamine](hdinsight-use-hive.md)
* [Kasutage siga Hdinsightiga](hdinsight-use-pig.md)
* [Hdinsightiga MapReduce kasutamine](hdinsight-use-mapreduce.md)

###<a name="hbase-clusters"></a>HBase kogumite

* [Klõpsake Hdinsightiga HBase kasutamise alustamine](hdinsight-hbase-tutorial-get-started-linux.md)
* [Arendamise kohta Hdinsightiga HBase taotlused](hdinsight-hbase-build-java-maven-linux.md)

###<a name="storm-clusters"></a>Torm kogumite

* [Java topoloogiatest arendamise Storm Hdinsightiga kohta](hdinsight-storm-develop-java-topology.md)
* [Kasutage Python komponendid Storm Hdinsightiga kohta](hdinsight-storm-develop-python-topology.md)
* [Juurutada ja kontrollida topoloogiatest torm Hdinsightiga kohta](hdinsight-storm-deploy-monitor-topology-linux.md)
