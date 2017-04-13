<properties
   pageTitle="Klassikaline režiim, kasutades Azure CLI NSGs loomist | Microsoft Azure'i"
   description="Siit saate teada, kuidas luua ja juurutada NSGs abil Azure'i CLI klassikaline režiim"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
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

# <a name="how-to-create-nsgs-classic-in-the-azure-cli"></a>Kuidas luua CLI Azure'i NSGs (klassikaline)

[AZURE.INCLUDE [virtual-networks-create-nsg-selectors-classic-include](../../includes/virtual-networks-create-nsg-selectors-classic-include.md)]

[AZURE.INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Selles artiklis antakse ülevaade klassikaline juurutamise mudel. Samuti saate [luua NSGs ressursihaldur juurutamise mudeli](virtual-networks-create-nsg-arm-cli.md).

[AZURE.INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

Proovi Azure'i CLI käsud allpool oodatud on lihtne keskkond, mis on juba loodud põhineb ülaltoodud stsenaariumi. Kui soovite käivitada käske, nagu need on kuvatud selles dokumendis, esmalt koostada test keskkond [on VNet](virtual-networks-create-vnet-classic-cli.md)loomisega.

## <a name="how-to-create-the-nsg-for-the-front-end-subnet"></a>Kuidas luua NSG esiosa alamvõrgu jaoks
Mõne NSG nimega nimega **NSG-FrontEnd** ülaltoodud stsenaariumi loomiseks järgige alltoodud juhiseid.

1. Kui teil on Azure CLI kunagi kasutanud, leiate [installida ja konfigureerida Azure CLI](../xplat-cli-install.md) ja järgige juhiseid selle kohani, kus saate valida oma Azure'i konto ja tellimuse.

2. Käivitage selle **`azure config mode`** käsk aktiveerimiseks klassikaline režiim, nagu allpool näidatud.

        azure config mode asm

    Oodatav väljund:

        info:    New mode is asm

3. Käivitage selle **`azure network nsg create`** käsk on NSG loomiseks.

        azure network nsg create -l uswest -n NSG-FrontEnd

    Oodatav väljund:

        info:    Executing command network nsg create
        info:    Creating a network security group "NSG-FrontEnd"
        info:    Looking up the network security group "NSG-FrontEnd"
        data:    Name                            : NSG-FrontEnd
        data:    Location                        : West US
        data:    Security group rules:
        data:    Name                               Source IP           Source Port  Destination IP   Destination Port  Protocol  Type      Action  Prior
        ity  Default
        data:    ---------------------------------  ------------------  -----------  ---------------  ----------------  --------  --------  ------  -----
        ---  -------
        data:    ALLOW VNET OUTBOUND                VIRTUAL_NETWORK     *            VIRTUAL_NETWORK  *                 *         Outbound  Allow   65000
             true   
        data:    ALLOW VNET INBOUND                 VIRTUAL_NETWORK     *            VIRTUAL_NETWORK  *                 *         Inbound   Allow   65000
             true   
        data:    ALLOW AZURE LOAD BALANCER INBOUND  AZURE_LOADBALANCER  *            *                *                 *         Inbound   Allow   65001
             true   
        data:    ALLOW INTERNET OUTBOUND            *                   *            INTERNET         *                 *         Outbound  Allow   65001
             true   
        data:    DENY ALL OUTBOUND                  *                   *            *                *                 *         Outbound  Deny    65500
             true   
        data:    DENY ALL INBOUND                   *                   *            *                *                 *         Inbound   Deny    65500
             true   
        info:    network nsg create command OK

    Parameetrid:

    - **-l (või - asukoht)**. Azure'i piirkond, kus luuakse uus NSG. Meie stsenaariumi *westus*.
    - **-n (või - nimi)**. Uue NSG nimi. Meie stsenaariumi *NSG-FrontEnd*.

4. Käivitage selle **`azure network nsg rule create`** käsku Loo reegel, mis lubab juurdepääsu pordi 3389 (RDP) Interneti kaudu.

        azure network nsg rule create -a NSG-FrontEnd -n rdp-rule -c Allow -p Tcp -r Inbound -y 100 -f Internet -o * -e * -u 3389

    Oodatav väljund:

        info:    Executing command network nsg rule create
        info:    Looking up the network security group "NSG-FrontEnd"
        info:    Creating a network security rule "rdp-rule"
        info:    Looking up the network security group "NSG-FrontEnd"
        data:    Name                            : rdp-rule
        data:    Source address prefix           : INTERNET
        data:    Source Port                     : *
        data:    Destination address prefix      : *
        data:    Destination Port                : 3389
        data:    Protocol                        : TCP
        data:    Type                            : Inbound
        data:    Action                          : Allow
        data:    Priority                        : 100
        info:    network nsg rule create command OK

    Parameetrid:

    - **-(või--nsg-nimi)**. NSG, kus luuakse reegli nimi. Meie stsenaariumi *NSG-FrontEnd*.
    - **-n (või - nimi)**. Uue reegli nimi. Meie stsenaariumi *rdp-reegel*.
    - **-c (või--toimingu)**. Juurdepääsutase reegli (Keela või luba).
    - **-p (või--protocol)**. Protocol (Tcp, Udp, või *) reegli.
    - **-r (või--tüüp)**. Ühenduse (sissetulev või väljaminev) suunas.
    - **-y (või--prioriteet)**. Reegli prioriteet.
    - **-f (või--allika-aadress eesliide)**. Allikas aadress eesliide CIDR või vaikimisi siltide kasutamine.
    - **-o (või--lähtevahemikus port)**. Lähteport või port vahemikus.
    - **-e (või--sihtkoha-aadress eesliide)**. Sihtkoha aadress eesliide CIDR või vaikimisi siltide kasutamine.
    - **-u (või--destination-port vahemikus)**. Sihtport või port vahemikus.

5. Käivitage selle **`azure network nsg rule create`** käsku Loo reegel, mis lubab juurdepääsu pordi 80 (HTTP) Interneti kaudu.

        azure network nsg rule create -a NSG-FrontEnd -n web-rule -c Allow -p Tcp -r Inbound -y 200 -f Internet -o * -e * -u 80

    Oodatud putput:

        info:    Executing command network nsg rule create
        info:    Looking up the network security group "NSG-FrontEnd"
        info:    Creating a network security rule "web-rule"
        info:    Looking up the network security group "NSG-FrontEnd"
        data:    Name                            : web-rule
        data:    Source address prefix           : INTERNET
        data:    Source Port                     : *
        data:    Destination address prefix      : *
        data:    Destination Port                : 80
        data:    Protocol                        : TCP
        data:    Type                            : Inbound
        data:    Action                          : Allow
        data:    Priority                        : 200
        info:    network nsg rule create command OK

6. Käivitage selle **`azure network nsg subnet add`** käsk linkimiseks soovitud NSG alamvõrgu esiosa.

        azure network nsg subnet add -a NSG-FrontEnd --vnet-name TestVNet --subnet-name FrontEnd

    Oodatav väljund:

        info:    Executing command network nsg subnet add
        info:    Looking up the network security group "NSG-FrontEnd"
        info:    Looking up the subnet "FrontEnd"
        info:    Looking up network configuration
        info:    Creating a network security group "NSG-FrontEnd"
        info:    network nsg subnet add command OK

## <a name="how-to-create-the-nsg-for-the-back-end-subnet"></a>Kuidas luua NSG tagasi end alamvõrgu jaoks
Mõne NSG nimega nimega *NSG-kirjutamata* ülaltoodud stsenaariumi loomiseks järgige alltoodud juhiseid.

3. Käivitage selle **`azure network nsg create`** käsk on NSG loomiseks.

        azure network nsg create -l uswest -n NSG-BackEnd

    Oodatav väljund:

        info:    Executing command network nsg create
        info:    Creating a network security group "NSG-BackEnd"
        info:    Looking up the network security group "NSG-BackEnd"
        data:    Name                            : NSG-BackEnd
        data:    Location                        : West US
        data:    Security group rules:
        data:    Name                               Source IP           Source Port  Destination IP   Destination Port  Protocol  Type      Action  Prior
        ity  Default
        data:    ---------------------------------  ------------------  -----------  ---------------  ----------------  --------  --------  ------  -----
        ---  -------
        data:    ALLOW VNET OUTBOUND                VIRTUAL_NETWORK     *            VIRTUAL_NETWORK  *                 *         Outbound  Allow   65000
             true   
        data:    ALLOW VNET INBOUND                 VIRTUAL_NETWORK     *            VIRTUAL_NETWORK  *                 *         Inbound   Allow   65000
             true   
        data:    ALLOW AZURE LOAD BALANCER INBOUND  AZURE_LOADBALANCER  *            *                *                 *         Inbound   Allow   65001
             true   
        data:    ALLOW INTERNET OUTBOUND            *                   *            INTERNET         *                 *         Outbound  Allow   65001
             true   
        data:    DENY ALL OUTBOUND                  *                   *            *                *                 *         Outbound  Deny    65500
             true   
        data:    DENY ALL INBOUND                   *                   *            *                *                 *         Inbound   Deny    65500
             true   
        info:    network nsg create command OK

    Parameetrid:

    - **-l (või - asukoht)**. Azure'i piirkond, kus luuakse uus NSG. Meie stsenaariumi *westus*.
    - **-n (või - nimi)**. Uue NSG nimi. Meie stsenaariumi *NSG-FrontEnd*.

4. Käivitage selle **`azure network nsg rule create`** käsku Loo reegel, mis lubab juurdepääsu pordi 1433 (SQL) esiosa alamvõrgu kaudu.

        azure network nsg rule create -a NSG-BackEnd -n sql-rule -c Allow -p Tcp -r Inbound -y 100 -f 192.168.1.0/24 -o * -e * -u 1433

    Oodatav väljund:

        info:    Executing command network nsg rule create
        info:    Looking up the network security group "NSG-BackEnd"
        info:    Creating a network security rule "sql-rule"
        info:    Looking up the network security group "NSG-BackEnd"
        data:    Name                            : sql-rule
        data:    Source address prefix           : 192.168.1.0/24
        data:    Source Port                     : *
        data:    Destination address prefix      : *
        data:    Destination Port                : 1433
        data:    Protocol                        : TCP
        data:    Type                            : Inbound
        data:    Action                          : Allow
        data:    Priority                        : 100
        info:    network nsg rule create command OK


5. Käivitage selle **`azure network nsg rule create`** käsku Loo reegel, mis keelab access Interneti-ühendus.

        azure network nsg rule create -a NSG-BackEnd -n web-rule -c Deny -p Tcp -r Outbound -y 200 -f * -o * -e Internet -u 80

    Oodatud putput:

        info:    Executing command network nsg rule create
        info:    Looking up the network security group "NSG-BackEnd"
        info:    Creating a network security rule "web-rule"
        info:    Looking up the network security group "NSG-BackEnd"
        data:    Name                            : web-rule
        data:    Source address prefix           : *
        data:    Source Port                     : *
        data:    Destination address prefix      : INTERNET
        data:    Destination Port                : 80
        data:    Protocol                        : TCP
        data:    Type                            : Outbound
        data:    Action                          : Deny
        data:    Priority                        : 200
        info:    network nsg rule create command OK

6. Käivitage selle **`azure network nsg subnet add`** käsk linkimiseks soovitud NSG tagasi end alamvõrgu.

        azure network nsg subnet add -a NSG-BackEnd --vnet-name TestVNet --subnet-name BackEnd

    Oodatav väljund:

        info:    Executing command network nsg subnet add
        info:    Looking up the network security group "NSG-BackEndX"
        info:    Looking up the subnet "BackEnd"
        info:    Looking up network configuration
        info:    Creating a network security group "NSG-BackEndX"
        info:    network nsg subnet add command OK
