<properties
   pageTitle="Tõrkeotsing Windows VM laiend tõrked | Microsoft Azure'i"
   description="Lisateavet Azure Windows VM laiend tõrgete tõrkeotsing"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="kundanap"
   manager="timlt"
   editor=""
   tags="top-support-issue,azure-resource-manager"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="03/29/2016"
   ms.author="kundanap"/>

# <a name="troubleshooting-azure-windows-vm-extension-failures"></a>Azure'i Windows VM laiend tõrgete tõrkeotsing

[AZURE.INCLUDE [virtual-machines-common-extensions-troubleshoot](../../includes/virtual-machines-common-extensions-troubleshoot.md)]

## <a name="viewing-extension-status"></a>Pikendamise oleku vaatamine
Azure'i ressursihaldur malle saab teostada Azure PowerShelli kaudu. Kui mall on täidetud, saab vaadata pikendamise oleku Azure'i Resource Explorer või käsurea tööriistade.

Siin on näide:

Azure'i PowerShelli:

      Get-AzureRmVM -ResourceGroupName $RGName -Name $vmName -Status

Siin on näide väljund.

      Extensions:  {
      "ExtensionType": "Microsoft.Compute.CustomScriptExtension",
      "Name": "myCustomScriptExtension",
      "SubStatuses": [
        {
          "Code": "ComponentStatus/StdOut/succeeded",
          "DisplayStatus": "Provisioning succeeded",
          "Level": "Info",
          "Message": "    Directory: C:\\temp\\n\\n\\nMode                LastWriteTime     Length Name
              \\n----                -------------     ------ ----                              \\n-a---          9/1/2015   2:03 AM         11
              test.txt                          \\n\\n",
                      "Time": null
          },
        {
          "Code": "ComponentStatus/StdErr/succeeded",
          "DisplayStatus": "Provisioning succeeded",
          "Level": "Info",
          "Message": "",
          "Time": null
        }
    }
  ]

## <a name="troubleshooting-extension-failures"></a>Tõrkeotsing laiend tõrked

### <a name="re-running-the-extension-on-the-vm"></a>Pikendamise uuesti töötavate VM

Kui kasutate skriptide VM abil kohandatud skript laiend, võib mõnikord tekib tõrge, kui VM on loodud, kuid ei ole skripti. Nende tingimuste alusel on soovitatav viis taastada selle tõrke laiendamine eemaldamine ja uuesti malli uuesti.
Märkus: tulevikus selle funktsiooni soovite suurendada eemaldamiseks vajadus desinstallimise laiendamine.


#### <a name="remove-the-extension-from-azure-powershell"></a>Azure'i PowerShelli laiendamine eemaldamine

    Remove-AzureRmVMExtension -ResourceGroupName $RGName -VMName $vmName -Name "myCustomScriptExtension"

Pärast laiendamist on eemaldatud, saab uuesti sooritatud skripte VM mall.
