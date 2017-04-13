<properties
   pageTitle="Ressursihaldur malli salajase võtme võlvkelder | Microsoft Azure'i"
   description="Näitab ressursihaldur skeemi võtme vault saladusi läbi malli kasutamise kohta."
   services="azure-resource-manager,key-vault"
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
   ms.date="06/23/2016"
   ms.author="tomfitz"/>

# <a name="key-vault-secret-template-schema"></a>Võtme vault salajane malli skeem

Loob salajane, mis on talletatud võtme võlvkelder. Selle ressursi tüüp sageli juurutatud [võtme vault](resource-manager-template-keyvault.md)lapse ressurss.

## <a name="schema-format"></a>Skeemi vorming

Võtme vault salajane loomiseks järgmist skeemi lisamine malli. Salajane saate määratleda, kas lapse ressursi võtme võlvikus või ülataseme ressursi. Saate määratleda selle ressursi lapse võtme vault juurutamisel sama malli. Peate määratleda salajane ülataseme ressursi võtme vault on juurutatud sama mall või kui peate looma mitme saladusi, hoidke ressursi tüübist. 

    {
        "type": enum,
        "apiVersion": "2015-06-01",
        "name": string,
        "properties": {
            "value": string
        },
        "dependsOn": [ array values ]
    }

## <a name="values"></a>Väärtused

Järgmises tabelis on kirjeldatud peate määrama skeemi väärtused.

