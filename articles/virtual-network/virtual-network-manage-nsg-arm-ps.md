<properties 
   pageTitle="PowerShelli kaudu sisse ressursihaldur NSGs haldamine | Microsoft Azure'i"
   description="Saate teada, kuidas hallata exising NSGs sisse ressursihaldur PowerShelli kaudu"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/14/2016"
   ms.author="jdial" />

# <a name="manage-nsgs-using-powershell"></a>PowerShelli kasutamine NSGs haldamine

[AZURE.INCLUDE [virtual-network-manage-arm-selectors-include.md](../../includes/virtual-network-manage-nsg-arm-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-manage-nsg-intro-include.md](../../includes/virtual-network-manage-nsg-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)]klassikaline juurutamise mudel.

[AZURE.INCLUDE [virtual-network-manage-nsg-arm-scenario-include.md](../../includes/virtual-network-manage-nsg-arm-scenario-include.md)]

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="retrieve-information"></a>Teabe toomiseks

Saate vaadata oma olemasoleva NSGs, toomine reeglid mõne olemasoleva NSG ja teada saada, millised ressursid on NSG on seostatud.

### <a name="view-existing-nsgs"></a>Saate vaadata olemasolevaid NSGs
Tellimuse kõik olemasolevad NSGs vaatamiseks käivitage selle `Get-AzureRmNetworkSecurityGroup` cmdlet-käsk nagu allpool näidatud.

    Get-AzureRmNetworkSecurityGroup

Oodatav tulemus:

    Name                 : NSG-BackEnd
    ResourceGroupName    : RG-NSG
    Location             : westus
    Id                   : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/
                           Microsoft.Network/networkSecurityGroups/NSG-BackEnd
    Etag                 : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ResourceGuid         : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
    ProvisioningState    : Succeeded
    Tags                 :                         
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]
    
    Name                 : NSG-FrontEnd
    ResourceGroupName    : RG-NSG
    Location             : eastus
    Id                   : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/NRP-RG/providers/
                           Microsoft.Network/networkSecurityGroups/NSG-FrontEnd
    Etag                 : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ResourceGuid         : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
    ProvisioningState    : Succeeded
    Tags                 : 
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]
                            
    Name                 : WEB1
    ResourceGroupName    : RG101
    Location             : eastus2
    Id                   : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG101/providers/M
                           icrosoft.Network/networkSecurityGroups/WEB1
    Etag                 : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ResourceGuid         : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
    ProvisioningState    : Succeeded
    Tags                 : 
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]


Mõne kindla ressursirühma NSGs loendi vaatamiseks käivitage selle `Get-AzureRmNetworkSecurityGroup` cmdlet-käsk nagu allpool näidatud. 

    Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG

Oodatav väljund:

    Name                 : NSG-BackEnd
    ResourceGroupName    : RG-NSG
    Location             : westus
    Id                   : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/
                           Microsoft.Network/networkSecurityGroups/NSG-BackEnd
    Etag                 : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ResourceGuid         : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
    ProvisioningState    : Succeeded
    Tags                 :                         
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]
    
    Name                 : NSG-FrontEnd
    ResourceGroupName    : RG-NSG
    Location             : eastus
    Id                   : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/NRP-RG/providers/
                           Microsoft.Network/networkSecurityGroups/NSG-FrontEnd
    Etag                 : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ResourceGuid         : xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
    ProvisioningState    : Succeeded
    Tags                 : 
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]
         
### <a name="list-all-rules-for-an-nsg"></a>Kõik reeglite loendi jaoks soovitud NSG

Mõne NSG nimega **NSG-FrontEnd**reegleid kuvamiseks käivitage selle `Get-AzureRmNetworkSecurityGroup` cmdlet-käsk nagu allpool näidatud. 

    Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd | Select SecurityRules -ExpandProperty SecurityRules

Oodatav väljund:
    
    Name                     : rdp-rule
    Id                       : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/                        Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/rdp-rule
    Etag                     : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ProvisioningState        : Succeeded
    Description              : Allow RDP
    Protocol                 : Tcp
    SourcePortRange          : *
    DestinationPortRange     : 3389
    SourceAddressPrefix      : Internet
    DestinationAddressPrefix : *
    Access                   : Allow
    Priority                 : 100
    Direction                : Inbound
    
    Name                     : web-rule
    Id                       : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/                        Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/web-rule
    Etag                     : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    ProvisioningState        : Succeeded
    Description              : Allow HTTP
    Protocol                 : Tcp
    SourcePortRange          : *
    DestinationPortRange     : 80
    SourceAddressPrefix      : Internet
    DestinationAddressPrefix : *
    Access                   : Allow
    Priority                 : 101
    Direction                : Inbound

