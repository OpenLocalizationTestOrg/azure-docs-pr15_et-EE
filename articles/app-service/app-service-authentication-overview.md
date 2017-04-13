<properties
    pageTitle="Autentimise ja luba teenuses Azure rakendus | Microsoft Azure'i"
    description="Kontseptuaalne viide ja ülevaade autentimine / autoriseerimine funktsioon Azure'i rakenduse teenuse jaoks"
    services="app-service"
    documentationCenter=""
    authors="mattchenderson"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="08/29/2016"
    ms.author="mahender"/>

# <a name="authentication-and-authorization-in-azure-app-service"></a>Autentimise ja luba teenuses Azure rakendus

## <a name="what-is-app-service-authentication--authorization"></a>Mis on rakenduse autentimise / loa?

Rakenduse autentimise / autoriseerimine on funktsioon, mis võimaldab rakenduse kasutajad sisse logida, nii, et teil pole vaja muuta koodi kirjutamata rakendus. Selle abil on lihtne kaitsta teie taotlus ja kasutaja andmeid.

Rakenduse teenus kasutab, kus kolmanda osapoole identiteedipakkuja talletab kontod ja autendib kasutajad ühendatud andmed. Rakendus sõltub teenusepakkuja identiteedi teavet, et rakendus pole info talletamiseks. Teenuse rakendus toetab viis identiteedipakkujad välja kasti: Azure Active Directory, Facebooki, Google, Account Microsoft ja Twitteri. Rakenduse saate kasutada mis tahes arvu need identiteedipakkujad võimalusi, kuidas need logige sisse oma kasutajatele. Sisseehitatud tugi laiendamiseks saate integreerida teise identiteedipakkuja või [oma kohandatud identiteedi lahenduse][custom-auth].

Kui soovite kohe alustada, leiate järgmistest õpetused.

