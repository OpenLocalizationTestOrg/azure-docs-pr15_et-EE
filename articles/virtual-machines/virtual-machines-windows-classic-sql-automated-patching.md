<properties
    pageTitle="Automaatse lappimine SQL serveri vms (klassikaline) | Microsoft Azure'i"
    description="Funktsiooni automatiseeritud lappimine selgitatakse jaoks SQL serveri Virtuaalmasinates töötab Azure juurutamise klassikalise režiimi kasutamine."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-service-management" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/26/2016"
    ms.author="jroth" />

# <a name="automated-patching-for-sql-server-in-azure-virtual-machines-classic"></a>Automaatse lappimine SQL Server Azure'i virtuaalmasinates (klassikaline)

> [AZURE.SELECTOR]
- [Ressursihaldur](virtual-machines-windows-sql-automated-patching.md)
- [Klassikaline](virtual-machines-windows-classic-sql-automated-patching.md)

Automaatne lappimine luuakse hoolduse jaoks on Azure virtuaalse masina SQL Server töötab. Automatiseeritud värskendusi saab installida ainult akna hoolduse ajal. SQL Server, see tagab süsteemi värskendused ja mis tahes seotud taaskäivitamist tekkida kõige paremini võimalik ajal andmebaasi. Automaatne lappimine sõltub [SQL Server IaaS Agent laiendamine](virtual-machines-windows-classic-sql-server-agent-extension.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Selles artiklis ressursihaldur versiooni, Kuva [Automatiseeritud lappimine SQL Server Azure'i Virtuaalmasinates ressursihaldur](virtual-machines-windows-sql-automated-patching.md).

## <a name="prerequisites"></a>Eeltingimused

Saate kasutada automatiseeritud lappimine, kaaluge järgmine kohustuslik tarkvara.

**Operatsioonisüsteem**:

- Windows Server 2012
- Windows Server 2012 R2

**SQL serveri versioon**:

- SQL Server 2012
- SQL serveri 2014
- SQL Server 2016

**Azure'i PowerShelli**:

- [Installige uusim Azure PowerShelli käske](../powershell-install-configure.md).

**SQL Server IaaS laiend**:

- [Installida SQL Server IaaS laiendamine](virtual-machines-windows-classic-sql-server-agent-extension.md).

## <a name="settings"></a>Sätted

Järgmises tabelis kirjeldatakse saab konfigureerida automaatse lappimine soovitud suvandid. Klassikaline vms, kasutage PowerShelli nende sätete konfigureerimiseks.

|Säte|Võimalikud väärtused|Kirjeldus|
|---|---|---|
|**Automaatse lappimine**|Lubada või keelata (keelatud)|Lubamine või keelamine automatiseeritud lappimine on Azure virtuaalse masina jaoks.|
|**Hoolduse ajakava**|Iga päev, Esmaspäev, Teisipäev, Kolmapäev, Neljapäev, Reede, Laupäev, pühapäev|Ajakava allalaadimine ja installimine Windows, SQL serveri ja Microsoft virtual arvuti.|
|**Hoolduse start tund**|0-24|Kohaliku alguskellaaeg virtuaalse masina värskendada.|
|**Hoolduse akna kestus**|30-180|Minutid, mille lubatud alla laadida ja värskenduste installimise lõpuleviimiseks.|
|**Paikade kategooria**|Oluliste|Kategooria värskendused alla laadima ja installima.|

## <a name="configuration-with-powershell"></a>Konfiguratsiooni PowerShelli abil

Järgmises näites kasutatakse PowerShelli konfigureerimiseks automatiseeritud lappimine mõne olemasoleva SQL serveri VM. Käsu **New-AzureVMSqlServerAutoPatchingConfig** konfigureerib automaatvärskenduste hooldustööd uues aknas.

    $aps = New-AzureVMSqlServerAutoPatchingConfig -Enable -DayOfWeek "Thursday" -MaintenanceWindowStartingHour 11 -MaintenanceWindowDuration 120  -PatchCategory "Important"

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -AutoPatchingSettings $aps | Update-AzureVM

Selle näite põhjal, järgmises tabelis kirjeldatakse kasulikku mõju sihtkohas Azure VM.

|Parameetri|Efekti|
|---|---|
|**DayOfWeek**|Iga Neljapäev installitud plaastrid.|
|**MaintenanceWindowStartingHour**|Alustage uuendused 11:00 am.|
|**MaintenanceWindowsDuration**|Plaastrid peab olema installitud 120 minutit. Algusaja põhjal, nad lõpule viima 1:00 pm.|
|**PatchCategory**|See parameeter on võimalik ainult "Tähtis".|

See võib kuluda mitu minutit installida ja konfigureerida SQL Server IaaS Agent.

Automaatse lappimine keelamiseks käitage sama skript ilma New-AzureVMSqlServerAutoPatchingConfig luba-parameeter. Nagu installiga võib kuluda mitu minutit keelamiseks automatiseeritud lappimine.

## <a name="next-steps"></a>Järgmised sammud

Muude saadaolevate automatiseerimise toimingute kohta leiate teavet teemast [SQL Server IaaS Agent laiendamine](virtual-machines-windows-classic-sql-server-agent-extension.md).

Töötab SQL Server Azure'i VMs kohta lisateabe saamiseks vt [SQL Server Azure'i Virtuaalmasinates ülevaade](virtual-machines-windows-sql-server-iaas-overview.md).
