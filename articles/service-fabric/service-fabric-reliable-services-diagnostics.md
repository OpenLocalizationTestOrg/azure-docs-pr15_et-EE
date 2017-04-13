<properties
   pageTitle="Stateful usaldusväärsed teenused diagnostika | Microsoft Azure'i"
   description="Diagnostika-funktsiooni Stateful usaldusväärsed teenused"
   services="service-fabric"
   documentationCenter=".net"
   authors="AlanWarwick"
   manager="timlt"
   editor=""/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="05/17/2016"
   ms.author="alanwar"/>

# <a name="diagnostic-functionality-for-stateful-reliable-services"></a>Diagnostika-funktsiooni Stateful usaldusväärsed teenused
Klassi Stateful usaldusväärne teenuste StatefulServiceBase eraldab [EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) sündmusi, mida saab kasutada teenust silumine, anda teadmisi, kuidas runtime töötab ja tõrkeotsingu.

## <a name="eventsource-events"></a>EventSource sündmused
Klassi Stateful usaldusväärne teenuste StatefulServiceBase EventSource nimi on "Microsoft-ServiceFabric-Services". Sündmused selle sündmuse allika kuvatakse aknas [Diagnostika sündmusi](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md#view-service-fabric-system-events-in-visual-studio) , kui teenus on [silumisel Visual Studio](service-fabric-debugging-your-application.md).

Tööriistad ja tehnoloogiad, mis aitavad koguda ja/või EventSource sündmuste vaatamine on [PerfView](http://www.microsoft.com/download/details.aspx?id=28567), [Microsoft Azure'i diagnostika](../cloud-services/cloud-services-dotnet-diagnostics.md)- ja [Microsoft TraceEvent teek](http://www.nuget.org/packages/Microsoft.Diagnostics.Tracing.TraceEvent).

## <a name="events"></a>Sündmused

|Sündmuse nimi|Sündmuse ID|Tase|Sündmuse kirjeldus|
|----------|--------|-----|-----------------|
|StatefulRunAsyncInvocation|1|Teatised|Kiiratav teenuse RunAsync tööülesande käivitamisel|
|StatefulRunAsyncCancellation|2|Teatised|Eraldunud, kui teenus RunAsync tööülesanne on tühistatud.|
|StatefulRunAsyncCompletion|3|Teatised|Kui teenus RunAsync tööülesanne on lõpule viidud kiiratav|
|StatefulRunAsyncSlowCancellation|4|Hoiatus|Kiiratav kui teenus RunAsync tööülesande võtab liiga kaua aega tühistamine|
|StatefulRunAsyncFailure|5|Tõrge|Eraldunud, kui teenus RunAsync tööülesande põhjustab erandi|

## <a name="interpret-events"></a>Sündmuste tõlgendamine

StatefulRunAsyncInvocation, StatefulRunAsyncCompletion ja StatefulRunAsyncCancellation sündmused on kasulik teenuse kirjutaja teenuse--samuti kui teenus on alustamine, tühistatud või lõpetatud ajastuse elutsükli mõista. See võib olla kasulik, kui silumine teenuseprobleemid või teenuse elutsükli mõistmine.

Teenuse poolt peaks tähelepanu Sule StatefulRunAsyncSlowCancellation ja StatefulRunAsyncFailure sündmused Kuna andmeloendite teenuse probleeme.

StatefulRunAsyncFailure on tekitatava iga kord, kui teenus RunAsync() ülesanne põhjustab erandi. Tavaliselt on erand näitab viga või teenuse viga. Lisaks erand põhjustab teenuse nurjumise, nii teisaldatakse see erineva sõlme. See võib olla kallis toimingu ja viivitada sissetulevad taotlused ajal teenus on teisaldatud. Teenuse poolt peaks põhjuse erandi ja võimalusel see leevendada.

StatefulRunAsyncSlowCancellation on tekitatava iga kord, kui RunAsync tööülesande taotluse tühistamise kestab kauem kui neli sekundit. Kui teenus võtab liiga kaua aega tühistamine, see mõjutab võimaluse teenuse teise sõlme kiiresti taaskäivitada. Teenuse üldine kättesaadavus võib mõjutada.
