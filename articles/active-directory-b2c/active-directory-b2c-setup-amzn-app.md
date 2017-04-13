<properties
    pageTitle="Azure Active Directory B2C: Amazon konfiguratsiooni | Microsoft Azure'i"
    description="Annavad tarbijatele oma rakendustes, mis on kaitstud Azure Active Directory B2C Amazon kontoga sisse logida ja logige sisse."
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

# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-amazon-accounts"></a>Azure Active Directory B2C: Annavad registreerumise ja logige sisse tarbijatele Amazon kontode

## <a name="create-an-amazon-application"></a>Amazon rakenduse loomine

Identiteedi pakkuja Azure Active Directory (Azure AD) B2C Amazon kasutamiseks peate Amazon rakenduse loomine ja pakkuda õige parameetrid. Teil on vaja seda teha Amazon kontot. Kui teil ei ole üks, saad [http://www.amazon.com/](http://www.amazon.com/)juures.

1. Minge [Amazon Arenduskeskus](https://login.amazon.com/) ja logige sisse oma Amazon konto identimisteave.
2. Kui te pole seda juba teinud, nuppu **Liitu**, järgige arendaja registreerimise juhiseid ja nõustuge poliitika.
3. Klõpsake nuppu **Registreeri uus rakendus**.

    ![Amazon veebisaidil uue rakenduse registreerimisel](./media/active-directory-b2c-setup-amzn-app/amzn-new-app.png)

4. Rakenduse teave (**nimi**, **Kirjeldus**ja **Privaatsuse teade URL-i**) ja klõpsake nuppu **Salvesta**.

    ![Rakenduse teabe uue rakenduse, mis Amazon registreerusite](./media/active-directory-b2c-setup-amzn-app/amzn-register-app.png)

5. Kopeerige **Veebi sätted** jaotises **Kliendi ID** ja **Kliendi salajane**väärtused. (Peate selle kuvamiseks nuppu **Kuva salajane** .) Peate konfigureerima Amazon identiteedi pakkuja oma rentniku mõlemad. Klõpsake alumises osas jaotise **redigeerimine** . **Kliendi salajane** on mõni oluline turvalisuse mandaati.

    ![Kliendi ID ja kliendi salajane uue rakenduse mis Amazon](./media/active-directory-b2c-setup-amzn-app/amzn-client-secret.png)

6. Sisestage `https://login.microsoftonline.com` **Lubatud JavaScripti päritolu** väli ja `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` **Lubatud tagastada URL** väljale. **{Rentniku}** asendada oma rentniku nimi (nt contoso.onmicrosoft.com). Klõpsake nuppu **Salvesta**. **{Rentniku}** väärtus on tõstutundlikud.

    ![Esitada oma uuele rakendusele, mis Amazon JavaScripti päritolu ja tagastada URL-id](./media/active-directory-b2c-setup-amzn-app/amzn-urls.png)

## <a name="configure-amazon-as-an-identity-provider-in-your-tenant"></a>Kui teie rentnik identiteedi pakkuja Amazon konfigureerimine

1. Järgmiste juhiste liikumiseks [B2C funktsioonid tera](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) Azure'i portaalis.
2. Enne B2C funktsioone, klõpsake **identiteedipakkujad**.
3. Klõpsake nuppu **+ Lisa** tera ülaosas.
4. Identiteedi pakkuja konfiguratsiooni sõbralik **nimi** ette. Sisestage näiteks "Amzn".
5. Klõpsake **identiteedi pakkuja tüüp**, valige **Amazon**ja klõpsake nuppu **OK**.
6. Klõpsake **selle identiteedipakkuja häälestamine** ja sisestage kliendi ID ja kliendi salajane Amazon taotluse varem loodud.
7. Klõpsake nuppu **OK** ja seejärel klõpsake nuppu **Loo** salvestada oma Amazon konfigureerimine.
