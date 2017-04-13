<properties
    pageTitle="SSL-i poliitika ja lõpuni SSL-i ja rakenduse lüüsi konfigureerimine | Microsoft Azure'i"
    description="Selles artiklis kirjeldatakse, kuidas konfigureerida lõpuni SSL-i rakenduse lüüsi Azure'i ressursihaldur PowerShelli abil"
    services="application-gateway"
    documentationCenter="na"
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

# <a name="configure-ssl-policy-and-end-to-end-ssl-with-application-gateway-using-powershell"></a>SSL-i poliitika ja lõpuni SSL-i ja PowerShelli kaudu rakenduste lüüsi konfigureerimine

## <a name="overview"></a>Ülevaade

Rakenduse lüüsi toetab krüptimist lõpuni liikluse. Rakenduse lüüsi see lõpetades rakenduse Gateway SSL-ühenduse. Lüüsi seejärel marsruutimise reeglite kehtib liiklust, uuesti krüptib paketi ja edastab paketi et vastav kirjutamata, mis on määratletud marsruutimise reeglite alusel. Vastuse veebiserver läheb läbi sama protsessi tagasi lõppkasutaja.

Teise funktsiooni selle rakenduse lüüsi toetab on keelamine teatud SSL-protokolli versioone. Lüüsi rakendus toetab keelamine järgmised protokolli versiooni; TLSv1.0, TLSv1.1 ja TLSv1.2.

> [AZURE.NOTE] SSL-i 2.0 ja SSL 3.0 on vaikimisi välja lülitatud ja ei saa lubatud. Need peetakse turvamata ja ei saa kasutada rakenduse lüüsi

![pilt][scenario]

## <a name="scenario"></a>Stsenaarium

Selle stsenaariumi korral saate teada, kuidas luua PowerShelli kaudu lõpuni SSL-i abil rakenduste portaali.

Selle stsenaariumi järgmist:

- Ressursi rühma nimega "appgw rg" loomine
- Luua nimega "appgwvnet" koos reserveeritud CIDR plokk 10.0.0.0/16 virtuaalse võrgu.
- Looge kaks alamvõrku nimega "appgwsubnet" ja "appsubnet".
- Saate luua väike rakendus lüüsi toetavad lõpuni SSL-krüptimist, mis keelab teatud SSL-i protokollid.

## <a name="before-you-begin"></a>Enne alustamist

Konfigureerimiseks koos rakenduste portaali lõpuni SSL-i sert on vaja lüüsi ja serdid on nõutavad taustväärtus serverid. Lüüsi sert kasutatakse krüptimiseks ja dekrüptida saadetud SSL-i liiklust. Lüüsi sert peab olema isikuandmete vahetus (pfx) vormingus. See failivorming võimaldab privaatvõti eksporditava mis on rakenduse lüüsi krüptimine ja dekrüptimine liiklust.

Selle taustväärtus peab olema lõpuni SSL-krüptimist whitelisted rakenduse Gateway. Selleks on taustaprogrammid avalik sertifikaat üleslaadimine rakenduse lüüsi. See tagab rakenduse lüüsi suhtleb ainult teadaolevad kirjutamata eksemplarid. See tagab täpsemaks side lõpuni.

Selle protsessi on kirjeldatud järgmist:

## <a name="create-the-resource-group"></a>Ressursi rühma loomine

Selles jaotises juhendab teid ressursirühma, mis sisaldab rakenduse lüüsi loomine.

### <a name="step-1"></a>Samm 1

Azure'i kontosse sisse logida.

    Login-AzureRmAccount

### <a name="step-2"></a>Samm 2

Valige tellimus, kasutada seda stsenaariumi.

    Select-AzureRmsubscription -SubscriptionName "<Subscription name>"

### <a name="step-3"></a>Samm 3

Looge ressursirühma (Jäta see juhis, kui kasutate ressursside olemasolevasse rühma).

    New-AzureRmResourceGroup -Name appgw-rg -Location "West US"

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Virtuaalse võrgu ja alamvõrgu, lüüsi rakenduse loomine

Järgmises näites luuakse virtuaalse võrgu ja kahe alamvõrku. Ühe alamvõrgu kasutatakse pikalt rakenduse lüüsi. Muud alamvõrgu kasutatakse taustaprogrammid, majutusteenuse veebirakenduse.

### <a name="step-1"></a>Samm 1

Määrake on aadressi valik alamvõrgu kasutada rakenduse lüüsi ise.

    $gwSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -AddressPrefix 10.0.0.0/24

> [AZURE.NOTE] Alamvõrku, mis on konfigureeritud lubama rakenduse lüüsi olema õigesti suurusega. Rakenduste portaali saab konfigureerida kuni 10 eksemplari. Iga eksemplari võtab alamvõrgu 1 IP-aadressi. Liiga väike on alamvõrgu võivad negatiivselt mõjutada skaala läbi rakenduste portaali.

### <a name="step-2"></a>Samm 2

