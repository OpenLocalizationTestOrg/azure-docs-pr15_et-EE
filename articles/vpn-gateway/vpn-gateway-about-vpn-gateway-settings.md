<properties 
   pageTitle="Virtuaalse võrgu lüüside VPN-lüüsi sätete kohta | Microsoft Azure'i"
   description="Lisateavet VPN-lüüsi Azure virtuaalse võrgu sätteid."
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
   ms.date="10/18/2016"
   ms.author="cherylmc" />

# <a name="about-vpn-gateway-settings"></a>VPN-lüüsi sätete kohta

Lüüsi ühenduse VPN lahenduse sõltub konfiguratsioonist mitme ressursse selleks, et saata võrguliiklust vahel virtuaalse ja kohapealse asukohad. Iga ressursi sisaldab konfigureeritav sätted. Ressursid ja sätete kombinatsiooni määrab ühenduse tulemuse.

Jaotised selles artiklis käsitletakse ressursid ja sätteid, mis on seotud VPN-i **Ressursihaldur** juurutamise mudeli lüüsi. Teil võib kasu saadaval kuvatakse ühenduse topoloogia skeemide abil. Iga ühendus lahenduse [VPN lüüsi](vpn-gateway-about-vpngateways.md) artikli leiate kirjeldused ja topoloogia diagrammid. 

## <a name="gwtype"></a>Lüüsi tüübid

Iga virtuaalse võrgu olla ainult üks virtuaalse võrgu lüüsi iga tüüpi. Kui loote virtuaalse võrgu lüüsi, peab veenduge, et lüüsi tüüp on õiged konfiguratsioonist.

-GatewayType jaoks saadaval väärtused: 

- VPN
- ExpressRoute

VPN-lüüsi nõuab selle `-GatewayType` *Vpn*.  

Näide:

    New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
    -Location 'West US' -IpConfigurations $gwipconfig -GatewayType Vpn `
    -VpnType RouteBased
 

## <a name="gwsku"></a>Lüüsi SKU-de jaoks


[AZURE.INCLUDE [vpn-gateway-gwsku-include](../../includes/vpn-gateway-gwsku-include.md)]

### <a name="configuring-the-gateway-sku"></a>Lüüsi SKU konfigureerimine

**Azure'i portaalis, mis määrab lüüsi SKU-ga**

Kui kasutate Azure portaali ressursihaldur virtuaalse võrgu lüüsi loomiseks, saate valida, kasutades ripploendit lüüsi SKU-ga. Suvandid, mis on esitatud vastavad lüüsi tüüp ja valite VPN-tüüp.

Näiteks kui valite lüüsi tüüp "VPN" ja selle VPN tüüp "poliitika põhinev", kuvatakse ainult "Basic" SKU kuna see on ainult SKU, mis on saadaval PolicyBased VPN. Kui valite "Marsruutimiseks põhinev", saate valida Basic, Standard- ja HighPerformance SKU-de jaoks. 


**Lüüsi SKU täpsustades PowerShelli abil**


Järgmises näites PowerShelli määrab selle `-GatewaySku` nimega *Standardne*.

    New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
    -Location 'West US' -IpConfigurations $gwipconfig -GatewaySku Standard `
    -GatewayType Vpn -VpnType RouteBased

**Lüüsi SKU muutmine**

Kui soovite uuendada oma lüüsi SKU võimsam SKU (alates Basic/Standard HighPerformance) saate kasutada funktsiooni `Resize-AzureRmVirtualNetworkGateway` PowerShelli cmdlet-käsk. Saate ka vahetate lüüsi SKU suurus cmdlet-käsu abil.

PowerShelli järgmises näites on kujutatud lüüsi suuruse HighPerformance SKU-ga.

    $gw = Get-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg
    Resize-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku HighPerformance

### <a name="estimated-aggregate-throughput-by-gateway-sku-and-type"></a>Eeldatav liitväärtuse poolt SKU lüüsi läbilaskevõime ja tüüp

Järgmises tabelis on lüüsi tüübid ja hinnanguline liitväärtuse läbilaskevõime. Selles tabelis kehtib ressursihaldur ja klassikaline juurutamise mudelid.

[AZURE.INCLUDE [vpn-gateway-table-gwtype-aggthroughput](../../includes/vpn-gateway-table-gwtype-aggtput-include.md)] 


## <a name="connectiontype"></a>Ühenduse tüübid

Ressursihaldur juurutamise mudeli, iga konfiguratsioon nõuab mõne kindla virtuaalse võrguühenduse lüüsi tüüp. Saadaval ressursihaldur PowerShelli väärtused `-ConnectionType` on:

- IPsec
- Vnet2Vnet
- ExpressRoute
- VPNClient

Järgmises näites PowerShelli loome S2S ühendus, mis nõuab ühendusetüübi *IPsec*.

    New-AzureRmVirtualNetworkGatewayConnection -Name localtovon -ResourceGroupName testrg `
    -Location 'West US' -VirtualNetworkGateway1 $gateway1 -LocalNetworkGateway2 $local `
    -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'


## <a name="vpntype"></a>VPN-i andmetüübid

Kui loote virtuaalse võrgu lüüsi VPN lüüsi konfigureerimise, määrake VPN-tüüp. VPN-tüüp, mille valimisel sõltub ühenduse topoloogia, mida soovite luua. Näiteks P2S ühenduse nõuab RouteBased VPN tüüp. Riistvara, mida kasutate sõltub ka VPN-tüüp. S2S konfiguratsioone vaja VPN seadet. Mõned VPN seadmed toetavad ainult teatud VPN.

