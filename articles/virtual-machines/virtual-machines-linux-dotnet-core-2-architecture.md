<properties
   pageTitle="Juurutamine arvutada ressursid Azure ressursihaldur mallide abil | Microsoft Azure'i"
   description="Azure virtuaalse masina DotNet Core õpetus"
   services="virtual-machines-linux"
   documentationCenter="virtual-machines"
   authors="neilpeterson"
   manager="timlt"
   editor="tysonn"
   tags="azure-service-management"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="09/21/2016"
   ms.author="nepeters"/>

# <a name="application-architecture-with-azure-resource-manager-templates"></a>Rakenduse arhitektuur Azure'i ressursihaldur mallide kasutamine

On Azure ressursihaldur juurutamise väljatöötamisel Arvuta nõuded tuleb vastendada Azure ressursid ja teenused. Kui rakendus koosneb mitu http lõpp-punktid, andmebaasi ja andmete vahemällu talletamise teenuse, Azure ressursside majutavate iga nende komponentide peab olema ratsionaliseeritud. Näiteks sisaldab valimi muusika poe rakendus virtuaalse masina majutatud veebirakenduse ja SQL-andmebaasi, mis on majutatud Azure SQL andmebaasis. 

Selle dokumendi üksikasjad, ressursid muusika poe Arvuta konfiguratsioonist proovi Azure'i ressursihaldur mall. Kõik sõltuvused ja kordumatute konfiguratsioone on esile tõstetud. Parima kasutuskogemuse saamiseks eelnevalt juurutada eksemplari lahendus Azure tellimuse ja töö koos Azure ressursihaldur mall. Täielik malli leiate siit – [Muusika poe juurutamise Ubuntu](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).

## <a name="virtual-machine"></a>Virtuaalse masina

Muusika poe rakendus sisaldab veebirakenduse, kus kliendid saate sirvida ja osta muusikat. Kui on mitu Azure'i teenuste majutavad veebirakenduste selle näite puhul kasutatakse virtuaalse masina. Valimi muusika poe malli abil virtuaalse masina juurutatakse, veebiserverisse installida ja muusika poe veebisaidi installinud ja konfigureerinud. Pärast käesoleva artikli ainult virtuaalse masina juurutamise on üksikasjalikult. Veebiserver ja rakenduse konfigureerimine on üksikasjalikult hiljem artiklis.

Virtuaalse masina saab lisada malli Visual Studio lisada uue ressursi viisardi abil või lisades kehtiv JSON juurutamise mall. Virtuaalse masina juurutamisel mitme seotud ressursid on vaja. Kui Visual Studio abil saate luua malli, luuakse teie jaoks järgmisi ressursse. Kui käsitsi loomine malli, tuleb need ressursid lisada ja konfigureerida.

Seda linki JSON valimi ressursihaldur Mall – [Virtuaalse masina JSON](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L295)kuvamiseks.

```none
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Compute/virtualMachines",
  "name": "[concat(variables('vmName'),copyindex())]",
  "location": "[resourceGroup().location]",
  "copy": {
    "name": "virtualMachineLoop",
    "count": "[parameters('numberOfInstances')]"
  },
  "tags": {
    "displayName": "virtual-machine"
  },
  "dependsOn": [
    "[concat('Microsoft.Storage/storageAccounts/', variables('vhdStorageName'))]",
    "[concat('Microsoft.Compute/availabilitySets/', variables('availabilitySetName'))]",
    "nicLoop"
  ],
  "properties": {
    "availabilitySet": {
      "id": "[resourceId('Microsoft.Compute/availabilitySets', variables('availabilitySetName'))]"
    },
      ........<truncated>  
    }
```

Kui juurutada, virtuaalse masina atribuutide näha Azure'i portaalis.

![Virtuaalse masina](./media/virtual-machines-linux-dotnet-core/vm.png)

## <a name="storage-account"></a>Salvestusruumi konto

