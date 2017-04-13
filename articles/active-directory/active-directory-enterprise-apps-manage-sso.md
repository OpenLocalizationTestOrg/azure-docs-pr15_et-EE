<properties
    pageTitle="Ühe sisselogimise halduse enterprise rakenduste Azure Active Directory eelvaates | Microsoft Azure'i"
    description="Saate teada, kuidas hallata ühekordse sisselogimise enterprise rakenduste Azure Active Directory abil"
    services="active-directory"
    documentationCenter=""
    authors="asmalser"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="09/30/2016"
    ms.author="asmalser"/>

# <a name="preview-managing-single-sign-on-for-enterprise-apps-in-the-new-azure-portal"></a>Eelvaade: Ühekordse sisselogimise uue Azure'i portaalis enterprise rakenduste haldamine

> [AZURE.SELECTOR]
- [Azure'i portaal](active-directory-enterprise-apps-manage-sso.md)
- [Azure'i klassikaline portaal](active-directory-sso-integrate-saas-apps.md)

Selles artiklis kirjeldatakse, kuidas kasutada [Azure portaali](https://portal.azure.com) saate hallata ühekordse sisselogimise sätteid rakendused, eriti need, mis on lisatud [Azure Active Directory (Azure AD) rakenduse Galerii](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery). Ühekordse sisselogimise teenuse Azure AD haldamine on praegu avaliku eelvaate ja selles artiklis kirjeldatakse uusi funktsioone kui ka mõned ajutised piirangud, mis luuakse ainult jooksul eelvaade. [Mis on eelvaates?](active-directory-preview-explainer.md)

##<a name="finding-your-apps-in-the-new-portal"></a>Uue portaali rakenduste leidmine

Mai 2016 seisuga kõik rakendused, mis on konfigureeritud ühe sisselogimise Directorys directory administraator [Azure Active Directory rakenduse Galerii](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery) [Azure klassikaline portaalis](https://manage.windowsazure.com)sees, saab nüüd vaadata ja hallatavate Azure'i portaalis.

**Ettevõtte rakenduste** jaotises Azure portaali, link, mille leiate [portaali](https://portal.azure.com)loendis **Rohkem teenuseid** leiate need rakendused. Ettevõtte rakendustel on kasutusele võetud ja kasutavad kasutajad oma ettevõttes.

![Ettevõtte rakenduste blade][1]

Valige **Kõik rakendused** konfigureeritud, sh rakendusi, mis oli lisatud galeriist kõigi rakenduste loendi kuvamiseks. Valige rakendus laadib ressursi tera rakenduse, kus aruandeid saab vaadata rakenduse ja erinevaid sätteid saab hallata.

Ühekordse sisselogimise sätete haldamiseks valige **ühekordse sisselogimise**.

![Rakenduse ressursi blade][2]


##<a name="single-sign-on-modes"></a>Ühekordse sisselogimise režiimi

**Ühekordse sisselogimise** tera algab **režiimi** menüü, mis võimaldab ühekordse sisselogimise režiimi konfigureerida. Saadaolevad suvandid on järgmised.

* **SAML-põhiste Logi sisse** – see suvand on saadaval, kui rakendus toetab täieliku välise ühekordse sisselogimise Azure Active Directory abil SAML 2.0 Protocol (protokoll).

* **Parooli põhinev Logi sisse** – see suvand on saadaval, kui Azure AD toetab parooli vormi täitmine selle rakenduse jaoks.

* **Lingitud sisse logida** - varem "Olemasolev ühekordse sisselogimise", see suvand saavad administraatorid paigutada selle rakenduse lingi oma kasutaja Azure AD kasutada paneeli või Office 365 rakenduse käiviti.

Režiimidest kohta leiate lisateavet teemast [Kuidas ühekordse sisselogimise Azure Active Directory töötamise](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work).


##<a name="saml-based-sign-on"></a>SAML-põhiste sisselogimise

**SAML-põhiste sisselogimise** suvand kuvatakse tera, mis on jagatud nelja jaotised.

###<a name="domains-and-urls"></a>Domeenide ja URL-id
See on, kus kõik rakenduse domeeni ja URL-ide üksikasjad lisatakse Azure AD kataloogi. Ühekordse sisselogimise töö rakenduse tegema kõiki sisendeid kuvatakse otse ekraanile, tuleks kõik valikuline sisendina saab vaadata, valides märkeruut **Kuva täpsemad URL-i sätted** . Toetatud sisendeid täielik loend sisaldab:

* **Logige sisse URL-i** – kui kasutaja läheb selle rakenduse sisse logima. Kui rakendus on konfigureeritud täita teenuse pakkuja algatatud ühekordse sisselogimise kohta, siis kui kasutaja viib seda URL-i, ei teenusepakkuja vajalikud ümbersuunamise Azure AD autentimiseks ja kasutajale sisse logida. Kui väli on täidetud, siis Azure AD kasutab seda URL-i käivitage rakendus Office 365 ja Azure AD Accessi juhtpaneeli kaudu. Kui väli on välja jäetud, siis Azure AD hoopis sooritab identiteedipakkuja-algatatud Logi sisse, kui käivitatakse rakenduse Office 365, Azure AD Accessi paneeli või Azure AD ühe sisselogimise URL-i.

* **Identifikaator** – see URI peaks kordumatult tuvastada rakenduse jaoks sisse logida mis ühe konfigureeritakse. See on väärtus, mis Azure AD saadab tagasi parameetrina sihtrühma kohaldamine SAML luba ja peaks selle kinnitamiseks. Üksuse ID, mis tahes SAML metaandmete rakenduse kuvatakse ka selle väärtuse.

* **Vasta URL** - vastus URL-i on kui rakenduse ootab SAML luba. See on ka edaspidi kinnituse tarbija teenus (ACS) URL-i. Kui need on sisestanud, klõpsake nuppu Edasi jätkamiseks järgmisel kuval. Kuva teave, mida on vaja konfigureerida rakendus küljel lubamiseks aktsepteerimiseks SAML luba: Azure'i AD.

* **Edastamine riigi** - relay olek on valikuline parameeter, mis aitavad öelda rakendus, kust kasutaja ümbersuunamiseks pärast autentimist on lõpule viidud. Tavaliselt väärtus on kehtiv URL-i taotluse, aga mõned rakendused kasutada väli teisiti (vt rakenduse ühekordse sisselogimise dokumentatsioonist). Võimalus määrata relay olek on uus funktsioon, mis on kordumatu uue Azure portaali.

###<a name="user-attributes"></a>Kasutaja atribuutide
See on, kus administraatorid saavad vaadata ja redigeerida atribuute, mis on saadetud SAML luba, et Azure AD küsimused rakendusse iga aja kasutajad sisse logida.

Eelvaate esmaväljalaske jaoks on toetatud ainult redigeeritavad atribuuti **Kasutaja identifikaator** atribuut. Atribuudi väärtus on välja Azure AD, mis tuvastab kordumatult iga kasutaja rakendusest. Näiteks kui rakendus on juurutatud, kasutades oma kasutajanime ja kordumatut tunnust "e-posti aadress", siis väärtus peaks olema seatud "user.mail" välja Azure AD.

Täiendavate atribuutide redigeerimine toetatakse edaspidised eelvaade.

###<a name="saml-signing-certificate"></a>SAML allkirjasert
Selles jaotises kirjeldatakse logida SAML sõned iga kord, kui kasutaja autendib rakenduse väljastanud kasutava Azure AD serdi üksikasjad. See võimaldab kontrollida praeguse serdi atribuutide, sh aegumiskuupäeva.

Võimalus edastamiseks serti ja täiendavate suvandite toetavad edaspidised eelvaateversiooniga serdi haldamine. Pange tähele, et sertifikaatide täielik haldamine saab teha endiselt [Azure'i klassikaline portaali](active-directory-sso-certs.md).

###<a name="application-configuration"></a>Rakenduse konfigureerimine
Lõplik jaotises antakse, dokumente ja/või juhtelementide konfigureerida rakenduse kasutamiseks Azure Active Directory identiteedi pakkuja nõutav.

**Rakenduse konfigureerimine** hüpikakna menüü annab uue kompaktsemaks, manustatud juhiseid rakenduse konfigureerimine. See on veel üks uus funktsioon uue Azure portaali kordumatu.

> [AZURE.NOTE] Täieliku manustatud dokumentatsiooni näiteks vt Salesforce.com rakendus. Täiendavad rakendused dokumentatsioonist pidevalt lisatakse eelvaates.

![Manustatud dokumendid][3]

##<a name="password-based-sign-on"></a>Parooli vastavalt sisselogimise
Kui rakenduse toetatud, valides parooli põhineva SSO režiimi ja käsku **Salvesta** kohe konfigureerib seda teha parooli põhinev SSO. Parooli põhinev SSO juurutamine kohta leiate lisateavet teemast [Kuidas ühekordse sisselogimise Azure Active Directory töötamise](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work).

![Parooli vastavalt sisselogimise][4]


##<a name="linked-sign-on"></a>Lingitud sisselogimise
Kui rakenduse toetatud, valides lingitud SSO režiimi võimaldab teil sisestada URL, mida soovite ümber, kui kasutaja klõpsab selle rakenduse Azure AD kasutada paneeli või Office 365. Lingitud SSO-d (varasema nimega "olemasoleva SSO") kohta leiate lisateavet teemast [Kuidas ühekordse sisselogimise Azure Active Directory töötamise](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work).

![Lingitud sisselogimine][5]

[1]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-blade.PNG
[2]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-sso-blade.PNG
[3]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-blade-embedded-docs.PNG
[4]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-blade-password-sso.PNG
[5]: ./media/active-directory-enterprise-apps-manage-sso/enterprise-apps-blade-linked-sso.PNG
