<properties 
    pageTitle="Azure'i teatis jaoturi - diagnostika juhised" 
    description="Juhised, kuidas diagnoosida Azure'i teatis jaoturi seotud levinud probleemid." 
    services="notification-hubs" 
    documentationCenter="Mobile" 
    authors="ysxu" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="notification-hubs" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="NA" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="10/03/2016" 
    ms.author="yuaxu"/>

#<a name="azure-notification-hubs---diagnosis-guidelines"></a>Azure'i teatis jaoturi - diagnostika juhised

##<a name="overview"></a>Ülevaade

Üks kõige levinumad küsimused Soovime kuulda Azure'i teatis jaoturi kliendid on Kuidas aru saada, miks nad ei näe oma rakenduse taustväärtus saadetud teatis kuvatakse kliendi seadme – miks ja kuhu jäeti teatised ja lahendusviisid. Selles artiklis me läheb läbi erinevad põhjused, miks teatised võib saada lähevad või ei jõua seadmetes. Käsitleme ka kaudu võimalust, kus saate analüüsida ja põhjus välja selgitada. 

Kõigepealt, on oluline, et aru saada, kuidas Azure'i teatis jaoturi sunnib välja teatised seadmete.
![][0]

Tüüpilised saada teatis meilivoo, sõnum saadetakse kaudu **rakenduse kirjutamata** **Azure'i teatis jaoturi (NH)** omakorda ei mõned töötlemine registreerumine arvestades konfigureeritud siltide ja sildi avaldiste "eesmärke" st kõik registreerimine, kes peavad saama teatise tõuketeatised määratlemiseks klõpsake. Saate neid registreerimise asuvad mõnda või kõiki meie toetatud platvormid - iOS-i, Google, Windowsi, Windows Phone, Kindle ja Baidu Hiina Androidi jaoks. Eesmärkide koostatud NH siis sunnib välja teatised, tükeldamine üle mitme paketi registreerimise seadme platvormi teatud **Push teatise teenuse (PNS)** – nt Apple APNS, GCM Google jne. NH autendib vastav PNS, saate määrata Azure'i klassikaline portaalis lehel konfigureerimine teate jaoturi mandaadi põhjal. PNS saadetakse teatised ja vastav **kliendi seadmed**. See on soovitatav viis esitamine tõuketeatised ja pange tähele, et teate kohaletoimetamine viimasel etapil toimub platvormi PNS ja seadme vahel platvormi. Seetõttu meil on neli põhilist komponendid - *Klient*, *rakenduse kirjutamata*, *Azure'i teatis jaoturi (NH)* ja *Push teatis Services (PNS)* ja mõnda neist võib põhjustada lähevad saada teatisi. Üksikasjalikumat teavet selle arhitektuur on saadaval [Teatis jaoturi ülevaade].

Teatiste tõrge võib juhtuda algse testi/lavastus, etapp, mis võivad viidata mõni konfiguratsiooniprobleem või see võib juhtuda, kui kõigi või osade teatised võib saada kaotatud mõningaid süvitsi rakendus, mis näitab, või sõnumside mustri probleemi valmistamisel. Klõpsake jaotises all käsitleme vahemikus levinud harvem selline, millest võite leida selge ja teised nii palju erinevaid kaotatud teatised stsenaariumid. 

##<a name="azure-notifications-hub-mis-configuration"></a>Azure'i teatised jaoturi vale konfiguratsioon 

Azure'i teatis jaoturi peab autentida ise arendaja rakenduse saama edukalt saada teatisi vastav PNS kontekstis. See on võimalik arendaja konto loomine ja vastav platvorm (Google, Apple, Windows jne) ja seejärel registreerimisel oma rakenduse, kus nad saavad, mida tuleb konfigureerida portaalis teatis jaoturi konfigureerimine jaotises arendaja. Kui pole teatised teevad kaudu, tuleks kõigepealt, et teate jaoturi sobitamine neid nende platvormi teatud arendaja arvesse loodud rakendusega on konfigureeritud õige mandaat. Leiate meie [Kasutamise alustamine õpetused] kasulik minna üle selle protsessi samm-sammult viisil. Siin on mõned levinud valesti konfiguratsiooni.