- [Autentimise lisamine oma iOS-i rakenduse] [ iOS] (või [Android], [Windows], [Xamarin.iOS], [Xamarin.Android], [Xamarin.Forms]või [Cordova])
- [Kasutaja autentimisega API rakendused teenuses Azure rakendus][apia-user]
- [Azure'i rakendust Service – osa 2 alustamine][web-getstarted]

## <a name="how-authentication-works-in-app-service"></a>Rakenduse teenuses autentimise tööpõhimõtted

Selleks, et autentida, kasutades ühte järgmistest soovitud identiteedipakkujad, peate esmalt konfigureerimine identiteedipakkuja rakenduse kohta teadma. Identiteedi pakkuja annab seejärel ID-d ja saladusi, et anda rakenduse teenus. See lõpetab usalda seose nii, et rakendus teenuse valideerib kasutaja kinnitused, nt autentimine märkide identiteedipakkuja kaudu.

Mõne loetletud teenusepakkuja abil kasutaja sisse logida, peab kasutaja lõpp-punkt, milles on selle pakkuja kasutajad ümber. Kui kliendid on veebibrauseri kaudu, peate rakenduse teenuse automaatselt suunata kõik autentimata kasutajad lõpp-punkt, milles on kasutajad. Muul juhul peate klientide suunamiseks `{your App Service base URL}/.auth/login/<provider>`, kus `<provider>` on üks järgmistest väärtustest: aad, Facebooki, google, microsoft või twitter. Selle artikli jaotist selgitatakse Mobile ja API stsenaariumid.

Kasutajad, kes teie taotlus veebibrauseri kaudu suhelda on küpsis määramine, et nad võivad jääda autenditud, kui nad sirvida rakenduse. Kliendi muude, nt mobiil, JSON web luba (JWT), mis on toodud selle `X-ZUMO-AUTH` päises väljastatakse kliendile. Mobiilirakenduste kliendi SDK-d käsitleda seda teie eest. Teine võimalus on Azure Active Directory identiteedi luba või juurdepääsu luba võib otse lisada soovitud `Authorization` päise kui [esitaja luba](https://tools.ietf.org/html/rfc6750).

Rakenduse teenuse kinnitada mis tahes küpsise või märku, et rakenduse probleemide kasutajate autentimiseks. Piirata, kellel on juurdepääs teie rakendus, selle artikli jaotisest [autoriseerimine](#authorization) .

### <a name="mobile-authentication-with-a-provider-sdk"></a>Mobiilne autentimine pakkujaga SDK

Pärast kõik konfigureerimist selle kirjutamata, saate muuta mobiilikliendid rakenduse teenuse sisse logima. On siin kaks võimalust:

- Kasutage Tarkvaraarenduskomplektist, mis antud identiteedipakkuja avaldab identiteedi ja seejärel pääseda rakenduse teenus.
- Kasutage üks rida koodi nii, et mobiilirakenduste kliendi SDK saavad kasutajad sisse logida.

>[AZURE.TIP] Enamik rakendusi tuleks pakkuja SDK abil saate ühtsema kogemus kui kasutajate sisselogimist, värskendamise tugiteenuste kasutamine ja muud eelised, mis määrab pakkuja saada.

Kui kasutate pakkuja SDK, kasutajad saavad sisse logida kogemusi, mis ühendab tihedalt rakendus töötab operatsioonisüsteem. See annab teile märgiks pakkuja ja mõni kasutaja teabe klient, mis hõlbustab palju tarbimine Graphi API-d ja kasutusvõimaluse kohandada. Aeg-ajalt kuvatakse Ajaveebid ja foorumid, seda nimetatakse "kliendi voogu" või "kliendi suunatud meilivoo", kuna kliendi koodi sisse logib kasutajate ja kliendi kood on pakkuja Luba juurdepääs.

Pärast pakkuja märgiks on vaja rakendust Service valideerimisreeglite saata. Pärast teenuse rakendus kinnitatakse luba, rakenduse teenus loob uue rakenduse teenuse loa, mis tagastatakse kliendile. Mobiilirakenduste kliendi SDK on helper meetodid selle Exchange'i haldamiseks ja automaatselt lisada luba kõik taotluste rakenduse taustväärtus. Arendajad saate pakkuja luba viide alles jätta, kui nad soovivad.

### <a name="mobile-authentication-without-a-provider-sdk"></a>Ilma pakkuja SDK mobiilne autentimine

Kui te ei soovi luua pakkuja SDK, saate lubada mobiilirakenduste funktsioon Azure'i rakendust Service saate sisse logida. Mobiilirakenduste kliendi SDK avaneb veebivaate enda valitud Meilikausta pakkuja ja kasutaja sisselogimine. Aeg-ajalt kuvatakse Ajaveebid ja foorumid, seda nimetatakse "serveri meilivoo" või "server suunatud meilivoo" Kuna server haldab protsessi, et kasutajatel märke ja saab klient SDK kunagi luba pakkuja.

Koodi käivitamiseks see vool on kaasatud autentimine õpetuse iga platvormi. Voo lõpus kliendi SDK rakenduse teenus on luba ja luba lisatakse automaatselt kõik taotluste rakenduse taustväärtus.

### <a name="service-to-service-authentication"></a>Teenusest autentimine

Kuigi saate anda kasutajatele juurdepääsu rakenduse, võite usaldada mõnda muusse rakendusse helistamiseks oma API. Näiteks võib teil olla üks web appi API kõne teise web Appis. Selle stsenaariumi korral saate identimisteabe teenuse konto asemel kasutajatunnust märgiks saada. Teenusele konto on ka *teenuse põhisumma* Azure Active Directory kõnepruugis ja autentimist kasutava sellise konto on ka teenusest stsenaarium.

>[AZURE.IMPORTANT] Kuna mobiilirakendused käivitada kliendi seadmetes, mobiilirakenduste ei _Loe, usaldusväärsete rakenduste ja teenuse põhisumma vool ei tohi kasutada_ . Selle asemel kasutada kasutaja kulgemist, mis on üksikasjalik varasemas versioonis.

Teenusest stsenaariumid, saate rakendust Service rakenduse Azure Active Directory abil kaitsta. Helistaja rakendus peab lihtsalt pakuvad Azure Active Directory teenuse põhisumma autoriseerimine luba, mis on saadud esitada kliendi ID ja kliendi salajane Azure Active Directory kaudu. See stsenaarium, mis kasutab ASP.net-i API rakenduste näide seletab õpetuse, [põhisumma autentimise API rakenduste][apia-service].

Kui soovite kasutada rakenduse autentimise hallata-teenusest stsenaarium, saate kliendi sertide või lihttekstautentimine. Kliendi sertide Azure kohta leiate teavet teemast [Kuidas soovite konfigureerida TLS vastastikune autentimine Web Apps](../app-service-web/app-service-web-configure-tls-mutual-auth.md). Elementaarautentimine ASP.net-i kohta leiate teavet teemast [Autentimise filtrid ASP.net-i veebi-API 2](http://www.asp.net/web-api/overview/security/authentication-filters).

Teenuse konto autentimine rakendus App teenuse loogika API rakendus on eriline juhtum, mis on üksikasjalikult kirjeldatud [kaudu oma kohandatud API majutatud rakenduse teenuse loogika rakendustega](../app-service-logic/app-service-logic-custom-hosted-api.md).

## <a name="authorization"></a>Luba rakenduse teenuses tööpõhimõtted

Teil on taotlusi, millele pääsete juurde rakenduse üle täielik kontroll. Rakenduse autentimise ja luba saab konfigureerida mis tahes järgmistest probleemidest:

- Luba ainult autenditud taotlusi rakenduse saavutamiseks.

    Kui brauser saab taotluse Anonüümne, suunab rakenduse teenuse lehele identiteedi pakkuja, mille valimisel nii, et kasutajad saavad sisse logida. Kui kutse on pärit mobiilsideseadme, on tagastatud vastus HTTP _401 volitamata_ vastus.

    See suvand, ei pea üldse rakenduse mis tahes autentimise koodi kirjutamiseks. Kui teil on vaja peenem autoriseerimine teavet kasutaja on saadaval oma koodi.

- Luba kõik kutsed jõuda rakenduse, kuid valideerida autenditud taotleb, ja mööda autentimine on HTTP päistes leiduv teave.

    See suvand asukohta oma rakenduse koodi autoriseerimine otsuseid. See pakub rohkem paindlikkust anonüümse päringute töötlemise, kuid peate koodi kirjutamist.

- Luba kõik kutsed rakenduse saavutamiseks ja taotluste autentimise teavet pole midagi teha.

    Sel juhul, autentimine / autoriseerimine funktsioon on välja lülitatud. Autentimise ja luba ülesanded on täiesti kuni teie taotlus koodi.

Eelmise käitumist juhitakse **toiming taotluse pole autenditud** suvand Azure'i portaalis. Kui valite * *sisse logida *pakkuja nimi* **, kõik nõuded tuleb autentida.** Luba taotluse (midagi) ** asukohta autoriseerimine otsust oma koodi, kuid see annab siiski autentimise teavet. Kui soovite oma hakkama kõik koodi, saate keelata autentimine / autoriseerimine funktsiooni.

## <a name="working-with-user-identities-in-your-application"></a>Kasutaja identiteedi oma rakenduse töötamine

Rakenduse teenuse edastab kasutaja teabe mõned rakenduse abil teisiti päised. Välise taotlusi keelata need päised ja ainult Esita kui seatakse rakenduse teenuse autentimise / autoriseerimine. Mõni näide päised on järgmised.

* X-MS-KLIENDI--TURVASUBJEKTINIMI
* X-MS-KLIENDI-PÕHISUMMA-ID
* X-MS-TOKEN-FACEBOOK-ACCESS-TOKEN
* X-MS-TOKEN-FACEBOOK-EXPIRES-ON

Kood, mis on kirjutatud framework või keele saad teavet, mida on vaja need päised. ASP.net-i 4.6 rakenduste **ClaimsPrincipal** seatakse automaatselt väärtused.

Rakenduse saab ka täiendavad kasutaja üksikasjad kaudu HTTP GET soovitud `/.auth/me` rakenduse lõpp-punkti. Lubatud luba, mis on kaasas kutse tagasi JSON last pakkuja, mida kasutatakse üksikasjad, aluseks oleva pakkuja luba ja mõne muu kasutaja teabe. Mobiilirakenduste serveri SDK-d pakuvad helper meetodid, et neid andmeid. Lisateabe saamiseks vt [Node.js SDK Azure'i Mobile'i rakenduste kasutamise kohta](../app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#howto-tables-getidentity)ja [.NET taustväärtus serveri SDK Azure'i mobiilirakenduste töötamine](../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#user-info).

## <a name="documentation-and-additional-resources"></a>Dokumendid ja täiendavad ressursid

### <a name="identity-providers"></a>Identiteedipakkujad
Järgmised õppetükid kuvamine rakenduse teenust kasutada eri autentimisteenuse pakkujate konfigureerimine:

- [Kuidas konfigureerida rakenduse kasutamiseks Azure Active Directory sisselogimine][AAD]
- [Kuidas konfigureerida rakenduse kasutamiseks Facebook login][Facebook]
- [Kuidas konfigureerida rakenduse kasutamiseks Google sisselogimine][Google]
- [Kuidas konfigureerida rakenduse kasutamiseks Microsoft Account sisselogimine][MSA]
- [Kuidas konfigureerida rakenduse kasutamiseks Twitteri login][Twitter]

Kui soovite kasutada mõnda identiteedi süsteemi peale need esitatud siin, saate kasutada ka [eelvaate kohandatud autentimise tugi Mobile'i rakendused .net-i serveri SDK][custom-auth], mida saab kasutada veebirakenduste, mobiilirakendused või API rakendused.

### <a name="web-applications"></a>Veebirakenduste
Järgmised õppetükid näitab, kuidas lisada autentimine veebirakenduse:

- [Azure'i rakendust Service – osa 2 alustamine][web-getstarted]

### <a name="mobile-applications"></a>Mobiilirakenduste
Järgmised õppetükid Kuva autentimise lisamine oma mobiilikliendid serveri suunatud voo abil:

- [Oma iOS-i rakenduse autentimise lisamine][iOS]
- [Androidi rakenduse autentimise lisamine] [Android]
- [Rakenduse Windowsi autentimist lisamine] [Windows]
- [Lisa rakenduse Xamarin.iOS autentimine] [Xamarin.iOS]
- [Lisa rakenduse Xamarin.Android autentimine] [Xamarin.Android]
- [Lisa rakenduse Xamarin.Forms autentimine] [Xamarin.Forms]
- [Rakenduse Cordova autentimise lisamine] [Cordova]

Kui soovite Azure Active Directory kulgemist kliendi suunatud kasutamiseks, kasutage järgmisi ressursse:

- [IOS-i kasutamine Active Directory Authentication Library][ADAL-iOS]
- [Kasutage Active Directory Authentication Library Androidi jaoks][ADAL-Android]
- [Kasutage Windows ja Xamarin Active Directory Authentication Library][ADAL-dotnet]

Kui soovite kasutada kliendi suunatud voo Facebook, kasutage järgmisi ressursse:

- [Facebooki SDK iOS-i kasutamine](../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#facebook-sdk)

Kui soovite kliendi suunatud voo kasutamiseks Twitteri, kasutage järgmisi ressursse:

- [IOS-i struktuuri Twitteri kasutamine](../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#twitter-fabric)

Kui soovite kliendi suunatud voo kasutamiseks Google, kasutage järgmisi ressursse:

- [IOS-i SDK Google sisselogimise kasutamine](../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#google-sdk)

### <a name="api-applications"></a>API rakendused
Järgmised õppetükid Kuva API rakenduste kaitsmise kohta:

- [Kasutaja autentimisega API rakendused teenuses Azure rakendus][apia-user]
- [Peamine autentimise API rakendused teenuses Azure rakendus][apia-service]









[apia-user]: ../app-service-api/app-service-api-dotnet-user-principal-auth.md
[apia-service]: ../app-service-api/app-service-api-dotnet-service-principal-auth.md

[web-getstarted]: ../app-service-web/app-service-web-get-started-2.md#authenticate-your-users

[iOS]: ../app-service-mobile/app-service-mobile-ios-get-started-users.md
[Android]: ../app-service-mobile/app-service-mobile-android-get-started-users.md
[Xamarin.iOS]: ../app-service-mobile/app-service-mobile-xamarin-ios-get-started-users.md
[Xamarin.Android]: ../app-service-mobile/app-service-mobile-xamarin-android-get-started-users.md
[Xamarin.Forms]: ../app-service-mobile/app-service-mobile-xamarin-forms-get-started-users.md
[Windows]: ../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-users.md
[Cordova]: ../app-service-mobile/app-service-mobile-cordova-get-started-users.md

[AAD]: ../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md
[Facebook]: ../app-service-mobile/app-service-mobile-how-to-configure-facebook-authentication.md
[Google]: ../app-service-mobile/app-service-mobile-how-to-configure-google-authentication.md
[MSA]: ../app-service-mobile/app-service-mobile-how-to-configure-microsoft-authentication.md
[Twitter]: ../app-service-mobile/app-service-mobile-how-to-configure-twitter-authentication.md

[custom-auth]: ../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#custom-auth

[ADAL-Android]: ../app-service-mobile/app-service-mobile-android-how-to-use-client-library.md#adal
[ADAL-iOS]: ../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#adal
[ADAL-dotnet]: ../app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library.md#adal
