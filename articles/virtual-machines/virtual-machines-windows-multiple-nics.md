<properties
   pageTitle="Luua mitu NICs Windows VM | Microsoft Azure'i"
   description="Siit saate teada, kuidas luua Windows VM Azure'i PowerShelli või ressursihaldur mallide kasutamine manustatud mitu NICs."
   services="virtual-machines-windows"
   documentationCenter=""
   authors="iainfoulds"
   manager="timlt"
   editor=""/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure"
   ms.date="10/27/2016"
   ms.author="iainfou"/>

# <a name="creating-a-windows-vm-with-multiple-nics"></a>Mitu NICs Windows VM loomine
Saate luua virtuaalse masina (VM) Azure, mis sisaldab mitme virtuaalse võrgu liidesed (NICs) oleks lisatud. Stsenaarium oleks teise alamvõrku ees- ja Ühenduvus või võrgus pühendunud jälgimise või varukoopia lahendus. See artikkel annab kiirülevaate käskude loomiseks lisatud mitu NICs VM. Üksikasjalikku teavet, sh kuidas luua mitu NICs sees oma PowerShelli skriptide, lugege lisateavet juurutamise [mitme-NIC VMs](../virtual-network/virtual-network-deploy-multinic-arm-ps.md). Erinevate [VM suurused](virtual-machines-windows-sizes.md) toetavad erineva arvu NICs, nii suurus vastavalt oma VM.

>[AZURE.WARNING] Kui loote VM - ei saa mõne olemasoleva VM NICs lisada mitu NICs siduda. Saate [luua VM põhjal algse virtuaalse ketta](virtual-machines-windows-vhd-copy.md) ja luua mitu NICs juurutamist VM.

## <a name="create-core-resources"></a>Looge core ressursid
Veenduge, et teil on [uusim Azure PowerShelli installinud ja konfigureerinud](../powershell-install-configure.md). Azure'i kontosse sisse logida:

```powershell
Login-AzureRmAccount
```

Järgmistes näidetes asendamine oma väärtustega näide parameetrite nimed. Näide parameetrite nimed sisalduv `myResourceGroup`, `mystorageaccount`, ja `myVM`.

Esmalt looge ressursirühma. Järgmises näites luuakse ressursirühma nimega `myResourceGroup` klõpsake soovitud `WestUs` asukoht:

```powershell
New-AzureRmResourceGroup -Name "myResourceGroup" -Location "WestUS"
```

Looge konto salvestusruumi hoida oma VMs. Järgmises näites luuakse salvestusruumi konto nimega `mystorageaccount`:

```powershell
$storageAcc = New-AzureRmStorageAccount -ResourceGroupName "myResourceGroup" `
    -Location "WestUS" -Name "mystorageaccount" `
    -Kind "Storage" -SkuName "Premium_LRS" 
```

## <a name="create-virtual-network-and-subnets"></a>Virtuaalse võrgu ja alamvõrku loomine
Määratleda kahe virtuaalse alamvõrku – üks ees liikluse ja üks tagaandmebaas liikluse. Järgmises näites määratleb kaks alamvõrku, nimega `mySubnetFrontEnd` ja `mySubnetBackEnd`:

```powershell
$mySubnetFrontEnd = New-AzureRmVirtualNetworkSubnetConfig -Name "mySubnetFrontEnd" `
    -AddressPrefix "192.168.1.0/24"
$mySubnetBackEnd = New-AzureRmVirtualNetworkSubnetConfig -Name "mySubnetBackEnd" `
    -AddressPrefix "192.168.2.0/24"
```

Saate luua oma virtuaalse võrgu ja alamvõrku. Järgmises näites luuakse virtuaalse võrgu nimega `myVnet`:

```powershell
$myVnet = New-AzureRmVirtualNetwork -ResourceGroupName "myResourceGroup" `
    -Location "WestUS" -Name "myVnet" -AddressPrefix "192.168.0.0/16" `
    -Subnet $mySubnetFrontEnd,$mySubnetBackEnd
```


## <a name="create-multiple-nics"></a>Mitu NICs loomine
Looge kaks NICs, manustamise üks NIC ees alamvõrgu asukoht ja ühe tagaandmebaas alamvõrgu. Järgmises näites luuakse kaks NICs, nimega `myNic1` ja `myNic2`:

```powershell
$frontEnd = $myVnet.Subnets|?{$_.Name -eq 'mySubnetFrontEnd'}
$myNic1 = New-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" `
    -Location "WestUS" -Name "myNic1" -SubnetId $frontEnd.Id

