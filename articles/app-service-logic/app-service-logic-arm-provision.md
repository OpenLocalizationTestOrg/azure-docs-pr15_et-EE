<properties 
    pageTitle="Azure'i rakenduse teenuses Azure ressursihaldur mallide kasutamine loogika rakenduse loomine | Microsoft Azure'i" 
    description="Azure'i ressursihaldur malli kasutamiseks juurutamiseks tühja loogika rakenduse määratlemiseks töövood." 
    services="logic-apps" 
    documentationCenter="" 
    authors="MSFTMan" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/25/2016" 
    ms.author="deonhe"/>

# <a name="create-a-logic-app-using-a-template"></a>Malli abil loogika rakenduse loomine

Mõne Azure'i ressursihaldur malli abil saate määratleda töövoogude kasutatud tühja loogika rakenduse loomine. Saate määrata, millised ressursid on juurutatud ja juurutamise täitmisel määratud parameetrite määratlemine. Saate oma juurutuste seda malli kasutada või seda vastavalt oma vajadustele kohandada.

Loogika rakenduse atribuutide kohta leiate lisateavet teemast [Loogika rakenduse töövoo haldamine API](https://msdn.microsoft.com/library/azure/mt643788.aspx). 

Näiteid ise määratluse leiate [autori loogika rakenduse määratlusi](app-service-logic-author-definitions.md). 

Mallide loomise kohta leiate lisateavet teemast [Azure ressursihaldur mallide koostamine](../resource-group-authoring-templates.md).

Täieliku Mall, vaadake [loogika rakenduse malli](https://github.com/Azure/azure-quickstart-templates/blob/master/101-logic-app-create/azuredeploy.json).

## <a name="what-you-will-deploy"></a>Mis on juurutamine

Selle malli abil saate juurutada loogika rakendus.

Juurutamise automaatselt käivitada, valige nupp järgmine:  

[![Azure'i juurutamine](media/app-service-logic-arm-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-logic-app-create%2Fazuredeploy.json)

## <a name="parameters"></a>Parameetrid

[AZURE.INCLUDE [app-service-logic-deploy-parameters](../../includes/app-service-logic-deploy-parameters.md)]

### <a name="testuri"></a>testUri

     "testUri": {
        "type": "string",
        "defaultValue": "http://azure.microsoft.com/en-us/status/feed/"
      }
    
## <a name="resources-to-deploy"></a>Juurutamiseks ressursid

### <a name="logic-app"></a>Loogika rakendus

Loob loogika rakendus.

Malli kasutab parameetri väärtuse loogika rakenduse nimi. Seda määrab loogika rakenduse asukoha ressursirühma samasse asukohta. 

Selle kindla määratluse läheb üks tund ja ping **testUri** parameetris määratud asukohta. 

    {
      "type": "Microsoft.Logic/workflows",
      "apiVersion": "2016-06-01",
      "name": "[parameters('logicAppName')]",
      "location": "[resourceGroup().location]",
      "tags": {
        "displayName": "LogicApp"
      },
      "properties": {
        "definition": {
          "$schema": "http://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "testURI": {
              "type": "string",
              "defaultValue": "[parameters('testUri')]"
            }
          },
          "triggers": {
            "recurrence": {
              "type": "recurrence",
              "recurrence": {
                "frequency": "Hour",
                "interval": 1
              }
            }
          },
          "actions": {
            "http": {
              "type": "Http",
              "inputs": {
                "method": "GET",
                "uri": "@parameters('testUri')"
              },
              "runAfter": {}
            }
          },
          "outputs": {}
        },
        "parameters": {}
      }
    }


## <a name="commands-to-run-deployment"></a>Käskude käivitamiseks juurutamine

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>PowerShelli

    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json -ResourceGroupName ExampleDeployGroup

### <a name="azure-cli"></a>Azure'i CLI

    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json -g ExampleDeployGroup


 
