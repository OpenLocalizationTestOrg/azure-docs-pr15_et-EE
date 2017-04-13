<properties
   pageTitle="Loome Mallid Linux VM laiendiga | Microsoft Azure'i"
   description="Lisateavet loome Azure'i ressursihaldur Mallid laiendiga Linux vms"
   services="virtual-machines-linux"
   documentationCenter=""
   authors="kundanap"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="03/29/2016"
   ms.author="kundanap"/>

# <a name="authoring-azure-resource-manager-templates-with-linux-vm-extensions"></a>Loome Linux VM laiendiga Azure'i ressursihaldur Mallid

[AZURE.INCLUDE [virtual-machines-common-extensions-authoring-templates](../../includes/virtual-machines-common-extensions-authoring-templates.md)]

: Azure'i CLI, käivitage järgmised commnad:

      Azure VM extension list

See käsk tagastab Publisheri nimi, laiend nimi ja versiooni järgmiselt:

      Publisher                   : Microsoft.Azure.Extensions  
      ExtensionName               : DockerExtension
      Version                     : 1.0

Kolm atribuutidest vastendamine "väljaandja", "tüüp" ja "typeHandlerVersion" vastavalt ülaltoodud malli koodilõigu sisse.

>[AZURE.NOTE]Soovitatav on alati kõige värskendatud funktsioonid laiend uusima versiooni abil.

## <a name="identifying-the-schema-for-the-extension-configuration-parameters"></a>Skeemi laiend konfiguratsiooni parameetrite tuvastamine

Järgmise juhise juurde, mille loome mõni laiend mall on selgitada vorming esitada konfiguratsiooni parameetrid. Iga laiend toetab oma komplekti parameetrid.

Vaadata valimi konfiguratsioone Linux laiendid, klõpsake dokumentatsiooni kohta leiate [Linux eExtensions näidised](virtual-machines-linux-extensions-configuration-samples.md).

Vaadake järgmist saada täielikult täielik malli VM laiendiga.

[Kohandatud skript laienduse Linux VM](https://github.com/Azure/azure-quickstart-templates/blob/b1908e74259da56a92800cace97350af1f1fc32b/mongodb-on-ubuntu/azuredeploy.json/)

Pärast koostamine malli, saate juurutada Azure CLI abil.
