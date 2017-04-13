<properties
    pageTitle="Automaatse registreerimise Windowsi domeeni ühendatud seadmete Azure Active Directory häälestamine | Microsoft Azure'i"
    description="Häälestada oma domeeni ühendatud seadmete Windows Azure Active Directory automaatselt ja vaikselt registreerida."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="markvi"/>



# <a name="set-up-automatic-registration-of-windows-domain-joined-devices-with-azure-active-directory"></a>Automaatse registreerimise Windowsi domeeni ühendatud seadmete Azure Active Directory häälestamine

[Azure Active Directory device põhineva juurdepääsu](active-directory-conditional-access.md)kasutamiseks peab teie Windowsi domeeni ühendatud arvutites registreerunud Azure Active Directory (Azure AD). Selles artiklis saate teada, mida peate tegema selleks, et häälestada Windows domeeni ühendatud seadmete Azure AD ettevõtte registreerimise.

Kasutades juurdepääsu Azure AD annab teile järgmised eelised:

-   Täiustatud Ühekordne sisselogimine (SSO) teenuses Azure AD rakenduste kaudu töö-või koolikonto
-   Ettevõtte rändlusteenuse sätted kõigis seadmetes
-   Juurdepääs Windowsi poe ettevõtetele
-   Tugevam autentimis- ja mugav sisselogimine Windows Hello

> [AZURE.NOTE] Windows 10 November Update pakub mõningaid täiustatud kasutuskogemuse Azure AD, kuid Windows 10 tähtpäeva Update toetab täielikult device põhineva juurdepääsu. Juurdepääsu kohta leiate lisateavet teemast [Azure Active Directory device põhineva juurdepääsu](active-directory-conditional-access.md). Windows 10 seadmes töökohal ja kuidas kasutaja registreerib Windows 10 seadmesse Azure AD kohta leiate lisateavet teemast [ettevõtte Windows 10: kasutage seadmete jaoks töö](active-directory-azureadjoin-windows10-devices-overview.md).

Varasemates versioonides Windows, sealhulgas nende versioonide registreerimine

-   Windows 8.1
-   Windows 7

Kui kasutate Windows Server arvuti töölaua, saate need platvormid registreerida:

- Windows Server 2016
- Windows Server 2012 R2
- Windows Server 2012
- Windows Server 2008 R2


## <a name="prerequisites"></a>Eeltingimused

