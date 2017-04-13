<properties
   pageTitle="Azure'i ressursihaldur tugi laadi koormusetasakaalustusteenuse | Microsoft Azure'i "
   description="Laadi koormusetasakaalustusteenuse Azure'i ressursihaldur PowerShelli abil. Laadi koormusetasakaalustusteenuse abil"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />


# <a name="using-azure-resource-manager-support-with-azure-load-balancer"></a>Azure'i laadi koormusetasakaalustusteenuse Azure'i ressursihaldur tugiteenuse kasutamine

Azure'i ressursihaldur on eelistatud management framework Azure teenuste jaoks. Azure'i laadi koormusetasakaalustusteenuse saab hallata Azure ressursihaldur vastavalt API-d ja tööriistade abil.

## <a name="concepts"></a>Mõisted

Ressursi halduri Azure'i laadi koormusetasakaalustusteenuse sisaldab lapse järgmistest allikatest:

- Laadi koormusetasakaalustusteenuse sisaldada ees IP konfiguratsioon – üks või mitu esiosa IP-aadressid, muidu tuntud virtuaalse IP-d (VIP). IP-aadressid olla sissepääsu liikluse jaoks.

- Tagaandmebaas aadress pool – need on virtuaalse masina võrgu kasutajaliidese kaart (midagi) mis laadi jagatakse seotud IP-aadressid.

- Laadi tasakaalustavad reeglid – reegli atribuudi kaartide antud esiosa IP ja port kombinatsiooni kogumisse tagasi end IP-aadresside ja portide kombinatsioon. Ühe laadi koormusetasakaalustusteenuse võib olla mitu laadi tasakaalustamiseks reeglid. Iga reegli puhul on ees IP ja pordi ja tagaandmebaas IP ja pordi seostatud VMs kombinatsioon.

- Sondid – sondid võimaldavad teil silma peal seisundi VM juhtudel. Kui soovitud seisundi juures nurjub, võetakse VM eksemplari pööre välja automaatselt.

- Sissetuleva NAT reeglid – NAT reeglite määratlemise Sissetuleva liikluse läbib ees IP lõpetamine ja jaotatud tagasi end IP.

![](./media/load-balancer-arm/load-balancer-arm.png)

## <a name="quickstart-templates"></a>Kiirjuhend Mallid

Azure'i ressursihaldur võimaldab ettevalmistamise rakenduste deklaratiivseid malli abil. Ühe malli, saate juurutada mitme teenuseid koos nende sõltuvusi. Sama malli abil saate korduvalt juurutada rakendust igas etapis rakenduse elutsükli.

Malle saate kaasata määratlused Virtuaalmasinates, virtuaalse võrgu, kättesaadavus komplekti võrgu liidesed (NICs), salvestusruumi kontod, koormus soolise, võrgu turberühmad ja avaliku IP-d. Mallide abil saate luua kõike, mida vajate keerukate rakenduse. Mallifail saab kontrollida sisuhaldus süsteemi versiooni kontrollimine ja koostöö jaoks.

[Lisateavet mallide kohta](http://go.microsoft.com/fwlink/?LinkId=544798)

[Lisateavet võrgu ressursid](../virtual-network/resource-groups-networking.md)

Kiirjuhend mallide abil Azure'i laadi koormusetasakaalustusteenuse leiate [GitHub hoidla](https://github.com/Azure/azure-quickstart-templates) majutusteenuse ühenduse loodud mallide.

Mallide näited:

- [2 VMs koormusetasakaalustusteenuse laadimine ja laadi tasakaalustamiseks reeglid](http://go.microsoft.com/fwlink/?LinkId=544799)
- [2 mõne VNET sisemise laadi koormusetasakaalustusteenuse ja laadi koormusetasakaalustusteenuse reegliga VMs](http://go.microsoft.com/fwlink/?LinkId=544800)
- [Laadi koormusetasakaalustusteenuse 2 VMs ja konfigureerige NAT reeglite LB](http://go.microsoft.com/fwlink/?LinkId=544801)


## <a name="setting-up-azure-load-balancer-with-a-powershell-or-cli"></a>Azure'i laadi koormusetasakaalustusteenuse PowerShelli või CLI seadistamine

Azure'i ressursihaldur cmdlet-käsud, käsurea tööriistade ja REST API-de kasutamise alustamine

- [Azure'i Networking](https://msdn.microsoft.com/library/azure/mt163510.aspx) saab kasutada laadi koormusetasakaalustusteenuse loomiseks.
- [Laadi koormusetasakaalustusteenuse, kasutades Azure ressursihaldur loomise kohta](load-balancer-get-started-ilb-arm-ps.md)
- [Azure'i CLI kasutades Azure ressursside haldamine](../xplat-cli-azure-resource-manager.md)
- [Laadi koormusetasakaalustusteenuse REST API-d](https://msdn.microsoft.com/library/azure/mt163651.aspx)


## <a name="next-steps"></a>Järgmised sammud

Võite ka [vastastikuste laadi koormusetasakaalustusteenuse Interneti loomise alustamiseks](load-balancer-get-started-internet-arm-ps.md) ja mis tüüpi teatud laadi koormusetasakaalustusteenuse võrgu liikluse käitumine [jaotuse režiimi](load-balancer-distribution-mode.md) konfigureerimine.

Saate teada, kuidas hallata [jõudeolekus TCP ajalõpp sätteid laadi koormusetasakaalustusteenuse](load-balancer-tcp-idle-timeout.md). See on oluline, kui teie rakendus vajab ühendused toitsid serverite laadi koormusetasakaalustusteenuse taha.
