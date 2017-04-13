<properties
    pageTitle="Azure'i paketi MPI rakendusi käivitada mitme eksemplari ülesannetega | Microsoft Azure'i"
    description="Saate teada, kuidas mitme eksemplari ülesande tüüp kasutamine Azure'i paketi sõnumi läbides kasutajaliidese (MPI) rakendusi käivitada."
    services="batch"
    documentationCenter=".net"
    authors="mmacy"
    manager="timlt"
    editor="" />

<tags
    ms.service="batch"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="big-compute"
    ms.date="10/21/2016"
    ms.author="marsma" />

# <a name="use-multi-instance-tasks-to-run-message-passing-interface-mpi-applications-in-azure-batch"></a>Mitme eksemplari tööülesannete kasutada sõnumi läbides kasutajaliidese (MPI) rakenduste käivitamiseks Azure'i paketi

Mitme eksemplari tööülesanded võimaldavad Azure'i paketi toimingu korraga mitme Arvuta sõlmed käivitada. Järgmised toimingud lubada suure jõudlusega arvutuste stsenaariumid nagu paketi rakenduste sõnumi läbides kasutajaliidese (MPI). Sellest artiklist saate teada, kuidas [Paketi .net-i] abil mitme eksemplari ülesannete täitmiseks[ api_net] teek.

>[AZURE.NOTE] Kuigi selle artikli näited keskenduda paketi .NET, MS-MPI ja Windows arvutada sõlmed, mitme eksemplari tööülesande põhimõtet siin kehtivad muude platvormide jaoks loodud ja hõlbustusvahendite (Python ja Linux sõlmed, nt Inteli MPI).

## <a name="multi-instance-task-overview"></a>Mitme eksemplari ülesande ülevaade

Paketi, iga tööülesande on tavaliselt ühe MSDN - täide esitamisel mitu ülesannet töö ja paketi teenuse koosolekut kavandava iga tööülesande täitmiseks sõlme. Siiski on tööülesande **mitme eksemplari sätete**konfigureerimisega annate paketi loomiseks hoopis üks esmane ülesanne ja mitu alamülesannete, mis siis täidetakse mitme sõlmed.

![Mitme eksemplari ülesande ülevaade][1]

Kui saadate tööülesande töö mitme eksemplari sätetega, sooritab paketi mitme eksemplari tööülesannete kordumatu mitu toimingut.

