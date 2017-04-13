<properties 
    pageTitle="Võrgu arhitektuur ülevaade rakenduse teenuse keskkonnas" 
    description="Võrgu topoloogia ofApp teenuse keskkonnas arhitektuuri ülevaade." 
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

# <a name="network-architecture-overview-of-app-service-environments"></a>Võrgu arhitektuur ülevaade rakenduse teenuse keskkonnas

## <a name="introduction"></a>Sissejuhatus ##
Rakenduse teenuse keskkonnas luuakse alati sees alamvõrgu [virtuaalse võrgu] [ virtualnetwork] -keskkonnas rakenduse teenuse rakendusi ei saa suhelda paikneva sama virtuaalse võrgu topoloogia privaatne lõpp-punktid.  Kuna kliendid võivad lukustada oma virtuaaltöölaua infrastruktuuri osad, on oluline mõista tüüpi võrgu toimuvate rakenduse teenuse keskkond teatise alusel.

## <a name="general-network-flow"></a>Üldine võrgu meilivoo ##
 
Kui rakendus teenuse keskkonnas (ASE) kasutab avalik virtuaalse IP-aadress (VIP) rakendused, saabub kogu sissetulev liiklus selle avaliku VIP.  See hõlmab HTTP ja HTTPS-liikluse rakendused, samuti muud liikluse FTP, Kaug silumine funktsioonid ja Azure haldamise toiminguid.  Täieliku loendi teatud pordid (nii kohustuslike ning valikuliste) saadaolevate avaliku VIP kohta leiate artiklist kontrollida [Sissetulev liiklus] [ controllinginboundtraffic] ka rakenduse teenuse keskkonnas. 

Rakenduse teenuse keskkonnas toetavad ka töötava rakendused, mis on seotud ainult virtuaalse võrgu sisemine aadress, nimetatakse ka aadressi ILB (sisemine laadi koormusetasakaalustusteenuse).  Klõpsake mõnda ILB lubatud ASE, HTTP ja HTTPS liikluse rakendused, samuti Kaug silumine kõned saabuvad ILB aadress.  Jaoks kõige levinum ILB-ASE, FTP/FTPS liikluse saabuvad ka ILB aadress.  Kuid Azure haldustoimingute on endiselt flow avaliku VIP, mis ILB 454/455 pordid lubatud ASE.

Alloleval joonisel on näidatud ülevaate erinevate sissetulevate ja väljaminevate võrgu puhul rakenduse teenuse keskkonnas, kus rakendused on seotud avalik virtuaalse IP-aadress.

![Üldine võrgu puhul][GeneralNetworkFlows]

Rakenduse teenuse keskkonnas saab suhelda mitmesuguseid privaatne kliendi lõpp-punktid.  Näiteks rakenduse teenuse keskkonnas rakendusi saate luua ühenduse andmebaasi server töötab IaaS virtuaalmasinates sama virtuaalse võrgu topoloogias.

>[AZURE.IMPORTANT] Vaadates võrgu diagramm, "Muu arvutada ressursside" on juurutatud erinevate alamvõrgu rakenduse teenuse keskkonnast. Ressursside sama alamvõrgu koos ASE juurutamine blokeerib ASE ühenduvust nendele ressurssidele (välja arvatud teatud sisene-ASE marsruutimine). Võtta kasutusele erinevaid alamvõrgu hoopis (sama VNET). Rakenduse teenuse keskkonnas siis võimalik ühendust luua. Täiendavaid konfiguratsiooni on vaja.

Rakenduse teenuse keskkonnas suhelda ja Sql DB Azure'i salvestusruumi haldamine ja opsüsteem keskkonnas rakenduse teenuse vajalikud ressursid.  Mõned SQL-i ja salvestusruumi ressursse, mis rakenduse teenuse keskkonnas suhtleb asuvad sama piirkonna rakenduse teenuse keskkonnas, kuigi teised asuvad remote Azure.  Selle tulemusena väljaminev Interneti-ühendus on alati vaja rakenduse teenuse keskkonnas õigesti. 

Kuna keskkonnas rakenduse teenus on juurutatud on alamvõrku, saab määrata sissetulev liiklus alamvõrgu võrgu turberühmad.  Kohta, kuidas määrata sissetulev liiklus rakenduse keskkonda, vt [artiklist][controllinginboundtraffic].

Jaoks lubada väljaminev Interneti-ühendus on rakenduse keskkond kohta leiate järgmistest artiklitest [Teekonna]töötamise kohta[ExpressRoute].  Sama lähenemisviisi kirjeldatud artiklis rakendab töötamisel saidilt ühenduvuse ja abil sunnitud tunneling.

## <a name="outbound-network-addresses"></a>Väljamineva meili aadressidele ##
Kui rakendus teenuse keskkonnas teeb väljaminevad kõned, IP-aadress on alati seostatud väljaminevad kõned.  Teatud IP-aadress, mida kasutatakse sõltub sellest, kas seda nimetatakse lõpp-punkti asub virtuaalse võrgu topoloogia, või virtuaalse võrgu topoloogia väljaspool.

Kui kutsutud lõpp-punkti on **väljaspool** virtuaalse võrgu topoloogia, kasutatakse Väljamineva meili aadress (ehk väljaminev NAT aadress) on rakenduse teenuse keskkonna avaliku VIP.  See aadress võib leida portaali kasutajaliidese atribuudid blade rakenduse keskkonnas.
 
![Väljaminevate IP-aadress][OutboundIPAddress]

See aadress võib määrata ka praeguseks, mis on avalik VIP ainult rakenduse teenuse keskkonnas rakenduse loomine ja seejärel läbimiseks on *nslookup* rakenduse aadressi. Tulemuseks IP-aadress on nii avaliku VIP kui ka rakenduse keskkonna väljaminev NAT aadress.

Kui **sees** virtuaalse võrgu topoloogia on kutsutud lõpp-punkti, Väljamineva meili aadressi helistaja rakendus on üksikute Arvuta ressursi rakendus töötab sisemine IP-aadress.  Kuid ei ole püsivate vastenduse võrgu virtuaalse sise IP-aadresside rakenduste.  Rakendusi saate liikuda üle erinevate Arvuta materjale ja ressursse rakenduse teenuse keskkonnas saate muuta skaleerimist tegevuse tõttu saadaval Arvuta hulk.

Kuna keskkonnas teenuse rakendus asub alati sees on alamvõrgu, on tagatud alamvõrgu vahemikus CIDR Arvuta ressursi rakendus töötab sisemine IP-aadress jääb alati olema.  Seetõttu, kui muud lõpp-punktid virtuaalse võrgustikus juurdepääs turvamiseks kasutatakse kohandatud ACL-ID või võrgu turberühmad, rakenduse teenuse keskkond, mis sisaldavad alamvõrgu vahemiku peab juurdepääsu.

Järgmisel joonisel on esitatud need mõisted täpsemalt:

![Väljamineva meili aadressidele][OutboundNetworkAddresses]

Ülaltoodud joonisel:

- Kuna avaliku VIP rakenduse teenuse keskkonna 192.23.1.2, mis on kasutatud kõned "Internet" lõpp-punktid tegemisel väljaminevate IP-aadress.
- CIDR lahtrivahemik, mis sisaldavad alamvõrgu rakenduse teenuse keskkonnas on 10.0.1.0/26.  Muud lõpp-punktid sees sama virtuaalse võrgu taristule näevad kõned rakendustest pärit kuskil selles vahemikus aadress.

## <a name="calls-between-app-service-environments"></a>Kõnede vahel rakenduse teenuse keskkonnas ##
Keerukamaid stsenaarium võib ilmneda juhul, kui mitu rakenduse teenuse keskkonnas sama virtuaalse võrgu juurutamine ja Väljamineva meili ühe rakenduse teenuse keskkonna teise rakenduse teenuse keskkonnas kõnede tegemine.  Järgmist tüüpi cross rakenduse teenuse keskkonna kõned käsitletakse ka "Internet" kõned.

Järgmisel joonisel on kujutatud kihiti olevad arhitektuur rakendustega ühe rakenduse teenuse keskkonda (nt "Esiuksest" veebirakenduste) rakenduste kutsub teist rakenduse teenuse keskkonnas (nt ettevõttesisese tagaandmebaas API rakendused mõeldud ei pääse Interneti kaudu). 

![Kõnede vahel rakenduse teenuse keskkonnas][CallsBetweenAppServiceEnvironments] 

Ülaltoodud näites on rakenduse teenuse keskkonna "ASE ühe" 192.23.1.2 väljaminevate IP-aadress.  Kui rakendus töötab see käsitleda rakenduse teenuse keskkonnas saab muudab Väljaminev kõne rakendusse töötab teine rakendus teenuse keskkonnas ("ASE kaks"), mis asub virtuaalse samasse võrku, väljaminevate kõnede tegemine "Internet".  Selle tulemusena saabuvate teine võrguliiklust rakenduse teenuse keskkond, mis kuvatakse need pärinevad 192.23.1.2 (st ei alamvõrgu aadress vahemiku esimene rakenduse teenuse keskkonna).

Ehkki kõned eri rakenduse teenuse keskkondades käsitletakse "Internet" helistada, kui nii rakenduse teenuse keskkondades asuvad Azure piirkonna võrguliiklust on jäävad piirkondliku Azure võrgus ja mitte füüsilise meilivoo avaliku Interneti kaudu.  Selle tulemusena saate võrgu turberühma alamvõrgu teine rakendus teenuse keskkonna ainult lubama sissetulevate kõnede esimese rakenduse teenuse keskkonnast (mille väljaminevate IP-aadress on 192.23.1.2), tagades seega turvaline side rakenduse teenuse keskkondades.

## <a name="additional-links-and-information"></a>Asjakohaste linkidega ja teave ##
Kõik artiklid ja kuidas-'s rakenduse teenuse keskkonnas on saadaval [seletusfail rakenduse teenuse keskkonna jaoks](../app-service/app-service-app-service-environments-readme.md).

Üksikasjadega saate tutvuda lehel sissetulev rakenduse teenuse keskkonnas kasutatavate portide ja võrgu turberühmad abil saate reguleerida sissetulev liiklus on saadaval [siin][controllinginboundtraffic].

Üksikasjad kasutaja määratletud marsruudib andmiseks Väljamineva meili rakenduse teenuse keskkondades on saadaval selles [artiklis][ExpressRoute]. 


<!-- LINKS -->
[virtualnetwork]: http://azure.microsoft.com/services/virtual-network/
[controllinginboundtraffic]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[ExpressRoute]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-configuration-expressroute/

<!-- IMAGES -->
[GeneralNetworkFlows]: ./media/app-service-app-service-environment-network-architecture-overview/NetworkOverview-1.png
[OutboundIPAddress]: ./media/app-service-app-service-environment-network-architecture-overview/OutboundIPAddress-1.png
[OutboundNetworkAddresses]: ./media/app-service-app-service-environment-network-architecture-overview/OutboundNetworkAddresses-1.png
[CallsBetweenAppServiceEnvironments]: ./media/app-service-app-service-environment-network-architecture-overview/CallsBetweenEnvironments-1.png

