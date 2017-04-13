<properties
    pageTitle="Hadoopi kättesaadavus le Hdinsightiga | Microsoft Azure'i"
    description="Hdinsightiga kasutab väga saadaval ja usaldusväärne kogumite koos mõne täiendavad pea sõlme."
    services="hdinsight"
    tags="azure-portal"
    editor="cgronlun"
    manager="jhubbard"
    authors="mumian"
    documentationCenter=""/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/21/2016"
    ms.author="jgao"/>


#<a name="availability-and-reliability-of-windows-based-hadoop-clusters-in-hdinsight"></a>Kättesaadavus ja usaldusväärsus Windowsi-põhiste Hadoopi le Hdinsightiga


>[AZURE.NOTE] Windowsi-põhiste Hdinsightiga kogumite on selles dokumendis kasutatud juhiseid. Kui kasutate Linux-põhine kobar, leiate teavet [kättesaadavus ja usaldusväärsus Linux-põhine Hadoopi kogumite rakenduses Hdinsightiga](hdinsight-high-availability-linux.md) Linux kohased.

Hdinsightiga võimaldab juurutada eri kobar liiki erinevat tüüpi andmete analüüsi töökoormus. Kobar tüübid pakutakse täna on Hadoopi kogumite päringu ja analüüsi töökoormus, HBase kogumite NoSQL töökoormus ja Storm kogumite sündmuse töötlemiseks reaalajas töökoormus. Antud kobar tüüp, on erinevad sõlmed erinevad rollid. Näiteks:



- Hadoopi kogumite Hdinsightiga jaoks on juurutatud kaks rollid:
    - Juhi sõlm (2 sõlmed)
    - Andmete sõlm (1 sõlm)

- Kolm rollid on juurutatud HBase kogumite Hdinsightiga jaoks:
    - Juhi servers (2 sõlmed)
    - Piirkonna servers (1 sõlm)
    - Juhtslaidi/Zookeeper sõlmed (3 sõlmed)

- Kolm rollid on juurutatud torm kogumite Hdinsightiga jaoks:
    - Nimbus sõlmed (2 sõlmed)
    - Inspektori servers (1 sõlm)
    - Zookeeper sõlmed (3 sõlmed)

Hadoopi kogumite Standard rakendusi on tavaliselt ühe pea sõlme. Hdinsightiga eemaldatakse see tõrge ühe punkti teisene pea sõlme /head Lisaks serveri/Nimbus sõlm suurendamiseks kättesaadavus ja usaldusväärsus teenuse haldamiseks töökoormus vaja. Sõlmed pea sõlmed/head serverid/Nimbus on koostatud haldamiseks rikkumine töötaja sõlmed sujuvalt, kuid juhtslaidi pea sõlme töötavate teenuste katkestuste põhjustaks kobar ei ole enam töötada.


