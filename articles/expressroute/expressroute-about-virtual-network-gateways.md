<properties 
   pageTitle="ExpressRoute virtuaalse võrgu lüüside kohta | Microsoft Azure'i"
   description="Teave virtuaalse võrgu lüüside ExpressRoute."
   services="expressroute"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager, azure-service-management"/>
<tags 
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/03/2016"
   ms.author="cherylmc" />

# <a name="about-virtual-network-gateways-for-expressroute"></a>Virtuaalse võrgu lüüside jaoks ExpressRoute kohta


Virtuaalse võrgu lüüsi saatmiseks võrguliiklust vahel Azure'i ja kohapealsete asukohad. Kui konfigureerite mõne ExpressRoute ühenduse, peate looma ja virtuaalse võrgu lüüsi ja virtuaalse võrguühenduse lüüsi konfigureerimine.

Kui loote virtuaalse võrgu lüüsi, saate määrata mitu sätteid. Üks nõutav sätted saate määrata, kas lüüsi kasutatakse ExpressRoute või -saidilt VPN-liikluse. Ressursihaldur juurutamise mudeli, milles on "-GatewayType".

Võrguliikluse saatmisel sihtotstarbeline privaatne ühenduse kasutate lüüsi tüüp "ExpressRoute". See on ka edaspidi mõne ExpressRoute lüüsi. Kui võrguliiklust saadetakse krüptitud avaliku Internetis, kasutage lüüsi tüüp "Vpn". See on edaspidi VPN-lüüsi. Saidi saidi, punkt-saidi ja VNet-VNet ühendused kõik kasutada VPN-lüüsi. 

Iga virtuaalse võrgu võib olla ainult üks virtuaalse võrgu lüüsi lüüsi tüüp. Näiteks saate määrata ühe virtuaalse võrgu - GatewayType Vpn kasutava lüüsi ja üks, mis kasutab - GatewayType ExpressRoute. See artikkel keskendub ExpressRoute virtuaalse võrgu lüüsi.

## <a name="gwsku"></a>Lüüsi SKU-de jaoks

[AZURE.INCLUDE [expressroute-gwsku-include](../../includes/expressroute-gwsku-include.md)]

Kui soovite oma võimsam lüüsi SKU lüüsi täiendada, enamikul juhtudel saate kasutada "Suuruse muutmise AzureRmVirtualNetworkGateway" PowerShelli cmdlet-käsk. See töötab uuendamine Standard ja HighPerformance SKU-de jaoks. Siiski uuendada UltraPerformance SKU-ga, peate lüüsi uuesti luua.

###  <a name="aggthroughput"></a>Hinnanguline liitväärtuse läbilaskevõime, lüüsi SKU-ga


Järgmises tabelis on lüüsi tüübid ja hinnanguline liitväärtuse läbilaskevõime. Selles tabelis kehtib ressursihaldur ja klassikaline juurutamise mudelid.

[AZURE.INCLUDE [expressroute-table-aggthroughput](../../includes/expressroute-table-aggtput-include.md)] 


## <a name="resources"></a>REST API-d ja PowerShelli cmdletid

Lisaressursid tehniline ja teatud süntaksi nõuded virtuaalse võrgu lüüsi konfiguratsioone REST API-d ja PowerShelli cmdlet-käskude kasutamisel leiate teemast järgmistel lehtedel:

|**Klassikaline** | **Ressursihaldur**|
|-----|----|
|[PowerShelli](https://msdn.microsoft.com/library/mt270335.aspx)|[PowerShelli](https://msdn.microsoft.com/library/mt163510.aspx)|
|[REST API-GA](https://msdn.microsoft.com/library/jj154113.aspx)|[REST API-GA](https://msdn.microsoft.com/library/mt163859.aspx)|


## <a name="next-steps"></a>Järgmised sammud

Vt Lisateavet saadaolevate ühenduse konfiguratsioone [ExpressRoute ülevaade](expressroute-introduction.md) . 







 
