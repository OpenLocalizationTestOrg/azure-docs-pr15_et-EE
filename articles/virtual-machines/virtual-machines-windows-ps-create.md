<properties
    pageTitle="Azure'i VM, mis PowerShelli abil luua | Microsoft Azure'i"
    description="PowerShelli kasutamine Azure ja Azure ressursihaldur kerge vaevaga luua uus VM, kus töötab Windows Server."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/21/2016"
    ms.author="davidmu"/>

# <a name="create-a-windows-vm-using-resource-manager-and-powershell"></a>Luua Windows VM ressursihaldur ja PowerShelli abil

Selles artiklis kirjeldatakse, kuidas kiiresti luua Azure virtuaalse masina, kus töötab Windows Server ja ressursse, mis läheb vaja [Ressursihaldur](../azure-resource-manager/resource-group-overview.md) ja PowerShelli abil. 

Kõik selles artiklis toodud juhiseid on vaja luua virtuaalse masina ning teha seda võtab umbes 30 minutit. Asendage näide parameetrite väärtused käsud nimed, mis muudavad teie keskkonda.

## <a name="step-1-install-azure-powershell"></a>Samm 1: Azure'i PowerShelli installimine

Vaadake, [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md) teavet Azure PowerShelli uusima versiooni installimise, valides tellimuse ja oma kontosse sisse logida.
        
## <a name="step-2-create-a-resource-group"></a>Samm 2: Looge ressursirühma

Kõik ressursse peavad sisaldama ressursirühma, nii saab luua kõigepealt.  

1. Siin on loend saadaval kohast, kuhu saab luua ressursse.

    ```powershell
    Get-AzureRmLocation | sort Location | Select Location
    ```

2. Ressursside asukoha seadmine. See käsk määrab **centralus**asukoht.

    ```powershell
    $location = "centralus"
    ```
    
3. Ressursi rühma loomine. See käsk loob ressursirühma nimega **myResourceGroup** teie määratud asukohta.

    ```powershell
    $myResourceGroup = "myResourceGroup"
    New-AzureRmResourceGroup -Name $myResourceGroup -Location $location
    ```
    
## <a name="step-3-create-a-storage-account"></a>Samm 3: Salvestusruumi konto loomine

[Salvestusruumi konto](../storage/storage-introduction.md) on vaja kettaruumi virtuaalse masina, mille loote kasutatavat talletamiseks. Salvestusruumi kontonimed peab olema 3 ja 24 märki ja võib sisaldada arvude ja ainult väiketähti.

1. Testige unikaalsuse salvestusruumi konto nimi. See käsk testib nimi **myStorageAccount**.

    ```powershell
    $myStorageAccountName = "mystorageaccount"
    Get-AzureRmStorageAccountNameAvailability $myStorageAccountName
    ```
    
    Kui käsk tagastab väärtuse **True**, on kordumatu Azure'is teie pakutud nime. 
    
2. Nüüd Looge konto salvestusruumi.
    
    ```powershell    
    $myStorageAccount = New-AzureRmStorageAccount -ResourceGroupName $myResourceGroup -Name $myStorageAccountName -SkuName "Standard_LRS" -Kind "Storage" -Location $location
    ```
    
## <a name="step-4-create-a-virtual-network"></a>Samm 4: Looge virtuaalse võrk

Kõik virtuaalmasinates on [virtuaalse võrgu](../virtual-network/virtual-networks-overview.md).

1. Looge alamvõrku, virtuaalse võrgu jaoks. See käsk loob alamvõrku, mis on nimega **mySubnet** aadress eesliitega, 10.0.0.0/24.
        
    ```powershell
    $mySubnet = New-AzureRmVirtualNetworkSubnetConfig -Name "mySubnet" -AddressPrefix 10.0.0.0/24
    ```
    
2. Nüüd looge virtuaalse võrgu. See käsk loob virtuaalse võrgu **myVnet** abil loodud alamvõrgu ja mõne aadress eesliitega **10.0.0.0/16**nimega.

    ```powershell
    $myVnet = New-AzureRmVirtualNetwork -Name "myVnet" -ResourceGroupName $myResourceGroup -Location $location -AddressPrefix 10.0.0.0/16 -Subnet $mySubnet
    ```
        
## <a name="step-5-create-a-public-ip-address-and-network-interface"></a>Juhis 5: Luua avaliku IP-aadress ja võrgu kasutajaliides

Suhtlus virtuaalse masina virtuaalse võrgu lubamiseks peate [avaliku IP-aadress](../virtual-network/virtual-network-ip-addresses-overview-arm.md) ja võrgu liidese.

