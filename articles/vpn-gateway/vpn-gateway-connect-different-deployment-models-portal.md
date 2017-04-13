<properties 
   pageTitle="Klassikaline virtuaalne võrkude ühendamiseks ressursihaldur virtuaalne võrkude portaalis | Microsoft Azure'i"
   description="Saate teada, kuidas klassikaline VNets ja ressursihaldur VNets VPN-lüüsi ja portaali kasutamise vahel VPN-ühenduse loomiseks"
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management,azure-resource-manager"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/03/2016"
   ms.author="cherylmc" />

# <a name="connect-virtual-networks-from-different-deployment-models-in-the-portal"></a>Ühenduse loomine virtuaalse võrgu eri juurutamise mudelid portaalis

> [AZURE.SELECTOR]
- [Portaal](vpn-gateway-connect-different-deployment-models-portal.md)
- [PowerShelli](vpn-gateway-connect-different-deployment-models-powershell.md)


Azure'i praegu on kaks juhtimine mudelit: klassikaline ja ressursside Manager (RM). Kui te kasutate Azure juba mõnda aega, siis ilmselt on Azure VMs ja eksemplari rollide klassikaline VNet töötab. Teie uuem VMs ja rolli aknad võib töötama VNet, mis on loodud rakenduses ressursihaldur. Selles artiklis tutvustatakse klassikaline VNets ühenduse ressursihaldur VNets lubamiseks ressursse, mis asub eraldi juurutamise mudelid kaudu ühenduse lüüs omavahel suhelda. 

