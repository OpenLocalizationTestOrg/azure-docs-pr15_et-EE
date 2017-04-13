<properties 
   pageTitle="Kuidas määrata sisemise privaatne staatiline"
   description="Sisemine staatiline IP (DIPs) ja kuidas neid hallata mõistmine"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/22/2016"
   ms.author="jdial" />

# <a name="how-to-set-a-static-internal-private-ip-address-using-powershell-classic"></a>Kuidas määrata staatilise sisemise privaatne IP-aadress (klassikaline) PowerShelli abil
Enamikul juhtudel peate ei määrata staatiline sisemise IP-aadress virtuaalne arvuti. VMs virtuaalse võrku, kuvatakse automaatselt vahemik, mis teie määratud sisemine IP-aadress. Kuid teatud juhtudel määramine kindla VM staatiline IP-aadress on mõistlik. Näiteks kui teie VM saab käitada DNS-i või domeenikontrolleri. Sisemine staatiline IP jääb VM isegi läbi Peata/deprovision olekus. 

> [AZURE.IMPORTANT] Azure'i on kaks eri juurutamise mudelite loomise ja ressursside töötamine: [ressursihaldur ja klassikaline](../resource-manager-deployment-model.md). Selles artiklis antakse ülevaade klassikaline juurutamise mudeli abil. Microsoft soovitab, et kõige uue juurutuste [ressursihaldur juurutamise mudeli](virtual-networks-static-private-ip-arm-ps.md)kasutamine.

## <a name="how-to-verify-if-a-specific-ip-address-is-available"></a>Kuidas kontrollida, kas teatud IP-aadress on saadaval
Veenduge, et kui IP-aadress *10.0.0.7* on saadaval on vnet nimega *TestVnet*, käivitage järgmine käsk PowerShelli ja veenduge väärtuse jaoks *IsAvailable*.

    Test-AzureStaticVNetIP –VNetName TestVNet –IPAddress 10.0.0.7 

    IsAvailable          : True
    AvailableAddresses   : {}
    OperationDescription : Test-AzureStaticVNetIP
    OperationId          : fd3097e1-5f4b-9cac-8afa-bba1e3492609
    OperationStatus      : Succeeded

>[AZURE.NOTE] Kui soovite kontrollimiseks ülaltoodud käsu turvalises keskkonnas jälgi juhised [loomine virtuaalse võrgu](virtual-networks-create-vnet-classic-portal.md) loomiseks on vnet nimega *TestVnet* ja veenduge, et see kasutab *10.0.0.0/8* aadressiruumi.

## <a name="how-to-specify-a-static-internal-ip-when-creating-a-vm"></a>Kuidas määrata sisemine staatiline VM loomisel
PowerShelli skripti allpool loob uue nimega *TestService*pilveteenus, seejärel toob pildi Azure, ja seejärel loob VM nimega *TestVM* uue pilveteenusesse, kasutades välja pilt, määrab VM olema alamvõrku, mis on nimega *alamvõrgu-1*ja seab *10.0.0.7* sisemine staatiline VM.

    New-AzureService -ServiceName TestService -Location "Central US"
    $image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}
    New-AzureVMConfig -Name TestVM -InstanceSize Small -ImageName $image.ImageName `
  	| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
  	| Set-AzureSubnet –SubnetNames Subnet-1 `
  	| Set-AzureStaticVNetIP -IPAddress 10.0.0.7 `
  	| New-AzureVM -ServiceName "TestService" –VNetName TestVnet

## <a name="how-to-retrieve-static-internal-ip-information-for-a-vm"></a>Kuidas hankida sisemine staatiline IP teave VM
Ülaltoodud skripti loodud VM staatilise sisemise IP teabe vaatamiseks käivitage järgmine käsk PowerShelli ja jälgitakse väärtuste *sordib*.

    Get-AzureVM -Name TestVM -ServiceName TestService

    DeploymentName              : TestService
    Name                        : TestVM
    Label                       : 
    VM                          : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.PersistentVM
    InstanceStatus              : Provisioning
    IpAddress                   : 10.0.0.7
    InstanceStateDetails        : Windows is preparing your computer for first use...
    PowerState                  : Started
    InstanceErrorCode           : 
    InstanceFaultDomain         : 0
    InstanceName                : TestVM
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

## <a name="how-to-remove-a-static-internal-ip-from-a-vm"></a>Kuidas eemaldada sisemine staatiline VM
Lisatud VM ülaltoodud skripti sisemine staatiline IP eemaldamiseks käivitage PowerShelli järgmine käsk:
    
    Get-AzureVM -ServiceName TestService -Name TestVM `
  	| Remove-AzureStaticVNetIP `
  	| Update-AzureVM

## <a name="how-to-add-a-static-internal-ip-to-an-existing-vm"></a>Kuidas lisada mõne olemasoleva VM sisemine staatiline
Sisemine staatiline lisamiseks IP VM loodud kasutades skripti ülaltoodud umbes ta järgmine käsk:

    Get-AzureVM -ServiceName TestService000 -Name TestVM `
  	| Set-AzureStaticVNetIP -IPAddress 10.10.0.7 `
  	| Update-AzureVM

## <a name="next-steps"></a>Järgmised sammud

[Reserveeritud IP](virtual-networks-reserved-public-ip.md)

[Eksemplari taseme avaliku IP (ILPIP)](virtual-networks-instance-level-public-ip.md)

[Reserveeritud IP REST API-d](https://msdn.microsoft.com/library/azure/dn722420.aspx)
 
