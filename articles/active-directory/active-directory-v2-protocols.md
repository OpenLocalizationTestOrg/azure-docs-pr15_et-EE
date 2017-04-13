<properties
    pageTitle="Azure'i AD v2.0 Protokollid | Microsoft Azure'i"
    description="Juhend Protokollid Azure AD v2.0 lõpp-punkti ei toeta."
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

# <a name="v20-protocols---oauth-20--openid-connect"></a>v2.0 Protokollid - OAuth 2.0 ja OpenID ühenduse loomine

Identiteedi-kui-a-service valdkonna standard protokollide OpenID ühendamine ja OAuth 2.0 saate Azure AD v2.0 lõpp-punkti.  Teenus on standardses, võib olla mis tahes kahe rakendusi protokolli väike erinevused.  Teave siin on kasu, kui valite saates otse oma koodi kirjutamine ja töötlemise HTTP päringuid või kasutage mõnda 3. peo avage allikas teeki, mitte ühte meie avatud allika teekide abil.
<!-- TODO: Need link to libraries above -->

> [AZURE.NOTE]
    Kõik Azure Active Directory stsenaariumid ja funktsioonid on toetatud v2.0 lõpp-punkt.  Kui kasutate v2.0 lõpp-punkti määramiseks lugege [v2.0 piirangud](active-directory-v2-limitations.md).

## <a name="the-basics"></a>Põhitõed
Peaaegu kõik OAuthi ja OpenID Connect puhul on neli osalist kaasatud exchange.

![OAuthi 2.0 rollid](../media/active-directory-v2-flows/protocols_roles.png)

- **Luba Server** on v2.0 lõpp-punkti.  See on tagada kasutaja identiteet, andmise ja tühistamise ressurssidele ja väljasta lube.  Seda nimetatakse ka identiteedipakkuja – käsitlemisel turvaliselt midagi teha kasutaja teabe, nende juurdepääsu ja usalda mõne meilivoo vahel seoseid.
- **Ressursi omanik** on tavaliselt lõppkasutaja.  See on pool, kes andmed kuulub, ja kellel on õigus muude tootjate andmed, või ressursside juurdepääsu lubamiseks.
- **OAuthi kliendi** on rakenduse tähistatud oma rakenduste ID-ga.  See on tavaliselt tootja, mille lõppkasutaja suhtleb ja selle taotleb sõned autoriseerimine serverist.  Kliendi peab olema antud ressursile juurdepääsu õigusega ressursi omanik.
- **Ressursi Server** on ressurss või andmete asukoht.  See loodab turvaliseks autentimiseks ja lubada OAuthi kliendi autoriseerimine Server, ja kasutab esitaja access_tokens tagamaks, et saate anda juurdepääsu ressursile.


## <a name="app-registration"></a>Rakenduse registreerimine
Iga rakendus, mis kasutab v2.0 lõpp-punkti peate olema registreeritud [apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) enne saavad omavahel suhelda, kasutades OAuthi või OpenID ühendus.  Rakenduse registreerimise käigus on koguda ja rakenduse väärtuste määramine:

- **Rakenduse Id** , mis tuvastab kordumatult rakenduse
- **Suunake URI** või saab kasutada vastuste tagasi rakendusse otsesed **Paketi identifikaator**
- Mõne muu stsenaarium kohased väärtused.

Lisateavet leiate siit saate teada, kuidas [registreerida rakendus](active-directory-v2-app-registration.md).

## <a name="endpoints"></a>Lõpp-punktid
Kui olete registreerunud, rakenduse suhtleb Azure AD päringu saatmine v2.0 lõpp-punkt:

```
https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize
https://login.microsoftonline.com/{tenant}/oauth2/v2.0/token
```

Kus on `{tenant}` saate teha ühte neljast erinevaid väärtusi:

| Väärtus | Kirjeldus |
| ----------------------- | ------------------------------- |
| `common` | Võimaldab kasutajal isikliku Microsofti kontod ja töö/kool kontod Azure Active Directory rakendusse sisse logida. |
| `organizations` | Võimaldab Azure Active Directory rakendusse sisse logida ainult need kasutajad, kellel töö/kool kontod. |
| `consumers` | Võimaldab ainult need kasutajad, isikliku Microsofti kontoga (MSA) rakendusse sisse logida. |
| `8eaef023-2b34-4da1-9baa-8bc8c9d6a490`või`contoso.onmicrosoft.com` | Võimaldab kindla Azure Active Directory rentnikust rakendusse sisse logida ainult need kasutajad, kellel töö/kool kontod.  Kas Azure AD rentniku sõbralik domeeninime või rentniku guid identifikaator saab kasutada.  |

Lisateavet selle kohta, kuidas kasutada nende lõpp-punktid, valige allolev konkreetse rakenduse tüüp.

## <a name="tokens"></a>Märkide
V2.0 rakendamise OAuth 2.0 ja OpenID ühendamine kasutama olulisel esitaja sõned, sh esitaja sõned JWTs arvuna. Esitaja märgiks on kerge Turbeloa, mis annab "esitaja" juurdepääs kaitstud ressursiga. Selles mõttes on "esitaja" isik, mida saab esitada luba. Kuigi peole tuleb esmalt autentimiseks Azure AD vastuvõtmiseks esitaja luba, kui ei võeta nõutud toimingute tagamiseks on luba edastamine, saab see kinni ja kasutada ootamatuid poole. Mõned turvalisus sõned on sisseehitatud süsteem volitamata isikutele takistades abil neid, esitaja sõned ei ole see süsteem ja turvalise kanali, nt transpordikihi Turve (HTTPS) vedamine peab. Kui esitaja luba edastatakse selge, on mees-keskmisel rünnak saab pahatahtlik isik hankida luba ja kasutage seda volitamata juurdepääsu kaitstud ressursi. Turvalisus põhimõtted rakendada, kui salvestamise või esitaja sõned hilisemaks kasutamiseks vahemällu. Alati veenduge, et teie rakendus edastab ja salvestab esitaja sõned turvaline viisil. Vaadake veel turvakaalutlused esitaja sõned, [RFC 6750 jaotise 5](http://tools.ietf.org/html/rfc6750).

Lisateavet eri tüüpi sõned v2.0 lõpp-punkti kasutatud on saadaval [v2.0 lõpp-punkti Turbeloa viide](active-directory-v2-tokens.md).

## <a name="protocols"></a>Protokollid

Kui olete valmis mõni näide taotluste vaatamiseks, alustamine ühte selle all õpetused.  Iga üks vastab kindla autentimise stsenaariumi.  Kui vajate abi otsustamisel, mis on teie jaoks õige voogu, vaadake [selle v2.0 abil saate koostada rakenduste tüübid](active-directory-v2-flows.md).

- [Koostada Mobile ja kohalikus rakenduses OAuthi 2.0](active-directory-v2-protocols-oauth-code.md)
- [Koostada Web Apps avatud ID-ga ühenduse loomine](active-directory-v2-protocols-oidc.md)
- [Ühelt leheküljelt rakenduste sammu pidamas OAuthi 2.0 peidetud koostamine](active-directory-v2-protocols-implicit.md)
- [Koosta deemonid või serveri pool protsesside identimisteabega OAuthi 2.0 kliendi Flow](active-directory-v2-protocols-oauth-client-creds.md)
- Saada sõned Web API abil soovitud OAuthi 2.0 kellegi nimel Flow (tulekul)

<!-- - Get tokens using a username & password with the OAuth 2.0 Resource Owner Password Credentials Flow (coming soon) --> 
