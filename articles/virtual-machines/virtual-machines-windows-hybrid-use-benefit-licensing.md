<properties
   pageTitle="Akna Server Azure'i hübriid kasutamine kasu | Microsoft Azure'i"
   description="Saate teada, kuidas oma Windows Server Software Assurance kasu tuua kohapealse litsentside Azure'i maksimeerimine"
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
   ms.workload="infrastructure-services"
   ms.date="07/13/2016"
   ms.author="georgem"/>

# <a name="azure-hybrid-use-benefit-for-windows-server"></a>Windows Server Azure'i hübriid kasutamine kasu

Klientidele, kes kasutavad Windows Server, kus teie tarkvaratagatise, saate tuua oma kohapealse Windows Server litsentside Azure ja käivitage Windows Server VMs Azure vähendatud eest. Azure'i hübriid kasutamine kasu võimaldab käivitamine Windows serveris VMs Azure ja ainult arve base Arvuta määr. Lisateavet leiate [Azure'i hübriid kasutamine kasu hulgilitsentsimise lehele](https://azure.microsoft.com/pricing/hybrid-use-benefit/). Selles artiklis selgitatakse, kuidas juurutada Windows Server VMs Azure hulgilitsentsimise saaksid kasutada.

> [AZURE.NOTE] Azure'i turuplatsi piltide abil ei saa juurutada Windows Server VMs kasutades Azure hübriid kasutada kasu. Juurutamist peab teie VMs PowerShelli või ressursihaldur mallide kasutamine õigesti registreerida oma VMs saamise alus Arvuta määr diskontomäära.

## <a name="pre-requisites"></a>Eeltingimused
Selleks, et kasutada Azure hübriid kasutamine kasu jaoks Windows Server VMs Azure on eeltingimused paar.

- On installitud Azure PowerShelli moodul
- Windows Serveri VHD üles laadida Azure Storage on

### <a name="install-azure-powershell"></a>Azure'i PowerShelli installimine
Veenduge, et olete [installinud ja konfigureerinud uusim Azure PowerShelli](../powershell-install-configure.md). Isegi juhul, kui te ei kavatse juurutada oma VMs ressursihaldur mallide kasutamine, peate Azure PowerShelli installitud üles laadida oma Windows Serveri VHD (vt järgmist sammu).

### <a name="upload-a-windows-server-vhd"></a>Windows Serveri VHD üleslaadimine

Windows Server VM Azure juurutamiseks peate esmalt looge VHD, mis sisaldab teie base Windows Server koostamine. Selle VHD peab olema õigesti koostatud kaudu Sysprep enne üleslaadimist Azure. Saate [Lisateavet VHD nõuded ja Sysprep protsess](./virtual-machines-windows-upload-image.md) ja [Sysprep serverirollide tugi](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles). Varundage VM enne Sysprep käivitamist. Kui olete valmis oma VHD, üleslaadimine on VHD Azure Storage konto abil soovitud `Add-AzureRmVhd` cmdlet järgmiselt:

```
Add-AzureRmVhd -ResourceGroupName MyResourceGroup -Destination "https://mystorageaccount.blob.core.windows.net/vhds/myvhd.vhd" -LocalFilePath 'C:\Path\To\myvhd.vhd'
```

> [AZURE.NOTE] Microsoft SQL Server ja SharePoint Serveri Dynamics saate kasutada ka teie tarkvaratagatise litsentsimine. Peate endiselt valmistada Windows Server pilt oma rakenduse komponentide installimine ja esitada litsentsi võtmed vastavalt sellele, siis Azure plaat üleslaadimine. Vaadake üle töötab Sysprep oma rakendusega, nt [kaalutluste kohta installimist SQL serveri Sysprepi abil](https://msdn.microsoft.com/library/ee210754.aspx) või [luua SharePoint Server 2016 viide pilt (Sysprep)](http://social.technet.microsoft.com/wiki/contents/articles/33789.build-a-sharepoint-server-2016-reference-image-sysprep.aspx)asjakohased dokumendid.

Samuti saate lugeda Lisateavet [VHD Azure protsessi üleslaadimise](./virtual-machines-windows-upload-image.md#upload-the-vm-image-to-your-storage-account)kohta.

> [AZURE.TIP] See artikkel keskendub Windows Server VMs juurutamine. Saate juurutada Windowsi kliendi VMs samal viisil. Järgmistes näidetes asendate `Server` koos `Client` õigesti.

## <a name="deploy-a-vm-via-powershell-quick-start"></a>VM kaudu PowerShelli kiire-Start juurutamine
PowerShelli kaudu oma Windows Server VM juurutamisel teil on lisatud täiendav parameeter jaoks `-LicenseType`. Kui olete oma VHD Azure'i üles laadida, saate luua uue VM abil `New-AzureRmVM` ja määrake hulgilitsentsimise tüüp järgmiselt:

```
New-AzureRmVM -ResourceGroupName MyResourceGroup -Location "West US" -VM $vm -LicenseType Windows_Server
```

Saate [lugeda Lisateavet üksikasjaliku selgituse kohta juurutamine VM Azure PowerShelli kaudu](./virtual-machines-windows-hybrid-use-benefit-licensing.md#deploy-windows-server-vm-via-powershell-detailed-walkthrough) alla või lugege rohkem kirjeldavat juhend teistmoodi luua [Windows VM ressursihaldur ja PowerShelli abil](./virtual-machines-windows-ps-create.md).

## <a name="deploy-a-vm-via-resource-manager"></a>VM kaudu ressursihaldur juurutamine
Ressursihaldur mudelid, lisatud täiendav parameeter jaoks `licenseType` saab määrata. Lisateavet leiate [Azure'i ressursihaldur mallide loomise](../resource-group-authoring-templates.md)kohta. Kui olete oma VHD Azure'i üles laadida, redigeerida ressursihaldur malli lisada litsentsi tüübi Arvuta pakkuja ja juurutada malli nagu tavaliselt:

```
"properties": {  
   "licenseType": "Windows_Server",
   "hardwareProfile": {
        "vmSize": "[variables('vmSize')]"
   },
```
 
## <a name="verify-your-vm-is-utilizing-the-licensing-benefit"></a>Veenduge, et teie VM on kasutades hulgilitsentsimise kasu
Kui olete oma VM juurutanud kuni ükskõik kumma PowerShelli või ressursihaldur juurutamise meetod, kontrollige litsentsitüübi koos `Get-AzureRmVM` järgmiselt:
 
```
Get-AzureRmVM -ResourceGroup MyResourceGroup -Name MyVM
```

Väljund on järgmine:

```
Type                     : Microsoft.Compute/virtualMachines
Location                 : westus
LicenseType              : Windows_Server
```

Sellele juurutatud ilma Azure'i hübriid kasutamine kasu litsentsimise juurutatud otse galeriist Azure'i VM näiteks järgmised VM:

```
Type                     : Microsoft.Compute/virtualMachines
Location                 : westus
LicenseType              : 
```
 
## <a name="detailed-powershell-walkthrough"></a>PowerShelli üksikasjaliku selgituse

Üksikasjalik PowerShelli järgmist Kuva täielik rakendamine VM. Lugege rohkem konteksti tegelik cmdlet-käskude ja eri osade [luua Windows VM ressursihaldur ja PowerShelli abil](./virtual-machines-windows-ps-create.md)luuakse. Saate läbi loomise oma ressursirühm, salvestusruumi konto ja virtuaalse võrk, seejärel oma VM määratlemine ja lõpuks luua oma VM.
 
Esmalt turvaliselt saada identimisteave, määrake asukoht ja ressursside rühma nimi:

```
$cred = Get-Credential
$location = "West US"
$resourceGroupName = "TestLicensing"
```

Avaliku IP loomiseks tehke järgmist.

```
$publicIPName = "testlicensingpublicip"
$publicIP = New-AzureRmPublicIpAddress -Name $publicIPName -ResourceGroupName $resourceGroupName -Location $location -AllocationMethod Dynamic
```

Teie alamvõrku, NIC ja VNET määratlemine

```
$subnetName = "testlicensingsubnet"
$nicName = "testlicensingnic"
$vnetName = "testlicensingvnet"
$subnetconfig = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/8
$vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $resourceGroupName -Location $location -AddressPrefix 10.0.0.0/8 -Subnet $subnetconfig
$nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $resourceGroupName -Location $location -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $publicIP.Id
```

Pange oma VM ja luua VM config.

```
$vmName = "testlicensing"
$vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize "Standard_A1"
```

Teie OS määratlemine

```
$computerName = "testlicensing"
$vm = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName $computerName -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
```

Lisage oma NIC VM.

```
$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
```

Määratlege salvestusruumi konto, mida soovite kasutada:

```
$storageAcc = Get-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -AccountName testlicensing
```

Laadige oma VHD, sobival viisil valmis, ja manustamine oma VM kasutamiseks.

```
$osDiskName = "licensing.vhd"
$osDiskUri = '{0}vhds/{1}{2}.vhd' -f $storageAcc.PrimaryEndpoints.Blob.ToString(), $vmName.ToLower(), $osDiskName
$urlOfUploadedImageVhd = "https://testlicensing.blob.core.windows.net/vhd/licensing.vhd"
$vm = Set-AzureRmVMOSDisk -VM $vm -Name $osDiskName -VhdUri $osDiskUri -CreateOption FromImage -SourceImageUri $urlOfUploadedImageVhd -Windows
```

Lõpuks luua oma VM ja määratleda, millist hulgilitsentsimise kasutada Azure hübriid kasutamine kasu.

```
New-AzureRmVM -ResourceGroupName $resourceGroupName -Location $location -VM $vm -LicenseType Windows_Server
```

## <a name="next-steps"></a>Järgmised sammud

Lugege lisateavet [Azure hübriid kasutamine kasu litsentsimise](https://azure.microsoft.com/pricing/hybrid-use-benefit/)kohta.

Lugege lisateavet [ressursihaldur mallide kasutamise](../azure-resource-manager/resource-group-overview.md)kohta.
