<properties
    pageTitle="Paljundada Hyper-V virtuaalmasinates VMM pilved PowerShelli (ressursihaldur) kaudu VMM saidi | Microsoft Azure'i"
    description="Kirjeldab, kuidas juurutada Azure saidi taastamist korraldab dispersioonanalüüs, Tõrkesiirde ja taastamise Hyper-V vms VMM pilved VMM saidi PowerShelli (ressursihaldur) kaudu"
    services="site-recovery"
    documentationCenter=""
    authors="sujaytalasila"
    manager="rochakm"
    editor="raynew"/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/19/2016"
    ms.author="sutalasi"/>

# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-a-secondary-vmm-site-using-powershell-resource-manager"></a>Paljundada Hyper-V virtuaalmasinates VMM pilved VMM saidi PowerShelli (ressursihaldur) kaudu

> [AZURE.SELECTOR]
- [Azure'i portaal](site-recovery-vmm-to-vmm.md)
- [Klassikaline portaal](site-recovery-vmm-to-vmm-classic.md)
- [PowerShelli - ressursihaldur](site-recovery-vmm-to-vmm-powershell-resource-manager.md)

Tere tulemast Azure'i saidi taastamine! Selles artiklis, kui soovite ise kohapealse Hyper-V virtuaalmasinates hallatava System Center virtuaalse masina Manager (VMM) pilved saidi kasutamine. 

Selles artiklis kirjeldatakse, kuidas PowerShelli abil automatiseerida levinud toiminguid, mida peate tegema, kui häälestate Azure saidi taastamise paljundada Hyper-V virtuaalmasinates süsteem Center VMM pilved System Center VMM pilved saidi.

See artikkel sisaldab stsenaarium eeltingimuste ja näidatakse 

- Kuidas häälestada taastamise teenuste hoidla
- Andmeallika VMM serveri ja target VMM Azure saidi taastamise pakkuja installimine
- VMM Server autoriloomingut register
- Dispersioonanalüüs poliitika konfigureerimine VMM pilve. Poliitika dispersioonanalüüs sätted rakenduvad kõik kaitstud virtuaalmasinates 
- Luba selle virtuaalmasinates kaitse. 
- Testige Tõrkesiirde VMs iseseisvalt või taastamise kava veendumaks, et kõik töötab ootuspäraselt.
- Planeeritud või planeerimata Tõrkesiirde VMs teha iseseisvalt või taastamise kava veendumaks, et kõik töötab ootuspäraselt.

Kui tekib probleeme häälestamise sel juhul, postitada oma küsimusi [Azure taastamise teenused Foorum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

> [AZURE.NOTE] Azure'i on kaks eri [juurutamise mudelite](../resource-manager-deployment-model.md) loomise ja ressursside töötamine: Azure'i ressursihaldur ja klassikaline. Azure'i on kaks portaalide – Azure klassikaline portaali, mis toetab klassikaline juurutamise mudeli ja Azure portaali nii juurutamise mudelid tugi. Selles artiklis antakse ülevaade ressursihaldur juurutamise mudel.



## <a name="on-premises-prerequisites"></a>Kohapealse eeltingimused

Siin on, mida peate esmaseid ja teiseseid kohapealse saitidel juurutada selle stsenaariumi:

**Eeltingimused** | **Üksikasjad** 
--- | ---
**VMM** | Soovitame juurutate VMM esmane saidi serveri ja saidi VMM server.<br/><br/> Samuti saate [ühe VMM server parkimise vahel](site-recovery-single-vmm.md). Selle tegemiseks peate olema vähemalt kahe pilve VMM-i server on konfigureeritud.<br/><br/> VMM serverid olema vähemalt opsüsteemi System Center 2012 SP1 uusimaid värskendusi.<br/><br/> Iga VMM server peab olema üks või pilve konfigureeritud ja kõik pilved peab olema Hyper-V võimsuse reeglite seadmine. <br/><br/>Pilved peab sisaldama vähemalt ühte VMM hosti rühmad.<br/><br/>VMM pilved [konfigureerimise VMM cloud struktuuri](https://msdn.microsoft.com/library/azure/dn469075.aspx#BKMK_Fabric), häälestamise kohta lisateabe saamiseks ja [tutvustust: loomine privaatne pilved System Center 2012 SP1 VMM](http://blogs.technet.com/b/keithmayer/archive/2013/04/18/walkthrough-creating-private-clouds-with-system-center-2012-sp1-virtual-machine-manager-build-your-private-cloud-in-a-month.aspx).<br/><br/> VMM serverid peaks olema Interneti-ühendus. 
**Hyper-V** | Hyper-V serverite peab olema vähemalt opsüsteemi Windows Server 2012 Hyper-V rolliga ja olete installinud uusimaid värskendusi.<br/><br/> Hyper-V server peaks sisaldama vähemalt ühte VMs.<br/><br/>  Hyper-V hosti serverid peaks asuma hosti rühmad põhi- ja VMM pilved.<br/><br/> Kui kasutate Hyper-V klaster Windows Server 2012 R2 peaksite installima [2961977 värskendamine](https://support.microsoft.com/kb/2961977)<br/><br/> Kui teil on Hyper-V klaster Windows Server 2012 tähele selle kobar ta ei luuakse automaatselt, kui teil on staatiline IP address-põhiste kobar. Peate kobar ta käsitsi konfigureerimine. [Lisateavet vt](http://social.technet.microsoft.com/wiki/contents/articles/18792.configure-replica-broker-role-cluster-to-cluster-replication.aspx).
**Pakkuja** | Saidi taastamine juurutamisel installida Azure saidi taastamise pakkuja VMM serverites. Pakkuja suhtleb saidi taastamine HTTPS 443 korraldab kopeerimine. Andmete kopeerimine toimub esmaseid ja teiseseid Hyper-V serverite üle LAN või VPN-ühendus.<br/><br/> Pakkuja serveris VMM vajab juurdepääsu idest: *. hypervrecoverymanager.windowsazure.com; *. accesscontrol.Windows.net; *. backup.windowsazure.com; *. blob.Core.Windows.net; *. store.core.windows.net.<br/><br/> Lisaks võimaldada tulemüüri teatist VMM serverid [Azure andmekeskuse IP-aadresside vahemikud](https://www.microsoft.com/download/confirmation.aspx?id=41653) ja luba (443) HTTPS-protokolli.

### <a name="network-mapping-prerequisites"></a>Võrgu vastenduse eeltingimused
Võrgu vastenduse kaardid vahel VMM VM esmaseid ja teiseseid VMM serverites, et:

- Optimaalselt asetage koopia VMs teisene Hyper-V hosts pärast Tõrkesiirde.
- Ühenduse vastav VM võrkude koopia VMs.
- Kui konfigureerite võrgu vastendamise koopia VMs ei ühendada mis tahes võrk pärast Tõrkesiirde.
- Kui soovite häälestada võrku kaardistamine saidi taastamisel juurutamise siin on, mida on vaja:

    - Veenduge, et VMs allikas Hyper-V hosti server on ühendatud võrku VMM VM. Selle võrgu peaks olema seotud loogilise võrgu, mis on seotud pilve.
    - Veenduge, et teisene pilve taastamise kasutamiseks on konfigureeritud vastavate VM võrk. Et VM võrgu seotud loogika võrk, mis on seotud teisene pilveteenuses.


VMM võrkude konfigureerimise kohta lisateabe saamiseks selle artikli allpool

- [Kuidas konfigureerida VMM loogilised võrgud](http://go.microsoft.com/fwlink/p/?LinkId=386307)
- [Kuidas seadistada VM võrgu ja lüüside VMM](http://go.microsoft.com/fwlink/p/?LinkId=386308)

[Lisateavet leiate teemast](site-recovery-network-mapping.md) võrgu kaardistamine tööpõhimõtete kohta.

###<a name="powershell-prerequisites"></a>PowerShelli eeltingimused
Veenduge, et teil on Azure PowerShelli kasutamiseks valmis. Kui kasutate juba PowerShelli, peate varasema versiooni täiendamine versiooniks versioon 0.8.10 või uuem versioon. PowerShelli häälestamise kohta leiate artiklist [juhend installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md). Kui teil on häälestamine ja PowerShelli konfigureeritud, saate vaadata kõiki saadaval cmdlettide teenuse [siin](https://msdn.microsoft.com/library/dn850420.aspx). 

Kohta näpunäiteid, mis aitavad saate kasutada cmdlet-käsud, nt kuidas parameetrite väärtused, sisendeid ja väljundeid on tavaliselt kasutatakse Azure PowerShelli, leiate [juhendi alustamine Azure'i cmdlet-käsud](https://msdn.microsoft.com/library/azure/jj554332.aspx).

## <a name="step-1-set-the-subscription"></a>Samm 1: Määrake tellimusega 

1. Azure'i PowerShelli Azure'i kontosse sisselogimise kaudu: järgmised cmdlet-käskude kasutamine
 
        $UserName = "<user@live.com>"
        $Password = "<password>"
        $SecurePassword = ConvertTo-SecureString -AsPlainText $Password -Force
        $Cred = New-Object System.Management.Automation.PSCredential -ArgumentList $UserName, $SecurePassword
        Login-AzureRmAccount #-Credential $Cred 
    

2. Tellimuste loendi hankimine. See kuvab ka selle subscriptionIDs iga tellimustest. Pange kirja tellimuse, milles soovite luua taastamise teenuste hoidla subscriptionID    

        Get-AzureRmSubscription 

3. See tellimus, kus taastamise teenuste hoidla on loodud viitavad Tellimuse ID määramine

        Set-AzureRmContext –SubscriptionID <subscriptionId>


## <a name="step-2-create-a-recovery-services-vault"></a>Samm 2: Looge võlvikus taastamise teenused 

1. Kui teil pole seda juba Azure'i ressursihaldur ressursi rühma loomine

        New-AzureRmResourceGroup -Name #ResourceGroupName -Location #location

2. Looge uus taastamise teenused vault ja salvestada loodud ASR vault objekti muutuja (kasutatakse hiljem). Saate alla laadida ka ASR vault objektide Postituse loomine Get-AzureRMRecoveryServicesVault cmdlet-käsu abil:-

        $vault = New-AzureRmRecoveryServicesVault -Name #vaultname -ResouceGroupName #ResourceGroupName -Location #location 

## <a name="step-3-set-the-recovery-services-vault-context"></a>Samm 3: Määrake taastamise teenuste hoidla kontekst

1.  Kui teil on juba loodud võlvkelder, käivitage selle all käsk saada vault.

        $vault = Get-AzureRmRecoveryServicesVault -Name #vaultname

2.  Töötab vault raames määratud selle all käsk.

        Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault



## <a name="step-4-install-the-azure-site-recovery-provider"></a>Samm 4: Installida Azure saidi taastamise pakkuja

1.  Arvutis VMM luua, käivitage järgmine käsk:
    
        New-Item c:\ASR -type directory
        
2.  Ekstrakti failid allalaaditud pakkuja järgmise käsu abil
    
        pushd C:\ASR\
        .\AzureSiteRecoveryProvider.exe /x:. /q

    
3.  Installige pakkuja kasutada järgmisi käske.
    
        .\SetupDr.exe /i
        $installationRegPath = "hklm:\software\Microsoft\Microsoft System Center Virtual Machine Manager Server\DRAdapter"
        do
        {
                        $isNotInstalled = $true;
                        if(Test-Path $installationRegPath)
                        {
                                        $isNotInstalled = $false;
                        }
        }While($isNotInstalled)

    Oodata installimise lõpuni.
    
4.  Registreerida serveri vault abil järgmine käsk:
    
        $BinPath = $env:SystemDrive+"\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin"
        pushd $BinPath
        $encryptionFilePath = "C:\temp\".\DRConfigurator.exe /r /Credentials $VaultSettingFilePath /vmmfriendlyname $env:COMPUTERNAME /dataencryptionenabled $encryptionFilePath /startvmmservice


## <a name="step-5-create-and-associate-a-replication-policy"></a>Juhis 5: Loomine ja seostamiseks poliitika dispersioonanalüüs

1.  Hyper-V 2012 R2 dispersioonanalüüs poliitika loomine, käivitage järgmine käsk:

    
        $ReplicationFrequencyInSeconds = "300";     #options are 30,300,900
        $PolicyName = “replicapolicy”
        $RepProvider = HyperVReplica2012R2
        $Recoverypoints = 24                    #specify the number of hours to retain recovery pints
        $AppConsistentSnapshotFrequency = 4 #specify the frequency (in hours) at which app consistent snapshots are taken
        $AuthMode = "Kerberos"  #options are "Kerberos" or "Certificate"
        $AuthPort = "8083"  #specify the port number that will be used for replication traffic on Hyper-V hosts
        $InitialRepMethod = "Online" #options are "Online" or "Offline"

        $policyresult = New-AzureRmSiteRecoveryPolicy -Name $policyname -ReplicationProvider $RepProvider -ReplicationFrequencyInSeconds $Replicationfrequencyinseconds -RecoveryPoints $recoverypoints -ApplicationConsistentSnapshotFrequencyInHours $AppConsistentSnapshotFrequency -Authentication $AuthMode -ReplicationPort $AuthPort -ReplicationMethod $InitialRepMethod 

    > [AZURE.NOTE] VMM pilveteenuses võib sisaldada Hyper-V hosts erineva versiooniga Windows Server (nagu mainitud eeltingimused Hyper-V), kuid dispersioonanalüüs poliitika on Opsüsteemi versioon teatud. Kui teil on erinevate hosts töötavate operatsioonisüsteemi versiooni, siis luua eraldi dispersioonanalüüs poliitikate iga tüüpi Opsüsteemi versioon. Jaoks näiteks: kui teil on viis hosts töötab Windows serverite 2012 ja Windows Server 2012 R2, kolm loomine kahe dispersioonanalüüs poliitikat – operatsioonisüsteemi versiooni jaoks eraldi.

2.  Saada esmane kaitse container (esmane VMM pilveteenus) ja taastamise kaitse container (taastamine VMM Cloud) käivitades järgmised käsud:
    
        $PrimaryCloud = "testprimarycloud"
        $primaryprotectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloud;  

        $RecoveryCloud = "testrecoverycloud"
        $recoveryprotectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $RecoveryCloud;  
  
3.  Saate tuua juhises 1 sõbralik nimi poliitika abil loodud poliitika

        $policy = Get-AzureRmSiteRecoveryPolicy -FriendlyName $policyname

4.  Alusta kaitse-ümbrisest (VMM pilveteenus) seost dispersioonanalüüs poliitika:

        $associationJob  = Start-AzureRmSiteRecoveryPolicyAssociationJob -Policy     $Policy -PrimaryProtectionContainer $primaryprotectionContainer -RecoveryProtectionContainer $recoveryprotectionContainer

5.  Oodake poliitika seos töö lõpuleviimiseks. Saate kontrollida, kui töö on lõppenud, kasutades järgmist PowerShelli koodilõigu.
   
        $job = Get-AzureRmSiteRecoveryJob -Job $associationJob
        if($job -eq $null -or $job.StateDescription -ne "Completed")
         {
            $isJobLeftForProcessing = $true;
        }

    Kui töö on lõpule jõudnud töötlemine, käivitage järgmine käsk:

        if($isJobLeftForProcessing)
        {
        Start-Sleep -Seconds 60
        }
        }While($isJobLeftForProcessing)


Märkige ruut selle toimingu lõpuleviimist, järgige [Tegevuse jälgimine](#monitor).

## <a name="step-5-configure-network-mapping"></a>Juhis 5: Konfigureerige võrgu vastendamine

1. Esimese käsu saab serverid praeguse Azure'i saidi taastamise hoidla. Käsk salvestab Microsoft Azure saidi taastamise serverid $Servers massiivi muutujat.

        $Servers = Get-AzureRmSiteRecoveryServer

2. Selle all käskude saada saidi taastamise võrgu allika VMM serveri ja target VMM.

        $PrimaryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[0]        

        $RecoveryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[1]

    
    > [AZURE.NOTE] Andmeallika VMM server võib olla esimene ja teine serverid massiivis. Märkige VMM serverite nimed ja saada võrkude õigesti


4. Lõplik cmdlet loob vastenduse esmane võrgu ja taastamise võrgu vahel. Cmdlet määrab esmane võrgu $PrimaryNetworks ja taastamise võrgu $RecoveryNetworks esimese osa esimene element.

        New-AzureRmSiteRecoveryNetworkMapping -PrimaryNetwork $PrimaryNetworks[0] -RecoveryNetwork $RecoveryNetworks[0]

## <a name="step-6-configure-storage-mapping"></a>Samm 6: Konfigureerige salvestusruumi vastendamine

1. Selle all käsk saab salvestusruumi liigitusi üheks $storageclassifications muutuja loendit.

        $storageclassifications = Get-AzureRmSiteRecoveryStorageClassification


2. Selle all käskude saada allika liigitamine $SourceClassificaion muutuja ja target liigitamine $TargetClassification muutuja sisse. 

        $SourceClassificaion = $storageclassifications[0]

        $TargetClassification = $storageclassifications[1]

    
    > [AZURE.NOTE] Lähte- ja liigitusi võib olla mis tahes elemendi massiivis. Viidata väljund selle all käsk aru lähte- ja liigitusi $storageclassifications massiivi indeks. 
    
    > Get-AzureRmSiteRecoveryStorageClassification | Valige-objekt - atribuut FriendlyName, Id | Tabelivorming


3. Selle cmdlet-käsk allpool loob vastenduse andmeallika liigitamine ja target liigitamine vahel. 

        New-AzureRmSiteRecoveryStorageClassificationMapping -PrimaryStorageClassification $SourceClassificaion -RecoveryStorageClassification $TargetClassification

## <a name="step-7-enable-protection-for-virtual-machines"></a>Juhis 7: Lubamine kaitse virtuaalmasinates

Pärast serverid, pilved ja võrgu on õigesti konfigureeritud, saate lubada kaitse virtuaalmasinates pilveteenuses. 


  1. Kaitse lubamiseks käivitage järgmine käsk saada kaitse container:
    
            $PrimaryProtectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloudName
    
  2. Saada kaitse üksus (VM), käivitage järgmine käsk:
    
            $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -friendlyName $VMName -ProtectionContainer $PrimaryProtectionContainer
        
  3. Lubada kopeerimine VM, käivitage järgmine käsk:

            $jobResult = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionentity -Protection Enable -Policy $policy


## <a name="test-your-deployment"></a>Testige oma juurutamine

Testimiseks käivitage test Tõrkesiirde jaoks ühe virtuaalse masina, või luua taastamise kava koosneb mitmest virtuaalmasinates ja käivitage test Tõrkesiirde lepingu raames juurutamise. Test Tõrkesiirde jäljendab oma Tõrkesiirde ja taastamise süsteem isoleeritakse võrgus. 

> [AZURE.NOTE] Azure'i portaalis saate luua rakenduse taastamis.

Märkige ruut selle toimingu lõpuleviimist, järgige [Tegevuse jälgimine](#monitor).


### <a name="run-a-test-failover"></a>Käivitage test Tõrkesiirde

1.  Käivitage selle cmdlet-käskude saada VM võrk, millele soovite testida Tõrkesiirde oma VMs all.

        $Servers = Get-AzureRmSiteRecoveryServer
        $RecoveryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[1]

2.  Tehke test Tõrkesiirde VM, tehes järgmist.
    
        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -FriendlyName $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -VMNetwork $RecoveryNetworks[1] 

2.  Tehke test Tõrkesiirde taastamis, tehes järgmist.
    
        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -Recoveryplan $recoveryplan -VMNetwork $RecoveryNetworks[1] 

### <a name="run-a-planned-failover"></a>Kavandatud Tõrkesiirde käivitamine

1. Tehke VM kavandatud Tõrkesiirde, tehes järgmist.
    
        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity

2. Tehke taastamis kavandatud Tõrkesiirde, tehes järgmist.
    
        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -Recoveryplan $recoveryplan

### <a name="run-an-unplanned-failover"></a>Planeerimata Tõrkesiirde käivitamine

1. Tehke planeerimata Tõrkesiirde VM, tehes järgmist.
        
        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity 

2. teha planeerimata Tõrkesiirde taastamise kava, tehes järgmist:
        
        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity 
    
## <a name=monitor></a>Jälgida tegevuste

Järgmised käsud abil saate jälgida. Pange tähele, et peate ootama töökohta lõpuleviimiseks töötlemiseks.

    Do
    {
        $job = Get-AzureSiteRecoveryJob -Id $associationJob.JobId;
        Write-Host "Job State:{0}, StateDescription:{1}" -f Job.State, $job.StateDescription;
        if($job -eq $null -or $job.StateDescription -ne "Completed")
        {
            $isJobLeftForProcessing = $true;
        }

    if($isJobLeftForProcessing)
        {
            Start-Sleep -Seconds 60
        }
    }While($isJobLeftForProcessing)



## <a name="next-steps"></a>Järgmised sammud

[Lisateavet vt](https://msdn.microsoft.com/library/azure/mt637930.aspx) Azure saidi taastamise ja Azure ressursihaldur PowerShelli cmdlet-käskude kohta.

