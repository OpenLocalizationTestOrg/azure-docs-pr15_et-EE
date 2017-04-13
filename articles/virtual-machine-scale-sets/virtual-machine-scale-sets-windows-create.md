<properties
    pageTitle="Luua on virtuaalse masina skaala seadmine PowerShelli kaudu | Microsoft Azure'i"
    description="Looge on virtuaalse masina skaala seadmine PowerShelli abil"
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="davidmu1"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="davidmu"/>

# <a name="create-a-windows-virtual-machine-scale-set-using-azure-powershell"></a>Virtuaalse masina skaalal Windows Azure'i PowerShelli abil määrata loomine

Neid juhiseid järgige täitke-in-the-tühjad lähenemine on Azure virtuaalse masina skaala määramine loomise kohta. Vt lisateavet skaala komplektid [Virtuaalse masina skaala määrab ülevaade](virtual-machine-scale-sets-overview.md) .

Kulub umbes 30 minutit teha selles artiklis toodud juhiseid.

## <a name="step-1-install-azure-powershell"></a>Samm 1: Azure'i PowerShelli installimine

Vaadake, [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md) teavet Azure PowerShelli uusima versiooni installimise, valides tellimuse ja oma kontosse sisse logida.

## <a name="step-2-create-resources"></a>Samm 2: Looge ressursid

Mida on vaja oma uue skaala määramine ressursside loomine.

### <a name="resource-group"></a>Ressursirühm

Virtuaalse masina skaala kogum peavad sisaldama ressursirühma.

1. Kui asetate ressursid asukoht saadaolevate asukohtade loendi hankimine:

        Get-AzureLocation | Sort Name | Select Name

2. Valige asukoht, mis teile sobib, asendage väärtus **$locName** selle asukoha nime ja seejärel looge muutuja:

        $locName = "location name from the list, such as Central US"

3. Asendage nimi, mida soovite kasutada uue ressursirühma ja seejärel looge muutuja **$rgName** väärtus: 

        $rgName = "resource group name"
        
4. Ressursirühma loomiseks tehke järgmist.
    
        New-AzureRmResourceGroup -Name $rgName -Location $locName

    Peaksite nägema midagi, nagu järgmises näites:

        ResourceGroupName : myrg1
        Location          : centralus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/########-####-####-####-############/resourceGroups/myrg1

### <a name="storage-account"></a>Salvestusruumi konto

Salvestusruumi konto kasutatakse virtuaalse masina operatsioonisüsteemi ketta ja skaleerimist kasutatud Diagnostikaandmete talletamiseks. Võimaluse korral on parimaks salvestusruumi konto jaoks iga virtuaalse masina loodud skaala kogum. Kui ei ole võimalik, lepingule kuni 20 vms salvestusruumi konto kohta. Selles artiklis näites luuakse kolme virtuaalmasinates kolm salvestusruumi kontod.

1. Asendage väärtus **$saName** salvestusruumi konto nimi. Testige unikaalsuse nimi. 

        $saName = "storage account name"
        Get-AzureRmStorageAccountNameAvailability $saName

    Kui vastus on **tõene**, on kordumatu teie pakutud nime.

3. Asendage väärtus **$saType** salvestusruumi konto tüüp ja seejärel looge muutuja:  

        $saType = "storage account type"
        
    Võimalikud väärtused on: Standard_LRS, Standard_GRS, Standard_RAGRS või Premium_LRS.
        
4. Konto loomiseks tehke järgmist.
    
        New-AzureRmStorageAccount -Name $saName -ResourceGroupName $rgName –Type $saType -Location $locName

    Peaksite nägema midagi, nagu järgmises näites:

        ResourceGroupName   : myrg1
        StorageAccountName  : myst1
        Id                  : /subscriptions/########-####-####-####-############/resourceGroups/myrg1/providers/Microsoft
                              .Storage/storageAccounts/myst1
        Location            : centralus
        AccountType         : StandardLRS
        CreationTime        : 3/15/2016 4:51:52 PM
        CustomDomain        :
        LastGeoFailoverTime :
        PrimaryEndpoints    : Microsoft.Azure.Management.Storage.Models.Endpoints
        PrimaryLocation     : centralus
        ProvisioningState   : Succeeded
        SecondaryEndpoints  :
        SecondaryLocation   :
        StatusOfPrimary     : Available
        StatusOfSecondary   :
        Tags                : {}
        Context             : Microsoft.WindowsAzure.Commands.Common.Storage.AzureStorageContext

