<properties
    pageTitle="Kasutada interaktiivsete taru Hdinsightiga sisse | Microsoft Azure'i"
    description="Saate teada, kuidas kasutada interaktiivsete taru (taru LLAP sisse) sisse Hdinsightile."
    keywords=""
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian" 
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/27/2016"
    ms.author="jgao"/>


# <a name="use-interactive-hive-in-hdinsight-preview"></a>Kasutada interaktiivsete taru sisse Hdinsightiga (eelvaade)

Interaktiivne taru (alias [Reaalajas pikk ja protsessi]( https://cwiki.apache.org/confluence/display/Hive/LLAP)) on Hdinsightiga [kobar tüüp]( hdinsight-hadoop-provision-linux-clusters.md#cluster-types).  Interaktiivne taru võimaldab mälu vahemällu, mis toodab taru päringute palju kiirem ja interaktiivsed. Uus funktsioon teeb Hdinsightiga ühte maailma kõige kiire, paindlik, ja avatud Big Data lahenduse pilve mälu vahemälu (kasutades taru ja säde) ja täiustatud analytics sügav integreerimine R teenuste kaudu. 

Interaktiivne taru kobar erineb Hadoopi kobar. See sisaldab ainult taru teenus. 

> [AZURE.NOTE] MapReduce, siga, Sqoop, Oozie ja muude teenuste eemaldatakse seda tüüpi kobar varsti.
Interaktiivne taru klaster taru teenuse pääseb ainult selle Ambari vaade, Linnulennutee, ja taru ODBC. Ei pääse taru konsooli Templeton, Azure'i CLI ja Azure PowerShelli kaudu. 


 


## <a name="create-an-interactive-hive-cluster"></a>Saate luua ka interaktiivsed taru kobar

Interaktiivne taru kobar on toetatud ainult Linuxi-põhiste kogumite. Hdinsightiga kogumite loomise kohta leiate teavet teemast [loomine Linux-põhine Hadoopi kogumite Hdinsightiga sisse](hdinsight-hadoop-provision-linux-clusters.md).


## <a name="execute-hive-from-interactive-hive"></a>Käivitada taru interaktiivsed taru kaudu

On erinevaid võimalusi, kuidas saate käivitada taru päringuid:

- Käivitage taru Ambari taru vaate kasutamine

    Vaate taru kasutamise kohta leiate artiklist [taru koos Hadoopi rakenduses Hdinsightiga vaate kasutamine]( hdinsight-hadoop-use-hive-ambari-view.md).

- Käivitage taru Linnulennutee abil

    Linnulennutee kasutamine Hdinsightiga kohta leiate teavet teemast [Kasutamine taru koos Hadoopi Linnulennutee Hdinsightiga sisse](hdinsight-hadoop-use-hive-beeline.md).

    Kasutage funktsiooni headnode või on tühi serva sõlm Linnulennutee.  On soovitatav kasutada mõnda tühja serva sõlme Linnulennutee.  Mõnda tühja edgenode koos mõne Hdinsightiga kobar loomise kohta leiate teemast [kasutamine tühja serva sõlmed Hdinsightiga](hdinsight-apps-use-edge-node.md).

- Käivitage taru taru ODBC abil

    ODBC taru kasutamise kohta leiate artiklist [Exceli ühendamine Hadoopi koos Microsoft taru ODBC-draiver](hdinsight-connect-excel-hive-odbc-driver.md).

**Ühendusstringi JDBC leidmiseks tehke järgmist.**

1.  Logige Ambari abil järgmine URL: https://<ClusterName>. AzureHDInsight.net.
2.  Klõpsake vasakpoolses menüüs **taru** .
3.  Klõpsake esiletõstetud ikooni kopeerige URL:

    ![Hdinsightiga Hadoopi interaktiivsed taru LLAP JDBC](./media/hdinsight-hadoop-use-interactive-hive/hdinsight-hadoop-use-interactive-hive-jdbc.png)

## <a name="see-also"></a>Vt ka
-   [Hadoopi loomine Linux-põhine kogumite sisse Hdinsightile](hdinsight-hadoop-provision-linux-clusters.md): saate teada, kuidas luua interaktiivne taru kogumite Hdinsightiga.
-   [Kasutada koos Hadoopi rakenduses Hdinsightiga Linnulennutee taru](hdinsight-hadoop-use-hive-beeline.md): saate teada, kuidas kasutada Linnulennutee esitada taru päringud.
-   [Hadoopi koos Microsoft taru ODBC-draiver Exceli ühendamine](hdinsight-connect-excel-hive-odbc-driver.md): saate teada, kuidas ühendada Exceli taru.