1. Paketi teenus loob ühe **esmane** ja mitu **alamülesanded** põhjal mitme eksemplari sätted. Tööülesannete (primaarne pluss Kõik alamülesanded) arv vastab mitu **eksemplari** (Arvuta sõlmed) teie määratud mitme eksemplari sätteid.
1. Paketi määrab ühe Arvuta sõlmed nimega soovitud **juhtslaidi**ja esmane ülesande juhtslaidi käivitada. See koosolekut kavandava alamülesanded Arvuta sõlmed eraldatud mitme eksemplari tööülesandega, üks alamtoiming ühe sõlme ülejäänud käivitada.
1. Esmast ja kõik alamülesanded mitme eksemplari sätetes määratud **levinud ressurss faile** alla laadida.
1. Pärast levinud ressursi allalaaditud, esmast ja alamülesannete mitme eksemplari sätetes määratud **koordineerituna käsu** käivitamine. Käsk koordineerituna kasutatakse tavaliselt ülesande täitmine sõlmed ettevalmistamine. See võib hõlmata tausta teenuste käivitamine (nt [Microsoft MPI][msmpi_msdn]'s `smpd.exe`) ja kontrollida, et sõlmed vaheline sõlm sõnumite saatmiseks valmis.
1. Esmane ülesanne käivitab **käsku** soovitud juhtslaidi sõlm *pärast* koordineerituna käsk on edukalt lõpule, esmast ja kõik alamülesanded. Käsk rakenduses on mitme eksemplari ülesanne ise käsurea ja täita ainult esmane ülesanne. Klõpsake mõnda [MS-MPI][msmpi_msdn]-põhise lahenduse, kui täidate MPI lubatud rakenduste abil on `mpiexec.exe`.

> [AZURE.NOTE] Kuigi see on funktsionaalselt erinevate, "mitme eksemplari ülesanne" pole kordumatud ülesande tüüp, nagu [StartTask] [ net_starttask] või [JobPreparationTask][net_jobprep]. Mitme eksemplari tööülesanne on lihtsalt standard paketi tööülesande ([CloudTask] [ net_task] paketi .NET-is) nime, kelle mitme eksemplari sätted on konfigureeritud. Selles artiklis nimetatakse see **mitme eksemplari ülesanne**.

## <a name="requirements-for-multi-instance-tasks"></a>Mitme eksemplari tööülesannete nõuded

Mitme eksemplari tööülesannete jaoks on vaja pool, **vaheline sõlm**teatisega lubatud ja **samaaegseid ülesande**täitmine keelatud. Kui käivitamisel mitme eksemplari tööülesande pargis ülemist sõlmevahet teatisega, mis on keelatud või *maxTasksPerNode* väärtus on suurem kui 1, kus kunagi Ajastatud ülesanne--jääb lõputult "aktiivne" olekus. See koodilõigu kuvatakse selle loomist selliste teegi paketi .net-i abil.

```csharp
CloudPool myCloudPool =
    myBatchClient.PoolOperations.CreatePool(
        poolId: "MultiInstanceSamplePool",
        targetDedicated: 3
        virtualMachineSize: "small",
        cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "4"));

// Multi-instance tasks require inter-node communication, and those nodes
// must run only one task at a time.
myCloudPool.InterComputeNodeCommunicationEnabled = true;
myCloudPool.MaxTasksPerComputeNode = 1;
```

Lisaks mitme eksemplari tööülesannete saab käivitada *ainult* sõlmed kaustadesse, mis on **loodud pärast 14 Detsember 2015**.

### <a name="use-a-starttask-to-install-mpi"></a>MPI installimiseks on StartTask abil

Mitme eksemplari ülesande MPI rakenduste käitamiseks peate esmalt installima MPI rakendamist (MS-MPI või Inteli MPI, näiteks) Arvuta sõlmed kogumi. See on hea aeg kasutada mõne [StartTask][net_starttask], mis aktiveeritakse iga kord, kui sõlm on ühendab või taaskäivitamist. See koodilõigu loob StartTask, mis määrab MS-MPI installipaketi [ressursifaili][net_resourcefile]. Alusta tööülesande käsurea käivitatakse pärast ressursifaili laaditakse sõlme. Sel juhul käsurea teostab, MS-MPI järelevalveta installi.

```csharp
// Create a StartTask for the pool which we use for installing MS-MPI on
// the nodes as they join the pool (or when they are restarted).
StartTask startTask = new StartTask
{
    CommandLine = "cmd /c MSMpiSetup.exe -unattend -force",
    ResourceFiles = new List<ResourceFile> { new ResourceFile("https://mystorageaccount.blob.core.windows.net/mycontainer/MSMpiSetup.exe", "MSMpiSetup.exe") },
    RunElevated = true,
    WaitForSuccess = true
};
myCloudPool.StartTask = startTask;

// Commit the fully configured pool to the Batch service to actually create
// the pool and its compute nodes.
await myCloudPool.CommitAsync();
```

### <a name="remote-direct-memory-access-rdma"></a>Serveri otsest mälu juurdepääs (RDMA)

Kui valite [RDMA-ühenduse võimalusega suurus](../virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md) , nt A9 Arvuta sõlmed olevate paketi, saate rakenduse MPI ära Azure suure jõudlusega, latentsusajaga serveri otsest mälu juurdepääs (RDMA) võrku.

Otsige suurused, mis on määratud "RDMA toega" järgmistes artiklites:

* **CloudServiceConfiguration** kaustu

  * [Pilveteenustega suurus](../cloud-services/cloud-services-sizes-specs.md) (Ainult Windows)

* **VirtualMachineConfiguration** kaustu

  * [Suurused Azure'i virtuaalmasinates](../virtual-machines/virtual-machines-linux-sizes.md) (Linux)

  * [Suurused Azure'i virtuaalmasinates](../virtual-machines/virtual-machines-windows-sizes.md) (Windows)

>[AZURE.NOTE] Ära RDMA kohta [Linux arvutada sõlmed](batch-linux-nodes.md), peate kasutama **Inteli MPI** sõlmed. CloudServiceConfiguration ja VirtualMachineConfiguration kohta leiate lisateavet jaotisest [rakenduskausta](batch-api-basics.md#Pool) paketi funktsioonide ülevaate.

## <a name="create-a-multi-instance-task-with-batch-net"></a>Mitme eksemplari ülesande loomine paketi .net-i abil

Nüüd kus oleme käsitlenud rakenduskausta nõuded ja MPI paketi installimine, loomine mitme eksemplari tööülesanne. See koodilõigu loome standard [CloudTask][net_task], siis konfigureerimine oma [MultiInstanceSettings] [ net_multiinstance_prop] atribuut. Nagu varem mainitud, mitme eksemplari tööülesande ei ole erinevate ülesande tüüp, kuid mitme eksemplari konfiguratsioonisätted standard paketi tööülesande.

```csharp
// Create the multi-instance task. Its command line is the "application command"
// and will be executed *only* by the primary, and only after the primary and
// subtasks execute the CoordinationCommandLine.
CloudTask myMultiInstanceTask = new CloudTask(id: "mymultiinstancetask",
    commandline: "cmd /c mpiexec.exe -wdir %AZ_BATCH_TASK_SHARED_DIR% MyMPIApplication.exe");

// Configure the task's MultiInstanceSettings. The CoordinationCommandLine will be executed by
// the primary and all subtasks.
myMultiInstanceTask.MultiInstanceSettings =
    new MultiInstanceSettings(numberOfNodes) {
    CoordinationCommandLine = @"cmd /c start cmd /c ""%MSMPI_BIN%\smpd.exe"" -d",
    CommonResourceFiles = new List<ResourceFile> {
    new ResourceFile("https://mystorageaccount.blob.core.windows.net/mycontainer/MyMPIApplication.exe",
                     "MyMPIApplication.exe")
    }
};

// Submit the task to the job. Batch will take care of splitting it into subtasks and
// scheduling them for execution on the nodes.
await myBatchClient.JobOperations.AddTaskAsync("mybatchjob", myMultiInstanceTask);
```

## <a name="primary-task-and-subtasks"></a>Esmane ülesanne ja alamülesannete

Kui loote mitme eksemplari sätete tööülesande, saate määrata Arvuta sõlmed, mis on tööülesande käivitada arv. Kui saadate tööülesande töö, loob paketi teenuse **esmane** ühe tööülesande ja piisavalt **alamülesanded** vastavad koos sõlmed määratud arv.

Järgmised toimingud on määratud id täisarv vahemikus 0 *numberOfInstances* - 1. Ülesande id 0-ga on esmane ülesanne ja kõik muud ID-d on alamülesanded. Näiteks kui loote tööülesande mitme eksemplari järgmised sätted, esmane ülesanne oleks id on 0, ja selle alamülesanded oleks ID-d 1 – 9.

```csharp
int numberOfNodes = 10;
myMultiInstanceTask.MultiInstanceSettings = new MultiInstanceSettings(numberOfNodes);
```

### <a name="master-node"></a>Juhtslaidi sõlm

Kui saadate mitme eksemplari tööülesande, paketi teenuse määrab ühe Arvuta sõlmed nimega "juhtslaidi" sõlm ja esmane ülesande juhtslaidi sõlme käivitada. Funktsiooni alamülesanded on ajastatud ülejäänud eraldatud mitme eksemplari tööülesande sõlmed käivitada.

## <a name="coordination-command"></a>Koordineerituna käsk

**Koordineerituna käsk** on sooritatud nii esmast ja alamülesannete.

Käsk koordineerituna kutsumise blokeerib--paketi ei käsu rakenduse kuni koordineerituna käsk on tagastatud edukalt Kõik alamülesanded. Käsk koordineerituna tuleks käivitada mõni nõutav tausta teenuseid, veenduge, et need on kasutamiseks valmis, ja seejärel väljuge rakendusest. Näiteks see koordineerituna käsk lahenduse kasutades MS-MPI versiooni 7 käivitatakse SMPD teenus sõlme, siis suletakse:

```
cmd /c start cmd /c ""%MSMPI_BIN%\smpd.exe"" -d
```

Pange tähele kasutamine `start` selle koordineerituna käsu. See on vajalik, kuna selle `smpd.exe` rakendus ei tagasta kohe pärast Andmetäite. Ilma kasutuse [Alustamine] [ cmd_start] käsk koordineerituna see käsk pole tagasi ja seetõttu blokeeritaks rakenduse käsu, kus töötab.

## <a name="application-command"></a>Rakenduse käsk

Kui esmane ülesanne ja kõik alamülesanded lõpetanud koordineerituna käsu, mitme eksemplari tööülesande käsurea on sooritatud funktsiooni esmane ülesanne *ainult*. Me kutsume seda **käsku** eristamiseks koordineerituna käsk.

MS-MPI rakenduste, kasutage käsku rakenduse käivitada MPI lubatud rakenduse abil `mpiexec.exe`. Siin on näiteks soovitud lahenduse abil MS-MPI versioon 7 käsku:

```
cmd /c ""%MSMPI_BIN%\mpiexec.exe"" -c 1 -wdir %AZ_BATCH_TASK_SHARED_DIR% MyMPIApplication.exe
```

>[AZURE.NOTE] Kuna MS-MPI `mpiexec.exe` kasutab funktsiooni `CCP_NODES` muutuja poolt vaikimisi (vt [keskkonna muutujate](#environment-variables)) näide rida käsk ülaltoodud välistab selle.

## <a name="environment-variables"></a>Keskkonna muutujad

Paketi loob mitut [muutujat keskkonna] [ msdn_env_var] teatud mitme eksemplari tööülesanded tööülesande mitme eksemplari eraldatud Arvuta sõlmed. Koordineerituna ja rakenduse käsk read saate otsida nende keskkonna muutujate, nagu saate skripte ja neid käivitada programmi.

Järgmiste keskkonna muutujate põhjustavad paketi teenuse kasutamiseks mitme eksemplari toimingud:

* `CCP_NODES`
* `AZ_BATCH_NODE_LIST`
* `AZ_BATCH_HOST_LIST`
* `AZ_BATCH_MASTER_NODE`
* `AZ_BATCH_TASK_SHARED_DIR`
* `AZ_BATCH_IS_CURRENT_NODE_MASTER`

Teave selle ja muude paketi Arvuta sõlm keskkonna muutujate üksikasjadega tutvumiseks sealhulgas nende sisu ja nähtavus leiate [arvutada sõlm keskkonna muutujate][msdn_env_var].

>[AZURE.TIP] Paketi Linuxi MPI proovi kood sisaldab mitut nende keskkonna muutujate kasutamise näide. [Koordineerituna-cmd] [ coord_cmd_example] Bash skripti laadib levinud rakendus ja sisendi Azure Storage failid, võimaldab faili System (NFS) ühiskasutus juhtslaidi sõlme ja konfigureerib jagatakse mitme eksemplari tööülesande NFS klientidele sõlmi.

## <a name="resource-files"></a>Ressursi failid

On kahte tüüpi ressurss faile mitme eksemplari tööülesannete silmas pidada: *Kõik* tööülesanded allalaadimine **levinud ressurss faile** (mõlemas primaarne ja alamülesannete), ja **ressurss faile** mitme eksemplari jaoks määratud tööülesannete ise, millised *ainult esmast* tööülesande allalaadimist.

Ühe või mitme **levinud ressurss faile** saate määrata ülesande mitme eksemplari sätteid. Need levinud ressursi failid on alla laaditud [Azure Storage](./../storage/storage-introduction.md) esmast iga sõlme **tööülesande ühiskasutusse antud kausta** ja kõik alamülesanded. Pääsete ühiskasutusega kataloogi tööülesande taotlus ja koordineerituna käsk read, kasutades funktsiooni `AZ_BATCH_TASK_SHARED_DIR` muutuja. Funktsiooni `AZ_BATCH_TASK_SHARED_DIR` tee on identne iga sõlme eraldatud mitme eksemplari tööülesanne, seega saate jagada ühe koordineerituna käsk esmast ja kõik alamülesanded vahel. Paketi "Jaga" Kaugpöördusteenuse mõttes kataloogi, kuid saate seda kasutada alusega või punkti nagu varem keskkonna muutujate vihje ühiskasutusse anda.

Ressursi failid teie määratud mitme eksemplari ülesanne ise ülesande töötavat kausta, laaditakse `AZ_BATCH_TASK_WORKING_DIR`, vaikimisi. Nagu mainitud, erinevalt levinud ressurss faile, ainult esmane tööülesande allalaadimist ressurss faile mitme eksemplari ise ülesande jaoks määratud.

> [AZURE.IMPORTANT] Kasuta alati keskkonna muutujate `AZ_BATCH_TASK_SHARED_DIR` ja `AZ_BATCH_TASK_WORKING_DIR` neid oma käsk read kataloogid viidata. Proovige ehitada teed käsitsi.

## <a name="task-lifetime"></a>Ülesande kestus

Esmane tööülesande kestuse määrab kogu mitme eksemplari tööülesande kestuse. Esmane väljumisel, kõik selle alamülesanded lõpetada. Esmase välju kood on tööülesande välju kood ja kasutatakse määratleda takistavaid tööülesande uuesti eesmärgil.

Kui mõne funktsiooni alamülesanded nurjub, väljumist null saatja koodi, näiteks kogu mitme eksemplari tööülesande nurjub. Mitme eksemplari tööülesanne on seejärel lõpetada ja uuesti proovida, kuni selle uuesti piiri.

Mitme eksemplari Tööülesande kustutamisel kustutatakse esmast ja kõik alamülesanded paketi teenus võttis ka. Kõik Alamülesanne kataloogid ja oma faile kustutatakse Arvuta sõlmed, just nagu standard ülesanne.

[TaskConstraints] [ net_taskconstraints] mitme eksemplari tööülesande, nt [MaxTaskRetryCount][net_taskconstraint_maxretry], [MaxWallClockTime][net_taskconstraint_maxwallclock], ja [RetentionTime] [ net_taskconstraint_retention] atribuudid, on au, kui need on standardne tööülesande, ning rakendada esmast ja kõik alamülesanded. Juhul, kui muudate [RetentionTime] [ net_taskconstraint_retention] atribuudi pärast lisamist mitme eksemplari tööülesande töö, selle muudatuse rakendatakse ainult esmane ülesanne. Kõik selle alamülesanded edasi kasutada algset [RetentionTime][net_taskconstraint_retention].

Arvuta sõlme tehtud ülesandeloendi kajastab alamtoimingu ID-d kui tehtud tööülesanne on osa mitme eksemplari.

## <a name="obtain-information-about-subtasks"></a>Alamülesannete kohta teabe saamiseks

Teegi paketi .net-i abil alamülesanded kohta teabe saamiseks helistage [CloudTask.ListSubtasks] [ net_task_listsubtasks] meetod. See meetod tagastab kõik alamülesanded teave ja teavet MSDN, mida tööülesanded. Selle teabe põhjal saate määrata iga alamtoimingu juurkaust, rakenduskausta ID-d, selle praeguses olekus, välju kood ja muud. Seda teavet saate kasutada koos [PoolOperations.GetNodeFile] [ poolops_getnodefile] viis korda 's failid. Pange tähele, et seda meetodit ei tagasta esmane ülesande (0-id).

> [AZURE.NOTE] Teisiti, paketi .NET meetodite mis toimivad mitme eksemplari [CloudTask] [ net_task] ise rakendamiseks *ainult* esmane ülesanne. Näiteks kui helistate [CloudTask.ListNodeFiles] [ net_task_listnodefiles] meetod mitme eksemplari tööülesandega, tagastatakse ainult esmane tööülesande failid.

Järgmised koodilõigu näitab, kuidas alamtoimingu teabe saamiseks, samuti taotleda mil nad täitnud sõlmed faili sisu.

```csharp
// Obtain the job and the multi-instance task from the Batch service
CloudJob boundJob = batchClient.JobOperations.GetJob("mybatchjob");
CloudTask myMultiInstanceTask = boundJob.GetTask("mymultiinstancetask");

// Now obtain the list of subtasks for the task
IPagedEnumerable<SubtaskInformation> subtasks = myMultiInstanceTask.ListSubtasks();

// Asynchronously iterate over the subtasks and print their stdout and stderr
// output if the subtask has completed
await subtasks.ForEachAsync(async (subtask) =>
{
    Console.WriteLine("subtask: {0}", subtask.Id);
    Console.WriteLine("exit code: {0}", subtask.ExitCode);

    if (subtask.State == TaskState.Completed)
    {
        ComputeNode node =
            await batchClient.PoolOperations.GetComputeNodeAsync(subtask.ComputeNodeInformation.PoolId,
                                                                 subtask.ComputeNodeInformation.ComputeNodeId);

        NodeFile stdOutFile = await node.GetNodeFileAsync(subtask.ComputeNodeInformation.TaskRootDirectory + "\\" + Constants.StandardOutFileName);
        NodeFile stdErrFile = await node.GetNodeFileAsync(subtask.ComputeNodeInformation.TaskRootDirectory + "\\" + Constants.StandardErrorFileName);
        stdOut = await stdOutFile.ReadAsStringAsync();
        stdErr = await stdErrFile.ReadAsStringAsync();

        Console.WriteLine("node: {0}:", node.Id);
        Console.WriteLine("stdout.txt: {0}", stdOut);
        Console.WriteLine("stderr.txt: {0}", stdErr);
    }
    else
    {
        Console.WriteLine("\tSubtask {0} is in state {1}", subtask.Id, subtask.State);
    }
});
```

## <a name="code-sample"></a>Proovi kood

[MultiInstanceTasks] [ github_mpi] kood valimi github näitab, kuidas kasutada mitme eksemplari tööülesande on [MS-MPI] [ msmpi_msdn] rakendus paketi arvutada sõlmed. Järgige [koostamine](#preparation) ja [täitmise](#execution) valimi käivitamiseks.

### <a name="preparation"></a>Ettevalmistamine

1. [Kuidas koostada ja käivitada lihtsa MS-MPI programmi]kaks esimest järgige[msmpi_howto]. See vastab prerequesites järgmises etapis.
1. *Väljalaske* versiooni [MPIHelloWorld] koostamine[ helloworld_proj] valimi MPI programmi. See on programm, mis käivitub Arvuta sõlmed mitme eksemplari ülesande.
1. Looge sisaldava zip faili `MPIHelloWorld.exe` (kuhu ehitatud etappi 2) ja `MSMpiSetup.exe` (mille saate alla laadida etappi 1). Kui rakenduse paketi järgmise juhise juurde laaditakse zip-fail.
1. Kasutada [Azure portaali] [ portal] paketi [rakenduse](batch-application-packages.md) nimega "MPIHelloWorld" loomine ning määrata zip-fail loodud eelmises etapis rakendusepaketi versioonina "1.0". Lisateabe saamiseks vaadake [üles- ja rakenduste haldamine](batch-application-packages.md#upload-and-manage-applications) .

>[AZURE.TIP] Koostamine *on versiooni* `MPIHelloWorld.exe` nii, et teil pole lisada mis tahes täiendavaid sõltuvused (nt `msvcp140d.dll` või `vcruntime140d.dll`) sisse oma rakendusepaketi.

### <a name="execution"></a>Täitmise

1. Laadige [azure-paketi-näidised] [ github_samples_zip] GitHub kaudu.
1. Avage MultiInstanceTasks **lahenduse** Visual Studio 2015. Funktsiooni `MultiInstanceTasks.sln` lahenduse fail asub:

    `azure-batch-samples\CSharp\ArticleProjects\MultiInstanceTasks\`

1. Sisestage paketi ja salvestusruumi konto mandaadid sisse `AccountSettings.settings` **Microsoft.Azure.Batch.Samples.Common** Projectis.
1. **Koostamine ja käivitage** MultiInstanceTasks lahendus käivitada näitaja tulemuse vahel Kuulake rakenduse kohta arvutada sõlmed paketi pargis.
1. *Valikuline*: [Azure'i portaali] kasutamine[ portal] või [Paketi Exploreri] [ batch_explorer] uurida valimi rakenduskausta, töö ja enne tööülesande ("MultiInstanceSamplePool", "MultiInstanceSampleJob", "MultiInstanceSampleTask") kustutamine ressursside.

>[AZURE.TIP] Saate alla laadida [Visual Studio ühenduse] [ visual_studio] tasuta, kui teil pole Visual Studio.

Väljund `MultiInstanceTasks.exe` on sarnane järgmist:

```
Creating pool [MultiInstanceSamplePool]...
Creating job [MultiInstanceSampleJob]...
Adding task [MultiInstanceSampleTask] to job [MultiInstanceSampleJob]...
Awaiting task completion, timeout in 00:30:00...

Main task [MultiInstanceSampleTask] is in state [Completed] and ran on compute node [tvm-1219235766_1-20161017t162002z]:
---- stdout.txt ----
Rank 2 received string "Hello world" from Rank 0
Rank 1 received string "Hello world" from Rank 0

---- stderr.txt ----

Main task completed, waiting 00:00:10 for subtasks to complete...

---- Subtask information ----
subtask: 1
        exit code: 0
        node: tvm-1219235766_3-20161017t162002z
        stdout.txt:
        stderr.txt:
subtask: 2
        exit code: 0
        node: tvm-1219235766_2-20161017t162002z
        stdout.txt:
        stderr.txt:

Delete job? [yes] no: yes
Delete pool? [yes] no: yes

Sample complete, hit ENTER to exit...
```

## <a name="next-steps"></a>Järgmised sammud

- Microsoft HPC ja Azure'i paketi meeskonna ajaveebi käsitletakse [MPI kasutajatugi Linuxi Azure'i paketi][blog_mpi_linux], ja sisaldab [OpenFOAM] kasutamise kohta teavet[ openfoam] paketi abil. Võite leida Python koodinäiteid [OpenFOAM näide github][github_mpi].

- Siit saate teada, [luua Linux Arvuta sõlmed](batch-linux-nodes.md) kasutada oma Azure'i paketi MPI lahenduste kohta.

[helloworld_proj]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/MultiInstanceTasks/MPIHelloWorld

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[batch_explorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[blog_mpi_linux]: https://blogs.technet.microsoft.com/windowshpc/2016/07/20/introducing-mpi-support-for-linux-on-azure-batch/
[cmd_start]: https://technet.microsoft.com/library/cc770297.aspx
[coord_cmd_example]: https://github.com/Azure/azure-batch-samples/blob/master/Python/Batch/article_samples/mpi/data/linux/openfoam/coordination-cmd
[github_mpi]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/MultiInstanceTasks
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_samples_zip]: https://github.com/Azure/azure-batch-samples/archive/master.zip
[msdn_env_var]: https://msdn.microsoft.com/library/azure/mt743623.aspx
[msmpi_msdn]: https://msdn.microsoft.com/library/bb524831.aspx
[msmpi_sdk]: http://go.microsoft.com/FWLink/p/?LinkID=389556
[msmpi_howto]: http://blogs.technet.com/b/windowshpc/archive/2015/02/02/how-to-compile-and-run-a-simple-ms-mpi-program.aspx
[openfoam]: http://www.openfoam.com/
[visual_studio]: https://www.visualstudio.com/vs/community/

[net_jobprep]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobpreparationtask.aspx
[net_multiinstance_class]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.aspx
[net_multiinstance_prop]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.multiinstancesettings.aspx
[net_multiinsance_commonresfiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.commonresourcefiles.aspx
[net_multiinstance_coordcmdline]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.coordinationcommandline.aspx
[net_multiinstance_numinstances]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.numberofinstances.aspx
[net_pool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_pool_create]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.createpool.aspx
[net_pool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx
[net_resourcefile]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.resourcefile.aspx
[net_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.starttask.aspx
[net_starttask_cmdline]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.starttask.commandline.aspx
[net_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
[net_taskconstraints]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.aspx
[net_taskconstraint_maxretry]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.maxtaskretrycount.aspx
[net_taskconstraint_maxwallclock]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.maxwallclocktime.aspx
[net_taskconstraint_retention]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.retentiontime.aspx
[net_task_listsubtasks]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.listsubtasks.aspx
[net_task_listnodefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.listnodefiles.aspx
[poolops_getnodefile]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.getnodefile.aspx

[portal]: https://portal.azure.com
[rest_multiinstance]: https://msdn.microsoft.com/library/azure/mt637905.aspx

[1]: ./media/batch-mpi/batch_mpi_01.png "Mitme eksemplari ülevaade"
