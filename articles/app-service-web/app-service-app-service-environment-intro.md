<properties 
    pageTitle="Sissejuhatus rakenduse teenuse keskkonnas" 
    description="Lisateavet rakenduse teenuse keskkond, mis on funktsioon, mis pakub turvaline, VNet liitunud, spetsiaalne skaala üksused töötab kõigi rakenduste kuvamiseks." 
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

# <a name="introduction-to-app-service-environment"></a>Sissejuhatus rakenduse teenuse keskkonnas

## <a name="overview"></a>Ülevaade ##
Rakenduse teenuse keskkond on [Premium] [ PremiumTier] teenuse leping võimalus Azure'i rakenduse teenus, mis sisaldab täielikult eraldatud ja pühendunud keskkonna turvaliselt ei tööta Azure'i rakendust Service rakenduste kõrge, sh [Veebirakenduste][WebApps], [Mobiilirakenduste][MobileApps], ja [API rakenduste][APIApps].  

Rakenduse teenuse keskkonnas on käepärane nõudva rakenduse töökoormus.

- Väga kõrge skaala
- Eraldamise ja turvalise võrgupääs

Kliente saate luua mitme rakenduse teenuse keskkonnas ühtse Azure piirkonna, samuti mitu Azure piirkondade lõikes.  See muudab rakenduse teenuse keskkonnas käepärane horisontaalselt skaleerimist riigi-vähem rakenduse astme toetavad suur RPS töökoormus.

Rakenduse teenuse keskkonnas on eraldatud töötavad ainult ühe kliendi rakendused ja on alati juurutatud virtuaalse võrku.  Kliendid on kohandatud kontroll nii sissetulevate ja väljaminevate rakenduse võrguliiklust ja rakendused saate luua kiire turvalist ühendust üle virtuaalse võrgu kohapealse ettevõtte ressurssidega.

Kõik artiklid ja kuidas-'s rakenduse teenuse keskkonnas on saadaval [seletusfail rakenduse teenuse keskkonna jaoks](../app-service/app-service-app-service-environments-readme.md).

Ülevaate sellest, kuidas rakenduse teenuse keskkonnas lubada suure ulatuse ja turvata võrgu kasutamine, vt [AzureCon Deep Dive] [ AzureConDeepDive] rakenduse teenuse keskkonna!

Suure sukelduda horisontaalselt skaleerimist kasutamise kohta mitme rakenduse teenuse keskkonnas leiate artiklist Kuidas seadistada on [rakenduse geo jaotatud andmed väiksemates vähem ruumi][GeodistributedAppFootprint].

Turbearhitektuur, näidatud AzureCon Deep Dive konfiguratsioonist vaatamiseks artiklist rakendamine on [kihiti turbearhitektuur](app-service-app-service-environment-layered-security.md) rakenduse teenuse keskkonnas.

Rakenduse teenuse keskkonnas käivitatud rakendused on nende mis varustava seadmed, nt web rakenduse tulemüürid (WAF).  Artikli [WAF, rakenduse teenuse keskkonna jaoks konfigureerimise](app-service-app-service-environment-web-application-firewall.md) kohta käsitletakse seda stsenaariumi. 

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="dedicated-compute-resources"></a>Sihtotstarbeline Arvuta ressursid ##
Kõik Arvuta ressursid keskkonnas teenuse rakendus on mõeldud ainult ühe tellimuse ja rakenduse teenuse keskkonnas saab konfigureerida kuni viiskümmend (50) Arvuta ressursid kasutamiseks ainult ühes rakenduses.

Rakenduse teenuse keskkonnas koosneb ees Arvuta ressursivaru, samuti üks kuni kolm töötaja Arvuta ressursi kaustu. 

Ees pool sisaldab Arvuta ressursse SSL-i lõpetamise kui ka automaatse koormusetasakaalustuseks rakendusetaotluste sees keskkonnas rakenduse teenuse eest vastutav. 

Iga töötaja kaust sisaldab Arvuta eraldatud [Rakenduse teenuse lepingud][AppServicePlan], mis sisaldavad omakorda ühe või mitme Azure'i rakendust Service rakendused.  Kuna rakendus teenuse keskkonnas võib olla kuni kolmest eri töötaja, on teil valida muu Arvuta ressursid iga töötaja kaust paindlikkust.  

Näiteks see võimaldab teil luua ühe töötaja rakenduskausta vähem võimas Arvuta ressurssidega rakenduse teenuse lepingud mõeldud rakenduste arendamise või test.  Teine (või isegi kolmas) töötaja pool võib kasutada võimsam Arvuta ressursse, mis on mõeldud rakenduse teenuse lepingud tootmise rakenduste.

Lisateavet Arvuta ressursside kaustu ees- ja töötaja saa kogus, vaadake, [Kuidas konfigureerimine keskkonnas rakenduse teenuse][HowToConfigureanAppServiceEnvironment].  

