<properties
    pageTitle="Luua HBase kogumite virtuaalse võrgus | Microsoft Azure'i"
    description="Alustamine Windows Azure Hdinsightiga HBase kasutamine. Saate teada, kuidas luua Hdinsightiga HBase kogumite Azure virtuaalse võrgus."
    keywords=""
    services="hdinsight,virtual-network"
    documentationCenter=""
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/18/2016"
   ms.author="jgao"/>

# <a name="create-hbase-clusters-in-azure-virtual-network"></a>Azure'i virtuaalse võrgu HBase kogumite loomine 

Saate teada, kuidas luua Azure Hdinsightiga HBase kogumite [Azure virtuaalse võrgu][1].

Virtuaalse võrgu integreerimise, saate HBase kogumite kasutusele võtta rakenduste virtuaalse samasse võrku nii, et rakendused ei saa suhelda HBase otse. Järgmised eelised.

- Otsest ühenduvuse HBase kobar, mis võimaldab HBase Java kaugprotseduurikutse kaudu suhtlemine sõlmi veebirakenduse kõne (RPC) API-d.
- Täiustatud jõudlus, jättes oma liikluse minna üle mitme lüüside ja koormus soolise.
- Võimalus tundliku teabe töötlemine turvalisemad viisil võtmata avaliku lõpp-punkti.

###<a name="prerequisites"></a>Eeltingimused
Enne alustamist selles õpetuses, peab teil olema järgmised:

- **An Azure'i tellimus**. Leiate [Azure'i saada tasuta prooviversioon](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- **Töökoha Azure PowerShelli abil**. Lugege [installi ja Azure PowerShelli kasutamine](https://azure.microsoft.com/documentation/videos/install-and-use-azure-powershell/). 

## <a name="create-hbase-cluster-into-virtual-network"></a>HBase kobar virtuaalse võrku loomine

Selles jaotises loote Linux-põhine HBase kobar sõltuvad Azure Storage konto Azure virtuaalse võrku, mis on [Azure ressursihaldur malli](../resource-group-template-deploy.md)abil. Muud kobar loomise võimalused ja mõistmine vt [loomine Hdinsightiga kogumite](hdinsight-hadoop-provision-linux-clusters.md). Malli loomine Hadoopi kogumite Hdinsightiga kasutamise kohta leiate lisateavet teemast [loomine Hadoopi kogumite Hdinsightiga Azure ressursihaldur mallide kasutamine rakenduses](hdinsight-hadoop-create-windows-clusters-arm-templates.md)

> [AZURE.NOTE] Osa atribuute on raske koodiga malli. Näiteks:
>
> * __Asukoht__: Ida-USA 2
> * __Cluster versioon: 3.4
> * __Kobar töötaja sõlm arv__: 4
> * __Vaikimisi salvestusruumi konto__: &lt;kobar nimi > talletamine
> * __Virtuaalne võrgu nimi__: &lt;kobar nimi >-vnet
> * __Virtuaalne võrgu aadressiruumi__: 10.0.0.0/16
> * __Alamvõrgu nimi__: vaikimisi
> * __Alamvõrgu aadress vahemiku__: 10.0.0.0/24
>
> &lt;Klaster nimi > asendada kobar nime saate sisestada malli abil.

1. Klõpsake malli avamiseks Azure'i portaalis järgmisel pildil. Mall asub avaliku bloobimälu ümbrises. 

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-hbase-cluster-in-vnet.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. **Kohandatud juurutus** keelest, sisestage järgmine:

    - **Tellimus**: valige tellimuse Azure Hdinsightiga kobar, sõltuvad salvestusruumi konto ja Azure virtuaalse võrgu loomiseks.
    - **Ressursirühm**: nuppu **Loo uus**ja määrake uue ressursi rühma nime.
    - **Asukoht**: valige asukoht ressursirühma.
    - **ClusterName**: sisestage Hadoopi kobar loodava nimi.
    - **Kobar kasutajanime ja parooli**: login vaikenimi on **administraator**.
    - **SSH kasutajanimi ja parool**: vaikimisi kasutajanimi on **sshuser**.  Saate selle ümber nimetada. 
    - **Nõustun tingimustel ja eespool määratletud tingimustele**: (valige)
    

3. Klõpsake nuppu **osta**. Kulub umbes 20 minutit klaster loomiseks. Kui klaster on loodud, saate klõpsata kobar tera portaalis selle avamiseks.

Pärast lõpetamist õpetuse, mida soovite kustutada klaster. Hdinsightiga, kus teie andmed on salvestatud Azure Storage, nii, et saate turvaline kustutada klaster, kui seda ei kasuta. Saate ka ostmisega on Hdinsightiga kobar isegi siis, kui seda ei kasuta. Kuna kulude klaster jaoks on mitu korda rohkem mäluruumi kui, on mõistlik economic kustutamiseks kogumite, kui nad ei kasuta. Kustutamine klaster juhised leiate teemast [haldamine Hadoopi kogumite Hdinsightiga, kasutades Azure portaali sisse](hdinsight-administer-use-management-portal.md#delete-clusters).

Uue HBase klaster töötamise alustamiseks saate toiminguid leitud [HBase Hadoopi rakenduses Hdinsightiga koos kasutamise alustamine](hdinsight-hbase-tutorial-get-started.md).

## <a name="connect-to-the-hbase-cluster-using-hbase-java-rpc-apis"></a>Ühenduse loomine HBase klaster HBase Java RPC API-de kasutamine

1.  Looge infrastruktuuri teenus (IaaS) virtuaalse masina sama Azure virtuaalse võrgu ja sama alamvõrgu. Uus IaaS virtuaalse masina loomise juhised leiate teemast [loomine virtuaalse masina opsüsteemi Windows Server](../virtual-machines/virtual-machines-windows-hero-tutorial.md). Kui juhistele seda dokumenti, peate kasutama võrgukonfiguratsioon järgmist:

    - __Virtuaalne võrgu__: &lt;kobar nimi >-vnet
    - __Alamvõrgu__: vaikimisi

    > [AZURE.IMPORTANT] Asendage &lt;kobar nimi > nimega saate kasutada, kui loote Hdinsightiga kobar eelmiste juhiste järgi.

    Nende väärtuste järgi tuleb konfigureerida virtuaalse masina kasutamiseks sama virtuaalse võrgu ja alamvõrgu Hdinsightiga kobar. See võimaldab need otse omavahel suhelda.

2.  Java rakenduste kasutamisel HBase kaugühenduse teel ühenduse peate kasutama täielik domeeninimi (FQDN). Selle määramiseks peavad teil olema kindla ühenduse DNS-i järelliite HBase kobar. Selleks, et saate kasutada ühte järgmistest:

    * Veebibrauseri abil Ambari tegemine.
    
        Liikuge sirvides https://&lt;ClusterName >.azurehdinsight.net/api/v1/clusters/&lt;ClusterName > / hosts?minimal_response = true. See muutuks JSON faili järelliidetega DNS-i.

    * Kasutage veebisaidil Ambari.

        1. Liikuge sirvides https://&lt;ClusterName >. azurehdinsight.net.
        2. Klõpsake **Hosts** ülalt menüüst.

    * ÜLEJÄÄNUD kõnede tegemiseks kasutada Curl:

            curl -u <username>:<password> -k https://<clustername>.azurehdinsight.net/ambari/api/v1/clusters/<clustername>.azurehdinsight.net/services/hbase/components/hbrest

        JavaScript Object märke (JSON) andmeid tagastada, leida "host_name". See sisaldab FQDN sõlmed klaster. Näiteks:

            ...
            "host_name": "wordkernode0.<clustername>.b1.cloudapp.net
            ...

        See osa kobar nimega domeeni nime alguses on DNS-i järelliite. Näiteks mycluster.b1.cloudapp.net.

    * Azure'i PowerShelli kasutamine
    
        Järgmine Azure PowerShelli skripti abil registreerumine funktsiooni **Get-ClusterDetail** , mida saab kasutada DNS-i järelliite tagastamiseks.

            function Get-ClusterDetail(
                [String]
                [Parameter( Position=0, Mandatory=$true )]
                $ClusterDnsName,
                [String]
                [Parameter( Position=1, Mandatory=$true )]
                $Username,
                [String]
                [Parameter( Position=2, Mandatory=$true )]
                $Password,
                [String]
                [Parameter( Position=3, Mandatory=$true )]
                $PropertyName
                )
            {
            <#
                .SYNOPSIS
                 Displays information to facilitate an HDInsight cluster-to-cluster scenario within the same virtual network.
                .Description
                 This command shows the following 4 properties of an HDInsight cluster:
                 1. ZookeeperQuorum (supports only HBase type cluster)
                    Shows the value of HBase property "hbase.zookeeper.quorum".
                 2. ZookeeperClientPort (supports only HBase type cluster)
                    Shows the value of HBase property "hbase.zookeeper.property.clientPort".
                 3. HBaseRestServers (supports only HBase type cluster)
                    Shows a list of host FQDNs that run the HBase REST server.
                 4. FQDNSuffix (supports all cluster types)
                    Shows the FQDN suffix of hosts in the cluster.
                .EXAMPLE
                 Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName ZookeeperQuorum
                 This command shows the value of HBase property "hbase.zookeeper.quorum".
                .EXAMPLE
                 Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName ZookeeperClientPort
                 This command shows the value of HBase property "hbase.zookeeper.property.clientPort".
                .EXAMPLE
                 Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName HBaseRestServers
                 This command shows a list of host FQDNs that run the HBase REST server.
                .EXAMPLE
                 Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName FQDNSuffix
                 This command shows the FQDN suffix of hosts in the cluster.
            #>

                $DnsSuffix = ".azurehdinsight.net"

                $ClusterFQDN = $ClusterDnsName + $DnsSuffix
                $webclient = new-object System.Net.WebClient
                $webclient.Credentials = new-object System.Net.NetworkCredential($Username, $Password)

                if($PropertyName -eq "ZookeeperQuorum")
                {
                    $Url = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/configurations?type=hbase-site&tag=default&fields=items/properties/hbase.zookeeper.quorum"
                    $Response = $webclient.DownloadString($Url)
                    $JsonObject = $Response | ConvertFrom-Json
                    Write-host $JsonObject.items[0].properties.'hbase.zookeeper.quorum'
                }
                if($PropertyName -eq "ZookeeperClientPort")
                {
                    $Url = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/configurations?type=hbase-site&tag=default&fields=items/properties/hbase.zookeeper.property.clientPort"
                    $Response = $webclient.DownloadString($Url)
                    $JsonObject = $Response | ConvertFrom-Json
                    Write-host $JsonObject.items[0].properties.'hbase.zookeeper.property.clientPort'
                }
                if($PropertyName -eq "HBaseRestServers")
                {
                    $Url1 = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/configurations?type=hbase-site&tag=default&fields=items/properties/hbase.rest.port"
                    $Response1 = $webclient.DownloadString($Url1)
                    $JsonObject1 = $Response1 | ConvertFrom-Json
                    $PortNumber = $JsonObject1.items[0].properties.'hbase.rest.port'

                    $Url2 = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/services/hbase/components/hbrest"
                    $Response2 = $webclient.DownloadString($Url2)
                    $JsonObject2 = $Response2 | ConvertFrom-Json
                    foreach ($host_component in $JsonObject2.host_components)
                    {
                        $ConnectionString = $host_component.HostRoles.host_name + ":" + $PortNumber
                        Write-host $ConnectionString
                    }
                }
                if($PropertyName -eq "FQDNSuffix")
                {
                    $Url = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/services/YARN/components/RESOURCEMANAGER"
                    $Response = $webclient.DownloadString($Url)
                    $JsonObject = $Response | ConvertFrom-Json
                    $FQDN = $JsonObject.host_components[0].HostRoles.host_name
                    $pos = $FQDN.IndexOf(".")
                    $Suffix = $FQDN.Substring($pos + 1)
                    Write-host $Suffix
                }
            }

        Pärast opsüsteemi Azure PowerShelli skripti, kasutage järgmist käsku DNS-i järelliite, kasutades funktsiooni **Get-ClusterDetail** tagastamiseks. Määrake oma Hdinsightiga HBase kobar nimi, administraatori nimi ja administraatori parooli selle käsu kasutamisel.

            Get-ClusterDetail -ClusterDnsName <yourclustername> -PropertyName FQDNSuffix -Username <clusteradmin> -Password <clusteradminpassword>

        Tagastab sufiks DNS-i. Näiteks **yourclustername.b4.internal.cloudapp.net**.

    * Kasutage RDP
    
        Samuti saate kaugtöölaua ühenduse HBase klaster (teil ühendatakse pea sõlme) ja saada DNS-i järelliite käsureale **ipconfig** käivitamine. Lubamine kaugtöölaua protokolli (RDP) ja kasutades RDP, klaster ühenduse juhised leiate teemast [haldamine Hadoopi kogumite sisse portaalis Azure Hdinsightiga][hdinsight-admin-portal].
        
        ![HDInsight.hbase.DNS.surffix][img-dns-surffix]


<!--
3.  Change the primary DNS suffix configuration of the virtual machine. This enables the virtual machine to automatically resolve the host name of the HBase cluster without explicit specification of the suffix. For example, the *workernode0* host name will be correctly resolved to workernode0 of the HBase cluster.

    To make the configuration change:

    1. RDP into the virtual machine.
    2. Open **Local Group Policy Editor**. The executable is gpedit.msc.
    3. Expand **Computer Configuration**, expand **Administrative Templates**, expand **Network**, and then click **DNS Client**.
    - Set **Primary DNS Suffix** to the value obtained in step 2:

        ![hdinsight.hbase.primary.dns.suffix][img-primary-dns-suffix]
    4. Click **OK**.
    5. Reboot the virtual machine.
-->

Veenduge, et virtuaalse masina saaksid suhelda HBase kobar, kasutage käsku `ping headnode0.<dns suffix>` virtual arvutist. Näiteks ping headnode0.mycluster.b1.cloudapp.net.

Java rakendus kasutab seda teavet, võite järgida juhiseid rakenduse loomiseks [Kasutada Maven luua Java rakendusi, mis kasutavad HBase koos Hdinsightiga (Hadoopi)](hdinsight-hbase-build-java-maven.md) . On rakenduse remote HBase serveriga ühendust luua, muuta **hbase-site.xml** faili selles näites Zookeeper FQDN kasutamiseks. Näiteks:

    <property>
        <name>hbase.zookeeper.quorum</name>
        <value>zookeeper0.<dns suffix>,zookeeper1.<dns suffix>,zookeeper2.<dns suffix></value>
    </property>

> [AZURE.NOTE] Nimelahendus Azure kohta lisateabe saamiseks vt virtuaalne võrkude, kuidas kasutada oma DNS-i server, sh [Nimi eraldusvõime (DNS)](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).

##<a name="next-steps"></a>Järgmised sammud

Selles õpetuses te teada, kuidas luua mõne HBase kobar. Lisateavet leiate järgmistest teemadest.

- [Alustamine Hdinsightiga](hdinsight-hadoop-linux-tutorial-get-started.md)
- [Hdinsightiga HBase dispersioonanalüüs konfigureerimine](hdinsight-hbase-geo-replication.md)
- [Loomine Hadoopi kogumite Hdinsightiga](hdinsight-provision-clusters.md)
- [Hadoopi rakenduses Hdinsightiga koos HBase kasutamise alustamine](hdinsight-hbase-tutorial-get-started.md)
- [Analüüsi Twitteri meeleolu koos HBase sisse Hdinsightiga](hdinsight-hbase-analyze-twitter-sentiment.md)
- [Virtuaalse võrgu ülevaade][vnet-overview]


[1]: http://azure.microsoft.com/services/virtual-network/
[2]: http://technet.microsoft.com/library/ee176961.aspx
[3]: http://technet.microsoft.com/library/hh847889.aspx

[hbase-get-started]: hdinsight-hbase-tutorial-get-started.md
[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md
[vnet-overview]: ../virtual-network/virtual-networks-overview.md
[vm-create]: ../virtual-machines/virtual-machines-windows-hero-tutorial.md

[azure-portal]: https://portal.azure.com
[azure-create-storageaccount]: ../storage-create-storage-account.md
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md#rdp

[hdinsight-powershell-reference]: https://msdn.microsoft.com/library/dn858087.aspx


[twitter-streaming-api]: https://dev.twitter.com/docs/streaming-apis
[twitter-statuses-filter]: https://dev.twitter.com/docs/api/1.1/post/statuses/filter


[powershell-install]: powershell-install-configure.md


[hdinsight-customize-cluster]: hdinsight-hadoop-customize-cluster.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-storage-powershell]: ../hdinsight-hadoop-use-blob-storage.md#powershell
[hdinsight-analyze-flight-delay-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md
[hdinsight-hive-odbc]: hdinsight-connect-excel-hive-ODBC-driver.md
[hdinsight-hbase-replication-dns]: hdinsight-hbase-geo-replication-configure-DNS.md

[img-dns-surffix]: ./media/hdinsight-hbase-provision-vnet/DNSSuffix.png
[img-primary-dns-suffix]: ./media/hdinsight-hbase-provision-vnet/PrimaryDNSSuffix.png
[img-provision-cluster-page1]: ./media/hdinsight-hbase-provision-vnet/hbasewizard1.png "Sätte üksikasjad uue HBase kobar"
[img-provision-cluster-page5]: ./media/hdinsight-hbase-provision-vnet/hbasewizard5.png "Skripti toimingu abil saate kohandada mõnda HBase kobar"

[azure-preview-portal]: https://portal.azure.com
