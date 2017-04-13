<properties
 pageTitle="Arvuta mahukat VMs Linux kohta | Microsoft Azure'i"
 description="Taustateabe ja teave kasutamise H-sari ja A8, A9, A10 ja A11 Arvuta mahukat suurused Linux vms hankimine"
 services="virtual-machines-linux"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,azure-service-management"/>
<tags
ms.service="virtual-machines-linux"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-linux"
 ms.workload="infrastructure-services"
 ms.date="09/21/2016"
 ms.author="danlep"/>

# <a name="about-h-series-and-compute-intensive-a-series-vms"></a>H-sari ja Arvuta mahukat A-sarja VMs kohta 

Siit leiate taustteavet ja mõned teave kasutamise uuem Azure H-sari ja varasemate A8, A9, A10 ja A11 suurused, nimetatakse ka *Arvuta mahukat* eksemplarid. See artikkel keskendub abil neid suurused Linux vms. Selles artiklis on saadaval [Windows](virtual-machines-windows-a8-a9-a10-a11-specs.md)vms ka.




[AZURE.INCLUDE [virtual-machines-common-a8-a9-a10-a11-specs](../../includes/virtual-machines-common-a8-a9-a10-a11-specs.md)]

## <a name="access-to-the-rdma-network"></a>RDMA võrgule juurdepääsu

Saate luua kogumite RDMA-ühenduse võimalusega Linux vms, et käivitada üks järgmistest toetatud Linux HPC jagamisel ja toetatud MPI rakendamist Azure'i RDMA võrgu ära. Lugege [Linux RDMA kobar käivitamiseks MPI rakenduste häälestamine](virtual-machines-linux-classic-rdma-cluster.md) Juurutussuvandid ja valimi konfigureerimistoimingute.

* **Jaotuse** - peate juurutama VMs Azure'i turuplatsi RDMA-ühenduse võimalusega SUSE Linux Enterprise Server (SLES) või OpenLogic CentOS vastavalt HPC piltidest. Ainult järgmistel piltidel turuplatsi toeta vajalikud Linuxi RDMA draiverid.

    * SLES 12 SP1 HPC SLES 12 SP1 HPC (Premium)
    
    * SLES 12 jaoks HPC SLES 12 jaoks HPC (Premium)
    
    * CentOS-põhiste 7.1 HPC
    
    * CentOS-põhiste 6,5 HPC
    
    >[AZURE.NOTE]H-sarja vms, soovitame kas SLES 12 SP1 HPC pildi või CentOS põhinev 7.1 HPC pilt.
    >
    >HPC CentOS-põhiste piltide tuum värskendused on keelatud **yum** konfiguratsioonifailis. Selle põhjuseks Linux RDMA draiverid on jaotatud pööret MINUTIS dokumendina ja draiveri värskendusi ei pruugi töötada, kui tuum on värskendatud.

