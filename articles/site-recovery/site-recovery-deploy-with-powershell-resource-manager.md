<properties
    pageTitle="Kaitsta serverid Azure'i Azure ressursihaldur Azure PowerShelli abil | Microsoft Azure'i"
    description="PowerShelli ja Azure ressursihaldur abil automatiseerida kaitse serverite Azure'i Azure saidi taastamine."
    services="site-recovery"
    documentationCenter=""
    authors="bsiva"
    manager="abhiag"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="backup-recovery"
    ms.date="09/27/2016"
    ms.author="bsiva"/>

# <a name="replicate-between-on-premises-hyper-v-virtual-machines-and-azure-by-using-powershell-and-azure-resource-manager"></a>Kohapealse Hyper-V virtuaalmasinates ja Azure korrata, kasutades PowerShelli ja Azure ressursihaldur

> [AZURE.SELECTOR]
- [Azure'i portaal](site-recovery-hyper-v-site-to-azure.md)
- [PowerShelli - ressursihaldur](site-recovery-deploy-with-powershell-resource-manager.md)
- [Klassikaline portaal](site-recovery-hyper-v-site-to-azure-classic.md)



## <a name="overview"></a>Ülevaade

Azure'i saidi taastamine aitab teie ettevõtte järjepidevus ja avariijärgne taastamine strateegia orkesteroinnin dispersioonanalüüs, Tõrkesiirde ja taastamise virtuaalmasinates juurutamise stsenaariumide arvu. Juurutamise stsenaariumide täieliku loendi leiate [Azure'i saidi taastamine ülevaade](site-recovery-overview.md).

Azure'i PowerShelli on leiate Azure'i kaudu Windows PowerShelli cmdlet-käskude. On võimalik töötada kahte tüüpi: Azure'i profiili mooduli või Azure ressursihaldur mooduli.

Saidi taastamise PowerShelli cmdlet-käskude, saadaval Azure PowerShelli jaoks Azure'i ressursihaldur, aitavad teil kaitsta ja taastada oma Azure serverites.

Selles artiklis kirjeldatakse, kuidas kasutada Windows PowerShelli koos Azure ressursihaldur, juurutada, konfigureerida ja korraldab serveri kaitse Azure saidi taastamine. Näiteks kasutada selles artiklis kirjeldatakse, kuidas kaitsta, ei õnnestu üle ja taastada virtuaalmasinates Hyper-V hosti Azure, Azure ressursihaldur Azure PowerShelli abil.

> [AZURE.NOTE] Saidi taastamise PowerShelli cmdlet-käskude praegu võimaldavad teil konfigureerida järgmisi: üks virtuaalse masina Manager saidi teise, virtuaalse masina halduri saidi Azure ja Azure Hyper-V saidi.

