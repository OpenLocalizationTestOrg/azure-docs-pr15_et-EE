<properties 
   pageTitle="Määrata marsruutimist ja kasutada virtuaalse seadmete Azure'i CLI klassikaline juurutamise mudeli | Microsoft Azure'i"
   description="Siit saate teada, kuidas määrata marsruutimise VNets Azure CLI klassikaline juurutamise mudeli kasutamine"
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
   ms.date="03/15/2016"
   ms.author="jdial" />

#<a name="control-routing-and-use-virtual-appliances-classic-using-the-azure-cli"></a>Juhtida marsruutimist ja virtuaalne seadmed (klassikaline) abil Azure'i CLI kasutamine

[AZURE.INCLUDE [virtual-network-create-udr-classic-selectors-include.md](../../includes/virtual-network-create-udr-classic-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Selles artiklis antakse ülevaade klassikaline juurutamise mudel. Võite ka [juhtelementi marsruutimine ja virtuaalne seadmed ressursihaldur juurutamise mudeli kasutamine](virtual-network-create-udr-arm-cli.md).

[AZURE.INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

Proovi Azure'i CLI käsud allpool oodatud on lihtne keskkond, mis on juba loodud põhineb ülaltoodud stsenaariumi. Kui soovite käivitada käske, nagu need on kuvatud selles dokumendis, luua keskkond, mis on näidatud [on VNet (klassikaline) abil Azure'i CLI loomine](virtual-networks-create-vnet-classic-cli.md).

[AZURE.INCLUDE [azure-cli-prerequisites-include.md](../../includes/azure-cli-prerequisites-include.md)]

## <a name="create-the-udr-for-the-front-end-subnet"></a>Looge UDR esiosa alamvõrgu jaoks
Marsruutimiseks tabeli ja marsruutimiseks esiosa alamvõrgu ülaltoodud stsenaariumi jaoks vajalik loomiseks järgige alltoodud juhiseid.

1. Käivitage selle **`azure config mode`** klassikalise režiimi aktiveerimiseks.

        azure config mode asm

    Väljund:

        info:    New mode is asm

3. Käivitage selle **`azure network route-table create`** käsk jaoks esiosa alamvõrgu marsruutimiseks tabeli loomiseks.

        azure network route-table create -n UDR-FrontEnd -l uswest

    Väljund:

        info:    Executing command network route-table create
        info:    Creating route table "UDR-FrontEnd"
        info:    Getting route table "UDR-FrontEnd"
        data:    Name                            : UDR-FrontEnd
        data:    Location                        : West US
        info:    network route-table create command OK

    Parameetrid:
    - **-l (või - asukoht)**. Azure'i piirkond, kus luuakse uus NSG. Meie stsenaariumi *westus*.
    - **-n (või - nimi)**. Uue NSG nimi. Meie stsenaariumi *NSG-FrontEnd*.

4. Käivitage selle **`azure network route-table route set`** käsku Loo marsruudi marsruutimiseks tabeli kohal loodud saata kõik liiklust sinna tagasi end alamvõrgu (192.168.2.0/24) **FW1** VM (192.168.0.4).

        azure network route-table route set -r UDR-FrontEnd -n RouteToBackEnd -a 192.168.2.0/24 -t VirtualAppliance -p 192.168.0.4

    Väljund:

        info:    Executing command network route-table route set
        info:    Getting route table "UDR-FrontEnd"
        info:    Setting route "RouteToBackEnd" in a route table "UDR-FrontEnd"
        info:    network route-table route set command OK

    Parameetrid:
    - **-r (või--marsruutimiseks tabelinime)**. Kui marsruudi lisatakse marsruutimiseks tabeli nimi. Meie stsenaariumi *UDR-FrontEnd*.
    - **-(või - aadress eesliide)**. Aadress eesliide alamvõrku, kus paketid on määratud. Meie stsenaariumi *192.168.2.0/24*.
    - **-t (või--edasi-sõnumihüppe kohta, pärast-tüüpi)**. Tippige objekti liikluse saadetakse. Võimalikud väärtused on *VirtualAppliance*, *VirtualNetworkGateway*, *VNETLocal*, *Interneti*või *mitte keegi*.
    - **-p (või--edasi sõnumihüppe kohta, pärast ip-aadressi**). Järgmise sõnumihüppe kohta, pärast IP-aadress. Meie stsenaariumi *192.168.0.4*.

5. Käivitage selle **`azure network vnet subnet route-table add`** käsk marsruutimiseks tabeli kohal **FrontEnd** alamvõrgu loodud siduda.

        azure network vnet subnet route-table add -t TestVNet -n FrontEnd -r UDR-FrontEnd

    Väljund:

        info:    Executing command network vnet subnet route-table add
        info:    Looking up the subnet "FrontEnd"
        info:    Looking up network configuration
        info:    Looking up network gateway route tables in virtual network "TestVNet" subnet "FrontEnd"
        info:    Associating route table "UDR-FrontEnd" and subnet "FrontEnd"
        info:    Looking up network gateway route tables in virtual network "TestVNet" subnet "FrontEnd"
        data:    Route table name                : UDR-FrontEnd
        data:      Location                      : West US
        data:      Routes:
        info:    network vnet subnet route-table add command OK 

    Parameetrid:
    - **-t (või--vnet-nimi)**. VNet, kus asub alamvõrgu nimi. Meie stsenaariumi *TestVNet*.
    - **- n (või--alamvõrgu-nime**. Lisatakse alamvõrgu marsruutimiseks tabeli nimi. Meie stsenaariumi *FrontEnd*.
 
## <a name="create-the-udr-for-the-back-end-subnet"></a>Tagasi end alamvõrgu jaoks UDR loomine
Marsruutimiseks tabeli ja marsruutimiseks tagasi end alamvõrgu ülaltoodud stsenaariumi jaoks vajalik loomiseks järgige alltoodud juhiseid.

3. Käivitage selle **`azure network route-table create`** käsu tagasi end alamvõrgu jaoks marsruutimiseks tabeli loomiseks.

        azure network route-table create -n UDR-BackEnd -l uswest

4. Käivitage selle **`azure network route-table route set`** käsku Loo marsruudi loodud kohal esiosa alamvõrgu (192.168.1.0/24) **FW1** VM (192.168.0.4) sinna kõik liikluse marsruutimiseks tabelis.

        azure network route-table route set -r UDR-BackEnd -n RouteToFrontEnd -a 192.168.1.0/24 -t VirtualAppliance -p 192.168.0.4

5. Käivitage selle **`azure network vnet subnet route-table add`** käsk marsruutimiseks tabeli kohal **Taustväärtus** alamvõrgu loodud siduda.

        azure network vnet subnet route-table add -t TestVNet -n BackEnd -r UDR-BackEnd

