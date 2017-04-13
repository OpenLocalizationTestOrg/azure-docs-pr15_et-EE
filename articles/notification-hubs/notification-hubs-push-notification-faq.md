<properties
    pageTitle="Azure'i teatis jaoturi - korduma kippuvad küsimused (KKK)"
    description="Korduma kippuvad küsimused lahendusi teatis jaoturi kujundamisel/rakendamise kohta"
    services="notification-hubs"
    documentationCenter="mobile"
    authors="ysxu"
    manager="erikre"
    keywords="tõuketeatised teatis, Tõuketeatiste, iOS-i Tõuketeatiste, Androidi Tõuketeatiste, iOS-i tõuketeatised, android tõuketeatised"
    editor="" />

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="yuaxu" />

#<a name="push-notifications-with-azure-notification-hubs---frequently-asked-questions"></a>Tõuketeatised ja Azure teatis jaoturi - korduma kippuvad küsimused

##<a name="general"></a>Üldine
###<a name="1---what-is-the-price-model-for-notification-hubs"></a>1. mis on teatise jaoturi mudeli hind?
Teatis jaoturi on saadaval kolmest tasemest:

* **Tasuta** - saada kuni 1 miljon sunnib kuus tellimuse kohta.
* **Põhi** - get 10 miljonit sunnib tellimuse kuus on aluseks kvoodi growth suvandite kohta.
* **Standard** - 10 miljonit sunnib kohta kuus on aluseks tellimuse saamine, kvoot suurendada suvandid ning rikkaliku telemeetria capabilties.

Lehel [Teate jaoturi hinnad] leiate uusimate üksikasjadega saate tutvuda. Funktsiooni hinnad on loodud tasemel tellimuse ja põhineb tõuketeatised teatis uurimised arvu, nii et Azure tellimuse olete loonud mitu nimeruumid või teatise jaoturi pole oluline.

**Tasuta** taseme pakutakse arengu eesmärgil SLA garantii puudub. Kuigi see taseme võib olla alustamiseks hea koht neile, kes uuritava võimaluste Tõuketeatiste Azure'i teatis jaoturi kaudu, ei pruugi suuremahuliste rakendused keskmise parim valik.

**Lihtsa** & **Standard** astme pakutakse tootmise kasutus sisaldab järgmisi olulisi funktsioone on lubatud *ainult jaoks taseme*:

- *RTF-vormingus telemeetria* - teatise jaoturi pakkumine mitmeid võimalusi telemeetria andmete eksportimine kui ka push teatise andmed ühenduseta vaatamiseks ja analüüsimiseks.
- *Mitme rendiõigus* – sobib, kui loote mobiilirakenduse kaudu, mis toetavad mitme rentniku teatis jaoturi abil. See võimaldab teil rakenduse tasemel teate jaoturi nimeruum Push teatis Services (PNS) identimisteabe määramine ja seejärel võite eraldada rentnikud, andes neile üksikuid jaoturi jaotises selle levinud nimeruumi. See võimaldab lihtne hooldus ajal säilitamise SAS võtmed saata ja tõuketeatised saadud teatise jaoturiga eraldatud iga tagada mitte rist – rentniku kattumise rentniku jaoks.
- *Tõuketeatised ajastatud* - võimaldab teil ajakava Tõuketeatiste, mida hiljem ootele ja saatis.
- *Hulgi importimine* – saate importida registreerimise mitmekaupa.

###<a name="2---what-is-the-notification-hubs-sla"></a>2. mida on teatis jaoturi SLA?
**Põhi** - ja **Standard** teatis jaoturi astme, me tagada, et vähemalt 99,9% ajast, õigesti konfigureeritud rakendused on võimalik Tõuketeatiste saatmine või toiminguid registreerimise haldamine jooksul toetatud taseme teatise jaoturi suhtes. Meie SLA kohta lisateabe saamiseks külastage [Teatis jaoturi SLA] lehe.

