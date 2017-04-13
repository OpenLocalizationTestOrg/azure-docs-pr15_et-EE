<properties
    pageTitle="Autentimise ja luba Azure rakendustes | Microsoft Azure'i"
    description="Kontseptuaalne viide ja ülevaade autentimine / autoriseerimine funktsioon Azure'i mobiilirakenduste kohta"
    services="app-service\mobile"
    documentationCenter=""
    authors="mattchenderson"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="mahender"/>

# <a name="authentication-and-authorization-in-azure-mobile-apps"></a>Autentimise ja luba Azure rakendustes

## <a name="what-is-app-service-authentication--authorization"></a>Mis on rakenduse autentimise / loa?

> [AZURE.NOTE] Selles teemas migreeritakse soovite konsolideeritud [rakenduse autentimise / autoriseerimine](../app-service/app-service-authentication-overview.md) teema, mis hõlmab Web ja Mobile API rakendused.

Rakenduse autentimise / autoriseerimine on funktsiooni, mis võimaldab rakenduse abil muutusteta koodi kirjutamata rakenduse nõutavad kasutajad sisse logida. Selle abil on lihtne kaitsta teie taotlus ja kasutaja andmeid.

Rakenduse teenus kasutab ühendatud identiteedi, milles 3-osaline **identiteedipakkuja** ("IDP") talletab kontod ja autendib kasutajad, ja rakendus kasutab seda isikut, mitte eraldi. Teenuse rakendus toetab viis identiteedipakkujad välja kasti: _Azure Active Directory_, _Facebooki_, _Google_, _Microsofti konto_ja _Twitteri_. Mõne muu identiteedipakkuja või oma kohandatud identiteedi lahenduse saate laiendada ka rakenduste tugi.

Rakenduse saate kasutada mis tahes arvu need identiteedipakkujad nii, et saate sisestada oma lõppkasutajad võimalusi, kuidas need sisse logida.

Kui soovite kohe tööle asuda, lugege ühte järgmistest õpetused:

- [Oma iOS-i rakenduse lisamine autentimine]
- [Rakenduse Xamarin.iOS autentimise lisamine]
- [Rakenduse Xamarin.Android autentimise lisamine]
- [Rakenduse Windowsi autentimist lisamine]

## <a name="how-authentication-works"></a>Kuidas toimib autentimine

Autentida, kasutades ühte selle identiteedipakkujad, et peate esmalt konfigureerimine identiteedipakkuja rakenduse kohta teadma. Identiteedipakkuja seejärel annab teile ID-d ja saladusi, et anda tagasi rakendus. See on usaldusseos on lõpule jõudnud ja võimaldab rakenduse teenuse identiteedid esitatud selle kinnitamiseks.

Need juhised on üksikasjalikult kirjeldatud järgmisi teemasid:

- [Kuidas konfigureerida rakenduse kasutamiseks Azure Active Directory sisselogimine]
- [Kuidas konfigureerida rakenduse kasutamiseks Facebook login]
- [Kuidas konfigureerida rakenduse kasutamiseks Google sisselogimine]
- [Kuidas konfigureerida rakenduse kasutamiseks Microsoft Account sisselogimine]
- [Kuidas konfigureerida rakenduse kasutamiseks Twitteri login]

Kui kõik konfigureerimist selle kirjutamata, saate muuta oma klientrakenduse ja logige sisse. On siin kaks võimalust:

- Üks rida koodi kasutamisel lasta mobiilirakenduste kliendi SDK sisselogimine kasutajad.
- Saate kasutada ka SDK avaldatud antud identiteedipakkuja identiteedi ja seejärel pääseda rakenduse teenus.

>[AZURE.TIP] Enamiku rakenduste kasutada pakkuja SDK native-tunne login kogemus ja võimendada värskendamise tugi ja muu teenusepakkuja kohased eelised.

### <a name="how-authentication-without-a-provider-sdk-works"></a>Kuidas töötab ilma pakkuja SDK autentimine

Kui te ei soovi luua pakkuja SDK, saate lubada mobiilirakenduste login teha. Mobiilirakenduste kliendi SDK avaneb veebivaate pakkuja enda valitud Meilikausta ja rakendusse. Aeg-ajalt Ajaveebid ja foorumid, kuvatakse see viidatud "serveri meilivoo" või "server suunatud meilivoo" alates server on haldamise login ja kliendi SDK saab kunagi luba pakkuja.

Koodi vaja käivitage see vool käsitletakse iga platvormi õpetuse autentimist. Voo lõpus kliendi SDK mõnda rakendust Service luba ja luba lisatakse automaatselt kõik taotluste soovitud taustväärtus.

### <a name="how-authentication-with-a-provider-sdk-works"></a>Kuidas töötab pakkujaga SDK autentimine

