<properties
   pageTitle="Teenuse struktuuri rakenduse juurutamine | Microsoft Azure'i"
   description="Kuidas juurutada ja teenuse struktuuri rakenduste eemaldamine"
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/25/2016"
   ms.author="ryanwi"/>

# <a name="deploy-and-remove-applications-using-powershell"></a>Juurutada ja PowerShelli kaudu rakenduste eemaldamine

> [AZURE.SELECTOR]
- [PowerShelli](service-fabric-deploy-remove-applications.md)
- [Visual Studio](service-fabric-publish-app-remote-cluster.md)

<br/>

Kui ka [rakenduse tüüp on pakitud][10], see on valmis juurutamiseks on Azure teenuse struktuuri kobar sisse. Juurutamise hõlmab kolme järgmist:

1. Rakenduse paketi üleslaadimine
2. Registri rakenduse tüüp
3. Rakenduse loomine

>[AZURE.NOTE] Kui kasutate Visual Studio juurutamine ja silumine rakendusi klaster kohaliku arengu, käsitletakse järgmisi samme automaatselt rakenduse projekti kaustas skriptide PowerShelli skripti kaudu. Selles artiklis antakse tausta kohta, mida nende skriptide teevad nii, et saate teha väljaspool Visual Studio samu toiminguid.

## <a name="upload-the-application-package"></a>Rakenduse paketi üleslaadimine

Rakenduse paketi üleslaadimine paneb selle kohta, kuhu pääseb sisemise teenuse struktuuri komponendid. PowerShelli abil saate teha üles. Enne selle artikli PowerShelli käske käivitamist, algavad alati ühenduse teenuse struktuuri kobar [Ühendamine-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx) abil.

