<properties
    pageTitle="Azure Active Directory B2C | Microsoft Azure'i"
    description="Kuidas otse ei toeta Azure Active Directory B2C protokollide abil saate luua rakendusi."
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

# <a name="azure-ad-b2c-authentication-protocols"></a>Azure'i AD B2C: Autentimine Protokollid

Azure Active Directory (Azure AD) B2C pakub identiteedi rakenduste teenust toetada kahte valdkonna standard protokollid: OpenID Ühenda ja OAuth 2.0. Teenus on standardses, kuid mis tahes kahe rakendusi protokolli võib olla väike erinevused.  Sellest juhendist teave saab teile kasulik, kui kirjutada oma koodi saadame ja HTTP päringute töötlemise, mitte on avatud allika teegi abil. Soovitame enne saate sukelduda üksikasjad iga teatud protokolli lugeda sellelt lehelt. Kuid kui teil on juba tuttavad Azure AD B2C, saate minna otse [protokolli kiirviidete juhised](#protocols).

<!-- TODO: Need link to libraries above -->

## <a name="the-basics"></a>Põhitõed
Iga rakendus, mis kasutab Azure AD B2C peab olema registreeritud [Azure portaali](https://portal.azure.com)B2C kataloogis. Rakenduse registreerimise käigus kogub ja määrab väärtuste rakenduse.

- **Rakenduse ID** , mis tuvastab kordumatult rakenduse.
- **Suunake URI** või saab kasutada vastuste tagasi rakendusse otsesed **paketi identifikaator** .
- Mõne muu stsenaarium kohased väärtused. Lisateavet, saate teada, [Kuidas registreerida rakenduse](active-directory-b2c-app-registration.md).

Pärast registreerimist rakenduse, see suhtleb Azure AD päringu saatmine v2.0 lõpp-punkt:

```
https://login.microsoftonline.com/common/oauth2/v2.0/authorize
https://login.microsoftonline.com/common/oauth2/v2.0/token
```

Peaaegu kõik OAuthi ja OpenID ühendamine puhul on kaasatud exchange neljal:

![OAuthi 2.0 rollid](./media/active-directory-b2c-reference-protocols/protocols_roles.png)

- **Luba server** on Azure AD v2.0 lõpp-punkti. Turvaline käsitlemisel midagi seotud kasutajateave ja juurdepääs. Käsitlemisel ka usalda vool vahel seoseid. See on kasutaja identiteedi, andmise ja tühistamise ressurssidele ja väljasta lube. See on ka identiteedipakkuja juures.
- **Ressursi omanik** on tavaliselt lõppkasutaja. See on pool, mis kuulub andmed ja see on õigus lubada muude tootjate juurdepääsu andmed või ressurss.
- **OAuthi kliendi** on teie rakendus. See on tähistatud rakenduse ID. See on tavaliselt poole, et lõppkasutajad suhelda. Samuti nõuab sõned autoriseerimine serverist. Ressursi omanik peab andma kliendi ressursi juurdepääsuõigust.
- **Ressursi server** on ressurss või andmete asukoht. Ta loodab turvaliseks autentimiseks ja lubada OAuthi kliendi autoriseerimine server. Samuti kasutab esitaja Accessi sõned tagamaks, et saate anda juurdepääsu ressursile.

## <a name="policies"></a>Poliitika
Väidetavalt Azure AD B2C poliitika on kõige olulisemad funktsioonid teenus. Azure'i AD B2C laiendab standard OAuth 2.0 ja OpenID ühendamine Protokollid poliitika kasutusele. Need võimaldavad Azure AD B2C teha enamat kui lihtne autentimise ja luba. Poliitika täielikult kirjeldada tarbija identiteedi kogemusi, sh sisse logida, sisselogimise ja profiili redigeerimine. Poliitikate saate määratleda mõne haldus UI. Ta saab teostada teisiti Päringuparameetri HTTP-taotluste autentimise abil. Poliitika ei ole standardsed funktsioonid OAuth 2.0 ja OpenID Connect, võtke aega, et neid mõista. Lisateavet leiate teemast [Azure AD B2C poliitika Lühijuhend](active-directory-b2c-reference-policies.md).

## <a name="tokens"></a>Märkide
Azure'i AD B2C rakendamise OAuth 2.0 ja OpenID ühendamine kasutab ulatuslikult esitaja sõned, sh esitaja märgid, mis on esindatud JSON web sõned (JWTs). Esitaja märgiks on kerge Turbeloa, mis annab "esitaja" juurdepääs kaitstud ressursiga. Esitaja on isik, mida saab esitada luba. Azure AD peavad esmalt autentimiseks peole esitaja luba vastu võtta. Kuid nõutud toimingute tagamiseks on luba edastamine ei tehta, saab see kinni ja kasutada ootamatuid poole.

Mõned turvalisus märgid on sisseehitatud süsteem, mis takistab volitamata isikud neid kasutanud, kuid esitaja sõned ei ole see süsteem. Nende vedamine peab olema turvaline kanali, nt transpordikihi Turve (HTTPS). Kui esitaja luba edastatakse väljaspool turvalise kanali, pahatahtlik tootja abil saate mees-in-Lähis rünnaku hankida luba ja kasutage seda volitamata juurdepääsu kaitstud ressursi. Turvalisus põhimõtted rakendada, kui esitaja sõned kuvamis- või hilisemaks kasutamiseks vahemällu. Alati veenduge, et teie rakendus edastab ja salvestab esitaja sõned turvaline viisil.

Täiendavad esitaja Turbeloa turvakaalutlused, vt [RFC 6750 jaotise 5](http://tools.ietf.org/html/rfc6750).

Erinevat tüüpi märgid kasutatud Azure AD B2C täiendavad andmed on saadaval [Azure AD Turbeloa viide](active-directory-b2c-reference-tokens.md).

## <a name="protocols"></a>Protokollid

Kui olete valmis mõned näide taotlusi vaadata, saate alustada üks järgmistest õpetused. Iga üks vastab kindla autentimise stsenaariumi. Kui vajate abi, kindlaks teha, milline on teie jaoks õige, lugege teemat [tüüpi rakendusi saate koostada Azure AD B2C abil](active-directory-b2c-apps.md).

- [Koostada Mobiiliversiooni ja kohalikke rakendusi OAuth 2.0 abil](active-directory-b2c-reference-oauth-code.md)
- [Koostada veebirakenduste abil OpenID ühenduse loomine](active-directory-b2c-reference-oidc.md)
