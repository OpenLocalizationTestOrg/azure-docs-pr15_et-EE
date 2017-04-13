<properties 
    pageTitle="Ettevalmistamise Redis vahemälu | Microsoft Azure'i" 
    description="Azure'i ressursihaldur malli abil saate juurutada Azure Redis vahemälu." 
    services="app-service" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="web" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/27/2016" 
    ms.author="sdanie"/>

# <a name="create-a-redis-cache-using-a-template"></a>Malli abil Redis vahemälu luua

Selles teemas saate teada, kuidas luua Azure'i ressursihaldur malli, mis kasutab Azure Redis vahemälu. Olemasoleva salvestusruumi konto saab hoida Diagnostikaandmete vahemälu. Saate teada, kuidas määratleda millised ressursid on juurutatud ja määratlemise parameetrid, mis on määratud, kui käivitatakse juurutamise. Saate oma juurutuste seda malli kasutada või seda vastavalt oma vajadustele kohandada.

Diagnostikasätete on praegu ühiskasutuses kõik vahemälu piirkonna tellimusele. Kõigi teiste sama piirkonna vahemälu värskendamine ühe vahemälu piirkonna mõjutab.

Mallide loomise kohta leiate lisateavet teemast [Azure ressursihaldur mallide koostamine](../resource-group-authoring-templates.md).

Täieliku Mall, vaadake [Redis vahemälu malli](https://github.com/Azure/azure-quickstart-templates/blob/master/101-redis-cache/azuredeploy.json).

>[AZURE.NOTE] Uue [Premium taseme](cache-premium-tier-intro.md) ressursihaldur Mallid on saadaval. 
>
>-    [Rühmitamise Premium Redis vahemälu loomine](https://azure.microsoft.com/documentation/templates/201-redis-premium-cluster-diagnostics/)
>-    [Luua Premium Redis vahemälu andmete püsimine](https://azure.microsoft.com/documentation/templates/201-redis-premium-persistence/)
>-    [Valikuline rühmitamise ja VNet Premium Redis vahemälu loomine](https://azure.microsoft.com/documentation/templates/201-redis-premium-vnet-cluster-diagnostics/)
>
>Uusimate mallide otsimiseks leiate [Azure'i Kiirjuhend mallide](https://azure.microsoft.com/documentation/templates/) ja otsida `Redis Cache`.

## <a name="what-you-will-deploy"></a>Mis on juurutamine

Selle malli juurutamist Azure Redis vahemälu, Diagnostikaandmete salvestusruumi kontot kasutava.

Juurutamise automaatselt käivitada, nuppu:

[![Azure'i juurutamine](./media/cache-redis-cache-arm-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-redis-cache%2Fazuredeploy.json)

## <a name="parameters"></a>Parameetrid

Koos Azure ressursihaldur, määratlege parameetrite väärtused, mida soovite määrata malli juurutamisel. Mall sisaldab nimega parameetrite jaotis, mis sisaldab kõiki parameetrite väärtused.
Määratlege parameeter on need väärtused, mis erinevad juurutate projekti või keskkond, mis on kasutusele võtavad põhjal. Määratleda parameetrite väärtused, mis on alati jääma samaks. Iga parameetri väärtuse kasutatakse määratleda juurutatud ressursside mall. 


[AZURE.INCLUDE [app-service-web-deploy-redis-parameters](../../includes/cache-deploy-parameters.md)]

### <a name="rediscachelocation"></a>redisCacheLocation

Redis vahemälu asukoht. Parima jõudluse tagamiseks kasutage paigal rakenduse kasutamiseks koos vahemälu.

    "redisCacheLocation": {
      "type": "string"
    }

### <a name="existingdiagnosticsstorageaccountname"></a>existingDiagnosticsStorageAccountName

Olemasoleva salvestusruumi konto, mida soovite kasutada diagnostika nimi. 

    "existingDiagnosticsStorageAccountName": {
      "type": "string"
    }

### <a name="enablenonsslport"></a>enableNonSslPort

Loogikaväärtus, mis näitab, kas lubada juurdepääsu mitte-SSL-pordid kaudu.

    "enableNonSslPort": {
      "type": "bool"
    }

### <a name="diagnosticsstatus"></a>diagnosticsStatus

Väärtus, mis näitab, kas diagnostika on lubatud. Kasutage sees või väljas.

    "diagnosticsStatus": {
      "type": "string",
      "defaultValue": "ON",
      "allowedValues": [
            "ON",
            "OFF"
        ]
    }
    
## <a name="resources-to-deploy"></a>Juurutamiseks ressursid

### <a name="redis-cache"></a>Redis vahemälu

Loob Azure Redis vahemälu.

    {
      "apiVersion": "2015-08-01",
      "name": "[parameters('redisCacheName')]",
      "type": "Microsoft.Cache/Redis",
      "location": "[parameters('redisCacheLocation')]",
      "properties": {
        "enableNonSslPort": "[parameters('enableNonSslPort')]",
        "sku": {
          "capacity": "[parameters('redisCacheCapacity')]",
          "family": "[parameters('redisCacheFamily')]",
          "name": "[parameters('redisCacheSKU')]"
        }
      },
      "resources": [
        {
          "apiVersion": "2015-07-01",
          "type": "Microsoft.Cache/redis/providers/diagnosticsettings",
          "name": "[concat(parameters('redisCacheName'), '/Microsoft.Insights/service')]",
          "location": "[parameters('redisCacheLocation')]",
          "dependsOn": [
            "[concat('Microsoft.Cache/Redis/', parameters('redisCacheName'))]"
          ],
          "properties": {
            "status": "[parameters('diagnosticsStatus')]",
            "storageAccountName": "[parameters('existingDiagnosticsStorageAccountName')]"
          }
        }
      ]
    }



## <a name="commands-to-run-deployment"></a>Käskude käivitamiseks juurutamine

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)] 

### <a name="powershell"></a>PowerShelli

    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-redis-cache/azuredeploy.json -ResourceGroupName ExampleDeployGroup -redisCacheName ExampleCache -redisCacheLocation "West US"

### <a name="azure-cli"></a>Azure'i CLI

    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-redis-cache/azuredeploy.json -g ExampleDeployGroup


