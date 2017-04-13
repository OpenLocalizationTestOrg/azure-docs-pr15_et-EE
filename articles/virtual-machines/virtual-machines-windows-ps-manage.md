<properties
    pageTitle="Hallata VMs ressursihaldur ja PowerShelli abil | Microsoft Azure'i"
    description="Hallata virtuaalmasinates Azure'i ressursihaldur ja PowerShelli abil."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="na"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="davidmu"/>

# <a name="manage-azure-virtual-machines-using-resource-manager-and-powershell"></a>Azure'i Virtuaalmasinates ressursihaldur ja PowerShelli abil haldamine

## <a name="install-azure-powershell"></a>Azure'i PowerShelli installimine
 
Vaadake, [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md) teavet Azure PowerShelli uusima versiooni installimise, valides tellimuse ja oma kontosse sisse logida.

## <a name="set-variables"></a>Muutujate määramine

Kõik käsud artiklis on vaja ressursirühm, kus asub virtuaalse masina nimi ja hallata virtuaalse masina nimi. Asendage väärtus **$rgName** ressursirühm, mis sisaldab virtuaalse masina nimi. Asendage väärtus **$vmName** VM nime. Looge muutujaid.

    $rgName = "resource-group-name"
    $vmName = "VM-name"

## <a name="display-information-about-a-virtual-machine"></a>Virtuaalse masina kohta teabe kuvamine

Virtuaalse masina teavet saada.
  
    Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName

See tagastab midagi, nagu järgmises näites:

    ResourceGroupName        : rg1
    Id                       : /subscriptions/{subscription-id}/resourceGroups/
                               rg1/providers/Microsoft.Compute/virtualMachines/vm1
    Name                     : vm1
    Type                     : Microsoft.Compute/virtualMachines
    Location                 : centralus
    Tags                     : {}
    AvailabilitySetReference : {
                                  "id": "/subscriptions/{subscription-id}/resourceGroups/
                                  rg1/providers/Microsoft.Compute/availabilitySets/av1"
                               }
    Extensions               : []
    HardwareProfile          : {
                                  "vmSize": "Standard_A0"
                               }
    InstanceView             : null
    NetworkProfile           : {
                                  "networkInterfaces": [
                                    {
                                      "properties.primary": null,
                                      "id": "/subscriptions/{subscription-id}/resourceGroups/
                                      rg1/providers/Microsoft.Network/networkInterfaces/nc1"
                                    }
                                  ]
                               }
    OSProfile                : {
                                  "computerName": "vm1",
                                  "adminUsername": "myaccount1",
                                  "adminPassword": null,
                                  "customData": null,
                                  "windowsConfiguration": {
                                    "provisionVMAgent": true,
                                    "enableAutomaticUpdates": true,
                                    "timeZone": null,
                                    "additionalUnattendContents": [],
                                    "winRM": null
                                  },
                                  "linuxConfiguration": null,
                                  "secrets": []
                               }
    Plan                     : null
    ProvisioningState        : Succeeded
    StorageProfile           : {
                                  "imageReference": {
                                    "publisher": "MicrosoftWindowsServer",
                                    "offer": "WindowsServer",
                                    "sku": "2012-R2-Datacenter",
                                    "version": "latest"
                                  },
                                  "osDisk": {
                                    "osType": "Windows",
                                    "encryptionSettings": null,
                                    "name": "osdisk",
                                    "vhd": {
                                      "Uri": "http://sa1.blob.core.windows.net/vhds/osdisk1.vhd"
                                    }
                                    "image": null,
                                    "caching": "ReadWrite",
                                    "createOption": "FromImage"
                                  }
                                  "dataDisks": [],
                               }
    DataDiskNames            : {}
    NetworkInterfaceIDs      : {/subscriptions/{subscription-id}/resourceGroups/
                                rg1/providers/Microsoft.Network/networkInterfaces/nc1}

## <a name="stop-a-virtual-machine"></a>Virtuaalse masina peatamine

Töötava virtuaalse masina peatada.

    Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName

Küsitakse kinnitust:

    Virtual machine stopping operation
    This cmdlet will stop the specified virtual machine. Do you want to continue?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):
        
Sisestage **Y** virtuaalse masina lõpetada.

Pärast paar minutit, tagastab see midagi, nagu järgmises näites:

    StatusCode : Succeeded
    StartTime  : 9/13/2016 12:11:57 PM
    EndTime    : 9/13/2016 12:14:40 PM

## <a name="start-a-virtual-machine"></a>Käivitada

