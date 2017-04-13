<properties 
   pageTitle="Jõustatud tunneling saidilt ühenduste abil ressursihaldur juurutamise mudeli konfigureerimine | Microsoft Azure'i"
   description="Kuidas redirect või "jõustada kõik Interneti seotud liikluse oma kohapealse võrra tagasi."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/10/2016"
   ms.author="cherylmc" />

# <a name="configure-forced-tunneling-using-the-azure-resource-manager-deployment-model"></a>Jõustatud tunneling Azure'i ressursihaldur juurutamise näidise konfigureerimine

> [AZURE.SELECTOR]
- [PowerShelli – klassikaline](vpn-gateway-about-forced-tunneling.md)
- [PowerShelli - ressursihaldur](vpn-gateway-forced-tunneling-rm.md)

Jõustatud tunneling võimaldab teil redirect või "Jõusta" kõik Interneti seotud liikluse tagasi oma kohapealse asukohta VPN saidilt tunneliga ülevaatuse ja auditeerimine kaudu. See on kriitiliste turvalisus nõue enamik ettevõtte IT poliitika.

Jõustatud tunneling ilma Interneti seotud liiklus teie VMs Azure on alati läbida: Azure'i võrgu taristule otse välja Interneti-ühendus, ilma suvandi abil saate kontrollida või kontrollida liiklus. Volitamata Interneti-ühendus võib põhjustada potentsiaalselt teabe või muud tüüpi turvalisus seotud rikkumiste eest.

Selles artiklis tutvustatakse konfigureerimise sunnitud tunneling virtuaalse võrkude ressursihaldur juurutamise mudeli abil loodud.

**Azure'i juurutamise mudelite kohta**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

**Juurutamise mudelite ja jõustatud tunneling tööriistad**

Jõustatud tunnelite ühenduse saab konfigureerida nii klassikaline juurutamise mudeli ja ressursihaldur juurutamise mudel. Vt lisateavet järgmises tabelis. Uuendame see tabel, kui selle konfiguratsiooni saadaval uus artikleid, uusi juurutamise mudeleid ja täiendavad tööriistad. Kui artikkel on saadaval, me lingi otse tabelist.

[AZURE.INCLUDE [vpn-gateway-table-forced-tunneling](../../includes/vpn-gateway-table-forcedtunnel-include.md)] 


## <a name="about-forced-tunneling"></a>Teave sunnitud tunneling


Järgmisel joonisel on näidatud, kuidas jõustatud tunnelite töötab. 

![Sunnitud Tunneling](./media/vpn-gateway-forced-tunneling-rm/forced-tunnel.png)

Ülaltoodud näites on Frontend alamvõrgu ei joondata tunneled. Töökoormus Frontend alamvõrku, saate jätkata aktsepteerida ja vastata kliendi Interneti kaudu otse. Keskel tasandi ja Taustväärtus alamvõrku sunnitakse tunneled. Väljaminevad ühendused Interneti kaudu nende kahe alamvõrku on sunnitud või ümber tagasi anda kohapealse saidi ühe S2S VPN tunnelite kaudu.

See võimaldab teil piirata ja kontrollida Interneti-ühendus oma virtuaalmasinates või pilveteenused Azure, jätkates lubamiseks oma mitmekihilise teenuse arhitektuur nõutav. Samuti saate rakendada jõustatud tunneling kogu virtuaalse võrkudega kui virtuaalse võrguga puudub Interneti-ühendusega töökoormus.

## <a name="requirements-and-considerations"></a>Nõuded ja kaalutlused

Jõustatud tunneling Azure on konfigureeritud virtuaalse võrgu kasutaja määratletud viisil kaudu. Kohapealse saidi liikluse ümber väljendatud hinnaks dollarites vaikimisi marsruudi Azure'i VPN lüüsiga. Kasutaja määratletud marsruutimine ja virtuaalne võrkude kohta leiate lisateavet teemast [kasutaja määratletud marsruudib ja IP ümbersuunamine](../virtual-network/virtual-networks-udr-overview.md).

