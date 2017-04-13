<properties
    pageTitle="Luua VM generalized VHD | Microsoft Azure'i"
    description="Siit saate teada, kuidas luua virtuaalse masina Windows Azure PowerShelli, kasutades ressursihaldur juurutamise mudeli pildil oleva generalized VHD."
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

# <a name="create-a-vm-from-a-generalized-vhd-image"></a>Luua VM generalized VHD pildilt

Generalized VHD pilt on olnud kogu teie isikliku konto teabe [Sysprepi](virtual-machines-windows-generalize-vhd.md)abil eemaldada. Saate luua generalized VHD Sysprep kohapealse VM, mis seejärel [üleslaadimise VHD Azure](virtual-machines-windows-upload-image.md), käitades või käivitades Sysprep olemasoleva Azure VM ja seejärel [soovitud VHD kopeerimist](virtual-machines-windows-vhd-copy.md).

Kui soovite luua VM eriotstarbelisi VHD, lugege teemat [loomine VM eriotstarbelisi VHD kaudu](virtual-machines-windows-create-vm-specialized.md).

Kiireim viis generalized VHD VM loomiseks on kasutada [Lühijuhend malli] (https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image). 


## <a name="prerequisites"></a>Eeltingimused

Kui te ei kavatse kasutada VHD, mis on üles laaditud kohapealse VM, nagu üks loodud abil Hyper-V, veenduge, et olete täitnud [ettevalmistamine Windows VHD üleslaadimiseks Azure'i](virtual-machines-windows-prepare-for-upload-vhd-image.md)juhiseid. 

Üleslaaditud VHDs nii olemasolevate Azure'i VM VHDs vaja üldiste, enne kui saate luua VM selle meetodi kasutamise. Lisateavet leiate teemast [Generalize Windowsi virtuaalse masina Sysprepi abil](virtual-machines-windows-generalize-vhd.md). 


## <a name="set-the-uri-of-the-vhd"></a>URI on VHD seadmine

URI VHD kasutamiseks on vormingus: https://**mystorageaccount**.blob.core.windows.net/**mycontainer**/**MyVhdName**.vhd. Selles näites on VHD nimega **myVHD** on salvestusruumi konto **mystorageaccount** container **mycontainer**.

```powershell
$imageURI = "https://mystorageaccount.blob.core.windows.net/mycontainer/myVhd.vhd"
```


## <a name="create-a-virtual-network"></a>Virtuaalse võrgu loomine

VNet ja alamvõrgu [virtuaalse võrgu](../virtual-network/virtual-networks-overview.md)loomine.


1. Looge alamvõrgu. Järgmises näites luuakse alamvõrku, mis on nimega **mySubnet** ressursi rühma **myResourceGroup** aadress eesliide **10.0.0.0/24**sisse.  

    ```powershell
    $rgName = "myResourceGroup"
    $subnetName = "mySubnet"
    $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```
      
2. Saate luua virtuaalse võrgu. Järgmises näites luuakse virtuaalse võrgu nimega **myVnet** **10.0.0.0/16**aadress eesliitega asukohas **Lääne US** .  

    ```powershell
    $location = "West US"
    $vnetName = "myVnet"
    $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    
            
## <a name="create-a-public-ip-address-and-network-interface"></a>Avaliku IP-aadress ja võrgu kasutajaliidese loomine

Suhtlus virtuaalse masina virtuaalse võrgu lubamiseks peate [avaliku IP-aadress](../virtual-network/virtual-network-ip-addresses-overview-arm.md) ja võrgu liidese.

1. Luua avaliku IP-aadressi. Selles näites luuakse nimega **myPip**avaliku IP-aadress. 

    ```powershell
    $ipName = "myPip"
    $pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location -AllocationMethod Dynamic
    ```       

2. NIC. loomine Selles näites luuakse NIC, mis on nimega **myNic**. 

    ```powershell
    $nicName = "myNic"
    $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName -Location $location -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
    ```

## <a name="create-the-network-security-group-and-an-rdp-rule"></a>Turberühma võrgu ja RDP reegli loomine

Logige sisse oma VM abil RDP saama, peate turvalisus reegel, mis lubab juurdepääsu RDP port 3389. 

Selles näites loob mõne NSG **myNsg** nimega **myRdpRule** , mis võimaldab RDP liikluse üle pordi 3389 reegli sisaldava nimega. NSGs kohta leiate lisateavet teemast [portide avamise VM Azure PowerShelli kaudu](virtual-machines-windows-nsg-quickstart-powershell.md).

```powershell
$nsgName = "myNsg"

$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389

$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
```


## <a name="create-a-variable-for-the-virtual-network"></a>Muutuja virtuaalse võrgu loomine

Saate luua muutuja lõplikus virtuaalse võrgu jaoks. 

```powershell
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name $vnetName
```

## <a name="create-the-vm"></a>VM loomine

Järgmist PowerShelli skripti näitab, kuidas häälestada virtuaalse masina konfiguratsioone ja üleslaaditud VM pilt allikana uue installimiseks kasutada.

</br>


```powershell
    # Enter a new user name and password to use as the local administrator account 
    # for remotely accessing the VM.
    $cred = Get-Credential
    
    # Name of the storage account where the VHD is located. This example sets the 
    # storage account name as "myStorageAccount"
    $storageAccName = "myStorageAccount"
    
    # Name of the virtual machine. This example sets the VM name as "myVM".
    $vmName = "myVM"
    
    # Size of the virtual machine. This example creates "Standard_D2_v2" sized VM. 
    # See the VM sizes documentation for more information: 
    # https://azure.microsoft.com/documentation/articles/virtual-machines-windows-sizes/
    $vmSize = "Standard_D2_v2"
    
    # Computer name for the VM. This examples sets the computer name as "myComputer".
    $computerName = "myComputer"
    
    # Name of the disk that holds the OS. This example sets the 
    # OS disk name as "myOsDisk"
    $osDiskName = "myOsDisk"
    
    # Assign a SKU name. This example sets the SKU name as "Standard_LRS"
    # Valid values for -SkuName are: Standard_LRS - locally redundant storage, Standard_ZRS - zone redundant storage, Standard_GRS - geo redundant storage, Standard_RAGRS - read access geo redundant storage, Premium_LRS - premium locally redundant storage. 
    $skuName = "Standard_LRS"
    
    # Get the storage account where the uploaded image is stored
    $storageAcc = Get-AzureRmStorageAccount -ResourceGroupName $rgName -AccountName $storageAccName
    
    # Set the VM name and size
    $vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize
    
    #Set the Windows operating system configuration and add the NIC
    $vm = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName $computerName `
        -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
    
    # Create the OS disk URI
    $osDiskUri = '{0}vhds/{1}-{2}.vhd' `
        -f $storageAcc.PrimaryEndpoints.Blob.ToString(), $vmName.ToLower(), $osDiskName
    
    # Configure the OS disk to be created from the existing VHD image (-CreateOption fromImage).
    $vm = Set-AzureRmVMOSDisk -VM $vm -Name $osDiskName -VhdUri $osDiskUri `
        -CreateOption fromImage -SourceImageUri $imageURI -Windows
    
    # Create the new VM
    New-AzureRmVM -ResourceGroupName $rgName -Location $location -VM $vm
```

## <a name="verify-that-the-vm-was-created"></a>Veenduge, et VM loodi 

Kui olete lõpetanud, peaksite nägema vastloodud VM [Azure portaali](https://portal.azure.com) jaotises **Sirvi** > **virtuaalmasinates**, või kasutades PowerShelli järgmised käsud:

```powershell
    $vmList = Get-AzureRmVM -ResourceGroupName $rgName
    $vmList.Name
```

## <a name="next-steps"></a>Järgmised sammud

Virtuaalne seadme Azure'i PowerShelliga haldamiseks vaadake teemat [haldamine virtuaalmasinates Azure'i ressursihaldur ja PowerShelli abil](virtual-machines-windows-ps-manage.md).


