<properties
    pageTitle="Muudab Azure AD v2.0 lõpp-punkti | Microsoft Azure'i"
    description="Rakenduse mudeli v2.0 avaliku eelvaate protokollide tehtud muudatuste kirjeldus."
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

# <a name="important-updates-to-the-v20-authentication-protocols"></a>Olulisi värskendusi v2.0 autentimise Protokollid
Tähelepanu arendajad! Järgmise kahe nädala jooksul me teeme mõned uuendused meie v2.0 autentimine protokollidega, mis võib tähendab suurte muudatuste kõik rakendused teie kirjutatud meie eelvaade perioodi jooksul.  

## <a name="who-does-this-affect"></a>Kes seda mõjutab?
Rakendused, mis on kirjutatud kasutada funktsiooni v2.0 lähenesid autentimise lõpp-punkti,

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
```

Lisateabe saamiseks v2.0 lõpp-punkti kohta leiate [siit](active-directory-appmodel-v2-overview.md).

Kui rakendus kodeerimise protokolli v2.0 v2.0 lõpp-punkti abil loodud kasutades ühte OpenID ühendamine või OAuthi web middlewares või muude tootjate 3. teekide autentida, teil peaks olema valmis Testige oma projekte ja vajadusel muudatusi teha.

## <a name="who-doesnt-this-affect"></a>Kes seda ei mõjuta?
Rakendused, mis on kirjutatud suhtes tootmise Azure AD autentimise endpoint,

```
https://login.microsoftonline.com/common/oauth2/authorize
```

Valitav protokoll on võimalik määrata ja pole probleeme muudatused.

Lisaks, kui teie rakendus **ainult** kasutab meie ADAL Raamatukogu autentida, ei pea midagi muuta.  ADAL on kaitstud rakenduse kaudu soovitud muudatused.  

## <a name="what-are-the-changes"></a>Milliseid muudatusi?
### <a name="removing-the-x5t-value-from-jwt-headers"></a>X5t väärtus JWT päiste eemaldamine
V2.0 lõpp-punkti kasutab JWT sõned langeda, mis sisaldavad või päisesektsioonita parameetrid koos asjakohaste metaandmeid luba.  Kui teil dekodeerida päises ühte meie praeguse JWTs, oleks umbes leidmiseks:

```
{ 
    "type": "JWT",
    "alg": "RS256",
    "x5t": "MnC_VZcATfM5pOYiJHMba9goEKY",
    "kid": "MnC_VZcATfM5pOYiJHMba9goEKY"
}
```

Kui soovitud "x5t" ja "laps" atribuudid tuvastada avalik võti, mida tuleks kasutada valideerimiseks on luba allkirja, nagu vormilt OpenID Connect metaandmete lõpp-punkti.

Muudatuse teeme siin on "x5t" atribuut eemaldada.  Teil võib jätkuvalt kasutada sama menetlustele valideerimiseks sõned, kuid tuleks kasutada ainult tuua õige avalik võti, OpenID Connect Protocol (protokoll) määratletud atribuudi "laps". 

> [AZURE.IMPORTANT] **Oma töö: Veenduge, et teie rakendus ei sõltu x5t väärtus olemasolu.**

### <a name="removing-profileinfo"></a>Profile_info eemaldamine
Varem v2.0 lõpp-punkti on antud esitus base64 kodeeritud JSON objekti Turbeloa vastustes nimetatakse `profile_info`.  Kui taotleda juurdepääsu sümboolse taotluse saatmisega v2.0 lõpp:

```
https://login.microsoftonline.com/common/oauth2/v2.0/token
```

Vastuse näeks järgmised JSON objekti:
```
{ 
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https://outlook.office.com/mail.read",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
    "refresh_token": "OAAABAAAAiL9Kn2Z27UubvWFPbm0gL...",
    "profile_info": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
}
```

Funktsiooni `profile_info` väärtust teave kasutaja logitud rakendusse - nende kuvatav nimi, eesnimi, perekonnanimi, e-posti aadressi, identifikaator jne.  Eelkõige on `profile_info` kasutati Turbeloa vahemällu ja kuvada eesmärgil.

Meil on nüüd eemaldamine on `profile_info` väärtus –, kuid ärge muretsege, anname veel seda teavet arendajatele veidi teistsugused kohas.  Asemel `profile_info`, v2.0 lõpp-punkti nüüd tagastab mõne `id_token` iga Turbeloa vastuseks:

```
{ 
    "token_type": "Bearer",
    "expires_in": 3599,
    "scope": "https://outlook.office.com/mail.read",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
    "refresh_token": "OAAABAAAAiL9Kn2Z27UubvWFPbm0gL...",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
}
```

Võib-olla dekodeerida ja sõeluda id_token saadud profile_info sama teavet alla laadida.  Funktsiooni id_token on mõne JSON Web Turbeloa (JWT), OpenID Connect määratletud sisuga.  Kood teha nii peaks olema üsna sarnased – peate lihtsalt ekstraktida soovitud id_token keskele lõigu (sisu) ja base64 dekodeerida juurdepääsu JSON objekti sees.

Järgmise kahe nädala jooksul, tuleks koodi rakenduse kas kasutaja teabe toomiseks soovitud `id_token` või `profile_info`; olenevalt sellest, kumb on olemas.  Kui muudatus on tehtud nii rakenduse sujuvalt hakkama üleminekut `profile_info` et `id_token` katkestusteta.

> [AZURE.IMPORTANT] **Oma töö: Veenduge, et teie rakendus ei sõltu olemasolu soovitud `profile_info` väärtus.**

### <a name="removing-idtokenexpiresin"></a>Id_token_expires_in eemaldamine
Sarnaselt `profile_info`, me eemaldate selle `id_token_expires_in` parameetri vastuseid.  Varem v2.0 lõpp-punkti tagasi väärtus `id_token_expires_in` koos iga id_token vastust, näiteks Autoriseerin vastuseks:

```
https://myapp.com?id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...&id_token_expires_in=3599...
```

Või loa vastuse:

```
{ 
    "token_type": "Bearer",
    "id_token_expires_in": 3599,
    "scope": "openid",
    "id_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
    "refresh_token": "OAAABAAAAiL9Kn2Z27UubvWFPbm0gL...",
    "profile_info": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsI...",
}
```

Funktsiooni `id_token_expires_in` näitab väärtus on id_token jääks jaoks sobiv sekundite arv.  Nüüd saame eemaldate selle `id_token_expires_in` täielikult väärtus.  Selle asemel võite kasutada OpenID Connect standard `nbf` ja `exp` väidab, et kontrollida ka id_token kehtivust.  Vt lisateavet [v2.0 Turbeloa viide](active-directory-v2-tokens.md) nende nõuete kohta.

> [AZURE.IMPORTANT] **Oma töö: Veenduge, et teie rakendus ei sõltu olemasolu soovitud `id_token_expires_in` väärtus.**


### <a name="changing-the-claims-returned-by-scopeopenid"></a>Tagastatud ulatus nõuete muutmise = openid
See muudatus on kõige olulisemad – tegelikult, mõjutab see peaaegu iga rakendus, mis kasutab v2.0 lõpp-punkti.  Paljud rakendused päringuid saata v2.0 lõpp-punkti abil soovitud `openid` ulatus, nt:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=...
&redirect_uri=...
&response_mode=form_post
&response_type=id_token
&scope=openid offline_access https://outlook.office.com/mail.read
```

