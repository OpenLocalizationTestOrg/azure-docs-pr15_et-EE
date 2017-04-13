<properties
    pageTitle="Azure Active Directory KKK | Microsoft Azure'i"
    description="Azure Active Directory alt leiate vastused küsimustele koos juurdepääsu Azure ja Azure Active Directory, KKK parooli haldus ja rakenduse access."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/16/2016"
    ms.author="markusvi"/>

# <a name="azure-active-directory-faq"></a>Azure Active Directory KKK

Azure Active Directory on täielik identiteedi lahendusena teenuse (IDaaS), mis hõlmavad kõigis identiteedi, juurdepääsu juhtimine ja turvalisus.


Lisateavet leiate teemast [mis on Azure Active Directory?](active-directory-whatis.md).



## <a name="accessing-azure-and-azure-active-directory"></a>Juurdepääs Azure ja Azure Active Directory


**Miks saan "tellimuste ei leitud" Azure AD Azure klassikaline portaalis (https://manage.windowsazure.com) katsel?**

**A:** Juurdepääs Azure'i klassikaline portaali nõuab iga kasutaja õiguste kohta Azure tellimuse. Kui teil on Office 365 tasuline või liikuge [http://aka.MS/accessAAD](http://aka.ms/accessAAD) ühekordne aktiveerimine samm Azure AD, vastasel juhul peate täielik [Azure'i prooviversiooni](https://azure.microsoft.com/pricing/free-trial/) või tasulise tellimuse aktiveerimiseks. 

Lisateavet leiate teemast:

- [Kuidas Azure'i tellimused on seostatud Azure Active Directory](active-directory-how-subscriptions-associated-directory.md)

- [Office 365 tellimuse Azure directory haldamine](active-directory-manage-o365-subscription.md)

---

**Q: mis on seos Azure AD, Office 365 ja Azure?**

**A:** Azure Active Directory pakub levinud identiteedi ja juurdepääsu võimaluste kõik Microsoft online Services. Kas kasutate Office 365, Microsoft Azure'i, Intune või teised, kasutate juba Azure AD sisselogimise lubamine ja kõik need teenused haldamine. 

Tegelikult kõik kasutajad on lubatud, teenuse Microsoft Online Services on määratletud Kasutajakontod Azure AD ühe või mitme eksemplari. Saate lubada neid kontod tasuta Azure AD võimalusi, nt pilveteenuse rakenduse access.
 
Lisaks Azure AD maksta teenuste (nt: Azure'i AD basic, Premium, EMS, jne) täiendavad muid võrgus teenused, nt Office 365 ja Microsoft Azure'i lahendustega ettevõtte skaala haldamine ja turvalisus.


---



## <a name="getting-started-with-hybrid-azure-ad"></a>Hübriidjuurutuse Azure AD töötamise alustamine


**Q: kuidas saate oma kohapealse kataloogi Azure AD ühenduse?**

**A:** Saate luua ühenduse oma kohapealse kataloogi Azure AD **Azure'i AD-ühenduse**kaudu. 

Täpsema teabe saamiseks vt [integreerimine Azure Active Directory oma kohapealse identiteedid](active-directory-aadconnect.md).


---

**Kuidas häälestada SSO oma kohapealse kataloogi ja minu pilveteenuses rakenduste?**

**A:** Peate ainult SSO häälestada oma kohapealse kataloogi ja Azure AD vahel. Kui pilveteenuse rakenduste Azure AD kaudu juurdepääsemiseks draivid teenuse kasutajate volituste asutusesisese õigesti autentimiseks automaatselt.

Rakendamine SSO kohapealse saate hõlpsasti saavutada federation lahendusi, nt ADFS-i või parooli räsi sünkroonimise konfigureerimisega. Mõlema variandi Azure'i AD-ühenduse konfiguratsiooni viisardi abil saate hõlpsalt juurutada.
  

Täpsema teabe saamiseks vt [integreerimine Azure Active Directory oma kohapealse identiteedid](active-directory-aadconnect.md).
  

---

**Q: kas Azure Active Directory anda oma asutuse kasutajate jaoks iseteenindusliku portaali?**

**A:** Jah, Azure Active Directory pakub iseteenindusliku kasutaja ja rakenduse access [Azure AD Accessi paani](http://myapps.microsoft.com) . Kui olete Office 365 klient, leiate paljude sama Office 365 portaalis. 

Lisateabe saamiseks lugege teemat [Sissejuhatus Accessi paani](active-directory-saas-access-panel-introduction.md). 



---

**Q: kas Azure AD aita oma kohapealse taristu haldamise?**

**A:** Jah, see. Azure'i AD Premium edition pakub **Ühenduse seisund**. Azure'i AD-ühendus seisundi abil saate jälgida ja saada aimu oma kohapealse identiteedi taristu ja sünkroonimise teenused.  

Lisateabe saamiseks vaadake [kuvari oma kohapealse identiteedi taristu ja sünkroonimise teenused pilveteenuses](active-directory-aadconnect-health.md).  

---

## <a name="password-management"></a>Parooli haldus

**Q: Kas ma kasutan Azure AD parooli allahindluse ilma parooli sünkroonimine? (Ehk, ma tahan kasutada Azure AD SSPR parooli allahindluse, kuid ma ei soovi talletatud soovitud cloud? paroolide)**

**A:** Selleks, et allahindluse AD paroolid Azure AD sünkroonimine pole vaja. Välise keskkonnas, Azure AD SSO tugineb kohapealse kataloogi autentida. Sel juhul ei nõua asutusesisese parool Azure AD jälgida.

---

**Q: kuidas palju aega kulub kirjutada tagasi kohapealse AD parooli?**

**A:** Parooli allahindluse toimib reaalajas. 

Lisateabe saamiseks vaadake teemat [Alustamine parooli haldamise](active-directory-passwords-getting-started.md) 


---

**Q: kas kasutada parooli allahindluse paroole, mida haldab administraator?**

**A:** Jah, kui teil on lubatud allahindluse parooli, administraatori parooli toimingute kirjutada tagasi oma asutusesiseses keskkonnas.  

Rohkem vastuseid parooli seotud küsimused, lugege teemat [Parooli halduse korduma kippuvad küsimused](active-directory-passwords-faq.md).

---

## <a name="application-access"></a>Accessi rakendus


**K: kust võib leida Azure AD eelnevalt integreeritud rakenduste loendit ja nende võimalusi?**

**A:** Azure AD on üle 2600 eelnevalt integreeritud rakenduste Microsoft, rakenduse teenusepakkujatele või partnerid. Kõik eelnevalt integreeritud rakendused toetavad SSO-d. SSO võimaldab teil oma rakendused teie ettevõtte mandaadi abil. Mõned rakenduste toetavad ka automatiseeritud ettevalmistamise ja tühistage ettevalmistamine

Eelnevalt integreeritud rakenduste täieliku loendi leiate teemast [Active Directory turuplatsilt](https://azure.microsoft.com/marketplace/active-directory/).


---

**K: kui mul on vaja ei ole Azure AD turuplatsil?**

**A:** Azure AD Premium, saate lisada ja mis tahes rakendus, mida soovite konfigureerida. Sõltuvalt rakenduse võimaluste ja oma eelistusi, saate konfigureerida SSO-d ja automatiseeritud ettevalmistamise.  

Lisateavet leiate teemast:

- [Ühekordse sisselogimise rakendustele, mis ei ole Azure Active Directory rakenduse Galerii konfigureerimine](active-directory-saas-custom-apps.md)
- [Luba automaatne ettevalmistamise kasutajatele ja rühmadele Azure Active Directory rakendustele SCIM abil](active-directory-scim-provisioning.md) 


---

**Q: Kuidas teha kasutajate rakenduste Azure Active Directory abil sisse logida?**
 
**A:** Azure Active directory on mitu võimalust kasutajad saaksid neid vaadata ja nende rakendustele nagu:

- Azure AD juurdepääsu paneel

- Office 365 rakenduse käiviti

- Otsest sisselogimise ühendatud rakenduste

- Sügav lingid ühendatud, parooli või olemasoleva rakendused

Lisateavet leiate teemast [juurutamine Azure AD integreeritud rakenduste kasutajatele](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users).


---

**Q: mis on Azure Active Directory erineval viisil võimaldab autentimis- ja ühekordse sisselogimise rakendustele?**
 
**A:** Azure Active Directory toetab palju standardne Protokollid autentimise ja luba nagu SAML 2.0, OpenID ühendus, OAuth 2.0 ja oli-Federation. Azure AD toetab samuti parooli Holvi ja automatiseeritud võimaluste rakendusi, mis toetavad ainult vormipõhist autentimist.  

Lisateavet leiate teemast:

- [Azure AD autentimise stsenaariumid](active-directory-authentication-scenarios.md)

- [Active Directory autentimise Protokollid](https://msdn.microsoft.com/library/azure/dn151124.aspx)

- [Kuidas ühekordse sisselogimise Azure Active Directory töötamise?](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work)


---

**Q: Kas lisada rakenduste kohapealse töötab?**

**A:** Azure'i AD Rakenduse puhverserveri pakub lihtne ja turvaline juurdepääs kohapealse veebirakendusi, mille valimisel. Pääsete need rakendused avate SaaS rakenduste Azure Active Directory samal viisil. Ei ole vaja VPN või muuta võrgu taristule.  

Lisateabe saamiseks vaadake, [Kuidas turvalist remote juurdepääsu kohapealse rakendused](active-directory-application-proxy-get-started.md).


--- 

**Q: kuidas kasutajad, kes kasutavad konkreetse rakenduse MFA nõue?**

**A:** Azure'i AD-tingimusvormingu juurdepääsu, saate määrata iga rakenduse jaoks kordumatu juurdepääsu poliitika. Teie poliitika, saate nõuda MFA kogu aeg või kui kasutajad ei ole kohaliku võrku ühendatud.  

Täpsema teabe saamiseks vt [tagada juurdepääs Office 365 ja Azure Active Directory ühendatud teiste rakenduste](active-directory-conditional-access.md).


---

**Q: mis on automatiseeritud kasutaja ettevalmistamise SaaS rakendusi?**

**A:** Azure Active Directory võimaldab automatiseerida loomise, hoolduse ja kasutaja identiteedid paljude populaarsete pilv (SaaS) rakendusi eemaldamist. 

Lisateabe saamiseks vt [automatiseerida kasutaja ettevalmistamise ja Deprovisioning SaaS rakenduste Azure Active Directory](active-directory-saas-app-provisioning.md)

---



