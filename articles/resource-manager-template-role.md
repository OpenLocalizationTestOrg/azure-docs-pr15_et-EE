<properties
   pageTitle="Ressursihaldur malli rollimääranguid | Microsoft Azure'i"
   description="Näitab ressursihaldur skeemi Rollimäärang kaudu malli kasutamise kohta."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/03/2016"
   ms.author="tomfitz"/>

# <a name="role-assignments-template-schema"></a>Roll ülesanded malli skeem

Määrab kasutaja, rühma või teenuse põhilise rollile määratud ulatuses.

## <a name="resource-format"></a>Ressursi vorming

Rollimäärang loomiseks lisage järgmised skeemi ressursid jaotisele malli.
    
    {
        "type": string,
        "apiVersion": "2015-07-01",
        "name": string,
        "dependsOn": [ array values ],
        "properties":
        {
            "roleDefinitionId": string,
            "principalId": string,
            "scope": string
        }
    }

## <a name="values"></a>Väärtused

Järgmises tabelis on kirjeldatud peate määrama skeemi väärtused.

| Nimi | Nõutav | Kirjeldus |
| ---- | -------- | ----------- |
| tüüp | Jah    | Ressursi tüüp loomiseks.<br /><br /> Ressursirühma:<br />**Microsoft.Authorization/roleAssignments**<br /><br />Ressurss:<br />**{pakkuja nimeruum} / {ressursi-tüüpi} / pakkujate/roleAssignments** |
| apiVersion |Jah | Ressursi loomiseks kasutatav API versioon.<br /><br /> Kasutage **2015-07-01**. | 
| Nimi | Jah | Uue Rollimäärang globaalselt kordumatu ID. |
| ei leia dependsOn | Ei | Komaga eraldatud massiivi ressursi nimed või ressursside ainulaadsed ID-d.<br /><br />Kogum ressursse, mis sõltub selle rolli määramine. Kui mis hõlmavaid ressursi ja ressursside juurutatakse sama malli rolli määramine, kaasake – see element tagada ressursi juurutatakse esmalt selle ressursi nimi. |
| atribuudid | Jah | Tuvastab rolli määratlus, põhisumma ja ulatus objekti atribuudid. |

### <a name="properties-object"></a>objekti atribuudid

| Nimi | Nõutav | Kirjeldus |
| ---- | -------- | ----------- |
| roleDefinitionId | Jah |  Olemasoleva rolli määratlus rolli määramine kasutada ID.<br /><br /> Kasutage järgmises vormingus:<br /> **/ subscriptions/{subscription-id}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}** |
| principalId | Jah | Globaalne ainuidentifikaator olemasoleva peamise. See väärtus sees kataloogi ID kaartide ja saate osutage kasutaja, teenuse põhilise või turberühma. |
| ulatus | Ei | Ulatus, mille kehtib selle rolli määramine.<br /><br />Ressursi rühmade jaoks kasutada:<br />**/resourceGroups/ /subscriptions/ {tellimuse id} {ressursi rühma nimi}**  <br /><br />Ressursid, kasutage.<br />**/ subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/{provider-namespace}/{resource-type}/{resource-name}** |  |


## <a name="how-to-use-the-role-assignment-resource"></a>Kuidas kasutada rolli ülesande ressurss

Kui peate lisama kasutaja, rühma või teenuse põhisumma rollile juurutamisel, lisage Rollimäärang malli. Rollimäärangud päritakse ulatus, kõrge, kui olete juba lisanud subjekt rollile tellimuse tasemel, pole vaja määrata selle ressursi või ressursirühma.

On palju tuleb sisestada rollimääranguid töötamisel väärtused. Saate tuua PowerShelli või Azure CLI väärtused.

### <a name="powershell"></a>PowerShelli

Rollimäärang nime jaoks on vaja globaalselt kordumatu. Saate luua uue tunnuse **nimi** on:

    $name = [System.Guid]::NewGuid().toString()

Saate tuua rolli definitsiooni tunnuse:

    $roledef = (Get-AzureRmRoleDefinition -Name Reader).id

Saate tuua identifikaator põhisummalt ühega järgmistest käskudest.

Rühma nimega **audiitorite**:

    $principal = (Get-AzureRmADGroup -SearchString Auditors).id

Kasutaja **exampleperson**:

    $principal = (Get-AzureRmADUser -SearchString exampleperson).id

Teenuse põhisumma nimega **exampleapp**:

    $principal = (Get-AzureRmADServicePrincipal -SearchString exampleapp).id 

