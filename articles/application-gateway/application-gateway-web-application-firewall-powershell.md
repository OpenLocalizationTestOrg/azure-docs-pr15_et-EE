<properties
   pageTitle="Konfigureerida Web rakenduse tulemüüri uude või olemasolevasse rakenduse lüüsi | Microsoft Azure'i"
   description="Selles artiklis on toodud juhised selle kohta, kuidas olemasolevad või uued rakenduste portaali tulemüüri web rakenduse kasutamise alustamine."
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
   ms.date="10/25/2016"
   ms.author="gwallace"/>

# <a name="configure-web-application-firewall-on-a-new-or-existing-application-gateway"></a>Uue või olemasoleva rakenduse Gateway Web rakenduse tulemüüri konfigureerimine

> [AZURE.SELECTOR]
- [Azure'i portaal](application-gateway-web-application-firewall-portal.md)
- [Azure'i ressursihaldur PowerShelli](application-gateway-web-application-firewall-powershell.md)

Web rakenduse tulemüüri (WAF) Azure'i rakenduse lüüsi kaitseb veebirakenduste levinud veebipõhine eest nagu SQL süst, saitidevaheline skriptimine eest ja seansi hijacks.

Azure'i rakenduse lüüs on kiht-7 laadi koormusetasakaalustusteenuse. Kas nad on kohapealse ja pilveteenuse pakub Tõrkesiirde jõudlus marsruutimine HTTP päringuid serveritest, vahel. Rakenduse pakub paljusid rakenduse kohaletoimetamise kontrolleril (ADC) funktsioonid, sh HTTP koormusetasakaalustuseks, seansi küpsise vastavalt osaleja, turvasoklite kiht (SSL) offload, kohandatud seisund sondid, mitme saidi tugi ja paljud teised. Toetatud funktsioonide täieliku loendi saamiseks külastage rakenduse lüüsi ülevaade

Artikkel kuvatakse [lisamine web rakenduse tulemüüri olemasoleva rakenduste portaali](#add-web-application-firewall-to-an-existing-application-gateway) ja [, mis kasutab tulemüüri web rakenduse rakenduste portaali loomine](#create-an-application-gateway-with-web-application-firewall).

![pilt][scenario]

## <a name="waf-configuration-differences"></a>WAF konfiguratsiooni erinevused

Kui olete läbi lugenud [PowerShelliga rakenduste portaali loomine](application-gateway-create-gateway-arm.md), mõistate SKU sätete konfigureerimiseks rakenduste portaali loomisel. WAF pakub täiendavaid sätteid määratleda konfigureerimisel kasutatava SKU rakenduste portaali. On täiendavaid muudatusi rakenduse lüüsi ise tehtud.

**SKU** - tavalise rakenduse lüüsi ilma WAF toetab **Standard\_Small**, **Standard\_keskmise**, ja **Standard\_suur** suurused. Sissejuhatus WAF, on kaks täiendavad SKU-d, **WAF\_keskmise** ja **WAF\_suur**. Väike rakendus lüüside WAF ei toetata.

**Esimese taseme** - saadaval väärtused on **Standardne** või **WAF**. Tulemüüri Web rakenduse kasutamisel tuleb valida **WAF** .

**Režiim** – see säte on WAF režiimi. lubatud väärtused on **avastamine** ja **ennetamine**. WAF on häälestatud tuvastamise režiimis, salvestatakse kõik ohud logifaili. Vältimise režiimis üritused on endiselt sisse logitud, kuid ründaja saab rakenduse lüüsi 403 volitamata vastuse.

## <a name="add-web-application-firewall-to-an-existing-application-gateway"></a>Olemasoleva rakenduste portaali tulemüüri web rakenduse lisamine

Veenduge, et kasutate Azure PowerShelli uusim versioon. Lisateave on saadaval [Koos ressursihaldur Windows PowerShelli kasutamine](../powershell-azure-resource-manager.md).

### <a name="step-1"></a>Samm 1

Azure'i kontosse sisse logida.

    Login-AzureRmAccount

### <a name="step-2"></a>Samm 2

Valige tellimus, kasutada seda stsenaariumi.

    Select-AzureRmSubscription -SubscriptionName "<Subscription name>"

### <a name="step-3"></a>Samm 3

Lüüsi, kuhu soovite lisada rakenduse Web tulemüüri, et tuua.

    $gw = Get-AzureRmApplicationGateway -Name "AdatumGateway" -ResourceGroupName "MyResourceGroup"


### <a name="step-4"></a>Samm 4

Konfigureerida web rakenduse tulemüüri SKU-ga. Saadaval suurused on **WAF\_suur** ja **WAF\_keskmise**. Kui web rakenduse tulemüüri kasutatakse soovitud taseme peab olema **WAF**, võimsus kinnitatud määramisel kasutatava sku.

    $gw | Set-AzureRmApplicationGatewaySku -Name WAF_Large -Tier WAF -Capacity 2

### <a name="step-5"></a>Juhis 5

WAF sätteid konfigureerida, nagu järgmises näites:

**WafMode** säte, on saadaval väärtused vältimine ja tuvastamine.

    $gw | Set-AzureRmApplicationGatewayWebApplicationFirewallConfiguration -Enabled $true -FirewallMode Prevention

### <a name="step-6"></a>Juhist 6

Värskendage lüüs rakenduse eelmises toimingus määratletud sätetega.

    Set-AzureRmApplicationGateway -ApplicationGateway $gw

See käsk värskendab lüüsi rakenduse Web rakenduse tulemüüri. Soovitatav on vaadata [Rakenduse lüüsi diagnostika](application-gateway-diagnostics.md) mõistmaks, kuidas vaadata oma rakenduste portaali logisid. WAF laadist turvalisus on vaja logid regulaarselt mõista turvalisus asendi oma veebirakendusi.

## <a name="create-an-application-gateway-with-web-application-firewall"></a>Rakenduse Web tulemüüriga rakenduste portaali loomine

Järgmised toimingud teid läbi kogu protsessi algusest lõpuni Web rakenduse tulemüüriga rakenduste portaali loomise kohta.

Veenduge, et kasutate Azure PowerShelli uusim versioon. Lisateave on saadaval [Koos ressursihaldur Windows PowerShelli kasutamine](../powershell-azure-resource-manager.md).

### <a name="step-1"></a>Samm 1

Azure'i sisse logida

    Login-AzureRmAccount

Teil palutakse autentimiseks mandaat.

### <a name="step-2"></a>Samm 2

Kontrollige konto tellimused.

    Get-AzureRmSubscription

### <a name="step-3"></a>Samm 3

Valida Azure tellimuste kasutada.

    Select-AzureRmsubscription -SubscriptionName "<Subscription name>"

### <a name="step-4"></a>Samm 4

Looge ressursirühma (Jäta see juhis, kui kasutate ressursside olemasolevasse rühma).

    New-AzureRmResourceGroup -Name appgw-rg -Location "West US"

Azure'i ressursihaldur nõuab kõigi asukoha määramiseks. Selle asukoha kasutatakse vaikeasukoha selle ressursi jaotises ressursid. Veenduge, et kõik käsud rakenduste portaali loomiseks kasutab sama ressursirühma.

Eelmises näites lõime ressursirühma nimega "appgw RG" ja asukoht "Lääne meie".

>[AZURE.NOTE] Kui teil on vaja konfigureerida kohandatud juures lüüsi rakenduse jaoks, vaadake teemat [loomine rakenduste portaali kohandatud sondid PowerShelli abil](application-gateway-create-probe-ps.md). Tutvuge [kohandatud sondid ja seisundi jälgimine](application-gateway-probe-overview.md) lisateabe saamiseks.

### <a name="step-5"></a>Juhis 5

Määrake on aadressi valik alamvõrgu kasutada rakenduse lüüsi ise.

    $gwSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -AddressPrefix 10.0.0.0/24

> [AZURE.NOTE] Alamvõrku, rakendus peab olema vähemalt 28 maski bittide. See väärtus lehed 10 saadaval aadressid alamvõrgu jaoks rakenduse lüüsi eksemplari. Väiksem alamvõrku, kus te ei saa lisada mitu eksemplari rakenduse lüüsi tulevikus.

### <a name="step-6"></a>Juhist 6

Määrake on aadressi valik kirjutamata aadress pool kasutamiseks.

    $nicSubnet = New-AzureRmVirtualNetworkSubnetConfig  -Name 'appsubnet' -AddressPrefix 10.0.2.0/24

### <a name="step-7"></a>Juhis 7

Loo juhises loodud virtuaalse eelnev alamvõrku ressursi jaotises võrku: [Ressursi rühma loomine](#create-the-resource-group)

    $vnet = New-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $gwSubnet, $nicSubnet

### <a name="step-8"></a>Samm 8

Saate tuua virtuaalse võrguressurssi ning alamvõrgu ressursid kasutamiseks toimige järgmiselt:

    $vnet = Get-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg
    $gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -VirtualNetwork $vnet
    $nicSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'appsubnet' -VirtualNetwork $vnet

### <a name="step-9"></a>Samm 9

Avaliku IP ressursi kasutatakse rakenduste portaali loomine. Avaliku IP-aadressi kasutatakse ühte järgmistest:

    $publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name 'appgwpip' -Location "West US" -AllocationMethod Dynamic

> [AZURE.IMPORTANT] Rakenduse lüüsi ei toeta domeeni sildiga määratletud loodud avaliku IP-aadressi kasutamine. Toetatakse ainult avaliku IP sildiga dünaamiliselt loodud domeeni. Kui vajate rakenduste lüüsi sõbralik DNS-i nimi, on soovitatav kasutada pseudonüümina CNAME-kirje.

### <a name="step-10"></a>Samm 10

Kõigi konfiguratsiooni üksuste peab häälestamine enne rakenduste portaali loomine. Järgmiste juhiste loomine konfiguratsiooni üksused, mida on vaja rakenduse lüüsi ressurss.

Saate luua rakenduse gateway IP konfiguratsiooni, see konfigureerib mis alamvõrgu lüüsi rakendus kasutab. Lüüsi rakenduse käivitamisel jätkab kaudu alamvõrku, mis on konfigureeritud IP-aadress ja marsruudib võrguliiklust tagaandmebaas IP kogumi IP-aadressid. Pidage meeles, et iga eksemplari võtab IP-aadress.

    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name 'gwconfig' -Subnet $gwSubnet

### <a name="step-11"></a>Samm 11

Konfigureerida tagaandmebaas IP address rakenduskausta kirjutamata web serverid IP-aadressid. IP-aadressid on IP-aadressid, mis saadetakse võrguliiklust, mis pärineb ees IP lõpp-punkti. Saate asendada järgmisi IP-aadresse lisada oma rakenduse IP address lõpp-punktid.

    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name 'pool01' -BackendIPAddresses 1.1.1.1, 2.2.2.2, 3.3.3.3

### <a name="step-12"></a>Samm 12

Laadige üles sert, kasutada ressursside SSL-i lubatud taustväärtus.

    $authcert = New-AzureRmApplicationGatewayAuthenticationCertificate -Name 'whitelistcert1' -CertificateFile <full path to .cer file>

### <a name="step-13"></a>Samm 13

Rakenduse lüüsi tagaandmebaas http sätete konfigureerimine. Määrake http sätted eelmises juhises üles laaditud serdi.

    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name 'setting01' -Port 443 -Protocol Https -CookieBasedAffinity Enabled -AuthenticationCertificates $authcert

### <a name="step-14"></a>Samm 14

Konfigureerige ees IP port avaliku IP lõpp-punkti. See port on lõppkasutajad ühenduse port.

    $fp = New-AzureRmApplicationGatewayFrontendPort -Name 'port01'  -Port 443

### <a name="step-15"></a>Samm 15

Saate luua ees IP-konfiguratsiooni, seda sätet kaardid avalik või privaatne ip-aadress ees rakenduse lüüsi. Järgmist toimingut seostab ees IP konfiguratsiooni eelmises etapis avaliku IP-aadressi.

    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name 'fip01' -PublicIPAddress $publicip

### <a name="step-16"></a>Samm 16

Konfigureerida rakendus lüüsi sert. Selle serdi abil dekrüptida ja uuesti krüptida liikluse rakenduse lüüsi.

    $cert = New-AzureRmApplicationGatewaySslCertificate -Name cert01 -CertificateFile <full path to .pfx file> -Password <password for certificate file>

### <a name="step-17"></a>Samm 17

Saate luua rakenduse lüüsi HTTP kuulajale. Määrake ees ip konfiguratsiooni, portide ja SSL-i serdi kasutada.

    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01 -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SslCertificate $cert

### <a name="step-18"></a>Etapp 18

Laadi koormusetasakaalustusteenuse marsruutimine reegli, mis konfigureerib käitumise laadimine koormusetasakaalustusteenuse luua. Selles näites tavaline round jaan reegli loomist.

    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name 'rule01' -RuleType basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool
   
### <a name="step-19"></a>Samm 19

Konfigureerida rakendus lüüsi eksemplari suurust.

    $sku = New-AzureRmApplicationGatewaySku -Name WAF_Medium -Tier WAF -Capacity 2

>[AZURE.NOTE]  Saate valida **WAF\_keskmise** ja **WAF\_suur**, taseme kui WAF kasutamine on alati **WAF**. On mõni muu arv vahemikus 1 kuni 10.

### <a name="step-20"></a>Samm 20

Režiimi konfigureerimine WAF, sobivad väärtused on **vältimine** ja **tuvastamine**.

    $config = New-AzureRmApplicationGatewayWafConfig -Enabled $true -WafMode "Prevention"

### <a name="step-21"></a>Samm 21

Saate luua rakenduste portaali konfiguratsiooni kõigi üksuste eelmist toimingut. Selles näites on rakenduse lüüsi nimetatakse "appgwtest".

    $appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -WebApplicationFirewallConfig $config -SslCertificates $cert -AuthenticationCertificates $authcert

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

Saate teada, kuidas konfigureerida diagnostikalogimine logige sündmusi, mis on avastamisel või Web rakenduse tulemüüriga külastades [Rakenduse lüüsi diagnostika](application-gateway-diagnostics.md)


[scenario]: ./media/application-gateway-web-application-firewall-powershell/scenario.png