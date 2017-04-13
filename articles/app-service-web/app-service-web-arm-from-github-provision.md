<properties 
    pageTitle="Veebirakenduse, mis on seotud GitHub hoidla juurutamine" 
    description="Azure'i ressursihaldur malli kasutamiseks juurutamiseks web appi, mis sisaldab projekti GitHub hoidla." 
    services="app-service" 
    documentationCenter="" 
    authors="cephalin" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="04/27/2016" 
    ms.author="cephalin"/>

# <a name="deploy-a-web-app-linked-to-a-github-repository"></a>Lingitud GitHub hoidla web minirakenduse juurutamine

Selles teemas saate teada, kuidas luua Azure'i ressursihaldur malli, mis kasutab web appi, mis on seotud projekti GitHub hoidlas. Saate teada, kuidas määratleda millised ressursid on juurutatud ja määratlemise parameetrid, mis on määratud, kui käivitatakse juurutamise. Saate oma juurutuste seda malli kasutada või seda vastavalt oma vajadustele kohandada.

Mallide loomise kohta leiate lisateavet teemast [Azure ressursihaldur mallide koostamine](../resource-group-authoring-templates.md).

Täieliku Mall, vaadake [Web Appi lingitud GitHub mallile](https://github.com/Azure/azure-quickstart-templates/blob/master/201-web-app-github-deploy/azuredeploy.json).

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="what-you-will-deploy"></a>Mis on juurutamine

Selle malli abil juurutamist web appi, mis sisaldab projekti GitHub kood.

Juurutamise automaatselt käivitada, nuppu:

[![Azure'i juurutamine](./media/app-service-web-arm-from-github-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-web-app-github-deploy%2Fazuredeploy.json)

## <a name="parameters"></a>Parameetrid

[AZURE.INCLUDE [app-service-web-deploy-web-parameters](../../includes/app-service-web-deploy-web-parameters.md)]

### <a name="repourl"></a>repoURL

GitHub hoidlas, mis sisaldab projekti juurutamiseks URL-i. See parameeter sisaldab vaikimisi, kuid see väärtus on mõeldud ainult näitab teile, kuidas anda URL-i hoidla. Kui testimine malli saate kasutada seda väärtust, kuid te soovite pakuvad URL-i oma hoidla töötamisel mall.

    "repoURL": {
        "type": "string",
        "defaultValue": "https://github.com/davidebbo-test/Mvc52Application.git"
    }

### <a name="branch"></a>haru

Hoidla kasutada rakenduse juurutamisel haru. Vaikeväärtus on juhtslaidi, kuid saate sisestada nime, iga haru hoidlas, mida soovite kasutada.

    "branch": {
        "type": "string",
        "defaultValue": "master"
    }
    
## <a name="resources-to-deploy"></a>Juurutamiseks ressursid

[AZURE.INCLUDE [app-service-web-deploy-web-host](../../includes/app-service-web-deploy-web-host.md)]

### <a name="web-app"></a>Web app

Loob web appi, mis on seotud projekti GitHub. 

Saate määrata web appi kaudu **siteName** parameetri nimi ja asukoht web appi kaudu **siteLocation** parameeter. **Ei leia dependsOn** elementi, mall määratleb veebirakenduse sõltuvad teenuse majutusteenuse leping. Kuna see sõltub majutusteenuse leping, ei looda veebirakenduse kuni majutusteenuse lepingut on lõpule jõudnud, luuakse. **Ei leia dependsOn** elemendi kasutatakse ainult juurutamise järjekorras. Kui te ei märgi veebirakenduse sõltuv majutusteenuse lepingut, Azure'i ressursi Mananger proovib korraga nii ressursside loomine ja te saate tõrketeate, kui veebirakenduse luuakse enne majutusteenuse leping.

Veebirakenduse on ka lapse ressurssi, mis on määratletud jaotisest **ressursid** . Lapse ressurss määratleb andmeallika juhtelemendi juurutatud veebirakendusega projekti. Selle malli Juhtelemendi allikas on seotud konkreetse GitHub hoidla. GitHub hoidla on määratletud koodiga **"RepoUrl": "https://github.com/davidebbo-test/Mvc52Application.git"** soovite ehk suur-koodi hoidla URL-i, kui soovite luua aruandemalli, mis kasutab korduvalt ühe projekti aga nõuab minimaalset parameetrite arvu.
Asemel suur-koodis hoidla URL-i, saate lisada parameeter hoidla URL-i ja kasutada **RepoUrl** atribuudi väärtust.

    {
      "apiVersion": "2015-08-01",
      "name": "[parameters('siteName')]",
      "type": "Microsoft.Web/sites",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[resourceId('Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
      ],
      "properties": {
        "serverFarmId": "[parameters('hostingPlanName')]"
      },
      "resources": [
        {
          "apiVersion": "2015-08-01",
          "name": "web",
          "type": "sourcecontrols",
          "dependsOn": [
            "[resourceId('Microsoft.Web/Sites', parameters('siteName'))]"
          ],
          "properties": {
            "RepoUrl": "[parameters('repoURL')]",
            "branch": "[parameters('branch')]",
            "IsManualIntegration": true
          }
        }
      ]
    }

## <a name="commands-to-run-deployment"></a>Käskude käivitamiseks juurutamine

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>PowerShelli

    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-github-deploy/azuredeploy.json -siteName ExampleSite -hostingPlanName ExamplePlan -siteLocation "West US" -ResourceGroupName ExampleDeployGroup

### <a name="azure-cli"></a>Azure'i CLI

    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-web-app-github-deploy/azuredeploy.json


 
