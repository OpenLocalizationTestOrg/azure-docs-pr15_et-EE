<properties
    pageTitle="Muuda allkirja räsi algoritmi jaoks Office 365 vastamine poole usaldus | Microsoft Azure'i"
    description="Sellelt lehelt leiate juhised muutmise SHA algoritmi federation usalda teenusekomplektiga Office 365"
    keywords="SHA1, SHA256, O365, liiduserverite, aadconnect, ADFS-i, ad FS-i, Muuda sha, federation usalda, tuginedes poole usaldus"
    services="active-directory"
    documentationCenter=""
    authors="anandyadavmsft"
    manager="samueld"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/01/2016"
    ms.author="anandy"/>

# <a name="change-signature-hash-algorithm-for-office-365-replying-party-trust"></a>Office 365 vastamisel poole usaldus allkirja räsi algoritmi muutmine

## <a name="overview"></a>Ülevaade

Azure Active Directory Federation Services (AD FS) märke selle märkide Microsoft Azure Active Directory tagamaks, et nad ei saa koos viisil. Allkirja alusel SHA1 või SHA256. Azure Active Directory toetab nüüd sõned allkirjastatud SHA256 algoritmi ja soovitame säte Luba sisselogimise algoritmi SHA256 kõrgeima taseme turvalisus. Selles artiklis kirjeldatakse seadmiseks luba sisselogimise algoritmi turvalisemad SHA256 taseme juhiseid.

## <a name="change-the-token-signing-algorithm"></a>Luba sisselogimise algoritmi muutmine

Pärast allkirja algoritmi ühe allpool protsesside seadmiseks märke AD FS-i funktsiooni sõned tuginedes poole usaldus SHA256 koos Office 365 jaoks. Te ei pea täiendavat konfiguratsiooni muudatusi teha, ja see muudatus ei mõjuta teie võimalus juurdepääs Office 365 või muude rakenduste Azure AD.

### <a name="ad-fs-management-console"></a>AD FS-i halduskonsooli

1. Avage esmane AD FS server AD FS-i halduskonsool.
2. Laiendage AD FS-i ja klõpsake nuppu, **Tuginedes tootja loodab**.
3. Paremklõpsake oma Office 365 ja Azure tuginedes poole usaldus ja valige **Atribuudid**.
4. Valige vahekaart **Täpsemalt** ja valige turvaline räsi algoritmi SHA256.
5. Klõpsake nuppu **OK**.

![SHA256 allkirjastamiseks algoritmi--MMC-s](./media/active-directory-aadconnectfed-sha256guidance/mmc.png)

### <a name="ad-fs-powershell-cmdlets"></a>AD FS-i PowerShelli cmdlet-käsud

1. Mis tahes AD FS server, avage PowerShelli jaotises administraatori õigusi.
2. Määrake turvaline räsi algoritmi cmdlet **Set-AdfsRelyingPartyTrust** abil.

 <code>Set-AdfsRelyingPartyTrust -TargetName 'Microsoft Office 365 Identity Platform' -SignatureAlgorithm 'http://www.w3.org/2001/04/xmldsig-more#rsa-sha256'</code>

## <a name="also-read"></a>Vt ka

* [Office 365 usalduskeskuse abil Azure'i AD-ühenduse parandamine](./active-directory-aadconnect-federation-management.md#repairing-the-trust)
