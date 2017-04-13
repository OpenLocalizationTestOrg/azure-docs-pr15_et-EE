<properties
   pageTitle="Azure'i VNets kasutajaga VPN-lüüsi ja PowerShelli | Microsoft Azure'i"
   description="Selles artiklis tutvustatakse virtuaalne võrkude ühendamine koos Azure ressursihaldur ja PowerShelli abil."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/31/2016"
   ms.author="cherylmc"/>

# <a name="configure-a-vnet-to-vnet-connection-for-resource-manager-using-powershell"></a>Konfigureerimine VNet-VNet ühenduse jaoks ressursihaldur PowerShelli abil

> [AZURE.SELECTOR]
- [Ressursihaldur - Azure'i portaal](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
- [Ressursihaldur - PowerShelli](vpn-gateway-vnet-vnet-rm-ps.md)
- [Klassikaline – klassikaline portaal](virtual-networks-configure-vnet-to-vnet-connection.md)

Selles artiklis tutvustatakse VNets ressursihaldur juurutamise mudeli abil VPN-lüüsi vahelise ühenduse loomise juhiseid. Virtuaalne võrkude võib olla samas või mõnes muus piirkondade ja samas või mõnes muus tellimused.


![v2v skeem](./media/vpn-gateway-vnet-vnet-rm-ps/v2vrmps.png)

### <a name="deployment-models-and-methods-for-vnet-to-vnet-connections"></a>Juurutamise mudelite ja VNet-VNet ühendused meetodid

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)]

Järgmises tabelis on praegu saadaval juurutamise mudelid ja meetodid VNet-VNet konfiguratsioone. Kui konfigureerimistoimingute kohanimi on saadaval, me lingi otse sellest tabelist.

[AZURE.INCLUDE [vpn-gateway-table-vnet-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)]

#### <a name="vnet-peering"></a>VNet silmitsemine

[AZURE.INCLUDE [vpn-gateway-vnetpeeringlink](../../includes/vpn-gateway-vnetpeeringlink-include.md)]


## <a name="about-vnet-to-vnet-connections"></a>VNet-VNet ühenduste kohta

Võrguga virtuaalse võrgu teise virtuaalse (VNet VNet) on sarnane on VNet ühenduse oma kohapealse saidi asukoha. Mõlemat tüüpi ühenduvuse kasutavad on Azure VPN-lüüsi turvaline tunneliga, kasutades IPsec/IKE. Erinevate piirkondade saab VNets, loote. Või muu tellimused. Saate ühendada isegi VNet-VNet suhtlemine mitme saidi konfiguratsioone. See võimaldab teil luua võrgu topoloogiatest, mis sisaldavad asutusesiseses Ühenduvus omavahel virtuaalse võrguühendus, nagu on näidatud järgmisel joonisel:


![Andmeühenduste kohta](./media/vpn-gateway-vnet-vnet-rm-ps/aboutconnections.png)
 
### <a name="why-connect-virtual-networks"></a>Miks ühenduse, virtuaalse võrkudes?

Kui soovite virtuaalne võrkude ühendamiseks järgmistel põhjustel.

- **Piirkonna geo-koondamise ja geo-kohalolek**
    - Saate häälestada oma geo-kopeerimine või sünkroonimise turvaline Ühenduvus ilma läheb üle Interneti-ühendusega lõpp-punktid.
    - Azure'i liikluse juhataja ja laadi koormusetasakaalustusteenuse, saate häälestada tugevalt saadaval töökoormus geo-koondamise mitme Azure'i piirkondade lõikes. Üks oluline näide on SQL alati kohta kättesaadavus levirühmadega levitada mitme Azure'i piirkondade häälestamiseks.

- **Piirkondliku mitmekihilise rakenduste eraldamise või äärist haldus**
    - Samas piirkonnas, saate häälestada mitmekihilise rakenduste abil mitme virtuaalse võrgu omavahel ühendatud eraldi või haldus nõuete tõttu.


### <a name="vnet-to-vnet-faq"></a>VNet-VNet KKK

[AZURE.INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)] 


## <a name="which-set-of-steps-should-i-use"></a>Millised juhised kasutada?

