<properties
   pageTitle="Ressursihaldur malli kiirtutvustus | Microsoft Azure'i"
   description="Samm-sammult samm-sammult juhendi ettevalmistamise lihtsa Azure'i IaaS arhitektuur ressursi halduri malli."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="navalev"
   manager="timlt"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/04/2016"
   ms.author="navale;tomfitz"/>
   
# <a name="resource-manager-template-walkthrough"></a>Ressursihaldur malli kiirtutvustus

Üks esimese küsimused malli loomisel on "Kuidas alustada?". Üks tühja malli, järgides [koostamine malli artiklis](resource-group-authoring-templates.md#template-format)kirjeldatud põhistruktuur ja ressursid ja sobivad parameetrid ja muutujate lisamine. Hea võimalus oleks läbi [Kiirjuhend Galerii](https://github.com/Azure/azure-quickstart-templates) ja vaadake, kas sarnased stsenaariumid üks, mida soovite luua. Saate ühendada mitu malli või redigeerige olemasolevat vastavalt oma konkreetse stsenaariumi. 

Heitkem pilk levinud taristu:

* Kahe virtuaalmasinates, mida kasutada sama salvestusruumi konto, on sama kättesaadavus määramine ja sama alamvõrgu virtuaalse võrgu.
* NIC ja VM IP-aadressi iga virtuaalse masina jaoks.
* Laadi koormusetasakaalustusteenuse koormuse tasakaalustamiseks pordi 80 reegel

![arhitektuur](./media/resource-group-overview/arm_arch.png)

Selles teemas juhatab teid läbi juhiseid, et taristu ressursihaldur malli loomine. Lõplik malli loomist põhineb Kiirjuhend malli ehk [2 VMs koormusetasakaalustusteenuse laadimine ja laadi tasakaalustamiseks reeglid](https://azure.microsoft.com/documentation/templates/201-2-vms-loadbalancer-lbrules/).

Kuid, mis on palju koostamise korraga, seega vaatame kõigepealt salvestusruumi konto loomine ja selle juurutamine. Pärast seda, kui olete õppinud salvestusruumi konto loomine, saate lisada muud ressursid ja uuesti malli infrastruktuuri lõpuleviimiseks.

>[AZURE.NOTE] Malli loomisel saate kasutada mis tahes tüüpi redaktor. Visual Studios annab teie käsutusse tööriistad, mis lihtsustavad malli arengu, kuid teil pole vaja Visual Studio õppeteema lõpuleviimiseks. Õpetuse Visual Studio Web App ja SQL-andmebaasi juurutamise loomiseks kasutamise kohta, vaadake [loomine ja juurutamine Azure ressursi rühma Visual Studio kaudu](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md). 

## <a name="create-the-resource-manager-template"></a>Ressursihaldur malli loomine

Mall on JSON fail, mis määratleb kõik ressursid on juurutamist. Lisaks võimaldab määratleda parameetrid juurutamisel, määratud muutujaid, mis on valmistatud teiste väärtused ja avaldised ja juurutamise väljundid. 

Alustame lihtsaim mall:

```json
    {
      "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {  },
      "variables": {  },
      "resources": [  ],
      "outputs": {  }
    }
 ```

Salvestage fail nimega **azuredeploy.json** (Pange tähele, et mall võib olla mis tahes nimi, mida soovite, just, et see tuleb json faili).

## <a name="create-a-storage-account"></a>Salvestusruumi konto loomine
Jaotises **ressursid** , lisage objekti, mis määratleb salvestusruumi konto, nagu allpool näidatud. 

```json
"resources": [
  {
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[parameters('storageAccountName')]",
    "apiVersion": "2015-06-15",
    "location": "[resourceGroup().location]",
    "properties": {
      "accountType": "Standard_LRS"
    }
  }
]
```

Võib tekkida, kui need atribuudid ja väärtused on pärit. Atribuudid **Tüüp**, **nimi**, **apiVersion**ja **asukoht** on standardne elemente, mis on saadaval kõigi ressursside tüüpide. Siit leiate levinud elementide veebisaidil [ressursside](resource-group-authoring-templates.md#resources)kohta. **nimi** on seatud parameetri väärtus, mida te kaotate käigus juurutamist ja **asukoht** kasutatavaid ressursirühma asukohaks. Vaatame, kuidas olete kindlaks teinud, **Tüüp** ja **apiVersion** allpool.

Jaotises **Atribuudid** , mis sisaldab kõiki atribuudid, mis esinevad vaid ressursile tüüp. Väärtus, saate määrata selle jaotise täpselt vastama panna toimingu REST API loomise selle ressursi tüüp. Salvestusruumi konto loomisel peate sisestama mõne **accountType**. Märkate [REST API salvestusruumi konto loomine](https://msdn.microsoft.com/library/azure/mt163564.aspx) , et jaotist atribuudid ja ülejäänud toimingu sisaldab ka **accountType** vara ja lubatud väärtused on avaldatud. Selles näites konto tüüp väärtuseks **Standard_LRS**, kuid te määrata mõne muu väärtuse või Luba kasutajatel konto tüübi parameetrina edasi.

Nüüd vaatame liikumine tagasi jaotisse **parameetrite** ja vaadake, kuidas määratlete salvestusruumi konto nimi. Saate lisateavet parameetrite [parameetrite](resource-group-authoring-templates.md#parameters)kasutamine. 

```json
"parameters" : {
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Storage Account Name"
      }
    }
}
```
Siin saate määratleda parameetri tüüp string, mis kuulub salvestusruumi konto nimi. Selle parameetri väärtus esitatakse malli juurutamisel.

## <a name="deploying-the-template"></a>Malli juurutamine
Meil on täielik malli salvestusruumi uue konto loomine. Kui te ei mäleta, mall on salvestatud **azuredeploy.json** faili:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters" : {
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Storage Account Name"
      }
    }
  },  
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[parameters('storageAccountName')]",
      "apiVersion": "2015-06-15",
      "location": "[resourceGroup().location]",
      "properties": {
        "accountType": "Standard_LRS"
      }
    }
  ]
}
```

Nagu näete, [ressursside juurutamise artiklis](resource-group-template-deploy.md)on üsna paar võimalust juurutada malli. Azure'i PowerShelli kaudu malli, saate kasutada.

```powershell
# create a new resource group
New-AzureRmResourceGroup -Name ExampleResourceGroup -Location "West Europe"

