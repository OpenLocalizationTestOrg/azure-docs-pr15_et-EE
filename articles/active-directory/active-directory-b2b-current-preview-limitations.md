<properties
   pageTitle="Praegune eelvaade piirangud Azure Active Directory B2B koostöö jaoks | Microsoft Azure'i"
   description="Azure Active Directory B2B toetab oma rist – ettevõtte seoseid, võimaldades äripartneritega valikuliselt juurdepääsu oma ettevõtte rakendused"
   services="active-directory"
   documentationCenter=""
   authors="viv-liu"
   manager="cliffdi"
   editor=""
   tags=""/>

<tags
   ms.service="active-directory"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="identity"
   ms.date="05/09/2016"
   ms.author="viviali"/>

# <a name="azure-ad-b2b-collaboration-preview-current-preview-limitations"></a>Azure'i AD B2B koostöö eelvaade: praegune eelvaate piirangud

- Mitmikautentimise (MFA) ei toeta välised kasutajad. Näiteks kui Contoso on MFA, kuid partneri organisatsiooniskeemi ei, siis partneri organisatsiooniskeemi kasutajad ei saa anda MFA B2B koostöö kaudu.
- Kutsed on võimalik ainult läbi CSV; üksikute kutsed ja API juurdepääsu ei toetata.
- Ainult Azure AD globaalne administraatorite saate üles laadida CSV-failid.
- Kutsete tarbija e-posti aadressid (nt hotmail.com, Gmail.com või teenuste comcast.net) pole praegu toetatud.
- Väliskasutajate juurdepääsu kohapealse ei testita rakendused.
- Väliste kasutajate on automaatselt puhastada, kui tegeliku kasutaja kustutatakse nende kataloogist.
- Kutsete leviloendid ei toetata.
- Kuni 2000 kirjeid saate üles laadida CSV kaudu.

## <a name="related-articles"></a>Seotud artiklid
Sirvige meie teised artiklid Azure AD B2B koostöö.

- [Mis on Azure AD B2B koostöö?](active-directory-b2b-what-is-azure-ad-b2b.md)
- [Kuidas see toimib](active-directory-b2b-how-it-works.md)
- [Üksikasjaliku selgituse](active-directory-b2b-detailed-walkthrough.md)
- [CSV-faili vorming viide](active-directory-b2b-references-csv-file-format.md)
- [Väliste kasutajate Turbeloa vorming](active-directory-b2b-references-external-user-token-format.md)
- [Väliste kasutajate objekti atribuutide muutmine](active-directory-b2b-references-external-user-object-attribute-changes.md)
- [Artikli registri Rakendusehaldus Azure Active Directory 's](active-directory-apps-index.md)
