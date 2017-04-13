<properties 
   pageTitle="Linkimine on ExpressRoute ringi virtuaalse võrgu PowerShelli abil | Microsoft Azure'i"
   description="Selle dokumendi pakub ülevaade sellest, kuidas lingitakse virtuaalne võrkude (VNets) ExpressRoute topoloogia ressursihaldur juurutamise mudeli ja PowerShelli abil."
   services="expressroute"
   documentationCenter="na"
   authors="ganesr"
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
   ms.author="ganesr" />

# <a name="link-a-virtual-network-to-an-expressroute-circuit"></a>Mõne ExpressRoute ringi virtuaalse võrgu linkimine

> [AZURE.SELECTOR]
- [Azure'i portaal - ressursihaldur](expressroute-howto-linkvnet-portal-resource-manager.md)
- [PowerShelli - ressursihaldur](expressroute-howto-linkvnet-arm.md)
- [PowerShelli – klassikaline](expressroute-howto-linkvnet-classic.md)


See artikkel aitab teil virtuaalne võrkude (VNets) link Azure'i ExpressRoute topoloogia ressursihaldur juurutamise mudeli ja PowerShelli abil. Virtuaalne võrkude võib olla sama tellimuse või mõne muu tellimuse osa.

**Azure'i juurutamise mudelite kohta**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="configuration-prerequisites"></a>Konfiguratsiooni eeltingimused

- Teil on vaja Azure PowerShelli moodulid uusim versioon (vähemalt versiooni 1.0). Vaadake, [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md) installimise PowerShelli cmdlet-käskude kohta lisateavet.
- Peate läbi vaadata enne alustamist konfiguratsiooni [eeltingimused](expressroute-prerequisites.md), [marsruutimise nõuded](expressroute-routing.md)ja [töövood](expressroute-workflows.md) .
- Peab teil on aktiivne ExpressRoute ringi. 
    - Järgige juhiseid, et [luua mõne ExpressRoute ringi](expressroute-howto-circuit-arm.md) ja on lubatud ühenduvuse pakkuja ringi. 
    - Veenduge, et on Azure privaatne silmitsemine oma ringi jaoks konfigureeritud. Artiklist [konfigureerimine marsruutimine](expressroute-howto-routing-arm.md) marsruutimise juhised. 
    - Veenduge, et Azure'i privaatne silmitsemine on konfigureeritud ja BGP silmitsemine vahel oma võrgu ja Microsoft on üles, nii et saate lubada lõpuni Ühenduvus.
    - Veenduge, et teil on virtuaalse võrgu ja virtuaalse võrgu lüüsi loodud ja täielikult ette valmistatud. Järgige juhiseid, et luua [VPN-lüüsi](../articles/vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md), kuid veenduge, et kasutada `-GatewayType ExpressRoute`.

Kuni 10 virtuaalse võrgu saate linkida standard ExpressRoute ringi. Kõik virtuaalne võrkude peab olema geopoliitiliste piirkonna standard ExpressRoute ringi kasutamisel. 

Link virtuaalne võrkude väljaspool ExpressRoute topoloogia geopoliitiline piirkond või suurema hulga virtuaalne võrkude ühendamine oma ExpressRoute ringi kui märkisite ExpressRoute premium lisandmoodul. Märkige ruut [FAQ](expressroute-faqs.md) premium lisandmooduli kohta lisateabe saamiseks.

## <a name="connect-a-virtual-network-in-the-same-subscription-to-a-circuit"></a>Ühenduse loomine virtuaalse võrgu sama tellimuse soovitud ringi

Saate luua ühenduse mõne ExpressRoute ringi virtuaalse võrgu lüüsi abil järgmine cmdlet-käsk. Veenduge, et virtuaalse võrgu lüüsi on loodud ja on valmis linkimise enne, kui käivitate cmdlet:

    $circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
    $gw = Get-AzureRmVirtualNetworkGateway -Name "ExpressRouteGw" -ResourceGroupName "MyRG"
    $connection = New-AzureRmVirtualNetworkGatewayConnection -Name "ERConnection" -ResourceGroupName "MyRG" -Location "East US" -VirtualNetworkGateway1 $gw -PeerId $circuit.Id -ConnectionType ExpressRoute

## <a name="connect-a-virtual-network-in-a-different-subscription-to-a-circuit"></a>Ühenduse lisamine ringi virtuaalse võrgu eri tellimus

Saate ühiskasutusse anda ka ExpressRoute ringi üle mitu tellimust. Järgmisel joonisel on lihtne, kuidas ühiskasutuse toimib ExpressRoute topoloogia skemaatiline üle mitu tellimust.

Iga väiksem pilved suure pilve väljendamiseks kasutatakse tellimused, mis kuuluvad erinevate osakondade ettevõttes. Kõigi osakondade teie asutuses saate kasutada oma tellimuse juurutamine nende teenuste – kuid nad ühe ExpressRoute ringi tagasi oma kohapealse võrguga ühenduse loomiseks saate ühiskasutusse anda. Ühe osakonna (selles näites: see) saate oma ExpressRoute ringi. Muude tellimuste teie asutuses saate kasutada ExpressRoute ringi.

>[AZURE.NOTE] Ühenduvus ja läbilaskevõime tasud sihtotstarbeline ringi rakendatakse ExpressRoute ringi omanik. Kõik virtuaalse võrgu jagada sama ribalaiust.

![Rist-tellimuse Ühenduvus](./media/expressroute-howto-linkvnet-classic/cross-subscription.png)

### <a name="administration"></a>Haldus

*Ringi omanik* on lubatud power kasutaja ExpressRoute ringi ressursi. Ringi omanik saab luua lube, mida saab lunastada *ringi kasutajad*. *Ringi kasutajad* on omanikud virtuaalse võrgu lüüsi (mis ei kuulu sama tellimuse ExpressRoute ringi nimega). *Ringi kasutajad* saavad lunastada lube (ühe loa kohta).

*Ringi omanik* on õigus muuta ja lube igal ajal tühistada. Tühistamise autoriseerimine tulemuseks kõik link ühendused kustutamise tellimusest, kelle juurdepääs on tühistatud.

### <a name="circuit-owner-operations"></a>Ringi omanik toimingud 

#### <a name="creating-an-authorization"></a>Luba loomine
    
Ringi omanik loob luba. Selle tulemusena autoriseerimine klahvi, mida saab kasutada ringi kasutaja oma virtuaalse võrgu lüüsid, et ExpressRoute ringi ühenduse loomine. Luba kehtib ainult üks ühendus.

Järgmised cmdlet-käsk koodilõigu näitab, kuidas luua luba:

    $circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
    Add-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit -Name "MyAuthorization1"
    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $circuit

        $circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
    $auth1 = Get-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit -Name "MyAuthorization1"
        

Vastuse see sisaldab võtme autoriseerimine ja olek:

    Name                   : MyAuthorization1
    Id                     : /subscriptions/&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&/resourceGroups/ERCrossSubTestRG/providers/Microsoft.Network/expressRouteCircuits/CrossSubTest/authorizations/MyAuthorization1
    Etag                   : &&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&& 
    AuthorizationKey       : ####################################
    AuthorizationUseStatus : Available
    ProvisioningState      : Succeeded

        

#### <a name="reviewing-authorizations"></a>Lube läbivaatamine

Ringi omanik saate vaadata kõik lube kindla poolt välja antud töötab järgmine cmdlet-käsk:

    $circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
    $authorizations = Get-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit
    

#### <a name="adding-authorizations"></a>Luba lisamine

Ringi omanik saab lisada lube abil järgmine cmdlet-käsk:

    $circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
    Add-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit -Name "MyAuthorization2"
    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $circuit
    
    $circuit = Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"
    $authorizations = Get-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit

    
#### <a name="deleting-authorizations"></a>Luba kustutamine

Ringi omanik saab Tühista/Kustuta kasutaja lube käitades järgmine cmdlet-käsk:

    Remove-AzureRmExpressRouteCircuitAuthorization -Name "MyAuthorization2" -ExpressRouteCircuit $circuit
    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $circuit    

### <a name="circuit-user-operations"></a>Ringi kasutaja toimingud

Ringi kasutaja peab partneri ID ja mõne autoriseerimine võti ringi omanik. Luba oluline on GUID.

Partneri ID on, saate kontrollida järgmine käsk.

    Get-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "MyRG"

#### <a name="redeeming-connection-authorizations"></a>Lunastamise ühenduse lube

Ringi kasutaja saab käitada lunastamiseks linki autoriseerimine järgmine cmdlet-käsk:

    $id = "/subscriptions/********************************/resourceGroups/ERCrossSubTestRG/providers/Microsoft.Network/expressRouteCircuits/MyCircuit"  
    $gw = Get-AzureRmVirtualNetworkGateway -Name "ExpressRouteGw" -ResourceGroupName "MyRG"
    $connection = New-AzureRmVirtualNetworkGatewayConnection -Name "ERConnection" -ResourceGroupName "RemoteResourceGroup" -Location "East US" -VirtualNetworkGateway1 $gw -PeerId $id -ConnectionType ExpressRoute -AuthorizationKey "^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^"

#### <a name="releasing-connection-authorizations"></a>Ühenduse lube vabastamine

Luba saate linke ExpressRoute ringi virtuaalse võrguga ühenduse kustutades vabastada.

## <a name="next-steps"></a>Järgmised sammud

ExpressRoute kohta leiate lisateavet teemast [ExpressRoute KKK](expressroute-faqs.md).
