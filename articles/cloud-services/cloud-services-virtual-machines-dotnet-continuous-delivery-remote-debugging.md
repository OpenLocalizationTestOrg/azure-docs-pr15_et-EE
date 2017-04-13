<properties
    pageTitle="Luba Kaug silumine pidev kohaletoimetamisega | Microsoft Azure'i"
    description="Saate teada, kuidas lubada remote silumine juurutada Azure pidev kohaletoimetamise kasutamisel"
    services="cloud-services"
    documentationCenter=".net"
    authors="TomArcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="tarcher"/>

# <a name="enable-remote-debugging-when-using-continuous-delivery-to-publish-to-azure"></a>Luba Kaug silumine avaldamiseks Azure'i pidev kohaletoimetamise kasutamisel

Saate lubada remote silumine Azure, pilveteenused või virtuaalmasinates, kui kasutate [pidev kohaletoimetamise](cloud-services-dotnet-continuous-delivery.md) avaldamiseks Azure'i järgmiste juhiste järgi.

## <a name="enabling-remote-debugging-for-cloud-services"></a>Kaug silumine cloud services lubamine

1. Koosta agent, algse keskkonna häälestamine Azure [Käsurea koostada Azure](http://msdn.microsoft.com/library/hh535755.aspx)kirjeldatud.
2. Kuna Kaug silumine käitusaja (msvsmon.exe) on nõutav paketi, installige [Remote Tools for Visual Studio 2015](http://www.microsoft.com/en-us/download/details.aspx?id=48155) (või kui kasutate Visual Studio 2013 [Remote tööriistad Microsoft Visual Studio 2013 värskenduse 5](https://www.microsoft.com/en-us/download/details.aspx?id=48156) ). Teise võimalusena saate kopeerida Kaug silumine kahendfaile süsteemis on installitud Visual Studio.
3. Serdi loomine [Serdid ülevaade Azure'i pilveteenustega](cloud-services-certs-create.md)kirjeldatud. Pfx- ja RDP serdi sõrmejälje hoida ja laadige üles sert target pilveteenusesse.
4. MSBuild käsureal järgmiste suvandite abil koostada ja paketti koos Kaug silumine lubatud. (Asendada tegelik süsteem ja projekti failide nurga sulgudes üksuste teed).

        msbuild /TARGET:PUBLISH /PROPERTY:Configuration=Debug;EnableRemoteDebugger=true;VSX64RemoteDebuggerPath="<remote tools path>";RemoteDebuggerConnectorCertificateThumbprint="<thumbprint of the certificate added to the cloud service>";RemoteDebuggerConnectorVersion="2.7" "<path to your VS solution file>"

    `VSX64RemoteDebuggerPath`on kausta tee, mis sisaldab msvsmon.exe Remote Tools for Visual Studio.
    `RemoteDebuggerConnectorVersion`on Azure SDK versioon oma pilveteenuses. See ka peaksid olema samad Visual Studio abil installitud versioon.

5. Avaldada target pilveteenuses eelmises etapis loodud paketi ja .cscfg-faili abil.
6. Importida serdi (pfx-fail) arvutisse, mis on Azure SDK Visual Studio .net-i installitud. Veenduge, et importida soovitud `CurrentUser\My` salv, muidu manustamise Visual Studios siluri nurjub.

## <a name="enabling-remote-debugging-for-virtual-machines"></a>Remote silumine virtuaalmasinates lubamine

1. Saate luua ka Azure virtuaalse masina. Vt teemat [loomine töötab Windows Server](../virtual-machines/virtual-machines-windows-hero-tutorial.md) või [luua ja hallata Azure'i Virtuaalmasinates Visual Studios](../virtual-machines/virtual-machines-windows-classic-manage-visual-studio.md).
2. [Azure'i klassikaline portaalilehele](http://go.microsoft.com/fwlink/p/?LinkID=269851)vaadata virtuaalse masina armatuurlaua virtuaalse masina **RDP serdi SÕRMEJÄLJE**kuvamiseks. Seda väärtust kasutatakse funktsiooni `ServerThumbprint` laiend konfiguratsiooni väärtus.
3. Kliendi serdi loomine [Serdid ülevaade Azure'i pilveteenustega](cloud-services-certs-create.md) kirjeldatud (jätta pfx- ja RDP serdi sõrmejälje).
4. Azure'i PowerShelli installimine (versioon 0.7.4 või uuem versioon), nagu on kirjeldatud [, kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md).
5. Käivitage järgmine skript RemoteDebug laiendamiseks. Asendage teed ja isikuandmeid oma, nt oma tellimuse nime, teenuse nimi ja sõrmejälje.

    >[AZURE.NOTE] See skript on konfigureeritud lubama Visual Studio 2015. Kui kasutate Visual Studio 2013, muuta selle `$referenceName` ja `$extensionName` ülesanded allpool kasutada `RemoteDebugVS2013` (asemel `RemoteDebugVS2015`).

    <pre>
   Lisage AzureAccount

    Valige-AzureSubscription "Tellimuse Microsoft"

    $vm = get-AzureVM - teenuse nimi "mytestvm1"-nime "mytestvm1"

    $endpoints = @( ,@{Name="RDConnVS2013"; PublicPort = 30400; PrivatePort = 30398} ,@{Name="RDFwdrVS2013"; PublicPort = 31400; PrivatePort = 31398})  

    foreach ($endpoint sisse $endpoints) {lisa-AzureEndpoint - VM $vm-$endpoint nimi. Nime - protokolli tcp - PublicPort $endpoint. PublicPort - LocalPort $endpoint. PrivatePort}

    $referenceName = "Microsoft.VisualStudio.WindowsAzure.RemoteDebug.RemoteDebugVS2015" $publisher = "Microsoft.VisualStudio.WindowsAzure.RemoteDebug" $extensionName = "RemoteDebugVS2015" $version = "1.*" $publicConfiguration = "<PublicConfig>< Connector.Enabled > true < /Connector.Enabled ><ClientThumbprint>56D7D1B25B472268E332F7FC0C87286458BFB6B2</ClientThumbprint><ServerThumbprint>E7DCB00CB916C468CC3228261D6E4EE45C8ED3C6</ServerThumbprint><ConnectorPort>30398</ConnectorPort><ForwarderPort>31398</ForwarderPort></PublicConfig>"

    $vm | Set-AzureVMExtension `
    -ReferenceName $referenceName ` 
    -Publisheri $publisher `
    -ExtensionName $extensionName ` 
    -versiooni $version ' - PublicConfiguration $publicConfiguration

    foreach ($extension $vm sisse. VM. ResourceExtensionReferences) {kui (($extension. Viitenimi - eq $referenceName) `
    -and ($extension.Publisher -eq $publisher) ` 
    - ja ($extension. Nimi – eq $extensionName) '- ja ($extension. Versiooni - eq $version)) {$extension. ResourceExtensionParameterValues [0]. Klahv = 'config.txt' leheküljepiiri}}

    $vm | Update-AzureVM </pre>

6. Importida serdi (pfx) arvutisse, mis on Azure SDK Visual Studio .net-i installitud.
