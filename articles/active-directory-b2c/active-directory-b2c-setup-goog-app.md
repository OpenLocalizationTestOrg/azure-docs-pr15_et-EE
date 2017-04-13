<properties
    pageTitle="Azure Active Directory B2C: Google + konfiguratsiooni | Microsoft Azure'i"
    description="Sisestage Google + kontod oma rakendustes, mis on kaitstud Azure Active Directory B2C tarbijatele registreerumise ja Logi sisse."
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

# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-google-accounts"></a>Azure Active Directory B2C: Annavad registreerumise ja logige sisse tarbijatele Google + kontod

## <a name="create-a-google-application"></a>Google + rakenduse loomine

Kasutate Google + identiteedi pakkuja Azure Active Directory (Azure AD) B2C, peate Google'i + rakenduse loomine ja pakkuda õige parameetrid. Peate Google + konto seda teha. Kui teil ei ole üks, saad [https://accounts.google.com/SignUp](https://accounts.google.com/SignUp)juures.

1. Minge [Google arendajatele Console](https://console.developers.google.com/) ja logige sisse oma Google + konto identimisteave.
2. Klõpsake nuppu **Loo projekt**, sisestage **projekti nime**ja seejärel klõpsake nuppu **Loo**.

    ![Google + – alustamine](./media/active-directory-b2c-setup-goog-app/google-get-started.png)

    ![Google + - uue projekti](./media/active-directory-b2c-setup-goog-app/google-new-project.png)

3. Klõpsake **API haldur** ja seejärel käsku **identimisteabe** vasakpoolsel navigeerimisribal.
4. Klõpsake vahekaarti **OAuthi nõusoleku ekraani** ülaosas.

    ![Google + - identimisteave](./media/active-directory-b2c-setup-goog-app/google-add-cred.png)

5. Valige või määrake sobiv **e-posti aadressi**, **toote nimi**ja klõpsake nuppu **Salvesta**.

    ![Google + - OAuthi nõusolekut Kuva](./media/active-directory-b2c-setup-goog-app/google-consent-screen.png)

6. Klõpsake **uue mandaadi** ja seejärel valige **OAuthi kliendi ID**.

    ![Google + - OAuthi nõusolekut Kuva](./media/active-directory-b2c-setup-goog-app/google-add-oauth2-client-id.png)

7. Valige jaotises **Tüüp** **veebirakenduse**.

    ![Google + - OAuthi nõusolekut Kuva](./media/active-directory-b2c-setup-goog-app/google-web-app.png)

8. Pakkuda oma rakenduse **nimi** , sisestage `https://login.microsoftonline.com` väljale **Luba JavaScript päritolu** ja `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` välja **autoriseeritud ümber suunata URI-d** . **{Rentniku}** asendada oma rentniku nimi (nt contosob2c.onmicrosoft.com). **{Rentniku}** väärtus on tõstutundlikud. Klõpsake nuppu **Loo**.

    ![Google + - kliendi ID loomine](./media/active-directory-b2c-setup-goog-app/google-create-client-id.png)

9. Kopeerige **Kliendi ID** ja **Kliendi salajane**väärtused. Peate konfigureerima Google + identiteedi pakkuja oma rentniku mõlemad. **Kliendi salajane** on mõni oluline turvalisuse mandaati.

    ![Google + - kliendi salajane](./media/active-directory-b2c-setup-goog-app/google-client-secret.png)

## <a name="configure-google-as-an-identity-provider-in-your-tenant"></a>Konfigureerida Google + oma rentniku identiteedi pakkuja

1. Järgmiste juhiste liikumiseks [B2C funktsioonid tera](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) Azure'i portaalis.
2. Enne B2C funktsioone, klõpsake **identiteedipakkujad**.
3. Klõpsake nuppu **+ Lisa** tera ülaosas.
4. Identiteedi pakkuja konfiguratsiooni sõbralik **nimi** ette. Näiteks sisestage "G +".
5. Klõpsake **identiteedi pakkuja tüüp**, valige **Google**ja klõpsake nuppu **OK**.
6. Klõpsake **selle identiteedipakkuja häälestamine** ja sisestage kliendi ID ja kliendi salajane Google + rakenduse varem loodud.
7. Klõpsake nuppu **OK** ja seejärel klõpsake nuppu **Loo** salvestada oma Google + konfigureerimine.
