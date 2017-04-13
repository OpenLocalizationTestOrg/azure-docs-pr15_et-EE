<properties 
    pageTitle="Turvaline ühenduse kirjutamata ressursse rakenduse teenuse keskkonnas" 
    description="Lisateavet turvaliselt ühenduse loomiseks taustväärtus ressursse rakenduse teenuse keskkonnas." 
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

# <a name="securely-connecting-to-backend-resources-from-an-app-service-environment"></a>Turvaline ühenduse kirjutamata ressursse rakenduse teenuse keskkonnas #

## <a name="overview"></a>Ülevaade ##
Kuna rakendus teenuse keskkond on alati loodud rakenduses **mõlemal** on Azure ressursihaldur virtuaalse võrgu, **või** klassikaline mudel [virtuaalse võrgu][virtualnetwork], Väljaminevad ühendused keskkonnas rakenduse teenuse kirjutamata muud ressursid, et saate flow üksnes virtuaalse võrgu kaudu.  Tehtud muudatuse tehtud juuni 2016, kus praeguseks saab kasutada ka üheks virtuaalse võrke, mida kasutada avaliku aadresside vahemikud või RFC1918 aadress tühikute (st Privaatne aadressid).  

Näiteks võib olla SQL Server töötab klaster virtuaalmasinates Port 1433 lukus.  Lõpp-punkti võib ACLd ainult juurdepääsetav muudest ressurssidest samas virtuaalse võrgus.  

