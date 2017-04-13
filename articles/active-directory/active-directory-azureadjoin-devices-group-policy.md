<properties
    pageTitle="Ühenduse Azure AD domeeni ühendatud seadmete jaoks Windows 10 kogemusi | Microsoft Azure'i"
    description="Selgitatakse, kuidas saavad administraatorid konfigureerida rühmapoliitika seadmete olema domeeni ühendatud ettevõtte võrku."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""
    tags="azure-classic-portal"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>

# <a name="connect-domain-joined-devices-to-azure-ad-for-windows-10-experiences"></a>Ühenduse loomine Windows 10 kogemusi Azure AD domeeni ühendatud seadmete

Domeeni Liitu on traditsiooniline viis ettevõtted on ühendatud seadmete tööd viimase 15 aasta ja palju muud. See on lubanud kasutajad sisse logida tema seadmetes, kasutades oma Windows Server Active Directory (Active Directory) töö või kooli kontod ja lubatud seda täielikult hallata järgmistesse seadmetesse. Ettevõtted tavaliselt toetuvad kujutise võimalusi sätte seadmete kasutajad ja üldiselt System Center Configuration Manager (SCCM) või rühmapoliitika haldamiseks kasutada neid.

Domeeni liitumine Windows 10 annab järgmised eelised pärast seadmete ühendamist Azure Active Directory (Azure AD):

- Ühekordne sisselogimine (SSO) Azure AD juurde igalt poolt
- Juurdepääs ettevõtte Windowsi poe kaudu töökoha või kooli konto (Microsofti konto pole nõutav)
- Ettevõtte nõuetele vastavuse rändluse kasutaja sätted kõigis seadmetes, kasutades töökoha või kooli konto (Microsofti konto pole nõutav)
- Keeruka autentimis- ja mugav sisselogimine Microsoft Passporti ja Windows Hello töökoha või kooli kontoga
- Võimalus piirata juurdepääsu ainult seadmetes, mis vastavad ettevõtte rühmapoliitika sätted

## <a name="prerequisites"></a>Eeltingimused

Domeeni Liitu jätkuvalt kasulik. Juhul, kui soovite saada SSO Azure AD kasu, rändlusteenuse sätted koos töö või kooli konto ning juurdepääs Windowsi poe töökoha või kooli kontoga, peate järgmist:

- Azure'i AD tellimus
- Kohapealse kataloogi laiendada Azure AD Azure'i AD-ühendus
- Poliitika, mis on seatud Azure AD domeeni ühendatud seadmete ühendamine
- Windows 10 koostamine (järk 10551 või uuemat versiooni) seadmete jaoks

Töö ja Windows Hello Microsoft Passporti lubamiseks peate ka järgmist:

