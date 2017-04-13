<properties
   pageTitle="Lähtesta on Azure VPN-lüüsi | Microsoft Azure'i"
   description="Selles artiklis tutvustatakse Azure'i VPN-lüüsi lähtestamisel. See artikkel kehtib VPN lüüside klassikaline ja ressursihaldur juurutamise mudelite."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager,azure-service-management"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/23/2016"
   ms.author="cherylmc"/>

# <a name="reset-an-azure-vpn-gateway-using-powershell"></a>Azure'i VPN lüüsi PowerShelli kaudu lähtestamine


Selles artiklis tutvustatakse lähtestamise Azure'i VPN lüüsi PowerShelli cmdlet-käskude abil. Need juhised kaasata nii klassikaline juurutamise mudeli ja ressursihaldur juurutamise mudel.

Azure'i VPN-lüüsi lähtestamisel on kasulik, kui te kaotate asukohaga virtuaalse Privaatvõrgu ühendus ühe või mitme S2S VPN tunnelite kohta. Sellisel juhul oma kohapealse VPN seadmete kõik töötavad õigesti, kuid ei saa luua IPsec tunnelid Azure'i VPN lüüside abil. 

Iga Azure'i VPN-lüüsi koosneb konfigureerimisel aktiivne-puhkerežiimis olev töötab kahel VM. PowerShelli cmdlet-käsu kasutamisel lähtestamiseks lüüsi taaskäivitamisega lüüsi ja seejärel varustuses asutusesiseses kuvatakse selle. Lüüsi hoiab avaliku IP-aadress on juba. See tähendab, et te ei pea värskendada konfiguratsiooni VPN ruuteri uue avaliku IP-aadress Azure'i VPN-lüüsi jaoks.  

Pärast käsu väljastamist taaskäivitatakse praegust aktiivse Azure'i VPN-lüüsi eksemplari kohe. Seal on lühike vahe on tõrkesiire aktiivne eksemplarist (on rebooted), valige eksemplari. Vahe peaks olema väiksem kui üks minut.

Kui ühendus ei taastatakse esimese taaskäivitage arvuti pärast, probleemi sama käsk uuesti taaskäivitage teise VM eksemplari (uus aktiivne lüüsi). Kui kaks taaskäivitamisega juurde uuesti tagasi, tekib veidi pikem kui nii VM juhtudel (aktiivne ja valige) on rebooted. See põhjustab pikema vahe virtuaalse Privaatvõrgu ühendus, kuni 2 kuni 4 minutit vms on taaskäivitamisega lõpuleviimiseks.

Pärast kahe taaskäivitamisega, kui teil on endiselt probleeme asutusesiseses ühendusprobleemide, palun avada esitada tugiteenuse taotluse Azure portaalist.

## <a name="before-you-begin"></a>Enne alustamist

Lähtestage oma lüüsi, kontrollige jaoks iga IPsec-saidilt (S2S) VPN tunneliga alltoodud võtme üksused. Mis tahes lahknevuse üksused, mis annab tulemuseks S2S VPN tunnelite Katkesta. Kinnitatava ja parandatakse oma kohapealse ja Azure VPN lüüside konfiguratsioone säästab mittevajalike taaskäivitamisega ja katkestuste klõpsake lüüside töötamine teiste ühenduste jaoks.

Veenduge enne lähtestamist lüüsi järgmised üksused:

- Interneti-IP-aadresside (VIP) lüüsi Azure'i VPN-ja kohapealse VPN-lüüsi on õigesti konfigureeritud nii on Azure ja kohapealse VPN poliitika.
- Eelnevalt jagatud võti peab olema sama nii Azure'i ja kohapealsete VPN lüüsid.
- Kui rakendate teatud IPsec/IKE konfiguratsiooni, nt krüptimist, räsi algoritmide ja PFS (täiuslik edasi hoidmise), tagada nii Azure'i ja kohapealsete VPN lüüsid olema sama konfiguratsioone.

## <a name="reset-a-vpn-gateway-using-the-resource-management-deployment-model"></a>Lähtesta VPN-lüüsi ressursihalduse juurutamise mudeli kasutamine

Ressursihaldur PowerShelli cmdlet lähtestamise lüüsi jaoks on `Reset-AzureRmVirtualNetworkGateway`. Järgmises näites lähtestab Azure'i VPN lüüsi, "VNet1GW" ressursirühm "TestRG1".

    $gw = Get-AzureRmVirtualNetworkGateway -Name VNet1GW -ResourceGroup TestRG1
    Reset-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw

## <a name="reset-a-vpn-gateway-using-the-classic-deployment-model"></a>Lähtesta VPN-lüüsi klassikaline juurutamise mudeli kasutamine

PowerShelli cmdleti lähtestamise Azure'i VPN-lüüsi jaoks on `Reset-AzureVNetGateway`. Järgmises näites lähtestab Azure'i VPN-lüüsi virtuaalse võrgu nimega "ContosoVNet".
 
    Reset-AzureVNetGateway –VnetName “ContosoVNet” 

Tulem:

    Error          :
    HttpStatusCode : OK
    Id             : f1600632-c819-4b2f-ac0e-f4126bec1ff8
    Status         : Successful
    RequestId      : 9ca273de2c4d01e986480ce1ffa4d6d9
    StatusCode     : OK


## <a name="next-steps"></a>Järgmised sammud
    
Lugege teemat [Teenusehaldus PowerShelli cmdleti viide](https://msdn.microsoft.com/library/azure/mt617104.aspx) ja Lisateavet [ressursihaldur PowerShelli cmdleti viide](http://go.microsoft.com/fwlink/?LinkId=828732) .






