<properties
   pageTitle="Osalejate diagnostika- ja jälgimine | Microsoft Azure'i"
   description="Selles artiklis kirjeldatakse diagnostika- ja jõudluse kontrollimise teenuse struktuuri usaldusväärne osalejate pikkus, sh sündmused ja jõudluse hinnale kiiratava selle funktsioonid."
   services="service-fabric"
   documentationCenter=".net"
   authors="abhishekram"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="07/05/2016"
   ms.author="abhisram"/>

# <a name="diagnostics-and-performance-monitoring-for-reliable-actors"></a>Diagnostika- ja jõudluse usaldusväärne osalejate jälgimine
Usaldusväärne osalejate käitusaja eraldab [EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) sündmused ja [jõudluse hinnale](https://msdn.microsoft.com/library/system.diagnostics.performancecounter.aspx). Need anda teadmisi, kuidas töötab runtime ja abi tõrkeotsingu ja jõudluse jälgimine.

## <a name="eventsource-events"></a>EventSource sündmused
Usaldusväärne osalejate käitusaja EventSource pakkuja nimi on "Microsoft-ServiceFabric-osalejate". Sündmuste selle sündmuse allika sellisena [Diagnostika sündmuste](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md#view-service-fabric-system-events-in-visual-studio) aknas näitleja rakendus on [silumisel Visual Studios](service-fabric-debugging-your-application.md).

Tööriistad ja tehnoloogiad, mis aitavad koguda ja/või EventSource sündmuste vaatamine on [PerfView](http://www.microsoft.com/download/details.aspx?id=28567), [Azure diagnostika](../cloud-services/cloud-services-dotnet-diagnostics.md), [Semantilise logimine](https://msdn.microsoft.com/library/dn774980.aspx)ja [Microsoft TraceEvent teek](http://www.nuget.org/packages/Microsoft.Diagnostics.Tracing.TraceEvent).

### <a name="keywords"></a>Märksõnad
Kõik sündmused, mis kuuluvad usaldusväärne osalejate EventSource on seotud üks või mitu märksõna. See võimaldab filtreerimine sündmused, mis on kogunud. Järgmised märksõna bittide on määratletud.

|Bit|Kirjeldus|
|---|---|
|0x1|Olulised sündmused, mis summarize toimimist struktuuri osalejate käitusaja kogum.|
|0X2|Sündmused, mis kirjeldavad näitleja meetodi kutsed kogum. Lisateavet leiate teemast [sissejuhatavat teemat osalejatele](service-fabric-reliable-actors-introduction.md#actors).|
|0x4|Seotud näitleja olekus sündmuste kogum. Lisateabe saamiseks lugege teemat [näitleja oleku haldamise](service-fabric-reliable-actors-state-management.md)kohta.|
|0x8|Seotud omakorda põhinev kokkulangevus rakenduses osaleja sündmuste kogum. Lisateabe saamiseks lugege teemat [kokkulangevus](service-fabric-reliable-actors-introduction.md#concurrency)kohta.|

## <a name="performance-counters"></a>Jõudluse hinnale
Usaldusväärne osalejate käitusaja määratleb järgmised jõudluse counter kategooriad.

|Kategooria|Kirjeldus|
|---|---|
|Teenuse struktuuri näitleja|Letid teatud Azure teenuse struktuuri osalejad, nt kellaaeg näitleja olekus salvestamiseks tehtud|
|Teenuse struktuuri näitleja meetod|Hinnale kindla teenuse struktuuri osalejate meetodid, nt kui tihti sisestusmeetod näitleja kutsumisel|

Iga ülaltoodud kategooriad on üks või mitu hinnale.

[Windowsi jõudlus kuvari](https://technet.microsoft.com/library/cc749249.aspx) rakendus, mis on saadaval operatsioonisüsteemis Windows vaikimisi saab koguda ja jõudluse andmete kuvamiseks. [Azure'i diagnostika](../cloud-services/cloud-services-dotnet-diagnostics.md) on mõni muu suvand jõudluse andmete kogumise ja üleslaadimine Azure tabelid.

### <a name="performance-counter-instance-names"></a>Jõudluse counter näiteks nimed
Klaster, mis on suur hulk näitleja teenuseid või näitleja teenuse sektsiooni on suure hulga näitleja jõudluse counter eksemplarid. Jõudluse counter näiteks nimed aitab tuvastada kindla [sektsiooni](service-fabric-reliable-actors-platform.md#service-fabric-partition-concepts-for-actors) ja näitleja meetod (vajadusel) jõudluse counter eksemplari seotud.

#### <a name="service-fabric-actor-category"></a>Teenuse struktuuri näitleja kategooria
Kategooria `Service Fabric Actor`, näiteks counter nimed on järgmises vormingus:

`ServiceFabricPartitionID_ActorsRuntimeInternalID`

*ServiceFabricPartitionID* on stringi esindus teenuse struktuuri sektsiooni ID-d on seotud jõudlusega counter eksemplari. Sektsiooni ID on GUID ja selle stringi esitus on loodud kaudu soovitud [`Guid.ToString`](https://msdn.microsoft.com/library/97af8hh4.aspx) meetodi vorming tähistaja "D".

*ActorRuntimeInternalID* on stringi esindus 64-bitise täisarvulise, mis on loodud struktuuri osalejate käitusaja selle kasutamiseks. See on kaasatud jõudluse counter eksemplari nimi oma unikaalsuse tagamiseks ja vastuollu muude jõudluse counter näiteks nimed. Kasutajad ei tohiks proovida tõlgendada selle osa jõudluse counter eksemplari nimi.

Järgmine on näide vastu, mis kuulub counter eksemplari nimi on `Service Fabric Actor` kategooria:

`2740af29-78aa-44bc-a20b-7e60fb783264_635650083799324046`

Eeltoodud näites `2740af29-78aa-44bc-a20b-7e60fb783264` stringi esitus teenuse struktuuri sektsiooni ID ja `635650083799324046` luuakse runtime's sisemise 64-bitine ID-d kasutada.

#### <a name="service-fabric-actor-method-category"></a>Teenuse struktuuri näitleja meetod kategooria
Kategooria `Service Fabric Actor Method`, näiteks counter nimed on järgmises vormingus:

`MethodName_ActorsRuntimeMethodId_ServiceFabricPartitionID_ActorsRuntimeInternalID`

*MethodName* on näitleja meetod, mis on seostatud jõudluse counter eksemplari nimi. Mõned loogika struktuuri osalejate pikkus, mis nimi ja Windowsi jõudlus counter näiteks nimed pikkust piiranguid loetavuse nõuded määratakse meetodi nime vorming.

*ActorsRuntimeMethodId* on stringi esindus 32-bitise täisarvulise, mis on loodud struktuuri osalejate käitusaja selle kasutamiseks. See on kaasatud jõudluse counter eksemplari nimi oma unikaalsuse tagamiseks ja vastuollu muude jõudluse counter näiteks nimed. Kasutajad ei tohiks proovida tõlgendada selle osa jõudluse counter eksemplari nimi.

*ServiceFabricPartitionID* on stringi esindus teenuse struktuuri sektsiooni ID-d on seotud jõudlusega counter eksemplari. Sektsiooni ID on GUID ja selle stringi esitus on loodud kaudu soovitud [`Guid.ToString`](https://msdn.microsoft.com/library/97af8hh4.aspx) meetodi vorming tähistaja "D".

*ActorRuntimeInternalID* on stringi esindus 64-bitise täisarvulise, mis on loodud struktuuri osalejate käitusaja selle kasutamiseks. See on kaasatud jõudluse counter eksemplari nimi oma unikaalsuse tagamiseks ja vastuollu muude jõudluse counter näiteks nimed. Kasutajad ei tohiks proovida tõlgendada selle osa jõudluse counter eksemplari nimi.

Järgmine on näide vastu, mis kuulub counter eksemplari nimi on `Service Fabric Actor Method` kategooria:

`ivoicemailboxactor.leavemessageasync_2_89383d32-e57e-4a9b-a6ad-57c6792aa521_635650083804480486`

Eeltoodud näites `ivoicemailboxactor.leavemessageasync` meetodi nime, on `2` on 32-bitine ID, mis on loodud selle käitusaja sisemiseks kasutamiseks `89383d32-e57e-4a9b-a6ad-57c6792aa521` stringi esitus teenuse struktuuri sektsiooni ID ja `635650083804480486` on 64-bitine ID, mis on loodud runtime's sisemise kasutada.

## <a name="list-of-events-and-performance-counters"></a>Sündmuste ja jõudluse hinnale loend

### <a name="actor-method-events-and-performance-counters"></a>Näitleja meetod sündmuste ja jõudluse hinnale
Usaldusväärne osalejate käitusaja eraldab [näitleja meetodite](service-fabric-reliable-actors-introduction.md#actors)seotud järgmised sündmused.

|Sündmuse nimi|Sündmuse ID|Tase|Võtmesõna|Kirjeldus|
|---|---|---|---|---|
|ActorMethodStart|7|Paljusõnaline|0X2|Osalejate käitusaja on autonoomsest näitleja sisestusmeetod.|
|ActorMethodStop|8|Paljusõnaline|0X2|Näitleja sisestusmeetod on lõpule jõudnud, käivitamisel. On käitusaja asünkroonne kõne näitleja meetodil on tagasi ja tööülesande tagastatud näitleja meetod on lõpule jõudnud.|
|ActorMethodThrewException|9|Hoiatus|0x3|Näitleja meetodil on käitusaja asünkroonne kõne ajal või tööülesande täitmise ajal tagastatud näitleja meetodit näitleja sisestusmeetod täitmise ajal ilmnes erand. See sündmus näitab mingi ebaõnnestumise näitleja koodi, mida uurimine.|

Usaldusväärne osalejate käitusaja avaldab järgmised jõudluse hinnale näitleja meetodite täitmist.

|Kategooria nimi|Counter nimi|Kirjeldus|
|---|---|---|
|Teenuse struktuuri näitleja meetod|Manamine sekundis|Mitu korda näitleja teenuse meetod on vaja järgida sekundis|
|Teenuse struktuuri näitleja meetod|Keskmine millisekundit kutsumise kohta.|Kuluv aeg millisekundites näitleja teenuse meetod käivitada|
|Teenuse struktuuri näitleja meetod|Erandid visati sekundis|Mitu korda, et osaleja teenuse meetod viskasin erandi sekundis|

### <a name="concurrency-events-and-performance-counters"></a>Kokkulangevus sündmuste ja jõudluse hinnale
Usaldusväärne osalejate käitusaja eraldab [kokkulangevus](service-fabric-reliable-actors-introduction.md#concurrency)seotud järgmised sündmused.

|Sündmuse nimi|Sündmuse ID|Tase|Võtmesõna|Kirjeldus|
|---|---|---|---|---|
|ActorMethodCallsWaitingForLock|12|Paljusõnaline|0x8|Sel juhul on kirjutatud iga uue pöörde osalejale alguses. See sisaldab näitleja kõned omandada Lukusta näitleja kohta, mis jõustab omakorda põhinev kokkulangevus ootel arv.|

Usaldusväärne osalejate käitusaja avaldab kokkulangevus seotud järgmised jõudluseloendurid.

|Kategooria nimi|Counter nimi|Kirjeldus|
|---|---|---|
|Teenuse struktuuri näitleja|# näitleja kõnede ootamine näitleja lukustamine|Arvu ootel näitleja kõned hankimise kohta näitleja lukustamine, mis jõustab omakorda põhinev kokkulangevus ootamine|
|Teenuse struktuuri näitleja|Oodake Keskmine millisekundit kohta lukustamine|Kuluv aeg (millisekundites) kohta näitleja lukustamine, mis jõustab omakorda põhinev kokkulangevus hankimine|
|Teenuse struktuuri näitleja|Keskmine millisekundit näitleja Lukusta hoitakse|Aeg (millisekundites), mille kohta näitleja lukustamine toimub|

### <a name="actor-state-management-events-and-performance-counters"></a>Näitleja olekus halduse sündmused ja jõudluse hinnale
Usaldusväärne osalejate käitusaja eraldab [näitleja olekus haldamisega](service-fabric-reliable-actors-state-management.md)seotud järgmised sündmused.

|Sündmuse nimi|Sündmuse ID|Tase|Võtmesõna|Kirjeldus|
|---|---|---|---|---|
|ActorSaveStateStart|10|Paljusõnaline|0x4|Osalejate käitusaja on salvestamine näitleja olek.|
|ActorSaveStateStop|11|Paljusõnaline|0x4|Osalejate käitusaja on näitleja olekus salvestamise lõpetanud.|

Usaldusväärne osalejate käitusaja avaldab järgmised jõudluse hinnale näitleja olekus haldamisega seotud.

|Kategooria nimi|Counter nimi|Kirjeldus|
|---|---|---|
|Teenuse struktuuri näitleja|Riigi Salvestustoiming Keskmine millisekundit kohta.|Kuluv aeg millisekundites näitleja oleku salvestamine|
|Teenuse struktuuri näitleja|Laadi olekus toimingus Keskmine millisekundit|Kuluv aeg millisekundites näitleja olekus laadimine|

### <a name="events-related-to-actor-replicas"></a>Näitleja koopiad seotud sündmused
Usaldusväärne osalejate käitusaja eraldab järgmised sündmused, mis on seotud [näitleja koopiad](service-fabric-reliable-actors-platform.md#service-fabric-partition-concepts-for-stateful-actors).

|Sündmuse nimi|Sündmuse ID|Tase|Võtmesõna|Kirjeldus|
|---|---|---|---|---|
|ReplicaChangeRoleToPrimary|1|Teatised|0x1|Näitleja koopia muuta esmane roll. See tähendab, et osalejad selle sektsiooni luuakse selle koopia.|
|ReplicaChangeRoleFromPrimary|2|Teatised|0x1|Näitleja koopia muuta-esmane roll. See tähendab, et osalejad selle sektsiooni luuakse enam selle koopia. Osalejad, mis on juba loodud selle koopia toimetatakse pole uutele päringutele. Osalejate need kustutatakse pärast pooleli taotlused on lõpule viidud.|

### <a name="actor-activation-and-deactivation-events-and-performance-counters"></a>Näitleja sisse- ja väljalülitamine sündmuste ja jõudluse hinnale
Usaldusväärne osalejate käitusaja eraldab järgmised sündmused, mis on seotud [näitleja sisse- ja väljalülitamine](service-fabric-reliable-actors-lifecycle.md).

|Sündmuse nimi|Sündmuse ID|Tase|Võtmesõna|Kirjeldus|
|---|---|---|---|---|
|ActorActivated|5|Teatised|0x1|Osalejale on juba aktiveeritud.|
|ActorDeactivated|6|Teatised|0x1|Osalejale on desaktiveeritud.|

Usaldusväärne osalejate käitusaja avaldab järgmised jõudluse hinnale seotud näitleja sisse- ja väljalülitamine.

|Kategooria nimi|Counter nimi|Kirjeldus|
|---|---|---|
|Teenuse struktuuri näitleja|Keskmine OnActivateAsync millisekundit.|Kuluv aeg millisekundites OnActivateAsync meetod käivitada|

### <a name="actor-request-processing-performance-counters"></a>Näitleja koosolekukutse töötlemine jõudluse hinnale
Kui mõnda muud klienti tugineb meetodi objekti näitleja puhverserveri kaudu, selle tulemuseks näitleja teenuse võrgu kaudu saadetakse koosolekukutse sõnumi. Teenuse töötleb sõnumi taotlus ja saadab vastuse kliendile. Usaldusväärne osalejate käitusaja avaldab järgmised jõudluse hinnale seotud näitleja koosolekukutse töötlemine.

|Kategooria nimi|Counter nimi|Kirjeldus|
|---|---|---|
|Teenuse struktuuri näitleja|# tasumata taotluste|Teenus töödeldavate taotluste arv|
|Teenuse struktuuri näitleja|Keskmine millisekundit taotluse kohta.|Kuluv aeg (millisekundites), millal teenus taotlus|
|Teenuse struktuuri näitleja|Keskmine millisekundit taotluse vahemäluasukohaga|Kuluv aeg (millisekundites) deserialiseerimiseks näitleja taotluse sõnumi saabumisel teenust|
|Teenuse struktuuri näitleja|Keskmine millisekundit vastuse sariväljaanne|Kuluv aeg (millisekundites) serialiseerida näitleja vastuse sõnumi teenust enne vastuse saatmist kliendile|

## <a name="next-steps"></a>Järgmised sammud
 - [Kuidas kasutada usaldusväärne osalejate teenuse struktuuri platvormi](service-fabric-reliable-actors-platform.md)
 - [Näitleja API dokumentides](https://msdn.microsoft.com/library/azure/dn971626.aspx)
 - [Proovi kood](https://github.com/Azure/servicefabric-samples)
