
<properties
    pageTitle="Azure'i AD v2.0 OAuthi kliendi identimisteabe Flow | Microsoft Azure'i"
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
    ms.date="09/26/2016"
    ms.author="dastrock"/>

# <a name="v20-protocols---oauth-20-client-credentials-flow"></a>v2.0 Protokollid - OAuthi 2.0 kliendi identimisteabe Flow

Funktsiooni [OAuth 2.0 kliendi identimisteabe anda](http://tools.ietf.org/html/rfc6749#section-4.4), nimetatakse ka "kahe jalaga OAuthi", saab juurdepääsu veebis majutatud ressursse rakenduse ID abil.  See on tavaliselt kasutatakse server server kasutusviisid, mida ilma lõppkasutaja kohe precense taustal käitada.  Seda tüüpi rakenduste sageli nimetatakse **deemonid** või **teenuse kontod**.

> [AZURE.NOTE]
    Kõik Azure Active Directory stsenaariumid ja funktsioonid on toetatud v2.0 lõpp-punkt.  Kui kasutate v2.0 lõpp-punkti määramiseks lugege [v2.0 piirangud](active-directory-v2-limitations.md).

Soovitud omase kolme "kolme jalaga OAuthi," kliendi rakendus on antud juurdepääs ressursi teatud kasutaja nimel.  **Delegeeritud** kasutaja rakendusse, tavaliselt käigus [nõusolek](active-directory-v2-scopes.md) on luba.  Siiski kliendi identimisteabe voogu, õigused on antud **otse** rakendusse ise.  Kui rakendus esitab märgiks ressursi, jõustab ressurss, et rakendus ise on autoriseerimine teeks teatud toimingu - ei, et mõni kasutaja on autoriseerimine.

## <a name="protocol-diagram"></a>Protocol (protokoll) skeem
Kogu kliendi identimisteabe voogu näeb välja umbes selline - iga juhiseid kirjeldatakse allpool.

![Kliendi identimisteabe meilivoo](../media/active-directory-v2-flows/convergence_scenarios_client_creds.png)

## <a name="get-direct-authorization"></a>Otsest luba hankimine 
On kaks võimalust, et rakendus saab tavaliselt otsest juurdepääsu ressursi autoriseerimine: allikast pääsuloendi või teenuserakenduse õiguste määramine Azure AD abil.  On mitu muud võimalust ressursi valida oma klientidele lubada ja iga ressursi serveri valida meetod, mis on kõige mõistlikum selle rakenduse.  Need kaks meetodit on kõige levinum Azure AD ja kliendid ja ressursid, millest soovite teha kliendi identimisteabe voogu võib seadet.

### <a name="access-control-lists"></a>Pääsuloendid
Antud ressursside pakkuja võib Jõusta autoriseerimine sisse, mis põhineb rakenduste ID-sid, et see teab ja annab mõne kindla pääsutaseme loendi.  Kui ressursi saab märgiks v2.0 lõpp-punkti, saate luba dekodeerida ja ekstraktida soovitud kliendi rakenduse ID kaudu soovitud `appid` ja `iss` taotluste.  Seejärel saate võrrelda, et taotluse vastu mõned pääsuloendi (ACL) ta jääb.  Granulaarsus ja pääsuloendi meetodit võib olla muutuv oluliselt ressursi ressursi.

Levinud kasutamine juhul sellise ACL-ID on testida jooksjad veebirakenduse või web API-ga.  Veebirakenduse api anda ainult selle täielikud õigused alamhulga erinevate klientidele.  Kuid käivitamiseks lõpuni kontrollib api, luuakse testi klient, mis saab sõned v2.0 lõpp ja saadab API.  Api saab siis ACL testi kliendi rakenduse ID jaoks täielik juurdepääs kogu selle api funktsioone.  Pange tähele, et kui teil on sellise loendi teenust, mida peaks olema veenduge, et mitte ainult valideerimiseks helistaja `appid`, aga ka kinnitada, et selle `iss` luba on usaldusväärne ka.

Seda tüüpi autoriseerimine on tavaline deemonid ja millel peavad juurde pääsema andmetele tarbija kasutajate isikliku Microsofti kontoga.  Andmete kuuluvad asutuste, soovitatakse teil omandada vajalikud loa perimssions rakenduse kaudu.

### <a name="application-permissions"></a>Teenuserakenduse õiguste
ACL-ID asemel saate API-d saab anda rakenduse **teenuserakenduse õiguste** nähtavaks tegemine.  Mõne rakendusel on antud rakenduse ettevõtte administraator ja saab kasutada ainult juurdepääsu andmetele selle organisatsiooni ja oma töötajatele.  Näiteks Microsoft Graphi seab mitme rakenduse õigusi:

- Kõigi postkastide meili lugemine
- Lugemine ja kirjutamine kõigi postkastide rakenduses Meil
- Kui mõni kasutaja meili saatmine
- Directory andmete lugemine
- [+ rohkem](https://graph.microsoft.io)

Selleks, et hankida rakenduse need õigused, saate teha järgmist.

#### <a name="request-the-permissions-in-the-app-registration-portal"></a>Rakenduse registreerimise portaali õiguste taotlemine

- Liikuge oma rakenduse [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)või [rakenduse loomine](active-directory-v2-app-registration.md) , kui te pole seda veel teinud.  Peate veenduge, et teie taotlus on loonud vähemalt ühe rakenduse salajane.
- Otsige üles jaotis **Otsese teenuserakenduse õiguste** ja lisage oma rakenduse jaoks on vaja õigusi.
- Veenduge, et **salvestada** rakenduse registreerimine

#### <a name="recommended-sign-the-user-into-your-app"></a>Soovitatav: logige oma rakenduse kasutaja

Tavaliselt koostamise rakendus, mis kasutab rakenduse õigusi, peate rakenduse olema lehe/vaade, mis võimaldab kinnitamiseks rakenduse õiguste haldus.  Selle lehe võib olla osa rakenduse registreerumise liikumisele, rakenduse sätetes või sihtotstarbeline "ühendust" kulgemist.  Paljudel juhtudel, on mõistlik kuvada selle rakenduse "ühendust" vaade alles pärast seda, kui kasutaja on töö või kooli Microsofti kontoga sisse loginud.

Kasutaja sisselogimine rakendusse võimaldab teil tuvastada organziation, kuhu kasutaja kuulub enne paludes neil teenuserakenduse õiguste kinnitamine.  Ajal tingimata vajalik, see aitab teil luua oma ettevõtte kasutajate jaoks selgem kogemus.  Logige kasutaja, järgige meie [v2.0 protokolli õpetused](active-directory-v2-protocols.md).

#### <a name="request-the-permissions-from-a-directory-admin"></a>Taotleda õiguste directory administraator

Kui olete valmis õiguste taotlemiseks: ettevõtte administraator, saate kasutaja v2.0 **administraator nõusoleku lõpp-punkti**ümber suunata.

```
// Line breaks for legibility only

GET https://login.microsoftonline.com/{tenant}/adminconsent?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&state=12345
&redirect_uri=http://localhost/myapp/permissions
```

```
// Pro Tip: Try pasting the below request in a browser!
```

```
https://login.microsoftonline.com/common/adminconsent?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&state=12345&redirect_uri=http://localhost/myapp/permissions
```

| Parameetri | | Kirjeldus |
| ----------------------- | ------------------------------- | --------------- |
| rentniku | nõutav | Directory rentniku, mida soovite taotleda luba.  Saate esitada guid või sõbralik nimi vormingus.  Kui teil pole teada, millised rentniku kasutaja kuulub ja soovite õiguse mis tahes rentniku sisse logida, kasutage `common`. |
| client_id | nõutav | Rakenduse Id, et registreerimise portaali ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) määratud rakenduse. |
| redirect_uri | nõutav | Redirect_uri, kuhu soovite vastuse saata oma rakenduse käsitlema.  See peab täpselt vastama ühte redirect_uris, mis on registreeritud portaali, kuid seda peab olema kodeeritud url ja võib-olla täiendavad tee segmente. |
| olek | soovitatav | Väärtus, mis sisaldab taotluse, mis tagastatakse loa vastuse.  See võib olla mis tahes sisu, mida soovite string.  Riigi kasutatakse kodeerida teavet rakenduse kasutaja olekus enne autentimine taotluse ilmnes, näiteks lehe või vaate nad. |

Selles etapis on Azure AD jõusta, et ainult rentnikuadministraator saab sisse logida tippige koosolekukutse.  Administraator palutakse kinnitada kõigi oma rakenduse registreerimise portaalis soovitud vahetult õigusi. 

##### <a name="successful-response"></a>Eduka vastus
Kui soovitud administraator kinnitab rakenduse õigusi, on eduka vastus:

```
GET http://localhost/myapp/permissions?tenant=a8990e1f-ff32-408a-9f8e-78d3b9139b95&state=state=12345&admin_consent=True
```

| Parameetri | Kirjeldus |
| ----------------------- | ------------------------------- | --------------- |
| rentniku | Directory rentniku antud rakenduse õigused, mis on saanud, guid vormingus. |
| olek | Väärtus, mis sisaldab taotluse, mis tagastatakse loa vastuse.  See võib olla mis tahes sisu, mida soovite string.  Riigi kasutatakse kodeerida teavet rakenduse kasutaja olekus enne autentimine taotluse ilmnes, näiteks lehe või vaate nad. |
| admin_consent | Seatakse `True`. |


##### <a name="error-response"></a>Tõrge vastus
Kui admin ei kinnita rakenduse õigusi, on nurjunud vastus:

```
GET http://localhost/myapp/permissions?error=permission_denied&error_description=The+admin+canceled+the+request
```

| Parameetri | Kirjeldus |
| ----------------------- | ------------------------------- | --------------- |
| tõrge | Kuvatakse tõrge koodi string, mis saab liigitada tüüpi ilmnevad tõrked ja seda saab kasutada reageerida tõrgete. |
| error_description | Teatud tõrketeade, mis aitab tuvastada tõrke põhjuseks arendaja.  |

Kui olete saanud eduka vastuse ettevalmistamise lõpp-punkti rakendusest, rakenduse on saanud vahetult õigusi selle nõutav.  Nüüd saate teisaldada peale paluda märgiks soovitud ressursi.

## <a name="get-a-token"></a>Märgiks hankimine
Kui rakenduse omandatud vajalikud autoriseerimine, saate jätkata hankimisega Accessi sõned API-de jaoks.  Kliendi märgiks anda mandaadi saamiseks saata postituse taotluse selle `/token` v2.0 lõpp-punkt:

```
POST /common/oauth2/v2.0/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded

client_id=535fb089-9ff3-47b6-9bfb-4f1264799865&scope=https%3A%2F%2Fgraph.microsoft.com%2F.default&client_secret=qWgdYAmab0YSkuL1qKv5bPX&grant_type=client_credentials
```

```
curl -X POST -H "Content-Type: application/x-www-form-urlencoded" -d 'client_id=535fb089-9ff3-47b6-9bfb-4f1264799865&scope=https%3A%2F%2Fgraph.microsoft.com%2F.default&client_secret=qWgdYAmab0YSkuL1qKv5bPX&grant_type=client_credentials' 'https://login.microsoftonline.com/common/oauth2/v2.0/token'
```

| Parameetri | | Kirjeldus |
| ----------------------- | ------------------------------- | --------------- |
| client_id | nõutav | Rakenduse Id, et registreerimise portaali ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) määratud rakenduse. |
| ulatus | nõutav | Edastatav väärtus on `scope` parameetri selle taotluse peaks olema soovitud ressursi, millel kinnitatud ressursiidentifikaator (URI) rakenduse ID) on `.default` järelliite.  Et anda Microsoft Graphi näiteks väärtus peab olema `https://graph.microsoft.com/.default`.  See väärtus teavitab v2.0 lõpp-punkti, et kõik vahetult õiguste konfigureerimist oma rakenduse, see peaks probleemi märgiks need seotud soovitud ressurss. |
| client_secret | nõutav | Teie loodud registreerimise portaalis oma rakenduse rakenduse salajane. |
| grant_type | nõutav | Peab olema `client_credentials`. | 