Peamised automaatse registreerimise nõude domeeni ühendatud seadmete abil Azure AD on ajakohane versiooni Azure Active Directory ühenduse (Azure'i AD-ühenduse).

Sõltuvalt sellest, kuidas saate juurutada Azure'i AD-ühenduse ja kas kasutasite kiire või kohandatud installi või kohapealse täienduse, järgmine kohustuslik tarkvara võib olla konfigureeritud automaatselt:

-   **Klõpsake ühenduspunkt kohapealse Active Directory**. Discovery Azure AD rentniku teavet arvutitest mis Registreeruge Azure AD.

-   **Active Directory Federation Services (AD FS) emissiooni muuta reeglid**. Arvuti autentimise registreerimist (korral ühendatud konfiguratsioone).

Kui teie organisatsioonide mõne seadme pole Windows 10 domeeni ühendatud seadmete, veenduge, et saate teha järgmisi toiminguid:

- Seadnud poliitika Azure AD nii, et kasutajad saavad registreerida seadmed
- Määrata lubatud alternatiiv mitmikautentimise AD FS-i integreeritud Windowsi autentimine (IWA)



## <a name="set-a-service-connection-point-for-discovery-of-the-azure-ad-tenant"></a>Määrake teenuse ühenduspunkt discovery Azure AD rentniku jaoks

Nime andmise kontekstis partition, kui arvutis on ühendatud domeeni mets konfiguratsiooni kuuluma teenuse ühenduse punkti objekti. Teenuse ühenduspunkt sisaldab discovery teavet Azure AD rentniku, kus arvutite registreerida. Mitme mets Active Directory konfiguratsiooni, peab ühenduspunkt on olemas kõik metsad domeeni ühendatud arvutites.

Ühenduspunkt teenus Active Directory järgmistesse kohtadesse järgmiselt:

    CN=62a0ff2e-97b9-4513-943f-0d221bd30080,CN=Device Registration Configuration,CN=Services,[Configuration Naming Context]

> [AZURE.NOTE] Mets koos selle Active Directory domeeni nimi *example.com*, konfiguratsiooni nime andmise kontekstis on CN = konfiguratsiooni, AV = näiteks AV = com.

Järgmised Windows PowerShelli skripti abil saate otsida Azure AD rentniku objekt ja on discovery väärtused. (Asendage konfiguratsiooni nimetamise konteksti näites oma konfiguratsioon nimetamise konteksti.)

    $scp = New-Object System.DirectoryServices.DirectoryEntry;

    $scp.Path = "LDAP://CN=62a0ff2e-97b9-4513-943f-0d221bd30080,CN=Device Registration Configuration,CN=Services,CN=Configuration,DC=example,DC=com";

    $scp.Keywords;

Funktsiooni `$scp.Keywords` väljund näitab Azure AD rentniku teave:

    azureADName:microsoft.com

    azureADId:72f988bf-86f1-41af-91ab-2d7cd011db47

Kui ühenduspunkt teenus pole olemas, looge järgmist PowerShelli skripti Azure'i AD-ühenduse server töötab:

    Import-Module -Name "C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1";

    $aadAdminCred = Get-Credential;

    Initialize-ADSyncDomainJoinedComputerSync –AdConnectorAccount [connector account name] -AzureADCredentials $aadAdminCred;


> [AZURE.NOTE]Kui käivitate `$aadAdminCred = Get-Credential`, kasutage vormingut *user@example.com* **Get-mandaati** dialoogiboksi kasutajanime.  
> Lähtesta-ADSyncDomainJoinedComputerSync cmdlet käivitamisel asendamine [*Connectori konto nimi*] on kasutusel Active Directory konnektor konto domeeni kontoga.  
> Cmdlet kasutab Active Directory PowerShelli moodul, mis esitab domeenikontrolleri Active Directory Web Services. Active Directory Web Services on toetatud domeenikontrollerite Windows Server 2008 R2 ja uuemad versioonid. Domeenikontrollerite Windows Server 2008 või varasemates versioonides, System.DirectoryServices API kaudu PowerShelli abil saate luua teenuse ühenduspunkt ja seejärel määrake märksõnade väärtused.

## <a name="create-ad-fs-rules-for-instant-device-registration-in-federated-organizations"></a>Saate luua reeglid AD FS-i kiirsõnumite seadme registreerimine ühendatud organisatsioonide

Ühendatud Azure AD konfiguratsioon seadmetes sõltuvad AD FS-i (või kohapealse liiduserveri) Azure AD autentimiseks. Seejärel registreerige vastu Azure Active Directory seadme registreerimise teenust (Azure'i AD seadme registreerimise teenus).

> [AZURE.NOTE] Kasutaja parooli hashes on sünkroonitud Azure AD ühendamata konfigureerimine ja Windows 10 ja Windows Server 2016 domeeni ühendatud arvutites autentimiseks Azure AD seadme registreerimise teenust. Kasutaja autendib abil identimisteavet, mida kasutaja kirjutab oma kohapealse arvuti kontod ja mis on edastatakse Azure AD Azure'i AD-ühenduse kaudu. Windows 10 ja Windows Server 2016 ühendamata konfiguratsiooni, peate suvandite määramise seadme vastavalt sertimiskeskus ettevõtte. Lisateabe saamiseks vt Windows 10 arvutites selle artikli jaotist allalaadimine Windows Installeri paketid.

Windows 10 ja Windows Server 2016 arvutites Azure'i AD-ühenduse seostab seadme objekti Azure AD kohapealse arvuti konto objekti. Järgmised nõuded tuleb olemas autentimisel Azure AD seadme registreerimise teenuse registreerimise lõpuleviimiseks ja seadme objekti loomiseks:

- http://schemas.microsoft.com/ws/2012/01/accounttype sisaldab DJ väärtust, mis tuvastab põhisumma Autentija kui domeeni ühendatud arvuti.
- http://schemas.microsoft.com/Identity/claims/onpremobjectguid sisaldab väärtust **atribuutiFederatedIdentityDistinguishedNameatribuuti kohapealse arvuti kontot** .
- http://schemas.microsoft.com/ws/2008/06/Identity/claims/primarysid sisaldab arvuti esmane turvalisuse ID (SID), mis vastab **objectSid** atribuudi väärtus kohapealse arvuti kontot.
- http://schemas.microsoft.com/ws/2008/06/Identity/claims/issuerid sisaldab väärtust, mis on Azure AD kasutab AD FS-i või: kohapealse turvalisus Turbeloa teenuse (STS) väljastatud luba usaldada. See on oluline mitme mets Active Directory konfiguratsiooni. Selle konfiguratsiooni puhul võib mets peale ühe, mis loob ühenduse Azure AD AD FS või kohapealse STS ühendatud arvutites. Kasutage AD FS-i väärtust http://<*domeeni nimi*>/ADFS-i/teenuste usalda /, kus <*domeeni nimi*> on kinnitatud domeeninime Azure AD.

Loomiseks reegleid käsitsi AD FS-i, kasutage järgmist PowerShelli skripti seanss, mis on seotud serverisse. Asendage esirea Azure AD teie asutuse kinnitatud domeeni nimi.

> [AZURE.NOTE] Kui te ei kasuta AD FS-i jaoks oma kohapealse liiduserveri, järgige oma tootja juhiseid nende nõuete probleemi reeglite loomiseks.

    $validatedDomain = "example.com"      # Replace example.com with your organization's validated domain name in Azure AD

    $rule1 = '@RuleName = "Issue object GUID"

        c1:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "515$", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"] &&

        c2:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"]

        => issue(store = "Active Directory", types = ("http://schemas.microsoft.com/identity/claims/onpremobjectguid"), query = ";objectguid;{0}", param = c2.Value);'

    $rule2 = '@RuleName = "Issue account type for domain-joined computers"

      c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "515$", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"]

      => issue(Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", Value = "DJ");'

> [AZURE.NOTE] Kasutage ainult kolme esimest reeglit, kui ühe mets on teie keskkonnas kohapealse. Kui teie arvutis on erinevate mets, kui üks Azure AD sünkroonimine või kui kasutate alternatiivse nimed, et need sünkroonimise konfiguratsiooni, peate lisama ka ülejäänud reeglite.

    $rule3 = '@RuleName = "Pass through primary SID"

      c1:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "515$", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"] &&

      c2:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"]

      => issue(claim = c2);'

    $rule4 = '@RuleName = "Issue AccountType with the value User when it’s not a computer account"

      NOT EXISTS([Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", Value == "DJ"])

      => add(Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", Value = "User");'

    $rule5 = '@RuleName = "Capture UPN when AccountType is User and issue the IssuerID"

      c1:[Type == "http://schemas.xmlsoap.org/claims/UPN"] &&

      c2:[Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", Value == "User"]

      => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c1.Value, ".+@(?<domain>.+)", "http://${domain}/adfs/services/trust/"));'

    $rule6 = '@RuleName = "Update issuer for DJ computer auth"

      c1:[Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", Value == "DJ"]

      => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = "http://'+$validatedDomain+'/adfs/services/trust/");'

    $existingRules = (Get-ADFSRelyingPartyTrust -Identifier urn:federation:MicrosoftOnline).IssuanceTransformRules

    $updatedRules = $existingRules + $rule1 + $rule2 + $rule3 + $rule4 + $rule5 + $rule6

    $crSet = New-ADFSClaimRuleSet -ClaimRule $updatedRules

    Set-AdfsRelyingPartyTrust -TargetIdentifier urn:federation:MicrosoftOnline -IssuanceTransformRules $crSet.ClaimRulesString

> [AZURE.NOTE] Windows 10 ja Windows Server 2016 domeeni ühendatud arvutites autentida IWA aktiivne oli-usalda lõpp majutatud AD FS-i abil. Veenduge, et lõpp-punkti on aktiivne. Kui kasutate teenuserakenduse puhverserverit, kindlasti, et see lõpp-punkti avaldatakse puhvri kaudu. Lõpp-punkti on ADFS-i/teenuste/usalda/13/windowstransport. Kontrollige, kas see on aktiivne, minge halduskonsooli AD FS-i **teenuse** > **lõpp-punktid**. Kui te ei kasuta AD FS-i jaoks oma kohapealse liiduserveri, järgige oma tootja juhiseid, veenduge, et vastav lõpp-punkti on aktiivne.



## <a name="set-up-ad-fs-for-authentication-of-device-registration"></a>Seadme registreerimise autentimise AD FS-i häälestamine

Veenduge, et IWA on määratud lubatud alternatiiv mitmikautentimise AD FS seadme registreerimiseks. Selle tegemiseks peate mõne emissiooni reeglit, mida läbib autentimise meetodit muuta.

1. AD FS-i halduskonsoolis, minge **AD FS-i** > **Usaldada seosed** > **Tuginedes tootja loodab**.

2. Paremklõpsake Microsoft Office 365 identiteedi platvormi tuginedes tootja usalda objekti ja seejärel valige **Redigeeri nõude reeglid**.

3.  Valige vahekaardi **Emissiooni muuta reeglid** **Lisa reegel**.

4.  Valige loendis **taotluste reegli** malli **saatmine taotluste kohandatud reegli abil**.

5.  Valige **edasi**.

6.  Sisestage väljale **taotluste reegli nimi** **Auth meetodit taotluste reegel**.

7.  Sisestage väljale **taotluste reegli** see käsk:

    `c:[Type == "http://schemas.microsoft.com/claims/authnmethodsreferences"]
    => issue(claim = c);`.

8. Sisestage oma liiduserveri selle PowerShelli käsu.

    `Set-AdfsRelyingPartyTrust -TargetName <RPObjectName> -AllowedAuthenticationClassReferences wiaormultiauthn`

<*RPObjectName*> on Azure AD tuginedes tootja usalda objekti tuginedes tootja objekti nimi. Tavaliselt on see objekt nimega *Microsoft Office 365 identiteedi platvormi*.



## <a name="deployment-and-rollout"></a>Juurutamise ja levitamise kohta.

Kui domeeni ühendatud arvutites vasta, on valmis Azure AD registreerida.

Windows 10 tähtpäeva Update ja Windows Server 2016 domeeni ühendatud arvutites automaatselt registreerida Azure AD järgmine kord, kui seadme taaskäivitamist või kui kasutaja logib Windows. Uued arvutid, mis on ühendatud domeeni registreerida Azure AD, kui seadme taaskäivitamist pärast domeeni liitumine.

> [AZURE.NOTE] Windows 10 domeeni ühendatud arvutites registreerida Azure AD automaatselt ainult siis, kui on seatud plaanida rühmapoliitika objekti.

Rühmapoliitika objekti abil saate määrata automaatse registreerimise Windows 10 ja Windows Server 2016 domeeni ühendatud arvutites plaanida. Windows 10 domeeni ühendatud arvutites automaatse registreerimise hakkama saate juurutada Windows Installeri pakett arvutis, kus saate valida.

> [AZURE.NOTE] Rühmapoliitika plaanida juhtelemendi käivitab ka registreerimise Windows 8.1 domeeni ühendatud arvutites. Saate kasutada Windows 8.1 domeeni ühendatud arvutites registreerusite poliitika. Või kui teil on Windowsi versiooni, Windows 7 või Windows Server versioonides, sh mix saate registreerida Windows 10 ja Windows Server 2016 arvutites Windows Installeri pakett abil.

### <a name="create-a-group-policy-object-to-control-the-rollout-of-automatic-registration"></a>Määrata automaatse registreerimise plaanida rühmapoliitika objekti loomine

Määrata automaatse registreerimise domeeni ühendatud arvutites Azure AD plaanida, saate juurutada rühmapoliitika **domeeni ühendatud arvutites seadmed registreerida** arvutisse, mida soovite registreerida. Näiteks saate juurutada poliitika on Organisatsiooniüksus või turberühma.

Poliitika seadmiseks tehke järgmist.

1. Avage serveri haldur ja seejärel valige **Tööriistad** > **Rühmapoliitika haldus**.

2. Minge domeeni sõlm, mis vastab domeeni, kuhu soovite aktiveerida automaatse registreerimise Windows 10 ja Windows Server 2016 arvutitesse.

3. Paremklõpsake **Rühmapoliitika objektid**ja seejärel valige **Uus**.

4. Sisestage oma rühmapoliitika objekti nimi. Näiteks *Automaatse registreerimise Azure AD*. Klõpsake nuppu **OK**.

4. Paremklõpsake oma uue rühmapoliitika objekti ja seejärel valige **Redigeeri**.

5. Avage **Arvuti konfiguratsioon** > **poliitikate** > **haldus mallid** > **Windowsi komponentide** > **seadme registreerimine**. Paremklõpsake **Register domeeni ühendatud arvutites seadmed**ja seejärel valige **Redigeeri**.

    > [AZURE.NOTE] Selle malli rühmapoliitika on ümber nimetatud varasemate versioonide rühmapoliitika halduskonsool. Kui kasutate konsooli varasemas versioonis, avage **Arvuti konfiguratsioon** > **poliitikate** > **Haldus mallid** > **Windowsi komponentide** > **Töökoha Liitu** > **automaatselt töökoha Liitu klientarvutite**.

6. Valige **lubatud**ja seejärel valige **Rakenda**.

7. Klõpsake nuppu **OK**.

8. Rühmapoliitika objekti link soovitud kohta. Näiteks saate linkida selle teatud organisatsiooniüksusega. Saate ka võib linkida selle teatud turberühma automaatselt registreerida Azure AD arvutites. Link selle poliitika määramiseks kõik domeeni ühendatud Windows 10 ja Windows Server 2016 arvutid ettevõtte domeeni rühmapoliitika objekti.

### <a name="download-windows-installer-packages-for-non-windows-10-computers"></a>Laadige alla Windows Installeri paketid Windows 10 arvutites  

Registreerida domeeni ühendatud arvutites opsüsteemi Windows 8.1, Windows 7, Windows Server 2012 R2, Windows Server 2012 või Windows Server 2008 R2, saate alla laadida ja installida Windows Installeri pakett (MSI) faile.

- [x64](http://download.microsoft.com/download/C/A/7/CA79FAE2-8C18-4A8C-A4C0-5854E449ADB8/Workplace_x64.msi)
- [x86](http://download.microsoft.com/download/C/A/7/CA79FAE2-8C18-4A8C-A4C0-5854E449ADB8/Workplace_x86.msi)

Juurutamise paketi tarkvara jaotuse süsteemi nagu System Center Configuration Manager abil. Paketi toetab *vaiksesse* parameetriga standard vaikne installisuvandid. System Center Configuration Manager 2016 pakub täiendavaid eeliseid nagu jälgida lõplikus registreerimise varasemate versioonide. Lisateabe saamiseks lugege teemat [süsteemi Center 2016](https://www.microsoft.com/en-us/cloud-platform/system-center).

Installiprogrammi loob ajastatud süsteemi, mis töötab kasutaja kontekstis. Tööülesande käivitatakse, kui kasutaja Windowsi sisse logib. Tööülesande vaikselt registreerib seade Azure AD kasutaja mandaati pärast autentimist IWA kaudu. Ajastatud ülesanne nägemiseks minge **Microsoft** > **Töökoha liituda**, ja seejärel avage Toiminguajasti teek.



## <a name="next-steps"></a>Järgmised sammud

- [Azure Active Directory juurdepääsu](active-directory-conditional-access.md)
