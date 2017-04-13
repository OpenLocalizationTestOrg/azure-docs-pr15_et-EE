<properties
    pageTitle="Trend Micro sügav turvalisus installimine VM | Microsoft Azure'i"
    description="Selles artiklis kirjeldatakse, kuidas installida ja konfigureerida Trend Micro turvalisus loodud klassikaline juurutamise mudeli Azure VM."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/24/2016"
    ms.author="iainfou"/>


# <a name="how-to-install-and-configure-trend-micro-deep-security-as-a-service-on-a-windows-vm"></a>Kuidas installida ja konfigureerida teenust Windows VM Trend Micro sügav turvalisus

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

Selles artiklis kirjeldatakse, kuidas installida ja konfigureerida Trend Micro sügav turvalisus uude või olemasolevasse virtual arvutisse (VM) Windows Server töötab teenus. Sügav turvalisuse teenuse sisaldab ründevaratõrje, tulemüüri, intrusion vältimise süsteemi ja terviklus jälgimine.

Kliendi on installitud turvalisus jätku VM esindaja kaudu. Uue virtuaalse masina, installite VM Agent koos sügav turvalisus Agent. Olemasoleva virtual-arvutisse, mis ei sisalda VM Agent, peate esmalt alla laadima ja installima esmalt. Selles artiklis käsitletakse nii olukordades.

Kui teil on olemasolev Trend Micro kohapealse lahenduse tellimus, saate oma Azure'i virtuaalmasinates kaitsta. Kui te ei ole veel klient, saate kasutajaks proovitellimuse. See lahendus kohta leiate lisateavet teemast Trend Micro ajaveebipostitusest [Microsoft Azure'i VM Agent laiend jaoks sügav turvalisus](http://go.microsoft.com/fwlink/p/?LinkId=403945).

## <a name="install-the-deep-security-agent-on-a-new-vm"></a>Uue VM sügav turvalisus Agent installimine

[Azure'i klassikaline portaalis](http://manage.windowsazure.com) saate installida VM Agent ja Trend Micro turvalisus laiend, kui **Galeriist** suvandi abil saate luua virtuaalse masina. Kui loote ühe virtuaalse masina, portaalis on lihtne viis lisada kaitse Trend Micro.

Suvand **Galeriist** avatakse viisard, mis aitab teil määrata ka virtual. Viisardi viimasel lehel saate installida VM Agent ja Trend Micro turvalisus laiendamine. Üldised juhised leiate teemast [loomine virtuaalse masina opsüsteemi Windows Azure'i klassikaline portaalis](virtual-machines-windows-classic-tutorial.md). Kui saate viimase viisardi lehel, tehke järgmist.

1.  Märkige jaotises **VM Agent**, **Installige VM Agent**.

2.  Märkige jaotises **Turve laiendid**, **Trend Micro sügav turvalisus Agent**.

    ![Installige VM Agent ja sügav turvalisus Agent](./media/virtual-machines-windows-classic-install-trend/InstallVMAgentandTrend.png)

3.  Klõpsake märkeruutu virtuaalse masina loomiseks.

## <a name="install-the-deep-security-agent-on-an-existing-vm"></a>Mõne olemasoleva VM sügav turvalisus Agent installimine

Mõne olemasoleva VM agent installimiseks vajate järgmist:

- Azure'i PowerShelli moodul, versioon 0.8.2 või uuemat versiooni, kohalikku arvutisse installitud. Saate kontrollida Azure PowerShelli abil installitud Office'i versioon on **Get-moodul Azure'i | tabelivorming versiooni** käsk. Juhised ja lingi uusim versioon, vaadake, [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md). Logige sisse oma Azure tellimuse abil `Add-AzureAccount`.

- Virtuaalne sihtarvutis installitud VM Agent.

Esmalt veenduge, et VM Agent on juba installitud. Täitke pilvepõhise teenuse nimi ja virtuaalse masina nimi ja administraatori tasemel Azure PowerShelli käsuviibas käivitage järgmised käsud. Asendage kõik hinnapakkumised, sh selle < ja > märgid.

    $CSName = "<cloud service name>"
    $VMName = "<virtual machine name>"
    $vm = Get-AzureVM -ServiceName $CSName -Name $VMName
    write-host $vm.VM.ProvisionGuestAgent

Kui te ei tea, pilveteenus ja virtuaalse masina nimi, käivitage **Get-AzureVM** oma praeguse tellimuse kõik virtuaalmasinates selle teabe kuvamiseks.

Kui **kirjutamine-host** käsk tagastab väärtuse **True**, VM Agent on installitud. Kui tagastab väärtuse **False**, lugege juhiseid ja Azure ajaveebi allalaadimise link postitada [VM Agent ja laiendid - osa 2](http://go.microsoft.com/fwlink/p/?LinkId=403947).

Kui VM Agent on installitud, käivitage järgmised käsud.

    $Agent = Get-AzureVMAvailableExtension TrendMicro.DeepSecurity -ExtensionName TrendMicroDSA

    Set-AzureVMExtension -Publisher TrendMicro.DeepSecurity –Version $Agent.Version -ExtensionName TrendMicroDSA -VM $vm | Update-AzureVM

## <a name="next-steps"></a>Järgmised sammud

Kulub paar minutit agent käivitamiseks, kui see on installitud. Pärast seda, peate aktiveerima sügav turvalisus virtual arvutisse nii, et seda saab hallata, sügav turvalisus ülemuse. Vaadake täiendavate juhiste saamiseks järgmist:

- See lahendus [Instant-On Cloud Security Microsoft Azure](http://go.microsoft.com/fwlink/?LinkId=404101) trendi 's artiklis
- [Windows PowerShelli skripti näide](http://go.microsoft.com/fwlink/?LinkId=404100) virtuaalse masina konfigureerimine
- Valimi [juhised](http://go.microsoft.com/fwlink/?LinkId=404099)

## <a name="additional-resources"></a>Lisaressursid

[Kuidas töötab Windows Server sisse logida]

[Azure'i VM laiendid ja -funktsioonid]


<!--Link references-->
[Kuidas töötab Windows Server sisse logida]: virtual-machines-windows-classic-connect-logon.md
[Azure'i VM laiendid ja -funktsioonid]: http://go.microsoft.com/fwlink/p/?linkid=390493&clcid=0x409
