<properties
   pageTitle="Teenuse struktuuri rakenduse täiendamine PowerShelli kaudu | Microsoft Azure'i"
   description="Selles artiklis tutvustatakse kogemus teenuse struktuuri rakenduse juurutamine, muutes koodi ja jooksvalt läbi versiooniuuenduse PowerShelli kaudu."
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


# <a name="service-fabric-application-upgrade-using-powershell"></a>Teenuse struktuuri rakenduse täiendamine PowerShelli abil

> [AZURE.SELECTOR]
- [PowerShelli](service-fabric-application-upgrade-tutorial-powershell.md)
- [Visual Studio](service-fabric-application-upgrade-tutorial.md)

<br/>

Kõige sagedamini kasutatavad ja Soovitatavad versioonitäienduse lähenemine on jälgida jooksva versiooniuuenduse.  Azure teenuse struktuuri jälgib täiustatud rakenduse rakendamist kogumi alusel. Kui mõni update domain (UD) versioon on täiendatud, teenuse struktuuri hindab rakenduse seisundi ning viib järgmise update domain või nurjub, olenevalt seisundi poliitikate uuele versioonile.

Jälgida rakenduse täiendamine toimub kasutades kohalikke või hallatava API-d, PowerShelli või REST. Juhised täiendamisel Visual Studio abil, vt [üleminekut rakenduse Visual Studio abil](service-fabric-application-upgrade-tutorial.md).

Teenuse struktuuri jälgida jooksva uuendamine, saate konfigureerida seisundi hindamise poliitika, mis kasutab teenuse struktuuri, et määrata, kui rakendus on terve rakenduse administraatori poole. Lisaks administraator saab konfigureerida toimingu võetakse seisundi hindamise nurjub (nt tehes on automaatne tagasipööramine.) Selles jaotises antakse ülevaade olulisematest punktidest SDK näidised, mis kasutab PowerShelli jälgida versiooniuuendus.

## <a name="step-1-build-and-deploy-the-visual-objects-sample"></a>Samm 1: Koostamine ja juurutamine Visual objektide näidis


Koostamine ja avaldamine rakenduse rakenduse Project, **VisualObjectsApplication,** paremklõpsake ja valige käsk **Avalda** .  Lisateavet leiate teemast [teenuse struktuuri rakenduse täiendamine õpetuse](service-fabric-application-upgrade-tutorial.md).  Teise võimalusena saate juurutada rakendust PowerShelli abil.

> [AZURE.NOTE] Enne teenuse struktuuri käske võib kasutada PowerShelli, peate esmalt ühendamiseks klaster, kasutades funktsiooni `Connect-ServiceFabricCluster` cmdlet-käsk. Samuti eeldatakse, et klaster on juba loodud teie kohalikus arvutis. [Oma teenuse struktuuri arenduskeskkond häälestamise](service-fabric-get-started.md)kohta leiate artiklist.

Pärast hoone projekti Visual Studios, saate kopeerida rakenduse pakett on ImageStore PowerShelli käsk **Kopeeri-ServiceFabricApplicationPackage** . Järgmise sammuna registreerida rakenduse teenuse struktuuri runtime **Register-ServiceFabricApplicationPackage** cmdlet-käsu abil. Lõplik etapiks on rakenduse eksemplari **New-ServiceFabricApplication** cmdleti abil.  Kolme järgmist on sarnane **Deploy** menüükäsk Visual Studio abil.

