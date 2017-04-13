<properties
    pageTitle="Looge oma Windows VM koopia | Microsoft Azure'i"
    description="Saate teada, kuidas luua oma eripärase Azure VM Windowsiga ressursihaldur juurutamise mudeli koopia."
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
    ms.date="09/21/2016"
    ms.author="cynthn"/>

# <a name="create-a-vm-from-a-specialized-vhd"></a>Eriotstarbelisi VHD VM loomine

Luua uue VM, lisades eriotstarbelisi VHD OS ketas PowerShelli kaudu. Eriotstarbelisi VHD säilitab Kasutajakontod, rakenduste ja muude olekus andmete oma algse VM. 

Kui soovite luua VM generalized VHD, lugege teemat [loomine generalized VHD pildilt VM](virtual-machines-windows-create-vm-generalized.md).

## <a name="create-the-subnet-and-vnet"></a>Alamvõrgu ja vNet loomine

VNet ja alamvõrgu [virtuaalse võrgu](../virtual-network/virtual-networks-overview.md)loomine.

1. Looge alamvõrgu. Selles näites loob alamvõrku, mis on nimega **mySubNet**, klõpsake ressursi rühma **myResourceGroup**ja määrab alamvõrgu aadress eesliide **10.0.0.0/24**.

    ```powershell
    $rgName = "myResourceGroup"
    $subnetName = "mySubNet"
    $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```

2. Saate luua soovitud vNet. Selles näites määrab virtuaalne võrgu nimi **myVnetName**, asukoht, kuhu soovite **Lääne USA**ja aadress eesliide **10.0.0.0/16**virtuaalse võrgu jaoks. 

    ```powershell
    $location = "West US"
    $vnetName = "myVnetName"
    $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    
            
## <a name="create-a-public-ip-address-and-nic"></a>Soovitud asukoht ja avaliku IP-aadressi loomine

Suhtlus virtuaalse masina virtuaalse võrgu lubamiseks peate [avaliku IP-aadress](../virtual-network/virtual-network-ip-addresses-overview-arm.md) ja võrgu liidese.

1. Saate luua avaliku IP. Selles näites avaliku IP-aadressi nimi on seatud **myIP**.

    ```powershell
    $ipName = "myIP"
    $pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location -AllocationMethod Dynamic
    ```       

2. NIC. loomine Selles näites NIC nimi on seatud **myNicName**.

    ```powershell
    $nicName = "myNicName"
    $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName -Location $location -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
    ```

## <a name="create-the-network-security-group-and-an-rdp-rule"></a>Turberühma võrgu ja RDP reegli loomine

Logige sisse oma VM abil RDP saama, peate turvalisuse reegel, mis lubab juurdepääsu RDP port 3389. Kuna olemasoleva on loodud uue VM VHD eriotstarbelisi VM pärast VM loomist saate kasutada andmeallika virtuaalse masina, mis oli sisselogimiseks kasutada RDP olemasolev konto.

Selles näites määrab NSG nime **myNsg** ja **myRdpRule**RDP reegli nimi.

```powershell
$nsgName = "myNsg"

$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389

$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
```

Lõpp-punktid ja NSG reeglite kohta leiate lisateavet teemast [portide avamise VM Azure PowerShelli kaudu](virtual-machines-windows-nsg-quickstart-powershell.md).

## <a name="create-the-vm-configuration"></a>Looge VM konfigureerimine

Häälestada VM konfiguratsiooni manustada kopeeritud VHD OS VHD nimega.


```powershell
    # Set the URI for the VHD that you want to use. In this example, the VHD file named "myOsDisk.vhd" is kept in a storage account named "myStorageAccount" in a container named "myContainer".
    $osDiskUri = "https://myStorageAccount.blob.core.windows.net/myContainer/myOsDisk.vhd"
    
    #Set the VM name and size. This example sets the VM name to "myVM" and the VM size to "Standard_A2".
    $vmName = "myVM"
    $vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize "Standard_A2"

    #Add the NIC
    $vm = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic.Id

    #Add the OS disk by using the URL of the copied OS VHD. In this example, when the OS disk is created, the term "osDisk" is appened to the VM name to create the OS disk name. This example also specifies that this Windows-based VHD should be attached to the VM as the OS disk.
    $osDiskName = $vmName + "osDisk"
    $vm = Set-AzureRmVMOSDisk -VM $vm -Name $osDiskName -VhdUri $osDiskUri -CreateOption attach -Windows
```


Kui teil on andmete ketast, mis tuleb lisada VM, peaksite lisama järgmist: 

```powershell
    # Optional: Add data disks by using the URLs of the copied data VHDs at the appropriate Logical Unit Number (Lun).
    $dataDiskName = $vmName + "dataDisk"
    $vm = Add-AzureRmVMDataDisk -VM $vm -Name $dataDiskName -VhdUri $dataDiskUri -Lun 0 -CreateOption attach
```

Andmed ja operatsioonisüsteemi ketta URL-ide nägema umbes järgmine: `https://StorageAccountName.blob.core.windows.net/BlobContainerName/DiskName.vhd`. Saate seda portaalis leida sirvides target salvestusruumi ümbris, klõpsates operatsioonisüsteemi või VHD kopeeritud andmed ja seejärel kopeerige URL-i sisu.


## <a name="create-the-vm"></a>VM loomine

Looge abil kuvatakse me äsja loodud VM.

```powershell
#Create the new VM
New-AzureRmVM -ResourceGroupName $rgName -Location $location -VM $vm
```

Kui see käsk õnnestus, näete väljundi umbes järgmine:

```
RequestId IsSuccessStatusCode StatusCode ReasonPhrase
--------- ------------------- ---------- ------------
                         True         OK OK   
 
```
 
## <a name="verify-that-the-vm-was-created"></a>Veenduge, et VM loodi 
 
Peaksite nägema vastloodud VM ühte [Azure portaali](https://portal.azure.com)jaotises **Sirvi** > **virtuaalmasinates**, või kasutades PowerShelli järgmised käsud:

```powershell
    $vmList = Get-AzureRmVM -ResourceGroupName $rgName
    $vmList.Name
```

## <a name="next-steps"></a>Järgmised sammud

Virtuaalne seadme sisse logida, liikuge VM [portaali](https://portal.azure.com), nuppu **Ühenda**ja kaugtöölaua RDP-faili avada. Kasutage konto identimisteave algse virtual arvuti virtuaalne seadme sisse logida. Lisateavet leiate teemast [ühenduse loomiseks ja kus töötab Windows Azure'i virtuaalarvuti sisse logida](virtual-machines-windows-connect-logon.md).







