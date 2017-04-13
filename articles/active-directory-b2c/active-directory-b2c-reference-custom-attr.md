<properties
    pageTitle="Azure Active Directory B2C: Kohandatud atribuute | Microsoft Azure'i"
    description="Kuidas kasutada kohandatud atribuute Azure Active Directory B2C tarbijatele kohta teavet koguda"
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

#  <a name="azure-active-directory-b2c-use-custom-attributes-to-collect-information-about-your-consumers"></a>Azure Active Directory B2C: Kohandatud atribuutide abil tarbijatele kohta teavet koguda

Azure Active Directory (Azure AD) B2C kataloogi kaasas valmiskomplekti teabe (Atribuudid): eesnimi, perekonnanimi, linn, sihtnumber ja muud atribuudid. Siiski iga tarbija suunatud taotlus on kordumatu nõuete kohta, millised atribuudid tarbijate koguda. Azure'i AD B2C, kus saate laiendada atribuute, mis on talletatud iga tavakasutaja konto määramine. Saate luua kohandatud atribuute [Azure portaali](https://portal.azure.com/) ja seda kasutada oma registreerumise poliitikad, nagu allpool näidatud. Saate lugeda ja kirjutada need atribuudid [Azure AD Graph API](active-directory-b2c-devquickstarts-graph-dotnet.md)abil.

> [AZURE.NOTE]
Kohandatud atribuute kasutada [Azure AD Graph API Directory skeemi laiendid](https://msdn.microsoft.com/library/azure/dn720459.aspx).

## <a name="create-a-custom-attribute"></a>Luua kohandatud atribuut

1. [Järgmiste juhiste abil liikuge B2C funktsioonid tera Azure'i portaalis](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Klõpsake **kasutaja atribuudid**.
3. Klõpsake nuppu **+ Lisa** tera ülaosas.
4. Sisestage **nimi** kohandatud atribuut (nt "ShoeSize") ja soovi korral **Kirjeldus**. Klõpsake nuppu **Loo**.

    > [AZURE.NOTE]
    Ainult "String" **Andmetüüp** on praegu saadaval.

Kohandatud atribuut on nüüd saadaval loendis **kasutaja**atribuutide ja kasutada oma registreerumise poliitika.

## <a name="use-a-custom-attribute-in-your-sign-up-policy"></a>Kasutage kohandatud atribuudi teie registreerumise poliitika

1. [Järgmiste juhiste abil liikuge B2C funktsioonid tera Azure'i portaalis](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade).
2. Klõpsake **registreerumise poliitika**.
3. Klõpsake oma registreerumise poliitika (nt "B2C_1_SiUp") selle avamiseks. Klõpsake nuppu **Redigeeri** tera ülaosas.
4. Klõpsake **registreerumise atribuudid** ja valige Kohandatud atribuut (nt "ShoeSize"). Klõpsake nuppu **OK**.
5. Klõpsake **rakenduse nõuded** ja valige kohandatud atribuuti. Klõpsake nuppu **OK**.
6. Klõpsake nuppu **Salvesta** tera ülaosas.

Saate kasutada funktsiooni "Käivita kohe" poliitika tarbijate kogemusi kinnitamiseks. Nüüd peaks loendis tarbija registreerumise käigus kogutud atribuudid "ShoeSize" näha ja vaadake seda tagasi saadetakse teie taotlus luba.

## <a name="notes"></a>Märkmete

- Koos registreerumise poliitikad, kohandatud atribuute saate kasutada ka registreerumise või sisselogimise poliitika ja kasutajaprofiili poliitikate redigeerimine.
- Pole teada piiramist kohandatud atribuute. See on loodud ainult esimest korda, kui seda kasutatakse igal poliitika ja pole **kasutaja atribuutide**loendis lisamist.
