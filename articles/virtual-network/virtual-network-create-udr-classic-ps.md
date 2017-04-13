<properties 
   pageTitle="Määrata marsruutimist ja kasutada PowerShelli kaudu klassikaline juurutamise mudeli virtuaalne seadmed | Microsoft Azure'i"
   description="Siit saate teada, kuidas määrata marsruutimise VNets klassikaline juurutamise mudeli PowerShelli kaudu"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags  
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/02/2016"
   ms.author="jdial" />

#<a name="control-routing-and-use-virtual-appliances-classic-using-powershell"></a>Määrata marsruutimist ja kasutada virtuaalne seadmed (klassikaline) PowerShelli abil

[AZURE.INCLUDE [virtual-network-create-udr-classic-selectors-include.md](../../includes/virtual-network-create-udr-classic-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Selles artiklis antakse ülevaade klassikaline juurutamise mudel.

[AZURE.INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

Valimi põhjal Azure PowerShelli käskude allpool oodata on lihtne keskkond, mis on juba loodud ülaltoodud stsenaariumi. Kui soovite käivitada käske, nagu need on kuvatud selles dokumendis, luua keskkond, mis on näidatud [loomine on VNet (klassikaline) PowerShelli kaudu](virtual-networks-create-vnet-classic-netcfg-ps.md).

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-the-udr-for-the-front-end-subnet"></a>Looge UDR esiosa alamvõrgu jaoks
Marsruutimiseks tabeli ja marsruutimiseks esiosa alamvõrgu ülaltoodud stsenaariumi jaoks vajalik loomiseks järgige alltoodud juhiseid.

3. Käivitage selle **`New-AzureRouteTable`** cmdlet-käsu jaoks esiosa alamvõrgu marsruutimiseks tabeli loomiseks.

        New-AzureRouteTable -Name UDR-FrontEnd `
            -Location uswest `
            -Label "Route table for front end subnet"

    Väljund:

        Name         Location   Label                          
        ----         --------   -----                          
        UDR-FrontEnd West US    Route table for front end subnet

4. Käivitage selle **`Set-AzureRoute`** cmdlet-käsu luua marsruudi marsruudi tabelis loodud kohal kõik liikluse sinna tagasi end alamvõrgu (192.168.2.0/24) **FW1** VM (192.168.0.4).
    
        Get-AzureRouteTable UDR-FrontEnd `
            |Set-AzureRoute -RouteName RouteToBackEnd -AddressPrefix 192.168.2.0/24 `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress 192.168.0.4

    Väljund:

        Name     : UDR-FrontEnd
        Location : West US
        Label    : Route table for frontend subnet
        Routes   : 
                   Name                 Address Prefix    Next hop type        Next hop IP address
                   ----                 --------------    -------------        -------------------
                   RouteToBackEnd       192.168.2.0/24    VirtualAppliance     192.168.0.4  

5. Käivitage selle **`Set-AzureSubnetRouteTable`** cmdlet marsruutimiseks tabeli seostamiseks loodud kohal **FrontEnd** alamvõrgu.

        Set-AzureSubnetRouteTable -VirtualNetworkName TestVNet `
            -SubnetName FrontEnd `
            -RouteTableName UDR-FrontEnd
 
## <a name="create-the-udr-for-the-back-end-subnet"></a>Tagasi end alamvõrgu jaoks UDR loomine
Marsruutimiseks tabeli ja marsruutimiseks tagasi end alamvõrgu ülaltoodud stsenaariumi jaoks vajalik loomiseks järgige alltoodud juhiseid.

3. Käivitage selle **`New-AzureRouteTable`** cmdlet-käsu jaoks tagasi end alamvõrgu marsruutimiseks tabeli loomiseks.

        New-AzureRouteTable -Name UDR-BackEnd `
            -Location uswest `
            -Label "Route table for back end subnet"

4. Käivitage selle **`Set-AzureRoute`** cmdlet-käsu luua marsruudi marsruudi tabelis loodud kohal kõik liikluse sinna esiosa alamvõrgu (192.168.1.0/24) **FW1** VM (192.168.0.4).

        Get-AzureRouteTable UDR-BackEnd `
            |Set-AzureRoute -RouteName RouteToFrontEnd -AddressPrefix 192.168.1.0/24 `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress 192.168.0.4

5. Käivitage selle **`Set-AzureSubnetRouteTable`** cmdlet marsruutimiseks tabeli seostamiseks loodud kohal alamvõrgu **Taustväärtus** .

        Set-AzureSubnetRouteTable -VirtualNetworkName TestVNet `
            -SubnetName BackEnd `
            -RouteTableName UDR-BackEnd

## <a name="enable-ip-forwarding-on-the-fw1-vm"></a>FW1 VM IP saatmise lubamine
Ümbersuunamise FW1 VM IP lubamiseks tehke järgmist.

1. Käivitage selle **`Get-AzureIPForwarding`** cmdlet-käsu kontrolli olekut IP ümbersuunamine

        Get-AzureVM -Name FW1 -ServiceName TestRGFW `
            | Get-AzureIPForwarding

    Väljund:

        Disabled

2. Käivitage selle **`Set-AzureIPForwarding`** käsk, et IP ümbersuunamine *FW1* VM jaoks.

        Get-AzureVM -Name FW1 -ServiceName TestRGFW `
            | Set-AzureIPForwarding -Enable
