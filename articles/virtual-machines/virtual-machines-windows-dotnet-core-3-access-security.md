<properties
   pageTitle="Juurdepääs ja turvalisuse Azure ressursihaldur Mallid | Microsoft Azure'i" 
   description="Azure virtuaalse masina DotNet Core õpetus"
   services="virtual-machines-windows"
   documentationCenter="virtual-machines"
   authors="neilpeterson"
   manager="timlt"
   editor="tysonn"
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="10/21/2016"
   ms.author="nepeters"/>

# <a name="access-and-security-in-azure-resource-manager-templates"></a>Juurdepääs ja turvalisuse Azure'i ressursihaldur Mallid

Azure tõenäoliselt rakendused peavad olema juurdepääs Interneti või VPN / teekonna ühenduse Azure. Muusika poe rakendus valimi, kus on veebisait kättesaadav internetis avaliku IP-aadressiga. Accessiga loodud olema kinnitatud rakenduse ühendused ja juurdepääsu virtuaalse masina ressursside ise. Accessi turbes? see on varustatud turberühma võrku. 

Selle dokumendi üksikasjad, kuidas muusika poe rakendus on turvatud valimi Azure'i ressursihaldur malli. Kõik sõltuvused ja kordumatute konfiguratsioone on esile tõstetud. Parima kasutuskogemuse saamiseks eelnevalt juurutada eksemplari lahendus Azure tellimuse ja töö koos Azure ressursihaldur mall. Täielik malli leiate siit – [Muusika poe juurutamise Windows](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).


## <a name="public-ip-address"></a>Avaliku IP-aadress

Azure'i ressursside avaliku juurdepääsu andmiseks saate kasutada avaliku IP-aadressi ressursi. Avaliku IP-aadressi saab konfigureerida staatiline või dünaamiline IP-aadress. Kui kasutatakse dünaamilise aadressi ja virtuaalse masina on peatatud ja deallocated, aadressid on eemaldatud. Kui arvuti käivitatakse uuesti, võib määratakse erinevate avaliku IP-aadress. IP-aadress takistada muutmist, saab reserveeritud IP-aadress. 

Azure'i ressursihaldur Mall, mis Visual Studio lisada uue ressursi viisardi abil või lisades kehtiv JSON malli saab lisada avaliku IP-aadressi. 

Seda linki kuvamiseks JSON valimi ressursihaldur Mall – [Avaliku IP-aadress](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L110).


```none
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/publicIPAddresses",
  "name": "[variables('publicIpAddressName')]",
  "location": "[resourceGroup().location]",
  "dependsOn": [],
  "tags": {
    "displayName": "public-ip"
  },
  "properties": {
    "publicIPAllocationMethod": "Dynamic",
    "dnsSettings": {
      "domainNameLabel": "[parameters('publicipaddressDnsName')]"
    }
  }
}
```

Avaliku IP-aadress võib olla seotud virtuaalse võrguadapteri või laadi koormusetasakaalustusteenuse. Selles näites Kuna muusika poe veebisaidi on mitu virtuaalmasinates, lõikes laadi avaliku IP-aadress on lisatud koormusetasakaalustusteenuse laadimine.

Seda linki JSON valimi ressursihaldur Mall – [avaliku IP-aadressi seos laadi koormusetasakaalustusteenuse](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L211)kuvamiseks.

```none
"frontendIPConfigurations": [
  {
    "properties": {
      "publicIPAddress": {
        "id": "[resourceId('Microsoft.Network/publicIPAddresses', variables('publicIpAddressName'))]"
      }
    },
    "name": "LoadBalancerFrontend"
  }
]
```

Avaliku IP-aadressi nagu on näha Azure'i portaalis. Pange tähele, et avaliku IP-aadress on seostatud laadi koormusetasakaalustusteenuse ja mitte virtuaalse masina. Võrgu koormus soolise on üksikasjalikult kirjeldatud järgmise dokumendi selle sarja.

![Avaliku IP-aadress](./media/virtual-machines-windows-dotnet-core/pubip-win.png)

Azure'i avaliku IP-aadresside kohta leiate lisateavet teemast [Azure IP-aadressid](../virtual-network/virtual-network-ip-addresses-overview-arm.md).

## <a name="network-security-group"></a>Võrgu turberühm

Kui access on loodud Azure ressursse, juurdepääsu tuleks piirata. Azure'i virtuaalmasinates, jaoks turvaline juurdepääs saab teha turberühma võrgu kaudu. Proovi muusika poe rakenduse koos virtuaalse masina kõigi juurdepääs on piiratud, välja arvatud http juurdepääsu pordi 80 ja pordi 3389 RDP juurdepääsu üle. Võrgu turberühma saab lisada Azure ressursihaldur Mall, mis Visual Studio lisada uue ressursi viisardi abil või lisades kehtiv JSON malli.

Seda linki JSON valimi ressursihaldur Mall – [Võrgu turberühma](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L57)kuvamiseks.

```none
{
  "apiVersion": "2016-03-30",
  "type": "Microsoft.Network/networkSecurityGroups",
  "name": "[variables('networkSecurityGroup')]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "network-security-group"
  },
  "properties": {
    "securityRules": [
      {
        "name": "http",
        "properties": {
          "description": "http endpoint",
          "protocol": "Tcp",
          "sourcePortRange": "*",
          "destinationPortRange": "80",
          "sourceAddressPrefix": "*",
          "destinationAddressPrefix": "*",
          "access": "Allow",
          "priority": 100,
          "direction": "Inbound"
        }
      },
      ........<truncated> 
    ]
  }
},
```

Selles näites võrgu turberühma on virtuaalse võrguressurssi deklareeritud alamvõrgu objekti. 

Seda linki JSON valimi ressursihaldur Mall – [võrgu turberühma seos virtuaalse võrguga](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L143)kuvamiseks.


```none
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
```

Siin on võrgu turberühma näeb Azure portaalist. Teate, mis on NSG saate seostada alamvõrgu ja / või võrgu kasutajaliidese. Sel juhul on NSG on seostatud mõne alamvõrgu. Selle konfiguratsiooni puhul rakendada kõigi ühendatud alamvõrku virtuaalmasinates sissetulevad reeglid.

![Võrgu turberühm](./media/virtual-machines-windows-dotnet-core/nsg-win.png)

Täpsemat teavet võrgu turberühmad, vaadake teemat [mis on võrgu turberühma]( https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/).

## <a name="next-step"></a>Järgmise juhise juurde

<hr>

[Samm 3 - saadavus ja Azure ressursihaldur korduvkasutatava skaala](./virtual-machines-windows-dotnet-core-4-availability-scale.md)