5. Korrake juhiseid 1 – 4, kolme salvestusruumi kontod, näiteks myst1, myst2 ja myst3 loomiseks.

### <a name="virtual-network"></a>Virtuaalse võrgu

Virtuaalse võrk on vaja virtuaalmasinates skaala määramine.

1. Asendage nimi, mida soovite kasutada alamvõrgu virtuaalse võrgu ja seejärel looge muutuja **$subnetName** väärtus: 

        $subnetName = "subnet name"
        
2. Alamvõrgu konfiguratsiooni loomiseks tehke järgmist.
    
        $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
        
    Aadress eesliide võivad olla erinevad virtuaalse võrgu.

3. Asendage nimi, mida soovite kasutada virtuaalse võrgu ja seejärel looge muutuja **$netName** väärtus: 

        $netName = "virtual network name"
        
4. Virtuaalse võrgu loomiseks tehke järgmist.
    
        $vnet = New-AzureRmVirtualNetwork -Name $netName -ResourceGroupName $rgName -Location $locName -AddressPrefix 10.0.0.0/16 -Subnet $subnet

### <a name="configuration-of-the-scale-set"></a>Ulatuse määramine konfigureerimine

Teil on ressursse, mis teil on vaja määrata skaala konfiguratsiooni, seega loome selle.  

1. Asendage nimi, mida soovite kasutada IP-konfiguratsiooni ja seejärel looge muutuja **$ipName** väärtus: 

        $ipName = "IP configuration name"
        
2. IP-konfiguratsiooni loomiseks tehke järgmist.

        $ipConfig = New-AzureRmVmssIpConfig -Name $ipName -LoadBalancerBackendAddressPoolsId $null -SubnetId $vnet.Subnets[0].Id

2. Asendage nimi, mida soovite kasutada ulatuse määramine konfiguratsiooni ja seejärel looge muutuja **$vmssConfig** väärtus:   

        $vmssConfig = "Scale set configuration name"
        
3. Ulatuse määramine konfiguratsiooni loomiseks tehke järgmist.

        $vmss = New-AzureRmVmssConfig -Location $locName -SkuCapacity 3 -SkuName "Standard_A0" -UpgradePolicyMode "manual"
        
    Selles näites skaala määrata loodava koos kolme virtuaalmasinates. Lisateavet skaala komplektid võimsus leiate [Virtuaalse masina skaala määrab ülevaade](virtual-machine-scale-sets-overview.md) . Selles etapis tuleb sisaldab ka säte on virtuaalmasinates (nimetatakse SkuName) mahu määramine. Suuruse, mis vastab teie vajadustele otsimiseks suuruses [virtuaalmasinates jaoks](../virtual-machines/virtual-machines-windows-sizes.md).
    
4. Ulatuse määramine konfiguratsiooni kasutajaliidese võrgukonfiguratsioon lisamiseks tehke järgmist.
        
        Add-AzureRmVmssNetworkInterfaceConfiguration -VirtualMachineScaleSet $vmss -Name $vmssConfig -Primary $true -IPConfiguration $ipConfig
        
    Peaksite nägema midagi, nagu järgmises näites:

        Sku                   : Microsoft.Azure.Management.Compute.Models.Sku
        UpgradePolicy         : Microsoft.Azure.Management.Compute.Models.UpgradePolicy
        VirtualMachineProfile : Microsoft.Azure.Management.Compute.Models.VirtualMachineScaleSetVMProfile
        ProvisioningState     :
        OverProvision         :
        Id                    :
        Name                  :
        Type                  :
        Location              : Central US
        Tags                  :

#### <a name="operating-system-profile"></a>Operatsioonisüsteemi profiil

1. Eesliite arvuti nimi, mida soovite kasutada, ja seejärel looge muutuja **$computerName** väärtus asendamiseks tehke järgmist. 

        $computerName = "computer name prefix"
        
2. Asendage väärtus **$adminName** on virtuaalmasinates administraatori konto nimi ja seejärel looge muutuja:

        $adminName = "administrator account name"
        
3. Asendage väärtus **$adminPassword** konto parooli ja seejärel looge muutuja:

        $adminPassword = "password for administrator accounts"
        