>[AZURE.NOTE] Saate kasutada ka `Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name "NSG-FrontEnd" | Select DefaultSecurityRules -ExpandProperty DefaultSecurityRules` **NSG-FrontEnd** NSG vaikimisi reeglite loendi.

### <a name="view-nsgs-associations"></a>Vaate NSGs ühendused

Ressursid on **NSG-FrontEnd** NSG seostada kuvamiseks käivitage selle `Get-AzureRmNetworkSecurityGroup` cmdlet-käsk nagu allpool näidatud.

    Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd

Otsige **NetworkInterfaces** ja **alamvõrku** atribuudid, nagu allpool näidatud:

    NetworkInterfaces    : []
    Subnets              : [
                             {
                               "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                               "IpConfigurations": []
                             }
                           ]

Ülaltoodud näites on NSG pole seotud mis tahes võrgu liidesed (NICs) ja see on seostatud mõne alamvõrgu nimega **FrontEnd**.

## <a name="manage-rules"></a>Reeglite haldamiseks

Saate lisada mõne olemasoleva NSG reegleid, olemasolevad reeglite redigeerimiseks ja Eemalda reeglid.

### <a name="add-a-rule"></a>Reegli lisamine

Lubada **Sissetulev** liiklus pordi **443** kaudu mis tahes seadme abil **NSG-FrontEnd** NSG reegli lisamiseks järgige alltoodud juhiseid.

1. Käivitage selle `Get-AzureRmNetworkSecurityGroup` cmdlet-käsk tuua olemasoleva NSG ja salvestada selle muutuja, nagu allpool näidatud.

        $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG `
            -Name NSG-FrontEnd

2. Käivitage selle `Add-AzureRmNetworkSecurityRuleConfig` cmdlet-käsk, nagu allpool näidatud.

        Add-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg `
            -Name https-rule `
            -Description "Allow HTTPS" `
            -Access Allow `
            -Protocol Tcp `
            -Direction Inbound `
            -Priority 102 `
            -SourceAddressPrefix * `
            -SourcePortRange * `
            -DestinationAddressPrefix * `
            -DestinationPortRange 443

3. Salvestada selle NSG tehtud muudatusi, käivitage selle `Set-AzureRmNetworkSecurityGroup` cmdlet-käsk nagu allpool näidatud.

        Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg

    Oodatav väljund nähtaval ainult turvalisus reegleid:

        Name                 : NSG-FrontEnd
        ...
        SecurityRules        : [
                                 {
                                   "Name": "rdp-rule",
                                   ...
                                 },
                                 {
                                   "Name": "web-rule",
                                   ...
                                 },
                                 {
                                   "Name": "https-rule",
                                   "Etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                                   "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/https-rule",
                                   "Description": "Allow HTTPS",
                                   "Protocol": "Tcp",
                                   "SourcePortRange": "*",
                                   "DestinationPortRange": "443",
                                   "SourceAddressPrefix": "*",
                                   "DestinationAddressPrefix": "*",
                                   "Access": "Allow",
                                   "Priority": 102,
                                   "Direction": "Inbound",
                                   "ProvisioningState": "Succeeded"
                                 }
                               ]

### <a name="change-a-rule"></a>Reegli muutmine

Sissetuleva liikluse lubamiseks **Internet** ainult kohal loodud reegli muutmiseks tehke järgmist.

1. Käivitage selle `Get-AzureRmNetworkSecurityGroup` cmdlet-käsk tuua olemasoleva NSG ja salvestada selle muutuja, nagu allpool näidatud.

        $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG `
            -Name NSG-FrontEnd

2. Käivitage selle `Set-AzureRmNetworkSecurityRuleConfig` cmdlet-käsk, nagu allpool näidatud.

        Set-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg `
            -Name https-rule `
            -Description "Allow HTTPS" `
            -Access Allow `
            -Protocol Tcp `
            -Direction Inbound `
            -Priority 102 `
            -SourceAddressPrefix * `
            -SourcePortRange Internet `
            -DestinationAddressPrefix * `
            -DestinationPortRange 443

3. Salvestada selle NSG tehtud muudatusi, käivitage selle `Set-AzureRmNetworkSecurityGroup` cmdlet-käsk nagu allpool näidatud.

        Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg

    Oodatav väljund nähtaval ainult turvalisus reegleid:

        Name                 : NSG-FrontEnd
        ...
        SecurityRules        : [
                                 {
                                   "Name": "rdp-rule",
                                   ...
                                 },
                                 {
                                   "Name": "web-rule",
                                   ...
                                 },
                                 {
                                   "Name": "https-rule",
                                   "Etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                                   "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/https-rule",
                                   "Description": "Allow HTTPS",
                                   "Protocol": "Tcp",
                                   "SourcePortRange": "*",
                                   "DestinationPortRange": "443",
                                   "SourceAddressPrefix": "Internet",
                                   "DestinationAddressPrefix": "*",
                                   "Access": "Allow",
                                   "Priority": 102,
                                   "Direction": "Inbound",
                                   "ProvisioningState": "Succeeded"
                                 }
                               ]

