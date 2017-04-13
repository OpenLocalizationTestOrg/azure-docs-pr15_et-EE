<properties
    pageTitle="Azure Active Directory B2C: Microsofti konto konfigureerimine | Microsoft Azure'i"
    description="Sisestage tarbijatele oma rakendustes, mis on kaitstud Azure Active Directory B2C Microsofti kontoga sisse logida ja logige sisse."
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
    ms.date="07/24/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-microsoft-accounts"></a>Azure Active Directory B2C: Annavad registreerumise ja logige sisse Microsofti kontoga tarbijatele

## <a name="create-a-microsoft-account-application"></a>Microsofti konto rakenduse loomine

Identiteedi pakkuja Azure Active Directory (Azure AD) B2C kasutada Microsofti kontot, peate rakenduse Microsoft konto loomine ja pakkuda õige parameetrid. Peate selleks Microsofti kontoga. Kui teil ei ole üks, saad [https://www.live.com/](https://www.live.com/)juures.

1. [Microsofti rakendus registreerimise portaali](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) ja logige sisse oma Microsofti konto identimisteave.
2. Klõpsake käsku **Lisa rakendus**.

    ![Microsofti konto - uue rakenduse lisamine](./media/active-directory-b2c-setup-msa-app/msa-add-new-app.png)

3. Rakenduse **nimi** ja klõpsake nuppu **Loo rakenduse**.

    ![Microsofti konto - rakenduse nimi](./media/active-directory-b2c-setup-msa-app/msa-app-name.png)

4. Kopeerige **Rakenduse Id**väärtus. Peate selle konfigureerida identiteedi pakkuja oma rentniku Microsofti kontoga.

    ![Microsofti konto - rakenduse Id](./media/active-directory-b2c-setup-msa-app/msa-app-id.png)

5. Klõpsake nuppu **Lisa** platvormi ja valige **Web**.

    ![Microsofti konto - platvormi lisamine](./media/active-directory-b2c-setup-msa-app/msa-add-platform.png)

    ![Microsofti konto - Web](./media/active-directory-b2c-setup-msa-app/msa-web.png)

6. Sisestage `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` välja **Ümber suunata URI-d** . **{Rentniku}** asendada oma rentniku nimi (nt contosob2c.onmicrosoft.com).

    ![Microsofti konto - URL-i suunamine](./media/active-directory-b2c-setup-msa-app/msa-redirect-url.png)

7. Klõpsake jaotises **Rakenduse saladusi** **Luua uus parool** . Kopeerige ekraanil kuvatakse uus parool. Peate selle konfigureerida identiteedi pakkuja oma rentniku Microsofti kontoga. See parool on mõni oluline turvalisuse mandaati.

    ![Microsofti konto - luua uus parool](./media/active-directory-b2c-setup-msa-app/msa-generate-new-password.png)

    ![Microsofti konto - uus parool](./media/active-directory-b2c-setup-msa-app/msa-new-password.png)

8. Märkige ruut, et **Live SDK toetab** jaotises **Täpsemad suvandid** . Klõpsake nuppu **Salvesta**.

    ![Microsofti konto - Live SDK tugi](./media/active-directory-b2c-setup-msa-app/msa-live-sdk-support.png)

## <a name="configure-microsoft-account-as-an-identity-provider-in-your-tenant"></a>Microsofti konto nimega identiteedi pakkuja oma rentniku konfigureerimine

1. Järgmiste juhiste liikumiseks [B2C funktsioonid tera](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) Azure'i portaalis.
2. Enne B2C funktsioone, klõpsake **identiteedipakkujad**.
3. Klõpsake nuppu **+ Lisa** tera ülaosas.
4. Identiteedi pakkuja konfiguratsiooni sõbralik **nimi** ette. Sisestage näiteks "MSA".
5. Klõpsake **identiteedi pakkuja tüüp**, valige **Microsofti konto**ja klõpsake nuppu **OK**.
6. Klõpsake **selle identiteedipakkuja häälestamine** ja sisestage rakenduse Id ja parool Microsofti konto taotluse varem loodud.
7. Klõpsake nuppu **OK** ja seejärel klõpsake nuppu **Loo** salvestada oma Microsofti konto konfigureerimine.
