<properties
   pageTitle="Klahv Vault salajane ressursihaldur malli abil | Microsoft Azure'i"
   description="Näitab, kuidas edasi salajase võtme hoidlast parameetrina käigus juurutamist."
   services="azure-resource-manager,key-vault"
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
   ms.date="06/23/2016"
   ms.author="tomfitz"/>

# <a name="pass-secure-values-during-deployment"></a>Liigu turvaline väärtuste juurutamisel

Kui peate juurutamisel parameetrina turvaline väärtust (nt parooli) edastada, saate talletada selle väärtuse salajase, mis on [Azure klahvi Vault](./key-vault/key-vault-whatis.md) ja muud ressursihaldur Mallid väärtus viidata. Saate lisada ainult viide salajane malli nii salajane kunagi kokku ja pole vaja käsitsi sisestage väärtus on salajane iga kord, kui juurutate ressursside. Saate määrata, millised kasutajad või teenuse põhisumma pääsevad salajane.  

## <a name="deploy-a-key-vault-and-secret"></a>Mõne võtme hoidla ja salajane juurutamine

Võtme hoidla, mis saab viidata muude ressursihaldur mallide loomiseks tuleb atribuudi **enabledForTemplateDeployment** väärtuseks **true**ja kasutajale või teenuse põhilise, mis käivitatakse juurutus, mis viitab salajane tuleb tagada juurdepääs.

Juurutamise on klahv hoidla ja salajane leiate jaotisest [klahvi vault skeemi](resource-manager-template-keyvault.md) ja [klahvi vault salajane skeemi](resource-manager-template-keyvault-secret.md).

## <a name="reference-a-secret-with-static-id"></a>Viide salajase staatilise id-ga

Viidatava salajane kaudu parameetrite-faili, mis läheb väärtused malli. Viidatava salajane sooritab ressursiidentifikaator võtme Vault ja salajane nimi. Selles näites võtme vault salajane peab juba olemas ja kasutate staatiline väärtus selle ressursi ID-d.

    "parameters": {
      "adminPassword": {
        "reference": {
          "keyVault": {
            "id": "/subscriptions/{guid}/resourceGroups/{group-name}/providers/Microsoft.KeyVault/vaults/{vault-name}"
          }, 
          "secretName": "sqlAdminPassword" 
        } 
      }
    }

Kogu parameetri failina tunduda:

    {
      "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "sqlsvrAdminLogin": {
          "value": ""
        },
        "sqlsvrAdminLoginPassword": {
          "reference": {
            "keyVault": {
              "id": "/subscriptions/{guid}/resourceGroups/{group-name}/providers/Microsoft.KeyVault/vaults/{vault-name}"
            },
            "secretName": "adminPassword"
          }
        }
      }
    }

Parameeter, mis aktsepteerib salajane peaks olema on **securestring**. Järgmises näites on kujutatud malli, mis kasutab SQL server, mis nõuab administraatori parooli jaotised.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "sqlsvrAdminLogin": {
                "type": "string",
                "minLength": 4
            },
            "sqlsvrAdminLoginPassword": {
                "type": "securestring"
            },
            ...
        },
        "resources": [
        {
            "name": "[variables('sqlsvrName')]",
            "type": "Microsoft.Sql/servers",
            "location": "[resourceGroup().location]",
            "apiVersion": "2014-04-01-preview",
            "properties": {
                "administratorLogin": "[parameters('sqlsvrAdminLogin')]",
                "administratorLoginPassword": "[parameters('sqlsvrAdminLoginPassword')]"
            },
            ...
        }],
        "variables": {
            "sqlsvrName": "[concat('sqlsvr', uniqueString(resourceGroup().id))]"
        },
        "outputs": { }
    }

## <a name="reference-a-secret-with-dynamic-id"></a>Dünaamiliste id-ga salajase viitamine.

Eelmise jaotise ilmnes kuidas staatiline ressursi id jaoks klahv vault salajane edasi. Siiski mõnel juhul peate viidata võtme vault salajase mis muutub praeguse juurutamise põhjal. Sel juhul ei saa te suur-koodi ressursi id failis parameetrid. Kahjuks ei saa dünaamiliselt luua ressursi id parameetrid faili kuna malli avaldiste parameetrite fail pole lubatud.

Dünaamiliselt luua võtme vault salajane ressursi ID-d, peate teisaldama ressurss, mis tuleb salajane pesastatud malli. Juhtslaidi malli lisamine pesastatud Mall ja edastama parameeter, mis sisaldab dünaamiliselt loodud ressursi ID-d.

    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "vaultName": {
          "type": "string"
        },
        "secretName": {
          "type": "string"
        }
      },
      "resources": [
        {
          "apiVersion": "2015-01-01",
          "name": "nestedTemplate",
          "type": "Microsoft.Resources/deployments",
          "properties": {
            "mode": "incremental",
            "templateLink": {
              "uri": "https://www.contoso.com/AzureTemplates/newVM.json",
              "contentVersion": "1.0.0.0"
            },
            "parameters": {
              "adminPassword": {
                "reference": {
                  "keyVault": {
                    "id": "[concat(resourceGroup().id, '/providers/Microsoft.KeyVault/vaults/', parameters('vaultName'))]"
                  },
                  "secretName": "[parameters('secretName')]"
                }
              }
            }
          }
        }
      ],
      "outputs": {}
    }


## <a name="next-steps"></a>Järgmised sammud

- Võtme võlvid üldist teavet leiate teemast [Alustamine Azure'i klahvi Vault](./key-vault/key-vault-get-started.md).
- Võtme vault virtuaalse masina kasutamise kohta leiate artiklist [turvakaalutlused Azure'i ressursihaldur](best-practices-resource-manager-security.md).
- Täieliku näiteid viitamine võtme saladusi, vt [klahvi Vault näited](https://github.com/rjmax/ArmExamples/tree/master/keyvaultexamples).

