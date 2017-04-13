<properties
   pageTitle="Tõrkeotsing Linux VM laiend tõrked | Microsoft Azure'i"
   description="Lisateavet Azure Linux VM laiend tõrgete tõrkeotsing"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="kundanap"
   manager="timlt"
   editor=""
   tags="top-support-issue,azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="support-article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="03/29/2016"
   ms.author="kundanap"/>

# <a name="troubleshooting-azure-linux-vm-extension-failures"></a>Azure'i Linux VM laiend tõrgete tõrkeotsing

[AZURE.INCLUDE [virtual-machines-common-extensions-troubleshoot](../../includes/virtual-machines-common-extensions-troubleshoot.md)]

## <a name="viewing-extension-status"></a>Pikendamise oleku vaatamine
Azure'i ressursihaldur malle saab teostada Azure'i CLI kaudu. Kui mall on täidetud, saab vaadata pikendamise oleku Azure'i Resource Explorer või käsurea tööriistade.

Siin on näide:

Azure'i CLI:

      azure vm get-instance-view


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

## <a name="troubleshooting-extenson-failures"></a>Tõrkeotsing Extenson tõrked:

### <a name="re-running-the-extension-on-the-vm"></a>Pikendamise uuesti töötavate VM

Kui kasutate skriptide VM abil kohandatud skript laiend, võib mõnikord tekib tõrge, kui VM on loodud, kuid ei ole skripti. Nende tingimuste alusel on soovitatav viis taastada selle tõrke laiendamine eemaldamine ja uuesti malli uuesti.
Märkus: tulevikus selle funktsiooni soovite suurendada eemaldamiseks vajadus desinstallimise laiendamine.

#### <a name="remove-the-extension-from-azure-cli"></a>Azure'i CLI laiendamine eemaldamine

      azure vm extension set --resource-group "KPRG1" --vm-name "kundanapdemo" --publisher-name "Microsoft.Compute.CustomScriptExtension" --name "myCustomScriptExtension" --version 1.4 --uninstall

Kui "publsher nimi" vastab "Azure'i vm get-eksemplari-vaade" väljundi laiend tüüp ja nimi on laiend ressursi malli nimi

Pärast laiendamist on eemaldatud, saab uuesti sooritatud skripte VM mall.
