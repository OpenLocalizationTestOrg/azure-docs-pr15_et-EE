<properties
    pageTitle="Luua mõne sündmuse jaoturi nimeruum sündmuse jaoturi ja lubada arhiivi on Azure ressursihaldur malli abil | Microsoft Azure'i"
    description="Luua mõne sündmuse jaoturi nimeruum sündmuse jaoturi ja lubada arhiivi Azure'i ressursihaldur malli abil"
    services="event-hubs"
    documentationCenter=".net"
    authors="ShubhaVijayasarathy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.devlang="tbd"
    ms.topic="article"
    ms.tgt_pltfrm="dotnet"
    ms.workload="na"
    ms.date="09/14/2016"
    ms.author="ShubhaVijayasarathy"/>

# <a name="create-an-event-hubs-namespace-with-event-hub-and-enable-archive-using-an-azure-resource-manager-template"></a>Luua mõne sündmuse jaoturi nimeruum sündmuse jaoturi ja lubada arhiivi on Azure ressursihaldur malli abil

Selles artiklis kirjeldatakse Azure'i ressursihaldur Mall, mis loob mõne sündmuse jaoturi nimeruum abil soovitud sündmus keskuses ja võimaldab arhiivi sisse oma sündmuse keskuses kasutamise kohta. Saate teada, kuidas määratleda millised ressursid on juurutatud ja määratlemise parameetrid, mis on määratud, kui käivitatakse juurutamise. Saate oma juurutuste seda malli kasutada või seda vastavalt oma vajadustele kohandada

Mallide loomise kohta leiate lisateavet teemast [Azure ressursihaldur koostamine malli][].

