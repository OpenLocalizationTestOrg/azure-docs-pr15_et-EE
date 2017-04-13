<properties 
    pageTitle="PowerShelli skripti loomiseks on rakenduse ülevaated ressurss" 
    description="Rakenduse ülevaated ressursid loomise automatiseerida." 
    services="application-insights" 
    documentationCenter="windows"
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="02/19/2016" 
    ms.author="awills"/>

#  <a name="powershell-script-to-create-an-application-insights-resource"></a>PowerShelli skripti loomiseks on rakenduse ülevaated ressurss

*Rakenduse ülevaated on eelvaade.*

Kui soovite jälgida uue rakenduse - või [Visual Studio rakenduse](https://azure.microsoft.com/services/application-insights/)ülevaated - rakenduse uus versioon, saate häälestada Microsoft Azure uue ressursi. Selle ressursi on, kus teie rakenduse telemeetria andmeid analüüsida ja kuvada. 

PowerShelli abil saate luua uue ressursi automatiseerida.

Näiteks kui arendate mobiilsideseadme rakendus, on tõenäoline, et igal ajal tekib mitu avaldatud versiooni oma rakendust oma klientide. Te ei soovi telemeetria tulemuse saamiseks segamini erinevates versioonides. Nii saate luua uue ressursi iga järgu jaoks oma Koosta protsess.

## <a name="script-to-create-an-application-insights-resource"></a>Skripti loomiseks on rakenduse ülevaated ressurss

Vaadake andmed asjakohaste cmdlet-käsk:

* [Uue AzureRmResource](https://msdn.microsoft.com/library/mt652510.aspx)
* [Uue AzureRmRoleAssignment](https://msdn.microsoft.com/library/mt678995.aspx)


*PowerShelli skripti*  

```PowerShell


###########################################
# Set Values
###########################################

# If running manually, uncomment before the first 
# execution to login to the Azure Portal:

# Add-AzureRmAccount

# Set the name of the Application Insights Resource

$appInsightsName = "TestApp"

# Set the application name used for the value of the Tag "AppInsightsApp" 
# - http://azure.microsoft.com/documentation/articles/azure-preview-portal-using-tags/
$applicationTagName = "MyApp"

# Set the name of the Resource Group to use.  
# Default is the application name.
$resourceGroupName = "MyAppResourceGroup"

###################################################
# Create the Resource and Output the name and iKey
###################################################

#Select the azure subscription
Select-AzureSubscription -SubscriptionName "MySubscription"

# Create the App Insights Resource

$resource = New-AzureRmResource `
  -ResourceName $appInsightsName `
  -ResourceGroupName $resourceGroupName `
  -Tag @{ Name = "AppInsightsApp"; Value = $applicationTagName} `
  -ResourceType "Microsoft.Insights/Components" `
  -Location "Central US" `
  -PropertyObject @{"Type"="ASP.NET"} `
  -Force

# Give owner access to the team

New-AzureRmRoleAssignment `
  -SignInName "myteam@fabrikam.com" `
  -RoleDefinitionName Owner `
  -Scope $resource.ResourceId 


#Display iKey
Write-Host "App Insights Name = " $resource.Name
Write-Host "IKey = " $resource.Properties.InstrumentationKey

```

## <a name="what-to-do-with-the-ikey"></a>Mida teha, et iKey

Iga ressursi on tähistatud instrumentation võtmed (iKey). Funktsiooni iKey on väljund skripti ressursside loomine. Koosta skripti peaks andma iKey et rakenduste ülevaateid SDK manustatud rakenduse.

On kaks võimalust selle iKey SDK kättesaadavaks teha.
  
* Klõpsake [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md): 
 * `<instrumentationkey>`*iKey*`</instrumentationkey>`
* Või -koodi [lähtestamine](app-insights-api-custom-events-metrics.md): 
 * `Microsoft.ApplicationInsights.Extensibility.
    TelemetryConfiguration.Active.InstrumentationKey = "`*iKey*`";`



## <a name="see-also"></a>Vt ka

* [Saate luua rakenduse ülevaated ja veebiressursid test](app-insights-powershell.md)
* [Azure'i diagnostika PowerShelliga jälgimine häälestamine](app-insights-powershell-azure-diagnostics.md) 
* [Teatiste seadmine PowerShelli abil](app-insights-powershell-alerts.md)

 