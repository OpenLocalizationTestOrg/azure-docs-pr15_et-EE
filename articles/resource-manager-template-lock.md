<properties
   pageTitle="Ressursihaldur malli ressursi lukud | Microsoft Azure'i"
   description="Näitab ressursihaldur skeemi ressursi lukud kaudu malli kasutamise kohta."
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
   ms.date="10/03/2016"
   ms.author="tomfitz"/>

# <a name="resource-locks-template-schema"></a>Ressursi lukud malli skeem

Loob ressursi ja selle lapse ressursid.

## <a name="schema-format"></a>Skeemi vorming

Lukustus loomiseks lisage järgmised skeemi ressursid jaotisele malli.
    
    {
        "type": enum,
        "apiVersion": "2015-01-01",
        "name": string,
        "dependsOn": [ array values ],
        "properties":
        {
            "level": enum,
            "notes": string
        }
    }



## <a name="values"></a>Väärtused

Järgmises tabelis on kirjeldatud peate määrama skeemi väärtused.

| Nimi | Nõutav | Kirjeldus |
| ---- | -------- | ----------- |
| tüüp | Jah | Ressursi tüüp loomiseks.<br /><br />Ressursid:<br />**{nimeruum} / {tüüp} / pakkujate/lukud**<br /><br/>Ressursi rühmade:<br />**Microsoft.Authorization/locks** |
| apiVersion | Jah | Ressursi loomiseks kasutatav API versioon.<br /><br />Kasutage.<br />**2015-01-01**<br /><br /> |
| Nimi | Jah | Väärtus, mis määrab ressurssi lukustada nii lukustus nimi. Võib olla kuni 64 märgi ja ei tohi sisaldada <>; %, &,?, või mis tahes juhtida märgid.<br /><br />Ressursid:<br />**{resource}/Microsoft.Authorization/{lockname}**<br /><br />Ressursi rühmade:<br />**{lockname}** |
| ei leia dependsOn | Ei | Komaga eraldatud loend ressursi nimed või ressursside ainulaadsed ID-d.<br /><br />Ressursside sõltub selle Lukusta kogum. Kui teil on lukustamine ressursi on juurutatud sama malli, kaasake – see element tagada ressursi juurutatakse esmalt selle ressursi nimi. | 
| atribuudid | Jah | Objekti, mis tüüpi lukustamine ja lukustus märkmeid tuvastab.<br /><br />[Objekti atribuutide](#properties-object)kuvamine |  

### <a name="properties-object"></a>objekti atribuudid

| Nimi | Nõutav | Kirjeldus |
| ---- | -------- | ----------- |
| tase   | Jah | Lukusta rakendamiseks ulatus tüüp.<br /><br />**CannotDelete** – kasutajad saavad ressursi muuta, kuid mitte kustutada.<br />**Kirjutuskaitstud** - kasutajad saavad lugeda ressurss, kuid neid ei saa kustutada või mis tahes toiminguid sooritada. |
| märkmete   | Ei | Lukusta kirjeldus. Võib olla kuni 512 märki. |


## <a name="how-to-use-the-lock-resource"></a>Lukusta ressursi kasutamise kohta

Saate lisada selle ressursi malli määratud toimingud ressursi vältimiseks. Lukusta kehtib kõik kasutajad ja rühmad.

Loomine ja kustutamine halduse lukud, peab teil olema juurdepääs **Microsoft.Authorization/** * või * *Microsoft.Authorization/locks/* ** toimingud. Sisseehitatud rolle, ainult **omanik** ja **kasutajale juurdepääs administraatori ** antakse need toimingud. Rollipõhine juurdepääsu reguleerimine kohta leiate teemast [Azure Rollipõhine juurdepääsu reguleerimine](./active-directory/role-based-access-control-configure.md).

Lukusta rakendatakse määratud ressurss ja mis tahes lapse ressursid.

Saate eemaldada lukustamine, PowerShelli käsu **Eemalda-AzureRmResourceLock** või selle [kustutamine toimingut](https://msdn.microsoft.com/library/azure/mt204562.aspx) REST API.

## <a name="examples"></a>Näited

Järgmises näites kehtib web Appis ei saa kustutada lukustamine.

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
                "name": "[variables('siteName')]",
                "type": "Microsoft.Web/sites",
                "location": "[resourceGroup().location]",
                "properties": {
                    "serverFarmId": "[parameters('hostingPlanName')]"
                },
            },
            {
                "type": "Microsoft.Web/sites/providers/locks",
                "apiVersion": "2015-01-01",
                "name": "[concat(variables('siteName'),'/Microsoft.Authorization/MySiteLock')]",
                "dependsOn": [ "[variables('siteName')]" ],
                "properties":
                {
                    "level": "CannotDelete",
                    "notes": "my notes"
                }
             }
        ],
        "outputs": {}
    }

Järgmine näide kehtib ei saa kustutada Lukusta ressursirühma.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {},
        "variables": {},
        "resources": [
            {
                "type": "Microsoft.Authorization/locks",
                "apiVersion": "2015-01-01",
                "name": "MyGroupLock",
                "properties":
                {
                    "level": "CannotDelete",
                    "notes": "my notes"
                }
            }
        ],
        "outputs": {}
    }

## <a name="next-steps"></a>Järgmised sammud

- Malli struktuuri kohta leiate teavet teemast [loome Azure'i ressursihaldur Mallid](resource-group-authoring-templates.md).
- Lukud kohta leiate lisateavet teemast [Lock ressursse Azure'i ressursihaldur](resource-group-lock-resources.md).