### <a name="azure-cli"></a>Azure'i CLI

Saate tuua rolli definitsiooni tunnuse:

    azure role show Reader --json | jq .[].Id -r

Saate tuua identifikaator põhisummalt ühega järgmistest käskudest.

Rühma nimega **audiitorite**:

    azure ad group show --search Auditors --json | jq .[].objectId -r

Kasutaja **exampleperson**:

    azure ad user show --search exampleperson --json | jq .[].objectId -r

Teenuse põhisumma nimega **exampleapp**:

    azure ad sp show --search exampleapp --json | jq .[].objectId -r

## <a name="examples"></a>Näited

Järgmist malli saab identifikaatori rolli ja kasutaja, rühma või teenuse põhilise identifikaatori. Seda määrab tasemel ressursi rühma roll.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "roleDefinitionId": {
                "type": "string"
            },
            "roleAssignmentId": {
                "type": "string"
            },
            "principalId": {
                "type": "string"
            }
        },
        "resources": [
            {
              "type": "Microsoft.Authorization/roleAssignments",
              "apiVersion": "2015-07-01",
              "name": "[parameters('roleAssignmentId')]",
              "properties":
              {
                "roleDefinitionId": "[concat(subscription().id, '/providers/Microsoft.Authorization/roleDefinitions/', parameters('roleDefinitionId'))]",
                "principalId": "[parameters('principalId')]",
                "scope": "[concat(subscription().id, '/resourceGroups/', resourceGroup().name)]"
              }
            }
        ],
        "outputs": {}
    }



Järgmise mall loob salvestusruumi konto ja määrab lugeja roll salvestusruumi konto. Kahe rühma ja lugeja rolli identifikaatoreid on lisatud malli kasutuselevõtu lihtsustamiseks. Neid väärtusi võib tuua juurutamise ajal skripti kaudu ja parameetrid möödas.

    {
      "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "roleName": {
          "type": "string"
        },
        "groupToAssign": {
          "type": "string",
          "allowedValues": [
            "Auditors",
            "Limited"
          ]
        }
      },
      "variables": {
        "readerRole": "[concat('/subscriptions/',subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7')]",
        "storageName": "[concat('storage', uniqueString(resourceGroup().id))]",
        "scope": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Storage/storageAccounts/', variables('storageName'))]",
        "Auditors": "1c272299-9729-462a-8d52-7efe5ece0c5c",
        "Limited": "7c7250f0-7952-441c-99ce-40de5e3e30b5",
        "fullRoleName": "[concat(variables('storageName'), '/Microsoft.Authorization/', parameters('roleName'))]"
      },
      "resources": [
        {
          "apiVersion": "2016-01-01",
          "type": "Microsoft.Storage/storageAccounts",
          "name": "[variables('storageName')]",
          "location": "[resourceGroup().location]",
          "sku": {
            "name": "Standard_LRS"
          },
          "kind": "Storage",
          "tags": {
            "displayName": "MyStorageAccount"
          },
          "properties": {}
        },
        {
          "type": "Microsoft.Storage/storageAccounts/providers/roleAssignments",
          "apiVersion": "2015-07-01",
          "name": "[variables('fullRoleName')]",
          "dependsOn": [
            "[variables('storageName')]"
          ],
          "properties": {
            "roleDefinitionId": "[variables('readerRole')]",
            "principalId": "[variables(parameters('groupToAssign'))]"
          }
        }
      ]
    }

## <a name="quickstart-templates"></a>Kiirjuhend Mallid

Järgmised Mallid kuvamine rolli määramine ressursi kasutamise kohta:

- [Ressursirühm sisseehitatud rolli määramine](https://azure.microsoft.com/documentation/templates/101-rbac-builtinrole-resourcegroup)
- [Olemasoleva VM sisseehitatud rolli määramine](https://azure.microsoft.com/documentation/templates/101-rbac-builtinrole-virtualmachine)
- [Mitme olemasoleva VMs sisseehitatud rolli määramine](https://azure.microsoft.com/documentation/templates/201-rbac-builtinrole-multipleVMs)

## <a name="next-steps"></a>Järgmised sammud

- Malli struktuuri kohta leiate teavet teemast [loome Azure'i ressursihaldur Mallid](resource-group-authoring-templates.md).
- Rollipõhine juurdepääsu reguleerimine kohta leiate lisateavet teemast [Azure Active Directory rolli vastavalt juurdepääsu reguleerimine](./active-directory/role-based-access-control-configure.md).
