<properties
   pageTitle="Rakenduste portaali SSL offload jaoks konfigureerida Azure ressursihaldur | Microsoft Azure'i"
   description="Sellelt lehelt leiate juhiseid, et luua rakenduste portaali SSL offload Azure'i ressursihaldur abil"
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
   ms.date="09/09/2016"
   ms.author="gwallace"/>

# <a name="configure-an-application-gateway-for-ssl-offload-by-using-azure-resource-manager"></a>Azure'i ressursihaldur abil rakenduste portaali jaoks SSL-i offload konfigureerimine

> [AZURE.SELECTOR]
-[Azure'i portaalis](application-gateway-ssl-portal.md)
-[Azure ressursihaldur PowerShelli](application-gateway-ssl-arm.md)
-[Azure klassikaline PowerShelli](application-gateway-ssl.md)

 Azure'i rakenduse lüüsi saab konfigureerida Gateway vältimiseks kulukas SSL dekrüptimine tööülesandeid juhtuda web serveripargi turvasoklite kiht (SSL) seansi lõpetamiseks. SSL-i offload lihtsustab ka ees serveri häälestamine ja haldamine veebirakenduse.

## <a name="before-you-begin"></a>Enne alustamist

1. Installige uusim versioon Azure PowerShelli cmdletid Web platvormi Installeri abil. Saate alla laadida ja installida **Windows PowerShelli** jaotisest [allalaadimiste lehe](https://azure.microsoft.com/downloads/)uusima versiooni.
2. Saate luua virtuaalse võrgu ja alamvõrgu, rakenduse lüüsi. Veenduge, et kasutate cloud kasutuselevõttu ega virtuaalmasinates alamvõrgu. Rakenduse lüüsi peab olema ise virtuaalse alamvõrku.
3. Saate konfigureerida kasutama rakenduse lüüsi serverid peab olemas või nende lõpp-punktid loonud kas virtuaalse võrgu või avaliku IP/VIP määratud.

## <a name="what-is-required-to-create-an-application-gateway"></a>Mida on vaja luua rakenduste portaali?


- **Tagaandmebaas serveri pool:** Tagaandmebaas serverid IP-aadresside loend. IP-aadresside loetletud kuuluma kas virtuaalse alamvõrku või tuleks avaliku IP/VIP.
- **Tagaandmebaas serverisätete pool:** Igal pool on sätted, nt portide, Protocol (protokoll) ja küpsise vastavalt osaleja. Need sätted on seotud on ja rakendatakse kõikides serverites maksimaalselt pool.
- **Ees port:** See port on avatud rakenduse lüüsi avaliku port. Liikluse tabab seda porti ja seejärel ümbersuunamist üks tagaandmebaas serverid.
- **Kuulajale:** Kuulajale on ees port, protokolli (Http- või Https need sätted on tõstutundlik), ja SSL-i serdi nimi (kui offload SSL-i konfigureerimine).
- **Reegel:** Reegli seob kuulajale ja tagaandmebaas server pargis ja määratleb, milliseid tagaandmebaas serveri rakenduskausta liiklus tuleks suunata kui see tabab kindla kuulajale. Praegu on toetatud ainult *tavaline* reegel. *Tavaline* reegel on ringi-jaan jaotust.

**Täiendavate konfiguratsiooni märkmed**

SSL-sertide paigutus, **HttpListener** protokolli tuleks muuta *Https* (tõstutundlik). **SslCertificate** element on lisatud **HttpListener** muutuja väärtusega konfigureeritud SSL-sert. Peaks uusimale ees pordi 443.

**Lubamiseks küpsise vastavalt osaleja**: rakenduste portaali saate konfigureerida nii, et tagada, et kliendi seansi taotluse on alati suunatud sama VM serveripargis veebi. Selle stsenaariumi tehakse süstena seansi küpsise, mis võimaldab lüüsi, mis suunaks liikluse õigesti. Küpsise vastavalt osaleja lubamiseks määrake **CookieBasedAffinity** *lubatud* **BackendHttpSettings** elementi.


## <a name="create-an-application-gateway"></a>Rakenduste portaali loomine

Azure'i klassikaline juurutamise mudeli ja Azure ressursihaldur vahe on loodud rakenduste portaali ja üksused, mida tuleb konfigureerida.

Ressursi halduri rakenduste portaali kõik komponendid on konfigureeritud eraldi ja seejärel panete koos lüüsi ressurssi loomiseks.


Siin on toimingud, mis on vaja luua rakenduste portaali.

1. Ressursihaldur jaoks ressursi rühma loomine
2. Virtuaalse võrgu, alamvõrgu ja avaliku IP rakenduse lüüsi loomine
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

Looge ressursirühma (Jäta see juhis, kui kasutate ressursside olemasolevasse rühma).

    New-AzureRmResourceGroup -Name appgw-rg -Location "West US"

Azure'i ressursihaldur nõuab kõigi asukoha määramiseks. Selle sätte kasutatakse vaikeasukoha selle ressursi jaotises ressursid. Veenduge, et kõik käsud rakenduste portaali loomiseks kasutab sama ressursirühma.

Ülaltoodud näites lõime ressursirühma nimega "appgw RG" ja asukoht "Lääne meie".

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Virtuaalse võrgu ja alamvõrgu, lüüsi rakenduse loomine

Järgmises näites kirjeldatakse, kuidas luua virtuaalse võrgu ressursihaldur abil.

### <a name="step-1"></a>Samm 1

    $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24

See näide määrab aadress vahemiku 10.0.0.0/24 alamvõrgu muutuja virtuaalse võrgu loomiseks kasutada.

### <a name="step-2"></a>Samm 2

    $vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet

See näidis loob virtuaalse võrgu nimega "appgwvnet" ressursi rühma "appgw-rg" 10.0.0.0/16 eesliite kasutamine alamvõrgu 10.0.0.0/24 Lääne USA regiooni jaoks.

### <a name="step-3"></a>Samm 3

    $subnet = $vnet.Subnets[0]

See näide määrab alamvõrgu objekti muutuja $subnet järgmiste juhiste juurde.

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Luua avaliku IP-aadressi ees konfigureerimine

    $publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name publicIP01 -location "West US" -AllocationMethod Dynamic

See näidis loob avaliku IP ressursi "publicIP01" ressursi rühma "appgw-rg" Lääne USA regiooni jaoks.


## <a name="create-an-application-gateway-configuration-object"></a>Rakenduse lüüsi konfigureerimise objekti loomine

### <a name="step-1"></a>Samm 1

    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet

Selles näites luuakse rakenduse gateway IP konfiguratsiooni nimega "gatewayIP01". Lüüsi rakenduse käivitamisel jätkab kaudu alamvõrku, mis on konfigureeritud IP-aadress ja IP-aadresside tagaandmebaas IP kogumi võrguliiklust marsruutida. Pidage meeles, et iga eksemplari võtab IP-aadress.

### <a name="step-2"></a>Samm 2

    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221,134.170.185.50

Selle valimi konfigureerib tagaandmebaas IP address pool nimega "pool01" koos IP-aadresside "134.170.185.46, 134.170.188.221,134.170.185.50." Need väärtused on IP-aadressid, mis saadetakse võrguliiklust, mis pärineb ees IP lõpp-punkti. Asendage IP-aadresside eelmises näites IP-aadressid web rakenduse lõpp-punktid.

### <a name="step-3"></a>Samm 3

    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Enabled

Selle valimi konfigureerib rakenduste lüüsi sätte "poolsetting01" abil tagaandmebaas rakenduskausta koormusetasakaalustusega võrguliiklust.

### <a name="step-4"></a>Samm 4

    $fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 443

See näide konfigureerib ees IP port nimega "frontendport01" avaliku IP lõpp-punkti.

### <a name="step-5"></a>Juhis 5

    $cert = New-AzureRmApplicationGatewaySslCertificate -Name cert01 -CertificateFile <full path for certificate file> -Password ‘<password>’

See näide konfigureerib SSL-ühenduse kasutatavat serti. Serdi peab olema pfx-vormingus ja parooli peab olema 4 – 12 märki.

### <a name="step-6"></a>Juhist 6

    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip

See näidis loob ees IP konfiguratsiooni nimega "fipconfig01" ja seostab ees IP konfiguratsiooni avaliku IP-aadressi.

### <a name="step-7"></a>Juhis 7

    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SslCertificate $cert


See näidis loob kuulajale nimi "listener01" ja seob ees pordi ees IP-konfiguratsiooni ja sert.

### <a name="step-8"></a>Samm 8

    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

See näidis loob laadi koormusetasakaalustusteenuse marsruutimine reeglit nimega "rule01", mis konfigureerib laadi koormusetasakaalustusteenuse käitumine.

### <a name="step-9"></a>Samm 9

    $sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

See näide konfigureerib rakenduste lüüsi eksemplari suurust.

>[AZURE.NOTE]  *InstanceCount* vaikeväärtus on 2 suurim väärtus on 10. *GatewaySize* vaikeväärtus on keskmine. Saate valida Standard_Small, Standard_Medium ja Standard_Large vahel.

## <a name="create-an-application-gateway-by-using-new-azureapplicationgateway"></a>Uus-AzureApplicationGateway abil rakenduste portaali loomine

    $appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -SslCertificates $cert

See näidis loob rakenduste portaali konfiguratsiooni kõigi üksuste eelmist toimingut. Näites nimetatakse rakenduse lüüsi "appgwtest".

## <a name="get-application-gateway-dns-name"></a>Saada rakenduse lüüsi DNS-i nimi

Kui lüüsi on loodud, on järgmiseks esiosa side konfigureerimine. Avaliku IP kasutamisel nõuab rakenduse lüüsi dünaamiliselt DNS-i nimi, mis ei ole lubatud. Lõppkasutajad tabab rakenduse lüüsi CNAME-kirje tagamiseks saab rakenduse lüüsi avaliku lõpp-punkti. [Kohandatud domeeni nimi Azure konfigureerimine](../cloud-services/cloud-services-custom-domain-name-portal.md). Selleks saate tuua rakenduste portaali ja selle seotud IP/DNS-i nimi abil PublicIPAddress element, mis on seotud rakenduse lüüsi üksikasjad. CNAME-kirje, mis suunab selle DNS-i nimi kaks veebirakenduste loomiseks tuleks kasutada rakenduse lüüsi DNS-i nimi. A-kirjete kasutamine ei ole soovitatav, kuna VIP võivad muutuda taaskäivitamisel rakenduse lüüsi.
    
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

Kui soovite konfigureerida rakenduste portaali kasutamiseks on sisemine laadi koormusetasakaalustusteenuse (ILB), lugege teemat [rakenduste abil on sisemine laadi koormusetasakaalustusteenuse (ILB) portaali loomine](application-gateway-ilb.md).

Kui soovite lisateavet laadimine tasakaalustavad suvandid üldiselt, lugege teemat:

- [Azure'i laadi koormusetasakaalustusteenuse](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure'i liikluse haldur](https://azure.microsoft.com/documentation/services/traffic-manager/)
