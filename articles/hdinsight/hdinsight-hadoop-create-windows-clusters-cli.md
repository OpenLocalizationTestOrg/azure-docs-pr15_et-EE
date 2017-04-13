<properties
   pageTitle="Windowsi-põhiste Hadoopi kogumite loomine abil Azure CLI Hdinsightiga"
    description="Saate teada, kuidas luua kogumite Windows Azure Hdinsightiga Azure CLI abil."
   services="hdinsight"
   documentationCenter=""
   tags="azure-portal"
   authors="mumian"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/02/2016"
   ms.author="jgao"/>

# <a name="create-windows-based-hadoop-clusters-in-hdinsight-using-azure-cli"></a>Windowsi-põhiste Hadoopi kogumite loomine abil Azure CLI Hdinsightiga

[AZURE.INCLUDE [selector](../../includes/hdinsight-selector-create-clusters.md)]

Saate teada, kuidas luua abil Azure CLI Hdinsightiga kogumite. Muud kobar loomise tööriistade ja funktsioonide klõpsake selle lehe ülaosas valige menüü või näha [kobar loomise võimalused](hdinsight-provision-clusters.md#cluster-creation-methods).

##<a name="prerequisites"></a>Eeltingimused

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


Enne alustamist selles artiklis antud juhiseid, peab teil olema järgmised:

- **Azure'i tellimus**. Leiate [Azure'i saada tasuta prooviversioon](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- **Azure'i CLI**.

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)] 

### <a name="access-control-requirements"></a>Accessi kontrolli nõuded

