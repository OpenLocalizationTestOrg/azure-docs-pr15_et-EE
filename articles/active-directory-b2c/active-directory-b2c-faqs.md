<properties
    pageTitle="Azure Active Directory B2C: Korduma kippuvad küsimused | Microsoft Azure'i"
    description="Korduma kippuvad küsimused Azure Active Directory B2C kohta"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/09/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-faqs"></a>Azure Active Directory B2C: KKK

Sellelt lehelt leiate vastused korduma kippuvatele küsimustele B2C Azure Active Directory (Azure AD). Kontrollimiseks tagasi värskendused.

### <a name="can-i-use-azure-ad-b2c-features-in-my-existing-employee-based-azure-ad-tenant"></a>Azure'i AD B2C funktsioone saab kasutada minu olemasoleva, töötaja Azure AD rentniku?

Praegu ei Azure AD B2C funktsioonid sisse lülitada sisse oma olemasoleva Azure AD rentniku. Me soovitame teil luua eraldi rentniku haldamine tarbijatele Azure AD B2C funktsioonide abil.

### <a name="can-i-use-azure-ad-b2c-to-provide-social-login-facebook-and-google-into-office-365"></a>Kas saan kasutada Azure AD B2C esitada social login (Facebooki ja Google) teenusekomplekti Office 365?

Microsoft Office 365 ei saa kasutada Azure AD B2C. Üldiselt ei saa kasutada pakuvad autentimise rakendustel SaaS (Office 365, Salesforce, Workday jne). See pakub identiteedi ja juurdepääsu juhtimine ainult tarbija suunatud web ja mobiilirakenduste ja pole rakendatav töötaja või partneri stsenaariumid.

### <a name="what-are-local-accounts-in-azure-ad-b2c-how-are-they-different-from-work-or-school-accounts-in-azure-ad"></a>Mis on Azure AD B2C kohalikud kontod? Mille poolest erinevad need töökoha või kooli konto kaudu Azure AD?

Azure AD rentniku, iga kasutaja rentnik (välja arvatud olemasoleva Microsofti kontodega kasutajad) logib meiliaadressiga vormi `<xyz>@<tenant domain>`, kus `<tenant domain>` on kinnitatud domeenid rentniku või algse `<...>.onmicrosoft.com` domeeni. Seda tüüpi konto on töökoha või kooli kontoga.

Azure AD B2C rentniku, enamik rakendusi soovite kasutaja sisse logida mis tahes suvalise meiliaadressi (nt joe@comcast.net, bob@gmail.com, sarah@contoso.com, või jim@live.com). Seda tüüpi konto on kohalik konto. Täna, toetame ka suvalise kasutajanimed (lihtsalt stringid) nimega kohalikud kontod (nt joe, bob, sarah või jim). Saate valida järgmiste kohaliku konto kaht liiki teenuses Azure AD B2C.

### <a name="which-social-identity-providers-do-you-support-now-which-ones-do-you-plan-to-support-in-the-future"></a>Millised suhtlusvõrgu identiteedipakkujad saate toetab nüüd? Millised kavatsete edaspidi toetab?

Toetame praegu Facebooki, Google +, LinkedIni ja Amazon. Lisame muude populaarsed social identiteedipakkujad vastavalt kliendi nõudmisel tugi.

### <a name="can-i-configure-scopes-to-gather-more-information-about-consumers-from-various-social-identity-providers"></a>Saate konfigureerida otsinguulatuste koguda tarbijate kohta lisateavet erinevate social identiteedipakkujad?

Ei, kuid see funktsioon on meie teejuht. Vaikimisi otsinguulatuste, kasutatakse meie toetatud komplekt social identiteedipakkujad on:

- Facebooki: e-posti
- Google +: e-posti
- Microsofti kontoga: openid e-posti profiili
- Amazon: profiili
- LinkedIni: r_emailaddress r_basicprofile

### <a name="does-my-application-have-to-be-run-on-azure-for-it-work-with-azure-ad-b2c"></a>Kas minu rakendus on Azure'i jaoks see töötab Azure AD B2C käivitada?

Ei, saate majutada oma rakenduse suvalist (pilve või kohapealse). Azure'i AD B2C suhelda läheb vaja läheb võimalus saata ja vastu võtta avalikult juurdepääsetava lõpp-punktid HTTP-päringud.

### <a name="i-have-multiple-azure-ad-b2c-tenants-how-can-i-manage-them-on-the-azure-portal"></a>Mul on mitme Azure AD B2C rentniku. Kuidas hallata neid Azure'i portaalis?