Määrake on aadressi valik kirjutamata aadress pool kasutamiseks.

    $nicSubnet = New-AzureRmVirtualNetworkSubnetConfig  -Name 'appsubnet' -AddressPrefix 10.0.2.0/24

### <a name="step-3"></a>Samm 3

Saate luua virtuaalse võrgu alamvõrku, mis on määratletud eelmist toimingut.

    $vnet = New-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $gwSubnet, $nicSubnet

### <a name="step-4"></a>Samm 4

Saate tuua virtuaalse võrguressurssi ja alamvõrgu ressursse, mis kasutada järgmist:

    $vnet = Get-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg
    $gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -VirtualNetwork $vnet
    $nicSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'appsubnet' -VirtualNetwork $vnet

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Luua avaliku IP-aadressi ees konfigureerimine

Avaliku IP ressursi kasutatakse rakenduste portaali loomine. Avaliku IP-aadressi kasutatakse järgmist toimingut.

    $publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -Name 'publicIP01' -Location "West US" -AllocationMethod Dynamic

> [AZURE.IMPORTANT] Rakenduse lüüsi ei toeta domeeni sildiga määratletud loodud avaliku IP-aadressi kasutamine. Toetatakse ainult avaliku IP sildiga dünaamiliselt loodud domeeni. Kui vajate rakenduste lüüsi sõbralik DNS-i nimi, on soovitatav kasutada pseudonüümina CNAME-kirje.

## <a name="create-an-application-gateway-configuration-object"></a>Rakenduse lüüsi konfigureerimise objekti loomine

Kõigi konfiguratsiooni üksuste peab häälestamine enne rakenduste portaali loomine. Järgmiste juhiste loomine konfiguratsiooni üksused, mida on vaja rakenduse lüüsi ressurss.

### <a name="step-1"></a>Samm 1

Saate luua rakenduse gateway IP konfiguratsiooni, selle sätte valimisel konfigureeritakse mis alamvõrgu lüüsi rakendus kasutab. Lüüsi rakenduse käivitamisel jätkab kaudu alamvõrku, mis on konfigureeritud IP-aadress ja marsruudib võrguliiklust tagaandmebaas IP kogumi IP-aadressid. Pidage meeles, et iga eksemplari võtab IP-aadress.

    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name 'gwconfig' -Subnet $gwSubnet

### <a name="step-2"></a>Samm 2

Saate luua ees IP-konfiguratsiooni, see säte kaardid avalik või privaatne ip-aadress ees rakenduse lüüsi. Järgmist toimingut seostab ees IP konfiguratsiooni eelmises etapis avaliku IP-aadressi.

    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name 'fip01' -PublicIPAddress $publicip

### <a name="step-3"></a>Samm 3

Konfigureerida tagaandmebaas IP address rakenduskausta kirjutamata web serverid IP-aadressid. IP-aadressid on IP-aadressid, mis saadetakse võrguliiklust, mis pärineb ees IP lõpp-punkti. Saate asendada järgmisi IP-aadresse lisada oma rakenduse IP address lõpp-punktid.

    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name 'pool01' -BackendIPAddresses 1.1.1.1, 2.2.2.2, 3.3.3.3

> [AZURE.NOTE] Täielik domeeninimi (FQDN) on ka ip-aadress kirjutamata serverite asemel sobiv väärtus parameetrit - BackendFqdns abil.

### <a name="step-4"></a>Samm 4

Konfigureerige ees IP port avaliku IP lõpp-punkti. See port on lõppkasutajad ühenduse port.

    $fp = New-AzureRmApplicationGatewayFrontendPort -Name 'port01'  -Port 443

### <a name="step-5"></a>Juhis 5

Konfigureerida rakendus lüüsi sert. Selle serdi abil dekrüptida ja uuesti krüptida liikluse rakenduse lüüsi.

    $cert = New-AzureRmApplicationGatewaySslCertificate -Name cert01 -CertificateFile <full path to .pfx file> -Password <password for certificate file>

> [AZURE.NOTE] See näide konfigureerib SSL-ühenduse kasutatavat serti. Serdi peab olema pfx-vormingus ja parooli peab olema 4 – 12 märki.

### <a name="step-6"></a>Juhist 6

Saate luua rakenduse lüüsi HTTP kuulajale. Määrake ees ip konfiguratsiooni, portide ja SSL-i serdi kasutada.

    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01 -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SslCertificate $cert

### <a name="step-7"></a>Juhis 7

Laadige üles sert, kasutada ressursside SSL-i lubatud taustväärtus.

