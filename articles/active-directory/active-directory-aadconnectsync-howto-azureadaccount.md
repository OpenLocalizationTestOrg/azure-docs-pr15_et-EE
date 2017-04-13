<properties
    pageTitle="Azure'i AD-ühendus sünkroonimine: Azure'i AD-teenuse konto haldamine | Microsoft Azure'i"
    description="Selles teemas dokumentide teenusekonto Azure AD taastamise kohta."
    services="active-directory"
    keywords="AADSTS70002, AADSTS50054, Azure'i AD-ühenduse sünkroonimine konnektori teenuse konto parooli lähtestamine"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/01/2016"
    ms.author="billmath"/>

# <a name="azure-ad-connect-sync-how-to-manage-the-azure-ad-service-account"></a>Azure'i AD-ühendus sünkroonimine: Azure'i AD-teenuse konto haldamine
Kasutatav Azure AD konnektor teenuse konto peaks olema tasuta teenus. Kui teil on vaja lähtestada oma mandaat, siis see teema on teie jaoks. Näiteks kui Üldadministraatoril on viga lähtestada PowerShelli kaudu teenuse konto parooli.

## <a name="reset-the-credentials"></a>Mandaadi lähtestamine
Kui määratletud Azure AD konnektor teenusekonto ei saa ühendust Azure AD tõttu autentimise probleeme, saate parooli lähtestada.

1. Azure'i AD-ühenduse server sünkroonimine sisse logida ja käivitage PowerShelli.
2. Käivitage `Add-ADSyncAADServiceAccount`.  
![PowerShelli cmdlet-käsk addadsyncaadserviceaccount](./media/active-directory-aadconnectsync-howto-azureadaccount/addadsyncaadserviceaccount.png)
3. Azure AD üldadministraator mandaat.

Selle cmdlet-käsu taastab teenuse konto parooli ja värskendage seda Azure AD ja sync engine.

## <a name="known-issues-these-steps-can-solve"></a>Teadaolevad probleemid lahendada järgmiselt.
Selles jaotises on tõrkeid kliendid, mis on määratud identimisteave, mis on Azure AD teenusekonto lähtestada loendit.

-----------
Sündmuse 6900  
Server ilmnes ootamatu tõrge parooli muutmine teatise töötlemine:  
AADSTS70002: Tõrke sisseseatud mandaat. AADSTS50054: Autentimiseks kasutatav vana parool.

----------
Sündmuse 659  
Tõrge parooli sünkroonimise konfiguratsioon toomisel. Microsoft.IdentityModel.Clients.ActiveDirectory.AdalServiceException:  
AADSTS70002: Tõrke sisseseatud mandaat. AADSTS50054: Autentimiseks kasutatav vana parool.

## <a name="next-steps"></a>Järgmised sammud

**Teemade ülevaade**

- [Azure'i AD-ühendus sünkroonimine: Kuupäevatabelid ja nende sünkroonimine kohandamine](active-directory-aadconnectsync-whatis.md)
- [Teie kohapealse identiteetide integreerimine Azure Active Directory](active-directory-aadconnect.md)
