<properties
    pageTitle="Litsentsimata kasutus | Microsoft Azure'i"
    description="Litsentsimata kasutus aruande aitab teil tuvastada litsentsimata kasutajad, kes ei kasuta makstud Azure AD funktsioone."
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

# <a name="unlicensed-usage-report"></a>Litsentsimata kasutamise aruanne

Litsentsimata kasutus aruande aitab teil tuvastada litsentsimata kasutajad, kes ei kasuta makstud Azure AD funktsioone. See võimaldab teil paremini kasutada ostetud litsentside ja tuvastamiseks teate, kui vajate täiendavaid litsentse. 

Aruandes kuvatakse aktiivse kasutamist makstud funktsioonid on viimase 30 päeva jooksul. 

## <a name="report-structure"></a>Aruande struktuur
 
| Veeru nimi          |    Kirjeldus |
| :--                  | :--         |
| Litsentsimata kasutajate      |    Kasutaja nimi |
| Funktsioon              | Funktsiooni nimi. Näide: tingimusvormingu juurdepääs |
| Rakenduste juurde | Rakendus, mis kasutab funktsiooni nimi. Näide: Office 365 SharePoint Online |

 
> [AZURE.NOTE] Kui kasutajakonto on kustutatud 'Litsentsimata kasutaja' veerg olema täidetud järgmised kelle ID, nt 1003000090D8B285


## <a name="conditional-access-feature"></a>Tingimusvormingu juurdepääsu funktsiooni

Litsentsimata kasutajate märgitakse juurdepääsemisel teenus, mis sisaldab juurdepääsu poliitika rakendatakse siis, kui neil on Azure AD Premium litsents. 

See kehtib MFA / asukoht poliitika, samuti seadme poliitikat, mis kasutavad Intune.
 

## <a name="see-also"></a>Vt ka

- [Tingimusvormingu Accessi kasutamine koos Office 365 ja muude Azure Active Directory ühendatud rakendused](active-directory-conditional-access.md)
- [Tingimusvormingu juurdepääsu Azure AD töötamise alustamine](active-directory-conditional-access-azuread-connected-apps.md) 


