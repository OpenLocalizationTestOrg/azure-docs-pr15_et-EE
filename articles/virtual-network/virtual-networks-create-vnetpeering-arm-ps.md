<properties
   pageTitle="Looge VNet silmitsemine PowerShelli cmdlet-käskude abil | Microsoft Azure'i"
   description="Saate teada, kuidas luua virtuaalse võrgu kaudu Azure portaali sisse ressursihaldur."
   services="virtual-network"
   documentationCenter=""
   authors="NarayanAnnamalai"
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
   ms.author="narayanannamalai; annahar"/>

# <a name="create-vnet-peering-using-powershell-cmdlets"></a>Looge VNet silmitsemine PowerShelli cmdlet-käskude kasutamine

[AZURE.INCLUDE [virtual-networks-create-vnet-selectors-arm-include](../../includes/virtual-networks-create-vnetpeering-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnetpeering-intro-include.md)]

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-basic-include](../../includes/virtual-networks-create-vnetpeering-scenario-basic-include.md)]

Lisamine PowerShelli abil silmitsemine VNet loomiseks järgige alltoodud juhiseid:

1. Kui te pole kunagi varem Azure PowerShelli, vaadake, [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md) ja järgige juhiseid kõik viis selleks Azure'i sisse logida ja valige oma tellimuse.

