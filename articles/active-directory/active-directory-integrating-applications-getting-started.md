<properties
   pageTitle="Saada rakenduste integreerimine Azure Active Directory Alustamisjuhend |  Microsoft Azure'i"
   description="Selles artiklis on saamisega alustamise juhend integreerimine Azure Active Directory (AD) rakendused kohapealse ja pilveteenuse rakendusi."
   services="active-directory"
   documentationCenter=""
   authors="ihenkel"
   manager="femila"
   editor=""/>

   <tags
      ms.service="active-directory"
      ms.devlang="na"
      ms.topic="article"
      ms.tgt_pltfrm="na"
      ms.workload="identity"
      ms.date="02/09/2016"
      ms.author="inhenk"/>

# <a name="integrating-azure-active-directory-with-applications-getting-started-guide"></a>Saada rakenduste integreerimine Azure Active Directory Alustamisjuhend
## <a name="overview"></a>Ülevaade
Selles teemas on mõeldud teile juhised integreerimiseks rakenduste abil Azure Active Directory (AD). Iga allpool sisaldab täpsemat teema lühikokkuvõtet, nii et saate tuvastada, milliseid osasid saamisega alustamine on teie jaoks oluline.  Linkide kaudu põhjalikult Dive iga teema kohta.

## <a name="before-you-begin-take-inventory"></a>Enne alustamist, teha
Enne hüpata rakenduste integreerimine Azure AD, on oluline teada, kus olete ja kuhu soovite minna.  Järgmised on mõeldud aitama teil mõtlema, milliseid rakenduse integreerimine Azure AD projekti.

### <a name="application-inventory"></a>Rakenduse varude
- Kus on kõik rakenduste? Kellele neid?
- Millist liiki autentimine rakenduste ei vaja?
- Kellel on vaja juurdepääsu taotluste?
- Kas soovite luua uue rakenduse juurutamine?
  - Kas ehitada ettevõttesiseseid ja juurutada Azure Arvuta eksemplar?
  - Kas kavatsete kasutada ühte, mis on saadaval Azure rakenduse Galerii

### <a name="user-and-group-inventory"></a>Kasutaja- ja varude
- Kus asuvad teie Kasutajakontod?
 - Kohapealse Active Directory
 - Azure AD
 - Eraldi rakenduses andmebaasis, mis kuulub teile
 - Ülikooli rakendustes
 - Kõik ülaltoodud
- Millised õigused ja rollimääranguid kas üksikkasutajate praegu on? Vaja üle vaadata oma Accessi või olete kindel, et kasutaja juurdepääs ja rolli ülesanded on nüüd sobiv?
- On rühmad juba loodud oma kohapealse Active Directory?
 - Kuidas oma rühma korraldada?
 - Kes on rühma liikmed?
 - Millised õigused/rollimääranguid kas rühmad praegu on?
- Kas peate puhastamiseks kasutaja/rühma andmebaaside enne integreerimise?  (See on päris oluline küsimus. Prügi, prügi välja.)

### <a name="access-management-inventory"></a>Laoseisu juurdepääsu haldamine
- Kuidas te praegu hallata kasutajate juurdepääsu taotluste? Mida ei vaja muuta?  Pidanud muul viisil juurdepääsu haldamiseks, näiteks koos [RBAC](role-based-access-control-configure.md) näiteks?
- Kellel on vaja juurde pääseb?

Võib-olla ei ole kõigile neile küsimustele vastused ette, kuid see on okay.  Sellest juhendist saate osa neile küsimustele vastata ja mõned kursis püsida otsuseid langetada.