[AZURE.INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

##<a name="connect-to-azure"></a>Azure'i ühendamine

Azure'i ühenduse loomiseks kasutada järgmine käsk:

    azure login

Autentimisel töökoha või kooli kontoga kohta leiate lisateavet teemast [Azure tellimuse: Azure'i CLI ühenduse loomine](../xplat-cli-connect.md).

Kasutage ARM lülitumine järgmine käsk:

    azure config mode arm

Abi saamiseks kasutage parameetrit **-h** .  Näiteks:

    azure hdinsight cluster create -h

##<a name="create-clusters"></a>Kogumite loomine

Enne kui saate luua ka Hdinsightiga kobar peab olema Azure'i Resource Management (ARM) ja Azure'i bloobimälu salvestusruumi konto. Mõne Hdinsightiga kobar loomiseks peate määrama järgmist:

- **Azure'i ressursirühm**: A Lake andmeanalüüsi konto tuleb luua Azure'i ressursirühm sees. Azure'i ressursihaldur võimaldab teil töö ressurssidega oma rakenduse rühmana. Saate juurutada, värskendamine või kustutada kõik ressursse rakenduse ühe koordineeritud kasutusel.

    Teie tellimus ressursi rühmade loendi:

        azure group list

    Uue ressursirühma loomiseks tehke järgmist.

        azure group create -n "<Resource Group Name>" -l "<Azure Location>"

- **Hdinsightiga kobar nimi**

- **Asukoht**: üks Azure andmekeskuste, mis toetab Hdinsightiga kogumite. Toetatud kohtade loendi leiate teemast [Hdinsightiga hinnad](https://azure.microsoft.com/pricing/details/hdinsight/).

- **Salvestusruumi vaikekonto**: Hdinsightiga kasutab mõnda Azure'i bloobimälu salvestusruumi container failisüsteemi. Azure Storage konto on nõutav, enne kui saate luua ka Hdinsightiga kobar.

    Azure'i salvestusruumi uue konto loomiseks tehke järgmist.

        azure storage account create "<Azure Storage Account Name>" -g "<Resource Group Name>" -l "<Azure Location>" --type LRS

    > [AZURE.NOTE]Salvestusruumi konto peab olema paikneb koos Hdinsightiga andmekeskuse.
    > Salvestusruumi konto tüüp ei saa ZRS, kuna ZRS ei toeta tabelis.

    Azure portaali kaudu Azure Storage konto loomise kohta leiate teemast [loomine, haldamine ja salvestusruumi konto kustutamine] [azure-loomine – storageaccount].

    Kui teil juba on salvestusruumi konto, kuid ei tea, kas konto nime ja konto võti, saate selle teabe toomiseks järgmised käsud:

        -- Lists Storage accounts
        azure storage account list
        -- Shows a Storage account
        azure storage account show "<Storage Account Name>"
        -- Lists the keys for a Storage account
        azure storage account keys list "<Storage Account Name>" -g "<Resource Group Name>"

    Saada teavet, kasutades Azure portaali kohta täpsema teabe saamiseks vt "Halda salvestusruumi konto" osa [kohta Azure salvestusruumi kontod](../storage/storage-create-storage-account#manage-your-storage-account).

- **(Valikuline) vaikimisi bloobimälu container**: **azure hdinsightiga kobar loomine** käsk loob ümbris, kui see pole olemas. Kui soovite luua ümbris eelnevalt, saate järgmine käsk:

    Azure'i salvestusruumi container loomine--konto nime "<Storage Account Name>"--konto võti <Storage Account Key> [ümbrisenimi]

Kui olete valmis salvestusruumi konto, olete valmis looma klaster:


    azure hdinsight cluster create -g <Resource Group Name> -c <HDInsight Cluster Name> -l <Location> --osType <Windows | Linux> --version <Cluster Version> --clusterType <Hadoop | HBase | Spark | Storm> --workerNodeCount 2 --headNodeSize Large --workerNodeSize Large --defaultStorageAccountName <Azure Storage Account Name>.blob.core.windows.net --defaultStorageAccountKey "<Default Storage Account Key>" --defaultStorageContainer <Default Blob Storage Container> --userName admin --password "<HTTP User Password>" --rdpUserName <RDP Username> --rdpPassword "<RDP User Password" --rdpAccessExpiry "03/01/2016"


##<a name="create-clusters-using-configuration-files"></a>Kogumite abil konfigureerimine failide loomine
Tavaliselt luua ka Hdinsightiga kobar, töö käitamist ja seejärel kustutage kobar kärpima maksumus. Käsurea liides annab teile võimalust salvestada faili, kuvatakse nii, et saate seda kasutada iga kord, kui loote klaster.  

    azure hdinsight config create [options ] <Config File Path> <overwirte>
    azure hdinsight config add-config-values [options] <Config File Path>
    azure hdinsight config add-script-action [options] <Config File Path>

Näide: Luua konfiguratsioonifail, mis sisaldab skripti toimingu käivitamise luua klaster.

    azure hdinsight config create "C:\myFiles\configFile.config"
    azure hdinsight config add-script-action --configFilePath "C:\myFiles\configFile.config" --nodeType HeadNode --uri <Script Action URI> --name myScriptAction --parameters "-param value"
    azure hdinsight cluster create --configurationPath "C:\myFiles\configFile.config"

##<a name="create-clusters-with-script-action"></a>Skripti toimingu kogumite loomine

Luua konfiguratsioonifail, mis sisaldab skripti toimingu käivitamise luua klaster.

    azure hdinsight config create "C:\myFiles\configFile.config"
    azure hdinsight config add-script-action --configFilePath "C:\myFiles\configFile.config" --nodeType HeadNode --uri <scriptActionURI> --name myScriptAction --parameters "-param value"

Toiminguga skripti klaster loomine

    azure hdinsight cluster create -g myarmgroup01 -l westus -y Windows --clusterType Hadoop --version 3.2 --defaultStorageAccountName mystorageaccount --defaultStorageAccountKey <defaultStorageAccountKey> --defaultStorageContainer mycontainer --userName admin --password <clusterPassword> --sshUserName sshuser --sshPassword <sshPassword> --workerNodeCount 1 –configurationPath "C:\myFiles\configFile.config" myNewCluster01


Üldine skripti toimingut leiate teemast [kohandamine Hdinsightiga kogumite kasutamata Script (Linux)](hdinsight-hadoop-customize-cluster.md).


## <a name="create-clusters-using-arm-templates"></a>Looge kogumite ARM mallide kasutamine

CLI abil saate luua kogumite, helistades ARM mallid. Leiate [Azure'i CLI juurutamine](hdinsight-hadoop-create-windows-clusters-arm-templates.md#deploy-with-azure-cli).

## <a name="see-also"></a>Vt ka

- [Alustamine Windows Azure Hdinsightiga](hdinsight-hadoop-linux-tutorial-get-started.md) – saate teada, kuidas alustada tööd oma Hdinsightiga kobar
- [Esitage Hadoopi töö programmiliselt](hdinsight-submit-hadoop-jobs-programmatically.md) – saate teada, kuidas programmiliselt edastab tööd Hdinsightiga
- [Hadoopi kogumite rakenduses abil Azure CLI Hdinsightiga haldamine](hdinsight-administer-use-command-line.md)
- [Azure'i CLI kasutamise Mac, Linux ja Windows Azure'i teenuse juhtimine](../virtual-machines-command-line-tools.md)
