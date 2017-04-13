<properties
   pageTitle="Luua ja muuta mõne ExpressRoute ringi ressursihaldur ja PowerShelli abil | Microsoft Azure'i"
   description="Selles artiklis kirjeldatakse, kuidas luua, ettevalmistamise, kinnitamine, värskendada, kustutada ja deprovision mõne ExpressRoute ringi."
   documentationCenter="na"
   services="expressroute"
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
   ms.author="ganesr"/>


# <a name="create-and-modify-an-expressroute-circuit"></a>Luua ja muuta mõne ExpressRoute ringi

> [AZURE.SELECTOR]
[Azure'i portaal - ressursihaldur](expressroute-howto-circuit-portal-resource-manager.md)
[PowerShelli - ressursihaldur](expressroute-howto-circuit-arm.md)
[PowerShelli – klassikaline](expressroute-howto-circuit-classic.md)


Selles artiklis kirjeldatakse, kuidas luua mõne Azure'i ExpressRoute ringi Windows PowerShelli cmdlet-käskude ja Azure ressursihaldur juurutamise mudeli abil. Selles artiklis ka näitab teile, kuidas oleku ringi, värskendage seda, või kustutada ja deprovision seda.

**Azure'i juurutamise mudelite kohta**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="before-you-begin"></a>Enne alustamist


- Azure'i PowerShelli moodulid uusima versiooni hankimiseks (vähemalt versiooni 1.0). Üksikasjalikud juhised selle kohta, kuidas konfigureerida oma arvuti kasutamiseks PowerShelli moodulid, järgige juhiseid, [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md).

- Enne alustamist konfiguratsiooni, kontrollige [eeltingimused](expressroute-prerequisites.md) ja [töövood](expressroute-workflows.md) .

## <a name="create-and-provision-an-expressroute-circuit"></a>Loomine ja mõne ExpressRoute ringi ettevalmistamine

### <a name="1-sign-in-to-your-azure-account-and-select-your-subscription"></a>1. logige sisse oma Azure'i kontosse ja valige oma tellimuse

Azure'i kontosse sisselogimiseks oma konfiguratsioon alustamiseks. PowerShelli kohta leiate lisateavet teemast [Windows PowerShelli kasutamine koos ressursihaldur](../powershell-azure-resource-manager.md). Järgmised näited abil saate ühendada:

    Login-AzureRmAccount

Kontrollige konto tellimused.

    Get-AzureRmSubscription

Valige tellimus, mida soovite luua ka ExpressRoute ringi jaoks.

    Select-AzureRmSubscription -SubscriptionId "<subscription ID>"

### <a name="2-get-the-list-of-supported-providers-locations-and-bandwidths"></a>2. toetatud pakkujad, asukohad ja andmemahud loendi hankimine

Enne mõne ExpressRoute ringi loomist peate toetatud ühenduvuse pakkujate, asukohad ja läbilaskevõime suvandite loend.

PowerShelli cmdlet-käsk `Get-AzureRmExpressRouteServiceProvider` tagastab see teave, mille abil saate hiljem toimingut:

    Get-AzureRmExpressRouteServiceProvider

Kontrollige, kui seal on loetletud pakkuja Ühenduvus. Märkige järgmine teave üles, sest teil on vaja seda hiljem soovitud ringi loomisel:

- Nimi

- PeeringLocations

- BandwidthsOffered

Nüüd oletegi valmis looma mõne ExpressRoute ringi.   

### <a name="3-create-an-expressroute-circuit"></a>3. mis ExpressRoute ringi loomine

Kui teil pole veel ressursirühma, luua enne luua oma ExpressRoute ringi. Selleks käivitage järgmine käsk:


    New-AzureRmResourceGroup -Name "ExpressRouteResourceGroup" -Location "West US"


Järgmises näites on kujutatud loomine Edenvale ExpressRoute ringi Equinix kuni 200-MB. Kui kasutate mõnda muud teenusepakkujat ja eri sätteid, asendada seda teavet, kui teete teie taotlus. Järgmises näites taotlus uue teenuse klahvi:

    New-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup" -Location "West US" -SkuTier Standard -SkuFamily MeteredData -ServiceProviderName "Equinix" -PeeringLocation "Silicon Valley" -BandwidthInMbps 200

Veenduge, et määratud õige SKU taseme- ja pere SKU-ga.

- SKU taseme määrab, kas mõni ExpressRoute standard- või lisandmooduli ExpressRoute premium on lubatud. Saate määrata *Standard* standard SKU-ga või *Premium* saamiseks premium lisandmoodul.

- SKU pere määratleb arvelduse tüüp. Saate määrata *Metereddata* mahupõhise andmeid ning *Unlimiteddata* piiramatu. Pange tähele, et saate muuta arvelduse tüüp: *Metereddata* *Unlimiteddata*, kuid ei saa muuta tüüp: *Unlimiteddata* *Metereddata*.


>[AZURE.IMPORTANT] Oma ExpressRoute ringi, saadetakse teile arve klahvi teenus on välja antud hetkest. Veenduge, et seda toimingut teha kui ühenduvuse pakkuja on valmis ringi ette valmistada.

Vastus sisaldab teenuse võtit. Saate kõik parameetrid üksikasjalikku kirjeldust, käitades järgmine:


    get-help New-AzureRmExpressRouteCircuit -detailed


### <a name="4-list-all-expressroute-circuits"></a>4. loendis kõik ExpressRoute topoloogia

Selleks, et kõik teie loodud ExpressRoute topoloogia loendi saamiseks selle `Get-AzureRmExpressRouteCircuit` käsk:


    Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

Vastuse näeb välja umbes järgmine näide:


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
    CircuitProvisioningState          : Enabled
    ServiceProviderProvisioningState  : NotProvisioned
    ServiceProviderNotes              :
    ServiceProviderProperties         : {
                                          "ServiceProviderName": "Equinix",
                                          "PeeringLocation": "Silicon Valley",
                                          "BandwidthInMbps": 200
                                        }
    ServiceKey                        : **************************************
    Peerings                          : []

See teave on igal ajal saate alla laadida, kasutades funktsiooni `Get-AzureRmExpressRouteCircuit` cmdlet-käsk. Helistamine ilma parameetrid on ära toodud kõik topoloogia. Oma teenuse key loetletakse *ServiceKey* välja.


    Get-AzureRmExpressRouteCircuit


Vastuse näeb välja umbes järgmine näide:


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
    ServiceProviderProvisioningState : NotProvisioned
    ServiceProviderNotes             :
    ServiceProviderProperties        : {
                                         "ServiceProviderName": "Equinix",
                                         "PeeringLocation": "Silicon Valley",
                                         "BandwidthInMbps": 200
                                       }
    ServiceKey                       : **************************************
    Peerings                         : []


Saate kõik parameetrid üksikasjalikku kirjeldust, käitades järgmine:


    get-help Get-AzureRmExpressRouteCircuit -detailed

### <a name="5-send-the-service-key-to-your-connectivity-provider-for-provisioning"></a>5. saata pakkuja connectivity Service'i võti ettevalmistamise

*ServiceProviderProvisioningState* teave hetkeseisu teenusepakkuja küljel ettevalmistamise kohta. Oleku pakub Microsoft küljel olek. Ringi olekus ettevalmistamise kohta leiate lisateavet artiklist [töövood](expressroute-workflows.md#expressroute-circuit-provisioning-states) .

Kui loote uue ExpressRoute ringi, saab ringi olekus järgmist:


    ServiceProviderProvisioningState : NotProvisioned
    CircuitProvisioningState         : Enabled



Ringi muutub järgmised oleku, kui ühenduvuse pakkuja tegeleb lubamist saate:

    ServiceProviderProvisioningState : Provisioning
    Status                           : Enabled

Saate kasutada ka ExpressRoute ringi, peab olema järgmised olekus:

    ServiceProviderProvisioningState : Provisioned
    CircuitProvisioningState         : Enabled

### <a name="6-periodically-check-the-status-and-the-state-of-the-circuit-key"></a>6. perioodiliselt oleku ja ringi võti oleku kontrollimine

Oleku ja ringi võtme olek kontrollimisel saate teada, kui teie pakkuja on lubanud oma ringi. Pärast ringi on konfigureeritud, kuvatakse *ServiceProviderProvisioningState* *Provisioned*, nagu on näidatud järgmises näites:


    Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"


Vastuse näeb välja umbes järgmine näide:


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

### <a name="7-create-your-routing-configuration"></a>7. luua oma marsruutimine konfigureerimine

Üksikasjalikud juhised leiate artiklist [ExpressRoute marsruutimise konfiguratsioon](expressroute-howto-routing-arm.md) luua ja muuta ringi peerings.


>[AZURE.IMPORTANT] Need juhised kehtivad ainult topoloogia teenuse pakkujad, kes pakub layer 2 connectivity services abil loodud. Kui kasutate teenuse pakkuja, mis pakub hallatavate layer 3 teenused (tavaliselt on IP VPN, nt MPLS), pakkuja Ühenduvus on konfigureerimine ja haldamine saate marsruutimist.

### <a name="8-link-a-virtual-network-to-an-expressroute-circuit"></a>8. virtuaalse võrgu linkida mõne ExpressRoute ringi

Järgmiseks link virtuaalse võrgu oma ExpressRoute ringi. Kasutage [ühendav virtuaalse võrkude ExpressRoute topoloogia](expressroute-howto-linkvnet-arm.md) artiklis ressursihaldur juurutamise mudeli töötamisel.

## <a name="getting-the-status-of-an-expressroute-circuit"></a>Oleku ExpressRoute topoloogia toomine

See teave on igal ajal saate alla laadida, kasutades funktsiooni `Get-AzureRmExpressRouteCircuit` cmdlet-käsk. Helistamine ilma parameetrid on ära toodud kõik topoloogia.

    Get-AzureRmExpressRouteCircuit


Vastus on sarnane järgmises näites:


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


Saate teavet teatud ExpressRoute ringi, saates ressursirühma nimi ja ringi nimi parameetrina kõne:


    Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"


Vastuse näeb välja umbes järgmine näide:


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


Kuvatakse kõik parameetrid üksikasjalikku kirjeldust, käitades järgmine:

    get-help get-azurededicatedcircuit -detailed


## <a name="modify"></a>Mõne ExpressRoute ringi muutmine

Saate muuta teatud atribuudid ExpressRoute topoloogia mõjutamata Ühenduvus.

Saate teha järgmised pole tööseisakute.

- Lubamine või keelamine lisandmooduli ExpressRoute premium oma ExpressRoute ringi.
- Teie ExpressRoute topoloogia läbilaskevõime suurendamine Pange tähele, et kasutuselevõttu läbilaskevõime on topoloogia ei toetata.
- Muuta piiramatu andmete mõõtmine leping Mahupõhised andmete põhjal. Pange tähele, et muutmise mõõtmine leping piiramatu andmete põhjal Mahupõhised andmeid ei toetata.
-  Saate lubada või keelata *Luba klassikaline toimingud*.

Piirangute ja kitsenduste kohta lisateabe saamiseks vaadake [ExpressRoute KKK](expressroute-faqs.md).

### <a name="to-enable-the-expressroute-premium-add-on"></a>Kui soovite ExpressRoute premium mittetöötamisel lisandmooduli lubamine

Saate lubada ExpressRoute premium lisandmooduli jaoks oma olemasoleva ringi, kasutades järgmist PowerShelli koodilõigu.

    $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    $ckt.Sku.Tier = "Premium"
    $ckt.sku.Name = "Premium_MeteredData"

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt


Ringi on nüüd lubatud ExpressRoute premium lisandmooduli funktsioonid. Pange tähele, et me alustame arveldus saate premium lisandmooduli võimalus, kui käsk on edukalt käivitada.

### <a name="to-disable-the-expressroute-premium-add-on"></a>ExpressRoute premium lisandmooduli keelamine

>[AZURE.IMPORTANT] See toiming võib nurjuda, kui kasutate ressursse, mis on suuremad kui, mis on standardne ringi jaoks lubatud.

Võtke arvesse järgmist.

- Enne, kui vahetate premium standard, peate veenduma, et virtuaalse võrgu, mis on lingitud ringi arv on väiksem kui 10. Kui te pole seda, teie taotlus värskendamine nurjub ja oleme teile premium määrade arve.

- Kõik virtuaalne võrkude geopoliitiliste mujal peab lingi eemaldamine. Kui te ei tee seda, teie taotlus värskendamine nurjub ja oleme teile premium määrade arve.

- Tabeli marsruutimiseks peab olema väiksem kui 4000 lennuliinide privaatne silmitsemine. Kui teie marsruutimiseks tabeli maht on suurem kui 4000 marsruudib, BGP seansi langeb ja ei reenabled kuni reklaamitud eesliiteid arvu läheb 4000 all.

Saate keelata ExpressRoute premium lisandmoodul olemasoleva ringi, kasutades järgmist PowerShelli cmdlet-käsk:


    $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    $ckt.Sku.Tier = "Standard"
    $ckt.sku.Name = "Standard_MeteredData"

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt


### <a name="to-update-the-expressroute-circuit-bandwidth"></a>ExpressRoute ringi läbilaskevõime värskendamiseks

Toetatud läbilaskevõime suvandeid pakkuja, märkige ruut [ExpressRoute KKK](expressroute-faqs.md). Saate valida mis tahes suurusega, mis on suurem kui oma olemasoleva topoloogia suurus.

>[AZURE.IMPORTANT] Te ei saa vähendada läbilaskevõime ExpressRoute topoloogia katkestamata. Kasutuselevõttu läbilaskevõime nõuab teilt deprovision ExpressRoute ringi ja seejärel reprovision uue ExpressRoute ringi.

Kui otsustate, mida teil on vaja suurus, kasutada oma ringi suuruse järgmine käsk:


    $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    $ckt.ServiceProviderProperties.BandwidthInMbps = 1000

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt


Oma ringi on suurusega Microsoft küljel. Seejärel tuleb teil ühendust võtta värskendada oma küljele vastavaks selle muudatuse konfiguratsioone pakkuja Ühenduvus. Kui see teatis, alustame arveldus saate värskendatud läbilaskevõime suvandi.


### <a name="to-move-the-sku-from-metered-to-unlimited"></a>-Piiramatu Mahupõhised liikumiseks SKU kaudu

Saate muuta, kasutades järgmist PowerShelli koodilõigu SKU ExpressRoute topoloogia.

    $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    $ckt.Sku.Family = "UnlimitedData"
    $ckt.sku.Name = "Premium_UnlimitedData"

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

### <a name="to-control-access-to-the-classic-and-resource-manager-environments"></a>Klassikaline ja ressursihaldur keskkonnas juurdepääsu piiramiseks  

Tutvuge [teisaldamine ExpressRoute topoloogia: klassikaline ressursihaldur juurutamise mudeli](expressroute-howto-move-arm.md)juhiseid.  


## <a name="deprovisioning-and-deleting-an-expressroute-circuit"></a>Deprovisioning ja mõne ExpressRoute ringi kustutamine

Võtke arvesse järgmist.

- Kõik virtuaalse võrgu kaudu ExpressRoute ringi peab lingi eemaldamine. Kui see toiming nurjus, siis kontrollige, kui virtuaalse võrgu mis tahes lingitakse ringi.

- Kui ExpressRoute ringi teenuse pakkuja ettevalmistamise olek on **Provisioning** või **Provisioned** saate koostööd oma teenusepakkujalt deprovision ringi nende poolel. Jätkame reserveerimine ressursid ja teile arve, kuni teenusepakkuja lõpetab deprovisioning ringi ja teavitab meile.

- Kui teenusepakkuja on eemaldatud ringi (teenuse pakkuja ettevalmistamise olek on seatud **mitte ette valmistatud**) saate kustutada siis ringi. See toiming peatab ringi arveldus

Saate kustutada oma ExpressRoute ringi, käivitage järgmine käsk:

    Remove-AzureRmExpressRouteCircuit -ResourceGroupName "ExpressRouteResourceGroup" -Name "ExpressRouteARMCircuit"



## <a name="next-steps"></a>Järgmised sammud

Kui olete loonud oma ringi, veenduge, et teha järgmist:

- [Luua ja muuta oma ExpressRoute ringi marsruutimine](expressroute-howto-routing-arm.md)
- [Oma ExpressRoute ringi virtuaalse võrgu linkimine](expressroute-howto-linkvnet-arm.md)
