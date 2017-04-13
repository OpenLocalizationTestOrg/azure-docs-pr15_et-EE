<properties
    pageTitle="v2.0 lõpp-punkti piirangud ja piirangutega | Microsoft Azure'i"
    description="Loendi piirangud ja piirangutega koos Azure AD v2.0 lõpp-punkti."
    services="active-directory"
    documentationCenter=""
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/30/2016"
    ms.author="dastrock"/>

# <a name="should-i-use-the-v20-endpoint"></a>Kasutada v2.0 lõpp-punkti?

Kui hoone rakendusi, mis integreerimine Azure Active Directory, peate otsustada, kas v2.0 lõpp-punkti ja autentimise Protokollid ei vasta teie vajadustele.  Algne Azure AD lõpp-punkti on endiselt täielikult toetatud ja mõnes osas on veel rikkalikud kui v2.0.  Siiski, selle v2.0 lõpp-punkti [tutvustab märkimisväärsed eelised](active-directory-v2-compare.md) arendajatele, võib ohtlikku uue programmeerimise mudeli kasutamine.

Aeg, meie soovitus kasutamine v2.0 lõpp-punkti kohta on järgmine:

- Kui soovite rakenduse toetamiseks isikliku Microsofti kontot, kasutage v2.0 lõpp-punkti.  Kuid veenduge, et eriti seotud konkreetselt töö ja kooli kontod selles artiklis toodud piirangud on teile arusaadavad.
- Kui teie rakendus nõuab ainult tugiandmeid töö ja kooli kontod, peaksite kasutama [algse Azure AD lõpp-punktid](active-directory-developers-guide.md).

Aja jooksul kasvab v2.0 lõpp-punkti siin loetletud piirangute kõrvaldamiseks nii, et ainult kunagi peate kasutama v2.0 lõpp-punkti.  Selles artiklis on vahepeal ette nähtud aitavad teil kindlaks teha, kas v2.0 lõpp-punkti on teie jaoks.  Jätkame aja jooksul kajastavad v2.0 lõpp-punkti, seega kontrollige uuesti, et teie nõuete suhtes v2.0 võimaluste reevaluate artiklit värskendamiseks.

Kui teil on olemasolev rakendus Azure AD, mis ei kasuta v2.0 lõpp-punkti, ei ole vaja alustada nullist.  Tulevikus meil pakkuda võimalust lubada olemasoleva Azure AD rakenduste v2.0 lõpp-punkti jaoks.

## <a name="restrictions-on-apps"></a>Rakenduste piirangud
Järgmist tüüpi rakendusi ei toeta praegu v2.0 lõpp-punkti.  Toetatud tüüpi rakendusi kirjeldus, vaadake [käesoleva artikli](active-directory-v2-flows.md).

