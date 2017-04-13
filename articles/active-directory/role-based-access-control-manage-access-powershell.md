<properties
    pageTitle="Rollipõhine juurdepääsu reguleerimine (RBAC) Azure PowerShelli abil hallata | Microsoft Azure'i"
    description="Kuidas hallata RBAC Azure'i PowerShelliga loetelu rollid, määramine rollid ja kustutamise rollimääranguid."
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
    ms.date="07/22/2016"
    ms.author="kgremban"/>

# <a name="manage-role-based-access-control-with-azure-powershell"></a>Rollipõhine juurdepääsu reguleerimine Azure PowerShelliga haldamine

> [AZURE.SELECTOR]
- [PowerShelli](role-based-access-control-manage-access-powershell.md)
- [Azure'i CLI](role-based-access-control-manage-access-azure-cli.md)
- [REST API-GA](role-based-access-control-manage-access-rest.md)


Rollipõhine juurdepääsu reguleerimine (RBAC) Azure portaali ja Azure ressursside haldamine API abil saate oma tellimusele kohandatud tasemel juurdepääsu haldamine. Selle funktsiooniga saate anda juurdepääsu Active Directory kasutajad, rühmad või teenuse põhisumma teatud rollide määrate neile teatud ulatuses.

Enne PowerShelli abil saate hallata RBAC, peab teil olema järgmised:

- Azure'i PowerShelli versiooni 0.8.8 või uuem versioon. Installige uusim versioon ja seostada Azure tellimuse, vaadake, [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md).

- Azure'i ressursihaldur cmdlet-käsud. Installige [Azure'i ressursihaldur cmdletid](https://msdn.microsoft.com/library/mt125356.aspx) PowerShelli.

## <a name="list-roles"></a>Loendi rollid

### <a name="list-all-available-roles"></a>Loetle kõik saadaolevad rollid
Loendi RBAC rollidele, mis on saadaval ja need toimingud, kellele anda juurdepääsu kontrollimiseks kasutada `Get-AzureRmRoleDefinition`.

```
Get-AzureRmRoleDefinition | FT Name, Description
```

![RBAC PowerShelli-Get AzureRmRoleDefinition - kuvatõmmis](./media/role-based-access-control-manage-access-powershell/1-get-azure-rm-role-definition1.png)

### <a name="list-actions-of-a-role"></a>Loenditoimingud rolli
Teatud rolli toimingute loend, kasutage `Get-AzureRmRoleDefinition <role name>`.

```
Get-AzureRmRoleDefinition Contributor | FL Actions, NotActions

(Get-AzureRmRoleDefinition "Virtual Machine Contributor").Actions
```

![RBAC PowerShelli-Get AzureRmRoleDefinition teatud rolli - kuvatõmmis](./media/role-based-access-control-manage-access-powershell/1-get-azure-rm-role-definition2.png)

## <a name="see-who-has-access"></a>Pääsuõigusega isikute vaatamine
RBAC Accessi ülesannete loendi kuvamiseks kasutage `Get-AzureRmRoleAssignment`.

### <a name="list-role-assignments-at-a-specific-scope"></a>Loendi rollimääranguid teatud ulatuses
Saate vaadata Accessi ülesannete määratud tellimus, ressursirühm või ressurss. Näiteks kuvatakse kõik aktiivse määrangute ressursirühma kuvamiseks kasutada `Get-AzureRmRoleAssignment -ResourceGroupName <resource group name>`.

```
Get-AzureRmRoleAssignment -ResourceGroupName Pharma-Sales-ProjectForcast | FL DisplayName, RoleDefinitionName, Scope
```

![RBAC PowerShelli-Get AzureRmRoleAssignment ressursirühma - kuvatõmmis](./media/role-based-access-control-manage-access-powershell/4-get-azure-rm-role-assignment1.png)

### <a name="list-roles-assigned-to-a-user"></a>Loendi kasutajale määratud rolle
Loendis määratud kasutajale määratud rolle ja rühmad, kuhu kuulub kasutajale määratud rolle, kasutage `Get-AzureRmRoleAssignment -SignInName <User email> -ExpandPrincipalGroups`.

```
Get-AzureRmRoleAssignment -SignInName sameert@aaddemo.com | FL DisplayName, RoleDefinitionName, Scope

Get-AzureRmRoleAssignment -SignInName sameert@aaddemo.com -ExpandPrincipalGroups | FL DisplayName, RoleDefinitionName, Scope
```

![RBAC PowerShelli-Get AzureRmRoleAssignment kasutaja - kuvatõmmis](./media/role-based-access-control-manage-access-powershell/4-get-azure-rm-role-assignment2.png)

### <a name="list-classic-service-administrator-and-coadmin-role-assignments"></a>Loendi klassikaline teenuse administraator ja coadmin rollimääranguid
Loendi Accessi määramisi klassikaline tellimuse administraator ja coadministrators kasutamine:

    Get-AzureRmRoleAssignment -IncludeClassicAdministrators

## <a name="grant-access"></a>Juurdepääsu andmine
### <a name="search-for-object-ids"></a>Otsi objekti ID-d
Määrata rolli, peate nii objekti (kasutaja, rühma või rakendus) ja ulatuse tuvastamiseks.

Kui te ei tea Tellimuse ID, leiate Azure'i portaalis **tellimuste** tera. Tellimuse ID päringu kohta leiate teavet teemast [Get-AzureSubscription](https://msdn.microsoft.com/library/dn495302.aspx) MSDN-is.

Azure'i AD rühma objekti ID saamiseks kasutage:

    Get-AzureRmADGroup -SearchString <group name in quotes>

Objekti ID Azure AD teenuse põhilise või rakenduse saamiseks kasutage:

    Get-AzureRmADServicePrincipal -SearchString <service name in quotes>

### <a name="assign-a-role-to-an-application-at-the-subscription-scope"></a>Taotluse tellimuse ulatuse määramine
Tellimuse ulatus taotluse juurdepääsu andmiseks saate kasutada.

    New-AzureRmRoleAssignment -ObjectId <application id> -RoleDefinitionName <role name> -Scope <subscription id>

![RBAC PowerShelli uus AzureRmRoleAssignment - kuvatõmmis](./media/role-based-access-control-manage-access-powershell/2-new-azure-rm-role-assignment2.png)

### <a name="assign-a-role-to-a-user-at-the-resource-group-scope"></a>Ressursi rühma ulatuses kasutajale rolli määramine
Ressursi rühma ulatuse kasutajale juurdepääsu andmiseks saate kasutada.

    New-AzureRmRoleAssignment -SignInName <email of user> -RoleDefinitionName <role name in quotes> -ResourceGroupName <resource group name>

![RBAC PowerShelli uus AzureRmRoleAssignment - kuvatõmmis](./media/role-based-access-control-manage-access-powershell/2-new-azure-rm-role-assignment3.png)

### <a name="assign-a-role-to-a-group-at-the-resource-scope"></a>Rühmale ressursi ulatuses rolli määramine
Rühma ressursi ulatuses juurdepääsu andmiseks saate kasutada.

    New-AzureRmRoleAssignment -ObjectId <object id> -RoleDefinitionName <role name in quotes> -ResourceName <resource name> -ResourceType <resource type> -ParentResource <parent resource> -ResourceGroupName <resource group name>

![RBAC PowerShelli uus AzureRmRoleAssignment - kuvatõmmis](./media/role-based-access-control-manage-access-powershell/2-new-azure-rm-role-assignment4.png)

## <a name="remove-access"></a>Juurdepääsu eemaldamine
Eemaldamiseks juurdepääsu kasutajate, rühmade ja rakenduste kasutamiseks tehke järgmist.

    Remove-AzureRmRoleAssignment -ObjectId <object id> -RoleDefinitionName <role name> -Scope <scope such as subscription id>

![RBAC PowerShelli Eemalda AzureRmRoleAssignment - kuvatõmmis](./media/role-based-access-control-manage-access-powershell/3-remove-azure-rm-role-assignment.png)

## <a name="create-a-custom-role"></a>Luua kohandatud roll
Luua kohandatud rolli, kasutage funktsiooni `New-AzureRmRoleDefinition` käsk.

Kui loote kohandatud rolli PowerShelli abil, peate alustada ühe [sisseehitatud rollid](role-based-access-built-in-roles.md). Redigeeri atribuute *toimingud*, *notActions*või *otsinguulatuste* , mille soovite lisada, ja seejärel salvestage muudatused uude rolli.

Järgmises näites algab *Virtuaalse masina kaasautori* roll ja mis kasutab luua kohandatud rolli, nimetatakse *Virtuaalse masina tehtemärk*. Uude rolli annab kõik loetuks toimingud *Microsoft.Compute*, *Microsoft.Storage*ja *Microsoft.Network* ressursside teenusepakkuja ja annab alustada, taaskäivitage ja virtuaalmasinates jälgimine. Kahe tellimusi saab kasutada kohandatud roll.

```
$role = Get-AzureRmRoleDefinition "Virtual Machine Contributor"
$role.Id = $null
$role.Name = "Virtual Machine Operator"
$role.Description = "Can monitor and restart virtual machines."
$role.Actions.Clear()
$role.Actions.Add("Microsoft.Storage/*/read")
$role.Actions.Add("Microsoft.Network/*/read")
$role.Actions.Add("Microsoft.Compute/*/read")
$role.Actions.Add("Microsoft.Compute/virtualMachines/start/action")
$role.Actions.Add("Microsoft.Compute/virtualMachines/restart/action")
$role.Actions.Add("Microsoft.Authorization/*/read")
$role.Actions.Add("Microsoft.Resources/subscriptions/resourceGroups/read")
$role.Actions.Add("Microsoft.Insights/alertRules/*")
$role.Actions.Add("Microsoft.Support/*")
$role.AssignableScopes.Clear()
$role.AssignableScopes.Add("/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e")
$role.AssignableScopes.Add("/subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624")
New-AzureRmRoleDefinition -Role $role
```

![RBAC PowerShelli-Get AzureRmRoleDefinition - kuvatõmmis](./media/role-based-access-control-manage-access-powershell/2-new-azurermroledefinition.png)

## <a name="modify-a-custom-role"></a>Kohandatud roll muutmine
Kohandatud rolli muutmiseks kasutage funktsiooni `Get-AzureRmRoleDefinition` käsk tuua rolli määratlus. Teiseks, tehke soovitud muudatused rolli määratlus. Kasutage funktsiooni `Set-AzureRmRoleDefinition` käsu Salvesta muudetud rolli määratlus.

Järgmises näites liidetakse selle `Microsoft.Insights/diagnosticSettings/*` toimingu *Virtuaalse masina tehtemärk* kohandatud roll.

```
$role = Get-AzureRmRoleDefinition "Virtual Machine Operator"
$role.Actions.Add("Microsoft.Insights/diagnosticSettings/*")
Set-AzureRmRoleDefinition -Role $role
```

![RBAC PowerShelli-Set AzureRmRoleDefinition - kuvatõmmis](./media/role-based-access-control-manage-access-powershell/3-set-azurermroledefinition-1.png)

Järgmises näites liidetakse Azure tellimuse määratav otsinguulatuste *Virtuaalse masina tehtemärk* kohandatud roll.

```
Get-AzureRmSubscription - SubscriptionName Production3

$role = Get-AzureRmRoleDefinition "Virtual Machine Operator"
$role.AssignableScopes.Add("/subscriptions/34370e90-ac4a-4bf9-821f-85eeedead1a2"
Set-AzureRmRoleDefinition -Role $role)
```

![RBAC PowerShelli-Set AzureRmRoleDefinition - kuvatõmmis](./media/role-based-access-control-manage-access-powershell/3-set-azurermroledefinition-2.png)

## <a name="delete-a-custom-role"></a>Kohandatud rolli kustutamine

Kohandatud rolli kustutamiseks kasutage funktsiooni `Remove-AzureRmRoleDefinition` käsk.

Järgmises näites eemaldab *Virtuaalse masina tehtemärk* kohandatud roll.

```
Get-AzureRmRoleDefinition "Virtual Machine Operator"

Get-AzureRmRoleDefinition "Virtual Machine Operator" | Remove-AzureRmRoleDefinition
```

![RBAC PowerShelli Eemalda AzureRmRoleDefinition - kuvatõmmis](./media/role-based-access-control-manage-access-powershell/4-remove-azurermroledefinition.png)

## <a name="list-custom-roles"></a>Kohandatud loendi rollid
Loendis Saadaolevad ülesande juures, mille ulatus rollid, kasutage funktsiooni `Get-AzureRmRoleDefinition` käsk.

Järgmises näites on loetletud kõik rollid, mis on saadaval valitud tellimuse.

```
Get-AzureRmRoleDefinition | FT Name, IsCustom
```

![RBAC PowerShelli-Get AzureRmRoleDefinition - kuvatõmmis](./media/role-based-access-control-manage-access-powershell/5-get-azurermroledefinition-1.png)

Järgmises näites *Virtuaalse masina tehtemärk* kohandatud roll pole saadaval *Production4* tellimuse, kuna see tellimus pole **AssignableScopes** roll.

![RBAC PowerShelli-Get AzureRmRoleDefinition - kuvatõmmis](./media/role-based-access-control-manage-access-powershell/5-get-azurermroledefinition2.png)

## <a name="see-also"></a>Vt ka
- [Azure'i ressursihaldur Azure PowerShelli abil](../powershell-azure-resource-manager.md)
[AZURE.INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]
