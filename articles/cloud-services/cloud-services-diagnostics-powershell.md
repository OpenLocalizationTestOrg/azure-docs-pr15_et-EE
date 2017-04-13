<properties
    pageTitle="Diagnostika PowerShelli kaudu Azure Cloud Services lubamine | Microsoft Azure'i"
    description="Saate teada, kuidas lubada diagnostika puhul pilveteenuste PowerShelli abil"
    services="cloud-services"
    documentationCenter=".net"
    authors="Thraka"
    manager="timlt"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="adegeo"/>


# <a name="enable-diagnostics-in-azure-cloud-services-using-powershell"></a>Luba diagnostika Azure'i Cloud Services PowerShelli abil

Saate koguda Diagnostikaandmete logid, nt Jõudluseloenduri: pilveteenus Azure diagnostika laiend abil jne. Selles artiklis kirjeldatakse, kuidas Azure'i diagnostika pikendamise lubamine pilveteenus PowerShelli kaudu.  Vaadake, [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md) jaoks eeltingimused, mis on vaja seda artiklit.

## <a name="enable-diagnostics-extension-as-part-of-deploying-a-cloud-service"></a>Diagnostika laiendamiseks käigus juurutamist pilveteenus

Seda moodust hea pidev integratsioon tüüpi stsenaariumid, kus saab lubada diagnostika laiend juurutamine pilveteenusesse osana. Kui loote uue pilveteenuses juurutamise saate lubada diagnostika laiend, mille parameetrit *ExtensionConfiguration* [New-AzureDeployment](https://msdn.microsoft.com/library/azure/mt589089.aspx) cmdlet. Parameetri *ExtensionConfiguration* võtab massiivi diagnostika konfiguratsioone saab luua [Uus-AzureServiceDiagnosticsExtensionConfig](https://msdn.microsoft.com/library/azure/mt589168.aspx) cmdlet-käsu abil.

Järgmises näites on kujutatud, kuidas saate lubada diagnostika pilveteenus ja WorkerRole WebRole kummalgi on erinevate diagnostika konfigureerimine.

    $service_name = "MyService"
    $service_package = "CloudService.cspkg"
    $service_config = "ServiceConfiguration.Cloud.cscfg"
    $webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml"
    $workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"

    $webrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WebRole" -DiagnosticsConfigurationPath $webrole_diagconfigpath
    $workerrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WorkerRole" -DiagnosticsConfigurationPath $workerrole_diagconfigpath

    New-AzureDeployment -ServiceName $service_name -Slot Production -Package $service_package -Configuration $service_config -ExtensionConfiguration @($webrole_diagconfig,$workerrole_diagconfig)

Kui diagnostika konfiguratsioonifail määrab StorageAccount elemendi salvestusruumi konto nime, siis cmdlet-käsu New-AzureServiceDiagnosticsExtensionConfig kasutab automaatselt salvestusruumi konto. Seda teha, peab olema sama nimega kasutusele Cloud teenuse tellimuse salvestusruumi konto.

Azure'i SDK 2,6 edasi laiend failid on MSBuild loodud avaldamine sisaldab Sihtväljund salvestusruumi konto nime põhjal diagnostika konfiguratsiooni stringi määratud teenuse konfiguratsioonifailis (.cscfg). Skripti allpool näidatakse, kuidas sõeluda avalda Sihtväljund laiend konfiguratsiooni faile ja konfigureerimiseks diagnostika laiend iga rolli juurutamisel pilveteenusesse.

    $service_name = "MyService"
    $service_package = "C:\build\output\CloudService.cspkg"
    $service_config = "C:\build\output\ServiceConfiguration.Cloud.cscfg"

    #Find the Extensions path based on service configuration file
    $extensionsSearchPath = Join-Path -Path (Split-Path -Parent $service_config) -ChildPath "Extensions"

    $diagnosticsExtensions = Get-ChildItem -Path $extensionsSearchPath -Filter "PaaSDiagnostics.*.PubConfig.xml"
    $diagnosticsConfigurations = @()
    foreach ($extPath in $diagnosticsExtensions)
    {
    #Find the RoleName based on file naming convention PaaSDiagnostics.<RoleName>.PubConfig.xml
    $roleName = ""
    $roles = $extPath -split ".",0,"simplematch"
    if ($roles -is [system.array] -and $roles.Length -gt 1)
        {
        $roleName = $roles[1]
        $x = 2
        while ($x -le $roles.Length)
            {
               if ($roles[$x] -ne "PubConfig")
                {
                    $roleName = $roleName + "." + $roles[$x]
                }
                else
                {
                    break
                }
                $x++
            }
        $fullExtPath = Join-Path -path $extensionsSearchPath -ChildPath $extPath
        $diagnosticsconfig = New-AzureServiceDiagnosticsExtensionConfig -Role $roleName -DiagnosticsConfigurationPath $fullExtPath
        $diagnosticsConfigurations += $diagnosticsconfig
        }
    }
    New-AzureDeployment -ServiceName $service_name -Slot Production -Package $service_package -Configuration $service_config -ExtensionConfiguration $diagnosticsConfigurations

Visual Studio Online kasutab sarnast lähenemisviisi automatiseeritud deploymnts pilveteenuste diagnostika-laiendiga. Vaadake [Avalda-AzureCloudDeployment.ps1](https://github.com/Microsoft/vso-agent-tasks/blob/master/Tasks/AzureCloudPowerShellDeployment/Publish-AzureCloudDeployment.ps1) täieliku näide.

Kui pole StorageAccount määratud diagnostika konfiguratsiooni, siis peate parameetrit StorageAccountName cmdlet edasi. Kui parameeter StorageAccountName on määratud, siis cmdlet alati kasutada määratud parameetri ja mitte üks, mis on määratud konfiguratsioonifailis diagnostika salvestusruumi konto.

Kui diagnostika salvestusruumi konto on mõni muu tellimus pilveteenuselt, siis peate selgesõnaliselt edasi StorageAccountName- ja StorageAccountKey parameetrite cmdlet-i. Kui diagnostika salvestusruumi konto on sama tellimuse cmdlet automaatselt päringu ning seada võtmeväärtuse, kui diagnostika pikendamise lubamine StorageAccountKey parameetri pole vaja. Juhul, kui diagnostika salvestusruumi konto on mõni muu tellimus, seejärel cmdlet ei pruugi saada võti automaatselt ja peate määrata võti StorageAccountKey parameetri kaudu.

    $webrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WebRole" -DiagnosticsConfigurationPath $webrole_diagconfigpath -StorageAccountName $diagnosticsstorage_name -StorageAccountKey $diagnosticsstorage_key
    $workerrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WorkerRole" -DiagnosticsConfigurationPath $workerrole_diagconfigpath -StorageAccountName $diagnosticsstorage_name -StorageAccountKey $diagnosticsstorage_key


## <a name="enable-diagnostics-extension-on-an-existing-cloud-service"></a>Diagnostika laiendamiseks klõpsake mõne olemasoleva pilveteenuses

Cmdlet [Set-AzureServiceDiagnosticsExtension](https://msdn.microsoft.com/library/azure/mt589140.aspx) abil saate lubada või värskendada diagnostika konfiguratsiooni pilveteenuses, kus töötab juba.


    $service_name = "MyService"
    $webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml"
    $workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"

    $webrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WebRole" -DiagnosticsConfigurationPath $webrole_diagconfigpath
    $workerrole_diagconfig = New-AzureServiceDiagnosticsExtensionConfig -Role "WorkerRole" -DiagnosticsConfigurationPath $workerrole_diagconfigpath

    Set-AzureServiceDiagnosticsExtension -DiagnosticsConfiguration @($webrole_diagconfig,$workerrole_diagconfig) -ServiceName $service_name


## <a name="get-current-diagnostics-extension-configuration"></a>Saada praegune diagnostika laiend konfigureerimine
[Get-AzureServiceDiagnosticsExtension](https://msdn.microsoft.com/library/azure/mt589204.aspx) cmdlet-käsu abil saate pilveteenus diagnostika konfiguratsioon.

    Get-AzureServiceDiagnosticsExtension -ServiceName "MyService"

## <a name="remove-diagnostics-extension"></a>Eemaldada diagnostika laiendamine
Diagnostika pilveteenus väljalülitamiseks saate kasutada cmdlet-käsu [Eemalda-AzureServiceDiagnosticsExtension](https://msdn.microsoft.com/library/azure/mt589183.aspx) .

    Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService"

Kui märkisite diagnostika laiendamine *Set-AzureServiceDiagnosticsExtension* või *New-AzureServiceDiagnosticsExtensionConfig* ilma parameetrit *rolli* saate eemaldada abil *Eemalda-AzureServiceDiagnosticsExtension* ilma parameetrit *rolli* laiendamine. *Roll* parameetri kasutamisel kui pikendamise lubamine siis seda tuleb kasutada ka eemaldamisel laiendamine.

Iga üksiku rolli diagnostika laiend eemaldamiseks järgmist.

    Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService" -Role "WebRole"


## <a name="next-steps"></a>Järgmised sammud

- Tõrkeotsing Azure diagnostika- ja teiste meetodite abil abil täiendavad juhised leiate teemast [Azure pilveteenustega ja Virtuaalmasinates diagnostika lubamine](cloud-services-dotnet-diagnostics.md).
- [Diagnostika konfiguratsioon skeemi](https://msdn.microsoft.com/library/azure/dn782207.aspx) selgitatakse erinevate XML-i konfiguratsioone suvandite diagnostika pikendamise eest.
- Diagnostika laiend Virtuaalmasinates lubamise kohta leiate teavet teemast [loomine Windowsi virtuaalse masina jälgida ja diagnostika Azure'i ressursihaldur malli abil](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md)  
