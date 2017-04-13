<properties
    pageTitle="Rollipõhine juurdepääsu reguleerimine (RBAC) Azure CLI haldamine | Microsoft Azure'i"
    description="Saate teada, kuidas hallata Rollipõhine juurdepääsu reguleerimine (RBAC) koos Azure käsurea liides loetelu rollid ja roll toimingud ja rollide määramine tellimust ja rakenduse otsinguulatuste."
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

# <a name="manage-role-based-access-control-with-the-azure-command-line-interface"></a>Rollipõhine juurdepääsu reguleerimine Azure käsurea liides haldamine

> [AZURE.SELECTOR]
- [PowerShelli](role-based-access-control-manage-access-powershell.md)
- [Azure'i CLI](role-based-access-control-manage-access-azure-cli.md)
- [REST API-GA](role-based-access-control-manage-access-rest.md)

Rollipõhine juurdepääsu reguleerimine (RBAC) Azure portaali ja Azure ressursihaldur API abil saate hallata juurdepääsu oma tellimust ja ressursside Kohandatud tase. Selle funktsiooniga saate anda juurdepääsu Active Directory kasutajad, rühmad või teenuse põhisumma teatud rollide määrate neile teatud ulatuses.

Enne Azure käsurea liides (CLI) abil saate hallata RBAC, peab teil olema järgmised:

- Azure'i CLI 0.8.8 versioon või uuem versioon. Installige uusim versioon ja seostada Azure tellimuse, leiate teemast [installida ja konfigureerida Azure CLI](../xplat-cli-install.md).
- Azure'i ressursihaldur Azure CLI. Minge [Azure'i CLI ressursihaldur koos kasutamise](../xplat-cli-azure-resource-manager.md) rohkem üksikasju.

## <a name="list-roles"></a>Loendi rollid

### <a name="list-all-available-roles"></a>Loetle kõik saadaolevad rollid
Loetle kõik saadaolevad rollid, saate kasutada.

        azure role list

Järgmises näites on kujutatud *saadaval kõikide rollide*loend.

```
azure role list --json | jq '.[] | {"roleName":.properties.roleName, "description":.properties.description}'
```

![Azure'i RBAC käsurea - Azure'i rolli loendi - kuvatõmmis](./media/role-based-access-control-manage-access-azure-cli/1-azure-role-list.png)

### <a name="list-actions-of-a-role"></a>Loenditoimingud rolli
Loendi rolli toimingud, saate kasutada.

    azure role show "<role name>"

Järgmises näites on kujutatud *kaasautor* ja *Virtuaalse masina kaasautor* rollid toimingud.

```
azure role show "contributor" --json | jq '.[] | {"Actions":.properties.permissions[0].actions,"NotActions":properties.permissions[0].notActions}'

azure role show "virtual machine contributor" --json | jq '.[] | .properties.permissions[0].actions'
```

![Azure'i RBAC käsurea - Azure'i rolli Näita - kuvatõmmis](./media/role-based-access-control-manage-access-azure-cli/1-azure-role-show.png)

##  <a name="list-access"></a>Loendi Accessi
### <a name="list-role-assignments-effective-on-a-resource-group"></a>Loendi rollimääranguid tõhus ressursirühma
Loendi rollimääranguid, mis on olemas ressursirühma, saate kasutada.

    azure role assignment list --resource-group <resource group name>

Järgmises näites on kujutatud *pharma-müügi-projecforcast* jaotises rollimääranguid.

```
azure role assignment list --resource-group pharma-sales-projecforcast --json | jq '.[] | {"DisplayName":.properties.aADObject.displayName,"RoleDefinitionName":.properties.roleName,"Scope":.properties.scope}'
```

![Azure'i RBAC käsurea - Azure'i rolli ülesande loend, rühma-kuvatõmmis](./media/role-based-access-control-manage-access-azure-cli/4-azure-role-assignment-list-1.png)

### <a name="list-role-assignments-for-a-user"></a>Loendi rollimäärangud kasutaja
Loendi rollimäärangud mõne kindla kasutaja ja rühmad kasutaja määratud ülesanded, saate kasutada.

    azure role assignment list --signInName <user email>

