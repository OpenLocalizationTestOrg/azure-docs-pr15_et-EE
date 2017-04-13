<properties
    pageTitle="Luua teenuse siini nimeruum teema ja tellimus on Azure ressursihaldur malli abil | Microsoft Azure'i"
    description="Luua teenuse siini nimeruum teema ja tellimuse Azure'i ressursihaldur malli abil"
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
    ms.date="10/14/2016"
    ms.author="sethm;shvija"/>

# <a name="create-a-service-bus-namespace-with-topic-and-subscription-using-an-azure-resource-manager-template"></a>Luua teenuse siini nimeruum teema ja tellimus on Azure ressursihaldur malli abil

Selles artiklis kirjeldatakse Azure'i ressursihaldur Mall, mis loob teenuse siini nimeruumi, teema ja tellimuse kasutamise kohta. Saate teada, kuidas määratleda millised ressursid on juurutatud ja määratlemine parameetritele, mis on määratud, kui käivitatakse juurutamise. Saate oma juurutuste seda malli kasutada või seda vastavalt oma vajadustele kohandada

Mallide loomise kohta leiate lisateavet teemast [Azure ressursihaldur loome Mallid][].

Täieliku Mall, leiate [teema ja tellimuse teenuse siini nimeruumi][] mall.

>[AZURE.NOTE] Azure'i ressursihaldur järgmised Mallid on saadaval allalaadimiseks ja juurutamine.
>
>-    [Teenuse siini nimeruum kuhjuda ja luba reegli loomine](service-bus-resource-manager-namespace-auth-rule.md)
>-    [Luua teenuse siini nimeruum järjekord](service-bus-resource-manager-namespace-queue.md)
>-    [Teenuse siini nimeruum loomine](service-bus-resource-manager-namespace.md)
>-    [Sündmuse jaoturi ja tarbija rühmaga mõni sündmus jaoturi nimeruum loomine](../event-hubs/event-hubs-resource-manager-namespace-event-hub.md)
>
>Uusimate mallide otsimiseks leiate [Azure'i Kiirjuhend][] kuukalendreid ja otsing "Teenuse siini."

## <a name="what-will-you-deploy"></a>Mida saate juurutada?

Selle malli abil juurutamist teenuse siini nimeruum tellimus ja teema.

[Teenuse siini teemade ja tellimuste](service-bus-queues-topics-subscriptions.md#topics-and-subscriptions) sisestage vormile üks-mitmele teatise *avaldamise/tellimise* muster.

Juurutamise automaatselt käivitada, nuppu:

[![Azure'i juurutamine](./media/service-bus-resource-manager-namespace-topic/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-servicebus-create-topic-and-subscription%2Fazuredeploy.json)

## <a name="parameters"></a>Parameetrid

Koos Azure ressursihaldur, määratlege parameetrite väärtused, mida soovite määrata malli juurutamisel. Mall sisaldab jaotis `Parameters` kõigi parameetrite väärtused, mis sisaldab. Määratlege parameeter on need väärtused, mis erinevad juurutate projekti või keskkond, mis on kasutusele võtavad põhjal. Määratleda parameetrite väärtused, mis jäävad alati samaks. Iga parameetri väärtuse kasutatakse määratleda juurutatud ressursside mall.

Mall määratleb järgmiste parameetrite abil.

### <a name="servicebusnamespacename"></a>serviceBusNamespaceName

Teenuse siini nimeruumi loomiseks nimi.

```
"serviceBusNamespaceName": {
"type": "string"
}
```

### <a name="servicebustopicname"></a>serviceBusTopicName

Teema loodud teenuse siini nimeruumi nimi.

```
"serviceBusTopicName": {
"type": "string"
}
```

### <a name="servicebussubscriptionname"></a>serviceBusSubscriptionName

Teenuse siini nimeruumi loodud tellimuse nimi.

```
"serviceBusSubscriptionName": {
"type": "string"
}
```

### <a name="servicebusapiversion"></a>serviceBusApiVersion

Malli teenuse siini API versiooni.

```
"serviceBusApiVersion": {
"type": "string"
}
```
## <a name="resources-to-deploy"></a>Juurutamiseks ressursid

Loob standard teenuse siini nimeruum tüüpi **sõnumid**, teema ja tellimuse.

```
"resources ": [{
        "apiVersion": "[variables('sbVersion')]",
        "name": "[parameters('serviceBusNamespaceName')]",
        "type": "Microsoft.ServiceBus/Namespaces",
        "location": "[variables('location')]",
        "kind": "Messaging",
        "sku": {
            "name": "StandardSku",
            "tier": "Standard"
        },
        "resources": [{
            "apiVersion": "[variables('sbVersion')]",
            "name": "[parameters('serviceBusTopicName')]",
            "type": "Topics",
            "dependsOn": [
                "[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"
            ],
            "properties": {
                "path": "[parameters('serviceBusTopicName')]",
            },
            "resources": [{
                "apiVersion": "[variables('sbVersion')]",
                "name": "[parameters('serviceBusSubscriptionName')]",
                "type": "Subscriptions",
                "dependsOn": [
                    "[parameters('serviceBusTopicName')]"
                ],
                "properties": {}
            }]
        }]
    }]
```

## <a name="commands-to-run-deployment"></a>Käskude käivitamiseks juurutamine

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a>PowerShelli

```
New-AzureResourceGroupDeployment -Name \<deployment-name\> -ResourceGroupName \<resource-group-name\> -TemplateUri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-topic-and-subscription/azuredeploy.json>
```

## <a name="azure-cli"></a>Azure'i CLI

```
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-topic-and-subscription/azuredeploy.json>
```

## <a name="next-steps"></a>Järgmised sammud

Nüüd, kui olete loonud ja juurutada Azure ressursihaldur abil, saate teada, kuidas hallata järgmiste ressursside vaatamise järgmistest artiklitest:

- [Teenuse siini PowerShelliga haldamine](service-bus-powershell-how-to-provision.md)
- [Teenuse siini ressursid Exploreriga teenuse siini haldamine](https://code.msdn.microsoft.com/Service-Bus-Explorer-f2abca5a)


  [Azure'i ressursihaldur mallide koostamine]: ../resource-group-authoring-templates.md
  [Azure'i Kiirjuhend Mallid]: https://azure.microsoft.com/documentation/templates/?term=service+bus
  [Learn more about Service Bus topics and subscriptions]: service-bus-queues-topics-subscriptions.md
  [Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
  [Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
  [Teenuse siini nimeruum teema ja tellimuse]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-topic-and-subscription/
