<properties
   pageTitle="Väliste kasutajate Turbeloa vormingu Azure Active Directory B2B koostöö eelvaade | Microsoft Azure'i"
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

# <a name="azure-ad-b2b-collaboration-preview-external-user-token-format"></a>Azure'i AD B2B koostöö eelvaade: Väliskasutajate Turbeloa vorming

Nõuded standard Azure AD luba on esitatud artiklis [toetatud Turbeloa ja taotluste](active-directory-token-and-claims.md) kohta azure.microsoft.com.

Mis on autenditud B2B koostöö väliste kasutajate jaoks erinevad nõuded on järgmised:<br/>
- **Objekti ID:** ressursi rentnikust objekti ID<br/>
- **TID**: rentniku rentnikust ressursi ID<br/>
- **Väljaandja**: see on ressursi rentniku<br/>
- **IDP**: see on home rentniku kasutaja<br/>
- **AltSecId**: see on alternatiivne turva-ID, mis on teile läbipaistmatu<br/>

## <a name="related-articles"></a>Seotud artiklid
Sirvige meie teised artiklid Azure AD B2B koostöö.

- [Mis on Azure AD B2B koostöö?](active-directory-b2b-what-is-azure-ad-b2b.md)
- [Kuidas see toimib](active-directory-b2b-how-it-works.md)
- [Üksikasjaliku selgituse](active-directory-b2b-detailed-walkthrough.md)
- [CSV-faili vorming viide](active-directory-b2b-references-csv-file-format.md)
- [Väliste kasutajate objekti atribuutide muutmine](active-directory-b2b-references-external-user-object-attribute-changes.md)
- [Praegune eelvaade piirangud](active-directory-b2b-current-preview-limitations.md)
- [Artikli registri Rakendusehaldus Azure Active Directory 's](active-directory-apps-index.md)
