Peate looma on VNet ja lüüsi alamvõrgu kõigepealt enne kallal järgmisi toiminguid. Lugege artiklit [konfigureerimine virtuaalse võrgu klassikaline portaalis](../articles/expressroute/expressroute-howto-vnet-portal-classic.md) lisateavet.   

## <a name="add-a-gateway"></a>Lüüsi lisamine

Lüüsi loomiseks kasutada käsu alla. Kindlasti asendada mis tahes väärtused ise.

    New-AzureVirtualNetworkGateway -VNetName "MyAzureVNET" -GatewayName "ERGateway" -GatewayType Dedicated -GatewaySKU  Standard

## <a name="verify-the-gateway-was-created"></a>Veenduge, et lüüsi on loodud

Allpool käsu abil saate kontrollida, kas lüüsi luuakse. See käsk toob ka lüüsi ID, mida te ei vaja muude toimingute jaoks.

    Get-AzureVirtualNetworkGateway

## <a name="resize-a-gateway"></a>Lüüsi suuruse muutmine

On mitmeid määratavaid [Lüüsi SKU-de jaoks](../articles/expressroute/expressroute-about-virtual-network-gateways.md). Järgmise käsu abil saate lüüsi SKU igal ajal muuta.

>[AZURE.IMPORTANT] See käsk ei tööta UltraPerformance lüüsi. Portaal on UltraPerformance lüüsi muutmiseks eemaldage esmalt olemasoleva ExpressRoute lüüsi, ja seejärel looge uus UltraPerformance lüüsi. Alandada mõne UltraPerformance Gateway lüüsi, eemaldage esmalt UltraPerformance lüüsi ja seejärel looge uus lüüs. 

    Resize-AzureVirtualNetworkGateway -GatewayId <Gateway ID> -GatewaySKU HighPerformance

## <a name="remove-a-gateway"></a>Lüüsi eemaldamine

Käsuga allpool lüüsi eemaldamine

    Remove-AzureVirtualNetworkGateway -GatewayId <Gateway ID>