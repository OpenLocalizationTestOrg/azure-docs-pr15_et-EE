<properties
    pageTitle="Teenuse siini nimeruumi ressursihaldur malli abil saate luua | Microsoft Azure'i"
    description="Azure'i ressursihaldur malli abil saate luua teenuse siini nimeruum"
    services="service-bus"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="tbd"
    ms.topic="article"
    ms.tgt_pltfrm="dotnet"
    ms.workload="na"
    ms.date="10/04/2016"
    ms.author="sethm;shvija"/>

# <a name="create-a-service-bus-namespace-using-an-azure-resource-manager-template"></a>Looge teenuse siini nimeruumi, mis on Azure ressursihaldur malli abil

Selles artiklis kirjeldatakse, kuidas kasutada Azure ressursihaldur malli, mis loob teenuse siini nimeruum tüüpi **sõnumid** koos Standard/Basic SKU-ga. See artikkel määratleb ka juurutamise täitmiseks määratud parameetrid. Saate oma juurutuste seda malli kasutada või seda vastavalt oma vajadustele kohandada.

Mallide loomise kohta leiate lisateavet teemast [Azure ressursihaldur loome Mallid][].

Täieliku Mall, vaadake [teenuse siini nimeruum malli][] github.

>[AZURE.NOTE] Azure'i ressursihaldur järgmised Mallid on saadaval allalaadimiseks ja juurutamine. 
>
>-    [Sündmuse jaoturi ja tarbija rühmaga mõni sündmus jaoturi nimeruum loomine](../event-hubs/event-hubs-resource-manager-namespace-event-hub.md)
>-    [Luua teenuse siini nimeruum järjekord](service-bus-resource-manager-namespace-queue.md)
>-    [Teema ja tellimuse teenuse siini nimeruum loomiseks](service-bus-resource-manager-namespace-topic.md)
>-    [Teenuse siini nimeruum kuhjuda ja luba reegli loomine](service-bus-resource-manager-namespace-auth-rule.md)
>
>Uusimate mallide otsimiseks leiate [Azure'i Kiirjuhend][] kuukalendreid ja teenuse siini otsimine.

## <a name="what-will-you-deploy"></a>Mida saate juurutada?

Selle malli abil juurutamist teenuse siini nimeruum [Basic, Standard, või Premium](https://azure.microsoft.com/pricing/details/service-bus/) SKU-ga.

Juurutamise automaatselt käivitada, nuppu:

[![Azure'i juurutamine](./media/service-bus-resource-manager-namespace/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-servicebus-create-namespace%2Fazuredeploy.json)

## <a name="parameters"></a>Parameetrid

Koos Azure ressursihaldur, määratlege parameetrite väärtused, mida soovite määrata malli juurutamisel. Mall sisaldab jaotis `Parameters` kõigi parameetrite väärtused, mis sisaldab. Määratlege parameeter on need väärtused, mis erinevad juurutate projekti või keskkond, mis on kasutusele võtavad põhjal. Määratleda parameetrite väärtused, mis jäävad alati samaks. Iga parameetri väärtuse kasutatakse määratleda juurutatud ressursside mall.

See mall määratleb järgmise parameetriga.

### <a name="servicebusnamespacename"></a>serviceBusNamespaceName

Teenuse siini nimeruumi loomiseks nimi.

```
"serviceBusNamespaceName": {
"type": "string",
"metadata": { 
    "description": "Name of the Service Bus namespace" 
    }
}
```

### <a name="servicebussku"></a>serviceBusSKU

Nime loomiseks teenuse siini [SKU-ga](https://azure.microsoft.com/pricing/details/service-bus/) .

```
"serviceBusSku": { 
    "type": "string", 
    "allowedValues": [ 
        "Basic", 
        "Standard",
        "Premium" 
    ], 
    "defaultValue": "Standard", 
    "metadata": { 
        "description": "The messaging tier for service Bus namespace" 
    } 

```

Mall määratleb väärtused, mis on lubatud selle parameetri (Basic, Standard või Premium) ja määrab vaikeväärtuse (standardne), kui pole väärtust määratud.

Teenuse siini hindade kohta leiate lisateavet teemast [teenuse siini hinnad ja arveldamise kohta][].

### <a name="servicebusapiversion"></a>serviceBusApiVersion

Malli teenuse siini API versiooni.

```
"serviceBusApiVersion": { 
       "type": "string", 
       "defaultValue": "2015-08-01", 
       "metadata": { 
           "description": "Service Bus ApiVersion used by the template" 
       } 
```

## <a name="resources-to-deploy"></a>Juurutamiseks ressursid

### <a name="service-bus-namespace"></a>Teenuse siini nimeruum

Loob standard teenuse siini nimeruum tüüpi **sõnumid**.

```
"resources": [
    {
        "apiVersion": "[parameters('serviceBusApiVersion')]",
        "name": "[parameters('serviceBusNamespaceName')]",
        "type": "Microsoft.ServiceBus/Namespaces",
        "location": "[variables('location')]",
        "kind": "Messaging",
        "sku": {
            "name": "StandardSku",
            "tier": "Standard"
        },
        "properties": {
        }
    }
]
```

## <a name="commands-to-run-deployment"></a>Käskude käivitamiseks juurutamine

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>PowerShelli

```
New-AzureRmResourceGroupDeployment -ResourceGroupName <resource-group-name> -TemplateFile https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-servicebus-create-namespace/azuredeploy.json
```

### <a name="azure-cli"></a>Azure'i CLI

```
azure config mode arm

azure group deployment create <my-resource-group> <my-deployment-name> --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-servicebus-create-namespace/azuredeploy.json
```

## <a name="next-steps"></a>Järgmised sammud

Nüüd, kui olete loonud ja juurutada Azure ressursihaldur abil, saate teada, kuidas hallata järgmiste ressursside lugedes järgmistest artiklitest:

- [Teenuse siini PowerShelliga haldamine](service-bus-powershell-how-to-provision.md)
- [Teenuse siini ressursid Exploreriga teenuse siini haldamine](https://code.msdn.microsoft.com/Service-Bus-Explorer-f2abca5a)

  [Azure'i ressursihaldur mallide koostamine]: ../resource-group-authoring-templates.md
  [Teenuse siini nimeruumi Mall]: https://github.com/Azure/azure-quickstart-templates/blob/master/101-servicebus-create-namespace/
  [Azure'i Kiirjuhend Mallid]: https://azure.microsoft.com/documentation/templates/?term=service+bus
  [Teenuse siini hinnad ja arveldamise kohta]: https://azure.microsoft.com/documentation/articles/service-bus-pricing-billing/
  [Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
  [Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
