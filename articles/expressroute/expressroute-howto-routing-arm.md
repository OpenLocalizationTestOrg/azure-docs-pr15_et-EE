<properties
   pageTitle="Kuidas konfigureerida marsruutimine on ExpressRoute ringi | Microsoft Azure'i"
   description="Selles artiklis tutvustatakse loomise ja privaatne, avalik ja Microsoft silmitsemine ExpressRoute topoloogia ettevalmistamise juhiseid. Selles artiklis samuti näitab, kuidas oleku, värskendada ja kustutada teie ringi jaoks peerings."
   documentationCenter="na"
   services="expressroute"
   authors="ganesr"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="hero-article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/05/2016"
   ms.author="ganesr"/>

# <a name="create-and-modify-routing-for-an-expressroute-circuit"></a>Luua ja muuta mõne ExpressRoute ringi marsruutimine


> [AZURE.SELECTOR]
[Azure'i portaal - ressursihaldur](expressroute-howto-routing-portal-resource-manager.md)
[PowerShelli - ressursihaldur](expressroute-howto-routing-arm.md)
[PowerShelli – klassikaline](expressroute-howto-routing-classic.md)



Selles artiklis tutvustatakse luua ja hallata marsruutimine konfigureerimine ExpressRoute ringi, mis PowerShelli ja Azure ressursihaldur juurutamise mudeli kasutamise juhiseid.  Alltoodud juhiseid ka näitab teile, kuidas kontrollida olek, värskendamine või kustutamine ja deprovision peerings jaoks soovitud ExpressRoute ringi. 


**Azure'i juurutamise mudelite kohta**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="configuration-prerequisites"></a>Konfiguratsiooni eeltingimused

- Peate uusima versiooni Azure PowerShelli moodulid, 1.0 või uuem versioon. 
- Veenduge, et teil vaadata lehe [eeltingimused](expressroute-prerequisites.md) , lehe [marsruutimine nõuded](expressroute-routing.md) ja [töövoogude](expressroute-workflows.md) lehelt enne alustamist konfigureerimine.
- Peab teil on aktiivne ExpressRoute ringi. Järgige loomine [ExpressRoute soovitud ringi](expressroute-howto-circuit-arm.md) ja on lubatud ühenduvuse pakkuja enne jätkamist ringi. Ringi ExpressRoute peab olema ettevalmistatud ja lubatud olekus, et teil oleks võimalik allpool kirjeldatud käitamine.

Need juhised kehtivad ainult elektriskeemide loodud pakub kiht 2 connectivity services teenusepakkujatele. Kui kasutate teenuse pakkuja hallatavad Layer 3 teenuseid (tavaliselt on IPVPN, nt MPLS), pakkuja Ühenduvus konfigureerimine ja haldamine saate marsruutimist.

>[AZURE.IMPORTANT] Me praegu reklaamida peerings konfigureeritud teenusepakkujatele teenuse haldusportaali kaudu. Me tegeleme selle võimalus kiiresti lubamine. Kontrollige oma teenusepakkujalt enne BGP peerings konfigureerimine.

Saate konfigureerida ühest, kahest või kõigi kolme peerings (Azure'i privaatne, Azure avalik ja Microsofti) jaoks mõne ExpressRoute ringi. Saate konfigureerida peerings valite järjekorras. Aga peab veenduge, et lõpuleviimist konfiguratsiooni iga silmitsemine ühe korraga. 

## <a name="azure-private-peering"></a>Azure'i privaatne silmitsemine

Selles jaotises antakse juhiseid kohta, kuidas luua, saada, värskendada ja kustutada mõne ExpressRoute ringi Azure privaatne silmitsemine konfigureerimine. 

### <a name="to-create-azure-private-peering"></a>Azure'i privaatne silmitsemine loomiseks

1. Importida PowerShelli mooduli ExpressRoute jaoks.
    
    Peate installima uusima PowerShelli Installeri [PowerShelli](http://www.powershellgallery.com/) galeriist ja Azure ressursihaldur moodulid importimine PowerShelli seansi alustamiseks ExpressRoute cmdlet-käskude abil. Peate käivitage PowerShelli administraatorina.

        Install-Module AzureRM

        Install-AzureRM

    Kõik AzureRM.* moodulid teadaoleva semantilise versiooni vahemikus importimine

        Import-AzureRM

    Saate importida ka lihtsalt valige mooduli vahemikus teadaolevad semantilise versioon 
        
        Import-Module AzureRM.Network 

    Oma kontosse sisselogimine

        Login-AzureRmAccount

    Valige tellimus, mida soovite luua ExpressRoute ringi
        
        Select-AzureRmSubscription -SubscriptionId "<subscription ID>"

2. Saate luua ka ExpressRoute ringi.
    
    Järgige juhiseid, et luua mõne [ExpressRoute ringi](expressroute-howto-circuit-arm.md) ja on selle ette valmistatud ühenduvuse pakkuja. 

    Kui teie ühenduvuse pakkuja pakub haldab Layer 3, saate taotleda lubamiseks Azure privaatne silmitsemine teile pakkuja Ühenduvus. Sel juhul pole vaja järgmises jaotises toodud juhiseid. Juhul, kui pakkuja ühenduvuse hallata, saate oma ringi loomise järel marsruutimine järgige alltoodud juhiseid. 

3. Märkige ruut ExpressRoute ringi, et tagada, et see on ette valmistatud.

    Esmalt peate kontrollima, kas ExpressRoute ringi on ette valmistatud ja ka lubatud. Vt järgmist näidet.

        Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    Vastus on midagi sarnast järgmises näites:

        Name                             : ExpressRouteARMCircuit
        ResourceGroupName                : ExpressRouteResourceGroup
        Location                         : westus
        Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
        Etag                             : W/"################################"
        ProvisioningState                : Succeeded
        Sku                              : {
                                             "Name": "Standard_MeteredData",
                                             "Tier": "Standard",
                                             "Family": "MeteredData"
                                           }
        CircuitProvisioningState         : Enabled
        ServiceProviderProvisioningState : Provisioned
        ServiceProviderNotes             : 
        ServiceProviderProperties        : {
                                             "ServiceProviderName": "Equinix",
                                             "PeeringLocation": "Silicon Valley",
                                             "BandwidthInMbps": 200
                                           }
        ServiceKey                       : **************************************
        Peerings                         : []


4. Konfigureerida Azure privaatne silmitsemine ringi.

    Veenduge, et enne jätkamist järgmiste juhiste abil, et teil on järgmised üksused:

    - Mõne /30 alamvõrgu esmane lingi. See ei tohi olla mis tahes aadressiruumi jaoks virtuaalse võrgu osa.
    - Mõne /30 alamvõrgu teisene lingi. See ei tohi olla mis tahes aadressiruumi jaoks virtuaalse võrgu osa.
    - Lubatud VLAN ID luua silmitsemine kohta. Veenduge, et ei ole muud silmitsemine sisse ringi kasutab sama VLAN ID.
    - Kui arvu silmitsemine. Saate kasutada nii 2-baidine ja 4-baidine ARVUDENA. Saate kasutada privaatse jaoks selle silmitsemine arvuna. Veenduge, et te ei kasuta 65515.
    - Mõne MD5 räsi kui kasutate ühte. **See pole kohustuslik**.
    
    Saate käivitage järgmine cmdlet-konfigureerida Azure privaatne silmitsemine oma ringi.

        Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -Circuit $ckt -PeeringType AzurePrivatePeering -PeerASN 100 -PrimaryPeerAddressPrefix "10.0.0.0/30" -SecondaryPeerAddressPrefix "10.0.0.4/30" -VlanId 200

        Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

    Kui kasutate mõnda MD5 räsi, saate allpool cmdlet-käsk.

        Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -Circuit $ckt -PeeringType AzurePrivatePeering -PeerASN 100 -PrimaryPeerAddressPrefix "10.0.0.0/30" -SecondaryPeerAddressPrefix "10.0.0.4/30" -VlanId 200  -SharedKey "A1B2C3D4"

        Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

    >[AZURE.IMPORTANT] Veenduge, et määrata oma AS number silmitsemine ASN kliendi ASN.

### <a name="to-view-azure-private-peering-details"></a>Azure'i privaatne silmitsemine üksikasjade vaatamiseks

Saate konfiguratsiooni üksikasjad abil järgmine cmdlet-käsk

        $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

        Get-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -Circuit $ckt   


### <a name="to-update-azure-private-peering-configuration"></a>Azure'i privaatne silmitsemine konfiguratsiooni värskendamine

Saate värskendada mis tahes osa konfiguratsiooni abil järgmine cmdlet-käsk. Järgmises näites, värskendatakse ringi VLAN ID-d 100 – 500.

    Set-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt -PeeringType AzurePrivatePeering -PeerASN 100 -PrimaryPeerAddressPrefix "10.0.0.0/30" -SecondaryPeerAddressPrefix "10.0.0.4/30" -VlanId 200

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt


### <a name="to-delete-azure-private-peering"></a>Azure'i privaatne silmitsemine kustutamine

Saate eemaldada silmitsemine konfiguratsioonist, käitades järgmine cmdlet-käsk.

>[AZURE.WARNING] Teil peab tagada kõik virtuaalse võrgu kaudu ExpressRoute ringi linkimata enne cmdlet-käsu käitamine. 

    Remove-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt
    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt



## <a name="azure-public-peering"></a>Azure'i avaliku silmitsemine

Selles jaotises antakse juhiseid kohta, kuidas luua, saada, värskendada ja kustutada mõne ExpressRoute ringi Azure avaliku silmitsemine konfigureerimine.

### <a name="to-create-azure-public-peering"></a>Azure'i avaliku silmitsemine loomiseks

1. Importida PowerShelli mooduli ExpressRoute jaoks.
    
    Peate installima uusima PowerShelli Installeri [PowerShelli](http://www.powershellgallery.com/) galeriist ja Azure ressursihaldur moodulid importimine PowerShelli seansi alustamiseks ExpressRoute cmdlet-käskude abil. Peate käivitage PowerShelli administraatorina.

        Install-Module AzureRM

        Install-AzureRM

    Kõik AzureRM.* moodulid teadaoleva semantilise versiooni vahemikus importimine

        Import-AzureRM

    Saate importida ka lihtsalt valige mooduli vahemikus teadaolevad semantilise versioon 
        
        Import-Module AzureRM.Network 

    Oma kontosse sisselogimine

        Login-AzureRmAccount

    Valige tellimus, mida soovite luua ExpressRoute ringi
        
        Select-AzureRmSubscription -SubscriptionId "<subscription ID>"

2. Saate luua ka ExpressRoute ringi.
    
    Järgige juhiseid, et luua mõne [ExpressRoute ringi](expressroute-howto-circuit-arm.md) ja on selle ette valmistatud ühenduvuse pakkuja. 

    Kui teie ühenduvuse pakkuja pakub haldab Layer 3, saate taotleda lubamiseks Azure avaliku silmitsemine teile pakkuja Ühenduvus. Sel juhul pole vaja järgmises jaotises toodud juhiseid. Juhul, kui pakkuja ühenduvuse hallata, saate oma ringi loomise järel marsruutimine järgige alltoodud juhiseid.

3. Märkige ruut ExpressRoute ringi, et tagada, et see on ette valmistatud.

    Esmalt peate kontrollima, kas ExpressRoute ringi on ette valmistatud ja ka lubatud. Vt järgmist näidet.

        Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    Vastus on midagi sarnast järgmises näites:

        Name                             : ExpressRouteARMCircuit
        ResourceGroupName                : ExpressRouteResourceGroup
        Location                         : westus
        Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
        Etag                             : W/"################################"
        ProvisioningState                : Succeeded
        Sku                              : {
                                             "Name": "Standard_MeteredData",
                                             "Tier": "Standard",
                                             "Family": "MeteredData"
                                           }
        CircuitProvisioningState         : Enabled
        ServiceProviderProvisioningState : Provisioned
        ServiceProviderNotes             : 
        ServiceProviderProperties        : {
                                             "ServiceProviderName": "Equinix",
                                             "PeeringLocation": "Silicon Valley",
                                             "BandwidthInMbps": 200
                                           }
        ServiceKey                       : **************************************
        Peerings                         : []   

4. Konfigureerida Azure avaliku silmitsemine ringi.

    Veenduge, et enne jätkamist täpsemaks on järgmine teave.

    - Mõne /30 alamvõrgu esmane lingi. See peab olema lubatud avaliku IPv4 eesliite.
    - Mõne /30 alamvõrgu teisene lingi. See peab olema lubatud avaliku IPv4 eesliite.
    - Lubatud VLAN ID luua silmitsemine kohta. Veenduge, et ei ole muud silmitsemine sisse ringi kasutab sama VLAN ID.
    - Kui arvu silmitsemine. Saate kasutada nii 2-baidine ja 4-baidine ARVUDENA.
    - Mõne MD5 räsi kui kasutate ühte. **See pole kohustuslik**.
    
    Saate käivitage järgmine cmdlet-Azure avaliku silmitsemine oma ringi konfigureerimine

        Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt -PeeringType AzurePublicPeering -PeerASN 100 -PrimaryPeerAddressPrefix "12.0.0.0/30" -SecondaryPeerAddressPrefix "12.0.0.4/30" -VlanId 100

        Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

    Kui kasutate mõnda MD5 räsi saate kasutada cmdlet allpool

        Add-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt -PeeringType AzurePublicPeering -PeerASN 100 -PrimaryPeerAddressPrefix "12.0.0.0/30" -SecondaryPeerAddressPrefix "12.0.0.4/30" -VlanId 100  -SharedKey "A1B2C3D4"

        Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt


    >[AZURE.IMPORTANT] Veenduge, et teie määratud silmitsemine ASN ja klientide ASN oma AS number.

### <a name="to-view-azure-public-peering-details"></a>Azure'i avaliku silmitsemine üksikasjade vaatamiseks

Saate konfiguratsiooni üksikasjad abil järgmine cmdlet-käsk

        $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

        Get-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -Circuit $ckt


### <a name="to-update-azure-public-peering-configuration"></a>Azure'i avaliku silmitsemine konfiguratsiooni värskendamine

Saate värskendada mis tahes osa konfiguratsiooni abil järgmine cmdlet-käsk

    Set-AzureRmExpressRouteCircuitPeeringConfig  -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt -PeeringType MicrosoftPeering -PeerASN 100 -PrimaryPeerAddressPrefix "123.0.0.0/30" -SecondaryPeerAddressPrefix "123.0.0.4/30" -VlanId 600 

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

Ringi VLAN ID-d värskendatakse 200 600 eeltoodud näites.

### <a name="to-delete-azure-public-peering"></a>Azure'i avaliku silmitsemine kustutamine

Saate eemaldada silmitsemine konfiguratsioonist, käitades järgmine cmdlet-käsk

    Remove-AzureRmExpressRouteCircuitPeeringConfig -Name "AzurePublicPeering" -ExpressRouteCircuit $ckt
    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

## <a name="microsoft-peering"></a>Microsoft silmitsemine

Selles jaotises antakse juhiseid kohta, kuidas luua, saada, värskendada ja kustutada Microsoft silmitsemine konfigureerimine on ExpressRoute ringi. 

### <a name="to-create-microsoft-peering"></a>Microsoft silmitsemine loomiseks

1. Importida PowerShelli mooduli ExpressRoute jaoks.
    
    Peate installima uusima PowerShelli Installeri [PowerShelli](http://www.powershellgallery.com/) galeriist ja Azure ressursihaldur moodulid importimine PowerShelli seansi alustamiseks ExpressRoute cmdlet-käskude abil. Peate käivitage PowerShelli administraatorina.

        Install-Module AzureRM

        Install-AzureRM

    Kõik AzureRM.* moodulid teadaoleva semantilise versiooni vahemikus importimine

        Import-AzureRM

    Saate importida ka lihtsalt valige mooduli vahemikus teadaolevad semantilise versioon 
        
        Import-Module AzureRM.Network 

    Oma kontosse sisselogimine

        Login-AzureRmAccount

    Valige tellimus, mida soovite luua ExpressRoute ringi
        
        Select-AzureRmSubscription -SubscriptionId "<subscription ID>"

2. Saate luua ka ExpressRoute ringi.
    
    Järgige juhiseid, et luua mõne [ExpressRoute ringi](expressroute-howto-circuit-arm.md) ja on selle ette valmistatud ühenduvuse pakkuja. 

    Kui teie ühenduvuse pakkuja pakub haldab Layer 3, saate taotleda lubamiseks Azure privaatne silmitsemine teile pakkuja Ühenduvus. Sel juhul pole vaja järgmises jaotises toodud juhiseid. Juhul, kui pakkuja ühenduvuse hallata, saate oma ringi loomise järel marsruutimine järgige alltoodud juhiseid.

3. Märkige ruut ExpressRoute ringi, et tagada, et see on ette valmistatud.

    Esmalt peate kontrollima, kas ExpressRoute ringi on ette valmistatud ja ka lubatud. Vt järgmist näidet.

        Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    Vastus on midagi sarnast järgmises näites:

        Name                             : ExpressRouteARMCircuit
        ResourceGroupName                : ExpressRouteResourceGroup
        Location                         : westus
        Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
        Etag                             : W/"################################"
        ProvisioningState                : Succeeded
        Sku                              : {
                                             "Name": "Standard_MeteredData",
                                             "Tier": "Standard",
                                             "Family": "MeteredData"
                                           }
        CircuitProvisioningState         : Enabled
        ServiceProviderProvisioningState : Provisioned
        ServiceProviderNotes             : 
        ServiceProviderProperties        : {
                                             "ServiceProviderName": "Equinix",
                                             "PeeringLocation": "Silicon Valley",
                                             "BandwidthInMbps": 200
                                           }
        ServiceKey                       : **************************************
        Peerings                         : []   
4. Microsoft silmitsemine ringi jaoks konfigureerida.

    Veenduge, et enne jätkamist, et teil on järgmine teave.

    - Mõne /30 alamvõrgu esmane lingi. See peab olema lubatud avaliku IPv4 eesliite kuuluv ja registreeritud mõne RIR / IRR.
    - Mõne /30 alamvõrgu teisene lingi. See peab olema lubatud avaliku IPv4 eesliite kuuluv ja registreeritud mõne RIR / IRR.
    - Lubatud VLAN ID luua silmitsemine kohta. Veenduge, et ei ole muud silmitsemine sisse ringi kasutab sama VLAN ID.
    - Kui arvu silmitsemine. Saate kasutada nii 2-baidine ja 4-baidine ARVUDENA.
    - Reklaamida eesliiteid: peate sisestama plaanite reklaamida BGP seansi üle kõik eesliiteid loendit. Ainult avaliku IP address eesliiteid aktsepteeritakse. Kui kavatsete saata eesliiteid kogumi, saate saata komaga eraldatud loend. Nende eesliidete tähised tuleb registreerida teile mõne RIR / IRR.
    - Kliendi ASN: Kui olete reklaami eesliidete tähised, mis pole registreeritud silmitsemine arvuna, saate määrata AS arvu, mis on registreeritud. **See pole kohustuslik**.
    - Marsruutimise registri nimi: Saate määrata selle RIR / IRR, mille vastu seda arvu ja eesliidete tähised on juba registreeritud.
    - MD5 räsi, kui kasutate ühte. **See pole kohustuslik.**
    
    Saate käivitage järgmine cmdlet-konfigureerida Microsoft silmitsemine oma ringi jaoks

        Add-AzureRmExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt -PeeringType MicrosoftPeering -PeerASN 100 -PrimaryPeerAddressPrefix "123.0.0.0/30" -SecondaryPeerAddressPrefix "123.0.0.4/30" -VlanId 300 -MicrosoftConfigAdvertisedPublicPrefixes "123.1.0.0/24" -MicrosoftConfigCustomerAsn 23 -MicrosoftConfigRoutingRegistryName "ARIN"

        Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt


### <a name="to-get-microsoft-peering-details"></a>Microsoft silmitsemine üksikasjade saamiseks

Saate konfiguratsiooni üksikasjad abil järgmine cmdlet-käsk.

        $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

        Get-AzureRmExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt


### <a name="to-update-microsoft-peering-configuration"></a>Värskendada Microsoft silmitsemine konfigureerimine

Saate värskendada mis tahes osa konfiguratsiooni abil järgmine cmdlet-käsk.

        Set-AzureRmExpressRouteCircuitPeeringConfig  -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt -PeeringType MicrosoftPeering -PeerASN 100 -PrimaryPeerAddressPrefix "123.0.0.0/30" -SecondaryPeerAddressPrefix "123.0.0.4/30" -VlanId 300 -MicrosoftConfigAdvertisedPublicPrefixes "124.1.0.0/24" -MicrosoftConfigCustomerAsn 23 -MicrosoftConfigRoutingRegistryName "ARIN"

        Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt
        

### <a name="to-delete-microsoft-peering"></a>Microsoft silmitsemine kustutamine

Saate eemaldada silmitsemine konfiguratsioonist, käitades järgmine cmdlet-käsk.

    Remove-AzureRmExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

## <a name="next-steps"></a>Järgmised sammud

Järgmise juhise juurde: [lingi lisamine VNet, et mõne ExpressRoute ringi](expressroute-howto-linkvnet-arm.md).

-  ExpressRoute töövoogude kohta leiate lisateavet teemast [ExpressRoute töövood](expressroute-workflows.md).

-  Ringi silmitsemine kohta leiate lisateavet teemast [ExpressRoute ahelatega ja marsruutimise domeenid](expressroute-circuit-peerings.md).

-  Virtuaalne võrkude töötamise kohta leiate lisateavet teemast [Virtual võrgu ülevaade](../virtual-network/virtual-networks-overview.md).

