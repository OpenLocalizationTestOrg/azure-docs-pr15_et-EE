<properties
    pageTitle="Azure'i paketi tõhusa loendi päringute | Microsoft Azure'i"
    description="Jõudluse suurendamine, filtreerides päringute paketi ressursse, nt teid, töö, tööülesannete teabe taotlemisel ja arvutada sõlmed."
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

# <a name="query-the-azure-batch-service-efficiently"></a>Päringu Azure'i paketi teenuse tõhus

Siit saate teada, kuidas andmeid, mis on tagastatud teenuse, kui päringu töö, tööülesanded ja arvutada sõlmed [Paketi .net-i] abil vähendades Azure'i paketi rakenduse jõudluse suurendamiseks[ api_net] teek.

Peaaegu kõik paketi rakenduste peate täitma mõnda tüüpi jälgimise või muude päringute teenuse paketi sageli intervalliga toiming. Näiteks, määratlemaks, kas on mis tahes ootel tööülesanded allesjäänud töö, peate andmete toomine iga tööülesande töö. Sõlmed olevate oleku määratlemiseks peavad teil olema igal pool sõlme andmeid. Selles artiklis selgitatakse, kuidas käivitada selliste päringute kõige tõhusam viis.

## <a name="meet-the-detaillevel"></a>Koosolek on DetailLevel

Valmistamisel paketi rakenduse saate üksuste meeldib töö, tööülesannete ja Arvuta sõlmed nummerdada tuhandeliste. Kui taotluse teavet nende ressursse, potentsiaalselt suurt hulka andmeid peab "cross kaabel" paketi teenuse rakenduse iga päringu kohta. Üksuste arv ja tüüp, mis on päringu tagastatud vähendades saate suurendada päringute kiirust, mistõttu rakenduse jõudlus.

Selle [Paketi .NET] [ api_net] API koodilõigu on loetletud *iga* tööülesande, mis on seotud tööd, koos *kõigi* iga tööülesande atribuudid:

```csharp
// Get a collection of all of the tasks and all of their properties for job-001
IPagedEnumerable<CloudTask> allTasks =
    batchClient.JobOperations.ListTasks("job-001");
```

Palju tõhusam loendi päringu, saate teha, rakendades päringusse "detail level". Saate seda teha, on [ODATADetailLevel] sisestamise[ odata] objekti [JobOperations.ListTasks] [ net_list_tasks] meetod. Selle koodilõigu tagastab ainult ID, käsurea ja lõpuleviidud tööülesannete Arvuta sõlm atribuudid:

```csharp
// Configure an ODATADetailLevel specifying a subset of tasks and
// their properties to return
ODATADetailLevel detailLevel = new ODATADetailLevel();
detailLevel.FilterClause = "state eq 'completed'";
detailLevel.SelectClause = "id,commandLine,nodeInfo";

// Supply the ODATADetailLevel to the ListTasks method
IPagedEnumerable<CloudTask> completedTasks =
    batchClient.JobOperations.ListTasks("job-001", detailLevel);
```

