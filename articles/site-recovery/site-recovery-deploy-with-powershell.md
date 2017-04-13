<properties
    pageTitle="Ise Hyper-V virtuaalmasinates VMM pilved Azure saidi taastamine ja PowerShelli abil | Microsoft Azure'i"
    description="Saate teada, kuidas dispersioonanalüüs, Hyper-V virtuaalmasinates VMM pilved saidi taastamine ja PowerShelli abil automatiseerida."
    services="site-recovery"
    documentationCenter=""
    authors="bsiva"
    manager="abhiag"
    editor="tysonn"/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="bsiva"/>

# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-azure-using-powershell---classic"></a>Paljundada Hyper-V virtuaalmasinates VMM pilved Azure PowerShelli – klassikaline kaudu

> [AZURE.SELECTOR]
- [Azure'i portaal](site-recovery-vmm-to-azure.md)
- [PowerShelli - ressursihaldur](site-recovery-vmm-to-azure-powershell-resource-manager.md)
- [Klassikaline portaal](site-recovery-vmm-to-azure-classic.md)
- [PowerShelli – klassikaline](site-recovery-deploy-with-powershell.md)


## <a name="overview"></a>Ülevaade

Azure'i saidi taastamine aitab teie ettevõtte järjepidevus ja avariijärgne taastamine (BCDR) strateegia orkesteroinnin dispersioonanalüüs, Tõrkesiirde ja taastamine virtuaalmasinates juurutamise stsenaariumide arvu. Juurutamise stsenaariumide täieliku loendi leiate [Azure'i saidi taastamine ülevaade](site-recovery-overview.md).

Selles artiklis näidatakse, kuidas PowerShelli abil tuleb teha siis, kui häälestate Azure saidi taastamise paljundada Hyper-V virtuaalmasinates süsteem Center VMM pilved Azure storage levinud toiminguid automatiseerida.

Artikkel sisaldab stsenaarium eeltingimuste ja näidatakse, kuidas häälestada saidi taastamise hoidla, installida Azure saidi taastamise pakkuja VMM-i serverisse, registreerida serveri vault, Azure storage konto lisamine installida Azure taastamise teenused agendi Hyper-V hosti serverid, VMM pilved, mis rakenduvad kogu kaitstud virtuaalmasinates kaitse sätete konfigureerimine , ja seejärel lubada nendes virtuaalmasinates kaitse. Lõpule jõuab testida Tõrkesiirde veendumaks, et kõik töötab ootuspäraselt.

