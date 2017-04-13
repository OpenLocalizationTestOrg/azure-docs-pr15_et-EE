<properties
    pageTitle="Rollipõhine juurdepääsu reguleerimine REST API-ga haldamine"
    description="Rollipõhine juurdepääsu reguleerimine REST API-ga haldamine"
    services="active-directory"
    documentationCenter="na"
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="multiple"
    ms.tgt_pltfrm="rest-api"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="managing-role-based-access-control-with-the-rest-api"></a>Rollipõhine juurdepääsu reguleerimine REST API-ga haldamine

> [AZURE.SELECTOR]
- [PowerShelli](role-based-access-control-manage-access-powershell.md)
- [Azure'i CLI](role-based-access-control-manage-access-azure-cli.md)
- [REST API-GA](role-based-access-control-manage-access-rest.md)

Rollipõhine juurdepääsu juhtimine (RBAC) Azure portaali ja Azure ressursihaldur API abil saate hallata juurdepääsu oma tellimust ja ressursside Kohandatud tase. Selle funktsiooniga saate anda juurdepääsu Active Directory kasutajad, rühmad või teenuse põhisumma teatud rollide määrate neile teatud ulatuses.

## <a name="list-all-role-assignments"></a>Loetle kõik rollimäärangud

Loetleb kõik rollimäärangud määratud ulatuse ja subscopes.

Loendi rollimääranguid, et teil peab olema juurdepääs `Microsoft.Authorization/roleAssignments/read` ulatuse toimingut. Sisseehitatud rollid on antud seda toimingut. Rollimääranguid ja haldamise juurdepääsu Azure ressursside kohta leiate lisateavet teemast [Azure Role-Based juurdepääsu reguleerimine](role-based-access-control-configure.md).

### <a name="request"></a>Taotlemine

**Saada** meetodit kasutada järgmist URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments?api-version={api-version}&$filter={filter}

URI, jooksul teha järgmised asendused kohandada teie taotlus:

1. Ulatus, mille soovite loendisse rollimääranguid *{ulatus}* asendada. Järgmised näited kujutavad erinevaid tasemeid ulatuse määramiseks tehke järgmist:

  - Tellimus: /subscriptions/ {tellimuse id}  
  - Ressursirühm: /subscriptions/ {tellimuse id} / resourceGroups/myresourcegroup1  
  - Ressurss: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. Asendage 2015-07-01 *{api-versioon}* .

3. *{Filter}* asendamiseks tingimus, mida soovite rakendada rolli ülesande loendi filtreerimiseks tehke järgmist.

  - Loendi rollimäärangud ainult määratud ulatuse, välja arvatud rollimääranguid veebisaidil subscopes:`atScope()`    
  - Loendi rollimäärangud mõne kindla kasutaja, rühma või rakendus:`principalId%20eq%20'{objectId of user, group, or service principal}'`  
  - Mõne kindla kasutaja, sh päritud rühmade rollimäärangud loendis |`assignedTo('{objectId of user}')`

### <a name="response"></a>Vastus

Olekukoodi: 200

```
{
  "value": [
    {
      "properties": {
        "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7",
        "principalId": "2f9d4375-cbf1-48e8-83c9-2a0be4cb33fb",
        "scope": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e",
        "createdOn": "2015-10-08T07:28:24.3905077Z",
        "updatedOn": "2015-10-08T07:28:24.3905077Z",
        "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
        "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
      },
      "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleAssignments/baa6e199-ad19-4667-b768-623fde31aedd",
      "type": "Microsoft.Authorization/roleAssignments",
      "name": "baa6e199-ad19-4667-b768-623fde31aedd"
    }
  ],
  "nextLink": null
}

```

## <a name="get-information-about-a-role-assignment"></a>Rollimäärang kohta teabe saamine

Saab teavet määratud roll ülesande identifikaator ühe Rollimäärang.

Rollimäärang kohta teabe saamiseks, peab olema juurdepääs `Microsoft.Authorization/roleAssignments/read` toiming. Sisseehitatud rollid on antud seda toimingut. Rollimääranguid ja haldamise juurdepääsu Azure ressursside kohta leiate lisateavet teemast [Azure Role-Based juurdepääsu reguleerimine](role-based-access-control-configure.md).

### <a name="request"></a>Taotlemine

**Saada** meetodit kasutada järgmist URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{role-assignment-id}?api-version={api-version}

URI, jooksul teha järgmised asendused kohandada teie taotlus:

1. Ulatus, mille soovite loendisse rollimääranguid *{ulatus}* asendada. Järgmised näited kujutavad erinevaid tasemeid ulatuse määramiseks tehke järgmist:

  - Tellimus: /subscriptions/ {tellimuse id}  
  - Ressursirühm: /subscriptions/ {tellimuse id} / resourceGroups/myresourcegroup1  
  - Ressurss: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. *{Rolli ülesande id}* asendada selle GUID identifikaator rolli määramine.

3. Asendage 2015-07-01 *{api-versioon}* .

### <a name="response"></a>Vastus

Olekukoodi: 200

```
{
  "properties": {
    "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/b24988ac-6180-42a0-ab88-20f7382dd24c",
    "principalId": "672f1afa-526a-4ef6-819c-975c7cd79022",
    "scope": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e",
    "createdOn": "2015-10-05T08:36:26.4014813Z",
    "updatedOn": "2015-10-05T08:36:26.4014813Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleAssignments/196965ae-6088-4121-a92a-f1e33fdcc73e",
  "type": "Microsoft.Authorization/roleAssignments",
  "name": "196965ae-6088-4121-a92a-f1e33fdcc73e"
}

```

## <a name="create-a-role-assignment"></a>Rollimäärang loomine

Loo Rollimäärang määratud ulatuse jaoks määratud põhisumma andmise määratud roll.

Rollimäärang loomiseks peab olema juurdepääs `Microsoft.Authorization/roleAssignments/write` toiming. Sisseehitatud rolle, ainult *omanik* ja *Kasutajale Accessi administraator* on antud seda toimingut. Rollimääranguid ja haldamise juurdepääsu Azure ressursside kohta leiate lisateavet teemast [Azure Role-Based juurdepääsu reguleerimine](role-based-access-control-configure.md).

### <a name="request"></a>Taotlemine

**Sellele** meetodit kasutada järgmist URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{role-assignment-id}?api-version={api-version}

URI, jooksul teha järgmised asendused kohandada teie taotlus:

1. Ulatus, mille soovite luua rollimääranguid *{ulatus}* asendada. Kui loote Rollimäärang ema ulatus, pärivad kõik lapse otsinguulatuste sama rolli määramine. Järgmised näited kujutavad erinevaid tasemeid ulatuse määramiseks tehke järgmist:

  - Tellimus: /subscriptions/ {tellimuse id}  
  - Ressursirühm: /subscriptions/ {tellimuse id} / resourceGroups/myresourcegroup1   
  - Ressurss: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. *{Rolli ülesande id}* asendada uute GUID, mis muutub uue Rollimäärang GUID ainuidentifikaator.

3. Asendage 2015-07-01 *{api-versioon}* .

Koosolekukutse sisusse, esitage väärtused järgmises vormingus:

```
{
  "properties": {
    "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
    "principalId": "5ac84765-1c8c-4994-94b2-629461bd191b"
  }
}

```

| Elemendi nime     | Nõutav | Tüüp   | Kirjeldus |
|------------------|----------|--------|-------------|
| roleDefinitionId | Jah      | String | Identifikaator roll. Identifikaator vorming on:`{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id-guid}` |
| principalId      | Jah      | String | ObjectId väärtuse, mis on määratud roll Azure AD põhisumma (kasutaja, rühma või teenuse põhilise). |

### <a name="response"></a>Vastus

Olekukoodi: 201

```
{
  "properties": {
    "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
    "principalId": "5ac84765-1c8c-4994-94b2-629461bd191b",
    "scope": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND",
    "createdOn": "2015-12-16T00:27:19.6447515Z",
    "updatedOn": "2015-12-16T00:27:19.6447515Z",
    "createdBy": null,
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND/providers/Microsoft.Authorization/roleAssignments/2e9e86c8-0e91-4958-b21f-20f51f27bab2",
  "type": "Microsoft.Authorization/roleAssignments",
  "name": "2e9e86c8-0e91-4958-b21f-20f51f27bab2"
}

```

## <a name="delete-a-role-assignment"></a>Rollimäärang kustutamine

Kustutage Rollimäärang määratud ulatuses.

Rollimäärang kustutamiseks peab olema juurdepääs on `Microsoft.Authorization/roleAssignments/delete` toiming. Sisseehitatud rolle, ainult *omanik* ja *Kasutajale Accessi administraator* on antud seda toimingut. Rollimääranguid ja haldamise juurdepääsu Azure ressursside kohta leiate lisateavet teemast [Azure Role-Based juurdepääsu reguleerimine](role-based-access-control-configure.md).

### <a name="request"></a>Taotlemine

Kasutage järgmist URI **kustutamine** meetodit:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{role-assignment-id}?api-version={api-version}

URI, jooksul teha järgmised asendused kohandada teie taotlus:

1. Ulatus, mille soovite luua rollimääranguid *{ulatus}* asendada. Järgmised näited kujutavad erinevaid tasemeid ulatuse määramiseks tehke järgmist:

  - Tellimus: /subscriptions/ {tellimuse id}  
  - Ressursirühm: /subscriptions/ {tellimuse id} / resourceGroups/myresourcegroup1  
  - Ressurss: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. Asendage rolli ülesande id GUID *{rolli ülesande id}* .

3. Asendage 2015-07-01 *{api-versioon}* .

### <a name="response"></a>Vastus

Olekukoodi: 200

```
{
  "properties": {
    "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
    "principalId": "5ac84765-1c8c-4994-94b2-629461bd191b",
    "scope": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND",
    "createdOn": "2015-12-17T23:21:40.8921564Z",
    "updatedOn": "2015-12-17T23:21:40.8921564Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND/providers/Microsoft.Authorization/roleAssignments/5eec22ee-ea5c-431e-8f41-82c560706fd2",
  "type": "Microsoft.Authorization/roleAssignments",
  "name": "5eec22ee-ea5c-431e-8f41-82c560706fd2"
}

```

## <a name="list-all-roles"></a>Kõikide rollide loend

Loetleb on saadaval veebisaidil määratud ulatuse rollid.

Rollide loend, peab olema juurdepääs `Microsoft.Authorization/roleDefinitions/read` ulatuse toimingut. Sisseehitatud rollid on antud seda toimingut. Rollimääranguid ja haldamise juurdepääsu Azure ressursside kohta leiate lisateavet teemast [Azure Role-Based juurdepääsu reguleerimine](role-based-access-control-configure.md).

### <a name="request"></a>Taotlemine

**Saada** meetodit kasutada järgmist URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions?api-version={api-version}&$filter={filter}

URI, jooksul teha järgmised asendused kohandada teie taotlus:

1. Asendage *{ulatus}* ulatus, mille soovite loendisse rollid. Järgmised näited kujutavad erinevaid tasemeid ulatuse määramiseks tehke järgmist:

  - Tellimus: /subscriptions/ {tellimuse id}  
  - Ressursirühm: /subscriptions/ {tellimuse id} / resourceGroups/myresourcegroup1  
  - Ressursi /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. Asendage 2015-07-01 *{api-versioon}* .

3. Tingimus, mida soovite rakendada rollide loendit filtreerida *{filter}* asendamiseks tehke järgmist.

  - Loendi rollid, selle lapse otsinguulatuste määratud ulatuses ja mis tahes ülesande jaoks saadaval:`atScopeAndBelow()`
  - Otsingu abil täpse kuvatav nimi rolli: `roleName%20eq%20'{role-display-name}'`. Vormi URL-ina kodeeritav rolli täpse kuvatav nimi. Näiteks`$filter=roleName%20eq%20'Virtual%20Machine%20Contributor'` |

### <a name="response"></a>Vastus

Olekukoodi: 200

```
{
  "value": [
    {
      "properties": {
        "roleName": "Virtual Machine Contributor",
        "type": "BuiltInRole",
        "description": "Lets you manage virtual machines, but not access to them, and not the virtual network or storage account they\u2019re connected to.",
        "assignableScopes": [
          "/"
        ],
        "permissions": [
          {
            "actions": [
              "Microsoft.Authorization/*/read",
              "Microsoft.Compute/availabilitySets/*",
              "Microsoft.Compute/locations/*",
              "Microsoft.Compute/virtualMachines/*",
              "Microsoft.Compute/virtualMachineScaleSets/*",
              "Microsoft.Insights/alertRules/*",
              "Microsoft.Network/applicationGateways/backendAddressPools/join/action",
              "Microsoft.Network/loadBalancers/backendAddressPools/join/action",
              "Microsoft.Network/loadBalancers/inboundNatPools/join/action",
              "Microsoft.Network/loadBalancers/inboundNatRules/join/action",
              "Microsoft.Network/loadBalancers/read",
              "Microsoft.Network/locations/*",
              "Microsoft.Network/networkInterfaces/*",
              "Microsoft.Network/networkSecurityGroups/join/action",
              "Microsoft.Network/networkSecurityGroups/read",
              "Microsoft.Network/publicIPAddresses/join/action",
              "Microsoft.Network/publicIPAddresses/read",
              "Microsoft.Network/virtualNetworks/read",
              "Microsoft.Network/virtualNetworks/subnets/join/action",
              "Microsoft.Resources/deployments/*",
              "Microsoft.Resources/subscriptions/resourceGroups/read",
              "Microsoft.Storage/storageAccounts/listKeys/action",
              "Microsoft.Storage/storageAccounts/read",
              "Microsoft.Support/*"
            ],
            "notActions": []
          }
        ],
        "createdOn": "2015-06-02T00:18:27.3542698Z",
        "updatedOn": "2015-12-08T03:16:55.6170255Z",
        "createdBy": null,
        "updatedBy": null
      },
      "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
      "type": "Microsoft.Authorization/roleDefinitions",
      "name": "9980e02c-c2be-4d73-94e8-173b1dc7cf3c"
    }
  ],
  "nextLink": null
}

```

## <a name="get-information-about-a-role"></a>Rolli kohta teabe saamine

Saab ühe rollile määratud roll määratlus identifikaator teavet. Kindla rolliga, kasutades oma kuvatav nimi kohta teabe saamiseks lugege teemat [kõikide rollide loend](role-based-access-control-manage-access-rest.md#list-all-roles).

Rolli kohta teabe saamiseks, peab olema juurdepääs `Microsoft.Authorization/roleDefinitions/read` toiming. Sisseehitatud rollid on antud seda toimingut. Rollimääranguid ja haldamise juurdepääsu Azure ressursside kohta leiate lisateavet teemast [Azure Role-Based juurdepääsu reguleerimine](role-based-access-control-configure.md).

### <a name="request"></a>Taotlemine

**Saada** meetodit kasutada järgmist URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

URI, jooksul teha järgmised asendused kohandada teie taotlus:

1. Ulatus, mille soovite loendisse rollimääranguid *{ulatus}* asendada. Järgmised näited kujutavad erinevaid tasemeid ulatuse määramiseks tehke järgmist:

  - Tellimus: /subscriptions/ {tellimuse id}  
  - Ressursirühm: /subscriptions/ {tellimuse id} / resourceGroups/myresourcegroup1  
  - Ressurss: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. Asendage GUID identifikaator rolli määratluse *{rolli määratlus id}* .

3. Asendage 2015-07-01 *{api-versioon}* .

### <a name="response"></a>Vastus

Olekukoodi: 200

```
{
  "value": [
    {
      "properties": {
        "roleName": "Virtual Machine Contributor",
        "type": "BuiltInRole",
        "description": "Lets you manage virtual machines, but not access to them, and not the virtual network or storage account they\u2019re connected to.",
        "assignableScopes": [
          "/"
        ],
        "permissions": [
          {
            "actions": [
              "Microsoft.Authorization/*/read",
              "Microsoft.Compute/availabilitySets/*",
              "Microsoft.Compute/locations/*",
              "Microsoft.Compute/virtualMachines/*",
              "Microsoft.Compute/virtualMachineScaleSets/*",
              "Microsoft.Insights/alertRules/*",
              "Microsoft.Network/applicationGateways/backendAddressPools/join/action",
              "Microsoft.Network/loadBalancers/backendAddressPools/join/action",
              "Microsoft.Network/loadBalancers/inboundNatPools/join/action",
              "Microsoft.Network/loadBalancers/inboundNatRules/join/action",
              "Microsoft.Network/loadBalancers/read",
              "Microsoft.Network/locations/*",
              "Microsoft.Network/networkInterfaces/*",
              "Microsoft.Network/networkSecurityGroups/join/action",
              "Microsoft.Network/networkSecurityGroups/read",
              "Microsoft.Network/publicIPAddresses/join/action",
              "Microsoft.Network/publicIPAddresses/read",
              "Microsoft.Network/virtualNetworks/read",
              "Microsoft.Network/virtualNetworks/subnets/join/action",
              "Microsoft.Resources/deployments/*",
              "Microsoft.Resources/subscriptions/resourceGroups/read",
              "Microsoft.Storage/storageAccounts/listKeys/action",
              "Microsoft.Storage/storageAccounts/read",
              "Microsoft.Support/*"
            ],
            "notActions": []
          }
        ],
        "createdOn": "2015-06-02T00:18:27.3542698Z",
        "updatedOn": "2015-12-08T03:16:55.6170255Z",
        "createdBy": null,
        "updatedBy": null
      },
      "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
      "type": "Microsoft.Authorization/roleDefinitions",
      "name": "9980e02c-c2be-4d73-94e8-173b1dc7cf3c"
    }
  ],
  "nextLink": null
}

```

## <a name="create-a-custom-role"></a>Luua kohandatud roll
Saate luua kohandatud roll.

Luua kohandatud rolli, peab olema juurdepääs `Microsoft.Authorization/roleDefinitions/write` toiming kõik selle `AssignableScopes`. Sisseehitatud rolle, ainult *omanik* ja *Kasutajale Accessi administraator* on antud seda toimingut. Rollimääranguid ja haldamise juurdepääsu Azure ressursside kohta leiate lisateavet teemast [Azure Role-Based juurdepääsu reguleerimine](role-based-access-control-configure.md).

### <a name="request"></a>Taotlemine

**Sellele** meetodit kasutada järgmist URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

URI, jooksul teha järgmised asendused kohandada teie taotlus:

1. Asendage kohandatud roll esimese *AssignableScope* *{ulatus}* . Järgmised näited kujutavad erinevaid tasemeid ulatuse määramiseks tehke järgmist.

  - Tellimus: /subscriptions/ {tellimuse id}  
  - Ressursirühm: /subscriptions/ {tellimuse id} / resourceGroups/myresourcegroup1  
  - Ressurss: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. *{Rolli määratlus id}* asendada uute GUID, mis muutub GUID identifikaator uue kohandatud roll.

3. Asendage 2015-07-01 *{api-versioon}* .

Koosolekukutse sisusse, esitage väärtused järgmises vormingus:

```
{
  "name": "7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7",
  "properties": {
    "roleName": "Virtual Machine Operator",
    "description": "Lets you monitor virtual machines and restart them.",
    "type": "CustomRole",
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ]
  }
}

```

| Elemendi nime | Nõutav | Tüüp | Kirjeldus |
|--------------|----------|------|-------------|
| Nimi         | Jah | String   | Kohandatud roll GUID ID.    |
| properties.roleName               | Jah | String   | Kuvatav nimi kohandatud roll. Märkide maksimaalne maht 128.                        |
| Properties.Description            | Ei  | String   | Kohandatud rolli kirjeldust. Suurim lubatud maht 1024 märki.                                               |
| Properties.Type                   | Jah | String   | Seatud "CustomRole."                                         |
| Properties.permissions.Actions    | Jah | String] | Massiivi toimingu stringide toimingud, mis on antud kohandatud rolli määramine.             |
| properties.permissions.notActions | Ei  | String] | Massiivi toimingu stringide toimingute välistamine toimingud, mis on antud kohandatud rolli määramine. |
| properties.assignableScopes       | Jah | String] | Massiivi otsinguulatuste, kus saate kasutada kohandatud roll.   |