Salvestusruumi kontod on palju talletamise võimalused ja võimalusi. Azure'i virtuaalse masinad kontekstis, hoiab salvestusruumi konto virtuaalne kõvaketas virtuaalse masina ja mis tahes täiendavate andmete ketast. Muusika poe valimi sisaldab ühe salvestusruumi konto korraldada iga virtuaalse masina virtual kõvakettale juurutamise. 

Seda linki JSON valimi ressursihaldur Mall – [Salvestusruumi konto](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L109)kuvamiseks.


```none
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Storage/storageAccounts",
  "name": "[variables('vhdStorageName')]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "storage-account"
  },
  "properties": {
    "accountType": "[variables('vhdStorageType')]"
  }
}
```

Salvestusruumi konto on virtuaalse masina ressursihaldur malli deklareerimise virtuaalse masina sees. 

Seda linki kuvamiseks JSON valimi ressursihaldur Mall – [virtuaalse masina ja salvestusruumi konto seos](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L341).

```none
"osDisk": {
  "name": "osdisk",
  "vhd": {
    "uri": "[concat(reference(concat('Microsoft.Storage/storageAccounts/',variables('vhdStorageName')), '2015-06-15').primaryEndpoints.blob,'vhds/osdisk', copyindex(), '.vhd')]"
  },
  "caching": "ReadWrite",
  "createOption": "FromImage"
}
```

Pärast juurutuse salvestusruumi konto saab vaadata Azure'i portaalis.

![Salvestusruumi konto](./media/virtual-machines-linux-dotnet-core/storacct.png)

Klõpsates salvestusruumi konto bloobimälu ümbris, virtuaalse kõvaketta faili iga virtuaalse masina juurutatud malliga näha.

![Virtuaalne kõvaketas](./media/virtual-machines-linux-dotnet-core/vhd.png)

