<properties
    pageTitle="Azure Active Directory PowerShelli eelvaade cmdlettide rühma haldamiseks Azure AD | Microsoft Azure'i"
    description="Sellelt lehelt leiate abil saate hallata oma rühmade Azure Active Directory PowerShelli näited"
    keywords="Azure'i AD Azure Active Directory, PowerShelli, rühmad, rühma haldamine"
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
    ms.date="09/29/2016"
    ms.author="curtand"/>

# <a name="azure-active-directory-preview-cmdlets-for-group-management"></a>Azure Active Directory eelvaade cmdletid rühma haldamine

> [AZURE.SELECTOR]
- [Azure'i portaal](active-directory-groups-create-azure-portal.md)
- [Azure'i klassikaline portaal](active-directory-accessmanagement-manage-groups.md)
- [PowerShelli](active-directory-accessmanagement-groups-settings-v2-cmdlets.md)

Järgmise dokumendi annab teile näiteid, kuidas PowerShelli abil saate hallata oma Azure Active Directory (Azure AD) rühma.  See sisaldab ka teavet kohta, kuidas seadistada Azure AD PowerShelli mooduli eelvaade. Esmalt peate [alla Azure AD PowerShelli moodul](http://go.microsoft.com/fwlink/p/?LinkId=828627).

## <a name="installing-the-azure-ad-powershell-module"></a>Azure'i AD PowerShelli mooduli installimist

Eelvaate AzureAD PowerShelli mooduli installimiseks kasutada järgmisi käske:

    PS C:\Windows\system32> install-module azureadpreview

Veenduge, et moodulit eelvaade on installitud, kasutage järgmist käsku:

    PS C:\Windows\system32> get-module azureadpreview

    ModuleType Version    Name                                ExportedCommands
    ---------- -------    ----                                ----------------
    Binary     1.1.146.0  azureadpreview                      {Add-AzureADAdministrati...}

Nüüd saate alustada mooduli cmdlet-käskude abil. Cmdlet-käskude AzureAD eelvaade mooduli täielikku kirjeldust, vaadake [online dokumentides](https://msdn.microsoft.com/library/azure/mt757216.aspx).

## <a name="connecting-to-the-directory"></a>Kataloogi ühenduse loomine
Enne alustamist Azure AD PowerShelli eelvaade cmdlet-käskude kasutamine rühmade haldamine, tuleb ühendada PowerShelli seansi kataloogi, mida soovite hallata. Kasutage selleks järgmine käsk:

    PS C:\Windows\system32> Connect-AzureAD -Force

Cmdlet küsib teilt identimisteavet, mida soovite kasutada oma kataloogi avamiseks. Selles näites me kasutame karen@drumkit.onmicrosoft.com tutvustamise kataloogi avamiseks. Cmdlet tulemuseks kinnituse seanss on edukalt ühendatud kataloogi kuvamiseks:

    Account                       Environment Tenant
    -------                       ----------- ------
    Karen@drumkit.onmicrosoft.com AzureCloud  85b5ff1e-0402-400c-9e3c-0f…

Nüüd saate alustada AzureAD eelvaade cmdlet-käskude abil saate hallata rühma kataloogis.

## <a name="retrieving-groups"></a>Rühmade toomine
Kataloogi rühmi toomiseks saate kasutada cmdlet-käsu Get-AzureADGroups. Kõigi rühmade kataloogis toomiseks kasutada i cmdlet ilma parameetrid:

    PS C:\Windows\system32> get-azureadgroup

Cmdlet Tagastab kõigi rühmade ühendatud kataloogis.

-ObjectId väärtuse parameetri abil saate tuua teatud rühma, mille teie määratud rühma ObjectId väärtuse:

    PS C:\Windows\system32> get-azureadgroup -ObjectId e29bae11-4ac0-450c-bc37-6dae8f3da61b

Cmdlet nüüd tagastab rühma, mille ObjectId väärtuse vastab sisestatud parameetri väärtus:

    DeletionTimeStamp            :
    ObjectId                     : e29bae11-4ac0-450c-bc37-6dae8f3da61b
    ObjectType                   : Group
    Description                  :
    DirSyncEnabled               :
    DisplayName                  : Pacific NW Support
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 9bb4139b-60a1-434a-8c0d-7c1f8eee2df9
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

Saate otsida kasutav kindla rühm parameetrit - filter. See parameeter võtab klausel ODATA filter ja tagastab kõik rühmad, mis vastavad filtri, nagu järgmises näites:

    PS C:\Windows\system32> Get-AzureADGroup -Filter "DisplayName eq 'Intune Administrators'"


    DeletionTimeStamp            :
    ObjectId                     : 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df
    ObjectType                   : Group
    Description                  : Intune Administrators
    DirSyncEnabled               :
    DisplayName                  : Intune Administrators
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 4dd067a0-6515-4f23-968a-cc2ffc2eff5c
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

Pange tähele, et AzureAD PowerShelli cmdlet-käskude rakendada OData päringu standard, leiate lisateavet [siin](https://msdn.microsoft.com/library/gg309461.aspx#BKMK_filter).

## <a name="creating-groups"></a>Rühmade loomine
Uue rühma loomiseks kataloogis cmdletiga New-AzureADGroup. Selle cmdlet-käsu loob uue turberühma nimega "Turundus":

    PS C:\Windows\system32> New-AzureADGroup -Description "Marketing" -DisplayName "Marketing" -MailEnabled $false -SecurityEnabled $true -MailNickName "Marketing"

## <a name="updating-groups"></a>Rühmade värskendamine
Olemasoleva rühma värskendamiseks kasutada cmdlet Set-AzureADGroup. Selles näites oleme muutmine atribuudi DisplayName rühma "Intune administraatorid." Esmalt ei leia rühma Get-AzureADGroup cmdlet-käsu abil ja filtreerimine atribuuti DisplayName.

    PS C:\Windows\system32> Get-AzureADGroup -Filter "DisplayName eq 'Intune Administrators'"


    DeletionTimeStamp            :
    ObjectId                     : 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df
    ObjectType                   : Group
    Description                  : Intune Administrators
    DirSyncEnabled               :
    DisplayName                  : Intune Administrators
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 4dd067a0-6515-4f23-968a-cc2ffc2eff5c
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

Järgmiseks oleme muutmine atribuudi kirjeldus uue väärtuse "Intune seadme administraatoritele":

    PS C:\Windows\system32> Set-AzureADGroup -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -Description "Intune Device Administrators"

Nüüd, kui me uuesti otsida üles rühm, siis näeme värskendatakse uue väärtuse atribuudi kirjeldus:

    PS C:\Windows\system32> Get-AzureADGroup -Filter "DisplayName eq 'Intune Administrators'"


    DeletionTimeStamp            :
    ObjectId                     : 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df
    ObjectType                   : Group
    Description                  : Intune Device Administrators
    DirSyncEnabled               :
    DisplayName                  : Intune Administrators
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 4dd067a0-6515-4f23-968a-cc2ffc2eff5c
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

## <a name="deleting-groups"></a>Rühma kustutamine
Rühmade kataloogi kustutamiseks kasutada cmdlet-käsu Eemalda-AzureADGroup järgmiselt:

    PS C:\Windows\system32> Remove-AzureADGroup -ObjectId b11ca53e-07cc-455d-9a89-1fe3ab24566b

## <a name="managing-members-of-groups"></a>Rühma liikmete haldamine
Kui teil on vaja uute liikmete lisamiseks rühma, kasutage lisa-AzureADGroupMember cmdlet-käsk. See käsk lisab liikme Intune administraatorite rühma kasutasime eelmises näites:

    PS C:\Windows\system32> Add-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -RefObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

ObjectId väärtuse - parameeter on ObjectId väärtuse, soovime liikme lisamiseks rühma ja - RefObjectId on ObjectId väärtuse, soovime liikme lisamiseks rühma kasutaja.

Olemasoleva rühma liikmete saamiseks kasutage cmdleti Get-AzureADGroupMember, nagu järgmises näites:

    PS C:\Windows\system32> Get-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df

    DeletionTimeStamp ObjectId                             ObjectType
    ----------------- --------                             ----------
                        72cd4bbd-2594-40a2-935c-016f3cfeeeea User
                        8120cc36-64b4-4080-a9e8-23aa98e8b34f User

Me varem lisatud rühma liige eemaldamiseks cmdletiga Eemalda-AzureADGroupMember, nagu siin näidatud:

    PS C:\Windows\system32> Remove-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -MemberId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

Rühma membership(s) kasutaja kinnitamiseks cmdletiga valige-AzureADGroupIdsUserIsMemberOf. Selle cmdlet-käsu võtab parameetrite ObjectId väärtuse, mille märkige ruut selle rühma liikmestaatusi kasutaja ja rühmad, mille kontrollida liikmestaatuste loendi. Rühmade loendi kuvamiseks peab olema antud kompleksarvu muutuja tüüpi "Microsoft.Open.AzureAD.Model.GroupIdsForMembershipCheck" kujul, seega me esmalt looma muutuja seda tüüpi:

    PS C:\Windows\system32> $g = new-object Microsoft.Open.AzureAD.Model.GroupIdsForMembershipCheck

Järgmiseks pakume groupIds sisse atribuut "GroupIds" selle kompleksarvu muutuja väärtused:

    PS C:\Windows\system32> $g.GroupIds = "b11ca53e-07cc-455d-9a89-1fe3ab24566b", "31f1ff6c-d48c-4f8a-b2e1-abca7fd399df"

Nüüd, kui soovime kontrollida rühmaliikmeid kasutaja koos ObjectId väärtuse 72cd4bbd-2594-40a2-935c-016f3cfeeeea $g rühmade suhtes, tuleks kasutame:

    PS C:\Windows\system32> Select-AzureADGroupIdsUserIsMemberOf -ObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea -GroupIdsForMembershipCheck $g

    OdataMetadata                                                                                               Value
    -------------                                                                                               -----
    https://graph.windows.net/85b5ff1e-0402-400c-9e3c-0f9e965325d1/$metadata#Collection(Edm.String)             {31f1ff6c-d48c-4f8a-b2e1-abca7fd399df}


Tagastatud väärtus on selle kasutaja kuulub rühmade loendi. Saate rakendada ka selle meetodi abil kontrollida kontaktid, rühmad või teenuse põhisumma liikmelisus antud loendi rühmad, valige-AzureADGroupIdsContactIsMemberOf, valige-AzureADGroupIdsGroupIsMemberOf või valige-AzureADGroupIdsServicePrincipalIsMemberOf abil

## <a name="managing-owners-of-groups"></a>Omanike rühma haldamine
Omanike rühma lisada, kasutage lisa-AzureADGroupOwner cmdlet-käsk:

    PS C:\Windows\system32> Add-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -RefObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

ObjectId väärtuse - parameeter on ObjectId väärtuse, soovime omaniku lisamiseks rühma ja - RefObjectId on ObjectId väärtuse, soovime lisamiseks rühma omaniku kasutaja.

Rühma omanikud toomiseks kasutada Get-AzureADGroupOwner:

    PS C:\Windows\system32> Get-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df

Cmdlet Tagastab määratud rühma omanikud loendit:

    DeletionTimeStamp ObjectId                             ObjectType
    ----------------- --------                             ----------
                        e831b3fd-77c9-49c7-9fca-de43e109ef67 User

Kui soovite omaniku eemaldamiseks rühmast, kasutage Eemalda-AzureADGroupOwner.

    PS C:\Windows\system32> remove-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -OwnerId e831b3fd-77c9-49c7-9fca-de43e109ef67

## <a name="next-steps"></a>Järgmised sammud

[Azure Active Directory cmdlet-käsud](http://go.microsoft.com/fwlink/p/?LinkId=808260)leiate Azure'i Active Directory PowerShelli rohkem dokumente.

* [Azure Active Directory levirühmadega ressursid juurdepääsu haldamine](active-directory-manage-groups.md)

* [Teie kohapealse identiteetide integreerimine Azure Active Directory](active-directory-aadconnect.md)
