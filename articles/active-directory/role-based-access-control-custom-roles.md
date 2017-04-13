<properties
    pageTitle="Kohandatud rollid Azure RBAC | Microsoft Azure'i"
    description="Saate teada, kuidas Azure'i tellimuse kohandatud rollid Azure Role-Based juurdepääsu kontroll Täpsem identiteedi haldamise määratlemine."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="kgremban"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="07/25/2016"
    ms.author="kgremban"/>


# <a name="custom-roles-in-azure-rbac"></a>Azure'i RBAC kohandatud rollid


Luua kohandatud rolli Azure Role-Based juurdepääsu juhtimine (RBAC), kui ükski sisseehitatud rolle ei vasta teie vajadustele juurdepääsetav. [Azure'i PowerShelli](role-based-access-control-manage-access-powershell.md), [Azure'i käsurea liides](role-based-access-control-manage-access-azure-cli.md) (CLI) ja [REST API](role-based-access-control-manage-access-rest.md)abil saab luua kohandatud rollid. Nagu sisseehitatud rollid, saate määrata kohandatud rollid kasutajate, rühmade ja rakenduste tellimus, ressursirühm ja ressursside otsinguulatuste. Kohandatud rollid talletatakse Azure AD rentniku ja selle kausta Azure AD kasutamiseks on subsciption kõigi tellimuste saab jagada.

Järgmises näites kohandatud rolli jälgimiseks ja taaskäivitada virtuaalmasinates:

```
{
  "Name": "Virtual Machine Operator",
  "Id": "cadb4a5a-4e7a-47be-84db-05cad13b6769",
  "IsCustom": true,
  "Description": "Can monitor and restart virtual machines.",
  "Actions": [
    "Microsoft.Storage/*/read",
    "Microsoft.Network/*/read",
    "Microsoft.Compute/*/read",
    "Microsoft.Compute/virtualMachines/start/action",
    "Microsoft.Compute/virtualMachines/restart/action",
    "Microsoft.Authorization/*/read",
    "Microsoft.Resources/subscriptions/resourceGroups/read",
    "Microsoft.Insights/alertRules/*",
    "Microsoft.Insights/diagnosticSettings/*",
    "Microsoft.Support/*"
  ],
  "NotActions": [

  ],
  "AssignableScopes": [
    "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e",
    "/subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624",
    "/subscriptions/34370e90-ac4a-4bf9-821f-85eeedeae1a2"
  ]
}
```
## <a name="actions"></a>Toimingud
Kohandatud rolli atribuudi **toimingud** määrab Azure toimingud, millele annab roll. See on toiming stringide turvatavate toimingute Azure ressursi teenusepakkuja tuvastavat kogum. Toiming stringid, mis sisaldavad metamärkide (\*) anda juurdepääsu kõik toimingud, mis vastavad toiming string. Näiteks:

-   `*/read`annab juurdepääsu lugeda toimingute ressursi igat tüüpi kõik Azure ressursi pakkujad.
-   `Microsoft.Network/*/read`toetused lugeda toimingute kõigi ressursside tüüpide Azure'i Microsoft.Network ressursi pakkuja juurde.
-   `Microsoft.Compute/virtualMachines/*`annab juurdepääsu kõik toimingud virtuaalmasinates ja selle laps ressursi tüübid.
-   `Microsoft.Web/sites/restart/Action`taaskäivitage veebisaitide juurdepääsu toetused.