[ZooKeeper](http://zookeeper.apache.org/ ) sõlmed (ZKs) on lisatud ja kasutatakse pea sõlmed juhataja valimine ja kindlustada töötaja sõlmed ja lüüside (GWs) teada, millal nurjuda üle teisene pea sõlme (pea Node1) kui aktiivne pea sõlme (pea Node0) on passiivne.

![Skeem, mis on väga usaldusväärne pea sõlmed Hdinsightiga Hadoopi rakendamisel.](./media/hdinsight-high-availability/hadoop.high.availability.architecture.diagram.png)




## <a name="check-active-head-node-service-status"></a>Aktiivne pea sõlme teenuse oleku kontrollimine
Määrata, millised pea sõlme on aktiivne ja märkige ruut pea sõlme töötavate teenuste seisundi, tuleb ühendada Hadoopi klaster Kaugtöölaua protokolli (RDP) abil. RDP juhised leiate teemast [haldamine Hadoopi kogumite Hdinsightiga Azure portaali abil sisse](hdinsight-administer-use-management-portal.md#connect-to-hdinsight-clusters-by-using-rdp). Kui on ühendatud klaster, topeltklõpsake ikooni **Hadoopi saadaval** töölaual selle Namenode oleku kohta, millised pea sõlme hankimine, Jobtracker, Templeton, Oozieservice, Metastore ja Hiveserver2 teenused on töötab või HDI 3.0, Namenode, ressursihaldur, ajalugu Server, Templeton, Oozieservice, Metastore ja Hiveserver2 teenuste jaoks.

![](./media/hdinsight-high-availability/Hadoop.Service.Availability.Status.png)

Pildil, on aktiivne pea sõlme *headnode0*.

## <a name="access-log-files-on-the-secondary-head-node"></a>Accessi logifailid teisene pea sõlme

Töö juurdepääsu logid teisene pea sõlme juhuks, kui see on muutunud aktiivne pea sõlme sirvimise JobTracker UI töötab nagu see peamine aktiivse sõlme. JobTracker juurdepääsuks peate ühenduse Hadoopi kobar, kasutades eelmises jaotises kirjeldatud RDP. Kui olete rakendusse klaster kaugele viidud, topeltklõpsake ikooni **Hadoopi nimi sõlm olek** kuvatakse töölaual ja seejärel klõpsake **NameNode logide** avamiseks logid teisene pea sõlme register.

![](./media/hdinsight-high-availability/Hadoop.Head.Node.Log.Files.png)


## <a name="configure-head-node-size"></a>Pea sõlme suurus konfigureerimine
Pea sõlmed jagatakse vaikimisi suurte virtuaalmasinates (VMs). See on piisavalt klaster töötab enamik Hadoopi töö haldamiseks. Kuid on stsenaariumi, mis võivad nõuda ülisuure VMs pea sõlmed. Üks näide on klaster on suur hulk väike Oozie töö haldamiseks.

Ülisuure VMs saab konfigureerida Hdinsightiga SDK või Azure PowerShelli cmdlet-käskude abil.

Loomine ja Azure PowerShelli abil soovitud klaster ettevalmistamise on dokumenteerida [haldamine Hdinsightiga PowerShelli kaudu](hdinsight-administer-use-powershell.md). Konfiguratsiooni, mis ülisuure pea sõlme nõuab lisamine soovitud `-HeadNodeVMSize ExtraLarge` parameeter on `New-AzureRmHDInsightcluster` järgmine kood kasutada cmdlet-käsk.

    # Create a new HDInsight cluster in Azure PowerShell
    # Configured with an ExtraLarge head-node VM
    New-AzureRmHDInsightCluster `
                -ResourceGroupName $resourceGroupName `
                -ClusterName $clusterName ` 
                -Location $location `
                -HeadNodeVMSize ExtraLarge `
                -DefaultStorageAccountName "$storageAccountName.blob.core.windows.net" `
                -DefaultStorageAccountKey $storageAccountKey `
                -DefaultStorageContainerName $containerName  `
                -ClusterSizeInNodes $clusterNodes

Lugu SDK, sarnaneb. Loomine ja SDK abil soovitud klaster ettevalmistamise on dokumenteerida [Hdinsightiga .NET SDK abil](hdinsight-provision-clusters.md#sdk). Konfiguratsiooni, mis ülisuure pea sõlme nõuab lisamine soovitud `HeadNodeSize = NodeVMSize.ExtraLarge` parameeter on `ClusterCreateParameters()` meetodit kasutada järgmine kood.

    # Create a new HDInsight cluster with the HDInsight SDK
    # Configured with an ExtraLarge head-node VM
    ClusterCreateParameters clusterInfo = new ClusterCreateParameters()
    {
        Name = clustername,
        Location = location,
        HeadNodeSize = NodeVMSize.ExtraLarge,
        DefaultStorageAccountName = storageaccountname,
        DefaultStorageAccountKey = storageaccountkey,
        DefaultStorageContainer = containername,
        UserName = username,
        Password = password,
        ClusterSizeInNodes = clustersize
    };


## <a name="next-steps"></a>Järgmised sammud

- [Apache ZooKeeper](http://zookeeper.apache.org/ )
- [Ühenduse loomine Hdinsightiga kogumite RDP abil](hdinsight-administer-use-management-portal.md#rdp)
- [Hdinsightiga .NET SDK abil](hdinsight-provision-clusters.md#sdk)
