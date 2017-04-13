<properties
    pageTitle="Silumine ja analüüsida Hadoopi teenustega hunnik puistab | Microsoft Azure'i"
    description="Automaatselt koguda hunnik puistab Hadoopi teenuste ja asetada Azure'i bloobimälu salvestusruumi konto silumine ja analüüsida."
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
    ms.date="10/19/2016"
    ms.author="jgao"/>


# <a name="collect-heap-dumps-in-blob-storage-to-debug-and-analyze-hadoop-services"></a>Koguda hunnik puistab rakenduses silumine ja analüüsimiseks Hadoopi teenuste bloobimälu

[AZURE.INCLUDE [heapdump-selector](../../includes/hdinsight-selector-heap-dump.md)]

Hunnik puistab sisaldavad hetktõmmise rakenduse mälu, sealhulgas muutujate väärtusi prügimäele loomise ajal. Need on väga kasulik diagnoosimise probleemid, mis ilmnevad käitusajal. Hunnik puistab automaatselt kogutud Hadoopi teenuste ja paigutada Azure'i bloobimälu salvestusruumi konto jaotises HDInsightHeapDumps kasutaja /. 

Üksikute kogumite teenust peab olema lubatud hunnik puistab mitmesuguste kogum. Vaikimisi on see funktsioon on välja lülitatud klaster jaoks. Nende hunnik puistab võib olla suur, seega on soovitatav jälgida bloobimälu salvestusruumi konto, kus nad salvestatakse lubamisel kogumist.

> [AZURE.NOTE] Selle artikli teave kehtib ainult Windowsi-põhiste Hdinsightiga. Linux-põhine Hdinsightiga kohta lisateabe saamiseks vt [hunnik puistab Hadoopi Hdinsightiga Linuxi-põhiste teenuste lubamine](hdinsight-hadoop-collect-debug-heap-dump-linux.md)

## <a name="eligible-services-for-heap-dumps"></a>Õigus teenuste jaoks hunnik puistab

Saate lubada hunnik puistab teenuse.

*  **hcatalog** - tempelton
*  **taru** - hiveserver2, metastore, derbyserver
*  **mapreduce** - jobhistoryserver
*  **lõng** - resourcemanager, nodemanager, timelineserver
*  **hdfs** - datanode, secondarynamenode, namenode

## <a name="configuration-elements-that-enable-heap-dumps"></a>Konfiguratsiooni elemente, mis hunnik puistab lubamine

Hunnik puistab teenuse sisselülitamiseks peate määramiseks vastavat konfiguratsiooni elementide selle teenuse, **service_name**määratud sektsioon.

    "javaargs.<service_name>.XX:+HeapDumpOnOutOfMemoryError" = "-XX:+HeapDumpOnOutOfMemoryError",
    "javaargs.<service_name>.XX:HeapDumpPath" = "-XX:HeapDumpPath=c:\Dumps\<service_name>_%date:~4,2%%date:~7,2%%date:~10,2%%time:~0,2%%time:~3,2%%time:~6,2%.hprof"

**Service_name** väärtus võib olla mis tahes ülaltoodud teenustest: tempelton, hiveserver2, metastore, derbyserver, jobhistoryserver, resourcemanager, nodemanager, timelineserver, datanode, secondarynamenode, või namenode.

## <a name="enable-using-azure-powershell"></a>Luba Azure PowerShelli abil

Näiteks hunnik puistab sisselülitamiseks jobhistoryserver Azure PowerShelli abil saate oleks tehke järgmist.

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

    $MapRedConfigValues = new-object 'Microsoft.WindowsAzure.Management.HDInsight.Cmdlet.DataObjects.AzureHDInsightMapReduceConfiguration'

    $MapRedConfigValues.Configuration = @{ "javaargs.jobhistoryserver.XX:+HeapDumpOnOutOfMemoryError"="-XX:+HeapDumpOnOutOfMemoryError" ; "javaargs.jobhistoryserver.XX:HeapDumpPath" = "-XX:HeapDumpPath=c:\\Dumps\\jobhistoryserver_%date:~4,2%_%date:~7,2%_%date:~10,2%_%time:~0,2%_%time:~3,2%_%time:~6,2%.hprof" }

## <a name="enable-using-net-sdk"></a>Kasutades .NET SDK lubamine

Näiteks hunnik puistab sisselülitamiseks jobhistoryserver Azure Hdinsightiga .NET SDK abil saate oleks tehke järgmist.

    clusterInfo.MapReduceConfiguration.ConfigurationCollection.Add(new KeyValuePair<string, string>("javaargs.jobhistoryserver.XX:+HeapDumpOnOutOfMemoryError", "-XX:+HeapDumpOnOutOfMemoryError"));

    clusterInfo.MapReduceConfiguration.ConfigurationCollection.Add(new KeyValuePair<string, string>("javaargs.jobhistoryserver.XX:HeapDumpPath", "-XX:HeapDumpPath=c:\\Dumps\\jobhistoryserver_%date:~4,2%_%date:~7,2%_%date:~10,2%_%time:~0,2%_%time:~3,2%_%time:~6,2%.hprof"));
