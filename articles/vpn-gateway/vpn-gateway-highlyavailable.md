<properties
   pageTitle="Azure'i VPN lüüside tugevalt saadaval konfiguratsioone ülevaade | Microsoft Azure'i"
   description="Selles artiklis antakse ülevaade tugevalt saadaval konfiguratsiooni suvandid Azure'i VPN lüüside abil."
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
   ms.date="09/24/2016"
   ms.author="yushwang"/>

# <a name="highly-available-cross-premises-and-vnet-to-vnet-connectivity"></a>Väga kättesaadav asutusesiseses ja VNet-VNet Ühenduvus

Selles artiklis antakse ülevaade tugevalt saadaval konfiguratsiooni võimaluste kohta oma asutusesiseses ja VNet-VNet Ühenduvus Azure VPN lüüside abil.

## <a name = "activestandby"></a>Azure'i VPN lüüsi koondamise kohta

Iga Azure'i VPN-lüüsi koosneb kahel on aktiivne-puhkerežiimis olev konfiguratsiooni. Mis tahes kavandatud hooldustööde või planeerimata häirete, mis juhtub aktiivne eksemplari, valige eksemplari soovite automaatselt (Tõrkesiirde) üle võtta ja jätkake S2S VPN- või VNet-VNet ühendused. Lühike katkemise põhjustab parameetrit üle. Kavandatud hooldustööde, tuleks ühendust taastada jooksul 10 – 15 minutit. Planeerimata probleemide ühenduse taastamine tuleb pikem, umbes 1 minut on halvima 1 ja pool minutit. P2S VPN kliendi ühenduste lüüsi, katkestatakse P2S ühendused ja kasutajate peate selle klientarvutite kaudu uuesti.

![Aktiivne-puhkerežiimis olev](./media/vpn-gateway-highlyavailable/active-standby.png)

## <a name="highly-available-cross-premises-connectivity"></a>Väga kättesaadav asutusesiseses Ühenduvus

Parem kättesaadavus ette rist tööruumide ühendusi, on paar võimalust saadaval.

- Mitme kohapealse VPN seadmed
- Aktiivne-active Azure'i VPN-lüüsi
- Kombinatsioon

### <a name = "activeactiveonprem"></a>Mitme kohapealse VPN seadmed

Saate mitu VPN seadmete kohapealse võrgu kaudu ühenduse loomiseks oma Azure'i VPN-lüüsi, nagu on näidatud järgmisel joonisel:

![Mitme kohapealse VPN](./media/vpn-gateway-highlyavailable/multiple-onprem-vpns.png)

Selle konfiguratsiooni pakub mitme aktiivse tunnelid sama Azure'i VPN lüüsist seadmetes kohapealse samasse asukohta. On mõned nõuded ja piirangud.

1. Peate looma oma seadmest VPN mitme S2S VPN-ühendused Azure'i. Kui loote ühenduse VPN mitmes seadmes sama kohapealse võrgu Azure, peate looma ühe kohtvõrgu lüüsi iga VPN-seade ja üks ühendus oma Azure'i VPN lüüsist kohtvõrgu lüüsi.

2. Vastab teie VPN seadmete kohtvõrgu lüüside peab olema kordumatu avaliku IP-aadresside atribuudi "GatewayIpAddress".

3. Selle konfiguratsiooni puhul nõutakse BGP. Iga kohtvõrgu lüüsi tähistav VPN seade peab olema kordumatu BGP omavahelistes IP-aadress määratud atribuudi "BgpPeerIpAddress".

4. AddressPrefix atribuudi välja iga kohtvõrgu lüüsi peab ei kattuks. "BgpPeerIpAddress" tuleks määrata /32 AddressPrefix väli, nt 10.200.200.254/32 CIDR vorming.

5. Kasutage sama eesliiteid sama kohapealse võrgu eesliidete Azure'i VPN lüüsi ja liiklus edastatakse nende tunnelite kaudu korraga BGP.

6. Iga ühendus arvestatakse tunnelites Azure'i VPN lüüsi, 10 Basic ja Standard SKU-de jaoks, maksimaalne arv vastu ja HighPerformance SKU 30. 

Selle konfiguratsiooni puhul Azure'i VPN-lüüsi on endiselt aktiivne-puhkerežiimis olev režiimis nii, nagu on kirjeldatud [ülal](#activestandby)ikkagi juhtuda Tõrkesiirde käitumine ja lühike katkestamine. Kuid see seadistus valvurid tõrked või segadusi kohapealse võrgu ja VPN seadmetes.
 
### <a name="active-active-azure-vpn-gateway"></a>Aktiivne-active Azure'i VPN-lüüsi

Nüüd saate luua ka Azure VPN-lüüsi on aktiivne aktiivne konfiguratsiooni, kus mõlemal juhul lüüsi vms loob S2S VPN tunnelite oma kohapealse VPN seadmesse, nagu on näidatud järgmisel joonisel:

![Aktiivne aktiivne](./media/vpn-gateway-highlyavailable/active-active.png)

Selle konfiguratsiooni puhul iga Azure'i lüüsi eksemplari on kordumatu avaliku IP-aadress ja iga loob mõne IPsec/IKE S2S VPN tunneliga kohapealse VPN seadmesse teie kohalikus võrgus lüüsi ja ühenduse määratud. Pange tähele, et mõlemad VPN tunnelite tegelikult osa sellest sama ühendust. Peate endiselt konfigureerimine seadme kohapealse VPN vastu võtta või luua kaks S2S VPN tunnelite nende kahe Azure'i VPN lüüsi avaliku IP-aadressid.

Azure'i lüüsi eksemplari on aktiivne-active konfiguratsiooni, kohapealse võrgu kaudu Azure virtuaalse võrgu liiklust suunatakse läbi mõlema tunneli korraga, isegi juhul, kui seadme kohapealse VPN võib kasuks ühe tunneliga teiste üle. Märkus. kuigi sama TCP või UDP voog kuvatakse alati läbida samas tunneliga või tee, kui hooldustööde sündmuse toimub ühes linnanimede.

Kui kavandatud hooldustööde või planeerimata sündmuse juhtub üks lüüsi eksemplari, katkestatakse IPsec tunneliga selle eksemplari oma kohapealse VPN seadmesse. Vastavate marsruudib VPN-seadmetes tuleb eemaldada või tagasi võtta automaatselt nii, et liiklus minna muude aktiivse IPsec tunneliga. Azure'i pool parameetrit üle ilmneb automaatselt probleemse eksemplarist active eksemplari.

### <a name="dual-redundancy-active-active-vpn-gateways-for-both-azure-and-on-premises-networks"></a>Kahe-koondamise: aktiivne-active VPN lüüside nii Azure ja kohapealse võrgu jaoks

Kõige usaldusväärne suvand on ühendada aktiivne aktiivne lüüside oma võrgu ja Azure, nagu on näidatud alloleval joonisel.

![Kahekordne koondamine](./media/vpn-gateway-highlyavailable/dual-redundancy.png)

Siin loomine ja Azure VPN-lüüsi on aktiivne aktiivne konfiguratsiooni häälestamine ja kahe kohtvõrgu lüüside loomine ja kahe ühendused oma kahe kohapealsete VPN seadmete ülalpool kirjeldatud. Tulem on kogu Ühenduvus 4 IPsec tunnelite Azure virtuaalse võrgu ja kohapealse võrgu vahel.

Kõik lüüside ja tunnelid on aktiivne Azure servast, nii et liiklus on levinud vahel kõigi 4 tunnelite korraga, kuigi iga TCP või UDP kulgemist jälgib uuesti sama tunneliga või tee Azure servast. Ehkki jagades liiklus, võite näha veidi parem läbilaskevõime üle IPsec tunnelid, selle konfiguratsiooni peamine eesmärk on kõrge kättesaadavus. Ja laadist statistika levitamine, on raske anda mõõtühikute erinevate rakenduse liikluse tingimuste mõjutab liitväärtuse läbilaskevõime.

See topoloogia nõuab kahe kohtvõrgu lüüside ja kahe ühenduste toetamiseks paari kohapealse VPN seadmete ja BGP on nõutavad kaks ühendused kohapealse samasse võrku. Need nõuded on sama, mis selle [kohal](#activeactiveonprem). 

## <a name="highly-available-vnet-to-vnet-connectivity-through-azure-vpn-gateways"></a>Väga kättesaadav VNet-VNet ühenduvuse kaudu Azure'i VPN lüüsid

Sama aktiivne aktiivne konfiguratsiooni saate rakendada ka Azure VNet VNet ühendused. Saate luua aktiivne aktiivne VPN lüüside nii virtuaalse võrkude ja ühendada need koos vormi sama kogu Ühenduvus 4 tunnelite vahel kaks VNets, nagu on näidatud alloleval joonisel:

![VNet-VNet](./media/vpn-gateway-highlyavailable/vnet-to-vnet.png)

See tagab on alati paari tunnelid vahel kahe virtuaalse võrgu jaoks kavandatud hooldustööde sündmused, mis isegi parem kättesaadavus. Ehkki jaoks sama topoloogia asutusesiseses ühenduvuse nõuab kahte ühendused, eespool näidatud VNet-VNet topoloogia on vaja ainult üks ühendus iga lüüsi. Lisaks BGP on valikuline, juhul kui teel marsruutimine VNet-VNet kaudu on nõutav.


## <a name="next-steps"></a>Järgmised sammud

Lugege teemat [Konfigureerimise Active Active VPN lüüside asukohaga ja VNet-VNet ühendused](vpn-gateway-activeactive-rm-powershell.md) juhiseid konfigureerimiseks aktiivne-active asukohaga ja VNet-VNet ühendused.
