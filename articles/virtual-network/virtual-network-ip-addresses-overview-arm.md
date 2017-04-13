<properties
   pageTitle="Avalike ja privaatvõ IP tegelemine Azure'i ressursihaldur | Microsoft Azure'i"
   description="Lisateavet avalike ja privaatvõ IP tegelemine Azure'i ressursihaldur"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-resource-manager" />
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="04/27/2016"
   ms.author="jdial" />

# <a name="ip-addresses-in-azure"></a>Azure'i IP-aadressid
Saate määrata IP-aadresside Azure ressursse suhelda muud Azure ressursid, kohapealse võrgu ja Interneti-ühendus. On kahte tüüpi saate kasutada Azure IP-aadressid.

- **Avaliku IP-aadressid**: Internet, sh Azure avalikkusele suunatud teenuste suhtlemiseks kasutada
- **Privaatne IP-aadressid**: edastamiseks Azure virtuaalse võrgus (VNet) kasutada, ja teie asutusesisese network VPN-lüüsi või ExpressRoute ringi laiendada võrgu Azure kasutamisel.

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][klassikaline juurutamise mudel](virtual-network-ip-addresses-overview-classic.md).

Kui olete tuttav klassikaline juurutamise mudeli, märkige ruut selle [IP-aadresside klassikaline vaheliste erinevuste ja ressursihaldur](virtual-network-ip-addresses-overview-classic.md#Differences-between-Resource-Manager-and-classic-deployments).

## <a name="public-ip-addresses"></a>Avaliku IP-aadressid
Avaliku IP-aadresside luba Azure ressursse suhelda Interneti- ja Azure avalikkusele suunatud teenused, nt [Azure Redis vahemälu](https://azure.microsoft.com/services/cache/), [Azure'i sündmuse jaoturi](https://azure.microsoft.com/services/event-hubs/), [SQL andmebaase](../sql-database/sql-database-technical-overview.md)ja [Azure salvestusruumi](../storage/storage-introduction.md).

Azure'i ressursihaldur on [avaliku IP](resource-groups-networking.md#public-ip-address) -aadress on ressurss, mis on oma omadused. Avaliku IP-aadressi ressursi saate seostada ühte järgmistest allikatest:

- Virtuaalmasinates (VM)
- Interneti-ühendusega koormus soolise
- VPN lüüsid
- Rakenduse lüüsid

### <a name="allocation-method"></a>Eraldatud meetod
On kaks võimalust, kus eraldatakse ressursile *avaliku* IP - *dünaamiline* või *staatiline*IP-aadress. Eraldatud vaikemeetod on *dünaamiline*, kui IP-aadress **on eraldatud selle loomise ajal** . Selle asemel avaliku IP-aadress on eraldatud käivitamisel (või luua) seotud ressursside (nt VM või laadi koormusetasakaalustusteenuse). IP-aadress on välja peatamine (või kustutamisel) ressursi. See põhjustab IP-aadressi muutmiseks saate peatada ja käivitada ressursi.

Tagada seotud ressursside IP-aadress jääb samaks, saate seada eraldatud meetodit konkreetselt *staatilise*. Sel juhul IP-aadress on määratud kohe. See on välja ainult siis, kui kustutate ressursi või muuta selle eraldatud *dünaamiliseks*.

>[AZURE.NOTE] Isegi siis, kui seate eraldatud meetodit *staatiline*, ei saa määrata tegeliku *avaliku IP ressursi*määratud IP-aadress. Selle asemel saab eraldada kogumi saadaval IP-aadresside ressursi luuakse Azure asukohta.

Staatilise avaliku IP-aadresside kasutatakse tavaliselt järgmistel juhtudel:

- Lõppkasutajad värskendama tulemüüri reeglid suhtlemiseks oma Azure ressursse.
- DNS-i nimelahendus, kus IP-aadress muudatuste nõuaks soovitud kirjete värskendamise.
- Muude rakenduste ja teenuste IP address põhineva turvalisuse mudelit, mis kasutavad suhelda oma Azure ressursse.
- Saate kasutada SSL-sertide seotud IP-aadress.

>[AZURE.NOTE] IP-aadresside vahemikud, millest avalike IP-aadresside (dünaamiline/staatiline) on eraldatud Azure loend on avaldatud [Azure'i andmekeskuse IP-vahemikke](https://www.microsoft.com/download/details.aspx?id=41653).

### <a name="dns-hostname-resolution"></a>DNS-i hostinime lahenduse
Saate määrata avaliku IP ressurssi, mis loob *domainnamelabel*vastendus sildi DNS-i domeeni nimi. *asukoha*. cloudapp.azure.com Azure hallatav DNS servers avaliku IP-aadress. Näiteks kui loote avaliku IP ressursi **contoso** nimega *domainnamelabel* **Lääne USA** Azure *asukohta*, lahendab täielikult kvalifitseeritud domeeni nimi (FQDN) **contoso.westus.cloudapp.azure.com** avaliku IP-aadressi ressursi. Selle FQDN-i abil saate luua kohandatud domeeni CNAME-kirje, mis osutab Azure'i avaliku IP-aadress.

>[AZURE.IMPORTANT] Iga domeeni nimesildi loodud peab olema kordumatu oma Azure asukohas.  

### <a name="virtual-machines"></a>Virtuaalmasinates
Saate seostada avaliku IP-aadressi [Windows](../virtual-machines/virtual-machines-windows-about.md) või [Linux](../virtual-machines/virtual-machines-linux-about.md) VM määrates selle **võrgu liidese**. Mitme võrgu kasutajaliidese VM puhul saate määrata see *peamine* võrgu liidese ainult. Saate määrata staatilise avaliku IP-aadress või dünaamiline VM.

### <a name="internet-facing-load-balancers"></a>Interneti-ühendusega koormus soolise
Avaliku IP-aadressi saate seostada ka [Azure laadimine koormusetasakaalustusteenuse](../load-balancer/load-balancer-overview.md)määrates laadi koormusetasakaalustusteenuse **frontend** konfigureerimine. Avaliku IP-aadressi toimib koormusetasakaalustusega virtuaalse IP-aadressi (VIP). Laadi koormusetasakaalustusteenuse, mis on ees, saate määrata dünaamiline või staatiline avaliku IP-aadress. Laadi koormusetasakaalustusteenuse ees, mis võimaldab [Mitme-VIP](../load-balancer/load-balancer-multivip.md) stsenaariumid nagu mitme rentniku keskkond veebisaitidega SSL-i-põhine saate määrata ka mitu avaliku IP-aadressi.

### <a name="vpn-gateways"></a>VPN lüüsid
[Azure'i VPN-lüüsi](../vpn-gateway/vpn-gateway-about-vpngateways.md) saab ühendada muude Azure'i VNets või kohapealse võrgu Azure virtuaalse võrgu (VNet). Peate oma **IP-konfiguratsiooni** lubamiseks selle serveri võrgus suhelda avaliku IP-aadressi määramiseks. Praegu saate *dünaamilise* avaliku IP-aadressi määrata ainult VPN-lüüsi.

### <a name="application-gateways"></a>Rakenduse lüüsid
Avaliku IP-aadressi saate seostada Azure [Rakenduse lüüsi](../application-gateway/application-gateway-introduction.md), määrates selle lüüsi **frontend** konfigureerimine. Avaliku IP-aadressi toimib koormusetasakaalustusega VIP. Praegu saate *dünaamilise* avaliku IP-aadressi määrata ainult rakenduse lüüsi frontend konfigureerimine.

### <a name="at-a-glance"></a>Veebisaidil kiire
Järgmises tabelis antakse ülevaade teatud atribuut, mille kaudu saab seostada ülataseme ressursi ja võimalike jaotamise meetodite (dünaamilised või staatilised), mida saab kasutada avaliku IP-aadressi.

|Kõrgeima taseme ressurss|IP-aadressi seos|Dünaamiliste|Staatiline|
|---|---|---|---|
|Virtuaalse masina|Võrgu liidese|Jah|Jah|
|Laadi koormusetasakaalustusteenuse|Esiosa konfigureerimine|Jah|Jah|
|VPN-lüüsi|Lüüsi IP konfigureerimine|Jah|Ei|
|Rakenduse lüüsi|Esiosa konfigureerimine|Jah|Ei|

## <a name="private-ip-addresses"></a>Privaatne IP-aadressid
Privaatne IP-aadresside luba Azure ressursse suhelda muud ressursid, [virtuaalse võrgu](virtual-networks-overview.md) või mõne kohapealse võrgu kaudu VPN-lüüsi või ExpressRoute ringi, kasutamata Interneti-kättesaadav IP-aadress.

Azure'i ressursihaldur juurutamise mudeli, Privaatne IP-aadress on seostatud järgmist tüüpi Azure ressursse:

- VMs
- Sisemise koormus soolise (ILBs)
- Rakenduse lüüsid

### <a name="allocation-method"></a>Eraldatud meetod
Privaatne IP-aadress on eraldatud alamvõrku, millele on manustatud ressursi aadressi vahemik. Ise alamvõrgu aadress vahemik on osa on VNet aadress vahemikus.

On kaks võimalust, kus on eraldatud privaatne IP-aadress: *dünaamilised* või *staatilised*. Eraldatud vaikemeetod on *dünaamiline*, kus IP-aadress on automaatselt eraldatud ressursi alamvõrgu (DHCP abil). IP-aadressi saate muuta, kui lõpetada ja alustada ressurss.

Saate seada eraldatud meetodit *staatiline* IP-aadress ei muutu tagamiseks. Sel juhul peate ka pakuvad lubatud IP-aadress, mis on osa ressursi alamvõrgu.

Staatilise privaatne IP-aadressid on tavaliselt kasutatakse:

- VMs, mis toimivad domeenikontrollerite või DNS-serverid.
- Ressursse, mis nõuavad tulemüüri reeglite abil IP-aadressid.
- Ressursside korrad muude rakenduste/ressursside kaudu IP-aadress.

### <a name="virtual-machines"></a>Virtuaalmasinates
Privaatne IP-aadress on määratud **võrgu liidese** [Windowsi](../virtual-machines/virtual-machines-windows-about.md) või [Linuxi](../virtual-machines/virtual-machines-linux-about.md) VM. Mitme võrgu kasutajaliidese VM korral saab iga kasutajaliidese määratud privaatne IP-aadress. Saate määrata dünaamilise või staatilise võrgu kasutajaliidese eraldatud meetod.

#### <a name="internal-dns-hostname-resolution-for-vms"></a>Sisemise DNS-i hostinime lahenduse (jaoks VMs)
Kõik Azure VMs on konfigureeritud [Azure hallatav DNS-serverid](virtual-networks-name-resolution-for-vms-and-role-instances.md#azure-provided-name-resolution) vaikimisi, kui otseselt konfigureerida kohandatud DNS-serverid. Need DNS-i serverid ette sisemise nimelahendus VMs, mis asuvad samas VNet sees.

Kui loote VM, lisatakse jaoks hostname oma privaatse IP-aadressi vastenduse Azure'i hallatav DNS servers. Mitme võrgu kasutajaliidese VM korral hostname on vastendatud esmane võrgu liidese privaatne IP-aadress.

Azure'i hallatav DNS-serverid konfigureeritud VMs saab lahendamiseks ning kõik nende VNet VMs hostinimed oma isiklike IP-aadressid.

### <a name="internal-load-balancers-ilb--application-gateways"></a>Sisemise koormus soolise (ILB) ja rakendus lüüsid
Saate määrata [Azure'i sisemine laadi koormusetasakaalustusteenuse](../load-balancer/load-balancer-internal-overview.md) (ILB) või mõne [Azure'i rakenduse lüüsi](../application-gateway/application-gateway-introduction.md) **esiosa** konfiguratsiooni privaatne IP-aadress. Ressursid (VNet) oma virtuaalse võrgustikus juurdepääs ainult sisemise lõpp-toimib privaatne IP-aadress ja Kaug võrguga ühendatud on VNet. Saate määrata kas dünaamilised või staatilised privaatne IP-aadress esiosa konfigureerimine.

### <a name="at-a-glance"></a>Veebisaidil kiire
Järgmises tabelis antakse ülevaade teatud atribuudi kaudu, mis võib olla seotud ülataseme ressursi ja võimalike jaotamise meetodite (dünaamilised või staatilised), mida saab kasutada privaatne IP-aadress.

|Kõrgeima taseme ressurss|IP address seos|Dünaamiliste|Staatiline|
|---|---|---|---|
|Virtuaalse masina|Võrgu liidese|Jah|Jah|
|Laadi koormusetasakaalustusteenuse|Esiosa konfigureerimine|Jah|Jah|
|Rakenduse lüüsi|Esiosa konfigureerimine|Jah|Jah|

## <a name="limits"></a>Piirangud

Raadiuse IP tegelemine on toodud täiskomplekti [piirab Networking](azure-subscription-service-limits.md#networking-limits) Azure. Need piirangud on piirkond ja tellimuse kohta. Saate vaikimisi piirangud maksimaalne piires vastavalt teie ettevõtte vajadustele suurendamiseks [pöörduge klienditoe poole](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade) .

## <a name="pricing"></a>Hinnad

Avaliku IP-aadresside võib-olla nominal tasuta. IP address hinnad Azure kohta lisateabe saamiseks vaadake läbi [IP-aadressi hinnad](https://azure.microsoft.com/pricing/details/ip-addresses) leht.

## <a name="next-steps"></a>Järgmised sammud
- [Staatilise avaliku IP Azure'i portaalis VM juurutamine](virtual-network-deploy-static-pip-arm-portal.md)
- [Juurutamine VM staatilise avaliku IP malli abil](virtual-network-deploy-static-pip-arm-template.md)
- [Staatilise privaatne IP-aadress Azure'i portaalis VM juurutamine](virtual-networks-static-private-ip-arm-pportal.md)
