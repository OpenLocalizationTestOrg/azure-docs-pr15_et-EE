<properties 
   pageTitle="VPN-lüüsi kohta | Microsoft Azure'i"
   description="Lisateavet Azure virtuaalse võrgu jaoks VPN-lüüsi ühendused."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager,azure-service-management"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="cherylmc" />

# <a name="about-vpn-gateway"></a>VPN-lüüsi kohta


Virtuaalse võrgu lüüsi saab saata võrguliiklust Azure'i ja kohapealse asukohtade vahel ka virtuaalse võrgu Azure'is (VNet VNet) vahel. Kui konfigureerite VPN-lüüsi, peate looma ja virtuaalse võrgu lüüsi ja virtuaalse võrguühenduse lüüsi konfigureerimine.

Ressursihaldur juurutamise mudeli, kui loote virtuaalse võrgu lüüsi ressurssi, saate määrata mitu sätted. Üks nõutav sätted on "-GatewayType". On kaks virtuaalse võrgu lüüsi tüüpi: VPN-i ja ExpressRoute. 

Võrguliikluse saatmisel sihtotstarbeline privaatne ühenduse kasutate lüüsi tüüp "ExpressRoute". See on ka edaspidi mõne ExpressRoute lüüsi. Kui võrguliiklust saadetakse krüptitud üle avaliku ühendus, kasutage lüüsi tüüp "Vpn". See on edaspidi VPN-lüüsi. Saidi saidi, punkti – saidile ja VNet-VNet ühendused kõik kasutada VPN-lüüsi.

Iga virtuaalse võrgu võib olla ainult üks virtuaalse võrgu lüüsi lüüsi tüüp. Näiteks saate määrata ühe virtuaalse võrgu - GatewayType ExpressRoute kasutava lüüsi ja üks, mis kasutab - GatewayType Vpn. See artikkel keskendub VPN-lüüsi. ExpressRoute kohta leiate lisateavet teemast [ExpressRoute tehniline ülevaade](../expressroute/expressroute-introduction.md).

## <a name="pricing"></a>Hinnad

[AZURE.INCLUDE [vpn-gateway-about-pricing-include](../../includes/vpn-gateway-about-pricing-include.md)] 


## <a name="gateway-skus"></a>Lüüsi SKU-de jaoks

[AZURE.INCLUDE [vpn-gateway-gwsku-include](../../includes/vpn-gateway-gwsku-include.md)]

