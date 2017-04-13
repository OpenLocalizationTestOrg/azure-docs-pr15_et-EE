<properties 
   pageTitle="Näiteks tase avaliku IP (ILPIP) | Microsoft Azure'i"
   description="ILPIP (PIP) ja kuidas neid hallata mõistmine"
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
   ms.date="02/10/2016"
   ms.author="jdial" />

# <a name="instance-level-public-ip-overview"></a>Eksemplari taseme avaliku IP ülevaade
Näiteks taseme avaliku IP (ILPIP) on avaliku IP-aadress, mida saate määrata otse oma VM või rolli eksemplari, mitte oma VM või rolli eksemplari paiknevaid pilveteenuses. See ei toimu VIP (virtuaalne IP), mis on seotud teie pilveteenuses. Pigem on täiendavad IP-aadress, mille abil saate oma VM või rolli eksemplari loomiseks.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Siit saate teada, kuidas [teha neid juhiseid kasutades ressursihaldur mudel](virtual-network-ip-addresses-overview-arm.md). 

Veenduge, et teil mõista, kuidas [IP-aadressid](virtual-network-ip-addresses-overview-classic.md) töötavad Azure.

>[AZURE.NOTE] Varem on ILPIP viidatud kui PIP, mis tähistab avaliku IP. 

![ILPIP ja VIP erinevus](./media/virtual-networks-instance-level-public-ip/Figure1.png)

Nagu on näidatud joonisel 1, pilveteenusesse pääseb abil VIP, samal ajal üksikuid VMs pääseb tavaliselt VIP abil:&lt;pordi number&gt;. Määrate mõne ILPIP teatud VM, et VM pääseb juurde otse abil IP-aadress.

Azure'i pilveteenus loomisel vastavaid A DNS-i kirjeid luuakse automaatselt täielik domeeninimi (FQDN) kaudu juurdepääsu lubamiseks tegelik VIP kasutamise asemel. Sama toimingut juhtub ILPIP, VM või rolli eksemplari FQDN asemel kuvatakse ILPIP, mis võimaldavad. Näiteks kui loote nimega *contosoadservice*pilveteenus ja saate konfigureerida web rolli nimega *contosoweb* koos kahe eksemplari, Azure'i register järgmine A kirjed linnanimede jaoks:

- contosoweb\_IN_0.contosoadservice.cloudapp.net
- contosoweb\_IN_1.contosoadservice.cloudapp.net 

>[AZURE.NOTE] Saate määrata ainult üks ILPIP iga VM või rolli eksemplari. Saate kasutada kuni 5 ILPIP isiku tellimuse kohta. Sel ajal, ei toetata ILPIP mitme-NIC vms.

## <a name="why-should-i-request-an-ilpip"></a>Miks ma soovin mõne ILPIP?
Kui soovite luua ühenduse oma VM või rolli eksemplari, IP-aadress see otse määratud, pilve kasutamise asemel teenuse VIP:&lt;pordi number&gt;, on ILPIP taotlus oma VM või oma rolli eksemplari.
- **Passiivne FTP** -, millel on ILPIP oma VM, võite saada liikluse praktiliselt igas Port te ei pea lõpp saada liikluse avada. See võimaldab stsenaariumid nagu passiivne FTP, kus valitud dünaamiliselt pordid.
- **Väljaminevate IP** - pärinevate VM väljaminev liiklus läheb ILPIP allikana ja see tuvastab kordumatult väliste üksuste VM.

