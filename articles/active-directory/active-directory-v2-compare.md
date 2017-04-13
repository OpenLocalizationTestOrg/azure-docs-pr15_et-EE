<properties
    pageTitle="Azure AD v2.0 lõpp-punkti | Microsoft Azure'i"
    description="Mõni algse Azure AD võrdlus ja v2.0 lõpp-punktid."
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

# <a name="whats-different-about-the-v20-endpoint"></a>Mille poolest erineb v2.0 lõpp-punkti kohta?

Kui olete tuttav Azure Active Directory või on integreeritud rakenduste Azure AD varem, võib olla mõned erinevused ei ootate v2.0 lõpp-punkti.  See dokument nõuab teie mõista nende erinevusi.

> [AZURE.NOTE]
    Kõik Azure Active Directory stsenaariumid ja funktsioonid on toetatud v2.0 lõpp-punkt.  Kui kasutate v2.0 lõpp-punkti määramiseks lugege [v2.0 piirangud](active-directory-v2-limitations.md).


## <a name="microsoft-accounts-and-azure-ad-accounts"></a>Microsofti kontod ja Azure AD kontod
v2.0 lõpp-punkti võimaldab arendajatel kirjutada Sisselogimine Microsofti Accounts nii Azure AD kontodelt, kasutades ühe auth endpoint aktsepteerib rakendused.  See pakub võimalust kirjutada oma rakenduse täielikult konto diagnostika; See võib olla teadlikud tüüpi kontot, mida kasutaja sisse logib.  Muidugi, *saate* muuta oma rakenduse teadlik kindla seansi kasutatava konto tüüp, kuid teil pole vaja.

