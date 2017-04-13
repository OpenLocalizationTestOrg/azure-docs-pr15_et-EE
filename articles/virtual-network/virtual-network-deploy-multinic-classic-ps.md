<properties
   pageTitle="Mitme NIC VMs PowerShelli kaudu klassikaline juurutamise mudeli juurutamine | Microsoft Azure'i"
   description="Saate teada, kuidas mitme NIC VMs PowerShelli kaudu klassikaline juurutamise mudeli juurutamine"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags  
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/02/2016"
   ms.author="jdial" />

#<a name="deploy-multi-nic-vms-classic-using-powershell"></a>Mitme NIC VMs (klassikaline) PowerShelli kaudu juurutamine

[AZURE.INCLUDE [virtual-network-deploy-multinic-classic-selectors-include.md](../../includes/virtual-network-deploy-multinic-classic-selectors-include.md)]

Saate luua Azure'i virtuaalmasinates (VMs) ja iga teie VMs mitme võrgu liidesed (NICs) manustamiseks. Mitu NICs lubada tüüpi liikluse eraldamine NICs üle. Näiteks üks NIC võib suhelda Internet, samas teise suhtleb ainult sisemisi ressursse, mis on Interneti-ühenduseta. Võimalus eraldi võrguliiklust üle mitme NICs on vaja mitme võrgu virtuaalse seadmed, nt rakenduse kohaletoimetamise ja WAN optimeeritud lahenduse.

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Siit saate teada, kuidas [nende](virtual-network-deploy-multinic-arm-ps.md)toimingute abil ressursihaldur mudeli.