$backEnd = $myVnet.Subnets|?{$_.Name -eq 'mySubnetBackEnd'}
$myNic2 = New-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" `
    -Location "WestUS" -Name "myNic2" -SubnetId $backEnd.Id
```

Tavaliselt saate ka luua [võrgu turberühma](../virtual-network/virtual-networks-nsg.md) või [laadimine koormusetasakaalustusteenuse](../load-balancer/load-balancer-overview.md) haldamiseks ja levitamine liikluse oma VMs üle. [Üksikasjalikumat mitme-NIC VM](../virtual-network/virtual-network-deploy-multinic-arm-ps.md) artiklis juhatab teid läbi võrgu Turberühma loomine ja määramine NICs.


## <a name="create-the-virtual-machine"></a>Virtuaalse masina loomine
Kohe alustada oma VM konfigureerimine. Iga VM maht on NICs, mille saate lisada VM arv on piiratud. Lugege lisateavet [Windows VM suurused](virtual-machines-windows-sizes.md). 

Esmalt seatud kirjeldamine VM soovitud `$cred` muutuja järgmiselt:

```powershell
$cred = Get-Credential
```

Järgmises näites määratleb nimega VM `myVM` ja kasutab VM suurus, mis toetab kuni kahe NICs (`Standard_DS2_v2`):

```powershell
$vmConfig = New-AzureRmVMConfig -VMName "myVM" -VMSize "Standard_DS2_v2"
```

Saate luua oma VM config ülejäänud. Järgmises näites luuakse Windows Server 2012 R2 VM:

```powershell
$vmConfig = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName Te"MyVM" `
    -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
$vmConfig = Set-AzureRmVMSourceImage -VM $vmConfig -PublisherName "MicrosoftWindowsServer" `
    -Offer "WindowsServer" -Skus "2012-R2-Datacenter" -Version "latest"
```

Manustada varem loodud kahe NICs:

```powershell
$vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $myNic1.Id -Primary
$vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $myNic2.Id
```

Salvestusruum ja virtuaalse ketta konfigureerimine oma uue VM:

```powershell
$blobPath = "vhds/WindowsVMosDisk.vhd"
$osDiskUri = $storageAcc.PrimaryEndpoints.Blob.ToString() + $blobPath
$diskName = "windowsvmosdisk"
$vmConfig = Set-AzureRmVMOSDisk -VM $vmConfig -Name $diskName -VhdUri $osDiskUri `
    -CreateOption "fromImage"
```

Lõpuks luua VM.

```powershell
New-AzureRmVM -VM $vmConfig -ResourceGroupName "myResourceGroup" -Location "WestUS"
```

## <a name="creating-multiple-nics-using-resource-manager-templates"></a>Luua mitu NICs ressursihaldur mallide kasutamine
Azure'i ressursihaldur mallide abil saate määratleda keskkonna deklaratiivseid JSON-failid. Leiate [Azure'i ressursihaldur ülevaade](../azure-resource-manager/resource-group-overview.md). Ressursihaldur Mallid võimalda loomine ressursi mitu eksemplari, nt mitu NICs loomise ajal. *Kopeeri* abil saate määrata loomiseks eksemplaride arv:

```bash
"copy": {
    "name": "multiplenics"
    "count": "[parameters('count')]"
}
```

Lugege lisateavet [mitmes eksemplaris, kasutades *eksemplari*loomise](../resource-group-create-multiple.md)kohta. 

Saate kasutada ka mõne `copyIndex()` seejärel lisandada arvu ressursinimi, mis võimaldab teil luua `myNic1`, `MyNic2`jne. Järgmisel joonisel on kujutatud näide lisades indeksi väärtus:

```bash
"name": "[concat('myNic', copyIndex())]", 
```

Saate lugeda täielik näide [loomise mitu NICs ressursihaldur mallide kasutamine](../virtual-network/virtual-network-deploy-multinic-arm-template.md).

## <a name="next-steps"></a>Järgmised sammud
Veenduge, et vaadata [Windows VM suurused](virtual-machines-windows-sizes.md) , kui proovite luua VM mitu NICs. Pöörake tähelepanu iga VM suurus toetab NICs maksimaalne arv. 

Pidage meeles, et ei saa lisada täiendavad NICs mõne olemasoleva VM, peate looma kõik NICs VM juurutamisel. Olge ettevaatlik, kui plaanite oma juurutuste veenduge, et teil on kõik nõutavad võrguühendus algusest.