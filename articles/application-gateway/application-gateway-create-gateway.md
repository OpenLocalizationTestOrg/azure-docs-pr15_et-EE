<properties
   pageTitle="Luua, käivitage või kustutamine rakenduste portaali | Microsoft Azure'i"
   description="Sellelt lehelt leiate juhiseid, et luua, konfigureerimine, käivitamine ja Azure rakenduste portaali kustutamine"
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

# <a name="create-start-or-delete-an-application-gateway"></a>Luua, käivitage või rakenduste portaali kustutamine

> [AZURE.SELECTOR]
- [Azure'i portaal](application-gateway-create-gateway-portal.md)
- [Azure'i ressursihaldur PowerShelli](application-gateway-create-gateway-arm.md)
- [Azure'i klassikaline PowerShelli](application-gateway-create-gateway.md)
- [Azure'i ressursihaldur Mall](application-gateway-create-gateway-arm-template.md)
- [Azure'i CLI](application-gateway-create-gateway-cli.md)

Azure'i rakenduse lüüs on kiht-7 laadi koormusetasakaalustusteenuse. Kas nad on kohapealse ja pilveteenuse pakub Tõrkesiirde jõudlus marsruutimine HTTP päringuid serveritest, vahel. Rakenduse lüüsi pakub paljusid rakenduse kohaletoimetamise kontrolleril (ADC) funktsioonid, sh HTTP koormusetasakaalustuseks, seansi küpsise vastavalt osaleja, turvasoklite kiht (SSL) offload, kohandatud seisund sondid, mitme saidi tugi ja paljud teised. Toetatud funktsioonide täieliku loendi saamiseks külastage [Rakenduse lüüsi ülevaade](application-gateway-introduction.md)

Selles artiklis tutvustatakse loomine, konfigureerida, käivitamine ja rakenduste portaali kustutamine juhiseid.

## <a name="before-you-begin"></a>Enne alustamist

1. Installige uusim versioon Azure PowerShelli cmdletid Web platvormi Installeri abil. Saate alla laadida ja installida **Windows PowerShelli** jaotisest [allalaadimiste lehe](https://azure.microsoft.com/downloads/)uusima versiooni.
2. Kui teil on mõne olemasoleva virtuaalse võrgu, valige olemasoleva tühja alamvõrgu või uus alamvõrgu loomine olemasoleva virtuaalse võrgu üksnes kasutamiseks, lüüsi rakenduse. Ei saa juurutada rakenduse lüüs kavatsete juurutada taha rakenduse lüüsi juhul, kui kasutatakse vnet silmitsemine ressursside virtuaalse võrku. Lisateabe saamiseks külastage [Vnet silmitsemine](../virtual-network/virtual-network-peering-overview.md)
3. Veenduge, et teil on kehtiv alamvõrgu töötamise virtuaalse võrgu. Veenduge, et kasutate cloud kasutuselevõttu ega virtuaalmasinates alamvõrgu. Lüüsi rakendus peab olema ise virtuaalse alamvõrku.
3. Saate konfigureerida kasutama rakenduse lüüsi jaoks vajalike peab olemas või nende lõpp-punktid loonud kas virtuaalse võrgu või avaliku IP/VIP määratud.

## <a name="what-is-required-to-create-an-application-gateway"></a>Mida on vaja luua rakenduste portaali?

Kui **New-AzureApplicationGateway** käsu abil saate luua rakenduse lüüsi, ei ole konfiguratsioon on seatud sel hetkel, ja vastloodud ressurss on konfigureeritud XML- või konfiguratsiooni objekti kaudu.

Väärtused on:

- **Tagaandmebaas serveri pool:** Tagaandmebaas serverid IP-aadresside loend. IP-aadresside loetletud kuuluma kas virtuaalse alamvõrku või tuleks avaliku IP/VIP.
- **Tagaandmebaas serverisätete pool:** Igal pool on sätted, nt portide, Protocol (protokoll) ja küpsise vastavalt osaleja. Need sätted on seotud on ja rakendatakse kõikides serverites maksimaalselt pool.
- **Ees port:** See port on avatud rakenduse lüüsi avaliku port. Liikluse tabab seda porti ja seejärel ümbersuunamist üks tagaandmebaas serverid.
- **Kuulajale:** Kuulajale on ees port, protokolli (Http- või Https need väärtused on tõstutundlik), ja SSL-i serdi nimi (kui offload SSL-i konfigureerimine).
- **Reegel:** Reegli seob kuulajale ja tagaandmebaas server pargis ja määratleb, milliseid tagaandmebaas serveri rakenduskausta liiklus tuleks suunata kui see tabab kindla kuulajale.

## <a name="create-an-application-gateway"></a>Rakenduste portaali loomine

Rakenduste portaali loomiseks tehke järgmist.

1. Lüüsi ressurssi luua.
2. Konfiguratsiooni XML-faili või konfiguratsiooni objekti loomine.
3. Äsja loodud rakenduse lüüsi ressursi konfiguratsiooni Kinnita.

>[AZURE.NOTE] Kui teil on vaja konfigureerida kohandatud juures lüüsi rakenduse jaoks, vaadake teemat [loomine rakenduste portaali kohandatud sondid PowerShelli abil](application-gateway-create-probe-classic-ps.md). Tutvuge [kohandatud sondid ja seisundi jälgimine](application-gateway-probe-overview.md) lisateabe saamiseks.

![Stsenaarium näide][scenario]

### <a name="create-an-application-gateway-resource"></a>Lüüsi ressurssi loomine

Lüüsi loomine cmdletiga **New-AzureApplicationGateway** , asendades väärtused ise. Arveldamine lüüsi ei käivitu sel hetkel. Arveldamine algab hiljem toimingut, kui lüüsi on edukalt.

Järgmises näites luuakse rakenduste portaali abil virtuaalse võrgust, mida nimetatakse "testvnet1" ja alamvõrku, mis on nimega "alamvõrgu-1".


    New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")

    VERBOSE: 4:31:35 PM - Begin Operation: New-AzureApplicationGateway
    VERBOSE: 4:32:37 PM - Completed Operation: New-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   55ef0460-825d-2981-ad20-b9a8af41b399


 *Kirjeldus*, *InstanceCount*ja *GatewaySize* on valikulised parameetrid.

Kinnitamiseks, et lüüsi on loodud, saate kasutada cmdlet-käsu **Get-AzureApplicationGateway** .

    Get-AzureApplicationGateway AppGwTest
    Name          : AppGwTest
    Description   :
    VnetName      : testvnet1
    Subnets       : {Subnet-1}
    InstanceCount : 2
    GatewaySize   : Medium
    State         : Stopped
    VirtualIPs    : {}
    DnsName       :

>[AZURE.NOTE]  *InstanceCount* vaikeväärtus on 2 suurim väärtus on 10. *GatewaySize* vaikeväärtus on keskmine. Saate valida väike, Keskmine ja suur.

*VirtualIPs* ja *dnsnameWindows* on kuvatud nii tühi, kuna lüüsi pole veel alanud. Need on loodud pärast lüüs on töökorras olekus.

## <a name="configure-the-application-gateway"></a>Rakenduse lüüsi konfigureerimine

XML-i või konfiguratsiooni objekti abil saate konfigureerida rakendus lüüsi.

## <a name="configure-the-application-gateway-by-using-xml"></a>XML-i abil rakenduses lüüsi konfigureerimine

Järgmises näites saate XML-faili kõigi rakenduste lüüsi sätete konfigureerimine ja kinnitage need lüüsi ressurssi.  

### <a name="step-1"></a>Samm 1  

Kopeerige järgmine tekst Notepadi.

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
        <FrontendPorts>
            <FrontendPort>
                <Name>(name-of-your-frontend-port)</Name>
                <Port>(port number)</Port>
            </FrontendPort>
        </FrontendPorts>
        <BackendAddressPools>
            <BackendAddressPool>
                <Name>(name-of-your-backend-pool)</Name>
                <IPAddresses>
                    <IPAddress>(your-IP-address-for-backend-pool)</IPAddress>
                    <IPAddress>(your-second-IP-address-for-backend-pool)</IPAddress>
                </IPAddresses>
            </BackendAddressPool>
        </BackendAddressPools>
        <BackendHttpSettingsList>
            <BackendHttpSettings>
                <Name>(backend-setting-name-to-configure-rule)</Name>
                <Port>80</Port>
                <Protocol>[Http|Https]</Protocol>
                <CookieBasedAffinity>Enabled</CookieBasedAffinity>
            </BackendHttpSettings>
        </BackendHttpSettingsList>
        <HttpListeners>
            <HttpListener>
                <Name>(name-of-the-listener)</Name>
                <FrontendPort>(name-of-your-frontend-port)</FrontendPort>
                <Protocol>[Http|Https]</Protocol>
            </HttpListener>
        </HttpListeners>
        <HttpLoadBalancingRules>
            <HttpLoadBalancingRule>
                <Name>(name-of-load-balancing-rule)</Name>
                <Type>basic</Type>
                <BackendHttpSettings>(backend-setting-name-to-configure-rule)</BackendHttpSettings>
                <Listener>(name-of-the-listener)</Listener>
                <BackendAddressPool>(name-of-your-backend-pool)</BackendAddressPool>
            </HttpLoadBalancingRule>
        </HttpLoadBalancingRules>
    </ApplicationGatewayConfiguration>

Redigeerige konfiguratsiooni üksuste sulgudes olevad väärtused. Salvestage fail laiendiga .xml.

>[AZURE.IMPORTANT] Http- või Https protokolli üksus on tõstutundlikud.

Järgmises näites kujutatakse häälestamine rakenduse lüüsi konfigureerimise faili abil. Näide laadi nõuded HTTP liikluse avaliku pordi 80 ja saadab võrguliiklust tagaandmebaas pordi 80 vahel kaks IP-aadressid.

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
        <FrontendPorts>
            <FrontendPort>
                <Name>FrontendPort1</Name>
                <Port>80</Port>
            </FrontendPort>
        </FrontendPorts>
        <BackendAddressPools>
            <BackendAddressPool>
                <Name>BackendPool1</Name>
                <IPAddresses>
                    <IPAddress>10.0.0.1</IPAddress>
                    <IPAddress>10.0.0.2</IPAddress>
                </IPAddresses>
            </BackendAddressPool>
        </BackendAddressPools>
        <BackendHttpSettingsList>
            <BackendHttpSettings>
                <Name>BackendSetting1</Name>
                <Port>80</Port>
                <Protocol>Http</Protocol>
                <CookieBasedAffinity>Enabled</CookieBasedAffinity>
            </BackendHttpSettings>
        </BackendHttpSettingsList>
        <HttpListeners>
            <HttpListener>
                <Name>HTTPListener1</Name>
                <FrontendPort>FrontendPort1</FrontendPort>
                <Protocol>Http</Protocol>
            </HttpListener>
        </HttpListeners>
        <HttpLoadBalancingRules>
            <HttpLoadBalancingRule>
                <Name>HttpLBRule1</Name>
                <Type>basic</Type>
                <BackendHttpSettings>BackendSetting1</BackendHttpSettings>
                <Listener>HTTPListener1</Listener>
                <BackendAddressPool>BackendPool1</BackendAddressPool>
            </HttpLoadBalancingRule>
        </HttpLoadBalancingRules>
    </ApplicationGatewayConfiguration>


### <a name="step-2"></a>Samm 2

Järgmiseks määrata rakenduse lüüsi. Cmdlet **Set-AzureApplicationGatewayConfig** kasutamine konfiguratsiooni XML-faili.


    Set-AzureApplicationGatewayConfig -Name AppGwTest -ConfigFile "D:\config.xml"

    VERBOSE: 7:54:59 PM - Begin Operation: Set-AzureApplicationGatewayConfig
    VERBOSE: 7:55:32 PM - Completed Operation: Set-AzureApplicationGatewayConfig
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   9b995a09-66fe-2944-8b67-9bb04fcccb9d

## <a name="configure-the-application-gateway-by-using-a-configuration-object"></a>Rakenduse lüüsi konfigureerimine konfiguratsiooni objekti abil

Järgmises näites on kujutatud kuidas konfigureerida rakenduse lüüsi konfigureerimine objektide abil. Kõigi konfiguratsiooni üksuste peab olema konfigureeritud eraldi ja seejärel lisatud objekti rakenduse lüüsi konfigureerimine. Pärast konfiguratsiooni objekti loomist saate käsu **Set-AzureApplicationGateway** varem loodud rakenduse lüüsi ressursi konfiguratsiooni kinnitamiseks.

>[AZURE.NOTE] Väärtuse määramisel iga konfiguratsiooni objekti, peate esmalt millise objekti PowerShelli kasutab Storage deklareerida. Esimese rea loomiseks üksikute üksuste määratleb, milliseid Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model (objekti nimi) kasutatakse.

### <a name="step-1"></a>Samm 1

Luua isiklikud kõik üksused.

Looge ees IP, nagu on näidatud järgmises näites.

    $fip = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendIPConfiguration
    $fip.Name = "fip1"
    $fip.Type = "Private"
    $fip.StaticIPAddress = "10.0.0.5"

Looge ees pordi, nagu on näidatud järgmises näites.

    $fep = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendPort
    $fep.Name = "fep1"
    $fep.Port = 80

Looge tagaandmebaas serveri pool.

 Määratleda IP-aadressid, mis lisatakse tagaandmebaas serveri pool, nagu on näidatud järgmises näites.


    $servers = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendServerCollection
    $servers.Add("10.0.0.1")
    $servers.Add("10.0.0.2")

 Kasutage $server objekti tagaandmebaas rakenduskausta objektile ($pool) väärtuste liitmiseks.

    $pool = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendAddressPool
    $pool.BackendServers = $servers
    $pool.Name = "pool1"

Looge tagaandmebaas serveri pool säte.

    $setting = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendHttpSettings
    $setting.Name = "setting1"
    $setting.CookieBasedAffinity = "enabled"
    $setting.Port = 80
    $setting.Protocol = "http"

Looge kuulajale.

    $listener = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpListener
    $listener.Name = "listener1"
    $listener.FrontendPort = "fep1"
    $listener.FrontendIP = "fip1"
    $listener.Protocol = "http"
    $listener.SslCert = ""

Reegli loomine.

    $rule = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpLoadBalancingRule
    $rule.Name = "rule1"
    $rule.Type = "basic"
    $rule.BackendHttpSettings = "setting1"
    $rule.Listener = "listener1"
    $rule.BackendAddressPool = "pool1"

### <a name="step-2"></a>Samm 2

Määrata kõigi isiklikud üksuste rakenduse lüüsi konfigureerimise objekti ($appgwconfig).

Lisage ees IP konfiguratsiooni.

    $appgwconfig = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.ApplicationGatewayConfiguration
    $appgwconfig.FrontendIPConfigurations = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendIPConfiguration]"
    $appgwconfig.FrontendIPConfigurations.Add($fip)

Konfiguratsiooni ees pordi lisada.

    $appgwconfig.FrontendPorts = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendPort]"
    $appgwconfig.FrontendPorts.Add($fep)

Konfiguratsiooni tagaandmebaas serveri kausta lisada.

    $appgwconfig.BackendAddressPools = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendAddressPool]"
    $appgwconfig.BackendAddressPools.Add($pool)

Saate lisada konfiguratsiooni tagaandmebaas rakenduskausta säte.

    $appgwconfig.BackendHttpSettingsList = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendHttpSettings]"
    $appgwconfig.BackendHttpSettingsList.Add($setting)

Lisage kuulajale konfiguratsiooni.

    $appgwconfig.HttpListeners = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpListener]"
    $appgwconfig.HttpListeners.Add($listener)

Reegli lisamine konfiguratsiooni.

    $appgwconfig.HttpLoadBalancingRules = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpLoadBalancingRule]"
    $appgwconfig.HttpLoadBalancingRules.Add($rule)

