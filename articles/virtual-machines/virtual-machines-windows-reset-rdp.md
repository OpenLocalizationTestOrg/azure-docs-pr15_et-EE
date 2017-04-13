<properties
    pageTitle="Lähtestage parool või kaugtöölaua konfiguratsiooni Windows VM | Microsoft Azure'i"
    description="Saate teada, kuidas lähtestada konto parooli või kaugtöölaua teenuste VM Windows Azure'i portaalis või Azure PowerShelli abil."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/01/2016"
    ms.author="iainfou"/>

# <a name="how-to-reset-the-remote-desktop-service-or-its-login-password-in-a-windows-vm"></a>Kaugtöölaua teenuste või oma sisselogimise parool Windows VM lähtestamine

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Kui te ei saa Windowsi virtuaalse masina (VM), saate kohaliku administraatori parooli lähtestamine või lähtestage kaugtöölaua teenuse konfigureerimine. Saate kas Azure portaali või VM Accessi laiend Azure PowerShelli parool lähtestada. Kui kasutate PowerShelli, veenduge, et teil on uusim PowerShelli mooduli töö arvutisse installitud ja olete sisse logitud rakendusse Azure tellimuse. Üksikasjalikud juhised, [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md)lugeda.

> [AZURE.TIP] Saate kontrollida PowerShelli abil installitud versioon`Import-Module Azure, AzureRM; Get-Module Azure, AzureRM | Format-Table Name, Version`

## <a name="windows-vms-in-resource-manager-deployment-model"></a>Windowsi VMs ressursihaldur juurutamise mudelis

### <a name="azure-portal"></a>Azure'i portaal
Valige oma VM, klõpsates nuppu **Sirvi** > **virtuaalmasinates** > *arvuti Windows virtual* > **Kõik sätted** > **parooli lähtestamine**. Parooli lähtestamine tera kuvatakse:

![Lehele parooli lähtestamine](./media/virtual-machines-windows-reset-rdp/Portal-RM-PW-Reset-Windows.png)

Sisestage kasutajanimi ja uus parool ja seejärel klõpsake nuppu **Salvesta**. Proovige uuesti oma VM ühendada.

### <a name="vmaccess-extension-and-powershell"></a>PowerShelli ja VMAccess laiend

Veenduge, et Azure'i PowerShelli 1.0 või uuem versioon on installitud ja olete sisse loginud oma konto abil soovitud `Login-AzureRmAccount` cmdlet-käsk.

#### <a name="reset-the-local-administrator-account-password"></a>**Kohaliku administraatori konto parooli lähtestamine**