### <a name="response"></a>Vastus

Olekukoodi: 201

```
{
  "properties": {
    "roleName": "Virtual Machine Operator",
    "type": "CustomRole",
    "description": "Lets you monitor virtual machines and restart them.",
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ],
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "createdOn": "2015-12-18T00:10:51.4662695Z",
    "updatedOn": "2015-12-18T00:10:51.4662695Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7",
  "type": "Microsoft.Authorization/roleDefinitions",
  "name": "7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7"
}

```

## <a name="update-a-custom-role"></a>Kohandatud rolli värskendamine

Saate muuta kohandatud roll.

Kohandatud rolli muutmiseks peab teil olema juurdepääs `Microsoft.Authorization/roleDefinitions/write` toiming kõik selle `AssignableScopes`. Sisseehitatud rolle, ainult *omanik* ja *Kasutajale Accessi administraator* on antud seda toimingut. Rollimääranguid ja haldamise juurdepääsu Azure ressursside kohta leiate lisateavet teemast [Azure Role-Based juurdepääsu reguleerimine](role-based-access-control-configure.md).

### <a name="request"></a>Taotlemine

**Sellele** meetodit kasutada järgmist URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

URI, jooksul teha järgmised asendused kohandada teie taotlus:

1. Asendage kohandatud roll esimese *AssignableScope* *{ulatus}* . Järgmised näited kujutavad erinevaid tasemeid ulatuse määramiseks tehke järgmist:

  - Tellimus: /subscriptions/ {tellimuse id}  
  - Ressursirühm: /subscriptions/ {tellimuse id} / resourceGroups/myresourcegroup1  
  - Ressurss: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. Asendage kohandatud rolli GUID identifikaator *{rolli määratlus id}* .