* **MPI** - Inteli MPI teegi 5.x

    Sõltuvalt turuplatsi pilt valimist eraldi litsentse, installimine, või Inteli MPI konfigureerimine võib-olla vaja järgmiselt: 
    
    * **SLES 12 SP1 HPC pildi** - installi Inteli MPI pakettide jaotatud VM, käivitage järgmine käsk:
    
            sudo rpm -v -i --nodeps /opt/intelMPI/intel_mpi_packages/*.rpm

    * **HPC pildi SLES 12** - peate alla laadima ja installima Inteli MPI eraldi registreerima. Lugege teemat [Inteli MPI teegi installi juhend](https://software.intel.com/sites/default/files/managed/7c/2c/intelmpi-2017-installguide-linux.pdf).
    
    * **HPC centOS-põhiste piltide** - Inteli MPI 5.1 on juba installitud.  

    Rühmitatud VMs MPI tööde käitamist on vaja täiendavad süsteeminõuded. Näiteks klaster vms, peate luua usaldus Arvuta sõlmed. Tüüpilised sätted, leiate teemast [Linux RDMA kobar käivitamiseks MPI rakenduste häälestamine](virtual-machines-linux-classic-rdma-cluster.md).


## <a name="considerations-for-hpc-pack-and-linux"></a>Kaalutlused HPC Pack ja Linux

[HPC Pack](https://technet.microsoft.com/library/jj899572.aspx), Microsofti tasuta HPC kobar ja töö halduse lahenduse, pakub üheks võimaluseks kasutamiseks Arvuta mahukat eksemplarid Linux. Uusimate HPC Pack 2012 R2 tugi mitme Linuxi käitamist arvutada sõlmed juurutatud Azure VMs, haldab Windows Server pea sõlme. Arvutage RDMA-ühenduse võimalusega Linux töötab Inteli MPI, HPC Pack plaanimine ja käivitage Linux MPI rakendusi, et juurdepääs RDMA võrgu sõlmed. Alustamiseks, lugege teemat [Alustamine Linux Arvuta sõlmed on Azure HPC Pack klaster](virtual-machines-linux-classic-hpcpack-cluster.md).

## <a name="network-topology-considerations"></a>Võrgu topoloogia kaalutlused

* Klõpsake RDMA lubatud Linux VMs Azure Eth1 on mõeldud RDMA võrguliiklust. Mis tahes Eth1 sätted või sellest võrgust viidates konfiguratsioonifailis teavet ei muutu. Eth0 on mõeldud tavalise Azure võrguliiklust.

* Azure'i IP üle InfiniBand (IB) ei toetata. Toetatakse ainult RDMA IB üle.

## <a name="rdma-driver-updates-for-sles-12"></a>RDMA draiveri värskendused SLES 12

Pärast põhjal SLES 12 HPC pildi VM loomist peate VMs RDMA võrguühendus RDMA draiverid värskendada. 

>[AZURE.IMPORTANT]Selle toimingu jaoks on **vajalik** SLES 12 HPC VM juurutuste Azure kõikides piirkondades. 
>Seda toimingut ei ole vaja SLES 12 SP1 juurutamisel HPC, CentOS vastavalt 7.1 HPC või CentOS vastavalt 6,5 HPC VM. 

Enne värskendamist draiverid, Peatage kõik **zypper** protsessid või mis tahes protsessid, mis lukustada SUSE repo andmebaaside VM. Muul juhul draiverid võib värskendada õigesti.  

Linux RDMA draiverid VM iga värskendamiseks käivitage teie klientarvutist Azure'i CLI käskude ühte järgmistest.

**SLES 12 HPC VM ette valmistatud klassikaline juurutamise mudeli**

```
azure config mode asm

azure vm extension set <vm-name> RDMAUpdateForLinux Microsoft.OSTCExtensions 0.1
```

**SLES 12 HPC VM ette valmistatud mudelis ressursihaldur juurutamine**

```
azure config mode arm

azure vm extension set <resource-group> <vm-name> RDMAUpdateForLinux Microsoft.OSTCExtensions 0.1
```

>[AZURE.NOTE]See võib võtta aega draiverite ja käsk tagastab ilma väljundi. Pärast värskendamist, teie VM uuesti ja tuleb minuti kasutamiseks valmis.

### <a name="sample-script-for-driver-updates"></a>Draiveri värskendusi skripti näide

Kui teil on kobar SLES 12 HPC vms, saate rakenduses klaster üle kõik sõlmed skript draiveri värskendus. Näiteks järgmine skript värskendused on 8 sõlme kobar draiverid.

```

#!/bin/bash -x

# Define a prefix naming scheme for compute nodes, e.g., cluster11, cluster12, etc.

vmname=cluster

# Plug in appropriate numbers in the for loop below.

for (( i=11; i<19; i++ )); do

# For VMs created in the classic deployment model, use the following command in your script.

azure vm extension set $vmname$i RDMAUpdateForLinux Microsoft.OSTCExtensions 0.1

# For VMs created in the Resource Manager deployment model, use the following command in your script.

# azure vm extension set <resource-group> $vmname$i RDMAUpdateForLinux Microsoft.OSTCExtensions 0.1

done

```


## <a name="next-steps"></a>Järgmised sammud

* Kättesaadavus ja hinnakirjad Arvuta mahukat suuruses kohta leiate üksikasjalikumat teavet teemast [Virtuaalmasinates hinnad](https://azure.microsoft.com/pricing/details/virtual-machines/#Linux).

* Salvestusruumi võimsuse ja ketta üksikasjad leiate teemast [virtuaalmasinates suurused](virtual-machines-linux-sizes.md).

* Alustamiseks juurutamise ja kasutamise Arvuta mahukat suurused RDMA Linux, lugege teemat [Linux RDMA kobar käivitamiseks MPI rakenduste häälestamine](virtual-machines-linux-classic-rdma-cluster.md).