Täna, kui kasutaja annab nõusoleku selle `openid` ulatus rakenduse saab hulgaliselt teavet kasutaja tulemiks oleva id_token.  Nende nõuete saate kaasata oma nimi, kasutajanimi, meiliaadress, objekti ID ja.

Selle värskenduse kohandame teabe mis on `openid` ulatus annab teie rakenduse access, parem comform spetsifikatsioonile OpenID ühenduse loomine.  Funktsiooni `openid` ulatus ainult võimaldab rakenduse kasutaja sisse logida, ja vastu võtta mõne konkreetse rakenduse identifikaator kasutaja soovitud `sub` väide on id_token.  Ainult mõne id_token nõuded on `openid` antud ulatus on puudub isikuandmeid.  Näide id_token nõuded on:

```
{ 
    "aud": "580e250c-8f26-49d0-bee8-1c078add1609",
    "iss": "https://login.microsoftonline.com/b9410318-09af-49c2-b0c3-653adc1f376e/v2.0 ",
    "iat": 1449520283,
    "nbf": 1449520283,
    "exp": 1449524183,
    "nonce": "12345",
    "sub": "MF4f-ggWMEji12KynJUNQZphaUTvLcQug5jdF2nl01Q",
    "tid": "b9410318-09af-49c2-b0c3-653adc1f376e",
    "ver": "2.0",
}
```

Kui soovite saada isikuandmeid (PII) rakenduse kasutaja kohta, peate oma rakenduse taotleda täiendavaid õigusi kasutaja.  Tutvustame tugi kaks uut otsinguulatuste spec OpenID Connect – selle `email` ja `profile` otsinguulatuste – mis võimaldavad teil seda teha.