#### <a name="successful-response"></a>Eduka vastus
Eduka vastuse võtab vormi:

```
{
  "token_type": "Bearer",
  "expires_in": 3599,
  "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNBVGZNNXBP..."
}
```

| Parameetri | Kirjeldus |
| ----------------------- | ------------------------------- |
| access_token | Nõutud juurdepääsu luba. Rakenduse saate kasutada seda luba turvalise ressursile, nt veebi-API autentida. |
| token_type | Näitab, et loa tüüp väärtus. Ainult tüüp, mis on Azure AD toetab `Bearer`.  |
| expires_in | Kui kaua kehtib juurdepääsu luba (sekundites). |

#### <a name="error-response"></a>Tõrge vastus
Vormi võtab vastuse tõrketeade:

```
{
  "error": "invalid_scope",
  "error_description": "AADSTS70011: The provided value for the input parameter 'scope' is not valid. The scope https://foo.microsoft.com/.default is not valid.\r\nTrace ID: 255d1aef-8c98-452f-ac51-23d051240864\r\nCorrelation ID: fb3d2015-bc17-4bb9-bb85-30c5cf1aaaa7\r\nTimestamp: 2016-01-09 02:02:12Z",
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

## <a name="use-a-token"></a>Kasutage märgiks
Nüüd, kui olete saanud märgiks, saate selle luba kutsed tegemiseks ressursi.  Luba aegumisel lihtsalt korrake taotluse selle `/token` lõpp-punkti omandada värske juurdepääsu luba.

```
GET /v1.0/me/messages
Host: https://graph.microsoft.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

```
// Pro Tip: Try the below command out! (but replace the token with your own)
```

```
curl -X GET -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q" 'https://graph.microsoft.com/v1.0/me/messages'
```

## <a name="code-sample"></a>Proovi kood
Näiteks rakenduse, et anda selle client_credentials, kasutades admin rakendab nõusoleku lõpp-punkti vaatamiseks vaadake meie [v2.0 daemon kood valimi](https://github.com/Azure-Samples/active-directory-dotnet-daemon-v2).