Azure'i säilitamise kohta lisateabe saamiseks leiate [Azure'i salvestusruumi dokumentatsioonist](https://azure.microsoft.com/documentation/services/storage/).

## <a name="virtual-network"></a>Virtuaalse võrgu

Kui virtuaalse masina nõuab sisemise võrgunduse, näiteks võimalust suhtlemine teiste virtuaalmasinates ja Azure ressursse, on Azure virtuaalse võrgu on nõutav.  Virtuaalse võrk ei hõlbustamine virtuaalse masina Interneti kaudu. Avaliku ühenduvuse jaoks on vaja avaliku IP-aadress, mis on üksikasjalikult allpool toodud selle sarja.

Seda linki JSON valimi ressursihaldur Mall – [virtuaalse võrgu ja alamvõrku](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L136)kuvamiseks.

```none
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/virtualNetworks",
  "name": "[variables('virtualNetworkName')]",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Network/networkSecurityGroups/', variables('networkSecurityGroup'))]"
  ],
  "tags": {
    "displayName": "virtual-network"
  },
  "properties": {
    "addressSpace": {
      "addressPrefixes": [
        "10.0.0.0/24"
      ]
    },
    "subnets": [
      {
        "name": "[variables('subnetName')]",
        "properties": {
          "addressPrefix": "10.0.0.0/24",
          "networkSecurityGroup": {
            "id": "[resourceId('Microsoft.Network/networkSecurityGroups', variables('networkSecurityGroup'))]"
          }
        }
      }
    ]
  }
}
```

Azure'i portaalis virtuaalse võrgu näeb järgmisel pildil. Pange tähele, et kõik virtuaalmasinates juurutatud malliga on lisatud virtuaalse võrgu.

![Virtuaalse võrgu](./media/virtual-machines-linux-dotnet-core/vnet.png)

## <a name="network-interface"></a>Võrgu liidese

 Võrgu liidese ühendab virtuaalse masina virtual võrgus, täpsemalt alamvõrku, mis on määratletud virtuaalse võrgu. 
 
 Seda linki JSON valimi ressursihaldur Mall – [Võrgu liidese](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L166)kuvamiseks.
 
```none
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/networkInterfaces",
  "name": "[concat(variables('networkInterfaceNamePrefix'), copyindex())]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "network-interface"
  },
  "copy": {
    "name": "nicLoop",
    "count": "[parameters('numberOfInstances')]"
  },
  "dependsOn": [
    "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]",
    "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]",
    "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIpAddressName'))]",
    "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'), '/inboundNatRules/', 'SSH-VM', copyIndex())]"
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
              "id": "[variables('lbPoolID')]"
            }
          ],
          "loadBalancerInboundNatRules": [
            {
              "id": "[concat(variables('lbID'),'/inboundNatRules/SSH-VM', copyIndex())]"
            }
          ]
        }
      }
    ]
  }
}
```

Iga virtuaalse masina ressursi sisaldab võrgu profiil. Võrgu liidese on seostatud virtuaalse masina selle profiili.  

Seda linki JSON valimi ressursihaldur Mall – [Virtuaalse masina võrgu profiili](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L350)vaatamiseks.


```none
"networkProfile": {
  "networkInterfaces": [
    {
      "id": "[resourceId('Microsoft.Network/networkInterfaces', concat(variables('networkInterfaceNamePrefix'), copyindex()))]"
    }
  ]
}
```

Azure'i portaalis võrgu liidese näeb järgmisel pildil. Sisemise IP-aadress ja virtuaalse masina seost näha võrgu kasutajaliidese ressurss.

![Võrgu liidese](./media/virtual-machines-linux-dotnet-core/nic.png)

Azure virtuaalse võrkude kohta lisateabe saamiseks leiate [Azure'i virtuaalse võrgu dokumentatsioonist](https://azure.microsoft.com/documentation/services/virtual-network/).

## <a name="azure-sql-database"></a>Azure'i SQL-andmebaas

Lisaks virtuaalse masina muusika poe veebisaiti majutada, juurutatakse Azure'i SQL-andmebaasi muusika poe andmebaasi. Virtuaalmasinates kogumi nõutakse, ja skaala ja -saadavus on teenuse sisse ehitatud, on see eelis, kasutades Azure'i SQL-andmebaasi siin.

Visual Studio lisada uue ressursi viisardi või lisades kehtiv JSON malli abil saab lisada Azure SQL-andmebaasi. SQL serveri ressursi sisaldab kasutajanime ja parooli, mis on antud administraatoriõigusi SQL-i eksemplari. Samuti lisatakse SQL-i tulemüüri ressursi. Vaikimisi saavad rakendused Azure SQL-i eksemplari suhtlemiseks. Luba välise rakenduse sellise SQL Server Management studio ühenduse SQL-i eksemplari, tulemüüri peab olema konfigureeritud. Muusika poe demo, vaikimisi konfiguratsioon on hea. 

Seda linki kuvamiseks JSON valimi – [DB Azure SQL-i](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L401)ressursihaldur mall.


```none
{
  "apiVersion": "2014-04-01-preview",
  "type": "Microsoft.Sql/servers",
  "name": "[variables('musicStoreSqlName')]",
  "location": "[resourceGroup().location]",

  "dependsOn": [],
  "tags": {
    "displayName": "sql-music-store"
  },
  "properties": {
    "administratorLogin": "[parameters('adminUsername')]",
    "administratorLoginPassword": "[parameters('sqlAdminPassword')]"
  },
  "resources": [
    {
      "apiVersion": "2014-04-01-preview",
      "type": "firewallrules",
      "name": "firewall-allow-azure",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Sql/servers/', variables('musicStoreSqlName'))]"
      ],
      "properties": {
        "startIpAddress": "0.0.0.0",
        "endIpAddress": "0.0.0.0"
      }
    }
  ]
}
```

SQL serveri ja MusicStore andmebaas nagu näha Azure portaali vaade.

![SQL serveri](./media/virtual-machines-linux-dotnet-core/sql.png)

Juurutamine Azure'i SQL-andmebaasi kohta lisateabe saamiseks leiate [Azure'i SQL-andmebaasi dokumentatsioonist](https://azure.microsoft.com/documentation/services/sql-database/).

## <a name="next-step"></a>Järgmise juhise juurde

<hr>

[Samm 2 - juurdepääsu ja turvalisuse Azure ressursihaldur Mallid](./virtual-machines-linux-dotnet-core-3-access-security.md)
