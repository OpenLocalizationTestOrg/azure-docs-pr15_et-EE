<properties
    pageTitle="Õppeteema – alustamine Azure'i paketi .NET teegi | Microsoft Azure'i"
    description="Siit saate teada, põhimõtted Azure'i paketi ja kuidas paketi teenus on näiteks sel juhul koos töötada."
    services="batch"
    documentationCenter=".net"
    authors="mmacy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="batch"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.workload="big-compute"
    ms.date="08/15/2016"
    ms.author="marsma"/>

# <a name="get-started-with-the-azure-batch-library-for-net"></a>Azure'i paketi teegi .NET kasutamise alustamine

> [AZURE.SELECTOR]
- [.NET-I](batch-dotnet-get-started.md)
- [Python](batch-python-tutorial.md)

[Azure'i paketi] põhialused[ azure_batch] ja [Paketi .NET] [ net_api] selles teemas teek nagu räägitakse C# valimi rakenduse samm-sammult. Vaatame, kuidas see rakendus valimi mõjutab paketi teenust töödelda paralleelselt töökoormus pilves, samuti, kuidas see suhtleb [Azure Storage](../storage/storage-introduction.md) faili lavastus ja otsing. Saate teada, levinud paketi rakenduse töövoo meetodite abil. Saate põhikomponentide paketi, töö, tööülesannete, kaustu, nt base mõistmiseks ja arvutada sõlmed.

![Paketi lahenduse töövoo (tavaline)][11]<br/>

## <a name="prerequisites"></a>Eeltingimused

Selles artiklis eeldatakse, et teil on C# ja Visual Studio tundma. Ka eeldatakse, et saaksite nõuetele, konto loomist on kirjeldatud allpool Azure ja paketi ja salvestusruumi teenuste jaoks.

### <a name="accounts"></a>Kontod

- **Azure'i konto**: kui teil pole veel Azure'i tellimus, [luua tasuta Azure'i konto][azure_free_account].
- **Paketi kontoga**: kui teil on Azure'i tellimus, [konto Azure'i paketi loomine](batch-account-create-portal.md).
- **Salvestusruumi konto**: lugege teemat [Loo konto salvestusruumi](../storage/storage-create-storage-account.md#create-a-storage-account) [kohta Azure salvestusruumi](../storage/storage-create-storage-account.md)kontodel.

> [AZURE.IMPORTANT] Paketi toetab praegu *ainult* **Üldine otstarve** salvestusruumi konto tüüp, nagu on kirjeldatud juhise #5 [Loo konto salvestusruumi](../storage/storage-create-storage-account.md#create-a-storage-account) rakenduses [kohta Azure salvestusruumi kontod](../storage/storage-create-storage-account.md).

### <a name="visual-studio"></a>Visual Studio

**Visual Studio 2015** valimi projekti koostamiseks peab teil olema. Leiate tasuta ja prooviversiooni versiooni Visual Studio [Visual Studio 2015 toodete ülevaade][visual_studio].

### <a name="dotnettutorial-code-sample"></a>*DotNetTutorial* koodi näidis

[DotNetTutorial] [ github_dotnettutorial] valimi on üks [azure-paketi-näidised] leitud paljude koodinäiteid[ github_samples] hoidla github. Saate alla laadida valimi hoidla avalehel nuppu **Laadi alla ZIP** või klõpsates [Azure'i paketi-näidised master.zip] [ github_samples_zip] otsene download link. Kui te olete eraldada ZIP-faili sisu, leiate lahendus järgmises kaustas:

`\azure-batch-samples\CSharp\ArticleProjects\DotNetTutorial`

### <a name="azure-batch-explorer-optional"></a>Azure'i paketi Explorer (valikuline)

[Azure'i paketi Exploreri] [ github_batchexplorer] on tasuta kasuliku, mis sisaldub [azure-paketi-näidised] [ github_samples] hoidla github. Selle õpetuse lõpuleviimiseks pole vajalik, see võib olla kasulik ajal arendamise ja silumine teie paketi lahendusi.

## <a name="dotnettutorial-sample-project-overview"></a>DotNetTutorial valimi projekti ülevaade

*DotNetTutorial* proovi kood on Visual Studio 2015 lahenduse, mis koosneb kahest projektide: **DotNetTutorial** ja **TaskApplication**.

- **DotNetTutorial** on kliendi rakendus, mis suhtleb paketi ja salvestusruumi teenuseid käivitada paralleelselt töökoormus arvutada sõlmed (virtuaalmasinates). DotNetTutorial töötab teie kohaliku töökoha.

- **TaskApplication** on programm, mis töötab Arvuta sõlmed Azure tegelikku tööd teha. Valimis, `TaskApplication.exe` sõelub teksti faili alla laadida Azure Storage (input-faili). Seejärel annab tulemiks ülemise kolme sõnu, mis kuvatakse Sisestuskeel faili sisaldava tekstifaili (väljundfail). Pärast seda loob väljundfail, TaskApplication lisatud faili Azure Storage. See muudab klientrakendusega allalaadimiseks saadaval. TaskApplication töötab samal ajal mitme Arvuta sõlmed paketi teenus.

Järgmine diagramm näitab esmane toimingud, mida klientrakendus, *DotNetTutorial*ja rakendus, mis on sooritatud tööülesanded, *TaskApplication*. Selle lihtsa töövoo on palju Arvuta lahendusi, mis on loodud paketi tüüpiline. Kuigi iga funktsioon on saadaval paketi teenuse näidata peaaegu iga paketi stsenaarium sisaldab sarnaseid protsessid.

![Töövoo paketi näide][8]<br/>

[**Samm 1.**](#step-1-create-storage-containers) Azure'i bloobimälu **ümbriste** luua.<br/>
[**Samm 2.**](#step-2-upload-task-application-and-data-files) Tööülesande taotluse failid ja Sisestuskeel failide üleslaadimiseks ümbriste.<br/>
[**Samm 3.**](#step-3-create-batch-pool) Saate luua paketi **kausta**.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**3.** Rakenduskausta **StartTask** laadib tööülesande binaarsed failid (TaskApplication) sõlmed liitumist pool.<br/>
[**Samm 4.**](#step-4-create-batch-job) Looge paketi **töö**.<br/>
[**Juhis 5.**](#step-5-add-tasks-to-job) Projekti **tööülesannete** lisada.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**5a.** Tööülesanded on ajastatud sõlmed käivitada.<br/>
    &nbsp;&nbsp;&nbsp;&nbsp;**5b.** Iga tööülesande allalaadimine oma sisendandmete Azure Storage ja seejärel algab täitmise.<br/>
[**Juhist 6.**](#step-6-monitor-tasks) Tööülesannete jälgimine.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**6a.** Ülesanded lõpetatuks üles laadida oma andmete Azure Storage.<br/>
[**Juhis 7.**](#step-7-download-task-output) Salvestusruumi tööülesande väljundi alla laadida.

Nagu mainitud, mitte iga paketi lahenduse teeb järgmist täpse ning võivad sisaldada palju muud, kuid *DotNetTutorial* valimi rakendus näitab levinud protsessid paketi lahenduse leida.

## <a name="build-the-dotnettutorial-sample-project"></a>Projekti *DotNetTutorial* valimi koostamine

Enne vaikeveebisaidiga valimi, peate määrama nii paketi ja salvestusruumi konto identimisteave *DotNetTutorial* projekti `Program.cs` faili. Kui te pole seda veel teinud, avage lahenduse Visual Studios, topeltklõpsates soovitud `DotNetTutorial.sln` lahenduse faili. Või avage selle jooksul Visual Studio abil soovitud **Fail > avamine > projekti/lahenduse** menüü.

Avatud `Program.cs` *DotNetTutorial* projekti raames. Seejärel sisestage oma kasutajanimi ja parool määratud faili ülaosas.

```
// Update the Batch and Storage account credential strings below with the values
// unique to your accounts. These are used when constructing connection strings
// for the Batch and Storage client objects.

// Batch account credentials
private const string BatchAccountName = "";
private const string BatchAccountKey  = "";
private const string BatchAccountUrl  = "";

// Storage account credentials
private const string StorageAccountName = "";
private const string StorageAccountKey  = "";
```

> [AZURE.IMPORTANT] Nagu eespool, peate määrama Azure Storage praegu **Üldine otstarve** salvestusruumi konto identimisteave. Paketi rakenduste kasutamine bloobimälu **Üldine otstarve** salvestusruumi kontol. Määrake identimisteabe salvestusruumi konto, mis on loodud, valides *bloobimälu* konto tüüp.

Paketi ja salvestusruumi konto identimisteave sees iga teenuse konto tera leiate [Azure'i portaalis][azure_portal]:

![Partii identimisteabe portaalis][9]
![salvestusruumi identimisteabe portaalis][10]<br/>

Nüüd, kui olete projekti mandaadiga värskendanud, paremklõpsake Solution Exploreris lahendus ja klõpsake **Lahenduse luua**. Kui teil palutakse, siis kinnitage mis tahes NuGet-paketid taastamine.

> [AZURE.TIP] Kui NuGet-paketid on taastatud automaatselt või kui te ei näe tõrgete kohta ei saa taastada paketid, veenduge, et on [Nugeti Package Manager] [ nuget_packagemgr] installitud. Seejärel lubada puuduvad pakettide allalaadimine. Vt [Lubamine paketi taastamine ajal koostada] [ nuget_restore] lubamiseks alla laadida.

Järgmistes jaotistes valimi rakenduse toimingud, mida see teeb töötlemine on töökoormus paketi teenus sisse murda, ja arutada neid toiminguid sellises üksikasjad. Soovitame teil viidata avatud lahenduse Visual Studios teed läbi käesoleva artikli ülejäänud töötades, kuna käsitletakse proovi kood ei igal real.

Liikuge ülaosas on `MainAsync` meetod *DotNetTutorial* projekti `Program.cs` faili Alusta etappi 1. Iga etapi allpool siis umbes järgib edenemisele meetod kõnede `MainAsync`.

## <a name="step-1-create-storage-containers"></a>Samm 1: Loo säilitus

![Ümbriste Azure Storage loomine][1]
<br/>

Pakett sisaldab tugi Azure Storage suhtlemiseks. Ümbriste teie salvestusruumi konto annab ülesandeid, mis töötavad teie paketi konto vajalikud failid. Funktsiooni ümbriste pakuvad ka salvestamiseks tööülesanded aedvili väljundi andmeid. Esimese asjana *DotNetTutorial* klientrakendusega ei on luua kolm ümbriste [Azure'i bloobimälu](../storage/storage-introduction.md).

- **rakendus**: see ümbris salvestab tööülesanded, samuti mis tahes sõltuvustega, nt dll-teekidega, käivitage rakendus.
- **Sisestuskeel**: tööülesannete andmefailid *Sisestuskeel* ümbris asukohast alla.
- **väljundi**: kui tööülesannete Sisestuskeel faili töötlemine lõpule viia, nad üles laadida tulemused *väljundi* ümbrises.

Salvestusruumi konto suhelda ja ümbriste loomiseks, kasutame [Azure'i salvestusruumi kliendi teek .net-i][net_api_storage]. Loome konto viide koos [CloudStorageAccount][net_cloudstorageaccount], ja, et luua mõne [CloudBlobClient][net_cloudblobclient]:

```csharp
// Construct the Storage account connection string
string storageConnectionString = String.Format(
    "DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}",
    StorageAccountName,
    StorageAccountKey);

// Retrieve the storage account
CloudStorageAccount storageAccount =
    CloudStorageAccount.Parse(storageConnectionString);

// Create the blob client, for use in obtaining references to
// blob storage containers
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
```

Kasutame funktsiooni `blobClient` viidata kogu taotlus ja laadige see parameetrina mitu võimalust. Järgmine näide on kood ploki vahetult järgnev ülal, kus me kutsume `CreateContainerIfNotExistAsync` soovitud ümbriste tegelikult loomiseks.

```csharp
// Use the blob client to create the containers in Azure Storage if they don't
// yet exist
const string appContainerName    = "application";
const string inputContainerName  = "input";
const string outputContainerName = "output";
await CreateContainerIfNotExistAsync(blobClient, appContainerName);
await CreateContainerIfNotExistAsync(blobClient, inputContainerName);
await CreateContainerIfNotExistAsync(blobClient, outputContainerName);
```

```csharp
private static async Task CreateContainerIfNotExistAsync(
    CloudBlobClient blobClient,
    string containerName)
{
        CloudBlobContainer container =
            blobClient.GetContainerReference(containerName);

        if (await container.CreateIfNotExistsAsync())
        {
                Console.WriteLine("Container [{0}] created.", containerName);
        }
        else
        {
                Console.WriteLine("Container [{0}] exists, skipping creation.",
                    containerName);
        }
}
```

Kui soovitud ümbriste on loodud, saate rakenduse kohe laadida faile, mis kasutavad toimingud.

> [AZURE.TIP] [Bloobimälu kaudu .net-i kasutamise kohta](../storage/storage-dotnet-how-to-use-blobs.md) leiate hea ülevaade Azure'i säilitus- ja plekid töötamine. Lugemispaanil loendi ülaosas peaks olema nagu paketi töötamist alustada.

## <a name="step-2-upload-task-application-and-data-files"></a>Samm 2: Tööülesande taotlus ja andmed failide üleslaadimine

![Ümbriste tööülesande taotlus ja Sisestuskeel (andmed) failide üleslaadimine][2]
<br/>

Faili üleslaadimine toiming, *DotNetTutorial* esmalt määratleb **rakendus** ja **Sisestuskeel** failiteed kogumeid, kui nad pole kohalikus arvutis. Seejärel lisatud need failid vastavad eelmises juhises loodud ümbriste abil.

```
// Paths to the executable and its dependencies that will be executed by the tasks
List<string> applicationFilePaths = new List<string>
{
    // The DotNetTutorial project includes a project reference to TaskApplication,
    // allowing us to determine the path of the task application binary dynamically
    typeof(TaskApplication.Program).Assembly.Location,
    "Microsoft.WindowsAzure.Storage.dll"
};

// The collection of data files that are to be processed by the tasks
List<string> inputFilePaths = new List<string>
{
    @"..\..\taskdata1.txt",
    @"..\..\taskdata2.txt",
    @"..\..\taskdata3.txt"
};

// Upload the application and its dependencies to Azure Storage. This is the
// application that will process the data files, and will be executed by each
// of the tasks on the compute nodes.
List<ResourceFile> applicationFiles = await UploadFilesToContainerAsync(
    blobClient,
    appContainerName,
    applicationFilePaths);

// Upload the data files. This is the data that will be processed by each of
// the tasks that are executed on the compute nodes within the pool.
List<ResourceFile> inputFiles = await UploadFilesToContainerAsync(
    blobClient,
    inputContainerName,
    inputFilePaths);
```

On kaks võimalust sisse `Program.cs` mis on seotud üleslaadimine:

- `UploadFilesToContainerAsync`: See meetod tagastab kogumi [ResourceFile] [ net_resourcefile] objektide (allpool) ja ettevõttesiseselt kõned `UploadFileToContainerAsync` üles laadida iga faili, mis *filePaths* parameeter.
- `UploadFileToContainerAsync`: See on meetod, mis tegelikult sooritab fail üles ja loob [ResourceFile] [ net_resourcefile] objektid. Pärast faili üleslaadimisel hangib ühiskasutusega juurdepääsu signatuuri (SAS) faili ja tagastab ResourceFile objekti, mis tähistab seda. Ühiskasutusse antud juurdepääs allkirjad, käsitletakse ka all.

```
private static async Task<ResourceFile> UploadFileToContainerAsync(
    CloudBlobClient blobClient,
    string containerName,
    string filePath)
{
        Console.WriteLine(
            "Uploading file {0} to container [{1}]...", filePath, containerName);

        string blobName = Path.GetFileName(filePath);

        CloudBlobContainer container = blobClient.GetContainerReference(containerName);
        CloudBlockBlob blobData = container.GetBlockBlobReference(blobName);
        await blobData.UploadFromFileAsync(filePath, FileMode.Open);

        // Set the expiry time and permissions for the blob shared access signature.
        // In this case, no start time is specified, so the shared access signature
        // becomes valid immediately
        SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy
        {
                SharedAccessExpiryTime = DateTime.UtcNow.AddHours(2),
                Permissions = SharedAccessBlobPermissions.Read
        };

        // Construct the SAS URL for blob
        string sasBlobToken = blobData.GetSharedAccessSignature(sasConstraints);
        string blobSasUri = String.Format("{0}{1}", blobData.Uri, sasBlobToken);

        return new ResourceFile(blobSasUri, blobName);
}
```

### <a name="resourcefiles"></a>ResourceFiles

Mõne [ResourceFile] [ net_resourcefile] pakub tööülesanded paketi Azure Storage, mis on laaditud Arvuta sõlme enne tööülesande käitatakse faili URL-i abil. [ResourceFile.BlobSource] [ net_resourcefile_blobsource] atribuut määrab faili täielik URL, nagu need on Azure Storage. Ühiskasutatava Accessi allkirja (SAS), mis pakub turvaline juurdepääs fail võib sisaldada ka URL-i. Enamik tööülesannete tüübid sees paketi .NET *ResourceFiles* atribuudi, sh:

- [CloudTask][net_task]
- [StartTask][net_pool_starttask]
- [JobPreparationTask][net_jobpreptask]
- [JobReleaseTask][net_jobreltask]

DotNetTutorial valimi rakendus ei kasuta JobPreparationTask või JobReleaseTask ülesandetüüpide hostimine, kuid saate neid [käivitada töö ettevalmistamine ja lõpetamise tööülesannete Azure'i paketi arvutada sõlmed](batch-job-prep-release.md)kohta.

### <a name="shared-access-signature-sas"></a>Ühiskasutusega juurdepääsu allkirja (SAS)

Ühiskasutusega juurdepääsu allkirjad on stringid, mis – kui URL-i osana – tagada turvaline juurdepääs ümbriste ja plekid Azure Storage. DotNetTutorial, rakendus kasutab nii bloobimälu ja ühiskasutuses container juurdepääsu allkirja URL-id ja näitab, kuidas saada stringide ühiskasutusega juurdepääsu allkirja salvestusruumi teenus.

- **Bloobimälu ühiskasutusse Accessi allkirjad**: selle kausta StartTask sisse DotNetTutorial kasutab bloobimälu ühiskasutusse Accessi allkirjad, kui seda allalaadimist Binaarfailid ja sisendandmete failide salvestusruumi (vt allpool samm #3). Funktsiooni `UploadFileToContainerAsync` meetod DotNetTutorial's `Program.cs` sisaldab koodi, mis hangib iga bloobimälu ühiskasutusega juurdepääsu allkirja. Seda tehes helistades [CloudBlob.GetSharedAccessSignature][net_sas_blob].

- **Container ühiskasutusse Accessi allkirjad**: Kuna iga tööülesande lõpetab oma töö Arvuta sõlme, see lisatud selle väljundfail *väljundi* container Azure Storage. Selleks kasutab TaskApplication container ühiskasutusega juurdepääsu allkirja, mis pakub ümbris kirjutusõiguse tee osana, kui see on lisatud faili. Container ühiskasutusega juurdepääsu allkirja saamiseks tehakse samal viisil kui kui saamiseks selle bloobimälu ühiskasutusse antud juurdepääs allkirja. DotNetTutorial, leiate nii on `GetContainerSasUrl` helper nõuab [CloudBlobContainer.GetSharedAccessSignature] [ net_sas_container] seda teha. Saate lugeda Lisateavet selle kohta, kuidas TaskApplication kasutab ümbris ühiskasutusse Accessi allkirja "juhist 6: kuvari tööülesannete."

> [AZURE.TIP] Kaheosaline sarjas ühiskasutusega juurdepääsu allkirjad, tutvuge [osa 1: mõistmine ühiskasutusega juurdepääsu allkirja (SAS) mudeli](../storage/storage-dotnet-shared-access-signature-part-1.md) ja [osa 2: loomine ja kasutamine bloobimälu ühiskasutusega juurdepääsu signatuuri (SAS)](../storage/storage-dotnet-shared-access-signature-part-2.md), lisateavet turvalist juurdepääsu teie salvestusruumi andmeid.

## <a name="step-3-create-batch-pool"></a>Samm 3: Looge paketi pool

![Paketi kausta loomine][3]
<br/>

Paketi **rakenduskausta** on kogum Arvuta sõlmed (virtuaalmasinates) kohta, mis käivitab paketi lisamine tööülesannetest.

Pärast seda lisatud rakendus ja andmete failide salvestusruumi konto, käivitub *DotNetTutorial* paketi teenuse koostööd teegi paketi .net-i abil. Selleks on [BatchClient] [ net_batchclient] esmalt luuakse:

```csharp
BatchSharedKeyCredentials cred = new BatchSharedKeyCredentials(
    BatchAccountUrl,
    BatchAccountName,
    BatchAccountKey);

using (BatchClient batchClient = BatchClient.Open(cred))
{
    ...
```

Järgmiseks kogumi Arvuta sõlmed on loodud paketi konto kõne `CreatePoolAsync`. `CreatePoolAsync`kasutab [BatchClient.PoolOperations.CreatePool] [ net_pool_create] meetod on tegelikult teenuse paketi loomine.

```csharp
private static async Task CreatePoolAsync(
    BatchClient batchClient,
    string poolId,
    IList<ResourceFile> resourceFiles)
{
    Console.WriteLine("Creating pool [{0}]...", poolId);

    // Create the unbound pool. Until we call CloudPool.Commit() or CommitAsync(),
    // no pool is actually created in the Batch service. This CloudPool instance is
    // therefore considered "unbound," and we can modify its properties.
    CloudPool pool = batchClient.PoolOperations.CreatePool(
            poolId: poolId,
            targetDedicated: 3,           // 3 compute nodes
            virtualMachineSize: "small",  // single-core, 1.75 GB memory, 224 GB disk
            cloudServiceConfiguration:
                new CloudServiceConfiguration(osFamily: "4")); // Win Server 2012 R2

    // Create and assign the StartTask that will be executed when compute nodes join
    // the pool. In this case, we copy the StartTask's resource files (that will be
    // automatically downloaded to the node by the StartTask) into the shared
    // directory that all tasks will have access to.
    pool.StartTask = new StartTask
    {
        // Specify a command line for the StartTask that copies the task application
        // files to the node's shared directory. Every compute node in a Batch pool
        // is configured with several pre-defined environment variables that you can
        // reference by using commands or applications run by tasks.

        // Since a successful execution of robocopy can return a non-zero exit code
        // (e.g. 1 when one or more files were successfully copied) we need to
        // manually exit with a 0 for Batch to recognize StartTask execution success.
        CommandLine = "cmd /c (robocopy %AZ_BATCH_TASK_WORKING_DIR% %AZ_BATCH_NODE_SHARED_DIR%) ^& IF %ERRORLEVEL% LEQ 1 exit 0",
        ResourceFiles = resourceFiles,
        WaitForSuccess = true
    };

    await pool.CommitAsync();
}
```

Kui loote on [CreatePool][net_pool_create], saate määrata mitu parameetrite arvu Arvuta sõlmed, [sõlmed mahtu](../cloud-services/cloud-services-sizes-specs.md)ja selle sõlmed operatsioonisüsteemi. *DotNetTutorial*, kasutame [CloudServiceConfiguration] [ net_cloudserviceconfiguration] Windows Server 2012 R2: [Pilveteenuste](../cloud-services/cloud-services-guestos-update-matrix.md)määramiseks. Siiski määramisega on [VirtualMachineConfiguration] [ net_virtualmachineconfiguration] selle asemel saate luua sõlmed loodud turuplatsi pilte, mis sisaldab nii Windows ja Linux pildid – lisateabe saamiseks vaadake [sätte Linux arvutada sõlmed Azure'i paketi kaustadesse](batch-linux-nodes.md) .

> [AZURE.IMPORTANT] Arvuta ressursside paketi ostmisega. Kulude minimeerimiseks saab alumine `targetDedicated` 1 enne valimi käivitamist.

Koos füüsilise sõlm atribuutidest, saate määrata ka mõne [StartTask] [ net_pool_starttask] mõeldud pool. Funktsiooni StartTask aktiveeritakse iga sõlme sõlme ühendab pool ja iga kord, kui sõlm taaskäivitamist. Funktsiooni StartTask on eriti kasulik rakenduste installimine Arvuta sõlmed enne ülesannete täitmiseks. Näiteks kui tööülesannete töödelda Python skriptide abil, võib kasutada mõne StartTask installimiseks Python Arvuta sõlmed.

Valimi rakenduses selle StartTask kopeerib salvestusruumist allalaaditavate failide (mis on määratud, kasutades [StartTask][net_starttask].[ ResourceFiles] [ net_starttask_resourcefiles] atribuudi) ühiskasutusse antud kausta, mis töötab sõlme *Kõik* tööülesanded pääsete juurde kataloogist StartTask töötamine. Sisuliselt kopeeritakse `TaskApplication.exe` ning ühiskasutusega kataloogi iga sõlme sõltuvustega nagu sõlme ühendab pool, nii, et kõik toimingud, mis töötavad sõlme sellele juurde pääsevad.

> [AZURE.TIP] Azure'i paketi **rakenduse paketid** funktsioon pakub teine võimalus teie rakendus peale Arvuta sõlmed pargis. Üksikasjad leiate [Azure'i paketi rakenduse paketid rakenduse juurutamine](batch-application-packages.md) .

Ka ülaltoodud koodilõigu märgatav on kaks keskkonna muutujate *käsureal* atribuuti on StartTask kasutamine: `%AZ_BATCH_TASK_WORKING_DIR%` ja `%AZ_BATCH_NODE_SHARED_DIR%`. Mitut muutujat keskkond, mis on omased paketi on konfigureeritud automaatselt iga MSDN paketi rakenduskausta sees. Mis tahes protsess, mis on sooritatud toimingu on juurdepääs nende keskkonna muutujate.

> [AZURE.TIP] Keskkonna kohta rohkem teada saadaolevad Arvuta sõlmed paketi rakenduskausta ja teavet tööülesande töötamise kataloogide, muutujate teemades [keskkonna sätteid ülesannete jaoks,](batch-api-basics.md#environment-settings-for-tasks) ja [failide ja kataloogide](batch-api-basics.md#files-and-directories) [paketi funktsioon ülevaade arendajatele](batch-api-basics.md).

## <a name="step-4-create-batch-job"></a>Samm 4: Looge pakett

![Töö paketi loomine][4]<br/>

Paketi **töö** on tööülesannete kogumi ning on seostatud kogumi Arvuta sõlmed. Projekti tööülesanded täita seotud pool Arvuta sõlmed.

Saate töö, mitte ainult korraldamine ja seotud töökoormus, aga ka teatud piirangud – näiteks maksimaalne käitusaja (töö ja laiend, ülesannete) ning muud tööd konto pakett tähtsuse järjekorras töö määramise tööülesannete jälgimine. Selles näites siiski töö on seostatud ainult pool, mis loodi juhises #3. Täiendavad atribuudid pole konfigureeritud.

Kõik paketi tööd on seotud konkreetse pool. See ühendus näitab, millised sõlmed projekti tööülesanded on käivitada. Saate määrata selle abil [CloudJob.PoolInformation] [ net_job_poolinfo] atribuudi, nagu on näidatud allpool koodilõigu.

```csharp
private static async Task CreateJobAsync(
    BatchClient batchClient,
    string jobId,
    string poolId)
{
    Console.WriteLine("Creating job [{0}]...", jobId);

    CloudJob job = batchClient.JobOperations.CreateJob();
    job.Id = jobId;
    job.PoolInformation = new PoolInformation { PoolId = poolId };

    await job.CommitAsync();
}
```

Nüüd, kui tööd on loodud, lisatakse tööülesannete töö tegemiseks.

## <a name="step-5-add-tasks-to-job"></a>Juhis 5: Lisada tööülesandeid töö

![Töö ülesannete lisamine][5]<br/>
*(1) ülesanded lisatakse töö, (2) tööülesanded on ajastatud sõlmed ja (3) tööülesanded allalaadimine andmefailid töötlemiseks*

Paketi **Tööülesanded** on üksikute üksuste töö, et käivitada Arvuta sõlmed. Tööülesande on mõne käsurea ja käivitab skriptide või täitmisfailid, kus saate määrata selle käsurea.

Tegelikku tööd teha, peab ülesanded lisatakse tööd. Iga [CloudTask] [ net_task] on konfigureeritud, kasutades käsurea atribuut ja [ResourceFiles] [ net_task_resourcefiles] (nagu on pool StartTask) mis tööülesande allalaadimist sõlm enne selle käsurea täidetakse automaatselt. Projectis *DotNetTutorial* valimi iga tööülesande töötleb ainult üks fail. Seega selle ResourceFiles kogu sisaldab ühe elemendi.

```csharp
private static async Task<List<CloudTask>> AddTasksAsync(
    BatchClient batchClient,
    string jobId,
    List<ResourceFile> inputFiles,
    string outputContainerSasUrl)
{
    Console.WriteLine("Adding {0} tasks to job [{1}]...", inputFiles.Count, jobId);

    // Create a collection to hold the tasks that we'll be adding to the job
    List<CloudTask> tasks = new List<CloudTask>();

    // Create each of the tasks. Because we copied the task application to the
    // node's shared directory with the pool's StartTask, we can access it via
    // the shared directory on the node that the task runs on.
    foreach (ResourceFile inputFile in inputFiles)
    {
        string taskId = "topNtask" + inputFiles.IndexOf(inputFile);
        string taskCommandLine = String.Format(
            "cmd /c %AZ_BATCH_NODE_SHARED_DIR%\\TaskApplication.exe {0} 3 \"{1}\"",
            inputFile.FilePath,
            outputContainerSasUrl);

        CloudTask task = new CloudTask(taskId, taskCommandLine);
        task.ResourceFiles = new List<ResourceFile> { inputFile };
        tasks.Add(task);
    }

    // Add the tasks as a collection, as opposed to issuing a separate AddTask call
    // for each. Bulk task submission helps to ensure efficient underlying API calls
    // to the Batch service.
    await batchClient.JobOperations.AddTaskAsync(jobId, tasks);

    return tasks;
}
```

> [AZURE.IMPORTANT] Kui nad juurde keskkonna muutujate nagu `%AZ_BATCH_NODE_SHARED_DIR%` või käivitada rakendus ei leia soovitud sõlm `PATH`, tööülesande käsk read peavad olema eesliide `cmd /c`. See konkreetselt käivitada käsk Tõlgi ja paluge see pärast teie käsk lõpetada. See tingimus on mittevajalike kui tööülesannete käivitada rakendus on sõlme `PATH` (nt *robocopy.exe* või *powershell.exe*) ja pole keskkonna muutujate kasutamist.

Sees on `foreach` tsükkel ülaltoodud koodilõigu, saate vaadata tööülesande käsurea ehitatakse nii, et *TaskApplication.exe*edastatakse kolme käsurea argumendid:

1. **Esimene argument** on faili töötlemiseks. See on kohaliku faili tee, nagu need on sõlme. Kui soovitud ResourceFile objekt lehel `UploadFileToContainerAsync` loomist, selle asemel faili nime kasutati selle atribuudi (parameetrina ResourceFile ehitaja). See näitab, et fail võib leida *TaskApplication.exe*samas kataloogis.

2. **Teine argument** määrab, et ülemised *N* sõnad tuleks alati kirjutada väljundfail. Valimis, seda on raske-koodiga, et ülemise kolme sõna kirjutatakse väljundfail.

3. **Kolmas argument** on ühiskasutatava Accessi allkirja (SAS), mis pakub **väljundi** container Azure Storage kirjutusõiguse. *TaskApplication.exe* kasutab seda ühiskasutusse antud juurdepääs allkirja URL-i, kui see lisatud väljundfail Azure Storage. Võite leida koodi seda funktsiooni `UploadFileToContainer` meetod TaskApplication projekti `Program.cs` faili:

```csharp
// NOTE: From project TaskApplication Program.cs

private static void UploadFileToContainer(string filePath, string containerSas)
{
        string blobName = Path.GetFileName(filePath);

        // Obtain a reference to the container using the SAS URI.
        CloudBlobContainer container = new CloudBlobContainer(new Uri(containerSas));

        // Upload the file (as a new blob) to the container
        try
        {
                CloudBlockBlob blob = container.GetBlockBlobReference(blobName);
                blob.UploadFromFile(filePath, FileMode.Open);

                Console.WriteLine("Write operation succeeded for SAS URL " + containerSas);
                Console.WriteLine();
        }
        catch (StorageException e)
        {

                Console.WriteLine("Write operation failed for SAS URL " + containerSas);
                Console.WriteLine("Additional error information: " + e.Message);
                Console.WriteLine();

                // Indicate that a failure has occurred so that when the Batch service
                // sets the CloudTask.ExecutionInformation.ExitCode for the task that
                // executed this application, it properly indicates that there was a
                // problem with the task.
                Environment.ExitCode = -1;
        }
}
```

## <a name="step-6-monitor-tasks"></a>Samm 6: Kuvari tööülesannete

![Ülesannete jälgimine][6]<br/>
*Klientrakenduse (1) jälgib ülesannete täitmise ja edu olek ja (2) tööülesannete Azure Storage tulemi andmete üleslaadimine*

Kui tööülesanded on lisatud tööd, nad on automaatselt ootele ja ajastatud täitmiseks Arvuta sõlmed maksimaalselt pool tööga seotud. Vastavalt teie määratud sätteid, paketi tegeleb kõik tööülesande queuing, ajastamise, proovitakse uuesti ja muid tööülesande halduse ülesandeid teie eest. On mitmeid võimalusi tööülesande täitmise jälgimiseks. DotNetTutorial kuvatakse aruanded ainult lõpetamise ja tööülesannete tõrge või edu olekus lihtsas näites.

Sees on `MonitorTasks` meetod DotNetTutorial's `Program.cs`, on kolm paketi .NET mõistet, et arutelu. Need on loetletud allpool nende ilmet järjestuses:

1. **ODATADetailLevel**: täpsustades [ODATADetailLevel] [ net_odatadetaillevel] loendis toiminguid (nt on tööülesannete loendi saamiseks) on oluline paketi rakenduse jõudluse tagamiseks. [Päringu Azure'i paketi teenuse tõhus](batch-efficient-list-queries.md) lugemine loendisse lisada kui kavatsete tehes mingit oleku teie paketi rakenduste jälgimine.

2. **TaskStateMonitor**: [TaskStateMonitor] [ net_taskstatemonitor] pakub helper Utiliidid paketi .NET rakenduste jälgimine tööülesande olekus. Klõpsake `MonitorTasks`, *DotNetTutorial* ootab kõik tööülesanded jõuda [TaskState.Completed] [ net_taskstate] aja jooksul. Selle lõpetab töö.

3. **TerminateJobAsync**: lõpetamise töökoha [JobOperations.TerminateJobAsync] [ net_joboperations_terminatejob] (või blokeerimise JobOperations.TerminateJob) märgib selle töö lõpetatuks. On vaja teha, kui teie paketi lahendus on kasutusel on [JobReleaseTask][net_jobreltask]. See on eri tüüpi tööülesande, mida on kirjeldatud [ettevalmistamine ja lõpetamise tööülesanded](batch-job-prep-release.md).

Funktsiooni `MonitorTasks` meetodi *DotNetTutorial* `Program.cs` all kuvatakse:

```csharp
private static async Task<bool> MonitorTasks(
    BatchClient batchClient,
    string jobId,
    TimeSpan timeout)
{
    bool allTasksSuccessful = true;
    const string successMessage = "All tasks reached state Completed.";
    const string failureMessage = "One or more tasks failed to reach the Completed state within the timeout period.";

    // Obtain the collection of tasks currently managed by the job. Note that we use
    // a detail level to  specify that only the "id" property of each task should be
    // populated. Using a detail level for all list operations helps to lower
    // response time from the Batch service.
    ODATADetailLevel detail = new ODATADetailLevel(selectClause: "id");
    List<CloudTask> tasks =
        await batchClient.JobOperations.ListTasks(JobId, detail).ToListAsync();

    Console.WriteLine("Awaiting task completion, timeout in {0}...",
        timeout.ToString());

    // We use a TaskStateMonitor to monitor the state of our tasks. In this case, we
    // will wait for all tasks to reach the Completed state.
    TaskStateMonitor taskStateMonitor
        = batchClient.Utilities.CreateTaskStateMonitor();

    try
    {
        await taskStateMonitor.WhenAll(tasks, TaskState.Completed, timeout);
    }
    catch (TimeoutException)
    {
        await batchClient.JobOperations.TerminateJobAsync(jobId, failureMessage);
        Console.WriteLine(failureMessage);
        return false;
    }

    await batchClient.JobOperations.TerminateJobAsync(jobId, successMessage);

    // All tasks have reached the "Completed" state, however, this does not
    // guarantee all tasks completed successfully. Here we further check each task's
    // ExecutionInfo property to ensure that it did not encounter a scheduling error
    // or return a non-zero exit code.

    // Update the detail level to populate only the task id and executionInfo
    // properties. We refresh the tasks below, and need only this information for
    // each task.
    detail.SelectClause = "id, executionInfo";

    foreach (CloudTask task in tasks)
    {
        // Populate the task's properties with the latest info from the
        // Batch service
        await task.RefreshAsync(detail);

        if (task.ExecutionInformation.SchedulingError != null)
        {
            // A scheduling error indicates a problem starting the task on the node.
            // It is important to note that the task's state can be "Completed," yet
            // still have encountered a scheduling error.

            allTasksSuccessful = false;

            Console.WriteLine("WARNING: Task [{0}] encountered a scheduling error: {1}",
                task.Id,
                task.ExecutionInformation.SchedulingError.Message);
        }
        else if (task.ExecutionInformation.ExitCode != 0)
        {
            // A non-zero exit code may indicate that the application executed by
            // the task encountered an error during execution. As not every
            // application returns non-zero on failure by default (e.g. robocopy),
            // your implementation of error checking may differ from this example.

            allTasksSuccessful = false;

            Console.WriteLine("WARNING: Task [{0}] returned a non-zero exit code - this may indicate task execution or completion failure.", task.Id);
        }
    }

    if (allTasksSuccessful)
    {
        Console.WriteLine("Success! All tasks completed successfully within the specified timeout period.");
    }

    return allTasksSuccessful;
}
```

## <a name="step-7-download-task-output"></a>Juhis 7: Allalaadimine tööülesande väljund

![Tööülesande väljund saate alla laadida salvestusruum][7]<br/>

Nüüd, kui töö on lõpule viidud, tööülesanded väljund saate alla laadida Azure Storage. Seda saab teha kõne `DownloadBlobsFromContainerAsync` sisse *DotNetTutorial* `Program.cs`:

```csharp
private static async Task DownloadBlobsFromContainerAsync(
    CloudBlobClient blobClient,
    string containerName,
    string directoryPath)
{
        Console.WriteLine("Downloading all files from container [{0}]...", containerName);

        // Retrieve a reference to a previously created container
        CloudBlobContainer container = blobClient.GetContainerReference(containerName);

        // Get a flat listing of all the block blobs in the specified container
        foreach (IListBlobItem item in container.ListBlobs(
                    prefix: null,
                    useFlatBlobListing: true))
        {
                // Retrieve reference to the current blob
                CloudBlob blob = (CloudBlob)item;

                // Save blob contents to a file in the specified folder
                string localOutputFile = Path.Combine(directoryPath, blob.Name);
                await blob.DownloadToFileAsync(localOutputFile, FileMode.Create);
        }

        Console.WriteLine("All files downloaded to {0}", directoryPath);
}
```

> [AZURE.NOTE] Kõne suunamiseks `DownloadBlobsFromContainerAsync` *DotNetTutorial* rakenduse määrab, kas failid tuleks laaditakse teie `%TEMP%` kausta. Võite muuta selle Väljastuskoht.

## <a name="step-8-delete-containers"></a>Samm 8: Kustuta ümbriste

Kuna ostmisega Azure Storage andmeid, on alati on hea mõte kasutada mis tahes plekid, mis ei ole enam vaja oma pakett-tööde eemaldada. Klõpsake DotNetTutorial's `Program.cs`, seda saab teha kolme kõned helper meetodit `DeleteContainerAsync`:

```csharp
// Clean up Storage resources
await DeleteContainerAsync(blobClient, appContainerName);
await DeleteContainerAsync(blobClient, inputContainerName);
await DeleteContainerAsync(blobClient, outputContainerName);
```

Meetodi kohta vaid hangib ümbris viide ja seejärel kutsub [CloudBlobContainer.DeleteIfExistsAsync][net_container_delete]:

```csharp
private static async Task DeleteContainerAsync(
    CloudBlobClient blobClient,
    string containerName)
{
    CloudBlobContainer container = blobClient.GetContainerReference(containerName);

    if (await container.DeleteIfExistsAsync())
    {
        Console.WriteLine("Container [{0}] deleted.", containerName);
    }
    else
    {
        Console.WriteLine("Container [{0}] does not exist, skipping deletion.",
            containerName);
    }
}
```

## <a name="step-9-delete-the-job-and-the-pool"></a>Samm 9: Töö ja kausta kustutamine

Lõplik etapis küsitakse kasutaja töö ja kausta, mis on loodud DotNetTutorial rakenduse kustutamine. Kuigi pole ostmisega ülesannete ise, mida *on* ja Arvuta sõlmed eest. Seetõttu soovitame, et sõlmed eraldada ainult vastavalt vajadusele. Kasutamata kaustu kustutamine võib olla teie hoolduse käigus.

Funktsiooni BatchClient [JobOperations] [ net_joboperations] ja [PoolOperations] [ net_pooloperations] mõlemad on vastava kustutamise meetodid, mida nimetatakse kui kinnitab kasutaja kustutamise:

```csharp
// Clean up the resources we've created in the Batch account if the user so chooses
Console.WriteLine();
Console.WriteLine("Delete job? [yes] no");
string response = Console.ReadLine().ToLower();
if (response != "n" && response != "no")
{
    await batchClient.JobOperations.DeleteJobAsync(JobId);
}

Console.WriteLine("Delete pool? [yes] no");
response = Console.ReadLine();
if (response != "n" && response != "no")
{
    await batchClient.PoolOperations.DeletePoolAsync(PoolId);
}
```

> [AZURE.IMPORTANT] Pidage meeles, mida teilt Arvuta ressursid – kustutamine kasutamata kaustu minimeerida maksumus. Ka, Pange tähele, et kustutada on kustutab kõik Arvuta sõlmed sees selle kausta ja et sõlmed andmeid saab parandamatu pärast kustutatakse kausta.

## <a name="run-the-dotnettutorial-sample"></a>Käivitage *DotNetTutorial* näidis

Valimi rakenduse käivitamisel konsooli väljund on järgmine. Täitmisel, ilmneda pausi veebisaidil `Awaiting task completion, timeout in 00:30:00...` ajal selle kausta Arvuta sõlmed käivitatud. Kasutada [Azure portaali] [ azure_portal] arvutada jälgida oma pool, ajal ja pärast Andmetäite sõlmed, töö ja tööülesandeid. Kasutada [Azure portaali] [ azure_portal] või [Azure salvestusruumi Exploreri] [ storage_explorers] salvestusruumi ressursid (ümbriste ja plekid), mis on loodud rakenduse kuvamiseks.

Rakenduse käivitamisel oma vaikimisi konfiguratsiooni, on tüüpilised täitmisaeg **ligikaudu 5 minutit** .

```
Sample start: 1/8/2016 09:42:58 AM

Container [application] created.
Container [input] created.
Container [output] created.
Uploading file C:\repos\azure-batch-samples\CSharp\ArticleProjects\DotNetTutorial\bin\Debug\TaskApplication.exe to container [application]...
Uploading file Microsoft.WindowsAzure.Storage.dll to container [application]...
Uploading file ..\..\taskdata1.txt to container [input]...
Uploading file ..\..\taskdata2.txt to container [input]...
Uploading file ..\..\taskdata3.txt to container [input]...
Creating pool [DotNetTutorialPool]...
Creating job [DotNetTutorialJob]...
Adding 3 tasks to job [DotNetTutorialJob]...
Awaiting task completion, timeout in 00:30:00...
Success! All tasks completed successfully within the specified timeout period.
Downloading all files from container [output]...
All files downloaded to C:\Users\USERNAME\AppData\Local\Temp
Container [application] deleted.
Container [input] deleted.
Container [output] deleted.

Sample end: 1/8/2016 09:47:47 AM
Elapsed time: 00:04:48.5358142

Delete job? [yes] no: yes
Delete pool? [yes] no: yes

Sample complete, hit ENTER to exit...
```

## <a name="next-steps"></a>Järgmised sammud

Julgelt *DotNetTutorial* ja *TaskApplication* eksperimenteerida erinevate Arvuta stsenaariumid muuta. Näiteks proovida, lisades mõne täitmise viivitus *TaskApplication*, näiteks [Thread.Sleep][net_thread_sleep], et simuleerida pikaajalisi tööülesanded ja jälgida neid portaalis. Proovige lisada rohkem tööülesandeid või Arvuta sõlmed arvu. Otsida ja luba kasutada mõne olemasoleva kausta kiiruse täitmise ajal loogika lisamine (*vihje*: vaadake `ArticleHelpers.cs` sisse [Microsoft.Azure.Batch.Samples.Common] [ github_samples_common] [azure-paketi-näidised]projekti[github_samples]).

Nüüd, kui olete tuttav lihtsa töövoo paketi lahenduse, on aeg minna paketi teenuse lisafunktsioonidele.

- Lugege [paketi funktsioon ülevaade arendajatele](batch-api-basics.md), mida soovitame kõik uue paketi kasutajate jaoks.
- Muude paketi arengu artikleid jaotises **arengu põhjalikumat** [õppeteema paketi]käivitamine[batch_learning_path].
- Tutvuge erinevate rakendamine töötlemine "ülemised N sõnad" töökoormus, kasutades paketi [TopNWords] [ github_topnwords] valimi.

[azure_batch]: https://azure.microsoft.com/services/batch/
[azure_free_account]: https://azure.microsoft.com/free/
[azure_portal]: https://portal.azure.com
[batch_learning_path]: https://azure.microsoft.com/documentation/learning-paths/batch/
[github_batchexplorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[github_dotnettutorial]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/DotNetTutorial
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_samples_common]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/Common
[github_samples_zip]: https://github.com/Azure/azure-batch-samples/archive/master.zip
[github_topnwords]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/TopNWords
[net_api]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[net_api_storage]: https://msdn.microsoft.com/library/azure/mt347887.aspx
[net_batchclient]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudblobclient]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.cloudblobclient.aspx
[net_cloudblobcontainer]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.cloudblobcontainer.aspx
[net_cloudstorageaccount]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.cloudstorageaccount.aspx
[net_cloudserviceconfiguration]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudserviceconfiguration.aspx
[net_container_delete]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.cloudblobcontainer.deleteifexistsasync.aspx
[net_job]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_job_poolinfo]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.protocol.models.cloudjob.poolinformation.aspx
[net_joboperations]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.joboperations
[net_joboperations_terminatejob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.terminatejobasync.aspx
[net_jobpreptask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobpreparationtask.aspx
[net_jobreltask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobreleasetask.aspx
[net_node]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.aspx
[net_odatadetaillevel]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.aspx
[net_pool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_pool_create]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.createpool.aspx
[net_pool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx
[net_pooloperations]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.pooloperations
[net_resourcefile]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.resourcefile.aspx
[net_resourcefile_blobsource]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.resourcefile.blobsource.aspx
[net_sas_blob]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblob.getsharedaccesssignature.aspx
[net_sas_container]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.blob.cloudblobcontainer.getsharedaccesssignature.aspx
[net_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.starttask.aspx
[net_starttask_resourcefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.starttask.resourcefiles.aspx
[net_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
[net_task_resourcefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.resourcefiles.aspx
[net_taskstate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.common.taskstate.aspx
[net_taskstatemonitor]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskstatemonitor.aspx
[net_thread_sleep]: https://msdn.microsoft.com/library/274eh01d(v=vs.110).aspx
[net_virtualmachineconfiguration]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.virtualmachineconfiguration.aspx
[nuget_packagemgr]: https://docs.nuget.org/consume/installing-nuget
[nuget_restore]: https://docs.nuget.org/consume/package-restore/msbuild-integrated#enabling-package-restore-during-build
[storage_explorers]: http://storageexplorer.com/
[visual_studio]: https://www.visualstudio.com/products/vs-2015-product-editions

[1]: ./media/batch-dotnet-get-started/batch_workflow_01_sm.png "Ümbriste Azure Storage loomine"
[2]: ./media/batch-dotnet-get-started/batch_workflow_02_sm.png "Ümbriste tööülesande taotlus ja Sisestuskeel (andmed) failide üleslaadimine"
[3]: ./media/batch-dotnet-get-started/batch_workflow_03_sm.png "Rakenduskausta paketi loomine"
[4]: ./media/batch-dotnet-get-started/batch_workflow_04_sm.png "Töö paketi loomine"
[5]: ./media/batch-dotnet-get-started/batch_workflow_05_sm.png "Töö ülesannete lisamine"
[6]: ./media/batch-dotnet-get-started/batch_workflow_06_sm.png "Ülesannete jälgimine"
[7]: ./media/batch-dotnet-get-started/batch_workflow_07_sm.png "Tööülesande väljund saate alla laadida salvestusruum"
[8]: ./media/batch-dotnet-get-started/batch_workflow_sm.png "Paketi lahenduse töövoo (täielik diagramm)"
[9]: ./media/batch-dotnet-get-started/credentials_batch_sm.png "Paketi identimisteabe portaalis"
[10]: ./media/batch-dotnet-get-started/credentials_storage_sm.png "Salvestusruumi identimisteabe portaalis"
[11]: ./media/batch-dotnet-get-started/batch_workflow_minimal_sm.png "Paketi lahenduse töövoo (minimaalsete diagramm)"
