<properties
    pageTitle="Paketi sõlm kasutamine paralleelselt ülesannetega maksimeerimine | Microsoft Azure'i"
    description="Suurendada tõhusust ja madalamad vähem Arvuta sõlmed ja töötab samaaegseid tööülesannete abil iga sõlme on Azure'i paketi pargis"
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
    ms.date="10/25/2016"
    ms.author="marsma" />

# <a name="maximize-azure-batch-compute-resource-usage-with-concurrent-node-tasks"></a>Azure'i paketi Arvuta ressursikasutus samaaegseid sõlm ülesannetega maksimeerimine

Käivitades rohkem kui ühe tööülesande korraga iga MSDN olevate Azure'i paketi, saate maksimeerimine ressursikasutuse sõlmed pool väiksema arvu. Mõned töökoormus see võib põhjustada töö lühem ja alumise maksumus.

Kui kõik soovitud sõlm ressursid pühendades ühe tööülesande kasu mõnel juhul, mitmeid olukordi kasu lubamisel mitu ülesannet nende ressursside jagamiseks:

 - Kui tööülesanded ei saa andmeid jagada **minimeerimine andmete edastamiseks** . Selle stsenaariumi korral võib oluliselt vähendada andmesidetasude väiksema arvu sõlmed ühiskasutusega andmete kopeerimine ja täitmine tööülesannete paralleelselt iga sõlme. See kehtib eriti siis, kui andmed kopeerida iga sõlm tuleb üle geograafiliste piirkondade vahel.

 - **Maximizing mälukasutuse** tööülesanded nõuavad palju mälu, kuid ainult ajal lühikest aega ja täitmise ajal muutuv aeg. Saate tööle vähem, kuid suurem, Arvuta sõlmed rohkem mälu käsitlema tõhus sellise diagrammi. Sõlmed oleks töötab samal ajal iga sõlme mitu ülesannet, kuid iga tööülesande sooviksite kasutada funktsiooni sõlmed rohked mälu korda.

 - Kui vaheline sõlm suhtlemine on vaja on **leevendada sõlm arv piirangud** . Praegu konfigureeritud vaheline sõlm suhtlemiseks kaustu on piiratud 50 Arvuta sõlmed. Kui iga sellise kogumi sõlm saab ülesannete täitmiseks paralleelselt, tööülesannete suurema hulga saab teostada korraga.

 - **Kohapealses imitatsiooniga kobar arvutada**, näiteks, kui te esmalt liigute Arvuta keskkonna Azure. Kui teie praegune kohapealse lahenduse aktiveeritakse mitu tööülesannete kohta MSDN, saate suurendada sõlm tööülesandeid täita täpsemalt selle konfiguratsiooni maksimaalne arv.

## <a name="example-scenario"></a>Näide stsenaarium

