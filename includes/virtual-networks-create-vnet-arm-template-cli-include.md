## <a name="deploy-the-arm-template-by-using-the-azure-cli"></a>ARM malli abil Azure'i CLI

Malli ARM laadisite Azure'i CLI abil, järgige alltoodud juhiseid.

1. Kui teil on Azure CLI kunagi kasutanud, leiate [installida ja konfigureerida Azure CLI](../articles/xplat-cli-install.md) ja järgige juhiseid selle kohani, kus saate valida oma Azure'i konto ja tellimuse.
2. Käivitage selle **`azure config mode`** käsu lülitumine ressursihaldur, nagu allpool näidatud.

        azure config mode arm

    Siin on oodatud väljundi ülaltoodud käsk:

        info:    New mode is arm

3. Vajaduse korral käivitage selle **`azure group create`** luua uue ressursirühma, nagu allpool näidatud. Pange tähele käsu väljund. Pärast väljund selgitatakse kasutatud parameetrite loend. Ressursi rühmade kohta lisateabe saamiseks külastage [Azure'i ressursihaldur ülevaade](../articles/resource-group-overview.md).

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

4. Käivitage selle **`azure group deployment create`** cmdlet-käsk Uus VNet juurutamiseks Mall ja parameetri faile alla laadida ja muuta kohal. Pärast väljund selgitatakse kasutatud parameetrite loend.

        azure group deployment create -g TestRG -n TestVNetDeployment -f C:\ARM\azuredeploy.json -e C:\ARM\azuredeploy-parameters.json

    Siin on oodatud väljundi ülaltoodud käsk:

        info:    Executing command group deployment create
        + Initializing template configurations and parameters
        + Creating a deployment
        info:    Created template deployment "TestVNetDeployment"
        + Registering providers
        info:    Registering provider microsoft.network
        + Waiting for deployment to complete
        data:    DeploymentName     : TestVNetDeployment
        data:    ResourceGroupName  : TestRG
        data:    ProvisioningState  : Succeeded
        data:    Timestamp          : 2015-08-14T21:56:11.152759Z
        data:    Mode               : Incremental
        data:    Name           Type    Value
        data:    -------------  ------  --------------
        data:    location       String  Central US
        data:    vnetName       String  TestVNet
        data:    addressPrefix  String  192.168.0.0/16
        data:    subnet1Prefix  String  192.168.1.0/24
        data:    subnet1Name    String  FrontEnd
        data:    subnet2Prefix  String  192.168.2.0/24
        data:    subnet2Name    String  BackEnd
        info:    group deployment create command OK

    - **-g (või--- ressursirühm)**. Luuakse uus VNet ressursirühma nimi.
    - **-f (või--malli-fail)**. Teie ARM malli faili tee.
    - **-e (või--parameetrite-fail)**. Teie ARM parameetrid faili tee.

5. Käivitage selle **`azure network vnet show`** käsk Uus vnet, atribuutide kuvamiseks, nagu allpool näidatud.

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
