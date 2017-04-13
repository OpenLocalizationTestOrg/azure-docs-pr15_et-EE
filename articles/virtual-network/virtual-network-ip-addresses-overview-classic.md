<properties
   pageTitle="Avalike ja privaatvõ IP tegelemine (klassikaline) Azure | Microsoft Azure'i"
   description="Lisateavet avalike ja privaatvõ IP tegelemine Azure"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-service-management" />
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/11/2016"
   ms.author="jdial" />

# <a name="ip-addresses-classic-in-azure"></a>IP-aadressid (klassikaline) Azure
Saate määrata IP-aadresside Azure ressursse suhelda muud Azure ressursid, kohapealse võrgu ja Interneti-ühendus. On kahte tüüpi IP-aadressid, saate kasutada Azure: avalike ja privaatvõ.

Interneti, sh Azure avalikkusele suunatud teenuste suhtlemiseks kasutatakse avaliku IP-aadressid.

Privaatne IP-aadressid, mida kasutatakse side Azure virtuaalse võrgu (VNet), pilveteenus ja kohapealse võrgu VPN-lüüsi või ExpressRoute ringi laiendada võrgu Azure kasutamisel.

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Siit saate teada, kuidas [nende](virtual-network-ip-addresses-overview-arm.md)toimingute abil ressursihaldur juurutamise mudeli.

