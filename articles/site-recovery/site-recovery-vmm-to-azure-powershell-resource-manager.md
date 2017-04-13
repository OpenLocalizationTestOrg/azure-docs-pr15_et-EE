<properties
    pageTitle="Ise Hyper-V virtuaalmasinates VMM pilved Azure saidi taastamine ja PowerShelli (ressursihaldur) abil | Microsoft Azure'i"
    description="Ise Hyper-V virtuaalmasinates VMM pilved Azure saidi taastamine ja PowerShelli abil"
    services="site-recovery"
    documentationCenter=""
    authors="Rajani-Janaki-Ram"
    manager="rochakm"
    editor="raynew"/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="rajanaki"/>

# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-azure-using-powershell-and-azure-resource-manager"></a>Paljundada Hyper-V virtuaalmasinates VMM pilved Azure PowerShelli ja Azure ressursihaldur abil

> [AZURE.SELECTOR]
- [Azure'i portaal](site-recovery-vmm-to-azure.md)
- [PowerShelli - ressursihaldur](site-recovery-vmm-to-azure-powershell-resource-manager.md)
- [Klassikaline portaal](site-recovery-vmm-to-azure-classic.md)
- [PowerShelli – klassikaline](site-recovery-deploy-with-powershell.md)



## <a name="overview"></a>Ülevaade

Azure'i saidi taastamine aitab teie ettevõtte järjepidevus ja avariijärgne taastamine (BCDR) strateegia orkesteroinnin dispersioonanalüüs, Tõrkesiirde ja taastamine virtuaalmasinates juurutamise stsenaariumide arvu. Juurutamise stsenaariumide täieliku loendi leiate [Azure'i saidi taastamine ülevaade](site-recovery-overview.md).

Selles artiklis näidatakse, kuidas PowerShelli abil tuleb teha siis, kui häälestate Azure saidi taastamise paljundada Hyper-V virtuaalmasinates süsteem Center VMM pilved Azure storage levinud toiminguid automatiseerida.

See artikkel sisaldab stsenaarium eeltingimuste ja näidatakse

- Kuidas häälestada taastamise teenuste hoidla
- Installige Azure'i saidi taastamise pakkuja VMM-i serverisse
- Registreerima server vault, Azure storage konto lisamine
- Installige Azure'i taastamise teenused agent Hyper-V hosti serverites
- VMM pilved, mis rakenduvad kogu kaitstud virtuaalmasinates kaitse sätete konfigureerimine
- Luba nende virtuaalmasinates kaitse.
- Testige ei lõppenud veendumaks, et kõik töötab ootuspäraselt.

