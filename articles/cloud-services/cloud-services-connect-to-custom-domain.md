<properties
  pageTitle="Kohandatud domeenikontrolleri pilveteenus ühenduse | Microsoft Azure'i"
  description="Siit saate teada, kuidas luua ühendus oma web/töötaja rollid kohandatud domeeni AD PowerShelli ja AD domeeni laiend abil"
  services="cloud-services"
  documentationCenter=""
  authors="Thraka"
  manager="timlt"
  editor=""/>

  <tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/21/2016"
    ms.author="adegeo"/>

# <a name="connecting-azure-cloud-services-roles-to-a-custom-ad-domain-controller-hosted-in-azure"></a>Ühenduse luua kohandatud majutatud Azure AD domeenikontrolleri Azure'i Cloud Services rollid

Me esmalt Azure virtuaalse võrgu (VNet) seada. Seejärel lisame Active Directory domeenikontrolleri, (majutatud kohta on Azure virtuaalse masina) on VNet. Järgmiseks me varem loodud VNet lisamine olemasoleva pilvepõhise teenuse rollid ja ühendada need hiljem domeenikontrolleri.

Enne kui me alustamiseks paar asja meeles pidada:

1.  Selle õpetuse kasutab PowerShelli, nii et palun veenduge, et teil on installitud Azure PowerShelli ja kasutamiseks valmis. Azure'i PowerShelli häälestamise abi saamiseks vaadake, [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md).

2.  AD domeenikontrolleri ning Web/töötaja rolli eksemplaride peavad olema selle VNet.

Järgige samm-sammult juhendi ja kui teil tekib probleeme, kommentaar meile allpool. Keegi saab teile tagasi (Jah, saame lugeda kommentaarid).

3. Mis on pilv viidatud võrgu teenuse <mark>peab olema</mark> **klassikaline virtuaalse võrgu**.

## <a name="create-a-virtual-network"></a>Virtuaalse võrgu loomine

Azure'i Azure klassikaline portaali või PowerShelli abil saate luua virtuaalse võrgus. Selles õpetuses kasutame PowerShelli. Azure'i klassikaline portaalis virtuaalse võrgu loomiseks vaadake teemat [Virtuaalse võrgu loomine](../virtual-network/virtual-networks-create-vnet-arm-pportal.md).

```powershell
#Create Virtual Network

$vnetStr =
@"<?xml version="1.0" encoding="utf-8"?>
<NetworkConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
    <VirtualNetworkConfiguration>
    <VirtualNetworkSites>
        <VirtualNetworkSite name="[your-vnet-name]" Location="West US">
        <AddressSpace>
            <AddressPrefix>[your-address-prefix]</AddressPrefix>
        </AddressSpace>
        <Subnets>
            <Subnet name="[your-subnet-name]">
            <AddressPrefix>[your-subnet-range]</AddressPrefix>
            </Subnet>
        </Subnets>
        </VirtualNetworkSite>
    </VirtualNetworkSites>
    </VirtualNetworkConfiguration>
</NetworkConfiguration>
"@;

$vnetConfigPath = "<path-to-vnet-config>"
Set-AzureVNetConfig -ConfigurationPath $vnetConfigPath
```

## <a name="create-a-virtual-machine"></a>Luua virtuaalse masina

Kui olete lõpetanud virtuaalse võrgu häälestamiseks, peate mõne AD domeenikontrolleri loomiseks. Selles õpetuses mõeldud me seadistada AD domeenikontrolleri kohta on Azure virtuaalse masina.

Selleks luua virtuaalse masina allpool käskude abil PowerShelli kaudu:

```powershell
# Initialize variables
# VNet and subnet must be classic virtual network resources, not Azure Resource Manager resources.

$vnetname = '<your-vnet-name>'
$subnetname = '<your-subnet-name>'
$vmsvc1 = '<your-hosted-service>'
$vm1 = '<your-vm-name>'
$username = '<your-username>'
$password = '<your-password>'
$affgrp = '<your- affgrp>'

# Create a VM and add it to the Virtual Network

New-AzureQuickVM -Windows -ServiceName $vmsvc1 -Name $vm1 -ImageName $imgname -AdminUsername $username -Password $password -AffinityGroup $affgrp -SubnetNames $subnetname -VNetName $vnetname
```

