<properties
    pageTitle="Azure Active Directory B2C | Microsoft Azure'i"
    description="Märkide välja Azure Active Directory B2C tüüpi."
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


# <a name="azure-ad-b2c-token-reference"></a>Azure'i AD B2C: Luba viide

Azure Active Directory (Azure AD) B2C eraldab mitut tüüpi turvalisus sõned nagu töötleb iga [autentimist kulgemist](active-directory-b2c-apps.md). Selles dokumendis kirjeldatakse vorming, Turve omadusi ja igat tüüpi luba sisu.

## <a name="types-of-tokens"></a>Märkide tüübid

Azure'i AD B2C toetab [OAuth 2.0 autoriseerimine Protocol (protokoll)](active-directory-b2c-reference-protocols.md), mis kasutab nii Accessi sõned ja Värskenda märkide. Samuti toetab autentimis- ja sisselogimist kaudu [OpenID ühendus](active-directory-b2c-reference-protocols.md), mis tutvustab luba tüüpi: ID luba. Iga nende tähiste on esitatud esitaja märgiks.

Esitaja märgiks on kerge Turbeloa, mis annab "esitaja" juurdepääs kaitstud ressursiga. Esitaja on isik, mida saab esitada luba. Azure AD peavad esmalt autentimiseks peole esitaja luba vastu võtta. Kuid nõutud toimingute tagamiseks on luba edastamine ei tehta, saab see kinni ja kasutada ootamatuid poole. Mõned turvalisus märgid on sisseehitatud süsteem volitamata osapoolte takistades abil neid, kuid esitaja sõned ei ole see süsteem. Nende vedamine peab olema turvaline kanali, nt transpordikihi Turve (HTTPS).

Kui esitaja luba edastatakse väljaspool turvalise kanali, pahatahtlik tootja abil saate mees-in-Lähis rünnaku hankida luba ja kasutage seda volitamata juurdepääsu kaitstud ressursi. Turvalisus põhimõtted rakendada, kui esitaja sõned kuvamis- või hilisemaks kasutamiseks vahemällu. Alati veenduge, et teie rakendus edastab ja salvestab esitaja sõned turvaline viisil.

