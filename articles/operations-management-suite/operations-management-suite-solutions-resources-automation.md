<properties
   pageTitle="Automaatika ressursside OMS lahendused | Microsoft Azure'i"
   description="Lahenduste OMS on üldjuhul tegevusraamatud Azure automatiseerimine nt jälgimisega seotud andmete kogumise ja töötlemise protsesside automatiseerimiseks.  Selles artiklis kirjeldatakse, kuidas lisada lahenduse tegevusraamatud ja nende seotud ressursid."
   services="operations-management-suite"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/19/2016"
   ms.author="bwren" />

# <a name="automation-resources-in-oms-solutions-preview"></a>Automaatika ressursside OMS lahenduste (eelvaade)

>[AZURE.NOTE]See on esialgse dokumentatsioonist loomine lahenduste OMS, mis on praegu eelvaade. Mis tahes allpool kirjeldatud skeemi on muutuda.   

[OMS lahenduste haldus](operations-management-suite-solutions.md) on üldjuhul tegevusraamatud Azure automatiseerimine nt jälgimisega seotud andmete kogumise ja töötlemise protsesside automatiseerimiseks.  Lisaks tegevusraamatud, sisaldab automatiseerimise kontod vara, näiteks muutujate ja ajakava lahendus tegevusraamatud, kasutatakse toetavad.  Selles artiklis kirjeldatakse, kuidas lisada lahenduse tegevusraamatud ja nende seotud ressursid.

>[AZURE.NOTE]Selle artikli näidised kasutada parameetrite ja muutujate, mis on nõutav või levinud lahendusi ja kirjeldatud [loomine lahenduste toimingud halduse komplekti (OMS)](operations-management-suite-solutions-creating.md) 


## <a name="prerequisites"></a>Eeltingimused
Selles artiklis eeldatakse, et olete juba tuttav kuidas [luua halduse lahenduse](operations-management-suite-solutions-creating.md) ja struktuuri lahenduse faili.

