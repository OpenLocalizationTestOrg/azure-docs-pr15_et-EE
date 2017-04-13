<properties
   pageTitle="Ressursihaldur malli võtme vault | Microsoft Azure'i"
   description="Näitab ressursihaldur skeemi võtme võlvid kaudu malli kasutamise kohta."
   services="azure-resource-manager,key-vault"
   documentationCenter="na"
   authors="tfitzmac"
   manager="wpickett"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/23/2016"
   ms.author="tomfitz"/>

# <a name="key-vault-template-schema"></a>Võtme vault malli skeemifailid

Loob võtme vault.

## <a name="schema-format"></a>Skeemi vorming

Võtme vault loomiseks lisage järgmine skeem ressursid jaotisele malli.

    {
        "type": "Microsoft.KeyVault/vaults",
        "apiVersion": "2015-06-01",
        "name": string,
        "location": string,
        "properties": {
            "enabledForDeployment": bool,
            "enabledForTemplateDeployment": bool,
            "enabledForVolumeEncryption": bool,
            "tenantId": string,
            "accessPolicies": [
                {
                    "tenantId": string,
                    "objectId": string,
                    "permissions": {
                        "keys": [ keys permissions ],
                        "secrets": [ secrets permissions ]
                    }
                }
            ],
            "sku": {
                "name": enum,
                "family": "A"
            }
        },
        "resources": [
             child resources
        ]
    }

## <a name="values"></a>Väärtused

Järgmises tabelis on kirjeldatud peate määrama skeemi väärtused.

