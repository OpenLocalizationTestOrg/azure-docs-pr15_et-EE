<properties
   pageTitle="Luua ja muuta mõne ExpressRoute ringi klassikaline juurutamise mudeli ja PowerShelli abil | Microsoft Azure'i"
   description="Selles artiklis tutvustatakse loomise ja mõne ExpressRoute ringi ettevalmistamise juhiseid. Selles artiklis kirjeldatakse, kuidas kontrollida olek, värskendamine või kustutamine ja deprovision oma ringi."
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
   ms.author="ganesr;cherylmc"/>

# <a name="create-and-modify-an-expressroute-circuit"></a>Luua ja muuta mõne ExpressRoute ringi

> [AZURE.SELECTOR]
[Azure'i portaal - ressursihaldur](expressroute-howto-circuit-portal-resource-manager.md)
[PowerShelli - ressursihaldur](expressroute-howto-circuit-arm.md)
[PowerShelli – klassikaline](expressroute-howto-circuit-classic.md)

Selles artiklis tutvustatakse juhised PowerShelli cmdlet-käskude ja klassikaline juurutamise mudeli abil on Azure ExpressRoute ringi loomiseks. Selles artiklis ka näitab teile, kuidas kontrollida olek, värskendamine või kustutamine ja deprovision mõne ExpressRoute ringi.

**Azure'i juurutamise mudelite kohta**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]


## <a name="before-you-begin"></a>Enne alustamist

### <a name="1-review-the-prerequisites-and-workflow-articles"></a>1. Vaadake eeltingimused ja töövoo artiklid

Veenduge, et teil vaadata [eeltingimused](expressroute-prerequisites.md) ja [töövoogude](expressroute-workflows.md) enne alustamist konfigureerimine.  


### <a name="2-install-the-latest-versions-of-the-azure-powershell-modules"></a>2 installida Azure PowerShelli moodulid uusimad versioonid 

Järgige juhiseid, [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md) üksikasjalikud juhised arvuti kasutada Azure PowerShelli moodulid konfigureerimise kohta.

### <a name="3-log-in-to-your-azure-account-and-select-a-subscription"></a>3. logige sisse oma Azure'i kontosse ja valige soovitud tellimus

1. Käivitage järgmine cmdlet-käsk abil on tõstetud Windows PowerShelli käsuviibas:

        Add-AzureAccount
2. Sisselogimise ekraanil kuvatavat, logige sisse oma kontole.

3. Tellimuste loendi hankimine.

        Get-AzureSubscription
4. Valige tellimus, mida soovite kasutada.
    
        Select-AzureSubscription -SubscriptionName "mysubscriptionname"

## <a name="create-and-provision-an-expressroute-circuit"></a>Loomine ja mõne ExpressRoute ringi ettevalmistamine

### <a name="1-import-the-powershell-modules-for-expressroute"></a>1. importige PowerShelli moodulid ExpressRoute jaoks

 Kui te pole seda juba teinud, peab Azure ja ExpressRoute moodulid importimine PowerShelli seansi alustamiseks ExpressRoute cmdlet-käskude abil. Moodulid importimine need installiti abil oma kohalikus arvutis soovitud asukohta. Sõltuvalt teie installida moodulid, võib asukoha erinevas Järgnevas näites. Vajadusel, muutke näide.  

    Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
    Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'

### <a name="2-get-the-list-of-supported-providers-locations-and-bandwidths"></a>2. toetatud pakkujad, asukohad ja andmemahud loendi hankimine

Enne mõne ExpressRoute ringi loomist peate toetatud ühenduvuse pakkujate, asukohad ja läbilaskevõime suvandite loend.

PowerShelli cmdlet-käsk `Get-AzureDedicatedCircuitServiceProvider` tagastab see teave, mille abil saate hiljem toimingut:

    Get-AzureDedicatedCircuitServiceProvider

Kontrollige, kui seal on loetletud pakkuja Ühenduvus. Märkige järgmine teave üles, sest teil on vaja seda hiljem soovitud ringi loomisel:

- Nimi

- PeeringLocations

- BandwidthsOffered

Nüüd oletegi valmis looma mõne ExpressRoute ringi.         

### <a name="3-create-an-expressroute-circuit"></a>3. mis ExpressRoute ringi loomine

Järgmises näites on kujutatud loomine Edenvale ExpressRoute ringi Equinix kuni 200-MB. Kui kasutate mõnda muud teenusepakkujat ja eri sätteid, asendada seda teavet, kui teete teie taotlus.

>[AZURE.IMPORTANT] Oma ExpressRoute ringi, saadetakse teile arve klahvi teenus on välja antud hetkest. Veenduge, et seda toimingut teha kui ühenduvuse pakkuja on valmis ringi ette valmistada.


Järgmises näites taotlus uue teenuse klahvi:

    $Bandwidth = 200
    $CircuitName = "MyTestCircuit"
    $ServiceProvider = "Equinix"
    $Location = "Silicon Valley"

    New-AzureDedicatedCircuit -CircuitName $CircuitName -ServiceProviderName $ServiceProvider -Bandwidth $Bandwidth -Location $Location -sku Standard -BillingType MeteredData

Või kui soovite luua mõne ExpressRoute ringi premium lisandmoodul, kasutage järgmises näites. [ExpressRoute KKK](expressroute-faqs.md) premium lisandmooduli kohta lisateabe saamiseks vaadake.

    New-AzureDedicatedCircuit -CircuitName $CircuitName -ServiceProviderName $ServiceProvider -Bandwidth $Bandwidth -Location $Location -sku Premium - BillingType MeteredData


Vastus sisaldab teenuse võti. Kuvatakse kõik parameetrid üksikasjalikku kirjeldust, käitades järgmine:

    get-help new-azurededicatedcircuit -detailed

### <a name="4-list-all-the-expressroute-circuits"></a>4. loendis kõik ExpressRoute topoloogia

Saate kasutada funktsiooni `Get-AzureDedicatedCircuit` käsk saada kõik teie loodud ExpressRoute topoloogia loendi:


    Get-AzureDedicatedCircuit

Vastus on midagi sarnast järgmises näites:

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : NotProvisioned
    Sku                              : Standard
    Status                           : Enabled

See teave on igal ajal saate alla laadida, kasutades funktsiooni `Get-AzureDedicatedCircuit` cmdlet-käsk. Helistamine ilma parameetrid on ära toodud kõik topoloogia. Teenuse tootevõti on loetletud *ServiceKey* välja.

    Get-AzureDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : NotProvisioned
    Sku                              : Standard
    Status                           : Enabled

Kuvatakse kõik parameetrid üksikasjalikku kirjeldust, käitades järgmine:

    get-help get-azurededicatedcircuit -detailed

### <a name="5-send-the-service-key-to-your-connectivity-provider-for-provisioning"></a>5. saata pakkuja connectivity Service'i võti ettevalmistamise


*ServiceProviderProvisioningState* teave praeguse teenusepakkuja küljel ettevalmistamise kohta. *Oleku* pakub Microsoft küljel olek. Ringi olekus ettevalmistamise kohta leiate lisateavet artiklist [töövood](expressroute-workflows.md#expressroute-circuit-provisioning-states) .

Kui loote uue ExpressRoute ringi, saab ringi olekus järgmist:

    ServiceProviderProvisioningState : NotProvisioned
    Status                           : Enabled


Ringi läheb järgmine riik, kui ühenduvuse pakkuja tegeleb lubamist saate:

    ServiceProviderProvisioningState : Provisioning
    Status                           : Enabled

Mõne ExpressRoute ringi peab olema järgmised olekus, saate seda kasutada:

    ServiceProviderProvisioningState : Provisioned
    Status                           : Enabled


### <a name="6-periodically-check-the-status-and-the-state-of-the-circuit-key"></a>6. perioodiliselt oleku ja ringi võti oleku kontrollimine

Nii saate teada, kui teie pakkuja on lubanud oma ringi. Pärast ringi on konfigureeritud, kuvatakse *ServiceProviderProvisioningState* *Provisioned* kujul, nagu on näidatud järgmises näites:

    Get-AzureDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

### <a name="7-create-your-routing-configuration"></a>7. luua oma marsruutimine konfigureerimine

Vaadake selle [ExpressRoute circuit marsruudi konfiguratsiooni (luua ja muuta ringi peerings)](expressroute-howto-routing-classic.md) üksikasjalikud juhised artiklist.

>[AZURE.IMPORTANT] Need juhised kehtivad ainult topoloogia teenuse pakkujad, kes pakub layer 2 connectivity services abil loodud. Kui kasutate teenuse pakkuja, mis pakub hallatavate layer 3 teenused (tavaliselt on IP VPN, nt MPLS), pakkuja Ühenduvus on konfigureerimine ja haldamine saate marsruutimist.

### <a name="8-link-a-virtual-network-to-an-expressroute-circuit"></a>8. virtuaalse võrgu linkida mõne ExpressRoute ringi

Järgmiseks link virtuaalse võrgu oma ExpressRoute ringi. Vaadake üksikasjalikke juhiseid [linkimise ExpressRoute skeeme virtuaalse võrgu](expressroute-howto-linkvnet-classic.md) . Kui teil on vaja luua virtuaalse võrgu ExpressRoute klassikaline juurutamise mudeli abil, vaadake [loomine virtuaalse võrgu jaoks ExpressRoute](expressroute-howto-vnet-portal-classic.md) olevaid juhiseid.

## <a name="getting-the-status-of-an-expressroute-circuit"></a>Oleku ExpressRoute topoloogia toomine

See teave on igal ajal saate alla laadida, kasutades funktsiooni `Get-AzureCircuit` cmdlet-käsk. Helistamine ilma parameetrid on ära toodud kõik topoloogia.

    Get-AzureDedicatedCircuit

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

    Bandwidth                        : 1000
    CircuitName                      : MyAsiaCircuit
    Location                         : Singapore
    ServiceKey                       : #################################
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

Saate teavet teatud ExpressRoute ringi, saates teenuse võti parameetrina kõne:

    Get-AzureDedicatedCircuit -ServiceKey "*********************************"

    Bandwidth                        : 200
    CircuitName                      : MyTestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled


Kuvatakse kõik parameetrid üksikasjalikku kirjeldust, käitades järgmine:

    get-help get-azurededicatedcircuit -detailed

## <a name="modifying-an-expressroute-circuit"></a>Mõne ExpressRoute ringi muutmine

Saate muuta teatud atribuudid ExpressRoute topoloogia mõjutamata Ühenduvus.

Saate teha järgmised pole tööseisakute.

- Lubamine või keelamine lisandmooduli ExpressRoute premium oma ExpressRoute ringi.
- Teie ExpressRoute topoloogia läbilaskevõime suurendamine Pange tähele, et kasutuselevõttu läbilaskevõime on topoloogia ei toetata.
- Muuta piiramatu andmete mõõtmine leping Mahupõhised andmete põhjal. Pange tähele, et muutmise mõõtmine leping piiramatu andmete põhjal Mahupõhised andmeid ei toetata.
- Saate lubada või keelata *Luba klassikaline toimingud*.

Vaadake lisateavet piirangute ja kitsenduste [ExpressRoute KKK](expressroute-faqs.md) .

### <a name="to-enable-the-expressroute-premium-add-on"></a>Kui soovite ExpressRoute premium mittetöötamisel lisandmooduli lubamine

Saate lubada ExpressRoute premium lisandmooduli jaoks oma olemasoleva ringi PowerShelli järgmise cmdleti abil.

    Set-AzureDedicatedCircuitProperties -ServiceKey "*********************************" -Sku Premium

    Bandwidth                        : 1000
    CircuitName                      : TestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Premium
    Status                           : Enabled

Oma ringi on nüüd lubatud ExpressRoute premium lisandmooduli funktsioonid. Pange tähele, et Alustame arveldus saate premium lisandmooduli võimalus, kui käsk on edukalt käivitada.

### <a name="to-disable-the-expressroute-premium-add-on"></a>ExpressRoute premium lisandmooduli keelamine

>[AZURE.IMPORTANT] See toiming võib nurjuda, kui kasutate ressursse, mis on suuremad kui, mis on standardne ringi jaoks lubatud.

Võtke arvesse järgmist.

- Peate veenduma, et lingitud ringi virtuaalne võrkude arv on väiksem kui 10 enne, kui vahetate premium standard. Kui te ei tee seda, siis teie taotlus värskendamine nurjub ja saate arve premium määra.

- Kõik virtuaalne võrkude geopoliitiliste mujal peab lingi eemaldamine. Kui te ei tee seda, siis teie taotlus värskendamine nurjub ja saate arve premium määra.

- Tabeli marsruutimiseks peab olema väiksem kui 4000 lennuliinide privaatne silmitsemine. Kui teie marsruutimiseks tabeli maht on suurem kui 4000 marsruudib, BGP seansi langeb ja ei reenabled kuni reklaamitud eesliiteid arvu läheb 4000 all.

ExpressRoute premium lisandmooduli saate oma olemasolevad ringi keelata PowerShelli järgmise cmdleti abil:

    Set-AzureDedicatedCircuitProperties -ServiceKey "*********************************" -Sku Standard

    Bandwidth                        : 1000
    CircuitName                      : TestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled



### <a name="to-update-the-expressroute-circuit-bandwidth"></a>ExpressRoute ringi läbilaskevõime värskendamiseks

Märkige ruut [ExpressRoute KKK](expressroute-faqs.md) toetatud läbilaskevõime suvandite pakkuja. Saate valida mis tahes suurusega, mis on suurem kui oma olemasoleva topoloogia suurus, kui füüsilise pordi (mille on teie ringi loodud) võimaldab.

>[AZURE.IMPORTANT] Te ei saa vähendada läbilaskevõime ExpressRoute topoloogia katkestamata. Kasutuselevõttu läbilaskevõime vajavad deprovision ExpressRoute ringi ja seejärel reprovision uue ExpressRoute ringi.

Kui otsustate, mida teil on vaja suurus, saate oma ringi suuruse järgmine käsk:

    Set-AzureDedicatedCircuitProperties -ServiceKey ********************************* -Bandwidth 1000

    Bandwidth                        : 1000
    CircuitName                      : TestCircuit
    Location                         : Silicon Valley
    ServiceKey                       : *********************************
    ServiceProviderName              : equinix
    ServiceProviderProvisioningState : Provisioned
    Sku                              : Standard
    Status                           : Enabled

Oma ringi kuvatakse on antud suurusega Microsoft küljel. Pöörduge värskendada oma küljele vastavaks selle muudatuse konfiguratsioone pakkuja Ühenduvus. Pange tähele, et me alustame arveldus saate sealt värskendatud läbilaskevõime suvandi kohta.

Kui suurendab ringi läbilaskevõime kuvatakse järgmine tõrketeade, tähendab see seal on jäänud pole piisavalt ribalaiust füüsilise portide, kus luuakse oma olemasoleva ringi. Teil on selle ringi kustutada ja luua uue ringi vajaliku suurusega. 

    Set-AzureDedicatedCircuitProperties : InvalidOperation : Insufficient bandwidth available to perform this circuit
    update operation
    At line:1 char:1
    + Set-AzureDedicatedCircuitProperties -ServiceKey ********************* ...
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : CloseError: (:) [Set-AzureDedicatedCircuitProperties], CloudException
        + FullyQualifiedErrorId : Microsoft.WindowsAzure.Commands.ExpressRoute.SetAzureDedicatedCircuitPropertiesCommand
    

## <a name="deprovisioning-and-deleting-an-expressroute-circuit"></a>Deprovisioning ja mõne ExpressRoute ringi kustutamine

Võtke arvesse järgmist.

- Kõik virtuaalse võrgu kaudu ExpressRoute ringi õnnestub selle toimingu jaoks peab lingi eemaldamine. Kontrollige, kas teil on virtuaalse võrgu, mis on seotud ringi, kui see toiming nurjus.

- Kui ExpressRoute ringi teenuse pakkuja ettevalmistamise olek on **Provisioning** või **Provisioned** saate koostööd oma teenusepakkujalt deprovision ringi nende poolel. Jätkame reserveerida ressursid ja kuni teenusepakkuja lõpetab deprovisioning ringi ja teavitab meil teile arve.

- Kui teenusepakkuja on eemaldatud ringi (teenuse pakkuja ettevalmistamise olek on seatud **mitte ette valmistatud**) saate kustutada siis ringi. See toiming peatab ringi arveldus.

Saate kustutada oma ExpressRoute ringi, käivitage järgmine käsk:

    Remove-AzureDedicatedCircuit -ServiceKey "*********************************"



## <a name="next-steps"></a>Järgmised sammud

Kui olete loonud oma ringi, veenduge, et teha järgmist:

- [Luua ja muuta oma ExpressRoute ringi marsruutimine](expressroute-howto-routing-classic.md)
- [Oma ExpressRoute ringi virtuaalse võrgu linkimine](expressroute-howto-linkvnet-classic.md)
