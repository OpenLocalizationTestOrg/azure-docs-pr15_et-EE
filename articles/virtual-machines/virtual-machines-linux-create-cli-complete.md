
<properties
   pageTitle="Täielik Linuxi keskkond Azure'i CLI abil luua | Microsoft Azure'i"
   description="Salvestusruumi, Linux VM, virtuaalse võrgu ja alamvõrgu, laadi koormusetasakaalustusteenuse, mis NIC, avaliku IP ja võrgu turberühma, kõik kaudu, kasutades Azure CLI kohalt loomine."
   services="virtual-machines-linux"
   documentationCenter="virtual-machines"
   authors="iainfoulds"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="10/24/2016"
   ms.author="iainfou"/>

# <a name="create-a-complete-linux-environment-by-using-the-azure-cli"></a>Azure'i CLI abil luua täieliku Linux keskkonnas

Selles artiklis me luua lihtsa võrgu laadi koormusetasakaalustusteenuse ja paari vms, mis sobivad arendamise ja lihtsate arvutuste jaoks. Tutvustame protsessi käsk käsk, kuni olete kaks töö, turvalist Linux VMs millele abil saate luua ühenduse suvalist Internetis. Seejärel saate liikuda keerukamaid võrgu ja keskkonnas.

Teel olles õpite tundma sõltuvus hierarhia ressursihaldur juurutamise mudeli annab teile, ja kui palju power pakub. Kui näete, kuidas funktsiooni on loodud, saate selle [Azure'i ressursihaldur mallide](../resource-group-authoring-templates.md)abil palju kiiremini taastada. Ka pärast seda, kui saate teada, kuidas teie keskkonnas osad kokku sobivad, automatiseerimiseks need mallide loomise muutub lihtsamaks.

Keskkonna sisaldab:

- Kahe VMs sees on kättesaadavus määramine.
- Laadi koormusetasakaalustusteenuse koormust tasakaalustavad port 80 reegli abil.
- Võrgu turvalisus jaotises (NSG) reegleid kaitsmine soovimatute liikluse oma VM.

![Tavaline keskkonna ülevaade](./media/virtual-machines-linux-create-cli-complete/environment_overview.png)

Selle kohandatud keskkonna loomiseks peate uusima [Azure'i CLI](../xplat-cli-install.md) ressursihaldur režiimis (`azure config mode arm`). Samuti peate mõne JSON sõelumine tööriista. Selles näites kasutatakse [jq](https://stedolan.github.io/jq/).

## <a name="quick-commands"></a>Kiirülevaate käsud
Kui teil on vaja kiiresti täita tööülesanne, jaotis järgmisega alus vajalikke käske üles laadida VM Azure. Üksikasjalikumat teavet ja konteksti jaoks leiate selle dokumendi ülejäänud iga toimingu käivitamine [siin](#detailed-walkthrough).

Veenduge, et olete sisse logitud ja ressursihaldur režiimi kasutamine [Azure CLI](../xplat-cli-install.md) .

```bash
azure config mode arm
```

Järgmistes näidetes asendamine oma väärtustega näide parameetrite nimed. Näide parameetrite nimed sisaldavad `myResourceGroup`, `mystorageaccount`, ja `myVM`.

Looge ressursirühma. Järgmises näites luuakse ressursirühma nimega `myResourceGroup` klõpsake soovitud `westeurope` asukoht:

```bash
azure group create -n myResourceGroup -l westeurope
```

JSON-parseri abil kontrollida ressursirühma.

```bash
azure group show myResourceGroup --json | jq '.'
```

Looge konto salvestusruumi. Järgmises näites luuakse salvestusruumi konto nimega `mystorageaccount` (salvestusruumikonto nimi peab olema kordumatu, et anda oma kordumatu nimi):

```bash
azure storage account create -g myResourceGroup -l westeurope \
  --kind Storage --sku-name GRS mystorageaccount
```

Kontrollige konto salvestusruumi JSON-parseri abil:

```bash
azure storage account show -g myResourceGroup mystorageaccount --json | jq '.'
```

Saate luua virtuaalse võrgu. Järgmises näites luuakse virtuaalse võrgu nimega `myVnet`:

```bash
azure network vnet create -g myResourceGroup -l westeurope\
  -n myVnet -a 192.168.0.0/16
```

Saate luua mõne alamvõrgu. Järgmises näites luuakse alamvõrku, mis on nimega `mySubnet`"

```bash
azure network vnet subnet create -g myResourceGroup \
  -e myVnet -n mySubnet -a 192.168.1.0/24
```

Kontrollige üle virtuaalse võrgu ja alamvõrgu JSON-parseri abil:


```bash
azure network vnet show myResourceGroup myVnet --json | jq '.'
```

Saate luua avaliku IP. Järgmises näites luuakse nimega avaliku IP `myPublicIP` DNS-i nimi `mypublicdns` (DNS-i nimi peab olema kordumatu, et anda oma kordumatu nimi):

```bash
azure network public-ip create -g myResourceGroup -l westeurope \
  -n myPublicIP  -d mypublicdns -a static -i 4
```

Laadi koormusetasakaalustusteenuse luua. Järgmises näites luuakse laadi koormusetasakaalustusteenuse, nimega `myLoadBalancer`:

```bash
azure network lb create -g myResourceGroup -l westeurope -n myLoadBalancer
```

Looge ees IP pool laadi koormusetasakaalustusteenuse jaoks ja seostada avaliku IP. Järgmises näites luuakse ees IP pool nimega `mySubnetPool`:

```bash
azure network lb frontend-ip create -g myResourceGroup -l myLoadBalancer \
  -i myPublicIP -n myFrontEndPool 
```

Looge tagaandmebaas IP pool laadi koormusetasakaalustusteenuse jaoks. Järgmises näites luuakse tagaandmebaas IP pool nimega `myBackEndPool`:

```bash
azure network lb address-pool create -g myResourceGroup -l myLoadBalancer \
  -n myBackEndPool
```

Saate luua SSH sissetuleva võrgu aadress tõlge (NAT) reeglid laadi koormusetasakaalustusteenuse. Järgmises näites luuakse kahe laadi koormusetasakaalustusteenuse reegleid, `myLoadBalancerRuleSSH1` ja `myLoadBalancerRuleSSH2`:

```bash
azure network lb inbound-nat-rule create -g myResourceGroup -l myLoadBalancer \
  -n myLoadBalancerRuleSSH1 -p tcp -f 4222 -b 22
azure network lb inbound-nat-rule create -g myResourceGroup -l myLoadBalancer \
  -n myLoadBalancerRuleSSH2 -p tcp -f 4223 -b 22
```

Veebirakenduse loomine sissetulev laadi koormusetasakaalustusteenuse NAT reeglid. Järgmises näites luuakse laadi koormusetasakaalustusteenuse reegli nimega`myLoadBalancerRuleWeb`

```bash
azure network lb rule create -g myResourceGroup -l myLoadBalancer \
  -n myLoadBalancerRuleWeb -p tcp -f 80 -b 80 \
  -t myFrontEndPool -o myBackEndPool
```

Laadi koormusetasakaalustusteenuse seisundi juures luua. Järgmises näites luuakse TCP juures, nimega `myHealthProbe`:

```bash
azure network lb probe create -g myResourceGroup -l myLoadBalancer \
  -n myHealthProbe -p "tcp" -i 15 -c 4
```

Laadi koormusetasakaalustusteenuse, IP kaustu ja NAT reeglite abil JSON-parseri kontrollimine

```bash
azure network lb show -g myResourceGroup -n myLoadBalancer --json | jq '.'
```

Luua esimese võrgu kasutajaliidese kaardi (NIC). Asendada selle `#####-###-###` jaotiste oma Azure tellimuse ID-ga. Tellimuse ID on märgitud väljund `jq` loote ressursside uurimisel. Saate vaadata oma tellimuse ID, mille `azure account list`. 

Järgmises näites luuakse NIC, mis on nimega `myNic1`:

```bash
azure network nic create -g myResourceGroup -l westeurope \
  -n myNic1 -m myVnet -k mySubnet \
  -d "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool" \
  -e "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1"
```

Looge teine NIC. Järgmises näites luuakse NIC, mis on nimega `myNic2`:

```bash
azure network nic create -g myResourceGroup -l westeurope \
  -n myNic2 -m myVnet -k mySubnet \
  -d "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool" \
  -e "/subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2"
```

Kontrollige kaks NICs JSON-parseri abil:

```bash
azure network nic show myResourceGroup myNic1 --json | jq '.'
azure network nic show myResourceGroup myNic2 --json | jq '.'
```

Võrgu Turberühma loomine. Järgmises näites luuakse võrgu turberühma nimega `myNetworkSecurityGroup`:

```bash
azure network nsg create -g myResourceGroup -l westeurope \
  -n myNetworkSecurityGroup
```

Lisage kaks sissetulevad reeglid võrgu turberühma. Järgmises näites luuakse kaks reegleid, `myNetworkSecurityGroupRuleSSH` ja `myNetworkSecurityGroupRuleHTTP`:

```bash
azure network nsg rule create -p tcp -r inbound -y 1000 -u 22 -c allow \
  -g myResourceGroup -a myNetworkSecurityGroup -n myNetworkSecurityGroupRuleSSH
azure network nsg rule create -p tcp -r inbound -y 1001 -u 80 -c allow \
  -g myResourceGroup -a myNetworkSecurityGroup -n myNetworkSecurityGroupRuleHTTP
```

Kontrollige üle võrgu turberühma ja sissetulevad reeglid JSON-parseri abil:

```bash
azure network nsg show -g myResourceGroup -n myNetworkSecurityGroup --json | jq '.'
```

Turberühma võrgu sidumiseks kaks NICs:

```bash
azure network nic set -g myResourceGroup -o myNetworkSecurityGroup -n myNic1
azure network nic set -g myResourceGroup -o myNetworkSecurityGroup -n myNic2
```

Looge kättesaadavus määramine. Järgmises näites luuakse seadmine nimega saadavus `myAvailabilitySet`:

```bash
azure availset create -g myResourceGroup -l westeurope -n myAvailabilitySet
```

Saate luua esimese Linux VM. Järgmises näites luuakse nimega VM `myVM1`:

```bash
azure vm create \
    --resource-group myResourceGroup \
    --name myVM1 \
    --location westeurope \
    --os-type linux \
    --availset-name myAvailabilitySet \
    --nic-name myNic1 \
    --vnet-name myVnet \
    --vnet-subnet-name mySubnet \
    --storage-account-name mystorageaccount \
    --image-urn canonical:UbuntuServer:16.04.0-LTS:latest \
    --ssh-publickey-file ~/.ssh/id_rsa.pub \
    --admin-username ops
```

Looge teine Linux VM. Järgmises näites luuakse nimega VM `myVM2`:

```bash
azure vm create \
    --resource-group myResourceGroup \
    --name myVM2 \
    --location westeurope \
    --os-type linux \
    --availset-name myAvailabilitySet \
    --nic-name myNic2 \
    --vnet-name myVnet \
    --vnet-subnet-name mySubnet \
    --storage-account-name mystorageaccount \
    --image-urn canonical:UbuntuServer:16.04.0-LTS:latest \
    --ssh-publickey-file ~/.ssh/id_rsa.pub \
    --admin-username ops
```

JSON-parseri kasutamine veendumaks, et kõik, mis on ehitatud:

```bash
azure vm show -g myResourceGroup -n myVM1 --json | jq '.'
azure vm show -g myResourceGroup -n myVM2 --json | jq '.'
```

Uue keskkonna eksportimiseks malli abil kiiresti luua uue eksemplari uuesti:

```bash
azure group export myResourceGroup
```

## <a name="detailed-walkthrough"></a>Üksikasjaliku selgituse
Üksikasjalikud juhised, mis selgitavad, mida iga käsu teeb, nagu te rajamise keskkonna. Need on kasulik, kui koostate arengu või oma kohandatud keskkonnas.

Veenduge, et olete sisse logitud ja ressursihaldur režiimi kasutamine [Azure CLI](../xplat-cli-install.md) .

```bash
azure config mode arm
```

Järgmistes näidetes asendamine oma väärtustega näide parameetrite nimed. Näide parameetrite nimed sisaldavad `myResourceGroup`, `mystorageaccount`, ja `myVM`.

## <a name="create-resource-groups-and-choose-deployment-locations"></a>Ressursi rühmade loomine ja valige juurutamise asukohad

Azure'i ressursi rühmad on loogiline juurutamise üksused, mis sisaldavad konfiguratsiooniteavet ja metaandmete lubamiseks ressursi juurutuste loogilise haldamine. Järgmises näites luuakse ressursirühma nimega `myResourceGroup` klõpsake soovitud `westeurope` asukoht:

```bash
azure group create --name myResourceGroup --location westeurope
```

Väljund:

```bash                        
info:    Executing command group create
+ Getting resource group myResourceGroup
+ Creating resource group myResourceGroup
info:    Created resource group myResourceGroup
data:    Id:                  /subscriptions/guid/resourceGroups/myResourceGroup
data:    Name:                myResourceGroup
data:    Location:            westeurope
data:    Provisioning State:  Succeeded
data:    Tags: null
data:
info:    group create command OK
```

## <a name="create-a-storage-account"></a>Salvestusruumi konto loomine

Teil on vaja salvestusruumi kontod oma VM ketast ja mis tahes täiendavate andmete ketast, mida soovite lisada. Saate luua salvestusruumi kontod peaaegu kohe pärast loomist ressursi rühmad.

Siin kasutame funktsiooni `azure storage account create` käsu läbimise konto, ressursirühma asukoht, mis määrab ja tüübi talletusruumi tugi soovite. Järgmises näites luuakse salvestusruumi konto nimega `mystorageaccount`:

```bash
azure storage account create \  
  --location westeurope \
  --resource-group myResourceGroup \
  --kind Storage --sku-name GRS \
  mystorageaccount
```

Väljund:

```bash
info:    Executing command storage account create
+ Creating storage account
info:    storage account create command OK
```

Meie ressursirühm uurida, kasutades funktsiooni `azure group show` käsk, kasutame [jq](https://stedolan.github.io/jq/) tööriist koos soovitud `--json` Azure CLI suvand. (Saate kasutada **jsawk** või eelistate sõeluda selle JSON keele teegist.)

```bash
azure group show myResourceGroup --json | jq '.'
```

Väljund:

```bash
{
  "tags": {},
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup",
  "name": "myResourceGroup",
  "provisioningState": "Succeeded",
  "location": "westeurope",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "resources": [
    {
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
      "name": "mystorageaccount",
      "type": "storageAccounts",
      "location": "westeurope",
      "tags": null
    }
  ],
  "permissions": [
    {
      "actions": [
        "*"
      ],
      "notActions": []
    }
  ]
}
```

Salvestusruumi konto abil CLI uurimiseks peate esmalt konto nimede ja võtmed. Asendage salvestusruumi konto järgmises näites nimi nimi, mille valimisel:

```
AZURE_STORAGE_CONNECTION_STRING="$(azure storage account connectionstring show mystorageaccount --resource-group myResourceGroup --json | jq -r '.string')"
```

Seejärel saate vaadata oma salvestusruumi teavet hõlpsalt:

```bash
azure storage container list
```

Väljund:

```bash
info:    Executing command storage container list
+ Getting storage containers
data:    Name  Public-Access  Last-Modified
data:    ----  -------------  -----------------------------
data:    vhds  Off            Sun, 27 Sep 2015 19:03:54 GMT
info:    storage container list command OK
```

## <a name="create-a-virtual-network-and-subnet"></a>Virtuaalse võrgu ja alamvõrgu loomine

Järgmine teil läheb vaja luua virtuaalse võrgu Azure ja on alamvõrku, kus saate luua oma VMs töötab. Järgmises näites luuakse virtuaalse võrgu nimega `myVnet` abil soovitud `192.168.0.0/16` aadress eesliide:

```bash
azure network vnet create --resource-group myResourceGroup --location westeurope \
  --name myVnet --address-prefixes 192.168.0.0/16 
```

Väljund:

```bash
info:    Executing command network vnet create
+ Looking up virtual network "myVnet"
+ Creating virtual network "myVnet"
+ Loading virtual network state
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet
data:    Name                            : myVnet
data:    Type                            : Microsoft.Network/virtualNetworks
data:    Location                        : westeurope
data:    ProvisioningState               : Succeeded
data:    Address prefixes:
data:      192.168.0.0/16
info:    network vnet create command OK
```

Uuesti kasutame--json võimalus `azure group show` ja **jq** , et näha, kuidas me hoone oma ressursse. Nüüd on `storageAccounts` ressurss ja `virtualNetworks` ressursi.  

```bash
azure group show myResourceGroup --json | jq '.'
```

Väljund:

```bash
{
  "tags": {},
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup",
  "name": "myResourceGroup",
  "provisioningState": "Succeeded",
  "location": "westeurope",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "resources": [
    {
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet",
      "name": "myVnet",
      "type": "virtualNetworks",
      "location": "westeurope",
      "tags": null
    },
    {
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
      "name": "mystorageaccount",
      "type": "storageAccounts",
      "location": "westeurope",
      "tags": null
    }
  ],
  "permissions": [
    {
      "actions": [
        "*"
      ],
      "notActions": []
    }
  ]
}
```

Nüüd on alamvõrgu loomine soovitud `myVnet` virtuaalse võrgu, millesse on juurutatud VMs. Kasutame funktsiooni `azure network vnet subnet create` käsk koos oleme juba loonud ressursside: selle `myResourceGroup` ressursirühm ja `myVnet` virtuaalse võrgu. Järgmises näites lisame nimega alamvõrgu `mySubnet` , eesliitega alamvõrgu aadress `192.168.1.0/24`:

```bash
azure network vnet subnet create --resource-group myResourceGroup \
  --vnet-name myVnet --name mySubnet --address-prefix 192.168.1.0/24
```

Väljund:

```bash
info:    Executing command network vnet subnet create
+ Looking up the subnet "mySubnet"
+ Creating subnet "mySubnet"
+ Looking up the subnet "mySubnet"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet
data:    Type                            : Microsoft.Network/virtualNetworks/subnets
data:    ProvisioningState               : Succeeded
data:    Name                            : mySubnet
data:    Address prefix                  : 192.168.1.0/24
data:
info:    network vnet subnet create command OK
```

Kuna alamvõrgu on loogiliselt sees virtuaalse võrgus, vaatame veidi teistsugused käsuga alamvõrgu teave. Kasutame käsk on `azure network vnet show`, kuid jätkame saabuvate JSON väljundi **jq**abil.

```bash
azure network vnet show myResourceGroup myVnet --json | jq '.'
```

Väljund:

```bash
{
  "subnets": [
    {
      "ipConfigurations": [],
      "addressPrefix": "192.168.1.0/24",
      "provisioningState": "Succeeded",
      "name": "mySubnet",
      "etag": "W/\"974f3e2c-028e-4b35-832b-a4b16ad25eb6\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet"
    }
  ],
  "tags": {},
  "addressSpace": {
    "addressPrefixes": [
      "192.168.0.0/16"
    ]
  },
  "dhcpOptions": {
    "dnsServers": []
  },
  "provisioningState": "Succeeded",
  "etag": "W/\"974f3e2c-028e-4b35-832b-a4b16ad25eb6\"",
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet",
  "name": "myVnet",
  "location": "westeurope"
}
```

## <a name="create-a-public-ip-address-pip"></a>Avaliku IP-aadress (PIP) loomine

Nüüd avaliku IP-aadress (PIP) anname oma laadi koormusetasakaalustusteenuse loomine. Selle abil saate luua ühenduse oma vms Interneti kaudu, kasutades funktsiooni `azure network public-ip create` käsk. Kuna vaikeaadressi on dünaamiline, saame luua nimega DNS-i kirje **cloudapp.azure.com** domeen, kasutades funktsiooni `--domain-name-label` suvand. Järgmises näites luuakse nimega avaliku IP `myPublicIP` DNS-i nimi `mypublicdns`. Kui DNS-i nimi peab olema kordumatu, sisestage oma DNS-i kordumatu nimi.

```bash
azure network public-ip create --resource-group myResourceGroup \
  --location westeurope --name myPublicIP --domain-name-label mypublicdns
```

Väljund:

```bash
info:    Executing command network public-ip create
+ Looking up the public ip "myPublicIP"
+ Creating public ip address "myPublicIP"
+ Looking up the public ip "myPublicIP"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP
data:    Name                            : myPublicIP
data:    Type                            : Microsoft.Network/publicIPAddresses
data:    Location                        : westeurope
data:    Provisioning state              : Succeeded
data:    Allocation method               : Dynamic
data:    Idle timeout                    : 4
data:    Domain name label               : mypublicdns
data:    FQDN                            : mypublicdns.westeurope.cloudapp.azure.com
info:    network public-ip create command OK
```

Avaliku IP-aadress on ka ülataseme ressurssi, nii et näete seda `azure group show`.

```bash
azure group show myResourceGroup --json | jq '.'
```

Väljund:

```bash
{
"tags": {},
"id": "/subscriptions/guid/resourceGroups/myResourceGroup",
"name": "myResourceGroup",
"provisioningState": "Succeeded",
"location": "westeurope",
"properties": {
    "provisioningState": "Succeeded"
},
"resources": [
    {
    "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP",
    "name": "myPublicIP",
    "type": "publicIPAddresses",
    "location": "westeurope",
    "tags": null
    },
    {
    "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet",
    "name": "myVnet",
    "type": "virtualNetworks",
    "location": "westeurope",
    "tags": null
    },
    {
    "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount",
    "name": "mystorageaccount",
    "type": "storageAccounts",
    "location": "westeurope",
    "tags": null
    }
],
"permissions": [
    {
    "actions": [
        "*"
    ],
    "notActions": []
    }
]
}
```

Saate uurida ressursi üksikasjade, näiteks alamdomeeni, täielik domeeninimi (FQDN), kasutades täieliku `azure network public-ip show` käsk. Avaliku IP-aadressi ressursi on eraldatud loogiliselt, kuid konkreetne aadress pole veel määratud. IP-aadressi saamiseks teil läheb vaja koormusetasakaalustusteenuse laadi, mida me ei ole veel loodud.

```bash
azure network public-ip show myResourceGroup myPublicIP --json | jq '.'
```

Väljund:

```bash
{
"tags": {},
"publicIpAllocationMethod": "Dynamic",
"dnsSettings": {
    "domainNameLabel": "mypublicdns",
    "fqdn": "mypublicdns.westeurope.cloudapp.azure.com"
},
"idleTimeoutInMinutes": 4,
"provisioningState": "Succeeded",
"etag": "W/\"c63154b3-1130-49b9-a887-877d74d5ebc5\"",
"id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP",
"name": "myPublicIP",
"location": "westeurope"
}
```

## <a name="create-a-load-balancer-and-ip-pools"></a>Laadi koormusetasakaalustusteenuse ja IP kaustu loomine
Laadi koormusetasakaalustusteenuse loomisel selle abil saate levitada liikluse üle mitme VMs. See sisaldab ka rakenduse koondamise käivitades vastata kasutaja hooldustööd või raske laadimise mitme VMs. Järgmises näites luuakse laadi koormusetasakaalustusteenuse, nimega `myLoadBalancer`:

```bash
azure network lb create --resource-group myResourceGroup --location westeurope \
  --name myLoadBalancer
```

Väljund:

```bash
info:    Executing command network lb create
+ Looking up the load balancer "myLoadBalancer"
+ Creating load balancer "myLoadBalancer"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer
data:    Name                            : myLoadBalancer
data:    Type                            : Microsoft.Network/loadBalancers
data:    Location                        : westeurope
data:    Provisioning state              : Succeeded
info:    network lb create command OK
```

Meie laadi koormusetasakaalustusteenuse on üsna tühi, seega vaatame luua mõne IP. Soovime luua kaks IP meie laadi koormusetasakaalustusteenuse – üks ots ja üks tagasi lõpuks jaoks. Ees IP pool on avalikult nähtav. Samuti on asukoht, kuhu anname PIP, mida me varem loodud. Siis kasutame tagaandmebaas rakenduskausta meie VMs asukohaks ühenduse loomiseks. Nii liiklus saate flow laadi koormusetasakaalustusteenuse vms kaudu.

Kõigepealt loome meie ees IP pool. Järgmises näites luuakse ees pool, nimega `myFrontEndPool`:

```bash
azure network lb frontend-ip create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --public-ip-name myPublicIP \
  --name myFrontEndPool 
```

Väljund:

```bash
info:    Executing command network lb frontend-ip create
+ Looking up the load balancer "myLoadBalancer"
+ Looking up the public ip "myPublicIP"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myFrontEndPool
data:    Provisioning state              : Succeeded
data:    Private IP allocation method    : Dynamic
data:    Public IP address id            : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP
info:    network lb mySubnet-ip create command OK
```

Pange tähele, kuidas kasutasime selle `--public-ip-name` vahetamise läbida soovitud `myPublicIP` varasemas versioonis loodud. Laadi koormusetasakaalustusteenuse avaliku IP-aadressi määramisel võimaldab teil jõuda oma VMs Internetis.

Järgmiseks loome meie teise IP pool seekord meie tagaandmebaas liikluse. Järgmises näites luuakse tagaandmebaas rakenduskausta, nimega `myBackEndPool`:

```bash
azure network lb address-pool create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myBackEndPool
```

Väljund:

```bash
info:    Executing command network lb address-pool create
+ Looking up the load balancer "myLoadBalancer"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myBackEndPool
data:    Provisioning state              : Succeeded
info:    network lb address-pool create command OK
```

Me näeme, kuidas meie laadi koormusetasakaalustusteenuse läheb koos `azure network lb show` ja JSON väljund:

```bash
azure network lb show myResourceGroup myLoadBalancer --json | jq '.'
```

Väljund:

```bash
{
  "etag": "W/\"29c38649-77d6-43ff-ab8f-977536b0047c\"",
  "provisioningState": "Succeeded",
  "resourceGuid": "f1446acb-09ba-44d9-b8b6-849d9983dc09",
  "outboundNatRules": [],
  "inboundNatPools": [],
  "inboundNatRules": [],
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer",
  "name": "myLoadBalancer",
  "type": "Microsoft.Network/loadBalancers",
  "location": "westeurope",
  "mySubnetIPConfigurations": [
    {
      "etag": "W/\"29c38649-77d6-43ff-ab8f-977536b0047c\"",
      "name": "myFrontEndPool",
      "provisioningState": "Succeeded",
      "publicIPAddress": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP"
      },
      "privateIPAllocationMethod": "Dynamic",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
    }
  ],
  "backendAddressPools": [
    {
      "etag": "W/\"29c38649-77d6-43ff-ab8f-977536b0047c\"",
      "name": "myBackEndPool",
      "provisioningState": "Succeeded",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool"
    }
  ],
  "loadBalancingRules": [],
  "probes": []
}
```

## <a name="create-load-balancer-nat-rules"></a>Laadi koormusetasakaalustusteenuse NAT reeglite loomine
Liikluse läbib meie laadi koormusetasakaalustusteenuse saamiseks tuleb luua võrgu aadressi tõlge (NAT) reeglid, mis määrata, kas sissetulevaid või väljaminevaid toimingud. Saate määrata kasutama protokolli ning seejärel välise pordid vastendamine sisemise pordid vastavalt soovile. Meie keskkonna loomine mõned reeglid selle kohta, mis võimaldavad SSH meie laadi koormusetasakaalustusteenuse meie vms kaudu. Me loodud TCP-pordid 4222 ja 4223 suunamiseks TCP pordi 22 meie VMs (mille loome hiljem). Järgmises näites luuakse reegli nimega `myLoadBalancerRuleSSH1` TCP port 4222 vastendamiseks port 22:

```bash
azure network lb inbound-nat-rule create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myLoadBalancerRuleSSH1 \
  --protocol tcp --frontend-port 4222 --backend-port 22
```

Väljund:

```bash
info:    Executing command network lb inbound-nat-rule create
+ Looking up the load balancer "myLoadBalancer"
warn:    Using default enable floating ip: false
warn:    Using default idle timeout: 4
warn:    Using default mySubnet IP configuration "myFrontEndPool"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myLoadBalancerRuleSSH1
data:    Provisioning state              : Succeeded
data:    Protocol                        : Tcp
data:    mySubnet port                   : 4222
data:    Backend port                    : 22
data:    Enable floating IP              : false
data:    Idle timeout in minutes         : 4
data:    mySubnet IP configuration id    : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool
info:    network lb inbound-nat-rule create command OK
```

Korrake juhiseid oma teise reegli NAT SSH. Järgmises näites luuakse reegli nimega `myLoadBalancerRuleSSH2` TCP port 4223 vastendamiseks port 22:

```bash
azure network lb inbound-nat-rule create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myLoadBalancerRuleSSH2 --protocol tcp \
  --frontend-port 4223 --backend-port 22
```

Vaatame ka minna ja NAT reegli TCP 80 Port web liikluse, kinnihakkamine reegli kuni meie IP kaustu luua. Kui me ühenduse luua reegli IP ühendada asemel kinnihakkamine reegli meie vms ükshaaval me saate lisada või eemaldada VMs IP kaustast. Laadi koormusetasakaalustusteenuse automaatselt liikluse kulgemist. Järgmises näites luuakse reegli nimega `myLoadBalancerRuleWeb` vastendamiseks TCP port 80 pordi 80:

```bash
azure network lb rule create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myLoadBalancerRuleWeb --protocol tcp \
  --frontend-port 80 --backend-port 80 --frontend-ip-name myFrontEndPool \
  --backend-address-pool-name myBackEndPool
```

Väljund:

```bash
info:    Executing command network lb rule create
+ Looking up the load balancer "myLoadBalancer"
warn:    Using default idle timeout: 4
warn:    Using default enable floating ip: false
warn:    Using default load distribution: Default
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myLoadBalancerRuleWeb
data:    Provisioning state              : Succeeded
data:    Protocol                        : Tcp
data:    mySubnet port                   : 80
data:    Backend port                    : 80
data:    Enable floating IP              : false
data:    Load distribution               : Default
data:    Idle timeout in minutes         : 4
data:    mySubnet IP configuration id    : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool
data:    Backend address pool id         : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool
info:    network lb rule create command OK
```

## <a name="create-a-load-balancer-health-probe"></a>Laadi koormusetasakaalustusteenuse seisundi juures loomine

Tervis sondi perioodiliselt kontrolli VMs, mis on jäänud meie laadi koormusetasakaalustusteenuse veendumaks, et nad on opsüsteem ja reageerima määratletud. Kui ei, siis need on jäetud toiming tagamaks, et kasutajad ei suunatud neile. Saate määratleda kohandatud otsib seisundi juures, koos intervallide ja ajalõpp väärtused. Seisund sondid kohta leiate lisateavet teemast [Laadi koormusetasakaalustusteenuse sondid](../load-balancer/load-balancer-custom-probe-overview.md). Järgmises näites luuakse TCP seisund probed nimega `myHealthProbe`:

```bash
azure network lb probe create --resource-group myResourceGroup \
  --lb-name myLoadBalancer --name myHealthProbe --protocol "tcp" \
  --interval 15 --count 4
```

Väljund:

```bash
info:    Executing command network lb probe create
warn:    Using default probe port: 80
+ Looking up the load balancer "myLoadBalancer"
+ Updating load balancer "myLoadBalancer"
data:    Name                            : myHealthProbe
data:    Provisioning state              : Succeeded
data:    Protocol                        : Tcp
data:    Port                            : 80
data:    Interval in seconds             : 15
data:    Number of probes                : 4
info:    network lb probe create command OK
```

Siin me määratud meie seisundikontrollid 15 sekundi järel. Me ei pane kuni nelja sondid (üks minut) enne laadi koormusetasakaalustusteenuse leiab, et selle hosti ei toimi enam.

## <a name="verify-the-load-balancer"></a>Laadi koormusetasakaalustusteenuse kinnitamine
Laadi koormusetasakaalustusteenuse konfigureerimine on nüüd lõppenud. Siin on toimingud, mida tegite eelnevalt.

1. Esmalt loodud laadi koormusetasakaalustusteenuse.
2. Seejärel ees IP pool loodud ja määratud avaliku IP.
3. Olete loonud tagaandmebaas IP pool, mida saab ühendada VMs edasi.
4. Pärast seda, kui olete loonud NAT reeglid, mis võimaldavad SSH vms haldamiseks koos reegel, mis lubab TCP 80 port meie web appi.
5. Lõpuks lisatud seisundi juures perioodiliselt kontrollida VMs. Selle seisundi juures tagab, et kasutajad ei katsel VM, mis pole enam toimimise või serveeritakse sisu.

Vaatame, kuidas oma laadi koormusetasakaalustusteenuse näeb nüüd:

```bash
azure network lb show --resource-group myResourceGroup \
  --name myLoadBalancer --json | jq '.'
```

Väljund:

```bash
{
  "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
  "provisioningState": "Succeeded",
  "resourceGuid": "f1446acb-09ba-44d9-b8b6-849d9983dc09",
  "outboundNatRules": [],
  "inboundNatPools": [],
  "inboundNatRules": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myLoadBalancerRuleSSH1",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1",
      "mySubnetIPConfiguration": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
      },
      "protocol": "Tcp",
      "mySubnetPort": 4222,
      "backendPort": 22,
      "idleTimeoutInMinutes": 4,
      "enableFloatingIP": false,
      "provisioningState": "Succeeded"
    },
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myLoadBalancerRuleSSH2",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2",
      "mySubnetIPConfiguration": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
      },
      "protocol": "Tcp",
      "mySubnetPort": 4223,
      "backendPort": 22,
      "idleTimeoutInMinutes": 4,
      "enableFloatingIP": false,
      "provisioningState": "Succeeded"
    }
  ],
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer",
  "name": "myLoadBalancer",
  "type": "Microsoft.Network/loadBalancers",
  "location": "westeurope",
  "mySubnetIPConfigurations": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myFrontEndPool",
      "provisioningState": "Succeeded",
      "publicIPAddress": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP"
      },
      "privateIPAllocationMethod": "Dynamic",
      "loadBalancingRules": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/loadBalancingRules/myLoadBalancerRuleWeb"
        }
      ],
      "inboundNatRules": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1"
        },
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2"
        }
      ],
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
    }
  ],
  "backendAddressPools": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myBackEndPool",
      "provisioningState": "Succeeded",
      "loadBalancingRules": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/loadBalancingRules/myLoadBalancerRuleWeb"
        }
      ],
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool"
    }
  ],
  "loadBalancingRules": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myLoadBalancerRuleWeb",
      "provisioningState": "Succeeded",
      "enableFloatingIP": false,
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/loadBalancingRules/myLoadBalancerRuleWeb",
      "mySubnetIPConfiguration": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/mySubnetIPConfigurations/myFrontEndPool"
      },
      "backendAddressPool": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool"
      },
      "protocol": "Tcp",
      "loadDistribution": "Default",
      "mySubnetPort": 80,
      "backendPort": 80,
      "idleTimeoutInMinutes": 4
    }
  ],
  "probes": [
    {
      "etag": "W/\"62a7c8e7-859c-48d3-8e76-5e078c5e4a02\"",
      "name": "myHealthProbe",
      "provisioningState": "Succeeded",
      "numberOfProbes": 4,
      "intervalInSeconds": 15,
      "port": 80,
      "protocol": "Tcp",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/probes/myHealthProbe"
    }
  ]
}
```

## <a name="create-an-nic-to-use-with-the-linux-vm"></a>Saate luua ka NIC Linuxi kasutamiseks

NICs on programmiliselt saadaval, kuna saate rakendada reeglid nende kasutamist. Saate määrata rohkem kui üks. Pärast `azure network nic create` käsku, saate ühenduse luua NIC laadi tagaandmebaas IP pool ja seostada NAT reegli SSH liikluse lubamiseks.
 
Asendada selle `#####-###-###` jaotiste oma Azure tellimuse ID-ga. Tellimuse ID on märgitud väljund `jq` loote ressursside uurimisel. Saate vaadata oma tellimuse ID, mille `azure account list`. 

Järgmises näites luuakse NIC, mis on nimega `myNic1`:

```bash
azure network nic create --resource-group myResourceGroup --location westeurope \
  --subnet-vnet-name myVnet --subnet-name mySubnet --name myNic1 \
  --lb-address-pool-ids /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool \
  --lb-inbound-nat-rule-ids /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1
```

Väljund:

```bash
info:    Executing command network nic create
+ Looking up the subnet "mySubnet"
+ Looking up the network interface "myNic1"
+ Creating network interface "myNic1"
data:    Id                              : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic1
data:    Name                            : myNic1
data:    Type                            : Microsoft.Network/networkInterfaces
data:    Location                        : westeurope
data:    Provisioning state              : Succeeded
data:    Enable IP forwarding            : false
data:    IP configurations:
data:      Name                          : Nic-IP-config
data:      Provisioning state            : Succeeded
data:      Private IP address            : 192.168.1.4
data:      Private IP allocation method  : Dynamic
data:      Subnet                        : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet
data:      Load balancer backend address pools:
data:        Id                          : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool
data:      Load balancer inbound NAT rules:
data:        Id                          : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1
data:
info:    network nic create command OK
```

Üksikasjad saate vaadata otse uurides ressurss. Saate uurida ressursi abil soovitud `azure network nic show` käsk:

```bash
azure network nic show myResourceGroup myNic1 --json | jq '.'
```

Väljund:

```bash
{
  "etag": "W/\"fc1eaaa1-ee55-45bd-b847-5a08c7f4264a\"",
  "provisioningState": "Succeeded",
  "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic1",
  "name": "myNic1",
  "type": "Microsoft.Network/networkInterfaces",
  "location": "westeurope",
  "ipConfigurations": [
    {
      "etag": "W/\"fc1eaaa1-ee55-45bd-b847-5a08c7f4264a\"",
      "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myNic1/ipConfigurations/Nic-IP-config",
      "loadBalancerBackendAddressPools": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool"
        }
      ],
      "loadBalancerInboundNatRules": [
        {
          "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH1"
        }
      ],
      "privateIPAddress": "192.168.1.4",
      "privateIPAllocationMethod": "Dynamic",
      "subnet": {
        "id": "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet"
      },
      "provisioningState": "Succeeded",
      "name": "Nic-IP-config"
    }
  ],
  "dnsSettings": {
    "appliedDnsServers": [],
    "dnsServers": []
  },
  "enableIPForwarding": false,
  "resourceGuid": "a20258b8-6361-45f6-b1b4-27ffed28798c"
}
```

Nüüd saame luua teine NIC, kinnihakkamine meie tagaandmebaas IP rakenduskausta uuesti. Reegli kellaaja sekundid NAT lubab SSH liikluse. Järgmises näites luuakse NIC, mis on nimega `myNic2`:

```bash
azure network nic create --resource-group myResourceGroup --location westeurope \
  --subnet-vnet-name myVnet --subnet-name mySubnet --name myNic2 \
  --lb-address-pool-ids  /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/backendAddressPools/myBackEndPool \
  --lb-inbound-nat-rule-ids /subscriptions/########-####-####-####-############/resourceGroups/myResourceGroup/providers/Microsoft.Network/loadBalancers/myLoadBalancer/inboundNatRules/myLoadBalancerRuleSSH2
```

## <a name="create-a-network-security-group-and-rules"></a>Turberühma võrgu ja reeglite loomine

Nüüd saame luua võrgu turberühma ja sissetulevad reeglid, mis haldavad juurdepääsu NIC. Võrgu turberühma saab rakendada NIC või alamvõrgu. Määratlege reeglid abil kontrollida liikluse ja sealt oma VMs. Järgmises näites luuakse võrgu turberühma nimega `myNetworkSecurityGroup`:

```bash
azure network nsg create --resource-group myResourceGroup --location westeurope \
  --name myNetworkSecurityGroup
```

Sissetulev reegel NSG lubamiseks sissetulevaid ühendusi port 22 (toetamiseks SSH) lisada. Järgmises näites luuakse reegli nimega `myNetworkSecurityGroupRuleSSH` lubamiseks TCP port 22:

```bash
azure network nsg rule create --resource-group myResourceGroup \
  --nsg-name myNetworkSecurityGroup --protocol tcp --direction inbound \
  --priority 1000 --destination-port-range 22 --access allow \
  --name myNetworkSecurityGroupRuleSSH
```

Nüüd lisame Sissetulev reegel NSG lubamiseks sissetulevaid ühendusi pordi 80 (toetamiseks web liiklus). Järgmises näites luuakse reegli nimega `myNetworkSecurityGroupRuleHTTP` lubamiseks TCP port 80:

```bash
azure network nsg rule create --resource-group myResourceGroup \
  --nsg-name myNetworkSecurityGroup --protocol tcp --direction inbound \
  --priority 1001 --destination-port-range 80 --access allow \
  --name myNetworkSecurityGroupRuleHTTP
```

> [AZURE.NOTE] Sissetulev reegel on sissetulevate võrguühenduste filter. Selles näites me siduda soovitud NSG VMs virtuaalse NIC, mis tähendab, et kõik taotluse port 22 on läbi NIC meie VM. See Sissetulev reegel on võrguühendus kohta ja ei kohta lõpp-mis on, mis on kohta klassikaline kasutuselevõttu. Pordi avamiseks peab jätate selle `--source-port-range` määramine '\*' (vaikeväärtus) sissetuleva taotlusi, **mis tahes** paluda pordi kaudu. Pordid tavaliselt dünaamiline.

## <a name="bind-to-the-nic"></a>Sidumiseks NIC

Funktsiooni NSG sidumiseks NICs. Peame meie NICs kasutajaga meie võrgu turberühma. Käivitage nii ühenduse luua nii meie NICs käsud:

```bash
azure network nic set --resource-group myResourceGroup --name myNic1 \
  --network-security-group-name myNetworkSecurityGroup
```

```bash
azure network nic set --resource-group myResourceGroup --name myNic2 \
  --network-security-group-name myNetworkSecurityGroup
```

## <a name="create-an-availability-set"></a>Luua mõne kättesaadavus
Kättesaadavus määrab aitab levitada oma VMs viga domeenid ja täiendamine domeenide üle. Loome seada oma vms olemasolu. Järgmises näites luuakse seadmine nimega saadavus `myAvailabilitySet`:

```bash
azure availset create --resource-group myResourceGroup --location westeurope
  --name myAvailabilitySet
```

Viga domeenide määratleda virtuaalmasinates levinud power lähte- ja võrgu vahetamise jagavad rühmitus. Vaikimisi eraldatakse kuni kolm viga domeenides virtuaalmasinates, mis on konfigureeritud oma kättesaadavuse määramine. Mõte on, et riistvara probleemi ühel domeenid viga ei mõjuta iga VM, mis töötab teie rakendus. Kui need on kättesaadavus kogum, jaotab Azure'i VMs domeenides viga.

Täiendamine domeenide näitavad virtuaalmasinates ja aluseks füüsilise riistvara, mis saab korraga taaskäivitama. Järjestuses, milles on rebooted täiendamine domeenide ei pruugi olla järjestikku kavandatud hooldustööde ajal, kuid ainult üks versiooniuuenduse taaskäivitatakse korraga. Klõpsake uuesti Azure'i automaatselt jaotab oma VMs versioonitäienduse domeenides kui panemist kättesaadavus saidil.

Lugege lisateavet [VMs kättesaadavuse haldamiseks](./virtual-machines-linux-manage-availability.md).

## <a name="create-the-linux-vms"></a>Linux VMs loomine

Olete loonud salvestusruumi ja ressursid toetama Interneti-juurdepääs VMs. Nüüd loome nende VMs ja secure need SSH abil, mis ei sisalda parooli. Selles näites me ei kavatse Ubuntu VM viimase LTS põhjal luua. Oleme selle pildi teabe leidmiseks, kasutades `azure vm image list`, nagu on kirjeldatud [Azure VM piltide otsimine](virtual-machines-linux-cli-ps-findimage.md).

Valisime pilt käsuga `azure vm image list westeurope canonical | grep LTS`. Selles näites me kasutame `canonical:UbuntuServer:16.04.0-LTS:16.04.201608150`. Viimase välja võtame `latest` nii, et tulevikus saame alati kõige uuemad koostamine. (Stringi, mida me kasutame `canonical:UbuntuServer:16.04.0-LTS:16.04.201608150`).

See järgmiseks etapiks on tuttav kõigile, kellel on juba loonud mõne ssh rsa avalike ja privaatvõ võti sidumine Linux või Mac-arvutisse, kasutades **ssh-keygen-t rsa -b 2048**. Kui teil on serdi võtme paari oma `~/.ssh` directory, saate luua need:

- Automaatselt, kasutades funktsiooni `azure vm create --generate-ssh-keys` suvand.
- Käsitsi [luua enda juhiste](virtual-machines-linux-mac-create-ssh-keys.md)järgi.

Teise võimalusena saate kasutada funktsiooni `--admin-password` meetod autentida SSH ühenduste VM loomise järel. See meetod on tavaliselt vähem turvaline.

Loome VM tuues kõik ressursid ja teavet koos selle `azure vm create` käsk:

```bash
azure vm create \
  --resource-group myResourceGroup \
  --name myVM1 \
  --location westeurope \
  --os-type linux \
  --availset-name myAvailabilitySet \
  --nic-name myNic1 \
  --vnet-name myVnet \
  --vnet-subnet-name mySubnet \
  --storage-account-name mystorageaccount \
  --image-urn canonical:UbuntuServer:16.04.0-LTS:latest \
  --ssh-publickey-file ~/.ssh/id_rsa.pub \
  --admin-username ops
```

Väljund:

```bash
info:    Executing command vm create
+ Looking up the VM "myVM1"
info:    Verifying the public key SSH file: /home/ahmet/.ssh/id_rsa.pub
info:    Using the VM Size "Standard_DS1"
info:    The [OS, Data] Disk or image configuration requires storage account
+ Looking up the storage account mystorageaccount
+ Looking up the availability set "myAvailabilitySet"
info:    Found an Availability set "myAvailabilitySet"
+ Looking up the NIC "myNic1"
info:    Found an existing NIC "myNic1"
info:    Found an IP configuration with virtual network subnet id "/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet" in the NIC "myNic1"
info:    This is an NIC without publicIP configured
info:    The storage URI 'https://mystorageaccount.blob.core.windows.net/' will be used for boot diagnostics settings, and it can be overwritten by the parameter input of '--boot-diagnostics-storage-uri'.
info:    vm create command OK
```

Saate luua ühenduse oma VM kohe oma vaikimisi SSH klahvide abil. Veenduge, et määrata sobiva, kuna me läbib laadi koormusetasakaalustusteenuse. (Meie esimese VM jaoks me häälestatud reegli NAT pordi 4222 suunata meie VM):

```bash
 ssh ops@mypublicdns.westeurope.cloudapp.azure.com -p 4222
```

Väljund:

```bash
The authenticity of host '[mypublicdns.westeurope.cloudapp.azure.com]:4222 ([xx.xx.xx.xx]:4222)' can't be established.
ECDSA key fingerprint is 94:2d:d0:ce:6b:fb:7f:ad:5b:3c:78:93:75:82:12:f9.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '[mypublicdns.westeurope.cloudapp.azure.com]:4222,[xx.xx.xx.xx]:4222' (ECDSA) to the list of known hosts.
Welcome to Ubuntu 16.04.1 LTS (GNU/Linux 4.4.0-34-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  Get cloud support with Ubuntu Advantage Cloud Guest:
    http://www.ubuntu.com/business/services/cloud

0 packages can be updated.
0 updates are security updates.

ops@myVM1:~$
```

Minna ja luua oma teise VM samal viisil:

```bash
azure vm create \
  --resource-group myResourceGroup \
  --name myVM2 \
  --location westeurope \
  --os-type linux \
  --availset-name myAvailabilitySet \
  --nic-name myNic2 \
  --vnet-name myVnet \
  --vnet-subnet-name mySubnet \
  --storage-account-name mystorageaccount \
  --image-urn canonical:UbuntuServer:16.04.0-LTS:latest \
  --ssh-publickey-file ~/.ssh/id_rsa.pub \
  --admin-username ops
```

Ja nüüd saate selle `azure vm show myResourceGroup myVM1` käsu uurida, mida olete loonud. Selles etapis kasutate oma Ubuntu VMs taha laadi koormusetasakaalustusteenuse Azure, mida saate sisse logida ainult koos oma SSH paari (kuna paroolid on keelatud). Saate installida nginx või httpd, juurutada web appi ja vaadata liiklus läbi laadi koormusetasakaalustusteenuse nii VMs.

```bash
azure vm show --resource-group myResourceGroup --name myVM1
```

Väljund:

```bash
info:    Executing command vm show
+ Looking up the VM "TestVM1"
+ Looking up the NIC "myNic1"
data:    Id                              :/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM1
data:    ProvisioningState               :Succeeded
data:    Name                            :myVM1
data:    Location                        :westeurope
data:    Type                            :Microsoft.Compute/virtualMachines
data:
data:    Hardware Profile:
data:      Size                          :Standard_DS1
data:
data:    Storage Profile:
data:      Image reference:
data:        Publisher                   :canonical
data:        Offer                       :UbuntuServer
data:        Sku                         :16.04.0-LTS
data:        Version                     :latest
data:
data:      OS Disk:
data:        OSType                      :Linux
data:        Name                        :clib45a8b650f4428a1-os-1471973896525
data:        Caching                     :ReadWrite
data:        CreateOption                :FromImage
data:        Vhd:
data:          Uri                       :https://mystorageaccount.blob.core.windows.net/vhds/clib45a8b650f4428a1-os-1471973896525.vhd
data:
data:    OS Profile:
data:      Computer Name                 :myVM1
data:      User Name                     :ops
data:      Linux Configuration:
data:        Disable Password Auth       :true
data:
data:    Network Profile:
data:      Network Interfaces:
data:        Network Interface #1:
data:          Primary                   :true
data:          MAC Address               :00-0D-3A-24-D4-AA
data:          Provisioning State        :Succeeded
data:          Name                      :LmyNic1
data:          Location                  :westeurope
data:
data:    AvailabilitySet:
data:      Id                            :/subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Compute/availabilitySets/myAvailabilitySet
data:
data:    Diagnostics Profile:
data:      BootDiagnostics Enabled       :true
data:      BootDiagnostics StorageUri    :https://mystorageaccount.blob.core.windows.net/
data:
data:      Diagnostics Instance View:
info:    vm show command OK

```


## <a name="export-the-environment-as-a-template"></a>Ekspordi keskkonna mallina
Nüüd, kui teil on ehitatud välja selles keskkonnas, mida teha, kui soovite luua veel keskkonnas sama parameetrid või tootmiskeskkonda, mis vastab selle? Ressursihaldur kasutab JSON Mallid, mis määratlevad kõik parameetrid keskkonnas. Koostate kogu keskkonnas välja selle JSON malli viitamine. Saate [koostada JSON mallide käsitsi](../resource-group-authoring-templates.md) või olemasoleva keskkonnas JSON malli loomiseks saate eksportida:

```bash
azure group export --name myResourceGroup
```

See käsk loob soovitud `myResourceGroup.json` oma praegust töötavat kausta fail. Selle malli põhjal keskkonnas loomisel küsitakse kõik ressursside nimed, sh laadi koormusetasakaalustusteenuse, võrgu liidesed või VMs nimesid. Asustada need nimed mallifail, lisades selle `-p` või `--includeParameterDefaultValue` parameeter on `azure group export` käsk eespool kuvatud. JSON malli, et määrata ressursside nimed, või [luua parameters.json faili](../resource-group-authoring-templates.md#parameters) mis määrab ressursside nimed redigeerimine.

Mallist keskkonnas loomiseks tehke järgmist.

```bash
azure group deployment create --resource-group myNewResourceGroup \
  --template-file myResourceGroup.json
```

Võite lugeda [Lisateavet juurutamise mallide põhjal](../resource-group-template-deploy-cli.md). Lisateavet artistide värskendada keskkonnas, parameetrid faili kasutada, ja kasutada Mallid ühe talletuskoht.

## <a name="next-steps"></a>Järgmised sammud

Nüüd olete valmis alustada mitme võrgu komponendid ja VMs töötamine. Saate selle valimi keskkonna rajamise rakenduse abil kasutusele põhiosa siin.
