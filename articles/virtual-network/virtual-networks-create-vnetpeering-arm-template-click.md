<properties
   pageTitle="Looge VNet silmitsemine ressursihaldur mallide kasutamine | Microsoft Azure'i"
   description="Saate teada, kuidas luua virtuaalse võrgu silmitsemine väärtusi sisse ressursihaldur."
   services="virtual-network"
   documentationCenter=""
   authors="narayanannamalai"
   manager="jefco"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/14/2016"
   ms.author="narayanannamalai;annahar"/>

# <a name="create-vnet-peering-using-resource-manager-templates"></a>Looge VNet silmitsemine ressursihaldur mallide kasutamine

[AZURE.INCLUDE [virtual-networks-create-vnet-selectors-arm-include](../../includes/virtual-networks-create-vnetpeering-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnetpeering-intro-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-basic-include](../../includes/virtual-networks-create-vnetpeering-scenario-basic-include.md)]

Mõne VNet silmitsemine ressursihaldur mallide abil loomiseks järgige alltoodud juhiseid:

1. Kui te pole kunagi varem Azure PowerShelli, vaadake, [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md) ja järgige juhiseid kõik viis selleks Azure'i sisse logida ja valige oma tellimuse.

    > [AZURE.NOTE] PowerShelli cmdleti haldamise VNet silmitsemine saadetakse [Azure PowerShelli 1,6.](http://www.powershellgallery.com/packages/Azure/1.6.0)

2. Allpool olevat teksti kuvatakse definitsiooni VNet silmitsemine lingi VNet1 VNet2, ülaltoodud stsenaariumi. Kopeerige sisu alla ja salvestage see fail nimega VNetPeeringVNet1.json.

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet1/LinkToVNet2",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "[resourceId('Microsoft.Network/virtualNetworks', 'vnet2')]"       
        }
            }
            }
        ]
        }

3. Allpool olevat jaotist kuvatakse definitsiooni VNet silmitsemine lingi VNet2 VNet1, ülaltoodud stsenaariumi.  Kopeerige sisu alla ja salvestage see fail nimega VNetPeeringVNet2.json.

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet2/LinkToVNet1",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "[resourceId('Microsoft.Network/virtualNetworks', 'vnet1')]"       
                }
            }
            }
        ]
        }

    Nagu näha malli üle, on mõned konfigureeritav atribuutide VNet silmitsemine.

  	|Suvand|Kirjeldus|Vaikimisi|
  	|:-----|:----------|:------|
  	|AllowVirtualNetworkAccess|Kas on mõne partneri VNet aadressiruumi virtual_network sildi lisada.|Jah|
  	|AllowForwardedTraffic|Liikluse piilus VNet pärit on kas aktsepteeritud või lähevad.|Ei|
  	|AllowGatewayTransit|Võimaldab omavahelistes VNet VNet lüüsi kasutama.|Ei|
  	|UseRemoteGateways|Kasutage oma omavahelistes VNet lüüsi. Omavahelistes VNet peab olema konfigureeritud lüüsi ja AllowGatewayTransit valitud. Ei saa kasutada seda suvandit, kui teil on konfigureeritud lüüsi.|Ei|

    Iga link VNet silmitsemine on ülaltoodud atribuutide kogum. Näiteks saate AllowVirtualNetworkAccess väärtuseks True, VNet silmitsemine link VNet1 VNet2 ja määrake selle väärtuseks Väär VNet silmitsemine lingi soovitud suunas pöörama.


4. Mallifail juurutamiseks käivitada cmdleti New-AzureRmResourceGroupDeployment loomine või juurutamise värskendada. Ressursihaldur mallide kasutamise kohta lisateabe saamiseks vaadake selle [artikli](../resource-group-template-deploy.md).

        New-AzureRmResourceGroupDeployment -ResourceGroupName <resource group name> -TemplateFile <template file path> -DeploymentDebugLogLevel all

    > [AZURE.NOTE] Asendage rühma nimi ja malli ressursifaili vastavalt vajadusele.

    Allpool on näide aluseks ülaltoodud stsenaariumi:

        New-AzureRmResourceGroupDeployment -ResourceGroupName VNet101 -TemplateFile .\VNetPeeringVNet1.json -DeploymentDebugLogLevel all

    Väljund on kuvatud.

        DeploymentName      : VNetPeeringVNet1
        ResourceGroupName   : VNet101
        ProvisioningState       : Succeeded
        Timestamp           : 7/26/2016 9:05:03 AM
        Mode            : Incremental
        TemplateLink        :
        Parameters          :
        Outputs         :
        DeploymentDebugLogLevel : RequestContent, ResponseContent

        New-AzureRmResourceGroupDeployment -ResourceGroupName VNet101 -TemplateFile .\VNetPeeringVNet2.json -DeploymentDebugLogLevel all

    Väljund on kuvatud.

        DeploymentName      : VNetPeeringVNet2
        ResourceGroupName   : VNet101
        ProvisioningState       : Succeeded
        Timestamp           : 7/26/2016 9:07:22 AM
        Mode            : Incremental
        TemplateLink        :
        Parameters          :
        Outputs         :
        DeploymentDebugLogLevel : RequestContent, ResponseContent

