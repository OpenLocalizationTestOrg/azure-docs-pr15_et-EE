<properties 
   pageTitle="Levinud PowerShelli käske vms | Microsoft Azure'i"
   description="Levinud PowerShelli käske, võite alustada loomise ja haldamise oma Windows Azure'i VMs"
   services="virtual-machines-windows"
   documentationCenter=""
   authors="davidmu1" 
   manager="timlt" 
   editor="tysonn" 
   tags="azure-resource-manager"/>
   
<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="09/27/2016"
   ms.author="davidmu" />

# <a name="common-powershell-commands-for-creating-and-managing-vms"></a>Levinud PowerShelli käske loomise ja haldamise VMs

Selles artiklis antakse ülevaade mõne Azure PowerShelli käske, mille abil saate luua ja hallata virtuaalmasinates teie Azure'i tellimus.  Üksikasjalikku abi teatud käsurealülitid ja suvanditega, saate kasutada **Abi saamiseks** *käsk*.

Vaadake, [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md) teavet Azure PowerShelli uusima versiooni installimise, valides tellimuse ja oma kontosse sisse logida.

## <a name="create-a-vm"></a>Luua VM

Ülesanne | Käsk
-------------- | -------------------------
VM konfiguratsiooni loomine | $vm = [New-AzureRmVMConfig](https://msdn.microsoft.com/library/mt603727.aspx) - VMName "vm_name" - VMSize "vm_size"<BR></BR><BR></BR>VM konfiguratsiooni kasutatakse määratlemine või VM sätete värskendamine. Konfiguratsiooni on lähtestatud VM ja selle [maht](virtual-machines-windows-sizes.md)nimi.
Konfiguratsiooni sätete lisamine | $vm = [Set-AzureRmVMOperatingSystem](https://msdn.microsoft.com/library/mt603843.aspx) - VM $vm-Windowsi - arvutinimi "arvuti_nimi"-mandaati $cred - ProvisionVMAgent - EnableAutoUpdate<BR></BR><BR></BR>Operatsioonisüsteemi sätted, sh [identimisteabe](https://technet.microsoft.com/library/hh849815.aspx) lisatakse konfiguratsiooni objekti varem loodud New-AzureRmVMConfig abil.
Võrgu liidese lisamine | $vm = [Lisada AzureRmVMNetworkInterface](https://msdn.microsoft.com/library/mt619351.aspx) - VM $vm-Id $nic. ID<BR></BR><BR></BR>VM peab olema [võrgu liidese](virtual-machines-windows-ps-create.md) virtuaalse võrgus suhelda. [Get-AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt619434.aspx) abil saate tuua olemasoleva võrgu kasutajaliidese objekti.
Määrake platvormi pilt | $vm = [Set-AzureRmVMSourceImage](https://msdn.microsoft.com/library/mt619344.aspx) - VM $vm - PublisherName "publisher_name" – pakub "publisher_offer" - SKU-de jaoks "product_sku"-"Viimane" versioon<BR></BR><BR></BR>[Pildi teave](virtual-machines-windows-cli-ps-findimage.md) lisatakse konfiguratsiooni objekti, mis on varem loodud New-AzureRmVMConfig abil. Tagastatud selle käsu objekt kasutatakse ainult siis, kui seate OS kettapuhastusriista kasutamiseks platvormi pilt.
Seadmine OS kettale platvormi pildi kasutamine | $vm = [Set-AzureRmVMOSDisk](https://msdn.microsoft.com/library/mt603746.aspx) - VM $vm-http://mystore1.blob.core.windows.net/vhds/disk_name.vhd"nimi"disk_name"- VhdUri" - CreateOption FromImage<BR></BR><BR></BR>Operatsioonisüsteemi ketta ja selle asukoha [laos](../storage/storage-powershell-guide-full.md) nimi on lisatud konfiguratsiooni objekt, mille olete varem loonud.
Seadmine OS kettapuhastusriista kasutamiseks generalized pilt | $vm = set-AzureRmVMOSDisk - VM $vm-https://mystore1.blob.core.windows.net/system/Microsoft.Compute/Images/myimages/myprefix-osDisk"nimi"disk_name"- SourceImageUri. {guid} .vhd"- VhdUri"https://mystore1.blob.core.windows.net/vhds/disk_name.vhd"- CreateOption FromImage-Windows<BR></BR><BR></BR>Operatsioonisüsteemi ketas, Lähtepildi asukoha ja ketta asukoha [salvestusruumi](../storage/storage-powershell-guide-full.md) nimi on lisatud konfiguratsiooni objekti.
Seadmine OS kettapuhastusriista kasutamiseks eriotstarbelisi pilt | $vm = set-AzureRmVMOSDisk - VM $vm-http://mystore1.blob.core.windows.net/vhds/"nimi"name_of_disk"- VhdUri" - CreateOption manustamine - Windows
Luua VM | [Uus-AzureRmVM]() - ResourceGroupName "resource_group_name"-asukoht "location_name" - VM $vm<BR></BR><BR></BR>Kõik ressursid on loodud [Ressursirühma](../powershell-azure-resource-manager.md). VM tuleb luua ressursirühma samasse [asukohta](https://msdn.microsoft.com/library/azure/dn495177.aspx) . Enne, kui käivitate selle käsu, käivitage uus-AzureRmVMConfig, Set-AzureRmVMOperatingSystem, Set-AzureRmVMSourceImage, lisa-AzureRmVMNetworkInterface ja Set-AzureRmVMOSDisk.

## <a name="get-information-about-vms"></a>VMs kohta teabe saamine

Ülesanne | Käsk
-------------- | -------------------------
Loendi VMs tellimus| [Get-AzureRmVM](https://msdn.microsoft.com/library/mt603718.aspx)
Loendi VMs ressursirühma | Get-AzureRmVM - ResourceGroupName "resource_group_name"<BR></BR><BR></BR>Teie tellimus on loendi saamiseks kasutage [Get-AzureRmResourceGroup](https://msdn.microsoft.com/library/mt679016.aspx).
VM kohta teabe saamine | Get-AzureRmVM - ResourceGroupName "resource_group_name"-nime "vm_name"

## <a name="manage-vms"></a>VMs haldamine

Ülesanne | Käsk
-------------- | -------------------------
Käivitage VM | [Algus-AzureRmVM](https://msdn.microsoft.com/library/mt603453.aspx) - ResourceGroupName "resource_group_name"-nime "vm_name"
VM peatamine | [Lõpeta – AzureRmVM](https://msdn.microsoft.com/library/mt603483.aspx) - ResourceGroupName "resource_group_name"-nime "vm_name"
Taaskäivitage käivitatud VM | [Uuesti-AzureRmVM](https://msdn.microsoft.com/library/mt603775.aspx) - ResourceGroupName "resource_group_name"-nime "vm_name"
VM kustutamine | [Eemalda – AzureRmVM](https://msdn.microsoft.com/library/mt603641.aspx) - ResourceGroupName "resource_group_name"-nime "vm_name"
Üldistada VM | [Set-AzureRmVm](https://msdn.microsoft.com/library/mt603688.aspx) - ResourceGroupName YourResourceGroup-nime "vm_name"-üldiste<BR></BR><BR></BR>Käivitage see käsk Salvesta-AzureRmVMImage enne.
Jäädvustada VM | [Salvesta-AzureRmVMImage](https://msdn.microsoft.com/library/mt619423.aspx) - ResourceGroupName "resource_group_name" - VMName "vm_name" - DestinationContainerName "image_container" - VHDNamePrefix "image_name_prefix"-tee "C:\filepath\filename.json"<BR></BR><BR></BR>Virtuaalse masina peab olema [sulgeda ja üldisi](virtual-machines-windows-generalize-vhd.md) pildi loomiseks kasutada. Enne, kui käivitate selle käsu, käivitage Set-AzureRmVm.
VM värskendamine | [Update-AzureRmVM](https://msdn.microsoft.com/library/mt603662.aspx) - ResourceGroupName "resource_group_name" - VM $vm<BR></BR><BR></BR>Saada abil Get-AzureRmVM VM konfiguratsioonile, muuta konfiguratsioonisätted VM objekti ja seejärel käivitage see käsk.
VM kettal andmete lisamine | [Lisa-AzureRmVMDataDisk](https://msdn.microsoft.com/library/mt603673.aspx) - VM $vm-https://mystore1.blob.core.windows.net/vhds/disk_name.vhd"nimi"disk_name"- VhdUri" - LUN #-vahemällu ReadWrite - DiskSizeinGB # - CreateOption tühi<BR></BR><BR></BR>Get-AzureRmVM abil VM objekti. Valige LUN ja ketta suurust. Käivitage Update-AzureRmVM konfiguratsiooni muudatuste rakendamiseks VM. Ketas, mille lisate on lähtestatud. Lähtestamisel ketast, mis lisatakse kohta leiate teavet teemast [haldamine Azure'i Virtuaalmasinates ressursihaldur ja PowerShelli abil](virtual-machines-windows-ps-manage.md).
VM andmed kettale eemaldamine | [Eemalda – AzureRmVMDataDisk](https://msdn.microsoft.com/library/mt603614.aspx) - VM $vm – nimi "disk_name"<BR></BR><BR></BR>Get-AzureRmVM abil VM objekti. Käivitage Update-AzureRmVM konfiguratsiooni muudatuste rakendamiseks VM.
VM laiend lisamine | [Set-AzureRmVMExtension](https://msdn.microsoft.com/library/mt603745.aspx) - ResourceGroupName "resource_group_name"-vm_name"asukoht"azure_location"- VMName"-nime "extension_name"-Publisher "publisher_name"-tüüp "extension_type" - TypeHandlerVersion "#. #"-sätted $Settings - ProtectedSettings $ProtectedSettings<BR></BR><BR></BR>Selle käsu korral [konfiguratsiooniteavet](virtual-machines-windows-extensions-configuration-samples.md) laiend, mida soovite installida.
Eemaldada VM laiendamine | [Eemalda – AzureRmVMExtension](https://msdn.microsoft.com/library/mt603782.aspx) - ResourceGroupName "resource_group_name"-vm_name"nimi"extension_name"- VMName"

## <a name="next-steps"></a>Järgmised sammud

- Lugege teemat põhitoimingud loomiseks virtuaalse masina [loomine Windowsi VM ressursihaldur ja PowerShelli abil](virtual-machines-windows-ps-create.md).

