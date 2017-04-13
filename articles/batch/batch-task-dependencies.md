<properties
    pageTitle="Tööülesande Azure'i paketi sõltuvused | Microsoft Azure'i"
    description="Muude toimingute MapReduce laad ja sarnase suur andmete töötlemiseks edukaks sõltuvad tööülesannete loomine rakenduses Azure'i paketi töökoormus."
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
    ms.date="09/28/2016"
    ms.author="marsma" />

# <a name="task-dependencies-in-azure-batch"></a>Azure'i paketi tööülesande sõltuvused

Tööülesande sõltuvused funktsioon Azure'i paketi on hea sobib siis, kui soovite töödelda:

- MapReduce laadis töökoormus pilveteenuses.
- Töö, mille andmetöötlus tööülesannete saab väljendada suunatud atsüklilised diagrammina (DAG).
- Mõne muu järgmise etapi tööülesanded sõltuvad tööülesandeid varustava väljund töö.

Paketi ülesandesõltuvused võimaldavad teil luua tööülesandeid, mis on ajastatud täitmise Arvuta sõlmed ainult ühe või mitme tööülesannete eduka lõpetamise järel. Näiteks saate luua tööd, mis muudab iga paneeli 3D filmi eraldi, paralleelsetes ülesannetega. Lõplik tööülesande--"Ühenda ülesanne"--ühendab sulatatud raamid täielik filmi sisse alles siis, kui kõik raamid on edukalt muudetud.

Toimingud sõltuvad muude ülesannete üks-ühele või üks-mitmele seose loomine Saate luua ka vahemiku sõltuvus, kus tööülesande sõltub edukaks rühma ülesanded on kindlas vahemikus ülesande ID-d. Saate ühendada need kolme lihtsa stsenaariumid loomiseks mitu-mitmele seosed.

## <a name="task-dependencies-with-batch-net"></a>Ülesandesõltuvused paketi .net-i abil

