<properties
   pageTitle="Loomine ja konfigureerimine rakenduste portaali on sisemine laadi koormusetasakaalustusteenuse (ILB), kasutades Azure ressursihaldur | Microsoft Azure'i"
   description="Sellelt lehelt leiate juhiseid, et luua, konfigureerida, käivitamine ja Kustuta sisemise koormuse koormusetasakaalustusteenuse (ILB) koos Azure rakenduste portaali Azure'i ressursihaldur"
   documentationCenter="na"
   services="application-gateway"
   authors="georgewallace"
   manager="carmonm"
   editor="tysonn"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/19/2016"
   ms.author="gwallace"/>


# <a name="create-an-application-gateway-with-an-internal-load-balancer-ilb-by-using-azure-resource-manager"></a>Azure'i ressursihaldur abil luua rakenduste portaali koos mõne sisemise koormuse koormusetasakaalustusteenuse (ILB)

> [AZURE.SELECTOR]
- [Azure'i klassikaline järgmiselt.](application-gateway-ilb.md)
- [Ressursihaldur PowerShelli järgmiselt.](application-gateway-ilb-arm.md)

Azure'i rakenduse lüüsi saab konfigureerida on Interneti-ühendusega VIP või sisemise lõpp-punkti, ei ole Interneti-ühendus, nimetatakse ka sisemise koormuse koormusetasakaalustusteenuse (ILB) lõpp. Lüüsi konfigureerimise abil soovitud ILB on kasulik sisemise ärivaldkonna rakendusi, mis Internetis puudub. See on abiks teenuste ja astme mitmekihilise rakenduses, mis istuda turvalisus piiri, ei ole Interneti-ühendus, kuid ikka nõua round-jaan laadimine jaotuse, seansi kleepunud või lõpetamise turvasoklite kiht (SSL).

Selles artiklis tutvustatakse rakenduste portaali ja mõne ILB konfigureerimine juhiseid.

## <a name="before-you-begin"></a>Enne alustamist

1. Installige uusim versioon Azure PowerShelli cmdletid Web platvormi Installeri abil. Saate alla laadida ja installida **Windows PowerShelli** jaotisest [allalaadimiste lehe](https://azure.microsoft.com/downloads/)uusima versiooni.
2. Rakenduse lüüsi jaoks luua virtuaalse võrgu ja on alamvõrgu. Veenduge, et kasutate cloud kasutuselevõttu ega virtuaalmasinates alamvõrgu. Rakenduse lüüsi peab olema ise virtuaalse alamvõrku.
3. Saate konfigureerida kasutama rakenduse lüüsi jaoks vajalike peab olemas või nende lõpp-punktid loonud kas virtuaalse võrgu või avaliku IP/VIP määratud.

## <a name="what-is-required-to-create-an-application-gateway"></a>Mida on vaja luua rakenduste portaali?


- **Tagaandmebaas serveri pool:** Tagaandmebaas serverid IP-aadresside loend. IP-aadresside loendis kas kuuluvuse virtuaalse võrku, kuid erinevad alamvõrgu rakenduse lüüsi või tuleks avaliku IP/VIP.
- **Tagaandmebaas serverisätete pool:** Igal pool on sätted, nt portide, Protocol (protokoll) ja küpsise vastavalt osaleja. Need sätted on seotud on ja rakendatakse kõikides serverites maksimaalselt pool.
- **Ees port:** See port on avatud rakenduse lüüsi avaliku port. Liikluse tabab seda porti ja seejärel ümbersuunamist üks tagaandmebaas serverid.
- **Kuulajale:** Kuulajale on ees port, protokolli (Http- või Https need tõstutundlik), ja SSL-i serdi nimi (kui offload SSL-i konfigureerimine).
- **Reegel:** Reegli seob kuulajale ja tagaandmebaas server pargis ja määratleb, milliseid tagaandmebaas serveri rakenduskausta liiklus tuleks suunata kui see tabab kindla kuulajale. Praegu on toetatud ainult *tavaline* reegel. *Tavaline* reegel on ringi-jaan jaotust.



## <a name="create-an-application-gateway"></a>Rakenduste portaali loomine

Klassikaline Azure ja Azure ressursihaldur abil vahe on, kus saate luua rakenduse lüüsi ja üksused, mida tuleb konfigureerida.
Ressursi halduri kõik üksused, mis muudavad rakenduste portaali on konfigureeritud eraldi ja seejärel panete koostööd, et luua rakenduse lüüsi ressursi.


Siin on toimingud, mida on vaja rakenduste portaali loomine.

1. Ressursihaldur jaoks ressursi rühma loomine
2. Virtuaalse võrgu ja alamvõrgu, lüüsi rakenduse loomine
3. Rakenduse lüüsi konfigureerimise objekti loomine
4. Lüüsi ressurssi loomine


## <a name="create-a-resource-group-for-resource-manager"></a>Ressursihaldur jaoks ressursi rühma loomine

Veenduge, et saate vahetada PowerShelli režiimi Azure'i ressursihaldur cmdlet-käskude kasutamine. Lisateave on saadaval [Koos ressursihaldur Windows PowerShelli kasutamine](../powershell-azure-resource-manager.md).

### <a name="step-1"></a>Samm 1

    Login-AzureRmAccount

### <a name="step-2"></a>Samm 2

Kontrollige konto tellimused.

    Get-AzureRmSubscription

Teil palutakse autentimiseks mandaat.<BR>

### <a name="step-3"></a>Samm 3

Valida Azure tellimuste kasutada. <BR>


    Select-AzureRmSubscription -Subscriptionid "GUID of subscription"


### <a name="step-4"></a>Samm 4

Saate luua uue ressursirühma (Jäta see juhis, kui kasutate ressursside olemasolevasse rühma).

    New-AzureRmResourceGroup -Name appgw-rg -location "West US"

Azure'i ressursihaldur nõuab kõigi asukoha määramiseks. Seda kasutatakse vaikeasukohaks selle ressursi jaotises ressursid. Veenduge, et kõik käsud rakenduste portaali loomiseks kasutab sama ressursirühma.

Ülaltoodud näites lõime ressursirühma nimega "appgw rg" ja asukoht "Lääne meie".

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Virtuaalse võrgu ja alamvõrgu, lüüsi rakenduse loomine

Järgmises näites kirjeldatakse, kuidas luua virtuaalse võrgu ressursihaldur abil.

### <a name="step-1"></a>Samm 1

    $subnetconfig = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24

Seda määrab aadress vahemiku 10.0.0.0/24 alamvõrgu muutuja virtuaalse võrgu loomiseks kasutada.

### <a name="step-2"></a>Samm 2

    $vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnetconfig

See loob virtuaalse võrgu nimega "appgwvnet" ressursi rühma "appgw-rg" 10.0.0.0/16 eesliite kasutamine alamvõrgu 10.0.0.0/24 Lääne USA regiooni jaoks.

### <a name="step-3"></a>Samm 3

    $subnet = $vnet.subnets[0]

Seda määrab alamvõrgu objekti muutuja $subnet järgmiste juhiste juurde.

## <a name="create-an-application-gateway-configuration-object"></a>Rakenduse lüüsi konfigureerimise objekti loomine

### <a name="step-1"></a>Samm 1

    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet

See loob rakenduse gateway IP konfiguratsiooni nimega "gatewayIP01". Lüüsi rakenduse käivitamisel jätkab kaudu alamvõrku, mis on konfigureeritud IP-aadress ja IP-aadresside tagaandmebaas IP kogumi võrguliiklust marsruutida. Pidage meeles, et iga eksemplari võtab IP-aadress.


### <a name="step-2"></a>Samm 2

    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221,134.170.185.50

See konfigureerib tagaandmebaas IP address kausta nimega "pool01" koos IP-aadresside "134.170.185.46, 134.170.188.221,134.170.185.50". Need on IP-aadressid, mis saadetakse võrguliiklust, mis pärineb ees IP lõpp-punkti. Saate asendada IP-aadresside eespool lisada oma rakenduse IP address lõpp-punktid.

### <a name="step-3"></a>Samm 3

    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Disabled

See konfigureerib tagaandmebaas kogumi rakenduse lüüsi sätte "poolsetting01" koormuse tasakaalustatud võrguliiklust.

### <a name="step-4"></a>Samm 4

    $fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 80

See konfigureerib ees IP port nimega "frontendport01" on ILB.

### <a name="step-5"></a>Juhis 5

    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -Subnet $subnet

See loob nimega "fipconfig01" ees IP-konfiguratsiooni ja seostab privaatne IP praeguse virtuaalse alamvõrku kaudu.

### <a name="step-6"></a>Juhist 6

    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp

See loob nimega "listener01" kuulajale ja seob ees pordi ees IP-konfiguratsiooni.

### <a name="step-7"></a>Juhis 7

    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

See loob laadi koormusetasakaalustusteenuse marsruutimine reegli nimega "rule01", mis konfigureerib laadi koormusetasakaalustusteenuse käitumine.

### <a name="step-8"></a>Samm 8

    $sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

See konfigureerib rakenduste lüüsi eksemplari suurust.

>[AZURE.NOTE]  *InstanceCount* vaikeväärtus on 2 suurim väärtus on 10. *GatewaySize* vaikeväärtus on keskmine. Saate valida Standard_Small, Standard_Medium ja Standard_Large vahel.

## <a name="create-an-application-gateway-by-using-new-azureapplicationgateway"></a>Uus-AzureApplicationGateway abil rakenduste portaali loomine

Loob rakenduste portaali konfiguratsiooni kõigi üksuste eespool antud juhiseid. Selles näites on rakenduse lüüsi nimetatakse "appgwtest".


    $appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku

See loob rakenduste portaali koos kõigi konfiguratsiooni üksuste eespool antud juhiseid. Näites nimetatakse rakenduse lüüsi "appgwtest".


## <a name="delete-an-application-gateway"></a>Rakenduste portaali kustutamine

Rakenduste portaali kustutamiseks peate tegema selleks.

1. **Peata-AzureRmApplicationGateway** cmdlet-käsu abil saate peatada lüüsi.
2. **Eemalda – AzureRmApplicationGateway** cmdlet-käsu abil saate eemaldada lüüsi.
3. Veenduge, et lüüsi on eemaldatud **Get-AzureApplicationGateway** cmdleti abil.


### <a name="step-1"></a>Samm 1

Saada taotlus lüüsi objekti ja seostada muutuja "$getgw".

    $getgw =  Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

### <a name="step-2"></a>Samm 2

**Lõpeta – AzureRmApplicationGateway** abil rakenduse lüüsi peatamine. See näidis kuvatakse **Peata-AzureRmApplicationGateway** cmdlet esimese rea, millele järgneb väljund.

    PS C:\> Stop-AzureRmApplicationGateway -ApplicationGateway $getgw  

    VERBOSE: 9:49:34 PM - Begin Operation: Stop-AzureApplicationGateway
    VERBOSE: 10:10:06 PM - Completed Operation: Stop-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   ce6c6c95-77b4-2118-9d65-e29defadffb8

Kui rakenduse lüüsi on peatatud olekus, **Eemalda-AzureRmApplicationGateway** cmdlet-käsu abil teenuse eemaldamine.


    PS C:\> Remove-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Force

    VERBOSE: 10:49:34 PM - Begin Operation: Remove-AzureApplicationGateway
    VERBOSE: 10:50:36 PM - Completed Operation: Remove-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   055f3a96-8681-2094-a304-8d9a11ad8301

>[AZURE.NOTE] Funktsiooni **-jõustada** parameetrit saab kasutada mis tahes kinnitusteate eemaldamine.


Veenduge, et teenus on eemaldatud, saate kasutada cmdlet-käsu **Get-AzureRmApplicationGateway** . Seda toimingut ei ole vaja.


    PS C:\>Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

    VERBOSE: 10:52:46 PM - Begin Operation: Get-AzureApplicationGateway

    Get-AzureApplicationGateway : ResourceNotFound: The gateway does not exist.
    .....

## <a name="next-steps"></a>Järgmised sammud

Kui soovite konfigureerida SSL offload, lugege teemat [konfigureerimine rakenduste portaali SSL offload](application-gateway-ssl.md).

Kui soovite konfigureerida rakenduste portaali kasutamiseks on ILB, lugege teemat [rakenduste abil on sisemine laadi koormusetasakaalustusteenuse (ILB) portaali loomine](application-gateway-ilb.md).

Kui soovite lisateavet laadimine tasakaalustavad suvandid üldiselt, lugege teemat:

- [Azure'i laadi koormusetasakaalustusteenuse](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure'i liikluse haldur](https://azure.microsoft.com/documentation/services/traffic-manager/)
