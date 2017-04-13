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
|**Põhja-Ameerika**|Ida-USA, Lääne USA, Ida-USA 2, Kesk-USA, Lõuna-, Kesk-USA, Põhja Kesk-USA, Kanada Central, Kanada Ida|Atlanta, Chicago, Dallase, Las Vegas, Los Angeles, New Yorgis, Seattle, Edenvale, Washington AV, Montreali +, Quebecis linn +, Toronto|
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

| **Teenuse pakkuja**  |**Microsoft Azure'i** | **Office 365 ja CRM Online'i** | **Asukohad** |
|-----------------------|--------------------|----------------|---------------|
| **AARNet** | Toetatud | Toetatud | Melbourne, Sydney |
| **[Aryaka võrkude]( http://www.aryaka.com/)** | Toetatud | Toetatud | Amsterdam, Edenvale, Singapur, Tokyo, Washington AV |
| **[AT & T NetBond]( https://www.synaptic.att.com/clouduser/html/productdetail/ATT_NetBond.htm)** | Toetatud | Toetatud | Amsterdam, Chicago, Dallase, Londoni, Edenvale, Singapur, Sydney, Washington AV |
| **[British Telecom]( http://www.globalservices.bt.com/uk/en/news/bt_to_provide_connectivity_to_microsoft_azure)** | Toetatud | Toetatud | Amsterdam, Hongkongi, Londoni, Edenvale, Singapur, Sydney, Tokyo, Washington AV |
|**CenturyLink** | Tulen varsti | Tulen varsti| Edenvale |
|**Hiina Telecom globaalne** | Toetatud | Pole toetatud | Hongkong |
|**[Cologix](http://www.cologix.com/solutions/cloud-connect/public-clouds/microsoft-cloud/)** | Toetatud | Tulen varsti | Dallase Montreali + Toronto |
| **[Colt]( http://www.colt.net/uk/en/news/colt-announces-dedicated-cloud-access-for-microsoft-azure-services-en.htm)**  |  Toetatud | Toetatud | Amsterdam, Dublini, Londoni Tokyo |
| **Comcast** | Toetatud | Toetatud | Chicago, Edenvale, Washington AV |
| **[CoreSite](http://www.coresite.com/solutions/cloud-services/public-cloud-providers/microsoft-azure-expressroute)** | Toetatud | Toetatud | Narva | 
| **[Equinix](http://www.equinix.com/partners/microsoft-azure/)** | Toetatud | Toetatud | Amsterdam, Atlanta, Chicago, Dallase, Hongkongi, Londoni, Los Angeles, Melbourne, New Yorgis, Osaka, Pariis +, Irvine, Seattle, Edenvale, Singapur, Sydney, Tokyo, Toronto, Washington AV |
| **euNetworks** |  Toetatud | Toetatud | Amsterdam |
| **GÉANT** | Toetatud | Toetatud | Amsterdam |
| **[Internet algatus Jaapan Inc - IIJ](http://www.iij.ad.jp/en/news/pressrelease/2015/1216-2.html)** |  Toetatud | Toetatud | Osaka, Tokyo |
| **[InterCloud]( https://www.intercloud.com/)** | Toetatud | Toetatud | Amsterdami Londoni aja järgi, Singapur, Washington AV |
| **Lahenduste Interneti - Cloud ühenduse loomine** | Toetatud | Toetatud | Amsterdami Londoni |
| **[Interxion](http://www.interxion.com/why-interxion/colocate-with-the-clouds/colocated-hybrid-cloud/microsoft-azure/)**  | Toetatud | Toetatud | Amsterdami Londoni Pariis |
| **JISC** | Toetatud | Toetatud | Londoni | 
| **[Tase 3 suhtlus]( http://your.level3.com/LP=882?WT.tsrc=02192014LP882AzureVanityAzureText)** | Toetatud | Toetatud | Amsterdam, Chicago, Dallase, Las Vegas +, Londoni, Seattle, Edenvale, Washington AV |
| **Megaport** | Toetatud | Toetatud | Dallase Hongkong, Las Vegas, Los Angeles, Melbourne, New Yorgis Seattle, Singapur, Sydney, Washington AV |
| **MTN** | Toetatud | Toetatud | Londoni |
| **Järgmise genereerimine andmed** | Tulen varsti | Tulen varsti | Newport(Wales) + |
| **NEXTDC** | Toetatud | Toetatud | Melbourne, Sydney |
| **NTT suhtlus** | Toetatud | Toetatud | Londoni Los Angeles, Osaka, Tokyo |
| **[Oranž]( http://www.orange-business.com/en/products/business-vpn-galerie)** | Toetatud | Toetatud | Hong Kong, Amsterdami Londoni, Edenvale, Singapur, Sydney, Washington AV |
| **Globaalne PCCW piiratud** | Toetatud | Toetatud | Hongkong |
| **[SingTel]( http://info.singtel.com/about-us/news-releases/singtel-provide-secure-private-access-microsoft-azure-public-cloud)** |  Toetatud | Toetatud | Singapur |
| **SoftBank** | Toetatud | Toetatud | Osaka, Tokyo | 
| **[Tata suhtlus](http://www.tatacommunications.com/lp/izo/azure/azure_index.html)** | Toetatud | Toetatud | Amsterdam, Chennai, Hong Kong, Londoni Mumbai, Edenvale, Singapur, Washington AV |
| **[TeleCity rühma]( http://www.telecitygroup.com/investor-centre/news_details.htm?locid=03100500400b00d&xml)** | Toetatud | Toetatud | Amsterdam, Dublini, Londoni |
| **Telefonica** | Toetatud | Toetatud | Irvine |
| **Telenor** | Toetatud | Toetatud | Amsterdami Londoni |
| **[Telstra Corporation]( http://www.telstra.com.au/business-enterprise/network-services/networks/cloud-direct-connect/)** | Toetatud | Toetatud | Melbourne, Sydney |
| **[Verizon](http://www.verizonenterprise.com/products/networking/secure-cloud-interconnect/)** | Toetatud | Toetatud | Amsterdam, Hongkongi, Londoni, Edenvale, Singapur, Sydney, Tokyo, Washington AV |
| **Vodafone** | Toetatud | Pole toetatud | Londoni | 
| **[Zayo rühma]( http://www.zayo.com/solutions/industries/connect-to-cloud-data-centers/cloud-connectivity/microsoft-expressroute/)** | Toetatud | Toetatud | Chicago, Los Angeles, New Yorgis Edenvale, Toronto, Washington AV |

 **+**tähistab tulekul

### <a name="national-cloud-environments"></a>Riigi cloud keskkonnas

#### <a name="us-government-cloud"></a>USA valitsuse pilveteenuses

| **Teenuse pakkuja**  |**Microsoft Azure'i** | **Office 365** | **Asukohad** |
|-----------------------|--------------------|----------------|---------------|
| **[AT & T NetBond]( https://www.synaptic.att.com/clouduser/html/productdetail/ATT_NetBond.htm)** | Toetatud | Toetatud | Chicago, Washington AV |
| **[Equinix](http://www.equinix.com/partners/microsoft-azure/)** | Toetatud | Toetatud | Chicago, Dallase, New York, Washington AV |
| **[Tase 3 suhtlus]( http://your.level3.com/LP=882?WT.tsrc=02192014LP882AzureVanityAzureText)** | Toetatud | Toetatud | Chicago, New York +, Washington AV |
| **[Verizon](http://news.verizonenterprise.com/2014/04/secure-cloud-interconnect-solutions-enterprise/)** | Toetatud | Toetatud | Chicago, Dallase + New York, Washington AV |

#### <a name="china"></a>Hiina

| **Teenuse pakkuja**  |**Microsoft Azure'i** | **Office 365** | **Asukohad** |
|-----------------------|--------------------|----------------|---------------|
| **Hiina Telecom** | Toetatud | Pole toetatud | Peking, Shanghai|
Lisateavet leiate teemast [ExpressRoute Hiinas](http://www.windowsazure.cn/home/features/expressroute/).

#### <a name="germany"></a>Saksamaa

| **Teenuse pakkuja**  |**Microsoft Azure'i** | **Office 365** | **Asukohad** |
|-----------------------|--------------------|----------------|---------------|
| **[Colt]( http://www.colt.net/uk/en/news/colt-announces-dedicated-cloud-access-for-microsoft-azure-services-en.htm)** | Toetatud | Pole toetatud | Berliini +, Frankfurt|
| **[Equinix](http://www.equinix.com/partners/microsoft-azure/)** | Toetatud | Pole toetatud | Frankfurt|
| **e-varju** | Toetatud | Pole toetatud | Berliini|
| **Interxion** | Toetatud | Pole toetatud | Frankfurt|

## <a name="nonpartners"></a>Ühenduvus pole loendis teenusepakkujatele kaudu

Kui eelmiste jaotiste pakkuja Ühenduvus pole loendis, saate ikkagi ühenduse luua.

- Märkige ruut kuvamiseks, kui need on ühendatud vahetada ülaltoodud tabelis mis tahes ühenduvuse teenusepakkuja juures. Saate järgmisi linke koguda pakutavaid Exchange'i pakkujate kohta lisateavet. Mitme ühenduvuse pakkujate ühendatud Ethernet vahetamine.

    - [Equinix pilvepõhise Exchange'i](http://www.equinix.com/services/interconnection-connectivity/cloud-exchange/)
    - [TeleCity CloudIX](http://www.telecitygroup.com/colocation-services/cloud-ix.htm)
    - [Interxion](http://www.interxion.com/why-interxion/colocate-with-the-clouds/colocated-hybrid-cloud/microsoft-azure/)
    - [NextDC](http://www.nextdc.com/)
    - [CoreSite](http://www.coresite.com/)
    - [Cologix](http://www.cologix.com/)
- On laiendamine silmitsemine asukoha valik võrgu Ühenduvus pakkuja.
    - Veenduge, et pakkuja Ühenduvus laiendab oma ühenduvuse väga kättesaadav viisil nii, et on pole ühe punkti tõrgete.
- Tellige mõne ExpressRoute ringi vahetamist nimega Microsoft ühenduse pakkuja Ühenduvus.
    - Järgige juhiseid [loomine mõne ExpressRoute ringi](expressroute-howto-circuit-classic.md) häälestamiseks Ühenduvus.

|**Ühenduvus pakkuja**|**Exchange'i**|**Asukohad**|
|---|---|---|
|**[1CLOUDSTAR](http://www.1cloudstar.com/service/cloudconnect-azure-expressroute/)**|Equinix|Singapur|
|**Alaskat suhtlus**|Equinix|Seattle|
|**[Lightower](http://www.lightower.com/network-solutions/cloud-connect/#microsoft-azure )**|Equinix|New York, Washington AV|
|**[Lucent](http://www.xo.com/)**|Equinix|Edenvale|


## <a name="expressroute-system-integrators"></a>ExpressRoute süsteemiintegraatorid

Lubada privaatne ühenduvuse vastaks paremini teie vajadustele võib olla keeruline, põhineb võrgu skaala. Saate töötada mis tahes süsteemiintegraatorid, aidata teil teenusekomplekti, et ExpressRoute järgmises tabelis loetletud.

|**Koostaja**|**Continent**|
|---|---|
|**[Avanade Inc kaubamärk.](http://www.avanade.com/)**| Asia, Europe, USA |
|**[DotNet lahendused](http://www.dotnetsolutions.co.uk/)**| Euroopa |
|**[Equinix professionaalsete teenuste](http://www.equinix.com/services/consulting/)**|MEILE|
|**[OneAs1a](http://www.oneas1a.com/express-connect-any-cloud-ecac)** | Asia |
|**[Perficient](http://www.perficient.com/Partners/Microsoft/Cloud/Azure-ExpressRoute)** | MEILE |
|**[Projekti juhtimine](http://www.projectleadership.net/azure)** | MEILE |

## <a name="next-steps"></a>Järgmised sammud

- ExpressRoute kohta leiate lisateavet teemast [ExpressRoute KKK](expressroute-faqs.md).
- Veenduge, et kõik eeltingimused on täidetud. Vt [ExpressRoute eeltingimused](expressroute-prerequisites.md).

<!--Image References-->
[0]: ./media/expressroute-locations/expressroute-locations-map.png "Asukoha kaart"
