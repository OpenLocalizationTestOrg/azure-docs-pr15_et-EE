<properties 
   pageTitle="Kuidas teisaldada VM või rolli eksemplari erinevate alamvõrgu"
   description="Saate teada, kuidas erinevate alamvõrgu VMs ja rolli aknad teisaldamine"
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

# <a name="how-to-move-a-vm-or-role-instance-to-a-different-subnet"></a>Kuidas teisaldada VM või rolli eksemplari erinevate alamvõrgu

PowerShelli abil saate teisaldada oma VMs ühe alamvõrgu teise virtuaalse samasse võrku (VNet). Rolli aknad saab teisaldada selle CSCFG redigeerimine, mitte PowerShelli kaudu.

>[AZURE.NOTE] See artikkel sisaldab teavet, mis on Azure klassikaline juurutuste ainult suhtes.

Miks teisaldada teise alamvõrgu VMs? Alamvõrgu migreerimise on kasulik, kui vanemate alamvõrgu on liiga väike ja ei saa laiendada selle alamvõrgu olemasoleva töötava VMs tõttu. Sel juhul saate luua uue, suurema alamvõrgu ja uus alamvõrgu VMs migreerimine ja seejärel pärast migreerimise lõpuleviimist, saate kustutada vana tühi alamvõrgu.

## <a name="how-to-move-a-vm-to-another-subnet"></a>Kuidas teisaldada teise alamvõrgu VM

VM üleviimiseks käivitage cmdlet Set-AzureSubnet PowerShelli, kasutades järgmises näites mallina. Järgmises näites oleme liiguvad TestVM kaudu oma Esita alamvõrgu alamvõrgu-2. Kindlasti redigeerimine näide vastavalt teie keskkonnas. Pange tähele, et iga kord, kui käivitate cmdleti Update-AzureVM protseduuri käigus, siis uuesti oma VM värskenduse käigus.

    Get-AzureVM –ServiceName TestVMCloud –Name TestVM `
  	| Set-AzureSubnet –SubnetNames Subnet-2 `
  	| Update-AzureVM

Kui teie määratud sisemise privaatne staatiline oma VM, siis on teil tühjendage see säte, enne kui saate liikuda uus alamvõrgu VM. Sel juhul kasutage järgmist:

    Get-AzureVM -ServiceName TestVMCloud -Name TestVM `
  	| Remove-AzureStaticVNetIP `
  	| Update-AzureVM
    Get-AzureVM -ServiceName TestVMCloud -Name TestVM `
  	| Set-AzureSubnet -SubnetNames Subnet-2 `
  	| Update-AzureVM

## <a name="to-move-a-role-instance-to-another-subnet"></a>Roll eksemplari teisaldamiseks teise alamvõrgu

Roll eksemplari teisaldamiseks CSCFG faili redigeerida. Järgmises näites oleme liiguvad "Role0" virtuaalse võrku *VNETName* kaudu oma Esita alamvõrgu *alamvõrgu-2*. Kuna rolli eksemplar oli juba juurutanud, kuvatakse ainult alamvõrgu nime muutmine = alamvõrgu – 2. Kindlasti redigeerimine näide vastavalt teie keskkonnas.

    <NetworkConfiguration>
        <VirtualNetworkSite name="VNETName" />
        <AddressAssignments>
           <InstanceAddress roleName="Role0">
                <Subnets><Subnet name="Subnet-2" /></Subnets>
           </InstanceAddress>
        </AddressAssignments>
    </NetworkConfiguration> 
