<properties
    pageTitle="Sündmuse jaoturi ja tarbija rühmaga, mis on Azure ressursihaldur malli abil soovitud sündmus jaoturi nimeruum loomine | Microsoft Azure'i"
    description="Sündmuse jaoturi ja tarbija rühma Azure'i ressursihaldur malli abil soovitud sündmus jaoturi nimeruum loomiseks"
    services="event-hubs"
    documentationCenter=".net"
    authors="sethmanheim"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.devlang="tbd"
    ms.topic="article"
    ms.tgt_pltfrm="dotnet"
    ms.workload="na"
    ms.date="08/31/2016"
    ms.author="sethm;shvija"/>

# <a name="create-an-event-hubs-namespace-with-event-hub-and-consumer-group-using-an-azure-resource-manager-template"></a>Sündmuse jaoturi ja tarbija rühmaga, mis on Azure ressursihaldur malli abil soovitud sündmus jaoturi nimeruum loomine

Selles artiklis kirjeldatakse Azure'i ressursihaldur Mall, mis loob mõne sündmuse jaoturi nimeruum mõni sündmus jaoturi ja tarbija rühma kasutamise kohta. Saate teada, kuidas määratleda millised ressursid on juurutatud ja määratlemise parameetrid, mis on määratud, kui käivitatakse juurutamise. Saate oma juurutuste seda malli kasutada või seda vastavalt oma vajadustele kohandada

Mallide loomise kohta leiate lisateavet teemast [Azure ressursihaldur loome Mallid][].

Täieliku Mall, vaadake [sündmuse jaoturi ja tarbija rühma malli][] github.

>[AZURE.NOTE]
>Uusimate mallide otsimiseks leiate [Azure'i Kiirjuhend][] kuukalendreid ja sündmuse jaoturi otsimine.

## <a name="what-will-you-deploy"></a>Mida saate juurutada?

Selle malli abil juurutamist mõni sündmus jaoturi nimeruum mõni sündmus jaoturi ja tarbija rühma.

[Sündmuse jaoturi](../event-hubs/event-hubs-what-is-event-hubs.md) on teenuse pakkumiseks sündmus ja telemeetria sissepääsu Azure suuri tasandil madal latentsus ja usaldusväärsus sündmus.

Juurutamise automaatselt käivitada, nuppu:

[![Azure'i juurutamine](./media/event-hubs-resource-manager-namespace-event-hub/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-event-hubs-create-event-hub-and-consumer-group%2Fazuredeploy.json)

## <a name="parameters"></a>Parameetrid

Koos Azure ressursihaldur, määratlege parameetrite väärtused, mida soovite määrata malli juurutamisel. Mall sisaldab jaotis `Parameters` kõigi parameetrite väärtused, mis sisaldab. Määratlege parameeter on need väärtused, mis erinevad juurutate projekti või keskkond, mis on kasutusele võtavad põhjal. Määratleda parameetrite väärtused, mis jäävad alati samaks. Juurutatud ressursside määratlemiseks kasutatakse iga parameetri väärtuse mall.

Mall määratleb järgmise parameetriga.

### <a name="eventhubnamespacename"></a>eventHubNamespaceName

Sündmuse jaoturi nimeruumi loomiseks nimi.

```
"eventHubNamespaceName": {
"type": "string"
}
```

### <a name="eventhubname"></a>eventHubName

Sündmuse jaoturi nime loodud sündmuse jaoturi nimeruum.

```
"eventHubName": {
"type": "string"
}
```

### <a name="eventhubconsumergroupname"></a>eventHubConsumerGroupName

Loodud sündmuse jaoturi tarbija rühma nimi.

```
"eventHubConsumerGroupName": {
"type": "string"
}
```

### <a name="apiversion"></a>apiVersion

Malli API versiooni.

```
"apiVersion": {
"type": "string"
}
```

## <a name="resources-to-deploy"></a>Juurutamiseks ressursid

Loob nimeruum tüüpi **EventHubs**, on sündmuse jaoturi ja tarbija rühma.

```
"resources":[  
      {  
         "apiVersion":"[variables('ehVersion')]",
         "name":"[parameters('namespaceName')]",
         "type":"Microsoft.EventHub/Namespaces",
         "location":"[variables('location')]",
         "sku":{  
            "name":"Standard",
            "tier":"Standard"
         },
         "resources":[  
            {  
               "apiVersion":"[variables('ehVersion')]",
               "name":"[parameters('eventHubName')]",
               "type":"EventHubs",
               "dependsOn":[  
                  "[concat('Microsoft.EventHub/namespaces/', parameters('namespaceName'))]"
               ],
               "properties":{  
                  "path":"[parameters('eventHubName')]"
               },
               "resources":[  
                  {  
                     "apiVersion":"[variables('ehVersion')]",
                     "name":"[parameters('consumerGroupName')]",
                     "type":"ConsumerGroups",
                     "dependsOn":[  
                        "[parameters('eventHubName')]"
                     ],
                     "properties":{  

                     }
                  }
               ]
            }
         ]
      }
   ],
```

## <a name="commands-to-run-deployment"></a>Käskude käivitamiseks juurutamine

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a>PowerShelli

```
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-event-hubs-create-event-hub-and-consumer-group/azuredeploy.json
```

## <a name="azure-cli"></a>Azure'i CLI

```
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri [https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-event-hubs-create-event-hub-and-consumer-group/azuredeploy.json][]
```

[Azure'i ressursihaldur mallide koostamine]: ../resource-group-authoring-templates.md
[Azure'i Kiirjuhend Mallid]:  https://azure.microsoft.com/documentation/templates/?term=event+hubs
[Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
[Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
[Sündmuse jaoturi ja tarbija rühma Mall]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-event-hubs-create-event-hub-and-consumer-group/
