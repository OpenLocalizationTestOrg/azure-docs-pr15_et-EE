<properties
    pageTitle="Azure'i AD õigustega identiteetide haldus | Microsoft Azure'i"
    description="Teema, mis selgitab, mis on Azure AD õigustega identiteetide haldus ja kuidas kasutada PIM cloud turvalisuse täiustamiseks."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="kgremban"/>

# <a name="azure-ad-privileged-identity-management"></a>Azure'i AD õigustega identiteetide haldus

Azure Active Directory (AD) õigustega identiteetide haldamisviis, kus saate hallata, kontrolli, ja jälgida juurdepääsu oma ettevõttes. See hõlmab kättesaadavust Azure AD ja muude Microsofti veebiteenuste nagu Office 365 või Microsoft Intune'i.  

> [AZURE.NOTE] Õigustega identiteetide haldus on saadaval ainult Azure Active Directory Premium P2 väljaannet. Lisateavet leiate teemast [Azure Active Directory väljaanded](active-directory-editions.md).

Ettevõtted, et vähendada rohkem inimesi, kellel on juurdepääs turvaline teabe või ressursse, kuna mis vähendab pahatahtlik kasutaja saada juurdepääsu soovite. Kasutajad peavad siiski endiselt õigustega toiminguid Azure, Office 365 või SaaS rakendustes. Ettevõtted ettekandmise kasutajate õigustega juurdepääsu Azure AD jälgida, mida kasutajad teevad tema administraatoriõigused. Azure'i AD õigustega identiteetide haldus aitab lahendada see risk.  

Azure'i AD õigustega identiteetide haldus aitab teil:  

- Vaadata, millised kasutajad on Azure AD administraatorid
- Nõudmisel, "samal ajal" Office 365 ja Intune, näiteks Microsoft Online Services administraatori juurdepääsu lubamine
- Administraatori ülesannete administraatori juurdepääs ajalugu ja muudatuste kohta aruannete hankimine
- Saada teatisi juurdepääsu õigustega rolli kohta

Azure'i AD õigustega identiteedi haldamine saate hallata sisseehitatud Azure AD ettevõtte rollid, sh:  

- Üldadministraator
- Arveldusadministraator
- Teenuseadministraator  
- Kasutaja administraator
- Parooliadministraator

## <a name="just-in-time-administrator-access"></a>Vaid Accessis aja administraator

Varem, võite määrata kasutaja administraatorirolli Azure klassikaline portaali või Windows PowerShelli kaudu. Selle tulemusena kasutaja saab **püsivate administraator**, alati aktiivne määratud roll. Azure'i AD õigustega identiteetide haldamisviis tutvustatakse mõistet **õigus administraator**. Õigus administraatorid peaksid olema kasutajatele, kes peavad õigustega juurdepääsu nüüd ja siis, kuid mitte iga päev. Roll on passiivne, kuni kasutaja vajab, siis on aktiveerimise lõpuleviimiseks ja aktiivne administraatoriks jaoks eelnevalt määratud aja jooksul.

## <a name="enable-privileged-identity-management-for-your-directory"></a>Kataloogi õigustega identiteedi haldamise lubamine

Azure'i AD õigustega identiteetide haldus saate hakata, [Azure portaali](https://portal.azure.com/).

>[AZURE.NOTE] Peate olema üldadministraator ettevõtte kontoga (nt @yourdomain.com), pole Microsofti kontot (näiteks @outlook.com), lubamiseks Azure AD õigustega identiteedi haldamise kataloog.

