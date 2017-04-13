<properties
    pageTitle="Hadoopi kogumite Hdinsightiga Ambari API abil rakenduses jälgimine | Microsoft Azure'i"
    description="Kasutage Apache Ambari API-de loomine, haldamine ja Hadoopi kogumite jälgimine. Intuitiivne tehtemärk tööriistad ja API-de peitmiseks Hadoopi keerukusest."
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"
    editor="cgronlun"
    manager="jhubbard"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="jgao"/>

# <a name="monitor-hadoop-clusters-in-hdinsight-using-the-ambari-api"></a>Hadoopi kogumite sisse Hdinsightiga Ambari API abil jälgimine

Saate teada, kuidas jälgida Hdinsightiga kogumite Ambari API abil.

> [AZURE.NOTE] Selle artikli teave on peamiselt Windowsi-põhiste Hdinsightiga kogumite, mis pakuvad Ambari REST API kirjutuskaitstud versioon. Linux-põhine kogumite, leiate teemast [haldamine Hadoopi kogumite Ambari abil](hdinsight-hadoop-manage-ambari.md).

## <a name="what-is-ambari"></a>Mis on Ambari?

[Apache Ambari] [ ambari-home] kasutatakse ettevalmistamise, haldamine ja Apache Hadoop kogumite jälgimine. See sisaldab on intuitiivne saidikogumi tehtemärk tööriistade ja komplekti API-d, mis peitmine keerukus Hadoopi, lihtsustamine kogumite toimimist. Funktsiooni API-de kohta leiate lisateavet teemast [Ambari API viide][ambari-api-reference]. 

Hdinsightiga toetab praegu ainult Ambari järelevalve funktsiooni. Ambari API 1.0 toetab Hdinsightiga versiooni 3.0 ja 2.1 kogumite. Selles artiklis antakse ülevaade juurdepääsul Ambari API-de Hdinsightiga versioon 3,1 ja 2.1 kogumite kohta. Kahe võtme vahe on mõned komponendid on muutunud võtta kasutusele uued võimalused (nt töö ajalugu Server). 

**Eeltingimused**

Enne alustamist selles õpetuses, peab teil olema järgmised:

- **Töökoha Azure PowerShelli abil**.

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

- (Valikuline) [cURL] [curl]. Selle installida, lugege teemat [cURL väljalasete ja alla laaditavaid faile][curl-download].

    >[AZURE.NOTE] Kui käsuga cURL Windowsis kasutada jutumärkide asemel ühe jutumärgid suvand väärtused.

- **An Windows Azure Hdinsightiga kobar**. Juhised kobar ettevalmistamise kohta leiate teemast [Hdinsightiga kasutamise alustamine] [ hdinsight-get-started] või [sätte Hdinsightiga kogumite][hdinsight-provision]. Peate läbima õpetuse järgmised andmed:

    Atribuut kobar|Azure'i PowerShelli muutuja nimi|Väärtus|Kirjeldus
    ---|---|---|---
    Hdinsightiga kobar nimi|$clusterName||Klaster Hdinsightiga nimi.
    Kobar kasutajanimi|$clusterUsername||Kobar kasutajanimi määratud klaster loomise.
    Parooli kobar|$clusterPassword||Kobar kasutaja parooli.

    >[AZURE.NOTE] FILL-in väärtuste tabeli. See on abiks läbi selles õpetuses.

## <a name="jump-start"></a>Alusta liikumine

On mitu võimalust kasutada Ambari Hdinsightiga kogumite jälgida.

**Azure'i PowerShelli kasutamine**

Järgmine on mõni Azure PowerShelli skripti MapReduce töö jälgimine teabe saamiseks *mõne Hdinsightiga 3,1 klaster.*  Võtme erinevus on, et saaksime tõmmata need andmed on LÕNG teenuse (mitte MapReduce).

    $clusterName = "<HDInsightClusterName>"
    $clusterUsername = "<HDInsightClusterUsername>"
    $clusterPassword = "<HDInsightClusterPassword>"

    $ambariUri = "https://$clusterName.azurehdinsight.net:443/ambari"
    $uriJobTracker = "$ambariUri/api/v1/clusters/$clusterName.azurehdinsight.net/services/yarn/components/resourcemanager"

    $passwd = ConvertTo-SecureString $clusterPassword -AsPlainText -Force
    $creds = New-Object System.Management.Automation.PSCredential ($clusterUsername, $passwd)

    $response = Invoke-RestMethod -Method Get -Uri $uriJobTracker -Credential $creds -OutVariable $OozieServerStatus

    $response.metrics.'yarn.queueMetrics'

Mõne Azure PowerShelli skripti saamine on MapReduce töö jälgimine teave *on Hdinsightiga 2.1 klaster*on järgmine:

    $clusterName = "<HDInsightClusterName>"
    $clusterUsername = "<HDInsightClusterUsername>"
    $clusterPassword = "<HDInsightClusterPassword>"

    $ambariUri = "https://$clusterName.azurehdinsight.net:443/ambari"
    $uriJobTracker = "$ambariUri/api/v1/clusters/$clusterName.azurehdinsight.net/services/mapreduce/components/jobtracker"

    $passwd = ConvertTo-SecureString $clusterPassword -AsPlainText -Force
    $creds = New-Object System.Management.Automation.PSCredential ($clusterUsername, $passwd)

    $response = Invoke-RestMethod -Method Get -Uri $uriJobTracker -Credential $creds -OutVariable $OozieServerStatus

    $response.metrics.'mapred.JobTracker'

