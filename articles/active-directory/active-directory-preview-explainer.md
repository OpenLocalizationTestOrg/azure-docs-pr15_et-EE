<properties
    pageTitle="Azure Active Directory eelvaade valmis | Microsoft Azure'i"
    description="Teema, mis selgitab Azure Active Directory klassikaline portaali ja Azure Active Directory eelvaate Azure'i portaalis erinevused."
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/12/2016"
    ms.author="curtand"/>


# <a name="preview-of-the-azure-active-directory-management-experience-in-the-azure-portal"></a>Azure'i portaalis teenuses Azure Active Directory haldamise eelvaade

Azure Active Directory (Azure AD) halduse kogemus on eelvaates Azure'i portaalis. Võite proovida seda välja, üldadministraatorina kataloogi [Azure](https://portal.azure.com) portaali sisse logida. Seejärel valige Azure Active Directory loendis teenused, kui see on näha või **rohkem teenuseid** kõigi teenuste loendi kuvamiseks valige. Teil pole vaja Azure tellimuse kasutada funktsiooni Azure'i AD-juhtimise kogemus Azure'i portaalis.


## <a name="capabilities-of-the-preview-experience"></a>Võimaluste kohta eelvaade

Eelvaate kogemus võimaldab teil hallata paljude directory ressursside kasutajate, rühmade ja rakenduste ning kataloogi sätted Azure'i portaalis, nt. Oleme parandada selle kasutusvõimalusi kaasata kõik funktsioonid, mis on olemas on Azure AD juhtimise kogemus [Azure klassikaline portaalis](https://manage.windowsazure.com). Seni on mõned directory haldamise tööülesandeid, mida te peate endiselt lõpuleviimine klassikaline portaalis.

## <a name="manage-the-same-azure-ad-tenants"></a>Sama Azure AD rentnike haldamine

Eelvaate kogemus loeb ja kirjutab sama Azure Active Directory rentnikuga klassikaline portaali ja Office 365 halduskeskus. Mis tahes nende portaalide tehtud muudatused kajastuvad kõik teised.

## <a name="use-the-same-authorization-logic"></a>Kasutage sama autoriseerimine loogika

Eelvaate kogemus kasutab sama autoriseerimine loogika olemasoleva Active Directory kliendid. Kasutajatele lubatud directory ressursse, mis põhineb directory rolli, näiteks globaalne administraator, kasutaja administraator, administraatori parooli muuta. Azure'i ressursid või Azure tellimuse rolli ei luba kasutaja haldamiseks directory ressursid. Lisateavet teemast Azure AD haldusrollide, [administraatorirollide Azure Active Directory](active-directory-assign-admin-roles.md). 

Kasutuskogemuse eelvaade on optimeeritud globaalse administraatorid. Kui kasutate eelvaade kogemus ajal sisse logitud kasutajale, mis pole üldadministraator, peate halvenenud kogemus. Näiteks võib olla võimalus valige nupp, mis võimaldab teil alustada tööülesande, mida te ei saa lõpule viia kataloogis. Me täiustame seda probleemi kiiresti.
 
## <a name="tell-us-what-you-think"></a>Meile öelda, mida te arvate

Eelvaate kogemusi [Azure AD tagasiside Foorum](https://social.msdn.microsoft.com/Forums/home?forum=WindowsAzureAD&filter=alltypes&sort=lastpostdesc)jaotises videoportaali administraator saab tagasiside.
