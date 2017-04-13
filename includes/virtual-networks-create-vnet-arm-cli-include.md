## <a name="how-to-create-a-vnet-using-the-azure-cli"></a>Kuidas luua VNet, mis on Azure CLI abil

Azure'i CLI abil saate hallata oma Azure ressursse Käsuviip, mis tahes arvutist, kus töötab Windows, Linux või OSX. Azure'i CLI abil on VNet loomiseks järgige alltoodud juhiseid.

1. Kui teil on Azure CLI kunagi kasutanud, leiate [installida ja konfigureerida Azure CLI](../articles/xplat-cli-install.md) ja järgige juhiseid selle kohani, kus saate valida oma Azure'i konto ja tellimuse.
2. Käsu **azure config režiimi** ressursihaldur lülitumine, nagu allpool näidatud.

        azure config mode arm

    Siin on oodatud väljundi ülaltoodud käsk:

        info:    New mode is arm

3. Vajaduse korral käivitage selle **Azure'i rühma loomine** luua uue ressursirühma, nagu allpool näidatud. Pange tähele käsu väljund. Pärast väljund selgitatakse kasutatud parameetrite loend. Ressursi rühmade kohta lisateabe saamiseks külastage [Azure'i ressursihaldur ülevaade](../articles/virtual-network/resource-group-overview.md#resource-groups).

        azure group create -n TestRG -l centralus

    Siin on oodatud väljundi ülaltoodud käsk:

        info:    Executing command group create
        + Getting resource group TestRG
        + Creating resource group TestRG
        info:    Created resource group TestRG
        data:    Id:                  /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG
        data:    Name:                TestRG
        data:    Location:            centralus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:
        info:    group create command OK

    - **-n (või - nimi)**. Uue ressursirühma nimi. Meie stsenaariumi *TestRG*.
    - **-l (või - asukoht)**. Azure'i piirkond, kus on loodud uue ressursirühma. Meie stsenaariumi *centralus*.

4. Käsu **azure võrgu vnet loomine** loomiseks on VNet ja soovitud alamvõrku, nagu allpool näidatud. 

        azure network vnet create -g TestRG -n TestVNet -a 192.168.0.0/16 -l centralus

    Siin on oodatud väljundi ülaltoodud käsk:

        info:    Executing command network vnet create
        + Looking up virtual network "TestVNet"
        + Creating virtual network "TestVNet"
        + Loading virtual network state
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet2
        data:    Name                            : TestVNet
        data:    Type                            : Microsoft.Network/virtualNetworks
        data:    Location                        : centralus
        data:    ProvisioningState               : Succeeded
        data:    Address prefixes:
        data:      192.168.0.0/16
        info:    network vnet create command OK

    - **-g (või--- ressursirühm)**. Kus on VNet luuakse ressursirühma nimi. Meie stsenaariumi *TestRG*.
    - **-n (või - nimi)**. Luua VNet nimi. Meie stsenaariumi *TestVNet*
    - **-(või--eesliiteid-aadress)**. CIDR blocks VNet aadressiruumi jaoks kasutatav loend. Meie stsenaariumi *192.168.0.0/16*
    - **-l (või - asukoht)**. Azure'i piirkond, kus on VNet luuakse. Meie stsenaariumi *centralus*.

5. Käsu **azure alamvõrku vnet loomine** loomiseks on alamvõrku, nagu allpool näidatud. Pange tähele käsu väljund. Pärast väljund selgitatakse kasutatud parameetrite loend.

        azure network vnet subnet create -g TestRG -e TestVNet -n FrontEnd -a 192.168.1.0/24

    Siin on oodatud väljundi ülaltoodud käsk:

        info:    Executing command network vnet subnet create
        + Looking up the subnet "FrontEnd"
        + Creating subnet "FrontEnd"
        + Looking up the subnet "FrontEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
        data:    Type                            : Microsoft.Network/virtualNetworks/subnets
        data:    ProvisioningState               : Succeeded
        data:    Name                            : FrontEnd
        data:    Address prefix                  : 192.168.1.0/24
        data:
        info:    network vnet subnet create command OK

    - **-e (või--vnet-nime**. VNet, kus luuakse alamvõrgu nimi. Meie stsenaariumi *TestVNet*.
    - **-n (või - nimi)**. Uue alamvõrgu nimi. Meie stsenaariumi *FrontEnd*.
    - **-(või - aadress eesliide)**. Alamvõrgu CIDR plokk. Neli meie stsenaariumi, *192.168.1.0/24*.

6. Korrake juhiseid 5 ülaltoodud loomiseks muud alamvõrku vajaduse korral. Meie stsenaariumi *Taustväärtus* alamvõrgu loomiseks alltoodud käsu käivitamine.

        azure network vnet subnet create -g TestRG -e TestVNet -n BackEnd -a 192.168.2.0/24

4. Käivitage **azure võrgu vnet Kuva** käsk Uus vnet, atribuutide vaatamiseks nagu allpool näidatud.

        azure network vnet show -g TestRG -n TestVNet

    Siin on oodatud väljundi ülaltoodud käsk:

        info:    Executing command network vnet show
        + Looking up virtual network "TestVNet"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet
        data:    Name                            : TestVNet
        data:    Type                            : Microsoft.Network/virtualNetworks
        data:    Location                        : centralus
        data:    ProvisioningState               : Succeeded
        data:    Address prefixes:
        data:      192.168.0.0/16
        data:    Subnets:
        data:      Name                          : FrontEnd
        data:      Address prefix                : 192.168.1.0/24
        data:
        data:      Name                          : BackEnd
        data:      Address prefix                : 192.168.2.0/24
        data:
        info:    network vnet show command OK
