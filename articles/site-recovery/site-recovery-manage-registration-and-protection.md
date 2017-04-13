<properties
    pageTitle="Eemalda serverid ja keelake kaitse | Microsoft Azure'i" 
    description="Selles artiklis kirjeldatakse, kuidas saidi taastamise hoidla serverite unregister ja keelamiseks kaitse virtuaalmasinates ja füüsilise servereid." 
    services="site-recovery" 
    documentationCenter="" 
    authors="rayne-wiselman" 
    manager="jwhit" 
    editor=""/>

<tags 
    ms.service="site-recovery" 
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery" 
    ms.date="10/05/2016" 
    ms.author="raynew"/>

# <a name="remove-servers-and-disable-protection"></a>Eemaldage serverid ja kaitse keelamine

Azure'i saidi taastamise teenuse aitab teie ettevõtte järjepidevus ja avariijärgne taastamine (BCDR) strateegia orkesteroinnin dispersioonanalüüs, Tõrkesiirde ja virtuaalmasinates ja füüsilise serveri. Masinad saab paljundada Azure'i või sekundaarne asutusesisese andmekeskuse. Kiire ülevaate saamiseks lugege [mis on Azure saidi taastamise?](site-recovery-overview.md)

## <a name="overview"></a>Ülevaade

Selles artiklis kirjeldatakse kuidas unregister serverid hoidlast saidi taastamine ja kaitse kaitstud saidi taastamine virtuaalmasinates keelamine. 