# deploy the template to the resource group
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile azuredeploy.json
```

Või malli abil Azure'i CLI, kasutage järgmist.

```
azure group create -n ExampleResourceGroup -l "West Europe"

azure group deployment create -f azuredeploy.json -g ExampleResourceGroup -n ExampleDeployment
```

Nüüd olete uhke omanik salvestusruumi konto!

Järgmised toimingud on ressursse, mis on vajalik juurutada arhitektuur kirjeldatud selles õpetuses algust. Järgmiste ressursside lisamist saate töötanud sama malli.

## <a name="availability-set"></a>Kättesaadavus määramine
Pärast määratluse salvestusruumi konto, lisada mõne transporditaristu määramine on virtuaalmasinates. Sel juhul ei ole täiendavaid atribuute, mis on nõutav, nii, et selle määratlus on üsna lihtne. Vt [REST API loomise kättesaadavus-seadmine](https://msdn.microsoft.com/library/azure/mt163607.aspx) täielik atribuudid jaotises juhuks, kui soovite määrata update domain count ja viga domeeni count väärtused.

```json
{
   "type": "Microsoft.Compute/availabilitySets",
   "name": "[variables('availabilitySetName')]",
   "apiVersion": "2015-06-15",
   "location": "[resourceGroup().location]",
   "properties": {}
}
```

Pange tähele, et muutuja väärtuseks on määratud **nimi** . Selle malli kättesaadavuse määramine nimi on vaja erinevasse kohta. Malli saate säilitada hõlpsam määratledes selle väärtuse üks kord ja seda kasutada mitmes kohas.

Määratud **Tüüp** väärtus sisaldab nii ressursi pakkuja ressursi tüüp. Kättesaadavus komplektid ressursi pakkuja on **Microsoft.Compute** ja ressursside tüüp on **availabilitySets**. Saate loendist saadaolevad ressursi pakkujad järgmist PowerShelli käsu:

```powershell
    Get-AzureRmResourceProvider -ListAvailable
```

Või kui kasutate Azure CLI, käivitage järgmine käsk:
```
    azure provider list