[AZURE.INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

Praegu ei saa ühe NIC VMs ja mitu NICs VMs sama pilveteenuses. Seega peate tagasi lõpuks serverites rakendada eri pilveteenuses kui ja kõik muud komponendid seda stsenaariumi. Alltoodud juhiseid kasutada mõnda pilveteenusesse nimeks *IaaSStory* peamised ressursid ja *IaaSStory-Taustväärtus* tagasi lõpuks serverid.

## <a name="prerequisites"></a>Eeltingimused

Enne juurutamist saate tagasi lõpuks serverites, peate selle stsenaariumi korral kõik vajalikud ressursid peamised pilveteenuses juurutamine. Vähemalt, peate looma virtuaalse võrgu koos alamvõrku, jaoks soovitud taustväärtus. Külastage [loomine virtuaalse võrgu PowerShelli abil](virtual-networks-create-vnet-classic-netcfg-ps.md) saate teada, kuidas juurutada virtuaalse võrgus.

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="deploy-the-back-end-vms"></a>Tagasi lõpuks VMs juurutamine

Kirjutamata VMs sõltuvad loomine ressursse, mis on loetletud allpool.

- **Taustväärtus alamvõrgu**. Andmebaasi serverid on osa eraldi alamvõrku, et eraldada liikluse. Allpool skripti eeldab, et see alamvõrgu on vnet nimega *WTestVnet*olemas.
- **Salvestusruumi konto andmeid ketast**. Parema jõudluse tagamiseks kasutatakse andmete ketast andmebaasi serverite ühtlase olekus ketas tehnoloogia, mis nõuab premium salvestusruumi konto. Veenduge, et juurutamiseks premium mälu tugi Azure asukoht.
- **Kättesaadavus**. Kõik andmebaasi serverid lisatakse ühe kättesaadavus, määrata tagamiseks on vähemalt üks VMs üles ja hoolduse ajal töötab.

### <a name="step-1---start-your-script"></a>Samm 1 - skripti käivitamine

Saate alla laadida täielik PowerShelli skripti kasutada [siin](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/classic/virtual-network-deploy-multinic-classic-ps.ps1). Järgige alltoodud muuta skripti töötada teie keskkonnas.

1. Muutke vastavalt oma olemasoleva ressursirühm juurutatud kohal [eeltingimused](#Prerequisites)alloleval muutujate väärtusi.

        $location              = "West US"
        $vnetName              = "WTestVNet"
        $backendSubnetName     = "BackEnd"

2. Saate muuta väärtuste põhjal väärtusi, mida soovite kasutada oma kirjutamata juurutamiseks allpool muutujate.

        $backendCSName         = "IaaSStory-Backend"
        $prmStorageAccountName = "iaasstoryprmstorage"
        $avSetName             = "ASDB"
        $vmSize                = "Standard_DS3"
        $diskSize              = 127
        $vmNamePrefix          = "DB"
        $dataDiskSuffix        = "datadisk"
        $ipAddressPrefix       = "192.168.2."
        $numberOfVMs           = 2

### <a name="step-2---create-necessary-resources-for-your-vms"></a>Samm 2 – Loo oma vms vajalikud ressursid

Peate looma uue pilveteenuses ja salvestusruumi konto jaoks andmete ketast kõik vms. Samuti peate määrama pilt ja kohaliku administraatorikonto VMs jaoks. Käivitada nende ressursside loomiseks toimige järgmiselt.

1. Saate luua uue pilveteenuses.

        New-AzureService -ServiceName $backendCSName -Location $location

2. Looge uus premium salvestusruumi konto.

        New-AzureStorageAccount -StorageAccountName $prmStorageAccountName `
            -Location $location `
            -Type Premium_LRS

3. Määrake loodud kohal praeguse salvestusruumi konto tellimuse salvestusruumi konto.

        $subscription = Get-AzureSubscription `
            | where {$_.IsCurrent -eq $true}  
        Set-AzureSubscription -SubscriptionName $subscription.SubscriptionName `
            -CurrentStorageAccountName $prmStorageAccountName

4. Valige pilt VM.

        $image = Get-AzureVMImage `
            | where{$_.ImageFamily -eq "SQL Server 2014 RTM Web on Windows Server 2012 R2"} `
            | sort PublishedDate -Descending `
            | select -ExpandProperty ImageName -First 1

5. Sea kohaliku administraatori konto identimisteave.

        $cred = Get-Credential -Message "Enter username and password for local admin account"

### <a name="step-3---create-vms"></a>Samm 3 – VMs loomine

Peate esitatavaks abil saate luua nii palju VMs, kui soovite, ja luua vajalikud NICs ja VMs kursis. Käivitada NICs ja VMs loomiseks toimige järgmiselt.

1. Alustamine on `for` pidev korrata käske luua VM ja kahe NICs nii mitu korda vastavalt vajadusele väärtuse alusel selle `$numberOfVMs` muutuja.

        for ($suffixNumber = 1; $suffixNumber -le $numberOfVMs; $suffixNumber++){

2. Luua mõne `VMConfig` pilt, suurus ja kättesaadavus seadmine VM objekti.

            $vmName = $vmNamePrefix + $suffixNumber
            $vmConfig = New-AzureVMConfig -Name $vmName `
                            -ImageName $image `
                            -InstanceSize $vmSize `
                            -AvailabilitySetName $avSetName  

3. Sätte VM Windowsiga VM.

            Add-AzureProvisioningConfig -VM $vmConfig -Windows `
                -AdminUsername $cred.UserName `
                -Password $cred.Password

4. NIC vaikesätted ja määrata see staatiline IP-aadress.

            Set-AzureSubnet -SubnetNames $backendSubnetName -VM $vmConfig
            Set-AzureStaticVNetIP -IPAddress ($ipAddressPrefix+$suffixNumber+3) -VM $vmConfig

5. Lisage teine NIC jaoks iga VM.

            Add-AzureNetworkInterfaceConfig -Name ("RemoteAccessNIC"+$suffixNumber) `
                -SubnetName $backendSubnetName `
                -StaticVNetIPAddress ($ipAddressPrefix+(53+$suffixNumber)) `
                -VM $vmConfig

6. Luua andmete ketast jaoks iga VM.

            $dataDisk1Name = $vmName + "-" + $dataDiskSuffix + "-1"    
            Add-AzureDataDisk -CreateNew -VM $vmConfig `
                -DiskSizeInGB $diskSize `
                -DiskLabel $dataDisk1Name `
                -LUN 0       

            $dataDisk2Name = $vmName + "-" + $dataDiskSuffix + "-2"   
            Add-AzureDataDisk -CreateNew -VM $vmConfig `
                -DiskSizeInGB $diskSize `
                -DiskLabel $dataDisk2Name `
                -LUN 1

7. Looge iga VM ja lõpetamiseks kursis.

            New-AzureVM -VM $vmConfig `
                -ServiceName $backendCSName `
                -Location $location `
                -VNetName $vnetName
        }

### <a name="step-4---run-the-script"></a>Samm 4 - skripti

Nüüd, kui saate alla laadida ja muutunud script vajaduste, umbes ta skript luua tagasi lõpuks andmebaasi VMs mitu NICs.

1. Salvestage oma skripti ja käivitage see **PowerShelli** käsuviiba või **PowerShell ISE**. Kuvatakse algse väljund, nagu allpool näidatud.

        OperationDescription    OperationId                          OperationStatus
        --------------------    -----------                          ---------------
        New-AzureService        xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded      
        New-AzureStorageAccount xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded      

        WARNING: No deployment found in service: 'IaaSStory-Backend'.

2. Täitke identimisteabe viiba vajalik teave ja klõpsake nuppu **OK**. Kuvatakse allpool väljundi.

        New-AzureVM             xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded
        New-AzureVM             xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded
