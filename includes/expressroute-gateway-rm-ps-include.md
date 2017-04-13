Juhised selle toimingu jaoks kasutage VNet, mis on allpool väärtuste põhjal. Selles loendis on esitatud ka täiendavaid sätteid ja nimed. Me ei kasuta seda loendit otse mis tahes juhiseid, kuigi me selles loendis väärtustel põhineva muutujate lisada. Saate kopeerida loendit, kui soovite kasutada, asendades väärtused ise.

Konfiguratsiooni soovitajate loend:
    
- Virtuaalne võrgu nimi = "TestVNet"
- Võrgu virtuaalse aadressiruumi = 192.168.0.0/16
- Ressursirühm = "TestRG"
- Subnet1 nimi = "FrontEnd" 
- Subnet1 aadressiruumi = "192.168.0.0/16"
- Lüüsi alamvõrgu nimi: "GatewaySubnet" alati peate nime lüüsi alamvõrgu *GatewaySubnet*.
- Lüüsi alamvõrgu aadressiruumi = "192.168.200.0/26"
- Piirkonna = "Ida-USA"
- Lüüsi nimi = "GW"
- Lüüsi IP nimi = "GWIP"
- Lüüsi IP-konfiguratsiooni nimi = "gwipconf"
-  Tüüp = "ExpressRoute" seda tüüpi jaoks on vaja ExpressRoute konfigureerimisel.
- Lüüsi avaliku IP nimi = "gwpip"


## <a name="add-a-gateway"></a>Lüüsi lisamine

1. Ühenduse Azure'i tellimuse. 

        Login-AzureRmAccount
        Get-AzureRmSubscription 
        Select-AzureRmSubscription -SubscriptionName "Name of subscription"

2. Selle ülesande jaoks oma muutujate deklareerida. Selles näites kasutatakse Kasuta muutujate allpool valimis. Kindlasti seda vastavalt sätteid, mida soovite kasutada. 
        
        $RG = "TestRG"
        $Location = "East US"
        $GWName = "GW"
        $GWIPName = "GWIP"
        $GWIPconfName = "gwipconf"
        $VNetName = "TestVNet"

3. Hoida virtuaalse võrgu objekti muutujana.

        $vnet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG

4. Lüüsi alamvõrgu lisada virtuaalse võrgu. Lüüsi alamvõrgu peab nimega "GatewaySubnet". Soovite luua lüüsi, mis on /27 või suurem (/ 26, / 25, jne.).
            
        Add-AzureRmVirtualNetworkSubnetConfig -Name GatewaySubnet -VirtualNetwork $vnet -AddressPrefix 192.168.200.0/26

5. Määrata.

            Set-AzureRmVirtualNetwork -VirtualNetwork $vnet

6. Hoida lüüsi alamvõrgu muutujana.

        $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet

7. Koosolekukutse avaliku IP-aadressi. IP-aadress on nõutav enne lüüsi loomine. Te ei saa määrata IP-aadress, mida soovite kasutada; See on dünaamiliselt eraldatud. Saate kasutada järgmises jaotises konfiguratsiooni IP-aadress. Funktsiooni AllocationMethod peab olema dünaamiline.

        $pip = New-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName $RG -Location $Location -AllocationMethod Dynamic

8. Looge oma lüüsi konfigureerimine. Lüüsi konfigureerimine määratleb alamvõrgu ja kasutada avaliku IP-aadressi. Selles etapis tuleb teil on täpsustades konfiguratsiooni, mida kasutatakse lüüsi loomine. See toiming ei loo tegelikult lüüsi objekti. Allpool valimi abil saate luua oma lüüsi konfigureerimine. 

        $ipconf = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName -Subnet $subnet -PublicIpAddress $pip

9. Lüüsi loomine. Selles etapis tuleb **-GatewayType** on eriti oluline. Kasutage väärtust **ExpressRoute**. Pange tähele, et pärast nende cmdlettide töötab, lüüsi võib võtta 20 minutit või rohkem, et luua.

        New-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG -Location $Location -IpConfigurations $ipconf -GatewayType Expressroute -GatewaySku Standard

## <a name="verify-the-gateway-was-created"></a>Veenduge, et lüüsi on loodud

Allpool käsu abil saate kontrollida, kas lüüsi luuakse.

    Get-AzureRmVirtualNetworkGateway -ResourceGroupName $RG

## <a name="resize-a-gateway"></a>Lüüsi suuruse muutmine

On mitmeid määratavaid [Lüüsi SKU-de jaoks](../articles/expressroute/expressroute-about-virtual-network-gateways.md). Järgmise käsu abil saate lüüsi SKU igal ajal muuta.

>[AZURE.IMPORTANT] See käsk ei tööta UltraPerformance lüüsi. Portaal on UltraPerformance lüüsi muutmiseks eemaldage esmalt olemasoleva ExpressRoute lüüsi, ja seejärel looge uus UltraPerformance lüüsi. Lüüsi on UltraPerformance Gateway alandada, eemaldage esmalt UltraPerformance lüüsi, ja seejärel looge uus lüüs.

    $gw = Get-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
    Resize-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku HighPerformance

## <a name="remove-a-gateway"></a>Lüüsi eemaldamine

Käsuga allpool lüüsi eemaldamine

    Remove-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG  
