<properties
   pageTitle="Kättesaadavus ja Azure ressursihaldur korduvkasutatava skaala | Microsoft Azure'i"
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

# <a name="availability-and-scale-in-azure-resource-manager-templates"></a>Kättesaadavus ja Azure ressursihaldur korduvkasutatava skaala

Kättesaadavus ja mastaabi viidata sees ja võimaluse rahuldamiseks. Kui rakendus peab olema kuni 99,9% ajast, see peab olema arhitektuur, mis võimaldab mitme samaaegseid Arvuta ressursse. Näiteks sisaldab konfiguratsiooni kõrgema kättesaadavuse asemel ühe veebisaidi, mitu eksemplari ühes kohas, koos tasakaalustamiseks tehnoloogia nende ees. Selle konfiguratsiooni puhul ühe eksemplari rakendus võib võtta hooldus, kuigi ülejäänud funktsioon. Skaala viitab teiselt rakenduste võimalus Teeni nõudmisel. Koormus tasakaalustatud rakendus, lisamise või eemaldamise eksemplarid pool võimaldab rakenduse rahuldamiseks skaala.

Selle dokumendi üksikasjad muusika poe valimi juurutamise konfigureerimist ja skaala. Kõik sõltuvused ja kordumatute konfiguratsioone on esile tõstetud. Parima kasutuskogemuse saamiseks eelnevalt juurutada eksemplari lahendus Azure tellimuse ja töö koos Azure ressursihaldur mall. Täielik malli leiate siit – [Muusika poe juurutamise Windows](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).

## <a name="availability-set"></a>Kättesaadavus määramine

Azure'i Virtuaalmasinates kättesaadavus-seadmine ulatub loogiliselt füüsilise majutajate ja muud infrastruktuuri komponendid, nt power tarvikute ja füüsilise riistvara üle. Kättesaadavus komplekti tagada ajal hooldustööd, seadme tõrge või muud aega, mitte kõigis virtuaalmasinates, sooritatakse. Kättesaadavus-seadmine saate lisada Azure ressursihaldur malli, Visual Studio lisada uue ressursi viisardi abil või kehtiv JSON lisamine malli.

Seda linki JSON valimi ressursihaldur Mall- [Saadavus määramine](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L368)kuvamiseks.


```none
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Compute/availabilitySets",
  "name": "[variables('availabilitySetName')]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "availability-set"
  },
  "properties": {
  }
}
```

Atribuut virtuaalse masina ressursi tühjad kättesaadavus-seadmine. 

Seda linki JSON valimi ressursihaldur Mall – [kättesaadavus seost virtuaalse masina](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L302)kuvamiseks.

```none
"properties": {
  "availabilitySet": {
    "id": "[resourceId('Microsoft.Compute/availabilitySets', variables('availabilitySetName'))]"
  }
```
Nagu näha Azure portaali kättesaadavust. Siin on üksikasjalikult iga virtuaalse masina ja konfigureerimise üksikasju.

![Kättesaadavus määramine](./media/virtual-machines-windows-dotnet-core/ase-win.png)

Täpsemat teavet kättesaadavus komplekti, vt [haldamine virtuaalmasinates olemasolu](./virtual-machines-windows-manage-availability.md). 

## <a name="network-load-balancer"></a>Võrgu laadi koormusetasakaalustusteenuse

Mõne kättesaadavus Sea näeb rakenduse tõrketaluvust, laadi koormusetasakaalustusteenuse teeb paljudel juhtudel taotluse saadaval ühe võrgu aadress. Mitu rakenduse eksemplari saate majutatud palju virtuaalmasinates igaüht laadi koormusetasakaalustusteenuse ühendatud. Nagu rakendus on kättesaadav, laadi koormusetasakaalustusteenuse marsruutimine sissetuleva taotluse manustatud liikmed. Laadi koormusetasakaalustusteenuse saab lisada Visual Studio lisada uue ressursi viisardi abil või lisamisega õigesti vormindatud JSON ressursi Azure'i ressursihaldur malli.

Seda linki JSON valimi ressursihaldur Mall – [Võrgu laadi koormusetasakaalustusteenuse](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L198)kuvamiseks.

```none
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/loadBalancers",
  "name": "[variables('loadBalancerName')]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "load-balancer"
  },
  ........<truncated>
}
```

