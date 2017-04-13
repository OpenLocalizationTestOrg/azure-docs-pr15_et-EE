<properties
   pageTitle="Luua, käivitage või kustutamine rakenduste portaali Azure'i ressursihaldur abil | Microsoft Azure'i"
   description="Sellelt lehelt leiate juhised loomine, konfigureerimine, käivitamine ja kustutada Azure rakenduste portaali Azure'i ressursihaldur abil"
   documentationCenter="na"
   services="application-gateway"
   authors="georgewallace"
   manager="carmonm"
   editor="tysonn"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="gwallace"/>


# <a name="create-start-or-delete-an-application-gateway-by-using-azure-resource-manager"></a>Luua, käivitage või kustutamine rakenduste portaali Azure'i ressursihaldur abil

> [AZURE.SELECTOR]
- [Azure'i portaal](application-gateway-create-gateway-portal.md)
- [Azure'i ressursihaldur PowerShelli](application-gateway-create-gateway-arm.md)
- [Azure'i klassikaline PowerShelli](application-gateway-create-gateway.md)
- [Azure'i ressursihaldur Mall](application-gateway-create-gateway-arm-template.md)
- [Azure'i CLI](application-gateway-create-gateway-cli.md)

Azure'i rakenduse lüüs on kiht-7 laadi koormusetasakaalustusteenuse. Kas nad on kohapealse ja pilveteenuse pakub Tõrkesiirde jõudlus marsruutimise HTTP päringuid serveritest, vahel. Rakenduse lüüsi pakub paljusid rakenduse kohaletoimetamise kontrolleril (ADC) funktsioonid, sh HTTP koormusetasakaalustuseks, seansi küpsis vastavalt osaleja, turvasoklite kiht (SSL) offload, kohandatud seisund sondid, mitme saidi tugi ja paljud teised. Toetatud funktsioonide täieliku loendi saamiseks külastage [Rakenduse lüüsi ülevaade](application-gateway-introduction.md)

Selles artiklis tutvustatakse loomine, konfigureerida, käivitamine ja rakenduste portaali kustutamine juhiseid.

>[AZURE.IMPORTANT] Enne, kui töötate Azure ressursse, on oluline mõista, et Azure'i praegu on kaks juurutamise mudelit: ressursihaldur ja klassikaline. Veenduge, et enne töötamine mis tahes Azure'i ressursi mõista [juurutamise mudelitel ja tööriistu](../azure-classic-rm.md) . Saate vaadata, klõpsates menüüd ülaosas selles artiklis eri tööriista dokumentatsioonist. Selle dokumendi käsitletakse rakenduste portaali loomine Azure ressursihaldur abil. Minge tavaversiooni kasutamiseks [loomine rakenduse lüüsi klassikaline juurutamine PowerShelli abil](application-gateway-create-gateway.md).


## <a name="before-you-begin"></a>Enne alustamist

1. Installige uusim versioon Azure PowerShelli cmdletid Web platvormi Installeri abil. Saate alla laadida ja installida **Windows PowerShelli** jaotisest [allalaadimiste lehe](https://azure.microsoft.com/downloads/)uusima versiooni.
2. Kui teil on mõne olemasoleva virtuaalse võrgu, valige olemasoleva tühja alamvõrgu või loomine on alamvõrgu virtuaalse olemasoleva võrgu üksnes kasutamiseks, lüüsi rakendus. Ei saa juurutada rakenduse lüüs kavatsete taha lüüsi rakenduse juurutamine ressursside virtuaalse võrku.
3. Saate konfigureerida kasutama rakenduse lüüsi jaoks vajalike peab olemas või nende lõpp-punktid loonud kas virtuaalse võrgu või avaliku IP/VIP määratud.

## <a name="what-is-required-to-create-an-application-gateway"></a>Mida on vaja luua rakenduste portaali?

- **Tagaandmebaas serveri pool:** Tagaandmebaas serverite IP-aadresside loend. IP-aadresside loetletud kuuluma kas virtuaalse alamvõrku või tuleks avaliku IP/VIP.
- **Tagaandmebaas serverisätete pool:** Igal pool on sätted, nt pordi, Protocol (protokoll) ja küpsis vastavalt osaleja. Need sätted on seotud on ja rakendatakse kõikides serverites maksimaalselt pool.
- **Ees port:** See port on avatud rakenduse lüüsi avaliku port. Liikluse tabab seda porti ja seejärel ümbersuunamist üks tagaandmebaas serverid.
- **Kuulajale:** Kuulajale on ees port, protokolli (Http- või Https need väärtused on tõstutundlik), ja SSL-i serdi nimi (kui offload SSL-i konfigureerimine).
- **Reegel:** Reegli seob kuulajale, tagaandmebaas server pargis ja määratleb, milliseid tagaandmebaas serveri rakenduskausta liiklus tuleks suunata kui see tabab kindla kuulajale.

## <a name="create-an-application-gateway"></a>Rakenduste portaali loomine

Klassikaline Azure ja Azure ressursihaldur abil vahe on, kus saate luua rakenduse lüüsi ja üksused, mida tuleb konfigureerida.

Ressursi halduri kõik üksused, mis muudavad rakenduste portaali on konfigureeritud eraldi ja seejärel panete koostööd, et luua rakenduse lüüsi ressursi.

Järgmised toimingud, mida on vaja rakenduste portaali loomine.

## <a name="create-a-resource-group-for-resource-manager"></a>Ressursihaldur jaoks ressursi rühma loomine

Veenduge, et kasutate Azure PowerShelli uusim versioon. Lisateabe saamiseks on saadaval [Koos ressursihaldur Windows PowerShelli kasutamine](../powershell-azure-resource-manager.md).

### <a name="step-1"></a>Samm 1

Azure'i sisse logida

    Login-AzureRmAccount

Teil palutakse autentimiseks mandaat.

### <a name="step-2"></a>Samm 2

Kontrollige konto tellimused.

    Get-AzureRmSubscription

### <a name="step-3"></a>Samm 3

Valida Azure tellimuste kasutama.

    Select-AzureRmSubscription -Subscriptionid "GUID of subscription"

### <a name="step-4"></a>Samm 4

Looge ressursirühma (Jäta see juhis, kui kasutate ressursside olemasolevasse rühma).

    New-AzureRmResourceGroup -Name appgw-rg -Location "West US"

Azure'i ressursihaldur nõuab kõigi asukoha määramiseks. Selle asukoha kasutatakse vaikeasukohaks selle ressursirühma ressurssidele. Veenduge, et kõik käsud rakenduste portaali loomiseks kasutab sama ressursirühma.

Ülaltoodud näites lõime ressursirühma nimega "appgw RG" ja asukoht "Lääne meie".

>[AZURE.NOTE] Kui teil on vaja konfigureerida kohandatud juures lüüsi rakenduse jaoks, vaadake teemat [loomine rakenduste portaali kohandatud sondid PowerShelli abil](application-gateway-create-probe-ps.md). Tutvuge [kohandatud sondid ja seisundi jälgimine](application-gateway-probe-overview.md) lisateabe saamiseks.

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Virtuaalse võrgu ja alamvõrgu, lüüsi rakenduse loomine

Järgmises näites kujutatakse luua virtuaalse võrgu ressursihaldur abil.

### <a name="step-1"></a>Samm 1

Määrata aadress vahemiku 10.0.0.0/24 alamvõrgu muutuja virtuaalse võrgu loomiseks kasutada.

    $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24

### <a name="step-2"></a>Samm 2

Luua virtuaalse võrgu nimega "appgwvnet" ressursi rühma "appgw-rg" 10.0.0.0/16 eesliite kasutamine alamvõrgu 10.0.0.0/24 Lääne USA regiooni jaoks.

    $vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet

### <a name="step-3"></a>Samm 3

Määrake alamvõrku muutuja jaoks järgmised toimingud, mis rakenduste portaali loomine.

    $subnet=$vnet.Subnets[0]

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Luua avaliku IP-aadressi ees konfigureerimine

Saate luua avaliku IP ressursi "publicIP01" ressursi rühma "appgw-rg" Lääne USA regiooni jaoks.

    $publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name publicIP01 -location "West US" -AllocationMethod Dynamic


## <a name="create-an-application-gateway-configuration-object"></a>Rakenduse lüüsi konfigureerimise objekti loomine

Kõigi konfiguratsiooni üksuste peab häälestamine enne rakenduste portaali loomine. Järgmiste juhiste loomine konfiguratsiooni üksused, mida on vaja rakenduse lüüsi ressurss.

### <a name="step-1"></a>Samm 1

Looge rakenduse gateway IP konfiguratsiooni nimega "gatewayIP01". Lüüsi rakenduse käivitamisel jätkab kaudu alamvõrku, mis on konfigureeritud IP-aadress ja marsruudib võrguliiklust tagaandmebaas IP kogumi IP-aadressid. Pidage meeles, et iga eksemplari võtab IP-aadress.

    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet

### <a name="step-2"></a>Samm 2

Konfigureerimine tagaandmebaas IP address kausta nimega "pool01" koos IP-aadresside "134.170.185.46, 134.170.188.221,134.170.185.50". IP-aadressid on IP-aadressid, mis saadetakse võrguliiklust, mis pärineb ees IP lõpp-punkti. Saate asendada eelmise IP-aadresside lisamiseks oma rakenduse IP address lõpp-punktid.

    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221,134.170.185.50

### <a name="step-3"></a>Samm 3

Rakenduse lüüsi sätte "poolsetting01" konfigureerimine tagaandmebaas rakenduskausta koormusetasakaalustusega võrguliiklust.

    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Disabled

### <a name="step-4"></a>Samm 4

Konfigureerige ees IP port nimega "frontendport01" avaliku IP lõpp-punkti.

    $fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 80

### <a name="step-5"></a>Juhis 5

Luua nimega "fipconfig01" ees IP konfiguratsiooni ja seostada ees IP konfiguratsiooni avaliku IP-aadressi.

    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip

### <a name="step-6"></a>Juhist 6

Looge kuulajale nimi "listener01" ja ees pordi ees IP-konfiguratsiooni seostada.

    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp

### <a name="step-7"></a>Juhis 7

Laadi koormusetasakaalustusteenuse marsruutimine nimega "rule01", mis konfigureerib käitumise laadimine koormusetasakaalustusteenuse reegli loomine.

    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

### <a name="step-8"></a>Samm 8

Konfigureerida rakendus lüüsi eksemplari suurust.

    $sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

>[AZURE.NOTE]  *InstanceCount* vaikeväärtus on 2 suurim väärtus on 10. *GatewaySize* vaikeväärtus on keskmine. Saate valida Standard_Small ning Standard_Medium Standard_Large.

## <a name="create-an-application-gateway-by-using-new-azurermapplicationgateway"></a>Uus-AzureRmApplicationGateway abil rakenduste portaali loomine

Saate luua rakenduste portaali konfiguratsiooni kõigi üksuste eelmist toimingut. Selles näites on rakenduse lüüsi nimetatakse "appgwtest".

    $appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku

### <a name="step-9"></a>Samm 9

Tuua avaliku IP ressursi rakenduse lüüsi seotud DNS-i ja VIP rakenduse lüüsi üksikasjad.

    Get-AzureRmPublicIpAddress -Name publicIP01 -ResourceGroupName appgw-rg  

## <a name="delete-an-application-gateway"></a>Rakenduste portaali kustutamine

Rakenduste portaali kustutamiseks tehke järgmist.

### <a name="step-1"></a>Samm 1

Hangi objekti application lüüsi ja seostada muutuja "$getgw".

    $getgw = Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

### <a name="step-2"></a>Samm 2

**Lõpeta – AzureRmApplicationGateway** abil rakenduse lüüsi peatamine.

    Stop-AzureRmApplicationGateway -ApplicationGateway $getgw  

Kui rakenduse lüüsi on peatatud olekus, **Eemalda-AzureRmApplicationGateway** cmdlet-käsu abil teenuse eemaldamine.

    Remove-AzureRmApplicationGateway -Name $appgwtest -ResourceGroupName appgw-rg -Force

>[AZURE.NOTE] Funktsiooni **-jõustada** parameetrit saab kasutada mis tahes kinnitusteate eemaldamine.

Veenduge, et teenus on eemaldatud, saate kasutada cmdlet-käsu **Get-AzureRmApplicationGateway** . Seda toimingut ei ole vaja.

    Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

## <a name="get-application-gateway-dns-name"></a>Saada rakenduse lüüsi DNS-i nimi

Kui lüüsi on loodud, on järgmiseks esiosa side konfigureerimine. Avaliku IP kasutamisel nõuab rakenduse lüüsi dünaamiliselt DNS-i nimi, mis ei ole lubatud. Lõppkasutajad tabab rakenduse lüüsi CNAME-kirje tagamiseks saab rakenduse lüüsi avaliku lõpp-punkti. [Kohandatud domeeni nimi Azure konfigureerimine](../cloud-services/cloud-services-custom-domain-name-portal.md). Selleks saate tuua rakenduste portaali ja selle seotud IP/DNS-i nimi, kasutades PublicIPAddress element, mis on seotud rakenduse lüüsi üksikasjad. CNAME-kirje, mis suunab selle DNS-i nimi kaks veebirakenduste loomiseks tuleks kasutada rakenduse lüüsi DNS-i nimi. A-kirjete kasutamine ei ole soovitatav, kuna VIP võivad muutuda taaskäivitamisel rakenduse lüüsi.
    
    Get-AzureRmPublicIpAddress -ResourceGroupName appgw-RG -Name publicIP01
        
    Name                     : publicIP01
    ResourceGroupName        : appgw-RG
    Location                 : westus
    Id                       : /subscriptions/<subscription_id>/resourceGroups/appgw-RG/providers/Microsoft.Network/publicIPAddresses/publicIP01
    Etag                     : W/"00000d5b-54ed-4907-bae8-99bd5766d0e5"
    ResourceGuid             : 00000000-0000-0000-0000-000000000000
    ProvisioningState        : Succeeded
    Tags                     : 
    PublicIpAllocationMethod : Dynamic
    IpAddress                : xx.xx.xxx.xx
    PublicIpAddressVersion   : IPv4
    IdleTimeoutInMinutes     : 4
    IpConfiguration          : {
                                 "Id": "/subscriptions/<subscription_id>/resourceGroups/appgw-RG/providers/Microsoft.Network/applicationGateways/appgwtest/frontendIP
                               Configurations/frontend1"
                               }
    DnsSettings              : {
                                 "Fqdn": "00000000-0000-xxxx-xxxx-xxxxxxxxxxxx.cloudapp.net"
                               }

## <a name="next-steps"></a>Järgmised sammud

Kui soovite konfigureerida SSL offload, lugege teemat [konfigureerimine rakenduste portaali SSL offload](application-gateway-ssl.md).

Kui soovite rakenduste portaali kasutamiseks on sisemine laadi koormusetasakaalustusteenuse konfigureerimine, lugege teemat [rakenduste abil on sisemine laadi koormusetasakaalustusteenuse (ILB) portaali loomine](application-gateway-ilb.md).

Kui soovite lisateavet laadimine tasakaalustavad suvandid üldiselt, lugege teemat:

- [Azure'i laadi koormusetasakaalustusteenuse](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure'i liikluse haldur](https://azure.microsoft.com/documentation/services/traffic-manager/)