1. **Üldine**
 
    a) tegemine veenduge, et teie teate jaoturi nimi (ilma Näpu) on sama:

    - Kui teil on klient, registreerimine 
    - Kui te saadate teatised kirjutamata,  
    - Kui olete konfigureerinud PNS mandaadi ja 
    - Mille olete konfigureerinud klient ja selle kirjutamata SAS mandaat. 
        
    (b) muuta kindel, et kasutate õiget SAS konfiguratsiooni stringid klient ja rakenduse taustväärtus. Rusikareegel, kui peate kasutama **DefaultListenSharedAccessSignature** klient ja **DefaultFullSharedAccessSignature** klõpsake rakenduse kirjutamata, (mis annab õiguse peaksid saama teatise saatmine NH)

2. **Apple Push teatise teenuse (APNS) konfigureerimine**
 
    Haldate kaks erinevat jaoturi – üks tootmiseks ja teise testimiseks eesmärk. See tähendab, et te ei kavatse kasutada Liivakasti keskkonnas eraldi jaoturi serti ja kavatsete kasutada eraldi jaoturi valmistamisel serdi üleslaadimine. Proovige erinevat tüüpi tunnistuste sama jaoturi üles laadida, kuna see võib põhjustada teatis tõrkeid rea alla. Kui leiate ise olukorras, kus teil on tahtmatult üleslaaditud erinevat tüüpi serdi sama jaoturi, on soovitatav jaoturi kustutada ja alustada puhtalt lehelt. Kui mingil põhjusel te ei saa kustutada jaoturi siis vähemalt, siis kustutage olemasolev registreerumine keskuse kaudu. 

3. **Google Cloud sõnumside (GCM) konfigureerimine** 

    (a) muuta kindel, et "Google Cloud sõnumside for Android" on lubamine jaotises pilve projekti. 
    
    ![][2]
    
    (b) tehke kindlasti "Serveri võti" ajal saamine mandaatide millist NH kasutatakse GCM autentimiseks. 
    
    ![][3]
    
    c) tehke kindlasti, et olete konfigureerinud "Projekti ID" mis on täiesti arvväärtuste üksus, mis armatuurlaual saate hankida:
    
    ![][1]

##<a name="application-issues"></a>Rakenduste seotud probleemid

1) **Sildid / Tag avaldised**

Kui kasutate segmendi publikule siltide või sildi avaldised, on alati võimalik, et teate saatmisel on pole sihtkoht on leitud sildid/sildi väljendeid määrate saada kõne põhjal. See on parim üle vaadata oma registreerimise tagamaks, et on sildid, mis vastavad kui teatise saatmine ja seejärel kontrollige teatise saamist ainult need registreerimise klientidega. Nt Kui kõik teie registreerimise koos NH olid tehtud öelda sildi "Poliitika" ja saadate teavitus sildiga "Sports", on see pole saadetakse igast seadmest. Keerukate juhul võivad sisaldada sildi avaldisi, kus saate ainult registreeritud "Silt A" või "Sildi B", kuid saatmisel teatised, olete suunatud "Sildi A & & sildi B". Omas diagnoosimine näpunäited allpool olevat jaotist, on võimalust, kus saate vaadata oma registreerimise koos neil on sildid. 

2) **Malli probleemid**

Kui kasutate malli, siis veenduge, et olete pärast juhised, mis on kirjeldatud aadressil [malli juhised]. 

3) **Vigaste registreerimine**

Eeldades, et teate jaoturi on õigesti konfigureeritud ning kõik sildid/sildi avaldised kasutati õigesti tulemuseks kehtiv sihtkohtade teatised vaja saata Otsi, NH käivitub välja mitme töötlemine töölehe paralleelselt - iga paketi kogumisse registreerimise sõnumite saatmiseks. 

> [AZURE.NOTE] Kuna me teha töötlemiseks paralleelselt, me ei garanteeri järjestuses, kus näidatakse teatised. 

Nüüd Azure'i teatised jaoturi on optimeeritud "Enamik veebisaidil üks kord" sõnumi kohaletoimetamise mudel. See tähendab, et me nii, et pole teatised saadetakse rohkem kui üks kord seadme üritavad de kordamise. See registreerimise läbi vaadata ja veenduge, et ainult üks sõnum on saadetud identifikaator kohta enne saatmist sõnum tegelikult PNS. Nagu iga paketi saadetakse PNS, mis omakorda on aktsepteerimise ja kinnitamise registreerimise, on võimalik, et PNS tuvastab tõrke üks või mitu registreerimise partii, tagastab tõrke Azure'i NH ja peatab töötlemise, seetõttu pukseerimine selle paketi täielikult. See kehtib eriti APNs-i, mis kasutab TCP voo protokolli. Kuigi me on optimeeritud veebisaidil kõige rohkem kui kohaletoimetamise sel juhul meie andmebaasi Tõrkuvat registreerimise eemaldamine ja proovige uuesti teate kohaletoimetamine ülejäänud selle partii seadmete jaoks.

