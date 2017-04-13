<properties
    pageTitle="Symantec Endpoint Protection installimine VM | Microsoft Azure'i"
    description="Saate teada, kuidas installida ja konfigureerida Symantec Endpoint Protection turvalisus laiend uude või olemasolevasse Azure VM loodud klassikaline juurutamise mudeliga."
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

# <a name="how-to-install-and-configure-symantec-endpoint-protection-on-a-windows-vm"></a>Kuidas installida ja konfigureerida Windows VM Symantec Endpoint Protection

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

Selles artiklis kirjeldatakse, kuidas installida ja konfigureerida Symantec Endpoint Protection kliendi arvutisse mõne olemasoleva virtual (VM) opsüsteemi Windows Server. See on täielik klient, mis sisaldab teenuste nagu viiruste, nuhkvara kaitse, tulemüüri ja sissetungimise vältimine. Kliendi on installitud turvalisus jätku VM Agent abil.

Kui teil on olemasolevale tellimusele Symantec kohapealse lahenduse, saate oma Azure'i virtuaalmasinates kaitsta. Kui te ei ole veel klient, saate kasutajaks prooviversiooni tellimuse. See lahendus kohta leiate lisateavet teemast [Symantec Endpoint Protection Microsoft Azure'i platvormil][Symantec]. Sellel lehel on lingid litsentsimise teavet ja juhiseid kliendi installimine, kui olete juba Symantec klient.

## <a name="install-symantec-endpoint-protection-on-an-existing-vm"></a>Mõne olemasoleva VM Symantec Endpoint Protection installimine

Enne alustamist, siis on vaja järgmist.

- Azure'i PowerShelli moodul, versioon 0.8.2 või uuem versioon tööarvutis. Saate kontrollida Azure PowerShelli abil installitud Office'i versioon on **Get-moodul Azure'i | tabelivorming versiooni** käsk. Juhised ja lingi uusim versioon, vaadake, [Kuidas installida ja konfigureerida Azure PowerShelli][PS]. Logige sisse oma Azure tellimuse abil `Add-AzureAccount`.

- VM Agent Azure virtuaalse masina töötab.

Esmalt veenduge, et VM Agent on juba installitud virtual arvutisse. Täitke pilvepõhise teenuse nimi ja virtuaalse masina nimi ja administraatori tasemel Azure PowerShelli käsuviibas käivitage järgmised käsud. Asendage kõik hinnapakkumised, sh selle < ja > märgid.

> [AZURE.TIP] Kui te ei tea pilveteenus ja virtuaalse masina nimed, käivitage **Get-AzureVM** loetleda kõik virtuaalmasinates teie praegune tellimus nimed.

    $CSName = "<cloud service name>"
    $VMName = "<virtual machine name>"
    $vm = Get-AzureVM -ServiceName $CSName -Name $VMName
    write-host $vm.VM.ProvisionGuestAgent

Kui käsk **kirjutamine-host** kuvab **väärtuse True**, VM Agent on installitud. Kui kuvatakse **False**, lugege juhiseid ja Azure ajaveebi allalaadimise link postitada [VM Agent ja laiendid – osa 2][Agent].

Kui VM Agent on installitud, nende käskude Symantec Endpoint Protection agent installimiseks.

    $Agent = Get-AzureVMAvailableExtension -Publisher Symantec -ExtensionName SymantecEndpointProtection

    Set-AzureVMExtension -Publisher Symantec –Version $Agent.Version -ExtensionName SymantecEndpointProtection -VM $vm | Update-AzureVM

Et Symantec turvalisus laiend on installitud ja ajakohasuse kontrollimine

1.  Logige sisse virtuaalse masina. Juhiste saamiseks vaadake, [Kuidas logida virtuaalse masina opsüsteemi Windows Server][Logon].
2.  Windows Server 2008 R2, klõpsake **Alustamine > Symantec Endpoint Protection**. Windows Server 2012 või Windows Server 2012 R2 avakuva, tippige **Symantec**, ja klõpsake nuppu **Symantec Endpoint Protection**.
3.  **Olek – Symantec Endpoint Protection** akna vahekaardil **olek** värskenduste rakendamine või vajaduse korral uuesti.

## <a name="additional-resources"></a>Lisaressursid

[Kuidas töötab Windows Server sisse logida][Logon]

[Azure'i VM laiendid ja -funktsioonid][Ext]


<!--Link references-->
[Symantec]: http://www.symantec.com/connect/blogs/symantec-endpoint-protection-now-microsoft-azure

[Portal]: http://manage.windowsazure.com

[Create]: virtual-machines-windows-classic-tutorial.md

[PS]: ../powershell-install-configure.md

[Agent]: http://go.microsoft.com/fwlink/p/?LinkId=403947

[Logon]: virtual-machines-windows-classic-connect-logon.md

[Ext]: http://go.microsoft.com/fwlink/p/?linkid=390493