<properties 
    pageTitle="Võrgu konfiguratsiooni üksikasjad teekonna töötamiseks" 
    description="Võrgu konfiguratsiooni üksikasjad töötavad rakenduse teenuse keskkonnas virtuaalse võrguga ühendatud mõne ExpressRoute ringi." 
    services="app-service" 
    documentationCenter="" 
    authors="stefsch" 
    manager="nirma" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/14/2016" 
    ms.author="stefsch"/>   

# <a name="network-configuration-details-for-app-service-environments-with-expressroute"></a>Võrgu konfiguratsiooni üksikasjad rakenduse teenuse ExpressRoute keskkonnad 

## <a name="overview"></a>Ülevaade ##
Kliendid, saate luua ühenduse mõne [Azure'i ExpressRoute] [ ExpressRoute] ringi virtuaalse võrgu infrastruktuuri, seega ulatub oma kohapealse võrgu Azure.  Teenuse keskkonnas rakenduse saab luua alamvõrgu [virtuaalse võrgu] [ virtualnetwork] taristu.  Rakendused rakendus teenuse keskkond, mis töötab seejärel luua turvalist ühendust tagaandmebaas juurde puuetega inimestele juurdepääsetavate ainult ExpressRoute kaudu.  

Teenuse keskkonnas rakenduse saab luua rakenduses **mõlemal** on Azure ressursihaldur virtuaalse võrgu, **või** klassikaline mudel virtuaalse võrgu.  Tehtud muudatuse tehtud juuni 2016, kus praeguseks saab kasutada ka nüüd üheks virtuaalse võrke, mida kasutada avaliku aadresside vahemikud või RFC1918 aadress tühikute (st Privaatne aadressid). 

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="required-network-connectivity"></a>Nõutav võrguühendus ##
On võrgu Ühenduvus nõuded rakenduse teenuse keskkonnas, mis pole võimalik algselt täita virtuaalse võrku ühendatud mõne ExpressRoute.  Rakenduse teenuse keskkonnas nõuavad selleks, et toimida järgmist:


-  Väljamineva võrguühendus Azure Storage lõpp-punktid worldwide kohta nii pordid 80 ja 443.  See hõlmab asub sama piirkonna rakenduse teenuse keskkonnas, samuti salvestusruumi lõpp-punktid asub **teiste** Azure'i piirkondade lõpp-punktid.  Azure'i salvestusruumi lõpp-punktid lahendada järgmised DNS-i administraatordomeenid: *table.core.windows.net*, *blob.core.windows.net*, *queue.core.windows.net* ja *file.core.windows.net*.  
-  Azure'i failid teenusesse port 445 väljaminev võrguühendus.
-  Väljamineva võrguühendus asuvad rakenduse teenuse keskkonnas sama piirkonna DB SQL-i lõpp-punktid.  DB SQL-i lõpp-punktid lahendamiseks järgmist domeeni all: *database.windows.net*.  Selleks, et avada pordid 1433 11000-11999 ja 14000-14999 juurdepääsu.  Lisateabe saamiseks lugege [artiklit SQL-i andmebaasi V12 pordi kasutamise kohta](../sql-database/sql-database-develop-direct-route-ports-adonet-v12.md).
-  Väljamineva võrguühendus Azure halduse lennuk lõpp-punktid (ASM nii ARM lõpp-punktid).  See hõlmab nii *management.core.windows.net* ja *management.azure.com*Väljamineva meili Ühenduvus. 
-  Väljamineva võrguühendus *ocsp.msocsp.com*, *mscrl.microsoft.com* ja *crl.microsoft.com*.  See on vajalik toeta SSL-i funktsioone.
-  DNS-i konfigureerimine virtuaalse võrgu jaoks peab olema võimalik lahendamine kõik lõpp-punktid ja domeenid mainitud varasemad punktid.  Kui nende lõpp-punktid ei lahene, rakenduse teenuse keskkonna loomise katsed ei õnnestu ja olemasoleva rakenduse teenuse keskkonnas on märgitud nii vigane.
-  Väljamineva pordi 53 on nõutavad DNS-serverid suhtlemiseks.
-  Kohandatud DNS-i serveri korral teises otsas VPN-lüüsi DNS-i server peab olema eesandmebaasis juurdepääsetavad rakenduse teenuse keskkond, mis sisaldavad alamvõrgu. 
-  Väljamineva võrguteed ei saa läbida sisemise ettevõtte puhverserverite, samuti ei saa Jõusta kohapealse tunneled.  Efektiivse NAT aadressi väljaminevaid võrguliiklust tehes vahetatakse rakenduse teenuse keskkonnas.  Rakenduse teenuse keskkonna väljaminevaid võrguliiklust NAT aadressi muutmine põhjustab ühenduvuse tõrkeid paljudele ülaltoodud lõpp-punktid.  Selle tulemuseks on nurjunud rakenduse teenuse keskkonna loomise katsed nagu varem terve rakenduse keskkonnas on märgitud nii vigane.  
-  Sissetulev vajalikud pordid juurdepääsu jaoks rakenduse teenuse keskkonnas lubama selles [artiklis]kirjeldatud[requiredports].

DNS-i nõuded saate täidetud, tagades kehtiv DNS-i taristu on konfigureeritud ja virtuaalse võrgu säilitada.  Kui mingil põhjusel DNS-i konfigureerimine muudetakse pärast keskkonnas rakenduse teenus on loodud, saate arendajate jõustada keskkonnas rakenduse teenuse hooleks uue DNS-i konfigureerimine.  Rakenduse teenuse keskkonna haldamise tera [Azure portaali] ülaosas käivitamise jooksva keskkonna taaskäivitage arvuti "Uuesti" ikooniga[ NewPortal] põhjustab keskkonna hooleks uue DNS-i konfigureerimine.

Sissetuleva võrgu juurdepääsu nõudeid täita konfigureerimisega [võrgu turberühma] [ NetworkSecurityGroups] rakenduse keskkonna alamvõrgu vaja juurdepääsu lubamiseks, nagu on kirjeldatud selles [artiklis][requiredports].

## <a name="enabling-outbound-network-connectivity-for-an-app-service-environment"></a>Rakenduse teenuse keskkonnas väljaminev võrguühenduse lubamine##
Vaikimisi vastloodud ExpressRoute ringi reklaamib vaikimisi marsruudi, mis võimaldab väljaminev Interneti-ühendus.  Selle konfiguratsiooni keskkonnas rakenduse teenuse saab ühendada muude Azure lõpp-punktid.

Levinud kliendi konfiguratsiooni on siiski määratleda oma vaikimisi protsessi (0.0.0.0/0), mis jõustab hoopis liikumist kohapealse väljaminevate Interneti-liikluse.  See liiklust alati piirid rakenduse teenuse keskkonnas, kuna väljaminev liiklus on kas blokeeritud kohapeal või NAT sooviksite tundmatu kogum, mis pole enam töötamine erinevate Azure lõpp-punktid.

Lahendus on määratleda alamvõrgu, mis sisaldab rakenduse teenuse keskkonnas (vähemalt ühe) kasutaja määratletud marsruudib (UDRs).  Mõne UDR määratleb alamvõrgu kohased marsruudib, mis kuvatakse au asemel vaikimisi marsruutimiseks.

Võimaluse korral on soovitatav kasutada järgmist konfiguratsiooni:

- ExpressRoute konfiguratsiooni reklaamib 0.0.0.0/0 ja vaikimisi Jõusta tunnelite kogu väljaminev liiklus kohapealse.
- UDR, mis on rakendatud rakenduse teenuse keskkond, mis sisaldavad alamvõrgu määratleb 0.0.0.0/0 järgmise sõnumihüppe kohta, pärast tüüpi Internet (kaugemal allapoole selles artiklis on näide selle).

Ühendatud need juhised on, et alamvõrgu tase UDR on ülimuslik ExpressRoute sunnitud tunneling, tagades seega rakenduse teenuse keskkonnast väljaminev Interneti-ühendus.

> [AZURE.IMPORTANT] Marsruudib, mis on määratletud UDR **peab** olema seotud ülimuslik mis tahes marsruudib reklaamida ExpressRoute konfiguratsioon.  Järgmises näites kasutatakse laialdane 0.0.0.0/0 aadress vahemiku ja sellisena saab potentsiaalselt kogemata tühistada marsruutimiseks reklaamide abil täpsemate aadresside vahemikud.
>
>Rakenduse teenuse keskkonnas ei ole toetatud ExpressRoute konfiguratsioone selle **rist reklaamida marsruudib avaliku silmitsemine tee privaatne silmitsemine tee kaudu**.  ExpressRoute konfiguratsioone, mis on avalik silmitsemine konfigureeritud, saate marsruutimiseks reklaamide Microsofti suure hulga Microsoft Azure'i IP-aadresside vahemikud.  Kui nende aadressid on rist-reklaamida privaatne silmitsemine tee, seetõttu on, et kõigi väljaminevate võrgu pakette rakenduse keskkonna alamvõrgu Jõusta tunneled kliendi kohapealse võrgu taristule.  Praegu ei toetata selle võrgu meilivoo rakenduse teenuse keskkonnas.  Selle probleemi lahendada on rist-reklaami marsruudib avaliku silmitsemine tee privaatne silmitsemine tee lõpetada.

Kasutaja määratletud marsruudib kohta on saadaval sellest [Ülevaade][UDROverview].  

Loomine ja konfigureerimine kasutaja määratletud marsruudib üksikasjad on saadaval sellest, [Kuidas soovite juhend][UDRHowTo].

## <a name="example-udr-configuration-for-an-app-service-environment"></a>Näide UDR keskkonnas rakendust Service konfigureerimine ##

**Eeltingimused**

1. Installige Azure'i PowerShelli [Azure'i allalaadimislehelt] [ AzureDownloads] (kuupäevaga juuni 2015 või uuem versioon).  Jaotises "Käsurea Tööriistad" on "Install" lingi "Windows Powershell" installitavad uusima PowerShelli cmdlet-käsud.

