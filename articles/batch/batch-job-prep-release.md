<properties
    pageTitle="Töö ettevalmistamine ja cleanup paketi | Microsoft Azure'i"
    description="Töö taseme ettevalmistustööd abil minimeerida andmete edastamiseks Azure'i paketi Arvuta sõlmed ja vabastage tööülesannete sõlm cleanup töö lõpetamist juures."
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
    ms.date="09/16/2016"
    ms.author="marsma" />

# <a name="run-job-preparation-and-completion-tasks-on-azure-batch-compute-nodes"></a>Azure'i paketi Arvuta sõlmed käitamist ettevalmistamine ja lõpetamise tööülesanded

 Azure'i pakett on sageli peab häälestamine enne selle tööülesanded on täidetud ja pärast töö hooldustööd, kui selle tööülesanded on lõpule viidud. Peate oma Arvuta sõlmed levinud tööülesande sisendandmete alla laadida või tööülesande väljundi andmete üleslaadimine Azure Storage pärast töö lõpulejõudmist. **Töö ettevalmistamine** ja **töö vabastage** tööülesannete abil saate teha järgmisi toiminguid.

## <a name="what-are-job-preparation-and-release-tasks"></a>Mis on töö ettevalmistamine ja vabastage tööülesanded?

Enne mõne tööülesannetest käivitada, käivitatakse kõik Arvuta sõlmed ajastatud vähemalt ühe tööülesande ettevalmistamine tööülesanne. Kui töö on lõpule jõudnud, töötab iga sõlme täidetud vähemalt ühe tööülesande kogumi väljaanne tööülesanne. Nagu ülesannetega tavaline paketi, saate määrata käsurea ettevalmistamiseks või väljaanne tööülesanne käitamisel kasutada.

Tööülesannete ettevalmistamine ja väljaanne pakkuda tuttavad paketi tööülesande funktsioone, nagu faili alla laadida ([ressurss faile][net_job_prep_resourcefiles]), tõstetud täitmise, kohandatud keskkonna muutujate, kuni täitmise kestus, proovi uuesti count ja faili säilitamise aeg.

Järgmistest jaotistest saate teada, kuidas kasutada [JobPreparationTask] [ net_job_prep] ja [JobReleaseTask] [ net_job_release] tunnid leitud [Paketi .NET] [ api_net] teek.

> [AZURE.TIP] Tööülesannete ettevalmistamine ja väljaanne on eriti abi "jagatud pool" keskkonnas, mis kogumi Arvuta sõlmed töö käivitatakse vahele ja kasutab palju tööd.

## <a name="when-to-use-job-preparation-and-release-tasks"></a>Millal kasutada töö ettevalmistamine ja vabastage tööülesanded

Töö ettevalmistamine ja tööülesannete väljaanne on hästi sobida järgmistes olukordades:

**Levinud tööülesande andmete allalaadimine**

Pakett-tööde sageli vaja andmete ühised sisendina projekti ülesannete jaoks. Näiteks iga päev risk analüüsi arvutused andmetes on töö kohased, veel levinud töösse kõik toimingud. Market andmed, sageli mitu gigabaiti suurus, tuleks laaditakse iga MSDN ainult üks kord, et kõik toimingud, mida sõlme töötab, saate seda kasutada. **Ettevalmistamine tööülesandega** abil saate alla laadida andmed iga sõlme enne muude toimingute täitmist töö.

**Töö-ja tööülesande kustutamine**

"Jagatud pool" keskkonnas, kui on pool Arvuta sõlmed ei ole kasutusest kõrvaldatud töö vahel, peate töö käivitatakse vaheline andmete kustutamiseks. Võib juhtuda sõlmed kettaruumi säästmine või oma ettevõtte turbepoliitikate vastavad. **Tööülesanne väljaanne** abil saate töö ettevalmistamine tööülesande alla laadida või tööülesande täitmise ajal loodud andmed kustutada.