Kui tekib probleeme häälestamise sel juhul, postitada oma küsimusi [Azure taastamise teenused Foorum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


> [AZURE.NOTE] Azure'i on kaks eri juurutamise mudelite loomise ja ressursside töötamine: [ressursihaldur ja klassikaline](../resource-manager-deployment-model.md). Selles artiklis antakse ülevaade klassikaline juurutamise mudeli abil.



## <a name="before-you-start"></a>Enne alustamist

Veenduge, et teil on nende eeltingimuste kohas.

### <a name="azure-prerequisites"></a>Azure'i eeltingimused

- Peate [Microsoft Azure'i](https://azure.microsoft.com/) kontosse. Saate alustada [tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/).
- Peate konto Azure storage kopeeritud andmete talletamiseks. Konto peab geo-dispersioonanalüüs lubatud. See peaks olema sama piirkonna Azure'i saidi taastamise hoidla ja olema sama tellimusega seostatud. [Lisateavet leiate teemast Azure salvestusruumi kohta](../storage/storage-introduction.md).
- Peate veenduge, et [eeltingimused Azure virtuaalse masina](site-recovery-best-practices.md#virtual-machines)järgi virtuaalmasinates, mida soovite kaitsta.

### <a name="vmm-prerequisites"></a>VMM eeltingimused
- Peate VMM server töötab System Center 2012 R2.
- Teil peab olema vähemalt üks cloud VMM-i server, mida soovite kaitsta. Pilveteenuses peaks sisaldama:
    - Ühe või mitme VMM hosti rühmad.
    - Ühe või mitme Hyper-V hosti serverite või kogumite iga host jaotises.
    - Ühe või mitme virtuaalmasinates allikas Hyper-V server.

### <a name="hyper-v-prerequisites"></a>Hyper-V eeltingimused

- Hosti Hyper-V serverid peab olema vähemalt opsüsteemi **Windows Server 2012** Hyper-V roll või **Microsoft Hyper-V Server 2012** ja olete installinud uusimaid värskendusi.
- Kui kasutate operatsioonisüsteemi Hyper-V kobar märget selle kobar ta ei luuakse automaatselt, kui teil on staatiline IP address-põhiste kobar. Peate kobar ta käsitsi konfigureerimine. Selleks tehke järgmist Server Manager > tõrkesiirdeklastri halduri ühenduse klaster nuppu **Konfigureerimine rolli** ja valige **Hyper-V koopia ta** kõrge-saadavus viisardi kuval **Valige roll** .
- Mis tahes Hyper-V hosti server või kobar, mille jaoks soovite hallata kaitse peab kuuluma VMM pilveteenusesse.

### <a name="network-mapping-prerequisites"></a>Võrgu vastenduse eeltingimused
Kui kasutate kaitse virtuaalmasinates Azure võrgu vastenduse kaartidel vahel VM VMM-i serverisse ja suunata Azure võrkude lubamiseks järgmist:

- Kõik seadmed, mis ei õnnestu üle samas võrgus, saate luua ühenduse teineteisega, olenemata sellest, millist taastamise kava need on.
- Kui võrgu lüüsi on seatud Azure võrgu häälestamine, virtuaalmasinates saate luua ühenduse muude kohapealse virtuaalmasinates.
- Kui konfigureerite võrgu vastenduse ainult virtuaalmasinates mis ei õnnestu üle sama taastamine lepingus on võimalik omavahel ühendada pärast Tõrkesiirde Azure.

Kui soovite võrgu kaardistamine juurutada, läheb teil vaja järgmist:

- Virtuaalmasinates, mida soovite kaitsta VMM-i serverisse peaks olema ühendatud VM võrk. Selle võrgu peaks olema seotud loogilise võrgu, mis on seotud pilve.
- Azure'i võrgustik, millele saab ühendada tiražeeritud virtuaalmasinates pärast Tõrkesiirde. Valite selle võrgu Tõrkesiirde ajal. Võrgu peaks olema sama piirkonna tellimuse Azure saidi taastamine.
- [Lisateavet leiate teemast](site-recovery-network-mapping.md) võrgu vastendamise kohta:

###<a name="powershell-prerequisites"></a>PowerShelli eeltingimused
Veenduge, et teil on Azure PowerShelli kasutamiseks valmis. Kui kasutate juba PowerShelli, peate varasema versiooni täiendamine versiooniks versioon 0.8.10 või uuem versioon. PowerShelli häälestamise kohta leiate artiklist [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md). Kui teil on häälestamine ja PowerShelli konfigureeritud, saate vaadata kõiki saadaval cmdlettide teenuse [siin](https://msdn.microsoft.com/library/dn850420.aspx).

Kohta näpunäiteid, mis aitavad saate kasutada cmdlet-käsud, nt kuidas parameetrite väärtused, sisendeid ja väljundeid on tavaliselt kasutatakse Azure PowerShelli, leiate [Azure'i cmdlettide kasutamise alustamine](https://msdn.microsoft.com/library/azure/jj554332.aspx).

## <a name="step-1-set-the-subscription"></a>Samm 1: Määrake tellimusega

PowerShelli, käivitage järgmised cmdlet-käsud:

```
$UserName = "<user@live.com>"
$Password = "<password>"
$AzureSubscriptionName = "prod_sub1"

$SecurePassword = ConvertTo-SecureString -AsPlainText $Password -Force
$Cred = New-Object System.Management.Automation.PSCredential -ArgumentList $UserName, $securePassword
Add-AzureAccount -Credential $Cred;
$AzureSubscription = Select-AzureSubscription -SubscriptionName $AzureSubscriptionName

```

Asendage "< >" sees elementide kindla teabe.

## <a name="step-2-create-a-site-recovery-vault"></a>Samm 2: Looge saidi taastamise hoidla

PowerShelli, elementide sees "< >" asendamine oma kindla teabe ja käivitage järgmised käsud:

```

$VaultName = "<testvault123>"
$VaultGeo  = "<Southeast Asia>"
$OutputPathForSettingsFile = "<c:\>"

```


```
New-AzureSiteRecoveryVault -Location $VaultGeo -Name $VaultName;
$vault = Get-AzureSiteRecoveryVault -Name $VaultName;

```

## <a name="step-3-generate-a-vault-registration-key"></a>Samm 3: Luua vault registreerimise võti

Luua autoriloomingut registreerimise võti. Pärast Azure saidi taastamise pakkuja Laadige alla ja installige see VMM server, saate selle klahvi abil registreerida VMM server autoriloomingut.

1.  Saada vault sätte faili ja määrake kontekstis.

    ```

    $VaultName = "<testvault123>"
    $VaultGeo  = "<Southeast Asia>"
    $OutputPathForSettingsFile = "<c:\>"

    $VaultSetingsFile = Get-AzureSiteRecoveryVaultSettingsFile -Location $VaultGeo -Name $VaultName -Path $OutputPathForSettingsFile;

    ```

2.  Vault raames määratud järgmised käsud:

    ```

    $VaultSettingFilePath = $vaultSetingsFile.FilePath
    $VaultContext = Import-AzureSiteRecoveryVaultSettingsFile -Path $VaultSettingFilePath -ErrorAction Stop

    ```

## <a name="step-4-install-the-azure-site-recovery-provider"></a>Samm 4: Installida Azure saidi taastamise pakkuja

1.  Arvutis VMM luua, käivitage järgmine käsk:

    ```

    pushd C:\ASR\

    ```

2.  Ekstrakti failid allalaaditud pakkuja järgmise käsu abil

    ```

    AzureSiteRecoveryProvider.exe /x:. /q

    ```

3.  Installige pakkuja kasutada järgmisi käske.

    ```

    .\SetupDr.exe /i

    ```

    ```

    $installationRegPath = "hklm:\software\Microsoft\Microsoft System Center Virtual Machine Manager Server\DRAdapter"
    do
    {
        $isNotInstalled = $true;
        if(Test-Path $installationRegPath)
        {
            $isNotInstalled = $false;
        }
    }While($isNotInstalled)

    ```

    Oodata installimise lõpuni.

4.  Registreerida serveri vault abil järgmine käsk:

    ```

    $BinPath = $env:SystemDrive+"\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin"
    pushd $BinPath
    $encryptionFilePath = "C:\temp\"
    .\DRConfigurator.exe /r /Credentials $VaultSettingFilePath /vmmfriendlyname $env:COMPUTERNAME /dataencryptionenabled $encryptionFilePath /startvmmservice

    ```

## <a name="step-5-create-an-azure-storage-account"></a>Juhis 5: Looge konto Azure salvestusruum

Kui teil pole kontot Azure storage, geo-dispersioonanalüüs lubatud konto loomine, käivitage järgmine käsk:

```

$StorageAccountName = "teststorageacc1"
$StorageAccountGeo  = "Southeast Asia"

New-AzureStorageAccount -StorageAccountName $StorageAccountName -Label $StorageAccountName -Location $StorageAccountGeo;

```

Pange tähele, et salvestusruumi konto peab olema sama piirkonna Azure saidi taastamise teenuse olema sama tellimusega seostatud.


## <a name="step-6-install-the-azure-recovery-services-agent"></a>Samm 6: Installida Azure taastamise teenused Agent

Azure portaali kaudu installida Azure taastamise teenused agendi iga Hyper-V hosti server asuvad VMM pilved, mida soovite kaitsta.

Kõik VMM hosts käivitage järgmine käsk:

```

marsagentinstaller.exe /q /nu

```


## <a name="step-7-configure-cloud-protection-settings"></a>Juhis 7: Konfigureerimine pilve sätted

1.  Pilveteenuse kaitse profiili Azure loomine, käivitage järgmine käsk:

    ```

    $ReplicationFrequencyInSeconds = "300";
    $ProfileResult = New-AzureSiteRecoveryProtectionProfileObject -ReplicationProvider HyperVReplica -RecoveryAzureSubscription $AzureSubscriptionName -RecoveryAzureStorageAccount $StorageAccountName -ReplicationFrequencyInSeconds  $ReplicationFrequencyInSeconds;

    ```

2.  Saada kaitse container käivitades järgmised käsud:

    ```

    $PrimaryCloud = "testcloud"
    $protectionContainer = Get-AzureSiteRecoveryProtectionContainer -Name $PrimaryCloud;

    ```

3.  Alusta kaitse-ümbrisest seost pilveteenuses:

    ```

    $associationJob = Start-AzureSiteRecoveryProtectionProfileAssociationJob -ProtectionProfile $profileResult -PrimaryProtectionContainer $protectionContainer;        

    ```

4.  Kui töö on lõpule jõudnud, käivitage järgmine käsk:


        $job = Get-AzureSiteRecoveryJob -Id $associationJob.JobId;
        if($job -eq $null -or $job.StateDescription -ne "Completed")
        {
        $isJobLeftForProcessing = $true;
        }


5.  Kui töö on lõpule jõudnud töötlemine, käivitage järgmine käsk:


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



Märkige ruut selle toimingu lõpuleviimist, järgige [Tegevuse jälgimine](#monitor).

## <a name="step-8-configure-network-mapping"></a>Samm 8: Konfigureerige võrgu vastendamine
Enne alustamist võrgu kaardistamine veenduge, et virtuaalmasinates VMM-i serverisse on ühendatud võrku VM. Lisaks saate luua ühe või mitme Azure virtuaalse võrgu. Pange tähele, et mitme VM võrgu saab vastendada ühe Azure võrgus.

Pange tähele, et kui target võrgus on mitu alamvõrku ja üks nende alamvõrku on alamvõrku, mis asub allika virtuaalse masina ja seejärel koopia virtuaalse masina ühendatakse selle target alamvõrgu Tõrkesiirde pärast sama nimi. Kui pole target alamvõrgu kattuvad nimega, virtuaalse masina ühendatud esimese alamvõrgu võrgus.

Esimese käsu saab serverid praeguse Azure'i saidi taastamise hoidla. Käsk salvestab Microsoft Azure saidi taastamise serverid $Servers massiivi muutujat.

    $Servers = Get-AzureSiteRecoveryServer


Teine käsk saab $Servers massiivi esimese serveri saidi taastamise võrku. Käsu talletab võrkude $Networks muutujana.


    $Networks = Get-AzureSiteRecoveryNetwork -Server $Servers[0]

Kolmas käsk saab Azure tellimuste Get-AzureSubscription cmdleti abil ja seejärel talletab selle väärtuse $Subscriptions muutujana.

    $Subscriptions = Get-AzureSubscription



Neljas käsk saab Azure'i $AzureVmNetworks muutuja Get-AzureVNetSite cmdlet-käsk ja seejärel selle väärtuse abil.


    $AzureVmNetworks = Get-AzureVNetSite



Lõplik cmdlet loob vastenduse esmane võrgu ja Azure virtuaalse masina võrgu vahel. Cmdlet määrab esmane võrgu $Networks esimene element. Cmdlet määrab virtuaalse masina võrgu $AzureVmNetworks esimese osa, kasutades oma ID-ga. Käsk sisaldab teie Azure tellimuse ID-ga.


    New-AzureSiteRecoveryNetworkMapping -PrimaryNetwork $Networks[0] -AzureSubscriptionId $Subscriptions[0].SubscriptionId -AzureVMNetworkId $AzureVmNetworks[0].Id


## <a name="step-9-enable-protection-for-virtual-machines"></a>Samm 9: Lubamine kaitse virtuaalmasinates

Pärast serverid pilved ja võrgu on õigesti konfigureeritud, saate lubada kaitse virtuaalmasinates pilveteenuses. Võtke arvesse järgmist.

Virtuaalmasinates peab vastama [Azure virtuaalse masina eeltingimused](site-recovery-best-practices.md#virtual-machines).

Kaitse lubamiseks operatsioonisüsteem ja operatsioonisüsteemi seatakse virtuaalse masina ketta atribuudid. Kui loote virtuaalse masina VMM virtuaalse masina malli abil saate atribuuti. Samuti saate nende atribuutide olemasolevaid virtuaalmasinates virtuaalse masina atribuudid vahekaartidel **üldist** ja **Riistvara konfiguratsiooni** . Kui te ei määra atribuutidest VMM kuvatakse saama need konfigureerida Azure saidi taastamise portaalis.



1.  Kaitse lubamiseks käivitage järgmine käsk saada kaitse container:

        $ProtectionContainer = Get-AzureSiteRecoveryProtectionContainer -Name $CloudName



2. Saada kaitse üksus (VM), käivitage järgmine käsk:


        $protectionEntity = Get-AzureSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $protectionContainer



3. Luba DR VM, käivitage järgmine käsk:



        $jobResult = Set-AzureSiteRecoveryProtectionEntity -ProtectionEntity $protectionEntity  -Protection Enable -Force



## <a name="test-your-deployment"></a>Testige oma juurutamine

Testimiseks käivitage test Tõrkesiirde jaoks ühe virtuaalse masina, või luua taastamise kava koosneb mitmest virtuaalmasinates ja käivitage test Tõrkesiirde lepingu raames juurutamise. Test Tõrkesiirde jäljendab oma Tõrkesiirde ja taastamise süsteem isoleeritakse võrgus. Pange tähele järgmist.

- Kui soovite pärast selle Tõrkesiirde kaugtöölaua kasutamise Azure virtuaalse masina ühenduse, lubada kaugtöölaua ühendus virtuaalse masina enne, kui käivitate test Tõrkesiirde.
- Pärast Tõrkesiirde, saate kasutada avaliku IP-aadressi ühenduse, virtuaalse masina Azure kaugtöölaua kasutamise. Kui soovite seda teha, veenduge, et teil pole mõnda domeeni poliitika, mis takistada ühenduse virtuaalse masina avalik aadressidel.

Märkige ruut selle toimingu lõpuleviimist, järgige [Tegevuse jälgimine](#monitor).

### <a name="create-a-recovery-plan"></a>Taastamis loomine

1. Looge XML-fail oma taastamise kava allpool andmete kasutamise mallina ja seejärel salvestage see "C:\RPTemplatePath.xml".
2. Saate muuta RecoveryPlan sõlm Id, nimi, PrimaryServerId ja SecondaryServerId.
3. Saate muuta ProtectionEntity sõlm PrimaryProtectionEntityId (vmid VMM kaudu).
4. Saate lisada rohkem VMs, lisades rohkem ProtectionEntity sõlmi.



        <#
        <?xml version="1.0" encoding="utf-16"?>
        <RecoveryPlan Id="d0323b26-5be2-471b-addc-0a8742796610" Name="rp-test"  PrimaryServerId="9350a530-d5af-435b-9f2b-b941b5d9fcd5"  SecondaryServerId="21a9403c-6ec1-44f2-b744-b4e50b792387" Description=""     Version="V2014_07">
          <Actions />
          <ActionGroups>
            <ShutdownAllActionGroup Id="ShutdownAllActionGroup">
              <PreActionSequence />
              <PostActionSequence />
            </ShutdownAllActionGroup>
            <FailoverAllActionGroup Id="FailoverAllActionGroup">
              <PreActionSequence />
              <PostActionSequence />
            </FailoverAllActionGroup>
            <BootActionGroup Id="DefaultActionGroup">
              <PreActionSequence />
              <PostActionSequence />
              <ProtectionEntity PrimaryProtectionEntityId="d4c8ce92-a613-4c63-9b03- cf163cc36ef8" />
            </BootActionGroup>
          </ActionGroups>
          <ActionGroupSequence>
            <ActionGroup Id="ShutdownAllActionGroup" ActionId="ShutdownAllActionGroup"  Before="FailoverAllActionGroup" />
            <ActionGroup Id="FailoverAllActionGroup" ActionId="FailoverAllActionGroup"  After="ShutdownAllActionGroup" Before="DefaultActionGroup" />
            <ActionGroup Id="DefaultActionGroup" ActionId="DefaultActionGroup" After="FailoverAllActionGroup"/>
          </ActionGroupSequence>
        </RecoveryPlan>
        #>



4. Sisestage malli andmeid:


        $TemplatePath = "C:\RPTemplatePath.xml";



5. Funktsiooni RecoveryPlan loomiseks tehke järgmist.

        $RPCreationJob = New-AzureSiteRecoveryRecoveryPlan -File $TemplatePath -WaitForCompletion;



### <a name="run-a-test-failover"></a>Käivitage test Tõrkesiirde

1.  Saada RecoveryPlan objekti, käivitage järgmine käsk:

        $RPObject = Get-AzureSiteRecoveryRecoveryPlan -Name $RPName;


2.  Test Tõrkesiirde käivitage järgmine käsk:


        $jobIDResult = Start-AzureSiteRecoveryTestFailoverJob -RecoveryPlan $RPObject -Direction PrimaryToRecovery;


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

[Lisateavet vt](https://msdn.microsoft.com/library/dn850420.aspx) Azure saidi taastamine PowerShelli cmdlet-käskude kohta. </a>.
