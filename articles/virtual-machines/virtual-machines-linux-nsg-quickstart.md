<properties
   pageTitle="Avatud pordid ja Linux VM lõpp-punktid | Microsoft Azure'i"
   description="Saate teada, kuidas avada port / luua oma Linux VM Azure ressursi halduri juurutamise mudeli ja Azure CLI abil lõpp"
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
   ms.workload="infrastructure-services"
   ms.date="10/27/2016"
   ms.author="iainfou"/>

# <a name="opening-ports-and-endpoints-to-a-linux-vm-in-azure"></a>Avatud pordid ja Linux VM Azure lõpp-punktid
Pordi avamine või loomine Azure lõpp virtuaalse masina (VM), alamvõrgu või VM võrgu liidese võrgu filtri loomine. Neid filtreid, nii sissetuleva ja väljamineva liikluse kontrollivate paigutamiseks seotud ressursside, mis saab liiklus võrgu turberühma. Kasutame võrguliikluse näide pordi 80.

## <a name="quick-commands"></a>Kiirülevaate käsud
Turberühma võrgu ja reeglite loomiseks peate [Azure'i CLI](../xplat-cli-install.md) installitud ja ressursihaldur režiimi kasutamine.

```bash
azure config mode arm
```

Järgmistes näidetes asendamine oma väärtustega näide parameetrite nimed. Näide parameetrite nimed sisalduv `myResourceGroup`, `myNetworkSecurityGroup`, ja `myVnet`.

Looge oma võrgu turberühm, sisestades oma nime ja asukoha õigesti. Järgmises näites luuakse võrgu turberühma nimega `myNetworkSecurityGroup` klõpsake soovitud `WestUS` asukoht:

```
azure network nsg create --resource-group myResourceGroup --location westus \
    --name myNetworkSecurityGroup
```

Lisage reegli luba HTTP-liikluse oma veebiserverile (või milline olukord teie omaga, nt SSH Accessi või andmebaasi ühenduvuse reguleerimine). Järgmises näites luuakse reegli nimega `myNetworkSecurityGroupRule` port 80 TCP liikluse lubamiseks:

```
azure network nsg rule create --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup --name myNetworkSecurityGroupRule \
    --protocol tcp --direction inbound --priority 1000 \
    --destination-port-range 80 --access allow
```

Võrgu turberühma seostada oma VM võrgu liidese (NIC). Järgmises näites seob mõne olemasoleva NIC nimega `myNic` koos võrgu turberühma nimega `myNetworkSecurityGroup`:

```
azure network nic set --resource-group myResourceGroup \
    --network-security-group-name myNetworkSecurityGroup --name myNic
```

Teise võimalusena saate seostada oma võrgu turberühma koos virtuaalse alamvõrku, mitte vaid ühe VM võrgu liidese. Järgmises näites seob olemasoleva alamvõrgu nimega `mySubnet` klõpsake soovitud `myVnet` virtuaalse võrgu Network turberühma nimega `myNetworkSecurityGroup`:

```
azure network vnet subnet set --resource-group myResourceGroup \
    --network-security-group-name myNetworkSecurityGroup \
    --vnet-name myVnet --name mySubnet
```

## <a name="more-information-on-network-security-groups"></a>Lisateavet võrgu turberühmad
Kiirülevaate käsud võimaldavad tööks liiklus minevaid oma VM. Võrgu turberühmad pakuvad palju suurepäraseid omadusi ja granulaarsus juurdepääsu oma ressursse kontrollimisel. Lugege lisateavet [võrgu turberühma ja ACL reeglid siin loomise](../virtual-network/virtual-networks-create-nsg-arm-cli.md)kohta.

Saate määratleda võrgu turberühmad ja ACL reeglite Azure'i ressursihaldur Mallid osana. Lisateavet [võrgu turberühmad malle loomise](../virtual-network/virtual-networks-create-nsg-arm-template.md)kohta.

Kui peate kasutama pordi suunamise vastendamiseks kordumatu välise port oma VM sisemise pordiga, kasutage laadi koormusetasakaalustusteenuse ja Address Translation (NAT) reeglid. Näiteks võite TCP port 8080 väliselt nähtavaks tegemine ja on liiklus suunatud TCP 80 porti VM. Saate teada, [mis Interneti-ühendusega laadi koormusetasakaalustusteenuse loomise](../load-balancer/load-balancer-get-started-internet-arm-cli.md)kohta.

## <a name="next-steps"></a>Järgmised sammud
Selles näites on loodud lihtsa reegli HTTP liikluse lubamiseks. Leiate teavet järgmistest artiklitest üksikasjalikumat keskkonnas loomise kohta:

- [Azure'i ressursihaldur ülevaade](../azure-resource-manager/resource-group-overview.md)
- [Mis on võrgu turvalisus jaotises (NSG)?](../virtual-network/virtual-networks-nsg.md)
- [Azure'i koormus soolise ressursihaldur ülevaade](../load-balancer2    /load-balancer-arm.md)