- Iga virtuaalse alamvõrku on valmis, süsteemi marsruutimise tabeli. Süsteemi marsruutimise tabelis on marsruudib järgmised kolm rühmad.

    - **Kohaliku VNet marsruudib:** Otse sihtkohta VMs virtuaalse samasse võrku
    
    - **Kohapealse marsruudib:** Azure'i VPN Gateway
    
    - **Vaikimisi marsruudi:** Otse Internetiga. Kõrvaldatakse paketid sinna privaatne IP-aadressid pole hõlmatud eelmise kahel viisil.

-  See toiming kasutab vaikimisi marsruudi lisada, ja seejärel seostada nii marsruutimise tabeli loomiseks oma VNet subnet(s) lubamiseks klõpsake nende alamvõrku jõustatud tunneling kasutaja määratletud marsruudib (UDR).

- Jõustatud tunneling peavad olema seostatud VNet, mis sisaldab marsruutimiseks vastavalt VPN-lüüsi. Peate määrama "vaikimisi saidi" asutusesiseses kohaliku saite virtuaalse võrku ühendatud.

- ExpressRoute sunnitud tunneling kaudu see süsteem pole konfigureeritud, kuid selle asemel on lubanud reklaami vaikimisi marsruudi ExpressRoute BGP silmitsemine seansid kaudu. Lugege lisateavet [ExpressRoute dokumentatsiooni](https://azure.microsoft.com/documentation/services/expressroute/) .

## <a name="configuration-overview"></a>Konfiguratsiooni ülevaade

Järgmiste toimingute abil saate luua ressursirühma ja on VNet. Seejärel saate VPN-lüüsi loomine ja konfigureerimine jõustatud tunneling. Käesolevas näites on virtuaalse võrgu "MultiTier VNet" 3 alamvõrku: *Frontend*, *Midtier*ja *kirjutamata*, 4 asutusesiseses ühendused: *DefaultSiteHQ*ja 3 *harude*.

Toimingu juhiseid seadmine *DefaultSiteHQ* jaoks vaikimisi saidi ühendus sunnitud tunneling ja konfigureerida selle Midtier ja kirjutamata alamvõrku kasutada sunnitud tunneling.

    
## <a name="before-you-begin"></a>Enne alustamist

Veenduge, et on enne konfiguratsioonile järgmist.

- Azure'i tellimuse. Kui teil pole veel Azure tellimuse, saate aktiveerida oma [MSDN-i abonendi kasu](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) või Logi üles [tasuta konto](https://azure.microsoft.com/pricing/free-trial/).

- Peate installige uusim versioon Azure ressursihaldur PowerShelli cmdlet-käskude (1.0 või uuem versioon). Vaadake, [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md) installimise PowerShelli cmdlet-käskude kohta lisateavet.


## <a name="configure-forced-tunneling"></a>Jõustatud tunneling konfigureerimine

1. PowerShelli konsooli, logige sisse oma Azure'i kontosse. Selle cmdlet-käsu küsib teilt sisselogimise Azure'i kontosse. Pärast sisselogimist, allalaadimist Kontosätete nii, et need on saadaval Azure PowerShelli.

        Login-AzureRmAccount 

2. Azure'i tellimuste loendi hankimine.

        Get-AzureRmSubscription

2. Määrake tellimus, mida soovite kasutada. 

        Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
        
3. Ressursi rühma loomine.

        New-AzureRmResourceGroup -Name "ForcedTunneling" -Location "North Europe"

4. Luua virtuaalse võrgu ja määrake alamvõrku. 

        $s1 = New-AzureRmVirtualNetworkSubnetConfig -Name "Frontend" -AddressPrefix "10.1.0.0/24"
        $s2 = New-AzureRmVirtualNetworkSubnetConfig -Name "Midtier" -AddressPrefix "10.1.1.0/24"
        $s3 = New-AzureRmVirtualNetworkSubnetConfig -Name "Backend" -AddressPrefix "10.1.2.0/24"
        $s4 = New-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -AddressPrefix "10.1.200.0/28"
        $vnet = New-AzureRmVirtualNetwork -Name "MultiTier-VNet" -Location "North Europe" -ResourceGroupName "ForcedTunneling" -AddressPrefix "10.1.0.0/16" -Subnet $s1,$s2,$s3,$s4

5. Looge lüüside kohalikus võrgus.

        $lng1 = New-AzureRmLocalNetworkGateway -Name "DefaultSiteHQ" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.111" -AddressPrefix "192.168.1.0/24"
        $lng2 = New-AzureRmLocalNetworkGateway -Name "Branch1" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.112" -AddressPrefix "192.168.2.0/24"
        $lng3 = New-AzureRmLocalNetworkGateway -Name "Branch2" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.113" -AddressPrefix "192.168.3.0/24"
        $lng4 = New-AzureRmLocalNetworkGateway -Name "Branch3" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -GatewayIpAddress "111.111.111.114" -AddressPrefix "192.168.4.0/24"
        
6. Marsruutimiseks tabeli ja marsruutimiseks reeglite loomine.

        New-AzureRmRouteTable –Name "MyRouteTable" -ResourceGroupName "ForcedTunneling" –Location "North Europe"
        $rt = Get-AzureRmRouteTable –Name "MyRouteTable" -ResourceGroupName "ForcedTunneling" 
        Add-AzureRmRouteConfig -Name "DefaultRoute" -AddressPrefix "0.0.0.0/0" -NextHopType VirtualNetworkGateway -RouteTable $rt
        Set-AzureRmRouteTable -RouteTable $rt


7. Midtier ja kirjutamata alamvõrku marsruutimiseks tabeli seostada.

        $vnet = Get-AzureRmVirtualNetwork -Name "MultiTier-Vnet" -ResourceGroupName "ForcedTunneling"
        Set-AzureRmVirtualNetworkSubnetConfig -Name "MidTier" -VirtualNetwork $vnet -AddressPrefix "10.1.1.0/24" -RouteTable $rt
        Set-AzureRmVirtualNetworkSubnetConfig -Name "Backend" -VirtualNetwork $vnet -AddressPrefix "10.1.2.0/24" -RouteTable $rt
        Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

8. Lüüsi loomine vaikimisi saidiga. See toiming võtab aega lõpetamiseks mõnikord 45 minutit või rohkem, kuna loote ja lüüsi konfigureerimise.<br> Funktsiooni `-GatewayDefaultSite` on cmdlet parameeter, mis võimaldab jõustatud marsruutimise konfiguratsioon, töö, nii et olge konfigureerimine seade õigesti. See parameeter on saadaval PowerShelli 1.0 või uuem versioon.

        $pip = New-AzureRmPublicIpAddress -Name "GatewayIP" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -AllocationMethod Dynamic
        $gwsubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet
        $ipconfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name "gwIpConfig" -SubnetId $gwsubnet.Id -PublicIpAddressId $pip.Id
        New-AzureRmVirtualNetworkGateway -Name "Gateway1" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -IpConfigurations $ipconfig -GatewayType Vpn -VpnType RouteBased -GatewayDefaultSite $lng1 -EnableBgp $false

9. Luua-saidilt VPN-ühendused.

        $gateway = Get-AzureRmVirtualNetworkGateway -Name "Gateway1" -ResourceGroupName "ForcedTunneling"
        $lng1 = Get-AzureRmLocalNetworkGateway -Name "DefaultSiteHQ" -ResourceGroupName "ForcedTunneling" 
        $lng2 = Get-AzureRmLocalNetworkGateway -Name "Branch1" -ResourceGroupName "ForcedTunneling" 
        $lng3 = Get-AzureRmLocalNetworkGateway -Name "Branch2" -ResourceGroupName "ForcedTunneling" 
        $lng4 = Get-AzureRmLocalNetworkGateway -Name "Branch3" -ResourceGroupName "ForcedTunneling" 

        New-AzureRmVirtualNetworkGatewayConnection -Name "Connection1" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng1 -ConnectionType IPsec -SharedKey "preSharedKey"
        New-AzureRmVirtualNetworkGatewayConnection -Name "Connection2" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng2 -ConnectionType IPsec -SharedKey "preSharedKey"
        New-AzureRmVirtualNetworkGatewayConnection -Name "Connection3" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng3 -ConnectionType IPsec -SharedKey "preSharedKey"
        New-AzureRmVirtualNetworkGatewayConnection -Name "Connection4" -ResourceGroupName "ForcedTunneling" -Location "North Europe" -VirtualNetworkGateway1 $gateway -LocalNetworkGateway2 $lng4 -ConnectionType IPsec -SharedKey "preSharedKey"

        Get-AzureRmVirtualNetworkGatewayConnection -Name "Connection1" -ResourceGroupName "ForcedTunneling"
        