Postitada kommentaare või küsimusi allosas käesoleva artikli või [Azure taastamise teenused Foorum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="unregister-a-vmm-server"></a>VMM serveri registreerimise tühistamine

Saate tühistada VMM server on hoidlast kustutades server Azure'i saidi taastamine portaalis vahekaardil **serverid** . Pange tähele järgmist.

-  **Ühendatud VMM server**: soovitame teil unregister VMM server, kui see on ühendatud Azure. See tagab, et kohapealne VMM serveris ja sellega seotud (VMM serverid, mis sisaldavad pilved, mis on vastendatud parkimise server, mida soovite kustutada) VMM serverid sätted on õigesti puhastada. Soovitame teil ainult eemaldada ühendatud serveriks kui on püsivate probleeme Ühenduvus.
- **Ühendatud VMM server**: kui VMM server pole ühendatud kustutamisel peate käivitada käsitsi käivitamiseks. Skripti on saadaval [Microsoft Galerii](http://aka.ms/asr-cleanup-script-vmm). Pange tähele VMM ID serveri käsitsi puhastus protsessi lõpuleviimiseks.
- **Kobar VMM server**: kui teil on vaja unregister VMM server, mis on juurutatud klaster tehke järgmist:

    - Kui server on ühendatud, kustutage ühendatud VMM server vahekaardil **serverid** . Pakkuja serveri desinstallimiseks iga kobar sõlm logida ja eemaldage see juhtpaneeli kaudu. Käivitage Kettapuhastus skript viidatud eelmises jaotises kõik passiivne sõlmed klaster registrikirjed kustutada.
    - Kui server pole ühendatud peate kõik kobar sõlmed Kettapuhastus skripti käivitada.

### <a name="unregister-an-unconnected-vmm-server"></a>On ühendatud VMM serveri registreerimise tühistamine

VMM-i server, mille soovite eemaldada:

1. Unregister VMM server Azure'i portaalis.
2. VMM-i serverisse laadida Kettapuhastus skripti.
3. Avage PowerShelli Käivita, et täitmise poliitikas vaikimisi (LocalMachine) ulatuse muutmine suvandiks administraator.
4. Järgige skripti. 

VMM serverid, mis on pilved, mis on ühendatud parkimise olete eemaldamise server:

1. Käivitage Kettapuhastus skript ja järgige juhiseid 2 – 4.
2. Määrake VMM VMM Server, mis on registreerimata ID. 
3. See skript eemaldab VMM Server ja pilveteenuse sidumine teabe andmed.


## <a name="unregister-a-hyper-v-server-in-a-hyper-v-site"></a>Unregister Hyper-V server Hyper-V saidil

Azure saidi taastamise juurutamisel asub Hyper-V Server Hyper-V saidi (pole VMM server) koos virtuaalmasinates kaitsmiseks saate unregister Hyper-V server on hoidlast järgmiselt:

1. Keelake kaitse virtuaalmasinates Hyper-V serveris.
2. Valige vahekaardil **serverid** Azure saidi taastamise portaalis server > Kustuta. Server ei pea olema ühendatud Azure, kui te seda teete.
3. Käivitage järgmine skript serveri sätted puhastamiseks ja unregister seda funktsiooni hoidlast. 

        pushd .
        try
        {
             $windowsIdentity=[System.Security.Principal.WindowsIdentity]::GetCurrent()
             $principal=new-object System.Security.Principal.WindowsPrincipal($windowsIdentity)
             $administrators=[System.Security.Principal.WindowsBuiltInRole]::Administrator
             $isAdmin=$principal.IsInRole($administrators)
             if (!$isAdmin)
             {
                "Please run the script as an administrator in elevated mode."
                $choice = Read-Host
                return;       
             }
    
            $error.Clear()    
            "This script will remove the old Azure Site Recovery Provider related properties. Do you want to continue (Y/N) ?"
            $choice =  Read-Host
        
            if (!($choice -eq 'Y' -or $choice -eq 'y'))
            {
            "Stopping cleanup."
            return;
            }
        
            $serviceName = "dra"
            $service = Get-Service -Name $serviceName
            if ($service.Status -eq "Running")
            {
                "Stopping the Azure Site Recovery service..."
                net stop $serviceName
            }
        
            $asrHivePath = "HKLM:\SOFTWARE\Microsoft\Azure Site Recovery"
            $registrationPath = $asrHivePath + '\Registration'
            $proxySettingsPath = $asrHivePath + '\ProxySettings'
            $draIdvalue = 'DraID'
        
            if (Test-Path $asrHivePath)
            {
                if (Test-Path $registrationPath)
                {
                    "Removing registration related registry keys."  
                    Remove-Item -Recurse -Path $registrationPath
                }
    
                if (Test-Path $proxySettingsPath)
            {
                    "Removing proxy settings"
                    Remove-Item -Recurse -Path $proxySettingsPath
                }
    
                $regNode = Get-ItemProperty -Path $asrHivePath
                if($regNode.DraID -ne $null)
                {            
                    "Removing DraId"
                    Remove-ItemProperty -Path $asrHivePath -Name $draIdValue
                }
                "Registry keys removed."
            }
    
            # First retrive all the certificates to be deleted
            $ASRcerts = Get-ChildItem -Path cert:\localmachine\my | where-object {$_.friendlyname.startswith('ASR_SRSAUTH_CERT_KEY_CONTAINER') -or $_.friendlyname.startswith('ASR_HYPER_V_HOST_CERT_KEY_CONTAINER')}
            # Open a cert store object
            $store = New-Object System.Security.Cryptography.X509Certificates.X509Store("My","LocalMachine")
            $store.Open('ReadWrite')
            # Delete the certs
            "Removing all related certificates"
            foreach ($cert in $ASRcerts)
            {
                $store.Remove($cert)
            }
        }catch
        {   
            [system.exception]
            Write-Host "Error occured" -ForegroundColor "Red"
            $error[0] 
            Write-Host "FAILED" -ForegroundColor "Red"
        }
        popd


## <a name="stop-protecting-a-hyper-v-virtual-machine"></a>Kaitsmine Hyper-V virtuaalse masina peatamine

Kui soovite peatada kaitsmine Hyper-V virtuaalse masina peate seda kaitse eemaldada. Sõltuvalt sellest, kuidas kaitse eemaldamiseks peate puhastamiseks kaitse sätete käsitsi arvutis. 

### <a name="remove-protection"></a>Kaitse eemaldamine

1. Pilveteenuse atribuudid vahekaarti **Virtuaalmasinates** , valige virtuaalse masina > **Eemalda**.
2. **Kinnitage eemaldamine virtuaalse masina** lehel on teil mitu võimalust.

    - **Keelake kaitse**– kui lubate ja salvestage see suvand saidi taastamine enam kaitstakse virtuaalse masina. Virtuaalse masina kaitse sätted kuvatakse puhastada automaatselt.
    - **Eemalda hoidlast soovitud**– kui valite selle suvandi, virtuaalse masina eemaldatakse ainult saidi taastamise hoidla. Kohapealse kaitse sätete virtuaalse masina ei mõjuta. Peate puhastamiseks sätteid käsitsi eemaldamine sätted ja virtuaalse masina eemaldamine tellimusest, Azure eemaldamiseks peate need puhastamiseks, käsitsi abil alltoodud juhiseid sätted.

Kui valite kustutada virtuaalse masina ja selle kõvaketta eemaldatakse sihtkoht.

### <a name="clean-up-protection-settings-manually-between-vmm-sites"></a>Puhasta sätted käsitsi (vahel VMM saidid)

Kui valisite **Peata haldamine virtuaalse masina**, käsitsi puhastamiseks sätted:

1. Esmane serveris käivitage see skript VMM konsooli puhastamiseks esmane virtuaalse masina sätteid. VMM konsooli nuppu PowerShelli avamiseks VMM PowerShelli konsooli. Asendage SQLVM1 oma virtuaalse masina nimi.

         $vm = get-scvirtualmachine -Name "SQLVM1"
         Set-SCVirtualMachine -VM $vm -ClearDRProtection

2. Teisene VMM Server käivitage see skript puhastamiseks teisene virtuaalse masina sätted:

        $vm = get-scvirtualmachine -Name "SQLVM1"
        Remove-SCVirtualMachine -VM $vm -Force

3. Teisene VMM Server, kui skripti värskendada virtuaalmasinates, Hyper-V hosti server nii, et teisene virtuaalse masina saab uuesti tuvastatud VMM konsooli.
4. Eespool kirjeldatud toimingud tühjendab dispersioonanalüüs sätted ainult VMM server. Kui soovite eemaldada virtuaalse masina dispersioonanalüüs virtuaalse masina jaoks. Peate tegema alltoodud juhiseid nii primaarsete ja teisese virtuaalmasinates. Käivitage selle all skripti dispersioonanalüüs eemaldada ja asendada SQLVM1 oma virtuaalse masina nimi.

        Remove-VMReplication –VMName “SQLVM1”


### <a name="clean-up-protection-settings-manually-between-on-premises-vmm-sites-and-azure"></a>Puhasta sätted käsitsi (vahel kohapealse VMM saidid ja Azure)

1. VMM-i serverisse käivitage see skript puhastamiseks esmane virtuaalse masina sätted:

        $vm = get-scvirtualmachine -Name "SQLVM1"
        Set-SCVirtualMachine -VM $vm -ClearDRProtection

2. Eespool kirjeldatud toimingud tühjendab dispersioonanalüüs sätted ainult VMM server. Kui teil on eemaldatud dispersioonanalüüs VMM serverist tagada dispersioonanalüüs virtual masina selle skripti serveris Hyper-V host eemaldada. Asendage SQLVM1 nimi oma virtuaalse masina ja host01.contoso.com Hyper-V server hosti nimi.

        $vmName = "SQLVM1"
        $hostName  = "host01.contoso.com"
        $vm = Get-WmiObject -Namespace "root\virtualization\v2" -Query "Select * From Msvm_ComputerSystem Where ElementName = '$vmName'" -computername $hostName
        $replicationService = Get-WmiObject -Namespace "root\virtualization\v2"  -Query "Select * From Msvm_ReplicationService"  -computername $hostName
        $replicationService.RemoveReplicationRelationship($vm.__PATH)

### <a name="clean-up-protection-settings-manually-between-hyper-v-sites-and-azure"></a>Puhasta sätted käsitsi (vahel Hyper-V saidid ja Azure)

1. Allikas Hyper-V hosti server eemaldamiseks dispersioonanalüüs virtual masina seda skripti kasutada. Asendage SQLVM1 oma virtuaalse masina nimi.

        $vmName = "SQLVM1"
        $vm = Get-WmiObject -Namespace "root\virtualization\v2" -Query "Select * From Msvm_ComputerSystem Where ElementName = '$vmName'"
        $replicationService = Get-WmiObject -Namespace "root\virtualization\v2"  -Query "Select * From Msvm_ReplicationService"
        $replicationService.RemoveReplicationRelationship($vm.__PATH)

## <a name="stop-protecting-a-vmware-virtual-machine-or-a-physical-server"></a>Kaitsmine VMware virtuaalse masina või füüsilise serveri peatamine

Kui soovite peatada kaitsmine VMware virtuaalse masina või füüsilise serveri peate seda kaitse eemaldada. Sõltuvalt sellest, kuidas kaitse eemaldamiseks peate puhastamiseks kaitse sätete käsitsi arvutis. 

### <a name="remove-protection"></a>Kaitse eemaldamine

1. Pilveteenuse atribuudid vahekaarti **Virtuaalmasinates** , valige virtuaalse masina > **Eemalda**.
2. **Virtuaalse masina eemaldamiseks** valige üks suvanditest:

    - **Keela kaitse (kasutamine taastamine süvitsi ja mahu suuruse muutmise)**– kuvatakse ainult vaadata ja Lubage see suvand, kui olete:
        - **Resized virtuaalse masina maht**– suuruse muutmisel virtuaalse masina on tõrkeolukorras kriitiline maht. Sellisel juhul valige see suvand. See keelab kaitse säilitades Azure taastamise punkte. Kui te lubage uuesti kaitse masina andmeid muudetud suurusega maht ei viida Azure.
        - **Käivitage Tõrkesiirde**– kui olete testinud keskkonna töötab Tõrkesiirde kohapealse VMware virtuaalmasinates või füüsilise serveri Azure, valige see suvand alustada kaitsta oma kohapealse virtuaalmasinates uuesti. See suvand keelab iga virtuaalse masina ja seejärel peate saidikataloogi kaitse neid. Pange tähele järgmist.
            - See säte on virtuaalse masina keelamine ei mõjuta koopia virtuaalse masina Azure.
            - Te ei tohi mobiilsus teenuse virtuaalse masina desinstallida.
    
    - **Keelake kaitse**– kui lubate ja salvestage see suvand seade on kaitstud enam saidi taastamine. Seadme sätted kuvatakse puhastada automaatselt.
    - **Eemalda hoidlast soovitud**– kui valite selle suvandi masina eemaldatakse ainult saidi taastamise hoidla. Kohapealse masina sätted ei mõjuta. Eemaldada sätted arvutis ja selle Azure virtuaalse masina eemaldamine tellimus ja te peate sätete puhastamiseks mobiilsus teenuse desinstallimine.
    
        ![Eemaldage suvandid](./media/site-recovery-manage-registration-and-protection/remove-vm.png)

