Saate nurjunud kohaletoimetamise proovi suhtes registreerimise Azure teatis jaoturi REST API-de kasutamine tõrketeave: [kohta sõnumi telemeetria: Saada teatis sõnumi telemeetria](https://msdn.microsoft.com/library/azure/mt608135.aspx) ja [PNS tagasiside](https://msdn.microsoft.com/library/azure/mt705560.aspx). Lugege teemat [SendRESTExample](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/SendRestExample) näiteks koodi.

##<a name="pns-issues"></a>PNS probleemid

Kui teavitussõnum ei ole esitatud vastav PNS, siis on seadmele teate selle eest. Azure'i teatis jaoturi välja pilt siin on ja on ei kontrolli või kui teate plaanitakse toimetada seade. Kuna platvormi teatis teenused on päris kindel, teatiste tavaliselt jõuda seadmete mõne sekundi PNS. Kui PNS on aga pidurdamise seejärel Azure'i teatis jaoturi rakendada mõne välja strateegia eksponentsiaalse tagasi ja kui PNS jääb kättesaamatu 30 min siis meil poliitika aegumise ja kukutage need sõnumid jäädavalt. 

Kui soovitud PNS üritab teatis, kuid seade on ühenduseta, teate on talletatud PNS piiratud aja jooksul ja seadme toimetada, kui see on saadaval. Ainult ühe kindla rakenduse teates on talletatud. Kui mitme teatised saadetakse, kui seade on ühenduseta, põhjustab iga uue teatise eelneva teate kaotsi. Selline käitumine hoida ainult uusim teate edaspidi coalescing APNs-i märguanded ja ahendamine rakenduses GCM (mis kasutab kokkuvajumine klahvi). Kui kaua aega seadme ühenduseta, hüljatakse kõik teatised, mida talletatakse on seda. Allikas - [APNs-i juhiseid] & [GCM juhised]

Azure'i teatis jaoturi - ei liigu eemaldamisviisiks klahvi kaudu HTTP-päise, kasutades üldise `SendNotification` API (näiteks .NET SDK – `SendNotificationAsync`) mis võtab ka HTTP päised, mille nimega on vastav PNS. 

##<a name="self-diagnose-tips"></a>Omas diagnoosimine näpunäited

Siin uurime erinevaid võimalusi diagnoosimine ja root põhjustada probleemidest teatamise jaoturi:

###<a name="verify-credentials"></a>Veenduge, et identimisteave

1. **PNS arendaja portaal**

    Kinnitage need vastav PNS arendaja portaalis (APNs-i GCM, WNS jne) abil meie [Kasutamise alustamine õpetused].

2. **Azure'i klassikaline portaal**

    Minge vaatama ja vastavad identimisteabe saadud portaalist PNS arendaja menüü konfigureerimine. 

    ![][4]

###<a name="verify-registrations"></a>Registreerimise kinnitamine

1. **Visual Studio**

    Kui kasutate Visual Studio arengu saate ühenduse loomine Microsoft Azure'i ja vaadata ja hallata Azure teenused, sh teatised keskuse kaudu "Server Explorer" hulga. See on kasulik peamiselt arendaja/testimiskeskkonnas. 

    ![][9]

    Saate vaadata ja hallata kõik registreerimise oma keskuses, mis talletatakse kenasti platvormi kohalikke või malli registreerimine, sildid, PNS identifikaator, registreerimise id ja aegumiskuupäeva. Saate redigeerida ka registreerimise pealt - mis on kasulik öelda, kui soovite muuta kõik sildid. 

    ![][8]
 
    > [AZURE.NOTE] Visual Studio funktsioonid registreerimise redigeerimiseks kasutada ainult arendaja katse piiratud arvu registreerimise käigus. Vaja määrata oma registreerimise mitmekaupa tekkimisel kaaluge impordi/ekspordi registreerimise funktsiooni siinkirjeldatud - [Impordi/ekspordi registreerimise] (https://msdn.microsoft.com/library/dn790624.aspx)

2. **Teenuse siini explorer**

    Paljud kliendid kasutada ServiceBus Exploreri siinkirjeldatud - [ServiceBus Exploreri] vaatamiseks ja haldamiseks oma teate jaoturi. See on saadaval code.microsoft.com - [ServiceBus Exploreri koodi] avatud allika projekt

###<a name="verify-message-notifications"></a>Veenduge, et teated

1. **Azure'i klassikaline portaal**

    Saate minna "Silumine" menüü test teatiste saatmiseks kliendid ilma mis tahes teenuse kirjutamata automaatselt üles ja töötab. 

    ![][7]

2. **Visual Studio**

    Lisaks saate saata test teatised Visual Studio vaid kaudu:

    ![][10]

    Lugege lisateavet Visual Studio teatis jaoturi Azure'i Exploreri funktsionaalsust siin - 
    
    - [VS Server Explorer ülevaade]
    - [VS Server Explorer ajaveebipostituse - 1]
    - [VS Server Explorer ajaveebipostituse - 2]

###<a name="debug-failed-notifications-review-notification-outcome"></a>Nurjunud teatised silumine / teatis tulemuste ülevaade

**EnableTestSend atribuut**

Kui saadate teatis teatis jaoturi kaudu, esialgu see lihtsalt saab järjekorras NH teha töötlemiseks aru saada kõik selle eesmärgid ja siis lõpuks NH saadab PNS. See tähendab, et kasutamisel REST API-ga või mis tahes kliendi SDK eduka tagasi saada kõne ainult tähendab, et sõnum on edukalt ootele pandud teate jaoturi abil. Mis juhtus, kui NH lõpuks sain sõnumi saatmiseks PNS ülevaate ei anna. Kui teie teatise on pole saabuma kliendi seade, on võimalik, et kui NH proovisin edastatavat sõnumit PNS, ilmnes tõrge, nt last maht ületab maksimaalselt lubatud, PNS või konfigureeritud NH mandaadid ei sobi jne. PNS tõrgete ülevaate saamiseks oleme juurutanud atribuudi [EnableTestSend funktsiooni]. Selle atribuudi automaatselt lubatud testi meilisõnumite saatmisel portaali või Visual Studio klient ja seega saate silumine üksikasjalikuma teabe vaatamiseks. Kasutage seda API-d, kui see on nüüd saadaval .NET SDK näitel kaudu ja lisatakse kõik kliendi SDK-d lõpuks. Kasutama ülejäänud kõne, lisa nimega "test" saada kõne lõpus nt päringustringi parameetri 

    https://mynamespace.servicebus.windows.net/mynotificationhub/messages?api-version=2013-10&test

*Näide (.NET SDK)*
 
Oletame, et kasutate .NET SDK kohalikke töölauateatise saata.

    NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(connString, hubName);
    var result = await hub.SendWindowsNativeNotificationAsync(toast);
    Console.WriteLine(result.State);
 
`result.State`kas lihtsalt maakond `Enqueued` täitmise ilma mis tahes ülevaate, mis on juhtunud teie tõuketeatised lõpus. Nüüd saate selle `EnableTestSend` kahendmuutujaga atribuudi lähtestamisel soovitud `NotificationHubClient` ja saan üksikasjaliku oleku teate saatmisel ilmnes PNS tõrgete kohta. Saada kõne siin võtab rohkem aega tagastamiseks, kuna see on ainult tagasi pärast NH on esitanud teate, et PNS tulemuse. 
 
    bool enableTestSend = true;
    NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(connString, hubName, enableTestSend);
    
    var outcome = await hub.SendWindowsNativeNotificationAsync(toast);
    Console.WriteLine(outcome.State);
    
    foreach (RegistrationResult result in outcome.Results)
    {
        Console.WriteLine(result.ApplicationPlatform + "\n" + result.RegistrationId + "\n" + result.Outcome);
    }

*Väljundi näidis*

    DetailedStateAvailable
    windows
    7619785862101227384-7840974832647865618-3
    The Token obtained from the Token Provider is wrong
 
See teade näitab, kas sobimatu mandaat on konfigureeritud teate jaoturi või klõpsake jaoturi registreerimise küsimus ja Soovitatavad kursuse oleks kustutada selle registreerimise ja lase enne sõnumi saatmist uuesti loomiseks kliendi. 
 
> [AZURE.NOTE] Pange tähele, et selle atribuudi kasutamine on tugevalt rakendus ja seega peate kasutama ainult see arendaja/testimiskeskkonnas piiratud hulgal registreerimise. Ainult saadame silumine teatised ja 10 seadmed. Meil on ka töötlemise silumine saadab 10 minutis on piiratud. 

###<a name="review-telemetry"></a>Telemeetria läbivaatamine 

1. **Azure'i klassikaline portaali kasutamine**

    Portaali võimaldab teil oma teate jaoturi kõik tegevuse kiire ülevaate saamiseks. 
    
    (a) "armatuurlaud" menüüs saate vaadata liidetud ülevaate registreerimine, teatised, samuti vigade kohta platvormi. 
    
    ![][5]
    
    (b) võite sisestada ka palju muud platvormi teatud mõõdikute tutvuge põhjalikult eriti PNS kindlate tõrgete tagastada siis, kui NH üritab saata teate PNS vahekaardilt "Jälgimine". 
    
    ![][6]
    
    c) peaks alustate läbivaatamise **Sissetulevate sõnumite** **Toimingud** **Eduka teatised** ja seejärel avage platvormi menüü üle vaadata PNS kindlate tõrgete kohta. 
    
    d) kui teil on valesti konfigureeritud autentimise sätetega teate jaoturi seejärel kuvatakse PNS autentimise tõrge. On hea märk kontrollida PNS mandaat. 

