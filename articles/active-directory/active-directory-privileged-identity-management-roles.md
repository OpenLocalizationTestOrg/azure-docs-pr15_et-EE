<properties
   pageTitle="Rollide PIM | Microsoft Azure'i"
   description="Siit saate teada, millised rollid kasutatakse õigustega identiteedid laiendiga Azure'i õigustega identiteetide haldus."
   services="active-directory"
   documentationCenter=""
   authors="kgremban"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="07/01/2016"
   ms.author="kgremban"/>

# <a name="roles-in-azure-ad-privileged-identity-management"></a>Rollide Azure AD au identiteetide haldus

<!-- **PLACEHOLDER: Need description of how this works. Azure PIM uses roles from MSODS objects.**-->

Saate määrata oma asutuse erinevate administraatorirollid Azure AD kasutajad. Neid rollimääranguid määrata, milliseid toiminguid, näiteks lisada või eemaldada kasutajad või teenuse sätete muutmise kasutajad saavad sooritada Azure AD, Office 365 ja muude Microsoft Online Services ja ühendatud rakendused.  

Globaalne administraator saab värskendada, millistel kasutajatel on **jäädavalt** Azure AD, PowerShelli cmdlet-käskude kasutamine, näiteks rollidele määratud `Add-MsolRoleMember` ja `Remove-MsolRoleMember`, või kui kirjeldatud [Azure Active Directory administraatorirollide määramine](active-directory-assign-admin-roles.md)klassikaline portaali kaudu.

Azure'i AD õigustega identiteetide haldus (PIM) haldab Azure AD poliitikate õigustega juurdepääsu kasutajate jaoks. PIM määrab kasutajatele ühe või mitme rollidele Azure AD ja saate määrata keegi olla jäädavalt roll, või roll õigus. Kui kasutaja on jäädavalt rollile määratud või aktiveerib õigus Rollimäärang, siis need saate hallata oma rollidele määratud õiguste Azure Active Directory, Office 365 ja muude rakenduste.

Ei ole vahet antud kellelegi võrreldes on õigus Rollimäärang püsiv juurdepääs. Ainult erinevus on, et mõned inimesed ei pea seda juurdepääs kogu aeg. Need on tehtud õigus roll ja selle sisse lülitada ja välja siis, kui neil on vaja.

## <a name="roles-managed-in-pim"></a>Hallatava PIM rollid

Õigustega identiteetide haldus võimaldab teil määrata kasutajatele levinud administraatorirollid, sh:


- **Üldadministraator** (tuntud ka kui ettevõtte administraator) on juurdepääs haldus kõik funktsioonid. Saate määrata rohkem kui üks üldadministraator teie asutuses. Isik, kes registreerub osta Office 365 automaatselt muutub globaalne administraator.
- **Administraatori õigustega rolli** haldab Azure AD PIM ja teiste kasutajate rollimäärangud värskendused.  
- **Arveldus administraatori** teostab oste, haldab tellimusi, tugipileteid ning jälgib teenuste seisundit.
- **Administraatori parooli** lähtestab paroole, haldab hooldustaotlusi ja jälgib teenuste seisundit. Parooliadministraatorid on piiratud kasutajate paroole lähtestada.
- **Teenuse administraator** haldab hooldustaotlusi ja jälgib teenuse seisund.

  > [AZURE.NOTE] Kui kasutate teenusekomplekti Office 365, siis enne teenuse administraatori rolli määramine kasutajale, esmalt kasutajale määrata administraatoriõigusi, nt Exchange Online'i teenusega.

- **Kasutajahalduse administraator** lähtestab paroole, jälgib teenuste seisundit ja haldab kasutajakontosid, kasutajarühmade ja hooldustaotlused. Kasutajahalduse administraator ei saa kustutada üldadministraator, luua muude administraatorirollide või üld-, arveldamine ja teenuse administraatorite paroolide lähtestamine.
- **Exchange'i administraator** pääseb Exchange Online'i kaudu Exchange'i halduskeskuses (EAC), ja saate teha peaaegu kõiki toiminguid Exchange Online'i.
- **SharePointi administraator** pääseb SharePoint Online'i SharePoint Online'i halduskeskuse kaudu ja saab SharePoint Online'is teha peaaegu kõiki toiminguid.
- **Skype'i ärirakenduse administraator** pääseb Skype'i ärirakenduse Skype'i ärirakenduse halduskeskuse kaudu ja saate teha peaaegu kõiki toiminguid Skype'i ärirakenduse veebiväljaande.

