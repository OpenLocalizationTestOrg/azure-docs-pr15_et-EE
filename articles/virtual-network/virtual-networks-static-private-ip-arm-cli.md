<properties 
   pageTitle="ARM režiimis CLI privaatne staatiline IP määramine | Microsoft Azure'i"
   description="Staatiline IP-d (DIPs) ja kuidas neid hallata ARM režiimis CLI kasutamise mõistmine"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
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

# <a name="how-to-set-a-static-private-ip-address-in-azure-cli"></a>Azure'i CLI staatilise privaatne IP-aadress määramine

[AZURE.INCLUDE [virtual-networks-static-private-ip-selectors-arm-include](../../includes/virtual-networks-static-private-ip-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Selles artiklis antakse ülevaade ressursihaldur juurutamise mudel. Samuti saate [hallata staatilise privaatne IP-aadress klassikaline juurutamise mudeli](virtual-networks-static-private-ip-classic-cli.md).

[AZURE.INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

Proovi Azure'i CLI käsud allpool oodatud lihtne keskkond juba loonud. Kui soovite käivitada käske, nagu need on kuvatud selles dokumendis, esmalt koostada testi keskkond, mis on kirjeldatud [on vnet loomine](virtual-networks-create-vnet-arm-cli.md).

## <a name="how-to-specify-a-static-private-ip-address-when-creating-a-vm"></a>Kuidas määrata staatilise privaatne IP-aadress VM loomisel
Nimega *DNS01* on VNet nimega *TestVNet* *192.168.1.101*, Privaatne staatiline IP alamvõrgu *FrontEnd* VM loomiseks tehke järgmist:

1. Kui teil on Azure CLI kunagi kasutanud, leiate [installida ja konfigureerida Azure CLI](../xplat-cli-install.md) ja järgige juhiseid selle kohani, kus saate valida oma Azure'i konto ja tellimuse.

2. Käsu **azure config režiimi** ressursihaldur lülitumine, nagu allpool näidatud.

        azure config mode arm

    Oodatav väljund:

        info:    New mode is arm

3. Käivitage selle **Azure'i võrgu avaliku ip loomine** luua avaliku IP VM. Pärast väljund selgitatakse kasutatud parameetrite loend.

        azure network public-ip create -g TestRG -n TestPIP -l centralus
    
    Oodatav väljund:

        info:    Executing command network public-ip create
        + Looking up the public ip "TestPIP"
        + Creating public ip address "TestPIP"
        + Looking up the public ip "TestPIP"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/publicIPAddresses/TestPIP
        data:    Name                            : TestPIP
        data:    Type                            : Microsoft.Network/publicIPAddresses
        data:    Location                        : centralus
        data:    Provisioning state              : Succeeded
        data:    Allocation method               : Dynamic
        data:    Idle timeout                    : 4
        info:    network public-ip create command OK

    - **-g (või--- ressursirühm)**. Nimi on loodud avaliku IP ressursirühma.
    - **-n (või - nimi)**. Avaliku IP nimi.
    - **-l (või - asukoht)**. Azure'i piirkond, kus luuakse avaliku IP. Meie stsenaariumi *centralus*.

3. **Azure'i võrgu nic loomine** loomiseks on NIC privaatne staatiline IP käsu käivitamine. Pärast väljund selgitatakse kasutatud parameetrite loend.

        azure network nic create -g TestRG -n TestNIC -l centralus -a 192.168.1.101 -m TestVNet -k FrontEnd

    Oodatav väljund:

        + Looking up the network interface "TestNIC"
        + Looking up the subnet "FrontEnd"
        + Creating network interface "TestNIC"
        + Looking up the network interface "TestNIC"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC
        data:    Name                            : TestNIC
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : centralus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 192.168.1.101
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
        data:
        info:    network nic create command OK

    - **-(või--era-ip-aadress)**. Staatiline IP privaatne NIC.
    - **-m (või--alamvõrgu-vnet-nimi)**. VNet, kus luuakse NIC nimi.
    - **-k (või--alamvõrgu-nimi)**. Kus luuakse NIC alamvõrgu nimi.

4. **Azure'i vm loomine** VM, kasutades avaliku IP ja NIC loodud kohal loomiseks käsu käivitamine. Pärast väljund selgitatakse kasutatud parameetrite loend.

        azure vm create -g TestRG -n DNS01 -l centralus -y Windows -f TestNIC -i TestPIP -F TestVNet -j FrontEnd -o vnetstorage -q bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2 -u adminuser -p AdminP@ssw0rd

    Oodatav väljund:

        info:    Executing command vm create
        + Looking up the VM "DNS01"
        info:    Using the VM Size "Standard_A1"
        warn:    The image "bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2" will be used for VM
        info:    The [OS, Data] Disk or image configuration requires storage account
        + Looking up the storage account vnetstorage
        + Looking up the NIC "TestNIC"
        info:    Found an existing NIC "TestNIC"
        info:    Found an IP configuration with virtual network subnet id "/subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd" in the NIC "TestNIC"
        info:    Found public ip parameters, trying to setup PublicIP profile
        + Looking up the public ip "TestPIP"
        info:    Found an existing PublicIP "TestPIP"
        info:    Configuring identified NIC IP configuration with PublicIP "TestPIP"
        + Updating NIC "TestNIC"
        + Looking up the NIC "TestNIC"
        + Creating VM "DNS01"
        info:    vm create command OK

    - **-y (või--os-tüüpi)**. Tüüpi operatsioonisüsteem VM, kas *Windows* või *Linux*.
    - **-f (või--nic-nimi)**. Kasutatakse VM NIC nimi.
    - **-i (või--avaliku-ip-nimi)**. Kasutatakse avaliku IP VM nime.
    - **-F (või--vnet-nimi)**. VNet, kus luuakse VM nimi.
    - **-j (või--vnet-alamvõrgu-nimi)**. Kus luuakse VM alamvõrgu nimi.

## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a>Kuidas hankida staatilise privaatne IP-aadressi teave VM

Staatilise privaatne IP-aadressi teavet VM loodud ülaltoodud skripti kuvamiseks käivitage järgmine käsk Azure'i CLI ja jälgida väärtused *Privaatne IP alloc-meetod* ja *Privaatne IP-aadress*:

    azure vm show -g TestRG -n DNS01

Oodatav väljund:

    info:    Executing command vm show
    + Looking up the VM "DNS01"
    + Looking up the NIC "TestNIC"
    + Looking up the public ip "TestPIP
    data:    Id                              :/subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Compute/virtualMachines/DNS01
    data:    ProvisioningState               :Succeeded
    data:    Name                            :DNS01
    data:    Location                        :centralus
    data:    Type                            :Microsoft.Compute/virtualMachines
    data:
    data:    Hardware Profile:
    data:      Size                          :Standard_A1
    data:
    data:    Storage Profile:
    data:      Source image:
    data:        Id                          :/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/services/images/bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2
    data:
    data:      OS Disk:
    data:        OSType                      :Windows
    data:        Name                        :cli08d7bd987a0112a8-os-1441774961355
    data:        Caching                     :ReadWrite
    data:        CreateOption                :FromImage
    data:        Vhd:
    data:          Uri                       :https://vnetstorage2.blob.core.windows.net/vhds/cli08d7bd987a0112a8-os-1441774961355vhd
    data:
    data:    OS Profile:
    data:      Computer Name                 :DNS01
    data:      User Name                     :adminuser
    data:      Windows Configuration:
    data:        Provision VM Agent          :true
    data:        Enable automatic updates    :true
    data:
    data:    Network Profile:
    data:      Network Interfaces:
    data:        Network Interface #1:
    data:          Id                        :/subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC
    data:          Primary                   :true
    data:          MAC Address               :00-0D-3A-90-1A-A8
    data:          Provisioning State        :Succeeded
    data:          Name                      :TestNIC
    data:          Location                  :centralus
    data:            Private IP alloc-method :Static
    data:            Private IP address      :192.168.1.101
    data:            Public IP address       :40.122.213.159
    info:    vm show command OK

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a>Kuidas eemaldada VM staatilise privaatne IP-aadress
Ei saa eemaldada staatilise privaatne IP-aadress on NIC Azure'i CLI ressursihaldur jaoks. Tuleb luua uus NIC, mis kasutab dünaamiline IP, eelmise NIC eemaldamine VM ja seejärel lisage uus NIC VM. Muuta NIC jaoks kasutatud VM int eh käsud eespool, järgige alltoodud juhiseid.
    
1. Luua uue NIC, kasutades dünaamilise IP eraldatud **azure võrgu nic loomine** käsu käivitamine. Pange tähele, kui teil pole vaja seekord IP-aadressi määramiseks.

        azure network nic create -g TestRG -n TestNIC2 -l centralus -m TestVNet -k FrontEnd

    Oodatav väljund:

        info:    Executing command network nic create
        + Looking up the network interface "TestNIC2"
        + Looking up the subnet "FrontEnd"
        + Creating network interface "TestNIC2"
        + Looking up the network interface "TestNIC2"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC2
        data:    Name                            : TestNIC2
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : centralus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 192.168.1.6
        data:      Private IP Allocation Method  : Dynamic
        data:      Subnet                        : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
        data:
        info:    network nic create command OK

2. **Azure'i vm seadmine** muuta kasutatavat VM NIC käsu käivitamine.

        azure vm set -g TestRG -n DNS01 -N TestNIC2

    Oodatav väljund:

        info:    Executing command vm set
        + Looking up the VM "DNS01"
        + Looking up the NIC "TestNIC2"
        + Updating VM "DNS01"
        info:    vm set command OK

3. Kui, käivitage **azure võrgu nic kustutamine** käsk Kustuta vana NIC.

        azure network nic delete -g TestRG -n TestNIC --quiet

    Oodatav väljund:

        info:    Executing command network nic delete
        + Looking up the network interface "TestNIC"
        + Deleting network interface "TestNIC"
        info:    network nic delete command OK

## <a name="how-to-add-a-static-private-ip-address-to-an-existing-vm"></a>Kuidas lisada mõne olemasoleva VM staatilise privaatne IP-aadress
Lisada staatilise privaatne IP-aadress kasutatavaid ülaltoodud skripti abil loodud VM NIC, käivitage järgmine käsk:

    azure network nic set -g TestRG -n TestNIC2 -a 192.168.1.101

Oodatav väljund:

    info:    Executing command network nic set
    + Looking up the network interface "TestNIC2"
    + Updating network interface "TestNIC2"
    + Looking up the network interface "TestNIC2"
    data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC2
    data:    Name                            : TestNIC2
    data:    Type                            : Microsoft.Network/networkInterfaces
    data:    Location                        : centralus
    data:    Provisioning state              : Succeeded
    data:    MAC address                     : 00-0D-3A-90-29-25
    data:    Enable IP forwarding            : false
    data:    Virtual machine                 : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Compute/virtualMachines/DNS01
    data:    IP configurations:
    data:      Name                          : NIC-config
    data:      Provisioning state            : Succeeded
    data:      Private IP address            : 192.168.1.101
    data:      Private IP Allocation Method  : Static
    data:      Subnet                        : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
    data:
    info:    network nic set command OK

## <a name="next-steps"></a>Järgmised sammud

- Lisateavet [reserveeritud avaliku IP](virtual-networks-reserved-public-ip.md) -aadressid.
- Lisateavet [astme avaliku IP (ILPIP)](virtual-networks-instance-level-public-ip.md) aadresside kohta.
- Konsulteerige [reserveeritud IP REST API-d](https://msdn.microsoft.com/library/azure/dn722420.aspx).