Lüüsi SKU-de jaoks kohta leiate lisateavet teemast [Lüüsi SKU-de jaoks](vpn-gateway-about-vpn-gateway-settings.md#gwsku).

### <a name="estimated-aggregate-throughput-by-sku"></a>Hinnanguline liitväärtuse läbilaskevõime poolt SKU-ga

Järgmises tabelis on lüüsi tüübid ja hinnanguline liitväärtuse läbilaskevõime. Selles tabelis kehtib ressursihaldur ja klassikaline juurutamise mudelid.

[AZURE.INCLUDE [vpn-gateway-table-gwtype-aggthroughput](../../includes/vpn-gateway-table-gwtype-aggtput-include.md)] 

## <a name="configuring-a-vpn-gateway"></a>VPN-lüüsi konfigureerimine

Kui konfigureerite VPN-lüüsi, kasutate juhiseid sõltuvad juurutamise mudeli virtuaalse võrgu loomiseks kasutatud. Näiteks kui olete loonud oma VNet näidise klassikaline juurutus, saate juhised ja juhised klassikaline juurutamise mudeli loomine ja konfigureerimine VPN-lüüsi sätted. Juurutamise mudelite kohta leiate lisateavet teemast [mõistmine ressursihaldur ja klassikaline juurutamise mudelid](../resource-manager-deployment-model.md).

VPN-ühenduse lüüs tugineb mitme ressursse, mis on konfigureeritud teatud sätted. Enamik ressursse saab konfigureerida eraldi, kuigi ta peab olema kindlas järjekorras mõnel juhul konfigureeritud. Saate alustada välja loomine ja konfigureerimine ühe konfigureerimine tööriista abil, nt Azure portaali ressursid. Saate siis otsustate hiljem mõnda muud tööriista, nt PowerShelli, konfigureerida täiendavaid materjale või muuta olemasolevaid ressursse vajaduse korral üle minna. Praegu ei saa konfigureerida iga ressursi ja ressursside säte Azure'i portaalis. Juhiseid iga ühenduse topoloogia artiklid saate määrata, millal on vaja teatud konfiguratsiooni tööriista. Üksikute ressursside ja VPN-lüüsi sätete kohta leiate teavet teemast [VPN lüüsi sätete](vpn-gateway-about-vpn-gateway-settings.md).

Järgmised jaotised sisaldavad tabelid, mis loendis:

- saadaval juurutamise mudeli
- saadaval Konfigureerimisriistad
- lingid, mis viivad teid otse artikkel, kui see on saadaval

Abil diagrammide ja kirjelduste valige ühenduse topoloogia, et need vastaksid teie vajadustele. Diagrammid kuvada peamise võrdlusalus topoloogiatest, kuid on võimalik luua keerukamaid konfiguratsioone abil diagrammid üldjoontes.

## <a name="site-to-site-and-multi-site"></a>Saitide ja mitme saidi

### <a name="site-to-site"></a>Saidilt

Lüüsi saidi saidi (S2S) VPN-ühendus on ühendus üle IPsec/IKE (IKEv1 või IKEv2) VPN tunneliga. Seda tüüpi ühenduse nõuab VPN seadme asub asutusesisene, mis on määratud avaliku IP-aadress ja ei asu taga mõnda NAT. S2S ühendused saab kasutada asukohaga ja hübriidi konfiguratsiooni.   

![S2S ühendus] (./media/vpn-gateway-about-vpngateways/demos2s.png "saidilt")


### <a name="multi-site"></a>Mitme saidi

Saate luua ja konfigureerida VPN-ühenduse lüüs oma VNet ja mitme kohapealse võrgu vahel. Kui töötate mitme ühendused, peate kasutama RouteBased VPN-tüüp (dünaamilise lüüsi klassikaline VNets). Kuna on VNet olla ainult üks VPN-lüüsi, kõik ühendused lüüsi kaudu ühiskasutusse anda läbilaskevõime. See on sageli nimetatakse "mitme sait" ühendust.
 

![Mitme saidi ühendus] (./media/vpn-gateway-about-vpngateways/demomulti.png "mitme saidi")

### <a name="deployment-models-and-methods-for-site-to-site-and-multi-site"></a>Juurutamise mudelite ja -saitide ja mitme saidi meetodid

[AZURE.INCLUDE [vpn-gateway-table-site-to-site](../../includes/vpn-gateway-table-site-to-site-include.md)] 

## <a name="vnet-to-vnet"></a>VNet-VNet

Võrguga virtuaalse võrgu teise virtuaalse (VNet VNet) on sarnane on VNet ühenduse oma kohapealse saidi asukoha. Mõlemat tüüpi ühenduvuse kasutavad VPN-lüüsi turvaline tunneliga, kasutades IPsec/IKE. Saate ühendada isegi VNet-VNet suhtlemine mitme saidi ühenduse konfiguratsioone. See võimaldab teil luua võrgu topoloogiatest, mis sisaldavad asutusesiseses Ühenduvus omavahel virtuaalse võrguühendus.

VNets, loote võib olla:

- samas või mõnes muus piirkondades
- tellimuste samas või mõnes muus rakenduses 
- sama erinevate juurutamise mudelid


![VNet VNet ühendus] (./media/vpn-gateway-about-vpngateways/demov2v.png "VNet-vnet")

#### <a name="connections-between-deployment-models"></a>Juurutamise mudelite vahelisi seoseid

Azure'i praegu on kaks juurutamise mudelit: klassikaline ja ressursihaldur. Kui te kasutate Azure juba mõnda aega, siis ilmselt on Azure VMs ja eksemplari rollide klassikaline VNet töötab. Teie uuem VMs ja rolli aknad võib töötama VNet, mis on loodud rakenduses ressursihaldur. Saate luua VNets, kui soovite lubada ressursside ühe VNet suhelda ressursid teise vahelist ühendust.

#### <a name="vnet-peering"></a>VNet silmitsemine

Teil on võimalik, et kasutada VNet silmitsemine luua ühendust, kui virtuaalse võrgu vastab teatud nõuetele. VNet silmitsemine virtuaalse võrgu lüüsi ei kasuta. Lisateavet leiate teemast [VNet silmitsemine](../virtual-network/virtual-network-peering-overview.md).


### <a name="deployment-models-and-methods-for-vnet-to-vnet"></a>Juurutamise mudelite ja VNet-VNet meetodid

[AZURE.INCLUDE [vpn-gateway-table-vnet-to-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)] 


## <a name="point-to-site"></a>Punkti – saidile

Lüüsi SharePointi saidil (P2S) VPN-ühendus võimaldab teil luua turvalist ühendust virtuaalse võrgu üksikute klientarvutist. P2S on VPN-ühendus SSTP (Secure turvasoklite Tunneling Protocol). P2S ühendused, pole vaja VPN seadme või avaliku IP-aadressi töötamiseks. Saate luua, alates klientarvuti VPN-ühendus. See lahendus on kasulik, kui soovite ühendada oma VNet mujalt, nagu kodus või konverentsi, või kui teil on vaid mõned kliendid, kes on vaja luua ühendus mõne VNet. P2S ühendused saab kasutada koos S2S ühenduste kaudu sama VPN-lüüsi kõik nii ühenduste konfiguratsiooni nõuded on kooskõlas.


![SharePointi saidil ühenduse] (./media/vpn-gateway-about-vpngateways/demop2s.png "punkti – saidile")

### <a name="deployment-models-and-methods-for-point-to-site"></a>Juurutamise mudelite ja punkti saidi meetodid

[AZURE.INCLUDE [vpn-gateway-table-point-to-site](../../includes/vpn-gateway-table-point-to-site-include.md)] 


## <a name="expressroute"></a>ExpressRoute

[AZURE.INCLUDE [expressroute-intro](../../includes/expressroute-intro-include.md)]

ExpressRoute kontekstis virtuaalse võrgu lüüsi on konfigureeritud lüüsi tüüp "ExpressRoute", mitte "Vpn". ExpressRoute kohta leiate lisateavet teemast [ExpressRoute tehniline ülevaade](../expressroute/expressroute-introduction.md).


## <a name="site-to-site-and-expressroute-coexisting-connections"></a>Saitide ja ExpressRoute kaasuvad ühendused

ExpressRoute on otsese, pühendunud ühendus Microsoft Services, sh Azure kaudu oma WAN (mitte Interneti avalike). Saidi saidi VPN liikluse liigub krüptitud avaliku Interneti kaudu. Ei saaks seda konfigureerida virtuaalse samasse võrku VPN saitide ja ExpressRoute ühendused on mitmeid eeliseid.

Turvaline Tõrkesiirde tee VPN saitide konfigureerimine ExpressRoute või saidid, mis ei kuulu teie võrgus, kuid mis on ühendatud ExpressRoute kaudu ühenduse VPN saitide abil. Teate, et see nõuab kahte virtuaalse võrgu lüüside virtuaalse samasse võrku, üks - GatewayType Vpn ja teised kasutavad - GatewayType ExpressRoute abil.


![Coexist ühendus] (./media/vpn-gateway-about-vpngateways/demoer.png "expressroute-site2site")


### <a name="deployment-models-and-methods-for-s2s-and-expressroute"></a>Juurutamise mudelite ja S2S ja ExpressRoute meetodid

[AZURE.INCLUDE [vpn-gateway-table-coexist](../../includes/vpn-gateway-table-coexist-include.md)] 


## <a name="next-steps"></a>Järgmised sammud

Plaanige oma VPN-lüüsi konfigureerimine. Vt [VPN lüüsi plaanimine ja kujundus](vpn-gateway-plan-design.md) ja [Azure kohapealse võrgu ühenduse](../guidance/guidance-connecting-your-on-premises-network-to-azure.md).








 