Lisateavet harjutamine ja Azure ressursse nimereeglid mustreid, leiate [Azure'i ressursid nimetamise põhimõtted][].

Täieliku Mall, vaadake [sündmuse jaoturi ja luba arhiivi malli][] github.

>[AZURE.NOTE]
>Uusimate mallide otsimiseks leiate [Azure'i Kiirjuhend][] kuukalendreid ja sündmuse jaoturi otsimine.

## <a name="what-you-deploy"></a>Juurutada?

Selle malli abil juurutada mõne sündmuse jaoturi nimeruum abil soovitud sündmus keskuses ja lubada arhiivi.

[Sündmuse jaoturi](../event-hubs/event-hubs-what-is-event-hubs.md) on teenuse pakkumiseks sündmus ja telemeetria sissepääsu Azure suuri tasandil madal latentsus ja usaldusväärsus sündmus. Sündmuse jaoturi arhiivi võimaldab teil automaatselt esitamisel voogesituse andmete oma sündmuse jaoturi Azure'i bloobimälu oma valik on määratud aja jooksul või suurus intervall enda valitud Meilikausta.

Juurutamise automaatseks käivitamiseks nuppu:

[![Azure'i juurutamine](./media/event-hubs-resource-manager-namespace-event-hub/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-eventhubs-create-namespace-and-enable-archive%2Fazuredeploy.json)

## <a name="parameters"></a>Parameetrid

Koos Azure ressursihaldur, määratlege parameetrite väärtused, mida soovite määrata malli juurutamisel. Mall sisaldab jaotis `Parameters` , mis sisaldab kõigi parameetrite väärtused. Määratlege parameeter on need väärtused, mis erinevad juurutate projekti või keskkond, mis on kasutusele võtavad põhjal. Määratleda parameetrite väärtused, mis on alati jääma samaks. Iga parameetri väärtuse kasutatakse määratleda juurutatud ressursside mall.

Mall määratleb järgmise parameetriga.

### <a name="eventhubnamespacename"></a>eventHubNamespaceName

Sündmuse jaoturi nimeruumi loomiseks nimi.

```
"eventHubNamespaceName":{  
     "type":"string",
     "metadata":{  
         "description":"Name of the EventHub namespace"
      }
}
```

### <a name="eventhubname"></a>eventHubName

Sündmuse jaoturi nime loodud sündmuse jaoturi nimeruum.

```
"eventHubName":{  
    "type":"string",
    "metadata":{  
        "description":"Name of the Event Hub"
    }
}
```

### <a name="messageretentionindays"></a>messageRetentionInDays

Soovitud säilitatakse sõnumid teie sündmuse keskuses päevade arv. 

```
"messageRetentionInDays":{
    "type":"int",
    "defaultValue": 1,
    "minValue":"1",
    "maxValue":"7",
    "metadata":{
       "description":"How long to retain the data in Event Hub"
     }
 }
```

### <a name="partitioncount"></a>partitionCount

Soovite oma sündmuse keskuses sektsioonid arv.

```
"partitionCount":{
    "type":"int",
    "defaultValue":2,
    "minValue":2,
    "maxValue":32,
    "metadata":{
        "description":"Number of partitions chosen"
    }
 }
```

### <a name="archiveenabled"></a>archiveEnabled

Klõpsake oma sündmuse jaoturi arhiivi lubamine.

```
"archiveEnabled":{
    "type":"string",
    "defaultValue":"true",
    "allowedValues": [
    "false",
    "true"],
    "metadata":{
        "description":"Enable or disable the Archive for your Event Hub"
    }
 }
```
### <a name="archiveencodingformat"></a>archiveEncodingFormat

Kodeerimise vorming saate määrata serialiseerida sündmuse andmed.

```
"archiveEncodingFormat":{
    "type":"string",
    "defaultValue":"Avro",
    "allowedValues":[
    "Avro"],
    "metadata":{
        "description":"The encoding format Archive serializes the EventData"
    }
}
```

### <a name="archivetime"></a>archiveTime

Kust arhiivi algab Azure'i bloobimälu andmete arhiveerimise ajavahemik.

```
"archiveTime":{
    "type":"int",
    "defaultValue":300,
    "minValue":60,
    "maxValue":900,
    "metadata":{
         "description":"the time window in seconds for the archival"
    }
}
```

### <a name="archivesize"></a>archiveSize

Kust arhiivi algab Azure'i bloobimälu andmete arhiveerimise suurus intervalli.

```
"archiveSize":{
    "type":"int",
    "defaultValue":314572800,
    "minValue":10485760,
    "maxValue":524288000,
    "metadata":{
        "description":"the size window in bytes for archival"
    }
}
```

### <a name="destinationstorageaccountresourceid"></a>destinationStorageAccountResourceId

Arhiivi jaoks on vaja salvestusruumi konto ressursi id lubamiseks soovitud Azure'i salvestusruumi Arhiiv.

```
 "destinationStorageAccountResourceId":{
    "type":"string",
    "metadata":{
        "description":"Your existing storage Account resource id where you want the blobs be archived"
    }
 }
```

### <a name="blobcontainername"></a>blobContainerName

Bloobimälu ümbris, kuhu soovite sündmuse andmete arhiivida.

```
 "blobContainerName":{
    "type":"string",
    "metadata":{
        "description":"Your existing storage Container that you want the blobs archived in"
    }
}
```


### <a name="apiversion"></a>apiVersion

Malli API versiooni.

```
 "apiVersion":{  
    "type":"string",
    "defaultValue":"2015-08-01",
    "metadata":{  
        "description":"ApiVersion used by the template"
    }
 }
```

## <a name="resources-to-deploy"></a>Juurutamiseks ressursid

Mõni sündmus jaoturi nimeruum tüüpi **EventHubs**, loob ja lubab arhiivi.

```
"resources":[  
      {  
         "apiVersion":"[variables('ehVersion')]",
         "name":"[parameters('eventHubNamespaceName')]",
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
                  "[concat('Microsoft.EventHub/namespaces/', parameters('eventHubNamespaceName'))]"
               ],
               "properties":{  
                  "path":"[parameters('eventHubName')]",
                  "MessageRetentionInDays":"[parameters('messageRetentionInDays')]",
                  "PartitionCount":"[parameters('partitionCount')]",
                  "ArchiveDescription":{
                        "enabled":"[parameters('archiveEnabled')]",
                        "encoding":"[parameters('archiveEncodingFormat')]",
                        "intervalInSeconds":"[parameters('archiveTime')]",
                        "sizeLimitInBytes":"[parameters('archiveSize')]",
                        "destination":{
                            "name":"EventHubArchive.AzureBlockBlob",
                            "properties":{
                                "StorageAccountResourceId":"[parameters('destinationStorageAccountResourceId')]",
                                "BlobContainer":"[parameters('blobContainerName')]"
                            }
                        } 
                  }
                  
               }
               
            }
         ]
      }
   ]
```

## <a name="commands-to-run-deployment"></a>Käskude käivitamiseks juurutamine

[AZURE.INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a>PowerShelli

```
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-eventhubs-create-namespace-and-enable-archive/azuredeploy.json
```

## <a name="azure-cli"></a>Azure'i CLI

```
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri [https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-eventhubs-create-namespace-and-enable-archive/azuredeploy.json][]
```

[Azure'i ressursihaldur mallide koostamine]: ../resource-group-authoring-templates.md
[Azure'i Kiirjuhend Mallid]:  https://azure.microsoft.com/documentation/templates/?term=event+hubs
[Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
[Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
[Event Hub and consumer group template]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-eventhubs-create-namespace-and-enable-archive/
[Azure'i ressursse nimetamise põhimõtted]: https://azure.microsoft.com/en-us/documentation/articles/guidance-naming-conventions/
[Sündmuse jaoturi ja luba arhiivi Mall]:https://github.com/Azure/azure-quickstart-templates/tree/master/201-eventhubs-create-namespace-and-enable-archive
