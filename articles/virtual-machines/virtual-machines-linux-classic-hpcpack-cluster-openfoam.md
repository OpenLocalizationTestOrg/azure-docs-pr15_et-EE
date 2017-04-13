<properties
 pageTitle="Käivitage OpenFOAM HPC Pack Linux VMs | Microsoft Azure'i"
 description="Microsoft HPC Pack kobar Azure juurutada ja käivitada mõne OpenFOAM töö mitme Linux arvutada sõlmed on RDMA kaudu."
 services="virtual-machines-linux"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-service-management,azure-resource-manager,hpc-pack"/>
<tags
 ms.service="virtual-machines-linux"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-linux"
 ms.workload="big-compute"
 ms.date="07/22/2016"
 ms.author="danlep"/>

# <a name="run-openfoam-with-microsoft-hpc-pack-on-a-linux-rdma-cluster-in-azure"></a>Käivitage Microsoft HPC Pack OpenFoam Linux RDMA kobar Azure

Selles artiklis kirjeldatakse, üks viis, kuidas käivitada OpenFoam Azure'i virtuaalmasinates. Siin on juurutamist Microsoft HPC Pack kobar Linux Arvuta sõlmed Azure ja Käivita mõne [OpenFoam](http://openfoam.com/) töö koos Inteli MPI. Saate RDMA-ühenduse võimalusega Azure VMs Arvuta sõlmed, nii, et Arvuta sõlmed suhelda Azure'i RDMA võrgu kaudu. Muude suvandite käivitamiseks OpenFoam Azure kaasake turuplatsilt, nt UberCloud's [OpenFoam 2.3 CentOS 6](https://azure.microsoft.com/marketplace/partners/ubercloud/openfoam-v2dot3-centos-v6/)ja käivitades [Azure'i paketi](https://blogs.technet.microsoft.com/windowshpc/2016/07/20/introducing-mpi-support-for-linux-on-azure-batch/)täielikult konfigureeritud trükikojas pildid on saadaval. 

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

OpenFOAM (kasutamiseks avage väli ja Stringimanipuleerimisfunktsioonide) on avatud lähtekoodi arvutuslik vedeliku dynamics (CFD) tarkvarapaketi, mida kasutatakse ulatuslikult matemaatika- ja teadusvõrrandeid, äri- ja akadeemiliste asutuste. See sisaldab tööriistu asenda-kohest vajadust, siduda eelkõige snappyHexMesh parallelized mesher, keerukate CAD mõõtmete ja enne ja pärast töötlemist. Peaaegu kõik protsessi käivitada paralleelselt, mis võimaldab kasutajatel käsutuses arvuti riistvara täielikult ära.  

Microsoft HPC Pack pakub suuremahuliste HPC ja paralleelselt rakendusi, sh MPI rakendusi, kogumite, Microsoft Azure'i virtuaalmasinates töötamine. HPC Pack toetab samuti töötava Linux HPC rakendusi Linux arvutada VMs juurutatud on HPC Pack kobar sõlm. Teemast [Alustamine Linux Arvuta sõlmed on HPC Pack klaster Azure](virtual-machines-linux-classic-hpcpack-cluster.md) HPC Pack Linux Arvuta sõlmed funktsiooniga tutvustus.

>[AZURE.NOTE] Selles artiklis on näidatud, kuidas käivitada Linuxi MPI töökoormus HPC Pack. Eeldatakse, et teil on mõne tuttava Linux süsteemi halduse ja Linux kogumite töötavate MPI töökoormus. Kui kasutate versiooni MPI ja OpenFOAM erineva selles artiklis, peate muuta mõne installi ja konfiguratsiooni juhiseid. 

## <a name="prerequisites"></a>Eeltingimused

*   **HPC Pack kobar RDMA-ühenduse võimalusega Linux arvutada sõlmed** - juurutada mõne HPC Pack kobar suurus A8, A9, H16r või H16rm Linux Arvuta sõlmed mõni [Azure ressursihaldur malli](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) või on [Azure PowerShelli skripti](virtual-machines-linux-classic-hpcpack-cluster-powershell-script.md)abil. Vaadake [Alustamine Linux Arvuta sõlmed on HPC Pack klaster Azure](virtual-machines-linux-classic-hpcpack-cluster.md) eeltingimused ja kas variant juhised. Kui valite suvandi PowerShelli skripti juurutamine, vt käesoleva artikli lõpus näidisfailide konfiguratsiooni näidisfaili. Selle konfiguratsiooni abil saate juurutada Azure'i HPC Pack kobar, mis koosneb suurus A8 Windows Server 2012 R2 pea sõlme ja 2 suurus A8 SUSE Linux Enterprise Server 12 Arvuta sõlmed. Teie tellimus ja teenuse nimede väärtused asendada. 

    **Täiendavad näpunäited**

    *   [H-sari ja Arvuta mahukat A-sarja VMs kohta](virtual-machines-windows-a8-a9-a10-a11-specs.md)leiate jaotisest Linux RDMA võrgu eeltingimused Azure.

    *   Kui kasutate suvandi PowerShelli skripti juurutamine, juurutada kõik Linuxi Arvuta sõlmed, ühe pilveteenuses kasutada RDMA võrguühendus sees.

    *   Pärast võtavad Linux sõlmed, ühendada SSH mis tahes täiendavaid haldustoimingute sooritamiseks. Otsimine SSH ühenduse üksikasju iga Linux VM Azure portaalis.  
        
*   **Inteli MPI** - OpenFOAM käitamist SLES 12 HPC arvutada Azure sõlmed, peate installima Inteli MPI teegi 5 käitusaja [Intel.com saidi](https://software.intel.com/en-us/intel-mpi-library/)kaudu. (Inteli MPI 5 on eelinstallitud HPC CentOS-põhiste piltide.)  Hiljem juhises vajaduse korral installida Inteli MPI oma Linux Arvuta sõlmed. Ettevalmistamiseks selles etapis tuleb pärast registreerimist koos Inteli, klõpsake linki kinnitavast seotud veebilehele. Seejärel kopeerige allalaadimise link .tgz faili jaoks sobiv versioon Inteli MPI. Selles artiklis on Inteli MPI versioon 5.0.3.048 alusel.

*   **OpenFOAM Allikas Pack** - allalaadimine OpenFOAM Allikas Pack tarkvara Linux [OpenFOAM Foundation saidi](http://openfoam.org/download/2-3-1-source/)kaudu. Selles artiklis põhineb nimega OpenFOAM-2.3.1.tgz Allikas Pack versiooni 2.3.1 allalaadimiseks saadaval. Järgige selle artikli lahti ja koostada OpenFOAM Linux Arvuta sõlmed.

*   **EnSight** (valikuline) – teie OpenFOAM simulatsiooni tulemuste vaatamiseks, laadige alla ja installige [EnSight](https://www.ceisoftware.com/download/) visualiseerimine ja analüüsi programm. Litsentsimine ja allalaadimine andmed on EnSight saidil.


## <a name="set-up-mutual-trust-between-compute-nodes"></a>Vastastikune usaldus vahel Arvuta sõlmed häälestamine

Rist-sõlm töö mitme Linuxi sõlmed nõuab sõlmed usaldusväärsuse üksteisest ( **rsh** või **ssh**) alusel. HPC Pack kobar loomisel Microsoft HPC Pack IaaS juurutamise skripti skripti automaatselt häälestab püsivate vastastikune usaldus teie määratud administraatori konto. Administraator kasutajate loodud soovitud klaster domeeni häälestada ajutine vastastikune usaldus sõlme vahel, kui neile on määratud töö ja hävitada seose pärast töö on valmis. Luua usalda iga kasutaja jaoks, sisestage HPC Pack usalda seose kasutava klaster RSA paari.

### <a name="generate-an-rsa-key-pair"></a>Luua mõne RSA paari

See on lihtne RSA võti paari, mis sisaldab avalik võti ja privaatvõti, Linux **ssh-keygen** käsu loomiseks.

1.  Logige arvutisse Linux.

2.  Käivitage järgmine käsk:

    ```
    ssh-keygen -t rsa
    ```

    >[AZURE.NOTE] Vajutage klahvi **Enter** kasutada vaikesätteid käsu lõpetamiseni. Sisestage parool siin; Kui küsitakse parooli, vajutage lihtsalt sisestusklahvi **Enter**.

    ![Luua mõne RSA paari][keygen]

3.  Muuda kataloogi ~/.ssh kataloog. Privaatvõti on talletatud id_rsa ja id_rsa.pub avalik võti.

    ![Era- ja võtmed][keys]

### <a name="add-the-key-pair-to-the-hpc-pack-cluster"></a>Võti paari lisamine HPC Pack kobar
1.  Tehke oma pea sõlme oma HPC Pack administraatorikontoga (administraatori konto häälestamist, kui käivitasite juurutamise skript) kaugtöölaua ühendus.

2. Kasutage funktsiooni kobar Active Directory domeeni domeeni kasutajakonto loomiseks standard Windows Server toiminguid. Näiteks kasutage pea sõlme tööriista Active Directory kasutaja ja arvutites. Selle artikli näited Oletagem, et luua nimega hpclab\hpcuser domeeni kasutaja.

3.  Looge fail nimega C:\cred.xml ja kopeerige see RSA olulisi andmeid. Cred.xml näidisfaili on käesoleva artikli lõpus.

    ```
    <ExtendedData>
        <PrivateKey>Copy the contents of private key here</PrivateKey>
        <PublicKey>Copy the contents of public key here</PublicKey>
    </ExtendedData>
    ```

4.  Avage käsuviip ja sisestage järgmine käsk hpclab\hpcuser konto identimisteave määrata. **Extendeddata** parameetri abil võtme andmete jaoks loodud C:\cred.xml faili nime.

    ```
    hpccred setcreds /extendeddata:c:\cred.xml /user:hpclab\hpcuser /password:<UserPassword>
    ```

    See käsk on lõpule jõudnud edukalt ilma väljundi. Pärast mandaadi kasutajakonto, peate käivitama töö seadmisele turvalises asukohas cred.xml faili salvestada või kustutada.

5.  Kui teie loodud RSA võti paari ühele oma Linux sõlmed, ärge unustage võtmed kustutada, kui olete lõpetanud, kasutades neid. Kui HPC Pack leiab id_rsa olemasoleva faili või id_rsa.pub faili, seda häälestada vastastikune usaldus.

>[AZURE.IMPORTANT] Me ei soovita töötab Linux töö administraatorina kobar ühiskasutusega kobar, kuna administraatori poolt töö töötab Linux sõlmed root konto. Kuid töötab administraator kasutaja poolt töö kohaliku Linux kasutajakontoga töö kasutaja sama nimi. Sel juhul HPC Pack häälestab vastastikune usaldus selle Linuxi kasutaja projektile eraldatud sõlmed üle. Saate häälestada Linuxi kasutaja käsitsi Linuxi sõlmed enne töö või HPC Pack luuakse kasutajale automaatselt töö esitamisel. Kui HPC Pack loob kasutaja, HPC Pack kustutab selle pärast töö lõpulejõudmist. Turvalisus ohtude vähendamiseks eemaldab HPC Pack pärast töö lõpetamist võtmed.

## <a name="set-up-a-file-share-for-linux-nodes"></a>Faili ühiskasutus Linux sõlmed häälestamine

Nüüd saate seadistada standardse SMB osa pea sõlme kausta. Kui soovite lubada Linux sõlmed levinud tee rakenduse failidele juurde pääseda, ühendage ühiskausta sõlmed Linux. Soovi korral saate muu faili ühiskasutuse suvandi, nt Azure failide ühiskasutus – mis tööiga - või NFS osa jaoks on soovitatav kasutada. Vaadake faili teabe ja üksikasjalikud juhised [Linux Arvuta sõlmed on HPC Pack klaster Azure alustamine](virtual-machines-linux-classic-hpcpack-cluster.md).

1.  Looge kaust pea sõlme ja ühiskasutusse anda kõigile seadmisega lugemis-ja kirjutamisõigusega õigusi. Näiteks jagada C:\OpenFOAM pea sõlme nimega \\ \\SUSE12RDMA-HN\OpenFOAM. Siin on *SUSE12RDMA-HN* pea sõlme hosti nimi.

2.  Avage Windows PowerShelli aken ja käivitage järgmine käsk:

    ```
    clusrun /nodegroup:LinuxNodes mkdir -p /openfoam

    clusrun /nodegroup:LinuxNodes mount -t cifs //SUSE12RDMA-HN/OpenFOAM /openfoam -o vers=2.1`,username=<username>`,password='<password>'`,dir_mode=0777`,file_mode=0777
    ```

Esimese käsu loob kausta nimega /openfoam kõik sõlmed LinuxNodes jaotises. Teine käsk kinnitused ühiskausta //SUSE12RDMA-HN/OpenFOAM Linux sõlmed koos dir_mode ja file_mode bittide seatud 777. *Kasutajanime* ja *parooli* käsk peaks olema pea sõlme kasutaja mandaat.

>[AZURE.NOTE]Funktsiooni "\`" sümbol, teine käsk on escape sümbol PowerShelli jaoks. "\`," tähendab "," (koma) on osa käsk.

## <a name="install-mpi-and-openfoam"></a>MPI ja OpenFOAM installimine

Käivituma OpenFOAM on MPI töökohtade RDMA võrgus, peate kompileerida OpenFOAM Inteli MPI teekide abil. 

Esmalt käitada mitut **clusrun** installida Inteli MPI teekide (kui see pole juba installitud) ja OpenFOAM oma Linux sõlmed. Kasutage pea sõlme jagamine teistega jagamiseks installifailid Linux sõlme vahel eelnevalt konfigureeritud.

>[AZURE.IMPORTANT]Näited nende installi ja koostamise juhiseid. Teil on vaja mõned teadmised Linux süsteemi halduse tagamiseks sõltuvad koostajad ja teekide õigesti installitud. Võib juhtuda teatud keskkonna muutujate või muud sätted oma versioonide Inteli MPI ja OpenFOAM muutmine. Lisateavet leiate teemast [Inteli MPI teegi Linux installi juhendi](http://registrationcenter-download.intel.com/akdlm/irc_nas/1718/INSTALL.html?lang=en&fileExt=.html) ja [OpenFOAM allika hoolduspaketi installimise](http://openfoam.org/download/2-3-1-source/) keskkonnas.


### <a name="install-intel-mpi"></a>Inteli MPI installimine

Salvestada allalaaditud installipaketi Inteli MPI (selles näites l_mpi_p_5.0.3.048.tgz) jaoks C:\OpenFoam pea sõlme nii, et Linux sõlmed pääseb /openfoam seda faili. Käivitage **clusrun** installida Inteli MPI teegi Linux sõlme.

1.  Järgmised käsud installipaketi kopeerida ja eraldada selle /opt/intel iga sõlme.

    ```
    clusrun /nodegroup:LinuxNodes mkdir -p /opt/intel

    clusrun /nodegroup:LinuxNodes cp /openfoam/l_mpi_p_5.0.3.048.tgz /opt/intel/

    clusrun /nodegroup:LinuxNodes tar -xzf /opt/intel/l_mpi_p_5.0.3.048.tgz -C /opt/intel/
    ```

2.  Inteli MPI teegi vaikselt installimiseks kasutage silent.cfg faili. Ühe näite leiate näidisfailide käesoleva artikli lõpus. Ühiskausta /openfoam paigutada seda faili. Silent.cfg faili kohta leiate üksikasjalikumat teavet teemast [Inteli MPI teegi Linux installi juhendi - vaikne installimine](http://registrationcenter-download.intel.com/akdlm/irc_nas/1718/INSTALL.html?lang=en&fileExt=.html#silentinstall).

    >[AZURE.TIP]Veenduge, et silent.cfg faili Linux teksti failina salvestatud joone lõpud (ainult Reavahetus, mitte CR LF). See toiming tagab, et see töötab õigesti sõlmed Linux.

3.  Installige Inteli MPI teegi vaigistatud.
 
    ```
    clusrun /nodegroup:LinuxNodes bash /opt/intel/l_mpi_p_5.0.3.048/install.sh --silent /openfoam/silent.cfg
    ```
    
### <a name="configure-mpi"></a>MPI konfigureerimine

Testimiseks, lisage järgmised read /etc/security/limits.conf iga Linux sõlmed:


    clusrun /nodegroup:LinuxNodes echo "*               hard    memlock         unlimited" `>`> /etc/security/limits.conf
    clusrun /nodegroup:LinuxNodes echo "*               soft    memlock         unlimited" `>`> /etc/security/limits.conf


Taaskäivitage Linux sõlmed pärast värskendamist limits.conf faili. Näiteks kasutada **clusrun** järgmine käsk:

```
clusrun /nodegroup:LinuxNodes systemctl reboot
```

Pärast taaskäivitada, veenduge, et ühiskausta on paigaldatud /openfoam.

### <a name="compile-and-install-openfoam"></a>Kompileerida ja installige OpenFOAM

Salvestada allalaaditud installipaketi OpenFOAM allika Pack (selles näites OpenFOAM-2.3.1.tgz) abil C:\OpenFoam pea sõlme, et Linux sõlmed pääseb /openfoam seda faili. Käivitage **clusrun** käsud Linux sõlme OpenFOAM koostada.


1.  Luua kausta /opt/OpenFOAM iga Linux sõlme, kopeerida allikas paketi see kaust ja ekstraktida see seal.

    ```
    clusrun /nodegroup:LinuxNodes mkdir -p /opt/OpenFOAM

    clusrun /nodegroup:LinuxNodes cp /openfoam/OpenFOAM-2.3.1.tgz /opt/OpenFOAM/
    
    clusrun /nodegroup:LinuxNodes tar -xzf /opt/OpenFOAM/OpenFOAM-2.3.1.tgz -C /opt/OpenFOAM/
    ```

2.  Koguda OpenFOAM Inteli MPI teegiga, häälestage esmalt mõni keskkonna muutujate Inteli MPI nii OpenFOAM. Nimega settings.sh bash skripti abil saate määrata muutujaid. Ühe näite leiate näidisfailide käesoleva artikli lõpus. Ühiskausta /openfoam paigutada selle faili (salvestatud Linux rea lõpud). See fail sisaldab ka MPI ja OpenFOAM runtimes, mida hiljem kasutada ka OpenFOAM töö sätted.

3. Installige OpenFOAM koostamiseks vajalikud sõltuvad paketid. Olenevalt teie Linux jaotuse, võib esmalt peate lisama hoidla. Käivitage **clusrun** käsud, mis on järgmine:

    ```
    clusrun /nodegroup:LinuxNodes zypper ar http://download.opensuse.org/distribution/13.2/repo/oss/suse/ opensuse
    
    clusrun /nodegroup:LinuxNodes zypper -n --gpg-auto-import-keys install --repo opensuse --force-resolution -t pattern devel_C_C++
    ```
    
    Vajaduse korral SSH iga Linux sõlme käskude kinnitamiseks esitataks õigesti.

4.  Käivitage järgmine käsk OpenFOAM koostada. Koostamise protsess võtab aega ja genereeritud Logiteave standard väljund suurt hulka, nii et kasutada **/ vaikevormingus** suvand vaikevormingus väljund kuvamiseks.

    ```
    clusrun /nodegroup:LinuxNodes /interleaved source /openfoam/settings.sh `&`& /opt/OpenFOAM/OpenFOAM-2.3.1/Allwmake
    ```
    
    >[AZURE.NOTE]Funktsiooni "\`" sümbol käsk on escape sümbol PowerShelli jaoks. "\`&" tähendab, et on "&" on osa käsk.

## <a name="prepare-to-run-an-openfoam-job"></a>Mõne OpenFOAM töö käivitamiseks ettevalmistamine

Nüüd saavad MPI töö, mis nimega sloshingTank3D, mis on üks OpenFoam näidised, kaks Linux sõlme käivitamiseks valmis. 

### <a name="set-up-the-runtime-environment"></a>Käitusaja keskkonna häälestamine

Häälestamine käitusaja keskkonnas MPI ja OpenFOAM Linux sõlmed, käivitage järgmine käsk Windows PowerShelli aknas pea sõlme. (See käsk on SUSE Linuxi jaoks sobiv ainult.)

```
clusrun /nodegroup:LinuxNodes cp /openfoam/settings.sh /etc/profile.d/
```

### <a name="prepare-sample-data"></a>Näidisandmete ettevalmistamine

Kasutage pea sõlme ühiskasutus eelnevalt konfigureeritud Linux sõlme (paigaldatud /openfoam) vahel faile ühiskasutusse anda.

1.  SSH ühele oma Linux arvutada sõlmed.

2.  Kui te pole seda juba teinud, käivitage järgmine käsk OpenFOAM käitusaja keskkonna häälestamiseks.

    ```
    $ source /openfoam/settings.sh
    ```
    
3.  Ühiskausta sloshingTank3D valimi kopeerida ja avage see.

    ```
    $ cp -r $FOAM_TUTORIALS/multiphase/interDyMFoam/ras/sloshingTank3D /openfoam/

    $ cd /openfoam/sloshingTank3D
    ```

4.  Väärtused, mis seda proovi kasutamisel võib kuluda kümneid minutit, et käivitada, seega võite soovida muuta mõningaid parameetreid teha kiirema. Üks lihtne valik on aeg samm muutujate deltaT ja writeInterval süsteemi/controlDict faili muutmiseks. Selles failis talletatakse kõik sisendandmete aja ja lugemise ja kirjutamise lahenduse andmete kontrollimiseks. Näiteks saate muuta väärtuse 0,5 0,05 deltaT ja writeInterval 0,05-0,5 väärtus.

    ![Samm muutujate muutmine][step_variables]

5.  Määrake soovitud väärtused muutujate süsteemi/decomposeParDict faili. Selles näites kasutatakse sõlmed kaks Linux 8 tuuma koos, et määrata numberOfSubdomains 16 ja n, et hierarchicalCoeffs (1 1 16), mis tähendab, et paralleelselt OpenFOAM 16 protsessid. Lisateavet leiate teemast [OpenFOAM kasutusjuhend: 3.4 töötab rakenduste paralleelselt](http://cfd.direct/openfoam/user-guide/running-applications-parallel/#x12-820003.4).

    ![Lagunevad protsessid][decompose]

6.  Käivitage järgmised käsud sloshingTank3D kataloogi Näidisandmete ettevalmistamine.

    ```
    $ . $WM_PROJECT_DIR/bin/tools/RunFunctions

    $ m4 constant/polyMesh/blockMeshDict.m4 > constant/polyMesh/blockMeshDict

    $ runApplication blockMesh

    $ cp 0/alpha.water.org 0/alpha.water

    $ runApplication setFields  
    ```
    
7.  Pea sõlme, peaksite nägema näidisfailide andmed kopeeritakse C:\OpenFoam\sloshingTank3D. (C:\OpenFoam ei pea sõlme ühiskausta.)

    ![Andmefailid pea sõlme][data_files]

### <a name="host-file-for-mpirun"></a>Host faili mpirun

Selles etapis loote host faili (Arvuta sõlmed loend), mis kasutab **mpirun** käsk.

1.  Üks Linux sõlmed, looge fail nimega hostfile /openfoam, klõpsake jaotises nii, et selle faili saab teile helistada /openfoam/hostfile kõik Linuxi sõlmed.

2.  Kirjutage oma Linux sõlm nimed sellesse faili. Selles näites fail sisaldab järgmist nimed:
    
    ```       
    SUSE12RDMA-LN1
    SUSE12RDMA-LN2
    ```
    
    >[AZURE.TIP]Saate luua ka selle faili C:\OpenFoam\hostfile pea sõlme. Kui valite selle suvandi, salvestage see tekstifail Linux rida lõpud (ainult Reavahetus, mitte CR LF). See tagab, et see töötab õigesti sõlmed Linux.

    **Bash skripti ümbris**

    Kui teil on palju Linux sõlmi ja soovite oma töö töötab vaid mõned neist, ei ole hea mõte kasutada fikseeritud host faili, kuna te ei tea, millised sõlmed eraldatakse oma töö. Sel juhul kirjutage bash skripti ümbrisesse jaoks **mpirun** loomiseks faili host automaatselt. Saate otsida näide bash skripti ümbris nimega hpcimpirun.sh käesoleva artikli lõpus, ja salvestage see /openfoam/hpcimpirun.sh. Selles näites skripti teeb järgmist.

    1.  Häälestab keskkonna muutujate **mpirun**ja mõned lisamine käsu parameetrid MPI töö käivitamiseks RDMA võrgu kaudu. Sel juhul seda määrab järgmised muutujad:

        *   I_MPI_FABRICS = shm:dapl
        *   I_MPI_DAPL_PROVIDER = ofa-v2-ib0
        *   I_MPI_DYNAMIC_CONNECTION = 0

    2.  Loob host faili vastavalt muutuv $CCP_NODES_CORES, mis on määratud pea sõlme HPC töö aktiveerimisel keskkonnas.

        Vormingu $CCP_NODES_CORES järgmised see muster.

        ```
        <Number of nodes> <Name of node1> <Cores of node1> <Name of node2> <Cores of node2>...`
        ```

        Kui

        * `<Number of nodes>`-eraldada selle töö sõlmed arv.  
        
        * `<Name of node_n_...>`-iga sõlme eraldada selle töö nime.
        
        * `<Cores of node_n_...>`-valdkond eraldada selle töö sõlme arvu.

        Näiteks kui töö peab kaks sõlme käivitamiseks, $CCP_NODES_CORES on sarnane
        
        ```
        2 SUSE12RDMA-LN1 8 SUSE12RDMA-LN2 8
        ```
        
    3.  Nõuab käsk **mpirun** ja lisab käsurea parameetrid kaks.

        * `--hostfile <hostfilepath>: <hostfilepath>`– loob script host faili tee

        * `-np ${CCP_NUMCPUS}: ${CCP_NUMCPUS}`-keskkonnas muutuja, mis pea sõlme HPC Pack, mis salvestab kokku valdkond eraldada selle töö arvu määratud. Sel juhul saate määrata protsesside jaoks **mpirun**arv.


## <a name="submit-an-openfoam-job"></a>Vormiandmete edastamine on OpenFOAM töö

Nüüd saate esitada töö HPC halduris. Tuleb läbida skripti hpcimpirun.sh käsk read mõne tööülesannetest.

1. Ühenduse loomine oma kobar pea sõlme ja HPC halduri käivitamine.

2. **Ressursside haldamine**, veenduge, et Linux Arvuta sõlmed oleks **Online'i** olekus. Kui pole, valige need ja klõpsake nuppu **Too veebis**.

3.  **Töö haldus**nuppu **Uus töökoht**.

4.  Sisestage nimi, nt _sloshingTank3D_töö.

    ![Töö üksikasjad][job_details]

5.  **Töö ressursid**, valige ressursi nimega "Sõlme" tüüp ja minimaalne väärtuseks 2. Selle konfiguratsiooni töö töötab kaks Linux sõlme, mis on kaheksa südamikud selles näites.

    ![Töö ressursid][job_resources]

6. Klõpsake vasakpoolsel navigeerimisribal nuppu **Tööülesanded redigeerimine** ja seejärel klõpsake nuppu **Lisa** töö tööülesande lisamiseks. Nelja ülesanded lisada töökoha järgmine käsk read ja sätted.

    >[AZURE.NOTE]Töötab `source /openfoam/settings.sh` häälestab keskkondades OpenFOAM ja MPI runtime nii, et iga järgmist helistab see enne käsu OpenFOAM.

    *   **Ülesanne 1**. Käivitage **decomposePar** andmefailide **interDyMFoam** töötab samal ajal luua.
    
        *   Ühe sõlme tööülesande määramine

        *   **Käsurida** - `source /openfoam/settings.sh && decomposePar -force > /openfoam/decomposePar${CCP_JOBID}.log`
    
        *   **Töö kataloog** - / openfoam/sloshingTank3D
        
        Järgmisel joonisel näha. Konfigureerige sarnaselt ülejäänud tööülesanded.

        ![Ülesanne 1 üksikasjad][task_details1]

    *   **Ülesanne 2**. Käivitage **interDyMFoam** paralleelselt valimi arvutada.

        *   Kaks sõlme tööülesande määramine

        *   **Käsurida** - `source /openfoam/settings.sh && /openfoam/hpcimpirun.sh interDyMFoam -parallel > /openfoam/interDyMFoam${CCP_JOBID}.log`

        *   **Töö kataloog** - / openfoam/sloshingTank3D

    *   **Ülesanne 3**. Käivitage **reconstructPar** ühendada üheks aja kataloogide iga processor_N_ kataloogist kogumit.

        *   Ühe sõlme tööülesande määramine

        *   **Käsurida** - `source /openfoam/settings.sh && reconstructPar > /openfoam/reconstructPar${CCP_JOBID}.log`

        *   **Töö kataloog** - / openfoam/sloshingTank3D

    *   **Tööülesande 4**. Käivitage **foamToEnsight** paralleelselt OpenFOAM tulemi failide teisendamine EnSight vorming ja nimega Ensight juhul directory kataloogis EnSight failid paigutada.

        *   Kaks sõlme tööülesande määramine

        *   **Käsurida** - `source /openfoam/settings.sh && /openfoam/hpcimpirun.sh foamToEnsight -parallel > /openfoam/foamToEnsight${CCP_JOBID}.log`

        *   **Töö kataloog** - / openfoam/sloshingTank3D

6.  Järgmised toimingud tõusvas järjestuses tööülesande sõltuvused lisada.

    ![Ülesandesõltuvused][task_dependencies]

7.  Klõpsake nuppu **Edasta** selle töö käivitamiseks.

    Vaikimisi esitab HPC Pack töö praeguse sisselogitud kasutaja kontona. Kui klõpsate nuppu **Esita**, võidakse kuvada dialoogiboks, mis palub teil sisestada kasutajanimi ja parool.

    ![Töö identimisteave][creds]

    Teatud tingimustel HPC Pack jätab kasutaja teavet enne sisestusteade ja ei kuva seda dialoogiboksi. Veendumaks, et HPC Pack uuesti kuvada, sisestage käsuviibale järgmine käsk ja seejärel esitama töö.

    ```
    hpccred delcreds
    ```

8.  Töö võtab mitu tundi vastavalt parameetritele, mille olete valimi kümneid minutit. Intensiivsuskaart, näete töö töötab Linux sõlmed. 

    ![Intensiivsuskaardi][heat_map]

    Iga sõlme kaheksa protsessi käivitatud.

    ![Linux protsessid][linux_processes]

9.  Pärast töö lõppemist leiate C:\OpenFoam\sloshingTank3D ja logifailide veebisaidil C:\OpenFoam kaustad töö tulemusi.


## <a name="view-results-in-ensight"></a>EnSight tulemuste kuvamine

Soovi korral kasutada [EnSight](https://www.ceisoftware.com/) visualiseerida ja analüüsida OpenFOAM töö tulemused. Visualiseerimine ja animatsiooni EnSight kohta leiate lisateavet teemast seda [videot](http://www.ceisoftware.com/wp-content/uploads/screencasts/vof_visualization/vof_visualization.html).

1.  Pärast installimist EnSight pea sõlme, käivitage see.

2.  Avage C:\OpenFoam\sloshingTank3D\EnSight\sloshingTank3D.case.

    Näete tank vaaturis.

    ![Tank EnSight][tank]

3.  Luua ka **Isosurface** **internalMesh**, ja seejärel valige muutuv **alpha_water**.

    ![Mõne isosurface loomine][isosurface]

4.  Sea eelmises juhises loodud **Isosurface_part** värvi. Näiteks määraks vee sinine.

    ![Isosurface värvi redigeerimine][isosurface_color]

5.  Luua mõne **Iso-helitugevuse** **seinad** , valides **seinad** **osade** paanil ja klõpsake tööriistaribal nuppu **Isosurfaces** .

6.  Dialoogiboksis nimega **Isovolume** **tüübi** valimine ja 0,5 Min **Isovolume** vahemiku määramine. Funktsiooni isovolume, klõpsake nuppu **Loo koos valitud osade**.

7.  Sea eelmises juhises loodud **Iso_volume_part** värvi. Näiteks määrata, et süvavee sinine.

8.  Määrake **seinad**. Näiteks, määrata läbipaistva valge.

9. Nüüd klõpsake **esitamiseks** soovitud tulemused kuvamiseks.

    ![Tank tulem][tank_result]

## <a name="sample-files"></a>Näidisfailide

### <a name="sample-xml-configuration-file-for-cluster-deployment-by-powershell-script"></a>Valimi XML-i konfiguratsioonifail kobar juurutamiseks PowerShelli skripti abil

 ```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>allvhdsje</StorageAccount>
  </Subscription>
  <Location>Japan East</Location>  
  <VNet>
    <VNetName>suse12rdmavnet</VNetName>
    <SubnetName>SUSE12RDMACluster</SubnetName>
  </VNet>
  <Domain>
    <DCOption>HeadNodeAsDC</DCOption>
    <DomainFQDN>hpclab.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>SUSE12RDMA-HN</VMName>
    <ServiceName>suse12rdma-je</ServiceName>
    <VMSize>A8</VMSize>
    <EnableRESTAPI />
    <EnableWebPortal />
  </HeadNode>
  <LinuxComputeNodes>
    <VMNamePattern>SUSE12RDMA-LN%1%</VMNamePattern>
    <ServiceName>suse12rdma-je</ServiceName>
    <VMSize>A8</VMSize>
    <NodeCount>2</NodeCount>
      <ImageName>b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-hpc-v20150708</ImageName>
  </LinuxComputeNodes>
</IaaSClusterConfig>
```

### <a name="sample-credxml-file"></a>Näidisfaili cred.xml

```
<ExtendedData>
  <PrivateKey>-----BEGIN RSA PRIVATE KEY-----
MIIEpQIBAAKCAQEAxJKBABhnOsE9eneGHvsjdoXKooHUxpTHI1JVunAJkVmFy8JC
qFt1pV98QCtKEHTC6kQ7tj1UT2N6nx1EY9BBHpZacnXmknpKdX4Nu0cNlSphLpru
lscKPR3XVzkTwEF00OMiNJVknq8qXJF1T3lYx3rW5EnItn6C3nQm3gQPXP0ckYCF
Jdtu/6SSgzV9kaapctLGPNp1Vjf9KeDQMrJXsQNHxnQcfiICp21NiUCiXosDqJrR
AfzePdl0XwsNngouy8t0fPlNSngZvsx+kPGh/AKakKIYS0cO9W3FmdYNW8Xehzkc
VzrtJhU8x21hXGfSC7V0ZeD7dMeTL3tQCVxCmwIDAQABAoIBAQCve8Jh3Wc6koxZ
qh43xicwhdwSGyliZisoozYZDC/ebDb/Ydq0BYIPMiDwADVMX5AqJuPPmwyLGtm6
9hu5p46aycrQ5+QA299g6DlF+PZtNbowKuvX+rRvPxagrTmupkCswjglDUEYUHPW
05wQaNoSqtzwS9Y85M/b24FfLeyxK0n8zjKFErJaHdhVxI6cxw7RdVlSmM9UHmah
wTkW8HkblbOArilAHi6SlRTNZG4gTGeDzPb7fYZo3hzJyLbcaNfJscUuqnAJ+6pT
iY6NNp1E8PQgjvHe21yv3DRoVRM4egqQvNZgUbYAMUgr30T1UoxnUXwk2vqJMfg2
Nzw0ESGRAoGBAPkfXjjGfc4HryqPkdx0kjXs0bXC3js2g4IXItK9YUFeZzf+476y
OTMQg/8DUbqd5rLv7PITIAqpGs39pkfnyohPjOe2zZzeoyaXurYIPV98hhH880uH
ZUhOxJYnlqHGxGT7p2PmmnAlmY4TSJrp12VnuiQVVVsXWOGPqHx4S4f9AoGBAMn/
vuea7hsCgwIE25MJJ55FYCJodLkioQy6aGP4NgB89Azzg527WsQ6H5xhgVMKHWyu
Q1snp+q8LyzD0i1veEvWb8EYifsMyTIPXOUTwZgzaTTCeJNHdc4gw1U22vd7OBYy
nZCU7Tn8Pe6eIMNztnVduiv+2QHuiNPgN7M73/x3AoGBAOL0IcmFgy0EsR8MBq0Z
ge4gnniBXCYDptEINNBaeVStJUnNKzwab6PGwwm6w2VI3thbXbi3lbRAlMve7fKK
B2ghWNPsJOtppKbPCek2Hnt0HUwb7qX7Zlj2cX/99uvRAjChVsDbYA0VJAxcIwQG
TxXx5pFi4g0HexCa6LrkeKMdAoGAcvRIACX7OwPC6nM5QgQDt95jRzGKu5EpdcTf
g4TNtplliblLPYhRrzokoyoaHteyxxak3ktDFCLj9eW6xoCZRQ9Tqd/9JhGwrfxw
MS19DtCzHoNNewM/135tqyD8m7pTwM4tPQqDtmwGErWKj7BaNZARUlhFxwOoemsv
R6DbZyECgYEAhjL2N3Pc+WW+8x2bbIBN3rJcMjBBIivB62AwgYZnA2D5wk5o0DKD
eesGSKS5l22ZMXJNShgzPKmv3HpH22CSVpO0sNZ6R+iG8a3oq4QkU61MT1CfGoMI
a8lxTKnZCsRXU1HexqZs+DSc+30tz50bNqLdido/l5B4EJnQP03ciO0=
-----END RSA PRIVATE KEY-----</PrivateKey>
  <PublicKey>ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDEkoEAGGc6wT16d4Ye+yN2hcqigdTGlMcjUlW6cAmRWYXLwkKoW3WlX3xAK0oQdMLqRDu2PVRPY3qfHURj0EEellpydeaSekp1fg27Rw2VKmEumu6Wxwo9HddXORPAQXTQ4yI0lWSerypckXVPeVjHetbkSci2foLedCbeBA9c/RyRgIUl227/pJKDNX2Rpqly0sY82nVWN/0p4NAyslexA0fGdBx+IgKnbU2JQKJeiwOomtEB/N492XRfCw2eCi7Ly3R8+U1KeBm+zH6Q8aH8ApqQohhLRw71bcWZ1g1bxd6HORxXOu0mFTzHbWFcZ9ILtXRl4Pt0x5Mve1AJXEKb username@servername;</PublicKey>
</ExtendedData>
```
### <a name="sample-silentcfg-file-to-install-mpi"></a>Silent.cfg näidisfaili MPI installimiseks

```
# Patterns used to check silent configuration file
#
# anythingpat - any string
# filepat     - the file location pattern (/file/location/to/license.lic)
# lspat       - the license server address pattern (0123@hostname)
# snpat       - the serial number pattern (ABCD-01234567)

# accept EULA, valid values are: {accept, decline}
ACCEPT_EULA=accept

# optional error behavior, valid values are: {yes, no}
CONTINUE_WITH_OPTIONAL_ERROR=yes

# install location, valid values are: {/opt/intel, filepat}
PSET_INSTALL_DIR=/opt/intel

# continue with overwrite of existing installation directory, valid values are: {yes, no}
CONTINUE_WITH_INSTALLDIR_OVERWRITE=yes

# list of components to install, valid values are: {ALL, DEFAULTS, anythingpat}
COMPONENTS=DEFAULTS

# installation mode, valid values are: {install, modify, repair, uninstall}
PSET_MODE=install

# directory for non-RPM database, valid values are: {filepat}
#NONRPM_DB_DIR=filepat

# Serial number, valid values are: {snpat}
#ACTIVATION_SERIAL_NUMBER=snpat

# License file or license server, valid values are: {lspat, filepat}
#ACTIVATION_LICENSE_FILE=

# Activation type, valid values are: {exist_lic, license_server, license_file, trial_lic, serial_number}
ACTIVATION_TYPE=trial_lic

# Path to the cluster description file, valid values are: {filepat}
#CLUSTER_INSTALL_MACHINES_FILE=filepat

# Intel(R) Software Improvement Program opt-in, valid values are: {yes, no}
PHONEHOME_SEND_USAGE_DATA=no

# Perform validation of digital signatures of RPM files, valid values are: {yes, no}
SIGNING_ENABLED=yes

# Select yes to enable mpi-selector integration, valid values are: {yes, no}
ENVIRONMENT_REG_MPI_ENV=no

# Select yes to update ld.so.conf, valid values are: {yes, no}
ENVIRONMENT_LD_SO_CONF=no


```

### <a name="sample-settingssh-script"></a>Skripti näide settings.sh

```
#!/bin/bash

# impi
source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh
export MPI_ROOT=$I_MPI_ROOT
export I_MPI_FABRICS=shm:dapl
export I_MPI_DAPL_PROVIDER=ofa-v2-ib0
export I_MPI_DYNAMIC_CONNECTION=0

# openfoam
export FOAM_INST_DIR=/opt/OpenFOAM
source /opt/OpenFOAM/OpenFOAM-2.3.1/etc/bashrc
export WM_MPLIB=INTELMPI
```


###<a name="sample-hpcimpirunsh-script"></a>Skripti näide hpcimpirun.sh

```
#!/bin/bash

# The path of this script
SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"

# Set mpirun runtime evironment
source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh
export MPI_ROOT=$I_MPI_ROOT
export I_MPI_FABRICS=shm:dapl
export I_MPI_DAPL_PROVIDER=ofa-v2-ib0
export I_MPI_DYNAMIC_CONNECTION=0

# mpirun command
MPIRUN=mpirun
# Argument of "--hostfile"
NODELIST_OPT="--hostfile"
# Argument of "-np"
NUMPROCESS_OPT="-np"

# Get node information from ENVs
NODESCORES=(${CCP_NODES_CORES})
COUNT=${#NODESCORES[@]}

if [ ${COUNT} -eq 0 ]
then
    # CCP_NODES_CORES is not found or is empty, just run the mpirun without hostfile arg.
    ${MPIRUN} $*
else
    # Create the hostfile file
    NODELIST_PATH=${SCRIPT_PATH}/hostfile_$$

    # Get every node name and write into the hostfile file
    I=1
    while [ ${I} -lt ${COUNT} ]
    do
        echo "${NODESCORES[${I}]}" >> ${NODELIST_PATH}
        let "I=${I}+2"
    done

    # Run the mpirun with hostfile arg
    ${MPIRUN} ${NUMPROCESS_OPT} ${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*

    RTNSTS=$?
    rm -f ${NODELIST_PATH}
fi

exit ${RTNSTS}

```





<!--Image references-->
[keygen]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/keygen.png
[keys]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/keys.png
[step_variables]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/step_variables.png
[data_files]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/data_files.png
[decompose]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/decompose.png
[job_details]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/job_details.png
[job_resources]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/job_resources.png
[task_details1]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/task_details1.png
[task_dependencies]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/task_dependencies.png
[creds]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/creds.png
[heat_map]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/heat_map.png
[tank]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/tank.png
[tank_result]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/tank_result.png
[isosurface]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/isosurface.png
[isosurface_color]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/isosurface_color.png
[linux_processes]: ./media/virtual-machines-linux-classic-hpcpack-cluster-openfoam/linux_processes.png