| Nimi | Väärtus |
| ---- | ---- | 
| tüüp | Loetelu<br />Nõutav<br />**Microsoft.KeyVault/vaults**<br /><br />Ressursi tüüp loomiseks. |
| apiVersion | Loetelu<br />Nõutav<br />**2015-06-01** või **2014 12-19 eelvaade**<br /><br />Ressursi loomiseks kasutatav API versioon. | 
| Nimi | String<br />Nõutav<br />Nimi, mis on kordumatu Azure'i üle.<br /><br />Nime loomiseks olulisi Vault. Kaaluge võimalust, kasutades funktsiooni [uniqueString](resource-group-template-functions.md#uniquestring) oma nimetamistava luua kordumatu nimi, nagu on näidatud järgmises näites. |
| asukoht | String<br />Nõutav<br />Võtme võlvid kehtiv regioon. Lubatud piirkondade määratlemiseks vaadata [toetatud regioonide](resource-manager-supported-services.md#supported-regions).<br /><br />Piirkonna majutada võtme vault. |
| atribuudid | Objekti<br />Nõutav<br />[objekti atribuudid](#properties)<br /><br />Objekti, mis määrab loomiseks olulisi vault tüüpi. |
| ressursid | Massiiv<br />Valikuline.<br />Lubatud väärtused: [Key vault salajane ressursid](resource-manager-template-keyvault-secret.md)<br /><br />Võtme vault lapse ressursid. |

<a id="properties" />
### <a name="properties-object"></a>objekti atribuudid

| Nimi | Väärtus |
| ---- | ---- | 
| enabledForDeployment | Kahendmuutuja<br />Valikuline.<br />**tõene** või **väär**<br /><br />Saate määrata, kui vault on lubatud virtuaalse masina või teenuse struktuuri juurutamiseks. |
| enabledForTemplateDeployment | Kahendmuutuja<br />Valikuline.<br />**tõene** või **väär**<br /><br />Saate määrata, kui vault on lubatud ressursihaldur malli kasutuselevõttu kasutamiseks. Lisateabe saamiseks lugege teemat [edasi turvaline väärtuste juurutamisel](resource-manager-keyvault-parameter.md) |
| enabledForVolumeEncryption | Kahendmuutuja<br />Valikuline.<br />**tõene** või **väär**<br /><br />Saate määrata, kui vault on lubatud krüptimine. |
| tenantId | String<br />Nõutav<br />**Globaalselt kordumatu ID**<br /><br />Tellimuse ID rentniku. Saate tuua see [Get-AzureRmSubscription](https://msdn.microsoft.com/library/azure/mt619284.aspx) PowerShelli cmdlet-käsk või selle **Azure'i konto kuvada** Azure'i CLI käsk. |
| accessPolicies | Massiiv<br />Nõutav<br />[accessPolicies objekt](#accesspolicies)<br /><br />Massiivi kuni 16 objekte, mis teenuse põhilise ja kasutajale õiguste määramine. |
| SKU-ga | Objekti<br />Nõutav<br />[objekti SKU-ga](#sku)<br /><br />SKU võtme vault jaoks. |

<a id="accesspolicies" />
### <a name="propertiesaccesspolicies-object"></a>properties.accessPolicies objekt

| Nimi | Väärtus |
| ---- | ---- | 
| tenantId | String<br />Nõutav<br />**Globaalselt kordumatu ID**<br /><br />Azure Active Directory rentniku sisaldav **ObjectId väärtuse** selle juurdepääsu poliitikale rentniku identifikaator |
| ObjectId väärtuse | String<br />Nõutav<br />**Globaalselt kordumatu ID**<br /><br />Objekti ainuidentifikaator Azure Active Directory kasutaja või teenuse põhilise, mis on juurdepääs vault. Saate tuua väärtus [Get-AzureRmADUser](https://msdn.microsoft.com/library/azure/mt679001.aspx) või [Get-AzureRmADServicePrincipal](https://msdn.microsoft.com/library/azure/mt678992.aspx) PowerShelli cmdlet-käskude või **azure ad kasutaja** või **azure ad sp** Azure'i CLI käsud. |
| õigused | Objekti<br />Nõutav<br />[objekti õiguste](#permissions)<br /><br />Klõpsake selle vault Active Directory objekti õiguste. |

<a id="permissions" />
### <a name="propertiesaccesspoliciespermissions-object"></a>properties.accessPolicies.permissions objekt

| Nimi | Väärtus |
| ---- | ---- | 
| Kiirklahvid | Massiiv<br />Nõutav<br />**Kõik**, **varukoopia**, **luua**, **dekrüptida**, **kustutamine**, **krüptida**, **saada**, **importida**, **loendi**, **taastamine**, **Logi**, **unwrapkey**, **värskendada**, **kinnitamine**, **wrapkey**<br /><br />Õigusi anda selle võlvkelder selle Active Directory objekti võtmed. See väärtus peab olema määratud üks või mitu lubatud väärtuste massiivi. |
| saladused | Massiiv<br />Nõutav<br />**Kõik**, **Kustuta**, **saada**, **loendi**, **määramine**<br /><br />Õiguste saladusi see võlvkelder selle Active Directory objekti. See väärtus peab olema määratud üks või mitu lubatud väärtuste massiivi. |

<a id="sku" />
### <a name="propertiessku-object"></a>Properties.SKU objekt

| Nimi | Väärtus |
| ---- | ---- | 
| Nimi | Loetelu<br />Nõutav<br />**Standard**või **premium** <br /><br />Teenuse kiht, KeyVault kasutada.  Standard toetab saladusi ja tarkvara kaitstud võtmed.  Premium lisab toe HSM kaitstud võtmed. |
| pere | Loetelu<br />Nõutav<br />**A** <br /><br />Sku pere kasutada. |
 
    
## <a name="examples"></a>Näited

Järgmine näide kasutab mõnda võtme hoidla ja salajane.

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

## <a name="quickstart-templates"></a>Kiirjuhend Mallid

Järgmised Kiirjuhend Mall kasutab võtme vault.

- [Võtme Vault loomine](https://azure.microsoft.com/documentation/templates/101-key-vault-create/)


## <a name="next-steps"></a>Järgmised sammud

- Võtme võlvid üldist teavet leiate teemast [Alustamine Azure'i klahvi Vault](./key-vault/key-vault-get-started.md).
- Näiteks viitamine võtme vault salajane juurutamisel Mallid, leiate teemast [turvaline väärtuste juurutamisel edasi](resource-manager-keyvault-parameter.md).

