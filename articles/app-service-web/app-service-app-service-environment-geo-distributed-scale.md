<properties 
    pageTitle="Geo jaotatud skaala rakenduse teenuse keskkonnas" 
    description="Saate teada, kuidas horisontaalselt mastaapimiseks rakenduste kaudu geo-jaotuse liikluse juhataja ja rakenduse teenuse keskkonnas." 
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
    ms.date="09/07/2016" 
    ms.author="stefsch"/>   

# <a name="geo-distributed-scale-with-app-service-environments"></a>Geo jaotatud skaala rakenduse teenuse keskkonnas

## <a name="overview"></a>Ülevaade ##
Rakenduse stsenaariumid, mis nõuavad väga kõrge skaala ei ületa Arvuta Ressursi võimsus saadaval rakenduse ühekordset kasutamist.  Hääletamine rakendused, spordi sündmused ja televisioonis meelelahutusürituste näited kõik stsenaariumid, mis nõuavad ülisuure skaala. Suure ulatuse nõuded saate täidetud horisontaalselt skaala läbi rakendusi, siis koos mitme rakendusejuurutuste tehtud ühest, samuti piirkondade, tegelema äärmiselt laadi nõuetele.

Rakenduse teenuse keskkonnas on ka optimaalne platvormi horisontaalne skaala välja.  Üks kord rakenduse teenuse keskkonnas konfigureerimine on valitud teadaolevad taotluse määr toetavad arendajate saate juurutada täiendavad rakenduse teenuse keskkonnas "küpsise eraldaja" mood laadi soovitud maksimaalne maht saavutamiseks.

Näiteks Oletame, et rakendus töötab ka rakenduse teenuse keskkonna konfiguratsioon on testitud käsitlema 20K taotlusi sekundis (RPS).  Kui soovitud tippväärtus laadi maht on 100K RPS, siis viis (5) rakenduse teenuse keskkonnas saab luua ja tagada taotluse võite hakkama plaanitud koormus konfigureeritud.

Kuna kliendid tavaliselt juurde rakenduste kaudu kohandatud (või edevus) domeen, vaja arendajate levitada rakendusetaotluste kõigis rakenduse teenuse keskkonna eksemplarid.  Hea viis selleks on kohandatud domeeni kasutamine on [Azure liikluse haldur profiili]lahendamiseks[AzureTrafficManagerProfile].  Profiili liikluse haldur saab konfigureerida osutama üldse üksikute rakenduse teenuse keskkonnas.  Liikluse haldur hakkama jaotavad kliendid automaatselt kõigis rakenduse teenuse keskkonnas tasakaalustamiseks liikluse haldur profiili sätete laadi põhjal.  Seda moodust töötab sõltumata sellest, kas kõik rakenduse teenuse keskkonnas on juurutatud ühe Azure piirkonnas või juurutatud kogu maailmas mitme Azure'i piirkondade lõikes.

Lisaks, kuna kliendid juurde rakenduste kaudu edevus domeeni, kliendid on teadlikud rakenduse teenuse keskkonnas rakendus töötab arv.  Selle tulemusena arendajate saate kiiresti ja kerge vaevaga lisamine ja eemaldamine, rakenduse teenuse keskkonnas täheldatud liikluse laadi põhjal.

Kontseptuaalne alloleval joonisel on kujutatud horisontaalselt mastaabitud välja kõigis kolmes rakenduse keskkonnas on ühest rakendus.

![Kontseptuaalne arhitektuur][ConceptualArchitecture] 

Selles teemas ülejäänud tutvustatakse seotud jaotatud topoloogia valimi rakenduse kasutamise mitme rakenduse teenuse keskkonnas häälestamise juhiseid.

## <a name="planning-the-topology"></a>Topoloogia kavandamine ##
Enne hoone läbi jaotatud rakenduse andmed väiksemates vähem ruumi, aitab see on mõned andmeühikuga teabe ette valmistada.

