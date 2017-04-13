<properties
   pageTitle="Süsteemi seisundi aruannetega tõrkeotsing | Microsoft Azure'i"
   description="Kirjeldatakse Seisundiaruannete, mis on saadetud Azure teenuse struktuuri komponendid ja nende kasutamine tõrkeotsingu kobar või rakenduste seotud probleemid."
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

# <a name="use-system-health-reports-to-troubleshoot"></a>Süsteemi seisundi aruannete abil saate tõrkeotsing

Azure teenuse struktuuri komponendid aruanne välja kasti klaster kõik üksused. [Seisund talletamiseks](service-fabric-health-introduction.md#health-store) loob ja kustutab üksuste süsteemi aruannete põhjal. See korraldab ka need hierarhia, mis salvestab üksuse kasutusviisid.

> [AZURE.NOTE] Mõista seotud mõisted, lugege lisateavet [teenuse struktuuri seisund mudeli](service-fabric-health-introduction.md)juures.

Süsteemi seisundi aruanded pakuvad nähtavus kobar ja rakenduse funktsionaalsust ja lipp probleemid seisundi kaudu. Rakendusi ja teenuseid, süsteemi Seisundiaruannete veenduge, et üksused on rakendatud ja käituvad õigesti teenuse struktuuri seisukohalt. Aruannete ei paku seisundi jälgimine äriloogika teenuse või tuvastamise riputatud protsessid. Kasutusteenuste saate rikastamine seisundi andmed oma loogika teatud teabega.

> [AZURE.NOTE] Valvekoerte Seisundiaruannete on nähtav ainult juhul, *kui* süsteemi luua. Üksuse kustutamisel seisund poe automaatselt kustutab kõik sellega seotud seisundiaruanded. Sama kehtib juhul, kui luuakse uus eksemplar üksuse (nt luuakse uus teenus koopia eksemplari). Kõigi aruannete seostatud vana eksemplar on kustutatud ja poest puhastada.

Süsteemi komponent aruanded, on tähistatud allikas, mis algab on "**süsteemi.**" eesliite. Valvekoerte ei saa kasutada sama eesliitega nende allikad, nagu aruannete Lubamatute parameetrite on tagasi lükatud.
Vaatame mõned süsteemi aruanded, et aru saada, mis käivitab neid ja kuidas need võimalike probleemide lahendamiseks.

> [AZURE.NOTE] Teenuse struktuuri jätkuvalt parandada nähtavus toimub kobar ja rakenduse intressi tingimustele aruandeid lisada.

## <a name="cluster-system-health-reports"></a>Kobar süsteemi seisund aruanded
Kobar seisund üksus luuakse automaatselt seisundi poest. Kui kõik töötab õigesti, pole see süsteemi aruande.

### <a name="neighborhood-loss"></a>Naabruskond kaotsimineku
**System.Federation** aruanded viga, kui tuvastab naabruskond kaotus. Aruanne on üksikud sõlmed ja sõlm ID on kaasatud atribuudi nimi. Kui üks naabruskond on kadunud kogu teenuse struktuuri Helista, tavaliselt eeldatavaid sündmustest (mõlemale poolele vahe aruanne). Kui veel linnaosade lähevad kaotsi, on veel sündmused.

Aruande määrab globaalne üürilepingu ajalõpu time to live. Aruanne on aga uuesti saata igal pool, kui TTL kestab seni, kui tingimus on aktiivne. Sündmuse eemaldatakse automaatselt, kui see lõpeb. Eemaldada, kui aegunud käitumine tagab, et aruanne on puhastada seisund poest õigesti, isegi juhul, kui aruannete sõlm on alla.

- **SourceId**: System.Federation
- **Atribuut**: **naabruskond** algab ja sisaldab teavet sõlm
- **Järgmised toimingud**: Miks läheb kaotsi ning uurida (nt vaadata kobar sõlmed suhtlemine).

## <a name="node-system-health-reports"></a>Sõlm süsteemi seisund aruanded
**System.FM**, mis moodustab Tõrkesiirde halduri teenus, on teavet kobar sõlmed haldava asutus. Iga sõlm peaks olema üks aruanne näitab seisu System.FM. Üksuste sõlm eemaldatakse sõlm olekus eemaldamisel (vt [RemoveNodeStateAsync](https://msdn.microsoft.com/library/azure/mt161348.aspx)).

### <a name="node-updown"></a>Sõlm üles/alla
System.FM aruanded nimega OK, kui sõlme ühendab Helista (see on tööks). See aruanded viga, kui sõlme väljub Helista (see on alla, kas uuendamise või lihtsalt tõttu, ei ole). Seisund hierarhia loodud seisund poe võtab toimingu juurutatud üksuste System.FM sõlm aruannetega korrelatsioonikordaja. Leiab sõlme virtuaalse ema juurutatud kõigi üksuste. Juurutatud üksuste sõlme on avatud päringute kaudu, kui sõlme on nagu, System.FM koos samas eksemplaris nimega eksemplari, mis on seotud üksustega. Kui System.FM teatab, et sõlme on alla või uuesti (uue eksemplari), kustutab seisund poe juurutatud üksused, mis võib alla sõlme või eelmise eksemplari sõlme automaatselt.

- **SourceId**: System.FM
- **Atribuut**: olek
- **Järgmised toimingud**: kui sõlme ei tööta uuendada, see peaks tulla uuesti pärast seda, kui see on täiendatud. Sel juhul tuleks seisundioleku vahetada naasmiseks OK. Kui sõlme ei tule tagasi või see nurjub, peab probleemi veel uurimine.

Järgmises näites on kujutatud System.FM sündmuse lisamine seisund state, OK sõlme:

```powershell

PS C:\> Get-ServiceFabricNodeHealth -NodeName Node.1
NodeName              : Node.1
AggregatedHealthState : Ok
HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 2
                        SentAt                : 4/24/2015 5:27:33 PM
                        ReceivedAt            : 4/24/2015 5:28:50 PM
                        TTL                   : Infinite
                        Description           : Fabric node is up.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 5:28:50 PM

```


### <a name="certificate-expiration"></a>Serdi aegumise
**System.FabricNode** aruannete hoiatus, kui kasutatavat sõlme serdid on aegumise lähedal. On kolme serdid ühe sõlme: **Certificate_cluster**, **Certificate_server**ja **Certificate_default_client**. Kui järgneb on vähemalt kaks nädalat, on aruande seisundioleku OK. Kui järgneb on kahe nädala jooksul, aruande tüüp on hoiatus. TTL need on lõpmatu ja need eemaldatakse sõlme klaster lahkumisel.

- **SourceId**: System.FabricNode
- **Atribuut**: algab **serti** ja sisaldab rohkem teavet serdi tüüp
- **Järgmised toimingud**: serdid värskendada, kui need on aegumise lähedal.

### <a name="load-capacity-violation"></a>Laadi võimsus rikkumine
Teenuse struktuuri laadi koormusetasakaalustusteenuse aruannete Hoiatus Kui tuvastatakse sõlm võimsus seotud rikkumisest teatamine.

 - **SourceId**: System.PLB
 - **Atribuut**: **võimsus** algab
 - **Järgmised toimingud**: märkige esitatud mõõdikute ja vaadata praegune võimsus sõlme.

## <a name="application-system-health-reports"></a>Rakenduse süsteemi seisund aruanded
**System.CM**, mis tähistab halduri teenus, on asutuse, mida haldab rakenduse teave.

### <a name="state"></a>Olek
System.CM aruanded nimega OK, kui rakendus on loodud või värskendatud. Teavitab seisund poe rakendus kustutamisel nii, et seda saab eemaldada poest.

- **SourceId**: System.CM
- **Atribuut**: olek
- **Järgmised toimingud**: kui rakendus on loodud, peaks sisaldama halduri seisundiaruanne. Muul juhul vaadata rakenduse päringu väljastanud (nt PowerShelli cmdlet-käsk * *Get-ServiceFabricApplication-ApplicationName *applicationName***).

Järgmises näites on kujutatud oleku sündmus, klõpsake selle **struktuuri: / WordCount** rakendus:

```powershell
PS C:\> Get-ServiceFabricApplicationHealth fabric:/WordCount -ServicesFilter None -DeployedApplicationsFilter None

ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Ok
ServiceHealthStates             : None
DeployedApplicationHealthStates : None
HealthEvents                    :
                                  SourceId              : System.CM
                                  Property              : State
                                  HealthState           : Ok
                                  SequenceNumber        : 82
                                  SentAt                : 4/24/2015 6:12:51 PM
                                  ReceivedAt            : 4/24/2015 6:12:51 PM
                                  TTL                   : Infinite
                                  Description           : Application has been created.
                                  RemoveWhenExpired     : False
                                  IsExpired             : False
                                  Transitions           : ->Ok = 4/24/2015 6:12:51 PM
```

## <a name="service-system-health-reports"></a>Teenuse süsteemi seisund aruanded
**System.FM**, mis moodustab Tõrkesiirde halduri teenus, on teenuste kohta teabe haldava asutus.

### <a name="state"></a>Olek
System.FM aruanded nimega OK loomisel teenus. Kustutab üksuse seisund poest kui teenus on kustutatud.

- **SourceId**: System.FM
- **Atribuut**: olek

Järgmises näites on kujutatud teenuse oleku sündmus **struktuuri: / WordCount/WordCountService**:

```powershell
PS C:\> Get-ServiceFabricServiceHealth fabric:/WordCount/WordCountService

ServiceName           : fabric:/WordCount/WordCountService
AggregatedHealthState : Ok
PartitionHealthStates :
                        PartitionId           : 875a1caa-d79f-43bd-ac9d-43ee89a9891c
                        AggregatedHealthState : Ok

HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 3
                        SentAt                : 4/24/2015 6:12:51 PM
                        ReceivedAt            : 4/24/2015 6:13:01 PM
                        TTL                   : Infinite
                        Description           : Service has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 6:13:01 PM
```

### <a name="unplaced-replicas-violation"></a>Esikolmikust koopiad rikkumine
Kui see ei leia paigutuse ühe või mitme teenuse koopiate, aruannete **System.PLB** hoiatus. Aruanne on eemaldatud, kui see lõpeb.

- **SourceId**: System.FM
- **Atribuut**: olek
- **Järgmised toimingud**: teenuse piiranguid ja paigutuse praeguse oleku kontrollimine.

Järgmises näites on kujutatud rikkumine konfigureeritud 7 target koopiad koos 5 sõlmed klaster teenuse:

```xml
PS C:\> Get-ServiceFabricServiceHealth fabric:/WordCount/WordCountService


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
                        SequenceNumber        : 131032232425505477
                        SentAt                : 3/23/2016 4:14:02 PM
                        ReceivedAt            : 3/23/2016 4:14:03 PM
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
                        Transitions           : Error->Warning = 3/22/2016 7:57:48 PM, LastOk = 1/1/0001 12:00:00 AM
```

## <a name="partition-system-health-reports"></a>Sektsiooni süsteemi seisund aruanded
**System.FM**, mis moodustab Tõrkesiirde halduri teenus, on teavet teenuse sektsiooni haldava asutus.

### <a name="state"></a>Olek
System.FM aruanded nimega OK kui sektsiooni on loodud ja on terve. Kustutab üksuse seisund poest sektsiooni kustutamisel.

Kui sektsiooni all count minimaalne koopia, selle aruanded viga. Kui sektsiooni pole count minimaalne koopia alla, kuid on target koopia count all, see aruannete hoiatus. Kui sektsiooni kvoorumi kaotsimineku, aruanded System.FM viga.

Muud olulised sündmused kaasata hoiatus ümberkonfigureerimine kulub oodatust oodatust ja kui koostamine kulub oodatust rohkem aega. Eeldatav korda koostamine ja ümberkonfigureerimist on konfigureeritav põhjal teenuste stsenaariumid. Näiteks kui teenus on Teratavu riigi, nt SQL-andmebaasi koostamine võtab rohkem kui teenuse riigi väikese summa.

- **SourceId**: System.FM
- **Atribuut**: olek
- **Järgmised toimingud**: kui seisundioleku pole OK, on võimalik, et mõned koopiad pole loodud, avatud, või end ülendada esmase või teisese õigesti. Paljudel juhtudel põhjuseks on teenuse viga Ava või rolli muutmine rakendamist.

Järgmises näites on kujutatud terve sektsiooni:

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/StatelessPiApplication/StatelessPiService | Get-ServiceFabricPartitionHealth
PartitionId           : 29da484c-2c08-40c5-b5d9-03774af9a9bf
AggregatedHealthState : Ok
ReplicaHealthStates   : None
HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 38
                        SentAt                : 4/24/2015 6:33:10 PM
                        ReceivedAt            : 4/24/2015 6:33:31 PM
                        TTL                   : Infinite
                        Description           : Partition is healthy.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 6:33:31 PM
```

Järgmises näites on kujutatud seisundi sektsioon, mis on count target koopia alla. Järgmiseks tuleb saada sektsiooni kirjeldus, mis näitab, kuidas see on konfigureeritud: **MinReplicaSetSize** on kaks ja **TargetReplicaSetSize** on seitse. Siis tulemuseks arvu sõlmed klaster: viis. Et sel juhul kaks koopiad ei saa paigutada.

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | Get-ServiceFabricPartitionHealth -ReplicasFilter None

PartitionId           : 875a1caa-d79f-43bd-ac9d-43ee89a9891c
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=false.

ReplicaHealthStates   : None
HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Warning
                        SequenceNumber        : 37
                        SentAt                : 4/24/2015 6:13:12 PM
                        ReceivedAt            : 4/24/2015 6:13:31 PM
                        TTL                   : Infinite
                        Description           : Partition is below target replica or instance count.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Ok->Warning = 4/24/2015 6:13:31 PM

PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService

PartitionId            : 875a1caa-d79f-43bd-ac9d-43ee89a9891c
PartitionKind          : Int64Range
PartitionLowKey        : 1
PartitionHighKey       : 26
PartitionStatus        : Ready
LastQuorumLossDuration : 00:00:00
MinReplicaSetSize      : 2
TargetReplicaSetSize   : 7
HealthState            : Warning
DataLossNumber         : 130743727710830900
ConfigurationNumber    : 8589934592


PS C:\> @(Get-ServiceFabricNode).Count
5
```

### <a name="replica-constraint-violation"></a>Koopia piirangu rikkumine
**System.PLB** aruannete hoiatus, kui see tuvastab koopia piirangu rikkumine ja ei saa paigutada sektsiooni koopiad.

- **SourceId**: System.PLB
- **Atribuut**: algab **ReplicaConstraintViolation**

## <a name="replica-system-health-reports"></a>Koopia süsteemi seisund aruanded
**System.RA**, mis moodustab ümberkonfigureerimine agent komponent, on asutuse jaoks koopia olekut.

### <a name="state"></a>Olek
**System.RA** aruanded nimega OK loomisel koopia.

- **SourceId**: System.RA
- **Atribuut**: olek

Järgmises näites on kujutatud terve koopia:

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | Get-ServiceFabricReplica | where {$_.ReplicaRole -eq "Primary"} | Get-ServiceFabricReplicaHealth
PartitionId           : 875a1caa-d79f-43bd-ac9d-43ee89a9891c
ReplicaId             : 130743727717237310
AggregatedHealthState : Ok
HealthEvents          :
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 130743727718018580
                        SentAt                : 4/24/2015 6:12:51 PM
                        ReceivedAt            : 4/24/2015 6:13:02 PM
                        TTL                   : Infinite
                        Description           : Replica has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 6:13:02 PM
```

### <a name="replica-open-status"></a>Koopia avatud olek
Kirjeldus See seisundiaruanne sisaldab API kõne käivitati algusaja (koordineeritud maailmaaega).

**System.RA** aruannete Hoiatus Kui avatud koopia kulub pikem kui konfigureeritud (vaikimisi: 30 minuti). Kui API mõjutab teenuse kättesaadavus, on aruande välja kiiremini (konfigureeritav intervall, mille vaikeväärtus on 30 sekundit). Mõõta aeg sisaldab paljundaja avamine ja teenuse Ava kuluv aeg. Atribuut olekuks OK, kui avatud on lõpule jõudnud.

- **SourceId**: System.RA
- **Atribuut**: **ReplicaOpenStatus**
- **Järgmised toimingud**: kui seisundioleku pole OK uurida miks kulub oodatust kauem avatud koopia.

### <a name="slow-service-api-call"></a>Aeglase API kutse
**System.RAP** ja **System.Replicator** aruande hoiatust, kui kasutaja teenuse kood kõne kestab kauem kui on konfigureeritud. Hoiatus on tühi, kui kõne on lõpule jõudnud.

- **SourceId**: System.RAP või System.Replicator
- **Atribuut**: aeglane API nimi. Kirjelduse leiate täpsemat teavet API on ootel aeg.
- **Järgmised toimingud**: uurida, miks kõne kulub oodatust rohkem aega.

Järgmises näites on kujutatud sektsiooni kvoorumi kaotsimineku ja teha, et selgitada välja, miks uurimine juhiseid. Üks kujundusmuudatusi on hoiatus seisundioleku, nii et saate selle seisund. See näitab, et teenuse toimingu oodatust kauem, sündmuse esitatud System.RAP. Pärast selle teabe, on järgmiseks teenuse kood vaadata ja seal uurida. Sel juhul **RunAsync** rakendamise stateful teenuse põhjustab töötlemata erandi. Selle koopiad ringlussevõtu nii, et te ei näe hoiatus olekus mis tahes koopiad. Saate uuesti, et saada seisundioleku ja vaadake, mis tahes erinevused koopia ID-ga. Teatud juhtudel ning korduste saavad anda teile.

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/HelloWorldStatefulApplication/HelloWorldStateful | Get-ServiceFabricPartitionHealth

PartitionId           : 72a0fb3e-53ec-44f2-9983-2f272aca3e38
AggregatedHealthState : Error
UnhealthyEvaluations  :
                        Error event: SourceId='System.FM', Property='State'.

ReplicaHealthStates   :
                        ReplicaId             : 130743748372546446
                        AggregatedHealthState : Ok

                        ReplicaId             : 130743746168084332
                        AggregatedHealthState : Ok

                        ReplicaId             : 130743746195428808
                        AggregatedHealthState : Warning

                        ReplicaId             : 130743746195428807
                        AggregatedHealthState : Ok

HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Error
                        SequenceNumber        : 182
                        SentAt                : 4/24/2015 7:00:17 PM
                        ReceivedAt            : 4/24/2015 7:00:31 PM
                        TTL                   : Infinite
                        Description           : Partition is in quorum loss.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Warning->Error = 4/24/2015 6:51:31 PM

PS C:\> Get-ServiceFabricPartition fabric:/HelloWorldStatefulApplication/HelloWorldStateful

PartitionId            : 72a0fb3e-53ec-44f2-9983-2f272aca3e38
PartitionKind          : Int64Range
PartitionLowKey        : -9223372036854775808
PartitionHighKey       : 9223372036854775807
PartitionStatus        : InQuorumLoss
LastQuorumLossDuration : 00:00:13
MinReplicaSetSize      : 2
TargetReplicaSetSize   : 3
HealthState            : Error
DataLossNumber         : 130743746152927699
ConfigurationNumber    : 227633266688

PS C:\> Get-ServiceFabricReplica 72a0fb3e-53ec-44f2-9983-2f272aca3e38 130743746195428808

ReplicaId           : 130743746195428808
ReplicaAddress      : PartitionId: 72a0fb3e-53ec-44f2-9983-2f272aca3e38, ReplicaId: 130743746195428808
ReplicaRole         : Primary
NodeName            : Node.3
ReplicaStatus       : Ready
LastInBuildDuration : 00:00:01
HealthState         : Warning

PS C:\> Get-ServiceFabricReplicaHealth 72a0fb3e-53ec-44f2-9983-2f272aca3e38 130743746195428808

PartitionId           : 72a0fb3e-53ec-44f2-9983-2f272aca3e38
ReplicaId             : 130743746195428808
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.RAP', Property='ServiceOpenOperationDuration', HealthState='Warning', ConsiderWarningAsError=false.

HealthEvents          :
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 130743756170185892
                        SentAt                : 4/24/2015 7:00:17 PM
                        ReceivedAt            : 4/24/2015 7:00:33 PM
                        TTL                   : Infinite
                        Description           : Replica has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 7:00:33 PM

                        SourceId              : System.RAP
                        Property              : ServiceOpenOperationDuration
                        HealthState           : Warning
                        SequenceNumber        : 130743756399407044
                        SentAt                : 4/24/2015 7:00:39 PM
                        ReceivedAt            : 4/24/2015 7:00:59 PM
                        TTL                   : Infinite
                        Description           : Start Time (UTC): 2015-04-24 19:00:17.019
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Warning = 4/24/2015 7:00:59 PM
```

Jaotises siluri vigase rakenduse käivitamisel kuvada diagnostika sündmuste windows on erand: RunAsync:

![Visual Studio 2015 diagnostika sündmuste: struktuuri RunAsync tõrge: / HelloWorldStatefulApplication.][1]

Visual Studio 2015 diagnostika sündmuste: RunAsync tõrge **struktuuri: / HelloWorldStatefulApplication**.

[1]: ./media/service-fabric-understand-and-troubleshoot-with-system-health-reports/servicefabric-health-vs-runasync-exception.png


### <a name="replication-queue-full"></a>Dispersioonanalüüs järjekord on täis.
**System.Replicator** aruannete Hoiatus Kui dispersioonanalüüs järjekord on täis. Esmase, see juhtub tavaliselt ühe või mitme teisene koopiad on aeglane tunnistame toimingud. Teisese, tavaliselt siis, kui teenus on aeglane toimingute rakendamiseks. Hoiatus on tühi, kui järjekorda pole enam täielik.

- **SourceId**: System.Replicator
- **Atribuut**: **PrimaryReplicationQueueStatus** või **SecondaryReplicationQueueStatus**, olenevalt sellest koopia roll

### <a name="slow-naming-operations"></a>Nime andmise aeglane toimingud

Kui nimetamise toiming võtab rohkem kui aktsepteeritav aruanded **System.NamingService** seisund oma peamine koopia. Nimetamise toimingud on [CreateServiceAsync](https://msdn.microsoft.com/library/azure/mt124028.aspx) või [DeleteServiceAsync](https://msdn.microsoft.com/library/azure/mt124029.aspx). Mitu võimalust leiate jaotises FabricClient, näiteks [teenuse haldus meetodite](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.servicemanagementclient.aspx) või [atribuudi haldamise võimalused](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.propertymanagementclient.aspx).

> [AZURE.NOTE] Teenuse nimetamise lahendab teenuste nimed klaster asukohta ja võimaldab kasutajatel hallata teenuste nimed ja atribuudid. See on teenuse struktuuri, liigendatud püsis teenus. Üks sektsioonid tähistab asutuse omanik, mis sisaldab metaandmete teenuse struktuuri nimede ja teenuste kohta. Teenuse struktuuri nimed vastendatakse erinevate sektsioonid, nimetatakse omaniku nimi sektsioonid, nii, et teenus on laiendatav. Lugege lisateavet [nimetamise teenuse](service-fabric-architecture.md)kohta.

Kui nimetamise toiming võtab oodatust rohkem aega, toiming on märgitud *peamine koopia, mis pakub toimingu nimetamise teenuse sektsiooni*aruande hoiatus. Kui toiming on lõpule jõudnud edukalt, tühjendatakse hoiatus. Kui toiming on lõpule jõudnud, viga, seisund aruanne sisaldab täpsemat teavet tõrke.

- **SourceId**: System.NamingService
- **Atribuut**: algab eesliide **Duration_** ja see tuvastab aeglane toiming ja teenuse struktuuri nime, millele on rakendatud toiming. Näiteks kui luua teenuse nimi struktuuri: / MyApp/MyService võtab liiga kaua aega, kui atribuut on Duration_AOCreateService.fabric:/MyApp/MyService. AO osutab nimetamise partition selle nimi ja roll.
- **Järgmised toimingud**: Miks nimetamise toiming nurjub sisse. Iga toimingu võib olla erinevad põhjuseid. Näiteks kustutada teenuse võib kinni sõlme, kuna host rakendus jookseb kokku sõlme tõttu kasutaja viga teenuse kood.

Järgmises näites on kujutatud loomine teenuse toiming. Selle toimingu võttis pikem konfigureeritud. AO uued katsed ja saadab töö väärtuseks ei. POLE koos ajalõpp Viimane toiming lõpule. Sel juhul on sama koopia esmane AO nii pole rollid.

```powershell
PartitionId           : 00000000-0000-0000-0000-000000001000
ReplicaId             : 131064359253133577
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.NamingService', Property='Duration_AOCreateService.fabric:/MyApp/MyService', HealthState='Warning', ConsiderWarningAsError=false.

HealthEvents          :
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 131064359308715535
                        SentAt                : 4/29/2016 8:38:50 PM
                        ReceivedAt            : 4/29/2016 8:39:08 PM
                        TTL                   : Infinite
                        Description           : Replica has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 4/29/2016 8:39:08 PM, LastWarning = 1/1/0001 12:00:00 AM

                        SourceId              : System.NamingService
                        Property              : Duration_AOCreateService.fabric:/MyApp/MyService
                        HealthState           : Warning
                        SequenceNumber        : 131064359526778775
                        SentAt                : 4/29/2016 8:39:12 PM
                        ReceivedAt            : 4/29/2016 8:39:38 PM
                        TTL                   : 00:05:00
                        Description           : The AOCreateService started at 2016-04-29 20:39:08.677 is taking longer than 30.000.
                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : Error->Warning = 4/29/2016 8:39:38 PM, LastOk = 1/1/0001 12:00:00 AM

                        SourceId              : System.NamingService
                        Property              : Duration_NOCreateService.fabric:/MyApp/MyService
                        HealthState           : Warning
                        SequenceNumber        : 131064360657607311
                        SentAt                : 4/29/2016 8:41:05 PM
                        ReceivedAt            : 4/29/2016 8:41:08 PM
                        TTL                   : 00:00:15
                        Description           : The NOCreateService started at 2016-04-29 20:39:08.689 completed with FABRIC_E_TIMEOUT in more than 30.000.
                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : Error->Warning = 4/29/2016 8:39:38 PM, LastOk = 1/1/0001 12:00:00 AM
```

## <a name="deployedapplication-system-health-reports"></a>DeployedApplication süsteemi seisundiaruanded
**System.Hosting** on juurutatud üksuste asutuse.

### <a name="activation"></a>Aktiveerimine
System.Hosting aruanded nimega OK kui rakendus on edukalt aktiveeritud sõlme. Muul juhul aruanded seda viga.

- **SourceId**: System.Hosting
- **Atribuut**: aktiveerimise, sh versioon levitamise kohta.
- **Järgmised toimingud**: kui rakendus on vigane, uurida, miks aktiveerimine nurjus.

Järgmises näites on kujutatud aktiveerimine:

```powershell
PS C:\> Get-ServiceFabricDeployedApplicationHealth -NodeName Node.1 -ApplicationName fabric:/WordCount

ApplicationName                    : fabric:/WordCount
NodeName                           : Node.1
AggregatedHealthState              : Ok
DeployedServicePackageHealthStates :
                                     ServiceManifestName   : WordCountServicePkg
                                     NodeName              : Node.1
                                     AggregatedHealthState : Ok

HealthEvents                       :
                                     SourceId              : System.Hosting
                                     Property              : Activation
                                     HealthState           : Ok
                                     SequenceNumber        : 130743727751144415
                                     SentAt                : 4/24/2015 6:12:55 PM
                                     ReceivedAt            : 4/24/2015 6:13:03 PM
                                     TTL                   : Infinite
                                     Description           : The application was activated successfully.
                                     RemoveWhenExpired     : False
                                     IsExpired             : False
                                     Transitions           : ->Ok = 4/24/2015 6:13:03 PM
```

### <a name="download"></a>Laadi alla
**System.Hosting** aruanded viga, kui rakendus alla laadida nurjus.

- **SourceId**: System.Hosting
- **Atribuut**: * *allalaadimine:*RolloutVersion***
- **Järgmised toimingud**: uurige allalaadimine sõlme nurjumise põhjust.

## <a name="deployedservicepackage-system-health-reports"></a>DeployedServicePackage süsteemi seisundiaruanded
**System.Hosting** on juurutatud üksuste asutuse.

### <a name="service-package-activation"></a>Teenuse paketi aktiveerimine
System.Hosting aruannete OK nii teenuse paketi aktiveerimise sõlme õnnestumise. Muul juhul aruanded seda viga.

- **SourceId**: System.Hosting
- **Atribuut**: aktiveerimine
- **Järgmised toimingud**: uurida, miks aktiveerimine nurjus.

### <a name="code-package-activation"></a>Koodi paketi aktiveerimine
**System.Hosting** aruanded nimega OK iga koodi paketi aktiveerimise õnnestumise. Kui aktiveerimine nurjub, aruannete hoiatus konfigureeritud. Kui **CodePackage** Aktiveeri või lõpeb suurem on konfigureeritud **CodePackageHealthErrorThreshold**, aruandeid majutava tõrge ilmnes tõrge. Kui pakett sisaldab mitut koodi paketid, luuakse iga aktiveerimise aruanne.

- **SourceId**: System.Hosting
- **Atribuut**: kasutab eesliite **CodePackageActivation** ja nimi kood paketi ja sisendpunkti, mis sisaldab * *CodePackageActivation:*CodePackageName*:*SetupEntryPoint/EntryPoint* ** (nt **CodePackageActivation:Code:SetupEntryPoint**)

### <a name="service-type-registration"></a>Teenuse tüüp registreerimine
**System.Hosting** aruannete väärtuseks OK, kui teenuse tüüp on edukalt registreeritud. See aruanded viga, kui registreerimise ei olnud teha aega (nagu konfigureeritud **ServiceTypeRegistrationTimeout**abil). Kui konto tüüp on sõlme registreerimata, selle põhjuseks Käitusaeg on suletud. Majutusteenuse aruannete hoiatus.

- **SourceId**: System.Hosting
- **Atribuut**: kasutatakse eesliite **ServiceTypeRegistration** ja see sisaldab teenuse tüüp nimi (nt **ServiceTypeRegistration:FileStoreServiceType**)

Järgmises näites on kujutatud terve juurutatud teenuse paketi:

```powershell
PS C:\> Get-ServiceFabricDeployedServicePackageHealth -NodeName Node.1 -ApplicationName fabric:/WordCount -ServiceManifestName WordCountServicePkg


ApplicationName       : fabric:/WordCount
ServiceManifestName   : WordCountServicePkg
NodeName              : Node.1
AggregatedHealthState : Ok
HealthEvents          :
                        SourceId              : System.Hosting
                        Property              : Activation
                        HealthState           : Ok
                        SequenceNumber        : 130743727751456915
                        SentAt                : 4/24/2015 6:12:55 PM
                        ReceivedAt            : 4/24/2015 6:13:03 PM
                        TTL                   : Infinite
                        Description           : The ServicePackage was activated successfully.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 6:13:03 PM

                        SourceId              : System.Hosting
                        Property              : CodePackageActivation:Code:EntryPoint
                        HealthState           : Ok
                        SequenceNumber        : 130743727751613185
                        SentAt                : 4/24/2015 6:12:55 PM
                        ReceivedAt            : 4/24/2015 6:13:03 PM
                        TTL                   : Infinite
                        Description           : The CodePackage was activated successfully.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 6:13:03 PM

                        SourceId              : System.Hosting
                        Property              : ServiceTypeRegistration:WordCountServiceType
                        HealthState           : Ok
                        SequenceNumber        : 130743727753644473
                        SentAt                : 4/24/2015 6:12:55 PM
                        ReceivedAt            : 4/24/2015 6:13:03 PM
                        TTL                   : Infinite
                        Description           : The ServiceType was registered successfully.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/24/2015 6:13:03 PM
```

### <a name="download"></a>Laadi alla
**System.Hosting** aruanded viga nurjumisel teenuste alla laadida.

- **SourceId**: System.Hosting
- **Atribuut**: * *allalaadimine:*RolloutVersion***
- **Järgmised toimingud**: uurige allalaadimine sõlme nurjumise põhjust.

### <a name="upgrade-validation"></a>Versioonitäienduse valideerimine
**System.Hosting** aruanded viga, kui täiendamise ajal valideerimine nurjub või sõlme ebaõnnestub.

- **SourceId**: System.Hosting
- **Atribuut**: kasutatakse eesliite **FabricUpgradeValidation** ja see sisaldab versioonitäienduse
- **Kirjeldus**: osutab ilmnes tõrge

## <a name="next-steps"></a>Järgmised sammud
[Teenuse struktuuri Seisundiaruannete kuvamine](service-fabric-view-entities-aggregated-health.md)

[Kuidas teatada ja teenuse seisundi kontrollimine](service-fabric-diagnostics-how-to-report-and-check-service-health.md)

[Saate jälgida ja teenuste kohalikult diagnoosimine](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Teenuse struktuuri rakenduse täiendamine](service-fabric-application-upgrade.md)
