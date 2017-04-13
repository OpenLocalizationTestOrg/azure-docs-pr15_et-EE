<properties
   pageTitle="Lingitud mallide abil ressursihaldur | Microsoft Azure'i"
   description="Kirjeldab, kuidas luua muutuv malli lahenduse Azure'i ressursihaldur mallis lingitud mallide kasutamise kohta. Näitab, kuidas edasi parameetrite väärtused, määrake parameetri faili ja dünaamiliselt loodud URL-id."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/02/2016"
   ms.author="tomfitz"/>

# <a name="using-linked-templates-with-azure-resource-manager"></a>Azure'i ressursihaldur lingitud mallide kasutamine

Kaudu sees ühe Azure'i ressursihaldur malli, saate linkida teise Mall, mis võimaldab teil lagunevad juurutamise komplekt suunatud, otstarve kohased mallid. Nagu lagunevatest rakenduse liigitada mitu koodi, lagunemine pakub kasu katsetada, taaskasutuse ja loetavuse.  

Parameetrite mahuksid peamised mallist lingitud malli ja parameetrid saab vastendada otse parameetrid või muutujad esitatud helistaja mall. Lingitud malli edasi anda ka väljundmuutuja tagasi andmeallika mall, mis võimaldab kahesuunaline andmevahetus mallide vahel.

## <a name="linking-to-a-template"></a>Malli linkimine

Saate luua seos kahe Mallid, lisades juurutamise ressursi peamised Mall, mis viitab lingitud mall. Määrake atribuudi **templateLink** URI lingitud malli. Saate sisestada parameetrite väärtused lingitud malli, määrates väärtused otse oma malli või parameetri faili linkimise teel. Järgmises näites kasutatakse atribuudi **parameetrite** otse parameetri väärtuse määramiseks.

    "resources": [ 
      { 
         "apiVersion": "2015-01-01", 
         "name": "linkedTemplate", 
         "type": "Microsoft.Resources/deployments", 
         "properties": { 
           "mode": "incremental", 
           "templateLink": {
              "uri": "https://www.contoso.com/AzureTemplates/newStorageAccount.json",
              "contentVersion": "1.0.0.0"
           }, 
           "parameters": { 
              "StorageAccountName":{"value": "[parameters('StorageAccountName')]"} 
           } 
         } 
      } 
    ] 

Ressursihaldur teenuse peab olema juurdepääs lingitud mall. Te ei saa määrata kohalik fail või fail, mis on teie kohalikus võrgus lingitud malli saadaval ainult. Saate anda ainult URI väärtus, mis sisaldab **http** - või **https**. Üheks võimaluseks on lingitud malli paigutamiseks salvestusruumi konto ja kasutada URI selle üksuse, nagu on näidatud järgmises näites.

    "templateLink": {
        "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
        "contentVersion": "1.0.0.0",
    }

Kuigi lingitud mall peab olema saadaval väliselt, pole vaja avalikkusele üldiselt kättesaadav. Malli saate lisada privaatne salvestusruumi konto, mis on juurdepääsetav ainult salvestusruumi konto omanik. Seejärel loote ühiskasutusega juurdepääsu allkirja (SAS) luba juurdepääsu lubamiseks käigus juurutamist. Saate lisada selle SAS sõnet URI lingitud malli. Juhised salvestusruumi konto malli seadistamine ja loomine SAS luba, leiate [Deploy ressursid ressursihaldur mallide ja Azure PowerShelli](resource-group-template-deploy.md) või [ressursihaldur mallide ja Azure CLI Deploy ressursid](resource-group-template-deploy-cli.md). 

Järgmises näites on kujutatud ema malli mis on lingitud mõne muu malli. Lingitud mall on kättesaadav SAS luba, mis parameetrina.

    "parameters": {
        "sasToken": { "type": "securestring" }
    },
    "resources": [
        {
            "apiVersion": "2015-01-01",
            "name": "linkedTemplate",
            "type": "Microsoft.Resources/deployments",
            "properties": {
              "mode": "incremental",
              "templateLink": {
                "uri": "[concat('https://storagecontosotemplates.blob.core.windows.net/templates/helloworld.json', parameters('sasToken'))]",
                "contentVersion": "1.0.0.0"
              }
            }
        }
    ],

Ehkki luba edastatakse turvaline stringina, logitakse URI lingitud malli, sh SAS luba, juurutamise toimingute selle ressursi rühma. Piirata, määrake soovitud aegumine luba.

