<properties
 pageTitle="NAMD koos Microsoft HPC Pack Linux VMs | Microsoft Azure'i"
 description="Microsoft HPC Pack kobar Azure juurutada ja käivitage NAMD simulatsiooni charmrun mitme Linuxi Arvuta sõlmed"
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
 ms.date="10/13/2016"
 ms.author="danlep"/>

# <a name="run-namd-with-microsoft-hpc-pack-on-linux-compute-nodes-in-azure"></a>Käivitage Microsoft HPC Pack Linux NAMD arvutada Azure sõlmed

Selles artiklis kirjeldatakse, üks viis, kuidas käivitada Azure'i virtuaalmasinates Linux suure jõudlusega arvutuste (HPC) töökoormus. Siin saate häälestada [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) kobar Azure koos Linux Arvuta sõlmed ja käivitage [NAMD](http://www.ks.uiuc.edu/Research/namd/) simulatsiooni arvutamine ja visualiseerida suurte molekulaarbioloogiliste süsteemi struktuuri.  

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

* **NAMD** (nanotasandil molekulaarne Dynamics programmi) on loodud suure jõudlusega modelleerimiseks sisaldavad kuni miljoneid aatomite suur molekulaarbioloogiliste süsteemid paralleelselt molekulaarne dynamics paketi. Need süsteemid näiteks viiruste, lahtri struktuurid ja suure valgud. NAMD skaala sadu valdkond jaoks tüüpiline simulatsioonid ja rohkem kui 500 000 südamikud suurim simulatsioonid.

* **Microsoft HPC Pack** pakub rühmades kohapealse arvutite või Azure'i virtuaalmasinates suuremahuliste HPC ja paralleelsete rakenduste töötamine. Nüüd toetab töötab Linux HPC rakenduste Linux Windows HPC töökoormus HPC Pack lahenduse algselt välja arvutada sõlm VMs juurutatud on HPC Pack kobar. Vaadake [Alustamine Linux Arvuta sõlmed on HPC Pack klaster Azure](virtual-machines-linux-classic-hpcpack-cluster.md) tutvustus.

Käivitamiseks Linux HPC töökoormus Azure'i muude suvandite kasutamiseks leiate teemast [tehnilised ressursid paketi ja suure jõudlusega arvutuste](../batch/batch-hpc-solutions.md).




## <a name="prerequisites"></a>Eeltingimused

