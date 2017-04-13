<properties
    pageTitle="Azure'i paketi kaustadesse Linuxi sõlmed | Microsoft Azure'i"
    description="Saate teada, kuidas töödelda oma paralleelselt Arvuta töökoormus kaustu, Linux virtuaalmasinates Azure'i partii kohta."
    services="batch"
    documentationCenter="python"
    authors="mmacy"
    manager="timlt"
    editor="" />

<tags
    ms.service="batch"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="na"
    ms.date="09/08/2016"
    ms.author="marsma" />

# <a name="provision-linux-compute-nodes-in-azure-batch-pools"></a>Linux Arvuta sõlmed Azure'i paketi kaustadesse ettevalmistamine

Azure'i paketi abil saate käivitada paralleelsetes Arvuta töökoormus Windows ja Linux virtuaalmasinates. Selles artiklis üksikasjad, kuidas luua Linux Arvuta sõlmed teenuses paketi abil nii [Paketi Python] [ py_batch_package] ja [Paketi .NET] [ api_net] kliendi teegid.

> [AZURE.NOTE] [Rakenduse paketid](batch-application-packages.md) pole praegu toetatud Linux Arvuta sõlmed.

## <a name="virtual-machine-configuration"></a>Virtuaalse masina konfigureerimine

Kui loote töölehel Arvuta sõlmed kogumi, on teil kaks võimalust, millest valige sõlm suurus ja operatsioonisüsteem: virtuaalse masina konfiguratsiooni ja Cloud Services konfigureerimine.