Selles artiklis [Paketi .net-i] abil ülesandesõltuvused konfigureerimine räägitakse[ net_msdn] teek. Esmalt näitame teile [lubada tööülesande sõltuvuse](#enable-task-dependencies) kohta oma töö ja seejärel näidata [tööülesande sõltuvusi](#create-dependent-tasks)konfigureerimine. Lõpuks käsitleme [sõltuvus stsenaariumid](#dependency-scenarios) , mis paketi toetab.

## <a name="enable-task-dependencies"></a>Ülesandesõltuvused lubamine

Ülesandesõltuvused paketi rakenduse kasutamiseks peate esmalt teavitama paketi teenuse, et töö kasutab ülesandesõltuvused. Paketi .net-i luba selle sisse oma [CloudJob] [ net_cloudjob] , seades selle [UsesTaskDependencies] [ net_usestaskdependencies] atribuut `true`:

```csharp
CloudJob unboundJob = batchClient.JobOperations.CreateJob( "job001",
    new PoolInformation { PoolId = "pool001" });

// IMPORTANT: This is REQUIRED for using task dependencies.
unboundJob.UsesTaskDependencies = true;
```

Eelmise koodilõigu, "batchClient" on näiteks [BatchClient] [ net_batchclient] klassi.

## <a name="create-dependent-tasks"></a>Sõltuvad tööülesannete loomine

Üks või mitu tööülesannet edukaks sõltuvale tööülesande loomiseks annate paketi, et tööülesanne "sõltub" muid toiminguid. Paketi .net-i, konfigureerige [CloudTask][net_cloudtask]. [Ei leia DependsOn] [net_dependson] atribuudi eksemplariga [TaskDependencies] [ net_taskdependencies] klassi:

```csharp
// Task 'Flowers' depends on completion of both 'Rain' and 'Sun'
// before it is run.
new CloudTask("Flowers", "cmd.exe /c echo Flowers")
{
    DependsOn = TaskDependencies.OnIds("Rain", "Sun")
},
```

See koodilõigu loob tööülesande "Lilledest", mis on ajastatud käivitamiseks Arvuta sõlme alles pärast seda, kui koos ID-d "Vihm" ja "P" tööülesanded on lõpule viidud ID-ga.

 > [AZURE.NOTE] Tööülesande lõpule viia, kui see on **lõpetatud** olekus ja selle **väljumine kood** on peetakse `0`. Paketi .net-i, tähendab see on [CloudTask][net_cloudtask]. [Olek] [net_taskstate] atribuudi väärtus `Completed` ja selle CloudTask [TaskExecutionInformation][net_taskexecutioninformation]. [ExitCode] [net_exitcode] atribuudi väärtus on `0`.

## <a name="dependency-scenarios"></a>Sõltuvus stsenaariumid

Stsenaariumit kolme lihtsa tööülesannete sõltuvus saate kasutada Azure'i paketi: üks-ühele, üks-mitmele ja ülesande ID vahemiku sõltuvus. Neid saab kombineerida neljas stsenaarium, mitu-mitmele esitada.

 Stsenaarium&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | Näide | |
 :-------------------: | ------------------- | -------------------
 [Üks-ühele](#one-to-one) | *taskB* sõltub *taskA* <p/> *taskB* pole on täitmise kavandatud, kuni *taskA* on lõpule viidud | ![Diagramm: üks-ühele tööülesande sõltuvus][1]
 [Üks-mitmele](#one-to-many) | *taskC* sõltub nii *taskA* *taskB* <p/> *taskC* pole on täitmise kavandatud, kuni nii *taskA* ja *taskB* on lõpule viidud | ![Diagramm: üks-mitmele tööülesande sõltuvus][2]
 [Ülesande ID vahemikus](#task-id-range) | *taskD* sõltub mitmesuguseid tööülesanded <p/> *taskD* pole on täitmise kavandatud, kuni ID-d *1* kuni *10* tööülesanded on lõpule viidud | ![Diagramm: Ülesande id vahemiku sõltuvus][3]

>[AZURE.TIP] Saate luua **mitu-mitmele** seosed, näiteks kui ülesanded, C, D, E ja F iga sõltub tööülesannete A ja B. See on kasulik, näiteks parallelized eeltöötlus stsenaariumid, kus järgmise etapi tööülesannete sõltuvad sellest, mitu varustava tööülesannete väljund.

### <a name="one-to-one"></a>Üks-ühele

Tööülesande, mis sõltub edukaks ühe teise ülesande loomiseks saate esitama ühe tööülesande ID, et [TaskDependencies][net_taskdependencies]. [OnId] [net_onid] staatiline meetod, kui te [ei leia DependsOn] asustada[ net_dependson] atribuut [CloudTask][net_cloudtask].

```csharp
// Task 'taskA' doesn't depend on any other tasks
new CloudTask("taskA", "cmd.exe /c echo taskA"),

// Task 'taskB' depends on completion of task 'taskA'
new CloudTask("taskB", "cmd.exe /c echo taskB")
{
    DependsOn = TaskDependencies.OnId("taskA")
},
```

### <a name="one-to-many"></a>Üks-mitmele

Mis sõltub edukaks mitme tööülesanded tööülesande loomiseks saate esitama kogumi ülesande ID-de [TaskDependencies][net_taskdependencies]. [OnIds] [net_onids] staatiline meetod, kui te [ei leia DependsOn] asustada[ net_dependson] atribuut [CloudTask][net_cloudtask].

```csharp
// 'Rain' and 'Sun' don't depend on any other tasks
new CloudTask("Rain", "cmd.exe /c echo Rain"),
new CloudTask("Sun", "cmd.exe /c echo Sun"),

// Task 'Flowers' depends on completion of both 'Rain' and 'Sun'
// before it is run.
new CloudTask("Flowers", "cmd.exe /c echo Flowers")
{
    DependsOn = TaskDependencies.OnIds("Rain", "Sun")
},
```

### <a name="task-id-range"></a>Ülesande ID vahemikus

Luua tööülesande, mis on sõltuvus edukaks rühma ülesanded, kelle ID-d vale vahemikus, esitama esimese ja viimase ülesande ID-d kuni [TaskDependencies][net_taskdependencies]. [OnIdRange] [net_onidrange] staatiline meetod, kui te [ei leia DependsOn] asustada[ net_dependson] atribuut [CloudTask][net_cloudtask].

>[AZURE.IMPORTANT] Ülesande ID vahemike kasutamisel teie sõltuvuste tööülesande ID vahemiku *peab* olema stringi esinduste täisarvuni väärtused. Lisaks iga vahemikus ülesande peab lõpule viia sõltuvad ülesande ajaks ajastada.

```csharp
// Tasks 1, 2, and 3 don't depend on any other tasks. Because
// we will be using them for a task range dependency, we must
// specify string representations of integers as their ids.
new CloudTask("1", "cmd.exe /c echo 1"),
new CloudTask("2", "cmd.exe /c echo 2"),
new CloudTask("3", "cmd.exe /c echo 3"),

// Task 4 depends on a range of tasks, 1 through 3
new CloudTask("4", "cmd.exe /c echo 4")
{
    // To use a range of tasks, their ids must be integer values.
    // Note that we pass integers as parameters to TaskIdRange,
    // but their ids (above) are string representations of the ids.
    DependsOn = TaskDependencies.OnIdRange(1, 3)
},
```

## <a name="code-sample"></a>Proovi kood

[TaskDependencies] [ github_taskdependencies] valimi projekt on üks [Azure'i paketi koodinäiteid] [ github_samples] github. See lahendus Visual Studio 2015 näitab, kuidas lubada tööülesande sõltuvus tööd, tööülesannete, mis sõltuvad otseselt muude toimingute loomine ja nende ülesannete täitmiseks aluseks olevate Arvuta sõlmed.

## <a name="next-steps"></a>Järgmised sammud

### <a name="application-deployment"></a>Rakenduse juurutamine

[Rakenduse paketid](batch-application-packages.md) funktsioon paketi abil on lihtne nii juurutada ja rakendusi, mida oma tööülesandeid täita arvutada sõlmed versioon.

### <a name="installing-applications-and-staging-data"></a>Rakenduste installimine ja vaheetapiandmed

Lugege teemat [rakenduste installimine ja lavastus andmete paketi arvutada sõlmed] [ forum_post] postitada Azure'i paketi Foorum ülevaate erinevate viiside ette valmistada oma sõlmed käivitada ülesanded. Kirjutanud üks Azure'i paketi meeskonna liikmed, see postitus on hea krundita võimalust saada failid (sh nii rakenduste ja tööülesande sisendandmete) oma Arvuta sõlmed peale.

[forum_post]: https://social.msdn.microsoft.com/Forums/en-US/87b19671-1bdf-427a-972c-2af7e5ba82d9/installing-applications-and-staging-data-on-batch-compute-nodes?forum=azurebatch
[github_taskdependencies]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/TaskDependencies
[github_samples]: https://github.com/Azure/azure-batch-samples
[net_batchclient]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_cloudtask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
[net_dependson]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.dependson.aspx
[net_exitcode]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskexecutioninformation.exitcode.aspx
[net_msdn]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[net_onid]: https://msdn.microsoft.com/library/microsoft.azure.batch.taskdependencies.onid.aspx
[net_onids]: https://msdn.microsoft.com/library/microsoft.azure.batch.taskdependencies.onids.aspx
[net_onidrange]: https://msdn.microsoft.com/library/microsoft.azure.batch.taskdependencies.onidrange.aspx
[net_taskexecutioninformation]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskexecutioninformation.aspx
[net_taskstate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.common.taskstate.aspx
[net_usestaskdependencies]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.usestaskdependencies.aspx
[net_taskdependencies]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskdependencies.aspx

[1]: ./media/batch-task-dependency/01_one_to_one.png "Diagramm: üks-ühele sõltuvus"
[2]: ./media/batch-task-dependency/02_one_to_many.png "Diagramm: üks-mitmele sõltuvus"
[3]: ./media/batch-task-dependency/03_task_id_range.png "Diagramm: ülesande id vahemiku sõltuvus"
