<properties 
    pageTitle="Rakenduse teenuse keskkonnas Web rakenduse tulemüüri (WAF) konfigureerimine" 
    description="Saate teada, kuidas konfigureerida web rakenduse tulemüüri rakenduse teenuse keskkonna ees." 
    services="app-service\web" 
    documentationCenter="" 
    authors="naziml" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/17/2016" 
    ms.author="naziml"/>    

# <a name="configuring-a-web-application-firewall-waf-for-app-service-environment"></a>Rakenduse teenuse keskkonnas Web rakenduse tulemüüri (WAF) konfigureerimine

## <a name="overview"></a>Ülevaade ##
Web rakenduse tulemüürid nagu [Barracuda WAF Azure](https://www.barracuda.com/programs/azure) , mis on kättesaadav [Azure'i turuplatsi](https://azure.microsoft.com/marketplace/partners/barracudanetworks/waf-byol/) aitab turvaline kontrollimise käigus oma veebirakenduste sissetulev web-liikluse blokeerida SQL-i süstid, saitidevahelise skriptimise, ründevara üles ja rakenduse DDoS ja teiste eest. See kontrollib ka vastuseid tagaandmebaas web serveritest andmete kaotsimineku vältimine (DLP). Koos eraldi ja täiendavad skaleerimist esitatud rakenduse teenuse keskkonnas, see pakub host business kriitilised web rakendustele on vaja pahatahtlik taotlusi ja suuremahulise liikluse optimaalne keskkonnas.

+[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="setup"></a>Häälestamine ##
Selle dokumendi me tuleb konfigureerida rakendus teenuse keskkonna taha mitme laadi tasakaalustatud eksemplarid Barracuda WAF nii, et ainult liiklus WAF jõuavad rakenduse teenuse keskkonna ja see ei ole DMZ kaudu juurdepääsetavad. Meil on ka Azure liikluse haldur ette meie Barracuda WAF eksemplarid laadimiseks saldo Azure andmekeskuste ja piirkondade vahel. Kõrge taseme skeem häälestamise näeks, mis on näidatud allpool.

![Arhitektuur][Architecture] 

> Märkus: Sissejuhatus [ILB toetavad rakenduse teenuse keskkonnas](app-service-environment-with-internal-load-balancer.md), saate konfigureerida veebirakendusele DMZ ja ainult kättesaadav privaatvõrgu ASE. 

## <a name="configuring-your-app-service-environment"></a>Rakenduse teenuse keskkonna konfigureerimise ##
Konfigureerida rakenduse teenuse keskkonnas vaadake [meie dokumentatsiooni](app-service-web-how-to-create-an-app-service-environment.md) teemal. Kui teil on rakenduse keskkond loodud, saate luua [Veebirakenduste](app-service-web-overview.md), [API rakendusi](../app-service-api/app-service-api-apps-why-best-platform.md) ja [Mobiilirakenduste](../app-service-mobile/app-service-mobile-value-prop.md) selles keskkonnas, kus on kõik kaitstud me konfigureerida järgmises jaotises WAF taha.

## <a name="configuring-your-barracuda-waf-cloud-service"></a>Barracuda WAF pilveteenuses konfigureerimine ##
Barracuda on [üksikasjalik artikkel](https://campus.barracuda.com/product/webapplicationfirewall/article/WAF/DeployWAFInAzure) sisse oma WAF virtual arvutisse Azure juurutamine. Kuid kuna me soovite koondamise ja pole tutvustamine tekkimist tõrge, soovite juurutada vähemalt 2 WAF eksemplari VMs sisse sama pilveteenuses neid juhiseid järgides.

### <a name="adding-endpoints-to-cloud-service"></a>Lõpp-punktid Cloud teenuse lisamine ###
Kui teil on 2 või rohkem WAF VM eksemplari oma pilveteenuses saate [Azure portaali](https://portal.azure.com/) lisada HTTP ja HTTPS-i lõpp-punktid, mis kasutavad rakendust, nagu on näidatud järgmisel pildil.

![Lõpp-punkti][ConfigureEndpoint]

Kui rakenduste kasutada muude lõpp-punktid, veenduge, et lisada need sellesse loendisse. 

### <a name="configuring-barracuda-waf-through-its-management-portal"></a>Barracuda WAF kaudu oma haldusportaal konfigureerimine ###
Barracuda WAF kasutab TCP Port 8000 konfigureerimise kaudu oma haldusportaal. Kuna meil on mitu eksemplari WAF VMs peate korrake toiminguid siin iga VM eksemplari. 


> Märkus: Kui olete teinud WAF konfiguratsioon, eemaldage TCP/8000 lõpp-punkti kõik teie WAF VMs hoida oma WAF turvaline.

Lisage halduse lõpp-punkti, nagu on näidatud järgmisel pildil oma Barracuda WAF konfigureerimiseks.

![Lisage halduse lõpp-punkti][AddManagementEndpoint]
 
Otsige sirvides üles oma pilveteenuses halduse lõpp brauseri abil. Kui teie pilveteenuses nimetatakse test.cloudapp.net, tuleks pääsete selle lõpp-punkti http://test.cloudapp.net:8000 sirvides. Peaksite nägema sisselogimise lehele nagu allpool teie määratud WAF VM häälestamise etapp mandaadi abil sisse.

![Sisselogimislehe haldus][ManagementLoginPage]

Üks kord sisselogimist peaksite nägema armatuurlaua üks alloleval pildil, mis esitab lihtsa statistikat WAF kaitse.

![Halduse armatuurlaua][ManagementDashboard]

Klõpsake vahekaardil teenused võimaldab teil oma WAF see kaitseb teenuste konfigureerimine. Teie Barracuda WAF konfigureerimise kohta lisateabe saamiseks võite tutvuda [oma dokumendid](https://techlib.barracuda.com/waf/getstarted1). Azure'i Web App järgmises näites on konfigureeritud serveeritakse liikluse HTTP ja HTTPS.

![Halduse teenuste lisamine][ManagementAddServices]

> Märkus: Sõltuvalt sellest, kuidas teie rakendused on konfigureeritud ja millised funktsioonid on kasutusel rakenduse teenuse keskkonna, peate suunamise liikluse TCP pordid 80 ja 443, v.a näiteks kui teil on IP SSL-i häälestamise Web App. Kasutada rakenduse teenuse keskkonnas võrgu portide loendit, vaadake jaotises [juhtelemendi sissetulev liiklus dokumentatsiooni](app-service-app-service-environment-control-inbound-traffic.md) võrgu pordid.

## <a name="configuring-microsoft-azure-traffic-manager-optional"></a>Konfigureerida Microsoft Azure liikluse Manager (valikuline) ##
Kui teie taotlus on mitme piirkondades saadaval siis, kui soovite laadida saldo need taha [Azure'i liikluse haldur](../traffic-manager/traffic-manager-overview.md). Selleks saate lisada lõpp [Azure klassikaline portaali](https://manage.azure.com) kasutamine pilveteenuses nimi oma WAF liikluse haldur profiili, nagu on näidatud järgmisel pildil. 

![Liikluse haldur lõpp-punkti][TrafficManagerEndpoint]

Kui teie rakendus nõuab autentimist, veenduge, et teil on teatud ressurss, mis ei nõua mis tahes autentimise liikluse haldur ping rakenduse olemasolu. URL-i konfigureerimine jaotises saate konfigureerida [Azure klassikaline portaali](https://manage.azure.com) , nagu allpool näidatud.

![Liikluse haldur konfigureerimine][ConfigureTrafficManager]

Oma WAF liikluse haldur ping suunata rakenduse peate veebisaidi tõlked häälestamise kohta oma Barracuda WAF liikluse suunata rakenduse, nagu on näidatud järgmises näites.

![Veebisaidi tõlked][WebsiteTranslations]

## <a name="securing-traffic-to-app-service-environment-using-network-security-groups-nsg"></a>Turvaliseks liikluse rakenduse teenuse keskkonnas võrgu turberühmad (NSG) abil##
Järgige [juhtelemendi sissetulev liiklus dokumentatsiooni](app-service-app-service-environment-control-inbound-traffic.md) üksikasjad piirata liikluse oma rakenduse teenuse keskkonnas WAF ainult abil oma pilveteenuses VIP-aadressi. Siin on valimi PowerShelli käsu TCP port 80 selle ülesande jaoks.


    Get-AzureNetworkSecurityGroup -Name "RestrictWestUSAppAccess" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP Barracuda" -Type Inbound -Priority 201 -Action Allow -SourceAddressPrefix '191.0.0.1'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP

Asendage funktsiooni SourceAddressPrefix koos virtuaalse IP Address (VIP), teie WAF pilveteenuses.

> Märkus: Muudab oma pilveteenuses VIP kustutamine ja uuesti loomisel pilveteenusesse. Veenduge, et jaotises võrku ressursi IP-aadressi värskendamiseks, kui teete. 
 
<!-- IMAGES -->
[Architecture]: ./media/app-service-app-service-environment-web-application-firewall/Architecture.png
[ConfigureEndpoint]: ./media/app-service-app-service-environment-web-application-firewall/ConfigureEndpoint.png
[AddManagementEndpoint]: ./media/app-service-app-service-environment-web-application-firewall/AddManagementEndpoint.png
[ManagementAddServices]: ./media/app-service-app-service-environment-web-application-firewall/ManagementAddServices.png
[ManagementDashboard]: ./media/app-service-app-service-environment-web-application-firewall/ManagementDashboard.png
[ManagementLoginPage]: ./media/app-service-app-service-environment-web-application-firewall/ManagementLoginPage.png
[TrafficManagerEndpoint]: ./media/app-service-app-service-environment-web-application-firewall/TrafficManagerEndpoint.png
[ConfigureTrafficManager]: ./media/app-service-app-service-environment-web-application-firewall/ConfigureTrafficManager.png
[WebsiteTranslations]: ./media/app-service-app-service-environment-web-application-firewall/WebsiteTranslations.png