Kuna valimi rakendus on avatud Interneti avaliku IP-aadressiga, see aadress on seostatud laadi koormusetasakaalustusteenuse. 

Seda linki kuvamiseks JSON valimi ressursihaldur Mall – [võrgu laadi koormusetasakaalustusteenuse seos avaliku IP-aadressiga](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L211).

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

Azure'i portaalis kuvatakse võrgu laadi koormusetasakaalustusteenuse ülevaade koos avaliku IP-aadressi.

![Võrgu laadi koormusetasakaalustusteenuse](./media/virtual-machines-windows-dotnet-core/nlb-win.png)

## <a name="load-balancer-rule"></a>Laadi koormusetasakaalustusteenuse reegel

Laadi koormusetasakaalustusteenuse kasutamisel on konfigureeritud reegleid, et määrata, kuidas saabuv liiklus ettenähtud ressursid. Rakenduse valimi muusika poe liikluse avaliku IP-aadressi porti 80 ja levitatakse üle kõik virtuaalmasinates pordi 80. 

Seda linki JSON valimi ressursihaldur Mall – [Laadi koormusetasakaalustusteenuse reegli](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L226)kuvamiseks.


```none
"loadBalancingRules": [
  {
    "name": "[variables('loadBalencerRule')]",
    "properties": {
      "frontendIPConfiguration": {
        "id": "[concat(resourceId('Microsoft.Network/loadBalancers', variables('loadBalancerName')), '/frontendIPConfigurations/LoadBalancerFrontend')]"
      },
      "backendAddressPool": {
        "id": "[variables('lbPoolID')]"
      },
      "protocol": "Tcp",
      "frontendPort": 80,
      "backendPort": 80,
      "enableFloatingIP": false,
      "idleTimeoutInMinutes": 5,
      "probe": {
        "id": "[variables('lbProbeID')]"
      }
    }
  }
]
```

Vaate võrgu laadi koormusetasakaalustusteenuse reegel portaalist.

![Võrgu laadi koormusetasakaalustusteenuse reegel](./media/virtual-machines-windows-dotnet-core/lbrule-win.png)

## <a name="load-balancer-probe"></a>Laadi koormusetasakaalustusteenuse juures

Laadi koormusetasakaalustusteenuse peab iga virtuaalse masina jälgida, et taotlused kätte üksnes töötab süsteemid. See leiab aset pidev katsetamine eelmääratletud pordi. Muusika poe juurutusega on konfigureeritud probe kõigi kaasatud virtuaalmasinates porti 80. 

Seda linki JSON valimi ressursihaldur Mall – [Laadi koormusetasakaalustusteenuse Probe](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L247)kuvamiseks.


```none
"probes": [
  {
    "properties": {
      "protocol": "Tcp",
      "port": 80,
      "intervalInSeconds": 15,
      "numberOfProbes": 2
    },
    "name": "lbprobe"
  }
]
```

Laadi koormusetasakaalustusteenuse juures näha Azure portaali.

![Võrgu laadi koormusetasakaalustusteenuse juures](./media/virtual-machines-windows-dotnet-core/lbprobe-win.png)

## <a name="inbound-nat-rules"></a>Sissetulev NAT reeglid

Laadi koormusetasakaalustusteenuse kasutamisel tuleb reeglid tuleb kasutusele võtta iga virtuaalse masina-laadi pääsevad varustavate. Näiteks kui loomine iga virtuaalse masina RDP-ühendus, see liiklus ei tohiks koormus tasakaalustatud, pigem määratud tee peaks olema konfigureeritud. Määratud tee on konfigureeritud, kasutades on sissetulev reegli NAT ressurss. Selle ressursi abil sissetuleva side saab vastendada üksikud Virtuaalmasinates. 

Muusika poest rakenduse vastendatakse alates 5000 port port 3389 RDP juurdepääsu iga Virtual arvutis. Funktsiooni `copyindex()` funktsiooni kasutatakse sissetuleva pordi inkrementida teise virtuaalse masina saab sissetulev port, 5001 kolmanda 5002, mis rahuldab ja muudeks tegevusteks.

Seda linki kuvamiseks JSON valimi ressursihaldur Mall – [NAT sissetulevad reeglid](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L260). 

