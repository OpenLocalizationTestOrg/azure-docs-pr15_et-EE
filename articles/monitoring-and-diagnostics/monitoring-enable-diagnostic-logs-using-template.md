<properties
    pageTitle="Luba automaatne Diagnostikasätete ressursihaldur malli abil | Microsoft Azure'i"
    description="Saate teada, kuidas luua diagnostikasätete, mis võimaldab teil oma diagnostikalogid, et sündmuse jaoturi voogesituseks või neid talletada salvestusruum konto ressursihaldur malli abil."
    authors="johnkemnetz"
    manager="rboucher"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="johnkem"/>

# <a name="automatically-enable-diagnostic-settings-at-resource-creation-using-a-resource-manager-template"></a>Luba automaatselt Diagnostikasätete ressursside loomisel ressursihaldur malli abil
Selles artiklis näitame kasutamist mõni [Azure ressursihaldur malli](../resource-group-authoring-templates.md) diagnostika sätete konfigureerimiseks ressursi loomisel. See võimaldab teil automaatseks käivitamiseks Diagnostikalogid ja soovite sündmuse jaoturi, arhiivimine need salvestusruumi konto või saata neile Log Analytics ressursi loomisel mõõdikute voogesitamine.

Diagnostikalogide ressursihaldur malli abil, mis võimaldab meetod sõltub ressursi tüüp.

- **Mitte-Arvuta** ressursid (nt võrgu turberühmad, loogika rakendused, automatiseerimine) saate kasutada [selles artiklis kirjeldatud Diagnostikasätete](./monitoring-overview-of-diagnostic-logs.md#diagnostic-settings).
- **Arvutage** [Selles artiklis kirjeldatud WAD/POISI konfiguratsioonifail](../vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines.md)kasutada ressursse (WAD/POISI-põhine).

Selles artiklis kirjeldame, kuidas konfigureerida diagnostika, tehes ühte järgmistest.

Põhitoimingud on järgmised:

1. Malli loomine JSON-failina, mis kirjeldab, kuidas luua ressurssi ja diagnostika lubamine.
2. [Deploy Mall, mis tahes juurutamise meetodi abil](../resource-group-template-deploy.md).

Allpool anname mallifail JSON-Arvuta ja Arvuta ressursside loomiseks peate näide.

## <a name="non-compute-resource-template"></a>Mitte-Arvuta ressursi Mall
Arvuta ressursid, peate tegema mõlemat:

1. Parameetrite lisamisel parameetrite bloobimälu salvestusruumikonto nimi, teenuse siini reegli ID ja/või OMS Log Analytics tööruumi ID (arhiivimine Diagnostikalogide, salvestusruumi konto voogesituse logid sündmuse jaoturi ja/või logid saata Log Analytics lubamine).

    ```json
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Name of the Storage Account in which Diagnostic Logs should be saved."
      }
    },
    "serviceBusRuleId": {
      "type": "string",
      "metadata": {
        "description": "Service Bus Rule Id for the Service Bus Namespace in which the Event Hub should be created or streamed to."
      }
    },
    "workspaceId":{
      "type": "string",
      "metadata": {
        "description": "Log Analytics workspace ID for the Log Analytics workspace to which logs will be sent."
      }
    }
    ```
2. Ressurss, mille jaoks soovite lubada Diagnostikalogid massiivis ressursid lisada ressursi tüüpi `[resource namespace]/providers/diagnosticSettings`.

    ```json
    "resources": [
      {
        "type": "providers/diagnosticSettings",
        "name": "Microsoft.Insights/service",
        "dependsOn": [
          "[/*resource Id for which Diagnostic Logs will be enabled>*/]"
        ],
        "apiVersion": "2015-07-01",
        "properties": {
          "storageAccountId": "[resourceId('Microsoft.Storage/storageAccounts', parameters('storageAccountName'))]",
          "serviceBusRuleId": "[parameters('serviceBusRuleId')]",
          "workspaceId": "[parameters('workspaceId')]",
          "logs": [ 
            {
              "category": "/* log category name */",
              "enabled": true,
              "retentionPolicy": {
                "days": 0,
                "enabled": false
              }
            }
          ]
        }
      }
    ]
    ```

Atribuutide bloobimälu diagnostika sätte järgib [selles artiklis kirjeldatud vorming](https://msdn.microsoft.com/library/azure/dn931931.aspx).

Siin on täielik näide, mis loob võrgu turberühma ja lülitab streaming sündmuse jaoturi ja salvestusruumi salvestusruumi konto.

```json

{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "nsgName": {
      "type": "string",
      "metadata": {
        "description": "Name of the NSG that will be created."
      }
    },
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Name of the Storage Account in which Diagnostic Logs should be saved."
      }
    },
    "serviceBusRuleId": {
      "type": "string",
      "metadata": {
        "description": "Service Bus Rule Id for the Service Bus Namespace in which the Event Hub should be created or streamed to."
      }
    },
    "workspaceId": {
      "type": "string",
      "metadata": {
        "description": "Log Analytics workspace ID for the Log Analytics workspace to which logs will be sent."
      }
    }
  },
  "variables": {},
  "resources": [
    {
      "type": "Microsoft.Network/networkSecurityGroups",
      "name": "[parameters('nsgName')]",
      "apiVersion": "2016-03-30",
      "location": "westus",
      "properties": {
        "securityRules": []
      },
      "resources": [
        {
          "type": "providers/diagnosticSettings",
          "name": "Microsoft.Insights/service",
          "dependsOn": [
            "[resourceId('Microsoft.Network/networkSecurityGroups', parameters('nsgName'))]"
          ],
          "apiVersion": "2015-07-01",
          "properties": {
            "storageAccountId": "[resourceId('Microsoft.Storage/storageAccounts', parameters('storageAccountName'))]",
            "serviceBusRuleId": "[parameters('serviceBusRuleId')]",
            "workspaceId": "[parameters('workspaceId')]",
            "logs": [
              {
                "category": "NetworkSecurityGroupEvent",
                "enabled": true,
                "retentionPolicy": {
                  "days": 0,
                  "enabled": false
                }
              },
              {
                "category": "NetworkSecurityGroupRuleCounter",
                "enabled": true,
                "retentionPolicy": {
                  "days": 0,
                  "enabled": false
                }
              }
            ]
          }
        }
      ],
      "dependsOn": []
    }
  ]
}

```

## <a name="compute-resource-template"></a>Arvutage ressursi Mall
Luba diagnostika Arvuta ressurssi, näiteks virtuaalse masina või teenuse struktuuri kobar, peate:

1. Lisage Azure diagnostika laiend VM ressursside määratlemine.
2. Määrake salvestusruumi konto ja/või sündmuse jaoturi parameetrina.
3. Saate lisada atribuudi XMLCfg, pääseks kõik XML-i märgid õigesti WADCfg XML-faili sisu.

> [AZURE.WARNING] See viimane toiming võib olla keeruline õige. Näiteks, et muutujad, mis on põgenenud ja õigesti vormindatud diagnostika konfiguratsioon skeemi kinnitanud, [lugege artiklit](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md#diagnostics-configuration-variables) .

Kogu protsessi, sh näidised, on kirjeldatud [selles dokumendis](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md).


## <a name="next-steps"></a>Järgmised sammud
- [Lisateavet leiate teemast Azure Diagnostikalogid](./monitoring-overview-of-diagnostic-logs.md)
- [Voona Azure'i Diagnostikalogid sündmuse jaoturi abil](./monitoring-stream-diagnostic-logs-to-event-hubs.md)
