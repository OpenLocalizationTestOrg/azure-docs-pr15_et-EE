<properties
   pageTitle="Mitu NICs VM loomine"
   description="Saate teada, kuidas loomine ja konfigureerimine vms mitu nics"
   services="virtual-network, virtual-machines"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-service-management,azure-resource-manager"
/>
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/02/2016"
   ms.author="jdial" />

# <a name="create-a-vm-with-multiple-nics"></a>Mitu NICs VM loomine

Saate luua Azure'i virtuaalmasinates (VMs) ja iga teie VMs mitme võrgu liidesed (NICs) manustamiseks. Mitme NIC on mitme võrgu virtuaalse seadmed, nt rakenduse kohaletoimetamise ja WAN optimeeritud lahenduse nõue. Mitme NIC pakub mitme võrgu liikluse funktsioonid, sh eraldamise liikluse vahel ees NIC lõpetamine ja tagasi end NIC(s) või eraldamine lennuk andmeside halduse lennuk liiklus.

![Mitme NIC VM jaoks](./media/virtual-networks-multiple-nics/IC757773.png)

Ülaltoodud joonisel VM kolme NICs, kus iga ühendatud muu alamvõrgu.

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]

- Interneti-ühendusega VIP (klassikaline juurutuste) on toetatud ainult NIC. "vaikimisi" On ainult üks VIP vaikimisi NIC. IP
- Sel ajal, ei toetata eksemplari tase avaliku IP (LPIP) aadressid (klassikaline juurutuste) jaoks mitme NIC VMs.
- Järjestuse NICs kaudu VM sees saab juhusliku ja võib muutuda ka kogu Azure'i infrastruktuuri värskendused. Siiski IP-aadressid ja vastavate ethernet MAC aadressid saavad samaks. Oletame näiteks, **Eth1** on IP-aadress 10.1.0.100 ja MAC-aadress 00-0D-3A-B0-39-0D; Azure'i infrastruktuuri värskendus- ja taaskäivitage arvuti pärast seda võib muuta **Eth2**, kuid IP ja Maci sidumine jäävad samaks. Kui taaskäivitamine on kliendi algatatud, NIC tellimuse jäävad samaks.
- Iga NIC iga VM aadressi peab asuma on alamvõrku, mitu NICs ühe VM saab iga määrata aadressid, mis on sama alamvõrgu.
- VM suurus määrab NICS, mida saate luua VM arv. Viide [Windows Server](../virtual-machines/virtual-machines-windows-sizes.md) ja [Linux](../virtual-machines/virtual-machines-linux-sizes.md) VM suurused artiklites määrata mitu NICS toetab iga VM suurus. 

## <a name="network-security-groups-nsgs"></a>Võrgu turberühmad (NSGs)
Ressursihaldur juurutamine, mis tahes NIC kohta VM võib olla seotud koos mõne võrgu turvalisus jaotises NSG, sh mis tahes NICs VM, mis on mitu NICs lubatud. Kui soovitud NIC on määratud sees on alamvõrku, kus on seostatud mõne NSG alamvõrgu aadressi, siis selle alamvõrgu NSG reeglid ka rakendada selle NIC. Lisaks seostada NSGs alamvõrku, saate mõne NIC seostada ka mõne NSG.

Kui soovitud alamvõrgu on seostatud mõne NSG, asukoht ja mõne selle alamvõrgu sees on eraldi seostatud mõne NSG, seotud NSG reeglite rakendamise **kulgemist järjestuses** vastavalt jõua või sealt välja NIC liiklus suunda:

- **Sissetulev liiklus** , mille sihtkoht on kõnealuse NIC juhtimine esmalt alamvõrku, kuni soovitud alamvõrgu NSG reegleid, läbides NIC sisse ja seejärel soovitud NIC NSG reeglite käivitamise enne käivitamise.
- **Väljamineva liikluse** , mille andmeallikaks on kõnealuse NIC liigub sujuvalt esmalt välja NIC, käivitamise soovitud NIC NSG reeglid enne läbib alamvõrgu ja seejärel soovitud alamvõrgu NSG reeglite käivitamise.

Lisateavet [Võrgu turberühmad](virtual-networks-nsg.md) ja kuidas need on rakendatud põhjal alamvõrku, VMs ja NICs ühendused.

## <a name="how-to-configure-a-multi-nic-vm-in-a-classic-deployment"></a>Kuidas seadistada mitme NIC VM klassikaline juurutamine

Järgmised juhised aitavad teil luua mitme NIC VM, mis sisaldab 3 NICs: vaikimisi NIC ja kahe täiendavad NICs. Funktsiooni konfigureerimistoimingute loob VM, mis konfigureeritakse vastavalt teenuse konfiguratsiooni faili fragment allpool:

    <VirtualNetworkSite name="MultiNIC-VNet" Location="North Europe">
    <AddressSpace>
      <AddressPrefix>10.1.0.0/16</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="Frontend">
            <AddressPrefix>10.1.0.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Midtier">
            <AddressPrefix>10.1.1.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Backend">
            <AddressPrefix>10.1.2.0/23</AddressPrefix>
          </Subnet>
          <Subnet name="GatewaySubnet">
            <AddressPrefix>10.1.200.0/28</AddressPrefix>
          </Subnet>
        </Subnets>
    … Skip over the remainder section …
    </VirtualNetworkSite>