**Cloud Services konfiguratsiooni** pakub Windows arvutada sõlmed *ainult*. Saadaval Arvuta sõlm suurused on loetletud [pilveteenustega suurused](../cloud-services/cloud-services-sizes-specs.md)ja saadaolevate operatsioonisüsteemide on loetletud [Azure'i Külastajate OS väljalasete ning SDK ühilduvuse maatriksi](../cloud-services/cloud-services-guestos-update-matrix.md). Kui loote kausta, mis sisaldab Azure'i pilveteenustega sõlmed, peate määrama ainult sõlm suurus ja selle "OS perekond," mis on toodud eelnevalt mainitud artikleid. Jaoks Windowsi kaustu arvutada sõlmed, on kõige sagedamini kasutatav pilveteenustega.

**Virtuaalne arvuti konfiguratsiooni** pakub Windows ja Linux pildid Arvuta sõlmed. Saadaval Arvuta sõlm suurused on loetletud [Azure'i virtuaalmasinates suurused](../virtual-machines/virtual-machines-linux-sizes.md) (Linux) ja [suurused Azure'i virtuaalmasinates](../virtual-machines/virtual-machines-windows-sizes.md) (Windows). Kausta, mis sisaldab virtuaalse masina konfiguratsiooni sõlmed loomisel peate määrama sõlmed, virtuaalse masina pilt viide ja paketi sõlm agent SKU olema installitud sõlmed suurust.

### <a name="virtual-machine-image-reference"></a>Viide virtuaalse masina pilt

Paketi teenuse kasutab [virtuaalse masina skaala määrab](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) Linux Arvuta sõlmed. Nende virtuaalmasinates pildid operatsioonisüsteem on esitatud [Azure'i turuplatsi][vm_marketplace]. Kui konfigureerite viite virtuaalse masina pilt, saate määrata atribuutide turuplatsi virtuaalse masina pilt. Järgmised atribuudid on nõutavad virtuaalse masina pilt viite loomisel.

| **Pildi viide atribuudid** | **Näide** |
| ----------------- | ------------------------ |
| Publisheri         | Kanoonilise                |
| Pakkumine             | UbuntuServer             |
| SKU-GA               | 14.04.4-LTS              |
| Versioon           | Viimane                   |

> [AZURE.TIP] Leiate lisateavet ja nende atribuutide loendi [navigeerimine](../virtual-machines/virtual-machines-linux-cli-ps-findimage.md)ja valige Linux virtuaalse masina pildid CLI või PowerShelli Azure'i turuplatsi pildid. Pange tähele, et kõik turuplatsi pildid on praegu ühilduv paketi. Lisateabe saamiseks vt [sõlm agent SKU-ga](#node-agent-sku).

### <a name="node-agent-sku"></a>Sõlm agent SKU-ga

Paketi sõlm agent on programm, mis töötab igal pool sõlm ja tagab käsk kontrolli sõlme ja paketi teenuste vahel. On rakendatud sõlm agent, nimetatakse SKU-de jaoks, operatsioonisüsteemides erinevalt. Sisuliselt virtuaalse masina konfiguratsiooni loomisel saate esmalt määrama virtuaalse masina pilt ning määrake sõlm agent installida pilt. Tavaliselt iga sõlm agent SKU ühildub mitme virtuaalse masina pilte. Siin on mõned näited sõlm agent SKU-de jaoks.

* Batch.Node.Ubuntu 14.04
* Batch.Node.CentOS 7
* Batch.Node.Windows amd64

> [AZURE.IMPORTANT] Kõik virtuaalse masina pilte, mis on saadaval kuvatakse Marketplace'ist ühilduvad praegu saadaval paketi sõlm agentide. Kasutage paketi SDK-d loendi saadaval sõlm agent SKU-de jaoks ja neisse ühilduvad virtuaalse masina pildid. [Loendi virtuaalse masina pilte](#list-of-virtual-machine-images) lisateabe saamiseks selle artikli teemast.

## <a name="create-a-linux-pool-batch-python"></a>Koostada Linuxi andmebaas: paketi Python

Kuvatakse järgmised koodilõigu näide sellest, kuidas kasutada [Microsoft Azure'i paketi kliendi teek Python] [ py_batch_package] Arvuta sõlmed Ubuntu Server kogumi loomiseks. Paketi Python mooduli dokumentides leiate [azure.batch paketi] [py_batch_docs] kohta vt dokumendid.

See koodilõigu loob mõne [ImageReference] [ py_imagereference] selgesõnaliselt ja määrab iga selle atribuute (publisher pakkumine SKU-ga, versioon). Kood, kuid soovitame kasutada [list_node_agent_skus] [ py_list_skus] meetodi abil määrata, ja valige saadaval pilt ja sõlm agent SKU kombinatsioonide käitusajal.

```python
# Import the required modules from the
# Azure Batch Client Library for Python
import azure.batch.batch_service_client as batch
import azure.batch.batch_auth as batchauth
import azure.batch.models as batchmodels

# Specify Batch account credentials
account = "<batch-account-name>"
key = "<batch-account-key>"
batch_url = "<batch-account-url>"

# Pool settings
pool_id = "LinuxNodesSamplePoolPython"
vm_size = "STANDARD_A1"
node_count = 1

# Initialize the Batch client
creds = batchauth.SharedKeyCredentials(account, key)
config = batch.BatchServiceClientConfiguration(creds, base_url = batch_url)
client = batch.BatchServiceClient(config)

# Create the unbound pool
new_pool = batchmodels.PoolAddParameter(id = pool_id, vm_size = vm_size)
new_pool.target_dedicated = node_count

# Configure the start task for the pool
start_task = batchmodels.StartTask()
start_task.run_elevated = True
start_task.command_line = "printenv AZ_BATCH_NODE_STARTUP_DIR"
new_pool.start_task = start_task

# Create an ImageReference which specifies the Marketplace
# virtual machine image to install on the nodes.
ir = batchmodels.ImageReference(
    publisher = "Canonical",
    offer = "UbuntuServer",
    sku = "14.04.2-LTS",
    version = "latest")

# Create the VirtualMachineConfiguration, specifying
# the VM image reference and the Batch node agent to
# be installed on the node.
vmc = batchmodels.VirtualMachineConfiguration(
    image_reference = ir,
    node_agent_sku_id = "batch.node.ubuntu 14.04")

# Assign the virtual machine configuration to the pool
new_pool.virtual_machine_configuration = vmc

# Create pool in the Batch service
client.pool.add(new_pool)
```

Nagu eelnevalt mainitud, soovitame [ImageReference] loomise asemel[ py_imagereference] , mida kasutate [list_node_agent_skus] [ py_list_skus] dünaamiliselt valida praegu toetatud sõlm agent/turuplatsi pilt kombinatsioonide meetod. Järgmised Python koodilõigu kuvatakse selle meetodi kasutamist.

```python
# Get the list of node agents from the Batch service
nodeagents = client.account.list_node_agent_skus()

# Obtain the desired node agent
ubuntu1404agent = next(agent for agent in nodeagents if "ubuntu 14.04" in agent.id)

# Pick the first image reference from the list of verified references
ir = ubuntu1404agent.verified_image_references[0]

# Create the VirtualMachineConfiguration, specifying the VM image
# reference and the Batch node agent to be installed on the node.
vmc = batchmodels.VirtualMachineConfiguration(
    image_reference = ir,
    node_agent_sku_id = ubuntu1404agent.id)
```

## <a name="create-a-linux-pool-batch-net"></a>Koostada Linuxi andmebaas: paketi .net-i

Järgmised koodilõigu kujutab näidet [Paketi .net-i] kasutamine[ nuget_batch_net] kliendi teek Arvuta sõlmed Ubuntu Server kogumi loomiseks. Saate otsida [paketi .NET dokumentides] [ api_net] MSDN-is.

Järgmised koodilõigu kasutab [PoolOperations][net_pool_ops]. [ListNodeAgentSkus] [net_list_skus] valige loendist praegu toetatud turuplatsi pilt ja sõlm agent SKU kombinatsioone. See meetod on soovitatav, kuna toetatud kombinatsioonide võib aeg-ajalt muuta. Kõige sagedamini, lisatakse toetatud kombinatsioone.

```csharp
// Pool settings
const string poolId = "LinuxNodesSamplePoolDotNet";
const string vmSize = "STANDARD_A1";
const int nodeCount = 1;

// Obtain a collection of all available node agent SKUs.
// This allows us to select from a list of supported
// VM image/node agent combinations.
List<NodeAgentSku> nodeAgentSkus =
    batchClient.PoolOperations.ListNodeAgentSkus().ToList();

// Define a delegate specifying properties of the VM image
// that we wish to use.
Func<ImageReference, bool> isUbuntu1404 = imageRef =>
    imageRef.Publisher == "Canonical" &&
    imageRef.Offer == "UbuntuServer" &&
    imageRef.SkuId.Contains("14.04");

// Obtain the first node agent SKU in the collection that matches
// Ubuntu Server 14.04. Note that there are one or more image
// references associated with this node agent SKU.
NodeAgentSku ubuntuAgentSku = nodeAgentSkus.First(sku =>
    sku.VerifiedImageReferences.Any(isUbuntu1404));

// Select an ImageReference from those available for node agent.
ImageReference imageReference =
    ubuntuAgentSku.VerifiedImageReferences.First(isUbuntu1404);

// Create the VirtualMachineConfiguration for use when actually
// creating the pool
VirtualMachineConfiguration virtualMachineConfiguration =
    new VirtualMachineConfiguration(
        imageReference: imageReference,
        nodeAgentSkuId: ubuntuAgentSku.Id);

// Create the unbound pool object using the VirtualMachineConfiguration
// created above
CloudPool pool = batchClient.PoolOperations.CreatePool(
    poolId: poolId,
    virtualMachineSize: vmSize,
    virtualMachineConfiguration: virtualMachineConfiguration,
    targetDedicated: nodeCount);

// Commit the pool to the Batch service
pool.Commit();
```

Kuigi eelmise koodilõigu kasutab [PoolOperations][net_pool_ops]. [ListNodeAgentSkus] [net_list_skus] toetatud meetodit dünaamiliselt loend ja valige pilt ja sõlm agent SKU kombinatsioone (soovitatav), saate konfigureerida ka mõne [ImageReference] [ net_imagereference] selgesõnaliselt:

```csharp
ImageReference imageReference = new ImageReference(
    publisher: "Canonical",
    offer: "UbuntuServer",
    skuId: "14.04.2-LTS",
    version: "latest");
```

## <a name="list-of-virtual-machine-images"></a>Virtuaalse masina piltide loetelu

Järgmises tabelis on loetletud turuplatsi virtuaalse masina pilte, mis ühilduvad saadaval paketi sõlm agentide artiklit viimase värskenduse. See on oluline ei ole lõpliku selles loendis, kuna pilte ja sõlm agentide võib lisada või eemaldada igal ajal. Soovitame, et teie paketi rakenduste ja teenuste Kasuta alati [list_node_agent_skus] [ py_list_skus] (Python) ja [ListNodeAgentSkus] [ net_list_skus] (paketi .NET) määramaks ja valida praegu saadaval SKU-de jaoks.

> [AZURE.WARNING] Järgmises loendis võib igal ajal muuta. Kasuta alati saadaval paketi API-de **loendis sõlm agent SKU** meetodite loend ja seejärel valige sõlm agent SKU-de jaoks ja ühilduv virtuaalse masina oma pakett-tööde käivitamisel.

| **Publisheri** | **Pakkumine** | **Pildi SKU-ga** | **Versioon** | **Sõlm agent SKU-ID** |
| ------- | ------- | ------- | ------- | ------- |
| Kanoonilise | UbuntuServer | 14.04.0-LTS | Viimane | Batch.Node.Ubuntu 14.04 |
| Kanoonilise | UbuntuServer | 14.04.1-LTS | Viimane | Batch.Node.Ubuntu 14.04 |
| Kanoonilise | UbuntuServer | 14.04.2-LTS | Viimane | Batch.Node.Ubuntu 14.04 |
| Kanoonilise | UbuntuServer | 14.04.3-LTS | Viimane | Batch.Node.Ubuntu 14.04 |
| Kanoonilise | UbuntuServer | 14.04.4-LTS | Viimane | Batch.Node.Ubuntu 14.04 |
| Kanoonilise | UbuntuServer | 14.04.5-LTS | Viimane | Batch.Node.Ubuntu 14.04 |
| Kanoonilise | UbuntuServer | 16.04.0-LTS | Viimane | Batch.Node.Ubuntu 16.04 |
| Credativ | Debian | 8 | Viimane | Batch.Node.Debian 8 |
| OpenLogic | CentOS | 7.0 | Viimane | Batch.Node.CentOS 7 |
| OpenLogic | CentOS | 7.1 | Viimane | Batch.Node.CentOS 7 |
| OpenLogic | CentOS-HPC | 7.1 | Viimane | Batch.Node.CentOS 7 |
| OpenLogic | CentOS | 7.2 | Viimane | Batch.Node.CentOS 7 |
| Oracle'i | Oracle'i-Linux | 7.0 | Viimane | Batch.Node.CentOS 7 |
| SUSE | openSUSE | 13.2 | Viimane | Batch.Node.openSUSE 13.2 |
| SUSE | openSUSE-hüpe | 42,1 | Viimane | Batch.Node.openSUSE 42,1 |
| SUSE | SLES-HPC | 12 | Viimane | Batch.Node.openSUSE 42,1 |
| SUSE | SLES | 12-SP1 | Viimane | Batch.Node.openSUSE 42,1 |
| Microsofti-reklaamid | Standard-andmete teadus-vm | Standard-andmete teadus-vm | Viimane | Batch.Node.Windows amd64 |
| Microsofti-reklaamid | Linux-andmete teadus-vm | linuxdsvm | Viimane | Batch.Node.CentOS 7 |
| MicrosoftWindowsServer | WindowsServer | 2008 R2-SP1 | Viimane | Batch.Node.Windows amd64 |
| MicrosoftWindowsServer | WindowsServer | 2012 – andmekeskusega | Viimane | Batch.Node.Windows amd64 |
| MicrosoftWindowsServer | WindowsServer | 2012 – R2-andmekeskusega | Viimane | Batch.Node.Windows amd64 |
| MicrosoftWindowsServer | WindowsServer | Windows Serveri-tehniline eelvaade | Viimane | Batch.Node.Windows amd64 |

## <a name="connect-to-linux-nodes"></a>Ühenduse loomine Linux sõlmed

Arendamise ajal või kui tõrkeotsing, võite leida selle olevate sõlmi sisselogimiseks vajalik. Erinevalt Windows Arvuta sõlmed, ei saa kasutada kaugtöölaua protokolli (RDP) ühenduse loomiseks Linux sõlmed. Selle asemel paketi teenus võimaldab SSH juurdepääsu iga sõlme kaugühenduse kaudu.

Järgmised Python koodilõigu loob kasutaja iga sõlme kogumi, mis on nõutav kaugühenduse kaudu. Seejärel valimisel prinditakse iga sõlme ühendusteabe secure shell (SSH).

```python
import datetime
import getpass
import azure.batch.batch_service_client as batch
import azure.batch.batch_auth as batchauth
import azure.batch.models as batchmodels

# Specify your own account credentials
batch_account_name = ''
batch_account_key = ''
batch_account_url = ''

# Specify the ID of an existing pool containing Linux nodes
# currently in the 'idle' state
pool_id = ''

# Specify the username and prompt for a password
username = 'linuxuser'
password = getpass.getpass()

# Create a BatchClient
credentials = batchauth.SharedKeyCredentials(
    batch_account_name,
    batch_account_key
)
batch_client = batch.BatchServiceClient(
        credentials,
        base_url=batch_account_url
)

# Create the user that will be added to each node in the pool
user = batchmodels.ComputeNodeUser(username)
user.password = password
user.is_admin = True
user.expiry_time = \
    (datetime.datetime.today() + datetime.timedelta(days=30)).isoformat()

# Get the list of nodes in the pool
nodes = batch_client.compute_node.list(pool_id)

# Add the user to each node in the pool and print
# the connection information for the node
for node in nodes:
    # Add the user to the node
    batch_client.compute_node.add_user(pool_id, node.id, user)

    # Obtain SSH login information for the node
    login = batch_client.compute_node.get_remote_login_settings(pool_id,
                                                                node.id)

    # Print the connection info for the node
    print("{0} | {1} | {2} | {3}".format(node.id,
                                         node.state,
                                         login.remote_login_ip_address,
                                         login.remote_login_port))
```

Siin on näide väljundi jaoks mõeldud pool, mis sisaldab nelja Linux sõlmed eelmise koodi.

```
Password:
tvm-1219235766_1-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50000
tvm-1219235766_2-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50003
tvm-1219235766_3-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50002
tvm-1219235766_4-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50001
```

Pange tähele, et asemel parooli, saate määrata mõne SSH avalik võti kasutaja loomisel sõlme. Python SDK, selleks **ssh_public_key** parameetri kasutamine [ComputeNodeUser][py_computenodeuser]. .Net-i, on seda teha, kasutades [ComputeNodeUser][net_computenodeuser]. [SshPublicKey] [net_ssh_key] atribuut.

## <a name="pricing"></a>Hinnad

Azure'i paketi põhineb Azure'i pilveteenustega ja Azure'i Virtuaalmasinates tehnoloogia. Paketi teenusesse on saadaval tasuta, mis tähendab, et ostmisega ainult Arvuta ressursid, et teie paketi lahendusi kasutama. Kui valite **Cloud Services konfiguratsiooni**, peate tasuma põhjal [pilveteenused hinnad] [ cloud_services_pricing] struktuuri. Kui valite **Virtuaalse masina konfiguratsiooni**, peate tasuma põhjal [Virtuaalmasinates hinnad] [ vm_pricing] struktuuri.

## <a name="next-steps"></a>Järgmised sammud

### <a name="batch-python-tutorial"></a>Paketi Python õpetus

Õpetuse põhjalikumalt töötada, kasutades Python paketi kohta, vaadake [Azure'i paketi Python kliendi alustamine](batch-python-tutorial.md). Selle companion [proovi kood] [ github_samples_pyclient] helper funktsiooni, sisaldab `get_vm_config_for_distro`, teise tehnika hankimiseks virtuaalse masina konfiguratsiooni, mis näitab.

### <a name="batch-python-code-samples"></a>Paketi Python koodinäiteid

Tutvuge selle muude [Python kood näidised] [ github_samples_py] [azure-paketi-näidised] [ github_samples] hoidla github mitu skriptide, mis näitab, kuidas teha levinud paketi toiminguid, näiteks pool, töö ja tööülesande loomine. [README] [ github_py_readme] mis kaasneb Python näidised on nõutav paketid installimise kohta üksikasju.

### <a name="batch-forum"></a>Paketi Foorum

[Azure'i paketi Foorum] [ forum] MSDN-is on hea koht arutada paketi ja teenuse kohta küsimusi. Hea "stickied" postituste lugemiseks ja postitada oma küsimusi, kui need tekivad ajal saate luua oma paketi lahendusi.

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_mgmt]: https://msdn.microsoft.com/library/azure/mt463120.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[cloud_services_pricing]: https://azure.microsoft.com/pricing/details/cloud-services/
[forum]: https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=azurebatch
[github_py_readme]: https://github.com/Azure/azure-batch-samples/blob/master/Python/Batch/README.md
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_samples_py]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch
[github_samples_pyclient]: https://github.com/Azure/azure-batch-samples/blob/master/Python/Batch/article_samples/python_tutorial_client.py
[portal]: https://portal.azure.com
[net_cloudpool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_computenodeuser]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenodeuser.aspx
[net_imagereference]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.imagereference.aspx
[net_list_skus]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listnodeagentskus.aspx
[net_pool_ops]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.aspx
[net_ssh_key]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenodeuser.sshpublickey.aspx
[nuget_batch_net]: https://www.nuget.org/packages/Azure.Batch/
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[py_account_ops]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.operations.html#azure.batch.operations.AccountOperations
[py_azure_sdk]: https://pypi.python.org/pypi/azure
[py_batch_docs]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.html
[py_batch_package]: https://pypi.python.org/pypi/azure-batch
[py_computenodeuser]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.models.html#azure.batch.models.ComputeNodeUser
[py_imagereference]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.models.html#azure.batch.models.ImageReference
[py_list_skus]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.operations.html#azure.batch.operations.AccountOperations.list_node_agent_skus
[vm_marketplace]: https://azure.microsoft.com/marketplace/virtual-machines/
[vm_pricing]: https://azure.microsoft.com/pricing/details/virtual-machines/