##### <a name="standalone-web-apis"></a>Autonoomse Web API-d
V2.0 lõpp-punkti teil võimalus [koostada Web API, mis on turvatud OAuthi 2.0 abil](active-directory-v2-flows.md#web-apis).  Siiski, selle veebi-API ainult võimalik saada sõned rakendusest, mis jagab sama rakenduse Id.  Ehitamine on veebi-API, mis on klientrakenduses erinevate rakenduste Id-ga ei toetata.  Kliendi ei saa taotleda või saada oma veebi-API õigused.

Kuidas luua veebi-API-ga, mida aktsepteerib sõned klientrakenduses sama rakenduse Id-ga, leiate artiklist v2.0 lõpp-punkti Veebiteenuste näidised [Alustamine](active-directory-appmodel-v2-overview.md#getting-started).

##### <a name="web-api-on-behalf-of-flow"></a>Web API-nimel-kohta meilivoo
Mitme arhitektuurides kaasata veebi-API-ga, mis tuleb kõne teise järgneval Web API, mõlemad turvatud v2.0 lõpp-punkt.  See stsenaarium on levinud kohalikke klientidele, mis on Veebiteenuste kirjutamata, mis omakorda kõned Microsoft Online'i teenuse või mõne muu sisseehitatud kohandatud veebi-API, mis toetab Azure AD.

Sel juhul saate toetatud OAuthi 2.0 Jwt esitaja mandaati anda, muidu tuntud On-Behalf-Of Flow abil.  Siiski voo sees-nimel-, on pole praegu toetatud v2.0 lõpp-punkti.  See vool üldiselt kättesaadav Azure toimimise vaatamiseks AD teenuse, märkige ruut välja [-nimel-kohta kood valimi github](https://github.com/AzureADSamples/WebAPI-OnBehalfOf-DotNet).

## <a name="restrictions-on-app-registrations"></a>Rakenduse registreerimise piirangud
Aeg, tuleb kõik rakendused, mida soovite integreerida v2.0 lõpp-punkti luua uue rakenduse registreerimine [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList).  Mis tahes olemasolevaid Azure AD või Microsofti Account rakendused ei ühildu v2.0 lõpp-punkti ega on mis tahes portaali Lisaks uue rakenduse registreerimise portaali registreeritud rakendused.  Plaanis võimalda lubada olemasoleva rakenduste v2.0 rakenduse kasutamiseks. Sel ajal on küll, pole migreerimise tee rakenduse v2.0 lõpp-punkti.

Samuti ei tööta rakendused, mis on registreeritud uue rakenduse registreerimise portaalis vastu algse Azure AD autentimise lõpp-punkti.  Aga abil saate rakenduse registreerimise portaali loodud rakendustele edukalt integreerida Microsofti konto autentimine lõpp-punkti, `https://login.live.com`.

Lisaks on loodud [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) rakenduse registreerimise järgmised piirangutega tehke järgmist.

- Atribuudi **kodulehele** ka **sisselogimise URL-i** ei toetata.  Koduleht, ilma neid rakendusi ei kuvata paanil minu Office'i rakendused.
- Rakenduse Id sel ajal kohta on lubatud ainult kahe rakenduse saladusi.
- Mille rakenduse registreerimine saab ainult vaadata ja hallata ühe arendaja konto.  See ei saa ühiselt mitu arendajatele.
- On mitmeid piiranguid redirect_uri lubatud vorming.  Vt lisateavet järgmisest jaotisest.

## <a name="restrictions-on-redirect-uris"></a>Redirect URI-d piirangud
Rakendusi, mis on juba registreeritud uue rakenduse registreerimise portaalis on praegu piiratud piiratud redirect_uri väärtuste kogumist.  Redirect_uri web rakenduste ja teenuste jaoks peab algama kava `https`, ja kõik redirect_uri väärtused peavad jagada ühe DNS-i domeeni.  Näiteks ei ole võimalik registreerida web appi, mis sisaldab redirect_uris:

`https://login-east.contoso.com`  
`https://login-west.contoso.com`

Registreerimise võrdleb kogu DNS-i nimi olemasoleva redirect_uri redirect_uri, lisatava DNS-i nimi. DNS-i nimi lisada taotluse nurjub, kui üks järgmistest tingimustest on täidetud:  

- Kui kogu DNS-i nimi uue redirect_uri ei sobi olemasoleva redirect_uri DNS-i nimi
- kui uus redirect_uri kogu DNS-i nimi pole olemasoleva redirect_uri alamdomeeni

Oletagem näiteks, kui rakendus on praegu redirect_uri:

`https://login.contoso.com`

Siis on võimalik lisada:

`https://login.contoso.com/new`

mis vastab täpselt DNS-i nimi või:

`https://new.login.contoso.com`

mis on DNS-i alamdomeen login.contoso.com.  Kui soovite rakenduse Logi sisse – east.contoso.com, millel on ja Logi sisse – west.contoso.com redirect_uris, siis peate lisama järgmised redirect_uris järjestuses.

`https://contoso.com`  
`https://login-east.contoso.com`  
`https://login-west.contoso.com`  

Kaks viimast saate lisada, kuna need on esimene redirect_uri, domeenide contoso.com. Seda piirangut eemaldatakse on eelseisvad väljaanne.

Saate teada, kuidas registreerida rakenduse uue rakenduse registreerimise portaalis, vaadake [käesoleva artikli](active-directory-v2-app-registration.md).

## <a name="restrictions-on-services--apis"></a>Teenuste ja API-de piirangud
V2.0 lõpp-punkti praegu toetab sisselogimine just mõne uue rakenduse registreerimise portaalis, kui see on registreeritud rakenduse kuulub [toetatud autentimise puhul](active-directory-v2-flows.md)loendit.  Need rakendused on ainult siiski võiks saada OAuth 2.0 Accessi sõned väga piiratud hulk ressursse.  V2.0 lõpp-punkti annab ainult access_tokens jaoks:

- Luba soovitud rakendus.  Rakenduse saate hankida mõne access_token ise, kui loogiline rakenduse koosneb mitme eri osade või astme.  See stsenaarium tegelikkuses vaatamiseks vaadake meie [Alustamine](active-directory-appmodel-v2-overview.md#getting-started) õpetused.
- Outlooki e-posti, kalendri ja kontaktide REST API-d, mis asuvad https://outlook.office.com.  Saate teada, kuidas kirjutada rakendus, mis kasutab neid API-d, vaadake olümpiaandmed [Office alustamine](https://www.msdn.com/office/office365/howto/authenticate-Office-365-APIs-using-v2) .
- Microsoft Graphi API-d.  Lugege Microsoft Graphi ja kõik andmed, mis on saadaval, külastage [https://graph.microsoft.io](https://graph.microsoft.io).

Praegu on toetatud pole muude teenustega.  Lisateavet Microsoft Online services lisatakse tulevikus ka teie enda loodud kohandatud ehitatud Web API-de ja teenuste tugi.

## <a name="restrictions-on-libraries--sdks"></a>Teekide ja SDK-d piirangud
Praegu on üsna piiratud teegi tugi v2.0 lõpp-punkti.  Kui soovite kasutada v2.0 lõpp-punkti tootmise rakenduses, on teil järgmised valikud.

- Kui teil on veebirakenduse loomine, saate turvaline meie üldiselt kättesaadav serveripoolne vahevara teha Logi sisse ja sümboolne valideerimine.  Nendeks OWIN avatud ID ühenduse vahevara ASP.net-i ja meie NodeJS pass lisandmoodul.  Meie vahevara proovi kood on saadaval jaotises meie [Alustamine](active-directory-appmodel-v2-overview.md#getting-started) ka.
- Muude platvormide jaoks loodud ja kohalikke & Mobile'i rakendused, saate ka integreerida koos v2.0 lõpp-punkti otse saatmine & rakenduse koodis sõnumeid Protocol (protokoll).  V2.0, OpenID ühendamine ja OAuthi Protokollid [konkreetselt dokumenteerida](active-directory-v2-protocols.md) aitavad teha mõne sellise integratsiooni.
- Lõpuks saate integreerida v2.0 lõpp-punkti avatud andmeallika avamine ID ühenduse loomiseks ja OAuthi teekide abil.  Protokoll v2.0 peaks ühilduma mitme avatud allika protokolli teekide ilma peamistest muudatustest.  Sellise teekide kättesaadavus sõltub keele- ja platvormi kohta ja [Avatud ID ühenduse loomiseks](http://openid.net/connect/) ja [OAuth 2.0](http://oauth.net/2/) veebisaitide hallata populaarsed rakenduste loendit. Teemast [Azure Active Directory (AD) v2.0 ja autentimise teekide](active-directory-v2-libraries.md) rohkem üksikasju ja avatud allika kliendi teekide ja näidised, mis on v2.0 lõpp-punkti loendit.

Meil on ka antud mõne [Microsoft autentimine teek (MSAL)](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) algse eelvaate .NET ainult.  Olete oodatud selle teegi .net-i klient ja server rakendustes katsetada, kuid kui see pole kaasas GA kvaliteediga eelvaade teegi toetavad.

## <a name="restrictions-on-protocols"></a>Protokollid piirangud
Lõpp-punkti v2.0 toetab vaid avatud ID ühenduse & OAuth 2.0.  Siiski kõiki funktsioone ja võimalusi iga protokolli on ühendatud v2.0 lõpp-punkti, sh:

- OpenID ühendamine `end_session_endpoint`, mis võimaldab rakenduse v2.0 lõpp-punkti kasutaja seansi lõpetamiseks.
- välja v2.0 lõpp-punkti id_tokens sisaldada ainult paariviisiline identifikaator kasutaja jaoks.  See tähendab, et kahe erinevates rakendustes saate eri jaoks sama kasutaja ID-d.  Pange tähele, et Microsoft Graphi päringud, `/me` lõpp-punkti, siis saab kasutaja, mida saab kasutada kõigis rakendustes correlatable ID saamiseks.
- välja v2.0 lõpp-punkti id_tokens ei sisalda mõnda `email` taotlemine kasutaja sel ajal, isegi juhul, kui kasutaja õigus vaadata oma e-posti viimistlemine.
- OpenID ühenduse Kasutajateave lõpp-punkti. Kasutajateave lõpp-punkti pole praegu v2.0 lõpp-punkti juurutatud.  Siiski kõik kasutajaprofiili andmete potentsiaalselt saaks selle lõpp-punkti veebisaidil on saadaval Microsoft Graphi `/me` lõpp-punkti.
- Roll ja rühma taotluste.  Sel ajal, v2.0 lõpp-punkti ei toeta väljastanud rolli või rühma taotluste id_tokens.

Paremini mõistmiseks Protocol (protokoll) funktsioonid on toetatud v2.0 lõpp-punkti ulatuse, lugeda meie [OpenID ühendus ja OAuthi 2.0 protokolli viide](active-directory-v2-protocols.md).

## <a name="restrictions-for-work--school-accounts"></a>Töö ja kooli kontod piirangud
Mõned funktsioonid on teatud Microsoft ettevõtte/äri kasutajatele, mis ei toeta veel v2.0 lõpp-punkti.

##### <a name="device-based-conditional-access-native-and-mobile-apps-and-the-microsoft-graph"></a>Microsoft Graphi Device põhineva juurdepääsu ja kohalikke ja Mobile'i rakendused
V2.0 lõpp-punkti ei toeta veel mobiili- ja kohalikke rakendused, näiteks kohalikke rakendusi operatsioonisüsteemis iOS või Android seadme autentimist.  See võib blokeerida oma kohalikus rakenduses Microsoft Graphi kutsudes teatud ettevõtete jaoks.  Seadme autentimine on nõutav, kui administraator määrab device põhineva juurdepääsu poliitika kohta.  V2.0 lõpp-punkti on kõige tõenäolisemad stsenaarium seadme põhinev tingimusvormingu juurdepääsu seadmine poliitika ressursi Microsoft Graphi, nt Outlooki API administraator.  Kui administraator määrab selle poliitika ja taotleb oma kohalikus rakenduses Microsoft Graphi sümboolse, taotluse lõpuks nurjub, kuna seadme autentimise veel ei toetata.  Koosolekukutse Microsoft Graphi, sõned veebirakenduste siiski on toetatud, kui poliitikat seade on konfigureeritud.  Web Appi stsenaariumi seadme autentimise tehakse kasutaja veebibrauseri kaudu.

Arendaja, siis tõenäoliselt ei saa kontrollida, kui poliitika on seatud Microsoft Graphi ressursid, või isegi teada, kui see juhtub.  Kui olete hoone rakenduse töö ja kooli kasutajate jaoks, peaksite kasutama [algse Azure AD lõpp-punkti](active-directory-developers-guide.md) kuni v2.0 lõpp-punkti toetab seadme autentimist.  Device põhineva juurdepääsu kohta lisateabe saamiseks vaadake [artiklit](active-directory-conditional-access.md#device-based-conditional-access).

##### <a name="windows-integrated-authentication-for-federated-tenants"></a>Windowsi integreeritud autentimine ühendatud rentnikega
Kui olete kasutanud Windowsi rakendustes ADAL (ja seega algse Azure AD lõpp-punkti), võib võtta ära nn SAML kinnituse anda.  See võimaldab kasutajatel ühendatud Azure AD rentnikud vaikselt autentimiseks oma kohapealne Active Directory eksemplar ilma sisestage oma kasutajanimi ja parool.  Praegu ei toetata SAML kinnituse anda v2.0 lõpp-punkti.
