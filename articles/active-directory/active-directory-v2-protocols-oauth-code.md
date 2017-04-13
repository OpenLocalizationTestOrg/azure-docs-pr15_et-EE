

<properties
    pageTitle="Azure'i AD v2.0 OAuthi autoriseerimine kood Flow | Microsoft Azure'i"
    description="Koosteüksuse veebirakenduste abil Azure AD OAuth 2.0 autentimine protokolli rakendamine."
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
    ms.date="08/08/2016"
    ms.author="dastrock"/>

# <a name="v20-protocols---oauth-20-authorization-code-flow"></a>v2.0 Protokollid - OAuthi 2.0 autoriseerimine kood Flow

OAuth 2.0 autoriseerimine koodi anda saab kasutada rakendusi, mis on seadmesse installitud hiljem juurde pääseda kaitstud ressursse, näiteks web API-d.  Rakenduse mudeli v2.0 rakendamine OAuth 2.0 kasutamisel saate lisada sisse logida ja API-juurdepääsu oma mobiil- ja lauaarvutite rakendused.  Sellest juhendist on keele sõltumatul ja kirjeldatakse, kuidas sõnumite saatmiseks ja vastuvõtmiseks HTTP ühte meie avatud lähtekoodi teekide kasutamata.

<!-- TODO: Need link to libraries -->

> [AZURE.NOTE]
    Kõik Azure Active Directory stsenaariumid ja funktsioonid on toetatud v2.0 lõpp-punkt.  Kui kasutate v2.0 lõpp-punkti määramiseks lugege [v2.0 piirangud](active-directory-v2-limitations.md).

