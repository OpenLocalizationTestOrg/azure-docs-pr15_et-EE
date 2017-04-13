<properties
   pageTitle="Ülevaade koos Azure VPN lüüside BGP | Microsoft Azure'i"
   description="Selles artiklis antakse ülevaade BGP Azure'i VPN lüüside abil."
   services="vpn-gateway"
   documentationCenter="na"
   authors="yushwang"
   manager="rossort"
   editor=""
   tags=""/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="06/16/2016"
   ms.author="yushwang"/>

# <a name="overview-of-bgp-with-azure-vpn-gateways"></a>BGP koos Azure VPN lüüside ülevaade

Selles artiklis antakse ülevaade Azure'i VPN lüüside toe BGP (äärise lüüsi Protocol).

## <a name="about-bgp"></a>BGP kohta

BGP on standardne marsruutimise protokoll levinud Interneti kahe või enama võrgu marsruutimine ja reachability teavet vahetada. Azure'i virtuaalne võrkude kontekstis kasutamisel BGP võimaldab lüüside Azure'i VPN ja teie asutusesisese VPN seadmed, nimetatakse BGP kolleegidega või naabrid, Exchange "marsruudib", mis teavitab nii lüüside kättesaadavuse ja nende eesliidete läbida lüüsid või ruuterid jaoks reachability kaasatud. BGP saate lubada teel marsruutimine vahel mitme võrgu lisaajast marsruudib BGP lüüsi õpib ühe BGP partnerilt kõik BGP kolleegidega.
 
### <a name="why-use-bgp"></a>Miks kasutada BGP?

BGP on valikuline funktsioon Azure marsruutimiseks vastavalt VPN lüüside kasutamisel. Peaksite ka veenduma, oma kohapealse VPN seadmed toetavad BGP enne funktsiooni. Saate kasutada Azure VPN lüüside ja teie asutusesisese VPN seadmete ilma BGP jätkata. See on samaväärne staatilise marsruudib (ilma BGP) *vs* abil dünaamiline marsruutimine BGP vahel oma võrgu ja Azure abil.

On mitmeid eelised ja BGP uusi võimalusi:

#### <a name="support-automatic-and-flexible-prefix-updates"></a>Automaatse ja paindlikus eesliite värskendused tugi

BGP, peate ainult deklareerida minimaalne eesliite teatud BGP partnerile üle IPsec S2S VPN tunneliga. See võib olla võimalikult väike hosti eesliite (/ 32) BGP omavahelistes IP-aadressi seadme kohapealse VPN. Saate määrata, mis kohapealse võrgu eesliiteid soovite reklaamida Azure'i Azure virtuaalse võrgu juurdepääsu lubamiseks.
    
Saate ka reklaamida suuremat eesliidete tähised, mis võivad sisaldada osa VNet aadress eesliiteid, nt suured privaatne IP address ruumi (nt 10.0.0.0/8). Pange tähele, kuigi eesliiteid ei saa oma VNet eesliiteid mõni identne. Nende marsruudib identne oma VNet eesliidete tähised on tagasi lükatud.

>[AZURE.IMPORTANT] Praegu reklaami Azure'i VPN lüüside vaikimisi tee (0.0.0.0/0) on blokeeritud. Kui see funktsioon on lubatud, antakse veelgi uuendused.

#### <a name="support-multiple-tunnels-between-a-vnet-and-an-on-premises-site-with-automatic-failover-based-on-bgp"></a>Toetab mitut tunnelid on VNet ja anda kohapealse saidi BGP põhjal automaatselt Tõrkesiirde

Saate luua mitu ühendust oma Azure'i VNet ja teie asutusesisese VPN seadmete paigal vahel. See funktsioon pakub mitme tunnelid (teed) on aktiivne aktiivne konfiguratsiooni kahe võrgu vahel. Kui üks tunnelid on välja lülitatud, vastavate marsruudib ei saa enam BGP kaudu ja liiklus nihkub automaatselt ülejäänud tunnelid.
    
Lihtne näide selle väga kättesaadav setup järgmisel joonisel:
    
![Mitme aktiivse teed](./media/vpn-gateway-bgp-overview/multiple-active-tunnels.png)

#### <a name="support-transit-routing-between-your-on-premises-networks-and-multiple-azure-vnets"></a>Tugiteenuste teel marsruutimine oma kohapealse võrgu ja mitme Azure'i VNets vahel

BGP võimaldab lüüside ja levitamine eesliiteid erinevate võrkudes, kas need on otseselt või kaudselt ühendatud. See saate lubada Azure VPN lüüside vahel oma kohapealse saitide ühest või mitmest mitme Azure virtuaalse võrgu marsruutimine teel.
    
Järgmisel joonisel on kujutatud mitme sõnumihüppe kohta, pärast topoloogia koos mitme tee, mida saate transiitliikluse kahe kohapealse võrgu kaudu Azure'i VPN lüüside jooksul Microsoft Networks vahel:

![Mitme sõnumihüppe kohta, pärast teel](./media/vpn-gateway-bgp-overview/full-mesh-transit.png)

## <a name="bgp-faqs"></a>BGP KKK


[AZURE.INCLUDE [vpn-gateway-bgp-faq-include](../../includes/vpn-gateway-bpg-faq-include.md)] 




## <a name="next-steps"></a>Järgmised sammud

Teemast [Alustamine BGP klõpsake Azure VPN lüüside](./vpn-gateway-bgp-resource-manager-ps.md) juhised konfigureerimiseks BGP asukohaga ja VNet-VNet ühendused.

