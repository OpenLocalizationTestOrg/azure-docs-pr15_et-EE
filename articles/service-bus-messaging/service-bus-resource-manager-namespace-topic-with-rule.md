<properties
    pageTitle="Luua teenuse siini nimeruum teema, tellimus ja reegel on Azure ressursihaldur malli abil | Microsoft Azure'i"
    description="Luua teenuse siini nimeruum teema, tellimus ja reegli Azure'i ressursihaldur malli abil"
    services="service-bus"
    documentationCenter=".net"
    authors="ShubhaVijayasarathy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="tbd"
    ms.topic="article"
    ms.tgt_pltfrm="dotnet"
    ms.workload="na"
    ms.date="10/25/2016"
    ms.author="ShubhaVijayasarathy"/>

# <a name="create-a-service-bus-namespace-with-topic-subscription-and-rule-using-an-azure-resource-manager-template"></a>Luua teenuse siini nimeruum teema, tellimus ja reegel on Azure ressursihaldur malli abil

Selles artiklis kirjeldatakse, kuidas kasutada Azure ressursihaldur malli, mis loob teenuse siini nimeruum teema, tellimus ja reegli (filter). Saate teada, kuidas määratleda millised ressursid on juurutatud ja määratlemise parameetrid, mis on määratud, kui käivitatakse juurutamise. Saate oma juurutuste seda malli kasutada või seda vastavalt oma vajadustele kohandada

Mallide loomise kohta leiate lisateavet teemast [Azure ressursihaldur loome Mallid][].

Tavade kohta lisateabe saamiseks ja Azure ressursse nimereeglid mustreid leiate teemast [Azure ressursse nimetamise põhimõtted][].

Täieliku Mall, leiate [teema, tellimus ja reegli teenuse siini nimeruumi][] mall.