```
Selles teemas loote salvestusruumi kontod, virtuaalmasinates ja virtuaalse võrgunduse, et töötavad koos.

- Microsoft.Storage
- Microsoft.Compute
- Microsoft.Network

Kindla pakkuja ressursside tüüpide nägemiseks käivitage PowerShelli järgmine käsk:

```powershell
    (Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Compute).ResourceTypes
```

Või Azure CLI, kuvatakse järgmine käsk tagasi saadaval tüüpi JSON-vormingus ja faili salvestada.

```
    azure provider show Microsoft.Compute --json > c:\temp.json
```

Peaksite nägema **availabilitySets** ühe **Microsoft.Compute**tüübid. Täielik nimi, tüüp on **Microsoft.Compute/availabilitySets**. Saate määrata mis tahes teie malli ressursside ressursinimi tüüp.

## <a name="public-ip"></a>Avaliku IP
Määratleda avaliku IP-aadressi. Vaata uuesti [REST API avaliku IP-aadresside](https://msdn.microsoft.com/library/azure/mt163590.aspx) atribuutide määramiseks.

```json
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/publicIPAddresses",
  "name": "[parameters('publicIPAddressName')]",
  "location": "[resourceGroup().location]",
  "properties": {
    "publicIPAllocationMethod": "Dynamic",
    "dnsSettings": {
      "domainNameLabel": "[parameters('dnsNameforLBIP')]"
    }
  }
}
```

Eraldatud meetod on seatud **dünaamiline** , kuid võib määramine väärtus peab teil või määrake selle parameetri väärtuse kinnitamiseks. Teil on lubatud läbida väärtus domeeni nimi sildi malli kasutajatele.

Nüüd vaatame, kuidas olete kindlaks teinud **apiVersion**. Lihtsalt vastab teie määratud väärtuse REST API, mida soovite kasutada, kui loote ressursi versiooni. Jah, saate vaadata selle ressursi tüüp REST API dokumentatsioonist. Või käivitage PowerShelli käsk teatud tüüpi.

```powershell
    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Network).ResourceTypes | Where-Object ResourceTypeName -eq publicIPAddresses).ApiVersions