> [AZURE.NOTE] Pole etapil platvormi teavitusteenuse ja seadme vahel SLA garantiid, kuna teatis jaoturi sõltuvad välise platvormi pakkujad esitamisel tõuketeatised teatis seade.

###<a name="3---which-customers-are-using-notification-hubs"></a>3. mis kliendid kasutate teatis jaoturi?
Meil on mitmeid klientidele, kes kasutavad teatis jaoturi abil mõned olulised need allpool:

* Sochi 2014 – 100s huvirühmade, 3 + miljonit seadmete, 2 nädala jooksul saadetud 150 + miljonit teatis. [CaseStudy - Sochi]
* Skanska - [CaseStudy - Skanska]
* Seattle Times - [CaseStudy - Seattle Times]
* Mural.ly - [CaseStudy - Mural.ly]
* 7Digital - [CaseStudy - 7Digital]
* Bingi rakendused – seadmed, miljonite 10s saatmine 3 miljonit teatised/päev.

###<a name="4-how-do-i-upgrade-or-downgrade-my-notification-hubs-to-change-my-service-tier"></a>4. Kuidas versiooni täiendamine või alandada minu teatis jaoturi minu teenuse kiht muuta?
Avage [Azure klassikaline portaali], klõpsake teenuse siini, ja klõpsake seejärel oma nimeruum teie teate jaoturi. Klõpsake menüüs skaala on võimalik muuta oma teatise jaoturi teenuse kiht.

![](./media/notification-hubs-faq/notification-hubs-classic-portal-scale.png)

##<a name="design--development"></a>Kujundus- ja arendustegevus
###<a name="1---which-server-side-platforms-do-you-support"></a>1. mis serveripoolne platvormid saate toetab?
Pakume SDK-d ja [täielik näidised] .NET, Java, PHP, Python, Node.js nii, et soovitud rakendus kirjutamata saab setup edastama teate jaoturi abil, mis tahes need platvormid. Teatis jaoturi API-de põhinevad ülejäänud liidesed nii, et saate valida otse rääkida need hoopis kui te ei soovi lisada mõne eest sõltuvus. Lisateavet leiate lehel [NH - REST API-d] .

###<a name="2---which-client-platforms-do-you-support"></a>2. mis kliendi platvormid saate toetab?
[Apple iOS](notification-hubs-ios-apple-push-notification-apns-get-started.md), [Android](notification-hubs-android-push-notification-google-gcm-get-started.md), [Ühes kohas Windowsi](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md), [Windows Phone](notification-hubs-windows-mobile-push-notifications-mpns.md), [Kindle](notification-hubs-kindle-amazon-adm-push-notification.md), [Android Hiina (Baidu) kaudu](notification-hubs-baidu-china-android-notifications-get-started.md), Xamarin saatmise Tõuketeatiste toetame ([iOS-i](xamarin-notification-hubs-ios-push-notification-apns-get-started.md) & [Androidi](xamarin-notification-hubs-push-notifications-android-gcm.md)), [Rakenduste Chrome'i](notification-hubs-chrome-push-notifications-get-started.md) ja [Safari](https://github.com/Azure/azure-notificationhubs-samples/tree/master/PushToSafari) platvormide jaoks loodud. Kasutamise alustamine õpetused tegelemine saatmise Tõuketeatiste need platvormid täieliku loendi leiate meie [NH - kasutamise alustamine õpetused] lehelt.

###<a name="3---do-you-support-smsemailweb-notifications"></a>3. mida toetavad SMS/e-posti/web teatised?
Teatis jaoturi eesmärk on eelkõige saada teatiste abil eelnimetatud platvormid mobiilirakendused. Me ei paku veel võimalust saata e-posti või SMS-teadete; Siiski võib olla koos teatise jaoturi saata kohalikke Tõuketeatiste [Azure'i Mobile'i rakenduste]kasutamise integreeritud kolmandate osapoolte platvormide jaoks loodud, mis pakuvad neid võimalusi.

Teatis jaoturi pakuvad ka mõne-brauseri tõuketeatised teatis kohaletoimetamise teenuse välja-,--box. Klientide võite rakendada selle peal toetatud serveripoolseid platvormid SignalR abil. Kui otsite Liivakasti Chrome'i brauseris rakenduste teatised saata, lugege teemat [Chrome'i rakenduste õpetuse].