## <a name="prerequisites"></a>Eeltingimused
- Azure'i tellimuse ja on Azure Active Directory kataloogis.  Kui teil pole veel Azure tellimuse, saate proovida Azure'i tasuta jaoks 30 päeva. [Proovige järele!](https://azure.microsoft.com/trial/get-started-active-directory/)

## <a name="application-integration-with-azure-ad"></a>Rakenduse integreerimine Azure AD
### <a name="finding-unsanctioned-cloud-applications-with-cloud-app-discovery"></a>Pilveteenuse rakenduse Discovery leidmine Ülikooli cloud rakenduste
Nagu eespool, võib olla rakendusi, mis pole haldab teie asutuses kuni praeguseni.  Laoseisu protsessi osana, on võimalik leida Ülikooli pilve rakendused. Vt [otsimise Ülikooli cloud rakenduste pilveteenuse rakenduse Discovery](active-directory-cloudappdiscovery-whatis.md).

### <a name="authentication-types"></a>Autentimise tüübid
Iga rakenduste võib-olla erinevad autentimisnõuded. Azure AD, sisselogimise serdid saab kasutada rakendusi, mis kasutavad SAML 2.0, oli Federation, või OpenID ühenduse Protokollid samuti parooli ühekordse sisselogimise. Rakenduse kohta lisateabe saamiseks vt autentimistüüpidest kasutamiseks koos Azure AD [Jaoks ühendatud ühekordse sisselogimise Azure Active Directory haldamine serdid](active-directory-sso-certs.md) ja [parooli põhjal ühekordse sisselogimise](active-directory-appssoaccess-whatis.md).

### <a name="enabling-sso-with-azure-ad-app-proxy"></a>Azure'i AD Rakenduse puhverserveri koos SSO lubamine
Microsoft Azure'i AD rakenduse puhverserver, kus saate sisestada pakendi sees oma privaatvõrgu turvaliselt, igal pool ja igas seadmes rakendustele juurdepääsemiseks. Kui olete installinud mõne rakenduse puhverserveri konnektor teie keskkonnas, seda saab hõlpsasti konfigureerida Azure AD.

### <a name="integrating-applications-with-azure-ad"></a>Rakenduste integreerimine Azure AD
Järgmised artiklid arutada võimalust rakenduste integreerimine Azure AD ja anda juhiseid.

- [Kindlaks teha, milline Active Directory kasutamiseks](active-directory-administer.md)
- [Azure'i rakenduse Galerii rakenduste kasutamine](active-directory-appssoaccess-whatis.md)
- [Integreerimine SaaS rakenduste õpetused loend](active-directory-saas-tutorial-list.md)

## <a name="managing-access-to-applications"></a>Rakenduste juurdepääsu haldamine
Järgmistes artiklites on kirjeldatud viisil, kui neil on integreeritud Azure AD ja Azure AD Azure AD konnektorite abil saate hallata rakendustele juurdepääsemiseks.

- [Rakenduste Azure AD kaudu juurdepääsu haldamine](active-directory-managing-access-to-apps.md)
- [Toimingute automatiseerimine Azure AD konnektorite abil](active-directory-saas-app-provisioning.md)
- [Rakenduse kasutajate määramine](active-directory-applications-guiding-developers-assigning-users.md)
- [Rakenduse rühmade määramine](active-directory-applications-guiding-developers-assigning-groups.md)
- [Ühiskasutuse kontod](active-directory-sharing-accounts.md)

## <a name="integrating-custom-applications"></a>Integreerimise kohandatud rakendused.
Kui kirjutate mõne uue rakenduse ja soovite aidata arendajate kasutamine power Azure AD, lugege teemat [Guiding arendajatele](active-directory-applications-guiding-developers-for-lob-applications.md).

Kui soovite lisada kohandatud rakenduse Azure'i rakenduste galeriisse, lugege teemat ["Tuua oma rakenduse" Azure AD iseteenindusliku SAML konfiguratsioon](http://blogs.technet.com/b/ad/archive/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-gt-now-in-preview.aspx).

## <a name="see-also"></a>Vt ka

- [Artikli registri Rakendusehaldus Azure Active Directory 's](active-directory-apps-index.md)
