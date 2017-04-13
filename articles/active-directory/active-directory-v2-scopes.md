<properties
    pageTitle="Azure'i AD v2.0 otsinguulatuste, õiguste ja nõusolekut | Microsoft Azure'i"
    description="Luba Azure AD v2.0 lõpp-punkti, sh otsinguulatuste, õiguste ja nõusolekut kirjeldus."
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

# <a name="scopes-permissions--consent-in-the-v20-endpoint"></a>Otsinguulatuste, õiguste ja nõusoleku v2.0 lõpp-punkti

Rakendused, millel on integreerimine Azure AD järgige luba mudelit, mis võimaldab kasutajatel määrata, kuidas rakenduse pääsevad oma andmetele.  See autoriseerimine mudel v2.0 rakendamine on värskendatud, muutmine, kuidas rakendus peab suhtlevad Azure AD.  Selles teemas käsitletakse seda autoriseerimine mudelit, sh otsinguulatuste, õiguste ja nõusolekut põhimõtted.

> [AZURE.NOTE]
    Kõik Azure Active Directory stsenaariumid ja funktsioonid on toetatud v2.0 lõpp-punkt.  Kui kasutate v2.0 lõpp-punkti määramiseks lugege [v2.0 piirangud](active-directory-v2-limitations.md).

## <a name="scopes--permissions"></a>Otsinguulatuste ja õigused

Azure AD rakendab [OAuth 2.0](active-directory-v2-protocols.md) autoriseerimine protokolli, mis on soovitud meetod, mis võimaldab 3 tootja rakenduse juurdepääsu veebis majutatud ressursid kasutaja nimel.  Veebis majutatud ressurss, mis ühendab Azure AD on ressursi identifikaator või **Rakenduse ID URI**.  Mõned Microsofti veebis majutatud ressursid on näiteks:

- Office 365 ühendatud e-posti API:`https://outlook.office.com`
- Azure'i AD Graph API:`https://graph.windows.net`
- Microsoft Graphi:`https://graph.microsoft.com`

Sama kehtib 3 tootja ressursse, mis on integreeritud Azure AD.  Mis tahes need ressursid saate määratleda ka õigused, mida saab kasutada funktsioone selle ressursi väiksem tükkideks jagada.  Näitena on Microsoft Graphi määratletud mõne õigusi:

