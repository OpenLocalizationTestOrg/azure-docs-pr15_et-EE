<properties
    pageTitle="Azure'i AD v2.0 peidetud Flow | Microsoft Azure'i"
    description="Koosteüksuse veebirakenduste kasutamise ühelt leheküljelt rakenduste Azure AD v2.0 rakendamine peidetud voogu."
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
    ms.date="09/16/2016"
    ms.author="dastrock"/>

# <a name="v20-protocols---spas-using-the-implicit-flow"></a>v2.0 Protokollid - SPAs peidetud voo abil
V2.0 lõpp-punkt, kus saate sisse logida kasutajad Microsofti kontoga isiklik ja töö/kool rakenduste ühe lehe.  Ühelt leheküljelt ja muud JavaScripti rakendused, mis töötavad eelkõige huvitav mõned probleemid, kui tegemist on autentimine brauseri näo:

- Nende rakenduste turvalisus omadused on oluliselt erinev traditsiooniline serveripõhine veebirakenduste.
- Mitme serverid autoriseerimine ja identiteedipakkujad ei toeta CORS taotlused.
- Tervet lehte brauser suunab muutuda eriti invasiivse, et kasutusvõimalused rakenduse eemale.

Nende rakenduste (arvates: AngularJS, Ember.js, React.js, jne) Azure AD toetab OAuthi 2.0 peidetud anda voogu.  Peidetud voogu on kirjeldatud [OAuthi 2.0 määratlus](http://tools.ietf.org/html/rfc6749#section-4.2).  Esmane kasu on, et see võimaldab rakenduse toomine sõned Azure AD ilma kirjutamata serveri identimisteavet exchange.  See võimaldab rakenduse kasutaja sisse logida, säilitada seanss ja saada sõned muude veebi API-de kõik kliendi sees JavaScripti koodi.  On mõned olulised turvakaalutlused peidetud voo - täpsemalt ümber [Klient](http://tools.ietf.org/html/rfc6749#section-10.3) ja [kasutajale isikustamine](http://tools.ietf.org/html/rfc6749#section-10.3)kasutamisel arvesse võtta.

Kui soovite peidetud voogu ja Azure AD abil saate lisada JavaScripti rakenduse autentimine, soovitame kasutada meie avatud allika JavaScripti teegi [adal.js](https://github.com/AzureAD/azure-activedirectory-library-for-js).  Mõned AngularJS õpetused on saadaval [siin](active-directory-appmodel-v2-overview.md#getting-started) , et aidata teil alustada.  

Juhul, kui te ei soovi kasutada Raamatukogu ühelt leheküljelt rakenduse protokoll sõnumite ja ise, järgige üldine alltoodud juhiseid.

> [AZURE.NOTE]
    Kõik Azure Active Directory stsenaariumid ja funktsioonid on toetatud v2.0 lõpp-punkt.  Kui kasutate v2.0 lõpp-punkti määramiseks lugege [v2.0 piirangud](active-directory-v2-limitations.md).
    
## <a name="protocol-diagram"></a>Protocol (protokoll) skeem
Kogu peidetud sisselogimine meilivoo näeb välja umbes selline - iga juhiseid kirjeldatakse allpool.

![OpenId ühenduse ujumisradade](../media/active-directory-v2-flows/convergence_scenarios_implicit.png)

## <a name="send-the-sign-in-request"></a>Sisselogimise kutse saatmine

Algselt sisselogimise kasutaja rakenduse, saate saata [OpenID Connect](active-directory-v2-protocols-oidc.md) luba taotluse ja saada mõne `id_token` v2.0 lõpp:

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=id_token+token
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&scope=openid%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read
&response_mode=fragment
&state=12345
&nonce=678910
```

> [AZURE.TIP] Klõpsake linki selle taotluse! Pärast sisselogimist, peaks teie brauser ümber `https://localhost/myapp/` koos mõne `id_token` aadressiribale.
    <a href="https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=id_token+token&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&scope=openid%20https%3A%2F%2Fgraph.microsoft.com%2Fmail.read&response_mode=fragment&state=12345&nonce=678910" target="_blank">https://login.microsoftonline.com/common/oauth2/v2.0/authorize...</a>

| Parameetri | | Kirjeldus |
| ----------------------- | ------------------------------- | --------------- |
| rentniku | nõutav | Funktsiooni `{tenant}` väärtus taotluse tee saab kontrollida, kes saavad logida rakendusse.  Lubatud väärtused on `common`, `organizations`, `consumers`, ja rentnikuhalduse identifikaatorid.  Lugege täpsemalt [protokolli põhitõed](active-directory-v2-protocols.md#endpoints). |
| client_id | nõutav | Rakenduse Id, et registreerimise portaali ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) määratud rakenduse. |
| response_type | nõutav | Peab sisaldama `id_token` jaoks OpenID Connect Logi sisse.  See võib sisaldada ka selle response_type `token`. Kasutades `token` siin võimaldab rakenduse saada juurdepääsu sümboolse kohe Autoriseerin lõpp-punkti ilma teha teise taotluse Autoriseerin lõpp-punkti.  Kui kasutate funktsiooni `token` response_type, on `scope` parameeter peab sisaldama, mille ulatus, mis näitab, milliseid ressursi väljaandmisel sõnet. |
| redirect_uri | soovitatav | Rakenduse, kus autentimine vastuseid saate saadetud ja vastuvõetud rakenduse redirect_uri.  See peab täpselt vastama ühte redirect_uris, mis on registreeritud portaali, kuid peab olema URL-ina kodeeritav. |
| ulatus | nõutav | Tühikuga eraldatud loend otsinguulatuste.  OpenID Connect peab sisaldama ulatuse `openid`, mis tähendab, et "Teid sisse logida" luba UI õigus.  Soovi korral võite ka kaasamiseks soovitud `email` või `profile` [otsinguulatuste](active-directory-v2-scopes.md) juurdepääsu täiendavad kasutaja andmeid.  Muud otsinguulatuste võib sisaldada ka selle taotluse taotlemise nõusolekut erinevate ressursse.  |
| response_mode | soovitatav | Saate määrata meetod, mis tuleks kasutada rakenduse tulemiks oleva Turbeloa tagasi saatmiseks.  Peaks olema `fragment` peidetud meilivoo jaoks.  |
| olek | soovitatav | Väärtus, mis sisaldab taotluse, mis tagastatakse loa vastuse.  See võib olla mis tahes sisu, mida soovite string.  Juhuslik kordumatu väärtus on tavaliselt kasutatakse [vältimine saitidevaheline taotluse võltsimist eest](http://tools.ietf.org/html/rfc6749#section-10.12).  Riigi kasutatakse ka kodeerida teavet rakenduse kasutaja olekus enne autentimine taotluse ilmnes, näiteks lehe või vaate nad. |
| nonss | nõutav | Väärtus, mis on loodud rakendust, kaasatakse tulemuseks id_token nimega nõude taotluse kaasatud.  Rakenduse saate seejärel veenduge, et see väärtus leevendada loa kordus eest.  Väärtus on tavaliselt randomiseeritud, kordumatu string, mis saab taotluse päritolu.  |
| küsimus | valikuline. | Näitab, et kasutaja suhtlus, mida on vaja tüüp.  Sel ajal kehtivad ainult väärtused on 'sisselogimine","pole, ja nõus".  `prompt=login`sunnib kasutajal sisestada oma volitusi selle taotluse hindamine ühekordse sisselogimise kohta.  `prompt=none`on vastand - tagab, et kasutaja ei ole esitatud mis tahes interaktiivsed küsimus üldse.  Kui taotlust ei saa lõpule viia vaikselt teel ühekordse sisselogimise kohta, v2.0 lõpp-punkti tagastavad vea.  `prompt=consent`käivitab OAuthi nõusolekut dialoogiboksi pärast kasutaja märke, kasutajalt rakenduse õigusi anda. |
| login_hint | valikuline. | Saab kasutada eelnevalt täitmiseks välja kasutajanimi/e-posti aadress sisselogimislehe kasutaja, kui te ei tea oma kasutajanime ette valmistada.  Kasutatakse sageli rakenduste see parameeter on juba ekstraktimist kasutajanimi on eelmise sisselogimise abil uuesti autentimisel on `preferred_username` taotlemine. |
| domain_hint | valikuline. | Võib olla üks `consumers` või `organizations`.  Kui, see e-postipõhise discovery protsessi vahele jätta kasutaja läheb läbi v2.0 sisselogimislehel ees on veidi sujuv kasutajaliides.  Sageli rakendused saavad kasutada see parameeter uuesti autentimisel, ekstraktimiseks soovitud `tid` on id_token nõuda.  Kui soovitud `tid` taotlemine väärtus on `9188040d-6c67-4c5b-b112-36a304b66dad`, kasutage `domain_hint=consumers`.  Muul juhul kasutada `domain_hint=organizations`. |

Selles etapis küsitakse kasutaja autentimise ja sisestage oma kasutajanimi ja parool.  Lõpp-punkti v2.0 tagab, et kasutajal on andnud õigused, nagu on näidatud ka selle `scope` päringu parameeter.  Kui kasutaja ei ole nõus mis tahes need õigused, seda küsib kasutaja nõusolekut vajalikke õigusi.  [Õiguste, nõusoleku ja mitme rentniku rakendused on siin esitatud](active-directory-v2-scopes.md)üksikasjad.

Kui kasutaja autendib ja annab nõusoleku, v2.0 lõpp-punkti tulemuseks rakenduse juures märgitud vastuseks `redirect_uri`, määratud meetodil on `response_mode` parameeter.

#### <a name="successful-response"></a>Eduka vastus

Eduka vastuse kasutades `response_mode=fragment` ja `response_type=id_token+token` näeb reapiiridega loetavuse järgmist:

```
GET https://localhost/myapp/#
access_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&token_type=Bearer
&expires_in=3599
&scope=https%3a%2f%2fgraph.microsoft.com%2fmail.read 
&id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&state=12345
```

| Parameetri | Kirjeldus |
| ----------------------- | ------------------------------- |
| access_token | Kui kaasatud `response_type` sisaldab `token`. Soovitud rakendus, kui Microsoft Graphi juurdepääsu luba.  Luba juurdepääs ei tohiks dekodeerida või muul viisil kontrollida, võib käsitleda on läbipaistmatu string. |
| token_type | Kui kaasatud `response_type` sisaldab `token`.  Alati `Bearer`. |
| expires_in | Kui kaasatud `response_type` sisaldab `token`.  Näitab luba on kehtiv vahemällu eesmärgil sekundite arv. |
| ulatus | Kui kaasatud `response_type` sisaldab `token`.  Näitab, mis on access_token kehtivad järgmised ulatused. |
| id_token | Id_token, mis rakenduse nõutav. Saate selle id_token kasutaja identiteedi ja kasutajale seansi alustada.  Lisateavet selle kohta id_tokens ja nende sisu on kaasatud [v2.0 lõpp-punkti Turbeloa viide](active-directory-v2-tokens.md).  |
| olek | Kui parameeter olek on kaasatud kutse, kuvatakse sama väärtuse vastus. Rakendus peaks kinnitamine olekus väärtused koosolekukutsete ja kutsele vastamise identsed. |


#### <a name="error-response"></a>Tõrge vastus
Tõrge vastuste võib ka saata selle `redirect_uri` nii, et rakendus võib käsitleda neid õigesti:

```
GET https://localhost/myapp/#
error=access_denied
&error_description=the+user+canceled+the+authentication
```

| Parameetri | Kirjeldus |
| ----------------------- | ------------------------------- |
| tõrge | Kuvatakse tõrge koodi string, mis saab liigitada tüüpi ilmnevad tõrked ja seda saab kasutada reageerida tõrgete. |
| error_description | Teatud tõrketeade, mis aitab tuvastada autentimise tõrke põhjuseks arendaja.  |

## <a name="validate-the-idtoken"></a>Kinnitage soovitud id_token
Ainult mõne id_token vastuvõtmine ei piisa autentida; peate selle id_token allkirja valideerimiseks ja kinnitamine luba kohta oma rakenduse nõuded nõuded.  V2.0 lõpp-punkti kasutab [JSON Web sõned (JWTs)](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) ja avaliku võtme krüptograafia sõned sisse ja veenduge, et need on lubatud.

Saate valida valideerimiseks on `id_token` kliendi koodi, kuid levinud on saatmiseks on `id_token` taustväärtus serveris ja teha seal valideerimine.  Kui olete kinnitanud selle id_token allkirja, on mõned nõuded tuleb teil kinnitamiseks.  Vt rohkem teavet, sealhulgas [Sõned kontrollimine](active-directory-v2-tokens.md#validating-tokens) ja [Olulist teavet kohta sisselogimise klahvi edastamiseks](active-directory-v2-tokens.md#validating-tokens) [v2.0 Turbeloa viide](active-directory-v2-tokens.md) .  Soovitame kasutada teegi sõelumine ja kontrollimine märkide - on vähemalt üks saadaval enamiku keelte ja platvormide jaoks.
<!--TODO: Improve the information on this-->

Samuti võite täiendavate taotluste sõltuvalt sellest, milline olukord teie domeenist.  Mõned levinud valideerimised on järgmised.

- Kasutaja/ettevõtte tagada on registreerunud rakendus.
- Tagada, et kasutajal on õige autoriseerimine ja õigused
- Tagada teatud tugevuse autentimine on ilmnenud, nt mitmikautentimise.

Nõuded on id_token kohta leiate lisateavet teemast [v2.0 lõpp-punkti Turbeloa viide](active-directory-v2-tokens.md).

Kui teil on id_token täielikult kinnitanud, saate alustada seansi kasutaja ja rakenduse kasutaja kohta teabe saamiseks selle id_token nõuete kasutamine.  Seda teavet saab kasutada Kuva, kirjeid, lubades jne.

## <a name="get-access-tokens"></a>Saada juurdepääsu sõned

Nüüd, kui te olete sisse logitud kasutaja rakenduse ühele lehele, saate avada Accessi sõned helistaja web turvatud Azure AD, nt [Microsoft Graphi](https://graph.microsoft.io)API-de jaoks.  Isegi juhul, kui olete juba saanud Turbeloa abil soovitud `token` response_type, saate kasutada seda meetodit omandada sõned lisaressurssidele suunata kasutajal uuesti sisse logida ilma.

Rakenduses tavaline OpenID ühendamine/OAuthi voogu, siis oleks seda tehes taotluse selle v2.0 `/token` lõpp-punkti.  Siiski ei toeta v2.0 lõpp-punkti CORS taotlusi, nii, et saada ja värskendada sõned AJAXI helistamist ei saa.  Selle asemel saate peidetud voogu peidetud IFRAME uue sõned saamiseks muude web API-de: 

```
// Line breaks for legibility only

https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=token
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&scope=https%3A%2F%2Fgraph.microsoft.com%2Fmail.read&response_mode=fragment
&state=12345&nonce=678910
&prompt=none
&domain_hint=organizations
&login_hint=myuser@mycompany.com
```

> [AZURE.TIP] Proovige kopeerimine ja kleepimine selle all taotluse sisse brauseri vahekaardi! (Ärge unustage asendada selle `domain_hint` ja `login_hint` väärtusi kasutaja õigete väärtuste)

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=token&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&scope=https%3A%2F%2Fgraph.microsoft.com%2Fmail.read&response_mode=fragment&state=12345&nonce=678910&prompt=none&domain_hint={{consumers-or-organizations}}&login_hint={{your-username}}
```

| Parameetri | | Kirjeldus |
| ----------------------- | ------------------------------- | --------------- |
| rentniku | nõutav | Funktsiooni `{tenant}` väärtus taotluse tee saab kontrollida, kes saavad logida rakendusse.  Lubatud väärtused on `common`, `organizations`, `consumers`, ja rentnikuhalduse identifikaatorid.  Lugege täpsemalt [protokolli põhitõed](active-directory-v2-protocols.md#endpoints). |
| client_id | nõutav | Rakenduse Id, et registreerimise portaali ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) määratud rakenduse. |
| response_type | nõutav | Peab sisaldama `id_token` jaoks OpenID Connect Logi sisse.  See võib sisaldada ka muid response_types, näiteks `code`. |
| redirect_uri | soovitatav | Rakenduse, kus autentimine vastuseid saate saadetud ja vastuvõetud rakenduse redirect_uri.  See peab täpselt vastama ühte redirect_uris, mis on registreeritud portaali, kuid peab olema URL-ina kodeeritav. |
| ulatus | nõutav | Tühikuga eraldatud loend otsinguulatuste.  Kaasake saamisega märkide kõik [otsinguulatuste](active-directory-v2-scopes.md) vajate intressi ressursi.  |
| response_mode | soovitatav | Saate määrata meetod, mis tuleks kasutada rakenduse tulemiks oleva Turbeloa tagasi saatmiseks.  Võib olla üks `query`, `form_post`, või `fragment`.  |
| olek | soovitatav | Väärtus, mis sisaldab taotluse, mis tagastatakse loa vastuse.  See võib olla mis tahes sisu, mida soovite string.  Juhuslik kordumatu väärtus on tavaliselt kasutatakse vältimine saitidevaheline taotluse võltsimist eest.  Riigi kasutatakse ka kodeerida teavet rakenduse kasutaja olekus enne autentimine taotluse ilmnes, näiteks lehe või vaate nad. |
| nonss | nõutav | Väärtus, mis on loodud rakendust, kaasatakse tulemuseks id_token nimega nõude taotluse kaasatud.  Rakenduse saate seejärel veenduge, et see väärtus leevendada loa kordus eest.  Väärtus on tavaliselt randomiseeritud, kordumatu string, mis saab taotluse päritolu.  |
| küsimus | nõutav | Värskendamise ja saada sõned peidetud IFRAME'i, peaksite kasutama `prompt=none` tagamiseks IFRAME'i ei hanguda v2.0 sisselogimislehel ja tagastab kohe. |
| login_hint | nõutav | Värskendamine & saada sõned peidetud IFRAME'i, peab kasutaja kasutajanimi sisaldama eristamiseks mitme seansid kasutaja võib olla antud hetkel ajas see vihje. Eelmise sisselogimist kasutades saate ekstraktida kasutajanimi on `preferred_username` taotlemine. |
| domain_hint | nõutav | Võib olla üks `consumers` või `organizations`.  Värskendamise & saada sõned peidetud IFRAME'i, peab sisaldama soovitud domain_hint kutse.  Mida tuleks ekstraktida soovitud `tid` nõuda id_token eelmise sisselogimine millist väärtuse määramiseks.  Kui soovitud `tid` taotlemine väärtus on `9188040d-6c67-4c5b-b112-36a304b66dad`, kasutage `domain_hint=consumers`.  Muul juhul kasutada `domain_hint=organizations`. |

Tänu selle `prompt=none` parameeter, taotlus kas edu või nurjuda kohe ja naaske rakenduse.  Eduka vastus saadetakse rakenduse juures on näidatud `redirect_uri`, määratud meetodil on `response_mode` parameeter.

#### <a name="successful-response"></a>Eduka vastus
Eduka vastuse kasutades `response_mode=fragment` näeb välja nagu:

```
GET https://localhost/myapp/#
access_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&state=12345
&token_type=Bearer
&expires_in=3599
&scope=https%3A%2F%2Fgraph.windows.net%2Fdirectory.read
```

| Parameetri | Kirjeldus |
| ----------------------- | ------------------------------- |
| access_token | Luba soovitud rakendus. |
| token_type | Alati `Bearer`. |
| olek | Kui parameeter olek on kaasatud kutse, kuvatakse sama väärtuse vastus. Rakendus peaks kinnitamine olekus väärtused koosolekukutsete ja kutsele vastamise identsed. |
| expires_in | Kui kaua kehtib juurdepääsu luba (sekundites). |
| ulatus | Otsinguulatuste, mis juurdepääsu luba kehtib. |

#### <a name="error-response"></a>Tõrge vastus
Tõrge vastuste võib ka saata selle `redirect_uri` nii, et rakendus võib käsitleda neid õigesti.  Kui `prompt=none`, on oodatud tõrge:

```
GET https://localhost/myapp/#
error=user_authentication_required
&error_description=the+request+could+not+be+completed+silently
```

| Parameetri | Kirjeldus |
| ----------------------- | ------------------------------- |
| tõrge | Kuvatakse tõrge koodi string, mis saab liigitada tüüpi ilmnevad tõrked ja seda saab kasutada reageerida tõrgete. |
| error_description | Teatud tõrketeade, mis aitab tuvastada autentimise tõrke põhjuseks arendaja.  |

Kui kuvatakse järgmine tõrketeade IFRAME'i taotluse, kasutaja peab interaktiivseks uuesti sisse logida uue loa toomiseks.  Saate valida, klõpsake mis tahes viisil on mõistlik rakenduse sel juhul käsitlema.

## <a name="refreshing-tokens"></a>Märkide värskendamine

Mõlemad `id_token`s ja `access_token`s aegub pärast lühikest aega, nii et teie rakendus peab olema valmis neid värskendada märkide perioodiliselt.  Luba tüüpi värskendamiseks saate teha sama peidetud IFRAME'i taotluse ülevalt, kasutades selleks funktsiooni `prompt=none` parameeter määrata Azure AD käitumine.  Kui soovite saada uut `id_token`, kasutage kindlasti `response_type=id_token` ja `scope=openid`, kui ka `nonce` parameeter.


## <a name="send-a-sign-out-request"></a>Välja logida kutse saatmine

Funktsiooni OpenIdConnect `end_session_endpoint` v2.0 lõpp-punkt praegu ei toetata. See tähendab, et teie rakendus ei saa saata v2.0 lõpp-punkti kasutaja seansi lõpetamiseks ja küpsiste määratud v2.0 lõpp-punkti kustutamine.
Kasutaja välja logima lihtsalt saate rakenduse oma kasutaja seansi lõpetamine ja jätta kasutaja seansi soovitud v2.0 lõpp-punkti sisse-taktitunne.  Järgmine kord, kui kasutaja proovib sisselogimine, nad näevad lehel "valige konto" kontodega tema aktiivselt sisse loginud sisse loetletud.
Sellel lehel saate Kasutaja väljalogimine kontoga, seansi koos v2.0 lõpp-punkti.

<!--

When you wish to sign the user out of the  app, it is not sufficient to clear your app's cookies or otherwise end the session with the user.  You must also redirect the user to the v2.0 endpoint for sign out.  If you fail to do so, the user will be able to re-authenticate to your app without entering their credentials again, because they will have a valid single sign-on session with the v2.0 endpoint.

You can simply redirect the user to the `end_session_endpoint` listed in the OpenID Connect metadata document:

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/logout?
post_logout_redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
```

| Parameter | | Description |
| ----------------------- | ------------------------------- | ------------ |
| post_logout_redirect_uri | recommended | The URL which the user should be redirected to after successful logout.  If not included, the user will be shown a generic message by the v2.0 endpoint.  |

-->