> [AZURE.NOTE] PowerShelli cmdleti haldamise VNet silmitsemine saadetakse [Azure PowerShelli 1,6.](http://www.powershellgallery.com/packages/Azure/1.6.0)

2. Lugege virtuaalse võrgu objektid.

        $vnet1 = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name vnet1
        $vnet2 = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name vnet2

3. Luua VNet silmitsemine, peate looma kaks lingid, üks iga suunas. Järgmist toimingut loob VNet silmitsemine link VNet1, et VNet2 esmalt:

        Add-AzureRmVirtualNetworkPeering -Name LinkToVNet2 -VirtualNetwork $vnet1 -RemoteVirtualNetworkId $vnet2.Id

    Väljund on kuvatud.

        Name            : LinkToVNet2
        Id: /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet1/virtualNetworkPeerings/LinkToVNet2
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName   : vnet101
        VirtualNetworkName  : vnet1
        PeeringState        : Initiated
        ProvisioningState   : Succeeded
        RemoteVirtualNetwork    : {
                                            "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet2"
                                        }
        AllowVirtualNetworkAccess   : True
        AllowForwardedTraffic   : False
        AllowGatewayTransit : False
        UseRemoteGateways   : False
        RemoteGateways      : null
        RemoteVirtualNetworkAddressSpace : null

4. Selles etapis tuleb luua lingi VNet silmitsemine VNet2 VNet1 abil:

        Add-AzureRmVirtualNetworkPeering -Name LinkToVNet1 -VirtualNetwork $vnet2 -RemoteVirtualNetworkId $vnet1.Id

    Väljund on kuvatud.

        Name            : LinkToVNet1
        Id              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet2/virtualNetworkPeerings/LinkToVNet1
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName   : vnet101
        VirtualNetworkName  : vnet2
        PeeringState        : Connected
        ProvisioningState   : Succeeded
        RemoteVirtualNetwork    : {
                                            "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet1"
                                        }
        AllowVirtualNetworkAccess   : True
        AllowForwardedTraffic   : False
        AllowGatewayTransit : False
        UseRemoteGateways   : False
        RemoteGateways      : null
        RemoteVirtualNetworkAddressSpace : null

5. Kui VNet silmitsemine link on loodud, kuvatakse allpool link olek.

        Get-AzureRmVirtualNetworkPeering -VirtualNetworkName vnet1 -ResourceGroupName vnet101 -Name linktovnet2

    Väljund on kuvatud.

        Name            : LinkToVNet2
        Id              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet1/virtualNetworkPeerings/LinkToVNet2
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName   : vnet101
        VirtualNetworkName  : vnet1
        PeeringState        : Connected
        ProvisioningState   : Succeeded
        RemoteVirtualNetwork    : {
                                             "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet2"
                                        }
        AllowVirtualNetworkAccess   : True
        AllowForwardedTraffic            : False
        AllowGatewayTransit              : False
        UseRemoteGateways                : False
        RemoteGateways                   : null
        RemoteVirtualNetworkAddressSpace : null

    On paar konfigureeritav atribuutide VNet silmitsemine.

  	|Suvand|Kirjeldus|Vaikimisi|
  	|:-----|:----------|:------|
  	|AllowVirtualNetworkAccess|Kas aadress omavahelistes VNet Virtual_network sildi lisada.|Jah|
  	|AllowForwardedTraffic|Liikluse piilus VNet pärit on kas aktsepteeritud või lähevad|Ei|
  	|AllowGatewayTransit|Võimaldab omavahelistes VNet VNet lüüsi kasutamiseks|Ei|
  	|UseRemoteGateways|Kasutage oma omavahelistes VNet lüüsi. Omavahelistes VNet peab olema konfigureeritud lüüsi ja AllowGatewayTransit valitud. Ei saa kasutada seda suvandit, kui teil on konfigureeritud lüüsi|Ei|

    Iga link VNet silmitsemine on ülaltoodud atribuutide kogum. Näiteks saate AllowVirtualNetworkAccess väärtuseks True, VNet silmitsemine link VNet1 VNet2 ja määrake selle väärtuseks Väär VNet silmitsemine lingi soovitud suunas pöörama.

        $LinktoVNet2 = Get-AzureRmVirtualNetworkPeering -VirtualNetworkName vnet1 -ResourceGroupName vnet101 -Name LinkToVNet2
        $LinktoVNet2.AllowForwardedTraffic = $true
        Set-AzureRmVirtualNetworkPeering -VirtualNetworkPeering $LinktoVNet2

    Saate käivitada Get-AzureRmVirtualNetworkPeering kontrollida atribuudi väärtus pärast muutmine. Alates väljund, saate vaadata AllowForwardedTraffic muutub Sea väärtuseks True pärast ülaltoodud cmdlet-käsud.

        Name            : LinkToVNet2
        Id          : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet1/virtualNetworkPeerings/LinkToVNet2
        Etag            : W/"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
        ResourceGroupName   : vnet101
        VirtualNetworkName  : vnet1
        PeeringState        : Connected
        ProvisioningState   : Succeeded
        RemoteVirtualNetwork    : {
                                            "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/vnet101/providers/Microsoft.Network/virtualNetworks/vnet2"
                                        }
        AllowVirtualNetworkAccess   : True
        AllowForwardedTraffic       : True
        AllowGatewayTransit     : False
        UseRemoteGateways       : False
        RemoteGateways      : null
        RemoteVirtualNetworkAddressSpace : null

    Pärast selle stsenaariumi, silmitsemine peaks olema võimalus algatada mis tahes virtuaalse masina virtual mis tahes seadme kaudu, nii VNets ühendused. Vaikimisi AllowVirtualNetworkAccess on True ja VNet silmitsemine on ettevalmistamise proper ACL VNets suhtlemine lubama. Siiski saate rakendada võrgu turvalisuse rühma (NSG) reeglid blokeerimiseks ei pruugi teatud alamvõrku või virtuaalmasinates saada peene struktuuriga kontrolli Accessi kahe virtuaalse võrgu vahel.  NSG reeglite loomise kohta lisateabe saamiseks vaadake selle [artikli](virtual-networks-create-nsg-arm-ps.md).

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-crosssub-include](../../includes/virtual-networks-create-vnetpeering-scenario-crosssub-include.md)]

PowerShelli kaudu tellimustes silmitsemine VNet loomiseks järgige alltoodud juhiseid:

1. Azure'i au sisse logida kasutaja A kasutaja konto tellimuse-A ja käivitage järgmine cmdlet-käsk:

        New-AzureRmRoleAssignment -SignInName <UserB ID> -RoleDefinitionName "Network Contributor" -Scope /subscriptions/<Subscription-A-ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetworks/VNet5

    On see pole kohustuslik, silmitsemine saab kindlaks isegi siis, kui kasutajad eraldi tõsta silmitsemine taotlusi nende vastavate VNets kui taotlused vastavad. Muud VNet õigustega kasutaja lisamise kohaliku VNet kasutajana on hõlpsam teha häälestamine.

2. Logige sisse kontoga õigustega kasutaja-B Azure tellimuse-b ja käivitage järgmine cmdlet-käsk:

        New-AzureRmRoleAssignment -SignInName <UserA ID> -RoleDefinitionName "Network Contributor" -Scope /subscriptions/<Subscription-B-ID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/VirtualNetworks/VNet3

3. Klõpsake kasutaja A kasutaja sisselogimise seansiga, käivitage järgmised cmdlet-käsk:

        $vnet3 = Get-AzureRmVirtualNetwork -ResourceGroupName hr-vnets -Name vnet3

        Add-AzureRmVirtualNetworkPeering -Name LinkToVNet5 -VirtualNetwork $vnet3 -RemoteVirtualNetworkId "/subscriptions/<Subscription-B-Id>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/virtualNetworks/VNet5" -BlockVirtualNetworkAccess

4. Kasutaja B sisselogimise seansi, käivitage järgmised cmdlet-käsk:

        $vnet5 = Get-AzureRmVirtualNetwork -ResourceGroupName vendor-vnets -Name vnet5

        Add-AzureRmVirtualNetworkPeering -Name LinkToVNet3 -VirtualNetwork $vnet5 -RemoteVirtualNetworkId "/subscriptions/<Subscriptoin-A-Id>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Network/virtualNetworks/VNet3" -BlockVirtualNetworkAccess

5. Pärast silmitsemine on loodud, mis tahes virtuaalse masina VNet3 tuleks mis tahes virtuaalse masina VNet5 suhelda.

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-transit-include](../../includes/virtual-networks-create-vnetpeering-scenario-transit-include.md)]

1. Selle stsenaariumi korral käivitage PowerShelli cmdlet-käskude luua VNet silmitsemine allpool.  Peate AllowForwardedTraffic atribuudi väärtuseks True ja HubVNet, mis võimaldab sissetulev liiklus väljaspool silmitsemine VNet aadressiruumi VNET1 linkida.

        $hubVNet = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name HubVNet
        $vnet1 = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name vnet1

        Add-AzureRmVirtualNetworkPeering -Name LinkToHub -VirtualNetwork $vnet1 -RemoteVirtualNetworkId $HubVNet.Id -AllowForwardedTraffic

        Add-AzureRmVirtualNetworkPeering -Name LinkToVNet1 -VirtualNetwork $HubVNet -RemoteVirtualNetworkId $vnet1.Id

2. Pärast silmitsemine on loodud, saate vaadake käesoleva [artikli](virtual-network-create-udr-arm-ps.md) ja määratleda kasutaja määratletud protsessi (UDR) ümbersuunamiseks VNet1 liikluse kaudu virtuaalse seadme oma võimaluste kasutamiseks. Kui määrate protsessi järgmise sõnumihüppe kohta, pärast aadressi, saate omavahelistes VNet HubVNet virtuaalse seadme IP-aadress. Allpool on näide:

        $route = New-AzureRmRouteConfig -Name TestNVA -AddressPrefix 10.3.0.0/16 -NextHopType VirtualAppliance -NextHopIpAddress 192.0.1.5

        $routeTable = New-AzureRmRouteTable -ResourceGroupName VNet101 -Location brazilsouth -Name TestRT -Route $route

        $vnet1 = Get-AzureRmVirtualNetwork -ResourceGroupName VNet101 -Name VNet1

        Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet1 -Name subnet-1 -AddressPrefix 10.1.1.0/24 -RouteTable $routeTable

        Set-AzureRmVirtualNetwork -VirtualNetwork $vnet1

