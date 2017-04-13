<properties
    pageTitle="Azure'i jälgimise REST API kiirtutvustus | Microsoft Azure'i"
    description="Kuidas taotluste autentimiseks ja Azure jälgimise REST API kasutamine."
    authors="mcollier, rboucher"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="mcollier"/>

# <a name="azure-monitoring-rest-api-walkthrough"></a>Azure'i REST API kiirtutvustus jälgimine
Selles artiklis kirjeldatakse, kuidas autentida nii, et teie koodi saate kasutada [Microsoft Azure'i kuvari REST API viide](https://msdn.microsoft.com/library/azure/dn931943.aspx).         

Azure'i kuvari API võimaldab programmiliselt allalaadimiseks saadaval vaikimisi argumendil määratlused (tüüp väärtuseks meetermõõdustik nagu CPU aega, kutsed jne), granulaarsus ja argumendil väärtused. Kui tuua, andmeid saab salvestada eraldi andmesalve, nt Azure'i SQL-andmebaasi, DocumentDB või Azure'i andmed Lake. Seal täiendavad analüüsi saab teha vastavalt vajadusele.

Lisaks töötamine erinevate argumendil andmepunkte, nagu selles artiklis tutvustatakse, kuvari API võimaldab teatiste reegleid, vaade logid ja palju muud. Saadaval toimingute täieliku loendi leiate teemast [Microsoft Azure'i kuvari REST API viide](https://msdn.microsoft.com/library/azure/dn931943.aspx).

## <a name="authenticating-azure-monitor-requests"></a>Azure'i kuvari taotlusi autentimine

Esimese asjana taotluse kinnitamiseks.

Azure'i ressursihaldur autentimise mudeli kasutamine ülesandeid, mis on Azure kuvari API käivitada. Seetõttu kõik taotlused peavad olema kinnitatud Azure Active Directory (Azure AD). Üks võimalus autentida klientrakenduse on luua Azure AD teenuse põhilise ja toomiseks autentimine (JWT) luba. Järgmine näidis skript näitab loomine teenuse Azure AD põhisumma PowerShelli kaudu. Üksikasjalikumat walk-through, vaadake dokumentatsiooni [Azure PowerShelli põhisumma ressursid juurdepääsu teenuse kasutamise](../resource-group-authenticate-service-principal.md#authenticate-service-principal-with-password—powershell)kohta. Samuti on võimalik [luua teenuse põhimõtet Azure portaali kaudu](../resource-group-create-service-principal-portal.md).

```PowerShell
$subscriptionId = "{azure-subscription-id}"
$resourceGroupName = "{resource-group-name}"
$location = "centralus"

# Authenticate to a specific Azure subscription.
Login-AzureRmAccount -SubscriptionId $subscriptionId

# Password for the service principal
$pwd = "{service-principal-password}"

# Create a new Azure AD application
$azureAdApplication = New-AzureRmADApplication `
                        -DisplayName "My Azure Monitor" `
                        -HomePage "https://localhost/azure-monitor" `
                        -IdentifierUris "https://localhost/azure-monitor" `
                        -Password $pwd

# Create a new service principal associated with the designated application
New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId

# Assign Reader role to the newly created service principal
New-AzureRmRoleAssignment -RoleDefinitionName Reader `
                          -ServicePrincipalName $azureAdApplication.ApplicationId.Guid

```

Päringu Azure'i kuvari API, kasutada klientrakenduse varem loodud teenuse põhilise autentida. PowerShelli skripti järgmises näites on kujutatud üks võimalus, aitavad JWT autentimise Turbeloa [Active Directory Authentication Library](../active-directory/active-directory-authentication-libraries.md) (ADAL) abil. Luba JWT edastatakse osana HTTP autoriseerimine parameetri taotlusi Azure'i kuvari REST API-ga.

```PowerShell
$azureAdApplication = Get-AzureRmADApplication -IdentifierUri "https://localhost/azure-monitor"

$subscription = Get-AzureRmSubscription -SubscriptionId $subscriptionId

$clientId = $azureAdApplication.ApplicationId.Guid
$tenantId = $subscription.TenantId
$authUrl = "https://login.windows.net/${tenantId}"

$AuthContext = [Microsoft.IdentityModel.Clients.ActiveDirectory.AuthenticationContext]$authUrl
$cred = New-Object -TypeName Microsoft.IdentityModel.Clients.ActiveDirectory.ClientCredential -ArgumentList ($clientId, $pwd)
 
$result = $AuthContext.AcquireToken("https://management.core.windows.net/", $cred)

# Build an array of HTTP header values 
$authHeader = @{
'Content-Type'='application/json'
'Accept'='application/json'
'Authorization'=$result.CreateAuthorizationHeader()
}
```

Pärast autentimist häälestamise etapp on lõpule viidud, saate päringuid käivitada seejärel Azure'i kuvari REST API-ga. On kaks abistavat päringud.

1. Argumendil määratlused ressursi loend

2. Saate tuua argumendil väärtusi


## <a name="retrieve-metric-definitions"></a>Saate tuua argumendil määratlused
>[AZURE.NOTE] Kasutades Azure kuvari REST API argumendil määratlused toomiseks kasutada "2016-03-01" API versioon.

```PowerShell
$apiVersion = "2016-03-01"
$request = "https://management.azure.com/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/${resourceProviderNamespace}/${resourceType}/${resourceName}/providers/microsoft.insights/metricDefinitions?api-version=${apiVersion}"

Invoke-RestMethod -Uri $request `
                  -Headers $authHeader `
                  -Method Get `
                  -Verbose
```
Rakenduse Azure'i loogika näib argumendil määratlused umbes järgmine pilt:

![Alt "JSON vaade argumendil määratlus vastuse."](./media/monitoring-rest-api-walkthrough/available_metric_definitions_logic_app_json_response_clean.png)

Lisateabe saamiseks leiate [loendi argumendil määratlused ressursi Azure'i kuvari REST API](https://msdn.microsoft.com/library/azure/mt743621.aspx) dokumentatsioonist.

## <a name="retrieve-metric-values"></a>Saate tuua argumendil väärtusi
Kui saadaval argumendil määratlused on teada, on siis võimalik tuua argumendil seotud väärtused. Filtreerimise taotlused selle väärtuseks meetermõõdustik nimi "väärtus" (mitte "localizedValue") kasutada (nt tuua "CpuTime" ja "taotleb argumendil andmepunktide). Kui ühtegi filtrit on määratud, tagastatakse vaikimisi väärtuseks meetermõõdustik.

>[AZURE.NOTE] Argumendil väärtusi, kasutades Azure kuvari REST API toomiseks kasutada versiooni API "2016-06-01".

**Meetod**: hankimine

**Taotleda URI**: https://management.azure.com/subscriptions/_{tellimuse id}_/resourceGroups/_{Ressursi rühma nimi}_/providers/_{ressursi-pakkuja-nimeruum}_/_{Ressursi-tüüpi}_/_{ressursinimi}_/providers/microsoft.insights/metrics?$filter=_{filter}_ja api-versioon =_{apiVersion}_

Näiteks RunsSucceeded antud ajavahemiku ja 1 tund aega tera argumendil andmepunktide toomiseks taotluse oleks järgmiselt:

```PowerShell
$apiVersion = "2016-06-01"
$filter = "(name.value eq 'RunsSucceeded') and aggregationType eq 'Total' and startTime eq 2016-09-23 and endTime eq 2016-09-24 and timeGrain eq duration'PT1H'"
$request = "https://management.azure.com/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/${resourceProviderNamespace}/${resourceType}/${resourceName}/providers/microsoft.insights/metrics?`$filter=${filter}&api-version=${apiVersion}"
(Invoke-RestMethod -Uri $request `
                   -Headers $authHeader `
                   -Method Get `
                   -Verbose).Value | ConvertTo-Json
```

Tulem oleks sarnane jälgimise kuvatõmmis näide:

![Alt "JSON vastus nähtaval argumendil väärtuse Average vastuse kellaaeg"](./media/monitoring-rest-api-walkthrough/available_metrics_logic_app_json_response.png)

Tuua mitu andmeid või koondamine punkte, lisada argumendil määratlus nimed ja koondamine tüüpi filter, nagu näha Järgnevas näites:

```PowerShell
$apiVersion = "2016-06-01"
$filter = "(name.value eq 'ActionsCompleted' or name.value eq 'RunsSucceeded') and (aggregationType eq 'Total' or aggregationType eq 'Average') and startTime eq 2016-09-23T13:30:00Z and endTime eq 2016-09-23T14:30:00Z and timeGrain eq duration'PT1M'"
$request = "https://management.azure.com/subscriptions/${subscriptionId}/resourceGroups/${resourceGroupName}/providers/${resourceProviderNamespace}/${resourceType}/${resourceName}/providers/microsoft.insights/metrics?`$filter=${filter}&api-version=${apiVersion}"
(Invoke-RestMethod -Uri $request `
                   -Headers $authHeader `
                   -Method Get `
                   -Verbose).Value | ConvertTo-Json
```

### <a name="use-armclient"></a>Kasutage ARMClient
Asemel PowerShelli (nagu eespool näidatud), on kasutada [ARMClient](https://github.com/projectkudu/ARMClient) teie Windowsi arvutisse. ARMClient tegeleb automaatselt Azure AD autentimine (ja tulemuseks JWT luba). Järgmised toimingud liigendamine argumendil andmete toomine ARMClient kasutamine:

1. Installige [Chocolatey](https://chocolatey.org/) ja [ARMClient](https://github.com/projectkudu/ARMClient).

2. Tippige terminal aknas, _armclient.exe Logi sisse_. See palub teil Azure'i sisse logida.

3. Tippige _armclient GET [your_resource_id]/providers/microsoft.insights/metricdefinitions?api-version=2016-03-01_

4. Tippige _armclient GET [your_resource_id]/providers/microsoft.insights/metrics?api-version=2016-06-01_


![Alt "Kasutamine ARMClient töötada Azure jälgimis REST API"](./media/monitoring-rest-api-walkthrough/armclient_metricdefinitions.png)


## <a name="retrieve-the-resource-id"></a>Saate tuua ressursi ID-d
REST API kasutamine aitab tõesti mõista saadaval argumendil määratlused, granulaarsus ja seotud väärtused. See teave on kasulik, kui kasutades [Azure teegis](https://msdn.microsoft.com/library/azure/mt417623.aspx).

Eelmise koodi kasutatava ressursi ID on soovitud Azure ressursi täielik tee. Päringu Azure Web App, oleks näiteks ressursi ID:

*/subscriptions/{Subscription-ID}/resourceGroups/{Resource-Group-Name}/Providers/Microsoft.Web/sites/{Site-Name}/*

Järgmine loend sisaldab ressursi ID vormingud erinevate Azure ressursse mõned näited:

* **Asjade jaoturi** - /subscriptions/_{tellimuse id}_/resourceGroups/_{Ressursi rühma nimi}_/providers/Microsoft.Devices/IotHubs/_{asjade jaoturi nime}_

* **Elastne SQL-i Pool** - /subscriptions/_{tellimuse id}_/resourceGroups/_{Ressursi rühma nimi}_/providers/Microsoft.Sql/servers/_{rakenduskausta db}_/elasticpools/_{SQL-i kausta nimi}_

* **SQL-andmebaasi (v12)** - /subscriptions/_{tellimuse id}_/resourceGroups/_{Ressursi rühma nimi}_/providers/Microsoft.Sql/servers/_{serverinime}_/databases/_{andmebaasi nimi}_

* **Teenuse siini** - /subscriptions/_{tellimuse id}_/resourceGroups/_{Ressursi rühma nimi}_/providers/Microsoft.ServiceBus/_{nimeruumi}_/_{servicebus nimi}_

* **VM skaala komplektid** - /subscriptions/_{tellimuse id}_/resourceGroups/_{Ressursi rühma nimi}_/providers/Microsoft.Compute/virtualMachineScaleSets/_{vm nime}_

* **VMs** - /subscriptions/_{tellimuse id}_/resourceGroups/_{Ressursi rühma nimi}_/providers/Microsoft.Compute/virtualMachines/_{vm nime}_

* **Sündmuse jaoturi** - /subscriptions/_{tellimuse id}_/resourceGroups/_{Ressursi rühma nimi}_/providers/Microsoft.EventHub/namespaces/_{eventhub nimeruum}_


On alternatiivne lähenemisviisi toomine ressursi ID, sh Azure ressursi Explorerit, vaatamiseks soovitud ressursi Azure'i portaalis ning PowerShelli või Azure CLI kaudu.

### <a name="azure-resource-explorer"></a>Azure'i Resource Explorer
Ressursi ID leidmiseks soovitud ressursi ühe kasulik lähenemine on [Azure ressursi Exploreri](https://resources.azure.com) tööriista kasutamine. Liikuge soovitud ressurss ja Heitke pilk ID, nagu järgmises pildil näidatud:

![Alt "Azure Resource Explorer"](./media/monitoring-rest-api-walkthrough/azure_resource_explorer.png)

### <a name="azure-portal"></a>Azure'i portaal
Ressursi ID-d saab ka Azure portaalist. Selleks, liikuge soovitud ressurss ja seejärel nuppu Atribuudid. Ressursi ID kuvatakse atribuutide tera, nagu on näha järgmine pilt:

![Alt "Azure'i portaalis tera atribuudid kuvatakse ressursi ID"](./media/monitoring-rest-api-walkthrough/resourceid_azure_portal.png)

### <a name="azure-powershell"></a>Azure'i PowerShelli
Ressursi ID-d saab tuua ka Azure PowerShelli cmdlet-käskude abil. Azure'i veebirakenduse ressursside ID saamiseks käivitada cmdleti Get-AzureRmWebApp nagu järgmine pilt:

![Alt "PowerShelli kaudu saadud ressursi ID"](./media\monitoring-rest-api-walkthrough\resourceid_powershell.png)

### <a name="azure-cli"></a>Azure'i CLI
Ressursi ID abil Azure'i CLI toomiseks käsu "azure Web Appis Kuva", määrata selle '--json' valikut, nagu on näidatud järgmine pilt:

![Alt "PowerShelli kaudu saadud ressursi ID"](./media\monitoring-rest-api-walkthrough\resourceid_azurecli.png)

## <a name="retrieve-activity-log-data"></a>Tegevuste Logi andmete toomiseks
Lisaks argumendil määratlused ja seotud väärtuste töötamise, on võimalik tuua täiendavad huvitav ülevaateid, mis on seotud Azure ressursse. Näitena on võimalik päringu [tegevuse](https://msdn.microsoft.com/library/azure/dn931934.aspx) logiandmed. Järgmises näites näitab Azure'i kuvari REST API päringu tegevuse logiandmed teatud kindla kuupäevavahemiku Azure tellimuse abil:

```PowerShell
$apiVersion = "2014-04-01"
$filter = "eventTimestamp ge '2016-09-23' and eventTimestamp le '2016-09-24'and eventChannels eq 'Admin, Operation'"
$request = "https://management.azure.com/subscriptions/${subscriptionId}/providers/microsoft.insights/eventtypes/management/values?api-version=${apiVersion}&`$filter=${filter}"
(Invoke-RestMethod -Uri $request `
                   -Headers $authHeader `
                   -Method Get `
                   -Verbose).Value | ConvertTo-Json
```

## <a name="next-steps"></a>Järgmised sammud
* Vaadake üle [Ülevaade jälgimine](monitoring-overview.md).
* Saate vaadata [toetatud mõõdikute Azure'i monitoris](monitoring-supported-metrics.md).
* Lugege läbi [Microsoft Azure'i kuvari REST API viide](https://msdn.microsoft.com/library/azure/dn931943.aspx).
* Vaadake üle [Azure teegis](https://msdn.microsoft.com/library/azure/mt417623.aspx).
