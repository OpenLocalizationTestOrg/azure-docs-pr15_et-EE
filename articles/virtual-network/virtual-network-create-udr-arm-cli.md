<properties 
   pageTitle="Kontrolli marsruutimist ja kasutada virtuaalseid seadmeid sisse ressursihaldur Azure'i CLI abil | Microsoft Azure'i"
   description="Saate teada, kuidas määrata marsruutimist ja Azure CLI virtuaalse seadmete kasutamine"
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
   ms.date="03/15/2016"
   ms.author="jdial" />

#<a name="create-user-defined-routes-udr-in-the-azure-cli"></a>Luua kasutaja määratletud Azure CLI marsruudib (UDR)

[AZURE.INCLUDE [virtual-network-create-udr-arm-selectors-include.md](../../includes/virtual-network-create-udr-arm-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Selles artiklis antakse ülevaade ressursihaldur juurutamise mudel. Samuti saate [luua UDRs klassikaline juurutamise mudeli](virtual-network-create-udr-classic-cli.md).

[AZURE.INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

Proovi Azure'i CLI käsud allpool oodatud on lihtne keskkond, mis on juba loodud põhineb ülaltoodud stsenaariumi. Kui soovite käivitada käske, nagu need on kuvatud selles dokumendis, esmalt koostada testi keskkonna juurutamine [selle malli](http://github.com/telmosampaio/azure-templates/tree/master/IaaS-NSG-UDR-Before)abil, käsku **Deploy Azure**, asendada vaikimisi parameetrite väärtused vajadusel ja järgige juhiseid teemas portaali.

[AZURE.INCLUDE [azure-cli-prerequisites-include.md](../../includes/azure-cli-prerequisites-include.md)]

## <a name="create-the-udr-for-the-front-end-subnet"></a>Looge UDR esiosa alamvõrgu jaoks
Marsruutimiseks tabeli ja marsruutimiseks esiosa alamvõrgu ülaltoodud stsenaariumi jaoks vajalik loomiseks järgige alltoodud juhiseid.

3. Käivitage selle **`azure network route-table create`** käsk jaoks esiosa alamvõrgu marsruutimiseks tabeli loomiseks.

        azure network route-table create -g TestRG -n UDR-FrontEnd -l uswest

    Väljund:

        info:    Executing command network route-table create
        info:    Looking up route table "UDR-FrontEnd"
        info:    Creating route table "UDR-FrontEnd"
        info:    Looking up route table "UDR-FrontEnd"
        data:    Id                              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/
        routeTables/UDR-FrontEnd
        data:    Name                            : UDR-FrontEnd
        data:    Type                            : Microsoft.Network/routeTables
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        info:    network route-table create command OK

    Parameetrid:
    - **-g (või--- ressursirühm)**. Kus luuakse selle UDR ressursirühma nimi. Meie stsenaariumi *TestRG*.
    - **-l (või - asukoht)**. Azure'i piirkond, kus luuakse uus UDR. Meie stsenaariumi *westus*.
    - **-n (või - nimi)**. Uue UDR nimi. Meie stsenaariumi *UDR-FrontEnd*.

4. Käivitage selle **`azure network route-table route create`** käsku Loo marsruudi marsruutimiseks tabeli kohal loodud saata kõik liiklust sinna tagasi end alamvõrgu (192.168.2.0/24) **FW1** VM (192.168.0.4).

        azure network route-table route create -g TestRG -r UDR-FrontEnd -n RouteToBackEnd -a 192.168.2.0/24 -y VirtualAppliance -p 192.168.0.4

    Väljund:

        info:    Executing command network route-table route create
        info:    Looking up route "RouteToBackEnd" in route table "UDR-FrontEnd"
        info:    Creating route "RouteToBackEnd" in a route table "UDR-FrontEnd"
        info:    Looking up route "RouteToBackEnd" in route table "UDR-FrontEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        routeTables/UDR-FrontEnd/routes/RouteToBackEnd
        data:    Name                            : RouteToBackEnd
        data:    Provisioning state              : Succeeded
        data:    Next hop type                   : VirtualAppliance
        data:    Next hop IP address             : 192.168.0.4
        data:    Address prefix                  : 192.168.2.0/24
        info:    network route-table route create command OK

    Parameetrid:
    - **-r (või--marsruutimiseks tabelinime)**. Kui marsruudi lisatakse marsruutimiseks tabeli nimi. Meie stsenaariumi *UDR-FrontEnd*.
    - **-(või - aadress eesliide)**. Aadress eesliide alamvõrku, kus paketid on määratud. Meie stsenaariumi *192.168.2.0/24*.
    - **-y (või--edasi-sõnumihüppe kohta, pärast-tüüpi)**. Tippige objekti liikluse saadetakse. Võimalikud väärtused on *VirtualAppliance*, *VirtualNetworkGateway*, *VNETLocal*, *Interneti*või *mitte keegi*.
    - **-p (või--edasi sõnumihüppe kohta, pärast ip-aadressi**). Järgmise sõnumihüppe kohta, pärast IP-aadress. Meie stsenaariumi *192.168.0.4*.

5. Käivitage selle **`azure network vnet subnet set`** käsk marsruutimiseks tabeli kohal **FrontEnd** alamvõrgu loodud siduda.

        azure network vnet subnet set -g TestRG -e TestVNet -n FrontEnd -r UDR-FrontEnd

    Väljund:

        info:    Executing command network vnet subnet set
        info:    Looking up the subnet "FrontEnd"
        info:    Looking up route table "UDR-FrontEnd"
        info:    Setting subnet "FrontEnd"
        info:    Looking up the subnet "FrontEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        virtualNetworks/TestVNet/subnets/FrontEnd
        data:    Type                            : Microsoft.Network/virtualNetworks/subnets
        data:    ProvisioningState               : Succeeded
        data:    Name                            : FrontEnd
        data:    Address prefix                  : 192.168.1.0/24
        data:    Network security group          : [object Object]
        data:    Route Table                     : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        routeTables/UDR-FrontEnd
        data:    IP configurations:
        data:      /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICWEB1/ipConf
        igurations/ipconfig1
        data:      /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICWEB2/ipConf
        igurations/ipconfig1
        data:    
        info:    network vnet subnet set command OK

    Parameetrid:
    - **-e (või--vnet-nimi)**. VNet, kus asub alamvõrgu nimi. Meie stsenaariumi *TestVNet*.
 
## <a name="create-the-udr-for-the-back-end-subnet"></a>Tagasi end alamvõrgu jaoks UDR loomine
Marsruutimiseks tabeli ja marsruutimiseks tagasi end alamvõrgu ülaltoodud stsenaariumi jaoks vajalik loomiseks järgige alltoodud juhiseid.

1. Käivitage selle **`azure network route-table create`** käsu tagasi end alamvõrgu jaoks marsruutimiseks tabeli loomiseks.

        azure network route-table create -g TestRG -n UDR-BackEnd -l westus

4. Käivitage selle **`azure network route-table route create`** käsku Loo marsruudi loodud kohal esiosa alamvõrgu (192.168.1.0/24) **FW1** VM (192.168.0.4) sinna kõik liikluse marsruutimiseks tabelis.

        azure network route-table route create -g TestRG -r UDR-BackEnd -n RouteToFrontEnd -a 192.168.1.0/24 -y VirtualAppliance -p 192.168.0.4

5. Käivitage selle **`azure network vnet subnet set`** käsk marsruutimiseks tabeli kohal **Taustväärtus** alamvõrgu loodud siduda.

        azure network vnet subnet set -g TestRG -e TestVNet -n BackEnd -r UDR-BackEnd

## <a name="enable-ip-forwarding-on-fw1"></a>FW1 IP saatmise lubamine
IP kasutatavaid **FW1**NIC edastamise lubamiseks tehke järgmist.

1. Käivitage selle **`azure network nic show`** käsk ja märkate väärtust **Luba IP**edastamiseks. Peaks olema seatud *väär (FALSE)*.

        azure network nic show -g TestRG -n NICFW1

    Väljund:
        
        info:    Executing command network nic show
        info:    Looking up the network interface "NICFW1"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        networkInterfaces/NICFW1
        data:    Name                            : NICFW1
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    MAC address                     : 00-0D-3A-30-95-B3
        data:    Enable IP forwarding            : false
        data:    Tags                            : displayName=NetworkInterfaces - DMZ
        data:    Virtual machine                 : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Compute/
        virtualMachines/FW1
        data:    IP configurations:
        data:      Name                          : ipconfig1
        data:      Provisioning state            : Succeeded
        data:      Public IP address             : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        publicIPAddresses/PIPFW1
        data:      Private IP address            : 192.168.0.4
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        virtualNetworks/TestVNet/subnets/DMZ
        data:    
        info:    network nic show command OK

2. Käivitage selle **`azure network nic set`** käsk, et IP ümbersuunamine.

        azure network nic set -g TestRG -n NICFW1 -f true

    Väljund:

        info:    Executing command network nic set
        info:    Looking up the network interface "NICFW1"
        info:    Updating network interface "NICFW1"
        info:    Looking up the network interface "NICFW1"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        networkInterfaces/NICFW1
        data:    Name                            : NICFW1
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    MAC address                     : 00-0D-3A-30-95-B3
        data:    Enable IP forwarding            : true
        data:    Tags                            : displayName=NetworkInterfaces - DMZ
        data:    Virtual machine                 : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Compute/
        virtualMachines/FW1
        data:    IP configurations:
        data:      Name                          : ipconfig1
        data:      Provisioning state            : Succeeded
        data:      Public IP address             : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        publicIPAddresses/PIPFW1
        data:      Private IP address            : 192.168.0.4
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        virtualNetworks/TestVNet/subnets/DMZ
        data:    
        info:    network nic set command OK

    Parameetrid:

    - **-f (või--enable-ip-ümbersuunamine)**. *True* või *false*.