Saate vaadata ka rollimääranguid, muutes käsu päritud rühmad:

    azure role assignment list --expandPrincipalGroups --signInName <user email>

Järgmises näites on kujutatud rollimääranguid, mis on antud funktsiooni *sameert@aaddemo.com* kasutaja. See hõlmab otse kasutajale määratud rolle ja päritud rühmade rollid.

```
azure role assignment list --signInName sameert@aaddemo.com --json | jq '.[] | {"DisplayName":.properties.aADObject.DisplayName,"RoleDefinitionName":.properties.roleName,"Scope":.properties.scope}'

azure role assignment list --expandPrincipalGroups --signInName sameert@aaddemo.com --json | jq '.[] | {"DisplayName":.properties.aADObject.DisplayName,"RoleDefinitionName":.properties.roleName,"Scope":.properties.scope}'
```

![Azure'i RBAC käsurea - Azure'i rolli ülesande loendi kasutaja - kuvatõmmis](./media/role-based-access-control-manage-access-azure-cli/4-azure-role-assignment-list-2.png)

##  <a name="grant-access"></a>Juurdepääsu andmine
Olete kindlaks teinud, mida soovite määrata rolli juurdepääsu andmiseks saate kasutada.

    azure role assignment create

### <a name="assign-a-role-to-group-at-the-subscription-scope"></a>Rühma tellimuse ulatuse määramine
Tellimuse ulatuses rühma lisamine rolli määramiseks kasutage.

    azure role assignment create --objectId  <group object id> --roleName <name of role> --subscription <subscription> --scope <subscription/subscription id>

Järgmises näites määratakse *tellimuse* ulatus *Christine Koch meeskonnatöö* *lugeja* roll.