```none
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/loadBalancers/inboundNatRules",
  "name": "[concat(variables('loadBalancerName'), '/', 'RDP-VM', copyIndex())]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "load-balancer-nat-rule"
  },
  "copy": {
    "name": "lbNatLoop",
    "count": "[parameters('numberOfInstances')]"
  },
  "dependsOn": [
    "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]"
  ],
  "properties": {
    "frontendIPConfiguration": {
      "id": "[variables('ipConfigID')]"
    },
    "protocol": "tcp",
    "frontendPort": "[copyIndex(5000)]",
    "backendPort": 3389,
    "enableFloatingIP": false
  }
}
```

Üks näide sissetulev NAT reegel, nagu on näha Azure portaali. Reegli RDP NAT luuakse iga virtuaalse masina juurutuse.

![Sissetulev NAT reegel](./media/virtual-machines-windows-dotnet-core/natrule-win.png)

Täpsemat teavet Azure'i võrgu laadi koormusetasakaalustusteenuse, vt [koormusetasakaalustuseks Azure'i infrastruktuuri teenuste jaoks](./virtual-machines-windows-load-balance.md).

## <a name="deploy-multiple-vms"></a>Mitme VMs juurutamine

Lõpuks kättesaadavus või laadi koormusetasakaalustusteenuse tõhusalt toimida, mitme virtuaalmasinates on nõutav. Mitme VMs saab juurutada Azure ressursihaldur malli kopeerimine funktsiooni abil. Funktsiooni Kopeeri abil ei ole vaja määratlemiseks Virtuaalmasinates määratud hulga, pigem seda väärtust saab dünaamiliselt pakkuda juurutamise ajal. Funktsiooni Kopeeri tarbib loodavate eksemplaride arv ja pidemete juurutamine virtuaalmasinates ja seotud ressursid õige arv.

Muusika poe näidismall parameeter on määratletud mis on arvu. Kasutab seda numbrit kogu malli loomisel virtuaalmasinates ja seotud ressursid.

```none
"numberOfInstances": {
  "type": "int",
  "minValue": 1,
  "defaultValue": 2,
  "metadata": {
    "description": "Number of VM instances to be created behind load balancer."
  }
},
```

Virtuaalse masina ressurss, antakse Kopeeri tsükkel nimi ja eksemplari parameetri juhitakse tulemuseks eksemplaride arv arv.

Seda linki JSON valimi ressursihaldur Mall – [Virtuaalse masina Kopeeri funktsioon](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L290)kuvamiseks. 


```none
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Compute/virtualMachines",
  "name": "[concat(variables('vmName'),copyindex())]",
  "location": "[resourceGroup().location]",
  "copy": {
    "name": "virtualMachineLoop",
    "count": "[parameters('numberOfInstances')]"
  }
```

Funktsiooni Kopeeri praeguse iteratsiooni pääseb soovitud `copyIndex()` funktsioon. Funktsiooni index Kopeeri väärtus saab nime virtuaalmasinates ja muud ressursid. Näiteks kui kaks eksemplari virtuaalse masina on juurutatud, nad vajavad erinevaid nimesid. Funktsiooni `copyIndex()` funktsiooni saab kasutada virtuaalse masina nimi osana luua kordumatu nimi. Näide selle `copyindex()` funktsiooni kasutatakse nimede eesmärgil näha virtuaalse masina ressursi. Siin on arvuti nimi on ühendamine funktsiooni `vmName` parameeter, ja `copyIndex()` funktsioon. 

Seda linki JSON valimi ressursihaldur Mall – [Kopeeri funktsioon Index](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L309)kuvamiseks. 


```none
"osProfile": {
  "computerName": "[concat(variables('vmName'),copyindex())]",
  "adminUsername": "[parameters('adminUsername')]",
  "adminPassword": "[parameters('adminPassword')]"
}
```

Funktsiooni `copyIndex` funktsiooni kasutatakse korduvalt muusika poe näidismall. Ressursside ja funktsioonide, kasutades `copyIndex` kaasata midagi ühekordsest virtuaalse masina sellise ja võrgu liidese laadi koormusetasakaalustusteenuse reeglid ja mis tahes sõltub funktsioonid. 

Kopeeri funktsiooni kohta leiate lisateavet teemast [mitmes eksemplaris ressursside Azure'i ressursihaldur loomine](../resource-group-create-multiple.md).

## <a name="next-step"></a>Järgmise juhise juurde

<hr>

[Juhis 4 – rakenduse juurutamine Azure ressursihaldur mallide kasutamine](./virtual-machines-windows-dotnet-core-5-app-deployment.md)