<properties
    pageTitle="Azure Active Directory B2C: LinkedIni konfiguratsiooni | Microsoft Azure'i"
    description="Annavad registreerumise ja logige sisse oma rakendustes, mis on kaitstud Azure Active Directory B2C LinkedIni kontode tarbijatele"
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

# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-linkedin-accounts"></a>Azure Active Directory B2C: Pakuvad tarbijatele LinkedIni kontode registreerumise ja sisselogimine

## <a name="create-a-linkedin-application"></a>LinkedIni rakenduse loomine

Identiteedi pakkuja Azure Active Directory (Azure AD) B2C LinkedIni kasutamiseks peate LinkedIni rakenduse loomine ja pakkuda õige parameetrid. Peate LinkedIni konto seda teha. Kui teil ei ole üks, saad [https://www.linkedin.com/](https://www.linkedin.com/)juures.

1. [Veebilehel LinkedIni arendajad](https://www.developer.linkedin.com/) ja logige sisse oma LinkedIni konto identimisteave.
2. Klõpsake ülemisel menüüribal nuppu **Minu rakendused** ja seejärel nuppu **Rakendus loomine**.

    ![LinkedIni - uus rakendus](./media/active-directory-b2c-setup-li-app/linkedin-new-app.png)

3. Vormi **Loo uus rakendus** , sisestage asjakohane teave (**Ettevõtte nimi**, **nimi**, **Kirjeldus**, **Logo URL-i**, **Rakenduse kasutamine**, **Veebisaidi URL-i**, **Business e-posti** ja **Töötelefon**).
4. **LinkedIni API kasutustingimustega** nõustuma ja klõpsake nuppu **Edasta**.

    ![LinkedIni - Register rakendus](./media/active-directory-b2c-setup-li-app/linkedin-register-app.png)

5. Kopeerige **Kliendi ID** ja **Kliendi salajane**väärtused. (Leiate need jaotises **Autentimine võtmed**.) Peate konfigureerima LinkedIni identiteedi pakkuja oma rentniku mõlemad.

    >[AZURE.NOTE] **Kliendi salajane** on mõni oluline turvalisuse mandaati.

6. Sisestage `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` välja **Volitatud ümber suunata URL-id** (jaotises **OAuth 2.0**). **{Rentniku}** asendada oma rentniku nimi (nt contoso.onmicrosoft.com). Klõpsake nuppu **Lisa**ja seejärel klõpsake nuppu **Värskenda**. **{Rentniku}** väärtus on tõstutundlikud.

    ![LinkedIni - häälestamise rakendus](./media/active-directory-b2c-setup-li-app/linkedin-setup.png)

## <a name="configure-linkedin-as-an-identity-provider-in-your-tenant"></a>LinkedIni nimega identiteedi pakkuja oma rentniku konfigureerimine

1. Järgmiste juhiste liikumiseks [B2C funktsioonid tera](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade) Azure'i portaalis.
2. Enne B2C funktsioone, klõpsake **identiteedipakkujad**.
3. Klõpsake nuppu **+ Lisa** tera ülaosas.
4. Identiteedi pakkuja konfiguratsiooni sõbralik **nimi** ette. Sisestage näiteks "LI".
5. Klõpsake **identiteedi pakkuja tüüp**, valige **LinkedIni**ja klõpsake nuppu **OK**.
6. Klõpsake **selle identiteedipakkuja häälestamine** ja sisestage kliendi ID ja kliendi salajane LinkedIni rakenduse varem loodud.
7. Klõpsake nuppu **OK** ja seejärel klõpsake nuppu **Loo** salvestada oma LinkedIni konfigureerimine.