Töötamine pakkuja SDK võimaldab sisse logida kogemus platvormi OS rakendus töötab tihedalt suhelda. See annab teile märgiks pakkuja ja mõni kasutaja teabe klient, mis hõlbustab palju tarbimine Graphi API-d ja kasutusvõimaluse kohandada. Aeg-ajalt kuvatakse Ajaveebid ja Foorumid seda nimetatakse "kliendi voogu" või "kliendi suunatud meilivoo" alates koodi klient on töötlemise login ja kliendi kood on pakkuja Luba juurdepääs.

Kui saadakse pakkuja luba, tuleb saata rakenduse teenuse valideerimisreeglite. Voo lõpus kliendi SDK mõnda rakendust Service luba ja luba lisatakse automaatselt kõik taotluste soovitud taustväärtus. Arendaja saab pakkuja luba viide alles jätta, kui nad soovivad.

## <a name="how-authorization-works"></a>Luba tööpõhimõtted

Rakenduse autentimise ja luba mitu valikut seab toimingu **tegemiseks, kui koosolekukutse pole autenditud**. Enne oma koodi saab antud taotluse, peate rakenduse teenuse kontrollimine, kui kutse on autenditud ja kui ei, hüljata ja püüdke on kasutajal proovida uuesti sisse logida.

Üheks võimaluseks on on autentimata taotlused suunata üks identiteedi pakkujad. Veebibrauseris, see võtaks tegelikult kasutaja uuele lehele. Siiski oma mobiilikliendi ei saa sel viisil ümber ja autentimata vastused saadetakse HTTP _401 volitamata_ vastuse. See antakse, teie klient teeb esimese taotluse tuleks alati login lõpp-punkti ja tehke kõned mis tahes API-d. Kui proovite kõne teisele API enne sisselogimist, kuvatakse teie kliendi tõrge.

Kui soovite, et täpsema juhtimine üle mis näitajaid nõuab autentimist, võite valida ka "pole toimingu (luba taotluse)" autentimata taotlused. Sel juhul kõik autentimise otsuseid oma rakenduse koodi edasi lükata. See võimaldab teil kohandatud autoriseerimine reeglite kindlatele kasutajatele juurdepääsu lubamine.

## <a name="documentation"></a>Dokumentatsioon

Järgmised õppetükid Kuva autentimise lisamine oma mobiilikliendid rakenduse teenuse abil:

- [Oma iOS-i rakenduse lisamine autentimine]
- [Rakenduse Xamarin.iOS autentimise lisamine]
- [Rakenduse Xamarin.Android autentimise lisamine]
- [Rakenduse Windowsi autentimist lisamine]

Järgmised õppetükid kuvamine rakenduse teenust kasutada eri autentimisteenuse pakkujate konfigureerimine:

- [Kuidas konfigureerida rakenduse kasutamiseks Azure Active Directory sisselogimine]
- [Kuidas konfigureerida rakenduse kasutamiseks Facebook login]
- [Kuidas konfigureerida rakenduse kasutamiseks Google sisselogimine]
- [Kuidas konfigureerida rakenduse kasutamiseks Microsoft Account sisselogimine]
- [Kuidas konfigureerida rakenduse kasutamiseks Twitteri login]

Kui soovite kasutada mõnda identiteedi süsteemi peale need esitatud siin, samuti saate kasutada [kohandatud autentimise tugi .net-i serveri SDK eelvaade](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#custom-auth).

[Oma iOS-i rakenduse lisamine autentimine]: app-service-mobile-ios-get-started-users.md
[Rakenduse Xamarin.iOS autentimise lisamine]: app-service-mobile-xamarin-ios-get-started-users.md
[Rakenduse Xamarin.Android autentimise lisamine]: app-service-mobile-xamarin-android-get-started-users.md
[Rakenduse Windowsi autentimist lisamine]: app-service-mobile-windows-store-dotnet-get-started-users.md

[Kuidas konfigureerida rakenduse kasutamiseks Azure Active Directory sisselogimine]: app-service-mobile-how-to-configure-active-directory-authentication.md
[Kuidas konfigureerida rakenduse kasutamiseks Facebook login]: app-service-mobile-how-to-configure-facebook-authentication.md
[Kuidas konfigureerida rakenduse kasutamiseks Google sisselogimine]: app-service-mobile-how-to-configure-google-authentication.md
[Kuidas konfigureerida rakenduse kasutamiseks Microsoft Account sisselogimine]: app-service-mobile-how-to-configure-microsoft-authentication.md
[Kuidas konfigureerida rakenduse kasutamiseks Twitteri login]: app-service-mobile-how-to-configure-twitter-authentication.md
