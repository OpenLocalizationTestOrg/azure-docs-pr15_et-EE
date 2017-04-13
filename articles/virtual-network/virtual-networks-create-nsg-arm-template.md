<properties
   pageTitle="Kuidas luua NSGs ARM režiimis malli abil | Microsoft Azure'i"
   description="Siit saate teada, kuidas luua ja juurutada NSGs rühmas malli abil"
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
   ms.date="02/02/2016"
   ms.author="jdial" />

# <a name="how-to-create-nsgs-using-a-template"></a>Kuidas luua NSGs malli abil

[AZURE.INCLUDE [virtual-networks-create-nsg-selectors-arm-include](../../includes/virtual-networks-create-nsg-selectors-arm-include.md)]

[AZURE.INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Selles artiklis antakse ülevaade ressursihaldur juurutamise mudel. Samuti saate [luua NSGs klassikaline juurutamise mudeli](virtual-networks-create-nsg-classic-ps.md).

[AZURE.INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

## <a name="nsg-resources-in-a-template-file"></a>NSG ressursside mallifail

Saate vaadata ja [Proovi Mall](https://raw.githubusercontent.com/telmosampaio/azure-templates/master/201-IaaS-WebFrontEnd-SQLBackEnd/NSGs.json)alla laadida.

Allpool olevat jaotist kuvatakse esiosa NSG, ülaltoodud stsenaariumi kindlaksmääramine.

      "apiVersion": "2015-06-15",
      "type": "Microsoft.Network/networkSecurityGroups",
      "name": "[parameters('frontEndNSGName')]",
      "location": "[resourceGroup().location]",
      "tags": {
        "displayName": "NSG - Front End"
      },
      "properties": {
        "securityRules": [
          {
            "name": "rdp-rule",
            "properties": {
              "description": "Allow RDP",
              "protocol": "Tcp",
              "sourcePortRange": "*",
              "destinationPortRange": "3389",
              "sourceAddressPrefix": "Internet",
              "destinationAddressPrefix": "*",
              "access": "Allow",
              "priority": 100,
              "direction": "Inbound"
            }
          },
          {
            "name": "web-rule",
            "properties": {
              "description": "Allow WEB",
              "protocol": "Tcp",
              "sourcePortRange": "*",
              "destinationPortRange": "80",
              "sourceAddressPrefix": "Internet",
              "destinationAddressPrefix": "*",
              "access": "Allow",
              "priority": 101,
              "direction": "Inbound"
            }
          }
        ]
      }

Seostada NSG esiosa alamvõrku, tuleb alamvõrgu määratluse malli muutmine ja kasutamiseks on NSG viite ID-d.

        "subnets": [
          {
            "name": "[parameters('frontEndSubnetName')]",
            "properties": {
              "addressPrefix": "[parameters('frontEndSubnetPrefix')]",
              "networkSecurityGroup": {
                "id": "[resourceId('Microsoft.Network/networkSecurityGroups', parameters('frontEndNSGName'))]"
              }
            }
          }, ...

Pange tähele sama tagasi lõpuks NSG ja tagasi end alamvõrgu malli teinud.

## <a name="deploy-the-arm-template-by-using-click-to-deploy"></a>ARM malli abil klõpsake juurutamine

Proovi Mall saadaval avaliku hoidla kasutab parameetri faili, mis sisaldab vaikimisi stsenaarium, mis on kirjeldatud ülal loomiseks kasutatud väärtused. Selle malli abil klõpsake juurutada, [seda](http://github.com/telmosampaio/azure-templates/tree/master/201-IaaS-WebFrontEnd-SQLBackEnd-NSG)linki juurutamiseks käsku **Deploy Azure**, vajadusel vaikimisi parameeter väärtuste asendamine ja järgige juhiseid portaalis.

## <a name="deploy-the-arm-template-by-using-powershell"></a>ARM malli PowerShelli abil

Malli ARM laadisite PowerShelli abil, järgige alltoodud juhiseid.

[AZURE.INCLUDE [powershell-preview-include.md](../../includes/powershell-preview-include.md)]

1. Kui te pole kunagi varem Azure PowerShelli, vaadake, [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md) ja järgige juhiseid kõik viis selleks Azure'i sisse logida ja valige oma tellimuse.

3. Käivitage selle **`New-AzureRmResourceGroup`** cmdlet-käsu loomine malli abil ressursirühma.

        New-AzureRmResourceGroup -Name TestRG -Location uswest `
            -TemplateFile 'https://raw.githubusercontent.com/telmosampaio/azure-templates/master/201-IaaS-WebFrontEnd-SQLBackEnd/azuredeploy.json' `
            -TemplateParameterFile 'https://raw.githubusercontent.com/telmosampaio/azure-templates/master/201-IaaS-WebFrontEnd-SQLBackEnd/azuredeploy.parameters.json'

    Oodatav väljund:

        ResourceGroupName : TestRG
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
                            Actions  NotActions
                            =======  ==========
                            *                  

        Resources         :
                            Name                Type                                     Location
                            ==================  =======================================  ========
                            sqlAvSet            Microsoft.Compute/availabilitySets       westus  
                            webAvSet            Microsoft.Compute/availabilitySets       westus  
                            SQL1                Microsoft.Compute/virtualMachines        westus  
                            SQL2                Microsoft.Compute/virtualMachines        westus  
                            Web1                Microsoft.Compute/virtualMachines        westus  
                            Web2                Microsoft.Compute/virtualMachines        westus  
                            TestNICSQL1         Microsoft.Network/networkInterfaces      westus  
                            TestNICSQL2         Microsoft.Network/networkInterfaces      westus  
                            TestNICWeb1         Microsoft.Network/networkInterfaces      westus  
                            TestNICWeb2         Microsoft.Network/networkInterfaces      westus  
                            NSG-BackEnd         Microsoft.Network/networkSecurityGroups  westus  
                            NSG-FrontEnd        Microsoft.Network/networkSecurityGroups  westus  
                            TestPIPSQL1         Microsoft.Network/publicIPAddresses      westus  
                            TestPIPSQL2         Microsoft.Network/publicIPAddresses      westus  
                            TestPIPWeb1         Microsoft.Network/publicIPAddresses      westus  
                            TestPIPWeb2         Microsoft.Network/publicIPAddresses      westus  
                            TestVNet            Microsoft.Network/virtualNetworks        westus  
                            testvnetstorageprm  Microsoft.Storage/storageAccounts        westus  
                            testvnetstoragestd  Microsoft.Storage/storageAccounts        westus  

        ResourceId        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG

## <a name="deploy-the-arm-template-by-using-the-azure-cli"></a>ARM malli abil Azure'i CLI

Malli ARM Azure'i CLI abil, järgige alltoodud juhiseid.

1. Kui teil on Azure CLI kunagi kasutanud, leiate [installida ja konfigureerida Azure CLI](../xplat-cli-install.md) ja järgige juhiseid selle kohani, kus saate valida oma Azure'i konto ja tellimuse.
2. Käivitage selle **`azure config mode`** käsu lülitumine ressursihaldur, nagu allpool näidatud.

        azure config mode arm

    Siin on oodatud väljundi ülaltoodud käsk:

        info:    New mode is arm

4. Käivitage selle **`azure group deployment create`** cmdlet-käsk Uus VNet juurutamiseks Mall ja parameetri faile alla laadida ja muuta kohal. Pärast väljund selgitatakse kasutatud parameetrite loend.

        azure group create -n TestRG -l westus -f 'https://raw.githubusercontent.com/telmosampaio/azure-templates/master/201-IaaS-WebFrontEnd-SQLBackEnd/azuredeploy.json' -e 'https://raw.githubusercontent.com/telmosampaio/azure-templates/master/201-IaaS-WebFrontEnd-SQLBackEnd/azuredeploy.parameters.json'

    Oodatav väljund:

        info:    Executing command group create
        info:    Getting resource group TestRG
        info:    Creating resource group TestRG
        info:    Created resource group TestRG
        info:    Initializing template configurations and parameters
        info:    Creating a deployment
        info:    Created template deployment "azuredeploy"
        data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG
        data:    Name:                TestRG
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:    
        info:    group create command OK

    - **-n (või - nimi)**. Ressursirühm luua nimi.
    - **-l (või - asukoht)**. Azure'i piirkond, kus luuakse ressursirühma.
    - **-f (või--malli-fail)**. Teie ARM malli faili tee.
    - **-e (või--parameetrite-fail)**. Teie ARM parameetrid faili tee.
