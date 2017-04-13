<properties
    pageTitle="Azure Active Directory identiteedi kaitse teatised | Microsoft Azure'i"
    description="Siit saate teada, kuidas toetada teatised oma juurdlus."
    services="active-directory"
    keywords="Azure'i active directory identiteedi kaitse, pilveteenuse rakenduse discovery, rakendused, Turve, risk, taseme, haavatavuse, turbepoliitika haldamine"
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

#<a name="azure-active-directory-identity-protection-notifications"></a>Azure Active Directory identiteedi kaitse teatised 


Azure'i AD identiteedi kaitse saadab saata automaatseid teavitusi meilisõnumite haldamiseks kasutaja risk ja risk sündmuste kahte tüüpi:

- Kasutaja rikutud meiliteatis

- Nädala digest e-posti.

## <a name="user-compromised-alert-email"></a>Kasutaja rikutud meiliteatis

Azure'i AD identiteedi kaitse tuvastab konto nagu rikutud luuakse kasutaja rikutud e-posti teatise. E-posti sisaldab linki lipuga märgitud risk aruande identiteedi kaitse armatuurlaua kasutajad. Soovitame, et saate kohe uurida teatised kahjustada.


## <a name="weekly-digest-email"></a>Nädala digest e-posti.

Nädala digest e-posti sisaldab uute risk sündmuste kokkuvõte.<br>
See sisaldab:

- Riski
- Kahtlaste tegevuste
- Tuvastatud nõrkade kohtade
- Identiteedi kaitse aruannete seotud lingid


<br>
![Parandamise](./media/active-directory-identityprotection-notifications/400.png "Remediation")
<br> 

Mida saab vahetada nädala digest meili saatmine.
<br><br>
![Kasutaja riske](./media/active-directory-identityprotection-notifications/62.png "User risks")
<br>
 

**Seotud konfiguratsiooni dialoogiboksi avamiseks**:

1. **Azure'i AD identiteedi kaitse** enne, klõpsake nuppu **sätted**.
<br><br>
![Kasutaja risk poliitika](./media/active-directory-identityprotection-notifications/401.png "User risk policy")
<br>

2. Klõpsake jaotises **üldist** **teatised**.
<br><br>
![Kasutaja risk poliitika](./media/active-directory-identityprotection-notifications/405.png "User risk policy")
<br>




## <a name="see-also"></a>Vt ka

- [Azure Active Directory identiteedi kaitse](active-directory-identityprotection.md) 

