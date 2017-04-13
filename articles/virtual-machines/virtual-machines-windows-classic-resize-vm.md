<properties
    pageTitle="Klassikalise Windowsi VM suuruse | Microsoft Azure'i"
    description="Suuruse muutmine Windowsi virtuaalse masina loodud klassikaline juurutamise mudeli, Azure PowerShelli kaudu."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="Drewm3"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="drewm"/>


# <a name="resize-a-windows-vm-created-in-the-classic-deployment-model"></a>Klassikaline juurutamise mudeli loodud Windows VM suuruse muutmine

Selles artiklis kirjeldatakse, kuidas suuruse muutmiseks Windows VM, klassikaline juurutamise mudeli Azure PowerShelli abil loodud.

Kui arutate võimalus VM suurust, on lahtrivahemik, läiked suuruse virtuaalse masina kontrollivate mõistetest. Esimene on piirkond, kus teie VM juurutatakse. Loendi regioonis saadaval VM suurused on Azure piirkondade veebilehe vahekaardil teenused. Teise on praegu majutada oma VM füüsilise riistvara. Füüsilise serverid hosting VMs rühmitatakse rühmades levinud füüsilise riistvara. VM suuruse muutmise meetod erineb sõltuvalt kui VM uus soovitud suurus toetab praegu hosting VM riistvara klaster.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Saate ka [loodud ressursihaldur juurutamise mudeli VM suurust](virtual-machines-windows-resize-vm.md).


## <a name="add-your-account"></a>Konto lisamine

Tuleb konfigureerida Azure PowerShelli töötamiseks klassikaline Azure ressursse. Järgige juhiseid saate konfigureerida Azure PowerShelli klassikalises ressursside haldamiseks.

1. Tippige käsureale PowerShelli `Add-AzureAccount` ja klõpsake nuppu **Sisesta**. 
2. Tippige oma Azure tellimusega seostatud meiliaadress ja klõpsake nuppu **Jätka**. 
3. Tippige oma konto parool. 
4. Klõpsake nuppu **Logi sisse**. 


## <a name="resize-in-the-same-hardware-cluster"></a>Sama riistvara klaster suuruse muutmine

Saadaval riistvara klaster majutusteenuse VM suurus VM suuruse muutmiseks tehke järgmist.

1. Käivitage järgmine käsk PowerShelli saadaolevad riistvara klaster majutusteenuse pilveteenusesse, mis sisaldab VM VM suurused.

    ```powershell
    Get-AzureService | where {$_.ServiceName -eq "<cloudServiceName>"}
    ```

2. Käivitage järgmised käsud suuruse VM.

    ```powershell
    Get-AzureVM -ServiceName <cloudServiceName> -Name <vmName> | Set-AzureVMSize -InstanceSize <newVMSize> | Update-AzureVM
    ```

## <a name="resize-on-a-new-hardware-cluster"></a>Suuruse muutmiseks klõpsake uue riistvara kobar

Suuruse muutmine ei ole saadaval riistvara klaster majutusteenuse VM, pilveteenuses suurus VM luuakse uuesti teenuseid ja kõik VMs pilveteenusesse. Iga pilveteenuses majutatakse ühe riistvara kobar nii, et kõik VMs pilveteenusesse, peab olema suurus, mis on toetatud riistvara klaster. Järgmised toimingud kirjeldab, kuidas kustutada, ja seejärel taasloomine pilveteenusesse VM suuruse.

1. Käivitage järgmine käsk PowerShelli piirkonna saadaolevad VM suurused. 

    ```powershell
    Get-AzureLocation | where {$_.Name -eq "<locationName>"}
    ```

2. Märkige üles kõik konfiguratsioonisätted iga VM pilveteenuses, mis sisaldab VM suurust muuta. 
3. Kustutage kõik VMs suvandi valimine soovitud ketast säilitamise jaoks iga VM pilveteenuses.
4. Looge VM abil soovitud VM suurus suurust.
5. Looge kõik VMs, mis olid abil VM suurus saadaval kohe majutusteenuses cloud riistvara klaster pilveteenuses.

Valimi skript kustutamine ja taasloomine abil uue VM suurus pilveteenus leiate [siit](https://github.com/Azure/azure-vm-scripts). 


## <a name="next-steps"></a>Järgmised sammud

- [Suuruse muutmine loodud ressursihaldur juurutamise mudeli VM](virtual-machines-windows-resize-vm.md).