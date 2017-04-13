<properties 
    pageTitle="Rakenduse teenuse keskkonnas | Microsoft Azure'i" 
    description="Mis on Azure rakenduse teenuse keskkonnas? Sissejuhatus rakenduse teenuse keskkonnas." 
    keywords="Azure'i rakenduse teenuse keskkonnas, virtuaalse võrgu, secure võrgunduse"
    services="app-service" 
    documentationCenter="" 
    authors="stefsch" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/04/2016" 
    ms.author="stefsch"/>

# <a name="app-service-environment-documentation"></a>Rakenduse Teenusedokumentatsiooni keskkonnas

Rakenduse teenuse keskkond on [Premium] [ PremiumTier] teenuse leping võimalus Azure'i rakenduse teenus, mis sisaldab täielikult eraldatud ja pühendunud keskkonna turvaliselt ei tööta Azure'i rakendust Service rakenduste kõrge, sh [Veebirakenduste][WebApps], [Mobiilirakenduste][MobileApps], ja [API rakenduste][APIApps].  

Rakenduse teenuse keskkonnas on käepärane nõudva rakenduse töökoormus.

- Väga kõrge skaala
- Eraldamise ja turvalise võrgupääs

Kliente saate luua mitme rakenduse teenuse keskkonnas ühtse Azure piirkonna, samuti mitu Azure piirkondade lõikes.  See muudab rakenduse teenuse keskkonnas käepärane horisontaalselt skaleerimist riigi-vähem rakenduse astme toetavad suur RPS töökoormus.

Rakenduse teenuse keskkonnas on eraldatud töötavad ainult ühe kliendi rakendused ja on alati juurutatud virtuaalse võrku.  Kliendid on kohandatud kontroll nii sissetulevate ja väljaminevate rakenduse abil [võrgu turberühmad]võrguliikluse[NetworkSecurityGroups].  Rakenduste saate ka luua kiire turvalist ühendust üle virtuaalse võrgu kohapealse ettevõtte ressurssidega.

Rakenduste sageli vaja juurdepääsu ettevõtte ressursse, nt ettevõttesisese andmebaase ja web services.  Pääsevad rakendused rakendus teenuse keskkonnas töötavate ressursse, mis on kättesaadav [- Saitide] kaudu[ SiteToSite] VPN-i ja [Azure ExpressRoute] [ ExpressRoute] ühendused.

* [Mis on rakenduse teenuse keskkonnas?](../app-service-web/app-service-app-service-environment-intro.md)
* [Rakenduse teenuse keskkonna loomine](../app-service-web/app-service-web-how-to-create-an-app-service-environment.md)
* [Rakenduste loomise rakendus teenuse keskkonnas](../app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase.md)
* [Loomise ja kasutamise on sisemine laadi koormusetasakaalustusteenuse rakenduse teenuse keskkonnas](../app-service-web/app-service-environment-with-internal-load-balancer.md)
* [Rakenduse teenuse keskkonnas konfigureerimine](../app-service-web/app-service-web-configure-an-app-service-environment.md) 
* [Skaleerimise rakendused rakendus teenuse keskkonnas](../app-service-web/app-service-web-scale-a-web-app-in-an-app-service-environment.md)
* [Võrgu turvalisus ja arhitektuur](../app-service-web/app-service-app-service-environment-network-architecture-overview.md)

## <a name="how-tos"></a>Kuidas kontakti

[AZURE.INCLUDE [app-service-blueprint-app-service-environment](../../includes/app-service-blueprint-app-service-environment.md)]


## <a name="videos"></a>Videod
[AZURE.VIDEO azurecon-2015-deploying-highly-scalable-and-secure-web-and-mobile-apps]

[AZURE.VIDEO microsoft-ignite-2015-running-enterprise-web-and-mobile-apps-on-azure-app-service]


<!-- LINKS -->
[PremiumTier]: http://azure.microsoft.com/pricing/details/app-service/
[WebApps]: http://azure.microsoft.com/documentation/articles/app-service-web-overview/
[MobileApps]: http://azure.microsoft.com/documentation/articles/app-service-mobile-value-prop-preview/
[APIApps]: http://azure.microsoft.com/documentation/articles/app-service-api-apps-why-best-platform/
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[SiteToSite]: https://azure.microsoft.com/documentation/articles/vpn-gateway-site-to-site-create/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
