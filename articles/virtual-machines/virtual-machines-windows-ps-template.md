<properties
    pageTitle="Ressursihaldur malli abil luua VM | Microsoft Azure'i"
    description="Ressursihaldur Mall ja PowerShelli abil saate hõlpsalt luua uue Windowsi virtuaalse masina."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/06/2016"
    ms.author="davidmu"/>

# <a name="create-a-windows-virtual-machine-with-a-resource-manager-template"></a>Windowsi virtuaalse masina ressursihaldur malli loomine

Selles artiklis tutvustab teile mõni Azure ressursihaldur Mall ja näidatakse, kuidas PowerShelli abil saate juurutada. Mall kasutab ühe virtuaalse masina ühe alamvõrgu uue virtuaalse võrgu Windows Server töötab.

Kulub umbes 20 minutit teha selles artiklis toodud juhiseid.

> [AZURE.IMPORTANT] Kui soovite oma VM on kättesaadavus komplekti osana, lisage see määramine VM loomisel. Praegu pole täiesti VM lisamiseks saadavus seadmine pärast selle loomist.

## <a name="step-1-create-the-template-file"></a>Samm 1: Loo mallifail

Saate luua oma malli abil [loome Azure'i ressursihaldur Mallid](../resource-group-authoring-templates.md)leiduva teabe. Saate juurutada Mallid, mis on teie jaoks loodud [Azure'i Quickstarts mallide](https://azure.microsoft.com/documentation/templates/)põhjal.

1. Avage oma lemmik tekstiredaktoris ja lisada ka on nõutav contentVersion nõutav skeemi element:

        {
          "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
          "contentVersion": "1.0.0.0",
        }
2. [Parameetrite](../resource-group-authoring-templates.md#parameters) alati need pole kohustuslikud, kuid need võimaldavad malli juurutamisel väärtuste sisestamiseks. Pärast contentVersion elemendi elemendi parameetrid ja selle tütarelemendid lisamine

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUserName": { "type": "string" },
            "adminPassword": { "type": "securestring" }
          },
        }

3. [Muutujaid](../resource-group-authoring-templates.md#variables) saab kasutada malli määrata väärtused, mis võivad muutuda sageli või väärtused, mis on vaja luua kombinatsiooni parameetrite väärtused. Pärast jaotist parameetrite muutujate elemendi lisamiseks tehke järgmist.

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUsername": { "type": "string" },
            "adminPassword": { "type": "securestring" }
          },
          "variables": {
            "vnetID":"[resourceId('Microsoft.Network/virtualNetworks','myvn1')]",
            "subnetRef": "[concat(variables('vnetID'),'/subnets/mysn1')]"  
          },
        }
        
4. [Ressursse](../resource-group-authoring-templates.md#resources) , näiteks virtuaalse masina, virtuaalse võrgu ja salvestusruumi konto on määratletud järgmine mall. Pärast jaotist muutujate ressursid jaotise lisamiseks tehke järgmist.

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUsername": { "type": "string" },
            "adminPassword": { "type": "securestring" }
          },
          "variables": {
            "vnetID":"[resourceId('Microsoft.Network/virtualNetworks','myvn1')]",
            "subnetRef": "[concat(variables('vnetID'),'/subnets/mysn1')]"
          },
          "resources": [
            {
              "type": "Microsoft.Storage/storageAccounts",
              "name": "mystorage1",
              "apiVersion": "2015-06-15",
              "location": "[resourceGroup().location]",
              "properties": { "accountType": "Standard_LRS" }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Network/publicIPAddresses",
              "name": "myip1",
              "location": "[resourceGroup().location]",
              "properties": {
                "publicIPAllocationMethod": "Dynamic",
                "dnsSettings": { "domainNameLabel": "mydns1" }
              }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Network/virtualNetworks",
              "name": "myvn1",
              "location": "[resourceGroup().location]",
              "properties": {
                "addressSpace": { "addressPrefixes": [ "10.0.0.0/16" ] },
                "subnets": [ {
                  "name": "mysn1",
                  "properties": { "addressPrefix": "10.0.0.0/24" }
                } ]
              }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Network/networkInterfaces",
              "name": "mync1",
              "location": "[resourceGroup().location]",
              "dependsOn": [
                "Microsoft.Network/publicIPAddresses/myip1",
                "Microsoft.Network/virtualNetworks/myvn1"
              ],
              "properties": {
                "ipConfigurations": [ {
                  "name": "ipconfig1",
                  "properties": {
                    "privateIPAllocationMethod": "Dynamic",
                    "publicIPAddress": {
                      "id": "[resourceId('Microsoft.Network/publicIPAddresses', 'myip1')]"
                    },
                    "subnet": { "id": "[variables('subnetRef')]" }
                  }
                } ]
              }
            },
            {
              "apiVersion": "2016-03-30",
              "type": "Microsoft.Compute/virtualMachines",
              "name": "myvm1",
              "location": "[resourceGroup().location]",
              "dependsOn": [
                "Microsoft.Network/networkInterfaces/mync1",
                "Microsoft.Storage/storageAccounts/mystorage1"
              ],
              "properties": {
                "hardwareProfile": { "vmSize": "Standard_A1" },
                "osProfile": {
                  "computerName": "myvm1",
                  "adminUsername": "[parameters('adminUsername')]",
                  "adminPassword": "[parameters('adminPassword')]"
                },
                "storageProfile": {
                  "imageReference": {
                    "publisher": "MicrosoftWindowsServer",
                    "offer": "WindowsServer",
                    "sku": "2012-R2-Datacenter",
                    "version" : "latest"
                  },
                  "osDisk": {
                    "name": "myosdisk1",
                    "vhd": {
                      "uri": "https://mystorage1.blob.core.windows.net/vhds/myosdisk1.vhd"
                    },
                    "caching": "ReadWrite",
                    "createOption": "FromImage"
                  }
                },
                "networkProfile": {
                  "networkInterfaces" : [ {
                    "id": "[resourceId('Microsoft.Network/networkInterfaces','mync1')]"
                  } ]
                }
              }
            } ]
          }
          
    >[AZURE.NOTE] Selles artiklis loob töötab opsüsteem Windows Serveri versioon. Muid kujutisi valimise kohta lisateabe saamiseks vt [navigeerimine ja valige Azure virtuaalse masina pilte Windows PowerShelli ja Azure CLI](virtual-machines-linux-cli-ps-findimage.md).  
            