2) **Programmeerimisjuurdepääs**

Rohkem üksikasju siin- 

- [Programmiline telemeetria juurdepääs]
- [Telemeetria Accessi kaudu API-de näidis] 

> [AZURE.NOTE] Mitme telemeetria seotud funktsioone nagu **Impordi/ekspordi registreerimise** **Telemeetria API-de kaudu juurdepääsu** jne on saadaval ainult Standard astme. Kui proovite nende funktsioonide kasutamiseks, kui teil on tasuta või lihtsa taseme seejärel saate erandi teade selle kohta, kasutades SDK ja mõne HTTP 403 (keelatud) kasutamisel need otse REST API-de. Veenduge, et teil on teisaldatud Standard kuni esimese taseme Azure'i klassikaline portaali kaudu.  

<!-- IMAGES -->
[0]: ./media/notification-hubs-diagnosing/Architecture.png
[1]: ./media/notification-hubs-diagnosing/GCMConfigure.png
[2]: ./media/notification-hubs-diagnosing/GCMEnable.png
[3]: ./media/notification-hubs-diagnosing/GCMServerKey.png
[4]: ./media/notification-hubs-diagnosing/PortalConfigure.png
[5]: ./media/notification-hubs-diagnosing/PortalDashboard.png
[6]: ./media/notification-hubs-diagnosing/PortalMonitoring.png
[7]: ./media/notification-hubs-diagnosing/PortalTestNotification.png
[8]: ./media/notification-hubs-diagnosing/VSRegistrations.png
[9]: ./media/notification-hubs-diagnosing/VSServerExplorer.png
[10]: ./media/notification-hubs-diagnosing/VSTestNotification.png
 
