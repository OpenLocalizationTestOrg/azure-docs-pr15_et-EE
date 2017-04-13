<properties
   pageTitle="Luua virtuaalse võrgu-saidilt VPN-ühenduse Azure'i ressursihaldur ja PowerShelli abil | Microsoft Azure'i"
   description="Selles artiklis tutvustatakse VNet, kasutades ressursihaldur juurutamise mudeli ja ühenduse lüüs S2S VPN-ühenduse kasutamine kohaliku kohapealse võrgu loomine."
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

# <a name="create-a-vnet-with-a-site-to-site-connection-using-powershell"></a>On VNet PowerShelli kaudu saidilt ühenduse loomine

> [AZURE.SELECTOR]
- [Ressursihaldur - Azure'i portaal](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
- [Ressursihaldur - PowerShelli](vpn-gateway-create-site-to-site-rm-powershell.md)
- [Klassikaline – klassikaline portaal](vpn-gateway-site-to-site-create.md)

Selles artiklis tutvustatakse virtuaalse võrgu ja Azure ressursihaldur juurutamise näidise kohapealse võrgu-saidilt VPN-lüüsi ühenduse loomiseks. Saitide ühendused saab kasutada asukohaga ja hübriid konfiguratsioone.

![Saitide skeem] (./media/vpn-gateway-create-site-to-site-rm-powershell/s2srmps.png "saidilt") 


### <a name="deployment-models-and-methods-for-site-to-site-connections"></a>Juurutamise mudelite ja -saidilt ühendused meetodid

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 

Järgmises tabelis on praegu saadaval juurutamise mudelid ja meetodid-saidilt konfiguratsioone. Kui konfigureerimistoimingute kohanimi on saadaval, me lingi otse sellest tabelist. 

[AZURE.INCLUDE [site-to-site table](../../includes/vpn-gateway-table-site-to-site-include.md)]

#### <a name="additional-configurations"></a>Täiendavad konfiguratsioone

Kui soovite ühendada VNets, kuid ei ole luua ühenduse kohapealse asukohta, vaadake [VNet-VNet ühenduse konfigureerimine](vpn-gateway-vnet-vnet-rm-ps.md). Kui soovite lisada saidilt ühenduse VNet, mis on juba ühenduse, lugege teemat [lisamine on VNet koos olemasoleva ühenduse lüüs VPN-ühendus S2S](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md).

## <a name="before-you-begin"></a>Enne alustamist

Veenduge, et enne konfigureerimine on järgmist.

- Ühilduvad VPN-seade ja keegi, kes saab seda konfigureerida. Lugege [VPN seadmed](vpn-gateway-about-vpn-devices.md). Kui te pole tuttav konfigureerimise seadme VPN, või ei tunne vahemike asub IP-aadress oma kohapealse võrgu konfiguratsiooni, on vaja kellegagi, kes saab anda need andmed teie eest.

- Väliselt suunatud avaliku IP-aadressi seadme VPN. IP-aadress ei leita taga mõnda NAT.
    
- Azure'i tellimuse. Kui teil pole veel Azure tellimuse, saate aktiveerida oma [MSDN-i abonendi kasu](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) või Logi üles [tasuta konto](https://azure.microsoft.com/pricing/free-trial/).
    
- Azure'i ressursihaldur PowerShelli cmdlet-käskude uusim versioon. Vaadake, [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md) installimise PowerShelli cmdlet-käskude kohta lisateavet.


## <a name="Login"></a>1. ühenduse loomiseks oma tellimuse 

Veenduge, et te lülitumine PowerShelli kasutamine ressursihaldur cmdlet-käsud. Lisateavet leiate teemast [Windows PowerShelli kasutamine koos ressursihaldur](../powershell-azure-resource-manager.md).

Avage oma PowerShelli konsooli ja teie kontoga ühendust luua. Järgmises näites abil saate ühendada:

    Login-AzureRmAccount

Kontrollige konto tellimused.

    Get-AzureRmSubscription 

Määrake tellimus, mida soovite kasutada.

    Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"

## <a name="VNet"></a>2 virtuaalse võrgu ja lüüsi alamvõrgu loomine

Näiteid kasutada lüüsi alamvõrku, /28. On võimalik luua lüüsi alamvõrgu /29 võimalikult väike, soovitame suuremat alamvõrku, mis sisaldab rohkem aadresse, valides vähemalt /28 või /27 luua. See võimaldab piisavalt aadressid mahutamiseks soovite edaspidi võimalike täiendavate kuvatakse.

Kui teil on juba virtuaalse võrgu lüüsi alamvõrku, mis on/29 või suurem, saate liikuda edasi [teie kohalikus võrgus lüüsi lisada](#localnet).


[AZURE.INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)]  

### <a name="to-create-a-virtual-network-and-a-gateway-subnet"></a>Luua virtuaalse võrgu ja lüüsi alamvõrgu

Järgmises näites abil saate luua virtuaalse võrgu- ja lüüsi alamvõrgu. Asendada selle väärtused ise. 

Esmalt looge ressursirühma.
    
    New-AzureRmResourceGroup -Name testrg -Location 'West US'

Järgmiseks luua virtuaalse võrgu. Veenduge, et teie määratud aadressi tühikuid ei kattu ühte aadressi tühikuid, et teil on kohapealse võrgu.

Järgmises näites luuakse nimega *testvnet* ja kahe alamvõrku, üks nimega *GatewaySubnet* virtuaalse võrk ja teine nimetatakse *Subnet1*. See on oluline ühe alamvõrgu nimega spetsiaalselt *GatewaySubnet*loomiseks. Kui te nimi on midagi muud, ei õnnestu oma ühenduse konfiguratsiooni. 

Määrake muutujaid.

    $subnet1 = New-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.0.0/28
    $subnet2 = New-AzureRmVirtualNetworkSubnetConfig -Name 'Subnet1' -AddressPrefix '10.0.1.0/28'

Saate luua soovitud VNet.

    New-AzureRmVirtualNetwork -Name testvnet -ResourceGroupName testrg `
    -Location 'West US' -AddressPrefix 10.0.0.0/16 -Subnet $subnet1, $subnet2

### <a name="gatewaysubnet"></a>Lüüsi alamvõrgu lisamiseks on juba loodud virtuaalse võrgus

See toiming on vaja ainult siis, kui teil on vaja lisada lüüsi alamvõrgu VNet, varem loodud.

Järgmises näites abil saate luua oma lüüsi alamvõrgu. Kindlasti nime lüüsi alamvõrgu "GatewaySubnet". Kui te nimi on midagi muud, loote mõnda alamvõrku, kuid Azure'i ei kohelge seda kui lüüsi alamvõrgu.

Määrake muutujaid.

    $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName testrg -Name testvnet

Saate luua lüüsi alamvõrgu.

    Add-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.3.0/28 -VirtualNetwork $vnet

Määrata. 

    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

## 3. <a name="localnet"> </a>teie kohalikus võrgus lüüsi lisamine

Virtuaalne võrku, kohtvõrgu lüüsi tavaliselt viitab oma kohapealse asukoht. Saate anda saidi nimi, mille Azure saate viidata ja määrata ka aadress ruumi eesliide kohtvõrgu lüüsi. 

Azure'i kasutab IP address eesliite teie määratud tuvastada, milliseid liiklust saata oma kohapealse asukohta. See tähendab, et peate määrama iga aadress eesliide, mida soovite olema seostatud teie kohalikus võrgus lüüsi. Kui kohapealse võrgu muutub, saate hõlpsasti värskendada nende eesliiteid. 

Kui kasutate PowerShelli näited, võtke arvesse järgmist.
    
- *GatewayIPAddress* on kohapealse VPN seadme IP-aadress. Seadme VPN ei leita taga mõnda NAT. 
- *AddressPrefix* on oma kohapealse aadressiruumi.

Ühe aadress eesliitega kohtvõrgu lüüsi lisamiseks tehke järgmist.

    New-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg `
    -Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix '10.5.51.0/24'

Kohaliku võrgu lüüsi koos mitme aadressi eesliiteid lisamiseks tehke järgmist.

    New-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg `
    -Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix @('10.0.0.0/24','20.0.0.0/24')

### <a name="to-modify-ip-address-prefixes-for-your-local-network-gateway"></a>IP-aadressi muutmiseks eesliidete teie kohalikus võrgus lüüsi jaoks

Mõnikord on teie kohalikus võrgus lüüsi eesliiteid muutuda. Muuta oma IP-aadress eesliiteid sammud sõltuvad sellest, kas teie loodud VPN-ühenduse lüüs. Käesoleva artikli jaotisest [muuta IP address eesliiteid lüüsi kohalikus võrgus](#modify) .


## <a name="PublicIP"></a>4. taotluse VPN-lüüsi avaliku IP-aadress

Järgmiseks koosolekukutse oma Azure'i VNet VPN-lüüsi eraldatav avaliku IP-aadress. See pole sama IP-aadress on määratud VPN seadme pigem määratakse Azure'i VPN-lüüsi ise. Te ei saa määrata IP-aadress, mida soovite kasutada. See on dünaamiliselt eraldatud lüüsi. Saate IP-aadressi konfigureerimisel seadme kohapealse VPN ühenduse lüüs.

Lüüsi Azure'i VPN-i ressursihaldur juurutamise mudeli praegu toetab vaid avaliku IP-aadresside dünaamilise jaotamise meetodi abil. Siiski ei tähenda see muudab IP-aadress. Ainult kellaaja Azure'i VPN gateway IP-aadress muutub on kui lüüsi on kustutatud ja uuesti luua. Lüüsi avaliku IP-aadressi ei muuda suurust, lähtestamine või muude sisemise hooldustööd/täienduste Azure'i VPN lüüsi üle.

Kasutage järgmist PowerShelli valimi.

    $gwpip= New-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName testrg -Location 'West US' -AllocationMethod Dynamic

## <a name="GatewayIPConfig"></a>5. luua lüüsi IP adresseerimise konfigureerimine

Lüüsi konfigureerimine määratleb alamvõrgu ja kasutada avaliku IP-aadressi. Järgmises näites abil saate luua oma lüüsi konfigureerimine.

    $vnet = Get-AzureRmVirtualNetwork -Name testvnet -ResourceGroupName testrg
    $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
    $gwipconfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name gwipconfig1 -SubnetId $subnet.Id -PublicIpAddressId $gwpip.Id 

## <a name="CreateGateway"></a>6. virtuaalse võrgu lüüsi loomine

Selles etapis tuleb teil luua virtuaalse võrgu lüüsi. Lüüsi loomine võib võtta kaua aega. Sageli 45 minutit või rohkem. 

Kasutage järgmisi väärtuseid:

- *-GatewayType* -saitide konfigureerimine on *Vpn*. Lüüsi tüüp on alati konfiguratsiooni, mida te rakendavad. Näiteks muude lüüsi konfiguratsioone võib olla vaja - GatewayType ExpressRoute. 

- *-VpnType* võib olla *RouteBased* (nimetatakse dünaamiliseks lüüsi mõned dokumendid) või *PolicyBased* (nimetatakse staatilise lüüsi mõned dokumendid). VPN lüüsi failitüüpide kohta leiate lisateavet teemast [VPN lüüsid](vpn-gateway-about-vpngateways.md#vpntype).
- *-GatewaySku* võib olla *tavaline*, *Standard*või *HighPerformance*.   

        New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
        -Location 'West US' -IpConfigurations $gwipconfig -GatewayType Vpn `
        -VpnType RouteBased -GatewaySku Standard

## <a name="ConfigureVPNDevice"></a>7. seadme VPN-i konfigureerimine

Sel hetkel, peate konfigureerida kohapealse VPN seadme virtuaalse võrgu lüüsi avaliku IP-aadress. Saate töötada kindlat konfiguratsiooni teavet oma seadme tootjalt. Viidata saate lisateavet [VPN seadmed](vpn-gateway-about-vpn-devices.md) .

Avaliku IP-aadressi virtuaalse võrgu lüüsi otsimiseks saate kasutada järgmises näites:

    Get-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName testrg

## <a name="CreateConnection"></a>8. VPN-ühenduse loomine

Järgmiseks luua virtuaalse võrgu lüüsi ja seadme VPN-saidilt VPN-ühendus. Ärge unustage asendage väärtused ise. Ühiskasutusega võti peab vastama teie VPN seadme konfiguratsiooni puhul kasutatud väärtus. Pange tähele, et selle `-ConnectionType` -saidilt on *IPsec*. 

Määrake muutujaid.

    $gateway1 = Get-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
    $local = Get-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg

Ühenduse loomine.

    New-AzureRmVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg `
    -Location 'West US' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
    -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'

Pärast lühikest aega, ühendus luuakse. 

## <a name="toverify"></a>Kinnitamiseks VPN-ühendus

On paar võimalust kinnitamiseks VPN-ühendus.

[AZURE.INCLUDE [vpn-gateway-verify-connection-rm](../../includes/vpn-gateway-verify-connection-rm-include.md)]

## <a name="modify"></a>IP-aadressi muutmiseks eesliidete kohtvõrgu lüüsi

Kui teil on vaja muuta oma kohalikus võrgus Gateway eesliiteid, kasutage järgmisi juhiseid. Kahte tüüpi juhised on olemas. Valige juhiseid sõltuvad sellest, kas olete juba loonud ühenduse lüüs. 

[AZURE.INCLUDE [vpn-gateway-modify-ip-prefix-rm](../../includes/vpn-gateway-modify-ip-prefix-rm-include.md)]

## <a name="modifygwipaddress"></a>Muuta kohtvõrgu lüüsi gateway IP-aadress

[AZURE.INCLUDE [vpn-gateway-modify-lng-gateway-ip-rm](../../includes/vpn-gateway-modify-lng-gateway-ip-rm-include.md)]

## <a name="next-steps"></a>Järgmised sammud

- Saate lisada virtuaalse võrguga virtuaalmasinates. Üksikasjalikud juhised leiate teemast [loomine virtuaalse masina](../virtual-machines/virtual-machines-windows-hero-tutorial.md) .

- BGP kohta leiate teavet teemast [BGP ülevaade](vpn-gateway-bgp-overview.md) ja [Kuidas konfigureerida BGP](vpn-gateway-bgp-resource-manager-ps.md).

