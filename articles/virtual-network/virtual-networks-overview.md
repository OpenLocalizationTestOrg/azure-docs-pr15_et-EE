<properties
   pageTitle="Azure virtuaalse võrgu (VNet) ülevaade"
   description="Lisateavet Azure virtuaalne võrkude (VNets)."
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/15/2016"
   ms.author="jdial" />

# <a name="virtual-network-overview"></a>Virtuaalse võrgu ülevaade

Azure virtuaalse võrgu (VNet) on pilves võrgu esitus.  See on loogiline eraldamise Azure'i pilveteenuste sihtotstarbeline tellimuse. IP address plokid, DNS-i sätted, turbepoliitikate ja marsruutimiseks tabelite selles võrgustikus saate täielikult kontrollida. Saate ka täpsemaks segmendi oma VNet alamvõrku sisse ja käivitage IaaS Azure'i virtuaalmasinates (VMs) ja/või [pilveteenustega (PaaS rolli aknad)](../cloud-services/cloud-services-choose-me.md). Lisaks saate luua ühenduse virtuaalse võrgu kohapealse võrgu üks [ühenduvuse suvandid](../vpn-gateway/vpn-gateway-about-vpngateways.md#site-to-site-and-multi-site) saadaval Azure abil. Sisuliselt saate laiendada võrgu Azure, üle täielik kontroll IP address plokid enterprise skaala leiate Azure'i kasu.

Paremini mõistmiseks VNets, Heitke pilk joonis, kus on kuvatud lihtsustatud kohapealse võrgu.

![Kohapealse võrgu](./media/virtual-networks-overview/figure01.png)

Ülaltoodud joonisel on kohapealse võrgu läbi ruuteri avaliku Interneti-ühendus. Saate vaadata ka tulemüüri ruuteri ja DMZ, mis on DNS-serveri ja web serveripargi vahel. Serveripargi web on koormus tasakaalustatud riistvara laadi koormusetasakaalustusteenuse Interneti-ühendus on avatud, mis tarbib ressursid sisemise alamvõrgu abil. Sisemise alamvõrgu on eraldatud DMZ teise tulemüüri, ja hakkab majutama Active Directory domeenikontrolleri serverid andmebaasi serverite ja rakenduste serverid.

Azure saate majutatud samasse võrku, nagu on näidatud alloleval joonisel.

![Azure virtuaalse võrgu](./media/virtual-networks-overview/figure02.png)

Pange tähele, kuidas Azure'i infrastruktuuri võtab ruuteri, mis võimaldab juurdepääsu oma VNet konfiguratsiooni avaliku Interneti rolli. Tulemüürid saate asendada turvalisuse rühmad (NSGs) iga üksiku alamvõrgu rakendatud. Ja füüsiline koormus soolise sõnadega internet sise- ja suunatud koormus soolise Azure.

>[AZURE.NOTE] On kaks juurutamise Azure: klassikaline (tuntud ka kui teenuste haldamine) ja Azure ressursi Manager (ARM). Klassikaline VNets võib lisada ka osaleja rühma või piirkondliku VNet loodud. Kui teil on mõne VNet osaleja nimel, on soovitatav migreerida [seda piirkondliku VNet](virtual-networks-migrate-to-regional-vnet.md).

## <a name="virtual-network-benefits"></a>Virtuaalse võrgu eelised

- **Eraldi**. VNets on üksteisest täielikult eraldatud. Mis võimaldab teil luua ühendamata võrkude arengu, testimine ja tootmise CIDR sama aadress kvartali kasutavad.

- **Avaliku Interneti-ühendus**. Kõik IaaS VMs ja PaaS rolli aknad on VNet pääsevad avaliku Interneti kaudu vaikimisi. Võrgu turberühmad (NSGs) abil saate hallata juurdepääsu.

- **Juurdepääs on VNet VMs**. PaaS rolli aknad ja IaaS VMs saab algatada virtuaalse samasse võrku ja nad saavad ühenduse üksteise abil privaatne IP-aadressid, isegi juhul, kui need on teise alamvõrku, ilma vajaduseta lüüsi konfigureerimine ja kasutamine avaliku IP-aadressid.

- **Nimelahendus**. Azure'i annab sisemise nimelahendus IaaS VMs ja PaaS rolli aknad, mida kasutatakse teie VNet. Saate oma DNS-serverid juurutada ja konfigureerida VNet neid kasutada.

- **Turvalisus**. Liikluse sisestamine ja väljumist virtuaalmasinates ja PaaS rolli aknad on VNet saab kontrollida, kasutades võrgu turberühmad.

- **Ühenduvus**. VNets üksteisest võrgu lüüsid või VNet silmitsemine abil saab ühendada. VNets saab ühendada kohapealse andmekeskuste-saidilt VPN võrgu või Azure'i ExpressRoute kaudu. Saidilt virtuaalse Privaatvõrgu ühendus kohta lisateabe saamiseks külastage [kohta VPN lüüsid](../vpn-gateway/vpn-gateway-about-vpngateways.md#site-to-site-and-multi-site). ExpressRoute kohta lisateabe saamiseks külastage [ExpressRoute tehniline ülevaade](../expressroute/expressroute-introduction.md). VNet silmitsemine kohta lisateabe saamiseks külastage [VNet silmitsemine](virtual-network-peering-overview.md).

    >[AZURE.NOTE] Veenduge, et loote on VNet enne juurutamist Azure keskkonna mis tahes IaaS VMs või PaaS rolli aknad. ARM vastavalt VMs nõua on VNet ja kui määrate mõne olemasoleva VNet Azure'i luuakse vaikimisi VNet, mis võib olla, et CIDR aadressi blokeerimine vastuolu kohapealse võrgu. Muutes te oma VNet ühenduse loomiseks kohapealse võrgu.

## <a name="subnets"></a>Alamvõrku

Alamvõrgu on vahemik IP-aadresside, klõpsake soovitud VNet, on VNet saab jagada mitu alamvõrku ettevõte ja turvalisus. VMs ja PaaS rolli aknad juurutatud sees on VNet alamvõrku (samas või mõnes muus) saate omavahel suhelda ilma eest configuration. Samuti saate konfigureerida marsruutimiseks tabelite ja NSGs on alamvõrgu.

## <a name="ip-addresses"></a>IP-aadressid


On kahte tüüpi IP-aadresside määratud ressursid Azure: *avalike* ja *privaatvõ*. Avaliku IP-aadresside luba suhelda Interneti ja muude Azure avalikkusele suunatud teenuste nagu [Azure'i Redis vahemälu](https://azure.microsoft.com/services/cache/), [Azure'i sündmuse jaoturi](https://azure.microsoft.com/documentation/services/event-hubs/)Azure ressursse. Privaatne IP-aadresside abil suhtlemine virtuaalse võrku, koos kaudu VPN-iga seotud ressursid, kasutamata Interneti-marsruuditavaid IP-aadressid.

Azure'i IP-aadresside kohta lisateabe saamiseks külastage [virtuaalse võrgu IP-aadressid](virtual-network-ip-addresses-overview-arm.md)

## <a name="azure-load-balancers"></a>Azure'i koormus soolise

Interneti kaudu Azure'i koormus soolise võivad puutuda virtuaalmasinates ja pilveteenustega Virtual võrku. Ärirakenduste, mis on sisemine ühendusega saab ainult koormus tasakaalustatud sisemise koormuse koormusetasakaalustusteenuse abil.

- **Välise koormuse koormusetasakaalustusteenuse**. Saate ka välise koormuse koormusetasakaalustusteenuse kõrge-saadavus ette IaaS VMs ja PaaS rolli aknad avaliku Interneti kaudu juurde pääseda.

- **Sisemised laadimine koormusetasakaalustusteenuse**. Saate ka sisemise koormuse koormusetasakaalustusteenuse kõrge-saadavus ette IaaS VMs ja PaaS rolli aknad pääseb teie VNet muud teenused.

Laadi tasakaalustamiseks Azure kohta lisateabe saamiseks külastage [koormuse koormusetasakaalustusteenuse ülevaade](../load-balancer/load-balancer-overview.md).

## <a name="network-security-group-nsg"></a>Võrgu turberühma (NSG)

Saate luua NSGs võrgu liidesed (NICs) sissetulevate ja väljaminevate juurdepääsu piiramiseks VMs ja alamvõrku. Iga NSG sisaldab ühte või IP-aadressi allikat, lähteport, sihtkoha IP-aadress ja sihtport põhineva lisareeglite määrata, kas liikluse on kinnitatud või keelatud. NSGs kohta lisateabe saamiseks külastage, [mis on võrgu turberühma](virtual-networks-nsg.md).

## <a name="virtual-appliances"></a>Virtuaalne seadmed

Virtuaalse seadme on lihtsalt üks teie VNet, mis töötab vastavalt tarkvara seadme funktsiooni, näiteks tulemüüri, WAN optimeerimine või sissetungi tuvastamise VM. Saate luua marsruudi Azure abil saate marsruutida liiklust VNet kaudu virtuaalse seadme oma võimaluste kasutamiseks.

Näiteks NSGs saab anda oma VNet turvalisus. Siiski NSGs pakuvad layer 4 juurdepääsu juhtimine loend (ACL) sissetulevad ja väljaminevad paketid. Kui soovite kasutada mudeli layer 7 turvalisus, peate kasutama tulemüüri seadme.

[Kasutaja määratletud marsruudib ja IP ümbersuunamine](virtual-networks-udr-overview.md)sõltuvad virtuaalne seadmed.

## <a name="limits"></a>Piirangud
On lubatud tellimuse virtuaalne võrkude maksimumarvuga, leiate [Azure'i Networking limiitide kohta](../azure-subscription-service-limits.md#networking-limits) lisateabe saamiseks.

## <a name="pricing"></a>Hinnad
Seal on mingit täiendavat kulu virtuaalse Azure abil. Arvuta eksemplarid, mis on Vnet jooksul tuleb tasuda ühtsete [Azure'i VM hinnad](https://azure.microsoft.com/pricing/details/virtual-machines/)kirjeldatud. [VPN lüüside](https://azure.microsoft.com/pricing/details/vpn-gateway/) ja [avaliku IP-aadresside] (https://azure.microsoft.com/pricing/details/ip-addresses/) on VNet kasutusel on ka laetud ühtsete.

## <a name="next-steps"></a>Järgmised sammud

- [Loo on VNet](virtual-networks-create-vnet-arm-pportal.md) ja alamvõrku.
- [Luua VM on VNet sisse](../virtual-machines/virtual-machines-windows-hero-tutorial.md).
- Lisateavet [NSGs](virtual-networks-nsg.md).
- Teave [kasutaja määratletud marsruudib ja IP ümbersuunamine](virtual-networks-udr-overview.md).