### <a name="step-3"></a>Samm 3

Kinnita konfiguratsiooni objekti rakenduse lüüsi ressursile **Set-AzureApplicationGatewayConfig**abil.

    Set-AzureApplicationGatewayConfig -Name AppGwTest -Config $appgwconfig

## <a name="start-the-gateway"></a>Lüüsi käivitamine

Kui lüüsi on konfigureeritud, **Algus-AzureApplicationGateway** cmdlet lüüsi alustamiseks kasutada. Rakenduste portaali arveldus algab pärast lüüsi on edukalt käivitatud.

> [AZURE.NOTE] **Algus-AzureApplicationGateway** cmdlet võib kuluda kuni 15-20 minutit lõpuni.

    Start-AzureApplicationGateway AppGwTest

    VERBOSE: 7:59:16 PM - Begin Operation: Start-AzureApplicationGateway
    VERBOSE: 8:05:52 PM - Completed Operation: Start-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   fc592db8-4c58-2c8e-9a1d-1c97880f0b9b

## <a name="verify-the-gateway-status"></a>Veenduge, et lüüsi olek

**Get-AzureApplicationGateway** cmdlet-käsu abil saate lüüsi olekut. Kui **Algus-AzureApplicationGateway** õnnestus eelmises etapis, *State* arvutisse olema installitud ja *Vip* ja *dnsnameWindows* peab olema lubatud kirjete.

