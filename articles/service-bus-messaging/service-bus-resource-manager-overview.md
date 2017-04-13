<properties
    pageTitle="Luua teenuse siini ressursid Azure'i ressursihaldur mallide kasutamine | Microsoft Azure'i"
    description="Azure'i ressursihaldur mallide abil automatiseerida teenuse siini ressursside loomine"
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
    ms.author="sethm"/>

# <a name="create-service-bus-resources-using-azure-resource-manager-templates"></a>Looge teenuse siini ressursse, mis on Azure ressursihaldur mallide kasutamine

Selles artiklis kirjeldatakse, kuidas luua ja juurutada Azure ressursihaldur Mallid, PowerShelli ja siini Service ressursi pakkuja teenuse siini ja sündmuse jaoturi ressursid.

Azure'i ressursihaldur Mallid, mis aitavad teil määratleda ressursid lahenduse juurutamine ja määrake parameetrite ja muutujate, mis võimaldavad teil viibite väärtuste sisestamiseks. Mall sisaldab JSON ja avaldised, mille abil saate koostada oma juurutamiseks väärtused. Vaadake lisateavet Azure ressursihaldur Mallid ja arutelu malli vormingu [loome Azure'i ressursihaldur Mallid](../resource-group-authoring-templates.md). 

>[AZURE.NOTE] Selle artikli näited Azure'i ressursihaldur abil saate luua teenuse siini nimeruum ja sõnumside üksus (järjekorra). Malli näiteid leiate [Azure'i Kiirjuhend kuukalendreid][] ja otsing "Teenuse siini."

## <a name="service-bus-and-event-hubs-resource-manager-templates"></a>Teenuse siini ja sündmuse jaoturi ressursihaldur Mallid

Need teenuse siini ja sündmuse jaoturi Azure'i ressursihaldur Mallid on saadaval allalaadimiseks ja juurutamine. Klõpsake järgmisi linke igale kirjele, malle github linkidega üksikasjad: 

- [Teenuse siini nimeruum loomine](service-bus-resource-manager-namespace.md)
- [Luua teenuse siini nimeruum järjekord](service-bus-resource-manager-namespace-queue.md)
- [Teenuse siini nimeruum teema ja tellimuse loomiseks](service-bus-resource-manager-namespace-topic.md)
- [Teenuse siini nimeruum kuhjuda ja luba reegli loomine](service-bus-resource-manager-namespace-auth-rule.md)
- [Sündmuse jaoturi ja tarbija rühmaga mõni sündmus jaoturi nimeruum loomine](../event-hubs/event-hubs-resource-manager-namespace-event-hub.md)

## <a name="deploy-with-powershell"></a>PowerShelliga juurutamine

Järgnevalt kirjeldatakse, kuidas PowerShelli abil saate juurutada Azure ressursihaldur Mall, mis loob **Standard** taseme teenuse siini nimeruumi ja selle nimeruumi jooksul järjekorda. Selles näites on [loomine teenuse siini nimeruum koos järjekorda](https://github.com/Azure/azure-quickstart-templates/tree/master/201-servicebus-create-queue) malli põhjal. Ligikaudse töövoog on järgmine:

1. PowerShelli installimine.
2. Mall ja (soovi korral) parameetri faili loomine.
2. PowerShelli, logige sisse oma Azure'i kontosse.
3. Kui üks olemas, saate luua uue ressursirühma.
4. Testige juurutamise.
5. Soovi korral määrake juurutamise režiimi.
6. Malli.

Lisateavet juurutamine Azure'i ressursihaldur Mallid, leiate [Deploy ressurssidest Azure'i ressursihaldur mallide abil][].

### <a name="install-powershell"></a>PowerShelli installimine

Installige Azure'i PowerShelli, järgides juhiseid, [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md).

### <a name="create-a-template"></a>Malli loomine

Kopeerige Mall [201-servicebus loomine – järjekord](https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-queue/azuredeploy.json) GitHub klooni või tehke järgmist.

```
{
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "serviceBusNamespaceName": {
            "type": "string",
            "metadata": {
                "description": "Name of the Service Bus namespace"
            }
        },
        "serviceBusQueueName": {
            "type": "string",
            "metadata": {
                "description": "Name of the Queue"
            }
        },
        "serviceBusApiVersion": {
            "type": "string",
            "defaultValue": "2015-08-01",
            "metadata": {
                "description": "Service Bus ApiVersion used by the template"
            }
        }
    },
    "variables": {
        "location": "[resourceGroup().location]",
        "sbVersion": "[parameters('serviceBusApiVersion')]",
        "defaultSASKeyName": "RootManageSharedAccessKey",
        "authRuleResourceId": "[resourceId('Microsoft.ServiceBus/namespaces/authorizationRules', parameters('serviceBusNamespaceName'), variables('defaultSASKeyName'))]"
    },
    "resources": [{
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
            "name": "[parameters('serviceBusQueueName')]",
            "type": "Queues",
            "dependsOn": [
                "[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"
            ],
            "properties": {
                "path": "[parameters('serviceBusQueueName')]"
            }
        }]
    }],
    "outputs": {
        "NamespaceConnectionString": {
            "type": "string",
            "value": "[listkeys(variables('authRuleResourceId'), variables('sbVersion')).primaryConnectionString]"
        },
        "SharedAccessPolicyPrimaryKey": {
            "type": "string",
            "value": "[listkeys(variables('authRuleResourceId'), variables('sbVersion')).primaryKey]"
        }
    }
}
```

