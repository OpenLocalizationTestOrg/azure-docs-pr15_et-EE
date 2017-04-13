<properties
    pageTitle="Et ressursihaldur PowerShelliga migreerimine | Microsoft Azure'i"
    description="Selles artiklis tutvustatakse platvormi toetatud migreerimise IaaS ressursside kaudu klassikaline Azure'i ressursihaldur Azure PowerShelli käskude abil"
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
    ms.date="10/19/2016"
    ms.author="cynthn"/>

# <a name="migrate-iaas-resources-from-classic-to-azure-resource-manager-by-using-azure-powershell"></a>Migreerimine IaaS ressursid klassikaline Azure'i ressursihaldur Azure PowerShelli abil

Need juhised näitavad, kuidas migreerida taristu teenus (IaaS) ressurssidena klassikaline juurutamise mudeli Azure'i ressursihaldur juurutamise mudeli Azure PowerShelli käskude abil. 

Kui soovite, saate ka migreerida ressursid [Azure'i käsurea liides (Azure'i CLI)](virtual-machines-linux-cli-migration-classic-resource-manager.md)abil.

- Toetatud migreerimise stsenaariumid, leiate [Azure'i ressursihaldur klassikalises ressursside IaaS migreerimise platvormi toetatud](virtual-machines-windows-migration-classic-resource-manager.md). 
- Üksikasjalikud juhised ja migreerimise samm-sammult juhendi leiate teemast [tehnika deep dive käsitleb klassikaline Azure'i ressursihaldur migratsiooni platvormi toetatud](virtual-machines-windows-migration-classic-resource-manager-deep-dive.md).

## <a name="step-1-plan-for-migration"></a>Samm 1: Migreerimise kavandamine

Siin on paar põhitõed, mida me soovitame kui hindate migreerimine IaaS ressursid klassikalises ressursside halduri.

- Lugege läbi [toetatud ja Toetuseta funktsioonid ja konfiguratsioone](virtual-machines-windows-migration-classic-resource-manager.md). Kui teil on toetamata konfiguratsioone või funktsioone kasutavate virtuaalmasinates, soovitame oodata konfigureerimise/funktsiooni tugi tuleb teada. Teine võimalus, kui see sobib teie vajadustele, eemaldamine vastava funktsiooni või kolima selle konfiguratsiooni lubamiseks migreerimise.
-   Kui teil on automatiseeritud skriptide, kus juurutada ja oma rakenduste täna, proovige luua sarnane test setup migreerimise nende skriptide abil. Teise võimalusena saate häälestada valimi keskkonnas Azure portaali kaudu.

>[AZURE.IMPORTANT]Migreerimise klassikalises ressursside halduri virtuaalse võrgu lüüside (-saidilt, Azure'i ExpressRoute, rakenduse lüüsi, punkt-ja saidi) praegu ei toetata. Klassikaline virtuaalse võrgu lüüsi migreerimiseks eemaldada lüüsi enne Kinnita toimimise võrgu (käivitada ettevalmistamine etappi lüüsi kustutamata) liikumiseks. Kui olete migreerimise, uuesti lüüsi Azure'i ressursihaldur.

## <a name="step-2-install-the-latest-version-of-azure-powershell"></a>Samm 2: Azure'i PowerShelli uusima versiooni installimine

On kaks peamist võimalust installida Azure PowerShelli: [PowerShelli Galerii](https://www.powershellgallery.com/profiles/azure-sdk/) või [Web platvormi Installer (WebPI)](http://aka.ms/webpi-azps). WebPI saab igakuised värskendused. PowerShelli Galerii saab pidevalt värskendusi. Selles artiklis on Azure PowerShelli versioon 2.1.0 alusel.

Installimise juhiste saamiseks vaadake, [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md).

<br>
## <a name="step-3-set-your-subscription-and-sign-up-for-migration"></a>Samm 3: Tellimuse seadmine ja migreerimise kasutajaks registreerumine

Esmalt käivitage PowerShelli käsuviibas. Migreerimise, peate nii klassikaline keskkonna häälestamine ja ressursihaldur.

Logige sisse oma konto ressursihaldur mudeli.

```powershell
    Login-AzureRmAccount
```

Saada saadaval tellimuste abil järgmine käsk:

```powershell
    Get-AzureRMSubscription | Sort SubscriptionName | Select SubscriptionName
```

Määrake Azure tellimuse praeguse seansi kohta. Selles näites määrab **Minu Azure tellimuse**vaikimisi tellimuse nime. Asendage oma näide tellimuse nime. 

```powershell
    Select-AzureRmSubscription –SubscriptionName "My Azure Subscription"
```

>[AZURE.NOTE] Registreerimise on ühekordse toiming, kuid peate tegema seda üks kord enne, kui proovite migreerimise. Registreerimata, kuvatakse järgmine tõrketeade: 

>   *BadRequest: Tellimus pole registreeritud migreerimise jaoks.* 

Migreerimise ressursi pakkuja registreerida, kasutades järgmine käsk:

```powershell
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
```

Oodake viis minutit registreerimise lõpuleviimiseks. Järgmise käsu abil saate kontrollida kinnituse olek.

```powershell
    Get-AzureRmResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
```

Veenduge, et RegistrationState on `Registered` enne jätkamist. 

Nüüd logige sisse oma konto jaoks, klassikaline.

```powershell
    Add-AzureAccount
```

Saada saadaval tellimuste abil järgmine käsk:

```powershell
    Get-AzureSubscription | Sort SubscriptionName | Select SubscriptionName
```

Määrake Azure tellimuse praeguse seansi kohta. Selles näites määrab vaikimisi tellimuse **Minu Azure'i tellimus**. Asendage oma näide tellimuse nime. 

```powershell
    Select-AzureSubscription –SubscriptionName "My Azure Subscription"
```

<br>

## <a name="step-4-make-sure-you-have-enough-azure-resource-manager-virtual-machine-cores-in-the-azure-region-of-your-current-deployment-or-vnet"></a>Samm 4: Veenduge, et teil on piisavalt Azure'i ressursihaldur virtuaalse masina tuuma Azure piirkonna praeguse juurutamise või VNET

Saate kontrollida numbril valdkond teil on Azure ressursihaldur PowerShelli käsk. Core kvootide kohta leiate lisateavet artiklist [piirangud ja Azure ressursihaldur](../azure-subscription-service-limits.md#limits-and-the-azure-resource-manager). 

Selles näites kontrollib **Lääne USA** piirkonna kättesaadavus. Asendage näide regiooni nimi oma. 

```powershell
Get-AzureRmVMUsage -Location "West US"
```

## <a name="step-5-run-commands-to-migrate-your-iaas-resources"></a>Juhis 5: Käivitage käsud migreerida oma IaaS ressursid

>[AZURE.NOTE] Kõik toimingud, mida on kirjeldatud allpool, idempotent. Kui teil on probleeme peale toetuseta funktsiooni või konfiguratsiooni tõrge, soovitame teil proovige uuesti ettevalmistamine, katkestada või Kinnita toiming. Platvormi proovib seejärel toimingu uuesti.

### <a name="migrate-virtual-machines-in-a-cloud-service-not-in-a-virtual-network"></a>Migreerimine virtuaalmasinates pilveteenuses (pole rakenduses virtuaalse võrk)

Saada pilveteenustega loendi järgmise käsu abil ja seejärel valige pilveteenuses, mida soovite migreerida. Kui VMs pilveteenusesse, virtuaalse võrgus või kui need on veebis või töötaja rollid, käsk tagastab tulemuseks tõrketeate.

```powershell
    Get-AzureService | ft Servicename
```

Saada pilveteenusesse juurutamise nimi. Selles näites on teenuse nimi **Minu teenus**. Asendage näide teenuse nimi oma teenuse nimi. 

```powershell
    $serviceName = "My Service"
    $deployment = Get-AzureDeployment -ServiceName $serviceName
    $deploymentName = $deployment.DeploymentName
```

Ettevalmistused virtuaalmasinates pilveteenuses migreerimise jaoks. Teil on kaks võimalust valida.

* **Suvand 1. VMs platvormi loodud virtuaalse võrgu migreerimine**

    Esmalt veenduge, kui migreerite pilveteenuses, kasutades järgmised käsud:

    ```powershell
    $validate = Move-AzureService -Validate -ServiceName $serviceName `
        -DeploymentName $deploymentName -CreateNewVirtualNetwork
    $validate.ValidationMessages
    ```

    Eelmise käsu kuvab hoiatused ja tõrkeid migreerimise blokeerida. Kui kinnitamine õnnestus, siis saate teisaldada **ettevalmistamine** juhisega:

    ```powershell
    Move-AzureService -Prepare -ServiceName $serviceName `
        -DeploymentName $deploymentName -CreateNewVirtualNetwork
    ```

* **Variant 2. Olemasoleva virtuaalse võrgu ressursihaldur juurutamise mudeli migreerimine**

    Selles näites määrab **myResourceGroup**, virtuaalse võrgu nime **myVirtualNetwork** ja alamvõrgu nime **mySubNet**ressursi rühma nime. Asendage näites nimesid oma ressursside nimed.

    ```powershell
    $existingVnetRGName = "myResourceGroup"
    $vnetName = "myVirtualNetwork"
    $subnetName = "mySubNet"
    ```

    Esmalt veenduge, kui migreerite pilveteenuses, kasutades järgmist käsku:

    ```powershell
    $validate = Move-AzureService -Validate -ServiceName $serviceName `
        -DeploymentName $deploymentName -UseExistingVirtualNetwork -VirtualNetworkResourceGroupName $existingVnetRGName -VirtualNetworkName $vnetName -SubnetName $subnetName
    $validate.ValidationMessages
    ```

    Eelmise käsu kuvab hoiatused ja tõrkeid migreerimise blokeerida. Kui kinnitamine õnnestus, siis võib teha järgmised ettevalmistamine toimingut:

    ```powershell
    Move-AzureService -Prepare -ServiceName $serviceName -DeploymentName $deploymentName `
        -UseExistingVirtualNetwork -VirtualNetworkResourceGroupName $existingVnetRGName `
        -VirtualNetworkName $vnetName -SubnetName $subnetName
    ```
    

Pärast ettevalmistamine õnnestub, kas eelmise suvandi, päringu VMs migreerimise olek. Veenduge, et need on selle `Prepared` olekus.

Selles näites määrab **myVM**VM nime. Asendage näide nimi oma VM nime.

    ```powershell
    $vmName = "myVM"
    $vm = Get-AzureVM -ServiceName $serviceName -Name $vmName
    $vm.VM.MigrationState
    ```

Kontrollige valmis ressursid konfiguratsiooni PowerShelli või Azure portaali abil. Kui te ei ole valmis migreerimise ja soovite naasta ja vana, kasutage järgmist käsku:

```powershell
    Move-AzureService -Abort -ServiceName $serviceName -DeploymentName $deploymentName
```

Kui valmis konfiguratsiooni sobib, saate liikuda ja kinnita ressursside abil järgmine käsk:

```powershell
    Move-AzureService -Commit -ServiceName $serviceName -DeploymentName $deploymentName
```

### <a name="migrate-virtual-machines-in-a-virtual-network"></a>Virtuaalmasinates virtuaalse võrgu migreerimine

Virtuaalmasinates virtuaalse võrku migreerida, saate võrgu migreerida. Funktsiooni virtuaalmasinates migreerida automaatselt võrgu. Valige virtuaalse võrgu, mida soovite migreerida. 

Selles näites määrab **myVnet**virtuaalne võrgu nimi. Asendage näide virtuaalne võrgu nimi oma. 

```powershell
    $vnetName = "myVnet"
```

>[AZURE.NOTE] Kui virtuaalse võrgu sisaldab web või töötaja rollid ja VMs toetuseta konfiguratsioone, kuvatakse teade valideerimisreegli tõrge.

Esmalt saate kontrollida, kui migreerite virtuaalse võrgu abil järgmine käsk:

```powershell
    Move-AzureVirtualNetwork -Validate -VirtualNetworkName $vnetName
```

Eelmise käsu kuvab hoiatused ja tõrkeid migreerimise blokeerida. Kui kinnitamine õnnestus, siis võib teha järgmised ettevalmistamine toimingut:

```powershell
    Move-AzureVirtualNetwork -Prepare -VirtualNetworkName $vnetName
```

Kontrollige konfiguratsiooni valmis virtuaalmasinates Azure PowerShelli või Azure portaali abil. Kui te ei ole valmis migreerimise ja soovite naasta ja vana, kasutage järgmist käsku:

```powershell
    Move-AzureVirtualNetwork -Abort -VirtualNetworkName $vnetName
```

Kui valmis konfiguratsiooni sobib, saate liikuda ja kinnita ressursside abil järgmine käsk:

```powershell
    Move-AzureVirtualNetwork -Commit -VirtualNetworkName $vnetName
```

### <a name="migrate-a-storage-account"></a>Migreerimine salvestusruumi konto

Kui olete lõpetanud migreerimine on virtuaalmasinates, soovitame migreerite salvestusruumi kontod.

Iga salvestusruumi konto migreerimiseks valmistumine järgmise käsu abil. Selles näites on salvestusruumikonto nimi **myStorageAccount**. Asendage näide nimi oma salvestusruumi konto nimi. 

```powershell
    $storageAccountName = "myStorageAccount"
    Move-AzureStorageAccount -Prepare -StorageAccountName $storageAccountName
```

Kontrollige konfiguratsiooni valmis salvestusruumi konto Azure PowerShelli või Azure portaali abil. Kui te ei ole valmis migreerimise ja soovite naasta ja vana, kasutage järgmist käsku:

```powershell
    Move-AzureStorageAccount -Abort -StorageAccountName $storageAccountName
```

Kui valmis konfiguratsiooni sobib, saate liikuda ja kinnita ressursside abil järgmine käsk:

```powershell
    Move-AzureStorageAccount -Commit -StorageAccountName $storageAccountName
```

## <a name="next-steps"></a>Järgmised sammud

- Migreerimise kohta leiate lisateavet teemast [Azure ressursihaldur klassikalises ressursside IaaS migreerimise platvormi toetatud](virtual-machines-windows-migration-classic-resource-manager.md).
- Migreerimine võrgu täiendavaid ressursse abil ressursihaldur Azure PowerShelli kaudu, kasutage [Teisalda-AzureNetworkSecurityGroup](https://msdn.microsoft.com/library/mt786729.aspx), [Teisalda-AzureReservedIP](https://msdn.microsoft.com/library/mt786752.aspx)ja [Teisaldamine-AzureRouteTable](https://msdn.microsoft.com/library/mt786718.aspx)sarnaseid juhiseid.
- Avatud lähtekoodi skriptide abil saate migreerida Azure ressursse klassikalises ressursside halduri, leiate [Azure'i ressursihaldur migreerimise migratsiooni kogukonna tööriistad](virtual-machines-windows-migration-scripts.md)