- **Kohandatud domeeni rakenduse:**  Mis on kasutavate klientide juurde pääseda rakenduse kohandatud domeeninime?  Kohandatud domeeni nimi on valimi rakenduse *www.scalableasedemo.com*
- **Liikluse haldur Domeen:**  Domeeninimi peab on [Azure liikluse haldur profiili]loomisel valida[AzureTrafficManagerProfile].  Sellise nimega kombineeritakse *trafficmanager.net* järelliite registreerida domeeni kirjet, mida hallatakse liikluse haldur.  Valitud nimi on valimi rakenduse, *scalable ase demo*.  Selle tulemusena hallatakse liikluse haldur täielik domeeninimi on *scalable ase demo.trafficmanager.net*.
- **Strateegia skaleerimist rakenduse andmed väiksemates vähem ruumi:**  Rakenduse andmed väiksemates vähem ruumi jagatakse üle mitme rakenduse teenuse keskkonnas ühe piirkonna?  Mitme piirkonna?  Mix-ja-vaste mõlema lähenemisel?  Otsus põhinema ootustele, kus kliendi liiklus pärinevad, samuti kui ka ülejäänud rakenduse toetavad tagaandmebaas taristu saab skaala.  Näiteks 100% kodakondsuseta rakendusega, rakenduse saab oluliselt mastaabitud abil mitme rakenduse teenuse keskkonnas kombinatsiooni Azure piirkonna, korrutatuna rakenduse teenuse keskkonnas juurutatud mitme Azure'i piirkondade lõikes.  15 + avaliku Azure piirkondades saadaval valida, tõeliselt koostada klientidele kogu maailmas hyper skaala rakenduse andmed väiksemates vähem ruumi.  Valimi rakenduse kasutada seda artiklit, kolm rakenduse teenuse keskkonnas on loodud ühe Azure piirkonna (Lõuna keskse US).
- **Failinimede mess rakenduse teenuse keskkonnas:**  Iga rakenduse teenuse keskkonna jaoks on vaja kordumatu nimi.  Lisaks ühe või mitme rakenduse keskkonnas on kasulik nimetamistava iga rakenduse teenuse keskkonna tuvastamiseks.  Valimi rakenduse kasutati lihtsa nimereeglistikku.  Kolm rakenduse teenuse keskkonnas nimed on *fe1ase*, *fe2ase*ja *fe3ase*.
- **Failinimede mess rakendused:**  Kuna kasutatakse mitu rakenduse eksemplari, on vaja iga eksemplari juurutatud rakenduse nimi.  Ühe vähe tuntud, kuid väga mugav funktsioon rakenduse teenuse keskkonnas on, et sama rakenduse nime saab kasutada üle mitme rakenduse teenuse keskkonnas.  Kuna iga rakenduse teenuse keskkond on kordumatu domeeni järelliite, saate valida arendajate uuesti kasutada iga keskkonnas täpselt sama rakenduse nimi.  Näiteks võib arendaja on rakenduste nimega järgmiselt: *myapp.foo1.p.azurewebsites.net*, *myapp.foo2.p.azurewebsites.net*, *myapp.foo3.p.azurewebsites.net*jne.  Valimi rakenduse küll iga rakenduse eksemplari on kordumatu nimi.  Rakenduse eksemplari nimed on *webfrontend1*, *webfrontend2*ja *webfrontend3*.


## <a name="setting-up-the-traffic-manager-profile"></a>Kuidas häälestada liikluse haldur profiil ##
Kui mitu rakenduse eksemplari on juurutatud mitme rakenduse teenuse keskkonnas, saate üksikuid rakenduse eksemplari registreeritud liikluse haldur.  Valimi rakenduse liikluse haldur profiili on vaja *scalable ase demo.trafficmanager.net* , mida saate marsruutida klientide mis tahes juurutatud rakenduse järgmistel juhtudel:

- **webfrontend1.fe1ase.p.azurewebsites.net:**  Esimese rakenduse teenuse keskkonnale juurutatud valimi rakenduse eksemplari.
- **webfrontend2.fe2ase.p.azurewebsites.net:**  Teine rakendus teenuse keskkonnale juurutatud valimi rakenduse eksemplari.
- **webfrontend3.fe3ase.p.azurewebsites.net:**  Kolmanda rakenduse teenuse keskkonnale juurutatud valimi rakenduse eksemplari.

Registreerida mitme Azure'i rakenduse teenuse lõpp-punktid, kõik töötab on **sama** Azure piirkond, on kõige lihtsam viis on PowerShelli [Azure'i halduri liikluse ressursihaldur toetavad][ARMTrafficManager].  

Esimene samm on Azure liikluse haldur profiili loomine.  Alljärgnev kood on kuvatud profiili loomise viisist valimi rakenduse.

    $profile = New-AzureTrafficManagerProfile –Name scalableasedemo -ResourceGroupName yourRGNameHere -TrafficRoutingMethod Weighted -RelativeDnsName scalable-ase-demo -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"

Pange tähele, kuidas *RelativeDnsName* parameeter on seatud *scalable ase demo*.  See on, kuidas domeeni nimi *scalable ase demo.trafficmanager.net* on loodud ja seostatud liikluse haldur profiili.

Parameetri *TrafficRoutingMethod* määratleb laadi tasakaalustamiseks poliitika liikluse haldur on abil saate määrata, kuidas kliendi laadi laiali kõik saadaolevad lõpp-punktid.  Selles näites *kaalutud* valitud meetodit.  Seetõttu klientide taotlusi levinud kõigis suhteline kaalu, mis on seotud iga lõpp-punkti põhjal registreeritud rakenduse lõpp-punktid. 

Loodud profiiliga iga rakenduse eksemplari lisatakse profiilile kohalikke Azure lõpp-punkti.  Alljärgnev kood toob iga esiosa veebirakenduse viite ja lisab seejärel konkreetse rakenduse nimega teel *TargetResourceId* parameetri liikluse haldur lõpp-punkti.


    $webapp1 = Get-AzureRMWebApp -Name webfrontend1
    Add-AzureTrafficManagerEndpointConfig –EndpointName webfrontend1 –TrafficManagerProfile $profile –Type AzureEndpoints -TargetResourceId $webapp1.Id –EndpointStatus Enabled –Weight 10

    $webapp2 = Get-AzureRMWebApp -Name webfrontend2
    Add-AzureTrafficManagerEndpointConfig –EndpointName webfrontend2 –TrafficManagerProfile $profile –Type AzureEndpoints -TargetResourceId $webapp2.Id –EndpointStatus Enabled –Weight 10

    $webapp3 = Get-AzureRMWebApp -Name webfrontend3
    Add-AzureTrafficManagerEndpointConfig –EndpointName webfrontend3 –TrafficManagerProfile $profile –Type AzureEndpoints -TargetResourceId $webapp3.Id –EndpointStatus Enabled –Weight 10
    
    Set-AzureTrafficManagerProfile –TrafficManagerProfile $profile
    
Pange tähele, kuidas on üks *Lisa-AzureTrafficManagerEndpointConfig* iga üksiku rakenduse eksemplari helistada.  PowerShelli käsu iga parameetri *TargetResourceId* viitab üks kolm juurutatud rakenduse eksemplari.  Liikluse haldur profiili kuvatakse laadi laiali registreeritud profiili kõigi kolme lõpp-punktid.

Kõigi kolme lõpp-punktid *paksus* parameetris kasutada sama väärtuse (10).  Selle tulemusena liikluse haldur levitada klientide taotlusi üle kõigi kolme rakenduse aknad suhteliselt ühtlaselt. 


## <a name="pointing-the-apps-custom-domain-at-the-traffic-manager-domain"></a>Osutage rakenduse kohandatud domeeni domeeni liikluse haldur ##
Viimane toiming, mis on vajalikud on kohandatud domeeni rakenduse liikluse haldur domeenil juhtida.  Valimi rakenduse see tähendab, et *www.scalableasedemo.com* suunatud *scalable ase demo.trafficmanager.net*.  Selles etapis tuleb peab domeeniregistraatoriga kohandatud domeeni haldava lõpule viia.  

