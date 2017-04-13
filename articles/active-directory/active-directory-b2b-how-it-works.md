<properties
   pageTitle="Azure'i AD B2B koostöö eelvaade: kuidas see toimib | Microsoft Azure'i"
   description="Kirjeldab, kuidas Azure Active Directory B2B koostöö toetab oma rist – ettevõtte seoseid, võimaldades äripartneritega valikuliselt juurdepääsu oma ettevõtte rakendused"
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

# <a name="azure-ad-b2b-collaboration-preview-how-it-works"></a>Azure'i AD B2B koostöö eelvaade: kuidas see toimib
Azure'i AD B2B koostöö põhineb kutsuda ja lunastage mudel. Esitate soovite töötada koos soovite neid kasutada rakendusi isikute meiliaadressid. Azure AD neile saadetakse e-posti kutsuda link. Partneri kasutaja järgmine link ja logige sisse oma Azure AD konto või logige abil küsitakse üles uue Azure AD kontoga.

1. Teie administraator kutsub partneri kasutajate üleslaadimisel [liigendatud CSV-faili](active-directory-b2b-references-csv-file-format.md) Azure'i portaalis.
2. Portaali saadab Kutsu e-kirju need partneri kasutajad.
3. Partneri meilisõnumis linki ja kasutajad palutakse, logige sisse oma töö mandaadi (kui need on juba kasutusel Azure AD) abil või Azure AD B2B koostöö kasutajaks registreerumine.
4. Partneri kasutajad suunatakse teid nad on kutsutud, kui neil on nüüd Accessi rakendus.

## <a name="directory-operations"></a>Directory toimingud
Partneri kasutajad on olemas oma Azure AD nimega välised kasutajad. See tähendab, et teie administraator saab ettevalmistamise litsentse, määrata liikmestaatuse ja edasi anda ettevõtte rakenduste kaudu Azure portaali või oma ettevõtte kasutajate jaoks nagu Azure PowerShelli kaudu juurdepääsu.

Samal ajal makstud Azure AD (Basic või Premium) tellimus pole vaja kasutada Azure AD B2B, jaoks, kellel on makstud Azure AD tellimuse (Basic või Premium) vastu järgmised täiendavad eelised:

 - Administraatorid saavad määrata rühmade rakendused, pakkudes lihtsam kutsutud kasutajate juurdepääsu haldamine.
 - Administraator rentniku sümboolika kasutatakse kujundamine kutse e-kirju ja tagastusväärtus kogemus, et rohkem konteksti kutsutud partneri kasutajad.

## <a name="related-articles"></a>Seotud artiklid
 Sirvige meie teised artiklid Azure AD B2B koostöö

 - [Mis on Azure AD B2B koostöö?](active-directory-b2b-what-is-azure-ad-b2b.md)
 - [Üksikasjaliku selgituse](active-directory-b2b-detailed-walkthrough.md)
 - [CSV-faili vorming viide](active-directory-b2b-references-csv-file-format.md)
 - [Väliste kasutajate Turbeloa vorming](active-directory-b2b-references-external-user-token-format.md)
 - [Väliste kasutajate objekti atribuutide muutmine](active-directory-b2b-references-external-user-object-attribute-changes.md)
 - [Praegune eelvaade piirangud](active-directory-b2b-current-preview-limitations.md)
 - [Artikli registri Rakendusehaldus Azure Active Directory 's](active-directory-apps-index.md)