Te ei pea PowerShelli expert seda artiklit, kuid peate aru põhimõtted, nt moodulid, cmdlet-käskude ja seansid. Windows PowerShelli kohta leiate lisateavet teemast [Windows PowerShelli töötamise alustamine](http://technet.microsoft.com/library/hh857337.aspx).

Samuti saate lugeda Lisateavet [Azure PowerShelli kasutamine Azure ressursihaldur](../powershell-azure-resource-manager.md)kohta.

> [AZURE.NOTE] Microsoft Partner, mis on osa programmi pilve lahenduse pakkuja (CSP) saate konfigureerida ja hallata klientide serverite klientide vastav CSP tellimuste (rentniku tellimused) kaitse.

## <a name="before-you-start"></a>Enne alustamist

Veenduge, et teil on nende eeltingimuste kohas.

- [Microsoft Azure'i](https://azure.microsoft.com/) konto. Saate alustada [tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/). Lisaks saate lugeda [Azure saidi taastamine Manager hindade](https://azure.microsoft.com/pricing/details/site-recovery/)kohta.
- Azure'i PowerShelli 1.0. See väljaanne ja kuidas seda installida kohta leiate teavet teemast [Azure PowerShelli 1.0.](https://azure.microsoft.com/)
- [AzureRM.SiteRecovery](https://www.powershellgallery.com/packages/AzureRM.SiteRecovery/) ja [AzureRM.RecoveryServices](https://www.powershellgallery.com/packages/AzureRM.RecoveryServices/) moodulid. Saate uusimad versioonid neid mooduleid [PowerShelli Galerii](https://www.powershellgallery.com/)

Selles artiklis iseloomustab Azure PowerShelli kasutamine Azure ressursihaldur konfigureerimine ja hallata oma serverite kaitse. Näiteks kasutada selles artiklis kirjeldatakse, kuidas kaitsta virtuaalse masina, töötab Hyper-V hosti, Azure. Selles näites on järgmine kohustuslik tarkvara. Põhjalikum kogumi nõuded erinevate saidi taastamise stsenaariumid, vaadake selle stsenaariumi seotud dokumente.

- Hyper-V server töötab Windows Server 2012 R2 või Microsoft Hyper-V Server 2012 R2, mis sisaldab ühe või mitme virtuaalmasinates.
- Hyper-V serverite Internetiga ühendatud, otseselt või puhverserveri.
- Virtuaalmasinates, mida soovite kaitsta peavad vastama [virtuaalse masina eeltingimused](site-recovery-best-practices.md#virtual-machines).


## <a name="step-1-sign-in-to-your-azure-account"></a>Samm 1: Azure'i kontosse sisselogimine


1. Avage PowerShelli konsooli ja selle käsu Azure'i kontosse sisse logida. Cmdlet tabeliveergude veebilehele, mis palub teil oma konto identimisteave.

        Login-AzureRmAccount

    Vaheldumisi, võite lisada ka oma konto identimisteave parameetrina soovitud `Login-AzureRmAccount` cmdlet-käsu abil soovitud `-Credential` parameeter.

    Kui olete CSP partneri töötamise rentniku nimel, määrata kliendi rentniku tenantID või rentniku esmane domeeninime abil.

        Login-AzureRmAccount -Tenant "fabrikam.com"

2. Konto võib olla mitu tellimust, et seostate tellimus, mida soovite kasutada konto.

        Select-AzureRmSubscription -SubscriptionName $SubscriptionName

3.  Veenduge, et teie tellimus on registreeritud Azure pakkujate kasutamiseks taastamise teenused ja saidi taastamine, kasutades järgmised käsud:

    - `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.RecoveryServices`
    -  `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.SiteRecovery`

    Need käsud väljund, kui **RegistrationState** on seatud **registreeritud**, jätkake juhisega 2. Kui ei, te peate end puuduvad pakkuja teie tellimus.

    Azure'i pakkuja Registreeruge saidi taastamine ja taastamise teenused, käivitage järgmine käsk:

        Register-AzureRmResourceProvider -ProviderNamespace Microsoft.SiteRecovery
        Register-AzureRmResourceProvider -ProviderNamespace Microsoft.RecoveryServices

    Veenduge, et pakkujate registreeritud edukalt, kasutades järgmised käsud: `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.RecoveryServices` ja `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.SiteRecovery`.



## <a name="step-2-set-up-the-recovery-services-vault"></a>Samm 2: Häälestamine vault taastamise teenused

1. Azure'i ressursihaldur ressursirühm, kus teil tuleb vault loomine või saate kasutada olemasolevat ressursi rühma loomine. Järgmise käsu abil saate luua uue ressursirühma:

        New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Geo  

    kus $ResourceGroupName muutuja sisaldab loodava ressursirühma nimi ja $Geo muutuja Azure piirkond, kus soovite luua ressursirühm "(nt Brasiilia South).

    Hankida soovitud loendi tellimuse, kasutades funktsiooni `Get-AzureRmResourceGroup` cmdlet-käsk.

2. Loo uus Azure taastamise teenused vault järgmiselt:

        $vault = New-AzureRmRecoveryServicesVault -Name <string> -ResourceGroupName <string> -Location <string>

    Saate alla laadida olemasoleva võlvid loendit, kasutades funktsiooni `Get-AzureRmRecoveryServicesVault` cmdlet-käsk.

> [AZURE.NOTE] Kui soovite saidi taastamine võlvid abil loodud klassikaline portaali või Azure'i teenuse haldus PowerShelli mooduli toiminguid, saate otsida selline võlvid loendit, kasutades funktsiooni `Get-AzureRmSiteRecoveryVault` cmdlet-käsk. Peaksite looma uue taastamise teenused vault kõigi uute toimingute. Saidi taastamine võlvid varem loodud on toetatud, kuid pole uusimaid funktsioone.

## <a name="step-3-set-the-recovery-services-vault-context"></a>Samm 3: Määrake taastamise teenused vault kontekst

1.  Vault raames määratud käivitage järgmine käsk:

        Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault

## <a name="step-4-create-a-hyper-v-site-and-generate-a-new-vault-registration-key-for-the-site"></a>Samm 4: Hyper-V saidi loomine ja luua uus vault registreerimise võti selle saidi jaoks.

1. Hyper-V uue saidi loomine järgmiselt:

        $sitename = "MySite"                #Specify site friendly name
        New-AzureRmSiteRecoverySite -Name $sitename

    Selle cmdlet-käsu käivitab saidi taastamise töö saidi loomiseks ja tagastab saidi taastamise töö objekti. Oodake täita ja veenduge, et töö töö lõpule viidud.

    Saate tuua töö objekti ja seetõttu olekut praeguse töö, kasutades cmdleti Get-AzureRmSiteRecoveryJob.
2. Luua ja laadige registreerimise võti selle saidi jaoks järgmiselt:

        $SiteIdentifier = Get-AzureRmSiteRecoverySite -Name $sitename | Select -ExpandProperty SiteIdentifier
        Get-AzureRmRecoveryServicesVaultSettingsFile -Vault $vault -SiteIdentifier $SiteIdentifier -SiteFriendlyName $sitename -Path $Path

    Kopeerige allalaaditud võti Hyper-V server. Teil on vaja registreerida Hyper-V server saidile võti.

## <a name="step-5-install-the-azure-site-recovery-provider-and-azure-recovery-services-agent-on-your-hyper-v-host"></a>Juhis 5: Installida Azure saidi taastamise pakkuja ja Azure taastamise Services Agent oma Hyper-V Server

1. [Microsofti](https://aka.ms/downloaddra)saidilt alla laadida installer pakkuja uusim versioon.

2. Jätkake registreerimise Käivita installer oma Hyper-V hosti ja installimise lõpus.

3. Küsimise korral sisestage allalaaditud saidi registreerimise võti ja täielik registreerimine Hyper-V hosti saidile.

4. Veenduge, et Hyper-V server on registreeritud saidile abil järgmine käsk:

        $server =  Get-AzureRmSiteRecoveryServer -FriendlyName $server-friendlyname

## <a name="step-6-create-a-replication-policy-and-associate-it-with-the-protection-container"></a>Samm 6: Dispersioonanalüüs poliitika loomine ja seostada kaitse ümbris

1. Dispersioonanalüüs poliitika loomine järgmiselt:

        $ReplicationFrequencyInSeconds = "300";     #options are 30,300,900
        $PolicyName = “replicapolicy”
        $Recoverypoints = 6                 #specify the number of recovery points
        $storageaccountID = Get-AzureRmStorageAccount -Name "mystorea" -ResourceGroupName "MyRG" | Select -ExpandProperty Id

        $PolicyResult = New-AzureRmSiteRecoveryPolicy -Name $PolicyName -ReplicationProvider “HyperVReplicaAzure” -ReplicationFrequencyInSeconds $ReplicationFrequencyInSeconds  -RecoveryPoints $Recoverypoints -ApplicationConsistentSnapshotFrequencyInHours 1 -RecoveryAzureStorageAccountId $storageaccountID

    Märkige ruut tagastatud töö tagamiseks, et dispersioonanalüüs poliitika loomine õnnestus.

    >[AZURE.IMPORTANT] Määratud salvestusruumi konto peaks olema sama Azure piirkonna oma taastamise teenused vault ja peab olema lubatud geo-dispersioonanalüüs.
    >
    > - Kui tüüp Azure Storage (klassikaline) on määratud salvestusruumi tagasi võtta, taastada Tõrkesiirde kaitstud masinad masina Azure'i IaaS (klassikaline).
    > - Kui tüüp Azure Storage (Azure'i ressursihaldur) on määratud salvestusruumi tagasi võtta, taastada Tõrkesiirde kaitstud masinad masina Azure'i IaaS (Azure'i ressursihaldur).

2. Saada kaitse ümbris vastava saidi järgmiselt:

        $protectionContainer = Get-AzureRmSiteRecoveryProtectionContainer
3.  Alusta kaitse-ümbrisest seost dispersioonanalüüs poliitika, järgmiselt:

        $Policy = Get-AzureRmSiteRecoveryPolicy -FriendlyName $PolicyName
        $associationJob  = Start-AzureRmSiteRecoveryPolicyAssociationJob -Policy $Policy -PrimaryProtectionContainer $protectionContainer

    Oodake, kuni seos töö lõpuleviimiseks ja veenduge, et edukalt lõpule.

##<a name="step-7-enable-protection-for-virtual-machines"></a>Juhis 7: Lubamine kaitse virtuaalmasinates

1. Saada kaitse üksuse vastav VM, mida soovite kaitsta järgmiselt:

        $VMFriendlyName = "Fabrikam-app"                    #Name of the VM
        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -ProtectionContainer $protectionContainer -FriendlyName $VMFriendlyName

2. Käivitage kaitsmine virtuaalse masina, järgmiselt:

        $Ostype = "Windows"                                 # "Windows" or "Linux"
        $DRjob = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionEntity -Policy $Policy -Protection Enable -RecoveryAzureStorageAccountId $storageaccountID  -OS $OStype -OSDiskName $protectionEntity.Disks[0].Name

    >[AZURE.IMPORTANT] Määratud salvestusruumi konto peaks olema sama Azure piirkonna oma taastamise teenused vault ja peab olema lubatud geo-dispersioonanalüüs.
    >
    > - Kui tüüp Azure Storage (klassikaline) on määratud salvestusruumi tagasi võtta, taastada Tõrkesiirde kaitstud masinad masina Azure'i IaaS (klassikaline).
    > - Kui tüüp Azure Storage (Azure'i ressursihaldur) on määratud salvestusruumi tagasi võtta, taastada Tõrkesiirde kaitstud masinad masina Azure'i IaaS (Azure'i ressursihaldur).

    > Kui teil kaitsta VM on manustatud rohkem kui üks ketas, määrake operatsioonisüsteemi ketas *OSDiskName* parameetri abil.

3. Oodake, kuni virtuaalmasinates pärast algse dispersioonanalüüs kaitstud olekusse saavutamiseks. See võib võtta aega, sõltuvalt teguritest nagu andmehulga järgitav ja Azure varustava läbilaskevõime. Töö oleku ja StateDescription värskendatakse korral jõuda kaitstud olekusse VM järgmiselt.

        PS C:\> $DRjob = Get-AzureRmSiteRecoveryJob -Job $DRjob

        PS C:\> $DRjob | Select-Object -ExpandProperty State
        Succeeded

        PS C:\> $DRjob | Select-Object -ExpandProperty StateDescription
        Completed

4. Värskendage taastamine atribuudid, näiteks VM rolli suurus ja Azure võrgu virtuaalse masina võrgu kasutajaliidese kaartide abil Tõrkesiirde manustamiseks.

        PS C:\> $nw1 = Get-AzureRmVirtualNetwork -Name "FailoverNw" -ResourceGroupName "MyRG"

        PS C:\> $VMFriendlyName = "Fabrikam-App"

        PS C:\> $VM = Get-AzureRmSiteRecoveryVM -ProtectionContainer $protectionContainer -FriendlyName $VMFriendlyName

        PS C:\> $UpdateJob = Set-AzureRmSiteRecoveryVM -VirtualMachine $VM -PrimaryNic $VM.NicDetailsList[0].NicId -RecoveryNetworkId $nw1.Id -RecoveryNicSubnetName $nw1.Subnets[0].Name

        PS C:\> $UpdateJob = Get-AzureRmSiteRecoveryJob -Job $UpdateJob

        PS C:\> $UpdateJob


        Name             : b8a647e0-2cb9-40d1-84c4-d0169919e2c5
        ID               : /Subscriptions/a731825f-4bf2-4f81-a611-c331b272206e/resourceGroups/MyRG/providers/Microsoft.RecoveryServices/vault
                           s/MyVault/replicationJobs/b8a647e0-2cb9-40d1-84c4-d0169919e2c5
        Type             : Microsoft.RecoveryServices/vaults/replicationJobs
        JobType          : UpdateVmProperties
        DisplayName      : Update the virtual machine
        ClientRequestId  : 805a22a3-be86-441c-9da8-f32685673112-2015-12-10 17:55:51Z-P
        State            : Succeeded
        StateDescription : Completed
        StartTime        : 10-12-2015 17:55:53 +00:00
        EndTime          : 10-12-2015 17:55:54 +00:00
        TargetObjectId   : 289682c6-c5e6-42dc-a1d2-5f9621f78ae6
        TargetObjectType : ProtectionEntity
        TargetObjectName : Fabrikam-App
        AllowedActions   : {Restart}
        Tasks            : {UpdateVmPropertiesTask}
        Errors           : {}



## <a name="step-8-run-a-test-failover"></a>Samm 8: Käivitage test Tõrkesiirde

1. Käivitage test Tõrkesiirde töö, järgmiselt:

        $nw = Get-AzureRmVirtualNetwork -Name "TestFailoverNw" -ResourceGroupName "MyRG" #Specify Azure vnet name and resource group

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -FriendlyName $VMFriendlyName -ProtectionContainer $protectionContainer

        $TFjob = Start-AzureRmSiteRecoveryTestFailoverJob -ProtectionEntity $protectionEntity -Direction PrimaryToRecovery -AzureVMNetworkId $nw.Id

2. Veenduge, et test VM luuakse Azure. (Testi Tõrkesiirde töö peatatakse, loomise katse VM Azure järel. Töö lõpulejõudmist puhastamine üles loodud esemete korral jätkata tööd, nagu on näidatud järgmises toimingus.)

3. Täitke test Tõrkesiirde, järgmiselt:

        $TFjob = Resume-AzureRmSiteRecoveryJob -Job $TFjob


##<a name="next-steps"></a>Järgmised sammud

[Lisateavet vt](https://msdn.microsoft.com/library/azure/mt637930.aspx) Azure saidi taastamise ja Azure ressursihaldur PowerShelli cmdlet-käskude kohta.