* **HPC Pack kobar Linux arvutada sõlmed** - HPC Pack kobar koos Linux Arvuta sõlmed mõni [Azure ressursihaldur malli](https://azure.microsoft.com/marketplace/partners/microsofthpc/newclusterlinuxcn/) või on [Azure PowerShelli skripti](virtual-machines-linux-classic-hpcpack-cluster-powershell-script.md)Azure juurutamine. Vaadake [Alustamine Linux Arvuta sõlmed on HPC Pack klaster Azure](virtual-machines-linux-classic-hpcpack-cluster.md) eeltingimused ja kas variant juhised. Kui valite suvandi PowerShelli skripti juurutamine, vt käesoleva artikli lõpus näidisfailide konfiguratsiooni näidisfaili. Selle faili konfigureerib Azure'i HPC Pack kobar, mis koosneb Windows Server 2012 R2 pea sõlme ja neli suurus suur CentOS 6.6 Arvuta sõlmed. Keskkonna vajaduste järgi kohandada seda faili.


* **NAMD tarkvara ja õpetuse failide** - alla laadida NAMD tarkvara Linux [NAMD](http://www.ks.uiuc.edu/Research/namd/) saidilt (vaja registreerida). Selles artiklis põhineb NAMD versioon 2.10 ja kasutab [Linux-x86_64 (64-bitine Inteli/AMD koos Ethernet)](http://www.ks.uiuc.edu/Development/Download/download.cgi?UserID=&AccessCode=&ArchiveID=1310) arhiivi. [NAMD kuueosalisest faile](http://www.ks.uiuc.edu/Training/Tutorials/#namd)alla laadida. Failid on skaneeritud failid ja teil on vaja Windows tööriista kobar pea sõlme failid ekstraktida. Ekstrakti failid, järgige selle artikli juhiseid. 

* **VMD** (valikuline) – NAMD tööpäevad tulemite kuvamiseks alla laadida ja molekulaarne visualiseeringu programmi [VMD](http://www.ks.uiuc.edu/Research/vmd/) teie valitud arvutisse installida. Praegune versioon on 1.9.2. Lugege teemat VMD, allalaadimissait alustada.  


## <a name="set-up-mutual-trust-between-compute-nodes"></a>Vastastikune usaldus vahel Arvuta sõlmed häälestamine
Rist-sõlm töö mitme Linuxi sõlmed nõuab sõlmed usaldusväärsuse üksteisest ( **rsh** või **ssh**) alusel. HPC Pack kobar loomisel Microsoft HPC Pack IaaS juurutamise skripti skripti automaatselt häälestab püsivate vastastikune usaldus teie määratud administraatori konto. Administraator kasutajate loodud soovitud klaster domeeni, tuleb luua ajutine vastastikune usaldus sõlme vahel, kui neile on määratud tööd. Hävitada seose siis, kui töö on lõpule jõudnud. Sisestage teha iga kasutaja jaoks, mis HPC Pack kasutab usalda seost klaster RSA paari. Järgige juhiseid.

### <a name="generate-an-rsa-key-pair"></a>Luua mõne RSA paari
See on lihtne RSA võti paari, mis sisaldab avalik võti ja privaatvõti, Linux **ssh-keygen** käsu loomiseks.

1.  Logige arvutisse Linux.

2.  Käivitage järgmine käsk:

    ```bash
    ssh-keygen -t rsa
    ```

    >[AZURE.NOTE] Vajutage klahvi **Enter** kasutada vaikesätteid käsu lõpetamiseni. Sisestage parool siin; Kui küsitakse parooli, vajutage lihtsalt sisestusklahvi **Enter**.

    ![Luua mõne RSA paari][keygen]

3.  Muuda kataloogi ~/.ssh kataloog. Privaatvõti on talletatud id_rsa ja id_rsa.pub avalik võti.

    ![Era- ja võtmed][keys]

### <a name="add-the-key-pair-to-the-hpc-pack-cluster"></a>Võti paari lisamine HPC Pack kobar
1.  [Kaugtöölaua ühendus](virtual-machines-windows-connect-logon.md) pea sõlme VM abil domeeni mandaat, tingimusel, kui olete juurutanud kobar (nt hpc\clusteradmin). Saate hallata klaster pea sõlme.

2. Kasutage funktsiooni kobar Active Directory domeeni domeeni kasutajakonto loomiseks standard Windows Server toiminguid. Näiteks kasutage pea sõlme tööriista Active Directory kasutaja ja arvutites. Selle artikli näited Oletagem, et luua domeeni kasutaja nimega hpcuser hpclab Domeen (hpclab\hpcuser).

3. Kasutaja domeeni lisada HPC Pack kobar kobar kasutajana. Lisateabe saamiseks vaadake [Lisa või Eemalda klaster kasutajad](https://technet.microsoft.com/library/ff919330.aspx).

2.  Looge fail nimega C:\cred.xml ja kopeerige see RSA olulisi andmeid. Ühe näite leiate näidisfailide käesoleva artikli lõpus.

    ```
    <ExtendedData>
        <PrivateKey>Copy the contents of private key here</PrivateKey>
        <PublicKey>Copy the contents of public key here</PublicKey>
    </ExtendedData>
    ```

3.  Avage käsuviip ja sisestage järgmine käsk hpclab\hpcuser konto identimisteave määrata. **Extendeddata** parameetri abil võtme andmete jaoks loodud C:\cred.xml faili nime.

    ```command
    hpccred setcreds /extendeddata:c:\cred.xml /user:hpclab\hpcuser /password:<UserPassword>
    ```

    See käsk on lõpule jõudnud edukalt ilma väljundi. Pärast mandaadi kasutajakonto, peate käivitama töö seadmisele turvalises asukohas cred.xml faili salvestada või kustutada.

5.  Kui teie loodud RSA võti paari ühele oma Linux sõlmed, ärge unustage võtmed kustutada, kui olete lõpetanud, kasutades neid. HPC Pack häälestada vastastikune usaldus, kui see leiab id_rsa olemasoleva faili või id_rsa.pub fail.

>[AZURE.IMPORTANT] Me ei soovita töötab Linux töö administraatorina kobar ühiskasutusega kobar, kuna administraatori poolt töö töötab Linux sõlmed root konto. Administraator kasutaja poolt töö töötab kohaliku Linux kasutajakonto kasutajana töö sama nimega. Sel juhul HPC Pack häälestab vastastikune usaldus selle Linuxi kasutaja jaoks kõik projektile eraldatud sõlmed üle. Saate häälestada Linuxi kasutaja käsitsi Linuxi sõlmed enne töö või HPC Pack luuakse kasutajale automaatselt töö esitamisel. Kui HPC Pack loob kasutaja, HPC Pack kustutab selle pärast töö lõpulejõudmist. Ohtu turvalisusele vähendamiseks võtmed eemaldatakse pärast töö lõpulejõudmist sõlmed.

## <a name="set-up-a-file-share-for-linux-nodes"></a>Faili ühiskasutus Linux sõlmed häälestamine

Nüüd SMB osa häälestamine ja ühiskausta seinale kõik Linuxi sõlmed lubamiseks Linux sõlmed levinud tee NAMD failidele juurde pääseda. Järgmised juhised pea sõlme ühiskausta ühendada. Osa on soovitatav jaotuse CentOS 6.6 nagu praegu ei toeta Azure'i teenus. Kui teie Linux sõlmed toetavad on Azure failikettal, vaadake, [Kuidas kasutada Azure failide talletamine Linux](../storage/storage-how-to-use-files-linux.md). Faili ühiskasutuse suvandid HPC Pack, vt [Linux Arvuta sõlmed on HPC Pack klaster Azure alustamine](virtual-machines-linux-classic-hpcpack-cluster.md).

1.  Looge kaust pea sõlme ja ühiskasutusse anda kõigile seadmisega lugemis-ja kirjutamisõigusega õigusi. Selles näites \\ \\CentOS66HN\Namd on kaust, kuhu CentOS66HN on pea sõlme hosti nimi nimi.

2. Nimega namd2 ühiskasutusega kaustas alamkausta loomine. Looge namd2, teise nimega namdsample alamkausta.

3. Väljavõte NAMD failid kausta skaneeritud Arhiiv **tõrva** või mõne muu Windows kasuliku, mis töötab Windowsi versiooni kaudu. 
    * Väljavõte NAMD tar Arhiiv \\ \\CentOS66HN\Namd\namd2.
    
    * Jaotises kuueosalisest failid ekstraktida \\ \\CentOS66HN\Namd\namd2\namdsample.

4. Avage Windows PowerShelli aken ja käivitage järgmised käsud Linux sõlmed ühiskausta ühendada.

    ```command
    clusrun /nodegroup:LinuxNodes mkdir -p /namd2

    clusrun /nodegroup:LinuxNodes mount -t cifs //CentOS66HN/Namd/namd2 /namd2 -o vers=2.1`,username=<username>`,password='<password>'`,dir_mode=0777`,file_mode=0777
    ```

Esimese käsu loob kausta nimega /namd2 kõik sõlmed LinuxNodes jaotises. Teine käsk kinnitused ühiskausta //CentOS66HN/Namd/namd2 dir_mode kausta peale ja file_mode bittide seatud 777. *Kasutajanime* ja *parooli* käsk peaks olema pea sõlme kasutaja mandaat.

>[AZURE.NOTE]Funktsiooni "\`" sümbol, teine käsk on escape sümbol PowerShelli jaoks. "\`," tähendab "," (koma) on osa käsk.


## <a name="create-a-bash-script-to-run-a-namd-job"></a>Bash skripti NAMD töö loomine

Oma töö NAMD peab *nodelist* faili **charmrun** sõlmed alustamiseks NAMD protsesside arvu määramiseks. Saate kasutada Bash skripti, mis loob nodelist faili ja **charmrun** selle nodelist failiga töötab. Seejärel saate esitada NAMD töö HPC halduris mis nõuab see skript.

Teie valitud tekstiredaktoris kasutamisel loomine Bash skripti /namd2 kaust, mis sisaldab NAMD Programmifailid ja pange sellele hpccharmrun.sh. Kiirülevaate tõendada mõiste, kopeerige käesoleva artikli lõpus on esitatud näide hpccharmrun.sh skripti ja valige [Edasta NAMD töö](#submit-a-namd-job).

>[AZURE.TIP] Salvesta tekstifaili Linux skripti rida lõpud (ainult Reavahetus, mitte CR LF). See tagab, et see töötab õigesti sõlmed Linux.

Järgnevalt on üksikasjad, mida tähendab see bash skript. 

1.  Määratleda mõned muutujad.

    ```bash
    #!/bin/bash

    # The path of this script
    SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"
    # Charmrun command
    CHARMRUN=${SCRIPT_PATH}/charmrun
    # Argument of ++nodelist
    NODELIST_OPT="++nodelist"
    # Argument of ++p
    NUMPROCESS="+p"
    ```

2.  Keskkonna muutujate sõlme teavet saada. $NODESCORES talletab $CCP_NODES_CORES tükeldatud sõnade loendit. $COUNT on $NODESCORES suurust.
    ```
    # Get node information from the environment variables
    NODESCORES=(${CCP_NODES_CORES})
    COUNT=${#NODESCORES[@]}
    ```    
    
    $CCP_NODES_CORES muutuja vorming on järgmine:

    ```
    <Number of nodes> <Name of node1> <Cores of node1> <Name of node2> <Cores of node2>…
    ```

    Muutuja on loetletud sõlmed, sõlm nimed ja arv iga sõlme valdkond, mis on eraldatud töö koguarv. Näiteks kui töö peab 10 südamikud käivitamiseks, $CCP_NODES_CORES väärtus on sarnane:

    ```
    3 CENTOS66LN-00 4 CENTOS66LN-01 4 CENTOS66LN-03 2
    ```
        
3.  Kui $CCP_NODES_CORES muutuja pole määratud, siis alustage otse **charmrun** . (See peaks ainult ilmneda juhul, kui käivitate selle skripti otse oma Linux sõlmed.)

    ```
    if [ ${COUNT} -eq 0 ]
    then
        # CCP_NODES is_CORES is not found or is empty, so just run charmrun without nodelist arg.
        #echo ${CHARMRUN} $*
        ${CHARMRUN} $*
    ```

4.  Või **charmrun**nodelist faili.

    ```
    else
        # Create the nodelist file
        NODELIST_PATH=${SCRIPT_PATH}/nodelist_$$

        # Write the head line
        echo "group main" > ${NODELIST_PATH}

        # Get every node name and number of cores and write into the nodelist file
        I=1
        while [ ${I} -lt ${COUNT} ]
        do
            echo "host ${NODESCORES[${I}]} ++cpus ${NODESCORES[$(($I+1))]}" >> ${NODELIST_PATH}
            let "I=${I}+2"
        done
```
5.  Käivitage **charmrun** nodelist faili, saada selle saatja olek ja eemaldada nodelist faili lõpus.

    ${CCP_NUMCPUS} on teise muutuja määratud HPC Pack pea sõlme. See talletab kokku valdkond eraldada selle töö arv. Kasutame määramiseks protsesside jaoks charmrun arv.

    ```
    # Run charmrun with nodelist arg
    #echo ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*
    ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*

    RTNSTS=$?
    rm -f ${NODELIST_PATH}
    fi

    ```
6.  Väljuge olekuga **charmrun** saatja.

    ```
    exit ${RTNSTS}
    ```



Nodelist faili, mis skripti genereeritud andmed on:

```
group main
host <Name of node1> ++cpus <Cores of node1>
host <Name of node2> ++cpus <Cores of node2>
…
```

Näiteks:

```
group main
host CENTOS66LN-00 ++cpus 4
host CENTOS66LN-01 ++cpus 4
host CENTOS66LN-03 ++cpus 2
```

## <a name="submit-a-namd-job"></a>Esitage NAMD töö

Nüüd olete valmis töö NAMD HPC halduris edastada.

1.  Ühenduse loomine oma kobar pea sõlme ja HPC halduri käivitamine.

2.  **Ressursside haldamine**, veenduge, et Linux Arvuta sõlmed oleks **Online'i** olekus. Kui pole, valige need ja klõpsake nuppu **Too veebis**.

2.  **Töö haldus**nuppu **Uus töökoht**.

3.  Sisestage nimi, nt *hpccharmrun*töö.

    ![Uue HPC töö][namd_job]

4.  **Projekti üksikasjade** lehel jaotises **Töö ressursid**valige **sõlm** nimega ressursside ja **miinimum** väärtuseks 3. , saame töö kolm Linuxi sõlmed ja iga sõlm on neli protsessorituuma.

    ![Töö ressursid][job_resources]

5. **Tööülesannete redigeerimine** vasakpoolsel navigeerimisribal nuppu ja seejärel klõpsake nuppu **Lisa** töö tööülesande lisamiseks.    


6. Klõpsake lehel **tööülesande üksikasjad ja i/o ümbersuunamine** määrata järgmised väärtused.

    * **Käsurida** -
`/namd2/hpccharmrun.sh ++remote-shell ssh /namd2/namd2 /namd2/namdsample/1-2-sphere/ubq_ws_eq.conf > /namd2/namd2_hpccharmrun.log`

        >[AZURE.TIP] Eelmise käsurida on ühe käsu ilma reapiire. See murtakse kuvada mitmel real jaotises **käsurea**.

    * **Töö kataloog** - /namd2

    * **Miinimum** - 3

    ![Tööülesande üksikasjad][task_details]

    >[AZURE.NOTE] Määrake töötamise kataloog siin **charmrun** püüab liikuge samas töötamise kataloog iga sõlme. Kui töötamise kataloogi pole määratud, HPC Pack käivitab käsu loodud üks Linux sõlmed CEIP nimega kaust. See põhjustab muude sõlmed järgmine tõrketeade: `/bin/bash: line 37: cd: /tmp/nodemanager_task_94_0.mFlQSN: No such file or directory.` selle probleemi vältimiseks määrake selle kausta tee, mida saab kasutada kõik sõlmed töötavat kausta.

5.  Klõpsake nuppu **OK** ja seejärel klõpsake nuppu **Edasta** selle töö käivitamiseks.

    Vaikimisi esitab HPC Pack töö praeguse sisselogitud kasutaja kontona. Dialoogiboksi paluda teil sisestada kasutajanimi ja parool, kui klõpsate nuppu **Edasta**.

    ![Töö identimisteave][creds]

    Teatud tingimustel HPC Pack jätab kasutaja teavet enne sisestusteade ja ei kuva seda dialoogiboksi. Veendumaks, et HPC Pack uuesti kuvada, sisestage käsuviibale järgmine käsk ja seejärel esitama töö.

    ```command
    hpccred delcreds
    ```

6.  Töö võtab mitu minutit lõpuni.

7.  Otsimine veebisaidil Logi töö \\ <headnodeName>\Namd\namd2\namd2_hpccharmrun.log ja väljundi failid \\ <headnodeName>\Namd\namd2\namdsample\1-2-sphere\.

8.  Soovi korral saate alustada oma töö tulemuste vaatamiseks VMD. Juhised selle NAMD visualiseerimine väljund (praegusel juhul Ubikitiini valgu molekul vee valdkonnas) failid on selles artiklis ei käsitleta. Üksikasjad leiate [NAMD õpetuse](http://www.life.illinois.edu/emad/biop590c/namd-tutorial-unix-590C.pdf) .

    ![Töö tulemused][vmd_view]

## <a name="sample-files"></a>Näidisfailide

### <a name="sample-xml-configuration-file-for-cluster-deployment-by-powershell-script"></a>Valimi XML-i konfiguratsioonifail kobar juurutamiseks PowerShelli skripti abil

```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
      <Location>West US</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>HeadNodeAsDC</DCOption>
    <DomainFQDN>hpclab.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>CentOS66HN</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>Large</VMSize>
    <EnableRESTAPI />
    <EnableWebPortal />
  </HeadNode>
  <LinuxComputeNodes>
    <VMNamePattern>CentOS66LN-%00%</VMNamePattern>
    <ServiceName>MyLnxCNService</ServiceName>
     <VMSize>Large</VMSize>
     <NodeCount>4</NodeCount>
    <ImageName>5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-66-20150325</ImageName>
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

### <a name="sample-hpccharmrunsh-script"></a>Skripti näide hpccharmrun.sh

```
#!/bin/bash

# The path of this script
SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"
# Charmrun command
CHARMRUN=${SCRIPT_PATH}/charmrun
# Argument of ++nodelist
NODELIST_OPT="++nodelist"
# Argument of ++p
NUMPROCESS="+p"

# Get node information from ENVs
# CCP_NODES_CORES=3 CENTOS66LN-00 4 CENTOS66LN-01 4 CENTOS66LN-03 4
NODESCORES=(${CCP_NODES_CORES})
COUNT=${#NODESCORES[@]}

if [ ${COUNT} -eq 0 ]
then
    # If CCP_NODES_CORES is not found or is empty, just run the charmrun without nodelist arg.
    #echo ${CHARMRUN} $*
    ${CHARMRUN} $*
else
    # Create the nodelist file
    NODELIST_PATH=${SCRIPT_PATH}/nodelist_$$

    # Write the head line
    echo "group main" > ${NODELIST_PATH}

    # Get every node name & cores and write into the nodelist file
    I=1
    while [ ${I} -lt ${COUNT} ]
    do
        echo "host ${NODESCORES[${I}]} ++cpus ${NODESCORES[$(($I+1))]}" >> ${NODELIST_PATH}
        let "I=${I}+2"
    done

    # Run the charmrun with nodelist arg
    #echo ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*
    ${CHARMRUN} ${NUMPROCESS}${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*

    RTNSTS=$?
    rm -f ${NODELIST_PATH}
fi

exit ${RTNSTS}
```

 





<!--Image references-->
[keygen]: ./media/virtual-machines-linux-classic-hpcpack-cluster-namd/keygen.png
[keys]: ./media/virtual-machines-linux-classic-hpcpack-cluster-namd/keys.png
[namd_job]: ./media/virtual-machines-linux-classic-hpcpack-cluster-namd/namd_job.png
[job_resources]: ./media/virtual-machines-linux-classic-hpcpack-cluster-namd/job_resources.png
[creds]: ./media/virtual-machines-linux-classic-hpcpack-cluster-namd/creds.png
[task_details]: ./media/virtual-machines-linux-classic-hpcpack-cluster-namd/task_details.png
[vmd_view]: ./media/virtual-machines-linux-classic-hpcpack-cluster-namd/vmd_view.png
