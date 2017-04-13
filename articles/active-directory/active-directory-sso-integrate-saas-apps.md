<properties
    pageTitle="Integreerimine Azure Active Directory ühekordse sisselogimise SaaS rakendused |  Microsoft Azure'i"
    description="Lubada ühekordse sisselogimise autentimise ja kasutajale ettevalmistamise kesksetest Accessi SaaS rakenduste Azure Active Directory haldamine. Kuidas integreerida SaaS rakenduste Azure Active Directory ülevaade."
    services="active-directory"
      keywords="integreerimine Azure AD SaaS rakendused"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="09/30/2016"
    ms.author="curtand"/>

# <a name="integrate-azure-active-directory-single-sign-on-with-saas-apps"></a>Integreerimine Azure Active Directory ühekordse sisselogimise SaaS rakendused  

> [AZURE.SELECTOR]
- [Azure'i portaal](active-directory-enterprise-apps-manage-sso.md)
- [Azure'i klassikaline portaal](active-directory-sso-integrate-saas-apps.md)

[AZURE.INCLUDE [active-directory-sso-use-case-intro](../../includes/active-directory-sso-use-case-intro.md)]

Ühekordse sisselogimise rakendus, mida te kasutate viimine ettevõtte jaoks häälestamise alustamiseks, kasutate olemasoleva kausta Azure Active Directory (Azure AD). Saate kasutada Azure AD kataloogi, mis teil saada Microsoft Azure'i, Office 365 või Windows Intune kaudu. Kui teil on kaks või rohkem neist, lugege teemat [Azure AD kataloogi haldamine](active-directory-administer.md) määratlemiseks kumba kasutada.

## <a name="authentication"></a>Autentimine

Rakenduste puhul, mis toetavad SAML 2.0 oli Federation, või OpenID Connect protokollid, Azure Active Directory kasutab sisselogimise serdid usalda seoste loomiseks. Selle kohta leiate lisateavet teemast [haldamine i ühendatud ühekordse sisselogimise serdid](active-directory-sso-certs.md).

Rakendusi, mis toetavad ainult HTML-i vormipõhist sisse logida, kasutab Azure Active Directory 'parooli Holvi' usalda seoseid luua. See võimaldab kasutajatel teie asutuses on automaatselt sisse logida SaaS rakenduse abil kasutaja konto andmed rakendusest SaaS Azure AD. Azure AD kogub ja talletab turvaliselt kasutaja konto andmed ja seotud parool. Lisateavet leiate teemast [parooli põhinev ühekordse sisselogimise](active-directory-appssoaccess-whatis.md#password-based-single-sign-on).

## <a name="authorization"></a>Luba

Ettevalmistatud konto võimaldab kasutajatel lubada kasutada rakenduse, kui need on autenditud ühekordse sisselogimise kaudu. Kasutaja ettevalmistamise saab teha käsitsi või mõnel juhul saate lisada ja kasutaja teabe põhjal Azure Active Directory tehtud muudatused SaaS rakendusest eemaldada. Olemasoleva Azure AD konnektorid automatiseeritud ettevalmistamise kasutamise kohta leiate lisateavet teemast [automatiseeritud kasutaja ettevalmistamise ja tühistage ettevalmistamise SaaS rakenduste](active-directory-saas-app-provisioning.md).

Muul juhul saate käsitsi lisamine rakendusse kasutajateabe, või kasutada muid ettevalmistamise lahendusi, mis on saadaval kuvatakse Marketplace'ist.

## <a name="access"></a>Juurdepääs

Azure AD pakub juurutada rakendused oma ettevõtte lõppkasutajatele kohandatavad viisidel. Teil on lukus, mis tahes kindla juurutamise või access lahendus. Saate [lahenduse, mis sobib kõige paremini teie vajadustele](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users).

## <a name="additional-considerations-for-applications-already-in-use"></a>Täiendavad kaalutlused rakenduste juba kasutusel

Häälestamise ühekordse sisselogimise jaoks rakendus, mis on juba teie asutuses on kasutusel on mõni muu protsess loomisprotsessi uued kontod mõne uue rakenduse kaudu. On paar esialgse juhised, sealhulgas: vastendamine kasutaja identiteedid rakenduse Azure AD identiteetidega ja mõistmine, kuidas kasutajatele, kellel pärast seda, kui see on integreeritud rakenduse sisselogimine.

> [AZURE.NOTE] Olemasoleva rakenduse SSO seadistamiseks peab teil olema nii Azure AD üldadministraatori õigused ja SaaS rakendus.

### <a name="mapping-user-accounts"></a>Kasutajakontode kaardistamine

Kasutaja identiteedi tavaliselt on kordumatu ID, mis võib olla e-posti aadress või kasutaja turvasubjektinimi (UPN). Kas soovite lingi (kaarti) iga kasutaja rakenduse identiteedi vastavaid Azure AD identiteedi. On paar võimalust, olenevalt sellest, kuidas täita seda oma rakenduse autentimise nõue.

Rakenduse identiteedid Azure AD identiteetidega vastendamise kohta leiate lisateavet teemast [määramine taotluste SAML luba välja](http://social.technet.microsoft.com/wiki/contents/articles/31257.azure-active-directory-customizing-claims-issued-in-the-saml-token-for-pre-integrated-apps.aspx) ja [määramine atribuudi vastendused ettevalmistamine](active-directory-saas-customizing-attribute-mappings.md).

### <a name="understanding-the-users-log-in-experience"></a>Kasutaja sisselogimine kogemus mõistmine

Kui te integreerida SSO rakendus, mis on juba kasutusel, on oluline mõista kasutusvõimalused mõjutab. Kõigi rakenduste, kasutajad hakkavad Azure AD mandaadi abil sisse logida. Samuti võib olla, et need tuleb kasutada erinevaid portaali rakenduse avamiseks.

Mõne rakenduse SSO saab teha rakenduse Logi kasutajaliidese, kuid muudes rakendustes, kasutaja on läbi keskse portaali, nt ([Minu rakendused](http://myapps.microsoft.com) või [Office365](http://portal.office.com/myapps)) sisse logida. Lisateavet eri tüüpi SSO-d ja nende kasutajate kogemusi, [mis](active-directory-appssoaccess-whatis.md)on rakenduse access ja ühekordse sisselogimise Azure Active Directory.

Mõne muu väärtuslik on *Suppressing kasutaja nõusoleku* [Guiding arendajate](active-directory-applications-guiding-developers-for-lob-applications.md) artiklis.

## <a name="next-steps"></a>Järgmised sammud


SaaS rakenduste leiate rakenduse Galerii Azure AD pakub mitmesuguseid [õpetused kohta, kuidas integreerida SaaS rakendused](active-directory-saas-tutorial-list.md).

Kui rakendus pole rakenduse Galerii, saate [selle kohandatud rakendus Azure AD Rakenduse galeriisse lisada](http://blogs.technet.com/b/ad/archive/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-gt-now-in-preview.aspx).

On palju üksikasjalikumalt kõigi järgmiste probleemide korral Azure.com teegis, alustades [mis on rakenduse access ja ühekordse sisselogimise Azure Active Directory.](active-directory-appssoaccess-whatis.md).

## <a name="see-also"></a>Vt ka

- [Artikli registri Rakendusehaldus Azure Active Directory 's](active-directory-apps-index.md)
