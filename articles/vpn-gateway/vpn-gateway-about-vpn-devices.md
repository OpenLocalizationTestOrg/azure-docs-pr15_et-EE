<properties 
   pageTitle="VPN seadmete-saidilt VPN-lüüsi ühenduste Azure virtuaalse võrkude kohta | Microsoft Azure'i"
   description="Selles artiklis käsitletakse VPN seadmete ja IPsec parameetrid S2S VPN-lüüsi ühendused ja sisaldab linke konfigureerimise juhised ja näited."
   services="vpn-gateway"
   documentationCenter="na"
   authors="yushwang"
   manager="rossort"
   editor=""
  tags="azure-resource-manager, azure-service-management"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/13/2016"
   ms.author="yushwang;cherylmc" />

# <a name="about-vpn-devices-for-site-to-site-vpn-gateway-connections"></a>VPN seadmed saitide VPN-lüüsi ühenduste kohta

VPN seade on vaja konfigureerida saidi saidi (S2S) VPN-ühendus. Saitide ühendused saab kasutada hübriid lahenduse loomiseks või kui soovite turvalist ühendust kohapealse võrgu ja virtuaalse võrgu vahel. Selles artiklis käsitletakse ühilduvate VPN ja konfiguratsiooni parameetrid. 

>[AZURE.NOTE] Saitide ühenduse konfigureerimisel on vaja seadme VPN avaliku IPv4 IP-aadress.                                                                                                                                                                               