Nüüd saate [Teenuse struktuuri Exploreri klaster ning teabe kuvamiseks](service-fabric-visualizing-your-cluster.md). Taotlus on veebiteenus, mis võib olla navigeerida Internet Exploreris, tippides [http://localhost:8081/visualobjects](http://localhost:8081/visualobjects) aadressiribale.  Peaksite nägema mõne visuaalse ujuobjektide kuval liikumine.  Lisaks saate rakenduse oleku **Get-ServiceFabricApplication** .

## <a name="step-2-update-the-visual-objects-sample"></a>Samm 2: Värskendada visuaalse objektide näidis

Märkate, et samm 1 juurutatud versiooniga visuaalse objektide ei pöörata. Vaatame täiendamine selle rakenduse ühele kui ka pöörata visuaalse objektid.

Valige VisualObjects.ActorService projekti VisualObjects lahenduse ja StatefulVisualObjectActor.cs faili avada. Liikuge selle faili meetodit `MoveObject`, kommentaaride välja `this.State.Move()`, ja kommenteerige välja `this.State.Move(true)`. Selle muudatuse pööratakse objektide pärast teenuse versioon on täiendatud.

Samuti tuleb värskendada *ServiceManifest.xml* faili (PackageRoot) projekti **VisualObjects.ActorService**. Värskendage *CodePackage* ja teenuse versioon 2.0 ja vastavad read *ServiceManifest.xml* faili.
Kui paremklõpsate lahenduse manifestifaili muudatusi teha, saate suvandi Visual Studio *Näidata faile redigeerida* .


Pärast muudatuste tegemist, manifest peaks välja nägema järgmine (muudatuste kuvamine esiletõstetud osad):

```xml
<ServiceManifestName="VisualObjects.ActorService" Version="2.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">

<CodePackageName="Code" Version="2.0">
```

(Leitud **VisualObjects** projekti jaotises **VisualObjects** lahenduse all) *ApplicationManifest.xml* faili värskendatud versiooni 2.0 **VisualObjects.ActorService** projekti. Lisaks rakenduse versiooni värskendatakse 2.0.0.0 1.0.0.0 kaudu. *ApplicationManifest.xml* peaks välja nägema järgmine koodilõigu.

```xml
<ApplicationManifestxmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="VisualObjects" ApplicationTypeVersion="2.0.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">

 <ServiceManifestRefServiceManifestName="VisualObjects.ActorService" ServiceManifestVersion="2.0" />
```


Nüüd, koostada projekti lihtsalt **ActorService** projekt, ja seejärel paremklõpsates ja klõpsake suvandi **koostamine** Visual Studio. Kui olete valinud **Kõik taastada**, peate värskendama versioonide jaoks kõik projektid, kuna kood tuleks on muutunud. Järgmise, vaatame paketti värskendatud rakendus, paremklõpsake ***VisualObjectsApplication***, valides menüü teenuse struktuuri, ja seejärel **paketi**. See toiming loob rakendusepaketi, mida saab kasutada.  Värskendatud rakenduse on valmis kasutusele võtta.


## <a name="step-3--decide-on-health-policies-and-upgrade-parameters"></a>Samm 3: Otsustada, missugused rakendamist ja täiendamine parameetrid

Tutvumine [rakenduse täiendamine parameetrite](service-fabric-application-upgrade-parameters.md) ja selle [täiendamise](service-fabric-application-upgrade.md) hea mõista erinevate versioonitäienduse parameetritest, ajalõpp ja seisundi kriteerium rakendatakse. See selgituse teenuse seisundi hindamise kriteeriumi on vaikimisi seatud (ja soovitatav) väärtused, mis tähendab, et kõigi teenuste ja eksemplari oleks _terve_ pärast versiooniuuendust.  

Siiski vaatame suurendada *HealthCheckStableDuration* 60 sekundi (nii, et teenused on terve 20 minutit enne uuele versioonile järgmise värskenduse domeeni jaoks).  Vaatame määrata ka *UpgradeDomainTimeout* olema 1200 sekundit ja *UpgradeTimeout* olema 3000 sekundit.

Lõpuks ka seadmine *UpgradeFailureAction* tagasivõtmisel. See suvand nõuab teenuse struktuuri tagasi pöörata rakenduse eelmise versiooni uuendamisel probleemidest esinemisel. Seega (samm 4) versioonitäienduse käivitamisel on määratud järgmiste parameetrite abil:

FailureAction = tagasipööramine

HealthCheckStableDurationSec = 60

UpgradeDomainTimeoutSec = 1200

UpgradeTimeout = 3000


## <a name="step-4-prepare-application-for-upgrade"></a>Samm 4: Ettevalmistamine taotluse täiendamine

Nüüd on rakenduse ehitatud ja täiendamiseks valmis. Kui avate PowerShelli aken administraatorina ning tippige **Get-ServiceFabricApplication**, see peaks teile teada, et see on rakenduse 1.0.0.0 tüüpi **VisualObjects** , mis on kasutusele võetud.  

Rakenduse pakett talletatakse jaotises järgmised suhteline tee, kus tihendamata teenuse struktuuri SDK - *Samples\Services\Stateful\VisualObjects\VisualObjects\obj\x64\Debug*. Selle kausta, kuhu on talletatud rakenduse pakett tuleks leida "Pakette" kausta. Märkige ruut ajatemplid, et tagada uusimate koostamine (võib-olla peate muutma teed õigesti ka).

Nüüd vaatame kopeerimiseks värskendatud rakendusepaketi teenuse struktuuri ImageStore (kus rakenduse paketid talletatakse teenuse struktuuri järgi). Parameetri *ApplicationPackagePathInImageStore* teavitab teenuse struktuuri, kus leiate selle rakenduse pakett. Oleme pannud värskendatud rakenduse "VisualObjects\_V2" (võib-olla peate muutma tee uuesti õigesti) järgmise käsu abil.

```powershell
Copy-ServiceFabricApplicationPackage  -ApplicationPackagePath .\Samples\Services\Stateful\VisualObjects\VisualObjects\obj\x64\Debug\Package
-ImageStoreConnectionString fabric:ImageStore   -ApplicationPackagePathInImageStore "VisualObjects\_V2"
```

Järgmiseks on selle rakenduse registreeruda teenuse struktuuri, mida saab teha järgmine käsk:

```powershell
Register-ServiceFabricApplicationType -ApplicationPathInImageStore "VisualObjects\_V2"
```

Kui eelmise käsu ei õnnestu, on tõenäoline, et teil on vaja taastada kõigi teenuste. Samm 2 nimetatud, tuleb värskendada oma WebService versiooni.

## <a name="step-5-start-the-application-upgrade"></a>Juhis 5: Rakenduse versioonitäienduse käivitamine

Nüüd saame olete kõik määratud rakenduse täiendamine käivitage järgmine käsk:

```powershell
Start-ServiceFabricApplicationUpgrade -ApplicationName fabric:/VisualObjects -ApplicationTypeVersion 2.0.0.0 -HealthCheckStableDurationSec 60 -UpgradeDomainTimeoutSec 1200 -UpgradeTimeout 3000   -FailureAction Rollback -Monitored
```


Rakenduse nimi on sama, nagu see oli kirjeldatud *ApplicationManifest.xml* faili. Teenuse struktuuri kasutab kindlaks teha, milline rakendus saamine uuendatakse selle nime. Kui seate liiga lühike ajalõpp, võib ilmneda tõrge teade, et probleem. Tõrkeotsingu jaotisest või suurendage soovitud ajalõpp.

Nüüd, kui rakenduse täiendamine tulu saate jälgida selle teenuse struktuuri Exploreriga või, kasutades järgmist PowerShelli käsu: **Get-ServiceFabricApplicationUpgrade struktuuri: / VisualObjects**.

Mõni minut, olek, mis teil, kasutades eelmises PowerShelli käsu märkida, et kõik update domeenide täiendati (valmis). Ja siis tuleb leida visuaalse objektide brauseriaknas hakanud pöörata!

Võite proovida üleminekut 2 versioonilt üle versioonile 3 või versiooni 2 versiooni 1 teostamiseks. Liikumine versiooni 2 version 1 peetakse versiooniuuendust. Mängida ajalõpp ja ise tegema tuttav nende rakendamist. Kui juurutate on Azure arvutikobaras, tuleb parameetrid õigesti seatud. See on hea määrata selle ajalõpp konservatiivselt.


## <a name="next-steps"></a>Järgmised sammud

[Rakenduse Visual Studio abil üleminekut](service-fabric-application-upgrade-tutorial.md) juhendab teid rakenduse täiendamine Visual Studio abil.

Saate määrata, kuidas rakenduse uuendab [täiendamine parameetrite](service-fabric-application-upgrade-parameters.md)abil.

Muuta oma rakenduse uuendamine ühilduvad õppida, kuidas kasutada [andmete sariväljaanne](service-fabric-application-upgrade-data-serialization.md).

Saate teada, kuidas täiendamisel ilmneb rakenduse [täpsemate teemade](service-fabric-application-upgrade-advanced.md)viidates täpsemate funktsioonide kasutamiseks.

Levinud probleemide rakenduse uuendamine viidates [tõrkeotsingu rakenduse uuendamine](service-fabric-application-upgrade-troubleshooting.md)toodud juhised.