Oma domeeniregistraatori domeeni halduse tööriista abil on CNAME-kirje loomiseks, mis suunab liikluse haldur domeeni kohandatud domeeni vajadustele.  Alloleval pildil on kujutatud selle CNAME konfiguratsiooni näeb:

![Kohandatud domeeni CNAME][CNAMEforCustomDomain] 

Kuigi ei hõlma selles teemas, pidage meeles, et iga üksiku rakenduse eksemplari, peab olema registreerunud ka kohandatud domeeni.  Muul juhul taotluse muudab rakenduse eksemplari, kui rakendus pole registreeritud rakenduse kohandatud domeeni, taotlus nurjub.  

Selles näites on kohandatud domeeni *www.scalableasedemo.com*ja iga rakenduse eksemplari on seostatud kohandatud domeeni.

![Kohandatud domeeni][CustomDomain] 

Sulgege Azure'i rakendust Service rakenduste kohandatud domeeninime registreerimine, leiate järgmistest artiklitest [registreerimise kohandatud domeenide]kohta[RegisterCustomDomain].

## <a name="trying-out-the-distributed-topology"></a>Proovida jaotatud topoloogia ##
Liikluse juhataja ja DNS-i konfigureerimine seetõttu on et taotlused *www.scalableasedemo.com* saab tehingust kaudu järgmist järjestust.

1. Brauseris või seadme teeb DNS-i otsingu jaoks *www.scalableasedemo.com*
2. Domeeniregistraatori juures kirje CNAME põhjustab DNS otsing ümber suunama Azure'i liikluse haldur.
3. DNS-i ette *scalable ase demo.trafficmanager.net* üks Azure'i liikluse haldur DNS-i serverid suhtes.
4. Laadi tasakaalustamiseks poliitika (varem kasutatud liikluse haldur profiili loomisel *TrafficRoutingMethod* parameeter) põhjal, liikluse haldur valige üks konfigureeritud lõpp-punktid ja naaske selle lõpp-punkti FQDN brauseri või seadme.
5.  Kuna FQDN-i lõpp-punkti URL-i teenuse keskkonnas rakendus töötab rakenduse eksemplari, brauseri või seadme küsib Microsoft Azure'i DNS-i serveri FQDN IP-aadressi lahendamiseks. 
6. Brauseri või seadme saadab HTTP/S päringu IP-aadress.  
7. Taotluse saabub rakenduse eksemplarid, mis töötavad rakenduse teenuse keskkonnas.

Konsooli alloleval pildil on DNS-i otsingu valimi rakenduse kohandatud domeeni edukalt lahendamine rakendus töötab üks kolmest valimi rakenduse teenuse keskkonnas (sel juhul teine kolm rakenduse teenuse keskkondades) näiteks:

![DNS-i otsing][DNSLookup] 

## <a name="additional-links-and-information"></a>Asjakohaste linkidega ja teave ##
Kõik artiklid ja kuidas-'s rakenduse teenuse keskkonnas on saadaval [seletusfail rakenduse teenuse keskkonna jaoks](../app-service/app-service-app-service-environments-readme.md).

PowerShelli [Azure'i halduri liikluse ressursihaldur toetavad]dokumendid[ARMTrafficManager].  

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[AzureTrafficManagerProfile]:  https://azure.microsoft.com/documentation/articles/traffic-manager-manage-profiles/
[ARMTrafficManager]:  https://azure.microsoft.com/documentation/articles/traffic-manager-powershell-arm/
[RegisterCustomDomain]:  https://azure.microsoft.com/en-us/documentation/articles/web-sites-custom-domain-name/


<!-- IMAGES -->
[ConceptualArchitecture]: ./media/app-service-app-service-environment-geo-distributed-scale/ConceptualArchitecture-1.png
[CNAMEforCustomDomain]:  ./media/app-service-app-service-environment-geo-distributed-scale/CNAMECustomDomain-1.png
[DNSLookup]:  ./media/app-service-app-service-environment-geo-distributed-scale/DNSLookup-1.png
[CustomDomain]:  ./media/app-service-app-service-environment-geo-distributed-scale/CustomDomain-1.png 
