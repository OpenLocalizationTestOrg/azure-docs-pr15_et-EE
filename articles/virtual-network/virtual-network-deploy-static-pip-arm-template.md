<properties
   pageTitle="Juurutamine VM malli abil rakenduses ressursihaldur staatilise avaliku IP | Microsoft Azure'i"
   description="Saate teada, kuidas juurutada VMs staatilise avaliku IP sisse ressursihaldur malli abil"
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
   ms.date="04/27/2016"
   ms.author="jdial" />

# <a name="deploy-a-vm-with-a-static-public-ip-using-a-template"></a>Juurutamine VM staatilise avaliku IP malli abil

[AZURE.INCLUDE [virtual-network-deploy-static-pip-arm-selectors-include.md](../../includes/virtual-network-deploy-static-pip-arm-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-deploy-static-pip-intro-include.md](../../includes/virtual-network-deploy-static-pip-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)]klassikaline juurutamise mudel.

[AZURE.INCLUDE [virtual-network-deploy-static-pip-scenario-include.md](../../includes/virtual-network-deploy-static-pip-scenario-include.md)]

## <a name="public-ip-resources-in-a-template-file"></a>Avaliku IP ressursside mallifail

Saate vaadata ja [Proovi Mall](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.json)alla laadida.

Allpool olevat jaotist kuvatakse avaliku IP ressursi ülaltoodud stsenaariumi kindlaksmääramine.

      {
        "apiVersion": "2015-06-15",
        "type": "Microsoft.Network/publicIPAddresses",
        "name": "[variables('webVMSetting').pipName]",
        "location": "[variables('location')]",
        "properties": {
          "publicIPAllocationMethod": "Static"
        },
        "tags": {
          "displayName": "PublicIPAddress - Web"
        }
      },

Pange tähele **publicIPAllocationMethod** atribuut, mis on seatud *staatilise*. Atribuut võib olla *dünaamiline* (vaikeväärtus) või *staatiline*. See säte staatilise garantiid, mis ei muutu kunagi avaliku IP-aadress.

Allpool olevat jaotist kuvatakse avaliku IP-aadressi seos võrgu liidese.

      {
        "apiVersion": "2015-06-15",
        "type": "Microsoft.Network/networkInterfaces",
        "name": "[variables('webVMSetting').nicName]",
        "location": "[variables('location')]",
        "tags": {
          "displayName": "NetworkInterface - Web"
        },
        "dependsOn": [
          "[concat('Microsoft.Network/publicIPAddresses/', variables('webVMSetting').pipName)]",
          "[concat('Microsoft.Network/virtualNetworks/', parameters('vnetName'))]"
        ],
        "properties": {
          "ipConfigurations": [
            {
              "name": "ipconfig1",
              "properties": {
                "privateIPAllocationMethod": "Static",
                "privateIPAddress": "[variables('webVMSetting').ipAddress]",
                "publicIPAddress": {
                  "id": "[resourceId('Microsoft.Network/publicIPAddresses',variables('webVMSetting').pipName)]"
                },
                "subnet": {
                  "id": "[variables('frontEndSubnetRef')]"
                }
              }
            }
          ]
        }
      },

Pange tähele **publicIPAddress** atribuut osutab nimega **variables('webVMSetting').pipName**ressursi **ID-d** . Mis on eespool näidatud avaliku IP ressursi nimi.

Lõpuks võrgu liidese ülaltoodud on loetletud **networkProfile** atribuudi loodava VM.

      "networkProfile": {
        "networkInterfaces": [
          {
            "id": "[resourceId('Microsoft.Network/networkInterfaces', variables('webVMSetting').nicName)]"
          }
        ]
      }

## <a name="deploy-the-template-by-using-click-to-deploy"></a>Malli abil klõpsake juurutamine

Proovi Mall saadaval avaliku hoidla kasutab parameetri faili, mis sisaldab vaikimisi stsenaarium, mis on kirjeldatud ülal loomiseks kasutatud väärtused. Selle malli abil klõpsake juurutada juurutamiseks klõpsake **Deploy Azure** [VM koos staatilise PIP](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/03-Static-public-IP) malli failis Readme.md. Asendage parameetrite vaikeväärtuste soovi ja sisestage tühjaks parameetrite väärtused.  Järgige juhiseid portaalis luua virtuaalse masina staatilise avaliku IP-aadress.