## <a name="public-ip-addresses"></a>Avaliku IP-aadressid
Avaliku IP-aadresside luba Azure ressursse suhelda Interneti- ja Azure avalikkusele suunatud teenused, nt [Azure Redis vahemälu](https://azure.microsoft.com/services/cache/), [Azure'i sündmuse jaoturi](https://azure.microsoft.com/services/event-hubs/), [SQL andmebaase](../sql-database/sql-database-technical-overview.md)ja [Azure salvestusruumi](../storage/storage-introduction.md).

Avaliku IP-aadress on seotud ressursside järgmist tüüpi:

- Pilveteenused
- IaaS Virtuaalmasinates (VM)
- PaaS rolli aknad
- VPN lüüsid
- Rakenduse lüüsid

### <a name="allocation-method"></a>Eraldatud meetod
Kui avaliku IP-aadress peab olema määratud Azure ressursside, on *dünaamiliselt* jagatakse on saadaval avaliku IP-aadressi ressursi luuakse asukohas. IP-aadress on välja, kui ressursi lõpetada. Juhuks, kui mõnda pilveteenusesse, see juhtub, kui kõik rolli aknad on peatatud, mis saate vältida *staatilise* (reserveeritud) IP-aadressi abil (vt [Pilveteenustega](#Cloud-services)).

>[AZURE.NOTE] IP-aadresside vahemikud, millest avaliku IP-aadressid on eraldatud Azure loend on avaldatud [Azure'i andmekeskuse IP-vahemikke](https://www.microsoft.com/download/details.aspx?id=41653).

### <a name="dns-hostname-resolution"></a>DNS-i hostinime lahenduse
Kui loote mõnda pilveteenusesse või mõne IaaS VM, peate pilvepõhise teenuse DNS-i nimi, mis on kordumatu üle kõik ressursid Azure. See loob vastenduse Azure'i hallatav DNS Servers jaoks *dnsnameWindows*. cloudapp.net avaliku IP-aadressi ressursi. Näiteks pilvepõhise teenuse DNS-i nimi **contoso**pilveteenus loomisel täielikult kvalifitseeritud domeeni nimi (FQDN) **contoso.cloudapp.net** lahendab pilvepõhise teenuse avaliku IP (VIP). Selle FQDN-i abil saate luua kohandatud domeeni CNAME-kirje, mis osutab Azure'i avaliku IP-aadress.

### <a name="cloud-services"></a>Pilveteenused
Pilveteenus on alati virtuaalse IP-aadress (VIP) edaspidi avaliku IP-aadress. Saate luua pilveteenuses seostamiseks erinevate pordid VIP sisemise pordid VMs ja rolli aknad teenuses cloud lõpp-punktid. 

Pilveteenus võib sisaldada mitut IaaS VMs või PaaS rolli aknad, kõik avatud sama pilvepõhise teenuse VIP kaudu. Saate määrata ka [mitu VIP pilveteenusesse](../load-balancer/load-balancer-multivip.md), mis võimaldab mitme-VIP stsenaariumid nagu mitme rentniku keskkond veebisaitidega SSL-i-põhine.

Saate tagada pilveteenus avaliku IP-aadress jääb samaks, isegi siis, kui kõik rolli aknad peatatud *staatiline* avaliku IP-aadress, [Reserveeritud IP](virtual-networks-reserved-public-ip.md)edaspidi abil. Saate luua staatiline IP (reserveeritud) ressursi teatud asukohta ja määrata mis tahes pilveteenuses asukohta. Te ei saa määrata tegeliku IP-aadressi jaoks reserveeritud IP, see on jagatakse saadaval IP-aadresside loomist asukohta. IP-aadress on vabastatud kuni konkreetselt kustutate.

Staatilise (reserveeritud) avaliku IP-aadressid on levinud stsenaariume kui pilveteenus:

- nõuab tulemüüri reeglite häälestamise lõppkasutajatega.
- DNS-i nimelahendus, sõltub ja dünaamiline IP nõuab soovitud kirjete värskendamise.
- tarbib vastavalt IP turvalisus mudeli kasutamine väliste veebiteenused.
- kasutab SSL-sertide seotud IP-aadress.

>[AZURE.NOTE] Klassikaline VM loomisel luuakse container *pilveteenuses* Azure'i, mis on virtuaalse IP-aadress (VIP). Kui loomine on lõpule jõudnud, portaali kaudu, default RDP või SSH *lõpp-punkti* on konfigureeritud portaali nii, et saate luua ühenduse kaudu pilvepõhise teenuse VIP VM. Saate reserveerida selle pilvepõhise teenuse VIP, mis pakub ühenduse VM reserveeritud IP-aadress. Saate avada täiendavaid pordid konfigureerimisega veel lõpp-punktid.

### <a name="iaas-vms-and-paas-role-instances"></a>IaaS VMs ja PaaS rolli aknad
Saate määrata avaliku IP-aadressi otse soovitud IaaS [VM](../virtual-machines/virtual-machines-linux-about.md) või PaaS rolli eksemplari mõnda pilveteenusesse. See on edaspidi eksemplari taseme avaliku IP-aadress ([ILPIP](virtual-networks-instance-level-public-ip.md)). Ainult saab dünaamiliseks avaliku IP-aadressi.

>[AZURE.NOTE] See on erinev ümbris IaaS VMs või PaaS rolli aknad, kuna pilveteenus võib sisaldada mitut IaaS VMs või PaaS rolli aknad, kõik avatud kaudu sama pilvepõhise teenuse VIP pilveteenus VIP.

### <a name="vpn-gateways"></a>VPN lüüsid
[VPN-lüüsi](../vpn-gateway/vpn-gateway-about-vpngateways.md) saab kasutada ka Azure VNet ühenduse muude Azure'i VNets või kohapealse võrgu. VPN-lüüsi on määratud lisamine avaliku IP address *dünaamiliselt*, mis võimaldab suhtlemine võrk.

### <a name="application-gateways"></a>Rakenduse lüüsid
Mõne Azure [rakenduste portaali](../application-gateway/application-gateway-introduction.md) saab kasutada Layer7 koormust tasakaalustavad võrguliikluse marsruutimiseks HTTP põhjal. Rakenduse lüüsi on määratud lisamine avaliku IP address *dünaamiliselt*, mis toimib koormusetasakaalustusega VIP.

### <a name="at-a-glance"></a>Ülevaade
Järgmises tabelis antakse ülevaade iga ressursi tüüp, koos võimalike jaotamise meetodite (dünaamiline/staatiline) ja võimalus määrata mitu avaliku IP-aadressi.

|Ressurss|Dünaamiliste|Staatiline|Mitu IP-aadressid|
|---|---|---|---|
|Pilveteenuses|Jah|Jah|Jah|
|IaaS VM või PaaS rolli eksemplari|Jah|Ei|Ei|
|VPN-lüüsi|Jah|Ei|Ei|
|Rakenduse lüüsi|Jah|Ei|Ei|

## <a name="private-ip-addresses"></a>Privaatne IP-aadressid
Privaatne IP-aadresside luba Azure ressursse muud ressursid, [virtuaalse võrgu](virtual-networks-overview.md)(VNet) või mõnda pilveteenusesse kohapealse võrgu (VPN-lüüsi või ExpressRoute ringi), kaudu suhtlemiseks kasutamata Interneti-kättesaadav IP-aadress.

Azure klassikaline juurutamise mudeli, saate määrata privaatne IP-aadress Azure järgmistest allikatest:

- IaaS VMs ja PaaS rolli aknad
- Sisemise koormuse koormusetasakaalustusteenuse
- Rakenduse lüüsi

### <a name="iaas-vms-and-paas-role-instances"></a>IaaS VMs ja PaaS rolli aknad
Klassikaline juurutamise mudeliga loodud virtuaalmasinates (VM) paigutatakse alati pilveteenuses, mis on sarnane PaaS rolli aknad. Privaatne IP-aadresside käitumine sarnanevad seega need ressursid.

See on oluline märkida pilveteenus loovad kahel viisil:

- Kui *autonoomse* pilveteenus, kui see ei ole virtuaalse võrgustikus.
- Virtuaalse võrgu osana.

#### <a name="allocation-method"></a>Eraldatud meetod
*Autonoomse* pilveteenuses korral saada ressursid on privaatne IP address eraldatud *dünaamiliselt* Azure andmekeskuse privaatne IP aadresse. Seda saab kasutada ainult teiste sama pilveteenuses VMs suhtlemiseks. IP-aadressi muutmiseks kui ressurss on peatatud ja alustada.

Korral juurutatud virtuaalse võrgustikus pilveteenus ressursid saada privaatne IP aadressid eraldatud aadress vahemikus seotud subnet(s) (vastavalt oma võrgukonfiguratsioon). See privaatne IP-aadressid saab kasutada kõigi selle VNet VMs vahel.

Lisaks puhul pilveteenuste sees on VNet, Privaatne IP-aadress on eraldatud *dünaamiliselt* (abil DHCP) vaikimisi. Saate muuta, kui ressurss on peatatud ja alustada. IP-aadress ei muutu tagamiseks peate määratud eraldatud meetodit *staatilise*ja sisestage vastava aadress vahemikus lubatud IP-aadress.

Staatilise privaatne IP-aadressid on tavaliselt kasutatakse:

 - VMs, mis toimivad domeenikontrollerite või DNS-serverid.
 - VMs, mis nõuavad tulemüüri reeglite abil IP-aadressid.
 - VMs, mis töötab teenuste juurde muude rakenduste kaudu IP-aadress.

#### <a name="internal-dns-hostname-resolution"></a>Sisemise DNS-i hostinime lahenduse
Kõik Azure VMs ja PaaS rolli aknad on konfigureeritud [Azure hallatav DNS-serverid](virtual-networks-name-resolution-for-vms-and-role-instances.md#azure-provided-name-resolution) vaikimisi juhul, kui otseselt konfigureerida kohandatud DNS-serverid. Need DNS-i serverid ette sisemise nimelahendus VMs ja rolli eksemplarid, mis asuvad samas VNet või pilvepõhises teenuses.

Kui loote VM, lisatakse jaoks hostname oma privaatse IP-aadressi vastenduse Azure'i hallatav DNS servers. Hostname vastendatakse korral mitme-NIC VM, esmane NIC. privaatne IP-aadress Siiski, selle vastenduse teave on piiratud ressursid sama pilveteenuses või VNet.

*Autonoomse* pilveteenuses korral on võimalik lahendada hostinimed kõik VMs/rolli aknad sees ainult sama pilveteenuses. Sees on VNet pilveteenus korral saab hostinimed VMs/rolli eksemplaride sees on VNet lahendamiseks.

### <a name="internal-load-balancers-ilb--application-gateways"></a>Sisemise koormus soolise (ILB) ja rakendus lüüsid
Saate määrata [Azure'i sisemine laadi koormusetasakaalustusteenuse](../load-balancer/load-balancer-internal-overview.md) (ILB) või mõne [Azure'i rakenduse lüüsi](../application-gateway/application-gateway-introduction.md) **esiosa** konfiguratsiooni privaatne IP-aadress. Ressursid (VNet) oma virtuaalse võrgustikus juurdepääs ainult sisemise lõpp-toimib privaatne IP-aadress ja Kaug võrguga ühendatud on VNet. Saate määrata kas dünaamilised või staatilised privaatne IP-aadress esiosa konfigureerimine. Saate määrata ka mitu privaatne IP-aadressi lubamiseks mitme-vip stsenaariumid.

### <a name="at-a-glance"></a>Ülevaade
Järgmises tabelis antakse ülevaade iga ressursi tüüp, koos võimalike jaotamise meetodite (dünaamiline/staatiline) ja võimalus määrata mitu privaatne IP-aadressi.

|Ressurss|Dünaamiliste|Staatiline|Mitu IP-aadressid|
|---|---|---|---|
|VM (pilveteenuses *eraldi* )|Jah|Jah|Jah|
|PaaS rolli eksemplari (pilveteenuses *eraldi* )|Jah|Ei|Jah|
|VM või PaaS rolli astme (on VNet)|Jah|Jah|Jah|
|Sisemise koormuse koormusetasakaalustusteenuse esiosa.|Jah|Jah|Jah|
|Rakenduse lüüsi esiosa.|Jah|Jah|Jah|

## <a name="limits"></a>Piirangud

Järgmises tabelis antakse ülevaade raadiuse IP tegelemine Azure tellimuse kohta. Saate vaikimisi piirangud maksimaalne piires vastavalt teie ettevõtte vajadustele suurendamiseks [pöörduge klienditoe poole](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade) .

||Vaikimisi limiit|Lubatud|
|---|---|---|
|Avaliku IP-aadressid (dünaamiline)|5|tugiteenuste osakonna poole pöördumine|
|Reserveeritud avaliku IP-aadressid|20|tugiteenuste osakonna poole pöördumine|
|Avaliku VIP juurutamise (pilveteenuses) kohta.|5|tugiteenuste osakonna poole pöördumine|
|Privaatne VIP (ILB) juurutamise (pilveteenuses) kohta.|1|1|

Veenduge, et loete täiskomplekti [piirab Networking](azure-subscription-service-limits.md#networking-limits) Azure.

## <a name="pricing"></a>Hinnad

Enamikul juhtudel on tasuta avaliku IP-aadressid. Ei ole nominal tasuta kasutada täiendavaid ja/või staatilise avaliku IP-aadressid. Veenduge, et mõistate [hinnad struktuuri avaliku IP-d](https://azure.microsoft.com/pricing/details/ip-addresses/).

## <a name="differences-between-resource-manager-and-classic-deployments"></a>Ressursihaldur ja klassikaline juurutuste erinevused
Allpool on ressursihaldur ja klassikaline juurutamise mudel IP adresseerimise funktsioonide võrdlus.

||Ressurss|Klassikaline|Ressursihaldur|
|---|---|---|---|
|**Avaliku IP-aadress**|VM|Nimetatakse ka ILPIP (ainult dünaamiline)|Edaspidi avaliku IP (dünaamiline või staatiline)|
|||Määratud IaaS VM-i või PaaS rolli eksemplari|Funktsiooni VM NIC seotud|
||Internet suunatud laadi koormusetasakaalustusteenuse|Edaspidi VIP (dünaamiline) või reserveeritud IP (staatiline)|Edaspidi avaliku IP (dünaamiline või staatiline)|
|||Määratud pilveteenusesse|Laadi koormusetasakaalustusteenuse esiosa config seotud|
||||
|**Privaatne IP-aadress**|VM|SUPLUST edaspidi|Privaatne IP-aadress edaspidi|
|||Määratud IaaS VM-i või PaaS rolli eksemplari|Funktsiooni VM NIC määratud|
||Sisemise koormuse koormusetasakaalustusteenuse (ILB)|Määratud ILB (dünaamiline või staatiline)|Määratud soovitud ILB esiosa konfigureerimine (dünaamilised või staatilised)|

## <a name="next-steps"></a>Järgmised sammud
- Klassikaline portaalis [Deploy VM staatilise privaatne IP-aadress](virtual-networks-static-private-ip-classic-pportal.md) .
