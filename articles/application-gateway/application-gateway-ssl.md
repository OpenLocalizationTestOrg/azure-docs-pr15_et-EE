<properties
   pageTitle="Rakenduste portaali jaoks SSL-i offload konfigureerimine klassikaline juurutamise abil | Microsoft Azure'i"
   description="Selles artiklis antakse juhiseid, et luua rakenduste portaali SSL offload Azure klassikaline juurutamise mudeli abil."
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

# <a name="configure-an-application-gateway-for-ssl-offload-by-using-the-classic-deployment-model"></a>Klassikaline juurutamise mudeli abil rakenduste portaali jaoks SSL-i offload konfigureerimine

> [AZURE.SELECTOR]
-[Azure'i portaalis](application-gateway-ssl-portal.md)
-[Azure ressursihaldur PowerShelli](application-gateway-ssl-arm.md)
-[Azure klassikaline PowerShelli](application-gateway-ssl.md)

Azure'i rakenduse lüüsi saab konfigureerida Gateway vältimiseks kulukas SSL dekrüptimine tööülesandeid juhtuda web serveripargi turvasoklite kiht (SSL) seansi lõpetamiseks. SSL-i offload lihtsustab ka ees serveri häälestamine ja haldamine veebirakenduse.

## <a name="before-you-begin"></a>Enne alustamist

1. Installige uusim versioon Azure PowerShelli cmdletid Web platvormi Installeri abil. Saate alla laadida ja installida **Windows PowerShelli** jaotisest [allalaadimiste lehe](https://azure.microsoft.com/downloads/)uusima versiooni.
2. Veenduge, et teil on kehtiv alamvõrgu töötamise virtuaalse võrgu. Veenduge, et kasutate cloud kasutuselevõttu ega virtuaalmasinates alamvõrgu. Lüüsi rakendus peab olema ise virtuaalse alamvõrku.
3. Saate konfigureerida kasutama rakenduse lüüsi jaoks vajalike peab olemas või nende lõpp-punktid loonud kas virtuaalse võrgu või avaliku IP/VIP määratud.

Rakenduste portaali SSL offload konfigureerimiseks tehke järgmist loetletud järjestuses.

1. [Rakenduste portaali loomine](#create-an-application-gateway)
2. [SSL-sertide üleslaadimine](#upload-ssl-certificates)
3. [Lüüsi konfigureerimine](#configure-the-gateway)
4. [Määrake lüüsi konfigureerimine](#set-the-gateway-configuration)
5. [Lüüsi käivitamine](#start-the-gateway)
6. [Veenduge, et lüüsi olek](#verify-the-gateway-status)


## <a name="create-an-application-gateway"></a>Rakenduste portaali loomine

Lüüsi loomine cmdletiga **New-AzureApplicationGateway** , asendades väärtused ise. Arveldamine lüüsi ei käivitu sel hetkel. Arveldamine algab hiljem toimingut, kui lüüsi on edukalt.

    New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")

Kinnitada, et lüüsi loodud, saate kasutada cmdlet-käsu **Get-AzureApplicationGateway** .

Valimi, *Kirjeldus*ja *InstanceCount* *GatewaySize* on valikulised parameetrid. *InstanceCount* vaikeväärtus on 2 suurim väärtus on 10. *GatewaySize* vaikeväärtus on keskmine. Ja väikesed on ka muid saadaval väärtusi. *VirtualIPs* ja *dnsnameWindows* on kuvatud nii tühjaks, kuna lüüsi pole veel alanud. Need väärtused on loodud pärast lüüs on töökorras olekus.

    Get-AzureApplicationGateway AppGwTest

## <a name="upload-ssl-certificates"></a>SSL-sertide üleslaadimine

**Lisa-AzureApplicationGatewaySslCertificate** abil saate üles laadida rakenduse lüüsi serveri serdi *pfx* -vormingus. Serdi nimi on kasutaja valitud nime ja peab olema kordumatu rakenduse lüüsi sees. Selle serdi viidatakse selle serdi toimingute kõigi rakenduste lüüsi nime.

See Järgmine näidis kuvatakse cmdlet-käsk, asendage väärtused valimis oma.

    Add-AzureApplicationGatewaySslCertificate  -Name AppGwTest -CertificateName GWCert -Password <password> -CertificateFile <full path to pfx file>

Järgmiseks kinnitage serdi üleslaadimine. Kasutage cmdleti **Get-AzureApplicationGatewayCertificate** .

See näidis kuvatakse cmdlet esimese rea, millele järgneb väljund.

    Get-AzureApplicationGatewaySslCertificate AppGwTest

    VERBOSE: 5:07:54 PM - Begin Operation: Get-AzureApplicationGatewaySslCertificate
    VERBOSE: 5:07:55 PM - Completed Operation: Get-AzureApplicationGatewaySslCertificate
    Name           : SslCert
    SubjectName    : CN=gwcert.app.test.contoso.com
    Thumbprint     : AF5ADD77E160A01A6......EE48D1A
    ThumbprintAlgo : sha1RSA
    State..........: Provisioned

>[AZURE.NOTE] Serdi parooli peab olema 4 – 12 märki, tähest ja numbrist. Erimärkide sisestamine ei aktsepteerita.

## <a name="configure-the-gateway"></a>Lüüsi konfigureerimine

Lüüsi konfiguratsiooni koosneb mitu väärtust. Väärtused saate seotud koos, et koostada konfiguratsiooni.

Väärtused on:

- **Tagaandmebaas serveri pool:** Tagaandmebaas serverite IP-aadresside loend. IP-aadresside loetletud kuuluma kas virtuaalse alamvõrku või tuleks avaliku IP/VIP.
- **Tagaandmebaas serverisätete pool:** Igal pool on sätted, nt pordi, Protocol (protokoll) ja küpsis vastavalt osaleja. Need sätted on seotud on ja rakendatakse kõikides serverites maksimaalselt pool.
- **Ees port:** See port on avatud rakenduse lüüsi avaliku port. Liikluse tabab seda porti ja seejärel ümbersuunamist üks tagaandmebaas serverid.
- **Kuulajale:** Kuulajale on ees port, protokolli (Http- või Https need väärtused on tõstutundlik), ja SSL-i serdi nimi (kui offload SSL-i konfigureerimine).
- **Reegel:** Reegli seob kuulajale ja tagaandmebaas server pargis ja määratleb, milliseid tagaandmebaas serveri rakenduskausta liiklus tuleks suunata kui see tabab kindla kuulajale. Praegu ainult *tavaline* reegel on toetatud. *Lihtne* reegel on ringi-jaan jaotust.

**Täiendavate konfiguratsiooni märkmed**

SSL-sertide paigutus, **HttpListener** protokolli tuleks muuta *Https* (tõstutundlik). Elemendi **SslCert** lisatakse **HttpListener** väärtuseks kasutatakse eelmiste SSL-sertide jaotis üles sama nimi. Tuleks ajakohastada ees pordi 443 abil.

**Lubamiseks küpsise vastavalt osaleja**: rakenduste portaali saate konfigureerida nii, et tagada, et kliendi seansi taotluse on alati suunatud sama VM serveripargis veebi. Selle stsenaariumi tehakse süstena seansi küpsise, mis võimaldab lüüsi, mis suunaks liikluse õigesti. Küpsise vastavalt osaleja lubamiseks määrake **CookieBasedAffinity** *lubatud* **BackendHttpSettings** elementi.



Konfiguratsiooni objekti või konfiguratsiooni XML-faili abil saate koostada oma konfiguratsioon.
Ehitada konfiguratsioonist konfiguratsiooni XML-faili abil, kasutage järgmises näites:

**Konfiguratsiooni XML-i näidis**

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
        <FrontendIPConfigurations />
        <FrontendPorts>
            <FrontendPort>
                <Name>FrontendPort1</Name>
                <Port>443</Port>
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
                <Protocol>Https</Protocol>
                <SslCert>GWCert</SslCert>
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


## <a name="set-the-gateway-configuration"></a>Määrake lüüsi konfigureerimine

Järgmisena saate määrata rakenduse lüüsi. Kas konfiguratsiooni objekti või konfiguratsiooni XML-faili saate kasutada cmdlet-käsu **Set-AzureApplicationGatewayConfig** .

    Set-AzureApplicationGatewayConfig -Name AppGwTest -ConfigFile D:\config.xml

## <a name="start-the-gateway"></a>Lüüsi käivitamine

Kui lüüsi on konfigureeritud, **Algus-AzureApplicationGateway** cmdlet lüüsi alustamiseks kasutada. Rakenduste portaali arveldus algab pärast lüüs on edukalt käivitatud.


**Märkus:** **Algus-AzureApplicationGateway** cmdlet võib kuluda kuni 15-20 minutit lõpuni.

    Start-AzureApplicationGateway AppGwTest

## <a name="verify-the-gateway-status"></a>Veenduge, et lüüsi olek

**Get-AzureApplicationGateway** cmdlet-käsu abil saate lüüsi olekut. Kui **Algus-AzureApplicationGateway** õnnestus eelmises etapis, *State* arvutisse olema installitud ja *VirtualIPs* ja *dnsnameWindows* peab olema lubatud kirjete.

See näidis kuvatakse rakenduse lüüsi, mis on ajakohane, töötab, ja valmis tegema liikluse.

    Get-AzureApplicationGateway AppGwTest

    Name          : AppGwTest2
    Description   :
    VnetName      : testvnet1
    Subnets       : {Subnet-1}
    InstanceCount : 2
    GatewaySize   : Medium
    State         : Running
    VirtualIPs    : {23.96.22.241}
    DnsName       : appgw-4c960426-d1e6-4aae-8670-81fd7a519a43.cloudapp.net


## <a name="next-steps"></a>Järgmised sammud


Kui soovite lisateavet laadimine tasakaalustavad suvandid üldiselt, lugege teemat:

- [Azure'i laadi koormusetasakaalustusteenuse](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure'i liikluse haldur](https://azure.microsoft.com/documentation/services/traffic-manager/)