[AZURE.INCLUDE [virtual-networks-create-vnet-scenario-asmtoarm-include](../../includes/virtual-networks-create-vnetpeering-scenario-asmtoarm-include.md)]

Mõne VNet silmitsemine klassikaline virtuaalse võrgu ja Azure ressursihaldur virtuaalse võrgu PowerShellis loomiseks tehke järgmist:

1. Virtuaalse võrgu objekti **VNET1**, Azure ressursihaldur virtuaalse võrgu jaoks on järgmine:

        $vnet1 = Get-AzureRmVirtualNetwork -ResourceGroupName vnet101 -Name vnet1

2. Selle stsenaariumi silmitsemine VNet loomiseks on vajalik ainult üks link, spetsiaalselt lingi kaudu **VNET1** **VNET2**. Selle toimingu jaoks on vaja teada klassikaline VNet ressursi ID-ga. Ressursi rühma ID vorming näeb välja selline:

        /subscriptions/{SubscriptionID}/resourceGroups/{ResourceGroupName}/providers/Microsoft.ClassicNetwork/virtualNetworks/{VirtualNetworkName}

    Kindlasti SubscriptionID, ResourceGroupName ja VirtualNetworkName asendada vastav nimedega.

    Seda saab teha järgmiselt:

        Add-AzureRmVirtualNetworkPeering -Name LinkToVNet2 -VirtualNetwork $vnet1 -RemoteVirtualNetworkId /subscriptions/xxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxx/resourceGroups/MyResourceGroup/providers/Microsoft.ClassicNetwork/virtualNetworks/VNET2

3. Üks kord VNet, luuakse silmitsemine link, näete lingi oleku nagu on näidatud allpool väljund:

        Name                             : LinkToVNet2
        Id                               : /subscriptions/xxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxx/resourceGroups/MyResourceGroup/providers/Microsoft.Network/virtualNetworks/VNET1/virtualNetworkPeerings/LinkToVNet2
        Etag                             : W/"acecbd0f-766c-46be-aa7e-d03e41c46b16"
        ResourceGroupName                : MyResourceGroup
        VirtualNetworkName               : VNET1
        PeeringState                     : Connected
        ProvisioningState                : Succeeded
        RemoteVirtualNetwork             : {
                                         "Id": "/subscriptions/xxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxx/resourceGroups/MyResourceGroup/providers/Microsoft.ClassicNetwork/virtualNetworks/VNET2"
                                       }
        AllowVirtualNetworkAccess        : True
        AllowForwardedTraffic            : False
        AllowGatewayTransit              : False
        UseRemoteGateways                : False
        RemoteGateways                   : null
        RemoteVirtualNetworkAddressSpace : null

## <a name="remove-vnet-peering"></a>VNet silmitsemine eemaldamine

1.  VNet, silmitsemine eemaldamiseks peate käivitage järgmine cmdlet-käsk:

        Remove-AzureRmVirtualNetworkPeering  

        Remove both links, using the following commands:

        Remove-AzureRmVirtualNetworkPeering -ResourceGroupName vnet101 -VirtualNetworkName vnet1 -Name linktovnet2
        Remove-AzureRmVirtualNetworkPeering -ResourceGroupName vnet101 -VirtualNetworkName vnet1 -Name linktovnet2

2. Kui eemaldate mõne VNET silmitsemine ühe lingi, omavahelistes lingi oleku läheb lahti. Selles olekus ei saa uuesti luua lingi kuni omavahelistes link olek muutub algataja. Soovitame enne uuesti luua VNet silmitsemine eemaldada nii lingid.