```
Mis tagastab järgmised väärtused:

    2015-06-15
    2015-05-01-preview
    2014-12-01-preview

Azure'i CLI API versioone kuvamiseks kasutada eelnevalt sama **azure pakkuja kuvamine** käsu käivitamine.

Kui loote uue malli, valige API kõige uuem versioon.

## <a name="virtual-network-and-subnet"></a>Virtuaalse võrgu ja alamvõrgu
Saate luua ühe alamvõrgu virtuaalse võrgus. Vaadake [REST API virtuaalse võrkude](https://msdn.microsoft.com/library/azure/mt163661.aspx) kõigi atribuutide määramiseks.

```json
{
   "apiVersion": "2015-06-15",
   "type": "Microsoft.Network/virtualNetworks",
   "name": "[parameters('vnetName')]",
   "location": "[resourceGroup().location]",
   "properties": {
     "addressSpace": {
       "addressPrefixes": [
         "10.0.0.0/16"
       ]
     },
     "subnets": [
       {
         "name": "[variables('subnetName')]",
         "properties": {
           "addressPrefix": "10.0.0.0/24"
         }
       }
     ]
   }
}
```

## <a name="load-balancer"></a>Laadi koormusetasakaalustusteenuse
Nüüd loote välise suunatud laadi koormusetasakaalustusteenuse. Kuna seda laadi koormusetasakaalustusteenuse kasutab avaliku IP-aadressi, peab kinnitama sõltuvus, **ei leia dependsOn** jaotises avaliku IP-aadressi. See tähendab, et laadi koormusetasakaalustusteenuse ei saa juurutada, kuni avaliku IP-aadress on lõpule jõudnud, juurutamine. Ilma seda sõltuvust, kuvatakse tõrge Kuna ressursihaldur proovib ressursside paralleelselt juurutada ja proovib määratud laadi koormusetasakaalustusteenuse avaliku IP-aadress, mida pole veel olemas. 

Loote koos tcp juures port 80 selle ressursi mõiste ka kirjutamata aadress pool, paar sissetuleva NAT reeglid RDP VMs sisse ja koormust tasakaalustavad reegel. Väljamöllimise [REST API laadi koormusetasakaalustusteenuse](https://msdn.microsoft.com/library/azure/mt163574.aspx) kõik atribuudid.

```json
{
   "apiVersion": "2015-06-15",
   "name": "[parameters('lbName')]",
   "type": "Microsoft.Network/loadBalancers",
   "location": "[resourceGroup().location]",
   "dependsOn": [
     "[concat('Microsoft.Network/publicIPAddresses/', parameters('publicIPAddressName'))]"
   ],
   "properties": {
     "frontendIPConfigurations": [
       {
         "name": "LoadBalancerFrontEnd",
         "properties": {
           "publicIPAddress": {
             "id": "[variables('publicIPAddressID')]"
           }
         }
       }
     ],
     "backendAddressPools": [
       {
         "name": "BackendPool1"
       }
     ],
     "inboundNatRules": [
       {
         "name": "RDP-VM0",
         "properties": {
           "frontendIPConfiguration": {
             "id": "[variables('frontEndIPConfigID')]"
           },
           "protocol": "tcp",
           "frontendPort": 50001,
           "backendPort": 3389,
           "enableFloatingIP": false
         }
       },
       {
         "name": "RDP-VM1",
         "properties": {
           "frontendIPConfiguration": {
             "id": "[variables('frontEndIPConfigID')]"
           },
           "protocol": "tcp",
           "frontendPort": 50002,
           "backendPort": 3389,
           "enableFloatingIP": false
         }
       }
     ],
     "loadBalancingRules": [
       {
         "name": "LBRule",
         "properties": {
           "frontendIPConfiguration": {
             "id": "[variables('frontEndIPConfigID')]"
           },
           "backendAddressPool": {
             "id": "[variables('lbPoolID')]"
           },
           "protocol": "tcp",
           "frontendPort": 80,
           "backendPort": 80,
           "enableFloatingIP": false,
           "idleTimeoutInMinutes": 5,
           "probe": {
             "id": "[variables('lbProbeID')]"
           }
         }
       }
     ],
     "probes": [
       {
         "name": "tcpProbe",
         "properties": {
           "protocol": "tcp",
           "port": 80,
           "intervalInSeconds": 5,
           "numberOfProbes": 2
         }
       }
     ]
   }
}
```

## <a name="network-interface"></a>Võrgu liidese
Loote 2 võrgu liidesed, üks iga VM. Selle asemel, et kaasata Topeltkirjete võrgu liideste, saate [copyIndex() funktsioon](resource-group-create-multiple.md) itereerima üle Kopeeri kursis (nimetatakse nicLoop) ja luua arv võrgu liidesed määratletud funktsiooni `numberOfInstances` muutujat. Võrgu liidese sõltub virtuaalse võrgu ja laadi koormusetasakaalustusteenuse loomine. Laadi koormusetasakaalustusteenuse aadressi pool ja NAT sissetulevad reeglid konfigureerimiseks kasutab alamvõrku, virtuaalse võrgu loomiskuupäeva ja laadi koormusetasakaalustusteenuse id määratletud.
Vaadake [REST API võrgu liidesed](https://msdn.microsoft.com/library/azure/mt163668.aspx) kõik atribuudid.

```json
{
   "apiVersion": "2015-06-15",
   "type": "Microsoft.Network/networkInterfaces",
   "name": "[concat(parameters('nicNamePrefix'), copyindex())]",
   "location": "[resourceGroup().location]",
   "copy": {
     "name": "nicLoop",
     "count": "[variables('numberOfInstances')]"
   },
   "dependsOn": [
     "[concat('Microsoft.Network/virtualNetworks/', parameters('vnetName'))]",
     "[concat('Microsoft.Network/loadBalancers/', parameters('lbName'))]"
   ],
   "properties": {
     "ipConfigurations": [
       {
         "name": "ipconfig1",
         "properties": {
           "privateIPAllocationMethod": "Dynamic",
           "subnet": {
             "id": "[variables('subnetRef')]"
           },
           "loadBalancerBackendAddressPools": [
             {
               "id": "[concat(variables('lbID'), '/backendAddressPools/BackendPool1')]"
             }
           ],
           "loadBalancerInboundNatRules": [
             {
               "id": "[concat(variables('lbID'),'/inboundNatRules/RDP-VM', copyindex())]"
             }
           ]
         }
       }
     ]
   }
}
```

## <a name="virtual-machine"></a>Virtuaalse masina
Loote 2 virtuaalmasinates, funktsiooniga copyIndex(), nagu tegite [võrgu liidesed](#network-interface)loomine.
Salvestusruumi konto, sõltub VM loomine võrgu kasutajaliidese ja kättesaadavus. See VM luuakse turuplatsi pildilt, nagu on määratletud funktsiooni `storageProfile` atribuudi - `imageReference` pilt Publisheri, pakkumine, sku ja versiooni määratlemiseks kasutatakse. Lõpuks diagnostika profiili on konfigureeritud diagnostika lubamine VM. 

Oluline atribuutide turuplatsi pildi leidmiseks tehke artikleid [Valige Linux virtuaalse masina pilte](./virtual-machines/virtual-machines-linux-cli-ps-findimage.md) või [Windowsi virtuaalse masina pilte](./virtual-machines/virtual-machines-windows-cli-ps-findimage.md) .

```json
{
   "apiVersion": "2015-06-15",
   "type": "Microsoft.Compute/virtualMachines",
   "name": "[concat(parameters('vmNamePrefix'), copyindex())]",
   "copy": {
     "name": "virtualMachineLoop",
     "count": "[variables('numberOfInstances')]"
   },
   "location": "[resourceGroup().location]",
   "dependsOn": [
     "[concat('Microsoft.Storage/storageAccounts/', parameters('storageAccountName'))]",
     "[concat('Microsoft.Network/networkInterfaces/', parameters('nicNamePrefix'), copyindex())]",
     "[concat('Microsoft.Compute/availabilitySets/', variables('availabilitySetName'))]"
   ],
   "properties": {
     "availabilitySet": {
       "id": "[resourceId('Microsoft.Compute/availabilitySets',variables('availabilitySetName'))]"
     },
     "hardwareProfile": {
       "vmSize": "[parameters('vmSize')]"
     },
     "osProfile": {
       "computerName": "[concat(parameters('vmNamePrefix'), copyIndex())]",
       "adminUsername": "[parameters('adminUsername')]",
       "adminPassword": "[parameters('adminPassword')]"
     },
     "storageProfile": {
       "imageReference": {
         "publisher": "[parameters('imagePublisher')]",
         "offer": "[parameters('imageOffer')]",
         "sku": "[parameters('imageSKU')]",
         "version": "latest"
       },
       "osDisk": {
         "name": "osdisk",
         "vhd": {
           "uri": "[concat('http://',parameters('storageAccountName'),'.blob.core.windows.net/vhds/','osdisk', copyindex(), '.vhd')]"
         },
         "caching": "ReadWrite",
         "createOption": "FromImage"
       }
     },
     "networkProfile": {
       "networkInterfaces": [
         {
           "id": "[resourceId('Microsoft.Network/networkInterfaces',concat(parameters('nicNamePrefix'),copyindex()))]"
         }
       ]
     },
     "diagnosticsProfile": {
       "bootDiagnostics": {
          "enabled": "true",
          "storageUri": "[concat('http://',parameters('storageAccountName'),'.blob.core.windows.net')]"
       }
     }
}
```

>[AZURE.NOTE] Piltide avaldatud **3 tootja müüjad**, tuleb teil määrata mõne muu atribuudi nimega `plan`. Ühe näite leiate [selle malli](https://github.com/Azure/azure-quickstart-templates/tree/master/checkpoint-single-nic) Kiirjuhend galeriist. 

Lõpetamist määratlemine oma malli ressursse.

## <a name="parameters"></a>Parameetrid

Määratleda jaotises parameetrite väärtused, mida saab määrata juurutamisel mall. Ainult väärtused, mis teie arvates tuleb muuta juurutamisel jaoks parameetrite määratlemine. Vaikeväärtuse saab parameeter, mida kasutatakse siis, kui ei ole juurutamisel anda. Samuti saate määratleda lubatud väärtuste, nagu on näidatud **imageSKU** parameetri.

```json
"parameters": {
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Name of storage account"
      }
    },
    "adminUsername": {
      "type": "string",
      "metadata": {
        "description": "Admin username"
      }
    },
    "adminPassword": {
      "type": "securestring",
      "metadata": {
        "description": "Admin password"
      }
    },
    "dnsNameforLBIP": {
      "type": "string",
      "metadata": {
        "description": "DNS for Load Balancer IP"
      }
    },
    "vmNamePrefix": {
      "type": "string",
      "defaultValue": "myVM",
      "metadata": {
        "description": "Prefix to use for VM names"
      }
    },
    "imagePublisher": {
      "type": "string",
      "defaultValue": "MicrosoftWindowsServer",
      "metadata": {
        "description": "Image Publisher"
      }
    },
    "imageOffer": {
      "type": "string",
      "defaultValue": "WindowsServer",
      "metadata": {
        "description": "Image Offer"
      }
    },
    "imageSKU": {
      "type": "string",
      "defaultValue": "2012-R2-Datacenter",
      "allowedValues": [
        "2008-R2-SP1",
        "2012-Datacenter",
        "2012-R2-Datacenter"
      ],
      "metadata": {
        "description": "Image SKU"
      }
    },
    "lbName": {
      "type": "string",
      "defaultValue": "myLB",
      "metadata": {
        "description": "Load Balancer name"
      }
    },
    "nicNamePrefix": {
      "type": "string",
      "defaultValue": "nic",
      "metadata": {
        "description": "Network Interface name prefix"
      }
    },
    "publicIPAddressName": {
      "type": "string",
      "defaultValue": "myPublicIP",
      "metadata": {
        "description": "Public IP Name"
      }
    },
    "vnetName": {
      "type": "string",
      "defaultValue": "myVNET",
      "metadata": {
        "description": "VNET name"
      }
    },
    "vmSize": {
      "type": "string",
      "defaultValue": "Standard_D1",
      "metadata": {
        "description": "Size of the VM"
      }
    }
  }
