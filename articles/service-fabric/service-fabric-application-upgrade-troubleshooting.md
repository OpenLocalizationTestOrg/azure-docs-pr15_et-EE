<properties
   pageTitle="Rakenduse uuendused tõrkeotsing | Microsoft Azure'i"
   description="Selles artiklis antakse ülevaade levinumate probleemide ümber üleminekut teenuse struktuuri rakendus ja kuidas need lahendada."
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/14/2016"
   ms.author="subramar"/>

# <a name="troubleshoot-application-upgrades"></a>Rakenduse uuendused tõrkeotsing

Selles artiklis antakse ülevaade levinud probleemide ümber üleminekut rakenduse Azure teenuse struktuuri ja kuidas need lahendada.

## <a name="troubleshoot-a-failed-application-upgrade"></a>Tõrkeotsing nurjunud rakenduse täiendamine

Kui versiooniuuenduse nurjub, **Get-ServiceFabricApplicationUpgrade** käsu väljund sisaldab täiendavat teavet silumine tõrge.  Järgmisest loendist saate määrata, kuidas täiendavat teavet saab kasutada.

1. Leidke tõrke tüüp.
2. Leidke tõrke põhjus.
3. Ühe või mitme vastasel komponendid uurimiseks eristada.

See teave on saadaval, kui tuvastab teenuse struktuuri ei saa tagasi pöörata või peatada versioonitäienduse sõltumata sellest, kas **FailureAction** .

### <a name="identify-the-failure-type"></a>Tõrge tüübi

**Get-ServiceFabricApplicationUpgrade**väljund, tuvastab **FailureTimestampUtc** ajatempliga (UTC) kus versioonitäienduse tõrke oli avastatud teenuse struktuuri ja **FailureAction** käivitati. **FailureReason** tuvastab ühte kolm võimalikku üksikasjalik ebaõnnestumise põhjused:

1. UpgradeDomainTimeout – näitab, et teatud täiendamine domeeni võttis liiga kaua aega ja **UpgradeDomainTimeout** on aegunud.
2. OverallUpgradeTimeout – näitab, et üldine versioonitäienduse võttis liiga kaua aega ja **UpgradeTimeout** on aegunud.
3. HealthCheck – näitab, et pärast on Värskenda domeen, rakenduse jäid vigane vastavalt määratud seisund poliitikate ja **HealthCheckRetryTimeout** aegunud.

Need kirjed ainult ilmu väljund kui uuele versioonile nurjub ja käivitab tagasipööramine. Lisateavet kuvatakse sõltuvalt tõrke.

### <a name="investigate-upgrade-timeouts"></a>Versioonitäienduse ajalõpud uurimine

Võtke kasutusele ajalõpp ebaõnnestumist kõige sagedamini põhjustab teenuse probleeme. Pärast selle lõiguga väljund on tüüpilised uuendamine kui teenus koopiad või eksemplari ei alustada uut koodi versioonis. Väli **UpgradeDomainProgressAtFailure** sisaldab ootel versioonitäienduse töö ajal tõrge hetktõmmise.

~~~
PS D:\temp> Get-ServiceFabricApplicationUpgrade fabric:/DemoApp

ApplicationName                : fabric:/DemoApp
ApplicationTypeName            : DemoAppType
TargetApplicationTypeVersion   : v2
ApplicationParameters          : {}
StartTimestampUtc              : 4/14/2015 9:26:38 PM
FailureTimestampUtc            : 4/14/2015 9:27:05 PM
FailureReason                  : UpgradeDomainTimeout
UpgradeDomainProgressAtFailure : MYUD1

                                 NodeName            : Node4
                                 UpgradePhase        : PostUpgradeSafetyCheck
                                 PendingSafetyChecks :
                                     WaitForPrimaryPlacement - PartitionId: 744c8d9f-1d26-417e-a60e-cd48f5c098f0

                                 NodeName            : Node1
                                 UpgradePhase        : PostUpgradeSafetyCheck
                                 PendingSafetyChecks :
                                     WaitForPrimaryPlacement - PartitionId: 4b43f4d8-b26b-424e-9307-7a7a62e79750
UpgradeState                   : RollingBackCompleted
UpgradeDuration                : 00:00:46
CurrentUpgradeDomainDuration   : 00:00:00
NextUpgradeDomain              :
UpgradeDomainsStatus           : { "MYUD1" = "Completed";
                                 "MYUD2" = "Completed";
                                 "MYUD3" = "Completed" }
UpgradeKind                    : Rolling
RollingUpgradeMode             : UnmonitoredAuto
ForceRestart                   : False
UpgradeReplicaSetCheckTimeout  : 00:00:00
~~~