###<a name="4---what-is-the-relation-between-azure-mobile-apps-and-azure-notification-hubs-and-when-do-i-use-what"></a>4. mis on Azure mobiilirakenduste ja Azure teatis jaoturi ja millal mida kasutada?
Kui teil on mõne olemasoleva mobiilirakenduse kirjutamata ja soovite lisada võimalus saata tõuketeatised saate kasutada Azure teatis jaoturi. Kui soovite häälestada oma mobiilirakenduse kirjutamata nullist siis võiksite kasutada Azure mobiilirakenduste kohta. Azure'i Mobile'i rakendus sätted automaatselt teate jaoturi saate saata Tõuketeatiste hõlpsalt kirjutamata mobiilirakenduse kaudu. Azure'i mobiilirakenduste hinnad sisaldab base kulude teate jaoturi ja te maksate ainult siis, kui avate lisaks kaasatud sunnib. Lisateave kulude on [Rakenduse teenuse hinnad] lehelt.

###<a name="5---how-many-devices-can-i-support-if-i-send-push-notifications-via-notification-hubs"></a>5. Kui palju seadmeid ma toetab, kui saadan Tõuketeatiste teatis jaoturi kaudu?
Toetatud seadmed arvu [Teatis jaoturi hinnad] lehelt leiate.

