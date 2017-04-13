<properties
    pageTitle="PowerShelli kasutamine häälestamine rakenduse ülevaated on Azure | Microsoft Azure'i"
    description="Automatiseerida konfigureerida Azure diagnostika toru rakenduse ülevaate saamiseks."
    services="application-insights"
    documentationCenter=".net"
    authors="sbtron"
    manager="douge"/>

<tags
    ms.service="application-insights"
    ms.workload="tbd"
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="11/17/2015"
    ms.author="awills"/>

# <a name="using-powershell-to-set-up-application-insights-for-an-azure-web-app"></a>PowerShelli abil saate häälestada rakenduse ülevaated Azure web app

[Microsoft Azure'i](https://azure.com) võib olla [konfigureeritud Azure diagnostika saatmiseks](app-insights-azure-diagnostics.md) [Visual Studio rakenduse ülevaated](app-insights-overview.md). Diagnostika seotud Azure'i pilveteenustega ja Azure VMs. Need täiendavad kaudu saadetud rakenduse ülevaateid SDK abil rakenduses telemeetria. Luua uusi ressursse, Azure automatiseerimine osana saate konfigureerida diagnostika PowerShelli kaudu.

## <a name="azure-template"></a>Azure'i Mall

Kui veebirakenduse on Azure ja loote oma ressursse, mis on Azure ressursihaldur malli abil, saate konfigureerida rakenduse ülevaated, lisades selle sõlme ressursid.

    {
      resources: [
        /* Create Application Insights resource */
        {
          "apiVersion": "2015-05-01",
          "type": "microsoft.insights/components",
          "name": "nameOfAIAppResource",
          "location": "centralus",
          "kind": "web",
          "properties": { "ApplicationId": "nameOfAIAppResource" },
          "dependsOn": [
            "[concat('Microsoft.Web/sites/', myWebAppName)]"
          ]
        }
       ]
     } 

* `nameOfAIAppResource`-Rakenduse ülevaated ressursi nimi
* `myWebAppName`-ID-d web app


## <a name="enable-diagnostics-extension-as-part-of-deploying-a-cloud-service"></a>Diagnostika laiendamiseks käigus juurutamist pilveteenus

Funktsiooni `New-AzureDeployment` cmdlet-käsk on parameetri `ExtensionConfiguration`, mis võtab massiivi diagnostika konfiguratsioone. Neid saab luua, kasutades funktsiooni `New-AzureServiceDiagnosticsExtensionConfig` cmdlet-käsk. Näiteks:

```ps

    $service_package = "CloudService.cspkg"
    $service_config = "ServiceConfiguration.Cloud.cscfg"
    $diagnostics_storagename = "myservicediagnostics"
    $webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml" 
    $workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"

    $primary_storagekey = (Get-AzureStorageKey `
     -StorageAccountName "$diagnostics_storagename").Primary
    $storage_context = New-AzureStorageContext `
       -StorageAccountName $diagnostics_storagename `
       -StorageAccountKey $primary_storagekey

    $webrole_diagconfig = `
     New-AzureServiceDiagnosticsExtensionConfig `
      -Role "WebRole" -Storage_context $storageContext `
      -DiagnosticsConfigurationPath $webrole_diagconfigpath
    $workerrole_diagconfig = `
     New-AzureServiceDiagnosticsExtensionConfig `
      -Role "WorkerRole" `
      -StorageContext $storage_context `
      -DiagnosticsConfigurationPath $workerrole_diagconfigpath

    New-AzureDeployment `
      -ServiceName $service_name `
      -Slot Production `
      -Package $service_package `
      -Configuration $service_config `
      -ExtensionConfiguration @($webrole_diagconfig,$workerrole_diagconfig)

``` 

## <a name="enable-diagnostics-extension-on-an-existing-cloud-service"></a>Diagnostika laiendamiseks klõpsake mõne olemasoleva pilveteenuses

Olemasoleva teenuse, kasutage `Set-AzureServiceDiagnosticsExtension`.

```ps
 
    $service_name = "MyService"
    $diagnostics_storagename = "myservicediagnostics"
    $webrole_diagconfigpath = "MyService.WebRole.PubConfig.xml" 
    $workerrole_diagconfigpath = "MyService.WorkerRole.PubConfig.xml"
    $primary_storagekey = (Get-AzureStorageKey `
         -StorageAccountName "$diagnostics_storagename").Primary
    $storage_context = New-AzureStorageContext `
        -StorageAccountName $diagnostics_storagename `
        -StorageAccountKey $primary_storagekey

    Set-AzureServiceDiagnosticsExtension `
        -StorageContext $storage_context `
        -DiagnosticsConfigurationPath $webrole_diagconfigpath `
        -ServiceName $service_name `
        -Slot Production `
        -Role "WebRole" 
    Set-AzureServiceDiagnosticsExtension `
        -StorageContext $storage_context `
        -DiagnosticsConfigurationPath $workerrole_diagconfigpath `
        -ServiceName $service_name `
        -Slot Production `
        -Role "WorkerRole"
```

## <a name="get-current-diagnostics-extension-configuration"></a>Saada praegune diagnostika laiend konfigureerimine

```ps

    Get-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```


## <a name="remove-diagnostics-extension"></a>Eemaldada diagnostika laiendamine

```ps

    Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService"
```

Kui märkisite diagnostika laiend, kasutades ühte `Set-AzureServiceDiagnosticsExtension` või `New-AzureServiceDiagnosticsExtensionConfig` ilma parameetrit rolli, siis saate eemaldada laiend abil `Remove-AzureServiceDiagnosticsExtension` ilma parameetrit roll. Roll parameetri kasutamisel kui pikendamise lubamine siis seda tuleb kasutada ka eemaldamisel laiendamine.

Iga üksiku rolli diagnostika laiend eemaldamiseks järgmist.

```ps

    Remove-AzureServiceDiagnosticsExtension -ServiceName "MyService" -Role "WebRole"
```


## <a name="see-also"></a>Vt ka

* [Azure'i jälgimine Cloud Services rakenduse ülevaated rakendused](app-insights-cloudservices.md)
* [Azure'i diagnostika saata rakenduse ülevaated](app-insights-azure-diagnostics.md)
* [Seadistamine teatiste automatiseerimine](app-insights-powershell-alerts.md)

