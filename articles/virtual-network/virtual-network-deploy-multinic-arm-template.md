<properties
   pageTitle="Mitme NIC VMs malli abil rakenduses ressursihaldur juurutamine | Microsoft Azure'i"
   description="Saate teada, kuidas juurutada mitme NIC VMs sisse ressursihaldur malli abil"
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
   ms.date="02/02/2016"
   ms.author="jdial" />

# <a name="deploy-multi-nic-vms-using-a-template"></a>Mitme NIC VMs malli abil juurutamine

[AZURE.INCLUDE [virtual-network-deploy-multinic-arm-selectors-include.md](../../includes/virtual-network-deploy-multinic-arm-selectors-include.md)]

[AZURE.INCLUDE [virtual-network-deploy-multinic-intro-include.md](../../includes/virtual-network-deploy-multinic-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][klassikaline juurutamise mudel](virtual-network-deploy-multinic-classic-ps.md).

[AZURE.INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

Kuna sel hetkel ei tohi olla ühe NIC VMs ja mitu NIcs VMs sama ressursi rühma, mida rakendab tagasi lõpuks serverites ressursirühma ja kõik muud komponendid teise turberühma. Alltoodud juhiseid kasutada ressursirühma nimega *IaaSStory* peamised ressursirühm ja *IaaSStory-kirjutamata* tagasi lõpuks serverites.

## <a name="prerequisites"></a>Eeltingimused

Tagasi lõpuks serverites juurutamiseks peate juurutamine peamised ressursirühma selle stsenaariumi korral kõik vajalikud ressursid. Järgmiste ressursside juurutada, järgige alltoodud juhiseid.

1. Avage [leht mallina](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC).
2. Klõpsake malli lehe paremas servas **ema ressursirühm** **Deploy Azure**.
3. Vajadusel muutke parameetrite väärtused ja seejärel juhised leiate Azure'i eelvaade portaali juurutamiseks ressursirühma.

> [AZURE.IMPORTANT] Veenduge, et teie salvestusruumi konto nimed on kordumatu. Dubleeritud salvestusruumi kontonimed ei saa Azure.

## <a name="understand-the-deployment-template"></a>Juurutamise malli mõistmine

Enne juurutamist dokumentide soovitud malli, veenduge, et teil mõista, mida see tähendab. Alltoodud juhiseid andma hea ülevaate kõnealuse mall.

1. Avage [leht mallina](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC).
2. Klõpsake malli faili avamiseks **azuredeploy.json** .
3. Pange tähele *osType* parameeter, mis on loetletud allpool. See parameeter on kasutatud mis VM pilti kasutada andmebaasi serveri valimiseks, koos mitme operatsioonisüsteemi seotud sätted.

        "osType": {
          "type": "string",
          "defaultValue": "Windows",
          "allowedValues": [
            "Windows",
            "Ubuntu"
          ],
          "metadata": {
            "description": "Type of OS to use for VMs: Windows or Ubuntu."
          }
        },

4. Liikuge kerides allapoole jaotiseni muutujate loendit ja märkige definitsiooni **dbVMSetting** muutujaid, mis on loetletud allpool. Ta saab ühte massiivi **dbVMSettings** muutuja sisalduvad elemendid. Kui olete tuttav tarkvara arengu terminoloogiaga, saate vaadata **dbVMSettings** muutujat mõne Hashtable talletatakse või mõne dictionay.

        "dbVMSetting": "[variables('dbVMSettings')[parameters('osType')]]"

5. Oletame, et te otsustate juurutamine Windowsi VMs töötab SQL-i tagasi lõppu. Seejärel **osType** väärtus oleks *Windows*ja **dbVMSetting** muutuja sisaldaks allpool, element, mis tähistab **dbVMSettings** muutuja esimene väärtus.

          "Windows": {
            "vmSize": "Standard_DS3",
            "publisher": "MicrosoftSQLServer",
            "offer": "SQL2014SP1-WS2012R2",
            "sku": "Standard",
            "version": "latest",
            "vmName": "DB",
            "osdisk": "osdiskdb",
            "datadisk": "datadiskdb",
            "nicName": "NICDB",
            "ipAddress": "192.168.2.",
            "extensionDeployment": "",
            "avsetName": "ASDB",
            "remotePort": 3389,
            "dbPort": 1433
          },

6. Pange tähele, et **vmSize** sisaldab väärtust *Standard_DS3*. Luba mitu NICs kasutamine ainult teatud VM suurused. Saate kontrollida, millist VM suurused on mitme NIC lubatud külastades [mitme NIC ülevaade](virtual-networks-multiple-nics.md).
7. Kerige allapoole suvandini **ressursid** ja pange tähele esimene element. See kirjeldab salvestusruumi konto. Selle konto salvestusruumi kasutatakse andmete ketast, mis kasutavad iga andmebaasi VM säilitada. Selle stsenaariumi korral on iga andmebaasi VM OS ketas, mis talletatakse regulaarselt ja kaks andmete plaati talletatud SSD (premium) salvestusruumi.

        {
          "apiVersion": "2015-05-01-preview",
          "type": "Microsoft.Storage/storageAccounts",
          "name": "[parameters('prmStorageName')]",
          "location": "[variables('location')]",
          "tags": {
            "displayName": "Storage Account - Premium"
          },
          "properties": {
            "accountType": "[parameters('prmStorageType')]"
          }
        },

8. Liikuge kerides jaotiseni järgmise ressursi, nagu allpool. Selle ressursi tähistab NIC kasutatakse andmebaasi Accessi andmebaasis iga VM. Pange tähele funktsiooni **Kopeeri** selle ressursi kasutamine. Mall võimaldab juurutada VMs nii palju, nagu soovite, **dbCount** parameetrite põhjal. Seega peate looma sama palju NICs andmebaasi juurdepääsu, üks iga VM.

        {
          "apiVersion": "2015-06-15",
          "type": "Microsoft.Network/networkInterfaces",
          "name": "[concat(variables('dbVMSetting').nicName,'-DA-', copyindex(1))]",
          "location": "[variables('location')]",
          "tags": {
            "displayName": "NetworkInterfaces - DB DA"
          },
          "copy": {
            "name": "dbniccount",
            "count": "[parameters('dbCount')]"
          },
          "properties": {
            "ipConfigurations": [
              {
                "name": "ipconfig1",
                "properties": {
                  "privateIPAllocationMethod": "Static",
                  "privateIPAddress": "[concat(variables('dbVMSetting').ipAddress,copyindex(4))]",
                  "subnet": {
                    "id": "[variables('backEndSubnetRef')]"
                  }
                }
              }
            ]
          }
        },

9. Liikuge kerides jaotiseni järgmise ressursi, nagu allpool. Selle ressursi tähistab NIC kasutatakse iga andmebaasi VM haldus. Taas, peate need NICs üks iga andmebaasi VM. Pange tähele **networkSecurityGroup** element, linkimise NSG, mis lubab juurdepääsu RDP/SSH ainult selle NIC.

        {
          "apiVersion": "2015-06-15",
          "type": "Microsoft.Network/networkInterfaces",
          "name": "[concat(variables('dbVMSetting').nicName, '-RA-',copyindex(1))]",
          "location": "[variables('location')]",
          "tags": {
            "displayName": "NetworkInterfaces - DB RA"
          },
          "copy": {
            "name": "dbniccount",
            "count": "[parameters('dbCount')]"
          },
          "properties": {
            "ipConfigurations": [
              {
                "name": "ipconfig1",
                "properties": {
                  "networkSecurityGroup": {
                    "id": "[resourceId('Microsoft.Network/networkSecurityGroups', parameters('remoteAccessNSGName'))]"
                  },
                  "privateIPAllocationMethod": "Static",
                  "privateIPAddress": "[concat(variables('dbVMSetting').ipAddress,copyindex(54))]",
                  "subnet": {
                    "id": "[variables('backEndSubnetRef')]"
                  }
                }
              }
            ]
          }
        },

10. Liikuge kerides jaotiseni järgmise ressursi, nagu allpool. Selle ressursi tähistab saadavus määrata kõik andmebaasi VMs jagada. Nii saate tagada, et alati on üks VM hoolduse ajal töötab määramine.

        {
          "apiVersion": "2015-06-15",
          "type": "Microsoft.Compute/availabilitySets",
          "name": "[variables('dbVMSetting').avsetName]",
          "location": "[variables('location')]",
          "tags": {
            "displayName": "AvailabilitySet - DB"
          }
        },

11. Liikuge kerides jaotiseni järgmise ressursi. Selle ressursi tähistab andmebaasi VMs, nagu on näha esimese ridu allpool. Pange tähele kasutada funktsiooni **Kopeeri** uuesti tagada, et mitme VMs on loodud põhineb **dbCount** parameetri. Teade ka saidikogumi **ei leia dependsOn** . See on loetletud kaks NICs oleks vaja enne VM juurutatakse koos kättesaadavus määramine ja salvestusruumi konto luua.

          "apiVersion": "2015-06-15",
          "type": "Microsoft.Compute/virtualMachines",
          "name": "[concat(variables('dbVMSetting').vmName,copyindex(1))]",
          "location": "[variables('location')]",
          "dependsOn": [
            "[concat('Microsoft.Network/networkInterfaces/', variables('dbVMSetting').nicName,'-DA-', copyindex(1))]",
            "[concat('Microsoft.Network/networkInterfaces/', variables('dbVMSetting').nicName,'-RA-', copyindex(1))]",
            "[concat('Microsoft.Compute/availabilitySets/', variables('dbVMSetting').avsetName)]",
            "[concat('Microsoft.Storage/storageAccounts/', parameters('prmStorageName'))]"
          ],
          "tags": {
            "displayName": "VMs - DB"
          },
          "copy": {
            "name": "dbvmcount",
            "count": "[parameters('dbCount')]"
          },

12. Liikuge kerides allapoole VM koodiressursi **networkProfile** element, nagu allpool. Pange tähele, et on kaks NICs, on viide iga VM. Kui loote mitu NICs VM, tuleb atribuudi **esmane** ühe NICs *true*ja *FALSE*ülejäänud.

        "networkProfile": {
          "networkInterfaces": [
            {
              "id": "[resourceId('Microsoft.Network/networkInterfaces', concat(variables('dbVMSetting').nicName,'-DA-',copyindex(1)))]",
              "properties": { "primary": true }
            },
            {
              "id": "[resourceId('Microsoft.Network/networkInterfaces', concat(variables('dbVMSetting').nicName,'-RA-',copyindex(1)))]",
              "properties": { "primary": false }
            }
          ]
        }
      }

## <a name="deploy-the-arm-template-by-using-click-to-deploy"></a>ARM malli abil klõpsake juurutamine

> [AZURE.IMPORTANT] Veenduge, et te järgige [eeltingimused](#Pre-requisites) enne alltoodud juhiseid.

Proovi Mall saadaval avaliku hoidla kasutab parameetri faili, mis sisaldab vaikimisi stsenaarium, mis on kirjeldatud ülal loomiseks kasutatud väärtused. Juurutada selle malli abil klõpsake juurutada, [seda](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC)linki, paremal **Taustväärtus ressursi rühmitamine (vt dokumentide)** klõpsake **Azure juurutamine**, vaikimisi parameetrite väärtused vajadusel asendada, ja järgige juhiseid portaalis.

Joonisel on uue ressursirühma sisu juurutamise pärast.

![Tagasi end ressursirühm](./media/virtual-network-deploy-multinic-arm-template/Figure2.png)

## <a name="deploy-the-template-by-using-powershell"></a>Malli PowerShelli abil

PowerShelli abil allalaaditud malli, järgige alltoodud juhiseid.

[AZURE.INCLUDE [powershell-preview-include.md](../../includes/powershell-preview-include.md)]

3. Käivitage selle **`New-AzureRmResourceGroup`** cmdlet-käsu loomine malli abil ressursirühma.

        New-AzureRmResourceGroup -Name IaaSStory-Backend -Location uswest `
            -TemplateFile 'https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.json' `
            -TemplateParameterFile 'https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.parameters.json'

    Oodatav väljund:

        ResourceGroupName : IaaSStory-Backend
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
                            Actions  NotActions
                            =======  ==========
                            *                  

        Resources         :
                            Name                 Type                                 Location
                            ===================  ===================================  ========
                            ASDB                 Microsoft.Compute/availabilitySets   westus  
                            DB1                  Microsoft.Compute/virtualMachines    westus  
                            DB2                  Microsoft.Compute/virtualMachines    westus  
                            NICDB-DA-1           Microsoft.Network/networkInterfaces  westus  
                            NICDB-DA-2           Microsoft.Network/networkInterfaces  westus  
                            NICDB-RA-1           Microsoft.Network/networkInterfaces  westus  
                            NICDB-RA-2           Microsoft.Network/networkInterfaces  westus  
                            wtestvnetstorageprm  Microsoft.Storage/storageAccounts    westus  

        ResourceId        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory-Backend

## <a name="deploy-the-template-by-using-the-azure-cli"></a>Malli abil Azure'i CLI

Malli abil Azure'i CLI, järgige alltoodud juhiseid.

1. Kui teil on Azure CLI kunagi kasutanud, leiate [installida ja konfigureerida Azure CLI](../xplat-cli-install.md) ja järgige juhiseid selle kohani, kus saate valida oma Azure'i konto ja tellimuse.
2. Käivitage selle **`azure config mode`** käsk lülitumine ressursihaldur, nagu allpool näidatud.

        azure config mode arm

    Siin on oodatud väljundi ülaltoodud käsk:

        info:    New mode is arm

3. [Parameetri faili](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.parameters.json)avada, valige selle sisu ja salvestage see oma arvutis olevasse faili. Selle näite puhul me salvestatud faili parameetrite *parameters.json*.

4. Käivitage selle **`azure group deployment create`** cmdlet-käsk Uus VNet juurutamiseks Mall ja parameetri faile alla laadida ja muuta kohal. Pärast väljund selgitatakse kasutatud parameetrite loend.

        azure group create -n IaaSStory-Backend -l westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.json -e parameters.json

    Oodatav väljund:

        info:    Executing command group create
        + Getting resource group IaaSStory-Backend
        + Creating resource group IaaSStory-Backend
        info:    Created resource group IaaSStory-Backend
        + Initializing template configurations and parameters
        + Creating a deployment
        info:    Created template deployment "azuredeploy"
        data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory-Backend
        data:    Name:                IaaSStory-Backend
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:
        info:    group create command OK