3. Asendage 2015-07-01 *{api-versioon}* .

Koosolekukutse sisusse, esitage väärtused järgmises vormingus:

```
{
  "name": "7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7",
  "properties": {
    "roleName": "Virtual Machine Operator",
    "description": "Lets you monitor virtual machines and restart them.",
    "type": "CustomRole",
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ]
  }
}

```

| Elemendi nime | Nõutav | Tüüp | Kirjeldus |
|--------------|----------|------|-------------|
| Nimi         | Jah      | String | Kohandatud roll GUID ID. |
| properties.roleName | Jah | String | Kuvatav nimi kohandatud värskendatud roll. |
| Properties.Description | Ei | String | Värskendatud kohandatud rolli kirjeldust. |
| Properties.Type | Jah | String | Seatud "CustomRole." |
| Properties.permissions.Actions | Jah | String] | Massiivi toimingu stringide toimingud, millele annab värskendatud kohandatud rolli määramine. |
| properties.permissions.notActions | Ei | String] | Massiivi toimingu stringide toimingute välistamine toiminguid, mis määrab värskendatud kohandatud rolli määramine. |
| properties.assignableScopes | Jah | String] | Massiivi otsinguulatuste värskendatud kohandatud roll saab kasutada. |

### <a name="response"></a>Vastus

