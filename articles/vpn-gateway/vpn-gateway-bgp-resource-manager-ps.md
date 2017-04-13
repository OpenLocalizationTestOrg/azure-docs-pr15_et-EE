<properties
   pageTitle="Kuidas seadistada BGP Azure'i VPN lüüside Azure'i ressursihaldur ja PowerShelli abil | Microsoft Azure'i"
   description="Selles artiklis tutvustatakse seadistamine BGP Azure'i VPN lüüside Azure'i ressursihaldur ja PowerShelli abil."
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
   ms.date="04/15/2016"
   ms.author="yushwang"/>

# <a name="how-to-configure-bgp-on-azure-vpn-gateways-using-azure-resource-manager-and-powershell"></a>Kuidas seadistada BGP Azure'i VPN lüüside Azure'i ressursihaldur ja PowerShelli abil

Selles artiklis tutvustatakse toimingute BGP asutusesiseses saidi saidi (S2S) VPN-ühendus ja VNet-VNet ühenduse ressursihaldur juurutamise mudeli ja PowerShelli abil.


**Azure'i juurutamise mudelite kohta**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="about-bgp"></a>BGP kohta

BGP on standardne marsruutimise protokoll levinud Interneti kahe või enama võrgu marsruutimine ja reachability teavet vahetada. BGP võimaldab Azure'i VPN lüüside ja teie kohapealse VPN seadmetes, nimetatakse BGP kolleegidega või naabrid, Exchange "marsruudib", mis teavitab nii lüüside kättesaadavuse ja reachability nende eesliidete läbida lüüsid või ruuterid seotud jaoks. BGP saate lubada teel marsruutimine vahel mitme võrgu lisaajast marsruudib BGP lüüsi õpib ühe BGP partnerilt kõik BGP kolleegidega.

Lugege rohkem arutelu eeliste BGP ja mõista tehnilised nõuded ja BGP kasutamisel peaksite arvesse võtma [BGP ülevaade koos Azure VPN lüüsid](./vpn-gateway-bgp-overview.md) .

## <a name="getting-started-with-bgp-on-azure-vpn-gateways"></a>Kasutamise alustamine BGP Azure'i VPN lüüsid

See artikkel annab teile juhiseid teha järgmisi toiminguid:

- [Osa 1 - luba BGP oma Azure'i VPN Gateway](#enablebgp)

- [Osa 2 - BGP asukohaga ühendust luua](#crossprembgp)

- [Osa 3 - BGP VNet-VNet ühendust luua](#v2vbgp)

Juhiseid iga osa moodustab põhilise koosteüksuse jaoks lubada BGP võrguühendust. Kui täidate kõik kolm osa, kas koostate topoloogia nagu on näidatud järgmisel joonisel:

![BGP topoloogia](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crosspremv2v.png)

Saate ühendada need koos, et koostada keerukamaid, mitme loodame, teel võrku, mis vastavad teie vajadustele.

## <a name ="enablebgp"></a>Osa 1 - BGP Azure VPN lüüsi konfigureerimine

Järgmist konfiguratsiooni setup BGP parameetrid Azure'i VPN-lüüsi, nagu on näidatud järgmisel joonisel:

![BGP lüüsi](./media/vpn-gateway-bgp-resource-manager-ps/bgp-gateway.png)

### <a name="before-you-begin"></a>Enne alustamist

- Veenduge, et teil on Azure tellimuse. Kui teil pole veel Azure tellimuse, saate aktiveerida oma [MSDN-i abonendi kasu](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) või Logi üles [tasuta konto](https://azure.microsoft.com/pricing/free-trial/).
    
- Peate installida Azure ressursihaldur PowerShelli cmdlet-käsud. Vaadake, [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md) installimise PowerShelli cmdlet-käskude kohta lisateavet.

### <a name="step-1---create-and-configure-vnet1"></a>Samm 1 - loomine ja konfigureerimine VNet1 

#### <a name="1-declare-your-variables"></a>1 tuvastada teie muutujad

Selle toiminguga saate Alustuseks deklareerimine meie muutujate. Järgmises näites kinnitab muutujate abil selle ülesande jaoks soovitud väärtused. Ärge unustage asendage väärtused ise tootmiseks konfigureerimisel. Kui teil on toodud juhiseid abil saate tutvuda sellise konfiguratsiooni, saate need muutujad. Muutke muutujaid, ja seejärel kopeerige ja kleepige oma PowerShelli konsooli.

    $Sub1          = "Replace_With_Your_Subcription_Name"
    $RG1           = "TestBGPRG1"
    $Location1     = "East US"
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
    $GWIPName1     = "VNet1GWIP"
    $GWIPconfName1 = "gwipconf1"
    $Connection12  = "VNet1toVNet2"
    $Connection15  = "VNet1toSite5"

#### <a name="2-connect-to-your-subscription-and-create-a-new-resource-group"></a>2. ühenduse tellimuse ja ressursside uue rühma loomine

Veenduge, et te lülitumine PowerShelli kasutamine ressursihaldur cmdlet-käsud. Lisateavet leiate teemast [Windows PowerShelli kasutamine koos ressursihaldur](../powershell-azure-resource-manager.md).

Avage oma PowerShelli konsooli ja teie kontoga ühendust luua. Järgmises näites abil saate ühendada:

    Login-AzureRmAccount
    Select-AzureRmSubscription -SubscriptionName $Sub1
    New-AzureRmResourceGroup -Name $RG1 -Location $Location1

#### <a name="3-create-testvnet1"></a>3. TestVNet1 loomine

Allpool näidis loob virtuaalse võrgu nimega TestVNet1 ja kolme alamvõrku, üks nimega GatewaySubnet, üks nimega FrontEnd ja ühe nimega kirjutamata. Kui väärtused, on oluline alati pange nimi oma lüüsi alamvõrgu spetsiaalselt GatewaySubnet. Kui te nimi on midagi muud, ei õnnestu oma lüüsi loomine. 

    $fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1
    $besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
    $gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1

    New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1

### <a name="step-2---create-the-vpn-gateway-for-testvnet1-with-bgp-parameters"></a>Samm 2 – BGP parameetriga TestVNet1 VPN-lüüsi loomine

#### <a name="1-create-the-ip-and-subnet-configurations"></a>1. IP ja alamvõrgu konfiguratsioone loomine

Koosolekukutse loote oma VNet lüüsi eraldatav avaliku IP-aadress. Saate määratleda ka alamvõrgu ja nõutud IP konfiguratsioone. 

    $gwpip1    = New-AzureRmPublicIpAddress -Name $GWIPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic
    
    $vnet1     = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
    $subnet1   = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
    $gwipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName1 -Subnet $subnet1 -PublicIpAddress $gwpip1

#### <a name="2-create-the-vpn-gateway-with-the-as-number"></a>2 AS numbriga VPN lüüsi loomine

Luua virtuaalse võrgu lüüsi TestVNet1. Pange tähele, et BGP marsruutimiseks vastavalt VPN-lüüsi ja parameetri lisamine – asparagiinisisaldusega TestVNet1 ASN (AS arv) määramiseks. Lüüsi loomine võib võtta aega (30 minuti või rohkem valmis).

    New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 -Location $Location1 -IpConfigurations $gwipconf1 -GatewayType Vpn -VpnType RouteBased -GatewaySku HighPerformance -Asn $VNet1ASN

#### <a name="3-obtain-the-azure-bgp-peer-ip-address"></a>3. Azure'i BGP omavahelistes IP-aadressi hankimine

Pärast lüüsi loomist peate hankida Azure'i VPN lüüsi BGP omavahelistes IP-aadress. See aadress on vaja konfigureerida Azure VPN-lüüsi BGP omavahelistes oma kohapealse VPN seadmete jaoks.

    $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
    $vnet1gw.BgpSettingsText

Viimase käsu kuvatakse vastava BGP konfiguratsioone Azure'i VPN lüüsi; näiteks:

    $vnet1gw.BgpSettingsText
    {
        "Asn": 65010,
        "BgpPeeringAddress": "10.12.255.30",
        "PeerWeight": 0
    }

Kui lüüsi on loodud, saate selle lüüsi asutusesiseses ühendus või BGP VNet-VNet ühenduse luua. Järgmistes jaotistes juhendab kasutamise lõpuleviimiseks juhised.

## <a name ="crossprembbgp"></a>Osa 2 - BGP asukohaga ühendust luua

Asutusesiseses ühenduse loomiseks peate kohaliku võrgu lüüsi tähistada seadme kohapealse VPN ja Azure VPN-lüüsi suhtlemiseks kohtvõrgu lüüsi ühenduse loomiseks. Selles artiklis antud juhiseid vahe on nõutavad BGP konfiguratsiooni parameetrite täiendavad atribuudid.

![BGP asutusesiseses](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crossprem.png)

Enne jätkamist veenduge, et olete täitnud [1 osa](#enablebgp) selle ülesande.

### <a name="step-1---create-and-configure-the-local-network-gateway"></a>Samm 1 - loomine ja konfigureerimine kohtvõrgu lüüsi

#### <a name="1-declare-your-variables"></a>1 tuvastada teie muutujad

Selle ülesande jätkab konfiguratsiooni joonisel. Kindlasti need, mida soovite kasutada oma konfiguratsioon väärtuste asendamine.

    $RG5           = "TestBGPRG5"
    $Location5     = "East US 2"
    $LNGName5      = "Site5"
    $LNGPrefix50   = "10.52.255.254/32"
    $LNGIP5        = "Your_VPN_Device_IP"
    $LNGASN5       = 65050
    $BGPPeerIP5    = "10.52.255.254"

Mõned asjad, mida kohtvõrgu lüüsi parameetrite kohta:

- Lüüsi kohalikus võrgus võib olla samas või mõnes muus asukohas ja ressursirühma nimega VPN-lüüsi. Järgmises näites kuvatakse need muu ressursi rühmade eri asukohtades olevate inimestega.

- Lüüsi kohtvõrgu deklareerimiseks peate minimaalne eesliide on hosti aadress VPN-seadmes BGP omavahelistes IP-aadressi. Sel juhul on mõne /32 eesliitega "10.52.255.254/32".

- Meeldetuletuseks, peate kasutama erinevaid BGP ASNs oma kohapealse võrgu ja Azure VNet vahel. Kui need on samad, peate muutma oma VNet ASN, kui seadme kohapealse VPN juba kasutada funktsiooni ASN vastastikune muude BGP naabritega.
    
Enne jätkamist, veenduge, et teil on ühendus tellimuse 1.

#### <a name="2-create-the-local-network-gateway-for-site5"></a>2. Site5 kohtvõrgu lüüsi loomine

Kindlasti ressursi rühma luua, kui see on loodud, enne kohtvõrgu lüüsi loomine. Pange tähele kahte täiendavad parameetrid kohtvõrgu lüüsi: Asn ja BgpPeerAddress.

    New-AzureRmResourceGroup -Name $RG5 -Location $Location5

    New-AzureRmLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG5 -Location $Location5 -GatewayIpAddress $LNGIP5 -AddressPrefix $LNGPrefix50 -Asn $LNGASN5 -BgpPeeringAddress $BGPPeerIP5

### <a name="step-2---connect-the-vnet-gateway-and-local-network-gateway"></a>Samm 2 - ühenduse VNet lüüsi ja kohtvõrgu lüüsi

#### <a name="1-get-the-two-gateways"></a>1. kaks lüüside hankimine

        $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
        $lng5gw  = Get-AzureRmLocalNetworkGateway -Name $LNGName5 -ResourceGroupName $RG5

#### <a name="2-create-the-testvnet1-to-site5-connection"></a>2 TestVNet1 Site5 ühenduse loomine

Selles etapis tuleb saavad luua ühenduse kaudu TestVNet1 Site5. Määrake "-EnableBGP $True" BGP selle ühenduse lubamiseks. Nagu varem, on võimalik, et sama Azure'i VPN lüüsi BGP nii BGP ühendused. Kui atribuudi ühendus pole lubatud BGP, lubada Azure ei selle ühenduse BGP, ehkki BGP parameetrid on juba konfigureeritud nii lüüsid.

    New-AzureRmVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng5gw -Location $Location1 -ConnectionType IPsec -SharedKey 'AzureA1b2C3' -EnableBGP $True


Järgmises näites on loetletud parameetrid sisestate BGP konfiguratsiooni jaotisse seadme kohapealse VPN selle ülesande jaoks.

    - Site5 ASN: 65050
    - Site5 BGP IP: 10.52.255.254
    - Eesliidete tähised teatada: (näiteks) 10.51.0.0/16 ja 10.52.0.0/16
    - Azure'i VNet ASN: 65010
    - Azure'i VNet BGP IP: 10.12.255.30
    - Staatilise marsruutimiseks: nexthop VPN tunneliga kasutajaliidese teie seadmes on marsruudi jaoks 10.12.255.30/32, lisamine
    - eBGP Multihop: tagada suvandi "multihop" eBGP on lubatud teie seadmes vajaduse korral

Mõne minuti pärast saa ühendust luua, ja BGP silmitsemine seansi hakkavad pärast selle ühenduse loomist.
 
## <a name ="v2vbgp"></a>Osa 3 - BGP VNet-VNet ühendust luua

Selles jaotises lisab VNet-VNet ühenduse BGP, nagu on näidatud alloleval joonisel. 

![BGP VNet-VNet jaoks](./media/vpn-gateway-bgp-resource-manager-ps/bgp-vnet2vnet.png)

Ülaltoodud eelmisi juhiseid Jätka allpool toodud juhiseid. Peate teostama loomiseks ja konfigureerimiseks TestVNet1 ja VPN-lüüsi koos BGP [I osa](#enablebgp) . 

### <a name="step-1---create-testvnet2-and-the-vpn-gateway"></a>Samm 1 - TestVNet2 ja VPN-lüüsi loomine

See on oluline veenduge, et IP-aadress ruumi uue virtuaalse võrgu TestVNet2, ei kattu oma VNet vahemikke.

Selles näites virtuaalne võrkude kuuluvad sama tellimus. Saate häälestada VNet-VNet seoseid eri tellimuste; Vaadake lisateabe saamiseks [ühenduse VNet-VNet konfigureerimine](./vpn-gateway-vnet-vnet-rm-ps.md) . Veenduge, et lisate selle "-EnableBgp $True" lubamiseks BGP ühenduste loomisel.

#### <a name="1-declare-your-variables"></a>1 tuvastada teie muutujad

Kindlasti need, mida soovite kasutada oma konfiguratsioon väärtuste asendamine.

    $RG2           = "TestBGPRG2"
    $Location2     = "West US"
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
    $GWIPName2     = "VNet2GWIP"
    $GWIPconfName2 = "gwipconf2"
    $Connection21  = "VNet2toVNet1"
    $Connection12  = "VNet1toVNet2"

#### <a name="2-create-testvnet2-in-the-new-resource-group"></a>2 TestVNet2 luua uue ressursirühma

    New-AzureRmResourceGroup -Name $RG2 -Location $Location2
    
    $fesub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName2 -AddressPrefix $FESubPrefix2
    $besub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName2 -AddressPrefix $BESubPrefix2
    $gwsub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName2 -AddressPrefix $GWSubPrefix2

    New-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2 -Location $Location2 -AddressPrefix $VNetPrefix21,$VNetPrefix22 -Subnet $fesub2,$besub2,$gwsub2

#### <a name="3-create-the-vpn-gateway-for-testvnet2-with-bgp-parameters"></a>3. luua VPN-lüüsi TestVNet2 koos BGP parameetrid

Koosolekukutse loote oma VNet lüüsi eraldatav avaliku IP-aadress. Saate määratleda ka alamvõrgu ja nõutud IP konfiguratsioone. 

    $gwpip2    = New-AzureRmPublicIpAddress -Name $GWIPName2 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic

    $vnet2     = Get-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2
    $subnet2   = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet2
    $gwipconf2 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName2 -Subnet $subnet2 -PublicIpAddress $gwpip2

Saate luua VPN-lüüsi AS arv. Pange tähele, et tuleb alistada vaikesäte ASN sisse oma Azure'i VPN lüüsid. Ühendatud VNets jaoks ASNs peavad olema erinevad BGP ja teel marsruutimist.

    New-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2 -Location $Location2 -IpConfigurations $gwipconf2 -GatewayType Vpn -VpnType RouteBased -GatewaySku Standard -Asn $VNet2ASN

### <a name="step-2---connect-the-testvnet1-and-testvnet2-gateways"></a>Samm 2 – TestVNet1 ja TestVNet2 lüüside ühenduse loomine

Selles näites on mõlemad lüüside ühe tellimuse. Seda toimingut saate sama PowerShelli seanss.

#### <a name="1-get-both-gateways"></a>1. nii lüüside hankimine

Veenduge, et saate sisse logida ja luua ühenduse tellimuse 1.

    $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
    $vnet2gw = Get-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2
    
#### <a name="2-create-both-connections"></a>2. nii ühenduste loomine

Selles etapis loote TestVNet1 ühenduse TestVNet2 ja ühenduse kaudu TestVNet2 TestVNet1 abil.

    New-AzureRmVirtualNetworkGatewayConnection -Name $Connection12 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet2gw -Location $Location1 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True

    New-AzureRmVirtualNetworkGatewayConnection -Name $Connection21 -ResourceGroupName $RG2 -VirtualNetworkGateway1 $vnet2gw -VirtualNetworkGateway2 $vnet1gw -Location $Location2 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3' -EnableBgp $True

>[AZURE.IMPORTANT] Kindlasti BGP lubamine mõlemad ühendused.

Pärast lõpetamist neid juhiseid, kuvatakse ühendus luua mõne minuti ja BGP silmitsemine seansi saab sisse VNet-VNet ühendus on lõpule viidud.

Kui olete kõik kolm osa selle, kas olete loonud võrgu topoloogia nagu allpool näidatud:

![BGP VNet-VNet jaoks](./media/vpn-gateway-bgp-resource-manager-ps/bgp-crosspremv2v.png)

## <a name="next-steps"></a>Järgmised sammud

Kui teie ühendus on loodud, saate lisada virtuaalse võrguga virtuaalmasinates. Üksikasjalikud juhised leiate teemast [loomine virtuaalse masina](../virtual-machines/virtual-machines-windows-hero-tutorial.md) .

