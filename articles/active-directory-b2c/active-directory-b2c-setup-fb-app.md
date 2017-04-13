<properties
    pageTitle="Azure Active Directory B2C: Facebooki konfiguratsiooni | Microsoft Azure'i"
    description="Sisestage tarbijatele Facebooki kontode oma rakendustes, mis on kaitstud Azure Active Directory B2C registreerumise ja Logi sisse."
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

# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-facebook-accounts"></a>Azure Active Directory B2C: Pakuvad tarbijatele Facebooki kontode registreerumise ja sisselogimine

## <a name="create-a-facebook-application"></a>Facebooki rakenduse loomine

Identiteedi pakkuja Azure Active Directory (Azure AD) B2C Facebooki kasutamiseks peate Facebooki rakenduse loomine ja pakkuda õige parameetrid. Vajate selleks Facebooki kontot. Kui teil ei ole üks, saad [https://www.facebook.com/](https://www.facebook.com/)veebisaidil.

1. Minge veebisaidile [Facebooki arendajatele](https://developers.facebook.com/) ja logige sisse oma Facebooki konto identimisteave.
2. Kui te pole seda juba teinud, peate registreeruma Facebooki arendaja. Selleks klõpsake **registreerida** (lehe paremasse nurka), aktsepteerida Facebook poliitikate ja registreerimise juhiseid.
3. Klõpsake nuppu **Minu rakendused** ja seejärel klõpsake nuppu **Lisa uus rakendus**. Valige **veebisaidi** platvorm ja klõpsake **Jäta ja luua rakenduse ID-d**.

    ![Facebooki - uue rakenduse lisamine](./media/active-directory-b2c-setup-fb-app/fb-add-new-app.png)

    ![Facebooki - uue rakenduse - veebisaidi lisamine](./media/active-directory-b2c-setup-fb-app/fb-add-new-app-website.png)

    ![Facebooki - rakenduse ID loomine](./media/active-directory-b2c-setup-fb-app/fb-new-app-skip.png)

4. Vormil pakkuda **Kuvatav nimi**, kehtiv **Kontakti e-posti**, õige **kategooria**ja klõpsake **rakenduse loomine ID-d**. Selleks, et teie Facebooki platvormi poliitikate aktsepteerimiseks ja täitke mõne online turvalisus sisse.

    ![Facebooki - uue rakenduse ID](./media/active-directory-b2c-setup-fb-app/fb-create-app-id.png)

5. Klõpsake vasakpoolsel navigeerimispaanil nuppu **sätted** .
6. Klõpsake nuppu **+ Lisa platvormi** ja seejärel valige **veebisaidi**.

    ![Facebooki - sätted](./media/active-directory-b2c-setup-fb-app/fb-settings.png)

    ![Facebooki - sätted – veebisait](./media/active-directory-b2c-setup-fb-app/fb-website.png)

7. Sisestage [https://login.microsoftonline.com/](https://login.microsoftonline.com/) **Saidi URL** väljale ja seejärel klõpsake käsku **Salvesta muudatused**.

    ![Facebooki - saidi URL-i](./media/active-directory-b2c-setup-fb-app/fb-site-url.png)

8. Kopeerige **Rakenduse ID**väärtus. Klõpsake nuppu **Kuva** ja kopeerige **Rakenduse salajane**väärtus. Peate konfigureerima Facebooki identiteedi pakkuja oma rentniku mõlemad. **Rakenduse salajane** on mõni oluline turvalisuse mandaati.

    ![Facebooki - rakenduse ID & rakenduse salajane](./media/active-directory-b2c-setup-fb-app/fb-app-id-app-secret.png)

9. Klõpsake nuppu **+ Lisa toote** vasakpoolsel navigeerimisribal ja seejärel **Facebook Login**kõrval nuppu **Alustamine** .

    ![Facebooki - Facebook Login](./media/active-directory-b2c-setup-fb-app/fb-login.png)

10. Sisestage `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` **Klientrakenduse OAuthi sätted** jaotise väljal **kehtiv OAuthi ümber suunata URI-d** . **{Rentniku}** asendada oma rentniku nimi (nt contosob2c.onmicrosoft.com). Klõpsake nuppu **Salvesta muudatused** lehe allosas.

    ![Facebooki - OAuthi Redirect URI](./media/active-directory-b2c-setup-fb-app/fb-oauth-redirect-uri.png)

11. Facebooki rakenduse kasutatavaks Azure AD B2C tegemiseks peate avalikult kättesaadavaks teha. Saate seda teha, klõpsates menüü **Läbivaatus rakenduse** Vasakpoolsel navigeerimispaanil ja lülitage parameetrit **Jah** lehe ülaosas ja klõpsake nuppu **Kinnita**.

    ![Facebooki - rakenduse avaliku](./media/active-directory-b2c-setup-fb-app/fb-app-public.png)

## <a name="configure-facebook-as-an-identity-provider-in-your-tenant"></a>Facebooki nimega identiteedi pakkuja oma rentniku konfigureerimine

1. Järgmiste juhiste liikumiseks [B2C funktsioonid tera](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) Azure'i portaalis.
2. Enne B2C funktsioone, klõpsake **identiteedipakkujad**.
3. Klõpsake nuppu **+ Lisa** tera ülaosas.
4. Identiteedi pakkuja konfiguratsiooni sõbralik **nimi** ette. Sisestage näiteks "FB".
5. Klõpsake **identiteedi pakkuja tüüp**, valige **Facebooki**ja klõpsake nuppu **OK**.
6. Klõpsake **selle identiteedipakkuja häälestamine** ja rakenduse ID ja rakenduse salajane (varem loodud Facebooki), sisestage **Kliendi ID** ja **Kliendi salajane** väljade vastavalt.
7. Klõpsake nuppu **OK**ja seejärel klõpsake nuppu **Loo** salvestada oma Facebooki konfigureerimine.