Järgmises näites on kujutatud rakenduse lüüsi, mis on üles, töötab, ja olete valmis tegema liikluse mõeldud `http://<generated-dns-name>.cloudapp.net`.

    Get-AzureApplicationGateway AppGwTest

    VERBOSE: 8:09:28 PM - Begin Operation: Get-AzureApplicationGateway
    VERBOSE: 8:09:30 PM - Completed Operation: Get-AzureApplicationGateway
    Name          : AppGwTest
    Description   :
    VnetName      : testvnet1
    Subnets       : {Subnet-1}
    InstanceCount : 2
    GatewaySize   : Medium
    State         : Running
    Vip           : 138.91.170.26
    DnsName       : appgw-1b8402e8-3e0d-428d-b661-289c16c82101.cloudapp.net


## <a name="delete-an-application-gateway"></a>Rakenduste portaali kustutamine

Rakenduste portaali kustutamiseks tehke järgmist.

1. Lüüsi peatamine **Peata-AzureApplicationGateway** cmdlet-käsu abil.
2. **Eemalda – AzureApplicationGateway** cmdlet-käsu abil saate eemaldada lüüsi.
3. Veenduge, et lüüsi on eemaldatud **Get-AzureApplicationGateway** cmdleti abil.

Järgmises näites on kujutatud esimesele reale, millele järgneb väljund **Peata-AzureApplicationGateway** cmdlet-käsk.

    Stop-AzureApplicationGateway AppGwTest

    VERBOSE: 9:49:34 PM - Begin Operation: Stop-AzureApplicationGateway
    VERBOSE: 10:10:06 PM - Completed Operation: Stop-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   ce6c6c95-77b4-2118-9d65-e29defadffb8