Teatud stsenaariumid, kui teil on vaja tugi üle 10000000 registreeritud seadmed, palun [võtke meiega ühendust](https://azure.microsoft.com/overview/contact-us/) otse ja me aitab teil mastaapimiseks teie lahendus.

###<a name="6---how-many-push-notifications-can-i-send-out"></a>6. Kui palju tõuketeatisi saate saata?
Sõltuvalt valitud taseme, Azure'i kuvatakse automaatselt mastaapida läbib süsteemi teatiste arvu põhjal.

>[AZURE.NOTE] Üldine kasutus maksumus saate tõusevad põhineb kätte Tõuketeatiste arv. Veenduge, et olete teadlik liigendatud lehel [Teatis jaoturi hinnad] olemasoleva taseme piirangud.

Klientide kasutavad teatis jaoturi saata miljoneid Tõuketeatiste iga päev. Teil pole midagi mastaapimiseks oma push teatised saavutamiseks, kui kasutate Azure teatis jaoturi teisiti teha.

###<a name="7---how-long-does-it-take-for-sent-push-notifications-to-reach-my-device"></a>7. Kui kaua aega võtab saadetud tõuketeatised seadmes jõuda?
Azure'i teatis jaoturi on võimalik töödelda vähemalt **1 miljon tõuketeatised teatise saadab minutiks** tavaline kasutada stsenaarium, kus sissetulevate laadimine on päris ühtsete ja pole spikey laadi. See määr võivad erineda sõltuvalt sildid, sissetulevate saadab laad ja väline tegur arv.

Hinnanguline kohaletoimetamise aja jooksul teenus on võimalik arvutada sihtkohtade platvormi ja marsruutimiseks sõnumid vastav tõuketeatised teatis kohaletoimetamise teenuste põhjal registreeritud sildid/sildi avaldiste kohta. Siit alates on tõuketeatised teenuseid (PNS) saata teate seade.

Mõne PNS ei garanteeri mis tahes SLA pakkuda teatised; siiski tavaliselt Tõuketeatiste enamik toimetatakse target seadmed (tavaliselt piires 10 minuti) mõne minuti jooksul aeg need saadetakse meie platvormi kaudu. Võib olla mõni võõrväärtuste, mis võib kuluda rohkem aega.

>[AZURE.NOTE] Azure'i teatis jaoturi on kohas, kui soovite mis tahes Tõuketeatiste mis ei saa toimetada PNS 30 minutiga poliitika. See viivitus võib olla mitu põhjust, kõige sagedamini Kuna PNS on pidurdamise rakenduse.

###<a name="8---is-there-any-latency-guarantee"></a>8. on olemas mis tahes latentsus tagatis?
Tõuketeatiste (need on esitatud välise, platvormi kohased tõuketeatiseteenus) olemus puudub latentsus garantii. Tõuketeatiste enamik saada tarniti tavaliselt paar minutit.

###<a name="9---what-are-the-considerations-i-need-to-take-into-account-when-designing-a-solution-with-namespaces-and-notification-hubs"></a>9. Millised kaalutlused vajan nimeruumid ja teatis jaoturi lahenduse kavandamisel arvesse võtta?

####<a name="mobile-appenvironment"></a>Mobiilse rakenduse/keskkonnas

* Peaks olema üks teate jaoturi mobiilirakenduses keskkonna kohta.
* Mitme rentniku stsenaariumi puhul iga rentniku peaks olema eraldi jaoturi.
* Sama teate jaoturi keskkondade vahel peab ei jaga, kuna see võib põhjustada probleeme rea alla saatmisel teatised. nt Apple pakub Liivakasti-ja tootmise Push kummalgi on eraldi mandaat.
* Vaikimisi saate saata test teatised registreeritud seadmetes Azure portaali või Azure integreeritud komponendi Visual Studio kaudu. Piirmäär on seatud 10 seadet, juhuslike valitud registreerimise kaustast.

>[AZURE.NOTE] Kui jaoturi konfigureeritud algselt Apple Liivakasti sertifikaadiga ja seejärel uuesti kasutada ka Apple'i toodangusertifikaat vana seadme sõned kehtivuse uue sertifikaadiga ja põhjustada sunnib nurjumise. See on kõige parem eraldi oma tootmisel ja keskkonnas ja kasutada erinevaid jaoturi viibite.

####<a name="pns-credentials"></a>PNS identimisteave

Mobiilirakenduses on registreeritud on platvorm arendaja portaali (Apple või Google jne) seejärel saate rakenduse identifikaator ja turve märgid, mis on rakenduse kirjutamata platvormi tõuketeatised teatis teenusele saaks saata Tõuketeatiste seadmete. Teatis jaoturi konfigureerida tuleb nende tähiste Turve, mis võib olla kujul serdid (nt Apple iOS-i või Windows Phone) või turbe tootenumbrid (Google Android, Windows jne). Seda tehakse tavaliselt teatis jaoturi tasemel, kuid saate teha ka stsenaariumis mitme rentniku tasemel nimeruum.

####<a name="namespaces"></a>Nimeruumid

Nimeruumid saab juurutamise rühmitamiseks.  See saab tähistada kõik teatis jaoturi kõik rentnikud sama rakenduse stsenaariumis mitme rentniku jaoks.

####<a name="geo-distribution"></a>Geo-jaotuse.

Geo-jaotus on alati kriitiliste tõuketeatised teatis stsenaariumide. See on märkida, et erinevate tõuketeatised teatis teenuste (nt APNs-i, GCM jne), mis lõpuks pakkuda soovitud Tõuketeatiste seadmeid, ei ole ühtlaselt jaotatud ühte.

Kui teil on rakendus, mida kasutatakse kogu maailmas, saate luua mitme jaoturi ära teatise jaoturi teenuse kättesaadavust eri Azure piirkondades üle maailma eri nimeruumid.

>[AZURE.NOTE] Suurendada halduskulud – eriti ümber registreerimine, et see pole eriti soovitatav ja ainult tuleb teha, kui on vaja selgesõnaliselt.

###<a name="10--should-i-do-registrations-from-the-app-backend-or-directly-through-client-devices"></a>10. teha registreerimise rakenduse kirjutamata või otse kliendi seadmed?
Registreerimise kaudu rakenduse taustväärtus on kasulik, kui peate tegema enne loomist registreerimise kliendi autentimise või kui teil on sildid, peab olema loodud või muudetud rakenduse taustväärtus, mõned rakenduse loogika põhjal. Lisateabe saamiseks lugege [kirjutamata registreerimise juhiseid] ja [kirjutamata registreerimise juhiseid - 2] lehtedel.

###<a name="11--what-is-the-push-notification-delivery-security-model"></a>11. mis on tõuketeatised teatis kohaletoimetamise mudelit?
Azure'i teatis jaoturi kasutamine on [Ühiskasutusse antud juurdepääs allkirja (SAS)](../storage/storage-dotnet-shared-access-signature-part-1.md)-põhine turvalisus mudel. Saate SAS sõned nimeruum juurtasemel või Varundustöö teatis jaoturi tasemel. Nende tähiste SAS saab määrata eri autoriseerimine reegliga nt saada sõnumi õigused, kuulata teatis õiguste jne. Lisateave on saadaval dokumendis [NH turvalisuse mudel] .

###<a name="12--how-should-i-handle-sensitive-payload-in-the-push-notifications"></a>12. Kuidas ma hakkama tundliku loomuga last klõpsake soovitud tõuketeatised?
Kõik teatised, toimetatakse target seadmed, platvormi Push teatis Services (PNS). Kui saatja saadab isikule teate Azure'i teatis jaoturi siis töötlemine ja edastama teate vastav PNS.

Kõik ühendused saatja PNS Azure'i teatised jaoturiga ja seejärel kasutage HTTPS.

>[AZURE.NOTE] Azure'i teatised jaoturi ei Logi last sõnumi mis tahes viisil.

Saatmine tundliku lõhkelaengu kuid soovitame turvaline Push mustri kui saatja Esita identifikaatoriga teade "ping teatis ilma tundliku loomuga last ja kui seadmes rakendus saab seda last seade, on võimalik helistada turvaliseks API otse toomiseks sõnumi andmed. Juhised, kuidas rakendada eespool kirjeldatud muster on saadaval [NH - Secure Push õpetuse] lehel.

##<a name="operations"></a>Toimingud
###<a name="1---what-is-the-disaster-recovery-dr-story"></a>1. mis on katastroof taastamine (DR) lugu?
Pakume metaandmete avariitaastet ulatus meie poolel (teate jaoturi nimi, ühendusstringi ja muu oluline teave). Stsenaarium DR käivitamisel registreerimise andmed on **ainult lõigu** teatis jaoturi infrastruktuuri lähevad kaotsi. Peate rakendada asustamiseks need andmed uuesti sisse oma uue jaoturi pärast taastamise lahendust.

- *1. etapp* – eri andmekeskuse teisene teate jaoturi loomine. Saate luua selle pealt DR sündmuse ajal või saate selle luua toomine minna. See ei tee palju erinevat, millise variandi valite, kuna teate jaoturi ettevalmistamise on üsna kiiresti protsessi järjekorras mõne hetke. Millel on üks algusest kaitsta teie mõjutavad oma halduse võimaluste, nii et see on väga soovitatav suvand DR sündmus.

- *Step2* - hüdraat teisene teate jaoturi abil registreerimise esmane teatis keskuse kaudu. See on soovitatav säilitada nii jaoturi sisse registreeritud ja proovige hoida neid sünkroonis pealt registreerimise tulevad-tavaliselt, mis ei töötanud ka tõttu omast andmete keskmise registreerimise aegumatuks PNS küljel proovida. Teatis jaoturi puhastamiseks neid kui me saame PNS aegunud või sobimatu registreerimist kohta tagasiside.  

Soovitus on kasutada ka rakenduse taustväärtus kas:

- Säilitab oma end registreeritud määratud nii, et seda saab teha hulgi lisada korral DR jaoturiga teisene teatis

**VÕI**

- Saab regulaarne dump registreerimise varukoopiana esmane keskuse kaudu ja seejärel soovitud hulgi lisada sekundaarne NH.

>[AZURE.NOTE] Registreerimise impordi/ekspordi funktsiooni, mis on saadaval standardse taseme on kirjeldatud dokumendi [Registreerimise eksportimine ja importimine] .

Kui teil on kirjutamata seejärel sisselülitamisel rakenduse mis tahes target seadmed, nad on ootel uus registreerimise teisene teate jaoturi ja lõpuks teisese teate jaoturi on registreeritud Aktiivsed seadmed.

Negatiivne on, et tekib aja jooksul, kui seadmetes, kus rakendused pole veel avanud ei saa teatisi.

###<a name="2---is-there-any-audit-log-capability"></a>2 on mis tahes auditilogi log võimalus?
Jaoturi teatiste kõik toimingud, mis on esitatud [Azure klassikaline portaali]Logi minek

##<a name="monitoring--troubleshooting"></a>Jälgimine ja tõrkeotsing
###<a name="1---what-troubleshooting-capabilities-are-available"></a>1. mida tõrkeotsingu funktsioonid on saadaval?
Azure'i teatis jaoturi pakuvad mitmesuguseid funktsioone, teha levinud vigade, eriti kõige levinum stsenaarium ümber kaotatud teatised. Üksikasjad leiate meie [NH - tõrkeotsingu] lühiülevaade.

###<a name="2---what-telemetry-features-are-available"></a>2. mida telemeetria funktsioonid on saadaval?
Azure'i teatis jaoturi lubab [Azure klassikaline portaali]telemeetria andmete vaatamiseks. Saadaval mõõdikute üksikasjad on [NH - mõõdikute] lehelt.

>[AZURE.NOTE] Eduka teatised ainult tähendab, et selle Tõuketeatiste toimetatud selle välise Lükketeatiste teenus (nt Apple APNS, GCM Google jne). See on kuni PNS esitamisel teate target seadmed. Tavaliselt, jätke PNS kohaletoimetamise mõõdikute kolmandatele isikutele.  

Pakume ka võimalus ekspordiks telemeetria andmeid programmiliselt ( **Standard** taseme). Lugege teemat [NH - mõõdikute valimi] üksikasju.

[Azure'i klassikaline portaal]: https://manage.windowsazure.com
[Teatis jaoturi hinnad]: http://azure.microsoft.com/pricing/details/notification-hubs/
[Teatis jaoturi SLA]: http://azure.microsoft.com/support/legal/sla/
[CaseStudy - Sochi]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=7942
[CaseStudy - Skanska]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=5847
[CaseStudy - Seattle korda]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=8354
[CaseStudy - Mural.ly]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=11592
[CaseStudy - 7Digital]: https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=3684
[NH - REST API-d]: https://msdn.microsoft.com/library/azure/dn530746.aspx
[NH - alustamise õpetus]: http://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started/
[Chrome'i rakendused õpetus]: http://azure.microsoft.com/documentation/articles/notification-hubs-chrome-get-started/
[Mobile Services Pricing]: http://azure.microsoft.com/pricing/details/mobile-services/
[Taustväärtus registreerimise juhiseid.]: https://msdn.microsoft.com/library/azure/dn743807.aspx
[Taustväärtus registreerimise juhiseid - 2]: https://msdn.microsoft.com/library/azure/dn530747.aspx
[Mudeli NH turvalisus]: https://msdn.microsoft.com/library/azure/dn495373.aspx
[NH - turvaline tõuketeatised õpetus]: http://azure.microsoft.com/documentation/articles/notification-hubs-aspnet-backend-ios-secure-push/
[NH - tõrkeotsing]: http://azure.microsoft.com/documentation/articles/notification-hubs-diagnosing/
[NH - mõõdikud]: https://msdn.microsoft.com/library/dn458822.aspx
[NH - mõõdikute näidis]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/FetchNHTelemetryInExcel
[Registreerimise eksportimine ja importimine]: https://msdn.microsoft.com/library/dn790624.aspx
[Azure Portal]: https://portal.azure.com
[täieliku näidised]: https://github.com/Azure/azure-notificationhubs-samples
[Azure'i mobiilirakendused]: https://azure.microsoft.com/en-us/services/app-service/mobile/
[Rakenduse teenuse hinnad]: https://azure.microsoft.com/en-us/pricing/details/app-service/
