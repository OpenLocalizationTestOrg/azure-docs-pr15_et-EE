<properties
    pageTitle="Hadoopi kogumite abil Azure'i CLI haldamine | Microsoft Azure'i"
    description="Azure'i CLI abil hallata Hadoopi le HDIsight"
    services="hdinsight"
    editor="cgronlun"
    manager="jhubbard"
    authors="mumian"
    tags="azure-portal"
    documentationCenter=""/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="jgao"/>

# <a name="manage-hadoop-clusters-in-hdinsight-using-the-azure-cli"></a>Hadoopi kogumite rakenduses abil Azure CLI Hdinsightiga haldamine

[AZURE.INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

Saate teada, kuidas [Azure'i käsurea kasutajaliidese](../xplat-cli-install.md) abil saate hallata Hadoopi kogumite Windows Azure Hdinsightiga sisse. Azure'i CLI rakendatakse Node.js. Seda saab kasutada mis tahes platvorm, mis toetab Node.js, sh Windowsi, Maci ja Linux.

Selles artiklis käsitletakse ainult Azure CLI Hdinsightiga abil. Üldised juhised, kuidas kasutada Azure CLI, lugege teemat [installida ja konfigureerida Azure CLI][azure-command-line-tools].

[AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

##<a name="prerequisites"></a>Eeltingimused

Enne alustamist selles artiklis, peab teil olema järgmised:

- **An Azure'i tellimus**. Leiate [Azure'i saada tasuta prooviversioon](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- **Azure'i CLI** - vt [installida ja konfigureerida Azure CLI](../xplat-cli-install.md) installi ja konfiguratsiooni teavet.
- **Azure'i ühenduse**, kasutades järgmine käsk:

        azure login

    Autentimisel töökoha või kooli kontoga kohta leiate lisateavet teemast [Azure tellimuse: Azure'i CLI ühenduse loomine](xplat-cli-connect.md).
    
- **Azure'i ressursihaldur režiimi aktiveerimine**, kasutades järgmine käsk:

        azure config mode arm

Abi saamiseks kasutage parameetrit **-h** .  Näiteks:

    azure hdinsight cluster create -h
    
##<a name="create-clusters"></a>Kogumite loomine

Vaadake [rakenduses abil Azure CLI Hdinsightiga kogumite loomine Linux-põhine](hdinsight-hadoop-create-linux-clusters-azure-cli.md).

##<a name="list-and-show-cluster-details"></a>Loendisse ja kobar üksikasjade kuvamine
Loendi ja kobar üksikasjade kuvamiseks saate kasutada järgmisi käske:

    azure hdinsight cluster list
    azure hdinsight cluster show <Cluster Name>

![HDI. CLIListCluster][image-cli-clusterlisting]


##<a name="delete-clusters"></a>Kogumite kustutamine

Järgmise käsu abil saate kustutada klaster:

    azure hdinsight cluster delete <Cluster Name>

Samuti saate kustutada klaster kustutades ressursirühm, mis sisaldab klaster. Pange tähele, et see kustutab kõik ressursid, sh salvestusruumi vaikekonto jaotises.

    azure group delete <Resource Group Name>

##<a name="scale-clusters"></a>Kogumite skaala

Hadoopi kobar suuruse muutmiseks tehke järgmist.

    azure hdinsight cluster resize [options] <clusterName> <Target Instance Count>


## <a name="enabledisable-http-access-for-a-cluster"></a>Lubada või keelata HTTP juurdepääsu klaster

    azure hdinsight cluster enable-http-access [options] <Cluster Name> <userName> <password>
    azure hdinsight cluster disable-http-access [options] <Cluster Name>

## <a name="enabledisable-rdp-access-for-a-cluster"></a>Lubada või keelata RDP juurdepääsu klaster

    azure hdinsight cluster enable-rdp-access [options] <Cluster Name> <rdpUserName> <rdpPassword> <rdpExpiryDate>
    azure hdinsight cluster disable-rdp-access [options] <Cluster Name>


##<a name="next-steps"></a>Järgmised sammud
Selles artiklis on õppinud erinevate Hdinsightiga kobar haldustoimingute sooritamiseks. Lisateabe saamiseks lugege järgmisi artikleid:

* [Azure'i portaal abil Hdinsightiga haldamine] [hdinsight-admin-portal]
* [Azure'i PowerShelli abil Hdinsightiga haldamine] [hdinsight-admin-powershell]
* [Alustamine Windows Azure Hdinsightiga] [hdinsight-get-started]
* [Kuidas kasutada Azure CLI] [azure-command-line-tools]


[azure-command-line-tools]: ../xplat-cli-install.md
[azure-create-storageaccount]: ../storage-create-storage-account.md
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/


[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md

[image-cli-account-download-import]: ./media/hdinsight-administer-use-command-line/HDI.CLIAccountDownloadImport.png
[image-cli-clustercreation]: ./media/hdinsight-administer-use-command-line/HDI.CLIClusterCreation.png
[image-cli-clustercreation-config]: ./media/hdinsight-administer-use-command-line/HDI.CLIClusterCreationConfig.png
[image-cli-clusterlisting]: ./media/hdinsight-administer-use-command-line/HDI.CLIListClusters.png "Loendi- ja Kuva kogumite"