- Kasutaja kalendri lugemine
- Kirjutage kasutaja kalender
- Kasutaja e-posti saatmine
- [+ rohkem](https://graph.microsoft.io)

Määratledes need õigused, saate ressursi on kohandatud kontrollida oma andmed ja kuidas see on avatud muu maailma.  3. tootja rakendust, saate taotleda järgmisi õigusi: lõppkasutaja - ja lõppkasutaja peab kinnitamine õigusi enne, kui rakendus nende nimel.  Järgi murenemist ressursi funktsioonide väiksemate õiguste komplekti, saab taotleda ainult kindlaid õigusi, et täida oma vajadusest ehitada 3 tootjate rakendused.  Samuti võimaldab lõppkasutajal täpselt kuidas rakendus kasutab oma andmeid leida, et need oleksid kindel, et rakendus on ei käitu on pahatahtlikul eesmärgil.

Azure AD ja OAuthi, on need õigused tuntud **otsinguulatuste**.  Samuti võite näha neid edaspidi **oAuth2Permissions**.  Mille ulatus on esindatud Azure AD stringi väärtus.  Microsoft Graphi näide jätkates, iga õiguse ulatus väärtus on:

- Lugege kasutaja kalendri.`Calendar.Read`
- Kirjutage kasutaja kalendri:`Mail.ReadWrite`
- Saata meilisõnumeid, kui kasutaja:`Mail.Send`

Rakenduse saate taotleda järgmisi õigusi määrates selle otsinguulatuste taotluste v2.0 lõpp-punkti, nagu allpool kirjeldatud.

## <a name="openid-connect-scopes"></a>Otsinguulatuste OpenId ühenduse loomine

V2.0 rakendamise OpenID ühendus on mõned täpselt määratletud otsinguulatuste, mis ei rakendata mis tahes ressursile - `openid`, `email`, `profile`, ja `offline_access`.

#### <a name="openid"></a>OpenId

Kui rakendus teeb sisselogimise kasutades [OpenID ühendus](active-directory-v2-protocols.md#openid-connect-sign-in-flow), seda taotleda selle `openid` ulatust.  Funktsiooni `openid` ulatus ilmub töö konto nõusolekut Kuva nimega "Teid sisse logida" õiguste ja isikliku Microsofti konto nõusolekut Kuva nimega "Profiili vaatamine ja rakenduste ja teenuste abil oma Microsofti kontoga ühenduse" õigus.  See õigus lubab rakenduse saada kasutaja ainuidentifikaator kujul soovitud `sub` taotlemine.  See pakub ka rakenduse kasutaja teave lõpp-punkti juurdepääs.  Funktsiooni `openid` ulatus saab kasutada ka v2.0 Turbeloa lõpp-punkti omandada id_tokens, mida saate kasutada secure HTTP kõned rakenduse eri osade vahel.

#### <a name="email"></a>E-posti

Funktsiooni `email` ulatus saab lisada koos selle `openid` ulatuse ja mis tahes muud.  See pakub rakenduse juurdepääs kasutaja esmase meiliaadressi kujul soovitud `email` taotlemine.  Funktsiooni `email` taotluste ainult kaasatakse sõned, kui e-posti aadress on seostatud kasutajakonto, mis ei ole alati nii.  Kui abil soovitud `email` ulatus võib rakenduse peaks olema valmis hakkama juhul, kui selle `email` taotluste pole luba.

#### <a name="profile"></a>Profiil

Funktsiooni `profile` ulatus saab lisada koos selle `openid` ulatuse ja mis tahes muud.  See pakub hulgaliselt teavet kasutaja juurdepääs rakenduse.  See sisaldab, kuid ei ole piiratud kasutaja eesnimi, perekonnanimi, kasutajanimi, objekti ID ja jne.  Profiili taotluste saadaval id_tokens konkreetse kasutaja täieliku loendi leiate [v2.0 Turbeloa viide](active-directory-v2-tokens.md).

#### <a name="offlineaccess"></a>Offline_access

Funktsiooni [ `offline_access` ulatus](http://openid.net/specs/openid-connect-core-1_0.html#OfflineAccess) rakendusel oma Accessi ressursid kasutaja nimel pikema perioodi jooksul.  Töö konto nõusolekut ekraanil kuvatakse selle ulatus "Juurdepääs igal ajal andmete" õigus.  Isiklik Microsofti konto nõusolekut ekraanil kuvatakse see õigus "Juurdepääs igal ajal oma teave".  Kui kasutaja kinnitab selle `offline_access` ulatus võib teie rakendus on lubatud saada värskendamise sõned v2.0 Turbeloa lõpp-punkti.  Värskenda märgid on vanade ja luba rakenduse omandada uue Accessi sõned nagu vanemaid aeguda.

Kui teie rakendus ei soovi selle `offline_access` ulatus, ei saadeta refresh_tokens.  See tähendab, et kui te lunastada mõne authorization_code [OAuth 2.0 autoriseerimine kood meilivoo](active-directory-v2-protocols.md#oauth2-authorization-code-flow), ainult saate tagasi mõne access_token kaudu soovitud `/token` lõpp-punkti.  Selle access_token kehtib lühiajaliselt aja (tavaliselt ühe tunni), kuid lõpuks aegub.  See ajahetkel rakenduse tuleb suunata kasutajal uuesti soovitud `/authorize` lõpp-punkti uue authorization_code toomiseks.  Ajal seda suunata kasutaja võib või ei vaja sisestage oma kasutajanimi ja parool uuesti või uuesti nõustuma õigused, sõltuvalt on rakendusest.

Saada ja kasutamise kohta lisateabe saamiseks värskendamine sõned, vaadake [v2.0 protokolli viide](active-directory-v2-protocols.md).


## <a name="requesting-individual-user-consent"></a>Taotleb üksikkasutaja luba

[OpenID ühendamine või OAuth 2.0](active-directory-v2-protocols.md) luba taotluse, saate rakenduse taotleda õigusi on vaja, kasutades funktsiooni `scope` päringu parameeter.  Näiteks, kui kasutaja logib rakendusse, rakenduse soovite saata näiteks järgmine (loetavuse reapiirid) koos:

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=query
&scope=
https%3A%2F%2Fgraph.microsoft.com%2Fcalendar.read%20
https%3A%2F%2Fgraph.microsoft.com%2Fmail.send
&state=12345
```

Funktsiooni `scope` parameeter on otsinguulatuste, mis taotleb rakenduse tühikuga eraldatud loend.  Iga üksiku ulatus on tähistatud ulatus väärtus lisamine 's ressursiidentifikaator (URI) rakenduse ID).  Ülaltoodud taotluse näitab, et rakendus peab kasutaja kalendri lugemine ja saatmine e-posti kasutaja õiguste.

Pärast seda, kui kasutaja sisestab oma volitusi, kontrollib sobitamine kirje **kasutaja nõusoleku**v2.0 lõpp-punkti.  Kui kasutaja ei ole nõus kõiki nõutud õigused varem, küsib v2.0 lõpp-punkti kasutajal nõutud õiguste andmine.  

![Töö konto nõusolekut kuvatõmmis](../media/active-directory-v2-flows/work_account_consent.png)

Kui kasutaja kinnitab luba, salvestatakse nõusoleku nii, et kasutaja ei ole edaspidised sisselogimist klõpsake uuesti nõusolekut.

## <a name="requesting-consent-for-an-entire-tenant"></a>Taotleb luba, on kogu rentniku jaoks

Sageli siis, kui ettevõtte ostud litsentsi või rakenduse tellimus, nad soovivad selle ette täielikult oma töötajatele.  Selle protsessi osana anda ettevõtte administraatori nõusoleku rakenduse iga töötaja nimel.  Kogu rentnikku nõusoleku andes ei kogenud selle organisatsiooni töötajad nõusolekut selle rakenduse kuva.

Taotlemiseks nõusolekut kõigi rentniku kasutajate jaoks, saate kasutada rakenduse soovitud **administraator nõusoleku lõpp-punkti**, allpool kirjeldatud.

## <a name="admin-restricted-scopes"></a>Otsinguulatuste administraator piiratud

Teatud Microsoft ökosüsteemis kõrge-õigused õiguste võib olla märgitud **administraator piiratud**.  Selliste otsinguulatuste näited.

- Mõne organizaion directory andmete lugemine:`Directory.Read`
- Andmete kirjutamise ettevõtte kataloogi:`Directory.ReadWrite`
- Turberühmad ettevõtte nimistust lugemine:`Groups.Read.All`

Kuigi tarbija kasutaja võib anda rakenduse juurdepääsu selliseid andmeid, organisatsiooni kasutajad on piiratud tundliku ettevõtte andmeid samu juurdepääsu andmine.  Kui rakenduse taotleb juurdepääsu ühele järgmisi õigusi ettevõtte kasutaja, saate kasutaja sellist teadet, et need on volitamata nõustuma oma rakenduse õigusi.

Kui teie rakenduse jaoks on vaja juurdepääsu nende administraator piiratud otsinguulatuste ettevõtted, peaks need otse ettevõtte administraator, kasutades selle **administraator nõusoleku lõpp-punkti**, allpool kirjeldatud koosolekukutse.

Kui administraator on annab nende õiguste haldus nõusolekut lõpp-punkti kaudu, antakse nõusolekut kõigi kasutajate rentniku, nagu eespool kirjeldatud.

## <a name="using-the-admin-consent-endpoint"></a>Administraator nõusolekut lõpp-punkti abil

Neid juhiseid järgides saab rakenduse koguda antud rentniku, sh administraator piiratud otsinguulatuste kõigi kasutajate õigusi.  Koodi näide, mis rakendab juhiseid desribes allpool vaatamiseks vaadake [administraator piiratud otsinguulatuste valimi](https://github.com/Azure-Samples/active-directory-dotnet-admin-restricted-scopes-v2).

#### <a name="request-the-permissions-in-the-app-registration-portal"></a>Rakenduse registreerimise portaali õiguste taotlemine

- Liikuge oma rakenduse [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)või [rakenduse loomine](active-directory-v2-app-registration.md) , kui te pole seda veel teinud.
- Otsige üles jaotis **Microsoft Graphi õigused** ja õigused, mis teie rakenduse jaoks on vaja lisada.
- Veenduge, et **salvestada** rakenduse registreerimine

#### <a name="recommended-sign-the-user-into-your-app"></a>Soovitatav: logige oma rakenduse kasutaja

Tavaliselt kui koostamise rakendus admin kasutab nõustu lõpp-punkti, tuleb rakendus on lehe/vaade, mis võimaldab kinnitamiseks rakenduse õiguste haldus.  Selle lehe võib olla osa rakenduse registreerumise liikumisele, rakenduse sätetes või asjakohast "ühendust" kulgemist.  Paljudel juhtudel, on mõistlik kuvada selle rakenduse "ühendust" vaade alles pärast seda, kui kasutaja on töö või kooli Microsofti kontoga sisse loginud.

Kasutaja sisselogimine rakendusse võimaldab teil tuvastada organziation, kuhu admin kuulub enne paludes neil kinnitamine vajalikke õigusi.  Ajal tingimata vajalik, see aitab teil luua oma ettevõtte kasutajate jaoks selgem kogemus.  Logige kasutaja, järgige meie [v2.0 protokolli õpetused](active-directory-v2-protocols.md).

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
| rentniku | nõutav | Directory rentniku, mida soovite taotleda luba.  Saate esitada guid või sõbralik nimi vormingus. |
| client_id | nõutav | Rakenduse Id, et registreerimise portaali ([apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)) määratud rakenduse. |
| redirect_uri | nõutav | Redirect_uri, kuhu soovite vastuse saata oma rakenduse käsitlema.  See peab täpselt vastama ühte redirect_uris, mis on registreeritud portaali. |
| olek | soovitatav | Väärtus, mis sisaldab taotluse, mis tagastatakse loa vastuse.  See võib olla mis tahes sisu, mida soovite string.  Riigi kasutatakse kodeerida teavet rakenduse kasutaja olekus enne autentimine taotluse ilmnes, näiteks lehe või vaate nad. |

Selles etapis on Azure AD jõusta, et ainult rentnikuadministraator saab sisse logida tippige koosolekukutse.  Administraator palutakse kinnitada kõigi oma rakenduse registreerimise portaalis soovitud õigused. 

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
| tõrge | Kuvatakse tõrge kood string, mis saab liigitada tüüpi ilmnevad tõrked ja seda saab kasutada reageerida tõrked. |
| error_description | Teatud tõrketeade, mis aitab tuvastada tõrke põhjuseks arendaja.  |

Kui olete saanud eduka vastuse administraator nõusolekut lõpp-punkti, rakenduse on saanud seda nõutud õigused.  Nüüd saate teisaldada peale märgiks soovitud ressursi paluda, nagu allpool kirjeldatud.

## <a name="using-permissions"></a>Kasutamise õigused

Pärast kasutaja nõustub oma rakenduse õigused, saate oma rakenduse hankimine Accessi märgid, mis kujutab teie rakendus juurdepääsuõigust mingil määral ressursi.  Antud juurdepääsu luba kasutada ainult ühe resorce, kuid kodeeritud sees on iga luba, mis on antud rakenduse selle ressursi.  Selleks et saada juurdepääsu sümboolse, rakenduse saate teha taotluse v2.0 Turbeloa lõpp-punkti.

```
POST common/oauth2/v2.0/token HTTP/1.1
Host: https://login.microsoftonline.com
Content-Type: application/json

{
    "grant_type": "authorization_code",
    "client_id": "6731de76-14a6-49ae-97bc-6eba6914391e",
    "scope": "https://outlook.office.com/mail.read https://outlook.office.com/mail.send",
    "code": "AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq..."
    "redirect_uri": "https://localhost/myapp",
    "client_secret": "zc53fwe80980293klaj9823"  // NOTE: Only required for web apps
}
```

Tulemuseks juurdepääsu luba saab kasutada siis HTTP päringuid ressursile – see tingimata näitab ressursile, et teie rakendus on õige õigust sooritada.  

Vt täpsemalt OAuth 2.0 protokoll ja kuidas hankida access sõned [v2.0 lõpp-punkti protokolli viide](active-directory-v2-protocols.md).