- Avaliku võtme infrastruktuuri (PKI) kasutaja serdid väljastamise.
- System Center Configuration Manager tehnilise eelvaate versiooni 1509. Lisateavet leiate teemadest [Microsoft süsteemi keskele Configuration Manager tehnilise eelvaate](https://technet.microsoft.com/library/dn965439.aspx#BKMK_TP3Update) ja [System Center Configuration Manager meeskonna ajaveeb](http://blogs.technet.com/b/configmgrteam/archive/2015/09/23/now-available-update-for-system-center-config-manager-tp3.aspx). See on vajalik Microsoft Passporti alusel kasutaja serdid juurutamine.

PKI juurutamise nõue alternatiivina saate teha järgmist:

- On paar domeenikontrollerid ja Windows Server 2016 Active Directory domeeniteenused.

Juurdepääsu lubamiseks saate luua rühmapoliitika sätted, mis pole täiendavad juurutuste domeeni ühendatud seadmete juurdepääsu lubamine. Hallata juurdepääsu reguleerimine seade on täidetud, on vaja järgmist:

- System Center Configuration Manager versioon 1509 tehnilise eelvaate pass stsenaariumid

## <a name="deployment-instructions"></a>Juurutamise juhised



### <a name="step-1-deploy-azure-active-directory-connect"></a>Samm 1: Juurutada Azure Active Directory ühenduse loomine

Azure'i AD-ühendus võimaldab teil ettevalmistamise kohapealse arvutite seadme objektina pilveteenuses. Juurutada Azure'i AD-ühenduse, vaadake "Installige Azure'i AD-ühenduse" [integreerimine Azure Active Directory oma kohapealse identiteedid](active-directory-aadconnect.md#install-azure-ad-connect)artiklist.

 - Kui järgisite [kohandatud installi jaoks Azure'i AD-ühenduse](./connect/active-directory-aadconnect-get-started-custom.md) (mitte Express install), siis toimige **loomine teenuse ühenduse osutage kohapealse Active Directory**, hiljem selles juhises.
 - Kui teil on ühendatud konfiguratsiooni Azure AD enne installimist Azure'i AD-ühenduse (nt kui olete juurutanud Active Directory Federation Services (AD FS) enne), seejärel järgige juhiseid **konfigureerimine AD FS-i taotlemine reeglid** hiljem selles juhises.

#### <a name="create-a-service-connection-point-in-on-premises-active-directory"></a>Punkti teenuse ühenduse loomiseks kohapealse Active Directory

Domeeni ühendatud seadmete abil soovitud ühenduspunkt automaatse registreerimisel Azure seadme registreerimise teenuse Azure AD rentniku teavet leida.

Azure'i AD-ühenduse server, käivitage PowerShelli järgmised käsud:

    Import-Module -Name "C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1";

    $aadAdminCred = Get-Credential;

    Initialize-ADSyncDomainJoinedComputerSync –AdConnectorAccount [connector account name] -AzureADCredentials $aadAdminCred;


Kui töötab cmdlet $aadAdminCred = Get-identimisteavet, kasutage vormingut *user@example.com* identimisteavet, kui Get-mandaati hüpikaken sisestatud kasutajanime.

Cmdlet lähtestamise-ADSyncDomainJoinedComputerSync... käivitamisel asendada domeeni konto, mida kasutatakse Active Directory konnektor konto [*Connectori konto nimi*].

#### <a name="configure-ad-fs-claim-rules"></a>Konfigureerimine AD FS-i nõude reeglid
Konfigureerimine AD FS-i nõude reeglid võimaldab hetke registreerimise arvuti, kuhu on Azure seadme registreerimise teenust, lubades arvutite autentida, kasutades Kerberos/NTLM AD FS-i kaudu. Selles etapis tuleb ilma arvutid saavad Azure AD viivitatud viisil (välja arvatud Azure'i AD-ühenduse sünkroonimine korda).

>[AZURE.NOTE]
Kui teil pole nimega federation serveri asutusesisese AD FS-i, järgige juhiseid oma hankija taotluste reeglite loomiseks.

AD FS server (või seansi lõpetamine ühendatud AD FS server), käivitage PowerShelli järgmised käsud:

      <#----------------------------------------------------------------------
     |   Modify the Azure AD Relying Party to include the claims needed
     |   for DomainJoin++. The rules include:
     |   -ObjectGuid
     |   -AccountType
     |   -ObjectSid
     +---------------------------------------------------------------------#>

    $existingRules = (Get-ADFSRelyingPartyTrust -Identifier urn:federation:MicrosoftOnline).IssuanceTransformRules

    $rule1 = '@RuleName = "Issue object GUID"
          c1:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "515$", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"] &&
          c2:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"]
          => issue(store = "Active Directory", types = ("http://schemas.microsoft.com/identity/claims/onpremobjectguid"), query = ";objectguid;{0}", param = c2.Value);'

    $rule2 = '@RuleName = "Issue account type for domain joined computers"
          c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "515$", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"]
          => issue(Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", Value = "DJ");'

    $rule3 = '@RuleName = "Pass through primary SID"
          c1:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "515$", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"] &&
          c2:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"]
          => issue(claim = c2);'

    $updatedRules = $existingRules + $rule1 + $rule2 + $rule3

    $crSet = New-ADFSClaimRuleSet -ClaimRule $updatedRules

    Set-AdfsRelyingPartyTrust -TargetIdentifier urn:federation:MicrosoftOnline -IssuanceTransformRules $crSet.ClaimRulesString

>[AZURE.NOTE]
Windows 10 arvutites kuvatakse autentida active oli-usalda lõpp-punkti korraldaja AD FS-i abil integreeritud Windowsi autentimist kasutades. Veenduge, et see lõpp-punkti on lubatud. Kui kasutate Web puhverserveri autentimine, ka tagama, et see lõpp-punkti avaldatakse puhvri kaudu. Saate seda teha, märkides ruudu ADFS-i/teenuste/usalda/13/windowstransport. Seda tuleks näidata, kui ka AD FS-i halduskonsooli **teenuse**lubatud > **lõpp-punktid**.


### <a name="step-2-configure-automatic-device-registration-via-group-policy-in-active-directory"></a>Samm 2: Konfigureerimine automaatse seadme registreerimine rühmapoliitika kaudu Active Directorys

Active Directory rühmapoliitika saate konfigureerida oma Windows 10 domeeni ühendatud seadmete automaatselt registreerimiseks Azure AD.

> [AZURE.NOTE]
> Uusima juhised, kuidas häälestada automaatse seadme registreerimine teemadest, [Kuidas häälestada automaatse registreerimise Windowsi domeeni liidetud seadmete Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md).
>
> Windows 10 on ümber selle rühmapoliitika malli. Kui kasutate opsüsteemi Windows 10 arvutist rühmapoliitika tööriista, kuvatakse poliitika kujul: <br>
> **Register domeeni ühendatud arvutites seadmed**<br>
> Poliitika on järgmises asukohas:<br>
> ***Arvuti konfiguratsiooni/poliitika/haldus mallid/Windows komponendid/seadme registreerimine***


## <a name="additional-information"></a>Lisateave
* [Windows 10 ettevõtte: seadmete jaoks töö kasutamine](active-directory-azureadjoin-windows10-devices-overview.md)
* [Pilveteenuse võimaluste ja Windows 10 seadmed kaudu Azure Active Directory liitumine laiendamine](active-directory-azureadjoin-user-upgrade.md)
* [Azure'i AD liitumise teave kasutamise stsenaariumid](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Ühenduse loomine Windows 10 kogemusi Azure AD domeeni ühendatud seadmete](active-directory-azureadjoin-devices-group-policy.md)
* [Azure'i AD liitumine häälestamine](active-directory-azureadjoin-setup.md)
