<properties
    pageTitle="Suuruse muutmine Windows VM | Microsoft Azure'i"
    description="Suuruse muutmine Windowsi virtuaalse masina loodud ressursihaldur juurutamise mudeli Azure PowerShelli kaudu."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="Drewm3"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="drewm"/>

    
# <a name="resize-a-windows-vm"></a>Windowsiga VM suuruse muutmine

Selles artiklis kirjeldatakse suuruse muutmise Windows VM, mis on loodud ressursihaldur juurutamise mudelis Azure PowerShelli kaudu.

Kui olete loonud virtuaalse masina (VM), saate muudate VM üles või alla, muutes VM suurus. Mõnel juhul peab deallocate VM esmalt. See võib juhtuda siis, kui uue suuruse pole saadaval, klõpsake riistvara kobar, mis on praegu hosting VM.

## <a name="resize-a-windows-vm-not-in-an-availability-set"></a>Windows VM ei kättesaadavus ematermin suuruse muutmine

1. Loendis Saadaolevad riistvara kobar, kus on majutatud VM VM suurused. 

    ```powershell
    Get-AzureRmVMSize -ResourceGroupName <resourceGroupName> -VMName <vmName> 
    ```

2. Kui loendis on soovitud suuruse, käivitage järgmised käsud suuruse VM. Kui soovitud loendis pole, minge samm 3.

    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <resourceGroupName> -VMName <vmName>
    $vm.HardwareProfile.VmSize = "<newVMsize>"
    Update-AzureRmVM -VM $vm -ResourceGroupName <resourceGroupName>
    ```

3. Kui soovitud suurus pole loendis, käivitage järgmised käsud Deallocate VM, selle suurust muuta ja taaskäivitage VM.

    ```powershell
    $rgname = "<resourceGroupName>"
    $vmname = "<vmName>"
    Stop-AzureRmVM -ResourceGroupName $rgname -VMName $vmname -Force
    $vm = Get-AzureRmVM -ResourceGroupName $rgname -VMName $vmname
    $vm.HardwareProfile.VmSize = "<newVMSize>"
    Update-AzureRmVM -VM $vm -ResourceGroupName $rgname
    Start-AzureRmVM -ResourceGroupName $rgname -Name $vmname
    ```

> [AZURE.WARNING] Deallocating VM välja mis tahes dünaamiline IP-aadresside määratud VM. Ketast OS- ja see ei mõjuta. 

## <a name="resize-a-windows-vm-in-an-availability-set"></a>Windows VM on kättesaadavus komplekti suuruse muutmine

Kui uue suuruse VM on kättesaadavus komplekti ei ole saadaval riistvara klaster praegu hosting VM, siis kõik VMs kättesaadavus määramine peate olema deallocated suuruse VM. Ka võib peate värskendama teiste VMs mahu kättesaadavus: määrake pärast ühe VM suurust muuta. Mõne kättesaadavus komplekti VM suuruse muutmiseks tehke järgmist.

1. Loendis Saadaolevad riistvara kobar, kus on majutatud VM VM suurused.

    ```powershell
    Get-AzureRmVMSize -ResourceGroupName <resourceGroupName> -VMName <vmName>
    ```

2. Kui loendis on soovitud suuruse, käivitage järgmised käsud suuruse VM. Kui see on loetletud, jätkake 3.

    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <resourceGroupName> -VMName <vmName>
    $vm.HardwareProfile.VmSize = "<newVmSize>"
    Update-AzureRmVM -VM $vm -ResourceGroupName <resourceGroupName>
    ```

3. Kui soovitud loendis pole, jätkake deallocate kõik VMs kättesaadavus määramine, suuruse VMs ja taaskäivitage neid tehke järgmist.

4.  Peatage kõik VMs kättesaadavus määramine.

    ```powershell
    $rg = "<resourceGroupName>"
    $as = Get-AzureRmAvailabilitySet -ResourceGroupName $rg
    $vmIds = $as.VirtualMachinesReferences
    foreach ($vmId in $vmIDs){
        $string = $vmID.Id.Split("/")
        $vmName = $string[8]
        Stop-AzureRmVM -ResourceGroupName $rg -Name $vmName -Force
    } 
    ```
              
5.  Suuruse muutmine ja uuesti VMs kättesaadavus määramine.

    ```powershell
    $rg = "<resourceGroupName>"
    $newSize = "<newVmSize>"
    $as = Get-AzureRmAvailabilitySet -ResourceGroupName $rg
    $vmIds = $as.VirtualMachinesReferences
    foreach ($vmId in $vmIDs){
        $string = $vmID.Id.Split("/")
        $vmName = $string[8]
        $vm = Get-AzureRmVM -ResourceGroupName $rg -Name $vmName
        $vm.HardwareProfile.VmSize = $newSize
        Update-AzureRmVM -ResourceGroupName $rg -VM $vm
        Start-AzureRmVM -ResourceGroupName $rg -Name $vmName
    }
    ```

## <a name="next-steps"></a>Järgmised sammud

- Täiendavad skaleeritavus, käivitage VM mitmes eksemplaris ja mastaapimiseks välja. Lisateabe saamiseks lugege teemat [automaatselt mastaapida Windowsi masinad virtuaalse masina skaala määramine](../virtual-machine-scale-sets/virtual-machine-scale-sets-windows-autoscale.md).



