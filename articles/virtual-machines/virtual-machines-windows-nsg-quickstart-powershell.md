<properties
   pageTitle="Avage pordid VM PowerShelli kaudu | Microsoft Azure'i"
   description="Saate teada, kuidas avada port / oma Windows VM Azure ressursi halduri juurutamise režiimi ja Azure PowerShelli abil lõpp-punkti loomine"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="10/27/2016"
   ms.author="iainfou"/>

# <a name="opening-ports-and-endpoints-to-a-vm-in-azure-using-powershell"></a>Avades pordid ja lõpp-punktid VM Azure PowerShelli abil
[AZURE.INCLUDE [virtual-machines-common-nsg-quickstart](../../includes/virtual-machines-common-nsg-quickstart.md)]

## <a name="quick-commands"></a>Kiirülevaate käsud
Võrgu turberühma ja ACL reeglite loomiseks peate [Azure PowerShelli paigaldatud uusim versioon](../powershell-install-configure.md). Samuti saate [teha järgmist Azure'i portaalis](virtual-machines-windows-nsg-quickstart-portal.md).

Azure'i kontosse sisse logida:

```powershell
Login-AzureRmAccount
```

Järgmistes näidetes asendamine oma väärtustega näide parameetrite nimed. Näide parameetrite nimed sisalduv `myResourceGroup`, `myNetworkSecurityGroup`, ja `myVnet`.

Reegli loomine. Järgmises näites luuakse reegli nimega `myNetworkSecurityGroupRule` port 80 TCP liikluse lubamiseks:

```powershell
$httprule = New-AzureRmNetworkSecurityRuleConfig -Name "myNetworkSecurityGroupRule" `
    -Description "Allow HTTP" -Access "Allow" -Protocol "Tcp" -Direction "Inbound" `
    -Priority "100" -SourceAddressPrefix "Internet" -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 80
```

Järgmiseks oma võrgu Turberühma loomine ja määramine HTTP reegli äsja loodud järgmiselt. Järgmises näites luuakse võrgu turberühma nimega `myNetworkSecurityGroup`:

```powershell
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName "myResourceGroup" `
    -Location "WestUS" -Name "myNetworkSecurityGroup" -SecurityRules $httprule
```

Nüüd vaatame määrata oma võrgu turberühma lisamine alamvõrgu. Järgmises näites määratakse olemasolevat virtuaalse võrku nimega `myVnet` muutuja `$vnet`:

```powershell
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName "myResourceGroup" `
    -Name "myVnet"
```

Teie alamvõrku oma võrgu turberühma seostada. Järgmises näites seob nimega alamvõrgu `mySubnet` koos teie võrgu turberühma:

```powershell
$subnetPrefix = $vnet.Subnets|?{$_.Name -eq 'mySubnet'}

Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name "mySubnet" `
    -AddressPrefix $subnetPrefix.AddressPrefix `
    -NetworkSecurityGroup $nsg
```

Lõpuks värskendamiseks virtuaalse võrgu selleks, et tehtud muudatuste jõustumiseks tehke järgmist.

```powershell
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```


## <a name="more-information-on-network-security-groups"></a>Lisateavet võrgu turberühmad
Kiirülevaate käsud võimaldavad tööks liiklus minevaid oma VM. Võrgu turberühmad pakuvad palju suurepäraseid omadusi ja granulaarsus juurdepääsu oma ressursse kontrollimisel. Lugege lisateavet [võrgu turberühma ja ACL reeglid siin loomise](../virtual-network/virtual-networks-create-nsg-arm-ps.md)kohta.

Saate määratleda võrgu turberühmad ja ACL reeglite Azure'i ressursihaldur Mallid osana. Lisateavet [võrgu turberühmad malle loomise](../virtual-network/virtual-networks-create-nsg-arm-template.md)kohta.

Kui peate kasutama pordi suunamise vastendamiseks kordumatu välise port oma VM sisemise pordiga, kasutage laadi koormusetasakaalustusteenuse ja Address Translation (NAT) reeglid. Näiteks võite TCP port 8080 väliselt nähtavaks tegemine ja on liiklus suunatud TCP 80 porti VM. Saate teada, [mis Interneti-ühendusega laadi koormusetasakaalustusteenuse loomise](../load-balancer/load-balancer-get-started-internet-arm-ps.md)kohta.

## <a name="next-steps"></a>Järgmised sammud
Selles näites on loodud lihtsa reegli HTTP liikluse lubamiseks. Leiate teavet järgmistest artiklitest üksikasjalikumat keskkonnas loomise kohta:

- [Azure'i ressursihaldur ülevaade](../azure-resource-manager/resource-group-overview.md)
- [Mis on võrgu turvalisus jaotises (NSG)?](../virtual-network/virtual-networks-nsg.md)
- [Azure'i koormus soolise ressursihaldur ülevaade](../load-balancer/load-balancer-arm.md)