Kui rakenduse lüüsi on peatatud olekus, **Eemalda-AzureApplicationGateway** cmdlet-käsu abil teenuse eemaldamine.


    Remove-AzureApplicationGateway AppGwTest

    VERBOSE: 10:49:34 PM - Begin Operation: Remove-AzureApplicationGateway
    VERBOSE: 10:50:36 PM - Completed Operation: Remove-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   055f3a96-8681-2094-a304-8d9a11ad8301

Veenduge, et teenus on eemaldatud, saate kasutada cmdlet-käsu **Get-AzureApplicationGateway** . Seda toimingut ei ole vaja.


    Get-AzureApplicationGateway AppGwTest

    VERBOSE: 10:52:46 PM - Begin Operation: Get-AzureApplicationGateway

    Get-AzureApplicationGateway : ResourceNotFound: The gateway does not exist.
    .....

## <a name="next-steps"></a>Järgmised sammud

Kui soovite konfigureerida SSL offload, lugege teemat [konfigureerimine rakenduste portaali SSL offload](application-gateway-ssl.md).

Kui soovite rakenduste portaali kasutamiseks on sisemine laadi koormusetasakaalustusteenuse konfigureerimine, lugege teemat [rakenduste abil on sisemine laadi koormusetasakaalustusteenuse (ILB) portaali loomine](application-gateway-ilb.md).

Kui soovite lisateavet laadimine tasakaalustavad suvandid üldiselt, lugege teemat:

- [Azure'i laadi koormusetasakaalustusteenuse](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure'i liikluse haldur](https://azure.microsoft.com/documentation/services/traffic-manager/)

[scenario]: ./media/application-gateway-create-gateway/scenario.png