<!-- LINKS -->
[Teatis jaoturi ülevaade]: notification-hubs-push-notification-overview.md
[Alustamise õpetus]: notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md
[Malli juhised]: https://msdn.microsoft.com/library/dn530748.aspx 
[APNs-i juhiseid.]: https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW4
[GCM juhised]: http://developer.android.com/google/gcm/adv.html
[Ekspordi/impordi registreerimine]: http://msdn.microsoft.com/library/dn790624.aspx
[ServiceBus Explorer]: http://msdn.microsoft.com/library/dn530751.aspx
[ServiceBus Exploreri kood]: https://code.msdn.microsoft.com/windowsazure/Service-Bus-Explorer-f2abca5a
[VS Server Explorer ülevaade]: http://msdn.microsoft.com/library/windows/apps/xaml/dn792122.aspx 
[VS Server Explorer ajaveebipostituse - 1]: http://azure.microsoft.com/blog/2014/04/09/deep-dive-visual-studio-2013-update-2-rc-and-azure-sdk-2-3/#NotificationHubs 
[VS Server Explorer ajaveebipostituse - 2]: http://azure.microsoft.com/blog/2014/08/04/announcing-release-of-visual-studio-2013-update-3-and-azure-sdk-2-4/ 
[Funktsioon EnableTestSend]: http://msdn.microsoft.com/library/microsoft.servicebus.notifications.notificationhubclient.enabletestsend.aspx
[Programmiline telemeetria juurdepääs]: http://msdn.microsoft.com/library/azure/dn458823.aspx
[Telemeetria Accessi kaudu API-de näidis]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/FetchNHTelemetryInExcel

 