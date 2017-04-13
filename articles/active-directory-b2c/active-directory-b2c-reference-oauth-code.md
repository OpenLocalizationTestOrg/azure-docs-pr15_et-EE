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

# <a name="azure-active-directory-b2c-oauth-20-authorization-code-flow"></a>Azure Active Directory B2C: OAuth 2.0 autoriseerimine kood meilivoo

OAuth 2.0 autoriseerimine koodi anda saab kasutada rakendusi, mis on seadmesse installitud hiljem juurde pääseda kaitstud ressursse, näiteks web API-d. Azure Active Directory (Azure AD) B2C OAuth 2.0 rakendamise abil saate lisada oma mobiil- ja lauaarvutite rakenduste registreerumise, Logi sisse ja muud identiteedi haldamise toiminguid. Sellest juhendist leiavad sõltumatu keel. See kirjeldab sõnumite saatmiseks ja vastuvõtmiseks HTTP ühte meie avatud lähtekoodi teekide kasutamata.

<!-- TODO: Need link to libraries -->

OAuth 2.0 autoriseerimine kood voogu on kirjeldatud jaotises [4.1 OAuth 2.0 kirjelduse](http://tools.ietf.org/html/rfc6749). Saate seda teha rakenduse tüüpi [veebirakenduste](active-directory-b2c-apps.md#web-apps) ja [algupäraselt installitud rakendusi](active-directory-b2c-apps.md#mobile-and-native-apps)enamikus autentimise ja luba. See võimaldab turvaliselt omandada **access_tokens**, mida saab kasutada ressursse, mis on tagatud ettevõtte [autoriseerimine serveri](active-directory-b2c-reference-protocols.md#the-basics)juurdepääsu rakendused.

Sellest juhendist keskendub kindla maitse OAuth 2.0 autoriseerimine kood meilivoo--**avaliku kliendid**. Avaliku klient on mis tahes klientrakendusega, mida ei saa määrata usaldusväärseks turvaliselt säilitada kontrolli usaldusväärsus salajane parool. See hõlmab mobiilirakendused, töölauarakenduste ja päris palju mis tahes rakendus, mis töötab seade ja vajab access_tokens. Kui soovite lisada identiteetide haldamisviis web Appi abil Azure AD B2C, kasutage [OpenID ühenduse loomine](active-directory-b2c-reference-oidc.md) , mitte OAuth 2.0.

Azure'i AD B2C laiendab standardit OAuth 2.0 liigub sujuvalt teha rohkem kui lihtne autentimise ja luba. See toob [**poliitika parameeter**](active-directory-b2c-reference-policies.md), mis võimaldab teil OAuth 2.0 kasutuskogemuse lisamiseks kasutage rakenduse, näiteks registreerumise, Logi sisse ja profiili haldus. Siin näitame teile OAuth 2.0 ja poliitikaid rakendada iga neid kogemusi kohalikke rakenduste kasutamine. Näitame hankimine access_tokens juurdepääsuks web API-d.

Alltoodud näites HTTP päringuid kasutatakse meie valimi B2C kataloogi, **fabrikamb2c.onmicrosoft.com**, kui ka meie proovi taotluse ja poliitika. Olete tasuta proovida, kasutades neid väärtusi taotlused ise või saate asendada need oma.
Siit saate teada, kuidas [oma B2C kataloogi, rakenduse ja poliitikate](#use-your-own-b2c-directory).

## <a name="1-get-an-authorization-code"></a>1. mõne kood hankimine
Luba kood voogu algab suunavad kasutajal kliendi soovitud `/authorize` lõpp-punkti. See on interaktiivne osa voogu, kui kasutaja on tegelikult tegutsema. Selle taotluse näitab kliendi õigused, mida on vaja hankida kasutaja soovitud `scope` parameeter ja käivitada poliitika on `p` parameeter. Kolm näidet on toodud allpool (reapiirid loetavuse), kus iga luua muu poliitika.

#### <a name="use-a-sign-in-policy"></a>Kasutage sisselogimiseks poliitika

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code
&redirect_uri=urn%3Aietf%3Awg%3Aoauth%3A2.0%3Aoob
&response_mode=query
&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&p=b2c_1_sign_in
```

#### <a name="use-a-sign-up-policy"></a>Kasutage registreerumise poliitika

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code
&redirect_uri=urn%3Aietf%3Awg%3Aoauth%3A2.0%3Aoob
&response_mode=query
&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&p=b2c_1_sign_up
```

#### <a name="use-an-edit-profile-policy"></a>Kasutage Redigeeri profiili poliitika

```
GET https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code
&redirect_uri=urn%3Aietf%3Awg%3Aoauth%3A2.0%3Aoob
&response_mode=query
&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&p=b2c_1_edit_profile
```

| Parameetri | See on nõutav? | Kirjeldus |
| ----------------------- | ------------------------------- | ----------------------- |
| client_id | Nõutav | Rakenduse ID [Azure portaali](https://portal.azure.com) rakenduse määratud. |
| response_type | Nõutav | Vastuse tüüp, mis sisaldavad `code` autoriseerimine kood meilivoo jaoks. |
| redirect_uri | Nõutav | Rakenduse, kus autentimine vastuseid saate saadetud ja vastuvõetud rakenduse redirect_uri. See peab täpselt vastama ühte portaalis registreeritud redirect_uris peab URL-ina kodeeritav puhul. |
| ulatus | Nõutav | Tühikuga eraldatud loend otsinguulatuste. Ühe ulatus väärtus näitab nii Azure AD õiguste, mis on nõutud. Ulatuse näitab, et teie rakendus peab on **Luba juurdepääs** saab esitada oma teenuse või veebi-API kasutamine kliendi ID, tähistab sama kliendi ID.  Funktsiooni `offline_access` ulatus näitab rakenduse peate mõne **refresh_token** vanade juurdepääsuks ressursse.  Saate kasutada ka funktsiooni `openid` ulatus taotleda mõne **id_token** Azure AD B2C. |
| response_mode | Soovitatav | Mida tuleks kasutada saatma tulemuseks authorization_code rakenduse meetodit. See võib olla üks "päring", "form_post" või "fragment".
| olek | Soovitatav | Väärtus, mis sisaldab taotluse, mis tagastatakse loa vastuse. See võib olla mis tahes sisu, mida soovite string. Juhuslik kordumatu väärtus on tavaliselt kasutatakse vältimine saitidevaheline taotluse võltsimist eest. Riigi kasutatakse ka kodeerida teavet rakenduse kasutaja olekus autentimise taotluse ilmnemisele nagu nad lehe või poliitika on täidetud. |
| p | Nõutav | Poliitika, mis käivitatakse. See on poliitika, mis on loodud B2C kataloogi nimi. Poliitika nimi väärtus peaks algama "b2c\_1\_". Lugege lisateavet poliitikate [laiendatav raamistik](active-directory-b2c-reference-policies.md). |
| küsimus | Valikuline. | Kasutaja suhtlus, mida on vaja tüüp. Praegu ainult kehtiv väärtus on 'sisselogimine', mis jõustab kasutajal sisestada oma volitusi taotluse alusel. Ühekordse sisselogimise ei jõustu. |

Selles etapis küsitakse kasutaja poliitika töövoo lõpuleviimiseks. See võib hõlmata kasutajal sisestada oma kasutajanimi ja parool, sisselogimine omase, kataloogi või mis tahes muust numbrist juhiseid, olenevalt sellest, kuidas poliitika on määratletud kasutajaks.

Pärast kasutaja lõpulejõudmist poliitika Azure AD tulemuseks rakenduse juures märgitud vastuseks `redirect_uri`, määratud meetodi abil soovitud `response_mode` parameeter. Vastuse saab täpselt sama, iga ülaltoodud juhtudel sõltumatu poliitika, mis on täidetud.

Eduka vastust, mis kasutab `response_mode=query` näeb välja nagu:

```
GET urn:ietf:wg:oauth:2.0:oob?
code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...        // the authorization_code, truncated
&state=arbitrary_data_you_can_receive_in_the_response                // the value provided in the request
```

| Parameetri | Kirjeldus |
| ----------------------- | ------------------------------- |
| kood | Authorization_code, mis rakenduse nõutav. Rakenduse saate kasutada luba kood on access_token target ressursi taotleda. Authorization_codes on väga lühiajalise. Tavaliselt need aeguvad umbes 10 minuti pärast. |
| olek | Eelmisse tabelisse täieliku kirjelduse leiate teemast. Kui parameeter olek on kaasatud kutse, kuvatakse sama väärtuse vastus. Rakendus peaks veenduge, et olekuväärtusi taotlus ja vastus on identse. |

Tõrge vastuste võib ka saata selle `redirect_uri` nii, et rakendus saate neid õigesti lahendab:

```
GET urn:ietf:wg:oauth:2.0:oob?
error=access_denied
&error_description=The+user+has+cancelled+entering+self-asserted+information
&state=arbitrary_data_you_can_receive_in_the_response
```

| Parameetri | Kirjeldus |
| ----------------------- | ------------------------------- |
| tõrge | Kuvatakse tõrge kood string, mis saab liigitada tüüpi ilmnevad tõrked ja seda saab kasutada reageerida tõrgete. |
| error_description | Teatud tõrketeade, mis aitab tuvastada autentimise tõrke põhjuseks arendaja. |
| olek | Selles jaotises esimese tabeli täieliku kirjelduse leiate teemast. Kui parameeter olek on kaasatud kutse, kuvatakse sama väärtuse vastus. Rakendus peaks veenduge, et olekuväärtusi taotlus ja vastus on identse. |


## <a name="2-get-a-token"></a>2 märgiks hankimine
Nüüd, kui mõni authorization_code omandatud, saab lunastada soovitud `code` märgiks soovitud ressursile, saates neile on `POST` taotlus on `/token` lõpp-punkti. Azure'i AD B2C, on teie enda taustväärtus veebi-API ainult ressurss, mille saate taotleda märgiks jaoks. Et kasutatakse taotlemise märgiks endale on kasutada oma rakenduse kliendi ID ulatuse.

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=authorization_code&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob

```

| Parameetri | See on nõutav? | Kirjeldus |
| ----------------------- | ------------------------------- | --------------------- |
| p | Nõutav | Poliitika, mida kasutati omandada luba kood. Te ei saa kasutada muu poliitika taotlus. Märkus. see parameeter lisamine *päringustringi*, mitte POST keha. |
| client_id | Nõutav | Rakenduse ID [Azure portaali](https://portal.azure.com) rakenduse määratud. |
| grant_type | Nõutav | Anda, mis peab olema tüüpi `authorization_code` autoriseerimine kood meilivoo jaoks. |
| ulatus | Võib seadet | Tühikuga eraldatud loend otsinguulatuste. Ühe ulatus väärtus näitab nii Azure AD õiguste, mis on nõutud. Ulatuse näitab, et teie rakendus peab on **Luba juurdepääs** saab esitada oma teenuse või veebi-API kasutamine kliendi ID, tähistab sama kliendi ID.  Funktsiooni `offline_access` ulatus näitab rakenduse peate mõne **refresh_token** vanade juurdepääsuks ressursse.  Saate kasutada ka funktsiooni `openid` ulatus taotleda mõne **id_token** Azure AD B2C. |
| kood | Nõutav | Authorization_code, mille ostsite esimesel etapil kulgemist. |
| redirect_uri | Nõutav | Kui olete saanud selle authorization_code rakenduse redirect_uri. |

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
| access_token | Allkirjastatud JSON Web Turbeloa (JWT) märku, et teie soovitud. |
| ulatus | Funktsiooni otsinguulatuste luba on lubatud, mida saab kasutada sõned hilisemaks kasutamiseks vahemällu. |
| expires_in | Pikkus, luba on kehtiv (sekundites). |
| refresh_token | Mõne OAuth 2.0 refresh_token. Rakenduse abil saate selle luba hankida täiendavaid sõned pärast praeguse luba on aegunud. Refresh_tokens on kaua elanud ja seda saab kasutada säilitamise ressurssidele laiendatud aja jooksul. Lugege täpsemalt [B2C Turbeloa viide](active-directory-b2c-reference-tokens.md). |

Tõrge vastuste näeb:

```
{
    "error": "access_denied",
    "error_description": "The user revoked access to the app.",
}
```

| Parameetri | Kirjeldus |
| ----------------------- | ------------------------------- |
| tõrge | Kuvatakse tõrge kood string, mis saab liigitada tüüpi ilmnevad tõrked ja seda saab kasutada tõrgete reageerima. |
| error_description | Teatud tõrketeade, mis aitab tuvastada autentimise tõrke põhjuseks arendaja. |

## <a name="3-use-the-token"></a>3. Kasutage luba
Nüüd, kui olete edukalt ostsite mõne `access_token`, saate kasutada funktsiooni luba kutsed oma kirjutamata web API-d, sh see on `Authorization` päis:

```
GET /tasks
Host: https://mytaskwebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

## <a name="4-refresh-the-token"></a>4. luba värskendamine
Access & Id sõned on lühiajalise. Peate värskendama need pärast need aeguvad jätkata pääse ressursid. Saate seda teha, esitades teise `POST` taotlus on `/token` lõpp-punkti. Seekord Sisestage soovitud `refresh_token` asemel on `code`:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

grant_type=refresh_token&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access&refresh_token=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob
```

| Parameetri | See on nõutav? | Kirjeldus |
| ----------------------- | ------------------------------- | -------- |
| p | Nõutav | Poliitika, mida kasutati algse refresh_token omandada. Te ei saa kasutada muu poliitika taotlus. Märkus. see parameeter lisamine *päringustringi*, mitte POST keha. |
| client_id | Soovitatav | Rakenduse ID [Azure portaali](https://portal.azure.com) rakenduse määratud. |
| grant_type | Nõutav | Anda, mis peab olema tüüpi `refresh_token` selle etapil autoriseerimine kood kulgemist. |
| ulatus | Soovitatav | Tühikuga eraldatud loend otsinguulatuste. Ühe ulatus väärtus näitab nii Azure AD õiguste, mis on nõutud. Ulatuse näitab, et teie rakendus peab on **Luba juurdepääs** saab esitada oma teenuse või veebi-API kasutamine kliendi ID, tähistab sama kliendi ID.  Funktsiooni `offline_access` ulatus näitab rakenduse peate mõne **refresh_token** vanade juurdepääsuks ressursse.  Saate kasutada ka funktsiooni `openid` ulatus taotleda mõne **id_token** Azure AD B2C. |
| redirect_uri | Valikuline. | Kui olete saanud selle authorization_code rakenduse redirect_uri. |
| refresh_token | Nõutav | Algne refresh_token ostsite teise jalga kulgemist. |

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
| access_token | Allkirjastatud JSON Web Turbeloa (JWT) märku, et teie soovitud. |
| ulatus | Funktsiooni otsinguulatuste luba on lubatud, mida saab kasutada sõned hilisemaks kasutamiseks vahemällu. |
| expires_in | Pikkus, luba on kehtiv (sekundites). |
| refresh_token | Mõne OAuth 2.0 refresh_token. Rakenduse abil saate selle luba hankida täiendavaid sõned pärast praeguse luba on aegunud. Refresh_tokens on kaua elanud ja seda saab kasutada säilitamise juurdepääsu ressursse laiendatus aja jooksul. Lugege täpsemalt [B2C Turbeloa viide](active-directory-b2c-reference-tokens.md). |

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

## <a name="use-your-own-b2c-directory"></a>Kasutage oma B2C kataloogi

Kui soovite proovida taotlused ise, peate esmalt teha kolme järgmist ja asendage näidisväärtused kohal oma:

- [Loo B2C directory](active-directory-b2c-get-started.md) ja kasutage oma taotlusi kataloogi nime.
- [Loo rakenduse](active-directory-b2c-app-registration.md) hankimine rakenduste ID-d ja on redirect_uri. Kas soovite kaasata **omakliendi** rakenduse.
- [Luua oma poliitikate](active-directory-b2c-reference-policies.md) saada teie poliitika nimed.