Väljund on:

![Jobtracker väljund][img-jobtracker-output]

**Kasutage cURL**

Järgmises näites saada kobar teabe cURL abil:

    curl -u <username>:<password> -k https://<ClusterName>.azurehdinsight.net:443/ambari/api/v1/clusters/<ClusterName>.azurehdinsight.net

Väljund on:

    {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/",
     "Clusters":{"cluster_name":"hdi0211v2.azurehdinsight.net","version":"2.1.3.0.432823"},
     "services"[
       {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/services/hdfs",
        "ServiceInfo":{"cluster_name":"hdi0211v2.azurehdinsight.net","service_name":"hdfs"}},
       {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/services/mapreduce",
        "ServiceInfo":{"cluster_name":"hdi0211v2.azurehdinsight.net","service_name":"mapreduce"}}],
     "hosts":[
       {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/hosts/headnode0",
        "Hosts":{"cluster_name":"hdi0211v2.azurehdinsight.net",
                 "host_name":"headnode0"}},
       {"href":"https://hdi0211v2.azurehdinsight.net/ambari/api/v1/clusters/hdi0211v2.azurehdinsight.net/hosts/workernode0",
        "Hosts":{"cluster_name":"hdi0211v2.azurehdinsight.net",
                 "host_name":"headnode0.{ClusterDNS}.azurehdinsight.net"}}]}

**10/8/2014 vabasta**:

Kui kasutate Ambari lõpp-punkti "https://{clusterDns}.azurehdinsight.net/ambari/api/v1/clusters/{clusterDns}.azurehdinsight.net/services/{servicename}/components/{componentname}" *host_name* välja annab sõlme asemel hostinimi täielik domeeninimi (FQDN). Selles näites tagastatakse enne 10/8/2014, lihtsalt "**headnode0**". Pärast 10/8/2014, saate FQDN "**headnode0. {} ClusterDNS} .azurehdinsight .net**", nagu on näidatud eelmises näites. See muudatus on hõlbustamiseks stsenaariumid, kus mitu kobar tüüpi (nt HBase ja Hadoopi) saab kasutada ühe virtuaalse võrgu (VNET). See juhtub näiteks, kui kasutada HBase tagaandmebaas Platvormi Hadoopi.

## <a name="ambari-monitoring-apis"></a>Ambari API-de jälgimine

Järgmises tabelis on loetletud mõned kõige levinum Ambari, jälgimise API kõned. API kohta leiate lisateavet teemast [Ambari API juhend][ambari-api-reference].

Kuvari API kõne|URI|Kirjeldus
---|---|---
Kogumite hankimine|`/api/v1/clusters`|
Saada kobar teave.|`/api/v1/clusters/<ClusterName>.azurehdinsight.net`|kogumite, teenused, hosts
Teenused|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services`|Teenuste hulka kuuluvad: hdfs, mapreduce
Saada teenuste teave.|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services/<ServiceName>`|
Saada teenuse komponendid|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services/<ServiceName>/components`|HDFS: namenode datanode<br/>MapReduce: jobtracker; tasktracker
Saada komponent teave.|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/services/<ServiceName>/components/<ComponentName>`|ServiceComponentInfo host-komponendid, mõõdikud
Hosts hankimine|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts`|headnode0 workernode0
Saada host teave.|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts/<HostName>`|
Hosti komponentide hankimine|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts/<HostName>/host_components`|namenode resourcemanager
Saada host komponent teave.|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/hosts/<HostName>/host_components/<ComponentName>`|HostRoles komponent, host, mõõdikud
Saada konfiguratsioone|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/configurations`|Config tüüpi: core – saidile, hdfs-saidi, mapred – saidile taru – saidile
Saada konfiguratsiooni teave.|`/api/v1/clusters/<ClusterName>.azurehdinsight.net/configurations?type=<ConfigType>&tag=<VersionName>`|Config tüüpi: core – saidile, hdfs-saidi, mapred – saidile taru – saidile


##<a name="next-steps"></a>Järgmised sammud

Nüüd olete õppinud jälgimise API kõned Ambari kasutamise kohta. Lisateavet leiate järgmistest teemadest.

- [Portaalis Azure Hdinsightiga kogumite haldamine][hdinsight-admin-portal]
- [Hdinsightiga kogumite Azure PowerShelli kaudu hallata][hdinsight-admin-powershell]
- [Käsurea liidest kasutades Hdinsightiga kogumite haldamine][hdinsight-admin-cli]
- [Hdinsightiga dokumentatsioon][hdinsight-documentation]
- [Alustamine Hdinsightiga][hdinsight-get-started]



[ambari-home]: http://ambari.apache.org/
[ambari-api-reference]: https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md

[curl]: http://curl.haxx.se
[curl-download]: http://curl.haxx.se/download.html

[microsoft-hadoop-SDK]: http://hadoopsdk.codeplex.com/wikipage?title=Ambari%20Monitoring%20Client

[powershell-install]: powershell-install-configure.md
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-admin-cli]: hdinsight-administer-use-command-line.md
[hdinsight-documentation]: /documentation/services/hdinsight/
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-provision]: hdinsight-provision-clusters.md

[img-jobtracker-output]: ./media/hdinsight-monitor-use-ambari-api/hdi.ambari.monitor.jobtracker.output.png
