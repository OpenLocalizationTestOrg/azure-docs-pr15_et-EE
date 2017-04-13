<properties 
   pageTitle="Virtuaalse võrgu avaliku IP-aadresside kasutamine"
   description="Siit saate teada, kuidas konfigureerida virtuaalse võrgu kasutada avaliku IP-aadressid"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="04/27/2016"
   ms.author="jdial" />

# <a name="public-ip-address-space-in-a-virtual-network-vnet"></a>Avaliku IP-aadress ruumi virtuaalse võrku (VNet)

Virtuaalne võrkude (VNets) võib olla nii avalike ja privaatvõ (RFC 1918 aadress blocks) IP address tühikuid. Kui lisate avaliku IP-aadresside vahemiku, käsitletakse privaatne VNet IP-aadress ruumi, mis on kättesaadav VNet, omavahel seotud VNets, ja teie asutusesisese asukohast ainult osa.

Alloleval pildil on VNet, mis sisaldab avalike ja privaatvõ IP-aadress tühikuid.

![Avaliku IP kontseptuaalne](./media/virtual-networks-public-ip-within-vnet/IC775683.jpg)

## <a name="how-do-i-add-a-public-ip-address-range"></a>Kuidas lisada avaliku IP-aadresside vahemiku?

Lisage avaliku IP-aadresside vahemiku samal viisil lisamist privaatne IP address vahemiku; faili *netcfg* abil või lisage konfiguratsiooni [Azure portaali](http://portal.azure.com). Kui loote oma VNet või saate tagasi minna ja lisada selle hiljem saate lisada avaliku IP-aadresside vahemiku. Järgmises näites on kuvatud avalike ja privaatvõ IP address tühikuid sama VNet konfigureeritud.

![Avaliku IP-aadressi portaalis](./media/virtual-networks-public-ip-within-vnet/IC775684.png)

## <a name="are-there-any-limitations"></a>Kas on mingeid piiranguid?

On paar IP-aadresside vahemikud, mis ei ole lubatud:

- 224.0.0.0/4 (multisaate)

- 255.255.255.255/32 (leviedastus)

- 127.0.0.0/8 (loopback)

- 169.254.0.0/16 (link-local)

- 168.63.129.16/32 (sisemise DNS-i)

## <a name="next-steps"></a>Järgmised sammud

[DNS-i serverid, mida on VNet haldamine](virtual-networks-manage-dns-in-vnet.md)