[Set-AzureRmVMAccessExtension](https://msdn.microsoft.com/library/mt619447.aspx) PowerShelli käsu abil saate lähtestada administraatori parooli või kasutaja nimi.

Teie kohaliku administraatori konto identimisteave abil luua järgmine käsk:

    $cred=Get-Credential

Kui tipite mõni muu nimi, kui praegune konto, VMAccess laiend järgmine käsk nimetab kohaliku administraatorikonto, määrab selle konto parool ja probleemid kaugtöölaua väljalogimisel sündmus. Kui kohaliku administraatorikonto on keelatud, VMAccess laiend selle lubab.

VM Accessi laiend abil saate määrata uue mandaadi järgmiselt:

    Set-AzureRmVMAccessExtension -ResourceGroupName "myRG" -VMName "myVM" -Name "myVMAccess" `
        -Location WestUS -UserName $cred.GetNetworkCredential().Username `
        -Password $cred.GetNetworkCredential().Password -typeHandlerVersion "2.0"


Asendage `myRG`, `myVM`, `myVMAccess`, ja asukoht oluline häälestust väärtustega.


#### <a name="reset-the-remote-desktop-service-configuration"></a>**Kaugtöölaua teenuse konfiguratsiooni lähtestamine**

Järgmine [Set-AzureRmVMExtension](https://msdn.microsoft.com/library/mt603745.aspx) või [Set-AzureRmVMAccessExtension](https://msdn.microsoft.com/library/mt619447.aspx), abil saate lähtestada oma VM Kaugpöördusteenuse. (Asendada selle `myRG`, `myVM`, `myVMAccess` ja asukoht oma väärtustega.)

    Set-AzureRmVMExtension -ResourceGroupName "myRG" -VMName "myVM" `
        -Name "myVMAccess" -ExtensionType "VMAccessAgent" -Location WestUS `
        -Publisher "Microsoft.Compute" -typeHandlerVersion "2.0"

Või:<br>

    Set-AzureRmVMAccessExtension -ResourceGroupName "myRG" -VMName "myVM" `
        -Name "myVMAccess" -Location WestUS -typeHandlerVersion "2.0


> [AZURE.TIP] Mõlemad käsud lisada uue nimega VM Accessi agendi virtuaalse masina. Mis tahes hetkel VM võib olla ainult üks VM Accessi esindaja. Juurdepääs VM agent atribuutide määramiseks edukalt eemaldamine Accessi agent seadmine varem, kasutades ühte `Remove-AzureRmVMAccessExtension` või `Remove-AzureRmVMExtension`. Alates Azure PowerShelli versiooni 1.2.2, saate vältida seda toimingut kasutades `Set-AzureRmVMExtension` koos mõne `-ForceRerun` suvand. Kui kasutate `-ForceRerun`, veenduge, et kasutada sama nime VM Accessi agendi jaoks vastavalt eelmise käsu.

Kui te ikka ei saa eemalt virtuaalne arvuti, lugege teemat [tõrkeotsing kaugtöölaua ühendusi Windowsi-põhiste Azure virtuaalse masina](virtual-machines-windows-troubleshoot-rdp-connection.md)proovida järgmist juhist.


## <a name="windows-vms-in-the-classic-deployment-model"></a>Windowsi VMs mudelis klassikaline juurutamine

### <a name="azure-portal"></a>Azure'i portaal

Klassikaline juurutamise mudeli abil loodud virtuaalmasinates, saate [Azure portaali](https://portal.azure.com) kaugtöölaua teenuse lähtestada. Klõpsake: **liikuge** > **virtuaalmasinates (klassikaline)** > *arvuti Windows virtual* > **Lähtestamine Remote …**. Kuvatakse järgmine leht.

![Suvand Lähtesta lehe RDP konfigureerimine](./media/virtual-machines-windows-reset-rdp/Portal-RDP-Reset-Windows.png)

Samuti võite nime ja kohaliku administraatorikonto parooli lähtestamise. Klõpsake nuppu: **liikuge** > **virtuaalmasinates (klassikaline)** > *arvuti Windows virtual* > **Kõik sätted** > **parooli lähtestamine**. Kuvatakse järgmine leht.

![Lehele parooli lähtestamine](./media/virtual-machines-windows-reset-rdp/Portal-PW-Reset-Windows.png)

Kui olete sisestanud uue kasutajanime ja parooli, klõpsake nuppu **Salvesta**.

### <a name="vmaccess-extension-and-powershell"></a>PowerShelli ja VMAccess laiend

Veenduge, et VM Agent virtual arvutisse installitud. VMAccess laiend ei pea olema installitud enne seda, saate kasutada, kui VM on saadaval. Veenduge, et VM Agent on juba installitud järgmise käsu abil. (Asendamiseks "myCloudService" ja "myVM" nimesid oma pilveteenus ja teie VM vastavalt. Saate teada nende nimed, käitades `Get-AzureVM` ilma parameetrid.)

    $vm = Get-AzureVM -ServiceName "myCloudService" -Name "myVM"
    write-host $vm.VM.ProvisionGuestAgent

Kui käsk **kirjutamine-host** kuvab **väärtuse True**, VM Agent on installitud. Kui kuvatakse **False**, lugege juhiseid ja [VM Agent ja laiendid – osa 2](http://go.microsoft.com/fwlink/p/?linkid=403947&clcid=0x409) Azure ajaveebipostitus allalaadimise link.

Kui olete loonud virtuaalse masina abil portaali, kontrollige kas `$vm.GetInstance().ProvisionGuestAgent` tagastab **väärtuse True**. Kui ei, saate selle käsu abil:

    $vm.GetInstance().ProvisionGuestAgent = $true

See käsk takistab järgmine tõrketeade, kui kasutate käsu **Set-AzureVMExtension** järgmised toimingud: "Sätte külaline Agent peab olema lubatud VM objektil, enne kui seate IaaS VM Accessi laiend."

#### <a name="reset-the-local-administrator-account-password"></a>**Kohaliku administraatori konto parooli lähtestamine**

Luua sisselogimise identimisteavet praeguse kohaliku administraatori konto nimi ja uus parool ja seejärel käivitage selle `Set-AzureVMAccessExtension` järgmiselt.

    $cred=Get-Credential
    Set-AzureVMAccessExtension –vm $vm -UserName $cred.GetNetworkCredential().Username `
        -Password $cred.GetNetworkCredential().Password  | Update-AzureVM

Kui tipite mõni muu nimi, kui praegune konto, VMAccess laiend nimetab kohaliku administraatorikonto, määrab selle konto parool ja väljunud kaugtöölaua probleemid. Kui kohaliku administraatorikonto on keelatud, VMAccess laiend selle lubab.

Need käsud lähtestada kaugtöölaua teenuse konfigureerimine.

#### <a name="reset-the-remote-desktop-service-configuration"></a>**Kaugtöölaua teenuse konfiguratsiooni lähtestamine**

Kaugtöölaua teenuse konfigureerimine lähtestamiseks käivitage järgmine käsk:

    Set-AzureVMAccessExtension –vm $vm | Update-AzureVM

VMAccess laiend töötab virtuaalse masina kaks käsud:

- `netsh advfirewall firewall set rule group="Remote Desktop" new enable=Yes`

See käsk võimaldab sisseehitatud Windowsi tulemüüri rühma, mis võimaldab kaugtöölaua liiklust, mis kasutab TCP porti 3389.

- `Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server' -name "fDenyTSConnections" -Value 0`

See käsk seab selle fDenyTSConnections registriväärtuse 0, kaugtöölaua lubamine.


## <a name="next-steps"></a>Järgmised sammud

Kui Azure VM Accessi laiend ei vasta ja te ei saa parooli lähtestada, saate [kohaliku Windowsi ühenduseta parooli lähtestamine](virtual-machines-windows-reset-local-password-without-agent.md). See meetod on täpsemate protsess ning nõuab teilt kettaruumi probleemne VM ühenduse loomine mõne muu VM. Tehke esmalt artiklit dokumenteerida ja proovida vaid viimasena meetodit ühenduseta parooli lähtestamine.

[Azure'i VM laiendid ja -funktsioonid](virtual-machines-windows-extensions-features.md)

[Ühenduse loomine mõne Azure virtuaalse masina RDP või SSH](http://msdn.microsoft.com/library/azure/dn535788.aspx)

[Windowsi-põhiste Azure virtuaalse masina kaugtöölaua ühenduse tõrkeotsing](virtual-machines-windows-troubleshoot-rdp-connection.md)