Näiteks sel juhul, kui on tööülesannete töö, tuhandeliste teise päringu tulemusi tavaliselt tagastatakse palju kiiremini, kui esimene. Lisateavet kasutamise ODATADetailLevel, kui olete loendi üksuste paketi .net-i API-ga on kaasatud [all](#efficient-querying-in-batch-net).

> [AZURE.IMPORTANT]
> Soovitame selle saate *alati* esitama objekti ODATADetailLevel .NET API kõned maksimaalne tõhusaks ja rakenduse jõudlust. Üksikasjalike taseme määramisega aitab teil langetada paketi vastuse ajad, parandada võrgu kasutamine ja minimeerida mälukasutuse klientrakendustes.

## <a name="filter-select-and-expand"></a>Filtreerimine, valige ja laiendamine

[Paketi .NET] [ api_net] ja [Paketi ülejäänud] [ api_rest] API-d pakuvad vähendab tagastatud loendi üksuste arvu, samuti teabe, mis tagastatakse iga võimalust. Saate teha määramisega **filter**, **Valige**ja **laiendage stringide** läbimiseks loendi päringud.

### <a name="filter"></a>Filtreerimine
Filtri string on avaldis, mis vähendab tagastatud üksuste arvu. Näiteks ainult töö töötab tööülesannete loendi loendis või ainult Arvuta sõlmed, mis on tööülesannete käivitamiseks valmis.

- Filtri stringi koosneb ühe või mitme avaldisi koos avaldis, mis koosneb atribuudi nimi, tehtemärk, ja väärtuse. Määratud atribuudid on iga üksuse tüüp, mida saate päringu, on tehtemärgid, mis on toetatud iga atribuudi.
- Mitme avaldiste kombineerimiseks loogika tehtemärkide abil `and` ja `or`.
- Selles näites stringi loendite filtreerimine ainult töötab "renderdada" tööülesanded: `(state eq 'running') and startswith(id, 'renderTask')`.

### <a name="select"></a>Valige
Valige stringi piirab atribuudi väärtused, mis tagastatakse iga üksuse jaoks. Määrake atribuudi nimede loend ja ainult atribuudi väärtused on tagastatud üksuste päringutulemites.

- Valige string koosneb atribuudi nimede komaga eraldatud loend. Saate määrata atribuute päring on suunatud üksuse tüüp.
- See näide Vali string saate määrata, et ainult kolme kinnisvarahindade tuleks tagastada siis iga tööülesande: `id, state, stateTransitionTime`.

### <a name="expand"></a>Laiendage
Laienda stringi vähendab API kõned, mida on vaja teatud teavet. Kui kasutate mõnda Laienda stringi, saab iga üksuse kohta lisateabe ühe API kõne. Asemel esimese saamiseks seejärel paluda teavet iga üksuse loendis üksuste loendit saate ühe API kõne sama teabe saamiseks mõne Laienda string. Vähem API kõned tähendab, et parema jõudluse.

- Sarnaselt valige stringi, Laienda stringi määrab, kas teatud andmed on kaasatud loendi päringutulemite.
- Laienda string on toetatud ainult siis, kui seda kasutatakse loetledes töö, töö ajakavade, tööülesannete ja kaustu. Praegu see toetab ainult statistika teavet.
- Kui kõik atribuudid on nõutavad ja valige stringi pole määratud, Laienda stringi *tuleb* kasutada statistika teabe saamiseks. Kui valige stringi kasutatakse saada alamhulga atribuudid, siis `stats` saab määrata valige stringi ja Laienda stringi ei pea olema määratud.
- Selles näites laiendamine stringi saate määrata, et statistika teabe tuleks tagastada siis loendis iga üksuse jaoks: `stats`.

> [AZURE.NOTE] Kui mõne päringu kolme stringi tüüpi ehitamine (filtreerimine, valige ja laiendamine), peate kontrollima atribuutide nimesid ja juhul vastavad mis vastavate REST API element. Näiteks töötades.NET [CloudTask](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask) ainekursust, peate määrama **state** asemel **State**, ehkki .net-i atribuut on [CloudTask.State](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.state). Saate vaadata tabelitest all atribuutide vastenduste .net-i ja REST API-de jaoks.

### <a name="rules-for-filter-select-and-expand-strings"></a>Reeglite filter, valige ja stringide laiendamine

- Atribuutide nimede filtreerimine, valige ja laiendage stringide peaks kuvatama [Paketi ülejäänud] samamoodi nagu[ api_rest] API - isegi siis, kui kasutate [Paketi .NET] [ api_net] või mõni selle muude paketi SDK-d.
- Atribuut kõik nimed on tõstutundlikud, kuid kinnisvarahindade on tõstutundlikud.
- Kuupäeva/kellaaja stringide võib olla üks kahest vormingud ja koos ees `DateTime`.

  - W3C – DTF vormingu näide:`creationTime gt DateTime'2011-05-08T08:49:37Z'`
  - RFC 1123 vormingu näide:`creationTime gt DateTime'Sun, 08 May 2011 08:49:37 GMT'`
- Kahendmuutujaga stringid on kas `true` või `false`.
- Kui määratud on sobimatu atribuut või tehtemärk, on `400 (Bad Request)` tulemuseks viga.

## <a name="efficient-querying-in-batch-net"></a>Tõhusa päringute paketi .NET-is

Sees [Paketi .NET] [ api_net] API, [ODATADetailLevel] [ odata] klassi kasutatakse sisestamise filter, valige ja laiendamine stringide loendi toimingud. Klassi ODataDetailLevel on kolm avaliku stringi atribuudid, mida saate määratud ehitaja või objekti pannud. Saate seejärel edastama ODataDetailLevel objekti parameetrina loendi erinevate toimingute nt [ListPools][net_list_pools], [ListJobs][net_list_jobs], ja [ListTasks][net_list_tasks].

- [ODATADetailLevel][odata]. [FilterClause] [ odata_filter]: Tagastatud üksuste arv piirata.
- [ODATADetailLevel][odata]. [SelectClause] [ odata_select]: Saate määrata, millised kinnisvarahindade tagastatakse iga üksusega.
- [ODATADetailLevel][odata]. [ExpandClause] [ odata_expand]: Too andmed kõigi üksuste ühe API kõne asemel eraldi iga üksuse jaoks.

Järgmised koodilõigu kasutab paketi .NET API tõhus päringu paketi teenuse statistika määratud kaustu. Selle stsenaariumi korral paketi kasutajal on nii kaustu. Testi rakenduskausta ID-d on eesliide "test" ja "tootekataloogi" on eesliide tootmiskausta ID-d. Koodilõigu, *myBatchClient* on õigesti lähtestatud eksemplari [BatchClient](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient) klassi.

```csharp
// First we need an ODATADetailLevel instance on which to set the filter, select,
// and expand clause strings
ODATADetailLevel detailLevel = new ODATADetailLevel();

// We want to pull only the "test" pools, so we limit the number of items returned
// by using a FilterClause and specifying that the pool IDs must start with "test"
detailLevel.FilterClause = "startswith(id, 'test')";

// To further limit the data that crosses the wire, configure the SelectClause to
// limit the properties that are returned on each CloudPool object to only
// CloudPool.Id and CloudPool.Statistics
detailLevel.SelectClause = "id, stats";

// Specify the ExpandClause so that the .NET API pulls the statistics for the
// CloudPools in a single underlying REST API call. Note that we use the pool's
// REST API element name "stats" here as opposed to "Statistics" as it appears in
// the .NET API (CloudPool.Statistics)
detailLevel.ExpandClause = "stats";

// Now get our collection of pools, minimizing the amount of data that is returned
// by specifying the detail level that we configured above
List<CloudPool> testPools =
    await myBatchClient.PoolOperations.ListPools(detailLevel).ToListAsync();
```

> [AZURE.TIP] Eksemplari [ODATADetailLevel] [ odata] mis on konfigureeritud valimine ja Laienda klauslitest saab edasi ka vastavad toomine meetodid, nt [PoolOperations.GetPool](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.getpool.aspx), piirata summa tagastatavad andmed.

## <a name="batch-rest-to-net-api-mappings"></a>Paketi ülejäänud .NET API vastendused

Atribuut nimede filtreerimine, valige ja laiendage stringide *peab* vastavalt oma REST API kolleegidega, nii nimi ja juhtum. Allpool tabelitest .net-i ja REST API kolleegidega vaheliste vastenduste.

### <a name="mappings-for-filter-strings"></a>Stringide filtri vastendused

- **.Net-i loendi meetodid**: .NET API meetodi selles veerus kuvatakse [ODATADetailLevel] aktsepteerib[ odata] objekti parameetrina.
- **Ülejäänud loendi taotlused**: iga REST API lehe jaotises selle veeru seotud sisaldab tabeli, mis määrab stringide *filtri* atribuudid ja toiminguid, mis on lubatud. Te kasutate nende atribuutide nimesid ja toimingud, kui te ehitada mõne [ODATADetailLevel.FilterClause] [ odata_filter] string.

| .Net-i loendi meetodid | ÜLEJÄÄNUD loendi taotlused |
|---|---|
| [CertificateOperations.ListCertificates][net_list_certs] | [Loendi kontol serdid][rest_list_certs]
| [CloudTask.ListNodeFiles][net_list_task_files] | [Tööülesande seostatud failide loend][rest_list_task_files]
| [JobOperations.ListJobPreparationAndReleaseTaskStatus][net_list_jobprep_status] | [Töö ettevalmistamine ja tööülesannete väljaanne töö oleku loend][rest_list_jobprep_status]
| [JobOperations.ListJobs][net_list_jobs] | [Konto töid loendis][rest_list_jobs]
| [JobOperations.ListNodeFiles][net_list_nodefiles] | [Faili sõlm][rest_list_nodefiles]
| [JobOperations.ListTasks][net_list_tasks] | [Tööga seotud ülesannete nimekiri][rest_list_tasks]
| [JobScheduleOperations.ListJobSchedules][net_list_job_schedules] | [Töö ajakava kontol loend][rest_list_job_schedules]
| [JobScheduleOperations.ListJobs][net_list_schedule_jobs] | [Töö ajakava seotud töödest loend][rest_list_schedule_jobs]
| [PoolOperations.ListComputeNodes][net_list_compute_nodes] | [Loendi Arvuta sõlmed pargis][rest_list_compute_nodes]
| [PoolOperations.ListPools][net_list_pools] | [Loendis kaustu kontol][rest_list_pools]

### <a name="mappings-for-select-strings"></a>Valige stringide puhul vastendused

- **Paketi .NET tüüpi**: paketi .NET API tüübid.
- **REST API üksuste**: selles veerus iga leht sisaldab ühte või mitut tabelit, mis loendis Tüüp REST API atribuutide nimesid. Kui te ehitada *Valige* stringide kasutatakse neid atribuudi nimesid. Te kasutate neid sama atribuutide nimesid, kui te ehitada mõne [ODATADetailLevel.SelectClause] [ odata_select] string.

| Paketi .net-i andmetüübid | REST API üksused |
|---|---|
| [Sert][net_cert] | [Serdi kohta teabe saamine][rest_get_cert] |
| [CloudJob][net_job] | [Töö kohta teabe saamine][rest_get_job] |
| [CloudJobSchedule][net_schedule] | [Töö ajakava kohta teabe saamine][rest_get_schedule] |
| [ComputeNode][net_node] | [Sõlm kohta teabe saamine][rest_get_node] |
| [CloudPool][net_pool] | [On kohta teabe saamine][rest_get_pool] |
| [CloudTask][net_task] | [Tööülesande kohta teabe saamine][rest_get_task] |

## <a name="example-construct-a-filter-string"></a>Näide: ehitada filter string

Kui te ehitada filter string [ODATADetailLevel.FilterClause][odata_filter], küsige ülaltoodud tabelis jaotises "Vastendused filter stringide puhul" Otsi REST API dokumentatsiooni leht, mis vastab loendis toiming, mida soovite teha. Leiate lehe multirow esimeses tabelis filtreeritavad atribuudid ja nende toetatud tehtemärke. Kui soovite, et tuua kõik tööülesanded, kelle välju kood on nullist erinev, näiteks [tööga seotud ülesannete] loendis selle rea[ rest_list_tasks] määrab rakendatav atribuut string ja lubatud tehtemärgid:

| Atribuut | Lubatud | Tüüp |
| :--- | :--- | :--- |
| `executionInfo/exitCode` | `eq, ge, gt, le , lt` | `Int` |

Seega oleks filter stringi loetelu kõik tööülesanded väljumise nullist erinev koodiga jaoks:

`(executionInfo/exitCode lt 0) or (executionInfo/exitCode gt 0)`

## <a name="example-construct-a-select-string"></a>Näide: ehitada valige string

Ehitada [ODATADetailLevel.SelectClause][odata_select], küsige ülaltoodud tabelis jaotises "Vastendused valige stringide puhul" ja liikuge REST API leht, mis vastab üksus, mida teil on loetletakse tüüp. Leiate lehe multirow esimeses tabelis valitav atribuudid ja nende toetatud tehtemärke. Kui soovite ID ja käsurea tööülesannete loendi toomiseks, näiteks leiate nende ridade tabeli rakendatav [tööülesande kohta teabe saamiseks][rest_get_task]:

| Atribuut | Tüüp | Märkmete |
| :--- | :--- | :--- |
| `id` | `String` | `The ID of the task.` |
| `commandLine` | `String` | `The command line of the task.` |

Seejärel oleks valige string ID ja käsurea iga loetletud tööülesandega, sealhulgas:

`id, commandLine`

## <a name="code-samples"></a>Koodinäiteid

### <a name="efficient-list-queries-code-sample"></a>Tõhusa loendi päringute koodi näidis

Tutvuge [EfficientListQueries] [ efficient_query_sample] valimi projekti github, et näha, kuidas tõhusa loendi päringute võivad mõjutada jõudlust rakenduses. See C# konsooli rakendus loob ja suure hulga ülesanded lisatakse tööd. Seejärel on mitu kõned [JobOperations.ListTasks] [ net_list_tasks] meetod ja möödub [ODATADetailLevel] [ odata] objekte, mis on konfigureeritud erinevad atribuudiväärtused, et valida tagastatavad andmed. See tekitab väljundi sarnaneb järgmisega.

```
Adding 5000 tasks to job jobEffQuery...
5000 tasks added in 00:00:47.3467587, hit ENTER to query tasks...

4943 tasks retrieved in 00:00:04.3408081 (ExpandClause:  | FilterClause: state eq 'active' | SelectClause: id,state)
0 tasks retrieved in 00:00:00.2662920 (ExpandClause:  | FilterClause: state eq 'running' | SelectClause: id,state)
59 tasks retrieved in 00:00:00.3337760 (ExpandClause:  | FilterClause: state eq 'completed' | SelectClause: id,state)
5000 tasks retrieved in 00:00:04.1429881 (ExpandClause:  | FilterClause:  | SelectClause: id,state)
5000 tasks retrieved in 00:00:15.1016127 (ExpandClause:  | FilterClause:  | SelectClause: id,state,environmentSettings)
5000 tasks retrieved in 00:00:17.0548145 (ExpandClause: stats | FilterClause:  | SelectClause: )

Sample complete, hit ENTER to continue...
```

Nagu on näidatud möödunud korda, võite märgatavalt vähendada päringu vastuse korda atribuutide ja tagastatud üksuste arv piirata. Leiate [Azure'i-paketi-näidised] kõnealuse ja ka muude valimi projektide[ github_samples] hoidla github.

### <a name="batchmetrics-library-and-code-sample"></a>BatchMetrics teek ja koodi näidis

Lisaks EfficientListQueries koodi ülaltoodud leiate [BatchMetrics] [ batch_metrics] projekti [azure-paketi-näidised] [ github_samples] GitHub hoidla. Projekti BatchMetrics valimi näitab, kuidas tõhusaks jälgimiseks Azure'i paketi töö paketi API abil.

[BatchMetrics] [ batch_metrics] valimi sisaldab .net-i klassi teegi projekt, mis saate lisada oma projekte ja lihtsat käsurea programmi kasutada ja teegi kasutamist.

Projekti valimi jooksul näitab järgmisi toiminguid:

1. Selleks, et alla laadida ainult atribuudid, peate teatud atribuutide valimine
2. Selleks, et alla laadida ainult muudatused alates viimase päringu oleku üleminek korda filtreerimine

Näiteks kuvatakse BatchMetrics teegis järgmisel viisil. Tagastab see ODATADetailLevel, mis määrab, et ainult selle `id` ja `state` atribuudid tuleks saada üksused, mis on esitatakse selle kohta päring. Samuti määrab, et ainult üksustele, kelle olek on muutunud määratud `DateTime` parameeter tuleks tagastada.

```csharp
internal static ODATADetailLevel OnlyChangedAfter(DateTime time)
{
    return new ODATADetailLevel(
        selectClause: "id, state",
        filterClause: string.Format("stateTransitionTime gt DateTime'{0:o}'", time)
    );
}
```

## <a name="next-steps"></a>Järgmised sammud

### <a name="parallel-node-tasks"></a>Paralleelne sõlm tööülesanded

[Azure'i paketi maksimeerimine arvutada ressursikasutus samaaegseid sõlm ülesannetega](batch-parallel-node-tasks.md) on teise artiklis paketi rakenduse jõudlusega seotud. Teatud tüüpi töökoormus saavad paralleelselt ülesannete täitmise kohta suuremat--, kuid vähem--arvutada sõlmed. Tutvuge [näiteks sel juhul](batch-parallel-node-tasks.md#example-scenario) artikli täpsemat sellise stsenaariumi.

### <a name="batch-forum"></a>Paketi Foorum

[Azure'i paketi Foorum] [ forum] MSDN-is on hea koht arutada paketi ja teenuse kohta küsimusi. Pea kohta üle kasulikku "sticky" postitused, ning postitada oma küsimusi, kui need tekivad ajal saate luua oma paketi lahendusi.


[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_listjobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[batch_metrics]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchMetrics
[efficient_query_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/EfficientListQueries
[forum]: https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=azurebatch
[github_samples]: https://github.com/Azure/azure-batch-samples
[odata]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.aspx
[odata_ctor]: https://msdn.microsoft.com/library/azure/dn866178.aspx
[odata_expand]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.expandclause.aspx
[odata_filter]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.filterclause.aspx
[odata_select]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.selectclause.aspx

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

[rest_list_certs]: https://msdn.microsoft.com/library/azure/dn820154.aspx
[rest_list_compute_nodes]: https://msdn.microsoft.com/library/azure/dn820159.aspx
[rest_list_job_schedules]: https://msdn.microsoft.com/library/azure/mt282174.aspx
[rest_list_jobprep_status]: https://msdn.microsoft.com/library/azure/mt282170.aspx
[rest_list_jobs]: https://msdn.microsoft.com/library/azure/dn820117.aspx
[rest_list_nodefiles]: https://msdn.microsoft.com/library/azure/dn820151.aspx
[rest_list_pools]: https://msdn.microsoft.com/library/azure/dn820101.aspx
[rest_list_schedule_jobs]: https://msdn.microsoft.com/library/azure/mt282169.aspx
[rest_list_task_files]: https://msdn.microsoft.com/library/azure/dn820142.aspx
[rest_list_tasks]: https://msdn.microsoft.com/library/azure/dn820187.aspx

[rest_get_cert]: https://msdn.microsoft.com/library/azure/dn820176.aspx
[rest_get_job]: https://msdn.microsoft.com/library/azure/dn820106.aspx
[rest_get_node]: https://msdn.microsoft.com/library/azure/dn820168.aspx
[rest_get_pool]: https://msdn.microsoft.com/library/azure/dn820165.aspx
[rest_get_schedule]: https://msdn.microsoft.com/library/azure/mt282171.aspx
[rest_get_task]: https://msdn.microsoft.com/library/azure/dn820133.aspx

[net_cert]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.certificate.aspx
[net_job]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_node]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.aspx
[net_pool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_schedule]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjobschedule.aspx
[net_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
