<properties
    pageTitle="Luua VM pilt on Azure VM | Microsoft Azure'i"
    description="Saate teada, kuidas luua mõne olemasoleva Azure VM loodud ressursihaldur juurutamise mudeli generalized VM pilt"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="cynthn"/>


# <a name="download-the-template-for-a-vm"></a>Selle malli allalaadimiseks VM

Kui loote VM Azure portaali või PowerShelli abil, on teie jaoks automaatselt loodud ressursihaldur malli. Selle malli abil saate kiiresti dubleerimine juurutamine. Mall sisaldab teavet kõigi ressursside kohta ressursirühma. Puhul virtuaalse masina, tähendab see mall ümbriste kõik, mis on loodud, et toetada VM ressursi rühma, sh võrgu ressursse.

## <a name="download-the-template-using-the-portal"></a>Portaalis malli allalaadimiseks

1. [Azure'i portaali](https://portal.azure.com/)sisse logida.
2. Üks jaoturi menüü, valige **Virtuaalmasinates**.
3. Valige loendist virtuaalse masina.
5. Valige **automatiseerimise skripti**.
6. Klõpsake nuppu **Laadi alla** ja salvestage ZIP-faili kohalikus arvutis.
7. Avage ZIP-faili ja väljavõte failid kausta. ZIP-fail sisaldab:
    
    - Deploy.ps1
    - Deploy.sh 
    - Deployer.RB
    - DeploymentHelper.cs
    - parameters.JSON
    - template.JSON

.Json fail on mall.
    
## <a name="download-the-template-using-powershell"></a>PowerShelli kasutamine malli allalaadimiseks

Samuti saate alla laadida .json mallifail cmdlet-käsu [Ekspordi-AzureRMResourceGroup](https://msdn.microsoft.com/library/mt715427.aspx) abil. Saate kasutada funktsiooni `-path` parameetri ette .json faili tee ja faili nimi. Selles näites kujutatakse malli ressursirühma nimega **myResourceGroup** **C:\users\public\downloads** kausta kohalikku arvutisse allalaadimiseks.

```powershell
    Export-AzureRmResourceGroup -ResourceGroupName "myResourceGroup" -Path "C:\users\public\downloads"
```

## <a name="next-steps"></a>Järgmised sammud

Juurutamine ressursid mallide kasutamise kohta leiate lisateavet teemast [ressursihaldur Mall ülevaade](../resource-manager-template-walkthrough.md).