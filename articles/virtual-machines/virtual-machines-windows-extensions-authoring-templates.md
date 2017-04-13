<properties
   pageTitle="Mallide koostamine Windows VM laiendiga | Microsoft Azure'i"
   description="Lisateavet loome vms Windows Azure'i ressursihaldur Mallid laiendiga"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="kundanap"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="03/29/2016"
   ms.author="kundanap"/>

# <a name="authoring-azure-resource-manager-templates-with-windows-vm-extensions"></a>Loome laiendiga Windows VM Azure'i ressursihaldur Mallid

[AZURE.INCLUDE [virtual-machines-common-extensions-authoring-templates](../../includes/virtual-machines-common-extensions-authoring-templates.md)]

: Azure'i PowerShelli, käivitage järgmine Azure PowerShelli cmdlet-käsk:

      Get-AzureVMAvailableExtension


Selle cmdlet kuvab publisher nimi, laiend nimi ja versiooni järgmiselt:

      Publisher                   : Microsoft.Azure.Extensions  
      ExtensionName               : DockerExtension
      Version                     : 1.0

Kolm atribuutidest vastendamine "väljaandja", "tüüp" ja "typeHandlerVersion" vastavalt ülaltoodud malli koodilõigu sisse.

>[AZURE.NOTE]Soovitatav on alati kõige värskendatud funktsioonid laiend uusima versiooni abil.

## <a name="identifying-the-schema-for-the-extension-configuration-parameters"></a>Skeemi laiend konfiguratsiooni parameetrite tuvastamine

Järgmise juhise juurde, mille loome mõni laiend mall on selgitada vorming esitada konfiguratsiooni parameetrid. Iga laiend toetab oma komplekti parameetrid.

Vaadata Windowsi laiendid valimi konfiguratsioone, lugege teemat [Windowsi laiendid näidised](virtual-machines-windows-extensions-configuration-samples.md).


Vaadake järgmist saada täielikult täielik malli VM laiendiga.

[Kohandatud skript laiend Windows VM](https://github.com/Azure/azure-quickstart-templates/blob/b1908e74259da56a92800cace97350af1f1fc32b/201-list-storage-keys-windows-vm/azuredeploy.json/)


Pärast koostamine malli, saate juurutamist Azure PowerShelli kaudu.
