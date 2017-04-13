<properties
    pageTitle="Azure'i AD domeeniteenused: Luba parooli sünkroonimise | Microsoft Azure'i"
    description="Azure Active Directory domeeniteenused töötamise alustamine"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/20/2016"
    ms.author="maheshu"/>

# <a name="enable-password-synchronization-to-azure-ad-domain-services"></a>Azure'i AD domeeniteenused parooli sünkroonimise lubamine
Ülesanded, saate lubada Azure AD domeeniteenused oma Azure AD rentniku jaoks. Järgmise ülesande on Azure AD domeeniteenused paroolide sünkroonimise lubamiseks. Pärast mandaadi sünkroonimine on häälestatud, kasutajad saavad sisse logida hallatava domeeni ettevõtte mandaadi abil.

Seotud toimingud on erinevad vastavalt kas teie ettevõttel on vaid Azure AD rentniku või kohapealse kataloogi sünkroonimine on seatud kasutab Azure'i AD-ühenduse.

<br>

> [AZURE.SELECTOR]
- [Vaid Azure AD rentniku](active-directory-ds-getting-started-password-sync.md)
- [Sünkroonitud Azure AD rentniku](active-directory-ds-getting-started-password-sync-synced-tenant.md)

<br>


## <a name="task-5-enable-password-synchronization-to-aad-domain-services-for-a-synced-azure-ad-tenant"></a>Ülesanne 5: Luba parooli sünkroonimine AAD domeeniteenused sünkroonitud Azure AD rentniku
A sünkroonitud Azure AD rentniku on seatud abil Azure'i AD-ühenduse ettevõtte kohapealse kataloogi sünkroonimine. Azure'i AD-ühendus ei sünkroonita Kerberose ja NTLM mandaati hashes Azure AD vaikimisi. Azure'i AD domeeni teenuste kasutamiseks peate konfigureerima Azure'i AD-ühenduse sünkroonida mandaati hashes nõutavaid NTLM ja Kerberos-autentimist. Järgmised toimingud lubada sünkroonimine oma Azure AD rentniku hashes vajalik mandaat.


### <a name="install-or-update-azure-ad-connect"></a>Installimine või Azure'i AD-ühenduse värskendamine
Installige uusim soovitatav vabastamist Azure'i AD-ühenduse domeeniga ühendatud arvuti. Kui teil on olemasoleva eksemplari Azure'i AD-ühenduse häälestamine, peate värskendama seda kasutama uusimat versiooni Azure'i AD-ühenduse. Teadaolevad probleemid/vead, mis võib juba kindlaksmääratud vältimiseks tagada kasutate alati Azure'i AD-ühenduse uusim versioon.

**[Laadi alla Azure AD ühenduse loomine](http://www.microsoft.com/download/details.aspx?id=47594)**

Soovitatav versioon: **1.1.281.0** - 7 August 2016 avaldada.

  > [AZURE.WARNING] Installige uusim soovitatav vabastamist Azure'i AD-ühenduse sünkroonida oma Azure AD rentniku pärand parooli identimisteabe (nõutav NTLM ja Kerberos-autentimist) lubamiseks. See funktsioon pole saadaval varasemate versioonide Azure'i AD-ühenduse või pärand tööriistaga DirSync.

Azure'i AD-ühenduse installimisjuhiseid on saadaval järgmises artiklis - [Azure'i AD-ühenduse alustamine](../active-directory/active-directory-aadconnect.md)


### <a name="enable-synchronization-of-ntlm-and-kerberos-credential-hashes-to-azure-ad"></a>Kerberose ja NTLM mandaati hashes Azure AD sünkroonimise lubamine
Iga AD mets, täielik parooli sünkroonimise, alustamiseks järgmist PowerShelli skripti käivitada ja kohapealse kõikide kasutajate identimisteavet hashes sünkroonida oma Azure AD rentniku lubamine. See skript võimaldab NTLM/Kerberos-autentimine sünkroonimist oma Azure AD rentniku jaoks vajalik mandaadi hashes.

```
$adConnector = "<CASE SENSITIVE AD CONNECTOR NAME>"  
$azureadConnector = "<CASE SENSITIVE AZURE AD CONNECTOR NAME>"  
Import-Module adsync  
$c = Get-ADSyncConnector -Name $adConnector  
$p = New-Object Microsoft.IdentityManagement.PowerShell.ObjectModel.ConfigurationParameter "Microsoft.Synchronize.ForceFullPasswordSync", String, ConnectorGlobal, $null, $null, $null
$p.Value = 1  
$c.GlobalParameters.Remove($p.Name)  
$c.GlobalParameters.Add($p)  
$c = Add-ADSyncConnector -Connector $c  
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $azureadConnector -Enable $false   
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $azureadConnector -Enable $true  
```

Kataloogi suurusest (arv kasutajate pakuvad jne), mandaati hashes Azure AD sünkroonimine võtab aega. Paroolide saab kasutada Azure AD domeeniteenused hallatava domeeni varsti pärast mandaadi hashes on sünkroonitud Azure AD.


<br>

## <a name="related-content"></a>Seotud teemad

- [Luba parooli sünkroonimine AAD domeeniteenused vaid Azure AD kataloog](active-directory-ds-getting-started-password-sync.md)

- [Mõni Azure AD domeeniteenused hallatava domeeni haldamine](active-directory-ds-admin-guide-administer-domain.md)

- [Liitumine virtuaalse masina Windows Azure AD domeeniteenused hallatava domeeni](active-directory-ds-admin-guide-join-windows-vm.md)

- [Azure'i AD domeeniteenused hallatava domeeni punane rolli Enterprise Linux virtuaalse masina liitumine](active-directory-ds-admin-guide-join-rhel-linux-vm.md)
