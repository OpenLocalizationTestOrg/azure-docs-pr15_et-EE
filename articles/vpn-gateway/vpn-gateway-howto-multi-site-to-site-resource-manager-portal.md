<properties
   pageTitle="Mitme lüüsi saidilt VPN-ühendused lisamine virtuaalse võrgu ressursihaldur juurutamise mudeli Azure'i portaalis | Microsoft Azure'i"
   description="Mitme saidi S2S ühendused lisada VPN-lüüsi, mis sisaldab mõne olemasoleva ühenduse"
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
   ms.date="10/10/2016"
   ms.author="cherylmc"/>



# <a name="add-a-site-to-site-connection-to-a-vnet-with-an-existing-vpn-gateway-connection"></a>On VNet koos olemasoleva ühenduse lüüs VPN-saidilt ühenduse lisamine

> [AZURE.SELECTOR]
- [Ressursihaldur - portaal](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md)
- [Klassikaline - PowerShelli](vpn-gateway-multi-site.md)

Selles artiklis tutvustatakse Azure portaali lisada saidi saidi (S2S) ühendused VPN-lüüsi, mis sisaldab mõne olemasoleva ühenduse abil. Seda tüüpi ühendus on sageli edaspidi "mitme sait" konfigureerimine. 

Selles artiklis abil saate lisada S2S ühenduse VNet, mis on juba S2S ühendus, punkt-ja saidi ühenduse või VNet-VNet ühendus. Ühenduste lisamisel kehtivad teatud piirangud. Kontrollige, [enne alustamist](#before) veenduge enne alustamist, et teie konfiguratsiooni selle artikli jaotisest. 

See artikkel kehtib ressursihaldur juurutamise mudeli abil loodud VNets, mis on RouteBased VPN-lüüsi. ExpressRoute /-saidilt kooseksisteerimisele ühenduse konfiguratsioone ei rakendata järgmisi juhiseid. Kohta leiate teavet [ExpressRoute/S2S kooseksisteerimisele ühendused](../expressroute/expressroute-howto-coexist-resource-manager.md) kooseksisteerimisele ühendused.

### <a name="deployment-models-and-methods"></a>Juurutamise mudelid ja meetodid

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

Uuendame selle tabeli nimega uue artikleid ja täiendavad Tabeliriistad selle konfiguratsiooni puhul. Kui artikkel on saadaval, me lingi otse sellest tabelist.

[AZURE.INCLUDE [vpn-gateway-table-multi-site](../../includes/vpn-gateway-table-multisite-include.md)] 


## <a name="before"></a>Enne alustamist

Kontrollige järgmist:

- Loote mõne ExpressRoute/S2S kooseksisteerimisele ühenduse.
- Teil on virtuaalse võrk, mis on loodud mõne olemasoleva ühenduse ressursihaldur juurutamise mudeli kasutamine.
- Lüüsi virtuaalse võrgu jaoks oma VNet on RouteBased. Kui teil on PolicyBased VPN-lüüsi, peate virtuaalse võrgu lüüsi kustutamine ja luua uue VPN-lüüsi RoutBased nimega.
- Ükski aadresside vahemikud kattuvad mis tahes VNets, mis ühendab selle VNet.
- Teil on seadmega VPN ja keegi, kes saab seda konfigureerida. Lugege [VPN seadmed](vpn-gateway-about-vpn-devices.md). Kui te pole tuttav konfigureerimise seadme VPN, või ei tunne vahemike asub IP-aadress oma kohapealse võrgu konfiguratsiooni, on vaja kellegagi, kes saab anda need andmed teie eest.
- Teil on seadme VPN väliselt suunatud avaliku IP-aadressi. IP-aadress ei leita taga mõnda NAT.


## <a name="part1"></a>Osa 1 - ühenduse konfigureerimine

1. Brauserist, liikuge [Azure portaali](http://portal.azure.com) ja vajadusel logige sisse oma Azure kontoga.
2. Klõpsake **ressurssidele** ja otsige üles oma **virtuaalse võrgu lüüsi** ressursid loendist ja klõpsake seda.
3. Enne **Virtual võrgu lüüsi** , klõpsake nuppu **ühendused**.

    ![Ühenduste blade](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/connectionsblade.png "Connections blade")<br>

4. Enne **ühendused** , klõpsake nuppu **+ Lisa**.

    ![Ühenduse nupp Lisa](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/addbutton.png "Add connection button")<br>

5. **Ühenduse lisamine** enne, täitke järgmised väljad:
    - **Nimi:** Soovite anda saidile loodava ühenduse nimi.
    - **Ühenduse tüüp:** Valige **saidi saidi (IPsec)**.

    ![Lisage ühenduse blade](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/addconnectionblade.png "Add connection blade")<br>

## <a name="part2"></a>Osa 2 - kohtvõrgu lüüsi lisamine

1. Klõpsake nuppu **kohtvõrgu lüüsi** ***valimine lüüsi kohalikus võrgus***. **Valige kohtvõrgu lüüsi** tera avaneb.

    ![Valige kohtvõrgu lüüsi](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/chooselng.png "Choose local network gateway")<br>
2. Klõpsake nuppu **Loo uus** **loomine kohaliku võrgu lüüsi** tera avamiseks.

    ![Kohaliku võrgu lüüsi blade loomine](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/createlngblade.png "Create local network gateway")<br>

3. Enne **loomine kohaliku võrgu lüüsi** , täitke järgmised väljad:
    - **Nimi:** Soovite anda ressursile kohtvõrgu lüüsi nimi.
    - **IP-aadress:** Avaliku IP-aadress VPN seadme saidil, mida soovite ühendada.
    - **Aadress:** Aadressiruumi soovitud marsruutida kohtvõrgu uus sait.
4. **Lüüsi loomine kohtvõrgu** enne muudatuste salvestamiseks klõpsake nuppu **OK** .

## <a name="part3"></a>Osa 3 – lisada ühiskasutusega võti ja ühenduse loomine

1. **Ühenduse lisamine** enne, lisage ühiskasutusega võti, mida soovite oma ühenduse loomiseks kasutada. Võite saada ühiskasutusega võti VPN seadmest või ühe siia ja konfigureerimist seadme VPN sama ühiskasutusega võtit kasutada. Oluline on see, et võtmed on täpselt ühesugused.

    ![Ühiskasutusega võti](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/sharedkey.png "Shared key")<br>
2. Tera allosas nuppu **OK** , et luua ühendus.

## <a name="part4"></a>Osa 4 - kinnitamine VPN-ühendus

Saate kontrollida oma VPN-ühendus portaalis või PowerShelli abil.

[AZURE.INCLUDE [vpn-gateway-verify-connection-rm](../../includes/vpn-gateway-verify-connection-rm-include.md)]


## <a name="next-steps"></a>Järgmised sammud

- Kui teie ühendus on loodud, saate lisada virtuaalse võrguga virtuaalmasinates. Vaadake selle virtuaalmasinates [õppeteema](https://azure.microsoft.com/documentation/learning-paths/virtual-machines) lisateavet.
