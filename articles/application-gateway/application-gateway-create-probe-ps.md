<properties
   pageTitle="Luua kohandatud juures rakenduste portaali sisse ressursihaldur PowerShelli abil | Microsoft Azure'i"
   description="Siit saate teada, kuidas luua kohandatud juures rakenduste portaali sisse ressursihaldur PowerShelli abil"
   services="application-gateway"
   documentationCenter="na"
   authors="georgewallace"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/06/2016"
   ms.author="gwallace" />

# <a name="create-a-custom-probe-for-azure-application-gateway-by-using-powershell-for-azure-resource-manager"></a>Luua kohandatud juures Azure'i rakenduse lüüsi Azure'i ressursihaldur PowerShelli abil

> [AZURE.SELECTOR]
- [Azure'i portaal](application-gateway-create-probe-portal.md)
- [Azure'i ressursihaldur PowerShelli](application-gateway-create-probe-ps.md)
- [Azure'i klassikaline PowerShelli](application-gateway-create-probe-classic-ps.md)

[AZURE.INCLUDE [azure-probe-intro-include](../../includes/application-gateway-create-probe-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][klassikaline juurutamise mudel](application-gateway-create-probe-classic-ps.md).

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

### <a name="step-1"></a>Samm 1

Logi sisse – AzureRmAccount abil autentida.

    Login-AzureRmAccount

### <a name="step-2"></a>Samm 2

Kontrollige konto tellimused.

    Get-AzureRmSubscription

### <a name="step-3"></a>Samm 3

Valida Azure tellimuste kasutada. <BR>


    Select-AzureRmSubscription -Subscriptionid "GUID of subscription"


### <a name="step-4"></a>Samm 4

Looge ressursirühma (Jäta see juhis, kui kasutate ressursside olemasolevasse rühma).

    New-AzureRmResourceGroup -Name appgw-rg -Location "West US"

Azure'i ressursihaldur nõuab kõigi asukoha määramiseks. Selle asukoha kasutatakse vaikeasukoha selle ressursi jaotises ressursid. Veenduge, et kõik käsud rakenduste portaali loomiseks kasutada ühte ressursirühma.

Ülaltoodud näites lõime ressursirühma nimega "appgw RG" ja asukoht "Lääne meie".

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Virtuaalse võrgu ja alamvõrgu, lüüsi rakenduse loomine

Virtuaalse võrgu ja alamvõrgu, rakenduse lüüsi loomine toimige järgmiselt.

### <a name="step-1"></a>Samm 1


Määrata aadress vahemiku 10.0.0.0/24 alamvõrgu muutuja virtuaalse võrgu loomiseks kasutada.

    $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24

### <a name="step-2"></a>Samm 2

Luua virtuaalse võrgu nimega "appgwvnet" ressursi rühma "appgw-rg" 10.0.0.0/16 eesliite kasutamine alamvõrgu 10.0.0.0/24 Lääne USA regiooni jaoks.

    $vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet


### <a name="step-3"></a>Samm 3

Määrata alamvõrgu muutuja jaoks järgmised toimingud, mis rakenduste portaali loomine.

    $subnet = $vnet.Subnets[0]

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Luua avaliku IP-aadressi ees konfigureerimine


Saate luua avaliku IP ressursi "publicIP01" ressursi rühma "appgw-rg" Lääne USA regiooni jaoks.

    $publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -Name publicIP01 -Location "West US" -AllocationMethod Dynamic


## <a name="create-an-application-gateway-configuration-object-with-a-custom-probe"></a>Luua kohandatud juures objekti rakenduse lüüsi konfigureerimine

Seadistage kõigi konfiguratsiooni üksuste enne rakenduste portaali loomine. Järgmiste juhiste loomine konfiguratsiooni üksused, mida on vaja rakenduse lüüsi ressurss.

### <a name="step-1"></a>Samm 1

Looge rakenduse gateway IP konfiguratsiooni nimega "gatewayIP01". Lüüsi rakenduse käivitamisel jätkab kaudu alamvõrku, mis on konfigureeritud IP-aadress ja IP-aadresside tagaandmebaas IP kogumi võrguliiklust marsruutida. Pidage meeles, et iga eksemplari võtab IP-aadress.

    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet


### <a name="step-2"></a>Samm 2


Konfigureerimine tagaandmebaas IP address kausta nimega "pool01" koos IP-aadresside "134.170.185.46, 134.170.188.221,134.170.185.50". Need väärtused on IP-aadressid, mis saadetakse võrguliiklust, mis pärineb ees IP lõpp-punkti. Saate asendada IP-aadresside eespool lisada oma rakenduse IP address lõpp-punktid.

    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221,134.170.185.50


### <a name="step-3"></a>Samm 3


Selles etapis tuleb on konfigureeritud kohandatud juures.

Kasutatud parameetrid on:

- **Intervall** – konfigureerib juures intervall kontrolle sekundites.
- **Ajalõpp** - määratleb juures ajalõpp HTTP vastuse kontroll.
- **-Hostname ja tee** - täielikku URL-i tee, mis kasutavad rakenduse lüüsi eksemplari seisundi määratlemiseks. Näiteks kui teil on veebisaidi http://contoso.com/, siis kohandatud juures saate konfigureerida "http://contoso.com/path/custompath.htm" on eduka HTTP vastuse juures kontrollimiseks.
- **UnhealthyThreshold** - vaja lipu tagaandmebaas eksemplari nimega nurjunud HTTP vastuste arvu *vigane*.

<BR>

    $probe = New-AzureRmApplicationGatewayProbeConfig -Name probe01 -Protocol Http -HostName "contoso.com" -Path "/path/path.htm" -Interval 30 -Timeout 120 -UnhealthyThreshold 8

### <a name="step-4"></a>Samm 4

Rakenduse lüüsi sätte "poolsetting01" liikluse jaoks konfigureerida tagaandmebaas kogumi. See toiming on ka ajalõpp konfiguratsiooni, mis on mõeldud rakenduse lüüsi päringu tagaandmebaas rakenduskausta vastuse. Kui tagaandmebaas vastuse tabab ajalõpu piirang, rakenduse lüüsi tühistab kutse. See väärtus erineb juures ajalõpp, mis on mõeldud ainult tagaandmebaas vastuse probe kontrollid.

    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Disabled -Probe $probe -RequestTimeout 80

### <a name="step-5"></a>Juhis 5

Konfigureerige ees IP port nimega "frontendport01" avaliku IP lõpp-punkti.

    $fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01 -Port 80

### <a name="step-6"></a>Juhist 6

Luua nimega "fipconfig01" ees IP-konfiguratsiooni ja seostada ees IP konfiguratsiooni avaliku IP-aadressi.


    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip

### <a name="step-7"></a>Juhis 7

Looge kuulajale nimi "listener01" ja ees pordi ees IP-konfiguratsiooni seostada.

    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp

### <a name="step-8"></a>Samm 8

Laadi koormusetasakaalustusteenuse marsruutimine nimega "rule01", mis konfigureerib käitumise laadimine koormusetasakaalustusteenuse reegli loomine.

    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

### <a name="step-9"></a>Samm 9

Konfigureerida rakendus lüüsi eksemplari suurust.

    $sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2


>[AZURE.NOTE]  *InstanceCount* vaikeväärtus on 2 suurim väärtus on 10. *GatewaySize* vaikeväärtus on keskmine. Saate valida Standard_Small, Standard_Medium ja Standard_Large vahel.

## <a name="create-an-application-gateway-by-using-new-azurermapplicationgateway"></a>Uus-AzureRmApplicationGateway abil rakenduste portaali loomine

Saate luua rakenduste portaali konfiguratsiooni kõigi üksuste eespool antud juhiseid. Selles näites on rakenduse lüüsi nimetatakse "appgwtest".

    $appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -Probes $probe -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku

## <a name="add-a-probe-to-an-existing-application-gateway"></a>Olemasoleva rakenduste portaali on juures lisamine

Teil on neli toimingut lisada kohandatud juures olemasoleva rakenduste portaali.

### <a name="step-1"></a>Samm 1

Lüüsi ressurssi laadimine PowerShelli muutuja **Get-AzureRmApplicationGateway**abil.

    $getgw =  Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

### <a name="step-2"></a>Samm 2

Olemasoleva lüüsi konfigureerimine on juures lisada.

    $getgw = Add-AzureRmApplicationGatewayProbeConfig -ApplicationGateway $getgw -Name probe01 -Protocol Http -HostName "contoso.com" -Path "/path/custompath.htm" -Interval 30 -Timeout 120 -UnhealthyThreshold 8


Näites kohandatud juures on konfigureeritud otsida URL-i tee contoso.com/path/custompath.htm iga 30 sekundi järel. Ajalõpp lävi 120 sekundit on konfigureeritud 8 nurjunud juures taotluste maksimaalne arv.

### <a name="step-3"></a>Samm 3

Funktsiooni juures tagaandmebaas rakenduskausta sätete konfigureerimise ja ajalõpp abil lisada **-seadmine – AzureRmApplicationGatewayBackendHttpSettings**.


     $getgw = Set-AzureRmApplicationGatewayBackendHttpSettings -ApplicationGateway $getgw -Name $getgw.BackendHttpSettingsCollection.name -Port 80 -Protocol Http -CookieBasedAffinity Disabled -Probe $probe -RequestTimeout 120

### <a name="step-4"></a>Samm 4

Konfiguratsiooni salvestamiseks rakenduse lüüsi **Set-AzureRmApplicationGateway**abil.

    Set-AzureRmApplicationGateway -ApplicationGateway $getgw

## <a name="remove-a-probe-from-an-existing-application-gateway"></a>Olemasoleva rakenduste portaali on juures eemaldamine

Siit leiate juhiseid olemasoleva rakenduste portaali eemaldamine kohandatud juures.

### <a name="step-1"></a>Samm 1

Lüüsi ressurssi laadimine PowerShelli muutuja **Get-AzureRmApplicationGateway**abil.

    $getgw =  Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg


### <a name="step-2"></a>Samm 2

Eemaldada rakenduse lüüsi juures konfiguratsiooni **Eemalda-AzureRmApplicationGatewayProbeConfig**abil.

    $getgw = Remove-AzureRmApplicationGatewayProbeConfig -ApplicationGateway $getgw -Name $getgw.Probes.name

### <a name="step-3"></a>Samm 3

Värskendada tagaandmebaas rakenduskausta säte eemaldamiseks selle juures ja käivitub **-seadmine – AzureRmApplicationGatewayBackendHttpSettings**.


     $getgw = Set-AzureRmApplicationGatewayBackendHttpSettings -ApplicationGateway $getgw -Name $getgw.BackendHttpSettingsCollection.name -Port 80 -Protocol http -CookieBasedAffinity Disabled

### <a name="step-4"></a>Samm 4

Konfiguratsiooni salvestamiseks rakenduse lüüsi **Set-AzureRmApplicationGateway**abil. 

    Set-AzureRmApplicationGateway -ApplicationGateway $getgw

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

Saate teada, kuidas konfigureerida SSL-i mahalaadimine külastades [Offload SSL-i konfigureerimine](application-gateway-ssl-arm.md)

