<properties
   pageTitle="Kuidas integreerimine Azure Active Directory | Microsoft Azure'i"
   description="Juhend eelised ja ressursside integreerimine Azure Active Directory jaoks."
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="bryanla"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/16/2016"
   ms.author="mbaldwin"/>

# <a name="integrating-with-azure-active-directory"></a>Integreerimine Azure Active Directory

[AZURE.INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Azure Active Directory pakub ettevõtted, kus äriklassi identiteetide haldus pilve rakendused.  Azure'i AD integreerimine annab kasutajate lihtsustatud sisselogimine ja vastavad IT poliitika rakenduse abil.

## <a name="how-to-integrate"></a>Kuidas integreerida

On mitu võimalust, et integreerimine Azure AD Rakenduse.  Kasutage nii palju või vähe need stsenaariumid asjakohased rakenduse.

### <a name="support-azure-ad-as-a-way-to-sign-in-to-your-application"></a>Toeta Azure AD nii, et teie rakendusse sisselogimine

**Logige sisse hõõrdumise vähendamiseks ja tugiteenuste vähendada.** Azure AD Rakenduse sisse logida kasutades oma kasutajad ei veel üks nimi ja parool meeles pidada.  Arendaja, siis on teil ühe vähem talletada ja kaitsmiseks parooli.  Ei peaks hakkama unustatud parooli lähtestamise võib olla ainult märkimisväärne kokkuhoid.  Azure AD volitused mõned maailma kõige populaarsemate pilv rakendusi, sh Office 365 ja Microsoft Azure'i Logi sisse.  Millel kasutajad miljoneid ettevõtted, võimalused on teie kasutaja on juba sisse loginud Azure AD.  Lugege lisateavet [lisamise tugi Azure AD Logi sisse](../active-directory-authentication-scenarios.md).

**Lihtsustage sisselogimise häälestamine rakenduse.**  Ajal rakenduse, Azure AD saate saata olulist teavet kasutaja, et saate täita eelnevalt oma registreerumise vormi või selle täielikult eemaldada.  Kasutajad saavad registreeruda nende Azure AD kontoga on tuttav nõusolekut kogemus sarnanevad sotsiaalmeedia ja mobiilirakenduste kaudu rakenduse.  Igale kasutajale, saate sisse logida ja rakendus, mis on integreeritud Azure AD nõudmata kaasamine sisse logida.  Lugege lisateavet [allkirjastamise üles rakenduse Azure'i AD kontosse sisselogimise](../../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md)kohta.

### <a name="browse-for-users-manage-user-provisioning-and-control-access-to-your-application"></a>Kasutajaid otsida, hallata kasutajate ettevalmistamine ja rakenduse juurdepääsu reguleerimine

**Otsige kataloogis kasutajad.**  Otsi ja otsige oma organisatsiooni teiste liikmetega, kui kutsudes teisi kasutajaid Graph API abil või juurdepääsu asemel neid Tippige e-posti aadressid.  Kasutajaid saate sirvida tuttavad aadress aadressiraamatu laadi liidese, sh asutuse hierarhial üksikasjade vaatamine.  Lugege lisateavet [Graph API](../active-directory-graph-api.md).

**Uuesti kasutada Active Directory rühmad ja leviloendid oma kliendi juba haldamine.**  Azure AD sisaldab rühmad oma kliendi juba abil meilisõnumite saatmine ja juurdepääsu haldamine.  Graafik API abil uuesti kasutada nende rühmade asemel oma kliendi luua ja hallata eraldi rakenduse rühmade kogum.  Rühma teabe saab saata ka rakenduse sõned sisselogimine.  Lugege lisateavet [Graph API](../active-directory-graph-api.md).

**Kasutage Azure AD juhtelemendile, kellel on juurdepääs teie.**  Administraatorid ja rakenduse omanikud Azure AD saate määrata Accessi rakenduste kindlatele kasutajatele ja rühmadele.  Graafik API abil saate lugeda selle loendi ja kontrollida ettevalmistamise ja ressursside tühistage ettevalmistamise ja rakenduse kaudu juurdepääsu abil.

**Azure AD rollide vastavalt juurdepääsu reguleerimine.**  Administraatorid ja rakenduse omanikud saate määrata rakenduse registreerimisel Azure AD määratlevad rollid kasutajad ja rühmad.  Roll teave saadetakse rakenduse sõned sisselogimine ja ka lugemiseks saab kasutada Graph API.  Lisateavet [Azure AD loa kasutamise](http://blogs.technet.com/b/ad/archive/2014/12/18/azure-active-directory-now-with-group-claims-and-application-roles.aspx)kohta.

### <a name="get-access-to-users-profile-calendar-email-contacts-files-and-more"></a>Juurdepääs kasutaja profiili, kalender, e-posti, kontaktide, failide ja palju muud

**Azure AD ei luba server Office 365 ja muude Microsoft business teenuste jaoks.**  Kui Azure AD kasutajatugi logige sisse oma rakenduse või oma praeguse kasutajakontode linkimine Azure AD kasutajakontosid OAuth 2.0 tugi, saate taotleda lugemis- ja kirjutusõiguse kasutaja profiili, kalender, e-posti, kontaktide, failide ja muud teavet.  Saate sujuvalt kirjutamise sündmuste kalender kasutaja ja lugeda või kirjutada oma OneDrive'i failide.  Lugege lisateavet [Office 365 API -de juurdepääs](https://msdn.microsoft.com/office/office365/howto/platform-development-overview).

### <a name="promote-your-application-in-the-azure-and-office-365-marketplaces"></a>Saab reklaamida Azure ja Office 365 turgude rakenduse

**Ettevõtted, kes kasutavad juba Azure AD miljoneid rakenduse tõstmine.**  Otsige ja sirvige nende turgude kasutajatele juba kasutavad ühte või rohkem pilveteenustega, muutes need sobivad pilveteenuse teenuse klientidele.  Lisateavet rakenduse [Azure'i](https://azure.microsoft.com/marketplace/partner-program/)turuplatsil edendamine.

**Kui registreeruda rakenduse, kuvatakse see oma Azure AD kasutada paneeli ja Office 365 rakendusekäiviti.**  Kasutajad saaksid kiiresti ja kerge vaevaga naasmiseks hiljem rakenduse parandamine kasutaja allikaid.  Lugege lisateavet [Azure AD juurdepääs paani](../active-directory-saas-access-panel-introduction.md).

### <a name="secure-device-to-service-and-service-to-service-communication"></a>Turvaline seadme-teenus ja teenusest side

**Azure AD kasutamise identiteetide haldus teenuste ja seadmete vähendab koodi kirjutamiseks peate, kuid lubab seda, et juurdepääsu haldamine.**  Teenuste ja seadmete saate sõned toomine abil OAuthi Azure AD ja kasutada neid sõned web API-de juurde.  Azure AD abil saate vältida keerukate autentimine koodi kirjutamist.  Kuna sellest, millised teenused ja seadmed on talletatud Azure AD, haldamiseks võtmed ja tühistamine ühes kohas, selle asemel, et seda oma rakenduse eraldi teha.

## <a name="benefits-of-integration"></a>Integreerimise eelised

Integreerimine Azure AD on kasu, mis ei nõua täiendavat koodi kirjutamiseks saate.

### <a name="integration-with-enterprise-identity-management"></a>Integreerimine ettevõtte identiteedi haldamine

**Vastavad IT poliitikate rakenduse spikker.**  Ettevõtted saavad ettevõtte identiteedi haldamise süsteemide integreerimine Azure AD, nii, et isiku lahkumisel ettevõtte need automaatselt kaotavad väliskasutajad juurdepääsu rakenduse ilma SELLETA kellel on vaja teha täiendavaid toiminguid tegema.  Saate hallata, kes saavad teie taotlus ja kindlaks teha, mis juurdepääsupoliitikaid - i jaoks vajalike näide mitmikautentimise - vähendamine teie vaja kirjutada koodi keerukate ettevõttepoliitikaid täitmiseks.  Azure AD pakub üksikasjalikke auditilogi, kes sisse logitud rakenduse nii, et see saab jälgida kasutust administraatoritele.

**Nii, et teie taotlus saate integreerida AD laiendab Azure AD Active Directory pilveteenusesse.**  Paljud ettevõtted kogu maailmas kasutada Active Directory oma peamine sisselogimine ja identiteedihaldussüsteemi ja nõuavad nende rakenduste AD töötamiseks.  Active Directory integreerimine Azure AD Rakenduse integreerub.

### <a name="advanced-security-features"></a>Täpsemad funktsioonid

**Mitmikautentimise.**  Azure AD pakub kohalikke mitmikautentimise.  IT-administraatorid saate nõuda mitmikautentimise oma rakenduse avamiseks, et teil pole kood tugi ise.  Lugege lisateavet [Mitmikautentimise](https://azure.microsoft.com/documentation/services/multi-factor-authentication/).

**Automaattuvastus Anomaalne sisselogimine.**  Azure AD töötleb rohkem kui miljardit sisselogimist päevas masina õ algoritmide abil avastada kahtlasest tegevusest ja teatage IT-administraatorid võimalikke probleeme.  Täiendavad Azure AD Logi sisse, saab rakenduse kasu kaitse. Lisateavet [aruande kuvamise Azure Active Directory juurde](../active-directory-view-access-usage-reports.md).

**Tingimusvormingu juurdepääs.**  Mitmikautentimise, lisaks administraatorid saavad nõua eritingimuste olema täidetud, enne kui kasutajad saavad sisse logida rakenduse.  Saate määrata tingimustele kaasata IP-aadress vahemikus kliendi seadmed, määratud rühmaliikmeid ja juurdepääsu kasutatava seadme olek.  Lugege lisateavet [tingimusvormingu Azure Active Directory access](../active-directory-conditional-access.md).

### <a name="easy-development"></a>Lihtne arendamine

**Valdkonna standard protokolle.**  Microsoft on otsustanud toetada tööstusstandarditele.  Azure AD toetab SAML 2.0, OpenID ühenduse 1.0, OAuth 2.0 ja oli-Federation 1.2 autentimise protokolle.  Graph API on OData 4.0 nõuetele.  Kui teie rakendus on juba toetab SAML 2.0 või OpenID ühenduse 1.0 Protokollid ühendatud Logi sisse, lisamise tugi Azure AD võib olla lihtne.  Lugege lisateavet [Azure AD toetatud autentimine Protokollid](../active-directory-authentication-protocols.md).

**Avage allikas teegid.**  Microsoft osutab täielikult toetatud avatud allika teekide populaarsed keelte ja kiiruse arengu platvormide jaoks.  Lähtekoodi on litsents Apache 2.0 ja te olete tasuta kahvli ja projektide tagasi aitama.  Lisateavet [teekide Azure AD autentimist](../active-directory-authentication-libraries.md).

### <a name="worldwide-presence-and-high-availability"></a>Kogu maailmas võrgusoleku- ja kõrge-saadavus

**Azure AD juurutatakse andmekeskuste kogu maailmas ja hallata ja jälgida ööpäev.**  Azure AD on Microsoft Azure'i ja Office 365 identiteedihaldussüsteemi ja juurutatakse 28 andmekeskuste kogu maailmas.  Andmed on tagatud vähemalt kolme andmekeskuste paljundada.  Globaalne koormus soolise lähima koopia oma andmeid sisaldav Azure AD kasutajate juurdepääsu tagamiseks ning automaatselt uuesti marsruutimine taotluste muude andmekeskuste, kui probleem on avastatud.

## <a name="next-steps"></a>Järgmised sammud

[Töö alustamine koodi kirjutamist](../active-directory-developers-guide.md#getting-started).

[Logi kasutajate Azure AD abil](../active-directory-authentication-scenarios.md)