Iga Azure AD B2C rentniku on eraldi B2C funktsioonid blade Azure'i portaalis. Vt [Azure AD B2C: registreerida rakenduse](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) saate teada, kuidas saate liikuda teatud rentniku B2C funktsioonid blade Azure'i portaalis. Azure AD B2C kataloogide Azure'i portaalis vaheldumisi ei säilita oma B2C funktsioonid blade avamine enamikus brauserites.

### <a name="how-do-i-customize-verification-emails-the-content-and-the-from-field-sent-by-azure-ad-b2c"></a>Kuidas kohandada kinnitamine e-kirju (sisu ja "kaudu:" välja) saadetud Azure AD B2C?

[Ettevõtte tootemargi funktsiooni](../active-directory/active-directory-add-company-branding.md) abil saate kohandada sisu kinnitamine e-kirju. Täpsemalt, saate kohandatud e-posti kaks elementi:

- **Banner Logo**: paremas allnurgas kuvatakse.
- **Taustavärvi**: ülaosas.

    ![Kohandatud kinnitusmeil kuvatõmmis](./media/active-directory-b2c-faqs/company-branded-verification-email.png)

Meilisõnumi signatuuri sisaldab B2C rentniku nimi sisestatud B2C rentniku esmakordsel loomisel. Saate muuta nimi, kasutades järgmisi juhiseid.

