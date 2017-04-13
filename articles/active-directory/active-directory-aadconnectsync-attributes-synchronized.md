<properties
    pageTitle="Azure'i AD-ühendus sünkroonimine: atribuute sünkroonitud Azure Active Directory | Microsoft Azure'i"
    description="Loendeid sünkroonitakse Azure Active Directory atribuute."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="markvi;andkjell"/>


# <a name="azure-ad-connect-sync-attributes-synchronized-to-azure-active-directory"></a>Azure'i AD-ühendus sünkroonimine: Azure Active Directory sünkroonimiseks atribuudid
Selles teemas on loetletud sünkroonitud sünkroonimine Azure'i AD-ühenduse atribuudid.  
Atribuudid on rühmitatud seotud Azure AD rakendus.

## <a name="attributes-to-synchronize"></a>Atribuutide sünkroonimiseks
Levinud küsimus on, *mis on minimaalne atribuutide sünkroonimiseks loendis*. Vaike- ja Soovitatavad lähenemine on hoida vaikimisi atribuute, et pilveteenuses saate ehitatakse täielik GAL (globaalne aadressiloend) ja saada kõik funktsioonid Teenusekomplektis Office 365 teenustest. Mõnel juhul on mõned atribuudid, mis teie asutus ei soovi pilveteenusega sünkroonitud kuna neid atribuute, mis sisaldavad tundlikku või PII (isikuttuvastava teabe) andmed, nagu järgmises näites:  
![vigased atribuudid](./media/active-directory-aadconnectsync-attributes-synchronized/badextensionattribute.png)

