<properties
   pageTitle="Kohapealse võrgu ühenduse Azure'i | Microsoft Azure'i"
   description="Selgitatakse ja võrreldakse toodud erinevad meetodid, nt Azure, Office 365 ja Dynamics CRM Online'i Microsofti pilveteenustega."
   services=""
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags=""/>
<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/25/2016"
   ms.author="jdial"/>
   
# <a name="connecting-your-on-premises-network-to-azure"></a>Kohapealse võrgu ühenduse Azure'i

Microsoft osutab mitut tüüpi pilveteenustega. Samal ajal kõiki teenuseid avaliku Interneti kaudu ühendust luua, saate ka luua ühenduse mõne teenuse Interneti kaudu või otsene ja privaatne ühendus Microsoft üle virtuaalse privaatvõrgu (VPN) tunneliga abil. See artikkel aitab teil otsustada, millist ühenduvuse suvand kõige paremini vastavad teie vajadustele, saate kasutada Microsofti pilveteenustega tüüpi. Enamiku organisatsioonide kasutavad mitu ühendust tüüpi allpool kirjeldatud.

## <a name="connecting-over-the-public-internet"></a>Avaliku Interneti kaudu ühenduse loomine

Selle ühendusetüübi saab kasutada Microsofti pilveteenustega otse internetis, nagu allpool näidatud.

![Internet] (./media/guidance-connecting-your-on-premises-network-to-azure/internet.png "Internet")

See ühendus on tavaliselt kasutatakse Microsofti pilveteenustega esimene tüüp. Järgmises tabelis on loetletud spetsialistid ja miinuste seda tüüpi ühendus.



| **Eelised**| **Kaalutlused**|
|---------|---------|
|Nõuab teie kohapealse võrgu muutmine seni, kuni kõik kliendi seadmed on kõik IP-aadresside ja portide Internetis piiramatu juurdepääs.|Kui liikluse on sageli krüptitud HTTPS-i kaudu, seda saab kinni teel kuna see läbib avaliku Interneti.|
|Saate luua ühenduse avaliku Internetis kõikide Microsofti pilveteenustega.|Ettearvamatuid latentsus kuna ühenduse läbib Interneti-ühendus.|
|Kasutab teie Interneti-ühendus.||
|Mis tahes ühenduvuse seadmete haldamine ei vaja.||

See ühendus ei ole ühenduvuse või ribalaius kulud alates olemasoleva Interneti-ühendust kasutada. 

## <a name="connecting-with-a-point-to-site-connection"></a>Punkti saidi ühenduse ühendamine

Selle ühendusetüübi saab kasutada mõnda Microsofti pilveteenustega kaudu Secure turvasoklite Tunneling Protocol (SSTP) tunneliga Internetis, nagu allpool näidatud.

![P2S] (./media/guidance-connecting-your-on-premises-network-to-azure/p2s.png "SharePointi saidil ühenduse")

Ühendus on tehtud olemasoleva Interneti-ühendus, kuid nõutavad on Azure VPN-lüüsi. Järgmises tabelis on loetletud spetsialistid ja miinuste seda tüüpi ühendus.

| **Eelised**| **Kaalutlused**|
|---------|---------|
|Nõuab teie kohapealse võrgu muutmine seni, kuni kõik kliendi seadmed on kõik IP-aadresside ja portide Internetis piiramatu juurdepääs.|Kui liikluse on krüptitud, kasutades IPSec, seda saab kinni teel kuna see läbib avaliku Interneti.|
|Kasutab teie Interneti-ühendus.|Ettearvamatuid latentsus kuna ühenduse läbib Interneti-ühendus.|
|Kuni 200 Mb/s lüüsi läbilaskevõime.|Jaoks on vaja luua ja hallata eraldi seoseid oma kohapealse võrgus iga seadme ja iga iga seadme peab ühenduse lüüs.|
|Saab kasutada Azure teenused, Azure'i virtuaalne võrkude (VNet) ühendatavad ühenduse näiteks Azure'i Virtuaalmasinates ja Azure pilveteenustega.|Nõuab minimaalsete poolelioleva manustamine Azure'i VPN-lüüsi.|
||Ei saa luua ühendust Microsoft Office 365 või Dynamics CRM Online'i kasutada.
||Ei saa kasutada Azure teenused, mida ei saa ühendada mõne VNet ühenduse.|