5. Kui juurutamise on lõpule jõudnud, saate kasutada cmdlet-käsk silmitsemine oleku kuvamiseks.

        Get-AzureRmVirtualNetworkPeering -VirtualNetworkName VNet1 -ResourceGroupName VNet101 -Name linktoVNet2

    Väljund on kuvatud.

        Name            : LinkToVNet2
        Id              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/VNet101/providers/Microsoft.Network/virtualNetworks/VNet1/virtualNetworkPeerings/LinkToVNet2
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName   : VNet101
        VirtualNetworkName  : VNet1
        ProvisioningState       : Succeeded
        RemoteVirtualNetwork    : {
                                            "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/VNet101/providers/Microsoft.Network/virtualNetworks/VNet2"
                                        }
        AllowVirtualNetworkAccess   : True
        AllowForwardedTraffic            : False
        AllowGatewayTransit              : False
        UseRemoteGateways                : False
        RemoteGateways                   : null
        RemoteVirtualNetworkAddressSpace : null

    Pärast selle stsenaariumi, silmitsemine peaks oskama kontaktiga mõlemad VNets mis tahes virtuaalse masina virtual mis tahes arvutist ühendused. Vaikimisi AllowVirtualNetworkAccess on True ja VNet silmitsemine on ettevalmistamise proper ACL VNets suhtlemine lubama. Siiski saate rakendada võrgu turvalisuse rühma (NSG) reeglid blokeerimiseks ei pruugi teatud alamvõrku või virtuaalmasinates saada trahvi tera kontrolli Accessi kahe virtuaalse võrgu vahel.  NSG reegli loomise kohta lisateabe saamiseks vaadake selle [artikli](virtual-networks-create-nsg-arm-ps.md).

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-crosssub-include](../../includes/virtual-networks-create-vnetpeering-scenario-crosssub-include.md)]

Mõne VNet silmitsemine tellimustes loomiseks järgige alltoodud juhiseid:

1. Azure'i au sisse logida kasutaja A kontolt tellimuse-a ja käivitage järgmine cmdlet-käsk:

        New-AzureRmRoleAssignment -SignInName <UserB ID> -RoleDefinitionName "Network Contributor" -Scope /subscriptions/<Subscription-A-ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetwork/VNet5

    On see pole kohustuslik, silmitsemine saab kindlaks isegi siis, kui kasutajad eraldi tõsta silmitsemine taotlusi nende vastavate Vnets kui taotlused vastavad. Kohaliku VNet kasutajate lisamine muude VNet õigustega kasutaja on hõlpsam teha häälestamine.

2. Logige sisse kontoga õigustega kasutaja-B Azure tellimuse-b ja käivitage järgmine cmdlet-käsk:

        New-AzureRmRoleAssignment -SignInName <UserA ID> -RoleDefinitionName "Network Contributor" -Scope /subscriptions/<Subscription-B-ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetwork/VNet3

3. Klõpsake kasutaja A kasutaja sisselogimise seansiga, käivitage see cmdlet-käsk:

        New-AzureRmResourceGroupDeployment -ResourceGroupName VNet101 -TemplateFile .\VNetPeeringVNet3.json -DeploymentDebugLogLevel all

    Siin on, kuidas JSON-fail on määratletud.  

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet3/LinkToVNet5",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "/subscriptions/<Subscription-B-ID>/resourceGroups/<resource group name>/providers/Microsoft.Network/virtualNetworks/VNet5"
                }
            }
            }
        ]
        }

4. Kasutaja B sisselogimise seansi, käivitage järgmine cmdlet-käsk:

        New-AzureRmResourceGroupDeployment -ResourceGroupName VNet101 -TemplateFile .\VNetPeeringVNet5.json -DeploymentDebugLogLevel all

    Siin on, kuidas JSON-fail on määratletud.

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet5/LinkToVNet3",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "/subscriptions/Subscription-A-ID /resourceGroups/<resource group name>/providers/Microsoft.Network/virtualNetworks/VNet3"
                }
            }
            }
        ]
        }

    Pärast selle stsenaariumi, silmitsemine peaks olema võimalus algatada mis tahes virtuaalse masina virtual mis tahes seadme kaudu, nii VNets ühendused erinevates tellimustes.

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-transit-include](../../includes/virtual-networks-create-vnetpeering-scenario-transit-include.md)]

