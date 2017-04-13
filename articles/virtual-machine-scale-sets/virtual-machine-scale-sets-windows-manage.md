<properties
    pageTitle="Hallata virtuaalse masina skaala komplekti VMs | Microsoft Azure'i"
    description="Hallata virtuaalmasinates skaalal virtuaalse masina määramine Azure PowerShelli kaudu."
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
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="davidmu"/>

# <a name="manage-virtual-machines-in-a-virtual-machine-scale-set"></a>Virtuaalmasinates virtuaalse masina skaala komplekti haldamine

Selles artiklis tööülesanded abil hallata oma virtuaalse masina skaala komplekti virtuaalmasinates.

Enamik tööülesanded hõlmavad haldamise virtuaalse masina skaala kogum nõua, et te ei tea arvuti, mida soovite hallata eksemplari ID-d. [Azure'i ressursi Exploreri](https://resources.azure.com) abil saate leida kogumi skaala virtuaalse masina eksemplari ID-d. Saate kasutada ka ressursi Exploreri lõpetamist tööülesandeid oleku kontrollimine.

Vaadake, [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md) teavet Azure PowerShelli uusima versiooni installimise, valides tellimuse ja oma kontosse sisse logida.

## <a name="display-information-about-a-scale-set"></a>Kuvada teavet skaala määramine

Saate üldteavet skaala kogum, mis on ka edaspidi eksemplari vaade. Või saate täpsemale teabele, näiteks teavet ressursside skaala määramine.

Asendage nimi või oma ressursirühm ja skaala määramine ja seejärel käivitage käsk pakutud väärtused:

    Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"

See tagastab umbes selline:

    Id                                          : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Compute/virtualMachineScaleSets/myvmss1
    Name                                        : myvmss1
    Type                                        : Microsoft.Compute/virtualMachineScaleSets
    Location                                    : centralus
    Sku                                         :
      Name                                      : Standard_A0
      Tier                                      : Standard
      Capacity                                  : 3
    UpgradePolicy                               :
      Mode                                      : Manual
    VirtualMachineProfile                       :
      OsProfile                                 :
        ComputerNamePrefix                      : vmss1
        AdminUsername                           : admin1
        WindowsConfiguration                    :
          ProvisionVMAgent                      : True
          EnableAutomaticUpdates                : True
    StorageProfile                              :
      ImageReference                            :
        Publisher                               : MicrosoftWindowsServer
        Offer                                   : WindowsServer
        Sku                                     : 2012-R2-Datacenter
        Version                                 : latest
      OsDisk                                    :
        Name                                    : vmssosdisk
        Caching                                 : ReadOnly
        CreateOption                            : FromImage
        VhdContainers[0]                        : https://astore.blob.core.windows.net/vmss
        VhdContainers[1]                        : https://gstore.blob.core.windows.net/vmss
        VhdContainers[2]                        : https://mstore.blob.core.windows.net/vmss
        VhdContainers[3]                        : https://sstore.blob.core.windows.net/vmss
        VhdContainers[4]                        : https://ystore.blob.core.windows.net/vmss
    NetworkProfile                              :
      NetworkInterfaceConfigurations[0]         :
        Name                                    : mync1
        Primary                                 : True
        IpConfigurations[0]                     :
          Name                                  : ip1
          Subnet                                :
            Id                                  : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Network/virtualNetworks/myvn1/subnets/mysn1
          LoadBalancerBackendAddressPools[0]    :
            Id                                  : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Network/loadBalancers/mylb1/backendAddressPools/bepool1
        LoadBalancerInboundNatPools[0]          :
            Id                                  : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Network/loadBalancers/mylb1/inboundNatPools/natpool1
    ExtensionProfile                            :
      Extensions[0]                             :
        Name                                    : Microsoft.Insights.VMDiagnosticsSettings
        Publisher                               : Microsoft.Azure.Diagnostics
        Type                                    : IaaSDiagnostics
        TypeHandlerVersion                      : 1.5
        AutoUpgradeMinorVersion                 : True
        Settings                                : {"xmlCfg":"...","storageAccount":"astore"}
    ProvisioningState                           : Succeeded
    
Pakutud väärtuste asendamine ressursside ja komplekti nimi. Asendage *#* virtuaalse masina, mida soovite teavet saada ja seejärel käivitage see identifikaatoriga eksemplari:

    Get-AzureRmVmssVM -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #
        
See tagastab midagi, nagu järgmises näites:

    Id                            : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Compute/
                                    virtualMachineScaleSets/myvmss1/virtualMachines/0
    Name                          : myvmss1_0
    Type                          : Microsoft.Compute/virtualMachineScaleSets/virtualMachines
    Location                      : centralus
    InstanceId                    : 0
    Sku                           :
      Name                        : Standard_A0
      Tier                        : Standard
    LatestModelApplied            : True
    StorageProfile                :
      ImageReference              :
        Publisher                 : MicrosoftWindowsServer
        Offer                     : WindowsServer
        Sku                       : 2012-R2-Datacenter
        Version                   : 4.0.20160617
      OsDisk                      :
        OsType                    : Windows
        Name                      : vmssosdisk-os-0-e11cad52959b4b76a8d9f26c5190c4f8
        Vhd                       :
          Uri                     : https://astore.blob.core.windows.net/vmss/vmssosdisk-os-0-e11cad52959b4b76a8d9f26c5190c4f8.vhd
        Caching                   : ReadOnly
        CreateOption              : FromImage
    OsProfile                     :
      ComputerName                : myvmss1-0
      AdminUsername               : admin1
      WindowsConfiguration        :
        ProvisionVMAgent          : True
        EnableAutomaticUpdates    : True
    NetworkProfile                :
      NetworkInterfaces[0]        :
        Id                        : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Compute/virtualMachineScaleSets/
                                    myvmss1/virtualMachines/0/networkInterfaces/mync1
    ProvisioningState             : Succeeded
    Resources[0]                  :
      Id                          : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Compute/virtualMachines/
                                    myvmss1_0/extensions/Microsoft.Insights.VMDiagnosticsSettings
      Name                        : Microsoft.Insights.VMDiagnosticsSettings
      Type                        : Microsoft.Compute/virtualMachines/extensions
      Location                    : centralus
      Publisher                   : Microsoft.Azure.Diagnostics
      VirtualMachineExtensionType : IaaSDiagnostics
      TypeHandlerVersion          : 1.5
      AutoUpgradeMinorVersion     : True
      Settings                    : {"xmlCfg":"...","storageAccount":"astore"}
      ProvisioningState           : Succeeded
        
## <a name="start-a-virtual-machine-in-a-scale-set"></a>Käivitada kogumi skaala

Pakutud väärtuste asendamine ressursside ja komplekti nimi. Asendage *#* virtuaalse masina, mida soovite käivitada ja seejärel selle identifikaatoriga:

    Start-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #

Ressursi Exploreris näeme, et eksemplari oleku on **töötab**:

    "statuses": [
      {
        "code": "ProvisioningState/succeeded",
        "level": "Info",
        "displayStatus": "Provisioning succeeded",
        "time": "2016-03-15T02:10:08.0730839+00:00"
      },
      {
        "code": "PowerState/running",
        "level": "Info",
        "displayStatus": "VM running"
      }
    ]

Saate alustada kõik virtuaalmasinates skaalal - InstanceId parameetri abil ei määrata.
    
## <a name="stop-a-virtual-machine-in-a-scale-set"></a>Ulatuse määramine virtuaalse masina peatamine

Pakutud väärtuste asendamine ressursside ja komplekti nimi. Asendage *#* identifikaatoriga virtuaalse masina, mida soovite peatada ja seejärel käivitage see:

    Stop-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #

Ressursi Exploreris näeme, et eksemplari oleku on **deallocated**:

    "statuses": [
      {
        "code": "ProvisioningState/succeeded",
        "level": "Info",
        "displayStatus": "Provisioning succeeded",
        "time": "2016-03-15T01:25:17.8792929+00:00"
      },
      {
        "code": "PowerState/deallocated",
        "level": "Info",
        "displayStatus": "VM deallocated"
      }
    ]
    
Virtuaalse masina peatada ja seda ei deallocate, kasutage parameetrit - StayProvisioned. Mitte - InstanceId parameetri abil saate peatada kõik virtuaalmasinates määramine.
    
## <a name="restart-a-virtual-machine-in-a-scale-set"></a>Taaskäivitage virtuaalse masina kogumi skaala

Asendage nimi oma ressursirühm ja skaala määramine pakutud väärtused. Asendage *#* identifikaatoriga virtuaalse masina, mida soovite uuesti ja seejärel käivitage see:

    Restart-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #
    
Kõik virtuaalmasinates taaskäivitage määramine ei kasuta - InstanceId parameeter.

## <a name="remove-a-virtual-machine-from-a-scale-set"></a>Ulatuse määramine virtuaalse masina eemaldamine

Asendage nimi oma ressursirühm ja skaala määramine pakutud väärtused. Asendage *#* identifikaatoriga virtuaalse masina, mille soovite eemaldada, ja seejärel käivitage see:  

    Remove-AzureRmVmss -ResourceGroupName "resource group name" –VMScaleSetName "scale set name" -InstanceId #

Mitte - InstanceId parameetri abil saate virtuaalse masina skaala määramine kõik korraga eemaldada.

## <a name="change-the-capacity-of-a-scale-set"></a>Ulatuse määramine võimsus muutmine

Saate lisada või eemaldada, muutes määramine võimsus virtuaalmasinates. Ulatuse määramine, mida soovite muuta, võimsus määramine, mida soovite, et see ja seejärel värskendage seadistatud uue mahuga skaala hankimine Asendage need käsud nimi oma ressursirühm ja skaala määramine pakutud väärtused.

  $vmss = get-AzureRmVmss - ResourceGroupName "ressursi rühma nimi" - VMScaleSetName "skaala määrata nimi" $vmss.sku.capacity = 5 värskendus-AzureRmVmss - ResourceGroupName "ressursi rühma nimi"-nime "mastaapimiseks hulga nimi" - VirtualMachineScaleSet $vmss 

Kui eemaldate virtuaalmasinates skaala määramine, eemaldatakse esmalt virtuaalmasinates koos kõrgeim ID-d.
