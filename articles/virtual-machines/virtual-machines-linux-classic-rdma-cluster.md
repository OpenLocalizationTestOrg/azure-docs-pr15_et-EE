<properties
 pageTitle="Linux RDMA kobar MPI rakenduste käivitamiseks | Microsoft Azure'i"
 description="Linux kobar suurusega H16r, H16mr, A8 ja A9 VMs kasutada Azure RDMA võrgu MPI rakenduste loomine"
 services="virtual-machines-linux"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-service-management"/>
<tags
ms.service="virtual-machines-linux"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-linux"
 ms.workload="infrastructure-services"
 ms.date="09/21/2016"
 ms.author="danlep"/>

# <a name="set-up-a-linux-rdma-cluster-to-run-mpi-applications"></a>Seadistamiseks Linux RDMA kobar MPI rakendused


Saate teada, kuidas häälestada Linux RDMA kobar Azure koos [H-sarjade või Arvuta mahukat A-sarja VMs](virtual-machines-linux-a8-a9-a10-a11-specs.md) paralleelselt sõnumi läbides kasutajaliidese (MPI) rakendusi käivitada. Selles artiklis antakse juhiseid ettevalmistamiseks Linux HPC pilt klaster Inteli MPI käivitamiseks. Seejärel juurutate kobar vms sellel pildil ja ühe RDMA-ühenduse võimalusega Azure VM suuruses (praegu H16r, H16mr, A8 või A9) abil. Klaster abil saate käivitada MPI rakendusi, mis suhelda tõhus madal latentsus, üle võrgu läbilaskevõime kõrge põhjal serveri otsest mälu juurdepääs (RDMA) tehnoloogia.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

## <a name="cluster-deployment-options"></a>Kobar Juurutussuvandid

Järgnevalt on meetodite abil saate luua Linux RDMA kobar koos või ilma Töö scheduler.


* **Azure'i CLI skriptide** - nagu allpool näha selle artikli abil [Azure'i käsurea liides](../xplat-cli-install.md) (CLI) skript kobar RDMA-ühenduse võimalusega vms juurutamise. CLI Teenusehaldus režiimis loob kobar sõlmed seeriatoodanguna mudelis klassikaline juurutus, nii et juurutamine palju Arvuta sõlmed võib kuluda mitu minutit. Klassikaline juurutamise mudeli kasutamine RDMA võrguühenduse lubamiseks juurutada VMs sama pilveteenuses.

* **Azure'i ressursihaldur Mallid** – saate kasutada ka ressursihaldur juurutamise mudeli juurutamiseks kobar RDMA-ühenduse võimalusega vms, mis ühendab RDMA võrku. Saate [oma malli loomise](../resource-group-authoring-templates.md)või märkige [Azure Kiirjuhend Mallid](https://azure.microsoft.com/documentation/templates/) poolt Microsofti või ühenduse lahenduse juurutamine malle soovite. Ressursihaldur malle saate võimalda kiire ja usaldusväärne Linux kobar juurutamine. Ressursihaldur juurutamise mudeli kasutamine RDMA võrguühenduse lubamiseks juurutada kättesaadavus samu VM.

* **HPC Pack** - loomine Microsoft HPC Pack kobar Azure ja lisage RDMA-ühenduse võimalusega Arvuta sõlmed, mis töötavad toetatud Linux jaotuse juurdepääsu RDMA võrgule. Vt [Alustamine Linux Arvuta sõlmed on Azure HPC Pack klaster](virtual-machines-linux-classic-hpcpack-cluster.md).

## <a name="sample-deployment-steps-in-classic-model"></a>Valimi juurutamise etappide klassikaline mudel

Järgmised toimingud näitab, kuidas kasutada Azure CLI juurutamine SUSE Linux Enterprise Server (SLES) 12 SP1 HPC VM Azure'i turuplatsilt, seda kohandada, ning luua kohandatud VM pilt. Seejärel kasutage pildi skript kobar RDMA-ühenduse võimalusega vms kasutuselevõtt. 