2. Soovitatav on kordumatu alamvõrgu on loodud kasutamiseks ainult rakenduse teenuse keskkond.  See tagab, et avab UDRs, mis on rakendatud alamvõrgu väljaminev liiklus ainult rakenduse teenuse keskkonnas.
3. **Tähtis**: juurutamine rakenduse teenuse keskkonnas kuni **pärast** ülaltoodud konfiguratsiooni järgmist.  See tagab väljaminev võrguühendus on saadaval enne kui proovite teenuse keskkonnas rakenduse juurutamine.

**Samm 1: Nimega marsruutimiseks tabeli loomine**

Järgmised koodilõigu loob marsruutimiseks tabeli nimega "DirectInternetRouteTable" piirkonnas Lääne meile Azure:

    New-AzureRouteTable -Name 'DirectInternetRouteTable' -Location uswest

**Samm 2: Looge üks või mitu marsruudib marsruudi tabelis**

Peate lisada ühe või mitme tabeliga marsruutimiseks selleks, et väljaminev Interneti-ühendus.  

Soovitatud lähenemisviisi konfigureerida väljaminev juurdepääs Internetile on määratleda protsessi jaoks 0.0.0.0/0, nagu allpool näidatud.
  
    Get-AzureRouteTable -Name 'DirectInternetRouteTable' | Set-AzureRoute -RouteName 'Direct Internet Range 0' -AddressPrefix 0.0.0.0/0 -NextHopType Internet

Pidage meeles, et selle 0.0.0.0/0 on laialdane aadress vahemikus ja sellisena, alistab täpsemale aadresside vahemikud, on ExpressRoute reklaamida järgi.  Uuesti kordamiseks varasema soovituse, kasutada mõne UDR ja 0.0.0.0/0 marsruudi ExressRoute konfiguratsiooni, et ainult reklaamib 0.0.0.0/0 kasutada ka koos. 

Teise võimalusena saate alla laadida CIDR vahemike kasutavad Azure'i täielik ja värskendatud loendi.  XML-faili, mis sisaldab kõiki Azure'i IP-aadresside vahemikud on saadaval [Microsofti allalaadimiskeskusest][DownloadCenterAddressRanges].  

Kuigi tähele, et need vahemikud muuta aja jooksul, seega mis nõuavad kasutaja määratletud marsruudib hoida sünkroonis perioodiliste käsitsi värskendamine.  Ka, kuna seal on vaikimisi ülempiir 100 marsruudib sisse ühe UDR, peate "kokkuvõte" mahutamiseks piires 100 marsruutimiseks Azure'i IP-aadressid pidage silmas, et UDR määratletud marsruudib peavad olema täpsem kui lennuliini reklaamida oma ExpressRoute järgi.  


**Samm 3: Seostada rakenduse teenuse keskkond, mis sisaldavad alamvõrgu marsruutimiseks tabel**

Viimase konfiguratsiooni sammuna tuleb seostada marsruutimiseks tabeli alamvõrgu kus juurutatakse rakenduse teenuse keskkonnas.  Järgmine käsk seob "DirectInternetRouteTable" "ASESubnet", mis sisaldab lõpuks keskkonnas rakenduse teenuse.

    Set-AzureSubnetRouteTable -VirtualNetworkName 'YourVirtualNetworkNameHere' -SubnetName 'ASESubnet' -RouteTableName 'DirectInternetRouteTable'


**Samm 4: Lõplik järgmiselt.**

Kui marsruutimiseks tabel on seotud alamvõrgu, on soovitatav esmalt testimiseks ja kinnitage soovitud efekti.  Näiteks juurutamine virtuaalse masina alamvõrgu sisse ja veenduge, et:


- Azure'i ja Azure'i lõpp-punktid väljamineva liikluse eelpool **selle artikli pääseb vabalt ExpressRoute ringi ette** .  See on väga oluline kinnitamiseks seda käitumist, kuna kui väljaminev liiklus alamvõrgu on endiselt sunnitud tunneled asutusesiseselt, rakenduse teenuse keskkonna loomine alati nurjub. 
- Lõpp-punktide jaoks DNS-i otsingud eelpool on kõik õigesti üksusekonfliktide. 

Kui eespool kirjeldatud toimingud on kinnitatud, peate kustutada virtuaalse masina, kuna alamvõrgu peab olema "tühi" rakenduse teenuse keskkond, mis on loodud ajal.
 
Jätkake loomise rakendus teenuse keskkonnas!

## <a name="getting-started"></a>Alustamine
Kõik artiklid ja kuidas-'s rakenduse teenuse keskkonnas on saadaval [seletusfail rakenduse teenuse keskkonna jaoks](../app-service/app-service-app-service-environments-readme.md).

Alustamine rakenduse teenuse keskkonnas, lugege teemat [rakenduse teenuse keskkonna tutvustus][IntroToAppServiceEnvironment]

Azure'i rakendust Service platform kohta leiate lisateavet teemast [Azure rakenduse teenuse][AzureAppService].

<!-- LINKS -->
[virtualnetwork]: http://azure.microsoft.com/services/virtual-network/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
[requiredports]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[NetworkSecurityGroups]: http://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[UDROverview]: http://azure.microsoft.com/documentation/articles/virtual-networks-udr-overview/
[UDRHowTo]: http://azure.microsoft.com/documentation/articles/virtual-networks-udr-how-to/
[HowToCreateAnAppServiceEnvironment]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[AzureDownloads]: http://azure.microsoft.com/en-us/downloads/ 
[DownloadCenterAddressRanges]: http://www.microsoft.com/download/details.aspx?id=41653  
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[IntroToAppServiceEnvironment]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[NewPortal]:  https://portal.azure.com
 

<!-- IMAGES -->
