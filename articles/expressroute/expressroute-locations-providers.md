<properties
   pageTitle="ExpressRoute asukohad | Microsoft Azure'i"
   description="Selles artiklis antakse üksikasjaliku ülevaate asukohad, kus teenuseid pakutakse ja Azure piirkondade ühendamiseks."
   services="expressroute"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor="" />
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/21/2016"
   ms.author="cherylmc" />

# <a name="expressroute-partners-and-peering-locations"></a>ExpressRoute partnerid ja silmitsemine asukohad

Selles artiklis tabelite teavet ExpressRoute ühenduvuse teenusepakkujate ExpressRoute geograafiliselt, Microsofti pilveteenustega toetatud üle ExpressRoute ja ExpressRoute süsteemiintegraatorid (SIs).

## <a name="partners"></a>ExpressRoute ühenduvuse pakkujad

ExpressRoute toetatakse kogu Azure piirkondade ja asukohad. Järgmised kaardi leiate Azure'i piirkondade ja ExpressRoute asukohad loendi. ExpressRoute asukohad viidata need, kus Microsoft eakaaslaste mitu teenusepakkujatele.

![Asukoha kaart][0]

On teil juurdepääs Azure'i teenustele kõigis piirkondades geopoliitiliste piirkonna kui vähemalt üks ExpressRoute asukoht geopoliitiliste piirkonnas ühendatud. Järgmisest tabelist leiate Azure'i piirkondade ExpressRoute asukohtadele geopoliitiline piirkond kaardil.

|**Geopoliitiline piirkond**|**Azure'i piirkondade**|**ExpressRoute asukohad**|
|---|---|---|
|**Põhja-Ameerika**|Ida-USA, Lääne USA, Ida-USA 2, Kesk-USA, Lõuna-, Kesk-USA, Põhja Kesk-USA, Kanada Central, Kanada Ida|Atlanta, Chicago, Dallase, Las Vegas, Los Angeles, New Yorgis, Seattle'i, Edenvale, Washington AV, Montreali +, Quebecis linn +, Toronto|
|**Lõuna-Ameerika**|Brasiilia Lõuna|Irvine|
|**Euroopa**|Põhja-Euroopa Lääne Euroopa, Suurbritannia Lääne, Suurbritannia Lõuna|Amsterdam, Dublini, Londoni, Newport(Wales) +, Pariis|
|**Asia**|Ida-Aasia, Kagu-Aasia|Hong Kong, Singapur|
|**Jaapan**|Jaapan Lääne, Jaapani Ida|Osaka, Tokyo|
|**Austraalia**|Austraalia kodutee Austraalia Ida|Melbourne, Sydney|
|**India**|India Lääne India Central, India, Lõuna|Chennai, Mumbai|



Järgmine tabel pakub teavet piirkondade ja geopoliitiliste piirmäärad liikmesriikide pilved.

|**Geopoliitiline piirkond**|**Azure'i piirkondade**|**ExpressRoute asukohad**|
|---|---|---|---|
|**USA valitsuse pilveteenuses**|USA valitsuse Iowa, USA gov – Virginia|Chicago, Dallase, New York, Washington AV|
|**Hiina**|Hiina Põhja, Hiina Ida|Peking, Shanghai|
|**Saksamaa**|Saksamaa Central, Saksamaa Ida|Berliini Frankfurt|


Ühenduvus geopoliitiliste piirkondade ei toeta standard ExpressRoute SKU-ga. Peate ExpressRoute premium lisandmooduli toetamiseks globaalse ühenduvuse lubamine. Ühenduvus liikmesriikide cloud keskkonnas ei toetata. Kui selline vajaduse korral saate töötada pakkuja Ühenduvus.


## <a name="connectivity-provider-locations"></a>Ühenduvus pakkuja asukohad

