<properties
    pageTitle="Azure'i SaaS rakenduste juurdepääsu | Microsoft Azure'i"
    description="Juurdepääsu Azure AD saate konfigureerida rakenduse kohta mitmikautentimise juurdepääsureeglid ja blokeerimise pole võrgus usaldusväärsete kasutajate jaoks. "
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
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="markvi"/>

# <a name="getting-started-with-azure-active-directory-conditional-access"></a>Azure Active Directory juurdepääsu töötamise alustamine

Azure Active Directory tingimusvormingu Accessi rakenduste [SaaS](https://azure.microsoft.com/overview/what-is-saas/) ja Azure AD ühendatud rakenduste abil saate konfigureerida alusel rühmitamine, asukoha ja rakenduse tundlikkuse juurdepääsu. 

Tingimusvormingu Accessi rakenduse tundlikkuse alusel, saate mitmikautentimise (MFA) juurdepääsureeglid taotluse kohta. MFA taotluse kohta leiate blokeerimise juurdepääsu kasutajatele, kellel ei ole usaldusväärsete võrgus. Saate rakendada kõigile kasutajatele rakendusse või ainult määratud turvalisus rühmades kasutajate jaoks määratud MFA reeglid.  Kasutajad võivad MFA nõue välistatud, kui need on juurdepääs rakenduse kaudu IP-aadress, mis on sees ettevõtte võrgus.

Need funktsioonid on saadaval klientidele, kes ostsid on Azure Active Directory Premium litsents.

## <a name="scenario-prerequisites"></a>Stsenaarium eeltingimused
* Azure Active Directory Premium litsentsi

* Ühendatud või hallatavate Azure Active Directory rentnik

* Ühendatud rentnike jaoks on vaja seda mitmikautentimise olema lubatud.

## <a name="configure-per-application-access-rules"></a>Rakenduse kohta juurdepääsureeglid konfigureerimine

Selles jaotises kirjeldatakse, kuidas konfigureerida rakenduse kohta juurdepääsureeglid.

1. Logige sisse kontoga, millel on üldadministraator Azure AD Azure klassikaline portaali.
2. Klõpsake vasakpoolsel paanil nuppu **Active Directory**.
3. Valige vahekaardil Directory kataloogi.
4. Valige menüü **rakendused** .
5. Valige rakendus, mis määratakse reegel.
6. Valige vahekaart **konfigureerimine** .
7. Liikuge kerides jaotiseni juurdepääsu reeglid. Valige soovitud juurdepääsureegleid.
8. Määrata kasutajad rakendatakse reegel.
9. Poliitika, valides **olema lubatud**.

##<a name="understanding-access-rules"></a>Juurdepääsureeglid mõistmine

See jaotis annab juurdepääsu reegleid toetatud Azure'i tingimusvormingu rakendus Accessis üksikasjalikku kirjeldust.

### <a name="specifying-the-users-the-access-rules-apply-to"></a>Määrab kasutajatele juurdepääsu reeglite rakendamine

Vaikimisi rakendatakse kõigile kasutajatele, millele on juurdepääs rakenduse poliitika. Siiski saate ka piirata poliitika kasutajad, mis on määratud turberühmade liikmed. Valige üks või mitu rühma juurdepääsu reegel rakendub rühma valiku dialoogi kasutatakse nuppu **Lisa rühm** . Selle dialoogiboksi saate kasutada ka eemaldada valitud rühmad. Reegleid valimisel rühmade rakendamiseks ainult jõustatakse juurdepääsu reeglid kasutajatele, kes kuuluvad ühte määratud turberühmad.

Turberühmad võib ka selgesõnaliselt jätta poliitika, valides suvandi **peale** ja määrata ühe või mitme rühmad. Kasutajad, mis on loendis **peale** rühma liige ei ole mitmikautentimise nõutakse, isegi juhul, kui need on rühma, mis rakendatakse reegel juurdepääsu.
Allpool näidatud Accessi reegli nõuab kõigi kasutajate rühma kasutada mitmikautentimise rakenduse kasutamisel haldurid.

![Sätte tingimusvormingu juurdepääsureeglid MFA](./media/active-directory-conditional-access-azuread-connected-apps/conditionalaccess-saas-apps.png)

## <a name="conditional-access-rules-with-mfa"></a>MFA juurdepääsu reeglite
Kui kasutaja on konfigureeritud, kasutades funktsiooni kasutaja mitmikautentimise, seda sätet kasutaja kombineerida rakenduse mitmikautentimise reegleid. See tähendab, et kasutaja, mis on konfigureeritud kasutaja mitmikautentimise jaoks on vaja teha mitmikautentimise isegi siis, kui nad on vabastatud rakenduse mitmikautentimise reeglid. Lisateavet mitme teguri autentimis- ja kasutaja sätted.

### <a name="access-rule-options"></a>Reegli juurdepääsusuvandid
Toetatud on järgmised suvandid:

* **Nõua mitmikautentimise**: kasutajad, kellele juurdepääsu reeglite rakendamine, on vaja täieliku mitmikautentimise enne poliitika kehtib rakendus.

* **Nõua mitmikautentimise pole tööl**: kasutaja, mis on pärit usaldusväärsete IP-aadress peavad pole mitmikautentimise sooritamiseks. Mitmikautentimise sätete lehel saab konfigureerida usaldusväärse IP-aadresside vahemikud.

* **Kui pole tööl juurdepääsu**: kasutaja, mis on pärit usaldusväärsete IP-aadress on blokeeritud. Mitmikautentimise sätete lehel saab konfigureerida usaldusväärse IP-aadresside vahemikud.

### <a name="setting-rule-status"></a>Sätte reegli olek
Accessi reegli olek võimaldab reegleid lülitada sisse või välja. Kui access reeglid on välja lülitatud, mitmikautentimise nõue on täitmisele.

### <a name="access-rule-evaluation"></a>Accessi reeglite hindamise

Juurdepääsureeglid hinnatakse, kui kasutaja pääseb juurde ühendatud rakendus, mis kasutab OAuth 2.0, OpenID arvutiga, SAML või oli-Federation. Lisaks juurdepääsureeglid hinnatakse, kui OAuth 2.0 ja OpenID ühendamine abil Värskenda luba juurdepääsu sümboolse hankimine. Kui poliitika hindamise nurjub, kui kasutatakse värskendamise luba, tagastatakse veaväärtus **invalid_grant** , see näitab, et kasutaja peab uuesti autentida kliendile.

###<a name="configure-federation-services-to-provide-multi-factor-authentication"></a>Mitmikautentimise esitada federation services konfigureerimine

Jaoks ühendatud rentnikud, võib MFA läbi Azure Active Directory või asutusesisese AD FS server.

Vaikimisi toimub MFA korraldaja Azure Active Directory lehte. MFA kohapealse konfigureerimiseks peab atribuudi **– SupportsMFA** seatud **tõene** Azure Active Directory, kasutades Azure AD moodul Windows PowerShelli jaoks.

Järgmises näites kirjeldatakse, kuidas lubada kohapealse MFA rentnikus contoso.com [cmdlet-käsu Set-MsolDomainFederationSettings](https://msdn.microsoft.com/library/azure/dn194088.aspx) abil.

    Set-MsolDomainFederationSettings -DomainName contoso.com -SupportsMFA $true

Lisaks selle lipu, peab olema konfigureeritud väline rentniku AD FS-i eksemplari mitmikautentimise sooritamiseks. Järgige [kohapealse Azure'i Mitmikautentimise kasutamise](../multi-factor-authentication/multi-factor-authentication-get-started-server.md)kohta.

## <a name="related-articles"></a>Seotud artiklid

- [Office 365 ja muude rakenduste juurdepääsu ühendatud Azure Active Directory](active-directory-conditional-access.md)
- [Artikli registri Rakendusehaldus Azure Active Directory 's](active-directory-apps-index.md)