1. Logige sisse [portaali Azure](https://portal.azure.com/) üldadministraatorina kataloogi.
2. Kui teie asutuses on rohkem kui ühe kausta, valige Azure portaali paremas ülanurgas oma kasutajanimi. Valige kataloogi, kus te kasutate Azure AD õigustega identiteetide haldus.
3. Valige **rohkem teenuseid** ja tekstivälja filtri abil saate otsida **Azure AD õigustega identiteetide haldus**.
4. Märkige ruut **Kinnita armatuurlaua** ja seejärel klõpsake nuppu **Loo**. Avaneb õigustega identiteedi haldamise rakendus.

Kui olete esimene isik kasutada Azure AD õigustega identiteetide haldamisviis kataloogis, siis [Turbeviisard](active-directory-privileged-identity-management-security-wizard.md) juhatab teid läbi algse ülesande kogemus. Pärast seda siis saab automaatselt esimene **administraator turvalisus** ja kataloogi **administraatori õigustega roll** .

Ainult õigustega roll administraator saab juurdepääsu haldamine teiste administraatorite jaoks. Saate [anda teistele kasutajatele võimaluse PIM hallata](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).

## <a name="privileged-identity-management-dashboard"></a>Õigustega identiteedi haldamise armatuurlaud

Azure'i AD õigustega identiteedi Manager pakub armatuurlaud, mis annab teile oluline teave, näiteks:

- Teatised, mis märgivad võimalusi parandada turvalisus
- Kasutajad, kellel on iga õigustega rollile määratud arv  
- Arvu õigus ja püsivate administraatorid.
- Alalise juurdepääsu arvustust

![PIM armatuurlaua - kuvatõmmis][2]

## <a name="privileged-role-management"></a>Õigustega rolli haldus

Azure'i AD õigustega identiteetide haldus, saate hallata administraatorid lisada või eemaldada iga rolli püsivate või õigus administraatorid.

![PIM Lisa/eemalda administraatorite - kuvatõmmis][3]

## <a name="configure-the-role-activation-settings"></a>Roll aktiveerimise sätete konfigureerimine

[Roll sätete](active-directory-privileged-identity-management-how-to-change-default-settings.md) abil saate konfigureerida õigus rolli aktiveerimise atribuutide, sh:

- Roll aktiveerimise perioodi kestus
- Teate rolli aktiveerimine
- Kasutaja peab rolli aktiveerimisprotsessi ajal esitada teavet  

![Kuvatõmmis PIM - administraatori aktiveerimine - sätted][4]

Pange tähele, et pilt, **Mitmikautentimise** nupud on keelatud. Teatud, väga au rollid, saame nõua MFA jaoks kõrgendatud kaitse.

## <a name="role-activation"></a>Roll aktiveerimine  

[Aktiveerimine rollid](active-directory-privileged-identity-management-how-to-activate-role.md), taotleb õigus administraator on aeg seotud "aktiveerimine" rolli. Suvand **Aktiveeri minu rolli** kasutamine Azure AD õigustega identiteetide haldus saab taotleda aktiveerimine.

Administraator, kes soovib rolli aktiveerimiseks peab lähtestamine Azure AD õigustega identiteetide haldamisviis Azure'i portaalis.

Roll aktiveerimine on kohandatavad. PIM sätted saate määrata aktiveerimise ja mis pikkus admin tuleb aktiveerida roll pakuvad teavet.

![PIM administraator taotluse rolli aktiveerimine - kuvatõmmis][5]

## <a name="review-role-activity"></a>Vaadake üle rolli tegevus

On kaks võimalust jälgida, kuidas teie töötajad ja Administraatorid kasutavad õigustega rollid. Esimene suvand kasutab [ajaloo audit](active-directory-privileged-identity-management-how-to-use-audit-log.md). Auditilogi ajaloo logib muutuste jälituse õigustega rollimääranguid ja rolli aktiveerimise ajalugu.

![PIM aktiveerimise ajalugu – kuvatõmmis][6]

Teine võimalus on tavaline [access kontrollib](active-directory-privileged-identity-management-how-to-start-security-review.md)häälestamine. Neid Accessi kommentaare saate läbi ja määratud läbivaataja (nt meeskonnatöö manager) või töötajate saate vaadata ise. See on parim viis jälgida, kes veel nõuab access ja kes enam ei.


## <a name="next-steps"></a>Järgmised sammud
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-configure/PIM_EnablePim.png
[2]: ./media/active-directory-privileged-identity-management-configure/PIM_Dash.png
[3]: ./media/active-directory-privileged-identity-management-configure/PIM_AddRemove.png
[4]: ./media/active-directory-privileged-identity-management-configure/PIM_RoleActivationSettings.png
[5]: ./media/active-directory-privileged-identity-management-configure/PIM_RequestActivation.png
[6]: ./media/active-directory-privileged-identity-management-configure/PIM_ActivationHistory.png