Teine näide tundliku lõpp-punktid käivitada kohapealse ja Azure kummagi [Saitide] kaudu ühendada[ SiteToSite] või [Azure'i ExpressRoute] [ ExpressRoute] ühendused.  Seetõttu saab ainult ressursside virtuaalse võrguga ühendatud-saidilt või ExpressRoute tunnelid juurde pääseda asutusesisese lõpp-punktid.

Kõik need stsenaariumid, töötab keskkonnas rakenduse teenuse rakendused on võimalik turvaliselt ühendada erinevad serverid ja ressursse.  Väljamineva liikluse rakendustest töötab keskkonnas rakenduse teenuse lõpp-punktid privaatne virtuaalse samasse võrku (või virtuaalse samasse võrku ühendatud), kuvatakse ainult meilivoo virtuaalse võrgu kaudu.  Väljamineva liikluse privaatne lõpp-punktid on liiguks avaliku Interneti kaudu.

Üks hoiatus kehtib väljaminev liiklus keskkonnas rakenduse teenuse lõpp-punktid virtuaalse võrgustikus abil.  Rakenduse teenuse keskkonnas ei jõua, asub **sama** nimega rakenduse teenuse keskkonna alamvõrgu virtuaalmasinates lõpp-punktid.  See tavalisel viisil ei tohiks probleem kui rakenduse teenuse keskkonnas on juurutatud alamvõrku, mis on reserveeritud kasutamiseks ainult ainult rakenduse teenuse keskkonnas sisse.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="outbound-connectivity-and-dns-requirements"></a>Väljamineva meili Ühenduvus ja DNS-i nõuded ##
Rakenduse teenuse keskkonnas õigesti, see nõuab väljaminev juurdepääsu erinevad lõpp-punktid. [Võrgu konfigureerimise jaoks ExpressRoute](app-service-app-service-environment-network-configuration-expressroute.md#required-network-connectivity) artikli jaotises "Nõutav võrguühendus" on väliste lõpp-punktid kasutavad ka ASE täieliku loendi.

Rakenduse teenuse keskkonnas nõua kehtiv DNS-i taristu virtuaalse võrgu jaoks konfigureeritud.  Kui mingil põhjusel DNS-i konfigureerimine muudetakse pärast keskkonnas rakenduse teenus on loodud, saate arendajate jõustada keskkonnas rakenduse teenuse hooleks uue DNS-i konfigureerimine.  Käivitamise jooksva keskkonna taaskäivitage arvuti abil "Taaskäivitus" ikoon, mis asub rakenduse teenuse keskkonna haldamise tera portaalis ülaosas põhjustab keskkonna hooleks uue DNS-i konfigureerimine.

Soovitatav, et kõik kohandatud DNS-i serverite on vnet olema häälestamise ette valmistada enne loomise rakendus teenuse keskkonnas.  Virtuaalse võrgu DNS-i konfiguratsiooni muutmise ajal rakenduse teenuse keskkonnas luuakse põhjustab rakenduse teenuse keskkonna loomise protsess ei ole.  Samamoodi, kui kohandatud DNS-i server olemas teises otsas VPN-lüüsi ja DNS-i server on kättesaamatu või pole saadaval, rakenduse teenuse keskkonna loomisprotsessi ka nurjub.

## <a name="connecting-to-a-sql-server"></a>Ühenduse loomine SQL serveri
Levinud SQL Server configuration on pordi 1433 kuulamise lõpp.

![SQL serveri lõpp-punkti][SqlServerEndpoint]

On kaks meetodite liikluse piiramine selle lõpp-punkti.


- [Võrgu pääsuloendid] [ NetworkAccessControlLists] (võrgu ACL-ID)

- [Võrgu turberühmad][NetworkSecurityGroups]


## <a name="restricting-access-with-a-network-acl"></a>Võrguga ACL juurdepääsu piiramine

Port 1433 saate turvatud võrgu pääsuloendi abil.  Ehk kliendi allolevas näites aadressid sees virtuaalse võrgu pärinevate ja blokeerib juurdepääsu kõigi teiste klientide.

![Võrgu juurdepääsu juhtimine loendi näide][NetworkAccessControlListExample]

Kõik rakendused rakendus teenuse keskkonnas virtuaalse samasse võrku SQL Server töötab saab ühenduse SQL serveri eksemplar SQL serveri virtuaalse masina kasutamise **VNet sisemise** IP-aadress.  

Alltoodud näites ühendusstringi viitab SQL serveri abil privaatne IP-aadress.

    Server=tcp:10.0.1.6;Database=MyDatabase;User ID=MyUser;Password=PasswordHere;provider=System.Data.SqlClient

Kuigi virtuaalse masina lisamine avaliku lõpp-punkti ka, kas ühenduse katsete abil avaliku IP-aadressi tõttu võrgu ACL tagasi lükata. 

## <a name="restricting-access-with-a-network-security-group"></a>Võrgu turberühma koos juurdepääsu piiramine
Juurdepääsu turvamiseks alternatiivne lähenemine on võrgu turberühma.  Võrgu turberühmad saab rakendada üksiku virtuaalmasinates või mõne alamvõrku, mis sisaldab virtuaalmasinates.

Kõigepealt tuleb luua võrgu turberühma:

    New-AzureNetworkSecurityGroup -Name "testNSGexample" -Location "South Central US" -Label "Example network security group for an app service environment"

Ainult VNet sisemise liikluse juurdepääsu piiramine on väga lihtne võrgu turberühma.  Vaikereeglit võrgu turvalisus jaotises Luba juurdepääs ainult muud võrgu kliendi sama virtuaalse võrgu kaudu.

Selle tulemusena lukustada SQL serveri juurdepääsu on sama lihtne kui töötab SQL serveri või alamvõrku, mis sisaldab soovitud virtuaalmasinates virtuaalmasinates võrgu turberühma koos oma vaikereeglit rakendamine.

Allpool valimi kehtib võrgu turberühma sisaldavad alamvõrgu:

    Get-AzureNetworkSecurityGroup -Name "testNSGExample" | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName 'testVNet' -SubnetName 'Subnet-1'
    
Seetõttu on turvalisuse reeglid, mis väline juurdepääs, lubades VNet sisemine juurdepääsu kogumist.

![Võrgu turvalisus Vaikereeglit][DefaultNetworkSecurityRules]


## <a name="getting-started"></a>Alustamine
Kõik artiklid ja kuidas-'s rakenduse teenuse keskkonnas on saadaval [seletusfail rakenduse teenuse keskkonna jaoks](../app-service/app-service-app-service-environments-readme.md).

Alustamine rakenduse teenuse keskkonnas, lugege teemat [rakenduse teenuse keskkonna tutvustus][IntroToAppServiceEnvironment]

Ümber juhtimine sissetulev liiklus rakenduse teenuse keskkonna leiate teemast [Sissetulev liiklus keskkonnas rakenduse teenuse juhtimine][ControlInboundASE]

Azure'i rakendust Service platform kohta leiate lisateavet teemast [Azure rakenduse teenuse][AzureAppService].

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]
 

<!-- LINKS -->
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[ControlInboundTraffic]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[SiteToSite]: https://azure.microsoft.com/documentation/articles/vpn-gateway-site-to-site-create/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
[NetworkAccessControlLists]: https://azure.microsoft.com/documentation/articles/virtual-networks-acl/
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[IntroToAppServiceEnvironment]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/ 
[ControlInboundASE]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/ 

<!-- IMAGES -->
[SqlServerEndpoint]: ./media/app-service-app-service-environment-securely-connecting-to-backend-resources/SqlServerEndpoint01.png
[NetworkAccessControlListExample]: ./media/app-service-app-service-environment-securely-connecting-to-backend-resources/NetworkAcl01.png
[DefaultNetworkSecurityRules]: ./media/app-service-app-service-environment-securely-connecting-to-backend-resources/DefaultNetworkSecurityRules01.png 