| Nimi | Väärtus |
| ---- | ---- | 
| tüüp | Loetelu<br />Nõutav<br />**saladused** (kui juurutatud lapse ressurss võtme vault) või<br /> **Microsoft.KeyVault/vaults/secrets** (kui ülataseme ressursi juurutamisel)<br /><br />Ressursi tüüp loomiseks. |
| apiVersion | Loetelu<br />Nõutav<br />**2015-06-01** või **2014 12-19 eelvaade**<br /><br />Ressursi loomiseks kasutatav API versioon. | 
| Nimi | String<br />Nõutav<br />Üksiku sõna lapse ressursi võlvikus klahv või vormingus juurutamisel **{võtme vault nimi} / {salajane nimi}** ülataseme ressursi lisada olulisi olemasolev vault juurutamisel.<br /><br />Salajane loomiseks nimi. |
| atribuudid | Objekti<br />Nõutav<br />[objekti atribuudid](#properties)<br /><br />Objekti, mis määrab salajane loomiseks väärtus. |
| ei leia dependsOn | Massiiv<br />Valikuline.<br />Komaga eraldatud loend ressursi nimed või ressursside ainulaadsed ID-d.<br /><br />Saidikogumi ressursside sõltub selle lingi. Kui klahv vault jaoks salajane on juurutatud sama malli, kaasake – see element tagada esmalt juurutamist võtme vault nime. |

<a id="properties" />
### <a name="properties-object"></a>objekti atribuudid

| Nimi | Väärtus |
| ---- | ---- | 
| väärtus | String<br />Nõutav<br /><br />Salajane väärtus võtme võlvkelder talletamiseks. Kui läbides selle atribuudi väärtust, kasutage tüüp **securestring**parameeter.  |

    
## <a name="examples"></a>Näited

Esimene näide kasutab salajase võtme võlvikus lapse ressurssi.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "keyVaultName": {
                "type": "string",
                "metadata": {
                    "description": "Name of the vault"
                }
            },
            "tenantId": {
                "type": "string",
                "metadata": {
                   "description": "Tenant ID for the subscription and use assigned access to the vault. Available from the Get-AzureRmSubscription PowerShell cmdlet"
                }
            },
            "objectId": {
                "type": "string",
                "metadata": {
                    "description": "Object ID of the AAD user or service principal that will have access to the vault. Available from the Get-AzureRmADUser or the Get-AzureRmADServicePrincipal cmdlets"
                }
            },
            "keysPermissions": {
                "type": "array",
                "defaultValue": [ "all" ],
                "metadata": {
                    "description": "Permissions to grant user to keys in the vault. Valid values are: all, create, import, update, get, list, delete, backup, restore, encrypt, decrypt, wrapkey, unwrapkey, sign, and verify."
                }
            },
            "secretsPermissions": {
                "type": "array",
                "defaultValue": [ "all" ],
                "metadata": {
                    "description": "Permissions to grant user to secrets in the vault. Valid values are: all, get, set, list, and delete."
                }
            },
            "vaultSku": {
                "type": "string",
                "defaultValue": "Standard",
                "allowedValues": [
                    "Standard",
                    "Premium"
                ],
                "metadata": {
                    "description": "SKU for the vault"
                }
            },
            "enabledForDeployment": {
                "type": "bool",
                "defaultValue": false,
                "metadata": {
                    "description": "Specifies if the vault is enabled for VM or Service Fabric deployment"
                }
            },
            "enabledForTemplateDeployment": {
                "type": "bool",
                "defaultValue": false,
                "metadata": {
                    "description": "Specifies if the vault is enabled for ARM template deployment"
                }
            },
            "enableVaultForVolumeEncryption": {
                "type": "bool",
                "defaultValue": false,
                "metadata": {
                    "description": "Specifies if the vault is enabled for volume encryption"
                }
            },
            "secretName": {
                "type": "string",
                "metadata": {
                    "description": "Name of the secret to store in the vault"
                }
            },
            "secretValue": {
                "type": "securestring",
                "metadata": {
                    "description": "Value of the secret to store in the vault"
                }
            }
        },
        "resources": [
        {
            "type": "Microsoft.KeyVault/vaults",
            "name": "[parameters('keyVaultName')]",
            "apiVersion": "2015-06-01",
            "location": "[resourceGroup().location]",
            "tags": {
                "displayName": "KeyVault"
            },
            "properties": {
                "enabledForDeployment": "[parameters('enabledForDeployment')]",
                "enabledForTemplateDeployment": "[parameters('enabledForTemplateDeployment')]",
                "enabledForVolumeEncryption": "[parameters('enableVaultForVolumeEncryption')]",
                "tenantId": "[parameters('tenantId')]",
                "accessPolicies": [
                {
                    "tenantId": "[parameters('tenantId')]",
                    "objectId": "[parameters('objectId')]",
                    "permissions": {
                        "keys": "[parameters('keysPermissions')]",
                        "secrets": "[parameters('secretsPermissions')]"
                    }
                }],
                "sku": {
                    "name": "[parameters('vaultSku')]",
                    "family": "A"
                }
            },
            "resources": [
            {
                "type": "secrets",
                "name": "[parameters('secretName')]",
                "apiVersion": "2015-06-01",
                "properties": {
                    "value": "[parameters('secretValue')]"
                },
                "dependsOn": [
                    "[concat('Microsoft.KeyVault/vaults/', parameters('keyVaultName'))]"
                ]
            }]
        }]
    }

Teine näide kasutab salajane ülataseme ressurssi, mis on talletatud olemasolev võtme vault.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "keyVaultName": {
                "type": "string",
                "metadata": {
                    "description": "Name of the existing vault"
                }
            },
            "secretName": {
                "type": "string",
                "metadata": {
                    "description": "Name of the secret to store in the vault"
                }
            },
            "secretValue": {
                "type": "securestring",
                "metadata": {
                    "description": "Value of the secret to store in the vault"
                }
            }
        },
        "variables": {},
        "resources": [
            {
                "type": "Microsoft.KeyVault/vaults/secrets",
                "apiVersion": "2015-06-01",
                "name": "[concat(parameters('keyVaultName'), '/', parameters('secretName'))]",
                "properties": {
                    "value": "[parameters('secretValue')]"
                }
            }
        ],
        "outputs": {}
    }


## <a name="next-steps"></a>Järgmised sammud

- Võtme võlvid üldist teavet leiate teemast [Alustamine Azure'i klahvi Vault](./key-vault/key-vault-get-started.md).
- Näiteks viitamine võtme vault salajane juurutamisel Mallid, leiate teemast [turvaline väärtuste juurutamisel edasi](resource-manager-keyvault-parameter.md).


