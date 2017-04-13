<properties 
   pageTitle="Privaatne staatiline IP määramine klassikaline režiim PowerShelli kaudu | Microsoft Azure'i"
   description="Privaatne staatiline IP (DIPs) ja kuidas neid hallata klassikaline režiim ja PowerShelli mõistmine"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
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

# <a name="how-to-set-a-static-private-ip-address-classic-in-powershell"></a>Kuidas määrata staatilise privaatne IP-aadress (klassikaline) PowerShellis

[AZURE.INCLUDE [virtual-networks-static-private-ip-selectors-classic-include](../../includes/virtual-networks-static-private-ip-selectors-classic-include.md)]

[AZURE.INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Selles artiklis antakse ülevaade klassikaline juurutamise mudel. Samuti saate [hallata staatilise privaatne IP-aadress ressursihaldur juurutamise mudeli](virtual-networks-static-private-ip-arm-ps.md).

[AZURE.INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

Valimi PowerShelli käske allpool oodatud lihtne keskkond juba loonud. Kui soovite käivitada käske, nagu need on kuvatud selles dokumendis, esmalt koostada testimiskeskkonnas, [Loo on VNet](virtual-networks-create-vnet-classic-netcfg-ps.md)kirjeldatud.

## <a name="how-to-verify-if-a-specific-ip-address-is-available"></a>Kuidas kontrollida, kas teatud IP-aadress on saadaval
Veenduge, et kui IP-aadress *192.168.1.101* on saadaval on VNet nimega *TestVNet*, käivitage järgmine käsk PowerShelli ja veenduge väärtuse jaoks *IsAvailable*.

    Test-AzureStaticVNetIP –VNetName TestVNet –IPAddress 192.168.1.101 

Oodatav väljund:

    IsAvailable          : True
    AvailableAddresses   : {}
    OperationDescription : Test-AzureStaticVNetIP
    OperationId          : fd3097e1-5f4b-9cac-8afa-bba1e3492609
    OperationStatus      : Succeeded

## <a name="how-to-specify-a-static-private-ip-address-when-creating-a-vm"></a>Kuidas määrata staatilise privaatne IP-aadress VM loomisel
PowerShelli skripti allpool loob uue pilveteenus nimega *TestService*, siis toob pildi Azure, loob VM nimega *DNS01* uue pilveteenusesse, kasutades välja pilt, määrab VM alamvõrgu, mis on *FrontEnd*nimega olema ja seab *192.168.1.7* VM staatilise privaatne IP-aadress:

    New-AzureService -ServiceName TestService -Location "Central US"
    $image = Get-AzureVMImage | where {$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}
    New-AzureVMConfig -Name DNS01 -InstanceSize Small -ImageName $image.ImageName |
      Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! |
      Set-AzureSubnet –SubnetNames FrontEnd |
      Set-AzureStaticVNetIP -IPAddress 192.168.1.7 |
      New-AzureVM -ServiceName TestService –VNetName TestVNet

Oodatav väljund:

    WARNING: No deployment found in service: 'TestService'.
    OperationDescription OperationId                          OperationStatus
    -------------------- -----------                          ---------------
    New-AzureService     fcf705f1-d902-011c-95c7-b690735e7412 Succeeded      
    New-AzureVM          3b99a86d-84f8-04e5-888e-b6fc3c73c4b9 Succeeded  

## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a>Kuidas hankida staatilise privaatne IP-aadressi teave VM
Staatilise privaatne IP-aadressi teavet VM loodud ülaltoodud skripti kuvamiseks käivitage järgmine käsk PowerShelli ja jälgitakse väärtused *sordib*:

    Get-AzureVM -Name DNS01 -ServiceName TestService

Oodatav väljund:

    DeploymentName              : TestService
    Name                        : DNS01
    Label                       : 
    VM                          : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.PersistentVM
    InstanceStatus              : Provisioning
    IpAddress                   : 192.168.1.7
    InstanceStateDetails        : Windows is preparing your computer for first use...
    PowerState                  : Started
    InstanceErrorCode           : 
    InstanceFaultDomain         : 0
    InstanceName                : DNS01
    InstanceUpgradeDomain       : 0
    InstanceSize                : Small
    HostName                    : rsR2-797
    AvailabilitySetName         : 
    DNSName                     : http://testservice000.cloudapp.net/
    Status                      : Provisioning
    GuestAgentStatus            : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.GuestAgentStatus
    ResourceExtensionStatusList : {Microsoft.Compute.BGInfo}
    PublicIPAddress             : 
    PublicIPName                : 
    NetworkInterfaces           : {}
    ServiceName                 : TestService
    OperationDescription        : Get-AzureVM
    OperationId                 : 34c1560a62f0901ab75cde4fed8e8bd1
    OperationStatus             : OK

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a>Kuidas eemaldada VM staatilise privaatne IP-aadress
Eemaldamiseks staatilise privaatne IP-aadress lisatakse VM eeltoodud skripti PowerShelli järgmine käsk:
    
    Get-AzureVM -ServiceName TestService -Name DNS01 |
      Remove-AzureStaticVNetIP |
      Update-AzureVM

Oodatav väljund:

    OperationDescription OperationId                          OperationStatus
    -------------------- -----------                          ---------------
    Update-AzureVM       052fa6f6-1483-0ede-a7bf-14f91f805483 Succeeded

## <a name="how-to-add-a-static-private-ip-address-to-an-existing-vm"></a>Kuidas lisada mõne olemasoleva VM staatilise privaatne IP-aadress
Staatiline lisamiseks privaatne IP-aadressi ülaltoodud umbes skripti abil loodud VM ta järgmine käsk:

    Get-AzureVM -ServiceName TestService -Name DNS01 |
      Set-AzureStaticVNetIP -IPAddress 192.168.1.7 |
      Update-AzureVM

Oodatav väljund:

    OperationDescription OperationId                          OperationStatus
    -------------------- -----------                          ---------------
    Update-AzureVM       77d8cae2-87e6-0ead-9738-7c7dae9810cb Succeeded 

## <a name="next-steps"></a>Järgmised sammud

- Lisateavet [reserveeritud avaliku IP](virtual-networks-reserved-public-ip.md) -aadressid.
- Lisateavet [astme avaliku IP (ILPIP)](virtual-networks-instance-level-public-ip.md) aadresside kohta.
- Konsulteerige [reserveeritud IP REST API-d](https://msdn.microsoft.com/library/azure/dn722420.aspx).