## <a name="linking-to-a-parameter-file"></a>Parameetri faili linkimine

Järgmises näites kasutatakse atribuudi **parametersLink** parameeter faili linkida.

    "resources": [ 
      { 
         "apiVersion": "2015-01-01", 
         "name": "linkedTemplate", 
         "type": "Microsoft.Resources/deployments", 
         "properties": { 
           "mode": "incremental", 
           "templateLink": {
              "uri":"https://www.contoso.com/AzureTemplates/newStorageAccount.json",
              "contentVersion":"1.0.0.0"
           }, 
           "parametersLink": { 
              "uri":"https://www.contoso.com/AzureTemplates/parameters.json",
              "contentVersion":"1.0.0.0"
           } 
         } 
      } 
    ] 

URI väärtus parameeter lingitud faili kohaliku faili ei saa ja peab sisaldama **http** - või **https**. Parameetri fail võib olla piiratud juurdepääs läbi SAS luba.

## <a name="using-variables-to-link-templates"></a>Link: Mallid muutujate abil

Eelmistes näidetes ilmnes kõva kodeeritud URL-i väärtuste malli lingid. Seda moodust võib abiks olla lihtne Mall, kuid see ei tööta ka siis, kui töötate suure hulga muutuv malle. Selle asemel saate luua staatiline muutuja, mis talletab base URL-i põhiaknas malli ja seejärel dünaamiliselt luua URL-ide lingitud mallidest, et base URL. Seda moodust on saab teisaldada või kahvli Mall, kuna soovite muuta staatilise muutuja peamised malli. Peamised malli edastab õige URI-d kogu lagunenud mall.

Järgmises näites kujutatakse base URL-i abil saate luua kaks URL-ide lingitud mallid (**sharedTemplateUrl** ja **vmTemplate**). 

    "variables": {
        "templateBaseUrl": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/postgresql-on-ubuntu/",
        "sharedTemplateUrl": "[concat(variables('templateBaseUrl'), 'shared-resources.json')]",
        "tshirtSizeSmall": {
            "vmSize": "Standard_A1",
            "diskSize": 1023,
            "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-2disk-resources.json')]",
            "vmCount": 2,
            "slaveCount": 1,
            "storage": {
                "name": "[parameters('storageAccountNamePrefix')]",
                "count": 1,
                "pool": "db",
                "map": [0,0],
                "jumpbox": 0
            }
        }
    }

Saate [deployment()](resource-group-template-functions.md#deployment) base URL-i saamiseks praeguse malli kasutada, ja kasutada seda URL-i saamiseks muud mallid samasse asukohta. See lähenemine on kasulik, kui asukoha Mall muutub (võib-olla tingitud versioonimise puhul) või kui soovite vältida raske kodeerimine failis malli URL-id. 

    "variables": {
        "sharedTemplateUrl": "[uri(deployment().properties.templateLink.uri, 'shared-resources.json')]"
    }

## <a name="conditionally-linking-to-templates"></a>Tingimuslikult linkimine Mallid

Saate linkida erinevaid malle, mille parameeter väärtuse, mida kasutatakse ehitada URI lingitud malli. Seda moodust töötab ka siis, kui peate määrama juurutamisel, mis seotud malli kasutada. Näiteks saate määrata ühe malli kasutamiseks salvestusruumi konto ja teise malli kasutada salvestusruumi uus konto.

Järgmises näites on kujutatud parameeter on salvestusruumikonto nimi ja määrata, kas salvestusruumi konto on uude või olemasolevasse parameeter.

    "parameters": {
        "storageAccountName": {
            "type": "String"
        },
        "newOrExisting": {
            "type": "String",
            "allowedValues": [
                "new",
                "existing"
            ]
        }
    },

Saate luua malli URI, mis sisaldab uude või olemasolevasse parameeter muutujana.

    "variables": {
        "templatelink": "[concat('https://raw.githubusercontent.com/exampleuser/templates/master/',parameters('newOrExisting'),'StorageAccount.json')]"
    },

Selle muutuja väärtuse ette juurutamise ressursi.

    "resources": [
        {
            "apiVersion": "2015-01-01",
            "name": "linkedTemplate",
            "type": "Microsoft.Resources/deployments",
            "properties": {
                "mode": "incremental",
                "templateLink": {
                    "uri": "[variables('templatelink')]",
                    "contentVersion": "1.0.0.0"
                },
                "parameters": {
                    "StorageAccountName": {
                        "value": "[parameters('storageAccountName')]"
                    }
                }
            }
        }
    ],

URI lahendab mallina nimega kas **existingStorageAccount.json** või **newStorageAccount.json**. Nende URI-d jaoks mallide loomine.

Järgmises näites on kujutatud **existingStorageAccount.json** mall.

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "storageAccountName": {
          "type": "String"
        }
      },
      "variables": {},
      "resources": [],
      "outputs": {
        "storageAccountInfo": {
          "value": "[reference(concat('Microsoft.Storage/storageAccounts/', parameters('storageAccountName')),providers('Microsoft.Storage', 'storageAccounts').apiVersions[0])]",
          "type" : "object"
        }
      }
    }