Sel juhul Alusta atribuutide selles teemas loendis ja tuvastada neid atribuute, mis peaks sisaldama tundliku või PII andmed ja ei saa sünkroonida. Seejärel tühjendage nende atribuutide installimisel [Azure AD rakendus ja atribuut filtreerimise](active-directory-aadconnect-get-started-custom.md#azure-ad-app-and-attribute-filtering)abil.

>[AZURE.WARNING] Kui tühistamine atribuute, peaksite olema ettevaatlik ja tühjendage ruut ainult neid atribuute, mis on täiesti ei saa sünkroonida. Muude atribuutide unselecting võib olla negatiivset mõju funktsioone.

## <a name="office-365-proplus"></a>Office 365 ProPlus

| Atribuudi nimi| Kasutaja| Kommentaari |
| --- | :-: | --- |
| accountEnabled| X| Määratleb kui konto on lubatud.|
| CN| X|  |
| displayName| X|  |
| objectSID| X| omaduste. AD kasutaja identifikaator kasutada sünkroonimise vahel Azure'i AD- ja AD.|
| pwdLastSet| X| omaduste. Kasutada teada, millal seadmele juba antud märgid. Kasutada nii parooli sünkroonimise ja federation.|
| sourceAnchor| X| omaduste. Püsiv identifikaator säilitamiseks lisatakse ja Azure AD seos.|
| usageLocation| X| omaduste. Kasutaja riik. Litsentsi määramine kasutada.|
| userPrincipalName| X| UPN-i on kasutaja sisselogimise ID-d. Kõige sagedamini sama nimega [meil] väärtuse.|

## <a name="exchange-online"></a>Exchange Online

| Atribuudi nimi| Kasutaja| Kontakti| Rühma| Kommentaari |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | Määratleb kui konto on lubatud.|
| abi| X| X|  |  |
| authOrig| X| X| X|  |
| c| X| X|  |  |
| CN| X|  | X|  |
| Co| X| X|  |  |
| ettevõtte| X| X|  |  |
| koos riigi| X| X|  |  |
| osakonna| X| X|  |  |
| kirjeldus| X| X| X|  |
| displayName| X| X| X|  |
| dLMemRejectPerms| X| X| X|  |
| dLMemSubmitPerms| X| X| X|  |
| extensionAttribute1| X| X| X|  |
| extensionAttribute10| X| X| X|  |
| extensionAttribute11| X| X| X|  |
| extensionAttribute12| X| X| X|  |
| extensionAttribute13| X| X| X|  |
| extensionAttribute14| X| X| X|  |
| extensionAttribute15| X| X| X|  |
| extensionAttribute2| X| X| X|  |
| extensionAttribute3| X| X| X|  |
| extensionAttribute4| X| X| X|  |
| extensionAttribute5| X| X| X|  |
| extensionAttribute6| X| X| X|  |
| extensionAttribute7| X| X| X|  |
| extensionAttribute8| X| X| X|  |
| extensionAttribute9| X| X| X|  |
| facsimiletelephonenumber| X| X|  |  |
| givenName| X| X|  |  |
| Kodutelefon| X| X|  |  |
| teave| X| X| X| See atribuut on praegu ei tarbitud rühmad.|
| Initsiaalid| X| X|  |  |
| l| X| X|  |  |
| legacyExchangeDN| X| X| X|  |
| mailNickname| X| X| X|  |
| mangedBy|  |  | X|  |
| haldur| X| X|  |  |
| liikme|  |  | X|  |
| mobiilse| X| X|  |  |
| Vaevuste-HABSeniorityIndex| X| X| X|  |
| Vaevuste-PhoneticDisplayName| X| X| X|  |
| msExchArchiveGUID| X|  |  |  |
| msExchArchiveName| X|  |  |  |
| msExchAssistantName| X| X|  |  |
| msExchAuditAdmin| X|  |  |  |
| msExchAuditDelegate| X|  |  |  |
| msExchAuditDelegateAdmin| X|  |  |  |
| msExchAuditOwner| X|  |  |  |
| msExchBlockedSendersHash| X| X|  |  |
| msExchBypassAudit| X|  |  |  |
| msExchCoManagedByLink|  |  | X|  |
| msExchDelegateListLink| X|  |  |  |
| msExchELCExpirySuspensionEnd| X|  |  |  |
| msExchELCExpirySuspensionStart| X|  |  |  |
| msExchELCMailboxFlags| X|  |  |  |
| msExchEnableModeration| X|  | X|  |
| msExchExtensionCustomAttribute1| X| X| X| See atribuut on praegu ei tarbitud Exchange Online'i. |
| msExchExtensionCustomAttribute2| X| X| X| See atribuut on praegu ei tarbitud Exchange Online'i. |
| msExchExtensionCustomAttribute3| X| X| X| See atribuut on praegu ei tarbitud Exchange Online'i. |
| msExchExtensionCustomAttribute4| X| X| X| See atribuut on praegu ei tarbitud Exchange Online'i. |
| msExchExtensionCustomAttribute5| X| X| X| See atribuut on praegu ei tarbitud Exchange Online'i. |
| msExchHideFromAddressLists| X| X| X|  |
| msExchImmutableID| X|  |  |  |
| msExchLitigationHoldDate| X| X| X|  |
| msExchLitigationHoldOwner| X| X| X|  |
| msExchMailboxAuditEnable| X|  |  |  |
| msExchMailboxAuditLogAgeLimit| X|  |  |  |
| msExchMailboxGuid| X|  |  |  |
| msExchModeratedByLink| X| X| X|  |
| msExchModerationFlags| X| X| X|  |
| msExchRecipientDisplayType| X| X| X|  |
| msExchRecipientTypeDetails| X| X| X|  |
| msExchRemoteRecipientType| X|  |  |  |
| msExchRequireAuthToSendTo| X| X| X|  |
| msExchResourceCapacity| X|  |  |  |
| msExchResourceDisplay| X|  |  |  |
| msExchResourceMetaData| X|  |  |  |
| msExchResourceSearchProperties| X|  |  |  |
| msExchRetentionComment| X| X| X|  |
| msExchRetentionURL| X| X| X|  |
| msExchSafeRecipientsHash| X| X|  |  |
| msExchSafeSendersHash| X| X|  |  |
| msExchSenderHintTranslations| X| X| X|  |
| msExchTeamMailboxExpiration| X|  |  |  |
| msExchTeamMailboxOwners| X|  |  |  |
| msExchTeamMailboxSharePointUrl| X|  |  |  |
| msExchUserHoldPolicies| X|  |  |  |
| msOrg-IsOrganizational|  |  | X|  |
| objectSID| X|  | X| omaduste. AD kasutaja identifikaator kasutada sünkroonimise vahel Azure'i AD- ja AD.|
| oOFReplyToOriginator|  |  | X|  |
| otherFacsimileTelephone| X| X|  |  |
| otherHomePhone| X| X|  |  |
| otherTelephone| X| X|  |  |
| Piipar| X| X|  |  |
| physicalDeliveryOfficeName| X| X|  |  |
| Sihtnumber| X| X|  |  |
| proxyAddresses| X| X| X|  |
| publicDelegates| X| X| X|  |
| pwdLastSet| X|  |  | omaduste. Kasutada teada, millal seadmele juba antud märgid. Kasutada nii parooli sünkroonimise ja federation.|
| reportToOriginator|  |  | X|  |
| reportToOwner|  |  | X|  |
| securityEnabled|  |  | X| Tuletatud groupType|
| SN| X| X|  |  |
| sourceAnchor| X| X| X| omaduste. Püsiv identifikaator säilitamiseks lisatakse ja Azure AD seos.|
| st| X| X|  |  |
| streetAddress| X| X|  |  |
| targetAddress| X| X|  |  |
| telephoneAssistant| X| X|  |  |
| telephoneNumber| X| X|  |  |
| thumbnailPhoto| X| X|  |  |
| pealkiri| X| X|  |  |
| unauthOrig| X| X| X|  |
| usageLocation| X|  |  | omaduste. Kasutaja riik. Litsentsi määramine kasutada.|
| userCertificate| X| X|  |  |
| userPrincipalName| X|  |  | UPN-i on kasutaja sisselogimise ID-d. Kõige sagedamini sama nimega [meil] väärtuse.|
| userSMIMECertificates| X| X|  |  |
| wWWHomePage| X| X|  |  |

## <a name="sharepoint-online"></a>SharePoint Online

| Atribuudi nimi| Kasutaja| Kontakti| Rühma| Kommentaari |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | Määratleb kui konto on lubatud.|
| authOrig| X| X| X|  |
| c| X| X|  |  |
| CN| X|  | X|  |
| Co| X| X|  |  |
| ettevõtte| X| X|  |  |
| koos riigi| X| X|  |  |
| osakonna| X| X|  |  |
| kirjeldus| X| X| X|  |
| displayName| X| X| X|  |
| dLMemRejectPerms| X| X| X|  |
| dLMemSubmitPerms| X| X| X|  |
| extensionAttribute1| X| X| X|  |
| extensionAttribute10| X| X| X|  |
| extensionAttribute11| X| X| X|  |
| extensionAttribute12| X| X| X|  |
| extensionAttribute13| X| X| X|  |
| extensionAttribute14| X| X| X|  |
| extensionAttribute15| X| X| X|  |
| extensionAttribute2| X| X| X|  |
| extensionAttribute3| X| X| X|  |
| extensionAttribute4| X| X| X|  |
| extensionAttribute5| X| X| X|  |
| extensionAttribute6| X| X| X|  |
| extensionAttribute7| X| X| X|  |
| extensionAttribute8| X| X| X|  |
| extensionAttribute9| X| X| X|  |
| facsimiletelephonenumber| X| X|  |  |
| givenName| X| X|  |  |
| hideDLMembership|  |  | X|  |
| Kodutelefon| X| X|  |  |
| teave| X| X| X|  |
| initsiaalid| X| X|  |  |
| ipPhone| X| X|  |  |
| l| X| X|  |  |
| e-posti| X| X| X|  |
| mailnickname| X| X| X|  |
| managedBy|  |  | X|  |
| haldur| X| X|  |  |
| liikme|  |  | X|  |
| middleName| X| X|  |  |
| mobiilse| X| X|  |  |
| msExchTeamMailboxExpiration| X|  |  |  |
| msExchTeamMailboxOwners| X|  |  |  |
| msExchTeamMailboxSharePointLinkedBy| X|  |  |  |
| msExchTeamMailboxSharePointUrl| X|  |  |  |
| objectSID| X|  | X| omaduste. AD kasutaja identifikaator kasutada sünkroonimise vahel Azure'i AD- ja AD.|
| oOFReplyToOriginator|  |  | X|  |
| otherFacsimileTelephone| X| X|  |  |
| otherHomePhone| X| X|  |  |
| otherIpPhone| X| X|  |  |
| otherMobile| X| X|  |  |
| otherPager| X| X|  |  |
| otherTelephone| X| X|  |  |
| Piipar| X| X|  |  |
| physicalDeliveryOfficeName| X| X|  |  |
| Sihtnumber| X| X|  |  |
| postOfficeBox| X| X|  |  |
| preferredLanguage| X|  |  |  |
| proxyAddresses| X| X| X|  |
| pwdLastSet| X|  |  | omaduste. Kasutada teada, millal seadmele juba antud märgid. Kasutada nii parooli sünkroonimise ja federation.|
| reportToOriginator|  |  | X|  |
| reportToOwner|  |  | X|  |
| securityEnabled|  |  | X| Tuletatud groupType|
| SN| X| X|  |  |
| sourceAnchor| X| X| X| omaduste. Püsiv identifikaator säilitamiseks lisatakse ja Azure AD seos.|
| st| X| X|  |  |
| streetAddress| X| X|  |  |
| targetAddress| X| X|  |  |
| telephoneAssistant| X| X|  |  |
| telephoneNumber| X| X|  |  |
| thumbnailPhoto| X| X|  |  |
| pealkiri| X| X|  |  |
| unauthOrig| X| X| X|  |
| URL-i| X| X|  |  |
| usageLocation| X|  |  | omaduste. Kasutaja riik. Litsentsi määramine kasutada.|
| userPrincipalName| X|  |  | UPN-i on kasutaja sisselogimise ID-d. Kõige sagedamini sama nimega [meil] väärtuse.|
| wWWHomePage| X| X|  |  |

## <a name="lync-online"></a>Lync Online'i

| Atribuudi nimi| Kasutaja| Kontakti| Rühma| Kommentaari |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | Määratleb kui konto on lubatud.|
| c| X| X|  |  |
| CN| X|  | X|  |
| Co| X| X|  |  |
| ettevõtte| X| X|  |  |
| osakonna| X| X|  |  |
| kirjeldus| X| X| X|  |
| displayName| X| X| X|  |
| facsimiletelephonenumber| X| X| X|  |
| givenName| X| X|  |  |
| Kodutelefon| X| X|  |  |
| ipPhone| X| X|  |  |
| l| X| X|  |  |
| e-posti| X| X| X|  |
| mailNickname| X| X| X|  |
| managedBy|  |  | X|  |
| haldur| X| X|  |  |
| liikme|  |  | X|  |
| mobiilse| X| X|  |  |
| msExchHideFromAddressLists| X| X| X|  |
| msRTCSIP-ApplicationOptions| X|  |  |  |
| msRTCSIP DeploymentLocator| X| X|  |  |
| msRTCSIP-joon| X| X|  |  |
| msRTCSIP-OptionFlags| X| X|  |  |
| msRTCSIP-OwnerUrn| X|  |  |  |
| msRTCSIP-PrimaryUserAddress| X| X|  |  |
| msRTCSIP-UserEnabled| X| X|  |  |
| objectSID| X|  | X| omaduste. AD kasutaja identifikaator kasutada sünkroonimise vahel Azure'i AD- ja AD.|
| otherTelephone| X| X|  |  |
| physicalDeliveryOfficeName| X| X|  |  |
| Sihtnumber| X| X|  |  |
| preferredLanguage| X|  |  |  |
| proxyAddresses| X| X| X|  |
| pwdLastSet| X|  |  | omaduste. Kasutada teada, millal seadmele juba antud märgid. Kasutada nii parooli sünkroonimise ja federation.|
| securityEnabled|  |  | X| Tuletatud groupType|
| SN| X| X|  |  |
| sourceAnchor| X| X| X| omaduste. Püsiv identifikaator säilitamiseks lisatakse ja Azure AD seos.|
| st| X| X|  |  |
| streetAddress| X| X|  |  |
| telephoneNumber| X| X|  |  |
| thumbnailPhoto| X| X|  |  |
| pealkiri| X| X|  |  |
| usageLocation| X|  |  | omaduste. Kasutaja riik. Litsentsi määramine kasutada.|
| userPrincipalName| X|  |  | UPN-i on kasutaja sisselogimise ID-d. Kõige sagedamini sama nimega [meil] väärtuse.|
| wWWHomePage| X| X|  |  |

## <a name="azure-rms"></a>Azure RMS-i

| Atribuudi nimi| Kasutaja| Kontakti| Rühma| Kommentaari |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | Määratleb kui konto on lubatud.|
| CN| X|  | X| Levinud nimi või pseudonüüm. Kõige sagedamini eesliite väärtus [meil].|
| displayName| X| X| X| String, mis tähistab sageli näidatud sõbralik nimi (eesnimi perekonnanimi) nime.|
| e-posti| X| X| X| täielik meiliaadress.|
| liikme|  |  | X|  |
| objectSID| X|  | X| omaduste. AD kasutaja identifikaator kasutada sünkroonimise vahel Azure'i AD- ja AD.|
| proxyAddresses| X| X| X| omaduste. Kasutada Azure AD. Sisaldab kasutaja jaoks kõik teisene meiliaadressid.|
| pwdLastSet| X|  |  | omaduste. Kasutada teada, millal seadmele juba antud märgid.|
| securityEnabled|  |  | X| Saadud groupType.|
| sourceAnchor| X| X| X| omaduste. Püsiv identifikaator säilitamiseks lisatakse ja Azure AD seos.|
| usageLocation| X|  |  | omaduste. Kasutaja riik. Litsentsi määramine kasutada.|
| userPrincipalName| X|  |  | See UPN-i on kasutaja sisselogimise ID-d. Kõige sagedamini sama nimega [meil] väärtuse.|

## <a name="intune"></a>Intune

| Atribuudi nimi| Kasutaja| Kontakti| Rühma| Kommentaari |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | Määratleb kui konto on lubatud.|
| c| X| X|  |  |
| CN| X|  | X|  |
| kirjeldus| X| X| X|  |
| displayName| X| X| X|  |
| e-posti| X| X| X|  |
| mailnickname| X| X| X|  |
| liikme|  |  | X|  |
| objectSID| X|  | X| omaduste. AD kasutaja identifikaator kasutada sünkroonimise vahel Azure'i AD- ja AD.|
| proxyAddresses| X| X| X|  |
| pwdLastSet| X|  |  | omaduste. Kasutada teada, millal seadmele juba antud märgid. Kasutada nii parooli sünkroonimise ja federation.|
| securityEnabled|  |  | X| Tuletatud groupType|
| sourceAnchor| X| X| X| omaduste. Püsiv identifikaator säilitamiseks lisatakse ja Azure AD seos.|
| usageLocation| X|  |  | omaduste. Kasutaja riik. Litsentsi määramine kasutada.|
| userPrincipalName| X|  |  | UPN-i on kasutaja sisselogimise ID-d. Kõige sagedamini sama nimega [meil] väärtuse.|

## <a name="dynamics-crm"></a>Dynamics CRM-iga

| Atribuudi nimi| Kasutaja| Kontakti| Rühma| Kommentaari |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | Määratleb kui konto on lubatud.|
| c| X| X|  |  |
| CN| X|  | X|  |
| Co| X| X|  |  |
| ettevõtte| X| X|  |  |
| koos riigi| X| X|  |  |
| kirjeldus| X| X| X|  |
| displayName| X| X| X|  |
| facsimiletelephonenumber| X| X|  |  |
| givenName| X| X|  |  |
| l| X| X|  |  |
| managedBy|  |  | X|  |
| haldur| X| X|  |  |
| liikme|  |  | X|  |
| mobiilse| X| X|  |  |
| objectSID| X|  | X| omaduste. AD kasutaja identifikaator kasutada sünkroonimise vahel Azure'i AD- ja AD.|
| physicalDeliveryOfficeName| X| X|  |  |
| Sihtnumber| X| X|  |  |
| preferredLanguage| X|  |  |  |
| pwdLastSet| X|  |  | omaduste. Kasutada teada, millal seadmele juba antud märgid. Kasutada nii parooli sünkroonimise ja federation.|
| securityEnabled|  |  | X| Tuletatud groupType|
| SN| X| X|  |  |
| sourceAnchor| X| X| X| omaduste. Püsiv identifikaator säilitamiseks lisatakse ja Azure AD seos.|
| st| X| X|  |  |
| streetAddress| X| X|  |  |
| telephoneNumber| X| X|  |  |
| pealkiri| X| X|  |  |
| usageLocation| X|  |  | omaduste. Kasutaja riik. Litsentsi määramine kasutada.|
| userPrincipalName| X|  |  | UPN-i on kasutaja sisselogimise ID-d. Kõige sagedamini sama nimega [meil] väärtuse.|

## <a name="3rd-party-applications"></a>rakenduste 3.
Selles rühmas on kasutada minimaalsete atribuute, mis on vajalikud üldise töökoormus või rakenduse atribuutide kogum. Saab kasutada, on töökoormus pole loendis teine lõik või mitte-Microsofti rakenduse. Konkreetselt kasutatakse järgmist:

- Yammeri (ainult kasutaja tarbitud)
- [Pakutud ressursid nagu SharePointi hübriid Business Business (B2B) rist-organisatsiooniskeemi koostöö stsenaariumid](http://go.microsoft.com/fwlink/?LinkId=747036)

Selles rühmas on atribuute, mida saate kasutada, kui ei kasutata Azure AD kataloogi tugi Office 365, Dynamics või Intune kogum. See on võrdlemisi väike hulk core atribuute.

| Atribuudi nimi| Kasutaja| Kontakti| Rühma| Kommentaari |
| --- | :-: | :-: | :-: | --- |
| accountEnabled| X|  |  | Määratleb kui konto on lubatud.|
| CN| X|  | X|  |
| displayName| X| X| X|  |
| givenName| X| X|  |  |
| e-posti| X|  | X|  |
| managedBy|  |  | X|  |
| mailNickName| X| X| X|  |
| liikme|  |  | X|  |
| objectSID| X|  |  | omaduste. AD kasutaja identifikaator kasutada sünkroonimise vahel Azure'i AD- ja AD.|
| proxyAddresses| X| X| X|  |
| pwdLastSet| X|  |  | omaduste. Kasutada teada, millal seadmele juba antud märgid. Kasutada nii parooli sünkroonimise ja federation.|
| SN| X| X|  |  |
| sourceAnchor| X| X| X| omaduste. Püsiv identifikaator säilitamiseks lisatakse ja Azure AD seos.|
| usageLocation| X|  |  | omaduste. Kasutaja riik. Litsentsi määramine kasutada.|
| userPrincipalName| X|  |  | UPN-i on kasutaja sisselogimise ID-d. Kõige sagedamini sama nimega [meil] väärtuse.|

## <a name="windows-10"></a>Windows 10
Windows 10 domeeni ühendatud computer(device) sünkroonib Azure AD osa atribuute. Stsenaariumid kohta leiate lisateavet teemast [ühenduse loomine domeeni ühendatud seadmete Azure AD Windows 10 jaoks kogemusi](active-directory-azureadjoin-devices-group-policy.md). Neid atribuute alati sünkroonida ja Windows 10 rakendus, mida saate valimise ei kuvata. Windows 10 domeeni ühendatud arvuti on tähistatud on atribuut userCertificate, mis on täidetud.

| Atribuudi nimi| Seadme| Kommentaari |
| --- | :-: | --- |
| accountEnabled| X| |
| deviceTrustType| X| Kõva väärtus domeeni ühendatud arvutites. |
| displayName | X| |
| MS-DS-CreatorSID | X| Nimetatakse ka registeredOwnerReference.|
| FederatedIdentity | X| Nimetatakse ka deviceID.|
| objectSID | X| Nimetatakse ka onPremisesSecurityIdentifier.|
| operatingSystem | X| Nimetatakse ka deviceOSType.|
| operatingSystemVersion | X| Nimetatakse ka deviceOSVersion.|
| userCertificate | X| |

Neid atribuute **kasutaja** on lisaks muude rakenduste teie valitud.  

| Atribuudi nimi| Kasutaja| Kommentaari |
| --- | :-: | --- |
| domainFQDN| X| Nimetatakse ka dnsDomainName. Nt contoso.com.|
| domainNetBios| X| Nimetatakse ka netBiosName. Näiteks CONTOSO.|

## <a name="exchange-hybrid-writeback"></a>Exchange'i hübriidjuurutuse tagasikirjutusega
Neid atribuute kirjutada tagasi: Azure'i AD kohapealse Active Directory **Exchange'i hübriidjuurutuse**lubamiseks valimisel. Sõltuvalt teie Exchange'i versiooni, võivad sünkroonida vähem atribuute.

| Atribuudi nimi| Kasutaja| Kontakti| Rühma| Kommentaari |
| --- | :-: | :-: | :-: | --- |
| Vaevuste-ExternalDirectoryObjectID| X|  |  | Saadud cloudAnchor Azure AD. See atribuut on uus Exchange 2016.|
| msExchArchiveStatus| X|  |  | Võrguarhiivi: Võimaldab arhiivida e-posti kliendid.|
| msExchBlockedSendersHash| X|  |  | Filtreerimine: Kirjutab tagasi kohapealse filtreerimise ja online turvalised ja blokeeritud saatjate andmed kliendi kaudu.|
| msExchSafeRecipientsHash| X|  |  | Filtreerimine: Kirjutab tagasi kohapealse filtreerimise ja online turvalised ja blokeeritud saatjate andmed kliendi kaudu.|
| msExchSafeSendersHash| X|  |  | Filtreerimine: Kirjutab tagasi kohapealse filtreerimise ja online turvalised ja blokeeritud saatjate andmed kliendi kaudu.|
| msExchUCVoiceMailSettings| X|  |  | Ühendatud sõnumside (UM) – Online kõneposti lubada: kasutavad Microsoft Lync Server näitamaks, et Lync serveri integreerimise kohapealse, et kasutajal on võrguteenuste kõneposti.|
| msExchUserHoldPolicies| X|  |  | Hoide ootele: Võimaldab pilveteenustega määratlemaks, millistel kasutajatel on Hoide hoidke all.|
| proxyAddresses| X| X| X| Ainult selle x500 Exchange Online'ist aadress on lisatud.|

## <a name="device-writeback"></a>Seadme tagasikirjutusega
Seadme objektid luuakse Active Directory. Need võivad olla seadmete ühendatud Azure AD või Windows 10 domeeni ühendatud arvutites.

| Atribuudi nimi| Seadme| Kommentaari |
| --- | :-: | --- |
| altSecurityIdentities | X| |
| displayName | X| |
| DN | X| |
| Vaevuste-CloudAnchor | X| |
| Vaevuste-DeviceID | X| |
| Vaevuste-DeviceObjectVersion | X| |
| Vaevuste-DeviceOSType | X| |
| Vaevuste-DeviceOSVersion | X| |
| Vaevuste-DevicePhysicalIDs | X| |
| Vaevuste-KeyCredentialLink | X| Ainult Windows Server 2016 AD skeemi abil |
| Vaevuste-IsCompliant | X| |
| Vaevuste-IsEnabled | X| |
| Vaevuste-IsManaged | X| |
| Vaevuste-RegisteredOwner | X| |


## <a name="notes"></a>Märkmete

- Alternatiivne ID kasutamisel sünkroonitakse kohapealse atribuut userPrincipalName Azure'i AD-atribuut onPremisesUserPrincipalName. Alternatiivne ID-atribuudid, näiteks e-posti, sünkroonitakse Azure'i AD-atribuut userPrincipalName.
- Ülal loetletud objekti tüüp **kasutaja** kehtib ka objekti tüüp **iNetOrgPerson**.

## <a name="next-steps"></a>Järgmised sammud
Lugege lisateavet [sünkroonimine Azure'i AD-ühenduse](active-directory-aadconnectsync-whatis.md) konfiguratsiooni.

Lugege lisateavet [oma kohapealse identiteedid Azure Active Directory integreerimine](active-directory-aadconnect.md).
