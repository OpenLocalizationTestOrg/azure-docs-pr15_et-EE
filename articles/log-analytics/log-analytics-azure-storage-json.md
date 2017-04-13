<properties
    pageTitle="Azure'i diagnostikalogid abil Log Analytics analüüsida | Microsoft Azure'i"
    description="Log Analytics saate lugeda logid Azure teenustepakkujate kirjutamine Azure diagnostikalogid Bloobivahemälu salvestusruumi JSON-vormingus."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>


# <a name="analyze-azure-diagnostic-logs-using-log-analytics"></a>Azure'i diagnostikalogid abil Log Analytics analüüs

Log Analytics saab koguda mis kirjutamine [diagnostikalogid Azure'i](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) bloobimälu JSON-vormingus Azure teenuse logid:

+ Automaatika (eelvaade)
+ Võtme Vault (eelvaade)
+ Rakenduste portaali (eelvaade)
+ Võrgu turberühma (eelvaade)

Järgmistes jaotistes teile PowerShelli abil.

+ Log Analytics koguda logid mäluruumi iga ressursi konfigureerimine  
+ Log Analytics lahendus Azure'i teenuse lubamine

Enne Log Analytics saate nende ressursside otsinguväli, peab olema lubatud Azure diagnostika. Kasutage funktsiooni `Set-AzureRmDiagnosticSetting` cmdlet logimise lubamiseks.

Viidata diagnostikalogimine lubamise kohta lisateavet järgmistest artiklitest:

+ [Võtme Vault](../key-vault/key-vault-logging.md)
+ [Rakenduse lüüsi](../application-gateway/application-gateway-diagnostics.md)
+ [Võrgu turberühm](../virtual-network/virtual-network-nsg-manage-log.md)

Dokumentide sisaldab ka teavet kohta:

+ Andmete kogumine tõrkeotsing
+ Andmete kogumine peatamine

## <a name="configure-log-analytics-to-collect-azure-diagnostic-logs"></a>Log Analytics kogumiseks Azure diagnostikalogid konfigureerimine

Nende teenuste logid koguda ja visualiseerida logid lahenduse lubamine toimub PowerShelli skriptide abil.

Järgmises näites lubab kõigi toetatud ressursid logimine

```
# update to be the storage account that logs will be written to. Storage account must be in the same region as the resource to monitor
# format is similar to "/subscriptions/ec11ca60-ab12-345e-678d-0ea07bbae25c/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount"
$storageAccountId = ""

$supportedResourceTypes = ("Microsoft.Automation/AutomationAccounts", "Microsoft.KeyVault/Vaults", "Microsoft.Network/NetworkSecurityGroups", "Microsoft.Network/ApplicationGateways")

# update location to match your storage account location
$resources = Get-AzureRmResource | where { $_.ResourceType -in $supportedResourceTypes -and $_.Location -eq "westus" }

foreach ($resource in $resources) {
    Set-AzureRmDiagnosticSetting -ResourceId $resource.ResourceId -StorageAccountId $storageAccountId -Enabled $true -RetentionEnabled $true -RetentionInDays 1
}
```


Mõned ressursid, on võimalik teha eelmise konfiguratsiooni juurde Azure portaali kaudu [Ülevaade, Azure'i diagnostikalogid](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-diagnostic-logs)kirjeldatud juhiseid.

## <a name="configure-log-analytics-to-collect-azure-diagnostic-logs"></a>Log Analytics kogumiseks Azure diagnostikalogid konfigureerimine

Meil on ette PowerShelli skripti moodul, mis ekspordib kaks cmdlet-käskude aidata konfigureerimise Log Analytics:

1. `Add-AzureDiagnosticsToLogAnalyticsUI`palub teil sisendi ja saab häälestada lihtsa konfiguratsioone
2. `Add-AzureDiagnosticsToLogAnalytics`suunab ressursside jälgimiseks sisendina ja seejärel konfigureerib Log Analytics  

### <a name="pre-requisites"></a>Eeltingimused

1. Azure'i PowerShelli versiooniga 1.0.8 või uuemat versiooni töö ülevaated cmdlet-käske.
  - [Kuidas installida ja konfigureerida Azure PowerShell](../powershell-install-configure.md)
  - Kontrollige oma versiooni cmdlet-käsud:`Import-Module AzureRM.OperationalInsights -MinimumVersion 1.0.8 `
2. Diagnostikalogimise on konfigureeritud Azure ressurss, mida soovite jälgida. Kasutage `Set-AzureRmDiagnosticSetting` või viidata [Kasutamine Log Analytics andmekogumise Azure storage kontodelt](log-analytics-azure-storage.md) diagnostika lubamise kohta.
3. [Log Analytics](https://portal.azure.com/#create/Microsoft.LogAnalyticsOMS) tööruumi  
4. AzureDiagnosticsAndLogAnalytics PowerShelli moodul
  - Galeriist PowerShelli mooduli [AzureDiagnosticsAndLogAnalytics](https://www.powershellgallery.com/packages/AzureDiagnosticsAndLogAnalytics/) allalaadimine

### <a name="option-1-run-the-interactive-configuration-scripts"></a>Variant 1: Interaktiivsed konfiguratsioon skripte

Avage PowerShelli ja käivitada.

```
# Connect to Azure
Login-AzureRmAccount
# If you have diagnostics logs being written to classic storage you will also need to run
Add-AzureAccount

# Import the module
Install-Module -Name AzureDiagnosticsAndLogAnalytics

# Run the UI configuration script
Add-AzureDiagnosticsToLogAnalyticsUI

```

On näidatud saadaolevate valikute loendit ja antud viip, et tehke oma valik.
Teil palutakse teha järgmisi valikuid:

+ Ressursi tüübid (Azure'i teenused) koguda logid
+ Ressursikeskuse eksemplari kogumiseks logid
+ Logige Analytics tööruumi teabe kogumiseks

Pärast selle skripti käivitamine, peaksite nägema kirjete Logi Analytics 30 minuti pärast uue Diagnostikaandmete kirjutatakse salvestusruumi. Kui kirjeid pole saadaval, kui seekord vaadake allpool olevat jaotist tõrkeotsingu.

### <a name="option-2-build-a-list-of-resources-and-pass-them-to-the-configuration-cmdlet"></a>Variant 2: Ressursside loendi koostamiseks ja edastada cmdlet konfigureerimine

Saate koostada loendi ressursse, mis on Azure diagnostika lubatud ja siis edasi ressursside cmdlet-i konfigureerimine.

Näete käitades cmdlet-käsu kohta täiendavat teavet `Get-Help Add-AzureDiagnosticsToLogAnalytics`.


OMS [Log Analytics PowerShelli cmdlet-käskude](https://msdn.microsoft.com/library/mt188224.aspx) kohta lisateabe

>[AZURE.NOTE] Kui ressursi ja tööruum on erinevate Azure'i tellimused, vahetada neid vastavalt vajadusele abil`Select-AzureRmSubscription -SubscriptionId <Subscription the resource is in>`

```
# Connect to Azure
Login-AzureRmAccount
# If you have diagnostics logs being written to classic storage you will also need to run
Add-AzureAccount

# Import the module
Install-Module -Name AzureDiagnosticsAndLogAnalytics

# Update the values below with the name of the resource that has Azure diagnostics enabled and the name of your Log Analytics workspace
$resourceName = "***example-resource***"
$workspaceName = "***log-analytics-workspace***"

# Find the resource to monitor:
$resource = Find-AzureRmResource -ResourceType "Microsoft.KeyVault/Vaults" -ResourceNameContains $resourceName

# Find the Log Analytics workspace to configure, for example:
$workspace = Find-AzureRmResource -ResourceType "Microsoft.OperationalInsights/workspaces" -ResourceNameContains $workspaceName

# Perform configuration
Add-AzureDiagnosticsToLogAnalytics $resource $workspace

# Enable the Log Analytics solution
Set-AzureRmOperationalInsightsIntelligencePack -ResourceGroupName $workspace.ResourceGroupName -WorkspaceName $workspace.Name -intelligencepackname KeyVault -Enabled $true

```
Pärast selle skripti käivitamine, peaksite nägema kirjete Logi Analytics 30 minuti pärast uue Diagnostikaandmete kirjutatakse salvestusruumi. Kui kirjeid pole saadaval, kui seekord vaadake allpool olevat jaotist tõrkeotsingu.  

>[AZURE.NOTE] Konfiguratsiooni ei ole nähtav, Azure'i portaalis. Saate kontrollida konfigureerimise abil soovitud `Get-AzureRmOperationalInsightsStorageInsight` cmdlet-käsk.  


## <a name="stopping-log-analytics-from-collecting-azure-diagnostic-logs"></a>Log Analytics kaudu kogumise Azure diagnostikalogid peatamine

Ressursi Log Analytics konfiguratsiooni kustutamiseks kasutage funktsiooni `Remove-AzureRmOperationalInsightsStorageInsight` cmdlet-käsk.

## <a name="troubleshooting-configuration-for-azure-diagnostic-logs"></a>Azure'i diagnostikalogid tõrkeotsingu konfigureerimine

*Probleem*

Ressurss, mis on logida klassikaline salvestusruumi konfigureerimisel, kuvatakse järgmine tõrketeade:

```
Select-AzureSubscription : The subscription id 7691b0d1-e786-4757-857c-7360e61896c3 doesn't exist.

Parameter name: id
```

*Eraldusvõime*

Klassikaline juurutamise mudel API sisse logida`Add-AzureAccount`

## <a name="troubleshooting-data-collection-for-azure-diagnostic-logs"></a>Andmete kogumine Azure diagnostikalogid tõrkeotsing

Kui te ei näe oma Azure ressursi Log Analytics andmete, saate kasutada järgmisi tõrkeotsingujuhiseid:

+ Veenduge, et andmed liiguvad salvestusruumi kontole
+ Veenduge, et teenuse Logi Analytics lahendus on lubatud
+ Veenduge, et Logi Analytics on konfigureeritud lugemise salvestusruum
+ Salvestusruumi konto võti värskendamine

### <a name="verify-data-is-flowing-to-the-storage-account"></a>Veenduge, et andmete pääseb vabalt salvestusruumi kontole

Saate kontrollida, kui andmete pääseb vabalt salvestusruumi konto tööriista, mis võimaldab sirvimise Azure salvestusruumi (nt Visual Studio) või PowerShelli abil.

Leida salvestusruumi konto on konfigureeritud ressursi logimiseks kasutage järgmist PowerShelli:

`Find-AzureRmResource -ResourceType "Microsoft.KeyVault/Vaults" | select ResourceId | Get-AzureRmDiagnosticSetting `

Salvestusruumi container, mis kasutavad Azure'i diagnostika on nimi, formaat.  

`insights-logs-<Category>`

Vaadake lisateavet mäluruumi konto sisu kuvamine [salvestusruumi cmdlet-käskude abil kontrollida hiljutiste andmete ümbris](../storage/storage-powershell-guide-full.md) .

### <a name="verify-the-log-analytics-solution-for-the-service-is-enabled"></a>Veenduge, et teenuse Logi Analytics lahendus on lubatud

Kui kasutate `Add-AzureDiagnosticsToLogAnalyticsUI`, õige Log Analytics lahendus teie jaoks automaatselt lubatud.

Kui lahendus on lubatud, käivitage järgmine PowerShelli:

`Get-AzureRmOperationalInsightsIntelligencePacks -ResourceGroupName $logAnalyticsResourceGroup -WorkspaceName $logAnalyticsWorkspaceName`

Kui lahendus on lubatud, saate lubada järgmist PowerShelli abil.

`Set-AzureRmOperationalInsightsIntelligencePack -ResourceGroupName $logAnalyticsResourceGroup -WorkspaceName $logAnalyticsWorkspaceName -IntelligencePackName $solution -Enabled $true`

Lahendus lubamiseks iga ressursi jaoks nimi leidmiseks kasutage järgmist PowerShelli, (muutuja on saadaval, kui olete importinud mooduli):

`$MonitorableResourcesToOMSSolutions`

### <a name="verify-that-log-analytics-is-configured-to-read-from-storage"></a>Veenduge, et Logi Analytics on konfigureeritud lugemise salvestusruum

Kui lisate lisaressursid Azure, peate diagnostika neid logimise lubamine ja Log Analytics konfigureerimine.
Millised ressursid ja salvestusruumi kontod Log Analytics on konfigureeritud kogumiseks logide jaoks kasutage järgmist PowerShelli:

```
# Find the Workspace ResourceGroup and Name
$logAnalyticsWorkspace = Get-AzureRmOperationalInsightsWorkspace

#Get the configuration for all resources:
Get-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $logAnalyticsWorkspace.ResourceGroupName -WorkspaceName $logAnalyticsWorkspace.Name

#Get the just the resources configured:
Get-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $logAnalyticsWorkspace.ResourceGroupName -WorkspaceName $logAnalyticsWorkspace.Name | select Containers
```

### <a name="update-the-storage-key"></a>Salvestusruumi võti värskendamine

Salvestusruumi konto klahvi muutmisel Log Analytics konfiguratsiooni peab värskendatakse uue tootenumbri.
Saate värskendada Log Analytics konfiguratsiooni uue tootenumbri järgmist PowerShelli kaudu:

`Set-AzureRmOperationalInsightsStorageInsight -ResourceGroupName $logAnalyticsWorkspace.ResourceGroupName -WorkspaceName $logAnalyticsWorkspace.Name –Name <Storage Insight Name> -StorageAccountKey $newKey `

Salvestusruumi ülevaate nime leidmiseks kasutage funktsiooni `Get-AzureRmOperationalInsightsStorageInsight` cmdlet-käsk, nagu on näidatud varasemates näited.

## <a name="next-steps"></a>Järgmised sammud

- [Kasutamine bloobimälu IIS ja tabelimälu sündmuste](log-analytics-azure-storage-iis-table.md) logide lugemiseks Azure teenuste selle kirjutamine diagnostika tabelimälu või IIS-i logid Bloobivahemälu salvestusruumi kirjutada.
- [Luba lahenduste](log-analytics-add-solutions.md) annab ülevaate andmed.
- [Otsingu päringute kasutamine](log-analytics-log-searches.md) andmete analüüsimiseks.
