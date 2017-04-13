<properties
    pageTitle="Azure'i AD v2.0 Turbeloa viide | Microsoft Azure'i"
    description="Märkide ja taotluste kiiratava v2.0 lõpp-punkti tüübid"
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

# <a name="v20-token-reference"></a>v2.0 Turbeloa viide

Lõpp-punkti v2.0 eraldab mitut tüüpi turvalisus märkide iga [autentimist meilivoo](active-directory-v2-flows.md)töötlemine. Selles dokumendis kirjeldatakse vorming, Turve omadusi ja igat tüüpi luba sisu.

> [AZURE.NOTE]
    Kõik Azure Active Directory stsenaariumid ja funktsioonid on toetatud v2.0 lõpp-punkt.  Kui kasutate v2.0 lõpp-punkti määramiseks lugege [v2.0 piirangud](active-directory-v2-limitations.md).

## <a name="types-of-tokens"></a>Märkide tüübid

V2.0 lõpp-punkti toetab [OAuth 2.0 autoriseerimine Protocol (protokoll)](active-directory-v2-protocols.md), mis kasutab nii access_tokens ja refresh_tokens.  Samuti toetab autentimis- ja Logi sisse [OpenID ühendus](active-directory-v2-protocols.md#openid-connect-sign-in-flow), mis tutvustab tüüpi luba, et id_token kaudu.  Iga nende tähiste on esitatud märgiks"esitaja".

Esitaja märgiks on kerge Turbeloa, mis annab "esitaja" juurdepääs kaitstud ressursiga. Selles mõttes on "esitaja" igal pool, mida saab esitada luba. Kuigi peole tuleb esmalt autentimiseks Azure AD vastuvõtmiseks esitaja luba, kui ei võeta nõutud toimingute tagamiseks on luba edastamine, saab see kinni ja kasutada ootamatuid poole. Mõned turvalisus sõned on sisseehitatud süsteem volitamata isikutele takistades abil neid, esitaja sõned ei ole see süsteem ja turvalise kanali, nt transpordikihi Turve (HTTPS) vedamine peab. Kui esitaja luba edastatakse selge, on mees-keskmisel rünnak saab pahatahtlik isik hankida luba ja kasutage seda volitamata juurdepääsu kaitstud ressursi. Turvalisus põhimõtted rakendada, kui salvestamise või esitaja sõned hilisemaks kasutamiseks vahemällu. Alati veenduge, et teie rakendus edastab ja salvestab esitaja sõned turvaline viisil. Vaadake veel turvakaalutlused esitaja sõned, [RFC 6750 jaotise 5](http://tools.ietf.org/html/rfc6750).

Mitme välja v2.0 lõpp-punkti märkide rakendatakse Json Web sõned või JWTs.  Mõne JWT on tihendatud, URL-i ohutu teabe üle kahe poole vahel.  JWTs sisalduvat teavet nimetatakse "nõuded" või kinnitused teabe esitaja ja luba teema kohta.  JWTs nõuded on JSON objektide kodeeritud ja seeriasertide edastamiseks.  Kuna JWTs, v2.0 lõpp-punkti välja logitud, kuid pole krüptitud, saate hõlpsasti kontrolli sisu on JWT silumine eesmärgil. Lisateavet JWTs, võib viidata [JWT määratlus](http://self-issued.info/docs/draft-ietf-oauth-json-web-token.html).

## <a name="idtokens"></a>Id_tokens

Id_tokens on sisselogimise Turbeloa rakenduse saab autentimist kasutades [OpenID Connect](active-directory-v2-protocols.md#openid-connect-sign-in-flow)täites vormile.  Need on esindatud [JWTs](#types-of-tokens)ja sisaldavad taotluste, mille abil saate oma rakenduse kasutaja rakendusse.  Nagu näed – need on levinud konto teabe kuvamine või juurdepääsu juhtimine otsuste rakenduse saate kasutada ka id_token nõuded.  Lõpp-punkti v2.0 probleemid ainult ühte tüüpi id_token, mis on ühtsed nõuded sõltumata sellest, milline kasutaja, mis on sisse logitud.  Mis on öelda, et selle id_tokens sisu ja vormingu saab sama nii isikliku Microsofti Account kasutajate ja töö-või koolikonto.

Id_tokens sisse loginud, kuid seekord krüptitud.  Kui minirakenduse saab ka id_token, tuleb [allkirja valideerimiseks](#validating-tokens) tõendada selle luba autentsust ja mõned nõuded luba tõendada selle kehtivust kinnitada.  Kinnitatud rakenduse nõuded erinevad sõltuvalt stsenaarium nõuetele, kuid on mõned [levinud taotluste valideerimised](#validating-tokens) , mis rakenduse peate tegema iga stsenaarium.

Täielik id_tokens nõuded on esitatud allpool, samuti valimi id_token.  Pange tähele, et id_tokens nõuded ei tagastata kindlas järjekorras.  Lisaks uute taotluste võimalik viia id_tokens igal ajal - rakenduse peaks leheküljepiiri, nagu uute taotluste tuuakse.  Järgmine loend sisaldab taotluste, mis rakenduse saab usaldusväärselt tõlgendamine kirjutamise ajal.  Vajaduse korral leiate veel rohkem üksikasju [OpenID Connect määratlus](http://openid.net/specs/openid-connect-core-1_0.html).

#### <a name="sample-idtoken"></a>Valimi id_token

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImtpZCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSJ9.eyJhdWQiOiI2NzMxZGU3Ni0xNGE2LTQ5YWUtOTdiYy02ZWJhNjkxNDM5MWUiLCJpc3MiOiJodHRwczovL2xvZ2luLm1pY3Jvc29mdG9ubGluZS5jb20vYjk0MTk4MTgtMDlhZi00OWMyLWIwYzMtNjUzYWRjMWYzNzZlL3YyLjAiLCJpYXQiOjE0NTIyODUzMzEsIm5iZiI6MTQ1MjI4NTMzMSwiZXhwIjoxNDUyMjg5MjMxLCJuYW1lIjoiQmFiZSBSdXRoIiwibm9uY2UiOiIxMjM0NSIsIm9pZCI6ImExZGJkZGU4LWU0ZjktNDU3MS1hZDkzLTMwNTllMzc1MGQyMyIsInByZWZlcnJlZF91c2VybmFtZSI6InRoZWdyZWF0YmFtYmlub0BueXkub25taWNyb3NvZnQuY29tIiwic3ViIjoiTUY0Zi1nZ1dNRWppMTJLeW5KVU5RWnBoYVVUdkxjUXVnNWpkRjJubDAxUSIsInRpZCI6ImI5NDE5ODE4LTA5YWYtNDljMi1iMGMzLTY1M2FkYzFmMzc2ZSIsInZlciI6IjIuMCJ9.p_rYdrtJ1oCmgDBggNHB9O38KTnLCMGbMDODdirdmZbmJcTHiZDdtTc-hguu3krhbtOsoYM2HJeZM3Wsbp_YcfSKDY--X_NobMNsxbT7bqZHxDnA2jTMyrmt5v2EKUnEeVtSiJXyO3JWUq9R0dO-m4o9_8jGP6zHtR62zLaotTBYHmgeKpZgTFB9WtUq8DVdyMn_HSvQEfz-LWqckbcTwM_9RNKoGRVk38KChVJo4z5LkksYRarDo8QgQ7xEKmYmPvRr_I7gvM2bmlZQds2OeqWLB1NSNbFZqyFOCgYn3bAQ-nEQSKwBaA36jYGPOVG2r2Qv1uKcpSOxzxaQybzYpQ
```

> [AZURE.TIP] Harjutamine, proovige kontrollimise nõuded valimi id_token, kleepides [calebb.net](https://calebb.net).

#### <a name="claims-in-idtokens"></a>Nõuete id_tokens
| Nimi | Nõue | Näide väärtus | Kirjeldus |
| ----------------------- | ------------------------------- | ------------ | --------------------------------- |
| Sihtrühma | `aud` | `6731de76-14a6-49ae-97bc-6eba6914391e` | Tuvastab luba adressaadini.  Id_tokens, publik on teie rakendus rakenduse Id, määrata rakenduse app registreerimise portaalis.  Rakenduse peaks kinnitage see väärtus ja hüljata luba, kui see ei vasta. |
| Väljaandja | `iss` | `https://login.microsoftonline.com/b9419818-09af-49c2-b0c3-653adc1f376e/v2.0 ` | Turvalisus Turbeloa teenuse (STS) sisuosast ja tagastab luba, samuti Azure AD rentniku, kus on autenditud kasutaja ID.  Rakenduse peaks valideerimiseks väljaandja taotluste tagamaks, et luba tulid v2.0 lõpp-punkti.  See tuleks kasutada ka guid osa nõude piirata kogumi rentnikud, millel on lubatud rakendusse sisse logida.  Guid, mis näitab, et kasutajal on Microsofti konto on tarbija kasutaja `9188040d-6c67-4c5b-b112-36a304b66dad`. |
| Välja | `iat` | `1452285331` | Kus antud luba, aeg esindatud epohhi aeg. |
| Aegumise aeg | `exp` | `1452289231` | Luba kehtetu, mille aeg esindatud epohhi aeg.  Minirakenduse kasutama selle nõude sümboolne eluiga kehtivust.  |
| Mitte enne | `nbf` | `1452285331` |  Luba kehtiv, mille aeg esindatud epohhi aeg. Tavaliselt on sama, mis Emissiooni aeg.  Minirakenduse kasutama selle nõude sümboolne eluiga kehtivust.  |
| Versioon | `ver` | `2.0` | Id_token, mis on määratletud Azure AD versioon.  V2.0 lõpp-punkti väärtuse puhul `2.0`. |
| Rentniku Id | `tid` | `b9419818-09af-49c2-b0c3-653adc1f376e` | Guid tähistav Azure AD rentniku, mis on kasutajale.  Töö ja kooli kontodelt guid saab kasutaja kuuluva ettevõtte püsiv rentniku ID-d.  Isiklikud kontod, väärtuse puhul `9188040d-6c67-4c5b-b112-36a304b66dad`.  Funktsiooni `profile` ulatus on vaja selleks, et saada selle nõude. |
| Koodi räsi | `c_hash` | `SGCPtt01wxwfgnYZy2VJtQ` | Koodi räsi sisaldub id_tokens ainult siis, kui selle id_token välja kõrval OAuth 2.0 autoriseerimine näeb välja umbes järgmine.  Seda saab kasutada valideerimiseks autentsust autoriseerimine-koodi.  [OpenID Connect määratlus](http://openid.net/specs/openid-connect-core-1_0.html) üksikasjalikumat teavet leiate teemast läbimiseks kinnitamine. |
| Accessi Turbeloa räsi | `at_hash` | `SGCPtt01wxwfgnYZy2VJtQ` | Accessi Turbeloa räsi sisaldub id_tokens ainult siis, kui selle id_token antakse OAuth 2.0 juurdepääsu sümboolse kõrval.  Saate kasutada juurdepääsu sümboolse ehtsuse kinnitamiseks.  [OpenID Connect määratlus](http://openid.net/specs/openid-connect-core-1_0.html) üksikasjalikumat teavet leiate teemast läbimiseks kinnitamine. |
| Nonss | `nonce` | `12345` | Funktsiooni nonss on strateegia loa kordus eest leevendada.  Rakenduse saate määrata mõne nonss luba taotluse, kasutades funktsiooni `nonce` päringu parameeter.  Esitate kutse väärtus elektritootmisel klõpsake soovitud id_token `nonce` taotluste, mille pole muudetud.  See võimaldab rakenduse kinnitamaks, et see seostab antud id_token rakenduse seansi taotluse märgitud väärtuse väärtus.  Rakenduse tuleks sooritada kinnitamine id_token valideerimise käigus. |
| Nimi | `name` | `Babe Ruth` | Nimi nõude pakub inimeste loetav väärtus, mis tuvastab luba teema. See väärtus on tagatud olema kordumatud, on muutlik ja on mõeldud kasutamiseks ainult kuvatava taustvärvi.  Funktsiooni `profile` ulatus on vaja selleks, et saada selle nõude. |
| E-posti | `email` | `thegreatbambino@nyy.onmicrosoft.com` | Esmane meiliaadress seotud kasutajakonto, kui see on olemas.  Selle väärtus on muutlik ja võivad aja jooksul muutuda konkreetse kasutaja.  Funktsiooni `email` ulatus on vaja selleks, et saada selle nõude. |
| Kasutajanimi | `preferred_username` | `thegreatbambino@nyy.onmicrosoft.com` | Esmane kasutajanimi v2.0 lõpp-punkti Kasutaja väljendamiseks kasutatakse.  See võib olla meiliaadressi, telefoninumbri või ilma määratud vorming üldine kasutajanimi.  Selle väärtus on muutlik ja võivad aja jooksul muutuda konkreetse kasutaja.  Funktsiooni `profile` ulatus on vaja selleks, et saada selle nõude. |
| Teema | `sub` | `MF4f-ggWMEji12KynJUNQZphaUTvLcQug5jdF2nl01Q` | Laenu põhisumma kohta, mis kinnitab luba teave, näiteks rakenduse kasutaja. See väärtus ei püsiv ja ei saa ümber või uuesti kasutada, nii et kasutamist autoriseerimine kontrolle teha turvaline, nt ressursi juurdepääsu luba kasutamisel. Kuna teema on alati olemas sõned Azure AD probleeme, soovitame selle väärtuse kasutamine üldine otstarve autoriseerimine süsteemi. |
| ObjectId väärtuse | `oid` | `a1dbdde8-e4f9-4571-ad93-3059e3750d23` | Objekti ID-d Azure AD süsteemi töökoha või kooli kontoga.  Selle nõude ei anta välja isikliku Microsofti kontode puhul.  Funktsiooni `profile` ulatus on vaja selleks, et saada selle nõude. |


## <a name="access-tokens"></a>Accessi sõned

Välja v2.0 lõpp-punkti juurdepääs sõned on ainult sel hetkel tarbitavad Microsoft Services.  Saate teha mõnda valideerimine või Accessi sõned kontrolli mis tahes hetkel toetatud stsenaariumid rakenduste ei vaja.  Te saate käsitleda Accessi sõned läbipaistmatu - nad on lihtsalt stringid, mis rakenduse Microsoft lähevad sisse HTTP päringuid.

Tulevikus, v2.0 lõpp-punkti tutvustab oma rakenduse saada juurdepääsu sõned teiste klientide võimalus.  Sel ajal, värskendatakse selle teabe teabe rakenduse access Turbeloa valideerimise ja muid sarnaseid ülesandeid täita.

Kui saate taotleda juurdepääsu sümboolse v2.0 lõpp-punkti, tagastab v2.0 lõpp-punkti ka mõned metaandmeid juurdepääsu luba oma rakenduse ettenähtud.  See teave sisaldab aegumise korral luba juurdepääs ja otsinguulatuste, mille jaoks on lubatud.  See võimaldab teha nutikad vahemällu Accessi sõned ilma sõeluda avatud rakenduse juurdepääsu luba ise.

## <a name="refresh-tokens"></a>Märkide värskendamine

Värskenda sõned on turvalisus märgid, mis rakenduse abil saate hankida uus juurdepääs märkide mõne OAuth 2.0 kulgemist.  See võimaldab rakenduse suhtlust kasutaja nõudmata pikaajalise juurdepääsu ressursid kasutaja nimel saavutamiseks.

Värskenda sõned on mitme ressurss.  See tähendab, et üks ressurss Turbeloa taotluse vastu värskendamise luba saab lunastada hoopis ressursile juurdepääsu märkide.

Turbeloa vastuseks värskendamise saamiseks rakenduse peab taotleda ja anda selle `offline_acesss` ulatust.   Lisateavet selle `offline_access` ulatus, vaadake [nõusolekut & otsinguulatuste artiklis allpool](active-directory-v2-scopes.md).

Värskenda sõned on, ja on alati, läbipaistmatu app.  Need on välja antud Azure AD v2.0 lõpp-punkti ja saate ainult tuleb kontrollida ja tõlgendada v2.0 lõpp-punkt.  Need on vanade, kuid rakenduse tuleks alati kirjutada oodata märgiks värskendamise kestab aega.  Värskenda sõned võib olla mis tahes ajahetkel jaoks mitmel põhjusel kehtetuks.  Ainus viis oma rakenduse teadma, kui värskendamine luba kehtib on proovib lunastada seda tehes Turbeloa taotluse v2.0 lõpp-punkti.

Kui te lunastada värskendamise luba jaoks uus juurdepääsu luba (ja kui antud rakenduse soovitud `offline_access` ulatus), saate uue värskendamise loa loa vastuse.  Peaksite salvestama äsja väljastatud värskendamise luba, asendaks kasutasite kutse.  See tagab, et teie värskendamise sõned kehtivad nii kaua.

## <a name="validating-tokens"></a>Märkide kontrollimine

Sel hetkel ainult Turbeloa valideerimise rakenduste peaks tegema valideerib id_tokens.  Mõne id_token valideerimiseks peaks rakenduse kinnitage nii on id_token allkiri ja selle id_token nõuded.

<!-- TODO: Link -->
Pakume teekide ja koodinäiteid, mis näitavad reageerimine hõlpsalt Turbeloa kontroll - selle all teabe lihtsalt jaoks on esitatud neile, kes soovivad mõista aluseks protsess.  Saadaval on mitu 3 tootja avatud allika teekide JWT valideerimisreeglite – on vähemalt üks võimalus peaaegu iga platvormi ja seal keele jaoks.

#### <a name="validating-the-signature"></a>Allkirja kontrollimine
Mõne JWT sisaldab kolme segmente, mis on eraldatud soovitud `.` märgi.  Esimese tuntakse **päise** **sisu**teine ja kolmas **allkirja**.  Lõigu allkirja saab kasutada nii, et see saab usaldada, rakenduse soovitud id_token ehtsuse kinnitamiseks.

Id_Tokens logitud valdkonna asümmeetriline tavalise krüptimise algoritmide kohta, nt RSA 256. Funktsiooni id_token päis sisaldab teavet võti ja krüptimise meetod logida luba:

```
{
  "typ": "JWT",
  "alg": "RS256",
  "kid": "MnC_VZcATfM5pOYiJHMba9goEKY"
}
```

Funktsiooni `alg` taotluste näitab algoritmi kasutatud logida Turbeloa, samal ajal soovitud `kid` taotluste näitab teatud avalik võti, mida kasutati logida luba.

Antud hetkel aega, võib v2.0 lõpp-punkti logige sisse mõne id_token ühe era-avaliku võtme paari kogum.  V2.0 lõpp-punkti pööratakse võimalike määramine klahvireas perioodiliselt nii, et need muutub automaatselt käsitlema tuleb kirjutada oma rakenduse.  Mõistlik sagedus värskendusi avaliku võtmed v2.0 lõpp-punkt on umbes 24 tundi.

Saate hankida allkirjastamiseks võtme andmed, kasutades OpenID Connect metaandmete dokument asub allkirja valideerimiseks vajalikud:

```
https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration
```

> [AZURE.TIP] Proovige seda URL-i brauseri!

See metaandmete dokument on JSON objekti, mis sisaldavad mitu kasulikku tekstilõigule teave, näiteks asukoha läbimiseks OpenID Connect autentimine nõutav erinevad lõpp-punktid.  

See hõlmab ka on `jwks_uri`, mis annab avaliku abil logige sõned kogumi asukoht.  JSON dokument asub selle `jwks_uri` sisaldab kõiki avaliku olulist teavet kasutab seda teatud ajahetkel.  Rakenduse saate kasutada funktsiooni `kid` taotlemine JWT päises valimiseks, mis avalik võti selles dokumendis on kasutatud logida kindla luba.  Seda saate teha allkirja valideerimine õige avalik võti ja näidatud algoritmi abil.

Allkirja valideerimine läbimiseks on selle dokumendi väljapoole – mitme avatud allika teekide on saadaval, mis aitavad teil teha vajaduse korral.

#### <a name="validating-the-claims"></a>Nõuded kontrollimine
Kui minirakenduse saab ka id_token korral kasutajate sisselogimist, seda tegema ka mõned kontrollid nõuete soovitud id_token.  Need sisaldavad, kuid ei ole piiratud:

- **Sihtrühma** nõude - kinnitamiseks pidi soovitud id_token rakenduse pöörata.
- **Pole enne** ja **Aegumise aja** nõuete - kinnitamaks, et selle id_token on aegunud, ei.
- **Väljaandja** nõude - kinnitamaks, et luba tõepoolest väljastanud app v2.0 lõpp-punkti.
- Funktsiooni **nonss** - nimega loa kordus rünnak vähendamiseks.
- ja muud...

Täielik loend taotluste valideerimised rakenduse tegema, vaadake [OpenID Connect määratlus](http://openid.net/specs/openid-connect-core-1_0.html#IDTokenValidation).

Üksikasjad oodatud väärtusega need sisalduvad kohal [id_token jaotis](#id_tokens).


## <a name="token-lifetimes"></a>Turbeloa eluajal

Järgmised Turbeloa eluajal pakutakse puhtalt mõistmise, nagu need aitavad arendamise ja silumine rakendusi.  Rakenduste tuleks alati kirjutada oodata, mis tahes need kestused, et need ei muutu – nad saavad ja mis tahes ajal muuta.

| Turbeloa | Eluiga | Kirjeldus |
| ----------------------- | ------------------------------- | ------------ |
| Id_Tokens (töö- või koolikonto kontod) | 1 tund | Id_Tokens kehtivad tavaliselt üks tund.  Teie web Appis saab kasutada seda sama eluiga säilitada oma seansi kasutaja (soovitatav) või valige hoopis seansi eluiga.  Kui teie rakendus peab saada uut id_token, tuleb lihtsalt esitada uue sisselogimist kuvatakse v2.0 lubada lõpp-punkti.  Kui kasutaja on kehtiv brauseriseansi koos v2.0 lõpp-punkti, nad võivad ei pea sisestage oma kasutajanimi ja parool uuesti. |
| Id_Tokens (isiklikud kontod) | 24 tunni | Id_Tokens kontode jaoks kehtivad tavaliselt 24 tundi.  Teie web Appis saab kasutada seda sama eluiga säilitada oma seansi kasutaja (soovitatav) või valige hoopis seansi eluiga.  Kui teie rakendus peab saada uut id_token, tuleb lihtsalt esitada uue sisselogimise soovitud v2.0 lubada lõpp-punkti.  Kui kasutaja on kehtiv brauseriseansi koos v2.0 lõpp-punkti, nad võivad ei pea sisestage oma kasutajanimi ja parool uuesti. |
| Accessi sõned (töö- või koolikonto kontod) | 1 tund | Nagu on näidatud Turbeloa vastuste Turbeloa metaandmete osana. |
| Accessi sõned (isiklikud kontod) | 1 tund | Nagu on näidatud Turbeloa vastuste Turbeloa metaandmete osana.  Isiklikud kontod nimel välja Access_tokens võib olla konfigureeritud erinevate eluiga, kuid tund on tavaliselt nii. |
| Värskenda sõned (töökoha või kooli kontoga) | Kuni 14 päeva jooksul | Ühe värskendamise luba kehtib kuni 14 päeva jooksul.  Siiski luba värskendamine võib muutuda sobimatu igal ajal mis tahes mitmel põhjusel, nii rakenduse jätkama proovida ja kasutada kuni selle nurjub, või rakenduse asendab selle uue värskendamise loa märgiks värskendamine.  Värskenda märgiks ka muutub sobimatu, kui kasutaja on sisestanud oma volitusi on 90 päeva möödunud. |
| Värskenda sõned (isiklikud kontod) | Kuni 1 aasta | Ühe värskendamise luba kehtib 1 aasta jooksul.  Siiski luba värskendamine võib muutuda sobimatu igal ajal mis tahes mitmel põhjusel, nii, et teie rakendus jätkama proovida ja kasutada märgiks värskendamise, kuni see ei õnnestu. |
| Luba koode (töö- või koolikonto kontod) | 10 minuti | Luba koodid on sihikindlalt lühiajaline ja tuleks kohe lunastada access_tokens ja refresh_tokens saabumisel. |
| Luba koode (isiklikud kontod) | 5 minutit. | Luba koodid on sihikindlalt lühiajaline ja tuleks kohe lunastada access_tokens ja refresh_tokens saabumisel.  Luba koodid nimel isiklikud kontod välja on ka ühekordse kasutamine. |