Kui teie seade ei kuvata [kinnitatud VPN seadmete](#devicetable) tabelis, jaotisest [mitte kinnitatud VPN seadmete](#additionaldevices) käesoleva artikli. On võimalik, et teie seade võib töötavad endiselt Azure'i. VPN seadme tugi, võtke ühendust oma seadme tootjalt.

**Pange tähele, kui vaatate tabelite üksused:**

- Seal on muutunud terminoloogia staatiline ja dünaamiline marsruutimiseks. Tõenäoliselt tuleb tekib nii. Funktsioonid ei muutu, muutuvad ainult nimed.
    - Staatiline marsruutimine = PolicyBased
    - Dünaamiline marsruutimine = RouteBased
- Suure jõudlusega VPN-lüüsi ja RouteBased VPN-lüüsi samu märgitud teisiti. Näiteks valideeritud VPN seadmed, mis ühilduvad RouteBased VPN lüüside ühilduvad ka Azure suure jõudlusega VPN-lüüsi. 


## <a name="devicetable"></a>Kinnitatud VPN seadmed 

Meil on kinnitatud standard VPN seadmete koostöös kogum seadme hankijatega. Kõigis seadmetes seadme peredele, mis on esitatud allpool olevas loendis peaks töötama Azure'i VPN lüüsid. Lugege teemat [VPN lüüsi](vpn-gateway-about-vpngateways.md) kinnitamiseks tüüpi lüüsi, mille peate looma lahenduse, mida soovite konfigureerida. 

Selleks, et konfigureerida seadme VPN, viidata linke, mis vastavad pere soovitud seade. VPN seadme tugi, võtke ühendust oma seadme tootjalt.



| **Hankija**                      | **Seadme pere**                                        | **Minimaalne Opsüsteemi versioon**                             | **PolicyBased**                                                                                                                                                                                                             | **RouteBased**                                                                                                                                                                    |
|---------------------------------|----------------------------------------------------------|----------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Sellega seotud Telesis                  | AR sarja VPN ruuterid                                    | 2.9.2                                              | Tulen varsti                                                                                                                                                                                                                                          | Ei ühildu                                                                                                                                                                                               |
| Barracuda võrke, Inc kaubamärk.        | Barracuda NextGen tulemüüri F-sari             | PolicyBased: 5.4.3 RouteBased: 6.2.0  | [Konfigureerimise juhised](https://techlib.barracuda.com/NGF/AzurePolicyBasedVPNGW) | [Konfigureerimise juhised](https://techlib.barracuda.com/NGF/AzureRouteBasedVPNGW)                                                                                                                                                                                              |
| Barracuda võrke, Inc kaubamärk.        |  Barracuda NextGen tulemüüri X-sarja                 | Barracuda tulemüür 6.5 | [Barracuda tulemüüri](https://techlib.barracuda.com/BFW/ConfigAzureVPNGateway) | Ei ühildu                                                                                                                                                                                               |
| Brokaatkujundusega                         | Vyatta 5400 vRouter                                      | Virtuaalne ruuteri 6.6R3-GA                            | [Konfigureerimise juhised](http://www1.brocade.com/downloads/documents/html_product_manuals/vyatta/vyatta_5400_manual/wwhelp/wwhimpl/js/html/wwhelp.htm#href=VPN_Site-to-Site%20IPsec%20VPN/Preface.1.1.html)                                       | Ei ühildu                                                                                                                                                                                               |
| Märkige ruut punkti                     | Lüüsi turvalisus                                         | R75.40 R75.40VS                                     | [Konfigureerimise juhised](https://supportcenter.checkpoint.com/supportcenter/portal?eventSubmit_doGoviewsolutiondetails=&solutionid=sk101275)                                         | [Konfigureerimise juhised](https://supportcenter.checkpoint.com/supportcenter/portal?eventSubmit_doGoviewsolutiondetails=&solutionid=sk101275) |
| Cisco                           | ASA                                                      | 8.3                                                | [Cisco näidised](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ASA)                                                                                                                                                                        | Ei ühildu                                                                                                                                                                                               |
| Cisco                           | ASR                                                      | IOS-i 15.1 (PolicyBased) iOS-i 15.2 (RouteBased)                | [Cisco näidised](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ASR)                                                                                                                                                                        | [Cisco näidised](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ASR)                                                                                                                                 |
| Cisco                           | ISR                                                      | IOS-i 15,0 (PolicyBased) iOS-i 15.1 (RouteBased *)               | [Cisco näidised](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ISR)                                                                                                                                                                        | [Cisco näidised *](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ISR)                                                                                                                                |
| Citrix                          | NetScaler MPX SDX, VPX      |10,1 ja ülaltoodud                                           | [Juhised integreerimine](https://docs.citrix.com/en-us/netscaler/11-1/system/cloudbridge-connector-introduction/cloudbridge-connector-azure.html)                                                                                                                                                                            | Ei ühildu                                                                                                                                                                                               |
| Dell SonicWALL                  | TZ sarja kuuluv NSA sarja kuuluv wreaks laastavalt sarja, E-klassi NSA sarja | SonicOS 5.8.x, [SonicOS 5.9.x](http://documents.software.dell.com/sonicos/5.9/microsoft-azure-configuration-guide/supported-platforms?ParentProduct=850), [SonicOS 6.x](http://documents.software.dell.com/sonicos/6.2/microsoft-azure-configuration-guide/supported-platforms?ParentProduct=646 )          | [Juhised – SonicOS 6.2](http://documents.software.dell.com/sonicos/6.2/microsoft-azure-configuration-guide?ParentProduct=646) [Juhised – SonicOS 5,9](http://documents.software.dell.com/sonicos/5.9/microsoft-azure-configuration-guide?ParentProduct=850)                                                                                                                                   | [Juhised – SonicOS 6.2](http://documents.software.dell.com/sonicos/6.2/microsoft-azure-configuration-guide?ParentProduct=646) [Juhised – SonicOS 5,9](http://documents.software.dell.com/sonicos/5.9/microsoft-azure-configuration-guide?ParentProduct=850)                                                                                                                                                                                      |
| F5                              | Sarja suur-IP                                 |           12,0                                            | [Konfigureerimise juhised](https://devcentral.f5.com/articles/connecting-to-windows-azure-with-the-big-ip)                                                                                                                                                                          | [Konfigureerimise juhised](https://devcentral.f5.com/articles/big-ip-to-azure-dynamic-ipsec-tunneling)                                                                                                                                                                                         |
| Fortinet                        | FortiGate                                                | FortiOS 5.2.7                                      | [Konfigureerimise juhised](http://docs.fortinet.com/d/fortigate-configuring-ipsec-vpn-between-a-fortigate-and-microsoft-azure)                                                                                                                                                                          | [Konfigureerimise juhised](http://docs.fortinet.com/d/fortigate-configuring-ipsec-vpn-between-a-fortigate-and-microsoft-azure)                                                                                                                                  |
| Internet algatus Jaapan (IIJ) | SEIL sarja                                              | SEIL / 4.60, SEIL/B1 4.60, SEIL/x86 3.20 X            | [Konfigureerimise juhised](http://www.iij.ad.jp/biz/seil/ConfigAzureSEILVPN.pdf)                                                                                                                                                                   | Ei ühildu                                                                                                                                                                                               |
| Kadakas                         | SRX                                                      | JunOS 10.2 (PolicyBased) JunOS 11,4 (RouteBased)            | [Kadakas näidised](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SRX)                                                                                                                                                                      | [Kadakas näidised](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SRX)                                                                                                                              |
| Kadakas                         | J-sarja                                                 | JunOS 10.4r9 (PolicyBased) JunOS 11,4 (RouteBased)          | [Kadakas näidised](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/JSeries)                                                                                                                                                                 | [Kadakas näidised](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/JSeries)                                                                                                                         |
| Kadakas                         | ISG                                                      | ScreenOS 6.3 (PolicyBased ja RouteBased)                  | [Kadakas näidised](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/ISG)                                                                                                                                                                      | [Kadakas näidised](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/ISG)                                                                                                                              |
| Kadakas                         | SSG                                                      | ScreenOS 6.2 (PolicyBased ja RouteBased)                  | [Kadakas näidised](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SSG)                                                                                                                                                                      | [Kadakas näidised](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SSG)                                                                                                                              |
| Microsoft                       | Marsruutimine ja Remote Access Service                        | Windows Server 2012                                | Ei ühildu                                                                                                                                                                                                                                       | [Microsofti näidised](http://go.microsoft.com/fwlink/p/?LinkId=717761)                                                                                           |
| Avatud süsteemide AG | Lüüsi missiooni juhtelemendi turvalisus | N/A | [Installi juhend](https://www.open.ch/_pdf/Azure/AzureVPNSetup_Installation_Guide.pdf) | [Installi juhend](https://www.open.ch/_pdf/Azure/AzureVPNSetup_Installation_Guide.pdf) |
| Openswan                        | Openswan                                                 | 2.6.32                                             | (Tulekul)                                                                                                                                                                                                                                        | Ei ühildu                                                                                                                                                                                               |
| Palo alt võrkude              | Opsüsteemi üle kõigis seadmetes     | LIIKUMINE-OS 6.1.5 või uuem versioon (PolicyBased) üle-OS 7.0.5 või uuem versioon (RouteBased)       | [Konfigureerimise juhised]( https://live.paloaltonetworks.com/t5/Configuration-Articles/How-to-Configure-VPN-Tunnel-Between-a-Palo-Alto-Networks/ta-p/59065)                                                                                                                                                                                          | [Konfigureerimise juhised](https://live.paloaltonetworks.com/t5/Integration-Articles/Configuring-IKEv2-VPN-for-Microsoft-Azure-Environment/ta-p/60340)                                                                                                                                                                                    |
| WatchGuard                      | Kõik                                                      | Fireware XTM v11.x                                 | [Konfigureerimise juhised](http://customers.watchguard.com/articles/Article/Configure-a-VPN-connection-to-a-Windows-Azure-virtual-network/)                                                                                                                                                                          | Ei ühildu                                                                                                                                                                                               |

(*) ISR 7200 sarja ruuterid toetavad ainult PolicyBased VPN.

## <a name="additionaldevices"></a>Mitte-kinnitatud VPN seadmed

Kui te ei näe oma seadme kinnitatud VPN seadmete tabelis loetletud, see endiselt võib töötada ka saidilt ühendus. Veenduge, et seadme VPN vastab miinimumnõuetele lüüsi nõuded [VPN lüüside](vpn-gateway-about-vpngateways.md#gateway-requirements) artikli jaotises esitatud juhiseid. Seadmete miinimumnõuetele tuleks töötada ka ka VPN lüüsid. Täiendavate tugiteenuste ja konfiguratsiooni juhiste saamiseks pöörduge oma seadme tootjalt.


## <a name="editing-device-configuration-samples"></a>Seadme konfiguratsiooni näidised redigeerimine

Pärast antud VPN seadme konfiguratsiooni proovi allalaadimiseks peate mõne väärtused kajastamiseks keskkonna sätteid. 

**Valimi redigeerimiseks tehke järgmist.**

1. Avage valimi Notepad abil. 
1. Otsida ja asendada kõik <*teksti*> stringide väärtused, mis on seotud teie keskkonnas. Veenduge, et kaasata < ja >. Kui määratud on nimi, valige nimi peavad olema kordumatud. Kui käsk ei tööta, leiate oma seadme tootja dokumentatsioonist.

| **Näidistekst**                | **Muutmine**                                                                                                        |
|----------------------------------|----------------------------------------------------------------------------------------------------------------------|
| &lt;RP_OnPremisesNetwork&gt;           | Teie valitud sellele objektile nime. Näide: myOnPremisesNetwork                                                       |
| &lt;RP_AzureNetwork&gt;                | Teie valitud sellele objektile nime. Näide: myAzureNetwork                                                            |
| &lt;RP_AccessList&gt;                  | Teie valitud sellele objektile nime. Näide: myAzureAccessList                                                         |
| &lt;RP_IPSecTransformSet&gt;           | Teie valitud sellele objektile nime. Näide: myIPSecTransformSet                                                       |
| &lt;RP_IPSecCryptoMap&gt;              | Teie valitud sellele objektile nime. Näide: myIPSecCryptoMap                                                          |
| &lt;SP_AzureNetworkIpRange&gt;         | Määrake vahemik. Näide: 192.168.0.0                                                                                  |
| &lt;SP_AzureNetworkSubnetMask&gt;      | Määrake alamvõrgu mask. Näide: 255.255.0.0                                                                            |
| &lt;SP_OnPremisesNetworkIpRange&gt;    | Määrake kohapealse vahemik. Näide: 10.2.1.0                                                                         |
| &lt;SP_OnPremisesNetworkSubnetMask&gt; | Määrake kohapealse alamvõrgu mask. Näide: 255.255.255.0                                                              |
| &lt;SP_AzureGatewayIpAddress&gt;       | See teave virtuaalse võrgu ja asub haldusportaal nimega **Gateway IP-aadress**. |
| &lt;SP_PresharedKey&gt;                | See teave on seotud virtuaalse võrgu ja asub haldusportaal võti haldamine.          |



## <a name="ipsec-parameters"></a>IPsec parameetrid

>[AZURE.NOTE] Kuigi väärtused järgmises tabelis on loetletud toetab Azure VPN-lüüsi, praegu on mingil viisil saate määrata, või valige Azure VPN-lüüsi teatud kombinatsiooni. Määrake piirangutest kohapealse VPN seadme kaudu. Lisaks peavad vahelt MSS 1350 juures.

### <a name="ike-phase-1-setup"></a>IKE etapp 1 häälestamine

| **Atribuut**                                       | **PolicyBased** | **RouteBased ja Standard- või suure jõudlusega VPN lüüsi** |
|----------------------------------------------------|--------------------------------|------------------------------------------------------------------|
| IKE versioon                                        | IKEv1                          | IKEv2                                                            |
| Diffie-Hellmani rühm                               | Rühma 2 (1024 bitine)             | Rühma 2 (1024 bitine)                                               |
| Autentimismeetodi määramine                              | Eelnevalt jagatud võti                 | Eelnevalt jagatud võti                                                   |
| Krüptimise algoritmide kohta                              | AES256 AES128 3DES             | AES256 3DES                                                      |
| Loob rakendus algoritmi                                  | SHA1(SHA128)                   | SHA1(SHA128) SHA2(SHA256)                                                     |
| Etapp 1 turvalisus seos (SA) eluea (aeg) | finantseerimisasutustele 28 800 sekundit                 | 10 800 sekundit                                                   |


### <a name="ike-phase-2-setup"></a>IKE etapp 2 häälestamine

| **Atribuut**                                                             | **PolicyBased**                 | **RouteBased ja Standard- või suure jõudlusega VPN lüüsi**   |
|--------------------------------------------------------------------------|------------------------------------------------|--------------------------------------------------------------------|
| IKE versioon                                                              | IKEv1                                          | IKEv2                                                              |
| Loob rakendus algoritmi                                                        | SHA1(SHA128)                                   | SHA1(SHA128)                                                       |
| Etapp 2 turvalisus seos (SA) eluea (aeg)                        | 3 600 sekundit                                  | 3 600 sekundit                                                                  |
| Etapp 2 turvalisus seos (SA) eluea (läbilaskevõime)                  | 102,400,000 KB                                 | -                                                                  |
| IPsec SA krüptimise ja autentimise pakub (järjekorras eelistus) | 1. ESP AES256 2. ESP AES128 3. ESP 3DES 4. N/A | Vaadata *lüüsi RouteBased IPsec seose (SA) pakub* (vt allpool) |
| Täiuslik edasi hoidmise (PFS)                                            | Ei                                             | Pole (*)|
| Surnud omavahelistes automaattuvastus                                                      | Pole toetatud                                  | Toetatud                                                          |

(*) Azure'i lüüsi nimega IKE serveri aktsepteerida PFS DH rühma 1, 2, 5, 14, 24.

### <a name="routebased-gateway-ipsec-security-association-sa-offers"></a>Pakub RouteBased lüüsi IPsec seose (SA)

Järgmises tabelis on loetletud IPsec SA krüptimise ja autentimise pakub. Pakkumised on loetletud eelistused järjestust, et pakkumine on esitatud või aktsepteeritud.

| **IPsec SA krüptimise ja autentimise pakub** | **Azure'i lüüsi algataja**                               | **Azure'i lüüsi serveri nimega**                                   |
|---------------------------------------------------|--------------------------------------------------------------|--------------------------------------------------------------|
| 1                                                 | ESP AES_256 SHA                                              | ESP AES_128 SHA                                              |
| 2                                                 | ESP AES_128 SHA                                              | ESP 3_DES MD5                                                |
| 3                                                 | ESP 3_DES MD5                                                | ESP 3_DES SHA                                                |
| 4                                                 | ESP 3_DES SHA                                                | Siin SHA1 koos ESP AES_128 null HMAC-i abil                      |
| 5                                                 | Siin SHA1 koos ESP AES_256 null HMAC-i abil                      | Siin SHA1 koos ESP 3_DES null HMAC-i abil                        |
| 6                                                 | Siin SHA1 koos ESP AES_128 null HMAC-i abil                      | Siin MD5 koos ESP 3_DES null HMAC-i, kus pole eluajal kavandatud |
| 7                                                 | Siin SHA1 koos ESP 3_DES null HMAC-i abil                        | Siin SHA1 ESP 3_DES SHA1, pole eluajal abil                    |
| 8                                                 | Siin MD5 koos ESP 3_DES null HMAC-i, kus pole eluajal kavandatud | Siin MD5 ESP 3_DES MD5, pole eluajal abil                     |
| 9                                                 | Siin SHA1 ESP 3_DES SHA1, pole eluajal abil                    | ESP DES MD5                                                  |
| 10                                                | Siin MD5 ESP 3_DES MD5, pole eluajal abil                     | ESP DES SHA1, pole eluajal                                   |
| 11                                                | ESP DES MD5                                                  | Siin SHA1 ESP DES null HMAC-i, kus pole eluajal kavandatud        |
| 12                                                | ESP DES SHA1, pole eluajal                                   | Siin MD5 ESP DES null HMAC-i, kus pole eluajal kavandatud        |
| 13                                                | Siin SHA1 ESP DES null HMAC-i, kus pole eluajal kavandatud        | Siin SHA1 ESP DES SHA1, pole eluajal abil                      |
| 14                                                | Siin MD5 ESP DES null HMAC-i, kus pole eluajal kavandatud        | Siin MD5 ESP DES MD5, pole eluajal abil                       |
| 15                                                | Siin SHA1 ESP DES SHA1, pole eluajal abil                      | ESP SHA, pole eluajal                                        |
| 16                                                | Siin MD5 ESP DES MD5, pole eluajal abil                       | ESP MD5, pole eluajal                                        |
| 17                                                | -                                                            | Siin SHA, pole eluajal                                         |
| 18                                                | -                                                            | Siin MD5, pole eluajal                                        |


- RouteBased ja suure jõudlusega VPN lüüside abil saate määrata IPsec ESP NULL krüptimine. Null vastavalt krüptimise kaitse töödeldavate andmete ja kasutada ainult kui maksimaalne läbilaskevõime ja miinimum latentsus on nõutav.  Kliendid, võite kasutada seda VNet-VNet side stsenaariumid, või kui krüptimise rakendatakse mujal lahendus.

- Asutusesiseses Ühenduvus Interneti kaudu, kasutage Azure'i VPN-lüüsi vaikesätted krüptimise ja loob rakendus algoritmide kohta eespool tabelites on loetletud teie kriitilised teatise turvalisuse tagamiseks.





