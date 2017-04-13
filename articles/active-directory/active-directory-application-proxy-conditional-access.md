<properties
    pageTitle="Tingimusvormingu Accessi rakenduste avaldatud Azure AD rakenduse puhverserver"
    description="Hõlmab häälestamise tingimusvormingu Accessi rakenduste juurde kaugühenduse teel abil Azure AD Rakenduse puhverserveri avaldada."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/22/2016"
    ms.author="kgremban"/>

# <a name="working-with-conditional-access"></a>Tingimusvormingu Accessiga töötamine

Saate konfigureerida juurdepääsureeglid tingimusvormingu juurdepääsu avaldatud rakenduse puhverserveri abil. See võimaldab teil.

- Nõua mitmikautentimise taotluse kohta
- Nõua mitmikautentimise ainult siis, kui kasutajad ei tööta
- Blokeeri kasutajate juurdepääsu taotlus, kui nad pole tööl

Reegleid saab rakendada kõigile kasutajatele ja rühmadele või ainult teatud kasutajad ja rühmad. Vaikimisi reegel kehtib kõigile kasutajatele, kellel on juurdepääs rakenduse. Siiski saate reegli piiratud kasutajatele, mis on määratud turberühmade liikmed.  

Juurdepääsureeglid hinnatakse, kui kasutaja pääseb juurde ühendatud rakendus, mis kasutab OAuth 2.0, OpenID arvutiga, SAML või oli-Federation. Lisaks juurdepääsureeglid hinnatakse värskendamise luba saab hankida juurdepääsu sümboolse OAuth 2.0 ja OpenID ühendus.

## <a name="conditional-access-prerequisites"></a>Juurdepääsu eeltingimused

- Azure Active Directory Premiumi tellimus
- Ühendatud või hallatavate Azure Active Directory rentniku jaoks
- Ühendatud rentnikud nõua, et mitmikautentimise (MFA) olema lubatud  
    ![Konfigureerige juurdepääsu reeglid – nõuavad](./media/active-directory-application-proxy-conditional-access/application-proxy-conditional-access.png)

## <a name="configure-per-application-multi-factor-authentication"></a>Rakenduse kohta mitmikautentimise konfigureerimine
1. Logige sisse administraatorina Azure klassikaline portaalis.
2. Avage Active Directory ja valige kaust, kus soovite lubada rakenduse puhverserver.
3. Klõpsake nuppu **rakendused** ja liikuge kerides jaotiseni **Juurdepääsureeglid** . Jaotises juurdepääsu reeglid kuvatakse ainult avaldatud rakenduse puhverserveri abil välise autentimise kasutavad rakendused.
4. Luba reegel, valides **Lubamine juurdepääsureeglid** **kohta**.
5. Määrata kasutajad ja rühmad, kellele reegleid rakendatakse. Valige üks või mitu rühma, millele juurdepääsu reegel kehtib nuppu **Lisa rühm** abil. Selle dialoogiboksi saate kasutada ka eemaldada valitud rühmad.  Reegleid valimisel rühmade rakendamiseks ainult jõustatakse juurdepääsu reeglid kasutajatele, kes kuuluvad ühte määratud turberühmad.  

  - Reeglit selgesõnaliselt välja jätta turberühmad, **peale** kontrollida ja määrata ühe või mitme rühmad. Kasutajad, kes kuuluvad rühma VA loendis ei pea mitmikautentimise teha.  

  - Kui kasutaja on konfigureeritud, kasutades funktsiooni kasutaja mitmikautentimise, see säte on ülimuslik rakenduse mitmikautentimise reeglid. See tähendab, et kasutaja, kes on konfigureeritud kasutaja mitmikautentimise jaoks on vaja teha mitmikautentimise isegi siis, kui nad on vabastatud rakenduse mitmikautentimise reeglid. Lisateavet [mitmikautentimise ja kasutaja sätted](../multi-factor-authentication/multi-factor-authentication.md).

6. Valige Accessi reegel, mille soovite seada.
    - **Nõua mitmikautentimise**: kasutajad, kelle juurdepääsureeglid suhtes on vaja täieliku mitmikautentimise enne juurdepääs rakenduse rakendatakse reegel.
    - **Nõua mitmikautentimise pole tööl**: ei peavad kasutajad vaadata rakenduse kaudu usaldusväärse IP-aadress mitmikautentimise sooritamiseks. Mitmikautentimise sätete lehel saab konfigureerida usaldusväärse IP-aadresside vahemikud.
    - **Kui pole tööl juurdepääsu**: juurdepääsul väljaspool oma ettevõtte võrku rakenduse kasutajad ei saa rakenduse avamiseks.


## <a name="configuring-mfa-for-federation-services"></a>MFA federation Services konfigureerimine
Jaoks ühendatud rentnikud, võib mitmikautentimise (MFA) läbi Azure Active Directory või asutusesisese AD FS server. Vaikimisi toimub MFA suvalisel lehel korraldaja Azure Active Directory. MFA kohapealse konfigureerimiseks käivitage Windows PowerShelli ja atribuudi – SupportsMFA abil saate määrata Azure AD moodul.

Järgmises näites kirjeldatakse, kuidas lubada kohapealse MFA rentnikus contoso.com [cmdlet-käsu Set-MsolDomainFederationSettings](https://msdn.microsoft.com/library/azure/dn194088.aspx) abil.`Set-MsolDomainFederationSettings -DomainName contoso.com -SupportsMFA $true `

Lisaks selle lipu, peab olema konfigureeritud väline rentniku AD FS-i eksemplari mitmikautentimise sooritamiseks. Järgige [Microsoft Azure'i mitmikautentimise kohapealse kasutamise](../multi-factor-authentication/multi-factor-authentication-get-started-server.md)kohta.


## <a name="see-also"></a>Vt ka

- [Nõuded rakendusi töötamine](active-directory-application-proxy-claims-aware-apps.md)
- [Rakenduse puhverserveri rakenduste avaldamine](active-directory-application-proxy-publish.md)
- [Ühekordse sisselogimise lubamiseks](active-directory-application-proxy-sso-using-kcd.md)
- [Avaldada oma domeeninime kasutavad rakendused](active-directory-application-proxy-custom-domains.md)

Uudiseid ja uusimate värskenduste, vaadake [rakenduse puhverserveri ajaveeb](http://blogs.technet.com/b/applicationproxyblog/)