### <a name="delete-a-rule"></a>Reegli kustutamine

1. Käivitage selle `Get-AzureRmNetworkSecurityGroup` cmdlet-käsk tuua olemasoleva NSG ja salvestada selle muutuja, nagu allpool näidatud.

        $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG `
            -Name NSG-FrontEnd

2. Käivitage selle `Remove-AzureRmNetworkSecurityRuleConfig` cmdlet-käsk, nagu allpool näidatud.

        Remove-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg `
            -Name https-rule

3. Salvestada selle NSG tehtud muudatusi, käivitage selle `Set-AzureRmNetworkSecurityGroup` cmdlet-käsk nagu allpool näidatud.

        Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg

    Oodatav väljund nähtaval ainult turvalisuse reeglid **https-reegel** enam kuvatakse teade:

        Name                 : NSG-FrontEnd
        ...
        SecurityRules        : [
                                 {
                                   "Name": "rdp-rule",
                                   ...
                                 },
                                 {
                                   "Name": "web-rule",
                                   ...
                                 }
                               ]

## <a name="manage-associations"></a>Ühenduste haldamine

Saate seostada ka NSG, et alamvõrku ja NICs. Samuti saate lagunevad mõne NSG mis tahes on seotud ressursside kaudu.

### <a name="associate-an-nsg-to-a-nic"></a>Seostada ka NSG on NIC

Seostada **NSG-FrontEnd** NSG **TestNICWeb1** NIC abil, järgige alltoodud juhiseid.

1. Käivitage selle `Get-AzureRmNetworkSecurityGroup` cmdlet-käsk tuua olemasoleva NSG ja salvestada selle muutuja, nagu allpool näidatud.

        $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG `
            -Name NSG-FrontEnd

2. Käivitage selle `Get-AzureRmNetworkInterface` cmdlet-käsk tuua olemasoleva NIC ja salvestada selle muutuja, nagu allpool näidatud.

        $nic = Get-AzureRmNetworkInterface -ResourceGroupName RG-NSG `
            -Name TestNICWeb1

3. Atribuudi **NetworkSecurityGroup** **NIC** muutuja väärtusele **NSG** muutuja, nagu allpool näidatud.

        $nic.NetworkSecurityGroup = $nsg

4. Salvestamine NIC tehtud muudatusi, käivitage selle `Set-AzureRmNetworkInterface` cmdlet-käsk nagu allpool näidatud.

        Set-AzureRmNetworkInterface -NetworkInterface $nic

    Oodatav väljund nähtaval ainult atribuudi **NetworkSecurityGroup** :

        NetworkSecurityGroup : {
                                 "SecurityRules": [],
                                 "DefaultSecurityRules": [],
                                 "NetworkInterfaces": [],
                                 "Subnets": [],
                                 "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd"
                               }

### <a name="dissociate-an-nsg-from-a-nic"></a>Lagunevad mõne NSG Võrguadapter:

Vaadelda eraldi **NSG-FrontEnd** NSG **TestNICWeb1** NIC kaudu, järgige alltoodud juhiseid.

1. Käivitage selle `Get-AzureRmNetworkSecurityGroup` cmdlet-käsk tuua olemasoleva NSG ja salvestada selle muutuja, nagu allpool näidatud.

        $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG `
            -Name NSG-FrontEnd

2. Käivitage selle `Get-AzureRmNetworkInterface` cmdlet-käsk tuua olemasoleva NIC ja salvestada selle muutuja, nagu allpool näidatud.

        $nic = Get-AzureRmNetworkInterface -ResourceGroupName RG-NSG `
            -Name TestNICWeb1

3. Atribuudi **NetworkSecurityGroup** **NIC** muutuja väärtuseks **$null**, nagu allpool näidatud.

        $nic.NetworkSecurityGroup = $null

4. Salvestamine NIC tehtud muudatusi, käivitage selle `Set-AzureRmNetworkInterface` cmdlet-käsk nagu allpool näidatud.

        Set-AzureRmNetworkInterface -NetworkInterface $nic

    Oodatav väljund nähtaval ainult atribuudi **NetworkSecurityGroup** :

        NetworkSecurityGroup : null

### <a name="dissociate-an-nsg-from-a-subnet"></a>Lagunevad mõne NSG on alamvõrgu kaudu

Vaadelda eraldi **NSG-FrontEnd** NSG **FrontEnd** alamvõrgu kaudu, järgige alltoodud juhiseid.

