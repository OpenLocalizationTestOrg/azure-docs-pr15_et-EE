<properties
    pageTitle="Teenuse siini autoriseerimine reeglit on Azure ressursihaldur malli abil luua | Microsoft Azure'i"
    description="Nimeruumi ja Azure ressursihaldur malli abil järjekorda teenuse siini autoriseerimine reegli loomine"
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

# <a name="create-a-service-bus-authorization-rule-for-namespace-and-queue-using-an-azure-resource-manager-template"></a>Nimeruumi ja mõni Azure ressursihaldur mall abil järjekorda teenuse siini autoriseerimine reegli loomine

Selles artiklis kirjeldatakse kasutamine Azure ressursihaldur Mall, mis loob [autoriseerimine reegel](service-bus-authentication-and-authorization.md#shared-access-signature-authentication) teenuse siini nimeruum ja järjekorda. Saate teada, kuidas määratleda millised ressursid on juurutatud ja määratlemine parameetritele, mis on määratud, kui käivitatakse juurutamise. Saate oma juurutuste seda malli kasutada või seda vastavalt oma vajadustele kohandada.

Mallide loomise kohta leiate lisateavet teemast [Azure ressursihaldur loome Mallid][].

Täieliku Mall, vaadake [teenuse siini auth reeglimalli][] github.

>[AZURE.NOTE] Azure'i ressursihaldur järgmised Mallid on saadaval allalaadimiseks ja juurutamine.
>
>-    [Sündmuse jaoturi ja tarbija rühmaga mõni sündmus jaoturi nimeruum loomine](../event-hubs/event-hubs-resource-manager-namespace-event-hub.md)
>-    [Luua teenuse siini nimeruum järjekord](service-bus-resource-manager-namespace-queue.md)
>-    [Teema ja tellimuse teenuse siini nimeruum loomiseks](service-bus-resource-manager-namespace-topic.md)
>-    [Teenuse siini nimeruum loomine](service-bus-resource-manager-namespace.md)
>
>Uusimate mallide otsimiseks leiate [Azure'i Kiirjuhend][] kuukalendreid ja otsing "Teenuse siini."

## <a name="what-will-you-deploy"></a>Mida saate juurutada?

Selle malli abil juurutamist teenuse siini autoriseerimine reegli nimeruum ja sõnumside üksus (järjekorras sel juhul).

See mall kasutab autentimise [Ühiskasutusse Accessi allkirja (SAS)](service-bus-sas-overview.md) . SAS võimaldab rakendusi autentida teenuse siini konfigureeritud nimeruumi või kiirsõnumside üksus (järjekorda või teema), millega on seotud õigusi kiirklahvide abil. Seejärel saate selle klahvi SAS luba, et kliendid saavad kasutada omakorda autentida teenuse siini loomiseks.

Juurutamise automaatselt käivitada, nuppu:

[![Azure'i juurutamine](./media/service-bus-resource-manager-namespace-auth-rule/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F301-servicebus-create-authrule-namespace-and-queue%2Fazuredeploy.json)

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

### <a name="namespaceauthorizationrulename"></a>namespaceAuthorizationRuleName 

Nimeruumi autoriseerimine reegli nimi.

```
"namespaceAuthorizationRuleName ": {
"type": "string"
}
```

### <a name="servicebusqueuename"></a>serviceBusQueueName

Teenuse siini nimeruumi järjekorra nimi.

```
"serviceBusQueueName": {
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

Loob standard teenuse siini nimeruum tüüpi **sõnumid**ja teenuse siini autoriseerimine reegli nimeruum ja üksus.

```
"resources": [
        {
            "apiVersion": "[variables('sbVersion')]",
            "name": "[parameters('serviceBusNamespaceName')]",
            "type": "Microsoft.ServiceBus/namespaces",
            "location": "[variables('location')]",
            "kind": "Messaging",
            "sku": {
                "name": "StandardSku",
                "tier": "Standard"
            },
            "resources": [
                {
                    "apiVersion": "[variables('sbVersion')]",
                    "name": "[parameters('serviceBusQueueName')]",
                    "type": "Queues",
                    "dependsOn": [
                        "[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"
                    ],
                    "properties": {
                        "path": "[parameters('serviceBusQueueName')]"
                    },
                    "resources": [
                        {
                            "apiVersion": "[variables('sbVersion')]",
                            "name": "[parameters('queueAuthorizationRuleName')]",
                            "type": "authorizationRules",
                            "dependsOn": [
                                "[parameters('serviceBusQueueName')]"
                            ],
                            "properties": {
                                "Rights": ["Listen"]
                            }
                        }
                    ]
                }
            ]
        }, {
            "apiVersion": "[variables('sbVersion')]",
            "name": "[variables('namespaceAuthRuleName')]",
            "type": "Microsoft.ServiceBus/namespaces/authorizationRules",
            "dependsOn": ["[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"],
            "location": "[resourceGroup().location]",
            "properties": {
                "Rights": ["Send"]
            }
        }
    ]
```

## <a name="commands-to-run-deployment"></a>Käskude käivitamiseks juurutamine

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>PowerShelli

```
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/301-servicebus-create-authrule-namespace-and-queue/azuredeploy.json>
```

## <a name="azure-cli"></a>Azure'i CLI

```
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri <https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/301-servicebus-create-authrule-namespace-and-queue/azuredeploy.json>
```

## <a name="next-steps"></a>Järgmised sammud

Nüüd, kui olete loonud ja juurutada Azure ressursihaldur abil, saate teada, kuidas hallata järgmiste ressursside vaatamise järgmistest artiklitest:

- [Teenuse siini PowerShelliga haldamine](service-bus-powershell-how-to-provision.md)
- [Teenuse siini ressursid Exploreriga teenuse siini haldamine](https://code.msdn.microsoft.com/Service-Bus-Explorer-f2abca5a)
- [Teenuse siini autentimise ja luba](service-bus-authentication-and-authorization.md)

  [Azure'i ressursihaldur mallide koostamine]: ../resource-group-authoring-templates.md
  [Azure'i Kiirjuhend Mallid]: https://azure.microsoft.com/documentation/templates/?term=service+bus
  [Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
  [Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
  [Teenuse siini auth reeglimalli]: https://github.com/Azure/azure-quickstart-templates/blob/master/301-servicebus-create-authrule-namespace-and-queue/
