<properties
    pageTitle="Azure'i AD B2C | Microsoft Azure'i"
    description="Millist tüüpi rakendusi saate koostada Azure Active Directory B2C."
    services="active-directory-b2c"
    documentationCenter=""
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="07/22/2016"
    ms.author="dastrock"/>

# <a name="azure-active-directory-b2c-types-of-applications"></a>Azure Active Directory B2C: Tüüpi rakendusi

Azure Active Directory (Azure AD) B2C toetab mitmesuguseid kaasaegne rakendus arhitektuurides autentimist. Kõik need põhinevad valdkonna standard Protokollid [OAuth 2.0](active-directory-b2c-reference-protocols.md) või [OpenID ühendus](active-directory-b2c-reference-protocols.md). Selle dokumendi kirjeldab lühidalt tüüpi rakendusi, mida saate koostada, keele- või platvormi sõltumatu eelistate. See aitab teil mõista üksikasjalik stsenaariumid enne [alustada rakenduste loomine](active-directory-b2c-overview.md#getting-started).

## <a name="the-basics"></a>Põhitõed
Iga rakendus, mis kasutab Azure AD B2C peab olema registreeritud [B2C directory](active-directory-b2c-get-started.md) [Azure portaali](https://portal.azure.com/)kaudu. Rakenduse registreerimise käigus kogub ja määrab väärtuste rakenduse.

- **Rakenduse ID** , mis tuvastab kordumatult rakenduse.
- **URI suunamine** otse teie rakendusse vastuste kasutatud.
- Muud stsenaarium kohased väärtused. Lisateavet leiate siit saate teada, kuidas [registreerida rakendus](active-directory-b2c-app-registration.md).

Kui rakendus on registreeritud, see suhtleb Azure AD Azure AD v2.0 lõpp-punkti päringu saatmine:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
https://login.microsoftonline.com/common/oauth2/v2.0/token
```

Iga taotlus, mis saadetakse Azure AD B2C määrab **poliitika**. Poliitika juhtelemendid Azure AD toimimist. Nende lõpp-punktid abil saate luua kohandatav kasutuskogemuse. Poliitika kaasata registreerumise, Logi sisse ja profiili-Redigeeri poliitika. Kui olete tuttav poliitikad, peaksite lugema Azure AD B2C [laiendatav raamistik](active-directory-b2c-reference-policies.md) kohta enne jätkamist.

Üksikasjalik sarnane mõju iga rakenduse v2.0 endpoint järgmiselt

1. Rakendus suunab kasutaja v2.0 lõpp-punkti [poliitika](active-directory-b2c-reference-policies.md)käivitada.
2. Kasutaja lõpetab poliitika poliitika määratluse kohaselt.
4. Rakenduse saab Turbeloa mingi v2.0 lõpp-punkti.
5. Rakendus kasutab Turbeloa juurdepääsuks kaitstud teavet või kaitstud ressursi.
6. Allika server kinnitatakse Turbeloa kinnitamaks, et access võib anda.
7. Rakenduse värskendab perioodiliselt Turbeloa.

<!-- TODO: Need a page for libraries to link to -->
Need juhised võivad olla erinevad veidi põhjal olete hoone rakendusest. Avage allikas teekide võite käsitleda üksikasjad teile.

## <a name="web-apps"></a>Veebirakenduste
Web Appsi (sh .NET, PHP, Java, Ruby, Python ja Node.js) mis on majutatud serveris ja brauseri kaudu, toetab Azure AD B2C [OpenID ühendamine](active-directory-b2c-reference-protocols.md) kõigi kasutajate kogemusi. See hõlmab sisselogimist, registreerumise, ja kasutajaprofiilide haldamine. Azure'i AD B2C rakendamise OpenID ühendus, alustab oma veebirakenduse nende kasutuskogemuse väljastanud taotluste autentimise Azure AD. Taotluse tulemus on ka `id_token`. See Turbeloa tähistab kasutaja identiteet. Pakub teavet kasutaja taotluste kujul:

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

Lugege lisateavet tüüpi sõned ja rakenduse [B2C luba viidata](active-directory-b2c-reference-tokens.md)nõuded saadaval.

Web Appis iga täitmise [poliitika](active-directory-b2c-reference-policies.md) on üksikasjalik järgmist:

![Web Appi ujumisradade pilt](./media/active-directory-b2c-apps/webapp.png)

Valideerimine on `id_token` , kasutades allkirja avalik võti, mis on saadud Azure AD piisab kasutaja identiteedi kinnitamiseks. Seda määrab ka küpsis seansi, mida saab kasutada tuvastada lehe edaspidised taotlused kasutaja.

See stsenaarium tegelikkuses vaatamiseks proovige ühte web appi sisselogimise koodi näidised on meie [Alustamine jaotises](active-directory-b2c-overview.md#getting-started).

Lisaks, et hõlbustada lihtsa sisselogimist veebirakenduse server võib ka peavad juurde pääsema tagaandmebaas veebiteenus. Sel juhul web app saate teha veidi teistsugused [OpenID ühenduse loomine meilivoo](active-directory-b2c-reference-oidc.md) ja hankida sõned autoriseerimine koodide abil ja sõned värskendamine. See stsenaarium on kujutatud järgmises [jaotises Web API -de](#web-apis).

<!--, and in our [WebApp-WebAPI Getting started topic](active-directory-b2c-devquickstarts-web-api-dotnet.md).-->

## <a name="web-apis"></a>Veebi-API
Azure'i AD B2C abil saate näiteks oma rakenduse rahulik veebi-API veebiteenused turvata. Web API-de abil saate OAuth 2.0 secure oma andmeid, sissetulevad taotlused http kaudu, kasutades sõned autentimine. Veebi-API helistaja lisab märgiks autoriseerimine päises HTTP-päring:

```
GET /api/items HTTP/1.1
Host: www.mywebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6...
Accept: application/json
...
```

Veebi-API saate luba API helistaja tuvastamiseks ja funktsiooni helistaja teabe eraldamiseks taotluste, mis on kodeeritud luba. Lugege lisateavet tüüpi sõned ja [Azure AD B2C luba viidata](active-directory-b2c-reference-tokens.md)rakenduse nõuded saadaval.

> [AZURE.NOTE]
    Azure'i AD B2C toetab praegu web API-d, millele pääseb juurde tuntud kliendid. Näiteks täielik rakenduse võivad sisaldada rakendus iOS-i ja Androidi rakenduse tagaandmebaas veebi-API. See arhitektuur on täielikult toetatud. Võimaldab partneri klient, nt teise iOS-i rakenduse, juurde pääseda sama veebi API praegu ei toetata. Kõik komponendid täieliku rakenduse peab jagada ühe rakenduste ID-ga.

Veebi-API võite saada sõned mitmesuguseid kliente, sh veebirakenduste, töölaua ja mobiilirakendused, ühelt leheküljelt rakendused, serveripoolne deemonid ja muud web API-d. Siin on näide täieliku kulgemist web app, mis nõuab veebi-API:

![Web App veebi-API ujumisradade pilt](./media/active-directory-b2c-apps/webapi.png)

Luba koode, värskendamise märkide ja märkide toomiseks vajalikud toimingud kohta lisateabe saamiseks lugege [OAuth 2.0 Protocol (protokoll)](active-directory-b2c-reference-oauth-code.md).

Saate teada, kuidas turvaliseks muutmine veebi-API Azure AD B2C, vaadake meie [Alustamine jaotises](active-directory-b2c-overview.md#getting-started)web API õpetused.

## <a name="mobile-and-native-apps"></a>Mobile ja kohalikke apps
Rakendused, mis installitakse seadmetes, nt mobiil- ja lauaarvutite rakendusi, on sageli vaja tagaandmebaas teenustele juurdepääsuks web API-de kasutajate nimel. Saate lisada kohandatud identiteedi haldamine kogemusi kohalikke rakendused ja turvaline tagaandmebaas kõneteenuseid Azure AD B2C ja [OAuth 2.0 autoriseerimine kood meilivoo](active-directory-b2c-reference-oauth-code.md)abil.  

See vool rakenduse käivitab [poliitikate](active-directory-b2c-reference-policies.md) ja võtab vastu ka `authorization_code` : Azure'i AD pärast kasutaja lõpulejõudmist poliitika. Funktsiooni `authorization_code` tähistab rakenduse õiguste tagaandmebaas teenuste kasutaja, kes on sisse loginud nimel helistamiseks. Rakenduse vahetamiseks klõpsake soovitud `authorization_code` taustal on `id_token` ja `refresh_token`.  Rakenduse saate kasutada funktsiooni `id_token` autentida tagaandmebaas veebi API HTTP päringuid. Kasutada ka funktsiooni `refresh_token` uue `id_token` ühe vanema aegumisel.

> [AZURE.NOTE]
    Azure'i AD B2C toetab praegu ainult enda tagaandmebaas veebiteenuse juurdepääsuks kasutatakse sõned. Näiteks täielik rakenduse võivad sisaldada rakendus iOS-i ja Androidi rakenduse tagaandmebaas veebi-API. See arhitektuur on täielikult toetatud. Võimaldades juurdepääsu partneri veebi-API OAuth 2.0 Accessi sõned abil oma iOS-i rakendus pole praegu toetatud. Kõik komponendid täieliku rakenduse peab jagada ühe rakenduste ID-ga.

![Rakenduste ujumisradade pilt](./media/active-directory-b2c-apps/native.png)

## <a name="current-limitations"></a>Praeguse piirangud
Azure'i AD B2C ei toeta praegu järgmist tüüpi rakendusi, kuid nad on selle roadmapy. Täiendavad piirangud ja piirangutega seotud Azure AD B2C on kirjeldatud [piirangud ja piirangutega](active-directory-b2c-limitations.md).

### <a name="single-page-apps-javascript"></a>Ühelt leheküljelt rakendused (JavaScript)
Paljud tänapäevased rakendused on kirjutatud peamiselt JavaScript ühe lehe rakenduse vastendamisel. Nad kasutavad sageli raamistiku nt AngularJS, Ember.js või Durandal. Üldiselt kättesaadav Azure AD teenuse toetab nende rakenduste abil OAuth 2.0 peidetud voogu. Kuid see vool ei ole veel saadaval Azure AD B2C.

### <a name="daemonsserver-side-apps"></a>Deemonid/serveripoolne rakendused
Samuti vajate rakendused, mis sisaldavad pikaajalisi protsesside või mis toimivad ilma kasutaja kohalolek turvalise ressursse, nt web API-d juurdepääsu. Need rakendused saate autentimiseks ja sõned abil rakenduse identiteedi (mitte delegeeritud kasutaja identiteet) ja OAuth 2.0 kliendi abil identimisteabe kulgemist.

See vool ei toeta praegu Azure AD B2C. Nende rakenduste pääsevad sõned alles pärast seda, kui on ilmnenud mõni interaktiivne kasutaja kulgemist.

### <a name="web-api-chains-on-behalf-of-flow"></a>Veebi-API ketid (vool nimel-kohta)
Mitme arhitektuurides kaasata web API, mis tuleb kõne teisele järgneval veebi-API, kui mõlemad on tagatud Azure AD B2C. See stsenaarium on levinud kohalikke klientidele, mis on Veebiteenuste tagaandmebaas. See siis helistab Microsoft Online'i teenuse, nt Azure AD Graph API.

Selle stsenaariumi aheldatud veebi-API saate toetatud OAuthi 2.0 JWT esitaja mandaati anda ka-nimel-kohta voo abil.  Siiski-nimel-kohta voogu pole praegu rakendatakse Azure AD B2C.
