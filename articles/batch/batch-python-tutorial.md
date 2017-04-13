<properties
    pageTitle="Õppeteema – alustamine Azure'i paketi Python kliendi | Microsoft Azure'i"
    description="Siit saate teada, Azure'i paketi ja kuidas luua lihtsa stsenaarium paketi teenuse põhimõtted"
    services="batch"
    documentationCenter="python"
    authors="mmacy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="batch"
    ms.devlang="python"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.workload="big-compute"
    ms.date="09/27/2016"
    ms.author="marsma"/>

# <a name="get-started-with-the-azure-batch-python-client"></a>Azure'i paketi Python kliendi kasutamise alustamine

> [AZURE.SELECTOR]
- [.NET-I](batch-dotnet-get-started.md)
- [Python](batch-python-tutorial.md)

[Azure'i paketi] põhialused[ azure_batch] ja [Paketi Python] [ py_azure_sdk] kliendi nagu me käsitletakse väike paketi rakendus kirjutatud Python. Vaatame kuidas kahe valimi skriptide kasutamine paketi teenust töödelda paralleelselt töökoormus kohta Linux virtuaalmasinates pilves, ja kuidas nad suhelda [Azure Storage](./../storage/storage-introduction.md) faili lavastus ja otsing. Saate lugege levinud paketi rakenduse töövoo ja base mõistmiseks paketi põhikomponentide näiteks tööde haldamine, tööülesannete, kaustu ja arvutada sõlmed.

![Paketi lahenduse töövoo (tavaline)][11]<br/>

## <a name="prerequisites"></a>Eeltingimused

Selles artiklis eeldatakse, et teil on tundma Python tehnoloogiat ja Linux. Ka eeldatakse, et saaksite nõuetele, konto loomist on kirjeldatud allpool Azure ja paketi ja salvestusruumi teenuste jaoks.

### <a name="accounts"></a>Kontod

- **Azure'i konto**: kui teil pole veel Azure'i tellimus, [luua tasuta Azure'i konto][azure_free_account].
- **Paketi kontoga**: kui teil on Azure'i tellimus, [konto Azure'i paketi loomine](batch-account-create-portal.md).
- **Salvestusruumi konto**: lugege teemat [Loo konto salvestusruumi](../storage/storage-create-storage-account.md#create-a-storage-account) [kohta Azure salvestusruumi](../storage/storage-create-storage-account.md)kontodel.

### <a name="code-sample"></a>Proovi kood

Python kuueosalisest [proovi kood] [ github_article_samples] on üks paljude paketi koodinäiteid leitud [azure-paketi-näidised] [ github_samples] hoidla github. Kõik näidised saate alla laadida, klõpsates **klooni või allalaadimine > Laadige alla ZIP** hoidla avalehel või klõpsates [Azure'i paketi-näidised master.zip] [ github_samples_zip] otsene download link. Kui te olete eraldada ZIP-faili sisu, kaks skriptide selles õpetuses mõeldud leidub selle `article_samples` directory:

`/azure-batch-samples/Python/Batch/article_samples/python_tutorial_client.py`<br/>
`/azure-batch-samples/Python/Batch/article_samples/python_tutorial_task.py`

### <a name="python-environment"></a>Python keskkonnas

Käivitage oma kohaliku töökoha *python_tutorial_client.py* valimi skripti, peate on **Python Tõlgi** versioon **2.7** või **3,3 +**. Skripti on testitud Windows ja Linux.

### <a name="cryptography-dependencies"></a>krüptograafia sõltuvused

Peate installima sõltuvused [krüptograafia] [ crypto] Raamatukogu, nõutud on `azure-batch` ning `azure-storage` Python paketid. Tehke üks järgmistest toimingutest oma platvormile vastav või viidata [krüptograafia installi] [ crypto_install] Lisateavet üksikasjad:

* Ubuntu

    `apt-get update && apt-get install -y build-essential libssl-dev libffi-dev libpython-dev python-dev`

* CentOS

    `yum update && yum install -y gcc openssl-dev libffi-devel python-devel`

* SLES/OpenSUSE

    `zypper ref && zypper -n in libopenssl-dev libffi48-devel python-devel`

* Windows

    `pip install cryptography`

>[AZURE.NOTE] Kui Python 3.3 + Linux installimist kasutada python3 ekvivalendid Python sõltuvused. Näiteks Ubuntu:`apt-get update && apt-get install -y build-essential libssl-dev libffi-dev libpython3-dev python3-dev`

### <a name="azure-packages"></a>Azure'i pakettide

Seejärel installige **Azure'i paketi** ja **Azure Storage** Python paketid. Saate seda teha **pip** ja *requirements.txt* leiate siit:

`/azure-batch-samples/Python/Batch/requirements.txt`

Järgmine **pip** käsk installida paketi ja salvestusruumi pakettide probleem:

`pip install -r requirements.txt`

Või saate installida [azure-paketi] [ pypi_batch] ja [azure-salvestusruumi] [ pypi_storage] Python paketid käsitsi:

`pip install azure-batch`<br/>
`pip install azure-storage`

> [AZURE.TIP] Peate oma käskudele eesliide `sudo` eesõiguseta konto kasutamisel. Näiteks `sudo pip install -r requirements.txt`. Python paketid installimise kohta leiate lisateavet teemast [Installimist pakettide] [ pypi_install] readthedocs.io kohta.

## <a name="batch-python-tutorial-code-sample"></a>Paketi Python kuueosalisest koodi näidis

Paketi Python kuueosalisest kood valimi koosneb kahe Python skripte ja mõned andmefailid.

- **python_tutorial_client.py**: suhtleb paketi ja salvestusruumi teenuseid käivitada paralleelselt töökoormus Arvuta sõlmed (virtuaalmasinates). Skripti *python_tutorial_client.py* töötab teie kohaliku töökoha.

- **python_tutorial_task.py**: skripti, mis töötab arvutada sõlmed Azure tegelikku tööd teha. Valimis, *python_tutorial_task.py* sõelub teksti faili alla laadida Azure Storage (input-faili). Seejärel annab tulemiks ülemise kolme sõnu, mis kuvatakse Sisestuskeel faili sisaldava tekstifaili (väljundfail). Pärast seda loob väljundfail, *python_tutorial_task.py* lisatud faili Azure Storage. See muudab kliendi skripti käivitamist sisse oma töökoha allalaadimiseks saadaval. Skripti *python_tutorial_task.py* töötab samal ajal mitme Arvuta sõlmed paketi teenus.

- **./data/taskdata\*.txt**: need kolm tekstifailid sisend ette käivitada Arvuta sõlmed tööülesandeid.

Järgmine diagramm näitab esmane toimingud, mida kliendi ja tööülesannete skriptide läbi. Selle lihtsa töövoo on palju Arvuta lahendusi, mis on loodud paketi tüüpiline. Kuigi iga funktsioon on saadaval paketi teenuse näidata peaaegu iga paketi stsenaarium sisaldab töövoo osade.

![Töövoo paketi näide][8]<br/>

[**Samm 1.**](#step-1-create-storage-containers) Azure'i bloobimälu **ümbriste** luua.<br/>
[**Samm 2.**](#step-2-upload-task-script-and-data-files) Ümbriste tööülesande skripte ja sisendi failide üleslaadimine.<br/>
[**Samm 3.**](#step-3-create-batch-pool) Saate luua paketi **kausta**.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**3.** Rakenduskausta **StartTask** laadib tööülesande script (python_tutorial_task.py) sõlmed liitumist pool.<br/>
[**Samm 4.**](#step-4-create-batch-job) Looge paketi **töö**.<br/>
[**Juhis 5.**](#step-5-add-tasks-to-job) Projekti **tööülesannete** lisada.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**5a.** Tööülesanded on ajastatud sõlmed käivitada.<br/>
    &nbsp;&nbsp;&nbsp;&nbsp;**5b.** Iga tööülesande allalaadimine oma sisendandmete Azure Storage ja seejärel algab täitmise.<br/>
[**Juhist 6.**](#step-6-monitor-tasks) Tööülesannete jälgimine.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**6a.** Ülesanded lõpetatuks üles laadida oma andmete Azure Storage.<br/>
[**Juhis 7.**](#step-7-download-task-output) Salvestusruumi tööülesande väljundi alla laadida.

Paketi mainitud, mitte iga lahenduse teeb järgmist täpse ning võivad sisaldada palju veel, kuid see valimi näitab levinud protsessid paketi lahenduse leida.

## <a name="prepare-client-script"></a>Kliendi skripti ettevalmistamine

Enne, kui käivitate valimi, lisada *python_tutorial_client.py*paketi ja salvestusruumi konto identimisteave. Kui te pole seda veel teinud, avage fail oma lemmik redaktoris ja järgmised read uuendada oma kasutajanimi ja parool.

```python
# Update the Batch and Storage account credential strings below with the values
# unique to your accounts. These are used when constructing connection strings
# for the Batch and Storage client objects.

# Batch account credentials
batch_account_name = "";
batch_account_key  = "";
batch_account_url  = "";

# Storage account credentials
storage_account_name = "";
storage_account_key  = "";
```

Paketi ja salvestusruumi konto identimisteave sees iga teenuse konto tera leiate [Azure'i portaalis][azure_portal]:

![Partii identimisteabe portaalis][9]
![salvestusruumi identimisteabe portaalis][10]<br/>

Järgmistes jaotistes analüüsime kasutavad skriptide töötlemine on töökoormus paketi teenus juhiseid. Soovitame teil regulaarselt viidata skriptide oma redaktoris töötamisel teed läbi ülejäänud artikkel.

Avage järgmine rea **python_tutorial_client.py** alustada samm 1:

```python
if __name__ == '__main__':
```

## <a name="step-1-create-storage-containers"></a>Samm 1: Loo säilitus

![Ümbriste Azure Storage loomine][1]
<br/>

Pakett sisaldab tugi Azure Storage suhtlemiseks. Ümbriste teie salvestusruumi konto annab ülesandeid, mis töötavad teie paketi konto vajalikud failid. Funktsiooni ümbriste pakuvad ka salvestamiseks tööülesanded aedvili väljundi andmeid. Esimese asjana *python_tutorial_client.py* skript ei on luua kolm ümbriste [Azure'i bloobimälu](../storage/storage-introduction.md#blob-storage).

- **rakendus**: see ümbris salvestab Python skript, tööülesannete, *python_tutorial_task.py*käivitamine.
- **Sisestuskeel**: tööülesannete andmefailid *Sisestuskeel* ümbris asukohast alla.
- **väljundi**: kui tööülesannete Sisestuskeel faili töötlemine lõpule viia, nad üles laadida tulemused *väljundi* ümbris.

Salvestusruumi konto suhelda ja ümbriste loomiseks, kasutame [azure-salvestusruumi] [ pypi_storage] paketi loomiseks on [BlockBlobService] [ py_blockblobservice] objekti--"bloobimälu klient." Seejärel looge kolm ümbriste bloobimälu kliendi konto salvestusruumi.

```python
 # Create the blob client, for use in obtaining references to
 # blob storage containers and uploading files to containers.
 blob_client = azureblob.BlockBlobService(
     account_name=_STORAGE_ACCOUNT_NAME,
     account_key=_STORAGE_ACCOUNT_KEY)

 # Use the blob client to create the containers in Azure Storage if they
 # don't yet exist.
 app_container_name = 'application'
 input_container_name = 'input'
 output_container_name = 'output'
 blob_client.create_container(app_container_name, fail_on_exist=False)
 blob_client.create_container(input_container_name, fail_on_exist=False)
 blob_client.create_container(output_container_name, fail_on_exist=False)
```

Kui soovitud ümbriste on loodud, saate rakenduse nüüd laadida faile, mis kasutavad toimingud.

> [AZURE.TIP] [Azure'i bloobimälu kaudu Python kasutamine](../storage/storage-python-how-to-use-blob-storage.md) pakub hea ülevaade Azure'i säilitus- ja plekid töötamine. Lugemispaanil loendi ülaosas peaks olema nagu paketi töötamist alustada.

## <a name="step-2-upload-task-script-and-data-files"></a>Samm 2: Tööülesande skripte ja andmete failide üleslaadimine

![Ümbriste tööülesande taotlus ja Sisestuskeel (andmed) failide üleslaadimine][2]
<br/>

Faili üleslaadimine toiming, *python_tutorial_client.py* esmalt määratleb **rakendus** ja **Sisestuskeel** failiteed kogumeid, kui nad pole kohalikus arvutis. Seejärel lisatud need failid vastavad eelmises juhises loodud ümbriste abil.

```python
 # Paths to the task script. This script will be executed by the tasks that
 # run on the compute nodes.
 application_file_paths = [os.path.realpath('python_tutorial_task.py')]

 # The collection of data files that are to be processed by the tasks.
 input_file_paths = [os.path.realpath('./data/taskdata1.txt'),
                     os.path.realpath('./data/taskdata2.txt'),
                     os.path.realpath('./data/taskdata3.txt')]

 # Upload the application script to Azure Storage. This is the script that
 # will process the data files, and is executed by each of the tasks on the
 # compute nodes.
 application_files = [
     upload_file_to_container(blob_client, app_container_name, file_path)
     for file_path in application_file_paths]

 # Upload the data files. This is the data that will be processed by each of
 # the tasks executed on the compute nodes in the pool.
 input_files = [
     upload_file_to_container(blob_client, input_container_name, file_path)
     for file_path in input_file_paths]
```

Loendi mõistmist, kasutades funktsiooni `upload_file_to_container` funktsiooni nimetatakse iga faili kogumite ja kahe [ResourceFile] [ py_resource_file] täidetakse saidikogumid. Funktsiooni `upload_file_to_container` funktsioon all kuvatakse:

```
def upload_file_to_container(block_blob_client, container_name, file_path):
    """
    Uploads a local file to an Azure Blob storage container.

    :param block_blob_client: A blob service client.
    :type block_blob_client: `azure.storage.blob.BlockBlobService`
    :param str container_name: The name of the Azure Blob storage container.
    :param str file_path: The local path to the file.
    :rtype: `azure.batch.models.ResourceFile`
    :return: A ResourceFile initialized with a SAS URL appropriate for Batch
    tasks.
    """
    blob_name = os.path.basename(file_path)

    print('Uploading file {} to container [{}]...'.format(file_path,
                                                          container_name))

    block_blob_client.create_blob_from_path(container_name,
                                            blob_name,
                                            file_path)

    sas_token = block_blob_client.generate_blob_shared_access_signature(
        container_name,
        blob_name,
        permission=azureblob.BlobPermissions.READ,
        expiry=datetime.datetime.utcnow() + datetime.timedelta(hours=2))

    sas_url = block_blob_client.make_blob_url(container_name,
                                              blob_name,
                                              sas_token=sas_token)

    return batchmodels.ResourceFile(file_path=blob_name,
                                    blob_source=sas_url)
```

### <a name="resourcefiles"></a>ResourceFiles

Mõne [ResourceFile] [ py_resource_file] pakub tööülesanded paketi Azure Storage, mis on laaditud Arvuta sõlme enne tööülesande käitatakse faili URL-i abil. [ResourceFile][py_resource_file]. **blob_source** atribuut määrab faili täielik URL, nagu need on Azure Storage. Ühiskasutatava Accessi allkirja (SAS), mis pakub turvaline juurdepääs fail võib sisaldada ka URL-i. Tööülesande enamiku paketi kaasata *ResourceFiles* atribuudi, sh:

- [CloudTask][py_task]
- [StartTask][py_starttask]
- [JobPreparationTask][py_jobpreptask]
- [JobReleaseTask][py_jobreltask]

Selle valimi ei kasuta JobPreparationTask või JobReleaseTask ülesandetüüpide hostimine, kuid saate neid [käivitada töö ettevalmistamine ja lõpetamise tööülesannete Azure'i paketi arvutada sõlmed](batch-job-prep-release.md)kohta.

### <a name="shared-access-signature-sas"></a>Ühiskasutusega juurdepääsu allkirja (SAS)

Ühiskasutusega juurdepääsu allkirjad on stringid, et tagada turvaline juurdepääs ümbriste ja plekid Azure Storage. *Python_tutorial_client.py* skript kasutab nii bloobimälu ja container ühiskasutusse antud juurdepääs allkirjade ja näitab, kuidas saada stringide ühiskasutusega juurdepääsu allkirja salvestusruumi teenus.

- **Bloobimälu ühiskasutusse Accessi allkirjad**: selle kausta StartTask kasutab Bloobivahemälu ühiskasutusega juurdepääsu allkirjade kui see laadib alla tööülesande skripte ja sisestatud andmete failide salvestusruumi (vt [Samm #3](#step-3-create-batch-pool) ). Funktsiooni `upload_file_to_container` funktsiooni *python_tutorial_client.py* sisaldab koodi, mis hangib iga bloobimälu ühiskasutusega juurdepääsu allkirja. Seda tehes helistades [BlockBlobService.make_blob_url] [ py_make_blob_url] moodulis salvestusruumi.

- **Container ühiskasutusega juurdepääsu allkirja**: Kuna iga tööülesande lõpetab oma töö Arvuta sõlme, see lisatud selle väljundfail *väljundi* container Azure Storage. Selleks kasutab *python_tutorial_task.py* container ühiskasutusega juurdepääsu allkirja, mis pakub kirjutusõiguse ümbris. Funktsiooni `get_container_sas_token` funktsiooni *python_tutorial_client.py* hangib kõigepealt ümbrist ühiskasutusega juurdepääsu allkirjaga, mis on seejärel edasi käsurea argumendina tööülesanded. [Tööülesannete lisamine projekti](#step-5-add-tasks-to-job)samm #5, käsitletakse container SAS kasutamist.

> [AZURE.TIP] Kaheosaline sarjas ühiskasutusega juurdepääsu allkirjad, tutvuge [osa 1: SAS andmemudeli mõistmine](../storage/storage-dotnet-shared-access-signature-part-1.md) ja [osa 2: loomine ja kasutamine on SAS bloobimälu teenuse](../storage/storage-dotnet-shared-access-signature-part-2.md), lisateavet turvalist juurdepääsu teie salvestusruumi andmeid.

## <a name="step-3-create-batch-pool"></a>Samm 3: Looge paketi pool

![Paketi kausta loomine][3]
<br/>

Paketi **rakenduskausta** on kogum Arvuta sõlmed (virtuaalmasinates) kohta, mis käivitab paketi lisamine tööülesannetest.

Pärast seda lisatud tööülesande skripte ja andmete failide salvestusruumi konto, käivitub *python_tutorial_client.py* koostööd paketi teenuse paketi Python mooduli abil. Selleks on [BatchServiceClient] [ py_batchserviceclient] on loodud:

```python
 # Create a Batch service client. We'll now be interacting with the Batch
 # service in addition to Storage.
 credentials = batchauth.SharedKeyCredentials(_BATCH_ACCOUNT_NAME,
                                              _BATCH_ACCOUNT_KEY)

 batch_client = batch.BatchServiceClient(
     credentials,
     base_url=_BATCH_ACCOUNT_URL)
```

Järgmiseks kogumi Arvuta sõlmed on loodud paketi konto kõne `create_pool`.

```python
def create_pool(batch_service_client, pool_id,
                resource_files, publisher, offer, sku):
    """
    Creates a pool of compute nodes with the specified OS settings.

    :param batch_service_client: A Batch service client.
    :type batch_service_client: `azure.batch.BatchServiceClient`
    :param str pool_id: An ID for the new pool.
    :param list resource_files: A collection of resource files for the pool's
    start task.
    :param str publisher: Marketplace image publisher
    :param str offer: Marketplace image offer
    :param str sku: Marketplace image sku
    """
    print('Creating pool [{}]...'.format(pool_id))

    # Create a new pool of Linux compute nodes using an Azure Virtual Machines
    # Marketplace image. For more information about creating pools of Linux
    # nodes, see:
    # https://azure.microsoft.com/documentation/articles/batch-linux-nodes/

    # Specify the commands for the pool's start task. The start task is run
    # on each node as it joins the pool, and when it's rebooted or re-imaged.
    # We use the start task to prep the node for running our task script.
    task_commands = [
        # Copy the python_tutorial_task.py script to the "shared" directory
        # that all tasks that run on the node have access to.
        'cp -r $AZ_BATCH_TASK_WORKING_DIR/* $AZ_BATCH_NODE_SHARED_DIR',
        # Install pip and the dependencies for cryptography
        'apt-get update',
        'apt-get -y install python-pip',
        'apt-get -y install build-essential libssl-dev libffi-dev python-dev',
        # Install the azure-storage module so that the task script can access
        # Azure Blob storage
        'pip install azure-storage']

    # Get the node agent SKU and image reference for the virtual machine
    # configuration.
    # For more information about the virtual machine configuration, see:
    # https://azure.microsoft.com/documentation/articles/batch-linux-nodes/
    sku_to_use, image_ref_to_use = \
        common.helpers.select_latest_verified_vm_image_with_node_agent_sku(
            batch_service_client, publisher, offer, sku)

    new_pool = batch.models.PoolAddParameter(
        id=pool_id,
        virtual_machine_configuration=batchmodels.VirtualMachineConfiguration(
            image_reference=image_ref_to_use,
            node_agent_sku_id=sku_to_use),
        vm_size=_POOL_VM_SIZE,
        target_dedicated=_POOL_NODE_COUNT,
        start_task=batch.models.StartTask(
            command_line=
            common.helpers.wrap_commands_in_shell('linux', task_commands),
            run_elevated=True,
            wait_for_success=True,
            resource_files=resource_files),
        )

    try:
        batch_service_client.pool.add(new_pool)
    except batchmodels.batch_error.BatchErrorException as err:
        print_batch_exception(err)
        raise
```

On loomisel saate määratleda on [PoolAddParameter] [ py_pooladdparam] , mis täpsustab mitme kausta atribuudid:

- **ID** pool (*id* - nõutav)<p/>Enamiku üksuste paketi, sarnaselt oma uue kausta peab olema kordumatu ID konto paketi piires. Teie kood viitab selles kaustas, kasutades oma ID-d, ja see on kuidas tuvastate rakenduskausta Azure [portaali][azure_portal].

- **Arvuta sõlmed arv** (*target_dedicated* – nõutav)<p/>Atribuut määrab, mitu VMs tuleb kasutada pool. See on oluline Pange tähele, et paketi kõik kontod vaikimisi **piirmäära** , mis piirab **tuuma** (ja seega Arvuta sõlmed) arvu paketi konto. Leiate [kvootide](batch-quota-limit.md)ja limiidid Azure'i paketi teenuse vaikimisi kvootide ja juhised, kuidas [suurendada kvoodi](batch-quota-limit.md#increase-a-quota) (nt kontol paketi valdkond maksimum). Kui te ei leia endale küsida "Miks ei minu pool saavutamiseks rohkem kui X sõlmed?" põhjuseks võib olla core kvoodi.

- **Operatsioonisüsteemi** sõlmed (*virtual_machine_configuration* **või** *cloud_service_configuration* – nõutav)<p/>*Python_tutorial_client.py*, loome Linux sõlmed abil soovitud [VirtualMachineConfiguration]kogumi[py_vm_config]. Funktsiooni `select_latest_verified_vm_image_with_node_agent_sku` toimida `common.helpers` lihtsustab töötamise [Azure'i Virtuaalmasinates turuplatsi] [ vm_marketplace] pildid. Lisateavet turuplatsiga piltide kasutamise kohta vt [Sätte Linux arvutada sõlmed Azure'i paketi kaustadesse](batch-linux-nodes.md) .

- **Arvuta sõlmed suurus** (*vm_size* – nõutav)<p/>Kuna määrame ära Linux sõlmed meie [VirtualMachineConfiguration][py_vm_config], saame Määrake VM suurus (`STANDARD_A1` selles näites): [Azure'i virtuaalmasinates suurused](../virtual-machines/virtual-machines-linux-sizes.md). Klõpsake uuesti [sätte Linux arvutada sõlmed Azure'i paketi kaustadesse](batch-linux-nodes.md) lisateabe saamiseks vaadake.

- **Tööülesande käivitamine** (*start_task* - ei pea)<p/>Koos selle kohal füüsilise sõlm atribuudid saate määrata ka mõne [StartTask] [ py_starttask] mõeldud pool (ei ole vajalik). Funktsiooni StartTask aktiveeritakse iga sõlme sõlme ühendab pool ja iga kord, kui sõlm taaskäivitamist. Funktsiooni StartTask on eriti kasulik Arvuta sõlmed ettevalmistamine täitmise ülesandeid, näiteks tööülesannete töötavad rakenduste installimine.<p/>Valimi rakenduses selle StartTask kopeerib failid, mis seda allalaadimine salvestusruumi (mis on määratud, kasutades funktsiooni StartTask **resource_files** atribuudi) StartTask *töö kataloog* *ühiskasutusse antud* kausta, mis töötab sõlme kõik tööülesanded on juurdepääs. Sisuliselt kopeeritakse `python_tutorial_task.py` ühiskasutusega kataloogi iga sõlme nagu sõlme ühendab pool, nii, et kõik toimingud, mis töötavad sõlme sellele juurde pääsevad.

Märkate kõne edastamiseks selle `wrap_commands_in_shell` helper funktsiooni. See funktsioon võtab kogumi eraldi käsud ja loob ühe käsurea vastav on tööülesande käsurea atribuut.

Ka ülaltoodud koodilõigu märgatav on kaks keskkonna muutujate soovitud StartTask atribuudis **command_line** kasutamine: `AZ_BATCH_TASK_WORKING_DIR` ja `AZ_BATCH_NODE_SHARED_DIR`. Mitut keskkonna muutujat, mis on omased paketi on konfigureeritud automaatselt iga MSDN paketi rakenduskausta sees. Mis tahes protsess, mis on sooritatud toimingu on juurdepääs nende keskkonna muutujate.

> [AZURE.TIP] Keskkonna muutujate saadaolevad Arvuta sõlmed paketi pool, samuti teavet tööülesande töötamise kataloogide kohta rohkem teada saada, leiate [Azure'i paketi funktsioonide ülevaate](batch-api-basics.md) **keskkonna sätteid ülesannete jaoks,** ja **failide ja kataloogide** .

## <a name="step-4-create-batch-job"></a>Samm 4: Looge pakett

![Töö paketi loomine][4]<br/>

Paketi **töö** on tööülesannete kogumi ja on seostatud kogumi Arvuta sõlmed. Projekti tööülesanded täita seotud rakenduskausta Arvuta sõlmed.

Abil saate töö mitte ainult korraldamine ja seotud töökoormus, aga ka teatud piirangud – näiteks maksimaalne käitusaja (töö ja laiend, ülesannete) määramise tööülesannete jälitamine ja muud tööd konto pakett tähtsuse järjekorras. Selles näites siiski töö on seostatud ainult pool, mis loodi juhises #3. Täiendavad atribuudid pole konfigureeritud.

Kõik paketi tööd on seotud konkreetse pool. See näitab, millised sõlmed projekti tööülesandeid täita. Saate määrata pool [PoolInformation] abil[ py_poolinfo] atribuudi, nagu on näidatud allpool koodilõigu.

```python
def create_job(batch_service_client, job_id, pool_id):
    """
    Creates a job with the specified ID, associated with the specified pool.

    :param batch_service_client: A Batch service client.
    :type batch_service_client: `azure.batch.BatchServiceClient`
    :param str job_id: The ID for the job.
    :param str pool_id: The ID for the pool.
    """
    print('Creating job [{}]...'.format(job_id))

    job = batch.models.JobAddParameter(
        job_id,
        batch.models.PoolInformation(pool_id=pool_id))

    try:
        batch_service_client.job.add(job)
    except batchmodels.batch_error.BatchErrorException as err:
        print_batch_exception(err)
        raise
```

Nüüd, kui tööd on loodud, lisatakse tööülesannete töö tegemiseks.

## <a name="step-5-add-tasks-to-job"></a>Juhis 5: Lisada tööülesandeid töö

![Töö ülesannete lisamine][5]<br/>
*(1) ülesanded lisatakse töö, (2) tööülesanded on ajastatud sõlmed ja (3) tööülesanded allalaadimine andmefailid töötlemiseks*

Paketi **Tööülesanded** on üksikute üksuste töö, et käivitada Arvuta sõlmed. Tööülesande on mõne käsurea ning skriptide või täitmisfailid, kus saate määrata selle käsurea töötab.

Tegelikku tööd teha, peab ülesanded lisatakse tööd. Iga [CloudTask] [ py_task] käsurea atribuut ja [ResourceFiles] [ py_resource_file] (nagu on pool StartTask) mis tööülesande allalaadimist sõlm enne selle käsurea täidetakse automaatselt. Valimis, iga tööülesande töötleb ainult üks fail. Seega selle ResourceFiles kogu sisaldab ühe elemendi.

```python
def add_tasks(batch_service_client, job_id, input_files,
              output_container_name, output_container_sas_token):
    """
    Adds a task for each input file in the collection to the specified job.

    :param batch_service_client: A Batch service client.
    :type batch_service_client: `azure.batch.BatchServiceClient`
    :param str job_id: The ID of the job to which to add the tasks.
    :param list input_files: A collection of input files. One task will be
     created for each input file.
    :param output_container_name: The ID of an Azure Blob storage container to
    which the tasks will upload their results.
    :param output_container_sas_token: A SAS token granting write access to
    the specified Azure Blob storage container.
    """

    print('Adding {} tasks to job [{}]...'.format(len(input_files), job_id))

    tasks = list()

    for input_file in input_files:

        command = ['python $AZ_BATCH_NODE_SHARED_DIR/python_tutorial_task.py '
                   '--filepath {} --numwords {} --storageaccount {} '
                   '--storagecontainer {} --sastoken "{}"'.format(
                    input_file.file_path,
                    '3',
                    _STORAGE_ACCOUNT_NAME,
                    output_container_name,
                    output_container_sas_token)]

        tasks.append(batch.models.TaskAddParameter(
                'topNtask{}'.format(input_files.index(input_file)),
                wrap_commands_in_shell('linux', command),
                resource_files=[input_file]
                )
        )

    batch_service_client.task.add_collection(job_id, tasks)
```

> [AZURE.IMPORTANT] Kui nad juurde keskkonna muutujate nagu `$AZ_BATCH_NODE_SHARED_DIR` või käivitada rakendus ei leia soovitud sõlm `PATH`, tööülesande käsk read peavad autonoomsest shell, näiteks koos `/bin/sh -c MyTaskApplication $MY_ENV_VAR`. See tingimus on mittevajalike kui tööülesannete käivitada rakendus on sõlme `PATH` ja viide mis tahes keskkonna muutujate.

Sees on `for` tsükkel ülaltoodud koodilõigu, näete, et tööülesanne käsurea ehitatakse viis käsurea argumendid, mis on edasi *python_tutorial_task.py*:

1. **filepath**: see on kohaliku faili tee, nagu need on sõlme. Kui soovitud ResourceFile objekt lehel `upload_file_to_container` on loodud eeltoodud toiminguga 2 kasutati selle atribuudi faili nime (selle `file_path` parameetri ResourceFile ehitaja). See näitab, et faili leiate sõlm nimega *python_tutorial_task.py*samas kaustas.

2. **numwords**: ülemised *N* sõnad tuleb kirjutada väljundi faili.

3. **storageaccount**: kuulub container, millele tuleks üles laaditud tööülesande väljundi salvestusruumi konto nimi.

4. **storagecontainer**: nime salvestusruumi-ümbrisest, millele tuleks väljund faili üles laadida.

5. **sastoken**: ühiskasutatava Accessi allkirja (SAS), mis pakub **väljundi** container Azure Storage kirjutusõiguse. *Python_tutorial_task.py* skript kasutab ühiskasutusega juurdepääsu allkirja kui loob BlockBlobService viite. See pakub ümbris kirjutusõiguse nõudmata salvestusruumi konto võti.

```python
# NOTE: Taken from python_tutorial_task.py

# Create the blob client using the container's SAS token.
# This allows us to create a client that provides write
# access only to the container.
blob_client = azureblob.BlockBlobService(account_name=args.storageaccount,
                                         sas_token=args.sastoken)
```

## <a name="step-6-monitor-tasks"></a>Samm 6: Kuvari tööülesannete

![Ülesannete jälgimine][6]<br/>
*Script (1) jälgib lõpetatusoleku ülesanded ja (2) tööülesannete Azure Storage tulemi andmete üleslaadimine*

Kui tööülesanded on lisatud tööd, nad on automaatselt ootele ja ajastatud täitmiseks Arvuta sõlmed maksimaalselt pool tööga seotud. Vastavalt teie määratud sätteid, paketi tegeleb kõik tööülesande queuing, ajastamise, proovitakse uuesti ja muid tööülesande halduse ülesandeid teie eest.

On mitmeid võimalusi tööülesande täitmise jälgimiseks. Funktsiooni `wait_for_tasks_to_complete` funktsiooni *python_tutorial_client.py* pakub lihtne näide jälgimise toimingud riik, sellisel juhul [lõpetatud] [ py_taskstate] olekus.

```python
def wait_for_tasks_to_complete(batch_service_client, job_id, timeout):
    """
    Returns when all tasks in the specified job reach the Completed state.

    :param batch_service_client: A Batch service client.
    :type batch_service_client: `azure.batch.BatchServiceClient`
    :param str job_id: The id of the job whose tasks should be to monitored.
    :param timedelta timeout: The duration to wait for task completion. If all
    tasks in the specified job do not reach Completed state within this time
    period, an exception will be raised.
    """
    timeout_expiration = datetime.datetime.now() + timeout

    print("Monitoring all tasks for 'Completed' state, timeout in {}..."
          .format(timeout), end='')

    while datetime.datetime.now() < timeout_expiration:
        print('.', end='')
        sys.stdout.flush()
        tasks = batch_service_client.task.list(job_id)

        incomplete_tasks = [task for task in tasks if
                            task.state != batchmodels.TaskState.completed]
        if not incomplete_tasks:
            print()
            return True
        else:
            time.sleep(1)

    print()
    raise RuntimeError("ERROR: Tasks did not reach 'Completed' state within "
                       "timeout period of " + str(timeout))
```

## <a name="step-7-download-task-output"></a>Juhis 7: Allalaadimine tööülesande väljund

![Tööülesande väljund saate alla laadida salvestusruum][7]<br/>

Nüüd, kui töö on lõpule viidud, tööülesanded väljund saate alla laadida Azure Storage. Seda saab teha kõne `download_blobs_from_container` sisse *python_tutorial_client.py*:

```python
def download_blobs_from_container(block_blob_client,
                                  container_name, directory_path):
    """
    Downloads all blobs from the specified Azure Blob storage container.

    :param block_blob_client: A blob service client.
    :type block_blob_client: `azure.storage.blob.BlockBlobService`
    :param container_name: The Azure Blob storage container from which to
     download files.
    :param directory_path: The local directory to which to download the files.
    """
    print('Downloading all files from container [{}]...'.format(
        container_name))

    container_blobs = block_blob_client.list_blobs(container_name)

    for blob in container_blobs.items:
        destination_file_path = os.path.join(directory_path, blob.name)

        block_blob_client.get_blob_to_path(container_name,
                                           blob.name,
                                           destination_file_path)

        print('  Downloaded blob [{}] from container [{}] to {}'.format(
            blob.name,
            container_name,
            destination_file_path))

    print('  Download complete!')
```

> [AZURE.NOTE] Kõne suunamiseks `download_blobs_from_container` *python_tutorial_client.py* määrab, et teie kodune kataloogi peaks faile alla laadida. Võite muuta selle Väljastuskoht.

## <a name="step-8-delete-containers"></a>Samm 8: Kustuta ümbriste

Kuna ostmisega Azure Storage andmeid, on alati on hea mõte kasutada mis tahes plekid, mis ei ole enam vaja oma pakett-tööde eemaldada. *Python_tutorial_client.py*, on seda teha on kolm kõnesid [BlockBlobService.delete_container][py_delete_container]:

```
# Clean up storage resources
print('Deleting containers...')
blob_client.delete_container(app_container_name)
blob_client.delete_container(input_container_name)
blob_client.delete_container(output_container_name)
```

## <a name="step-9-delete-the-job-and-the-pool"></a>Samm 9: Töö ja pool kustutamine

Viimane toiming teil palutakse töö ja pool, mis on loodud *python_tutorial_client.py* skript kustutamine. Kuigi pole ostmisega ülesannete ise, mida *on* ja Arvuta sõlmed eest. Seetõttu soovitame, et sõlmed eraldada ainult vastavalt vajadusele. Kasutamata kaustu kustutamine võib olla teie hoolduse käigus.

Funktsiooni BatchServiceClient [JobOperations] [ py_job] ja [PoolOperations] [ py_pool] mõlemad on vastava kustutamise meetodid, mida nimetatakse kui kustutamise kinnitamine:

```python
# Clean up Batch resources (if the user so chooses).
if query_yes_no('Delete job?') == 'yes':
    batch_client.job.delete(_JOB_ID)

if query_yes_no('Delete pool?') == 'yes':
    batch_client.pool.delete(_POOL_ID)
```

> [AZURE.IMPORTANT] Pidage meeles, et ostmisega Arvuta ressursid--kustutamine kasutamata kaustu minimeerida maksumus. Arvestage ka, kas kustutamine on kustutab kõik Arvuta sõlmed sees selle pool, ning et andmeid sõlmed parandamatu pärast pool kustutatakse.

## <a name="run-the-sample-script"></a>Käivitage skripti näide

Kui käivitate *python_tutorial_client.py* skripti kuueosalisest [proovi kood][github_article_samples], konsooli väljundi sarnaneb järgmisega. Pausi juures on `Monitoring all tasks for 'Completed' state, timeout in 0:20:00...` ajal selle kausta Arvuta sõlmed luuakse alustada, ja selle kausta start tööülesande käsud on täidetud. Kasutada [Azure portaali] [ azure_portal] arvutada oma rakenduskausta jälgimiseks ja pärast Andmetäite sõlmed, töö ja tööülesandeid. Kasutada [Azure portaali] [ azure_portal] või [Microsoft Azure'i salvestusruumi Exploreri] [ storage_explorer] salvestusruumi ressursse (ümbriste ja plekid), mis on loodud rakenduse kuvamiseks.

>[AZURE.TIP] Käivitage *python_tutorial_client.py* skript kasutamine on `azure-batch-samples/Python/Batch/article_samples` kataloogi. Suhteline tee kasutab funktsiooni `common.helpers` mooduli importida, seega võite näha `ImportError: No module named 'common'` kui teil ei on selle Directory käsikirjaga.

Tüüpilised täitmisaeg on **ligikaudu 5 – 7 minutit** oma vaikimisi konfiguratsiooni valimi käivitamisel.

```
Sample start: 2016-05-20 22:47:10

Uploading file /home/user/py_tutorial/python_tutorial_task.py to container [application]...
Uploading file /home/user/py_tutorial/data/taskdata1.txt to container [input]...
Uploading file /home/user/py_tutorial/data/taskdata2.txt to container [input]...
Uploading file /home/user/py_tutorial/data/taskdata3.txt to container [input]...
Creating pool [PythonTutorialPool]...
Creating job [PythonTutorialJob]...
Adding 3 tasks to job [PythonTutorialJob]...
Monitoring all tasks for 'Completed' state, timeout in 0:20:00..........................................................................
  Success! All tasks reached the 'Completed' state within the specified timeout period.
Downloading all files from container [output]...
  Downloaded blob [taskdata1_OUTPUT.txt] from container [output] to /home/user/taskdata1_OUTPUT.txt
  Downloaded blob [taskdata2_OUTPUT.txt] from container [output] to /home/user/taskdata2_OUTPUT.txt
  Downloaded blob [taskdata3_OUTPUT.txt] from container [output] to /home/user/taskdata3_OUTPUT.txt
  Download complete!
Deleting containers...

Sample end: 2016-05-20 22:53:12
Elapsed time: 0:06:02

Delete job? [Y/n]
Delete pool? [Y/n]

Press ENTER to exit...
```

## <a name="next-steps"></a>Järgmised sammud

Julgelt *python_tutorial_client.py* ja *python_tutorial_task.py* eksperimenteerida erinevate Arvuta stsenaariumid muuta. Näiteks, proovige lisamise jaoks täitmise viivitusi *python_tutorial_task.py* simuleerida pikaajalisi tööülesanded ja jälgida neid portaalis. Proovige lisada rohkem tööülesandeid või Arvuta sõlmed arvu. Otsida ja luba kasutada mõne olemasoleva kausta kiiruse täitmise ajal loogika lisamine.

Nüüd, kui olete tuttav lihtsa töövoo paketi lahenduse, on aeg minna paketi teenuse lisafunktsioonidele.

- Lugege läbi artikkel [Ülevaade Azure'i paketi funktsioonid](batch-api-basics.md) , mis soovitatav, kui te pole teenust.
- Muude paketi arengu artikleid jaotises **arengu põhjalikumat** [õppeteema paketi]käivitamine[batch_learning_path].
- Tutvuge erinevate rakendamine töötlemine "ülemised N sõnad" töökoormus partii [TopNWords] [ github_topnwords] valimi.

[azure_batch]: https://azure.microsoft.com/services/batch/
[azure_free_account]: https://azure.microsoft.com/free/
[azure_portal]: https://portal.azure.com
[batch_learning_path]: https://azure.microsoft.com/documentation/learning-paths/batch/
[blog_linux]: http://blogs.technet.com/b/windowshpc/archive/2016/03/30/introducing-linux-support-on-azure-batch.aspx
[crypto]: https://cryptography.io/en/latest/
[crypto_install]: https://cryptography.io/en/latest/installation/
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_samples_zip]: https://github.com/Azure/azure-batch-samples/archive/master.zip
[github_topnwords]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/TopNWords
[github_article_samples]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch/article_samples

[nuget_packagemgr]: https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c
[nuget_restore]: https://docs.nuget.org/consume/package-restore/msbuild-integrated#enabling-package-restore-during-build

[py_account_ops]: http://azure-sdk-for-python.readthedocs.org/en/latest/ref/azure.batch.operations.html#azure.batch.operations.AccountOperations
[py_azure_sdk]: https://pypi.python.org/pypi/azure
[py_batch_docs]: http://azure-sdk-for-python.readthedocs.org/en/latest/ref/azure.batch.html
[py_batch_package]: https://pypi.python.org/pypi/azure-batch
[py_batchserviceclient]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.html#azure.batch.BatchServiceClient
[py_blockblobservice]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.blockblobservice.html#azure.storage.blob.blockblobservice.BlockBlobService
[py_cloudtask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.CloudTask
[py_computenodeuser]: http://azure-sdk-for-python.readthedocs.org/en/latest/ref/azure.batch.models.html#azure.batch.models.ComputeNodeUser
[py_cs_config]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.CloudServiceConfiguration
[py_delete_container]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.baseblobservice.html#azure.storage.blob.baseblobservice.BaseBlobService.delete_container
[py_gen_blob_sas]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.baseblobservice.html#azure.storage.blob.baseblobservice.BaseBlobService.generate_blob_shared_access_signature
[py_gen_container_sas]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.baseblobservice.html#azure.storage.blob.baseblobservice.BaseBlobService.generate_container_shared_access_signature
[py_image_ref]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.ImageReference
[py_imagereference]: http://azure-sdk-for-python.readthedocs.org/en/latest/ref/azure.batch.models.html#azure.batch.models.ImageReference
[py_job]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.operations.html#azure.batch.operations.JobOperations
[py_jobpreptask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.JobPreparationTask
[py_jobreltask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.JobReleaseTask
[py_list_skus]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.operations.html#azure.batch.operations.AccountOperations.list_node_agent_skus
[py_make_blob_url]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.baseblobservice.html#azure.storage.blob.baseblobservice.BaseBlobService.make_blob_url
[py_pool]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.operations.html#azure.batch.operations.PoolOperations
[py_pooladdparam]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.PoolAddParameter
[py_poolinfo]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.PoolInformation
[py_resource_file]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.ResourceFile
[py_samples_github]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch/
[py_starttask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.StartTask
[py_starttask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.StartTask
[py_task]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.CloudTask
[py_taskstate]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.TaskState
[py_vm_config]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.VirtualMachineConfiguration
[pypi_batch]: https://pypi.python.org/pypi/azure-batch
[pypi_storage]: https://pypi.python.org/pypi/azure-storage

[pypi_install]: http://python-packaging-user-guide.readthedocs.io/en/latest/installing/
[storage_explorer]: http://storageexplorer.com/
[visual_studio]: https://www.visualstudio.com/products/vs-2015-product-editions
[vm_marketplace]: https://azure.microsoft.com/marketplace/virtual-machines/

[1]: ./media/batch-python-tutorial/batch_workflow_01_sm.png "Ümbriste Azure Storage loomine"
[2]: ./media/batch-python-tutorial/batch_workflow_02_sm.png "Ümbriste tööülesande taotlus ja Sisestuskeel (andmed) failide üleslaadimine"
[3]: ./media/batch-python-tutorial/batch_workflow_03_sm.png "Rakenduskausta paketi loomine"
[4]: ./media/batch-python-tutorial/batch_workflow_04_sm.png "Töö paketi loomine"
[5]: ./media/batch-python-tutorial/batch_workflow_05_sm.png "Töö ülesannete lisamine"
[6]: ./media/batch-python-tutorial/batch_workflow_06_sm.png "Ülesannete jälgimine"
[7]: ./media/batch-python-tutorial/batch_workflow_07_sm.png "Tööülesande väljund saate alla laadida salvestusruum"
[8]: ./media/batch-python-tutorial/batch_workflow_sm.png "Paketi lahenduse töövoo (täielik diagramm)"
[9]: ./media/batch-python-tutorial/credentials_batch_sm.png "Paketi identimisteabe portaalis"
[10]: ./media/batch-python-tutorial/credentials_storage_sm.png "Salvestusruumi identimisteabe portaalis"
[11]: ./media/batch-python-tutorial/batch_workflow_minimal_sm.png "Paketi lahenduse töövoo (minimaalsete diagramm)"
