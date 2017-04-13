<properties 
   pageTitle="Klassikaline virtuaalne võrkude ühendamiseks ressursihaldur virtuaalne võrkude PowerShelli kaudu | Microsoft Azure'i"
   description="Saate teada, kuidas luua VPN-ühendus klassikaline VNets ja ressursihaldur VNets VPN-lüüsi ja PowerShelli abil"
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
   ms.date="09/23/2016"
   ms.author="cherylmc" />

# <a name="connect-virtual-networks-from-different-deployment-models-using-powershell"></a>Ühenduse loomine virtuaalse võrgu eri juurutamise mudelid PowerShelli abil

> [AZURE.SELECTOR]
- [Portaal](vpn-gateway-connect-different-deployment-models-portal.md)
- [PowerShelli](vpn-gateway-connect-different-deployment-models-powershell.md)

Azure'i praegu on kaks juhtimine mudelit: klassikaline ja ressursside Manager (RM). Kui te kasutate Azure juba mõnda aega, siis ilmselt on Azure VMs ja eksemplari rollide klassikaline VNet töötab. Teie uuem VMs ja rolli aknad võib töötama VNet, mis on loodud rakenduses ressursihaldur. Selles artiklis tutvustatakse klassikaline VNets ühenduse ressursihaldur VNets lubamiseks ressursse, mis asub eraldi juurutamise mudelid kaudu ühenduse lüüs omavahel suhelda.

