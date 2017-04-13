<properties
   pageTitle="Kopeerige andmed ja sealt WASB Lake andmesalve Distcp abil | Microsoft Azure'i"
   description="Andmete kopeerimisel ja sealt Azure'i salvestusruumi plekid Lake andmesalve Distcp tööriista abil"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/28/2016"
   ms.author="nitinme"/>

# <a name="use-distcp-to-copy-data-between-azure-storage-blobs-and-data-lake-store"></a>Azure'i salvestusruumi plekid Lake andmesalve andmete kopeerimiseks kasutada Distcp

> [AZURE.SELECTOR]
- [DistCp abil](data-lake-store-copy-data-wasb-distcp.md)
- [AdlCopy abil](data-lake-store-copy-data-azure-storage-blob.md)


Kui olete loonud Hdinsightiga kobar, kellel on juurdepääs Lake andmesalve konto, saate Hadoopi ökosüsteemi tööriistad nagu Distcp andmed kopeerida **ja sealt** Hdinsightiga kobar salvestusruumi (WASB) Lake andmesalve kontole. Selles artiklis antakse juhiseid kohta, kuidas saavutada.

##<a name="prerequisites"></a>Eeltingimused

Enne alustamist selles artiklis, peab teil olema järgmised:

- **An Azure'i tellimus**. Leiate [Azure'i saada tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/).
- **Luba Azure tellimuse** Lake andmesalve avaliku eelvaade. Vaadake [juhiseid](data-lake-store-get-started-portal.md#signup).
- **Windows azure Hdinsightiga kobar** Lake andmesalve kontole. Vaadake teemat [mõne Hdinsightiga kobar andmete Lake poe loomine](data-lake-store-hdinsight-hadoop-use-portal.md). Veenduge, et kaugtöölaua lubamine klaster.

## <a name="do-you-learn-fast-with-videos"></a>Kas saate teada, videote abil kiiresti?

[Vaadake seda videot](https://mix.office.com/watch/1liuojvdx6sie) kohta, kuidas kopeerida andmeid Azure salvestusruumi plekid ning Lake andmesalve DistCp abil.

## <a name="use-distcp-from-remote-desktop-windows-cluster-or-ssh-linux-cluster"></a>Kasutage Distcp kaugtöölaua (Windowsi klaster) või SSH (Linux klaster)

Mõne Hdinsightiga kobar kaasas Distcp kasuliku, mida saate kasutada eri allikatest pärit andmete kopeerimine mõnda Hdinsightiga kobar. Kui olete konfigureerinud Hdinsightiga kobar kasutamiseks Lake andmesalve soovitud täiendav salvestusruum, saab Distcp kasuliku kasutatud välja-ja--box andmete kopeerimiseks ja sealt ka Lake andmesalve konto. Selles jaotises vaatame Distcp kasuliku kasutamise kohta.

1. Kui teil on Windows kobar, remote arvesse ka Hdinsightiga kobar mis on juurdepääs Lake andmesalve kontole. Juhised leiate teemast [kogumite abil RDP ühenduse loomine](../hdinsight/hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp). Avage töölaua kobar Hadoopi käsurea.

    Kui teil on Linux kobar, kasutage SSH ühenduse klaster. Vt [ühenduse loomine Linux-põhine Hdinsightiga kobar](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md#connect-to-a-linux-based-hdinsight-cluster). Käivitage käsud SSH küsimus.

3. Kontrollige, kas saate luua Azure'i salvestusruumi plekid (WASB). Käivitage järgmine käsk:

        hdfs dfs –ls wasb://<container_name>@<storage_account_name>.blob.core.windows.net/

    See peaks andma salvestusruumi bloobimälu sisukorda.

4. Samamoodi, kontrollige, kas pääsete Lake andmesalve konto klaster. Käivitage järgmine käsk:

        hdfs dfs -ls adl://<data_lake_store_account>.azuredatalakestore.net:443/

    See peaks andma failide ja kaustade Lake andmesalve konto loendit.

5. Kasutage Distcp WASB andmete kopeerimine Lake andmesalve konto.

        hadoop distcp wasb://<container_name>@<storage_account_name>.blob.core.windows.net/example/data/gutenberg adl://<data_lake_store_account>.azuredatalakestore.net:443/myfolder

    See **** kopeerib/näide/andmete/Gutenbergi/kausta sisu sisse WASB **/myfolder** Lake andmesalve konto.

6. Samuti Lake andmesalve konto andmete kopeerimine WASB Distcp abil.

        hadoop distcp adl://<data_lake_store_account>.azuredatalakestore.net:443/myfolder wasb://<container_name>@<storage_account_name>.blob.core.windows.net/example/data/gutenberg

    See sisu **/myfolder** kopeerib Lake andmesalve **** konto/näide/andmete/Gutenbergi/WASB kausta.

## <a name="see-also"></a>Vt ka

- [Azure'i salvestusruumi plekid andmete kopeerimine andmesalve Lake](data-lake-store-copy-data-azure-storage-blob.md)
- [Turvaline andmete andmesalve Lake](data-lake-store-secure-data.md)
- [Azure'i andmeanalüüsi Lake Lake andmesalve kasutamine](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Lake andmesalve Azure Hdinsightiga kasutamine](data-lake-store-hdinsight-hadoop-use-portal.md)