2. *VirtualMachineTemplate.json*malli failina salvestada.

## <a name="step-2-create-the-parameters-file"></a>Samm 2: Looge parameetrite fail

Ressursi parameetrite, mis on määratletud malli väärtuste määramiseks luua parameetrite fail, mis sisaldab väärtusi, mida kasutatakse malli juurutamisel.

1. Tekstiredaktoris, kopeerige see JSON sisu uus fail nimega *Parameters.json*:

        {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "adminUserName": { "value": "mytestacct1" },
            "adminPassword": { "value": "mytestpass1" }
          }
        }

    >[AZURE.NOTE] Vt lisateavet [kasutajanime ja parooli nõuete](virtual-machines-windows-faq.md#what-are-the-username-requirements-when-creating-a-vm)kohta.

2. Salvestage fail parameetrid.

## <a name="step-3-install-azure-powershell"></a>Samm 3: Azure'i PowerShelli installimine

Vaadake, [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md) teavet Azure PowerShelli uusima versiooni installimise, valides tellimuse ja oma kontosse sisse logida.

## <a name="step-4-create-a-resource-group"></a>Samm 4: Ressursi rühma loomine

Kõik ressursid tuleb kasutusele [Ressursirühma](../azure-resource-manager/resource-group-overview.md).

1. Siin on loend saadaval kohast, kuhu saab luua ressursse.

        Get-AzureRmLocation | sort DisplayName | Select DisplayName

2. Asendage väärtus **$locName** asukoht loendist, näiteks **Kesk-USA**. Saate luua muutuja.

        $locName = "location name"
        
3. Asendage väärtus **$rgName** nimi uue ressursirühma. Saate luua muutuja ja ressursirühma.

        $rgName = "resource group name"
        New-AzureRmResourceGroup -Name $rgName -Location $locName
        
    Peaksite nägema midagi, nagu järgmises näites:
    
        ResourceGroupName : myrg1
        Location          : centralus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/{subscription-id}/resourceGroups/myrg1

## <a name="step-5-create-the-resources-with-the-template-and-parameters"></a>Juhis 5: Ressursside loomine malli ja parameetrid

Asendage väärtus **$templateFile** tee ja faili malli nimi. Asendage väärtus **$parameterFile** tee ja parameetrite faili nimi. Muutujate loomine ja seejärel malli. 

        $templateFile = "template file"
        $parameterFile = "parameter file"
        New-AzureRmResourceGroupDeployment -ResourceGroupName $rgName -TemplateFile $templateFile -TemplateParameterFile $parameterFile

    You will see something like this:

        DeploymentName    : VirtualMachineTemplate
        ResourceGroupName : myrg1
        ProvisioningState : Succeeded
        Timestamp         : 4/14/2016 8:11:37 PM
        Mode              : Incremental
        TemplateLink      :
        Parameters        :
                            Name             Type                       Value
                            ===============  =========================  ==========
                            adminUsername    String                     mytestacct1
                            adminPassword    SecureString

        Outputs           :

>[AZURE.NOTE] Saate juurutada Mallid ja parameetrite Azure storage konto. Lisateavet leiate teemast [Azure PowerShelli kasutamine Azure Storage](../storage/storage-powershell-guide-full.md).

## <a name="next-steps"></a>Järgmised sammud

- Kui probleemid juurutamise, järgmise juhisega oleks vaadata [tõrkeotsingu ressursside rühma juurutuste Azure'i portaal](../resource-manager-troubleshoot-deployments-portal.md)
- Saate teada, kuidas hallata virtuaalse masina, vaadates [haldamine virtuaalmasinates Azure'i ressursihaldur ja PowerShelli abil](virtual-machines-windows-ps-manage.md)loodud.