Täiendavad turvakaalutlused esitaja sõned kohta vt [RFC 6750 jaotise 5](http://tools.ietf.org/html/rfc6750).

Paljude Azure AD B2C probleemid märkide rakendatakse ka JSON web (JWTs). Mõne JWT on tihendatud, URL-i ohutu teabe üle kahe poole vahel. JWTs sisaldavad teavet nõuete tuntud. Need on kinnitused esitaja kohta käiva teabe ja luba teema. JWTs nõuded on JSON objekte, mis on kodeeritud ja seeriasertide edastamiseks. Kuna JWTs, Azure AD B2C välja logitud, kuid pole krüptitud, saab hõlpsalt JWT, et see silumine sisu kontrollida. Saadaval on mitu tööriista teevad, sh [calebb.net](http://calebb.net). JWTs kohta lisateabe saamiseks vaadake [JWT tehnilised andmed](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html).

### <a name="id-tokens"></a>ID sõned

Mõne ID luba on kujul Turbeloa, mis rakenduse saab Azure AD B2C `authorize` ja `token` lõpp-punktid. ID sõned on esindatud [JWTs](#types-of-tokens)ja need sisaldavad taotluste, mille abil saate tuvastada rakenduse kasutajad. Kui ID sõned saadakse soovitud `authorize` lõpp-punkti, kasutatakse sageli veebirakenduste kasutajad sisse logida. Kui ID sõned saadakse soovitud `token` lõpp-punkti, nad saavad saata HTTP päringuid jooksul suhtlemine kahest osast sama rakenduse või teenuse. Nõuded saate kasutada ka ID luba vastavalt vajadusele. Need on levinud konto teabe kuvamiseks või juurdepääsu juhtimine otsuste rakenduse.  

ID sõned logitud, kuid need pole praegu krüptitud. Kui teie rakendus või API saab ka ID luba, tuleb [allkirja valideerimiseks](#token-validation) tõestamaks, et luba autentsust. Teie rakenduse või API peab kinnitada ka mõned nõuded luba tõestamaks, et see on lubatud. Sõltuvalt stsenaarium nõuetele, kinnitanud rakenduse nõuded võivad erineda, kuid teie rakendus peab tegema mõned [levinud taotluste valideerimised](#token-validation) iga stsenaariumi.

#### <a name="sample-id-token"></a>Valimi ID luba
```
// Line breaks for display purposes only

eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6IklkVG9rZW5TaWduaW5nS2V5Q29udGFpbmVyIn0.
eyJleHAiOjE0NDIzNjAwMzQsIm5iZiI6MTQ0MjM1NjQzNCwidmVyIjoiMS4wIiwiaXNzIjoiaHR0cHM6Ly9s
b2dpbi5taWNyb3NvZnRvbmxpbmUuY29tLzc3NTUyN2ZmLTlhMzctNDMwNy04YjNkLWNjMzExZjU4ZDkyNS92
Mi4wLyIsImFjciI6ImIyY18xX3NpZ25faW5fc3RvY2siLCJzdWIiOiJOb3Qgc3VwcG9ydGVkIGN1cnJlbnRs
eS4gVXNlIG9pZCBjbGFpbS4iLCJhdWQiOiI5MGMwZmU2My1iY2YyLTQ0ZDUtOGZiNy1iOGJiYzBiMjlkYzYi
LCJpYXQiOjE0NDIzNTY0MzQsImF1dGhfdGltZSI6MTQ0MjM1NjQzNCwiaWRwIjoiZmFjZWJvb2suY29tIn0.
h-uiKcrT882pSKUtWCpj-_3b3vPs3bOWsESAhPMrL-iIIacKc6_uZrWxaWvIYkLra5czBcGKWrYwrAC8ZvQe
DJWZ50WXQrZYODEW1OUwzaD_I1f1HE0c2uvaWdGXBpDEVdsD3ExKaFlKGjFR2V7F-fPThkVDdKmkUDQX3bVc
yyj2V2nlCQ9jd7aGnokTPfLfpOjuIrTsAdPcGpe5hfSEuwYDmqOJjGs9Jp1f-eSNEiCDQOaTBSvr479L5ptP
XWeQZyX2SypN05Rjr05bjZh3j70ZUimiocfJzjibeoDCaQTz907yAg91WYuFOrQxb-5BaUoR7K-O7vxr2M-_
CQhoFA

```

### <a name="access-tokens"></a>Accessi sõned

Juurdepääsu sümboolse on samuti kujul Turbeloa, mis rakenduse saab Azure AD B2C `authorize` ja `token` lõpp-punktid. Accessi sõned on esindatud ka [JWTs](#types-of-tokens)ja need sisaldavad taotluste, mille abil saate oma veebiteenuste ja API-de kasutajate tuvastamiseks.

Accessi sõned logitud, kuid need ei ole krüptitud praegu - ja väga sarnane id sõned.  Accessi sõned tuleks kasutada veebiteenuste ja API-d juurdepääsu andmiseks ja tuvastamine ja nende teenuste kasutaja autentida.  Kuid nad ei paku mis tahes argumendi loa nende teenuste veebisaidil.  See tähendab, et selle `scp` nõude sõned juurdepääsu piirata või muul moel esindavad välja antud teema Luba juurdepääs.

Kui teie API saab juurdepääsu sümboolse, tuleb [allkirja valideerimiseks](#token-validation) tõestamaks, et luba autentsust. Oma API peab kinnitada ka mõned nõuded luba tõestamaks, et see on lubatud. Sõltuvalt stsenaarium nõuetele, kinnitanud rakenduse nõuded võivad erineda, kuid teie rakendus peab tegema mõned [levinud taotluste valideerimised](#token-validation) iga stsenaariumi.

### <a name="claims-in-id--access-tokens"></a>Nõuded ID ja Accessi sõned

Azure'i AD B2C kasutamisel on teil kohandatud juhtida oma sõned sisu. Saate konfigureerida [poliitikate](active-directory-b2c-reference-policies.md) saatmine teatud kasutaja andmekogumite väidab, et teie rakendus, mille jaoks on vaja oma toimingute. Nende nõuete võivad sisaldada näiteks kasutaja standardatribuudid `displayName` ja `emailAddress`. Võite ka [kohandatud atribuute](active-directory-b2c-reference-custom-attr.md) , mida saate määratleda B2C kataloogis. Iga ID & juurdepääsu luba, mis kuvatakse sisaldab turvalisusega seotud nõuete kogum. Rakenduste abil saate nende nõuete turvaliselt autentida kasutajate ja kutsed.

Pange tähele, et nõuded ID sõned ei tagastata kindlas järjekorras. Lisaks uute taotluste juurutatav ID sõned igal ajal. Rakenduse ei peaks murda nagu uue taotluste tuuakse. Siit leiate Azure'i AD B2C välja ID & Accessi sõned olemas eeldatavat taotluste. Täiendavate nõuete poliitika määratakse. Harjutamine, proovige kontrollimise nõuded valimi ID luba, kleepides [calebb.net](http://calebb.net). Lisateavet leiate [OpenID Connect määratlus](http://openid.net/specs/openid-connect-core-1_0.html).

| Nimi | Nõue | Näide väärtus | Kirjeldus |
| ----------------------- | ------------------------------- | ------------ | --------------------------------- |
| Sihtrühma | `aud` | `90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6` | Mõne sihtrühma taotluste tuvastab luba adressaadini. Azure'i AD B2C, publik on teie rakendus rakenduse ID, antud rakenduse app registreerimise portaalis. Rakenduse peaks kinnitage see väärtus ja hüljata luba, kui see ei vasta. |
| Väljaandja | `iss` | `https://login.microsoftonline.com/775527ff-9a37-4307-8b3d-cc311f58d925/v2.0/` | Selle nõude tuvastab turvalisus Turbeloa teenuse (STS) sisuosast ja tagastab luba. See tuvastab ka Azure AD kataloogi, kus on autenditud kasutaja. Rakenduse peaks kinnitage väljaandja taotluste tagamaks, et luba tulid v2.0 lõpp-punkti. |
| Välja | `iat` | `1438535543` | See väide on aeg, kus on antud luba, esindatud epohhi aeg. |
| Lõppemise aeg | `exp` | `1438539443` | Lõppemise aeg taotlemine luba kehtetu, mille aeg on esindatud epohhi aeg. Minirakenduse kasutama selle nõude sümboolne eluiga kehtivust.  |
| Mitte enne | `nbf` | `1438535543` | See väide on aeg mis luba kehtiv, esindatud epohhi aeg. See on tavaliselt sama, mis on antud luba aeg. Minirakenduse kasutama selle nõude sümboolne eluiga kehtivust.  |
| Versioon | `ver` | `1.0` | See on versiooni ID luba, mis on määratletud Azure AD. |
| Koodi räsi | `c_hash` | `SGCPtt01wxwfgnYZy2VJtQ` | Koodi räsi sisaldab mõnda ID luba ainult siis, kui on antud luba koos OAuth 2.0 autoriseerimine näeb välja umbes järgmine. Koodi räsi saab valideerimiseks autentsust autoriseerimine-koodi. Vaadake, kuidas sooritada kinnitamine [OpenID Connect määratlus](http://openid.net/specs/openid-connect-core-1_0.html) rohkem üksikasju. |
| Accessi Turbeloa räsi | `at_hash` | `SGCPtt01wxwfgnYZy2VJtQ` | Ainult siis, kui on antud luba juurdepääsu sümboolse OAuth 2.0 koos sisaldub mõnda Accessi Turbeloa räsi on ID luba. Saab kasutada ka Accessi Turbeloa räsi juurdepääsu sümboolse ehtsuse kinnitamiseks. Vaadake, kuidas sooritada kinnitamine [OpenID Connect määratlus](http://openid.net/specs/openid-connect-core-1_0.html) rohkem üksikasju. |
| Nonss | `nonce` | `12345` | Mõne nonss on strateegia leevendada loa kordus eest kasutada. Rakenduse saate määrata mõne nonss luba taotluse, kasutades funktsiooni `nonce` päringu parameeter. Esitate kutse väärtus elektritootmisel mille pole muudetud on `nonce` ID luba ainult väide. See võimaldab rakenduse kinnitamiseks suhtes see määratud nõudmisel rakenduse seansi seostab antud ID luba väärtus väärtus. Rakenduse tuleks sooritada kinnitamine ID Turbeloa valideerimise käigus. |
| Teema | `sub` | `Not supported currently. Use oid claim.` | See on subjekt kohta, mis kinnitab luba teave, näiteks rakenduse kasutaja. See väärtus on püsiv ja seda ei saa ümber või uuesti kasutada. Seda saab teha autoriseerimine kontrolle turvaline, nt ressursi juurdepääsu luba kasutamisel. Siiski teema taotluste pole veel täidetud Azure AD B2C. Konfigureerige oma poliitikate kaasata objekti ID `oid` taotlemine ja väärtusega abil saate tuvastada kasutajad, selle asemel, et kasutada teema nõue autoriseerimine. |
| Autentimise kontekstis klassi viide | `acr` | `b2c_1_sign_in` | See on nimi, ID luba omandada kasutatud poliitika.  |
| Autentimise aeg | `auth_time` | `1438535543` | Selle nõude on aeg, mille kasutaja sisestatud viimase identimisteabe esindatud epohhi aeg. |


### <a name="refresh-tokens"></a>Märkide värskendamine

Värskenda sõned on turvalisus märgid, mis rakenduse abil saate hankida uus ID sõned ja märkide mõne OAuth 2.0 meilivoo juurde. Nad annavad oma rakenduse pikaajalise juurdepääsu ressursse kasutajate nimel nõudmata suhtluse nende kasutajatega.

Saada Värskenda Turbeloa Turbeloa vastuseks rakenduse peab taotlemine on `offline_acesss` ulatust. Lisateavet selle `offline_access` ulatus, Soovitage [Azure AD B2C protokolli viide](active-directory-b2c-reference-protocols.md).

Värskenda sõned on, ja on alati, läbipaistmatu app. Need on välja antud Azure AD ja neid kontrollida ja tõlgendada ainult Azure AD. Need on vanade, kuid ei saa kirjutada oma rakenduse koos eeldusel, et värskendamine luba kestab teatud aja jooksul. Värskenda sõned võib olla mis tahes hetkel jaoks mitmel põhjusel kehtetuks. Ainus viis oma rakenduse teadma, kui värskendamine luba kehtib on proovib lunastada seda tehes Turbeloa taotluse Azure AD.

Kui te lunastada värskendamise luba uue märgiks (ja kui teie rakendus on `offline_access` ulatus), saate uue värskendamise loa loa vastuse. Luba äsja välja värskendamine, peaksite salvestama. See peaks värskendamise luba varem kasutatud taotluse asendada. See aitab tagada, et teie värskendamise sõned kehtivad nii kaua.

## <a name="token-validation"></a>Turbeloa valideerimine

Valideerimiseks märgiks, peaksite rakenduse nii allkiri ja luba nõuded.

Mitme avatud allika teekide on saadaval valideerimise JWTs, sõltuvalt oma eelistatud keeles. Soovitame, et saate need suvandid uurimine asemel rakendada oma valideerimise loogikat. Sellest juhendist teave aitab teil saate teada, kuidas kasutada õigesti neisse teekidesse.

### <a name="validate-the-signature"></a>Kinnitage allkiri
Mõne JWT sisaldab kolme segmente, eraldades need on `.` märk. Esimese on **päis**, teine on **sisu**ja kolmas on **allkiri**. Lõigu allkirja saab kasutada nii, et see saab usaldada, rakenduse luba ehtsuse kinnitamiseks.

Azure'i AD B2C sõned logitud industry-standard asümmeetriline krüptimise algoritmide kohta, nt RSA 256 abil. Luba päis sisaldab teavet võti ja krüptimise meetod logida luba:

```
{
        "typ": "JWT",
        "alg": "RS256",
        "kid": "GvnPApfWMdLRi8PDmisFn7bprKg"
}
```

Funktsiooni `alg` taotluste näitab algoritmi, mida kasutati logida luba. Funktsiooni `kid` taotluste näitab teatud avalik võti, mida kasutati logida luba.

Mis tahes ajal, võib Azure AD logige märgiks mõni era-avaliku võtme paari kogumi abil. Azure AD pööratakse võimalik kogum võtmed aeg-ajalt nii, et need muutub automaatselt käsitlema tuleb kirjutada oma rakenduse. Mõistlik sagedus Azure AD avaliku võtmed värskendusi on kord 24 tunni jooksul.

Azure'i AD B2C on OpenID Connect metaandmete lõpp-punkti. See võimaldab rakenduste toomiseks teavet Azure AD B2C käitusajal. See teave sisaldab lõpp-punktid, Turbeloa sisu ja logida klahvid luba. B2C kataloogi sisaldab JSON metaandmete dokumendi iga poliitika. Näiteks metaandmete dokumendi jaoks soovitud `b2c_1_sign_in` poliitika on `fabrikamb2c.onmicrosoft.com` asub aadressil:

```
https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/v2.0/.well-known/openid-configuration?p=b2c_1_sign_in
```

`fabrikamb2c.onmicrosoft.com`on kasutaja, mida kasutatakse B2C kataloogi ja `b2c_1_sign_in` on kasutatud omandada luba poliitika. Määrata, milliseid kasutati logida märgiks (ja kust toomiseks metaandmeid), on teil kaks võimalust. Esmalt poliitika nimi sisaldab selle `acr` luba nõue. Nõuded on JWT kehast saab sõeluda, base 64 dekodeerimise keha ja deserializing JSON string, mis on tulemuseks. Funktsiooni `acr` taotluste saab teie poliitika, mida kasutati probleemi luba.  Teie teine võimalus on kodeerida väärtuse poliitika on `state` parameeter kui saata, ja seejärel dekodeerida, et määrata, milliseid kasutati. Kas meetod on lubatud.

Metaandmete dokument on JSON objekti, mis sisaldab mitu kasulikku teavet. Nendeks asukoha nõutav OpenID Connect autentida lõpp-punktid. Need sisaldavad ka `jwks_uri`, mis annab asukoha komplekti avaliku klahvid, mida kasutatakse sõned logima. Asukoht on antud siin, et see on parim toomiseks asukoha dünaamiliselt kasutades metaandmete dokument ja välja sõelumine `jwks_uri`:

```
https://login.microsoftonline.com/fabrikamb2c.onmicrosoft.com/discovery/v2.0/keys?p=b2c_1_sign_in
```

JSON dokument asub see URL, mis sisaldab kõiki selle avaliku olulise teabe kasutamine hetkel. Rakenduse saate kasutada funktsiooni `kid` taotlemine JWT päises valimiseks avalik võti JSON dokument, mida kasutatakse logige kindla luba. Seda saate teha allkirja valideerimine õige avalik võti ja näidatud algoritmi abil.

Kuidas teha allkirja valideerimine kirjelduse on selle dokumendi väljapoole. Mitme avatud allika teekide on saadaval, et saaksite seda, kui teil on vaja.

### <a name="validate-the-claims"></a>Kinnitage nõuded
Kui teie rakendus või API saab ka ID luba, seda tegema ka mitu kontrollid nõuete ID luba. Need sisaldavad, kuid ei ole piiratud.

- **Sihtrühma** taotluste: See kinnitab, et pidi ID luba rakenduse pöörata.
- **Pole enne** ja **aegumise aja** nõuded: need veenduge, et ID luba on aegunud, mitte.
- **Väljaandja** taotluste: See kinnitab, et luba väljastanud app Azure AD.
- **Nonss**: see on strateegia loa kordus rünnak vähendamiseks.

Rakenduse tuleks teha valideerimised täieliku loendi vt [OpenID Connect määratlus](https://openid.net). Eelmise [jaotise Turbeloa](#types-of-tokens)on lisada oodatud väärtusega need andmed.  

## <a name="token-lifetimes"></a>Turbeloa eluajal

Järgmised Turbeloa eluajal on olemas täiendavaid oma teadmisi. Need aitavad teil, kui teil tekib ja silumine rakendusi. Pange tähele, et teie rakendused tuleks alati kirjutada oodata, mis tahes need kestused, et need ei muutu. Nad saavad ja võib muutuda.  Lugege lisateavet Turbeloa eluajal Azure AD B2C kohandamise kohta [siin](active-directory-b2c-token-session-sso.md).

| Turbeloa | Eluiga | Kirjeldus |
| ----------------------- | ------------------------------- | ------------ |
| ID sõned | Üks tund | ID sõned kehtivad tavaliselt üks tund. Oma veebirakenduse abil saate selle eluiga säilitada oma seansid kasutajatega (soovitatav). Saate valida ka muu seansi eluiga. Kui teie rakendus peab saada uut ID-d Turbeloa, tuleb lihtsalt teha uus koosolekukutse sisselogimise Azure AD. Kui kasutaja on kehtiv brauseriseansi Azure AD, võib selle kasutaja ei pea sisestage kasutajanimi ja parool uuesti. |
| Märkide värskendamine | Kuni 14 päeva jooksul | Ühe värskendamise luba kehtib kuni 14 päeva jooksul. Siiski märgiks värskendamine võib muutuda sobimatu igal ajal mis tahes mitmel põhjusel. Rakenduse jätkama, proovige kasutada värskendamise luba kuni taotlus nurjub, või oma app vahetab välja rakenduse värskendamine luba uuega.  Ka märgiks värskendamine võib muutuda kui 90 päeva on möödunud viimati sisestatud kasutaja mandaat. |
| Luba koodid | Viie minuti järel | Luba koodid on teadlikult lühiajaline. Nad peaksid lunastada kohe juurdepääsu sõned, ID sõned või Värskenda sõned saabumisel. |
