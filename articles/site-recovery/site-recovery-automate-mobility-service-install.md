<properties
    pageTitle="Paljundada VMware virtuaalmasinates Azure saidi taastamine kasutades Azure automatiseerimine DSC | Microsoft Azure'i"
    description="Kirjeldab, kuidas kasutada Azure automatiseerimine DSC automaatselt juurutada Azure saidi taastamine mobiilsus teenuse ja Azure agent virtuaalse/füüsilise masinad Azure."
    services="site-recovery"
    documentationCenter=""
    authors="krnese"
    manager="lorenr"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/26/2016"
    ms.author="krnese"/>

# <a name="replicate-vmware-virtual-machines-to-azure-by-using-site-recovery-with-azure-automation-dsc"></a>Paljundada VMware virtuaalmasinates Azure'i Azure automatiseerimine DSC saidi taastamise abil

Operations Management Suite, pakume teile Põhjalik varundamine ja mille abil saate oma tegevuse järjepidevuse kava osana katastroofi lahendus.

Me alustamine koos Hyper-V abil Hyper-V koopia. Kuid oleme laiendanud heterogeensete häälestamise toetama, kuna kliendid on mitu hypervisors ja platvormid nende pilved.

Kui kasutate VMware töökoormus ja/või füüsilise serveri täna, management server töötab kõik komponendid Azure saidi taastamise keskkonna Azure, side- ja kopeerimisest tegema, kui Azure on teie sihtkoht.

## <a name="deploy-the-site-recovery-mobility-service-by-using-automation-dsc"></a>Saidi taastamise mobiilsus teenuse abil automatiseerimise DSC juurutamine
Alustuseks vaatame, tehes kiirülevaate jaotus, mida teeb see management server.

Management server töötab mitu serverirollid. Üks rollidest on *konfiguratsiooni*, mis koordinaadid side ja haldab andmete kopeerimise ja taastamise abil.

Lisaks toimib *protsessi* roll dispersioonanalüüs lüüsi. Selle rolli saab dispersioonanalüüs kaitstud allika masinad, optimeerib vahemällu, tihendamise ja krüptimise ja siis saadab Azure storage konto. Üks protsessi rolli funktsioonid on ka push mobiilsus teenuse installimine kaitstud masinad ja automaatne tuvastamine VMware VMs tegemiseks.

Kui migreerite Azure, tegeleb *juhtslaidi target* roll andmete kopeerimine selle toimingu käigus.

