<properties
   pageTitle="Luua Linux VM mitu NICs | Microsoft Azure'i"
   description="Siit saate teada, kuidas luua Linux VM mitu NICs manustatud Azure'i CLI või ressursihaldur mallide kasutamine."
   services="virtual-machines-linux"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="10/27/2016"
   ms.author="iainfou"/>

# <a name="creating-a-linux-vm-with-multiple-nics"></a>Mitu NICs Linux VM loomine
Saate luua virtuaalse masina (VM) Azure, mis sisaldab mitme virtuaalse võrgu liidesed (NICs) oleks lisatud. Stsenaarium oleks teise alamvõrku ees- ja Ühenduvus või võrgus pühendunud jälgimise või varukoopia lahendus. See artikkel annab kiirülevaate käskude loomiseks lisatud mitu NICs VM. Üksikasjalikku teavet, sh kuidas luua mitu NICs sees oma Bash skriptid, lugege lisateavet juurutamise [mitme-NIC VMs](../virtual-network/virtual-network-deploy-multinic-arm-cli.md). Erinevate [VM suurused](virtual-machines-linux-sizes.md) toetavad erineva arvu NICs, nii suurus vastavalt oma VM.

>[AZURE.WARNING] Kui loote VM - ei saa mõne olemasoleva VM NICs lisada mitu NICs siduda. Saate [luua VM põhjal algse virtuaalse ketta](virtual-machines-linux-copy-vm.md) ja luua mitu NICs juurutamist VM.

## <a name="quick-commands"></a>Kiirülevaate käsud
Veenduge, et olete sisse logitud ja ressursihaldur režiimi kasutamine [Azure CLI](../xplat-cli-install.md) .

```bash
azure config mode arm
```

Järgmistes näidetes asendamine oma väärtustega näide parameetrite nimed. Näide parameetrite nimed sisalduv `myResourceGroup`, `mystorageaccount`, ja `myVM`.

Esmalt looge ressursirühma. Järgmises näites luuakse ressursirühma nimega `myResourceGroup` klõpsake soovitud `WestUS` asukoht:

```bash
azure group create myResourceGroup -l WestUS
```

Looge konto salvestusruumi hoida oma VMs. Järgmises näites luuakse salvestusruumi konto nimega `mystorageaccount`:

```bash
azure storage account create mystorageaccount -g myResourceGroup \
    -l WestUS --kind Storage --sku-name PLRS
```

Luua ühenduse oma VMs virtuaalse võrgu. Järgmises näites luuakse virtuaalse võrgu nimega `myVnet` aadress eesliitega, `192.168.0.0/16`:

```bash
azure network vnet create -g myResourceGroup -l WestUS \
    -n myVnet -a 192.168.0.0/16
```

Luua kaks virtuaalse alamvõrku – üks ees liikluse ja üks tagaandmebaas liikluse. Järgmises näites luuakse kaks alamvõrku, nimega `mySubnetFrontEnd` ja `mySubnetBackEnd`:

```bash
azure network vnet subnet create -g myResourceGroup -e myVnet \
    -n mySubnetFrontEnd -a 192.168.1.0/24
azure network vnet subnet create -g myResourceGroup -e myVnet \
    -n mySubnetBackEnd -a 192.168.2.0/24
```

Looge kaks NICs, manustamise üks NIC ees alamvõrgu asukoht ja ühe tagaandmebaas alamvõrgu. Järgmises näites luuakse kaks NICs, nimega `myNic1` ja `myNic2`, ning lisab need teie alamvõrku:

```bash
azure network nic create -g myResourceGroup -l WestUS \
    -n myNic1 -m myVnet -k mySubnetFrontEnd
azure network nic create -g myResourceGroup -l WestUS \
    -n myNic2 -m myVnet -k mySubnetBackEnd
```

Lõpuks luua oma VM, lisades kaks NICs varem loodud. Järgmises näites luuakse nimega VM `myVM`:

```bash
azure vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --location WestUS \
    --os-type linux \
    --nic-names myNic1,myNic2 \
    --vm-size Standard_DS2_v2 \
    --storage-account-name mystorageaccount \
    --image-urn UbuntuLTS \
    --admin-username ops \
    --ssh-publickey-file ~/.ssh/id_rsa.pub
```