```

## <a name="variables"></a>Muutujad

Jaotises muutujate saate määratleda väärtused, mida kasutatakse teie malli rohkem kui ühes kohas või väärtused, mis on valmistatud teiste avaldiste või muutujat. Muutujaid kasutatakse sageli lihtsustamiseks süntaks malli.

```json
"variables": {
    "availabilitySetName": "myAvSet",
    "subnetName": "Subnet-1",
    "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',parameters('vnetName'))]",
    "subnetRef": "[concat(variables('vnetID'),'/subnets/',variables ('subnetName'))]",
    "publicIPAddressID": "[resourceId('Microsoft.Network/publicIPAddresses',parameters('publicIPAddressName'))]",
    "numberOfInstances": 2,
    "lbID": "[resourceId('Microsoft.Network/loadBalancers',parameters('lbName'))]",
    "frontEndIPConfigID": "[concat(variables('lbID'),'/frontendIPConfigurations/LoadBalancerFrontEnd')]",
    "lbPoolID": "[concat(variables('lbID'),'/backendAddressPools/BackendPool1')]",
    "lbProbeID": "[concat(variables('lbID'),'/probes/tcpProbe')]"
  }
```

Mall on lõpule viidud! Saate võrrelda malli täielik malli [Kiirjuhend Galerii](https://github.com/Azure/azure-quickstart-templates) jaotises [2 VMs koormusetasakaalustusteenuse laadimine ja koormusetasakaalustusteenuse reeglid malli laadimine](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-loadbalancer-lbrules)suhtes. Malli võib olla veidi teistsugune põhjal, kasutades eri versioonide numbrid. 

Kasutasite salvestusruumi konto juurutamisel sama käskude abil saate juurutada uuesti mall. Teil pole vaja salvestusruumi konto kustutamine enne juurutamist uuesti, kuna ressursihaldur vahele uuesti loomise ressursse, mis on juba olemas ja on muutunud.

## <a name="next-steps"></a>Järgmised sammud

- [Azure'i ressursihaldur malli Visualizer (ARMViz)](http://armviz.io/#/) on suurepärane vahend visualiseerida ARM Mallid, nagu võivad olla liiga suur mõista ainult lugemiseks json-faili.
- Malli struktuuri kohta leiate lisateavet teemast [Azure ressursihaldur loome Mallid](resource-group-authoring-templates.md).
- Malli kasutamise kohta leiate teemast [Deploy ressursirühma Azure'i ressursihaldur malli abil](resource-group-template-deploy.md)