Paralleelselt tööülesande täitmise täitmist näiteks Oletame, et teie tööülesande taotlus CPU ja mälu nõuded on nii, et [Standard\_D1](../cloud-services/cloud-services-sizes-specs.md#general-purpose-d) sõlmed on piisavalt. Kuid töö lõpetamiseks nõutav kellaaja 1000 sõlmed on vaja.

Standard kasutamise asemel\_D1 sõlmed, mis on 1 Protsessori tuum, võite kasutada [Standard\_D14](../cloud-services/cloud-services-sizes-specs.md#memory-intensive-d) sõlmed, mis on 16 tuuma ja paralleelsete tööülesande täitmise lubamine. Seetõttu saab *16 korda vähem sõlmed* --asemel 1000 sõlmed, vaid 63 oleks vaja. Lisaks, kui rakenduse suurte failide või andmeid on vaja iga sõlme, töö kestus ja tõhusust on uuesti paranenud andmed kopeeritakse ainult 16 sõlmed.

## <a name="enable-parallel-task-execution"></a>Paralleelne tööülesande täitmise lubamine

Saate konfigureerida Arvuta sõlmed paralleelselt käsu täitmiseks rakenduskausta tasemel. Teegiga paketi .NET seadmine [CloudPool.MaxTasksPerComputeNode] [ maxtasks_net] atribuut on loomisel. Kui kasutate paketi REST API-ga, määrata [maxTasksPerNode] [ rest_addpool] elemendi koosolekukutse kehasse rakenduskausta loomise ajal.

Azure'i paketi võimaldab teil määrata maksimaalne ülesannete kohta sõlm kuni neli korda (4 x) sõlm valdkond arv. Näiteks kui pool on konfigureeritud sõlmed suuruse "Suur" (neli tuuma), siis `maxTasksPerNode` võib olla seatud 16. Valdkond iga sõlm suuruses palju Lisateavet [suurused pilveteenustega](../cloud-services/cloud-services-sizes-specs.md). Teenuse piirangute kohta leiate lisateavet teemast [kvootide ja Azure'i paketi teenuse piirangud](batch-quota-limit.md).

> [AZURE.TIP] Kindlasti arvesse võtta selle `maxTasksPerNode` väärtus, kui te ehitada mõnda [autoscale valemi] [ enable_autoscaling] oma kausta jaoks. Näiteks valem, mis arvutab `$RunningTasks` võib oluliselt mõjutada ülesannete kohta sõlm suurendamist. Lisateabe saamiseks vaadake [automaatselt skaala arvutada sõlmed on Azure'i paketi kogumi](batch-automatic-scaling.md) .

## <a name="distribution-of-tasks"></a>Ülesannete jaotamine

Kui Arvuta sõlmed pargis saate ülesandeid täita samaaegselt, on oluline, et määrata, kuidas soovite toimingud, mida levitatakse kogu kogumi sõlmed.

[CloudPool.TaskSchedulingPolicy] abil[ task_schedule] atribuudi, saate määrata, et tööülesanded määratakse ühtlaselt üle kõik sõlmed kogumi ("levitamine"). Või saate määrata, et võimalikult nii palju tööülesanded määratakse iga sõlme, enne kui tööülesanded määratakse teise sõlme kogumi ("pakkimine").

Näide sellest, kuidas see funktsioon on väärtuslik, kaaluge nende [Standard\_D14](../cloud-services/cloud-services-sizes-specs.md#memory-intensive-d) sõlmed (näites ülaltoodud), mis on konfigureeritud ka [CloudPool.MaxTasksPerComputeNode] [ maxtasks_net] väärtuse 16. Kui [CloudPool.TaskSchedulingPolicy] [ task_schedule] on konfigureeritud on [ComputeNodeFillType] [ fill_type] *Pack*, see maksimeerimine kõik 16 valdkond iga sõlme kasutus ja luba on [autoscaling pool](batch-automatic-scaling.md) Kärbi kasutamata sõlmed kaustast (mis tahes tööülesandeid ilma sõlmed). See vähendab ressursikasutus ja säästab raha.

## <a name="batch-net-example"></a>Paketi .net-i näide

Selle [Paketi .NET] [ api_net] API koodilõigu kuvatakse taotluse luua kausta, mis sisaldab nelja suure sõlmed kuni nelja tööülesannete sõlm kohta. Seda määrab ülesande ajastamine poliitika, mida iga sõlme täitmine toimingud enne tööülesande määramine teise sõlme kogumi. Paketi .NET API abil kaustu lisamise kohta leiate lisateavet teemast [BatchClient.PoolOperations.CreatePool][poolcreate_net].

```csharp
CloudPool pool =
    batchClient.PoolOperations.CreatePool(
        poolId: "mypool",
        targetDedicated: 4
        virtualMachineSize: "large",
        cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "4"));

pool.MaxTasksPerComputeNode = 4;
pool.TaskSchedulingPolicy = new TaskSchedulingPolicy(ComputeNodeFillType.Pack);
pool.Commit();
```

## <a name="batch-rest-example"></a>Paketi ülejäänud näide

Selle [Paketi ülejäänud] [ api_rest] API koodilõigu kuvatakse taotluse luua kausta, mis sisaldab kahte suurte sõlme kuni nelja ülesannete kohta sõlm. REST API abil kaustu lisamise kohta leiate lisateavet teemast [Lisa konto lisamine pool][rest_addpool].

```json
{
  "odata.metadata":"https://myaccount.myregion.batch.azure.com/$metadata#pools/@Element",
  "id":"mypool",
  "vmSize":"large",
  "cloudServiceConfiguration": {
    "osFamily":"4",
    "targetOSVersion":"*",
  }
  "targetDedicated":2,
  "maxTasksPerNode":4,
  "enableInterNodeCommunication":true,
}
```

> [AZURE.NOTE] Saate määrata selle `maxTasksPerNode` elemendi ja [MaxTasksPerComputeNode] [ maxtasks_net] atribuut ainult pool loomise ajal. Ta ei saa muuta pärast on juba loodud.

## <a name="code-sample"></a>Proovi kood

[ParallelNodeTasks] [ parallel_tasks_sample] projekti github on näidatud [CloudPool.MaxTasksPerComputeNode] [ maxtasks_net] atribuut.

See C# konsooli rakendus kasutab [Paketi .NET] [ api_net] teegi loomiseks on üks või mitu Arvuta sõlmed. See käivitab tööülesannete konfigureeritav arv need sõlmed simuleerida muutuv koormus. Rakenduse väljund saate määrata, millised sõlmed täidetud iga tööülesande. Rakendus annab ülevaate töö parameetrid ja kestus. Kokkuvõte osa kaks erinevat käivitatakse valimi rakenduse väljund kuvatakse allpool.

```
Nodes: 1
Node size: large
Max tasks per node: 1
Tasks: 32
Duration: 00:30:01.4638023
```

Esimese proovi taotluse täitmine näitab, et ühe sõlm pool ja vaikesäte üks tööülesanne kohta sõlm, töö kestus on üle 30 minutit.

```
Nodes: 1
Node size: large
Max tasks per node: 4
Tasks: 32
Duration: 00:08:48.2423500
```

Teine käivitada valimi näitab töö kestus märgatav vähenemine. See on pool konfigureeritud nelja ülesannetega ühe sõlme, mis võimaldab paralleelselt tööülesande täitmise peaaegu kvartali aega töö tegemiseks.

> [AZURE.NOTE] Töö kestus rakenduses ülaltoodud kokkuvõtted ei sisalda rakenduskausta loomise ajal. Iga ülaltoodud töökohtade esitati varem loodud kaustu, mille Arvuta sõlmed olid *jõudeoleku* olekus esitamise ajal.

## <a name="next-steps"></a>Järgmised sammud

### <a name="batch-explorer-heat-map"></a>Paketi Exploreri Intensiivsuskaart

[Azure'i paketi Exploreri][batch_explorer], üks Azure'i paketi [valimi rakenduste][github_samples], sisaldab *Intensiivsuskaardi* funktsioon, mis pakub visualiseerimine tööülesande täitmise. Kui olete täitmise [ParallelTasks] [ parallel_tasks_sample] valimi rakenduse, saate Intensiivsuskaardi funktsiooni hõlpsalt iga sõlme paralleelselt ülesannete täitmiseks.

![Paketi Exploreri Intensiivsuskaart][1]

*Paketi Exploreri Intensiivsuskaart, millel neli sõlmed, koos iga sõlme praegu täidetavad neli tööülesannete kogumi*

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[batch_explorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[cloudpool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[enable_autoscaling]: https://msdn.microsoft.com/library/azure/dn820173.aspx
[fill_type]: https://msdn.microsoft.com/library/microsoft.azure.batch.common.computenodefilltype.aspx
[github_samples]: https://github.com/Azure/azure-batch-samples
[maxtasks_net]: http://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.maxtaskspercomputenode.aspx
[rest_addpool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[parallel_tasks_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/ParallelTasks
[poolcreate_net]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.createpool.aspx
[task_schedule]: https://msdn.microsoft.com/library/microsoft.azure.batch.cloudpool.taskschedulingpolicy.aspx

[1]: ./media/batch-parallel-node-tasks\heat_map.png
