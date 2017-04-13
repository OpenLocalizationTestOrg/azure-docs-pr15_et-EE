<properties
   pageTitle="Muuta kohtvõrgu gateway IP address eesliiteid ja gateway IP | Microsoft Azure'i"
   description="Selles artiklis tutvustatakse IP address eesliiteid teie kohalikus võrgus Gateway muutmine"
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
   ms.date="08/08/2016"
   ms.author="cherylmc"/>

# <a name="modify-local-network-gateway-settings-using-powershell"></a>PowerShelli kasutamine kohtvõrgu lüüsi sätete muutmine

Mõnikord teie kohalikus võrgus lüüsi AddressPrefix või GatewayIPAddress sätteid muuta. Järgmised juhised aitavad teil teie kohalikus võrgus lüüsi sätete muutmine. Samuti saate muuta neid sätteid Azure'i portaalis.

## <a name="before-you-begin"></a>Enne alustamist
    
Azure'i ressursihaldur PowerShelli cmdlet-käskude uusima versiooni installimiseks peate. Vaadake, [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md) installimise PowerShelli cmdlet-käskude kohta lisateavet.

## <a name="to-modify-ip-address-prefixes"></a>IP-aadressi muutmiseks eesliidete

[AZURE.INCLUDE [vpn-gateway-modify-ip-prefix-rm](../../includes/vpn-gateway-modify-ip-prefix-rm-include.md)]

## <a name="to-modify-the-gateway-ip-address"></a>Lüüsi IP-aadressi muutmiseks

[AZURE.INCLUDE [vpn-gateway-modify-lng-gateway-ip-rm](../../includes/vpn-gateway-modify-lng-gateway-ip-rm-include.md)]

## <a name="next-steps"></a>Järgmised sammud

Saate kontrollida ühenduse lüüs. Teemast [kinnitamine ühenduse lüüs](vpn-gateway-verify-connection-resource-manager.md).

