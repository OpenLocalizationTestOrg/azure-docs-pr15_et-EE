<properties
    pageTitle="Azure'i teatis jaoturi"
    description="Saate teada, kuidas kasutada Azure. Koodinäiteid kirjutatud C# .net-i API abil."
    authors="ysxu"
    manager="erikre"
    editor=""
    services="notification-hubs"
    documentationCenter=""/>

<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="multiple"
    ms.devlang="multiple"
    ms.topic="hero-article"
    ms.date="08/25/2016"
    ms.author="yuaxu"/>


#<a name="azure-notification-hubs"></a>Azure'i teatis jaoturi

##<a name="overview"></a>Ülevaade

Azure'i teatis jaoturi pakuvad lihtne kasutada, paljudele, mastaabitud kontorist tõuketeatised taristu, mis võimaldab saata mobiililükketeatised mis tahes kirjutamata (pilve või kohapealse) klõpsake mis tahes mobiilne platvorm.

Teatis jaoturi, abil saate hõlpsalt saata mitu platvormi, isikupärastatud Tõuketeatiste niisutussüsteemide eri platvormi teate süsteemid (PNS) üksikasju. Ühe API kõne, saate suunata üksikkasutajate või kogu publik segmente sisaldava miljoneid kasutajaid, kõigis oma seadmetes.

Saate teatise jaoturi enterprise ja tarbija stsenaariumid. Näiteks:

- Server uudiste teatised saata miljoneid koos madal latentsus (teatis jaoturi volitused Bingi rakenduste eelinstallitud kõigis Windows ja Windows Phone seadmetes).
- Saatke kuponge asukoht põhineb kasutaja segmente.
- Kasutajate või rühmade rahandus/Sportmängud rakenduste sündmus teateid saata.
- Teavitage kasutajaid enterprise sündmuste nagu uued sõnumid/meilid ja müügivihjed.
- Saada üks-tööajaga-paroolid kohustuslikuks mitmikautentimise jaoks.



##<a name="what-are-push-notifications"></a>Mis on tõuketeatised?

Nutitelefonides ja tahvelarvutites saate "Teavita" kasutajate Sündmuse ilmnemise. Need teatised võib selleks kuluda palju vorme.

