<properties
    pageTitle="Azure AD .NET protokolli ülevaade | Microsoft Azure'i"
    description="Selles artiklis kirjeldatakse, kuidas lubada juurdepääsu veebirakenduste ja web API-de oma rentniku Azure Active Directory ja OpenID Connect abil HTTP sõnumite abil."
    services="active-directory"
    documentationCenter=".net"
    authors="priyamohanram"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/03/2016"
    ms.author="priyamo"/>


# <a name="authorize-access-to-web-applications-using-openid-connect-and-azure-active-directory"></a>Luba juurdepääs veebirakenduste OpenID ühendamine ja Azure Active Directory abil

[OpenID ühenduse loomine](http://openid.net/specs/openid-connect-core-1_0.html) on lihtne identiteedi kiht ehitatud OAuth 2.0 Protocol (protokoll). OAuth 2.0 määratleb hankimine ja kasutada **Accessi sõned** kaitstud ressursse, kuid need määratleda standard meetodite identiteedi teavet. OpenID ühendus rakendab autentimise pikendamine OAuth 2.0 autoriseerimine protsessi teavet lõppkasutaja kujul on `id_token` mis kinnitab, et kasutaja ID, samuti pakub profiiliteavet kasutaja kohta.

Kui olete hoone veebirakendus, mis on majutatud serveris ja brauseri kaudu juurde, on OpenID ühendust meie soovitus.

## <a name="authentication-flow-using-openid-connect"></a>Autentimise meilivoo abil OpenID ühenduse loomine

Kõige olulisemaid meilivoo sisaldab järgmist – üks neist kirjeldatakse täpsemalt allpool.

![Ühenduse loomine OpenId autentimise meilivoo](media/active-directory-protocols-openid-connect-code/active-directory-oauth-code-flow-web-app.png)


## <a name="send-the-sign-in-request"></a>Sisselogimise kutse saatmine

Kui oma veebirakenduse peab kasutaja autentida, peab see suunaks kasutajal on `/authorize` lõpp-punkti. See kutse sarnaneb esimesel etapil on [OAuthi 2.0 autoriseerimine kood Flow](active-directory-protocols-oauth-code.md), mõned olulised erinevused.

- Kutse peab sisaldama ulatuse `openid` klõpsake soovitud `scope` parameeter.
- Funktsiooni `response_type` parameeter peab sisaldama `id_token`.
- Kutse peab sisaldama soovitud `nonce` parameeter.

Nii valimi taotluse näeks välja järgmine:

```
// Line breaks for legibility only

GET https://login.microsoftonline.com/{tenant}/oauth2/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=id_token
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=form_post
&scope=openid
&state=12345
&nonce=7362CAEA-9CA5-4B43-9BA3-34D7C303EBA7
```

| Parameetri | | Kirjeldus |
| ----------------------- | ------------------------------- | --------------- |
| rentniku | nõutav | Funktsiooni `{tenant}` väärtus taotluse tee saab kontrollida, kes saavad logida rakendusse.  Lubatud väärtused on rentniku identifikaatorite, nt `8eaef023-2b34-4da1-9baa-8bc8c9d6a490` või `contoso.onmicrosoft.com` või `common` rentniku sõltumatul märkide |
| client_id | nõutav | Rakenduse Id määratud rakenduse registreerimisel Azure AD. See leiate Azure'i klassikaline portaalis. Klõpsake **Active Directory**, klõpsake kausta, klõpsake rakendus ja seejärel klõpsake **konfigureerimine** |
| response_type | nõutav | Peab sisaldama `id_token` jaoks OpenID Connect Logi sisse.  See võib sisaldada ka muid response_types, näiteks `code`. |
| ulatus | nõutav | Tühikuga eraldatud loend otsinguulatuste.  OpenID Connect peab sisaldama ulatuse `openid`, mis tähendab, et "Teid sisse logida" luba UI õigus.  Selle taotluse taotleb luba võib sisaldada ka muid otsinguulatuste. |
| nonss | nõutav | Väärtus, mis on kaasatud taotluse loodud rakendust, kaasatakse tulemuseks `id_token` nagu nõue.  Rakenduse saate seejärel veenduge, et see väärtus leevendada loa kordus eest.  Väärtus on tavaliselt randomiseeritud, kordumatu string või GUID, mida saate kasutada päritolu ja taotluse.  |
| redirect_uri | soovitatav | Rakenduse, kus autentimine vastuseid saate saadetud ja vastuvõetud rakenduse redirect_uri.  See peab täpselt vastama ühte redirect_uris, mis on registreeritud portaali, kuid peab olema URL-ina kodeeritav. |
| response_mode | soovitatav | Saate määrata meetod, mis tuleks kasutada saatma tulemuseks authorization_code rakenduse.  Toetatud väärtused on `form_post` jaoks *HTTP vormi postitamine* või `fragment` jaoks *URL-i fragment*.  Veebirakenduste soovitame kasutada `response_mode=form_post` kõige turvalisem edastamise sõned rakenduse tagamiseks.  
| olek | soovitatav | Väärtus, mis sisaldab taotluse, mis tagastatakse loa vastuse.  See võib olla mis tahes sisu, mida soovite string.  Juhuslik kordumatu väärtus on tavaliselt kasutatakse [vältimine saitidevaheline taotluse võltsimist eest](http://tools.ietf.org/html/rfc6749#section-10.12).  Riigi kasutatakse ka kodeerida teavet rakenduse kasutaja olekus enne autentimine taotluse ilmnes, näiteks lehe või vaate nad. |
| küsimus | valikuline. | Näitab, et kasutaja suhtlus, mida on vaja tüüp.  Praegu kehtivad ainult väärtused on 'sisselogimine","pole"ja"nõustu.  `prompt=login`sunnib kasutajal sisestada oma volitusi selle taotluse hindamine ühekordse sisselogimise kohta.  `prompt=none`on vastand - tagab, et kasutaja ei ole esitatud mis tahes interaktiivsed küsimus üldse.  Kui taotlust ei saa lõpule viia vaikselt teel ühekordse sisselogimise kohta, lõpp-punkti tagastavad vea.  `prompt=consent`käivitab OAuthi nõusolekut dialoogiboksi pärast kasutaja märke, kasutajalt rakenduse õigusi anda. |
| login_hint | valikuline. | Saab kasutada eelnevalt täitmiseks välja kasutajanimi/e-posti aadress sisselogimislehe kasutaja, kui te ei tea oma kasutajanime ette valmistada.  Kasutatakse sageli rakenduste see parameeter on juba ekstraktimist kasutajanimi on eelmise sisselogimise abil uuesti autentimisel on `preferred_username` taotlemine. |


Selles etapis küsitakse kasutaja autentimise ja sisestage oma kasutajanimi ja parool.

### <a name="sample-response"></a>Valimi vastus

Valimi vastust, kui kasutaja on kinnitatud, võib välja näha selline:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNB...&state=12345
```

| Parameetri | Kirjeldus |
| ----------------------- | ------------------------------- |
| id_token | Funktsiooni `id_token` soovitud rakendus. Saate kasutada funktsiooni `id_token` kasutaja identiteedi ja kasutajale seansi alustada.  |
| olek | Väärtus, mis sisaldab taotluse, mis tagastatakse loa vastuse. Juhuslik kordumatu väärtus on tavaliselt kasutatakse [vältimine saitidevaheline taotluse võltsimist eest](http://tools.ietf.org/html/rfc6749#section-10.12).  Riigi kasutatakse ka kodeerida teavet rakenduse kasutaja olekus enne autentimise taotluse ilmnes, näiteks lehe või vaate nad. |

### <a name="error-response"></a>Tõrge vastus
Tõrge vastuste võib ka saata selle `redirect_uri` nii, et rakendus võib käsitleda neid õigesti:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

error=access_denied&error_description=the+user+canceled+the+authentication
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

## <a name="validate-the-idtoken"></a>Kinnitage soovitud id_token

Ainult vastu ka `id_token` ei piisa autentida; peate allkirja valideerimiseks ja veenduge, et nõuded on `id_token` oma rakenduse nõuete kohta. Azure AD lõpp-punkti kasutab JSON Web sõned (JWTs) ja avaliku võtme krüptograafia sõned sisse ja veenduge, et need on lubatud.

Saate valida valideerimiseks on `id_token` kliendi koodi, kuid levinud on saatmiseks on `id_token` taustväärtus serveris ja teha seal valideerimine. Kui te olete kontrollitud allkirja selle `id_token`, on mõned nõuded tuleb teil kinnitamiseks.

Samuti võite täiendavate taotluste sõltuvalt sellest, milline olukord teie domeenist. Mõned levinud valideerimised on järgmised.

- Kasutaja/ettevõtte tagada on registreerunud rakendus.
- Tagada, et kasutajal on õige autoriseerimine ja õigused
- Tagada teatud tugevuse autentimine on ilmnenud, nt mitmikautentimise.

Kui teil on täielikult kontrollitud ja selle `id_token`, saate alustada kasutaja seansi ja kasutada nõuded on `id_token` rakenduse kasutaja kohta teabe saamiseks. Seda teavet saab kasutada Kuva, kirjeid, lubades jne. Turbeloa tüübid ja nõuete kohta lisateabe saamiseks lugege [toetatud Turbeloa ja taotluste tüübid](active-directory-token-and-claims.md).

## <a name="send-a-sign-out-request"></a>Välja logida kutse saatmine

Kui soovite kasutaja rakenduse välja logida, pole piisavalt tühjendage oma rakenduse küpsised või muul viisil Lõpeta seanss ja kasutajale.  Samuti peate häälestama ümbersuunamise kasutajal on `end_session_endpoint` jaoks Logi välja.  Kui te ei tee, saab kasutaja uuesti autentimiseks rakenduse sisestamata volituste uuesti, kuna need on lubatud ühekordse sisselogimise seansi Azure AD lõpp-punkti.

Saate lihtsalt ümber suunata kasutajal on `end_session_endpoint` OpenID Connect metaandmete dokumendis loetletud:

```
GET https://login.microsoftonline.com/common/oauth2/logout?
post_logout_redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F

```

| Parameetri | | Kirjeldus |
| ----------------------- | ------------------------------- | ------------ |
| post_logout_redirect_uri | soovitatav | Kasutaja peaks ümber pärast eduka välju URL.  Kui ei sisalda kasutaja kuvatakse üldise sõnumi.  |

## <a name="token-acquisition"></a>Turbeloa saamine

Veebirakenduste palju vaja Logige kasutaja, vaid ka juurdepääs veebiteenuse abil OAuthi kasutaja nimel. See stsenaarium ühendab OpenID Connect kasutaja autentimise ajal samaaegselt tarkvara on `authorization_code` saamiseks kasutatakse `access_tokens` OAuthi autoriseerimine kood Flow abil.

## <a name="get-access-tokens"></a>Saada juurdepääsu sõned

Hankida access sõned, peate muutmine Logi sisse taotluse ülevalt.

```
// Line breaks for legibility only

GET https://login.microsoftonline.com/{tenant}/oauth2/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e      // Your registered Application Id
&response_type=id_token+code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F       // Your registered Redirect Uri, url encoded
&response_mode=form_post                              // form_post', or 'fragment'
&scope=openid
&resource=https%3A%2F%2Fservice.contoso.com%2F                                   
&state=12345                                         // Any value, provided by your app
&nonce=678910                                        // Any value, provided by your app
```

Sh õigus otsinguulatuste taotlus ja abil `response_type=code+id_token`, `authorize` lõpp-punkti tagab, et kasutajal on andnud õigused, nagu on näidatud selle `scope` päringu parameetrite ja tagastada rakenduse juurdepääsu sümboolse vahetada mõne kood.

### <a name="successful-response"></a>Eduka vastus

Eduka vastuse kasutades `response_mode=form_post` näeb välja nagu:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNB...&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&state=12345
```

| Parameetri | Kirjeldus |
| ----------------------- | ------------------------------- |
| id_token | Funktsiooni `id_token` soovitud rakendus. Saate kasutada funktsiooni `id_token` kasutaja identiteedi ja kasutajale seansi alustada. |
| kood | Authorization_code, mis rakenduse nõutav. Rakenduse saate kasutada taotleda juurdepääsu ressursile target sümboolse luba kood. Authorization_codes on väga lühiajalise, tavaliselt need aeguvad umbes 10 minuti pärast. |
| olek | Kui parameeter olek on kaasatud kutse, kuvatakse sama väärtuse vastus. Rakendus peaks veenduge, et koosolekukutsete ja kutsele vastamise olekus väärtused on identne. |

### <a name="error-response"></a>Tõrge vastus

Tõrge vastuste võib ka saata selle `redirect_uri` nii, et rakendus võib käsitleda neid õigesti:

```
POST /myapp/ HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded

error=access_denied&error_description=the+user+canceled+the+authentication
```

| Parameetri | Kirjeldus |
| ----------------------- | ------------------------------- |
| tõrge | Kuvatakse tõrge koodi string, mis saab liigitada tüüpi ilmnevad tõrked ja seda saab kasutada reageerida tõrgete. |
| error_description | Teatud tõrketeade, mis aitab tuvastada autentimise tõrke põhjuseks arendaja.  |

Võimalikud tõrkekoodid ja nende soovitatav kliendi toimingu kirjeldus, leiate [tõrkekoodid autoriseerimine lõpp-punkti tõrkeid](#error-codes-for-authorization-endpoint-errors).

Kui olete saanud luba `code` ja `id_token`, saate kasutaja sisselogimine ja saada juurdepääsu sõned nende nimel.  Kasutaja sisse logida, peate kontrollima selle `id_token` täpselt nii, nagu eespool kirjeldatud. Märkide juurdepääsu saamiseks võite järgida meie [OAuthi protokolli dokumentatsioon](active-directory-protocols-oauth-code.md#Use-the-Authorization-Code-to-Request-an-Access-Token)kirjeldatud juhiseid.
