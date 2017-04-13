<properties
    pageTitle="Azure Active Directory B2C | Microsoft Azure'i"
    description="Hoone veebirakendusi OpenID ühendamine Azure Active Directory rakendamise abil autentimine."
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
    ms.topic="article"
    ms.date="07/22/2016"
    ms.author="dastrock"/>

# <a name="azure-active-directory-b2c-web-sign-in-with-openid-connect"></a>Azure Active Directory B2C: Veebis sisselogimine OpenID Connect

OpenID Connect pole autentimise protokoll, mis ehitatud OAuth 2.0, mida saab kasutada veebirakenduste kasutajad turvaliseks sisselogimiseks.  OpenID Connect Azure Active Directory (Azure AD) B2C rakendamise abil saate tellida registreerumise, Logi sisse ja muude identiteetide haldamisviis kogemusi oma veebirakendusi Azure AD. Sellest juhendist näitab teile, kuidas seda teha keele sõltumatul viisil. See kirjeldab, kuidas sõnumite saatmiseks ja vastuvõtmiseks HTTP ühte meie avatud lähtekoodi teekide kasutamata.

[OpenID ühendamine](http://openid.net/specs/openid-connect-core-1_0.html) laiendab OAuth 2.0 *autoriseerimine* protokoll kasutamiseks on *autentimine* Protocol (protokoll). See võimaldab teil teha ühekordse sisselogimise OAuthi abil. Mõistet, mis `id_token`, mis on Turbeloa, mis võimaldab kasutaja identiteedi ja kasutajale põhiprofiil teavet.

Kuna see laiendab OAuth 2.0, samuti võimaldab turvaliselt omandada **access_tokens**rakendused. Saate kasutada Accessi ressursse, mis on tagatud ettevõtte [autoriseerimine server](active-directory-b2c-reference-protocols.md#the-basics)access_tokens. Soovitame OpenID ühenduse loomine, kui olete hoone veebirakenduse, mis on majutatud serveris ja brauseri kaudu. Kui soovite lisada identiteetide haldamisviis mobiil- või töölaua rakenduste abil Azure AD B2C, kasutage [OAuth 2.0](active-directory-b2c-reference-oauth-code.md) asemel OpenID ühendus.

Azure'i AD B2C laiendab standard OpenID Connect protokoll teha rohkem kui lihtne autentimise ja luba. See toob [**poliitika parameeter**](active-directory-b2c-reference-policies.md), mis võimaldab kasutada OpenID Connect kasutuskogemuse lisamiseks rakenduse – näiteks registreerumise, Logi sisse ja profiili haldus. Siin näitame teile neid kogemusi rakendamiseks oma veebirakendusi OpenID ühendamine ja -poliitikad kasutamise kohta. Samuti näitame teile hankimine access_tokens juurdepääsuks web API-d.

Alltoodud näites HTTP päringuid kasutatakse meie valimi B2C kataloogi, **fabrikamb2c.onmicrosoft.com**, kui ka meie valimi rakenduse **https://aadb2cplayground.azurewebsites.net** ja poliitika. Olete tasuta proovida taotlused ise, kasutades neid väärtusi või saate asendada need oma.
Siit saate teada, kuidas [oma B2C rentniku, rakenduse ja -poliitikad](#use-your-own-b2c-directory).

## <a name="send-authentication-requests"></a>Päringuid autentimine
Kui oma veebirakenduse peab kasutaja autentimiseks ja käivitada poliitika, saate selle otse kasutajal on `/authorize` lõpp-punkti. See on interaktiivne osa voogu, kus kasutaja tegelikult võtab olenevalt poliitika.

Selle taotluse näitab kliendi õigused, mida on vaja hankida kasutaja soovitud `scope` parameetri ja käivitada poliitika on `p` parameeter. Kolm näidet on toodud allpool (reapiirid loetavuse), kus iga luua muu poliitika. Iga taotluse toimimise kohta teada saamiseks proovige kutse kleepides brauseris ja see töötab.

#### <a name="use-a-sign-in-policy"></a>Kasutage sisselogimiseks poliitika

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code+id_token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=form_post
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_sign_in
```

#### <a name="use-a-sign-up-policy"></a>Kasutage registreerumise poliitika

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code+id_token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=form_post
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_sign_up
```

#### <a name="use-an-edit-profile-policy"></a>Kasutage Redigeeri profiili poliitika

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code+id_token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=form_post
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_edit_profile
```

| Parameetri | See on nõutav? | Kirjeldus |
| ----------------------- | ------------------------------- | ----------------------- |
| client_id | Nõutav | Rakenduse ID [Azure portaali](https://portal.azure.com/) rakenduse määratud. |
| response_type | Nõutav | Vastuse tüüp, mis sisaldavad `id_token` OpenID ühenduse loomiseks. Kui teie web Appis peate sõned helistamine veebi-API, saate kasutada `code+id_token`, kuna oleme siin teinud. |
| redirect_uri | Soovitatav | Rakenduse, kus autentimine vastuseid saate saadetud ja vastuvõetud rakenduse redirect_uri. See peab täpselt vastama ühte portaalis registreeritud redirect_uris peab URL-ina kodeeritav puhul. |
| ulatus | Nõutav | Tühikuga eraldatud loend otsinguulatuste. Ühe ulatus väärtus näitab nii Azure AD õiguste, mis on nõutud. Funktsiooni `openid` ulatus näitab õigust kasutaja sisselogimine ja ülevaate andmete kujul **id_tokens** (rohkem teavet selle kohta selle artikli) kasutaja kohta. Funktsiooni `offline_access` ulatus on valikuline web apps. See näitab, et rakenduse peate mõne **refresh_token** vanade juurdepääsuks ressursse. |
| response_mode | Soovitatav | Mida tuleks kasutada saatma tulemuseks authorization_code rakenduse meetodit. See võib olla üks "query", "form_post" või "fragment".  "form_post" on soovitatav parim turvalisus. |
| olek | Soovitatav | Väärtus, mis sisaldab taotluse, mis tagastatakse loa vastuse. See võib olla mis tahes sisu, mida soovite string. Juhuslik kordumatu väärtus on tavaliselt kasutatakse vältimine saitidevaheline taotluse võltsimist eest. Riigi kasutatakse ka kodeerida teavet rakenduse kasutaja olekus autentimise taotluse ilmnemisele nagu nad lehe. |
| nonss | Nõutav | Väärtus, mis sisaldab taotluse (rakenduse abil loodud) tulemuseks id_token nimega nõude kaasatavad. Rakenduse saate seejärel veenduge, et see väärtus leevendada loa kordus eest. Väärtus on tavaliselt randomiseeritud, kordumatu string, mis saab taotluse päritolu. |
| p | Nõutav | Poliitika, mis käivitatakse. See on poliitika, mis on loodud teie B2C rentniku nimi. Poliitika nimi väärtus peaks algama "b2c\_1\_". Lugege lisateavet poliitikate [laiendatav raamistik](active-directory-b2c-reference-policies.md). |
| küsimus | Valikuline. | Kasutaja suhtlus, mida on vaja tüüp. Praegu ainult kehtiv väärtus on 'sisselogimine', mis jõustab kasutajal sisestada oma volitusi taotluse alusel. Ühekordse sisselogimise ei jõustu. |

Selles etapis küsitakse kasutaja poliitika töövoo lõpuleviimiseks. See võib hõlmata kasutajal sisestada oma kasutajanimi ja parool, sisselogimine omase, kataloogi või mis tahes muust numbrist juhiseid, olenevalt sellest, kuidas poliitika on määratletud kasutajaks.

Pärast kasutaja lõpulejõudmist poliitika Azure AD tulemuseks rakenduse juures märgitud vastuseks `redirect_uri`, määratud meetodi abil soovitud `response_mode` parameeter. Vastuse saab täpselt sama, iga ülaltoodud juhtudel sõltumatu poliitika, mis on täidetud.

Eduka vastuse kasutades `response_mode=fragment` näeks:

```
GET https://aadb2cplayground.azurewebsites.net/#
id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...
&state=arbitrary_data_you_can_receive_in_the_response
```

| Parameetri | Kirjeldus |
| ----------------------- | ------------------------------- |
| id_token | Id_token, mis rakenduse nõutav. Saate selle id_token kasutaja identiteedi ja kasutajale seansi alustada. Lisateavet id_tokens ja nende sisu on kaasatud [Azure AD B2C Turbeloa viide](active-directory-b2c-reference-tokens.md). |
| kood | Soovitud rakendus, kui kasutasite authorization_code `response_type=code+id_token`. Rakenduse saate kasutada luba kood on access_token target ressursi taotleda. Authorization_codes on väga lühiajalise. Tavaliselt need aeguvad umbes 10 minuti pärast. |
| olek | Kui parameeter olek on kaasatud kutse, kuvatakse sama väärtuse vastus. Rakendus peaks kinnitamine olekus väärtused koosolekukutsete ja kutsele vastamise identsed. |

Tõrge vastuseid saab saata ka selle `redirect_uri` nii, et rakendus võib käsitleda neid õigesti:

```
GET https://aadb2cplayground.azurewebsites.net/#
error=access_denied
&error_description=the+user+canceled+the+authentication
&state=arbitrary_data_you_can_receive_in_the_response
```

| Parameetri | Kirjeldus |
| ----------------------- | ------------------------------- |
| tõrge | Kuvatakse tõrge koodi string, mis saab liigitada tüüpi ilmnevad tõrked ja seda saab kasutada reageerida tõrgete. |
| error_description | Teatud tõrketeade, mis aitab tuvastada autentimise tõrke põhjuseks arendaja. |
| olek | Eelmisse tabelisse täieliku kirjelduse leiate teemast. Kui parameeter olek on kaasatud kutse, kuvatakse sama väärtuse vastus. Rakendus peaks kinnitamine olekus väärtused koosolekukutsete ja kutsele vastamise identsed. |


## <a name="validate-the-idtoken"></a>Kinnitage soovitud id_token
Ainult mõne id_token vastuvõtmine ei piisa autentida--peate selle id_token allkirja valideerimiseks ja veenduma luba ühe oma rakenduse nõuded nõuded. Azure'i AD B2C kasutab [JSON Web sõned (JWTs)](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) ja avaliku võtme krüptograafia sõned sisse ja veenduge, et need on lubatud.

On palju avatud lähtekoodi teekide puhul, kus kontrollimine JWTs, sõltuvalt teie eelistus keele jaoks on saadaval. Soovitame uurimine nende suvanditega, mitte oma valideerimise loogika rakendamine. Teave siin on kasu aru saada, kuidas kasutada õigesti neisse teekidesse.

Azure'i AD B2C on OpenID Connect metaandmete lõpp, mis võimaldab rakenduse toomiseks teavet Azure AD B2C käitusajal. See teave sisaldab lõpp-punktid, Turbeloa sisu ja allkirjastamine klahvid luba. On JSON metaandmete dokumendi iga poliitika oma B2C rentniku jaoks. Näiteks metaandmete dokumendi jaoks soovitud `b2c_1_sign_in` poliitika on `fabrikamb2c.onmicrosoft.com` asub aadressil:

`https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/v2.0/.well-known/openid-configuration?p=b2c_1_sign_in`

Selle konfiguratsiooni dokumendi atribuudid on selle `jwks_uri`, mille väärtus oleks sama poliitika:

`https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/discovery/v2.0/keys?p=b2c_1_sign_in`.

Selleks, et kindlaks teha, millised poliitika oli kasutusel on id_token sisselogimise (ja kus toomiseks metaandmete kaudu), on teil kaks võimalust. Esmalt poliitika nimi sisaldab selle `acr` on id_token nõue. Sõeluda on id_token olevate nõuete kohta leiate teavet teemast [Azure AD B2C Turbeloa viide](active-directory-b2c-reference-tokens.md). Teie teine võimalus on kodeerida väärtuse poliitika on `state` parameeter kui saata, ja seejärel dekodeerida, et määrata, milliseid kasutati. Kas meetod on täiesti lubatud.

Pärast omandatud OpenID Connect metaandmete lõpp metaandmete dokument, saate RSA 256 avaliku võtmed (mis asub selle lõpp-punkti) soovitud id_token allkirja kinnitamiseks. Võib olla mitu klahvid loendis selle näitaja, mis tahes ajal, iga tähistatud on `kid`. Funktsiooni id_token päis sisaldab ka mõne `kid` taotlemine, mis näitab, et nende klahvide kasutatud logida selle id_token. Teemast [Azure AD B2C Turbeloa viide](active-directory-b2c-reference-tokens.md) Lisateavet [kinnitamise sõned](active-directory-b2c-reference-tokens.md#validating-tokens) ja [olulise teabe edastamiseks võtme sisselogimise kohta](active-directory-b2c-reference-tokens.md#validating-tokens).
<!--TODO: Improve the information on this-->

Kui olete kinnitatud selle id_token allkirja, on mitmeid väidab, et peate kontrollida, näiteks:

- Peaks kinnitamist kuvatakse `nonce` taotlemine vältimiseks loa kordus eest. Selle väärtus peab olema määratud taotluse Logi sisse.
- Peaks kinnitamist kuvatakse `aud` tagamaks, et selle id_token on välja antud oma rakenduse taotlemine. Selle väärtuse peaks olema teie rakendus rakenduste ID-d.
- Peaks kinnitamist kuvatakse `iat` ja `exp` väidab, et tagada, et selle id_token on aegunud, ei.

Samuti on mitu rohkem valideerimine, mis tuleks teha, [OpenID ühenduse Core Spec](http://openid.net/specs/openid-connect-core-1_0.html)üksikasjalikult kirjeldatud.  Samuti võite täiendavate taotluste, olenevalt sellest, milline olukord teie domeenist. Mõned levinud valideerimised on järgmised.

- Tagada, et kasutaja/ettevõte on registreerunud rakenduse.
- Tagada, et kasutajal on õige autoriseerimine ja õigused.
- Tagada, et teatud tugevuse autentimine on ilmnenud, nt Azure Mitmikautentimise.

Nõuded on id_token kohta leiate lisateavet teemast [Azure AD B2C Turbeloa viide](active-directory-b2c-reference-tokens.md).

Kui teil on id_token täielikult kinnitanud, saate alustada kasutaja seansi ja nõuete kasutamine on id_token rakenduse kasutaja kohta teabe saamiseks. Seda teavet saab kasutada Kuva, kirjeid, autoriseerimine ja jne.

## <a name="get-a-token"></a>Märgiks hankimine
Kui kõik, mis on teie web appi peab tegema käivitada poliitikad, võite mõne jaotistest vahele jätta. Järgmiste jaotiste kehtivad ainult web rakendused, mida on vaja teha autenditud veebi-API kõned ja Azure AD B2C on ka kaitstud.

Saab lunastada authorization_code, mille ostsite (abil `response_type=code+id_token`) soovitud ressursi saatmisega märgiks on `POST` taotlus on `/token` lõpp-punkti. Praegu ainult ressurss, mille saate taotleda märgiks jaoks on teie enda taustväärtus veebi-API. Mess taotlemise märgiks endale on kasutada oma rakenduse kliendi ID ulatuse.

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=authorization_code&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob&client_secret=<your-application-secret>

```

| Parameetri | See on nõutav? | Kirjeldus |
| ----------------------- | ------------------------------- | --------------------- |
| p | Nõutav | Poliitika, mida kasutati omandada luba kood. Te ei saa kasutada muu poliitika taotlus. Pange tähele, et see parameeter lisamine **päringustringi**, mitte POST keha. |
| client_id | Nõutav | Rakenduse ID [Azure portaali](https://portal.azure.com/) rakenduse määratud. |
| grant_type | Nõutav | Anda, mis peab olema tüüpi `authorization_code` autoriseerimine kood meilivoo jaoks. |
| ulatus | Soovitatav | Tühikuga eraldatud loend otsinguulatuste. Ühe ulatus väärtus näitab nii Azure AD õiguste, mis on nõutud. Funktsiooni `openid` ulatus näitab õigust kasutaja sisselogimine ja ülevaate **id_tokens**kujul kasutaja andmeid. Selle saab teie enda taustväärtus veebi-API, mis on esindatud sama rakenduse ID klient märkide saada. Funktsiooni `offline_access` ulatus näitab rakenduse peate mõne **refresh_token** vanade juurdepääsuks ressursse. |
| kood | Nõutav | Authorization_code, mille ostsite esimesel etapil kulgemist. |
| redirect_uri | Nõutav | Kui olete saanud selle authorization_code rakenduse redirect_uri. |
| client_secret | Nõutav | Teie loodud [Azure portaali](https://portal.azure.com/)rakenduse salajane. Selle rakenduse salajase on oluline turvalisus. Hoidke seda turvaliselt serverisse. Mida peaks ka Olge ettevaatlik, et pöörata selle kliendi salajase perioodiliselt. |

Eduka loa vastuse näeb:

```
{
    "not_before": "1442340812",
    "token_type": "Bearer",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "scope": "90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access",
    "expires_in": "3600",
    "refresh_token": "AAQfQmvuDy8WtUv-sd0TBwWVQs1rC-Lfxa_NDkLqpg50Cxp5Dxj0VPF1mx2Z...",
}
```
| Parameetri | Kirjeldus |
| ----------------------- | ------------------------------- |
| not_before | Aeg, mille luba peetakse kehtiv epohhi ajal. |
| token_type | Turbeloa tüüp väärtus. Ainult tüüp, mis toetab Azure AD on esitaja. |
| access_token | Allkirjastatud JWT märku, et teie soovitud. |
| ulatus | Funktsiooni otsinguulatuste luba on lubatud, mida saab kasutada sõned hilisemaks kasutamiseks vahemällu. |
| expires_in | Pikkus, aeg, mis on access_token kehtib (sekundites). |
| refresh_token | Mõne OAuth 2.0 refresh_token. Rakenduse abil saate selle luba hankida täiendavaid sõned pärast praeguse luba on aegunud.  Refresh_tokens on kaua elanud ja seda saab kasutada säilitamise ressurssidele laiendatud aja jooksul. Lugege täpsemalt [B2C Turbeloa viide](active-directory-b2c-reference-tokens.md). Teate, et teil tuleb kasutada ulatuse `offline_access` autoriseerimine ja selleks, et saada vastuseks refresh_token Turbeloa taotlused. |

Tõrge vastuste näeb:

```
{
    "error": "access_denied",
    "error_description": "The user revoked access to the app.",
}
```

| Parameetri | Kirjeldus |
| ----------------------- | ------------------------------- |
| tõrge | Kuvatakse tõrge koodi string, mis saab liigitada tüüpi ilmnevad tõrked ja seda saab kasutada reageerida tõrgete. |
| error_description | Teatud tõrketeade, mis aitab tuvastada autentimise tõrke põhjuseks arendaja. |

## <a name="use-the-token"></a>Luba kasutamine
Nüüd, kui olete edukalt ostsite mõne `access_token`, saate kasutada funktsiooni luba kutsed oma kirjutamata web API-d, sh see on `Authorization` päis:

```
GET /tasks
Host: https://mytaskwebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

## <a name="refresh-the-token"></a>Luba värskendamine
Funktsiooni id_tokens on lühiajalise. Peate värskendama need pärast need aeguvad jätkamiseks pääse ressursid. Saate seda teha, esitades teise `POST` taotlus on `/token` lõpp-punkti. Sel ajal, sisestage soovitud `refresh_token` asemel on `code`:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=refresh_token&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=openid offline_access&refresh_token=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob&client_secret=<your-application-secret>
```

| Parameetri | Nõutav | Kirjeldus |
| ----------------------- | ------------------------------- | -------- |
| p | Nõutav | Poliitika, mida kasutati algse refresh_token omandada. Te ei saa kasutada muu poliitika taotlus. Pange tähele, et see parameeter lisamine **päringustringi**, mitte POST keha. |
| client_id | Nõutav | Rakenduse ID [Azure portaali](https://portal.azure.com/) rakenduse määratud. |
| grant_type | Nõutav | Anda, mis peab olema tüüpi `refresh_token` selle etapil autoriseerimine kood kulgemist. |
| ulatus | Soovitatav | Tühikuga eraldatud loend otsinguulatuste. Ühe ulatus väärtus näitab nii Azure AD õiguste, mis on nõutud. Funktsiooni `openid` ulatus näitab õigust kasutaja sisselogimine ja ülevaate **id_tokens**kujul kasutaja andmeid. Selle saab teie enda taustväärtus veebi-API, mis on esindatud sama rakenduse ID klient märkide saada. Funktsiooni `offline_access` ulatus näitab rakenduse peate mõne **refresh_token** vanade juurdepääsuks ressursse. |
| redirect_uri | Soovitatav | Kui olete saanud selle authorization_code rakenduse redirect_uri. |
| refresh_token | Nõutav | Algne refresh_token ostsite teise jalga kulgemist. Teate, et teil tuleb kasutada ulatuse `offline_access` autoriseerimine ja selleks, et saada vastuseks refresh_token Turbeloa taotlused. |
| client_secret | Nõutav | Teie loodud [Azure portaali](https://portal.azure.com/)rakenduse salajane. Selle rakenduse salajase on oluline turvalisus. Hoidke seda turvaliselt serverisse. Mida peaks ka Olge ettevaatlik, et pöörata selle kliendi salajase perioodiliselt. |

Eduka loa vastuse näeb:

```
{
    "not_before": "1442340812",
    "token_type": "Bearer",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "scope": "90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access",
    "expires_in": "3600",
    "refresh_token": "AAQfQmvuDy8WtUv-sd0TBwWVQs1rC-Lfxa_NDkLqpg50Cxp5Dxj0VPF1mx2Z...",
}
```
| Parameetri | Kirjeldus |
| ----------------------- | ------------------------------- |
| not_before | Aeg, mille luba peetakse kehtiv epohhi ajal. |
| token_type | Turbeloa tüüp väärtus. Ainult tüüp, mis toetab Azure AD on esitaja. |
| access_token | Allkirjastatud JWT märku, et teie soovitud. |
| ulatus | Funktsiooni otsinguulatuste luba on lubatud, mida saab kasutada sõned hilisemaks kasutamiseks vahemällu. |
| expires_in | Pikkus, aeg, mis on access_token kehtib (sekundites). |
| refresh_token | Mõne OAuth 2.0 refresh_token. Rakenduse abil saate selle luba hankida täiendavaid sõned pärast praeguse luba on aegunud.  Refresh_tokens on kaua elanud, ja seda saab kasutada säilitamise ressurssidele laiendatud aja jooksul. Lugege täpsemalt [B2C Turbeloa viide](active-directory-b2c-reference-tokens.md). |

Tõrge vastuste näeb:

```
{
    "error": "access_denied",
    "error_description": "The user revoked access to the app.",
}
```

| Parameetri | Kirjeldus |
| ----------------------- | ------------------------------- |
| tõrge | Kuvatakse tõrge koodi string, mis saab liigitada tüüpi ilmnevad tõrked ja seda saab kasutada reageerida tõrgete. |
| error_description | Teatud tõrketeade, mis aitab tuvastada autentimise tõrke põhjuseks arendaja. |


## <a name="send-a-sign-out-request"></a>Väljunud kutse saatmine

Kui soovite kasutaja rakenduse välja logida, ei ole piisavalt selge oma rakenduse küpsised või muul viisil Lõpeta seanss ja kasutajale. Kasutaja peab ümber suunata ka Azure AD Logi välja. Kui te ei tee, võib olla kasutaja reauthenticate rakenduse ilma volituste uuesti sisestada. Seda sellepärast, et neil on lubatud ühekordse sisselogimise seansi Azure AD.

Te lihtsalt ümber suunata kasutajal on `end_session_endpoint` mis on loetletud samas dokumendis OpenID Connect metaandmete kirjeldatud varasemates "Kontrolli selle id_token":

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/logout?
p=b2c_1_sign_in
&post_logout_redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
```

| Parameetri | See on nõutav? | Kirjeldus |
| ----------------------- | ------------------------------- | ------------ |
| p | Nõutav | Poliitika, mida soovite kasutada kasutaja rakenduse välja logima. |
| post_logout_redirect_uri | Soovitatav | URL, mida kasutaja peaks ümber pärast eduka väljunud. Kui see pole kaasatud, kasutaja kuvatakse üldise sõnumi Azure AD B2C järgi.  |

> [AZURE.NOTE]
    Samal ajal suunavad kasutajal on `end_session_endpoint` mõne kasutaja ühekordse sisselogimise riigi Azure AD B2C selge, seda ei kirjuta kasutaja välja kasutaja omase pakkuja (IDP) seanss. Kui kasutaja valib sama IDP ajal järgmise sisselogimine, nad saavad olema reauthenticated, sisestamata volituste. Kui kasutaja soovib B2C rakenduse välja logida, seda ei tähenda soovivad oma Facebooki kontot täielikult väljalogimiseks. Siiski puhul kohalikud kontod, kasutaja seanss lõpetatakse õigesti.

## <a name="use-your-own-b2c-tenant"></a>Kasutage oma B2C rentniku

Kui soovite proovida taotlused ise, peate esmalt teha kolme järgmist ja asendage näidisväärtused kohal oma:

- [B2C rentniku loomine](active-directory-b2c-get-started.md)ja kasutage oma rentniku rakenduses taotlused nime.
- [Loo rakenduse](active-directory-b2c-app-registration.md) hankimine rakenduste ID-d ja on redirect_uri. Soovite arvutamiseks viitesse lisada mõne **web app/web api** rakenduse ja soovi korral ka **rakenduse salajane**loomine.
- [Luua oma poliitikate](active-directory-b2c-reference-policies.md) saada teie poliitika nimed.