Peate enne käivitage PowerShelli käske näites järgmine kohustuslik tarkvara.

- Azure'i tellimuse.
- Virtuaalne võrgus konfigureeritud. Vt lisateavet VNets [Virtuaalse võrgu ülevaade](virtual-networks-overview.md) .
- Azure'i PowerShelli uusim versioon laaditakse alla ja installitakse. Vaadake, [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md).

Mitu NICs VM loomiseks tehke järgmist:

1. Valige Azure VM Pildigalerii VM pilt. Pange tähele, et piltide muutmine sageli regiooniti on saadaval. Järgmises näites määratud pilt võib muuta või võib pole teie piirkonnas olla, et olla kindel määramiseks peate pildi.

        $image = Get-AzureVMImage `
            -ImageName "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-201410.01-en.us-127GB.vhd"

1. Looge VM konfigureerimine.

        $vm = New-AzureVMConfig -Name "MultiNicVM" -InstanceSize "ExtraLarge" `
            -Image $image.ImageName –AvailabilitySetName "MyAVSet"

1. Saate luua vaikimisi administraatori Logi sisse.

        Add-AzureProvisioningConfig –VM $vm -Windows -AdminUserName "<YourAdminUID>" `
            -Password "<YourAdminPassword>"

1. Täiendavad NICs lisada VM konfigureerimine.

        Add-AzureNetworkInterfaceConfig -Name "Ethernet1" `
            -SubnetName "Midtier" -StaticVNetIPAddress "10.1.1.111" -VM $vm
        Add-AzureNetworkInterfaceConfig -Name "Ethernet2" `
            -SubnetName "Backend" -StaticVNetIPAddress "10.1.2.222" -VM $vm

1. Määrake vaikimisi NIC. alamvõrgu ja IP-aadress

        Set-AzureSubnet -SubnetNames "Frontend" -VM $vm
        Set-AzureStaticVNetIP -IPAddress "10.1.0.100" -VM $vm

1. Saate luua virtuaalse võrgu VM.

        New-AzureVM -ServiceName "MultiNIC-CS" –VNetName "MultiNIC-VNet" –VMs $vm

>[AZURE.NOTE] Siin määratud VNet peab juba olemas (nagu mainitud eeltingimused). Järgmises näites määrab nimega **MultiNIC-VNet**virtuaalse võrgus.

## <a name="limitations"></a>Piirangud

Järgmised piirangud kehtivad mitme NIC funktsiooni kasutamisel:

- Mitme NIC VMs tuleb luua Azure virtuaalne võrkude (VNets). Mitte-VNet VMs ei saa konfigureerida mitme NICs.
- Kõik VMs on kättesaadavus komplekti peate kasutama kas mitme NIC või ühe NIC. Ei saa olla mitme NIC VMs ja ühe NIC VMs on kättesaadavus määramine. Samu reegleid rakendada vms mõnda pilveteenusesse.
- Koos ühe NIC VM ei saa konfigureerida mitme NICs (ja vastupidi) pärast juurutatud ilma kustutamine ja uuesti luua selle.


## <a name="secondary-nics-access-to-other-subnets"></a>Teisene NICs juurdepääsu muude alamvõrku

Vaikimisi konfigureeritud teisene NICs pole vaikimisi lüüsiga, mille tõttu piiratud liiklust teisene NICs sisse sama alamvõrgu jooksul. Kui kasutajad soovite lubada teisene NICs väljaspool oma alamvõrgu rääkida, need tuleb konfigureerida lüüsi, nagu allpool kirjeldatud marsruutimise tabeli kirje lisamine.

>[AZURE.NOTE] Enne juuli 2015 võib olla konfigureeritud kõik NICs lüüs loodud VMs. Vaikimisi lüüsi teisene NICs jaoks ei saa eemaldada, kuni need VMs on rebooted. Operatsioonisüsteemide kasutavate nõrk hosti marsruutimise mudeli, nt Linux, saate Interneti-ühendus katkestada kui sissepääsu ja sealt liikluse kasutamine eri NICs.

### <a name="configure-windows-vms"></a>Windowsi VMs konfigureerimine

Oletame, et teil on Windows VM koos kahe NICs järgmiselt:

- Esmane NIC IP-aadress: 192.168.1.4
- Teisene NIC IP-aadress: 192.168.2.5

Tabeli IPv4 marsruutimiseks selle VM peaks välja nägema selline:

    IPv4 Route Table
    ===========================================================================
    Active Routes:
    Network Destination        Netmask          Gateway       Interface  Metric
              0.0.0.0          0.0.0.0      192.168.1.1      192.168.1.4      5
            127.0.0.0        255.0.0.0         On-link         127.0.0.1    306
            127.0.0.1  255.255.255.255         On-link         127.0.0.1    306
      127.255.255.255  255.255.255.255         On-link         127.0.0.1    306
        168.63.129.16  255.255.255.255      192.168.1.1      192.168.1.4      6
          192.168.1.0    255.255.255.0         On-link       192.168.1.4    261
          192.168.1.4  255.255.255.255         On-link       192.168.1.4    261
        192.168.1.255  255.255.255.255         On-link       192.168.1.4    261
          192.168.2.0    255.255.255.0         On-link       192.168.2.5    261
          192.168.2.5  255.255.255.255         On-link       192.168.2.5    261
        192.168.2.255  255.255.255.255         On-link       192.168.2.5    261
            224.0.0.0        240.0.0.0         On-link         127.0.0.1    306
            224.0.0.0        240.0.0.0         On-link       192.168.1.4    261
            224.0.0.0        240.0.0.0         On-link       192.168.2.5    261
      255.255.255.255  255.255.255.255         On-link         127.0.0.1    306
      255.255.255.255  255.255.255.255         On-link       192.168.1.4    261
      255.255.255.255  255.255.255.255         On-link       192.168.2.5    261
    ===========================================================================

Pange tähele, et vaikimisi marsruudi (0.0.0.0) on saadaval esmane NIC. ainult Te ei saa juurdepääsu ressursid väljaspool alamvõrgu teisene NIC, nagu allpool näha:

    C:\Users\Administrator>ping 192.168.1.7 -S 192.165.2.5

    Pinging 192.168.1.7 from 192.165.2.5 with 32 bytes of data:
    PING: transmit failed. General failure.
    PING: transmit failed. General failure.
    PING: transmit failed. General failure.
    PING: transmit failed. General failure.

Teisene NIC vaikimisi Marsruudi lisamiseks tehke järgmist.

1. Käsureale, käivitage ära tunda indeksi number teisene NIC alltoodud käsk:

        C:\Users\Administrator>route print
        ===========================================================================
        Interface List
         29...00 15 17 d9 b1 6d ......Microsoft Virtual Machine Bus Network Adapter #16
         27...00 15 17 d9 b1 41 ......Microsoft Virtual Machine Bus Network Adapter #14
          1...........................Software Loopback Interface 1
         14...00 00 00 00 00 00 00 e0 Teredo Tunneling Pseudo-Interface
         20...00 00 00 00 00 00 00 e0 Microsoft ISATAP Adapter #2
        ===========================================================================

2. Pange tähele teisel väljal tabeli abil registri 27 (järgmises näites).
3. Käsuviip, käivitage käsk **marsruutimiseks lisada** , nagu allpool näidatud. Selles näites on milles 192.168.2.1 on vaikimisi lüüsi jaoks teisese NIC:

        route ADD -p 0.0.0.0 MASK 0.0.0.0 192.168.2.1 METRIC 5000 IF 27

4. Ühenduvuse testimiseks, minge tagasi Käsuviip ja proovite ping erinevate alamvõrgu kaudu teisene NIC näha int eh järgmises näites:

        C:\Users\Administrator>ping 192.168.1.7 -S 192.165.2.5

        Reply from 192.168.1.7: bytes=32 time<1ms TTL=128
        Reply from 192.168.1.7: bytes=32 time<1ms TTL=128
        Reply from 192.168.1.7: bytes=32 time=2ms TTL=128
        Reply from 192.168.1.7: bytes=32 time<1ms TTL=128

5. Samuti saate marsruutimiseks tabeli kontrollida äsja lisatud marsruutimiseks, nagu allpool näidatud:

        C:\Users\Administrator>route print

        ...

        IPv4 Route Table
        ===========================================================================
        Active Routes:
        Network Destination        Netmask          Gateway       Interface  Metric
                  0.0.0.0          0.0.0.0      192.168.1.1      192.168.1.4      5
                  0.0.0.0          0.0.0.0      192.168.2.1      192.168.2.5   5005
                127.0.0.0        255.0.0.0         On-link         127.0.0.1    306

### <a name="configure-linux-vms"></a>Linux VMs konfigureerimine

Linux vms, kuna vaikekäitumine kasutab nõrk hosti marsruutimist soovitame teisene NICs piirduvad üksnes sama alamvõrgu liikluse puhul. Aga kui teatud stsenaariumide nõuda väljaspool alamvõrgu, kasutajate tuleks lubada marsruutimine tagamaks, et sissepääsu ja sealt liikluse kasutab sama NIC. poliitika

## <a name="next-steps"></a>Järgmised sammud

- Juurutada [MultiNIC VMs 2 rakendus stsenaariumi ressursihaldur juurutamine](virtual-network-deploy-multinic-arm-template.md).
- Juurutada [MultiNIC VMs 2 rakendus stsenaariumi klassikaline juurutamine](virtual-network-deploy-multinic-classic-ps.md).
