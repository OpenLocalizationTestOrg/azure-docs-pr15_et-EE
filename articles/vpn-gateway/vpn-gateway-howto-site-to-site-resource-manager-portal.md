<properties
   pageTitle="Luua virtuaalse võrgu-saidilt VPN-ühenduse Azure'i ressursihaldur ja Azure portaali abil | Microsoft Azure'i"
   description="Ressursihaldur juurutamise näidise VNet loomise ja ühendage see oma kohalikku kohapealse võrgu lüüsi S2S VPN-ühenduse kasutamine."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/14/2016"
   ms.author="cherylmc"/>

# <a name="create-a-vnet-with-a-site-to-site-connection-using-the-azure-portal"></a>On VNet Azure'i portaalis saidilt ühenduse loomine

> [AZURE.SELECTOR]
- [Ressursihaldur - Azure'i portaal](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
- [Ressursihaldur - PowerShelli](vpn-gateway-create-site-to-site-rm-powershell.md)
- [Klassikaline – klassikaline portaal](vpn-gateway-site-to-site-create.md)


Selles artiklis tutvustatakse virtuaalse võrgu ja Azure ressursihaldur juurutamise mudeli ja Azure portaali abil kohapealse võrgu-saidilt VPN-lüüsi ühenduse loomiseks. Saitide ühendused saab kasutada asukohaga ja hübriid konfiguratsioone.

![Skeem](./media/vpn-gateway-howto-site-to-site-resource-manager-portal/s2srmportal.png)


### <a name="deployment-models-and-methods-for-site-to-site-connections"></a>Juurutamise mudelite ja -saitide ühendused meetodid

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 

Järgmises tabelis on praegu saadaval juurutamise mudelid ja meetodid-saidilt konfiguratsioone. Kui konfigureerimistoimingute kohanimi on saadaval, me lingi otse selle tabeli.

[AZURE.INCLUDE [site-to-site table](../../includes/vpn-gateway-table-site-to-site-include.md)]

#### <a name="additional-configurations"></a>Täiendavad konfiguratsioone 

Kui soovite ühendada VNets, kuid ei ole luua ühenduse kohapealse asukohta, vaadake [VNet-VNet ühenduse konfigureerimine](vpn-gateway-vnet-vnet-rm-ps.md). Kui soovite lisada saidilt ühenduse VNet, mis on juba ühenduse, lugege teemat [lisamine on VNet koos olemasoleva ühenduse lüüs VPN-ühendus S2S](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md).

## <a name="before-you-begin"></a>Enne alustamist

Veenduge, et on enne konfiguratsioonile järgmist:

- Ühilduvad VPN-seade ja keegi, kes saab seda konfigureerida. Lugege [VPN seadmed](vpn-gateway-about-vpn-devices.md). Kui te pole tuttav konfigureerimise seadme VPN, või ei tunne vahemike asub IP-aadress oma kohapealse võrgu konfiguratsiooni, on vaja kellegagi, kes saab anda need andmed teie eest.

- Väliselt suunatud avaliku IP-aadressi seadme VPN. IP-aadress ei leita taga mõnda NAT.
    
- Azure'i tellimuse. Kui teil pole veel Azure tellimuse, saate aktiveerida oma [MSDN-i abonendi kasu](http://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) või Logi üles [tasuta konto](http://azure.microsoft.com/pricing/free-trial/).

### <a name="values"></a>Selle konfiguratsiooni näidisväärtused


Kasutab neid juhiseid kasutamisel saate kasutada Näidisväärtuste konfigureerimine.

- **VNet nimi:** TestVNet1
- **Aadress:** 10.11.0.0/16 ja 10.12.0.0/16
- **Alamvõrku:**
    - FrontEnd: 10.11.0.0/24
    - Taustväärtus: 10.12.0.0/24
    - GatewaySubnet: 10.12.255.0/27
- **Ressursirühm:** TestRG1
- **Asukoht:** Ida-USA
- **DNS-i Server:** 8.8.8.8
- **Lüüsi nimi:** VNet1GW
- **Avaliku IP:** VNet1GWIP
- **VPN-tüüp:** Protsessi-põhine.
- **Ühenduse tüüp:** Saidi saidi (IPsec)
- **Lüüsi tüüp:** VPN
- **Kohtvõrgu lüüsi nimi:** Site2
- **Ühenduse nimi:** VNet1toSite2


## <a name="CreatVNet"></a>1. virtuaalse võrgu loomine 

Kui teil on juba VNet, kontrollige, kas sätted on ühildu teie VPN-lüüsi kujundus. Mis tahes alamvõrku, mis võivad kattuvad muude võrkude väliskontaktidega erilist tähelepanu. Kui teil on kattuvate alamvõrku, ühendust ei tööta õigesti. Kui teie VNet õiged sätted on konfigureeritud, saate alustada [DNS-i serveri määramine](#dns) jaotises toodud juhised.

### <a name="to-create-a-virtual-network"></a>Virtuaalse võrgu loomine

[AZURE.INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-basic-vnet-rm-portal-include.md)]  

## <a name="subnets"></a>2 täiendavad aadressiruumi ja alamvõrku lisamine

Saate lisada täiendavad aadressiruumi ja alamvõrku oma VNet pärast selle loomist.

[AZURE.INCLUDE [vpn-gateway-additional-address-space](../../includes/vpn-gateway-additional-address-space-include.md)] 

## <a name="dns"></a>3. määrata DNS-i server

### <a name="to-specify-a-dns-server"></a>DNS-i serveri määramiseks

[AZURE.INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]

## <a name="gatewaysubnet"></a>4. lüüsi alamvõrgu loomine

Enne virtuaalse võrgu ühenduse lüüs, peate esmalt lüüsi alamvõrgu virtuaalse võrgu, millele, millega soovite ühenduse luua. Võimaluse korral on parem luua lüüsi alamvõrku, kasutate CIDR plokk /28 või /27 tulevaste konfigureerimise nõuded mahutamiseks piisavalt IP-aadressid.

Kui loote selle konfiguratsiooni teostamiseks, vaadake need [väärtused](#values) teie alamvõrku lüüsi loomisel.

### <a name="to-create-a-gateway-subnet"></a>Lüüsi alamvõrgu loomiseks


[AZURE.INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

## <a name="VNetGateway"></a>5. virtuaalse võrgu lüüsi loomine

Kui loote selle konfiguratsiooni teostamiseks, võib viidata [Näidisväärtuste konfigureerimine](#values).

### <a name="to-create-a-virtual-network-gateway"></a>Lüüsi virtuaalse võrgu loomine

[AZURE.INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]

## <a name="LocalNetworkGateway"></a>6. kohtvõrgu lüüsi loomine

Selle kohalikus võrgus lüüsi viitab oma kohapealse asukoht. Pange kohtvõrgu lüüsi nimi, mille Azure saate viidata. 

Kui loote selle konfiguratsiooni teostamiseks, võib viidata [Näidisväärtuste konfigureerimine](#values).

### <a name="to-create-a-local-network-gateway"></a>Kohaliku võrgu lüüsi loomiseks

[AZURE.INCLUDE [vpn-gateway-add-lng-rm-portal](../../includes/vpn-gateway-add-lng-rm-portal-include.md)]

## <a name="VPNDevice"></a>7. seadme VPN-i konfigureerimine

[AZURE.INCLUDE [vpn-gateway-configure-vpn-device-rm](../../includes/vpn-gateway-configure-vpn-device-rm-include.md)]

## <a name="CreateConnection"></a>8.-saidilt VPN-ühenduse loomine

Virtuaalse võrgu lüüsi ja seadme VPN-saidilt VPN-ühendus luua. Ärge unustage asendage väärtused ise. Ühiskasutusega võti peab vastama teie VPN seadme konfiguratsiooni puhul kasutatud väärtus. 

Enne selle jaotise, veenduge, et teie virtuaalse võrgu lüüsi ja kohtvõrgu lüüside loomise lõpetanud. Kui loote selle konfiguratsiooni kasutab, vaadake need [väärtused](#values) ühenduse loomisel.

### <a name="to-create-the-vpn-connection"></a>VPN-ühenduse loomiseks


[AZURE.INCLUDE [vpn-gateway-add-site-to-site-connection-rm-portal](../../includes/vpn-gateway-add-site-to-site-connection-rm-portal-include.md)]

## <a name="VerifyConnection"></a>9. kinnitamine VPN-ühendus

Saate kontrollida oma VPN-ühendus portaalis või PowerShelli abil.

[AZURE.INCLUDE [vpn-gateway-verify-connection-rm](../../includes/vpn-gateway-verify-connection-rm-include.md)]

## <a name="next-steps"></a>Järgmised sammud

- Kui teie ühendus on loodud, saate lisada virtuaalse võrguga virtuaalmasinates. Vaadake selle virtuaalmasinates [õppeteema](https://azure.microsoft.com/documentation/learning-paths/virtual-machines) lisateavet.

- BGP kohta leiate teavet teemast [BGP ülevaade](vpn-gateway-bgp-overview.md) ja [Kuidas konfigureerida BGP](vpn-gateway-bgp-resource-manager-ps.md).