Kui tekib probleeme häälestamise sel juhul, postitada oma küsimusi [Azure taastamise teenused Foorum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

> [AZURE.NOTE] Azure'i on kaks eri juurutamise mudelite loomise ja ressursside töötamine: [ressursihaldur ja klassikaline](../resource-manager-deployment-model.md). Selles artiklis antakse ülevaade ressursihaldur juurutamise mudeli abil.

## <a name="before-you-start"></a>Enne alustamist

Veenduge, et teil on nende eeltingimuste kohas.

### <a name="azure-prerequisites"></a>Azure'i eeltingimused

- Peate [Microsoft Azure'i](https://azure.microsoft.com/) kontosse. Kui teil pole ühte, alustage [tasuta konto](https://azure.microsoft.com/free). Lisaks saate lugeda [Azure saidi taastamine Manager hinnad](https://azure.microsoft.com/pricing/details/site-recovery/).
- Peate CSP tellimust, kui üritate välja dispersioonanalüüs CSP tellimuse stsenaariumi. Lugege lisateavet, [Kuidas registreeruda CSP programmi](https://msdn.microsoft.com/library/partnercenter/mt156995.aspx)CSP programm.
- Peate konto Azure v2 salvestusruumi (ressursihaldur) Azure'i kopeeritud andmete talletamiseks. Konto peab geo-dispersioonanalüüs lubatud. See peaks olema sama piirkonna Azure saidi taastamise teenuse ja olema seostatud sama tellimus või tellimuse CSP. Azure'i salvestusruumi häälestamise kohta leiate lisateavet teemast [Sissejuhatus Microsoft Azure'i Tabelimäluga](../storage/storage-introduction.md) viite.
- Peate veenduge, et [eeltingimused Azure virtuaalse masina](site-recovery-best-practices.md#azure-virtual-machine-requirements)järgi virtuaalmasinates, mida soovite kaitsta.

> [AZURE.NOTE] Praegu ainult VM taseme toimingud on võimalik PowerShelli kaudu. Tugiteenuste jaoks lepingu taseme tehakse varsti.  Nüüd olete piiratud läbimiseks fail pulloverid ainult "kaitstud VM" granulaarsus ja pole tasemel taastamise kava.

### <a name="vmm-prerequisites"></a>VMM eeltingimused
- Peate VMM server töötab System Center 2012 R2.
- Mis tahes VMM server, mis sisaldab virtuaalmasinates, mida soovite kaitsta, peate kasutama Azure saidi taastamise pakkuja. See on installitud Azure saidi taastamise juurutamise käigus.
- Teil peab olema vähemalt üks cloud VMM-i server, mida soovite kaitsta. Pilveteenuses peaks sisaldama:
    - Ühe või mitme VMM hosti rühmad.
    - Ühe või mitme Hyper-V hosti serverite või kogumite iga host jaotises.
    - Ühe või mitme virtuaalmasinates allikas Hyper-V server.
- Lisateavet VMM pilved häälestamise kohta:
    - Lisateavet leiate privaatne VMM pilved, [mis](http://go.microsoft.com/fwlink/?LinkId=324952) on uut privaatne pilve System Center 2012 R2 VMM ja [VMM 2012](http://go.microsoft.com/fwlink/?LinkId=324956)ja pilved kohta.
    - Lisateavet [VMM cloud struktuuri konfigureerimise](https://msdn.microsoft.com/library/azure/dn469075.aspx#BKMK_Fabric) kohta
    - Pärast oma pilveteenuses struktuuri elemendid on olemas loomise privaatne pilved [Privaatne cloud VMM loomise](http://go.microsoft.com/fwlink/p/?LinkId=324953) ja sisse kohta lisateavet [tutvustust: loomine privaatne pilved System Center 2012 SP1 VMM](http://go.microsoft.com/fwlink/p/?LinkId=324954).


### <a name="hyper-v-prerequisites"></a>Hyper-V eeltingimused

- Hosti Hyper-V serverid peab olema vähemalt opsüsteemi **Windows Server 2012** Hyper-V roll või **Microsoft Hyper-V Server 2012** ja olete installinud uusimaid värskendusi.
- Kui kasutate operatsioonisüsteemi Hyper-V kobar märget selle kobar ta ei luuakse automaatselt, kui teil on staatiline IP address-põhiste kobar. Peate kobar ta käsitsi konfigureerimine. Jaoks
- Juhiste saamiseks vaadake, [Kuidas konfigureerida Hyper-V koopia ta](http://blogs.technet.com/b/haroldwong/archive/2013/03/27/server-virtualization-series-hyper-v-replica-broker-explained-part-15-of-20-by-yung-chou.aspx).
- Mis tahes Hyper-V hosti server või kobar, mille jaoks soovite hallata kaitse peab kuuluma VMM pilveteenusesse.

### <a name="network-mapping-prerequisites"></a>Võrgu vastenduse eeltingimused
Kui kaitse virtuaalmasinates Azure virtuaalse masina võrkude VMM-i serverisse võrgu vastenduse kaartide ja suunata Azure võrkude lubamiseks järgmist:

- Kõik seadmed, mis ei õnnestu üle samas võrgus, saate luua ühenduse teineteisega, olenemata sellest, millist taastamise kava need on.
- Kui võrgu lüüsi on seatud Azure võrgu häälestamine, virtuaalmasinates saate luua ühenduse muude kohapealse virtuaalmasinates.
- Kui konfigureerite ei võrgu kaardistamine, saab ainult sama taastamise leping ei õnnestu üle virtuaalmasinates omavahel ühendada, ei lõppenud Azure pärast.

Kui soovite võrgu kaardistamine juurutada, läheb teil vaja järgmist:

- Virtuaalmasinates, mida soovite kaitsta VMM-i serverisse peaks olema ühendatud VM võrk. Selle võrgu peaks olema seotud loogilise võrgu, mis on seotud pilve.
- Azure'i võrgustik, millele saab ühendada tiražeeritud virtuaalmasinates pärast ei lõppenud. Valite võrku ei lõppenud ajal. Võrgu peaks olema sama piirkonna tellimuse Azure saidi taastamine.

Lisateavet võrgu kaardistamine

- [Kuidas konfigureerida VMM loogilised võrgud](http://go.microsoft.com/fwlink/p/?LinkId=386307)
- [Kuidas seadistada VM võrgu ja lüüside VMM](http://go.microsoft.com/fwlink/p/?LinkId=386308)
- [Kuidas seadistada ja jälgida virtuaalse võrkude Azure](https://azure.microsoft.com/documentation/services/virtual-network/)


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

1. Ressursi rühma loomine Azure ressursihaldur, kui teil pole seda juba

        New-AzureRmResourceGroup -Name #ResourceGroupName -Location #location

2. Looge uus taastamise teenused vault ja salvestada loodud ASR vault objekti muutuja (kasutatakse hiljem). Saate alla laadida ka ASR vault objektide Postituse loomine Get-AzureRMRecoveryServicesVault cmdlet-käsu abil:-

        $vault = New-AzureRmRecoveryServicesVault -Name #vaultname -ResouceGroupName #ResourceGroupName -Location #location

## <a name="step-3-set-the-recovery-services-vault-context"></a>Samm 3: Määrake taastamise teenuste hoidla kontekst

1.  Töötab vault raames määratud selle all käsk.

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


## <a name="step-5-create-an-azure-storage-account"></a>Juhis 5: Looge konto Azure salvestusruum

1. Kui teil pole kontot Azure storage, geo-dispersioonanalüüs lubatud konto loomine sama nimega vault Geo, käivitage järgmine käsk:

        $StorageAccountName = "teststorageacc1" #StorageAccountname
        $StorageAccountGeo  = "Southeast Asia"  
        $ResourceGroupName =  “myRG”            #ResourceGroupName
        $RecoveryStorageAccount = New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageAccountName -Type “StandardGRS” -Location $StorageAccountGeo

Pange tähele, et salvestusruumi konto peab olema sama piirkonna Azure saidi taastamise teenuse olema sama tellimusega seostatud.

## <a name="step-6-install-the-azure-recovery-services-agent"></a>Samm 6: Installida Azure taastamise teenused Agent

1. Azure taastamise teenused agendi veebisaidil [http://aka.ms/latestmarsagent](http://aka.ms/latestmarsagent) alla laadida ja installida iga Hyper-V hosti server asuvad VMM pilved, mida soovite kaitsta.

2. Kõik VMM hosts käivitage järgmine käsk:

    marsagentinstaller.exe /q /nu

## <a name="step-7-configure-cloud-protection-settings"></a>Juhis 7: Konfigureerimine pilve sätted

1.  Azure'i dispersioonanalüüs poliitika loomine, käivitage järgmine käsk:


        $ReplicationFrequencyInSeconds = "300";     #options are 30,300,900
        $PolicyName = “replicapolicy”
        $Recoverypoints = 6                 #specify the number of recovery points

        $policryresult = New-AzureRmSiteRecoveryPolicy -Name $policyname -ReplicationProvider HyperVReplicaAzure -ReplicationFrequencyInSeconds $replicationfrequencyinseconds -RecoveryPoints $recoverypoints -ApplicationConsistentSnapshotFrequencyInHours 1 -RecoveryAzureStorageAccountId "/subscriptions/q1345667/resourceGroups/test/providers/Microsoft.Storage/storageAccounts/teststorageacc1"

2.  Saada kaitse container käivitades järgmised käsud:

        $PrimaryCloud = "testcloud"
        $protectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloud;  

3.  Saada poliitika üksikasjad muutuja abil tööd, mis on loodud ja viitavad poliitika sõbralik nimi.

        $policy = Get-AzureRmSiteRecoveryPolicy -FriendlyName $policyname

4.  Alusta kaitse-ümbrisest seost dispersioonanalüüs poliitika:

        $associationJob  = Start-AzureRmSiteRecoveryPolicyAssociationJob -Policy     $Policy -PrimaryProtectionContainer $protectionContainer  

5.  Kui töö on lõpule jõudnud, käivitage järgmine käsk:

        $job = Get-AzureRmSiteRecoveryJob -Job $associationJob
        if($job -eq $null -or $job.StateDescription -ne "Completed")
         {
        $isJobLeftForProcessing = $true;
        }

6.  Kui töö on lõpule jõudnud töötlemine, käivitage järgmine käsk:

        if($isJobLeftForProcessing)
        {
        Start-Sleep -Seconds 60
        }
        }While($isJobLeftForProcessing)

Märkige ruut selle toimingu lõpuleviimist, järgige [Tegevuse jälgimine](#monitor).

## <a name="step-8-configure-network-mapping"></a>Samm 8: Konfigureerige võrgu vastendamine

Enne alustamist võrgu kaardistamine veenduge, et virtuaalmasinates VMM-i serverisse on ühendatud võrku VM. Lisaks saate luua ühe või mitme Azure virtuaalse võrgu.

Lisateavet selle kohta, kuidas luua virtuaalse võrgu [loomine virtuaalse võrgu - saidilt VPN-ühenduse Azure'i ressursihaldur ja PowerShelli abil](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md) Azure'i ressursihaldur ja PowerShelli kasutamine

Pange tähele, mitme virtuaalse masina võrgu saab vastendada ühe Azure võrgus. Kui target võrgus on mitu alamvõrku ja üks nende alamvõrku on alamvõrgu allika virtuaalse masina asub sama nimi, siis koopia virtuaalse masina ühendatakse selle target alamvõrgu pärast ei lõppenud. Kui pole target alamvõrgu kattuvad nimega, virtuaalse masina ühendatud esimese alamvõrgu võrgus.

1. Esimese käsu saab serverid praeguse Azure'i saidi taastamise hoidla. Käsk salvestab Microsoft Azure saidi taastamise serverid $Servers massiivi muutujat.

        $Servers = Get-AzureRmSiteRecoveryServer

2. Teine käsk saab $Servers massiivi esimese serveri saidi taastamise võrku. Käsu talletab võrkude $Networks muutujana.


        $Networks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[0]

3. Kolmas käsk saab Azure'i ja seejärel selle väärtuse $AzureVmNetworks muutujana.

        $AzureVmNetworks =  Get-AzureRmVirtualNetwork

4. Lõplik cmdlet loob vastenduse esmane võrgu ja Azure virtuaalse masina võrgu vahel. Cmdlet määrab esmane võrgu $Networks esimene element. Cmdlet määrab virtuaalse masina võrgu $AzureVmNetworks esimene element.

        New-AzureRmSiteRecoveryNetworkMapping -PrimaryNetwork $Networks[0] -AzureVMNetworkId $AzureVmNetworks[0]


## <a name="step-9-enable-protection-for-virtual-machines"></a>Samm 9: Lubamine kaitse virtuaalmasinates

Pärast serverid, pilved ja võrgu on õigesti konfigureeritud, saate lubada kaitse virtuaalmasinates pilveteenuses.

 Võtke arvesse järgmist.

 - Virtuaalmasinates peab vastama Azure nõuetele. Märkige nende [eeltingimuste](site-recovery-best-practices.md) ja tugiteenuste kavandamise juhend.

 - Kaitse, operatsioonisüsteem ja operatsioonisüsteemi seatakse virtuaalse masina ketta atribuudid. Kui loote virtuaalse masina VMM virtuaalse masina malli abil saate atribuuti. Samuti saate nende atribuutide olemasolevaid virtuaalmasinates virtuaalse masina atribuudid vahekaartidel **üldist** ja **Riistvara konfiguratsiooni** . Kui te ei määra atribuutidest VMM kuvatakse saama need konfigureerida Azure saidi taastamise portaalis.


  1. Kaitse lubamiseks käivitage järgmine käsk saada kaitse container:

            $ProtectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $CloudName

  2. Saada kaitse üksus (VM), käivitage järgmine käsk:

            $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -friendlyName $VMName -ProtectionContainer $protectionContainer

  3. Luba DR VM, käivitage järgmine käsk:

            $jobResult = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionentity -Protection Enable –Force -Policy $policy -RecoveryAzureStorageAccountId  $storageID "/subscriptions/217653172865hcvkchgvd/resourceGroups/rajanirgps/providers/Microsoft.Storage/storageAccounts/teststorageacc1



## <a name="test-your-deployment"></a>Testige oma juurutamine

Testimiseks käivitage test fail-üle ühe virtuaalse masina, või luua taastamise kava koosneb mitmest virtuaalmasinates ja käivitage test fail-üle leping juurutamise. Testi ei lõppenud jäljendab oma ei lõppenud ja taastamise süsteem isoleeritakse võrgus. Pange tähele järgmist.

- Kui soovite pärast ei lõppenud kaugtöölaua kasutamise Azure virtuaalse masina ühenduse, lubada kaugtöölaua ühendus virtuaalse masina enne, kui käivitate test Tõrkesiirde.
- Pärast ei lõppenud, saate kasutada avaliku IP-aadressi ühenduse, virtuaalse masina Azure kaugtöölaua kasutamise. Kui soovite seda teha, veenduge, et teil pole mõnda domeeni poliitika, mis takistada ühenduse virtuaalse masina avalik aadressidel.

Märkige ruut selle toimingu lõpuleviimist, järgige [Tegevuse jälgimine](#monitor).


### <a name="run-a-test-failover"></a>Käivitage test Tõrkesiirde

1.  Test Tõrkesiirde käivitage järgmine käsk:

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $protectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -AzureVMNetworkId <string>  

### <a name="run-a-planned-failover"></a>Kavandatud Tõrkesiirde käivitamine

1. Kavandatud Tõrkesiirde käivitage järgmine käsk:

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $protectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -AzureVMNetworkId <string>  

### <a name="run-an-unplanned-failover"></a>Planeerimata Tõrkesiirde käivitamine

1. Planeerimata Tõrkesiirde käivitage järgmine käsk:

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $protectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -AzureVMNetworkId <string>  


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