Kasutage `Get-AzureRmProviderOperation` (PowerShellis) või `azure provider operations show` (Azure'i CLI) loendi toimingute Azure ressursi teenusepakkuja. Samuti võite need käsud, veenduge, et toiming on string on lubatud ja metamärkide toiming stringide laiendamiseks.

```
Get-AzureRMProviderOperation Microsoft.Compute/virtualMachines/*/action | FT Operation, OperationName

Get-AzureRMProviderOperation Microsoft.Network/*
```

![PowerShelli kuvatõmmis - Get-AzureRMProviderOperation Microsoft.Compute/virtualMachines/*/action | FT tehe OperationName](./media/role-based-access-control-configure/1-get-azurermprovideroperation-1.png)

```
azure provider operations show "Microsoft.Compute/virtualMachines/*/action" --js on | jq '.[] | .operation'

azure provider operations show "Microsoft.Network/*"
```

![Azure'i CLI kuvatõmmis - Azure'i pakkuja toimingute Kuva "Microsoft.Compute/virtualMachines/\*või meetme kavandatavatest" ](./media/role-based-access-control-configure/1-azure-provider-operations-show.png)

## <a name="notactions"></a>NotActions
Kasutage **NotActions** atribuut, kui hõlpsam määratletakse kogumi toimingute kohta, mida soovite lubada arvatud piiratud toimingud. Kohandatud rolli antud juurdepääs on arvutatud lahutamise **toimingud** toimingute **NotActions** toimingud.

> [AZURE.NOTE] Kui kasutaja on määratud roll, mis ei hõlma toimingu **NotActions**ja on määratud teine osa, mis annab sama toimingut, lubatakse kasutaja selle toimingu sooritamiseks. **NotActions** pole Keela reegel – see on lihtsalt mugavam lubatud toimingute loomine, kui on vaja välistada teatud toiminguid.

## <a name="assignablescopes"></a>AssignableScopes
Kohandatud rolli atribuudi **AssignableScopes** määrab otsinguulatuste (tellimuste, ressursside rühmade või ressursse), kus kohandatud roll on ülesande jaoks saadaval. Kohandatud roll tellimuste või ressursside rühmad, mis nõuavad selle ülesande kättesaadavaks teha ja pole tarbetu kasutusvõimalused ülejäänud tellimuste või ressursside rühmad.

Lubatud määratav otsinguulatuste näited.

-   "/ tellimuste/c276fc76-9cd4-44c9-99a7-4fd71546436e", "/ tellimuste/e91d47c4-76f3-4271-a796-21b4ecfe3624" - muudab roll kahe tellimuste ülesande jaoks saadaval.
-   "/ tellimuste/c276fc76-9cd4-44c9-99a7-4fd71546436e" - muudab roll ühe tellimuse ülesande jaoks saadaval.
-  "/ tellimuste/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/võrgu" – teeb roll ülesande jaoks saadaval ainult jaotises võrku ressursi.

> [AZURE.NOTE] Kasutage vähemalt üks tellimus, ressursirühm või ressursi ID-ga.

## <a name="custom-roles-access-control"></a>Kohandatud rollid juurdepääsu juhtimine
Kohandatud rolli atribuudi **AssignableScopes** ka juhtelemendid, kes saab vaadata, muutmine ja kustutamine roll.

- Kes saavad luua kohandatud rolli?
    Omanikud (ja kasutajale juurdepääs administraatorid) tellimuste, saate ressursi rühmad ja ressursse luua kohandatud rollid kasutamiseks need otsinguulatuste.
    Kasutaja loomise roll peab olema `Microsoft.Authorization/roleDefinition/write` rolli **AssignableScopes** toimimist.

- Kes saab muuta kohandatud rolli?
    Omanikud (ja kasutajale juurdepääs administraatorid) tellimustest, ressursside rühmad ja ressursid saate muuta nende otsinguulatuste kohandatud rollid. Kasutajad peavad olema selle `Microsoft.Authorization/roleDefinition/write` kohandatud roll **AssignableScopes** toimimist.

- Kes saavad vaadata kohandatud rollid?
    Sisseehitatud kõikide rollide Azure'i RBAC luba vaatamise rollid, mis on saadaval. Kasutajad, kes saavad teha on `Microsoft.Authorization/roleDefinition/read` ulatus toimingut saate vaadata selle ulatuses ülesande jaoks saadaolevaid RBAC-rollid.

## <a name="see-also"></a>Vt ka
- [Rollipõhine juurdepääsu juhtimine](role-based-access-control-configure.md): alustamine RBAC Azure'i portaalis.
- Saate teada, kuidas juurdepääsu ja hallata:
    - [PowerShelli](role-based-access-control-manage-access-powershell.md)
    - [Azure'i CLI](role-based-access-control-manage-access-azure-cli.md)
    - [REST API-GA](role-based-access-control-manage-access-rest.md)
- [Sisseehitatud rolle](role-based-access-built-in-roles.md): rollid, mis tulevad standard RBAC üksikasjade toomine.
