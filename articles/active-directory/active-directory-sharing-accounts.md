<properties
    pageTitle="Konto abil Azure AD |  Microsoft Azure'i"
    description="Kirjeldab, kuidas Azure Active Directory võimaldab asutustel turvaliselt jagada kohapealse rakendused ja tarbija pilveteenustega kontod."
    services="active-directory"
    documentationCenter=""
    authors="msStevenPo"
    manager="femila"
    editor=""/>

 <tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/09/2016"  
    ms.author="stevenpo"/>

# <a name="sharing-accounts-with-azure-ad"></a>Azure AD kontod ühiskasutusse

## <a name="overview"></a>Ülevaade
Mõnikord peavad organisatsioonid kasutamiseks mitu inimest ühe kasutajanime ja parooliga. See juhtub tavaliselt kaks juhtudel:

- Rakendusi, mis nõuavad iga kasutaja jaoks kordumatu kasutajanime ja parooli kasutamisel kas kohapealse rakendused või tarbija cloud services (nt ettevõtte sotsiaalmeedia kontod).
- Kui loote mitme kasutaja keskkonnas. Peate ühe, kohaliku konto, mis on laiendatud õigusi ja saab kasutada core (nt kohaliku "globaalne administraator" konto Office 365 jaoks) või Salesforce'i root konto häälestamise, haldamine ja taastamine tegevusi.

Nende kontodega oleks traditsiooniliselt ühiskasutusse antud levitamise identimisteabe (kasutajanime ja parooli) paremas isikutele või nende salvestamise ühiskasutatavasse asukohta, kus mitme usaldusväärse agentide saate neile juurde pääseda.

Traditsiooniline ühiskasutuse mudel sisaldab mitmeid puudusi.

- Uute rakenduste juurdepääsu lubamine saate levitada kõigile mandaat, millele juurdepääsu tuleks nõuab.
- Iga ühiskasutusse antud rakendusest võib olla vaja oma kordumatu mandaadikomplekt ühiskasutuses, mitmele kriteeriumikogumile identimisteabe pidage meeles, et kasutajad. Kui kasutajad on palju identimisteabe meeles pidada, risk suureneb hakkab kasutama riskantne tavad. (nt kirjutamine alla paroolid).
- Te ei saa öelda, kellel on juurdepääs rakenduse.
- Te ei saa öelda, kellel on *juurdepääs* rakenduse.
- Kui soovite eemaldada Accessi rakendus, peate kasutajaandmed ja neid uuesti jagada kõigile, mis tuleb selle rakenduse access.

## <a name="azure-active-directory-account-sharing"></a>Azure Active Directory konto jagamine

Azure AD annab uue lähenemisviisi kaudu ühiskasutusse antud kontod, mis kõrvaldab need vead.

Azure AD administraator konfigureerib, millised rakendused kasutaja pääseb juurde Accessi paneeli abil ja valides tüüpi ühekordse sisselogimise kõige paremini sobib see rakendus. Üks neist tüübid, *parooli-põhiste ühekordse sisselogimise kohta*, võimaldab Azure AD toimivad omamoodi "ta" käigus sisselogimise rakenduse.

Kasutajate logige sisse oma ettevõtte konto üks kord. See on sama kontoga, mida nad kasutavad regulaarselt laua- või e-posti juurdepääsu. Need leida ja ainult need määratud rakenduste juurde. Ühiskasutusega kontode, seda rakenduste loendit võib sisaldada mis tahes arv jagatud identimisteabe. Lõppkasutaja ei pea meeles pidada või kirjutage erinevate kontode nad võivad kasutada.

Ühiskasutusega kontod mitte ainult suurendada haldusrühmadest ja parandada kasutatavuse, samuti suurendavad turvalisuse. Mandaadi kasutamise õigustega kasutajad ei näe ühiskasutusega parooli, kuid pigem saada õigused kasutada parool on juhitud autentimise meilivoo osana. Lisaks teatud parooli SSO rakenduste puhul on teil võimalus on Azure AD perioodiliselt edastamiseks (Värskenda) parooli abil suur, keerukate paroolide, suurendades konto turvalisus. Administraator saate hõlpsasti anda või tühistada juurdepääsu rakenduse ja ka teada, kellel on juurdepääs kontole ja kes seda varem juurde.

Azure AD toetab ühiskasutusega kontode jaoks mis tahes ettevõtte mobiilsus komplekti (EMS), Premium või litsentsitud kasutajad, üle ühe parooli igat tüüpi rakendusi sisse logida. Saate ühiskasutada kontosid mis tahes tuhandeliste eelnevalt integreeritud rakenduste rakenduse Galerii ja saate lisada oma parooli autentimisel rakenduse [kohandatud SSO rakendused](active-directory-sso-integrate-saas-apps.md).

Azure'i AD-funktsioone, mis konto ühiskasutuse lubamine on järgmised.

- [Parooli Ühekordne sisselogimine](active-directory-appssoaccess-whatis.md#password-based-single-sign-on)
- Parooli ühekordse sisselogimise agent
- [Rühmakuuluvus](active-directory-accessmanagement-self-service-group-management.md)
- Kohandatud parooli rakendused
- [Armatuurlaua/kasutusaruannete rakendus](active-directory-passwords-get-insights.md)
- Lõppkasutaja juurdepääsu portaalide
- [Rakenduse puhverserver](active-directory-application-proxy-get-started.md)
- [Active Directory turuplats](https://azure.microsoft.com/marketplace/active-directory/all/)

## <a name="sharing-an-account"></a>Jagamine konto
Ühiskasutus peate konto Azure AD abil:

- Rakenduse [rakenduste galeriisse](https://azure.microsoft.com/marketplace/active-directory/) või [kohandatud rakenduse](http://blogs.technet.com/b/ad/archive/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-gt-now-in-preview.aspx) lisamine
- Konfigureerige rakenduse parooli ühekordse sisselogimise (SSO)
- Kasutage [ülesande põhjal](active-directory-accessmanagement-group-saasapps.md) ja valige suvand ühiskasutusega mandaadi sisestamiseks
- Valikuline: mõned rakendused, nt Facebooki, Twitteri või LinkedIni, saate lubada suvandi [Azure AD automatiseeritud parooli pikendamine](http://blogs.technet.com/b/ad/archive/2015/02/20/azure-ad-automated-password-roll-over-for-facebook-twitter-and-linkedin-now-in-preview.aspx)

Saate ka teie ühiskasutusse antud konto turvalisemaks mitme teguriga autentimine (MFA) (lugege lisateavet [turvamise rakenduste Azure AD](../multi-factor-authentication/multi-factor-authentication-get-started.md)) ja saate delegeerida võimalus haldamine, kellel on juurdepääs rakenduse abil [Azure AD iseteenindusliku](active-directory-accessmanagement-self-service-group-management.md) rühma haldamine.

## <a name="related-articles"></a>Seotud artiklid

- [Artikli registri Rakendusehaldus Azure Active Directory 's](active-directory-apps-index.md)
- [Tingimusvormingu Accessi rakenduste kaitsmine](active-directory-conditional-access.md)
- [Iseteeninduslik rühma haldamine/SSAA](active-directory-accessmanagement-self-service-group-management.md)
