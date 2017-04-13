<properties
    pageTitle="Azure Active Directory juurdepääsu KKK | Microsoft Azure'i"
    description="Korduma kippuvad küsimused juurdepääsu kohta "
    services="active-directory"
    documentationCenter=""
    authors="MarkusVi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="markvi"/>

# <a name="azure-active-directory-conditional-access-faq"></a>Azure Active Directory juurdepääsu KKK

## <a name="which-applications-work-with-conditional-access-policies"></a>Millised rakendused töötada tingimusvormingu juurdepääsupoliitikaid?

**A:** Lugege teemat [tingimusvormingu Accessi-millised rakendused on toetatud](active-directory-conditional-access-supported-apps.md).

## <a name="are-conditional-access-policies-enforced-for-b2b-collaboration-and-guest-users"></a>Juurdepääsu poliitikate jõustamine B2B koostöö ja külalisi?

**A:** Poliitikate jõustamine B2B koostöö kasutajate jaoks. Kuid mõnel juhul kasutaja ei pruugi poliitika tingimusele vastavaks, kui näiteks ettevõtte ei toeta mitmikautentimise. 

Poliitika on praegu ei jõustatud SharePointi Külastajate kasutajate jaoks. SharePointi püsib Külastajate seost. Külalisena kasutajate kontod ei kehti Accessi poliitikat autentimise server. Külalisena juurdepääsu saab hallata SharePointi veebisaidil.

## <a name="does-a-sharepoint-online-policy-also-apply-to-onedrive-for-business"></a>Kas SharePoint Online'i poliitika rakendada ka OneDrive for Business?

**A:** Jah.
 
## <a name="why-cant-i-set-a-policy-on-client-apps-like-word-or-outlook"></a>Miks ei saa määrata poliitika kliendi rakendused, nagu Word või Outlooki?

**A:** Juurdepääsu poliitika nõuded teenuse kasutamiseks ja jõustamine kui autentimine juhtub selle teenuse. Poliitika on seatud otse sisse klientrakendusega; selle asemel rakendatakse, kui seda teenust sisse helistab. Näiteks SharePointi jaoks seatud poliitika kehtib klientide kutsumine SharePoint ja poliitika määramine Exchange'i kehtib Outlook.


## <a name="does-a-conditional-access-policy-apply-to-service-accounts"></a>Kas juurdepääsu poliitika rakendada teenuse konto?

**A:** Kõik kasutajakontod juurdepääsu poliitikaid rakendada. See hõlmab kasutajakontode kasutada kontod. Paljudel juhtudel, mis töötab järelevalveta teenusekonto ei saa poliitika vastavaks. See on näiteks juhul, kui on vaja MFA. Sellisel juhul saate teenuste kontod välistatud poliitika, kasutades juurdepääsu rühmapoliitika sätted. Lisateavet kasutajate siin poliitika rakendamist.
