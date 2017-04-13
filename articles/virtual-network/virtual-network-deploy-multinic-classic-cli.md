<properties
   pageTitle="Mitme NIC VMs abil Azure'i CLI klassikaline juurutamise mudeli juurutamine | Microsoft Azure'i"
   description="Saate teada, kuidas juurutada mitme NIC VMs Azure'i CLI klassikaline juurutamise mudeli kasutamine"
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

#<a name="deploy-multi-nic-vms-classic-using-the-azure-cli"></a>Mitme NIC VMs (klassikaline) abil Azure'i CLI juurutamine

[AZURE.INCLUDE [virtual-network-deploy-multinic-classic-selectors-include.md](../../includes/virtual-network-deploy-multinic-classic-selectors-include.md)]

Saate luua Azure'i virtuaalmasinates (VMs) ja iga teie VMs mitme võrgu liidesed (NICs) manustamiseks. Mitu NICs lubada tüüpi liikluse eraldamine NICs üle. Näiteks üks NIC võib suhelda Internet, samas teise suhtleb ainult sisemisi ressursse, mis on Interneti-ühenduseta. Võimalus eraldi võrguliiklust üle mitme NICs on vaja mitme võrgu virtuaalse seadmed, nt rakenduse kohaletoimetamise ja WAN optimeeritud lahenduse.

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]Siit saate teada, kuidas [teha neid juhiseid kasutades ressursihaldur mudel](virtual-network-deploy-multinic-arm-cli.md).