- Funktsiooni `email` ulatus on väga lihtne – nii teie rakenduse juurdepääsu kasutaja esmase meiliaadressi kaudu soovitud `email` on id_token nõue.  Pange tähele, et selle `email` taotluste alati ei sisalda id_tokens – seda ainult kaasatakse võimalusel kasutajaprofiili.
- Funktsiooni `profile` ulatus annab teie rakenduse juurdepääsu kõik põhilised teavet kasutaja – oma nimi, kasutajanimi, objekti ID jne.

See võimaldab teil kood rakenduse minimaalsete avalikustamist viisil – saate esitada kasutaja jaoks lihtsalt teavet, et teie rakenduse, jaoks on vaja oma tööd teha.  Kui soovite jätkata saavad rakenduse saab praegu kasutajateavet täiskomplekti, tuleks kõigi kolme otsinguulatuste kaasamiseks oma luba kutsed.

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=...
&redirect_uri=...
&response_mode=form_post
&response_type=id_token
&scope=openid profile email offline_access https://outlook.office.com/mail.read
```

Rakenduse saate alustada saatmist selle `email` ja `profile` otsinguulatuste kohe ja v2.0 lõpp-punkti on nõustumine nende kahe otsinguulatuste ja paluda õiguste kasutajate eest vastavalt vajadusele.  Siiski muutmine tõlgendamisel on `openid` ulatus ei jõustu paar nädalat.

> [AZURE.IMPORTANT] **Oma töö: lisage soovitud `profile` ja `email` otsinguulatuste, kui teie rakendus nõuab teavet kasutaja.**  Pidage meeles, et ADAL kuvatakse nii nende õiguste taotlusi vaikimisi. 

### <a name="removing-the-issuer-trailing-slash"></a>Eemaldamine lõputühikud kaldkriips väljaandja.
Varem võeti väljaandja väärtus, mis kuvatakse sõned v2.0 lõpp

```
https://login.microsoftonline.com/{some-guid}/v2.0/
```

Kus on guid tenantId Azure AD rentniku luba andnud.  Need muudatused väljaandja väärtus muutub

```
https://login.microsoftonline.com/{some-guid}/v2.0 
```

nii sõned ja OpenID Connect discovery dokumenti.

> [AZURE.IMPORTANT] **Oma töö: Veenduge, et teie rakendus aktsepteerib väljaandja väärtus ja ilma lõpunullid kaldkriips väljaandja valideerimise käigus.**

## <a name="why-change"></a>Miks muuta?
Peamine motivatsioon tutvustus need muudatused on vastavaks OpenID Connect standard määratlus.  Olles OpenID Connect nõuetele, loodame minimeerimiseks integreerimine Microsoft identiteedi teenustega ja muude identiteedi teenustega valdkonnas erinevused.  Soovime arendajatel oma lemmik avatud andmeallika autentimise teekide abil muutmata teekide majutada Microsoft erinevused.

## <a name="what-can-you-do"></a>Mida teha?
Alates tänasest, võite alustada kõik eespool kirjeldatud muudatuste tegemist.  Mida peaks kohe.

1.  **Eemaldada sõltuvusi on `x5t` päise parameeter.**
2.  **Nõtkelt toime üleminekut `profile_info` et `id_token` Turbeloa vastused.**
3.  **Eemaldada sõltuvusi on `id_token_expires_in` vastuse parameeter.**
3.  **Lisage soovitud `profile` ja `email` otsinguulatuste rakenduse kui teie rakendus peab lihtsa Kasutajateave.**
4.  **Aktsepteerige sõned koos või ilma lõpunullid kaldkriips väljaandja väärtused.**

Meie [v2.0 protokolli dokumendid](active-directory-v2-protocols.md) on juba värskendatud kajastamiseks need muudatused, seega võite kasutada seda abimaterjalina aidata koodi värskendamise.

Kui teil on veel küsimusi ulatuse muudatusi, vastake jõuda meile Twitter veebisaidil @AzureAD.

## <a name="how-often-will-protocol-changes-occur"></a>Kui sageli protokolli muudatused juhtub?
Me ei näe, ette täpsemaks katkestamine muutub autentimise protokollid.  Meil on teadlikult komplekteerimine need muudatused sisse ühe väljaanne nii, et te ei pea seda tüüpi värskenduse toimingu uuesti igal ajal kiiresti läbida.  Muidugi jätkame ühendatud v2.0 autentimisteenus, et saate kasutada funktsioone lisada, kuid need muudatused tuleks söödalisandi ja ei katkesta olemasoleva koodi.

Lõpetuseks soovime öelda Täname asjad, mida proovida eelvaate aja jooksul.  Meie ehituseeskirjad kogemusi ja teadmisi on eriti abiks juhul siiani ja loodame, et jätkate oma arvamust ja ideede jagamiseks.

Kodeerimise palju õnne!

Microsofti Identity jagamine
