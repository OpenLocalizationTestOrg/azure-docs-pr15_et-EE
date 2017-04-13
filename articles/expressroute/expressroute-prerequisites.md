<properties
   pageTitle="Eeltingimused ExpressRoute vastuvõtmiseks | Microsoft Azure'i"
   description="Sellelt lehelt leiate loendi nõudeid täita, enne kui saate tellida mõni Azure ExpressRoute ringi."
   documentationCenter="na"
   services="expressroute"
   authors="cherylmc"
   manager="carmonm"
   editor=""/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="cherylmc"/>


# <a name="expressroute-prerequisites--checklist"></a>ExpressRoute eeltingimused ja kontroll-loend  

Kasutades ExpressRoute Microsofti pilveteenustega ühendust luua, peate veenduge, et allpool toodud järgmised nõuded on täidetud.

[AZURE.INCLUDE [expressroute-office365-include](../../includes/expressroute-office365-include.md)]

## <a name="azure-account"></a>Azure'i konto

- Kehtiv ja Microsoft Azure'i kontosse. See on vajalik ExpressRoute ringi häälestamine. ExpressRoute topoloogia on ressursside Azure'i tellimused. Azure'i tellimuse on ka siis, kui ühendus on piiratud Microsoft Azure'i pilveteenustega, nt Office 365 teenuste ja CRM Online'i.
- Aktiivne Office 365 tellimus (kui teenusekomplekti Office 365 teenused). Lisateavet käesoleva artikli jaotisest [Office 365 erinõuetega](#office-365-specific-requirements) .

## <a name="connectivity-provider"></a>Ühenduvus pakkuja
- Saate töötada [ExpressRoute ühenduvuse partneri](expressroute-locations.md#partners) ühenduse Microsofti pilveteenuse. Saate häälestada oma kohapealse võrgu ja Microsofti vahelise ühenduse on [kolm võimalust](expressroute-introduction.md#howtoconnect). 
- Kui teie pakkuja on pole ExpressRoute ühenduvuse partner, saate endiselt ühendada Microsofti pilveteenuse [Exchange'i pakkuja](expressroute-locations.md#nonpartners)kaudu.

## <a name="network-requirements"></a>Võrgu nõuded
- **Üleliigne Ühenduvus**: ei ole koondamise füüsilise teie ja teie pakkuja vahelise ühenduvuse kohta. Microsoft ei nõua liigsete BGP seansid tuleb luua Microsofti ruuterid ja silmitsemine ruuterid vahel, isegi siis, kui teil on ainult [üks füüsiline ühendus pilveteenuse Exchange](expressroute-faqs.md#onep2plink). 
- **Marsruutimine**: sõltuvalt sellest, kuidas ühendada Microsofti Cloud, teie või teie pakkuja vaja häälestada ja hallata BGP seansid [marsruutimise domeenide](expressroute-circuit-peerings.md)jaoks. Mõned Ethernet ühenduvuse pakkuja või Exchange'i teenusepakkuja võivad pakkuda BGP juhtimise mõne väärtuse – lisa teenus.
- **NAT**: Microsoft on võimalik ainult läbi Microsoft silmitsemine avaliku IP-aadressid. Kui te kasutate kohapealse võrgu privaatne IP-aadressid, teie või teie pakkuja vaja tõlkida privaatne IP-aadresside avaliku IP aadressid [NAT abil](expressroute-nat.md).
- **QoS**: Skype for Business on erinevate teenuste (nt heli, video, tekst), mis nõuavad liigendatud QoS kohtlemine. Teie ja teie pakkuja peaksite järgima [QoS nõuetele](expressroute-qos.md).
- **Võrgu turvalisus**: kaaluge [võrgu turvalisus](../best-practices-network-security.md) Microsoft Cloud ExpressRoute kaudu ühenduse loomisel.
 
## <a name="office-365"></a>Office 365

Kui kavatsete lubada Office 365 kasutamine ExpressRoute, vaadake lisateabe saamiseks Office 365 nõuded järgmised dokumendid.


- [Office 365 ExpressRoute ülevaade](https://support.office.com/en-us/article/Azure-ExpressRoute-for-Office-365-6d2534a2-c19c-4a99-be5e-33a0cee5d3bd)
- [Office 365 ExpressRoute marsruutimine](https://support.office.com/en-us/article/Routing-with-ExpressRoute-for-Office-365-e1da26c6-2d39-4379-af6f-4da213218408)
- [Office 365 URL-id ja IP-aadresside vahemikud](https://support.office.com/en-us/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2)
- [Võrgu plaanimine ja jõudluse häälestamine Teenusekomplektis Office 365](https://support.office.com/en-us/article/Network-planning-and-performance-tuning-for-Office-365-e5f1228c-da3c-4654-bf16-d163daee8848)
- [Võrgu läbilaskevõime kalkulaatorid ja tööriistad](https://support.office.com/en-us/article/Network-and-migration-planning-for-Office-365-f5ee6c33-bcd7-4b0b-b0f8-dc1d9fb8d132)
- [Office 365 integreerimine kohapealsete keskkondadega](https://support.office.com/en-us/article/Office-365-integration-with-on-premises-environments-263faf8d-aa21-428b-aed3-2021837a4b65)

## <a name="crm-online"></a>CRM Online'i 
Kui kavatsete lubada CRM Online'i ExpressRoute kohta, vaadake lisateavet CRM Online'i järgmised dokumendid

- [CRM Online'i URL-id](https://support.microsoft.com/kb/2655102) ja [IP-aadresside vahemikud](https://support.microsoft.com/kb/2728473)

## <a name="next-steps"></a>Järgmised sammud

- ExpressRoute kohta leiate lisateavet teemast [ExpressRoute KKK](expressroute-faqs.md).
- ExpressRoute ühenduvuse teenusepakkuja otsimine. Vt [ExpressRoute partnerid ja silmitsemine asukohad](expressroute-locations.md).
- Vaadake [teemast marsruutimine](expressroute-routing.md), [NAT](expressroute-nat.md) ja [QoS](expressroute-qos.md)nõuded.
- Konfigureerige ExpressRoute ühendust.
    - [Mõne ExpressRoute ringi loomine](expressroute-howto-circuit-classic.md)
    - [Marsruutimine konfigureerimine](expressroute-howto-routing-classic.md)
    - [Link on VNet mõne ExpressRoute ringi](expressroute-howto-linkvnet-classic.md)