Selles artiklis näete kahte eri juhiseid. Ühe teemas kirjeldatud toimingutega [VNets, mis asuvad samas tellimus](#samesub)ja teine [VNets, mis ei asu muu tellimuste](#difsub)jaoks. Võtme komplekti vahe on kas saate luua ja konfigureerida kõik virtuaalse võrgu ja lüüsi ressursid sama PowerShelli seansi jooksul.

Selles artiklis toodud juhiseid kasutada muutujat, mis on iga jaotise alguses. Kui teil juba on töötamine olemasoleva VNets, muuta muutujaid vastavalt oma keskkonna sätteid. 

![Mõlemad ühendused](./media/vpn-gateway-vnet-vnet-rm-ps/differentsubscription.png)


## <a name="samesub"></a>Kuidas luua ühendus VNets, mis on ühe tellimuse

![v2v skeem](./media/vpn-gateway-vnet-vnet-rm-ps/v2vrmps.png)

### <a name="before-you-begin"></a>Enne alustamist
    
Enne alustamist peate installima Azure ressursihaldur PowerShelli cmdlet-käsud. Vaadake, [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md) installimise PowerShelli cmdlet-käskude kohta lisateavet.

### <a name="Step1"></a>Samm 1 - leping IP-aadresside vahemikud


Järgmistes juhistes, saame luua kahe virtuaalse võrgu koos vastavate lüüsi alamvõrku ja konfiguratsioone. Seejärel loome VPN-ühendus kahe VNets vahel. See on oluline leping oma võrgu konfigureerimise IP-aadresside vahemikud. Pidage meeles, et peate veenduge, et ükski oma VNet vahemike või kohalikus võrgus vahemike kattu mis tahes viisil.

Kasutame näidetes järgmised väärtused:

**Väärtused TestVNet1:**

- VNet nimi: TestVNet1
- Ressursirühm: TestRG1
- Asukoht: Ida-USA
- TestVNet1: 10.11.0.0/16 ja 10.12.0.0/16
- FrontEnd: 10.11.0.0/24
- Taustväärtus: 10.12.0.0/24
- GatewaySubnet: 10.12.255.0/27
- DNS-i Server: 8.8.8.8
- GatewayName: VNet1GW
- Avaliku IP: VNet1GWIP
- VPNType: RouteBased
- Connection(1to4): VNet1toVNet4
- Connection(1to5): VNet1toVNet5
- ConnectionType: VNet2VNet


**TestVNet4 väärtused:**

- VNet nimi: TestVNet4
- TestVNet2: 10.41.0.0/16 ja 10.42.0.0/16
- FrontEnd: 10.41.0.0/24
- Taustväärtus: 10.42.0.0/24
- GatewaySubnet: 10.42.255.0/27
- Ressursirühm: TestRG4
- Asukoht: Lääne USA.
- DNS-i Server: 8.8.8.8
- GatewayName: VNet4GW
- Avaliku IP: VNet4GWIP
- VPNType: RouteBased
- Ühendus: VNet4toVNet1
- ConnectionType: VNet2VNet



### <a name="Step2"></a>Samm 2: loomine ja konfigureerimine TestVNet1

1. Oma muutujate deklareerimine

    Alustuseks deklareerimine muutujate. Selles näites kinnitab muutujate abil selle ülesande jaoks soovitud väärtused. Enamikul juhtudel peaks oma asendage väärtused. Siiski saate need muutujad, kui teil on tutvuda seda tüüpi konfigureerimise juhised. Muutke muutujaid vajadusel ja seejärel kopeerige ja kleepige need oma PowerShelli konsooli.

        $Sub1 = "Replace_With_Your_Subcription_Name"
        $RG1 = "TestRG1"
        $Location1 = "East US"
        $VNetName1 = "TestVNet1"
        $FESubName1 = "FrontEnd"
        $BESubName1 = "Backend"
        $GWSubName1 = "GatewaySubnet"
        $VNetPrefix11 = "10.11.0.0/16"
        $VNetPrefix12 = "10.12.0.0/16"
        $FESubPrefix1 = "10.11.0.0/24"
        $BESubPrefix1 = "10.12.0.0/24"
        $GWSubPrefix1 = "10.12.255.0/27"
        $DNS1 = "8.8.8.8"
        $GWName1 = "VNet1GW"
        $GWIPName1 = "VNet1GWIP"
        $GWIPconfName1 = "gwipconf1"
        $Connection14 = "VNet1toVNet4"
        $Connection15 = "VNet1toVNet5"

2. Ühenduse loomine oma tellimuse

    Lülitumine PowerShelli kasutamine ressursihaldur cmdlet-käsud. Avage oma PowerShelli konsooli ja teie kontoga ühendust luua. Järgmises näites abil saate ühendada:

        Login-AzureRmAccount

    Kontrollige konto tellimused.

        Get-AzureRmSubscription 

    Määrake tellimus, mida soovite kasutada.

        Select-AzureRmSubscription -SubscriptionName $Sub1

3. Uue ressursirühma loomine

        New-AzureRmResourceGroup -Name $RG1 -Location $Location1

4. Alamvõrgu konfiguratsioone TestVNet1 loomine

    Selles näites loob virtuaalse võrgu nimega TestVNet1 ja kolme alamvõrku, üks nimega GatewaySubnet, üks nimega FrontEnd ja nimega ühele kirjutamata. Kui väärtused, on oluline alati pange nimi oma lüüsi alamvõrgu spetsiaalselt GatewaySubnet. Kui te nimi on midagi muud, ei õnnestu oma lüüsi loomine. 

    Järgmises näites kasutatakse varem määratavad muutujaid. Selles näites kasutatakse lüüsi alamvõrgu on /27. On võimalik luua lüüsi alamvõrgu /29 võimalikult väike, soovitame suuremat alamvõrku, mis sisaldab rohkem aadresse, valides vähemalt /28 või /27 luua. See võimaldab piisavalt aadressid mahutamiseks soovite edaspidi võimalike täiendavate kuvatakse. 

        $fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1
        $besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
        $gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1


5. TestVNet1 loomine

        New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 `
        -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1

6. Taotluse avaliku IP-aadress

    Koosolekukutse loote oma VNet lüüsi eraldatav avaliku IP-aadress. Pange tähele, et selle AllocationMethod on dünaamiline. Te ei saa määrata IP-aadress, mida soovite kasutada. See on dünaamiliselt eraldatud lüüsi. 

        $gwpip1 = New-AzureRmPublicIpAddress -Name $GWIPName1 -ResourceGroupName $RG1 `
        -Location $Location1 -AllocationMethod Dynamic

7. Lüüsi konfigureerimine loomine

    Lüüsi konfigureerimine määratleb alamvõrgu ja kasutada avaliku IP-aadressi. Käesolevas näites abil saate luua oma lüüsi konfigureerimine. 

        $vnet1 = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
        $subnet1 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
        $gwipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName1 `
        -Subnet $subnet1 -PublicIpAddress $gwpip1

8. Lüüsi TestVNet1 loomine

    Selles etapis loote virtuaalse võrgu lüüsi oma TestVNet1. VNet-VNet konfiguratsioone vaja RouteBased VpnType. Lüüsi loomine võib võtta aega (45 minutit või rohkem valmis).

        New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 `
        -Location $Location1 -IpConfigurations $gwipconf1 -GatewayType Vpn `
        -VpnType RouteBased -GatewaySku Standard

### <a name="step-3---create-and-configure-testvnet4"></a>Samm 3 - loomine ja konfigureerimine TestVNet4

Kui olete konfigureerinud TestVNet1, luua TestVNet4. Järgige allpool, asendades väärtused ise vastavalt vajadusele. Selles etapis tuleb teha sama PowerShelli seansi jooksul Kuna tegemist on sama tellimus.

1. Oma muutujate deklareerimine

    Kindlasti need, mida soovite kasutada oma konfiguratsioon väärtuste asendamine.

        $RG4 = "TestRG4"
        $Location4 = "West US"
        $VnetName4 = "TestVNet4"
        $FESubName4 = "FrontEnd"
        $BESubName4 = "Backend"
        $GWSubName4 = "GatewaySubnet"
        $VnetPrefix41 = "10.41.0.0/16"
        $VnetPrefix42 = "10.42.0.0/16"
        $FESubPrefix4 = "10.41.0.0/24"
        $BESubPrefix4 = "10.42.0.0/24"
        $GWSubPrefix4 = "10.42.255.0/27"
        $DNS4 = "8.8.8.8"
        $GWName4 = "VNet4GW"
        $GWIPName4 = "VNet4GWIP"
        $GWIPconfName4 = "gwipconf4"
        $Connection41 = "VNet4toVNet1"

    Enne jätkamist, veenduge, et teil on ühendus tellimuse 1.

2. Uue ressursirühma loomine

        New-AzureRmResourceGroup -Name $RG4 -Location $Location4

3. Alamvõrgu konfiguratsioone TestVNet4 loomine

        $fesub4 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName4 -AddressPrefix $FESubPrefix4
        $besub4 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName4 -AddressPrefix $BESubPrefix4
        $gwsub4 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName4 -AddressPrefix $GWSubPrefix4

4. TestVNet4 loomine

        New-AzureRmVirtualNetwork -Name $VnetName4 -ResourceGroupName $RG4 `
        -Location $Location4 -AddressPrefix $VnetPrefix41,$VnetPrefix42 -Subnet $fesub4,$besub4,$gwsub4

5. Taotluse avaliku IP-aadress

        $gwpip4 = New-AzureRmPublicIpAddress -Name $GWIPName4 -ResourceGroupName $RG4 `
        -Location $Location4 -AllocationMethod Dynamic

6. Lüüsi konfigureerimine loomine

        $vnet4 = Get-AzureRmVirtualNetwork -Name $VnetName4 -ResourceGroupName $RG4
        $subnet4 = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet4
        $gwipconf4 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName4 -Subnet $subnet4 -PublicIpAddress $gwpip4

7. TestVNet4 lüüsi loomine

        New-AzureRmVirtualNetworkGateway -Name $GWName4 -ResourceGroupName $RG4 `
        -Location $Location4 -IpConfigurations $gwipconf4 -GatewayType Vpn `
        -VpnType RouteBased -GatewaySku Standard

### <a name="step-4---connect-the-gateways"></a>Juhis 4 – lüüside ühenduse loomine

1. Saada nii virtuaalse võrgu lüüsid

    Selles näites, kuna mõlemad lüüsid on sama tellimuse selles etapis tuleb lõpule viia sama PowerShelli seanss.

        $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1
        $vnet4gw = Get-AzureRmVirtualNetworkGateway -Name $GWName4 -ResourceGroupName $RG4


2. TestVNet1 TestVNet4 ühenduse loomine

    Selles etapis loote ühenduse TestVNet1 TestVNet4 abil. Kuvatakse teile ühiskasutusse antud klahvi näidetes viidatud. Saate oma väärtuste ühiskasutusega võti. Oluline on see, et ühiskasutusse antud võti peab vastama nii ühenduste jaoks. Ühenduse loomist saate lõpuleviimiseks lühikest aega võtta.

        New-AzureRmVirtualNetworkGatewayConnection -Name $Connection14 -ResourceGroupName $RG1 `
        -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet4gw -Location $Location1 `
        -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'

3. TestVNet4 TestVNet1 ühenduse loomine

    See toiming on sarnaneb ülaltoodud, kuid loote ühenduse TestVNet4 TestVNet1 abil. Veenduge, et jagatud võtmed vastavad.

        New-AzureRmVirtualNetworkGatewayConnection -Name $Connection41 -ResourceGroupName $RG4 `
        -VirtualNetworkGateway1 $vnet4gw -VirtualNetworkGateway2 $vnet1gw -Location $Location4 `
        -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'

    Ühenduse tuleks mõne minuti pärast.

4. Kontrollige ühendust. Leiate jaotisest [kuidas veenduda, et teie ühendus](#verify).


## <a name="difsub"></a>Kuidas luua ühendus VNets, mis on erinevate tellimused


![v2v skeem](./media/vpn-gateway-vnet-vnet-rm-ps/v2vdiffsub.png)

Selle stsenaariumi korral ühendust TestVNet1 ja TestVNet5. TestVNet1 ja TestVNet5 asuda mõnda muud tellimust. Selle konfiguratsiooni juhised lisada täiendavaid VNet-VNet ühendust TestVNet5 TestVNet1 ühenduse loomiseks. 

Vahe siin on mõned konfigureerimine juhiseid tuleb teha eraldi PowerShelli seansi teise tellimuse raames. Eriti kui kaks tellimuste kuuluvad erinevad ettevõtted. 

Jätka eelmisi juhiseid ülaltoodud juhiseid. [Samm 1](#Step1) ja [etapp 2](#Step2) loomine ja konfigureerimine TestVNet1 TestVNet1 ja VPN-lüüsi peate teostama. Kui olete samm 1 ja samm 2, jätkake juhisega 5 TestVNet5 loomiseks.

### <a name="step-5---verify-the-additional-ip-address-ranges"></a>Juhis 5 – Veenduge, et täiendavad IP-aadresside vahemikud

See on oluline veenduge, et IP-aadress ruumi uue virtuaalse võrgu TestVNet5, ei kattuks mõnda oma VNet vahemike või kohalikus võrgus lüüsi vahemike. 

Selles näites võib virtuaalne võrkude kuuluvad erinevad ettevõtted. Selle toiminguga saate selle TestVNet5 järgmised väärtused:

**TestVNet5 väärtused:**

- VNet nimi: TestVNet5
- Ressursirühm: TestRG5
- Asukoht: Jaapan Ida
- TestVNet5: 10.51.0.0/16 ja 10.52.0.0/16
- FrontEnd: 10.51.0.0/24
- Taustväärtus: 10.52.0.0/24
- GatewaySubnet: 10.52.255.0.0/27
- DNS-i Server: 8.8.8.8
- GatewayName: VNet5GW
- Avaliku IP: VNet5GWIP
- VPNType: RouteBased
- Ühendus: VNet5toVNet1
- ConnectionType: VNet2VNet

**Täiendavate TestVNet1 väärtused:**

- Ühendus: VNet1toVNet5


### <a name="step-6---create-and-configure-testvnet5"></a>Juhist 6 – loomine ja konfigureerimine TestVNet5

Selles etapis tuleb teha kontekstis uude tellimusse. See osa võib teha muus asutuses, mis kuulub tellimuse administraator.

1. Oma muutujate deklareerimine

    Kindlasti need, mida soovite kasutada oma konfiguratsioon väärtuste asendamine.

        $Sub5 = "Replace_With_the_New_Subcription_Name"
        $RG5 = "TestRG5"
        $Location5 = "Japan East"
        $VnetName5 = "TestVNet5"
        $FESubName5 = "FrontEnd"
        $BESubName5 = "Backend"
        $GWSubName5 = "GatewaySubnet"
        $VnetPrefix51 = "10.51.0.0/16"
        $VnetPrefix52 = "10.52.0.0/16"
        $FESubPrefix5 = "10.51.0.0/24"
        $BESubPrefix5 = "10.52.0.0/24"
        $GWSubPrefix5 = "10.52.255.0/27"
        $DNS5 = "8.8.8.8"
        $GWName5 = "VNet5GW"
        $GWIPName5 = "VNet5GWIP"
        $GWIPconfName5 = "gwipconf5"
        $Connection51 = "VNet5toVNet1"

2. Ühenduse loomine tellimuse 5

    Avage oma PowerShelli konsooli ja teie kontoga ühendust luua. Järgmises näites abil saate ühendada:

        Login-AzureRmAccount

    Kontrollige konto tellimused.

        Get-AzureRmSubscription 

    Määrake tellimus, mida soovite kasutada.

        Select-AzureRmSubscription -SubscriptionName $Sub5

3. Uue ressursirühma loomine

        New-AzureRmResourceGroup -Name $RG5 -Location $Location5

4. Alamvõrgu konfiguratsioone TestVNet4 loomine
    
        $fesub5 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName5 -AddressPrefix $FESubPrefix5
        $besub5 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName5 -AddressPrefix $BESubPrefix5
        $gwsub5 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName5 -AddressPrefix $GWSubPrefix5

5. TestVNet5 loomine

        New-AzureRmVirtualNetwork -Name $VnetName5 -ResourceGroupName $RG5 -Location $Location5 `
        -AddressPrefix $VnetPrefix51,$VnetPrefix52 -Subnet $fesub5,$besub5,$gwsub5

6. Taotluse avaliku IP-aadress

        $gwpip5 = New-AzureRmPublicIpAddress -Name $GWIPName5 -ResourceGroupName $RG5 `
        -Location $Location5 -AllocationMethod Dynamic


7. Lüüsi konfigureerimine loomine

        $vnet5 = Get-AzureRmVirtualNetwork -Name $VnetName5 -ResourceGroupName $RG5
        $subnet5  = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet5
        $gwipconf5 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName5 -Subnet $subnet5 -PublicIpAddress $gwpip5

8. TestVNet5 lüüsi loomine

        New-AzureRmVirtualNetworkGateway -Name $GWName5 -ResourceGroupName $RG5 -Location $Location5 `
        -IpConfigurations $gwipconf5 -GatewayType Vpn -VpnType RouteBased -GatewaySku Standard

### <a name="step-7---connecting-the-gateways"></a>Juhis 7 – lüüside ühenduse loomine

Selles näites lüüside erinevate tellimused, kuna me olete tükeldada selles etapis tuleb kaks PowerShelli seansid märgitud [tellimuse 1] ja [tellimuse 5].

1. **[Tellimuse 1]** Tellimuse 1 virtuaalse võrgu lüüsi hankimine

    Veenduge, et saate sisse logida ja ühenduse tellimuse 1.

        $vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1

    Kopeerige väljund järgmisi elemente ja saatke tellimuse 5 administraator e-posti või muul viisil.

        $vnet1gw.Name
        $vnet1gw.Id

    Kaks elementi on sarnane järgmine näide väljund väärtused:

        PS D:\> $vnet1gw.Name
        VNet1GW
        PS D:\> $vnet1gw.Id
        /subscriptions/b636ca99-6f88-4df4-a7c3-2f8dc4545509/resourceGroupsTestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW

2. **[Tellimuse 5]** Tellimuse 5 virtuaalse võrgu lüüsi hankimine

    Veenduge, et saate sisse logida ja ühenduse tellimuse 5.

        $vnet5gw = Get-AzureRmVirtualNetworkGateway -Name $GWName5 -ResourceGroupName $RG5

    Kopeerige väljund järgmisi elemente ja saatke tellimuse 1 administraator e-posti või muul viisil.

        $vnet5gw.Name
        $vnet5gw.Id

    Kaks elementi on sarnane järgmine näide väljund väärtused:

        PS C:\> $vnet5gw.Name
        VNet5GW
        PS C:\> $vnet5gw.Id
        /subscriptions/66c8e4f1-ecd6-47ed-9de7-7e530de23994/resourceGroups/TestRG5/providers/Microsoft.Network/virtualNetworkGateways/VNet5GW

3. **[Tellimuse 1]** TestVNet1 TestVNet5 ühenduse loomine

    Selles etapis loote ühenduse TestVNet1 TestVNet5 abil. Erinevus on selle $vnet5gw ei saa otse saadud, kuna see on mõni muu tellimus. Peate luua uue PowerShelli objekti väärtused edastatud tellimuse 1 eespool antud juhiseid. Kasutage alltoodud näites. Asendage nimi, Id ja ühiskasutuses võti oma väärtused. Oluline on see, et ühiskasutusse antud võti peab vastama nii ühenduste jaoks. Ühenduse loomist saate lõpuleviimiseks lühikest aega võtta.

    Veenduge, et loote tellimuse 1. 
    
        $vnet5gw = New-Object Microsoft.Azure.Commands.Network.Models.PSVirtualNetworkGateway
        $vnet5gw.Name = "VNet5GW"
        $vnet5gw.Id   = "/subscriptions/66c8e4f1-ecd6-47ed-9de7-7e530de23994/resourceGroups/TestRG5/providers/Microsoft.Network/virtualNetworkGateways/VNet5GW"
        $Connection15 = "VNet1toVNet5"
        New-AzureRmVirtualNetworkGatewayConnection -Name $Connection15 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet5gw -Location $Location1 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'

4. **[Tellimuse 5]** TestVNet5 TestVNet1 ühenduse loomine

    See toiming on sarnaneb ülaltoodud, kuid loote ühenduse TestVNet5 TestVNet1 abil. Sama protsessi saadud tellimuse 1 väärtustel põhineva PowerShelli objekti kehtib ka siin. Selles etapis tuleb kindlasti, et vastavad jagatud võtmed.

    Veenduge, et loote tellimuse 5.

        $vnet1gw = New-Object Microsoft.Azure.Commands.Network.Models.PSVirtualNetworkGateway
        $vnet1gw.Name = "VNet1GW"
        $vnet1gw.Id = "/subscriptions/b636ca99-6f88-4df4-a7c3-2f8dc4545509/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW "
        New-AzureRmVirtualNetworkGatewayConnection -Name $Connection51 -ResourceGroupName $RG5 -VirtualNetworkGateway1 $vnet5gw -VirtualNetworkGateway2 $vnet1gw -Location $Location5 -ConnectionType Vnet2Vnet -SharedKey 'AzureA1b2C3'

## <a name="verify"></a>Kuidas kontrollida ühenduse


[AZURE.INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)]


[AZURE.INCLUDE [verify connection powershell](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)] 


## <a name="next-steps"></a>Järgmised sammud

- Kui teie ühendus on loodud, saate lisada virtuaalse võrguga virtuaalmasinates. Üksikasjalikud juhised leiate teemast [loomine virtuaalse masina](../virtual-machines/virtual-machines-windows-hero-tutorial.md) .
- BGP kohta leiate teavet teemast [BGP ülevaade](vpn-gateway-bgp-overview.md) ja [Kuidas konfigureerida BGP](vpn-gateway-bgp-resource-manager-ps.md). 