>[AZURE.NOTE] Azure'i ressursihaldur järgmised Mallid on saadaval allalaadimiseks ja juurutamine.
>
>-    [Teenuse siini nimeruum kuhjuda ja luba reegli loomine](service-bus-resource-manager-namespace-auth-rule.md)
>-    [Luua teenuse siini nimeruum järjekord](service-bus-resource-manager-namespace-queue.md)
>-    [Teenuse siini nimeruum loomine](service-bus-resource-manager-namespace.md)
>-    [Teema ja tellimuse teenuse siini nimeruum loomiseks](service-bus-resource-manager-namespace-topic.md)
>
>Uusimate mallide otsimiseks leiate [Azure'i Kiirjuhend][] kuukalendreid ja teenuse siini otsimine.

## <a name="what-will-you-deploy"></a>Mida saate juurutada?

Selle malli abil saate juurutada teenuse siini nimeruum teema, tellimus ja reegli (filter).

[Teenuse siini teemade ja tellimuste](service-bus-queues-topics-subscriptions.md#topics-and-subscriptions) sisestage vormile üks-mitmele teatise *avaldamise/tellimise* muster. Teemade ja tellimused, kasutades komponendid jaotatud rakendus pole otse omavahel suhelda, selle asemel nad Exchange'i sõnumeid teema, mis on vahendajaks kaudu. Teema tellimust sarnaneb virtuaalse järjekorda, mida saab teema saadetud sõnumite koopiad. Filtri tellimus võimaldab teil määrata, milliseid sõnumeid saata teema peaks kuvatama kindla teemaga seotud tellimuse sees.

## <a name="what-are-rules-filters"></a>Mis on reeglid (filtrid)?

Mitme stsenaariumi töödelda sõnumid, mis on teatud omadused erinevatel viisidel. Selle saate konfigureerida tellimused, et leida sõnumid, mis on soovitud atribuudid ja seejärel nende atribuutide teatud muudatusi teha. Ajal teenuse siini tellimuste näha kõiki sõnumeid, mis saadetakse teema, saate kopeerida ainult need sõnumid alamhulk virtuaalse tellimuse järjekorda. Seda saab teha tellimuse filtrite abil. Rules(filters) kohta lisateabe saamiseks vt [teenuse siini järjekorrad, teemade, ja tellimused][].

Juurutamise automaatselt käivitada, nuppu:

[![Azure'i juurutamine](./media/service-bus-resource-manager-namespace-topic/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-servicebus-create-topic-subscription-rule%2Fazuredeploy.json)

## <a name="parameters"></a>Parameetrid

Koos Azure ressursihaldur, tuleks väärtuste määramiseks malli juurutamisel parameetrite määratlemine. Mall sisaldab jaotis `Parameters` , mis sisaldab kõigi parameetrite väärtused. Määratlege parameeter on need väärtused, mis erinevad juurutate projekti või keskkond, mis on kasutusele võtavad põhjal. Määratleda parameetrite väärtused, mis on alati jääma samaks. Iga parameetri väärtuse kasutatakse määratleda juurutatud ressursside mall.

Mall määratleb järgmisi:

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
### <a name="servicebusrulename"></a>serviceBusRuleName

Rule(filter), mis on loodud teenuse siini nimeruumi nimi.

```
   "serviceBusRuleName": {
   "type": "string",
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

Loob standard teenuse siini nimeruum tüüpi **sõnumid**, teema ja tellimuse ja reegleid.

```
 "resources": [{
        "apiVersion": "[variables('sbVersion')]",
        "name": "[parameters('serviceBusNamespaceName')]",
        "type": "Microsoft.ServiceBus/Namespaces",
        "location": "[variables('location')]",
        "sku": {
            "name": "Standard",
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
                "path": "[parameters('serviceBusTopicName')]"
            },
            "resources": [{
                "apiVersion": "[variables('sbVersion')]",
                "name": "[parameters('serviceBusSubscriptionName')]",
                "type": "Subscriptions",
                "dependsOn": [
                    "[parameters('serviceBusTopicName')]"
                ],
                "properties": {},
                "resources": [{
                    "apiVersion": "[variables('sbVersion')]",
                    "name": "[parameters('serviceBusRuleName')]",
                    "type": "Rules",
                    "dependsOn": [
                        "[parameters('serviceBusSubscriptionName')]"
                    ],
                    "properties": {
                        "filter": {
                            "sqlExpression": "StoreName = 'Store1'"
                        },
                        "action": {
                            "sqlExpression": "set FilterTag = 'true'"
                        }
                    }
                }]
            }]
        }]
    }]
```

## <a name="commands-to-run-deployment"></a>Käskude käivitamiseks juurutamine

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a>PowerShelli

```
New-AzureResourceGroupDeployment -Name \<deployment-name\> -ResourceGroupName \<resource-group-name\> -TemplateUri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-topic-subscription-rule/azuredeploy.json>
```

## <a name="azure-cli"></a>Azure'i CLI

```
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-servicebus-create-topic-subscription-rule/azuredeploy.json>
```

## <a name="next-steps"></a>Järgmised sammud

Nüüd, kui olete loonud ja juurutada Azure ressursihaldur abil, saate teada, kuidas hallata järgmiste ressursside vaatamise järgmistest artiklitest:

- [Azure'i teenus siini abil Azure automatiseerimine haldamine](service-bus-automation-manage.md)
- [Teenuse siini PowerShelliga haldamine](service-bus-powershell-how-to-provision.md)
- [Teenuse siini ressursid Exploreriga teenuse siini haldamine](https://code.msdn.microsoft.com/Service-Bus-Explorer-f2abca5a)


  [Azure'i ressursihaldur mallide koostamine]: ../resource-group-authoring-templates.md
  [Azure'i Kiirjuhend Mallid]: https://azure.microsoft.com/documentation/templates/?term=service+bus
  [Learn more about Service Bus topics and subscriptions]: service-bus-queues-topics-subscriptions.md
  [Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
  [Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
  [Azure'i ressursid nimetamise põhimõtted]: https://azure.microsoft.com/en-us/documentation/articles/guidance-naming-conventions/
  [Teenuse siini nimeruum teema, tellimus ja reegel]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-topic-subscription-rule/
  [Teenuse siini järjekorrad, teemasid ja tellimused]:service-bus-queues-topics-subscriptions.md
  