OAuth 2.0 autoriseerimine kood voogu on kirjeldatud jaotises [4.1 OAuth 2.0 kirjelduse](http://tools.ietf.org/html/rfc6749).  Seda saab teha rakenduse tüüpi [veebirakenduste](active-directory-v2-flows.md#web-apps) ja [algupäraselt installitud rakendusi](active-directory-v2-flows.md#mobile-and-native-apps)enamikus autentimise ja luba.  See võimaldab rakendusi hankida turvaliselt access_tokens, mida saate kasutada ressursse, mis on kinnitatud, kasutades v2.0 lõpp-punkti juurdepääs.  

## <a name="protocol-diagram"></a>Protocol (protokoll) skeem
Kõrge, kogu autentimise voogu native/mobile rakenduse veidi on järgmine:

![OAuthi Auth kood meilivoo](../media/active-directory-v2-flows/convergence_scenarios_native.png)

## <a name="request-an-authorization-code"></a>Kutse mõne kood
Luba kood voogu algab suunavad kasutajal kliendi soovitud `/authorize` lõpp-punkti.  Selle taotluse näitab kliendi õigusi on vaja kasutaja hankimine:

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=query
&scope=openid%20offline_access%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&state=12345
```

> [AZURE.TIP] Klõpsake linki selle taotluse! Pärast sisselogimist, peaks teie brauser ümber `https://localhost/myapp/` koos mõne `code` aadressiribale.
    <a href="https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=code&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&response_mode=query&scope=openid%20offline_access%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read&state=12345" target="_blank">https://login.microsoftonline.com/common/oauth2/v2.0/authorize...</a>

| Parameetri | | Kirjeldus |
| ----------------------- | ------------------------------- | --------------- |
| rentniku | nõutav | Funktsiooni `{tenant}` väärtus taotluse tee saab kontrollida, kes saavad logida rakendusse.  Lubatud väärtused on `common`, `organizations`, `consumers`, ja rentnikuhalduse identifikaatorid.  Lugege täpsemalt [protokolli põhitõed](active-directory-v2-protocols.md#endpoints). |
| client_id | nõutav | Rakenduse Id, et registreerimise portaali ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) määratud rakenduse. |
| response_type | nõutav | Peab sisaldama `code` autoriseerimine kood meilivoo jaoks. |
| redirect_uri | soovitatav | Rakenduse, kus autentimine vastuseid saate saadetud ja vastuvõetud rakenduse redirect_uri.  See peab täpselt vastama ühte redirect_uris, mis on registreeritud portaali, kuid peab olema URL-ina kodeeritav.  Kohalikke ja Mobile'i rakendused, kasutage vaikeväärtust, milleks on `https://login.microsoftonline.com/common/oauth2/nativeclient`. |
| ulatus | nõutav | Tühikuga eraldatud loend [otsinguulatuste](active-directory-v2-scopes.md) soovitud kasutaja nõusolekut.  |
| response_mode | soovitatav | Saate määrata meetod, mis tuleks kasutada rakenduse tulemuseks Turbeloa tagasi saatmiseks.  Võib olla `query` või `form_post`.  |
| olek | soovitatav | Väärtus, mis sisaldab taotluse, mis tagastatakse loa vastuse.  See võib olla mis tahes sisu, mida soovite string.  Juhuslik kordumatu väärtus on tavaliselt kasutatakse [vältimine saitidevaheline taotluse võltsimist eest](http://tools.ietf.org/html/rfc6749#section-10.12).  Riigi kasutatakse ka kodeerida teavet rakenduse kasutaja olekus enne autentimine taotluse ilmnes, näiteks lehe või vaate nad. |
| küsimus | valikuline. | Näitab, et kasutaja suhtlus, mida on vaja tüüp.  Sel ajal kehtivad ainult väärtused on 'sisselogimine","pole"ja"nõus".  `prompt=login`sunnib kasutajal sisestada oma volitusi selle taotluse hindamine ühekordse sisselogimise kohta.  `prompt=none`on vastand - tagab, et kasutaja ei ole esitatud mis tahes interaktiivsed küsimus üldse.  Kui taotlust ei saa lõpule viia vaikselt teel ühekordse sisselogimise kohta, v2.0 lõpp-punkti tagastavad vea.  `prompt=consent`käivitab OAuthi nõusolekut dialoogiboksi pärast kasutaja märke, kasutajalt rakenduse õigusi anda. |
| login_hint | valikuline. | Saab kasutada eelnevalt täitmiseks välja kasutajanimi/e-posti aadress sisselogimislehe kasutaja, kui te ei tea oma kasutajanime ette valmistada.  Kasutatakse sageli rakenduste see parameeter on juba ekstraktimist kasutajanimi on eelmise sisselogimise abil uuesti autentimisel on `preferred_username` taotlemine. |
| domain_hint | valikuline. | Võib olla üks `consumers` või `organizations`.  Kui, see e-postipõhise discovery protsessi vahele jätta kasutaja läheb läbi v2.0 sisselogimislehel ees on veidi sujuv kasutajaliides.  Sageli rakendused saavad kasutada see parameeter uuesti autentimisel, ekstraktimiseks soovitud `tid` kaudu eelmise sisselogimine.  Kui soovitud `tid` taotlemine väärtus on `9188040d-6c67-4c5b-b112-36a304b66dad`, kasutage `domain_hint=consumers`.  Muul juhul kasutada `domain_hint=organizations`. |

Selles etapis küsitakse kasutaja autentimise ja sisestage oma kasutajanimi ja parool.  Lõpp-punkti v2.0 tagab, et kasutajal on andnud õigused, nagu on näidatud ka selle `scope` päringu parameeter.  Kui kasutaja ei ole nõus mis tahes need õigused, seda küsib kasutaja nõusolekut vajalikke õigusi.  [Õiguste, nõusoleku ja mitme rentniku rakendused on siin esitatud](active-directory-v2-scopes.md)üksikasjad.

Kui kasutaja autendib ja annab nõusoleku, v2.0 lõpp-punkti tulemuseks rakenduse juures märgitud vastuseks `redirect_uri`, määratud meetodil on `response_mode` parameeter.

#### <a name="successful-response"></a>Eduka vastus
Eduka vastuse kasutades `response_mode=query` näeb välja nagu:

```
GET https://login.microsoftonline.com/common/oauth2/nativeclient?
code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...
&state=12345
```

| Parameetri | Kirjeldus |
| ----------------------- | ------------------------------- |
| kood | Authorization_code, mis rakenduse nõutav. Rakenduse saate kasutada taotleda juurdepääsu ressursile target sümboolse luba kood.  Authorization_codes on väga lühiajalise, tavaliselt need aeguvad umbes 10 minuti pärast. |
| olek | Kui parameeter olek on kaasatud kutse, kuvatakse sama väärtuse vastus. Rakendus peaks kinnitamine olekus väärtused koosolekukutsete ja kutsele vastamise identsed. |

#### <a name="error-response"></a>Tõrge vastus
Tõrge vastuste võib ka saata selle `redirect_uri` nii, et rakendus võib käsitleda neid õigesti:

```
GET https://login.microsoftonline.com/common/oauth2/nativeclient?
error=access_denied
&error_description=the+user+canceled+the+authentication
```

| Parameetri | Kirjeldus |
| ----------------------- | ------------------------------- |
| tõrge | Kuvatakse tõrge koodi string, mis saab liigitada tüüpi ilmnevad tõrked ja seda saab kasutada reageerida tõrgete. |
| error_description | Teatud tõrketeade, mis aitab tuvastada autentimise tõrke põhjuseks arendaja.  |

#### <a name="error-codes-for-authorization-endpoint-errors"></a>Tõrkekoodid autoriseerimine lõpp-punkti tõrkeid

Järgmises tabelis kirjeldatakse saate tagasi erinevate tõrkekoodid on `error` parameetri tõrge tagasiside.

| Tõrkekood | Kirjeldus | Kliendi toiming |
|------------|-------------|---------------|
| invalid_request | Protocol (protokoll) viga, nt puuduvad parameeter. | Parandamine ja kutse uuesti. See on areng tõrge püütakse tavaliselt algse testimise käigus.|
| unauthorized_client | Klientrakenduse ei ole lubatud taotleda luba näeb välja umbes järgmine. | See juhtub tavaliselt kui klientrakendus pole registreeritud Azure AD või kasutaja Azure AD rentniku ei lisata. Rakenduse saate kiiresti juhiseid rakenduse installimine ja selle lisamist Azure AD kasutaja. |
| ACCESS_DENIED | Ressursi omanik nõusolekut keelatud | Klientrakenduse saab seda ei saa jätkata, v.a juhul, kui kasutaja nõustub kasutaja teatada. |
| unsupported_response_type | Luba server ei toeta taotluse vastuse tüüp. | Parandamine ja kutse uuesti. See on areng tõrge püütakse tavaliselt algse testimise käigus.|
|server_error | Server ilmnes ootamatu tõrge. | Kutse uuesti. Nende vigade võib põhjustada ajutist tingimusi. Klientrakenduse võib selgitada kasutaja, et oma vastuse viibib tähtaja ajutine tõrge. |
| temporarily_unavailable | Server pole ajutiselt hõivatud päringu töötlemiseks. | Kutse uuesti. Klientrakenduse võib selgitada kasutaja, et oma vastuse viibib tähtaja ajutine seisund. |
| invalid_resource |Sihtrakenduse ressursi ei sobi, kuna seda pole, Azure AD ei leia või see on õigesti konfigureeritud.| See näitab, et ressurss, kui see on olemas, ei ole konfigureeritud rentniku. Rakenduse saate kiiresti juhiseid rakenduse installimine ja selle lisamist Azure AD kasutaja. |

## <a name="request-an-access-token"></a>Taotleda juurdepääsu sümboolse
Nüüd, kui olete saanud mõne authorization_code ja on antud õigus kasutaja, saab lunastada soovitud `code` jaoks on `access_token` soovitud ressursile, saates on `POST` taotlus on `/token` lõpp-punkti:

```
// Line breaks for legibility only

POST /{tenant}/oauth2/v2.0/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&scope=https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&code=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq3n8b2JRLk4OxVXr...
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&grant_type=authorization_code
&client_secret=JqQX2PNo9bpM0uEihUPzyrh    // NOTE: Only required for web apps
```

> [AZURE.TIP] Proovige selle päringu käivitamist postiljon! (Ärge unustage asendada selle `code`)     [ ![Läbiviimisel postiljon](./media/active-directory-v2-protocols-oauth-code/runInPostman.png)](https://app.getpostman.com/run-collection/8f5715ec514865a07e6a)

| Parameetri | | Kirjeldus |
| ----------------------- | ------------------------------- | --------------------- |
| rentniku | nõutav | Funktsiooni `{tenant}` väärtus taotluse tee saab kontrollida, kes saavad logida rakendusse.  Lubatud väärtused on `common`, `organizations`, `consumers`, ja rentnikuhalduse identifikaatorid.  Lugege täpsemalt [protokolli põhitõed](active-directory-v2-protocols.md#endpoints). |
| client_id | nõutav | Rakenduse Id, et registreerimise portaali ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) määratud rakenduse. |
| grant_type | nõutav | Peab olema `authorization_code` autoriseerimine kood meilivoo jaoks. |
| ulatus | nõutav | Tühikuga eraldatud loend otsinguulatuste.  Otsinguulatuste, nõutav antud etapil peab olema otsinguulatuste, esimesel etapil nõutud Alamhulk või võrdne.  Kui otsinguulatuste, määratud taotlus ühendada mitu ressursi serverid, siis v2.0 lõpp-punkti tagasi märgiks määratud esimese ulatuse ressursi.  Veel üksikasjalik kirjeldus otsinguulatuste, vaadake [õigused, nõusoleku ja otsinguulatuste](active-directory-v2-scopes.md).  |
| kood | nõutav | Authorization_code, mille ostsite esimesel etapil kulgemist.   |
| redirect_uri | nõutav | Sama redirect_uri väärtus, mida kasutati funktsiooni authorization_code hankimine. |
| client_secret | nõutav web apps | Rakenduse salajane rakenduse registreerimise portaali loodud rakenduse.  See peaks ei tohi kasutada rakenduste, kuna client_secrets ei saa usaldusväärselt salvestatud seadmetes.  See on nõutav veebirakenduste ja veebi API-d, mida hüpikmenüüsid talletamiseks soovitud client_secret turvaliselt serveripoolse. |

#### <a name="successful-response"></a>Eduka vastus
Eduka loa vastuse näeb:

```
{
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https%3A%2F%2Fgraph.microsoft.com%2Fmail.read",
    "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctOD...",
}
```
| Parameetri | Kirjeldus |
| ----------------------- | ------------------------------- |
| access_token | Nõutud juurdepääsu luba. Rakenduse saate kasutada seda luba turvalise ressursile, nt veebi-API autentida. |
| token_type | Näitab, et loa tüüp väärtus. Ainult tüüp, mis toetab Azure AD on esitaja  |
| expires_in | Kui kaua kehtib juurdepääsu luba (sekundites). |
| ulatus | Otsinguulatuste, mis on access_token kehtib. |
| refresh_token |  OAuth 2.0 värskendamise luba. Rakenduse abil saate selle luba hankida täiendavat juurdepääsu sõned pärast praeguse juurdepääsu luba on aegunud.  Refresh_tokens on vanade ja saab kasutada säilitamise ressurssidele laiendatud aja jooksul.  Lugege täpsemalt [v2.0 Turbeloa viide](active-directory-v2-tokens.md).  |
| id_token | Ka allkirjastamata JSON Web Turbeloa (JWT). Rakenduse saab base64Url dekodeerida selle märgiks sisse logitud kasutaja teavet taotleda lõigud. Rakenduse saate vahemälu väärtused ja neid kuvada, kuid see ärge neid autoriseerimine või turvalisus piirmäärad.  Vt lisateavet id_tokens [v2.0 lõpp-punkti Turbeloa viide](active-directory-v2-tokens.md). |

#### <a name="error-response"></a>Tõrge vastus
Tõrge vastuste näeb:

```
{
  "error": "invalid_scope",
  "error_description": "AADSTS70011: The provided value for the input parameter 'scope' is not valid. The scope https://foo.microsoft.com/mail.read is not valid.\r\nTrace ID: 255d1aef-8c98-452f-ac51-23d051240864\r\nCorrelation ID: fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7\r\nTimestamp: 2016-01-09 02:02:12Z",
  "error_codes": [
    70011
  ],
  "timestamp": "2016-01-09 02:02:12Z",
  "trace_id": "255d1aef-8c98-452f-ac51-23d051240864",
  "correlation_id": "fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7"
}
```

| Parameetri | Kirjeldus |
| ----------------------- | ------------------------------- |
| tõrge | Kuvatakse tõrge koodi string, mis saab liigitada tüüpi ilmnevad tõrked ja seda saab kasutada reageerida tõrgete. |
| error_description | Teatud tõrketeade, mis aitab tuvastada autentimise tõrke põhjuseks arendaja.  |
| error_codes | STS kindlate tõrkekoodide, mis aitavad diagnostika loend.  |
| ajatempli | Aeg, mille viga. |
| trace_id | Koosolekukutse, mis aitavad diagnostika ainuidentifikaator.  |
| correlation_id | Koosolekukutse, mis aitavad diagnostika üle komponendid ainuidentifikaator. |

#### <a name="error-codes-for-token-endpoint-errors"></a>Tõrkekoodid Turbeloa lõpp-punkti tõrkeid

| Tõrkekood | Kirjeldus | Kliendi toiming |
|------------|-------------|---------------|
| invalid_request | Protocol (protokoll) viga, nt puuduvad parameeter. | Parandamine ja edastage taotlus |
| invalid_grant | Luba kood ei sobi või on aegunud. | Proovige uue taotluse selle `/authorize` lõpp-punkti |
| unauthorized_client | Autenditud kliendi pole õigust kasutatav loa andmine tüüp. | See juhtub tavaliselt kui klientrakendus pole registreeritud Azure AD või kasutaja Azure AD rentniku ei lisata. Rakenduse saate kiiresti juhiseid rakenduse installimine ja selle lisamist Azure AD kasutaja. |
| invalid_client | Kliendi autentimine nurjus. | Kliendi identimisteave ei sobi. Lahendamiseks värskendab teenuserakenduse administraatori mandaadi. |
| unsupported_grant_type | Luba server ei toeta loa andmine tüüp. | Muuta taotluse anda tüüpi. Seda tüüpi tõrge peaks toimuma ainult arendamise käigus ning leitud esimene katsetamine. |
| invalid_resource | Sihtrakenduse ressursi ei sobi, kuna seda pole, Azure AD ei leia või see on õigesti konfigureeritud. | See näitab, et ressurss, kui see on olemas, ei ole konfigureeritud rentniku. Rakenduse saate kiiresti juhiseid rakenduse installimine ja selle lisamist Azure AD kasutaja. |
| interaction_required | Kutse nõuab kasutaja suhtluse. Näiteks on samm täiendavate autentimine on nõutav. | Proovige uuesti taotluse sama ressursiga. |
| temporarily_unavailable | Server pole ajutiselt hõivatud päringu töötlemiseks. | Kutse uuesti. Klientrakenduse võib selgitada kasutaja, et oma vastuse viibib tähtaja ajutine seisund.|

## <a name="use-the-access-token"></a>Kasutage juurdepääsu luba
Nüüd, kui olete edukalt ostsite mõne `access_token`, saate kasutada luba Web API-de taotlusi, sh see on `Authorization` päis:

> [AZURE.TIP] Käivitada taotlus postiljon! (Asendada selle `Authorization` päise esimese)     [ ![Läbiviimisel postiljon](./media/active-directory-v2-protocols-oauth-code/runInPostman.png)](https://app.getpostman.com/run-collection/8f5715ec514865a07e6a)

```
GET /v1.0/me/messages
Host: https://graph.microsoft.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

## <a name="refresh-the-access-token"></a>Luba juurdepääs värskendamine
Access_tokens on lühike elas ja peate värskendama need pärast need aeguvad edaspidi juurdepääsu ressursid.  Saate seda teha, esitades teise `POST` taotlus on `/token` lõpp-punkti, seekord, pakkudes selle `refresh_token` asemel on `code`:

```
// Line breaks for legibility only

POST /{tenant}/oauth2/v2.0/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&scope=https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&refresh_token=OAAABAAAAiL9Kn2Z27UubvWFPbm0gLWQJVzCTE9UkP3pSx1aXxUjq...
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&grant_type=refresh_token
&client_secret=JqQX2PNo9bpM0uEihUPzyrh    // NOTE: Only required for web apps
```

> [AZURE.TIP] Proovige selle päringu käivitamist postiljon! (Ärge unustage asendada selle `refresh_token`)     [ ![Läbiviimisel postiljon](./media/active-directory-v2-protocols-oauth-code/runInPostman.png)](https://app.getpostman.com/run-collection/8f5715ec514865a07e6a)

| Parameetri | | Kirjeldus |
| ----------------------- | ------------------------------- | -------- |
| rentniku | nõutav | Funktsiooni `{tenant}` väärtus taotluse tee saab kontrollida, kes saavad logida rakendusse.  Lubatud väärtused on `common`, `organizations`, `consumers`, ja rentnikuhalduse identifikaatorid.  Lugege täpsemalt [protokolli põhitõed](active-directory-v2-protocols.md#endpoints). |
| client_id | nõutav | Rakenduse Id, et registreerimise portaali ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) määratud rakenduse. |
| grant_type | nõutav | Peab olema `refresh_token` selle etapil autoriseerimine kood kulgemist. |
| ulatus | nõutav | Tühikuga eraldatud loend otsinguulatuste.  Otsinguulatuste, nõutav antud etapil peab olema otsinguulatuste, nõutud algse authorization_code taotluse lisamaterjalidele Alamhulk või võrdne.  Kui otsinguulatuste, määratud taotlus ühendada mitu ressursi serverid, siis v2.0 lõpp-punkti tagasi märgiks määratud esimese ulatuse ressursi.  Veel üksikasjalik kirjeldus otsinguulatuste, vaadake [õigused, nõusoleku ja otsinguulatuste](active-directory-v2-scopes.md).  |
| refresh_token | nõutav | Refresh_token, mille ostsite teise jalga kulgemist.   |
| redirect_uri | nõutav | Sama redirect_uri väärtus, mida kasutati funktsiooni authorization_code hankimine. |
| client_secret | nõutav web apps | Rakenduse salajane rakenduse registreerimise portaali loodud rakenduse.  See peaks ei tohi kasutada rakenduste, kuna client_secrets ei saa usaldusväärselt salvestatud seadmetes.  See on nõutav veebirakenduste ja veebi API-d, mida hüpikmenüüsid talletamiseks soovitud client_secret turvaliselt serveripoolse. |

#### <a name="successful-response"></a>Eduka vastus
Eduka loa vastuse näeb:

```
{
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https%3A%2F%2Fgraph.microsoft.com%2Fmail.read",
    "refresh_token": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGAMxZGUTdM0t4B4...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIyZDRkMTFhMi1mODE0LTQ2YTctOD...",
}
```
| Parameetri | Kirjeldus |
| ----------------------- | ------------------------------- |
| access_token | Nõutud juurdepääsu luba. Rakenduse saate kasutada seda luba turvalise ressursile, nt veebi-API autentida. |
| token_type | Näitab, et loa tüüp väärtus. Ainult tüüp, mis toetab Azure AD on esitaja  |
| expires_in | Kui kaua kehtib juurdepääsu luba (sekundites). |
| ulatus | Otsinguulatuste, mis on access_token kehtib. |
| refresh_token |  Uue OAuth 2.0 värskendamise loa. Vana värskendamise luba tuleks asendada selle värskelt omandatud värskendamise märgiks tagada teie värskendamise sõned kehtivad nii kaua.  |
| id_token | Ka allkirjastamata JSON Web Turbeloa (JWT). Rakenduse saab base64Url dekodeerida selle märgiks sisse logitud kasutaja teavet taotleda lõigud. Rakenduse saate vahemälu väärtused ja neid kuvada, kuid see ärge neid autoriseerimine või turvalisus piirmäärad.  Vt lisateavet id_tokens [v2.0 lõpp-punkti Turbeloa viide](active-directory-v2-tokens.md). |

#### <a name="error-response"></a>Tõrge vastus
```
{
  "error": "invalid_scope",
  "error_description": "AADSTS70011: The provided value for the input parameter 'scope' is not valid. The scope https://foo.microsoft.com/mail.read is not valid.\r\nTrace ID: 255d1aef-8c98-452f-ac51-23d051240864\r\nCorrelation ID: fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7\r\nTimestamp: 2016-01-09 02:02:12Z",
  "error_codes": [
    70011
  ],
  "timestamp": "2016-01-09 02:02:12Z",
  "trace_id": "255d1aef-8c98-452f-ac51-23d051240864",
  "correlation_id": "fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7"
}
```

| Parameetri | Kirjeldus |
| ----------------------- | ------------------------------- |
| tõrge | Kuvatakse tõrge koodi string, mis saab liigitada tüüpi ilmnevad tõrked ja seda saab kasutada reageerida tõrgete. |
| error_description | Teatud tõrketeade, mis aitab tuvastada autentimise tõrke põhjuseks arendaja.  |
| error_codes | STS kindlate tõrkekoodide, mis aitavad diagnostika loend.  |
| ajatempli | Aeg, mille viga. |
| trace_id | Koosolekukutse, mis aitavad diagnostika ainuidentifikaator.  |
| correlation_id | Koosolekukutse, mis aitavad diagnostika üle komponendid ainuidentifikaator. |

Kuvatavad tõrkekoodid ja Soovitatavad kliendi toimingu kirjeldust, leiate [tõrkekoodid Turbeloa lõpp-punkti tõrkeid](#error-codes-for-token-endpoint-errors).
