<properties
   pageTitle="Konfigureerimise Active Active S2S VPN-ühendused Azure'i VPN lüüside Azure'i ressursihaldur ja PowerShelli abil | Microsoft Azure'i"
   description="Selles artiklis tutvustatakse seadistamine aktiivne aktiivne ühendused Azure'i VPN lüüside Azure'i ressursihaldur ja PowerShelli abil."
   services="vpn-gateway"
   documentationCenter="na"
   authors="yushwang"
   manager="rossort"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/26/2016"
   ms.author="yushwang"/>

# <a name="configure-active-active-s2s-vpn-connections-with-azure-vpn-gateways-using-azure-resource-manager-and-powershell"></a>Aktiivne-active S2S VPN-ühendused ja Azure VPN lüüside Azure'i ressursihaldur ja PowerShelli abil konfigureerimine

Selles artiklis tutvustatakse juhised loomiseks aktiivne aktiivne asukohaga ja VNet-VNet ühendused ressursihaldur juurutamise mudeli ja PowerShelli abil.


**Azure'i juurutamise mudelite kohta**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="about-highly-available-cross-premises-connections"></a>Väga kättesaadav asutusesiseses ühenduste kohta

Kõrge-saadavus asukohaga ja VNet-VNet ühenduvuse saavutamiseks tuleks juurutada VPN lüüside ja luua mitme paralleelse seoseid oma võrgu ja Azure. Kohta leiate teemast [väga saadaval asukohaga ja VNet-VNet Connectivity](./vpn-gateway-highlyavailable.md) Ühenduvus suvandid ja topoloogia ülevaade.

Selles artiklis toodud juhiseid, et aktiivne aktiivne asutusesiseses VPN-ühendus, ja aktiivne aktiivne seos kahe virtuaalse võrgu häälestamine:

- [Osa 1 - loomine ja konfigureerimine oma Azure'i VPN-lüüsi aktiivne aktiivne režiim](#aagateway)

- [Osa 2 - aktiivne aktiivne asutusesiseses loomiseks](#aacrossprem)

- [Osa 3 - aktiivne aktiivne VNet-VNet loomiseks](#aav2v)

- [Osa 4 - olemasoleva aktiivne aktiivne ja aktiivne-puhkerežiimis olev lüüsi värskendamine](#aaupdate)

Saate ühendada need koos, et koostada keerukamaid tugevalt saadaval võrgutopoloogia, mis vastab teie vajadustele.

>[AZURE.IMPORTANT] Pange tähele, et aktiivne aktiivne režiim töötab ainult HighPerformance SKU


## <a name ="aagateway"></a>Osa 1 - loomine ja konfigureerimine aktiivne aktiivne VPN lüüsid

Järgmised toimingud konfigureerib Azure'i VPN lüüsi aktiivne-active režiimis. Peamised erinevused aktiivne aktiivne ja aktiivne-puhkerežiimis olev lüüside:

- Peate looma kaks Gateway IP konfiguratsioone koos kahe avaliku IP-aadressid
- Peate seadistate EnableActiveActiveFeature lipp
- Lüüsi SKU peab olema HighPerformance

Muud atribuudid on sama, mis active active lüüside. 

### <a name="before-you-begin"></a>Enne alustamist

- Veenduge, et teil on Azure tellimuse. Kui teil pole veel Azure tellimuse, saate aktiveerida oma [MSDN-i abonendi kasu](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) või Logi üles [tasuta konto](https://azure.microsoft.com/pricing/free-trial/).
    
- Peate installida Azure ressursihaldur PowerShelli cmdlet-käsud. Vaadake, [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md) installimise PowerShelli cmdlet-käskude kohta lisateavet.

### <a name="step-1---create-and-configure-vnet1"></a>Samm 1 - loomine ja konfigureerimine VNet1

#### <a name="1-declare-your-variables"></a>1 tuvastada teie muutujad

Selle toiminguga saate Alustuseks deklareerimine meie muutujate. Järgmises näites kinnitab muutujate abil selle ülesande jaoks soovitud väärtused. Ärge unustage asendage väärtused ise tootmiseks konfigureerimisel. Kui teil on toodud juhiseid abil saate tutvuda sellise konfiguratsiooni, saate need muutujad. Muutke muutujaid, ja seejärel kopeerige ja kleepige oma PowerShelli konsooli.

    $Sub1          = "Ross"
    $RG1           = "TestAARG1"
    $Location1     = "West US"
    $VNetName1     = "TestVNet1"
    $FESubName1    = "FrontEnd"
    $BESubName1    = "Backend"
    $GWSubName1    = "GatewaySubnet"
    $VNetPrefix11  = "10.11.0.0/16"
    $VNetPrefix12  = "10.12.0.0/16"
    $FESubPrefix1  = "10.11.0.0/24"
    $BESubPrefix1  = "10.12.0.0/24"
    $GWSubPrefix1  = "10.12.255.0/27"
    $VNet1ASN      = 65010
    $DNS1          = "8.8.8.8"
    $GWName1       = "VNet1GW"
    $GW1IPName1    = "VNet1GWIP1"
    $GW1IPName2    = "VNet1GWIP2"
    $GW1IPconf1    = "gw1ipconf1"
    $GW1IPconf2    = "gw1ipconf2"
    $Connection12  = "VNet1toVNet2"
    $Connection151 = "VNet1toSite5_1"
    $Connection152 = "VNet1toSite5_2"

#### <a name="2-connect-to-your-subscription-and-create-a-new-resource-group"></a>2. ühenduse tellimuse ja ressursside uue rühma loomine

Veenduge, et te lülitumine PowerShelli kasutamine ressursihaldur cmdlet-käsud. Lisateavet leiate teemast [Windows PowerShelli kasutamine koos ressursihaldur](../powershell-azure-resource-manager.md).

Avage oma PowerShelli konsooli ja teie kontoga ühendust luua. Järgmises näites abil saate ühendada:

    Login-AzureRmAccount
    Select-AzureRmSubscription -SubscriptionName $Sub1
    New-AzureRmResourceGroup -Name $RG1 -Location $Location1

#### <a name="3-create-testvnet1"></a>3. TestVNet1 loomine

Allpool näidis loob virtuaalse võrgu nimega TestVNet1 ja kolme alamvõrku, üks nimega GatewaySubnet, üks nimega FrontEnd ja nimega ühele kirjutamata. Kui väärtused, on oluline alati pange nimi oma lüüsi alamvõrgu spetsiaalselt GatewaySubnet. Kui te nimi on midagi muud, ei õnnestu oma lüüsi loomine. 

    $fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1
    $besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
    $gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1

    New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1

### <a name="step-2---create-the-vpn-gateway-for-testvnet1-with-active-active-mode"></a>Samm 2 – aktiivne-active režiimiga TestVNet1 VPN-lüüsi loomine

#### <a name="1-create-the-public-ip-addresses-and-gateway-ip-configurations"></a>1. luua avaliku IP-aadressid ja gateway IP konfiguratsioone

Kahe avaliku IP-aadresside loote oma VNet lüüsi eraldatav koosolekukutse. Saate määratleda ka alamvõrgu ja nõutud IP konfiguratsioone. 

    $gw1pip1    = New-AzureRmPublicIpAddress -Name $GW1IPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic
    $gw1pip2    = New-AzureRmPublicIpAddress -Name $GW1IPName2 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic

    $vnet1      = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
    $subnet1    = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
    $gw1ipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW1IPconf1 -Subnet $subnet1 -PublicIpAddress $gw1pip1
    $gw1ipconf2 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW1IPconf2 -Subnet $subnet1 -PublicIpAddress $gw1pip2

#### <a name="2-create-the-vpn-gateway-with-active-active-configuration"></a>2. aktiivne aktiivne konfiguratsiooni VPN-lüüsi loomine

Luua virtuaalse võrgu lüüsi TestVNet1. Pange tähele, et kahe GatewayIpConfig kirjeid EnableActiveActiveFeature lippu. Aktiivne aktiivne režiim nõuab marsruutimiseks vastavalt VPN lüüsi, HighPerformance SKU-ga. Lüüsi loomine võib võtta aega (30 minuti või rohkem valmis).

    New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 -Location $Location1 -IpConfigurations $gw1ipconf1,$gw1ipconf2 -GatewayType Vpn -VpnType RouteBased -GatewaySku HighPerformance -Asn $VNet1ASN -EnableActiveActiveFeature -Debug

#### <a name="3-obtain-the-gateway-public-ip-addresses-and-the-bgp-peer-ip-address"></a>3. saada lüüsi avaliku IP-aadressid ja BGP omavahelistes IP-aadress

Pärast lüüsi loomist peate hankida Azure'i VPN lüüsi BGP omavahelistes IP-aadress. See aadress on vaja konfigureerida Azure VPN-lüüsi BGP omavahelistes oma kohapealse VPN seadmete jaoks.

    $gw1pip1 = Get-AzureRmPublicIpAddress -Name $GW1IPName1 -ResourceGroupName $RG1
    $gw1pip2 = Get-AzureRmPublicIpAddress -Name $GW1IPName2 -ResourceGroupName $RG1
    $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1

Kasutage kahe avaliku IP-aadresside VPN-lüüsi ja nende vastavate BGP omavahelistes IP-aadressid iga lüüsi eksemplari eraldatud kuvamiseks järgmised cmdlet-käsud:

    PS D:\> $gw1pip1.IpAddress
    40.112.190.5

    PS D:\> $gw1pip2.IpAddress
    138.91.156.129

    PS D:\> $vnet1gw.BgpSettingsText
    {
      "Asn": 65010,
      "BgpPeeringAddress": "10.12.255.4,10.12.255.5",
      "PeerWeight": 0
    }

Lüüsi eksemplari järjestuse avaliku IP-aadressid ja vastavad BGP silmitsemine aadressid on samad. Selles näites lüüsi VM avaliku IP 40.112.190.5, peaks kasutama 10.12.255.4 BGP silmitsemine aadress ja koos 138.91.156.129 lüüs hakkab kasutama 10.12.255.5. See teave on vajalik, kui häälestate oma sees tööruumide VPN seadmete ühenduse lüüs aktiivne aktiivne. Lüüs on kujutatud diagramm, kõik aadressidega:

![aktiivne-active lüüsi](./media/vpn-gateway-activeactive-rm-powershell/active-active-gw.png)

Kui lüüsi on loodud, saate selle lüüsi aktiivne aktiivne asutusesiseses või VNet-VNet ühendust luua. Järgmistes jaotistes juhendab kasutamise lõpuleviimiseks juhised.


## <a name ="aacrossprem"></a>Osa 2 - aktiivne aktiivne asukohaga ühendust luua

Asutusesiseses ühenduse loomiseks peate kohaliku võrgu lüüsi tähistada seadme kohapealse VPN ja Azure VPN-lüüsi suhtlemiseks kohtvõrgu lüüsi ühenduse loomiseks. Selles näites on Azure VPN-lüüsi aktiivne aktiivne režiimis. Selle tulemusena isegi juhul, kui seal on ainult üks kohapealse VPN-seade (kohtvõrgu lüüs) ja üks ühendus ressursi, nii Azure'i VPN-lüüsi eksemplari loob S2S VPN tunnelite kohapealse seadmega.

Enne jätkamist veenduge, et olete täitnud [1 osa](#aagateway) selle ülesande.

### <a name="step-1---create-and-configure-the-local-network-gateway"></a>Samm 1 - loomine ja konfigureerimine kohtvõrgu lüüsi

#### <a name="1-declare-your-variables"></a>1 tuvastada teie muutujad

Selle ülesande jätkab konfiguratsiooni joonisel. Kindlasti need, mida soovite kasutada oma konfiguratsioon väärtuste asendamine.

    $RG5           = "TestAARG5"
    $Location5     = "West US"
    $LNGName51     = "Site5_1"
    $LNGPrefix51   = "10.52.255.253/32"
    $LNGIP51       = "131.107.72.22"
    $LNGASN5       = 65050
    $BGPPeerIP51   = "10.52.255.253"

Mõned asjad, mida kohtvõrgu lüüsi parameetrite kohta:

- Lüüsi kohalikus võrgus võib olla samas või mõnes muus asukohas ja ressursirühma nimega VPN-lüüsi. Selles näites kirjeldatakse neid erinevate ressursside rühmad, kuid Azure samasse asukohta.

- Kui see on ainult üks kohapealse VPN-seade, nagu eespool näidatud, aktiivne aktiivne ühendus saate töötada koos või ilma BGP Protocol (protokoll). Selles näites kasutatakse BGP asutusesiseses ühenduse.

- Kui BGP on lubatud, peate deklareerida kohtvõrgu lüüsi eesliide on hosti aadress VPN-seadmes BGP omavahelistes IP-aadressi. Sel juhul on mõne /32 eesliitega "10.52.255.253/32".

- Meeldetuletuseks, peate kasutama erinevaid BGP ASNs oma kohapealse võrgu ja Azure VNet vahel. Kui need on samad, peate muutma oma VNet ASN, kui seadme kohapealse VPN juba kasutab funktsiooni ASN vastastikune muude BGP naabritega.

#### <a name="2-create-the-local-network-gateway-for-site5"></a>2. Site5 kohtvõrgu lüüsi loomine
    
Enne jätkamist, veenduge, et teil on ühendus tellimuse 1. Kui see pole veel loodud, saate luua ressursirühma.

    New-AzureRmResourceGroup       -Name $RG5 -Location $Location5
    New-AzureRmLocalNetworkGateway -Name $LNGName51 -ResourceGroupName $RG5 -Location $Location5 -GatewayIpAddress $LNGIP51 -AddressPrefix $LNGPrefix51 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP51

### <a name="step-2---connect-the-vnet-gateway-and-local-network-gateway"></a>Samm 2 - ühenduse VNet lüüsi ja kohtvõrgu lüüsi

#### <a name="1-get-the-two-gateways"></a>1. kaks lüüside hankimine

    $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
    $lng5gw1 = Get-AzureRmLocalNetworkGateway   -Name $LNGName51 -ResourceGroupName $RG5

#### <a name="2-create-the-testvnet1-to-site5-connection"></a>2 TestVNet1 Site5 ühenduse loomine

Selles etapis tuleb saavad luua ühenduse kaudu TestVNet1 Site5_1 koos "EnableBGP" määratud $True.

    New-AzureRmVirtualNetworkGatewayConnection -Name $Connection151 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw1 -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP True

#### <a name="3-vpn-and-bgp-parameters-for-your-on-premises-vpn-device"></a>3. VPN-i ja BGP seadme kohapealse VPN parameetrid

Järgmises näites on loetletud parameetrid sisestate BGP konfiguratsiooni jaotisse seadme kohapealse VPN selle ülesande jaoks.

    - Site5 ASN: 65050
    - Site5 BGP IP: 10.52.255.253
    - Eesliidete tähised teatada: (näiteks) 10.51.0.0/16 ja 10.52.0.0/16
    - Azure'i VNet ASN: 65010
    - Azure'i VNet BGP IP 1: 10.12.255.4 tunneliga, et 40.112.190.5 jaoks
    - Azure'i VNet BGP IP 2: 10.12.255.5 tunneliga, et 138.91.156.129 jaoks
    - Staatilise marsruudib: sihtkohta 10.12.255.4/32, nexthop VPN tunneliga liidese 40.112.190.5 sihtkoha 10.12.255.5/32 nexthop VPN tunneliga liides 138.91.156.129
    - eBGP Multihop: tagada suvandi "multihop" eBGP on lubatud teie seadmes vajaduse korral

Mõne minuti pärast saa ühendust luua, ja BGP silmitsemine seansi hakkavad pärast selle ühenduse loomist. Selles näites on seni konfigureeritud ainult üks kohapealse VPN seadme tulemuseks skeemi allpool näidatud:

![aktiivne-aktiivne-crossprem](./media/vpn-gateway-activeactive-rm-powershell/active-active.png)

### <a name="step-3---connect-two-on-premises-vpn-devices-to-the-active-active-vpn-gateway"></a>Samm 3 – aktiivne aktiivne VPN-lüüsi kahe kohapealse VPN seadmete ühendamine

Kui teil on kaks VPN seadmete kohapealse samasse võrku, võite saavutada kahekordne koondamise luues ühenduse Azure'i VPN lüüsist teise VPN-seade.

#### <a name="1-create-the-second-local-network-gateway-for-site5"></a>1. Site5 teine kohtvõrgu lüüsi loomine

Pange tähele, et gateway IP-aadress, aadress eesliide ja teine kohtvõrgu lüüsi BGP silmitsemine aadress peab ei kattu eelmise kohtvõrgu lüüsi sama kohapealse võrgu jaoks. 

    $LNGName52     = "Site5_2"
    $LNGPrefix52   = "10.52.255.254/32"
    $LNGIP52       = "131.107.72.23"
    $BGPPeerIP52   = "10.52.255.254"

    New-AzureRmLocalNetworkGateway -Name $LNGName52 -ResourceGroupName $RG5 -Location $Location5 -GatewayIpAddress $LNGIP52 -AddressPrefix $LNGPrefix52 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP52
 
#### <a name="2-connect-the-vnet-gateway-and-the-second-local-network-gateway"></a>2. ühenduse VNet lüüsi ja teine kohtvõrgu lüüsi

Saate luua ühenduse TestVNet1 Site5_2 koos "EnableBGP" määratud $True

    $lng5gw2 = Get-AzureRmLocalNetworkGateway   -Name $LNGName52 -ResourceGroupName $RG5

    New-AzureRmVirtualNetworkGatewayConnection -Name $Connection152 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw2 -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP True

#### <a name="3-vpn-and-bgp-parameters-for-your-second-on-premises-vpn-device"></a>3. VPN-i ja BGP teise kohapealse VPN seadme parameetrid

Samuti all loendite parameetrid siis sisestage VPN teise seadmesse:

    - Site5 ASN: 65050
    - Site5 BGP IP: 10.52.255.254
    - Eesliidete tähised teatada: (näiteks) 10.51.0.0/16 ja 10.52.0.0/16
    - Azure'i VNet ASN: 65010
    - Azure'i VNet BGP IP 1: 10.12.255.4 tunneliga, et 40.112.190.5 jaoks
    - Azure'i VNet BGP IP 2: 10.12.255.5 tunneliga, et 138.91.156.129 jaoks
    - Staatilise marsruudib: sihtkohta 10.12.255.4/32, nexthop VPN tunneliga liidese 40.112.190.5 sihtkoha 10.12.255.5/32 nexthop VPN tunneliga liides 138.91.156.129
    - eBGP Multihop: tagada suvandi "multihop" eBGP on lubatud teie seadmes vajaduse korral

Kui ühendus (tunnelid) on loodud, on teil kaks liigsete VPN seadmed ja kohapealse võrgu ja Azure tunnelid:

![kahe-koondamise-crossprem](./media/vpn-gateway-activeactive-rm-powershell/dual-redundancy.png)


## <a name ="aav2v"></a>Osa 3 - aktiivne aktiivne VNet-VNet ühendust luua

Selles jaotises loob aktiivne-active VNet-VNet ühenduse BGP abil. 

Ülaltoodud eelmisi juhiseid Jätka allpool toodud juhiseid. Peate teostama loomine ja konfigureerimine TestVNet1 ja VPN-lüüsi BGP [1 osa](#aagateway) . 

### <a name="step-1---create-testvnet2-and-the-vpn-gateway"></a>Samm 1 - TestVNet2 ja VPN-lüüsi loomine

See on oluline veenduge, et IP-aadress ruumi uue virtuaalse võrgu TestVNet2, ei kattu oma VNet vahemikke.

Selles näites virtuaalne võrkude kuuluvad sama tellimus. Saate häälestada VNet-VNet seoseid eri tellimuste; Vaadake lisateabe saamiseks [ühenduse VNet-VNet konfigureerimine](./vpn-gateway-vnet-vnet-rm-ps.md) . Veenduge, et lisate selle "-EnableBgp $True" lubamiseks BGP ühenduste loomisel.

#### <a name="1-declare-your-variables"></a>1 tuvastada teie muutujad

Kindlasti need, mida soovite kasutada oma konfiguratsioon väärtuste asendamine.

    $RG2           = "TestAARG2"
    $Location2     = "East US"
    $VNetName2     = "TestVNet2"
    $FESubName2    = "FrontEnd"
    $BESubName2    = "Backend"
    $GWSubName2    = "GatewaySubnet"
    $VNetPrefix21  = "10.21.0.0/16"
    $VNetPrefix22  = "10.22.0.0/16"
    $FESubPrefix2  = "10.21.0.0/24"
    $BESubPrefix2  = "10.22.0.0/24"
    $GWSubPrefix2  = "10.22.255.0/27"
    $VNet2ASN      = 65020
    $DNS2          = "8.8.8.8"
    $GWName2       = "VNet2GW"
    $GW2IPName1    = "VNet2GWIP1"
    $GW2IPconf1    = "gw2ipconf1"
    $GW2IPName2    = "VNet2GWIP2"
    $GW2IPconf2    = "gw2ipconf2"
    $Connection21  = "VNet2toVNet1"
    $Connection12  = "VNet1toVNet2"

#### <a name="2-create-testvnet2-in-the-new-resource-group"></a>2 TestVNet2 luua uue ressursirühma

    New-AzureRmResourceGroup -Name $RG2 -Location $Location2

    $fesub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName2 -AddressPrefix $FESubPrefix2
    $besub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName2 -AddressPrefix $BESubPrefix2
    $gwsub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName2 -AddressPrefix $GWSubPrefix2

    New-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2 -Location $Location2 -AddressPrefix $VNetPrefix21,$VNetPrefix22 -Subnet $fesub2,$besub2,$gwsub2

#### <a name="3-create-the-active-active-vpn-gateway-for-testvnet2"></a>3. TestVNet2 aktiivne aktiivne VPN-lüüsi loomine

Kahe avaliku IP-aadresside loote oma VNet lüüsi eraldatav koosolekukutse. Saate määratleda ka alamvõrgu ja nõutud IP konfiguratsioone. 

    $gw2pip1    = New-AzureRmPublicIpAddress -Name $GW2IPName1 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic
    $gw2pip2    = New-AzureRmPublicIpAddress -Name $GW2IPName2 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic

    $vnet2      = Get-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2
    $subnet2    = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet2
    $gw2ipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW2IPconf1 -Subnet $subnet2 -PublicIpAddress $gw2pip1
    $gw2ipconf2 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW2IPconf2 -Subnet $subnet2 -PublicIpAddress $gw2pip2

Saate luua VPN-lüüsi AS arv ja "EnableActiveActiveFeature" lippu. Pange tähele, et tuleb alistada vaikesäte ASN sisse oma Azure'i VPN lüüsid. Ühendatud VNets jaoks ASNs peavad olema erinevad BGP ja teel marsruutimist.

    New-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2 -Location $Location2 -IpConfigurations $gw2ipconf1,$gw2ipconf2 -GatewayType Vpn -VpnType RouteBased -GatewaySku HighPerformance -Asn $VNet2ASN -EnableActiveActiveFeature

### <a name="step-2---connect-the-testvnet1-and-testvnet2-gateways"></a>Samm 2 – TestVNet1 ja TestVNet2 lüüside ühenduse loomine

Selles näites on mõlemad lüüside ühe tellimuse. Seda toimingut saate sama PowerShelli seanss.

#### <a name="1-get-both-gateways"></a>1. nii lüüside hankimine

Veenduge, et saate sisse logida ja ühenduse tellimuse 1.

    $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
    $vnet2gw = Get-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2
    
#### <a name="2-create-both-connections"></a>2. nii ühenduste loomine

Selles etapis loote TestVNet1 ühenduse TestVNet2 ja ühenduse kaudu TestVNet2 TestVNet1 abil.

    New-AzureRmVirtualNetworkGatewayConnection -Name $Connection12 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet2gw -Location $Location1 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True

    New-AzureRmVirtualNetworkGatewayConnection -Name $Connection21 -ResourceGroupName $RG2 -VirtualNetworkGateway1 $vnet2gw -VirtualNetworkGateway2 $vnet1gw -Location $Location2 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True

>[AZURE.IMPORTANT] Kindlasti BGP lubamine mõlemad ühendused.

Pärast neid juhiseid ühenduse loob paar minutit ja selle BGP silmitsemine seansi saab ühenduse VNet-VNet kahekordne koondamine täidetud:

![aktiivne-aktiivne-v2v](./media/vpn-gateway-activeactive-rm-powershell/vnet-to-vnet.png)

## <a name ="aaupdate"></a>Osa 4 - olemasoleva aktiivne aktiivne ja aktiivne-puhkerežiimis olev lüüsi värskendamine

Viimases osas kirjeldatakse, kuidas saate konfigureerida mõne olemasoleva Azure'i VPN-lüüsi kaudu aktiivne-puhkerežiimis olev aktiivne aktiivne režiim, või vastupidi.

>[AZURE.IMPORTANT] Pange tähele, et aktiivne aktiivne režiim töötab ainult HighPerformance SKU

### <a name="configure-an-active-standby-gateway-to-active-active-gateway"></a>Aktiivne-puhkerežiimis olev lüüsist aktiivne aktiivne lüüsi konfigureerimine

#### <a name="1-gateway-parameters"></a>1. parameetrite lüüsi

Järgmises näites on aktiivne-puhkerežiimis olev lüüsi teisendab on aktiivne aktiivne lüüsi. Teil on vaja luua teise avaliku IP-aadress ja seejärel lisage teine Gateway IP-konfiguratsiooni. Allpool on kuvatud kasutatavad parameetrid.

    $GWName     = "TestVNetAA1GW"
    $VNetName   = "TestVNetAA1"
    $RG         = "TestVPNActiveActive01"
    $GWIPName2  = "gwpip2"
    $GWIPconf2  = "gw1ipconf2"

    $vnet       = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG
    $subnet     = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
    $gw         = Get-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
    $location   = $gw.Location

#### <a name="2-create-the-public-ip-address-then-add-the-second-gateway-ip-configuration"></a>2 luua avaliku IP-aadress ja seejärel lisage teine lüüsi IP konfigureerimine

    $gwpip2     = New-AzureRmPublicIpAddress -Name $GWIPName2 -ResourceGroupName $RG -Location $location -AllocationMethod Dynamic
    Add-AzureRmVirtualNetworkGatewayIpConfig -VirtualNetworkGateway $gw -Name $GWIPconf2 -Subnet $subnet -PublicIpAddress $gwpip2 

#### <a name="3-enable-active-active-mode-and-update-the-gateway"></a>3. luba aktiivne aktiivne režiim ja värskendage lüüs

Peate määrama lüüsi objekti PowerShelli tegelik värskenduse käivitamiseks. Lüüsi objekti SKU ka välja vahetada HighPerformance, kuna see on varem loodud Standard.

    Set-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -EnableActiveActiveFeature -GatewaySku HighPerformance

Selle värskenduse tegemiseks võib kuluda 30-45 minutit.

### <a name="configure-an-active-active-gateway-to-active-standby-gateway"></a>Aktiivne-active lüüsist aktiivne-puhkerežiimis olev lüüsi konfigureerimine

#### <a name="1-gateway-parameters"></a>1. parameetrite lüüsi

Kasutage sama parameetrid ülal, saada nime IP-konfiguratsiooni, mille soovite eemaldada.

    $GWName     = "TestVNetAA1GW"
    $RG         = "TestVPNActiveActive01"

    $gw         = Get-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
    $ipconfname = $gw.IpConfigurations[1].Name

#### <a name="2-remove-the-gateway-ip-configuration-and-disable-the-active-active-mode"></a>2. eemaldada gateway IP-konfiguratsiooni ja aktiivne-aktiivse režiimi keelamine

Samuti peate määrama lüüsi objekti PowerShelli tegelik värskenduse käivitamiseks.

    Remove-AzureRmVirtualNetworkGatewayIpConfig -Name $ipconfname -VirtualNetworkGateway $gw
    Set-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -DisableActiveActiveFeature

Värskendamiseks võib kuluda kuni 30-45 minutit.


## <a name="next-steps"></a>Järgmised sammud

Kui teie ühendus on loodud, saate lisada virtuaalse võrguga virtuaalmasinates. Üksikasjalikud juhised leiate teemast [loomine virtuaalse masina](../virtual-machines/virtual-machines-windows-hero-tutorial.md) .

