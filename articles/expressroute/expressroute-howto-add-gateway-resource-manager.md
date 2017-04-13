<properties
   pageTitle="Ressursihaldur ja PowerShelli abil ExpressRoute VNet lüüsi virtuaalse võrku lisamine | Microsoft Azure'i"
   description="Selles artiklis tutvustatakse Vnet lüüsi on juba loodud ressursihaldur VNet jaoks ExpressRoute lisamine"
   documentationCenter="na"
   services="expressroute"
   authors="charwen"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>

<tags 
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services" 
   ms.date="10/10/2016"
   ms.author="charwen"/>

# <a name="configure-a-virtual-network-gateway-for-expressroute-using-resource-manager-and-powershell"></a>Ressursihaldur ja PowerShelli abil ExpressRoute virtuaalse võrgu lüüsi konfigureerimine


> [AZURE.SELECTOR]
- [PowerShelli - ressursihaldur](expressroute-howto-add-gateway-resource-manager.md)
- [PowerShelli – klassikaline](expressroute-howto-add-gateway-classic.md)


Selles artiklis juhendab teid lisamiseks, suuruse muutmine ja eemaldamine virtuaalse võrgu (VNet) lüüsi olemasoleva VNet juhiseid. Juhised selle konfiguratsiooni spetsiaalselt VNets, mis on loodud rakendust **ressursihaldur juurutamise mudeli** ja see kasutada ExpressRoute konfigureerimisel. 

**Azure'i juurutamise mudelite kohta**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="before-beginning"></a>Enne alustamist

Veenduge, et olete installinud selle konfiguratsiooni jaoks vajalik Azure PowerShelli cmdlet-käskude (1.0.2 või uuem versioon). Kui teil pole installitud cmdlet-käsud, peate tegema enne konfigureerimise juhised. Azure'i PowerShelli installimise kohta lisateabe saamiseks vaadake, [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md).


[AZURE.INCLUDE [expressroute-gateway-rm-ps](../../includes/expressroute-gateway-rm-ps-include.md)]

    
## <a name="next-steps"></a>Järgmised sammud

Kui olete loonud VNet lüüsi, saate oma VNet linkida mõne ExpressRoute ringi. Vaadake [Link on ExpressRoute ringi virtuaalse võrgus](expressroute-howto-linkvnet-arm.md).