> [AZURE.NOTE] Vaikimisi juures saab avalik võti **vaikimisi** SSL siduvad tagasi end IP-aadress ja võrreldakse avaliku võtmeväärtuse, et avalik võti väärtus siin. Välja avalik võti ei pruugi olla ettenähtud saidi, millele on liikluse flow **kui** kasutate hosti päised ja SNI back-end. Kahtluse korral külastage https://127.0.0.1/ tagasi lõpetatakse, millist serti kasutatakse **vaikimisi** SSL-i sidumine kinnitamiseks. Avalik võti: taotluse abil saate selles jaotises. Kui kasutate hosti päised ja SNI HTTPS sidumiste ja te ei saa vastus ja serdi taotluse käsitsi brauseri abil https://127.0.0.1/ tagasi-otsas, peab häälestamine vaikimisi SSL-i sidumine tagasi-otsas. Kui te seda teete, sondid ei õnnestu ja selle tagaandmebaas olema ei ole whitelisted.

    $authcert = New-AzureRmApplicationGatewayAuthenticationCertificate -Name 'whitelistcert1' -CertificateFile C:\users\gwallace\Desktop\cert.cer

> [AZURE.NOTE] Selles etapis tuleb esitatud peaks olema avalik võti pfx cert kohal olevat taustväärtus. Eksportige installitud kirjutamata Serveri sert (mitte juurkausta sert). CER vormindamine ning kasutada seda toimingut. See toiming ehk taustväärtus lüüsi rakenduse abil.

### <a name="step-8"></a>Samm 8

Rakenduse lüüsi tagaandmebaas http sätete konfigureerimine. Määrake http sätted eelmises juhises üles laaditud serdi.

    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name 'setting01' -Port 443 -Protocol Https -CookieBasedAffinity Enabled -AuthenticationCertificates $authcert

### <a name="step-9"></a>Samm 9

Laadi koormusetasakaalustusteenuse marsruutimine reegli, mis konfigureerib käitumise laadimine koormusetasakaalustusteenuse luua. Selles näites tavaline round jaan reegli loomist.

    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name 'rule01' -RuleType basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

### <a name="step-10"></a>Samm 10

Konfigureerida rakendus lüüsi eksemplari suurust.  Saadaval suurused on **Standard\_väike**, **Standard\_keskmise**, ja **Standard\_suur**.  Maht, on saadaval 1 kuni 10.

    $sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

>[AZURE.NOTE] Eksemplari arv 1 saab valida katsetamiseks. See on oluline teada, et iga eksemplari arv jaotises kahel ei hõlma SLA ja on seega pole soovitatav. Väike lüüsid on kasutusel arendaja test, mitte tootmiseks.

### <a name="step-11"></a>Samm 11

SSL-i poliitika kasutada rakenduse lüüsi konfigureerimine. Lüüsi rakendus toetab võimalus keelata teatud SSL-protokolli versioone.

Protokolli versiooni, mis võib olla keelatud loendi järgmised väärtused.

- **TLSv1_0**
- **TLSv1_1**
- **TLSv1_2**

Järgmises näites keelab TLSv1\_0.

    $sslPolicy = New-AzureRmApplicationGatewaySslPolicy -DisabledSslProtocols TLSv1_0

## <a name="create-the-application-gateway"></a>Lüüsi rakenduse loomine

Kasuta eelmist toimingut, saate luua rakenduse lüüsi. Lüüsi loomine on pikaajalise protsess.

    $appgw = New-AzureRmApplicationGateway -Name appgateway -SslCertificates $cert -ResourceGroupName "appgw-rg" -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -SslPolicy $sslPolicy -AuthenticationCertificates $authcert -Verbose

## <a name="disable-ssl-protocol-versions-on-an-existing-application-gateway"></a>SSL-i protokolli versiooni olemasoleva rakenduste portaali keelamine

Eelmist toimingut teid läbi lõpuni SSL-i rakenduse loomine ja keelamine teatud SSL-protokolli versioonid. Järgmises näites keelatakse teatud SSL-i poliitika olemasoleva rakenduste portaali.

### <a name="step-1"></a>Samm 1

Saate tuua rakenduse lüüsi värskendamiseks.

    $gw = Get-AzureRmApplicationGateway -Name AdatumAppGateway -ResourceGroupName AdatumAppGatewayRG

### <a name="step-2"></a>Samm 2

SSL-i poliitika määratleda. Järgmises näites TLSv1.0 ja TLSv1.1 on keelatud.

    Set-AzureRmApplicationGatewaySslPolicy -DisabledSslProtocols TLSv1_0, TLSv1_1 -ApplicationGateway $gw

### <a name="step-3"></a>Samm 3

Lõpetuseks, värskendage lüüs. See on oluline Pange tähele, et see viimane toiming pikaajalise tööülesande. Kui see on valmis, konfigureeritakse lõpuni SSL-i rakenduse lüüsi.

    $gw | Set-AzureRmApplicationGateway

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

Lisateavet Web rakenduse tulemüüriga rakenduste portaali kaudu oma veebirakenduste Turvalisus kõvendid külastades [Web rakenduse tulemüüri ülevaade](application-gateway-webapplicationfirewall-overview.md)

[scenario]: ./media/application-gateway-end-to-end-ssl-powershell/scenario.png