## <a name="samples"></a>Näidised
Saate ressursihaldur Näidismallid automatiseerimise ressursid [GitHub Kiirjuhend Mallid](https://github.com/azureautomation/automation-packs/tree/master/101-sample-automation-resource-templates).

## <a name="automation-account"></a>Automaatika konto
Kõigi ressursside Azure automatiseerimine sisalduvad [automatiseerimise konto](../automation/automation-security-overview.md#automation-account-overview).  Nagu on kirjeldatud [OMS tööruumi ja automatiseerimise konto](operations-management-suite-solutions-creating.md#oms-workspace-and-automation-account) automatiseerimise konto ei sisalda halduse lahenduse, kuid kuuluma lahendus on installitud.  Kui see pole saadaval, siis lahenduse installimine nurjub.

Nende automatiseerimine konto nimi on iga automatiseerimise ressursi nimi.  Seda parameetrit **account_name** , nagu järgmises näites käitusjuhendi ressursi lahendus.
    
    "name": "[concat(parameters('accountName'), '/MyRunbook'))]"


## <a name="runbooks"></a>Tegevusraamatud
[Azure'i automaatika käitusjuhendi](../automation/automation-runbook-types.md) ressursid **Microsoft.Automation/automationAccounts/runbooks** tüüp ja atribuudid on järgmises tabelis.

| Atribuut | Kirjeldus |
|:--|:--|
| runbookType | Saate määrata, millist tüüpi käitusjuhendi. <br><br> Skripti - PowerShelli skripti <br>PowerShelli - PowerShelli töövoo <br> GraphPowerShell – graafiline PowerShelli skripti käitusjuhendi <br> GraphPowerShellWorkflow – graafiline PowerShelli töövoo käitusjuhendi   |
| logProgress | Saate määrata, kas [edenemise kirjete](../automation/automation-runbook-output-and-messages.md) loomise jaoks käitusjuhendi. |
| logVerbose  | Saate määrata, kas [Paljusõnaline kirjete](../automation/automation-runbook-output-and-messages.md) loomise jaoks käitusjuhendi. |
| kirjeldus | Valikuline kirjeldus käitusjuhendi. |
| publishContentLink | Saate määrata käitusjuhendi sisu. <br><br>URI - Uri käitusjuhendi sisu.  Sellest saab .ps1 faili PowerShelli ja skripti tegevusraamatud ja eksporditud graafilise käitusjuhendi faili jaoks graafik käitusjuhendi.  <br> versiooni - käitusjuhendi oma jälgimise versiooni. |

Allpool on näiteks käitusjuhendi ressursi.  Selles näites toob käitusjuhendi [PowerShelli Galerii](https://www.powershellgallery.com).

    "name": "[concat(parameters('accountName'), '/Start-AzureV2VMs'))]",
    "type": "Microsoft.Automation/automationAccounts/runbooks",
    "apiVersion": "[variables('AutomationApiVersion')]",
    "location": "[parameters('regionId')]",
    "dependsOn": [
        "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('startVmsRunbookName'))]"
    ],
    "tags": { },
    "properties": {
        "runbookType": "PowerShell",
        "logProgress": "true",
        "logVerbose": "true",
        "description": "",
        "publishContentLink": {
            "uri": "https://www.powershellgallery.com/api/v2/items/psscript/package/Update-ModulesInAutomationToLatestVersion/1.0/Start-AzureV2VMs.ps1",
            "version": "1.0"
        }
    }

### <a name="starting-a-runbook"></a>Mõne käitusjuhendi käivitamine
On kaks võimalust alustamiseks on käitusjuhendi haldamise lahendus.

- Lahendus on installitud käitusjuhendi alustamiseks luua [töö ressursside](#automation-jobs) , nagu allpool kirjeldatud.
- Ajakava käitusjuhendi alustamiseks luua [ajakava ressurss](#schedules) , nagu allpool kirjeldatud. 


## <a name="automation-jobs"></a>Automaatika tööde haldamine
Selleks, et alustada on käitusjuhendi kui halduse lahendus on installitud, saate luua **töö** ressursi.  Töö ressursid **Microsoft.Automation/automationAccounts/jobs** tüüp ja atribuudid on järgmises tabelis.

| Atribuut | Kirjeldus |
|:--|:--|
| käitusjuhendi    | Ühe **nimi** üksuse käitusjuhendi nimega.  |
| parameetrid | Üksuse iga parameetri nõutud käitusjuhendi. |


Töö sisaldab käitusjuhendi nime ja mis tahes parameetrite väärtused, mis saadetakse käitusjuhendi.  Töö peab [sõltuvad sellest,](operations-management-suite-solutions-creating.md#resources) mis see on hakanud alates käitusjuhendi käitusjuhendi tuleb luua enne tööd.  Saate luua ka sõltuvused muud tööd tegevusraamatud, mis tuleb enne praeguse täita.

Töö ressursi nimi peab sisaldama GUID, mille tavaliselt määrab parameeter.  Lugege lisateavet GUID parameetrite [loomine lahenduste toimingud halduse komplekti (OMS)](operations-management-suite-solutions-creating.md#parameters)kohta.  

Järgmine on näide töö ressurss, mis algab on käitusjuhendi halduse lahendus on installitud.  Mõnda muud tegevusraamatud tuleb sooritada enne selle ühte käivitub, nii, et see sõltuvustega selle käitusjuhendi töökohta.  Projekti üksikasjade pakutakse parameetrite ja muutujate asemel on määratud otse kaudu.

    {
        "name": "[concat(parameters('accountName'), '/', parameters('startVmsRunbookGuid'))]",
        "type": "Microsoft.Automation/automationAccounts/jobs",
        "apiVersion": "[variables('AutomationApiVersion')]",
        "location": "[parameters('regionId')]",
        "dependsOn": [
            "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('startVmsRunbookName'))]",
            "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/jobs/', parameters('initialPrepRunbookGuid'))]"
        ],
        "tags": { },
        "properties": {
            "runbook": {
                "name": "[variables('startVmsRunbookName')]"
            },
            "parameters": {
                "AzureConnectionAssetName": "[variables('AzureConnectionAssetName')]",
                "ResourceGroupName": "[variables('ResourceGroupName')]"
            }
        }
    }


## <a name="certificates"></a>Serdid
[Azure'i automaatika serdid](../automation/automation-certificates.md) **Microsoft.Automation/automationAccounts/certificates** tüüp ja atribuudid on järgmises tabelis.

| Atribuut | Kirjeldus |
|:--|:--|
| base64Value   | Base 64 väärtus serti. |
| sõrmejälje    | Sõrmejälje serti. |

Allpool on näiteks serdi ressursi.

    "name": "[concat(parameters('accountName'), '/MyCertificate')]",
    "type": "certificates",
    "apiVersion": "2015-01-01-preview",
    "location": "[parameters('regionId')]",
    "tags": {},
    "dependsOn": [
    ],
    "properties": {
        "base64Value": "MIIC1jCCAA8gAwIAVgIYJY4wXCXH/YAHMtALh7qEFzANAgkqhkiG5w0AAQUFGDAUMRIwEBYDVQQDEwlsA3NhAGevc3QqHhcNMTZwNjI5MHQxMjAsWhcNOjEwNjI5NKWwMDAwWjAURIwEAYDVQQBEwlsA2NhAGhvc3QwggEiMA0GCSqGSIA3DQEAAQUAA4IADwAwggEKAoIAAQDIyzv2A0RUg1/AAryI9W1DGAHAqqGdlFfTkUSDfv+hEZTAwKv0p8daqY6GroT8Du7ctQmrxJsy8JxIpDWxUaWwXtvv1kR9eG9Vs5dw8gqhjtOwgXvkOcFdKdQwA82PkcXoHlo+NlAiiPPgmHSELGvcL1uOgl3v+UFiiD1ro4qYqR0ITNhSlq5v2QJIPnka8FshFyPHhVtjtKfQkc9G/xDePW8dHwAhfi8VYRmVMmJAEOLCAJzRjnsgAfznP8CZ/QUczPF8LuTZ/WA/RaK1/Arj6VAo1VwHFY4AZXAolz7xs2sTuHplVO7FL8X58UvF7nlxq48W1Vu0l8oDi2HjvAgMAAAGjJDAiMAsGA1UdDwREAwIEsDATAgNVHSUEDDAKAggrAgEFNQcDATANAgkqhkiG9w0AAQUFAAOCAQEAk8ak2A5Ug4Iay2v0uXAk95qdAthJQN5qIVA13Qay8p4MG/S5+aXOVz4uMXGt18QjGds1A7Q8KDV4Slnwn95sVgA5EP7akvoGXhgAp8Dm90sac3+aSG4fo1V7Y/FYgAgpEy4C/5mKFD1ATeyyhy3PmF0+ZQRJ7aLDPAXioh98LrzMZr1ijzlAAKfJxzwZhpJamAwjZCYqiNZ54r4C4wA4QgX9sVfQKd5e/gQnUM8gTQIjQ8G2973jqxaVNw9lZnVKW3C8/QyLit20pNoqX2qQedwsqg3WCUcPRUUqZ4NpQeHL/AvKIrt158zAfU903yElAEm2Zr3oOUR4WfYQ==",
        "thumbprint": "F485CBE5569F7A5019CB68D7G6D987AC85124B4C"
    }

## <a name="credentials"></a>Identimisteave
[Azure'i automaatika identimisteabe](../automation/automation-credentials.md) **Microsoft.Automation/automationAccounts/credentials** tüüp ja atribuudid on järgmises tabelis.

| Atribuut | Kirjeldus |
|:--|:--|
| Kasutajanimi   | Kasutajanimi identimisteabega. |
| parooli   | Parooli identimisteabega. |

Allpool on näiteks mandaati ressursi.

    "name": "[concat(parameters('accountName'), '/', variables('credentialName'))]",
    "type": "Microsoft.Automation/automationAccounts/runbooks/credentials",
    "apiVersion": "[variables('AutomationApiVersion')]",
    "location": "[parameters('regionId')]",
    "tags": { },
    "dependsOn": [
    ],
    "properties": {
        "userName": "User01",
        "password": "password"
    }


## <a name="schedules"></a>Ajakava
[Azure'i automaatika ajakava](../automation/automation-schedules.md) **Microsoft.Automation/automationAccounts/schedules** tüüp ja atribuudid on järgmises tabelis.

| Atribuut | Kirjeldus |
|:--|:--|
| kirjeldus | Ajasta valikuline kirjeldus. |
| startTime   | Saate määrata ajakava algusaja objektina kuupäev ja kellaaeg. Kui selle saab teisendada kehtiv kuupäev ja kellaaeg, võib anda string. |
| isEnabled   | Saate määrata, kas ajakava on lubatud. |
| intervall    | Tüüp, salvestusintervalli ajakava.<br><br>päeva<br>Hour |
| Frequency   | Frequency, mis ajakava peaks tule või number. |

Allpool on näiteks ajakava ressurss.

    "name": "[concat(parameters('accountName'), '/', variables('variableName'))]",
    "type": "microsoft.automation/automationAccounts/schedules",
    "apiVersion": "[variables('AutomationApiVersion')]",
    "tags": { },
    "dependsOn": [
    ],
    "properties": {
        "description": "Schedule for StartByResourceGroup runbook",
        "startTime": "10/30/2016 12:00:00",
        "isEnabled": "true",
        "interval": "day",
        "frequency": "1"
    }


## <a name="variables"></a>Muutujad
[Azure'i automaatika muutujate](../automation/automation-variables.md) tüüpi **Microsoft.Automation/automationAccounts/variables** ja atribuudid on järgmises tabelis.

| Atribuut | Kirjeldus |
|:--|:--|
| kirjeldus | Muutuja valikuline kirjeldus. |
| isEncrypted | Saate määrata, kas muutuja peaksid olema krüptitud. |
| tüüp        | Muutuja andmetüüp. |
| väärtus       | Muutuja väärtuse. |

Allpool on näiteks muutuv ressursi.

    "name": "[concat(parameters('accountName'), '/', variables('StartTargetResourceGroupsAssetName')) ]",
    "type": "microsoft.automation/automationAccounts/variables",
    "apiVersion": "[variables('AutomationApiVersion')]",
    "tags": { },
    "dependsOn": [
    ],
    "properties": {
        "description": "Description for the variable.",
        "isEncrypted": "true",
        "type": "string",
        "value": "'This is a string value.'"
    }


## <a name="modules"></a>Moodulid
Teie lahendus ei pea määratlemine [globaalne moodulid](../automation/automation-integration-modules.md) kasutada oma tegevusraamatud, sest need alati kättesaadav.  Tuleb kaasata muud moodul, mis kasutavad teie tegevusraamatud ressurss ja käitusjuhendi peaks sõltuvad mooduli ressursi tagamaks, et see on loodud enne käitusjuhendi.

[Integreerimine moodulid](../automation/automation-integration-modules.md) **Microsoft.Automation/automationAccounts/modules** tüüp ja atribuudid on järgmises tabelis.

| Atribuut | Kirjeldus |
|:--|:--|
| contentLink | Saate määrata mooduli sisu. <br><br>URI - Uri käitusjuhendi sisu.  Sellest saab .ps1 faili PowerShelli ja skripti tegevusraamatud ja eksporditud graafilise käitusjuhendi faili jaoks graafik käitusjuhendi.  <br> versiooni - käitusjuhendi oma jälgimise versiooni. |

Allpool on näiteks mooduli ressursi.

    {       
        "name": "[concat(parameters('accountName'), '/', variables('OMSIngestionModuleName'))]",
        "type": "Microsoft.Automation/automationAccounts/modules",
        "apiVersion": "[variables('AutomationApiVersion')]",
        "dependsOn": [
        ],
        "properties": {
            "contentLink": {
                "uri": "https://devopsgallerystorage.blob.core.windows.net/packages/omsingestionapi.1.3.0.nupkg"
            }
        }
    }

### <a name="updating-modules"></a>Moodulid värskendamine
Kui värskendate halduse lahenduse, mis sisaldab käitusjuhendi, mis kasutab ajakava ja uue versiooni teie lahendus on uus moodul, mis kasutavad seda käitusjuhendi, siis käitusjuhendi võib kasutada mooduli vanemat versiooni.  Tuleks lisada järgmised tegevusraamatud teie lahendus ja looge töö käivitamiseks need enne mis tahes muud tegevusraamatud.  See tagab, et kõik moodulid on värskendatud nagu enne selle tegevusraamatud laaditakse.

- [Update-ModulesinAutomationToLatestVersion](https://www.powershellgallery.com/packages/Update-ModulesInAutomationToLatestVersion/1.03/DisplayScript) tagavad, et kõik moodulid kasutavad tegevusraamatud teie lahendus on uusim versioon.  
- [ReRegisterAutomationSchedule – MS-Mgmt](https://www.powershellgallery.com/packages/ReRegisterAutomationSchedule-MS-Mgmt/1.0/DisplayScript) registreerige seda uuesti kõik tagamaks, et selle tegevusraamatud nendega seotud kasutamisel uusima moodulid ajakava ressursid.


Järgmine on näide sellest nõutavat elementi, mis toetavad moodulit värskenduse lahendus.
    
    "parameters": {
        "ModuleImportGuid": {
            "type": "string",
            "metadata": {
                "control": "guid"
            }
        },
        "ReRegisterRunbookGuid": {
            "type": "string",
            "metadata": {
                "control": "guid"
            }
        }
    },

    "variables": {
        "ModuleImportRunbookName": "Update-ModulesInAutomationToLatestVersion",
        "ModuleImportRunbookUri": "https://www.powershellgallery.com/packages/Update-ModulesInAutomationToLatestVersion/1.03/Content/Update-ModulesInAutomationToLatestVersion.ps1",
        "ModuleImportRunbookDescription": "Imports modules and also updates all Azure dependent modules that are in the same Automation Account.",
        "ReRegisterSchedulesRunbookName": "Update-ModulesInAutomationToLatestVersion",
        "ReRegisterSchedulesRunbookUri": "https://www.powershellgallery.com/packages/ReRegisterAutomationSchedule-MS-Mgmt/1.0/Content/ReRegisterAutomationSchedule-MS-Mgmt.ps1",
        "ReRegisterSchedulesRunbookDescription": "Reregisters schedules to ensure that they use latest modules."
    },

    "resources": [
        {
            "name": "[concat(parameters('accountName'), '/', variables('ModuleImportRunbookName'))]",
            "type": "Microsoft.Automation/automationAccounts/runbooks",
            "apiVersion": "[variables('AutomationApiVersion')]",
            "dependsOn": [
            ],
            "location": "[parameters('regionId')]",
            "tags": { },
            "properties": {
                "runbookType": "PowerShell",
                "logProgress": "true",
                "logVerbose": "true",
                "description": "[variables('ModuleImportRunbookDescription')]",
                "publishContentLink": {
                    "uri": "[variables('ModuleImportRunbookUri')]",
                    "version": "1.0.0.0"
                }
            }
        },
        {
            "name": "[concat(parameters('accountName'), '/', parameters('ModuleImportGuid'))]",
            "type": "Microsoft.Automation/automationAccounts/jobs",
            "apiVersion": "[variables('AutomationApiVersion')]",
            "location": "[parameters('regionId')]",
            "dependsOn": [
                "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('ModuleImportRunbookName'))]"
            ],
            "tags": { },
            "properties": {
                "runbook": {
                    "name": "[variables('ModuleImportRunbookName')]"
                },
                "parameters": {
                    "ResourceGroupName": "[resourceGroup().name]",
                    "AutomationAccountName": "[parameters('accountName')]",
                    "NewModuleName": "AzureRM.Insights"
                }
            }
        },
        {
          "name": "[concat(parameters('accountName'), '/', variables('ReRegisterSchedulesRunbookName'))]",
          "type": "Microsoft.Automation/automationAccounts/runbooks",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "dependsOn": [
          ],
          "location": "[parameters('regionId')]",
          "tags": { },
          "properties": {
            "runbookType": "PowerShell",
            "logProgress": "true",
            "logVerbose": "true",
            "description": "[variables('ReRegisterSchedulesRunbookDescription')]",
            "publishContentLink": {
              "uri": "[variables('ReRegisterSchedulesRunbookUri')]",
              "version": "1.0.0.0"
            }
          }
        },
        {
          "name": "[concat(parameters('accountName'), '/', parameters('reRegisterSchedulesGuid'))]",
          "type": "Microsoft.Automation/automationAccounts/jobs",
          "apiVersion": "[variables('AutomationApiVersion')]",
          "location": "[parameters('regionId')]",
          "dependsOn": [
            "[concat('Microsoft.Automation/automationAccounts/', parameters('accountName'), '/runbooks/', variables('ReRegisterSchedulesRunbookName'))]"
          ],
          "tags": { },
          "properties": {
            "runbook": {
              "name": "[variables('ReRegisterSchedulesRunbookName')]"
            },
            "parameters": {
              "targetSubscriptionId": "[subscription().subscriptionId]",
              "resourcegroup": "[resourceGroup().name]",
              "automationaccount": "[parameters('accountName')]"
            }
        }
    ]


## <a name="next-steps"></a>Järgmised sammud

- [Lisa oma lahendus vaade](operations-management-suite-solutions-resources-views.md) kogutud andmete visualiseerimiseks.