Käivitage virtuaalse masina, kui see on peatatud.

    Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName

Pärast paar minutit, tagastab see midagi, nagu järgmises näites:

    StatusCode : Succeeded
    StartTime  : 9/13/2016 12:32:55 PM
    EndTime    : 9/13/2016 12:35:09 PM

Kui soovite virtual arvutisse, kus töötab juba taaskäivitada, kirjeldatud kasutamine **Uuesti-AzureRmVM** edasi.

## <a name="restart-a-virtual-machine"></a>Virtuaalse masina taaskäivitamine

Taaskäivitage töötava virtuaalse masina.

    Restart-AzureRmVM -ResourceGroupName $rgName -Name $vmName

See tagastab midagi, nagu järgmises näites:

    StatusCode : Succeeded
    StartTime  : 9/13/2016 12:54:40 PM
    EndTime    : 9/13/2016 12:55:54 PM

## <a name="delete-a-virtual-machine"></a>Virtuaalse masina kustutamine

Kustutage virtuaalse masina.  

    Remove-AzureRmVM -ResourceGroupName $rgName –Name $vmName

> [AZURE.NOTE] Saate kasutada funktsiooni **-Jõusta** parameeter Kinnitusviiba kuvamisel vahele jätta.

Kui te ei kasuta Jõusta parameetrit-, küsitakse kinnitust:

    Virtual machine removal operation
    This cmdlet will remove the specified virtual machine. Do you want to continue?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):

See tagastab midagi, nagu järgmises näites:

    RequestId  IsSuccessStatusCode  StatusCode  ReasonPhrase
    ---------  -------------------  ----------  ------------
                              True          OK  OK

## <a name="update-a-virtual-machine"></a>Virtuaalne arvuti värskendamine

Selles näites on kujutatud värskendamine virtuaalse masina suurus.
        
    $vmSize = "Standard_A1"
    $vm = Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName
    $vm.HardwareProfile.vmSize = $vmSize
    Update-AzureRmVM -ResourceGroupName $rgName -VM $vm
    
See tagastab midagi, nagu järgmises näites:

    RequestId  IsSuccessStatusCode  StatusCode  ReasonPhrase
    ---------  -------------------  ----------  ------------
                              True          OK  OK
                              
Saadaval suurused virtuaalse masina loendi leiate [Azure'i virtuaalmasinates suurused](virtual-machines-windows-sizes.md) .

## <a name="add-a-data-disk-to-a-virtual-machine"></a>Virtuaalse masina kettal andmete lisamine

Selles näites kujutatakse lisamiseks mõne olemasoleva virtuaalse masina andmed kettale.

    $vm = Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName
    Add-AzureRmVMDataDisk -VM $vm -Name "disk-name" -VhdUri "https://mystore1.blob.core.windows.net/vhds/datadisk1.vhd" -LUN 0 -Caching ReadWrite -DiskSizeinGB 1 -CreateOption Empty
    Update-AzureRmVM -ResourceGroupName $rgName -VM $vm

Ketas, mille lisate on lähtestatud. Ketas lähtestada, saate sisse logida ja kasutada. Kui olete installinud WinRM ja serdi seda kui lõite, saate lähtestada ketas Kaug-PowerShelli. Samuti saate kohandatud skript laiend: 

    $location = "location-name"
    $scriptName = "script-name"
    $fileName = "script-file-name"
    Set-AzureRmVMCustomScriptExtension -ResourceGroupName $rgName -Location $locName -VMName $vmName -Name $scriptName -TypeHandlerVersion "1.4" -StorageAccountName "mystore1" -StorageAccountKey "primary-key" -FileName $fileName -ContainerName "scripts"

Skripti fail võib sisaldada umbes lähtestada selle ketast järgmine kood:

    $disks = Get-Disk |   Where partitionstyle -eq 'raw' | sort number

    $letters = 70..89 | ForEach-Object { ([char]$_) }
    $count = 0
    $labels = @("data1","data2")

    foreach($d in $disks) {
        $driveLetter = $letters[$count].ToString()
        $d | 
        Initialize-Disk -PartitionStyle MBR -PassThru |
        New-Partition -UseMaximumSize -DriveLetter $driveLetter |
        Format-Volume -FileSystem NTFS -NewFileSystemLabel $labels[$count] `
            -Confirm:$false -Force 
        $count++
    }

## <a name="next-steps"></a>Järgmised sammud

Kui probleemid juurutamine, võib vaadata [tõrkeotsingu ressursside rühma juurutuste Azure'i portaal](../resource-manager-troubleshoot-deployments-portal.md)