Lisateave teenuse [VPN-lüüsi](../vpn-gateway/vpn-gateway-about-vpngateways.md) , selle [hinnad](https://azure.microsoft.com/pricing/details/vpn-gateway)ja väljaminev liiklus andmete [hinnad](https://azure.microsoft.com/pricing/details/data-transfers).

## <a name="connecting-with-a-site-to-site-connection"></a>Saitide ühenduse kaudu ühendamise

Selle ühendusetüübi saab kasutada mõnda Microsofti pilveteenustega kaudu on IPSec tunneliga Internetis, nagu allpool näidatud.

![S2S] (./media/guidance-connecting-your-on-premises-network-to-azure/s2s.png "Saitide ühendus")

Ühendus on tehtud olemasoleva Interneti-ühenduse, kuid nõutavad on Azure VPN-lüüsi seotud hinnad ja väljaminev liiklus andmete edastamiseks hinnad on. Järgmises tabelis on loetletud spetsialistid ja miinuste seda tüüpi ühendus.

| **Eelised**| **Kaalutlused**|
|---------|---------|
|Kõigis seadmetes kohapealse võrgus saaksid suhelda Azure'i teenuste ühendatud on VNet nii, et ei ole vaja konfigureerida üksikute ühendused iga seadmega.|Kui liikluse on krüptitud, kasutades IPSec, seda saab kinni teel kuna see läbib avaliku Interneti.|
|Kasutab teie Interneti-ühendus.|Ettearvamatuid latentsus kuna ühenduse läbib Interneti-ühendus.|
|Saab kasutada ühenduse Azure'i teenuseid, mis on VNet, nt Virtuaalmasinates saab ühendada pilveteenustega.|Peate konfigureerimine ja haldamine valideeritud VPN seadme * kohapealse.|
|Kuni 200 Mb/s lüüsi läbilaskevõime.|Nõuab minimaalsete poolelioleva manustamine Azure'i VPN-lüüsi.|
|Saate jõustada väljaminev liiklus cloud virtuaalmasinates ülevaatuse ja kasutaja määratletud marsruudib või äärise lüüsi protokolli (BGP) abil logimise kohapealse võrgu kaudu algatada **.|Ei saa luua ühendust Microsoft Office 365 või Dynamics CRM Online'i kasutada.|
||Ei saa kasutada Azure teenused, mida ei saa ühendada mõne VNet ühenduse.|
||Kui kasutate teenuseid, mis algatada ühendused tagasi kohapealse seadmed ja nõuavad oma turbepoliitikate, peate võib vahel kohapealse võrgu- ja Azure'i tulemüüri.|

- * [Kinnitatud VPN seadmete](../vpn-gateway/vpn-gateway-about-vpn-devices.md#validated-vpn-devices)loendi kuvamine.
- ** Lisateavet [kasutaja määratletud marsruudib](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md) või [BGP](../vpn-gateway/vpn-gateway-bgp-overview.md) jõustamine marsruutimine: Azure'i VNets seadmes kohapealse kasutamise kohta.

## <a name="connecting-with-a-dedicated-private-connection"></a>Ühendab sihtotstarbeline privaatne ühendus

Selle ühendusetüübi saab kasutada kõigi Microsofti pilveteenustega spetsiaalne privaatne kaudu Microsoft, mis läbida Internet, nagu allpool näidatud.

![ER] (./media/guidance-connecting-your-on-premises-network-to-azure/er.png "ExpressRoute ühendus")

Ühendus nõuab ExpressRoute teenuse ja ühenduvuse pakkuja ühenduse kasutamine. Järgmises tabelis on loetletud spetsialistid ja miinuste seda tüüpi ühendus.

| **Eelised**| **Kaalutlused**|
|---------|---------|
|Liikluse ei saa teel kinni avaliku Interneti kaudu, kuna kasutatakse asjakohast ühenduse teenuse pakkuja kaudu.|Nõuab kohapealse ruuteri haldus.|
|Kuni 10 Gb s ExpressRoute ringi ja läbilaskevõime kuni 2 Gb/s iga lüüsi läbilaskevõime.|Jaoks on vaja ühenduvust pakkuja asjakohast ühenduse.|
|Prognoositavad latentsus Kuna tegemist on sihtotstarbeline Microsoft, mis läbida Interneti-ühendus.|Võib olla vaja minimaalsete poolelioleva manustamine ühe või mitme Azure'i VPN lüüside (kui ringi ühenduse VNets).|
|Ei nõua krüptitud suhtlus, kuigi liikluse, saate soovi korral krüptida.| Kui kasutate pilveteenustega, mis algatada ühendused tagasi kohapealse seadmed, peate võib vahel kohapealse võrgu- ja Azure'i tulemüüri.|
|Kõik Microsofti pilveteenustega, kus mõned erandid * otse saab ühendada.|Nõuab võrgu aadress tõlge (NAT) kohapealse IP-aadresside sisestamise andmekeskuste Microsofti teenuste eest, mida ei saa ühendada VNet.* *|
|Saate jõustada väljaminev liiklus algatada cloud virtuaalmasinates ülevaatuse ja logimine BGP abil kohapealse võrgu kaudu.|

- * [Teenuste loend](../expressroute/expressroute-faqs.md#supported-services) , mida ei saa kasutada ExpressRoute kuvamine. Office 365 ühenduse loomiseks peab olema kinnitatud Azure tellimuse.  Lisateavet artiklist [Azure'i ExpressRoute Office 365 jaoks](https://support.office.com/article/Azure-ExpressRoute-for-Office-365-6d2534a2-c19c-4a99-be5e-33a0cee5d3bd?ui=en-US&rs=en-US&ad=US&fromAR=1) .
- ** Lisateavet ExpressRoute [NAT](../expressroute/expressroute-nat.md) nõuetele.

Lisateave [ExpressRoute](../expressroute/expressroute-introduction.md), selle seotud [hinnad](https://azure.microsoft.com/pricing/details/expressroute)ja [ühenduvuse pakkujad](../expressroute/expressroute-locations.md#connectivity-provider-locations).

## <a name="additional-considerations"></a>Täiendavad kaalutlused

- Mitut ülaltoodud suvandid on erinevad ülempiirid VNet ühendused, lüüsi ühendused ja muude kriteeriumide toetada. Soovitatav on Azure [võrgunduse piirab](../azure-subscription-service-limits.md#networking-limits) mõista, kui need mõjutavad ühenduvuse tüübid, mida kasutate vaatate. 
- Kui plaanite ühenduse lüüs-saidilt VPN-ühenduse sama VNet, nagu on ExpressRoute lüüsi, te peaksite end olulised piirangud esmalt. Üksikasjalikumat teavet leiate teemast [ExpressRoute konfigureerimine ja - saidilt kooseksisteerimisele ühendused](../expressroute/expressroute-howto-coexist-resource-manager.md#limits-and-limitations) .

## <a name="next-steps"></a>Järgmised sammud

Ressursside allpool selgitatakse, kuidas rakendada ühenduse tüübid artikkel.

-   [Rakendada punkti saidi ühenduse](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)
-   [Rakendada saidilt ühendus](guidance-hybrid-network-vpn.md)
-   [Rakendada asjakohast privaatne ühendus](guidance-hybrid-network-expressroute.md)
-   [Rakendada asjakohast privaatne ühenduse saidilt ühenduse kõrge-saadavus](guidance-hybrid-network-expressroute-vpn-failover.md)
