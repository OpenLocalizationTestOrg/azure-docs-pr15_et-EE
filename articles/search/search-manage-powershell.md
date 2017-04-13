<properties 
    pageTitle="Azure'i Otsingu haldamine PowerShelli skriptide abil | Microsoft Azure'i | Majutatud pilveteenuses otsing" 
    description="Azure'i otsinguteenuse PowerShelli skriptide abil hallata. Luua või värskendada teenuse Azure otsingu ja hallata Azure otsingu haldus klahve" 
    services="search" 
    documentationCenter="" 
    authors="seansaleh" 
    manager="mblythe" 
    editor=""
    tags="azure-resource-manager"/>

<tags 
    ms.service="search" 
    ms.devlang="na" 
    ms.workload="search" 
    ms.topic="article" 
    ms.tgt_pltfrm="powershell" 
    ms.date="08/15/2016" 
    ms.author="seasa"/>

# <a name="manage-your-azure-search-service-with-powershell"></a>Azure'i otsinguteenuse PowerShelliga haldamine
> [AZURE.SELECTOR]
- [Portaal](search-manage.md)
- [PowerShelli](search-manage-powershell.md)
- [REST API-GA](search-get-started-management-api.md)

Selles teemas kirjeldatakse PowerShelli käske paljude haldamise toiminguid teha Azure'i otsingu teenuste jaoks. Me juhendab otsinguteenuse loomise, suuruse muutmist ja hallata oma API võtmed.
Need käsud samaaegselt [Azure'i Otsingu haldamine REST API](http://msdn.microsoft.com/library/dn832684.aspx)haldamise võimalusi.

## <a name="prerequisites"></a>Eeltingimused
 
- Azure'i PowerShelli 1.0 või suurem peab teil olema. Juhised leiate teemast [installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md).
- Te peate olema sisseloginud Azure tellimusele PowerShellis kirjeldatud allpool.

Esmalt peate sisselogimiseks Azure'i see käsk:

    Login-AzureRmAccount

Määrake Microsoft Azure'i sisselogimise dialoogiboksi meiliaadress Azure'i konto ja parooli.

Teise võimalusena saate [põhisumma teenuse mitte interaktiivseks Logi sisse](../resource-group-authenticate-service-principal.md).

Kui teil on mitu Azure tellimust, peate Azure tellimuse seadmine. Praeguse tellimuste loendi kuvamiseks käivitage see käsk.

    Get-AzureRmSubscription | sort SubscriptionName | Select SubscriptionName

Määramaks, et see tellimus, käivitage järgmine käsk. Järgmises näites on tellimuse nime `ContosoSubscription`.

    Select-AzureRmSubscription -SubscriptionName ContosoSubscription

## <a name="commands-to-help-you-get-started"></a>Käsud, mis aitavad teil alustada.

    $serviceName = "your-service-name-lowercase-with-dashes"
    $sku = "free" # or "basic" or "standard" for paid services
    $location = "West US"
    # You can get a list of potential locations with
    # (Get-AzureRmResourceProvider -ListAvailable | Where-Object {$_.ProviderNamespace -eq 'Microsoft.Search'}).Locations
    $resourceGroupName = "YourResourceGroup" 
    # If you don't already have this resource group, you can create it with 
    # New-AzureRmResourceGroup -Name $resourceGroupName -Location $location

    # Register the ARM provider idempotently. This must be done once per subscription
    Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.Search" -Force

    # Create a new search service
    # This command will return once the service is fully created
    New-AzureRmResourceGroupDeployment `
        -ResourceGroupName $resourceGroupName `
        -TemplateUri "https://gallery.azure.com/artifact/20151001/Microsoft.Search.1.0.9/DeploymentTemplates/searchServiceDefaultTemplate.json" `
        -NameFromTemplate $serviceName `
        -Sku $sku `
        -Location $location `
        -PartitionCount 1 `
        -ReplicaCount 1
    
    # Get information about your new service and store it in $resource
    $resource = Get-AzureRmResource `
        -ResourceType "Microsoft.Search/searchServices" `
        -ResourceGroupName $resourceGroupName `
        -ResourceName $serviceName `
        -ApiVersion 2015-08-19
    
    # View your resource
    $resource
    
    # Get the primary admin API key
    $primaryKey = (Invoke-AzureRmResourceAction `
        -Action listAdminKeys `
        -ResourceId $resource.ResourceId `
        -ApiVersion 2015-08-19).PrimaryKey

    # Regenerate the secondary admin API Key
    $secondaryKey = (Invoke-AzureRmResourceAction `
        -ResourceType "Microsoft.Search/searchServices/regenerateAdminKey" `
        -ResourceGroupName $resourceGroupName `
        -ResourceName $serviceName `
        -ApiVersion 2015-08-19 `
        -Action secondary).SecondaryKey

    # Create a query key for read only access to your indexes
    $queryKeyDescription = "query-key-created-from-powershell"
    $queryKey = (Invoke-AzureRmResourceAction `
        -ResourceType "Microsoft.Search/searchServices/createQueryKey" `
        -ResourceGroupName $resourceGroupName `
        -ResourceName $serviceName `
        -ApiVersion 2015-08-19 `
        -Action $queryKeyDescription).Key
    
    # View your query key
    $queryKey

    # Delete query key
    Remove-AzureRmResource `
        -ResourceType "Microsoft.Search/searchServices/deleteQueryKey/$($queryKey)" `
        -ResourceGroupName $resourceGroupName `
        -ResourceName $serviceName `
        -ApiVersion 2015-08-19
        
    # Scale your service up
    # Note that this will only work if you made a non "free" service
    # This command will not return until the operation is finished
    # It can take 15 minutes or more to provision the additional resources
    $resource.Properties.ReplicaCount = 2
    $resource | Set-AzureRmResource
    
    # Delete your service
    # Deleting your service will delete all indexes and data in the service
    $resource | Remove-AzureRmResource
    
## <a name="next-steps"></a>Järgmised sammud
    
Nüüd, kui teie teenus on loodud, saate järgmiste juhiste juurde: koostada [register](search-what-is-an-index.md), [päringu registri](search-query-overview.md)ja lõpuks luua ja hallata oma otsingu rakendus, mis kasutab Azure otsing.

- [Azure'i portaalis Azure otsingu registri loomine](search-create-index-portal.md)

- [Päringu Azure'i otsinguregistrisse Otsi Exploreriga Azure'i portaalis](search-explorer.md)

- [Häälestamise indekseerija alla laadida andmeid muude teenustega](search-indexer-overview.md)

- [.NET Azure otsingu kasutamine](search-howto-dotnet-sdk.md)

- [Azure'i otsingu liiklust analüüs](search-traffic-analytics.md)
