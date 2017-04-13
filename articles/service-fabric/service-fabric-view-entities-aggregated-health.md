<properties
   pageTitle="Azure teenuse struktuuri üksuste kuvamiseks kokkuvõtliku seisund | Microsoft Azure'i"
   description="Kirjeldab, kuidas päringu, vaadata ja hinnata Azure teenuse struktuuri üksuste liidetud seisund seisundi ja üldine päringute kaudu."
   services="service-fabric"
   documentationCenter=".net"
   authors="oanapl"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/28/2016"
   ms.author="oanapl"/>

# <a name="view-service-fabric-health-reports"></a>Teenuse struktuuri Seisundiaruannete kuvamine
Azure teenuse struktuuri tutvustab [seisund mudelit](service-fabric-health-introduction.md) , mis koosneb seisund üksusest, mida saate komponendid ja valvekoerte aruande kohalikke tingimusi, mida nad jälgivad. [Seisund talletada](service-fabric-health-introduction.md#health-store) liitmise seisund kõik andmed, kas üksused on terve.

Välja kasti, lisatakse klaster Seisundiaruannete saadetud süsteemi komponendid. Lugege lisateavet veebisaidil [süsteemi seisund aruannete abil saate tõrkeotsing](service-fabric-understand-and-troubleshoot-with-system-health-reports.md).

Teenuse struktuuri pakub liidetud seisundi üksuste mitu võimalust:

- [Teenuse struktuuri Explorer](service-fabric-visualizing-your-cluster.md) või muude visualiseeringu tööriistad

- Seisund päringuid (kaudu PowerShelli, API või REST)

- Üldine päringute mis saatja loendi üksused, mis on üks atribuudid (kaudu PowerShelli, API või REST) seisund

Need suvandid näitamaks kasutame kohaliku kobar koos viis sõlmed. Kõrval olevat **struktuuri: / süsteemi** (mis on olemas välja kasti) rakenduse mõni muu rakendus on juurutatud. Üks rakendus on **struktuuri: / WordCount**. See rakendus sisaldab stateful teenus, mis on konfigureeritud seitse koopiad. Kuna seal on ainult viis sõlmed, süsteemi kuvatakse hoiatus, et sektsioon on target count all.

```xml
<Service Name="WordCountService">
    <StatefulService ServiceTypeName="WordCountServiceType" TargetReplicaSetSize="7" MinReplicaSetSize="2">
      <UniformInt64Partition PartitionCount="1" LowKey="1" HighKey="26" />
    </StatefulService>
</Service>
```

## <a name="health-in-service-fabric-explorer"></a>Seisund teenuse struktuuri Exploreris
Teenuse struktuuri Explorer annab klaster visuaalse ülevaate. Alloleval pildil näete mis:

- Rakenduse **struktuuri: / WordCount** on punane (jaotises tõrge), sest see on esitatud **MyWatchdog** atribuudi **kättesaadavus**tõrge sündmus.

- Ühe oma teenuste **struktuuri: / WordCount/WordCountService** kollane (jaotises hoiatus). Kuna teenus on konfigureeritud seitse koopiad, nad ei saa kõik panna, kui on ainult viis sõlmed. Kuigi see on näidatud siin, pole teenuse sektsiooni tõttu süsteemi aruanne kollane. Kollane partition käivitab kollane teenus.

- Klaster on punane punane rakenduse tõttu.

Väärtustamise kasutab vaikepoliitikaid kobar manifesti ja Rakendusmanifest. Need on range poliitika ja ei luba jätmine.

Vaate kobar teenuse struktuuri Exploreris:

![Vaate kobar teenuse struktuuri Exploreriga.][1]

[1]: ./media/service-fabric-view-entities-aggregated-health/servicefabric-explorer-cluster-health.png


> [AZURE.NOTE] Lisateavet [Teenuse struktuuri Explorer](service-fabric-visualizing-your-cluster.md).

## <a name="health-queries"></a>Seisund päringud
Teenuse struktuuri seab seisund päringute iga [üksuse tüüpi](service-fabric-health-introduction.md#health-entities-and-hierarchy)toetatud. Need pääseb API (meetodite leiate **FabricClient.HealthManager**), PowerShelli cmdlet-käskude ja ülejäänud. Need päringud tagastavad täielik tervise teavet üksuse: liidetud seisundioleku, üksuse seisundi sündmusi, lapse seisund Ühendriigid (kui see on rakendatav) ja vigane hinnangud, kui üksus ei ole terve.

> [AZURE.NOTE] Kui see on täielikult täidetud seisund poes, tagastatakse seisund üksus. Üksuse peab olema aktiivne (ei kustutata) ja on süsteemi aruanne. Selle ema üksuste hierarhia ahelas peab olema süsteemi aruandeid. Kui mõni järgmised tingimused on täidetud, seisund päringute tagastada erandi, mis näitab, miks üksus on tagasi.

Päringute seisund peate läbima üksuse identifikaator, mis sõltub üksuse tüüp. Päringute Aktsepteeri valikuline seisund parameetritega. Pole rakendamist on määratud, kasutatakse [rakendamist](service-fabric-health-introduction.md#health-policies) kobar või rakenduse manifestist hindamiseks. Päringute vastu ka filtrite ainult osaline laste või sündmuste--need, mis järgivad määratud filtrid.

> [AZURE.NOTE] Väljundi filtrid rakendatakse serveripoolse, et sõnumi vastuse suurust on vähendatud. Soovitame, et te väljundi filtrite abil saate piirata tagastatavaid andmeid, mitte filtrite kliendis.

Ettevõtte seisund sisaldab:

- Liidetud seisundioleku üksus. Arvutatud üksus Seisundiaruannete, lapse seisund Ühendriigid (kui see on rakendatav) ja rakendamist seisund pood. Lugege lisateavet [üksuse hindamine](service-fabric-health-introduction.md#entity-health-evaluation).  

- Seisund sündmuste olemi.

- Saidikogumi seisundi riikide kõik üksused, mis võib olla laste laste. Üksuse identifikaatorite ja liidetud seisundioleku sisaldavad seisund olekus. Täielik tervise lapsele, helistamine päringu seisundi lapse üksuse tüüp ja edastama lapse identifikaator.

- Osutage aruannet, mida vallandanud olek üksuse, kui üksus ei ole terve vigane hinnangud.

## <a name="get-cluster-health"></a>Saada kobar seisund
Tagastab kobar üksuse seisundi ja see sisaldab seisund riikide rakenduste ja sõlmed (lapsed klaster). Sisend:

- [Valikulised] Kobar aluseid hindamise sõlmed ja kobar sündmused.

- [Valikulised] Rakenduse seisund poliitika kaart, kasutada alistada rakenduse manifest poliitika rakendamist.

- [Valikulised] Filtrite sündmusi, sõlmed ja rakendused, et määrata, millised kirjed huvi pakuvad peaks ja tulem (näiteks ainult vigu või nii hoiatused ja vead). Kõik sündmused, sõlmed ja rakenduste kasutatakse hindamaks, kui üksus kokkuvõtliku seisund, olenemata filter.

### <a name="api"></a>API-GA
Kobar seisundi saamiseks luua mõne `FabricClient` ja kutsuda oma **HealthManager** [GetClusterHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getclusterhealthasync.aspx) meetod.

Järgmise kõne saab seisundit kobar:

```csharp
ClusterHealth clusterHealth = await fabricClient.HealthManager.GetClusterHealthAsync();
```

Järgmine kood saab seisundit kobar sõlmed ja rakenduste kohandatud kobar seisund poliitika ja filtrite abil. See loob [ClusterHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.clusterhealthquerydescription.aspx), mis sisaldab Sisestuskeel teavet.

```csharp
var policy = new ClusterHealthPolicy()
{
    MaxPercentUnhealthyNodes = 20
};
var nodesFilter = new NodeHealthStatesFilter()
{
    HealthStateFilterValue = HealthStateFilter.Error | HealthStateFilter.Warning
};
var applicationsFilter = new ApplicationHealthStatesFilter()
{
    HealthStateFilterValue = HealthStateFilter.Error
};
var queryDescription = new ClusterHealthQueryDescription()
{
    HealthPolicy = policy,
    ApplicationsFilter = applicationsFilter,
    NodesFilter = nodesFilter,
};

ClusterHealth clusterHealth = await fabricClient.HealthManager.GetClusterHealthAsync(queryDescription);
```

### <a name="powershell"></a>PowerShelli
Cmdlet-käsk saada kobar seisund on [Get-ServiceFabricClusterHealth](https://msdn.microsoft.com/library/mt125850.aspx). Esmalt ühenduse klaster [Ühendamine-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) cmdleti abil.

Klaster on viis sõlmed, süsteemi rakenduse ja struktuuri: / WordCount konfigureeritud, nagu on kirjeldatud.

Järgmine cmdlet-käsk saab seisundit kobar seisund vaikepoliitikaid abil. Liidetud seisundioleku on hoiatus, kuna struktuuri: / WordCount rakendus on hoiatus. Pange tähele, kuidas vigane hinnanguid anda üksikasjad vallandanud liidetud seisundi tingimustele.

```xml
PS C:\> Get-ServiceFabricClusterHealth

AggregatedHealthState   : Warning
UnhealthyEvaluations    :
                          Unhealthy applications: 100% (1/1), MaxPercentUnhealthyApplications=0%.

                          Unhealthy application: ApplicationName='fabric:/WordCount', AggregatedHealthState='Warning'.

                              Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.

                              Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Warning'.

                                  Unhealthy event: SourceId='System.PLB',
                          Property='ServiceReplicaUnplacedHealth_Secondary_a1f83a35-d6bf-4d39-b90d-28d15f39599b', HealthState='Warning',
                          ConsiderWarningAsError=false.


NodeHealthStates        :
                          NodeName              : _Node_2
                          AggregatedHealthState : Ok

                          NodeName              : _Node_0
                          AggregatedHealthState : Ok

                          NodeName              : _Node_1
                          AggregatedHealthState : Ok

                          NodeName              : _Node_3
                          AggregatedHealthState : Ok

                          NodeName              : _Node_4
                          AggregatedHealthState : Ok

ApplicationHealthStates :
                          ApplicationName       : fabric:/System
                          AggregatedHealthState : Ok

                          ApplicationName       : fabric:/WordCount
                          AggregatedHealthState : Warning

HealthEvents            : None
```

Järgmine PowerShelli cmdlet-käsk saab klaster seisundi, kasutades kohandatud rakenduse poliitika. See filtreerib tulemuste saamiseks ainult tõrge või Hoiatus rakenduste ja sõlmed. Seetõttu ei sõlmed on tagastada, kuna need on kõik terved. Ainult struktuuri: / WordCount rakendus peab kinni rakenduste filter. Kuna kohandatud poliitika määrab hoiatused veana struktuuri silmas pidada: / WordCount rakenduse rakenduse hinnatakse nagu tõrge, ja seega on klaster.

```powershell
PS c:\> $appHealthPolicy = New-Object -TypeName System.Fabric.Health.ApplicationHealthPolicy
$appHealthPolicy.ConsiderWarningAsError = $true
$appHealthPolicyMap = New-Object -TypeName System.Fabric.Health.ApplicationHealthPolicyMap
$appUri1 = New-Object -TypeName System.Uri -ArgumentList "fabric:/WordCount"
$appHealthPolicyMap.Add($appUri1, $appHealthPolicy)
Get-ServiceFabricClusterHealth -ApplicationHealthPolicyMap $appHealthPolicyMap -ApplicationsFilter "Warning,Error" -NodesFilter "Warning,Error"


AggregatedHealthState   : Error
UnhealthyEvaluations    :
                          Unhealthy applications: 100% (1/1), MaxPercentUnhealthyApplications=0%.

                          Unhealthy application: ApplicationName='fabric:/WordCount', AggregatedHealthState='Error'.

                              Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.

                              Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Error'.

                                  Unhealthy event: SourceId='System.PLB',
                          Property='ServiceReplicaUnplacedHealth_Secondary_a1f83a35-d6bf-4d39-b90d-28d15f39599b', HealthState='Warning',
                          ConsiderWarningAsError=true.


NodeHealthStates        : None
ApplicationHealthStates :
                          ApplicationName       : fabric:/WordCount
                          AggregatedHealthState : Error

HealthEvents            : None

```

### <a name="rest"></a>ÜLEJÄÄNUD
Saate [Saada taotlus](https://msdn.microsoft.com/library/azure/dn707669.aspx) või [postituse koosolekukutse](https://msdn.microsoft.com/library/azure/dn707696.aspx) kehasse kirjeldatud rakendamist sisaldava kobar seisund.

## <a name="get-node-health"></a>Saada sõlm seisund
Tagastab sõlm üksuse seisundi ja see sisaldab teatatud sõlme seisund sündmused. Sisend:

- [Vaja] Sõlme nimi, mis tuvastab sõlme.

- [Valikulised] Kobar seisund rühmapoliitika sätted hindamise seisund.

- [Valikulised] Filtrite sündmusi, et määrata, millised kirjed huvi pakuvad peaks ja tulem (näiteks ainult vigu või nii hoiatused ja vead). Kõikide sündmuste kasutatakse hindamaks, kui üksus kokkuvõtliku seisund, olenemata filter.

### <a name="api"></a>API-GA
Saada sõlm seisund API kaudu, luua mõne `FabricClient` ja kutsuda oma HealthManager [GetNodeHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getnodehealthasync.aspx) meetod.

Järgmine kood saab sõlm seisundi määratud sõlm nimi:

```csharp
NodeHealth nodeHealth = await fabricClient.HealthManager.GetNodeHealthAsync(nodeName);
```

Järgmine kood saab seisundit sõlm määratud sõlm nimi ja sündmuste filter ja kohandatud Poliitikasätte läbib [NodeHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.nodehealthquerydescription.aspx).

```csharp
var queryDescription = new NodeHealthQueryDescription(nodeName)
{
    HealthPolicy = new ClusterHealthPolicy() {  ConsiderWarningAsError = true },
    EventsFilter = new HealthEventsFilter() { HealthStateFilterValue = HealthStateFilter.Warning },
};

NodeHealth nodeHealth = await fabricClient.HealthManager.GetNodeHealthAsync(queryDescription);
```

### <a name="powershell"></a>PowerShelli
Cmdlet-käsk saada sõlm seisund on [Get-ServiceFabricNodeHealth](https://msdn.microsoft.com/library/mt125937.aspx). Esmalt ühenduse klaster [Ühendamine-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) cmdleti abil.
Järgmine cmdlet-käsk saab seisundit sõlm seisund vaikepoliitikaid abil:

```powershell
PS C:\> Get-ServiceFabricNodeHealth _Node_1


NodeName              : _Node_1
AggregatedHealthState : Ok
HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 6
                        SentAt                : 3/22/2016 7:47:56 PM
                        ReceivedAt            : 3/22/2016 7:48:19 PM
                        TTL                   : Infinite
                        Description           : Fabric node is up.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 3/22/2016 7:48:19 PM, LastWarning = 1/1/0001 12:00:00 AM
```

Järgmine cmdlet-käsk saab seisundit kõik sõlmed klaster:

```powershell
PS C:\> Get-ServiceFabricNode | Get-ServiceFabricNodeHealth | select NodeName, AggregatedHealthState | ft -AutoSize

NodeName AggregatedHealthState
-------- ---------------------
_Node_2                     Ok
_Node_0                     Ok
_Node_1                     Ok
_Node_3                     Ok
_Node_4                     Ok
```

### <a name="rest"></a>ÜLEJÄÄNUD
Saate [Saada taotlus](https://msdn.microsoft.com/library/azure/dn707650.aspx) või [postituse koosolekukutse](https://msdn.microsoft.com/library/azure/dn707665.aspx) kehasse kirjeldatud rakendamist sisaldava sõlm seisund.

## <a name="get-application-health"></a>Saada taotlus seisund
Tagastab seisundi rakenduse üksus. See sisaldab seisund riikide juurutatud rakendusega ja teenuse laste. Sisend:

- [Vaja] Rakenduse nimi (URI) rakenduse tuvastav.

- [Valikulised] Rakenduse aluseid kasutada rakenduste manifest poliitikad alistada.

- [Valikulised] Filtrite sündmuste, teenuste ja juurutatud rakendused, et määrata, millised kirjed huvi pakuvad peaks ja tulem (näiteks ainult vigu või nii hoiatused ja vead). Kõik sündmused, teenuste ja juurutatud rakenduste kasutatakse hindamaks, kui üksus kokkuvõtliku seisund, olenemata filter.

### <a name="api"></a>API-GA
Rakenduse seisundi saamiseks luua mõne `FabricClient` ja kutsuda oma HealthManager [GetApplicationHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getapplicationhealthasync.aspx) meetod.

Järgmine kood antavad rakenduse seisundi määratud rakenduse nimi (URI)

```csharp
ApplicationHealth applicationHealth = await fabricClient.HealthManager.GetApplicationHealthAsync(applicationName);
```

Järgmine kood saab rakenduse seisundi määratud rakenduse nimi (URI), filtrite ja kohandatud poliitikate määratud [ApplicationHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.applicationhealthquerydescription.aspx)kaudu.

```csharp
HealthStateFilter warningAndErrors = HealthStateFilter.Error | HealthStateFilter.Warning;
var serviceTypePolicy = new ServiceTypeHealthPolicy()
{
    MaxPercentUnhealthyPartitionsPerService = 0,
    MaxPercentUnhealthyReplicasPerPartition = 5,
    MaxPercentUnhealthyServices = 0,
};
var policy = new ApplicationHealthPolicy()
{
    ConsiderWarningAsError = false,
    DefaultServiceTypeHealthPolicy = serviceTypePolicy,
    MaxPercentUnhealthyDeployedApplications = 0,
};

var queryDescription = new ApplicationHealthQueryDescription(applicationName)
{
    HealthPolicy = policy,
    EventsFilter = new HealthEventsFilter() { HealthStateFilterValue = warningAndErrors },
    ServicesFilter = new ServiceHealthStatesFilter() { HealthStateFilterValue = warningAndErrors },
    DeployedApplicationsFilter = new DeployedApplicationHealthStatesFilter() { HealthStateFilterValue = warningAndErrors },
};

ApplicationHealth applicationHealth = await fabricClient.HealthManager.GetApplicationHealthAsync(queryDescription);
```

### <a name="powershell"></a>PowerShelli
Cmdlet-käsk Saada taotlus seisund on [Get-ServiceFabricApplicationHealth](https://msdn.microsoft.com/library/mt125976.aspx). Esmalt ühenduse klaster [Ühendamine-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) cmdleti abil.

Järgmine cmdlet-käsk tagastab seisundi soovitud **struktuuri: / WordCount** rakendus:

```powershell
PS c:\>
PS C:\WINDOWS\system32>  Get-ServiceFabricApplicationHealth fabric:/WordCount


ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Warning
UnhealthyEvaluations            :
                                  Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.

                                  Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Warning'.

                                      Unhealthy event: SourceId='System.PLB',
                                  Property='ServiceReplicaUnplacedHealth_Secondary_a1f83a35-d6bf-4d39-b90d-28d15f39599b', HealthState='Warning',
                                  ConsiderWarningAsError=false.

ServiceHealthStates             :
                                  ServiceName           : fabric:/WordCount/WordCountService
                                  AggregatedHealthState : Warning

                                  ServiceName           : fabric:/WordCount/WordCountWebService
                                  AggregatedHealthState : Ok

DeployedApplicationHealthStates :
                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_0
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_2
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_3
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_4
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_1
                                  AggregatedHealthState : Ok

HealthEvents                    :
                                  SourceId              : System.CM
                                  Property              : State
                                  HealthState           : Ok
                                  SequenceNumber        : 360
                                  SentAt                : 3/22/2016 7:56:53 PM
                                  ReceivedAt            : 3/22/2016 7:56:53 PM
                                  TTL                   : Infinite
                                  Description           : Application has been created.
                                  RemoveWhenExpired     : False
                                  IsExpired             : False
                                  Transitions           : Error->Ok = 3/22/2016 7:56:53 PM, LastWarning = 1/1/0001 12:00:00 AM

                                  SourceId              : MyWatchdog
                                  Property              : Availability
                                  HealthState           : Ok
                                  SequenceNumber        : 131031545225930951
                                  SentAt                : 3/22/2016 9:08:42 PM
                                  ReceivedAt            : 3/22/2016 9:08:42 PM
                                  TTL                   : Infinite
                                  Description           : Availability checked successfully, latency ok
                                  RemoveWhenExpired     : False
                                  IsExpired             : False
                                  Transitions           : Error->Ok = 3/22/2016 8:55:39 PM, LastWarning = 1/1/0001 12:00:00 AM
```

Järgmine PowerShelli cmdlet-käsk edastab kohandatud poliitika. See filtreerib laste ja sündmused.

```powershell
PS C:\> Get-ServiceFabricApplicationHealth -ApplicationName fabric:/WordCount -ConsiderWarningAsError $true -ServicesFilter Error -EventsFilter Error -DeployedApplicationsFilter Error

ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Error
UnhealthyEvaluations            :
                                  Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.

                                  Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Error'.

                                      Unhealthy event: SourceId='System.PLB',
                                  Property='ServiceReplicaUnplacedHealth_Secondary_a1f83a35-d6bf-4d39-b90d-28d15f39599b', HealthState='Warning',
                                  ConsiderWarningAsError=true.

ServiceHealthStates             :
                                  ServiceName           : fabric:/WordCount/WordCountService
                                  AggregatedHealthState : Error

DeployedApplicationHealthStates : None
HealthEvents                    : None
```

### <a name="rest"></a>ÜLEJÄÄNUD
Saate rakenduse seisundi [Saada taotlus](https://msdn.microsoft.com/library/azure/dn707681.aspx) või [postituse koosolekukutse](https://msdn.microsoft.com/library/azure/dn707643.aspx) kehasse kirjeldatud rakendamist sisaldava.

## <a name="get-service-health"></a>Saada teenuse seisund
Tagastab üksuse teenuse seisund. See sisaldab sektsiooni seisund olekus. Sisend:

- [Vaja] Teenuse nimi (URI) teenuse tuvastav.

- [Valikulised] Rakenduse aluseid kasutada rakenduse manifest poliitika alistada.

- [Valikulised] Filtrite sündmuste ja sektsioonid, et määrata, millised kirjed huvi pakuvad peaks ja tulem (näiteks ainult vigu või nii hoiatused ja vead). Kõik sündmused ja sektsioonid kasutatakse hindamaks, kui üksus kokkuvõtliku seisund, olenemata filter.

### <a name="api"></a>API-GA
Saada teenuse seisund API kaudu, luua mõne `FabricClient` ja kutsuda oma HealthManager [GetServiceHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getservicehealthasync.aspx) meetod.

Järgmises näites saab määratud teenuse nimega (URI) teenuse seisund:

```charp
ServiceHealth serviceHealth = await fabricClient.HealthManager.GetServiceHealthAsync(serviceName);
```

Järgmine kood saab teenuse seisund määratud teenuse nimi (URI), filtrite ja kohandatud Poliitikasätte [ServiceHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.servicehealthquerydescription.aspx)kaudu:

```csharp
var queryDescription = new ServiceHealthQueryDescription(serviceName)
{
    EventsFilter = new HealthEventsFilter() { HealthStateFilterValue = HealthStateFilter.All },
    PartitionsFilter = new PartitionHealthStatesFilter() { HealthStateFilterValue = HealthStateFilter.Error },
};

ServiceHealth serviceHealth = await fabricClient.HealthManager.GetServiceHealthAsync(queryDescription);
```

### <a name="powershell"></a>PowerShelli
Cmdlet-käsk saada teenuse seisund on [Get-ServiceFabricServiceHealth](https://msdn.microsoft.com/library/mt125984.aspx). Esmalt ühenduse klaster [Ühendamine-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) cmdleti abil.

Järgmine cmdlet-käsk saab seisundit vaikepoliitikaid abil teenuse seisund:

```powershell
PS C:\> Get-ServiceFabricServiceHealth -ServiceName fabric:/WordCount/WordCountService


ServiceName           : fabric:/WordCount/WordCountService
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.PLB',
                        Property='ServiceReplicaUnplacedHealth_Secondary_a1f83a35-d6bf-4d39-b90d-28d15f39599b', HealthState='Warning',
                        ConsiderWarningAsError=false.

PartitionHealthStates :
                        PartitionId           : a1f83a35-d6bf-4d39-b90d-28d15f39599b
                        AggregatedHealthState : Warning

HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 10
                        SentAt                : 3/22/2016 7:56:53 PM
                        ReceivedAt            : 3/22/2016 7:57:18 PM
                        TTL                   : Infinite
                        Description           : Service has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 3/22/2016 7:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM

                        SourceId              : System.PLB
                        Property              : ServiceReplicaUnplacedHealth_Secondary_a1f83a35-d6bf-4d39-b90d-28d15f39599b
                        HealthState           : Warning
                        SequenceNumber        : 131031547693687021
                        SentAt                : 3/22/2016 9:12:49 PM
                        ReceivedAt            : 3/22/2016 9:12:49 PM
                        TTL                   : 00:01:05
                        Description           : The Load Balancer was unable to find a placement for one or more of the Service's Replicas:
                        fabric:/WordCount/WordCountService Secondary Partition a1f83a35-d6bf-4d39-b90d-28d15f39599b could not be placed, possibly,
                        due to the following constraints and properties:  
                        Placement Constraint: N/A
                        Depended Service: N/A

                        Constraint Elimination Sequence:
                        ReplicaExclusionStatic eliminated 4 possible node(s) for placement -- 1/5 node(s) remain.
                        ReplicaExclusionDynamic eliminated 1 possible node(s) for placement -- 0/5 node(s) remain.

                        Nodes Eliminated By Constraints:

                        ReplicaExclusionStatic:
                        FaultDomain:fd:/0 NodeName:_Node_0 NodeType:NodeType0 UpgradeDomain:0 UpgradeDomain: ud:/0 Deactivation Intent/Status:
                        None/None
                        FaultDomain:fd:/1 NodeName:_Node_1 NodeType:NodeType1 UpgradeDomain:1 UpgradeDomain: ud:/1 Deactivation Intent/Status:
                        None/None
                        FaultDomain:fd:/3 NodeName:_Node_3 NodeType:NodeType3 UpgradeDomain:3 UpgradeDomain: ud:/3 Deactivation Intent/Status:
                        None/None
                        FaultDomain:fd:/4 NodeName:_Node_4 NodeType:NodeType4 UpgradeDomain:4 UpgradeDomain: ud:/4 Deactivation Intent/Status:
                        None/None

                        ReplicaExclusionDynamic:
                        FaultDomain:fd:/2 NodeName:_Node_2 NodeType:NodeType2 UpgradeDomain:2 UpgradeDomain: ud:/2 Deactivation Intent/Status:
                        None/None


                        RemoveWhenExpired     : True
                        IsExpired             : False
```

### <a name="rest"></a>ÜLEJÄÄNUD
Saate [hankida taotluse](https://msdn.microsoft.com/library/azure/dn707609.aspx) või [postituse koosolekukutse](https://msdn.microsoft.com/library/azure/dn707646.aspx) kehasse kirjeldatud rakendamist sisaldava teenuse seisund.

## <a name="get-partition-health"></a>Saada partition seisund
Tagastab seisundi partition üksus. See sisaldab koopia seisund olekus. Sisend:

- [Vaja] Sektsiooni ID (GUID), mis määratleb.

- [Valikulised] Rakenduse aluseid kasutada rakenduse manifest poliitika alistada.

- [Valikulised] Filtrite sündmused ja koopiad, et määrata, millised kirjed huvi pakuvad peaks ja tulem (näiteks ainult vigu või nii hoiatused ja vead). Kõik sündmused ja koopiad kasutatakse hindamaks, kui üksus kokkuvõtliku seisund, olenemata filter.

### <a name="api"></a>API-GA
Saada partition seisund API kaudu, luua mõne `FabricClient` ja kutsuda oma HealthManager [GetPartitionHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getpartitionhealthasync.aspx) meetod. Määra valikuliste parameetrite, luua [PartitionHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.partitionhealthquerydescription.aspx).

```csharp
PartitionHealth partitionHealth = await fabricClient.HealthManager.GetPartitionHealthAsync(partitionId);
```

### <a name="powershell"></a>PowerShelli
Cmdlet-käsk saada partition seisund on [Get-ServiceFabricPartitionHealth](https://msdn.microsoft.com/library/mt125869.aspx). Esmalt ühenduse klaster [Ühendamine-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) cmdleti abil.

Järgmine cmdlet-käsk saab seisundi kõik sektsioonid jaoks soovitud **struktuuri: / WordCount/WordCountService** teenuse:

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | Get-ServiceFabricPartitionHealth


PartitionId           : a1f83a35-d6bf-4d39-b90d-28d15f39599b
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=false.

ReplicaHealthStates   :
                        ReplicaId             : 131031502143040223
                        AggregatedHealthState : Ok

                        ReplicaId             : 131031502346844060
                        AggregatedHealthState : Ok

                        ReplicaId             : 131031502346844059
                        AggregatedHealthState : Ok

                        ReplicaId             : 131031502346844061
                        AggregatedHealthState : Ok

                        ReplicaId             : 131031502346844058
                        AggregatedHealthState : Ok

HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Warning
                        SequenceNumber        : 76
                        SentAt                : 3/22/2016 7:57:26 PM
                        ReceivedAt            : 3/22/2016 7:57:48 PM
                        TTL                   : Infinite
                        Description           : Partition is below target replica or instance count.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Warning = 3/22/2016 7:57:48 PM, LastOk = 1/1/0001 12:00:00 AM
```

### <a name="rest"></a>ÜLEJÄÄNUD
Saate [Saada taotlus](https://msdn.microsoft.com/library/azure/dn707683.aspx) või [postituse koosolekukutse](https://msdn.microsoft.com/library/azure/dn707680.aspx) kehasse kirjeldatud rakendamist sisaldava sektsiooni seisund.

## <a name="get-replica-health"></a>Saada koopia seisund
Tagastab seisundi stateful teenuse koopia või kodakondsuseta teenuse eksemplari. Sisend:

- [Vaja] Selle sektsiooni ID (GUID) ja koopia ID, mis tuvastab koopia.

- [Valikulised] Rakenduse seisund poliitika parameetreid kasutatakse rakenduste manifest poliitikad alistada.

- [Valikulised] Filtrite sündmusi, et määrata, millised kirjed huvi pakuvad peaks ja tulem (näiteks ainult vigu või nii hoiatused ja vead). Kõikide sündmuste kasutatakse hindamaks, kui üksus kokkuvõtliku seisund, olenemata filter.

### <a name="api"></a>API-GA
Saada koopia seisundi API kaudu, luua mõne `FabricClient` ja kutsuda oma HealthManager [GetReplicaHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getreplicahealthasync.aspx) meetod. Täpsemad parameetrid määramiseks kasutage [ReplicaHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.replicahealthquerydescription.aspx).

```csharp
ReplicaHealth replicaHealth = await fabricClient.HealthManager.GetReplicaHealthAsync(partitionId, replicaId);
```

### <a name="powershell"></a>PowerShelli
Cmdlet-käsk Saada koopia seisund on [Get-ServiceFabricReplicaHealth](https://msdn.microsoft.com/library/mt125808.aspx). Esmalt ühenduse klaster [Ühendamine-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) cmdleti abil.

Järgmine cmdlet-käsk saab peamine koopia jaoks kõik partitsioonid teenuse seisundi:

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | Get-ServiceFabricReplica | where {$_.ReplicaRole -eq "Primary"} | Get-ServiceFabricReplicaHealth


PartitionId           : a1f83a35-d6bf-4d39-b90d-28d15f39599b
ReplicaId             : 131031502143040223
AggregatedHealthState : Ok
HealthEvents          :
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 131031502145556748
                        SentAt                : 3/22/2016 7:56:54 PM
                        ReceivedAt            : 3/22/2016 7:57:12 PM
                        TTL                   : Infinite
                        Description           : Replica has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 3/22/2016 7:57:12 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="rest"></a>ÜLEJÄÄNUD
Saate koopia seisundi [Saada taotlus](https://msdn.microsoft.com/library/azure/dn707673.aspx) või [postituse koosolekukutse](https://msdn.microsoft.com/library/azure/dn707641.aspx) kehasse kirjeldatud rakendamist sisaldava.

## <a name="get-deployed-application-health"></a>Saada juurutatud rakendusega seisund
Tagastab rakenduse juurutatud sõlm üksuse seisundi. See sisaldab juurutatud teenuse paketi seisund olekus. Sisend:

- [Vaja] Rakenduse nimi (URI) ja sõlme nimi (stringi) juurutatud rakendusega tuvastavat.

- [Valikulised] Rakenduse aluseid kasutada rakenduste manifest poliitikad alistada.

- [Valikulised] Filtrite sündmuste ja määrata, millised kirjed juurutatud pakettide huvi pakuvad peaks ja tulem (näiteks ainult vigu või nii hoiatused ja vead). Kõik sündmused ja juurutatud teenuse pakettide kasutatakse hindamaks, kui üksus kokkuvõtliku seisund, olenemata filter.

### <a name="api"></a>API-GA
Saada seisundi rakenduse juurutatud sõlme API kaudu, luua mõne `FabricClient` ja kutsuda oma HealthManager [GetDeployedApplicationHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getdeployedapplicationhealthasync.aspx) meetod. Valikuliste parameetrite määramiseks kasutage [DeployedApplicationHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.deployedapplicationhealthquerydescription.aspx).

```csharp
DeployedApplicationHealth health = await fabricClient.HealthManager.GetDeployedApplicationHealthAsync(
    new DeployedApplicationHealthQueryDescription(applicationName, nodeName));
```

### <a name="powershell"></a>PowerShelli
Cmdlet-käsk saada juurutatud rakendusega seisund on [Get-ServiceFabricDeployedApplicationHealth](https://msdn.microsoft.com/library/mt163523.aspx). Esmalt ühenduse klaster [Ühendamine-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) cmdleti abil. Et teada saada, kui rakendus on juurutatud, Käivita [Get-ServiceFabricApplicationHealth](https://msdn.microsoft.com/library/mt125976.aspx) ja vaadata juurutatud rakendusega lapsi.

Järgmine cmdlet-käsk saab seisundit, on **struktuuri: / WordCount** rakenduse juurutatud **_Node_2**.

```powershell
PS C:\> Get-ServiceFabricDeployedApplicationHealth -ApplicationName fabric:/WordCount -NodeName _Node_2


ApplicationName                    : fabric:/WordCount
NodeName                           : _Node_2
AggregatedHealthState              : Ok
DeployedServicePackageHealthStates :
                                     ServiceManifestName   : WordCountServicePkg
                                     NodeName              : _Node_2
                                     AggregatedHealthState : Ok

                                     ServiceManifestName   : WordCountWebServicePkg
                                     NodeName              : _Node_2
                                     AggregatedHealthState : Ok

HealthEvents                       :
                                     SourceId              : System.Hosting
                                     Property              : Activation
                                     HealthState           : Ok
                                     SequenceNumber        : 131031502143710698
                                     SentAt                : 3/22/2016 7:56:54 PM
                                     ReceivedAt            : 3/22/2016 7:57:12 PM
                                     TTL                   : Infinite
                                     Description           : The application was activated successfully.
                                     RemoveWhenExpired     : False
                                     IsExpired             : False
                                     Transitions           : Error->Ok = 3/22/2016 7:57:12 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="rest"></a>ÜLEJÄÄNUD
Saate [Saada taotlus](https://msdn.microsoft.com/library/azure/dn707644.aspx) või [postituse koosolekukutse](https://msdn.microsoft.com/library/azure/dn707688.aspx) kehasse kirjeldatud rakendamist sisaldava juurutatud rakendusega seisund.

## <a name="get-deployed-service-package-health"></a>Saada juurutatud teenuse paketi seisund
Tagastab paketi üksuse juurutatud teenuse seisund. Sisend:

- [Vaja] Rakenduse nimi (URI), sõlme nimi (stringi) ja teenuse näidata tuvastamine juurutatud teenuse paketi nimi (stringi).

- [Valikulised] Rakenduse aluseid kasutada rakenduse manifest poliitika alistada.

- [Valikulised] Filtrite sündmusi, et määrata, millised kirjed huvi pakuvad peaks ja tulem (näiteks ainult vigu või nii hoiatused ja vead). Kõikide sündmuste kasutatakse hindamaks, kui üksus kokkuvõtliku seisund, olenemata filter.

### <a name="api"></a>API-GA
Saada paketi juurutatud teenuse seisundi API kaudu, luua mõne `FabricClient` ja kutsuda oma HealthManager [GetDeployedServicePackageHealthAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getdeployedservicepackagehealthasync.aspx) meetod. Valikuliste parameetrite määramiseks kasutage [DeployedServicePackageHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.deployedservicepackagehealthquerydescription.aspx).

```csharp
DeployedServicePackageHealth health = await fabricClient.HealthManager.GetDeployedServicePackageHealthAsync(
    new DeployedServicePackageHealthQueryDescription(applicationName, nodeName, serviceManifestName));
```

### <a name="powershell"></a>PowerShelli
Cmdlet-käsk saada paketi juurutatud teenuse seisundi on [Get-ServiceFabricDeployedServicePackageHealth](https://msdn.microsoft.com/library/mt163525.aspx). Esmalt ühenduse klaster [Ühendamine-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) cmdleti abil. Kui rakendus on juurutatud vaatamiseks käivitada [Get-ServiceFabricApplicationHealth](https://msdn.microsoft.com/library/mt125976.aspx) ja vaadake juurutatud rakendused. Mis on rakenduses teenuse vaatamiseks vaadake juurutatud teenuse paketi laste [Get-ServiceFabricDeployedApplicationHealth](https://msdn.microsoft.com/library/mt163523.aspx) väljund.

Järgmine cmdlet-käsk saab seisundi **WordCountServicePkg** teenuse pakett on **struktuuri: / WordCount** rakenduse juurutatud **_Node_2**. Üksusel **System.Hosting** aruanded eduka pakett sisendpunkti aktiveerimise ja eduka teenuse tüüp registreerimise.

```powershell
PS C:\> Get-ServiceFabricDeployedApplication -ApplicationName fabric:/WordCount -NodeName _Node_2 | Get-ServiceFabricDeployedServicePackageHealth -ServiceManifestName WordCountServicePkg


ApplicationName       : fabric:/WordCount
ServiceManifestName   : WordCountServicePkg
NodeName              : _Node_2
AggregatedHealthState : Ok
HealthEvents          :
                        SourceId              : System.Hosting
                        Property              : Activation
                        HealthState           : Ok
                        SequenceNumber        : 131031502301306211
                        SentAt                : 3/22/2016 7:57:10 PM
                        ReceivedAt            : 3/22/2016 7:57:12 PM
                        TTL                   : Infinite
                        Description           : The ServicePackage was activated successfully.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 3/22/2016 7:57:12 PM, LastWarning = 1/1/0001 12:00:00 AM

                        SourceId              : System.Hosting
                        Property              : CodePackageActivation:Code:EntryPoint
                        HealthState           : Ok
                        SequenceNumber        : 131031502301568982
                        SentAt                : 3/22/2016 7:57:10 PM
                        ReceivedAt            : 3/22/2016 7:57:12 PM
                        TTL                   : Infinite
                        Description           : The CodePackage was activated successfully.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 3/22/2016 7:57:12 PM, LastWarning = 1/1/0001 12:00:00 AM

                        SourceId              : System.Hosting
                        Property              : ServiceTypeRegistration:WordCountServiceType
                        HealthState           : Ok
                        SequenceNumber        : 131031502314788519
                        SentAt                : 3/22/2016 7:57:11 PM
                        ReceivedAt            : 3/22/2016 7:57:12 PM
                        TTL                   : Infinite
                        Description           : The ServiceType was registered successfully.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 3/22/2016 7:57:12 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="rest"></a>ÜLEJÄÄNUD
Saate juurutatud teenuseseisund paketi [Saada taotlus](https://msdn.microsoft.com/library/azure/dn707677.aspx) või [postituse koosolekukutse](https://msdn.microsoft.com/library/azure/dn707689.aspx) kehasse kirjeldatud rakendamist sisaldava.

## <a name="health-chunk-queries"></a>Seisund kogumi päringud
Seisund kogumi päringute võib tagastada Mitmetasemeline kobar laste (rekursiivselt) kohta sisendi filtrid. See toetab Täpsemad filtrid, mis võimaldavad palju paindlikkust väljendada tagastatakse, millised teatud laste tähistatud oma kordumatut tunnust või muude rühma identifikaator ja/või seisundioleku. Vaikimisi on lapsi pole kaasatud, seisund käsud, mis sisaldavad alati esimese taseme laste erinevalt.

[Seisund päringute](service-fabric-view-entities-aggregated-health.md#health-queries) tagastamiseks ainult esimese taseme laste määratud üksuse kohta nõutav filtrid. Laste laste saamiseks tuleb helistada iga üksuse kohta huvi tervise API-d. Samuti kindlate üksuste seisundi saamiseks tuleb ühe seisund API kõne iga soovitud üksuse jaoks. Täpsem filtreerimine kogumi päring võimaldab taotleda mitu üksust huvi ühe päringu minimeerimine sõnumi suurus ja teadete arv.

Päringu kogumi väärtus, et te võite seisundioleku veel kobar üksuste (potentsiaalselt kõik kobar üksused, alustades nõutav juursaidil) ühe kõne. Keerukate seisund päringu saab väljendada näiteks:

- Tagastab ainult rakenduste tõrge, ja nende rakenduste hulka kõigi teenuste hoiatus | tõrge. Tagastatud teenuste, kaasata kõik partitsioonid.

- Tagastab ainult seisundi 4 rakendusi, mis on määratud nende nimed.

- Tagastab ainult seisundi rakendused soovitud rakendus tüüp.

- Tagastab kõik juurutatud üksuste sõlme. Tagastab kõik rakendused, kõik juurutatud rakenduste määratud sõlme ja juurutatud teenuse pakettide sõlme.

- Tagastab kõigi koopiate veebisaidil tõrge. Tagastab kõik rakendused, teenused, sektsioonid ja ainult koopiad veebisaidil tõrge.

- Tagastab kõik rakendused. Sisaldavad määratud teenuse kõik partitsioonid.

Praegu seisund kogumi päring on avatud ainult kobar üksus. Tagastab see kobar seisund kogumi, mis sisaldab:

- Kokkuvõtliku kobar seisundioleku.

- Seisund olekus kogumi loendis sõlmed, mis järgivad sisendi filtrid.

- Seisund olekus kogumi programmide loendis Sisestuskeel filtrid sellega. Rakenduse seisund iga riigi kogumi sisaldab loendit kogumi sisendi filtrid ja kõik juurutatud rakendused, mis järgivad filtrite kogumi loendi kõikide teenustega. Sama lastele juurutatud rakendused ja teenused. Sellisel viisil kõigi üksuste klaster potentsiaalselt tagastatavad kui nõutud hierarhiasse.

### <a name="cluster-health-chunk-query"></a>Kobar seisund kogumi päring
Tagastab kobar üksuse seisundi ja see sisaldab hierarhilise seisund olekus osa vaja laste. Sisend:

- [Valikulised] Kobar aluseid hindamise sõlmed ja kobar sündmused.

- [Valikulised] Rakenduse seisund poliitika kaart, kasutada alistada rakenduse manifest poliitika rakendamist.

- [Valikulised] Filtrite sõlmed ja rakendused, et määrata, millised kirjed huvi pakuvad peaks ja tulemis. Filtrid on kindlate üksuste rühmale mõne üksuse või kõigi üksuste samal tasemel puhul. Filtrite loendit võib sisaldada ühe üldine filtri ja/või teatud identifikaatorite trahvi tera üksustele päringu tagastatavad filtrid. Kui see on tühi, ei tagastata tütarde vaikimisi.
Lugege lisateavet [NodeHealthStateFilter](https://msdn.microsoft.com/library/azure/system.fabric.health.nodehealthstatefilter.aspx) ja [ApplicationHealthStateFilter](https://msdn.microsoft.com/library/azure/system.fabric.health.applicationhealthstatefilter.aspx)filtrid. Rakenduse filtreid saab rekursiivselt määrata lastele Täpsemad filtrid.

Kogumi tulem sisaldab laste, et filtrid.

Praegu ei tagasta kogumi päringu vigane hinnangute või üksuse sündmused. Kas saab täiendavat teavet olemasoleva kobar seisund päringu abil.

### <a name="api"></a>API-GA
Saada kobar seisund kogumi, luua mõne `FabricClient` ja kutsuda oma **HealthManager** [GetClusterHealthChunkAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.healthclient.getclusterhealthchunkasync.aspx) meetod. Te saate edastada [ClusterHealthQueryDescription](https://msdn.microsoft.com/library/azure/system.fabric.description.clusterhealthchunkquerydescription.aspx) kirjeldamiseks rakendamist ja Täpsemad filtrid.

Järgmine kood saab kobar seisund kogumi Täpsemad filtrid.

```csharp
var queryDescription = new ClusterHealthChunkQueryDescription();
queryDescription.ApplicationFilters.Add(new ApplicationHealthStateFilter()
    {
        // Return applications only if they are in error
        HealthStateFilter = HealthStateFilter.Error
    });

// Return all replicas
var wordCountServiceReplicaFilter = new ReplicaHealthStateFilter()
    {
        HealthStateFilter = HealthStateFilter.All
    };

// Return all replicas and all partitions
var wordCountServicePartitionFilter = new PartitionHealthStateFilter()
    {
        HealthStateFilter = HealthStateFilter.All
    };
wordCountServicePartitionFilter.ReplicaFilters.Add(wordCountServiceReplicaFilter);

// For specific service, return all partitions and all replicas
var wordCountServiceFilter = new ServiceHealthStateFilter()
{
    ServiceNameFilter = new Uri("fabric:/WordCount/WordCountService"),
};
wordCountServiceFilter.PartitionFilters.Add(wordCountServicePartitionFilter);

// Application filter: for specific application, return no services except the ones of interest
var wordCountApplicationFilter = new ApplicationHealthStateFilter()
    {
        // Always return fabric:/WordCount application
        ApplicationNameFilter = new Uri("fabric:/WordCount"),
    };
wordCountApplicationFilter.ServiceFilters.Add(wordCountServiceFilter);

queryDescription.ApplicationFilters.Add(wordCountApplicationFilter);

var result = await fabricClient.HealthManager.GetClusterHealthChunkAsync(queryDescription);
```

### <a name="powershell"></a>PowerShelli
Cmdlet-käsk saada kobar seisund on [Get-ServiceFabricClusterChunkHealth](https://msdn.microsoft.com/library/mt644772.aspx). Esmalt ühenduse klaster [Ühendamine-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) cmdleti abil.

Järgmine kood saab sõlmed ainult juhul, kui need on välja arvatud teatud sõlm, mis tuleks alati tagastatakse tõrge.

```xml
PS C:\> $errorFilter = [System.Fabric.Health.HealthStateFilter]::Error;
$allFilter = [System.Fabric.Health.HealthStateFilter]::All;

$nodeFilter1 = New-Object System.Fabric.Health.NodeHealthStateFilter -Property @{HealthStateFilter=$errorFilter}
$nodeFilter2 = New-Object System.Fabric.Health.NodeHealthStateFilter -Property @{NodeNameFilter="_Node_1";HealthStateFilter=$allFilter}
# Create node filter list that will be passed in the cmdlet
$nodeFilters = New-Object System.Collections.Generic.List[System.Fabric.Health.NodeHealthStateFilter]
$nodeFilters.Add($nodeFilter1)
$nodeFilters.Add($nodeFilter2)

Get-ServiceFabricClusterHealthChunk -NodeFilters $nodeFilters

HealthState                  : Error
NodeHealthStateChunks        :
                               TotalCount            : 1

                               NodeName              : _Node_1
                               HealthState           : Ok

ApplicationHealthStateChunks : None
```

Järgmine cmdlet-käsk saab kobar kogumi rakenduse filtritega.

```xml
$errorFilter = [System.Fabric.Health.HealthStateFilter]::Error;
$allFilter = [System.Fabric.Health.HealthStateFilter]::All;

# All replicas
$replicaFilter = New-Object System.Fabric.Health.ReplicaHealthStateFilter -Property @{HealthStateFilter=$allFilter}

# All partitions
$partitionFilter = New-Object System.Fabric.Health.PartitionHealthStateFilter -Property @{HealthStateFilter=$allFilter}
$partitionFilter.ReplicaFilters.Add($replicaFilter)

# For WordCountService, return all partitions and all replicas
$svcFilter1 = New-Object System.Fabric.Health.ServiceHealthStateFilter -Property @{ServiceNameFilter="fabric:/WordCount/WordCountService"}
$svcFilter1.PartitionFilters.Add($partitionFilter)

$svcFilter2 = New-Object System.Fabric.Health.ServiceHealthStateFilter -Property @{HealthStateFilter=$errorFilter}

$appFilter = New-Object System.Fabric.Health.ApplicationHealthStateFilter -Property @{ApplicationNameFilter="fabric:/WordCount"}
$appFilter.ServiceFilters.Add($svcFilter1)
$appFilter.ServiceFilters.Add($svcFilter2)

$appFilters = New-Object System.Collections.Generic.List[System.Fabric.Health.ApplicationHealthStateFilter]
$appFilters.Add($appFilter)

Get-ServiceFabricClusterHealthChunk -ApplicationFilters $appFilters

HealthState                  : Error
NodeHealthStateChunks        : None
ApplicationHealthStateChunks :
                               TotalCount            : 1

                               ApplicationName       : fabric:/WordCount
                               ApplicationTypeName   : WordCount
                               HealthState           : Error
                               ServiceHealthStateChunks :
                                   TotalCount            : 1

                                   ServiceName           : fabric:/WordCount/WordCountService
                                   HealthState           : Error
                                   PartitionHealthStateChunks :
                                       TotalCount            : 1

                                       PartitionId           : a1f83a35-d6bf-4d39-b90d-28d15f39599b
                                       HealthState           : Error
                                       ReplicaHealthStateChunks :
                                           TotalCount            : 5

                                           ReplicaOrInstanceId   : 131031502143040223
                                           HealthState           : Ok

                                           ReplicaOrInstanceId   : 131031502346844060
                                           HealthState           : Ok

                                           ReplicaOrInstanceId   : 131031502346844059
                                           HealthState           : Ok

                                           ReplicaOrInstanceId   : 131031502346844061
                                           HealthState           : Ok

                                           ReplicaOrInstanceId   : 131031502346844058
                                           HealthState           : Error
```

Järgmine cmdlet-käsk tagastab kõik juurutatud üksuste sõlme.

```xml
$errorFilter = [System.Fabric.Health.HealthStateFilter]::Error;
$allFilter = [System.Fabric.Health.HealthStateFilter]::All;

$dspFilter = New-Object System.Fabric.Health.DeployedServicePackageHealthStateFilter -Property @{HealthStateFilter=$allFilter}
$daFilter =  New-Object System.Fabric.Health.DeployedApplicationHealthStateFilter -Property @{HealthStateFilter=$allFilter;NodeNameFilter="_Node_2"}
$daFilter.DeployedServicePackageFilters.Add($dspFilter)

$appFilter = New-Object System.Fabric.Health.ApplicationHealthStateFilter -Property @{HealthStateFilter=$allFilter}
$appFilter.DeployedApplicationFilters.Add($daFilter)

$appFilters = New-Object System.Collections.Generic.List[System.Fabric.Health.ApplicationHealthStateFilter]
$appFilters.Add($appFilter)
Get-ServiceFabricClusterHealthChunk -ApplicationFilters $appFilters


HealthState                  : Error
NodeHealthStateChunks        : None
ApplicationHealthStateChunks :
                               TotalCount            : 2

                               ApplicationName       : fabric:/System
                               HealthState           : Ok
                               DeployedApplicationHealthStateChunks :
                                   TotalCount            : 1

                                   NodeName              : _Node_2
                                   HealthState           : Ok
                                   DeployedServicePackageHealthStateChunks :
                                       TotalCount            : 1

                                       ServiceManifestName   : FAS
                                       HealthState           : Ok



                               ApplicationName       : fabric:/WordCount
                               ApplicationTypeName   : WordCount
                               HealthState           : Error
                               DeployedApplicationHealthStateChunks :
                                   TotalCount            : 1

                                   NodeName              : _Node_2
                                   HealthState           : Ok
                                   DeployedServicePackageHealthStateChunks :
                                       TotalCount            : 2

                                       ServiceManifestName   : WordCountServicePkg
                                       HealthState           : Ok

                                       ServiceManifestName   : WordCountWebServicePkg
                                       HealthState           : Ok
```

### <a name="rest"></a>ÜLEJÄÄNUD
Saate kobar seisund kogumi koos [Saada taotlus](https://msdn.microsoft.com/library/azure/mt656722.aspx) või [postituse taotlus](https://msdn.microsoft.com/library/azure/mt656721.aspx) , mis sisaldab rakendamist ja Täpsemad filtrid, mis on kirjeldatud kehasse.

## <a name="general-queries"></a>Üldised päringud
Üldine päringute tagastamiseks määratud tüüpi teenuse struktuuri üksuste loendit. Need on avatud API (meetodite kohta **FabricClient.QueryManager**) kaudu, PowerShelli cmdlet-käskude ja ülejäänud kaudu. Need päringud liitmine mitme komponentide alampäringud. Üks neist on selle [seisundi talletada](service-fabric-health-introduction.md#health-store), mis kuvab liidetud seisundioleku iga päringu tulemi jaoks.  

> [AZURE.NOTE] Üldised päringud tagastavad liidetud seisundioleku üksuse ja rikkaliku seisundi andmed ei sisalda. Kui ettevõte pole terve, saate jälgida seisundi päringutega kõik selle seisundi teavet, sh sündmusi, lapse seisund olekus ja vigane hinnangud.

Kui üldine päringud ei tagasta mõne üksuse tundmatutelt seisundioleku, on võimalik, et seisund poe pole täielik andmed üksuse kohta. Samuti on võimalik, et ei olnud eduka Alampäringu seisund poest (näiteks side tõrge, või oli seisund poe rakendus). Järeltegevuse olemi seisund päringu abil. Kui olevat Alampäringu siirdamiseks vigu, nt võrguprobleemide, võib selle järeltegevuse päringu õnnestuda. Selle võib ka teile rohkem üksikasju poest seisundi kohta, miks üksus on avatud.

Päringuid, mis sisaldavad **HealthState** üksused on:

- Sõlm loend: tagastab loendi sõlmed klaster (leheküljed).
  - API: [FabricClient.QueryClient.GetNodeListAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.queryclient.getnodelistasync.aspx)
  - PowerShelli: Get-ServiceFabricNode
- Rakenduste loend: rakenduste loend annab klaster (leheküljed).
  - API: [FabricClient.QueryClient.GetApplicationListAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.queryclient.getapplicationlistasync.aspx)
  - PowerShelli: Get-ServiceFabricApplication
- Teenuste loend: teenuste loend annab rakenduses (leheküljed).
  - API: [FabricClient.QueryClient.GetServiceListAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.queryclient.getservicelistasync.aspx)
  - PowerShelli: Get-ServiceFabricService
- Sektsiooni loend: tagastab sektsioonide loend (leheküljed) teenus.
  - API: [FabricClient.QueryClient.GetPartitionListAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.queryclient.getpartitionlistasync.aspx)
  - PowerShelli: Get-ServiceFabricPartition
- Koopia loend: tagastab koopiate loendi sektsiooni (leheküljed).
  - API: [FabricClient.QueryClient.GetReplicaListAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.queryclient.getreplicalistasync.aspx)
  - PowerShelli: Get-ServiceFabricReplica
- Rakenduste loendi juurutatud: juurutatud rakenduste loend annab sõlme.
  - API: [FabricClient.QueryClient.GetDeployedApplicationListAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.queryclient.getdeployedapplicationlistasync.aspx)
  - PowerShelli: Get-ServiceFabricDeployedApplication
- Juurutatud teenuste paketi loend: tagastab loendi pakettide juurutatud rakendusega.
  - API: [FabricClient.QueryClient.GetDeployedServicePackageListAsync](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.queryclient.getdeployedservicepackagelistasync.aspx)
  - PowerShelli: Get-ServiceFabricDeployedApplication

> [AZURE.NOTE] Mõned päringute tulemeid leheküljed. Need päringud on tuletatud loendi [PagedList<T>](https://msdn.microsoft.com/library/azure/mt280056.aspx). Kui tulemusi ei sobi sõnumi, tagastatakse ainult lehe ja on ContinuationToken, kus jälgitakse kui loend on peatatud. Jätkate helistada sama päringu ja edastada jätkamiseks luba eelmise päringu järgmise tulemuse saamiseks.

### <a name="examples"></a>Näited

Järgmine kood saab vigane rakenduste klaster:

```csharp
var applications = fabricClient.QueryManager.GetApplicationListAsync().Result.Where(
  app => app.HealthState == HealthState.Error);
```

Järgmine cmdlet-käsk saab rakenduse üksikasjade struktuuri: / WordCount rakendus. Pange tähele, et seisundioleku on hoiatus.

```powershell
PS C:\> Get-ServiceFabricApplication -ApplicationName fabric:/WordCount

ApplicationName        : fabric:/WordCount
ApplicationTypeName    : WordCount
ApplicationTypeVersion : 1.0.0
ApplicationStatus      : Ready
HealthState            : Warning
ApplicationParameters  : { "WordCountWebService_InstanceCount" = "1";
                         "_WFDebugParams_" = "[{"ServiceManifestName":"WordCountWebServicePkg","CodePackageName":"Code","EntryPointType":"Main","Debug
                         ExePath":"C:\\Program Files (x86)\\Microsoft Visual Studio
                         14.0\\Common7\\Packages\\Debugger\\VsDebugLaunchNotify.exe","DebugArguments":" {74f7e5d5-71a9-47e2-a8cd-1878ec4734f1} -p
                         [ProcessId] -tid [ThreadId]","EnvironmentBlock":"_NO_DEBUG_HEAP=1\u0000"},{"ServiceManifestName":"WordCountServicePkg","CodeP
                         ackageName":"Code","EntryPointType":"Main","DebugExePath":"C:\\Program Files (x86)\\Microsoft Visual Studio
                         14.0\\Common7\\Packages\\Debugger\\VsDebugLaunchNotify.exe","DebugArguments":" {2ab462e6-e0d1-4fda-a844-972f561fe751} -p
                         [ProcessId] -tid [ThreadId]","EnvironmentBlock":"_NO_DEBUG_HEAP=1\u0000"}]" }
```

Järgmine cmdlet-käsk antavad teenuste seisundi olek hoiatus

```powershell
PS C:\> Get-ServiceFabricApplication | Get-ServiceFabricService | where {$_.HealthState -eq "Warning"}


ServiceName            : fabric:/WordCount/WordCountService
ServiceKind            : Stateful
ServiceTypeName        : WordCountServiceType
IsServiceGroup         : False
ServiceManifestVersion : 1.0.0
HasPersistedState      : True
ServiceStatus          : Active
HealthState            : Warning
```

## <a name="cluster-and-application-upgrades"></a>Kobar ja rakenduse uuendamine
Mõne jälgida uuendamisel kobar ja rakenduse teenuse struktuuri kontrollib tagamaks, et kõik jääb terve seisund. Kui ettevõte on vigane, nagu hinnatud abil konfigureeritud rakendamist, kehtib versioonitäienduse versiooniuuenduse kohased poliitika määrata järgmise toimingu. Uuele versioonile võib peatatud lubamiseks kasutaja suhtluse (nt probleemide tõrke tingimustega või poliitikaid muutmist) või see võib automaatselt tagasi pöörduda hea eelmise versiooni.

*Kobar* uuendamisel saate kobar versioonitäienduse oleku. Versioonitäienduse oleku sisaldab vigane hindamine, mis osutage, mis on vigane klaster. Kui täiendamise on tagasi pöörata tõttu seisundiga on probleeme, jätab täiendamise oleku meelde viimase vigane põhjust. See teave aitab administraatorid uurida, mis läks valesti pärast uuele versioonile tagasi pöörata või on peatatud.

Samuti *rakenduse* uuendamisel kõigi vigane hinnangute sisalduvad rakenduse täiendamise oleku.

Järgmine kuvatakse rakenduse täiendamise oleku muudetud struktuuri: / WordCount rakendus. Valvekoer teatatud tõrke üks selle koopiad. Uuele versioonile on jooksvalt tagasi, kuna ning kontrolli ei järgita.

```powershell
PS C:\> Get-ServiceFabricApplicationUpgrade fabric:/WordCount

ApplicationName               : fabric:/WordCount
ApplicationTypeName           : WordCount
TargetApplicationTypeVersion  : 1.0.0.0
ApplicationParameters         : {}
StartTimestampUtc             : 4/21/2015 5:23:26 PM
FailureTimestampUtc           : 4/21/2015 5:23:37 PM
FailureReason                 : HealthCheck
UpgradeState                  : RollingBackInProgress
UpgradeDuration               : 00:00:23
CurrentUpgradeDomainDuration  : 00:00:00
CurrentUpgradeDomainProgress  : UD1

                                NodeName            : _Node_1
                                UpgradePhase        : Upgrading

                                NodeName            : _Node_2
                                UpgradePhase        : Upgrading

                                NodeName            : _Node_3
                                UpgradePhase        : PreUpgradeSafetyCheck
                                PendingSafetyChecks :
                                EnsurePartitionQuorum - PartitionId: 30db5be6-4e20-4698-8185-4bd7ca744020
NextUpgradeDomain             : UD2
UpgradeDomainsStatus          : { "UD1" = "Completed";
                                "UD2" = "Pending";
                                "UD3" = "Pending";
                                "UD4" = "Pending" }
UnhealthyEvaluations          :
                                Unhealthy services: 100% (1/1), ServiceType='WordCountServiceType', MaxPercentUnhealthyServices=0%.

                                  Unhealthy service: ServiceName='fabric:/WordCount/WordCountService', AggregatedHealthState='Error'.

                                      Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.

                                      Unhealthy partition: PartitionId='a1f83a35-d6bf-4d39-b90d-28d15f39599b', AggregatedHealthState='Error'.

                                          Unhealthy replicas: 20% (1/5), MaxPercentUnhealthyReplicasPerPartition=0%.

                                          Unhealthy replica: PartitionId='a1f83a35-d6bf-4d39-b90d-28d15f39599b',
                                  ReplicaOrInstanceId='131031502346844058', AggregatedHealthState='Error'.

                                              Error event: SourceId='DiskWatcher', Property='Disk'.

UpgradeKind                   : Rolling
RollingUpgradeMode            : UnmonitoredAuto
ForceRestart                  : False
UpgradeReplicaSetCheckTimeout : 00:15:00
```

Lisateavet [teenuse struktuuri rakenduse täiendamine](service-fabric-application-upgrade.md).

## <a name="use-health-evaluations-to-troubleshoot"></a>Kasutage seisundi hinnangud tõrkeotsing
Iga kord, kui on klaster või rakenduse probleeme, vaadake kobar või rakenduse seisundi täpselt, mis on valesti. Vigane hinnangute üksikasjad kohta, mis käivitab praeguse vigane olekus. Kui teil on vaja, saab süvitsi vigane lapse üksuseks tuvastamiseks põhjus.

> [AZURE.NOTE] Vigane hinnangute kuvada praegune seisund olekusse hinnatakse esimene põhjus üksus. Võib olla mitu sündmused, mis käivitab see olek, kuid see ei saa kajastub hinnangute. Lisateabe saamiseks süvitsi seisund üksuseks aru saada kõik vigane aruannete klaster.

## <a name="next-steps"></a>Järgmised sammud
[Süsteemi seisundi aruannete abil saate tõrkeotsing](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)

[Lisada kohandatud teenuse struktuuri seisundiaruanded](service-fabric-report-health.md)

[Kuidas teatada ja teenuse seisundi kontrollimine](service-fabric-diagnostics-how-to-report-and-check-service-health.md)

[Saate jälgida ja teenuste kohalikult diagnoosimine](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Teenuse struktuuri rakenduse täiendamine](service-fabric-application-upgrade.md)