Näiteks kui teie rakendus nõuab [Microsoft Graphi](https://graph.microsoft.io)täiendavate funktsioonide ja andmed on saadaval ettevõtte kasutajatele, nt nende SharePointi saitide või Directory andmed.  Kuid paljude toimingute tegemiseks [kasutaja meili lugemine](https://graph.microsoft.io/docs/api-reference/v1.0/resources/message), nt koodi saab kirjutada täpselt nii Microsofti Accounts ja Azure AD kontode jaoks.  

Rakenduse integreerimise Microsofti Accounts ja Azure AD kontod on nüüd ühe lihtne toiming.  Saate juurde pääseda nii tarbija ja ettevõtte maailma lõpp-punktid, ühte teeki ja ühest rakendusest registreerimine kogumit.  V2.0 lõpp-punkti kohta lisateabe saamiseks lugege teemat [ülevaates](active-directory-appmodel-v2-overview.md).


## <a name="new-app-registration-portal"></a>Uus rakendus registreerimise portaal
lõpp-punkti v2.0 saab ainult registreerida uude asukohta: [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList).  See on portaali, kus saate hankida rakenduse Id oma rakenduse sisselogimislehel ja muud ilmet kohandada.  Peate portaali on mida Microsofti konto - isikliku või töö/kool konto.  

Jätkame lisada rohkem funktsioone selle rakenduse registreerimise portaali aja jooksul.  Soovidele on, et see portaal uude asukohta, kus saate minna midagi ja kõike, mida teha oma Microsofti rakenduste haldamine.


## <a name="one-app-id-for-all-platforms"></a>Ühe kõigi platvormide jaoks loodud rakenduse Id
Algse Azure Active Directory teenus, mida on registreeritud mitme eri rakendused ühe projekti.  Olid sunnitud kasutamine eraldi rakenduse registreerimise kohalikke kliendid ja veebirakenduste:

![Vana rakenduse registreerimine kasutajaliides](../media/active-directory-v2-flows/old_app_registration.PNG)

Näiteks kui varem nii veebisait ja iOS-i rakendus, pidite registreerida neid eraldi, kasutades kahte erinevat rakenduste ID-sid.  Kui oleksite veebisait ja taustväärtus, mis on web api, võib registreerumist iga eraldi rakendusena Azure AD.  Kui oleksite rakendus iOS-i ja Androidi rakendus, samuti võib registreerumist kaks erinevate rakenduste.  

<!-- You may have even registered different apps for each of your build environments - one for dev, one for test, and one for production. -->

Nüüd, peate on ühest rakendusest registreerimist ja ühe rakenduse Id jaoks oma projekte.  Saate lisada mitu "platvormid" iga projekti ja esitada asjakohased andmeid iga platvormi lisamist.  Muidugi, saate luua nii palju rakendused, kui soovite vastavalt oma vajadustele, kuid enamikel juhtudel tuleb ainult üks rakenduse Id vajalikud.

<!-- You can also label a particular platform as "production-ready" when it is ready to be published to the outside world, and use that same Application Id safely in your development environments. -->

Meie eesmärk on, et see viia veel lihtsustatud rakenduse haldamise ja arendamise kogemust, ning luua kokkuvõtlik ühe projekti, mis võib olla töötasite.


## <a name="scopes-not-resources"></a>Otsinguulatuste, mitte ressursid
Algne Azure AD teenus, saate rakenduse käituvad **Ressursi**või adressaadi sõned.  Ressursi saate määrata mitmesuguseid **otsinguulatuste** või **oAuth2Permissions** , mida saab aru, võimaldades kliendi rakendused taotleda sõned selle ressursi otsinguulatuste hulk.  Kaaluge Azure AD Graph API ressursi näide.

- Ressursiidentifikaator, või `AppID URI`:`https://graph.windows.net/`
- Otsinguulatuste, või `OAuth2Permissions`: `Directory.Read`, `Directory.Write`jne.  

Kõik see kehtib ka selle v2.0 lõpp-punkti.  Rakenduse saate endiselt käitumine, kui otsinguulatuste määratlemine ja identifitseerib URI.  Kliendi rakendused saate ikka need otsinguulatuste juurdepääsu taotlemine.  Viis, kus taotleb kliendi need õigused on muutunud.  Varem on OAuth 2.0 lubada Azure AD taotluse võib olla vanal:

```
GET https://login.microsoftonline.com/common/oauth2/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&resource=https%3A%2F%2Fgraph.windows.net%2F
...
```

Kui parameetri **Ressursi** märgitud, millist ressursi taotleb kliendi rakendus luba.  Azure AD arvutada Azure'i portaalis staatilise konfiguratsiooni alusel ja sõned vastavalt välja antud rakendusest vajalikke õigusi.  Nüüd sama OAuth 2.0 lubada taotluse näeb välja umbes:

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&scope=https%3A%2F%2Fgraph.windows.net%2Fdirectory.read%20https%3A%2F%2Fgraph.windows.net%2Fdirectory.write
...
```

Kui parameetri **ulatus** näitab, milline ressurss ja õiguste rakendus taotleb luba. Soovitud ressurss on endiselt väga päringus – see on lihtsalt hõlmas iga ulatus parameetri väärtuse.  Sel viisil parameetriga ulatus võimaldab v2.0 lõpp-punkti rohkem vastavaks OAuth 2.0 spetsifikatsioon ja joondatakse täpsemalt levinud valdkonna tavasid.  See lubab ka rakenduste [suureneva nõusolekut](#incremental-and-dynamic-consent), mida on kirjeldatud järgmises jaotises sooritamiseks.

## <a name="incremental-and-dynamic-consent"></a>Täiendavad ja dünaamiline nõusolekut
Rakenduste registreeritud üldiselt kättesaadav Azure AD teenus, mis on vaja oma nõutav OAuth 2.0 õiguste määramine portaalis Azure rakenduse loomise ajal:

![Õiguste registreerimise kasutajaliides](../media/active-directory-v2-flows/app_reg_permissions.PNG)

Õiguste vajalik rakendus on konfigureeritud **staatiliselt**.  Kuigi see lubatud konfigureerimine Azure'i portaal on olemas rakendus ja hoida koodi kena ja lihtne, esitatakse mõned probleemid arendajatele:

- Rakenduse oli kõiki vajalikke õigusi selle kunagi vaja rakenduse loomise ajal.  Lisamise õigusi aja jooksul oli keeruline protsess.
- Rakenduse ma tean, et kõik selle kunagi oleks juurdepääs ette valmistada ressursid.  See on raske luua rakendusi, mis võiksid juurdepääsu suvalise arvu ressursid.
- Rakenduse pidi kõigi nende õiguste kunagi vaja pärast kasutaja esimest sisselogimist.  Mõnel juhul see viis väga pikaks loendi õigused takistada lõppkasutajad kaudu kinnitamise rakenduse juurdepääsu algse Logi sisse.

V2.0 lõpp-punkti, saate määrata rakenduse peab **dünaamiliselt**, käitusajal ajal tavalise rakenduse kasutamise õigused.  Selleks saate määrata otsinguulatuste, rakenduse peab mis tahes ajal, sealhulgas need on `scope` parameeter luba taotluse:

```
GET https://login.microsoftonline.com/common/oauth2/v2.0/authorize?
client_id=2d4d11a2-f814-46a7-890a-274a72a7309e
&scope=https%3A%2F%2Fgraph.windows.net%2Fdirectory.read%20https%3A%2F%2Fgraph.windows.net%2Fdirectory.write
...
```

Ülaltoodud taotluste õigusi rakenduse Azure AD kasutaja kataloogi andmeid, samuti kirjutada oma kataloogi andmeid lugeda.  Kui kasutaja on andnud need õigused varem selle kindla rakenduse, sisestage oma volitusi ja rakendus alla.  Kui kasutaja ei ole nõus mis tahes järgmisi õigusi, küsib v2.0 lõpp-punkti Kasutaja nõusolekut need õigused.  Lisateavet saate lugeda [õigused, nõusoleku ja otsinguulatuste](active-directory-v2-scopes.md).

Võimaldab rakenduse õiguste taotlemiseks dünaamiliselt kaudu soovitud `scope` parameetri pakub teie kasutuskogemuse üle täielik kontroll.  Soovi korral saate koondada teie nõusolekut kogemusi ja küsige ühe algse luba taotluse kõik õigused.  Või kui teie rakendus nõuab suure hulga õigused, saate valida koguda need õigused kasutajal sammhaaval, nagu nad üritavad rakenduse teatud funktsioone kasutada aja jooksul.

## <a name="well-known-scopes"></a>Tuntud otsinguulatuste

#### <a name="offline-access"></a>Ühenduseta juurdepääs
v2.0 lõpp-punkti nõuda tuntud uue õigusetaseme kasutamine rakenduste – selle `offline_access` ulatust.  Kõik rakendused on vaja taotleda see õigus, kui need on vaja juurde ka siis, kui kasutaja võib olla aktiivselt ei kasuta rakendust pikemat ressursid kasutaja nimel.  Funktsiooni `offline_access` ulatus kuvatakse kasutajale nõusolekut dialoogid nimega "Juurdepääsu andmete võrgust väljas", mille kasutaja peate nõustuma.  Paluda selle `offline_access` õigus võimaldab oma veebirakenduse saada OAuth 2.0 refresh_tokens v2.0 lõpp-punkti.  Refresh_tokens on vanade ja saab vahetada uue OAuth 2.0 access_tokens pikaks Accessi.  

Kui teie rakendus ei soovi selle `offline_access` ulatus, ei saadeta refresh_tokens.  See tähendab, et kui te lunastada mõne authorization_code [OAuth 2.0 autoriseerimine kood meilivoo](active-directory-v2-protocols.md#oauth2-authorization-code-flow), ainult saate tagasi mõne access_token kaudu soovitud `/token` lõpp-punkti.  Selle access_token kehtib lühiajaliselt aja (tavaliselt ühe tunni), kuid lõpuks aegub.  See ajahetkel rakenduse tuleb suunata kasutajal uuesti soovitud `/authorize` lõpp-punkti uue authorization_code toomiseks.  Ajal seda suunata kasutaja võib või ei vaja sisestage oma kasutajanimi ja parool uuesti või uuesti nõustuma õigused, sõltuvalt on rakendusest.

OAuth 2.0, refresh_tokens ja access_tokens kohta lisateabe saamiseks vaadake [v2.0 protokolli viide](active-directory-v2-protocols.md).

#### <a name="openid-profile--email"></a>OpenID, profiili ja e-posti

Algne Azure Active Directory teenus, annaks kõige olulisemaid OpenID Connect sisselogimise voogu hulgaliselt teavet, mis on saadud id_token kasutaja kohta.  Nõuded on id_token saate kaasata kasutaja nimi, kasutajanimi, meiliaadress, objekti ID ja muu.

Meil on nüüd piirata teabe mis on `openid` ulatus annab rakendus ei pääse juurde.  'Openid' ulatuse ainult Lubage oma rakenduse kasutaja sisse logida ja vastu võtta mõne konkreetse rakenduse identifikaator kasutaja.  Kui soovite saada isikuandmeid (PII) rakenduse kasutaja kohta, peate oma rakenduse taotleda täiendavaid õigusi kasutaja.  Tutvustame kaks uut otsinguulatuste – selle `email` ja `profile` otsinguulatuste – mis võimaldavad teil seda teha.

Funktsiooni `email` ulatus on väga lihtne – nii teie rakenduse juurdepääsu kasutaja esmase meiliaadressi kaudu soovitud `email` on id_token nõue.  Funktsiooni `profile` ulatus annab teie rakenduse juurdepääsu kõik põhilised teavet kasutaja – oma nimi, kasutajanimi, objekti ID jne.

See võimaldab teil kood rakenduse mood minimaalsete avaldamine – ainult paluda kasutaja teave, et teie rakenduse, jaoks on vaja oma tööd teha.  Nende otsinguulatuste kohta lisateabe saamiseks vaadake [v2.0 ulatus viide](active-directory-v2-scopes.md). 

## <a name="token-claims"></a>Turbeloa nõuded

Nõuded sõned välja v2.0 lõpp-punkti ei ole identne sõned välja üldiselt kättesaadav Azure AD lõpp-punktid - uus teenus migreerimine rakenduste peaks eelda teatud kindlate taotluste on olemas id_tokens või access_tokens.   Märkide v2.0 lõpp-punkti välja ühilduvad OAuth 2.0 ja OpenID Connect tehnilised andmed, kuid võib järgida erinevate semantika kui üldiselt kättesaadav Azure AD teenus.

Väljapaisatud v2.0 sõned teatud nõuete kohta leiate teemast [v2.0 Turbeloa viide](active-directory-v2-tokens.md).

## <a name="limitations"></a>Piirangud
On mõned piirangud olema teadlik v2.0 punkti kasutamisel.  Vaadake [v2.0 piirangud dokumendi](active-directory-v2-limitations.md) kuvamiseks, kui kõik need piirangud rakendamine kindla stsenaariumist.