**Logi säilitamine**

Võite talletaks tööülesannete andvate logifailid või võib-olla ootamatult sulguda dump faile, mis võib olla loodud nurjunud rakendused. **Tööülesanne väljaanne** kasutamine sellisel juhul tihendamine ja üles laadida andmed on [Azure Storage] [ azure_storage] konto.

>[AZURE.TIP] Teine võimalus jää püsima logid töö ja muud tööülesande väljundi andmed on kasutada [Azure'i paketi faili põhimõtted](batch-task-output.md) teeki.

## <a name="job-preparation-task"></a>Tööülesandega ettevalmistamine

Enne mõne töö ülesannete täitmise, käivitab paketi ettevalmistamine tööülesanne iga MSDN, mis on ajastatud ülesande käivitamine. Vaikimisi ootab paketi teenuse tööülesanne ettevalmistamine enne käivitamist käivitada sõlme ajastatud ülesannete lõpule viia. Siiski saate konfigureerida teenus pole ootama. Kui sõlme taaskäivitamist töö ettevalmistamiseks käivitub uuesti, kuid samuti saate keelata seda käitumist.

Ettevalmistamine tööülesanne on täide ainult sõlmi, mis on ajastatud ülesanne. See takistab mittevajalike ettevalmistamine tööülesande juhuks, kui sõlm pole määratud tööülesande. See võib juhtuda siis, kui töökohta tööülesannete arv on väiksem kui sõlmed pargis arv. See kehtib ka siis, kui [samaaegseid ülesande täitmine](batch-parallel-node-tasks.md) on lubatud, mis jätab mõned sõlmed jõude kui tööülesannete arv on väiksem kui kokku võimalike samaaegseid tööülesanded. Ei tööta jõude sõlmed ettevalmistamine tööülesandega, saate andmesidetasude kulutada vähem raha.

> [AZURE.NOTE] [JobPreparationTask] [ net_job_prep_cloudjob] erineb [CloudPool.StartTask] [ pool_starttask] , et JobPreparationTask aktiveeritakse iga töö alguses tuleks StartTask aktiveeritakse ainult siis, kui Arvuta sõlme esmalt ühendab on või taaskäivitamist.

## <a name="job-release-task"></a>Tööülesanne väljaanne

Pärast töö on märgitud lõpetatuks, käivitatakse iga sõlme täidetud vähemalt ühe tööülesande kogumi väljaanne tööülesanne. Väljastanud lõpetada taotluse lõpetatuks märkida tööd. Paketi teenuse seejärel seab töö *lõpetada*, aktiivne või esitatava tööülesannete, tööga seotud lõpeb ja töötab väljaanne tööülesanne. Töö liigub *lõpetatud* olek.

> [AZURE.NOTE] Töö kustutamise teostab ka väljaanne tööülesanne. Juhul, kui tööd on juba lõpetatud, vabasta tööülesanne on ei tööta teist korda töö hiljem kustutatakse.

## <a name="job-prep-and-release-tasks-with-batch-net"></a>Töö prep ja ülesannete paketi .net-i versioon

Määrata tööülesandele töö ettevalmistamine kasutamiseks on [JobPreparationTask] [ net_job_prep] objekti oma töö [CloudJob.JobPreparationTask] [ net_job_prep_cloudjob] atribuut. Samuti käivitamine on [JobReleaseTask] [ net_job_release] ja selle määrata oma töö [CloudJob.JobReleaseTask] [ net_job_prep_cloudjob] atribuudi seadmiseks väljaanne tööülesanne.

Klõpsake selle koodilõigu `myBatchClient` on [BatchClient]eksemplari[net_batch_client], ja `myPool` on mõne olemasoleva kausta paketi kontol.

```csharp
// Create the CloudJob for CloudPool "myPool"
CloudJob myJob =
    myBatchClient.JobOperations.CreateJob(
        "JobPrepReleaseSampleJob",
        new PoolInformation() { PoolId = "myPool" });

// Specify the command lines for the job preparation and release tasks
string jobPrepCmdLine =
    "cmd /c echo %AZ_BATCH_NODE_ID% > %AZ_BATCH_NODE_SHARED_DIR%\\shared_file.txt";
string jobReleaseCmdLine =
    "cmd /c del %AZ_BATCH_NODE_SHARED_DIR%\\shared_file.txt";

// Assign the job preparation task to the job
myJob.JobPreparationTask =
    new JobPreparationTask { CommandLine = jobPrepCmdLine };

// Assign the job release task to the job
myJob.JobReleaseTask =
    new JobPreparationTask { CommandLine = jobReleaseCmdLine };

await myJob.CommitAsync();
```

Nagu varem mainitud, käivitatakse vabasta tööülesanne, kui tööd on lõpetatud või kustutada. Töökoha [JobOperations.TerminateJobAsync]lõpetada[net_job_terminate]. Töö koos [JobOperations.DeleteJobAsync]kustutamine[net_job_delete]. Tavaliselt lõpetada või töö kustutamine, kui selle tööülesanded on lõpule viidud, või kui ajalõpp, mille olete määratlenud jõudnud.

```csharp
// Terminate the job to mark it as Completed; this will initiate the
// Job Release Task on any node that executed job tasks. Note that the
// Job Release Task is also executed when a job is deleted, thus you
// need not call Terminate if you typically delete jobs after task completion.
await myBatchClient.JobOperations.TerminateJobAsy("JobPrepReleaseSampleJob");
```

## <a name="code-sample-on-github"></a>Github koodi näidis

Lugege teemat töö ettevalmistamine ja vabastada tööülesannete toimingus, märkige ruut välja [JobPrepRelease] [ job_prep_release_sample] valimi projekti github. Selle rakenduse konsooli teeb järgmist.

1. Loob on kaks "väike" sõlme.
2. Loob töö töö koostamisel väljaanne, ja ülesannete.
3. Töötab töö ettevalmistamine ülesanne, mis kirjutab esmalt sõlm ID on sõlm "ühiskasutuses" Directorys tekstifaili.
4. Tööülesande töötab iga sõlm, mis kirjutab selle ülesande ID sama tekstifail.
5. Kui kõik tööülesanded on lõpule viidud (või aeg, mille jõutakse), prinditakse iga sõlme konsooli teksti faili sisu.
6. Kui töö on valmis, käivitatakse väljaanne tööülesandega sõlme faili kustutada.
6. Prindib välju koodid töö ettevalmistamine ja väljaanne tööülesanded iga sõlme, mil nad täidetud.
7. Pausid täitmise töö ja/või kausta kustutamise kinnitamise lubama.

Valimi rakendusest väljund on järgmine:

```
Attempting to create pool: JobPrepReleaseSamplePool
Created pool JobPrepReleaseSamplePool with 2 small nodes
Checking for existing job JobPrepReleaseSampleJob...
Job JobPrepReleaseSampleJob not found, creating...
Submitting tasks and awaiting completion...
All tasks completed.

Contents of shared\job_prep_and_release.txt on tvm-2434664350_1-20160623t173951z:
-------------------------------------------
tvm-2434664350_1-20160623t173951z tasks:
  task001
  task004
  task005
  task006

Contents of shared\job_prep_and_release.txt on tvm-2434664350_2-20160623t173951z:
-------------------------------------------
tvm-2434664350_2-20160623t173951z tasks:
  task008
  task002
  task003
  task007

Waiting for job JobPrepReleaseSampleJob to reach state Completed
...

tvm-2434664350_1-20160623t173951z:
  Prep task exit code:    0
  Release task exit code: 0

tvm-2434664350_2-20160623t173951z:
  Prep task exit code:    0
  Release task exit code: 0

Delete job? [yes] no
yes
Delete pool? [yes] no
yes

Sample complete, hit ENTER to exit...
```

>[AZURE.NOTE] Kuna sõlmed (mõned sõlmed on valmis ülesandeid teistele enne) uue pargis muutuv loomine ja käivitamine aeg, võite näha väljund. Täpsemalt, kuna tööülesanded kiiresti lõpule viia, üks selle kausta sõlmed võivad täita kõik projekti tööülesanded. Sellisel juhul te teate, et tööks ettevalmistamine ja väljaanne tööülesanded puuduvad sõlm, mida pole tööülesandeid.

### <a name="inspect-job-preparation-and-release-tasks-in-the-azure-portal"></a>Kontrolli töö ettevalmistamine ja väljaanne ülesandeid Azure'i portaalis

Valimi rakenduse käivitamisel saate kasutada [Azure portaali] [ portal] töö ja ülesannete atribuutide vaatamiseks või isegi alla laadida ühiskasutusega faili, mida on muudetud projekti tööülesanded.

Pildil kuvatakse **ettevalmistamine tööülesannete blade** Azure portaali pärast Käivita valimi rakenduse. Pärast tööülesannete lõpetanud *JobPrepReleaseSampleJob* atribuute (kuid enne kustutamist oma töö ja pool) ja klõpsake nuppu **ettevalmistustööd** või **väljaanne, tööülesannete** , nende atribuutide vaatamiseks.

![Töö ettevalmistamine atribuudid Azure'i portaalis][1]

## <a name="next-steps"></a>Järgmised sammud

### <a name="application-packages"></a>Rakenduse paketid

Lisaks ettevalmistamine tööülesandega, saate ka [rakenduse paketid](batch-application-packages.md) funktsiooni partii tööülesande täitmise Arvuta sõlmed valmistuda. See funktsioon on eriti kasulik juurutamine rakendusi, mis ei nõua installer, rakendusi, mis sisaldavad palju faile (100 +) või rakendusi, mis nõuavad range versiooni kontrollimine.

### <a name="installing-applications-and-staging-data"></a>Rakenduste installimine ja vaheetapiandmed

See MSDN-i Foorum post annab ülevaate ettevalmistamine oma sõlmed tööülesannete töötab mitu võimalust:

[Rakenduste installimine ja vaheetapiandmed töölehe arvutada sõlmed][forum_post]

Kirjutanud üks Azure'i paketi meeskonna liikmed, arutatakse, mitu meetodid, mille abil saate juurutada rakendused ja arvutada sõlmed.

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_listjobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[azure_storage]: https://azure.microsoft.com/services/storage/
[portal]: https://portal.azure.com
[job_prep_release_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/JobPrepRelease
[forum_post]: https://social.msdn.microsoft.com/Forums/en-US/87b19671-1bdf-427a-972c-2af7e5ba82d9/installing-applications-and-staging-data-on-batch-compute-nodes?forum=azurebatch
[net_batch_client]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudjob]:https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_job_prep]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobpreparationtask.aspx
[net_job_prep_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobpreparationtask.aspx
[net_job_prep_resourcefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobpreparationtask.resourcefiles.aspx
[net_job_delete]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.deletejobasync.aspx
[net_job_terminate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.terminatejobasync.aspx
[net_job_release]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobreleasetask.aspx
[net_job_release_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobreleasetask.aspx
[pool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx

[net_list_certs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.certificateoperations.listcertificates.aspx
[net_list_compute_nodes]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listcomputenodes.aspx
[net_list_job_schedules]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobscheduleoperations.listjobschedules.aspx
[net_list_jobprep_status]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobpreparationandreleasetaskstatus.aspx
[net_list_jobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx
[net_list_nodefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listnodefiles.aspx
[net_list_pools]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listpools.aspx
[net_list_schedule_jobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobscheduleoperations.listjobs.aspx
[net_list_task_files]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.listnodefiles.aspx
[net_list_tasks]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listtasks.aspx

[1]: ./media/batch-job-prep-release/portal-jobprep-01.png
