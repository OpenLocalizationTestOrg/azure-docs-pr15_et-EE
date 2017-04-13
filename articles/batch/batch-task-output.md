<properties
    pageTitle="Töö ja tööülesande väljund püsimine Azure'i paketi | Microsoft Azure'i"
    description="Saate teada, kuidas kasutada Azure Storage püsival poe paketi ülesanne ja töö väljund ja luba selle nõutud väljundi vaatamine Azure'i portaalis."
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
    ms.date="09/07/2016"
    ms.author="marsma" />

# <a name="persist-azure-batch-job-and-task-output"></a>Azure'i paketi töö ja tööülesannete väljundi jää püsima

Käivitate paketi tavaliselt tööülesanded aedvili väljundi, mis tuleb hoida ja seejärel hiljem toodavate muude toimingute töösse, klientrakendusega, mida töö või mõlemad. Selle väljundi võib olla loodud sisendandmete töötlemise failidega või seotud tööülesande täitmise logifailid. Selles artiklis tutvustatakse .net-i klassi teeki, mis kasutab põhimõtted vastavalt tehnika püsima sellise tööülesande väljundi Azure'i bloobimälu, mistõttu saadaval isegi siis, kui olete oma kaustu, töö, kustutamine ja arvutada sõlmed.

Selle artikli tehnika kasutamisel saate vaadata ka oma tööülesande väljundi **Salvestatud väljund faili** ja [Azure portaali] **Salvestatud logid** [portal].

![Salvestatud väljundi failide ja salvestatud logid lülitid portaalis][1]