## <a name="creating-multiple-nics-using-azure-cli"></a>Mitu NICs kasutamine Azure CLI loomine
Kui olete varem loonud abil Azure'i CLI VM, kiirülevaate käsud peaksid olema kursis. Protsess on sama NIC ühe või mitu NICs loomiseks. Lugege lisateavet [juurutamine mitu NICs Azure'i CLI kasutamine](../virtual-network/virtual-network-deploy-multinic-arm-cli.md), sh skriptimise hoidke kaudu, luua kõik NICs protsessi.

Järgmises näites luuakse kaks NICs, nimega `myNic1` ja `myNic2`, üks NIC ühenduse iga alamvõrgu abil:

```bash
azure network nic create --resource-group myResourceGroup --location WestUS \
    -n myNic1 --subnet-vnet-name myVnet --subnet-name mySubnetFrontEnd
azure network nic create --resource-group myResourceGroup --location WestUS \
    -n myNic2 --subnet-vnet-name myVnet --subnet-name mySubnetBackEnd
```

Tavaliselt ka loote [Võrgu turberühma](../virtual-network/virtual-networks-nsg.md) või [laadimine koormusetasakaalustusteenuse](../load-balancer/load-balancer-overview.md) haldamiseks ja levitamine liikluse oma VMs üle. Klõpsake uuesti käsud on sama mitu NICs töötamisel. Järgmises näites luuakse võrgu turberühma nimega `myNetworkSecurityGroup`:

```bash
azure network nsg create --resource-group myResourceGroup --location WestUS \
    --name myNetworkSecurityGroup
```

Turberühma võrgu kaudu oma NICs sidumiseks `azure network nic set`. Järgmises näites seob `myNic1` ja `myNic2` koos `myNetworkSecurityGroup`:

```bashazure 
azure network nic set --resource-group myResourceGroup --name myNic1 \
    --network-security-group-name myNetworkSecurityGroup
azure network nic set --resource-group myResourceGroup --name myNic2 \
    --network-security-group-name myNetworkSecurityGroup
```

Kui loote VM, nüüd määrata mitu NICs. Pigem abil `--nic-name` esitada ühe NIC, selle asemel saate kasutada `--nic-names` ja sisestage NICs komaga eraldatud loend. Samuti peate Olge ettevaatlik, kui valite VM suurus. Saate lisada VM NICs koguarvu piirangud on. Lugege lisateavet [Linux VM suurused](virtual-machines-linux-sizes.md). Järgmises näites on kujutatud kuidas määrata mitu NICs ja seejärel VM suurus, mis toetab mitut NICs kasutamine (`Standard_DS2_v2`):

```bash
azure vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --location WestUS \
    --os-type linux \
    --nic-names myNic1,myNic2 \
    --vm-size Standard_DS2_v2 \
    --storage-account-name mystorageaccount \
    --image-urn UbuntuLTS \
    --admin-username ops \
    --ssh-publickey-file ~/.ssh/id_rsa.pub
```

## <a name="creating-multiple-nics-using-resource-manager-templates"></a>Luua mitu NICs ressursihaldur mallide kasutamine
Azure'i ressursihaldur mallide abil saate määratleda keskkonna deklaratiivseid JSON-failid. Leiate [Azure'i ressursihaldur ülevaade](../azure-resource-manager/resource-group-overview.md). Ressursihaldur Mallid võimalda loomine ressursi mitu eksemplari, nt mitu NICs loomise ajal. *Kopeeri* abil saate määrata loomiseks eksemplaride arv:

```bash
"copy": {
    "name": "multiplenics"
    "count": "[parameters('count')]"
}
```

Lugege lisateavet [mitmes eksemplaris, kasutades *eksemplari*loomise](../resource-group-create-multiple.md)kohta. 

Saate kasutada ka mõne `copyIndex()` seejärel lisandada arvu ressursinimi, mis võimaldab teil luua `myNic1`, `myNic2`jne. Järgmisel joonisel on kujutatud näide lisades indeksi väärtus:

```bash
"name": "[concat('myNic', copyIndex())]", 
```

Saate lugeda täielik näide [loomise mitu NICs ressursihaldur mallide kasutamine](../virtual-network/virtual-network-deploy-multinic-arm-template.md).

## <a name="next-steps"></a>Järgmised sammud
Veenduge, et vaadata [Linux VM suurused](virtual-machines-linux-sizes.md) , kui proovite luua mitu NICs VM. Pöörake tähelepanu iga VM suurus toetab NICs maksimaalne arv. 

Pidage meeles, et ei saa lisada täiendavad NICs mõne olemasoleva VM, peate looma kõik NICs VM juurutamisel. Olge ettevaatlik, kui plaanite oma juurutuste veenduge, et teil on kõik nõutavad võrguühendus algusest.