Saate luua ühenduse VNets, mis on erinevate tellimused ja erinevate piirkondade vahel. Samuti saate ühendada VNets, mis on juba ühenduste kohapealse võrgu, peaasi, et lüüsi, et need on konfigureeritud on dünaamiline või marsruutimiseks. VNet-VNet ühenduste kohta lisateabe saamiseks vt käesoleva artikli lõpus [VNet-VNet FAQ](#faq) .

### <a name="deployment-models-and-methods-for-vnet-to-vnet-connections"></a>Juurutamise mudelite ja VNet-VNet ühendused meetodid

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

Uuendame järgmises tabelis, kui uus ja täiendavad Tabeliriistad selle konfiguratsiooni puhul. Kui artikkel on saadaval, me lingi otse tabelist.<br><br>

**VNet-VNet**

[AZURE.INCLUDE [vpn-gateway-table-vnet-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)] 

#### <a name="vnet-peering"></a>VNet silmitsemine

[AZURE.INCLUDE [vpn-gateway-vnetpeeringlink](../../includes/vpn-gateway-vnetpeeringlink-include.md)]

## <a name="before-beginning"></a>Enne alustamist

Järgmised toimingud sõelub dünaamiline või marsruutimiseks lüüsi konfigureerimine iga VNet ja lüüside vahel VPN-ühenduse loomiseks vajalikud sätted. Selle konfiguratsiooni ei toeta staatiline või poliitika lüüsid. 

Selles artiklis kasutame klassikaline portaali, Azure portaali ja PowerShelli. Praegu pole võimalik luua selle konfiguratsiooni ainult Azure'i portaalis.

### <a name="prerequisites"></a>Eeltingimused

 - Mõlemad VNets on juba loodud.
 - Aadresside vahemike jaoks soovitud VNets kattuvad üksteisega või mõnda muud ühendused, mis võivad seotud lüüside vahemikud kattuvad.
 - Olete installinud uusimaid PowerShelli cmdlet-käskude (1.0.2 või uuem versioon). Lisateabe saamiseks vaadake [, kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md) . Veenduge, et installida nii teenuse haldus (SM) ja ressursside Manager (RM) cmdlet-käsud. 

### <a name="values"></a>Näide sätted

Näide seadistada abimaterjalina.

**Klassikaline VNet sätted**

VNet nimi = ClassicVNet <br>
Asukoht = Lääne USA. <br>
Virtuaalse võrgu aadress tühikute = 10.0.0.0/24 <br>
Alamvõrgu-1 = 10.0.0.0/27 <br>
GatewaySubnet = 10.0.0.32/29 <br>
Kohaliku Network Name = RMVNetLocal <br>

**Ressursihaldur VNet sätted**

VNet nimi = RMVNet <br>
Ressursirühm = RG1 <br>
Virtuaalse võrgu IP Address tühikuid = 192.168.0.0/16 <br>
Alamvõrgu-1 = 192.168.1.0/24 <br>
GatewaySubnet = 192.168.0.0/26 <br>
Asukoht = Ida-USA <br>
Virtuaalne võrgu lüüsi nimi = RMGateway <br>
Lüüsi avaliku IP nimi = gwpip <br>
Lüüsi tüüp = VPN <br>
VPN-tüüp = marsruutimiseks vastavalt <br>
Kohaliku võrgu lüüsi = ClassicVNetLocal <br>

## <a name="createsmgw"></a>Jaotis 1: Klassikaline VNet sätete konfigureerimine

Selles jaotises loome kohalikus võrgus ja lüüsi oma klassikaline VNet. Selle jaotise juhiseid kasutada klassikaline portaali. Praegu ei paku Azure portaali sätted, mis on seotud klassikaline VNet.

### <a name="part-1---create-a-new-local-network"></a>Osa 1 - kohaliku uue võrgu loomine

Avage [klassikaline portaali](https://manage.windowsazure.com) ja logige sisse oma Azure kontoga.

1. Klõpsake Kuva alumises vasakus nurgas nuppu **Uus** > **Võrguteenuste** > **Virtuaalse võrgu** > **Lisa kohalikus võrgus**.

2. Tippige aknas **saate määrata oma kohalikus võrgus üksikasjad** nimi RM VNet, mida soovite ühendada. Tippige väljale **VPN seadme IP-aadress (valikuline)** mis tahes kehtiv avaliku IP-aadress. See on ainult ajutine kohatäite. IP-aadress hiljem muuta. Klõpsake akna paremas alanurgas olevat noolenuppu.
 
3. Lehel **Määrake aadress ruumi** **Alates IP** tekstiväljale Tippige võrgu eesliite ja CIDR Blokeeri ressursihaldur VNet, mida soovite ühendada. Selle sätte abil määrata aadressiruumi RM VNet marsruutimiseks.

### <a name="part-2---associate-the-local-network-to-your-vnet"></a>Osa 2 - kohalikus võrgus, et teie VNet seostada.

1. **Virtuaalne võrkude** lehe ülaosas virtuaalne võrkude kuvale liikumiseks klõpsake nuppu ja seejärel märkige oma klassikaline VNet. Klõpsake lehel teie VNet **konfigureerimine** konfiguratsiooni lehele liikumiseks.

2. Jaotises **saidilt ühenduvuse** ühendus, märkige ruut **Loo ühendus kohaliku võrgu** . Seejärel valige loodud kohalikus võrgus. Kui teil on mitu kohalikku võrku, loodud, kindlasti valige rippmenüüst oma ressursihaldur VNet esindamiseks loodud.

3. Klõpsake lehe allosas **salvestada** .

### <a name="part-3---create-the-gateway"></a>Osa 3 - lüüsi loomine

1. Pärast salvestamist sätted, klõpsake käsku **armatuurlaua** lehe muuta lehe ülaosas. Armatuurlaua lehe allservas **Lüüsi loomiseks**klõpsake **Dünaamiline marsruutimist**. Klõpsake nuppu **Jah** lüüsi loomise alustamiseks. Selle konfiguratsiooni puhul nõutakse dünaamiline marsruutimine lüüsi.

2. Oodake, kuni lüüsi luua. See võib mõnikord võtta 45 minutit või rohkem valmis.

### <a name="ip"></a>Osa 4 - lüüsi avaliku IP-aadressi kuvamine

Pärast lüüsi on loodud, saate vaadata lehel **armatuurlaua** gateway IP-aadress. See on teie lüüsi avaliku IP-aadress. Kirjutage üles või kopeerige avaliku IP-aadressi. Kasutate selle hiljem juhiseid konfiguratsioonist ressursihaldur VNet kohalikus võrgus loomisel.


## <a name="creatermgw"></a>Jaotis 2: Ressursihaldur VNet sätete konfigureerimine

Selles jaotises loome virtuaalse võrgu lüüsi ja kohalikus võrgus oma ressursihaldur VNet. Ei alga kuni järgmised toimingud pärast klassikaline VNet lüüsi avaliku IP-aadress.

Pildid on toodud näited. Ärge unustage asendage väärtused ise. Kui loote selle konfiguratsiooni kasutab, vaadake need [väärtused](#values).


### <a name="part-1---create-a-gateway-subnet"></a>Osa 1 - lüüsi alamvõrgu loomine

Enne virtuaalse võrgu ühenduse lüüs, peate esmalt lüüsi alamvõrgu virtuaalse võrgu, millele, millega soovite ühenduse luua. Luua lüüsi alamvõrgu CIDR count /28 või suurem (/ 27, / 26, jne.)

[AZURE.INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)]

Brauserist, liikuge [Azure portaali](http://portal.azure.com) ja Azure kontoga sisselogimine.

[AZURE.INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

### <a name="part-2---create-a-virtual-network-gateway"></a>Osa 2 - virtuaalse võrgu lüüsi loomine


[AZURE.INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]


### <a name="part-3---create-a-local-network-gateway"></a>Osa 3 - kohtvõrgu lüüsi loomine

Selle kohalikus võrgus lüüsi viitab tavaliselt oma kohapealse asukoht. See ütleb Azure'i IP-aadress ulatub marsruutimiseks asukoha ja seadme selle asukoha jaoks avaliku IP-aadress. Sel juhul see viitab aga aadress vahemik ja seotud oma klassikaline VNet ja virtuaalse võrgu lüüsi avaliku IP-aadressi.

Pange kohtvõrgu lüüsi nimi, mille Azure saate viidata. Saate luua oma kohalikus võrgus lüüsi ajal virtuaalse võrgu lüüsi loomist. Selle konfiguratsiooni puhul saate kasutada avaliku IP-aadressi määratud klassikaline VNet lüüsi [eelmisest jaotisest](#ip).

[AZURE.INCLUDE [vpn-gateway-add-lng-rm-portal](../../includes/vpn-gateway-add-lng-rm-portal-include.md)]


### <a name="part-4---copy-the-public-ip-address"></a>Osa 4 - avaliku IP-aadressi kopeerimine

Kui virtuaalse võrgu lüüsi on lõpule jõudnud, loomise, kopeerige avaliku IP-aadress, mis on seostatud lüüsi. Saate kasutada seda, kui saate konfigureerida oma klassikaline VNet Kohtvõrgu sätted. 


## <a name="section-3-modify-the-local-network-for-the-classic-vnet"></a>Jaotis 3: Klassikaline VNet muuta kohalikus võrgus

Avage [klassikaline portaalis](https://manage.windowsazure.com).

2. Klassikaline portaali vasakus servas, liikuge kerides allapoole ja klõpsake nuppu **võrgu**. Klõpsake lehel **võrkude** **Kohaliku võrgu** lehe ülaosas. 

3. Klõpsake kohalikus võrgus konfigureeritud osa 1. Klõpsake lehe allosas nuppu **Redigeeri**.

4. Klõpsake lehel **teie kohalikus võrgus üksikasjad** asendada ressursihaldur lüüsi eelmises jaotises loodud avaliku IP-aadress kohatäite IP-aadress. Klõpsake noolt liikumiseks järgmise jaotise juurde. Veenduge, et **Aadressiruumi** oleks õige ja klõpsake muudatuste kinnitamiseks märke.

## <a name="connect"></a>Jaotises 4: Ühenduse loomine

Selles jaotises saame luua selle VNets vahelise ühenduse. Juhised selle jaoks on vaja PowerShelli. Ei saa luua sellega kas portaalide. Veenduge, et alla laadinud ja installinud klassikaline (SM) ja ressursside Manager (RM) PowerShelli cmdlet-käsud.


1. Logige sisse oma Azure'i konto PowerShelli konsooli. Järgmine cmdlet-käsk küsib teilt sisselogimise Azure'i kontosse. Pärast sisselogimist, laaditakse teie konto sätted, et need on saadaval Azure PowerShelli.

        Login-AzureRmAccount 

    Azure'i tellimuste loendi hankimine, kui teil on mitu tellimust.

        Get-AzureRmSubscription

    Määrake tellimus, mida soovite kasutada. 

        Select-AzureRmSubscription -SubscriptionName "Name of subscription"


2. Azure'i konto klassikaline PowerShelli cmdlet-käskude lisamine. Selleks saate kasutada järgmine käsk:

        Add-AzureAccount

3. Määrata ühiskasutusega tootevõti, käitades Järgmine näidis. Selles näites `-VNetName` on klassikaline VNet nimi ja `-LocalNetworkSiteName` on teie määratud kohalikus võrgus, kui olete konfigureerinud selle klassikalise portaalis nimi. Funktsiooni `-SharedKey` on väärtus, mille saate luua ja määrata. Siin saate määrata väärtus peab olema sama väärtuse, kus saate määrata järgmise juhise juurde ühenduse loomisel.

        Set-AzureVNetGatewayKey -VNetName ClassicVNet `
        -LocalNetworkSiteName RMVNetLocal -SharedKey abc123

4. Looge VPN-ühendus, käitades järgmised käsud:
    
    **Määrake muutujad**

        $vnet01gateway = Get-AzureRMLocalNetworkGateway -Name ClassicVNetLocal -ResourceGroupName RG1
        $vnet02gateway = Get-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName RG1

    **Ühenduse loomine**<br> Pange tähele, et selle `-ConnectionType` on IPsec, mitte "Vnet2Vnet". Selles näites `-Name` on nimi, mida soovite helistada ühendust. Järgmine näidis loob ühenduse nimega "*rm-klassikaline-ühendus*".
        
        New-AzureRmVirtualNetworkGatewayConnection -Name rm-to-classic-connection -ResourceGroupName RG1 `
        -Location "East US" -VirtualNetworkGateway1 `
        $vnet02gateway -LocalNetworkGateway2 `
        $vnet01gateway -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'

## <a name="verify-your-connection"></a>Veenduge, et teie ühendus

Klassikaline portaalis Azure portaali või PowerShelli abil saate kontrollida ühenduse. Järgmiste juhiste abil saate kontrollida ühenduse. Asendage väärtused ise.

[AZURE.INCLUDE [vpn-gateway-verify connection](../../includes/vpn-gateway-verify-connection-rm-include.md)] 

## <a name="faq"></a>VNet-VNet KKK

FAQ andmete VNet-VNet ühenduste kohta lisateabe vaatamiseks.

[AZURE.INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)] 


