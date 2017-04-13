<properties
   pageTitle="Avage pordid Azure'i portaalis VM | Microsoft Azure'i"
   description="Saate teada, kuidas avada port / lõpp oma Windows VM ressursi halduri juurutamise mudeli kasutamine Azure portaali loomine"
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

# <a name="opening-ports-to-a-vm-in-azure-using-the-azure-portal"></a>Avades pordid VM Azure Azure'i portaalis
[AZURE.INCLUDE [virtual-machines-common-nsg-quickstart](../../includes/virtual-machines-common-nsg-quickstart.md)]

## <a name="quick-commands"></a>Kiirülevaate käsud
Samuti saate [teha järgmist Azure PowerShelli kaudu](virtual-machines-windows-nsg-quickstart-powershell.md).

Esmalt Looge oma võrgu turberühma. Valige ressursirühma portaalis, klõpsake nuppu **Lisa**, seejärel otsige ja valige "Network turberühma":

![Võrgu turberühma lisamine](./media/virtual-machines-windows-nsg-quickstart-portal/add-nsg.png)

Sisestage oma võrgu turberühma nimi, valige või ressursside rühma loomine ja valige asukoht. Klõpsake nuppu **Loo** lõpetades:

![Võrgu Turberühma loomine](./media/virtual-machines-windows-nsg-quickstart-portal/create-nsg.png)

Valige oma uue võrgu turberühma. Valige 'sissetuleva turvalisuse reeglid' ja seejärel klõpsake nuppu **Lisa** reegli loomiseks.

![Sissetuleva reegli lisamine](./media/virtual-machines-windows-nsg-quickstart-portal/add-inbound-rule.png)

Sisestage oma uue reegli nimi. Pordi 80 on vaikimisi juba sisestatud. See blade on, kus soovite muuta allikas, Protocol (protokoll) ja sihtkoha täiendavad reeglid lisamisel oma võrgu turberühma. Klõpsake nuppu **OK** , et luua reegli:

![Sissetuleva reegli loomine](./media/virtual-machines-windows-nsg-quickstart-portal/create-inbound-rule.png)

Teie viimane toiming on teie võrgu turberühma seostada on alamvõrgu või teatud võrgu liidese. Vaatame seostada võrgu turberühma lisamine alamvõrgu. Valige 'Alamvõrku' ja seejärel klõpsake **seostada**.

![Võrgu turberühma seostada on alamvõrgu](./media/virtual-machines-windows-nsg-quickstart-portal/associate-subnet.png)

Valige virtuaalse võrgu ja seejärel valige sobiv alamvõrgu.

![Võrgu turberühma virtuaalse võrgunduse seostada](./media/virtual-machines-windows-nsg-quickstart-portal/select-vnet-subnet.png)

Nüüd olete loonud võrgu turberühma, loodud Sissetulev reegel, mis liiklust port 80 ja seostatud mõne alamvõrgu. Pordi 80 on mis tahes VMs, kui ühendate selle alamvõrgu.


## <a name="more-information-on-network-security-groups"></a>Lisateavet võrgu turberühmad
Kiirülevaate käsud võimaldavad tööks liiklus minevaid oma VM. Võrgu turberühmad pakuvad palju suurepäraseid omadusi ja granulaarsus juurdepääsu oma ressursse kontrollimisel. Lugege lisateavet [võrgu turberühma ja ACL reeglid siin loomise](../virtual-network/virtual-networks-create-nsg-arm-ps.md)kohta.

Saate määratleda võrgu turberühmad ja ACL reeglite Azure'i ressursihaldur Mallid osana. Lisateavet [võrgu turberühmad malle loomise](../virtual-network/virtual-networks-create-nsg-arm-template.md)kohta.

Kui peate kasutama pordi suunamise vastendamiseks kordumatu välise port oma VM sisemise pordiga, kasutage laadi koormusetasakaalustusteenuse ja Address Translation (NAT) reeglid. Näiteks võite TCP port 8080 väliselt nähtavaks tegemine ja on liiklus suunatud TCP 80 porti VM. Saate teada, [mis Interneti-ühendusega laadi koormusetasakaalustusteenuse loomise](../load-balancer/load-balancer-get-started-internet-arm-ps.md)kohta.

## <a name="next-steps"></a>Järgmised sammud
Selles näites on loodud lihtsa reegli HTTP liikluse lubamiseks. Leiate teavet järgmistest artiklitest üksikasjalikumat keskkonnas loomise kohta:

- [Azure'i ressursihaldur ülevaade](../azure-resource-manager/resource-group-overview.md)
- [Mis on võrgu turvalisus jaotises (NSG)?](../virtual-network/virtual-networks-nsg.md)
- [Azure'i koormus soolise ressursihaldur ülevaade](../load-balancer/load-balancer-arm.md)
