<properties
   pageTitle="Ressursihaldur malli linkimise ressursid | Microsoft Azure'i"
   description="Näitab ressursihaldur skeemi seoseid seotud ressursside malli kasutamise kohta."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/05/2016"
   ms.author="tomfitz"/>

# <a name="resource-links-template-schema"></a>Ressursside lingid malli skeem

Loob kaks ressursid seos. Lingi rakendatakse tuntud allika ressursi ressursi. Lingi teise ressursi nimetatakse target ressursi.

## <a name="schema-format"></a>Skeemi vorming

Lingi loomiseks lisage järgmised skeemi ressursid jaotisele malli.
    
    {
        "type": enum,
        "apiVersion": "2015-01-01",
        "name": string,
        "dependsOn": [ array values ],
        "properties":
        {
            "targetId": string,
            "notes": string
        }
    }



## <a name="values"></a>Väärtused

Järgmises tabelis on kirjeldatud peate määrama skeemi väärtused.

| Nimi | Väärtus |
| ---- | ---- |
| tüüp | Loetelu<br />Nõutav<br />**{nimeruum} / {tüüp} / pakkujate/lingid**<br /><br />Ressursi tüüp loomiseks. {Nimeruumi} {tüüp} väärtused viitavad allika ressursi pakkuja nimeruum ja ressursside tüüp. |
| apiVersion | Loetelu<br />Nõutav<br />**2015-01-01**<br /><br />Ressursi loomiseks kasutatav API versioon. |  
| Nimi | String<br />Nõutav<br />**{resouce}/Microsoft.Resources/{linkname}**<br /> kuni 64 märgist ja ei tohi sisaldada <>; %, &,?, või mis tahes juhtida märgid.<br /><br />A väärtus, et specifes nii andmeallika ressursi nimi ja lingi nimi. |
| ei leia dependsOn | Massiiv<br />Valikuline.<br />Komaga eraldatud loend ressursi nimed või ressursside ainulaadsed ID-d.<br /><br />Saidikogumi ressursside sõltub selle lingi. Kui ühendate ressursside on juurutatud sama malli, lisage nende ressursside nimed katsetama esmalt tagamiseks – see element. | 
| atribuudid | Objekti<br />Nõutav<br />[objekti atribuudid](#properties)<br /><br />Objekti, mis tuvastab ressursi linkimiseks ja märkmeid lingi kohta. |  

<a id="properties" />
### <a name="properties-object"></a>objekti atribuudid

| Nimi | Väärtus |
| ------- | ---- |
| targetId | String<br />Nõutav<br />**{ressursi id}**<br /><br />Identifikaator target ressursi linkida. |
| märkmete | String<br />Valikuline.<br />kuni 512 märki<br /><br />Lukusta kirjeldus. |


## <a name="how-to-use-the-link-resource"></a>Link ressursi kasutamise kohta

Saate rakendada seos kahe ressursse, kui ressursid on sõltuvus, mis pärast juurutamine. Näiteks rakendus võib andmebaasiga ühenduse loomiseks muu ressursi rühma. Saate määratleda selle sõltuvus lingi loomine rakenduses andmebaasi. Lingid, mis võimaldavad teil dokumendi seos kahe ressursid. Hiljem saab keegi teie asutuses või päringud ressursi teada, kuidas ressursi töötavad koos muude ressursside lingid.

Kõigi lingitud ressursid peab kuuluma sama tellimus. Iga ressursi saab linkida 50 muud ressursid. Kui mõni lingitud ressursid on kustutatud või teisaldatud, link omanik puhastamiseks ülejäänud link.

Töötada läbi ülejäänud lingid, lugege teemat [Lingitud ressursse](https://msdn.microsoft.com/library/azure/mt238499.aspx).

Azure'i PowerShelli järgmise käsu abil saate vaadata kõik lingid teie tellimus. Saate sisestada muude parameetrite tulemeid piiritleda.

    Get-AzureRmResource -ResourceType Microsoft.Resources/links -isCollection -ResourceGroupName <YourResourceGroupName>

## <a name="examples"></a>Näited

Järgmises näites kehtib kirjutuskaitstud Lukusta web appi.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "hostingPlanName": {
                "type": "string"
            }
        },
        "variables": {
            "siteName": "[concat('site',uniqueString(resourceGroup().id))]"
        },
        "resources": [
            {
                "apiVersion": "2015-08-01",
                "type": "Microsoft.Web/serverfarms",
                "name": "[parameters('hostingPlanName')]",
                "location": "[resourceGroup().location]",
                "sku": {
                    "tier": "Free",
                    "name": "f1",
                    "capacity": 0
                },
                "properties": {
                    "numberOfWorkers": 1
                }
            },
            {
                "apiVersion": "2015-08-01",
                "name": "[variables('siteName')]",
                "type": "Microsoft.Web/sites",
                "location": "[resourceGroup().location]",
                "dependsOn": [ "[parameters('hostingPlanName')]" ],
                "properties": {
                    "serverFarmId": "[parameters('hostingPlanName')]"
                }
            },
            {
                "type": "Microsoft.Web/sites/providers/links",
                "apiVersion": "2015-01-01",
                "name": "[concat(variables('siteName'),'/Microsoft.Resources/SiteToStorage')]",
                "dependsOn": [ "[variables('siteName')]" ],
                "properties": {
                    "targetId": "[resourceId('Microsoft.Storage/storageAccounts','storagecontoso')]",
                    "notes": "This web site uses the storage account to store user information."
                }
            }
        ],
        "outputs": {}
    }

## <a name="quickstart-templates"></a>Kiirjuhend Mallid

Järgmised Mallid Kiirjuhend juurutada ressursid link.

- [Loogika rakenduse järjekorda Teavita](https://azure.microsoft.com/documentation/templates/201-alert-to-queue-with-logic-app)
- [Teatiste abil vaikne loogika rakendusega](https://azure.microsoft.com/documentation/templates/201-alert-to-slack-with-logic-app)
- [Mõne olemasoleva lüüsi rakenduse API ettevalmistamine](https://azure.microsoft.com/documentation/templates/201-api-app-gateway-existing)
- [Uus lüüs rakenduse API ettevalmistamine](https://azure.microsoft.com/documentation/templates/201-api-app-gateway-new)
- [Malli abil loogika rakenduse pluss API rakenduse loomine](https://azure.microsoft.com/documentation/templates/201-logic-app-api-app-create)
- [Loogika rakendus, mis saadab sõnumi teksti, kui teatise tule](https://azure.microsoft.com/documentation/templates/201-alert-to-text-message-with-logic-app)


## <a name="next-steps"></a>Järgmised sammud

- Malli struktuuri kohta leiate teavet teemast [loome Azure'i ressursihaldur Mallid](resource-group-authoring-templates.md).