Selles näites versioonitäienduse nurjus täiendamine domeeni *MYUD1* ja olid ummikus kaks sektsioonid (*744c8d9f-1d26-417e-a60e-cd48f5c098f0* ja *4b43f4d8-b26b-424e-9307-7a7a62e79750*). Sektsioonid olid kinni, kuna runtime ei saa paigutada target sõlmed *Node1* ja *Node4*esmane koopiad (*WaitForPrimaryPlacement*).

Veenduge, et need kaks sõlmed on täiendamine domeeni *MYUD1*saab kasutada käsku **Get-ServiceFabricNode** . *UpgradePhase* ütleb *PostUpgradeSafetyCheck*, mis tähendab, et turvalisuse kontrolli toimuvad pärast lõpetamist kõik sõlmed täiendamine domeeni üleminekut. Kogu teave osutab võimalikke probleeme rakenduse koodi uue versiooni. Levinumaid probleeme on teenuse tõrked Ava või edendamine esmane kood teed.

Mõne *UpgradePhase* *PreUpgradeSafetyCheck* tähendab, et ilmnenud probleemid uuendamise domeeni ettevalmistamine enne seda tehti. Sel juhul on levinumaid probleeme teenuse tõrgete lahendamine on Sule või alandamine esmane kood tee kaudu.

Praeguse **UpgradeState** on *RollingBackCompleted*, nii, et algne versioonitäienduse peab on tehtud a tagasipööramine **FailureAction**, mis automaatselt tagasi pöörata täiendamise korral. Kui algse versioonitäienduse koos käsitsi **FailureAction**, siis uuele versioonile oleks selle asemel olla peatatud olekus lubamiseks reaalajas silumine rakenduse.

### <a name="investigate-health-check-failures"></a>Uurige tõrkeid seisundi kontrollimine

Erinevad probleemid, mis võib juhtuda pärast üleminekut ja läbides kõik turvalisuse kontrolli kõik sõlmed täiendamine domeenis saate käivitada seisundi kontrolli ebaõnnestumist. Pärast selle lõiguga väljund on tüüpiline versioonitäienduse tõrke tõttu nurjunud seisundikontrollid. Väli **UnhealthyEvaluations** sisaldab seisundikontrollid, mida ei saanud vastavalt määratud [seisund poliitika](service-fabric-health-introduction.md)täiendamise ajal hetktõmmise.

~~~
PS D:\temp> Get-ServiceFabricApplicationUpgrade fabric:/DemoApp

ApplicationName                         : fabric:/DemoApp
ApplicationTypeName                     : DemoAppType
TargetApplicationTypeVersion            : v4
ApplicationParameters                   : {}
StartTimestampUtc                       : 4/24/2015 2:42:31 AM
UpgradeState                            : RollingForwardPending
UpgradeDuration                         : 00:00:27
CurrentUpgradeDomainDuration            : 00:00:27
NextUpgradeDomain                       : MYUD2
UpgradeDomainsStatus                    : { "MYUD1" = "Completed";
                                          "MYUD2" = "Pending";
                                          "MYUD3" = "Pending" }
UnhealthyEvaluations                    :
                                          Unhealthy services: 50% (2/4), ServiceType='PersistedServiceType', MaxPercentUnhealthyServices=0%.

                                          Unhealthy service: ServiceName='fabric:/DemoApp/Svc3', AggregatedHealthState='Error'.

                                              Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.

                                              Unhealthy partition: PartitionId='3a9911f6-a2e5-452d-89a8-09271e7e49a8', AggregatedHealthState='Error'.

                                                  Error event: SourceId='Replica', Property='InjectedFault'.

                                          Unhealthy service: ServiceName='fabric:/DemoApp/Svc2', AggregatedHealthState='Error'.

                                              Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.

                                              Unhealthy partition: PartitionId='744c8d9f-1d26-417e-a60e-cd48f5c098f0', AggregatedHealthState='Error'.

                                                  Error event: SourceId='Replica', Property='InjectedFault'.

UpgradeKind                             : Rolling
RollingUpgradeMode                      : Monitored
FailureAction                           : Manual
ForceRestart                            : False
UpgradeReplicaSetCheckTimeout           : 49710.06:28:15
HealthCheckWaitDuration                 : 00:00:00
HealthCheckStableDuration               : 00:00:10
HealthCheckRetryTimeout                 : 00:00:10
UpgradeDomainTimeout                    : 10675199.02:48:05.4775807
UpgradeTimeout                          : 10675199.02:48:05.4775807
ConsiderWarningAsError                  :
MaxPercentUnhealthyPartitionsPerService :
MaxPercentUnhealthyReplicasPerPartition :
MaxPercentUnhealthyServices             :
MaxPercentUnhealthyDeployedApplications :
ServiceTypeHealthPolicyMap              :
~~~

