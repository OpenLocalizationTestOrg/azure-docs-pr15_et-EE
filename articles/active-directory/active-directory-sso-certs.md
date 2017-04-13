<properties
    pageTitle="Kuidas hallata Federation serdid Azure AD | Microsoft Azure'i"
    description="Saate teada, kuidas kohandada sertide federation aegumiskuupäeva ja pikendamine aegub varsti serdid."
    services="active-directory"
    documentationCenter=""
    authors="asmalser-msft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/09/2016"
    ms.author="asmalser-msft"/>

#<a name="managing-certificates-for-federated-single-sign-on-in-azure-active-directory"></a>Sertide ühendatud ühekordse sisselogimise Azure Active Directory haldamine

Selles artiklis antakse ülevaade serdid, mille Azure Active Directory loob kindlakstegemiseks ühendatud Ühekordne sisselogimine (SSO) SaaS rakenduste seotud levinud küsimustele.

See artikkel kehtib ainult seotud rakendusi, mis on konfigureeritud kasutama **Azure AD ühekordse sisselogimise**, nagu on näidatud järgmises näites:

![Azure'i AD ühekordse sisselogimise](./media/active-directory-sso-certs/fed-sso.PNG)

##<a name="how-to-customize-the-expiration-date-for-your-federation-certificate"></a>Kuidas kohandada oma Federation sertifikaadi aegumiskuupäeva

Serdid on vaikimisi pärast kahte aastat. Saate valida mõne muu aegumiskuupäeva teie serdi järgides allolevaid juhiseid. Kaasatud kuvatõmmised kasutada Salesforce'i huvides näide, kuid neid juhiseid saate rakendada mõne ühendatud SaaS rakenduse.

1. Azure Active Directorys Kiirkäivituse rakenduse, klõpsake lehel **Konfigureeri ühekordse sisselogimise**kohta.

    ![Avage SSO konfiguratsiooniviisardi.](./media/active-directory-sso-certs/config-sso.png)

2. Valige **Azure AD ühekordse sisselogimise**ja seejärel klõpsake nuppu **edasi**.

3. Tippige väljale **Sisselogimise URL-i** rakenduse ja märkige ruut jaoks **ühendatud ühekordse sisselogimise jaoks kasutatava serdi konfigureerimine**. Klõpsake nuppu **edasi**.

    ![Azure'i AD ühekordse sisselogimise](./media/active-directory-sso-certs/new-app-config-sso.PNG)

4. Järgmisel lehel Valige **Genereeri uut serti**ja valige, kui kaua kehtib ka serdi soovite. Klõpsake nuppu **edasi**.

    ![Luua uut serti](./media/active-directory-sso-certs/new-app-config-cert.PNG)

5. Järgmiseks klõpsake **serdi alla laadida**. Serdi üleslaadimine kindla SaaS rakenduse kohta leiate teavet klõpsake nuppu **Kuva konfigureerimise juhised**.

    ![Laadige alla ja seejärel laadige üles sert](./media/active-directory-sso-certs/new-app-config-app.PNG)

6. Serti ei lubatud, kuni valitakse dialoogiboksi allosas ruut kinnituse ja vajutage Edasta.

##<a name="how-to-renew-a-certificate-that-will-soon-expire"></a>Kuidas uuendada ei aeguks kiiresti serti

Kasutajate jaoks pole oluline tööseisakute tohiks ideaalvariandis mitte pikendamise alltoodud juhiseid. Pildid kasutatud selles jaotises funktsioon Salesforce'i näide, kuid neid juhiseid saate rakendada mõne ühendatud SaaS rakenduse.

1. Azure Active Directorys Kiirkäivituse rakenduse, klõpsake lehe **Konfigureerimine ühekordse sisselogimise**kohta.

    ![Avage SSO konfiguratsiooniviisardi](./media/active-directory-sso-certs/renew-sso-button.PNG)

2. Dialoogiboksi esimesel lehel **Azure AD ühekordse sisselogimise** peaks olema juba valitud, nii et klõpsake nuppu **edasi**.

3. Teisel lehel, märkige ruut **Konfigureeri ühendatud ühekordse sisselogimise jaoks kasutatava serdi**kohta. Klõpsake nuppu **edasi**.

    ![Azure'i AD ühekordse sisselogimise](./media/active-directory-sso-certs/renew-config-sso.PNG)

4. Järgmisel lehel Valige **Genereeri uut serti**ja valige, kui kaua kehtib ka uut serti soovite. Klõpsake nuppu **edasi**.

    ![Luua uut serti](./media/active-directory-sso-certs/new-app-config-cert.PNG)

5. Klõpsake **serdi alla laadida**. Edukalt rewnew oma sert, peate tegema järgmised kaks toimingut:

    - Uue serdi üleslaadimine SaaS rakenduse ühekordse sisselogimise konfiguratsiooni ekraani. Saate teada, kuidas seda teha oma kindla SaaS rakenduse, klõpsake nuppu **Kuva konfigureerimise juhised**.

    - Azure AD, märkige ruut kinnituse dialoogiboks lubamiseks uut serti allosas ja klõpsake **järgmise** esitada.

    > [AZURE.IMPORTANT] Ühekordne sisselogimine rakendusse keelatakse hetkel kas üks neid kahte juhist on lõpule viidud, kuid aktiveeritakse uuesti kui teine etapp on lõpule viidud. Seetõttu tööseisakute minimeerimiseks palun valmistada tegemise toiminguid on lühike üksteisest aja jooksul.

    ![Laadige alla ja seejärel laadige üles sert](./media/active-directory-sso-certs/renew-config-app.PNG)

## <a name="related-articles"></a>Seotud artiklid

- [Artikli registri Rakendusehaldus Azure Active Directory 's](active-directory-apps-index.md)
- [Rakenduse access ja ühekordse sisselogimise Azure Active Directory](active-directory-appssoaccess-whatis.md)
- [SAML-põhiste ühekordse sisselogimise tõrkeotsing](active-directory-saml-debugging.md)