> [AZURE.SELECTOR]
[Asukohtade pakkuja](expressroute-locations.md#connectivity-provider-locations)
[pakkujate asukoha järgi](expressroute-locations-providers.md#connectivity-provider-locations)

### <a name="production-azure"></a>Tootmise Azure
| **Asukoht**  | **Teenuse pakkujad** |
|---------------|-----------------------|
| **Amsterdam** | AT & T NetBond, British Telecom, Colt, Equinix, euNetworks, Aryaka võrkude GÉANT, InterCloud, Interneti lahendused – pilveteenuses ühendust, Interxion, tase 3 suhtlus, oranž, Tata suhtlus, TeleCity jaotises Telenor, Verizon |
| **Atlanta** | Equinix |
| **Chennai** | Tata suhtlus |
| **Chicago** | AT & T NetBond, Comcast, Equinix, tase 3 suhtlus, Zayo rühma |
| **Dallase** | AT & T NetBond, Cologix, Equinix, tase 3 suhtlus Megaport |
| **Dublini** | Colt, Telecity rühma |
| **Hongkong** | British Telecom, Hiina Telecom globaalne Equinix, Megaport, oranž, PCCW globaalne piiratud, Tata suhtlus Verizon |
| **Londoni** | AT & T NetBond, British Telecom, Colt, Equinix, InterCloud, lahendused Interneti - ühenduse pilves, Interxion Jisc, tase 3 suhtlus, MTN, NTT suhtlus, oranž, Tata suhtlus, Telecity jaotises Telenor Verizon, Vodafone |
| **Las Vegas** | Tase 3 suhtlus +, Megaport
| **Narva** | CoreSite, Equinix, Megaport, NTT, Zayo Group |
| **Melbourne** | AARNet, Equinix, Megaport, NEXTDC, Telstra Corporation |
| **New York** | Equinix, Megaport, Zayo Group |
| **Newport(Wales) +** | Järgmise genereerimine andmete + |
| **Montreal** | Cologix + |
| **Mumbai** | Tata suhtlus |
| **Osaka** | Equinix, Interneti algatus Jaapan Inc - IIJ, NTT suhtlus, Softbank |
| **Pariis** | Interxion Equinix + |
| **Irvine** | Equinix Telefonica |
| **Seattle** | Equinix, tase 3 suhtlus Megaport |
| **Edenvale** | Aryaka võrke, AT & T NetBond British Telecom, CenturyLink +, Comcast, Equinix, tase 3 suhtlus, oranž, Tata suhtlus, Verizon, Zayo Group |
| **Singapur** | Aryaka võrke, AT & T NetBond, British Telecom, Equinix, InterCloud, Megaport, oranž, SingTel, Tata suhtlus, Verizon |
| **Sydney** | AARNet, AT & T NetBond, British Telecom, Equinix, Megaport, NEXTDC, oranž, Telstra Corporation, Verizon |
| **Tokyo** | Aryaka võrke, British Telecom, Colt, Equinix, Interneti algatus Jaapan Inc - IIJ, NTT suhtlus, Softbank, Verizon |
| **Toronto** | Cologix, Equinix, Zayo Group |
| **Washingtoni AV** | Aryaka võrke, AT & T NetBond, British Telecom, Comcast, Equinix, InterCloud, tase 3 suhtlus, Megaport, oranž, Tata suhtlus, Verizon, Zayo Group |

 **+**tähistab tulekul

### <a name="national-cloud-environments"></a>Riigi cloud keskkonnas

#### <a name="us-government-cloud"></a>USA valitsuse pilveteenuses

| **Asukoht**  |**Teenuse pakkujad** |
|---------------|--------------------|
| **Chicago** | AT & T NetBond Equinix, tase 3 suhtlus Verizon |
| **Dallase** |  Equinix, Verizon + |
| **New York** | Equinix, tase 3 suhtlus + Verizon |
| **Washingtoni AV** | AT & T NetBond Equinix, tase 3 suhtlus Verizon |

#### <a name="china"></a>Hiina

| **Asukoht**  | **Teenuse pakkujad** |
|---------------|-----------------------|
| **Beijing** | Hiina Telecom |
| **Shanghai** |  Hiina Telecom |
Lisateavet leiate teemast [ExpressRoute Hiinas](http://www.windowsazure.cn/home/features/expressroute/)

#### <a name="germany"></a>Saksamaa

| **Asukoht**  | **Teenuse pakkujad** |
|---------------|-----------------------|
| **Berliini** | Colt + e-varju |
| **Frankfurt** | Colt, Equinix, Interxion |

## <a name="nonpartners"></a>Ühenduvus pole loendis teenusepakkujatele kaudu

Kui eelmiste jaotiste pakkuja Ühenduvus pole loendis, saate ikkagi ühenduse luua.

- Märkige ruut kuvamiseks, kui need on ühendatud vahetada ülaltoodud tabelis mis tahes ühenduvuse teenusepakkuja juures. Saate koguda pakutavaid Exchange'i pakkujate kohta lisateabe saamiseks järgmisi linke. Mitme ühenduvuse pakkujate ühendatud Ethernet vahetamine.

    - [Exchange'i Equinix pilveteenuses](http://www.equinix.com/services/interconnection-connectivity/cloud-exchange/)
    - [TeleCity CloudIX](http://www.telecitygroup.com/colocation-services/cloud-ix.htm)
    - [InterXion](http://www.interxion.com/)
    - [NextDC](http://www.nextdc.com/)
    - [CoreSite](http://www.coresite.com/)
    - [Cologix](http://www.cologix.com/)
- On laiendamine silmitsemine asukoha valik võrgu Ühenduvus pakkuja.
    - Veenduge, et pakkuja Ühenduvus laiendab oma ühenduvuse väga kättesaadav viisil nii, et on pole ühe punkti tõrgete.
- Tellige mõne ExpressRoute ringi vahetamist nimega Microsoft ühenduse pakkuja Ühenduvus.
    - Järgige juhiseid [loomine mõne ExpressRoute ringi](expressroute-howto-circuit-classic.md) häälestamiseks Ühenduvus.

|**Asukoht**|**Exchange'i**|**Ühenduvus pakkujad**|
|-------------|------------|-------------------------|
| **New York** | Equinix | Lightower |
| **Seattle** | Equinix | Alaskat suhtlus |
| **Edenvale** | Equinix | Lucent |
| **Singapur** | Equinix | 1CLOUDSTAR |
| **Washingtoni AV** | Equinix | Lightower |

## <a name="expressroute-system-integrators"></a>ExpressRoute süsteemiintegraatorid

Lubada privaatne ühenduvuse vastaks paremini teie vajadustele võib olla keeruline, põhineb võrgu skaala. Saate töötada mis tahes süsteemiintegraatorid, aidata teil teenusekomplekti, et ExpressRoute järgmises tabelis loetletud.

|**Continent**|**Süsteemiintegraatorid**|
|-------------|---------------------|
| **Asia** | Avanade Inc, OneAs1a|
| **Euroopa** | Avanade Inc Dotnet lahendused|
| **MEILE** | Avanade Inc, Equinix professionaalsete teenuste, Perficient projekti juhtimine|

## <a name="next-steps"></a>Järgmised sammud

- ExpressRoute kohta leiate lisateavet teemast [ExpressRoute KKK](expressroute-faqs.md).
- Veenduge, et kõik eeltingimused on täidetud. Vt [ExpressRoute eeltingimused](expressroute-prerequisites.md).

<!--Image References-->
[0]: ./media/expressroute-locations/expressroute-locations-map.png "Asukoha kaart"
