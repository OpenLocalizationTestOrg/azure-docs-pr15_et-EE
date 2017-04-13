<properties 
   pageTitle="Loomine ja konfigureerimine rakenduste portaali sisemise koormuse koormusetasakaalustusteenuse (ILB) virtuaalse võrgus | Microsoft Azure'i"
   description="Sellelt lehelt leiate juhiseid, et konfigureerida Azure rakenduste portaali sisemise koormusetasakaalustusega lõpp"
   documentationCenter="na"
   services="application-gateway"
   authors="georgewallace"
   manager="jdial"
   editor="tysonn"/>
<tags 
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services" 
   ms.date="08/19/2016"
   ms.author="gwallace"/>

# <a name="create-an-application-gateway-with-an-internal-load-balancer-ilb"></a>Sisemise koormusetasakaalustus (ILB) rakenduste portaali loomine

> [AZURE.SELECTOR]
- [Azure'i klassikaline järgmiselt.](application-gateway-ilb.md)
- [Ressursihaldur PowerShelli järgmiselt.](application-gateway-ilb-arm.md)

Rakenduse lüüsi saab konfigureerida Interneti vastastikuste virtuaalse IP- või on sisemine lõpp-punkti ei kasuta Interneti-ühendus, nimetatakse ka sisemise laadi koormusetasakaalustusteenuse (ILB) lõpp-punkti. Lüüsi konfigureerimise abil soovitud ILB on kasulik sisemise ärivaldkonna rakenduste Interneti avatud. See on abiks teenuste/astme mitmekihilise rakenduses, mis asub turvalisus äärist, mis ei kasuta Interneti, kuid ikka nõua round jaan jaotust, seansi kleepunud või SSL-i lõpetamine. Selles artiklis tutvustatakse rakenduste portaali ja mõne ILB konfigureerimine juhiseid.

## <a name="before-you-begin"></a>Enne alustamist

1. Installige Azure'i PowerShelli cmdlet-käskude kasutamine Web platvormi Installeri uusim versioon. Saate alla laadida ja installida **Windows PowerShelli** jaotises [lehe alla laadida](https://azure.microsoft.com/downloads/)uusim versioon.
2. Veenduge, et teil on kehtiv alamvõrgu töötamise virtuaalse võrgu.
3. Veenduge, et taustväärtus serverid on virtuaalse võrgu või koos avaliku IP/VIP määratud.


Rakenduste portaali loomiseks tegema järgmised toimingud toodud järjestuses. 

1. [Rakenduste portaali loomine](#create-a-new-application-gateway)
2. [Lüüsi konfigureerimine](#configure-the-gateway)
3. [Määrake lüüsi konfigureerimine](#set-the-gateway-configuration)
4. [Lüüsi käivitamine](#start-the-gateway)
4. [Veenduge, et lüüsi](#verify-the-gateway-status)



## <a name="create-an-application-gateway"></a>Rakenduste portaali loomiseks tehke järgmist.

**Lüüsi loomine**, kasutage funktsiooni `New-AzureApplicationGateway` cmdlet-käsk, asendades väärtused ise. Pange tähele, et arveldamine lüüsi ei käivitu sel hetkel. Arveldamine algab hiljem toimingut, kui lüüsi on edukalt.

    PS C:\> New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")

    VERBOSE: 4:31:35 PM - Begin Operation: New-AzureApplicationGateway 
    VERBOSE: 4:32:37 PM - Completed Operation: New-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error 
    ----       ----------------     ------------                             ----
    Successful OK                   55ef0460-825d-2981-ad20-b9a8af41b399

**Kinnitamiseks** , et lüüsi on loodud, saate kasutada funktsiooni `Get-AzureApplicationGateway` cmdlet-käsk. 

Valimi, *Kirjeldus*, *InstanceCount*ja *GatewaySize* on valikulised parameetrid. *InstanceCount* vaikeväärtus on 2 suurim väärtus on 10. *GatewaySize* vaikeväärtus on keskmine. Ja väikesed on ka muid saadaval väärtusi. *VIP* ja *dnsnameWindows* on kuvatud nii tühi, kuna lüüsi pole veel alanud. Need on loodud pärast lüüs on töökorras olekus. 

    PS C:\> Get-AzureApplicationGateway AppGwTest

    VERBOSE: 4:39:39 PM - Begin Operation:
    Get-AzureApplicationGateway VERBOSE: 4:39:40 PM - Completed 
    Operation: Get-AzureApplicationGateway
    Name: AppGwTest 
    Description: 
    VnetName: testvnet1 
    Subnets: {Subnet-1} 
    InstanceCount: 2 
    GatewaySize: Medium 
    State: Stopped 
    VirtualIPs: 
    DnsName:


## <a name="configure-the-gateway"></a>Lüüsi konfigureerimine

Lüüsi konfiguratsiooni koosneb mitu väärtust. Väärtused saate seotud koos, et koostada konfiguratsiooni.
 
Väärtused on:

- **Taustväärtus serveri pool:** Taustväärtus serverite IP-aadresside loend. IP-aadresside loendis kas kuuluvuse VNet alamvõrgu või peaks olema avaliku IP/VIP. 
- **Taustväärtus pool serverisätete:** Igal pool on sätted, nt portide, Protocol (protokoll) ja küpsise vastavalt osaleja. Need sätted on seotud on ja rakendatakse kõikides serverites maksimaalselt pool.
- **Frontend Port:** See port on avada rakendus lüüsi avaliku port. Liikluse tabab seda porti ja seejärel ümbersuunamist ühele kirjutamata serverid.
- **Kuulajale:** Kuulajale on frontend port, protokolli (Http- või Https need tõstutundlik), ja SSL-i serdi nimi (kui offload SSL-i konfigureerimine). 
- **Reegel:** Reegli seob kuulajale ja kirjutamata server pargis ja määratleb, milliseid taustväärtus serveri rakenduskausta liiklus tuleks suunata kui see tabab kindla kuulajale. Praegu on toetatud ainult *tavaline* reegel. *Tavaline* reegel on ringi-jaan jaotust.

Konfiguratsiooni objekti loomisega või konfiguratsiooni XML-faili abil saate koostada oma konfiguratsioon. Ehitada konfiguratsioonist konfiguratsiooni XML-faili abil, kasutage alltoodud valimi.



Võtke arvesse järgmist.


- *FrontendIPConfigurations* elemendi kirjeldatakse asjakohaste rakenduste lüüsi konfigureerida koos mõne ILB ILB üksikasjad. 

- Era-"seadma Frontend IP *Tüüp*

- *StaticIPAddress* peaks olema seatud soovitud sisemine IP, millel saab lüüsi liiklust. Pange tähele, et *StaticIPAddress* element on valikuline. Kui ei määra, Saadaval sisemise IP alamvõrgu juurutatud on valitud. 

- Elemendi *nime* *FrontendIPConfiguration* määratud väärtusest tuleks kasutada funktsiooni HTTPListener *FrontendIP* element on FrontendIPConfiguration viidata.

 **Konfiguratsiooni XML-i näidis**

 

        <?xml version="1.0" encoding="utf-8"?>
        <ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
            <FrontendIPConfigurations>
                <FrontendIPConfiguration>
                    <Name>fip1</Name> 
                    <Type>Private</Type> 
                    <StaticIPAddress>10.0.0.10</StaticIPAddress> 
                </FrontendIPConfiguration>
            </FrontendIPConfigurations>
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
                    <FrontendIP>fip1</FrontendIP>
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
    


## <a name="set-the-gateway-configuration"></a>Määrake lüüsi konfigureerimine

Järgmiseks tuleb määrata rakenduse lüüsi. Saate kasutada funktsiooni `Set-AzureApplicationGatewayConfig` cmdlet-käsu konfigureerimine objekti või konfiguratsiooni XML-faili. 

    PS C:\> Set-AzureApplicationGatewayConfig -Name AppGwTest -ConfigFile D:\config.xml

    VERBOSE: 7:54:59 PM - Begin Operation: Set-AzureApplicationGatewayConfig 
    VERBOSE: 7:55:32 PM - Completed Operation: Set-AzureApplicationGatewayConfig
    Name       HTTP Status Code     Operation ID                             Error 
    ----       ----------------     ------------                             ----
    Successful OK                   9b995a09-66fe-2944-8b67-9bb04fcccb9d

## <a name="start-the-gateway"></a>Lüüsi käivitamine

Kui lüüsi on konfigureeritud, kasutage funktsiooni `Start-AzureApplicationGateway` cmdlet-käsu käivitamine lüüsi. Rakenduste portaali arveldus algab pärast lüüsi on edukalt käivitatud. 


> [AZURE.NOTE] Funktsiooni `Start-AzureApplicationGateway` cmdlet võib kuluda kuni 15-20 minutit. 
   
    PS C:\> Start-AzureApplicationGateway AppGwTest 

    VERBOSE: 7:59:16 PM - Begin Operation: Start-AzureApplicationGateway 
    VERBOSE: 8:05:52 PM - Completed Operation: Start-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error 
    ----       ----------------     ------------                             ----
    Successful OK                   fc592db8-4c58-2c8e-9a1d-1c97880f0b9b

## <a name="verify-the-gateway-status"></a>Veenduge, et lüüsi olek

Kasutage funktsiooni `Get-AzureApplicationGateway` cmdlet-käsu lüüsi olekut. Kui *Algus-AzureApplicationGateway* õnnestus eelmises etapis, olek peaks olema *töötab*ja Vip ja dnsnameWindows peab olema lubatud kirjete. See näidis kuvatakse cmdlet esimese rea, millele järgneb väljund. Selles näites lüüsi töötab ja on valmis tegema liikluse. 

> [AZURE.NOTE] Lüüsi rakendus on konfigureeritud aktsepteerima liikluse konfigureeritud ILB lõpp-punktile 10.0.0.10 selles näites.

    PS C:\> Get-AzureApplicationGateway AppGwTest 

    VERBOSE: 8:09:28 PM - Begin Operation: Get-AzureApplicationGateway 
    VERBOSE: 8:09:30 PM - Completed Operation: Get-AzureApplicationGateway
    Name          : AppGwTest
    Description   : 
    VnetName      : testvnet1
    Subnets       : {Subnet-1}
    InstanceCount : 2
    GatewaySize   : Medium
    State         : Running
    VirtualIPs    : {10.0.0.10}
    DnsName       : appgw-b2a11563-2b3a-4172-a4aa-226ee4c23eed.cloudapp.net

## <a name="next-steps"></a>Järgmised sammud


Kui soovite lisateavet laadimine tasakaalustavad suvandid üldiselt, lugege teemat:

- [Azure'i laadi koormusetasakaalustusteenuse](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure'i liikluse haldur](https://azure.microsoft.com/documentation/services/traffic-manager/)