Täpsemat on saadaval arvutada ressursi suurused toetatud rakenduse teenuse keskkonnas, küsige [Rakenduse teenuse hinnad] [ AppServicePricing] lehele ja vaadake üle rakenduse teenuse keskkonnas Premiumi, hinnad taseme saadaolevate suvandite.

## <a name="virtual-network-support"></a>Virtuaalne tugi ##
Teenuse keskkonnas rakenduse saab luua rakenduses **mõlemal** on Azure ressursihaldur virtuaalse võrgu, **või** klassikaline mudel virtuaalse võrgu ([virtuaalne võrkude kohta leiate][MoreInfoOnVirtualNetworks]).  Kuna rakendus teenuse keskkonnas olemas alati virtuaalse võrgu ja täpsemalt sees alamvõrgu virtuaalse võrgu, saate kasutada turbefunktsioonid virtuaalse võrkude määrata nii sissetulevate ja väljaminevate võrgu suhtlus.  

Rakenduse teenuse keskkonnas võib olla kas Interneti avaliku IP-aadress või sisemise vastastikuste ainult Azure'i sisemine laadi koormusetasakaalustusteenuse (ILB) aadress.

Saate kasutada [võrgu turberühmad] [ NetworkSecurityGroups] piirata sissetuleva võrgu side alamvõrku, kus asub rakenduse teenuse keskkonnas.  See võimaldab teil rakendused taha varustava seadmed ja teenused, nagu web rakenduse tulemüürid ja SaaS pakkujad käivitamiseks.

Rakenduste sageli vaja juurdepääsu ettevõtte ressursse, nt ettevõttesisese andmebaase ja web services.  Levinum lähenemine on teha neid lõpp-punktid saadaval ainult voolav Azure virtuaalse võrgustikus sisemise võrguliiklust.  Kui rakendus teenuse keskkond on ühendatud samasse võrku virtuaalse sise teenuseid, rakendusi keskkonnas pääsevad need, sh lõpp-punktid eesandmebaasis [- Saitide] kaudu[ SiteToSite] ja [Azure ExpressRoute] [ ExpressRoute] ühendused.

Täpsemat teavet rakenduse teenuse keskkonnas virtuaalse võrkude ja kohapealse võrgu kasutamise kohta leiate järgmised artiklid [Võrgu arhitektuur][NetworkArchitectureOverview], [Juhtimine sissetulev liiklus][ControllingInboundTraffic], ja [Turvaliselt ühenduse taustaprogrammid][SecurelyConnectingToBackends]. 

## <a name="getting-started"></a>Alustamine

Alustamine rakenduse teenuse keskkonnas, lugege teemat [Kuidas soovite luua An rakenduse teenuse keskkonnas][HowToCreateAnAppServiceEnvironment]

Kõik artiklid ja kuidas-'s rakenduse teenuse keskkonnas on saadaval [seletusfail rakenduse teenuse keskkonna jaoks](../app-service/app-service-app-service-environments-readme.md).

Azure'i rakendust Service platform kohta leiate lisateavet teemast [Azure rakenduse teenuse][AzureAppService].

Rakenduse teenuse keskkonnas võrgu arhitektuur ülevaate leiate teemast [Võrgu arhitektuur ülevaade] [ NetworkArchitectureOverview] artikkel.

Jaoks üksikasjad ExpressRoute, keskkonnas rakenduse teenuse kasutamise kohta leiate järgmistest artiklitest [teekonna]ja rakenduse teenuse keskkonnas[NetworkConfigDetailsForExpressRoute].

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[PremiumTier]: http://azure.microsoft.com/pricing/details/app-service/
[MoreInfoOnVirtualNetworks]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[AppServicePlan]: http://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/
[HowToCreateAnAppServiceEnvironment]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[WebApps]: http://azure.microsoft.com/documentation/articles/app-service-web-overview/
[MobileApps]: http://azure.microsoft.com/documentation/articles/app-service-mobile-value-prop-preview/
[APIApps]: http://azure.microsoft.com/documentation/articles/app-service-api-apps-why-best-platform/
[LogicApps]: http://azure.microsoft.com/documentation/articles/app-service-logic-what-are-logic-apps/
[AzureConDeepDive]:  https://azure.microsoft.com/documentation/videos/azurecon-2015-deploying-highly-scalable-and-secure-web-and-mobile-apps/
[GeodistributedAppFootprint]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-geo-distributed-scale/
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[SiteToSite]: https://azure.microsoft.com/documentation/articles/vpn-gateway-site-to-site-create/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
[HowToConfigureanAppServiceEnvironment]:  http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment/
[ControllingInboundTraffic]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[SecurelyConnectingToBackends]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-securely-connecting-to-backend-resources/
[NetworkArchitectureOverview]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-architecture-overview/
[NetworkConfigDetailsForExpressRoute]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-configuration-expressroute/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/ 

<!-- IMAGES -->

 