Kaitstud seadmed me toetuvad *mobiilsus teenuse*. See osa on juurutatud iga seadme (Vmware'i VM või füüsiline server), mida soovite korrata Azure. See sisaldab andmeid kirjutab arvutis ja edastab need management server (protsessi roll).

Kui olete tegelevad järjepidevuse, on oluline mõista teie töökoormus, oma taristu ja seotud osade. Seejärel saate nõuetele taastamise aja eesmärk (RTO) ja taastamise punkti eesmärk (RPO). Sellega mobiilsus teenuse on oluline, et teie töökoormus on kaitstud, nagu ootate.

Aga kuidas saab, optimeeritud viisil tagada, et teil on usaldusväärne kaitstud häälestus teatud komponendid Operations Management Suite abiga?

Selles artiklis on toodud näide sellest, kuidas saate kasutada Azure automatiseerimine soovitud oleku konfiguratsioon (DSC), koos saidi taastamise, tagamaks, et:

- Mobiilsus teenuse ja Azure VM agent on juurutatud Windows seadmed, mida soovite kaitsta.
- Mobility teenus ja Azure VM agent alati töötab Azure'i dispersioonanalüüs target korral.

## <a name="prerequisites"></a>Eeltingimused

- Hoidla salvestada vajalik häälestus
- Hoidla salvestada nõutav parool management serveri registreerima

 > [AZURE.NOTE] Iga management serveri jaoks luuakse kordumatu parool. Kui te ei kavatse juurutada mitme management serverid, tuleb tagada, et õige parool on salvestatud passphrase.txt faili.

- Windows Management Framework (WMF) 5.0 installitud arvutites, mis te soovite lubada kaitse (nõue automatiseerimise DSC)

 > [AZURE.NOTE] Kui soovite kasutada Windowsi DSC masinad, mis on installitud WMF 4.0, leiate jaotisest [Kasutada DSC lahti keskkonnas](#Use DSC in disconnected environments).

Mobiilsus teenuse installimist käsurea kaudu ja aktsepteerib mitut argumenti. Seetõttu tuleb teil on kahendfaile (pärast ekstraktimine häälestust) ja salvestaks need kohas, kus saate kuulata neid DSC konfiguratsiooni abil.

## <a name="step-1-extract-binaries"></a>Samm 1: Ekstrakti kahendfaile

1. Ekstrakti failid, et peate selle setup, liikuge oma management serveri järgmisse kausta:

    **\Microsoft Azure'i saidi Recovery\home\svsystems\pushinstallsvc\repository**

    Selles kaustas, peaksite nägema MSI-faili, nimega:

    **Microsoft-ASR_UA_version_Windows_GA_date_Release.exe**

    Kasutage installiprogrammi eraldada järgmine käsk:

    **.\Microsoft-ASR_UA_9.1.0.0_Windows_GA_02May2016_release.exe /q /x:C:\Users\Administrator\Desktop\Mobility_Service\Extract**

2. Valige kõik failid ja saatke neile tihendatud (zip) kausta.

Nüüd on teil kahendfaile, et teil on vaja mobiilsus teenuse häälestamise automatiseerimise DSC abil automatiseerida.

### <a name="passphrase"></a>Parooli

Järgmiseks peate kindlaks teha, kuhu soovite selle tihendatud kaust. Saate kasutada Azure storage konto, nagu on näidatud hiljem parool, mida vajate häälestamise talletamiseks. Seejärel registreeruma agent management serveri käigus.

Parool, mida teil management Serveri juurutamisel saab salvestada tekstifaili passphrase.txt.

Viige sihtotstarbeline ümbrises Azure storage konto nii tihendatud kaust ja parool.

![Kausta asukoht](./media/site-recovery-automate-mobilitysevice-install/folder-and-passphrase-location.png)

Kui te ei soovi säilitada need failid teie võrgus ühiskasutatav, saate seda teha. Soovite lihtsalt veenduge, et DSC ressurss, mida te kasutate hiljem on juurdepääs ja saan häälestamise ja parool.

## <a name="step-2-create-the-dsc-configuration"></a>Samm 2: Looge DSC konfigureerimine

Häälestamise sõltub WMF 5.0. Seadme konfigureerimise kaudu automatiseerimise DSC edukalt rakendamiseks, WMF 5.0 peab olema Esita.

Keskkonna kasutab järgmine näide DSC konfiguratsioon:

```powershell
configuration ASRMobilityService {

    $RemoteFile = 'https://knrecstor01.blob.core.windows.net/asr/ASR.zip'
    $RemotePassphrase = 'https://knrecstor01.blob.core.windows.net/asr/passphrase.txt'
    $RemoteAzureAgent = 'http://go.microsoft.com/fwlink/p/?LinkId=394789'
    $LocalAzureAgent = 'C:\Temp\AzureVmAgent.msi'
    $TempDestination = 'C:\Temp\asr.zip'
    $LocalPassphrase = 'C:\Temp\Mobility_service\passphrase.txt'
    $Role = 'Agent'
    $Install = 'C:\Program Files (x86)\Microsoft Azure Site Recovery'
    $CSEndpoint = '10.0.0.115'
    $Arguments = '/Role "{0}" /InstallLocation "{1}" /CSEndpoint "{2}" /PassphraseFilePath "{3}"' -f $Role,$Install,$CSEndpoint,$LocalPassphrase

    Import-DscResource -ModuleName xPSDesiredStateConfiguration

    node localhost {

        File Directory {
            DestinationPath = 'C:\Temp\ASRSetup\'
            Type = 'Directory'            
        }

        xRemoteFile Setup {
            URI = $RemoteFile
            DestinationPath = $TempDestination
            DependsOn = '[File]Directory'
        }

        xRemoteFile Passphrase {
            URI = $RemotePassphrase
            DestinationPath = $LocalPassphrase
            DependsOn = '[File]Directory'
        }

        xRemoteFile AzureAgent {
            URI = $RemoteAzureAgent
            DestinationPath = $LocalAzureAgent
            DependsOn = '[File]Directory'
        }

        Archive ASRzip {
            Path = $TempDestination
            Destination = 'C:\Temp\ASRSetup'
            DependsOn = '[xRemotefile]Setup'
        }

        Package Install {
            Path = 'C:\temp\ASRSetup\ASR\UNIFIEDAGENT.EXE'
            Ensure = 'Present'
            Name = 'Microsoft Azure Site Recovery mobility Service/Master Target Server'
            ProductId = '275197FC-14FD-4560-A5EB-38217F80CBD1'
            Arguments = $Arguments
            DependsOn = '[Archive]ASRzip'
        }

        Package AzureAgent {
            Path = 'C:\Temp\AzureVmAgent.msi'
            Ensure = 'Present'
            Name = 'Windows Azure VM Agent - 2.7.1198.735'
            ProductId = '5CF4D04A-F16C-4892-9196-6025EA61F964'
            Arguments = '/q /l "c:\temp\agentlog.txt'
            DependsOn = '[Package]Install'
        }

        Service ASRvx {
            Name = 'svagents'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service ASR {
            Name = 'InMage Scout Application Service'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service AzureAgentService {
            Name = 'WindowsAzureGuestAgent'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }

        Service AzureTelemetry {
            Name = 'WindowsAzureTelemetryService'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }
    }
}
```
Konfiguratsiooni tuleb teha järgmist:

- Muutujate ütleb konfiguratsiooni kust kahendfaile mobiilsus teenuse ja Azure VM agent, kust parool ja salvestuskoha väljund.
- Konfiguratsiooni impordib xPSDesiredStateConfiguration DSC ressurss, nii et saate kasutada `xRemoteFile` failide allalaadimiseks hoidla.
- Konfiguratsiooni loob kaust, kuhu soovite talletada kahendfaile.
- Ressursi arhiivi failid väljavõte ZIP kaust.
- Paketi installi ressursi installib mobiilsus teenuse kaudu soovitud UNIFIEDAGENT. EXE installer teatud argumendiga. (Muutujaid, mis ehitada argumendid tuleb et see vastaks teie keskkonnas.)
- Paketi AzureAgent ressursi installida Azure VM agent, mis on soovitatav iga VM, mis töötab Azure. Azure'i VM agent ka võimaldab lisada laiendid VM pärast Tõrkesiirde.
- Service ressursi või ressursse tagab Mobility teenuste ja Azure teenuste alati töötab.

Konfiguratsiooni salvestamine **ASRMobilityService**.

>[AZURE.NOTE] Ärge unustage CSIP konfiguratsioonis kajastamiseks tegelik management server, et agent õigesti ühendatud ja kasutate õiget parooli.

## <a name="step-3-upload-to-automation-dsc"></a>Samm 3: Üleslaadimine DSC automatiseerimine

Kuna tehtud DSC konfiguratsiooni import nõutav DSC ressursi mooduli (xPSDesiredStateConfiguration), peate enne üleslaadimist DSC konfiguratsiooni selle mooduli automaatika importida.

Automatiseerimise kontosse sisse logida, liikuge sirvides **varad** > **moodulid**ja klõpsake nuppu **Sirvi Galerii**.

Siin saate otsida mooduli ja importimine oma kontole.

![Impordi moodul](./media/site-recovery-automate-mobilitysevice-install/search-and-import-module.png)

Kui olete lõpetanud, see, minge oma arvuti, kuhu on installitud Azure'i ressursihaldur moodulid ja jätkake importimine vastloodud DSC konfigureerimine.

### <a name="import-cmdlets"></a>Impordi cmdlet-käsud

Azure'i tellimuse sisselogimiseks PowerShellis. Keskkonna ja automatiseerimise muutujat konto teavet cmdlet-käskude muutmiseks tehke järgmist.

```powershell
$AAAccount = Get-AzureRmAutomationAccount -ResourceGroupName 'KNOMS' -Name 'KNOMSAA'
```

Konfiguratsiooni üleslaadimine automatiseerimise DSC abil järgmine cmdlet-käsk:

```powershell
$ImportArgs = @{
    SourcePath = 'C:\ASR\ASRMobilityService.ps1'
    Published = $true
    Description = 'DSC Config for Mobility Service'
}
$AAAccount | Import-AzureRmAutomationDscConfiguration @ImportArgs
```

### <a name="compile-the-configuration-in-automation-dsc"></a>Automaatika DSC konfiguratsiooni kompileerida

Järgmiseks peate kompileerida automatiseerimise DSC, konfiguratsiooni nii, et saate alustada sõlmed selle registreerimiseks. Mida teil saavutada käitades järgmine cmdlet-käsk:

```powershell
$AAAccount | Start-AzureRmAutomationDscCompilationJob -ConfigurationName ASRMobilityService
```

See võib võtta mõne minuti, kuna juurutamist põhiliselt konfiguratsiooni majutatud DSC pull teenusega.

Pärast saate koostada konfiguratsiooni, saate hankida töö teavet PowerShelli (Get-AzureRmAutomationDscCompilationJob) abil või [Azure portaali](https://portal.azure.com/).

![Töö tuua](./media/site-recovery-automate-mobilitysevice-install/retrieve-job.png)

Teil on nüüd edukalt avaldatud ja automatiseerimise DSC konfiguratsioonist DSC üles laadida.

## <a name="step-4-onboard-machines-to-automation-dsc"></a>Samm 4: Pardal masinad DSC automatiseerimine
>[AZURE.NOTE] Üks eeltingimused lõpuleviimine sel juhul on, et teie Windowsi masinad värskendatakse WMF uusima versiooniga. Saate alla laadida ja installida õige versiooni teie platvorm [Allalaadimiskeskuse](https://www.microsoft.com/download/details.aspx?id=50395)kaudu.

Nüüd loote mõnda metaconfig DSC, et rakendate oma sõlmed jaoks. See õnnestub, peate tuua URL-i lõpp-punkti ja automatiseerimise valitud konto Azure primaarvõti. Need väärtused jaotises **klahvid** **kõiki sätteid** enne automatiseerimise konto jaoks leiate.

![Väärtused](./media/site-recovery-automate-mobilitysevice-install/key-values.png)

Selles näites on teil on Windows Server 2012 R2 füüsiline server, mida soovite saidi taastamise abil kaitsta.

### <a name="check-for-any-pending-file-rename-operations-in-the-registry"></a>Märkige ruut mis tahes faili ümbernimetamiseks toiminguid registris ootel

Enne alustamist automatiseerimise DSC lõpp-punkti serveri seostada, soovitame, et saate otsida mis tahes faili ümbernimetamiseks toiminguid registris ootel. Need võivad keelata häälestamise lõpetamine tõttu taaskäivitamist.

Käivitage järgmine cmdlet-veenduge, et server pole taaskäivitamist.

```powershell
Get-ItemProperty 'HKLM:\SYSTEM\CurrentControlSet\Control\Session Manager\' | Select-Object -Property PendingFileRenameOperations
```
Kui kuvatakse tühi, siis on jätkamiseks OK. Kui ei, peate pöörduma, taaskäivitus server hoolduse ajal.

Rakendada konfiguratsiooni serveris, käivitage teenuse PowerShell integreeritud skriptimise keskkonnas (ISE) ja käivitage järgmine skript. See on põhiosas DSC kohaliku konfiguratsiooni juhendavad kohalik Configuration Manager mootori automatiseerimise DSC teenuse registreerida ja tuua konfiguratsiooni (ASRMobilityService.localhost).

```powershell
[DSCLocalConfigurationManager()]
configuration metaconfig {
    param (
        $URL,
        $Key
    )
    node localhost {
        Settings {
            RefreshFrequencyMins = '30'
            RebootNodeIfNeeded = $true
            RefreshMode = 'PULL'
            ActionAfterReboot = 'ContinueConfiguration'
            ConfigurationMode = 'ApplyAndMonitor'
            AllowModuleOverwrite = $true
        }

        ResourceRepositoryWeb AzureAutomationDSC {
            ServerURL = $URL
            RegistrationKey = $Key
        }

        ConfigurationRepositoryWeb AzureAutomationDSC {
            ServerURL = $URL
            RegistrationKey = $Key
            ConfigurationNames = 'ASRMobilityService.localhost'
        }

        ReportServerWeb AzureAutomationDSC {
            ServerURL = $URL
            RegistrationKey = $Key
        }
    }
}
metaconfig -URL 'https://we-agentservice-prod-1.azure-automation.net/accounts/<YOURAAAccountID>' -Key '<YOURAAAccountKey>'

Set-DscLocalConfigurationManager .\metaconfig -Force -Verbose
```

Selle konfiguratsiooni põhjustab kohalik Configuration Manager mootori registreerima automatiseerimise DSC. Samuti määrab, kuidas peaks toimima mootori, mida ta peaks tegema, kui seal on konfiguratsiooni drift (ApplyAndAutoCorrect) ja kuidas see peaks jätkata konfiguratsiooni, kui on vaja uuesti.

Pärast seda skripti sõlme peaks algama automatiseerimise DSC registreerida.

![Sõlm registreerimine pooleli.](./media/site-recovery-automate-mobilitysevice-install/register-node.png)

Kui lähete Azure portaali, saate vaadata registreeritud sõlm on nüüd ilmus portaalis.

![Registreeritud sõlm portaalis](./media/site-recovery-automate-mobilitysevice-install/registered-node.png)

Server, käivitage järgmine PowerShelli cmdlet-käsk sõlme õigesti registreeritud kinnitamiseks:

```powershell
Get-DscLocalConfigurationManager
```

Pärast konfiguratsiooni on tõmmatakse ja rakendatud server, saate seda kontrollida, käitades järgmine cmdlet-käsk:

```powershell
Get-DscConfigurationStatus
```

Väljund näitab, kas server on edukalt tõmmatakse selle konfiguratsioon.

![Väljund](./media/site-recovery-automate-mobilitysevice-install/successful-config.png)

Lisaks mobiilsus teenuse install on oma Logi, mis võib leida *SystemDrive*\ProgramData\ASRSetupLogs.

See on õige. Teil on nüüd edukalt juurutatud ja registreeritud mobiilsus teenuse arvutisse, mida soovite saidi taastamise abil kaitsta. DSC veenduge, et vajalikud teenused töötavad alati.

![Juurutamise](./media/site-recovery-automate-mobilitysevice-install/successful-install.png)

Pärast management server tuvastab juurutamise, saate konfigureerida kaitse ja lubada kopeerimine seadmesse saidi taastamise abil.

## <a name="use-dsc-in-disconnected-environments"></a>Kasutage DSC lahti keskkonnas

Kui teie masinad pole Interneti-ühendus, saate siiski toetuvad DSC juurutada ja konfigureerida mobiilsus teenuse töökoormus, mida soovite kaitsta.

Saate lähtestada oma DSC pull serverisse esitada põhiosas sama võimalusi, mille saate automatiseerimise DSC teie keskkonnas. Mis on kliendid tõmbab DSC lõpp-punkti konfiguratsiooni (kui see on registreeritud). Teine võimalus on käsitsi push DSC konfigureerimine oma masinad kohalikult või kaugühendusega.

Pange tähele, et selles näites on lisatud parameeter arvuti nimi. Nüüd asub remote failide remote ühiskasutus, mis peaks olema juurdepääs masinad, mida soovite kaitsta. Skripti lõppu jõustab konfiguratsiooni ja seejärel hakkab eesmärk arvuti DSC konfiguratsiooni rakendada.

### <a name="prerequisites"></a>Eeltingimused

Veenduge, et xPSDesiredStateConfiguration PowerShelli moodul on installitud. Windowsi masinad, kuhu on installitud WMF 5.0, saate installida xPSDesiredStateConfiguration mooduli käitades järgmine cmdlet-käsk target arvutites.

```powershell
Find-Module -Name xPSDesiredStateConfiguration | Install-Module
```

Saate alla laadida ja salvestada mooduli juhuks, kui teil on vaja jagada seda Windowsi masinad, mis on WMF 4.0. Käivitage see cmdlet arvutisse, kus viibib PowerShellGet (WMF 5.0):

```powershell
Save-Module -Name xPSDesiredStateConfiguration -Path <location>
```

Ka WMF 4.0, veenduge, et [Windows 8.1 värskendamine KB2883200](https://www.microsoft.com/download/details.aspx?id=40749) on installitud masinad.

Pärast konfiguratsioon saate lükata Windowsi masinad, mis on WMF 5.0 ja WMF 4.0:

```powershell
configuration ASRMobilityService {
    param (
        [Parameter(Mandatory=$true)]
        [ValidateNotNullOrEmpty()]
        [System.String] $ComputerName
    )

    $RemoteFile = '\\myfileserver\share\asr.zip'
    $RemotePassphrase = '\\myfileserver\share\passphrase.txt'
    $RemoteAzureAgent = '\\myfileserver\share\AzureVmAgent.msi'
    $LocalAzureAgent = 'C:\Temp\AzureVmAgent.msi'
    $TempDestination = 'C:\Temp\asr.zip'
    $LocalPassphrase = 'C:\Temp\Mobility_service\passphrase.txt'
    $Role = 'Agent'
    $Install = 'C:\Program Files (x86)\Microsoft Azure Site Recovery'
    $CSEndpoint = '10.0.0.115'
    $Arguments = '/Role "{0}" /InstallLocation "{1}" /CSEndpoint "{2}" /PassphraseFilePath "{3}"' -f $Role,$Install,$CSEndpoint,$LocalPassphrase

    Import-DscResource -ModuleName xPSDesiredStateConfiguration

    node $ComputerName {      
        File Directory {
            DestinationPath = 'C:\Temp\ASRSetup\'
            Type = 'Directory'            
        }

        xRemoteFile Setup {
            URI = $RemoteFile
            DestinationPath = $TempDestination
            DependsOn = '[File]Directory'
        }

        xRemoteFile Passphrase {
            URI = $RemotePassphrase
            DestinationPath = $LocalPassphrase
            DependsOn = '[File]Directory'
        }

        xRemoteFile AzureAgent {
            URI = $RemoteAzureAgent
            DestinationPath = $LocalAzureAgent
            DependsOn = '[File]Directory'
        }

        Archive ASRzip {
            Path = $TempDestination
            Destination = 'C:\Temp\ASRSetup'
            DependsOn = '[xRemotefile]Setup'
        }

        Package Install {
            Path = 'C:\temp\ASRSetup\ASR\UNIFIEDAGENT.EXE'
            Ensure = 'Present'
            Name = 'Microsoft Azure Site Recovery mobility Service/Master Target Server'
            ProductId = '275197FC-14FD-4560-A5EB-38217F80CBD1'
            Arguments = $Arguments
            DependsOn = '[Archive]ASRzip'
        }

        Package AzureAgent {
            Path = 'C:\Temp\AzureVmAgent.msi'
            Ensure = 'Present'
            Name = 'Windows Azure VM Agent - 2.7.1198.735'
            ProductId = '5CF4D04A-F16C-4892-9196-6025EA61F964'
            Arguments = '/q /l "c:\temp\agentlog.txt'
            DependsOn = '[Package]Install'
        }

        Service ASRvx {
            Name = 'svagents'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service ASR {
            Name = 'InMage Scout Application Service'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service AzureAgentService {
            Name = 'WindowsAzureGuestAgent'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }

        Service AzureTelemetry {
            Name = 'WindowsAzureTelemetryService'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }
    }
}
ASRMobilityService -ComputerName 'MyTargetComputerName'

Start-DscConfiguration .\ASRMobilityService -Wait -Force -Verbose
```

Kui soovite oma DSC pull serverisse teie ettevõtte võrgus võimalusi, mida saate automatiseerimise DSC jäljendamiseks väärtustada, lugege teemat [häälestamise DSC pull veebiserverisse](https://msdn.microsoft.com/powershell/dsc/pullserver?f=255&MSPPError=-2147217396).

## <a name="optional-deploy-a-dsc-configuration-by-using-an-azure-resource-manager-template"></a>Valikuline: Juurutamine DSC konfigureerimine on Azure ressursihaldur malli abil

Selles artiklis on suunatud kuidas saate luua oma DSC konfigureerimine automaatselt mobiilsus teenuse ja Azure VM Agent--juurutada ja veenduge, et need töötavad arvutites, mida soovite kaitsta. Meil on Azure ressursihaldur malli, mida selle DSC konfiguratsiooni juurutama uue või olemasoleva Azure automatiseerimine konto. Mall kasutab sisendparameetrite automatiseerimise varad keskkonna muutujat sisaldavate loomiseks.

Pärast juurutamist malli, saate lihtsalt viidata samm 4 järgmist juhendit pardal oma masinad.

Mall tuleb teha järgmist:

1. Kasutage automatiseerimise konto või looge uus
2. Tehke sisendparameetrite.
    - ASRRemoteFile--asukoht, kuhu on talletatud mobiilsus teenuse install
    - ASRPassphrase--asukoht, kuhu on talletatud passphrase.txt fail
    - ASRCSEndpoint--management serveri IP-aadress
3. XPSDesiredStateConfiguration PowerShelli mooduli importimine
4. Luua ja koostada DSC konfigureerimine

Eelmist toimingut ilmneb nii, et saate alustada kasutuselevõtt teie masinad kaitse õiges järjestuses.

Malli juhised juurutuse asub [github](https://github.com/krnese/AzureDeploy/tree/master/OMS/MSOMS/DSC).

Malli PowerShelli abil:

```powershell
$RGDeployArgs = @{
    Name = 'DSC3'
    ResourceGroupName = 'KNOMS'
    TemplateFile = 'https://raw.githubusercontent.com/krnese/AzureDeploy/master/OMS/MSOMS/DSC/azuredeploy.json'
    OMSAutomationAccountName = 'KNOMSAA'
    ASRRemoteFile = 'https://knrecstor01.blob.core.windows.net/asr/ASR.zip'
    ASRRemotePassphrase = 'https://knrecstor01.blob.core.windows.net/asr/passphrase.txt'
    ASRCSEndpoint = '10.0.0.115'
    DSCJobGuid = [System.Guid]::NewGuid().ToString()
}
New-AzureRmResourceGroupDeployment @RGDeployArgs -Verbose
```

## <a name="next-steps"></a>Järgmised sammud

Kui juurutate Mobility teenuse agentide, saate selle virtuaalmasinates [lubada kopeerimine](site-recovery-vmware-to-azure.md#step-6-replicate-applications) .