>[AZURE.TIP]Sarnaselt juhiste abil saate juurutada kobar RDMA-ühenduse võimalusega vms muud toetatud HPC pildid Azure'i turuplatsi põhjal. Mõned juhised võivad erineda veidi märkida. Näiteks Inteli MPI sisalduv ja konfigureeritud vaid mõne neid pilte. Ja kui juurutate SLES 12 HPC VM asemel SLES 12 SP1 HPC VM, peate värskendama RDMA draiverid. Lisateavet leiate teemast [kohta A8, A9, A10, ja A11 Arvuta mahukat eksemplarid](virtual-machines-linux-a8-a9-a10-a11-specs.md#rdma-driver-updates-for-sles-12).


### <a name="prerequisites"></a>Eeltingimused

*   **Klientarvuti** - peate Mac-arvutis, Linux või kliendi Windowsi-põhises arvutis Azure'i suhelda. Need juhised eeldavad, et kasutate mõnda muud klienti Linux.

*   **Azure'i tellimuse** – kui olete tellinud, saate luua [tasuta konto](https://azure.microsoft.com/free/) vaid paar minutit. Suuremate kogumite, on soovitatav pensionitingimustega tellimuse või muid ostmise võimalusi. 

*   **VM suurus-saadavus** - praegu on järgmised eksemplari suurused RDMA toega: H16r, H16mr, A8 ja A9. Saadavus Azure piirkondade [toodete saadaval regiooniti](https://azure.microsoft.com/regions/services/) otsida. 

*   **Tuuma kvoodi** - suurendada kvoote valdkond kobar Arvuta mahukat vms juurutamiseks peate. Näiteks peate vähemalt 128 tuuma, kui soovite juurutada 8 A9 VMs, nagu on näidatud selles artiklis. Teie tellimus võib ka piirata valdkond juurutate teatud VM suurus peredele, sh H-sarja. Taotleda kvoodi suurendada, [avage mõni Online'i kliendi tugiteenuse taotluse](../azure-supportability/how-to-create-azure-support-request.md) tasuta. 

*   **Azure'i CLI** - [installida](../xplat-cli-install.md) Azure CLI ja [Azure tellimuse ühenduse](../xplat-cli-connect.md) klientarvuti.


### <a name="step-1-provision-a-sles-12-sp1-hpc-vm"></a>Samm 1. SLES 12 SP1 HPC VM ettevalmistamine

Pärast sisselogimist Azure'i Azure CLI, käivitage `azure config list` kinnitada, et väljund kuvatakse Teenusehaldus režiim. Kui see pole, määrata selle käsu režiimi.


    azure config mode asm


Tippige loendi kõigi tellimused, mida teil on õigus seda kasutada järgmist:


    azure account list

Praeguse aktiivne tellimus on tähistatud `Current` määratud `true`. Kui see tellimus pole soovitud klaster loomiseks kasutada, seada aktiivne tellimus asjakohased tellimuse Id:

    azure account set <subscription-Id>

Azure, pilte avalikult kättesaadavaks SLES 12 SP1 HPC kuvamiseks käivitage järgmine käsk, eeldades shell keskkonna toetab **grep**:


    azure vm image list | grep "suse.*hpc"

Nüüd ettevalmistamise mõne RDMA-ühenduse võimalusega VM pildiga SLES 12 SP1 HPC järgmise käsu:

    azure vm create -g <username> -p <password> -c <cloud-service-name> -l <location> -z A9 -n <vm-name> -e 22 b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-sp1-hpc-v20160824

Kui

* Suuruse (selles näites A9) on üks RDMA-ühenduse võimalusega VM suurused.

* Välise SSH pordi number (selles näites on vaikimisi SSH 22) on mis tahes sobiv pordi number. Sisemise SSH pordi number on seatud 22.

* Azure'i piirkonna määratud asukohta on loodud uue pilveteenuses. Määrake asukoht, mille valite VM suurus on saadaval.

* SLES 12 SP1 pildi nime praegu võib olla `b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-sp1-hpc-v20160824` või `b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-sp1-hpc-priority-v20160824` SUSE prioriteet tugiteenuste (lisatasu).

### <a name="step-2-customize-the-vm"></a>Samm 2. VM kohandamine

Pärast VM on lõpule jõudnud, ettevalmistamise, SSH VM, on VM välise IP-aadress (või DNS-i nimi) ja välise pordi number, mille konfigureeritud ja seda kohandada. Ühenduse leiate teavet artiklist [Kuidas logida virtuaalse masina töötab Linux](virtual-machines-linux-mac-create-ssh-keys.md). Käskude tegutsemine kasutaja konfigureeritud VM, kui root access täitma etapi.

>[AZURE.IMPORTANT]Microsoft Azure'i ei paku root access Linux vms. Pääsete haldus, kui kasutaja on VM ühendatud, käivitage käskude abil `sudo`.

* **Värskendused** – abil **zypper**Installi värskendused. Samuti võite installida NFS Utiliidid. 

    >[AZURE.IMPORTANT]SLES 12 SP1 HPC VM, soovitame, et te ei rakenda tuum värskendusi, mis võivad põhjustada probleeme Linux RDMA draiverid.

* **Inteli MPI** - installimise Inteli MPI SLES 12 SP1 HPC VM käivitage järgmine käsk:

        sudo rpm -v -i --nodeps /opt/intelMPI/intel_mpi_packages/*.rpm

* **Lukusta mälu** - RDMA jaoks saadaval lukustada, lisada või muuta järgmisi sätteid /etc/security/limits.conf faili jaoks MPI koode. (Peate root access selle faili redigeerimiseks.) 

    ```
    <User or group name> hard    memlock <memory required for your application in KB>

    <User or group name> soft    memlock <memory required for your application in KB>
    ```

    >[AZURE.NOTE]Katsetamiseks, saate määrata memlock-piiramatu. Näide: `<User or group name>    hard    memlock unlimited`. Lisateabe saamiseks lugege teemat [Kõige paremini teadaolevad meetodid säte lukustatud mälumaht](https://software.intel.com/en-us/blogs/2014/12/16/best-known-methods-for-setting-locked-memory-size).

* **SSH klahvid SLES vms** - luua SSH klahvid luua oma kasutajakonto vahel Arvuta sõlmed usalda SLES kobar, kui MPI tööde. (Kui olete juurutanud CentOS vastavalt HPC VM, Ärge järgige selles etapis tuleb. Hiljem juhised artiklist häälestamiseks passwordless SSH usalda kobar sõlme vahel pärast pildi jäädvustamiseks ja juurutada klaster.) 

    Käivitage järgmine käsk luua SSH võtmed. Kui teilt küsitakse, kas sisend, vajutage sisestusklahvi Enter võtmed vaikeasukoha parooli määramata.

        ssh-keygen

    Avalik võti lisada authorized_keys faili teadaolevad avaliku võtmed.

        cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys

    ~/.Ssh kataloog, redigeerimine või loomine "config" fail. Sisestage soovitud IP-aadresside vahemiku privaatvõrgu, mida plaanite kasutada Azure (selles näites 10.32.0.0/16).

        host 10.32.0.*
        StrictHostKeyChecking no

    Teise võimalusena loendis iga VM klaster privaatvõrgu IP-aadress:

    ```
    host 10.32.0.1
     StrictHostKeyChecking no
    host 10.32.0.2
     StrictHostKeyChecking no
    host 10.32.0.3
     StrictHostKeyChecking no
    ```

    >[AZURE.NOTE]Konfigureerimise `StrictHostKeyChecking no` saate luua võimalikud ohtu turvalisusele, kui vahemiku või teatud IP-aadress on määratud.

* **Rakenduste** - installige kõik rakendused, kui teil on vaja see VM või teha muud kohandused enne selle pildi jäädvustamiseks.

### <a name="step-3-capture-the-image"></a>Samm 3. Jäädvustada pilt

Pildi jäädvustamiseks, käivitage järgmine käsk esmalt Linuxi. See käsk deprovisions VM, kuid säilitab Kasutajakontod ja SSH tutvustatakse, mida saate häälestada.

```
sudo waagent -deprovision
```

Teie klientarvutist käivitage järgmised Azure'i CLI käsud jäädvustada pilt. Vaadake, [kuidas jäädvustada pildina klassikaline Linux virtuaalse masina](virtual-machines-linux-classic-capture-image.md) üksikasju.  

```
azure vm shutdown <vm-name>

azure vm capture -t <vm-name> <image-name>

```

Kui need käsud VM pildi tegemist kasutamiseks ja VM kustutatakse. Nüüd olete valmis kasutama klaster kohandatud pilt.

### <a name="step-4-deploy-a-cluster-with-the-image"></a>Samm 4. Pildiga klaster juurutamine

Muuta järgmist Bash skripti vastav väärtustega keskkonna ja käivitage see teie klientarvutist. Kuna Azure'i kasutab VMs seeriatoodanguna mudelis klassikaline juurutus, kulub 8 A9 VMs soovitatud selle skripti juurutamiseks paar minutit.

```
#!/bin/bash -x
# Script to create a compute cluster without a scheduler in a VNet in Azure
# Create a custom private network in Azure
# Replace 10.32.0.0 with your virtual network address space
# Replace <network-name> with your network identifier
# Replace "West US" with an Azure region where the VM size is available
# See Azure Pricing pages for prices and availability of compute-intensive VMs

azure network vnet create -l "West US" -e 10.32.0.0 -i 16 <network-name>

# Create a cloud service. All the compute-intensive instances need to be in the same cloud service for Linux RDMA to work across InfiniBand.
# Note: The current maximum number of VMs in a cloud service is 50. If you need to provision more than 50 VMs in the same cloud service in your cluster, contact Azure Support.

azure service create <cloud-service-name> --location "West US" –s <subscription-ID>

# Define a prefix naming scheme for compute nodes, e.g., cluster11, cluster12, etc.

vmname=cluster

# Define a prefix for external port numbers. If you want to turn off external ports and use only internal ports to communicate between compute nodes via port 22, don’t use this option. Since port numbers up to 10000 are reserved, use numbers after 10000. Leave external port on for rank 0 and head node.

portnumber=101

# In this cluster there will be 8 size A9 nodes, named cluster11 to cluster18. Specify your captured image in <image-name>. Specify the username and password you used when creating the SSH keys.

for (( i=11; i<19; i++ )); do
        azure vm create -g <username> -p <password> -c <cloud-service-name> -z A9 -n $vmname$i -e $portnumber$i -w <network-name> -b Subnet-1 <image-name>
done

# Save this script with a name like makecluster.sh and run it in your shell environment to provision your cluster
```

## <a name="considerations-for-a-centos-hpc-cluster"></a>Kaalutlused CentOS HPC kobar

Kui soovite häälestada põhjal üks Azure'i turuplatsil SLES 12 asemel HPC CentOS-põhiste piltide jaoks HPC klaster, järgige eelmises jaotises toodud üldised juhised. Pöörake tähelepanu järgmised erinevused, kui ettevalmistamine ja konfigureerida VM.

1. Inteli MPI on juba installitud ette valmistatud CentOS vastavalt HPC pildilt VM. 

2. Lukusta mälu sätted on juba lisatud soovitud VM /etc/security/limits.conf failis.

2. Luua SSH klahvireas olete ette VM jäädvustada. Selle asemel, soovitame kasutaja autentimise häälestamise pärast selle cluser juurutamist. Leiate järgmisest jaotisest.  

### <a name="set-up-passwordless-ssh-trust-on-the-cluster"></a>Passwordless SSH usalda klaster häälestamine

CentOS-põhiste HPC klaster, on kaks võimalust loomise vahel Arvuta sõlmed usalda: host autentimise ja kasutaja autentimise. Host autentimise on väljaspool käesoleva artikli ja üldiselt peab tegema mõne laiend skripti kaudu juurutamisel. Kasutaja autentimise on mugav loomise usalda pärast juurutamise ja nõuab loomine ja ühiskasutus SSH klahvide vahel Arvuta sõlmed klaster. See meetod passwordless SSH login tuntakse ja on vajalik, kui MPI tööde. 

Proovi skripti, kui suure osa moodustab ühenduse on saadaval [GitHub](https://github.com/tanewill/utils/blob/master/user_authentication.sh) lihtne kasutaja autentimise CentOS vastavalt HPC klaster. Alla laadida ja selle skripti, täitke järgmised juhised. Saate muuta selle skripti või mõne muu meetodi abil saate luua passwordless SSH autentimise kobar Arvuta sõlmed vahel.

    wget https://raw.githubusercontent.com/tanewill/utils/master/ user_authentication.sh
    
Skripti käivitamiseks peate teadma eesliide alamvõrgu IP-aadressid. Ühele kobar sõlmed järgmise käsu saada eesliide. Oma väljundi peaks välja nägema umbes 10.1.3.5 ja eesliide on selle 10.1.3 osa.

    ifconfig eth0 | grep -w inet | awk '{print $2}'

Nüüd käivitage skript kolme parameetritega: Arvuta sõlmed levinud kasutajanimi, levinud Arvuta sõlmed ja eelmise käsu tagastatud alamvõrgu eesliide kasutaja parooli.


    ./user_authentication.sh <myusername> <mypassword> 10.1.3

See skript teeb järgmist.

* Loob kausta host sõlm nimega .ssh, mille jaoks on vaja passwordless Logi sisse. 
* Loob konfiguratsioonifail .ssh kataloogi, mis juhendab lubada login igalt klaster passwordless sisse logida. 
* Loob failide nimede sõlm ja sõlme klaster sõlme IP-aadressid. Need failid on jäänud pärast skripti edaspidiseks kasutamiseks. 
* Loob era- ja avalik võti paari iga kobar sõlme, sh host sõlm ja loob kirjed authorized_keys faili.

>[AZURE.WARNING]Skriptide käivitamise saate luua võimalikud ohtu turvalisusele. Veenduge, et selle avaliku olulise teabe ~/.ssh levitatakse ei.


## <a name="configure-intel-mpi"></a>Inteli MPI konfigureerimine

Azure'i Linux RDMA MPI rakenduste käitamiseks peate konfigureerimine teatud kindlate Inteli MPI keskkonna muutujaid. Siin on valimi Bash skripti konfigureerida rakenduse käivitamiseks muutujaid. Muuta vastavalt vajadusele oma installi Inteli MPI mpivars.sh tee.

```
#!/bin/bash -x

# For a SLES 12 SP1 HPC cluster

source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh

# For a CentOS-based HPC cluster 

# source /opt/intel/impi/5.1.3.181/bin64/mpivars.sh

export I_MPI_FABRICS=shm:dapl

# THIS IS A MANDATORY ENVIRONMENT VARIABLE AND MUST BE SET BEFORE RUNNING ANY JOB
# Setting the variable to shm:dapl gives best performance for some applications
# If your application doesn’t take advantage of shared memory and MPI together, then set only dapl

export I_MPI_DAPL_PROVIDER=ofa-v2-ib0

# THIS IS A MANDATORY ENVIRONMENT VARIABLE AND MUST BE SET BEFORE RUNNING ANY JOB

export I_MPI_DYNAMIC_CONNECTION=0

# THIS IS A MANDATORY ENVIRONMENT VARIABLE AND MUST BE SET BEFORE RUNNING ANY JOB

# Command line to run the job

mpirun -n <number-of-cores> -ppn <core-per-node> -hostfile <hostfilename>  /path <path to the application exe> <arguments specific to the application>

#end
```

Host faili vorming on järgmine. Lisage üks rida iga sõlme klaster. Määrake privaatne IP-aadresside kaudu soovitud VNet määratletud varasemas versioonis, pole DNS-i nimed. Näiteks IP-aadressid 10.32.0.1 ja 10.32.0.2 koos kahe hosts fail sisaldab järgmist:

```
10.32.0.1:16
10.32.0.2:16
```

## <a name="run-mpi-on-a-basic-two-node-cluster"></a>Tavaline kaks sõlme klaster MPI käivitamine

Kui te pole seda juba teinud, häälestage esmalt keskkonna jaoks Inteli MPI. 

```
# For a SLES 12 SP1 HPC cluster

source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh

# For a CentOS-based HPC cluster 

# source /opt/intel/impi/5.1.3.181/bin64/mpivars.sh
```

### <a name="run-a-simple-mpi-command"></a>Lihtne MPI käsu käivitamine

Arvuta sõlmed kuvamiseks, et MPI on õigesti installitud ja saate suhelda vahel vähemalt kaks arvutada sõlmed lihtsa MPI käsuga käitamist. **Mpirun** järgmine käsk töötab kaks sõlme käsk **hostname (hostinimi)** .

```
mpirun -ppn 1 -n 2 -hosts <host1>,<host2> -env I_MPI_FABRICS=shm:dapl -env I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -env I_MPI_DYNAMIC_CONNECTION=0 hostname
```
Oma väljundi peaks loendit, sõlmed, mida te edasi sisendina jaoks `-hosts`. Näiteks tagastab on kaks sõlme **mpirun** käsu väljund sarnaneb järgmisega:

```
cluster11
cluster12
```

### <a name="run-an-mpi-benchmark"></a>Mõne MPI võrdlusalus käivitamine

Inteli MPI järgmine käsk käivitatakse pingpong võrdlusalus kontrollida kobar konfigureerimine ja RDMA võrguühendust.

```
mpirun -hosts <host1>,<host2> -ppn 1 -n 2 -env I_MPI_FABRICS=dapl -env I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -env I_MPI_DYNAMIC_CONNECTION=0 IMB-MPI1 pingpong
```

Töötamine kobar koos kahe sõlmed, peaksite väljund sarnaneb järgmisega. Azure'i RDMA võrgus, oodatud latentsus või alla 3 mikrosekundites sõnumi mahud kuni 512 baiti.

```
#------------------------------------------------------------
#    Intel (R) MPI Benchmarks 4.0 Update 1, MPI-1 part
#------------------------------------------------------------
# Date                  : Fri Jul 17 23:16:46 2015
# Machine               : x86_64
# System                : Linux
# Release               : 3.12.39-44-default
# Version               : #5 SMP Thu Jun 25 22:45:24 UTC 2015
# MPI Version           : 3.0
# MPI Thread Environment:
# New default behavior from Version 3.2 on:
# the number of iterations per message size is cut down
# dynamically when a certain run time (per message size sample)
# is expected to be exceeded. Time limit is defined by variable
# "SECS_PER_SAMPLE" (=> IMB_settings.h)
# or through the flag => -time

# Calling sequence was:
# /opt/intel/impi_latest/bin64/IMB-MPI1 pingpong
# Minimum message length in bytes:   0
# Maximum message length in bytes:   4194304
#
# MPI_Datatype                   :   MPI_BYTE
# MPI_Datatype for reductions    :   MPI_FLOAT
# MPI_Op                         :   MPI_SUM
#
#
# List of Benchmarks to run:
# PingPong
#---------------------------------------------------
# Benchmarking PingPong
# #processes = 2
#---------------------------------------------------
       #bytes #repetitions      t[usec]   Mbytes/sec
            0         1000         2.23         0.00
            1         1000         2.26         0.42
            2         1000         2.26         0.85
            4         1000         2.26         1.69
            8         1000         2.26         3.38
           16         1000         2.36         6.45
           32         1000         2.57        11.89
           64         1000         2.36        25.81
          128         1000         2.64        46.19
          256         1000         2.73        89.30
          512         1000         3.09       157.99
         1024         1000         3.60       271.53
         2048         1000         4.46       437.57
         4096         1000         6.11       639.23
         8192         1000         7.49      1043.47
        16384         1000         9.76      1600.76
        32768         1000        14.98      2085.77
        65536          640        25.99      2405.08
       131072          320        50.68      2466.64
       262144          160        80.62      3101.01
       524288           80       145.86      3427.91
      1048576           40       279.06      3583.42
      2097152           20       543.37      3680.71
      4194304           10      1082.94      3693.63

# All processes entering MPI_Finalize

```



## <a name="next-steps"></a>Järgmised sammud

* Proovige juurutamine ja töötab teie Linux MPI rakenduste klaster Linux.

* Vt Inteli MPI [Inteli MPI teegi dokumentatsiooni](https://software.intel.com/en-us/articles/intel-mpi-library-documentation/) juhiseid.

* Proovige [Kiirjuhend malli](https://github.com/Azure/azure-quickstart-templates/tree/master/intel-lustre-clients-on-centos) loomiseks on Inteli läige kobar HPC CentOS asuva pildi abil. Lisateavet leiate teemast selle [ajaveebipostitusest](https://blogs.msdn.microsoft.com/arsen/2015/10/29/deploying-intel-cloud-edition-for-lustre-on-microsoft-azure/).
