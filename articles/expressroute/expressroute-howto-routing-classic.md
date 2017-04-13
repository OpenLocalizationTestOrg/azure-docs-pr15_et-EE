<properties
   pageTitle="Kuidas konfigureerida marsruutimine on ExpressRoute ringi klassikaline juurutamise mudeli PowerShelli kaudu | Microsoft Azure'i"
   description="Selles artiklis tutvustatakse loomise ja privaatne, avalik ja Microsoft silmitsemine ExpressRoute topoloogia ettevalmistamise juhiseid. Selles artiklis samuti näitab, kuidas oleku, värskendada ja kustutada teie ringi jaoks peerings."
   documentationCenter="na"
   services="expressroute"
   authors="ganesr"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="ganesr"/>

# <a name="create-and-modify-routing-for-an-expressroute-circuit"></a>Luua ja muuta mõne ExpressRoute ringi marsruutimine

> [AZURE.SELECTOR]
[Azure'i portaal - ressursihaldur](expressroute-howto-routing-portal-resource-manager.md)
[PowerShelli - ressursihaldur](expressroute-howto-routing-arm.md)
[PowerShelli – klassikaline](expressroute-howto-routing-classic.md)



Selles artiklis tutvustatakse luua ja hallata marsruutimine konfigureerimine ExpressRoute ringi, mis PowerShelli ja klassikaline juurutamise mudeli kasutamise juhiseid. Alltoodud juhiseid ka näitab teile, kuidas kontrollida olek, värskendamine või kustutamine ja deprovision peerings jaoks soovitud ExpressRoute ringi.


**Azure'i juurutamise mudelite kohta**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="configuration-prerequisites"></a>Konfiguratsiooni eeltingimused

- Peate Azure PowerShelli moodulid uusim versioon. Saate alla laadida uusima PowerShelli mooduli [Azure'i allalaadimislehelt](https://azure.microsoft.com/downloads/)PowerShelli osa. Järgige üksikasjalikud juhised [, kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md) lehel kohta, kuidas konfigureerida oma arvuti Azure PowerShelli moodulid kasutamiseks. 
- Veenduge, et teil vaadata lehe [eeltingimused](expressroute-prerequisites.md) , lehe [marsruutimine nõuded](expressroute-routing.md) ja [töövoogude](expressroute-workflows.md) lehelt enne alustamist konfigureerimine.
- Peab teil on aktiivne ExpressRoute ringi. Järgige juhiseid, et [luua mõne ExpressRoute ringi](expressroute-howto-circuit-classic.md) ja on lubatud ühenduvuse pakkuja enne jätkamist ringi. Ringi ExpressRoute peab olema ettevalmistatud ja lubatud olekus, et teil oleks võimalik allpool kirjeldatud käitamine.

>[AZURE.IMPORTANT] Need juhised kehtivad ainult elektriskeemide loodud pakub kiht 2 connectivity services teenusepakkujatele. Kui kasutate teenuse pakkuja hallatavad Layer 3 teenuseid (tavaliselt on IPVPN, nt MPLS), pakkuja Ühenduvus konfigureerimine ja haldamine saate marsruutimist.

Saate konfigureerida ühest, kahest või kõigi kolme peerings (Azure'i privaatne, Azure avalik ja Microsofti) jaoks mõne ExpressRoute ringi. Saate konfigureerida peerings valite järjekorras. Siiski saate peab veenduge, et, et iga silmitsemine ühe korraga konfiguratsiooni. 

## <a name="azure-private-peering"></a>Azure'i privaatne silmitsemine

Selles jaotises antakse juhiseid kohta, kuidas luua, saada, värskendada ja kustutada mõne ExpressRoute ringi Azure privaatne silmitsemine konfigureerimine. 

### <a name="to-create-azure-private-peering"></a>Azure'i privaatne silmitsemine loomiseks

1. **Importida PowerShelli mooduli ExpressRoute jaoks.**
    
    ExpressRoute cmdlet-käskude kasutamine peab Azure ja ExpressRoute moodulid importida PowerShelli seanss. Käivitage järgmised käsud Azure ja ExpressRoute moodulid importida PowerShelli seanss.  

        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'

2. **Saate luua ka ExpressRoute ringi.**
    
    Järgige juhiseid, et luua mõne [ExpressRoute ringi](expressroute-howto-circuit-classic.md) ja on selle ette valmistatud ühenduvuse pakkuja. Kui teie ühenduvuse pakkuja pakub haldab Layer 3, saate taotleda lubamiseks Azure privaatne silmitsemine teile pakkuja Ühenduvus. Sel juhul pole vaja järgmises jaotises toodud juhiseid. Juhul, kui pakkuja ühenduvuse hallata, saate oma ringi loomise järel marsruutimine järgige alltoodud juhiseid. 

3. **Märkige ruut ExpressRoute ringi, et tagada, et see on ette valmistatud.**

    Esmalt peate kontrollima, kas ExpressRoute ringi on ette valmistatud ja ka lubatud. Vt järgmist näidet.

        PS C:\> Get-AzureDedicatedCircuit -ServiceKey "*********************************"

        Bandwidth                        : 200
        CircuitName                      : MyTestCircuit
        Location                         : Silicon Valley
        ServiceKey                       : *********************************
        ServiceProviderName              : equinix
        ServiceProviderProvisioningState : Provisioned
        Sku                              : Standard
        Status                           : Enabled

    Veenduge, et ringi kuvatakse Provisioned ja lubatud. Kui seda pole, saate töötada saada vajalik riigi-ja olek teie ringi pakkuja Ühenduvus.

        ServiceProviderProvisioningState : Provisioned
        Status                           : Enabled


4. **Konfigureerida Azure privaatne silmitsemine ringi.**

    Veenduge, et enne jätkamist järgmiste juhiste abil, et teil on järgmised üksused:

    - Mõne /30 alamvõrgu esmane lingi. See ei tohi olla mis tahes aadressiruumi jaoks virtuaalse võrgu osa.
    - Mõne /30 alamvõrgu teisene lingi. See ei tohi olla mis tahes aadressiruumi jaoks virtuaalse võrgu osa.
    - Lubatud VLAN ID luua silmitsemine kohta. Veenduge, et ei ole muud silmitsemine sisse ringi kasutab sama VLAN ID.
    - Kui arvu silmitsemine. Saate kasutada nii 2-baidine ja 4-baidine ARVUDENA. Saate kasutada privaatse jaoks selle silmitsemine arvuna. Veenduge, et te ei kasuta 65515.
    - Mõne MD5 räsi kui kasutate ühte. **See pole kohustuslik**.
    
    Saate käivitage järgmine cmdlet-konfigureerida Azure privaatne silmitsemine oma ringi.

        New-AzureBGPPeering -AccessType Private -ServiceKey "*********************************" -PrimaryPeerSubnet "10.0.0.0/30" -SecondaryPeerSubnet "10.0.0.4/30" -PeerAsn 1234 -VlanId 100

    Kui kasutate mõnda MD5 räsi, saate allpool cmdlet-käsk.

        New-AzureBGPPeering -AccessType Private -ServiceKey "*********************************" -PrimaryPeerSubnet "10.0.0.0/30" -SecondaryPeerSubnet "10.0.0.4/30" -PeerAsn 1234 -VlanId 100 -SharedKey "A1B2C3D4"

    >[AZURE.IMPORTANT] Veenduge, et määrata oma AS number silmitsemine ASN kliendi ASN.

### <a name="to-view-azure-private-peering-details"></a>Azure'i privaatne silmitsemine üksikasjade vaatamiseks

Saate konfiguratsiooni üksikasjad abil järgmine cmdlet-käsk

    Get-AzureBGPPeering -AccessType Private -ServiceKey "*********************************"
    
    AdvertisedPublicPrefixes       : 
    AdvertisedPublicPrefixesState  : Configured
    AzureAsn                       : 12076
    CustomerAutonomousSystemNumber : 
    PeerAsn                        : 1234
    PrimaryAzurePort               : 
    PrimaryPeerSubnet              : 10.0.0.0/30
    RoutingRegistryName            : 
    SecondaryAzurePort             : 
    SecondaryPeerSubnet            : 10.0.0.4/30
    State                          : Enabled
    VlanId                         : 100


### <a name="to-update-azure-private-peering-configuration"></a>Azure'i privaatne silmitsemine konfiguratsiooni värskendamine

Saate värskendada mis tahes osa konfiguratsiooni abil järgmine cmdlet-käsk. Järgmises näites, värskendatakse ringi VLAN ID-d 100 – 500.

    Set-AzureBGPPeering -AccessType Private -ServiceKey "*********************************" -PrimaryPeerSubnet "10.0.0.0/30" -SecondaryPeerSubnet "10.0.0.4/30" -PeerAsn 1234 -VlanId 500 -SharedKey "A1B2C3D4"

### <a name="to-delete-azure-private-peering"></a>Azure'i privaatne silmitsemine kustutamine

Saate eemaldada silmitsemine konfiguratsioonist, käitades järgmine cmdlet-käsk.

>[AZURE.WARNING] Teil peab tagada kõik virtuaalse võrgu kaudu ExpressRoute ringi linkimata enne cmdlet-käsu käitamine. 

    Remove-AzureBGPPeering -AccessType Private -ServiceKey "*********************************"


## <a name="azure-public-peering"></a>Azure'i avaliku silmitsemine

Selles jaotises antakse juhiseid kohta, kuidas luua, saada, värskendada ja kustutada mõne ExpressRoute ringi Azure avaliku silmitsemine konfigureerimine.

### <a name="to-create-azure-public-peering"></a>Azure'i avaliku silmitsemine loomiseks

1. **Importida PowerShelli mooduli ExpressRoute jaoks.**
    
    ExpressRoute cmdlet-käskude kasutamine peab Azure ja ExpressRoute moodulid importida PowerShelli seanss. Käivitage järgmised käsud Azure ja ExpressRoute moodulid importida PowerShelli seanss. 

        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'

2. **Mõne ExpressRoute ringi loomine**
    
    Järgige juhiseid, et luua mõne [ExpressRoute ringi](expressroute-howto-circuit-classic.md) ja on selle ette valmistatud ühenduvuse pakkuja. Kui teie ühenduvuse pakkuja pakub haldab Layer 3, saate taotleda lubamiseks Azure avaliku silmitsemine teile pakkuja Ühenduvus. Sel juhul pole vaja järgmises jaotises toodud juhiseid. Juhul, kui pakkuja ühenduvuse hallata, saate oma ringi loomise järel marsruutimine järgige alltoodud juhiseid.

3. **Märkige ruut ExpressRoute ringi, et tagada, et see on ette valmistatud**

    Esmalt peate kontrollima, kas ExpressRoute ringi on ette valmistatud ja ka lubatud. Vt järgmist näidet.

        PS C:\> Get-AzureDedicatedCircuit -ServiceKey "*********************************"

        Bandwidth                        : 200
        CircuitName                      : MyTestCircuit
        Location                         : Silicon Valley
        ServiceKey                       : *********************************
        ServiceProviderName              : equinix
        ServiceProviderProvisioningState : Provisioned
        Sku                              : Standard
        Status                           : Enabled

    Veenduge, et ringi kuvatakse Provisioned ja lubatud. Kui seda pole, saate töötada saada vajalik riigi-ja olek teie ringi pakkuja Ühenduvus.

        ServiceProviderProvisioningState : Provisioned
        Status                           : Enabled

    

4. **Azure'i avaliku silmitsemine ringi konfigureerimine**

    Veenduge, et enne jätkamist täpsemaks on järgmine teave.

    - Mõne /30 alamvõrgu esmane lingi. See peab olema lubatud avaliku IPv4 eesliite.
    - Mõne /30 alamvõrgu teisene lingi. See peab olema lubatud avaliku IPv4 eesliite.
    - Lubatud VLAN ID luua silmitsemine kohta. Veenduge, et ei ole muud silmitsemine sisse ringi kasutab sama VLAN ID.
    - Kui arvu silmitsemine. Saate kasutada nii 2-baidine ja 4-baidine ARVUDENA.
    - Mõne MD5 räsi kui kasutate ühte. **See pole kohustuslik**.
    
    Saate käivitage järgmine cmdlet-Azure avaliku silmitsemine oma ringi konfigureerimine

        New-AzureBGPPeering -AccessType Public -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -PeerAsn 1234 -VlanId 200

    Kui kasutate mõnda MD5 räsi saate kasutada cmdlet allpool

        New-AzureBGPPeering -AccessType Public -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -PeerAsn 1234 -VlanId 200 -SharedKey "A1B2C3D4"

    >[AZURE.IMPORTANT] Veenduge, et teie määratud silmitsemine ASN ja klientide ASN oma AS number.

### <a name="to-view-azure-public-peering-details"></a>Azure'i avaliku silmitsemine üksikasjade vaatamiseks

Saate konfiguratsiooni üksikasjad abil järgmine cmdlet-käsk

    Get-AzureBGPPeering -AccessType Public -ServiceKey "*********************************"
    
    AdvertisedPublicPrefixes       : 
    AdvertisedPublicPrefixesState  : Configured
    AzureAsn                       : 12076
    CustomerAutonomousSystemNumber : 
    PeerAsn                        : 1234
    PrimaryAzurePort               : 
    PrimaryPeerSubnet              : 131.107.0.0/30
    RoutingRegistryName            : 
    SecondaryAzurePort             : 
    SecondaryPeerSubnet            : 131.107.0.4/30
    State                          : Enabled
    VlanId                         : 200


### <a name="to-update-azure-public-peering-configuration"></a>Azure'i avaliku silmitsemine konfiguratsiooni värskendamine

Saate värskendada mis tahes osa konfiguratsiooni abil järgmine cmdlet-käsk

    Set-AzureBGPPeering -AccessType Public -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -PeerAsn 1234 -VlanId 600 -SharedKey "A1B2C3D4"

Ringi VLAN ID-d värskendatakse 200 600 eeltoodud näites.

### <a name="to-delete-azure-public-peering"></a>Azure'i avaliku silmitsemine kustutamine

Saate eemaldada silmitsemine konfiguratsioonist, käitades järgmine cmdlet-käsk

    Remove-AzureBGPPeering -AccessType Public -ServiceKey "*********************************"

## <a name="microsoft-peering"></a>Microsoft silmitsemine

Selles jaotises antakse juhiseid kohta, kuidas luua, saada, värskendada ja kustutada Microsoft silmitsemine konfigureerimine on ExpressRoute ringi. 

### <a name="to-create-microsoft-peering"></a>Microsoft silmitsemine loomiseks

1. **Importida PowerShelli mooduli ExpressRoute jaoks.**
    
    ExpressRoute cmdlet-käskude kasutamine peab Azure ja ExpressRoute moodulid importida PowerShelli seanss. Käivitage järgmised käsud Azure ja ExpressRoute moodulid importida PowerShelli seanss.  

        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
        Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'

2. **Mõne ExpressRoute ringi loomine**
    
    Järgige juhiseid, et luua mõne [ExpressRoute ringi](expressroute-howto-circuit-classic.md) ja on selle ette valmistatud ühenduvuse pakkuja. Kui teie ühenduvuse pakkuja pakub haldab Layer 3, saate taotleda lubamiseks Azure privaatne silmitsemine teile pakkuja Ühenduvus. Sel juhul pole vaja järgmises jaotises toodud juhiseid. Juhul, kui pakkuja ühenduvuse hallata, saate oma ringi loomise järel marsruutimine järgige alltoodud juhiseid.

3. **Märkige ruut ExpressRoute ringi, et tagada, et see on ette valmistatud**

    Esmalt peate kontrollima, kas ExpressRoute ringi on Provisioned ja lubatud olekus.

        PS C:\> Get-AzureDedicatedCircuit -ServiceKey "*********************************"

        Bandwidth                        : 200
        CircuitName                      : MyTestCircuit
        Location                         : Silicon Valley
        ServiceKey                       : *********************************
        ServiceProviderName              : equinix
        ServiceProviderProvisioningState : Provisioned
        Sku                              : Standard
        Status                           : Enabled

    Veenduge, et ringi kuvatakse Provisioned ja lubatud. Kui seda pole, saate töötada saada vajalik riigi-ja olek teie ringi pakkuja Ühenduvus.

        ServiceProviderProvisioningState : Provisioned
        Status                           : Enabled


4. **Microsofti jaoks ringi silmitsemine konfigureerimine**

    Veenduge, et enne jätkamist, et teil on järgmine teave.

    - Mõne /30 alamvõrgu esmane lingi. See peab olema lubatud avaliku IPv4 eesliite kuuluv ja registreeritud mõne RIR / IRR.
    - Mõne /30 alamvõrgu teisene lingi. See peab olema lubatud avaliku IPv4 eesliite kuuluv ja registreeritud mõne RIR / IRR.
    - Lubatud VLAN ID luua silmitsemine kohta. Veenduge, et ei ole muud silmitsemine sisse ringi kasutab sama VLAN ID.
    - Kui arvu silmitsemine. Saate kasutada nii 2-baidine ja 4-baidine ARVUDENA.
    - Reklaamida eesliiteid: peate sisestama plaanite reklaamida BGP seansi üle kõik eesliiteid loendit. Ainult avaliku IP address eesliiteid aktsepteeritakse. Kui kavatsete saata eesliiteid kogumi, saate saata komaga eraldatud loend. Nende eesliidete tähised tuleb registreerida teile mõne RIR / IRR.
    - Kliendi ASN: Kui olete reklaami eesliidete tähised, mis pole registreeritud silmitsemine arvuna, saate määrata AS arvu, mis on registreeritud. **See pole kohustuslik**.
    - Marsruutimise registri nimi: Saate määrata selle RIR / IRR, mille vastu seda arvu ja eesliidete tähised on juba registreeritud.
    - Mõne MD5 räsi, kui kasutate ühte. **See pole kohustuslik.**
    
    Saate käivitage järgmine cmdlet-konfigureerida Microsoft pering oma ringi jaoks

        New-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -VlanId 300 -PeerAsn 1234 -CustomerAsn 2245 -AdvertisedPublicPrefixes "123.0.0.0/30" -RoutingRegistryName "ARIN" -SharedKey "A1B2C3D4"


### <a name="to-view-microsoft-peering-details"></a>Kui soovite Microsoft silmitsemine üksikasjade kuvamine

Saate konfiguratsiooni üksikasjad abil järgmine cmdlet-käsk.

    Get-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************"
    
    AdvertisedPublicPrefixes       : 123.0.0.0/30
    AdvertisedPublicPrefixesState  : Configured
    AzureAsn                       : 12076
    CustomerAutonomousSystemNumber : 2245
    PeerAsn                        : 1234
    PrimaryAzurePort               : 
    PrimaryPeerSubnet              : 10.0.0.0/30
    RoutingRegistryName            : ARIN
    SecondaryAzurePort             : 
    SecondaryPeerSubnet            : 10.0.0.4/30
    State                          : Enabled
    VlanId                         : 300


### <a name="to-update-microsoft-peering-configuration"></a>Värskendada Microsoft silmitsemine konfigureerimine

Saate värskendada mis tahes osa konfiguratsiooni abil järgmine cmdlet-käsk.

        Set-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************" -PrimaryPeerSubnet "131.107.0.0/30" -SecondaryPeerSubnet "131.107.0.4/30" -VlanId 300 -PeerAsn 1234 -CustomerAsn 2245 -AdvertisedPublicPrefixes "123.0.0.0/30" -RoutingRegistryName "ARIN" -SharedKey "A1B2C3D4"

### <a name="to-delete-microsoft-peering"></a>Microsoft silmitsemine kustutamine

Saate eemaldada silmitsemine konfiguratsioonist, käitades järgmine cmdlet-käsk.

    Remove-AzureBGPPeering -AccessType Microsoft -ServiceKey "*********************************"

## <a name="next-steps"></a>Järgmised sammud

Edasi, [Link on VNet, et mõne ExpressRoute ringi](expressroute-howto-linkvnet-classic.md).


-  Töövoogude kohta leiate lisateavet teemast [ExpressRoute töövood](expressroute-workflows.md).
-  Ringi silmitsemine kohta leiate lisateavet teemast [ExpressRoute ahelatega ja marsruutimise domeenid](expressroute-circuit-peerings.md).

