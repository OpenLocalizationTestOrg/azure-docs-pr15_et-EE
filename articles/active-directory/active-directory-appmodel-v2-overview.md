<properties
    pageTitle="lõpp-punkti ülevaade v2.0 | Microsoft Azure'i"
    description="Rakenduste abil nii Microsofti Account ja Azure Active Directory sisselogimist building tutvustus."
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
    ms.date="09/27/2016"
    ms.author="dastrock"/>

# <a name="sign-in-microsoft-account--azure-ad-users-in-a-single-app"></a>Sisselogimine Microsofti Account ja Azure AD kasutajate ühest rakendusest

Varem rakenduse arendaja, kes toetavad nii Microsofti kontod ja Azure Active Directory vajalikud kahte eraldi süsteemide integreerimine.  Nüüd olete me kasutusele uus autentimise API versioon, mis võimaldab kasutajatel mõlemat tüüpi kontode Azure AD süsteemi abil sisse logida.  Ühendatud autentimise süsteemi tuntakse **v2.0 lõpp-punkti**.  Koos v2.0 lõpp-punkti ühe lihtsa integreerimine võimaldab teil jõuda publikule, mis ulatub üle miljonite kasutajate isiklikud ja töö/kool.

Rakendused, mis kasutavad v2.0 lõpp-punkti saab kasutada ka [Microsoft Graphi](https://graph.microsoft.io) ja [Office 365](https://msdn.microsoft.com/office/office365/howto/authenticate-Office-365-APIs-using-v2) konto tüüpi abil REST API-d.

<!-- For a quick introduction to the v2.0 endpoint, please view the [Getting Started with Microsoft Identities: Enterprise Grade Sign In For Your Apps](https://azure.microsoft.com/documentation/videos/build-2016-getting-started-with-microsoft-identities-enterprise-grade-sign-in-for-your-apps/) video. -->

## <a name="getting-started"></a>Alustamine
[AZURE.VIDEO build-2016-getting-started-with-microsoft-identities-enterprise-grade-sign-in-for-your-apps]

Valige järgmisest loendist rakendus meie avatud allika teekide ja raamistiku abil luua oma lemmik platvormi.  Teise võimalusena saate OAuth 2.0 ja OpenID Connect protokolli dokumentatsioon saata ja Protocol (protokoll) sõnumeid otse soovitud auth teegi kasutamata.

<!-- TODO: Finalize this table  -->
[AZURE.INCLUDE [active-directory-v2-quickstart-table](../../includes/active-directory-v2-quickstart-table.md)]

## <a name="whats-new"></a>Mis on uut
Põhiteave siin on kasulik mõista, mis on ja mida ei ole võimalik v2.0 lõpp-punkti.

- Lisateavet [tüüpi rakendusi saate koostada koos v2.0 lõpp-punkti](active-directory-v2-flows.md).
- Mõistma [piirangud ja piirangutega, piiranguid](active-directory-v2-limitations.md) koos v2.0 lõpp-punkti.
- Hiljuti lisasime tugi [administraator piiratud otsinguulatuste](active-directory-v2-scopes.md) ja [OAuth2 kliendi identimisteabe anda](active-directory-v2-protocols-oauth-client-creds.md).  Proovige neid!

## <a name="reference"></a>Viide
Neid linke oleks kasulik uurida süvitsi platvorm:

- Koosta 2016: [Alustamine Microsoft identiteedid: Enterprise hinde logige sisse oma rakenduste](https://azure.microsoft.com/documentation/videos/build-2016-getting-started-with-microsoft-identities-enterprise-grade-sign-in-for-your-apps/)
- Hankida abi virnas ületäitumise [Azure'i – active directory](http://stackoverflow.com/questions/tagged/azure-active-directory) abil või [adal](http://stackoverflow.com/questions/tagged/adal) sildid.
- [v2.0 viide Protocol (protokoll)](active-directory-v2-protocols.md)
- [v2.0 Turbeloa viide](active-directory-v2-tokens.md)
- [v2.0 teegi teatmematerjalid](active-directory-v2-libraries.md)
- [Otsinguulatuste ja nõusoleku v2.0 lõpp-punkti](active-directory-v2-scopes.md)
- [Microsofti rakenduse registreerimine portaalis](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)
- [Office 365 REST API viide](https://msdn.microsoft.com/office/office365/howto/authenticate-Office-365-APIs-using-v2)
- [Microsoft Graphi](https://graph.microsoft.io)