Windowsi poe ja Windows Phone rakendusi, saab teatise _töölauateatisega_kujul: kuvatakse Mittemodaalne aken heliga märku uus teatis. Muud tüüpi teatis, mis on toetatud kaasata _paani_, _töötlemata_ja _Märgi_ teatised. Toetatud Windowsi seadmes teatiste tüüpide kohta leiate lisateavet teemast [paanid, märgid, ja teatisi](http://msdn.microsoft.com/library/windows/apps/hh779725.aspx).

Apple iOS-seadmetes push Samuti teavitab kasutaja dialoogiboks ja paluda kasutajal kuvamine või teate sulgemine. Klõpsake menüüs **Vaade** avaneb rakendus, mis võtab vastu sõnumi. IOS-i teatiste kohta leiate lisateavet teemast [iOS-i teatised](http://go.microsoft.com/fwlink/?LinkId=615245).

Tõuketeatiste aidata mobiilsideseadmete, jäädes tõhusate värske teabe kuvamiseks. Teatiste saatmist taustväärtus abil mobiilsideseadmete isegi siis, kui vastav rakenduste seadmes pole aktiivne. Tõuketeatised on oluline osa tarbija rakendused, mida kasutatakse suurendamiseks rakenduse kaasamine ja kasutamist. Teatiste on kasulik ettevõtetele, ka siis, kui ajakohast teavet suureneb töötaja tundlikkuse business sündmustele reageerida.

Mõned mobiilsideseadmete kaasamine stsenaariumid teatud näited:

1.  Värskendamisel tile Windows 8 või Windows Phone praeguse finantsteavet.
2.  Kasutaja, kellel on mõni töö üksuse töövoo vastavalt ettevõtte rakenduse kasutajale määratud töölauateatis teavitamine.
3.  Märgi numbriga praeguse müügi kuvamiseks viib CRM-i rakendus (nt Microsoft Dynamics CRM-i).

##<a name="how-push-notifications-work"></a>Kuidas Push teatised töö

Tõuketeatiste edastatakse platvormi kohased infrastruktuuri nimetatakse _Platvormi teate süsteemid_ (PNS) kaudu. Mõne PNS pakub Barebone funktsioonid (st ei toeta leviedastuse personalization) ja on levinud liidesel puudub. Teatise saatmine Windowsi poe rakendusse, peab arendaja näiteks pöörduge WNS (Windowsi teenus). IOS-seadmes teatise saatmiseks sama arendaja on pöörduge APNs-i (Apple Push teavitusteenuse), ja saatke sõnum teist korda. Azure'i teatis jaoturi aidata levinud kasutajaliides, koos muude funktsioonide toetamiseks Tõuketeatiste üle iga platvormi esitada.

Kõrge, küll, kõik platvormi teate süsteemid tehke sama muster.

1.  Kliendi rakenduse Kontaktid tuua oma _toime_PNS. Täitepide tüüp sõltub süsteem. WNS, on URI või "teatis kanali." APNs-i, on märgiks.
2.  Kliendi rakendus talletab selle pidet rakenduse _tagaandmebaas_ hilisemaks kasutamiseks. WNS, kuvatakse tagaandmebaas on tavaliselt mõnda pilveteenusesse. Apple, nimetatakse süsteemi _pakkuja_.
3.  Tõuketeatised teatise saatmine kontaktid rakenduse tagaandmebaas PNS pidet abil saate suunata kindla kliendi rakenduse eksemplari.
4.  PNS edastab teate seadmele määratud pidet.

![][0]

##<a name="the-challenges-of-push-notifications"></a>Tõuketeatiste probleemid

Kuigi need süsteemid on väga võimas, endiselt lahkumist palju tööd rakenduse arendaja rakendamiseks isegi levinud push teatis stsenaariumid, nt leviedastust või saatmist Tõuketeatiste segmenditud kasutajad.

Tõuketeatised on üks kõige nõutud funktsioonide pilveteenustega mobiilirakendused. Selle põhjuseks on, et neid tööle vajalikku infrastruktuuri on üsna keerukas ja peamiselt seotud peamised äriloogika rakenduse. Mõned probleeme hoone nõudmisel tõuketeatised on:

- **Platvormi sõltuvus.** Teatiste saatmiseks seadmed erinevate platvormide kodeeritakse mitme liideste tagasi-end. Mitte ainult madala üksikasjad erinevad, kuid esitluse teatise (paan, töölauateatisega või märk) on ka platvormi sõltuvate. Erinevused võib kaasa tuua keerukas ja raske säilitada tagaandmebaas kood.

- **Skaala.** Selle taristu skaleerimist on kahel viisil:
    + PNS suuniste kohta seadme sõned tuleb värskendada iga kord, kui rakendus on käivitatud. See viib liikluse (ja järgnevad andmebaasi avab) lihtsalt seadme sõned ajakohastamine suurt hulka. Kui seadmete arv kasvab (tõenäoliselt miljoneid), luua ja säilitada selles taristu maksumus on nonnegligible.

    + Enamik PNSs ei toeta levisaate mitmes seadmes. See järgib, et levisaate miljoneid seadmed, on tulemuseks miljoneid kõned on PNSs. Mastaapimiseks taotlused võimalus on nontrivial, kuna tavaliselt rakenduste arendajatele soovite säilitada kokku latentsus. Näiteks viimase seadme kuvatakse teade peaksid saama teatise 30 minuti pärast saatmist teatised, nagu paljudel juhtudel võivad seda kahjustada eesmärk on tõuketeatisi.
- **Marsruutimist.** PNSs võimalda seadme sõnumi saatmiseks. Siiski enamik rakendustes teatised on suunatud kasutajate ja/või huvirühmade (nt kõik töötajad määratud teatud kliendi konto). Sellisel kujul marsruutida seadmete õige teatised, et rakendus tagaandmebaas tuleb säilitada registri, mis seostab huvirühmade seadme sõned. See pea kohal lisab kogu aeg market ja hooldus kulude rakendus.

##<a name="why-use-notification-hubs"></a>Miks kasutada teatis jaoturi?

Jaoturi kõrvaldada keerukuse teatis: teil tuleb toime tulla tõuketeatisi. Saate teate jaoturi. Teatis jaoturi kasutage täielikku paljudele, mastaabitud kontorist tõuketeatised teatis taristu ja märgatavalt vähendada tõuketeatised kohased koodi kirjutamata rakendus töötab. Teatis jaoturi rakendada kõiki tõuketeatised taristu funktsioone. Seadmete vastutate ainult registreerimisel PNS pidemetega ja selle kirjutamata vastutab platvormi sõltumatul sõnumite saatmiseks kasutajatele või rühmadele intressi, nagu on näidatud järgmisel joonisel:

![][1]


Teatis jaoturi pakuvad valmis kasutada push teatis taristu järgmised eelised:

- **Mitme kaudu.**
    +  Tugi kõigi põhi mobiilside kaudu. Teatis jaoturi saate saata Tõuketeatiste Windowsi poe, iOS-i, Androidi ja Windows Phone rakendused.

    +  Teatis jaoturi pakuvad levinud kasutajaliidese saata teatised kõigi toetatud platvormid. Platvormi kohased Protokollid ei ole vaja. Rakenduse tagaandmebaas saadab teatisi platvormi kohased või CAL vormingus. Rakendus suhtleb ainult teatis jaoturi.

    +  Seadmehalduse pidet. Jaoturi säilitab pidet registri ja tagasiside PNSs teatis.

- **Mis tahes tagaandmebaas töötab**: pilveteenuste või kohapealse, .NET, PHP, Java sõlm, jne.

- **Skaala.** Teatis jaoturi skaala miljoneid ilma vaja uuesti arhitekt või Kildu.


- **RTF-vormingus seadmine kohaletoimetamise mustrid**:

    - *Leviedastuse*: võimaldab lähedal samaaegne leviedastuse miljoneid seadmed ühe API kõne.

    - *Ainusaate/multisaate*: Push sildid, mis tähistab üksikkasutajate, sh kõigis oma seadmetes; või suurema rühma; näiteks eraldi vormis tegureid (telefoni ja tahvelarvuti).

    - *Osadeks*: Push keerukate lõiku, mis on määratletud sildi avaldisi (näiteks New York Yankees pärast seadet).

    Iga seadme selle pidet saatmisel teate jaoturi, saate määrata ühe või mitme _Sildid_. Lisateavet [Sildid]. Sildid pole eelnevalt ette valmistatud või kõrvaldada. Siltide pakub lihtsat võimalust saata teatised kasutajate või rühmade intressi. Kuna sildid võivad sisaldada mis tahes konkreetse rakenduse identifikaator (nt kasutaja või rühma ID-d), saavad kasutada vabastab koormust, millel on talletamiseks ja haldamiseks seadme pidemetega rakenduse tagaandmebaas.

- **Isikupärastamissaidi**: iga seade võib olla üks või mitu Mallid saavutamiseks seadme kohta lokaliseerimine ja isikupärastamist mõjutamata tagaandmebaas kood.

- **Turvalisus**: ühiskasutusse antud juurdepääs salajane (SAS) või väline autentimist.

- **RTF-vormingus telemeetria**: saadaval portaalis ja programmiliselt.


##<a name="integration-with-app-service-mobile-apps"></a>Rakenduse teenuse mobiilirakendused integreerimine

Azure'i teenustes sujuvalt ja ühendav kogemus hõlbustamiseks on [Rakenduse teenuse mobiilirakenduste] tugi Tõuketeatiste teatis jaoturi abil. [Rakenduse teenuse mobiilirakenduste] pakub väga paindlik, mis on saadaval globaalselt mobiilirakenduse arengu platvorm Enterprise arendajate ja süsteemiintegraatorid, mis toob mobiilsideseadmete arendajate suurel hulgal erinevaid võimalusi.

Mobiilirakenduste arendajad saate kasutada järgmisi töövooga teatis jaoturi:

1. Saate tuua seadme PNS pide
2. Registreerida teatis jaoturi kaudu mugav Mobile'i rakendused kliendi SDK register API seade ja [Mallid]
    + Pange tähele, et mobiilirakenduste ribad ära kõik sildid sisse registreeritud turvalisuse tagamiseks. Töötamine teatis jaoturi kaudu oma kirjutamata otse siduda siltide seadmed.
3. Teatiste saatmine oma rakenduse kirjutamata teatis jaoturi abil

Siin on mõned mugavustega toonud arendajatele selle integratsiooniga.

- **Rakenduste Mobiilikliendi SDK-d.** Nende mitme platvormi SDK-d lihtne API-d pakuvad registreerimist ja rääkida teate jaoturi lingitud mobiilirakenduse abil automaatselt. Arendajad pole vaja kaudu teatis jaoturi identimisteabe tõrkeid ja täiendav töötamise.
    + Funktsiooni SDK-d automaatselt sildistamine seadmele mobiilirakenduste autenditud kasutaja ID-ga lubamiseks PTT kasutaja stsenaariumi.
    + Funktsiooni SDK-d kasutada automaatselt Mobile'i rakenduste installimine ID GUID registreerima teatis jaoturi, salvestades arendajad säilitada mitme teenuse GUID probleeme.
    
- **Installi mudel.** Mobiilirakenduste töötab teatis jaoturi viimase tõuketeatised mudeli tähistada kõik tõuketeatised atribuudid, mis on seotud seadme JSON installi, mis joondatakse tõuketeatised teatis teenustega ja on lihtne kasutada.

- **Paindlikkust.** Arendajad saate alati teatis jaoturi otse töötada isegi integratsiooniga kohas.

- **Integreeritud teenuses [Azure portaali].** Tõuketeatised on võimalus on esindatud visuaalselt mobiilirakenduste kohta ning arendajate saate hõlpsalt töötada seotud teate jaoturi mobiilirakenduste kaudu.



##<a name="next-steps"></a>Järgmised sammud

Lisateavet teate jaoturi leiate teemadest:

+ **[Kuidas kliendid on teatis jaoturi abil]**

+ **[Teatis jaoturi õpetused ja juhikud]**

+ **Teatis jaoturi alustamine õppetükid** ([iOS], [Android], [universaalne Windowsi], [Windows Phone], [Kindle], [Xamarin.iOS], [Xamarin.Android])

Oluline .NET hallatav API viited jaoks Tõuketeatiste asuvad siin:

+ [Microsoft.WindowsAzure.Messaging.NotificationHub]
+ [Microsoft.ServiceBus.Notifications]


  [0]: ./media/notification-hubs-overview/registration-diagram.png
  [1]: ./media/notification-hubs-overview/notification-hub-diagram.png
  [Kuidas kliendid on teatis jaoturi abil]: http://azure.microsoft.com/services/notification-hubs
  [Teatis jaoturi õppematerjalid ja juhikud]: http://azure.microsoft.com/documentation/services/notification-hubs
  [iOS-i]: http://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started
  [Android]: http://azure.microsoft.com/documentation/articles/notification-hubs-android-get-started
  [Windowsi universaalne]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-get-started
  [Windows Phone]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-phone-get-started
  [Kindle]: http://azure.microsoft.com/documentation/articles/notification-hubs-kindle-get-started
  [Xamarin.iOS]: http://azure.microsoft.com/documentation/articles/partner-xamarin-notification-hubs-ios-get-started
  [Xamarin.Android]: http://azure.microsoft.com/documentation/articles/partner-xamarin-notification-hubs-android-get-started
  [Microsoft.WindowsAzure.Messaging.NotificationHub]: http://msdn.microsoft.com/library/microsoft.windowsazure.messaging.notificationhub.aspx
  [Microsoft.ServiceBus.Notifications]: http://msdn.microsoft.com/library/microsoft.servicebus.notifications.aspx
  [Rakenduse teenuse mobiilirakendused]: https://azure.microsoft.com/en-us/documentation/articles/app-service-mobile-value-prop/
  [Mallid]: notification-hubs-templates-cross-platform-push-messages.md
  [Azure'i portaal]: https://portal.azure.com
  [Sildid]: (http://msdn.microsoft.com/library/azure/dn530749.aspx)
