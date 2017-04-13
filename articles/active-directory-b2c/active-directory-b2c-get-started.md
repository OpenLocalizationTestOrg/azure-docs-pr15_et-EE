<properties
    pageTitle="Azure Active Directory B2C: Loomine Azure Active Directory B2C rentnikku | Microsoft Azure'i"
    description="Kuidas luua on Azure Active Directory B2C rentnik teema"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.topic="article"
    ms.devlang="na"
    ms.date="08/30/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-create-an-azure-ad-b2c-tenant"></a>Azure Active Directory B2C: Luua mõne Azure AD B2C rentniku

Microsoft Azure Active Directory (Azure AD) B2C kasutamise alustamiseks järgige kolme käesolevas artiklis toodud juhiseid.

## <a name="step-1-sign-up-for-an-azure-subscription"></a>Samm 1: Azure'i tellimuse kasutajaks

Kui teil on juba Azure'i tellimus, jätke see juhis vahele. Kui ei, [Azure tellimuse](../active-directory/sign-up-organization.md) kasutajaks registreerumine ja Azure AD B2C juurdepääs.

## <a name="step-2-create-an-azure-ad-b2c-tenant"></a>Samm 2: Looge on Azure AD B2C rentniku

Järgmiste juhiste abil saate luua uue Azure AD B2C rentniku. Praegu ei B2C funktsioonid sisse lülitada sisse oma olemasoleva rentnikke.

1. Logige sisse [Azure klassikaline portaali](https://manage.windowsazure.com/) tellimuse administraatorina. See on sama töö või kooli kontoga või sama Microsofti kontoga sisselogimiseks Azure kasutatud.
2. Klõpsake nuppu **Uus** > **rakenduse teenuste** > **Active Directory** > **Directory** > **kohandatud loomine**.

    ![Kuvatõmmis – rentniku loomine hakanud](./media/active-directory-b2c-get-started/new-directory.png)

3. Valige oma rentniku **nimi**, **Domeeni nimi** ja **riik või regioon** .
4. Märkige ruut, mis ütleb, et **see on B2C directory**.
5. Klõpsake toimingu lõpuleviimiseks.

    ![Kuvatõmmis märge luua B2C kataloog](./media/active-directory-b2c-get-started/create-b2c-directory.png)

6. Teie rentnikukontoga on loodud ja kuvatakse Active Directory laiendada. Tegite ka rentniku üldadministraator. Saate lisada muid globaalne administraatorite vastavalt vajadusele.

    > [AZURE.IMPORTANT]
    Kui kavatsete kasutada B2C rentniku tootmise rakenduse, lugege artiklit [tootmisvõimsust vs eelvaade B2C rentnikud](active-directory-b2c-reference-tenant-type.md)kohta. Pange tähele, et on teadaolevad probleemid, kui kustutate mõne olemasoleva B2C rentniku ja uuesti luua selle domeeni nimi. Teil on muu domeeninimega B2C rentniku loomine.

## <a name="step-3-navigate-to-the-b2c-features-blade-on-the-azure-portal"></a>Samm 3: Liikuge B2C funktsioonid tera Azure'i portaalis

1. Liikuge Active Directory laiend navigeerimisriba vasakul pool.
2. Otsige üles oma rentniku vahekaardil **Directory** ja klõpsake seda.
3. Klõpsake vahekaarti **konfigureerimine** .
4. **B2C** administreerimine linki **Halda B2C sätted** .

    ![Kuvatõmmis directory B2C konfigureerimine](./media/active-directory-b2c-get-started/b2c-directory-configure-tab.png)

5. Azure'i portaalis B2C funktsioonid tera nähtaval avatakse uus brauserivahekaart või aknas.

    > [AZURE.IMPORTANT]
    Võib kuluda kuni 2 – 3 minutit oma rentniku oleks juurdepääs Azure'i portaalis. Pärast tükk aega parandamiseks proovitakse uuesti järgmisi juhiseid. Kui ei, võtke ühendust toega.

6. Kinnitage oma Startboard selle tera hõlpsaks juurdepääsuks. (PIN-koodi tööriista on märgitud punase funktsioonid tera ülemises parempoolses nurgas.)

    ![Kuvatõmmis tiibmutri B2C funktsioonid](./media/active-directory-b2c-get-started/b2c-features-blade.png)

    > [AZURE.NOTE]
    Saate hallata kasutajad ja rühmad, Iseteeninduslik parooli lähtestamine konfigureerimine ja ettevõtte tootemargi funktsioonid oma rentniku [Azure klassikaline portaalis](https://manage.windowsazure.com/).

## <a name="easy-access-to-the-b2c-features-blade-on-the-azure-portal"></a>Hõlpsalt juurde B2C funktsioonid tera Azure'i portaalis

Et parandada leitavust, oleme lisanud otsetee B2C funktsioonid tera Azure'i portaalis.

1. Teie B2C rentniku üldadministraatorina Azure portaali sisse logida. Kui teil on juba loginud sisse eri rentniku, lülitage rentnikke (ülemises paremas nurgas).
2. Klõpsake vasakpoolsel navigeerimispaanil nuppu **Sirvi** .
3. Klõpsake **Azure AD B2C** B2C funktsioonid tera juurdepääsu.

    ![Kuvatõmmis Sirvi B2C funktsioonid tera](./media/active-directory-b2c-get-started/b2c-browse.png)

## <a name="next-steps"></a>Järgmised sammud

Saate teada, kuidas registreerida rakenduse Azure AD B2C ja koostamiseks Kiirkäivituse rakenduse abil lugemise [Azure Active Directory B2C: registreerida rakenduse](active-directory-b2c-app-registration.md).