Valige VPN-tüüp peavad vastama kõik ühenduse lahendus soovite luua. Näiteks, kui soovite luua ühenduse lüüs S2S VPN ja P2S VPN ühenduse lüüs virtuaalse samasse võrku, saate kasutaks VPN-tüüp *RouteBased* Kuna P2S nõuab RouteBased VPN tüüp. Lisaks peaksite veenduge, et seadme VPN toetatud RouteBased VPN-ühendus. 

Kui virtuaalse võrgu lüüsi on loodud, ei saa muuta VPN-tüüp. Teil on virtuaalse võrgu lüüsi kustutamine ja uue konto loomiseks. On kahte tüüpi toiminguid VPN.

[AZURE.INCLUDE [vpn-gateway-vpntype](../../includes/vpn-gateway-vpntype-include.md)]


Järgmises näites PowerShelli määrab selle `-VpnType` *RouteBased*nimega. Lüüsi loomisel peate veenduge, et - VpnType on õiged konfiguratsioonist. 

    New-AzureRmVirtualNetworkGateway -Name vnetgw1 -ResourceGroupName testrg `
    -Location 'West US' -IpConfigurations $gwipconfig `
    -GatewayType Vpn -VpnType RouteBased

##  <a name="requirements"></a>Lüüsi nõuded

[AZURE.INCLUDE [vpn-gateway-table-requirements](../../includes/vpn-gateway-table-requirements-include.md)] 


## <a name="gwsub"></a>Lüüsi alamvõrgu

Virtuaalse võrgu lüüsi konfigureerida, peate esmalt oma VNet lüüsi alamvõrgu loomiseks. Lüüsi alamvõrgu peab nimega *GatewaySubnet* töötaks õigesti. Sellise nimega võimaldab Azure'i teada, et see alamvõrgu tuleks kasutada lüüsi.

Lüüsi teie alamvõrku suurus sõltub täielikult konfiguratsioon, mida soovite luua. Kuigi see on võimalik luua lüüsi alamvõrgu /29 võimalikult väike, me soovitame teil luua lüüsi alamvõrgu /28 või suurem (/ 28, /27, /26, jne.). 

Lüüsi suuremaks loomise ei luba teil käivitumist lüüsi mahupiirangud suhtes. Näiteks võib-olla olete loonud virtuaalse võrgu lüüsi lüüsi alamvõrgu suurusega /29 S2S ühenduse. Te nüüd soovite konfigureerida S2S/ExpressRoute koos konfigureerimine. Selle konfiguratsiooni nõuab lüüsi alamvõrgu minimaalse suurusega /28. Looge oma konfiguratsioon, oleksite lüüsi alamvõrgu miinimumnõuetele ühenduse, mis on /28 mahutamiseks muuta.

Ressursihaldur PowerShelli järgmises näites on kujutatud lüüsi alamvõrku, nimega GatewaySubnet. Saate vaadata CIDR märke määrab on /27, mis võimaldab piisavalt enamik konfiguratsioone, mis on praegu olemas IP-aadressid.

    Add-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.0.3.0/27

[AZURE.INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)] 


## <a name="lng"></a>Kohalikus võrgus lüüsid

VPN lüüsi konfigureerimine loomisel kohtvõrgu lüüsi sageli tähistab oma kohapealse asukoht. Klassikaline juurutamise mudeli, kohtvõrgu lüüsi viidatud kui kohalik saidi. 

Pange kohtvõrgu lüüsi nimi, seadme kohapealse VPN avaliku IP-aadress ja aadress eesliidete tähised, mis asuvad kohapealse asukoha määramiseks. Azure'i suunatud võrguliikluse eesliiteid sihtkoha aadress, konsulteerib teie kohalikus võrgus lüüsi jaoks määratud konfiguratsiooni ja marsruudib pakette vastavalt sellele. Samuti Määrake kohalikus võrgus lüüside VNet-VNet konfiguratsioone, mis kasutavad VPN-ühenduse lüüs jaoks.

PowerShelli järgmises näites luuakse uus kohtvõrgu lüüsi:

    New-AzureRmLocalNetworkGateway -Name LocalSite -ResourceGroupName testrg `
    -Location 'West US' -GatewayIpAddress '23.99.221.164' -AddressPrefix '10.5.51.0/24'

Mõnikord on vaja kohtvõrgu lüüsi sätete muutmine. Näiteks kui saate lisada või muuta aadress vahemiku või kui muutub VPN seadme IP-aadress. Klassikaline VNet, saate nende sätete lehel kohaliku võrgu klassikaline portaalis. Jaoks ressursihaldur, lugege teemat [Muuda kohtvõrgu lüüsi sätete PowerShelli kaudu](vpn-gateway-modify-local-network-gateway.md).

## <a name="resources"></a>REST API-d ja PowerShelli cmdletid

Lisaressursid tehniline ja teatud süntaksi nõuded VPN-lüüsi konfiguratsioone REST API-d ja PowerShelli cmdlet-käskude kasutamisel leiate teemast järgmistel lehtedel:

|**Klassikaline** | **Ressursihaldur**|
|-----|----|
|[PowerShelli](https://msdn.microsoft.com/library/mt270335.aspx)|[PowerShelli](https://msdn.microsoft.com/library/mt163510.aspx)|
|[REST API-GA](https://msdn.microsoft.com/library/jj154113.aspx)|[REST API-GA](https://msdn.microsoft.com/library/mt163859.aspx)|


## <a name="next-steps"></a>Järgmised sammud

Vt Lisateavet saadaolevate ühenduse konfiguratsioone [VPN lüüsi](vpn-gateway-about-vpngateways.md) . 







 
