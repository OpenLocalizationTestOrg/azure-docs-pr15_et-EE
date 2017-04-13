<properties
    pageTitle="Azure Active Directory B2C: Rakenduse registreerimine | Microsoft Azure'i"
    description="Kuidas registreeruda Azure Active Directory B2C rakenduse"
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
    ms.topic="get-started-article"
    ms.date="08/30/2016"
    ms.author="swkrish"/>


# <a name="azure-active-directory-b2c-register-your-application"></a>Azure Active Directory B2C: Register rakenduse

## <a name="prerequisite"></a>Nõutav

Koostamiseks rakendus, mida aktsepteerib tarbija registreerumise ja logige sisse, peate esmalt registreerima rakendus on Azure Active Directory B2C rentnik. Saada oma rentniku abil [loomine mõne Azure AD B2C rentniku](active-directory-b2c-get-started.md)kirjeldatud juhised. Kui olete kõik selle artikli juhiseid, on teil B2C funktsioonid tera oma Startboard kinnitatud.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="navigate-to-the-b2c-features-blade"></a>Liikuge tera B2C funktsioonid

Kui teil on kinnitatud teie Startboard B2C funktsioonid tera, näete kohe, kui logite [Azure portaali](https://portal.azure.com/) B2C rentniku üldadministraatorina tera.

Võite kasutada ka tera, klõpsates nuppu **Sirvi** ja seejärel **Azure AD B2C** Vasakpoolsel navigeerimispaanil [Azure portaali](https://portal.azure.com/).

> [AZURE.IMPORTANT] Peate olema üldadministraator B2C rentniku saama B2C funktsioonid tera juurde. Üldadministraator: teine rentnik või mis tahes rentniku kasutaja juurdepääs puudub.  Mida saab vahetada oma B2C rentniku abil rentniku vahetaja Azure portaali paremas ülanurgas.

## <a name="register-an-application"></a>Rakenduse registreerimine

1. Azure'i portaalis enne B2C funktsioonid, klõpsake nuppu **rakendused**.
2. Klõpsake nuppu **+ Lisa** tera ülaosas.
3. Sisestage rakenduse tarbijatele kirjeldavate rakenduse **nimi** . Näiteks võite sisestada "Contoso B2C app".
4. Kui kirjutate veebipõhine rakendus, tavalise **kaasata veebirakenduse / web API** aktiveerimine **Jah**. **Vasta URL-id** on lõpp-punktid, kus tulemuseks Azure AD B2C kõik märgid, mis taotleb rakenduse. Sisestage näiteks `https://localhost:44321/`. Kui teie veebirakenduse ka helistamine mõned veebi-API, mis on tagatud Azure AD B2C, peaksite loomiseks on **Rakenduse salajane** ka, klõpsates nuppu **Loo võti** .

    > [AZURE.NOTE] Mõne **Rakenduse salajane** on mõni oluline turvalisuse mandaati ja õigesti turvatud.

5. Kui kirjutate mobiilirakendust, tavalise **kaasamine omakliendi** aktiveerimine **Jah**. Vaikimisi **URI ümber suunata** , mis on teie jaoks automaatselt loodud allapoole kopeerida.
6. Klõpsake nuppu **Loo** rakenduse registreerimiseks.
7. Klõpsake äsja loodud rakenduse peate kasutama hiljem koodis globaalselt kordumatu **rakenduse kliendi ID** allapoole kopeerida.

> [AZURE.IMPORTANT] Rakenduste loodud B2C funktsioonid tera on samas kohas hallata. Kui redigeerite B2C rakenduste PowerShelli või mõne muu portaalis need muutuvad toetamata ja tõenäoliselt Azure AD B2C ei tööta.

## <a name="build-a-quick-start-application"></a>Lühijuhend rakenduse koostamine

Nüüd, kui teil on rakenduse Azure AD B2C registreeritud, saate täita meie kiire alustada õpetused tööks. Siin on mõned soovitused.

[AZURE.INCLUDE [active-directory-v2-quickstart-table](../../includes/active-directory-b2c-quickstart-table.md)]
