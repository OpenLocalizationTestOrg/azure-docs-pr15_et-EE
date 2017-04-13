<properties
    pageTitle="Konfigureerimine alati kättesaadavus rühma Azure VM PowerShelli abil"
    description="Selle õpetuse kasutab ressursse, mis on loodud mudeliga klassikaline juurutamise ja kasutab PowerShelli Azure alati klõpsake kättesaadavus rühma loomiseks."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="MikeRayMSFT"
    manager="jhubbard"
    editor=""
    tags="azure-service-management" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/22/2016"
    ms.author="mikeray" />

# <a name="configure-always-on-availability-group-in-azure-vm-with-powershell"></a>Konfigureerimine alati kättesaadavus rühma Azure VM PowerShelli abil

> [AZURE.SELECTOR]
- [Ressursihaldur: Mall](virtual-machines-windows-portal-sql-alwayson-availability-groups.md)
- [Ressursihaldur: käsitsi](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md)
- [Klassikaline: kasutajaliides](virtual-machines-windows-classic-portal-sql-alwayson-availability-groups.md)
- [Klassikaline: PowerShelli](virtual-machines-windows-classic-ps-sql-alwayson-availability-groups.md)

<br/>

> [AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

Andmebaasi administraatorid madalam rakendada kõrge kättesaadavus SQL Server süsteemi maksumus aitab Azure'i virtuaalmasinates (VMs). Selle õpetuse näidatakse, kuidas rakendada kättesaadavus rühma abil SQL serveri alati kohta –-lõpuni sees Azure keskkonnas. Õpetuse lõpus oma SQL serveri alati klõpsake lahenduse Azure koosneb järgmisi elemente:

- Mitu alamvõrku, sh on ees- ja tagaandmebaas alamvõrgu sisaldavad virtuaalse võrgus

- Domeenikontrolleri Active Directory (AD) domeeniga

- Kahe SQL serveri VMs tagaandmebaas alamvõrgu juurutatud ja ühendatud AD domeeni

- 3-sõlm WSFC kobar mudeliga sõlm enamik kvoorumi

- Kättesaadavus rühma kaks sünkroonse Kinnita koopiad andmebaasi kättesaadavus

See stsenaarium on valitud selle lihtsa, pole selle maksumus tõhusust või muud tegurid Azure jaoks. Näiteks saate minimeerida arvu kahe-koopia kättesaadavus rühma VMs salvestama Arvuta tundi Azure domeenikontrolleri kvoorumi failide ühiskasutuse haldur 2 sõlme WSFC kobar abil. See meetod vähendab ühte ülaltoodud konfiguratsiooni loendus VM.

Selles õpetuses on mõeldud kuvamiseks häälestama eespool kirjeldatud lahendus ilma täpsemad üksikasjad iga toimingu juhiseid. Seetõttu asemel kuvatud GUI konfigureerimistoimingute, kasutab PowerShelli skriptimise teid kiiresti üksikasjalikke juhiseid. Eeldab järgmist:

- Teil on juba konto Azure virtuaalse masina tellimus.

- Olete installinud [Azure PowerShelli cmdlet-käsud](../powershell-install-configure.md).

- Teil on juba ühtlase mõistmine alati klõpsake kättesaadavus rühmad kohapealne lahendustele. Lisateavet leiate teemast [Alati klõpsake kättesaadavus rühmade (SQL Server)](https://msdn.microsoft.com/library/hh510230.aspx).

## <a name="connect-to-your-azure-subscription-and-create-the-virtual-network"></a>Ühenduse loomine Azure tellimuse ja virtuaalse võrgu loomine

1. Kohalikus arvutis PowerShelli aknas importimine Azure mooduli, avaldamise sätted faili oma arvutisse alla laadida ja ühendage seansi PowerShelli Azure tellimuse importimise allalaaditud avaldamise sätted.

        Import-Module "C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\Azure\Azure.psd1"
        Get-AzurePublishSettingsFile
        Import-AzurePublishSettingsFile <publishsettingsfilepath>

    Käsk **Get-AzurePublishgSettingsFile** põhjal automaatselt genereerida haldamiseks serdi Azure allalaadimine see oma arvutisse. Brauseris avatakse automaatselt ja teil palutakse sisestada Microsofti konto identimisteave Azure tellimuse. Allalaaditud **.publishsettings** fail sisaldab kõik andmed, mida vajate oma Azure tellimuse haldamiseks. Pärast faili salvestamist kohalikku kausta, importige see käsk **Impordi-AzurePublishSettingsFile** .

    >[AZURE.NOTE] Publishsettings fail sisaldab mandaat (kodeerimata), mida kasutatakse teie Azure'i tellimused ja teenuste haldamine. Selle faili parim turvalisus on salvestatakse ajutiselt väljaspool teie andmeallika kataloogide (nt kaustas Libraries\Documents) ja seejärel kustutage, kui importimine on lõpule viidud. Pahatahtlik kasutaja omandada juurdepääsu publishsettings faili redigeerimine, loomine ja kustutamine Azure'i teenuste.

1. Määratleda sarja muutujaid, mida kasutate oma pilvepõhise IT taristu loomiseks.

        $location = "West US"
        $affinityGroupName = "ContosoAG"
        $affinityGroupDescription = "Contoso SQL HADR Affinity Group"
        $affinityGroupLabel = "IaaS BI Affinity Group"
        $networkConfigPath = "C:\scripts\Network.netcfg"
        $virtualNetworkName = "ContosoNET"
        $storageAccountName = "<uniquestorageaccountname>"
        $storageAccountLabel = "Contoso SQL HADR Storage Account"
        $storageAccountContainer = "https://" + $storageAccountName + ".blob.core.windows.net/vhds/"
        $winImageName = (Get-AzureVMImage | where {$_.Label -like "Windows Server 2008 R2 SP1*"} | sort PublishedDate -Descending)[0].ImageName
        $sqlImageName = (Get-AzureVMImage | where {$_.Label -like "SQL Server 2012 SP1 Enterprise*"} | sort PublishedDate -Descending)[0].ImageName
        $dcServerName = "ContosoDC"
        $dcServiceName = "<uniqueservicename>"
        $availabilitySetName = "SQLHADR"
        $vmAdminUser = "AzureAdmin"
        $vmAdminPassword = "Contoso!000"
        $workingDir = "c:\scripts\"

    Pöörake tähelepanu järgmistele tagamaks, et oma käsud õnnestub hiljem:

    - Muutujate **$storageAccountName** ja **$dcServiceName** peab olema kordumatu, kuna neid kasutatakse tuvastamiseks vastavalt teie pilvepõhise salvestusruumi konto ja pilveteenuse server, Interneti-ühendusega arvutid.

    - Määratud muutujate **$affinityGroupName** ja **$virtualNetworkName** nimed on konfigureeritud virtuaalse võrgu konfigureerimise dokument, mida te kasutate hiljem.

    - **$sqlImageName** määratleb värskendatud nime VM pilt, mis sisaldab SQL Server 2012 Service Pack 1 Enterprise Edition.

    - Lihtsa, **Contoso! 000** on kasutusel kogu laadida sama parool.

1. Osaleja rühma loomine.

        New-AzureAffinityGroup `
            -Name $affinityGroupName `
            -Location $location `
            -Description $affinityGroupDescription `
            -Label $affinityGroupLabel

1. Luua virtuaalse võrgu konfigureerimise faili importimisega.

        Set-AzureVNetConfig `
            -ConfigurationPath $networkConfigPath

    Otsingukonfiguratsiooni fail sisaldab järgmist XML-dokumendi. Lühidalt, seda määrab virtuaalse võrgust, mida nimetatakse **ContosoNET** osaleja jaotises nimega **ContosoAG**ja see on aadress ruumi **10.10.0.0/16** ja on kaks alamvõrku, **10.10.1.0/24** ja **10.10.2.0/24**, mis on ees alamvõrgu ja tagasi alamvõrgu vastavalt. Eesmise alamvõrgu on kui asetate klientrakendustes nagu Microsoft SharePointi ja tagasi alamvõrgu on kui asetate SQL serveri VMs. Kui muudate **$affinityGroupName** ja **$virtualNetworkName** muutujate varasemas versioonis, peate muutma ka allpool vastavad nimed.

        <NetworkConfiguration xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
          <VirtualNetworkConfiguration>
            <Dns />
            <VirtualNetworkSites>
              <VirtualNetworkSite name="ContosoNET" AffinityGroup="ContosoAG">
                <AddressSpace>
                  <AddressPrefix>10.10.0.0/16</AddressPrefix>
                </AddressSpace>
                <Subnets>
                  <Subnet name="Front">
                    <AddressPrefix>10.10.1.0/24</AddressPrefix>
                  </Subnet>
                  <Subnet name="Back">
                    <AddressPrefix>10.10.2.0/24</AddressPrefix>
                  </Subnet>
                </Subnets>
              </VirtualNetworkSite>
            </VirtualNetworkSites>
          </VirtualNetworkConfiguration>
        </NetworkConfiguration>

1. Mis on seotud osaleja rühma olete loonud ja selle seadmine praeguse salvestusruumi konto teie tellimus salvestusruumi konto loomine.

        New-AzureStorageAccount `
            -StorageAccountName $storageAccountName `
            -Label $storageAccountLabel `
            -AffinityGroup $affinityGroupName
        Set-AzureSubscription `
            -SubscriptionName (Get-AzureSubscription).SubscriptionName `
            -CurrentStorageAccount $storageAccountName

1. Näiteks Põhiliselt server luua uue pilvepõhise teenuse ja -saadavus määramine.

        New-AzureVMConfig `
            -Name $dcServerName `
            -InstanceSize Medium `
            -ImageName $winImageName `
            -MediaLocation "$storageAccountContainer$dcServerName.vhd" `
            -DiskLabel "OS" |
            Add-AzureProvisioningConfig `
                -Windows `
                -DisableAutomaticUpdates `
                -AdminUserName $vmAdminUser `
                -Password $vmAdminPassword |
                New-AzureVM `
                    -ServiceName $dcServiceName `
                    –AffinityGroup $affinityGroupName `
                    -VNetName $virtualNetworkName

    Selle sarja veetorustiku käskude teha järgmisi toiminguid:

    - **Uus-AzureVMConfig** loob VM konfigureerimine.

    - **Lisa-AzureProvisioningConfig** annab autonoomse Windows Serveri konfiguratsiooni parameetrid.

    - **Lisa-AzureDataDisk** lisab Active Directory andmete vahemälu suvand puudub, salvestamiseks kasutavate andmed kettale.

    - **Uus-AzureVM** loob uue pilveteenuses ja loob uue Azure VM uue pilveteenuses.

1. Oodake, kuni uus VM täielikult ette valmistatud ja oma tööd kataloogi Remote'i töölaua faili alla laadida. Kuna uue Azure VM võtab kaua aega sätet, on aega pidev jätkuvalt uue VM küsitlus, kuni see on kasutamiseks valmis.

        $VMStatus = Get-AzureVM -ServiceName $dcServiceName -Name $dcServerName

        While ($VMStatus.InstanceStatus -ne "ReadyRole")
        {
            write-host "Waiting for " $VMStatus.Name "... Current Status = " $VMStatus.InstanceStatus
            Start-Sleep -Seconds 15
            $VMStatus = Get-AzureVM -ServiceName $dcServiceName -Name $dcServerName
        }

        Get-AzureRemoteDesktopFile `
            -ServiceName $dcServiceName `
            -Name $dcServerName `
            -LocalPath "$workingDir$dcServerName.rdp"

Näiteks Põhiliselt server on nüüd ette valmistatud. Järgmiseks tuleb konfigureerida Active Directory domeeni, näiteks Põhiliselt selles serveris. PowerShelli aken avatuks jätta oma kohalikus arvutis. Kuvatakse selle hiljem uuesti abil luua kaks SQL serveri VMs.

## <a name="configure-the-domain-controller"></a>Selle domeenikontrolleri konfigureerimine

1. Ühenduse näiteks Põhiliselt serveri käivitades Remote'i töölaua fail. Kasutage arvutisse administraatori AzureAdmin kasutajanime ja parooli **Contoso! 000**, mis teie määratud, kui loote uue VM.

1. Avage administraatoriõigustes PowerShelli aken.

1. Käivitage järgmine **DCPROMO. EXE** käsk **corp.contoso.com** domeeni andmete kataloogide kettadraivi M häälestamine.

        dcpromo.exe `
            /unattend `
            /ReplicaOrNewDomain:Domain `
            /NewDomain:Forest `
            /NewDomainDNSName:corp.contoso.com `
            /ForestLevel:4 `
            /DomainNetbiosName:CORP `
            /DomainLevel:4 `
            /InstallDNS:Yes `
            /ConfirmGc:Yes `
            /CreateDNSDelegation:No `
            /DatabasePath:"C:\Windows\NTDS" `
            /LogPath:"C:\Windows\NTDS" `
            /SYSVOLPath:"C:\Windows\SYSVOL" `
            /SafeModeAdminPassword:"Contoso!000"

    Kui käsk on lõpule jõudnud, VM taaskäivitamist automaatselt.

1. Ühenduse näiteks Põhiliselt server uuesti käivitades Remote'i töölaua fail. Seekord **CORP\Administrator**sisse logima.

1. Avage administraatoriõigustes PowerShelli aken ja importimine Active Directory PowerShelli mooduli kasutamise järgmine käsk:

        Import-Module ActiveDirectory

1. Käivitage järgmised käsud domeeni kolme kasutajaid lisada.

        $pwd = ConvertTo-SecureString "Contoso!000" -AsPlainText -Force
        New-ADUser `
            -Name 'Install' `
            -AccountPassword  $pwd `
            -PasswordNeverExpires $true `
            -ChangePasswordAtLogon $false `
            -Enabled $true
        New-ADUser `
            -Name 'SQLSvc1' `
            -AccountPassword  $pwd `
            -PasswordNeverExpires $true `
            -ChangePasswordAtLogon $false `
            -Enabled $true
        New-ADUser `
            -Name 'SQLSvc2' `
            -AccountPassword  $pwd `
            -PasswordNeverExpires $true `
            -ChangePasswordAtLogon $false `
            -Enabled $true

    **CORP\Install** saab seadistada kõik seotud SQL Serveri teenuse eksemplari, WSFC kobar ja kättesaadavus rühm. **CORP\SQLSvc1** ja **CORP\SQLSvc2** kasutatakse SQL Serveri teenuse kontode kaks SQL serveri vms.

1. Järgmisena käivitage järgmised käsud **CORP\Install** loomiseks arvuti objektide Domeen õigusi anda.

        Cd ad:
        $sid = new-object System.Security.Principal.SecurityIdentifier (Get-ADUser "Install").SID
        $guid = new-object Guid bf967a86-0de6-11d0-a285-00aa003049e2
        $ace1 = new-object System.DirectoryServices.ActiveDirectoryAccessRule $sid,"CreateChild","Allow",$guid,"All"
        $corp = Get-ADObject -Identity "DC=corp,DC=contoso,DC=com"
        $acl = Get-Acl $corp
        $acl.AddAccessRule($ace1)
        Set-Acl -Path "DC=corp,DC=contoso,DC=com" -AclObject $acl

    Eespool nimetatud GUID on GUID arvuti objekti tüüp. **CORP\Install** konto peab **Lugemine kõik atribuudid** ja **Luua arvuti objektide** õiguste loomiseks aktiivse otsese objektide jaoks WSFC kobar. **Lugege kõik atribuudid** õigus on juba antud CORP\Install vaikimisi nii, et teil pole vaja selgesõnaliselt anda. Õigusi on vaja luua WSFC kobar kohta leiate lisateavet teemast [Tõrkesiirde kobar üksikasjalik juhend: Active Directory kontod konfigureerida](https://technet.microsoft.com/library/cc731002%28v=WS.10%29.aspx).

    Nüüd, kui olete lõpetanud Active Directory ja kasutajale objektid, saate luua kaks SQL serveri VMs ja selle domeeni liituma.

## <a name="create-the-sql-server-vms"></a>SQL serveri VMs loomine

1. Edasi kasutada PowerShelli aknas avatud kohalikus arvutis. Järgmised täiendavad muutujad määratlemine

        $domainName= "corp"
        $FQDN = "corp.contoso.com"
        $subnetName = "Back"
        $sqlServiceName = "<uniqueservicename>"
        $quorumServerName = "ContosoQuorum"
        $sql1ServerName = "ContosoSQL1"
        $sql2ServerName = "ContosoSQL2"
        $availabilitySetName = "SQLHADR"
        $dataDiskSize = 100
        $dnsSettings = New-AzureDns -Name "ContosoBackDNS" -IPAddress "10.10.0.4"

    IP address **10.10.0.4** on tavaliselt määratud esimese VM **10.10.0.0/16** alamvõrku Azure virtuaalse võrgu loomist. Kontrollige see on näiteks Põhiliselt serveri aadress, käitades **IPCONFIG**.

1. Käivitage esimene VM loomist WSFC kobar, nimega **ContosoQuorum**veetorustiku järgmised käsud:

        New-AzureVMConfig `
            -Name $quorumServerName `
            -InstanceSize Medium `
            -ImageName $winImageName `
            -MediaLocation "$storageAccountContainer$quorumServerName.vhd" `
            -AvailabilitySetName $availabilitySetName `
            -DiskLabel "OS" |
            Add-AzureProvisioningConfig `
                -WindowsDomain `
                -AdminUserName $vmAdminUser `
                -Password $vmAdminPassword `
                -DisableAutomaticUpdates `
                -Domain $domainName `
                -JoinDomain $FQDN `
                -DomainUserName $vmAdminUser `
                -DomainPassword $vmAdminPassword |
                Set-AzureSubnet `
                    -SubnetNames $subnetName |
                    New-AzureVM `
                        -ServiceName $sqlServiceName `
                        –AffinityGroup $affinityGroupName `
                        -VNetName $virtualNetworkName `
                        -DnsSettings $dnsSettings

    Pange tähele ülaltoodud käsu kohta järgmist:

    - **Uus-AzureVMConfig** loob VM konfiguratsiooni soovitud kättesaadavus hulga nimi. Edaspidised VMs luuakse samanimelist kättesaadavus määramine, et nad liidetud kättesaadavus samu.

    - **Lisa-AzureProvisioningConfig** ühendab VM loodud Active Directory domeeni.

    - **Set-AzureSubnet** paigutab VM alamvõrgu tagasi.

    - **Uus-AzureVM** loob uue pilveteenuses ja loob uue Azure VM uue pilveteenuses. Parameetri **DnsSettings** saate määrata, kas DNS-i server serverite uue pilveteenuses on selle IP address **10.10.0.4**, mis on näiteks Põhiliselt serveri IP-aadress. See parameeter on vaja uue VMs pilveteenusesse Active Directory domeeni edukalt liituma. Selle parameetri ilma peate käsitsi seadmine IPv4 sätted oma VM kasutada näiteks Põhiliselt serveri esmane DNS-i server pärast VM on ette valmistatud ja seejärel Liitu VM Active Directory domeeni.

1. Käivitage järgmised käsud veetorustiku loomine SQL serveri VMs nimega **ContosoSQL1** ja **ContosoSQL2**.

        # Create ContosoSQL1...
        New-AzureVMConfig `
            -Name $sql1ServerName `
            -InstanceSize Large `
            -ImageName $sqlImageName `
            -MediaLocation "$storageAccountContainer$sql1ServerName.vhd" `
            -AvailabilitySetName $availabilitySetName `
            -HostCaching "ReadOnly" `
            -DiskLabel "OS" |
            Add-AzureProvisioningConfig `
                -WindowsDomain `
                -AdminUserName $vmAdminUser `
                -Password $vmAdminPassword `
                -DisableAutomaticUpdates `
                -Domain $domainName `
                -JoinDomain $FQDN `
                -DomainUserName $vmAdminUser `
                -DomainPassword $vmAdminPassword |
                Set-AzureSubnet `
                    -SubnetNames $subnetName |
                    Add-AzureEndpoint `
                        -Name "SQL" `
                        -Protocol "tcp" `
                        -PublicPort 1 `
                        -LocalPort 1433 |
                        New-AzureVM `
                            -ServiceName $sqlServiceName

        # Create ContosoSQL2...
        New-AzureVMConfig `
            -Name $sql2ServerName `
            -InstanceSize Large `
            -ImageName $sqlImageName `
            -MediaLocation "$storageAccountContainer$sql2ServerName.vhd" `
            -AvailabilitySetName $availabilitySetName `
            -HostCaching "ReadOnly" `
            -DiskLabel "OS" |
            Add-AzureProvisioningConfig `
                -WindowsDomain `
                -AdminUserName $vmAdminUser `
                -Password $vmAdminPassword `
                -DisableAutomaticUpdates `
                -Domain $domainName `
                -JoinDomain $FQDN `
                -DomainUserName $vmAdminUser `
                -DomainPassword $vmAdminPassword |
                Set-AzureSubnet `
                    -SubnetNames $subnetName |
                    Add-AzureEndpoint `
                        -Name "SQL" `
                        -Protocol "tcp" `
                        -PublicPort 2 `
                        -LocalPort 1433 |
                        New-AzureVM `
                            -ServiceName $sqlServiceName

    Võtke arvesse järgmist ülaltoodud käsud.

    - **New-AzureVMConfig** kasutab sama kättesaadavus hulga nimi on näiteks Põhiliselt server ja kasutab SQL Server 2012 Service Pack 1 Enterprise Edition pildi virtuaalse masina galeriis. Seda määrab ka operatsioonisüsteemi ketas lugemine vahemällu ainult (pole kirjutada vahemällu). Soovitatav on andmebaasi failide migreerimine andmeid eraldi kettale saate manustada VM ja konfigureeritakse ei loe või vahemällu. Järgmine parim asi aga eemaldada kirjutada vahemällu kettal operatsioonisüsteemi, kuna te ei saa eemaldada lugeda vahemällu kettal operatsioonisüsteemi.

    - **Lisa-AzureProvisioningConfig** ühendab VM loodud Active Directory domeeni.

    - **Set-AzureSubnet** paigutab VM alamvõrgu tagasi.

    - **Lisa-AzureEndpoint** lisab Accessi lõpp-punktid klientrakendustes pääseks juurde SQL serveri teenuste juhul Internetis. Erinevate pordid on antud ContosoSQL1 ja ContosoSQL2.

    - **Uus-AzureVM** loob uue SQL serveri VM ContosoQuorum nimega sama pilveteenuses. VMs peab paigutamiseks sama pilveteenuses, kui soovite, et neid samu kättesaadavus.

1. Oodake, kuni iga VM täielikult ette valmistatud ja selle serveri töölaua faili allalaadimiseks töötamise kataloogi. Funktsiooni tsükkel tsüklit kuni kolm uut VMs ja käivitab neid ülataseme kõverate sulgudes olevad käsud.

        Foreach ($VM in $VMs = Get-AzureVM -ServiceName $sqlServiceName)
        {
            write-host "Waiting for " $VM.Name "..."

            # Loop until the VM status is "ReadyRole"
            While ($VM.InstanceStatus -ne "ReadyRole")
            {
                write-host "  Current Status = " $VM.InstanceStatus
                Start-Sleep -Seconds 15
                $VM = Get-AzureVM -ServiceName $VM.ServiceName -Name $VM.InstanceName
            }

            write-host "  Current Status = " $VM.InstanceStatus

            # Download remote desktop file
            Get-AzureRemoteDesktopFile -ServiceName $VM.ServiceName -Name $VM.InstanceName -LocalPath "$workingDir$($VM.InstanceName).rdp"
        }

    SQL serveri VMs on nüüd ette valmistatud ja töötab, kuid installitakse SQL serveri vaikesuvandite.

## <a name="initialize-the-wsfc-cluster-vms"></a>WSFC kobar VMs lähtestamine

Selles jaotises peate muutma kolme serverid kasutate WSFC kobar ja SQL Serveri install. Täpsemalt:

- (Kõik serverid) Peate installima **Tõrkesiirdeklastrite** funktsioon.

- (Kõik serverid) Peate lisama **CORP\Install** arvutisse **administraatori poole**.

- (ContosoSQL1 ja ContosoSQL2 ainult) Peate lisama **CORP\Install** nimega **süsteemiadministraator** rolli vaikimisi andmebaasis.

- (ContosoSQL1 ja ContosoSQL2 ainult) Peate lisama **NT AUTHORITY\System** nimega login järgmisi õigusi:

    - Mis tahes kättesaadavus rühma muuta

    - Ühenduse loomine SQL-i

    - Kuva serveri olek

- (ContosoSQL1 ja ContosoSQL2 ainult) SQL serveri VM on juba lubatud **TCP** -protokolli. Siiski peate siiski avada SQL serveri Kaugpöördusteenuse tulemüüri.

Nüüd olete valmis alustama. Alates **ContosoQuorum**, tehke järgmist.

1. Ühenduse **ContosoQuorum** käivitades on kaugtöölaua failid. Kasutage arvutisse administraatori **AzureAdmin** kasutajanime ja parooli **Contoso! 000**, mille määrasite VMs loomisel.

1. Veenduge, et arvutid on edukalt liitunud **corp.contoso.com**.

1. Oodake, kuni SQL Serveri installimise lõpuleviimiseks töötab automaatse lähtestamise toimingud enne jätkamist.

1. Avage administraatoriõigustes PowerShelli aken.

1. Installige Windows tõrkesiirdeklastrite funktsioon.

        Import-Module ServerManager
        Add-WindowsFeature Failover-Clustering

1. Lisage **CORP\Install** kohaliku administraatorina.

        net localgroup administrators "CORP\Install" /Add

1. Logige välja ContosoQuorum. Olete selle serveri nüüd teha.

        logoff.exe

Järgmiseks lähtestada **ContosoSQL1** ja **ContosoSQL2**. Järgige alltoodud juhiseid, mis on täpselt ühesugused nii SQL serveri VMs.

1. Ühenduse loomine kahe SQL serveri VMs käivitades on kaugtöölaua failid. Kasutage arvutisse administraatori **AzureAdmin** kasutajanime ja parooli **Contoso! 000**, mille määrasite VMs loomisel.

1. Veenduge, et arvutid on edukalt liitunud **corp.contoso.com**.

1. Oodake, kuni SQL Serveri installimise lõpuleviimiseks töötab automaatse lähtestamise toimingud enne jätkamist.

1. Avage administraatoriõigustes PowerShelli aken.

1. Installige Windows tõrkesiirdeklastrite funktsioon.

        Import-Module ServerManager
        Add-WindowsFeature Failover-Clustering

1. Lisage **CORP\Install** kohaliku administraatorina

        net localgroup administrators "CORP\Install" /Add

1. SQL serveri PowerShelli pakkuja importida.

        Set-ExecutionPolicy -Execution RemoteSigned -Force
        Import-Module -Name "sqlps" -DisableNameChecking

1. Lisage **CORP\Install** süsteemiadministraator rolli jaoks vaikimisi SQL serveri eksemplar.

        net localgroup administrators "CORP\Install" /Add
        Invoke-SqlCmd -Query "EXEC sp_addsrvrolemember 'CORP\Install', 'sysadmin'" -ServerInstance "."

1. **NT AUTHORITY\System** kuuluva login kolm eespool kirjeldatud õigused.

        Invoke-SqlCmd -Query "CREATE LOGIN [NT AUTHORITY\SYSTEM] FROM WINDOWS" -ServerInstance "."
        Invoke-SqlCmd -Query "GRANT ALTER ANY AVAILABILITY GROUP TO [NT AUTHORITY\SYSTEM] AS SA" -ServerInstance "."
        Invoke-SqlCmd -Query "GRANT CONNECT SQL TO [NT AUTHORITY\SYSTEM] AS SA" -ServerInstance "."
        Invoke-SqlCmd -Query "GRANT VIEW SERVER STATE TO [NT AUTHORITY\SYSTEM] AS SA" -ServerInstance "."

1. Avage tulemüüri Kaugpöördusteenuse SQL serveri jaoks.

        netsh advfirewall firewall add rule name='SQL Server (TCP-In)' program='C:\Program Files\Microsoft SQL Server\MSSQL11.MSSQLSERVER\MSSQL\Binn\sqlservr.exe' dir=in action=allow protocol=TCP

1. Logige välja nii VMs.

        logoff.exe

Lõpuks, kui olete valmis kättesaadavus rühma konfigureerida. Kasutate SQL serveri PowerShelli pakkuja **ContosoSQL1**kõik tööd teha.

## <a name="configure-the-availability-group"></a>Kättesaadavus rühma konfigureerimine

1. Ühenduse **ContosoSQL1** uuesti käivitades on kaugtöölaua failid. Asemel arvutikonto abil sisselogimist **CORP\Install**sisse logima.

1. Avage administraatoriõigustes PowerShelli aken.

1. Järgmiste muutujate määratlemine

        $server1 = "ContosoSQL1"
        $server2 = "ContosoSQL2"
        $serverQuorum = "ContosoQuorum"
        $acct1 = "CORP\SQLSvc1"
        $acct2 = "CORP\SQLSvc2"
        $password = "Contoso!000"
        $clusterName = "Cluster1"
        $timeout = New-Object System.TimeSpan -ArgumentList 0, 0, 30
        $db = "MyDB1"
        $backupShare = "\\$server1\backup"
        $quorumShare = "\\$server1\quorum"
        $ag = "AG1"

1. SQL serveri PowerShelli pakkuja importida.

        Set-ExecutionPolicy RemoteSigned -Force
        Import-Module "sqlps" -DisableNameChecking

1. Muuta SQL Serveri teenuse konto jaoks ContosoSQL1 CORP\SQLSvc1.

        $wmi1 = new-object ("Microsoft.SqlServer.Management.Smo.Wmi.ManagedComputer") $server1
        $wmi1.services | where {$_.Type -eq 'SqlServer'} | foreach{$_.SetServiceAccount($acct1,$password)}
        $svc1 = Get-Service -ComputerName $server1 -Name 'MSSQLSERVER'
        $svc1.Stop()
        $svc1.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Stopped,$timeout)
        $svc1.Start();
        $svc1.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Running,$timeout)

1. Muuta SQL Serveri teenuse konto jaoks ContosoSQL2 CORP\SQLSvc2.

        $wmi2 = new-object ("Microsoft.SqlServer.Management.Smo.Wmi.ManagedComputer") $server2
        $wmi2.services | where {$_.Type -eq 'SqlServer'} | foreach{$_.SetServiceAccount($acct2,$password)}
        $svc2 = Get-Service -ComputerName $server2 -Name 'MSSQLSERVER'
        $svc2.Stop()
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Stopped,$timeout)
        $svc2.Start();
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Running,$timeout)

1. Kohaliku töötamise kataloogi alla laadida [Loomine WSFC kobar alati klõpsake kättesaadavus rühmade Azure VM](http://gallery.technet.microsoft.com/scriptcenter/Create-WSFC-Cluster-for-7c207d3a) **CreateAzureFailoverCluster.ps1** . Kasutate selle skripti aitavad teil luua otstarbekas WSFC kobar. Oluline teave selle kohta, kuidas WSFC suhtleb Azure võrgu, vt [kättesaadavuse ja SQL Server Azure'i Virtuaalmasinates katastroofiabi](virtual-machines-windows-sql-high-availability-dr.md).

1. Oma töö kausta ja luua WSFC kobar allalaaditud skripti.

        Set-ExecutionPolicy Unrestricted -Force
        .\CreateAzureFailoverCluster.ps1 -ClusterName "$clusterName" -ClusterNode "$server1","$server2","$serverQuorum"

1. Luba alati klõpsake kättesaadavus rühmad jaoks vaikimisi SQL Serveri eksemplari **ContosoSQL1** ja **ContosoSQL2**.

        Enable-SqlAlwaysOn `
            -Path SQLSERVER:\SQL\$server1\Default `
            -Force
        Enable-SqlAlwaysOn `
            -Path SQLSERVER:\SQL\$server2\Default `
            -NoServiceRestart
        $svc2.Stop()
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Stopped,$timeout)
        $svc2.Start();
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Running,$timeout)

1. Varukoopia luua ja SQL Serveri teenuse kontode jaoks õiguste andmine. Kasutage seda kausta ettevalmistamiseks teise koopia andmebaasi olemasolu.

        $backup = "C:\backup"
        New-Item $backup -ItemType directory
        net share backup=$backup "/grant:$acct1,FULL" "/grant:$acct2,FULL"
        icacls.exe "$backup" /grant:r ("$acct1" + ":(OI)(CI)F") ("$acct2" + ":(OI)(CI)F")

1. Luua andmebaasi **ContosoSQL1** nimega **MyDB1**, teha nii täieliku varukoopia ja Logi varukoopia ja taastada need **ContosoSQL2** **Koos** NORECOVERY.

        Invoke-SqlCmd -Query "CREATE database $db"
        Backup-SqlDatabase -Database $db -BackupFile "$backupShare\db.bak" -ServerInstance $server1
        Backup-SqlDatabase -Database $db -BackupFile "$backupShare\db.log" -ServerInstance $server1 -BackupAction Log
        Restore-SqlDatabase -Database $db -BackupFile "$backupShare\db.bak" -ServerInstance $server2 -NoRecovery
        Restore-SqlDatabase -Database $db -BackupFile "$backupShare\db.log" -ServerInstance $server2 -RestoreAction Log -NoRecovery

1. Luua kättesaadavus rühma lõpp-punktid SQL serveri VMs ja asjakohased õigused pannud lõpp-punktid.

        $endpoint =
            New-SqlHadrEndpoint MyMirroringEndpoint `
            -Port 5022 `
            -Path "SQLSERVER:\SQL\$server1\Default"
        Set-SqlHadrEndpoint `
            -InputObject $endpoint `
            -State "Started"
        $endpoint =
            New-SqlHadrEndpoint MyMirroringEndpoint `
            -Port 5022 `
            -Path "SQLSERVER:\SQL\$server2\Default"
        Set-SqlHadrEndpoint `
            -InputObject $endpoint `
            -State "Started"

        Invoke-SqlCmd -Query "CREATE LOGIN [$acct2] FROM WINDOWS" -ServerInstance $server1
        Invoke-SqlCmd -Query "GRANT CONNECT ON ENDPOINT::[MyMirroringEndpoint] TO [$acct2]" -ServerInstance $server1
        Invoke-SqlCmd -Query "CREATE LOGIN [$acct1] FROM WINDOWS" -ServerInstance $server2
        Invoke-SqlCmd -Query "GRANT CONNECT ON ENDPOINT::[MyMirroringEndpoint] TO [$acct1]" -ServerInstance $server2

1. Looge kättesaadavus koopiad.

        $primaryReplica =
            New-SqlAvailabilityReplica `
            -Name $server1 `
            -EndpointURL "TCP://$server1.corp.contoso.com:5022" `
            -AvailabilityMode "SynchronousCommit" `
            -FailoverMode "Automatic" `
            -Version 11 `
            -AsTemplate
        $secondaryReplica =
            New-SqlAvailabilityReplica `
            -Name $server2 `
            -EndpointURL "TCP://$server2.corp.contoso.com:5022" `
            -AvailabilityMode "SynchronousCommit" `
            -FailoverMode "Automatic" `
            -Version 11 `
            -AsTemplate

1. Lõpuks kättesaadavus rühma loomine ja liitumine teise koopia kättesaadavaks rühma.

        New-SqlAvailabilityGroup `
            -Name $ag `
            -Path "SQLSERVER:\SQL\$server1\Default" `
            -AvailabilityReplica @($primaryReplica,$secondaryReplica) `
            -Database $db
        Join-SqlAvailabilityGroup `
            -Path "SQLSERVER:\SQL\$server2\Default" `
            -Name $ag
        Add-SqlAvailabilityDatabase `
            -Path "SQLSERVER:\SQL\$server2\Default\AvailabilityGroups\$ag" `
            -Database $db

## <a name="next-steps"></a>Järgmised sammud
Olete nüüd edukalt rakendatud SQL serveri alati klõpsake Azure kättesaadavus rühma loomise abil. Konfigureerimine on kuulajale selle kättesaadavus rühma jaoks, vaadake [konfigureerimine on ILB kuulajale alati klõpsake kättesaadavus rühmade Azure](virtual-machines-windows-classic-ps-sql-int-listener.md).

Muud Azure SQL serveri kasutamise kohta leiate artiklist [SQL Server Azure'i Virtuaalmasinates](virtual-machines-windows-sql-server-iaas-overview.md).
