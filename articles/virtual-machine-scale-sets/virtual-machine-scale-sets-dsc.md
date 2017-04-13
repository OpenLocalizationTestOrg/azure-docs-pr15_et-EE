<properties
   pageTitle="Abil soovitud maakond konfiguratsiooni virtuaalse masina skaala komplektid | Microsoft Azure'i"
   description="Azure'i DSC laiendiga määrab virtuaalse masina skaala abil"
   services="virtual-machine-scale-sets"
   documentationCenter=""
   authors="zjalexander"
   manager="timlt"
   editor=""
   tags="azure-service-management,azure-resource-manager"
   keywords=""/>

<tags
   ms.service="virtual-machine-scale-sets"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="na"
   ms.date="09/15/2016"
   ms.author="zachal"/>

# <a name="using-virtual-machine-scale-sets-with-the-azure-dsc-extension"></a>Azure'i DSC laiendiga määrab virtuaalse masina skaala abil

[Azure'i soovitud oleku konfiguratsioon (DSC)](../virtual-machines/virtual-machines-windows-extensions-dsc-overview.md) laiendamine sündmuseohjuri saab kasutada [Virtuaalse masina skaala komplektid (VMSS)](virtual-machine-scale-sets-overview.md) . VMSS võimaldab juurutada ja hallata suure hulga virtuaalmasinates ja saate elastically mastaapimiseks sisse ja välja laadimine vastuseks. DSC saab konfigureerida VMs need veebis, nii et need töötavad tootmise tarkvara.

## <a name="differences-between-deploying-to-vm-and-vmss"></a>Erinevused VM ja VMSS juurutamine

Malli põhistruktuuri VMSS on pisut erinev ühe VM. Täpsemalt ühe VM kasutab sõlme "virtualMachines" all. On kande tüüp "laiendid", kus DSC lisatakse Mall

```
"resources": [
          {
              "name": "Microsoft.Powershell.DSC",
              "type": "extensions",
              "location": "[resourceGroup().location]",
              "apiVersion": "2015-06-15",
              "dependsOn": [
                  "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
              ],
              "tags": {
                  "displayName": "dscExtension"
              },
              "properties": {
                  "publisher": "Microsoft.Powershell",
                  "type": "DSC",
                  "typeHandlerVersion": "2.20",
                  "autoUpgradeMinorVersion": false,
                  "forceUpdateTag": "[parameters('dscExtensionUpdateTagVersion')]",
                  "settings": {
                      "configuration": {
                          "url": "[concat(parameters('_artifactsLocation'), '/', variables('dscExtensionArchiveFolder'), '/', variables('dscExtensionArchiveFileName'))]",
                          "script": "DscExtension.ps1",
                          "function": "Main"
                      },
                      "configurationArguments": {
                          "nodeName": "[variables('vmName')]"
                      }
                  },
                  "protectedSettings": {
                      "configurationUrlSasToken": "[parameters('_artifactsLocationSasToken')]"
                  }
              }
          }
      ]
```

VMSS sõlm on "atribuudid" jaotis "VirtualMachineProfile", "extensionProfile" atribuut abil. DSC lisatakse jaotises "laiendid"

```
"extensionProfile": {
            "extensions": [
                {
                    "name": "Microsoft.Powershell.DSC",
                    "properties": {
                        "publisher": "Microsoft.Powershell",
                        "type": "DSC",
                        "typeHandlerVersion": "2.20",
                        "autoUpgradeMinorVersion": false,
                        "forceUpdateTag": "[parameters('DscExtensionUpdateTagVersion')]",
                        "settings": {
                            "configuration": {
                                "url": "[concat(parameters('_artifactsLocation'), '/', variables('DscExtensionArchiveFolder'), '/', variables('DscExtensionArchiveFileName'))]",
                                "script": "DscExtension.ps1",
                                "function": "Main"
                            },
                            "configurationArguments": {
                                "nodeName": "localhost"
                            }
                        },
                        "protectedSettings": {
                            "configurationUrlSasToken": "[parameters('_artifactsLocationSasToken')]"
                        }
                    }
                }
            ]
```

## <a name="behavior-for-vmss"></a>VMSS käitumine

VMSS käitumine on identne ühe VM käitumine. Kui luuakse uus VM, on automaatselt ettevalmistatud DSC laiendiga. WMF uuemat versiooni vajadusel pikendamisega VM taaskäivitamisega enne online. Kui see on veebis, DSC konfiguratsiooni ZIP allalaaditavate failide ja selle ette VM. Lisateavet leiate [Azure'i DSC laiend ülevaade](../virtual-machines/virtual-machines-windows-extensions-dsc-overview.md).

## <a name="next-steps"></a>Järgmised sammud ##
Uurige [Azure'i ressursihaldur malli DSC pikendamise eest](../virtual-machines/virtual-machines-windows-extensions-dsc-template.md).

Siit saate teada, kuidas [DSC laiend turvaliselt tegeleb mandaat](../virtual-machines/virtual-machines-windows-extensions-dsc-credentials.md). 

Azure'i DSC laiend sündmuseohjuri kohta leiate lisateavet teemast [Azure soovitud riik konfiguratsiooni laiend sündmuseohjuri tutvustus](../virtual-machines/virtual-machines-windows-extensions-dsc-overview.md). 

PowerShelli DSC, [külastage PowerShelli dokumentatsiooni](https://msdn.microsoft.com/powershell/dsc/overview)kohta lisateavet. 