Saate luua ühenduse VNets, mis on erinevate tellimused ja erinevate piirkondade vahel. Samuti saate ühendada VNets, mis on juba ühenduste kohapealse võrgu, peaasi, et lüüsi, et need on konfigureeritud on dünaamiline või marsruutimiseks. VNet-VNet ühenduste kohta lisateabe saamiseks vt käesoleva artikli lõpus [VNet-VNet FAQ](#faq) .

### <a name="deployment-models-and-methods-for-connecting-vnets-in-different-deployment-models"></a>Juurutamise mudelite ja ühenduse VNets erinevate juurutamise mudelid meetodid

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

Uuendame järgmises tabelis, kui uus ja täiendavad Tabeliriistad selle konfiguratsiooni puhul. Kui artikkel on saadaval, me lingi otse tabelist.<br><br>

[AZURE.INCLUDE [vpn-gateway-table-vnet-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)] 

#### <a name="vnet-peering"></a>VNet silmitsemine

[AZURE.INCLUDE [vpn-gateway-vnetpeeringlink](../../includes/vpn-gateway-vnetpeeringlink-include.md)]


## <a name="before-beginning"></a>Enne alustamist

Järgmised toimingud sõelub dünaamiline või marsruutimiseks lüüsi konfigureerimine iga VNet ja lüüside vahel VPN-ühenduse loomiseks vajalikud sätted. Selle konfiguratsiooni ei toeta staatiline või poliitika lüüsid.

### <a name="prerequisites"></a>Eeltingimused

 - Mõlemad VNets on juba loodud.
 - Aadresside vahemike jaoks soovitud VNets kattuvad üksteisega või mõnda muud ühendused, mis võivad seotud lüüside vahemikud kattuvad.
 - Olete installinud uusimaid PowerShelli cmdlet-käskude (1.0.2 või uuem versioon). Lisateabe saamiseks vaadake [, kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md) . Veenduge, et installida nii teenuse haldus (SM) ja ressursside Manager (RM) cmdlet-käsud. 

### <a name="exampleref"></a>Näide sätted

Näide seadistada abimaterjalina.

**Klassikaline VNet sätted**

VNet nimi = ClassicVNet <br>
Asukoht = Lääne USA. <br>
Virtuaalse võrgu aadress tühikute = 10.0.0.0/24 <br>
Alamvõrgu-1 = 10.0.0.0/27 <br>
GatewaySubnet = 10.0.0.32/29 <br>
Kohaliku Network Name = RMVNetLocal <br>
GatewayType = DynamicRouting

**Ressursihaldur VNet sätted**

VNet nimi = RMVNet <br>
Ressursirühm = RG1 <br>
Virtuaalse võrgu IP Address tühikuid = 192.168.0.0/16 <br>
Alamvõrgu-1 = 192.168.1.0/24 <br>
GatewaySubnet = 192.168.0.0/26 <br>
Asukoht = Ida-USA <br>
Lüüsi avaliku IP nimi = gwpip <br>
Kohaliku võrgu lüüsi = ClassicVNetLocal <br>
Virtuaalse võrgu lüüsi nimi = RMGateway <br>
Lüüsi IP adresseerimise konfigureerimine = gwipconfig


## <a name="createsmgw"></a>Jaotis 1 - klassikaline VNet konfigureerimine

### <a name="part-1---download-your-network-configuration-file"></a>Osa 1 - oma võrgu konfigureerimise faili alla laadida

1. Logige sisse kontole Azure PowerShelli konsooli laiendatud õigustega. Järgmine cmdlet-käsk küsib teilt sisselogimise Azure'i kontosse. Pärast sisselogimist, allalaadimist Kontosätete nii, et need on saadaval Azure PowerShelli. Kasutate so PowerShelli cmdlet-käskude see osa konfiguratsiooni lõpuleviimiseks.

        Add-AzureAccount

2. Azure'i võrgu konfigureerimise faili eksportida, käivitage järgmine käsk. Saate muuta mõnda muusse asukohta vajadusel eksporditava faili asukoht. Saate faili redigeerida ja importige see Azure'i.

        Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml

3. Avage allalaaditud selle redigeerimiseks XML-faili. Vt näide konfiguratsioonifail võrgu [Network Configuration skeemi](https://msdn.microsoft.com/library/jj157100.aspx).
 

### <a name="part-2--verify-the-gateway-subnet"></a>Osa 2 - lüüsi alamvõrgu kinnitamine

**VirtualNetworkSites** element, kui see on juba loodud lisada oma VNet lüüsi alamvõrgu. Konfiguratsioonifail võrgu töötamisel tuleb lüüsi alamvõrgu nimega "GatewaySubnet" või Azure ei tunne ja kasutada seda lüüsi alamvõrgu.

[AZURE.INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)]

**Näide:**
    
    <VirtualNetworkSites>
      <VirtualNetworkSite name="ClassicVNet" Location="West US">
        <AddressSpace>
          <AddressPrefix>10.0.0.0/24</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="Subnet-1">
            <AddressPrefix>10.0.0.0/27</AddressPrefix>
          </Subnet>
          <Subnet name="GatewaySubnet">
            <AddressPrefix>10.0.0.32/29</AddressPrefix>
          </Subnet>
        </Subnets>
      </VirtualNetworkSite>
    </VirtualNetworkSites>
       
### <a name="part-3---add-the-local-network-site"></a>Osa 3 - kohtvõrgu saidile lisamine

Kohalikus võrgus saidi lisamist tähistab RM VNet, mida soovite ühendada. Kui teil pole veel olemas **LocalNetworkSites** element, kui ühe faili lisamiseks. Selles etapis konfiguratsiooni, on VPNGatewayAddress võib olla mis tahes kehtiv avaliku IP-aadressi Kuna me ei ole veel loonud lüüsi ressursihaldur VNet. Pärast lüüsi koostamiseks me õige avaliku IP-aadress on määratud RM lüüsi asendada selle kohatäite IP-aadress.

    <LocalNetworkSites>
      <LocalNetworkSite name="RMVNetLocal">
        <AddressSpace>
          <AddressPrefix>192.168.0.0/16</AddressPrefix>
        </AddressSpace>
        <VPNGatewayAddress>13.68.210.16</VPNGatewayAddress>
      </LocalNetworkSite>
    </LocalNetworkSites>

### <a name="part-4---associate-the-vnet-with-the-local-network-site"></a>Osa 4 - funktsiooni VNet seostada kohtvõrgu saidi

Selles jaotises me Määrake kohalikus võrgus saidile, mis ühendatava VNet abil. Selles näites on ressursihaldur VNet, mida te viidatud varasemas versioonis. Veenduge, et vastavad nimed. See toiming ei loo lüüsi. Seda määrab kohalikus võrgus, mis loob ühenduse lüüs.

        <Gateway>
          <ConnectionsToLocalNetwork>
            <LocalNetworkSiteRef name="RMVNetLocal">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
          </ConnectionsToLocalNetwork>
        </Gateway>

### <a name="part-5---save-the-file-and-upload"></a>Osa 5 - salvestage fail ja üleslaadimine

Salvestage fail ja seejärel importige see Azure käivitage järgmine käsk. Veenduge, et saate muuta faili tee vastavalt vajadusele keskkonnas.

        Set-AzureVNetConfig -ConfigurationPath C:\AzureNet\NetworkConfig.xml

Peaksite nägema midagi sarnast järgmise tulemi, mis näitab, et importimine õnnestus.

        OperationDescription        OperationId                      OperationStatus                                                
        --------------------        -----------                      ---------------                                                
        Set-AzureVNetConfig        e0ee6e66-9167-cfa7-a746-7casb9    Succeeded 

### <a name="part-6---create-the-gateway"></a>Osa 6 - lüüsi loomine 

Klassikaline portaali kaudu või PowerShelli abil saate luua VNet lüüsi.

Järgmises näites abil saate luua dünaamilisi marsruutimise lüüsi:

    New-AzureVNetGateway -VNetName ClassicVNet -GatewayType DynamicRouting

Lüüsi olekut saate kontrollida, kasutades funktsiooni `Get-AzureVNetGateway` cmdlet-käsk.


## <a name="creatermgw"></a>Jaotis 2: Konfigureerimine RM VNet lüüsi

RM VNet VPN-lüüsi loomiseks täitke järgmised juhised. Ei alga juhiseid kuni pärast klassikaline VNet lüüsi avaliku IP-aadress. 

1. **Azure'i kontosse sisselogimine** PowerShelli konsooli. Järgmine cmdlet-käsk küsib teilt sisselogimise Azure'i kontosse. Pärast sisselogimist, laaditakse teie konto sätted, et need on saadaval Azure PowerShelli.

        Login-AzureRmAccount 

    Azure'i tellimuste loendi hankimine, kui teil on mitu tellimust.

        Get-AzureRmSubscription

    Määrake tellimus, mida soovite kasutada. 

        Select-AzureRmSubscription -SubscriptionName "Name of subscription"


2.  **Loo lüüsi kohalikus võrgus**. Virtuaalne võrku, kohtvõrgu lüüsi tavaliselt viitab oma kohapealse asukoht. Sel juhul kohtvõrgu lüüsi viitab teie klassikaline VNet. Pange nimi, mille Azure saate viidata ja määrata ka aadress ruumi eesliide. Azure'i kasutab IP address eesliite teie määratud tuvastada, milliseid liiklust saata oma kohapealse asukohta. Kui teil on vaja kohandada teavet siin hiljem enne loomise lüüsi, saate muuta väärtusi ja proovi uuesti käivitada.<br><br>
    - `-Name`on nimi, mida soovite määrata kohtvõrgu lüüsi viidata.<br>
    - `-AddressPrefix`on teie klassikaline VNet aadress ruumi.<br>
    - `-GatewayIpAddress`on klassikaline VNet lüüsi avaliku IP-aadress. Kindlasti muuta järgmises näites kajastamiseks õige IP-aadress.
    
            New-AzureRmLocalNetworkGateway -Name ClassicVNetLocal `
            -Location "West US" -AddressPrefix "10.0.0.0/24" `
            -GatewayIpAddress "n.n.n.n" -ResourceGroupName RG1

3. Lüüsi virtuaalse võrgu jaoks ressursihaldur VNet eraldatav **taotleda avaliku IP-aadressi** . Te ei saa määrata IP-aadress, mida soovite kasutada. IP-aadress on dünaamiliselt eraldatud virtuaalse võrgu lüüsi. Siiski ei tähenda see muudab IP-aadress. Ainult kellaaja virtuaalse võrgu lüüsi IP-aadress muutub on kui lüüsi on kustutatud ja uuesti luua. See ei muuda suurust, lähtestamine või muude sisemiste hooldustööd/uuendamine lüüsi üle.<br>Selles etapis tuleb ka määrata muutuja, mida kasutatakse hiljem.


        $ipaddress = New-AzureRmPublicIpAddress -Name gwpip `
        -ResourceGroupName RG1 -Location 'EastUS' `
        -AllocationMethod Dynamic

4. **Veenduge, et virtuaalse võrgu on lüüsi alamvõrgu**. Kui pole lüüsi alamvõrgu on olemas, lisada. Veenduge, et lüüsi alamvõrgu nimega *GatewaySubnet*.

5. **Saate tuua alamvõrgu** lüüsi kasutanud, käivitage järgmine käsk. Selles etapis tuleb ka määrata muutuja kasutada järgmise juhise juurde.
    - `-Name`on teie ressursihaldur VNet nimi.
    - `-ResourceGroupName`on selle VNet seotud ressursirühma. Lüüsi alamvõrgu peab olemas selle VNet ja peab olema nimega *GatewaySubnet* töötaks õigesti.


            $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name GatewaySubnet `
            -VirtualNetwork (Get-AzureRmVirtualNetwork -Name RMVNet -ResourceGroupName RG1) 

6. **Loo lüüsi IP adresseerimise konfigureerimine**. Lüüsi konfigureerimine määratleb alamvõrgu ja kasutada avaliku IP-aadressi. Järgmises näites abil saate luua oma lüüsi konfigureerimine.<br>Selles etapis tuleb soovitud `-SubnetId` ja `-PublicIpAddressId` parameetrite edastada tuleb atribuudi id alamvõrgu ja IP-aadressi objektid, vastavalt. Te ei saa kasutada lihtne string. Nende muutujate määratakse etapis avaliku IP ja samm alamvõrgu toomiseks.

        $gwipconfig = New-AzureRmVirtualNetworkGatewayIpConfig `
        -Name gwipconfig -SubnetId $subnet.id `
        -PublicIpAddressId $ipaddress.id
    
7. **Loo ressursihaldur virtuaalse võrgu lüüsi** , käivitage järgmine käsk. Funktsiooni `-VpnType` peab olema *RouteBased*. 45 minutit või mitu selle lõpuleviimiseks võib kuluda.

        New-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName RG1 `
        -Location "EastUS" -GatewaySKU Standard -GatewayType Vpn `
        -IpConfigurations $gwipconfig `
        -EnableBgp $false -VpnType RouteBased

8. **Avaliku IP-aadressi kopeerimine** üks kord VPN-lüüsi on loodud. Saate kasutada seda, kui saate konfigureerida oma klassikaline VNet Kohtvõrgu sätted. Järgmine cmdlet-käsk abil saate tuua avaliku IP-aadressi. Avaliku IP-aadress on tagasi loetletud *sordib*.

        Get-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName RG1

## <a name="section-3-modify-the-local-network-for-the-classic-vnet"></a>Jaotis 3: Klassikaline VNet muuta kohalikus võrgus

Mida saab teha seda toimingut, kas eksportimine võrgu konfigureerimise faili, seda redigeerida, ja seejärel salvestamine ja importida faili taas Azure. Või saate muuta selle sätte klassikaline portaalis. 

### <a name="to-modify-in-the-portal"></a>Muuta portaalis

1. Avage [klassikaline portaalis](https://manage.windowsazure.com).

2. Klassikaline portaali vasakus servas, liikuge kerides allapoole ja klõpsake nuppu **võrgu**. Klõpsake lehel **võrkude** **Kohaliku võrgu** lehe ülaosas. 

3. **Kohaliku võrgu** lehel klõpsake kohalikus võrgus, et 1 osas konfigureeritud ja seejärel liikuge lehe allserva ja klõpsake nuppu **Redigeeri**.

4. Klõpsake lehel **teie kohalikus võrgus üksikasjad** asendada ressursihaldur lüüsi eelmises jaotises loodud avaliku IP-aadress kohatäite IP-aadress. Klõpsake noolt liikumiseks järgmise jaotise juurde. Veenduge, et **Aadressiruumi** oleks õige ja klõpsake muudatuste kinnitamiseks märke.

### <a name="to-modify-using-the-network-configuration-file"></a>Muuta network configuration-faili abil

1. Võrgu konfigureerimise faili eksportida.
2. Tekstiredaktoris, muutke VPN gateway IP-aadress.

        <VPNGatewayAddress>13.68.210.16</VPNGatewayAddress>

3. Muudatuste salvestamiseks ja naasmiseks Azure'i redigeeritud faili importimine.


## <a name="connect"></a>Jaotis 4: Lüüside vahel ühenduse loomiseks

Lüüside vahelise ühenduse loomine nõuab PowerShelli. Võib-olla peate konto Azure klassikaline PowerShelli cmdlet-käskude lisamine. Selleks saate kasutada järgmine cmdlet-käsk: 
        
    Add-AzureAccount

1. **Määrata ühiskasutusse antud tootevõti** , käivitage järgmine käsk. Selles näites `-VNetName` on klassikaline VNet nimi ja `-LocalNetworkSiteName` on teie määratud kohalikus võrgus, kui olete konfigureerinud selle klassikalise portaalis nimi. Funktsiooni `-SharedKey` on väärtus, mille saate luua ja määrata. Siin saate määrata väärtus peab olema sama väärtuse, kus saate määrata järgmise juhise juurde ühenduse loomisel. Tagasi selle valimi näitama **olek: eduka**.

        Set-AzureVNetGatewayKey -VNetName ClassicVNet `
        -LocalNetworkSiteName RMVNetLocal -SharedKey abc123

2. Looge VPN-ühendus, käitades järgmised käsud.

    **Määrake muutujad**

        $vnet01gateway = Get-AzureRMLocalNetworkGateway -Name ClassicVNetLocal -ResourceGroupName RG1
        $vnet02gateway = Get-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName RG1

    **Ühenduse loomine**<br> Pange tähele, et selle `-ConnectionType` on IPsec, mitte Vnet2Vnet.
        
        New-AzureRmVirtualNetworkGatewayConnection -Name RM-Classic -ResourceGroupName RG1 `
        -Location "East US" -VirtualNetworkGateway1 `
        $vnet02gateway -LocalNetworkGateway2 `
        $vnet01gateway -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'

3. Saate vaadata oma ühenduse kummagi portaalis või kasutades funktsiooni `Get-AzureRmVirtualNetworkGatewayConnection` cmdlet-käsk.

        Get-AzureRmVirtualNetworkGatewayConnection -Name RM-Classic -ResourceGroupName RG1

## <a name="faq"></a>VNet-VNet KKK

FAQ andmete VNet-VNet ühenduste kohta lisateabe vaatamiseks.

[AZURE.INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)] 