### <a name="create-a-parameters-file-optional"></a>Parameetrite faili loomine (valikuline)

Kasutada valikuliste parameetrite faili, [201-servicebus loomine – järjekord](https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-queue/azuredeploy.parameters.json) -failist koopia. Asendage väärtus `serviceBusNamespaceName` teenuse siini nimeruumi soovite luua selle juurutuse ja asendada väärtus nimega `serviceBusQueueName` nimega järjekorra soovite luua. 

```
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "serviceBusNamespaceName": {
            "value": "<myNamespaceName>"
        },
        "serviceBusQueueName": {
            "value": "<myQueueName>"
        },
        "serviceBusApiVersion": {
            "value": "2015-08-01"
        }
    }
}
```

Lisateavet leiate teemast [parameetri fail](../resource-group-template-deploy.md#parameter-file) .

### <a name="log-in-to-azure-and-set-the-azure-subscription"></a>Azure'i sisse logida ja Azure tellimuse seadmine

Alates PowerShelli käsuviibas, käivitage järgmine käsk:

```
Login-AzureRmAccount
```

Teil palutakse Azure'i kontosse sisse logida. Kui olete sisse loginud, käivitage järgmine käsk saadaval tellimuste kuvamiseks.

```
Get-AzureRMSubscription
```

See käsk tagastab loendi saadaval Azure'i tellimused. Valige tellimus praeguse seansi kohta, käivitage järgmine käsk. Asendage `<YourSubscriptionId>` koos GUID Azure'i tellimus, mida soovite kasutada.

```
Set-AzureRmContext -SubscriptionID <YourSubscriptionId>
```

### <a name="set-the-resource-group"></a>Ressursirühma seadmine

Kui teil pole olemasoleva ressursirühma, saate luua uue ressursirühma käsu **New-AzureRmResourceGroup** . Sisestage nimi ja ressursirühm asukohta, mida soovite kasutada. Näiteks:

```
New-AzureRmResourceGroup -Name MyDemoRG -Location "West US"
```

Kui õnnestub, kuvatakse uue ressursirühma kokkuvõte.

```
ResourceGroupName : MyDemoRG
Location          : westus
ProvisioningState : Succeeded
Tags              :
ResourceId        : /subscriptions/<GUID>/resourceGroups/MyDemoRG
```

### <a name="test-the-deployment"></a>Juurutamise

Kinnitage oma juurutuse käivitades funktsiooni `Test-AzureRmResourceGroupDeployment` cmdlet-käsk. Juurutamise testimisel sisestada täpselt nii, nagu teeksite juurutamise täitmisel parameetrid.

```
Test-AzureRmResourceGroupDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json
```

### <a name="create-the-deployment"></a>Juurutamise loomine

Uue juurutamise loomiseks käivitage selle `New-AzureRmResourceGroupDeployment` käsk ja sisestage vastava viiba vajalik parameetrid. Parameetrid lisage juurutamise malli faili nimi oma ressursirühm ja tee või URL-i nimi. Kui parameeter **režiimis** pole määratud, kasutatakse **Inkrementmuutus** vaikeväärtus. Lisateabe saamiseks vt [Inkrementmuutus ja täielik juurutuste](../resource-group-template-deploy.md#incremental-and-complete-deployments).

Järgmine käsk palub teil kolm parameetrite PowerShelli aknas:

```
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json
```

Selle asemel faili parameetrite määramiseks kasutage järgmine käsk.

```
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json -TemplateParameterFile <path to parameters file>\azuredeploy.parameters.json
```

Tekstisisene parameetrid saate kasutada ka juurutuse cmdlet käivitamisel. Käsk on järgmine:

```
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json -parameterName "parameterValue"
```

[Täieliku](../resource-group-template-deploy.md#incremental-and-complete-deployments) juurutamise käivitamiseks määrata **režiimi** parameetri **lõpuleviimine**.

```
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -Mode Complete -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json 
```

### <a name="verify-the-deployment"></a>Juurutamise kontrollimine

Kui ressursse on juurutatud edukalt, kuvatakse juurutamise kokkuvõtte PowerShelli aknas:

```
DeploymentName    : MyDemoDeployment
ResourceGroupName : MyDemoRG
ProvisioningState : Succeeded
Timestamp         : 4/19/2016 10:38:30 PM
Mode              : Incremental
TemplateLink      :
Parameters        :
                    Name             Type                       Value
                    ===============  =========================  ==========
                    serviceBusNamespaceName  String             <namespaceName>
                    serviceBusQueueName  String                 <queueName>
                    serviceBusApiVersion  String                2015-08-01

```

## <a name="next-steps"></a>Järgmised sammud

Nüüd näete, tavaline töövoo ja käsud on Azure ressursihaldur malli kasutamise kohta. Üksikasjalikumat teavet leiate järgmistest linkidest:

- [Azure'i ressursihaldur ülevaade][]
- [Azure'i ressursihaldur malle ressursse juurutamine][]
- [Mallide koostamine](../resource-group-authoring-templates.md)


[Azure'i ressursihaldur ülevaade]: ../resource-group-overview.md
[Azure'i ressursihaldur malle ressursid juurutamine]: ../resource-group-template-deploy.md
[Azure'i Kiirjuhend kuukalendreid]: https://azure.microsoft.com/documentation/templates/?term=service+bus