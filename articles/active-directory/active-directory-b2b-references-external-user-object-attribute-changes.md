<properties
   pageTitle="Väliste kasutajate objekti atribuutide muutmine Azure Active Directory B2B koostöö Preview | Microsoft Azure'i"
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
   ms.workload="na"
   ms.date="05/09/2016"
   ms.author="viviali"/>

# <a name="azure-ad-b2b-collaboration-preview-external-user-object-attribute-changes"></a>Azure'i AD B2B koostöö eelvaade: Väliskasutajate objekti atribuutide muutmine

Kasutaja objekti esindab iga kasutaja on Azure AD kataloogi. Kasutaja objekti Azure AD läbib eri atribuutide muutmine etappide B2B koostöö Kutsu lunastamiseks kulgemist. Kasutaja objekti tähistav kataloogis partneri kasutajal on atribuute, mis muutuvad lunastada kellaaeg, millal partneri kasutaja klõpsab link meilisõnumiga kutse. Täpsemalt:

- Täidetakse **SignInName** ja **AltSecId** atribuudid
- Kasutaja turvasubjektinimi vahetatakse atribuuti **DisplayName** (user_fabrikam.com#EXT#@contoso.com) sisselogimise nimi(user@fabrikam.com)

Jälgimise neist atribuutidest Azure AD aitavad teil kas partneri kasutaja on lunastada oma B2B koostöö kutse tõrkeotsing.

## <a name="related-articles"></a>Seotud artiklid
Sirvige meie teised artiklid Azure AD B2B koostöö.

- [Mis on Azure AD B2B koostöö?](active-directory-b2b-what-is-azure-ad-b2b.md)
- [Kuidas see toimib](active-directory-b2b-how-it-works.md)
- [Üksikasjaliku selgituse](active-directory-b2b-detailed-walkthrough.md)
- [CSV-faili vorming viide](active-directory-b2b-references-csv-file-format.md)
- [Väliste kasutajate Turbeloa vorming](active-directory-b2b-references-external-user-token-format.md)
- [Praegune eelvaade piirangud](active-directory-b2b-current-preview-limitations.md)
- [Artikli registri Rakendusehaldus Azure Active Directory 's](active-directory-apps-index.md)
