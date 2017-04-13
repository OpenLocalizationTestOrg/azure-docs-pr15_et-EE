<properties
   pageTitle="Täpsemad usaldusväärne teenuste kasutamist | Microsoft Azure'i"
   description="Lugege täpsemalt teenuse struktuuri usaldusväärsed teenused paindlikkuse teenuste kasutamist."
   services="Service-Fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor="masnider"/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/19/2016"
   ms.author="vturecek"/>

# <a name="advanced-usage-of-the-reliable-services-programming-model"></a>Täpsemad programmeerimisega mudeli usaldusväärne teenuste kasutamist
Azure teenuse struktuuri lihtsustab kirjutamine ja usaldusväärne kodakondsuseta ja stateful teenuste haldamine. Sellest juhendist rääkida täpsemalt tavadega usaldusväärsed teenused teenuste üle rohkem kontrolli ja paindlikkust juurde. Enne selle juhendit lugeda, tutvuda [Programmeerimisega mudeli usaldusväärne teenuseid](service-fabric-reliable-services-introduction.md).

Stateful-ja kodakondsuseta on esmane viisidest kasutaja koodi.

 - `RunAsync`on üldotstarbeline sisendpunkti jaoks teie teenuse kood.
 - `CreateServiceReplicaListeners`ja `CreateServiceInstanceListeners` side kuulajatele kliendi taotluste avada.
 
Enamik teenuste, nende kahe kirje punktid on piisavalt. Harva kui rohkem kontrolli teenuse elutsükkel on nõutav, täiendavad elutsükli sündmused on saadaval.

## <a name="stateless-service-instance-lifecycle"></a>Kodakondsuseta teenuse eksemplari elutsükkel

Kodakondsuseta teenuse elutsükkel on väga lihtne. Kodakondsuseta teenuse saab avada, suletud, või katkestatud. `RunAsync`kodakondsuseta teenuses käivitatakse kui teenuse eksemplari on avatud ja kui teenuse eksemplari suletakse või katkestatud tühistatud. 

Kuigi `RunAsync` peaks olema piisav peaaegu kõigil juhtudel, avamine, sulgemine ja katkestamist sündmuste kodakondsuseta teenuses on saadaval ka:

- `Task OnOpenAsync(IStatelessServicePartition, CancellationToken)`
  OnOpenAsync nimetatakse kui kodakondsuseta eksemplari on kasutada. Laiendatud teenuste lähtestamine tööülesannete sel ajal käivitada.

- `Task OnCloseAsync(CancellationToken)`
  OnCloseAsync nimetatakse kodakondsuseta eksemplari minnes nõtkelt sulgeda. See võib juhtuda, et selle teenuse kood versioonitäiendusel, teisaldatakse eksemplari tõttu koormusetasakaalustuseks või siirdamiseks viga avastatakse. OnCloseAsync saab kasutada turvaline sulgeda ressursse, mis tahes tausta töötlemise lõpetamine, valmis salvestamise välise riigi või sulgeda olemasolevad ühendused.

- `void OnAbort()`
  OnAbort nimetatakse kui kodakondsuseta eksemplari suletakse jõuga. Seda nimetatakse üldiselt püsivate tõrgete tuvastamisel sõlme või teenuse struktuuri tingimata ei saa hallata teenuse eksemplari elutsükli tõttu sisemine.

## <a name="stateful-service-replica-lifecycle"></a>Stateful teenuse koopia elutsükkel

Stateful teenuse koopia elutsükkel on märksa keerukam kui kodakondsuseta teenuse eksemplari. Lisaks avamine, sulgemine ja sündmuste katkestada, stateful teenuse koopia läbib rolli muudatused kestuse ajal. Koopia stateful teenuse muutumisel roll, on `OnChangeRoleAsync` sündmuse käivitamisel:

- `Task OnChangeRoleAsync(ReplicaRole, CancellationToken)`
  OnChangeRoleAsync nimetatakse muutmisel stateful teenuse koopia on rolli, näiteks esmane või teisene. Esmane koopiad on esitatud kirjutamine olek (on lubatud loomine ja kirjutada usaldusväärne saidikogumid). Teisene koopiad on esitatud loetuks olek (ainult saate lugeda olemasoleva usaldusväärne saidikogumid kaudu). Enamik töö stateful teenuses on läbi peamine koopia. Teisene koopiad saab teha kirjutuskaitstud kinnitamine, aruande genereerimine, andmete hankimiseks või muud kirjutuskaitstud tööd.

Stateful teenusega ainult peamine koopia Kirjutuspääsuga olekusse ja seega on üldiselt, millal teenus on läbimiseks tegelikku tööd. Funktsiooni `RunAsync` meetod stateful teenuses käivitatakse ainult siis, kui stateful teenuse koopia on esmane. Funktsiooni `RunAsync` meetod on tühistatud peamine koopia rolli muutumisel primaarne eemale, samuti Sule ja katkestamist sündmuste ajal. 

Abil soovitud `OnChangeRoleAsync` sündmuse võimaldab teil teha sõltuvalt koopia rollirühma rolli muutmise töö.

Stateful teenus pakub sama nelja elutsükli sündmuste kodakondsuseta teenus, sama semantikat ja kasutamise juhtudel:

- `Task OnOpenAsync(IStatefulServicePartition, CancellationToken)`
- `Task OnCloseAsync(CancellationToken)`
- `void OnAbort()`



## <a name="next-steps"></a>Järgmised sammud
Täpsemate teemad, mis on seotud teenuse struktuuri, lugege järgmisi artikleid:

- [Stateful usaldusväärne teenuste konfigureerimine](service-fabric-reliable-services-configuration.md)

- [Teenuse struktuuri seisund tutvustus](service-fabric-health-introduction.md)

- [Tõrkeotsingu süsteemi seisund aruannete kasutamine](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)

- [Services konfigureerimise abil teenuse struktuuri kobar ressursihaldur](service-fabric-cluster-resource-manager-configure-services.md)