Järgmises näites kuvatakse **newStorageAccount.json** mall. Pange tähele, et nagu olemasoleva salvestusruum Mall salvestusruumi konto objekti tagasi väljundid. Kas lingitud malli töötab juhtslaidi mall.

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "storageAccountName": {
          "type": "string"
        }
      },
      "resources": [
        {
          "type": "Microsoft.Storage/storageAccounts",
          "name": "[parameters('StorageAccountName')]",
          "apiVersion": "2016-01-01",
          "location": "[resourceGroup().location]",
          "sku": {
            "name": "Standard_LRS"
          },
          "kind": "Storage",
          "properties": {
          }
        }
      ],
      "outputs": {
        "storageAccountInfo": {
          "value": "[reference(concat('Microsoft.Storage/storageAccounts/', parameters('StorageAccountName')),providers('Microsoft.Storage', 'storageAccounts').apiVersions[0])]",
          "type" : "object"
        }
      }
    }

## <a name="complete-example"></a>Täielik näide

Järgmises näites mallide kuvamine lihtsustatud korraldust lingitud mallide abil illustreerida mitut põhimõtet selles artiklis. Eeldatakse, et malle on lisatud samal pakendi salvestusruumi konto avaliku juurdepääsuga välja lülitatud. Lingitud malli edastab väärtus tagasi põhimalli **väljundid** jaotises.

Faili **parent.json** koosneb järgmistest elementidest:

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "containerSasToken": { "type": "string" }
      },
      "resources": [
        {
          "apiVersion": "2015-01-01",
          "name": "linkedTemplate",
          "type": "Microsoft.Resources/deployments",
          "properties": {
            "mode": "incremental",
            "templateLink": {
              "uri": "[concat(uri(deployment().properties.templateLink.uri, 'helloworld.json'), parameters('containerSasToken'))]",
              "contentVersion": "1.0.0.0"
            }
          }
        }
      ],
      "outputs": {
        "result": {
          "type": "object",
          "value": "[reference('linkedTemplate').outputs.result]"
        }
      }
    }

Faili **helloworld.json** koosneb järgmistest elementidest:

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {},
      "variables": {},
      "resources": [],
      "outputs": {
        "result": {
            "value": "Hello World",
            "type" : "string"
        }
      }
    }
    
PowerShellis ümbris märgiks hankimine ja juurutamine mallide abil.

    Set-AzureRmCurrentStorageAccount -ResourceGroupName ManageGroup -Name storagecontosotemplates
    $token = New-AzureStorageContainerSASToken -Name templates -Permission r -ExpiryTime (Get-Date).AddMinutes(30.0)
    New-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup -TemplateUri ("https://storagecontosotemplates.blob.core.windows.net/templates/parent.json" + $token) -containerSasToken $token

Azure'i CLI, ümbris märgiks hankimine ja juurutamine Mallid järgmine kood. Praegu peab võimaldama juurutamise nimi, kui URI, mis sisaldab SAS luba malli abil.  

    expiretime=$(date -I'minutes' --date "+30 minutes")  
    azure storage container sas create --container templates --permissions r --expiry $expiretime --json | jq ".sas" -r
    azure group deployment create -g ExampleGroup --template-uri "https://storagecontosotemplates.blob.core.windows.net/templates/parent.json?{token}" -n tokendeploy  

Teil palutakse sisestada SAS sõnet parameetrina. Peate eessõna koos **?**luba.

## <a name="next-steps"></a>Järgmised sammud
- Juurutamise tellimuse ressurssidena määratlemise kohta leiate teemast [Azure ressursihaldur Mallid sõltuvused määratlemine](resource-group-define-dependencies.md)
- Saate teada, kuidas määrata ühe ressursi, kuid mitme eksemplari loomine, lugege teemat [mitmes eksemplaris ressursside Azure'i ressursihaldur loomine](resource-group-create-multiple.md)
