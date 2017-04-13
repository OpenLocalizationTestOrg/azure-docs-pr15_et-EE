## <a name="how-to-create-a-classic-vnet-using-azure-cli"></a>Kuidas luua klassikaline VNet, kasutades Azure CLI

Azure'i CLI abil saate hallata oma Azure ressursse Käsuviip, mis tahes arvutist, kus töötab Windows, Linux või OSX. Azure'i CLI abil on VNet loomiseks järgige alltoodud juhiseid.

1. Kui teil on Azure CLI kunagi kasutanud, leiate [installida ja konfigureerida Azure CLI](../articles/xplat-cli-install.md) ja järgige juhiseid selle kohani, kus saate valida oma Azure'i konto ja tellimuse.
2. Käsu **azure võrgu vnet loomine** loomiseks on VNet ja soovitud alamvõrku, nagu allpool näidatud. Pärast väljund selgitatakse kasutatud parameetrite loend.

            azure network vnet create --vnet TestVNet -e 192.168.0.0 -i 16 -n FrontEnd -p 192.168.1.0 -r 24 -l "Central US"
    
    Oodatav väljund:

            info:    Executing command network vnet create
            + Looking up network configuration
            + Looking up locations
            + Setting network configuration
            info:    network vnet create command OK

    - **--vnet**. Luua VNet nimi. Meie stsenaariumi *TestVNet*
    - **-e (või--- aadressiruumi jaoks)**. VNet aadressiruumi. Meie stsenaariumi *192.168.0.0*
    - **-i (või - cidr)**. Võrgu mask CIDR vormingus. Meie stsenaariumi *16*.
    - **- n (või--alamvõrgu-nime**). Esimese alamvõrgu nimi. Meie stsenaariumi *FrontEnd*.
    - **-p (või--alamvõrgu-algus-ip)**. Alates alamvõrgu või alamvõrgu aadressiruumi IP-aadress. Meie stsenaariumi *192.168.1.0*.
    - **-r (või--alamvõrgu-cidr)**. Võrgu mask alamvõrgu CIDR vormingus. Meie stsenaariumi *24*.
    - **-l (või - asukoht)**. Azure'i piirkond, kus on VNet luuakse. Meie stsenaariumi, *Kesk-USA*.

3. Käsu **azure alamvõrku vnet loomine** loomiseks on alamvõrku, nagu allpool näidatud. Pärast väljund selgitatakse kasutatud parameetrite loend.

            azure network vnet subnet create -t TestVNet -n BackEnd -a 192.168.2.0/24
    
    Siin on oodatud väljundi ülaltoodud käsk:

            info:    Executing command network vnet subnet create
            + Looking up network configuration
            + Creating subnet "BackEnd"
            + Setting network configuration
            + Looking up the subnet "BackEnd"
            + Looking up network configuration
            data:    Name                            : BackEnd
            data:    Address prefix                  : 192.168.2.0/24
            info:    network vnet subnet create command OK

    - **-t (või--vnet-nime**. VNet, kus luuakse alamvõrgu nimi. Meie stsenaariumi *TestVNet*.
    - **-n (või - nimi)**. Uue alamvõrgu nimi. Meie stsenaariumi *Taustväärtus*.
    - **-(või - aadress eesliide)**. Alamvõrgu CIDR plokk. Neli meie stsenaariumi, *192.168.2.0/24*.

4. Käivitage **azure võrgu vnet Kuva** käsk Uus vnet, atribuutide vaatamiseks nagu allpool näidatud.

            azure network vnet show

    Siin on oodatud väljundi ülaltoodud käsk:

            info:    Executing command network vnet show
            Virtual network name: TestVNet
            + Looking up the virtual network sites
            data:    Name                            : TestVNet
            data:    Location                        : Central US
            data:    State                           : Created
            data:    Address space                   : 192.168.0.0/16
            data:    Subnets:
            data:      Name                          : FrontEnd
            data:      Address prefix                : 192.168.1.0/24
            data:
            data:      Name                          : BackEnd
            data:      Address prefix                : 192.168.2.0/24
            data:
            info:    network vnet show command OK