1. Selle stsenaariumi korral saate juurutada valimi malli loomiseks VNet silmitsemine allpool.  Peate AllowForwardedTraffic atribuudi väärtuseks True, mis võimaldab võrgu virtuaalse seadme piilus VNet saata ja vastu võtta liikluse.

    Siin on malli loomiseks on VNet silmitsemine kaudu HubVNet VNet1. Pange tähele, et AllowForwardedTraffic väärtuseks false.

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "HubVNet/LinkToVNet1",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": false,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "[resourceId('Microsoft.Network/virtualNetworks', 'vnet1')]"       
                }
            }
            }
            }
        ]
        }

2. Siin on malli loomiseks on VNet silmitsemine kaudu VNet1 HubVnet. Pange tähele, et AllowForwardedTraffic on seatud väärtusele tõene.

        {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        },
        "variables": {
        },
        "resources": [
            {
            "apiVersion": "2016-06-01",
            "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
            "name": "VNet1/LinkToHubVNet",
            "location": "[resourceGroup().location]",
            "properties": {
            "allowVirtualNetworkAccess": true,
            "allowForwardedTraffic": true,
            "allowGatewayTransit": false,
            "useRemoteGateways": false,
                "remoteVirtualNetwork": {
                "id": "[resourceId('Microsoft.Network/virtualNetworks', 'HubVnet')]"       
                }
            }
            }
        ]
        }


3. Pärast silmitsemine on loodud, leiate selle [artikli](virtual-network-create-udr-arm-ps.md) kasutaja määratletud routes(UDR) kaudu virtuaalse seadme oma võimaluste kasutamiseks VNet1 liikluse ümber määratleda. Kui määrate marsruutimiseks järgmise sõnumihüppe kohta, pärast aadressi, saate omavahelistes VNet HubVNet virtuaalse seadme IP-aadress.

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-asmtoarm-include](../../includes/virtual-networks-create-vnetpeering-scenario-asmtoarm-include.md)]

Erinevate juurutamise mudeli vahel virtuaalse silmitsemine loomiseks tehke järgmist:
1. Allpool olevat teksti kuvatakse definitsiooni VNet silmitsemine lingi VNET1 VNET2 selle stsenaariumi. Ainult üks link on vajalik vastastikune klassikaline virtuaalse võrgu Azure ressursi halduri virtuaalse võrku.

    Veenduge, et teie tellimuse ID, kus asub klassikaline virtuaalse võrgu või VNET2 jaoks luua ja muuta MyResouceGroup korral ressurss rühma nime.

    {"$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#", "contentVersion": "1.0.0.0", "parameetrid": {}, "muutujad": {}, "ressursid": [{"apiVersion": "2016-06-01", "tüüp": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings", "nimi": "VNET1/LinkToVNET2", "asukoht": "[resourceGroup () .location]", "atribuudid": {"allowVirtualNetworkAccess": true, "allowForwardedTraffic": väär, "allowGatewayTransit": väär, "useRemoteGateways": väär, "remoteVirtualNetwork": {"id": "[ResourceIdkasutamisel (' Microsoft.ClassicNetwork/virtualNetworks',"VNET2")]"}}}]}

2. Mallifail juurutada, käivitage järgmine cmdlet loomine või juurutamise värskendada.

        New-AzureRmResourceGroupDeployment -ResourceGroupName MyResourceGroup -TemplateFile .\VnetPeering.json -DeploymentDebugLogLevel all

        Output shows:

        DeploymentName          : VnetPeering
        ResourceGroupName       : MyResourceGroup
        ProvisioningState       : Succeeded
        Timestamp               : XX/XX/YYYY 5:42:33 PM
        Mode                    : Incremental
        TemplateLink            :
        Parameters              :
        Outputs                 :
        DeploymentDebugLogLevel : RequestContent, ResponseContent

3. Kui juurutamise on loodud, saate käivitage järgmine cmdlet-silmitsemine oleku kuvamiseks:

        Get-AzureRmVirtualNetworkPeering -VirtualNetworkName VNET1 -ResourceGroupName MyResourceGroup -Name LinkToVNET2

        Output shows:

        Name                             : LinkToVNET2
        Id                               : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/MyResource
                                   Group/providers/Microsoft.Network/virtualNetworks/VNET1/virtualNetworkPeering
                                   s/LinkToVNET2
        Etag                             : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName                : MyResourceGroup
        VirtualNetworkName               : VNET1
        PeeringState                     : Connected
        ProvisioningState                : Succeeded
        RemoteVirtualNetwork             : {
                                     "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/M
                                   yResourceGroup/providers/Microsoft.ClassicNetwork/virtualNetworks/VNET2"
                                   }
        AllowVirtualNetworkAccess        : True
        AllowForwardedTraffic            : False
        AllowGatewayTransit              : False
        UseRemoteGateways                : False
        RemoteGateways                   : null
        RemoteVirtualNetworkAddressSpace : null

Pärast klassikaline VNet ja ressursihaldur VNet vahel on loodud silmitsemine, peaks olema võimalus algatada ühendused: kõik virtuaalse masina VNET1 mis tahes virtuaalse masina VNET2 ja vastupidi.
