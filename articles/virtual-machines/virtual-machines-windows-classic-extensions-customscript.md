<properties
   pageTitle="Kohandatud skript laiend Windows VM | Microsoft Azure'i"
   description="Toimingute automatiseerimine Azure VM konfiguratsiooni, kasutades kohandatud skript laiend PowerShelli skriptide käitamiseks remote Windows VM"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="kundanap"
   manager="timlt"
   editor=""
   tags="azure-service-management"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="08/06/2015"
   ms.author="kundanap"/>

# <a name="custom-script-extension-for-windows-virtual-machines"></a>Kohandatud skript laiend Windows virtuaalmasinates

Selles artiklis antakse ülevaade kohandatud skript laiend kasutamine Windows VMs Azure'i teenuse haldus API-de Azure PowerShelli cmdlet-käskude abil.

Virtuaalse masina (VM) laiendid on loodud, Microsoft ja usaldusväärsed kolmanda osapoole tootjad laiendamiseks VM. VM laiendid ülevaate leiate teemast [Azure VM laiendid ja funktsioonid](virtual-machines-windows-extensions-features.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Siit saate teada, kuidas [teha järgmist ressursihaldur mudeli abil](virtual-machines-windows-extensions-customscript.md).

## <a name="custom-script-extension-overview"></a>Kohandatud skript laiend ülevaade

Windows Script kohandatud laiendiga käivitada PowerShelli skriptide remote VM ilma teenusesse selle. Saate käivitada skriptide pärast ettevalmistamise VM või VM elutsükli igal ajal mis tahes täiendavaid pordid avamata. Kasutada enamikul juhtudel töötab kohandatud skript laiend kaasata töötab, installimine ja konfigureerimine VM täiendavat tarkvara, kui see on ette valmistatud.

### <a name="prerequisites-for-running-the-custom-script-extension"></a>Kohandatud skript laiend kasutamise eeltingimused

1. Installige <a href="http://azure.microsoft.com/downloads" target="_blank">Azure'i PowerShelli cmdlet-käskude</a> 0.8.0 versioon või uuem versioon.
2. Kui soovite skriptide käitamiseks mõne olemasoleva VM, veenduge, et VM Agent on lubatud VM. Kui see on installitud, järgige neid [juhiseid](virtual-machines-windows-classic-agents-and-extensions.md). Kui VM Azure portaali kaudu, siis VM Agent vaikimisi installitakse.
3. Skriptide käitamiseks VM Azure Storage soovitud laadi. Skriptid võivad pärineda ümbris ühe või mitme salvestusruumi ümbriste.
4. Skripti peaks Autor nii, et kirje skripti, mis käivitatakse laiendamine, käivitab teiste skripte.

## <a name="custom-script-extension-scenarios"></a>Kohandatud skript laiend stsenaariumid

### <a name="upload-files-to-the-default-container"></a>Vaike-ümbrisest failide üleslaadimine

Järgmises näites on kujutatud, kuidas käivitada oma skriptide VM kui need on vaikimisi konto tellimuse salvestusruumi ümbrises. Oma skriptide üleslaadimiseks ümbrisenimi. Abil saate kontrollida salvestusruumi vaikekonto soovitud **Get-AzureSubscription – vaikimisi** käsk.

Järgmises näites luuakse VM, kuid saate kasutada ka sama stsenaariumi mõne olemasoleva VM.

    # Create a new VM in Azure.
    $vm = New-AzureVMConfig -Name $name -InstanceSize Small -ImageName $imagename
    $vm = Add-AzureProvisioningConfig -VM $vm -Windows -AdminUsername $username -Password $password
    // Add Custom Script extension to the VM. The container name refers to the storage container that contains the file.
    $vm = Set-AzureVMCustomScriptExtension -VM $vm -ContainerName $container -FileName 'start.ps1'
    New-AzureVM -ServiceName $servicename -Location $location -VMs $vm
    #  After the VM is created, the extension downloads the script from the storage location and executes it on the VM.

    # Viewing the  script execution output.
    $vm = Get-AzureVM -ServiceName $servicename -Name $name
    # Use the position of the extension in the output as index.
    $vm.ResourceExtensionStatusList[i].ExtensionSettingStatus.SubStatusList

### <a name="upload-files-to-a-non-default-storage-container"></a>Failide üleslaadimine-vaikimisi salvestusruumi container

See stsenaarium näitab, kuidas kasutada-vaikimisi salvestusruumi container ühe tellimuse või mõnda muud tellimust skripte ja failide üleslaadimine. Selles näites on kujutatud mõne olemasoleva VM, kuid samu toiminguid saab teha, kui loote VM.

        Get-AzureVM -Name $name -ServiceName $servicename | Set-AzureVMCustomScriptExtension -StorageAccountName $storageaccount -StorageAccountKey $storagekey -ContainerName $container -FileName 'file1.ps1','file2.ps1' -Run 'file.ps1' | Update-AzureVM

### <a name="upload-scripts-to-multiple-containers-across-different-storage-accounts"></a>Mitme ümbriste skriptide üleslaadimine üle erinevate salvestusruumi kontod

  Kui skripti faile talletatakse üle mitme ümbriste, tuleb sisestada ühiskasutusega täielik juurdepääs allkirjad (SAS) URL-i failid skriptide käitamiseks.

      Get-AzureVM -Name $name -ServiceName $servicename | Set-AzureVMCustomScriptExtension -StorageAccountName $storageaccount -StorageAccountKey $storagekey -ContainerName $container -FileUri $fileUrl1, $fileUrl2 -Run 'file.ps1' | Update-AzureVM


### <a name="add-the-custom-script-extension-from-the-azure-portal"></a>Lisada kohandatud skript laiend Azure'i portaalis

Minge VM <a href="https://portal.azure.com/ " target="_blank">Azure portaali</a> ja laiendamine lisada, määrates skriptifail käivitamiseks.

  ![Määrake soovitud skriptifail][5]


### <a name="uninstall-the-custom-script-extension"></a>Kohandatud skript laiend desinstallimine

Järgmise käsu abil saate kohandatud skript laiend eemaldada VM.

      get-azureVM -ServiceName KPTRDemo -Name KPTRDemo | Set-AzureVMCustomScriptExtension -Uninstall | Update-AzureVM

### <a name="use-the-custom-script-extension-with-templates"></a>Kohandatud skript laiend mallide kasutamine

Kohandatud skript laiend Azure'i ressursihaldur malle kasutamise kohta leiate teemast [kasutades kohandatud skript pikendamist Windows vms Azure'i ressursihaldur mallide abil](virtual-machines-windows-extensions-customscript.md).

<!--Image references-->
[5]: ./media/virtual-machines-windows-classic-extensions-customscript/addcse.png