## <a name="deploy-the-template-by-using-powershell"></a>Malli PowerShelli abil

PowerShelli abil allalaaditud malli, järgige alltoodud juhiseid.

1. Kui te pole kunagi varem Azure PowerShelli, vaadake, [Kuidas installida ja konfigureerida Azure PowerShell](../powershell-install-configure.md) ja järgige juhiseid 1 – 3.

2. Käivitage PowerShelli konsooli **New-AzureRmResourceGroup** cmdlet luua uue ressursirühma vajaduse korral. Kui teil on juba loodud ressursirühma, jätkake 3.

        New-AzureRmResourceGroup -Name PIPTEST -Location westus

    Oodatav väljund:

        ResourceGroupName : PIPTEST
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/StaticPublicIP

3. Käivitage PowerShelli konsooli **New-AzureRmResourceGroupDeployment** cmdlet-malli.

        New-AzureRmResourceGroupDeployment -Name DeployVM -ResourceGroupName PIPTEST `
            -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.json `
            -TemplateParameterUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.parameters.json

    Oodatav väljund:

        DeploymentName    : DeployVM
        ResourceGroupName : PIPTEST
        ProvisioningState : Succeeded
        Timestamp         : <Deployment date> <Deployment time>
        Mode              : Incremental
        TemplateLink      :
                            Uri            : https://raw.githubusercontent.com/Azure/azure-quickstart-templates/mas
                            ter/IaaS-Story/03-Static-public-IP/azuredeploy.json
                            ContentVersion : 1.0.0.0

        Parameters        :
                            Name                      Type                       Value     
                            ========================  =========================  ==========
                            vnetName                  String                     WTestVNet
                            vnetPrefix                String                     192.168.0.0/16
                            frontEndSubnetName        String                     FrontEnd  
                            frontEndSubnetPrefix      String                     192.168.1.0/24
                            storageAccountNamePrefix  String                     iaasestd  
                            stdStorageType            String                     Standard_LRS
                            osType                    String                     Windows   
                            adminUsername             String                     adminUser
                            adminPassword             SecureString                         

        Outputs           :

## <a name="deploy-the-template-by-using-the-azure-cli"></a>Malli abil Azure'i CLI

Malli abil Azure'i CLI, järgige alltoodud juhiseid.

1. Kui te pole kunagi varem Azure'i CLI, järgige [installida ja konfigureerida Azure CLI](../xplat-cli-install.md) artiklis toodud juhiseid ja seejärel CLI ühenduse tellimuse [Azure tellimuse: Azure'i käsurea liides (Azure'i CLI) ühenduse](../xplat-cli-connect.md) artikli jaotises "Kasuta azure login autentida interaktiivseks" juhiseid.
2. Käsu **azure config režiimi** ressursihaldur lülitumine, nagu allpool näidatud.

        azure config mode arm

    Siin on oodatud väljundi ülaltoodud käsk:

        info:    New mode is arm

3. [Parameetri faili](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.parameters.json)avada, valige selle sisu ja salvestage see oma arvutis olevasse faili. Selle näite puhul, salvestatakse fail nimega *parameters.json*parameetrid. Kui soovitud faili parameetrite väärtused muuta, kuid vähemalt, on soovitatav väärtust adminPassword parameetri kordumatu, keeruka parooli muuta.

4. Käivitage juurutada uue VNet Mall ja parameetri failid alla laadida ning muudetud kohal cmdlet **juurutamise azure rühma loomine** . Käsu alla, Asendage <path> tee salvestatud fail. 

        azure group create -n PIPTEST2 -l westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.json -e <path>\parameters.json

    Oodatav väljund (kasutatud parameetrite väärtused on loetletud):

        info:    Executing command group create
        + Getting resource group PIPTEST2
        + Creating resource group PIPTEST2
        info:    Created resource group PIPTEST2
        + Initializing template configurations and parameters
        + Creating a deployment
        info:    Created template deployment "azuredeploy"
        data:    Id:                  /subscriptions/<Subscription ID>/resourceGroups/PIPTEST2
        data:    Name:                PIPTEST2
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:
        info:    group create command OK
