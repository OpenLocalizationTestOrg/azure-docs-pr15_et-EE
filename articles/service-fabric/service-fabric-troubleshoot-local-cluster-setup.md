<properties
   pageTitle="Teie kohaliku teenuse struktuuri kobar häälestamise tõrkeotsing | Microsoft Azure'i"
   description="Selles artiklis antakse ülevaade kogum klaster kohaliku arengu tõrkeotsingu näpunäited"
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="07/08/2016"
   ms.author="seanmck"/>

# <a name="troubleshoot-your-local-development-cluster-setup"></a>Teie kohaliku arengu kobar häälestamise tõrkeotsing

Kui tekib probleeme kohaliku Azure teenuse struktuuri arengu klaster suhtlemisel, vaadake järgmisi soovitusi võimalike lahenduste.

## <a name="cluster-setup-failures"></a>Kobar häälestuse nurjumine

### <a name="cannot-clean-up-service-fabric-logs"></a>Ei saa puhastamiseks teenuse struktuuri logid

#### <a name="problem"></a>Probleem

Samal ajal töötab DevClusterSetup skripti, kuvatakse tõrge umbes järgmine:

    Cannot clean up C:\SfDevCluster\Log fully as references are likely being held to items in it. Please remove those and run this script again.
    At line:1 char:1 + .\DevClusterSetup.ps1
    + ~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo : NotSpecified: (:) [Write-Error], WriteErrorException
    + FullyQualifiedErrorId : Microsoft.PowerShell.Commands.WriteErrorException,DevClusterSetup.ps1


#### <a name="solution"></a>Lahendus

PowerShelli aktiivse akna sulgemine ja avage uus PowerShelli aken administraatorina. Nüüd peaks saama skripti käivitamine.

## <a name="cluster-connection-failures"></a>Kobar ühenduse tõrked

### <a name="service-fabric-powershell-cmdlets-are-not-recognized-in-azure-powershell"></a>Azure'i PowerShelli ei tuvasta teenuse struktuuri PowerShelli cmdlet-käsud

#### <a name="problem"></a>Probleem

Kui proovite käivitada mis tahes teenuse struktuuri PowerShelli cmdlet-käsud, näiteks `Connect-ServiceFabricCluster` Azure PowerShelli aknas see nurjub, räägivad cmdlet ei tuvastata. Selle põhjuseks on, et Azure PowerShelli kasutab 32-bitine versioon Windows PowerShelli (isegi 64-bitine OS versioonid), ja teenuse struktuuri cmdlet-käsud töötavad ainult 64-bitine keskkonnas.

#### <a name="solution"></a>Lahendus

Alati käivitada otse Windows PowerShelli cmdlet-käskude teenuse struktuuri.

>[AZURE.NOTE] Azure'i PowerShelli uusim versioon ei loo teisiti otsetee nii, et see peaks toimuma enam.

### <a name="type-initialization-exception"></a>Erandi tüüp lähtestamine

#### <a name="problem"></a>Probleem

Kui loote klaster PowerShellis, kuvatakse tõrge TypeInitializationException System.Fabric.Common.AppTrace.

#### <a name="solution"></a>Lahendus

Teie tee muutuja installimisel ei õigesti seatud. Logige Windowsist välja ja uuesti sisse. See on täielikult värskendada oma tee.

### <a name="cluster-connection-fails-with-object-is-closed"></a>Kobar ühenduse nurjub "Objekt on suletud"

#### <a name="problem"></a>Probleem

Ühenduse loomine – ServiceFabricCluster kõne nurjub viga umbes järgmine:

    Connect-ServiceFabricCluster : The object is closed.
    At line:1 char:1
    + Connect-ServiceFabricCluster
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo : InvalidOperation: (:) [Connect-ServiceFabricCluster], FabricObjectClosedException
    + FullyQualifiedErrorId : CreateClusterConnectionErrorId,Microsoft.ServiceFabric.Powershell.ConnectCluster

#### <a name="solution"></a>Lahendus

PowerShelli aktiivse akna sulgemine ja avage uus PowerShelli aken administraatorina. Nüüd peaks saama ühendada.

### <a name="fabric-connection-denied-exception"></a>Struktuuri ühenduse keelatud erandi

#### <a name="problem"></a>Probleem

Visual Studio silumine, saate FabricConnectionDeniedException tõrge.

#### <a name="solution"></a>Lahendus

See tõrge ilmneb tavaliselt, kui te proovige proovige alustada teenuse host töödelda käsitsi, selle asemel, võimaldades teenuse struktuuri runtime käivitage see teie eest.

Veenduge, et teil pole teenuse projektide seadmine käivitus projekte teie lahendus. Ainult teenuse struktuuri rakenduse projektide seadma käivitus-projektid.

>[AZURE.TIP] Kui pärast häälestamise, kohaliku klaster hakkab ebanormaalselt käitumine, kohaliku kobar halduri system tray rakenduse abil saate lähtestada. See eemaldab olemasoleva kobar ja uue konto häälestamine. Pange tähele, et kõik juurutatud rakendused ja seotud andmeid, eemaldatakse.

## <a name="next-steps"></a>Järgmised sammud

- [Kuupäevatabelid ja nende klaster süsteemi seisund aruannetega tõrkeotsing](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)
- [Teenuse struktuuri Exploreriga klaster visualiseerimine](service-fabric-visualizing-your-cluster.md)