>[AZURE.NOTE] [Azure'i paketi faili põhimõtted] [ nuget_package] .NET klassiteek, mida selles artiklis käsitletakse praegu eelvaade. Mõned siinkirjeldatud funktsioonid võivad muutuda enne üldiselt kättesaadav.

## <a name="task-output-considerations"></a>Tööülesande väljundi peaksite arvesse võtma

Teie paketi lahendus kujundamisel tuleb arvesse võtta mitut tegurit, mis on seotud töö ja tööülesannete väljundeid.

* **Arvutage sõlm eluiga**: arvutada sõlmed on sageli siirdamiseks, eriti autoscale lubatud kaustu. Tööülesanded, mis töötavad sõlm väljundeid on saadaval ainult siis, kui sõlm on olemas ja faili säilitamine aja jooksul ainult teie määratud tööülesande jaoks. Tagamaks, et tööülesande väljundi säilib, tööülesannete peab seetõttu oma väljundi failide üleslaadimine püsival poodi, näiteks Azure Storage.

* **Väljundi salvestusruumi**: püsivad ülesandeandmeid väljundi püsival salvestusruumi, saate [Azure'i salvestusruumi SDK](../storage/storage-dotnet-how-to-use-blobs.md) tööülesande koodi üles laadida tööülesande väljundi bloobimälu salvestusruumi container. Kui otsustate container ja failinimede mess, klientrakenduse või muude toimingute töösse seejärel leidke ja selle väljundi põhjal soovite alla laadida.

* **Väljundi ülekanne**: saate tuua tööülesande väljundi otse Arvuta sõlmed olevate või Azure Storage kui tööülesannete säilivad nende väljund. Tuua mõne toimingu väljund otse Arvuta sõlme, peate faili nimi ja selle väljundi asukohta sõlme. Kui te püsivad väljund Azure Storage järgmise etapi tööülesannete või klientrakenduse peavad olema Azure Storage alla laadida, kasutades Azure salvestusruumi SDK faili täielik tee.

* **Väljundi vaatamine**: kui liikuda paketi tööülesande Azure portaali ja valige **sõlm faile**, on esitatud kõik failid sellega seotud tööülesande, mitte ainult väljund faili teile huvi. Klõpsake uuesti Arvuta sõlmed failid on saadaval ainult siis, kui sõlm on olemas ja faili säilitamine aja jooksul ainult teie määratud tööülesande. Tööülesande väljundi, mis te olete kestnud Azure Storage portaali või rakendust, nt [Azure salvestusruumi Exploreri]kuvamiseks[storage_explorer], peate teada oma asukohta ja avage fail otse.

## <a name="help-for-persisted-output"></a>Nõutud väljundi spikker

Paketi meeskonnatöö on määratletud, et aidata teil rohkem hõlpsalt püsida töö ja tööülesande väljund, ja rakendada kogumi nimereeglid .NET klassiteek [Azure'i paketi faili põhimõtted] [ nuget_package] teeki, mille abil saate oma paketi rakendused. Lisaks on Azure portaali teadlik nende nimereeglid nii, et leiaksite hõlpsalt teegi abil salvestatud failid.

## <a name="using-the-file-conventions-library"></a>Faili põhimõtted teegi kasutamine

[Azure'i paketi faili põhimõtted] [ nuget_package] on .NET klassiteek, mis paketi .NET rakenduste abil saate hõlpsalt talletada ja tuua tööülesande väljundeid ja sealt Azure Storage. See on mõeldud kasutamiseks nii ülesanne ja kliendi kood--tööülesande kood püsib failide ja kliendi koodi loendisse ja taastada. Tööülesannete saate kasutada ka teegi allalaadimise väljundeid varustava ülesandeid, näiteks [ülesandesõltuvused](batch-task-dependencies.md) stsenaariumi.

Teegi põhimõtted hoolitseb tagada, et säilitus- ja tööülesande väljund failid on nimega vastavalt soovite ja kui säilis Azure'i mäluruumi õigesse kohta üles. Kui toote väljundeid, on lihtne leida väljundeid antud töö või ülesande loetelu või toomine väljundeid ID ja otstarve, selle asemel, et leida failinimed või kui see on olemas salvestusruumi.

Näiteks saate teegis "kuvada kõik vahe tööülesande 7" või "saada mulle pisipildi eelvaate jaoks töö *mymovie*," teadmata nimesid või konto salvestusruumi asukoht.

### <a name="get-the-library"></a>Teegi hankimine

Saate hankida teek, mis sisaldab uutel ja laiendab [CloudJob] [ net_cloudjob] ja [CloudTask] [ net_cloudtask] tunnid uued meetodid, kaudu [Nugeti][nuget_package]. Saate lisada Visual Studio projekti [Nugeti teegi Package Manager][nuget_manager].

>[AZURE.TIP] Te ei leia [lähtekoodi] [ github_file_conventions] Azure'i paketi faili põhimõtted teegi github Microsoft Azure'i SDK hoidla .net-i jaoks.

## <a name="requirement-linked-storage-account"></a>Nõue: lingitud salvestusruumi konto

Väljundeid püsival salvestusruumi kasutamise põhimõtted faili teeki talletada ja vaadata neid Azure'i portaalis, peate [lingi Azure Storage konto](batch-application-packages.md#link-a-storage-account) paketi kontole. Kui te pole seda veel teinud, link salvestusruumi konto konto paketi Azure portaali kaudu:

**Paketi konto** blade > **sätted** > **Salvestusruumi konto** > **Salvestusruumi konto** (pole) > tellimuse salvestusruumi konto valige

Üksikasjalikumat walk-through salvestusruumi konto linkimise kohta, leiate [Azure'i paketi rakenduse paketid rakenduse juurutamine](batch-application-packages.md).

## <a name="persist-output"></a>Väljundi jää püsima

On kaks esmane toimingute sooritamiseks salvestamisel töö ja tööülesannete väljundi faili põhimõtted teegi: salvestusruumi-ümbrisest luua ja salvestada väljundi ümbris.

>[AZURE.WARNING] Kuna kõik töö ja tööülesannete väljundeid on salvestatud samasse container, [salvestusruumi ahendamine piirangud](../storage/storage-performance-checklist.md#blobs) korral täitmisele suure hulga tööülesannete proovige püsida samal ajal faile.

### <a name="create-storage-container"></a>Salvestusruumi container loomine

Enne tööülesannete hakata püsib väljund salvestusruumi, peate looma bloobimälu salvestusruumi container, millele nad saavad väljundi laaditakse. Toiming, helistades [CloudJob][net_cloudjob]. [PrepareOutputStorageAsync] [net_prepareoutputasync]. Selle meetodi laiend võtab on [CloudStorageAccount] [ net_cloudstorageaccount] objekti parameetrina ja loob nimega nii, et selle sisu on leitav Azure portaali ja andmetoomisteenused meetodid, mida käsitletakse selle artikli ümbris.

Saate paigutada tavaliselt järgmine kood klientrakenduse--rakendus, mis loob teie kaustu, töö ja tööülesandeid.

```csharp
CloudJob job = batchClient.JobOperations.CreateJob(
    "myJob",
    new PoolInformation { PoolId = "myPool" });

// Create reference to the linked Azure Storage account
CloudStorageAccount linkedStorageAccount =
    new CloudStorageAccount(myCredentials, true);

// Create the blob storage container for the outputs
await job.PrepareOutputStorageAsync(linkedStorageAccount);
```

### <a name="store-task-outputs"></a>Tööülesande väljundeid talletamine

Nüüd, kui olete valmis on ümbris paanide bloobimälu, tööülesannete, saate salvestada väljundi-ümbrisest abil [TaskOutputStorage] [ net_taskoutputstorage] klassi leitud faili põhimõtted teegis.

Tööülesande koodis kõigepealt looma on [TaskOutputStorage] [ net_taskoutputstorage] objekti ja seejärel kui tööülesanne on töö lõpetanud, kõne [TaskOutputStorage][net_taskoutputstorage]. [SaveAsync] [net_saveasync] salvestada selle väljundi Azure Storage meetod.

```csharp
CloudStorageAccount linkedStorageAccount = new CloudStorageAccount(myCredentials);
string jobId = Environment.GetEnvironmentVariable("AZ_BATCH_JOB_ID");
string taskId = Environment.GetEnvironmentVariable("AZ_BATCH_TASK_ID");

TaskOutputStorage taskOutputStorage = new TaskOutputStorage(
    linkedStorageAccount, jobId, taskId);

/* Code to process data and produce output file(s) */

await taskOutputStorage.SaveAsync(TaskOutputKind.TaskOutput, "frame_full_res.jpg");
await taskOutputStorage.SaveAsync(TaskOutputKind.TaskPreview, "frame_low_res.jpg");
```

"Väljund tüüpi" parameetri kategooria nõutud failid. Leidub neli eelmääratletud [TaskOutputKind] [ net_taskoutputkind] tüüpi: "TaskOutput", "TaskPreview", "TaskLog" ja "TaskIntermediate." Kohandatud kohta, milliseid võite määrata ka siis, kui nad oleks teie töövoog.

Väljundi järgmist tüüpi saate määrata, millist tüüpi väljundid, kui teete päringu hiljem paketi nõutud väljundid antud tööülesande lisamiseks. Teisisõnu, kui olete loendi väljundeid tööülesande, saate filtreerida väljundi tüübid nimekirjas. Näiteks "mulle *Eelvaade* väljund tööülesande *109*." Lisateavet loetelu ja toomine väljundeid kuvatakse [tuua väljundi](#retrieve-output) selle artikli.

>[AZURE.TIP] Millist väljundi tähistab Azure'i portaalis konkreetses failis kuvamiskoha: *TaskOutput*-kategoriseeritud faile kuvada "Tööülesande väljund faili", ja *TaskLog* faile kuvada "Tööülesande logid."

### <a name="store-job-outputs"></a>Töö väljundeid talletamine

Lisaks talletamise tööülesande väljundeid, saate salvestada väljundeid, mis on seostatud mõne kogu töö. Näiteks filmi renderdamise töö toimingu Ühenda võib täielikult sulatatud filmi püsida töö võrdsed. Kui teie töö on valmis, klientrakenduse saate lihtsalt loendisse ja tuua väljundeid töö ja ei pea päringu üksikute tööülesannete.

Töö väljundi talletada, helistades [JobOutputStorage][net_joboutputstorage]. [SaveAsync] [net_joboutputstorage_saveasync] meetod ja määrake [JobOutputKind] [ net_joboutputkind] ja faili nimi:

```
CloudJob job = await batchClient.JobOperations.GetJobAsync(jobId);
JobOutputStorage jobOutputStorage = job.OutputStorage(linkedStorageAccount);

await jobOutputStorage.SaveAsync(JobOutputKind.JobOutput, "mymovie.mp4");
await jobOutputStorage.SaveAsync(JobOutputKind.JobPreview, "mymovie_preview.mp4");
```

Nagu TaskOutputKind tööülesande väljundeid jaoks, mida kasutate [JobOutputKind] [ net_joboutputkind] liigitamiseks töö parameeter on jätkunud failid. See parameeter võimaldab hiljem päringu (loendi) mõnda kindlat tüüpi väljund. Funktsiooni JobOutputKind sisaldab väljundi-ja eelvaade ja toetab luua kohandatud tüüpi.

### <a name="store-task-logs"></a>Tööülesande Logide salvestamine

Lisaks püsib faili püsival mäluruumi kui tööülesande või töö on lõpule jõudnud, võite leida seda vaja jää püsima failid on värskendatud tööülesande--logifailid täitmise ajal või `stdout.txt` ja `stderr.txt`, näiteks. Selleks leiate Azure'i paketi faili põhimõtted teegi [TaskOutputStorage][net_taskoutputstorage]. [SaveTrackedAsync] [net_savetrackedasync] meetod. Koos [SaveTrackedAsync][net_savetrackedasync], saate sõlme (intervalliga teie määratud) faili värskenduste jälitamine ja püsivad Azure Storage need värskendused.

Järgmised koodilõigu, kasutame [SaveTrackedAsync] [ net_savetrackedasync] värskendamiseks `stdout.txt` Azure Storage iga 15 sekundi järel tööülesannet täitmise ajal:

```csharp
TimeSpan stdoutFlushDelay = TimeSpan.FromSeconds(3);
string logFilePath = Path.Combine(
    Environment.GetEnvironmentVariable("AZ_BATCH_TASK_DIR"), "stdout.txt");

// The primary task logic is wrapped in a using statement that sends updates to
// the stdout.txt blob in Storage every 15 seconds while the task code runs.
using (ITrackedSaveOperation stdout =
        await taskStorage.SaveTrackedAsync(
        TaskOutputKind.TaskLog,
        logFilePath,
        "stdout.txt",
        TimeSpan.FromSeconds(15)))
{
    /* Code to process data and produce output file(s) */

    // We are tracking the disk file to save our standard output, but the
    // node agent may take up to 3 seconds to flush the stdout stream to
    // disk. So give the file a moment to catch up.
    await Task.Delay(stdoutFlushDelay);
}
```

`Code to process data and produce output file(s)`on lihtsalt kohatäite koodis tööülesande töötaksid tavalisel viisil. Näiteks võib teil laadib andmed Azure Storage ja täidab teisendus või arvutamise seda tähist. Selle koodilõigu oluline osa näitab, kuidas saate mähkida sellist koodi lisamine `using` Blokeeri perioodiliselt värskendada faili [SaveTrackedAsync][net_savetrackedasync].

Funktsiooni `Task.Delay` on vaja see lõpus `using` plokk, et sõlm agent on aega tühjendage Alustuseks standard välja sisu stdout.txt faili sõlme (sõlm agent on programm, mis töötab igal pool sõlm ja tagab käsk kontrolli sõlme ja paketi teenuste vahel). Seda kohe, on võimalik viimase sekundi väljundi jääda. See viivitus ei pruugi olla vaja kõik failid.

>[AZURE.NOTE] Kui lubate faili jälgimise koos SaveTrackedAsync, ainult *lisab* jälitatud fail on jätkunud Azure Storage. Kasutage seda meetodit ainult jälgimise-pööramine logifailid või muid faile, mis on lisatud, mis on, andmete ainult lisatakse faili lõppu, kui see on värskendatud.

## <a name="retrieve-output"></a>Saate tuua väljund

Kui laadite oma nõutud väljundi Azure'i paketi faili põhimõtted teegi kaudu, teete tööülesande - ja töö-kesksele viisil. Saate taotleda väljund antud ülesanne või töö teadmata teest bloobimälu salvestusruumi või isegi faili nimi. Võite lihtsalt öelda, "Anna mulle väljund faili tööülesande *109*."

Järgmised koodilõigu seni, kuni kõik soovitud tööülesannetest, prinditakse väljundi failide ülesande kohta leiate teavet ja seejärel allalaadimine failide salvestusruumi.

```csharp
foreach (CloudTask task in myJob.ListTasks())
{
    foreach (TaskOutputStorage output in
        task.OutputStorage(storageAccount).ListOutputs(
            TaskOutputKind.TaskOutput))
    {
        Console.WriteLine($"output file: {output.FilePath}");

        output.DownloadToFileAsync(
            $"{jobId}-{output.FilePath}",
            System.IO.FileMode.Create).Wait();
    }
}
```

## <a name="task-outputs-and-the-azure-portal"></a>Tööülesande väljundeid ja Azure portaal

Azure'i portaalis kuvatakse tööülesande väljundeid ja logid, mis on kestnud lingitud Azure Storage konto abil nimereeglid, mis on leitud [Azure'i paketi faili põhimõtted README][github_file_conventions_readme]. Saate rakendada need põhimõtted ise enda valitud Meilikausta keeles või faili põhimõtted teegi saate kasutada oma .net-i rakendused.

### <a name="enable-portal-display"></a>Lubada portaali kuvamine

Teie väljundeid portaalis kuvamise lubamiseks peab vastama järgmistele nõuetele:

 1. [Link Azure Storage konto](#requirement-linked-storage-account) paketi kontole.
 2. Säilitus- ja failide jaoks eelmääratletud nimereeglid järgima kui püsib väljundeid. Leiate need põhimõtted määratluse faili põhimõtted teegis [README][github_file_conventions_readme]. Kui kasutate [Azure'i paketi faili põhimõtted] [ nuget_package] teegi püsima oma väljundi selle nõude on täidetud.

### <a name="view-outputs-in-the-portal"></a>Vaate väljundeid portaalis

Azure'i portaalis tööülesande väljundeid ja logide vaatamiseks liikuge tööülesande nime, kelle väljundi huvi, siis klõpsake **Salvestatud väljund faili** või **Salvestatud logid**. Sellel pildil on kuvatud **Salvestatud väljund faili** ülesande ID "007":

![Tööülesande väljundeid blade Azure'i portaalis][2]

## <a name="code-sample"></a>Proovi kood

[PersistOutputs] [ github_persistoutputs] valimi projekt on üks [Azure'i paketi koodinäiteid] [ github_samples] github. See lahendus Visual Studio 2015 näitab, kuidas kasutada Azure'i paketi faili põhimõtted teeki püsima tööülesande väljund püsival salvestusruumi. Valimi käivitamiseks tehke järgmist.

1. Avage projekt **Visual Studio 2015**.
2. Lisada paketi ja salvestusruumi **konto identimisteave** **AccountSettings.settings** Microsoft.Azure.Batch.Samples.Common projekt.
3. **Koostamine** (kuid ei tööta) lahendus. Kui kuvatakse vastav viip, taastamine NuGet-paketid.
4. Azure portaali abil üles soovitud [rakendusepaketi](batch-application-packages.md) **PersistOutputsTask**jaoks. Kaasa selle `PersistOutputsTask.exe` ja selle sõltuvad komplektide ZIP-paketti, määrata rakenduse ID, et "PersistOutputsTask" ja "1.0" rakenduse paketi versioon.
5. **Alustamine** projekti **PersistOutputs** (Käivita).

## <a name="next-steps"></a>Järgmised sammud

### <a name="application-deployment"></a>Rakenduse juurutamine

[Rakenduse paketid](batch-application-packages.md) funktsioon paketi abil on lihtne nii juurutada ja rakendusi, mida oma tööülesandeid täita arvutada sõlmed versioon.

### <a name="installing-applications-and-staging-data"></a>Rakenduste installimine ja vaheetapiandmed

Lugege teemat [rakenduste installimine ja lavastus andmete paketi arvutada sõlmed] [ forum_post] postitada Azure'i paketi Foorum ülevaate eri võimalused oma sõlmed toimingute ettevalmistamine. Kirjutanud üks Azure'i paketi meeskonna liikmed, see postitus on hea krundita võimalust saada failid (sh nii rakenduste ja tööülesande sisendandmete) peale oma Arvuta sõlmed ja mõned iga meetodi silmas pidada.

[forum_post]: https://social.msdn.microsoft.com/Forums/en-US/87b19671-1bdf-427a-972c-2af7e5ba82d9/installing-applications-and-staging-data-on-batch-compute-nodes?forum=azurebatch
[github_file_conventions]: https://github.com/Azure/azure-sdk-for-net/tree/AutoRest/src/Batch/FileConventions
[github_file_conventions_readme]: https://github.com/Azure/azure-sdk-for-net/blob/AutoRest/src/Batch/FileConventions/README.md
[github_persistoutputs]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/PersistOutputs
[github_samples]: https://github.com/Azure/azure-batch-samples
[net_batchclient]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_cloudstorageaccount]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.cloudstorageaccount.aspx
[net_cloudtask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
[net_fileconventions_readme]: https://github.com/Azure/azure-sdk-for-net/blob/AutoRest/src/Batch/FileConventions/README.md
[net_joboutputkind]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.joboutputkind.aspx
[net_joboutputstorage]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.joboutputstorage.aspx
[net_joboutputstorage_saveasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.joboutputstorage.saveasync.aspx
[net_msdn]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[net_prepareoutputasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.cloudjobextensions.prepareoutputstorageasync.aspx
[net_saveasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputstorage.saveasync.aspx
[net_savetrackedasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputstorage.savetrackedasync.aspx
[net_taskoutputkind]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputkind.aspx
[net_taskoutputstorage]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputstorage.aspx
[nuget_manager]: https://docs.nuget.org/consume/installing-nuget
[nuget_package]: https://www.nuget.org/packages/Microsoft.Azure.Batch.Conventions.Files
[portal]: https://portal.azure.com
[storage_explorer]: http://storageexplorer.com/

[1]: ./media/batch-task-output/task-output-01.png "Salvestatud väljundi failide ja salvestatud logid lülitid portaalis"
[2]: ./media/batch-task-output/task-output-02.png "Tööülesande väljundeid blade Azure'i portaalis"