## <a name="promote-your-virtual-machine-to-a-domain-controller"></a>Saab reklaamida oma virtuaalse masina domeenikontrolleri
Kui ka AD domeenikontrolleri konfigureerimiseks virtuaalse masina, peate VM sisse logida ja seda konfigureerida.

VM sisse logida, saate hankida RDP-faili PowerShelli kaudu, kasutage alltoodud käske.

```powershell
# Get RDP file
Get-AzureRemoteDesktopFile -ServiceName $vmsvc1 -Name $vm1 -LocalPath <rdp-file-path>
```

Kui te olete sisse logitud rakendusse VM, Install Virtual arvuti kui ka AD domeenikontrolleri järgides samm-sammult juhendi kohta, [Kuidas häälestada oma kliendi AD domeenikontrolleri](http://social.technet.microsoft.com/wiki/contents/articles/12370.windows-server-2012-set-up-your-first-domain-controller-step-by-step.aspx).

## <a name="add-your-cloud-service-to-the-virtual-network"></a>Lisage oma pilveteenuses virtuaalse võrku

Järgmiseks peate VNet, vastloodud juurutamise pilvepõhise teenuse lisamine. Selleks muuta oma pilvepõhise teenuse cscfg asjakohaste jaotiste lisamine oma cscfg Visual Studio või teie valitud redaktori abil.

```xml
<ServiceConfiguration serviceName="[hosted-service-name]" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="[os-family]" osVersion="*">
    <Role name="[role-name]">
    <Instances count="[number-of-instances]" />
    </Role>
    <NetworkConfiguration>

    <!--optional-->
    <Dns>
        <DnsServers><DnsServer name="[dns-server-name]" IPAddress="[ip-address]" /></DnsServers>
    </Dns>
    <!--optional-->

    <!--VNet settings
        VNet and subnet must be classic virtual network resources, not Azure Resource Manager resources.-->
    <VirtualNetworkSite name="[virtual-network-name]" />
    <AddressAssignments>
        <InstanceAddress roleName="[role-name]">
        <Subnets>
            <Subnet name="[subnet-name]" />
        </Subnets>
        </InstanceAddress>
    </AddressAssignments>
    <!--VNet settings-->

    </NetworkConfiguration>
</ServiceConfiguration>
```

Järgmiseks cloud services projekti koostamine ja selle juurutama Azure. Juurutamine oma cloud services pakett Azure abi saamiseks vaadake, [Kuidas loomine ja Deploy pilveteenus](cloud-services-how-to-create-deploy.md#deploy)

## <a name="connect-your-webworker-roles-to-the-domain"></a>Ühenduse loomine oma web/töötaja role(s) domeeni

Kui teenus cloud projekti on juurutatud Azure, ühenduse abil AD domeeni laiend AD domeeni oma rolli aknad. Olemasoleva cloud services juurutamise laiend AD domeeni lisada ja Liituge kohandatud domeeni, käivitada PowerShellis järgmised käsud:

```powershell
# Initialize domain variables

$domain = '<your-domain-name>'
$dmuser = '$domain\<your-username>'
$dmpswd = '<your-domain-password>'
$dmspwd = ConvertTo-SecureString $dmpswd -AsPlainText -Force
$dmcred = New-Object System.Management.Automation.PSCredential ($dmuser, $dmspwd)

# Add AD Domain Extension to the cloud service roles

Set-AzureServiceADDomainExtension -Service <your-cloud-service-hosted-service-name> -Role <your-role-name> -Slot <staging-or-production> -DomainName $domain -Credential $dmcred -JoinOption 35
```

Ja see on õige.

Cloud services peaks olema nüüd liidetud kohandatud domeenikontrolleri. Kui soovite lisateavet erinevate suvandite kohta, kuidas konfigureerida AD domeeni laiend, kasutage PowerShelli abi, nagu allpool näidatud.

```powershell
help Set-AzureServiceADDomainExtension
help New-AzureServiceADDomainExtensionConfig
```