![RBAC Azure käsurea - Azure'i Rollimäärang loomiseks rühma-kuvatõmmis](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-1.png)

### <a name="assign-a-role-to-an-application-at-the-subscription-scope"></a>Taotluse tellimuse ulatuse määramine
Taotluse tellimuse ulatuse määramine, saate kasutada.

    azure role assignment create --objectId  <applications object id> --roleName <name of role> --subscription <subscription> --scope <subscription/subscription id>

Järgmine näide annab *kaasautori* roll *Azure AD* rakenduse valitud tellimust.

 ![RBAC Azure käsurea - Azure'i Rollimäärang rakenduse loomine](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-2.png)

### <a name="assign-a-role-to-a-user-at-the-resource-group-scope"></a>Ressursi rühma ulatuses kasutajale rolli määramine
Ressursi rühma ulatuses kasutajale määrata rolli, saate kasutada.

    azure role assignment create --signInName  <user email address> --roleName "<name of role>" --resourceGroup <resource group name>

Järgmine näide annab *Virtuaalse masina kaasautori* roll *samert@aaddemo.com* kasutaja *Pharma-müügi-ProjectForcast* ressursi rühma ulatuses.

![RBAC Azure käsurea - Azure'i Rollimäärang loomiseks kasutaja-kuvatõmmis](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-3.png)

### <a name="assign-a-role-to-a-group-at-the-resource-scope"></a>Rühmale ressursi ulatuses rolli määramine
Ressursi ulatuses rühma lisamine rolli määramiseks kasutage:

    azure role assignment create --objectId <group id> --role "<name of role>" --resource-name <resource group name> --resource-type <resource group type> --parent <resource group parent> --resource-group <resource group>

Järgmine näide annab *Azure AD* rühma *alamvõrgu* *Virtuaalse masina kaasautori* roll.

![RBAC Azure käsurea - Azure'i Rollimäärang loomiseks rühma-kuvatõmmis](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-assignment-create-4.png)

##  <a name="remove-access"></a>Juurdepääsu eemaldamine
Rollimäärang eemaldamiseks kasutada.

    azure role assignment delete --objectId <object id to from which to remove role> --roleName "<role name>"

Järgmises näites eemaldab kaudu *Virtuaalse masina kaasautor* Rollimäärang on *sammert@aaddemo.com* *Pharma-müügi-ProjectForcast* ressursirühm kasutajale.
Näiteks siis eemaldab rolli määramine rühmale tellimust.

![Azure'i RBAC käsurea - Azure'i rolli määramine Kustuta - kuvatõmmis](./media/role-based-access-control-manage-access-azure-cli/3-azure-role-assignment-delete.png)

## <a name="create-a-custom-role"></a>Luua kohandatud roll
Kasutage kohandatud rolli loomiseks

    azure role create --inputfile <file path>

Järgmises näites luuakse kohandatud rolli, nimetatakse *Virtuaalse masina tehtemärk*. Kohandatud roll annab kõik loetuks toimingud *Microsoft.Compute*, *Microsoft.Storage*ja *Microsoft.Network* ressursside teenusepakkuja ja annab alustada, taaskäivitage ja virtuaalmasinates jälgimine. Kahe tellimusi saab kasutada kohandatud roll. Selles näites kasutatakse sisendina JSON faili.

![JSON - kohandatud mõiste - kuvatõmmis](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-create-1.png)

![RBAC Azure käsurea - Azure'i rolli loomine – kuvatõmmis](./media/role-based-access-control-manage-access-azure-cli/2-azure-role-create-2.png)

## <a name="modify-a-custom-role"></a>Kohandatud roll muutmine

Kohandatud rolli muutmiseks kasutage funktsiooni `azure role show` käsk tuua rolli määratlus. Teiseks, tehke soovitud muudatused rolli määratluse faili. Kasutage `azure role set` salvestamine muudetud roll määratlus.

    azure role set --inputfile <file path>

Järgmises näites liidetakse *Microsoft.Insights/diagnosticSettings/* toimingu **toimingud**ja Azure tellimus **AssignableScopes** virtuaalse masina tehtemärk kohandatud roll.

![JSON - muutmine kohandatud määratlus - kuvatõmmis](./media/role-based-access-control-manage-access-azure-cli/3-azure-role-set-1.png)

![Azure'i RBAC käsurea - Azure'i roll set - kuvatõmmis](./media/role-based-access-control-manage-access-azure-cli/3-azure-role-set2.png)

## <a name="delete-a-custom-role"></a>Kohandatud rolli kustutamine

Kohandatud roll kustutamiseks esmalt kasutada funktsiooni `azure role show` käsk, et määrata rolli **ID-d** . Seejärel kasutage funktsiooni `azure role delete` käsk kustutada roll, määrates **ID-d**.

Järgmises näites eemaldab *Virtuaalse masina tehtemärk* kohandatud roll.

![Azure'i RBAC käsurea - Azure'i rolli Kustuta - kuvatõmmis](./media/role-based-access-control-manage-access-azure-cli/4-azure-role-delete.png)

## <a name="list-custom-roles"></a>Kohandatud loendi rollid

Loendis Saadaolevad ülesande juures, mille ulatus rollid, kasutage funktsiooni `azure role list` käsk.

Järgmises näites on loetletud kõik rolli, mis ülesande valitud tellimuse jaoks on saadaval.

```
azure role list --json | jq '.[] | {"name":.properties.roleName, type:.properties.type}'
```

![Azure'i RBAC käsurea - Azure'i rolli loendi - kuvatõmmis](./media/role-based-access-control-manage-access-azure-cli/5-azure-role-list1.png)

Järgmises näites *Virtuaalse masina tehtemärk* kohandatud roll pole saadaval *Production4* tellimuse, kuna see tellimus pole **AssignableScopes** roll.

```
azure role list --json | jq '.[] | if .properties.type == "CustomRole" then .properties.roleName else empty end'
```

![Azure'i RBAC käsurea - Azure'i roll loendi jaoks kohandatud rollid - kuvatõmmis](./media/role-based-access-control-manage-access-azure-cli/5-azure-role-list2.png)





## <a name="rbac-topics"></a>RBAC Teemad
[AZURE.INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]