- Tellimuse administraatori [Azure klassikaline portaali](https://manage.windowsazure.com/) sisse logida.
- Liikuge oma B2C rentniku.
- Klõpsake vahekaardil **konfigureerimine** .
- Saate muuta **kausta atribuudid** jaotises välja **nimi** .
- Klõpsake lehe allosas **salvestada** .

Praegu ei ole võimalik muuta selle "kaudu:" välja e-posti kohta. Kui olete huvitatud seda võimalust ja keha kinnitusmeil täielikult kohandada, hääletada [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/suggestions/15334335-fully-customizable-verification-emails)funktsiooni.

### <a name="how-can-i-migrate-my-existing-user-names-passwords-and-profiles-from-my-database-to-azure-ad-b2c"></a>Kuidas saate migreerida minu olemasoleva kasutajanimed, paroolid ja profiili minu andmebaasist Azure AD B2C abil?

Saate kirjutada oma Migreerimistööriista Azure AD Graph API. Lugege teemat [Graph API valimi](active-directory-b2c-devquickstarts-graph-dotnet.md) üksikasju. Kohaneda erinevate migreerimise Valikud ja tööriistad välja-ja--box tulevikus.

### <a name="what-password-policy-is-used-for-local-accounts-in-azure-ad-b2c"></a>Paroolipoliitika, mida kasutatakse Azure AD B2C kohalikud kontod?

Azure'i AD B2C paroolipoliitika kohaliku kontode põhineb poliitika Azure AD. Azure'i AD B2C kasutaja sisse logida, registreerumise või Logi sisse ja parooli lähtestamine poliitikate kasutab "tugev" parooli tugevus ja mis tahes paroolid ei aeguks. Lugege lisateavet [Azure AD paroolipoliitika](https://msdn.microsoft.com/library/azure/jj943764.aspx) .

### <a name="can-i-use-azure-ad-connect-to-migrate-consumer-identities-that-are-stored-on-my-on-premises-active-directory-to-azure-ad-b2c"></a>Kasutada Azure'i AD-ühenduse tarbija identiteedid, mis on talletatud minu kohapealse Active Directory Azure AD B2C migreerida?

Ei, Azure'i AD-ühenduse pole mõeldud töötamiseks Azure AD B2C. Kohaneda erinevate migreerimise Valikud ja tööriistad välja-ja--box tulevikus.

### <a name="does-azure-ad-b2c-work-with-crm-systems-such-as-microsoft-dynamics"></a>Azure'i AD B2C töötab nt Microsoft Dynamics CRM-i süsteemidega?

Praegu ei. Integreerimine need süsteemid on meie teejuht.

### <a name="does-azure-ad-b2c-work-with-sharepoint-on-premises-2016-or-earlier"></a>Kas Azure AD B2C SharePointi asutusesiseste 2016 töö ja varasemate versioonidega?

Praegu ei. Azure'i AD B2C pole tugi SAML 1.1 märgid, mis SharePointi asutusesisese vajate ehitatud portaalide ja rakenduste pood. Pange tähele, et Azure AD B2C on mõeldud SharePoint välise partneri ühiskasutuse stsenaarium; teemast [Azure AD B2B](http://blogs.technet.com/b/ad/archive/2015/09/15/learn-all-about-the-azure-ad-b2b-collaboration-preview.aspx) asemel.

### <a name="should-i-use-azure-ad-b2c-or-b2b-to-manage-external-identities"></a>Kasutada Azure AD B2C või B2B välise identiteedid hallata?

Lugege seda artiklit [väliste identiteedid](../active-directory/active-directory-b2b-compare-external-identities.md) Lisateavet oma välise identiteedi stsenaariumid vastav funktsioonide rakendamise kohta.

### <a name="what-reporting-and-auditing-features-does-azure-ad-b2c-provide-are-they-the-same-as-in-azure-ad-premium"></a>Millist aruandlus- ja funktsioonide kontrollimine kas Azure AD B2C annab? Kas nad on sama mis Azure AD Premium?

Ei, ei toeta Azure AD B2C samu aruanded nimega Azure AD Premium. Azure'i AD B2C on vabastamine lihtsa aruandlus- ja auditeerimine API-de varsti.

### <a name="can-i-localize-the-ui-of-pages-served-by-azure-ad-b2c-what-languages-are-supported"></a>Saate ma localize lehtede kätte Azure AD B2C UI? Millistes keeltes on toetatud?

Praegu Azure AD B2C on optimeeritud inglise ainult. Plaanis hakkama lokaliseerimine funktsioonid nii kiiresti kui võimalik.

### <a name="can-i-use-my-own-urls-on-my-sign-up-and-sign-in-pages-that-are-served-by-azure-ad-b2c-for-instance-can-i-change-the-url-from-loginmicrosoftonlinecom-to-logincontosocom"></a>Mis on Azure AD B2C kätte registreerumise ja logige sisse lehtedel saab kasutada oma URL-id? Näiteks saate muuta URL-i kaudu login.microsoftonline.com login.contoso.com?

Praegu ei. See funktsioon on meie teejuht. Samuti võtke arvesse, et **domeenide** menüüs oma rentniku Azure klassikaline portaalis domeeni kinnitamisega ei tee seda.

### <a name="how-do-i-delete-my-azure-ad-b2c-tenant"></a>Kuidas kustutada minu Azure AD B2C rentniku?

Teie Azure AD B2C rentniku kustutamiseks tehke järgmist

- Järgmiste juhiste liikumiseks [B2C funktsioonid tera](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) Azure'i portaalis.
- Liikuge **rakendused**, **identiteedipakkujad** ja **kõigi poliitikate** labad ja kustutada kõik kirjed igas.
- Nüüd sisselogimine [Azure klassikaline portaali](https://manage.windowsazure.com/) tellimuse administraatorina. (See on sama töö või kooli kontoga või sama Microsofti kontoga sisselogimiseks Azure kasutatud)
- Active Directory laiend vasakul ja klõpsake oma B2C rentniku.
- Klõpsake vahekaarti **Kasutajad** .
- Märkige iga kasutaja Lülita (te praegu sisse logitud, st tellimuse administraator kasutaja välja). Valige lehe allosas **kustutamine** ja klõpsake nuppu **Jah** küsimise.
- Klõpsake vahekaarti **rakendused** .
- Valige **minu ettevõte kuulub rakenduste** väljal **Kuva** drop-down ja klõpsake märkeruutu.
- Kuvatakse rakendus nimega **b2c-laiendid-rakenduse** allpool. Valige lehe allosas **kustutamine** ja klõpsake nuppu **Jah** küsimise.
- Valige oma B2C rentniku liikuge Active Directory laiend uuesti.
- Klõpsake lehe allosas **kustutada** . Kuva protsessi lõpuleviimiseks järgige juhiseid.

### <a name="can-i-get-azure-ad-b2c-as-part-of-enterprise-mobility-suite"></a>Kas ma saan Azure AD B2C osana ettevõtte mobiilsus?

Ei, Azure AD B2C on grupikindlustusleping Azure'i teenus on ja pole osa ettevõtte mobiilsus.

### <a name="how-do-i-report-issues-with-azure-ad-b2c"></a>Kuidas teatada Azure AD B2C probleeme?

Vaadake [faili tugi Azure Active Directory B2C taotlused](active-directory-b2c-support.md).

## <a name="more-information"></a>Lisateave

Samuti võite üle vaadata praeguse [teenusepiirangud, piiranguid ja piiranguid](active-directory-b2c-limitations.md).