[AZURE.INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

Praegu ei saa ühe NIC VMs ja mitu NICs VMs sama pilveteenuses. Seega peate tagasi lõpuks serverites rakendada eri pilveteenuses kui ja kõik muud komponendid seda stsenaariumi. Alltoodud juhiseid kasutada mõnda pilveteenusesse nimeks *IaaSStory* peamised ressursid ja *IaaSStory-Taustväärtus* tagasi lõpuks serverid.

## <a name="prerequisites"></a>Eeltingimused

Enne juurutamist saate tagasi lõpuks serverites, peate selle stsenaariumi korral kõik vajalikud ressursid peamised pilveteenuses juurutamine. Vähemalt, peate looma virtuaalse võrgu koos alamvõrku, jaoks soovitud taustväärtus. Külastage [loomine virtuaalse võrgu Azure'i CLI abil](virtual-networks-create-vnet-classic-cli.md) saate teada, kuidas juurutada virtuaalse võrgus.

[AZURE.INCLUDE [azure-cli-prerequisites-include.md](../../includes/azure-cli-prerequisites-include.md)]

## <a name="deploy-the-back-end-vms"></a>Tagasi lõpuks VMs juurutamine

Kirjutamata VMs sõltuvad loomine ressursse, mis on loetletud allpool.

- **Salvestusruumi konto andmeid ketast**. Parema jõudluse tagamiseks kasutatakse andmete ketast andmebaasi serverite ühtlase olekus ketas tehnoloogia, mis nõuab premium salvestusruumi konto. Veenduge, et juurutamiseks premium mälu tugi Azure asukoht.
- **NICs**. Iga VM on kaks NICs, üks Accessi andmebaasi ja üks haldus.
- **Kättesaadavus**. Kõik andmebaasi serverid lisatakse ühe kättesaadavus, määrata tagamiseks on vähemalt üks VMs üles ja hoolduse ajal töötab.

### <a name="step-1---start-your-script"></a>Samm 1 - skripti käivitamine

Saate alla laadida täielik bash skripti kasutada [siin](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/classic/virtual-network-deploy-multinic-classic-cli.sh). Järgige alltoodud muuta skripti töötada teie keskkonnas.

1. Muutke vastavalt oma olemasoleva ressursirühm juurutatud kohal [eeltingimused](#Prerequisites)allpool muutujate väärtusi.

        location="useast2"
        vnetName="WTestVNet"
        backendSubnetName="BackEnd"

2. Saate muuta väärtuste põhjal väärtusi, mida soovite kasutada oma kirjutamata juurutamiseks alloleval muutujate.

        backendCSName="IaaSStory-Backend"
        prmStorageAccountName="iaasstoryprmstorage"
        image="0b11de9248dd4d87b18621318e037d37__RightImage-Ubuntu-14.04-x64-v14.2.1"
        avSetName="ASDB"
        vmSize="Standard_DS3"
        diskSize=127
        vmNamePrefix="DB"
        osDiskName="osdiskdb"
        dataDiskPrefix="db"
        dataDiskName="datadisk"
        ipAddressPrefix="192.168.2."
        username='adminuser'
        password='adminP@ssw0rd'
        numberOfVMs=2

### <a name="step-2---create-necessary-resources-for-your-vms"></a>Samm 2 – Loo oma vms vajalikud ressursid

1. Saate luua uue pilveteenuses kõik taustväärtus vms. Kasutamisest on `$backendCSName` muutuv ressursi rühma nimi ja `$location` Azure regiooni jaoks.

        azure service create --serviceName $backendCSName \
            --location $location

2. OS- ja ketast kasutavad teie enda VMs lisatasu salvestusruumi konto luua.

        azure storage account create $prmStorageAccountName \
            --location $location \
            --type PLRS

### <a name="step-3---create-vms-with-multiple-nics"></a>Samm 3 – mitu NICs VMs loomine

1. Käivitage esitatavaks luua mitme VMs, võttes aluseks olevat `numberOfVMs` muutujat.

        for ((suffixNumber=1;suffixNumber<=numberOfVMs;suffixNumber++));
        do

2. Määratlege iga VM nime ja iga kahe NICs IP-aadress.

            nic1Name=$vmNamePrefix$suffixNumber-DA
            x=$((suffixNumber+3))
            ipAddress1=$ipAddressPrefix$x

            nic2Name=$vmNamePrefix$suffixNumber-RA
            x=$((suffixNumber+53))
            ipAddress2=$ipAddressPrefix$x

4. Saate luua VM. Pange tähele selle kasutamist on `--nic-config` parameeter, mis sisaldab loendit kõigi NICs nime, alamvõrgu ja IP-aadress.

            azure vm create $backendCSName $image $username $password \
                --connect $backendCSName \
                --vm-name $vmNamePrefix$suffixNumber \
                --vm-size $vmSize \
                --availability-set $avSetName \
                --blob-url $prmStorageAccountName.blob.core.windows.net/vhds/$osDiskName$suffixNumber.vhd \
                --virtual-network-name $vnetName \
                --subnet-names $backendSubnetName \
                --nic-config $nic1Name:$backendSubnetName:$ipAddress1::,$nic2Name:$backendSubnetName:$ipAddress2::

5. Looge iga VM kaks andmete plaati.

            azure vm disk attach-new $vmNamePrefix$suffixNumber \
                $diskSize \
                vhds/$dataDiskPrefix$suffixNumber$dataDiskName-1.vhd

            azure vm disk attach-new $vmNamePrefix$suffixNumber \
                $diskSize \
                vhds/$dataDiskPrefix$suffixNumber$dataDiskName-2.vhd
        done

### <a name="step-4---run-the-script"></a>Samm 4 - skripti

Nüüd, kui saate alla laadida ja muutunud script vajaduste, käivitage skript luua tagasi lõpuks andmebaasi VMs mitu NICs.

1. Salvestage oma skripti ja käivitage see kaudu oma **Bash** terminalis. Kuvatakse algse väljund, nagu allpool näidatud.

        info:    Executing command service create
        info:    Creating cloud service
        data:    Cloud service name IaaSStory-Backend
        info:    service create command OK
        info:    Executing command storage account create
        info:    Creating storage account
        info:    storage account create command OK
        info:    Executing command vm create
        info:    Looking up image 0b11de9248dd4d87b18621318e037d37__RightImage-Ubuntu-14.04-x64-v14.2.1
        info:    Looking up virtual network
        info:    Looking up cloud service
        info:    Getting cloud service properties
        info:    Looking up deployment
        info:    Creating VM

2. Mõne minuti pärast täitmise lõpetatakse ja te näete väljund ülejäänud, nagu allpool näidatud.

        info:    OK
        info:    vm create command OK
        info:    Executing command vm disk attach-new
        info:    Getting virtual machines
        info:    Adding Data-Disk
        info:    vm disk attach-new command OK
        info:    Executing command vm disk attach-new
        info:    Getting virtual machines
        info:    Adding Data-Disk
        info:    vm disk attach-new command OK
        info:    Executing command vm create
        info:    Looking up image 0b11de9248dd4d87b18621318e037d37__RightImage-Ubuntu-14.04-x64-v14.2.1
        info:    Looking up virtual network
        info:    Looking up cloud service
        info:    Getting cloud service properties
        info:    Looking up deployment
        info:    Creating VM
        info:    OK
        info:    vm create command OK
        info:    Executing command vm disk attach-new
        info:    Getting virtual machines
        info:    Adding Data-Disk
        info:    vm disk attach-new command OK
        info:    Executing command vm disk attach-new
        info:    Getting virtual machines
        info:    Adding Data-Disk
        info:    vm disk attach-new command OK
