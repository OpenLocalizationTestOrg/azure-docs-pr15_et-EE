<properties
 pageTitle="Käivitage täht-CCMi + koos HPC Pack Linux VMs | Microsoft Azure'i"
 description="Microsoft HPC Pack kobar Azure juurutada ja käivitada ka täht-CCMi + töö mitme Linux arvutada sõlmed on RDMA kaudu."
 services="virtual-machines-linux"
 documentationCenter=""
 authors="xpillons"
 manager="timlt"
 editor=""
 tags="azure-service-management,azure-resource-manager,hpc-pack"/>
<tags
 ms.service="virtual-machines-linux"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-linux"
 ms.workload="big-compute"
 ms.date="09/13/2016"
 ms.author="xpillons"/>

# <a name="run-star-ccm-with-microsoft-hpc-pack-on-a-linux-rdma-cluster-in-azure"></a>Käivitage täht-CCMi + koos Microsoft HPC Pack Linux RDMA klaster Azure
Selles artiklis kirjeldatakse, kuidas juurutada Microsoft HPC Pack kobar Azure ja Käivita on [CD-adapco täht-CCMi +](http://www.cd-adapco.com/products/star-ccm%C2%AE) töö mitme Linuxi Arvuta sõlmed, mis on ühendatud InfiniBand.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Microsoft HPC Pack pakub mitmesuguseid suuremahuliste HPC ja paralleelselt rakendusi, sh MPI rakendusi, kogumite, Microsoft Azure'i virtuaalmasinates töötamine. HPC Pack toetab ka Linux MSDN VMs on HPC Pack kobar juurutatud Linux HPC rakendused. Sissejuhatus Linuxi arvutada sõlmed HPC Pack leiate teemast [Alustamine Linux Arvuta sõlmed on Azure HPC Pack klaster](virtual-machines-linux-classic-hpcpack-cluster.md).

## <a name="set-up-an-hpc-pack-cluster"></a>Häälestamine on HPC Pack kobar
Laadige alla HPC Pack IaaS juurutamise skriptide [Allalaadimiskeskuse](https://www.microsoft.com/en-us/download/details.aspx?id=44949) kaudu ja need eraldada kohalikult.

Azure'i PowerShelli on nõutav. Kui teie kohalikus arvutis pole konfigureeritud PowerShelli, lugege artiklit [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md).

Kirjutamise ajal Linux pildid Azure'i turuplatsilt (mis sisaldab InfiniBand draiverid Azure) on SLES 12 CentOS 6.5 ja CentOS 7.1. Selles artiklis on alusel SLES 12 kasutamist. Kõik Linux pildid, kuvatakse Marketplace'ist HPC toetavad nime toomiseks käivitada PowerShelli järgmine käsk:

```
    get-azurevmimage | ?{$_.ImageName.Contains("hpc") -and $_.OS -eq "Linux" }
```

Väljund on loetletud asukohta, kus need pildid on saadaval ja pildi (**ImageName**) kasutatakse hiljem juurutamise malli nime.

Enne juurutamist klaster, tuleb koostada HPC Pack juurutamise mallifail. Kuna me ei suunamise väike kobar, pea sõlme olema selle domeenikontrolleri ja kohaliku andmebaasi SQL-i.

Järgmist malli juurutamine pea sõlme, luua XML-fail nimega **MyCluster.xml**ja teie enda **SubscriptionId**, **StorageAccount**, **asukoht**, **VMName**ja **teenuse nimi** väärtuste asendamine.

    <?xml version="1.0" encoding="utf-8" ?>
    <IaaSClusterConfig>
      <Subscription>
        <SubscriptionId>99999999-9999-9999-9999-999999999999</SubscriptionId>
        <StorageAccount>mystorageaccount</StorageAccount>
      </Subscription>
      <Location>North Europe</Location>
      <VNet>
        <VNetName>hpcvnetne</VNetName>
        <SubnetName>subnet-hpc</SubnetName>
      </VNet>
      <Domain>
        <DCOption>HeadNodeAsDC</DCOption>
        <DomainFQDN>hpc.local</DomainFQDN>
      </Domain>
      <Database>
        <DBOption>LocalDB</DBOption>
      </Database>
      <HeadNode>
        <VMName>myhpchn</VMName>
        <ServiceName>myhpchn</ServiceName>
        <VMSize>Standard_D4</VMSize>
      </HeadNode>
      <LinuxComputeNodes>
        <VMNamePattern>lnxcn-%0001%</VMNamePattern>
        <ServiceNamePattern>mylnxcn%01%</ServiceNamePattern>
        <MaxNodeCountPerService>20</MaxNodeCountPerService>
        <StorageAccountNamePattern>mylnxstorage%01%</StorageAccountNamePattern>
        <VMSize>A9</VMSize>
        <NodeCount>0</NodeCount>
        <ImageName>b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-hpc-v20150708</ImageName>
      </LinuxComputeNodes>
    </IaaSClusterConfig>

PowerShelli käsu sisse tõstetud käsuviipa juht-sõlm loomine käivitamiseks.

```
    .\New-HPCIaaSCluster.ps1 -ConfigFile MyCluster.xml
```

20 – 30 minuti jooksul pärast pea sõlme peaks olema valmis. Saate luua ühenduse selle Azure portaali kaudu, klõpsates ikooni **Loo ühendus** virtuaalse masina.

Lõpuks peate DNS-i ekspediitor parandamiseks. Selleks käivitage DNS-i haldur.

1.  Paremklõpsake serveri nimi DNS-i halduri, valige **Atribuudid**ja seejärel klõpsake vahekaarti **Edasisuunamislahendusi** .

2.  Mis tahes edasisuunamislahendusi eemaldamiseks nuppu **Redigeeri** ja seejärel klõpsake nuppu **OK**.

3.  Veenduge, et ruut **Kasuta root vihjete, kui ei edasisuunamislahendusi on saadaval** oleks märgitud, ja seejärel klõpsake nuppu **OK**.

## <a name="set-up-linux-compute-nodes"></a>Linux Arvuta sõlmed häälestamine
Juurutamist Linux Arvuta sõlmed pea sõlme loomiseks kasutatud sama juurutamise malli abil.

Oma kohalikus arvutis faili **MyCluster.xml** kopeerida pea sõlme ja värskendama **NodeCount** sildi arvu sõlmed, mida soovite kasutada (< = 20). Olge ettevaatlik, et on piisavalt saadaval tuumad Azure meiliteavitus, kuna iga A9 eksemplari tarbib 16 tuuma teie tellimus. Saate kasutada A8 eksemplarid (8 tuuma) asemel A9, kui soovite kasutada sama eelarve veel VMs.

Kopeerige pea sõlme HPC Pack IaaS juurutamise skriptide.

Käivitage tõstetud käsuviipa Azure PowerShelli järgmised käsud:

1.  Käivitage **Lisa-AzureAccount** Azure tellimuse loomiseks.

2.  Kui teil on mitu tellimust, käivitage **Get-AzureSubscription** neid loetleda.

3.  Määrake vaikimisi tellimuse käivitades selle **Valige-AzureSubscription - SubscriptionName xxxx-vaikimisi** käsk.

4.  Käivitage **.\New-HPCIaaSCluster.ps1 - ConfigFile MyCluster.xml** alustamiseks juurutamine Linux arvutada sõlmed.

    ![Toimingu juht-sõlm juurutamine][hndeploy]

Avage tööriist HPC Pack halduri. Mõne minuti pärast Linux Arvuta sõlmed kuvatakse regulaarselt kobar Arvuta sõlmed loendit. Klassikaline juurutamise režiimis, luuakse IaaS VMs järjest. Nii kui sõlmed arv on oluline, saada kõik juurutatud võib kuluda päris palju aega.

![Linux sõlmed HPC Pack halduris][clustermanager]

Nüüd, kui kõik sõlmed on klaster käivitumist, on täiendav taristu sätteid muuta.

## <a name="set-up-an-azure-file-share-for-windows-and-linux-nodes"></a>Windows ja Linux sõlmed Azure'i failikettale häälestamine
Azure'i faili teenuse abil saate talletada skriptide, rakenduse paketid ja andmefailid. Azure'i faili pakub CIFS võimaluste peal Azure'i bloobimälu püsiv pood. Pange tähele, et see ei ole kõige skaleeritav lahendus, kuid see on lihtsam ja ei nõua eraldi VMs.

Luua mõne Azure'i failikettal juhistele artiklis [alustamine Windows Azure'i failisalvestusruumi](..\storage\storage-dotnet-how-to-use-files.md).

Hoida oma salvestusruumi konto nimi **saname**, ühiskasutus failinime nimega **kaustanimi**ja salvestusruumi konto võti nimega **sakey**.

### <a name="mount-the-azure-file-share-on-the-head-node"></a>Mount pea sõlme Azure'i faili ühiskasutusse andmine
Avage administraatoriõigustes Käsuviip ja käivitage järgmine käsk mandaadi talletamiseks võlvkelder kohalikus arvutis:

```
    cmdkey /add:<saname>.file.core.windows.net /user:<saname> /pass:<sakey>
```

Azure'i failikettale ühendada käivitage:

```
    net use Z: \\<saname>.file.core.windows.net\<sharename> /persistent:yes
```

### <a name="mount-the-azure-file-share-on-linux-compute-nodes"></a>Azure'i fail share Linux Mount arvutada sõlmed
Ühe kasulik, mis on kaasas HPC Pack on clusrun tööriist. Selle käsurea tööriista abil saate käivitada samaaegselt sama käsk Arvuta sõlmed kogumi. Meie puhul kasutatakse mount Azure'i faili osa ja püsivad see taaskäivitamisega jääda.
Administraatoriõigustes Käsuviip pea sõlme, käivitage järgmised käsud.

Ühendamine kausta loomiseks tehke järgmist.

```
    clusrun /nodegroup:LinuxNodes mkdir -p /hpcdata
```

Ühendada Azure'i faili ühiskasutusse andmine:

```
    clusrun /nodegroup:LinuxNodes mount -t cifs //<saname>.file.core.windows.net/<sharename> /hpcdata -o vers=2.1,username=<saname>,password='<sakey>',dir_mode=0777,file_mode=0777
```

Püsima mount ühiskasutus:

```
    clusrun /nodegroup:LinuxNodes "echo //<saname>.file.core.windows.net/<sharename> /hpcdata cifs vers=2.1,username=<saname>,password='<sakey>',dir_mode=0777,file_mode=0777 >> /etc/fstab"
```

## <a name="install-star-ccm"></a>Installige täht-CCMi +
Azure'i VM juhtudel A8 ja A9 pakuvad InfiniBand tugi ja RDMA võimalusi. Tuum draiverid, mis võimaldavad need funktsioonid on saadaval Windows Server 2012 R2, SUSE 12, CentOS 6.5 ja CentOS 7.1 pildid Azure'i turuplatsi. Microsoft MPI ja Inteli MPI (versioon 5.x) on kaks MPI teekide Azure need draiverid toetavad.

CD-adapco täht-CCMi + vabastage 11.x ja hiljem on kombineeritud Inteli MPI versioon 5.x, et InfiniBand Azure'i tugi on kaasatud.

Hankida soovitud Linux64 täht-CCMi + pakkimine [CD-adapco portaali](https://steve.cd-adapco.com). Meie puhul kasutasime versiooni 11.02.010 kombineeritud täpsust.

Looge pea sõlme **/hpcdata** Azure Failide ühiskasutamine, shell script nimega **setupstarccm.sh** sisu on järgmine. See skript käivitub häälestamiseks täht iga MSDN-CCMi + kohalikult.

#### <a name="sample-setupstarcmsh-script"></a>Skripti näide setupstarcm.sh
```
    #!/bin/bash
    # setupstarcm.sh to set up STAR-CCM+ locally

    # Create the CD-adapco main directory
    mkdir -p /opt/CD-adapco

    # Copy the STAR-CCM package from the file share to the local directory
    cp /hpcdata/StarCCM/STAR-CCM+11.02.010_01_linux-x86_64.tar.gz /opt/CD-adapco/

    # Extract the package
    tar -xzf /opt/CD-adapco/STAR-CCM+11.02.010_01_linux-x86_64.tar.gz -C /opt/CD-adapco/

    # Start a silent installation of STAR-CCM without the FLEXlm component
    /opt/CD-adapco/starccm+_11.02.010/STAR-CCM+11.02.010_01_linux-x86_64-2.5_gnu4.8.bin -i silent -DCOMPUTE_NODE=true -DNODOC=true -DINSTALLFLEX=false

    # Update memory limits
    echo "*               hard    memlock         unlimited" >> /etc/security/limits.conf
    echo "*               soft    memlock         unlimited" >> /etc/security/limits.conf
```
Nüüd, et täht häälestamine – CCMi + kõik teie Linux arvutada sõlmed, avage administraatoriõigustes Käsuviip ja käivitage järgmine käsk:

```
    clusrun /nodegroup:LinuxNodes bash /hpcdata/setupstarccm.sh
```

Kui käsk töötab, saate jälgida CPU hõivatus intensiivsuskaart, halduri abil. Mõne minuti pärast kõik sõlmed peaks olema õigesti seadistatud.

## <a name="run-star-ccm-jobs"></a>Käivitage täht-CCMi + tööde haldamine
HPC Pack kasutatakse oma töö scheduler võimaluste käivitamiseks täht-tööde haldamine CCMi +. Selleks tuleb mõni skripte käivitada töö ja täht kasutatud tugi-CCMi +. Sisendandmete säilitatakse Azure'i fail share kõigepealt lihtne.

Kasutatakse järgmist PowerShelli skripti järjekorda täht-CCMi + töö. See on kolme argumenti:

*  Mudeli nimi

*  Arvu sõlmed kasutada.

*  Iga sõlme kasutatava valdkond arv

Kuna täht-CCMi + saate täita mälu ribalaiust, tavaliselt on parem kasutada vähem tuumad kohta Arvuta sõlmed ja lisada uusi sõlmi. Täpset arvu valdkond ühe sõlme sõltub protsessor pere ja InterConnecti kiirust.

Sõlmed on eraldatud ainult töö ja ei saa ühiskasutusse anda muud tööd. Töö pole alustatud on MPI tööd otse. **Runstarccm.sh** shell script hakkavad MPI käivitit.

Sisestuskeel mudeli ja **runstarccm.sh** script on talletatud **/hpcdata** ühiskasutus, mis on varem paigaldatud.

Logifailide on nimega töö ID-ga ja talletatakse selle **/hpcdata ühiskasutus**koos TÄHTE-CCMi + väljundi faili.


#### <a name="sample-submitstarccmjobps1-script"></a>SubmitStarccmJob.ps1 skripti näide
```
    Add-PSSnapin Microsoft.HPC -ErrorAction silentlycontinue
    $scheduler="headnodename"
    $modelName=$args[0]
    $nbCoresPerNode=$args[2]
    $nbNodes=$args[1]

    #---------------------------------------------------------------------------------------------------------
    # Create a new job; this will give us the job ID that's used to identify the name of the uploaded package in Azure
    #
    $job = New-HpcJob -Name "$modelName $nbNodes $nbCoresPerNode" -Scheduler $scheduler -NumNodes $nbNodes -NodeGroups "LinuxNodes" -FailOnTaskFailure $true -Exclusive $true
    $jobId = [String]$job.Id

    #---------------------------------------------------------------------------------------------------------
    # Submit the job    
    $workdir =  "/hpcdata"
    $execName = "$nbCoresPerNode runner.java $modelName.sim"

    $job | Add-HpcTask -Scheduler $scheduler -Name "Compute" -stdout "$jobId.log" -stderr "$jobId.err" -Rerunnable $false -NumNodes $nbNodes -Command "runstarccm.sh $execName" -WorkDir "$workdir"


    Submit-HpcJob -Job $job -Scheduler $scheduler
```
Asendage **runner.java** oma eelistatud täht-CCMi + Java mudeli käiviti ja logimine kood.

#### <a name="sample-runstarccmsh-script"></a>Skripti näide runstarccm.sh
```
    #!/bin/bash
    echo "start"
    # The path of this script
    SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"
    echo ${SCRIPT_PATH}
    # Set the mpirun runtime environment
    export CDLMD_LICENSE_FILE=1999@flex.cd-adapco.com

    # mpirun command
    STARCCM=/opt/CD-adapco/STAR-CCM+11.02.010/star/bin/starccm+

    # Get node information from ENVs
    NODESCORES=(${CCP_NODES_CORES})
    COUNT=${#NODESCORES[@]}
    NBCORESPERNODE=$1

    # Create the hostfile file
    NODELIST_PATH=${SCRIPT_PATH}/hostfile_$$
    echo ${NODELIST_PATH}

    # Get every node name and write into the hostfile file
    I=1
    NBNODES=0
    while [ ${I} -lt ${COUNT} ]
    do
        echo "${NODESCORES[${I}]}" >> ${NODELIST_PATH}
        let "I=${I}+2"
        let "NBNODES=${NBNODES}+1"
    done
    let "NBCORES=${NBNODES}*${NBCORESPERNODE}"

    # Run STAR-CCM with the hostfile argument
    #  
    ${STARCCM} -np ${NBCORES} -machinefile ${NODELIST_PATH} \
        -power -podkey "<yourkey>" -rsh ssh \
        -mpi intel -fabric UDAPL -cpubind bandwidth,v \
        -mppflags "-ppn $NBCORESPERNODE -genv I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -genv I_MPI_DAPL_UD=0 -genv I_MPI_DYNAMIC_CONNECTION=0" \
        -batch $2 $3
    RTNSTS=$?
    rm -f ${NODELIST_PATH}

    exit ${RTNSTS}
```

Meie testi kasutasime Power nõudmisel litsentsi luba. Selle luba teil on seatud **$CDLMD_LICENSE_FILE** keskkonna muutuja **1999@flex.cd-adapco.com** ja **- podkey** suvandi käsurea võti.

Pärast teatud lähtestamine skripti ekstraktib-- **$CCP_NODES_CORES** keskkonna muutujaid, mis HPC Pack set - loendi koostamiseks hostfile, mis kasutab MPI käivitit sõlmed. See hostfile sisaldab loendit Arvuta sõlm nimed, mida kasutatakse töö üks nimi rea kohta.

Vormingu **$CCP_NODES_CORES** järgmised see muster.

```
<Number of nodes> <Name of node1> <Cores of node1> <Name of node2> <Cores of node2>...`
```

Kui:

* `<Number of nodes>`on eraldatud selle töö sõlmed arv.

* `<Name of node_n_...>`on iga sõlme eraldada selle töö nimi.

* `<Cores of node_n_...>`on valdkond eraldada selle projekti sõlm arv.

Arvu valdkond (**$NBCORES**) arvutatakse ka sõlmed (**$NBNODES**) arvu ja valdkond ühe sõlme arvu põhjal (parameetrina antud **$NBCORESPERNODE**).

MPI suvandeid, mida kasutatakse koos Inteli MPI Azure need on:

*   `-mpi intel`Inteli MPI määramiseks.

*   `-fabric UDAPL`Azure'i InfiniBand verbide kasutamine.

*   `-cpubind bandwidth,v`optimeerida läbilaskevõime MPI tähega-CCMi +.

*   `-mppflags "-ppn $NBCORESPERNODE -genv I_MPI_DAPL_PROVIDER=ofa-v2-ib0 -genv I_MPI_DAPL_UD=0 -genv I_MPI_DYNAMIC_CONNECTION=0"`Inteli MPI töötamine Azure'i InfiniBand ja nõutav numbri valdkond sõlm kohta.

*   `-batch`TÄHT käivitamiseks-CCMi + kogumitena pole Kasutajaliidese abil.


Lõpuks alustada tööd, veenduge, et teie sõlmed on ja töötab ja on võrgus halduris. PowerShelli käsureale, käivitage järgmine:

```
    .\ SubmitStarccmJob.ps1 <model> <nbNodes> <nbCoresPerNode>
```

## <a name="stop-nodes"></a>Sõlmed peatamine
Hiljem kui olete valmis oma testide abil, saate kasutada järgmisi HPC Pack PowerShelli käske lõpetada ja alustada sõlmed:

```
    Stop-HPCIaaSNode.ps1 -Name <prefix>-00*
    Start-HPCIaaSNode.ps1 -Name <prefix>-00*
```

## <a name="next-steps"></a>Järgmised sammud
Proovige muud Linux töökoormus. Näiteks järgmistest teemadest.

* [Käivitage Microsoft HPC Pack Linux NAMD arvutada Azure sõlmed](virtual-machines-linux-classic-hpcpack-cluster-namd.md)

* [Käivitage Microsoft HPC Pack OpenFOAM Linux RDMA kobar Azure](virtual-machines-linux-classic-hpcpack-cluster-openfoam.md)


<!--Image references-->
[hndeploy]: ./media/virtual-machines-linux-classic-hpcpack-cluster-starccm/hndeploy.png
[clustermanager]: ./media/virtual-machines-linux-classic-hpcpack-cluster-starccm/ClusterManager.png