## <a name="how-to-request-an-ilpip-during-vm-creation"></a>Kuidas taotleda mõne ILPIP VM loomise ajal
PowerShelli skripti allpool loob uue pilveteenus nimega *FTPService*, siis toob pildi Azure, loob VM nimega *FTPInstance* kasutades välja pilt, määrab VM kasutada ka ILPIP ja VM lisatakse uus teenus:

    New-AzureService -ServiceName FTPService -Location "Central US"
    $image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}
    New-AzureVMConfig -Name FTPInstance -InstanceSize Small -ImageName $image.ImageName `
  	| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
  	| Set-AzurePublicIP -PublicIPName ftpip | New-AzureVM -ServiceName FTPService -Location "Central US"

## <a name="how-to-retrieve-ilpip-information-for-a-vm"></a>Kuidas hankida VM ILPIP teave
Ülaltoodud skripti loodud VM ILPIP teabe kuvamiseks käivitage järgmine käsk PowerShelli ja jälgida väärtused *PublicIPAddress* ja *PublicIPName*:

    Get-AzureVM -Name FTPInstance -ServiceName FTPService

    DeploymentName              : FTPService
    Name                        : FTPInstance
    Label                       : 
    VM                          : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.PersistentVM
    InstanceStatus              : ReadyRole
    IpAddress                   : 100.74.118.91
    InstanceStateDetails        : 
    PowerState                  : Started
    InstanceErrorCode           : 
    InstanceFaultDomain         : 0
    InstanceName                : FTPInstance
    InstanceUpgradeDomain       : 0
    InstanceSize                : Small
    HostName                    : FTPInstance
    AvailabilitySetName         : 
    DNSName                     : http://ftpservice888.cloudapp.net/
    Status                      : ReadyRole
    GuestAgentStatus            : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.GuestAgentStatus
    ResourceExtensionStatusList : {Microsoft.Compute.BGInfo}
    PublicIPAddress             : 104.43.142.188
    PublicIPName                : ftpip
    NetworkInterfaces           : {}
    ServiceName                 : FTPService
    OperationDescription        : Get-AzureVM
    OperationId                 : 568d88d2be7c98f4bbb875e4d823718e
    OperationStatus             : OK

## <a name="how-to-remove-an-ilpip-from-a-vm"></a>Kuidas eemaldada mõne ILPIP VM
ILPIP, lisatakse VM ülaltoodud skripti eemaldamiseks käivitage PowerShelli järgmine käsk:
    
    Get-AzureVM -ServiceName FTPService -Name FTPInstance `
  	| Remove-AzurePublicIP `
  	| Update-AzureVM

## <a name="how-to-add-an-ilpip-to-an-existing-vm"></a>Kuidas lisada mõne ILPIP mõne olemasoleva VM
Lisada mõne ILPIP ülaltoodud skripti abil loodud VM, käivitage järgmine käsk:

    Get-AzureVM -ServiceName FTPService -Name FTPInstance `
  	| Set-AzurePublicIP -PublicIPName ftpip2 `
  	| Update-AzureVM

## <a name="how-to-associate-an-ilpip-to-a-vm-by-using-a-service-configuration-file"></a>Kuidas siduda mõne ILPIP VM teenuse konfiguratsiooni faili abil
Teenuse konfigureerimine (CSCFG) faili abil saate seostada ka mõne ILPIP VM. Valimi XML-i allpool näitab, kuidas konfigureerida pilveteenuses kasutada ka ILPIP nimega *MyPublicIP* rolli eksemplari: 
    
    <?xml version="1.0" encoding="utf-8"?>
    <ServiceConfiguration serviceName="ReservedIPSample" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="4" osVersion="*" schemaVersion="2014-01.2.3">
      <Role name="WebRole1">
        <Instances count="1" />
        <ConfigurationSettings>
          <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true" />
        </ConfigurationSettings>
      </Role>
      <NetworkConfiguration>
        <VirtualNetworkSite name="VNet"/>
        <AddressAssignments>
          <InstanceAddress roleName="VMRolePersisted">
            <Subnets>
              <Subnet name="Subnet1"/>
              <Subnet name="Subnet2"/>
            </Subnets>
            <PublicIPs>
              <PublicIP name="MyPublicIP" domainNameLabel="MyPublicIP" />
            </PublicIPs>
          </InstanceAddress>
        </AddressAssignments>
      </NetworkConfiguration>
    </ServiceConfiguration>

## <a name="next-steps"></a>Järgmised sammud

- Mõista, kuidas [IP tegelemine](virtual-network-ip-addresses-overview-classic.md) töötab klassikaline juurutamise mudeli.

- Lisateavet [reserveeritud IP-d](virtual-networks-reserved-public-ip.md).
 