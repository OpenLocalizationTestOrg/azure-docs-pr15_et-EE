<properties 
   pageTitle="Võrgu liidesed | Microsoft Azure'i"
   description="Lisateavet Azure võrgu liidesed Azure'i ressursihaldur."
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
   ms.date="09/23/2016"
   ms.author="jdial" />

# <a name="network-interfaces"></a>Võrgu liidesed

Võrgu liidese (NIC) on virtuaalse masina (VM) ja tarkvara võrgu vaheline ühendus. Selles artiklis selgitatakse võrgu liidese on ja kuidas seda kasutatakse Azure ressursihaldur juurutamise mudeli.

Microsoft soovitab ressursihaldur juurutamise näidise uusi ressursse rakendades, kuid saate juurutada VMs [klassikaline](virtual-network-ip-addresses-overview-classic.md) juurutamise mudeli võrguühendus. Kui olete tuttav klassikaline, on oluline erinevused VM võrgunduse ressursihaldur juurutamise mudeli. Lugege lisateavet erinevuste kohta artiklist [virtuaalse masina networking – klassikaline](virtual-network-ip-addresses-overview-classic.md#differences-between-resource-manager-and-classic-deployments) .

Azure, võrgu liidese:

1. On ressurss, mille saab luua, kustutada, ja on oma konfigureeritav sätted.
2. Peab olema ühendatud ühe alamvõrgu ühe Azure virtuaalse võrku (VNet), kui see on loodud. Kui olete tuttav VNets, lugege lisateavet nende [Virtual võrgu ülevaate](virtual-networks-overview.md) artiklist. NIC peab olema ühendatud VNet, mis on olemas sama Azure [asukoht](https://azure.microsoft.com/regions) ja [tellimuse](../azure-glossary-cloud-terminology.md#subscription) nimega NIC. Võrguadapter on loodud, saate muuta alamvõrku, mis on ühendatud, kuid te ei saa muuta VNet, mis on ühendatud.
3. Nimi on määratud sellele, mida ei saa muuta NIC loomise järel. Nimi peab olema kordumatu ja Azure [ressursirühm](../azure-resource-manager/resource-group-overview.md#resource-groups), kuid ei pea olema kordumatu tellimus, on loodud Azure asukoha või VNet, mis on ühendatud. Tavaliselt on mitu NICs loodud Azure tellimuse. Soovitatav on, et saate töötada nimereeglistik, mis teeb haldamise mitu NICs lihtsam, kui vaikenimed. [Soovitatav nimereeglid Azure ressursid](../guidance/guidance-naming-conventions.md) saamiseks lugege artiklit soovitusi.
4. Võivad olla seotud VM, kuid saab lisada ainult ühe VM, mis on olemas NIC. samasse asukohta
5. On MAC-aadress, mis on jätkunud NIC jaoks koos seni, kuni see jääb külge VM. MAC-aadress on jätkunud kas VM taaskäivitamist (alates jooksul operatsioonisüsteemi) või peatatud (tühistage eraldatud) ja Azure portaali, Azure PowerShelli või Azure käsurea liides kasutusele võtnud. Kui see on lahti VM kaudu ja manustatud erinevate VM, saab NIC erinevate MAC-aadress. Kui kustutatakse NIC, MAC-aadress on määratud muude NICs.
6. Peab olema üks esmane **Privaatne** staatiline või dünaamiline *IPv4* IP-aadress see määratud.
8. Võib-olla ühte sellega seotud avaliku IP-aadressi ressursi.
9. Toetab kiirendada võrgunduse ja ühe-root I/O virtualization (SR-IOV) kindlad VM suurused teatud versioonidega operatsioonisüsteemi Microsoft Windows Server. Selle EELVAATE funktsiooni kohta lisateabe saamiseks lugege artiklit [kiirendatud networking virtuaalse masina jaoks](virtual-network-accelerated-networking-powershell.md) .
10. Saate vastu võtta liikluse sinna pole määratud, kui on lubatud IP ümbersuunamine NIC. privaatne IP-aadressid Kui VM töötab näiteks tulemüüri tarkvara, see marsruudib pole mõeldud oma IP-aadressid. VM peab siiski käivitada tarkvara marsruutimine või liikluse ümbersuunamine, kuid selleks IP ümbersuunamine peab olema lubatud on NIC.
11. Sageli luuakse sama ressursirühm on seotud VM või sama VNet, mis on ühendatud, kuigi see ei pea olema.

## <a name="vms-with-multiple-network-interfaces"></a>Mitme võrgu liidesed VMs

Mitu NICs saate manustatud VM, kuid kui nii, siis pidage silmas järgmist:  

- VM maht peab toetama mitu NICs. Lisateavet selle kohta, mis toetavad VM suurused mitu NICs, lugege artikleid [Windows Server VM suurused](../virtual-machines/virtual-machines-windows-sizes.md) ja [Linux VM suurused](../virtual-machines/virtual-machines-linux-sizes.md) .   
- VM peab olema vähemalt kaks NICs luua. Kui VM on loodud ainult üks NIC, isegi siis, kui VM suurus toetab rohkem kui üks, ei saa te lisada täiendavad NICs VM pärast selle loomist. Kui vähemalt kaks NICs VM on loodud, saate manustada täiendavate NICs VM pärast selle loomist, kui VM suurus toetab rohkem kui kaks NICs.  
- Teisene NICs (esmane NIC ei saa lahti) saate eemaldada VM, kui VM on seotud vähemalt kolme NICs. NICs ei saa eemaldada, kui VM on kaks või vähem NICs kinnitatud.  
- NICs kaudu sees VM järjestuse saab juhusliku ja võib muutuda ka kogu Azure'i infrastruktuuri värskendusi. Siiski IP-aadressid ja vastavate ethernet MAC aadressid, jäävad samaks. Näiteks Oletame, et operatsioonisüsteem tuvastab Azure'i NIC1 Eth1 nimega. Eth1 on IP-aadress 10.1.0.100 ja MAC-aadressi 00-0D-3A-B0-39-0D. Pärast Azure infrastruktuur värskendada ja taaskäivitage, operatsioonisüsteem nüüd tuvastada, Azure'i NIC1 nimega Eth2, kuid IP- ja MAC-aadressid olema sama, nagu need olid kui operatsioonisüsteem tuvastatud Azure'i NIC1 Eth1 nimega. Kui taaskäivitamine on kliendi algatatud, NIC tellimuse jäävad samaks jooksul operatsioonisüsteemi.  
- Kui VM on liige on [kättesaadavus](../azure-glossary-cloud-terminology.md#availability-set), kõik VMs kättesaadavus määramine peab olema üks NIC või mitu NICs. Kui VMs on mitu NICs, arv, nad on ei pea olema sama, kui iga VM on vähemalt kaks NICs.

## <a name="next-steps"></a>Järgmised sammud

- Siit saate teada, kuidas luua VM ühe NIC [loomine VM](../virtual-machines/virtual-machines-windows-hero-tutorial.md) artiklist.
- Saate teada, kuidas luua VM mitu NICs [Deploy VM koos mitme NIC](virtual-network-deploy-multinic-arm-ps.md) artiklist.
- Saate teada, kuidas luua mõne NIC mitme IP konfiguratsioone artiklist [Azure'i virtuaalmasinates jaoks mitu IP-aadressid](virtual-network-multiple-ip-addresses-powershell.md) .