Esmalt uurib seisundi kontrolli ebaõnnestumist nõuab teenuse struktuuri seisund andmemudeli mõistmine. Kuid isegi ilma sellise põhjalikumat mõistmist, näeme, et kahe teenused on vigane: *struktuuri: / DemoApp/Svc3* ja *struktuuri: / DemoApp/Svc2*, koos tõrge Seisundiaruannete ("InjectedFault" sel juhul). Selles näites kaks neljast teenused on vigane, mis on väiksemad vaikimisi 0% vigane (*MaxPercentUnhealthyServices*).

Uuele versioonile on peatatud vastasel korral, määrates **FailureAction** käsitsi käivitamisel uuele versioonile. Selle režiimi võimaldab meil uurib reaalajas nurjunud olekus enne täiendavate toiming.

### <a name="recover-from-a-suspended-upgrade"></a>Peatatud versiooniuuenduse taastamine

Tagasipööramine **FailureAction**on ei vaja, kuna versioonitäienduse automaatselt koondab tagasi, vastasel korral taastamine. Käsitsi **FailureAction**, kus on mitu taastesuvandeid.

1. A tagasipööramine käsitsi käivitamine
2. Jätkake läbi ülejäänud versioonitäienduse käsitsi
3. Jätkake jälgida täiendamist

Nupp **Start-ServiceFabricApplicationRollback** saab igal ajal tagasipööramine rakenduse käivitamiseks. Kui käsk tagastab edukalt, tagasipööramine kutse on registreeritud süsteemi ja käivitab varsti pärast seda.

**CV-ServiceFabricApplicationUpgrade** käsku saab kasutada käsitsi läbi ülejäänud täiendamise jätkamiseks korraga üks versiooniuuenduse domeeni. Selles režiimis kontrollitakse ainult turvalisuse süsteem. Tehakse enam seisundikontrollid. See käsk saab kasutada ainult siis, kui *UpgradeState* kuvatakse *RollingForwardPending*, mis tähendab, et versioonitäienduse domeenil on lõpule jõudnud, üleminekut, kuid järgmise käivitas (ootel).

Käsk **Update-ServiceFabricApplicationUpgrade** saab kasutada jätkamiseks nii turvalisuse funktsioon jälgida uuele versioonile üle ja läbi.

~~~
PS D:\temp> Update-ServiceFabricApplicationUpgrade fabric:/DemoApp -UpgradeMode Monitored

UpgradeMode                             : Monitored
ForceRestart                            :
UpgradeReplicaSetCheckTimeout           :
FailureAction                           :
HealthCheckWaitDuration                 :
HealthCheckStableDuration               :
HealthCheckRetryTimeout                 :
UpgradeTimeout                          :
UpgradeDomainTimeout                    :
ConsiderWarningAsError                  :
MaxPercentUnhealthyPartitionsPerService :
MaxPercentUnhealthyReplicasPerPartition :
MaxPercentUnhealthyServices             :
MaxPercentUnhealthyDeployedApplications :
ServiceTypeHealthPolicyMap              :

PS D:\temp>
~~~

Uuele versioonile jätkub täiendamine domeeni, kus on viimase peatatud ja kasutada sama versioonitäiendus parameetrid ja seisund poliitika enne. Vajaduse korral, mis tahes versioonitäienduse parameetrite ja eelmise väljund näidatud rakendamist saab muuta sama käsk kui täiendamise uuesti. Selles näites jätkatakse versioonitäienduse jälgitud režiimis parameetrid ja seisund poliitikaid, ei muutu.

## <a name="further-troubleshooting"></a>Täpsemaks tõrkeotsinguks

### <a name="service-fabric-is-not-following-the-specified-health-policies"></a>Teenuse struktuuri ei ole pärast määratud seisund poliitika

Võimalik põhjus 1:

Teenuse struktuuri kõik protsentide tõlgib tegelike arvude üksuste (nt koopiad, sektsioonid ja teenused) hindamine ja ümardatakse alati ülespoole kogu üksuste. Näiteks kui maksimaalne _MaxPercentUnhealthyReplicasPerPartition_ on 21% ja seal on viis, siis teenuse struktuuri võimaldab kahe vigane koopiad (selle is,'Math.Ceiling (5\*0,21)). Seega peaksite seisund poliitika vastavalt määratud.

Võimalik põhjus 2:

Rakendamist on määratud nii protsente kokku teenuste ja mitte kindla teenuse eksemplari. Näiteks enne uuendamist, kui rakendus on neli teenuse eksemplari A, B, C ja D, kus teenuse-D on vigane, kuid vähe mõju rakenduse. Soovime ignoreerida teadaolevad Vigane teenuse D versioonitäiendus ja parameetri *MaxPercentUnhealthyServices* 25%, et toodeti ainult A, B ja C peavad olema terve määramine.

Siiski uuendamisel, D võib muutuda terve samas C muutub vigane. Uuele versioonile veel õnnestub, kuna ainult 25% teenused on vigane. Siiski võib tulemuseks esitatavatele ootamatutele tõrgete tõttu C on ootamatult vigane asemel D. Sellisel juhul tuleks modelleerida D muu teenuse tüübiks a, B ja C. Kuna kohta teenuse tüüp on määratud rakendamist, saate rakendada eri vigane protsent lävede erinevad teenused. 

### <a name="i-did-not-specify-a-health-policy-for-application-upgrade-but-the-upgrade-still-fails-for-some-time-outs-that-i-never-specified"></a>Kas määrata, seisund poliitika rakenduse täiendamine, kuid uuele versioonile ei saa ikka jaoks määratud kunagi mõned ajalõpp

Kui rakendamist pole antud versioonitäienduse taotluse, et need on võetud *ApplicationManifest.xml* praeguse rakenduse versiooni kohta. Näiteks kui värskendate rakenduse X versioonilt 2.0, rakenduste seisund poliitikad versiooni 1.0 versiooni 1.0 jaoks määratud kasutada. Kui uuele versioonile võib kasutada erinevaid tervise poliitika, siis poliitika vaja määrata rakenduse täiendamine API kõne osana. API kõne osana määratud poliitikaid rakendada ainult uuendamisel. Pärast versioonitäienduse lõpulejõudmist kasutatakse määratud *ApplicationManifest.xml* poliitikaid.

### <a name="incorrect-time-outs-are-specified"></a>Vale ajalõpp on määratud

Mõtlesite kohta, mis juhtub, kui ajalõpp on seatud ebajärjekindlalt. Näiteks võib teil on *UpgradeTimeout* , mis on väiksem kui *UpgradeDomainTimeout*. Vastus on, tagastatakse tõrge. Vead tagastatakse, kui *UpgradeDomainTimeout* on väiksem kui *HealthCheckWaitDuration* ja *HealthCheckRetryTimeout*või *UpgradeDomainTimeout* on väiksem kui *HealthCheckWaitDuration* ja *HealthCheckStableDuration*summa.

### <a name="my-upgrades-are-taking-too-long"></a>Minu uuendamine võtab liiga kaua aega

Versiooniuuenduse lõpuleviimiseks aeg sõltub seisundikontrollid ja ajalõpp määratud. Seisundikontrollid ja ajalõpp sõltuvad sellest, kui kaua võtab kopeerida, juurutada ja stabiliseerida rakendus. Koos ajalõpp liiga agressiivne võib tähendab rohkem nurjunud uuendamine, seega soovitame konservatiivselt alustades enam ajalõpp.

Siin on kiire näpunäited kuidas soovitud ajalõpp suhtlete uuendamise korda:

Uuendab jaoks mõne uuendamise domeeni ei saa lõpule viia kiiremini *HealthCheckWaitDuration* + *HealthCheckStableDuration*.

Versioonitäienduse ei saa ebaõnnestuda kiiremini *HealthCheckWaitDuration* + *HealthCheckRetryTimeout*.

Versioonitäienduse aeg versioonitäienduse domeeni jaoks on piiratud *UpgradeDomainTimeout*.  Kui *HealthCheckRetryTimeout* *HealthCheckStableDuration* on nii null ja rakenduse seisundi hoiab üleminek edasi ja tagasi, siis versioonitäienduse lõpuks ajalõpp *UpgradeDomainTimeout*kohta. *UpgradeDomainTimeout* algab loendamine üks kord alustab praeguse täiendamine domeeni uuele versioonile.

## <a name="next-steps"></a>Järgmised sammud

[Täiendamist oma rakenduse abil Visual Studio](service-fabric-application-upgrade-tutorial.md) juhendab teid rakenduse täiendamine Visual Studio abil.

[Rakenduse abil PowerShelli üleminekut](service-fabric-application-upgrade-tutorial-powershell.md) juhendab teid rakenduse täiendamine PowerShelli kaudu.

Saate määrata, kuidas rakenduse uuendab [Täiendamine parameetrite](service-fabric-application-upgrade-parameters.md)abil.

Muuta oma rakenduse uuendamine ühilduvad õppida, kuidas kasutada [Andmete sariväljaanne](service-fabric-application-upgrade-data-serialization.md).

Saate teada, kuidas täiendamisel ilmneb rakenduse [Täpsemate teemade](service-fabric-application-upgrade-advanced.md)viidates täpsemate funktsioonide kasutamiseks.

Levinud probleemide rakenduse uuendamine viidates [Rakenduse uuendused tõrkeotsingu](service-fabric-application-upgrade-troubleshooting.md)juhiseid.
 