1. Käivitage selle `Get-AzureRmVirtualNetwork` cmdlet-käsk tuua olemasoleva VNet ja salvestada selle muutuja, nagu allpool näidatud.

        $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName RG-NSG `
            -Name TestVNet

2. Käivitage selle `Get-AzureRmVirtualNetworkSubnetConfig` cmdlet-käsk tuua **FrontEnd** alamvõrgu ja salvestada selle muutuja, nagu allpool näidatud.

        $subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet `
            -Name FrontEnd 

3. Atribuudi **NetworkSecurityGroup** **alamvõrgu** muutuja väärtuseks **$null**, nagu allpool näidatud.

        $subnet.NetworkSecurityGroup = $null

4. Salvestamine alamvõrgu tehtud muudatusi, käivitage selle `Set-AzureRmVirtualNetwork` cmdlet-käsk nagu allpool näidatud.

        Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

    Oodatud väljundi nähtaval ainult atribuutide **FrontEnd** alamvõrgu. Pange tähele, ei saa atribuudi **NetworkSecurityGroup**.

            ...
            Subnets           : [
                                  {
                                    "Name": "FrontEnd",
                                    "Etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                                    "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                                    "AddressPrefix": "192.168.1.0/24",
                                    "IpConfigurations": [
                                      {
                                        "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb2/ipConfigurations/ipconfig1"
                                      },
                                      {
                                        "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb1/ipConfigurations/ipconfig1"
                                      }
                                    ],
                                    "ProvisioningState": "Succeeded"
                                  },
                                    ...
                                ]

### <a name="associate-an-nsg-to-a-subnet"></a>Mõne NSG on alamvõrgu seostada.

Seostada **NSG-FrontEnd** NSG **FronEnd** alamvõrgu uuesti, järgige alltoodud juhiseid.

1. Käivitage selle `Get-AzureRmVirtualNetwork` cmdlet-käsk tuua olemasoleva VNet ja salvestada selle muutuja, nagu allpool näidatud.

        $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName RG-NSG `
            -Name TestVNet

2. Käivitage selle `Get-AzureRmVirtualNetworkSubnetConfig` cmdlet-käsk tuua **FrontEnd** alamvõrgu ja salvestada selle muutuja, nagu allpool näidatud.

        $subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet `
            -Name FrontEnd 

1. Käivitage selle `Get-AzureRmNetworkSecurityGroup` cmdlet-käsk tuua olemasoleva NSG ja salvestada selle muutuja, nagu allpool näidatud.

        $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG `
            -Name NSG-FrontEnd

3. Atribuudi **NetworkSecurityGroup** **alamvõrgu** muutuja väärtuseks **$null**, nagu allpool näidatud.

        $subnet.NetworkSecurityGroup = $nsg

4. Salvestamine alamvõrgu tehtud muudatusi, käivitage selle `Set-AzureRmVirtualNetwork` cmdlet-käsk nagu allpool näidatud.

        Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

    Oodatav väljund nähtaval ainult **NetworkSecurityGroup** atribuut **FrontEnd** alamvõrgu:

        ...
        "NetworkSecurityGroup": {
                                  "SecurityRules": [],
                                  "DefaultSecurityRules": [],
                                  "NetworkInterfaces": [],
                                  "Subnets": [],
                                  "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd"
                                }
        ...

## <a name="delete-an-nsg"></a>Mõne NSG kustutamine

Mõne NSG saate kustutada ainult siis, kui see on seotud, mis tahes ressursi. Mõne NSG kustutamiseks järgige alltoodud juhiseid.

1. Mõne NSG seotud ressursid käivitage selle `azure network nsg show` nagu on näidatud [Vaade NSGs rakendustega](#View-NSGs-associations).
2. Kui soovitud NSG on seostatud mis tahes NICs, käivitage selle `azure network nic set` nagu on näidatud [Dissociate on NSG kaudu Võrguadapter](#Dissociate-an-NSG-from-a-NIC) iga NIC. jaoks 
3. Kui soovitud NSG on seotud mis tahes alamvõrku, käivitage selle `azure network vnet subnet set` nagu on näidatud [Dissociate on NSG kaudu on alamvõrgu](#Dissociate-an-NSG-from-a-subnet) iga alamvõrgu jaoks.
4. Funktsiooni NSG kustutamiseks käivitage selle `Remove-AzureRmNetworkSecurityGroup` cmdlet-käsk nagu allpool näidatud.

        Remove-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd -Force

    >[AZURE.NOTE] Funktsiooni **-Jõusta** parameetri tagab, et te ei pea kustutamise kinnitamiseks.

## <a name="next-steps"></a>Järgmised sammud

- [Logimise](virtual-network-nsg-manage-log.md) NSGs.