Lugege [Azure AD määramine administraatorirollid](active-directory-assign-admin-roles.md) ja [administraatorirollide määramine Teenusekomplektis Office 365](https://support.office.com/article/Assigning-admin-roles-in-Office-365-eac4d046-1afd-4f1a-85fc-8219c79e1504)kohta lisateavet järgmistest artiklitest.

<!--**PLACEHOLDER: The above article may not be the one we want since PIM gets roles from places other that Office 365**-->


Alates PIM, saate [need rollid ja kasutajale määrata](active-directory-privileged-identity-management-how-to-add-role-to-user.md) , et kasutaja saab [aktiveerida roll vastavalt vajadusele](active-directory-privileged-identity-management-how-to-activate-role.md).

Kui soovite anda teisele kasutajale juurdepääs PIM ise hallata, rollid, mis nõuab PIM kasutajal on on veelgi kirjeldatud [Kuidas PIM juurdepääsu anda](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).


<!-- ## The PIM Security Administrator Role **PLACEHOLDER: Need description of the Security Administrator role.**-->

## <a name="roles-not-managed-in-pim"></a>Pole hallatava PIM rollid

Rollide Exchange Online'i ja SharePoint Online'i eespool, välja arvatud pole esindatud Azure AD ja seega ei ole näha PIM. Kohandatud rollimääranguid neid Office 365 teenuste muutmise kohta leiate lisateavet teemast [Office 365 õigused](https://support.office.com/article/Permissions-in-Office-365-da585eea-f576-4f55-a1e0-87090b6aaa9d).

Azure'i tellimused ja ressursside rühmad on ka pole esindatud Azure AD. Azure'i tellimuste haldamine, vaadake, [Kuidas lisada või muuta Azure administraatorirollid](../billing-add-change-azure-subscription-administrator.md) ja Azure RBAC kohta lisateabe saamiseks vt [Azure_Role-Based juurdepääsu reguleerimine](role-based-access-control-configure.md).

<!--**The above links might be replaced by ones that are from within this documentation repository **-->


## <a name="user-roles-and-signing-in"></a>Kasutaja rollid ja sisselogimine
Mõned Microsofti teenuste ja rakenduste, rolli määramine kasutaja ei pruugi olla piisav, et kasutaja olema administraator.

Juurdepääs Azure'i klassikaline portaali nõuab kasutaja olema administraator või koostöö administraator Azure tellimusel, isegi juhul, kui kasutaja ei pea Azure tellimuste haldamine.  Näiteks haldamiseks konfiguratsioonisätted Azure AD klassikaline portaalis, kasutaja peavad olema üldadministraator Azure AD ja tellimuse koostöö administraator Azure tellimuse kohta.  Saate teada, kuidas kasutajaid lisada Azure tellimused, vaadake, [Kuidas lisada või muuta Azure administraatori rollid](../billing-add-change-azure-subscription-administrator.md).

Teenuse Microsoft Online Services juurdepääsu nõuda kasutajale määrata ka litsentsi enne selle teenuse portaali avamiseks või administraatoritoiminguid.

## <a name="assign-a-license-to-a-user-in-azure-ad"></a>Azure AD kasutajale litsentsi määramine

1. Logige sisse [Azure klassikaline portaali] (http://manage.windowsazure.com) üldadministraatori kontot või koostöö administraator konto.
2. Põhimenüü **Kõigi üksuste** valimine
3. Valige kataloog, mida soovite töötada ja mis on seotud litsentsid.
4. Valige **litsentsid**. Saadaolevate litsentside loendis kuvatakse.
5. Valige litsentsi leping, mis sisaldab litsentse, mida soovite jagada.
6. Valige **määrata kasutajatele**.
7. Valige kasutaja, mida soovite litsentsi määrata.
8. Klõpsake nuppu **Määra** .  Kasutaja saab nüüd sisse logida Azure.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Järgmised sammud
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]