Oletame, et teil on nimega *MyApplicationType* kausta, mis sisaldab on vajalik rakendus väljendada, teenuse manifestid ning kood/andmesaidi paketid. Käsk [Kopeeri-ServiceFabricApplicationPackage](https://msdn.microsoft.com/library/mt125905.aspx) lisatud paketi kobar Image Store. **Get-ImageStoreConnectionStringFromClusterManifest** cmdlet, mis on osa teenuse struktuuri SDK PowerShelli moodul, kasutatakse pilt poe ühendusstringi toomiseks.  Importimine SDK mooduli, käivitage.

```
Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
```

Saate kopeerida rakendusepaketi *C:\users\ryanwi\Documents\Visual Studio 2015\Projects\MyApplication\myapplication\pkg\debug* *c:\temp\MyApplicationType* (Nimeta "silumine" kataloogi "MyApplicationType"). Järgmises näites on lisatud paketi:

~~~
PS C:\temp> dir

    Directory: c:\temp


Mode                LastWriteTime         Length Name                                                                                   
----                -------------         ------ ----                                                                                   
d-----        8/12/2016  10:23 AM                MyApplicationType                                                                          

PS C:\temp> tree /f .\Stateless1Pkg

C:\TEMP\MyApplicationType
│   ApplicationManifest.xml
|
└───Stateless1Pkg
  	|   ServiceManifest.xml
    │
    └───Code
    │   │  Microsoft.ServiceFabric.Data.dll
    │   │  Microsoft.ServiceFabric.Data.Interfaces.dll
    │   │  Microsoft.ServiceFabric.Internal.dll
    │   │  Microsoft.ServiceFabric.Internal.Strings.dll
    │   │  Microsoft.ServiceFabric.Services.dll
    │   │  ServiceFabricServiceModel.dll
    │   │  MyService.exe
    │   │  MyService.exe.config
    │   │  MyService.pdb
    │   │  System.Fabric.dll
    │   │  System.Fabric.Strings.dll
    │   │
    │   └───en-us
  	|         Microsoft.ServiceFabric.Internal.Strings.resources.dll
  	|         System.Fabric.Strings.resources.dll
  	|
    ├───Config
    │     Settings.xml
    │
    └───Data
          init.dat

PS C:\temp> Copy-ServiceFabricApplicationPackage -ApplicationPackagePath MyApplicationType -ImageStoreConnectionString (Get-ImageStoreConnectionStringFromClusterManifest(Get-ServiceFabricClusterManifest))
Copy application package succeeded

PS D:\temp>
~~~

## <a name="register-the-application-package"></a>Rakenduse pakett registreerimine

Rakenduse pakett registreerimisel teeb rakenduse tüüp ja versioon kasutamiseks saadaval Rakendusmanifest deklareeritud. Süsteemi loeb üles laaditud eelmises etapis paketi, kinnitamine (võrdub töötab [Test-ServiceFabricApplicationPackage](https://msdn.microsoft.com/library/mt125950.aspx) kohalikult) paketi, töötlemine paketi sisu ja kopeerige töödeldud paketi sisemise süsteemi asukohta.

~~~
PS D:\temp> Register-ServiceFabricApplicationType MyApplicationType
Register application type succeeded

PS D:\temp> Get-ServiceFabricApplicationType

ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : AppManifestVersion1
DefaultParameters      : {}

PS D:\temp>
~~~

[Register-ServiceFabricApplicationType](https://msdn.microsoft.com/library/mt125958.aspx) käsk tagastab ainult siis, kui süsteemi on edukalt kopeeritud rakenduse pakett. Kui kaua sõltub rakendusepaketi sisu. Vajadusel saab kasutada parameetrit **– TimeoutSec** esitama enam ajalõpp. (Vaikimisi ajalõpp on 60 sekundi järel.)

Käsk [Get-ServiceFabricApplicationType](https://msdn.microsoft.com/library/mt125871.aspx) ära toodud kõik edukalt registreeritud rakenduse tüüp versioonid.

## <a name="create-the-application"></a>Rakenduse loomine

Saate lähtestada rakenduse abil rakenduse tüüp versioon, mis on registreeritud edukalt käsu [New-ServiceFabricApplication](https://msdn.microsoft.com/library/mt125913.aspx) kaudu. Iga rakenduse nimi peab algama selle *struktuuri:* kava ja olema kordumatu iga rakenduse eksemplari. Sel ajal luuakse vaikimisi teenuste sihtrakenduse tüüpi Rakendusmanifest määratletud.

~~~
PS D:\temp> New-ServiceFabricApplication fabric:/MyApp MyApplicationType AppManifestVersion1

ApplicationName        : fabric:/MyApp
ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : AppManifestVersion1
ApplicationParameters  : {}

PS D:\temp> Get-ServiceFabricApplication  

ApplicationName        : fabric:/MyApp
ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : AppManifestVersion1
ApplicationStatus      : Ready
HealthState            : OK
ApplicationParameters  : {}

PS D:\temp> Get-ServiceFabricApplication | Get-ServiceFabricService

ServiceName            : fabric:/MyApp/MyService
ServiceKind            : Stateless
ServiceTypeName        : MyServiceType
IsServiceGroup         : False
ServiceManifestVersion : SvcManifestVersion1
ServiceStatus          : Active
HealthState            : Ok

PS D:\temp>
~~~

Käsk [Get-ServiceFabricApplication](https://msdn.microsoft.com/library/mt163515.aspx) on loetletud kõik rakenduse eksemplarid, mis on edukalt loodud koos oma üldise oleku.

Käsk [Get-ServiceFabricService](https://msdn.microsoft.com/library/mt125889.aspx) loetleb kõik teenuse eksemplarid, mis on loodud rakenduse eksemplari sees. Vaikimisi services (vajaduse korral) on loetletud siin.

Mitu rakenduse eksemplari saab luua mis tahes antud versiooni registreeritud rakenduse tüüp. Iga rakenduse eksemplari käivitatakse eraldi oma töö directory ja protsess.

## <a name="remove-an-application"></a>Rakenduse eemaldamine

Kui rakenduse eksemplari pole enam vaja, saate selle jäädavalt käsu [Eemalda-ServiceFabricApplication](https://msdn.microsoft.com/library/mt125914.aspx) abil eemaldada. See käsk eemaldab automaatselt kõiki teenuseid, mis kuuluvad ka rakenduse jäädavalt eemaldada kõik teenuse olek. Seda toimingut ei saa tühistada ja rakenduse riik ei saa taastada.

~~~
PS D:\temp> Remove-ServiceFabricApplication fabric:/MyApp

Confirm
Continue with this operation?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):
Remove application instance succeeded

PS D:\temp> Get-ServiceFabricApplication
PS D:\temp>
~~~

Kui rakenduse tüübi konkreetse versiooni ei ole enam vaja, peaksite seda unregister [Registreerimise tühistamise-ServiceFabricApplicationType](https://msdn.microsoft.com/library/mt125885.aspx) käsu abil. Registreerimise tühistamine kasutamata tüüpi välja rakenduse paketi sisu tüüpi pilt poe kasutatav ruum. Rakenduse tüübi saab registreerimata, kui ühtegi rakendust kasutada saavad selle vastu ja ei kuni täienduste viitavad sellele.

~~~
PS D:\temp> Get-ServiceFabricApplicationType

ApplicationTypeName    : DemoAppType
ApplicationTypeVersion : v1
DefaultParameters      : {}

ApplicationTypeName    : DemoAppType
ApplicationTypeVersion : v2
DefaultParameters      : {}

ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : AppManifestVersion1
DefaultParameters      : {}

PS D:\temp> Unregister-ServiceFabricApplicationType MyApplicationType AppManifestVersion1
Unregister application type succeeded

PS D:\temp> Get-ServiceFabricApplicationType

ApplicationTypeName    : DemoAppType
ApplicationTypeVersion : v1
DefaultParameters      : {}

ApplicationTypeName    : DemoAppType
ApplicationTypeVersion : v2
DefaultParameters      : {}

PS D:\temp>
~~~

## <a name="troubleshooting"></a>Tõrkeotsing

### <a name="copy-servicefabricapplicationpackage-asks-for-an-imagestoreconnectionstring"></a>Kopeeri – ServiceFabricApplicationPackage küsib mõne ImageStoreConnectionString

Teenuse struktuuri SDK keskkond juba peaks olema õige vaikesätted, luua. Kuid kui vaja, kõik käsud ImageStoreConnectionString peaksid olema samad väärtus, mida kasutatakse teenuse struktuuri kobar. Selle leidmiseks kobar manifestis käsk [Get-ServiceFabricClusterManifest](https://msdn.microsoft.com/library/mt126024.aspx) kaudu:

~~~
PS D:\temp> Copy-ServiceFabricApplicationPackage .\MyApplicationType

cmdlet Copy-ServiceFabricApplicationPackage at command pipeline position 1
Supply values for the following parameters:
ImageStoreConnectionString:

PS D:\temp> Get-ServiceFabricClusterManifest
<ClusterManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Name="Server-Default-SingleNode" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">

    [...]

    <Section Name="Management">
      <Parameter Name="ImageStoreConnectionString" Value="file:D:\ServiceFabric\Data\ImageStore" />
    </Section>

    [...]

PS D:\temp> Copy-ServiceFabricApplicationPackage .\MyApplicationType -ImageStoreConnectionString file:D:\ServiceFabric\Data\ImageStore
Copy application package succeeded

PS D:\temp>
~~~

## <a name="next-steps"></a>Järgmised sammud

[Teenuse struktuuri rakenduse täiendamine](service-fabric-application-upgrade.md)

[Teenuse struktuuri seisund tutvustus](service-fabric-health-introduction.md)

[Diagnoosimine ja teenuse struktuuri teenuse tõrkeotsing](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Mudel rakenduse teenuse struktuuri](service-fabric-application-model.md)

<!--Link references--In actual articles, you only need a single period before the slash-->
[10]: service-fabric-application-model.md
[11]: service-fabric-application-upgrade.md
