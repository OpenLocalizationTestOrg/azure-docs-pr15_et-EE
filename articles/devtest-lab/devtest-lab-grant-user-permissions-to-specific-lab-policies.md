<properties
    pageTitle="Kindla lab poliitikate kasutusõiguste andmine | Microsoft Azure'i"
    description="Saate teada, kuidas teatud lab poliitikate DevTest Labsissa iga kasutaja vajadustest kasutusõiguste andmine"
    services="devtest-lab,virtual-machines,visual-studio-online"
    documentationCenter="na"
    authors="tomarcher"
    manager="douge"
    editor=""/>

<tags
    ms.service="devtest-lab"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/25/2016"
    ms.author="tarcher"/>

# <a name="grant-user-permissions-to-specific-lab-policies"></a>Kindla lab poliitikate kasutusõiguste andmine

## <a name="overview"></a>Ülevaade

Selles artiklis illustreerib, kuidas anda kasutajatele õigused teatud lab poliitika PowerShelli abil. Nii õigusi saab rakendada põhjal iga kasutaja vajadustele. Näiteks soovite anda teatud kasutaja VM rühmapoliitika sätted, kuid mitte maksumus poliitikate muuta.

## <a name="policies-as-resources"></a>Poliitika ressurssidena

[Azure'i Rollipõhine juurdepääsu reguleerimine](../active-directory/role-based-access-control-configure.md) artiklis kirjeldatud RBAC võimaldab vahendite Azure kohandatud juurdepääsu haldamine. RBAC abil saate eraldada ülesandeid oma meeskonna DevOps ja anda juurdepääsu üksnes summa kasutajatele vajadusest teha oma tööd.

Poliitika on DevTest Labs, ressursi tüüp, mis võimaldab RBAC toimingu **Microsoft.DevTestLab/labs/policySets/policies/**. Iga lab poliitika on ressurss poliitika ressursside kirjas ja saab nimega, mille ulatus RBAC rollile määrata.

Näiteks selleks, et anda kasutajatele lugemis-ja kirjutamisõigusega õigus **Lubatud VM suurused** poliitika, luua kohandatud rolli, mis töötab **Microsoft.DevTestLab/labs/policySets/policies/** * toiming ja seejärel määrake sobiv kasutajad selle kohandatud rolli ulatus * *Microsoft.DevTestLab/labs/policySets/policies/AllowedVmSizesInLab**.

Kohandatud rollid RBAC kohta lisateabe saamiseks lugege teemat [kohandatud rollid juurdepääsu reguleerimine](../active-directory/role-based-access-control-custom-roles.md).

##<a name="creating-a-lab-custom-role-using-powershell"></a>PowerShelli kasutamine lab kohandatud rolli loomine
Selleks, et alustada, peate lugeda järgmistest artiklitest, mis selgitab, kuidas installida ja konfigureerida Azure PowerShelli cmdlet-käsud: [https://azure.microsoft.com/blog/azps-1-0-pre](https://azure.microsoft.com/blog/azps-1-0-pre).

Kui olete häälestanud Azure PowerShelli cmdletid, saate teha järgmisi toiminguid:

- Loetle kõik toimingud/toimingud ressursi pakkuja
- Kindla rolliga loendi toimingud:
- Luua kohandatud roll

Järgmist PowerShelli skripti illustreerib näiteid, kuidas teha järgmisi toiminguid:

    ‘List all the operations/actions for a resource provider.
    Get-AzureRmProviderOperation -OperationSearchString "Microsoft.DevTestLab/*"

    ‘List actions in a particular role.
    (Get-AzureRmRoleDefinition "DevTest Labs User").Actions

    ‘Create custom role.
    $policyRoleDef = (Get-AzureRmRoleDefinition "DevTest Labs User")
    $policyRoleDef.Id = $null
    $policyRoleDef.Name = "Policy Contributor"
    $policyRoleDef.IsCustom = $true
    $policyRoleDef.AssignableScopes.Clear()
    $policyRoleDef.AssignableScopes.Add("/subscriptions/<SubscriptionID> ")
    $policyRoleDef.Actions.Add("Microsoft.DevTestLab/labs/policySets/policies/*")
    $policyRoleDef = (New-AzureRmRoleDefinition -Role $policyRoleDef)

##<a name="assigning-permissions-to-a-user-for-a-specific-policy-using-custom-roles"></a>Õiguste määramisel kasutaja jaoks teatud poliitika, kasutades kohandatud rollid
Kui olete määratlenud oma kohandatud rollid, saate need määrata kasutajatele. Selleks, et kasutajale määrata kohandatud rolli, tuleb kõigepealt hankida **ObjectId väärtuse** , mis tähistab kasutaja. Selleks, et kasutada cmdlet **Get-AzureRmADUser** .

Järgmises näites on **ObjectId väärtuse** *SomeUser* kasutaja 05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3.

    PS C:\>Get-AzureRmADUser -SearchString "SomeUser"

    DisplayName                    Type                           ObjectId
    -----------                    ----                           --------
    someuser@hotmail.com                                          05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3

Kui olete **ObjectId väärtuse** kasutaja ja kohandatud nime, saate määrata rolli kasutajale **Uus-AzureRmRoleAssignment** cmdlet-käsk:

    PS C:\>New-AzureRmRoleAssignment -ObjectId 05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3 -RoleDefinitionName "Policy Contributor" -Scope /subscriptions/<SubscriptionID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.DevTestLab/labs/<LabName>/policySets/policies/AllowedVmSizesInLab

Eelmises näites kasutatakse **AllowedVmSizesInLab** poliitika. Võite kasutada mõnda järgmist poliitikat:

- MaxVmsAllowedPerUser
- MaxVmsAllowedPerLab
- AllowedVmSizesInLab
- LabVmsShutdown

[AZURE.INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Järgmised sammud

Kui olete andnud kasutaja õigused teatud lab poliitikad, siin on mõned järgmised toimingud:

- [Turvaline juurdepääs lab](devtest-lab-add-devtest-user.md).

- [Lab poliitika määramine](devtest-lab-set-lab-policy.md).

- [Lab malli loomine](devtest-lab-create-template.md).

- [Loo kohandatud esemeid oma vms](devtest-lab-artifact-author.md).

- [Lisa koos esemeid lab VM](devtest-lab-add-vm-with-artifacts.md).
