<properties
    pageTitle="Tüüpi v2.0 lõpp-punkti | Microsoft Azure'i"
    description="Tüüpi rakendusi ja stsenaariumid, mis ei toeta Azure AD v2.0 lõpp-punkti."
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

# <a name="types-of-apps-for-the-v20-endpoint"></a>Tüüpi rakendusi v2.0 lõpp-punkti
V2.0 lõpp-punkti toetab mitmesuguseid tänapäevane rakenduse arhitektuur, mis põhinevad industry standard Protokollid [OAuth 2.0](active-directory-v2-protocols.md#oauth2-authorization-code-flow) ja/või [OpenID Connect](active-directory-v2-protocols.md#openid-connect-sign-in-flow)autentimist.  Selle dokumendi lühikirjeldused, millist tüüpi rakendusi saate koostada, keele- või platvormi sõltumatu eelistate.  See aitab teil mõista kõrge stsenaariumid, enne kui saate [hakata kohe kood](active-directory-appmodel-v2-overview.md#getting-started).

> [AZURE.NOTE]
    Kõik Azure Active Directory stsenaariumid ja funktsioonid on toetatud v2.0 lõpp-punkt.  Kui kasutate v2.0 lõpp-punkti määramiseks lugege [v2.0 piirangud](active-directory-v2-limitations.md).

## <a name="the-basics"></a>Põhitõed
Iga rakendus, mis kasutab v2.0 lõpp-punkti peate olema registreeritud [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList).  Rakenduse registreerimise käigus on koguda ja rakenduse väärtuste määramine:

- **Rakenduse Id** , mis tuvastab kordumatult rakenduse
- **URI suunamine** otse teie rakendusse vastuste kasutatavate
- Mõne muu stsenaarium kohased väärtused.  Lisateavet saate teada, kuidas [registreerida rakendus](active-directory-v2-app-registration.md).

Kui olete registreerunud, rakenduse suhtleb Azure AD, saates taotlusi Azure Active Directory v2.0 lõpp-punkti.  Pakume avatud allika raamistiku & teekide puhul, kus Olge ettevaatlik taotlused üksikasjad või saate rakendada autentimise loogika enda poolt käsitöö taotluste nende lõpp-punktid.

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
https://login.microsoftonline.com/common/oauth2/v2.0/token
```
<!-- TODO: Need a page for libraries to link to -->

## <a name="web-apps"></a>Veebirakenduste
Web Appsi (.NET, PHP, Java, Ruby, Python, sõlm jne), millele pääseb brauseri kaudu, saate teha kasutaja sisselogimine [OpenID Connect](active-directory-v2-protocols.md#openid-connect-sign-in-flow)abil.  OpenID Connect web Appi saab ka `id_token`, Turbeloa, mis kinnitab, et kasutaja identiteedi ja teave kasutaja taotluste kujul:

```
// Partial raw id_token
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6ImtyaU1QZG1Cd...

// Partial content of a decoded id_token
{
    "name": "John Smith",
    "email": "john.smith@gmail.com",
    "oid": "d9674823-dffc-4e3f-a6eb-62fe4bd48a58"
    ...
}
```

Saate teadmisi kõigi tüüpi sõned ja [v2.0 Turbeloa viite](active-directory-v2-tokens.md)rakendus nõuded saadaval.

Veebirakenduste serveri sisselogimise autentimise voogu võtab kõrge järgmist:

![Web Appi ujumisradade pilt](../media/active-directory-v2-flows/convergence_scenarios_webapp.png)

Veenduge, et kasutaja identiteedi ja seansi küpsise, mida saab kasutada tuvastada kasutaja lehe edaspidised taotlused piisab id_token, kasutades avaliku allkirjastamiseks klahvi saadud v2.0 lõpp-punkti kinnitamine.

See stsenaarium tegelikkuses vaatamiseks proovida ühte web appi sisselogimise koodi näidised meie [Alustamine](active-directory-appmodel-v2-overview.md#getting-started) jaotises.

Lisaks lihtsa sisselogimine, veebirakenduse server võib ka peavad juurde pääsema mõne muu veebiteenuse, nt REST API.  Sel juhul saate veebirakenduse serveri kombineeritud OpenID ühendus ja OAuth 2.0 meilivoo, kasutades [OAuthi 2.0 kood meilivoo](active-directory-v2-protocols.md#oauth2-authorization-code-flow)osaleda. Selle stsenaariumi käsitletakse meie [Web Appis-WebAPI alustamine teema](active-directory-v2-devquickstarts-webapp-webapi-dotnet.md)all.

## <a name="web-apis"></a>Veebi-API
Saate secure veebiteenuste samuti, nagu teie rakendus rahulik Web API v2.0 lõpp-punkti.  Selle asemel id_tokens ja küpsised, veebi-API kasutada OAuth 2.0 access_tokens kaitsta oma andmeid ja autentimiseks sissetulevad taotlused.  Funktsiooni helistaja Web API lisab mõne access_token luba päises HTTP-päring:

```
GET /api/items HTTP/1.1
Host: www.mywebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6...
Accept: application/json
...
```

Veebi-API saate kasutada funktsiooni access_token kinnitada API helistaja ID ja helistaja teabe ekstraktida taotluste, mis on kodeeritud soovitud access_token.  Saate teadmisi kõigi tüüpi sõned ja [v2.0 Turbeloa viite](active-directory-v2-tokens.md)rakendus nõuded saadaval.

Veebi-API-ga saate anda kasutajatele õigus valida-sisse/loobuda teatud funktsioonid või andmete asetades tuntud ka kui [otsinguulatuste](active-directory-v2-scopes.md)õigused.  Helistaja rakenduse on õigus saada, peab kasutaja nõusoleku ulatus on meilivoo ajal.  Lõpp-punkti v2.0 hoolitseb küsib kasutaja õiguste ja salvestamise kõik access_tokens, et veebi-API saab need õigused.  Kõik veebi-API peab muretsema on kinnitamise access_tokens, saab iga ja läbimiseks õige autoriseerimine kontrollid.

Veebi-API-ga võite saada access_tokens igat tüüpi rakendusi, sh veebirakenduste server, töölaua ja mobiilirakendused, ühelt leheküljelt rakendused, serveri pool deemonid ja isegi muu veebi-API-d.  Kõrge taseme voogu web api autentimine on järgmine:

![Veebi-API ujumisradade pilt](../media/active-directory-v2-flows/convergence_scenarios_webapi.png)

Üksikasjalikud juhised saada access_tokens authorization_codes ning refresh_tokens kohta lisateabe saamiseks lugege [OAuth 2.0 Protocol (protokoll)](active-directory-v2-protocols-oauth-code.md).

Saate teada, kuidas tagada web api koos OAuth2 access_tokens, vaadake meie [Alustamine jaotises](active-directory-appmodel-v2-overview.md#getting-started)koodinäiteid web API-ga.


## <a name="mobile-and-native-apps"></a>Mobile ja kohalikke rakendused
Rakendusi, mis on installitud seadme mobiil- ja lauaarvutite rakendused, nagu on sageli vaja kirjutamata teenustele juurdepääsuks Web API-d andmete talletamiseks ja saate kasutada mitmesuguseid funktsioone kasutaja nimel.  Nende rakenduste saate lisada taustväärtus teenuste abil [OAuthi 2.0 kood meilivoo](active-directory-v2-protocols-oauth-code.md)sisselogimine ja autoriseerimine.  

Klõpsake selle voo lisamine rakenduse saab ka authorization_code v2.0 lõpp-punkti korral kasutajate sisselogimist, mis tähistab rakenduse õiguse kirjutamata teenuste logitud kasutaja nimel helistamiseks.  Rakenduse saate vahetada siis authoriztion_code mõne OAuth 2.0 access_token ja mõne refresh_token taustal.  Rakenduse saate kasutada funktsiooni access_token autentimiseks Web API-de sisse HTTP päringuid ja saate kasutada funktsiooni refresh_token kuvatakse uus access_tokens vanemaid aeguda.

![Rakenduste ujumisradade pilt](../media/active-directory-v2-flows/convergence_scenarios_native.png)

## <a name="single-page-apps-javascript"></a>Ühelt leheküljelt rakendused (javascript)
Paljud tänapäevased rakendused on lisamine ühele lehele rakenduse (SPA) ees kirjutatud peamiselt JavaScripti ja sageli kasutate raamistikku nagu AngularJS, Ember.js, Durandal jne.  Azure AD v2.0 lõpp-punkti toetab nende rakenduste abil [OAuthi 2.0 peidetud Flow](active-directory-v2-protocols-implicit.md).

See vool rakenduse saab sõned kaudu soovitud v2.0 lubada lõpp-punkti otse ilma kirjutamata server server vahetamise.  See võimaldab kõik autentimise loogika- ja seansi töötlemine võtta paigutada täiesti JavaScripti klient, ilma ülearuse lehe ümbersuunamisi.

![Peidetud meilivoo ujumisradade pilt](../media/active-directory-v2-flows/convergence_scenarios_implicit.png)

See stsenaarium tegelikkuses vaatamiseks proovida ühte meie [Alustamine](active-directory-appmodel-v2-overview.md#getting-started) jaotises koodinäiteid ühelt leheküljelt rakendus.

### <a name="daemonsserver-side-apps"></a>Deemonid ja serveri pool rakendused
Samuti vajate rakendused, mis sisaldavad kaua töötavad protsessid või mis toimivad ilma kasutaja kohalolek turvalise ressursse, näiteks Web API-d juurdepääsu.  Nende rakenduste saate autentimiseks ja kasutades rakenduse identiteedi (mitte delegeeritud kasutaja identiteet) OAuth 2.0 kliendi sõned identimisteabe kulgemist.

See vool rakenduse hangib sõned suheldes otse soovitud `/token` lõpp-punkt:

![Daemon rakenduse ujumisradade pilt](../media/active-directory-v2-flows/convergence_scenarios_daemon.png)

Daemon rakenduse, vt kliendi mandaadi documeenation meie [Alustamine](active-directory-appmodel-v2-overview.md#getting-started) jaotises või [.NET valimi rakenduse](https://github.com/Azure-Samples/active-directory-dotnet-daemon-v2)viidata.

## <a name="current-limitations"></a>Praeguse piirangud
Järgmist tüüpi rakendusi ei toeta praegu v2.0 lõpp-punkti, kuid oleksid tegevuskava.  [V2.0 piirangud artiklis](active-directory-v2-limitations.md)kirjeldatud täiendavad piirangud ja piirangutega v2.0 lõpp-punkti.

### <a name="chained-web-apis-on-behalf-of"></a>Aheldatud web API-de (-nimel-kohta)
Mitme arhitektuurides kaasata veebi-API-ga, mis tuleb kõne teise järgneval Web API, mõlemad turvatud v2.0 lõpp-punkt.  See stsenaarium on levinud kohalikke klientidele, mis on Veebiteenuste kirjutamata, mis omakorda nõuab Microsoft Online'i teenusele, näiteks Office 365 või Graph API.

See aheldatud Veebiteenuste stsenaarium saate toetatud OAuthi 2.0 Jwt esitaja mandaati anda, muidu tuntud [On-Behalf-Of Flow](active-directory-v2-protocols.md#oauth2-on-behalf-of-flow)abil.  Siiski voo sees-nimel-, pole praegu rakendatakse v2.0 lõpp-punkti.  See vool üldiselt kättesaadav Azure toimimise vaatamiseks AD teenuse, märkige ruut välja [-nimel-kohta kood valimi github](https://github.com/AzureADSamples/WebAPI-OnBehalfOf-DotNet).