Olekukoodi: 201

```
{
  "properties": {
    "roleName": "Virtual Machine Operator",
    "type": "CustomRole",
    "description": "Lets you monitor virtual machines and restart them.",
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ],
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "createdOn": "2015-12-18T00:10:51.4662695Z",
    "updatedOn": "2015-12-18T00:10:51.4662695Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7",
  "type": "Microsoft.Authorization/roleDefinitions",
  "name": "7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7"
}

```

## <a name="delete-a-custom-role"></a>Kohandatud rolli kustutamine

Kustutage kohandatud roll.

Kohandatud rolli kustutamiseks peab olema juurdepääs `Microsoft.Authorization/roleDefinitions/delete` toiming kõik selle `AssignableScopes`. Sisseehitatud rolle, ainult *omanik* ja *Kasutajale Accessi administraator* on antud seda toimingut. Rollimääranguid ja haldamise juurdepääsu Azure ressursside kohta leiate lisateavet teemast [Azure Role-Based juurdepääsu reguleerimine](role-based-access-control-configure.md).

### <a name="request"></a>Taotlemine

Kasutage järgmist URI **kustutamine** meetodit:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

URI, jooksul teha järgmised asendused kohandada teie taotlus:

1. Asendage *{ulatus}* ulatus, mille soovite kustutada rolli määratlus. Järgmised näited kujutavad erinevaid tasemeid ulatuse määramiseks tehke järgmist:

  - Tellimus: /subscriptions/ {tellimuse id}  
  - Ressursirühm: /subscriptions/ {tellimuse id} / resourceGroups/myresourcegroup1  
  - Ressurss: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. *{Rolli määratlus id}* asendada kohandatud rolli id GUID rolli määratlus.

3. Asendage 2015-07-01 *{api-versioon}* .

### <a name="response"></a>Vastus

Olekukoodi: 200

```
{
  "properties": {
    "roleName": "Virtual Machine Operator",
    "type": "CustomRole",
    "description": "Lets you monitor virtual machines and restart them.",
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ],
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "createdOn": "2015-12-16T00:07:02.9236555Z",
    "updatedOn": "2015-12-16T00:07:02.9236555Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/0bd62a70-e1b8-4e0b-a7c2-75cab365c95b",
  "type": "Microsoft.Authorization/roleDefinitions",
  "name": "0bd62a70-e1b8-4e0b-a7c2-75cab365c95b"
}

```


[AZURE.INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]