1. Luua avaliku IP-aadressi. See käsk loob nimega **myPublicIp** eraldatud meetod, **dünaamiline**avaliku IP-aadress.
 
    ```powershell
    $myPublicIp = New-AzureRmPublicIpAddress -Name "myPublicIp" -ResourceGroupName $myResourceGroup -Location $location -AllocationMethod Dynamic
    ```
        
2. Looge võrgu liidese. See käsk loob võrgu liidese, nimega **myNIC**.

    ```powershell
    $myNIC = New-AzureRmNetworkInterface -Name "myNIC" -ResourceGroupName $myResourceGroup -Location $location -SubnetId $myVnet.Subnets[0].Id -PublicIpAddressId $myPublicIp.Id
    ```
       
## <a name="step-6-create-a-virtual-machine"></a>Samm 6: Luua virtuaalse masina

Nüüd, kui teil on olemas kõik osad, on aeg virtuaalse masina loomiseks.

1. Käivitage see käsk administraatori konto nimi ja parool virtuaalse masina.

        $cred = Get-Credential -Message "Type the name and password of the local administrator account."
        
    Parooli peab olema kell 12-123 tähemärki ja on vähemalt üks väiketähtedes märgi, suurtähed märgi, üks number ja ühe erimärgi. 
        
2. Konfiguratsiooni objekti virtuaalse masina jaoks luua. See käsk loob konfiguratsiooni objekti nimega **myVmConfig** , mis määratleb VM nimi ja VM suurust.

    ```powershell
    $myVm = New-AzureRmVMConfig -VMName "myVM" -VMSize "Standard_DS1_v2"
    ```
     
    Saadaval suurused virtuaalse masina loendi leiate [Azure'i virtuaalmasinates suurused](virtual-machines-windows-sizes.md) .
    
3. Operatsioonisüsteemi VM sätete konfigureerimine. See käsk määrab arvuti nimi, operatsioonisüsteemi tüüp ja konto identimisteave VM.

    ```powershell
    $myVM = Set-AzureRmVMOperatingSystem -VM $myVM -Windows -ComputerName "myVM" -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    ```
    
4. Pildi kasutamiseks ette valmistada VM määratleda. See käsk määratleb Windows Server pildi kasutamiseks VM. 

    ```powershell
    $myVM = Set-AzureRmVMSourceImage -VM $myVM -PublisherName "MicrosoftWindowsServer" -Offer "WindowsServer" -Skus "2012-R2-Datacenter" -Version "latest"
    ```
        
    Valimise piltide kasutamise kohta leiate lisateavet teemast [navigeerimine ja valige virtuaalse masina pildid Windows Azure'i PowerShelli või CLI](virtual-machines-windows-cli-ps-findimage.md).
        
5. Lisage loodud võrgu liidese konfiguratsiooni.

    ```powershell
    $myVM = Add-AzureRmVMNetworkInterface -VM $myVM -Id $myNIC.Id
    ```
        
6. Määratleda nime ja asukoha kõvaketta VM. Ümbris virtuaalse kõvaketta fail on salvestatud. See käsk loob ümbris nimega **vhds/WindowsVMosDisk.vhd** salvestusruumi konto loodud ketas.

    ```powershell
    $blobPath = "vhds/myOsDisk1.vhd"
    $osDiskUri = $myStorageAccount.PrimaryEndpoints.Blob.ToString() + $blobPath
    ```
        
7. Operatsioonisüsteemi ketta teavet lisada VM konfigureerimine. Asendage väärtus **$diskName** operatsioonisüsteemi ketta nimi. Muutuja luua ja lisada teavet ketta konfiguratsiooni.
    
    ```powershell
    $vm = Set-AzureRmVMOSDisk -VM $myVM -Name "myOsDisk1" -VhdUri $osDiskUri -CreateOption fromImage
    ```
        
8. Lõpuks luua virtuaalse masina.

    ```
    New-AzureRmVM -ResourceGroupName $myResourceGroup -Location $location -VM $myVM
    ```
                                  
## <a name="next-steps"></a>Järgmised sammud

- Kui probleemid juurutamise, järgmise juhisega oleks vaadata [tõrkeotsingu ressursside rühma juurutuste Azure'i portaal](../resource-manager-troubleshoot-deployments-portal.md)
- Saate teada, kuidas hallata virtuaalse masina, vaadates [haldamine virtuaalmasinates Azure'i ressursihaldur ja PowerShelli abil](virtual-machines-windows-ps-manage.md)loodud.
- Ära malli abil saate luua virtuaalse masina teave loomine [Windowsi virtuaalse masina ressursihaldur malli](virtual-machines-windows-ps-template.md) abil
