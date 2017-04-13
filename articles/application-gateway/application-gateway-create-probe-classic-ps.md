<properties
   pageTitle="Luua kohandatud juures rakenduse lüüsi klassikaline juurutamise mudeli PowerShelli abil | Microsoft Azure'i"
   description="Siit saate teada, kuidas luua kohandatud juures rakenduse lüüsi klassikaline juurutamise mudeli PowerShelli abil"
   services="application-gateway"
   documentationCenter="na"
   authors="georgewallace"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags  
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="gwallace" />

# <a name="create-a-custom-probe-for-azure-application-gateway-classic-by-using-powershell"></a>Luua kohandatud juures Azure'i rakenduse lüüsi PowerShelli abil (klassikaline)

> [AZURE.SELECTOR]
- [Azure'i portaal](application-gateway-create-probe-portal.md)
- [Azure'i ressursihaldur PowerShelli](application-gateway-create-probe-ps.md)
- [Azure'i klassikaline PowerShelli](application-gateway-create-probe-classic-ps.md)

[AZURE.INCLUDE [azure-probe-intro-include](../../includes/application-gateway-create-probe-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Siit saate teada, kuidas [teha neid juhiseid kasutades ressursihaldur mudel](application-gateway-create-probe-ps.md).

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-an-application-gateway"></a>Rakenduste portaali loomine

Rakenduste portaali loomiseks tehke järgmist.

1. Lüüsi ressurssi luua.
2. Konfiguratsiooni XML-faili või konfiguratsiooni objekti loomine.
3. Äsja loodud rakenduse lüüsi ressursi konfiguratsiooni Kinnita.

### <a name="create-an-application-gateway-resource"></a>Lüüsi ressurssi loomine

Lüüsi loomine cmdletiga **New-AzureApplicationGateway** , asendades väärtused ise. Arveldamine lüüsi ei käivitu sel hetkel. Arveldamine algab hiljem toimingut, kui lüüsi on edukalt.

Järgmises näites luuakse rakenduste portaali abil virtuaalse võrgust, mida nimetatakse "testvnet1" ja alamvõrku, mis on nimega "alamvõrgu-1".

    New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")

Kinnitamiseks, et lüüsi on loodud, saate kasutada cmdlet-käsu **Get-AzureApplicationGateway** .

    Get-AzureApplicationGateway AppGwTest

>[AZURE.NOTE]  *InstanceCount* vaikeväärtus on 2 suurim väärtus on 10. *GatewaySize* vaikeväärtus on keskmine. Saate valida väike, Keskmine ja suur.

 *VirtualIPs* ja *dnsnameWindows* on kuvatud nii tühi, kuna lüüsi pole veel alanud. Need on loodud pärast lüüs on töökorras olekus.

## <a name="configure-an-application-gateway"></a>Rakenduste portaali konfigureerimine

XML-i või konfiguratsiooni objekti abil saate konfigureerida rakendus lüüsi.

## <a name="configure-an-application-gateway-by-using-xml"></a>XML-i abil rakenduste portaali konfigureerimine

Järgmises näites saate XML-faili kõigi rakenduste lüüsi sätete konfigureerimine ja kinnitage need lüüsi ressurssi.  

### <a name="step-1"></a>Samm 1

Kopeerige järgmine tekst Notepadi.

    <ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
    <FrontendIPConfigurations>
        <FrontendIPConfiguration>
            <Name>fip1</Name>
            <Type>Private</Type>
        </FrontendIPConfiguration>
    </FrontendIPConfigurations>    
    <FrontendPorts>
        <FrontendPort>
            <Name>port1</Name>
            <Port>80</Port>
        </FrontendPort>
    </FrontendPorts>
    <Probes>
        <Probe>
            <Name>Probe01</Name>
            <Protocol>Http</Protocol>
            <Host>contoso.com</Host>
            <Path>/path/custompath.htm</Path>
            <Interval>15</Interval>
            <Timeout>15</Timeout>
            <UnhealthyThreshold>5</UnhealthyThreshold>
        </Probe>
      </Probes>
     <BackendAddressPools>
        <BackendAddressPool>
            <Name>pool1</Name>
            <IPAddresses>
                <IPAddress>1.1.1.1</IPAddress>
                <IPAddress>2.2.2.2</IPAddress>
            </IPAddresses>
        </BackendAddressPool>
    </BackendAddressPools>
    <BackendHttpSettingsList>
        <BackendHttpSettings>
            <Name>setting1</Name>
            <Port>80</Port>
            <Protocol>Http</Protocol>
            <CookieBasedAffinity>Enabled</CookieBasedAffinity>
            <RequestTimeout>120</RequestTimeout>
            <Probe>Probe01</Probe>
        </BackendHttpSettings>
    </BackendHttpSettingsList>
    <HttpListeners>
        <HttpListener>
            <Name>listener1</Name>
            <FrontendIP>fip1</FrontendIP>
        <FrontendPort>port1</FrontendPort>
            <Protocol>Http</Protocol>
        </HttpListener>
    </HttpListeners>
    <HttpLoadBalancingRules>
        <HttpLoadBalancingRule>
            <Name>lbrule1</Name>
            <Type>basic</Type>
            <BackendHttpSettings>setting1</BackendHttpSettings>
            <Listener>listener1</Listener>
            <BackendAddressPool>pool1</BackendAddressPool>
        </HttpLoadBalancingRule>
    </HttpLoadBalancingRules>
    </ApplicationGatewayConfiguration>


Redigeerige konfiguratsiooni üksuste sulgudes olevad väärtused. Salvestage fail laiendiga .xml.

Järgmises näites kujutatakse konfiguratsioonifail abil saate häälestada rakenduse lüüsi, laadimise saldo HTTP liikluse avaliku pordi 80 ja saata tagaandmebaas pordi 80 vahel kaks IP-aadressid, kasutades kohandatud juures võrguliiklust.

>[AZURE.IMPORTANT] Http- või Https protokolli üksus on tõstutundlikud.

Uue üksuse konfiguratsiooni \<Probe\> lisatakse konfigureerida kohandatud sondid.

Konfiguratsiooni parameetrid on:

- **Nimi** – viide nimi kohandatud juures.
- **Protokoll** - protokoll, mida kasutatakse (võimalikud väärtused on HTTP- või HTTPS).
- **Host** ja **tee** - täielikku URL-i tee, mis kasutavad rakenduse lüüsi eksemplari seisundi määratlemiseks. Näiteks kui teil on veebisaidi http://contoso.com/, siis kohandatud juures saate konfigureerida "http://contoso.com/path/custompath.htm" on eduka HTTP vastuse juures kontrollimiseks.
- **Intervall** – konfigureerib juures intervall kontrolle sekundites.
- **Ajalõpp** - määratleb juures ajalõpp HTTP vastuse kontroll.
- **UnhealthyThreshold** - vaja lipu tagaandmebaas eksemplari nimega nurjunud HTTP vastuste arvu *vigane*.

Viidatakse nime juures olevat <BackendHttpSettings> konfiguratsiooni määrata, millised tagaandmebaas rakenduskausta kasutab kohandatud juures sätted.

## <a name="add-a-custom-probe-configuration-to-an-existing-application-gateway"></a>Olemasoleva rakenduste portaali lisamine kohandatud juures konfigureerimine

Rakenduste portaali praeguse konfiguratsiooni muutmise nõuab kolm toimingut: saada praegune XML-konfiguratsioonifail, on kohandatud juures muuta ja rakenduse lüüsi ja uue XML-i sätete konfigureerimine.

### <a name="step-1"></a>Samm 1

Saada XML-faili abil get-AzureApplicationGatewayConfig. See ekspordib konfiguratsiooni XML-i lisada juures sätet muuta.

    Get-AzureApplicationGatewayConfig -Name "<application gateway name>" -Exporttofile "<path to file>"


### <a name="step-2"></a>Samm 2

Avage tekstiredaktoris XML-fail. Lisage soovitud `<probe>` jaotise pärast `<frontendport>`.

    <Probes>
        <Probe>
            <Name>Probe01</Name>
            <Protocol>Http</Protocol>
            <Host>contoso.com</Host>
            <Path>/path/custompath.htm</Path>
            <Interval>15</Interval>
            <Timeout>15</Timeout>
            <UnhealthyThreshold>5</UnhealthyThreshold>
        </Probe>
    </Probes>

Lisage XML-i jaotises backendHttpSettings juures nimi, nagu on näidatud järgmises näites:

        <BackendHttpSettings>
            <Name>setting1</Name>
            <Port>80</Port>
            <Protocol>Http</Protocol>
            <CookieBasedAffinity>Enabled</CookieBasedAffinity>
            <RequestTimeout>120</RequestTimeout>
            <Probe>Probe01</Probe>
        </BackendHttpSettings>

XML-faili salvestada.

### <a name="step-3"></a>Samm 3

**Set-AzureApplicationGatewayConfig**abil värskendada rakenduse lüüsi konfigureerimise uut XML-faili abil. See värskendab teie rakenduse lüüsi uus konfiguratsioon.

    Set-AzureApplicationGatewayConfig -Name "<application gateway name>" -Configfile "<path to file>"


## <a name="next-steps"></a>Järgmised sammud

Kui soovite konfigureerida offload turvasoklite kiht (SSL), lugege teemat [konfigureerimine rakenduste portaali SSL offload](application-gateway-ssl.md).

Kui soovite rakenduste portaali kasutamiseks on sisemine laadi koormusetasakaalustusteenuse konfigureerimine, lugege teemat [rakenduste abil on sisemine laadi koormusetasakaalustusteenuse (ILB) portaali loomine](application-gateway-ilb.md).
