<properties
    pageTitle="PowerShelli abil automatiseerida teenuse struktuuri Rakendusehaldus | Microsoft Azure'i"
    description="Juurutada, täiendamine, testige ja teenuse struktuuri rakenduste PowerShelli abil eemaldada."
    services="service-fabric"
    documentationCenter=".net"
    authors="rwike77"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-fabric"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/25/2016"
    ms.author="ryanwi"/>

# <a name="automate-the-application-lifecycle-using-powershell"></a>Rakenduse elutsükli PowerShelli abil automatiseerida.

Mitme [teenuse struktuuri rakenduse elutsükli](service-fabric-application-lifecycle.md) automatiseerida.  Selles artiklis kirjeldatakse PowerShelli kasutamine levinud automatiseerida juurutamine, üleminekut, eemaldades ja Azure teenuse struktuuri rakenduste testimine.  Hallatavad ja HTTP API-de rakenduse halduse jaoks on saadaval ka. Vt [rakenduse elutsükli](service-fabric-application-lifecycle.md) lisateavet.  

## <a name="prerequisites"></a>Eeltingimused
Enne, kui teisaldate artiklist tööülesannete, kindlasti järgmist.

+ Saate tutvuda teenuse struktuuri mõisted kirjeldatud [tehniline ülevaade teenuse struktuuri](service-fabric-technical-overview.md).
+ [Installimine käitusaja, SDK ja tööriistu](service-fabric-get-started.md), mis installib ka **ServiceFabric** PowerShelli mooduli.
+ [Luba PowerShelli skripti täitmist](service-fabric-get-started.md#enable-powershell-script-execution).
+ Käivitage kohaliku kobar.  Käivitage PowerShelli uus aken administraatorina ja seejärel käivitage kobar häälestamise skripti SDK kaustast:`& "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\ClusterSetup\DevClusterSetup.ps1"`
+ Enne, kui käivitate selle artikli PowerShelli käske, esmalt ühendada kohaliku teenuse struktuuri kobar [**Ühendamine-ServiceFabricCluster**](https://msdn.microsoft.com/library/azure/mt125938.aspx)abil.`Connect-ServiceFabricCluster localhost:19000`
+ Järgmised toimingud on vaja v1 rakendusepaketi juurutada ja v2 rakenduse pakett üleminek versioonile. Laadige [ **WordCount** proovi taotluse](http://aka.ms/servicefabricsamples) (asub alustamine näidised). Koostamine ja pakkimine rakenduse Visual Studio (Paremklõpsake Solution Exploreris ja valige **paketi** **WordCount** ). Kopeerige v1 paketi `C:\ServiceFabricSamples\Services\WordCount\WordCount\pkg\Debug` et `C:\Temp\WordCount`. Kopeeri `C:\Temp\WordCount` et `C:\Temp\WordCountV2`, uuendage v2 rakendusepaketi loomine. Avatud `C:\Temp\WordCountV2\ApplicationManifest.xml` tekstiredaktoris. **ApplicationManifest** elementi, saate muuta **ApplicationTypeVersion** atribuudi "1.0.0" "2.0.0" rakenduse versiooninumbri värskendada. Muudetud ApplicationManifest.xml faili salvestada.

## <a name="task-deploy-a-service-fabric-application"></a>Ülesanne: Juurutamine teenuse struktuuri rakendus

Kui olete ehitatud ja pakitud rakendus (või alla laadida rakenduse pakett), saate juurutada rakenduse sisse kohaliku teenuse struktuuri kobar. Juurutamise hõlmab rakenduse paketi üleslaadimine, registreerimisel rakenduse tüüp ja rakenduse loomine. Uus rakendus arvutikobaras juurutamiseks kasutage selle jaotise juhiseid.

### <a name="step-1-upload-the-application-package"></a>Samm 1: Laadida rakenduse pakett
Pildi poe rakenduse paketi üleslaadimine asetab asukohas kättesaadavaks sisemise teenuse struktuuri komponendid.  Rakenduse pakett sisaldab vajalikud rakenduse väljendada, teenuse manifestid ning kood, konfigureerimine ja andmete pakett (s) rakenduse või teenuse eksemplari loomiseks.  Käsk [**Kopeeri-ServiceFabricApplicationPackage**](https://msdn.microsoft.com/library/azure/mt125905.aspx) lisatud paketi. Näiteks:

```powershell
Copy-ServiceFabricApplicationPackage C:\Temp\WordCount\ -ImageStoreConnectionString file:C:\SfDevCluster\Data\ImageStoreShare -ApplicationPackagePathInImageStore WordCount
```

### <a name="step-2-register-the-application-type"></a>Samm 2: Registreerimine rakenduse tüüp
Rakenduse pakett registreerimisel teeb rakenduse tüüp ja versioon kasutamiseks saadaval Rakendusmanifest deklareeritud. Süsteemi loeb paketi, mis on üles laaditud juhises 1, veenduge, et paketi (võrdne töötab [**Test-ServiceFabricApplicationPackage**](https://msdn.microsoft.com/library/azure/mt125950.aspx) kohalikult), töötlemine paketi sisu ja kopeerige töödeldud paketi sisemise süsteemi asukohta.  Käivitage [**Register-ServiceFabricApplicationType**](https://msdn.microsoft.com/library/azure/mt125958.aspx) cmdlet-käsk:

```powershell
Register-ServiceFabricApplicationType WordCount
```
Rakenduse tüüpi registreeritud klaster kuvamiseks käivitage cmdlet-käsk [Get-ServiceFabricApplicationType](https://msdn.microsoft.com/library/azure/mt125871.aspx) :

```powershell
Get-ServiceFabricApplicationType
```

### <a name="step-3-create-the-application-instance"></a>Samm 3: Looge rakenduse
Rakenduse saate lähtestada, kasutades rakenduse tüüp versioon, mis on registreeritud edukalt käsu [**New-ServiceFabricApplication**](https://msdn.microsoft.com/library/azure/mt125913.aspx) abil. Iga rakenduse nimi on deklareeritud veebisaidil juurutada aja ja peavad algama selle **struktuuri:** kava ja olema kordumatu iga rakenduse eksemplari. Rakenduse nimi ja rakenduse tüüp versioon **ApplicationManifest.xml** faili rakendusepaketi deklareerida. Kui mis tahes vaikimisi teenustele määratletud sihtrakenduse tüüpi Rakendusmanifest, siis need on loodud sel ajal.

```powershell
New-ServiceFabricApplication fabric:/WordCount WordCount 1.0.0
```

Käsk [**Get-ServiceFabricApplication**](https://msdn.microsoft.com/library/azure/mt163515.aspx) on loetletud kõik rakenduse eksemplarid, mis on edukalt loodud koos oma üldise oleku. Käsk [**Get-ServiceFabricService**](https://msdn.microsoft.com/library/azure/mt125889.aspx) sisaldab loendit kõigist edukalt loodi maksimaalselt rakenduse eksemplari eksemplare. Vaikimisi services (vajaduse korral) on loetletud.

```powershell
Get-ServiceFabricApplication

Get-ServiceFabricApplication | Get-ServiceFabricService
```

## <a name="task-upgrade-a-service-fabric-application"></a>Ülesanne: Täiendamine teenuse struktuuri rakendus
Täiendamist varem juurutatud teenuse struktuuri rakendusega värskendatud rakendusepaketi abil. Selles ülesandes uuendab WordCount rakendus, mis on juurutatud "ülesanne: teenuse struktuuri rakenduse juurutamine." Lisateabe saamiseks lugege [teenuse struktuuri rakenduse täiendamine](service-fabric-application-upgrade.md) kaudu.

Hoida asju näiteks lihtsa, värskendatud rakenduse versiooninumbri WordCountV2 rakenduse pakett loodud eeltingimused. Suurema stsenaarium eeldab teenuse kood, konfigureerimine või andmete failide värskendamine ja taastamine ja pakendamine rakenduse arvudega värskendatud versiooni.  

### <a name="step-1-upload-the-updated-application-package"></a>Samm 1: Laadida värskendatud rakendusepaketi
WordCount v1 rakendus on täiendamiseks valmis. Kui avate PowerShelli aken, kui administraator ja tüüp [**Get-ServiceFabricApplication**](https://msdn.microsoft.com/library/azure/mt163515.aspx), kuvatakse see versioon 1.0.0 WordCount rakenduse tüüp on juurutatud.  

Nüüd kopeerida värskendatud rakendusepaketi teenuse struktuuri pilt poest (kui rakenduse paketid on talletatud teenuse struktuuri). Parameetri **ApplicationPackagePathInImageStore** teavitab teenuse struktuuri, kus leiate selle rakenduse pakett. Järgmine käsk kopeerib **WordCountV2** pilt poest rakenduse pakett:  

```powershell
Copy-ServiceFabricApplicationPackage C:\Temp\WordCountV2\ -ImageStoreConnectionString file:C:\SfDevCluster\Data\ImageStoreShare -ApplicationPackagePathInImageStore WordCountV2

```
### <a name="step-2-register-the-updated-application-type"></a>Samm 2: Registreerimine värskendatud rakenduse tüüp
Järgmiseks on rakenduse uus versioon registreeruda teenuse struktuuri, mida saab [**Register-ServiceFabricApplicationType**](https://msdn.microsoft.com/library/azure/mt125958.aspx) cmdleti abil.

```powershell
Register-ServiceFabricApplicationType WordCountV2
```

### <a name="step-3-start-the-upgrade"></a>Samm 3: Versioonitäienduse käivitamine
Versioonitäienduse parameetrid, ajalõpud ja seisundi kriteeriumid saab rakendada rakenduse uuendused. Lugege lisateavet [rakenduse täiendamine parameetrite](service-fabric-application-upgrade-parameters.md) ja [täiendamise](service-fabric-application-upgrade.md) dokumendid. Kõigi teenuste ja eksemplari peab olema _terve_ pärast uuele versioonile.  Määratud **HealthCheckStableDuration** 60 sekundi järel (nii, et teenused on terve 20 minutit enne uuele versioonile järgmise versioonitäienduse domeeni jaoks).  Määrata ka **UpgradeDomainTimeout** 1200 sekundit ja **UpgradeTimeout** 3000 sekundit. Lõpuks seatud **UpgradeFailureAction** **tagasipööramine**, mis taotleb, et teenuse struktuuri koondab tagasi rakenduse eelmise versiooni, kui uuele versioonile üleminekul on ilmnenud tõrkeid.

Nüüd saate alustada rakenduse täiendamine abil [**Algus-ServiceFabricApplicationUpgrade**](https://msdn.microsoft.com/library/azure/mt125975.aspx) cmdlet-käsk:

```powershell
Start-ServiceFabricApplicationUpgrade -ApplicationName fabric:/WordCount -ApplicationTypeVersion 2.0.0 -HealthCheckStableDurationSec 60 -UpgradeDomainTimeoutSec 1200 -UpgradeTimeout 3000  -FailureAction Rollback -Monitored
```

Pange tähele, et rakenduse nimi on sama, mis varem juurutatud v1.0.0 rakenduse nimi (struktuuri: / WordCount). Teenuse struktuuri kasutab kindlaks teha, milline rakendus saamine uuendatakse selle nime. Kui seate liiga lühike ajalõpp, võib ilmneda ajalõpp tõrge teade, et probleem. Vaadake [tõrkeotsing rakenduse uuendused](service-fabric-application-upgrade-troubleshooting.md)või suurendada selle ajalõpp.

### <a name="step-4-check-upgrade-progress"></a>Samm 4: Märkige versioonitäienduse edenemine
Rakenduse täiendamine edenemist saate jälgida [Teenuse struktuuri Exploreri](service-fabric-visualizing-your-cluster.md)kaudu või kasutades [**Get-ServiceFabricApplicationUpgrade**](https://msdn.microsoft.com/library/azure/mt125988.aspx) cmdlet-käsk:

```powershell
Get-ServiceFabricApplicationUpgrade fabric:/WordCount
```

Paar minutit, [Get-ServiceFabricApplicationUpgrade](https://msdn.microsoft.com/library/azure/mt125988.aspx) cmdlet-käsk näitab, et kõik täiendamine domeenide täiendati (valmis).

## <a name="task-test-a-service-fabric-application"></a>Ülesanne: Testige teenuse struktuuri rakendus

Teenuse kvaliteedi kirjutamiseks arendajad peavad saama usaldusväärsed taristu vead testimiseks stabiilsuse oma teenuste esile. Teenuse struktuuri annab arendajate võimalus põhjustada viga toimingud ja testida teenuste juuresolekul tõrkeid kaose ja Tõrkesiirde test stsenaariumide abil.  Lisateabe saamiseks lugege läbi [Testability ülevaade](service-fabric-testability-overview.md) .

### <a name="step-1-run-the-chaos-test-scenario"></a>Samm 1: Run kaose test stsenaarium
Kaose test stsenaarium loob üle kogu teenuse struktuuri kobar vead. Stsenaarium tihendab vead üldiselt näha, kuude või aastate mõne tunni pärast. Üksteise vead kõrge viga määra kombinatsiooni leiab nurgas juhtudel, et teisiti vastamata. Järgmises näites käivitatakse kaose test stsenaarium 60 minutit.

```powershell
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$concurrentFaults = 3
$waitTimeBetweenIterationsSec = 60

Invoke-ServiceFabricChaosTestScenario -TimeToRunMinute $timeToRun -MaxClusterStabilizationTimeoutSec $maxStabilizationTimeSecs -MaxConcurrentFaults $concurrentFaults -EnableMoveReplicaFaults -WaitTimeBetweenIterationsSec $waitTimeBetweenIterationsSec
```

### <a name="step-2-run-the-failover-test-scenario"></a>Samm 2: Käivitada Tõrkesiirde test stsenaarium
Selle Tõrkesiirde kontrollimine stsenaarium sihtkohtade kindla teenuse sektsiooni asemel terve kobar ja muude teenuste jätab ei mõjuta. Stsenaarium itereerib jäljendatud vead teenuse valideerimine kaudu oma äriloogika käitamise ajal. Tõrge teenuse valideerimisel näitab probleemi, mis tuleb uurimine. Tõrkesiirde test tekitab korraga kaose testi stsenaarium, mis võivad põhjustada mitme vead asemel ainult üks viga. Järgmises näites töötab Tõrkesiirde katset 60 minutit vastu struktuuri: / WordCount/WordCountService teenus.

```powershell
$timeToRun = 60
$maxStabilizationTimeSecs = 180
$waitTimeBetweenFaultsSec = 10
$serviceName = "fabric:/WordCount/WordCountService"

Invoke-ServiceFabricFailoverTestScenario -TimeToRunMinute $timeToRun -MaxServiceStabilizationTimeoutSec $maxStabilizationTimeSecs -WaitTimeBetweenFaultsSec $waitTimeBetweenFaultsSec -ServiceName $serviceName -PartitionKindUniformInt64 -PartitionKey 1
```

## <a name="task-remove-a-service-fabric-application"></a>Ülesanne: Eemaldada teenuse struktuuri rakendus
Saate kustutada eksemplari juurutatud rakendusega, ettevalmistatud tüübi eemaldamine klaster ja rakenduse pakett eemaldamine on ImageStore.

### <a name="step-1-remove-an-application-instance"></a>Samm 1: Eemaldage rakenduse eksemplari
Kui rakenduse eksemplari pole enam vaja, saate selle jäädavalt [**Eemaldamine-ServiceFabricApplication**](https://msdn.microsoft.com/library/azure/mt125914.aspx) cmdleti abil eemaldada. See eemaldab automaatselt ka kõigi teenuste kuuluva rakenduse, jäädavalt eemaldada kõik teenuse olek. Seda toimingut ei saa tühistada ja rakenduse riik ei saa taastada.

```powershell
Remove-ServiceFabricApplication fabric:/WordCount
```

### <a name="step-2-unregister-the-application-type"></a>Samm 2: Unregister rakenduse tüüp
Kui teil pole enam vaja konkreetse versiooni rakenduse tüübi, unregister [**Registreerimise tühistamise-ServiceFabricApplicationType**](https://msdn.microsoft.com/library/azure/mt125885.aspx) cmdleti abil. Registreerimise tühistamine kasutamata tüüpi välja rakendusepaketi pilt poes kasutatav ruum. Rakenduse tüübi saab registreerimata, kui pole ühtegi rakendust käivitatud selle vastu või ootel viitamine selle rakenduse uuendused.

```powershell
Unregister-ServiceFabricApplicationType WordCount 1.0.0
```

### <a name="step-3-remove-the-application-package"></a>Samm 3: Rakenduse paketi eemaldamiseks
Kui rakenduse tüüp on registreerimata, saate rakenduse pakett eemaldada pildi poe [**Eemalda-ServiceFabricApplicationPackage**](https://msdn.microsoft.com/library/azure/mt163532.aspx) cmdleti abil.

```powershell
Remove-ServiceFabricApplicationPackage -ImageStoreConnectionString file:C:\SfDevCluster\Data\ImageStoreShare -ApplicationPackagePathInImageStore WordCount
```

## <a name="next-steps"></a>Järgmised sammud
[Teenuse struktuuri rakenduse elutsükkel](service-fabric-application-lifecycle.md)

[Rakenduse juurutamine](service-fabric-deploy-remove-applications.md)

[Rakenduse täiendamine](service-fabric-application-upgrade.md)

[Azure teenuse struktuuri cmdlet-käsud](https://msdn.microsoft.com/library/azure/mt125965.aspx)

[Azure teenuse struktuuri testability cmdlet-käsud](https://msdn.microsoft.com/library/azure/mt125844.aspx)