4. Operatsioonisüsteemi profiili loomiseks tehke järgmist.

        Set-AzureRmVmssOsProfile -VirtualMachineScaleSet $vmss -ComputerNamePrefix $computerName -AdminUsername $adminName -AdminPassword $adminPassword

#### <a name="storage-profile"></a>Salvestusruumi profiil

1. Asendage nimi, mida soovite kasutada salvestusruumi profiili ja seejärel looge muutuja **$storageProfile** väärtus:  

        $storageProfile = "storage profile name"
        
2. Muutujad, mis määratlevad pilti kasutada loomiseks tehke järgmist.  
      
        $imagePublisher = "MicrosoftWindowsServer"
        $imageOffer = "WindowsServer"
        $imageSku = "2012-R2-Datacenter"
        
    Muude piltide kasutamise kohta teabe otsimiseks vaadake [navigeerimine ja valige Azure virtuaalse masina Windows PowerShelli ja Azure CLI pilte](../virtual-machines/virtual-machines-windows-cli-ps-findimage.md).
        
3. Asendage väärtus **$vhdContainers** loend, mis sisaldab teed, kus virtuaalse kõvaketta on talletatud, näiteks "https://mystorage.blob.core.windows.net/vhds", ja seejärel looge muutuja:
       
        $vhdContainers = @("https://myst1.blob.core.windows.net/vhds","https://myst2.blob.core.windows.net/vhds","https://myst3.blob.core.windows.net/vhds")
        
4. Salvestusruumi profiili loomiseks tehke järgmist.

        Set-AzureRmVmssStorageProfile -VirtualMachineScaleSet $vmss -ImageReferencePublisher $imagePublisher -ImageReferenceOffer $imageOffer -ImageReferenceSku $imageSku -ImageReferenceVersion "latest" -Name $storageProfile -VhdContainer $vhdContainers -OsDiskCreateOption "FromImage" -OsDiskCaching "None"  

### <a name="virtual-machine-scale-set"></a>Virtuaalse masina skaala määramine

Lisaks saate luua skaala määramine.

1. Asendage väärtus **$vmssName** skaala määramine virtuaalse masina nimi ja seejärel looge muutuja:

        $vmssName = "scale set name"
        
2. Ulatuse määramine loomiseks tehke järgmist.

        New-AzureRmVmss -ResourceGroupName $rgName -Name $vmssName -VirtualMachineScaleSet $vmss

    Peaksite nägema umbes see näide, mis näitab eduka juurutamise:

        Sku                   : Microsoft.Azure.Management.Compute.Models.Sku
        UpgradePolicy         : Microsoft.Azure.Management.Compute.Models.UpgradePolicy
        VirtualMachineProfile : Microsoft.Azure.Management.Compute.Models.VirtualMachineScaleSetVMProfile
        ProvisioningState     : Updating
        OverProvision         :
        Id                    : /subscriptions/########-####-####-####-############/resourceGroups/myrg1/providers/Microso
                                ft.Compute/virtualMachineScaleSets/myvmss1
        Name                  : myvmss1
        Type                  : Microsoft.Compute/virtualMachineScaleSets
        Location              : centralus
        Tags                  :

## <a name="step-3-explore-resources"></a>Samm 3: Uurimine ressursid

Järgmiste ressursside kaudu uurida loodud virtuaalse masina skaala määramine.

- Azure'i portaal - piiratud hulk teavet on saadaval portaalis.
- [Azure'i Resource Explorer](https://resources.azure.com/) – see tööriist on parim oma skaala määramine hetkeseisu uurimine.
- Azure'i PowerShelli – kasutage teabe saamiseks selle käsu:

        Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"
        
        Or 
        
        Get-AzureRmVmssVM -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"
        

## <a name="next-steps"></a>Järgmised sammud

- Ulatuse määramine, et teie loodud teabe abil [hallata virtuaalmasinates virtuaalse masina skaala Set](virtual-machine-scale-sets-windows-manage.md) haldamine
- Kaaluge võimalust seada üles automaatset mastaapimist seada [automaatse suuruse muutmise ja virtuaalse masina skaala määrab](virtual-machine-scale-sets-autoscale-overview.md) teabe abil oma skaala
- Lisateavet vertikaalne skaleerimist teguriga [vertikaalne autoscale koos virtuaalse masina skaala komplekti](virtual-machine-scale-sets-vertical-scale-reprovision.md) läbivaatamine
